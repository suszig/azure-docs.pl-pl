<properties 
    pageTitle="Azure Search 管理 REST API の使用 | Microsoft Azure | ホスト型クラウド検索サービス" 
    description="管理 REST API を利用し、ホスト型のクラウド検索サービスの Azure Search を管理する" 
    services="search" 
    documentationCenter="" 
    authors="HeidiSteen" 
    manager="mblythe" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="11/04/2015" 
    ms.author="heidist"/>

# Azure Search 管理 REST API の使用

Azure Search REST 管理 API は、ポータルでプログラムを使用して管理タスクを実行するものです。 サービス管理操作には、サービスの作成または削除、サービスのスケーリング、およびキーの管理があります。 このチュートリアルには、サービス管理 API を示すサンプル クライアント アプリケーションが用意されています。 また、ローカル開発環境でサンプルを実行するのに必要な構成手順も含まれます。

このチュートリアルを完了するには、以下が必要です。

- Visual Studio 2012 または 2013
- サンプル クライアント アプリケーションのダウンロード

このチュートリアルを完了する過程で、Azure Search と Azure Active Directory (AD) の 2 つのサービスをプロビジョニングします。 さらに、クライアント アプリケーションと Azure のリソース マネージャー エンドポイントとの間で信頼を確立する AD アプリケーションを作成します。

このチュートリアルを完了するには、Azure アカウントが必要です。


##サンプル アプリケーションのダウンロード

このチュートリアルは C# で記述された Windows のコンソール アプリケーションを基にしていて、Visual Studio 2012 または 2013 で実行することができます。

Codeplex のクライアント アプリケーションを検索する [Azure Search Management API Demo](https://azuresearchmgmtapi.codeplex.com/)します。


##アプリケーションの構成

サンプル アプリケーションを実行する前に、認証を有効にして、クライアント アプリケーションからリソース マネージャー エンドポイントに送信された要求が許可されるようにする必要があります。 認証の要件から発生し、 [Azure リソース マネージャー](http://msdn.microsoft.com/library/azure/dn790568.aspx), 、Search サービス管理関連も含め、API を使用して要求されたすべてのポータル関連操作の基盤となっています。 Azure Search のサービス管理 API は、Azure リソース マネージャーの拡張機能であるため、その依存関係を継承しています。  

Azure リソース マネージャーには、ID プロバイダーとして Azure Active Directory サービスが必要です。 

リソース マネージャーへの要求を許可するアクセス トークンを取得するには、クライアント アプリケーションに Active Directory を呼び出すためのコード セグメントが必要です。 コード セグメントとコード セグメントを使用する前提条件となる手順は、この記事から一時的に移動されました: [Azure Resource Manager 要求](http://msdn.microsoft.com/library/azure/dn790557.aspx)します。

前述のリンクの指示に従うか、またはチュートリアルをひととおり実行する場合はこのドキュメントの手順に従ってください。

このセクションでは、次のタスクを実行します。

1. AD サービスの作成
1. AD アプリケーションの作成
1. ダウンロードしたサンプル クライアント アプリケーションの詳細を登録することによる、AD アプリケーションの構成
1. 要求の認証を取得するための値を使用したサンプル クライアント アプリケーションのロード

> [AZURE.NOTE] 次のリンクが、リソース マネージャーへのクライアント要求の認証に Azure Active Directory を使用して背景を提供します。 [Azure リソース マネージャー](http://msdn.microsoft.com/library/azure/dn790568.aspx), 、[Azure Resource Manager 要求](http://msdn.microsoft.com/library/azure/dn790557.aspx), 、と [Azure Active Directory](http://msdn.microsoft.com/library/azure/jj673460.aspx)します。

###Active Directory サービスの作成

1. サインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。

2. 左側のナビゲーション ウィンドウ下にスクロールし、クリックして **Active Directory**します。

4. クリックして **新規** を開くには **App Services** | **Active Directory**します。 このステップでは、新しい Active Directory サービスを作成します。 このサービスは、この後で定義する AD アプリケーションをホストします。 新しいサービスを作成することで、Azure で既にホストしている他のアプリケーションからチュートリアルを切り離すことができます。

5. クリックして **ディレクトリ** | **カスタム作成**します。

6. サービス名、ドメイン、および地理的な場所を入力します。 ドメインは一意である必要があります。 チェック マークをクリックしてサービスを作成します。

     ![][5]

###このサービス用の新しい AD アプリケーションの作成

1. 今作成した"SearchTutorial" Active Directory サービスを選択します。

2. 上部のメニューをクリックして **アプリケーション**します。 
 
3. クリックして **アプリケーションを追加**します。 AD アプリケーションは、それを ID プロバイダーとして使用するクライアント アプリケーションに関する情報を格納します。  
 
4. 選択 **[組織が開発中のアプリケーションを追加**します。 このオプションは、アプリケーション ギャラリーに発行されていないアプリケーションの登録を設定します。 クライアント アプリケーションはアプリケーション ギャラリーに含まれていないため、このチュートリアルではこれが適切な選択です。

     ![][6]
 
5. 「Azure-Search-Manager」などの名前を入力します。

6. 選択 **ネイティブ クライアント アプリケーション** アプリケーションの種類。 Windows クライアント (コンソール) アプリケーションの場合、これがサンプル アプリケーションでは正しい手順です。Web アプリケーションの場合は異なります。

     ![][7]
 
7. リダイレクト URI に、「http://localhost/Azure-Search-Manager-App」と入力します。 Azure Active Directory では、OAuth 2.0 承認要求に対してユーザー エージェントがこの URI にリダイレクトされます。 この値は、物理的なエンドポイントである必要はありませんが、有効な URI である必要があります。 

    このチュートリアルの目的のためには任意の値を入力できますが、入力した値は、サンプル アプリケーションでの管理用の接続に必要な入力になります。 
 
7. チェック マークをクリックして、Active Directory アプリケーションを作成します。 左側のナビゲーション ウィンドウには、"Azure-Search-Manager-App" が表示されます。

###AD アプリケーションの構成
 
9. 作成した AD アプリケーション "Azure-Search-Manager-App" をクリックします。 左側のナビゲーション ウィンドウに一覧表示されています。

10. クリックして **構成** 上部のメニュー。
 
11. アクセス許可と選択] にスクロールして **Azure 管理 API**します。 この手順で、クライアント アプリケーションがアクセスする必要がある API (ここでは、Azure リソース マネージャー API) および必要なアクセス レベルを指定します。

12. 委任されたアクセス許可の [ドロップ ダウン リストからをクリックして **Access Azure サービス管理 (プレビュー**)。
 
     ![][8]
 
13. 変更を保存します。 

アプリケーションの構成ページを開いたままにします。 次の手順では、このページから値をコピーして、サンプル アプリケーションに入力します。

###登録とサブスクリプションの値を使用したサンプル アプリケーション プログラムのロード

このセクションでは、ポータルから取得した有効な値を代入して、Visual Studio でソリューションを編集します。
追加する値は、Program.cs の上部に表示されます。

        private const string TenantId = "<your tenant id>";
        private const string ClientId = "<your client id>";
        private const string SubscriptionId = "<your subscription id>";
        private static readonly Uri RedirectUrl = new Uri("<your redirect url>");

いないものがあれば [Codeplex からサンプル アプリケーションをダウンロード](https://azuresearchmgmtapi.codeplex.com/), 、この手順で必要になります。

1. 開いている、 **ManagementAPI.sln** Visual Studio でします。

2. Program.cs を開きます。

3. `ClientId` を指定します。 前の手順で開いたままにした AD アプリケーションの構成ページから、ポータルの AD アプリケーションの構成ページからクライアント ID をコピーして、Program.cs に貼り付けます。

4. `RedirectUrl` を指定します。 同じポータルのページからリダイレクト URI をコピーし、Program.cs に貼り付けます。

    ![][9]

5. `TenantID.` を指定します。 
    - [Active Directory]、[SearchTutorial (サービス)] に戻ります。 
    - クリックして **アプリケーション** 上部のバーです。 
    - クリックして **エンドポイントの表示** ページの下部にあります。 
    - 一覧の最後の、OAUTH 2.0 承認エンドポイントをコピーします。 
    - TenantID にエンドポイントを貼り付けます。TenantID 以外のすべての URI パラメーターの値を除去します。

    "https://login.windows.net/55e324c7-1656-4afe-8dc3-43efcd4ffa50/oauth2/authorize?api-version=1.0" の場合、"55e324c7-1656-4afe-8dc3-43efcd4ffa50" 以外すべてを削除します。

    ![][10]

6. `SubscriptionID` を指定します。
    - メインのポータル ページに移動します。
    - クリックして **設定** 左側のナビゲーション ウィンドウの下部にあります。
    - [サブスクリプション] タブで、サブスクリプション ID をコピーし、Program.cs に貼り付けます。

7. 保存し、ソリューションを構築します。


##アプリケーションの探索

最初のメソッド呼び出しでブレークポイントを追加して、プログラムをステップ実行できるようにします。 キーを押して **f5 キーを押して** をアプリケーションを実行し、キーを押します **F11** コードをステップ実行します。

サンプル アプリケーションでは、既存の Azure サブスクリプションに対する無料 Azure Search サービスを作成します。 既に無料のサービスがサブスクリプションに存在する場合は、サンプル アプリケーションは失敗します。 サブスクリプションごとに 1 つだけ無料検索サービスが許可されます。

1. ソリューション エクスプ ローラーから Program.cs を開き、Main (string[] void) 関数に移動します。 
 
3. 注意して **ExecuteArmRequest** を使用して、Azure リソース マネージャー エンドポイントに対する要求を実行 `https://management.azure.com/subscriptions` 、指定された `subscriptionID`します。 このメソッドはプログラム全体で使用され、Azure リソース マネージャー API または Search 管理 API を使用して操作を実行します。

3. Azure リソース マネージャーへの要求は認証および承認される必要があります。 これは、 **GetAuthorizationHeader** によって呼び出されるメソッド、 **ExecuteArmRequest**  から借用メソッド [Azure Resource Manager 要求](http://msdn.microsoft.com/library/azure/dn790557.aspx)します。 注意して **GetAuthorizationHeader** 呼び出し `https://management.core.windows.net` アクセス トークンを取得します。

4. サブスクリプションに対して有効なユーザー名とパスワードを使用してサインインすることが求められます。

5. 次に、Azure リソース マネージャーのプロバイダーに新しい Azure Search サービスが登録されます。 繰り返しますが、これは、 **ExecuteArmRequest** メソッドは、この時間を使用してサブスクリプションの Azure で検索サービスを作成するために使用 `providers/Microsoft.Search/register`します。 

6. プログラムの残りの部分を使用して、 [Azure Search 管理 REST API](http://msdn.microsoft.com/library/dn832684.aspx)します。 この API の `api-version` は Azure リソース マネージャーの API バージョンとは異なることに注意してください。 たとえば、`/listAdminKeys?api-version=2014-07-31-Preview` は、Azure Search Management REST API の `api-version` を示しています。

    次の一連の操作では、今作成したサービス定義および管理 API キーの取得、キーの再生成と取得、レプリカとパーティションの変更、最後にサービスの削除を行います。

    サービスのレプリカ、またはパーティションの数を変更する場合、無償のエディションを使用しているとこの操作は失敗すると想定されます。 標準エディションだけがパーティションとレプリカを追加できます。

    最後の操作はサービスの削除です。

##次のステップ

このチュートリアルを完了した後は、次の記事によって Active Directory サービスでのサービスの管理または認証について詳しく知ることができます。

- クライアント アプリケーションと Active Directory との統合の詳細については、 参照してください [Azure Active Directory にアプリケーションを統合](http://msdn.microsoft.com/library/azure/dn151122.aspx)します。
- Azure のその他のサービス管理操作の詳細については、 参照してください [サービスの管理](http://msdn.microsoft.com/library/azure/dn578292.aspx)します。

<!--Anchors-->
[Download the sample application]: #Download
[Configure the application]: #config
[Explore the application]: #explore
[Next Steps]: #next-steps

<!--Image references-->
[5]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-Service.PNG
[6]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App.PNG
[7]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-App-prop.PNG
[8]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPermissions.PNG
[9]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-ConfigPage.PNG
[10]: ./media/search-get-started-management-api/Azure-Search-MGMT-AD-OAuthEndpoint.PNG

<!--Link references-->
[Manage your search solution in Microsoft Azure]: search-manage.md
[Azure Search development workflow]: search-workflow.md
[Create your first azure search solution]: search-create-first-solution.md
[Create a geospatial search app using Azure Search]: search-create-geospatial.md


 
