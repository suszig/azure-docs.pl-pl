<properties 
    pageTitle="Logic Apps でのカスタム API の呼び出し" 
    description="App Service でホストされたカスタム API のロジック アプリでの使用" 
    authors="stepsic-microsoft-com" 
    manager="dwrede" 
    editor="" 
    services="app-service\logic" 
    documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    
    ms.topic="article"
    ms.date="10/07/2015"
    ms.author="stepsic"/>
    
# App Service でホストされたカスタム API のロジック アプリでの使用

ロジック アプリには、さまざまなサービス用に 40 以上のコネクタの豊富なセットがありますが、独自のカスタム API を呼び出して独自のコードを実行することもできます。 独自のカスタム Web API をホストする最も簡単かつスケーラブルな方法の 1 つは、App Service を使用することです。 この記事では、App Service の Web アプリでホストされる任意の Web API を呼び出す方法について説明します。

## Web アプリのデプロイ

最初に、App Service で Web アプリとして API をデプロイする必要があります。 ここに示す手順は、基本的な展開をカバー: [ASP.NET web アプリの作成](web-sites-dotnet-get-started.md)します。

取得する、 **URL** に表示される Web アプリでの **Essentials** Web アプリの上部にあります。

## API の呼び出し

まず、新しい空のロジック アプリを作成します。 作成された空のロジック アプリを作成したら、クリックして **編集** または **トリガーとアクション** を選択し、 **を最初から作成**します。

最初に、おそらくする定期的なトリガーを使用するかをクリックして、 **このロジックを手動で実行**します。 次に、実際に API の呼び出しを行います。 これを行うには、クリックして、緑 **HTTP** 右側にあるアクションです。

1. 選択、 **メソッド** -これは、API のコードで定義
2.  **URL** のセクションを貼り付けます、 **URL** 、展開済みの Web アプリ
3. 必要な場合は **ヘッダー** 次のような JSON 形式に含めます。 `{"Content-type" : "application/json", "Accept" : "application/json" }`
4. API はパブリックなかどうかにしておく **認証** 空白です。 API の呼び出しをセキュリティで保護する場合は、次のセクションを参照してください。
5. 最後に、含める、 **本文** API で定義されている質問のです。

クリックして **保存** コマンド バーにします。 クリックすると **を今すぐ実行** の呼び出し、API と実行の一覧に応答が表示されます。

パブリック API がある場合は、これが十分に機能しますが、API をセキュリティで保護する場合は、他にもいくつかの方法があります。

1. *コード変更は必要ありません* -して API を保護するすべてのコードの変更や再展開を必要とせずに Azure Active Directory を使用することができます。
2. API のコードで基本認証、AAD 認証、または証明書認証を適用します。 

## コード変更を伴わない API の呼び出しのセキュリティ保護 

このセクションでは、ロジック アプリ用と Web アプリ用の 2 つの Azure Active Directory アプリケーションを作成します。  Web アプリの呼び出しの認証は、ロジック アプリの AAD アプリケーションに関連付けられたサービス プリンシパル (クライアント ID とシークレット) を使用して行います。 最後に、ロジック アプリの定義にアプリケーション ID を追加します。 

### パート 1: ロジック アプリのアプリケーション ID の設定

これは、ロジック アプリが Active Directory に対する認証に使用する ID です。 だけ *必要がある* これを行うディレクトリです。 たとえば、選択可能ロジック アプリのすべてに同じ id を使用するしたい場合に、ロジック アプリごとの一意のものを作成することも可能性がありますができます。 この操作は UI で、または PowerShell を使用して実行できます。

#### Azure クラシック ポータルを使用してアプリケーション ID を作成する

1. 移動 [Azure クラシック ポータルでの Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) し、Web アプリを使用するディレクトリを選択
2. クリックして、 **アプリケーション** ] タブ
3. クリックして **追加** ページの下部にあるコマンド バーの
4. ID に使用する名前を付けて、[次へ] 矢印をクリックします。
5. ドメイン形式の一意の文字列を 2 つのテキスト ボックスに入力し、チェックマークをクリックします。
6. クリックして、 **構成** このアプリケーションのタブをクリックします。
7. コピー、 **クライアント ID**, 、これは、ロジック アプリの使用
8.  **キー** ] をクリック **の期間を選択** 1 年間または 2 年間
9. クリックして、 **保存** (数秒待機する必要があります)、画面の下部にあるボタンをクリックします。
10. ここで、ボックス内のキーをコピーしてください。 このキーもロジック アプリで使用します。

#### PowerShell を使用したアプリケーション ID の作成
1. `Switch-AzureMode AzureResourceManager`
2. `Add-AzureAccount`
3. `New-AzureADApplication -DisplayName "MyLogicAppID" -HomePage "http://someranddomain.tld" -IdentifierUris "http://someranddomain.tld" -Password "Pass@word1!"`
4. コピーしてください、 **テナント ID**, 、 **アプリケーション ID** と使用したパスワード

### パート 2: AAD アプリの ID による Web アプリの保護

Web アプリが既にデプロイされている場合、ポータルでそのアプリを有効にすることができます。 それ以外の場合、Azure リソース マネージャーによるデプロイの中で承認を有効にすることができます。

#### Azure ポータルで承認を有効にする

1. Web アプリに移動し、クリックして、 **設定** コマンド バーにします。
2. クリックして **承認/認証**します。 
3. 電源を **に**します。

この時点で、アプリケーションが自動的に作成されます。 このアプリケーションのクライアント ID は、パート 3 で必要となるため、次の手順を実行する必要があります。

1. 移動して [Azure クラシック ポータルでの Active directory](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory) し、ディレクトリを選択します。 
2. 検索ボックスでアプリを検索します。
3. 一覧からアプリをクリックします。
4. をクリックして、 **構成** ] タブ
5. 確認する必要があります、 **クライアント ID**

#### ARM テンプレートを使用して Web アプリをデプロイする

最初に、Web アプリ用のアプリケーションを作成する必要があります。 これは、ロジック アプリに使用するアプリケーションとは別に作成します。 第 1 部で、そのため、上記の手順に従ってを開始して、 **ホームページ** と **IdentifierUris** 実際 https:// を使用して**URL** Web アプリのです。

>[AZURE.NOTE]使用する必要があります、Web アプリのアプリケーションを作成するとき、 [Azure クラシック ポータル アプローチ](https://manage.windowsazure.com/#Workspaces/ActiveDirectoryExtension/directory), PowerShell コマンドレットが、web サイトへのユーザーのサインインに必要なアクセス許可を設定しないようにします。

1 回、クライアント ID と、展開テンプレートには、Web アプリの sub リソースとして以下を含めますテナント ID があります。

```
"resources" : [
    {
        "apiVersion" : "2015-08-01",
        "name" : "web",
        "type" : "config",
        "dependsOn" : [
            "[concat('Microsoft.Web/sites/','parameters('webAppName'))]"
        ],
        "properties" : {
            "siteAuthEnabled": true,
            "siteAuthSettings": {
              "clientId": "<<clientID>>",
              "issuer": "https://sts.windows.net/<<tenantID>>/",
            }
        }
    }
]
```

展開を実行する、空の Web アプリケーションと、ロジック アプリがまとめて展開を自動的に AAD クリックを使用する、次のボタン。
[![Azure へのデプロイします。](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-logic-app-custom-api%2Fazuredeploy.json) 

完全なテンプレートを参照してください。 [ロジック アプリが App Service でホストされていると、AAD で保護されている、カスタム API を呼び出す](https://github.com/Azure/azure-quickstart-templates/blob/master/201-logic-app-custom-api/azuredeploy.json)します。


### パート 3: ロジック アプリの承認セクションの入力

 **承認** のセクションで、 **HTTP** アクション。
`{"tenant":"<<tenantId>>", "audience":"<<clientID from Part 2>>", "clientId":"<<clientID from Part 1>>","secret": "<<Password or Key from Part 1>>","type":"ActiveDirectoryOAuth" }`

| 要素 | 説明 |
|---------|-------------|
| type | 認証の種類。 ActiveDirectoryOAuth 認証の場合、値は ActiveDirectoryOAuth です。 |
| テナント | AD テナントを識別するためのテナント識別子。 |
| 対象となる読者 | 必須。 接続するリソース。 |
| clientID | Azure AD アプリケーションのクライアント識別子。 |
| secret | 必須。 トークンを要求しているクライアントのシークレット。 | 

これは、上記のテンプレートで既に設定されていますが、直接ロジック アプリを作成する場合は、[承認] セクション全体を追加する必要があります。

## コード内の API の保護

### 証明書認証

クライアント証明書を使用して、Web アプリへの受信要求を検証できます。 参照してください [方法に TLS 相互認証を構成する Web アプリの](app-service-web-configure-tls-mutual-auth.md) 、コードを設定する方法についてです。 

 *承認* セクションを入力してください: `{"type": "clientcertificate","password": "test","pfx": "long-pfx-key"}`です。 

| 要素 | 説明 |
|---------|-------------|
| type | 必須。 認証の種類。SSL クライアント証明書の場合、値には ClientCertificate を使用する必要があります。 |
| pfx | 必須。 Base64 でエンコードされた PFX ファイルのコンテンツ。 |
| パスワード | 必須。 PFX ファイルにアクセスするためのパスワード。 |

### 基本認証

基本認証 (ユーザー名とパスワードなど) を使用して、受信要求を検証できます。 基本認証は一般的なパターンで、アプリをビルドする任意の言語で実行できます。

 *承認* セクションを入力してください: `{"type": "basic","username": "test","password": "test"}`です。 

| 要素 | 説明 |
|---------|-------------|
| type | 必須。 認証の種類。 基本認証の場合、値には Basic を使用する必要があります。 |
| ユーザー名 | 必須。 認証するユーザー名。 |
| パスワード | 必須。 認証するパスワード。 |
 
### コード内での AAD 認証の処理

既定では、ポータルで有効にする Azure Active Directory 認証では、きめ細かい承認は実行されません。 たとえば、この認証では、API が特定のユーザーまたはアプリにはロックされず、特定のテナントのみにロックされます。

たとえば、コード内で API をロジック アプリのみに制限したい場合は、JWT を含むヘッダーを抽出して呼び出し元を照会し、一致しないすべての要求を拒否できます。

この記事を参照して、独自のコードでは、完全に実装し、ポータル] 機能を活用していない場合は、理解を深める、: [Azure App Service での認証に Active Directory を使用して](web-sites-authentication-authorization.md)します。

ただし、ロジック アプリのアプリケーション ID を作成して API の呼び出しに使用する場合は、このページの手順に従う必要があります。

