<properties
    pageTitle="CORS を使用して JavaScript から API アプリを使用する | Microsoft Azure"
    description="JavaScript クライアントから、および CORS を使用して Azure App Service の API アプリを使用する方法について説明します"
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
    ms.topic="get-started-article"
    ms.date="12/04/2015"
    ms.author="tdykstra"/>

# CORS を使用して JavaScript から API アプリを使用する

[AZURE.INCLUDE [app-service-api-get-started-selector](../../includes/app-service-api-get-started-selector.md)]

## 概要

このチュートリアルでは、API アプリとは異なるドメインから提供されている Web サイトの JavaScript コードから API アプリを使用する方法を示します。 サンプルのクライアントでは、AngularJS を使用します。

![](./media/app-service-api-cors-consume-javascript/homepageazure.png)
 
これは、Azure App Service で API アプリを使用する方法を示すチュートリアルのシリーズの 2 回目です。 移動する、一連の最初から最初のトピックを選択して、 **トピック** 、ページの上部にあるドロップダウン リストです。

## Azure App Service における CORS のサポート

セキュリティ上の理由から、ブラウザーの既定の動作では JavaScript が含まれるドメイン以外のドメインに対して、JavaScript が API 呼び出しを行うことができません。 たとえば、contoso.com の Web ページから contoso.com API エンドポイントには呼び出しができますが、fabrikam.com エンドポイントには呼び出しができません。 クロス オリジン リソース共有 (CORS) は、このようなクロス ドメインの API 呼び出しを行う必要があるシナリオを有効にするように設計されたインターネット プロトコルです。 Azure App Service のこのようなシナリオの例として、API が API アプリを実行している間に JavaScript クライアントが Web アプリを実行している場合などがあります。  

Azure App Service には、API アプリの呼び出しを許可するドメインを簡単に構成する仕組みが用意されています。CORS 機能は、Java や Node.js など、API Apps サービスでサポートされるすべての言語で同じように動作します。

## このチュートリアルの使い方

このチュートリアルでダウンロードしてでの API アプリを作成したサンプル アプリケーションでは、 [このシリーズの ASP.NET のバージョンの最初のチュートリアル](app-service-api-dotnet-get-started.md)します。 Java、Node.js を使用する場合は、「、 [CORS の構成] セクション](#corsconfig) 下すべての API アプリに適用される一般的な手順です。

## ContactsList.Angular サンプル プロジェクト

 [ContactsList サンプル アプリケーション](https://github.com/Azure-Samples/app-service-api-dotnet-contact-list), 、ContactsList.Angular プロジェクトは ContactsList.API Web API プロジェクト用の単純 AngularJS クライアントです。 

API を呼び出して AngularJS JavaScript コードは、 *index.html* ContactsList.Angular プロジェクト内のファイルです。 このコードは API の Get メソッドが `$scope.refresh()` に定義されている場合のように、関数を定義して、`$scope` オブジェクトに追加します。

        angular.module('myApp', []).controller('contactListCtrl', function ($scope, $http) {
            $scope.baseurl = 'http://localhost:51864';
        
            $scope.refresh = function () {
                $scope.status = "Refreshing Contacts...";
                $http({
                    method: 'GET',
                    url: $scope.baseUrl + '/api/contacts',
                    headers: {
                        'Content-Type': 'application/json'
                    }
                }).then(function (results) {
                    $scope.contacts = results.data;
                    $scope.status = "Contacts loaded";
                }, function (err) {
                    $scope.status = "Error loading contacts";
                });
            };
        
            // POST and DELETE not shown
        
            $scope.refresh();
        });

ページを読み込みます (末尾の前に示したスニペットの) がワイヤード (有線) にするときに、コードが $scope.refresh() メソッドを呼び出す、 **更新** UI のボタンをクリックします。

        <th><button class="btn btn-sm btn-info" ng-click="refresh()">Refresh</button></th>

## AngularJS プロジェクトをローカルで実行する

このセクションでは、クライアントをローカルで実行でき、API もローカルで実行されている間にはこれを呼び出すことができることを確認します。

1. ContactsList.API の開始時、ContactsList.Angular の前に、ContactsList.API および ContactsList.Angular プロジェクトをスタートアップ プロジェクトとして設定します。 

2. F5 キーを押してプロジェクトを開始します。

    AngularJS の UI には、ローカルに格納されている連絡先が表示されます。UI を使用して、連絡先の追加および削除ができます。

    ![](./media/app-service-api-cors-consume-javascript/homepagelocal.png)

3. ブラウザー ウィンドウを閉じます。

## Azure API アプリを指すように AngularJS プロジェクトを変更します。 

次に、AngularJS フロントエンドをクラウドで実行して、クラウドで実行されている API バックエンドを呼び出します。 フロントエンドを Azure にデプロイする前に、前に作成した Azure API アプリがコードで呼び出されるように、AngularJS プロジェクトの API エンドポイントを変更する必要があります。

1. ContactsList.Angular プロジェクトで開きます *index.html*します。

2. `baseUrl` を localhost の URL に設定する行をコメントアウトし、`baseUrl` を azurewebsites.net の URL に設定する行をコメント解除して、プレースホルダーを前に作成した API アプリの実際の名前に置き換えます。  API アプリに ContactsListAPI という名前を付けた場合、コードは次の例のようになります。

        $scope.baseUrl = 'https://ContactsListAPI.azurewebsites.net';
        //$scope.baseUrl = 'http://localhost:51864';

### ContactsList.Angular プロジェクトを Web アプリにデプロイする

AngularJS プロジェクトをデプロイする新しい Web アプリも作成できますが、このチュートリアルでは、前のチュートリアルで作成した Web アプリにデプロイします。 Web アプリの名前には、元々は ASP.NET MVC プロジェクトをデプロイしていたが、このデプロイメントの後は AngularJS コードが実行されることを反映することができます。

8.  **ソリューション エクスプ ローラー**, を ContactsList.Angular プロジェクトを右クリックして、クリックして **発行**します。

9. クリックして、 **プロファイル** ] タブをクリックします。

3.   **プロファイル** のステップ、 **Web の発行** ウィザード] をクリックして **Microsoft Azure App Service**します。

4.  **App Service** ] ダイアログ ボックスで、サブスクリプションを選択します。

5.  **ビュー** 既定値に設定 **リソース グループ**, 、このチュートリアル シリーズ用に作成したリソース グループを展開します。

7. 最初のチュートリアルで作成した web アプリを選択します (API アプリを選択しないことを確認してください)、順にクリック **OK**します。

8. クリックして、 **設定** ] タブをクリックします。

9. 展開 **ファイル発行オプション**, 、し、[ **転送先にその他のファイルを削除する**です。

    ![](./media/app-service-api-cors-consume-javascript/removeadditionalfiles.png)

    通常、既存の App Service の Web アプリに Web プロジェクトをデプロイする場合、変更は更新またはファイルの新規作成のどちらかであるため、"追加のファイルを削除する" オプションは必要ありません。 この場合、異なるプロジェクトを同じ Web アプリにデプロイしているため、新しいものでは必要ない前のデプロイメントのファイルが多数発生する可能性があります。 

10. クリックして **発行**します。

    Visual Studio を使うと、ContactsList.Angular プロジェクトが Web アプリにデプロイされ、Web アプリの URL がブラウザーで開きます。 ブラウザーにはローカルでの実行時に表示されたのと同じ AngularJS の UI が表示されます。ただし、今はフロントエンドがバックエンドとは異なるドメイン (Web アプリの URL) で実行されているため、失敗します。 

    ![](./media/app-service-api-cors-consume-javascript/corserror.png)

## <a id="corsconfig"></a> Azure のターゲット API アプリに対して CORS を構成する

8. 別のブラウザー ウィンドウで、 [Azure ポータル](https://portal.azure.com/)します。

9. クリックして **参照 > API Apps**, 、しターゲット API アプリを選択します。 このチュートリアルでは、1 つ目のチュートリアルで ContactsList.API プロジェクトに作成した API アプリが該当します。

10.  **API アプリ** ブレードで、をクリックして **設定**します。

11. 検索、 **API** セクションは、クリックして **CORS**します。

12. テキスト ボックスに、呼び出し元として許可する URL を入力します。 たとえば、ContactsListMVC という名前の Web アプリに JavaScript アプリケーションをデプロイした場合、「http://contactslistmvc.azurewebsites.net」と入力します。

    URL を入力する代わりに、元のドメインをすべて受け入れることを指定するアスタリスク (*) を入力することができます。

13. クリックして **保存**します。

    ![](./media/app-service-api-cors-consume-javascript/corsinportal.png)

14. AngularJS クライアントを表示するブラウザー ウィンドウに移動し、ページを更新または] をクリックして、 **更新** ] ボタンをクリックします。

    ページには、Azure API アプリのファイル システムに格納されている連絡先が表示されます。

    ![](./media/app-service-api-cors-consume-javascript/homepageazure.png)

### Azure リソース マネージャーのツールにおける CORS

Azure リソース マネージャーの Azure PowerShell、CLI などのツールを使用して、API アプリに対して CORS を構成することも、または [リソース エクスプ ローラー](https://resources.azure.com/)します。 

`cors` プロパティは、<site name>/web リソースの Microsoft.Web/sites/config リソース タイプで設定します。 たとえば、 **リソース エクスプ ローラー**, には、 **サブスクリプション > {サブスクリプション} > resourceGroups > {リソース グループ} > プロバイダー > Microsoft.Web > サイト > {ƒtƒcƒg} > 構成 > web**, と cors のプロパティが表示されます。

        "cors": {
            "allowedOrigins": [
                "contactslistmvc.azurewebsites.net"
            ]
        }

### App Service の CORS と Web API の CORS

以降のセクションで取り上げるように、ASP.NET Web API プロジェクトでは、CORS をコードの中で簡単に構成することができます。 ただし、App Service の CORS と Web API の CORS の両方を一緒に使っている場合、App Service の CORS が優先され、Web API の CORS は無効となります。 たとえば、App Service で元のドメインを 1 つ有効にして、Web API コードですべての元のドメインを有効にした場合、Azure API アプリは Azure で指定したドメインからの呼び出しのみを受け付けます。


## Web API コードで CORS を構成する方法

Web API プロジェクトでインストールすることができます、 [Microsoft.AspNet.WebApi.Cors](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Cors/) API を使用すると、コードでどのドメインを指定する NuGet パッケージは JavaScript からの呼び出しを受け入れます。 このプロセスの詳細は「 [ASP.NET Web API 2 でのクロス オリジン要求有効すると](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api) 徹底的にします。 ASP.NET Web API を使用してビルドされた API アプリの場合、プロセスはまったく同じですが、以下に簡単にまとめています。

1. Web API プロジェクトが含まれる、 `config.EnableCors()` 内のコード行、 **登録** のメソッド、 **WebApiConfig**, 次の例のようにします。 

        public static class WebApiConfig
        {
            public static void Register(HttpConfiguration config)
            {
                // Web API configuration and services
                
                // The following line enables you to control CORS by using Web API code
                config.EnableCors();
    
                // Web API routes
                config.MapHttpAttributeRoutes();
    
                config.Routes.MapHttpRoute(
                    name: "DefaultApi",
                    routeTemplate: "api/{controller}/{id}",
                    defaults: new { id = RouteParameter.Optional }
                );
            }
        }

1. Web API のコントローラーで、`ContactsController` クラスまたは個々のアクション メソッドに `EnableCors` 属性を追加します。 次の例では、コントローラー全体に CORS のサポートが適用されます。

        namespace ContactList.Controllers
        {
            [HttpOperationExceptionFilterAttribute]
            [EnableCors(origins:"*", headers:"*", methods: "*")]
            public class ContactsController : ApiController
 
    > **注**: すべてのパラメーターにワイルドカードを使用して、 `EnableCors` 属性では、デモンストレーションのためにだけ使用し、すべてのオリジンとすべての HTTP 要求 API が開かします。 この属性は注意して使用してください。

## 次のステップ 

このチュートリアルでは、クライアントの JavaScript コードが、別のドメイン内の API を呼び出すための App Service の CORS サポートを有効にする方法を説明しました。 開始された系列を取得する API アプリで次の記事でについても説明 [App Service の API アプリの認証の](app-service-api-authentication.md)です。

