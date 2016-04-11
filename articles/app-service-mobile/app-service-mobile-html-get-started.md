<properties
    pageTitle="HTML/JavaScript アプリ用モバイル アプリ バックエンドの使用 | Azure App Service Mobile Apps"
    description="このチュートリアルでは、HTML5 および JavaScript で Web アプリ開発用の Azure モバイル アプリ バックエンドを使用する方法を説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="ggailey777"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-html5"
    ms.devlang="javascript"
    ms.topic="get-started-article"
    ms.date="11/18/2015"
    ms.author="glenga"/>


#HTML アプリの作成

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)] 
&nbsp;  
<!--- [AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]-->

>[AZURE.IMPORTANT] このトピックは現在サポートされていませんモバイル アプリで、Html/javascript アプリのクイック スタートは、Azure ポータルから一時的に削除されているためです。 まもなく、再びサポートされるようになる予定です。 もうしばらくお待ちください。

##概要

このチュートリアルでは、クラウド ベースのバックエンド サービスを HTML5/JavaScript Web アプリに追加する方法を説明します。 詳細については、次を参照してください。 [モバイル アプリとは](app-service-mobile-value-prop.md)です。 

完成したアプリケーションのスクリーンショットは次のようになります。

![完成したアプリのスクリーン ショット](./media/app-service-mobile-html-get-started/mobile-quickstart-completed-html.png)

HTML アプリの他のすべての Mobile Apps チュートリアルを行う前に、このチュートリアルを完了する必要があります。 

##前提条件

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント。 アカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。このアプリは評価終了後も使用できます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/)をご覧ください。

* [Visual Studio Community 2013] またはそれ以降のバージョンを選択します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](https://tryappservice.azure.com/?appServiceName=mobile), 、App Service で有効期間の短いスターター モバイル アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

##新しいモバイル アプリ バックエンドの作成

新しいモバイル アプリ バックエンドを作成するには、次の手順に従います。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

これで、モバイル クライアント アプリケーションで使用できる Azure モバイル アプリ バックエンドのプロビジョニングが完了しました。 次は、簡単な "todo list" バックエンドのサーバー プロジェクトをダウンロードして、それを Azure に発行します。

## サーバー プロジェクトのダウンロード

1.  [Azure Portal], 、] をクリックして **すべてを参照** > **Web Apps**, 、先ほど作成したモバイル アプリ バックエンド順にクリックします。 

2. モバイル アプリ バックエンドでクリックして **すべての設定** [ **モバイル アプリ** ] をクリックして **クイック スタート** > **Html/javascript**します。

3. [ **をダウンロードして、サーバー プロジェクトを実行** で **新しいアプリを作成する**, 、] をクリックして **ダウンロード**, 、ローカル コンピューターに圧縮されたプロジェクト ファイルを抽出し、Visual Studio でソリューションを開きます。

4. プロジェクトをビルドして、NuGet パッケージを復元します。

##サーバー プロジェクトでの CORS の有効化

クロス オリジン リソース共有 (CORS) は、Web ベースのアプリでどのドメインからの要求が安全であり、ブラウザーによって許可される必要があるかを示す方法の 1 つです。 モバイル アプリ バックエンドにアクセスするすべての Web サイトに対して CORS エントリを追加する必要があります。 標準的な ASP.NET Web API の振る舞いを使用して CORS 設定の制御を行います。 詳細については、次を参照してください。 [ASP.NET Web API でのクロス オリジン要求を有効にすると](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors)です。

既定では、ポータルからダウンロードするクライアントのクイック スタート プロジェクトは、localhost 上のポート 8000 で実行されます。 このため、次にサーバー プロジェクトで `http://localhost:8000` に対して CORS を有効にします。  

1. ツール] メニューの Visual Studio で [ **NuGet パッケージ マネージャー** > **パッケージ マネージャー コンソール**, 、として Nuget.org を選択して、 **パッケージ ソース** し、コンソール ウィンドウに次のコマンドを実行します。
 
        Install-Package Microsoft.AspNet.WebApi.Cors  

2. App_Start/Startup.MobileApp.cs プロジェクト ファイルを開き、次の using ステートメントを追加します。

        using System.Web.Http.Cors;

3. 次に、次のコードを追加、 **Startup.ConfigureMobileApp** した後に、 **HttpConfiguration** (*config*) が作成します。

        // Enable CORS support for localhost port 8000, all headers and methods.
        var cors = new EnableCorsAttribute("http://localhost:8000", "*", "*");
        config.EnableCors(cors);

4. 更新内容を保存します。

次に、CORS が有効なプロジェクトを Azure にデプロイします。

##Azure へのサーバー プロジェクトの発行

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

##クライアント プロジェクトのダウンロードおよび実行

1. クリックして、モバイル アプリ バックエンド用のブレードに戻り **すべての設定** [ **モバイル アプリ** ] をクリックして **クイック スタート** > **Html/javascript**します。 

2.  [ **をダウンロードして、Html/javascript プロジェクトを実行** で **新しいアプリを作成**, 、] をクリックして **ダウンロード** し、ローカル コンピューターに圧縮されたプロジェクト ファイルを保存します。

3. 圧縮されたプロジェクト ファイルを保存した場所を参照し、ファイル、コンピューター上に展開しのいずれかから次のコマンド ファイル、 **server** サブフォルダーです。

    + **windows の起動時** (Windows コンピューター)
    + **launch-mac.command** (Mac OS X コンピューター)
    + **launch-linux.sh** (Linux コンピューター)

    > [AZURE.NOTE] Windows コンピューターで次のように入力します。 `R` PowerShell が要求すると、スクリプトを実行することを確認します。 Web ブラウザーでは、インターネットからダウンロードしたスクリプトであるため、実行しないよう警告されることがあります。 その場合は、ブラウザーがスクリプトの読み込みを開始するよう要求する必要があります。

    これにより、新しいアプリケーションをホストする Web サーバーがローカル コンピューター上で起動します。

4. Web ブラウザーで URL <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> を開いて、アプリケーションを開始します。 クライアント アプリが Azure のモバイル アプリ バックエンドに接続されるように構成されます。

5. アプリケーションで、意味のあるテキストをなど入力 _に関するチュートリアルを完了_, で、 **Enter 新しいタスク**, 、] をクリックし、 **追加**します。

    ![アプリの実行](./media/app-service-mobile-html-get-started/mobile-quickstart-startup-html.png)

    これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。 要求のデータは、モバイル アプリ スキーマの TodoItem テーブルに挿入されます。 テーブルに格納された項目はサービスによって返され、データはアプリケーションの 2 番目の列に表示されます。

    > [AZURE.TIP] モバイル サービスを照会および挿入 app.js ファイルであるデータにアクセスするコードを確認できます。 

<!-- Anchors. -->
<!-- Images. -->
<!-- URLs. -->
[Get started with authentication]: app-service-mobile-windows-store-dotnet-get-started-users.md
[Mobile App SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure Portal]: https://portal.azure.com/

[Visual Studio Community 2013]: https://www.visualstudio.com/downloads
 


