<properties 
    pageTitle="Azure App Service での API アプリのデプロイ " 
    description="Azure サブスクリプションに API アプリ プロジェクトをデプロイする方法について説明します。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/08/2015" 
    ms.author="tdykstra"/>

# Azure App Service での API アプリのデプロイ 

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

このチュートリアルで作成した Web API プロジェクトをデプロイ、 [前のチュートリアル](app-service-dotnet-create-api-app.md) を新しい [API アプリ](app-service-api-apps-why-best-platform.md)します。 Visual Studio を使用して、[API アプリのリソースを作成する [Azure App Service](../app-service/app-service-value-prop-what-is.md) Azure API アプリを Web API コードを展開するとします。 

### その他のデプロイ オプション

API アプリをデプロイする方法は他にも多数あります。 API アプリは、 [web アプリ](../app-service-web/app-service-web-overview.md) web サービス、およびすべてのホストの追加機能と [web アプリの展開方法で利用可能な](../app-service-web/web-sites-deploy.md) API アプリで使用することもできます。 API アプリをホストする Web アプリは、Azure プレビュー ポータルでは API アプリ ホストと呼ばれ、[API アプリ ホスト] ポータル ブレードを使用してデプロイを構成できます。 API アプリ ホスト] ブレードの詳細については、次を参照してください。 [API アプリの管理](app-service-api-manage-in-portal.md)します。

API アプリが Web アプリをベースにしているという事実は、ASP.NET 以外のプラットフォーム用に記述されたコードを API アプリにデプロイできることを意味しています。 Git を使用して Node.js コードを API アプリにデプロイする例を参照してください。 [Azure App Service での Node.js API アプリを作成](app-service-api-nodejs-api-app.md)します。
 
## <a id="provision"></a>Azure での API アプリの作成 

このセクションで、Visual Studio を使用して **Web の発行** Azure で API アプリを作成するウィザード。 手順では、API アプリの名前を入力するように指示、」と入力 *ContactsList*します。

[AZURE.INCLUDE [app-service-api-pub-web-create](../../includes/app-service-api-pub-web-create.md)]

## <a id="deploy"></a>新しい Azure API アプリへのコードのデプロイ

使用すると、同じ **Web の発行** ウィザード、新しい API アプリにコードをデプロイします。

[AZURE.INCLUDE [app-service-api-pub-web-deploy](../../includes/app-service-api-pub-web-deploy.md)]

## Azure API アプリの呼び出し 

前のチュートリアルで Swagger UI を有効にしたので、その Swagger UI を使用して、API アプリが Azure で実行されていることを確認できます。

1.  [Azure プレビュー ポータル](https://portal.azure.com), に移動して、 **API アプリ** デプロイした API アプリのブレードです。

2. API アプリの URL をクリックします。

    ![URL をクリック](./media/app-service-dotnet-deploy-api-app/clickurl.png)

    [API アプリが正常に作成されました] ページが表示されます。

3. ブラウザーのアドレス バーで、URL の最後に "/swagger" を追加します。

4. 表示される Swagger ページで、[ **連絡先 > 取得 > Try it Out**します。

    ![実際に使ってみる](./media/app-service-dotnet-deploy-api-app/swaggerui.png)

## ポータルでの API 定義の表示

1.  [Azure プレビュー ポータル](https://portal.azure.com), に戻り、 **API アプリ** デプロイした API アプリのブレードです。

4. クリックして **API の定義**します。 
 
    アプリの **API の定義** ブレードは、アプリケーションを作成したときに定義した API 操作の一覧を表示します。 

    ![API の定義](./media/app-service-dotnet-deploy-api-app/29-api-definition-v3.png)

次に、API 定義を変更し、その変更がポータルに反映されていることを確認します。

5. Visual Studio でプロジェクトに戻り、次のコードを追加、 **ContactsController.cs** ファイルです。   

        [HttpPost]
        public HttpResponseMessage Post([FromBody] Contact contact)
        {
            // todo: save the contact somewhere
            return Request.CreateResponse(HttpStatusCode.Created);
        }

    このコードを追加、 **投稿** 新規投稿に使用できるメソッドを `Contact` インスタンスを API です。

    Contacts クラスのコードが次のように表示されます。

        public class ContactsController : ApiController
        {
            [HttpGet]
            public IEnumerable<Contact> Get()
            {
                return new Contact[]{
                            new Contact { Id = 1, EmailAddress = "barney@contoso.com", Name = "Barney Poland"},
                            new Contact { Id = 2, EmailAddress = "lacy@contoso.com", Name = "Lacy Barrera"},
                            new Contact { Id = 3, EmailAddress = "lora@microsoft.com", Name = "Lora Riggs"}
                        };
            }
        
            [HttpPost]
            public HttpResponseMessage Post([FromBody] Contact contact)
            {
                // todo: save the contact somewhere
                return Request.CreateResponse(HttpStatusCode.Created);
            }
        }

7.  **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **発行**します。 

9. クリックして、 **プレビュー** ] タブ

10. クリックして **プレビューの開始** にどのファイルを Azure にコピーするを参照してください。  

    ![[Web の発行] ダイアログ](./media/app-service-dotnet-deploy-api-app/39-re-publish-preview-step-v2.png)

11. クリックして **発行**します。

6. 最初に発行したときのように、ゲートウェイを再起動します。

12. 発行プロセスが完了するに戻り、ポータルを閉じてから再び開きます、 **API の定義** ブレードです。 先ほど作成し、Azure サブスクリプションに直接デプロイした新しい API エンドポイントが表示されます。

    ![API の定義](./media/app-service-dotnet-deploy-api-app/38-portal-with-post-method-v4.png)

## 次のステップ

ここまでは、Visual Studio の直接デプロイする機能を使用して、API が正常に機能しているかどうかのテストを簡単に実行する方法について説明しました。  [次のチュートリアル](../app-service-dotnet-remotely-debug-api-app.md), 、Azure で実行中に、API アプリをデバッグする方法について説明します。

API アプリは、API をホストするための特別な機能を持つ Web アプリです。つまり、Web アプリで動作する任意のデプロイ方法を使用することができます。 Web アプリの配置オプションの詳細については、次を参照してください。 [Azure App Service で web アプリのデプロイ](../app-service-web/web-sites-deploy.md)します。

API Apps の機能については、次を参照してください。 [API apps とは?](app-service-api-apps-why-best-platform.md)します。


