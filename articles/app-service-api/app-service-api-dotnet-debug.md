<properties 
    pageTitle="Azure App Service の API アプリのデバッグ" 
    description="Visual Studio を使用して、Azure App Service で実行中の API アプリをデバッグする方法について説明します。" 
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


# Azure App Service の API アプリのデバッグ

[AZURE.INCLUDE [app-service-api-v2-note](../../includes/app-service-api-v2-note.md)]

## 概要

実行するように構成する ASP.NET Web API コードをデバッグするこのチュートリアルでは、 [API アプリ](app-service-api-apps-why-best-platform.md) で [Azure App Service](../app-service/app-service-value-prop-what-is.md)します。 ローカルで実行しているときと Azure でリモートで実行しているときに、このコードをデバッグします。

チュートリアル、API アプリを扱います [作成](app-service-dotnet-create-api-app.md) と [展開](app-service-dotnet-deploy-api-app.md) このシリーズの前のチュートリアルでします。

## リモートでの API アプリのデバッグ

リモート デバッグを有効にするには、デバッグ ビルドを Azure にデプロイする必要があります。

1. Visual Studio で **ソリューション エクスプ ローラー**, 、内で展開したプロジェクトを右クリックして [、前の「](app-service-dotnet-deploy-api-app.md), 、を選択して **発行**します。

2. **[Web の発行]** ダイアログで、**[設定]** タブ、**[デバッグ]** ビルド構成の順に選択します。

4. **[発行]** をクリックします。

    ![Publish project](./media/app-service-api-dotnet-debug/rd-debug-publish.png)

    ブラウザー ウィンドウが API アプリのベース URL に対して開きます。

4. ブラウザーのアドレス バーで、URL の最後に swagger/ を追加し、Enter キーを押します。

    この手順では、指示に従って、Swagger UI を有効にしたでは、 [作成](app-service-dotnet-create-api-app.md) チュートリアルです。

    ![Swagger UI](./media/app-service-api-dotnet-debug/rd-swagger-ui.png)

5. Visual Studio に戻って、**[表示]、[サーバー エクスプローラー]** の順にクリックします。

6. **サーバー エクスプローラー**で、**[Azure]、[App Service]** ノードの順に展開します。

7. API アプリをデプロイしたときに作成または選択したリソース グループを探します。

8. リソース グループの下にある API アプリのノードを右クリックし、**[デバッガーの接続]** を選択します。

    ![デバッガーの接続](./media/app-service-api-dotnet-debug/rd-attach-debugger.png)

    リモート デバッガーが接続を試みます。 場合によっては、接続を確立するために **[デバッガーの接続]** を再度クリックすることが必要になる場合があります。失敗した場合は、再試行してください。

    ![デバッガーの接続](./media/app-service-api-dotnet-debug/rd-attaching.png)

9. 接続が確立されると、開く、 **ContactsController.cs** API アプリ プロジェクトでファイルし、でブレークポイントを追加、 `取得` メソッドです。

    ![コントローラーへのブレークポイントの適用](./media/app-service-api-dotnet-debug/rd-breakpoints.png)

10. ブラウザーのセッションに戻り、**[Get]** 動詞をクリックして *Contact* オブジェクトのスキーマを表示し、**[Try it Out]** をクリックします。 設定したブレークポイントでプログラムの実行が停止します。ここで、コントローラーのロジックをデバッグできます。

    ![試してみる](./media/app-service-api-dotnet-debug/rd-try-it-out.png)

## ローカルでの API アプリのデバッグ

Azure サーバーへのラウンドトリップによってデバッグが低速になる場合など、ローカルでの API アプリのデバッグが必要になることがあります。 このセクションでは、テスト クライアントとして Swagger UI を使用してローカルで API アプリをデバッグする方法を示しています。

2. 移動し、ブラウザー、 [Azure プレビュー ポータル](https://portal.azure.com)します。

3. サイド バーで **[参照]** をクリックし、**[API Apps]** を選択します。

    ![Azure ポータルの ](./media/app-service-api-dotnet-debug/ld-browse.png)

4. サブスクリプションの API アプリの一覧から作成した API アプリを選択します。

    ![API App List](./media/app-service-api-dotnet-debug/ld-api-app-list.png)

5. API アプリのブレードで、**[API アプリ ホスト]** をクリックします。

    ![API App Host](./media/app-service-api-dotnet-debug/ld-api-app-blade-api-app-host.png)

6. API アプリ ホストのブレードで、**[すべての設定]** をクリックします。

    ![API App Host All Settings](./media/app-service-api-dotnet-debug/ld-api-app-host-all-settings.png)

7. **[設定]** ブレードで、**[アプリケーション設定]** をクリックします。

    ![API App Host Application Settings](./media/app-service-api-dotnet-debug/ld-application-settings.png)

8. **[Web アプリの設定]** ブレードで、下へスクロールして **[アプリケーション設定]** セクションを表示します。

    ![API App Host Application Settings for Local Debug](./media/app-service-api-dotnet-debug/ld-app-settings-for-local-debugging.png)

1. Visual Studio で、API アプリのプロジェクトの *web.config* ファイルを開きます。

9. **[アプリケーション設定]** で、次の各キーと値を *web.config* ファイルの **appSettings** セクションに追加します。
    - **EMA\_MicroserviceId**
    - **EMA\_Secret**
    - **EMA\_RuntimeUrl**

    この手順を完了すると、*web.config* の **appSettings** セクションは、次のスクリーンショットのようになります。

    ![API App Host Application Settings for Local Debug](./media/app-service-api-dotnet-debug/ld-debug-settings.png)

    **注:**、 *EMA_* に追加する値、 *web.config* このセクションでファイルには、機密の認証情報が含まれています。 したがって、これらの値は、パブリックなソース管理リポジトリには保存しないことをお勧めします。 詳細については、次を参照してください。 [ASP.NET と Azure App Service にパスワードやその他の機密情報を展開するためのベスト プラクティス](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)します。

10. API アプリのコント ローラーのコードにブレークポイントを設定、 `取得` メソッドです。

11. F5 キーを押して Visual Studio のデバッグ セッションを開始します。

13.  API アプリのアクセス レベルが **[パブリック (匿名)]** に設定されている場合、Swagger UI ページをテストに使用できます。

    * When the browser loads the page, you see an HTTP 403 error page. Add */swagger* to the end of the URL in your browser's address bar and press Enter.
    
    * Once the Swagger UI has loaded, click the **Get** verb on the browser window to display the schema for the Contact object, and then click **Try it Out**.

   設定したブレークポイントでプログラムの実行が停止します。ここで、コントローラーのロジックをデバッグできます。

14. API アプリのアクセス レベルに設定されている場合 **パブリック (認証済み)**, を認証し、次の手順に示すようにブラウザー ツールを使用する必要があります [API アプリの保護](app-service-api-dotnet-add-authentication.md#use-postman-to-send-a-post-request) Post 要求は。

    * ゲートウェイのログイン URL に移動し、ログインするための資格情報を入力します。
    * x-zumo-auth Cookie から Zumo トークンの値を取得します。
    * 要求に x-zumo-auth ヘッダーを追加し、その値を x-zumo-auth Cookie の値に設定します。
    * 要求を送信します。

    **注:** ローカルで実行している場合は、API アプリに対するアクセスを Azure で制御して、そのメソッドの実行を認証済みのユーザーのみに限定することができません。 Azure で実行している場合、API アプリを対象とするすべてのトラフィックはゲートウェイを介してルーティングされるため、認証されていない要求がゲートウェイを通過することはありません。 ローカルで実行している場合はリダイレクトされません。つまり、認証されていない要求から API アプリへのアクセスが妨げられることはありません。 このような認証により、ログオン ユーザーに関する情報を取得するコードなど、API アプリの認証関連コードを正常に実行できるという利点があります。 ゲートウェイが API アプリの認証がどのように処理する方法の詳細については、次を参照してください。 [API apps と mobile apps の認証](../app-service/app-service-authentication-overview.md#azure-app-service-gateway)します。

## 次のステップ

このチュートリアルでは、API Apps をデバッグする方法について説明しました。

トラブルシューティングの詳細については、次を参照してください。 [Visual Studio を使用して Azure App Service で web アプリケーションのトラブルシューティング](../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md)します。 API Apps は、Web サービスをホストする追加機能を備えた Web アプリです。そのため、Web アプリに使用する API アプリ用と同じデバッグとトラブルシューティング ツールを使用できます。





