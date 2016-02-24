<properties 
    pageTitle="ASP.NET Web API と SQL Database を使用する Rest サービスを Azure App Service に作成する" 
    description="Visual Studio により ASP.NET Web API を使用するアプリケーションを Azure Web アプリケーションにデプロイする方法を示すチュートリアル。" 
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
    ms.topic="article" 
    ms.date="12/04/2015" 
    ms.author="riande"/>

# ASP.NET Web API と SQL Database を使用する Rest サービスを Azure App Service に作成する

このチュートリアルでは、ASP.NET web アプリを展開する方法、 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Visual Studio 2013 または Visual Studio 2013 Community Edition で Web の発行ウィザードを使用しています。 

Azure アカウントは無料で開くことができます。また、まだ Visual Studio 2013 を持っていない場合は、SDK によって Visual Studio 2013 for Web Express が自動的にインストールされます。 これで、Azure 向けの開発を完全に無料で始めることができます。

このチュートリアルは、Azure を使用した経験がない読者を対象に作成されています。 このチュートリアルでは、クラウドで動作する単純な Web アプリケーションを作成します。
 
学習内容:

* Azure SDK をインストールして、Azure 向け開発用にコンピューターを準備する方法
* Visual Studio の ASP.NET MVC 5 プロジェクトを作成して Azure アプリケーションに発行する方法
* ASP.NET Web API を使用して REST ベースの API 呼び出しを可能にする方法
* SQL データベースを使用して Azure にデータを保存する方法
* Azure にアプリケーションの更新を発行する方法

ASP.NET MVC 5 に基づく、データベース アクセスに ADO.NET Entity Framework を使用する、簡単な連絡先リスト Web アプリケーションをビルドします。 次の図に、完成したアプリケーションを示します。

![web サイトのスクリーン ショット][intro001]

<!-- the next line produces the "Set up the development environment" section as see at http://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/ -->
[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

### プロジェクトを作成する

1. Visual Studio 2013 を起動します。
1.  **ファイル** ボタンをクリックし **新しいプロジェクト**します。
3.  **新しいプロジェクト** ] ダイアログ ボックスで、展開 **Visual c#** を選択して **Web**  し、[ **ASP.NET Web アプリケーション**します。 アプリケーションの名前 **ContactManager** ] をクリック **OK**します。

    ![New Project dialog box](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr4.png)

1.  **新しい ASP.NET プロジェクト** ダイアログ ボックスで、 **MVC** テンプレート チェック **Web API** ] をクリックし、 **認証の変更**します。

    ![[新しい ASP.NET プロジェクト] ダイアログ ボックス](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt3.png)

1.  **認証の変更** ダイアログ ボックスで、をクリックして **認証なし**, 、] をクリックし、 **[ok]**します。

    ![[認証なし]](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/GS13noauth.png)

    作成中のサンプル アプリケーションには、ユーザーのログインが必要な機能は実装されません。 認証および承認機能を実装する方法については、次を参照してください。、 [次のステップ](#nextsteps) このチュートリアルの末尾にある「します。 

1.  **新しい ASP.NET プロジェクト** ] ダイアログ ボックスで確認、 **クラウドでホスト** がチェックされ、をクリックして **OK**します。

    ![[新しい ASP.NET プロジェクト] ダイアログ ボックス](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt3.png)

Azure にまだサインしていない場合は、サインインするように求められます。

1. 基づく一意の名前を構成ウィザードが提示されます *ContactManager* (下図を参照してください)。 近くのリージョンを選択します。 使用する [azurespeed.com](http://www.azurespeed.com/ "AzureSpeed.com") 最も待機時間のデータ センターを検索します。 
2. 前に、データベース サーバーを作成していない場合は、選択 **新しいサーバーの作成**, 、データベースのユーザー名とパスワードを入力します。

    ![Configure Azure Website](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configAz.PNG)

データベース サーバーがある場合は、そのサーバーを使用して新しいデータベースを作成します。 データベース サーバーは貴重なリソースであるため、一般的にはデータベースごとにデータベース サーバーを作成するのではなく、同じサーバー上にテスト用や開発用の複数のデータベースを作成してください。 Web サイトとデータベースが同じリージョンにあることを確認してください。

![Configure Azure Website](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/configWithDB.PNG)

### ページのヘッダーとフッターを設定する


1.  **ソリューション エクスプ ローラー**, 、展開、 *views \shared* フォルダーとオープン、 *_Layout.cshtml* ファイルです。

    ![ソリューション エクスプ ローラーの _Layout.cshtml][newapp004]

1. 内容を置き換える、 *\shared_layout.cshtml* を次のコード ファイル。


        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="utf-8" />
            <title>@ViewBag.Title - Contact Manager</title>
            <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
            <meta name="viewport" content="width=device-width" />
            @Styles.Render("~/Content/css")
            @Scripts.Render("~/bundles/modernizr")
        </head>
        <body>
            <header>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p class="site-title">@Html.ActionLink("Contact Manager", "Index", "Home")</p>
                    </div>
                </div>
            </header>
            <div id="body">
                @RenderSection("featured", required: false)
                <section class="content-wrapper main-content clear-fix">
                    @RenderBody()
                </section>
            </div>
            <footer>
                <div class="content-wrapper">
                    <div class="float-left">
                        <p>&copy; @DateTime.Now.Year - Contact Manager</p>
                    </div>
                </div>
            </footer>
            @Scripts.Render("~/bundles/jquery")
            @RenderSection("scripts", required: false)
        </body>
        </html>
            
上記のマークアップにより、アプリケーション名"My ASP.NET App"から"Contact Manager"に変更しへのリンクを削除 **ホーム**, 、**に関する** と **連絡先**します。

### ローカルでアプリケーションを実行する

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。
アプリケーションのホーム ページが既定のブラウザーに表示されます。
    ![To Do List のホーム ページ](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr5.png)

これで、Azure にデプロイするアプリケーションを作成するために必要な操作が完了しました。 データベース機能は後で追加します。

## Azure にアプリケーションをデプロイする

1. Visual Studio でプロジェクトを右クリックして **ソリューション エクスプ ローラー** 選択 **発行** 、コンテキスト メニューからです。

    ![プロジェクトのコンテキスト メニューに公開する][PublishVSSolution]

     **Web の発行** ウィザードが開きます。

12. クリックして **発行**します。

![Settings tab](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/pw.png)

Azure サーバーにファイルをコピーする処理が開始されます。  **出力** ウィンドウがどのような展開アクションを実行したが表示され、展開が正常に完了を報告します。

14. 自動的に既定のブラウザーが開き、デプロイ先のサイトの URL にアクセスします。

    これで、作成したアプリケーションはクラウドで実行されています。
    
    ![To Do List のホーム ページが Azure で実行する][rxz2]

## アプリケーションにデータベースを追加する

次に、MVC アプリケーションを更新して、連絡先を表示および更新してデータをデータベースに保存する機能を追加します。 アプリケーションでは、データベースの作成およびデータベース内のデータの読み取りと更新に Entity Framework を使用します。

### 連絡先のデータ モデル クラスを追加する

まず、コードで単純なデータ モデルを作成します。

1.  **ソリューション エクスプ ローラー**, を Models フォルダーを右クリックして、 **追加**, 、し **クラス**します。

    ![モデル] フォルダーのコンテキスト メニューでクラスの追加][adddb001]

2.  **新しい項目の追加** ダイアログ ボックスで、新しいクラス ファイルの名前 *Contact.cs*, 、クリックして **追加**します。

    ![新しい項目の追加] ダイアログ ボックスの追加][adddb002]

3. Contacts.cs ファイルの内容を次のコードに置き換えます。

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
                public string Email { get; set; }
                public string Twitter { get; set; }
                public string Self
                {
                    get { return string.Format(CultureInfo.CurrentCulture,
                         "api/contacts/{0}", this.ContactId); }
                    set { }
                }
            }
        }

 **にお問い合わせください** クラスについては、それぞれの連絡先と、プライマリ キー (ContactID) データベースで必要とされる保存データを定義します。 データ モデルについての詳細を取得できます、 [次のステップ](#nextsteps) このチュートリアルの末尾にある「します。

### アプリケーション ユーザーが連絡先を操作できる Web ページを作成する

ASP.NET MVC では、スキャフォールディング機能によって、作成、読み取り、更新、および削除 (CRUD) の各操作を実行するコードを自動的に生成できます。

## データのコントローラーとビューを追加する

1.  **ソリューション エクスプ ローラー**, 、Controllers フォルダーを展開します。

3. プロジェクトをビルド **(Ctrl + Shift + B)**します。 (スキャフォールディング機能の使用前にプロジェクトをビルドする必要があります。) 

4. Controllers フォルダーを右クリックし、をクリックして **追加**, 、クリックして **コント ローラー**します。

    ![フォルダーのコンテキスト メニューでコント ローラーの追加][addcode001]

1.  **スキャフォールディングの追加** ダイアログ ボックスで、 **Entity Framework を使用するビューと MVC コント ローラー** ] をクリック **追加**します。

 ![コントローラーの追加](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rrAC.png)

6. コント ローラー名を設定 **HomeController**します。 選択 **連絡先** モデル クラスとして。 クリックして、 **新しいデータ コンテキスト** ボタンをクリックし、既定の"ContactManager.Models.ContactManagerContext"を **新しいデータ コンテキスト型**します。 クリックして **追加**します。

    ![Add Controller dialog box](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rr9.png)

    ダイアログ ボックスで、"HomeController という名前のファイルは既に存在します。 ファイルを置き換えますか?" とたずねられます。 クリックして **はい**します。 新しいプロジェクトで作成した home コントローラーで上書きされます。 連絡先リストに新しい home コントローラーが使用されるようになります。

    Visual Studio は、コント ローラー メソッドとビューの CRUD データベース操作の作成 **連絡先** オブジェクトです。

## Migrations の有効化、データベースの作成、サンプル データとデータ初期化子の追加 ##

次のタスクが有効にするのには、 [Code First Migrations](http://curah.microsoft.com/55220) 作成したデータ モデルに基づいてデータベースを作成するために機能します。

1.  **ツール** メニューの [ **ライブラリ パッケージ マネージャー** し **パッケージ マネージャー コンソール**します。

    ![ツール] メニューの [パッケージ マネージャー コンソール][addcode008]

2.  **パッケージ マネージャー コンソール** ウィンドウで、次のコマンドを入力します。

        enable-migrations 
  
     **、Enable-migrations** コマンドは、作成、 *移行* フォルダーとその、そのフォルダーに格納、 *Configuration.cs* ファイルを編集して Migration を構成します。 

2.  **パッケージ マネージャー コンソール** ウィンドウで、次のコマンドを入力します。

        add-migration Initial

     **、Add-migration Initial** という名前のクラスを生成するコマンド **& lt; date_stamp & gt; 初期** データベースを作成します。 最初のパラメーター ( *初期* ) は任意であり、ファイルの名前を作成するために使用します。 新しいクラス ファイルを確認できます **ソリューション エクスプ ローラー**します。

     **初期** クラス、 **を** メソッドが Contacts テーブルを作成し、 **ダウン** (前の状態に戻したい場合に使用) メソッドを使用します。

3. 開いている、 *migrations \configuration.cs* ファイルです。 

4. 次の名前空間を追加します。 

         using ContactManager.Models;

5. 置き換える、 *シード* メソッドを次のコード。
        
        protected override void Seed(ContactManager.Models.ContactManagerContext context)
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
                   Twitter = "debra_example"
               },
                new Contact
                {
                    Name = "Thorsten Weinrich",
                    Address = "5678 1st Ave W",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "thorsten@example.com",
                    Twitter = "thorsten_example"
                },
                new Contact
                {
                    Name = "Yuhong Li",
                    Address = "9012 State st",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "yuhong@example.com",
                    Twitter = "yuhong_example"
                },
                new Contact
                {
                    Name = "Jon Orton",
                    Address = "3456 Maple St",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "jon@example.com",
                    Twitter = "jon_example"
                },
                new Contact
                {
                    Name = "Diliana Alexieva-Bosseva",
                    Address = "7890 2nd Ave E",
                    City = "Redmond",
                    State = "WA",
                    Zip = "10999",
                    Email = "diliana@example.com",
                    Twitter = "diliana_example"
                }
                );
        }

    このコードでは、連絡先情報を使用してデータベースを初期化します。 シード データベースの生成の詳細については、次を参照してください。 [デバッグ Entity Framework (EF) Db](http://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)します。


1.  **パッケージ マネージャー コンソール** コマンドを入力します。

        update-database

    ![パッケージ マネージャー コンソールのコマンド][addcode009]

     **データベースの更新** 、データベースを作成する最初の移行を実行します。 既定では、データベースは SQL Server Express LocalDB データベースとして作成されます

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。 

アプリケーションでは、登録されたデータが表示され、編集、詳細、削除のリンクが示されます。

![データの MVC view][rxz3]

## ビューの編集

1. 開いている、 *Views\Home\Index.cshtml* ファイルです。 次の手順では、生成されたマークアップと置き換わりますを使用するコード [jQuery](http://jquery.com/) と [Knockout.js](http://knockoutjs.com/)します。 この新しいコードは、Web API と JSON を使用して連絡先リストを取得し、knockout.js を使用して連絡先データを UI にバインドします。 詳細については、次を参照してください。、 [次のステップ](#nextsteps) このチュートリアルの末尾にある「します。 


2. このファイルの内容を次のコードに置き換えます。

        @model IEnumerable<ContactManager.Models.Contact>
        @{
            ViewBag.Title = "Home";
        }
        @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                function ContactsViewModel() {
                    var self = this;
                    self.contacts = ko.observableArray([]);
                    self.addContact = function () {
                        $.post("api/contacts",
                            $("#addContact").serialize(),
                            function (value) {
                                self.contacts.push(value);
                            },
                            "json");
                    }
                    self.removeContact = function (contact) {
                        $.ajax({
                            type: "DELETE",
                            url: contact.Self,
                            success: function () {
                                self.contacts.remove(contact);
                            }
                        });
                    }

                    $.getJSON("api/contacts", function (data) {
                        self.contacts(data);
                    });
                }
                ko.applyBindings(new ContactsViewModel());  
        </script>
        }
        <ul id="contacts" data-bind="foreach: contacts">
            <li class="ui-widget-content ui-corner-all">
                <h1 data-bind="text: Name" class="ui-widget-header"></h1>
                <div><span data-bind="text: $data.Address || 'Address?'"></span></div>
                <div>
                    <span data-bind="text: $data.City || 'City?'"></span>,
                    <span data-bind="text: $data.State || 'State?'"></span>
                    <span data-bind="text: $data.Zip || 'Zip?'"></span>
                </div>
                <div data-bind="if: $data.Email"><a data-bind="attr: { href: 'mailto:' + Email }, text: Email"></a></div>
                <div data-bind="ifnot: $data.Email"><span>Email?</span></div>
                <div data-bind="if: $data.Twitter"><a data-bind="attr: { href: 'http://twitter.com/' + Twitter }, text: '@@' + Twitter"></a></div>
                <div data-bind="ifnot: $data.Twitter"><span>Twitter?</span></div>
                <p><a data-bind="attr: { href: Self }, click: $root.removeContact" class="removeContact ui-state-default ui-corner-all">Remove</a></p>
            </li>
        </ul>
        <form id="addContact" data-bind="submit: addContact">
            <fieldset>
                <legend>Add New Contact</legend>
                <ol>
                    <li>
                        <label for="Name">Name</label>
                        <input type="text" name="Name" />
                    </li>
                    <li>
                        <label for="Address">Address</label>
                        <input type="text" name="Address" >
                    </li>
                    <li>
                        <label for="City">City</label>
                        <input type="text" name="City" />
                    </li>
                    <li>
                        <label for="State">State</label>
                        <input type="text" name="State" />
                    </li>
                    <li>
                        <label for="Zip">Zip</label>
                        <input type="text" name="Zip" />
                    </li>
                    <li>
                        <label for="Email">E-mail</label>
                        <input type="text" name="Email" />
                    </li>
                    <li>
                        <label for="Twitter">Twitter</label>
                        <input type="text" name="Twitter" />
                    </li>
                </ol>
                <input type="submit" value="Add" />
            </fieldset>
        </form>

3. コンテンツのフォルダーを右クリックし、をクリックして **追加**, 、クリックして **新しい項目]**します。

    ![コンテンツのフォルダーのショートカット メニューのスタイル シートの追加][addcode005]

4.  **新しい項目の追加** ] ダイアログ ボックスで、「 **スタイル** 右上に検索ボックスを選択し、 **スタイル シート**します。
    ![新しい項目の追加] ダイアログ ボックスの追加][rxStyle]

5. ファイルに名前を *Contacts.css* ] をクリック **追加**します。 このファイルの内容を次のコードに置き換えます。
    
        .column {
            float: left;
            width: 50%;
            padding: 0;
            margin: 5px 0;
        }
        form ol {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }
        form li {
            padding: 1px;
            margin: 3px;
        }
        form input[type="text"] {
            width: 100%;
        }
        #addContact {
            width: 300px;
            float: left;
            width:30%;
        }
        #contacts {
            list-style-type: none;
            margin: 0;
            padding: 0;
            float:left;
            width: 70%;
        }
        #contacts li {
            margin: 3px 3px 3px 0;
            padding: 1px;
            float: left;
            width: 300px;
            text-align: center;
            background-image: none;
            background-color: #F5F5F5;
        }
        #contacts li h1
        {
            padding: 0;
            margin: 0;
            background-image: none;
            background-color: Orange;
            color: White;
            font-family: Trebuchet MS, Tahoma, Verdana, Arial, sans-serif;
        }
        .removeContact, .viewImage
        {
            padding: 3px;
            text-decoration: none;
        }

    このスタイル シートは Contact Manager アプリケーションで使用されるレイアウト、色、スタイルに適用されます。

6. 開いている、 *app_start \bundleconfig.cs* ファイルです。


7. 登録する次のコードを追加、 [Knockout](http://knockoutjs.com/index.html "KO") プラグインします。

        bundles.Add(new ScriptBundle("~/bundles/knockout").Include(
                    "~/Scripts/knockout-{version}.js"));
    このサンプルでは Knockout を使用して、画面テンプレートを処理する動的な JavaScript コードを簡略化します。

8. Contents/css エントリを登録の変更、 *contacts.css* スタイル シートです。 次の行を変更します。

                 bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/site.css"));
変更後:

        bundles.Add(new StyleBundle("~/Content/css").Include(
                   "~/Content/bootstrap.css",
                   "~/Content/contacts.css",
                   "~/Content/site.css"));

1. パッケージ マネージャー コンソールで、次のコマンドを実行して Knockout をインストールします。

        Install-Package knockoutjs

## Web API を使用する REST ベースのインターフェイスに対応したコントローラーを追加する

1.  **ソリューション エクスプ ローラー**, コント ローラーを右クリックし、クリックして、 **追加** し **コント ローラー.** 

1.  **スキャフォールディングの追加** ] ダイアログ ボックスで、「 **Web API 2 コント ローラーを、操作の Entity Framework を使用して** ] をクリックし、 **追加**します。

    ![API コントローラーの追加](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt1.png)

4.  **コント ローラーの追加** ] ダイアログ ボックスで、コント ローラー名として「ContactsController」と入力します。 "Contact (ContactManager.Models)"を選択して、 **モデル クラス**します。  既定値の保持、 **データ コンテキスト クラス**します。 

6. クリックして **追加**します。

### ローカルでアプリケーションを実行する

1. Ctrl キーを押しながら F5 キーを押してアプリケーションを実行します。

    ![インデックスのページ][intro001]

2. 連絡先を入力し、クリックして **追加**します。 アプリケーションはホーム ページに戻り、入力した連絡先が表示されます。

    ![タスク一覧の項目のインデックス ページ][addwebapi004]

3. ブラウザーで、次のように追加します。 **/api/連絡先** URL にします。

    追加後の URL は http://localhost:1234/api/contacts のようになります。 追加した REST ベースの Web API によって、保存されている連絡先が返されます。 Firefox と Chrome では、それらのデータが XML 形式で表示されます。

    ![タスク一覧の項目のインデックス ページ][rxFFchrome]
    

    IE will prompt you to open or save the contacts.

    ![Web API save dialog][addwebapi006]
    
    
    You can open the returned contacts in notepad or a browser.
    
    This output can be consumed by another application such as mobile web page or application.

    ![Web API save dialog][addwebapi007]

    **Security Warning**: At this point, your application is insecure and vulnerable to CSRF attack. Later in the tutorial we will remove this vulnerability. For more information see [Preventing Cross-Site Request Forgery (CSRF) Attacks][prevent-csrf-attacks].
## XSRF 保護を追加する

クロスサイト リクエスト フォージェリ (XSRF または CSRF) は、Web でホストされるアプリケーションに対する攻撃であり、それによって悪意のある Web サイトが、クライアント ブラウザーとそのブラウザーが信頼する Web サイトの間のやり取りに影響を及ぼす可能性があります。 これらの攻撃が可能になるのは、要求ごとに Web ブラウザーが自動的に認証トークンを Web サイトに送信するからです。 標準的な例は、ASP.NET のフォーム認証チケットなどの認証クッキーです。 ただし、永続的な認証メカニズム (Windows 認証や基本認証など) を使用する Web サイトも、これらの攻撃の対象になることがあります。

XSRF 攻撃はフィッシング攻撃とは異なります。 フィッシング攻撃には攻撃対象とのやり取りが必要です。 フィッシング攻撃では、悪意のある Web サイトがターゲット Web サイトを模擬し、攻撃対象は重要な情報を攻撃者に提供するようにだまされます。 XSRF 攻撃では、多くの場合に攻撃対象とのやり取りは必要ありません。 むしろ攻撃者が利用するのは、ブラウザーがすべての関連クッキーを模擬 Web サイトに自動的に送信することです。

詳細については、次を参照してください。、 [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))します。

1.  **ソリューション エクスプ ローラー**, 、右 **ContactManager** プロジェクトを **追加** ] をクリックし、 **クラス**します。

2. ファイルに名前を *ValidateHttpAntiForgeryTokenAttribute.cs* し、次のコードを追加します。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Net;
        using System.Net.Http;
        using System.Web.Helpers;
        using System.Web.Http.Controllers;
        using System.Web.Http.Filters;
        using System.Web.Mvc;
        namespace ContactManager.Filters
        {
            public class ValidateHttpAntiForgeryTokenAttribute : AuthorizationFilterAttribute
            {
                public override void OnAuthorization(HttpActionContext actionContext)
                {
                    HttpRequestMessage request = actionContext.ControllerContext.Request;
                    try
                    {
                        if (IsAjaxRequest(request))
                        {
                            ValidateRequestHeader(request);
                        }
                        else
                        {
                            AntiForgery.Validate();
                        }
                    }
                    catch (HttpAntiForgeryException e)
                    {
                        actionContext.Response = request.CreateErrorResponse(HttpStatusCode.Forbidden, e);
                    }
                }
                private bool IsAjaxRequest(HttpRequestMessage request)
                {
                    IEnumerable<string> xRequestedWithHeaders;
                    if (request.Headers.TryGetValues("X-Requested-With", out xRequestedWithHeaders))
                    {
                        string headerValue = xRequestedWithHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(headerValue))
                        {
                            return String.Equals(headerValue, "XMLHttpRequest", StringComparison.OrdinalIgnoreCase);
                        }
                    }
                    return false;
                }
                private void ValidateRequestHeader(HttpRequestMessage request)
                {
                    string cookieToken = String.Empty;
                    string formToken = String.Empty;
                    IEnumerable<string> tokenHeaders;
                    if (request.Headers.TryGetValues("RequestVerificationToken", out tokenHeaders))
                    {
                        string tokenValue = tokenHeaders.FirstOrDefault();
                        if (!String.IsNullOrEmpty(tokenValue))
                        {
                            string[] tokens = tokenValue.Split(':');
                            if (tokens.Length == 2)
                            {
                                cookieToken = tokens[0].Trim();
                                formToken = tokens[1].Trim();
                            }
                        }
                    }
                    AntiForgery.Validate(cookieToken, formToken);
                }
            }
        }

1. 次の追加 *を使用して* ステートメントを contracts コント ローラーにアクセスできるように、 **[ValidateHttpAntiForgeryToken]** 属性です。

        using ContactManager.Filters;

1. 追加、 **[ValidateHttpAntiForgeryToken]** 属性の Post メソッドを **ContactsController** XSRF の脅威から保護します。 コードを"PutContact"、"PostContact"を追加し、 **DeleteContact** アクション メソッドです。

        [ValidateHttpAntiForgeryToken]
            public IHttpActionResult PutContact(int id, Contact contact)
            {

1. 更新プログラム、 *スクリプト* のセクションで、 *Views\Home\Index.cshtml* ファイル、XSRF トークンを取得するコードを含めます。

         @section Scripts {
            @Scripts.Render("~/bundles/knockout")
            <script type="text/javascript">
                @functions{
                   public string TokenHeaderValue()
                   {
                      string cookieToken, formToken;
                      AntiForgery.GetTokens(null, out cookieToken, out formToken);
                      return cookieToken + ":" + formToken;                
                   }
                }

               function ContactsViewModel() {
                  var self = this;
                  self.contacts = ko.observableArray([]);
                  self.addContact = function () {

                     $.ajax({
                        type: "post",
                        url: "api/contacts",
                        data: $("#addContact").serialize(),
                        dataType: "json",
                        success: function (value) {
                           self.contacts.push(value);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }
                     });

                  }
                  self.removeContact = function (contact) {
                     $.ajax({
                        type: "DELETE",
                        url: contact.Self,
                        success: function () {
                           self.contacts.remove(contact);
                        },
                        headers: {
                           'RequestVerificationToken': '@TokenHeaderValue()'
                        }

                     });
                  }

                  $.getJSON("api/contacts", function (data) {
                     self.contacts(data);
                  });
               }
               ko.applyBindings(new ContactsViewModel());
            </script>
         }


## Azure および SQL データベースにアプリケーションの更新を発行する

アプリケーションを発行するには、前に説明した手順を繰り返します。

1.  **ソリューション エクスプ ローラー**, プロジェクトを右クリックして、選択 **発行**します。

    ![発行][rxP]

5. クリックして、 **設定** ] タブをクリックします。
    

1. [ **Contactsmanagercontext (contactsmanagercontext)**, 、] をクリックして、 **v** を変更するにはアイコン *リモート接続文字列* 連絡先データベースの接続文字列にします。 クリックして **ContactDB**します。

    ![設定](./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rt5.png)

7. チェック ボックスをオン **コード First Migrations を実行 (アプリケーションの起動時に実行)**します。

1. をクリックして **次** ] をクリックし、 **プレビュー**します。 Visual Studio で、追加または更新されるファイルの一覧が表示されます。

8. クリックして **発行**します。
デプロイが完了すると、ブラウザーでアプリケーションのホーム ページが開かれます。

    ![連絡先のインデックス ページ][intro001]

    Visual Studio の発行プロセスに自動的に展開された接続文字列が構成されている *Web.config* SQL データベースを指すファイルです。 また、Code First Migrations は、デプロイ後にアプリケーションが初めてデータベースに接続するときに、データベースを最新バージョンに自動的にアップグレードするよう構成されました。

    この構成の結果 Code First によって作成されたデータベース、コードを実行している、 **初期** 前に作成したクラスです。 この処理は、デプロイ後にアプリケーションが初めてデータベースにアクセスしようとしたときに行われました。

9. アプリケーションをローカルで実行したときと同様に連絡先を入力して、データベースのデプロイが成功したことを確認します。

入力した項目が保存され、Contact Manager のページに表示されることを確認すると、その項目がデータベースに保存されたことがわかります。

![連絡先のインデックス ページ][addwebapi004]

これで、データの保存先に SQL Database を使用して、アプリケーションがクラウドで実行されるようになりました。 Azure 上でアプリケーションのテストを終えたら、そのアプリケーションを削除します。 アプリケーションはパブリックであり、アクセスを制限するメカニズムを備えていません。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 次のステップ

実際のアプリケーションでは認証と権限承認が必要になるため、その目的でメンバーシップ データベースを使用します。 このチュートリアル [OAuth, Membership and SQL Database との安全な ASP.NET MVC アプリケーションを配置](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md) はこのチュートリアルに基づいており、メンバーシップ データベースと web アプリケーションを展開する方法を示します。

Azure アプリケーションにデータを保存するには、Azure Storage を使用する方法もあります。Azure Storage には、非リレーショナル データを BLOB 形式とテーブル形式で保存できます。 Web API、ASP.NET MVC、および Window Azure の詳細については、次の Web ページを参照してください。
 

* 追加する [getting Started with MVC を使用して Entity Framework][EFCodeFirstMVCTutorial]
* [ASP.NET MVC 5 の入門ページ](http://www.asp.net/mvc/tutorials/mvc-5/introduction/getting-started)
* [ASP.NET Web API の入門ページ](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)
* [Debugging WAWS (WAWS のデバッグ)](web-sites-dotnet-troubleshoot-visual-studio.md)

このチュートリアルとサンプル アプリケーションが執筆 [Rick Anderson](http://blogs.msdn.com/b/rickandy/) (Twitter [@RickAndMSFT](https://twitter.com/RickAndMSFT)) と Tom Dykstra と Barry Dorrans (Twitter [@blowdart](https://twitter.com/blowdart))。 

役に立った内容や改善点など、皆様からのご意見をお寄せください。このチュートリアルに関してだけでなく、ここで紹介した製品に関するご意見やご要望もお待ちしております。 お寄せいただいたご意見は、今後の改善に役立たせていただきます。 特に、メンバーシップ データベースの構成とデプロイの自動化に関するご意見をお待ちしております。 

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- bookmarks -->
[Add an OAuth Provider]: #addOauth
[Add Roles to the Membership Database]:#mbrDB
[Create a Data Deployment Script]:#ppd
[Update the Membership Database]:#ppd2
[setupdbenv]: #bkmk_setupdevenv
[setupwindowsazureenv]: #bkmk_setupwindowsazure
[createapplication]: #bkmk_createmvc4app
[deployapp1]: #bkmk_deploytowindowsazure1
[adddb]: #bkmk_addadatabase
[addcontroller]: #bkmk_addcontroller
[addwebapi]: #bkmk_addwebapi
[deploy2]: #bkmk_deploydatabaseupdate

<!-- links -->
[EFCodeFirstMVCTutorial]: http://www.asp.net/mvc/tutorials/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
[dbcontext-link]: http://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx


<!-- images-->
[rxE]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxE.png
[rxP]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxP.png
[rx22]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/
[rxb2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxb2.png
[rxz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz.png
[rxzz]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxzz.png
[rxz2]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz2.png
[rxz3]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz3.png
[rxStyle]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxStyle.png
[rxz4]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz4.png
[rxz44]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxz44.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxPrevDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPrevDB.png
[rxOverwrite]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxOverwrite.png
[rxPWS]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxPWS.png
[rxNewCtx]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxNewCtx.png
[rxAddApiController]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxAddApiController.png
[rxFFchrome]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxFFchrome.png
[intro001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobil-intro-finished-web-app.png
[rxCreateWSwithDB]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/rxCreateWSwithDB.png
[setup007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-setup-azure-site-004.png
[setup009]: ../Media/dntutmobile-setup-azure-site-006.png
[newapp002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-002.png
[newapp004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-createapp-004.png
[firsdeploy007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-005.png
[firsdeploy009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-deploy1-publish-007.png
[adddb001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-001.png
[adddb002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-adddatabase-002.png
[addcode001]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-context-menu.png
[addcode002]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-controller-dialog.png
[addcode004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-index-context.png
[addcode005]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-add-contents-context-menu.png
[addcode007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-controller-modify-bundleconfig-context.png
[addcode008]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-menu.png
[addcode009]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-migrations-package-manager-console.png
[addwebapi004]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-added-contact.png
[addwebapi006]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-save-returned-contacts.png
[addwebapi007]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/dntutmobile-webapi-contacts-in-notepad.png
[Add XSRF Protection]: #xsrf
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[Add XSRF Protection]: #xsrf
[ImportPublishSettings]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishSettings.png
[ImportPublishProfile]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ImportPublishProfile.png
[PublishVSSolution]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/PublishVSSolution.png
[ValidateConnection]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/ValidateConnection.png
[WebPIAzureSdk20NetVS12]: ./media/web-sites-dotnet-rest-service-aspnet-api-sql-database/WebPIAzureSdk20NetVS12.png
[prevent-csrf-attacks]: http://www.asp.net/web-api/overview/security/preventing-cross-site-request-forgery-(csrf)-attacks
 

