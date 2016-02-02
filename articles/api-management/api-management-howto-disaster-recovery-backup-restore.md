<properties 
    pageTitle="Azure API Management でサービスのバックアップと復元を使用して障害復旧を実装する方法" 
    description="Azure API Management でバックアップと復元を使用して障害復旧を行う方法について説明します。" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/07/2015" 
    ms.author="sdanie"/>


# Azure API Management でサービスのバックアップと復元を使用して障害復旧を実装する方法

Azure API Management を介して API の発行と管理を行うように選択することで、そうしなければ自分で設計、実装、および管理する必要のある、さまざまなフォールト トレランス機能やインフラストラクチャ機能を利用できるようになります。 Azure プラットフォームにより、わずかな料金で潜在的な障害の大部分が軽減されます。

API Management サービスがホストされているリージョンに影響する可用性の問題から復旧するためには、いつでも異なるリージョンにサービスを再構成できるように準備しておく必要があります。 可用性の目標と目標復旧時間に応じて、バックアップ サービスを 1 つまたは複数のリージョンを予約し、それらの構成と内容がアクティブ サービスとの同期を維持しようとすることができます。 サービス バックアップと復元の機能は、障害復旧戦略を実装するために必要な構成要素となります。

このガイドでは、Azure リソース マネージャーの要求を認証する方法と、API Management サービス インスタンスをバックアップおよび復元する方法を説明します。
>[AZURE.NOTE] 障害復旧用に API Management サービス インスタンスをバックアップおよび復元するプロセスは、ステージングなどのシナリオ用に API Management サービス インスタンスをレプリケートするためにも使用できます。

## Azure リソース マネージャーの要求の認証

>[AZURE.IMPORTANT] バックアップと復元用の REST API では、Azure リソース マネージャーが使用されます。API Management のエンティティ管理には REST API 以外の認証メカニズムも用意されています。 このセクションの手順では、Azure リソース マネージャーの要求を認証する方法について説明します。 詳細については、次を参照してください。 [Azure Resource Manager 要求](http://msdn.microsoft.com/library/azure/dn790557.aspx)します。

Azure リソース マネージャーを使用してリソースに実行するすべてのタスクは、Azure Active Directory で以下の手順に従って認証する必要があります。

-   Azure Active Directory テナントにアプリケーションを追加する。
-   追加したアプリケーションのアクセス許可を設定する。
-   Azure リソース マネージャーへの要求を認証するためのトークンを取得する。

最初の手順は、Azure Active Directory アプリケーションの作成です。 ログイン、 [Azure Classic Portal](http://manage.windowsazure.com/) インスタンスおよびに移動し、API Management サービスを含むサブスクリプションを使用して、 **アプリケーション** 、既定の Azure Active Directory] タブをクリックします。
>[AZURE.NOTE] Azure Active Directory の既定のディレクトリがアカウントに表示されない場合は、必要なアクセス許可をアカウントに付与するよう Azure サブスクリプションの管理者に連絡してください。 既定のディレクトリを見つける方法の詳細については、次を参照してください。 [既定のディレクトリを見つける](../virtual-machines/resource-group-create-work-id-from-persona.md/#locate-your-default-directory-in-the-azure-portal)します。

![Azure Active Directory アプリケーションを作成する][api-management-add-aad-application]

**[追加]**、**[組織で開発中のアプリケーションを追加]** を順にクリックし、**[ネイティブ クライアント アプリケーション]** を選択します。 わかりやすい名前を入力し、次へ進む矢印をクリックします。 プレース ホルダー URL を入力します。 `http://resources` の、 **リダイレクト URI**, これは必須フィールドですが、値は後で使用されません。 チェック ボックスをオンにして、アプリケーションを保存します。

アプリケーションが保存されたら、**[構成]** をクリックし、ページを **[他のアプリケーションに対するアクセス許可]** セクションが表示されるまで下へスクロールして、**[アプリケーションの追加]** をクリックします。

![Add permissions][api-management-aad-permissions-add]

**[Windows** **Azure Service Management API]** を選択し、チェック ボックスをオンにしてアプリケーションを追加します。

![Add permissions][api-management-aad-permissions]

新たに追加された **Windows** **Azure Service Management API** アプリケーションの横にある **[デリゲートされたアクセス許可]** をクリックし、**[Azure Service 管理へのアクセス (プレビュー)]** のチェック ボックスをオンにして、**[保存]** をクリックします。

![Add permissions][api-management-aad-delegated-permissions]

サービス インスタンスをバックアップおよび復元する API を呼び出す前に、トークンを取得する必要があります。 次の例では、 [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget パッケージをトークンを取得します。

    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace GetTokenResourceManagerRequests
    {
        class Program
        {
            static void Main(string[] args)
            {
                var authenticationContext = new AuthenticationContext("https://login.windows.net/{tenant id}");
                var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});
    
                if (result == null) {
                    throw new InvalidOperationException("Failed to obtain the JWT token");
                }
    
                Console.WriteLine(result.AccessToken);
    
                Console.ReadLine();
            }
        }
    }

置換 `tentand {id}`, 、`{application id}`, と `{redirect uri}` 、次の手順を使用します。

置換 `{テナント id}` 作成した Azure Active Directory アプリケーションのテナント id にします。 ID にアクセスするには、**[エンドポイントの表示]** をクリックします。

![Endpoints][api-management-aad-default-directory]

![Endpoints][api-management-endpoint]

置換 `{application id}` と `{redirect uri}` を使用して、 **クライアント Id** と URL、 **Redirect Uris** セクションから、Azure Active Directory アプリケーションの **構成** ] タブをクリックします。

![リソース][api-management-aad-resources]

値を指定したら、次の例のようなコードによってトークンが返されます。

![トークン][api-management-arm-token]

以降のセクションで説明されているバックアップおよび復元の操作を呼び出す前に、REST 呼び出しに承認要求ヘッダーを設定します。

    request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);

## <a name="step1"> </a>API Management サービスのバックアップ

API Management サービスをバックアップするには、次の HTTP 要求を発行します。

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

各値の説明:

* `subscriptionId` -API Management サービスを含むサブスクリプションの id をバックアップする
* `resourceGroupName` -'Api の既定値は {サービス地域}' の形式で文字列を `サービス地域` 、API Management サービスの Azure リージョンにしようとしているバックアップがホストされているなど `North Central US`
* `serviceName` -API Management サービスの名前加えたのバックアップの作成時に指定
* `api バージョン` -置き換えます `2014年-02-14`

要求の本文に、ターゲットの Azure ストレージ アカウント名、アクセス キー、BLOB コンテナー名、バックアップ名を指定します。

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

値を設定、 `Content-type` 要求ヘッダーを `-application/json`します。

バックアップは完了まで何分もかかる長時間の処理です。 要求が成功したかどうかと、バックアップ処理が開始されるお届け、 `202 Accepted` 応答ステータス コードを `場所` ヘッダー。 "Get"要求の URL を `場所` ヘッダーを操作の状態を確認します。 バックアップの進行中は、「202 Accepted」状態コードの受け取りが続きます。 応答コードの `200 OK` バックアップ操作が正常に完了が示されます。

**メモ**:

- 要求の本文に指定された**コンテナー**は、**存在する必要があります**。
* バックアップの進行中には、SKU のアップグレードやダウングレード、ドメイン名の変更、その他の操作などの、**サービス管理操作は試行しないでください**。
* バックアップの復元は、作成されたときから **7 日間だけ保証されます**。
* 分析レポートの生成に使用される**使用状況データ**は、バックアップに**含まれません**。 使用 [Azure API Management REST API の][] 保管のための分析レポートを定期的に取得します。
* サービス バックアップを実行する頻度は、復旧ポイントの目標に影響を与えます。 その頻度を最小限に抑えるため、定期的なバックアップを実装すると共に、API Management サービスに対して重要な変更を行った後のオンデマンドのバックアップを実行することをお勧めします。
* バックアップ処理の進行中にサービス構成 (API、ポリシー、開発者ポータルの外観など) に対して行われる**変更**は、**バックアップに含まれないので失われる可能性があります**。

## <a name="step2"> </a>API Management サービスの復元

以前に作成されたバックアップから API Management サービスを復元するには、次の HTTP 要求を行います。

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

各値の説明:

* `subscriptionId` -にバックアップを復元する API Management サービスを含むサブスクリプションの id
* `resourceGroupName` -'Api の既定値は {サービス地域}' の形式で文字列を `サービス地域` など、Azure のリージョンにバックアップを復元する API Management サービスがホストされている場所を識別する `North Central US`
* `serviceName` -復元先のサービスが、作成時に指定した API Management の名前
* `api バージョン` -置き換えます `2014年-02-14`

要求の本文に、バックアップ ファイルの場所、つまり Azure ストレージ アカウント名、アクセス キー、BLOB コンテナー名、バックアップ名を指定します。

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

値を設定、 `Content-type` 要求ヘッダーを `-application/json`します。

復元は、完了までに 30 分以上かかることのある長時間の操作です。 要求が成功したかどうかと、復元処理が開始されるお届け、 `202 Accepted` 応答ステータス コードを `場所` ヘッダー。 "Get"要求の URL を `場所` ヘッダーを操作の状態を確認します。 復元の進行中は、「202 Accepted」状態コードの受け取りが続きます。 応答コードの `200 OK` 復元操作が正常に完了が示されます。
>[AZURE.IMPORTANT] 復元先のサービスの **SKU** は、復元されるバックアップ サービスの SKU と**一致しなければなりません**。
>
>復元処理の進行中にサービス構成 (API、ポリシー、開発者ポータルの外観など) に対して行われる**変更**は、**上書きされることがあります**。

## 次のステップ

バックアップ/復元プロセスに関する 2 つのチュートリアルを、次の Microsoft ブログで参照してください。

-   [Azure API Management アカウントのレプリケート](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
    -   Gisela さんによって投稿された記事です。
-   [Azure API Management: バックアップと構成を復元します。](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Stuart さんによる詳細なアプローチで、公式のガイダンスに沿ったものではありませんが、非常に興味深い記事です。


[backup an api management service]: #step1 
[restore an api management service]: #step2 
[azure api management rest api]: http://msdn.microsoft.com/library/azure/dn781421.aspx 
[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png 
[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png 
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png 
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png 
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png 
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png 
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png 
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png 

