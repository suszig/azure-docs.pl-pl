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



# HTML アプリの作成

[AZURE.INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)] 
&nbsp;

>[AZURE.IMPORTANT] HTML/JavaScript アプリのクイック スタートは Azure ポータルから一時的に削除されているため、現在このトピックは Mobile Apps には適用されません。 まもなく、再びサポートされるようになる予定です。 もうしばらくお待ちください。

## 概要

このチュートリアルでは、クラウド ベースのバックエンド サービスを HTML5/JavaScript Web アプリに追加する方法を説明します。 詳細については、次を参照してください。 [モバイル アプリとは](app-service-mobile-value-prop.md)します。

完成したアプリケーションのスクリーンショットは次のようになります。

![完成したアプリのスクリーン ショット](./media/app-service-mobile-html-get-started/mobile-quickstart-completed-html.png)

HTML アプリの他のすべての Mobile Apps チュートリアルを行う前に、このチュートリアルを完了する必要があります。

## 前提条件

このチュートリアルを完了するには、以下が必要です。

* アクティブな Azure アカウント。 アカウントがない場合、Azure 試用版にサインアップして、最大 10 件の無料 Mobile Apps を入手できます。このアプリは評価終了後も使用できます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/)します。

* [Visual Studio Community 2013] 以降のバージョン。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](https://tryappservice.azure.com/?appServiceName=mobile), 、App Service で有効期間の短いスターター モバイル アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 新しいモバイル アプリ バックエンドの作成

新しいモバイル アプリ バックエンドを作成するには、次の手順に従います。

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

これで、モバイル クライアント アプリケーションで使用できる Azure モバイル アプリ バックエンドのプロビジョニングが完了しました。 次は、簡単な "todo list" バックエンドのサーバー プロジェクトをダウンロードして、それを Azure に発行します。

## サーバー プロジェクトのダウンロード

1. [Azure ポータル] をクリックして **すべてを参照** > **Web Apps**, 、先ほど作成したモバイル アプリ バックエンド順にクリックします。

2. モバイル アプリ バックエンドで、**[すべての設定]** をクリックし、**[モバイル アプリ]** の下で **[クイック スタート]**、**[HTML/JavaScript]** の順にクリックします。

3. **[新しいアプリの作成]** の **[サーバー プロジェクトをダウンロードして実行する]** の下で **[ダウンロード]** をクリックし、圧縮されたプロジェクト ファイルをローカル コンピューターに抽出して、Visual Studio でソリューションを開きます。

4. プロジェクトをビルドして、NuGet パッケージを復元します。

## サーバー プロジェクトでの CORS の有効化

クロス オリジン リソース共有 (CORS) は、Web ベースのアプリでどのドメインからの要求が安全であり、ブラウザーによって許可される必要があるかを示す方法の 1 つです。 モバイル アプリ バックエンドにアクセスするすべての Web サイトに対して CORS エントリを追加する必要があります。 標準的な ASP.NET Web API の振る舞いを使用して CORS 設定の制御を行います。 詳細については、次を参照してください。 [ASP.NET Web API でのクロス オリジン要求を有効にすると](http://www.asp.net/web-api/overview/security/enabling-cross-origin-requests-in-web-api#enable-cors)します。

既定では、ポータルからダウンロードするクライアントのクイック スタート プロジェクトは、localhost 上のポート 8000 で実行されます。 このためには、次の CORS を有効にする、 `http://localhost:8000` サーバー プロジェクトにします。

1. ツールメニューの Visual Studio で **[NuGet Package Manager]**、**[Package Manager Console]** の順にクリックし、**[パッケージのソース]** として Nuget.org を選択して、コンソール ウィンドウで次のコマンドを実行します。

        Install-Package Microsoft.AspNet.WebApi.Cors  

2. App_Start/Startup.MobileApp.cs プロジェクト ファイルを開き、次の using ステートメントを追加します。

        using System.Web.Http.Cors;

3. 次に、**HttpConfiguration** (*config*) の作成後に、次のコードを **Startup.ConfigureMobileApp** メソッドに追加します。

        // Enable CORS support for localhost port 8000, all headers and methods.
        var cors = new EnableCorsAttribute("http://localhost:8000", "*", "*");
        config.EnableCors(cors);

4. 更新内容を保存します。

次に、CORS が有効なプロジェクトを Azure にデプロイします。

## Azure へのサーバー プロジェクトの発行

[AZURE.INCLUDE [app-service-mobile-dotnet-backend-publish-service](../../includes/app-service-mobile-dotnet-backend-publish-service.md)]

## クライアント プロジェクトのダウンロードおよび実行

1. モバイル アプリ バックエンドのブレードに戻り、**[すべての設定]** をクリックし、**[モバイル アプリ]** の下で **[クイック スタート]**、**[HTML/JavaScript]** の順にクリックします。

2.  **[新しいアプリの作成]** の **[HTML/Javascript プロジェクトをダウンロードして実行する]** の下で **[ダウンロード]** をクリックし、圧縮されたプロジェクト ファイルをローカル コンピュータに保存します。

3. 圧縮されたプロジェクト ファイルの保存場所を参照し、ファイルをコンピューター上に展開して、**server** サブフォルダーから次のコマンド ファイルの 1 つを開始します。

    + **launch-windows** (Windows コンピューター)
    + **launch-mac.command** (Mac OS X コンピューター)
    + **launch-linux.sh** (Linux コンピューター)
    > [AZURE.NOTE] Windows コンピューターで次のように入力します。 `R` PowerShell が要求すると、スクリプトを実行することを確認します。 Web ブラウザーでは、インターネットからダウンロードしたスクリプトであるため、実行しないよう警告されることがあります。 その場合は、ブラウザーがスクリプトの読み込みを開始するよう要求する必要があります。

    これにより、新しいアプリケーションをホストする Web サーバーがローカル コンピューター上で起動します。

4. URL を開く <a href="http://localhost:8000/" target="_blank">http://localhost:8000/</a> で web ブラウザーが、アプリケーションを開始します。 クライアント アプリが Azure のモバイル アプリ バックエンドに接続されるように構成されます。

5. アプリケーションで、たとえば、tutorial_ の意味のあるテキストを入力で **Enter 新しいタスク**, 、クリックして **追加**します。

    ![アプリの実行](./media/app-service-mobile-html-get-started/mobile-quickstart-startup-html.png)

    これで、Azure でホストされている新しいモバイル アプリ バックエンドに POST 要求が送信されます。 要求のデータは、モバイル アプリ スキーマの TodoItem テーブルに挿入されます。 テーブルに格納された項目はサービスによって返され、データはアプリケーションの 2 番目の列に表示されます。
    > [AZURE.TIP] モバイル サービスにアクセスして app.js ファイルにあるデータを照会および挿入するコードを確認できます。 





[get started with authentication]: app-service-mobile-windows-store-dotnet-get-started-users.md 
[mobile app sdk]: http://go.microsoft.com/fwlink/?LinkId=257545 
[azure portal]: https://portal.azure.com/ 
[visual studio community 2013]: https://www.visualstudio.com/downloads 

