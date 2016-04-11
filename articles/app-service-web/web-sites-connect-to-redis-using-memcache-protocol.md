<properties
    pageTitle="Memcache プロトコルを経由して Azure App Service の Web アプリを Redis Cache に接続する | Microsoft Azure"
    description="Memcache プロトコルを使用して Azure App Service の Web アプリを Redis Cache に接続する"
    services="app-service\web"
    documentationCenter="php"
    authors="SyntaxC4"
    manager="wpickett"
    editor="riande"/>

<tags
    ms.service="app-service-web"
    ms.devlang="php"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="windows"
    ms.workload="na"
    ms.date="09/16/2015"
    ms.author="cfowler"/>

# Memcache プロトコルを経由して Azure App Service の Web アプリを Redis Cache に接続する

この記事での WordPress web アプリに接続する方法を学習 [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) に [Azure Redis Cache][12] を使用して、 [Memcache][13] プロトコルです。 メモリ内のキャッシュに Memcached サーバーを使用する Web アプリがすでにある場合、Azure App Service に移行して、アプリケーション コードへの変更がほとんど、もしくはまったくない状態で Microsoft Azure でファースト パーティーのキャッシュ ソリューションが使用できます。 さらに、.NET、PHP、Node.js、Java、Python などの一般的なアプリケーション フレームワークを使用しながら、既存の Memcache の知識を利用してメモリ内のキャッシュ用に Azure Redis Cache を使い、Azure App Service で拡張性の高い分散型アプリを作成できます。  

App Service Web Apps により、Web Apps の Memcache shim でこのアプリケーション シナリオが有効になります。Memcache shim はローカルの Memcached サーバーで、Azure Redis Cache への呼び出しをキャッシュする Memcache プロキシの役割を果たします。 これにより、Memcache プロトコルを使用して通信するアプリでは Redis Cache を使用してデータをキャッシュできるようになります。 この Memcache shim はプロトコル レベルで動作するので、Memcache プロトコルを使用して通信する限り、あらゆるアプリケーションやアプリケーション フレームワークで使用できます。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## 前提条件

Web Apps の Memcache shim は、Memcache プロトコルを使用して通信することを条件に、あらゆるアプリケーションで使用できます。 この特定の例では、参照アプリケーションは、Azure Marketplace からプロビジョニングできるスケーラブルな WordPress サイトです。

次の記事で説明されている手順に従います。

* [Azure Redis Cache Service のインスタンスをプロビジョニングする][1]
* [Azure でスケーラブルな WordPress サイトをデプロイする][0]

スケーラブルな WordPress サイトをデプロイして、Redis Cache インスタンスをプロビジョニングしたら、Azure App Service Web Apps で Memcache shim を有効にできるようになります。

## Web Apps の Memcache shim を有効にする

Memcache shim を構成するためには 3 つのアプリ設定を作成する必要があります。 これを行うさまざまななどのメソッドを使用して、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715), 、 [旧ポータル][3], 、 [Azure PowerShell コマンドレット][5] または [Azure コマンド ライン インターフェイス][5]します。 この投稿のためには、ここを使用して、 [Azure ポータル][4] アプリ設定を設定します。 次の値を取得できる **設定** Redis Cache インスタンスのブレードです。

![Azure Redis Cache の設定ブレード](./media/web-sites-connect-to-redis-using-memcache-protocol/1-azure-redis-cache-settings.png)

### REDIS_HOST アプリ設定の追加

最初のアプリ設定を作成する必要がありますが、 **redis \_host** アプリ設定です。 この設定では、shim がキャッシュ情報を転送する場所を設定します。 REDIS_HOST アプリ設定から取得するに必要な値、 **プロパティ** Redis Cache インスタンスのブレードです。

![Azure Redis Cache のホスト名](./media/web-sites-connect-to-redis-using-memcache-protocol/2-azure-redis-cache-hostname.png)

アプリケーション設定のキーを設定して **redis \_host** とするアプリケーション設定の値、 **ホスト名** Redis Cache インスタンスのです。

![Web App AppSetting REDIS_HOST](./media/web-sites-connect-to-redis-using-memcache-protocol/3-azure-website-appsettings-redis-host.png)

### REDIS_KEY アプリ設定の追加

2 番目のアプリケーション設定を作成する必要がありますが、 **redis \_key** アプリ設定です。 この設定では、Redis Cache インスタンスに安全にアクセスするために必要な認証トークンを提供します。 REDIS_KEY アプリ設定のために必要な値を取得する、 **アクセス キー** Redis Cache インスタンスのブレードです。

![Azure Redis Cache の主キー](./media/web-sites-connect-to-redis-using-memcache-protocol/4-azure-redis-cache-primarykey.png)

アプリケーション設定のキーを設定して **redis \_key** とするアプリケーション設定の値、 **主キー** Redis Cache インスタンスのです。

![Azure Websites のアプリ設定の REDIS_KEY](./media/web-sites-connect-to-redis-using-memcache-protocol/5-azure-website-appsettings-redis-primarykey.png)

### MEMCACHESHIM_REDIS_ENABLE アプリ設定の追加

最後のアプリ設定は Web Apps で Memcache Shim を有効にするために使用します。REDIS_HOST と REDIS_KEY を使用して Azure Redis Cache に接続し、キャッシュの呼び出しを転送します。 アプリケーション設定のキーを設定して **MEMCACHESHIM\_REDIS\_ENABLE** し、値を **true**します。

![Web アプリのアプリ設定の MEMCACHESHIM_REDIS_ENABLE](./media/web-sites-connect-to-redis-using-memcache-protocol/6-azure-website-appsettings-enable-shim.png)

完了した時点をクリックして、次の 3 つのアプリ設定を追加すると、 **保存**します。

## PHP で Memcache 拡張機能を有効にする

アプリケーションで Memcache プロトコルを使用するには、PHP (WordPress サイトの言語フレームワーク) に Memcache 拡張機能をインストールする必要があります。

### php_memcache 拡張機能のダウンロード

参照 [PECL][6]します。 [キャッシュ] カテゴリをクリックして [memcache][7]します。 [Downloads] 列で [DLL] のリンクをクリックします。

![PHP PECL の Website](./media/web-sites-connect-to-redis-using-memcache-protocol/7-php-pecl-website.png)

Web Apps で有効な PHP バージョンの 非スレッド セーフ (NTS) x86 のリンクをダウンロードします (既定は PHP 5.4)。

![PHP PECL の Website の Memcache パッケージ](./media/web-sites-connect-to-redis-using-memcache-protocol/8-php-pecl-memcache-package.png)

### php_memcache 拡張機能を有効にする

ファイルをダウンロードしたら解凍し、アップロード、 **php \_memcache.dll** に、 **d:\\home\\site\\wwwroot\\bin\\ext\\** ディレクトリ。 php_memcache.dll を Web アプリにアップロードしたら、PHP ランタイムで拡張機能を有効にする必要があります。 Azure ポータルで Memcache 拡張機能を有効にするを開く、 **アプリケーション設定** web アプリのブレードのキーを使用して新しいアプリ設定の追加、 **php \_extensions** と値 **bin\\ext\\php_memcache.dll**します。


> [AZURE.NOTE] Web アプリは、複数の PHP 拡張機能を読み込む必要がある、PHP_EXTENSIONS の値はコンマ区切りの DLL ファイルへの相対パスのリストをする必要があります。

![Web アプリのアプリ設定の PHP_EXTENSIONS](./media/web-sites-connect-to-redis-using-memcache-protocol/9-azure-website-appsettings-php-extensions.png)

完了すると、クリックして **保存**します。

## Memcache WordPress プラグインのインストール

> [AZURE.NOTE] ダウンロードすることも、 [Memcached Object Cache プラグイン](https://wordpress.org/plugins/memcached/) を WordPress.org からです。

WordPress プラグインのページでクリックして **新規追加**します。

![WordPress プラグインのページ](./media/web-sites-connect-to-redis-using-memcache-protocol/10-wordpress-plugin.png)

検索ボックスに入力 **memcached** とキーを押します **Enter**します。

![WordPress の Add New プラグイン](./media/web-sites-connect-to-redis-using-memcache-protocol/11-wordpress-add-new-plugin.png)

検索 **Memcached Object Cache** の一覧をクリックし、 **今すぐインストール**します。

![WordPress の Install Memcache プラグイン](./media/web-sites-connect-to-redis-using-memcache-protocol/12-wordpress-install-memcache-plugin.png)

### Memcache WordPress プラグインを有効にする

>[AZURE.NOTE] このブログの指示に従って [Web Apps でサイト拡張機能を有効にする方法][8] Visual Studio チームのサービスをインストールします。

`wp-config.php` ファイルで、ファイルの最後にある編集の終了コメントの上に次のコードを追加します。

```php
$memcached_servers = array(
    'default' => array('localhost:' . getenv("MEMCACHESHIM_PORT"))
);
```

このコードが貼り付けられると、モナコではドキュメントを自動的に保存します。

次のステップは、オブジェクト キャッシュのプラグインを有効にすることです。 これは、ドラッグ アンド ドロップで **object-cache.php** から **wp コンテンツ/memcached** フォルダーを **wp コンテンツ** フォルダー Memcache オブジェクト キャッシュ機能を有効にします。

![memcache object-cache.php プラグインの場所を指定](./media/web-sites-connect-to-redis-using-memcache-protocol/13-locate-memcache-object-cache-plugin.png)

これで、 **object-cache.php** ファイルは、 **wp コンテンツ** フォルダ、Memcached オブジェクト キャッシュが有効になりました。

![memcache object-cache.php プラグインの場所を有効化](./media/web-sites-connect-to-redis-using-memcache-protocol/14-enable-memcache-object-cache-plugin.png)

## Memcache オブジェクト キャッシュ プラグインが機能していることを確認する

Web Apps の Memcache shim を有効にするためのすべての手順が完了しました。 最後にデータが Redis Cache インスタンスに入力されていることを確認します。

### Azure Redis Cache で非 SSL ポートのサポートを有効にする

>[AZURE.NOTE] この記事の執筆時に、Redis CLI がサポートしない SSL 接続がので、次の手順が必要です。

Azure ポータルで、この Web アプリ用に作成した Redis Cache インスタンスを参照します。 キャッシュのブレードが開いたら、次のようをクリックして、 **設定** アイコン。

![Azure Redis Cache の [設定] ボタン](./media/web-sites-connect-to-redis-using-memcache-protocol/15-azure-redis-cache-settings-button.png)

選択 **アクセス ポート** リストからです。

![Azure Redis Cache のアクセス ポート](./media/web-sites-connect-to-redis-using-memcache-protocol/16-azure-redis-cache-access-port.png)

クリックして **いいえ** の **SSL 経由でのみアクセスを許可する**です。

![Azure Redis Cache のアクセス ポート (SSL のみ)](./media/web-sites-connect-to-redis-using-memcache-protocol/17-azure-redis-cache-access-port-ssl-only.png)

非 SSL ポートが設定されたことが確認できます。 クリックして **保存**します。

![Azure Redis Cache の Redis アクセス ポータル (非 SSL)](./media/web-sites-connect-to-redis-using-memcache-protocol/18-azure-redis-cache-access-port-non-ssl.png)

### redis-cli から Azure Redis Cache に接続する

>[AZURE.NOTE] この手順では、その redis が開発用コンピューターでローカルにインストールされていると想定しています。 [次の手順に従って Redis をインストール][9]します。

任意のコマンドライン コンソールを開き、次のコマンドを入力します。

```shell
redis-cli –h <hostname-for-redis-cache> –a <primary-key-for-redis-cache> –p 6379
```

置換、 **< ホスト名のキャッシュ用の-redis - >** 実際の xxxxx.redis.cache.windows.net ホスト名を持つ、 **< プライマリのキーのキャッシュ用の-redis - >** キャッシュのアクセス キーを押して、 **Enter**します。 CLI が Redis Cache インスタンスに接続されたら、任意の redis コマンドを発行します。 次のスクリーンショットでは、キーの一覧表示を選択しています。

![Redis-CLI 端末から Azure Redis Cache に接続](./media/web-sites-connect-to-redis-using-memcache-protocol/19-redis-cli-terminal.png)

キーを一覧表示する呼び出しは、値を返す必要があります。 返さない場合は、Web アプリに移動してもう一度試します。

## まとめ

ご利用ありがとうございます。 これで、WordPress アプリには一元的なメモリ内キャッシュが導入され、増加するスループットをサポートできるようになりました。 Web Apps の Memcache Shim は、プログラミング言語やアプリケーション フレームワークに関係なく、あらゆる Memcache クライアントで使用できることを忘れないでください。 フィードバックを提供するか、Web アプリの Memcache shim に関する質問を投稿するには、投稿 [MSDN フォーラム][10] または [Stackoverflow][11]します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)


[0]: http://bit.ly/1F0m3tw
[1]: http://bit.ly/1t0KxBQ
[2]: http://manage.windowsazure.com
[3]: http://portal.azure.com
[4]: ../powershell-install-configure.md
[5]: /downloads
[6]: http://pecl.php.net
[7]: http://pecl.php.net/package/memcache
[8]: http://blog.syntaxc4.net/post/2015/02/05/how-to-enable-a-site-extension-in-azure-websites.aspx
[9]: http://redis.io/download#installation
[10]: https://social.msdn.microsoft.com/Forums/home?forum=windowsazurewebsitespreview
[11]: http://stackoverflow.com/questions/tagged/azure-web-sites
[12]: /services/cache/
[13]: http://memcached.org

