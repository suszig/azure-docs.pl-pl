<properties
    pageTitle="Azure App Service で API Apps と ASP.NET を使用する | Microsoft Azure"
    description="Visual Studio 2015 を使用して、Azure App Service で ASP.NET API アプリを作成、デプロイし、使用する方法について説明します。"
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="11/25/2015"
    ms.author="tdykstra"/>

# Azure App Service で API Apps と ASP.NET を使用する

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

## 概要

このチュートリアルで作成、 [App Service API apps](app-service-api-apps-why-best-platform.md), 、ASP.NET Web API を API アプリに配置し、ASP.NET MVC クライアントから API アプリを使用します。 このチュートリアルは ASP.NET に関する知識を前提としていますが、Microsoft Azure の使用経験は問いません。 このチュートリアルでは、クラウドで動作する Web API とクライアント アプリケーションを作成します。

サンプル アプリケーションは簡単な連絡先一覧です。 次の図は、MVC アプリが API から受信したデータを表示する方法を示しています。

![](./media/app-service-api-dotnet-get-started/mvccontacts.png)

## 学習内容

Azure App Service が備えている機能のうち、API の開発とホスティングで特に利便性を発揮するのは次の 3 点です。

* API メタデータの統合サポート
* CORS のサポート
* 認証と承認のサポート
 
このトピックは、これらの機能を紹介するシリーズの第 1 回目となるチュートリアルです。 このチュートリアルでは API のメタデータを中心に説明します。2 回目では CORS について、3 回目と 4 回目では認証と承認について取り上げます。

チュートリアルで紹介する内容は次のとおりです。

* Azure SDK for .NET をインストールして、Azure 向け開発用にコンピューターを準備する方法。
* Visual Studio 2015 に組み込まれたツールを利用し、Azure App Service で API アプリと Web アプリを使用する方法。
* Swashbuckle NuGet パッケージを利用して API 検出を自動化し、Swagger API 定義 JSON を動的に生成する方法。
* 自動的に生成されたクライアント コードを使用し、.NET クライアントから API アプリを使用する方法。
* Azure ポータルを使用し、API アプリ メタデータのエンドポイントを構成する方法。
* クライアントのドメインが API のドメインと異なるとき、CORS を利用して JavaScript クライアントから API アプリを呼び出す方法。
* Azure Active Directory を使用し、認証されていないアクセスから API を保護する方法。
* Azure Active Directory にログインしたユーザーが保護されている API を使用する方法。
* サービス プリンシパルを使用し、保護されている API を使用する方法。

## 前提条件

### ASP.NET Web API

このチュートリアルでは、ASP.NET Web API について理解しています。紹介する場合を参照してください。 [ASP.NET Web API 2 の概要](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)します。

## Visual Studio 2015

指示とスクリーン ショットは Visual Studio 2015 の使用を前提としていますが、Visual Studio 2013 の場合でも操作は同じです。

## Azure アカウント

このチュートリアルを完了するには、Azure アカウントが必要です。 そのための方法は次のとおりです。

* [Azure アカウントを無料で開く](/pricing/free-trial/?WT.mc_id=A261C142F)します。 Azure の有料サービスを試用できるクレジットが提供されます。 このクレジットを使い切ってもアカウントは維持されるため、Azure App Service の Web Apps 機能など、無料の Azure サービスと機能を利用できます。
* [Visual Studio のサブスクライバーの特典を有効に](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)します。 MSDN サブスクリプションにより、有料の Azure サービスを利用できるクレジットが毎月与えられます。

場合は、Azure アカウントにサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751)します。 App Service で有効期間の短いスターター アプリをすぐに作成するが、クレジット_カードは不要、および契約も必要ありません。

[AZURE.INCLUDE [install-sdk-2015-2013](../../includes/install-sdk-2015-2013.md)]

このチュートリアルでは、Azure SDK for .NET 2.8.1 以降のバージョンが必要です。

## サンプル アプリケーションの概要

このチュートリアルでは、API アプリと web アプリにデプロイするコード、 [Azure-Samples/app-service-api-dotnet-contact-list](https://github.com/Azure-Samples/app-service-api-dotnet-contact-list) GitHub リポジトリ。 ContactsList Visual Studio ソリューションには次のプロジェクトが含まれています。

* **ContactsList.API** -ASP.NET Web API プロジェクト名と電子メール アドレスのリストを返します。 Get メソッドの最初の呼び出しでハードコードされた連絡先が 3 つ返されます。後続の Put、Post、Delete メソッドの呼び出しで変更がローカルの JSON ファイルに保存されます。
* **ContactsList.MVC** -ContactsList API 用の ASP.NET MVC クライアントです。
* **ContactsList.Angular** -ContactsList API 用の単純 AngularJS UI クライアントです。 保護されていない (認証のない) API アプリを呼び出す方法を示します。
* **ContactsList.Angular.AAD** -Azure Active Directory を使用してユーザーを認証する方法を示す、AngularJS クライアントです。
* **CompanyContacts.API** -ASP.NET Web API プロジェクトを連絡先のハードコードされた一覧を返す Get 要求に応答します。 によって呼び出される、 **ContactsList.API** service サービス (サービス プリンシパル) の認証を使用して、API を呼び出す方法を示すためのメソッドを取得します。
 
## サンプル アプリケーションのダウンロード 

1. ダウンロード、 [Azure-Samples/app-service-api-dotnet-contact-list](https://github.com/Azure-Samples/app-service-api-dotnet-contact-list) リポジトリです。

    実行できます [.zip ファイルをダウンロード](https://github.com/Azure-Samples/app-service-api-dotnet-contact-list/archive/master.zip) またはローカル コンピューターでリポジトリを複製します。 

2. Visual Studio 2015 または 2013 で ContactsList ソリューションを開きます。

2. ソリューションをビルドし、NuGet パッケージを復元します。

## Swagger のメタデータと UI の使用

サポート [Swagger](http://swagger.io/) 2.0 API のメタデータが Azure App Service に組み込まれています。 それぞれの API アプリで、API のメタデータを Swagger の JSON 形式で返す URL エンドポイントを定義できます。 そのエンドポイントから返されたメタデータを使うことで、API の使いやすさに寄与するクライアント コードを生成できます。 

このセクションでは、ASP.NET Web API プロジェクトのメタデータを自動的に生成する方法のほか、API テスト ツールの実行について説明します。 これらのタスクではまだ、Azure App Service を使用しません。API Apps でメタデータを活用する方法については後ほど説明します。

ASP.NET Web API プロジェクトに Swagger 2.0 のメタデータを指定するのにはインストール、 [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet パッケージ。 Swashbuckle はリフレクションを使用し、メタデータを動的に生成します。 ダウンロードした ContactsList.API プロジェクトで Swashbuckle NuGet パッケージが既にインストールされているしを使用して新しいプロジェクトを作成するときに既にインストールされて、 **Azure API アプリ** プロジェクト テンプレートです。 (Visual Studio で: **ファイル > 新規 > プロジェクト > ASP.NET Web アプリケーション > Azure API アプリ**.)

チュートリアルのこのセクションでは、生成した Swagger 2.0 メタデータを確認し、Swagger メタデータに基づいてテスト UI を試します。 

2. ContactsList.API プロジェクトをスタートアップ プロジェクトとして設定します。 (CompanyContacts.API プロジェクトではありません。このプロジェクトは後続のチュートリアルで使用します。)
 
4. F5 キーを押してデバッグ モードでプロジェクトを実行します。

    ブラウザーが開き、403 Forbidden ページが表示されます。

    ![](./media/app-service-api-dotnet-get-started/403.png)

12. ブラウザーのアドレス バーで、行の末尾に `swagger/docs/v1` を追加し、Return を押します。 (URL は `http://localhost:51864/swagger/docs/v1` です。)

    これは API の Swagger 2.0 JSON メタデータを返すために Swashbuckle で使用される既定の URL です。 Internet Explorer を使用している場合、v1.json ファイルをダウンロードするように求められます。

    ![](./media/app-service-api-dotnet-get-started/iev1json.png)

    Chrome または Edge を使用している場合、ブラウザーのウィンドウで JSON が表示されます。

    ![](./media/app-service-api-dotnet-get-started/chromev1json.png)

    次の例は、API の Swagger メタデータの最初のセクションと Get メソッドの定義です。 このメタデータに基づき、次の手順とチュートリアルの後半で使用する Swagger UI はクライアント コードを自動生成します。

        {
          "swagger": "2.0",
          "info": {
            "version": "v1",
            "title": "ContactsList.API"
          },
          "host": "localhost:51864",
          "schemes": [ "http" ],
          "paths": {
            "/api/Contacts": {
              "get": {
                "tags": [ "Contacts" ],
                "operationId": "Contacts_Get",
                "consumes": [ ],
                "produces": [ "application/json", "text/json", "application/xml", "text/xml" ],
                "responses": {
                  "200": {
                    "description": "OK",
                    "schema": {
                      "type": "array",
                      "items": { "$ref": "#/definitions/Contact" }
                    }
                  }
                },
                "deprecated": false
              },

1. ブラウザーを閉じます。

3. ContactsList.API プロジェクトで **ソリューション エクスプ ローラー**, を開き、 *\swaggerconfig.cs* ファイル一覧をスクロールして、次のコードし、[コメント解除します。

        /*
            })
        .EnableSwaggerUi(c =>
            {
        */

    SwaggerConfig.cs ファイルは、プロジェクトに Swashbuckle パッケージをインストールすると作成されます。 このファイルでは、さまざまな方法で Swashbuckle を構成できます。

    コメントを解除したコードにより、以降の手順で使用する Swagger UI が有効になります。 API アプリ プロジェクト テンプレートで Web API プロジェクトを作成すると、セキュリティ対策として既定でこのコードのコメントが解除されます。

5. プロジェクトを再度実行します。

3. ブラウザーのアドレス バーで、行の末尾に `swagger` を追加し、Return を押します。 (URL は `http://localhost:51864/swagger` です。)

4. Swagger UI ページが表示されたら、クリックして **連絡先** 利用可能なメソッドを参照します。

    ![](./media/app-service-api-dotnet-get-started/contactsmethods.png)

5. クリックして **取得 > 試してみる**します。

    Swagger UI が ContactsList Get メソッドを呼び出し、JSON 結果を表示します。

    ![](./media/app-service-api-dotnet-get-started/gettryitout.png)

6. クリックして **Post**, 、下のテキスト ボックスをクリックして **モデル スキーマ**します。

    モデル スキーマをクリックすると、入力ボックスに事前入力されます。このボックスには、Post メソッドのパラメーター値を指定できます。

    ![](./media/app-service-api-dotnet-get-started/post.png)

7. 次の例のように `contact` パラメーター入力ボックスで JSON を変更するか、自分の名前と電子メール アドレスを代わりに指定します。

        {
          "CreatedBy": "",
          "EmailAddress": "carson@contoso.com",
          "Id": 4,
          "Name": "Alexander Carson"
        } 

10. クリックして **試してみる**します。

    ContactsList API は HTTP 200 と応答本文を返し、追加内容を確認します。

11. クリックして **取得 > 試してみる**します。

    Get メソッドの応答に新しい連絡先が含まれます。 

12. Put、Delete、Get by ID メソッドも試し、ブラウザーを閉じます。

Swashbuckle はあらゆる ASP.NET Web API プロジェクトで利用できます。 Swagger メタデータ生成を既存のプロジェクトに追加する場合、Swashbuckle パッケージをインストールします。 App Service の API アプリとしてデプロイする新しいプロジェクトを作成する場合は、ASP.NET を使用して **Azure API アプリ** プロジェクト テンプレートを次の図に示すようにします。

![](./media/app-service-api-dotnet-get-started/apiapptemplate.png)

そのテンプレートにより、Swashbuckle がインストールされた Web API プロジェクトが作成されます。

**注:** 既定では、Swashbuckle がコント ローラー メソッドの Swagger 操作 Id の重複を生成します。 この現象は、コントローラーに HTTP メソッドのオーバーロード (`Get()` と `Get(id)` など) が存在すると発生します。 オーバー ロードを処理する方法については、次を参照してください。 [をカスタマイズする Swashbuckle で生成された API 定義](app-service-api-dotnet-swashbuckle-customize.md)します。 Visual Studio で Web API プロジェクトを作成するには、Azure API アプリ テンプレートを使用して、一意の操作 Id を生成するコードが自動的に追加する、 *SwaggerConfig.cs* ファイルです。  

## Azure で API アプリを作成し、それに ContactsList.API プロジェクトをデプロイする

このセクションでは、Visual Studio に統合されている Azure のツールを使用 **Web の発行** Azure で新しい API アプリを作成するウィザード。 次に、ContactsList.API プロジェクトを新しい API アプリにデプロイし、もう一度、今度はクラウドで実行中に Swagger UI を実行して API を呼び出します。

1.  **ソリューション エクスプ ローラー**, を ContactsList.API プロジェクトを右クリックして、クリックして **発行**します。

3.   **プロファイル** のステップ、 **Web の発行** ウィザード] をクリックして **Microsoft Azure App Service**します。

    ![](./media/app-service-api-dotnet-get-started/selectappservice.png)

4. まだ行っていない場合は Azure アカウントにサインインし、有効期限が切れている場合は資格情報を更新します。

4. App Service] ダイアログ ボックスで、Azure を選択して **サブスクリプション** を使用し、をクリックする **新規**します。

    ![](./media/app-service-api-dotnet-get-started/clicknew.png)

3.  **ホスティング** のタブ、 **App Service の作成** ダイアログ ボックスで、をクリックして **の種類の変更**, 、順にクリック **API アプリ**します。

    ![](./media/app-service-api-dotnet-get-started/apptype.png)

4. 入力、 **API アプリの名前** 内で一意である、 *azurewebsites.net* ドメイン。 

    Visual Studio により、プロジェクト名に date-time 文字列が追加された一意の名前が提案されます。  その名前で問題なければ、それを利用して構いません。 

    入力した名前が既に使用されている場合は、右側に緑色のチェック マークではなく赤色の感嘆符が表示されます。この場合、別の名前を入力する必要があります。

    Azure では、この名前がアプリケーションの URL のプレフィックスとして使用されます。 この名前と完全な URL では *. azurewebsites.net*します。 たとえば、名前が `ContactsListAPI` である場合、URL は `contactslistapi.azurewebsites.net` です。

6.  **リソース グループ** ドロップダウン リストで、したい場合は、"ContactsListGroup"または別の名前を入力してください。

    このボックスでは、既存を選択できます。 [リソース グループ](../azure-preview-portal-using-resource-groups.md) か、サブスクリプション内の既存のリソース グループとは異なる名前を入力して新しいものを作成します。

    このチュートリアルでは、新しいリソース グループを作成すると便利です。チュートリアルのために作成したすべての Azure リソースを 1 回の手順で簡単に削除できるからです。

4. クリックして、 **新規** ボックスの横に、 **App Service プラン** ボックスの一覧です。 

    ![](./media/app-service-api-dotnet-get-started/createas.png)

    App Service プランについては、次を参照してください。 [App Service プランの概要](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)します。

5.  **App Service プランの構成** ダイアログ ボックスで、したい場合は、"ContactsListPlan"または別の名前を入力してください。

5.  **場所** ドロップダウン リストで、地に最も近い場所を選択します。

    この設定によって、アプリが実行される Azure データ センターが指定されます。 このチュートリアルでは、任意のリージョンを選択することができます。任意のリージョンを選択しても、大きな違いはありません。 最小化するためにアクセスするクライアントにできるだけ近くにサーバーを選択して、運用アプリケーションが、 [遅延](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)します。

5.  **サイズ** ボックスの一覧をクリックして **Free**します。

    無料の価格レベルでこのチュートリアルに十分な性能が提供されます。

6.  **App Service プランの構成** ダイアログ ボックスで、をクリックして **[ok]**します。

    ![](./media/app-service-api-dotnet-get-started/configasp.png)

7.  **App Service の作成** ダイアログ ボックスで、をクリックして **作成**します。

    Visual Studio を使うと、API アプリが作成され、新しい API アプリに必要なすべての設定が含まれる発行プロファイルが作成されます。 次の手順では、新しい発行プロファイルを使用し、プロジェクトをデプロイします。 
 
    **注:** を Azure App Service で API アプリを作成するには、その他の方法があります。 Visual Studio では、新しいプロジェクトを作成するとき、同じダイアログが利用できます。 Azure ポータルを使用して、API アプリを作成することも [Windows PowerShell 用の Azure コマンドレット](../powershell-install-configure.md), 、または [クロスプラット フォーム コマンド ライン インターフェイス](../xplat-cli.md)します。

8.  **接続** のタブ、 **Web の発行** ウィザード] をクリックして **発行**します。

    ![](./media/app-service-api-dotnet-get-started/clickpublish.png)

    Visual Studio を使うと、ContactsList.API プロジェクトに新しい API アプリがデプロイされ、ブラウザーで API アプリの URL が開きます。 ブラウザーに "正常に作成されました" ページが表示されます。

    ![](./media/app-service-api-dotnet-get-started/appcreated.png)

11. ブラウザーのアドレス バーの URL に「swagger」を追加し、Enter キーを押します。 (URL は `http://{apiappname}.azurewebsites.net/swagger` です。)

    ブラウザーに前に見た Swagger UI が表示されますが、今度はクラウドで実行しています。 Get メソッドを試してみます。既定の 3 件の連絡先に戻ります。先に行った変更がローカル ファイルに保存されたためです。これから行う変更は Azure API アプリのファイル システムに保存されます。

12. 開いている、 [Azure ポータル](https://portal.azure.com/)します。
 
14. クリックして **参照 > API Apps > {新しい API アプリ}**します。

    ![](./media/app-service-api-dotnet-get-started/choosenewapiappinportal.png)

16. をクリックして **設定**, 、し、次に、 **設定** ブレードは、API のセクションを検索してクリックして **API の定義**します。 

    ![](./media/app-service-api-dotnet-get-started/apidefinsettings.png)

    API 定義ブレードでは、JSON 形式で Swagger 2.0 メタデータを返す URL を指定できます。 Visual Studio により API アプリが作成されるとき、先に見た既定値 (API アプリの基礎 URL + `/swagger/docs/v1`) に API 定義 URL が設定されます。 

    ![](./media/app-service-api-dotnet-get-started/apidefurl.png)

    クライアント コードを生成する API アプリを選択するとき、Visual Studio はこの URL からメタデータを取得します。 

### Azure リソース マネージャーのツールにおける API 定義 URL

Azure リソース マネージャーの Azure PowerShell、CLI などのツールを使用して、API アプリの API 定義の URL を構成することも、または [リソース エクスプ ローラー](https://resources.azure.com/)します。 

設定、 `apiDefinition` Microsoft.Web/sites/config リソースの種類のプロパティ、 <site name>/web リソースです。 たとえば、 **リソース エクスプ ローラー**, には、 **サブスクリプション > {サブスクリプション} > resourceGroups > {リソース グループ} > プロバイダー > Microsoft.Web > サイト > {ƒtƒcƒg} > 構成 > web**, と cors のプロパティが表示されます。

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

## <a id="codegen"></a> 生成されたクライアント コードを使用して .NET クライアントから利用する 

Azure API アプリと Swagger の統合の利点の 1 つは、自動コード生成です。 生成されたクライアント クラスにより、API アプリを呼び出すコードの記述が容易になります。

このセクションでは、ASP.NET MVC Web アプリから API アプリを使用する方法について確認します。 先にローカルで MVC クライアントと Web API を実行し、次に Azure App Service で新しい Web アプリにクライアントをデプロイし、クラウドで実行します。

### クライアント コードの生成

API アプリのクライアント コードは、Visual Studio を使用して生成するか、コマンド ラインから生成することができます。 このチュートリアルでは、Visual Studio を使用します。 コマンドラインから実行する方法についての readme ファイルを参照してください、 [Azure/autorest](https://github.com/azure/autorest) GitHub.com のリポジトリにします。

ContactsList.MVC プロジェクトによりクライアント コードが既に生成されていますが、それを削除して再生成し、独自の API アプリの URL を既定のターゲット URL にできます。

1. Visual Studio で **ソリューション エクスプ ローラー**, 、ContactsList.MVC プロジェクトで、削除、 *ContactsList.API* フォルダーです。

    このフォルダーは、今まさに行おうとしているコード生成プロセスを使って作成されたものです。

    ![](./media/app-service-api-dotnet-get-started/deletecodegen.png)

2. ContactsList.MVC プロジェクトを右クリックし、 **追加 > REST API クライアント**です。

    ![](./media/app-service-api-dotnet-get-started/codegenmenu.png)

3.  **REST APi のクライアントの追加** ダイアログ ボックスで、をクリックして **Microsoft Azure API アプリからダウンロード**, 、] をクリックし、 **参照**します。

    ![](./media/app-service-api-dotnet-get-started/codegenbrowse.png)

8.  **App Service** ] ダイアログ ボックスで、展開、 **ContactsListGroup** リソースをグループ化し、API アプリを選択し、クリックして **OK**します。

    このダイアログ ボックスでは、API アプリが多すぎてスクロールできない場合、いくつかの方法で一覧の API アプリを整理できます。 検索文字列を入力し、名前で API アプリを絞り込むこともできます。

    ![](./media/app-service-api-dotnet-get-started/codegenselect.png)

    API アプリが一覧に表示されない場合、API アプリを作成するときに、アプリの種類を (Web アプリから API アプリに) 変更する手順を誤って省略した可能性があります。 その場合は、先行する手順をやり直して新しい API アプリを作成してください。 API アプリに別の名前を選択する必要があります。または、ポータルに移動して先に Web アプリを削除しておいてください。 

    戻ると、 **REST API のクライアントの追加** ダイアログ ボックスで、テキスト ボックスのデータが、ポータルの前に紹介した API の定義の URL の値が格納されました。 

    ![](./media/app-service-api-dotnet-get-started/codegenurlplugged.png)

    代替として、参照ダイアログを利用する代わりに URL を直接入力できます。 たとえば、API を Web アプリにデプロイしたとき、それが参照ダイアログに表示されない場合、Swagger メタデータを返す URL をここに直接入力できます。

    また、 **既存の Swagger メタデータ ファイルを選択** オプション。 Azure にデプロイする前にコードを生成する場合、ローカルで実行し、Swagger JSON ファイルをダウンロードし、それをここで選択できます。 

9.  **REST API のクライアントの追加** ダイアログ ボックスで、をクリックして **OK**します。

    Visual Studio が API アプリを元に命名したフォルダーを作成し、クライアント クラスを生成します。

    ![](./media/app-service-api-dotnet-get-started/codegenfiles.png)

5. 開いている *Controllers\ContactsController.cs* 、生成されたクライアントを使用して、API を呼び出すコードを表示します。 

    次のスニペットは、クライアント オブジェクトをインスタンス化し、Get メソッドを呼び出す方法を示しています。

        private ContactsListAPI db = new ContactsListAPI(new Uri("http://localhost:51864"));
        
        public ActionResult Index()
        {
            return View(db.Contacts.Get());
        }

    このコードにより、クライアント クラス コンストラクターへの API プロジェクトのローカル IIS Express URL に渡されるので、MVC Web プロジェクトと API プロジェクトをローカルで実行できます。 コンストラクター パラメーターを省略した場合、既定のエンドポイントはコードの生成元の URL です。 

6. クライアント クラスは API アプリ名に基づく別の名前で生成されます。型名がプロジェクトで生成されたものに一致するようにこのコードを変更します。 たとえば、API アプリに ContactsListAPIContoso という名前を付けた場合、コードは次のようになります。

        private ContactsListAPIContoso db = new ContactsListAPIContoso(new Uri("http://localhost:51864"));
        
        public ActionResult Index()
        {
            return View(db.Contacts.Get());
        }

7. ソリューションをビルドします。

MVC プロジェクトのコントローラーとビューは、Entity Framework に対してスキャフォールディングされたコントローラーとビューに似ています。これはそのように作られているためです。つまり、Entity Framework データ モデルをスキャフォールディングし、Entity Framework データベース コンテキストの代わりに REST API クライアントを使用するように細かな変更を加えるという方法です。

### ローカルで実行する

1. ContactsList.API の開始時、ContactsList.MVC の前に、ContactsList.API および ContactsList.MVC プロジェクトをスタートアップ プロジェクトとして設定します。 (ソリューションを右クリックし、クリックして **プロパティ**, 、] をクリックして **マルチ スタートアップ プロジェクト**, 、各プロジェクトに設定し、 **開始**します。 上/下矢印アイコンを使用し、ContactsList.API をリストの最初にします。) 

2. F5 キーを押してプロジェクトを開始します。

    1 つのブラウザーに API の 403 ページが表示され、別のブラウザーに MVC アプリのホーム ページが表示されます。

3. クリックして **連絡先** MVC アプリのホーム ページを表示するブラウザーのメニュー バーにします。

    MVC の UI には、ローカルに格納されている連絡先が表示されます。UI を使用して、連絡先の追加および削除ができます。

    ![](./media/app-service-api-dotnet-get-started/mvccontacts.png)

### Azure で Web アプリを作成し、それに ContactsList.MVC プロジェクトをデプロイする

このセクションでは、先に Web アプリを作成した同じメソッドで API アプリを作成し、同じメソッドで Web プロジェクトを Azure Web アプリにデプロイします。

#### Azure API アプリを指すように MVC プロジェクトを変更します。 

Azure にデプロイする前に、コードがデプロイされると、localhost の代わりに先に作成した Azure API アプリを呼び出すように、MVC プロジェクトで API エンドポイントを変更します。

1. ContactsList.MVC プロジェクトで開きます *Controllers\ContactsController.cs*します。

2. API ベース URL を localhost URL に設定する行をコメントアウトし、コンストラクター パラメーターのない行をコメント解除します。 コードは、両方の行のクラス名にコード生成元の API アプリの名前が反映されることを除き、次の例のようになります。

        private ContactsListAPI db = new ContactsListAPI();
        //private ContactsListAPI db = new ContactsListAPI(new Uri("http://localhost:51864"));

    既定のターゲット URL は Azure API アプリです。これはそこからコードを生成したためです。別のメソッドでコードを生成したのであれば、場合によっては、ローカル URL を指定したときと同じ方法で Azure API アプリ URL を指定する必要があります。 

#### MVC サイトをホストする Web アプリを作成する

1.  **ソリューション エクスプ ローラー**, を ContactsList.MVC プロジェクトを右クリックして、クリックして **発行**します。

2.  **Web の発行** ウィザード] をクリックして、 **プロファイル** ] タブをクリックします。

3.   **プロファイル** のタブ、 **Web の発行** ウィザード] をクリックして **Microsoft Azure App Service**します。

5.  **App Service** ] ダイアログ ボックスをクリックして **新規**します。

3.  **ホスティング** のタブ、 **App Service の作成** ダイアログ ボックスで、をクリックして **の種類の変更**, 、型かどうかを確認 **Web アプリ**します。

4. 入力、 **Web アプリ名** 内で一意である、 *azurewebsites.net* ドメイン。 

5. Azure を選択して **サブスクリプション** を操作します。

6.  **リソース グループ** ドロップダウン リストで、先ほど作成した同じリソース グループを選択します。

4.  **App Service プラン** ドロップダウン リストで、先ほど作成した同じプランを選択します。 

7. クリックして **作成**します。

    Visual Studio は、web アプリを作成、発行プロファイルを作成し、表示、 **接続** のステップ、 **Web の発行** ウィザード。

### ContactsList.Web プロジェクトを新しい Web アプリにデプロイする

3.   **接続** のステップ、 **Web の発行** ウィザード] をクリックして **発行**します。

    Visual Studio を使うと、ContactsList.MVC プロジェクトが新しい Web アプリにデプロイされ、Web アプリの URL がブラウザーで開きます。 ローカルで表示されていたものと同じ MVC UI が現れます。ただし、今回は Azure API アプリのファイル システムに保存されている連絡先が表示されます。

    ![](./media/app-service-api-dotnet-get-started/codegencontacts.png)

## 次のステップ

このチュートリアルでは、API アプリを作成、それにコードをデプロイし、.NET クライアントから使用する方法について学習しました。 API アプリの取得中に、次のチュートリアル シリーズの番組をどのように開始する [CORS を使用して、JavaScript クライアントから API アプリを使用する](app-service-api-cors-consume-javascript.md)です。

