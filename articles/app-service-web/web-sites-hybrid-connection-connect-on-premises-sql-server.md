<properties
    pageTitle="ハイブリッド接続を使用して Azure App Service の Web アプリからオンプレミスの SQL Server に接続する"
    description="Microsoft Azure で Web アプリを作成し、それをオンプレミスの SQL Server データベースに接続します。"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/13/2015"
    ms.author="cephalin"/>

# ハイブリッド接続を使用して Azure App Service の Web アプリからオンプレミスの SQL Server に接続する

ハイブリッド接続が接続できる [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) 、静的 TCP ポートを使用する内部設置型リソースへの Web アプリです。 サポートされているリソースには、Microsoft SQL Server、MySQL、HTTP Web APIs、Mobile Services、およびほとんどのカスタム Web サービスが含まれます。

このチュートリアルでは、App Service web アプリを作成する方法について学びますが、 [Azure プレビュー](http://go.microsoft.com/fwlink/?LinkId=529715), 、新しいハイブリッド接続機能を使用して、ローカル オンプレミス SQL Server データベースに web アプリを接続、ハイブリッド接続を使用して App Service web アプリへのアプリケーションを展開する、単純な ASP.NET アプリケーションを作成します。 完成した Azure の Web アプリでは、ユーザー資格情報をオンプレミスのメンバーシップ データベースに保存します。 このチュートリアルは、Azure または ASP.NET を使用した経験がない読者を対象に作成されています。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。
>
>ハイブリッド接続機能の Web Apps 部分はでのみ使用できますが、 [Azure ポータル](https://portal.azure.com)します。 BizTalk サービスでの接続を作成するを参照してください。 [ハイブリッド接続](http://go.microsoft.com/fwlink/p/?LinkID=397274)します。  

## 前提条件 ##

このチュートリアルを完了するには、以下の製品が必要です。 すべて無料版を利用できるため、Azure の開発を完全に無料で始めることができます。

- **Azure サブスクリプション** - 無料サブスクリプションについてを参照してください [Azure 無料試用版](/pricing/free-trial/)します。

- **Visual Studio 2013** を参照してください、Visual Studio 2013 の無料評価版をダウンロードする - [Visual Studio のダウンロード](http://www.visualstudio.com/downloads/download-visual-studio-vs)します。 これをインストールしてから、次に進みます。

- **Microsoft .NET Framework 3.5 Service Pack 1** -オペレーティング システムが Windows 8.1、Windows Server 2012 R2、Windows 8、Windows Server 2012、Windows 7 または Windows Server 2008 R2 の場合は、有効にできますこのコントロール パネル > プログラムと機能 > [Windows の機能を切り替えます。 それ以外の場合、ダウンロードすることから、 [Microsoft ダウンロード センター](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22)します。

- **SQL Server 2014 Express with Tools** -Microsoft SQL Server Express の無料版をダウンロード、 [Microsoft Web プラットフォーム データベースのページ](http://www.microsoft.com/web/platform/database.aspx)します。 選択、 **Express** (LocalDB ではない) のバージョン。  **Express with Tools** バージョンには、このチュートリアルで使用する SQL Server Management Studio が含まれています。

- **SQL Server Management Studio Express** - これは SQL Server 2014 express with Tools のダウンロード、上記で説明したが、個別にインストールする必要がある場合は、ダウンロードしてインストールしてから、 [SQL Server Express のダウンロード ページ](http://www.microsoft.com/web/platform/database.aspx)します。

このチュートリアルでは、Azure サブスクリプションを持っている、Visual Studio 2013 をインストールしている、および .NET Framework 3.5 をインストールしているか有効にしていると想定しています。 このチュートリアルでは、Azure のハイブリッド接続機能 (静的 TCP ポートの既定のインスタンス) を使用する構成に SQL Server 2014 Express をインストールする方法を説明します。 SQL Server をインストールしていない場合は、チュートリアルを開始する前に、上記の場所から SQL Server 2014 Express with Tools をダウンロードしてください。

### メモ ###
ハイブリッド接続でオンプレミスの SQL Server または SQL Server Express のデータベースを使用するには、TCP/IP が静的ポートで有効になっている必要があります。 SQL Server の既定のインスタンスは静的ポート 1433 を使用しますが、名前付きインスタンスは静的ポート 1433 を使用しません。

オンプレミス ハイブリッド接続マネージャーのエージェントをインストールするコンピューターの条件は、次のとおりです。

- 次を使用して Azure に送信接続できること

Port|理由
---|---
80|**必要な** の証明書の検証および必要に応じてデータ接続用の HTTP ポート。
使用します|**省略可能な** データ接続用です。 443 への送信接続ができない場合は、TCP ポート 80 を使用します。
5671 と 9352|**推奨** が、データ接続用の省略可能です。 このモードでは通常、スループットが高くなります。 これらのポートへの送信接続ができない場合は、TCP ポート 443 を使用します。
- 到達できる必要があります、 *hostname*:*portnumber* 内部設置型リソースのです。

この記事の手順では、オンプレミスのハイブリッド接続のエージェントをホストするコンピューターからブラウザーを使用していると想定しています。

スキップ」から開始する場合は、既にある SQL Server を構成およびを上記の条件を満たす環境をインストールできます [オンプレミス SQL Server データベースを作成する](#CreateSQLDB)です。

<a name="InstallSQL"></a>
## A. SQL Server Express をインストールし、TCP/IP を有効にして、オンプレミスの SQL Server データベースを作成する ##

このセクションでは、Web アプリケーションが Azure プレビュー環境を使用するために、SQL Server Express をインストールし、TCP/IP を有効にして、データベースを作成する方法を説明します。

### SQL Server Express をインストールする ###

1. SQL Server Express をインストールするには、実行、 **SQLEXPRWT_x64_ENU.exe** または **SQLEXPR_x86_ENU.exe** ダウンロードしたファイルです。 SQL Server インストール センター ウィザードが表示されます。

    ![SQL Server のインストール][SQLServerInstall]

2. 選択 **SQL Server の新規スタンドアロン インストールまたは既存のインストールに機能の追加**します。 次の手順については、表示されるまで、既定の選択と設定を受け入れ、 **インスタンスの構成** ページです。

3.  **インスタンスの構成** ページで、選択 **既定のインスタンス**します。

    ![既定のインスタンスの選択][ChooseDefaultInstance]

    既定では、SQL Server の既定のインスタンスが静的ポート 1433 の SQL Server クライアントから要求をリッスンします。ハイブリッド接続機能には静的ポート 1433 が必要です。 名前付きインスタンスは動的ポートと UDP を使用します。これはハイブリッド接続ではサポートされません。

4. 既定値を受け入れ、 **サーバー構成** ページです。

5.  **データベース エンジンの構成** ] ページで、[ **認証モード**, 、選択 **混合モード (SQL Server 認証と Windows 認証)**, 、パスワードを指定します。

    ![混合モードの選択][ChooseMixedMode]

    このチュートリアルでは、SQL Server 認証を使用します。 後で必要になるため、入力したパスワードを忘れないでください。

6. ウィザードの残りの手順を実行し、インストールを完了します。

### TCP/IP を有効にする ###
TCP/IP を有効にするには、SQL Server Express をインストールした際にインストールされた SQL Server 構成マネージャーを使用します。 手順に従います [for SQL Server の TCP/IP ネットワーク プロトコルの有効化](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) 続行する前にします。

<a name="CreateSQLDB"></a>
### オンプレミスの SQL Server データベースを作成する ###

Visual Studio Web アプリケーションには、Azure がアクセスできるメンバーシップ データベースが必要です。 これには、(既定で MVC テンプレートが使用する LocalDB データベースではなく) SQL Server または SQL Server Express データベースが必要なため、次にメンバーシップ データベースを作成します。

1. SQL Server Management Studio で、インストールした SQL Server に接続します (場合、 **サーバーへの接続** に移動し、ダイアログが自動的に表示されない **オブジェクト エクスプ ローラー** 左のウィンドウで **接続**, 、] をクリックし、 **データベース エンジン**.)
    ![サーバーへの接続][SSMSConnectToServer]

     **サーバーの種類**, 、選択 **データベース エンジン**します。  **サーバー名**, 、使用する **localhost** またはを使用しているコンピューターの名前。 選択 **SQL Server 認証**, 、sa のユーザー名および先ほど作成したパスワードでログインしとします。

2. SQL Server Management Studio を使用して、新しいデータベースを作成するには、右 **データベース** オブジェクト エクスプ ローラーで、をクリックしてに **新しいデータベース**します。

    ![新しいデータベースの作成][SSMScreateNewDB]

3.  **新しいデータベース** ダイアログ ボックスで、データベース名として「membershipdb」と入力し、、 **OK**します。

    ![データベース名を指定する][SSMSprovideDBname]

    この時点では、データベースに変更を加えることはできないことに注意してください。 メンバーシップ情報は、後で Web アプリケーションを実行すると、自動的に追加されます。

4. 展開する場合は、オブジェクト エクスプ ローラーで **データベース**, 、メンバーシップ データベースが作成されたことが表示されます。

    ![作成した [membershipdb][SSMSMembershipDBCreated]

<a name="CreateSite"></a>
## B. Azure ポータルで Web アプリを作成する ##

> [AZURE.NOTE] このチュートリアルで使用する場合、Azure ポータルで web アプリが既に場合スキップする [ハイブリッド接続および BizTalk サービスを作成する](#CreateHC) から開始してください。

1.  [Azure ポータル](https://portal.azure.com), 、クリックして **新規** > **Web + モバイル** > **Web アプリ**します。

    ![新しいボタン][新規]

2. Web アプリを構成し、 **作成**します。

    ![Web サイト名][WebsiteCreationBlade]

3. しばらくすると、Web アプリケーションが作成され、Web アプリケーションのブレードが表示されます。 ブレードは縦方向にスクロールできるダッシュボードで、Web アプリを管理できます。

    ![Web サイトを実行している][WebSiteRunningBlade]

    クリックすると、web アプリがライブことを確認する、 **参照** 既定のページを表示するアイコン。

次に、Web アプリケーションに対するハイブリッド接続と BizTalk サービスを作成します。

<a name="CreateHC"></a>
## C. ハイブリッド接続および BizTalk サービスを作成する ##

1. バックアップ、ポータルでの設定に移動し、をクリックして **ネットワーク** > **ハイブリッド接続エンドポイントを構成する**です。

    ![ハイブリッド接続][CreateHCHCIcon]

2. [ハイブリッド接続] ブレードをクリックして **追加** > **新しいハイブリッド接続**します。

3.  **ハイブリッド接続を作成** ブレード。
    -  **名前**, 、接続の名前を指定します。
    -  **Hostname**, 、SQL Server ホスト コンピューターのコンピューターの名前を入力します。
    -  **ポート**, 、1433 (SQL Server の既定のポート) を入力します。
    - クリックして **BizTalk サービス** > **新しい BizTalk サービス** し、BizTalk サービスの名前を入力します。

    ![ハイブリッド接続の作成][TwinCreateHCBlades]

5. クリックして **OK** 2 回クリックします。

    処理が完了したら、 **通知** 領域が緑色の点滅 **成功** と **ハイブリッド接続** ブレードが新しいハイブリッド接続ステータスが表示されます **接続されていない**します。

    ![作成した 1 つのハイブリッド接続][CreateHCOneConnectionCreated]

これで、クラウド ハイブリッド接続のインフラストラクチャの重要な部分が完了しました。 次に、対応するオンプレミスの部分を作成します。

<a name="InstallHCM"></a>
## D. オンプレミス ハイブリッド接続マネージャーをインストールして接続を完了する ##

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

これで、ハイブリッド接続のインフラストラクチャが完成しました。この接続を使用する Web アプリケーションを作成できます。

<a name="CreateASPNET"></a>
## E. 基本的な ASP.NET Web プロジェクトを作成し、データベース接続文字列を編集して、プロジェクトをローカルで実行する ##

### 基本的な ASP.NET プロジェクトを作成する ###
1. Visual Studio での **ファイル** ] メニューの [新しいプロジェクトを作成します。

    ![新しい Visual Studio project][HCVSNewProject]

2.  **テンプレート** のセクション、 **新しいプロジェクト** ダイアログで、 **Web** 選択 **ASP.NET Web アプリケーション**, 、] をクリックし、 **[ok]**します。

    ![ASP.NET Web アプリケーションの選択][HCVSChooseASPNET]

3.  **新しい ASP.NET プロジェクト** ] ダイアログ ボックスで選択 **MVC**, 、] をクリックし、 **[ok]**します。

    ![MVC を選択][HCVSChooseMVC]

4. プロジェクトが作成されると、アプリケーションの readme ページが表示されます。 まだ Web プロジェクトを実行しないでください。

    ![リリース ノート ページ][HCVSReadmePage]

### アプリケーションのデータベース接続文字列を編集する ###

この手順では、ローカル SQL Server Express データベースの場所をアプリケーションに伝える接続文字列を編集します。 接続文字列は、アプリケーションの Web.config ファイルにあり、アプリケーションの構成情報が含まれています。

> [AZURE.NOTE] アプリケーションが SQL Server Express ではなく、Visual Studio の既定の LocalDB で作成したデータベースを使用するためには、プロジェクトを実行する前に、この手順を完了したことが重要です。

1. ソリューション エクスプローラーで、Web.config ファイルをダブルクリックします。

    ![Web.config][HCVSChooseWebConfig]

2. 編集、 **connectionStrings** 構文を次の例では、ローカル コンピューターに SQL Server データベースを指すセクション。

    ![接続文字列][HCVSConnectionString]

    接続文字列を作成する際には、次の事項に留意してください。

    - 既定のインスタンスではなく、名前付きインスタンス (YourServer\SQLEXPRESS など) に接続している場合は、静的ポートを使用するように SQL Server を構成する必要があります。 静的ポートを構成する方法の詳細については、次を参照してください。 [を特定のポートでリッスンするように SQL Server を構成する方法](http://support.microsoft.com/kb/823938)します。 既定では、名前付きインスタンスは動的ポートと UDP を使用します。これはハイブリッド接続ではサポートされません。

    - ポート (例に示すように既定では 1433) を接続文字列に指定することをお勧めします。これにより、ローカル SQL Server で TCP が有効になり、正しいポートが使用されます。

    - SQL Server 認証を使用して接続するのを忘れないでください。接続文字列にユーザー ID とパスワードを指定します。

3. をクリックして **保存** Visual studio を Web.config ファイルを保存します。

### プロジェクトをローカルで実行し、新しいユーザーを登録する ###

1. ここで、[デバッグ] の [参照] ボタンをクリックして、新しい Web プロジェクトをローカルで実行します。 この例では、Internet Explorer を使用します。

    ![プロジェクトを実行する][HCVSRunProject]

2. 既定の web ページの右上、順にクリックして **登録** 新しいアカウントを登録します。

    ![新しいアカウントの登録][HCVSRegisterLocally]

3. ユーザー名とパスワードを入力します。

    ![ユーザー名とパスワードの入力][HCVSCreateNewAccount]

    これで、アプリケーションのメンバーシップ情報を保持するローカル SQL Server にデータベースが自動的に作成されます。 一方のテーブル (**dbo です。AspNetUsers**) が保持している web アプリ ユーザー資格情報入力したものです。 このテーブルは後でチュートリアルで使用されます。

4. 既定の Web ページのブラウザー ウィンドウを閉じます。 これで、Visual Studio でアプリケーションが停止します。

これで、アプリケーションを Azure に発行してテストする次の手順に進む準備ができました。

<a name="PubNTest"></a>
## F. Web アプリケーションを Azure に発行してテストする ##

アプリケーションを App Service Web アプリに発行し、テストします。ローカル コンピューターのデータベースに Web アプリを接続する際に、前に構成したハイブリッド接続がどのように使用されるのかを確認します。

### Web アプリケーションを発行する ###

1. Azure ポータルの App Service Web アプリに対する発行プロファイルをダウンロードできます。 [Web アプリのブレードで、次のようにクリックします。 **発行プロファイルを取得**, 、お使いのコンピューターに、ファイルを保存します。

    ![発行プロファイルのダウンロード設定][PortalDownloadPublishProfile]

    次に、このファイルを Visual Studio Web アプリケーションにインポートします。

2. Visual Studio では、ソリューション エクスプ ローラーでプロジェクト名を右クリックして **発行**します。

    ![公開] を選択][HCVSRightClickProjectSelectPublish]

3.  **Web の発行** ダイアログで、 **プロファイル** ] タブで、選択 **インポート**します。

    ![インポート][HCVSPublishWebDialogImport]

4. ダウンロードした発行プロファイルを参照を選択してクリックして **OK**します。

    ![プロファイルを参照する][HCVSBrowseToImportPubProfile]

5. 発行情報がインポートされ、上の表示、 **接続** 、ダイアログ ボックスのタブをクリックします。

    ![発行] をクリック][HCVSClickPublish]

    クリックして **発行**します。

    発行が完了すると、ブラウザーが起動し、見慣れた ASP.NET アプリケーションが表示されますが、Azure クラウドでライブになっているという点が異なります。

次に、ライブ Web アプリケーションを使用して、ハイブリッド接続の実行を確認します。

### 完成した Web アプリケーションを Azure でテストする ###

1. 上部にある Azure の web ページの右選択 **ログイン**します。

    ![テスト ログ][HCTestLogIn]

2. App Service Web アプリが、ローカル コンピューターの Web アプリケーションのメンバーシップ データベースに接続されています。 これを確認するには、前にローカル データベースに入力した資格情報でログインします。

    ![こんにちはあいさつ文][HCTestHelloContoso]

3. 新しいハイブリッド接続を詳細にテストするには、Azure Web アプリケーションからログオフし、別のユーザーとして登録します。 新しいユーザー名とパスワードを入力し、 **登録**します。

    ![テストは、別のユーザーを登録][HCTestRegisterRelecloud]

4. ハイブリッド接続を通じて新しいユーザーの資格情報がローカル データベースに保存されたことを確認するには、ローカル コンピューターで SQL Management Studio を開きます。 オブジェクト エクスプ ローラーで、 **[membershipdb]** データベースを展開し、展開 **テーブル**します。 右クリックし、 **dbo です。AspNetUsers** メンバーシップ テーブルが表示され選択 **[上位 1000年行** 結果を表示します。

    ![結果を表示][HCTestSSMSTree]

5. ローカル メンバーシップ テーブルには、ローカルで作成したメンバーシップと、Azure クラウドで作成したメンバーシップの両方が表示されています。 クラウドで作成したメンバーシップは、Azure のハイブリッド接続機能でオンプレミスのデータベースに保存されています。

    ![内部設置型データベースの登録済みユーザー][HCTestShowMemberDb]

これで、Azure クラウドの Web アプリとオンプレミスの SQL Server データベース間のハイブリッド接続を使用する ASP.NET Web アプリケーションを作成し、デプロイすることができました。 ご利用ありがとうございます。

## 関連項目 ##
[ハイブリッド接続の概要](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh Twist によるハイブリッド接続の紹介 (チャネル 9 ビデオ)](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[ハイブリッド接続の概要](/services/biztalk-services/)

[BizTalk サービス: [ダッシュボード]、[監視]、[スケール]、[構成]、および [ハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs/)

[シームレスなアプリケーションの移植性を使用して実際のハイブリッド クラウドをビルドする (チャネル 9 ビデオ)](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[ハイブリッド接続を使用して Azure のモバイル サービスから内部設置型の SQL Server に接続する](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)

[Connect to an on-premises SQL Server from Azure Mobile Services using Hybrid Connections (ハイブリッド接続を使用して Azure Mobile Services から内部設置型の SQL Server に接続する) (チャネル 9 ビデオ)](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

[ASP.NET の Id の概要](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]


<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png

