<properties
    pageTitle="Node.js で Azure Redis Cache を使用する方法 | Microsoft Azure"
    description="Node.js と node_redis を使用して Azure Redis Cache を使用します。"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="dwrede"
    editor="v-lincan"/>

<tags
    ms.service="cache"
    ms.devlang="nodejs"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="12/03/2015"
    ms.author="sdanie"/>


# Node.js で Azure Redis Cache を使用する方法

> [AZURE.SELECTOR]
- [.Net](cache-dotnet-how-to-use-azure-redis-cache.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)


Azure Redis Cache を使用すると、Microsoft が管理している、セキュリティで保護された専用 Redis Cache にアクセスできます。 キャッシュは、Microsoft Azure 内の任意のアプリケーションからアクセスできます。

このトピックでは、Node.js を使用して Azure Redis Cache を使用する方法を説明しています。 Node.js で Azure Redis Cache を使用する別の例を参照してください。 [Socket.IO 使用する Node.js チャット アプリケーションを Azure の web サイトの [] に基づく][]します。


## 前提条件

インストール [node_redis](https://github.com/mranney/node_redis):

    npm install redis

このチュートリアルでは [node_redis](https://github.com/mranney/node_redis), に記載されている任意の Node.js クライアントを使用することができますが、 [http://redis.io/clients](http://redis.io/clients)します。

## Azure で Redis Cache を作成する

[Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=398536), 、クリックして **新規**, 、**データ + ストレージ**, を選択し、 **Redis Cache**します。

  ![][1]

DNS ホスト名を入力します。フォームが '<name>
  。.redis.cache.windows.net という ' です。 **[作成]** をクリックします。

  ![][2]


  作成すると、キャッシュ [参照する](cache-configure.md#configure-redis-cache-settings) キャッシュ設定を表示します。 **[キー]** の下のリンクをクリックして、プライマリ キーをコピーします。 このキーは、要求を認証するために必要です。

  ![][4]


  ## 非 SSL エンドポイントを有効にする


  **[ポート]** の下のリンクをクリックし、[SSL 経由でのみアクセスを許可する] に対して **[いいえ]** をクリックします。 これにより、キャッシュに対して非 SSL ポートが有効になります。 現在、node_redis クライアントは SSL をサポートしていません。

  ![][3]


  ## キャッシュに何かを追加して取得する

  var redis = require("redis");

  キャッシュ名とアクセス キーを追加します。
  var client = redis.createClient (6379、'<name>..redis.cache.windows.net という '、{auth_pass: '<key>'})。

    client.set("foo", "bar", function(err, reply) {
        console.log(reply);
    });
    
    client.get("foo",  function(err, reply) {
        console.log(reply);
    });

出力:

    OK
    bar

## 次のステップ

- [キャッシュ診断を有効にする](cache-how-to-monitor.md#enable-cache-diagnostics) できるように [モニター](cache-how-to-monitor.md) 、キャッシュの正常性。
- 公式の読み取り [Redis ドキュメント](http://redis.io/documentation)します。




[1]: ./media/cache-nodejs-get-started/cache01.png 
[2]: ./media/cache-nodejs-get-started/cache02.png 
[3]: ./media/cache-nodejs-get-started/cache03.png 
[4]: ./media/cache-nodejs-get-started/cache04.png 
[build a node.js chat application with socket.io on an azure website]: ../app-service-web/web-sites-nodejs-chat-app-socketio.md 

