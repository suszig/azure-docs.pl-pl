<properties
    pageTitle="Azure App Service のエンタープライズ クラスの WordPress"
    description="Azure App Service にエンタープライズ クラスの WordPress サイトをホストする方法について説明します。"
    services="app-service\web"
    documentationCenter=""
    authors="tfitzmac"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="web"
    ms.date="11/06/2015"
    ms.author="tomfitz"/>

#Azure App Service のエンタープライズ クラスの WordPress

Azure App Service は、ミッション クリティカルな系の大規模なスケールのスケーラブルな安全性、使いやすい環境を提供 [WordPress][wordpress] サイトです。 Microsoft 自体などのエンタープライズ クラスのサイトの運営、 [Office][officeblog] と [Bing][bingblog] ブログです。 このドキュメントでは、Azure App Service Web Apps を使用して、大量の訪問者を処理できるエンタープライズ クラスの、クラウド ベースの WordPress サイトを確立し、維持する方法について説明しています。

## アーキテクチャと計画

基本的な WordPress インストールの要件は 2 つだけです。

* **MySQL データベース** を通じて使用可能な - [ClearDB が Azure Marketplace で][cdbnstore], 、または Azure Virtual Machines でいずれかを使用して、独自の MySQL インストールを管理する [Windows][mysqlwindows] または [Linux][mysqllinux]です。

    > [AZURE.NOTE] ClearDB では、構成ごとに異なるパフォーマンス特性を持つ複数の MySQL 構成を提供します。 参照してください、 [Azure ストア][cdbnstore] については、Azure ストアを通じて提供される製品または [ClearDB の料金](http://www.cleardb.com/pricing.view) ClearDB から直接サービスの場合。

* **PHP 5.2.4 以上** -Azure App Service は現在提供 [PHP version 5.4、5.5 では、5.6][phpwebsite]します。

    > [AZURE.NOTE] 常に最新のセキュリティ修正プログラムがあることを確認するのには、PHP の最新バージョンで実行されていることをお勧めします。

###基本的なデプロイ

基本要件だけを使用して、Azure リージョン内に基本的なソリューションを作成できます。

![単一の Azure リージョンでホストされている Azure Web アプリと MySQL データベース][basic-diagram]

このデプロイでは、サイトの複数の Web Apps インスタンスを作成することで、アプリケーションをスケールアウトできますが、いずれも特定リージョンのデータ センターにホストされます。 このリージョンの外部からアクセスするユーザーがサイトを使用すると、応答時間が長くなることがあります。また、このリージョンのデータ センターがダウンした場合は、アプリケーションもダウンします。


###複数リージョンのデプロイ

Azure を使用して [Traffic Manager][trafficmanager], 、WordPress サイトを訪問者の 1 つだけの URL を提供するときに複数の地域を拡大することはできます。 すべてのユーザーは Traffic Manager を介してサイトにアクセスし、負荷分散構成に基づいて特定のリージョンにルーティングされます。

![Azure の Web アプリは CDBR の高可用性のルーターを使用して、地域間での MySQL にルーティングする複数の地域でホストされています。][multi-region-diagram]

各リージョン内でも、WordPress サイトは複数の Web Apps インスタンス間に展開されることがありますが、こうしたスケーリングはリージョンに固有です。つまり、トラフィックの多いリージョンと少ないリージョンでは、異なるスケーリングが実施されます。

レプリケーションと複数の MySQL データベースへのルーティングを行うことができます ClearDB を使用して [CDBR High Availability Router][cleardbscale] (左、図) または [MySQL Cluster CGE][cge]します。

###メディア ストレージとキャッシュを用いる複数リージョンのデプロイ

サイトがアップロードを受け入れている場合、またはメディア ファイルをホストする場合は、Azure BLOB ストレージを使用します。 キャッシュする場合は、次のように検討してください [Redis cache][rediscache], 、[Memcache クラウド](https://azure.microsoft.com/marketplace/partners/garantiadata/memcached/), 、[MemCachier](https://azure.microsoft.com/marketplace/partners/memcachier/memcachier/), 、または、他のキャッシュ オファリングの 1 つ、 [Azure ストア](http://azure.microsoft.com/gallery/store/)します。

![Azure の Web アプリは、Managed Cache、Blob ストレージ、CDN を使用した CDBR の高可用性のルーターを MySQL に使用し、複数のリージョンでホストされています。][performance-diagram]

BLOB ストレージは、既定では複数のリージョンにまたがって地理的に分散されるため、すべてのサイトにファイルをレプリケートしなくてもかまいません。 あるも有効にできます Azure [コンテンツ配信ネットワーク (CDN)][cdn] 、訪問者に近いエンド ノードへのファイルが配布して、Blob ストレージです。

###計画

####その他の要件

目的 | 方法
------------------------|-----------
**大きいファイルのアップロードまたは保存** | [WordPress plugin for using Blob storage (BLOB ストレージを使うための WordPress プラグイン)][storageplugin]
**電子メールの送信** | [SendGrid][storesendgrid] と [SendGrid 用 WordPress プラグイン][sendgridplugin]
**カスタム ドメイン名** | [Azure App Service のカスタム ドメイン名の構成][customdomain]
**HTTPS** | [Web アプリに対する HTTPS を Azure App Service で有効にする][httpscustomdomain]
**運用前検証** | [Azure App Service で web アプリのステージング環境をセットアップ][staging] <p>ステージングから運用にも web アプリに切り替えると、WordPress の構成で移動することに注意してください。 ステージング中のアプリを運用段階に切り替える前に、すべての設定が本番アプリの要件に合わせて更新されていることを確認する必要があります。</p>
**監視とトラブルシューティング** | [Azure App Service での web アプリの診断ログを有効にする][log] と [Azure App Service で Web アプリの監視][monitor]
**サイトのデプロイ** | [Azure App Service での Web アプリのデプロイ][deploy]

####高可用性と障害復旧

目的 | 方法
------------------------|-----------
**サイトの負荷分散** または **geo の分散** | [Azure Traffic Manager によるトラフィックのルーティング][trafficmanager]
**バックアップと復元** | [Azure App Service で web アプリのバックアップ][backup] と [Azure App Service で web アプリの復元][restore]

####パフォーマンス

クラウド内のパフォーマンスは、主にキャッシュとスケールアウトによって達成されます。しかし、Web Apps のホスティングに関するメモリや帯域幅などの属性も、考慮に入れる必要があります。

目的 | 方法
------------------------|-----------
**App Service インスタンス機能について学ぶ** |  [App Service 階層 の機能と料金の詳細][websitepricing]
**キャッシュ リソース** | [Redis キャッシュ][rediscache], 、[Memcache クラウド](https://azure.microsoft.com/marketplace/partners/garantiadata/memcached/), 、[MemCachier](https://azure.microsoft.com/marketplace/partners/memcachier/memcachier/), 、または、他のキャッシュ オファリングの 1 つ、 [Azure ストア](/gallery/store/)
**アプリケーションのスケール** | [Azure App Service で web アプリのスケール][websitescale] と [ClearDB High Availability ルーティング][cleardbscale]します。 ホストし、する必要がある独自の MySQL インストールを管理する場合 [MySQL Cluster CGE][cge] スケール アウト

####移行

既存の WordPress サイトから Azure App Service に移行する方法は、次の 2 種類です。

* **[WordPress のエクスポート][export]** -この方法でエクスポートし、Azure App Service を使用して上の新しい WordPress サイトにインポート可能なブログのコンテンツ、 [WordPress importer プラグイン][import]します。

    > [AZURE.NOTE] このプロセスでは、コンテンツは移行することができます、プラグインやテーマ、またはその他のカスタマイズには移行できません。 これらは、改めて手動でインストールする必要があります。

* **手動移行** - [でサイトをバックアップ][wordpressbackup] と [データベース][wordpressdbbackup], 手動で Azure App Service で web アプリに復元し、MySQL データベースの詳細にカスタマイズされたサイトを移行し、プラグインやテーマ、さらにその他のカスタマイズを手動でインストールする手間が関連付けられています。

## 詳細な手順

### 新規 WordPress サイトを作成する

1. 使用して、 [Azure Marketplace][cdbnstore] で特定したサイズの MySQL データベースを作成する、 [アーキテクチャと計画](#planning) ] セクションで、自分のサイトをホストするリージョン。

2. 手順に従います [Azure App Service での WordPress web アプリを作成][createwordpress] 新しい WordPress web アプリを作成します。 Web アプリを作成するときに選択 **既存の MySQL データベースを使用して** 手順 1 で作成したデータベースを選択します。

既存の WordPress サイトを移行する場合は、次を参照してください。 [既存の WordPress サイトを Azure に移行](#Migrate-an-existing-WordPress-site-to-Azure) 後、新しい web アプリを作成します。

### 既存の WordPress サイトを Azure に移行する

説明したように、 [アーキテクチャと計画](#planning) セクションで、WordPress サイトを移行する 2 つの方法があります。

* **エクスポートおよびインポート** - カスタマイズの量が多くないサイトや場所、たいコンテンツを移動します。

* **バックアップし、復元** - さまざまなカスタマイズでサイトを丸ごと移行します。

以下のセクションのいずれかに従って、サイトを移行します。

####エクスポートとインポートによる移行

1. 使用 [WordPress のエクスポート][export] 、既存のサイトをエクスポートします。

2. 」の手順を使用して新しい web アプリを作成、 [新しい WordPress サイトを作成する](#Create-a-new-WordPress-site) セクションです。

3. ログインし、Web Apps で WordPress サイトをクリックして **プラグイン** ]-> [ **新規追加**します。 検索し、インストールする、 **WordPress Importer** プラグインします。

4. Importer プラグインがインストールされた後は、をクリックして **ツール** ]-> [ **インポート**, 、し、[ **WordPress** WordPress importer プラグインを使用します。

5.  **WordPress のインポート** ] ページで [ **Choose File**します。 既存の WordPress サイトからエクスポートした WXR ファイルを参照し、選択 **ファイルのアップロードとインポート**します。

6. クリックして **送信**します。 インポートに成功したことを示すメッセージが表示されます。

8. これらすべての手順を完了した後で web アプリのブレードからサイトを再起動して、 [Azure ポータル][mgmtportal]します。

サイトをインポートした後に、以下の手順を実行して、インポート ファイルに含まれない設定を有効にすることが必要になる場合があります。

使用している要素 | これを行うには、次の手順を実行します。
------------------ | ----------
**固定リンク** | 新しいサイトの WordPress ダッシュ ボードで、次のようにクリックします **設定** ]-> [ **Permalinks** し、固定リンクの構造を更新。
**イメージまたはメディア リンク** | 新しい場所へのリンクを更新する、 [Velvet Blues Update URLs プラグイン][velvet], 、検索と置換ツールでは、データベースに手動でまたは
**テーマ** | 移動して **外観** ]-> [ **テーマ** し必要に応じて、サイトのテーマを更新
**メニュー** | 使用しているテーマがメニューをサポートしている場合は、ホーム ページへのリンクに使用していないサブディレクトリが残っていることがあります。 移動して **外観** ]-> [ **メニュー** し、更新

####バックアップと復元による移行

1. バックアップを作成、既存の WordPress サイトに情報を使用して [WordPress backups][wordpressbackup]します。

2. 情報を使用して、既存のデータベースをバックアップ [データベースをバックアップする][wordpressdbbackup]です。

3. 新しいデータベースを作成し、バックアップを復元します。

    1. 新しいデータベースを購入、 [Azure Marketplace][cdbnstore], で MySQL データベースを設定または、 [Windows][mysqlwindows] または [Linux][mysqllinux] VM です。

    2. などの MySQL クライアントを使用して [MySQL Workbench][workbench], 、新しいデータベースに接続し、WordPress データベースをインポートします。

    3. データベースを更新して、ドメイン エントリを "mywordpress.azurewebsites.net" などのように、新しい Azure App Service のドメインに変更します。 使用して、 [検索し、置換用 WordPress データベース スクリプト][searchandreplace] をすべてのインスタンスを安全に変更します。

4. Azure ポータルで新しい Web アプリを作成し、WordPress のバックアップを発行します。

    1. 新しい web アプリを作成、 [Azure ポータル][mgmtportal] を使用してデータベースに **新規** ]-> [ **Web + モバイル** ]-> [ **Azure Marketplace** ]-> [ **Web Apps** ]-> [ **Web アプリ + SQL** (または **Web アプリ + MySQL**)]-> [ **作成**します。 必要なすべての設定を構成して、空の Web アプリを作成します。

    2. WordPress バックアップでは、検索、 **wp-config.php** ファイルし、エディターで開きます。 以下のエントリを新しい MySQL データベースの情報に変更します。

        * **DB_NAME** -データベースのユーザー名

        * **DB_USER** -データベースにアクセスするために使用するユーザー名

        * **DB_PASSWORD** -ユーザー パスワード

        これらのエントリを変更した後は、保存して閉じる、 **wp-config.php** ファイルです。

    3. 使用して、 [Azure App Service で web アプリのデプロイ][deploy] を使用し、WordPress バックアップを Azure App Service で web アプリに展開する展開方法を有効にする情報です。

5. WordPress サイトがデプロイされると、サイトの *.azurewebsite.net という URL を使用して新しいサイト (App Service Web アプリとして) アクセスできるようになります。

###サイトの構成

WordPress サイトを作成、移行した後は、以下の情報を参照して、パフォーマンスの向上と追加機能の有効化を行います。

目的 | 方法
------------- | -----------
**App Service プランのモード、サイズを設定し、スケーリングを有効化する** | [Azure App Service の Web アプリをスケーリングする][websitescale]
**永続的なデータベース接続を有効にする** <p>既定では、WordPress は永続的なデータベース接続を使用しない複数の接続に制限されるデータベースへの接続を引き起こす可能性があります。</p>  | <ol><li><p>編集します <strong>wp-が含まれています/wp-db.php</strong> ファイルを保存します。</p></li><li><p>次の行を見つけます。</p><code>$this->dbh = mysql_connect( $this->dbhost, $this->dbuser, $this->dbpassword, $new_link, $client_flags );</code></li><li><p>この行を次の行で置き換えます。</p><code>$dbh]-> [この = mysql_pconnect ($この]-> [dbhost dbuser、]、[この $]-> [この $dbpassword $client_flags) です。 <br/>場合 (false! $ エラー報告を = =) {/br/> & nbsp; & nbsp; エラー報告 ($レベル)。 <br/>} </code></li><li><p>次の行を見つけます。</p><code>$this->dbh = @mysql_connect( $this->dbhost, $this->dbuser, $this->dbpassword, $new_link, $client_flags ); </code></li><li><p>この行を次の行で置き換えます。</p><code>$dbh]-> [この = @mysql_pconnect ($この]-> [dbhost dbuser、]、[この $]-> [この $dbpassword $client_flags) です。 </code></li><li><p>ファイルを保存します。 <strong>wp-が含まれています/wp-db.php</strong> ファイルし、サイトを再展開します。</p></li></ol><div class="wa-note"><span class="wa-icon-bulb"></span><p>これらの変更は、WordPress の更新時に上書きされます。</p><p>WordPress は既定で自動更新を行いますが、この設定は <strong>wp-config.php</strong> ファイルを編集し、 <code>('WP_AUTO_UPDATE_CORE', false ) を追加することで、無効にできます。</code></p><p>更新に対処するもう 1 つの手段として、WebJob を使用する方法があります。WebJob は、 <strong>wp-db.php</strong> ファイルを監視し、ファイルが更新されるたびに前掲の変更を行います。 手順については、「 <a href="http://www.hanselman.com/blog/IntroducingWindowsAzureWebJobs.aspx">WebJob の概要</a> をご覧ください。</p></div>
**パフォーマンスの向上** | <ul><li><p><a href="http://ppe.blogs.msdn.com/b/windowsazure/archive/2013/11/18/disabling-arr-s-instance-affinity-in-windows-azure-web-sites.aspx">ARR クッキーの無効化</a> -複数の Web Apps インスタンスで WordPress を実行しているときに、パフォーマンスを向上させることができます</p></li><li><p>キャッシュを有効にする。 <a href="http://msdn.microsoft.com/library/azure/dn690470.aspx">Redis Cache</a> (プレビュー) を <a href="https://wordpress.org/plugins/redis-object-cache/">Redis Object Cache WordPress プラグインと</a>組み合わせて使用するか、 <a href="/gallery/store/">Azure ストア</a></p></li><li><p><a href="http://ruslany.net/2010/03/make-wordpress-faster-on-iis-with-wincache-1-1/">WordPress を Wincache で高速化する方法</a> -Web アプリ Wincache は既定で有効にします。</p></li><li><p><a href="../web-sites-scale/">Azure App Service の Web アプリをスケーリングする</a> および <a href="http://www.cleardb.com/developers/cdbr/introduction">ClearDB High Availability ルーティング</a> または <a href="http://www.mysql.com/products/cluster/">MySQL Cluster CGE</a></p></li></ul>
**ストレージ用 BLOB の使用** | <ol><li><p><a href="../storage-create-storage-account/">Azure Storage アカウントの作成</a></p></li><li><p>コンテンツ配信ネットワーク (CDN) <a href="../cdn-how-to-use/">を使用して、</a> BLOB に格納されているデータを地理的に分散させる方法を確認します。</p></li><li><p>Azure Storage for WordPress プラグインを <a href="https://wordpress.org/plugins/windows-azure-storage/">インストールして構成します</a>.</p><p>このプラグインに関する詳細な設定と構成の情報については、 <a href="http://plugins.svn.wordpress.org/windows-azure-storage/trunk/UserGuide.docx">ユーザー ガイドを参照してください</a>.</p> </li></ol>
**電子メールの有効化** | <ol><li><p><a href="/gallery/store/sendgrid/sendgrid-azure/">Azure ストアで SendGrid を有効にします。</a></p></li><li><p><a href="http://wordpress.org/plugins/sendgrid-email-delivery-simplified/">WordPress 用の SendGrid プラグインをインストールします。</a></p></li></ol>
**カスタム ドメイン名の構成** | [Azure App Service のカスタム ドメイン名の構成][customdomain]
**カスタム ドメイン名に対する HTTPS の有効化** | [Web アプリに対する HTTPS を Azure App Service で有効にする][httpscustomdomain]
**サイトの負荷分散、または地理的な分散** | [Azure Traffic Manager によるトラフィックのルーティング][trafficmanager]します。 カスタム ドメインを使用している場合は、次を参照してください [Azure App Service でのカスタム ドメイン名を構成する][customdomain] Traffic Manager を使用してカスタム ドメイン名を持つについて。
**自動化されたバックアップを有効にする** | [Azure App Service での Web アプリのバックアップ][backup]
**診断ログの有効化** | [Azure App Service の Web アプリの診断ログの有効化][log]

## 次のステップ

* [WordPress optimization (WordPress の最適化)](http://codex.wordpress.org/WordPress_Optimization)

* [Azure App Service での WordPress から Multisite への変換](web-sites-php-convert-wordpress-multisite.md)

* [Azure 用の ClearDB アップグレード ウィザード](http://www.cleardb.com/store/azure/upgrade)

* [Azure App Service で Web アプリのサブフォルダーに WordPress をホストする (ブログの投稿)](http://blogs.msdn.com/b/webapps/archive/2013/02/13/hosting-wordpress-in-a-subfolder-of-your-windows-azure-web-site.aspx)

* [Step-By-Step: Create a WordPress site using Azure (ステップ バイ ステップ: Azure で WordPress サイトを作成する)](http://blogs.technet.com/b/blainbar/archive/2013/08/07/article-create-a-wordpress-site-using-windows-azure-read-on.aspx)

* [Host your existing WordPress blog on Azure (Azure で既存の WordPress ブログをホストする)](http://blogs.msdn.com/b/msgulfcommunity/archive/2013/08/26/migrating-a-self-hosted-wordpress-blog-to-windows-azure.aspx)

* [Enabling pretty permalinks in WordPress (WordPress で見やすい固定リンクを有効にする)](http://www.iis.net/learn/extensions/url-rewrite-module/enabling-pretty-permalinks-in-wordpress)

* [How to migrate and run your WordPress blog on Azure Websites (WordPress ブログを Azure Websites に移行し、運用する方法)](http://www.kefalidis.me/2012/06/how-to-migrate-and-run-your-wordpress-blog-on-windows-azure-websites/)

* [WordPress を無料の Azure App Service で実行する方法](http://architects.dzone.com/articles/how-run-wordpress-azure)

* [WordPress on Azure in 2 minutes or less (Azure で 2 分以内に WordPress を稼動)](http://www.sitepoint.com/wordpress-windows-azure-2-minutes-less/)

* [Moving a WordPress blog to Azure - Part 1: Creating a WordPress blog on Azure（WordPress ブログを Azure に移転する - パート 1: Azure へのWordPress ブログの作成）](http://www.davebost.com/2013/07/10/moving-a-wordpress-blog-to-windows-azure-part-1)

* [Moving a WordPress blog to Azure - Part 2: Transferring your content (WordPress ブログを Azure に移転する - パート 2: コンテンツの転送)](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-transferring-your-content)

* [Moving a WordPress blog to Azure - Part 3: Setting up your custom domain (WordPress ブログを Azure に移転する - パート 3: カスタム ドメインのセットアップ)](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-3-setting-up-your-custom-domain)

* [Moving a WordPress blog to Azure - Part 4: Pretty permalinks and URL Rewrite rules（WordPress ブログを Azure に移転する - パート 4: 見やすい固定リンクと URL 書き換えルール）](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-4-pretty-permalinks-and-url-rewrite-rules)

* [Moving a WordPress blog to Azure - Part 5: Moving from a subfolder to the root（ WordPress ブログを Azure に移転する - パート 5: ルートへのサブフォルダーの移動）](http://www.davebost.com/2013/07/11/moving-a-wordpress-blog-to-windows-azure-part-5-moving-from-a-subfolder-to-the-root)

* [Azure アカウントに WordPress Web アプリを設定する方法](http://www.itexperience.net/2014/01/20/how-to-set-up-a-wordpress-website-in-your-windows-azure-account/)

* [Propping up WordPress on Azure (Azure で WordPress を設置する)](http://www.johnpapa.net/wordpress-on-azure/)

* [Tips for WordPress on Azure (Azure で WordPress を運用するヒント)](http://www.johnpapa.net/azurecleardbmysql/)

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

[performance-diagram]: ./media/web-sites-php-enterprise-wordpress/performance-diagram.png
[basic-diagram]: ./media/web-sites-php-enterprise-wordpress/basic-diagram.png
[multi-region-diagram]: ./media/web-sites-php-enterprise-wordpress/multi-region-diagram.png
[wordpress]: http://www.microsoft.com/web/wordpress
[officeblog]: http://blogs.office.com/
[bingblog]: http://blogs.bing.com/
[cdbnstore]: http://www.cleardb.com/store/azure
[storageplugin]: https://wordpress.org/plugins/windows-azure-storage/
[sendgridplugin]: http://wordpress.org/plugins/sendgrid-email-delivery-simplified/
[phpwebsite]: web-sites-php-configure.md
[customdomain]: web-sites-custom-domain-name.md
[trafficmanager]: ../traffic-manager/traffic-manager-overview.md
[backup]: web-sites-backup.md
[restore]: web-sites-restore.md
[rediscache]: https://azure.microsoft.com/documentation/services/redis-cache/
[managedcache]: http://msdn.microsoft.com/library/azure/dn386122.aspx
[websitescale]: web-sites-scale.md
[managedcachescale]: http://msdn.microsoft.com/library/azure/dn386113.aspx
[cleardbscale]: http://www.cleardb.com/developers/cdbr/introduction
[staging]: web-sites-staged-publishing.md
[monitor]: web-sites-monitor.md
[log]: web-sites-enable-diagnostic-log.md
[httpscustomdomain]: web-sites-configure-ssl-certificate.md
[mysqlwindows]: ../virtual-machines-mysql-windows-server-2008r2.md
[mysqllinux]: ../virtual-machines-linux-mysql-use-opensuse.md
[cge]: http://www.mysql.com/products/cluster/
[websitepricing]: /pricing/details/app-service/
[export]: http://en.support.wordpress.com/export/
[import]: http://wordpress.org/plugins/wordpress-importer/
[wordpressbackup]: http://wordpress.org/plugins/wordpress-importer/
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[createwordpress]: web-sites-php-web-site-gallery.md
[velvet]: https://wordpress.org/plugins/velvet-blues-update-urls/
[mgmtportal]: https://portal.azure.com/
[wordpressbackup]: http://codex.wordpress.org/WordPress_Backups
[wordpressdbbackup]: http://codex.wordpress.org/Backing_Up_Your_Database
[workbench]: http://www.mysql.com/products/workbench/
[searchandreplace]: http://interconnectit.com/124/search-and-replace-for-wordpress-databases/
[deploy]: web-sites-deploy.md
[posh]: ../install-configure-powershell.md
[Azure CLI]: ../xplat-cli-install.md
[storesendgrid]: https://azure.microsoft.com/marketplace/partners/sendgrid/sendgrid-azure/
[cdn]: ../cdn-how-to-use.md
 

