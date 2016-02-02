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

## 前提条件

このチュートリアルを完了するには、以下の製品が必要です。 すべて無料版を利用できるため、Azure の開発を完全に無料で始めることができます。

- **Azure サブスクリプション** - 無料サブスクリプションについてを参照してください [Azure 無料試用版](/pricing/free-trial/)します。

- **Visual Studio 2013** を参照してください、Visual Studio 2013 の無料評価版をダウンロードする - [Visual Studio のダウンロード](http://www.visualstudio.com/downloads/download-visual-studio-vs)します。 これをインストールしてから、次に進みます。

- **Microsoft .NET Framework 3.5 Service Pack 1** - オペレーティング システムが Windows 8.1、Windows Server 2012 R2、Windows 8、Windows Server 2012、Windows 7、または Windows Server 2008 R2 の場合は、[コントロール パネル] > [プログラムと機能] > [Windows の機能の有効化または無効化] の順に選択してこれを実行できます。それ以外の場合、ダウンロードすることから、 [Microsoft ダウンロード センター](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22)します。

- **SQL Server 2014 Express with Tools** -Microsoft SQL Server Express の無料版をダウンロード、 [Microsoft Web プラットフォーム データベースのページ](http://www.microsoft.com/web/platform/database.aspx)します。 **Express** (LocalDB ではない) 版を選択します。 **Express with Tools** 版には、このチュートリアルで使用する SQL Server Management Studio が含まれています。

- **SQL Server Management Studio Express** - これは SQL Server 2014 express with Tools のダウンロード、上記で説明したが、個別にインストールする必要がある場合は、ダウンロードしてインストールしてから、 [SQL Server Express のダウンロード ページ](http://www.microsoft.com/web/platform/database.aspx)します。

このチュートリアルでは、Azure サブスクリプションを持っている、Visual Studio 2013 をインストールしている、および .NET Framework 3.5 をインストールしているか有効にしていると想定しています。 このチュートリアルでは、Azure のハイブリッド接続機能 (静的 TCP ポートの既定のインスタンス) を使用する構成に SQL Server 2014 Express をインストールする方法を説明します。 SQL Server をインストールしていない場合は、チュートリアルを開始する前に、上記の場所から SQL Server 2014 Express with Tools をダウンロードしてください。

### メモ

ハイブリッド接続でオンプレミスの SQL Server または SQL Server Express のデータベースを使用するには、TCP/IP が静的ポートで有効になっている必要があります。 SQL Server の既定のインスタンスは静的ポート 1433 を使用しますが、名前付きインスタンスは静的ポート 1433 を使用しません。

オンプレミス ハイブリッド接続マネージャーのエージェントをインストールするコンピューターの条件は、次のとおりです。

- 次を使用して Azure に送信接続できること

 Port| 理由
---|---
 80| 証明書の検証用の HTTP ポート (**必須**)。必要に応じて、データ接続に使用することもできます。
 使用します| データ接続用 (**任意**)。443 への送信接続ができない場合は、TCP ポート 80 を使用します。
 5671 と 9352| データ接続用の**推奨**ポート。ただし、必須ではありません。このモードでは通常、スループットが高くなります。これらのポートへの送信接続ができない場合は、TCP ポート 443 を使用します。

- オンプレミス リソースの *hostname*:*portnumber* に到達できること

この記事の手順では、オンプレミスのハイブリッド接続のエージェントをホストするコンピューターからブラウザーを使用していると想定しています。

スキップ」から開始する場合は、既にある SQL Server を構成およびを上記の条件を満たす環境をインストールできます [オンプレミス SQL Server データベースを作成する](#CreateSQLDB)します。

<a name="InstallSQL"></a>
## A.SQL Server Express をインストールし、TCP/IP を有効にして、オンプレミスの SQL Server データベースを作成する

このセクションでは、Web アプリケーションが Azure プレビュー環境を使用するために、SQL Server Express をインストールし、TCP/IP を有効にして、データベースを作成する方法を説明します。

### SQL Server Express をインストールする

1. SQL Server Express をインストールするには、ダウンロードした **SQLEXPRWT_x64_ENU.exe** または **SQLEXPR_x86_ENU.exe** ファイルを実行します。 SQL Server インストール センター ウィザードが表示されます。

    ![SQL Server Install][sqlserverinstall]

2. **[SQL Server の新規スタンドアロン インストールを実行するか、既存のインストールに機能を追加します]** をクリックします。 指示に従って、**[インスタンスの構成]** ページが表示されるまで、既定の選択と設定を使用します。

3. **[インスタンスの構成]** ページで、**[既定のインスタンス]** を選択します。

    ![Choose Default Instance][choosedefaultinstance]

    既定では、SQL Server の既定のインスタンスが静的ポート 1433 の SQL Server クライアントから要求をリッスンします。ハイブリッド接続機能には静的ポート 1433 が必要です。 名前付きインスタンスは動的ポートと UDP を使用します。これはハイブリッド接続ではサポートされません。

4. **[サーバーの構成]** ページでは既定値をそのまま使用します。

5. **[データベース エンジンの構成]** ページの **[認証モード]** で、**[混合モード (SQL Server 認証と Windows 認証)]** を選択して、パスワードを入力します。

    ![Choose Mixed Mode][choosemixedmode]

    このチュートリアルでは、SQL Server 認証を使用します。 後で必要になるため、入力したパスワードを忘れないでください。

6. ウィザードの残りの手順を実行し、インストールを完了します。

### TCP/IP を有効にする

TCP/IP を有効にするには、SQL Server Express をインストールした際にインストールされた SQL Server 構成マネージャーを使用します。 手順に従います [for SQL Server の TCP/IP ネットワーク プロトコルの有効化](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) 続行する前にします。

<a name="CreateSQLDB"></a>
### オンプレミスの SQL Server データベースを作成する

Visual Studio Web アプリケーションには、Azure がアクセスできるメンバーシップ データベースが必要です。 これには、(既定で MVC テンプレートが使用する LocalDB データベースではなく) SQL Server または SQL Server Express データベースが必要なため、次にメンバーシップ データベースを作成します。

1. SQL Server Management Studio で、インストールした SQL Server に接続します (**[サーバーへの接続]** ダイアログが自動的に表示されない場合は、左パネルの **[オブジェクト エクスプローラー]** に移動し、**[接続]**、次に **[データベース エンジン]** をクリックします)。
    ![サーバーへの接続][ssmsconnecttoserver]

    **[サーバーの種類]** には、**[データベース エンジン]** を選択します。 **[サーバー名]** には、**[localhost]**、または使用しているコンピューターの名前を使用します。 **[SQL Server 認証]** を選択し、前に作成したユーザー名とパスワードでログインします。

2. SQL Server Management Studio を使用して新しいデータベースを作成するには、オブジェクト エクスプローラーで **[データベース]** を右クリックしてから、**[新しいデータベース]** をクリックします。

    ![Create new database][ssmscreatenewdb]

3. **[新しいデータベース]** ダイアログで、データベース名に「MembershipDB」と入力し、**[OK]** をクリックします。

    ![Provide database name][ssmsprovidedbname]

    この時点では、データベースに変更を加えることはできないことに注意してください。 メンバーシップ情報は、後で Web アプリケーションを実行すると、自動的に追加されます。

4. オブジェクト エクスプローラーで、**[データベース]** を展開すると、メンバーシップ データベースが作成されたことが示されます。

    ![作成された MembershipDB][ssmsmembershipdbcreated]

<a name="CreateSite"></a>
## B.Azure ポータルで Web アプリを作成する

> [AZURE.NOTE] このチュートリアルで使用する場合、Azure ポータルで web アプリが既に場合スキップする [ハイブリッド接続および BizTalk サービスを作成する](#CreateHC) から開始してください。

1. [Azure ポータル](https://portal.azure.com), 、クリックして **新規** > **Web + モバイル** > **Web アプリ**します。

    ![[新規] ボタン][new]

2. Web アプリを構成し、**[作成]** をクリックします。

    ![Web サイト名][websitecreationblade]

3. しばらくすると、Web アプリケーションが作成され、Web アプリケーションのブレードが表示されます。 ブレードは縦方向にスクロールできるダッシュボードで、Web アプリを管理できます。

    ![Web サイト実行][websiterunningblade]

    Web アプリがライブかどうかを確認するには、**[参照]** アイコンをクリックして、既定のページを表示します。

次に、Web アプリケーションに対するハイブリッド接続と BizTalk サービスを作成します。

<a name="CreateHC"></a>
## C.ハイブリッド接続および BizTalk サービスを作成する

1. ポータルの設定に戻り、**[ネットワーク]** > **[ハイブリッド接続エンドポイントを構成する]** をクリックします。

    ![ハイブリッド接続][createhchcicon]

2. [ハイブリッド接続] ブレードで、**[追加]** > **[新しいハイブリッド接続]** をクリックします。

3. **[ハイブリッド接続の作成]** ブレードで、次の手順を実行します。
    - **[名前]** に、接続の名前を入力します。
    - **[ホスト名]** には、SQL Server ホスト コンピューターのコンピューター名を入力します。
    - **[ポート]** には、1433 (SQL Server の既定のポート) を入力します。
    - **[Biz Talk Service]**、**[新しい BizTalk サービス]** の順にクリックし、BizTalk サービスの名前を入力します。

    ![Create a hybrid connection][twincreatehcblades]

5. **[OK]** を 2 回クリックします。

    処理が完了すると、**[通知]** 領域に緑色の "**SUCCESS**" という文字が点滅します。**[ハイブリッド接続]** ブレードには、状態が **[未接続]** の新しいハイブリッド接続が表示されます。

    ![1 つのハイブリッド接続が作成された][createhconeconnectioncreated]

これで、クラウド ハイブリッド接続のインフラストラクチャの重要な部分が完了しました。 次に、対応するオンプレミスの部分を作成します。

<a name="InstallHCM"></a>
## D.オンプレミスのハイブリッド接続マネージャーをインストールして接続を完了する

[AZURE.INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

これで、ハイブリッド接続のインフラストラクチャが完成しました。この接続を使用する Web アプリケーションを作成できます。

<a name="CreateASPNET"></a>
## E.基本的な ASP.NET Web プロジェクトを作成し、データベース接続文字列を編集して、プロジェクトをローカルで実行する

### 基本的な ASP.NET プロジェクトを作成する

1. Visual Studio の **[ファイル]** メニューで、新しいプロジェクトを作成します。

    ![New Visual Studio project][hcvsnewproject]

2. **[新しいプロジェクト]** ダイアログの **[テンプレート]** セクションで、**[Web]**、**[ASP.NET Web アプリケーション]** の順に選択して、**[OK]** をクリックします。

    ![Choose ASP.NET Web Application][hcvschooseaspnet]

3. **[新しい ASP.NET プロジェクト]** ダイアログで、**[MVC]** を選択し、**[OK]** をクリックします。

    ![Choose MVC][hcvschoosemvc]

4. プロジェクトが作成されると、アプリケーションの readme ページが表示されます。 まだ Web プロジェクトを実行しないでください。

    ![Readme page][hcvsreadmepage]

### アプリケーションのデータベース接続文字列を編集する

この手順では、ローカル SQL Server Express データベースの場所をアプリケーションに伝える接続文字列を編集します。 接続文字列は、アプリケーションの Web.config ファイルにあり、アプリケーションの構成情報が含まれています。
> [AZURE.NOTE] アプリケーションで、Visual Studio の既定の LocalDB ではなく、SQL Server Express で作成したデータベースを使用するためには、プロジェクトを実行する前にこの手順を完了することが重要です。

1. ソリューション エクスプローラーで、Web.config ファイルをダブルクリックします。

    ![Web.config][hcvschoosewebconfig]

2. **[connectionStrings]** セクションを編集して、ローカル コンピューターの SQL Server データベースを指すようにします。次の例の構文を使用します。

    ![Connection string][hcvsconnectionstring]

    接続文字列を作成する際には、次の事項に留意してください。

    - 既定のインスタンスではなく、名前付きインスタンス (YourServer\SQLEXPRESS など) に接続している場合は、静的ポートを使用するように SQL Server を構成する必要があります。 静的ポートを構成する方法の詳細については、次を参照してください。 [を特定のポートでリッスンするように SQL Server を構成する方法](http://support.microsoft.com/kb/823938)します。 既定では、名前付きインスタンスは動的ポートと UDP を使用します。これはハイブリッド接続ではサポートされません。

    - ポート (例に示すように既定では 1433) を接続文字列に指定することをお勧めします。これにより、ローカル SQL Server で TCP が有効になり、正しいポートが使用されます。

    - SQL Server 認証を使用して接続するのを忘れないでください。接続文字列にユーザー ID とパスワードを指定します。

3. Visual Studio で **[保存]** をクリックして、Web.config ファイルを保存します。

### プロジェクトをローカルで実行し、新しいユーザーを登録する

1. ここで、[デバッグ] の [参照] ボタンをクリックして、新しい Web プロジェクトをローカルで実行します。 この例では、Internet Explorer を使用します。

    ![Run project][hcvsrunproject]

2. 既定の Web ページの右上で、**[登録]** をクリックして新しいアカウントを登録します。

    ![Register a new account][hcvsregisterlocally]

3. ユーザー名とパスワードを入力します。

    ![Enter user name and password][hcvscreatenewaccount]

    これで、アプリケーションのメンバーシップ情報を保持するローカル SQL Server にデータベースが自動的に作成されます。 テーブルの 1 つ (**dbo.AspNetUsers**) に、入力した Web アプリ ユーザー資格情報が保持されます。 このテーブルは後でチュートリアルで使用されます。

4. 既定の Web ページのブラウザー ウィンドウを閉じます。 これで、Visual Studio でアプリケーションが停止します。

これで、アプリケーションを Azure に発行してテストする次の手順に進む準備ができました。

<a name="PubNTest"></a>
## F.Web アプリケーションを Azure に発行してテストする

アプリケーションを App Service Web アプリに発行し、テストします。ローカル コンピューターのデータベースに Web アプリを接続する際に、前に構成したハイブリッド接続がどのように使用されるのかを確認します。

### Web アプリケーションを発行する

1. Azure ポータルの App Service Web アプリに対する発行プロファイルをダウンロードできます。 Web アプリのブレードで、**[発行プロファイルを取得]** をクリックし、コンピューターにファイルを保存します。

    ![発行プロファイルのダウンロード][portaldownloadpublishprofile]

    次に、このファイルを Visual Studio Web アプリケーションにインポートします。

2. Visual Studio で、ソリューション エクスプローラーのプロジェクト名を右クリックし、**[発行]** を選択します。

    ![Select publish][hcvsrightclickprojectselectpublish]

3. **[Web の発行]** ダイアログの **[プロファイル]** タブで、**[インポート]** を選択します。

    ![インポート][hcvspublishwebdialogimport]

4. ダウンロードした発行プロファイルを参照して、選択してから、**[OK]** をクリックします。

    ![Browse to profile][hcvsbrowsetoimportpubprofile]

5. 発行情報がインポートされ、ダイアログの **[接続]** タブに表示されます。

    ![[発行] をクリック][hcvsclickpublish]

    **[発行]** をクリックします。

    発行が完了すると、ブラウザーが起動し、見慣れた ASP.NET アプリケーションが表示されますが、Azure クラウドでライブになっているという点が異なります。

次に、ライブ Web アプリケーションを使用して、ハイブリッド接続の実行を確認します。

### 完成した Web アプリケーションを Azure でテストする

1. Azure の Web ページの右上で、**[ログイン]** を選択します。

    ![テスト ログイン][hctestlogin]

2. App Service Web アプリが、ローカル コンピューターの Web アプリケーションのメンバーシップ データベースに接続されています。 これを確認するには、前にローカル データベースに入力した資格情報でログインします。

    ![Hello greeting][hctesthellocontoso]

3. 新しいハイブリッド接続を詳細にテストするには、Azure Web アプリケーションからログオフし、別のユーザーとして登録します。 新しいユーザー名とパスワードを入力して、**[登録]** をクリックします。

    ![Test register another user][hctestregisterrelecloud]

4. ハイブリッド接続を通じて新しいユーザーの資格情報がローカル データベースに保存されたことを確認するには、ローカル コンピューターで SQL Management Studio を開きます。 オブジェクト エクスプローラーで、**[MembershipDB]** データベースを展開してから、**[テーブル]** を展開します。 **[dbo.AspNetUsers]** メンバーシップ テーブルを右クリックし、**[先頭の 1000 行を選択]** を選択して、結果を確認します。

    ![View the results][hctestssmstree]

5. ローカル メンバーシップ テーブルには、ローカルで作成したメンバーシップと、Azure クラウドで作成したメンバーシップの両方が表示されています。 クラウドで作成したメンバーシップは、Azure のハイブリッド接続機能でオンプレミスのデータベースに保存されています。

    ![オンプレミスのデータベースに保存された登録済みのユーザー][hctestshowmemberdb]

これで、Azure クラウドの Web アプリとオンプレミスの SQL Server データベース間のハイブリッド接続を使用する ASP.NET Web アプリケーションを作成し、デプロイすることができました。 ご利用ありがとうございます。

## 関連項目

[ハイブリッド接続の概要](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[Josh twist によるハイブリッド接続 (チャネル 9 ビデオ) が導入されています](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[ハイブリッド接続の概要](/services/biztalk-services/)

[BizTalk サービス: ダッシュ ボード、モニター、スケール、構成、およびハイブリッド接続] タブ](../biztalk-dashboard-monitor-scale-tabs/)

[シームレスなアプリケーションの移植性 (Channel 9 のビデオ) と実際のハイブリッド クラウドの構築](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[ハイブリッド接続を使用して Azure のモバイル サービスから内部設置型 SQL Server に接続します。](../mobile-services-dotnet-backend-hybrid-connections-get-started.md)

[ハイブリッド接続 (チャネル 9 ビデオ) を使用して Azure Mobile Services から内部設置型 SQL Server に接続します。](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

[ASP.NET Identity の概要](http://www.asp.net/identity)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]




[sqlserverinstall]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png 
[choosedefaultinstance]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png 
[choosemixedmode]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png 
[ssmsconnecttoserver]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png 
[ssmscreatenewdb]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png 
[ssmsprovidedbname]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png 
[ssmsmembershipdbcreated]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png 
[new]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png 
[newwebsite]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png 
[websitecreationblade]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png 
[websiterunningblade]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png 
[browse]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png 
[defaultwebsitepage]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png 
[createhchcicon]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png 
[createhcaddhc]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png 
[twincreatehcblades]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png 
[createhccreatebts]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png 
[createbtscomplete]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png 
[createhcsuccessnotification]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png 
[createhconeconnectioncreated]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png 
[hcicon]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png 
[notconnected]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png 
[notconnectedblade]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png 
[clicklistenersetup]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png 
[clicktoinstallhcm]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png 
[applicationrunwarning]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png 
[uac]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png 
[hcminstalling]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png 
[hcminstallcomplete]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png 
[hcstatusconnected]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png 
[hcvsnewproject]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png 
[hcvschooseaspnet]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png 
[hcvschoosemvc]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png 
[hcvsreadmepage]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png 
[hcvschoosewebconfig]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png 
[hcvsconnectionstring]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png 
[hcvsrunproject]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png 
[hcvsregisterlocally]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png 
[hcvscreatenewaccount]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png 
[portaldownloadpublishprofile]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png 
[hcvspublishprofileindownloadsfolder]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png 
[hcvsrightclickprojectselectpublish]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png 
[hcvspublishwebdialogimport]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png 
[hcvsbrowsetoimportpubprofile]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png 
[hcvsclickpublish]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png 
[hctestlogin]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png 
[hctesthellocontoso]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png 
[hctestregisterrelecloud]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png 
[hctestssmstree]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png 
[hctestshowmemberdb]: ./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png 

