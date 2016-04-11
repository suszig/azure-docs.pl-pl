<properties 
    pageTitle="認証されたクライアントからの API アプリの呼び出し" 
    description="Azure Active Directory で認証された Web アプリ クライアントから Azure API アプリを呼び出す方法について説明します。" 
    keywords="app service,azure app service,authentication,azure authentication,api,authentication api"
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2015" 
    ms.author="tdykstra"/>

# Azure Active Directory で認証された Web アプリ クライアントからの Azure API アプリの呼び出し

## 概要

このチュートリアルでは、Azure Active Directory (AAD) によって保護されている API アプリを、同じく AAD によって保護されている Web アプリから呼び出す方法について説明します。 このチュートリアルを完了すると、Web アプリと API アプリが Azure サブスクリプションで実行されます。  次のスクリーン ショットに示すように、Web アプリには、API アプリを呼び出すことで取得するデータが表示されます。

![](./media/app-service-api-authentication-client-flow/aboutpage.png)

ここでは、ユーザーが API アプリでの再ログインを求められないように、AAD 資格情報を Web アプリから API アプリに渡す方法について説明します。

以下の手順を実行します。

- API アプリを作成し、AAD 認証を使用するように構成します。
- Web アプリを作成し、AAD 認証を使用するように構成します。
- 保護されている API アプリを呼び出すためのコードを Web アプリに追加します。
- Web アプリと API アプリの両方を Azure にデプロイします。
- テストを実行して、Web アプリが API アプリを呼び出せることを確認します。

### クライアント フローの認証

Web アプリに追加したコードと呼ばれるプロセスを実装 [クライアント フロー](../app-service/app-service-authentication-overview.md#client-flow) 認証します。 次の図は、AAD アクセス トークンを取得して、そのトークンを API アプリ (Zumo) トークンと交換するプロセスを示しています。

![](./media/app-service-api-authentication-client-flow/clientflow.png)

API apps の認証に API アプリ ゲートウェイのロールに慣れていない場合は、次を参照してください。 [API apps と mobile apps の認証](../app-service/app-service-authentication-overview.md)します。

## 前提条件

このチュートリアルを開始する前にあることを確認します [Visual Studio 2015](https://www.visualstudio.com/) と [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003) をインストールします。 この手順は、Visual Studio 2013 でも有効です。

このチュートリアルは、Visual Studio で Web プロジェクトを操作する方法を理解していることを前提としています。

## AAD での API アプリの作成とセキュリティ保護

1. API アプリ プロジェクトを作成またはダウンロードします。
 
    * プロジェクトを作成する」の指示に従います [API アプリの作成](app-service-dotnet-create-api-app.md)します。

    * プロジェクトをダウンロードするから取得して、 [ContactsList GitHub リポジトリ](https://github.com/tdykstra/ContactsList)します。

    これで、連絡先データを返す Web API プロジェクトを利用できます。

    ![](./media/app-service-api-authentication-client-flow/swaggerlocal.png)

2. Azure で、新しい API アプリに API アプリ プロジェクトをデプロイします。

    手順に従い [API アプリのデプロイ](app-service-dotnet-deploy-api-app.md)します。

     [Azure preview portal], 、 **API の定義** API アプリのブレードでは、Get を表示および Web API の Post メソッドをプロジェクトを展開します。

    ![](./media/app-service-api-authentication-client-flow/apiappinportal.png)

4. Azure Active Directory (AAD) を ID プロバイダーとして使用して、API アプリをセキュリティで保護します。
 
    AAD での指示に従います [API アプリの保護](app-service-api-dotnet-add-authentication.md)します。 実行する必要はありません、 **Post 要求を送信する使用 Postman** チュートリアルのセクションです。

    終了したら、API アプリの **Azure Active Directory** ブレードで、 [Azure preview portal] AAD アプリケーションのクライアント ID と、AAD テナントです。

    ![](./media/app-service-api-authentication-client-flow/aadblade.png)

    AAD アプリケーションの **構成** ] タブで、 [Azure portal] API アプリのアプリ ID URL と応答 URL です。

    ![](./media/app-service-api-authentication-client-flow/aadconfig.png)

## AAD での Web アプリの作成とセキュリティ保護

このセクションでは、AAD 認証用に設定されている Web プロジェクトをダウンロードして構成します。 プロジェクトが、 **に関する** を表示するページでは、ログオン ユーザーの情報を要求します。

![](./media/app-service-api-authentication-client-flow/aboutpagestart.png)

このページには、API アプリから取得した連絡先情報を表示するセクションを後で追加します。

1. Web プロジェクトをダウンロード、 [Webapp-groupclaims-dotnet リポジトリ](https://github.com/AzureADSamples/WebApp-GroupClaims-DotNet/)
 
2. 指示に従って **サンプルを実行する方法** で、 [Readme ファイル](https://github.com/AzureADSamples/WebApp-GroupClaims-DotNet/blob/master/README.md), 、次の例外。
 
    * Readme ファイルでは Visual Studio 2013 を使用するよう指示していますが、Visual Studio 2015 を使用できます。 

    * 新しい ADD アプリケーションを作成するのではなく、作成済みの AAD アプリケーションを使用します。
 
    * 同じ **App ID URI** AAD アプリケーションに既にあることです。 (Readme ファイルで指定された形式に変更しないでください)。 
    
    * その他の AAD アプリケーション設定を指示どおりに変更します。特に、サインオン URL と応答 URL をサンプル アプリのベース URL に設定します。

Web アプリと API アプリの両方が同じ AAD アクセス トークンを使用できるように、API アプリ用に作成したアプリ ID URI をそのまま使用します。 readme で規定された形式にアプリ ID URI を変更すると、その URI で Web アプリにアクセスすることはできますが、API アプリにはアクセスできません。  その AAD トークンを API アプリのゲートウェイに渡して Zumo トークンを取得することはできません。ゲートウェイは、ゲートウェイ URL と "login/aad" で構成されているアプリ ID URI のトークンを想定しているからです。   

## API アプリ用に生成されたクライアント コードの追加

このセクションでは、API アプリを呼び出すための型指定されたインターフェイスに対して、自動生成されたコードを追加します。 

8.  Visual studio で **ソリューション エクスプ ローラー**, - Webapp-groupclaims-dotnet プロジェクトを右クリックし、をクリックして、 **追加 > Azure API アプリ クライアント**です。

9.   **Microsoft Azure API アプリのクライアントを追加** ダイアログ ボックスで、AAD で保護されている選択、API アプリ。

    新しいフォルダーを参照してくださいコード生成が完了したら、 **ソリューション エクスプ ローラー**, 、API アプリの名前に置き換えます。 このフォルダーには、クライアント クラスとデータ モデルを実装するコードが含まれています。 

10. あいまいな参照が原因で生成されたコードでの修正 *ContactsList/ContactsExtensions.cs*: 2 つのインスタンスを変更する `Task.Factory.StartNew` に `System.Threading.Tasks.Task.Factory.StartNew`します。
 
## AAD トークンを Zumo トークンと交換するためのコードを追加します。

1.  HomeController.cs で、App Service SDK と JSON シリアライザー用の `using` ステートメントを追加します。

        using Microsoft.Azure.AppService;
        using Newtonsoft.Json.Linq;

2. AAD アプリケーションのゲートウェイ URL とアプリ ID URI の定数を追加します。 ゲートウェイ URL が http ではなく、https であることを確認します。

        private const string GATEWAY_URL = "https://clientflowgroupaeb4ae60b7cb4f3d966dfa43b6607f30.azurewebsites.net/";
        private const string APP_ID_URI = GATEWAY_URL + "login/aad";

2.  API アプリを呼び出すためにクライアント オブジェクトを取得するメソッドを追加します。

        public async Task<AppServiceClient> GetAppServiceClient()
        {
            var appServiceClient = new AppServiceClient(GATEWAY_URL);
            string userObjectID = ClaimsPrincipal.Current.FindFirst
                ("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
        
            var authContext = new AuthenticationContext
                (ConfigHelper.Authority, new TokenDbCache(userObjectID));
        
            ClientCredential credential = new ClientCredential
                (ConfigHelper.ClientId, ConfigHelper.AppKey);
        
            // Get the AAD token.
            AuthenticationResult result = authContext.AcquireToken(APP_ID_URI, credential);
            var aadToken = new JObject();
            aadToken["access_token"] = result.AccessToken;
        
            // Send the AAD token to the gateway and get a Zumo token
            var appServiceUser = await appServiceClient.LoginAsync
                ("aad", aadToken).ConfigureAwait(false);
        
            return appServiceClient;
        }

    このコードで `ConfigHelper.Authority` たとえば"https://login.microsoftonline.com/、{tenant}"に解決します。"https://login.microsoftonline.com/contoso.onmicrosoft.com"です。 

2.  API アプリを呼び出すコードを、`About` メソッドの末尾にある `return View()` ステートメントの直前に追加します  (次の手順では `About` ビューにコードを追加して、返されたデータを表示します)。

        var appServiceClient = await GetAppServiceClient();
        var client = appServiceClient.CreateContactsList();
        var contacts = client.Contacts.Get();
        ViewData["contacts"] = contacts;

3.  *Views/Home/About.cshtml*, 、コードを追加した直後に、 `h2` 連絡先情報を表示する見出しです。

        <h3>Contacts</h3>
        <table class="table table-striped table-bordered table-condensed table-hover">
            <tr>
                <th>Name</th>
                <th>Email</th>
            </tr>
        
            @foreach (WebAppGroupClaimsDotNet.Models.Contact contact in (IList<WebAppGroupClaimsDotNet.Models.Contact>)ViewData["contacts"])
            {
                <tr>
                    <td>@contact.Name</td>
                    <td>@contact.EmailAddress</td>
                </tr>
            }
        
        </table>


3. アプリケーションの Web.config ファイルで、次のバインド リダイレクトを `<assemblyBinding>` 開始タグの後ろに追加します。

        <dependentAssembly>
          <assemblyIdentity name="System.Net.Http.Primitives" publicKeyToken="b03f5f7f11d50a3a" culture="neutral"/>
          <bindingRedirect oldVersion="0.0.0.0-4.2.28.0" newVersion="4.2.28.0"/>
        </dependentAssembly>

    このバインド リダイレクトがないとアプリは失敗します。App Service SDK には、System.Net.Http.Primitives バージョン 1.5.0.0 への依存関係が含まれていますが、プロジェクトで使用されるバージョンは 4.2.28.0 だからです。
 
3. 指示に従って、 **このサンプルを Azure にデプロイ** で、 [Readme ファイル](https://github.com/AzureADSamples/WebApp-GroupClaims-DotNet/)します。

5. アプリケーションを実行します。

    ![](./media/app-service-api-authentication-client-flow/homepage.png)

6. クリックして **サインイン**, 、し、このアプリケーションを使用している AAD ドメインに、ユーザーの資格情報を入力します。

    ![](./media/app-service-api-authentication-client-flow/signedin.png)

7. クリックして **に関する**します。

    About コントローラーとビューに追加したコードが実行され、API アプリへの認証が正常に行われ、Get メソッドを呼び出したことが表示されます。

    ![](./media/app-service-api-authentication-client-flow/aboutpage.png)

## トラブルシューティング

### HTTP 400 エラー 

ゲートウェイ URL が http ではなく、https であることを確認します。  Azure プレビュー ポータルから直接コピーした場合は、http が指定さている可能性があります。
 
### SQL サーバー エラー

アプリケーションは、GroupClaimContext の接続文字列で識別される SQL Server データベースにアクセスできる必要があります。 デプロイされたサイトの接続文字列が、SQL Database インスタンスを指定していることを確認します。  正しい接続文字列は、デプロイされた Web.config または Azure ランタイム構成設定に配置できます。

## 謝辞

Govind S. Yadav に協力してくれた ([@govindsyadav](https://twitter.com/govindsyadav)) に対してこのチュートリアルの展開を支援します。 

## 次のステップ

App Service API Apps のクライアント フローの認証方法は確認しました。 API アプリで認証を処理するには、その他の方法については、次を参照してください。 [API apps と mobile apps の認証](../app-service/app-service-authentication-overview.md)します。 

[Azure portal]: https://manage.windowsazure.com/
[Azure preview portal]: https://portal.azure.com/

