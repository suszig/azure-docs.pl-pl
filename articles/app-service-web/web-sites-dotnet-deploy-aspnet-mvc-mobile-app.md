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
[Visual Studio Express 2013 for Web が ][visual studio express 2013]
または、Visual Studio のプロフェッショナル エディションを既にお持ちの場合は
はそれを使用することもできます。 [Visual Studio 2015] を使用することができますが、スクリーン ショットは、ASP.NET 4.x テンプレートを使用する必要があります。

[AZURE.INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

## 作成するアプリケーション:

このチュートリアルでは、単純なものにモバイル機能を追加します
会議一覧アプリケーションで提供されている、 [スタート プロジェクト ][starterproject]します。 次のスクリーン ショットは、ASP.NET セッションを完了しました。
アプリケーションに Internet Explorer 11 F12 のブラウザー エミュレーターで表示
開発者ツール。

![][fixedsessionsbytag]

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

1. Azure SDK for .NET をインストールするには、次のリンクをクリックします。 Visual Studio 2013 をまだインストールしていない場合は、次のリンクをクリックするとインストールされます。 このチュートリアルには、Visual Studio 2013 が必要です。 [Azure SDK for Visual Studio 2013 ][azuresdkvs2013]
1. Web Platform Installer のウィンドウで、**[インストール]** をクリックし、インストールの手順を進めます。

モバイル ブラウザー エミュレーターも必要です。 次のいずれでも動作
します。

-   ブラウザー エミュレーター [Internet Explorer 11 F12 開発者ツール ][emulatorie11] (すべてのモバイル場合に使用します。
    のスクリーン ショットで使用しています)。 Windows Phone 8、Windows Phone 7、Apple iPad のエージェント文字列のプリセットが搭載されています。
-   ブラウザー エミュレーター [Google Chrome DevTools ][emulatorchrome]します。 多数の Android デバイスに加え、Apple iPhone、Apple iPad、Amazon Kindle Fire 用のプリセットが含まれています。 タッチイベントのエミュレーションにも対応します
-   [元の opera Mobile Emulator ][emulatoropera]

次のトピック用に C\# のソース コードを含む Visual Studio プロジェクトが
用意されています。

-   [スタート プロジェクトのダウンロード ][starterproject]
-   [完成したプロジェクトのダウンロード ][completedproject]

## <a name="bkmk_DeployStarterProject"></a>Azure web アプリにスタート プロジェクトをデプロイします。

1.  会議一覧アプリケーションをダウンロード [スタート プロジェクト ][starterproject]します。

2.  Windows エクスプローラーで MvcMobileStarterBeta.zip ファイルを右クリックし、*[プロパティ]* をクリックします。

3.  **Mvc5Mobile.zip のプロパティ** ] ダイアログ ボックス
選択、 **ブロックを解除する** ] ボタンをクリックします。 (ブロックを解除すると、Web からダウンロード
使用しようとする際に、 *.zip* ダウンロードしたファイルを
セキュリティに関する警告を回避できます)。

4.  右クリックし、 *Mvc5Mobile.zip* ファイルおよび選択した **すべて展開** に
ファイルを解凍します。

5.  Visual Studio で *Mvc5Mobile.sln* ファイルを開きます。

6.  ソリューション エクスプローラで目的のプロジェクトを右クリックし、**[発行]** をクリックします。

    ![][deployclickpublish]

7.  [Web の発行] で、**[Microsoft Azure Web アプリケーション]** をクリックします。

    ![][deployclickwebsites]

8.  **[サインイン]** をクリックします。

    ![][deploysignin]

9.  プロンプトに従って Azure アカウントにログインします。

11. サインインが完了すると、[既存の Web アプリケーションを選択] ダイアログ ボックスが表示されます。 **[新規]** をクリックします。

    ![][deploynewwebsite]

12. **[Web アプリ名]** フィールドで、一意のアプリ名のプレフィックスを指定します。 完全修飾 web アプリ名になります *< プレフィックス >*. azurewebsites.net します。 また、**[App Service プラン]**、**[リソース グループ]**、**[リージョン]** の各フィールドを構成します。 **[作成]** をクリックします。

    ![][deploysitesettings]

13. [Web の発行] ダイアログに、新しい Web アプリケーションの設定が入力されます。 **[発行]** をクリックします。

    ![][deploypublishsite]

    Visual Studio で Azure Web アプリケーションへの発行が完了すると、デスクトップ ブラウザーが開き、ライブ Web アプリケーションが表示されます。

14. モバイル ブラウザー エミュレーターを起動、URL をコピーします
会議アプリケーション (*<prefix>*. azurewebsites.net) には、エミュレーター、およびをクリックし、
右上のボタンをクリックして **[Browse by tag**します。 Internet
既定のブラウザーとして explorer 11 だけ入力する必要が `F12`, 、し、
`Ctrl + 8`, 、ブラウザー プロファイルを変更 **Windows Phone**します。 この
下の例の画像、 *AllTags* (選択から縦モードのビュー
**[Browse by tag**)。

    ![][alltags]

>[AZURE.NOTE] Visual Studio の中からでも MVC 5 アプリケーションをデバッグできますが、Web アプリケーションを Microsoft Azure に再度発行すると、モバイル ブラウザーまたはブラウザー エミュレーターからライブ Web アプリケーションを直接確認できます。

モバイル デバイス上でも読みやすい表示になっています。 既にことができます。
ブートス トラップ CSS フレームワークによって適用された視覚効果をいくつかを参照してください。
**[ASP.NET]** リンクをクリックします。

![][sessionsbytagasp.net]

ASP.NET タグ ビューは、ブートス トラップによって、画面に合わせてズーム
自動的にします。 ただし、このビューを最適な状態を調整することができます。
モバイル ブラウザーです。 たとえば、 **日付** 列はにくく
参照してください。 変更、チュートリアルの後半で、 *AllTags* ビューを
モバイル対応します。

## <a name="bkmk_bootstrap"></a> ブートス トラップ CSS フレームワーク

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

`Ctrl キーを押しながら F5 キーを押して`アプリケーションを実行します。

折りたたみ可能なナビゲーション バーが、通常今すぐ確認します。
記号付きリストです。 **[Browse by tag]** を再度クリックし、**[ASP.NET]** をクリックします。
モバイル エミュレーター ビューが表示されなくされるようになりました
画面に合わせてズームし、表示するために横方向にスクロールする必要があります。
テーブルの右側にあります。

![][sessionsbytagasp.netnobootstrap]

変更内容を元に戻すし、モバイル ブラウザーを更新
モバイル対応画面が復元されたことを確認します。

ASP.NET MVC 5 固有のブートス トラップではありませんし、利用できます。
あらゆる web アプリケーションでこれらの機能です。 しかし、
ASP.NET MVC 5 プロジェクト テンプレートを使用、MVC 5 Web アプリケーションが実行できるように
ブートス トラップの既定で利用できます。

ブートストラップの詳細については、
[ブートス トラップ ][bootstrapsite] サイトです。

次のセクションでは、モバイル ブラウザー専用ビューを作成する方法について
説明します。

## <a name="bkmk_overrideviews"></a> ビュー、レイアウト、および部分ビューをオーバーライドします。

(レイアウトと部分ビューを含む) 任意のビューをオーバーライドすることができます。
モバイル ブラウザー全般、個々 のモバイル ブラウザーまたはのいずれか
特定のブラウザーです。 モバイル専用ビューを用意するには、ビューをコピーすることができます。
ファイルし、追加 *します。モバイル* ファイル名にします。 たとえば、モバイル
*インデックス* ビューをコピーできます *Views\\Home\\Index.cshtml* に
*Views\\Home\\Index.Mobile.cshtml*します。

このセクションでは、モバイル専用のレイアウト ファイルを作成します。

最初に、コピー *Views\\Shared\\\_Layout.cshtml* に
*Views\\Shared\\\_Layout.Mobile.cshtml*します。 開いている *\_Layout.Mobile.cshtml*
タイトルを変更および **MVC5 Application** に * * MVC5 Application
(Mobile) * *。

各 `Html.ActionLink` ナビゲーション バーの呼び出し、"Browse by"をそれぞれのリンクを削除します。
*ActionLink*します。次のコードは、完了した `< ul クラス =「nav ナビゲーション バーのナビゲーション」>` モバイル レイアウト ファイルのタグ。

    <ul class="nav navbar-nav">
        <li>@Html.ActionLink("Home", "Index", "Home")</li>
        <li>@Html.ActionLink("Date", "AllDates", "Home")</li>
        <li>@Html.ActionLink("Speaker", "AllSpeakers", "Home")</li>
        <li>@Html.ActionLink("Tag", "AllTags", "Home")</li>
    </ul>

コピー、 *Views\\Home\\AllTags.cshtml* ファイル
*Views\\Home\\AllTags.Mobile.cshtml*します。 新しいファイルを開き、次のように
`< h2 >` 要素を"Tags"から"Tags (M)"。

    <h2>Tags (M)</h2>

デスクトップ ブラウザーを使用して、およびモバイル ブラウザーを使用してタグ ページを参照します。
Microsoft Azure ストレージ エミュレーターが起動されます。 モバイル ブラウザー エミュレーターは、2 つの変更を示しています。
(タイトル *\_Layout.Mobile.cshtml* とタイトル
*AllTags.Mobile.cshtml*)。

![][alltagsmobile_layoutmobile]

これに対し、デスクトップでの表示は変わっていません (タイトル *\_Layout.cshtml* と 
*AllTags.cshtml*)。

![][alltagsmobile_layoutmobiledesktop]

## <a name="bkmk_browserviews"></a> ブラウザー専用ビューを作成します。

モバイル専用やデスクトップ専用のビューだけでなくすることができます。
個別のブラウザーに対してビューを作成します。 たとえば、作成することができます。
iPhone や Android ブラウザー専用のビューです。 このセクションの
iPhone ブラウザーと iPhone バージョンのレイアウトを作成します
*AllTags* 表示します。

開いている、 *Global.asax* ファイルし、の下位に次のコードを追加します
`Application_Start` メソッドです。

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
選択し、 `using System.web.webpages;'`します。 `DisplayModeProvider` 型と
`System.Web.WebPages` されている名前空間、
`DisplayModeProvider` と `DefaultDisplayMode` 型が定義されています。

![][resolvedefaultdisplaymode]

別の方法として、単純にファイルの `using` セクションに、
`を使用して` ファイルのセクションです。

    using System.Web.WebPages;

変更を保存します。 ファイル
*Views\\Shared\\\_Layout.Mobile.cshtml* ファイル
*Views\\Shared\\\_Layout.iPhone.cshtml*します。 新しいファイルを開く
タイトルを変更および `MVC5 Application (Mobile)` に
`MVC5 Application (iPhone)`します。

コピー、 *Views\\Home\\AllTags.Mobile.cshtml* ファイル
*Views\\Home\\AllTags.iPhone.cshtml*します。 新しいファイルに次のように変更します。
`< h2 >` 要素から"Tags (M)"から"Tags (iPhone)"です。

アプリケーションを実行します。 モバイル ブラウザー エミュレーターを実行、そのユーザーの確認
エージェントが"iPhone"に設定を見つけて、 *AllTags* 表示します。 する場合
Internet Explorer 11 F12 開発者ツールでは、エミュレーターの使用
次のエミュレーションを構成します。

-   [ブラウザー プロファイル] = **[Windows Phone]**
-   [ユーザー エージェント文字列] = **[カスタム]**
-   [カスタム文字列] = 「**Apple-iPhone5C1/1001.525**」

次のスクリーン ショット、 *AllTags* でビューがレンダリングされた、
(これは、iPhone 5c のユーザー エージェント文字列)、カスタム ユーザー エージェント文字列と Internet Explorer 11 F12 開発者ツールのエミュレーターです。

![][alltagsiphone_layoutiphone]

モバイル ブラウザーで **[Speakers]** リンクをタップします。 ないため
モバイル ビュー (*AllSpeakers.Mobile.cshtml*)、既定のスピーカー ビュー
(*AllSpeakers.cshtml*)、モバイル レイアウト ビューを使用してレンダリング
(*\_Layout.Mobile.cshtml*)。 タイトルの下に示すように * * MVC5 Application
(Mobile)** *\_Layout.Mobile.cshtml で定義された*します。

![][allspeakers_layoutmobile]

既定の (非モバイル) ビューの描画をグローバルに無効化にします。
設定して、モバイル レイアウト内 `RequireConsistentDisplayMode` に
`true` で、 *Views\\\_ViewStart.cshtml* ファイルは、次のようにします。

    @{
        Layout = "~/Views/Shared/_Layout.cshtml";
        DisplayModeProvider.Instance.RequireConsistentDisplayMode = true;
    }

`RequireConsistentDisplayMode` に設定されている `true`, 、モバイル レイアウト
(*\_Layout.Mobile.cshtml*) モバイル ビューだけが使用されます (つまり、
ビュー ファイルの形式は、 *** ViewName**します。.Mobile.cshtml*)。 必要な場合があります。
設定する `RequireConsistentDisplayMode` に `true` 場合は、モバイル レイアウト
非モバイル ビューでうまく機能しません。 次のスクリーン ショットします。
どのように *スピーカー* ページが表示されるときに `RequireConsistentDisplayMode` は
設定 `true` (せずに、上部にあるナビゲーション バーに文字列"(Mobile)") です。

![][allspeakers_layoutmobileoverridden]

ビュー ファイルで
`RequireConsistentDisplayMode` に `false` ファイルの表示にします。 この
次のマークアップ、 *Views\\Home\\AllSpeakers.cshtml* ファイルのセット
`RequireConsistentDisplayMode` に `false`:

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

## <a name="bkmk_Improvespeakerslist"></a> スピーカー一覧を強化します。

いま見たように、 *スピーカー* ビューも読み取れますが、リンクが
小さいと、モバイル デバイスではタップが困難です。 このセクションの
*AllSpeakers* ビューをモバイル対応を表示します。
大規模な tap 簡単リンクおよびをすばやく検索する検索ボックスが含まれています
スピーカーです。

ブートス トラップを使用する [リスト グループのリンクされた][] にスタイル設定
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

`クラス ="list-group"` 属性、 `< div >` タグが適用される、
リストのスタイリングをブートス トラップと `クラス =「入力グループ項目」` 属性
ブートストラップのリスト項目のスタイリングが各リンクに適用されます。

モバイル ブラウザーの表示を更新します。 更新されたビューは次のようになります。

![][allspeakersfixed]

ブートス トラップ [リスト グループのリンクされた][] スタイリングごとに、全体のボックスを使用するが
クリック可能になり、これより優れたユーザー エクスペリエンスが大幅にリンクします。 切り替えて、
デスクトップを表示し、一貫したルック アンド フィールを確認します。

![][allspeakersfixeddesktop]

にくくなっていますモバイル ブラウザー ビューは強化されましたが、
スピーカーの長い一覧を移動します。 ブートス トラップが用意されていません、
検索フィルター機能の「-既製がに追加することができます、
わずか数行のコードです。 最初のビューにこの検索ボックスを追加するは
フィルター関数の JavaScript コードにフックします。 イン
*Views\\Home\\AllSpeakers.cshtml*, 、追加、\<form\> タグの直後、\<h2\> タグを次のようにします。

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

注意して、 `< フォーム >` と `< 入力 >` タグの両方にブートス トラップがあります。
スタイルを適用します。 `< Span >` 要素は、ブートス トラップ
[glyphicon[]][] to the
と入力します。

*Scripts* フォルダーで、*filter.js* という JavaScript ファイルを追加します。 開く
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
まず `バンドルします。追加` ステートメント (の **jquery** バンドル) を含める
*Scripts\\filter.js*, 、次のようにします。

     bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                "~/Scripts/jquery-{version}.js",
                "~/Scripts/filter.js"));

**Jquery** バンドルは、既定によって既に描画 *\_Layout*
されています。 後で、同じ JavaScript コードを利用して、フィルター機能を
他のリスト ビューに適用できます。

モバイル ブラウザーを情報に更新し、*AllSpeakers* ビューに移動します。 [
検索ボックスに、「sc」と入力します。 スピーカー一覧がフィルターようになりました
検索文字列に従っています。

![][allspeakersfixedsearchbysc]

## <a name="bkmk_improvetags"></a> タグ一覧を強化します。

ように、 *スピーカー* ビュー、 *タグ* ビューも読み取れますが、リンクが、
リンクが小さく、モバイル デバイスではタップが困難です。 解決するには、 *タグ* と同じように表示、 *スピーカー* を表示する場合は、次のコード変更を使用して、 `Html.ActionLink` メソッドの構文 *Views\\Home\\AllTags.cshtml*:

    @Html.ActionLink(tag, 
                     "SessionsByTag", 
                     new { tag }, 
                     new { @class = "list-group-item" })

デスクトップ ブラウザーを更新すると、次のように表示されます。

![][alltagsfixeddesktop]

モバイル ブラウザーを更新すると、次のように表示されます。

![][alltagsfixed]
>[AZURE.NOTE] モバイル ブラウザーで元の一覧書式設定が残り、ブートストラップのスタイリングが正常に機能していないように見えますが、これは先ほどモバイル専用のビューを作成したためです。 ここではブートストラップ CSS フレームワークを使用してレスポンシブ Web デザインを作成するため、これらのモバイル専用のビューとモバイル専用のレイアウト ビューを削除してください。 その後、モバイル ブラウザーを更新すると、ブートストラップのスタイリングが表示されます。

## <a name="bkmk_improvedates"></a> 日付一覧を強化します。

向上させることができます、 *日付* 表示を改善する、 *スピーカー* と
*タグ* 前に、次のように説明されているコードの変更を使用する場合に表示 `Html.ActionLink` メソッドの構文 *Views\\Home\\AllDates.cshtml*:

    @Html.ActionLink(date.ToString("ddd, MMM dd, h:mm tt"), 
                     "SessionsByDate", 
                     new { date }, 
                     new { @class = "list-group-item" })

モバイル ブラウザー ビューを更新すると、次のように表示されます。

![][alldatesfixed]

さらに向上することができます、 *日付* 、日付と時刻を整理することによってビュー
高めることができます。 これには、ブートストラップの
[panels[]][] styling. *Views\\Home\\AllDates.cshtml*
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

このコードでは、個別 `< div クラスを探して [パネル] パネル-プライマリ] = >` タグを
内に作成し、一覧の日付ごと、 [リスト グループのリンクされた][] ため、
を使用します。 このコードを実行すると、モバイル ブラウザーの表示は
次のようになります。

![][alldatesfixed2]

デスクトップ ブラウザーに切り替えます。 モバイル ブラウザーと一貫性を保った画面が表示されます。

![][alldatesfixed2desktop]

## <a name="bkmk_improvesessionstable"></a> SessionsTable ビュー

このセクションで、 *SessionsTable* の詳細を表示
モバイル対応します。 変更箇所は、これまでに作成した他のビューよりも広範囲にわたります。

モバイル ブラウザーでは、タップ、 **タグ** ] ボタンをクリックし、入力 `asp` で、
と入力します。

![][alltagsfixedsearchbyasp]

**[ASP.NET]** リンクをタップします。

![][sessionstabletagasp.net]

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

-   ブートス トラップを使用して [カスタムのリンクされたリスト グループの][]
    セッション情報を縦方向に書式設定できるようにこの
    情報は、(リスト グループ項目テキストなどのクラスを使用して) モバイル ブラウザーで読み取れる
-   適用される、 [グリッド システムの][] に、
    レイアウト、セッション項目が、デスクトップで水平方向にフローできるように
    ブラウザーと垂直方向に、モバイル ブラウザーで (クラスを使用して、col-md-4)
-   使用して、 [レスポンシブ ユーティリティ][] に
    (hidden-xs クラスを使用して) モバイル ブラウザーで表示すると、セッション タグを非表示にします。

タイトル リンクをタップして、それぞれのセッションに移動することもできます。 イメージ
次のコードの変更が反映されます。

![][fixedsessionsbytag]

ユーザーが自動的に適用したブートス トラップのグリッド システムによって、
モバイル ブラウザーの縦方向にセッションです。 さらに、タグ
表示されません。 デスクトップ ブラウザーに切り替えます。

![][sessionstablefixedtagasp.netdesktop]

デスクトップ ブラウザーでは、タグが表示されています。 また、適用したブートストラップのグリッド システムによって、
セッション項目が 2 列に表示されています。 拡大するかどうかは、
ブラウザー、配置が 3 つの列に変わることが表示されます。

## <a name="bkmk_improvesessionbycode"></a> SessionByCode ビュー

最後に、*SessionByCode* ビューをモバイル対応に修正します。

モバイル ブラウザーでは、タップ、 **タグ** ] ボタンをクリックし、入力 `asp` で、
と入力します。

![][alltagsfixedsearchbyasp]

**[ASP.NET]** リンクをタップします。 ASP.NET タグのセッションが表示されます。

![][fixedsessionsbytag]

選択して、* * ASP.NET でのシングル ページ アプリケーションを構築し、
AngularJS * * リンクです。

![][sessionbycode3-644]

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

![][sessionbycodefixed3-644]

## まとめ

このチュートリアルでは、ASP.NET MVC 5 を使用して モバイル対応の Web アプリケーションを
開発する方法を学習しました。 チェックの内容は次のとおりです

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

-   [レスポンシブ web デザインの 9 つの基本原則](http://blog.froont.com/9-basic-principles-of-responsive-web-design/)
-   [ブートス トラップ ][bootstrapsite]
-   [ブートス トラップの公式ブログ][]
-   [Tutorial Republic から、twitter Bootstrap Tutorial][]
-   [ブートス トラップのプレイ グラウンド][]
-   [W3C Recommendation モバイル Web アプリケーションのベスト プラクティス][]
-   [W3C Candidate Recommendation メディア クエリ][]

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)





[deploy the starter project to an azure web app]: #bkmk_DeployStarterProject 
[bootstrap css framework]: #bkmk_bootstrap 
[override the views, layouts, and partial views]: #bkmk_overrideviews 
[create browser-specific views]: #bkmk_browserviews 
[improve the speakers list]: #bkmk_Improvespeakerslist 
[improve the tags list]: #bkmk_improvetags 
[improve the dates list]: #bkmk_improvedates 
[improve the sessionstable view]: #bkmk_improvesessionstable 
[improve the sessionbycode view]: #bkmk_improvesessionbycode 
[visual studio express 2013]: http://www.visualstudio.com/downloads/download-visual-studio-vs#d-express-web 
[visual studio 2015]: https://www.visualstudio.com/downloads/download-visual-studio-vs 
[azuresdkvs2013]: http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409 
[fiddler]: http://www.fiddler2.com/fiddler2/ 
[emulatorie11]: http://msdn.microsoft.com/library/ie/dn255001.aspx 
[emulatorchrome]: https://developers.google.com/chrome-developer-tools/docs/mobile-emulation 
[emulatoropera]: http://www.opera.com/developer/tools/mobile/ 
[starterproject]: http://go.microsoft.com/fwlink/?LinkID=398780&clcid=0x409 
[completedproject]: http://go.microsoft.com/fwlink/?LinkID=398781&clcid=0x409 
[bootstrapsite]: http://getbootstrap.com/ 
[webpiazuresdk23netvs13]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/WebPIAzureSdk23NetVS13.png 
[linked list group]: http://getbootstrap.com/components/#list-group-linked 
[glyphicon]: http://getbootstrap.com/components/#glyphicons 
[panels]: http://getbootstrap.com/components/#panels 
[custom linked list group]: http://getbootstrap.com/components/#list-group-custom-content 
[grid system]: http://getbootstrap.com/css/#grid 
[responsive utilities]: http://getbootstrap.com/css/#responsive-utilities 
[official bootstrap blog]: http://blog.getbootstrap.com/ 
[twitter bootstrap tutorial from tutorial republic]: http://www.tutorialrepublic.com/twitter-bootstrap-tutorial/ 
[the bootstrap playground]: http://www.bootply.com/ 
[w3c recommendation mobile web application best practices]: http://www.w3.org/TR/mwabp/ 
[w3c candidate recommendation for media queries]: http://www.w3.org/TR/css3-mediaqueries/ 
[deployclickpublish]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-1.png 
[deployclickwebsites]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-2.png 
[deploysignin]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-3.png 
[deployusername]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-4.png 
[deploypassword]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-5.png 
[deploynewwebsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-6.png 
[deploysitesettings]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-7.png 
[deploypublishsite]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/deploy-to-azure-website-8.png 
[mobilehomepage]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/mobile-home-page.png 
[fixedsessionsbytag]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-Fixed.png 
[alltags]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags.png 
[sessionsbytagasp.net]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET.png 
[sessionsbytagasp.netnobootstrap]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsByTag-ASP.NET-NoBootstrap.png 
[alltagsmobile_layoutmobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile.png 
[alltagsmobile_layoutmobiledesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsMobile-_LayoutMobile-Desktop.png 
[resolvedefaultdisplaymode]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/Resolve-DefaultDisplayMode.png 
[alltagsiphone_layoutiphone]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTagsIPhone-_LayoutIPhone.png 
[allspeakers_layoutmobile]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile.png 
[allspeakers_layoutmobileoverridden]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-_LayoutMobile-Overridden.png 
[allspeakersfixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed.png 
[allspeakersfixeddesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-Desktop.png 
[allspeakersfixedsearchbysc]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllSpeakers-Fixed-SearchBySC.png 
[alltagsfixeddesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-Desktop.png 
[alltagsfixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed.png 
[alldatesfixed]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed.png 
[alldatesfixed2]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2.png 
[alldatesfixed2desktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllDates-Fixed2-Desktop.png 
[alltagsfixedsearchbyasp]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/AllTags-Fixed-SearchByASP.png 
[sessionstabletagasp.net]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Tag-ASP.NET.png 
[sessionstablefixedtagasp.netdesktop]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionsTable-Fixed-Tag-ASP.NET-Desktop.png 
[sessionbycode3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-3-644.png 
[sessionbycodefixed3-644]: ./media/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/SessionByCode-Fixed-3-644.png 

