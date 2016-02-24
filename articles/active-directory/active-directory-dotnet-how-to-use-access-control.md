<properties
    pageTitle="Access Control の使用方法 (.NET) | Microsoft Azure"
    description="このガイドでは、ユーザーが Web アプリケーションにアクセスしようとするときに、それらのユーザーを認証するために Azure アプリケーションで Access Control Service (ACS) を使用する方法について説明します。"
    services="active-directory"
    documentationCenter=".net"
    authors="msmbaldwin"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="12/05/2015" 
    ms.author="mbaldwin"/>




# Azure Active Directory の Access Control で Web ユーザーを認証する方法


このガイドでは、Microsoft、Google、Yahoo、Facebook などの ID プロバイダーのユーザーが Web アプリケーションにアクセスしようとするときに、それらのユーザーを認証するために Azure Active Directory の Access Control (Access Control サービス (ACS) とも呼ばれます) を使用する方法について説明します。


## ACS とは

ほとんどの開発者は ID の専門家ではなく、アプリケーションやサービスの認証と承認のメカニズムの開発に時間をかけたくはありません。 ACS は、Web アプリケーションとサービスにアクセスするユーザーを認証するための簡単な方法を提供する Azure サービスです。これを使えば、複雑な認証ロジックをコードに追加する必要はありません。

ACS では、次の機能を使用できます。

-   Windows Identity Foundation (WIF) との統合。
-   Microsoft アカウント (旧 Windows Live ID)、Google、Yahoo、Facebook などの一般的な Web ID プロバイダーのサポート。
-   Active Directory フェデレーション サービス (AD FS) 2.0 のサポート。
-   Open Data Protocol (OData)-ベースの管理サービスを提供します。
    ACS 設定へのプログラムでアクセスします。
-   ACS 設定への管理アクセスが可能な管理ポータル
    削除する必要があります。

ACS の詳細については、次を参照してください。 [Access Control Service 2.0][]します。

## 概念

ACS はクレームベース ID のプリンシパルに基づいており、これは内部またはクラウドで実行されているアプリケーションの認証メカニズムを作成するための一貫した方法になっています。 クレームベース ID は、アプリケーションとサービスが組織内、他の組織内、およびインターネット上のユーザーに関する必要な ID 情報を取得するための一般的な方法を提供します。

このガイドのタスクを完了するには、このガイドで使用される次の用語と概念を理解している必要があります。


**クライアント** -web アプリケーションにアクセスしようとするブラウザー。

**証明書利用者 (rp) アプリケーション** -web アプリです。 RP アプリケーションは、外部の機関に認証を委託する Web サイトまたは Web サービスです。 ID の分野では、"RP がその機関を信頼する" という表現を使います。 このガイドでは、アプリケーションが ACS を信頼するように構成する方法について説明します。

**トークン** -ユーザーは、RP アプリケーションが信頼している機関によって発行された有効なトークンを提示することによって、RP アプリケーションにアクセス許可を取得します。 クライアントの認証時に発行されたセキュリティ データのコレクションです。 これには、クレームのセット、つまり、ユーザーの名前や年齢、ユーザー ロールの ID など、認証されたユーザーの属性が含まれます。 トークンはデジタル署名されるため、発行者を特定でき、その内容は変更できません。

**Id プロバイダー (IP)** : ユーザー id を認証し、Microsoft アカウント (Windows Live ID)、Facebook、Google、Twitter、および Active Directory などのセキュリティ トークンを発行する機関です。 ACS が IP を信頼するように構成されている場合、ACS はその IP によって発行されたトークンを受け入れて検証します。 ACS は、ときに、アプリケーションは、ACS を信頼する、同時に複数の Ip を信頼できるので、アプリケーションことができます、認証するオプションを自動的に ACS が信頼している ip アドレスのいずれかがユーザーに提示できます。

**フェデレーション プロバイダー (FP)** -Id プロバイダー (Ip) は、ユーザーの直接の知識がある、自分の資格情報を使用してユーザーを認証し、ユーザーに関するクレームを発行します。 フェデレーション プロバイダー (FP) は、それとは別の種類の機関です。 ユーザーを直接認証する代わりに、FP は認証を仲介します。 証明書利用者アプリケーションと 1 つ以上の IP の間の仲介役として機能します。 ACS はフェデレーション プロバイダー (FP) です。

**ACS 規則エンジン** -クレーム変換規則では、RP で使用できるように信頼されている Ip からのトークンのクレームを変換します。 ACS には、RP に指定するクレーム変換規則を適用するルール エンジンが含まれています。

**Access Control 名前空間** -アプリケーション内で ACS リソースのアドレスを指定するための一意のスコープを提供します。 この名前空間には、信頼する IP、サービスを提供する RP アプリケーション、受信トークンに適用する規則などの設定が含まれ、アプリケーションと開発者が ACS とのやり取りに使用するエンドポイントが表示されます。

次の図は、Web アプリケーションで ACS 認証がどのように機能するかを示しています。

![][0]

1.  クライアント (ここではブラウザー) が RP のページを要求します。
2.  要求がまだ認証されていないので、RP は信頼している機関 (つまり
    ユーザーが ACS をされている、信頼できる機関です。 ACS は、この RP
    この RP 用に指定されている Ip の選択肢を持つユーザー。 この
    ユーザーは、適切な IP を選択します。
3.  クライアントは IP の認証ページに移動し、ユーザーにログオンを求めま
    ユーザーにログオンします。
4.  クライアントが認証されたら (たとえば、ID 資格情報が入力さ
    資格情報が入力されたら)、IP セキュリティ トークンを発行します。
5.  セキュリティ トークンの発行後、IP は、IP が ACS に発行したセキュリティ トークンを送信するようクライアントに指示します。
6.  ACS は IP によって発行されたセキュリティ トークンを検証し、
    ACS 規則エンジンにこのトークンの id のクレームを計算します。
    出力 id 要求、および新しいセキュリティ トークンを発行
    これらの出力クレームが含まれています。
7.  ACS は、ACS が RP に発行したセキュリティ トークンを送信するようクライアントに指示します。 RP がセキュリティ トークンの署名を検証して、アプリケーション ビジネス ロジックにより使用されるクレームを抽出し、最初に要求されたページを返します。

## 前提条件


このガイドのタスクを完了するには、次のものが必要です。

-   Azure サブスクリプション
-   Microsoft Visual Studio 2012
-   Id および Access Tool for Visual Studio 2012 (ダウンロードするを参照してください [Identity and Access Tool][])。


## Access Control 名前空間の作成

Azure で Active Directory Access Control を使用するには、Access Control 名前空間を作成します。 名前空間では、
アプリケーション内で ACS リソースのアドレスを指定します。

1.  ログイン、 [Azure 管理ポータル][] (https://manage.WindowsAzure.com)。

2.  クリックして **Active Directory**します。  

    ![][1]

3.  新しい Access Control 名前空間を作成する] をクリックして **新規**します。 **App Services** と **アクセス制御** が選択されます。 クリックして **簡易作成**します。

    ![][2]

4.  名前空間の名前を入力します。 名前が一意であるかが確認されます。

5.  名前空間が使用されるリージョンを選択します。 最適なパフォーマンスをアプリケーションをデプロイする地域を順にクリックを使用して **作成**します。

名前空間が作成されて有効化されます。

## ASP.NET MVC アプリケーションを作成する

この手順では、ASP.NET MVC アプリケーションを作成します。 後の手順では、このシンプルな Web フォーム アプリケーションを ACS に統合します。

1.  Visual Studio 2012 または Visual Studio Express for Web 2012 を起動します (それより前のバージョンの Visual Studio は、このチュートリアルでは機能しません)。
1.  クリックして **ファイル**, 、] をクリックし、 **新しいプロジェクト**します。
1.  [Visual c# Web テンプレート、し、、[ **ASP.NET MVC 4 Web アプリケーション**します。

    このガイドでは MVC アプリケーションを使用しますが、このタスクにはどの種類の Web アプリケーションでも使用できます。

    ![][3]

1.  **名**, 、型 **MvcACS**, 、] をクリックし、 **OK**します。
1. 次のダイアログ ボックスで選択 **インターネット アプリケーション**, 、] をクリックし、 **OK**します。
1. 編集、 *Views\Shared\_LoginPartial.cshtml* ファイルし、内容を次のコードに置き換えます。

        @if (Request.IsAuthenticated)
        {
            string name = "Null ID";
            if (!String.IsNullOrEmpty(User.Identity.Name))
            {
                name = User.Identity.Name;
            }
            <text>
            Hello, @Html.ActionLink(name, "Manage", "Account", routeValues: null, htmlAttributes: new { @class = "username", title = "Manage" })!
                    @using (Html.BeginForm("LogOff", "Account", FormMethod.Post, new { id = "logoutForm" }))
                    {
                        @Html.AntiForgeryToken()
                        <a href="javascript:document.getElementById('logoutForm').submit()">Log off</a>
                    }
            </text>
        }
        else
        {
            <ul>
                <li>@Html.ActionLink("Register", "Register", "Account", routeValues: null, htmlAttributes: new { id = "registerLink" })</li>
                <li>@Html.ActionLink("Log in", "Login", "Account", routeValues: null, htmlAttributes: new { id = "loginLink" })</li>
            </ul>
        }

現在のところ、ACS は User.Identity.Name を設定しないため、上記の変更を加える必要があります。

1. F5 キーを押してアプリケーションを実行します。 既定の ASP.NET MVC アプリケーションが Web ブラウザーに表示されます。

## Web アプリケーションを ACS と統合する

このタスクでは、ASP.NET Web アプリケーションを ACS と統合します。

1.  ソリューション エクスプ ローラーでは、MvcACS プロジェクトを右クリックして [ **Id およびアクセス**します。

    場合、 **Id およびアクセス** オプションが表示されないコンテキスト メニューで、Identity and Access Tool をインストールします。 詳細については、次を参照してください。 [Identity and Access Tool]します。

    ![][4]

2.   **プロバイダー** ] タブで [ **Azure Access Control Service を使用して**します。

    ![][44]

3.  クリックして、 **構成** リンクします。

    ![][444]

    Visual Studio により、Access Control 名前空間に関する情報が要求されます。 前の手順で作成した名前空間名を入力します (上の画像では Test ですが、実際には別の名前空間です)。 Azure 管理ポータルに戻り、対称キーを取得します。

    ![][17]

4.  Azure の管理ポータルでアクセス制御名前空間をクリックし、をクリックして **管理**します。

    ![][8]

5.  クリックして **管理サービス** ] をクリックし、 **管理クライアント**です。

    ![][18]

6.  をクリックして **対称キー**, 、] をクリックして **[キーの表示**, 、キーの値をコピーします。 クリックして **キャンセル** 変更を加えずに管理クライアントの編集] ページを終了します。

    ![][19]

7.  Visual Studio でのキーを貼り付け、 **名前空間の管理キーを入力** フィールドでをクリックし、 **管理キー**, 、順にクリック **[ok]**します。

    ![][20]

    Visual Studio は、名前空間に関する情報を使用して ACS 管理ポータルに接続し、ID プロバイダー、領域、戻り先 URL など、名前空間の設定を取得します。

8.  選択 **Windows Live ID** (Microsoft アカウント) し、[ok] をクリックします。

    ![][5]

## ACS との統合をテストする

このタスクでは、RP アプリケーションと ACS の統合をテストする方法について説明します。

-   Visual Studio で F5 キーを押してアプリケーションを実行します。

アプリケーションが ACS と統合されており、Windows Live ID (Microsoft アカウント) を選択した場合、既定の ASP.NET Web フォーム アプリケーションを開く代わりに、ブラウザーが Microsoft アカウントのサインイン ページにリダイレクトされます。 サインインすると有効なユーザー名、パスワード、MvcACS アプリケーションにリダイレクトされます。

![][6]

ご利用ありがとうございます。 ACS と ASP.NET Web アプリケーションを正常に統合できました。 ACS が Microsoft アカウントの資格情報を使用してユーザーの認証を処理するようになっています。

## ACS により送信されたクレームを参照する

このセクションでは、ACS によって送信されたクレームが参照されるようにアプリケーションを変更します。  Identity and Access Tool により、IP からアプリケーションへのすべてのクレームを通過する規則グループが作成されています。  ID プロバイダーが異なると送信されるクレームが異なる点に注意してください。

1. 開いている、 *controllers \homecontroller.cs* ファイルです。 追加、 **を使用して** の声明 **System.Threading**:

    using System.Threading;

1. HomeController でクラスを追加、 *クレーム* メソッド。

    public ActionResult Claims()
    {
        ViewBag.Message =「、要求ページです」;。

        ViewBag.ClaimsIdentity = Thread.CurrentPrincipal.Identity;

        return View();
    }

1. 右クリックして、 *クレーム* メソッドと select **ビューの追加**します。

![][66]

1. クリックして **追加**します。

1. 内容を置き換える、 *Views\Home\Claims.cshtml* を次のコード ファイル。

        @{
            ViewBag.Title = "Claims";
        }
        <hgroup class="title">
            <h1>@ViewBag.Title.</h1>
            <h2>@ViewBag.Message</h2>
        </hgroup>
        <h3>Values from Identity</h3>
        <table>
            <tr>
                <td>
                    IsAuthenticated:
                </td>
                <td>
                    @ViewBag.ClaimsIdentity.IsAuthenticated
                </td>
            </tr>
            <tr>
                <td>
                    Name:
                </td>
                <td>
                    @ViewBag.ClaimsIdentity.Name
                </td>
            </tr>
        </table>
        <h3>Claims from ClaimsIdentity</h3>
        <table>
            <tr>
                <th>
                    Claim Type
                </th>
                <th>
                    Claim Value
                </th>
            </tr>
                @foreach (System.Security.Claims.Claim claim in ViewBag.ClaimsIdentity.Claims ) {
            <tr>
                <td>
                    @claim.Type
                </td>
                <td>
                    @claim.Value
                </td>
            </tr>
        }
        </table>

1. アプリケーションを実行しに移動し、 *クレーム* メソッド。

![][666]

アプリケーションでクレームの使い方の詳細については、次を参照してください。、 [Windows Identity Foundation ライブラリ](http://msdn.microsoft.com/library/hh377151.aspx)します。

## ACS 管理ポータルでアプリケーションを参照する

Visual Studio の Identity and Access Tool は、アプリケーションと ACS と自動的に統合します。

[Azure の Access Control を使用する] オプションを選択してアプリケーションを実行すると、Identity and Access Tool によりアプリケーションが証明書利用者として追加されて、選択された ID プロバイダーを使用するように構成され、アプリケーションの既定のクレーム変換規則が生成および選択されます。

これらの構成設定は、ACS 管理ポータルで確認して変更できます。 ポータルで変更を確認するには、次のステップを使用します。

1.  Windows にログイン [Azure 管理ポータル](http://manage.WindowsAzure.com)します。

2.  クリックして **Active Directory**します。

    ![][8]

3.  Access Control の名前空間を選択し、クリックして **管理**します。 この操作により、ACS 管理ポータルが開きます。

    ![][9]


4.  クリックして **証明書利用者アプリケーション**します。

    新しい MvcACS アプリケーションが、証明書利用者アプリケーションの一覧に表示されます。 領域は、自動的にアプリケーション メイン ページに設定されます。

    ![][10]


5.  クリックして **MvcACS**します。

    [証明書利用者アプリケーションの編集] ページには、MvcACS Web アプリケーションの構成設定が表示されます。 このページの設定を変更して保存すると、変更がすぐにアプリケーションに適用されます。

    ![][11]

6.  ID プロバイダーやクレーム変換規則など、MvcACS アプリケーションの他の構成設定が表示されるまで、ページを下にスクロールします。

    ![][12]

次のセクションでは、ACS 管理ポータルの機能を使用して Web アプリケーションに変更を加え、どれくらい簡単に変更できるかを示します。

## ID プロバイダーを追加する

ACS 管理ポータルを使用して、MvcACS アプリケーションの認証を変更しましょう。 この例では、Google を MvcACS の ID プロバイダーとして追加します。

1.  クリックして **Id プロバイダー** (ナビゲーション メニュー) の順にクリック **追加**します。

    ![][13]

2.  クリックして **Google** ] をクリックし、 **次**します。 MvcACS アプリケーションのチェック ボックスが既定でオンになっています。

    ![][14]

3. [保存] をクリックします。

    ![][15]


これで完了です。 Visual Studio に戻る場合は、MvcACS アプリケーションのプロジェクトを開き] をクリックして **Id およびアクセス**, 、ツール、Windows Live ID と Google id プロバイダーを一覧表示します。  

![][16]

また、アプリケーションを実行すると、効果を確認できます。 アプリケーションで複数の ID プロバイダーがサポートされる場合、ユーザーのブラウザーにはまず ACS によりホストされているページが表示され、ユーザーは ID プロバイダーの選択を求められます。

![][7]

ユーザーが ID プロバイダーを選択したら、ブラウザーは ID プロバイダーのサインイン ページに移動します。

## 参照トピック

ACS と統合された Web アプリケーションを作成できました。 ただし、まだ始まりにすぎません。 このシナリオを展開することができます。

たとえば、この RP に他の ID プロバイダーを追加したり、Active Directory ドメイン サービスなどのエンタープライズ ディレクトリに登録されたユーザーが Web アプリケーションにログオンできるようにしたりすることができます。

アプリケーション ビジネス ロジックで処理するためにアプリケーションに送信されるクレームを決定する規則を、名前空間に追加することもできます。

さらに ACS の機能を調べたりより多くのシナリオを試してみたりする場合、次を参照してください。 [Access Control Service 2.0]します。



  [What is ACS?]: #what-is
  [Concepts]: #concepts
  [Prerequisites]: #pre
  [Create an ASP.NET MVC Application]: #create-web-app
  [Create an Access Control Namespace]: #create-namespace
  [Integrate your Web Application with ACS]: #Identity-Access
  [Test the Integration with ACS]: #Test-ACS
  [View the Application in the ACS Management Portal]: acs-portal
  [Add an Identity Provider]: #add-IP
  [What's Next]: #whats-next
  [vcsb]: #bkmk_viewClaims
  [vpp]: #bkmk_VP

  [Access Control Service 2.0]: http://go.microsoft.com/fwlink/?LinkID=212360
  [Identity and Access Tool]: http://go.microsoft.com/fwlink/?LinkID=245849
  [Azure Management Portal]: http://manage.WindowsAzure.com

  [0]: ./media/active-directory-dotnet-how-to-use-access-control/acs-01.png
  [1]: ./media/active-directory-dotnet-how-to-use-access-control/acsCreateNamespace.png
  [2]: ./media/active-directory-dotnet-how-to-use-access-control/acsQuickCreate.png
  [3]: ./media/active-directory-dotnet-how-to-use-access-control/rzMvc.png
  [4]: ./media/active-directory-dotnet-how-to-use-access-control/rzIA.png
[44]: ./media/active-directory-dotnet-how-to-use-access-control/rzPT.png
 [444]: ./media/active-directory-dotnet-how-to-use-access-control/rzC.png
  [5]: ./media/active-directory-dotnet-how-to-use-access-control/acsIdAndAccess1.png
  [6]: ./media/active-directory-dotnet-how-to-use-access-control/acsMSFTAcct.png
  [66]: ./media/active-directory-dotnet-how-to-use-access-control/rzAv.png
  [666]: ./media/active-directory-dotnet-how-to-use-access-control/rzCl.png
  [7]: ./media/active-directory-dotnet-how-to-use-access-control/acsSignIn.png
  [8]: ./media/active-directory-dotnet-how-to-use-access-control/acsClickManage.png
  [9]: ./media/active-directory-dotnet-how-to-use-access-control/acsACSPortal.png
  [10]: ./media/active-directory-dotnet-how-to-use-access-control/acsRPPage.png
  [11]: ./media/active-directory-dotnet-how-to-use-access-control/acsEdit-RP.png
  [12]: ./media/active-directory-dotnet-how-to-use-access-control/acsEdit-RP2.png
  [13]: ./media/active-directory-dotnet-how-to-use-access-control/acsAdd-Idp.png
  [14]: ./media/active-directory-dotnet-how-to-use-access-control/acsAdd-Google.png
  [15]: ./media/active-directory-dotnet-how-to-use-access-control/acsSave-Google.png
  [16]: ./media/active-directory-dotnet-how-to-use-access-control/acsIdAndA-after.png
  [17]: ./media/active-directory-dotnet-how-to-use-access-control/acsConfigAcsNamespace.png
  [18]: ./media/active-directory-dotnet-how-to-use-access-control/acsManagementService.png
  [19]: ./media/active-directory-dotnet-how-to-use-access-control/acsShowKey.png
  [20]: ./media/active-directory-dotnet-how-to-use-access-control/acsConfigAcsNamespace2.png

