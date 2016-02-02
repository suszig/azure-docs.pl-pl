<properties 
    pageTitle="Azure Active Directory の認証を使用して Azure App Service で .NET MVC Web アプリを作成する" 
    description="Azure Active Directory の認証を使用して Azure App Service に ASP.NET MVC 基幹業務アプリケーションを作成する方法について説明します。" 
    services="app-service\web, active-directory" 
    documentationCenter=".net" 
    authors="cephalin" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="web" 
    ms.date="12/10/2015" 
    ms.author="cephalin"/>


# Azure Active Directory の認証を使用して Azure App Service で .NET MVC Web アプリを作成する

この記事では、ASP.NET MVC 基幹業務アプリケーションを作成する方法を説明します [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) を使用して [Azure Active Directory](/services/active-directory/) id プロバイダーとして。 使用する方法も学習、 [Azure Active Directory Graph Client Library](http://blogs.msdn.com/b/aadgraphteam/archive/2014/06/02/azure-active-directory-graph-client-library-1-0-publish.aspx) アプリケーションでディレクトリ データを照会します。

使用する Azure Active Directory テナントを Azure 専用ディレクトリにするか、オンプレミスの Active Directory (AD) とディレクトリを同期して、オンプレミス ユーザーやリモート ユーザーがシングル サインオン環境を構築できます。
>[AZURE.NOTE] Azure App Service Web Apps の場合、ボタンを数回クリックするだけで、Azure Active Directory に対する認証を構成できます。 詳細については、次を参照してください。 [Azure App Service での認証に Active Directory を使用して](web-sites-authentication-authorization.md)します。

<a name="bkmk_build"></a>
## 学習内容

次の機能を備え、作業項目を追跡する、単純な LOB の "作成、読み取り、更新、削除" (CRUD) アプリケーションを App Service Web Apps に作成します。

- Azure Active Directory に対してユーザーを認証する
- サインインとサインアウト機能を実装する
- 使用して `[Authorize]` CRUD 操作ごとにユーザーを承認するには
- を使用してデータを Azure Active Directory に照会 [Azure Active Directory Graph API](http://msdn.microsoft.com/library/azure/hh974476.aspx)
- 使用して [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (代わりに Windows Identity Foundation (WIF)、これは ASP.NET の進化と簡単に認証と WIF よりも承認設定

<a name="bkmk_need"></a>
## 前提条件

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

このチュートリアルを完了するには、以下が必要です。

- 各グループのユーザーが属する Azure Active Directory テナント
- Azure Active Directory テナントでアプリケーションを作成するためのアクセス許可
- Visual Studio 2013 以降
- [Azure SDK 2.8.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) またはそれ以降

<a name="bkmk_sample"></a>
## サンプル アプリケーションを基幹業務テンプレートとして使用する

このチュートリアルでは、サンプル アプリケーション [WebApp RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims), 、Azure Active Directory チームによって作成され、簡単に新しい基幹業務アプリケーションを作成するテンプレートとして使用できます。 以下の機能が組み込まれています。

- 使用して [OpenID Connect](http://openid.net/connect/) Azure Active Directory で認証するには
- サンプルが含まれます `TaskTracker` コント ローラーの標準的な使用も含めて、アプリケーションで特定の操作に対してさまざまなロールを承認する方法を示す `[Authorize]`します。
- ユーザーとグループを即座に割り当てることができる、定義済みロールを持つマルチテナント アプリケーションです。

<a name="bkmk_run" />
## サンプル アプリケーションを実行する

1.  サンプル ソリューションに複製またはダウンロード [WebApp RoleClaims-DotNet](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims) をローカル ディレクトリ。

2.  」の手順に従って [方法を実行サンプルの 1 つのテナント アプリとして](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims#how-to-run-the-sample-as-a-single-tenant-app) Azure Active Directory アプリケーションおよびプロジェクトを設定します。
アプリケーションをマルチテナントからシングル テナントに変換するには、必ずすべての手順に従ってください。

3.  [Azure クラシック ポータル](https://manage.windowsazure.com) 作成した Azure Active Directory アプリケーションの表示] をクリックして、 **ユーザー** ] タブをクリックします。 次に、必要なロールに目的のユーザーを割り当てます。
    >[AZURE.NOTE] ユーザーだけでなくグループにロールを割り当てる場合は、Azure Active Directory テナントをアップグレードする必要があります [Azure Active Directory Premium](/pricing/details/active-directory/)します。 アプリケーションの旧ポータル UI を表示する場合は、 **ユーザー** タブの代わりに、 **ユーザーとグループ] タブで、Azure Active Directory テナントの Azure Active Directory Premium を試行できます * * ライセンス** ] タブをクリックします。 

3.  アプリケーションの構成が完了したら、 `f5 キーを押して` で Visual Studio で ASP.NET アプリケーションを実行します。

4.  アプリケーションが読み込まれたら、**[サインイン]** をクリックし、Azure クラシック ポータルで管理者ロールを持つユーザーでサインインします。

5.  Azure Active Directory アプリケーションを適切に構成し、Web.config で対応する設定項目を設定している場合は、ログイン ページにリダイレクトされます。 Azure クラシック ポータルで Azure Active Directory アプリケーションを作成するために使用したアカウントは Azure Active Directory アプリケーションの既定の所有者であるため、そのアカウントでログオンします。

<a name="bkmk_deploy"></a>
## サンプル アプリケーションを Azure App Service Web Apps にデプロイする

ここでは、アプリケーションを Azure App Service の Web アプリに発行します。 」の手順がまだ存在 [README.md](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims/blob/master/README.md) App Service Web Apps では、これらの手順にも展開するためのローカル デバッグ環境の構成を無効です。 デバッグ構成を維持したままデプロイする方法について説明します。

1. プロジェクトを右クリックし、**[発行]** を選択します。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/publish-app.png)

2. **[Microsoft Azure Web Apps]** を選択します。

3. Azure にサインインしていない場合は、**[アカウントの追加]** をクリックし、Azure サブスクリプションの Microsoft アカウントを使用してサインインします。

4. サインインしたら、**[新規]** をクリックして Azure で新しい Web アプリを作成します。

5. **[ホスティング]** にて、すべての必須フィールドに必要事項を入力します。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-create-website.png)

5. このアプリケーションでロール マッピング、キャッシュされたトークン、すべてのアプリケーション データを格納するには、データベース接続が必要です。  **[App Service の作成]** ダイアログ ボックスで、**[サービス]** をクリックします。 **[SQL Database]** の隣にあるプラス記号をクリックして新しいデータベースを追加します。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/4-create-database.png)

5. **[SQL Database の構成]** ダイアログ ボックスで、サーバーを選択または作成して名前を設定し、**[OK]** をクリックします。

     ![](./media/web-sites-dotnet-lob-application-azure-ad/4-config-database.png)

6. **[作成]** をクリックします。 Web アプリが作成されると、**[Web の発行]** ダイアログが開きます。

7. **[宛先 URL]** で、**http** を **https** に変更します。 URL 全体をテキスト エディターにコピーします。 この URL は後で使用します。 次に、**[次へ]** をクリックします。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/5-change-to-https.png)

8. **[組織認証を有効にする]** チェック ボックスをオフにします。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/6-enable-code-first-migrations.png)

8. **[RoleClaimContext]** を展開し、**[Code First Migrations の実行 (アプリケーションの起動時に実行)]** チェック ボックスをオンにします。 [Code First Migrations](https://msdn.microsoft.com/data/jj591621.aspx) 後でその他の Code First データ モデルを定義するときに Azure でのアプリのデータベース スキーマの更新を支援します。

9. **[発行]** をクリックして Web への発行に進む代わりに、**[閉じる]** をクリックします。 **[はい]** をクリックし、発行プロファイルへの変更を保存します。

2. [Azure クラシック ポータル](https://manage.windowsazure.com), 、Azure Active Directory テナントに移動し、クリックして、 **アプリケーション** ] タブをクリックします。

2. ページの下部にある **[追加]** をクリックします。

2. **[組織で開発中のアプリケーションを追加]** をクリックします。

3. **[Web アプリケーションや Web API]** を選択します。

4. アプリケーションに名前を付けて、**[次へ]** をクリックします。

5. アプリケーションのプロパティを設定 **サインオン URL** 前の手順で保存した web アプリの URL を (例: `https://<site-name>.azurewebsites.net/`)、および **APP ID URI** に `https://<aad-tenanet-name>/<app-name>`します。次に、**[完了]** をクリックします。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/7-app-properties.png)

2.  更新プログラム、アプリケーション マニフェストに記載された手順から前の手順を行ったのと同じ方法で、アプリケーションが作成されると、 [アプリケーション ロールを定義](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims#step-2-define-your-application-roles)します。

3.  [Azure クラシック ポータル](https://manage.windowsazure.com) 作成した Azure Active Directory アプリケーションの表示] をクリックして、 **ユーザー** ] タブをクリックします。 次に、必要なロールに目的のユーザーを割り当てます。

6. **[構成]** タブをクリックします。

7. **[キー]** で、ドロップダウンから **[1 年]** を選択して新しいキーを作成します。

8. **[他のアプリケーションに対するアクセス許可]** の **Azure Active Directory** エントリで、**[デリゲートされたアクセス許可]** ボックスの一覧から **[サインインとユーザー プロファイルの読み取り]** と **[ディレクトリ データの読み取り]** を選択します。
    > [AZURE.NOTE] ここで実際に必要となるアクセス許可は、アプリケーションに含める機能によって異なります。 中には**全体管理者**ロールを設定する必要があるアクセス許可もありますが、このチュートリアルで必要となるのは**ユーザー** ロールのみです。

9.  **[保存]** をクリックします。

10.  保存された構成のページから移動する前に以下の情報をテキスト エディターにコピーします。

    -   Client ID
    -   Key (if you navigate away from the page, you will not be able to see the key again)

11. Visual Studio で、プロジェクトの **Web.Release.config** を開きます。次の XML を挿入、 `< configuration >` タグ、および各キーの値を新しい Azure Active Directory アプリケーションに保存されている情報に置き換えます。
<pre class="prettyprint">
                  
&lt;appSettings&gt;
  < キーを追加"ida: ClientId"の値を = ="
                  <mark>[例: 82692da5-a86f-44c9-9d53-2f88d52b478b]</mark>
                  "xdt:Transform ="SetAttributes"xdt:Locator="Match(key)"/>
  < キーを追加"ida: AppKey"の値を = ="
                  <mark>[に対する rZJJ9bHSi/cYnYwmQFxLYDn/6EfnrnIfKoNzv9NKgbo の例: =]</mark>
                  "xdt:Transform ="SetAttributes"xdt:Locator="Match(key)"/>
  < キーを追加"Ida:postlogoutredirecturi"の値を = ="
                  <mark>[例: https://mylobapp.azurewebsites.net/]</mark>
                  "xdt:Transform ="SetAttributes"xdt:Locator="Match(key)"/>
</appSettings>
                </pre>

ida:PostLogoutRedirectUri の値の末尾が "/" になっていることを確認します。

1. プロジェクトを右クリックし、**[発行]** を選択します。

2. **[発行]** をクリックして Azure App Service Web Apps に発行します。

上記の手順が完了すると、Azure クラシック ポータルに 2 つの Azure Active Directory アプリケーションが構成されます。1 つは Visual Studio のデバッグ環境用のアプリケーション、もう 1 つは Azure に発行された Web アプリ用のアプリケーションです。 デバッグ中は、**Debug** 構成を Azure Active Directory に対応させるために Web.config のアプリ設定が使用されます。発行時 (既定では、**Release** 構成が発行されたとき) は、Web.Release.config のアプリ設定の変更が組み込まれた、変換された Web.config がアップロードされます。

発行した Web アプリをデバッガーにアタッチする場合 (発行した Web アプリのコードのデバッグ シンボルをアップロードする必要があります)、Azure デバッグの Debug 構成のクローンを作成できます。このとき、Web.Release.config の Azure Active Directory 設定を使用するカスタム Web.config 変換 (たとえば、Web.AzureDebug.config) が使用されます。 これにより、静的構成を異なる環境間で維持できます。

<a name="bkmk_crud"></a>
## 基幹業務の機能をサンプル アプリケーションに追加する

チュートリアルのこのセクションでは、サンプル アプリケーションを利用して、目的の基幹業務の機能を構築する方法について説明します。 単純な CRUD 作業項目トラッカーを作成します。これは、TaskTracker コントローラーに似ていますが、使うのは CRUD の標準的なスキャフォールディングと設計パターンです。 また、そこに含まれている Scripts\AadPickerLibrary.js を使い、Azure Active Directory Graph API からのデータでアプリケーションを強化します。

5.  Models フォルダーに作成、新しい [Code First](http://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application) と呼ばれる WorkItem.cs をモデル化し、コードを次のコードに置き換えます。

    using System.ComponentModel.DataAnnotations;
    
    namespace WebApp_RoleClaims_DotNet.Models
    {
        public class WorkItem
        {
            [Key]
            public int ItemID { get; set; }
            public string AssignedToID { get; set; }
            public string AssignedToName { get; set; }
            public string Description { get; set; }
            public WorkItemStatus Status { get; set; }
        }
    
        public enum WorkItemStatus
        {
            Open, 
            Investigating, 
            Resolved, 
            Closed
        }
    }

6.  DAL\RoleClaimContext.cs を開き、次の強調表示されたコードを追加します。
<pre class="prettyprint">
                  
public class RoleClaimContext : DbContext
{
   public RoleClaimContext() : base("RoleClaimContext") { }
   public DbSet&lt;Task&gt; Tasks { get; set; }
    
                  <mark>public DbSet&lt;WorkItem&gt; WorkItems { get; set; }</mark>
                  
    public DbSet&lt;TokenCacheEntry&gt; TokenCacheEntries { get; set; }
}
                </pre>

7.  プロジェクトをビルドして、新しいモデルから Visual Studio のスキャフォールディング ロジックにアクセスできるようにします。

8.  新しくスキャフォールディングした項目の追加 `WorkItemsController` コント ローラーのフォルダーにします。 そのためには、**[コントローラー]** を右クリックし、**[追加]** をポイントしてから、**[新しくスキャフォールディングした項目]** を選択します。

9.  **[Entity Framework を使用したビュー付きの MVC 5 コントローラー]** を選択し、**[追加]** をクリックします。

10. 作成したモデルを選択し、**[追加]** をクリックします。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/8-add-scaffolded-controller.png)

9.  Controllers\WorkItemsController.cs を開きます。

11. 強調表示された [Authorize] 装飾を以下の各操作に追加します。
<pre class="prettyprint">
                  
...
                  <mark>[Authorize(Roles = "Admin, Observer, Writer, Approver")]</mark>
                  
public class WorkItemsController : Controller
{
   ...
    
                  <mark>[Authorize(Roles = "Admin, Writer")]</mark>
                  
    public ActionResult Create()
   ...
    
                  <mark>[Authorize(Roles = "Admin, Writer")]</mark>
                  
    public async Task&lt;ActionResult&gt; Create([Bind(Include = "ItemID,AssignedToID,AssignedToName,Description,Status")] WorkItem workItem)
   ...
    
                  <mark>[Authorize(Roles = "Admin, Writer")]</mark>
                  
    public async Task&lt;ActionResult&gt; Edit(int? id)
   ...
    
                  <mark>[Authorize(Roles = "Admin, Writer")]</mark>
                  
    public async Task&lt;ActionResult&gt; Edit([Bind(Include = "ItemID,AssignedToID,AssignedToName,Description,Status")] WorkItem workItem)
   ...
    
                  <mark>[Authorize(Roles = "Admin, Writer, Approver")]</mark>
                  
    public async Task&lt;ActionResult&gt; Delete(int? id)
   ...
    
                  <mark>[Authorize(Roles = "Admin, Writer, Approver")]</mark>
                  
    public async Task&lt;ActionResult&gt; DeleteConfirmed(int id)
   ...
}
                </pre>

ロール マッピングの処理は Azure クラシック ポータルの UI で行うため、必要となるのは、各操作で正しいロールが承認されているかどうかを確認することだけです。
> [AZURE.NOTE] お気付き、 <code>[ValidateAntiForgeryToken]</code> 装飾一部の操作を使用します。 説明されている動作により [Brock Allen](https://twitter.com/BrockLAllen) で [MVC 4、AntiForgeryToken とクレーム](http://brockallen.com/2012/07/08/mvc-4-antiforgerytoken-and-claims/) ため、HTTP POST が偽造防止トークン検証失敗する可能性があります。
> + Azure Active Directory は、既定で偽造防止トークンに必要な http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider を送信しません。
> + Azure Active Directory が AD FS と同期したディレクトリの場合、AD FS トラストは既定では要求を送信しない http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider するか、このクレームを送信する AD FS を手動で構成できますが。
> これについては次の手順で対処します。

12.  App_Start\Startup.Auth.cs で、次のコードの行を追加、 `ConfigureAuth` メソッドです。 各名前付け解決エラーを右クリックすると、そのエラーが修正されます。

        AntiForgeryConfig.UniqueClaimTypeIdentifier = ClaimTypes.NameIdentifier です。

    `ClaimTypes.NameIdentifies` specifies the claim `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`, which Azure Active Directory does supply. Now that you have taken care of the authorization part (seriously, that didn't take long), you can devote your time to the actual functionality of the actions. 

13. Create() と Edit() に次のコードを追加すると、後で JavaScript でいくつかの変数を使用できるようにします。 各名前付け解決エラーを右クリックすると、そのエラーが修正されます。

        ViewData["token"] = AcquireToken(ClaimsPrincipal.Current.FindFirst(Globals.ObjectIdClaimType).Value);
        ViewData["tenant"] = ConfigHelper.Tenant;

13. `AcquireToken()` メソッドがまだ定義されていませんが、そのための定義、 `WorkItemsController` クラスのようになりました。 各名前付け解決エラーを右クリックすると、そのエラーが修正されます。

        static string AcquireToken(string userObjectId)
        {
            ClientCredential cred = new ClientCredential(ConfigHelper.ClientId, ConfigHelper.AppKey);
            Claim tenantIdClaim = ClaimsPrincipal.Current.FindFirst(Globals.TenantIdClaimType);
            AuthenticationContext authContext = new AuthenticationContext(String.Format(CultureInfo.InvariantCulture, ConfigHelper.AadInstance, tenantIdClaim.Value), new TokenDbCache(userObjectId));
            AuthenticationResult result = authContext.AcquireTokenSilent(ConfigHelper.GraphResourceId, cred, new UserIdentifier(userObjectId, UserIdentifierType.UniqueId));
            return result.AccessToken;
        }

14. Views\WorkItems\Create.cshtml (自動的にスキャフォールディングされた項目) での検索、 `Html.BeginForm` ヘルパー メソッドし、次のように変更します。
<pre class="prettyprint">
                  @ (Html.BeginForm (を使用します。
                  <mark>「作成」、「作業項目」FormMethod.Post、新しい {id =「メイン フォーム」}</mark>
                  ))
{
   @Html.AntiForgeryToken()
   <div class="form-horizontal">
       <h4>WorkItem</h4>
       <hr />
       @Html.ValidationSummary(true, "", new { @class = "text-danger" })
       <div class="form-group">
           <div class="col-md-10">
               @Html.EditorFor (モデル = > モデルです。新しい AssignedToID {htmlAttributes = new {@class =「コントロール」
                  <mark>、@type =「隠れた」</mark>
                   } })
               @Html.ValidationMessageFor(model => model.AssignedToID, "", new { @class = "text-danger" })
           </div>
       </div>
       <div class="form-group">
           @Html.LabelFor(model => model.AssignedToName, htmlAttributes: new { @class = "control-label col-md-2" })
           <div class="col-md-10">
               @Html.EditorFor(model => model.AssignedToName, new { htmlAttributes = new { @class = "form-control" } })
               @Html.ValidationMessageFor(model => model.AssignedToName, "", new { @class = "text-danger" })
           </div>
       </div>
       <div class="form-group">
           @Html.LabelFor(model => model.Description, htmlAttributes: new { @class = "control-label col-md-2" })
           <div class="col-md-10">
               @Html.EditorFor(model => model.Description, new { htmlAttributes = new { @class = "form-control" } })
               @Html.ValidationMessageFor(model => model.Description, "", new { @class = "text-danger" })
           </div>
       </div>
       <div class="form-group">
           @Html.LabelFor(model => model.Status, htmlAttributes: new { @class = "control-label col-md-2" })
           <div class="col-md-10">
               @Html.EnumDropDownListFor(model => model.Status, htmlAttributes: new { @class = "form-control" })
               @Html.ValidationMessageFor(model => model.Status, "", new { @class = "text-danger" })
           </div>
       </div>
       <div class="form-group">
           <div class="col-md-offset-2 col-md-10">
               < 入力の型 =「送信」値"Create"クラスの = ="btn btn default" 
                  <mark>id =「submit ボタン」</mark>
                   />
           </div>
       </div>
   </div>
    
                  <mark>< スクリプト >
           // People/Group Picker Code
           var maxResultsPerPage = 14;
           var input = document.getElementById("AssignedToName");
           var token = "@ViewData["token"]";
           var tenant = "@ViewData["tenant"]";
           var picker = new AadPicker(maxResultsPerPage, input, token, tenant);
           // Submit the selected user/group to be asssigned.
           $("#submit-button").click({ picker: picker }, function () {
               if (!picker.Selected())
                   return;
               $("#main-form").get()[0].elements["AssignedToID"].value = picker.Selected().objectId;
           });
   </script></mark>
                  
{
                </pre>

スクリプトでは、AadPicker オブジェクトを呼び出す [Azure Active Directory Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) 、入力に一致するユーザーとグループを検索します。

15. 開いている、 [パッケージ マネージャー コンソール](http://docs.nuget.org/Consume/Package-Manager-Console) 実行 **Enable-migrations – EnableAutomaticMigrations**します。 Azure にアプリケーションを発行したときに選択したオプションと同様に、このコマンドによりで、アプリのデータベース スキーマを更新 [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx) Visual Studio でデバッグする場合。

15. ここで、Visual Studio デバッガーでアプリを実行するか、Azure App Service Web Apps に再発行します。アプリケーションの所有者としてログインしに移動 `https://&lt;webappname&gt;.azurewebsites.net/WorkItems/Create`します。これで、ドロップダウン リストから Azure Active Directory のユーザーまたはグループを選択するか、一覧をフィルター処理するためのデータを入力できます。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/9-create-workitem.png)

16. フォームの残りの部分を設定し、**[作成]** をクリックします。 これで、~/WorkItems/Index ページに新しく作成した作業項目が表示されます。 注目次のスクリーン ショットでは削除された、 `AssignedToID` Views\WorkItems\Index.cshtml 内の列です。

    ![](./media/web-sites-dotnet-lob-application-azure-ad/10-workitem-index.png)

11. 次に、**[編集]** ビューにも同じような変更を加えます。 Views\WorkItems\Edit.cshtml では変更する、 `Html.BeginForm` ヘルパー メソッドは、前の手順で Views\WorkItems\Create.cshtml のものと同じです (前の強調表示されたコードを"Edit"には、"Create"を置き換えます)。

これで完了です。

承認とさまざまな操作の基幹業務機能を WorkItems コントローラーに構成できましたので、さまざまなアプリケーション ロールのユーザーとしてログインし、アプリケーションの応答を確認してみてください。

![](./media/web-sites-dotnet-lob-application-azure-ad/11-edit-unauthorized.png)

<a name="bkmk_resources"></a>
## 他の関連リソース

- [SSL と Authorize 属性を使用してアプリケーションを保護します。](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md#protect-the-application-with-ssl-and-the-authorize-attribute)
- [Active Directory を Azure App Service での認証の使用します。](web-sites-authentication-authorization.md)
- [AD FS の認証に Azure App Service で .NET MVC web アプリを作成します。](web-sites-dotnet-lob-application-adfs.md)
- [Microsoft Azure Active Directory のサンプルとドキュメント](https://github.com/AzureADSamples)
- [Vittorio Bertocci のブログ](http://blogs.msdn.com/b/vbertocci/)
- [VS2013 Web プロジェクトを WIF から Katana に移行します。](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
- [Azure の新しいハイブリッド接続いない父親の #hybridCloud](/documentation/videos/new-hybrid-connections-not-your-fathers-hybridcloud/)
- [Active Directory と Azure Active Directory の類似点](http://technet.microsoft.com/library/dn518177.aspx)
- [ディレクトリ同期とシングル サインオンのシナリオ](http://technet.microsoft.com/library/dn441213.aspx)
- [Azure Active Directory には、トークンとクレームの種類がサポートされています。](http://msdn.microsoft.com/library/azure/dn195587.aspx)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]






