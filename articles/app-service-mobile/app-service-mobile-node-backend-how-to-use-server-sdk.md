<properties
    pageTitle="Mobile Apps 用 Node.js バックエンド サーバー SDK を操作する方法 | Azure App Service"
    description="Azure App Service Mobile Apps 用の Node.js バックエンド サーバー SDK を操作する方法について説明します。"
    services="app-service\mobile"
    documentationCenter=""
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-multiple"
    ms.devlang="node"
    ms.topic="article"
    ms.date="12/02/2015"
    ms.author="adrianhall"/>

# Azure Mobile Apps Node.js SDK の使用方法

[AZURE.INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]
&nbsp;

[AZURE.INCLUDE [app-service-mobile-note-mobile-services](../../includes/app-service-mobile-note-mobile-services.md)]

この記事では、Azure App Service Mobile Apps で Node.js バックエンドを使用する方法についての詳細な情報と例を提供します。

> [AZURE.NOTE] この SDK はプレビュー段階です。  そのため、運用環境でこの SDK を使用することは推奨されません。  このドキュメントの例で使用の v2.0.0 beta1 [azure-mobile-apps]します。

## <a name="Introduction"></a>はじめに

Azure App Service Mobile Apps は、Web アプリケーションにモバイルに最適化されたデータ アクセス Web API を追加する機能を提供します。  Azure のアプリケーション
ASP.NET および Node.js web アプリケーション サービス Mobile Apps SDK を提供しています。  この SDK を使用すると、次の処理を実行できます。

- データ アクセスのためのテーブル操作 (読み取り、挿入、更新、削除)
- カスタム API 操作

両方の操作は、認証のソーシャル id を含む、Azure App Service で許可されているすべての id プロバイダーを通じて提供します。
Facebook、Twitter、Google、Microsoft と Azure Active Directory など、エンタープライズ id 用のプロバイダー。

各ユース ケースのサンプルを検索する、 [samples directory on GitHub]します。

### <a name="howto-cmdline-basicapp"></a>方法: コマンド ラインを使用して基本的な Node.js バックエンドを作成する

Azure App Service Mobile Apps の Node.js バックエンドはすべて ExpressJS アプリケーションとして開始されます。  ExpressJS は、最も人気のある web サービス フレームワークです。
Node.js に対して使用できます。  基本的なを作成する [Express] 次のようにアプリケーション。

1. コマンドまたは PowerShell ウィンドウで、プロジェクト用の新しいディレクトリを作成します。

        mkdir basicapp

2. npm init を実行して、パッケージの構造を初期化します。

        cd basicapp
        npm init

    npm init コマンドでは、プロジェクトを初期化するための一連の質問が示されます。  以下の出力例を参照してください。

    ![npm init の出力][0]

3. npm リポジトリから express および azure-mobile-apps ライブラリをインストールします。

        npm install --save express azure-mobile-apps

4. app.js ファイルを作成して、基本的なモバイル サーバーを実装します。

        var express = require('express'),
            azureMobileApps = require('azure-mobile-apps');

        var app = express(),
            mobile = azureMobileApps();

        // Define a TodoItem table
        mobile.tables.add('TodoItem');

        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);

このアプリケーションは、1 つのエンドポイントを持つ単純なモバイルに最適化された web Api を作成 (`/tables/TodoItem`) を提供します。
基になる SQL データへの認証されていないアクセスは、動的スキーマを使用して格納します。  適して次に、
クライアント ライブラリのクイック スタート:

- [Android クライアントのクイック スタート]
- [iOS クライアントのクイック スタート]
- [Windows ストア クライアントのクイック スタート]
- [Xamarin.iOS クライアントのクイック スタート]
- [Xamarin.Android クライアントのクイック スタート]
- [Xamarin.Forms クライアントのクイック スタート]


この基本的なアプリケーションでのコードを見つけることができます、 [basicapp sample on GitHub]します。

### <a name="howto-vs2015-basicapp"></a>方法: Visual Studio 2015 を使用して Node バックエンドを作成する

Visual Studio 2015 には、IDE 内で Node.js アプリケーションを開発するための拡張機能が必要です。  起動し、ダウンロードして、インストール、 [Node.js Tools 1.1 for Visual Studio]します。  Node.js Tools for Visual Studio をインストールしたら、Express 4.x アプリケーションを作成します。

1. 開いている、 **新しいプロジェクト** ダイアログ (から **ファイル** > **新規** > **プロジェクト...**)。

2. 展開 **テンプレート** > **JavaScript** > **Node.js**します。

3. 選択、 **基本的な Azure Node.js Express 4 アプリケーション**します。

4. プロジェクト名を入力します。  をクリックして *OK*します。

    ![Visual Studio 2015 の新しいプロジェクト][1]

5. 右クリックし、 **npm** ノード **インストール新しい npm パッケージの管理]**します。

6. 最初の Node.js アプリケーションの作成時に、npm カタログを更新することが必要な場合があります。  この場合に、適宜 -] をクリックして **更新**します。

7. 入力 _azure モバイル アプリ_ 検索ボックスにします。  をクリックして、 **azure モバイル アプリ 2.0.0** パッケージ化、メニューから、[ **パッケージのインストール**します。

    ![新しい npm パッケージのインストール][2]

8. をクリックして **閉じる**します。

9. 開いている、 _app.js_ ファイルを Azure Mobile Apps SDK のサポートを追加します。  ライブラリの require ステートメントの下の 6 行目に、次のコードを追加します。

        var bodyParser = require('body-parser');
        var azureMobileApps = require('azure-mobile-apps');

    他の app.use ステートメントの後の約 27 行目に、次のコードを追加します。

        app.use('/users', users);

        // Azure Mobile Apps Initialization
        var mobile = azureMobileApps();
        mobile.tables.add('TodoItem');
        app.use('mobile');

    ファイルを保存します。

10. (この API は http://localhost:3000 に処理できる)、アプリケーションをローカルで実行するかまたは Azure に発行します。

### <a name="download-quickstart"></a>方法: Git を使用して Node.js バックエンド クイック スタート コード プロジェクトをダウンロードする

ポータルを使用して新しい Node.js モバイル アプリ バックエンドを作成すると **クイック スタート** ブレードで、新しい Node.js プロジェクトが作成され、サイトに展開します。 ポータルでは、テーブルと API を追加したり、Node.js バックエンドのコード ファイルを編集したりできます。 また、さまざまなデプロイ ツールの 1 つを使用してバックエンド プロジェクトをダウンロードすると、テーブルと API を追加または変更した後でプロジェクトを再発行できます。 詳細については、次を参照してください。、 [Azure App Service Deployment Guide]します。 次の手順では、Git リポジトリを使用して、クイック スタート プロジェクトのコードをダウンロードします。

1. Git をまだインストールしていない場合はインストールします。 Git をインストールするために必要な手順は、オペレーティング システムによって異なります。 参照してください [Git のインストール](http://git-scm.com/book/en/Getting-Started-Installing-Git) オペレーティング システム固有の配布およびインストールのガイダンスを提供します。

2. 手順に従います [web アプリのリポジトリを有効にする](../app-service-web/web-sites-publish-source-control.md#Step4) バックエンド サイトで、デプロイ ユーザー名とパスワードの変更の Git リポジトリを有効にします。

3. モバイル アプリ バックエンドのブレードのメモ、 **Git クローン URL** 設定します。

4.  次の例のように、Git 対応のコマンド ライン ツールで Git クローン URL を使用して `git clone` コマンドを実行します。このとき、必要な場合はパスワードを入力します。

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. ローカル ディレクトリ (上の例では /todolist) を参照し、プロジェクト ファイルがダウンロード済みであることを確認します。 /tables サブフォルダーには、todoitem.json ファイル (テーブルに対するアクセス許可を定義します) と、todoitem.js ファイル (テーブルの CRUD 操作スクリプトを定義します) があります。

6. プロジェクト ファイルに変更を加えた後に、次のコマンドを実行して、変更をサイトに追加、コミット、アップロードします。

        $ git commit -m "updated the table script"
        $ git push origin master

    新しいファイルをプロジェクトに追加する場合は、最初に `git add .` コマンドを実行する必要があります。

一連の新しいコミットがサイトにプッシュされるたびに、そのサイトは再発行されます。

### <a name="howto-publish-to-azure"></a>方法: Azure に Node.js バックエンドを発行する

Microsoft Azure では、Azure サービスに Azure App Service Mobile Apps Node.js バックエンドを発行するための多数のメカニズムが提供されます。  Visual Studio に統合されたデプロイ ツール、コマンドライン ツールおよびソース管理に基づく継続的なデプロイ オプションも利用します。  このトピックの詳細についてを参照してください、 [Azure App Service Deployment Guide]します。

Azure App Service には、デプロイ前に確認する必要がある Node.js アプリケーションに関する以下の特定のアドバイスがあります。

- する方法 [specify the Node Version]
- する方法 [use Node modules]

## <a name="TableOperations"></a>テーブル操作

azure-mobile-apps Node.js Server SDK では、WebAPI として Azure SQL Database に格納されたデータ テーブルを公開するためのメカニズムが提供されます。  以下の 5 つの操作が提供されます。

| 操作 | 説明 |
| --------- | ----------- |
| GET/tables/_tablename_ | テーブルのすべてのレコードを取得します。 |
| GET/tables/_tablename_/:id | テーブルの特定のレコードを取得します。 |
| POST/tables/_tablename_ | テーブルに新しいレコードを作成します。 |
| 修正プログラム/tables/_tablename_/:id | テーブルの既存のレコードを更新します。 |
| DELETE/tables/_tablename_/:id | テーブルのレコードを削除します。 |

この web Api をサポートしています [OData] 、およびサポートするために、テーブル スキーマを拡張して [offline data sync]します。

### <a name="howto-dynamicschema"></a>方法: 動的スキーマを使用してテーブルを定義する

テーブルを使用する前に定義する必要があります。  テーブルは、静的スキーマで (開発者がスキーマ内の列を定義する場合)、または動的に (SDK で受信要求に基づいてスキーマを制御する場合) 定義できます。 さらに、開発者は、定義に Javascript コードを追加することで、WebAPI の特定の側面を制御できます。

ベスト プラクティスとして、テーブル ディレクトリ内の Javascript ファイルに各テーブルを定義し、tables.import() メソッドを使用してテーブルをインポートする必要があります。
basic-app を拡張すると、app.js ファイルが次のように調整されます。

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define the database schema that is exposed
    mobile.tables.import('./tables');

    // Provide initialization of any tables that are statically defined
    mobile.tables.initialize().then(function () {
        // Add the mobile API so it is accessible as a Web API
        app.use(mobile);

        // Start listening on HTTP
        app.listen(process.env.PORT || 3000);
    });

次のように ./tables/TodoItem.js にテーブルを定義します。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Additional configuration for the table goes here

    module.exports = table;

テーブルでは、既定で動的スキーマが使用されます。  動的スキーマをグローバルにオフに、アプリケーション設定 **MS_DynamicSchema** Azure ポータル内で false にします。

完全な例を見つけることができます、 [todo sample on GitHub]します。

### <a name="howto-staticschema"></a>方法: 静的スキーマを使用してテーブルを定義する

WebAPI を使用して公開する列を明示的に定義することができます。  azure-mobile-apps Node.js SDK では、指定した一覧にオフライン データ同期に必要な他の列が自動的に追加されます。  たとえば、クイック スタート クライアント アプリケーションには、text (文字列) と complete (ブール値) という 2 つの列を持つテーブルが必要になります。  これは、テーブル定義 JavaScript ファイル (テーブル ディレクトリにある) に次のように定義できます。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    module.exports = table;

静的にテーブルを定義する場合は、tables.initialize() メソッドを呼び出して起動時にデータベース スキーマを作成する必要もあります。  Tables.initialize() メソッドが戻る、 [Promise] -これは、web サービスが初期化されているデータベースの前に要求を処理しないことを確認に使用します。

### <a name="howto-sqlexpress-setup"></a>方法: ローカル コンピューター上で開発データ ストアとして SQL Express を使用する

Azure Mobile Apps Node SDK には、データをすぐに使用できる 3 つのオプションが用意されています。

- 使用して、 **メモリ** 非永続的例ストアを提供するドライバー
- 使用して、 **mssql** ドライバー開発のための SQL Express のデータ ストアを提供するには
- 使用して、 **mssql** 実稼働環境に Azure SQL Database データ ストアを提供するドライバー

Azure Mobile Apps Node.js SDK を使用して、 [mssql Node.js package] を設定し、SQL Express と SQL データベースへの接続を使用します。  このパッケージでは、SQL Express インスタンスで TCP 接続を有効にする必要があります。

> [AZURE.TIP] メモリ ドライバーは、テストするための機能の完全なセットを提供していません。  バックエンドをローカルでテストする場合
SQL Express のデータ ストアと mssql ドライバーの使用はお勧めします。

1. ダウンロードしてインストール [Microsoft SQL Server 2014 Express]します。  必ず、SQL Server 2014 Express with Tools エディションをインストールしてください。  しない限り、明示的に
64 ビット サポートを必要な 32 ビット バージョンを実行しているときより少ないメモリは消費します。

2. SQL Server 2014 構成マネージャーを実行します。

  1. 展開、 **SQL Server ネットワーク構成** 左側にある [ツリー] メニュー内のノードです。
  2. をクリックして **SQLEXPRESS のプロトコル**します。
  3. 右クリックして **TCP/IP** 選択 **を有効にする**です。  をクリックして **OK** ] ポップアップ ダイアログです。
  4. 右クリックして **TCP/IP** 選択 **プロパティ**します。
  5. をクリックして、 **IP アドレス** ] タブをクリックします。
  6. 検索、 **IPAll** ノードです。   **TCP ポート** フィールドに「 **1433**します。

         ![Configure SQL Express for TCP/IP][3]
  7. をクリックして **OK**します。  をクリックして **OK** ] ポップアップ ダイアログです。
  8. をクリックして **SQL Server サービス** 左側にある [ツリー] メニューにします。
  9. 右クリックして **SQL Server (SQLEXPRESS)** 選択 **再起動**
  10. SQL Server 2014 構成マネージャーを閉じます。

3. SQL Server 2014 Management Studio を実行して、ローカルの SQL Express インスタンスに接続します。

  1. オブジェクト エクスプ ローラーで、インスタンスを右クリックし、選択 **プロパティ**
  2. 選択、 **セキュリティ** ページです。
  3. 確認、 **SQL Server と Windows 認証モード** が選択されています。
  4. をクリックして **OK**

        ![SQL Express 認証の構成][4]

  5. 展開 **セキュリティ** > **ログイン** オブジェクト エクスプ ローラー
  6. 右クリックして **ログイン** 選択 **新しいログインしています.**
  7. ログイン名を入力します。  選択 **SQL Server 認証**します。  パスワードを入力し、同じパスワードを入力 **パスワードの確認入力**します。  パスワードは、Windows の複雑さの要件を満たしている必要があります。
  8. をクリックして **OK**

        ![SQL Express に新しいユーザーを追加する][5]

  9. 新しいログインを右クリックし、選択 **プロパティ**
  10. 選択、 **サーバーの役割** ページ
  11. チェック ボックスをオン] の横に、 **dbcreator** サーバーの役割
  12. をクリックして **OK**
  13. SQL Server 2015 Management Studio を閉じます。

選択したユーザー名とパスワードは必ず記録してください。  特定のデータベース要件に応じて、他のサーバーの役割またはアクセス許可の割り当てが必要になる場合があります。

Node.js アプリケーションが読み取る、 **SQLCONNSTR_MS_TableConnectionString** 環境変数をこのデータベースの接続文字列を確認します。  ご使用の環境内にこれを設定することができます。  たとえば、PowerShell を使用して、以下のようにこの環境変数を設定することができます。

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

TCP/IP 接続を介して、データベースにアクセスし、接続用のユーザー名とパスワードを入力する必要があることに注意してください。

### <a name="howto-config-localdev"></a>方法: ローカル開発用のプロジェクトを構成する

Azure のモバイル アプリがという JavaScript ファイルを読み取る _azureMobile.js_ ローカル ファイル システムからです。  実稼働環境で Azure Mobile Apps SDK を構成する-内でのアプリ設定を使用してこのファイルを使用する必要があります、 [Azure Portal] 代わりにします。   _AzureMobile.js_ ファイルは、構成オブジェクトをエクスポートする必要があります。  最も一般的な設定は次のとおりです。

- データベース設定
- 診断ログ設定
- 代替 CORS 設定

例 _azureMobile.js_ は次のファイル上で指定したデータベースの設定を実装します。

    module.exports = {
        cors: {
            origins: [ 'localhost' ]
        },
        data: {
            provider: 'mssql',
            server: '127.0.0.1',
            database: 'mytestdatabase',
            user: 'azuremobile',
            password: 'T3stPa55word'
        },
        logging: {
            level: 'verbose'
        }
    };

追加することをお勧め _azureMobile.js_ に、 _.gitignore_ ファイル (またはファイルを無視するその他のソース コード管理) からパスワードを防止するには
クラウドに格納されています。  常にアプリケーション設定] で運用環境の設定を構成、 [Azure Portal]します。

### <a name="howto-appsettings"><a>モバイル アプリを構成するためのアプリ設定

ほとんどの設定、 _azureMobile.js_ ファイルに、対応するアプリケーション設定がある、 [Azure Portal]します。  次のリストを使用して、構成、
アプリケーション設定では app:

| アプリ設定                 | _azureMobile.js_ 設定  | 説明                               | 有効な値                                |
| :-------------------------- | :------------------------ | :---------------------------------------- | :------------------------------------------ |
| **MS_MobileAppName**        | name                      | アプリの名前                       | string                                      |
| **MS_MobileLoggingLevel**   | logging.level             | ログ記録するメッセージの最小ログ レベル      | error、warning、info、verbose、debug、silly |
| **MS_DebugMode**            | debug                     | デバッグ モードの有効化または無効化              | true、false                                 |
| **MS_TableSchema**          | data.schema               | SQL テーブルの既定のスキーマ名        | string (既定: dbo)                       |
| **MS_DynamicSchema**        | data.dynamicSchema        | デバッグ モードの有効化または無効化              | true、false                                 |
| **MS_DisableVersionHeader** | version (undefined に設定)| X-ZUMO-Server-Version ヘッダーの無効化 | true、false                                 |
| **MS_SkipVersionCheck**     | skipversioncheck          | クライアント API バージョン チェックの無効化     | true、false                                 |

アプリ設定を設定するには:

1. ログイン、 [Azure Portal]します。
2. 選択 **すべてのリソース** または **App Services** モバイル アプリの名前をクリックします。
3. クリックしていない場合、既定で [設定] ブレードが開きます **設定**します。
4. をクリックして **アプリケーション設定** 共通のメニュー。
5. [アプリ設定] セクションまでスクロールします。
6. アプリ設定が既に存在する場合は、アプリ設定の値をクリックして値を編集します。
7. アプリ設定が存在しない場合は、[キー] ボックスにアプリ設定を入力し、[値] ボックスに値を入力します。
8. 完了したら後でクリックして **保存**します。

ほとんどのアプリ設定を変更した場合、サービスの再起動が必要になります。

### <a name="howto-use-sqlazure"></a>方法: SQL Database を運用データ ストアとして使用する

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Azure SQL Database をデータ ストアとして使用する方法は、Azure App Service アプリケーションのすべての種類で同じです。 モバイル アプリ バックエンドをまだ作成していない場合は、次の手順に従って新しいものを作成します。

1. ログイン、 [Azure Portal]します。

2. 上にあるウィンドウの左、クリックして、 **+ 新規** ボタン > **Web + モバイル** > **モバイル アプリ**, 、モバイル アプリ バックエンド用の名前を指定します。

3.  **リソース グループ** ボックスで、アプリと同じ名前を入力します。

4. 既定の App Service プランが選択されます。  App Service プランを変更する場合は、これを行う App Service プランをクリックして > **[新規作成] +**します。  新しい App Service プランの名前を指定し、適切な場所を選択します。  [価格レベル] をクリックし、サービスに適切な価格レベルを選択します。 選択 **をすべて表示** より価格オプションをなどのビューに **Free** と **共有**します。  クリックして、価格レベルを選択すると、 **選択** ] ボタンをクリックします。  戻り、 **App Service プラン** ブレードで、をクリックして **OK**します。

5. クリックして **作成**します。 これにより、モバイル アプリ バックエンドが作成され、後でサーバー プロジェクトをデプロイすることができます。  モバイル アプリ バックエンドのプロビジョニングには数分かかる場合があります。  モバイル アプリ バックエンドがプロビジョニングされたら、ポータルが開き、 **設定** バックエンドのモバイル アプリのブレードです。

モバイル アプリ バックエンドが作成されたら、既存の SQL データベースをモバイル アプリ バックエンドに接続するか、新しい SQL データベースを作成するかを選択できます。  このセクションでは、新しい SQL データベースを作成します。

> [AZURE.NOTE] 代わりを選択できる新しいモバイル アプリ バックエンドと同じ場所にデータベースを既にがある場合 **、既存のデータベースを使用して** しそのデータベースを選択します。 別の場所にあるデータベースを使用することは、帯域幅コストと待機時間が増加するため、お勧めしません。

6. 新しいモバイル アプリ バックエンドでクリックして **設定** > **モバイル アプリ** > **データ** > **+ 追加**します。

7.  **データ接続を追加** ブレードで、をクリックして **SQL データベース - 必要な設定を構成する** > **新しいデータベースを作成**します。  新しいデータベースの名前を入力、 **名前** フィールドです。

8. クリックして **Server**します。   **新しいサーバー** ブレードで、一意のサーバーの名前を入力、 **サーバー名** フィールド、および適切な提供 **サーバー管理ログイン** と **パスワード**します。  ように **アクセス サーバーに azure のサービスを許可する** がオンになっています。  をクリックして **OK**します。

    ![Azure SQL Database の作成][6]

9.  **新しいデータベース** ブレードをクリック **OK**します。

10. 戻り、 **データ接続を追加** ブレードで、 **接続文字列**, 、ログインとデータベースを作成するときに上記で指定したパスワードを入力します。  既存のデータベースを使用する場合は、そのデータベースのログイン資格情報を入力します。  入力されると、クリックして **OK**します。

11. 戻り、 **データ接続を追加** に再びブレードをクリックして **[ok]** データベースを作成します。

<!--- END OF ALTERNATE INCLUDE -->

データベースの作成には数分かかる場合があります。  使用して、 **通知** 領域、展開の進行状況を監視するためです。  データベースのデプロイが正常に完了するまでは、先に進まないでください。  正常にデプロイされると、モバイル バックエンドの [アプリ設定] で、SQL Database インスタンスの接続文字列が作成されます。  このアプリ設定を表示、 **設定** > **アプリケーション設定** > **接続文字列**です。

### <a name="howto-tables-auth"></a>方法: テーブルへのアクセスに認証を要求する

テーブルのエンドポイントでアプリ サービスの認証を使用する場合は、[アプリ サービスの認証を行う必要があります、 [Azure Portal] 最初です。  次に
詳細については、Azure App Service で認証を構成する方法を使用する id プロバイダーの構成ガイドを確認します。

- [Azure Active Directory 認証の構成方法]
- [Facebook 認証の構成方法]
- [Google 認証の構成方法]
- [Microsoft 認証の構成方法]
- [Twitter 認証の構成方法]

各テーブルには、テーブルへのアクセスを制御するために使用できる access プロパティがあります。  次のサンプルでは、認証を必要とする静的に定義されたテーブルを示します。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

access プロパティには、次の 3 つの値を使用できます。

  - *匿名* クライアント アプリケーションが認証なしのデータの読み取りを許可されていることを示します。
  - *認証* クライアント アプリケーションが要求に有効な認証トークンを送信する必要があることを示します
  - *無効になっている* このテーブルが現在無効になっていることを示します。

access プロパティが定義されていない場合は、非認証アクセスが許可されます。

### <a name="howto-tables-disabled"></a>方法: 特定のテーブル操作へのアクセスを無効にする

テーブルでの表示だけでなく、個々の操作を制御するために access プロパティを使用できます。  操作には以下の 4 つがあります。

  - *読み取る* 、テーブルの rest ベースの取得操作は、
  - *挿入* テーブルに対して RESTful POST 操作は、
  - *更新* テーブルで RESTful PATCH 操作は、
  - *削除* 、テーブルの rest ベースの削除操作は、

たとえば、読み取り専用の非認証テーブルを指定するとします。  これは、次のようなテーブル定義で指定することができます。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Read-Only table - only allow READ operations
    table.read.access = 'anonymous';
    table.insert.access = 'disabled';
    table.update.access = 'disabled';
    table.delete.access = 'disabled';

    module.exports = table;

### <a name="howto-tables-query"></a>方法: テーブル操作で使用されるクエリを調整する

テーブル操作の一般的な要件は、データの制限付きビューを提供することです。  たとえば、あるテーブルが提供します。
ユーザーが読み取りのみ、または独自のレコードを更新できるように、認証されたユーザー ID でタグ付けします。  次のテーブルの定義
この機能を提供します。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define a static schema for the table
    table.columns = {
        "userId": "string",
        "text": "string",
        "complete": "boolean"
    };
    table.dynamicSchema = false;

    // Require authentication for this table
    table.access = 'authenticated';

    // Ensure that only records for the authenticated user are retrieved
    table.read(function (context) {
        context.query.where({ userId: context.user.id });
        return context.execute();
    });

    // When adding records, add or overwrite the userId with the authenticated user
    table.insert(function (context) {
        context.item.userId = context.user.id;
        return context.execute();
    }

    module.exports = table;

通常はクエリを実行する操作には、where 句で調整できる query プロパティがあります。    クエリのプロパティは
 [QueryJS] OData クエリをデータ バックエンドが処理できるものに変換するために使用します。  単純な等値の場合
(前述したよう)、マップを使用していることができます。 特定の SQL 句の追加も比較的容易です。

    context.query.where('myfield eq ?', 'value');

### <a name="howto-tables-softdelete"></a>方法: テーブルの論理削除を構成する

論理削除では実際にレコードは削除されません。  代わりに、削除列を true に設定して、データベース内のレコードを削除済みとしてマークします。  Mobile Client SDK で IncludeDeleted() が使用されない限り、Azure Mobile Apps SDK によって結果から論理削除レコードが自動的に削除されます。  論理削除のテーブルを構成するには、テーブル定義ファイルで softDelete プロパティを設定します。  例を以下に示します。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Turn on Soft Delete
    table.softDelete = true;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

クライアント アプリケーションから、WebJob またはカスタム メカニズムを使用して、レコードを消去するためのメカニズムを確立する必要があります。

### <a name="howto-tables-seeding"></a>方法: データベースに対するデータのシード処理を実行する

新しいアプリケーションを作成する場合、データでのテーブルのシード処理が必要になることがあります。  これは、として、テーブル定義の JavaScript ファイル内で行うことができます。
ページが開きます。

    var azureMobileApps = require('azure-mobile-apps');

    var table = azureMobileApps.table();

    // Define the columns within the table
    table.columns = {
        "text": "string",
        "complete": "boolean"
    };
    table.seed = [
        { text: 'Example 1', complete: false },
        { text: 'Example 2', complete: true }
    ];

    // Turn off dynamic schema
    table.dynamicSchema = false;

    // Require authentication to access the table
    table.access = 'authenticated';

    module.exports = table;

データのシード処理は、テーブルが Azure Mobile Apps SDK で作成されている場合にのみ行われることに注意してください。  場合は既にテーブル
存在する、データベース内でデータに挿入されていないテーブルです。  動的スキーマが有効な場合、スキーマから推論されます。
シード データです。

initialize() メソッドを明示的に呼び出して、サービスの実行開始時にテーブルを作成することをお勧めします。

## <a name="CustomAPI"></a>カスタム API

/tables エンドポイント経由のデータ アクセス API に加え、Azure Mobile Apps ではカスタム API も提供できます。  カスタム Api が定義されています。
テーブルの定義と同様の方法とまったく同じ認証などの機能にアクセスできます。

アプリ サービスの認証、カスタム API を使用する場合は、[アプリ サービスの認証を構成する必要があります、 [Azure Portal] 最初です。  次に
詳細については、Azure App Service で認証を構成する方法を使用する id プロバイダーの構成ガイドを確認します。

- [Azure Active Directory 認証の構成方法]
- [Facebook 認証の構成方法]
- [Google 認証の構成方法]
- [Microsoft 認証の構成方法]
- [Twitter 認証の構成方法]

### <a name="howto-customapi-basic"></a>方法: 単純なカスタム API を定義する

カスタム API は、テーブル API とほぼ同じ方法で定義されます。

1. 作成、 **api** ディレクトリ
2. [API 定義 JavaScript ファイルを作成、 **api** ディレクトリ。
3. インポートするには、インポート メソッドを使用して、 **api** ディレクトリ。

前に使用した basic-app サンプルに基づくプロトタイプの api 定義を以下に示します。

    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Import the Custom API
    mobile.api.import('./api');

    // Add the mobile API so it is accessible as a Web API
    app.use(mobile);

    // Start listening on HTTP
    app.listen(process.env.PORT || 3000);

使用してサーバーの日付を返す簡単な API を見てみましょう、 _Date.now()_ メソッドです。  api/date.js ファイルを以下に示します。

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };

    module.exports = api;

各パラメーターは、標準的な RESTful 動詞 (GET、POST、PATCH または DELETE) のいずれかです。  このメソッドは、標準的な [ExpressJS Middleware] 、必要な出力を送信する関数。

### <a name="howto-customapi-auth"></a>方法: カスタム API へのアクセスに認証を要求する

Azure Mobile Apps SDK では、テーブル エンドポイントとカスタム API の両方に対して同じ方法で認証を実装します。  前のセクションで開発された API に認証を追加するには、追加、 **アクセス** プロパティ。

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // All methods must be authenticated.
    api.access = 'authenticated';

    module.exports = api;

以下のように、特定の操作で認証を指定することもできます。

    var api = {
        get: function (req, res, next) {
            var date = { currentTime: Date.now() };
            res.status(200).type('application/json').send(date);
        });
    };
    // The GET methods must be authenticated.
    api.get.access = 'authenticated';

    module.exports = api;

認証を必要とするカスタム API には、テーブル エンドポイントで使用されるものと同じトークンを使用する必要があります。

## <a name="Debugging"></a>デバッグおよびトラブルシューティング

Azure App Service では、Node.js アプリケーションに関するいくつかのデバッグとトラブルシューティングの手法が提供されます。
これらの手法をすべて使用できます。

- [Azure App Service の監視]
- [Azure App Service での診断ログの有効化]
- [Visual Studio での Azure App Service のトラブルシューティング]

### <a name="howto-diagnostic-logs"></a>方法: Azure Mobile Apps の診断ログに書き込む

Node.js アプリケーションは、広範囲の診断ログ ツールにアクセスできます。  Azure Mobile Apps Node.js SDK を使用して内部的には、 [Winston] 診断ログに記録します。  これは、自動的に有効になっているデバッグ モードを有効にするか、設定、 **MS_DebugMode** アプリ設定を true に、 [Azure Portal]します。  生成されたログは、診断ログに表示されます、 [Azure Portal]します。

## <a name="in-portal-editing"></a>ポータル内のコード編集機能

Azure ポータルに用意されている特殊なツールを使用すると、コード プロジェクトをダウンロードしなくても、Node.js バックエンド プロジェクトを簡単に操作できます。 ポータルの [テーブルの簡単操作] と [API の簡単操作] を使用すると、ポータル内でテーブルとカスタム API をすぐに作成して操作できます。 Visual Studio Team Services の "Monaco" エディターを使用して、ポータル内ですぐにテーブル操作と API スクリプトを編集することもできます。

### <a name="work-easy-tables"></a>方法: Azure ポータルで [テーブルの簡単操作] を使用する

クリックすると、 **簡単テーブル** バックエンド サイトの設定新しいテーブルを追加または変更したり、既存のテーブルを削除します。 さらに、テーブル内のデータを表示することもできます。

![Work with Easy Tables](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-tables.png)

テーブル用のコマンド バーには、次のコマンドが用意されています。

+ **アクセス許可を変更** の変更、読み取りアクセス許可の挿入、更新、およびテーブルの操作を削除します。 匿名アクセスを許可するオプション、認証を要求するオプション、操作に対するすべてのアクセスを無効にするオプションがあります。 これにより、table.json プロジェクト コード ファイルは変更されます。
+ **スクリプトを編集** -テーブルのスクリプト ファイルが Visual Studio Team Services エディターで開きます。
+ **スキーマの管理** - の追加または列を削除またはテーブルのインデックスを変更します。
+ **表のクリア** -既存のテーブルを切り捨てますすべてのデータ行を削除するとは、スキーマのまま変更されません。
+ **行を削除する** -個々 のデータ行を削除します。
+ **ストリーミング ログ ビュー** -サイトのストリーミング ログ サービスに接続します。

###<a name="work-easy-apis"></a>方法: Azure ポータルで [API の簡単操作] を使用する

クリックすると、 **簡単 Api** バックエンド サイトの設定で新しいカスタム API エンドポイントを追加または変更したり既存 API エンドポイントを削除します。

![Work with Easy APIs](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-easy-apis.png)

ポータルでは、特定の HTTP アクションのアクセス許可を変更できます。また、Visual Studio Team Services エディターで API スクリプト ファイルをしたり、ストリーミング ログを表示したりできます。

###<a name="online-editor"></a>方法: Visual Studio Team Services でコードを編集する

Azure ポータルでは、ローカル コンピューターにプロジェクトをダウンロードする必要なく、Visual Studio Team Services を使用して Node.js バックエンド スクリプト ファイルを編集できます。 オンライン エディターでスクリプト ファイルを編集するには、次の手順に従います。

1. モバイル アプリ バックエンドのブレードでクリックして **すべての設定** > か、 **簡単なテーブル** または **簡単な Api**, テーブルまたは API をクリックし、クリックして **スクリプトを編集**します。 スクリプト ファイルが Visual Studio Team Services エディターで開かれます。

    ![Visual Studio Team Services コード エディター](./media/app-service-mobile-node-backend-how-to-use-server-sdk/mobile-apps-visual-studio-editor.png)

2. オンライン エディターで、コード ファイルを変更します。 変更内容は、入力時に自動的に保存されています。

エディターから、サイト上のコードを実行することもできます。


<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ../../includes/media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png

<!-- URLs -->
[Android Client QuickStart]: app-service-mobile-android-get-started.md
[iOS Client QuickStart]: app-service-mobile-ios-get-started.md
[Xamarin.iOS Client QuickStart]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android Client QuickStart]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms Client QuickStart]: app-service-mobile-xamarin-forms-get-started.md
[Windows Store Client QuickStart]: app-service-mobile-windows-store-dotnet-get-started.md
[HTML/Javascript Client QuickStart]: app-service-html-get-started.md
[offline data sync]: app-service-mobile-offline-data-sync.md
[How to configure Azure Active Directory Authentication]: app-service-mobile-how-to-configure-active-directory-authentication.md
[How to configure Facebook Authentication]: app-service-mobile-how-to-configure-facebook-authentication.md
[How to configure Google Authentication]: app-service-mobile-how-to-configure-google-authentication.md
[How to configure Microsoft Authentication]: app-service-mobile-how-to-configure-microsoft-authentication.md
[How to configure Twitter Authentication]: app-service-mobile-how-to-configure-twitter-authentication.md
[Azure App Service Deployment Guide]: ../app-service-web/web-site-deploy.md
[Monitoring an Azure App Service]: ../app-service-web/web-sites-monitor.md
[Enable Diagnostic Logging in Azure App Service]: ../app-service-web/web-sites-enable-diagnostic-log.md
[Toubleshoot an Azure App Service in Visual Studio]: ../app-service-web/web-sites-dotnet-troubleshoot-visual-studio.md
[specify the Node Version]: ../nodejs-specify-node-version-azure-apps.md
[use Node modules]: ../nodejs-use-node-mobiles-azure-apps.md
[Create a new Azure App Service]: ../app-service-web/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: http://expressjs.com/

[Azure Portal]: https://portal.azure.com/
[OData]: http://www.odata.org
[Promise]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[basicapp sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[todo sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[samples directory on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Node.js Tools 1.1 for Visual Studio]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Node.js package]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: http://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS Middleware]: http://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston


