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

>[AZURE.NOTE] バックアップと障害復旧のための API Management サービス インスタンスの復元のプロセスは、ステージングなどのシナリオ用の API Management サービス インスタンスをレプリケートするためも使用できます。

## Azure リソース マネージャーの要求の認証

>[AZURE.IMPORTANT] バックアップと復元の REST API では、Azure リソース マネージャーを使用し、API Management のエンティティを管理するための REST Api よりも異なる認証メカニズムを持ちます。 このセクションの手順では、Azure リソース マネージャーの要求を認証する方法について説明します。 詳細については、次を参照してください。 [Azure Resource Manager 要求](http://msdn.microsoft.com/library/azure/dn790557.aspx)します。

Azure リソース マネージャーを使用してリソースに実行するすべてのタスクは、Azure Active Directory で以下の手順に従って認証する必要があります。

-   Azure Active Directory テナントにアプリケーションを追加する。
-   追加したアプリケーションのアクセス許可を設定する。
-   Azure リソース マネージャーへの要求を認証するためのトークンを取得する。

最初の手順は、Azure Active Directory アプリケーションの作成です。 ログイン、 [Azure Classic Portal](http://manage.windowsazure.com/) インスタンスおよびに移動し、API Management サービスを含むサブスクリプションを使用して、 **アプリケーション** 、既定の Azure Active Directory] タブをクリックします。

>[AZURE.NOTE] Azure Active Directory の既定のディレクトリが自分のアカウントに表示されていない場合は、自分のアカウントに必要なアクセス許可を付与する Azure サブスクリプションの管理者に問い合わせてください。 既定のディレクトリを見つける方法の詳細については、次を参照してください。 [既定のディレクトリを見つける](../virtual-machines/resource-group-create-work-id-from-persona.md/#locate-your-default-directory-in-the-azure-portal)します。

![Azure Active Directory アプリケーションを作成する][api-management-add-aad-application]

クリックして **追加**, 、**[組織が開発中のアプリケーションを追加**, を選択し、 **ネイティブ クライアント アプリケーション**します。 わかりやすい名前を入力し、次へ進む矢印をクリックします。 プレース ホルダー URL を入力します。 `http://resources` の、 **リダイレクト URI**, これは必須フィールドですが、値は後で使用されません。 チェック ボックスをオンにして、アプリケーションを保存します。

クリックして、アプリケーションが保存されると、 **構成**, 、まで下へスクロール、 **他のアプリケーションに対するアクセス許可** セクションを **アプリケーションの追加**します。

![Add permissions][api-management-aad-permissions-add]

選択 **Windows** **Azure サービス管理 API** 、アプリケーションを追加する] チェック ボックスをクリックします。

![Add permissions][api-management-aad-permissions]

をクリックして **委任されたアクセス許可** 、新たに追加するは、横にある **Windows** **Azure サービス管理 API** アプリケーション、チェック ボックスをオン **Access Azure サービス管理 (プレビュー)**, 、] をクリック **保存**します。

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

`{tentand id}`、`{application id}`、および `{redirect uri}` を、次の指示に従って置き換えます。

`{tenant id}` を、作成した Azure Active Directory アプリケーションのテナント ID に置き換えます。 クリックして、id にアクセスする **エンドポイントを表示**します。

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

* `subscriptionId` - バックアップ対象の API Management サービスを含むサブスクリプションの ID
* `resourceGroupName` - "Api-Default-{service-region}" 形式の文字列。ここで、`service-region` はバックアップ対象の API Management サービスがホストされている Azure リージョンです。例: `North-Central-US`
* `serviceName` - バックアップを作成する API Management サービスの、作成時に指定された名前
* `api-version` -に置き換えます `2014-02-14`

要求の本文に、ターゲットの Azure ストレージ アカウント名、アクセス キー、BLOB コンテナー名、バックアップ名を指定します。

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

`Content-Type` 要求ヘッダーの値を `application/json` に設定します。

バックアップは完了まで何分もかかる長時間の処理です。  要求が成功してバックアップ処理が開始されると、`Location` ヘッダーのある `202 Accepted` 応答状態コードを受け取ります。  `Location` ヘッダー内の URL に "GET" 要求を出して、処理のステータスを確認します。 バックアップの進行中は、「202 Accepted」状態コードの受け取りが続きます。 応答コードの `200 OK` は、バックアップ処理が正常に終了したことを示します。

**注**:

- **コンテナー** 要求本文に指定 **存在する必要があります**します。
* バックアップが、進行中は **サービス管理操作をしないように** などの SKU のアップグレードやダウン グレード、ドメイン名の変更などです。 
* 復元、 **バックアップは 7 日間だけ保証** 、作成されたときからです。 
* **使用状況データ** 分析レポートを作成するために使用される **が含まれていない** バックアップにします。 使用 [Azure API Management REST API][] 保管のための分析レポートを定期的に取得します。
* サービス バックアップを実行する頻度は、復旧ポイントの目標に影響を与えます。 その頻度を最小限に抑えるため、定期的なバックアップを実装すると共に、API Management サービスに対して重要な変更を行った後のオンデマンドのバックアップを実行することをお勧めします。
* **変更** バックアップ中にサービス構成 (Api、ポリシー、開発者ポータルの外観など) に対する操作が進行 **バックアップに含まれない場合があり、そのため、失われます**します。

## <a name="step2"> </a>API Management サービスの復元
以前に作成されたバックアップから API Management サービスを復元するには、次の HTTP 要求を行います。

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

各値の説明:

* `subscriptionId` - バックアップの復元先の API Management サービスを含むサブスクリプションの ID
* `resourceGroupName` - "Api-Default-{service-region}" 形式の文字列。ここで、`service-region` はバックアップの復元先の API Management サービスがホストされている Azure リージョンです。例: `North-Central-US`
* `serviceName` - 復元先の API Management サービスの、作成時に指定された名前
* `api-version` -に置き換えます `2014-02-14`

要求の本文に、バックアップ ファイルの場所、つまり Azure ストレージ アカウント名、アクセス キー、BLOB コンテナー名、バックアップ名を指定します。

    '{  
        storageAccount : {storage account name for the backup},  
        accessKey : {access key for the account},  
        containerName : {backup container name},  
        backupName : {backup blob name}  
    }'

`Content-Type` 要求ヘッダーの値を `application/json` に設定します。

復元は、完了までに 30 分以上かかることのある長時間の操作です。  要求が成功して復元処理が開始されると、`Location` ヘッダーのある `202 Accepted` 応答状態コードを受け取ります。  `Location` ヘッダー内の URL に "GET" 要求を出して、処理のステータスを確認します。 復元の進行中は、「202 Accepted」状態コードの受け取りが続きます。 応答コードの `200 OK` は、復元処理が正常に終了したことを示します。

>[AZURE.IMPORTANT] **、SKU** 復元先のサービスの **と一致する必要があります** 復元されるバックアップ サービスの SKU。
>
>**変更** 行った復元中にサービス構成 (Api、ポリシー、開発者ポータルの外観など) に、操作が実行中です。 **上書きされる可能性が**です。

## 次のステップ
バックアップ/復元プロセスに関する 2 つのチュートリアルを、次の Microsoft ブログで参照してください。

-   [Azure API Management アカウントのレプリケート](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/) 
    -   Gisela さんによって投稿された記事です。
-   [Azure API Management: 構成のバックアップと復元](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
    -   Stuart さんによる詳細なアプローチで、公式のガイダンスに沿ったものではありませんが、非常に興味深い記事です。

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
 

