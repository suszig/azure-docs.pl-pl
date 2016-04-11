<properties 
    pageTitle="メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET Web フォーム アプリケーションを作成して Azure App Service にデプロイする" 
    description="このチュートリアルでは、SQL Database と連動するセキュリティ保護された ASP.NET 4.5 Web フォーム Web アプリケーションを開発する方法と、このアプリケーションを Azure にデプロイする方法について説明します。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Erikre" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/10/2015" 
    ms.author="erikre"/>


# メンバーシップ、OAuth、SQL Database を使用した安全な ASP.NET Web フォーム アプリケーションを作成して Azure App Service にデプロイする

##概要
このチュートリアルでは、SQL Database と連動するセキュリティ保護された ASP.NET 4.5 Web フォーム Web アプリケーションを開発する方法と、このアプリケーションを Azure にデプロイする方法について説明します。 

>[AZURE.NOTE] 
このチュートリアルの MVC バージョンを次を参照してください。 [認証および SQL DB と ASP.NET MVC アプリの作成と Azure App Service にデプロイ](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)します。

Azure アカウントは無料で開くことができます。また、まだ Visual Studio 2013 を持っていない場合は、SDK によって Visual Studio 2013 for Web Express が自動的にインストールされます。 これで、Azure 向けのアプリケーションを無料で開発できます。

このチュートリアルは、Microsoft Azure を使用した経験がない読者を対象に作成されています。 このチュートリアルを完了すると、クラウド データベースを使用する Web アプリケーションをクラウドで運用できるようになります。

学習内容:

- ASP.NET 4.5 Web フォーム プロジェクトを作成し、そのプロジェクトを Azure App Service に発行する方法
- OAuth および ASP.NET メンバーシップを使用してアプリケーションを保護する方法
- 1 つのデータベースを使用してメンバーシップとアプリケーション データの両方に対応する方法
- Web フォームのスキャフォールディングを使用して、ユーザーが自分でデータを変更できるような Web ページを作成する方法
- 新しいメンバーシップ API を使用してユーザーおよびロールを追加する方法
- SQL データベースを使用して Azure にデータを保存する方法

今回は、Entity Framework を使用してデータベースにアクセスするシンプルな連絡先リスト Web アプリケーションを、ASP.NET 4.5 Web フォームを基盤に開発します。 以下は、データベースへの読み取り/書き込みアクセスを許可する Web フォーム ページの画像です。

![連絡先 - 編集ページ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms00.png)

>[AZURE.NOTE] 
このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントを持っていない場合は、<a href="/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F" target="_blank">Visual Studio サブスクライバーの特典を有効にする</a>か、<a href="/pricing/free-trial/?WT.mc_id=A261C142F" target="_blank">無料試用版にサインアップ</a>してください。 アカウントがサインアップする前に Azure を使ってみる場合は、 [App Service の試用](https://tryappservice.azure.com/), することができますをすぐに作成、有効期間の短い ASP.NET スターター サイトで、Azure は無料です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

##開発環境を設定する 
最初に、Visual Studio 2013 と Azure SDK for .NET をインストールして開発環境を設定します。

1. インストール [Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=306566), 、インストールされていない場合。  
2. インストール [Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)します。 このチュートリアルでは、Azure SDK for Visual Studio 2013 の前に Visual Studio 2013 をインストールする必要があります。 マシンに既にある SDK の依存関係の数に応じて、SDK のインストールには長時間 (数分から 30 分以上) かかる場合があります。  

3. 実行またはインストールの実行可能ファイルを保存] をクリックするメッセージが表示されたら **実行**します。
4.  **Web Platform Installer** ウィンドウで、をクリックして **インストール** し、インストールを実行します。  
    ![Web Platform Installer](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-01.png)  

      この SDK をインストール済みの場合、インストールされる項目の数は 0 になります。 インストールされる項目の数は、の左下に表示されます、 **Web Platform Installer** ウィンドウです。  

5. まだしていない場合 **Visual Studio Update 2**, 、ダウンロードおよびインストール **[Visual Studio 2013 Update 2](http://www.microsoft.com/download/details.aspx?id=42666)** またはそれ以降。  

    >[AZURE.NOTE]  
    Goggle OAuth 2.0 を使用するには、また、警告を表示させずに SSL をローカルで使用するには、Visual Studio 2013 Update 2 以降をインストールする必要があります。 また、Web フォームのスキャフォールディングを使用するには Update 2 が必要です。  

インストールが完了すると、開発に必要なツールがすべて揃います。

##Azure 環境を設定する
このセクションでは、Azure と SQL データベースを Azure 内に作成して、Azure 環境を設定します。

###Azure で Web アプリと SQL Database を作成する 
このチュートリアルでは、Web アプリが共有ホスティング環境で実行されます。つまり、Web アプリを実行する仮想マシン (VM) が、Azure App Service の他の Web アプリと共有されています。 共有ホスティング環境は、低コストでクラウドの利用を開始できる方法です。 後で Web トラフィックが増加したら、アプリケーションの規模を変更して専用 VM 上で実行するように設定してニーズを満たすことができます。 もっと複雑なアーキテクチャが必要な場合は、Azure のクラウド サービスに移行できます。 クラウド サービスは専用 VM 上で実行され、ユーザーのニーズに応じて構成できます。 

Azure SQL Database は、SQL Server テクノロジに基づいて構築されたクラウドベースのリレーショナル データベース サービスです。 SQL Server で動作するツールおよびアプリケーションは、SQL Database でも動作します。

1.  [Azure クラシック ポータル](https://manage.windowsazure.com/), 、クリックして **Web Apps** 左側のタブをクリックし **新規**します。  
    ![Web Platform Installer](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-02.png)
2. クリックして **Web アプリ**, 、] をクリックし、 **カスタム作成**します。  
    ![カスタム作成します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-03.png)  
     **新しい Web アプリ - カスタム作成** ウィザードが開きます。  

3.  **Web アプリの作成** 手順と、ウィザードの入力内の文字列、 **URL** 、アプリケーションの一意の URL として使用する] ボックス。 ここに入力した文字列と、このテキスト ボックスの右側に表示されている文字列を組み合わせたものが実際の URL になります。 図に表示されているため URL **別の URL を選択する必要があります**します。  
    ![連絡先 - 新しい Web サイトを作成します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-04.png)  
4. [Web ホスティング プラン] ドロップダウン リストで、現在の所在地に最も近いリージョンを選択します。 この設定によって、使用する VM が実行されるデータ センターが指定されます。
5.  **データベース** ドロップダウン リストで、選択 **無料の 20 MB SQL データベースを作成**します。
6.  **DB 接続文字列名** ボックスで、既定値のままにして *DefaultConnection*します。
7. ボックスの下部にある矢印をクリックします。 
ウィザード、 **データベース設定の指定** 手順です。
8.  **名前** ボックスに、入力 *`ContactDB`*します。  
    ![データベースの設定](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-05.png)  
9.  **Server** ボックスで、 **新しい SQL データベース** サーバーです。
または、以前に SQL Server データベースを作成した場合は、ボックスの一覧からその SQL Server を選択できます。
10. 設定、 **地域** と同じエリアには、web アプリを作成します。
11. 管理者が入力 **ログイン名** と **パスワード**します。 
選択した場合は **新しい SQL データベース サーバー** 、既存の名前とパスワードで入力していない、新しい名前と、データベースへのアクセス時に使用するようになりました定義しているパスワードを入力してください。 以前に作成した SQL Server を選択した場合は、その SQL Server の作成時に設定したパスワードを入力します。 このチュートリアルではありませんをチェックする、 **詳細** ボックス。
12. 終了したら、ダイアログ ボックスの右下にあるチェック マークをクリックします。

 **Azure クラシック ポータル** に戻ります、 **Web Apps** ] ページで、および **ステータス** 列が、サイトが作成されていることを示しています。 直後に (通常は 1 分未満)、 **ステータス** 列が、サイトが正常に作成されたことを示しています。 自分のアカウントで所有するサイトの数を横に表示、左にあるナビゲーション バーで、 **Web アプリ** アイコン、およびデータベースの数が横に表示、 **SQL データベース** アイコン。
##ASP.NET Web フォーム アプリケーションを作成する 
Web アプリを作成しましたが、まだその中にコンテンツがありません。 次のステップでは、Azure に発行する Visual Studio Web アプリケーションを作成します。
###プロジェクトを作成する 
1. 選択 **新しいプロジェクト** から、 **ファイル** Visual Studio のメニュー。  
    ![[ファイル] メニューの [新しいプロジェクト](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms01.png)  
2. 選択、 **テンプレート** ]-> [ **Visual c#** ]-> [ **Web** 左側のテンプレートのグループです。 
3. 選択、 **ASP.NET Web アプリケーション** 中央の列でのテンプレートです。
4. プロジェクトに名前を *ContactManager* ] をクリック **OK**します。  
    ![新しいプロジェクト] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms02.png)  

      このチュートリアル シリーズで、プロジェクトの名前は **ContactManager**します。 チュートリアル シリーズのさまざまな場面で提供されるコードを説明どおりに動作させるため、まったく同じ名前を使用することをお勧めします。  

5.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、 **Web フォーム** テンプレートです。 オフにして、 **クラウドでホスト** ] チェック ボックスをオンになっている場合、をクリックして **[ok]**します。  
    ![新しい ASP.NET プロジェクト] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms03.png)  
    Web フォーム アプリケーションが作成されます。

###マスター ページを更新する
ASP.NET Web フォームでは、マスター ページを使うことで、アプリケーションのページに共通のレイアウトを適用することができます。 これには、アプリケーションのすべてのページ (またはページ グループ) に適用する外観、操作性、標準の動作を定義するマスター ページを 1 つ作成します。 次に、表示するコンテンツが格納されるコンテンツ ページを個別に作成します。 ユーザーがコンテンツ ページを要求すると、ASP.NET はコンテンツ ページとマスター ページをマージします。これによって、マスター ページのレイアウトとコンテンツ ページのコンテンツが組み合わされ、出力されます。
今回の新しいサイトでは、アプリケーションの名前とリンクを更新する必要があります。 このリンクは、連絡先の詳細を表示するページを参照するリンクです。 これらの変更を実施するには、マスター ページの HTML を変更します。 

1.  **ソリューション エクスプ ローラー**, を検索して開く、 *Site.Master* ページです。 
2. 場合は、ページが **デザイン** ビューで、切り替える **ソース** 表示します。
3. ページのマークアップが次のように表示されるように、マークアップを変更または追加して、マスター ページを更新します。

        <%@ Master Language="C#" AutoEventWireup="true" CodeBehind="Site.master.cs" Inherits="ContactManager.SiteMaster" %>
        
        <!DOCTYPE html>
        
        <html lang="en">
        <head runat="server">
            <meta charset="utf-8" />
            <meta name="viewport" content="width=device-width, initial-scale=1.0" />
            <title><%: Page.Title %> - Contact Manager</title>
        
            <asp:PlaceHolder runat="server">
                <%: Scripts.Render("~/bundles/modernizr") %>
            </asp:PlaceHolder>
            <webopt:bundlereference runat="server" path="~/Content/css" />
            <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
        
        </head>
        <body>
            <form runat="server">
                <asp:ScriptManager runat="server">
                    <Scripts>
                        <%--To learn more about bundling scripts in ScriptManager see http://go.microsoft.com/fwlink/?LinkID=301884 --%>
                        <%--Framework Scripts--%>
                        <asp:ScriptReference Name="MsAjaxBundle" />
                        <asp:ScriptReference Name="jquery" />
                        <asp:ScriptReference Name="bootstrap" />
                        <asp:ScriptReference Name="respond" />
                        <asp:ScriptReference Name="WebForms.js" Assembly="System.Web" Path="~/Scripts/WebForms/WebForms.js" />
                        <asp:ScriptReference Name="WebUIValidation.js" Assembly="System.Web" Path="~/Scripts/WebForms/WebUIValidation.js" />
                        <asp:ScriptReference Name="MenuStandards.js" Assembly="System.Web" Path="~/Scripts/WebForms/MenuStandards.js" />
                        <asp:ScriptReference Name="GridView.js" Assembly="System.Web" Path="~/Scripts/WebForms/GridView.js" />
                        <asp:ScriptReference Name="DetailsView.js" Assembly="System.Web" Path="~/Scripts/WebForms/DetailsView.js" />
                        <asp:ScriptReference Name="TreeView.js" Assembly="System.Web" Path="~/Scripts/WebForms/TreeView.js" />
                        <asp:ScriptReference Name="WebParts.js" Assembly="System.Web" Path="~/Scripts/WebForms/WebParts.js" />
                        <asp:ScriptReference Name="Focus.js" Assembly="System.Web" Path="~/Scripts/WebForms/Focus.js" />
                        <asp:ScriptReference Name="WebFormsBundle" />
                        <%--Site Scripts--%>
                    </Scripts>
                </asp:ScriptManager>
        
                <div class="navbar navbar-inverse navbar-fixed-top">
                    <div class="container">
                        <div class="navbar-header">
                            <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                                <span class="icon-bar"></span>
                                <span class="icon-bar"></span>
                                <span class="icon-bar"></span>
                            </button>
                            <a class="navbar-brand" runat="server" id="ContactDemoLink" href="~/Contacts/Default.aspx">Contact Demo</a>
                        </div>
                        <div class="navbar-collapse collapse">
                            <ul class="nav navbar-nav">
                                <li><a runat="server" href="~/">Home</a></li>
                                <li><a runat="server" href="~/About">About</a></li>
                                <li><a runat="server" href="~/Contact">Contact</a></li>
                            </ul>
                            <asp:LoginView runat="server" ViewStateMode="Disabled">
                                <AnonymousTemplate>
                                    <ul class="nav navbar-nav navbar-right">
                                        <li><a runat="server" href="~/Account/Register">Register</a></li>
                                        <li><a runat="server" href="~/Account/Login">Log in</a></li>
                                    </ul>
                                </AnonymousTemplate>
                                <LoggedInTemplate>
                                    <ul class="nav navbar-nav navbar-right">
                                        <li><a runat="server" href="~/Account/Manage" title="Manage your account">Hello, <%: Context.User.Identity.GetUserName()  %> !</a></li>
                                        <li>
                                            <asp:LoginStatus runat="server" LogoutAction="Redirect" LogoutText="Log off" LogoutPageUrl="~/" OnLoggingOut="Unnamed_LoggingOut" />
                                        </li>
                                    </ul>
                                </LoggedInTemplate>
                            </asp:LoginView>
                        </div>
                    </div>
                </div>
                <div class="container body-content">
                    <asp:ContentPlaceHolder ID="MainContent" runat="server">
                    </asp:ContentPlaceHolder>
                    <hr />
                    <footer>
                        <p>&copy; <%: DateTime.Now.Year %> - Contact Manager</p>
                    </footer>
                </div>
            </form>
        </body>
        </html>

    このチュートリアルでは、Web フォームのスキャフォールディングを追加します (後述)。 スキャフォールディングによって、上記の "Contact Demo" リンクの参照先ページが作成されます。
 
###ローカルでアプリケーションを実行する 
 
1.  **ソリューション エクスプ ローラー**, を右クリックし、 *Default.aspx* ページをオンに **スタート ページに設定**します。 
2. 次に、キーを押して **CTRL + F5** アプリケーションを実行します。  
    アプリケーションの既定のページが、既定のブラウザー ウィンドウに表示されます。
    ![連絡先 - 新しい Web サイトを作成します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms04.png)  

これで、Azure にデプロイするアプリケーションを作成するために必要な操作が完了しました。 データベースの機能と、連絡先データを表示および編集するためのページは、後ほど追加します。
###Azure にアプリケーションをデプロイする
現在、アプリケーションを作成してローカルで実行しています。このアプリケーションを Azure にデプロイします。

1. Visual Studio でプロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行** 、コンテキスト メニューからです。  
    ![発行の選択](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms05.png)  
     **Web の発行** ] ダイアログ ボックスが表示されます。  

2.  **プロファイル** のタブ、 **Web の発行** ダイアログ ボックスで、をクリックして **Azure Web App**します。  
      
3. 既に署名されていない場合は、クリックして、 **サイン イン** ボタンをクリックして、 **既存の Web アプリを選択して** ] ダイアログ ボックス。 サインインしたら、このチュートリアルの最初に作成した Web アプリを選択します。 クリックして **OK** を続行します。  
    ![既存の Web サイト] ダイアログ ボックスを選択します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms07.png)  
Visual Studio によって発行設定がダウンロードされます。
4.  **Web の発行** ] ダイアログ ボックスをクリックして **発行**します。  
    ![Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms08.png)  
全体的な発行ステータスが表示されます、 **Web 発行アクティビティ** Visual Studio 内でのウィンドウ。  
    ![Web 発行アクティビティ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms09.png)  

これで、作成したアプリケーションはクラウドで実行されています。 このアプリケーションを次に Visual Studio からデプロイするときは、変更したファイル (または新しいファイル) のみがデプロイされます。  
    ![ブラウザーでのアプリ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms10.png)  

>[AZURE.NOTE] 
既に存在する Web アプリへの発行でエラーが発生した場合、その発行先を消去してから新しいファイルを追加します。  
ここでも、アプリケーションの発行ただし、 **Web の発行** ダイアログ ボックスで、 **設定** ] タブをクリックします。 構成を設定し、 **デバッグ** オプションを選択し、 **転送先にその他のファイルを削除する**です。 選択 **発行** をもう一度アプリケーションを展開します。  
    ![Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms11.png)  

##アプリケーションにデータベースを追加する 
次に、Web フォーム アプリケーションを更新して、連絡先を表示および更新して既定のデータベースにデータを保存する機能を追加します。 Web フォーム プロジェクトを作成した際にデータベースも既定で作成されています。 アプリケーションは Entity Framework を使用してこのデータベースにアクセスし、データベース内のデータを更新します。
###データ モデル クラスを追加する 
まず、コードを使ってシンプルなデータ モデルを作成します。 このデータ モデルが `Contacts` というクラスに格納されます。 `Contacts` というクラス名にした理由は、Web Forms テンプレートが作成した Contact.aspx.cs ファイルに格納されている `Contact` クラスとのクラス名の競合を避けるためです。  

1.  **ソリューション エクスプ ローラー**, を右クリックし、 *モデル* クリックしてフォルダー **追加** ]-> [ **クラス**します。  
    ![クラスを選択します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms12.png)  
     **新しい項目の追加** ] ダイアログ ボックスが表示されます。  

2. この新しいクラスの名前を *Contacts.cs* ] をクリック **追加**します。  
    ![[新しい項目] ダイアログ ボックスを追加します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13.png)  
3. 既定のコードを以下のコードに置き換えます。  

        using System.ComponentModel.DataAnnotations;
        using System.Globalization;
        
        namespace ContactManager.Models
        {
            public class Contacts
            {
                [ScaffoldColumn(false)]
                [Key]
                public int ContactId { get; set; }
                public string Name { get; set; }
                public string Address { get; set; }
                public string City { get; set; }
                public string State { get; set; }
                public string Zip { get; set; }
                [DataType(DataType.EmailAddress)]
                public string Email { get; set; }
            }
        }

 **連絡先** クラスのそれぞれの連絡先とする主キーを格納するデータ定義 (`ContactID`)、データベースが必要です。  **連絡先** クラスが表示される連絡先データを表します。 Contacts オブジェクトの各インスタンスはリレーショナル データベース テーブルの各行に相当し、Contacts クラスの各プロパティは、リレーショナル データベース テーブルの各列に関連付けられます。 データベースに格納されている連絡先データについては、このチュートリアルで後ほど説明します。

###Web フォームのスキャフォールディング 
作成した、 **連絡先** モデル クラス。 Web Forms Scaffolder を使用して、生成するようになりましたが、 *リスト*, 、*挿入*, 、*編集* と *削除* このデータを使用する場合に使用されるページです。 Web Forms Scaffolder は、Entity Framework、ブートストラップ、動的データを使用します。 既定では、Web Forms Scaffolder はプロジェクト テンプレートに含まれており、Visual Studio 2013 を使用する際にプロジェクトの拡張機能としてインストールされます。

次の手順を実行することで、Web Forms Scaffolder を使用できるようになります。

1. Visual Studio のメニュー バーから選択 **ツール** ]-> [ **拡張機能と更新プログラム**します。  
     **拡張機能と更新プログラム** ] ダイアログ ボックスが表示されます。
2. ダイアログ ボックスの左側のウィンドウから次のように選択します。 **オンライン** ]-> [ **Visual Studio ギャラリー** ]-> [ **ツール** ]-> [ **スキャフォールディング**します。
3. 「Web Forms Scaffolding」が一覧に表示されない場合は、ダイアログ ボックス右側の検索ボックスに「Web Forms Scaffolding」と入力します。  
4. Web Forms Scaffolder がインストールされていない場合は、選択 **ダウンロード** をダウンロードして ' Web Forms Scaffolding をインストールします。 必要に応じて Visual Studio を再起動します。 確認された場合は、プロジェクトへの変更を必ず保存してください。  
    ![拡張機能と更新プログラム] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/ExtensionsAndUpdatesDB.png)  
5. プロジェクトをビルドします (**Ctrl + Shift + B**)。  
    プロジェクトは、スキャフォールディング機能を使用する前にビルドする必要があります。  
6.  **ソリューション エクスプ ローラー**, を右クリックし、 *プロジェクト* し、[ **追加** ]-> [ **新しいスキャフォールディング項目**します。  
     **スキャフォールディングの追加** ] ダイアログ ボックスが表示されます。
7. 選択 **Web フォーム** し、左側のウィンドウから **Entity Framework を使用して Web フォーム ページ** 中央のペインからです。 クリックして **追加**します。  
    ![ダイアログ ボックスの [スキャフォールディングを追加します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13a.png)  
     **Add Web Forms Pages** ] ダイアログ ボックスが表示されます。  

8.  **Add Web Forms Pages** ダイアログ ボックスで、セット、 **モデル クラス** に `Contacts (ContactManager.Models)`します。 設定、 **データ コンテキスト クラス** に `ApplicationDbContext (ContactManager.Models)`します。 クリックして **追加**します。 
    ![Web フォーム ページのダイアログ ボックスを追加します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13b.png)  

Web Forms Scaffolder は、格納する新しいフォルダーを追加 *Default.aspx*, 、*Delete.aspx*, 、*Edit.aspx*, 、および *Insert.aspx* ページです。 また、Web Forms Scaffolder が作成、 *DynamicData* を含むフォルダー、 *EntityTemplates* フォルダーと *FieldTemplates* フォルダーです。 `ApplicationDbContext` はメンバーシップ データベースと連絡先データの両方に使用されます。

###データ モデルを使用するようにアプリケーションを構成する 
次の作業では、作成したデータ モデルに基づいてデータベースを作成するための Code First Migrations 機能を有効化すると共に、 サンプル データとデータ初期化子を追加します。  

1.  **ツール** メニューの [ **NuGet パッケージ マネージャー** し **パッケージ マネージャー コンソール**します。  
    ![Web フォーム ページのダイアログ ボックスを追加します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13c.png)  
2. [パッケージ マネージャー コンソール] ウィンドウで、次のコマンドを入力します。  

        enable-migrations
 
    Enable-migrations コマンドを作成、 *移行* フォルダー、および、そのフォルダーに配置、 *Configuration.cs* ファイルを編集データベースをシードし、データの移行を構成することができます。  
3.  **パッケージ マネージャー コンソール** ウィンドウで、次のコマンドを入力します。  

        add-migration Initial

     `add-migration Initial` で < date_stamp > Initial というファイルを生成するコマンド、 *移行* 、データベースを作成するフォルダーです。 最初のパラメーター (Initial) は任意です。このパラメーターは、このファイルの名前の作成に使用されます。 新しいクラス ファイルを確認できます **ソリューション エクスプ ローラー**します。 `Initial` クラスでは、`Up` メソッドを使用して `Contact` テーブルを作成し、`Down` メソッドを使用してそのテーブルを削除します (前の状態に戻します)。  
4. 開いている、 *migrations \configuration.cs* ファイルです。 

5. 次の名前空間を追加します。  

        using ContactManager.Models;

6. `Seed` メソッドを次のコードに置き換えます。  

        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contacts
               {
                   ContactId = 1,
                   Name = "Ivan Irons",
                   Address = "One Microsoft Way",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "ivani@wideworldimporters.com",
               },
               new Contacts
                {
                    ContactId = 2,
                    Name = "Brent Scholl",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "brents@wideworldimporters.com",
                },
                new Contacts
                {
                    ContactId = 3,
                    Name = "Terrell Bettis",
                    Address = "9012 State St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "terrellb@wideworldimporters.com",
                },
                new Contacts
                {
                    ContactId = 4,
                    Name = "Jo Cooper",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "joc@wideworldimporters.com",
                },
                new Contacts
                {
                    ContactId = 5,
                    Name = "Ines Burnett",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "inesb@wideworldimporters.com",
                }
                );
        }

    このコードでは、連絡先情報を使用してデータベースを初期化 (初期データ投入) します。 シード データベースの生成の詳細については、次を参照してください。 [シード処理、およびデバッグ Entity Framework (EF) Db](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)します。  
7.  **パッケージ マネージャー コンソール** コマンドを入力します。  

        update-database

`update-database` によって、データベースを作成する最初の Migration が実行されます。 既定では、データベースは SQL Server Express LocalDB データベースとして作成されます  
    ![パッケージ マネージャー コンソール](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13d.png)  

###アプリケーションをローカルで実行してデータを表示する 
次に、アプリケーションを実行して連絡先の表示を確認します。

1. 最初に、プロジェクトをビルドします (**Ctrl + Shift + B**)。  
2. キーを押して **Ctrl + F5** アプリケーションを実行します。  
    ブラウザーが開き、表示、 *Default.aspx* ページです。
3. 選択、 **Contact Demo** 表示するページの上部にあるリンク、 *Contact List* ページです。  
    ![[連絡先リスト] ページ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms17.png)  

##プロジェクトに対して SSL を有効にする 
Secure Sockets Layer (SSL) は、Web サーバーと Web クライアント間の通信に定義されたプロトコルで、暗号化によって通信の安全性を強化することができます。 SSL を使わないと、クライアントとサーバー間で送信されるデータが、ネットワークに物理的にアクセスできる第三者によるパケット スニッフィングの標的になります。 また、プレーンな HTTP を使用すると、いくつかの一般的な認証方式の安全性も低下します。 具体的には、基本認証とフォーム認証で送信する資格情報が暗号化されません。 安全性を確保するには、これらの認証方式で SSL を使用する必要があります。

1.  **ソリューション エクスプ ローラー**, をクリックして、 **ContactManager** プロジェクトをし、キーを押します **f4 キーを押して** を表示する、 **プロパティ** ウィンドウです。 
2. 変更 **SSL を有効に** に `true`します。 
3. コピー、 **SSL URL** 後で使用できるようにします。  
    SSL URL になります `https://localhost:44300/` 以前作成した SSL web アプリ (下図)。
    ![プロジェクトのプロパティ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms18.png)  
4.  **ソリューション エクスプ ローラー**, を右クリックして、 **Contact Manager** プロジェクトを **プロパティ**します。
5. 左側のタブをクリックして **Web**します。
6. 変更、 **プロジェクト Url** を使用する、 **SSL URL** 以前に保存します。  
    ![プロジェクトの Web プロパティ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms19.png)  
7. キーを押してページを保存 **CTRL + S**します。
8. キーを押して **Ctrl + F5** アプリケーションを実行します。  
Visual Studio により、SSL の警告を回避するためのオプションが表示されます。  
9. クリックして **はい** を IIS Express SSL 証明書を信頼し、続行します。  
    ![IIS Express SSL 証明書の詳細](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms20.png)  
    セキュリティ警告が表示されます。  

10. クリックして **はい** してローカル ホストに証明書をインストールします。  
    ![セキュリティ警告] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21.png)  
    ブラウザー ウィンドウが表示されます。  

ローカルで SSL を使用して Web アプリケーションを簡単にテストできるようになりました。



##OAuth 2.0 プロバイダーを追加する 
ASP.NET Web フォームは、メンバーシップと認証のオプションが強化されています。 OAuth もこうした強化点の 1 つです。 OAuth は、Web、モバイル、およびデスクトップのアプリケーションからシンプルで標準的な方法で安全に認証するためのオープン プロトコルです。 ASP.NET MVC インターネット テンプレートは OAuth を使用して、Facebook、Twitter、Google、Microsoft を認証プロバイダーとしてサポートします。 このチュートリアルでは Google のみを認証プロバイダーとして使用しますが、コードを少し変更すれば他のプロバイダーも使用できます。 他のプロバイダーを実装する手順は、このチュートリアルで説明する手順とほとんど同じです。 

このチュートリアルでは、認証の他にロールを使用して権限を付与します。 `canEdit` ロールに追加したユーザーだけが連絡先を変更 (作成、編集、削除) できます。 

次の手順を実行することで、Google 認証プロバイダーを追加できます。

1. 開いている、 *App_Start\Startup.Auth.cs* ファイルです。 
2. `app.UseGoogleAuthentication()` メソッドのコメント文字を削除して、メソッドを次のような記述にします。  

        app.UseGoogleAuthentication(new GoogleOAuth2AuthenticationOptions()
        {
            ClientId = "",
            ClientSecret = ""
        });

3. 移動し、 [Google Developers Console](https://console.developers.google.com/)します。 Google デベロッパーの電子メール アカウント (gmail.com) でサインインする必要があります。 Google アカウントがあるない場合は、選択、 **アカウントの作成** リンクします。  
    次に確認、 **Google Developers Console**します。
    ![Google Developers Console](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21a.png)  

4. クリックして、 **プロジェクトを選択して** > **プロジェクトの作成** プロジェクト名と (既定値を使用することができます) ID を入力します。 [] をクリックして、 **契約のチェック ボックス** と **作成** ] ボタンをクリックします。  
    ![Google - 新しいプロジェクト](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21b.png)  
    新しいプロジェクトが数秒で作成され、新しいプロジェクトのページがブラウザーに表示されます。
5.  **Google Developers Console** ドロップダウン メニューをクリックして **API Manager**, 、順にクリック **資格情報**します。
6. クリックして、 **Create New Client ID** [ **OAuth**します。  
     **Create Client ID** ダイアログが表示されます。
    ![Google のクライアント ID を作成します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21c.png)  
7.  **Create Client ID** ダイアログ ボックスで、既定のまま **Web アプリケーション** のアプリケーションの種類。  
8. 設定、 **Authorized JavaScript Origins** にこのチュートリアルで既に使用した SSL URL (**https://localhost:44300/** 他の SSL プロジェクトを作成した)。  
    この URL がアプリケーションのオリジンです。 このサンプルではローカルホストのテスト URL を入力するだけですが、 アカウントには複数の URL を入力できます (ローカルホストおよび本稼働用)。  

9. 設定、 **Authorized Redirect URI** 以下にします。  

        https://localhost:44300/signin-google  

    この値は、ASP.NET OAuth ユーザーが Google の OAuth サーバーとの通信に使用する URI です。 上記で使用した SSL URL を忘れないでください (**https://localhost:44300/** 他の SSL プロジェクトを作成した)。 
 
10. クリックして、 **作成** ] ボタンをクリックします。
11. Visual Studio で、更新、 `UseGoogleAuthentication` のメソッド、 *Startup.Auth.cs* コピー アンド ペーストでページ、 **AppId** と **App Secret** メソッドにします。  **AppId** と **App Secret** の下に表示される値はサンプルでありは機能しません。  

        using System;
        using Microsoft.AspNet.Identity;
        using Microsoft.AspNet.Identity.EntityFramework;
        using Microsoft.AspNet.Identity.Owin;
        using Microsoft.Owin;
        using Microsoft.Owin.Security.Cookies;
        using Microsoft.Owin.Security.DataProtection;
        using Microsoft.Owin.Security.Google;
        using Owin;
        using ContactManager.Models;
        
        namespace ContactManager
        {
            public partial class Startup {
        
                // For more information on configuring authentication, please visit http://go.microsoft.com/fwlink/?LinkId=301883
                public void ConfigureAuth(IAppBuilder app)
                {
                    // Configure the db context and user manager to use a single instance per request
                    app.CreatePerOwinContext(ApplicationDbContext.Create);
                    app.CreatePerOwinContext(ApplicationUserManager.Create);
        
                    // Enable the application to use a cookie to store information for the signed in user
                    // and to use a cookie to temporarily store information about a user logging in with a third party login provider
                    // Configure the sign in cookie
                    app.UseCookieAuthentication(new CookieAuthenticationOptions
                    {
                        AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
                        LoginPath = new PathString("/Account/Login"),
                        Provider = new CookieAuthenticationProvider
                        {
                            OnValidateIdentity = SecurityStampValidator.OnValidateIdentity(
                                validateInterval: TimeSpan.FromMinutes(20),
                                regenerateIdentity: (manager, user) => user.GenerateUserIdentityAsync(manager))
                        }
                    });
                    // Use a cookie to temporarily store information about a user logging in with a third party login provider
                    app.UseExternalSignInCookie(DefaultAuthenticationTypes.ExternalCookie);
        
                    // Uncomment the following lines to enable logging in with third party login providers
                    //app.UseMicrosoftAccountAuthentication(
                    //    clientId: "",
                    //    clientSecret: "");
        
                    //app.UseTwitterAuthentication(
                    //   consumerKey: "",
                    //   consumerSecret: "");
        
                    //app.UseFacebookAuthentication(
                    //   appId: "",
                    //   appSecret: "");
        
                    app.UseGoogleAuthentication(new GoogleOAuth2AuthenticationOptions()
                    {
                        ClientId = "000000000000.apps.googleusercontent.com",
                        ClientSecret = "00000000000"
                    });
                }
            }
        }

12. キーを押して **CTRL + F5** をビルドして、アプリケーションを実行します。 クリックして、 **ログイン** リンクします。
13.  **別のサービスを使用してログイン**, をクリックして **Google**します。  
    ![ログインします。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21d.png)  
14. 資格情報の入力が必要な場合、Google のサイトにリダイレクトされるので、リダイレクト先のサイトで資格情報を入力します。  
    ![Google - サインイン](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21e.png)  
15. 資格情報を入力すると、先ほど作成した Web アプリケーションへの権限の付与が求められます。  
    ![プロジェクトのデフォルト サービス アカウント](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21f.png)  
16. クリックして **受け入れる**します。 今すぐにリダイレクトされます、 **登録** のページ、 **ContactManager** アプリケーションが Google アカウントを登録することができます。
    ![Google アカウントを登録します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21g.png)  
17. Google アカウントに使用するローカルの電子メール登録名を変更できますが、通常は既定の電子メール エイリアス (認証に使用したエイリアス) を変更しません。 クリックして **ログイン**します。

##メンバーシップ API を使用してアクセスを制限する 
ASP.NET アプリケーションの開発では、認証に ASP.NET Identity というメンバーシップ システムを使用できます。 ASP.NET Identity によって、ユーザー固有のプロファイル データとアプリケーション データを簡単に統合できるだけでなく、 アプリケーションのユーザー プロファイルに永続性を適用することができます。 データを保存するには、SQL Server データベースやその他のデータ ストアで含む *NoSQL* Azure Storage テーブルなどのデータを格納します。

既定の ASP.NET Web フォーム テンプレートを使用する場合は、あらかじめ組み込まれているメンバーシップ機能をアプリケーション実行時にすぐに使うことができます。 ここでは、ASP.NET Identity を使用して管理者ロールを追加し、このロールにユーザーを割り当てます。 その後、連絡先データの変更に使用される、管理者用フォルダーおよび同フォルダー内のページへのアクセスを制限する方法について説明します。

###管理者を追加する 
ASP.NET Identity を使用すると、管理者ロールの追加および管理者ロールへのユーザーの割り当てを、コードを使って実行できます。 

1.  **ソリューション エクスプ ローラー**, を開き、 *Configuration.cs* ファイルで、 *移行* フォルダーです。
2. 次の `using` ステートメントを `ContactManger.Migrations` 名前空間に追加します。  

        using Microsoft.AspNet.Identity;
        using Microsoft.AspNet.Identity.EntityFramework;

3. 次の `AddUserAndRole` メソッドを `Configuration` クラスに追加します。これは `Seed` メソッドの後に追加します。  

        public void AddUserAndRole(ContactManager.Models.ApplicationDbContext context)
        {
            IdentityResult IdRoleResult;
            IdentityResult IdUserResult;
        
            var roleStore = new RoleStore<IdentityRole>(context);
            var roleMgr = new RoleManager<IdentityRole>(roleStore);
        
            if (!roleMgr.RoleExists("canEdit"))
            {
                IdRoleResult = roleMgr.Create(new IdentityRole { Name = "canEdit" });
            }
        
            //var userStore = new UserStore<ApplicationUser>(context);
            //var userMgr = new UserManager<ApplicationUser>(userStore);
            var userMgr = new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(context));
        
            var appUser = new ApplicationUser
            {
                UserName = "canEditUser@wideworldimporters.com",
                Email = "canEditUser@wideworldimporters.com"
            };
            IdUserResult = userMgr.Create(appUser, "Pa$$word1");
        
            if (!userMgr.IsInRole(userMgr.FindByEmail("canEditUser@wideworldimporters.com").Id, "canEdit"))
            {
              //  IdUserResult = userMgr.AddToRole(appUser.Id, "canEdit");
                IdUserResult = userMgr.AddToRole(userMgr.FindByEmail("canEditUser@wideworldimporters.com").Id, "canEdit");
            }
        }

4. `AddUserAndRole` メソッドの呼び出しを `Seed` メソッドの開始部分に追加します。 以下には `Seed` メソッドの開始部分だけが表示されている点に注意してください。  

        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            AddUserAndRole(context);

5. すべての変更を保存した後、 **パッケージ マネージャー コンソール** 、次のコマンドを実行します。  

        Update-Database

このコードと呼ばれる新しいロールを作成する `canEdit` され、canEditUser@wideworldimporters.com の電子メールが、新しいローカル ユーザーを作成します。 次に canEditUser@wideworldimporters.com に追加コード、 `canEdit` ロールです。 詳細については、次を参照してください。、 [ASP.NET Identity](http://www.asp.net/identity) リソース ページです。  

###管理者用フォルダーへのアクセスを制限する 
 **ContactManager** サンプル アプリケーションでは、匿名ユーザーとログイン ユーザーの両方が連絡先を表示します。 このセクションを完了すると、連絡先を追加できるのは canEdit ロールが割り当てられたログイン ユーザーだけになります。

という名前のフォルダーを作成 *Admin* "canEdit"ロールに割り当てられているユーザーのみがアクセスできます。

1.  **ソリューション エクスプ ローラー**, 、フォルダーにサブフォルダーを追加、 *連絡先* フォルダーと新しいサブフォルダー名 *Admin*します。
2. 次のファイルを移動、 *連絡先* フォルダーを *Contacts/admin* フォルダー。  
    - *Delete.aspx *と* Delete.aspx.cs*
    - *Edit.aspx *と* Edit.aspx.cs*
    - *Insert.aspx *と* Insert.aspx.cs*
3. 内のリンク参照を更新 *Contacts/Default.aspx* 追加することで"Admin/"にリンクしているページ参照の前に *Insert.aspx*, 、*Edit.aspx*, と *Delete.aspx* 次のようにします。  

        <%@ Page Title="ContactsList" Language="C#" MasterPageFile="~/Site.Master" CodeBehind="Default.aspx.cs" Inherits="ContactManager.Contacts.Default" ViewStateMode="Disabled" %>
        <%@ Register TagPrefix="FriendlyUrls" Namespace="Microsoft.AspNet.FriendlyUrls" %>
        
        <asp:Content runat="server" ContentPlaceHolderID="MainContent">
            <h2>Contacts List</h2>
            <p>
                <asp:HyperLink runat="server" NavigateUrl="Admin/Insert.aspx" Text="Create new" />
            </p>
            <div>
                <asp:ListView runat="server"
                    DataKeyNames="ContactId" ItemType="ContactManager.Models.Contacts"
                    AutoGenerateColumns="false"
                    AllowPaging="true" AllowSorting="true"
                    SelectMethod="GetData">
                    <EmptyDataTemplate>
                        There are no entries found for Contacts
                    </EmptyDataTemplate>
                    <LayoutTemplate>
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>Name</th>
                                    <th>Address</th>
                                    <th>City</th>
                                    <th>State</th>
                                    <th>Zip</th>
                                    <th>Email</th>
                                    <th>&nbsp;</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr runat="server" id="itemPlaceholder" />
                            </tbody>
                        </table>
                    </LayoutTemplate>
                    <ItemTemplate>
                        <tr>
                            <td>
                                <asp:DynamicControl runat="server" DataField="Name" ID="Name" Mode="ReadOnly" />
                            </td>
                            <td>
                                <asp:DynamicControl runat="server" DataField="Address" ID="Address" Mode="ReadOnly" />
                            </td>
                            <td>
                                <asp:DynamicControl runat="server" DataField="City" ID="City" Mode="ReadOnly" />
                            </td>
                            <td>
                                <asp:DynamicControl runat="server" DataField="State" ID="State" Mode="ReadOnly" />
                            </td>
                            <td>
                                <asp:DynamicControl runat="server" DataField="Zip" ID="Zip" Mode="ReadOnly" />
                            </td>
                            <td>
                                <asp:DynamicControl runat="server" DataField="Email" ID="Email" Mode="ReadOnly" />
                            </td>
                            <td>
                                <a href="Admin/Edit.aspx?ContactId=<%#: Item.ContactId%>">Edit</a> | 
                                <a href="Admin/Delete.aspx?ContactId=<%#: Item.ContactId%>">Delete</a>
                            </td>
                        </tr>
                    </ItemTemplate>
                </asp:ListView>
            </div>
        </asp:Content>

4. 次の 3 つのファイルで 6 つ記述されている `Response.Redirect("Default.aspx")` コードを `Response.Redirect("~/Contacts/Default.aspx")` に更新します。  
    - *Delete.aspx.cs*
    - *Edit.aspx.cs*
    - *Insert.aspx.cs*  

    これで、データを表示および更新した際に、すべてのリンクが正常に動作するようになりました。
5. アクセスを制限する、 *Admin* フォルダーから、 **ソリューション エクスプ ローラー** を右クリックし、 *管理者* フォルダーを選択 **新しい項目の追加**します。
6. Visual c# Web テンプレートの一覧から選択 **Web 構成ファイル** 、中央の一覧から既定の名前を受け入れる *Web.config*, 、し、[ **追加**します。
7. 既存の XML の内容を置き換える、 *Web.config* を次のファイル。

        <?xml version="1.0"?>
        <configuration>
          <system.web>
            <authorization>
              <allow roles="canEdit"/>
              <deny users="*"/>
            </authorization>
          </system.web>
        </configuration>

8. 保存、 *Web.config* ファイルです。 
     *Web.config* ファイルに含まれるページを"canEdit"ロールに割り当てられたユーザーのみがアクセスできることを指定する、 *Admin* フォルダーです。 

"CanEdit"ロールの一部ではないユーザーがデータを変更しようとしたときにリダイレクトされます、 *ログイン* ページです。

##データベースが関連付けられたアプリケーションを Azure にデプロイする 
Web アプリケーションが完成したら、Azure に発行することができます。

###アプリケーションを発行する 
1. Visual Studio でプロジェクトをビルドします (**Ctrl + Shift + B**)。
2. プロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行**します。  
    ![[発行] メニュー オプション](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms22.png)  
     **Web の発行** ] ダイアログ ボックスが表示されます。  
    ![Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms22a.png)  
3.  **プロファイル** ] タブで [ **Azure App Service** が選択されていない場合は、発行先として。  
    ![Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms23.png)  
4. クリックして **サイン イン** 既にサインインしていない場合。
5. このチュートリアルで作成した既存の web アプリを選択、 **既存の Web アプリ** ドロップダウン ボックスし、をクリックして、 **[ok]** ] ボタンをクリックします。  
    ![既存の Web サイト] ダイアログ ボックスを選択します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms25.png)  
    プロファイルに対する変更の保存が確認された場合は、選択 **はい**します。
6. クリックして、 **設定** ] タブをクリックします。  
    ![既存の Web サイト] ダイアログ ボックスを選択します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms26.png)  
7. 設定、 **構成** ドロップダウン ボックスを **デバッグ**します。
8. クリックして、 **下向きの矢印** 隣にアイコン **ApplicationDbContext** に設定し、 **ContactDB**します。
9. チェック、 **Code First Migrations の実行** チェック ボックスをオンします。  
    この例では、アプリケーションを最初に発行する際にのみ、このチェック ボックスをオンにします。 これにより、 *シード* メソッドに、 *Configuration.cs* ファイルは一度だけ呼び出すことができます。  

10. クリックして **発行**します。  
    アプリケーションが Azure に発行されます。

>[AZURE.NOTE]  
発行プロファイルの作成後、Visual Studio を再起動した場合、接続文字列がドロップダウン リストに表示されない場合があります。 その場合は、先ほど作成した発行プロファイルを編集する代わりに、新しく作成前に行ったのと同じ方法と、次の手順、 **設定** ] タブをクリックします)。  

###Azure のアプリケーションを確認する 
1. ブラウザーで、クリックして、 **Contact Demo** リンクします。  
    [Contacts List] が表示されます。
    ![ブラウザーに表示される連絡先](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms27.png)  

2. 選択 **[新規作成** 上、 **連絡先リスト** ページです。  
    ![ブラウザーに表示される連絡先](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms29.png)  
    リダイレクトされます、 **ログイン** 連絡先の変更がアカウントを使用してログインしていない場合のページです。  
3. 次の電子メール アドレスとパスワードを入力すると、クリックして、 **ログイン** ] ボタンをクリックします。  
    **電子メール**: `canEditUser@wideworldimporters.com`  
    **パスワード**: `Pa$$word1`  
    ![ログイン ページ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms28.png)  

4. フィールドごとに新しいデータを入力してキーを押します、 **挿入** ] ボタンをクリックします。  
    ![新規追加] ページにお問い合わせください](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms30.png)  
     *EditContactList.aspx* ページには、新しいレコードが表示されます。  
    ![新規追加] ページにお問い合わせください](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms31.png)  

5. 選択、 **ログオフ** リンクします。

###アプリケーションを停止する 
他のユーザーによるサンプル アプリケーションへの登録や、アプリケーションの使用を避けるには、Web アプリを停止します。

1. Visual Studio で、 **表示] メニューの**, [ **サーバー エクスプ ローラー**します。 
2.  **サーバー エクスプ ローラー**, に移動 **Web アプリ**します。
3. 各 web アプリのインスタンスを右クリックして **Web アプリの停止**します。  
    ![メニュー項目の Web サイトを停止します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms26a.png)  

    または Azure クラシック ポータルからを web アプリを選択し、をクリックして、 **停止** ページの下部にあるアイコン。  
    ![新規追加] ページにお問い合わせください](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms26b.png)  

##データベースを確認する 
データベースを直接表示して変更する方法も理解しておく必要があります。 データベースを直接操作する方法を知っておくことで、データベースのデータを確認し、各テーブルにおけるデータの格納方法を把握できるようになります。

###SQL Azure DB を調べる 
1. Visual Studio で開きます **サーバー エクスプ ローラー** に移動して **ContactDB**します。
2. 右クリックして **ContactDB** 選択 **[SQL Server オブジェクト エクスプ ローラーで開く**します。  
    ![SQL Server オブジェクト エクスプ ローラーのメニュー項目で開く](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms32.png)  
3. 場合、 **ファイアウォール ルールの追加** ] ダイアログ ボックスが表示されたら、選択 **ファイアウォール ルールの追加**します。  
      展開することはできませんと **SQL データベース** と確認できない **ContactDB** Visual Studio から特定のファイアウォール ポートまたはポートの範囲を開く手順を行うことができます。 これを行うには、説明に従い **Azure ファイアウォール ルールを設定する** の終わり近く、 [MVC チュートリアル](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)します。 代わりに、確認することも、ローカル データベースのデータを構築、実行、およびアプリケーションをローカルにデータを追加して (**CTRL + F5** Visual Studio から)。  

4. 場合、 **サーバーへの接続** ] ダイアログ ボックスが表示されたら、入力、 **パスワード** 先頭のキーを押してこのチュートリアルで作成した、 **接続** ] ボタンをクリックします。  
      パスワードを思い出せない場合は、ローカルのプロジェクト ファイルから見つけることができます。  **ソリューション エクスプ ローラー**, 、展開、 *プロパティ* フォルダーの順に展開し、 *PublishProfiles* フォルダーです。 開いている、 *contactmanager.pubxml* ファイル (ファイルは異なる方法で命名されている可能性があります)。 発行パスワードを検索します。

5. 展開、 **contactDB** データベースし、順に展開 **テーブル**します。
6. 右クリックし、 **dbo です。AspNetUsers** テーブルが表示され選択 **データの表示**します。  
    ![メニュー項目のデータを表示します。](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms34.png)  
    canEditUser@contoso.com ユーザーに関連付けられているデータが表示されます。  
    ![ContactManager ウィンドウ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms35.png)  

###データベースを編集して、管理者ロールにユーザーを追加する 
先ほどは、コードを使用してユーザーを canEdit ロールに追加しました。 それ以外にも、メンバーシップ テーブル内のデータを直接操作する方法もあります。 以下の手順では、その方法を使用してロールにユーザーを追加します。

1.  **SQL Server オブジェクト エクスプ ローラー**, を右クリックして **dbo です。[AspNetUserRoles** 選択 **データの表示**します。
    ![[AspNetUserRoles data](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms36.png)  
2. コピー、 *RoleId* し、空の (新しい) 行に貼り付けます。  
    ![[AspNetUserRoles data](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms37.png)  
3.  **Dbo です。AspNetUsers** テーブルをロールを割り当てるユーザーを検索し、ユーザーをコピーする *Id*します。
4. コピーした *Id* に、 **UserId** に新しい行のフィールド、 **AspNetUserRoles** テーブルです。  

>[AZURE.NOTE]  
もっと簡単にユーザーとロールを管理できるように現在ツールを作成しています。  

##次のステップ
ASP.NET Web フォームの詳細については、次を参照してください。 [ASP.NET Web フォームについて学習](http://www.asp.net/web-forms) 、ASP.NET web アプリで、 [Microsoft Azure のチュートリアルとガイド](http://azure.microsoft.com/documentation/services/web-sites/#net)します。

このチュートリアルは MVC のチュートリアルに基づいてが [認証および SQL DB と ASP.NET MVC アプリの作成し、Azure App Service にデプロイ](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md) Rick anderson (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) と Tom Dykstra と Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart))。 

役に立った内容や改善点など、皆様からのご意見をお寄せください。このチュートリアルに関してだけでなく、ここで紹介した製品に関するご意見やご要望もお待ちしております。 お寄せいただいたご意見は、今後の改善に役立たせていただきます。 要求アンケートに新しいトピックに投票したり [Me 方法とコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。

 


