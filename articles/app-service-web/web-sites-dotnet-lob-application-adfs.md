<properties 
    pageTitle="AD FS の認証を使用して Azure App Service で .NET MVC Web アプリを作成する" 
    description="Azure App Service Web アプリを使用してオンプレミス STS で認証を行う ASP.NET MVC 基幹業務アプリケーションを作成する方法について説明します。 このチュートリアルでは、オンプレミス STS としての AD FS を対象にしています。" 
    services="app-service\web" 
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
    ms.date="12/15/2015" 
    ms.author="cephalin"/>

# AD FS の認証を使用して Azure App Service で .NET MVC Web アプリを作成する

この記事では、ASP.NET MVC 基幹業務アプリケーションを作成する方法を説明します [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) 内部設置型を使用して [Active Directory フェデレーション サービス](http://technet.microsoft.com/library/hh831502.aspx) id プロバイダーとして。 このシナリオは、すべてのデータがオンサイトに格納されることが求められる組織で Azure App Service Web アプリを使用して基幹業務アプリケーションを作成する場合に適しています。

>[AZURE.NOTE] Azure App Service Web Apps の別のエンタープライズ認証や承認オプションの概要については、次を参照してください。 [Azure App Service での認証に Active Directory を使用して](web-sites-authentication-authorization.md)します。

<a name="bkmk_build"></a>
## 学習内容 ##

Azure App Service Web アプリで次の機能を持つ基本的な ASP.NET アプリケーションを作成します。

- AD FS に対してユーザーの認証を行う。
- `[Authorize]` を使用して、操作ごとにユーザーを承認する。
- Visual Studio でのデバッグと App Service Web アプリへの発行の両方に対応する静的構成を使用する (1 回構成するだけで、いつでもデバッグおよび発行できる)。  

<a name="bkmk_need"></a>
## 前提条件 ##

[AZURE.INCLUDE [free-trial-note](../../includes/free-trial-note.md)]

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

このチュートリアルを完了するには、以下が必要です。

- 内部設置型 AD FS デプロイメント (使用するテスト ラボのエンド ツー エンド チュートリアルでは、次を参照してください [テスト ラボ: スタンドアロン STS (テストのみ) 用の Azure VM での AD FS に関する](TODO))。
- 証明書利用者信頼を AD FS 管理で作成するためのアクセス許可
- Visual Studio 2013
- [Azure SDK 2.5.1](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) またはそれ以降

<a name="bkmk_sample"></a>
## サンプル アプリケーションを基幹業務テンプレートとして使用する ##

このチュートリアルでは、サンプル アプリケーション [WebApp WSFederation-DotNet)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet), 、Azure Active Directory チームが作成されます。 AD FS は WS-Federation をサポートするため、これをテンプレートとして使用すると、簡単に新しい基幹業務アプリケーションを作成できます。 このアプリケーションには、次の機能があります。

- 使用して [Ws-federation](http://msdn.microsoft.com/library/bb498017.aspx) 内部設置型の認証に AD FS の展開
- サインインとサインアウト機能
- 使用して [Microsoft.Owin](http://www.asp.net/aspnet/overview/owin-and-katana/an-overview-of-project-katana) (代わりに Windows Identity Foundation (WIF)、これは ASP.NET の進化と簡単に認証と WIF よりも承認設定

<a name="bkmk_setup"></a>
## サンプル アプリケーションを設定する ##

2.  サンプル ソリューションに複製またはダウンロード [WebApp WSFederation-DotNet](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) をローカル ディレクトリ。

    > [AZURE.NOTE] 」の手順に [README.md](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet/blob/master/README.md) を Azure Active Directory にアプリケーションを設定する方法を説明します、と AD FS を設定はこのチュートリアルではそのため、手順に従ってここで代わりにします。

3.  ソリューションを開きで controllers \accountcontroller.cs を開きます、 **ソリューション エクスプ ローラー**します。

    このコードでは、WS-Federation を使用してユーザーを認証するために、単に認証チャレンジを発行しています。 認証はすべて App_Start\Startup.Auth.cs を使用して構成します。

4.  App_Start\Startup.Auth.cs を開きます。 `ConfigureAuth` メソッドには、次の行が含まれています。

        app.UseWsFederationAuthentication(
            new WsFederationAuthenticationOptions
            {
                Wtrealm = realm,
                MetadataAddress = metadata                                      
            });

    OWIN では、これが WS-Federation 認証を構成するのに必要な最小限のコードです。 これは、Web.config のいろいろな場所に XML が挿入される WIF よりもはるかに簡単で洗練されています。 必要な情報は、証明書利用者 (RP) の識別子と AD FS サービスのメタデータ ファイルの URL のみです。 次に例を示します。

    -   RP 識別子: `https://contoso.com/MyLOBApp`
    -   メタデータのアドレス: `http://adfs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`

5.  App_Start\Startup.Auth.cs で、静的文字列定義を次のように変更します。強調表示されている部分が変更箇所です。  
    <pre class="prettyprint">
    プライベートの静的文字列レルム = ConfigurationManager.AppSettings["ida:<mark>RPIdentifier</mark>"];
    <mark><del>プライベートの静的文字列 aadInstance ConfigurationManager.AppSettings["ida:AADInstance ="];</del></mark>
    <mark><del>プライベートの静的文字列テナント ConfigurationManager.AppSettings["ida:Tenant ="];</del></mark>
    <mark><del>プライベートの静的文字列メタデータ = 文字列。書式設定 ("{0}/\\ {1 \}/federationmetadata/2007-06/federationmetadata.xml"、aadInstance、テナント)。</del></mark>
    <mark>private static string metadata = string.Format("https://{0}/federationmetadata/2007-06/federationmetadata.xml", ConfigurationManager.AppSettings["ida:ADFS"]);</mark>

    <mark><del>文字列の機関 = String.Format (CultureInfo.InvariantCulture、aadInstance、テナント)。</del></mark>
    </pre>

6.  次に、Web.config に対応する変更を加えます。 Web.config を開き、アプリ設定を次のように変更します。強調表示されている部分が変更箇所です。  
    <pre class="prettyprint">
    & lt; appSettings & gt;
      & lt; キーを追加"webpages:Version"の値を = =「3.0.0.0」/& gt;
      & lt; キーを追加 ="web ページ: 有効になっている"値 ="false"/& gt;
      & lt; キーを追加"ClientValidationEnabled"の値を = ="true"/& gt;
      & lt; キーを追加"UnobtrusiveJavaScriptEnabled"の値を = ="true"/& gt;
      <mark><del>& lt; キーを追加 ="ida: Wtrealm"value =「[WebApp WSFederation-DotNet https://contoso.onmicrosoft.com/WebApp-WSFederation-DotNet のアプリ ID URI の入力]」/& gt;</del></mark>
      <mark><del>& lt; キーを追加 ="ida: AADInstance"値"https://login.windows.net"=/& gt;</del></mark>
      <mark><del>& lt; キーを追加「ida: テナント」の値を = =「[テナント名の入力、contoso.onmicrosoft.com]」/& gt;</del></mark>
      <mark>& lt; キーを追加"ida: RPIdentifier"の値を = ="[https://localhost:44320 など、AD FS で構成されたパーティの証明書利用者の識別子を入力/]"/& gt;</mark>
      <mark>& lt; キーを追加"ida: ADFS"の値を = =「[adfs.contoso.com など、AD FS サービスの FQDN の入力]」/& gt;</mark>

    & lt;/appSettings & gt;
    </pre>

    それぞれの環境に合わせてキーの値を入力してください。

7.  アプリケーションをビルドしてエラーがないことを確認します。

これで終了です。 これで、サンプル アプリケーションで AD FS を使用する準備が整いました。 ただし、後で AD FS のこのアプリケーションを使用して RP 信頼を構成する作業が残っています。

<a name="bkmk_deploy"></a>
## サンプル アプリケーションを Azure App Service Web アプリに展開する

ここでは、デバッグ環境を維持しながら App Service Web アプリで Web アプリにアプリケーションを発行します。 AD FS での RP 信頼を設定する前にアプリケーションを発行するため、この段階では認証はまだ機能しません。 ただし、この操作を行うと、後で RP 信頼を構成するために使用する Web アプリの URL を確認できます。

1. プロジェクトを右クリックして **発行**します。

    ![](./media/web-sites-dotnet-lob-application-adfs/01-publish-website.png)

2. 選択 **Microsoft Azure App Service**します。
3. Azure にサインインしていない場合はクリックして **サイン イン** し、Azure サブスクリプションの Microsoft アカウントを使用してサインインします。
4. サインインしたら **新規** 新しい web アプリを作成します。
5. すべての必須フィールドに必要事項を入力します。 後でオンプレミス データに接続するため、この Web アプリ用のデータベースは作成しません。

    ![](./media/web-sites-dotnet-lob-application-adfs/02-create-website.png)

6. クリックして **作成**します。 Web アプリが作成されると、[Web の発行] ダイアログが開きます。
7.  **送信先 URL**, 、変更 **http** に **https**します。 URL 全体をテキスト エディターにコピーします。 この URL は後で使用します。 クリックして **発行**します。

    ![](./media/web-sites-dotnet-lob-application-adfs/03-destination-url.png)

11. Visual Studio で開きます **Web.Release.config** プロジェクトにします。 次の XML を `<configuration>` タグに挿入し、発行した Web アプリの URL でキー値を置き換えます。  
    <pre class="prettyprint">
& lt; appSettings & gt;
   & lt; キーを追加"ida: RPIdentifier"の値を = ="<mark>[例: https://mylobapp.azurewebsites.net/]</mark>"xdt:Transform ="SetAttributes"xdt:Locator="Match(key)"/& gt;
& lt;/appSettings & gt;</pre>

この作業を完了すると、Visual Studio のデバッグ環境用と Azure に発行した Web アプリ用の 2 つの RP 識別子がプロジェクトに構成されたことになります。 この 2 つの環境のそれぞれに対して AD FS に RP 信頼を設定します。 デバッグ中は、Web.config のアプリ設定を使用して **デバッグ** 構成作業と AD FS と発行されている場合 (既定では、 **リリース** 構成が発行)、Web.Release.config のアプリ設定の変更が組み込まれた、変換された Web.config がアップロードします。

Azure で発行した Web アプリをデバッガーにアタッチする場合 (つまり、発行した Web アプリのコードのデバッグ シンボルをアップロードする必要があります)、Azure デバッグの Debug 構成のクローンを作成できます。このとき、Web.Release.config のアプリ設定を使用するカスタム Web.config 変換 (たとえば、Web.AzureDebug.config) が使用されます。 これにより、静的構成を異なる環境間で維持できます。

<a name="bkmk_rptrusts"></a>
## AD FS 管理で証明書利用者信頼を構成する ##

AD FS を使用してサンプル アプリケーションの認証を実際に行う前に、AD FS 管理で RP 信頼を構成する必要があります。 デバッグ環境用と発行した Web アプリ用の 2 つの別個の RP 信頼を設定する必要があります。

> [AZURE.NOTE] 両方の環境の次の手順を繰り返すことを確認します。

4.  AD FS サーバーで、AD FS の管理権限を持つ資格情報でログインします。
5.  AD FS 管理を開きます。 右クリック **AD FS\Trusted \ 信頼** 選択 **証明書利用者信頼の追加**します。

    ![](./media/web-sites-dotnet-lob-application-adfs/1-add-rptrust.png)

5.   **データ ソースの選択** ] ページで選択 **証明書利用者に関するデータを手動で入力**します。 

    ![](./media/web-sites-dotnet-lob-application-adfs/2-enter-rp-manually.png)

6.   **表示名の指定** ページで、アプリケーションの表示名を入力し、クリックして **次**します。
7.   **プロトコルの選択** ] ページで [ **次**します。
8.   **証明書の構成** ] ページで [ **次**します。

    > [AZURE.NOTE] HTTPS を既に使用する場合は、以降の暗号化されたトークンはオプションです。 このページで AD FS からのトークンの暗号化を設定する場合は、トークンの暗号化解除ロジックをコードに追加する必要があります。 詳細については、次を参照してください。 [OWIN Ws-federation ミドルウェア構成を受け入れるのトークンを暗号化する手動で](http://chris.59north.com/post/2014/08/21/Manually-configuring-OWIN-WS-Federation-middleware-and-accepting-encrypted-tokens.aspx)します。
  
5.  次の手順に進む前に、Visual Studio プロジェクトの情報を 1 つ確認する必要があります。 プロジェクト プロパティで、 **SSL URL** アプリケーションです。 

    ![](./media/web-sites-dotnet-lob-application-adfs/3-ssl-url.png)

6.  AD FS 管理で再度、 **URL の構成** のページ、 **追加証明書利用者信頼のウィザード**, [ **Ws-federation パッシブ プロトコルのサポートを有効にする** し、前の手順でメモした Visual Studio プロジェクトの SSL URL を入力します。 クリックして **次**します。

    ![](./media/web-sites-dotnet-lob-application-adfs/4-configure-url.png)

    > [AZURE.NOTE] URL は、認証が成功したら、クライアントに送信する場所を指定します。 デバッグ環境では、<code>https://localhost:&lt;port&gt;/</code> にする必要があります。 発行した Web アプリに対しては、Web アプリの URL を指定します。

7.   **識別子の構成** ] ページで、プロジェクトの SSL URL が既に表示されていることを確認して、クリックして **次**します。 クリックして **次** は既定の設定を使用して、ウィザードの最後までです。

    > [AZURE.NOTE] Visual Studio プロジェクトの App_Start\Startup.Auth.cs での値に対してこの識別子を照合 <code>WsFederationAuthenticationOptions.Wtrealm</code> フェデレーション認証時にします。 既定では、前の手順のアプリケーションの URL が RP 識別子として追加されます。

8.  これで、AD FS でのプロジェクトの RP アプリケーションの構成が完了しました。 次に、アプリケーションで必要なクレームを送信するようにこのアプリケーションを構成します。  **要求規則の編集** ダイアログが表示される既定のウィザードの最後にすぐに開始できるようにします。 少なくとも次のクレームを構成します (かっこ内にスキーマを示します)。

    -   設定する、ASP.NET で使用される名前 (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) - `User.Identity.Name`です。
    -   ユーザー プリンシパル名 (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn) - 組織内のユーザーを一意に識別するために使用します。
    -   ロール (http://schemas.microsoft.com/ws/2008/06/identity/claims/role) - としてグループ メンバーシップを併用する `[Authorize(Roles="role1, role2,...")]` コント ローラーや操作を承認するために装飾します。 実際には、これがロールの承認に関して最も効率的な方法であるとは限りません (特に、AD ユーザーが常に数百のセキュリティ グループに属していて、SAML トークンの数百のロール要求が発生する場合)。 これに代わる方法は、特定のグループ内のユーザーのメンバーシップに応じて、条件付きで 1 つのロール クレームを送信する方法です。 ただし、このチュートリアルでは、単純なやり方を使用します。
    -   名前 ID (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier) - 偽造防止検証に使用できます。 偽装防止検証と連携させる方法の詳細については、次を参照してください。、 **サンプル アプリケーションに基幹業務機能を追加** の [Azure Active Directory 認証の Azure App Service で .NET MVC web アプリを作成](web-sites-dotnet-lob-application-azure-ad.md#bkmk_crud)します。

    > [AZURE.NOTE] アプリケーションを構成する必要がクレームの種類は、アプリケーションのニーズによって決まります。 Azure Active Directory アプリケーション (RP 信頼) によってサポートされているクレームの一覧、たとえば、表示 [サポートされているトークンとクレームの種類](http://msdn.microsoft.com/library/azure/dn195587.aspx)します。

8.  要求規則の編集ダイアログ ボックスで [ **ルールの追加**します。
9.  次に示すように、名前、UPN、ロール クレームを構成し、 **完了**します。

    ![](./media/web-sites-dotnet-lob-application-adfs/5-ldap-claims.png)

    次に、ID のクレームでページの手順に従って、一時的な名前を作成、 [SAML アサーションで名前識別子](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx)します。

9.  クリックして **ルールの追加** 再度します。
10. 選択 **カスタム規則を使用して要求を送信** ] をクリック **次**します。
11. 次の規則言語を貼り付け、 **カスタム規則** ボックスに、ルールの名前 **Per Session Identifier** ] をクリック **完了**します。  
    <pre class="prettyprint">
    c1: [型"http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"= =] & amp; & amp;
    c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant"]
        => add(
            store = "_OpaqueIdStore",
            種類 = ("<mark>http://contoso.com/internal/sessionid</mark>"),
            クエリ「{0}、\\ 1 \\、\\ {2 \}; \\ 3 \\、\\ {4 \\}」、=
            param = "useEntropy",
            param = c1.Value,
            param = c1.OriginalIssuer,
            param = "",
            param = c2.Value);
    </pre>

    これで、次のようなカスタム規則が作成されました。

    ![](./media/web-sites-dotnet-lob-application-adfs/6-per-session-identifier.png)

9.  クリックして **ルールの追加** 再度します。
10. 選択 **入力方向の要求を変換** ] をクリック **次**します。
11. (カスタム規則で作成したクレームの種類を使用して) 次のように規則を構成し、クリックして **完了**します。

    ![](./media/web-sites-dotnet-lob-application-adfs/7-transient-name-id.png)

    上の一時的な名前 ID クレームに関する手順の詳細については、次を参照してください。 [SAML アサーションで名前識別子](http://blogs.msdn.com/b/card/archive/2010/02/17/name-identifiers-in-saml-assertions.aspx)します。

12. クリックして **適用** で、 **要求規則の編集** ダイアログ。 その結果、次のスクリーンショットのようになります。

    ![](./media/web-sites-dotnet-lob-application-adfs/8-all-claim-rules.png)

    > [AZURE.NOTE] ここでも、デバッグ環境と発行した web アプリの両方に対して次の手順を繰り返すことを確認します。

<a name="bkmk_test"></a>
## アプリケーションのフェデレーション認証をテストする

この段階で、AD FS に対するアプリケーションの認証ロジックをテストできます。 ここで使用している AD FS ラボ環境には、Active Directory (AD) のテスト グループに属しているテスト ユーザーが設定されています。

![](./media/web-sites-dotnet-lob-application-adfs/10-test-user-and-group.png)

デバッガーで認証をテストする場合、必要な操作は `F5` キーを押すことだけです。 発行した Web アプリでの認証をテストするには、対象の URL に移動します。

Web アプリケーションが読み込まれると、クリックして **サイン イン**します。 AD FS によって選択された認証方法によって、ログイン ダイアログまたは AD FS によって提供されるログイン ページのどちらかが表示されます。 Internet Explorer 11 を使った例を次に示します。

![](./media/web-sites-dotnet-lob-application-adfs/9-test-debugging.png)

AD FS デプロイメントの AD ドメインにユーザーとしてログインするとを使用してホーム ページが表示されます **こんにちは、 <User Name>!** 上隅にあります。 例を次に示します。

![](./media/web-sites-dotnet-lob-application-adfs/11-test-debugging-success.png)

これまで、次の処理が行われてきました。

- アプリケーションが正常に AD FS にアクセスし、対応する RP 識別子が AD FS データベースに見つかった。
- AD FS によって正常に AD ユーザーが認証され、アプリケーションのホームページにリダイレクトされた。
- として AD FS は、隅にあるユーザー名が表示されたことによって示されるため、アプリケーション名の要求 (http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name) を正常に送信します。 

名前クレームが存在しない場合は、という表示 **ハロー,!**します。 Views\Shared\_LoginPartial.cshtml 見てを行うことがわかりますが使用されている `User.Identity.Name` にユーザー名を表示します。 前に説明したように、ASP.NET では、認証されたユーザーの名前クレームがこのプロパティに設定されます (SAML トークンで取得できる場合)。 AD FS によって送信されるすべてのクレームを確認するには、Controllers\HomeController.cs の Index 操作メソッドにブレークポイントを設定します。 ユーザーが認証された後、`System.Security.Claims.Current.Claims` コレクションを調べます。

![](./media/web-sites-dotnet-lob-application-adfs/12-test-debugging-all-claims.png) 

<a name="bkmk_authorize"></a>
## 特定のコントローラーまたは操作に対してユーザーを承認する

RP 信頼構成にロール クレームとしてグループ メンバーシップが含まれているため、コントローラーと操作の `[Authorize(Roles="...")]` 装飾内で直接これらを使用できます。 "作成、読み取り、更新、削除" (CRUD) パターンを持つ基幹業務アプリケーションでは、特定のロールが各操作にアクセスすることを承認できます。 ここでは、既存の Home コントローラーでこの機能を試してみましょう。

1. Controllers\HomeController.cs を開きます。
2. 認証されたユーザーが持つセキュリティ グループ メンバーシップを使用して、次のように `About` と `Contact` の操作メソッドを装飾します。  
    <pre class="prettyprint">
    <mark>[Authorize(Roles="Test Group")]</mark>
    public ActionResult About()
    {
        ViewBag.Message = "Your application description page.";

        return View();
    }

    <mark>[Authorize(Roles="Domain Admins")]</mark>
    public ActionResult Contact()
    {
        ViewBag.Message = "Your contact page.";

        return View();
    }
    </pre>

    追加しているため **Test User** に **テスト グループ** 、AD FS ラボ環境ではテスト Group を使用してに関する承認をテスト `About`します。  `Contact`, 、失敗のケースをテスト **Domain Admins**, をする **Test User** 属していません。

3. 」と入力して、デバッガーを起動 `F5` 、サインインをクリックし、 **に関する**します。 認証されたユーザーによるこの操作の実行が承認されている場合、`~/About/Index` ページが正常に表示されます。
4. クリックして **連絡先**, 、この例は承認されません **テスト ユーザー** アクションのです。 ただし、ブラウザーは AD FS にリダイレクトされ、次のメッセージが表示されます。

    ![](./media/web-sites-dotnet-lob-application-adfs/13-authorize-adfs-error.png)

    AD FS サーバーのイベント ビューアーでこのエラーを調べると、次の例外メッセージを確認できます。  
    <pre class="prettyprint">
    Microsoft.identityserver.web.invalidrequestexception:: MSIS7042: <mark>同じクライアントのブラウザー セッション last '11' 秒間に '6' requests にしました。</mark> 詳しくは、管理者に問い合わせてください。
       at Microsoft.IdentityServer.Web.Protocols.PassiveProtocolHandler.UpdateLoopDetectionCookie(WrappedHttpListenerContext context)
       at Microsoft.IdentityServer.Web.Protocols.WSFederation.WSFederationProtocolHandler.SendSignInResponse(WSFederationContext context, MSISSignInResponse response)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.ProcessProtocolRequest(ProtocolContext protocolContext, PassiveProtocolHandler protocolHandler)
       at Microsoft.IdentityServer.Web.PassiveProtocolListener.OnGetContext(WrappedHttpListenerContext context)
    </pre>

    このエラーの理由は、ユーザーのロールが承認されない場合に MVC が既定で 401 Unauthorized を返すためです。 これが、ID プロバイダー (AD FS) への再認証クレームのトリガーとなります。 ユーザーは既に認証されているため、AD FS によって同じページが表示されることになります。これが原因で別の 401 が発行され、結果的にリダイレクト ループになります。 そこで、リダイレクト ループを続ける代わりに意味のメッセージを表示する単純なロジックで `HandleUnauthorizedRequest` の メソッドをオーバーライドします。

5. プロジェクトに AuthorizeAttribute.cs という名前のファイルを作成し、次のコードを貼り付けます。

        using System;
        using System.Web.Mvc;
        using System.Web.Routing;
        
        namespace WebApp_WSFederation_DotNet
        {
            [AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
            public class AuthorizeAttribute : System.Web.Mvc.AuthorizeAttribute
            {
                protected override void HandleUnauthorizedRequest(AuthorizationContext filterContext)
                {
                    if (filterContext.HttpContext.Request.IsAuthenticated)
                    {
                        filterContext.Result = new System.Web.Mvc.HttpStatusCodeResult((int)System.Net.HttpStatusCode.Forbidden);
                    }
                    else
                    {
                        base.HandleUnauthorizedRequest(filterContext);
                    }
                }
            }
        }

    このオーバーライド コードは、認証-にもかかわらず承認されない場合、HTTP 401 (Unauthorized) ではなく HTTP 403 (Forbidden) を送信します。

6. もう一度 `F5` キーを押してデバッガーを実行します。 クリックすると **連絡先** (魅力的で) より説明的なエラー メッセージが表示されます。

    ![](./media/web-sites-dotnet-lob-application-adfs/14-unauthorized-forbidden.png)

7. アプリケーションを Azure App Service Web アプリにもう一度発行して、ライブ アプリケーションの動作をテストします。

<a name="bkmk_data"></a>
## オンプレミス データに接続する

基幹業務アプリケーションに Azure Active Directory ではなく AD FS を実装する 1 つの理由として、組織データをオフプレミスに保持する場合のコンプライアンスの問題があります。 これはことも意味を使用することはできないために、Azure で web アプリに内部設置型データベースをアクセスする必要があります [SQL Database](/services/sql-database/) web アプリのデータ層として。

Azure App Service Web Apps は、2 つの方法で内部設置型データベースへのアクセスをサポートしています: [ハイブリッド接続](../integration-hybrid-connection-overview.md) と [仮想ネットワーク](web-sites-integrate-with-vnet.md)します。 詳細については、次を参照してください。 [を使用して VNET 統合と Azure App Service Web Apps でのハイブリッド接続](http://azure.microsoft.com/blog/2014/10/30/using-vnet-or-hybrid-conn-with-websites/)します。

<a name="bkmk_resources"></a>
## 他の関連リソース

- [SSL と Authorize 属性を使用してアプリケーションを保護する](web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database.md#protect-the-application-with-ssl-and-the-authorize-attribute)
- [Azure App Service での認証には、Active Directory を使用します](web-sites-authentication-authorization.md)
- [Azure Active Directory の認証を使用して Azure App Service で .NET MVC Web アプリを作成する](web-sites-dotnet-lob-application-azure-ad.md)
- [Visual Studio 2013 で ASP.NET のオンプレミス組織認証オプション (ADFS) を使用する方法に関するページ](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)
- [Vittorio Bertocci のブログ](http://blogs.msdn.com/b/vbertocci/)
- [VS2013 Web プロジェクトを WIF から Katana に移植する方法に関するページ](http://www.cloudidentity.com/blog/2014/09/15/MIGRATE-A-VS2013-WEB-PROJECT-FROM-WIF-TO-KATANA/)
- [Active Directory フェデレーション サービスの概要に関するページ](http://technet.microsoft.com/library/hh831502.aspx)
- [WS-Federation 1.1 仕様](http://download.boulder.ibm.com/ibmdl/pub/software/dw/specs/ws-fed/WS-Federation-V1-1B.pdf?S_TACT=105AGX04&S_CMP=LP)

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]
 
 


