<properties
    pageTitle="Azure App Service での API Apps のユーザー認証 |Microsoft Azure"
    description="認証されたユーザーのみがアクセスできるよう Azure App Service で API アプリを保護する方法について説明します。"
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
    ms.date="11/30/2015"
    ms.author="tdykstra"/>


# Azure App Service の API Apps でのユーザー認証

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

## 概要

このチュートリアルでは、Azure App Service の認証および承認機能を使用して API アプリを保護する方法と、エンド ユーザーの代わりに API アプリを使用する方法について説明します。 このチュートリアルで取り上げている認証プロバイダーは Azure Active Directory、API は ASP.NET Web API、クライアントはブラウザーで実行される AngularJS シングル ページ アプリケーションです。

![](./media/app-service-api-dotnet-user-principal-auth/contactspageazure.png)

## App Service での認証と承認

このチュートリアルで使用する認証機能の概要については、このシリーズの前のチュートリアルを参照してください。 [Azure App Service で API Apps の認証と承認](app-service-api-authentication.md)します。

## このチュートリアルの使い方

このチュートリアルをダウンロードしてでの API アプリを作成するサンプル アプリケーションの構築、 [、API Apps と ASP.NET の取得の最初のチュートリアル シリーズの開始](app-service-api-dotnet-get-started.md)します。

## ContactsList.Angular.AAD サンプル プロジェクト

[ContactsList サンプル アプリケーション](https://github.com/Azure-Samples/app-service-api-dotnet-contact-list), 、ContactsList.Angular.AAD プロジェクトは、AngularJS クライアントと Azure Active Directory を操作するためのコードが含まれます。 コードが参照して、AAD サンプルに基づいて、 [Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi](https://github.com/Azure-Samples/active-directory-angularjs-singlepageapp-dotnet-webapi) リポジトリです。

ContactsList.Angular.AAD プロジェクト内のコードは、単純な ContactsLists.Angular プロジェクトとは構成が異なります。 API を呼び出すコードは、ContactsList.Angular.AAD プロジェクト内の *app/scripts/contactsSvc.js* ファイルにあります。

        angular.module('contactsListApp')
        .factory('contactsSvc', ['$http', function ($http) {
            //var apiEndpoint = "https://{your api app name}.azurewebsites.net";
            var apiEndpoint = "https://localhost:44300";
    
            $http.defaults.useXDomain = true;
            delete $http.defaults.headers.common['X-Requested-With']; 
    
            return {
                getItems: function () {
                    return $http.get(apiEndpoint + '/api/contacts');
                },
                getItem : function(id){
                    return $http.get(apiEndpoint + '/api/contacts/' + id);
                },
                postItem : function(item){
                    return $http.post(apiEndpoint + '/api/contacts', item);
                },
                putItem : function(item){
                    return $http.put(apiEndpoint + '/api/contacts/', item);
                },
                deleteItem : function(id){
                    return $http({
                        method: 'DELETE',
                        url: apiEndpoint + '/api/contacts/' + id
                    });
                }
            };
        }]);

ここでは、 `取得` メソッドというラベルの付いた `getItems`します。 コント ローラー (*app/scripts/contactsCtrl.js*)、 `getItems` がワイヤード (有線) 最大 `$scope.populate`します。

        $scope.populate = function () {
            contactsSvc.getItems().then(function (results) {
                $scope.contactsList = results.data;
                $scope.loadingMessage = "";
            }, function (err) {
                $scope.loadingMessage = "";
                $scope.error = "Error: " + err;
            });
        };

ビュー (*app/views/Contacts.html*) で、$scope.populate は初期化時に呼び出されます。

        <div ng-init="populate()">

コードを追加すると、自動的と承認トークンの API 要求をログに記録される、 [Azure Active Directory の認証ライブラリの JavaScript](https://github.com/AzureAD/azure-activedirectory-library-for-js), で、 *adal.js* と *adal-angular.js* ファイルです。

*App.js* ファイルでは、このコードでは構成情報と `$http` プロバイダーに、 `adalProvider.init` 関数です。 構成情報には、各 API エンドポイントに関連する AAD アプリケーション クライアント ID と、この AngularJS アプリに関連するクライアント ID が含まれています。  `Init` 関数にインターセプターを追加する、 `$http` プロバイダー、認証トークンを要求に追加これらことです。

        var endpoints = { 
            //"https://{your api app name}.azurewebsites.net/": "{your client id}"
            "https://localhost:44300/": "{your client id}"
        };
    
        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: '{your tenant url}',
                clientId: '{your client id}',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
                //cacheLocation: 'localStorage', // enable this for 
            },
            $httpProvider
            );

## Azure での認証および承認の設定

1. [Azure ポータル](https://portal.azure.com/), に移動し、 **API アプリ** できるように、認証されたユーザーのみを保護する API アプリのブレードで呼び出すことができます。 (このチュートリアルでは、ContactsList.API プロジェクトをデプロイした API アプリを選択します)。

2. **[設定]** をクリックします

2. **[機能]** セクションを探し、**[認証/承認]** をクリックします。

    ![](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. **[認証/承認]** ブレードで、**[オン]** をクリックします。

4. **[要求が認証されていない場合のアクション]** ドロップダウン リストで、**[Azure Active Directory でログイン]** を選択します。

5. **[認証プロバイダー]** の下の **[Azure Active Directory]** をクリックします。

    ![](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. **[Azure Active Directory 設定]** ブレードで **[Express]** をクリックします。

    ![](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    ここで、"Express" は、Azure が AAD テナントに AAD アプリケーションを自動的に作成することを意味します。 新しい AAD アプリケーションのクライアント ID を取得するために Azure クラシック ポータルに後で移動して選択するために、新しい AAD アプリケーションの名前をメモします。

7. **[OK]** をクリックします。

10. **認証/承認**ブレードで、**[保存]** をクリックします。

8. API アプリが現在保護されていることを確認するには、API アプリの URL に移動 + `/swagger` Swagger UI を使用する最初のチュートリアルで実行したようです。

    今度はログオン ページにリダイレクトされます。

    ![](./media/app-service-api-dotnet-user-principal-auth/loginpage.png)

11. [Azure クラシック ポータル](https://manage.windowsazure.com/), に進み、 **Azure Active Directory**します。

    アクセスする必要のある特定の Azure Active Directory 設定が現在の Azure ポータルでまだ利用できないため、クラシック ポータルに移動する必要があります。

12. **[ディレクトリ]** タブで AAD テナントをクリックします。

    ![](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. **[アプリケーション]、[自分の会社が所有するアプリケーション]** の順にクリックし、チェック マークをクリックします。

    新しいアプリケーションを確認するには、ページを更新する必要がある場合があります。

15. アプリケーションの一覧から、API アプリの認証を有効にしたときに Azure によって作成されたアプリケーション名をクリックします。

    ![](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. **[構成]** をクリックします。

    ![](./media/app-service-api-dotnet-user-principal-auth/configure.png)

15. ページ下部で **[マニフェストの管理]、[マニフェストのダウンロード]** をクリックし、編集できる場所にファイルを保存します。

16. ダウンロードしたマニフェスト ファイル内で検索、  `oauth2AllowImplicitFlow` プロパティです。 このプロパティの値を変更 `false` に `true`, 、ファイルを保存します。

    この設定は、JavaScript のシングル ページ アプリケーションからのアクセスに必要です。 これにより、Oauth 2.0 ベアラー トークンが URL フラグメントで返されるようになります。

16. **[マニフェストの管理]、[マニフェストのアップロード]** の順にクリックし、前の手順で更新したファイルをアップロードします。

17. チュートリアルの後の手順で、値をコピーして貼り付けることができるよう、このページは開いたままにします。

## Azure API アプリを呼び出す ContactsList.Angular.AAD プロジェクトの構成

次の手順では、アプリケーションを Azure にデプロイし、そこで実行する方法について説明します。ただし、少し変更を加えることで、ローカルで実行できる場合があります。 サンプル コードには、localhost URL エンドポイントが含まれています。 ローカルで実行するには、SSL 用にプロジェクトを設定し、プロジェクト コードで localhost の SSL URL を使用して、AAD アプリケーションの構成で localhost の SSL URL を使用します。 ローカルで実行している場合、AngularJS コードにより、ログオンしているユーザーのみに API の呼び出しが許可されますが、他のクライアントから認証されていない呼び出し側によって API が呼び出される場合があります。

1. ContactsList.Angular.AAD プロジェクトで、*app/scripts/app.js* ファイルを開きます。

8. 設定するコードで、 `エンドポイント` 変数、コメント アウト localhost エンドポイントと Azure のエンドポイントをコメント解除します。

10. "yourclientid" を、AAD アプリケーションのクライアント ID の実際の値に置き換えます。この値は、クラシック ポータルの AAD アプリケーションの **[構成]** タブで見つかります。

2. "{your api app name}" を、ContactsList.API プロジェクトをデプロイした API アプリの名前に置き換えます。

    コードは、現在次の例のようです。

        var endpoints = {
            "https://contactslistapi.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
            //"https://localhost:44300/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. また、 *app.js*, への呼び出しで `adalProvider.init`, 、置換"{、テナント url}"と"{クライアント id、}"を実際の値。

    コードは、次の例のようになります。

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

1. *app/scripts/contactsSvc.js* ファイルで、*app.js* に対して行ったように、エンドポイント URL を localhost から API アプリ URL に変更します。

    コードは、現在次の例のようです。

        var apiEndpoint = "https://contactslistapi.azurewebsites.net";
        //var apiEndpoint = "https://localhost:44300";


## Azure Web アプリの AAD アプリケーションの構成

1. クラシック ポータルの AAD アプリケーションの **[構成]** タブにある **[サインオン URL]** フィールドで、既に設定されている URL を削除し、Web アプリのベース URL (末尾のスラッシュを含む) に置き換えます (これは、API アプリの URL ではなく、AngularJS コードを実行する Web アプリの URL であることに注意してください)。

    ![](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

3. **[応答 URL]** フィールドで、既に設定されている URL を Web アプリのベース URL に置き換えます。

    ![](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

4. **[保存]** をクリックします。

## Azure への ContactsList.Angular.AAD プロジェクトのデプロイ

8. **ソリューション エクスプローラー**で ContactsList.Angular.AAD プロジェクトを右クリックし、**[発行]** をクリックします。

9. **[Microsoft Azure App Service]** をクリックします。

10. **[App Service]** ダイアログ ボックスの **[サブスクリプション]** ボックスの一覧でサブスクリプションを選択します。

11. このチュートリアルで作成したリソース グループを展開し、2 回目のチュートリアルで作成した Web アプリを選択します。

    ![](./media/app-service-api-dotnet-user-principal-auth/deploytowebapp.png)

12. **[OK]** をクリックします。

12. **Web の発行** ウィザード] をクリックして、 **接続** ] タブで、し、[、 **送信先 URL** 変更ボックス `http://` に `https://`

    ![](./media/app-service-api-dotnet-user-principal-auth/httpsinconntab.png)

    この設定により、デプロイが成功した後に既定のブラウザーで開かれる URL が決まります。

12. **Web の発行**ウィザードで **[設定]** タブをクリックします。**[ファイル発行オプション]** を展開し、**[発行先の追加ファイルを削除する]** チェック ボックスをオンにします。

7. **[発行]** をクリックします。

    Visual Studio によってプロジェクトがデプロイされ、アプリのホーム ページがブラウザーで開きます。

## Azure での AngularJS Web アプリのテスト

8. **[Contacts]** タブをクリックします。

    ログインを求められます。

9. AAD テナントのユーザー資格情報でログインします。

10. **[連絡先]** ページが表示されます。

    ![](./media/app-service-api-dotnet-user-principal-auth/contactspageazure.png)

これでフロントエンドは認証ユーザーに代わって API を呼び出すことができ、認証されていないユーザーは、API を呼び出すことはできません。

## 次のステップ

このチュートリアルでは、App Service の認証および承認を使用して、認証されたユーザーのみが呼び出すことができるよう、API アプリへのアクセスを制限しました。 シリーズの次のチュートリアルで学習する方法 [サービス間のシナリオ、API アプリへのアクセス制限](app-service-api-dotnet-service-principal-auth.md)します。





