<properties
   pageTitle="Web アプリに対して DevOps 環境を効果的に使用する"
   description="デプロイメント スロットを使用してアプリケーションの複数の開発環境をセットアップおよび管理する方法を説明する"
   services="app-service\web"
   documentationCenter=""
   authors="sunbuild"
   manager="yochayk"
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="web"
   ms.date="09/24/2015"
   ms.author="sumuth"/>

# Web アプリに対して DevOps 環境を効果的に使用する

この記事をセットアップして、開発、ステージング、QA および実稼働環境など、アプリケーションの複数のバージョンの web アプリケーションの展開の管理では説明します。 アプリケーションの各バージョンは、デプロイメント プロセスでの特定のニーズに対応する開発環境であると考えることができます。たとえば、開発チームは品質保証環境を使用することで、変更を運用環境にプッシュする前にアプリケーションの品質をテストすることができます。
複数の開発環境を設定することは、困難な作業となる可能性があります。リソース (コンピューティング、Web アプリ、データベース、キャッシュなど) の追跡および管理を行う必要があるからです。これらの環境で、ある環境のコンテンツを別の環境にデプロイします。

## 非運用環境 (ステージング、開発、品質保証) のセットアップ
運用 Web アプリがアップされ実行されたら、次のステップとして非運用環境を作成します。 展開を使用するのにはスロットようにで実行していることを確認して、 **標準** または **Premium** App Service プラン モードです。 デプロイ スロットは、実際には固有のホスト名を持つライブ Web アプリです。 Web アプリのコンテンツと構成の各要素は、(運用スロットを含む) 2 つのデプロイ スロットの間でスワップすることができます。 デプロイ スロットにアプリケーションをデプロイすることには、次のメリットがあります。

1. ステージング デプロイ スロットで Web アプリの変更を検証した後に、運用スロットにスワップできます。
2. スロットに Web アプリをデプロイした後に運用サイトにスワップすると、運用サイトへのスワップ前にスロットのすべてのインスタンスが準備されます。 これにより、Web アプリをデプロイする際のダウンタイムがなくなります。 トラフィックのリダイレクトはシームレスであるため、スワップ操作によりドロップされる要求はありません。 このワークフロー全体を構成することで自動化できます [自動スワップ](web-sites-staged-publishing/#configure-auto-swap-for-your-web-app) スワップ前の検証が必要ない場合。
3. スワップ後も、以前のステージング Web アプリ スロットに元の運用 Web アプリが残っているため、 運用スロットにスワップした変更が想定どおりでない場合は、適切な動作が確認されている元の Web アプリにすぐに戻すことができます。

ステージング展開スロットを設定するには、次を参照してください。 [Azure App Service で web アプリのステージング環境をセットアップ](web-sites-staged-publishing) します。 どの環境も独自のリソース セットを備える必要があります。たとえば、Web アプリでデータベースを使用する場合は、運用 Web アプリとステージング Web アプリの両方がそれぞれ別々のデータベースを使用する必要があります。  ステージング開発環境を設定する場合は、データベース、ストレージ、キャッシュなどのステージング開発環境リソースを追加します。

## 複数の開発環境を使用する例

任意のプロジェクトが少なくとも 2 つの環境でソース コード管理に従う必要がありますがときに、コンテンツ管理システムを使用して、開発および運用環境アプリケーション フレームワークなど、アプリケーションがすぐには、このシナリオをサポートしない問題が発生実行可能性があります。 このことは、以下に説明するいくつかの一般的なフレームワークの場合に当てはまります。 CMS/フレームワークを使用する場合は、以下に示すような多くの質問が頭に浮かびます。

1. それをさまざまな環境に分割するにはどうすればよいか
2. 変更できるファイルはどれか、フレームワークのバージョン更新プログラムに影響を与えないファイルはどれか
3. 環境ごとに構成を管理するにはどうすればよいか
4. モジュール/プラグインのバージョン更新プログラム、コア フレームワークのバージョン更新プログラムを管理するにはどうすればよいか

プロジェクトのために複数の環境をセットアップする方法は多数あります。以下の例では、それぞれのアプリケーションについて該当する方法を 1 つだけ紹介します。

### WordPress
このセクションでは、WordPress のスロットを使用してデプロイメント ワークフローをセットアップする方法を説明します。 ほとんどの CMS ソリューションの場合と同様に WordPress でも、そのまま、複数の開発環境を使用することはできません。 App Service Web Apps には、コードの外部に構成設定を容易に格納できる機能がいくつかあります。

ステージング スロットを作成する前に、複数の環境をサポートするようにアプリケーション コードをセットアップします。 WordPress で複数の環境をサポートするには、ローカル開発 Web アプリの `wp-config.php` を編集し、ファイルの先頭に次のコードを追加する必要があります。 これにより、アプリケーションは選択された環境に基づく適切な構成を利用できるようになります。

```
// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) {
    // local development
    $config_file = 'config/wp-config.local.php';
}
elseif  ((strpos(getenv('WP_ENV'),'stage') !== false) ||  (strpos(getenv('WP_ENV'),'prod' )!== false )){
      //single file for all azure development environments
      $config_file = 'config/wp-config.azure.php';
}
$path = dirname(__FILE__) . '/';
if (file_exists($path . $config_file)) {
// include the config file if it exists, otherwise WP is going to fail
require_once $path . $config_file;
```

呼ばれる web アプリのルートの下のフォルダーを作成 `config` 2 番目のファイルのファイルを追加します。  `wp-config.azure.php`  と `wp-config.local.php`  、azure とローカルの環境をそれぞれ表します。

`wp-config.local.php` に次のコードをコピーします。

```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', 'yourdatabasename');

/** MySQL database username */
define('DB_USER', 'yourdbuser');

/** MySQL database password */
define('DB_PASSWORD', 'yourpassword');

/** MySQL hostname */
define('DB_HOST', 'localhost');
/**
 * For developers: WordPress debugging mode.
 * * Change this to true to enable the display of notices during development.
 * It is strongly recommended that plugin and theme developers use WP_DEBUG
 * in their development environments.
 */
define('WP_DEBUG', true);

//Security key settings
define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');

/**
 * WordPress Database Table prefix.
 *
 * You can have multiple installations in one database if you give each a unique
 * prefix. Only numbers, letters, and underscores please!
 */
$table_prefix  = 'wp_';
```

上記のセキュリティ キーを設定することは、Web アプリをハッキングから守ることに役立つので、一意の値を使用します。 上記で説明したセキュリティ キーの文字列を生成する必要がある場合は、この [リンク] (https://api.wordpress.org/secret-key/1.1/salt) を使用して新しいキー/値を作成する自動ジェネレーターを移動できます。

`wp-config.azure.php` に次のコードをコピーします。


```
<?php
// MySQL settings
/** The name of the database for WordPress */

define('DB_NAME', getenv('DB_NAME'));

/** MySQL database username */
define('DB_USER', getenv('DB_USER'));

/** MySQL database password */
define('DB_PASSWORD', getenv('DB_PASSWORD'));

/** MySQL hostname */
define('DB_HOST', getenv('DB_HOST'));

/**
* For developers: WordPress debugging mode.
*
* Change this to true to enable the display of notices during development.
* It is strongly recommended that plugin and theme developers use WP_DEBUG
* in their development environments.
* Turn on debug logging to investigate issues without displaying to end user. For WP_DEBUG_LOG to
* do anything, WP_DEBUG must be enabled (true). WP_DEBUG_DISPLAY should be used in conjunction
* with WP_DEBUG_LOG so that errors are not displayed on the page */

*/
define('WP_DEBUG', getenv('WP_DEBUG'));
define('WP_DEBUG_LOG', getenv('TURN_ON_DEBUG_LOG'));
define('WP_DEBUG_DISPLAY',false);

//Security key settings
/** If you need to generate the string for security keys mentioned above, you can go the automatic generator to create new keys/values: https://api.wordpress.org/secret-key/1.1/salt **/
define('AUTH_KEY' ,getenv('DB_AUTH_KEY'));
define('SECURE_AUTH_KEY',  getenv('DB_SECURE_AUTH_KEY'));
define('LOGGED_IN_KEY', getenv('DB_LOGGED_IN_KEY'));
define('NONCE_KEY', getenv('DB_NONCE_KEY'));
define('AUTH_SALT',  getenv('DB_AUTH_SALT'));
define('SECURE_AUTH_SALT', getenv('DB_SECURE_AUTH_SALT'));
define('LOGGED_IN_SALT',   getenv('DB_LOGGED_IN_SALT'));
define('NONCE_SALT',   getenv('DB_NONCE_SALT'));

/**
* WordPress Database Table prefix.
*
* You can have multiple installations in one database if you give each a unique
* prefix. Only numbers, letters, and underscores please!
*/
$table_prefix  = getenv('DB_PREFIX');
```

#### 相対パスの使用
最後に、WordPress アプリが相対パスを使用できるようにします。 WordPress では、データベースに URL 情報を格納します。 この方法だと、1 つの環境から別の環境にコンテンツを移動するのがより難しくなります。ローカルからステージまたはステージから運用環境に移動するたびにデータベースを更新する必要があるためです。 1 つの環境から別の使用法をデプロイするたびに、データベースを展開すると発生することが問題のリスクを軽減する、 [相対ルート プラグインをリンクする](https://wordpress.org/plugins/root-relative-urls/) WordPress 管理者のダッシュ ボードを使用してインストールするかから手動でダウンロードを [ここ](https://downloads.wordpress.org/plugin/root-relative-urls.zip)します。

`wp-config.php` ファイルに、`That's all, stop editing!` コメントの前に次のエントリを追加します。

```
    define('WP_HOME', 'http://' . $_SERVER ['HTTP_HOST']);
    define('WP_SITEURL', 'http://' . $_SERVER ['HTTP_HOST']);
    define('WP_CONTENT_URL', '/wp-content');
    define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);
```
WordPress の管理者用ダッシュボードの [`Plugins`] メニューでプラグインをアクティブにします。  WordPress アプリの固定リンク設定を保存します。

#### 最終の `wp-config.php` ファイル
WordPress 中核となる更新プログラムには影響しません、 `wp-config.php` , 、`wp-config.azure.php` と `wp-config.local.php` ファイルです。 最終的に、この `wp-config.php` ファイルは次のようになります。

```
<?php
/**
 * The base configurations of the WordPress.
 *
 * This file has the following configurations: MySQL settings, Table Prefix,
 * Secret Keys, and ABSPATH. You can find more information by visiting
 *
 * Codex page. You can get the MySQL settings from your web host.
 *
 * This file is used by the wp-config.php creation script during the
 * installation. You don't have to use the web web app, you can just copy this file
 * to "wp-config.php" and fill in the values.
 *
 * @package WordPress
 */

// Support multiple environments
// set the config file based on current environment
if (strpos($_SERVER['HTTP_HOST'],'localhost') !== false) { // local development
    $config_file = 'config/wp-config.local.php';
}
elseif  ((strpos(getenv('WP_ENV'),'stage') !== false) ||  (strpos(getenv('WP_ENV'),'prod' )!== false )){
    $config_file = 'config/wp-config.azure.php';
}


$path = dirname(__FILE__) . '/';
if (file_exists($path . $config_file)) {
    // include the config file if it exists, otherwise WP is going to fail
    require_once $path . $config_file;
}

/** Database Charset to use in creating database tables. */
define('DB_CHARSET', 'utf8');

/** The Database Collate type. Don't change this if in doubt. */
define('DB_COLLATE', '');


/* That's all, stop editing! Happy blogging. */

define('WP_HOME', 'http://' . $_SERVER['HTTP_HOST']);
define('WP_SITEURL', 'http://' . $_SERVER['HTTP_HOST']);
define('WP_CONTENT_URL', '/wp-content');
define('DOMAIN_CURRENT_SITE', $_SERVER['HTTP_HOST']);

/** Absolute path to the WordPress directory. */
if ( !defined('ABSPATH') )
    define('ABSPATH', dirname(__FILE__) . '/');

/** Sets up WordPress vars and included files. */
require_once(ABSPATH . 'wp-settings.php');
```

#### ステージング環境のセットアップ
ログインし、Azure Web サイトで実行する WordPress web アプリが既に [Azure ポータル](http://portal.azure.com) WordPress web アプリに移動します。 そうでない場合は、Marketplace から Web アプリを作成できます。 詳細については、クリックして [ここ](web-sites-php-web-site-gallery)します。
[設定]、[デプロイメント スロット]、[追加] の順にクリックし、名前ステージでデプロイメント スロットを作成します。デプロイメント スロットは、上記で作成したプライマリ Web アプリケーションと同じリソースを共有する別の Web アプリケーションです。

![ステージ デプロイメント スロットを作成する](./media/app-service-web-staged-publishing-realworld-scenarios/1setupstage.png)

別の MySQL データベース (たとえば、`wordpress-stage-db`) をリソース グループ `wordpressapp-group` に追加します。

 ![リソース グループに MySQL データベースを追加する](./media/app-service-web-staged-publishing-realworld-scenarios/2addmysql.png)

ステージ デプロイメント スロットの接続文字列を、新規に作成したデータベース `wordpress-stage-db` を指すように更新します。 運用 Web アプリ `wordpressapp-group` とステージング Web アプリ `wordpressprodapp-stage` はそれぞれ別のデータベースを指す必要があるので注意してください。

#### 環境固有のアプリ設定の構成
開発者は、キー値の文字列ペアを、App Settings と呼ばれる Web アプリに関連付けられた構成情報の一部として Azure に格納することができます。 実行時に、App Service Web Apps はこれらの値を自動的に取得し、Web アプリで実行されているコードから利用できるようにします。  この機能は、パスワードを使用したデータベース接続文字列などの機密情報が `wp-config.php` などのファイルにクリア テキストとして現れないのでセキュリティ上のメリットがあります。

以下に定義したこのプロセスは、ファイルの変更と WordPress アプリのデータベースの変更の両方が含まれている場合に実行する場合に便利です。

- WordPress バージョンのアップグレード
- 新しいプラグインの追加、プラグインの編集またはアップグレード
- 新しいテーマの追加、テーマの編集またはアップグレード

アプリ設定の構成:

- データベース情報
- オン/オフ WordPress ログ
- WordPress のセキュリティ設定

![Wordpress Web アプリ用のアプリ設定](./media/app-service-web-staged-publishing-realworld-scenarios/3configure.png)

運用 Web アプリとステージ スロット用に次のアプリ設定が追加されていることを確認します。 運用 Web アプリとステージング Web アプリはそれぞれ別のデータベースを使用するので注意してください。
オフに **スロット設定** WP_ENV を除くすべての設定パラメーターのチェック ボックスをオンします。 これにより、Web アプリの構成と、ファイルの内容およびデータベースがスワップされます。 場合 **スロット設定** は **Checked** , 、web アプリのアプリ設定、接続文字列の構成では、スワップ操作を実施する際に、環境間で移動されますおよびためデータベースの変更が存在する場合これは表示されません、運用 web アプリを中断します。

WebMatrix または任意のツール (FTP、Git、PhpMyAdmin など) を使用して、ローカル開発環境の Web アプリをステージ Web アプリおよびデータベースにデプロイします。

![WordPress Web アプリの [Web Matrix 公開] ダイアログ ボックス](./media/app-service-web-staged-publishing-realworld-scenarios/4wmpublish.png)

ステージング Web アプリを参照し、テストします。 Web アプリのテーマが更新され得るシナリオについて検討します。ここでは、ステージング Web アプリケーションを扱います。

![スロットをスワップする前にステージング Web アプリを参照する](./media/app-service-web-staged-publishing-realworld-scenarios/5wpstage.png)


 問題がなければ場合、] をクリックして **スワップ** で実稼働環境に、コンテンツを移動するステージング web アプリの設定] ボタンをクリックします。 ここで web アプリとデータベースの中に環境全体でスワップすべて **スワップ** 操作します。

![Wordpress のプレビュー変更をスワップする](./media/app-service-web-staged-publishing-realworld-scenarios/6swaps1.png)

 > [AZURE.NOTE]
 >しかし、ファイル (データベースを更新しない) をプッシュする必要があるシナリオがある場合 **チェック** 、 **スロット設定** 、関連するすべてのデータベース *アプリ設定* と *接続文字列の設定* スワップを実行する前に Azure ポータル内で web アプリ設定] ブレードでします。 このケース DB_NAME、DB_HOST、DB_PASSWORD、DB_USER、既定の接続文字列の設定は表示されません変更のプレビューで実施する際に、 **スワップ**します。 完了すると、この時点で、 **スワップ** 操作 WordPress web アプリが更新プログラム ファイルが **のみ**します。

スワップを実行する前に実稼働の WordPress web アプリは、ここで
![スロットをスワップする前の運用 Web アプリ](./media/app-service-web-staged-publishing-realworld-scenarios/7bfswap.png)

スワップ操作を実施したところ、運用 Web アプリでテーマが更新されました。

![スロットをスワップした後の運用 Web アプリ](./media/app-service-web-staged-publishing-realworld-scenarios/8afswap.png)

必要なときの状況で **ロールバック** , 、運用 web アプリの設定に移動し、をクリックできます **スワップ** を web アプリとデータベース運用からステージング スロットにスワップ] ボタンをクリックします。 注意すべき重要ながいる場合は、データベースの変更が含まれています、 **スワップ** なりますの時点で、次を現在のデータベースにデータベースの変更を配置する必要があるステージング web アプリを再展開するときに、操作がある可能性がある以前の運用ステージ データベース、ステージング web アプリが指します。

#### 概要
データベースを使用する任意のアプリケーションのプロセスを汎用化するには

1. ローカル環境にアプリケーションをインストールします。
2. 環境固有の構成を含めます (ローカルおよび Azure Web アプリケーション)。
3. App Service Web Apps-ステージング、実稼働環境をセットアップします。
4. 運用アプリケーションが Azure で既に実行されている場合は、運用コンテンツ (ファイル/コード、およびデータベース) をローカルおよびステージング環境と同期します。
5. ローカル環境でアプリケーションを開発します。
6. メンテナンスやロック モードと同期データベースの下で運用 web アプリのステージング環境と開発環境を運用環境からコンテンツを配置します。
7. ステージング環境とテストに展開します。
8. 運用環境にデプロイします。
9. 手順 4. ~ 6. を繰り返します

### Umbraco
このセクションでは、Umbraco CMS がカスタム モジュールを使用して複数の DevOps 環境間でデプロイをどのように行うのかを説明します。この例では、複数の開発環境を管理するための異なるアプローチを紹介します。

[Umbraco CMS](http://umbraco.com/) は、人気のある .NET CMS 使用するソリューションの多くの開発者が提供する 1 つ [Courier2](http://umbraco.com/products/more-add-ons/courier-2) モジュールを開発から運用環境にステージング環境にデプロイします。 Umbraco CMS Web アプリのローカル開発環境は、Visual Studio または WebMatrix を使用して簡単に作成することができます。

1. Visual Studio での Umbraco web アプリの作成] をクリックして [ここ](https://our.umbraco.org/documentation/Installation/install-umbraco-with-nuget) します。
2. WebMatrix を使用した Umbraco web アプリを作成するにはクリックして [ここ](http://umbraco.com/help-and-support/video-tutorials/getting-started/working-with-webmatrix)します。

アプリケーションにおいて `install` フォルダーを削除することを忘れないでください。また、このフォルダーを、ステージ Web アプリまたは運用 Web アプリに決してアップロードしないでください。 このチュートリアルでは、WebMatrix を使用します。

#### ステージング環境のセットアップ
Umbraco CMS の Web アプリが既にアップされ実行されている場合は、Umbraco CMS Web アプリ用に前述のデプロイメント スロットを作成します。 そうでない場合は、Marketplace から Web アプリを作成することができます。

新しく作成したデータベースを指す、ステージのデプロイ スロットの接続文字列を更新 **umbraco ステージ db**します。 運用 web アプリ (umbraositecms-1) とステージング web アプリ (umbracositecms 1 段階) **必要があります** 別のデータベース] をポイントします。

![新しいステージング データベースでステージング Web アプリの接続文字列を更新する](./media/app-service-web-staged-publishing-realworld-scenarios/9umbconnstr.png)

をクリックして  **発行取得設定** デプロイ スロットの **ステージ**します。 これにより、発行設定ファイルがダウンロードされます。このファイルには、Visual Studio または Webmatrix がアプリケーションをローカル開発 Web アプリから Azure Web アプリへ発行するのに必要とする情報がすべて格納されています。

 ![ステージング Web アプリの発行設定を取得する](./media/app-service-web-staged-publishing-realworld-scenarios/10getpsetting.png)

- ローカルの開発用の web アプリを開きます **WebMatrix** または **Visual Studio**します。 このチュートリアルでは Web Matrix を使用します。そこで、まず、ステージング Web アプリの発行設定ファイルをインポートする必要があります。

![Web Matrix を使用して Umbraco の発行設定をインポートする](./media/app-service-web-staged-publishing-realworld-scenarios/11import.png)

- ダイアログ ボックスで変更を確認し、ローカルの web アプリを Azure web アプリにデプロイ *umbracositecms 1 段階*します。 ファイルをステージング Web アプリに直接デプロイする場合は、`~/app_data/TEMP/` フォルダー内のファイルを除外します。これらのファイルは、ステージ Web アプリが最初に起動されたときに再生成されます。 `~/app_data/umbraco.config` ファイルも、再生成されるので、除外します。

![Web Matrix での発行に関する変更を確認する](./media/app-service-web-staged-publishing-realworld-scenarios/12umbpublish.png)

- Umbraco ローカル Web アプリがステージング Web アプリに正常に発行されたら、ステージング Web アプリを参照し、いくつかのテストを実行して、もし問題があれば排除します。

#### Courier2 デプロイメント モジュールのセットアップ
 [Courier2](http://umbraco.com/products/more-add-ons/courier-2) については、運用 web アプリをステージング web アプリの右ボタンでコンテンツをプッシュするモジュール、スタイル シート、開発モジュールなどに無料の展開および更新プログラムを展開するときに、運用環境の web アプリを中断するリスクを減らすことが面倒な作業です。
ドメインの Courier2 のライセンスを購入 `*.azurewebsites.net` とカスタム ドメイン (たとえば http://abc.com) ライセンスを購入すると、ダウンロードしたライセンスに配置 (します。使用許諾契約書ファイル) で、 `bin` フォルダーです。

![Bin フォルダーの下にライセンス ファイルをドロップする](./media/app-service-web-staged-publishing-realworld-scenarios/13droplic.png)

Courier2 パッケージをダウンロード [ここ](https://our.umbraco.org/projects/umbraco-pro/umbraco-courier-2/) します。 ステージ web アプリにログオンし、http://umbracocms-site-stage.azurewebsites.net/umbraco と答えるをクリックして **開発者** メニューをクリック **パッケージ** します。 をクリックして **インストール** ローカル パッケージ

![Umbraco パッケージ インストーラー](./media/app-service-web-staged-publishing-realworld-scenarios/14umbpkg.png)

インストーラーを使用して courier2 パッケージをアップロードします。

![Courier モジュールのパッケージをアップロードする](./media/app-service-web-staged-publishing-realworld-scenarios/15umbloadpkg.png)

構成する [courier.config ファイルを更新する必要があります  **Config** web アプリのフォルダーです。

```xml
<!-- Repository connection settings -->
  <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
  <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear:  -->
        <repository name="production web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
            <url>http://umbracositecms-1.azurewebsites.net</url>
            <user>0</user>
            <!--<login>user@email.com</login> -->
            <!-- <password>user_password</password>-->
           <!-- <passwordEncoding>Clear</passwordEncoding>-->
           </repository>
  </repositories>
 ```

 `<repositories>`, 、運用サイトの URL とユーザー情報を入力します。 既定の Umbraco メンバーシップ プロバイダーを使用している場合は、その管理ユーザーの ID を追加し <user> 参照してください。 カスタム Umbraco メンバーシップ プロバイダーを使用している場合は使用 `<login>`,、`<password>` Courier2 モジュールに運用サイトに接続する方法を理解します。 詳細については、確認、 [ドキュメント](http://umbraco.com/help-and-support/customer-area/courier-2-support-and-download/developer-documentation) の媒体使用モジュールです。

同様に、運用サイトで Courier モジュールをインストールし、ここで示すように、それぞれの courier.config ファイルでステージ Web アプリをポイントするように構成します。

```xml
  <!-- Repository connection settings -->
  <!-- For each site, a custom repository must be configured, so Courier knows how to connect and authenticate-->
  <repositories>
        <!-- If a custom Umbraco Membership provider is used, specify login & password + set the passwordEncoding to clear:  -->
        <repository name="Stage web app" alias="stage" type="CourierWebserviceRepositoryProvider" visible="true">
            <url>http://umbracositecms-1-stage.azurewebsites.net</url>
            <user>0</user>
           </repository>
  </repositories>
```

Umbraco CMS web アプリのダッシュ ボードで Courier2 タブをクリックし、場所を選択します。 `courier.config` 説明したようにリポジトリ名が表示されます。運用 Web アプリとステージング Web アプリの両方で同じ操作を行います。

![宛先 Web アプリ リポジトリを表示する](./media/app-service-web-staged-publishing-realworld-scenarios/16courierloc.png)

次に、ステージング サイトから運用サイトに何らかのコンテンツをデプロイします。 [コンテンツ] に移動し、既存のページを選択するか、または新しいページを作成します。 ページのタイトルが変更されている web アプリケーションから既存のページを選択 **はじめに: 新しい** [今すぐ] をクリックして **を保存して発行**します。

![ページのタイトルを変更して公開する](./media/app-service-web-staged-publishing-realworld-scenarios/17changepg.png)

これで変更されたページを選択し、 *を右クリックして* すべてのオプションを表示します。 をクリックして **媒体使用** および展開] ダイアログ ボックスを表示します。 をクリックして **展開** 展開を開始するには

![Courier モジュールのデプロイメント ダイアログ ボックス](./media/app-service-web-staged-publishing-realworld-scenarios/18dialog1.png)

変更を確認し、[続行] をクリックします。

![Courier モジュールのデプロイメント ダイアログ ボックスの確認対象の変更](./media/app-service-web-staged-publishing-realworld-scenarios/19dialog2.png)

デプロイメント ログに、デプロイが正常に完了したかどうかが示されます。

 ![Courier モジュールからのデプロイメント ログを表示する](./media/app-service-web-staged-publishing-realworld-scenarios/20successdlg.png)

運用 Web アプリを参照し、変更が反映されているかどうかを確認します。

 ![運用 Web アプリを参照する](./media/app-service-web-staged-publishing-realworld-scenarios/21umbpg.png)

Courier の詳細な使用方法については、ドキュメントを参照してください。

#### Umbraco CMS バージョンのアップグレード方法

Courier では、Umbraco CMS のバージョン間のアップグレードに関するヘルプをデプロイしません。 Umbraco CMS のバージョンをアップグレードする場合は、カスタム モジュールまたはサード パーティ モジュールとの互換性、および Umbraco のコア ライブラリとの互換性が大丈夫かどうかを確認する必要があります。 ベスト プラクティスを以下に示します。

1. アップグレードを実行する前に、Web アプリとデータベースを必ずバックアップしてください。 Azure Web App では、バックアップ機能を使用して Web サイトの自動バックアップをセットアップできます。また、必要に応じて、復元機能を使用してサイトを復元することができます。 詳細については、次を参照してください。 [web アプリをバックアップする方法](web-sites-backup) と [web アプリを復元する方法](web-sites-restore)します。

2. 現在使用しているサードパーティ製のパッケージが、アップグレード先のバージョンと互換性があるかどうかを確認します。 パッケージのダウンロード ページで、プロジェクトが Umbraco CMS のバージョンと互換性があることを確認します。

前述のようにローカルで web アプリをアップグレードする方法の詳細について以下のガイドライン [ここ](https://our.umbraco.org/documentation/getting-started/setup/upgrading/general)します。

ローカル開発サイトがアップグレードされたら、変更内容をステージング Web アプリに発行します。 アプリケーションをテストし、問題がなければ場合は、使用 **スワップ** ボタン **スワップ** 、ステージング サイトの運用 web アプリです。 実行するときに、 **スワップ** 操作、web アプリの構成に影響が及びます変更を表示することができます。 この **スワップ** web アプリとデータベース スワップして操作します。 すなわち、スワップ後、運用 Web アプリは umbraco-stage-db データベースを指すようになり、ステージング Web アプリは umbraco-prod-db データベースを指すようになります。

![Umbraco CMS をデプロイするためのスワップ プレビュー](./media/app-service-web-staged-publishing-realworld-scenarios/22umbswap.png)

Web アプリとデータベースの両方をスワップすることのメリット:  
1. 別の web アプリの以前のバージョンにロールバックすることができます **スワップ** アプリケーション問題がある場合。
2. アップグレードでは、ステージング Web アプリから運用 Web アプリおよびデータベースに、ファイルおよびデータベースをデプロイする必要があります。 ファイルとデータベースをデプロイする場合は、問題となる可能性のある事柄が多数あります。 使用して **スワップ** 機能のアップグレード時にダウンタイムを低減して、スロットの変更を配置するときに発生する障害のリスクを軽減しています。
3. 行うことができます **A と B のテストを実施** を使用して [実稼働環境でテスト](http://azure.microsoft.com/documentation/videos/introduction-to-azure-websites-testing-in-production-with-galin-iliev/) 機能

この例では、Umbraco Courier モジュールに類似したカスタム モジュールを作成して複数の環境でデプロイメントを管理する場合のプラットフォームの柔軟性について示しています。

## 参照
[Azure App Service を使用したアジャイル ソフトウェア開発](app-service-agile-software-development)

[Azure App Service の Web アプリのステージング環境を設定する](web-sites-staged-publishing)

[運用環境以外のデプロイメント スロットへの Web アクセスを禁止する方法](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)


