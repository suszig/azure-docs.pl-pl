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
* Visual Studio を使用する方法 **サーバー エクスプ ローラー** をリモート ファイルを開き、リモート デバッグ セッションを開始します。 
* 使用する方法、 [Azure ポータル](/overview/preview-portal/) を監視し、web アプリを管理します。

> [AZURE.NOTE] このチュートリアルでは、Azure App Service; で ASP.NET を使用する方法ASP.NET web アプリケーションを開発する方法を教えません。 ASP.NET MVC 5 の概要については、次を参照してください。 [ASP.NET MVC 5 の概要](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) 上、 [ASP.NET](http://asp.net/) サイトです。 Azure App Service を使用する方法について詳しく説明する資料へのリンクを参照してください、 [次のステップ](#next-steps) セクションです。
> 
> ご協力スコープ] 図形と--このチュートリアルのアプローチを参照してください対象は、ここでは、入門チュートリアルのままにしてフィードバックしたいその他のトピックがある場合、 [Disqus コメント](#comments) チュートリアルの最後にします。

##<a name="video"></a>Microsoft Azure にサインアップする

このチュートリアルを完了するには、Azure アカウントが必要です。 そのための方法は次のとおりです。

* [Azure アカウントを無料で開く](/pricing/free-trial/?WT.mc_id=A261C142F)します。 Azure の有料サービスを試用できるクレジットが提供されます。 このクレジットを使い切ってもアカウントは維持されるため、Azure App Service の Web Apps 機能など、無料の Azure サービスと機能を利用できます。
* [Visual Studio のサブスクライバーの特典を有効に](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)します。 Visual Studio サブスクリプションにより、有料の Azure サービスを利用できるクレジットが毎月与えられます。

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

2.  **ファイル** ] メニューをクリックして **新規 > プロジェクト**します。

3.  **新しいプロジェクト** ダイアログ ボックスで、をクリックして **c# > Web > ASP.NET Web アプリケーション**します。 選択できる場合は、 **Visual Basic**します。

3. 確認して **.NET Framework 4.5.2** ターゲット フレームワークとして選択します。

4.  [Azure の Application Insights](app-insights-overview.md) web アプリの可用性、パフォーマンス、および使用状況を監視します。 クリア、 **Application Insights をプロジェクトに追加** やってしたくない場合はチェック ボックスです。

4. アプリケーションの名前 **MyExample**します。

5. Click **OK**.

    ![New Project dialog box](./media/web-sites-dotnet-get-started/GS13newprojdb.png)

5.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、 **MVC** テンプレートです。

    [MVC](http://www.asp.net/mvc) web アプリの開発に ASP.NET フレームワークです。

7. クリックして **認証変更**します。

    ![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started/GS13changeauth.png)

6.  **認証の変更** ダイアログ ボックスで、をクリックして **認証なし**, 、] をクリックし、 **[ok]**します。

    ![[認証なし]](./media/web-sites-dotnet-get-started/GS13noauth.png)

    作成しているサンプル アプリケーションでは、ユーザーはログインできません。  [次のステップ](#next-steps) 」セクションは、認証と承認を実装するチュートリアルにリンクします。

5.  **新しい ASP.NET プロジェクト** ] ダイアログ ボックスで、下にある設定のままに **Microsoft Azure** 変更せずに、クリックして **OK**します。

    ![New ASP.NET Project dialog box](./media/web-sites-dotnet-get-started/GS13newaspnetprojdb.png)

    既定の設定は、Visual Studio が Web プロジェクトのための Azure Web アプリを作成することを指定します チュートリアルの次のセクションでは、Web プロジェクトを新しく作成した Web アプリにデプロイします。

5. Azure にまだサインインしていない場合は、サインインを求めるメッセージが表示されます。 Azure サブスクリプションを管理するために使用するアカウントの ID とパスワードを使用してサインインします。

    サインインしているときに、 **App Service の作成** ] ダイアログ ボックスで、どのようなリソースを作成します。

    ![Signed in to Azure](./media/web-sites-dotnet-get-started/configuresitesettings.png)

3.  **App Service の作成** ] ダイアログ ボックスで、入力、 **Web アプリ名** 内で一意である、 *azurewebsites.net* ドメイン。 たとえば、MyExample という名前の右側に番号を付加して一意にすることができます (例: MyExample810)。 既定の Web 名が作成される場合は、それが一意になるため、そのまま使用することができます。

    入力した名前が既に使用されている場合は、右側に緑色のチェック マークではなく赤色の感嘆符が表示されます。この場合、別の名前を入力する必要があります。

    Azure では、この名前がアプリケーションの URL のプレフィックスとして使用されます。 この名前と完全な URL では *. azurewebsites.net* (] の横に示すように、 **Web アプリ名** テキスト ボックス)。 たとえば、名前が `MyExample810` である場合、URL は `MyExample810.azurewebsites.net` です。 URL は一意であることが必要です。

4. 横に、 **App Service プラン** ドロップダウンをクリック、 **新規...** ] ボタンをクリックします。

     [次のステップ](#next-steps) セクションには App Service プランに関する情報へのリンク。

5. 入力 **MyExamplePlan**, 、またはプラン名には、必要に応じて別の名前。

6.  **リソース グループ** ドロップダウン リストで、新しいリソース グループの名前を入力します。

     [次のステップ](#next-steps) セクションにはリソース グループに関する情報へのリンク。

5. 入力 **MyExampleGroup**, 、またはリソース グループ名、必要に応じて別の名前。

5.  **地域** ドロップダウン リストで、地に最も近い場所を選択します。

    この設定によって、Web アプリが実行される Azure データ センターが指定されます。 このチュートリアルでは、任意のリージョンを選択することができます。任意のリージョンを選択しても、大きな違いはありません。 ただし、運用 web アプリでは、選択して web サーバーを最小化するために、サイトにアクセスしているブラウザーのできるだけ近くに [遅延](http://www.bing.com/search?q=web%20latency%20introduction&qs=n&form=QBRE&pq=web%20latency%20introduction&sc=1-24&sp=-1&sk=&cvid=eefff99dfc864d25a75a83740f1e0090)します。

5. データベース フィールドは変更せずそのままにします。

    このチュートリアルでは、データベースは使用しません。  [次のステップ](#next-steps) 」セクションのデータベースを使用する方法を説明するチュートリアルにリンクします。

6. Click **OK**.

    ![](./media/web-sites-dotnet-get-started/configuresitesettings2.png)

    Visual Studio により、指定したフォルダーに Web プロジェクトが数秒で作成され、指定した Azure リージョンに Web アプリが作成されます。  

     **ソリューション エクスプ ローラー** ] ウィンドウでは、新しいプロジェクトでファイルとフォルダーが表示されます。

    ![ソリューション エクスプローラー
](./media/web-sites-dotnet-get-started/solutionexplorer.png)

     **Azure App Service アクティビティ** ウィンドウは、web アプリが作成されたことを示しています。

    ![Web app created](./media/web-sites-dotnet-get-started/GS13sitecreated1.png)

    Web アプリを表示および **サーバー エクスプ ローラー**します。

    ![Web app created](./media/web-sites-dotnet-get-started/siteinse.png)

## Web アプリにプロジェクトをデプロイする

このセクションでは、図の手順 2 に示すように、Web プロジェクトを Web アプリにデプロイします。

![Create and deploy](./media/web-sites-dotnet-get-started/Create_App.png)

1.  **ソリューション エクスプ ローラー**, をプロジェクトを右クリックして **発行**します。

    ![Choose Publish](./media/web-sites-dotnet-get-started/choosepublish.png)

    数秒で、 **Web の発行** ウィザードが表示されます。 ウィザードが開き、 *発行プロファイル* 、新しい web アプリに web プロジェクトを展開するための設定を持ちます。 クリックすることが別の web アプリにデプロイする場合は、 **プロファイル** 異なるプロファイルを作成するタブをクリックします。 このチュートリアルでは、以前に作成した Web アプリにデプロイする設定を使用します。

8.  **接続** のタブ、 **Web の発行** ウィザード、] をクリックして **次**します。

    ![Successfully validated connection](./media/web-sites-dotnet-get-started/GS13ValidateConnection.png)

10.  **設定** ] タブ、[ **次**します。

    既定値を使用できます **構成** と **ファイル発行オプション**します。

    使用することができます、 **構成** リモート デバッグ用のデバック ビルドを展開するドロップダウンが表示されます。  [次のステップ](#next-steps) 」セクションをリモートでデバッグ モードで Visual Studio を実行する方法を説明するチュートリアルにリンクします。

    ![Settings tab](./media/web-sites-dotnet-get-started/GS13SettingsTab.png)

11.  **プレビュー** ] タブ、[ **発行**します。

    Azure にコピーされるファイルをクリックするを参照する場合 **プレビューの開始** クリックする前に **発行**します。

    ![](./media/web-sites-dotnet-get-started/GS13previewoutput.png)

    クリックすると、 **発行** Visual Studio は、Azure サーバーにファイルをコピーするプロセスを開始します。

     **出力** と **Azure App Service アクティビティ** windows がどのような展開アクションを実行したを表示して、展開が正常に完了を報告します。

    ![Output window reporting successful deployment](./media/web-sites-dotnet-get-started/PublishOutput.png)

    デプロイが成功すると、自動的に既定のブラウザーが開き、デプロイ先の Web アプリの URL にアクセスします。これで、作成したアプリケーションはクラウドで実行されています。 ブラウザー アドレス バーの URL は、インターネットから読み込まれた Web アプリを示します。

    ![Web app running in Azure](./media/web-sites-dotnet-get-started/GS13deployedsite.png)

13. 次のセクションで使用するため、このブラウザー ウィンドウは開いたままにします。

**ヒント:** 有効にすることができます、 **Web 1 クリックして発行** 簡単に展開できるツールバーです。 クリックして **ビュー > ツールバー**, 、し、[ **Web 1 クリックして発行**します。 プロファイルを選択する、ボタンをクリックして、公開、またはボタンをクリックして、ツールバーを使用する、 **Web の発行** ウィザード。

![Web One Click Publish Toolbar](./media/web-sites-dotnet-get-started/weboneclickpublish.png)

## サーバー エクスプローラーでリモート ファイルを開く

テストして、web アプリをデバッグするには、行うことができます、リモート サイトにすぐに一時的な変更を開きでファイルを編集して **サーバー エクスプ ローラー**します。

1.   **サーバー エクスプ ローラー**, に移動 **Azure > App Service > MyExampleGroup**, 、web アプリのノードを展開します。

2. 展開 **ファイル > ビュー > ホーム**, をダブルクリックし、 *Index.cshtml* ファイルです。

    ![](./media/web-sites-dotnet-get-started/indexfileinse.png)

3. `<h1>ASP.NET</h1>` を `<h1>Azure App Service</h1>` に変更します。

4. ファイルを保存します。

5. Azure で実行されているサイトが表示されているブラウザー ウィンドウを更新します。 

    ![](./media/web-sites-dotnet-get-started/afterindexedit.png)

この変更は、デプロイされたサイトには適用されますが、ローカルのプロジェクトには適用されません。 プロジェクトを再デプロイすると、サイトはこの変更を加える前の状態に戻ります。

この機能は便利です [詳細なエラー メッセージを取得するために、Web.config ファイルで customErrors を一時的にオフ](web-sites-dotnet-troubleshoot-visual-studio.md)します。

 **サーバー エクスプ ローラー** ビュー アプリケーション ログをリアルタイムで、アプリケーションの書き込みとも、App Service ノードを右クリックし、web アプリケーション設定、Visual Studio のウィンドウで、リモート デバッグ セッションを開始へのアクセスを取得できます。

![](./media/web-sites-dotnet-get-started/sewebappmenu.png)

詳細については、次を参照してください。 [Visual Studio で web アプリを Azure のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

## Azure ポータルで Web アプリを監視および管理する

 [Azure ポータル](/services/management-portal/) 管理し、Azure のサービスを作成した web アプリなどの監視に使用できる web インターフェイスです。 チュートリアルのこのセクションでは、ポータルで何ができるかを確認できます。

1. ブラウザーでに移動 [https://portal.azure.com](https://portal.azure.com), 、Azure の資格情報を使用してサインインします。

2. クリックして **App Services**, 、web アプリの名前をクリックします。

     **Web アプリ** ブレードには、設定と web アプリの使用状況の統計の概要が表示されます。

    ![Web app blade](./media/web-sites-dotnet-get-started/portaldashboard.png)

    この時点では、Web アプリにはトラフィックがそれほど存在しないため、グラフには何も表示されない可能性があります。 アプリケーションにアクセスし、何回かページを更新してから、ポータル ページを更新すると、統計が表示されます。

3. クリックして **設定** web アプリを構成するための詳細オプションを参照します。

    ![Click Settings](./media/web-sites-dotnet-get-started/portaldashboard2.png)

    設定のタイプの一覧が表示されます。

    ![](./media/web-sites-dotnet-get-started/portalconfigure1.png)

4. をクリックして **アプリケーション設定** にポータルで構成できる設定の種類の例を参照してください。

    たとえば、web アプリに使用される .NET のバージョンを制御などの機能を有効にする [Websocket](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/), 、設定と [接続文字列の値](/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)です。

    ![Portal web app configure tab](./media/web-sites-dotnet-get-started/portalconfigure2.png)

これらは、ポータル機能の一部にすぎません。 新しい Web アプリを作成したり、既存の Web アプリを削除したり、Web アプリを停止して再起動したり、データベースや仮想マシンなど、その他の種類の Azure サービスを管理したりできます。  

## 次のステップ

このチュートリアルでは、Azure の Web アプリに単純な Web アプリケーションを作成してデプロイする方法について説明しました。 Azure App Service の Web アプリの詳細についての関連トピックとリソースがあります。

* データベースと承認の機能を追加する方法

    データベースにアクセスし、承認されたユーザーへの一部のアプリケーション機能を制限する方法を説明するチュートリアルについては、次を参照してください。 [メンバーシップ、OAuth、SQL データベースとの安全な ASP.NET MVC アプリケーションを Azure web アプリにデプロイ](/develop/net/tutorials/web-site-with-sql-database/)します。 そのチュートリアルの MVC 5 の場合は、ある程度の知識を前提としていますMVC 5 に慣れていない場合は、次を参照してください。 [ASP.NET MVC 5 の概要](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started)します。

* Web プロジェクトをデプロイする他の方法

    Visual Studio を使用するか、web アプリに web プロジェクトを展開するには、その他の方法については [展開を自動化する](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery) から、 [ソース管理システム](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control), を参照してください [Azure web アプリを展開する方法](web-sites-deploy.md)します。

    Visual Studio により、デプロイを自動化するために使用可能な Windows PowerShell スクリプトも生成できます。 詳細については、次を参照してください。 [(Azure で構築実際のクラウド アプリケーション) を自動化してすべて](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything)です。

* Web アプリのトラブルシューティングの方法

    Visual Studio には、リアルタイムで生成される Azure ログの表示を容易にする機能があります。 Azure では、リモートでデバッグ モードを使用して実行することもできます。 詳細については、次を参照してください。 [Visual Studio で web アプリを Azure のトラブルシューティング](web-sites-dotnet-troubleshoot-visual-studio.md)します。

* カスタム ドメイン名および SSL の追加方法

    SSL および独自のドメイン (contoso.azurewebsites.net の代わりに www.contoso.com など) の使用方法については、次のリソースを参照してください。

    * [Azure App Service のカスタム ドメイン名の構成](web-sites-custom-domain-name.md)
    * [Azure の Web サイトでの HTTPS の有効化](web-sites-configure-ssl-certificate.md)

* リアルタイム機能 (チャットなど) の追加方法

    Web アプリでリアルタイム機能 (チャット サービス、ゲーム、株価情報など) を含める場合を使用して最適なパフォーマンスを実現できます [ASP.NET SignalR](http://www.asp.net/signalr) で、 [Websocket](/blog/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites/) 転送方法。 詳細については、次を参照してください。 [Azure web apps で使用して SignalR](http://www.asp.net/signalr/overview/signalr-20/getting-started-with-signalr-20/using-signalr-with-windows-azure-web-sites)します。

* Web アプリケーションに対する App Service、Azure Cloud Services、および Azure Virtual Machines の選択方法

    Azure では、このチュートリアルで示したように App Service Web Apps で、または Cloud Services や Virtual Machines で、Web アプリケーションを実行することができます。 詳細については、次を参照してください。 [Azure web アプリ、クラウド サービス、および Vm: どれを使用する場合ですか?](/manage/services/web-sites/choose-web-app-service/)します。

* [App Service プランを選択または作成する方法](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

* [リソース グループを選択または作成する方法](../azure-preview-portal-using-resource-groups.md)

## 変更内容
* Web サイトから App Service への変更ガイド 』 を参照してください。 [Azure App Service と既存の Azure サービス](http://go.microsoft.com/fwlink/?LinkId=529714)します。


