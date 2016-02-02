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

Socket.IO は、WebSocket を使用して node.js サーバーとクライアントの間のリアルタイム通信を提供します。 また、古いブラウザーで動作する他のトランスポート (ロング ポーリングなど) へのフォールバックもサポートします。 このチュートリアルは、Azure の web アプリとして Socket.IO ベースのチャット アプリケーションをホストする手順を説明し、方法を説明する [スケール](#scale-out) アプリケーションを使用して、 [Azure Redis Cache](/documentation/services/cache)します。 Socket.IO の詳細については、次を参照してください。 [http://socket.io/][socketio]します。
> [AZURE.NOTE] このタスクの手順に適用 [App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714)。 クラウド サービスを参照してください <a href="http://www.windowsazure.com/develop/nodejs/tutorials/app-using-socketio/">Azure クラウド サービスで Socket.IO 使用する Node.js チャット アプリケーションをビルド</a>します。


## チャットのサンプルのダウンロード

このプロジェクトでは、チャットのサンプル [Socket.IO を使用してください。
GitHub のリポジトリ] です。 次の手順を実行してサンプルをダウンロードし、
先ほど作成したプロジェクトに追加します。

1.  ダウンロード、 [ZIP または GZ アーカイブ リリース ][release] Socket.IO プロジェクトの (このドキュメントのバージョン 1.3.5 を使用しました)

3.  アーカイブおよびコピーを抽出、 **examples \\chat**
    ディレクトリを新しい場所にコピーします。 たとえば、次のように入力します。
    **\\node\\chat**します。

## App.js の変更とモジュールのインストール

1.  **index.js** ファイルの名前を **app.js** に変更します。 これにより、Azure はこれが Node.js アプリケーションであることを検出できます。

1.  テキスト エディターで **app.js** ファイルを開きます。 を含む行を変更する `var io = require('../..')(サーバー);` 下図のようにします。

        var express = require('express');
        var app = express();
        var server = require('http').createServer(app);
        // var io = require('../..')(server);
        // New:
        var io = require('socket.io')(server);
        var port = process.env.PORT || 3000;

3. 開いている、 **package.json** ファイルを下にある socket.io への参照を追加 `の依存関係`, 次のように。

        "dependencies": {
          "express": "3.4.8",
          "socket.io": "1.3.5"
        }

4. コマンドラインでに変更、 **\\node\\chat** npm、このアプリケーションで必要なモジュールのインストールを使用してディレクトリ。

        npm install

    これにより、モジュールが **node_modules** という名前のサブフォルダーにインストールされます。

## Azure Web アプリの作成

Azure Web アプリを作成し、Git 発行を有効にして、Web アプリの WebSocket サポートを有効にするには、次の手順に従います。
> [AZURE.NOTE] このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版アカウントを数分で作成することができます。 詳細については、「 <a href="http://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A7171371E" target="_blank">Azure 無料試用版</a>します。

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
    > [AZURE.NOTE] モジュールのインストール中に、"インポートされたプロジェクト ... が見つかりませんでした" というエラーが表示される場合があります。 このエラーは無視してかまいません。

4. Socket.IO では、WebSocket を使用します。WebSocket は、Azure では既定で有効になりません。 Web Socket を有効にするには、次のコマンドを使用します。

        azure site set -w

    Web アプリの名前の入力を求められた場合は、入力します。
    >[AZURE.NOTE]
    >"azure site set -w" コマンドは、Version 0.7.4 以降の Azure  コマンド ライン インターフェイスでのみ機能します。 使用して WebSocket サポートを有効することも、 [Azure ポータル](https://portal.azure.com)します。
    >
    >Azure ポータルを使用して Websocket を有効にするには、[Web Apps] ブレードから Web アプリをクリックし、**[すべての設定]** > **[アプリケーションの設定]** をクリックします。 **[Web Sockets]** で、**[オン]** をクリックします。 その後、**[保存]** をクリックします。

5. Azure の Web アプリを表示するには、次のコマンドを使用して Web ブラウザーを起動し、ホストされている Web アプリに移動します。

        azure site browse


これで、アプリケーションは Azure で実行されるようになり、Socket.IO を使用する複数のクライアント間でチャット メッセージを中継できます。

## スケールアウト

__アダプター__を使用してメッセージとイベントを複数のアプリケーション インスタンスに分配することにより、Socket.IO アプリケーションをスケールアウトできます。 複数のアダプターがある、利用可能なときに、 [socket.io-redis](https://github.com/automattic/socket.io-redis) アダプターは、Azure Redis Cache 機能を簡単に使用できます。
> [AZURE.NOTE] Socket.IO ソリューションのスケールアウトに対する追加要件は、スティッキ セッションのサポートです。 Azure Web Apps では Azure Request Routing によるスティッキ セッションが既定で有効になっています。 詳細については、を参照してください [Azure の Web サイトのインスタンスのアフィニティ](http://azure.microsoft.com/blog/2013/11/18/disabling-arrs-instance-affinity-in-windows-azure-web-sites/)。

### Redis Cache の作成

手順を実行 [Azure Redis Cache でキャッシュを作成](/documentation/articles/cache-dotnet-how-to-use-azure-redis-cache/#create-a-cache) 新しいキャッシュを作成します。
> [AZURE.NOTE] キャッシュの__ホスト名__と__プライマリ キー__を保存します。これらは次の手順で必要になります。

### Redis および socket.io-redis モジュールの追加

1. コマンドラインから、変更、 __\\node\\chat__ ディレクトリと、次のコマンドを使用します。

        npm install socket.io-redis@0.1.4 redis@0.12.1 --save

    > [AZURE.NOTE] このコマンドで指定されているバージョンは、この記事をテストするときに使用したバージョンです。

2. 変更、 __app.js__ ファイルに、次を追加する行の直後に `var io = require('socket.io')(server);';`

     var pub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
     var sub = require('redis').createClient(6379,'redishostname', {auth_pass: 'rediskey', return_buffers: true});
    
     var redis = require('socket.io-redis');
     io.adapter(redis({pubClient: pub, subClient: sub}));

 __redishostname__ および __rediskey__ は、実際の Redis Cache のホスト名とキーに置き換えます。

 これにより、前の手順で作成した Redis Cache に対する発行とサブスクライブ クライアントが作成されます。 その後、クライアントは、Redis Cache を使用してアプリケーション インスタンス間のメッセージおよびイベントの受け渡しを行うように Socket.IO を構成するために、アダプターで使用されます。
 > [AZURE.NOTE] __socket.io-redis__ アダプターは Redis と直接通信できますが、現在のバージョンは Azure Redis Cache で必要な認証をサポートしていません。 したがって、__redis__ モジュールを使用して最初の接続が作成された後、クライアントが __socket.io-redis__ アダプターに渡されます。
 >
 > Azure Redis Cache はポート 6380 を使用するセキュリティで保護された接続をサポートしますが、この例で使用されているモジュールは、2014/7/14 の時点では、セキュリティで保護された接続をサポートしていません。 前記のコードは、セキュリティで保護されていない既定のポート 6379 を使用しています。

3. 変更した __app.js__ を保存します。

### 変更のコミットと再デプロイ

コマンドラインから、 __\\node\\chat__ ディレクトリに、次のコマンドを使用して変更をコミットし、アプリケーションを再配置します。

    git add .
    git commit -m "implementing scale out"
    git push azure master

変更をサーバーにプッシュした後は、次のコマンドを使用してサイトを複数のインスタンスに拡大できます。

    azure site scale instances --instances #

__#__ は、作成するインスタンスの数です。

複数のブラウザーまたはコンピューターから Web アプリに接続して、メッセージがすべてのクライアントに正しく送信されることを確認できます。

## トラブルシューティング

### 接続制限

Azure Web Apps は複数の SKU で使用でき、SKU によってサイトで使用できるリソースが決まります。 これには、許可される WebSocket 接続の数が含まれます。 詳細については、次を参照してください。、 [Web Apps の料金のページの ][pricing]します。

### WebSocket を使用してメッセージを送信できない

クライアントのブラウザーがロング ポーリングへのフォールバックを続け、WebSocket を使用しない場合、次のいずれかによって解決する可能性があります。

* **トランスポートを WebSocket だけに限定してみる**

    Socket.IO がメッセージング トランスポートとして WebSocket を使用するには、サーバーとクライアントの両方が WebSocket をサポートしている必要があります。 どちらかがサポートしていない場合、Socket.IO はロング ポーリングなどの別のトランスポートをネゴシエートします。 Socket.IO によって使用されるトランスポートの既定のリストは ` websocket htmlfile、xhr ポーリング、jsonp ポーリング`します。 次のコードを追加して Websocket だけを使用するように強制できます、 **app.js** を含む行の後にファイルを `, 、ニックネーム = {};`します。

        io.configure(function() {
          io.set('transports', ['websocket']);
        });

    > [AZURE.NOTE] 上記のコードは通信を WebSocket だけに制限するので、WebSocket をサポートしない古いブラウザーは、上記のコードが有効になっている間はサイトに接続できないことに注意してください。

* **SSL を使用する**

    WebSocket は **Upgrade** ヘッダーなどの使用頻度が低い一部の HTTP ヘッダーに依存します。 Web プロキシなど、中間ネットワーク デバイスの中にはこれらのヘッダーを削除するものがあります。 この問題を避けるには、WebSocket 接続を SSL 経由で確立できます。

    これを実現する簡単に Socket.IO を構成するのには `オリジン プロトコルが一致`します。 この指定は、Socket.IO に、Web ページに対する HTTP/HTTPS 要求を送信するときと同じように WebSocket 通信をセキュリティで保護するように指示します。 ブラウザーが HTTPS URL を使用して Web サイトにアクセスすると、それ以降の Socket.IO を使用した WebSocket 通信は SSL によってセキュリティで保護されます。

    この構成を有効にするには、この例を変更するには、次のコードを追加、 **app.js** を含む行の後にファイルを `, 、nicknames = {};`します。

        io.configure(function() {
          io.set('match origin protocol', true);
        });

* **web.config の設定を確認する**

  Node.js アプリケーションをホストする Azure Web アプリは、**web.config** ファイルを使用して、受け取った要求を Node.js アプリケーションにルーティングします。 Node.js アプリケーションで WebSocket が正常に機能するためには、**web.config** に次のエントリが含まれる必要があります。

      <webSocket enabled="false"/>

  これにより IIS WebSocket モジュールが無効になりますが、それには WebSocket の独自の実装が含まれ、それは Socket.IO などの Node.js 固有の WebSocket モジュールと競合します。 この行が存在しないかに設定されているかどうかは `true`, 、WebSocket トランスポートがアプリケーションに機能していない理由があります。

  通常、Node.js アプリケーションには **web.config** ファイルが含まれないので、Azure Websites はデプロイされるときに Node.js アプリケーション用にこのファイルを自動的に生成します。 このファイルはサーバー上に自動的に生成されるので、Web サイトでこのファイルを参照するには FTP または FTPS URL を使用する必要があります。 クラシック ポータルでサイトの FTP および FTPS URL を検索するには、Web サイトを選択して、**[ダッシュボード]** リンクをクリックします。 URL は **[概要]** セクションに表示されます。
  > [AZURE.NOTE] **web.config** ファイルが Azure Websites によって生成されるのは、アプリケーションで提供されていない場合だけです。 アプリケーション プロジェクトのルートで **web.config** ファイルを提供した場合は、それが Azure Web Apps によって使用されます。

  エントリが存在しないかの値に設定されているかどうかは `true`, 、し、作成する必要があります、 **web.config** 、Node.js アプリケーションのルートの値を指定して `false`します。 参考として、エントリ ポイントとして **app.js** を使用するアプリケーションの既定の **web.config** を次に示します。

      <?xml version="1.0" encoding="utf-8"?>
      
    
      <configuration>
        <system.webServer>
          
          <webSocket enabled="false" />
          <handlers>
            
            <add name="iisnode" path="app.js" verb="*" modules="iisnode"/>
          </handlers>
          <rewrite>
            <rules>
              
              <rule name="NodeInspector" patternSyntax="ECMAScript" stopProcessing="true">
                <match url="^app.js\/debug[\/]?" />
              </rule>
    
              
              <rule name="StaticContent">
                <action type="Rewrite" url="public{REQUEST_URI}"/>
              </rule>
    
              
              <rule name="DynamicContent">
                <conditions>
                  <add input="{REQUEST_FILENAME}" matchType="IsFile" negate="True"/>
                </conditions>
                <action type="Rewrite" url="app.js"/>
              </rule>
            </rules>
          </rewrite>
          
          
        </system.webServer>
      </configuration>

  アプリケーションで **app.js** 以外のエントリ ポイントを使用している場合は、すべての **app.js** を正しいエントリ ポイントに置き換える必要があります。 たとえば、**app.js** を **server.js** に置き換えます。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 次のステップ

このチュートリアルでは、Azure Web アプリでホストされるチャット アプリケーションを作成する方法を説明しました。 このアプリケーションは、Azure クラウド サービスとしてホストすることもできます。 これを実現する方法の手順については、次を参照してください。 [Socket.IO 使用する Node.js チャット アプリケーションを Azure クラウド サービス ][cloudservice]します。

詳細については、「関連項目、 [Node.js デベロッパー センター](/develop/nodejs/)します。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [プレビュー ポータル内の移動に関するリファレンス](http://go.microsoft.com/fwlink/?LinkId=529715)。


[socketio]: http://socket.io/ 
[completed-app]: ./media/web-sites-nodejs-chat-app-socketio/websitesocketcomplete.png 
[socket.io github repository]: https://github.com/Automattic/socket.io 
[release]: https://github.com/Automattic/socket.io/releases 
[cloudservice]: /develop/nodejs/tutorials/app-using-socketio/ 
[chat-example-view]: ./media/web-sites-nodejs-chat-app-socketio/socketio-2.png 
[npm-output]: ./media/web-sites-nodejs-chat-app-socketio/socketio-7.png 
[pricing]: /pricing/details/web-sites/ 

