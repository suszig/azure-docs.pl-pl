<properties
    pageTitle="Azure AD AngularJS の概要 | Microsoft Azure"
    description="ユーザーのサインインに個人の Microsoft アカウントと会社/学校アカウントの両方を使用する Angular JS シングル ページ アプリを構築する方法について説明します。"
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
    ms.date="12/09/2015"
    ms.author="dastrock"/>


# アプリ モデル v2.0 プレビュー: AngularJS シングル ページ アプリへのサインインの追加 - .NET

この記事では、Microsoft が提供するアカウントによるサインインを、Azure Active Directory のアプリ モデル v2.0 を使用して AngularJS アプリに追加します。  アプリ モデル v2.0 を使用すると、アプリで統合を 1 回実行するだけで、個人アカウントと職場/学校アカウントの両方でユーザーを認証できるようになります。

ここで扱うサンプルは、タスクをバックエンドの REST API に格納する簡単な To-Do List シングル ページ アプリです。.NET 4.5 MVC フレームワークで記述され、Azure AD の OAuth ベアラー トークンを使用して保護されます。  AngularJS アプリが、オープン ソース JavaScript 認証ライブラリを使用して [adal.js](https://github.com/AzureAD/azure-activedirectory-library-for-js) を全体のサインイン プロセスを処理し、REST API を呼び出すためのトークンを取得します。  このようなその他の REST Api に対する認証に同じパターンを適用できます、 [Graph](https://graph.microsoft.com) または Azure リソース マネージャー Api です。

## ダウンロード

作業を開始するには、Visual Studio をダウンロードしてインストールする必要があります。  複製することができますし、または [ダウンロード](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/skeleton.zip) スケルトン アプリ。

```
git clone --branch skeleton https://github.com/AzureADQuickStarst/AppModelv2-SinglePageApp-AngularJS-DotNet.git
```

スケルトン アプリには、簡単な AngularJS アプリ用の定型コードがすべて含まれますが、ID 関連の部分はまったく含まれません。  代わりに複製できます理解できない場合または [ダウンロード](https://github.com/AzureADQuickStarts/AppModelv2-SinglePageApp-AngularJS-DotNet/archive/complete.zip) 、完全なサンプルです。

```
git clone https://github.com/AzureADSamples/SinglePageApp-AngularJS-DotNet.git
```

## アプリを登録します

アプリを最初に、作成、 [アプリ登録ポータル](https://apps.dev.microsoft.com), 、次の [詳細な手順について](active-directory-v2-app-registration.md)します。  次のことを確認します。

- 追加、 **Web** アプリのプラットフォームです。
- 正しい入力 **リダイレクト URI**します。 このサンプルの既定値は `https://localhost:44326/` です。
- ままにして、 **暗黙のフローを許可する** ] チェック ボックスを有効にします。 

下のコピー、 **アプリケーション ID** アプリに割り当てられている、間もなく必要があります。 

## adal.js をインストールする
まず、ダウンロードしたプロジェクトに移動し、adal.js をインストールします。  使用していれば [bower](http://bower.io/) をインストールするだけこのコマンドを実行します。  任意の依存関係のバージョンの不一致の以降のバージョンを選択します。
```
bower install adal-angular#experimental
```

または、手動でダウンロードできます [adal.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal.min.js) と [adal-angular.js](https://raw.githubusercontent.com/AzureAD/azure-activedirectory-library-for-js/experimental/dist/adal-angular.min.js)します。  両方のファイルを `TodoSPA` プロジェクトの `app/lib/adal-angular-experimental/dist` ディレクトリに追加します。

Visual Studio でプロジェクトを開き、メイン ページ本文の最後に adal.js を読み込みます。

```html
<!--index.html-->

...

<script src="App/bower_components/dist/adal.min.js"></script>
<script src="App/bower_components/dist/adal-angular.min.js"></script>

...
```

## REST API を設定する

バックエンドの REST API が動作するように設定します。  プロジェクトのルートにある `web.config` を開き、`audience` の値を置き換えます。  REST API は、この値を使用して、AJAX 要求で Angular アプリから受け取ったトークンを検証します。

```xml
<!--web.config-->

...

    <appSettings>
        <add key="ida:Audience" value="[Your-application-id]" />
    </appSettings>
    
...
```

REST API のしくみについて説明するときにはよくあることです。  自由にコードでは、いろいろ調べてが、Azure ad での Api を web を保護する方法の詳細を確認する場合はチェック アウト [今回](active-directory-v2-devquickstarts-dotnet-api.md)します。 

## ユーザーをサインインする
次に、ID コードを作成します。  既にお気付きかもしれませんが、adal.js に含まれる AngularJS プロバイダーが Angular のルーティング メカニズムを適切に処理します。  最初に、adal モジュールをアプリケーションに追加します。

```js
// app/scripts/app.js

angular.module('todoApp', ['ngRoute','AdalAngular'])
.config(['$routeProvider','$httpProvider', 'adalAuthenticationServiceProvider',
 function ($routeProvider, $httpProvider, adalProvider) {

...
```

アプリケーション ID で `adalProvider` を初期化できるようになります。

```js
// app/scripts/app.js

...

adalProvider.init({
        
        // Use this value for the public instance of Azure AD
        instance: 'https://login.microsoftonline.com/', 
        
        // The 'common' endpoint is used for multi-tenant applications like this one
        tenant: 'common',
        
        // Your application id from the registration portal
        clientId: '<Your-application-id>',
        
        // If you're using IE, uncommment this line - the default HTML5 sessionStorage does not work for localhost.
        //cacheLocation: 'localStorage',
         
    }, $httpProvider);
```

アプリを保護し、ユーザーをサインインするために必要な情報はすべて adal.js に含まれています。  アプリの特定のルートにサインインを強制するためのコードは 1 行だけで済みます。

```js
// app/scripts/app.js

...

}).when("/TodoList", {
    controller: "todoListCtrl",
    templateUrl: "/static/views/TodoList.html",
    requireADLogin: true, // Ensures that the user must be logged in to access the route
})

...
```

ユーザーが `TodoList` リンクをクリックしたとき、サインインの処理が必要な場合は、adal.js によって Azure AD に自動的にリダイレクトされます。  また、コントローラーで adal.js を呼び出すことによって、サインインおよびサインアウト要求を明示的に送信することもできます。

```js
// app/scripts/homeCtrl.js

angular.module('todoApp')
// Load adal.js the same way for use in controllers and views   
.controller('homeCtrl', ['$scope', 'adalAuthenticationService','$location', function ($scope, adalService, $location) {
    $scope.login = function () {
        
        // Redirect the user to sign in
        adalService.login();
        
    };
    $scope.logout = function () {
        
        // Redirect the user to log out    
        adalService.logOut();
    
    };
...
```

## ユーザー情報を表示する
ユーザーがサインインしたら、サインインしたユーザーの認証データにアプリケーションからアクセスする必要があります。  adal.js は、`userInfo` オブジェクトでこの情報を公開します。  ビューでこのオブジェクトにアクセスするには、まず adal.js を対応するコントローラーのルート スコープに追加します。

```js
// app/scripts/userDataCtrl.js

angular.module('todoApp')
// Load ADAL for use in view
.controller('userDataCtrl', ['$scope', 'adalAuthenticationService', function ($scope, adalService) {}]);
```

こうすると、ビューで `userInfo` オブジェクトを直接アドレス指定できます。 

```html
<!--app/views/UserData.html-->

...

    <!--Get the user's profile information from the ADAL userInfo object-->
    <tr ng-repeat="(key, value) in userInfo.profile">
        <td>{{key}}</td>
        <td>{{value}}</td>
    </tr>
...
```

また、`userInfo` オブジェクトを使用してユーザーがサインインしているかどうかも判定できます。

```html
<!--index.html-->

...

    <!--Use the ADAL userInfo object to show the right login/logout button-->
    <ul class="nav navbar-nav navbar-right">
        <li><a class="btn btn-link" ng-show="userInfo.isAuthenticated" ng-click="logout()">Logout</a></li>
        <li><a class="btn btn-link" ng-hide="userInfo.isAuthenticated" ng-click="login()">Login</a></li>
    </ul>
...
```

## REST API を呼び出す
最後に、いくつかのトークンを取得し、タスクを作成、読み取り、更新、削除する REST API を呼び出します。  ただし、  実行する必要はありません *もの*します。  トークンの取得、キャッシュ、更新は adal.js が自動的に行います。  また、REST API に送信される AJAX 要求へのトークンの添付も行われます。  

そのしくみは すべての助けを借りては [AngularJS インターセプター](https://docs.angularjs.org/api/ng/service/$http), 、adal.js を送信および受信の http メッセージを変換することができます。  さらに、adal.js は、ウィンドウと同じドメインに送信される要求が AngularJS アプリと同じアプリケーション ID 用のトークンを使用するものと想定します。  そのために、Angular アプリと NodeJS REST API の両方で同じアプリケーション ID を使用しました。  もちろん、必要に応じてこの動作をオーバーライドし、他の REST API 用のトークンを取得するよう adal.js に指示できますが、このシナリオではシンプルに既定のままにします。

次のスニペットは、Azure AD からベアラー トークンを含む要求を簡単に送信できることを示しています。

```js
// app/scripts/todoListSvc.js

...
return $http.get('/api/tasks');
...
```

ご利用ありがとうございます。  Azure AD に統合されたシングル ページ アプリが完成しました。  アプリは、  ユーザーの認証を行い、OpenID Connect を使用してバックエンドの REST API を安全に呼び出し、ユーザーについての基本情報を取得できます。  個人の Microsoft アカウントまたは Azure AD の職場/学校アカウントを持つユーザーが既定でサポートされます。  アプリを実行し、ブラウザーで `https://localhost:44326/` に移動して、  個人の Microsoft アカウントまたは職場/学校アカウントを使用してサインインしてください。  ユーザーの To-Do List にタスクを追加し、サインアウトした後で、  他の種類のアカウントを使用してサインインしてみてください。 Azure AD テナントを作業/学校のユーザーを作成する必要がある場合 [いずれかを取得する方法については、ここ](active-directory-howto-tenant.md) (無料です)。

アプリ モデルのバージョン 2.0 のプレビューについて学習を続行するに戻って、 [v2.0 開発者ガイド](active-directory-appmodel-v2-overview.md)します。  その他のリソースについては、以下を参照してください。

- [GitHub の Azure 用サンプル >>](https://github.com/Azure-Samples)
- [Stack Overflow の Azure AD >>](http://stackoverflow.com/questions/tagged/azure-active-directory)
- Azure AD に関するドキュメントを [Azure.com >>](http://azure.microsoft.com/documentation/services/active-directory/)
