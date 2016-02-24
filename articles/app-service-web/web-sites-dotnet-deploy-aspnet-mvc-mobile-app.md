<properties 
    pageTitle="ASP.NET MVC 5 モバイル Web アプリケーションを Azure App Service に展開する" 
    description="ASP.NET MVC 5 Web アプリケーションのモバイル機能を使用して Web アプリケーションを Azure App Service に展開する方法のチュートリアル。" 
    services="app-service" 
    documentationCenter=".net" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/16/2015" 
    ms.author="cephalin;riande"/>


# ASP.NET MVC 5 モバイル Web アプリケーションを Azure App Service に展開する

このチュートリアルでは、モバイル対応の ASP.NET MVC 5 Web アプリケーションを作成し、
web アプリをモバイル対応し、Azure App Service に展開します。 このチュートリアルを実行するには、 
[Visual Studio Express 2013 for Web][Visual Studio Express 2013]
または、Visual Studio のプロフェッショナル エディションを既にお持ちの場合は
はそれを使用することもできます。 [Visual Studio 2015] を使用することができますが、スクリーン ショットは、ASP.NET 4.x テンプレートを使用する必要があります。

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## 作成するアプリケーション:

このチュートリアルでは、単純なものにモバイル機能を追加します
[スタート プロジェクト] で提供されている会議一覧アプリケーション [StarterProject] です。 次のスクリーン ショットは、ASP.NET セッションを完了しました。
アプリケーションに Internet Explorer 11 F12 のブラウザー エミュレーターで表示
開発者ツール。

![FixedSessionsByTag]

Internet Explorer 11 F12 開発者ツールや [Fiddler を使用することができます。
[ツール] をデバッグする際に、[Fiddler]、
想定しています。 

## 学習内容

ここでは次の内容について学習します。

-   Visual Studio 2013 を使用して、Web アプリケーションを Azure App Service に直接発行する方法
-   ASP.NET MVC 5 テンプレートで CSS Bootstrap フレームワークを使用する方法
    モバイル デバイスでの表示を向上させる
-   特定のモバイル モバイル専用ビューを作成する方法
    iPhone や Android などのブラウザー
-   レスポンシブ ビュー (さまざまなに対応するビューを作成する方法
    複数のデバイスのブラウザー)

## 開発環境を設定する

Azure SDK for .NET 2.5.1 以降をインストールして、開発環境を設定します。 

1. Azure SDK for .NET をインストールするには、次のリンクをクリックします。 Visual Studio 2013 をまだインストールしていない場合は、次のリンクをクリックするとインストールされます。 このチュートリアルには、Visual Studio 2013 が必要です。 [Azure SDK for Visual Studio 2013][AzureSDKVs2013]
1. Web Platform Installer のウィンドウで [ **インストール** し、インストールを実行します。

モバイル ブラウザー エミュレーターも必要です。 次のいずれでも動作
します。

-   [Internet Explorer 11 F12 開発者ツール] のブラウザー エミュレーター [EmulatorIE11] (すべてのモバイル場合に使用します。
    のスクリーン ショットで使用しています)。 Windows Phone 8、Windows Phone 7、Apple iPad のエージェント文字列のプリセットが搭載されています。
-   [Google Chrome DevTools] のブラウザー エミュレーター [EmulatorChrome] です。 多数の Android デバイスに加え、Apple iPhone、Apple iPad、Amazon Kindle Fire 用のプリセットが含まれています。 タッチイベントのエミュレーションにも対応します
-   [Opera Mobile Emulator][EmulatorOpera]

付属する c# ソース コードを visual Studio プロジェクトがあります。
用意されています。

-   [スタート プロジェクトのダウンロード][StarterProject]
-   [プロジェクトのダウンロードが完了しました][CompletedProject]

##<a name="bkmk_DeployStarterProject"></a>Azure Web アプリケーションにスタート プロジェクトを展開する

1.  会議一覧アプリケーションの [スタート プロジェクト] のダウンロード [StarterProject] です。

2.  Windows エクスプ ローラーで Mvc5Mobile.zip ファイルを右クリックし、選択 *プロパティ*します。

3.   **Mvc5Mobile.zip のプロパティ** ] ダイアログ ボックス
選択、 **ブロックを解除する** ] ボタンをクリックします。 (ブロックを解除すると、Web からダウンロード
使用しようとする際に、 *.zip* ダウンロードしたファイルを
セキュリティに関する警告を回避できます)。

4.  右クリックし、 *Mvc5Mobile.zip* ファイルおよび選択した **すべて展開** に
ファイルを解凍します。 

5.  Visual Studio で開き、 *Mvc5Mobile.sln* ファイルです。

6.  ソリューション エクスプ ローラーでプロジェクトを右クリックし、クリックして **発行**します。

    ![DeployClickPublish]

7.  Web の発行] をクリックして **Microsoft Azure Web Apps**します。

    ![DeployClickWebSites]

8.  クリックして **サインイン**します。

    ![DeploySignIn]

9.  プロンプトに従って Azure アカウントにログインします。

11. サインインが完了すると、[既存の Web アプリケーションを選択] ダイアログ ボックスが表示されます。 クリックして **新しい**します。

    ![DeployNewWebsite]  

12.  **Web アプリ名** フィールドで、一意のアプリ名のプレフィックスを指定します。 完全修飾 web アプリ名になります *& lt; プレフィックス >*. azurewebsites.net します。 また、構成、 **App Service プラン**, 、**リソース グループ**, 、および **地域** フィールドです。 クリックして **作成**します。

    ![DeploySiteSettings]

13. [Web の発行] ダイアログに、新しい Web アプリケーションの設定が入力されます。 クリックして **発行**します。

    ![DeployPublishSite]

    Visual Studio で Azure Web アプリケーションへの発行が完了すると、デスクトップ ブラウザーが開き、ライブ Web アプリケーションが表示されます。

14. モバイル ブラウザー エミュレーターを起動、URL をコピーします
会議アプリケーション (*<prefix>*. azurewebsites.net) には、エミュレーター、およびをクリックし、
右上のボタンをクリックして **[Browse by tag**します。 Internet
既定のブラウザーとして explorer 11 だけ入力する必要が `F12`, 、し、
`Ctrl+8`, 、ブラウザー プロファイルを変更して **Windows Phone**します。 この
下の例の画像、 *AllTags* (選択から縦モードのビュー
**タグによってブラウズ**)。

    ![AllTags]

>[AZURE.NOTE] Visual Studio の中からでも MVC 5 アプリケーションをデバッグできますが、Azure に再度、モバイル ブラウザーまたはブラウザー エミュレーターから直接、ライブ web アプリケーションを検証する、web アプリケーションを発行できます。

モバイル デバイス上でも読みやすい表示になっています。 既にことができます。
ブートス トラップ CSS フレームワークによって適用された視覚効果をいくつかを参照してください。
クリックして、 **ASP.NET** リンクします。

![SessionsByTagASP.NET]

ASP.NET タグ ビューは、ブートス トラップによって、画面に合わせてズーム
自動的にします。 ただし、このビューを最適な状態を調整することができます。
モバイル ブラウザーです。 たとえば、 **日付** 列はにくく
参照してください。 変更、チュートリアルの後半で、 *AllTags* ビューを
モバイル対応します。

##<a name="bkmk_bootstrap"></a> ブートストラップ CSS フレームワーク

MVC 5 の新機能の 1 つに、ブートストラップの標準サポートがあります。 あります。
先ほど確認した方法でさまざまなビューがすぐに向上、
想定しています。 たとえば、上部にあるナビゲーション バーが自動的には
ブラウザーの幅が小さい場合に折りたたむことができます。 デスクトップ ブラウザーで
ブラウザー ウィンドウのサイズを変更して、ナビゲーション バーがどのように変化するかを参照してください。
ルック アンド フィールです。 これが、ブートストラップに組み込まれたレスポンシブ Web デザイン
です。

ブートストラップを使用しない場合の Web アプリケーションの外観を表示するには、
*App\_Start\\BundleConfig.cs* とを含む行をコメント アウト
*bootstrap.js* と *bootstrap.css*します。 次のコードは、変更後の `RegisterBundles` メソッド
2 つのステートメント、 `RegisterBundles` メソッドは、変更後。

     bundles.Add(new ScriptBundle("~/bundles/bootstrap").Include(
              //"~/Scripts/bootstrap.js",
              "~/Scripts/respond.js"));

    bundles.Add(new StyleBundle("~/Content/css").Include(
              //"~/Content/bootstrap.css",
              "~/Content/site.css"));

`Ctrl+F5` キーを押してアプリケーションを実行します。

折りたたみ可能なナビゲーション バーが、通常今すぐ確認します。
記号付きリストです。 クリックして **[Browse by tag** 再度] をクリックし、 **ASP.NET**します。
モバイル エミュレーター ビューが表示されなくされるようになりました
画面に合わせてズームし、表示するために横方向にスクロールする必要があります。
テーブルの右側にあります。

![SessionsByTagASP.NETNoBootstrap]

変更内容を元に戻すし、モバイル ブラウザーを更新
モバイル対応画面が復元されたことを確認します。

ASP.NET MVC 5 固有のブートス トラップではありませんし、利用できます。
あらゆる web アプリケーションでこれらの機能です。 しかし、
ASP.NET MVC 5 プロジェクト テンプレートを使用、MVC 5 Web アプリケーションが実行できるように
ブートス トラップの既定で利用できます。

ブートストラップの詳細については、
[Bootstrap][BootstrapSite] サイトです。

次のセクションでは、モバイル ブラウザー専用ビューを作成する方法について
説明します。

##<a name="bkmk_overrideviews"></a> ビュー、レイアウト、および部分ビューをオーバーライドする

(レイアウトと部分ビューを含む) 任意のビューをオーバーライドすることができます。
モバイル ブラウザー全般、個々 のモバイル ブラウザーまたはのいずれか
特定のブラウザーです。 モバイル専用ビューを用意するには、ビューをコピーすることができます。
ファイルし、追加 *します。モバイル* ファイル名にします。 たとえば、モバイル
*インデックス* ビューをコピーできます *Views\\Home\\Index.cshtml* に
*Views\\Home\\Index.Mobile.cshtml*します。

このセクションでは、モバイル専用のレイアウト ファイルを作成します。

最初に、コピー *Views\\Shared\\\_Layout.cshtml* に
*Views\\Shared\\\_Layout.Mobile.cshtml*します。 開いている *\_Layout.Mobile.cshtml*
タイトルを変更および **MVC5 Application** に **MVC5 Application
(Mobile)**します。

各 `Html.ActionLink` ナビゲーション バーの呼び出し、"Browse by"をそれぞれのリンクを削除します。
*ActionLink*します。 次のコードは、モバイル レイアウト ファイルの完成した `<ul class="nav navbar-nav">` タグです。

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

コピー、 *Views\\Home\\AllTags.cshtml* ファイル
*Views\\Home\\AllTags.Mobile.cshtml*します。 新しいファイルを開き、次のように
`<h2>` 要素を"Tags"から"Tags (M)"。

    <h2>Tags (M)</h2>

デスクトップ ブラウザーを使用して、およびモバイル ブラウザーを使用してタグ ページを参照します。
Microsoft Azure ストレージ エミュレーターが起動されます。 モバイル ブラウザー エミュレーターは、2 つの変更を示しています。
(タイトル *\_Layout.Mobile.cshtml* とタイトル
*AllTags.Mobile.cshtml*)。

![AllTagsMobile_LayoutMobile]

これに対し、デスクトップでの表示は変わっていません (タイトル *\_Layout.cshtml* と 
*AllTags.cshtml*)。

![AllTagsMobile_LayoutMobileDesktop]

##<a name="bkmk_browserviews"></a> ブラウザー専用のビューを作成する

モバイル専用やデスクトップ専用のビューだけでなくすることができます。
個別のブラウザーに対してビューを作成します。 たとえば、作成することができます。
iPhone や Android ブラウザー専用のビューです。 このセクションの
iPhone ブラウザーと iPhone バージョンのレイアウトを作成します
 *AllTags* 表示します。

開いている、 *Global.asax* ファイルし、の下位に次のコードを追加します
`Application_Start` メソッド。

    DisplayModeProvider.Instance.Modes.Insert(0, new DefaultDisplayMode("iPhone")
    {
        ContextCondition = (context => context.GetOverriddenUserAgent().IndexOf
            ("iPhone", StringComparison.OrdinalIgnoreCase) >= 0)
    });

このコードは、照合される"iPhone"という表示モードを定義します。
に対する受信要求ごとにします。 受信要求と一致する場合、
(つまり、ユーザー エージェント文字列を格納する場合に定義した条件
ASP.NET MVC は 名前に "iPhone" サフィックスを含むビューを
探します。

>[AZURE.NOTE] モバイル ブラウザー専用の表示モードを追加するには、iPhone や Android などのような場合は、最初の引数を設定することを確認して `0` (一覧の先頭に挿入)、ブラウザー専用のモードよりも優先モバイル テンプレートを確認する (* します。*.Mobile.cshtml)。 逆にモバイル テンプレートをリストの一番上に配置すると、目的の表示モードに優先して選択されます (最初に一致したエントリが選択されますが、モバイル テンプレートはすべてのモバイル ブラウザーで一致します)。 

コードを右クリックし `DefaultDisplayMode`, 、選択 **解決**, と
選択し、 `using System.Web.WebPages;`です。 `DisplayModeProvider` 型と
`System.Web.WebPages` されている名前空間、
`DisplayModeProvider`  `DefaultDisplayMode` 型が定義されています。

![ResolveDefaultDisplayMode]

別の方法として、単純にファイルの `using` セクションに、
`using` ファイルのセクションです。

    using System.Web.WebPages;

変更を保存します。 ファイル
*Views\\Shared\\\_Layout.Mobile.cshtml* ファイル
*Views\\Shared\\\_Layout.iPhone.cshtml*します。 新しいファイルを開く
タイトルを変更および `MVC5 Application (Mobile)` に
`MVC5 Application (iPhone)`

コピー、 *Views\\Home\\AllTags.Mobile.cshtml* ファイル
*Views\\Home\\AllTags.iPhone.cshtml*します。 新しいファイルに次のように変更します。
 `<h2>` 要素から"Tags (M)"から"Tags (iPhone)"です。

アプリケーションを実行します。 モバイル ブラウザー エミュレーターを実行、そのユーザーの確認
エージェントが"iPhone"に設定を見つけて、 *AllTags* 表示します。 する場合
Internet Explorer 11 F12 開発者ツールでは、エミュレーターの使用
次のエミュレーションを構成します。

-   ブラウザー プロファイル = **Windows Phone**
-   ユーザー エージェント文字列 =  **カスタム**
-   カスタム文字列 = **Apple iPhone5C1/1001.525**

次のスクリーン ショット、 *AllTags* でビューがレンダリングされた、
(これは、iPhone 5c のユーザー エージェント文字列)、カスタム ユーザー エージェント文字列と Internet Explorer 11 F12 開発者ツールのエミュレーターです。

![AllTagsIPhone_LayoutIPhone]

モバイル ブラウザーで、選択、 **スピーカー** リンクします。 ないため
モバイル ビュー (*AllSpeakers.Mobile.cshtml*)、既定のスピーカー ビュー
(*AllSpeakers.cshtml*)、モバイル レイアウト ビューを使用してレンダリング
(*\_Layout.Mobile.cshtml*)。 タイトルの下に示すように **MVC5 Application
(Mobile)** 定義された *\_Layout.Mobile.cshtml*します。

![AllSpeakers_LayoutMobile]

既定の (非モバイル) ビューの描画をグローバルに無効化にします。
設定して、モバイル レイアウト内 `RequireConsistentDisplayMode` に
`true`  *Views\\\_ViewStart.cshtml* ファイルは、次のようにします。

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

 `RequireConsistentDisplayMode` に設定されている `true`, 、モバイル レイアウト
(*\_Layout.Mobile.cshtml*) モバイル ビューだけが使用されます (つまり、
ビュー ファイルの形式は、 ***ViewName**します。*.Mobile.cshtml*)。 必要な場合があります。
設定する `RequireConsistentDisplayMode` に `true` 場合は、モバイル レイアウト
非モバイル ビューでうまく機能しません。 次のスクリーン ショットします。
どのように *スピーカー* ページが表示されるときに `RequireConsistentDisplayMode` は
設定 `true` (せずに、上部にあるナビゲーション バーに文字列"(Mobile)") です。

![AllSpeakers_LayoutMobileOverridden]

ビュー ファイルで
`RequireConsistentDisplayMode`  `false` ファイルの表示にします。 この
次のマークアップ、 *Views\\Home\\AllSpeakers.cshtml* ファイルのセット
`RequireConsistentDisplayMode`  `false`:

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All speakers";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = false;
    }

このセクションでは、モバイル レイアウトとビューを作成する方法を説明しましたし、
レイアウトとビューの iPhone などの特定のデバイスを作成する方法。
しかし、ブートス トラップ CSS フレームワークの主な利点は、
レスポンシブ レイアウトの単一のスタイル シートを適用できることを意味します。
デスクトップ、電話番号、およびタブレットの一貫した外観を作成する各ブラウザー間で、
ようになりました。 次のセクションでは、ブートス トラップの作成を利用する方法を見ていきます
モバイル対応ビューです。

##<a name="bkmk_Improvespeakerslist"></a> スピーカー一覧を強化する

いま見たように、 *スピーカー* ビューも読み取れますが、リンクが
小さいと、モバイル デバイスではタップが困難です。 このセクションの
 *AllSpeakers* ビューをモバイル対応を表示します。
大規模な tap 簡単リンクおよびをすばやく検索する検索ボックスが含まれています
スピーカーです。

スタイルをブートス トラップの [リンクされたリスト グループ] を使用することができます。
向上させる、 *スピーカー* 表示します。  *Views\\Home\\AllSpeakers.cshtml*,、
Razor ファイルの内容を次のコードに置き換えます。

     @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, "SessionsBySpeaker", new { speaker }, new { @class = "list-group-item" })
        }
    </div>

 `class="list-group"` 属性、 `<div>` タグが適用される、
ブートス トラップのリストのスタイリングが適用され、 `class="input-group-item"` 属性
ブートストラップのリスト項目のスタイリングが各リンクに適用されます。

モバイル ブラウザーの表示を更新します。 更新されたビューは次のようになります。

![AllSpeakersFixed]

の [リンクされたリスト グループ] のブートス トラップのスタイリングが各ボックス全体をにより、します。
クリック可能になり、これより優れたユーザー エクスペリエンスが大幅にリンクします。 切り替えて、
デスクトップを表示し、一貫したルック アンド フィールを確認します。

![AllSpeakersFixedDesktop]

にくくなっていますモバイル ブラウザー ビューは強化されましたが、
スピーカーの長い一覧を移動します。 ブートス トラップが用意されていません、
検索フィルター機能の「-既製がに追加することができます、
わずか数行のコードです。 最初のビューにこの検索ボックスを追加するは
フィルター関数の JavaScript コードにフックします。 イン
*Views\\Home\\AllSpeakers.cshtml*, 、追加の \ < form\ > タグの直後、\ < h2\ > タグで次のようにします。

    @model IEnumerable<string>

    @{
        ViewBag.Title = "All Speakers";
    }

    <h2>Speakers</h2>

    <form class="input-group">
        <span class="input-group-addon"><span class="glyphicon glyphicon-search"></span></span>
        <input type="text" class="form-control" placeholder="Search speaker">
    </form>
    <br />
    <div class="list-group">
        @foreach (var speaker in Model)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class = "list-group-item" })
        }
    </div>

注意して、 `<form>` と `<input>` タグの両方にブートス トラップがあります。
スタイルを適用します。  `<span>` 要素は、ブートス トラップ
[glyphicon]から、
と入力します。

 *スクリプト* フォルダーと呼ばれる JavaScript ファイルを追加 *filter.js*します。 開く
ファイルとそこに次のコードを貼り付けます。

    $(function () {

        // reset the search form when the page loads
        $("form").each(function () {
            this.reset();
        });

        // wire up the events to the <input> element for search/filter
        $("input").bind("keyup change", function () {
            var searchtxt = this.value.toLowerCase();
            var items = $(".list-group-item");

            // show all speakers that begin with the typed text and hide others
            for (var i = 0; i < items.length; i++) {
                var val = items[i].text.toLowerCase();
                val = val.substring(0, searchtxt.length);
                if (val == searchtxt) {
                    $(items[i]).show();
                }
                else {
                    $(items[i]).hide();
                }
            }
        });
    });

さらに、登録済みのバンドルに filter.js を含める必要があります。 開く
*App\_Start\\BundleConfig.cs* 、最初のバンドルを変更します。 変更、
最初 `bundles.Add` ステートメント (の **jquery** バンドル) を含める
*Scripts\\filter.js*, 、次のようにします。

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

 **Jquery** バンドルは、既定によって既に描画 *\_Layout*
されています。 後で、同じ JavaScript コードを利用して、フィルター機能を
他のリスト ビューに適用できます。

モバイル ブラウザーを更新し、 *AllSpeakers* 表示します。 [
検索ボックスに、「sc」と入力します。 スピーカー一覧がフィルターようになりました
検索文字列に従っています。

![AllSpeakersFixedSearchBySC]

##<a name="bkmk_improvetags"></a> タグ一覧を強化する

ように、 *スピーカー* ビュー、 *タグ* ビューも読み取れますが、リンクが、
リンクが小さく、モバイル デバイスではタップが困難です。 解決するには、 *タグ* と同じように表示、 *スピーカー* を表示する場合は、次のコード変更を使用して、 `Html.ActionLink` メソッドの構文 *Views\\Home\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

デスクトップ ブラウザーを更新すると、次のように表示されます。

![AllTagsFixedDesktop]

モバイル ブラウザーを更新すると、次のように表示されます。 

![AllTagsFixed]

>[AZURE.NOTE] 場合、元の一覧書式設定、モバイル ブラウザーでがまだ存在していないブートス トラップのスタイリングをどのように見えますが、これは先ほどモバイル専用のビューを作成します。 ここではブートストラップ CSS フレームワークを使用してレスポンシブ Web デザインを作成するため、これらのモバイル専用のビューとモバイル専用のレイアウト ビューを削除してください。 その後、モバイル ブラウザーを更新すると、ブートストラップのスタイリングが表示されます。

##<a name="bkmk_improvedates"></a> 日付一覧を強化する

向上させることができます、 *日付* 表示を改善する、 *スピーカー* と
*タグ* 前に、次のように説明されているコードの変更を使用する場合に表示 `Html.ActionLink` メソッドの構文 *Views\\Home\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

モバイル ブラウザー ビューを更新すると、次のように表示されます。

![AllDatesFixed]

さらに向上することができます、 *日付* 、日付と時刻を整理することによってビュー
高めることができます。 これには、ブートストラップの
[パネル]スタイルを設定します。 *Views\\Home\\AllDates.cshtml*
内容、 *Views\\Home\\AllDates.cshtml* ファイルと、
置き換えます。

    @model IEnumerable<DateTime>

    @{
        ViewBag.Title = "All Dates";
    }

    <h2>Dates</h2>

    @foreach (var dategroup in Model.GroupBy(x=>x.Date))
    {
        <div class="panel panel-primary">
            <div class="panel-heading">
                @dategroup.Key.ToString("ddd, MMM dd")
            </div>
            <div class="panel-body list-group">
                @foreach (var date in dategroup)
                {
                    @Html.ActionLink(date.ToString("h:mm tt"), 
                                     "SessionsByDate", 
                                     new { date }, 
                                     new { @class = "list-group-item" })
                }
            </div>
        </div>
    }

このコードでは、個別 `<div class="panel panel-primary">` のタグ付け
一覧で、日付ごとの [リンクされたリスト グループ] を使用して、
を使用します。 このコードを実行すると、モバイル ブラウザーの表示は
次のようになります。

![AllDatesFixed2]

デスクトップ ブラウザーに切り替えます。 モバイル ブラウザーと一貫性を保った画面が表示されます。

![AllDatesFixed2Desktop]

##<a name="bkmk_improvesessionstable"></a> SessionsTable ビューを強化する

このセクションで、 *SessionsTable* の詳細を表示
モバイル対応します。 変更箇所は、これまでに作成した他のビューよりも広範囲にわたります。

モバイル ブラウザーでは、タップ、 **タグ** ] ボタンをクリックし、入力 `asp` で、
と入力します。

![AllTagsFixedSearchByASP]

タップして、 **ASP.NET** リンクします。

![SessionsTableTagASP.NET]

ご覧のとおりは形式で表示されているテーブル
デスクトップ ブラウザーで表示するには、この設計されています。 ただし、少しは
モバイル ブラウザーで読み取るには困難です。 これを修正するために、
*Views\\Home\\SessionsTable.cshtml* の内容を置き換えると、
次のコードを含むファイル:

    @model IEnumerable<Mvc5Mobile.Models.Session>

    <h2>@ViewBag.Title</h2>

    <div class="container">
        <div class="row">
            @foreach (var session in Model)
            {
                <div class="col-md-4">
                    <div class="list-group">
                        @Html.ActionLink(session.Title, 
                                         "SessionByCode", 
                                         new { session.Code }, 
                                         new { @class="list-group-item active" })
                        <div class="list-group-item">
                            <div class="list-group-item-text">
                                @Html.Partial("_SpeakersLinks", session)
                            </div>
                            <div class="list-group-item-info">
                                @session.DateText
                            </div>
                            <div class="list-group-item-info small hidden-xs">
                                @Html.Partial("_TagsLinks", session)
                            </div>
                        </div>
                    </div>
                </div>
            }
        </div>
    </div>

このコードでは、次の 3 つの処理を行います。

-   ブートス トラップの [カスタムのリンクされたリスト グループ] を使用してください。
    セッション情報を縦方向に書式設定できるようにこの
    情報は、(リスト グループ項目テキストなどのクラスを使用して) モバイル ブラウザーで読み取れる
-   [グリッド システム] からを適用します
    レイアウト、セッション項目が、デスクトップで水平方向にフローできるように
    ブラウザーと垂直方向に、モバイル ブラウザーで (クラスを使用して、col-md-4)
-   [レスポンシブ ユーティリティ] からの使用します。
    (hidden-xs クラスを使用して) モバイル ブラウザーで表示すると、セッション タグを非表示にします。

タイトル リンクをタップして、それぞれのセッションに移動することもできます。 イメージ
次のコードの変更が反映されます。

![FixedSessionsByTag]

ユーザーが自動的に適用したブートス トラップのグリッド システムによって、
モバイル ブラウザーの縦方向にセッションです。 さらに、タグ
表示されません。 デスクトップ ブラウザーに切り替えます。

![SessionsTableFixedTagASP.NETDesktop]

デスクトップ ブラウザーでは、タグが表示されています。 また、適用したブートストラップのグリッド システムによって、
セッション項目が 2 列に表示されています。 拡大するかどうかは、
ブラウザー、配置が 3 つの列に変わることが表示されます。

##<a name="bkmk_improvesessionbycode"></a> ISessionByCode ビューを強化する

最後に、修正、 *SessionByCode* ビューをモバイル対応します。

モバイル ブラウザーでは、タップ、 **タグ** ] ボタンをクリックし、入力 `asp` で、
と入力します。

![AllTagsFixedSearchByASP]

タップして、 **ASP.NET** リンクします。 ASP.NET タグのセッションが表示されます。

![FixedSessionsByTag]

選択、 **ASP.NET でのシングル ページ アプリケーションを構築し、
AngularJS** リンクします。

![SessionByCode3 644]

既定のデスクトップ ビューでも問題はありませんが、ブートストラップの GUI コンポーネントを使用して簡単に表示品質を高めることができます。

開いている *Views\\Home\\SessionByCode.cshtml* と内容に置き換えます
マークアップと置き換えます。

    @model Mvc5Mobile.Models.Session

    @{
        ViewBag.Title = "Session details";
    }
    <h3>@Model.Title (@Model.Code)</h3>
    <p>
        <strong>@Model.DateText</strong> in <strong>@Model.Room</strong>
    </p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Speakers
        </div>
        @foreach (var speaker in Model.Speakers)
        {
            @Html.ActionLink(speaker, 
                             "SessionsBySpeaker", 
                             new { speaker }, 
                             new { @class="panel-body" })
        }
    </div>

    <p>@Model.Abstract</p>

    <div class="panel panel-primary">
        <div class="panel-heading">
            Tags
        </div>
        @foreach (var tag in Model.Tags)
        {
            @Html.ActionLink(tag, 
                             "SessionsByTag", 
                             new { tag }, 
                             new { @class = "panel-body" })
        }
    </div>

新しいマークアップでは、ブートストラップのパネル スタイリングによりモバイル ビューが強化されています。 

モバイル ブラウザーの表示を更新します。 次の図では、行ったコードの変更が
反映されています。

![SessionByCodeFixed3 644]

## まとめ

このチュートリアルでは、ASP.NET MVC 5 を使用して モバイル対応の Web アプリケーションを
開発する方法を学習しました。 学習した内容は次のとおりです。

-   ASP.NET MVC 5 アプリケーションを App Service の Web アプリケーションに展開する
-   ブートストラップを使用した MVC 5 アプリケーションでのレスポンシブな
    Web レイアウトの作成
-   ビュー全般や個別のビューのレイアウト、ビュー、および部分
    ビューのオーバーライド
-   `RequireConsistentDisplayMode` プロパティを使用した、レイアウトや部分的な
    `RequireConsistentDisplayMode` プロパティ
-   iPhone ブラウザーなど、特定のブラウザー専用のビュー
    の作成
-   Razor コードでのブートストラップのスタイリングの適用

## 関連項目

-   [レスポンシブ Web デザインの 9 つの基本原則](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
-   [Bootstrap][BootstrapSite]
-   [ブートス トラップの公式ブログ][]
-   [Tutorial Republic による twitter Bootstrap Tutorial][]
-   [ブートス トラップのプレイ グラウンド][]
-   [W3c 勧告: モバイル Web アプリケーション ベスト プラクティス][]
-   [メディア クエリに関する W3C Candidate Recommendation][]

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- Internal Links -->
[Deploy the starter project to an Azure web app]: #bkmk_DeployStarterProject
[Bootstrap CSS Framework]: #bkmk_bootstrap
[Override the Views, Layouts, and Partial Views]: #bkmk_overrideviews
[Create Browser-Specific Views]:#bkmk_browserviews
[Improve the Speakers List]: #bkmk_Improvespeakerslist
[Improve the Tags List]: #bkmk_improvetags
[Improve the Dates List]: #bkmk_improvedates
[Improve the SessionsTable View]: #bkmk_improvesessionstable
[Improve the SessionByCode View]: #bkmk_improvesessionbycode

<!-- External Links -->
[Visual Studio Express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web
[Visual Studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs
[AzureSDKVs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409
[Fiddler]: http://www.fiddler2.com/fiddler2/
[EmulatorIE11]: http://msdn.microsoft.com/library/ie/dn255001.aspx
[EmulatorChrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation
[EmulatorOpera]: http://www.opera.com/developer/tools/mobile/
[StarterProject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409
[CompletedProject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409
[BootstrapSite]: http://getbootstrap.com/
[WebPIAzureSdk23NetVS13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png
[linked list group]: http://getbootstrap.com/components/#list-group-linked
[glyphicon]: http://getbootstrap.com/components/#glyphicons
[panels]: http://getbootstrap.com/components/#panels
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content
[grid system]: http://getbootstrap.com/css/#grid
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities
[Official Bootstrap Blog]: http://blog.getbootstrap.com/
[Twitter Bootstrap Tutorial from Tutorial Republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/
[The Bootstrap Playground]: http://www.bootply.com/
[W3C Recommendation Mobile Web Application Best Practices]: http://www.w3.org/TR/mwabp/
[W3C Candidate Recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/

<!-- Images -->
[DeployClickPublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png
[DeployClickWebSites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png
[DeploySignIn]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png
[DeployUsername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png
[DeployPassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png
[DeployNewWebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png
[DeploySiteSettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png
[DeployPublishSite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png
[MobileHomePage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png
[FixedSessionsByTag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png
[AllTags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png
[SessionsByTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png
[SessionsByTagASP.NETNoBootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png
[AllTagsMobile_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png
[AllTagsMobile_LayoutMobileDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png
[ResolveDefaultDisplayMode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png
[AllTagsIPhone_LayoutIPhone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png
[AllSpeakers_LayoutMobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png
[AllSpeakers_LayoutMobileOverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png
[AllSpeakersFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png
[AllSpeakersFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png
[AllSpeakersFixedSearchBySC]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png
[AllTagsFixedDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[AllTagsFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png
[AllDatesFixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png
[AllDatesFixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png
[AllDatesFixed2Desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png
[AllTagsFixedSearchByASP]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png
[SessionsTableTagASP.NET]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png
[SessionsTableFixedTagASP.NETDesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png
[SessionByCode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png
[SessionByCodeFixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png
 

