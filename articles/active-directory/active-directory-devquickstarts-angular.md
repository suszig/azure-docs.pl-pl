<properties
    pageTitle="Azure AD AngularJS の概要 | Microsoft Azure"
    description="サインインし、OAuth を使用して Azure AD で保護されている API を呼び出すために Azure AD と連携する Angular JS シングル ページ アプリを構築する方法"
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="javascript"
    ms.topic="article"
    ms.date="10/13/2015"
    ms.author="dastrock"/>


# Azure AD で AngularJS シングル ページ アプリをセキュリティで保護する

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Azure AD を使用すると、シングル ページ アプリに対する、サインイン、サインアウト、セキュリティで保護された OAuth API 呼び出しを簡単に追加できます。  アプリは、Active Directory アカウントを使用してユーザー認証を処理することも、Office 365 API や Azure API などの Azure AD によって保護された任意の Web API を使用することもできます。

ブラウザーで実行している javascript アプリケーションに対しては、Active Directory 認証ライブラリ (adal.js) を提供します。  adal.js の唯一の目的は、アプリからトークンに容易にアクセスできるようにすることです。  それがどれほど簡単であるかを示すために、ここで、次のような、AngularJS To Do List アプリケーションを構築します。

- Azure AD を ID プロバイダーとして使用して、ユーザーがアプリにサインインする処理を実行します。
- ユーザーについての、いくつかの情報を表示します。
- AAD からのベアラー トークンを使用して、アプリの To Do List API を安全に呼び出します。
- アプリからユーザーがサインアウトする処理を実行します。

完全に動作するアプリケーションを構築するには、次の手順を行う必要があります。

2. Azure AD にアプリケーションを登録します。
3. ADAL をインストールし、SPA を構成する
5. ADAL を使用して、SPA 内のページをセキュリティで保護する

最初に、 [アプリのスケルトンをダウンロードする](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) または [、完全なサンプルをダウンロード](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip)します。  また、ユーザーを作成し、アプリケーションを登録することを可能にするための Azure AD テナントも必要です。  場合は、テナントをまだ持っていない [いずれかを取得する方法について](active-directory-howto-tenant.md)します。

## *1.DirectorySearcher アプリケーションを登録します。*
アプリでユーザーを認証し、トークンを取得できるようにするには、まず、アプリを Azure AD テナントに登録する必要があります。

-   サインイン、 [Azure 管理ポータル](https://manage.windowsazure.com)
-   左側のナビゲーションで] をクリックして **Active Directory**
-   アプリケーションの登録先となるテナントを選択します。
-   クリックして、 **アプリケーション** タブをクリックし、クリックして **追加** 下部のドロアーでします。
-   画面の指示に従ってされ、新しい作成 **Web アプリケーションまたは WebAPI**します。
    -    **名前** アプリケーションはエンドユーザーにアプリケーションを記述します。
    -    **リダイレクト Uri** AAD がトークンを返す先の場所です。  このサンプルの既定の場所は `https://localhost:44326/` です。
-   登録が完了するには、AAD が割り当てられますアプリ一意 **クライアント ID**します。  この値が必要する次のセクションでのでコピーから、 **構成** ] タブをクリックします。
- adal.js は Azure AD との通信に、OAuth の暗黙的なフローを使用します。  次の手順を実行して、アプリケーションに対して暗黙的なフローを有効にする必要があります。
    - クリックして、アプリケーション マニフェストをダウンロード **マニフェストの管理**します。
    - マニフェストを開いて、`oauth2AllowImplicitFlow` プロパティを見つけます。 値を `true` に設定します。
    - 保存 (&) をクリックして、アプリケーション マニフェストをアップロード **マニフェストの管理** もう一度です。

## *2.ADAL をインストールする (&)、SPA を構成します。*
アプリケーションを Azure AD に登録したので、adal.js をインストールし、ID 関連のコードを記述できます。

-   まず、パッケージ マネージャー コンソールを使用して、adal.js を TodoSPA プロジェクトに追加します。
  - ダウンロード [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal.js) にそれを追加し、 `App/Scripts/` プロジェクト ディレクトリ。
  - ダウンロード [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/master/lib/adal-angular.js) にそれを追加し、 `App/Scripts/` プロジェクト ディレクトリ。
  - 各スクリプトを `index.html` の `</body>` の前に読み込みます。

```js
...
<script src="App/Scripts/adal.js"></script>
<script src="App/Scripts/adal-angular.js"></script>
...
```

-   SPA のバックエンド To Do List API は、ブラウザーからのトークンを受け入れできるようにするために、アプリの登録に関する構成情報を必要とします。 TodoSPA プロジェクトで、`web.config` を開きます。  Azure ポータルで入力した値が反映されるように、`<appSettings>` セクションの要素の値を置き換えます。  これらの値は、コードで ADAL を使用する際に常に参照されます。
    -   `ida:Tenant` は、Azure AD テナントのドメイン (たとえば、contoso.onmicrosoft.com) です。
    -    `ida:Audience` する必要があります、 **クライアント ID** のポータルからコピーしたアプリケーションです。

## *3.ADAL を使用して、SPA 内のページを保護するには*
adal.js は、AngularJS ルートおよび http プロバイダーと連携するように構築されているので、SPA の個々のビューをセキュリティで保護できます。

- `App/Scripts/app.js` に、adal.js モジュールを組み込みます。

```js
angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {
...
```
- これで、`App/Scripts/app.js` でも、アプリケーション登録の構成値を使用して `adalProvider` を初期化できます。

```js
adalProvider.init(
  {
      instance: 'https://login.microsoftonline.com/',
      tenant: 'Enter your tenant name here e.g. contoso.onmicrosoft.com',
      clientId: 'Enter your client ID here e.g. e9a5a8b6-8af7-4719-9821-0deef255f68e',
      extraQueryParameter: 'nux=1',
      //cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
  },
  $httpProvider
);
```
- 次に、アプリの `TodoList` ビューをセキュリティで保護します。必要なコードは、`requireADLogin` の 1 行のみです。

```js
...
}).when("/TodoList", {
        controller: "todoListCtrl",
        templateUrl: "/App/Views/TodoList.html",
        requireADLogin: true,
...
```

これで、ユーザーのサインインを処理し、ベアラー トークンで保護された要求をバックエンド API に送信できる、安全なシングル ページ アプリが完成しました。  ユーザーは `TodoList` リンクをクリックすると、必要に応じて、サインインを処理するために、adal.js により Azure AD に自動的にリダイレクトされます。  さらに、アプリケーションのバックエンドに送信されるすべての ajax 要求に対して、access_token が adal.js により自動的にアタッチされます。  上記の例は、adal.js を使用する SPA を構築するための必要最小限のコードを含んでいますが、これら以外にも、SPA で役立つその他の機能が多数存在します。

- サインイン要求とサインアウト要求を明示的に発行するために、adal.js を呼び出すコントローラーで関数を定義できます。  `App/Scripts/homeCtrl.js` で、次のように記述します。

```js
...
$scope.login = function () {
    adalService.login();
};
$scope.logout = function () {
    adalService.logOut();
};
...
```
- アプリの UI にユーザー情報を表示することもできます。  Adal サービスが既にに追加されて、 `userDataCtrl` コント ローラーにアクセスできるように、 `userInfo` 、関連付けられたビュー内のオブジェクト `App/Views/UserData.html`:

```js
<p>{{userInfo.userName}}</p>
<p>aud:{{userInfo.profile.aud}}</p>
<p>iss:{{userInfo.profile.iss}}</p>
...
```

- ユーザーがサインインしているかどうかを確認することが必要なシナリオも多数存在します。  `userInfo` オブジェクトを使用して、この情報を収集することもできます。  たとえば、`index.html` で、認証ステータスに応じて、[ログイン] ボタンまたは [ログアウト] ボタンのいずれかを表示できます。

```js
<li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
<li><a class="btn btn-link" ng-hide=" userInfo.isAuthenticated" ng-click="login()">Login</a></li>
```

ご利用ありがとうございます。 Azure AD に統合されたシングル ページ アプリが完成しました。  このアプリは、ユーザーの認証処理を行い、OAuth 2.0 を使用してバックエンドを安全に呼び出し、ユーザーについての基本情報を取得することができます。  テナントに一連のユーザーを設定します (設定していない場合)。  To Do List SPA を実行し、それらのユーザーの一人としてサインインします。  タスクをユーザーの To Do List に追加し、サインアウトして、再びサインインします。

adal.js を使用することにより、これらの共通 ID 機能のすべてを容易にアプリケーションに組み込むことができます。  キャッシュ管理、OAuth プロトコル サポート、ログイン UI を使用してのユーザーの提示、有効期限切れとなったトークンの更新など、面倒な操作を容易に実装できます。

リファレンスについては、完全なサンプル (構成値) を除くが提供される [ここ](https://github.com/AzureADQuickStarts/SinglePageApp-AngularJS-DotNet/archive/complete.zip)します。  ここからは、さらなるシナリオに進むことができます。  次のチュートリアルを試してみてください。

[SPA から CORS Web API の呼び出し](https://github.com/AzureAdSamples/SinglePageApp-WebAPI-AngularJS-DotNet)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]


