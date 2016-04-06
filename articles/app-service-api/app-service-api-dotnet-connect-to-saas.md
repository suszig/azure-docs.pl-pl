<properties 
    pageTitle="Azure App Service のカスタム SaaS コネクタ ASP.NET API アプリ" 
    description="API アプリから SaaS プラットフォームに接続するコードを記述する方法と、.NET クライアントから API アプリを呼び出す方法について説明します。" 
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
    ms.date="09/22/2015" 
    ms.author="tdykstra"/>

# Azure App Service での ASP.NET API アプリから SaaS プラットフォームへの接続

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルではコード記述と構成方法、 [API アプリ](app-service-api-apps-why-best-platform.md) に接続する、 [ソフトウェアとしてのサービス (SaaS) プラットフォーム](../app-service/app-service-authentication-overview.md#obotosaas) を使用して、 [App Service API apps SDK for .NET](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service/)します。 使用して .NET クライアントから API アプリを呼び出す方法も紹介、 [App Service SDK for .NET](http://www.nuget.org/packages/Microsoft.Azure.AppService)します。 このチュートリアルでは、最終的に、Azure App Service で実行される .NET API アプリを .NET コンソール アプリのクライアントで呼び出すことができるようになります。 この API アプリから Dropbox API を呼び出し、ユーザーの Dropbox アカウント内にあるファイルとフォルダーの一覧を返します。

カスタム API アプリから直接 SaaS の API を呼び出すコードを記述する代わりを呼び出すことができます、事前にパッケージされた [コネクタ API アプリ](../app-service-logic/app-service-logic-what-are-biztalk-api-apps.md)します。 実行する方法については、次を参照してください。 [展開 SaaS コネクタ API アプリを構成および](app-service-api-connnect-your-app-to-saas-connector.md)です。

このチュートリアルでは、次の手順について説明します。

* Visual Studio で API アプリ プロジェクトを作成します。 
* 構成、 *apiapp.json* API アプリから Dropbox サービスに接続を有効にするファイルです。
* Dropbox を呼び出して結果を返すコードを追加します。
* Azure で新しい API アプリを作成します。
* API アプリにプロジェクトをデプロイします。
* API アプリを構成します。
* ゲートウェイを構成します。
* テスト クライアントを作成します。
* テスト クライアントを実行します。

## 前提条件

このチュートリアルの前提条件は次のとおりです。

* 完了して、 [API アプリの作成](app-service-dotnet-create-api-app.md) と [API アプリのデプロイ](app-service-dotnet-deploy-api-app.md) チュートリアルです。
  
* 説明したように、認証のため、Azure App Service ゲートウェイ アーキテクチャの基礎知識がある [API apps と mobile apps の認証](app-service-authentication-overview.md)します。

* 説明したように、Azure プレビュー ポータルで API アプリを使用する方法を理解して [API アプリとゲートウェイのブレードに移動する方法](app-service-api-manage-in-portal.md#navigate)します。

## API アプリ プロジェクトの作成
 
手順については、直接的なプロジェクトの名前を入力する、場合に、入力 *SimpleDropbox*します。 

[AZURE.INCLUDE [app-service-api-create](../../includes/app-service-api-create.md)]

## 構成、 *apiapp.json* ファイル

API アプリから SaaS プラットフォームへの呼び出しを送信するには、SaaS プラットフォームがで指定するのには、 *apiapp.json* ファイルです。 

1. 開いている、 *apiapp.json* ファイルを追加、 `authentication` プロパティを次に示すように (前のプロパティの後にコンマを追加する必要ありますも)。

        "authentication": [
          {
            "type": "dropbox"
          }
        ]

    apiapp.json ファイルの全体像は次の例のようになります。 

        {
            "$schema": "http://json-schema.org/schemas/2014-11-01/apiapp.json#",
            "id": "SimpleDropBox",
            "namespace": "microsoft.com",
            "gateway": "2015-01-14",
            "version": "1.0.0",
            "title": "SimpleDropBox",
            "summary": "",
            "author": "",
            "endpoints": {
                "apiDefinition": "/swagger/docs/v1",
                "status": null
            },
            "authentication": [
              {
                "type": "dropbox"
              }
            ]
        }

2. ファイルを保存します。

`authentication` プロパティの設定には、いくつかの効果が伴います。

* ポータルの API アプリ ブレードに、SaaS プラットフォームのクライアント ID とクライアントのシークレット値を入力できる UI が表示されます。

    ![](./media/app-service-api-dotnet-connect-to-saas/authblade.png)

* これにより、API アプリがゲートウェイから SaaS プロバイダーのアクセス トークンを取得できるようになります。このトークンは、SaaS プロバイダーの API を呼び出すときに使用します。

`authentication` プロパティは配列ですが、今回のプレビュー リリースでは、複数プロバイダーの指定がサポートされていません。

サポートされているプラットフォームの一覧は、次を参照してください。 [他の SaaS プラットフォームにアクセスするユーザーの同意を取得する](../app-service/app-service-authentication-overview.md#obotosaas)です。

この例のように、スコープを指定することもできます。

        "authentication": [
          {
            "type": "google",
            "scopes": ["https://www.googleapis.com/auth/userinfo.email", "https://www.googleapis.com/auth/userinfo.profile"]
          }
        ]

使用可能なスコープは、各 SaaS プロバイダーによって定義され、プロバイダーの開発者ポータルで確認できます。

## Dropbox を呼び出すコードの追加

1. インストール、 [DropboxRestAPI](https://www.nuget.org/packages/DropboxRestAPI) SimpleDropbox プロジェクトに NuGet パッケージ。

    *  **ツール** ] メニューをクリックして **NuGet パッケージ マネージャー > パッケージ マネージャー コンソール**します。

    *  **パッケージ マネージャー コンソール** ウィンドウで、このコマンドを入力します。
     
            install-package DropboxRestAPI  

1. 開いている *controllers \valuescontroller.cs* 、すべてのファイルにコードを次のコードに置き換えます。

        using DropboxRestAPI;
        using Microsoft.Azure.AppService.ApiApps.Service;
        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Threading.Tasks;
        using System.Web.Http;
        
        namespace SimpleDropBox2.Controllers
        {
            public class ValuesController : ApiController
            {
                public async Task<IEnumerable<string>> Get()
                {
                    // Retrieve the token from the gateway
                    var runtime = Runtime.FromAppSettings(Request);
                    var dropboxTokenResult = await runtime.CurrentUser.GetRawTokenAsync("dropbox");
        
                    // Create a Dropbox client object that will send the token
                    // with REST API calls to Dropbox.
                    var dropboxClient = new Client(
                        new Options
                        {
                            AccessToken = dropboxTokenResult.Properties["AccessToken"]
                        }
                    );
        
                    // Call the Dropbox API
                    var metadata = await dropboxClient.Core.Metadata.MetadataAsync("/");
        
                    // Return a list of files and folders.
                    return metadata.contents.Select(md => md.path);
                }
            }
        }

    クライアントがこのメソッドを呼び出す前に、ユーザーが Dropbox にログインしていて、API アプリからユーザーの Dropbox アカウントへのアクセスに同意している必要があります。 App Service ゲートウェイへのアクセス トークンを提供することで、Dropbox がその同意を確認します。 このコードによりゲートウェイからトークンを取得し、次の図の手順 6. と 7. に示すように、Dropbox API への呼び出しにこのトークンを含めます。

    ![](./media/app-service-api-dotnet-connect-to-saas/saastoken.png)

2. プロジェクトをビルドします。

## Azure での API アプリの作成

このセクションでは、Visual Studio を使用 **Web の発行** Azure で API アプリを作成するウィザード。 手順では、API アプリの名前を入力するように指示、」と入力 *SimpleDropbox*します。

[AZURE.INCLUDE [app-service-api-pub-web-create](../../includes/app-service-api-pub-web-create.md)]

## コードのデプロイ

使用すると、同じ **Web の発行** ウィザード、新しい API アプリにコードをデプロイします。

[AZURE.INCLUDE [app-service-api-pub-web-deploy](../../includes/app-service-api-pub-web-deploy.md)]

## 認証の着信呼び出しの構成

Azure App Service で API アプリからの認証済みの発信呼び出しを許可するには、API アプリで認証済みユーザーからの着信呼び出しを取得する必要があります。 これは OAuth 2.0 の一般的な要件ではありませんが、App Service ゲートウェイ アーキテクチャが現在実装されているため、その要件になります。

このセクションのスクリーンショットでは、ContactsList API アプリが表示されていますが、このチュートリアルで作成する SimpleDropbox API アプリでも、そのプロセスは同じです。

### 着信呼び出しの認証を要求するように、API アプリを構成します。

[AZURE.INCLUDE [app-service-api-config-auth](../../includes/app-service-api-config-auth.md)]

### ゲートウェイの ID プロバイダーの構成

[AZURE.INCLUDE [app-service-api-gateway-config-auth](../../includes/app-service-api-gateway-config-auth.md)]

## 発信呼び出しの認証の構成

API アプリから Dropbox API を呼び出すことができるようにするには、Dropbox の開発者向けサイトで作成する Dropbox アプリと API アプリの間で設定を交換する必要があります。

### Dropbox.com サイトでの Dropbox アプリの作成

[AZURE.INCLUDE [app-service-api-create-dropbox-app](../../includes/app-service-api-create-dropbox-app.md)]

### Dropbox と API アプリの間での設定の交換

次の手順は、Dropbox コネクタ API アプリに関するものですが、このチュートリアルで作成する SimpleDropbox API アプリでも、その手順と UI は同じです。

> **注:** SimpleDropbox API アプリの [Dropbox クライアント ID とクライアント シークレットのフィールドが表示されない場合 **認証** ブレードのスクリーン ショットで示すように必ず API アプリ プロジェクトを API アプリをデプロイした後、指示に従ってゲートウェイを再起動することです。 ある値"dropbox"、 `authentication` のプロパティ、 *apiapp.json* 先ほどデプロイしたファイルはどのようなポータルを起動してこれらのフィールドを表示します。

[AZURE.INCLUDE [app-service-api-exchange-dropbox-settings](../../includes/app-service-api-exchange-dropbox-settings.md)]

## テスト クライアントの作成

このセクションでは、Visual Studio によって生成されたクライアント コードを使用して SimpleDropbox API アプリを呼び出すコンソール アプリ プロジェクトを作成します。 このコンソール アプリは、Windows フォームのブラウザー コントロールをインスタンス化して、ユーザーによるゲートウェイや Dropbox ログイン Web ページの操作を処理します。

### プロジェクトを作成する

1. Visual Studio で、コンソール アプリケーション プロジェクトを作成し、名前を付けます *SimpleDropboxTest*します。

2. System.Windows.Forms への参照を設定します。
 
    *  **ソリューション エクスプ ローラー**, を右クリックして **参照**, 、クリックして **参照の追加**します。

    * 左にあるチェック ボックスをオンに **System.Windows.Forms**, 、順にクリック **OK**します。
     
    ![](./media/app-service-api-dotnet-connect-to-saas/setref.png)

    コンソール アプリケーションでは、Windows フォームのアセンブリを使用して、ユーザーがゲートウェイと Dropbox にログインできるようにする必要があるときに、ブラウザー コントロールをインスタンス化します。

### 生成されたクライアント コードの追加

このセクションのスクリーンショットには、ContactsList プロジェクトと API アプリが表示されていますが、このチュートリアルでは、SimpleDropboxTest プロジェクトと SimpleDropbox API アプリを選択します。

[AZURE.INCLUDE [app-service-api-dotnet-add-generated-client](../../includes/app-service-api-dotnet-add-generated-client.md)]

### API アプリを呼び出すコードの追加

3. 開いている *Program.cs* 内のコードを次のコードに置き換えます。
        
        using Microsoft.Azure.AppService;
        using Newtonsoft.Json;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Windows.Forms;
        
        namespace SimpleDropboxTest
        {
            enum Step
            {
                GatewayLogin,
                GetSaaSConsentLink,
                GetUserConsent
            }
        
            class Program
            {
                private const string GATEWAY_URL = @"{gateway url}";
                private const string URL_TOKEN = "#token=";
                private const string SAAS_URL = "dropbox.com";
                private static Form frm = new Form();
                private static string responseURL = "";
                private static Step step;
                [STAThread]
                static void Main(string[] args)
                {
                    // Create the web browser control
                    WebBrowser browser = new WebBrowser();
                    browser.Dock = DockStyle.Fill;
                    browser.Navigated += CheckResponseURL;
                    frm.Controls.Add(browser);
                    frm.Width = 640;
                    frm.Height = 480;
        
                    // Create the gateway and API app clients.
                    AppServiceClient appServiceClient = new AppServiceClient(GATEWAY_URL);
                    SimpleDropbox simpleDropboxClient = appServiceClient.CreateSimpleDropbox();
        
                    // Navigate browser to gateway login URL for configured identity provider.
                    // Identity provider for this example is Azure Active Directory.
                    step = Step.GatewayLogin;
                    browser.Navigate(string.Format(@"{0}/login/aad", GATEWAY_URL));
                    frm.ShowDialog();
                    Console.WriteLine("Logged in to gateway, response URL=" + responseURL);
        
                    // Get user ID and Zumo token from return URL, then call 
                    // the gateway URL to log in the gateway client.
                    var encodedJson = responseURL.Substring(responseURL.IndexOf(URL_TOKEN) + URL_TOKEN.Length);
                    var decodedJson = Uri.UnescapeDataString(encodedJson);
                    var result = JsonConvert.DeserializeObject<dynamic>(decodedJson);
                    string userId = result.user.userId;
                    string userToken = result.authenticationToken;
                    appServiceClient.SetCurrentUser(userId, userToken);
        
                    // Call gateway API to get consent link URL for target SaaS platform.
                    // SaaS platform for this example is Dropbox.
                    // See the tutorial for an explanation of
                    // the redirectURL parameter for GetConsentLinkAsync
                    var gatewayConsentLink = appServiceClient.GetConsentLinkAsync("SimpleDropbox", GATEWAY_URL).Result;
                    Console.WriteLine("\nGot gateway consent link, URL=" + gatewayConsentLink);
        
                    // Navigate browser to consent link URL returned from gateway.
                    // Response URL will be the SaaS logon link
                    step = Step.GetSaaSConsentLink;
                    browser.Navigate(gatewayConsentLink);
                    frm.ShowDialog();
                    Console.WriteLine("\nGot SaaS consent link response, URL=" + responseURL);
        
                    // Navigate browser to login/consent link for SaaS platform.
                    step = Step.GetUserConsent;
                    browser.Navigate(responseURL);
                    frm.ShowDialog();
                    Console.WriteLine("\nGot Dropbox login response, URL=" + responseURL);
        
                    Console.WriteLine("\nResponse from SaaS Provider");
                    var response = simpleDropboxClient.Values.Get();
                    foreach (string s in response)
                    {
                        Console.WriteLine(s);
                    }
                    Console.Read();
                }
        
                static void CheckResponseURL(object sender, WebBrowserNavigatedEventArgs e)
                {
                    if ((step == Step.GatewayLogin && e.Url.AbsoluteUri.IndexOf(URL_TOKEN) > -1)
                        || (step == Step.GetSaaSConsentLink && e.Url.AbsoluteUri.IndexOf(SAAS_URL) > -1)
                        || (step == Step.GetUserConsent && e.Url.AbsoluteUri.IndexOf(GATEWAY_URL) > -1))
                    {
                        responseURL = e.Url.AbsoluteUri;
                        frm.Close();
                    }
                }
        
            }
        }

1. {gateway url} の部分をゲートウェイの実際の URL に置き換えます。
 
    ゲートウェイの URL を取得できます、 **ゲートウェイ** ポータルのブレードで。

    ![](./media/app-service-api-dotnet-connect-to-saas/gwurl.png)

        private const string GATEWAY_URL = @"https://sd1aeb4ae60b7cb4f3d966dfa43b660.azurewebsites.net";

    > **重要な**: URL で始まっていることを確認 `https://`, ではなく、 `http://`です。 **ポータルから http:// をコピーする場合は、コードに貼り付けるときに https:// に変更する必要があります。**

### コードの説明

このコンソール アプリケーションは、最小限のコードを使用して、クライアント アプリが実行する手順を説明するように設計されています。 運用環境のアプリケーションは、通常はコンソール アプリケーションでないため、エラー処理とログ記録を実装します。

コードの実行内容を次に簡単に示します。

* ブラウザーで、ID プロバイダー (この場合は Azure Active Directory) で構成されたゲートウェイのログイン URL を開きます。 
     
* ハンドルは、ユーザーがログインした後に応答の URL を想定します。 ユーザー ID と Zumo トークンを抽出し、App Service クライアント オブジェクトを提供してください。 

* App Service クライアント オブジェクトを使用して、ログインと同意の Dropbox へのリンクにリダイレクトされるゲートウェイの URL を取得することができます。 図の手順 1. です。

* ブラウザーでゲートウェイの同意 URL を開きます。 ブラウザーが Dropbox のログインと同意のリンクにリダイレクトされます。 図の手順 2. です。 
     
* ユーザーが Dropbox.com にログインして同意した後、ブラウザーを閉じます。 図の手順 3. です。 
 
* API アプリを呼び出します。 図の手順 5. です  (手順 4. は Dropbox.com とゲートウェイの間でバックグラウンドで実行され、手順 6. と 7. はクライアントではなく API アプリから実行されます)。

![](./media/app-service-api-dotnet-connect-to-saas/saastoken.png)

その他のメモ:

* `Main` メソッドの `STAThread` 属性は、Web ブラウザー コントロールに必要ですが、API アプリの設定または呼び出しには関係ありません。

* 表示されるゲートウェイ ログイン URL の末尾は、Azure Active Directory を表す `/aad` になります。

        browser.Navigate(string.Format(@"{0}/login/aad", GATEWAY_URL));

    他のプロバイダーを使用する値を次に示します。
    * "microsoftaccount"
    * "facebook"
    * "twitter"
    * "google"
<br/><br/>

* `GetConsentLinkAsync()` メソッドの 2 番目のパラメーターは、ユーザーが Dropbox にログインしてユーザー アカウントへのアクセスに同意した後に、同意サーバーによりリダイレクトされるコールバック URL です。 

        var gatewayConsentLink = appServiceClient.GetConsentLinkAsync("SimpleDropbox", GATEWAY_URL).Result;

    このパラメーターには、通常はユーザーがクライアント アプリケーション内で移動する次の Web ページを指定します。 このデモ コードはコンソール アプリ内にあるため、移動先のアプリケーション ページはなく、このコードにより、ゲートウェイの URL が便利なランディング ページとして指定されます。 

    クライアント アプリケーションでは、この URL にリダイレクトされることと、エラー メッセージがないことを検証する必要があります。 ログインと同意のプロセスが失敗した場合は、リダイレクト URL にクエリ文字列のエラー メッセージが含まれている可能性があります。 詳細については、次を参照してください。、 [トラブルシューティング](#troubleshooting) セクションです。 

## テスト

1. SimpleDropboxTest コンソール アプリケーションを実行します。

2. 最初のログオン ページで、Azure Active Directory の資格情報 (Google や Twitter など、別の ID プロバイダーをゲートウェイで構成している場合は、それらの資格情報) を使用してサインインします。

    ![](./media/app-service-api-dotnet-connect-to-saas/aadlogon.png)

3. Dropbox.com のログイン ページで、Dropbox の資格情報を使用してサインインします。

    ![](./media/app-service-api-dotnet-connect-to-saas/dblogon.png)

4. Dropbox の同意ページで、アプリケーションにデータへのアクセス許可を付与します。

    ![](./media/app-service-api-dotnet-connect-to-saas/dbconsent.png)

    次に、コンソール アプリから API アプリを呼び出し、Dropbox アカウント内のファイルの一覧を返します。

    ![](./media/app-service-api-dotnet-connect-to-saas/testclient.png)

## トラブルシューティング

このセクションは、次のトピックで構成されています。

* [ゲートウェイのログイン後の HTTP エラー 405](#405)
* [Dropbox のログイン ページの代わりに表示される HTTP エラー 400](#400)
* [API アプリの呼び出し中に発生する HTTP エラー 403](#403)

### <a id="405"></a> ゲートウェイのログイン後の HTTP エラー 405

コードから GetConsentLinkAsync を呼び出したときに HTTP エラー 405 が発生する場合は、ゲートウェイ URL に http:// ではなく https:// を使用していることを確認します。

![](./media/app-service-api-dotnet-connect-to-saas/http405.png)

エラーを許可されていない 405 メソッドが表示される、クライアントが SSL 以外の HTTP POST 要求を作成しようとした場合、ゲートウェイにリダイレクト *https://*, 、させ、GET 要求をリダイレクトします。 同意のリンクを取得するための URL で受け取ることができるのは、POST 要求のみです。

### <a id="400"></a>Dropbox ログイン ページの代わりに表示される HTTP エラー 400

適切であるかどうかを確認 **クライアント ID** で API アプリの **認証** ブレードで、先頭または末尾にスペースがないかどうかを確認します。 

### <a id="403"></a> API アプリの呼び出し中に発生する HTTP エラー 403

* 確認して、 **アクセス レベル** に設定されているアプリ、API の **パブリック (認証済み)**, ではなく、 **内部**します。

* 適切であるかどうかを確認 **クライアント シークレット** で API アプリの **認証** ブレードで、先頭または末尾にスペースがないかどうかを確認します。

Dropbox ログイン後のリダイレクト URL は、次の例のようになります。

    https://sd1aeb4ae60b7cb4f3d966dfa43b6607f30.azurewebsites.net/?error=RmFpbGVkIHRvIGV4Y2hhbmdlIGNvZGUgZm9yIHRva2VuLiBEZXRhaWxzOiB7ImVycm9yX2Rlc2NyaXB0aW9uIjogIkludmFsaWQgY2xpZW50X2lkIG9yIGNsaWVudF9zZWNyZXQiLCAiZXJyb3IiOiAiaW52YWxpZF9jbGllbnQifQ%3d%3d

クエリ文字列の値 `error` の末尾から %3d%3d を削除すると、これが有効な base64 のエンコード文字列になります。 この文字列をデコードすると、次のようなエラー メッセージが表示されます。

    Failed to exchange code for token. Details: {"error_description": "Invalid client_id or client_secret", "error": "invalid_client"}

## 次のステップ

SaaS プラットフォームに接続する API アプリのコード記述と構成の方法について説明しました。  API アプリでの認証を処理する方法に関するその他のチュートリアルへのリンクを参照してください。 [認証 API apps と mobile apps の次のステップ](../app-service/app-service-authentication-overview.md#next-steps)します。 

[Azure preview portal]: https://portal.azure.com/
[Azure portal]: https://manage.windowsazure.com/


