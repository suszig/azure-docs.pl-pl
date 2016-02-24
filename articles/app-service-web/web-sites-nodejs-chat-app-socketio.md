<properties
    pageTitle="Azure App Service での Socket.IO を使用する Node.js チャット アプリケーションの構築"
    description="Azure でホストされる node.js Web アプリで socket.io を使用する方法を示すチュートリアル。"
    services="app-service\web"
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="11/20/2015"
    ms.author="robmcm"/>




# Azure App Service での Socket.IO を使用する Node.js チャット アプリケーションの構築

Socket.IO は、WebSocket を使用して node.js サーバーとクライアントの間のリアルタイム通信を提供します。 また、古いブラウザーで動作する他のトランスポート (ロング ポーリングなど) へのフォールバックもサポートします。 このチュートリアルは、Azure の web アプリとして Socket.IO ベースのチャット アプリケーションをホストする手順を説明し、方法を説明する [スケール](#scale-out) アプリケーションを使用して、 [Azure Redis Cache](/documentation/services/cache)します。 Socket.IO の詳細については、[http://socket.io/][socketio] を参照してください。

> [AZURE.NOTE] このタスクの手順に適用 [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)。 クラウド サービスを参照してください <a href="http://www.windowsazure.com/develop/nodejs/tutorials/app-using-socketio/">Azure クラウド サービスで Socket.IO 使用する Node.js チャット アプリケーションをビルド</a>します。


## チャットのサンプルのダウンロード

このプロジェクトでは、チャットのサンプルを使用して、 [Socket.IO
GitHub リポジトリ]します。 次の手順を実行してサンプルをダウンロードし、
先ほど作成したプロジェクトに追加します。

1.  ダウンロード、 [ZIP または GZ アーカイブ リリース][release] Socket.IO プロジェクトの (このドキュメントのバージョン 1.3.5 を使用しました)


3.  アーカイブおよびコピーを抽出、 **examples \\chat**
    ディレクトリを新しい場所にコピーします。 たとえば、次のように入力します。
    **\\node\\chat**します。

## App.js の変更とモジュールのインストール

1.  名前の変更、 **index.js** ファイルを **app.js**します。 これにより、Azure はこれが Node.js アプリケーションであることを検出できます。

1.  開いている、 **app.js** ファイルをテキスト エディターでします。 `var io = require('../..')(server);` を含む行を以下に示すように変更します。

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;


3. 開いている、 **package.json** ファイルを下にある socket.io への参照を追加 `dependencies`, 、次のようにします。

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

4. コマンドラインでに変更、 **\\node\\chat** npm、このアプリケーションで必要なモジュールのインストールを使用してディレクトリ。

        npm install

    という名前のサブフォルダーに、モジュールがインストールされます **node_modules**します。

## Azure Web アプリの作成

Azure Web アプリを作成し、Git 発行を有効にして、Web アプリの WebSocket サポートを有効にするには、次の手順に従います。

> [AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「<a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure の無料試用版サイト</a>」を参照してください。

1. Azure  コマンド ライン インターフェイス (Azure CLI) をインストールし、Azure サブスクリプションに接続します。 参照してください [のインストールし、Azure CLI の構成](../xplat-cli)します。

2. Azure で初めてリポジトリを設定する場合は、ログイン資格情報を作成する必要があります。 Azure CLI で、次のコマンドを入力します。

        azure site deployment user set [username] [password]


3. 変更、 **\\node\chat** web アプリとローカル Git リポジトリのディレクトリと新しい Azure を作成する、次のコマンドを使用します。 このコマンドにより、「azure」という名前の Git リモートも作成されます。

        azure site create mysitename --git

    "mysitename" を一意の Web アプリ名に置き換える必要があります。

2. 次のコマンドを使用して、既存のファイルをローカル リポジトリにコミットします。

        git add .
        git commit -m "Initial commit"

3. 次のコマンドで、ファイルを Azure Web Apps リポジトリにプッシュします。

        git push azure master

    メッセージが表示されたら、手順 2. の資格情報を入力します。 モジュールがサーバーにインポートされると、ステータス メッセージを受信します。 この処理が完了すると、アプリケーションが Azure Web アプリでホストされるようになります。

    > [AZURE.NOTE] モジュールのインストール中にエラーが表示することがありますが ' インポートされたプロジェクト... が見つかりませんでした '。 このエラーは無視してかまいません。

4. Socket.IO では、WebSocket を使用します。WebSocket は、Azure では既定で有効になりません。 Web Socket を有効にするには、次のコマンドを使用します。

        azure site set -w

    Web アプリの名前の入力を求められた場合は、入力します。

    >[AZURE.NOTE]
    >"azure site set -w" コマンドは、Version 0.7.4 以降の Azure  コマンド ライン インターフェイスでのみ機能します。 使用して WebSocket サポートを有効することも、 [Azure ポータル](https://portal.azure.com)します。
    >
    >Azure ポータルを使用して Websocket を有効にするには、Web アプリ] ブレードから web アプリをクリックして] をクリックして **設定をすべて** > **アプリケーション設定**します。  **Web ソケット**, をクリックして **に**します。 クリックして **保存**します。

5. Azure の Web アプリを表示するには、次のコマンドを使用して Web ブラウザーを起動し、ホストされている Web アプリに移動します。

        azure site browse

これで、アプリケーションは Azure で実行されるようになり、Socket.IO を使用する複数のクライアント間でチャット メッセージを中継できます。

##スケールアウト

Socket.IO アプリケーションを使用して、スケール アウトできる、 __アダプター__ メッセージとイベントを複数のアプリケーション インスタンスを配布します。 複数のアダプターがある、利用可能なときに、 [socket.io-redis](https://github.com/automattic/socket.io-redis) アダプターは、Azure Redis Cache 機能を簡単に使用できます。

> [AZURE.NOTE] Socket.IO ソリューションのスケール アウトに対する追加の要件は、スティッキ セッションのサポートです。 Azure Web Apps では Azure Request Routing によるスティッキ セッションが既定で有効になっています。 詳細については、次を参照してください [Azure の Web サイトのインスタンスのアフィニティ。](http://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)

###Redis Cache の作成

手順を実行 [Azure Redis Cache でキャッシュを作成](/documentation/articles/cache-dotnet-how-to-use-azure-redis-cache/#create-a-cache) 新しいキャッシュを作成します。

> [AZURE.NOTE] 保存、 __ホスト名__ と __主キー__ 、キャッシュのこれらが必要に次の手順でします。

###Redis および socket.io-redis モジュールの追加

1. コマンドラインから、変更、 __\\node\\chat__ ディレクトリと、次のコマンドを使用します。

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] このコマンドで指定されたバージョンは、この記事をテストするときに使用したバージョンです。

2. 変更、 __app.js__ 次を追加するファイルの直後後の行 `var io = require('socket.io')(server);`

        var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
        var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});

        var redis = require('socket.io-redis');
        io.adapter(redis({pubClient: pub, subClient: sub}));

    置換 __redishostname__ と __rediskey__ ホスト名と、Redis キャッシュのキーを使用します。

    これにより、前の手順で作成した Redis Cache に対する発行とサブスクライブ クライアントが作成されます。 その後、クライアントは、Redis Cache を使用してアプリケーション インスタンス間のメッセージおよびイベントの受け渡しを行うように Socket.IO を構成するために、アダプターで使用されます。

    > [AZURE.NOTE] 中に、 __socket.io-redis__ アダプターが Redis に直接通信できますが、現在のバージョンは Azure Redis cache で必要な認証をサポートしていません。 使用して、最初の接続が作成されるため、 __redis__ モジュールで、クライアントに渡される、 __socket.io-redis__ アダプター。
    >
    > Azure Redis Cache はポート 6380 を使用するセキュリティで保護された接続をサポートしますが、この例で使用されているモジュールは、2014/7/14 の時点では、セキュリティで保護された接続をサポートしていません。 前記のコードは、セキュリティで保護されていない既定のポート 6379 を使用しています。

3. 保存、変更された __app.js__

###変更のコミットと再デプロイ

コマンドラインから、 __\\node\\chat__ ディレクトリに、次のコマンドを使用して変更をコミットし、アプリケーションを再配置します。

    git add .
    git commit -m "implementing scale out"
    git push azure master

変更をサーバーにプッシュした後は、次のコマンドを使用してサイトを複数のインスタンスに拡大できます。

    azure site scale instances --instances #

ここで __#__ 作成するインスタンスの数です。

複数のブラウザーまたはコンピューターから Web アプリに接続して、メッセージがすべてのクライアントに正しく送信されることを確認できます。

## トラブルシューティング

###接続制限

Azure Web Apps は複数の SKU で使用でき、SKU によってサイトで使用できるリソースが決まります。 これには、許可される WebSocket 接続の数が含まれます。 詳細については、次を参照してください。、 [Web Apps の料金のページ][pricing]します。

###WebSocket を使用してメッセージを送信できない

クライアントのブラウザーがロング ポーリングへのフォールバックを続け、WebSocket を使用しない場合、次のいずれかによって解決する可能性があります。

* **トランスポートを WebSocket だけに限定してみる**

    Socket.IO がメッセージング トランスポートとして WebSocket を使用するには、サーバーとクライアントの両方が WebSocket をサポートしている必要があります。 どちらかがサポートしていない場合、Socket.IO はロング ポーリングなどの別のトランスポートをネゴシエートします。 Socket.IO によって使用されるトランスポートの既定のリストは、` websocket, htmlfile, xhr-polling, jsonp-polling` です。 次のコードを追加して Websocket だけを使用するように強制できます、 **app.js** を含む行の後にファイルを `, nicknames = {};`です。

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] Websocket をサポートしない古いブラウザーは、通信を Websocket だけに制限するので、上記のコードは、アクティブなときに、サイトに接続できないに注意してください。

* **SSL を使用する**

    Websocket のいずれか小さいほうに使用される HTTP ヘッダーに依存など、 **アップグレード** ヘッダー。 Web プロキシなど、中間ネットワーク デバイスの中にはこれらのヘッダーを削除するものがあります。 この問題を避けるには、WebSocket 接続を SSL 経由で確立できます。

    これを実現する簡単な方法は、Socket.IO を `match origin protocol` に構成することです。 この指定は、Socket.IO に、Web ページに対する HTTP/HTTPS 要求を送信するときと同じように WebSocket 通信をセキュリティで保護するように指示します。 ブラウザーが HTTPS URL を使用して Web サイトにアクセスすると、それ以降の Socket.IO を使用した WebSocket 通信は SSL によってセキュリティで保護されます。

    この構成を有効にするには、この例を変更するに次のコードを追加、 **app.js** を含む行の後にファイルを `, nicknames = {};`します。

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **web.config の設定を確認する**

    Node.js アプリケーションの使用をホストする azure web アプリ、 **web.config** ファイル受信要求を Node.js アプリケーションにルーティングします。 Websocket Node.js アプリケーションで正しく機能するため、 **web.config** 次のエントリを含める必要があります。

        <webSocket enabled="false"/>

    これにより IIS WebSocket モジュールが無効になりますが、それには WebSocket の独自の実装が含まれ、それは Socket.IO などの Node.js 固有の WebSocket モジュールと競合します。 この行が存在しない場合、または `true` に設定されている場合は、WebSocket トランスポートがアプリケーションで動作しない原因になる可能性があります。

    通常、Node.js アプリケーションは含めないでください、 **web.config** ファイル、Azure の web サイトが自動的に生成 Node.js アプリケーション用の展開。 このファイルはサーバー上に自動的に生成されるので、Web サイトでこのファイルを参照するには FTP または FTPS URL を使用する必要があります。 見つかります FTP および FTPS Url で従来のポータル サイトの web アプリを選択し、 **ダッシュ ボード** リンクします。 Url を表示、 **概要** セクションです。

    > [AZURE.NOTE]  **Web.config** 、アプリケーションが 1 つを提供しない場合に、ファイルは、Azure Websites によって生成されるだけです。 指定した場合、 **web.config** ファイルに、アプリケーション プロジェクトのルートで、Azure Web アプリで使用されます。

    エントリが存在しないかの値に設定されているかどうかは `true`, 、し、作成する必要があります、 **web.config** 、Node.js アプリケーションのルートの値を指定して `false`します。  リファレンスについては、既定値を次に示します **web.config** を使用するアプリケーションの **app.js** エントリ ポイントとして。

        <?xml version="1.0" encoding="utf-8"?>
        <!--
             This configuration file is required if iisnode is used to run node processes behind
             IIS or IIS Express.  For more information, visit:

             https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config
        -->

        <configuration>
          <system.webServer>
            <!-- Visit http://blogs.msdn.com/b/windowsazure/archive/2013/11/14/introduction-to-websockets-on-windows-azure-web-sites.aspx for more information on WebSocket support -->
            <webSocket enabled="false" />
            <handlers>
              <!-- Indicates that the server.js file is a node.js web app to be handled by the iisnode module -->
              <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
            </handlers>
            <rewrite>
              <rules>
                <!-- Do not interfere with requests for node-inspector debugging -->
                <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                  <match url="^app.js\/debug[\/]?" />
                </rule>

                <!-- First we consider whether the incoming URL matches a physical file in the /public folder -->
                <rule name="StaticContent">
                  <action type="Rewrite" url="public{REQUEST_URI}"/>
                </rule>

                <!-- All other URLs are mapped to the node.js web app entry point -->
                <rule name="DynamicContent">
                  <conditions>
                    <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                  </conditions>
                  <action type="Rewrite" url="app.js"/>
                </rule>
              </rules>
            </rewrite>
            <!--
              You can control how Node is hosted within IIS using the following options:
                * watchedFiles: semi-colon separated list of files that will be watched for changes to restart the server
                * node_env: will be propagated to node as NODE_ENV environment variable
                * debuggingEnabled - controls whether the built-in debugger is enabled

              See https://github.com/tjanczuk/iisnode/blob/master/src/samples/configuration/web.config for a full list of options
            -->
            <!--<iisnode watchedFiles="web.config;*.js"/>-->
          </system.webServer>
        </configuration>

    アプリケーションが以外のエントリ ポイントを使用する場合 **app.js**, 、箇所をすべて置き換える必要があります **app.js** を正しいエントリ ポイントです。 たとえば、置換 **app.js** と **server.js**します。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

##次のステップ

このチュートリアルでは、Azure Web アプリでホストされるチャット アプリケーションを作成する方法を説明しました。 このアプリケーションは、Azure クラウド サービスとしてホストすることもできます。 これを実現する方法の手順については、次を参照してください。 [Azure クラウド サービスで Socket.IO 使用する Node.js チャット アプリケーションをビルド][cloudservice]します。

詳細については、「関連項目、 [Node.js デベロッパー センター](/develop/nodejs/)します。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [プレビュー ポータル内の移動に関するリファレンス。](http://go.microsoft.com/fwlink/?LinkId=529715)

[socketio]: http://socket.io/
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png
[Socket.IO GitHub repository]: https://github.com/Automattic/socket.io
[release]: https://github.com/Automattic/socket.io/releases
[cloudservice]: /develop/nodejs/tutorials/app-using-socketio/

[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png
[pricing]: /pricing/details/web-sites/
 

