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
* アプリケーションによって管理されているデータベースに登録されているユーザー認証および承認する方法 (ローカル認証を使用して [ASP.NET Identity](http://asp.net/identity/))。
* ADO.NET Entity Framework 6 の Code First を使用して、SQL データベースのデータを読み書きする方法
* Entity Framework の Code First Migrations を使用してデータベースをデプロイする方法
* Azure SQL Database を使用してリレーショナル データをクラウドに保存する方法
* データベースを使用する web プロジェクトをデプロイする方法、 [web アプリ](http://go.microsoft.com/fwlink/?LinkId=529714) Azure App Service でします。

>[AZURE.NOTE] これは、長いチュートリアルです。 Azure App Service と Visual Studio の web プロジェクトの基礎にする場合は、「 [Azure App Service で ASP.NET web アプリの作成](web-sites-dotnet-get-started.md)します。 情報をトラブルシューティングするには、次を参照してください。、 [トラブルシューティング](#troubleshooting) セクションです。
>
>Azure アカウントにサインアップする前に Azure App Service の使用開始を取得する場合に移動または [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 前提条件

このチュートリアルを完了するには、Microsoft Azure アカウントが必要です。 アカウントを持っていない場合は、[Visual Studio サブスクライバーの特典を有効にする](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F)か、[無料試用版にサインアップ](/pricing/free-trial/?WT.mc_id=A261C142F)してください。

開発環境をセットアップするインストールする必要があります [Visual Studio 2013 Update 5](http://go.microsoft.com/fwlink/?LinkId=390521) (以上) の最新バージョンと、 [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=324322&clcid=0x409)します。 この記事は、Visual Studio Update 4 および SDK 2.8.1 を対象にしています。 同じ手順は、最新の Visual Studio 2015 の機能 [Azure SDK for .NET](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409) インストールされているが一部の画面が図と異なる表示します。

## ASP.NET MVC 5 アプリケーションを作成する

### プロジェクトを作成する

1.  **ファイル** ] メニューをクリックして **新しいプロジェクト**します。

    ![[ファイル] メニューの [新しいプロジェクト]](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/gs13newproj.png)

1.  **新しいプロジェクト** ] ダイアログ ボックスで、展開 **c#** を選択して **Web** [ **インストールされたテンプレート**, 、し、[ **ASP.NET Web アプリケーション**します。

1. アプリケーションの名前 **ContactManager**, 、] をクリックし、 **OK**します。

    ![New Project dialog box](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13newprojdb.png)
 
    **注:** 「ContactManager」と入力してください。 これからコピーするコード ブロックは、ContactManager というプロジェクト名が前提となっています。 

1.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、 **MVC** テンプレートです。 確認 **認証** に設定されている **個々 のユーザー アカウント**, 、**クラウドでホスト** がオンになってと **App Service** が選択されています。

    ![[新しい ASP.NET プロジェクト] ダイアログ ボックス](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newproject.png)

1. Click **OK**.

3. ときに、 **Microsoft Azure Web アプリの設定を構成する** ダイアログが表示されたら、Azure にサインインしているかどうかを確認します。 サインインしていないためには、または現在のログインが期限切れの場合に、資格情報を再入力します。

    ![資格情報を再入力する](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/reentercredentials.png)

2. Web アプリの名前を指定する場合に値を変更、 **Web アプリ名** ボックス。

    Web アプリの URL は「{名前}.azurewebsites.net」になります。そのため、名前は azurewebsites.net ドメインで一意にする必要があります。 構成ウィザードからはプロジェクト名の「ContactManager」に数字を付けたものが一意の名前として提案されます。このチュートリアルではそれで問題ありません。

5.  **App Service プラン** ドロップダウンの選択 **App Service プランの新規作成** と、図のように、"StandardWeb"などの名前を入力します。

    既存の App Service プランを選択することもできます。 App Service プランについては、次を参照してください。 [Azure App Service プランの詳細な概要](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)します。 

5.  **リソース グループ** ドロップダウンの選択 **新しいリソース グループの作成** と、図のように、"ExampleMVC"などの名前を入力します。

    既存のリソース グループを選択することもできます。 ただし、新しいリソース グループを作成し、このチュートリアルでそれだけを使用した場合、完了後、チュートリアルのために作成したすべての Azure リソースを簡単に削除できます。 リソース グループについては、次を参照してください。 [Azure リソース マネージャーの概要](../resource-group-overview.md)します。 

7. 近くのリージョンを選択します。 

    クリックしてしないで **OK** まだです。 次の手順では、データベース リソースを構成します。 ダイアログ ボックスは次の図のようになります。 

    ![新しいプランとリソース グループ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newplanandgroup.png)
 
2. 選択 **新しいサーバーの作成**, 、サーバー名、ユーザー名とパスワードを入力します。 

    サーバー名は一意にする必要があります。 小文字、数字、ハイフンを使用できます。 末尾にハイフンを使用することはできません。 ユーザー名とパスワードは新しいサーバーに作成する新しい資格情報となります。 

    既にデータベース サーバーがある場合、新規作成せずにその既存のサーバーを選択できます。 データベース サーバーは貴重なリソースであるため、一般的にはデータベースごとにデータベース サーバーを作成するのではなく、同じサーバー上にテスト用や開発用の複数のデータベースを作成してください。 ただし、このチュートリアルのために、サーバーを一時的に必要とします。Web サイトと同じリソース グループにサーバーを作成すると、チュートリアルの終了時に、リソース グループを削除することで、Web アプリとデータベース リソースの両方を簡単に削除できます。 

    既存のデータベース サーバーを選択した場合、Web アプリとデータベースが同じリージョンにあることを確認してください。

    ![新しいデータベースを使用する](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/newdb.png)

4. Click **OK**.

    Visual Studio は ContactManager Web プロジェクトを作成し、指定したリソース グループと App Service プランを作成し、指定した名前で Web アプリを Azure App Service に作成します。

### ページのヘッダーとフッターを設定する

1.  **ソリューション エクスプ ローラー** を開く、 *Layout.cshtml* ファイルで、 *views \shared* フォルダーです。

    ![_Layout.cshtml in Solution Explorer][newapp004]

1. 内容を置き換える、 *Layout.cshtml* を次のコード ファイル。

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

1. Visual Studio でプロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行** 、コンテキスト メニューからです。

    ![プロジェクトのコンテキスト メニューの [発行]](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)
    
     **Web の発行** ウィザードが開きます。

1.  **Web の発行** ] ダイアログ ボックスをクリックして **発行**します。

    ![Publish](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr3.png)

    これで、作成したアプリケーションはクラウドで実行されています。 このアプリケーションを次に展開するときは、変更したファイル (または新しいファイル) のみが展開されます。

    ![クラウドで実行中](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss4.PNG)

## プロジェクトに対して SSL を有効にする ##

1.  **ソリューション エクスプ ローラー**, をクリックして、 **ContactManager** プロジェクトをし、f4 キーを開く] をクリックして、 **プロパティ** ウィンドウです。

3. 変更 **SSL を有効に** に **True**します。 

4. コピー、 **SSL URL**します。

    過去に SSL Web アプリを作成したことがなければ、SSL URL は https://localhost:44300/ になります。

    ![SSL を有効にする][rxSSL]
 
1.  **ソリューション エクスプ ローラー**, を右クリックして、 **Contact Manager** プロジェクトを **プロパティ**します。

1. クリックして、 **Web** ] タブをクリックします。

1. 変更、 **プロジェクト Url** を使用する、 **SSL URL** ページ (コントロール S) を保存します。

    ![SSL を有効にする](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr1.png)
 
1. 次の図に示すように、Internet Explorer が Visual Studio によって起動されるブラウザーであることを確認します。

    ![既定のブラウザー](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss12.PNG)

    ブラウザー セレクターによって、Visual Studio が起動するブラウザーを指定することができます。 変更を加える場合、複数のブラウザーを選択して、Visual Studio が各ブラウザーを更新するようにできます。 詳細については、次を参照してください。 [Visual Studio 2013 で Browser Link を使用して](http://www.asp.net/visual-studio/overview/2013/using-browser-link)します。

    ![ブラウザー セレクター](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss13.png)

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 をクリックして **はい** IIS Express は、自己署名証明書を信頼する側のプロセスを開始するが生成されます。

     ![IIS Express が生成した自己署名証明書を信頼するよう促す指示](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss26.PNG)

1. 読み取り、 **セキュリティの警告** クリックしてダイアログ **はい** を表す証明書をインストールする場合は、  **localhost**します。

    ![localhost の IIS Express の証明書に関する警告 ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss27.PNG)

1. Ie、 *ホーム* ページし、SSL の警告はありません。

     ![警告が表示されていない、localhost の SSL を有効にした IE](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss28.PNG)

     SSL を利用する場合、Internet Explorer は最適な選択肢です。証明書を承認し、警告なしで HTTPS コンテンツを表示するからです。 Microsoft Edge と Google Chrome も証明書を承認します。 Firefox は独自の証明書ストアを使用します。したがって、警告を表示します。

     ![FireFox の証明書に関する警告](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss30.PNG)

## アプリケーションにデータベースを追加する

次に、アプリケーションを更新して、連絡先を表示および更新してデータをデータベースに保存する機能を追加します。 アプリケーションでは、データベースの作成およびデータの読み取りと更新に Entity Framework を使用します。

### 連絡先のデータ モデル クラスを追加する

まず、コードで単純なデータ モデルを作成します。

1.  **ソリューション エクスプ ローラー**, を Models フォルダーを右クリックして、 **追加**, 、し **クラス**します。

    ![Add Class in Models folder context menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr5.png)

2.  **新しい項目の追加** ダイアログ ボックスで、新しいクラス ファイルの名前 *Contact.cs*, 、クリックして **追加**します。

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
 **にお問い合わせください** クラスについては、それぞれの連絡先と主キーでは、保存するデータ定義 *ContactID*, 、つまり、データベースが必要とします。

### アプリケーション ユーザーが連絡先を操作できる Web ページを作成する

ASP.NET MVC のスキャフォールディング機能によって、作成、読み取り、更新、削除 (CRUD 操作) を実行するコードを自動的に生成できます。 

1. プロジェクトをビルド **(Ctrl + Shift + B)**します。 (スキャフォールディング機能の使用前にプロジェクトをビルドする必要があります。)
 
1.  **ソリューション エクスプ ローラー**, Controllers フォルダーを右クリックし、クリックして、 **追加**, 、] をクリックし、 **コント ローラー**します。

    ![Add Controller in Controllers folder context menu][addcode001]

5.  **スキャフォールディングの追加** ダイアログ ボックスで、 **MVC 5 コント ローラーとビュー、EF を使用して** ] をクリックし、 **追加**します。
    
    ![Add Scaffold dlg](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rr6.png)

1.  **モデル クラス** ドロップダウン ボックスで、 **Contact (ContactManager.Models)**します。 (下図を参照)。

1.  **データ コンテキスト クラス**, [ **ApplicationDbContext (ContactManager.Models)**します。  **ApplicationDbContext** 、メンバーシップ DB と連絡先データの両方に使用されます。

1.  **コント ローラー名** テキスト入力ボックスに、コント ローラー名として「cmcontroller「と」を入力します。 

    ![New data ctx dlg](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss5.PNG)

1. クリックして **追加**します。

   Visual Studio では、コント ローラーをによりメソッドとビューの CRUD データベース操作と **連絡先** オブジェクトです。

## Migrations の有効化、データベースの作成、サンプル データとデータ初期化子の追加 ##

次のタスクが有効にするのには、 [Code First Migrations](http://msdn.microsoft.com/library/hh770484.aspx) 作成したデータ モデルに基づいてデータベース テーブルを作成するために機能します。

1.  **ツール** メニューの [ **NuGet パッケージ マネージャー** し **パッケージ マネージャー コンソール**します。

    ![Package Manager Console in Tools menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/SS6.png)

2.  **パッケージ マネージャー コンソール** ウィンドウで、次のコマンドを入力します。

        enable-migrations

     **、Enable-migrations** コマンドを作成、 *移行* フォルダー、および、そのフォルダーに配置、 *Configuration.cs* ファイルを編集データベースをシードし、移行を構成することができます。 

2.  **パッケージ マネージャー コンソール** ウィンドウで、次のコマンドを入力します。

        add-migration Initial


     **、Add-migration Initial** という名前のファイルを生成するコマンド **& lt; date_stamp & gt; 初期** で、 *移行* フォルダーです。 このファイルのコードにより、データベース テーブルが作成されます。 最初のパラメーター ( **初期** ) ファイルの名前を作成するために使用します。 新しいクラス ファイルを確認できます **ソリューション エクスプ ローラー**します。

     **初期** クラス、 **を** メソッドが Contacts テーブルを作成し、 **ダウン** (前の状態に戻したい場合に使用) メソッドを使用します。

3. 開いている、 *migrations \configuration.cs* ファイルです。 

4. 次の `using` ステートメントを追加します。 

         using ContactManager.Models;

5. 置き換える、 *シード* メソッドを次のコード。

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

6.  **パッケージ マネージャー コンソール** コマンドを入力します。

        update-database

    ![パッケージ マネージャー コンソールのコマンド][addcode009]

     **データベースの更新** 、データベースを作成する最初の移行を実行します。 既定では、データベースは SQL Server Express LocalDB データベースとして作成されます 

7. クリックしてアプリケーションを実行するには、CTRL + F5 キーを押して、 **CM Demo** リンクです。 または、https://localhost:(port#)/Cm に移動します。 

    アプリケーションでは、登録されたデータが表示され、編集、詳細、削除のリンクが示されます。 データの作成、編集、削除、表示を行うことができます。

    ![データの MVC view][rx2]

## OAuth2 プロバイダーを追加する

>[AZURE.NOTE] ASP.NET サイト上のチュートリアルへチュートリアルのこのリンク Google および Facebook 開発者ポータルをサイトを使用する方法の詳細な手順についてです。 ただし、Google と Facebook のサイトの更新頻度はこれらのチュートリアルよりも高いため、チュートリアルは最新の状態でない場合もあります。 手順に従っていて問題が発生した場合は、このチュートリアルの最後にある Disqus のお勧めのコメントで、変更内容の一覧を確認してください。 

[OAuth](http://oauth.net/ "http://oauth.net/") は、web、モバイル、およびデスクトップ アプリケーションからシンプルで標準の方法で安全に認証を使用するオープン プロトコルです。 ASP.NET MVC インターネット テンプレートは OAuth を使用して、Facebook、Twitter、Google、Microsoft を認証プロバイダーとしてサポートします。 このチュートリアルでは Google のみを認証プロバイダーとして使用しますが、コードを少し変更すれば他のプロバイダーも使用できます。 他のプロバイダーを実装する手順は、このチュートリアルで説明する手順とほとんど同じです。 認証プロバイダーとして Facebook を使用して、次を参照してください。 [MVC 5 アプリケーションの Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン ](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on)します。

このチュートリアルでは、認証の他にロールを使用して権限を付与します。 追加するユーザーのみ、 *canEdit* ロールにデータを変更することは (作成、編集、または連絡先を削除) します。

1. 指示に従って、 [MVC 5 アプリケーションの Facebook、Twitter、LinkedIn、Google の OAuth2 サインオン ](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on#goog)  [ **OAuth2 用の Google アプリを設定する OAuth 2 用の Google アプリを作成する**です。

3. アプリケーションを実行してテストし、Google 認証を使用してログオンできることを確認します。

2. プロバイダー固有のアイコンとソーシャル ログイン ボタンを作成する場合は、「 [非常に ASP.NET MVC 5 用ソーシャル ログイン ボタン](http://www.jerriepelser.com/blog/pretty-social-login-buttons-for-asp-net-mvc-5)

## メンバーシップ API を使用する

このセクションでは、ローカル ユーザーを追加して、 *canEdit* ロールをメンバーシップ データベースにします。 ユーザーのみ、 *canEdit* ロールがデータを編集できます。 ベスト プラクティスは、実行可能な操作に基づいてロール名を *canEdit* はという名前のロールよりも好ましい *admin*します。 アプリケーションの発展、ときにロールを追加新しいなど *canDeleteMembers* ではなくわかりにくく *superAdmin*します。

1. 開いている、 *migrations \configuration.cs* ファイルし、次の追加 `using` ステートメント。

        using Microsoft.AspNet.Identity;
        using Microsoft.AspNet.Identity.EntityFramework;

1. 次の追加 **AddUserAndRole** メソッドをクラス。

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

1. 新しいメソッドを呼び出して、 **シード** メソッド。

        protected override void Seed(ContactManager.Models.ApplicationDbContext context)
        {
            AddUserAndRole(context);
            context.Contacts.AddOrUpdate(p => p.Name,
                // Code removed for brevity
        }

    変更内容を次のイメージが表示 *シード* メソッド。

    ![code image](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss24.PNG)

    このコードと呼ばれる新しいロールを作成する *canEdit*, 、新しいローカル ユーザー *user1@contoso.com*, 、し、追加 *user1@contoso.com* に、 *canEdit* ロールです。 詳細については、次を参照してください。、 [ASP.NET Identity チュートリアル](http://www.asp.net/identity/overview/features-api) ASP.NET サイトです。

## 一時的なコードを使用して新しいソーシャル ログイン ユーザーを canEdit ロールに追加する  ##

このセクションでは一時的に変更して、 **ExternalLoginConfirmation** 、OAuth プロバイダーを登録する新しいユーザーを追加するアカウント コント ローラーのメソッド、 *canEdit* ロールです。 同様のツールを提供するお役に立てば [WSAT](http://msdn.microsoft.com/library/ms228053.aspx) 今後できるようにするを作成し、ユーザー アカウントとロールを編集します。 そのときまで、一時的なコードを利用し、同じ機能を実現できます。

1. 開いている、 **controllers \accountcontroller.cs** ファイルに移動して、 **ExternalLoginConfirmation** メソッドです。

1. 次の呼び出しを追加 **AddToRoleAsync** 直前に、 **SignInAsync** 呼び出します。

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

このチュートリアルの中で、アプリケーションを Azure にデプロイします。そこでのログオンには、Google などサード パーティの認証プロバイダーが使用されます。 これにより、新しく登録されたアカウントに追加されます、 *canEdit* ロールです。 Web アプリの URL と Google ID さえあればだれでも登録し、データベースを更新することができます。 そのような操作が第三者によって行われるのを防ぐためには、サイトを停止する必要があります。 だれことを確認することができます、 *canEdit* 確認するには、データベース ロール。

 **パッケージ マネージャー コンソール** 上向きの矢印キーを押し、次のコマンドを呼び出します。

        Update-Database

 **Update-database** コマンドを実行する、 **シード** メソッド、および実行している、 **AddUserAndRole** メソッドの前に追加します。  **AddUserAndRole** メソッドは、ユーザーを作成 *user1@contoso.com* にユーザーを追加し、 *canEdit* ロールです。

## SSL と Authorize 属性を使用してアプリケーションを保護する ##

このセクションで適用する、 [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) アクション メソッドへのアクセスを制限する属性です。 匿名ユーザーがのみを表示できる、 **インデックス** home コント ローラーのアクション メソッドです。 登録済みのユーザーが連絡先データを表示できる (、 **インデックス** と **詳細** Cm コント ローラーのページ)、バージョン情報] ページで、および連絡先] ページ。 内のユーザーのみ、 *canEdit* ロールがデータを変更するアクセス アクション メソッドを実行します。

1. 開いている、 *\filterconfig.cs* ファイルし、置換、 *RegisterGlobalFilters* (を 2 つのフィルターを追加する) に次のメソッド。

        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new HandleErrorAttribute());
            filters.Add(new System.Web.Mvc.AuthorizeAttribute());
            filters.Add(new RequireHttpsAttribute());
        }
        
    このコードを追加、 [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) フィルターおよび [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) アプリケーションにフィルターします。  [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) フィルターにより、匿名ユーザーがアプリケーションのメソッドに一切アクセスします。 使用する、 [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) を匿名ユーザーがログインできる、ホーム ページを表示できるように、2 つのメソッドの承認要件を免除し属性です。   [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) HTTPS を介して web アプリにアクセスできる必要があります。

    追加する方法の 1 つは、 [Authorize](http://msdn.microsoft.com/library/system.web.mvc.authorizeattribute.aspx) 属性および [RequireHttps](http://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) 属性を各コント ローラーもありますアプリケーション全体に適用するセキュリティのベスト プラクティスです。 アプリケーション全体に適用すれば、新しいコントローラーやアクション メソッドを追加したとき、それらが自動的に保護されます。ユーザー自身で適用する必要がありません。 詳細については、次を参照してください。 [ASP.NET MVC アプリケーションと新しい AllowAnonymous 属性をセキュリティで保護する](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx)です。 

1. 追加、 [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) 属性を **インデックス** Home コント ローラーのメソッドです。  [AllowAnonymous](http://blogs.msdn.com/b/rickandy/archive/2012/03/23/securing-your-asp-net-mvc-4-app-and-the-new-allowanonymous-attribute.aspx) 属性を使用ホワイト リストを認証方法です。 

        public class HomeController : Controller
        {
          [AllowAnonymous]
          public ActionResult Index()
          {
             return View();
          }

    グローバルに検索を行う場合 *AllowAnonymous*, 、Account コント ローラーのログインと登録メソッドで使用されていることがわかります。

1.  *CmController.cs*, 、追加 `[Authorize(Roles = "canEdit")]` 、HttpGet メソッドと HttpPost メソッドにデータ (作成、編集、削除、Index と Details を除くすべてのアクション メソッド) を変更する、 *Cm* コント ローラーです。 追加後のコードは次のようになります。 

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

1. 前のセッションにログインしたまま、ヒット、 **ログアウト** リンクします。

1. をクリックして、 **に関する** または **連絡先** リンクします。 匿名ユーザーはこれらのページを表示できないため、ログイン ページにリダイレクトされます。

1. クリックして、 **新しいユーザーとして登録** リンクを電子メールでローカル ユーザーを追加 *joe@contoso.com*します。 確認 *Joe* 約、ホームを表示して、ページにお問い合わせください。 

    ![login](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss14.PNG)

1. クリックして、 *CM Demo* をリンクし、データを表示することを確認します。

1. ページ上の編集リンクをクリックし、ログイン ページにリダイレクトされます (新しいローカル ユーザーに追加されないため、 *canEdit* ロール)。

1. としてログイン *user1@contoso.com* パスワード"p_assw0rd1"("word"の「0」はゼロ)。 先ほど選択した編集ページにリダイレクトされます。 

    このアカウントとパスワードでログインできない場合は、ソース コードからパスワードをコピーして貼り付けてみてください。 ログインできない場合、 **UserName** の列、 **AspNetUsers** テーブルを確認 *user1@contoso.com* が追加されました。 

1. データを変更できることを確認します。

## Azure にアプリケーションをデプロイする

1. Visual Studio でプロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行** 、コンテキスト メニューからです。

    ![プロジェクトのコンテキスト メニューの [発行]][firsdeploy003]

     **Web の発行** ウィザードが開きます。

1. クリックして、 **設定** ] タブの左側にある、 **Web の発行** ] ダイアログ ボックス。 

2. クリックして、 **v** アイコンをクリックして、 **リモート接続文字列** の **ApplicationDbContext** し、プロジェクトを作成したときに作成したデータベースを選択します。
   
    ![設定](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrc2.png)

1.  **ContactManagerContext**, [ **Code First Migrations の実行**します。

    ![設定](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrc3.png)

1. クリックして **発行**します。

1. としてログイン *user1@contoso.com* (パスワード"p_assw0rd1") のデータを編集できることを確認します。

1. ログアウトします。

1. 移動して、 [Google Developers Console](https://console.developers.google.com/) および、 **資格情報** タブ リダイレクト URIS と JavaScript orgins ように変更が Azure の URL を使用します。

1. Google または Facebook を使用してログインします。 Google または Facebook アカウントを追加するは、 **canEdit** ロールです。 メッセージと共に HTTP 400 エラーが表示 *要求のリダイレクト URI: https://contactmanager {my version}.azurewebsites.net/signin-google 登録リダイレクト uri が一致しませんでした*, 、加えた変更が反映されるまで待機する必要があります。 数分経ってからこのエラーが発生する場合は、URI は正しいことを確認します。

### Web アプリを停止して第三者の登録を防ぐ  

1.  **サーバー エクスプ ローラー**, に移動 **Azure > アプリ サービス > {リソース グループ} > {web アプリ}**します。

4. Web アプリを右クリックして **停止**します。 

    またはから、 [Azure ポータル](https://portal.azure.com/), 、web アプリのブレードに移動し、をクリックできます、 **停止** ブレードの上部にあるアイコン。

    ![web アプリ ポータルの停止](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/stopweb.png)

### AddToRoleAsync の削除、発行、テスト

1. コメント アウトしてから次のコードを削除するか、 **ExternalLoginConfirmation** 、Account コント ローラー メソッド。

        await UserManager.AddToRoleAsync(user.Id, "canEdit");

1. プロジェクトをビルドします。これにより、ファイルの変更が保存され、コンパイル エラーがないか確認が行われます。

5. プロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行**します。

       ![Publish in project context menu](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/GS13publish.png)
    
4. クリックして、 **プレビューの開始** ] ボタンをクリックします。 更新する必要のあるファイルだけがデプロイされます。

5. Visual Studio またはポータルから Web アプリを開きます。 **Web アプリが停止した状態で発行することはできません**します。

    ![start web app](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss15.png)

5. Visual Studio に戻り、をクリックして **発行**します。

3. Azure アプリケーションが既定のブラウザーで起動します。 ログインして、ログアウトすると、匿名ユーザーとしてホーム ページを表示できます。  

4. クリックして、 **に関する** リンクします。 ログイン ページにリダイレクトされます。

5. クリックして、 **登録** 、ログイン ページにリンクし、ローカル アカウントを作成します。 このローカル アカウントを使用して読み取り専用ページにアクセスすることができますが、データを変更するページにアクセスできないことを確認 (によって保護されている、 *canEdit* ロール)。 ローカル アカウント アクセスは、後でこのチュートリアルの中で削除します。 

    ![登録](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss16.PNG)

1. 移動できることを確認、 *に関する* と *連絡先* ページです。

    ![ログオフ](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/ss17.PNG)

1. クリックして、 **CM Demo** に移動するリンク、 **Cm** コント ローラーです。 追加する代わりに、 *Cm* URL にします。 

    ![CM page](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr4.png)
 
1. [編集] リンクをクリックします。 

    ログイン ページにリダイレクトされます。 

2.  **別のサービスを使用してログイン**, をクリックして Google または Facebook とログ過去に登録したアカウントを使用しています。 (短時間で作業して、セッション Cookie がタイムアウトしていない場合は、以前に使用した Google アカウントまたは Facebook アカウントで自動的にログインされます)。

2. そのアカウントにログインした状態でデータを編集できることを確認します。

    **注:** することはできません Google からログアウトこのアプリとログに、同じブラウザーで別の google アカウント。 1 つのブラウザーを使用している場合は、Google に移動してログアウトする必要があります。 同じサード パーティの認証システム (Google など) に属している別のアカウントでログオンするためには、異なるブラウザーを使用する必要があります。

    Google アカウント情報の氏名を入力しないと、NullReferenceException が発生します。

## SQL Azure DB を調べる ##

1.  **サーバー エクスプ ローラー**, に移動 **Azure > SQL データベース > {データベース}**

2. データベースを右クリックし、[ **[SQL Server オブジェクト エクスプ ローラーで開く**します。
 
    ![open in SSOX](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr12.png)
 
3. このデータベースに接続したことがない場合は、現在の IP アドレスへのアクセスを有効にするファイアウォール規則を追加するように求められることがあります。 IP アドレスは、自動的に入力されます。 クリックするだけ **ファイアウォール ルールの追加** アクセスを有効にします。

    ![add firewall rule](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/addfirewallrule.png)

3. データベース サーバーの作成時に指定したユーザー名とパスワードを使用してデータベースにログインします。 
 
1. 右クリックして、 **AspNetUsers** テーブルが表示され選択 **データの表示**します。

    ![CM page](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rrr8.png)
 
1. Id であるへの登録した Google アカウントをメモします、 **canEdit** ロール、および Id の *user1@contoso.com*します。 以外のユーザーになる条件は、 **canEdit** ロールです。 この点については、次のステップで確認します。

    ![CM page](./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/s2.png)
 
2.  **SQL Server オブジェクト エクスプ ローラー**, を右クリックして **AspNetUserRoles** 選択 **データの表示**します。

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

- [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (ログイン、電子メール確認、パスワードのリセットを使用できる安全な ASP.NET MVC 5 web アプリケーションの作成)](http://www.asp.net/mvc/overview/getting-started/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset)
- [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication (SMS と電子メールの 2 要素認証を使用する ASP.NET MVC 5 アプリケーション)](http://www.asp.net/mvc/overview/getting-started/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication)
- [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure (ASP.NET および Azure へのパスワードなどの機密データのデプロイのベスト プラクティス)](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure) 
- [Facebook、Google の OAuth2 で ASP.NET MVC 5 アプリケーションを作成](http://www.asp.net/mvc/tutorials/mvc-5/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on ) ユーザー登録データベースには、認証プロバイダーとして Facebook の使用に関する詳細な手順については、プロファイル データを追加する方法の手順についても説明します。
- [Getting Started with ASP.NET MVC 5 (ASP.NET MVC 5 の使用)](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)

Entity Framework を使用する方法についてより高度なチュートリアルについては、次を参照してください。 [EF と MVC の概要](http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)します。

このチュートリアルが @blowdart [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) と Tom Dykstra と Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart))。 

***フィードバックのままにしてください。*** 立った内容やなどして、強化されたで紹介した製品に関しても自体、チュートリアルに関してだけでなく、します。 お寄せいただいたご意見は、今後の改善に役立たせていただきます。 要求アンケートに新しいトピックに投票したり [Me 方法とコードの表示](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)します。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Using the Membership API]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2

[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[deployapp11]: #bkmk_deploytowindowsazure11
[adddb]: #bkmk_addadatabase


<!-- images-->
[rx2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rx2.png

[rx5]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx5.png
[rx6]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx6.png
[rx7]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx7.png
[rx8]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx8.png
[rx9]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rx9.png

[rxb]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxb.png


[rxSSL]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/rxSSL.png

[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT2.png

[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rxNOT]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxNOT.png
[rr1]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rr1.png

[rxPrevDB]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxPrevDB.png

[rxWSnew]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxWSnew2.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/rxCreateWSwithDB.png

[setup007]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-setup-azure-site-004.png

[newapp004]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-createapp-004.png

[firsdeploy003]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-deploy1-publish-001.png

[adddb002]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-controller-add-context-menu.png

[addcode008]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/dntutmobile-migrations-package-manager-console.png


[Important information about ASP.NET in Azure web apps]: #aspnetwindowsazureinfo
[Next steps]: #nextsteps

[ImportPublishSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database-vs2013/ImportPublishSettings.png
 

