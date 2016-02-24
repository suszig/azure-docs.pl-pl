<properties 
    pageTitle="Azure App Service での Web アプリの構成" 
    description="Azure App Service での Web アプリの構成方法" 
    services="app-service" 
    documentationCenter="" 
    authors="erikre" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/08/2015" 
    ms.author="erikre"/>


# Azure App Service での Web アプリの構成 #

Web アプリを構成する方法に関するトピック、 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=529715)します。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## アプリケーションの設定

1.  [Azure ポータル](https://portal.azure.com), 、web アプリのブレードを開きます。
2. クリックして **設定をすべて**です。
3. クリックして **アプリケーション設定**します。

![](./media/web-sites-configure/configure01.png)

 **アプリケーション設定** ブレードにはいくつかのカテゴリにグループ化された設定します。

### 全般設定

**Framework のバージョン**です。 アプリでこれらのフレームワークを使用する場合は、以下のオプションを設定します。 

- **.NET framework**: .NET framework のバージョンを設定します。 
- **PHP**: PHP のバージョンを設定または * * OFF * * PHP を無効にします。 
- **Java**: Java のバージョンを選択または **OFF** Java を無効にします。 使用して、 **Web コンテナー** か Tomcat と Jetty のバージョンを選択するにはオプションです。
- **Python**: Python のバージョンを選択または **OFF** Python を無効にします。

技術的な理由で、Web アプリで Java を有効にすると、.NET、PHP、Python オプションが無効になります。

<a name="platform"></a>
**プラットフォーム**します。 Web アプリが 32 ビット環境で実行されるか 64 ビット環境で実行されるかを選択します。 64 ビット環境では Basic モードまたは Standard モードを使用する必要があります。 Free モードと Shared モードは常に 32 ビット環境で実行されます。

**Web ソケット**します。 設定 **ON** WebSocket プロトコルを有効にする例では、web アプリで使用する場合の [ASP.NET SignalR](http://www.asp.net/signalr) または [socket.io](web-sites-nodejs-chat-app-socketio.md)します。

<a name="alwayson"></a>
**常にオン**します。 既定では、アイドル状態がしばらく続くと Web アプリはアンロードされます。 これにより、システムではリソースを節約できます。 基本または標準モードで有効にできます **Always On** 、アプリを保持するすべての時間をロードします。 有効にした場合、アプリでは、継続的な web ジョブが実行される、 **Always On**, 、または web ジョブが確実に実行されない可能性があります。

**管理されたパイプライン バージョン**です。 IIS の設定 [パイプライン モード](http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application)します。 この設定は、以前のバージョンの IIS を必要とするレガシ アプリを使用する場合を除いて、[統合] (既定) のままにしておきます。

**自動スワップ**します。 デプロイ スロットの自動スワップを有効にした場合、App Service は、スロットに対して更新をプッシュしたときに、Web アプリを運用環境に自動的にスワップします。 詳細については、[Azure App Service で web アプリのスロットを段階的展開] を参照してください (web-サイト-ステージング-publishing.md)。


### デバッグ

**リモート デバッグ**します。 リモート デバッグを有効にします。 これを有効にすると、Visual Studio でリモート デバッガーを使用して、Web アプリに直接接続できます。 リモート デバッグは 48 時間有効です。 


### アプリケーション設定

このセクションには、起動時に Web アプリがロードする名前/値ペアが含まれます。 

- .NET アプリの場合、実行時にこれらの設定が .NET 構成の `AppSettings` に挿入され、既存の設定がオーバーライドされます。 

- PHP、Python、Java および Node アプリケーションでは、実行時に環境変数としてこれらの設定にアクセスできます。 各アプリ設定で、2 つの環境変数が作成されます。1 つは、アプリ設定エントリで指定された名前になり、もう 1 つはプレフィックスとして APPSETTING_ が付加されます。 両方に同じ値が格納されます。

### 接続文字列

リンク済みリソースの接続文字列です。 

.NET アプリの場合、これらの接続文字列は、実行時に .NET 構成の `connectionStrings` 設定に挿入され、キーがリンクされたデータベース名である既存のエントリがオーバーライドされます。 

PHP、Python、Java、および Node アプリケーションの場合、実行時にこれらの設定が環境変数として使用できるようになり、環境変数にはプレフィックスとして接続の種類が付加されます。 環境変数使用されるプレフィックスは次のとおりです。 

- SQL Server: SQLCONNSTR_
- MySQL: MYSQLCONNSTR_
- SQL Database: SQLAZURECONNSTR_
- カスタム: CUSTOMCONNSTR_

たとえば、MySql の接続文字列が `connectionstring1` という名前であれば、`MYSQLCONNSTR_connectionString1` という環境変数でアクセスされます。

### 既定のドキュメント

既定のドキュメントは、Web サイトのルート URL に表示される Web ページです。  一覧で最初に一致するファイルが使用されます。 

Web アプリでは、静的コンテンツを提供する代わりに URL に基づいてルーティングするモジュールを使用することがあります。その場合には、このような既定のドキュメントはありません。    

### ハンドラー マッピング

この領域では、特定のファイル拡張子に関する要求を処理するカスタム スクリプト プロセッサを追加します。 

- **拡張機能**です。 処理するファイル拡張子 (*.php や handler.fcgi など) を指定します。 
- **スクリプト プロセッサ パス**します。 スクリプト プロセッサの絶対パスを指定します。 指定したファイル拡張子に一致するファイルに対する要求が、スクリプト プロセッサによって処理されます。 アプリのルート ディレクトリは `D:\home\site\wwwroot` というパスを使って参照します。
- **追加の引数**します。 スクリプト プロセッサのオプションのコマンド ライン引数です。 


### 仮想アプリケーションとディレクトリ 
 
仮想アプリケーションとディレクトリを構成するには、各仮想ディレクトリとそれに対応する物理パス (Web サイトのルートに対する相対パス) を指定します。 必要に応じて、選択、 **アプリケーション** 仮想ディレクトリをアプリケーションとしてのマークを付けるチェック ボックスをオンします。


## 診断ログの有効化

診断ログを有効にするには

1. Web アプリのブレードで、次のようにクリックします。 **設定をすべて**です。
2. クリックして **の診断ログを**します。 

ログ記録をサポートする Web アプリケーションの診断ログの書き込みに関するオプション: 

- **アプリケーションのログ記録**します。 ファイル システムに、アプリケーションのログを書き込みます。 ログ記録は、12 時間継続されます。 

**レベル**します。 アプリケーション ログが有効な場合、このオプションは記録する情報のレベルを指定します (エラー、警告、情報、または詳細)。

**Web サーバーのログ記録**します。 ログは W3C 拡張ログ ファイル形式で保存されます。 

**エラー メッセージの詳細な**です。 詳細なエラー メッセージの .htm ファイルを保存します。 ファイルは、/LogFiles/DetailedErrors に保存されます。 

**失敗した要求トレース**します。 XML ファイルへのログ要求が失敗しました。 ファイルに保存されて/LogFiles/W3SVC*xxx*, 、xxx は一意の識別子。 このフォルダーには、1 つの XSL ファイルと 1 つ以上の XML ファイルが格納されます。 この XSL ファイルは、XML ファイルのコンテンツの書式設定とフィルター処理を行う役割を果たすため、必ずダウンロードしてください。

ログ ファイルを表示するには、次のように FTP の資格情報を作成する必要があります。

1. Web アプリのブレードで、次のようにクリックします。 **設定をすべて**です。
2. クリックして **デプロイ資格情報**します。
3. ユーザー名とパスワードを入力します。
4. クリックして **保存**します。

![](./media/web-sites-configure/configure03.png)


完全な FTP ユーザー名は"\username"です。 *アプリ* web アプリの名前を指定します。 ユーザー名が表示されている web アプリのブレードで [ **Essentials**します。  

![](./media/web-sites-configure/configure02.png)

## その他の 構成タスク

### SSL 

基本モードまたは標準モードでは、カスタム ドメインの SSL 証明書をアップロードすることができます。 詳細については、次を参照してください。 [HTTPS web アプリの有効化](web-sites-configure-ssl-certificate.md)します。 

アップロードされた証明書を表示する] をクリックして **設定をすべて** > **カスタム ドメインと SSL**します。

### ドメイン名

Web アプリのカスタム ドメイン名を追加します。 詳細については、次を参照してください。 [Azure App Service で web アプリのカスタム ドメイン名を構成する](web-sites-custom-domain-name.md)です。

ドメイン名を表示する] をクリックして **設定をすべて** > **カスタム ドメインと SSL**します。

### デプロイメント

- 継続的なデプロイを設定します。 参照してください [Git Azure App Service で Web アプリのデプロイを使用しました。](web-sites-publish-source-control.md)
- デプロイメント スロット: 参照してください [Azure App Service で Web アプリのステージング環境へのデプロイ](web-sites-staged-publishing.md)

デプロイ スロットを表示する] をクリックして **設定をすべて** > **デプロイ スロット**します。


### Monitoring

基本または標準モードでは、最大 3 つの地理的に分散場所からの HTTP または HTTPS エンドポイントの可用性をテストできます。 HTTP 応答コードがエラー (4xx または 5xx) である場合、または、応答に 30 秒以上かかる場合、監視テストは失敗します。 すべての指定した場所から監視テストが成功した場合、エンドポイントは利用可能と見なされます。 

詳細については、次を参照してください。 [方法: web エンドポイントの状態を監視](http://go.microsoft.com/fwLink/?LinkID=279906&clcid=0x409)します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 次のステップ

- [カスタム ドメイン名の構成](web-sites-custom-domain-name.md)
- [HTTPS の有効化](web-sites-configure-ssl-certificate.md)
- [Azure App Service の Web アプリをスケーリングする](web-sites-scale.md)
- [Azure App Service の Web アプリを監視するための基本](web-sites-monitor.md)

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
 

