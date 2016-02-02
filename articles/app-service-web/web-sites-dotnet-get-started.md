<properties
    pageTitle="Azure App Service での ASP.NET Web アプリの作成 | Microsoft Azure"
    description="このチュートリアルでは、Visual Studio 2013 で ASP.NET Web プロジェクトを作成したり、それを Azure App Service の Web アプリにデプロイしたりする方法について説明します。"
    services="app-service\web"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="12/07/2015"
    ms.author="tdykstra"/>


# Azure App Service での ASP.NET Web アプリの作成

> [AZURE.SELECTOR]
- [.Net](web-sites-dotnet-get-started.md)
- [Node.js](web-sites-nodejs-develop-deploy-mac.md)
- [Java](web-sites-java-get-started.md)
- [PHP - Git](web-sites-php-mysql-deploy-use-git.md)
- [PHP - FTP](web-sites-php-mysql-deploy-use-ftp.md)
- [Python](web-sites-python-ptvs-django-mysql.md)


## 概要

このチュートリアルでは、ASP.NET web アプリケーションをデプロイする方法、 [Azure App Service で web アプリ](app-service-web-overview.md) Visual Studio 2015 または Visual Studio 2013 を使用しています。 このチュートリアルは、Azure を使用した経験がない ASP.NET 開発者を対象に作成されています。 このチュートリアルでは、クラウドで動作する単純な Web アプリケーションを作成します。

次の図に、完成したアプリケーションを示します。

![Web app home page](./media/web-sites-dotnet-get-started/deployedandazure.png)

学習内容:

* インストールすることによって Azure 開発用にコンピューターを準備する方法、 [Azure SDK for .NET](../dotnet-sdk/)します。
* ASP.NET MVC 5 Web プロジェクトの作成時に新しい App Service Web アプリを作成するように Visual Studio を設定する方法。
* Visual Studio を使用して、App Service Web アプリに WEB プロジェクトをデプロイする方法。
* Visual Studio **サーバー エクスプローラー** を使用して、リモート ファイルを開き、リモート デバッグ セッションを開始する方法。
* 使用する方法、 [Azure ポータル](/overview/preview-portal/) を監視し、web アプリを管理します。

> [AZURE.NOTE] このチュートリアルでは、Azure App Service と共に ASP.NET を使用する方法を説明しており、ASP.NET Web アプリケーションの開発方法については扱っていません。 ASP.NET MVC 5 の概要については、次を参照してください。 [ASP.NET MVC 5 の概要](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) 上、 [ASP.NET](http://asp.net/) サイトです。 Azure App Service を使用する方法について詳しく説明する資料へのリンクを参照してください、 [次のステップ](#next-steps) セクションです。
> 
> ご協力スコープ] 図形と--このチュートリアルのアプローチを参照してください対象は、ここでは、入門チュートリアルのままにしてフィードバックしたいその他のトピックがある場合、 [Disqus コメント](#comments) チュートリアルの最後にします。

## <a name="video"></a>Microsoft Azure へのサインアップします。

このチュートリアルを完了するには、Azure アカウントが必要です。 そのための方法は次のとおりです。

* [Azure アカウントを無料で開く](/pricing/free-trial/?WT.mc_id=A261C142F)します。 Azure の有料サービスを試用できるクレジットが提供されます。 このクレジットを使い切ってもアカウントは維持されるため、Azure App Service の Web Apps 機能など、無料の Azure サービスと機能を利用できます。
* [Visual Studio のアクティブ化するサブスクライバーの特典](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)します。 Visual Studio サブスクリプションにより、有料の Azure サービスを利用できるクレジットが毎月与えられます。

場合は、Azure アカウントにサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751)します。 有効期間が短いスターター Web アプリを App Service ですぐに作成できます。このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

次のビデオでは、Scott Hanselman が Microsoft Azure の無料試用版に簡単にサインアップできることを説明します。 (時間: 1:58)

> [AZURE.VIDEO sign-up-for-microsoft-azure]

[AZURE.INCLUDE [install-sdk-2015-2013](../../includes/install-sdk-2015-2013.md)]

## プロジェクトと Web アプリケーションを作成する

最初の手順では、Visual Studio で Web プロジェクトを作成し、Azure App Service で Web アプリケーションを作成します。 作成が完了したら、インターネットで Web アプリを公開できるように、そのプロジェクトを Web アプリにデプロイします。

次の図では、作成とデプロイの手順の内容を示しています。

![Create and deploy](./media/web-sites-dotnet-get-started/Create_App.png)

1. Visual Studio 2015 または Visual Studio 2013 を開きます。

    Visual Studio 2013 を使用する場合は、画面がスクリーンショットと若干異なる可能性がありますが、基本的に手順は同じです。

2. **[ファイル]** メニューの **[新規作成]、[プロジェクト]** の順にクリックします。

3. **[新しいプロジェクト]** ダイアログ ボックスで、**[C#]、[Web]、[ASP.NET Web アプリケーション]** の順にクリックします。 必要に応じて、**[Visual Basic]** を選択することもできます。

3. ターゲット フレームワークとして **[.NET Framework 4.5.2]** が選択されていることを確認します。

4.  [Azure Application Insights](app-insights-overview.md) web アプリの可用性、パフォーマンス、および使用状況を監視します。 Application Insights を使わない場合は、**[Application Insights をプロジェクトに追加する]** チェック ボックスをオフにします。

4. アプリケーションに「**MyExample**」という名前を付けます。

5. **[OK]** をクリックします。

    ![New Project dialog box](./media/web-sites-dotnet-get-started/GS13newprojdb.png)

5. **[新しい ASP.NET プロジェクト]** ダイアログ ボックスで、**[MVC]** テンプレートを選択します。

    [MVC](http://www.asp.net/mvc) web アプリの開発に ASP.NET フレームワークです。

7. **[認証の変更]** をクリックします。

    ![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started/GS13changeauth.png)

6. **[認証の変更]** ダイアログ ボックスで、**[認証なし]** をクリックし、**[OK]** をクリックします。

    ![[認証なし]](./media/web-sites-dotnet-get-started/GS13noauth.png)

    作成しているサンプル アプリケーションでは、ユーザーはログインできません。  [次のステップ](#next-steps) 」セクションは、認証と承認を実装するチュートリアルにリンクします。

5. **[新しい ASP.NET プロジェクト]** ダイアログ ボックスで、**[Microsoft Azure]** の設定はそのままにして、**[OK]** をクリックします。

    ![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started/GS13newaspnetprojdb.png)

    既定の設定は、Visual Studio が Web プロジェクトのための Azure Web アプリを作成することを指定します チュートリアルの次のセクションでは、Web プロジェクトを新しく作成した Web アプリにデプロイします。

5. Azure にまだサインインしていない場合は、サインインを求めるメッセージが表示されます。 Azure サブスクリプションを管理するために使用するアカウントの ID とパスワードを使用してサインインします。

    サインインすると、**[App Service の作成]** ダイアログ ボックスが表示され、作成するリソースの情報が求められます。

    ![Signed in to Azure](./media/web-sites-dotnet-get-started/configuresitesettings.png)

3. **[App Service の作成]** ダイアログ ボックスで、*azurewebsites.net* ドメインで一意となる **Web アプリの名前**を入力します。 たとえば、MyExample という名前の右側に番号を付加して一意にすることができます (例: MyExample810)。 既定の Web 名が作成される場合は、それが一意になるため、そのまま使用することができます。

    入力した名前が既に使用されている場合は、右側に緑色のチェック マークではなく赤色の感嘆符が表示されます。この場合、別の名前を入力する必要があります。

    Azure では、この名前がアプリケーションの URL のプレフィックスとして使用されます。 URL 全体は、この名前に *.azurewebsites.net* を追加して構成されます (**[Web アプリ名]** ボックスの横を参照)。 たとえば、名前が `MyExample810`, 、url は、 `MyExample810.azurewebsites.net`します。 URL は一意であることが必要です。

4. **[App Service プラン]** ドロップダウンの隣にある **[新規]** ボタンをクリックします。

    [次のステップ](#next-steps) セクションには App Service プランに関する情報へのリンク。

5. プラン名には、「**MyExamplePlan**」、または必要に応じて別の名前を入力します。

6. **[リソース グループ]** ボックスに新しいリソース グループの名前を入力します。

    [次のステップ](#next-steps) セクションにはリソース グループに関する情報へのリンク。

5. リソース グループ名には、「**MyExampleGroup**」、または必要に応じて別の名前を入力します。

5. **[リージョン]** ドロップダウン リストで、最も近い場所を選択します。

    この設定によって、Web アプリが実行される Azure データ センターが指定されます。このチュートリアルでは、任意のリージョンを選択することができます。任意のリージョンを選択しても、大きな違いはありません。ただし、運用 web アプリでは、選択して web サーバーを最小化するために、サイトにアクセスしているブラウザーのできるだけ近くに [遅延](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)します。

5. データベース フィールドは変更せずそのままにします。

    このチュートリアルでは、データベースは使用しません。  [次のステップ](#next-steps) 」セクションのデータベースを使用する方法を説明するチュートリアルにリンクします。

6. **[OK]** をクリックします。

    ![](./media/web-sites-dotnet-get-started/configuresitesettings2.png)

    Visual Studio により、指定したフォルダーに Web プロジェクトが数秒で作成され、指定した Azure リージョンに Web アプリが作成されます。

    **[ソリューション エクスプローラー]** ウィンドウには、新しいプロジェクトのファイルとフォルダーが表示されます

    ![ソリューション エクスプローラー](./media/web-sites-dotnet-get-started/solutionexplorer.png)

    **[Azure App Service アクティビティ]** ウィンドウには、Web アプリが作成されたことが表示されます。

    ![Web app created](./media/web-sites-dotnet-get-started/GS13sitecreated1.png)

    作成された Web アプリが**サーバー エクスプローラー**に表示されます。

    ![Web app created](./media/web-sites-dotnet-get-started/siteinse.png)

## Web アプリにプロジェクトをデプロイする

このセクションでは、図の手順 2 に示すように、Web プロジェクトを Web アプリにデプロイします。

![Create and deploy](./media/web-sites-dotnet-get-started/Create_App.png)

1. **[ソリューション エクスプローラー]** でプロジェクトを右クリックし、**[発行]** を選択します。

    ![Choose Publish](./media/web-sites-dotnet-get-started/choosepublish.png)

    数秒で、**Web の発行**ウィザードが表示されます。 このウィザードは、新しい Web アプリに Web プロジェクトをデプロイするための設定が含まれる*発行プロファイル*に対して開かれます。 他の Web アプリにデプロイする場合は、**[プロファイル]** タブをクリックして別のプロファイルを作成します。 このチュートリアルでは、以前に作成した Web アプリにデプロイする設定を使用します。

8. **Web の発行**ウィザードの **[接続]** タブで、**[次へ]** をクリックします。

    ![Successfully validated connection](./media/web-sites-dotnet-get-started/GS13ValidateConnection.png)

10. **[設定]** タブの **[次へ]** をクリックします。

    **[構成]** および **[ファイル発行オプション]** には既定値を使用できます。

    **[構成]** ボックスの一覧を使用して、リモート デバッグ用のデバッグ ビルドをデプロイできます。  [次のステップ](#next-steps) 」セクションをリモートでデバッグ モードで Visual Studio を実行する方法を説明するチュートリアルにリンクします。

    ![Settings tab](./media/web-sites-dotnet-get-started/GS13SettingsTab.png)

11. **[プレビュー]** タブで、**[発行]** をクリックします。

    Azure にコピーされるファイルを確認するには、**[発行]** をクリックする前に **[プレビューの開始]** をクリックします。

    ![](./media/web-sites-dotnet-get-started/GS13previewoutput.png)

    **[発行]** をクリックすると、Visual Studio では Azure サーバーにファイルをコピーするプロセスが開始されます。

    **[出力]** ウィンドウと **[Azure App Service アクティビティ]** ウィンドウでは、実行されたデプロイ操作が表示され、デプロイが問題なく完了したことが報告されます。

    ![Output window reporting successful deployment](./media/web-sites-dotnet-get-started/PublishOutput.png)

    デプロイが成功すると、自動的に既定のブラウザーが開き、デプロイ先の Web アプリの URL にアクセスします。これで、作成したアプリケーションはクラウドで実行されています。 ブラウザー アドレス バーの URL は、インターネットから読み込まれた Web アプリを示します。

    ![Web app running in Azure](./media/web-sites-dotnet-get-started/GS13deployedsite.png)

13. 次のセクションで使用するため、このブラウザー ウィンドウは開いたままにします。

**ヒント:** 簡易デプロイの場合も、**[Web の 1 クリック発行]** ツール バーを有効にできます。 **[表示]、[ツール バー]** の順にクリックし、**[Web の 1 クリック発行]** を選択します。 このツール バーを使用すると、プロファイルの選択、ボタンをクリックして発行、またはボタンをクリックして **Web の発行**ウィザードを開くことができます。

![Web One Click Publish Toolbar](./media/web-sites-dotnet-get-started/weboneclickpublish.png)

## サーバー エクスプローラーでリモート ファイルを開く

Web アプリをテストしてデバッグしているときにリモート サイトに対してすばやく一時的な変更を行うには、**サーバー エクスプローラー**でファイルを開いて編集します。

1.  **サーバー エクスプローラー**で、**[Azure]、[App Service]、[MyExampleGroup]** の順に移動し、Web アプリのノードを展開します。

2. **[ファイル]、[ビュー]、[ホーム]** の順に展開し、*Index.cshtml* ファイルをダブルクリックします。

    ![](./media/web-sites-dotnet-get-started/indexfileinse.png)

3. 変更 `< h1 > ASP.NET </h1 >` に `< h1 > Azure App Service </h1 >`します。

4. ファイルを保存します。

5. Azure で実行されているサイトが表示されているブラウザー ウィンドウを更新します。

    ![](./media/web-sites-dotnet-get-started/afterindexedit.png)

この変更は、デプロイされたサイトには適用されますが、ローカルのプロジェクトには適用されません。 プロジェクトを再デプロイすると、サイトはこの変更を加える前の状態に戻ります。

この機能は便利です [詳細なエラー メッセージを取得するために、Web.config ファイルで customErrors を一時的にオフ](web-sites-dotnet-troubleshoot-visual-studio.md)します。

また、**サーバー エクスプローラー**で App Service ノードを右クリックし、Visual Studio ウィンドウで Web アプリの設定にアクセスして、リモート デバッグ セッションを開始し、アプリケーションがアプリケーション ログを書き込む際にリアルタイムで表示することもできます。

![](./media/web-sites-dotnet-get-started/sewebappmenu.png)

詳細については、次を参照してください。 [Visual Studio で web アプリを Azure のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

## Azure ポータルで Web アプリを監視および管理する

[Azure ポータル](/services/management-portal/) 管理し、Azure のサービスを作成した web アプリなどの監視に使用できる web インターフェイスです。 チュートリアルのこのセクションでは、ポータルで何ができるかを確認できます。

1. ブラウザーでに移動 [https://portal.azure.com](https://portal.azure.com), 、Azure の資格情報を使用してサインインします。

2. **[App Services]** をクリックした後、Web アプリの名前をクリックします。

    **[Web アプリ]** ブレードには、Web アプリの設定と利用統計の概要が表示されます。

    ![Web app blade](./media/web-sites-dotnet-get-started/portaldashboard.png)

    この時点では、Web アプリにはトラフィックがそれほど存在しないため、グラフには何も表示されない可能性があります。 アプリケーションにアクセスし、何回かページを更新してから、ポータル ページを更新すると、統計が表示されます。

3. **[設定]** をクリックすると、Web アプリの構成についてさらに多くのオプションが表示されます。

    ![[設定] のクリック](./media/web-sites-dotnet-get-started/portaldashboard2.png)

    設定のタイプの一覧が表示されます。

    ![](./media/web-sites-dotnet-get-started/portalconfigure1.png)

4. **[アプリケーション設定]** をクリックすると、ポータルで構成できる設定の種類の例が表示されます。

    たとえば、web アプリに使用される .NET のバージョンを制御などの機能を有効にする [Websocket](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/), 、設定と [接続文字列の値](/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)します。

    ![Portal web app configure tab](./media/web-sites-dotnet-get-started/portalconfigure2.png)

これらは、ポータル機能の一部にすぎません。 新しい Web アプリを作成したり、既存の Web アプリを削除したり、Web アプリを停止して再起動したり、データベースや仮想マシンなど、その他の種類の Azure サービスを管理したりできます。

## 次のステップ

このチュートリアルでは、Azure の Web アプリに単純な Web アプリケーションを作成してデプロイする方法について説明しました。 Azure App Service の Web アプリの詳細についての関連トピックとリソースがあります。

* データベースと承認の機能を追加する方法

    データベースにアクセスし、承認されたユーザーへの一部のアプリケーション機能を制限する方法を説明するチュートリアルについては、次を参照してください。 [メンバーシップ、OAuth、SQL データベースとの安全な ASP.NET MVC アプリケーションを Azure web アプリにデプロイ](/develop/net/tutorials/web-site-with-sql-database/)します。 そのチュートリアルの MVC 5 の場合は、ある程度の知識を前提としていますMVC 5 に慣れていない場合は、次を参照してください。 [ASP.NET MVC 5 の概要](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started)します。

* Web プロジェクトをデプロイする他の方法

    Visual Studio を使用するか、web アプリに web プロジェクトを展開するには、その他の方法については [展開を自動化する](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) から、 [ソース管理システム](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), を参照してください [Azure web アプリを展開する方法](web-sites-deploy.md)します。

    Visual Studio により、デプロイを自動化するために使用可能な Windows PowerShell スクリプトも生成できます。 詳細については、次を参照してください。 [(Azure で構築実際のクラウド アプリケーション) を自動化してすべて](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)します。

* Web アプリのトラブルシューティングの方法

    Visual Studio には、リアルタイムで生成される Azure ログの表示を容易にする機能があります。 Azure では、リモートでデバッグ モードを使用して実行することもできます。 詳細については、次を参照してください。 [Visual Studio で web アプリを Azure のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

* カスタム ドメイン名および SSL の追加方法

    SSL および独自のドメイン (contoso.azurewebsites.net の代わりに www.contoso.com など) の使用方法については、次のリソースを参照してください。

    * [Azure App Service でカスタム ドメイン名を構成します。](web-sites-custom-domain-name.md)
    * [Azure の web サイトに対して HTTPS を有効にします。](web-sites-configure-ssl-certificate.md)

* リアルタイム機能 (チャットなど) の追加方法

    Web アプリでリアルタイム機能 (チャット サービス、ゲーム、株価情報など) を含める場合を使用して最適なパフォーマンスを実現できます [ASP.NET SignalR](http://www.asp.net/signalr) で、 [Websocket](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/) 転送方法。 詳細については、次を参照してください。 [Azure web apps で使用して SignalR](http://www.asp.net/signalr/overview/signalr-20/getting-started-with-signalr-20/using-signalr-with-windows-azure-web-sites)します。

* Web アプリケーションに対する App Service、Azure Cloud Services、および Azure Virtual Machines の選択方法

    Azure では、このチュートリアルで示したように App Service Web Apps で、または Cloud Services や Virtual Machines で、Web アプリケーションを実行することができます。 詳細については、を参照してください [Azure web アプリ、クラウド サービス、および Vm: どれを使用する場合ですか?](/manage/services/web-sites/choose-web-app-service/)。.

* [選択するか、App Service プランを作成する方法](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

* [選択するか、リソース グループを作成する方法](../azure-preview-portal-using-resource-groups.md)

## 変更内容

* Web サイトから App Service への変更ガイド 』 を参照してください。 [Azure App Service と既存の Azure サービス](http://go.microsoft.com/fwlink/?LinkId=529714)します。





