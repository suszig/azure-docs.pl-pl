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

## 概要

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
このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントを持っていない場合は、 <a href="/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F" target="_blank">、Visual Studio のサブスクライバーの特典をアクティブ化</a> または <a href="/pricing/free-trial/?WT.mc_id=A261C142F" target="_blank">無料評価版にサインアップ</a>します。 アカウントがサインアップする前に Azure を使ってみる場合は、 [App Service の試用](https://tryappservice.azure.com/), することができますをすぐに作成、有効期間の短い ASP.NET スターター サイトで、Azure は無料です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 開発環境を設定する

最初に、Visual Studio 2013 と Azure SDK for .NET をインストールして開発環境を設定します。

1. インストール [Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkId=306566), 、インストールされていない場合。
2. インストール [Azure SDK for Visual Studio 2013](http://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)します。このチュートリアルでは、Azure SDK for Visual Studio 2013 の前に Visual Studio 2013 をインストールする必要があります。マシンに既にある SDK の依存関係の数に応じて、SDK のインストールには長時間 (数分から 30 分以上) かかる場合があります。

3. インストール プログラムの実行または保存を求めるメッセージが表示されたら、**[実行]** をクリックします。
4. **Web Platform Installer** ウィンドウで、をクリックして **インストール** し、インストールを実行します。  
    ![Web プラットフォーム インストーラー](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-01.png)

      この SDK をインストール済みの場合、インストールされる項目の数は 0 になります。 インストールされる項目の数は **[Web Platform Installer]** ウィンドウの左下に表示されます。

5. まだしていない場合 **Visual Studio Update 2**, 、ダウンロードおよびインストール * *[Visual Studio 2013 Update 2](http://www.microsoft.com/download/details.aspx?id=42666)* * 以降。
    >[AZURE.NOTE]  
    Goggle OAuth 2.0 を使用するには、また、警告を表示させずに SSL をローカルで使用するには、Visual Studio 2013 Update 2 以降をインストールする必要があります。 また、Web フォームのスキャフォールディングを使用するには Update 2 が必要です。  

インストールが完了すると、開発に必要なツールがすべて揃います。

## Azure 環境を設定する

このセクションでは、Azure と SQL データベースを Azure 内に作成して、Azure 環境を設定します。

### Azure で Web アプリと SQL Database を作成する

このチュートリアルでは、Web アプリが共有ホスティング環境で実行されます。つまり、Web アプリを実行する仮想マシン (VM) が、Azure App Service の他の Web アプリと共有されています。 共有ホスティング環境は、低コストでクラウドの利用を開始できる方法です。 後で Web トラフィックが増加したら、アプリケーションの規模を変更して専用 VM 上で実行するように設定してニーズを満たすことができます。 もっと複雑なアーキテクチャが必要な場合は、Azure のクラウド サービスに移行できます。 クラウド サービスは専用 VM 上で実行され、ユーザーのニーズに応じて構成できます。

Azure SQL Database は、SQL Server テクノロジに基づいて構築されたクラウドベースのリレーショナル データベース サービスです。 SQL Server で動作するツールおよびアプリケーションは、SQL Database でも動作します。

1. [Azure クラシック ポータル](https://manage.windowsazure.com/), 、クリックして **Web Apps** 左側のタブをクリックし **新規**します。  
    ![Web プラットフォーム インストーラー](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-02.png)
2. **[Web アプリ]**、**[カスタム作成]** の順にクリックします。  
    ![カスタム作成](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-03.png)  
    **新しい Web アプリ - カスタム作成** ウィザードが開きます。

3. ウィザードの **[Web アプリの作成]** 手順で、アプリケーションの一意の URL として使用する文字列を **[URL]** ボックスに入力します。 ここに入力した文字列と、このテキスト ボックスの右側に表示されている文字列を組み合わせたものが実際の URL になります。 図に表示されているため URL **別の URL を選択する必要があります**します。  
    ![連絡先 - 新規 Web サイト作成](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-04.png)
4. [Web ホスティング プラン] ドロップダウン リストで、現在の所在地に最も近いリージョンを選択します。 この設定によって、使用する VM が実行されるデータ センターが指定されます。
5. **[データベース]** ボックスの一覧の **[無料の 20 MB SQL データベースの作成]** を選択します。
6. **[DB 接続文字列名]** ボックスは、既定値の *DefaultConnection* をそのまま使用します。
7. ボックスの下部にある矢印をクリックします。 
ウィザードの **[データベースの設定の指定]** 手順に進みます。
8. **名** 入力ボックスで、*`ContactDB`* します。  
    ![データベース設定](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/Intro-SecureWebForms-05.png)
9. **[サーバー]** ボックスで、**[新しい SQL データベース サーバー]** を選択します。
または、以前に SQL Server データベースを作成した場合は、ボックスの一覧からその SQL Server を選択できます。
10. **[リージョン]** は、作成した Web アプリと同じエリアに設定します。
11. 管理者の**ログイン名**と**パスワード**を入力します。 
**[新しい SQL データベース サーバー]** を選択した場合は、既存の名前とパスワードではなく、このデータベースへのアクセス時に使用する新しい名前とパスワードを入力してください。 以前に作成した SQL Server を選択した場合は、その SQL Server の作成時に設定したパスワードを入力します。 このチュートリアルでは、**[データベースの詳細設定を構成します]** チェック ボックスをオンにしません。
12. 終了したら、ダイアログ ボックスの右下にあるチェック マークをクリックします。

**Azure クラシック ポータル**が **[Web Apps]** ページに戻り、**[状態]** 列にサイトが作成中であることが示されます。 しばらくすると (通常は 1 分未満)、サイトの作成に成功したことが **[状態]** 列に示されます。 左側にあるナビゲーション バーでは、アカウントで所有するサイト数が **[Web アプリ]** アイコンの横に表示され、データベース数が **[SQL データベース]** アイコンの横に表示されます。
## ASP.NET Web フォーム アプリケーションを作成する

Web アプリを作成しましたが、まだその中にコンテンツがありません。 次のステップでは、Azure に発行する Visual Studio Web アプリケーションを作成します。
### プロジェクトを作成する

1. Visual Studio の **[ファイル]** メニューで **[新しいプロジェクト]** を選択します。  
    ![File Menu - New Project](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms01.png)
2. 左側で、**[テンプレート]**、**[Visual C#]**、**[Web]** テンプレート グループの順に選択します。
3. 中央の列で **[ASP.NET Web アプリケーション]** を選択します。
4. プロジェクトに名前を *ContactManager* ] をクリック **OK**します。  
    ![[新しいプロジェクト] ダイアログ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms02.png)

      このチュートリアル シリーズでは、プロジェクトの名前を **ContactManager** としています。 チュートリアル シリーズのさまざまな場面で提供されるコードを説明どおりに動作させるため、まったく同じ名前を使用することをお勧めします。

5. **[新しい ASP.NET プロジェクト]** ダイアログ ボックスで、**[Web フォーム]** テンプレートを選択します。 **[クラウドでのホスト]** チェック ボックスをオフにして (オンになっている場合)、**[OK]** をクリックします。  
    ![New ASP.NET Project dialog box](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms03.png)  
    Web フォーム アプリケーションが作成されます。

### マスター ページを更新する

ASP.NET Web フォームでは、マスター ページを使うことで、アプリケーションのページに共通のレイアウトを適用することができます。 これには、アプリケーションのすべてのページ (またはページ グループ) に適用する外観、操作性、標準の動作を定義するマスター ページを 1 つ作成します。 次に、表示するコンテンツが格納されるコンテンツ ページを個別に作成します。 ユーザーがコンテンツ ページを要求すると、ASP.NET はコンテンツ ページとマスター ページをマージします。これによって、マスター ページのレイアウトとコンテンツ ページのコンテンツが組み合わされ、出力されます。
今回の新しいサイトでは、アプリケーションの名前とリンクを更新する必要があります。 このリンクは、連絡先の詳細を表示するページを参照するリンクです。 これらの変更を実施するには、マスター ページの HTML を変更します。

1. **ソリューション エクスプローラー**で *Site.Master* ページを見つけて開きます。
2. ページが **[デザイン]** ビューで表示されている場合は **[ソース]** ビューに切り替えます。
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

### ローカルでアプリケーションを実行する

1. **ソリューション エクスプローラー**で *Default.aspx* ページを右クリックし、**[スタート ページに設定]** を選択します。
2. **Ctrl キーを押しながら F5 キーを押して**アプリケーションを実行します。  
    アプリケーションの既定のページが、既定のブラウザー ウィンドウに表示されます。
    ![連絡先 - 新規 Web サイト作成](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms04.png)

これで、Azure にデプロイするアプリケーションを作成するために必要な操作が完了しました。 データベースの機能と、連絡先データを表示および編集するためのページは、後ほど追加します。
### Azure にアプリケーションをデプロイする

現在、アプリケーションを作成してローカルで実行しています。このアプリケーションを Azure にデプロイします。

1. Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、コンテキスト メニューの **[発行]** をクリックします。  
    ![[発行] の選択](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms05.png)  
    **[Web の発行]** ダイアログ ボックスが表示されます。

2. **[Web の発行]** ダイアログ ボックスの **[プロファイル]** タブで、**[Azure Web アプリ]** をクリックします。

3. まだサインインしていない場合、**[既存の Web アプリを選択]** ダイアログ ボックスの **[サインイン]** をクリックします。 サインインしたら、このチュートリアルの最初に作成した Web アプリを選択します。 **[OK]** をクリックして続行します。  
    ![既存の Web サイト選択ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms07.png)  
Visual Studio によって発行設定がダウンロードされます。
4. **[Web の発行]** ダイアログ ボックスの **[発行]** をクリックします。  
    ![[Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms08.png)  
Visual Studio の **[Web 発行アクティビティ]** ウィンドウに、発行の全体的なステータスが表示されます。  
    ![Web Publishing Activity](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms09.png)

これで、作成したアプリケーションはクラウドで実行されています。 このアプリケーションを次に Visual Studio からデプロイするときは、変更したファイル (または新しいファイル) のみがデプロイされます。  
    ![ブラウザーでのアプリ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms10.png)
>[AZURE.NOTE] 
既に存在する Web アプリへの発行でエラーが発生した場合、その発行先を消去してから新しいファイルを追加します。  
その後、アプリケーションをもう一度発行しますが、**[Web の発行]** ダイアログ ボックスで **[設定]** タブを選択します。 次に構成に **[デバッグ]** を設定して、**[発行先のその他のファイルを削除]** オプションを選択し、 選択 **発行** をもう一度アプリケーションを展開します。  
    ![[Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms11.png)  

## アプリケーションにデータベースを追加する

次に、Web フォーム アプリケーションを更新して、連絡先を表示および更新して既定のデータベースにデータを保存する機能を追加します。 Web フォーム プロジェクトを作成した際にデータベースも既定で作成されています。 アプリケーションは Entity Framework を使用してこのデータベースにアクセスし、データベース内のデータを更新します。
### データ モデル クラスを追加する

まず、コードを使ってシンプルなデータ モデルを作成します。 このデータ モデルと呼ばれるクラス内に含まれる `連絡先`します。  `連絡先` クラス名が選択されたとクラス名の競合を回避する、 `連絡先` クラスは、Web Forms テンプレートが作成した Contact.aspx.cs ファイルに含まれています。

1. **ソリューション エクスプローラー**で *Models* フォルダーを右クリックし、**[追加]**、**[クラス]** の順にクリックします。  
    ![クラスの選択](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms12.png)  
    **新しい項目の追加** ] ダイアログ ボックスが表示されます。

2. この新しいクラスの名前を *Contacts.cs* ] をクリック **追加**します。  
    ![Add New Item dialog box](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13.png)
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


**連絡先** クラスのそれぞれの連絡先とする主キーを格納するデータ定義 (`ContactID`)、データベースが必要です。 **Contacts** クラスは、表示される連絡先データを表しています。 Contacts オブジェクトの各インスタンスはリレーショナル データベース テーブルの各行に相当し、Contacts クラスの各プロパティは、リレーショナル データベース テーブルの各列に関連付けられます。 データベースに格納されている連絡先データについては、このチュートリアルで後ほど説明します。

### Web フォームのスキャフォールディング

現在 **Contacts** モデル クラスが作成されています。 次は、このデータの操作に使用される *List*、*Insert*、*Edit*、*Delete* の各ページを、Web Forms Scaffolder を使って生成します。 Web Forms Scaffolder は、Entity Framework、ブートストラップ、動的データを使用します。 既定では、Web Forms Scaffolder はプロジェクト テンプレートに含まれており、Visual Studio 2013 を使用する際にプロジェクトの拡張機能としてインストールされます。

次の手順を実行することで、Web Forms Scaffolder を使用できるようになります。

1. Visual Studio のメニュー バーで **[ツール]**、**[拡張機能と更新プログラム]** の順に選択します。  
    **[拡張機能と更新プログラム]** ダイアログ ボックスが表示されます。
2. ダイアログ ボックスの左側のウィンドウで、**[オンライン]**、**[Visual Studio ギャラリー]**、**[ツール]**、**[スキャフォールディング]** の順に選択します。
3. 「Web Forms Scaffolding」が一覧に表示されない場合は、ダイアログ ボックス右側の検索ボックスに「Web Forms Scaffolding」と入力します。
4. Forms Scaffolder がインストールされていない場合は、**[ダウンロード]** を選択して Web Forms Scaffolding をインストールします。 必要に応じて Visual Studio を再起動します。 確認された場合は、プロジェクトへの変更を必ず保存してください。  
    ![[拡張機能と更新プログラム] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/ExtensionsAndUpdatesDB.png)
5. プロジェクトをビルドします (**Ctrl + Shift + B**)。  
    プロジェクトは、スキャフォールディング機能を使用する前にビルドする必要があります。
6. **ソリューション エクスプローラー**で*プロジェクト*を右クリックし、**[追加]**、**[新しいスキャフォールディング項目]** の順に選択します。  
    **[スキャフォールディングの追加]** ダイアログ ボックスが表示されます。
7. 左側のウィンドウで **[Web フォーム]** を選択し、中央のウィンドウで **[Entity Framework を使用している Web フォーム]** を選択して、 **[追加]** をクリックします。  
    ![[スキャフォールディングの追加] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13a.png)  
    **[Web フォーム ページの追加]** ダイアログ ボックスが表示されます。

8. **Add Web Forms Pages** ダイアログ ボックスで、セット、 **モデル クラス** に `連絡先 (ContactManager.Models)`します。 設定、 **データ コンテキスト クラス** に `ApplicationDbContext (ContactManager.Models)`します。 **[追加]** をクリックします。 
    ![Add Web Forms Pages dialog box](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13b.png)

Web Forms Scaffolder によって、*Default.aspx*、*Delete.aspx*、*Edit.aspx*、*Insert.aspx* の各ページを格納する新しいフォルダーが作成されます。 また、*DynamicData* フォルダーも作成されます。このフォルダーには *EntityTemplates* フォルダーと *FieldTemplates* フォルダーが格納されています。  `ApplicationDbContext` メンバーシップ データベースと連絡先データの両方に使用されます。

### データ モデルを使用するようにアプリケーションを構成する

次の作業では、作成したデータ モデルに基づいてデータベースを作成するための Code First Migrations 機能を有効化すると共に、 サンプル データとデータ初期化子を追加します。

1. **[ツール]** メニューの **[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** を順に選択します。  
    ![[Web フォーム ページの追加] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13c.png)
2. [パッケージ マネージャー コンソール] ウィンドウで、次のコマンドを入力します。

        enable-migrations

    enable-migrations コマンドによって *Migrations* フォルダーが作成され、そのフォルダーに *Configuration.cs* ファイルが格納されます。このファイルを編集して、データベースに初期データを投入し、データ移行を構成します。
3. **[パッケージ マネージャー コンソール]** ウィンドウで、次のコマンドを入力します。

        add-migration Initial

    `、Add-migration Initial` という名前のファイルを生成するコマンド <date_stamp>Initial、 *移行* 、データベースを作成するフォルダーです。最初のパラメーター (Initial) は任意です。このパラメーターは、このファイルの名前の作成に使用されます。新しいクラス ファイルは**ソリューション エクスプローラー**で表示できます。 `初期` クラス、 `を` メソッドを作成、 `連絡先` テーブル、および `ダウン` (前の状態に戻したい場合に使用) メソッドを使用します。
4. *Migrations\Configuration.cs* ファイルを開きます。

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
7. **[パッケージ マネージャー コンソール]** で、次のコマンドを入力します。

        update-database


`update-database` によって、データベースを作成する最初の移行が実行されます。 既定では、データベースは SQL Server Express LocalDB データベースとして作成されます  
    ![パッケージ マネージャー コンソール](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms13d.png)

### アプリケーションをローカルで実行してデータを表示する

次に、アプリケーションを実行して連絡先の表示を確認します。

1. 最初にプロジェクトをビルドします (**Ctrl + Shift + B**)。
2. **Ctrl キーを押しながら F5 キーを押して**アプリケーションを実行します。  
    ブラウザーが開き、*Default.aspx* ページが表示されます。
3. ページ最上部の **Contact Demo** リンクを選択して *Contact List* ページを表示します。  
    ![Contacts List Page](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms17.png)

## プロジェクトに対して SSL を有効にする

Secure Sockets Layer (SSL) は、Web サーバーと Web クライアント間の通信に定義されたプロトコルで、暗号化によって通信の安全性を強化することができます。 SSL を使わないと、クライアントとサーバー間で送信されるデータが、ネットワークに物理的にアクセスできる第三者によるパケット スニッフィングの標的になります。 また、プレーンな HTTP を使用すると、いくつかの一般的な認証方式の安全性も低下します。 具体的には、基本認証とフォーム認証で送信する資格情報が暗号化されません。 安全性を確保するには、これらの認証方式で SSL を使用する必要があります。

1. **ソリューション エクスプローラー**で **ContactManager** プロジェクトをクリックし、**F4 キー**を押して **[プロパティ]** ウィンドウを表示します。
2. 変更 **SSL が有効になっている** に `true`します。
3. 後で使用するための **SSL URL** をコピーします。  
    SSL URL になります `https://localhost:44300/` 以前作成した SSL web アプリ (下図)。
    ![プロジェクトのプロパティ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms18.png)
4. **ソリューション エクスプローラー**で **Contact Manager** プロジェクトを右クリックし、**[プロパティ]** をクリックします。
5. 左側のタブで **[Web]** をクリックします。
6. 変更、 **プロジェクト Url** を使用する、 **SSL URL** 以前に保存します。  
    ![プロジェクトの Web プロパティ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms19.png)
7. **CTRL + S キーを押して**ページを保存します。
8. **Ctrl キーを押しながら F5 キーを押して**アプリケーションを実行します。  
Visual Studio により、SSL の警告を回避するためのオプションが表示されます。
9. IIS Express SSL 証明書を信頼する場合は **[はい]** をクリックして続行します。  
    ![IIS Express SSL 証明書の詳細](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms20.png)  
    セキュリティ警告が表示されます。

10. **[はい]** をクリックしてローカルホストに証明書をインストールします。  
    ![セキュリティ警告ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21.png)  
    ブラウザー ウィンドウが表示されます。

ローカルで SSL を使用して Web アプリケーションを簡単にテストできるようになりました。



## OAuth 2.0 プロバイダーを追加する

ASP.NET Web フォームは、メンバーシップと認証のオプションが強化されています。 OAuth もこうした強化点の 1 つです。 OAuth は、Web、モバイル、およびデスクトップのアプリケーションからシンプルで標準的な方法で安全に認証するためのオープン プロトコルです。 ASP.NET MVC インターネット テンプレートは OAuth を使用して、Facebook、Twitter、Google、Microsoft を認証プロバイダーとしてサポートします。 このチュートリアルでは Google のみを認証プロバイダーとして使用しますが、コードを少し変更すれば他のプロバイダーも使用できます。 他のプロバイダーを実装する手順は、このチュートリアルで説明する手順とほとんど同じです。

このチュートリアルでは、認証の他にロールを使用して権限を付与します。 追加したユーザーのみ、 `canEdit` ロールがデータを変更できます (作成、編集、または連絡先を削除) します。

次の手順を実行することで、Google 認証プロバイダーを追加できます。

1. *App_Start\Startup.Auth.cs* ファイルを開きます。
2. コメント文字を削除して、から `アプリです。UseGoogleAuthentication()` メソッドとしてメソッドが表示されるように依存します。

        app.UseGoogleAuthentication(new GoogleOAuth2AuthenticationOptions()
        {
            ClientId = "",
            ClientSecret = ""
        });

3. 移動し、 [Google Developers Console](https://console.developers.google.com/)します。 Google デベロッパーの電子メール アカウント (gmail.com) でサインインする必要があります。 Google アカウントを持っていない場合は、**[Create an account]** リンクを選択します。  
    次に確認、 **Google Developers Console**します。
    ![Google Developers Console](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21a.png)

4. **[Select a project]** から**[Create Project]** ボタンをクリックして、プロジェクトの名前と ID を入力します (既定値を使用できます)。 **同意のチェック ボックス**をオンにして、**[Create]** ボタンをクリックします。  
    ![Google - 新しいプロジェクト](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21b.png)  
    新しいプロジェクトが数秒で作成され、新しいプロジェクトのページがブラウザーに表示されます。
5. **Google Developers Console** ドロップダウン メニューから、 **[API Manager]**、**[Credentials]** の順にクリックします。
6. **[OAuth]** にある **[Create New Client ID]** をクリックします。  
    **Create Client ID** ダイアログが表示されます。
    ![Google - クライアント ID の作成](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21c.png)
7. **[Create Client ID]** ダイアログ ボックスでは、アプリケーションの種類を既定の **[Web application]** のままにします。
8. 設定、 **Authorized JavaScript Origins** にこのチュートリアルで既に使用した SSL URL (**https://localhost:44300/** 他の SSL プロジェクトを作成した)。  
    この URL がアプリケーションのオリジンです。 このサンプルではローカルホストのテスト URL を入力するだけですが、 アカウントには複数の URL を入力できます (ローカルホストおよび本稼働用)。

9. **[Authorized Redirect URI]** には次の値を設定します。

        https://localhost:44300/signin-google  

    この値は、ASP.NET OAuth ユーザーが Google の OAuth サーバーとの通信に使用する URI です。 上記で使用した SSL URL を忘れないでください (**https://localhost:44300/** 他の SSL プロジェクトを作成した)。

10. **[Create]** ボタンをクリックします。
11. Visual Studio で、更新、 `UseGoogleAuthentication` のメソッド、 *Startup.Auth.cs* コピー アンド ペーストでページ、 **AppId** と **App Secret** メソッドにします。 次に示す**アプリケーション ID** と**アプリケーション シークレット**の値はサンプルであり、動作しません。

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

12. **Ctrl キーを押しながら F5 キーを押して**アプリケーションをビルドし、実行します。 **[Log in]** リンクをクリックします。
13. **別のサービスを使用してログイン**, をクリックして **Google**します。  
    ![Log in](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21d.png)
14. 資格情報の入力が必要な場合、Google のサイトにリダイレクトされるので、リダイレクト先のサイトで資格情報を入力します。  
    ![Google - サインイン](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21e.png)
15. 資格情報を入力すると、先ほど作成した Web アプリケーションへの権限の付与が求められます。  
    ![プロジェクトのデフォルト サービス アカウント](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21f.png)
16. **[Accept]** をクリックします。 今すぐにリダイレクトされます、 **登録** のページ、 **ContactManager** アプリケーションが Google アカウントを登録することができます。
    ![Google アカウントへの登録](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms21g.png)
17. Google アカウントに使用するローカルの電子メール登録名を変更できますが、通常は既定の電子メール エイリアス (認証に使用したエイリアス) を変更しません。 **[Log in]** をクリックします。

## メンバーシップ API を使用してアクセスを制限する

ASP.NET アプリケーションの開発では、認証に ASP.NET Identity というメンバーシップ システムを使用できます。 ASP.NET Identity によって、ユーザー固有のプロファイル データとアプリケーション データを簡単に統合できるだけでなく、 アプリケーションのユーザー プロファイルに永続性を適用することができます。 データは、SQL Server データベースやその他のデータ ストア (Azure Storage テーブルなどの *NoSQL* データ ストアを含む) に保存できます。

既定の ASP.NET Web フォーム テンプレートを使用する場合は、あらかじめ組み込まれているメンバーシップ機能をアプリケーション実行時にすぐに使うことができます。 ここでは、ASP.NET Identity を使用して管理者ロールを追加し、このロールにユーザーを割り当てます。 その後、連絡先データの変更に使用される、管理者用フォルダーおよび同フォルダー内のページへのアクセスを制限する方法について説明します。

### 管理者を追加する

ASP.NET Identity を使用すると、管理者ロールの追加および管理者ロールへのユーザーの割り当てを、コードを使って実行できます。

1. **ソリューション エクスプローラー**で、*Migrations* フォルダーの *Configuration.cs* ファイルを開きます。
2. 次の追加 `を使用して` 内のステートメント、 `'contactmanger.migrations'` 名前空間。

        using Microsoft.AspNet.Identity;
        using Microsoft.AspNet.Identity.EntityFramework;

3. 次の追加 `AddUserAndRole` メソッドを `構成` クラスの後に、 `シード` メソッド。

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

4. 呼び出しを追加して、 `AddUserAndRole` 内の先頭からメソッド、 `シード` メソッドです。 先頭だけに注意してください、 `シード` メソッドが表示されています。

        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            AddUserAndRole(context);

5. すべての変更を保存したら、**パッケージ マネージャー コンソール**で次のコマンドを実行します。

        Update-Database


このコードと呼ばれる新しいロールを作成する `canEdit` され、canEditUser@wideworldimporters.com の電子メールが、新しいローカル ユーザーを作成します。 次に canEditUser@wideworldimporters.com に追加コード、 `canEdit` ロールです。 詳細については、次を参照してください。、 [ASP.NET Identity](http://www.asp.net/identity) リソース ページです。

### 管理者用フォルダーへのアクセスを制限する

現在、サンプル アプリケーション **ContactManager** では、匿名ユーザーとログイン ユーザーの両方が連絡先を表示できますが、 このセクションを完了すると、連絡先を追加できるのは canEdit ロールが割り当てられたログイン ユーザーだけになります。

canEdit ロールが割り当てられたユーザーだけがアクセスできる、*Admin* という名前のフォルダーを作成します。

1. **ソリューション エクスプローラー**で、*Contacts* フォルダーにサブフォルダーを追加し、新しいサブフォルダーに「*Admin*」という名前を付けます。
2. 次に示すファイルを *Contacts* フォルダーから *Contacts/Admin* フォルダーに移動します。
    - *Delete.aspx * および* Delete.aspx.cs*
    - *Edit.aspx * および* Edit.aspx.cs*
    - *Insert.aspx * および* Insert.aspx.cs*
3. *Contacts/Default.aspx* 内のリンク参照を更新します。これには、次のように *Insert.aspx*、*Edit.aspx*、*Delete.aspx* にリンクしているページ参照の前に "Admin/" を追加します。

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

4. 6 つの参照を更新、 `Response.Redirect("Default.aspx")` コード `Response.Redirect("~/Contacts/Default.aspx")` の次の 3 つのファイル。
    - *Delete.aspx.cs*
    - *Edit.aspx.cs*
    - *Insert.aspx.cs*

    これで、データを表示および更新した際に、すべてのリンクが正常に動作するようになりました。
5. *Admin* フォルダーへのアクセスを制限するには、**ソリューション エクスプローラー**で *Admin* フォルダーを右クリックして、**[新しい項目の追加]** を選択します。
6. Visual C# Web テンプレートの一覧で、中央の一覧にある **[Web 構成ファイル]** を選択し、既定の名前「*Web.config*」のままで **[追加]** を選択します。
7. *Web.config* ファイルの XML の内容を次の内容で置き換えます。

        <?xml version="1.0"?>
        <configuration>
          <system.web>
            <authorization>
              <allow roles="canEdit"/>
              <deny users="*"/>
            </authorization>
          </system.web>
        </configuration>

8. *Web.config* ファイルを保存します。 
    *Web.config* の指定によって、"canEdit" ロールが割り当てられたユーザー以外は *Admin* フォルダーに格納されたページにアクセスできなくなりました。

canEdit ロールが割り当てられていないユーザーがデータを変更しようとすると、[*Log in*] ページにリダイレクトされます。

## データベースが関連付けられたアプリケーションを Azure にデプロイする

Web アプリケーションが完成したら、Azure に発行することができます。

### アプリケーションを発行する

1. Visual Studio でプロジェクトをビルドします (**Ctrl + Shift + B**)。
2. **ソリューション エクスプローラー**で、プロジェクトを右クリックして **[発行]** をクリックします。  
    ![[発行] メニュー オプション](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms22.png)  
    **[Web の発行]** ダイアログ ボックスが表示されます。  
    ![[Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms22a.png)
3. **プロファイル** ] タブで [ **Azure App Service** が選択されていない場合は、発行先として。  
    ![[Web の発行] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms23.png)
4. **[サインイン]** をクリックします (サインインしていない場合)。
5. **[既存の Web アプリ]** ドロップダウン ボックスで、このチュートリアルで作成した Web アプリを選択し、**[OK]** をクリックします。  
    ![既存の Web サイト選択ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms25.png)  
    プロファイルに対する変更の保存が確認された場合は、選択 **はい**します。
6. **[設定]** タブをクリックします。  
    ![Select Existing Web Site dialog box](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms26.png)
7. **[構成]** ドロップダウン ボックスを **[デバッグ]** に設定します。
8. **[ApplicationDbContext]** の**下向き矢印**アイコンをクリックして、**ContactDB** に設定します。
9. **[Code First Migrations を実行]** チェック ボックスをオンにします。  
    この例では、アプリケーションを最初に発行する際にのみ、このチェック ボックスをオンにします。 これによって、*Configuration.cs* ファイルの *Seed* メソッドが 1 回だけ呼び出されます。

10. **[発行]** をクリックします。  
    アプリケーションが Azure に発行されます。

>[AZURE.NOTE]  
発行プロファイルの作成後、Visual Studio を再起動した場合、接続文字列がドロップダウン リストに表示されない場合があります。 その場合は、先ほど作成した発行プロファイルを編集する代わりに、先ほどと同じ方法で新しい発行プロファイルを作成し、以降の **[設定]** タブの手順に従ってください。)  

### Azure のアプリケーションを確認する

1. ブラウザーで **[Contact Demo]** リンクをクリックします。  
    [Contacts List] が表示されます。
    ![ブラウザーでの連絡先一覧](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms27.png)

2. **[Contacts List]** ページで **[Create New]** を選択します。  
    ![ブラウザーでの連絡先一覧](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms29.png)  
    リダイレクトされます、 **ログイン** 連絡先の変更がアカウントを使用してログインしていない場合のページです。
3. 次の電子メール アドレスとパスワードを入力して **[Log in]** ボタンをクリックします。  
    **電子メール**: `canEditUser@wideworldimporters.com`  
    **パスワード**: `Pa$ word1`  
    ![ログイン ページ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms28.png)

4. 各フィールドに新しいデータを入力して **[Insert]** ボタンをクリックします。  
    ![新規連絡先追加ページ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms30.png)  
    *EditContactList.aspx* ページには、新しいレコードが表示されます。  
    ![新規連絡先追加ページ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms31.png)

5. **[Log off]** リンクを選択します。

### アプリケーションを停止する

他のユーザーによるサンプル アプリケーションへの登録や、アプリケーションの使用を避けるには、Web アプリを停止します。

1. Visual Studio の **[表示]** メニューで **[サーバー エクスプローラー]** を選択します。
2. **サーバー エクスプローラー**で **[Web アプリ]** に移動します。
3. 各 web アプリのインスタンスを右クリックして **Web アプリの停止**します。  
    ![Web サイト停止メニュー項目](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms26a.png)

    または Azure クラシック ポータルからを web アプリを選択し、をクリックして、 **停止** ページの下部にあるアイコン。  
    ![新規連絡先追加ページ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms26b.png)

## データベースを確認する

データベースを直接表示して変更する方法も理解しておく必要があります。 データベースを直接操作する方法を知っておくことで、データベースのデータを確認し、各テーブルにおけるデータの格納方法を把握できるようになります。

### SQL Azure DB を調べる

1. Visual Studio で**サーバー エクスプローラー**を開き、**[ContactDB]** に移動します。
2. **[ContactDB]** を右クリックし、**[SQL Server オブジェクト エクスプローラーで開く]** を選択します。  
    ![Open in SQL Server Object Explorer menu item](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms32.png)
3. **[ファイアウォール ルールの追加]** ダイアログ ボックスが表示されたら、**[ファイアウォール ルールを追加]** をクリックします。  
      **[SQL データベース]** を展開できず、**ContactDB** を Visual Studio で表示できない場合は、手順に従って特定のファイアウォール ポート (またはポート範囲) を開放する必要があります。 これを行うには、説明に従い **Azure ファイアウォール ルールを設定する** の終わり近く、 [MVC チュートリアル](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md)します。 または、アプリケーションをローカルでビルド、実行し (Visual Studio で **Ctrl キーを押しながら F5 キーを押す**)、データを追加して、ローカル データベースのデータを確認する方法もあります。

4. **[サーバーへの接続]** ダイアログ ボックスが表示された場合、このチュートリアルの冒頭で作成した**パスワード**を入力して **[接続]** ボタンをクリックします。  
      パスワードを思い出せない場合は、ローカルのプロジェクト ファイルから見つけることができます。 これには、**ソリューション エクスプローラー**で *Properties* フォルダー、*PublishProfiles* フォルダーの順に展開し、 *contactmanager.pubxml* ファイルを開き (別の名前が付けられている可能性があります)、 発行パスワードを検索します。

5. **[contactDB]** データベース、**[テーブル]** の順に展開します。
6. **[dbo.AspNetUsers]** テーブルを右クリックし、**[データの表示]** を選択します。  
    ![[データの表示] メニュー項目](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms34.png)  
    canEditUser@contoso.com ユーザーに関連付けられているデータが表示されます。  
    ![ContactManager ウィンドウ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms35.png)

### データベースを編集して、管理者ロールにユーザーを追加する

先ほどは、コードを使用してユーザーを canEdit ロールに追加しました。 それ以外にも、メンバーシップ テーブル内のデータを直接操作する方法もあります。 以下の手順では、その方法を使用してロールにユーザーを追加します。

1. **SQL Server オブジェクト エクスプ ローラー**, 、を右クリックして **dbo です。[AspNetUserRoles** 選択 **データの表示**します。
    ![AspNetUserRoles データ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms36.png)
2. *RoleId* をコピーして空の (新しい) 行に貼り付けます。  
    ![AspNetUserRoles データ](./media/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/SecureWebForms37.png)
3. **dbo.AspNetUsers** テーブルで、ロールを割り当てるユーザーを特定し、ユーザーの *ID* をコピーします。
4. コピーした *ID* を、**AspNetUserRoles** テーブルの新しい行の **[UserId]** フィールドに追加します。

>[AZURE.NOTE]  
もっと簡単にユーザーとロールを管理できるように現在ツールを作成しています。  

## 次のステップ

ASP.NET Web フォームの詳細については、次を参照してください。 [ASP.NET Web フォームについて学習](http://www.asp.net/web-forms) 、ASP.NET web アプリで、 [Microsoft Azure のチュートリアルとガイド](http://azure.microsoft.com/documentation/services/web-sites/#net)します。

このチュートリアルは MVC のチュートリアルに基づいてが [認証および SQL DB と ASP.NET MVC アプリの作成し、Azure App Service にデプロイ](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md) Rick anderson (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) と Tom Dykstra と Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart))します。

役に立った内容や改善点など、皆様からのご意見をお寄せください。このチュートリアルに関してだけでなく、ここで紹介した製品に関するご意見やご要望もお待ちしております。 お寄せいただいたご意見は、今後の改善に役立たせていただきます。 要求アンケートに新しいトピックに投票したり [Me 方法とコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。







