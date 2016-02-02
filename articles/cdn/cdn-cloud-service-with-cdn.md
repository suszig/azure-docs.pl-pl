<properties 
    pageTitle="クラウド サービスと Azure CDN との統合" 
    description="統合 Azure CDN エンドポイントからコンテンツを提供するクラウド サービスをデプロイする方法について説明するチュートリアル" 
    services="cdn, cloud-services" 
    documentationCenter=".net" 
    authors="camsoper" 
    manager="dwrede" 
    editor="tysonn"/>

<tags 
    ms.service="cdn" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="12/02/2015" 
    ms.author="casoper"/>



# <a name="intro"></a> クラウド サービスと Azure CDN との統合します。

クラウド サービスを Azure CDN に統合すると、クラウド サービスの場所からコンテンツを配信できます。 この方法には、次の利点があります。

- クラウド サービスのプロジェクト ディレクトリへの画像、スクリプト、およびスタイルシートのデプロイと更新を簡単に行うことができる。
- クラウド サービスの NuGet パッケージ (たとえば、jQuery バージョン、Bootstrap バージョン) を簡単にアップグレードできる。
- Web アプリケーションと CDN によって配信されるコンテンツをすべて同じ Visual Studio インターフェイスから管理できる。
- Web アプリケーションと CDN によって配信されるコンテンツのデプロイメント ワークフローを一元化できる。
- ASP.NET のバンドルと縮小を Azure CDN と統合できる。

## 学習内容

このチュートリアルで学習する内容は次のとおりです。

-   [Azure CDN エンドポイントをクラウド サービスと統合 Azure CDN から Web ページの静的コンテンツを配信する.](#deploy)
-   [クラウド サービスで静的コンテンツのキャッシュ設定を構成します。](#caching)
-   [Azure CDN を介してコント ローラー アクションからコンテンツを配信します。](#controller)
-   [バンドルおよび縮小されたスクリプトのデバッグ Visual Studio でのエクスペリエンスを維持しながらの Azure CDN を介してコンテンツを配信します。](#bundling)
-   [Azure CDN がオフラインのとき、スクリプトや CSS にフォールバックを構成します。](#fallback)

## 学習内容

既定の ASP.NET MVC テンプレートを使用してクラウド サービス Web ロールをデプロイし、統合された Azure CDN からコンテンツ (たとえば、画像、コントローラー アクションの結果、既定の JavaScript ファイルおよび CSS ファイル) を配信するコードを追加します。さらに、CDN がオフラインになった場合に提供されるバンドルのフォールバック メカニズムを構成するコードを作成します。

## 前提条件

このチュートリアルの前提条件は次のとおりです。

-   アクティブな [Microsoft Azure アカウント](/account/)
-   Visual Studio 2015 年 [Azure SDK](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409)

> [AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。
> + することができます [無料 Azure アカウントを開く](/pricing/free-trial/) -クレジット有料の Azure サービスを使用することができますをこれらの使用後にも、アカウントは維持まで無料の web サイトなどの Azure のサービスを使用します。
> + することができます [MSDN サブスクライバーの特典をアクティブ化](/pricing/member-offers/msdn-benefits-details/) -MSDN サブスクリプション クレジットにより、毎月提供される Azure の有料サービスを使用することができます。

<a name="deploy"></a>
## クラウド サービスのデプロイ

このセクションでは、Visual Studio 2015 の既定の ASP.NET MVC アプリケーション テンプレートをクラウド サービス Web ロールにデプロイした後、これを新しい CDN エンドポイントと統合します。 次の手順に従ってください。

1. Visual Studio 2015 で、メニュー バーから **[ファイル]、[新規]、[プロジェクト]、[クラウド]、[Azure クラウド サービス]** の順に選択して新しい Azure クラウド サービスを作成します。 クラウド サービスに名前を付けて、**[OK]** をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-1-new-project.PNG)

2. **[ASP.NET Web ロール]** を選択し、**[>]** ボタンをクリックします。 [OK] をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-2-select-role.PNG)

3. **[MVC]** を選択し、**[OK]** をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-3-mvc-template.PNG)

4. この Web ロールを Azure クラウド サービスに発行します。 クラウド サービス プロジェクトを右クリックし、**[発行]** を選択します。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-4-publish-a.png)

5. まだ Microsoft Azure にサインインしていない場合は、**[アカウントの追加...]** ドロップダウンをクリックして、**[アカウントの追加]** メニュー項目をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-5-publish-signin.png)

6. サインイン ページで、Azure アカウントをアクティブ化する際に使用した Microsoft アカウントでサインインします。
7. サインインしたら、[**次へ**] をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-6-publish-signedin.png)

8. この段階でまだクラウド サービス アカウントまたはストレージ アカウントを作成していない場合は、Visual Studio の指示に従ってこれらのアカウントを作成します。 **[クラウド サービスとアカウントの作成]** ダイアログで、目的のサービスの名前を入力し、目的のリージョンを選択します。 **[作成]** をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-7-publish-createserviceandstorage.png)

9. 発行の設定ページで、構成を確認し、**[発行]** をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-cs-8-publish-finalize.png)
    >[AZURE.NOTE] クラウド サービスの発行には時間がかかります。 [すべてのロールの Web のデプロイを有効にする] オプションを使用すると、Web ロールに対する高速な (ただし一時的な) 更新を提供することでクラウド サービスのデバッグが高速化されます。 このオプションの詳細については、次を参照してください。 [Azure ツールを使用してクラウド サービスの発行](http://msdn.microsoft.com/library/ff683672.aspx)します。

    [**Microsoft Azure のアクティビティ ログ**] に発行状態が [**完了**] と表示されたら、このクラウド サービスと統合される CDN エンドポイントを作成します。
    >[AZURE.WARNING] パブリッシュ後は、デプロイしたクラウド サービスには、エラー画面が表示されている場合は可能性の高いをデプロイしたクラウド サービスが使用されているので、 [ゲスト OS を .NET 4.5.2 を含まない](../cloud-services/cloud-services-guestos-update-matrix.md#news-updates)します。 この問題を回避する [.NET 4.5.2 スタートアップ タスクとして展開する](../cloud-services/cloud-services-dotnet-install-dotnet.md)します。

## 新しい CDN プロファイルを作成する

CDN プロファイルは、CDN エンドポイントのコレクションです。 各プロファイルには、1 つ以上の CDN エンドポイントが含まれます。 複数のプロファイルを使って、インターネット ドメイン、Web アプリケーション、またはその他の一部の基準別に CDN エンドポイントを整理する必要が生じる場合があります。
> [AZURE.TIP] このチュートリアルで使用する CDN プロファイルが既にあるを場合に進みます [新しい CDN エンドポイントを作成する](#create-a-new-cdn-endpoint)します。

**新しい CDN プロファイルを作成するには**

1. [Azure 管理ポータル](https://portal.azure.com), 、左上でクリックして **新規**します。 **[新規]** ブレードで、**[メディア + CDN]**、**[CDN]** の順に選択します。

    新しい CDN プロファイル ブレードが表示されます。

    ![新しい CDN プロファイル][new-cdn-profile]

2. CDN プロファイルの名前を入力します。

3. **[価格レベル]** を選択するか、既定値を使用します。

4. **リソース グループ**を選択または作成します。 これは、ストレージ アカウントと同じリソース グループである必要はありません。

5. この CDN プロファイルの**サブスクリプション**を選択します。 この場合は、このチュートリアルでの便宜上、ストレージ アカウントと同じサブスクリプションである必要があります。

6. **[場所]** を選択します。 これは、CDN プロファイル情報が格納される Azure の場所です。 CDN エンドポイントの場所には影響しません。 ストレージ アカウントと同じ場所である必要はありません。

7. **[作成]** ボタンをクリックして、新しいプロファイルを作成します。

## 新しい CDN エンドポイントを作成する

**ストレージ アカウントに対する新しい CDN エンドポイントを作成するには**

1. [Azure 管理ポータル](https://portal.azure.com), 、CDN プロファイルに移動します。 これは、前の手順でダッシュボードにピン留めしている可能性があります。 ピン留めしていない場合は、**[参照]**、**[CDN プロファイル]** の順にクリックし、エンドポイントの追加先のプロファイルをクリックします。

    CDN プロファイル ブレードが表示されます。

    ![CDN プロファイル][cdn-profile-settings]

2. **[エンドポイントの追加]** ボタンをクリックします。

    ![[エンドポイントの追加] ボタン][cdn-new-endpoint-button]

    **[エンドポイントの追加]** ブレードが表示されます。

    ![[エンドポイントの追加] ブレード][cdn-add-endpoint]

3. この CDN エンドポイントの**名前**を入力します。この名前は、ドメインでキャッシュされたリソースにアクセスするために使用が `< EndpointName >. azureedge.net`します。

4. **[配信元の種類]** ドロップダウンで、*[クラウド サービス]* を選択します。

5. **[配信元ホスト名]** ドロップダウンで、クラウド サービスを選択します。

6. **[元のパス]**、**[配信元のホスト ヘッダー]**、および **[プロトコル/配信元ポート]** の既定値はそのまま使用します。 少なくとも 1 つのプロトコル (HTTP または HTTPS) を指定する必要があります。

7. **[追加]** ボタンをクリックして、新しいエンドポイントを作成します。

8. エンドポイントが作成されると、プロファイルのエンドポイントの一覧に表示されます。 一覧には、キャッシュされたコンテンツへのアクセスに使用する URL と元のドメインが表示されます。

    ![CDN エンドポイント][cdn-endpoint-success]
    > [AZURE.NOTE] エンドポイントはすぐには使用できません。 登録が CDN ネットワークに反映されるまでに最大で 90 分かかる場合があります。 CDN ドメイン名を直ちに使用しようとするユーザーは、CDN を経由してコンテンツを取得できるようになるまでは状態コード 404 を受け取る場合があります。

## CDN エンドポイントをテストする

発行状態が **完了**, ブラウザー ウィンドウを開きに移動 * * http://<cdnName>*.azureedge.net/Content/bootstrap.css** します。この設定では、次の URL を使用します。

    http://camservice.azureedge.net/Content/bootstrap.css

この URL は、CDN エンドポイントの次のオリジン URL に対応します。

    http://camcdnservice.cloudapp.net/Content/bootstrap.css

移動すると **http://*&lt;cdnName&gt;*.azureedge.net/Content/bootstrap.css**, 、ブラウザーによって求められますダウンロードしたり、発行された Web アプリケーションから bootstrap.css を開いたりします。

![](media/cdn-cloud-service-with-cdn/cdn-1-browser-access.PNG)

任意のパブリックにアクセスできる URL に同様にアクセスできる **http://*&lt;serviceName&gt;*.cloudapp.net/**, 、CDN エンドポイントからです。 次に例を示します。

-   /Script パスの .js ファイル
-   /Content パスの任意のコンテンツ ファイル
-   任意のコントローラー/アクション
-   CDN エンドポイントでクエリ文字列が有効になっている場合、クエリ文字列を含む任意の URL

実際には、上記の構成、クラウド サービス全体をホストできます **http://*&lt;cdnName&gt;*.azureedge.net/**します。 移動する場合は、 **http://camservice.azureedge.net/**, 、Home/index からのアクションの結果を表示します。

![](media/cdn-cloud-service-with-cdn/cdn-2-home-page.PNG)

ただし、これが、Azure CDN を介してクラウド サービス全体を提供するのが常に適切なやり方 (または一般的に適切なやり方) であることにはなりません。 いくつかの注意点があります。

-   現時点で、Azure CDN ではプライベートなコンテンツを配信できないため、この方法ではサイト全体をパブリックにする必要があります。
-   CDN エンドポイントがオフラインになる何らかの理由で場合、かどうかスケジュールされたメンテナンスであろうユーザー エラーでクラウド サービス全体がオフラインになる顧客をオリジン URL にリダイレクトできる場合を除き、 **http://*&lt;serviceName&gt;*.cloudapp.net/**します。
-   カスタムの Cache-control 設定しても (を参照してください [静的ファイルは、クラウド サービスのオプションのキャッシュを構成する](#caching)), 、CDN エンドポイントでは、高度に動的なコンテンツのパフォーマンスは向上しません。 上に示すように CDN エンドポイントからホーム ページを読み込もうとした場合、非常に単純な既定のホーム ページを初めて読み込むときに 5 秒以上かかります。 このページに毎分更新する必要がある動的コンテンツが含まれていたとしたら、クライアント エクスペリエンスはどうなるでしょうか。 動的コンテンツを CDN エンドポイントから配信するにはキャッシュの有効期限が短く設定されている必要があります。これは、CDN エンドポイントで頻繁にキャッシュ ミスが発生することになります。 その結果、クラウド サービスのパフォーマンスが低下し、CDN の目的が果たせなくなります。

また、クラウド サービスで Azure CDN から配信するコンテンツをケースバイケースで決定する方法もあります。 CDN エンドポイントから個々のコンテンツ ファイルにアクセスする方法については既に説明しました。 紹介した方法で CDN エンドポイントを介して特定のコント ローラー アクションを配信する [Azure CDN を介してコント ローラー アクションからコンテンツを配信](#controller)します。

<a name="caching"></a>
## クラウド サービスの静的ファイルのキャッシュ オプションを構成する

Azure CDN 統合をクラウド サービスに組み込むと、CDN エンドポイントで静的コンテンツをどのようにキャッシュするかを指定できます。これを行うには、開く *Web.config* Web ロールからのプロジェクト (たとえば、WebRole1) を追加、 `< staticContent >` 要素を `< system.webServer >`します。次の XML では、3 日間で有効期限が切れるキャッシュを追加しています。

    <system.webServer>
      <staticContent>
        <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="3.00:00:00"/>
      </staticContent>
      ...
    </system.webServer>

この操作を実行すると、クラウド サービス内のすべての静的ファイルは、CDN キャッシュ内で同じ規則に従います。 キャッシュ設定をより細かく制御するには、*Web.config* ファイルをフォルダーに追加し、そこに設定を追加します。 たとえば、*Web.config* ファイルを *\Content* フォルダーに追加して、その内容を次の XML で置き換えます。

    <?xml version="1.0"?>
    <configuration>
      <system.webServer>
        <staticContent>
          <clientCache cacheControlMode="UseMaxAge" cacheControlMaxAge="15.00:00:00"/>
        </staticContent>
      </system.webServer>
    </configuration>

この設定は、*\Content* フォルダーのすべての静的ファイルを 15 日間キャッシュすることを指定しています。

詳細を構成する方法について、 `< clientCache >` 要素を参照してください [クライアント キャッシュ < clientCache >](http://www.iis.net/configreference/system.webserver/staticcontent/clientcache)します。

[Azure CDN を介してコント ローラー アクションからコンテンツを配信](#controller), も説明した、CDN キャッシュ内のコント ローラー アクションの結果に対してキャッシュ設定を構成する方法です。

<a name="controller"></a>
## Azure CDN を介してコントローラー アクションからコンテンツを配信する

クラウド サービス Web ロールを Azure CDN と統合すると、Azure CDN を介してコントローラー アクションからコンテンツを簡単に配信できます。 (上に示した)、Azure CDN を介して直接サービスを提供しているクラウド [Maarten Balliauw](https://twitter.com/maartenballiauw) おもしろい使用を実現する方法を示していますで MemeGenerator コント ローラー [Azure CDN による web の遅延時間の短縮](http://channel9.msdn.com/events/TechDays/Techdays-2014-the-Netherlands/Reducing-latency-on-the-web-with-the-Windows-Azure-CDN)します。 ここではその方法を再現します。

ミームを生成するサービスが Chuck Norris の若い画像に基づく、クラウドであるとします (による撮影 [Alan Light](http://www.flickr.com/photos/alan-light/218493788/)) 次のようにします。

![](media/cdn-cloud-service-with-cdn/cdn-5-memegenerator.PNG)

ある単純な `インデックス` ことが可能では、画像内にジョークを指定する操作は、アクションに投稿すると、ミームを生成します。 題材が Chuck Norris のため、このページは全世界で受け入れられると予想されます。 これは、Azure CDN で半動的コンテンツを配信するよい例です。

上記の手順を実行して、このコントローラー アクションを設定します。

1. *\Controllers* フォルダーに、*MemeGeneratorController.cs* という名前の新しい .cs ファイルを作成し、その内容を次のコードで置き換えます。 強調表示されている箇所は CDN 名で置き換えてください。

     using System;
     using System.Collections.Generic;
     using System.Diagnostics;
     using System.Drawing;
     using System.IO;
     using System.Net;
     using System.Web.Hosting;
     using System.Web.Mvc;
     using System.Web.UI;
    
     namespace WebRole1.Controllers
     {
         public class MemeGeneratorController : Controller
         {
             static readonly Dictionary<string, Tuple<string ,string>> Memes = new Dictionary<string, Tuple<string, string>>();
    
             public ActionResult Index()
             {
                 return View();
             }
    
             [HttpPost, ActionName("Index")]
             public ActionResult Index_Post(string top, string bottom)
             {
                 var identifier = Guid.NewGuid().ToString();
                 if (!Memes.ContainsKey(identifier))
                 {
                     Memes.Add(identifier, new Tuple<string, string>(top, bottom));
                 }
    
                 return Content("<a href=\"" + Url.Action("Show", new {id = identifier}) + "\">here's your meme</a>");
             }
    
             [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
             public ActionResult Show(string id)
             {
                 Tuple<string, string> data = null;
                 if (!Memes.TryGetValue(id, out data))
                 {
                     return new HttpStatusCodeResult(HttpStatusCode.NotFound);
                 }
    
                 if (Debugger.IsAttached) // Preserve the debug experience
                 {
                     return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                 }
                 else // Get content from Azure CDN
                 {
                     return Redirect(string.Format("http://<yourCdnName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
                 }
             }
    
             [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]
             public ActionResult Generate(string top, string bottom)
             {
                 string imageFilePath = HostingEnvironment.MapPath("~/Content/chuck.bmp");
                 Bitmap bitmap = (Bitmap)Image.FromFile(imageFilePath);
    
                 using (Graphics graphics = Graphics.FromImage(bitmap))
                 {
                     SizeF size = new SizeF();
                     using (Font arialFont = FindBestFitFont(bitmap, graphics, top.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                     {
                         graphics.DrawString(top.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), 10f));
                     }
                     using (Font arialFont = FindBestFitFont(bitmap, graphics, bottom.ToUpperInvariant(), new Font("Arial Narrow", 100), out size))
                     {
                         graphics.DrawString(bottom.ToUpperInvariant(), arialFont, Brushes.White, new PointF(((bitmap.Width - size.Width) / 2), bitmap.Height - 10f - arialFont.Height));
                     }
                 }
    
                 MemoryStream ms = new MemoryStream();
                 bitmap.Save(ms, System.Drawing.Imaging.ImageFormat.Png);
                 return File(ms.ToArray(), "image/png");
             }
    
             private Font FindBestFitFont(Image i, Graphics g, String text, Font font, out SizeF size)
             {
                 // Compute actual size, shrink if needed
                 while (true)
                 {
                     size = g.MeasureString(text, font);
    
                     // It fits, back out
                     if (size.Height < i.Height &&
                          size.Width < i.Width) { return font; }
    
                     // Try a smaller font (90% of old size)
                     Font oldFont = font;
                     font = new Font(font.Name, (float)(font.Size * .9), font.Style);
                     oldFont.Dispose();
                 }
             }
         }
     }

2. 既定値を右クリックして `Index()` アクションを選択 **ビューの追加**します。

    ![](media/cdn-cloud-service-with-cdn/cdn-6-addview.PNG)

3.  次の既定の設定をそのまま受け入れ、**[追加]** をクリックします。

    ![](media/cdn-cloud-service-with-cdn/cdn-7-configureview.PNG)

4. 新しい *Views\MemeGenerator\Index.cshtml* を開き、その内容を、ジョークを送信するための次の単純な HTML で置き換えます。

     <h2>Meme Generator</h2>
    
     <form action="" method="post">
         <input type="text" name="top" placeholder="Enter top text here" />
         <br />
         <input type="text" name="bottom" placeholder="Enter bottom text here" />
         <br />
         <input class="btn" type="submit" value="Generate meme" />
     </form>

5. クラウド サービスを発行しに移動 **http://*&lt;serviceName&gt;*.cloudapp.net/MemeGenerator/Index** お使いのブラウザーにします。

フォームの値を送信すると `/memegenerator/index`, 、 `Index_Post` へのリンクが返されます、 `表示` それぞれの入力の識別子を持つアクション メソッドです。 リンクをクリックすると、次のコードに到達します。

    [OutputCache(VaryByParam = "*", Duration = 1, Location = OutputCacheLocation.Downstream)]
    public ActionResult Show(string id)
    {
        Tuple<string, string> data = null;
        if (!Memes.TryGetValue(id, out data))
        {
            return new HttpStatusCodeResult(HttpStatusCode.NotFound);
        }
    
        if (Debugger.IsAttached) // Preserve the debug experience
        {
            return Redirect(string.Format("/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
        else // Get content from Azure CDN
        {
            return Redirect(string.Format("http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top={0}&bottom={1}", data.Item1, data.Item2));
        }
    }

ローカル デバッガーが接続されている場合は、ローカル リダイレクトにより通常のデバッグ エクスペリエンスが得られます。 クラウド サービスで実行されている場合は、次の場所にリダイレクトされます。

    http://<yourCDNName>.azureedge.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

この URL は、CDN エンドポイントの次のオリジン URL に対応します。

    http://<youCloudServiceName>.cloudapp.net/MemeGenerator/Generate?top=<formInput>&bottom=<formInput>

使用して、 `OutputCacheAttribute` 属性を `生成` アクションの結果をキャッシュする方法、Azure CDN が有効かを指定します。 次のコードでは、キャッシュの有効期限が 1 時間 (3,600 秒) に設定されています。

    [OutputCache(VaryByParam = "*", Duration = 3600, Location = OutputCacheLocation.Downstream)]

同様に、任意のキャッシュ オプションを使用して、Azure CDN を介してクラウド サービスの任意のコントローラー アクションからコンテンツを配信できます。

次のセクションでは、バンドルされたスクリプト、縮小されたスクリプト、および CSS を Azure CDN 経由で提供する方法について説明します。

<a name="bundling"></a>
## ASP.NET のバンドルと縮小を Azure CDN と統合できる。

スクリプトや CSS スタイルシートはあまり頻繁に変更されないため、Azure CDN キャッシュの有力な候補になります。 Azure CDN を介して Web ロール全体を提供するやり方は、バンドルや縮小を Azure CDN と統合する方法として最も簡単です。 ただし、このやり方を希望しない方のために、次のような ASP.NET のバンドルと縮小の開発者エクスペリエンスを維持しながらこれを実現する方法を紹介します。

-   優れたデバッグ モード エクスペリエンス
-   合理的なデプロイ
-   スクリプトや CSS のバージョンをアップグレードするためのクライアントの即時の更新
-   CDN エンドポイントのに障害に対するフォールバック メカニズム
-   最小限のコードの変更

**WebRole1** で作成したプロジェクトを [、Azure web サイトと Azure CDN エンドポイントを統合し、Azure CDN から Web ページの静的コンテンツを配信](#deploy), 、開かれている *app_start \bundleconfig.cs* 、を見て、 `バンドルします。Add()` メソッドの呼び出しです。

    public static void RegisterBundles(BundleCollection bundles)
    {
        bundles.Add(new ScriptBundle("~/bundles/jquery").Include(
                    "~/Scripts/jquery-{version}.js"));
        ...
    }

1 つ目の `バンドルします。Add()` ステートメントは、仮想ディレクトリに、スクリプト バンドルを追加 `~/bundles/jquery`します。 実行して [ *Views\Shared\_Layout.cshtml* に、スクリプト バンドル タグがどのようにレンダリングされています。 次の Razor コードが見つかります。

    @Scripts.Render("~/bundles/jquery")

Azure Web ロールでこの Razor コードが実行時にレンダリングされます、 `< スクリプト >` 次のようなスクリプト バンドルのためのタグ。

    <script src="/bundles/jquery?v=FVs3ACwOLIVInrAl5sdzR2jrCDmVOWFbZMY6g6Q0ulE1"></script>

ただし、実行した場合に、Visual Studio」と入力して `f5 キーを押して`, 、バンドル内の各スクリプト ファイルが個別にレンダリングされます (上記のケースでは、1 つだけのスクリプト ファイルは、バンドル内)。

    <script src="/Scripts/jquery-1.10.2.js"></script>

これにより、JavaScript コードを開発環境でデバッグできる一方で、運用環境での同時クライアント接続数 (バンドル) を削減し、ファイルのダウンロード パフォーマンス (縮小) を高めることができます。 これは、Azure CDN 統合によって得られる優れた機能です。 加えて、レンダリングされたバンドルには自動生成されたバージョン文字列が既に含まれます。そこで、この機能を複製することで、NuGet を介して jQuery バージョンを更新するたびにできる限り迅速にクライアント側にその更新が反映されるようにすることができます。

ASP.NET のバンドルおよび縮小を CDN エンドポイントと統合するには、次の手順に従います。

1. 戻り *app_start \bundleconfig.cs*, 、変更、 `バンドルします。Add()` 別に使用する方法を [Bundle コンス トラクター](http://msdn.microsoft.com/library/jj646464.aspx), 、CDN アドレスを指定します。 これを行うには、置換、 `RegisterBundles` メソッド定義を次のコード。

     public static void RegisterBundles(BundleCollection bundles)
     {
         bundles.UseCdn = true;
         var version = System.Reflection.Assembly.GetAssembly(typeof(Controllers.HomeController))
             .GetName().Version.ToString();
         var cdnUrl = "http://<yourCDNName>.azureedge.net/{0}?v=" + version;
    
         bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")).Include(
                     "~/Scripts/jquery-{version}.js"));
    
         bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")).Include(
                     "~/Scripts/jquery.validate*"));
    
         // Use the development version of Modernizr to develop with and learn from. Then, when you're
         // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
         bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")).Include(
                     "~/Scripts/modernizr-*"));
    
         bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap")).Include(
                     "~/Scripts/bootstrap.js",
                     "~/Scripts/respond.js"));
    
         bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                     "~/Content/bootstrap.css",
                     "~/Content/site.css"));
     }

 必ずに置き換えて `< yourCDNName >` Azure CDN の名前に置き換えます。

 の設定で、 `バンドルします。UseCdn = true` し、巧妙に作成された CDN URL を各バンドルに追加します。 たとえば、コードの最初のコンストラクター

     new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery"))

 は、次のコードと同じです。

     new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "http://<yourCDNName>.azureedge.net/bundles/jquery?v=<W.X.Y.Z>"))

 このコンストラクターは、ASP.NET のバンドルおよび縮小機能に対して、ローカルにデバッグされたときに個々のスクリプト ファイルをレンダリングする一方で、指定された CDN アドレスを使用して対象のスクリプトにアクセスすることを指定しています。 ここで、この巧妙に作成された CDN URL の 2 つの重要な特性に注意してください。

 -   この CDN URL のオリジンは、 `http://&lt;yourcloudservice&gt;.cloudapp.net/bundles/jquery?v=<w.x.y.z>;`, 、実際には、クラウド サービス内のスクリプト バンドルの仮想ディレクトリであります。
 -   CDN コンストラクターを使用しているため、バンドルのための CDN スクリプト タグには、レンダリングされた URL 内の自動生成されたバージョン文字列が含まれません。 Azure CDN でキャッシュ ミスを強制的に発生させるには、スクリプト バンドルを変更するたびに一意のバージョン文字列を手動で生成する必要があります。 また、この一意のバージョン文字列は、バンドルがデプロイされた後の Azure CDN でのキャッシュ ヒットを最大化するために、デプロイの有効期間を通じて一定に保たれる必要があります。
 -   クエリ文字列 v =<W.X.Y.Z> からプル *properties \assemblyinfo.cs* 、Web ロール プロジェクトにします。Azure に発行するたびにアセンブリ バージョンを増分する操作を含むデプロイ ワークフローを導入できます。また、ワイルドカード文字 * を使って単にプロジェクトの *Properties\AssemblyInfo.cs* を変更して、ビルドするたびに自動的にバージョン文字列を増分するよう設定することもできます。次に例を示します。

         [assembly: AssemblyVersion("1.0.0.*")]

     デプロイの有効期間を通じて一意の文字列を生成する操作を合理化するための他の戦略も使用できます。

3. クラウド サービスを再発行し、ホーム ページにアクセスします。

4. ページの HTML コードを表示します。 クラウド サービスに変更を再発行するたびに一意のバージョン文字列を付けて CDN URL がレンダリングされているのを確認できます。 次に例を示します。

     ...
    
     <link href="http://camservice.azureedge.net/Content/css?v=1.0.0.25449" rel="stylesheet"/>
    
     <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25449"></script>
    
     ...
    
     <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25449"></script>
    
     <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25449"></script>
    
     ...

5. Visual Studio で、Visual Studio でクラウド サービスのデバッグ」と入力して `f5 キーを押して`.、

6. ページの HTML コードを表示します。 Visual Studio での一貫したデバッグ エクスペリエンスが得られるように個別にレンダリングされた各スクリプト ファイルを確認できます。

     ...
    
         <link href="/Content/bootstrap.css" rel="stylesheet"/>
     <link href="/Content/site.css" rel="stylesheet"/>
    
         <script src="/Scripts/modernizr-2.6.2.js"></script>
    
     ...
    
         <script src="/Scripts/jquery-1.10.2.js"></script>
    
         <script src="/Scripts/bootstrap.js"></script>
     <script src="/Scripts/respond.js"></script>
    
     ...   


<a name="fallback"></a>
## CDN URL のフォールバック メカニズム

何かの理由で Azure CDN エンドポイントに障害が発生した場合に備えて、JavaScript または Bootstrap を読み込むためのフォールバック オプションとしてオリジン Web サーバーにアクセスできるようにしておくと便利です。 CDN が利用できないために Web サイトの画像が失われることは深刻な事態ですが、スクリプトやスタイルシートで提供される重要なページ機能が失われることは、さらに深刻な事態です。

[バンドル](http://msdn.microsoft.com/library/system.web.optimization.bundle.aspx) クラスと呼ばれるプロパティが含まれています。 [CdnFallbackExpression](http://msdn.microsoft.com/library/system.web.optimization.bundle.cdnfallbackexpression.aspx) CDN 障害に対するフォールバック メカニズムを構成することができます。 このプロパティを使用するには、次の手順に従います。

1. Web ロール プロジェクトを開きます *app_start \bundleconfig.cs*, にそれぞれの CDN URL を追加した [Bundle コンス トラクター](http://msdn.microsoft.com/library/jj646464.aspx), 、既定のバンドルにフォールバック メカニズムを追加する次の強調表示された変更を加えると。

     public static void RegisterBundles(BundleCollection bundles)
     {
         var version = System.Reflection.Assembly.GetAssembly(typeof(BundleConfig))
             .GetName().Version.ToString();
         var cdnUrl = "http://cdnurl.azureedge.net/.../{0}?" + version;
         bundles.UseCdn = true;
    
         bundles.Add(new ScriptBundle("~/bundles/jquery", string.Format(cdnUrl, "bundles/jquery")) 
                     { CdnFallbackExpression = "window.jquery" }
                     .Include("~/Scripts/jquery-{version}.js"));
    
         bundles.Add(new ScriptBundle("~/bundles/jqueryval", string.Format(cdnUrl, "bundles/jqueryval")) 
                     { CdnFallbackExpression = "$.validator" }
                     .Include("~/Scripts/jquery.validate*"));
    
         // Use the development version of Modernizr to develop with and learn from. Then, when you&#39;re
         // ready for production, use the build tool at http://modernizr.com to pick only the tests you need.
         bundles.Add(new ScriptBundle("~/bundles/modernizr", string.Format(cdnUrl, "bundles/modernizer")) 
                     { CdnFallbackExpression = "window.Modernizr" }
                     .Include("~/Scripts/modernizr-*"));
    
         bundles.Add(new ScriptBundle("~/bundles/bootstrap", string.Format(cdnUrl, "bundles/bootstrap"))     
                     { CdnFallbackExpression = "$.fn.modal" }
                     .Include(
                             "~/Scripts/bootstrap.js",
                             "~/Scripts/respond.js"));
    
         bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css")).Include(
                     "~/Content/bootstrap.css",
                     "~/Content/site.css"));
     }

 `CdnFallbackExpression` が null でないスクリプトは、HTML に挿入、バンドルが正常に読み込まれたかどうかをテストし、ない場合は、バンドルにはオリジン Web サーバーから直接アクセスします。 このプロパティは、それぞれの CDN バンドルが適切に読み込まれたかどうかをテストする JavaScript 式に設定する必要があります。 各バンドルをテストするために必要な式は、コンテンツによって異なります。 上記の既定のバンドルに対して、次のように定義されています。

 -   `window.jquery` {version} .js に定義されました。
 -   `$.validator` は jquery.validate.js に定義されて
 -   `ウィンドウです。Modernizr` は modernizer-{version} .js に定義されています。
 -   `$$.fn.modal` は bootstrap.js に定義されている。

 お気付きの CdnFallbackExpression を設定したしていない、 `~/Cointent/css` バンドルします。これは、現在があるため、 [System.Web.Optimization のバグ](https://aspnetoptimization.codeplex.com/workitem/104) 挿入、 `< スクリプト >` 、期待されるのではなくフォールバック CSS のタグ `< リンク >` タグ。

 ただし、適切な [スタイル バンドルのフォールバック](https://github.com/EmberConsultingGroup/StyleBundleFallback) によって提供される [Ember Consulting Group](https://github.com/EmberConsultingGroup)します。

2. CSS の対処法を使用するには、Web ロール プロジェクトの新しい .cs ファイルを作成 *App_Start* という名前のフォルダー *StyleBundleExtensions.cs*, 、その内容を置き換えると、 [GitHub からコード](https://github.com/EmberConsultingGroup/StyleBundleFallback/blob/master/Website/App_Start/StyleBundleExtensions.cs)します。

4. *App_Start\StyleFundleExtensions.cs* で、名前空間の名前を Web ロールの名前 (**WebRole1** など) に変更します。

3. 戻り `app_start \bundleconfig.cs` 最後 `バンドルします。追加` ステートメントを次の強調表示されたコード。

        bundles.Add(new StyleBundle("~/Content/css", string.Format(cdnUrl, "Content/css"))
            <mark>.IncludeFallback("~/Content/css", "sr-only", "width", "1px")</mark>
            .Include(
                  "~/Content/bootstrap.css",
                  "~/Content/site.css"));

    この新しいメソッドでは、同じアイデアを使用するスクリプトを HTML に挿入します。つまり、DOM を調べて CSS バンドルに定義されているクラス名、規則名、および規則値が一致するかどうかを確認し、一致しない場合はオリジン Web サーバーにフォールバックします。

4. クラウド サービスを発行し、ホーム ページにアクセスします。

5. ページの HTML コードを表示します。 次のようなスクリプトが挿入されていることを確認できます。

     ...
    
     <link href="http://az632148.azureedge.net/Content/css?v=1.0.0.25474" rel="stylesheet"/>
     <script>(function() {
                     var loadFallback,
                         len = document.styleSheets.length;
                     for (var i = 0; i < len; i++) {
                         var sheet = document.styleSheets[i];
                         if (sheet.href.indexOf('http://camservice.azureedge.net/Content/css?v=1.0.0.25474') !== -1) {
                             var meta = document.createElement('meta');
                             meta.className = 'sr-only';
                             document.head.appendChild(meta);
                             var value = window.getComputedStyle(meta).getPropertyValue('width');
                             document.head.removeChild(meta);
                             if (value !== '1px') {
                                 document.write('<link href="/Content/css" rel="stylesheet" type="text/css" />');
                             }
                         }
                     }
                     return true;
                 }())||document.write('<script src="/Content/css"><\/script>');</script>
    
         <script src="http://camservice.azureedge.net/bundles/modernizer?v=1.0.0.25474"></script>
     <script>(window.Modernizr)||document.write('<script src="/bundles/modernizr"><\/script>');</script>
    
     ... 
    
         <script src="http://camservice.azureedge.net/bundles/jquery?v=1.0.0.25474"></script>
     <script>(window.jquery)||document.write('<script src="/bundles/jquery"><\/script>');</script>
    
         <script src="http://camservice.azureedge.net/bundles/bootstrap?v=1.0.0.25474"></script>
     <script>($.fn.modal)||document.write('<script src="/bundles/bootstrap"><\/script>');</script>
    
     ...

 CSS バンドルに対して挿入されたスクリプトがエラーの残りをまだ含まれていることに注意してください、 `CdnFallbackExpression` 行のプロパティ。

     }())||document.write('<script src="/Content/css"><\/script>');</script>

 ただし、(すぐ上の行の) || 式の最初の部分は常に true を返すため、document.write() 関数が実行されることはありません。

## 詳細情報

- [Azure コンテンツ配信ネットワーク (CDN) の概要](http://msdn.microsoft.com/library/azure/ff919703.aspx)
- [Web アプリケーションで Azure CDN からコンテンツを配信します。](cdn-serve-content-from-cdn-in-your-web-application.md)
- [Azure CDN と Azure の web サイトを統合します。](cdn-websites-with-cdn.md)
- [ASP.NET のバンドルと縮小](http://www.asp.net/mvc/tutorials/mvc-4/bundling-and-minification)
- [Azure 用 CDN の使用](cdn-how-to-use.md)



[new-cdn-profile]: ./media/cdn-cloud-service-with-cdn/cdn-new-profile.png 
[cdn-profile-settings]: ./media/cdn-cloud-service-with-cdn/cdn-profile-settings.png 
[cdn-new-endpoint-button]: ./media/cdn-cloud-service-with-cdn/cdn-new-endpoint-button.png 
[cdn-add-endpoint]: ./media/cdn-cloud-service-with-cdn/cdn-add-endpoint.png 
[cdn-endpoint-success]: ./media/cdn-cloud-service-with-cdn/cdn-endpoint-success.png 

