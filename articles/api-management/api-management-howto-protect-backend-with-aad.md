<properties
    pageTitle="Azure Active Directory と API Management で Web API バックエンドを保護する方法"
    description="Azure Active Directory と API Management で Web API バックエンドを保護する方法について説明します。" 
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

# Azure Active Directory と API Management で Web API バックエンドを保護する方法

次のビデオでは、Web API バックエンドをビルドし、Azure Active Directory と API Management で OAuth 2.0 プロトコルを使用して保護する方法を示します。  この記事では、ビデオで説明する手順の概要と追加情報を紹介します。 この 24 分間のビデオでは、次の手順について説明しています。

-   Web API バックエンドをビルドし、AAD で保護する - 1:30 開始
-   API Management に API をインポートする - 7:10 開始
-   API を呼び出す開発者ポータルを構成する - 9:09 開始
-   API を呼び出すデスクトップ アプリケーションを構成する - 18:08 開始
-   要求を事前承認する JWT 検証ポリシーを構成する - 20:47 開始

>[AZURE.VIDEO protecting-web-api-backend-with-azure-active-directory-and-api-management]

## Azure AD Directory を作成する

Azure Active Directory を使用して Web API バックエンドを保護するには、AAD テナントが必要です。 このビデオという名前のテナントで **APIMDemo** を使用します。 AAD テナントを作成するにサインインする、 [Azure Classic Portal](https://manage.windowsazure.com) ] をクリック **新規**]-> [**App Services**]-> [**Active Directory**]-> [**ディレクトリ**]-> [**カスタム作成**します。 

![Azure Active Directory][api-management-create-aad-menu]

この例のという名前のディレクトリで **APIMDemo** という名前の既定ドメインで作成された **DemoAPIM.onmicrosoft.com**します。 このディレクトリはビデオ全体で使用されます。

![Azure Active Directory][api-management-create-aad]

## Azure Active Directory で保護された Web API サービスを作成する

この手順では、Web API バックエンドは Visual Studio 2013 を使用して作成されます。 この手順はビデオの 1:30 から開始されます。 Visual Studio のクリックで Web API バックエンド プロジェクトを作成する **ファイル**]-> [**新規**]-> [**プロジェクト**, 、して **ASP.NET Web アプリケーション** から、 **Web** テンプレートの一覧です。 このビデオでは、プロジェクトの名前が **APIMAADDemo**します。 クリックして **OK** プロジェクトを作成します。 

![Visual Studio][api-management-new-web-app]

をクリックして **Web API** から、 **、テンプレートの一覧で選択** Web API プロジェクトを作成します。 Azure Directory の認証] をクリックを構成する **認証の変更**します。

![新しいプロジェクト][api-management-new-project]

クリックして **組織アカウント**, を指定し、 **ドメイン** AAD テナントのです。 この例では、ドメインは **DemoAPIM.onmicrosoft.com**します。 ディレクトリのドメインを取得できます、 **ドメイン** ディレクトリのタブをクリックします。

![ドメイン][api-management-aad-domains]

必要な設定を構成、 **認証の変更** ] ダイアログ ボックスをクリック **OK**します。

![認証の変更][api-management-change-authentication]

クリックすると、 **OK** Visual Studio は、Azure AD ディレクトリにアプリケーションを登録しようし、Visual Studio にサインインするように求められますことがあります。 ディレクトリの管理者アカウントを使用してサインインします。

![Visual Studio にサインインする][api-management-sign-in-vidual-studio]

構成するのにはこのプロジェクトを Azure Web API としてチェック ボックスをオン **クラウドでホスト** ] をクリックし、 **OK**します。

![新しいプロジェクト][api-management-new-project-cloud]

Azure へのサインインを求めるメッセージが表示され、Web アプリを構成できるようになります。

![構成][api-management-configure-web-app]

この例で、新しい **App Service プラン** という名前 **APIMAADDemo** を指定します。

クリックして **OK** Web アプリを構成し、プロジェクトを作成します。

## Web API プロジェクトにコードを追加します。

ビデオの次の手順では、Web API プロジェクトにコードを追加します。 この手順は 4 35 から開始されます。

この例の Web API は、モデルとコントローラーを使用して基本的な電卓サービスを実装します。 右クリックし、サービスのモデルを追加する **モデル** で **ソリューション エクスプ ローラー** 選択 **追加**, 、**クラス**します。 クラスに名前を `CalcInput` ] をクリック **追加**します。

`CalcInput.cs` ファイルの先頭に次の `using` ステートメントを追加します。

    using Newtonsoft.Json;

 生成されたクラスを次のコードで置き換えます。

    public class CalcInput
    {
        [JsonProperty(PropertyName = "a")]
        public int a;

        [JsonProperty(PropertyName = "b")]
        public int b;
    }

右クリック **コント ローラー** で **ソリューション エクスプ ローラー** 選択 **追加**]-> [**コント ローラー**します。 選択 **Web API 2 コント ローラー - 空** ] をクリック **追加**します。 型 **CalcController** コント ローラーの名前を指定し、クリックして **追加**します。

![コントローラーの追加][api-management-add-controller]

`CalcController.cs` ファイルの先頭に次の `using` ステートメントを追加します。

    using System.IO;
    using System.Web;
    using APIMAADDemo.Models;

生成されたコントローラー クラスを次のコードで置き換えます。 このコードは、Basic Calculator API の操作である `Add`、`Subtract`、`Multiply`、`Divide` を実装します。

    [Authorize]
    public class CalcController : ApiController
    {
        [Route("api/add")]
        [HttpGet]
        public HttpResponseMessage GetSum([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a + b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/sub")]
        [HttpGet]
        public HttpResponseMessage GetDiff([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a - b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/mul")]
        [HttpGet]
        public HttpResponseMessage GetProduct([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a * b);
            HttpResponseMessage response = Request.CreateResponse();
            response.Content = new StringContent(xml, System.Text.Encoding.UTF8, "application/xml");
            return response;
        }

        [Route("api/div")]
        [HttpGet]
        public HttpResponseMessage GetDiv([FromUri]int a, [FromUri]int b)
        {
            string xml = string.Format("<result><value>{0}</value><broughtToYouBy>Azure API Management - http://azure.microsoft.com/apim/ </broughtToYouBy></result>", a / b);
HttpResponseMessage 応答 = Request.CreateResponse() です。
応答です。コンテンツ = 新しい StringContent (xml、System.Text.Encoding.UTF8、"アプリケーションまたは xml") です。
応答を返す
}
}


キーを押して **F6** をビルドし、ソリューションを確認します。

## Azure にプロジェクトを発行する

この手順では、Visual Studio プロジェクトを Azure に発行します。 この手順はビデオの 5:45 から開始されます。

でプロジェクトを Azure に発行するを右クリックし、 **APIMAADDemo** Visual Studio でプロジェクト **発行**します。 既定の設定のまま、 **Web の発行** ] ダイアログ ボックスをクリック **発行**します。

![Web の発行][api-management-web-publish]

## Azure AD バックエンド サービス アプリケーションにアクセス許可を付与する

Web API プロジェクトの構成と公開プロセスの一環として、バックエンド サービス用の新しいアプリケーションが Azure AD Directory に作成されます。 ビデオの 6:13 から始まるビデオの手順では、Web API バックエンドにアクセス許可が付与されます。

![アプリケーション][api-management-aad-backend-app]

必要なアクセス許可を構成するアプリケーションの名前をクリックします。 移動し、 **構成** ] タブでまで下へスクロールし、 **他のアプリケーションに対するアクセス許可** セクションです。 クリックして、 **アプリケーションのアクセス許可** の横にあるドロップダウン **Windows** **Azure Active Directory**, 、チェック ボックスをオン **ディレクトリ データを読み取る**, 、] をクリック **保存**します。

![Add permissions][api-management-aad-add-permissions]

>[AZURE.NOTE] 場合 **Windows** **Azure Active Directory** をクリックして [その他のアプリケーションのアクセス許可] 一覧にない **アプリケーションの追加** し、一覧から追加します。

書き留めて、 **App Id URI** API Management 開発者ポータルの Azure AD アプリケーションが構成されている場合は、後の手順で使用するためです。

![アプリ ID URI][api-management-aad-sso-uri]

## API Management に Web API をインポートする

API の構成は、Azure クラシック ポータルから API 発行者ポータルにアクセスして行います。 クリックして、パブリッシャー ポータルにアクセス **管理** 、API Management サービスの Azure クラシック ポータルで。 API Management サービス インスタンスを作成がない場合は、次を参照してください。 [API Management サービス インスタンスの作成][] で、 [最初の Api][] チュートリアルです。

![パブリッシャー ポータル][api-management-management-console]

操作は、 [Api に手動で追加](api-management-howto-add-operations.md), 、またはインポート可能です。 このビデオでは、操作は 6:40 から始まる Swagger 形式でインポートされます。

次の内容のファイルを `calcapi.json` という名前で作成し、コンピューターに保存します。 `host` 属性がお使いの Web API バックエンドを指定していることを確認します。 この例では、`"host": "apimaaddemo.azurewebsites.net"` が使用されます。

{
"swagger": "2.0",
"info": {
「タイトル」:"Calculator"
「説明」:"Arithmetics HTTP 経由で!"、
「バージョン」:「1.0」
},
「ホスト」:"apimaaddemo.azurewebsites.net"
"basePath":"/api"、
"schemes": [
"http"
],
"paths": {
"/add?、{、} = & b = {b}": {
          "get": {
            「説明」:「で応答の 2 つの数値の合計です」、。
            "operationId":「2 つの整数を追加する」
            "parameters": [
              {
                "name":"a"、
                「に」:"query"
                「説明」:"最初のオペランド。 既定値は <code>51</code>.",
                "required": true,
                "default":「51」
                「列挙」: [
                  "51"
                ]
              },
              {
                "name":"b",
                「に」:"query"
                「説明」:"2 番目のオペランド。 既定値は <code>49</code>.",
                "required": true,
                "default":「49」
                「列挙」: [
                  "49"
                ]
              }
            ],
            「応答」: {}
          }
        },
        "/sub?、{、} = & b = {b}": {
          "get": {
            「説明」:「で応答 2 つの数値の差」、。
            "operationId":「2 つの整数を減算」
            "parameters": [
              {
                "name":"a"、
                「に」:"query"
                「説明」:"最初のオペランド。 既定値は <code>100</code>.",
                "required": true,
                "default":「100」
                「列挙」: [
                  "100"
                ]
              },
              {
                "name":"b",
                「に」:"query"
                「説明」:"2 番目のオペランド。 既定値は <code>50</code>.",
                "required": true,
                "default":「50」
                「列挙」: [
                  "50"
                ]
              }
            ],
            「応答」: {}
          }
        },
        "/div?、{、} = & b = {b}": {
          "get": {
            「説明」:「で応答の 2 つの数値の商」、。
            "operationId":「2 つの整数を除算」
            "parameters": [
              {
                "name":"a"、
                「に」:"query"
                「説明」:"最初のオペランド。 既定値は <code>100</code>.",
                "required": true,
                "default":「100」
                「列挙」: [
                  "100"
                ]
              },
              {
                "name":"b",
                「に」:"query"
                「説明」:"2 番目のオペランド。 既定値は <code>20</code>.",
                "required": true,
                "default":「20」
                「列挙」: [
                  "20"
                ]
              }
            ],
            「応答」: {}
          }
        },
        "/mul?、{、} = & b = {b}": {
          "get": {
            「説明」:「で応答の 2 つの数値の積です」、。
            "operationId":「2 つの整数を乗算」
            "parameters": [
              {
                "name":"a"、
                「に」:"query"
                「説明」:"最初のオペランド。 既定値は <code>20</code>.",
                "required": true,
                "default":「20」
                「列挙」: [
                  "20"
                ]
              },
              {
                "name":"b",
                「に」:"query"
                「説明」:"2 番目のオペランド。 既定値は <code>5</code>.",
                "required": true,
                "default":「5」
                「列挙」: [
                  "5"
                ]
              }
            ],
            「応答」: {}
          }
        {
      {
    }

電卓 API をインポートする] をクリックして **Api** から、 **API Management** 、左側のメニューをクリックし、 **API のインポート**します。

![API ボタンのインポート][api-management-import-api]

電卓 API を構成するには、次の手順を実行します。

1. クリックして **ファイルから**, を参照、 `calculator.json` ファイルを保存して、をクリックして、 **Swagger** オプション ボタンをクリックします。
2. 型 **calc** に、 **Web API URL サフィックス** ] ボックスに貼り付けます。
3. 内のクリックして、 **製品 (オプション)** ] を選択 **スターター**します。
4. クリックして **保存** 、API をインポートします。

![新しい API を追加する][api-management-import-new-api]

API がインポートされると、API の概要ページがパブリッシャー ポータルに表示されます。

## 開発者ポータルから API の呼び出しに失敗する場合

この時点で API は API Management にインポートされましたが、バックエンド サービスが Azure AD 認証で保護されているため、まだ開発者ポータルから正常に呼び出すことはできません。 これについては次の手順を使用して説明します。ビデオの 7:40 から始まります。

クリックして **デベロッパー ポータル** パブリッシャー ポータルの右上側からです。

![開発者ポータル][api-management-developer-portal-menu]

クリックして **Api** ] をクリックし、 **電卓** API です。

![開発者ポータル][api-management-dev-portal-apis]

クリックして **やって**します。

![試してみる][api-management-dev-portal-try-it]

をクリックして **送信** の応答のステータスに注意してくださいと **401 Unauthorized**します。

![送信][api-management-dev-portal-send-401]

バックエンド API が Azure Active Directory によって保護されているため、要求は承認されていません。 API を正常に呼び出すには、OAuth 2.0 を使用して開発者を承認するよう、開発者ポータルを構成する必要があります。 このプロセスについては以降のセクションで説明します。

## 開発者ポータルを AAD アプリケーションとして登録する

OAuth 2.0 を使用して開発者を承認するように開発者ポータルを構成する最初の手順で、開発者ポータルを AAD アプリケーションとして登録します。 この説明については、ビデオでは 8:27 から始まります。

この例では、このビデオの最初の手順から Azure AD テナントに移動 **APIMDemo** に移動し、 **アプリケーション** ] タブをクリックします。

![新規アプリケーション][api-management-aad-new-application-devportal]

クリックして、 **追加** 新しい Azure Active Directory アプリケーションを作成するボタンをクリックし、選択 **[組織が開発中のアプリケーションを追加**します。

![新規アプリケーション][api-management-new-aad-application-menu]

選択 **Web アプリケーションや Web API**, 、名前を入力し、次へ進む矢印をクリックします。 この例では **APIMDeveloperPortal** を使用します。

![新規アプリケーション][api-management-aad-new-application-devportal-1]

 **サインオン URL** 、API Management サービスの URL を入力し、追加 `/signin`します。 この例では * * https://contoso5.portal.azure-api.net/signin * * を使用します。

 **アプリ Id URL** 、API Management サービスの URL を入力し、いくつか一意の文字を追加します。 必要な文字は、この例では **https://contoso5.portal.azure-api.net/dp** を使用します。 ときに、必要な **アプリのプロパティ** はアプリケーションを作成するチェック マークをクリックして構成します。

![新規アプリケーション][api-management-aad-new-application-devportal-2]

## API Management OAuth 2.0 承認サーバーを構成する

次の手順では、API Management に OAuth 2.0 承認サーバーを構成します。 この手順の説明については、ビデオの 9:43 から始まります。

クリックして **セキュリティ** 左側の [API Management] メニューからをクリックして **OAuth 2.0**, 、] をクリックし、 **承認を追加** サーバーです。

![承認サーバーの追加][api-management-add-authorization-server]

名前とオプションの説明を入力、 **名前** と **説明** フィールドです。 これらのフィールドは、OAuth 2.0 承認サーバーを API Management サービス インスタンス内で識別するために使用されます。 この例では **承認サーバーのデモ** を使用します。 後で OAuth 2.0 サーバーを指定して API の認証に使用するときに、この名前を選択します。

 **クライアント登録ページ URL** プレース ホルダーの値を入力します。 `http://localhost`します。   **クライアント登録ページ URL** を作成し、自分のアカウントを構成するユーザー アカウントの管理をサポートする OAuth 2.0 プロバイダーのユーザーが使用できるページへのポインターします。 この例では、ユーザーは自身のアカウントを作成も構成もしないため、プレースホルダーを使用します。

![承認サーバーの追加][api-management-add-authorization-server-1]

次に、指定 **Authorization endpoint URL** と **トークン エンドポイント URL**します。

![承認サーバー][api-management-add-authorization-server-1a]

これらの値を取得できる、 **アプリのエンドポイント** デベロッパー ポータル用に作成した AAD アプリケーションのページです。 移動するエンドポイントにアクセスする、 **構成** AAD アプリケーションのタブをクリックして **エンドポイントを表示**します。

![アプリケーション][api-management-aad-devportal-application]

![エンドポイントの表示][api-management-aad-view-endpoints]

コピー、 **OAuth 2.0 承認エンドポイント** 貼り付けます、 **Authorization endpoint URL** ] ボックスに貼り付けます。

![承認サーバーの追加][api-management-add-authorization-server-2]

コピー、 **OAuth 2.0 トークン エンドポイント** 貼り付けます、 **トークン エンドポイント URL** ] ボックスに貼り付けます。

![承認サーバーの追加][api-management-add-authorization-server-2a]

トークン エンドポイントに貼り付ける、だけでなくという名前の追加の本文パラメーターを追加 **リソース** と値を使用するため、 **App Id URI** Visual Studio プロジェクトの発行時に作成されたバックエンド サービスの AAD アプリケーションからです。

![アプリ ID URI][api-management-aad-sso-uri]

次に、クライアントの資格情報を指定します。 これは、アクセスする必要があるリソースの資格情報です (この場合はバックエンド サービス)。

![クライアントの資格情報][api-management-client-credentials]

取得する、 **クライアント Id**, に移動し、 **構成** のバックエンド サービスおよびコピーの AAD アプリケーション] タブ、 **クライアント Id**します。

取得する、 **クライアント シークレット** ] をクリックして、 **の期間を選択** ボックスの一覧、 **キー** セクションし、間隔を指定します。 この例では 1 年間を使用します。

![クライアント ID][api-management-aad-client-id]

クリックして **保存** を構成を保存し、キーを表示します。 

>[AZURE.IMPORTANT] このキーをメモしてをおきます。 Azure Active Directory の構成ウィンドウを閉じると、キーは再表示できません。

キーをクリップボードにコピー、パブリッシャー ポータルに戻りにキーを貼り付けます、 **クライアント シークレット** ] ボックスに、をクリックして **保存**します。

![承認サーバーの追加][api-management-add-authorization-server-3]

クライアント資格情報の直後にあるのは、認証コード付与です。 この認証コードとスイッチを Azure AD 開発者ポータル アプリケーションにコピーの構成] ページ、およびに認証付与を貼り付けて、 **応答 URL** ] をクリックし、 **保存** 再度します。

![応答 URL][api-management-aad-reply-url]

次の手順では、開発者ポータル AAD アプリケーションのアクセス許可を構成します。 クリックして **アプリケーションのアクセス許可** チェック ボックスをオンおよび **ディレクトリ データを読み取る**します。 をクリックして **保存** この変更を保存し **アプリケーションに追加**します。

![Add permissions][api-management-add-devportal-permissions]

検索アイコンをクリックして型 **APIM** に始まる] ボックスで選択 **APIMAADDemo**, 、保存するチェック マークをクリックします。

![Add permissions][api-management-aad-add-app-permissions]

をクリックして **委任されたアクセス許可** の **APIMAADDemo** チェック ボックスをオンおよび **Access APIMAADDemo**, 、] をクリック **保存**します。 これにより、開発者ポータル アプリケーションからバックエンド サービスにアクセスできるようになります。

![Add permissions][api-management-aad-add-delegated-permissions]

## 電卓 API 用に OAuth 2.0 のユーザー認証を有効にする

OAuth 2.0 サーバーが構成されたので、API のセキュリティ設定で指定できるようになります。 この手順の説明については、ビデオの 14:30 から始まります。

クリックして **Api** 左側のメニューをクリック  **電卓** を表示してその設定を構成します。

![電卓 API][api-management-calc-api]

移動、 **セキュリティ** チェック] タブ、 **OAuth 2.0** ] チェック ボックスから必要な承認サーバーを選択して、 **承認サーバー** ボックスの一覧] をクリック **保存**します。

![電卓 API][api-management-enable-aad-calculator]

## 開発者ポータルから電卓 API を正常に呼び出す

OAuth 2.0 認証を API で構成したら、デベロッパー センターから操作を正常に呼び出すことができます。 この手順の説明については、ビデオの 15:00 から始まります。

戻り、 **2 つの整数を追加** 開発者ポータル] をクリック、電卓サービスの操作 **やって**します。 新しい項目に注意してください、 **承認** 追加した承認サーバーに対応するセクションです。

![電卓 API][api-management-calc-authorization-server]

選択 **認証コード** 承認ドロップダウン ボックスの一覧し、使用するアカウントの資格情報を入力します。 既にアカウントでサインインしている場合、確認メッセージは表示されません。

![電卓 API][api-management-devportal-authorization-code]

をクリックして **送信** に注意してくださいと、 **応答のステータス** の **200 OK** と応答の内容に、操作の結果。

![電卓 API][api-management-devportal-response]

## API を呼び出すデスクトップ アプリケーションを構成する

ビデオの 16:30 から始まる次の手順では、API を呼び出す単純なデスクトップ アプリケーションを構成します。 最初の手順では、デスクトップ アプリケーションを Azure AD に登録し、アクセス権をディレクトリとバックエンド サービスに付与します。 18:25 からは、電卓 API で操作を呼び出すデスクトップ アプリケーションのデモを紹介します。

## 要求を事前承認する JWT 検証ポリシーを構成する

最後の手順をビデオでは、20時 48分から開始し、使用する方法を示しています、 [JWT を検証する](https://msdn.microsoft.com/library/azure/034febe3-465f-4840-9fc6-c448ef520b0f#ValidateJWT) 受信要求ごとのアクセス トークンを検証することによって、要求を事前承認ポリシーをします。 JWT の検証ポリシーで要求が検証されない場合、要求は API Management によってブロックされ、バックエンドへは渡されません。

    <validate-jwt header-name="Authorization" failed-validation-httpcode="401" failed-validation-error-message="Unauthorized. Access token is missing or invalid.">
        <openid-config url="https://login.windows.net/DemoAPIM.onmicrosoft.com/.well-known/openid-configuration" />
        <required-claims>
            <claim name="aud">
                <value>https://DemoAPIM.NOTonmicrosoft.com/APIMAADDemo</value>
            </claim>
        </required-claims>
    </validate-jwt>

構成と、このポリシーを使用して別のデモについては、次を参照してください。 [クラウド カバー エピソード 177: API の管理機能より](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) を 13:50 まで早送りしています。 15:00 まで早送りし、ポリシー エディターで構成されるポリシーをご覧いただいた後、開発者ポータルから操作を呼び出す方法について、必要な認証トークンを使用した場合と使用しない場合の両方のデモを 18:50 からご覧ください。

## 次のステップ
-   チェック アウトの詳細 [ビデオ](https://azure.microsoft.com/documentation/videos/index/?services=api-management) API Management についてです。
-   バックエンド サービスを保護するには、他の方法を参照してください。 [相互証明書認証](api-management-howto-mutual-certificates.md) と [VPN または ExpressRoute 経由で接続](api-management-howto-setup-vpn)します。

[api-management-management-console]: ./media/api-management-howto-protect-backend-with-aad/api-management-management-console.png

[api-management-import-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-import-new-api.png
[api-management-create-aad-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad-menu.png
[api-management-create-aad]: ./media/api-management-howto-protect-backend-with-aad/api-management-create-aad.png
[api-management-new-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-web-app.png
[api-management-new-project]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project.png
[api-management-new-project-cloud]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-project-cloud.png
[api-management-change-authentication]: ./media/api-management-howto-protect-backend-with-aad/api-management-change-authentication.png
[api-management-sign-in-vidual-studio]: ./media/api-management-howto-protect-backend-with-aad/api-management-sign-in-vidual-studio.png
[api-management-configure-web-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-configure-web-app.png
[api-management-aad-domains]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-domains.png
[api-management-add-controller]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-controller.png
[api-management-web-publish]: ./media/api-management-howto-protect-backend-with-aad/api-management-web-publish.png
[api-management-aad-backend-app]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-backend-app.png
[api-management-aad-add-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-permissions.png
[api-management-developer-portal-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-developer-portal-menu.png
[api-management-dev-portal-apis]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-apis.png
[api-management-dev-portal-try-it]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-try-it.png
[api-management-dev-portal-send-401]: ./media/api-management-howto-protect-backend-with-aad/api-management-dev-portal-send-401.png
[api-management-aad-new-application-devportal]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal.png
[api-management-aad-new-application-devportal-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-1.png
[api-management-aad-new-application-devportal-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-new-application-devportal-2.png
[api-management-aad-devportal-application]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-devportal-application.png
[api-management-add-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server.png
[api-management-aad-sso-uri]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-sso-uri.png
[api-management-aad-view-endpoints]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-view-endpoints.png
[api-management-aad-client-id]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-client-id.png
[api-management-add-authorization-server-1]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1.png
[api-management-add-authorization-server-2]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2.png
[api-management-add-authorization-server-2a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-2a.png
[api-management-add-authorization-server-3]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-3.png
[api-management-aad-reply-url]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-reply-url.png
[api-management-add-devportal-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-devportal-permissions.png
[api-management-aad-add-app-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-app-permissions.png
[api-management-aad-add-delegated-permissions]: ./media/api-management-howto-protect-backend-with-aad/api-management-aad-add-delegated-permissions.png
[api-management-calc-api]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-api.png
[api-management-enable-aad-calculator]: ./media/api-management-howto-protect-backend-with-aad/api-management-enable-aad-calculator.png
[api-management-devportal-authorization-code]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-authorization-code.png
[api-management-devportal-response]: ./media/api-management-howto-protect-backend-with-aad/api-management-devportal-response.png
[api-management-calc-authorization-server]: ./media/api-management-howto-protect-backend-with-aad/api-management-calc-authorization-server.png
[api-management-add-authorization-server-1a]: ./media/api-management-howto-protect-backend-with-aad/api-management-add-authorization-server-1a.png
[api-management-client-credentials]: ./media/api-management-howto-protect-backend-with-aad/api-management-client-credentials.png
[api-management-new-aad-application-menu]: ./media/api-management-howto-protect-backend-with-aad/api-management-new-aad-application-menu.png

[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Manage your first API]: api-management-get-started.md

