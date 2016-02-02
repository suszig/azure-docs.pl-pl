<properties 
    pageTitle="認証および SQL DB を使用する ASP.NET MVC アプリの作成と、Azure App Service へのデプロイ" 
    description="SQL Database バックエンドを使用して ASP.NET MVC 5 Web アプリを作成し、認証および承認を追加して、それを Azure にデプロイする方法について説明します。" 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="Rick-Anderson" 
    writer="Rick-Anderson" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="get-started-article" 
    ms.date="12/07/2015" 
    ms.author="riande"/> 


# 認証および SQL DB を使用する ASP.NET MVC アプリの作成と、Azure App Service へのデプロイ

このチュートリアルでは、ユーザーが Facebook または Google の資格情報を使用してログインできる、セキュリティで保護された ASP.NET MVC 5 Web アプリを構築する方法について説明します。 このアプリは、データベースへのアクセスに ADO.NET Entity Framework を使用する簡単な連絡先リストです。 アプリを展開する [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)します。

このチュートリアルを完了すると、クラウド データベースを使用し、セキュリティで保護されたデータ主導型 Web アプリケーションをクラウドで起動して実行できるようになります。 次の図は、完成したアプリケーションのログイン ページを示しています。

![login page][rxb]

学習内容:

* Visual Studio で、セキュリティで保護された ASP.NET MVC 5 Web プロジェクトを作成する方法
* Google または Facebook アカウントの資格情報でログオンしたユーザー認証および承認する方法 (ソーシャル プロバイダーの認証を使用して [OAuth 2.0](http://oauth.net/2 "http://oauth.net/2"))。
* アプリケーションによって管理されているデータベースに登録されているユーザー認証および承認する方法 (ローカル認証を使用して [ASP.NET Identity](http://asp.net/identity/))します。
* ADO.NET Entity Framework 6 の Code First を使用して、SQL データベースのデータを読み書きする方法
* Entity Framework の Code First Migrations を使用してデータベースをデプロイする方法
* Azure SQL Database を使用してリレーショナル データをクラウドに保存する方法
* データベースを使用する web プロジェクトをデプロイする方法、 [web アプリ](http://go.microsoft.com/fwlink/?LinkId=529714) Azure App Service でします。

>[AZURE.NOTE] これは長いチュートリアルです。 Azure App Service と Visual Studio の web プロジェクトの基礎にする場合は、「 [Azure App Service で ASP.NET web アプリの作成](web-sites-dotnet-get-started.md)します。 情報をトラブルシューティングするには、次を参照してください。、 [トラブルシューティング](#troubleshooting) セクションです。
>
>Azure アカウントにサインアップする前に Azure App Service の使用開始を取得する場合に移動または [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 前提条件

このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを持っていない場合は、 [、Visual Studio のサブスクライバーの特典をアクティブ化](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) または [無料評価版にサインアップ](/pricing/free-trial/?WT.mc_id=A261C142F)します。

開発環境をセットアップするインストールする必要があります [Visual Studio 2013 Update 5](http://go.microsoft.com/fwlink/?LinkId=390521) (以上) の最新バージョンと、 [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)します。この記事は、Visual Studio Update 4 および SDK 2.8.1 を対象にしています。同じ手順は、最新の Visual Studio 2015 の機能 [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) インストールされているが一部の画面が図と異なる表示します。

## ASP.NET MVC 5 アプリケーションを作成する

### プロジェクトを作成する

1. **[ファイル]** メニューの **[新しいプロジェクト]** をクリックします。

    ![New Project in File menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/gs13newproj.png)

1. **[新しいプロジェクト]** ダイアログ ボックスで、**[インストールされているテンプレート]** の下にある **[Visual C#]** を展開して **[Web]** を選択し、**[ASP.NET Web アプリケーション]** を選択します。

1. アプリケーションに「**ContactManager**」という名前を付けて、**[OK]** をクリックします。

    ![New Project dialog box](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13newprojdb.png)

    **注:** 必ず「ContactManager」と入力してください。 これからコピーするコード ブロックは、ContactManager というプロジェクト名が前提となっています。

1. **[新しい ASP.NET プロジェクト]** ダイアログ ボックスで、**[MVC]** テンプレートを選択します。 **[認証]** に **[個別ユーザー アカウント]** が設定されていること、**[クラウドでのホスト]** がオンになっていること、**[App Service]** が選択されていることを確認します。

    ![New ASP.NET Project dialog box](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newproject.png)

1. **[OK]** をクリックします。

3. ときに、 **Microsoft Azure Web アプリの設定を構成する** ダイアログが表示されたら、Azure にサインインしているかどうかを確認します。 サインインしていないためには、または現在のログインが期限切れの場合に、資格情報を再入力します。

    ![資格情報を再入力する](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/reentercredentials.png)

2. Web アプリに名前を指定する場合は、**[Web アプリ名]** ボックスの値を変更します。

    Web アプリの URL は「{名前}.azurewebsites.net」になります。そのため、名前は azurewebsites.net ドメインで一意にする必要があります。 構成ウィザードからはプロジェクト名の「ContactManager」に数字を付けたものが一意の名前として提案されます。このチュートリアルではそれで問題ありません。

5. **[App Service プラン]** ドロップダウンから **[新しい App Service プランの作成]** を選択し、図に示すように「StandardWeb」などの名前を入力します。

    既存の App Service プランを選択することもできます。 App Service プランについては、次を参照してください。 [Azure App Service プランの詳細な概要](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)します。

5. **[リソース グループ]** ドロップダウンから **[新規リソース グループの作成]** を選択し、図に示すように「ExampleMVC」などの名前を入力します。

    既存のリソース グループを選択することもできます。 ただし、新しいリソース グループを作成し、このチュートリアルでそれだけを使用した場合、完了後、チュートリアルのために作成したすべての Azure リソースを簡単に削除できます。 リソース グループについては、次を参照してください。 [Azure リソース マネージャーの概要](../resource-group-overview.md)します。

7. 近くのリージョンを選択します。

    **[OK]** はまだクリックしないでください。 次の手順では、データベース リソースを構成します。 ダイアログ ボックスは次の図のようになります。

    ![新しいプランとリソース グループ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newplanandgroup.png)

2. **[サーバーの新規作成]** を選択し、サーバー名、ユーザー名、パスワードを入力します。

    サーバー名は一意にする必要があります。 小文字、数字、ハイフンを使用できます。 末尾にハイフンを使用することはできません。 ユーザー名とパスワードは新しいサーバーに作成する新しい資格情報となります。

    既にデータベース サーバーがある場合、新規作成せずにその既存のサーバーを選択できます。 データベース サーバーは貴重なリソースであるため、一般的にはデータベースごとにデータベース サーバーを作成するのではなく、同じサーバー上にテスト用や開発用の複数のデータベースを作成してください。 ただし、このチュートリアルのために、サーバーを一時的に必要とします。Web サイトと同じリソース グループにサーバーを作成すると、チュートリアルの終了時に、リソース グループを削除することで、Web アプリとデータベース リソースの両方を簡単に削除できます。

    既存のデータベース サーバーを選択した場合、Web アプリとデータベースが同じリージョンにあることを確認してください。

    ![新しいデータベースを使用する](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newdb.png)

4. **[OK]** をクリックします。

    Visual Studio は ContactManager Web プロジェクトを作成し、指定したリソース グループと App Service プランを作成し、指定した名前で Web アプリを Azure App Service に作成します。

### ページのヘッダーとフッターを設定する

1. **ソリューション エクスプローラー**の *Views\Shared* フォルダーにある *Layout.cshtml* ファイルを開きます。

    ![_Layout.cshtml in Solution Explorer][newapp004]

1. *Layout.cshtml* ファイルの内容を次のコードに置き換えます。

     <!DOCTYPE html>
     <html>
     <head>
         <meta charset="utf-8" />
         <meta name="viewport" content="width=device-width, initial-scale=1.0">
         <title>@ViewBag.Title - Contact Manager</title>
         @Styles.Render("~/Content/css")
         @Scripts.Render("~/bundles/modernizr")
    
     </head>
     <body>
         <div class="navbar navbar-inverse navbar-fixed-top">
             <div class="container">
                 <div class="navbar-header">
                     <button type="button" class="navbar-toggle" data-toggle="collapse" data-target=".navbar-collapse">
                         <span class="icon-bar"></span>
                         <span class="icon-bar"></span>
                         <span class="icon-bar"></span>
                     </button>
                     @Html.ActionLink("CM Demo", "Index", "Cm", new { area = "" }, new { @class = "navbar-brand" })
                 </div>
                 <div class="navbar-collapse collapse">
                     <ul class="nav navbar-nav">
                         <li>@Html.ActionLink("Home", "Index", "Home")</li>
                         <li>@Html.ActionLink("About", "About", "Home")</li>
                         <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                     </ul>
                     @Html.Partial("_LoginPartial")
                 </div>
             </div>
         </div>
         <div class="container body-content">
             @RenderBody()
             <hr />
             <footer>
                 <p>&copy; @DateTime.Now.Year - Contact Manager</p>
             </footer>
         </div>
    
         @Scripts.Render("~/bundles/jquery")
         @Scripts.Render("~/bundles/bootstrap")
         @RenderSection("scripts", required: false)
     </body>
     </html>

 このコードにより、ヘッダーとフッターのアプリケーション名が「My ASP.NET Application」と「Application name」から「Contact Manager」と「CM Demo」に変更されます。

### ローカルでアプリケーションを実行する

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

    アプリケーションのホーム ページが既定のブラウザーに表示されます。

    ![ローカルで実行されている Web アプリ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr2.png)

これで、Azure にデプロイするアプリケーションを作成するために必要な操作が完了しました。

## Azure にアプリケーションをデプロイする

1. Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、コンテキスト メニューの **[発行]** をクリックします。

    ![プロジェクトのコンテキスト メニューの ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)

    **Web の発行**ウィザードが開きます。

1. **[Web の発行]** ダイアログ ボックスの **[発行]** をクリックします。

    ![Publish](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr3.png)

    これで、作成したアプリケーションはクラウドで実行されています。 このアプリケーションを次にデプロイするときは、変更したファイル (または新しいファイル) のみがデプロイされます。

    ![クラウドで実行中](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss4.PNG)

## プロジェクトに対して SSL を有効にする

1. **ソリューション エクスプローラー**で **ContactManager** プロジェクトをクリックし、F4 キーをクリックして **[プロパティ]** ウィンドウを開きます。

3. **[SSL 有効]** を **[True]** に変更します。

4. **[SSL URL]** をコピーします。

    過去に SSL Web アプリを作成したことがなければ、SSL URL は https://localhost:44300/ になります。

    ![SSL を有効にする][rxssl]

1. **ソリューション エクスプローラー**で **Contact Manager** プロジェクトを右クリックし、**[プロパティ]** をクリックします。

1. **[Web]** タブをクリックします。

1. **SSL URL** を使用するように **[プロジェクト URL]** を変更し、ページを保存します (Ctrl + S)。

    ![SSL を有効にする](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr1.png)

1. 次の図に示すように、Internet Explorer が Visual Studio によって起動されるブラウザーであることを確認します。

    ![既定のブラウザー](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss12.PNG)

    ブラウザー セレクターによって、Visual Studio が起動するブラウザーを指定することができます。 変更を加える場合、複数のブラウザーを選択して、Visual Studio が各ブラウザーを更新するようにできます。 詳細については、次を参照してください。 [Visual Studio 2013 で Browser Link を使用して](http://www.asp.net/visual-studio/overview/2013/using-browser-link)します。

    ![ブラウザー セレクター](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss13.png)

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 **[はい]** をクリックし、IIS Express が生成した自己署名証明書を信頼するプロセスを開始します。

     ![IIS Express が生成した自己署名証明書を信頼するよう促す指示](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss26.PNG)

1. 読み取り、 **セキュリティの警告** クリックしてダイアログ **はい** を表す証明書をインストールする場合は、  **localhost**します。

    ![localhost の IIS Express の証明書に関する警告](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss27.PNG)

1. IE は*ホーム* ページを表示し、SSL の警告はありません。

     ![警告が表示されていない、localhost の SSL を有効にした IE](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss28.PNG)

     SSL を利用する場合、Internet Explorer は最適な選択肢です。証明書を承認し、警告なしで HTTPS コンテンツを表示するからです。 Microsoft Edge と Google Chrome も証明書を承認します。 Firefox は独自の証明書ストアを使用します。したがって、警告を表示します。

     ![FireFox の証明書に関する警告](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss30.PNG)

## アプリケーションにデータベースを追加する

次に、アプリケーションを更新して、連絡先を表示および更新してデータをデータベースに保存する機能を追加します。 アプリケーションでは、データベースの作成およびデータの読み取りと更新に Entity Framework を使用します。

### 連絡先のデータ モデル クラスを追加する

まず、コードで単純なデータ モデルを作成します。

1. **ソリューション エクスプローラー**で、Models フォルダーを右クリックし、**[追加]**、**[クラス]** の順にクリックします。

    ![Add Class in Models folder context menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr5.png)

2. **[新しい項目の追加]** ダイアログ ボックスで、新しいクラス ファイルに「*Contact.cs*」という名前を付け、[**追加**] をクリックします。

    ![[新しい項目の追加] ダイアログ ボックス][adddb002]

3. Contact.cs ファイルの内容を次のコードに置き換えます。

        using System.ComponentModel.DataAnnotations;
        using System.Globalization;
        namespace ContactManager.Models
        {
            public class Contact
            {
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

**Contact** クラスでは、各連絡先について保存するデータと、データベースが必要とする主キー (*ContactID*) を定義します。

### アプリケーション ユーザーが連絡先を操作できる Web ページを作成する

ASP.NET MVC のスキャフォールディング機能によって、作成、読み取り、更新、削除 (CRUD 操作) を実行するコードを自動的に生成できます。

1. プロジェクトをビルドします **(Ctrl + Shift + B)**。 (スキャフォールディング機能の使用前にプロジェクトをビルドする必要があります。)

1. **ソリューション エクスプローラー**で、Controllers フォルダーを右クリックし、**[追加]**、**[コントローラー]** の順にクリックします。

    ![Add Controller in Controllers folder context menu][addcode001]

5. **[スキャフォールディングの追加]** ダイアログ ボックスで、**[MVC 5 コントローラーとビュー、EF を使用]** を選択し、**[追加]** をクリックします。

    ![Add Scaffold dlg](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr6.png)

1. **[モデル クラス]** ボックスで **Contact (ContactManager.Models)** を選択します。 (下図を参照)。

1. **[データ コンテキスト クラス]** で **ApplicationDbContext (ContactManager.Models)** を選択します。 この **ApplicationDbContext** が、メンバーシップ DB と連絡先データの両方に使用されます。

1. **[コントローラー名]** テキスト入力ボックスで、コントローラー名として「CmController」と入力します。

    ![New data ctx dlg](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss5.PNG)

1. **[追加]** をクリックします。

   Visual Studio によって、**Contact** オブジェクトの CRUD データベース操作に対応したメソッドとビューを含むコントローラーが作成されます。

## Migrations の有効化、データベースの作成、サンプル データとデータ初期化子の追加

次のタスクが有効にするのには、 [Code First Migrations](http://msdn.microsoft.com/library/hh770484.aspx) 作成したデータ モデルに基づいてデータベース テーブルを作成するために機能します。

1. **[ツール]** メニューの **[NuGet パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** を順に選択します。

    ![Package Manager Console in Tools menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/SS6.png)

2. **[パッケージ マネージャー コンソール]** ウィンドウで、次のコマンドを入力します。

        enable-migrations

    **enable-migrations** コマンドによって *Migrations* フォルダーが作成され、そのフォルダーに *Configuration.cs* ファイルが保存されます。このファイルを編集して、データベースの初期データ投入を行い、Migration を構成できます。

2. **[パッケージ マネージャー コンソール]** ウィンドウで、次のコマンドを入力します。

        add-migration Initial

    **、Add-migration Initial** という名前のファイルを生成するコマンド **< date_stamp > Initial** で、 *移行* フォルダーです。 このファイルのコードにより、データベース テーブルが作成されます。 最初のパラメーター (**Initial**) は、このファイルの名前の作成に使用されます。 新しいクラス ファイルは**ソリューション エクスプローラー**で表示できます。

    **Initial** クラスでは、**Up** メソッドを使用して Contacts テーブルを作成し、**Down** メソッドを使用してそのテーブルを削除します (前の状態に戻します)。

3. *Migrations\Configuration.cs* ファイルを開きます。

4. 次の追加 `を使用して` ステートメントです。

         using ContactManager.Models;

5. *Seed* メソッドを次のコードに置き換えます。

        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            context.Contacts.AddOrUpdate(p => p.Name,
               new Contact
               {
                   Name = "Debra Garcia",
                   Address = "1234 Main St",
                   City = "Redmond",
                   State = "WA",
                   Zip = "10999",
                   Email = "debra@example.com",
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                }
                );
        }

    このコードでは、連絡先情報を使用してデータベースを初期化 (初期データ投入) します。 シード データベースの生成の詳細については、次を参照してください。 [シード処理、およびデバッグ Entity Framework (EF) Db](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)します。

6. **[パッケージ マネージャー コンソール]** で、次のコマンドを入力します。

        update-database

    ![Package Manager Console commands][addcode009]

    **update-database** によって、データベースを作成する最初の移行が実行されます。 既定では、データベースは SQL Server Express LocalDB データベースとして作成されます

7. クリックしてアプリケーションを実行するには、CTRL + F5 キーを押して、 **CM Demo** リンクです。 または、https://localhost:(port#)/Cm に移動します。

    アプリケーションでは、登録されたデータが表示され、編集、詳細、削除のリンクが示されます。 データの作成、編集、削除、表示を行うことができます。

    ![MVC view of data][rx2]

## OAuth2 プロバイダーを追加する

>[AZURE.NOTE] Google および Facebook の開発者ポータル サイトの使用方法の詳細な手順については、このチュートリアルから ASP.NET サイト上のチュートリアルへのリンクを参照してください。 ただし、Google と Facebook のサイトの更新頻度はこれらのチュートリアルよりも高いため、チュートリアルは最新の状態でない場合もあります。 手順に従っていて問題が発生した場合は、このチュートリアルの最後にある Disqus のお勧めのコメントで、変更内容の一覧を確認してください。 

[OAuth](http://oauth.net/ "http://oauth.net/") は、web、モバイル、およびデスクトップ アプリケーションからシンプルで標準の方法で安全に認証を使用するオープン プロトコルです。 ASP.NET MVC インターネット テンプレートは OAuth を使用して、Facebook、Twitter、Google、Microsoft を認証プロバイダーとしてサポートします。 このチュートリアルでは Google のみを認証プロバイダーとして使用しますが、コードを少し変更すれば他のプロバイダーも使用できます。 他のプロバイダーを実装する手順は、このチュートリアルで説明する手順とほとんど同じです。 認証プロバイダーとして Facebook を使用して、次を参照してください。 [MVC 5 アプリケーションの Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン ](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on)します。

このチュートリアルでは、認証の他にロールを使用して権限を付与します。 *canEdit* ロールに追加したユーザーのみがデータを変更 (連絡先を作成、編集、削除) できます。

1. 指示に従って、 [MVC 5 アプリケーションの Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン ](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on#goog)  [ **OAuth2 用の Google アプリを設定する OAuth 2 用の Google アプリを作成する**します。

3. アプリケーションを実行してテストし、Google 認証を使用してログオンできることを確認します。

2. プロバイダー固有のアイコンとソーシャル ログイン ボタンを作成するには、を参照して [ASP.NET MVC 5 用ソーシャル ログイン ボタンをなかなか](http://www.jerriepelser.com/blog/pretty-social-login-buttons-for-asp-net-mvc-5)

## メンバーシップ API を使用する

このセクションでは、ローカル ユーザーと *canEdit* ロールをメンバーシップ データベースに追加します。 *canEdit* ロールのユーザーのみがデータを編集することができます。 実行可能な操作に基づいてロール名を付けるのが望ましいので、この場合は *admin* より *canEdit* が適しています。 その後、必要に応じて、(わかりにくい *superAdmin* ではなく) *canDeleteMembers* などの新しいロールを追加します。

1. 開いている、 *migrations \configuration.cs* ファイルし、次の追加 `を使用して` ステートメント。

        using Microsoft.AspNet.Identity;
        using Microsoft.AspNet.Identity.EntityFramework;

1. 次の **AddUserAndRole** メソッドをクラスに追加します。

        bool AddUserAndRole(ContactManager.Models.ApplicationDbContext context)
        {
            IdentityResult ir;
            var rm = new RoleManager<IdentityRole>
                (new RoleStore<IdentityRole>(context));
            ir = rm.Create(new IdentityRole("canEdit"));
            var um = new UserManager<ApplicationUser>(
                new UserStore<ApplicationUser>(context));
            var user = new ApplicationUser()
            {
                UserName = "user1@contoso.com",
            };
            ir = um.Create(user, "P_assw0rd1");
            if (ir.Succeeded == false)
                return ir.Succeeded;
            ir = um.AddToRole(user.Id, "canEdit");
            return ir.Succeeded;
        }

1. 新しいメソッドを **Seed** メソッドから呼び出します。

        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            AddUserAndRole(context);
            context.Contacts.AddOrUpdate(p => p.Name,
                // Code removed for brevity
        }

    次の画像は、*Seed* メソッドに対する変更を示しています。

    ![code image](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss24.PNG)

    このコードと呼ばれる新しいロールを作成する *canEdit*, 、新しいローカル ユーザー *user1@contoso.com*, 、し、追加 *user1@contoso.com* に、 *canEdit* ロールです。 詳細については、次を参照してください。、 [ASP.NET Identity チュートリアル](http://www.asp.net/identity/overview/features-api) ASP.NET サイトです。

## 一時的なコードを使用して新しいソーシャル ログイン ユーザーを canEdit ロールに追加する

このセクションでは、Account コントローラーの **ExternalLoginConfirmation** メソッドを一時的に変更して、*canEdit* ロールに新しいユーザーを追加し、OAuth プロバイダーに登録します。 同様のツールを提供するお役に立てば [WSAT](http://msdn.microsoft.com/library/ms228053.aspx) 今後できるようにするを作成し、ユーザー アカウントとロールを編集します。 そのときまで、一時的なコードを利用し、同じ機能を実現できます。

1. **Controllers\AccountController.cs** ファイルを開いて、**ExternalLoginConfirmation** メソッドに移動します。

1. 次の **AddToRoleAsync** への呼び出しを **SignInAsync** の呼び出しの直前に追加します。

        await UserManager.AddToRoleAsync(user.Id, "canEdit");

   このコードは、新しく登録したユーザーを "canEdit" ロールに追加します。これにより、データの変更 (編集) を伴うアクション メソッドへのアクセス権がユーザーに割り当てられます。 次のスニペットでは、コンテキストに新しい行のコードを示します。

          // POST: /Account/ExternalLoginConfirmation
          [HttpPost]
          [AllowAnonymous]
          [ValidateAntiForgeryToken]
          public async Task ExternalLoginConfirmation(ExternalLoginConfirmationViewModel model, string returnUrl)
          {
             if (User.Identity.IsAuthenticated)
             {
                return RedirectToAction("Index", "Manage");
             }
             if (ModelState.IsValid)
             {
                // Get the information about the user from the external login provider
                var info = await AuthenticationManager.GetExternalLoginInfoAsync();
                if (info == null)
                {
                   return View("ExternalLoginFailure");
                }
                var user = new ApplicationUser { UserName = model.Email, Email = model.Email };
                var result = await UserManager.CreateAsync(user);
                if (result.Succeeded)
                {
                   result = await UserManager.AddLoginAsync(user.Id, info.Login);
                   if (result.Succeeded)
                   {
                      await UserManager.AddToRoleAsync(user.Id, "canEdit");
                      await SignInManager.SignInAsync(user, isPersistent: false, rememberBrowser: false);
                      return RedirectToLocal(returnUrl);
                   }
                }
                AddErrors(result);
             }
             ViewBag.ReturnUrl = returnUrl;
             return View(model);
          }


このチュートリアルの中で、アプリケーションを Azure にデプロイします。そこでのログオンには、Google などサード パーティの認証プロバイダーが使用されます。 新しく登録されたアカウントは、*canEdit* ロールに追加されます。 Web アプリの URL と Google ID さえあればだれでも登録し、データベースを更新することができます。 そのような操作が第三者によって行われるのを防ぐためには、サイトを停止する必要があります。 だれが *canEdit* ロールに追加されているかは、データベースを調べることによって確認できます。

**[パッケージ マネージャー コンソール]** で上方向キーを押し、次のコマンドを呼び出します。

        Update-Database

**Update-Database** コマンドは **Seed** メソッドを実行し、それが先に追加した **AddUserAndRole** メソッドを実行します。  **AddUserAndRole** メソッドは、ユーザーを作成 *user1@contoso.com* にユーザーを追加し、 *canEdit* ロールです。

## SSL と Authorize 属性を使用してアプリケーションを保護する

このセクションで適用する、 [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) アクション メソッドへのアクセスを制限する属性です。 匿名ユーザーが表示できるのは、home コントローラーの **Index** アクション メソッドだけになります。 登録ユーザーは、連絡先データ (Cm コントローラーの **[Index]** ページと **[Details]** ページ)、[About] ページ、[Contact] ページを表示できます。 *canEdit* ロールを与えられているユーザーのみがアクション メソッドを実行してデータを変更できます。

1. *App_Start\FilterConfig.cs* ファイルを開き、*RegisterGlobalFilters* メソッドを次の内容に置き換えます (2 つのフィルターを追加します)。

        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
            filters.Add(new System.Web.Mvc.AuthorizeAttribute());
            filters.Add(new RequireHttpsAttribute());
        }

    このコードを追加、 [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) フィルターおよび [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) アプリケーションにフィルターします。  [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) フィルターにより、匿名ユーザーがアプリケーションのメソッドに一切アクセスします。 使用する、 [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) を匿名ユーザーがログインできる、ホーム ページを表示できるように、2 つのメソッドの承認要件を免除し属性です。   [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) HTTPS を介して web アプリにアクセスできる必要があります。

    追加する方法の 1 つは、 [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) 属性および [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) 属性を各コント ローラーもありますアプリケーション全体に適用するセキュリティのベスト プラクティスです。 アプリケーション全体に適用すれば、新しいコントローラーやアクション メソッドを追加したとき、それらが自動的に保護されます。ユーザー自身で適用する必要がありません。 詳細については、次を参照してください。 [ASP.NET MVC アプリケーションと新しい AllowAnonymous 属性をセキュリティで保護する](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx)します。

1. 追加、 [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) 属性を **インデックス** Home コント ローラーのメソッドです。  [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) 属性を使用ホワイト リストを認証方法です。

        public class HomeController : Controller
        {
          [AllowAnonymous]
          public ActionResult Index()
          {
             return View();
          }

    グローバルに検索すると、Account コントローラーのログイン メソッドや登録メソッドで *AllowAnonymous* が使われていることがわかります。

1. *CmController.cs*, 、追加 `[Authorize (ロール ="canEdit")]` 、HttpGet メソッドと HttpPost メソッドにデータ (作成、編集、削除、Index と Details を除くすべてのアクション メソッド) を変更する、 *Cm* コント ローラーです。 追加後のコードは次のようになります。

        // GET: Cm/Create
        [Authorize(Roles = "canEdit")]
        public ActionResult Create()
        {
           return View(new Contact { Address = "123 N 456 W",
            City="Great Falls", Email = "ab@cd.com", Name="Joe Smith", State="MT",
           Zip = "59405"});
        }
        // POST: Cm/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
         [Authorize(Roles = "canEdit")]
        public ActionResult Create([Bind(Include = "ContactId,Name,Address,City,State,Zip,Email")] Contact contact)
        {
            if (ModelState.IsValid)
            {
                db.Contacts.Add(contact);
                db.SaveChanges();
                return RedirectToAction("Index");
            }
            return View(contact);
        }
        // GET: Cm/Edit/5
        [Authorize(Roles = "canEdit")]
        public ActionResult Edit(int? id)
        {
            if (id == null)
            {
                return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
            }
            Contact contact = db.Contacts.Find(id);
            if (contact == null)
            {
                return HttpNotFound();
            }
            return View(contact);
        }

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

1. まだ前のセッションにログインしている場合は、**ログアウト** リンクをクリックします。

1. **[About]** または **[Contact]** リンクをクリックします。 匿名ユーザーはこれらのページを表示できないため、ログイン ページにリダイレクトされます。

1. クリックして、 **新しいユーザーとして登録** リンクを電子メールでローカル ユーザーを追加 *joe@contoso.com*します。 *Joe* であれば Home ページ、About ページ、Contact ページを表示できることを確認します。

    ![login](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss14.PNG)

1. *[CM Demo]* リンクをクリックし、データが表示されることを確認します。

1. このページ上の編集リンクをクリックすると、ログイン ページにリダイレクトされます (新しいローカル ユーザーが *canEdit* ロールに追加されていないため)。

1. としてログイン *user1@contoso.com* パスワード"p_assw0rd1"("word"の「0」はゼロ)。 先ほど選択した編集ページにリダイレクトされます。

    このアカウントとパスワードでログインできない場合は、ソース コードからパスワードをコピーして貼り付けてみてください。 ログインできない場合、 **UserName** の列、 **AspNetUsers** テーブルを確認 *user1@contoso.com* が追加されました。

1. データを変更できることを確認します。

## Azure にアプリケーションをデプロイする

1. Visual Studio の**ソリューション エクスプローラー**で、プロジェクトを右クリックし、コンテキスト メニューの **[発行]** をクリックします。

    ![プロジェクトのコンテキスト メニューの ][firsdeploy003]

    **Web の発行**ウィザードが開きます。

1. **[Web の発行]** ダイアログ ボックスの左側の **[設定]** タブをクリックします。

2. **[v]** アイコンをクリックして **ApplicationDbContext** の **[リモート接続文字列]** を選択し、プロジェクトの作成時に作成したデータベースを選択します。

    ![設定](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrc2.png)

1. **ContactManagerContext** で、**[Code First Migrations を実行する]** を選択します。

    ![settings](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrc3.png)

1. **[発行]** をクリックします。

1. としてログイン *user1@contoso.com* (パスワード"p_assw0rd1") のデータを編集できることを確認します。

1. ログアウトします。

1. 移動して、 [Google Developers Console](https://console.developers.google.com/) し、[、 **資格情報** タブ リダイレクト URIS と JavaScript orgins ように変更が Azure の URL を使用します。

1. Google または Facebook を使用してログインします。 これにより、Google または Facebook のアカウントが **canEdit** ロールに追加されます。 メッセージと共に HTTP 400 エラーが表示 *要求のリダイレクト URI: https://contactmanager {my version}.azurewebsites.net/signin-google 登録リダイレクト uri が一致しませんでした*, 、加えた変更が反映されるまで待機する必要があります。 数分経ってからこのエラーが発生する場合は、URI は正しいことを確認します。

### Web アプリを停止して第三者の登録を防ぐ

1. **サーバー エクスプローラー**で、**[Azure]、[App Service]、{リソース グループ}、{Web アプリ}** の順に移動します。

4. Web アプリを右クリックし、**[停止]** を選択します。

    またはから、 [Azure ポータル](https://portal.azure.com/), 、web アプリのブレードに移動し、をクリックできます、 **停止** ブレードの上部にあるアイコン。

    ![web アプリ ポータルの停止](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/stopweb.png)

### AddToRoleAsync の削除、発行、テスト

1. Account コントローラーの **ExternalLoginConfirmation** メソッドから次のコードをコメント アウトまたは削除します。

        await UserManager.AddToRoleAsync(user.Id, "canEdit");

1. プロジェクトをビルドします。これにより、ファイルの変更が保存され、コンパイル エラーがないか確認が行われます。

5. **ソリューション エクスプローラー**で、プロジェクトを右クリックして **[発行]** をクリックします。

       ![Publish in project context menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)

4. **[プレビューの開始]** をクリックします。 更新する必要のあるファイルだけがデプロイされます。

5. Visual Studio またはポータルから Web アプリを開きます。 **Web アプリが停止した状態で発行することはできません。**

    ![start web app](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss15.png)

5. Visual Studio に戻り、**[発行]** をクリックします。

3. Azure アプリケーションが既定のブラウザーで起動します。 ログインして、ログアウトすると、匿名ユーザーとしてホーム ページを表示できます。

4. **[About]** リンクをクリックします。 ログイン ページにリダイレクトされます。

5. ログイン ページの **[登録]** リンクをクリックし、ローカル アカウントを作成します。 読み取り専用ページにはアクセスできるが、データの変更を伴うページにはアクセスできない (*canEdit* ロールによって保護されている) ことをこのローカル アカウントを使用して確認します。 ローカル アカウント アクセスは、後でこのチュートリアルの中で削除します。

    ![登録](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss16.PNG)

1. *[About]* ページと *[Contact]* ページにアクセスできることを確認します。

    ![ログオフ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss17.PNG)

1. **[CM Demo]** リンクをクリックして **Cm** コントローラーに移動します。 URL に *Cm* を付加することによって移動してもかまいません。

    ![CM page](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr4.png)

1. [編集] リンクをクリックします。

    ログイン ページにリダイレクトされます。

2. **[Use another service to log in]** で [Google] または [Facebook] をクリックし、過去に登録したアカウントでログインします (短時間で作業して、セッション Cookie がタイムアウトしていない場合は、以前に使用した Google アカウントまたは Facebook アカウントで自動的にログインされます)。

2. そのアカウントにログインした状態でデータを編集できることを確認します。

    **注:** このアプリケーションで Google からログアウトし、同じブラウザーで別の Google アカウントにログインすることはできません。 1 つのブラウザーを使用している場合は、Google に移動してログアウトする必要があります。 同じサード パーティの認証システム (Google など) に属している別のアカウントでログオンするためには、異なるブラウザーを使用する必要があります。

    Google アカウント情報の氏名を入力しないと、NullReferenceException が発生します。

## SQL Azure DB を調べる

1. **サーバー エクスプローラー**で、**[Azure]、[SQL Databases]、{データベース}** の順に移動します。

2. データベースを右クリックし、**[SQL Server オブジェクト エクスプローラーで開く]** を選択します。

    ![open in SSOX](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr12.png)

3. このデータベースに接続したことがない場合は、現在の IP アドレスへのアクセスを有効にするファイアウォール規則を追加するように求められることがあります。 IP アドレスは、自動的に入力されます。 **[ファイアウォール規則の追加]** をクリックするだけで、アクセスが有効になります。

    ![add firewall rule](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/addfirewallrule.png)

3. データベース サーバーの作成時に指定したユーザー名とパスワードを使用してデータベースにログインします。

1. **AspNetUsers** テーブルを右クリックし、**[データの表示]** を選択します。

    ![CM page](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr8.png)

1. Id であるへの登録した Google アカウントをメモします、 **canEdit** ロール、および Id の *user1@contoso.com*します。 それ以外のユーザーは **canEdit** ロールに含めないようにする必要があります。 この点については、次のステップで確認します。

    ![CM page](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/s2.png)

2. **SQL Server オブジェクト エクスプローラー**で **[AspNetUserRoles]** を右クリックし、**[データの表示]** を選択します。

    ![CM page](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rs1.png)

3. いることを確認、 **UserId** から *user1@contoso.com* 登録した Google アカウントおよびです。

## トラブルシューティング

問題が発生した場合の対処方法について推奨事項を次に示します。

* SQL Database のプロビジョニング エラー - 最新の SDK がインストールされていることを確認してください。 2.8.1 より前のバージョンでは、シナリオによっては、VS でデータベース サーバーまたはデータベースを作成しようとするとエラーが発生するというバグがあります。
* Azure リソースの作成時のエラー メッセージ "お使いのサブスクリプション プランの種類では操作がサポートされていません" - 同上。
* -の展開時のエラーを読み進めるを検討してください、 [基本的な ASP.NET 展開](web-sites-dotnet-get-started.md) 記事です。 そのデプロイのシナリオはより単純であるため、同じ問題が発生した場合に分離が簡単になる可能性があります。 たとえば、一部のエンタープライズ環境では、企業のファイアウォールにより、Web デプロイから必要な Azure に接続できない場合があります。
* Azure リソースを作成する別の方法を使用した場合に展開するときに Web の発行ウィザードで接続文字列を選択するオプションが (たとえば、web アプリをデプロイしようとして、ポータルで SQL データベースを作成、)、SQL データベースが web アプリに関連付けられていない場合がありません。 最も簡単な解決策として、このチュートリアルで示すように、VS を使用して新しい Web アプリとデータベースを作成します。 チュートリアルをもう一度やり直す必要はありません。Web の発行ウィザードで、新しい Web アプリの作成を選択すると、プロジェクトの作成時と同じ Azure リソースの作成ダイアログが表示されます。
* Google または Facebook の開発者ポータルの指示が古い - このチュートリアルの最後にある Disqus のお勧めのコメントを参照してください。

## 次のステップ

ユーザーを認証する基本 ASP.NET MVC Web アプリケーションが作成されました。 一般的な認証タスクと機密データの保持方法に関する詳細については、次のチュートリアルを参照してください。

- [ログイン、電子メールの確認とパスワードのリセットをセキュリティで保護された ASP.NET MVC 5 web アプリケーションを作成します。](http://www.asp.net/mvc/overview/getting-started/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset)
- [SMS と電子メール 2 要素認証を使用 ASP.NET MVC 5 アプリケーション](http://www.asp.net/mvc/overview/getting-started/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication)
- [ASP.NET と Azure へパスワードやその他の機密情報を展開するためのベスト プラクティス](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure)
- [Facebook と Google の OAuth2 を使用した ASP.NET MVC 5 アプリケーションを作成する](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on) ユーザー登録データベースには、認証プロバイダーとして Facebook の使用に関する詳細な手順については、プロファイル データを追加する方法の手順についても説明します。
- [ASP.NET MVC 5 の概要](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)

Entity Framework を使用する方法についてより高度なチュートリアルについては、次を参照してください。 [EF と MVC の概要](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)します。

このチュートリアルが @blowdart [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) と Tom Dykstra と Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart))します。

*** ままにしてください。 フィードバック *** 立った内容やなどして、強化されたで紹介した製品に関しても自体、チュートリアルに関してだけでなく、します。 お寄せいただいたご意見は、今後の改善に役立たせていただきます。 要求アンケートに新しいトピックに投票したり [Me 方法とコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)




[add an oauth provider]: #addOauth 
[using the membership api]: #mbrDB 
[create a data deployment script]: #ppd 
[update the membership database]: #ppd2 
[setupwindowsazureenv]: #bkmk_setupwindowsazure 
[createapplication]: #bkmk_createmvc4app 
[deployapp1]: #bkmk_deploytowindowsazure1 
[deployapp11]: #bkmk_deploytowindowsazure11 
[adddb]: #bkmk_addadatabase 
[rx2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rx2.png 
[rx5]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx5.png 
[rx6]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx6.png 
[rx7]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx7.png 
[rx8]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx8.png 
[rx9]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx9.png 
[rxb]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxb.png 
[rxssl]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxSSL.png 
[rxnot]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png 
[rxnot2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT2.png 
[rxnot]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png 
[rxnot]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png 
[rxnot]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png 
[rr1]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rr1.png 
[rxprevdb]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxPrevDB.png 
[rxwsnew]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxWSnew2.png 
[rxcreatewswithdb]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxCreateWSwithDB.png 
[setup007]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-setup-azure-site-004.png 
[newapp004]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-createapp-004.png 
[firsdeploy003]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-deploy1-publish-001.png 
[adddb002]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-adddatabase-002.png 
[addcode001]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-controller-add-context-menu.png 
[addcode008]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-migrations-package-manager-menu.png 
[addcode009]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-migrations-package-manager-console.png 
[important information about asp.net in azure web apps]: #aspnetwindowsazureinfo 
[next steps]: #nextsteps 
[importpublishsettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/ImportPublishSettings.png 

