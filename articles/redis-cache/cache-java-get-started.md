<properties
   pageTitle="Java で Azure Redis Cache を使用する方法 | Microsoft Azure"
    description="Java を使用して Azure Redis Cache を使用します"
    services="redis-cache"
    documentationCenter=""
    authors="steved0x"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="cache"
    ms.devlang="java"
    ms.topic="hero-article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="12/03/2015"
    ms.author="sdanie"/>

# Java で Azure Redis Cache を使用する方法

> [AZURE.SELECTOR]
- [.Net](cache-dotnet-how-to-use-azure-redis-cache.md)
- [Node.js](cache-nodejs-get-started.md)
- [Java](cache-java-get-started.md)
- [Python](cache-python-get-started.md)

Azure Redis Cache を使用すると、Microsoft が管理している専用の Redis Cache にアクセスできます。 キャッシュは、Microsoft Azure 内の任意のアプリケーションからアクセスできます。

このトピックでは、Java を使用して Azure Redis Cache を使用する方法を説明します。


## 前提条件

[Jedis](https://github.com/xetorthio/jedis) -Redis 用 Java クライアント

このチュートリアルでは Jedis に記載されている任意の Java クライアントを使用することが [http://redis.io/clients](http://redis.io/clients)します。


## Azure で Redis Cache を作成する

 [Azure ポータル](http://go.microsoft.com/fwlink/?LinkId=398536), 、クリックして **新規**, 、**データ + ストレージ**, を選択し、 **Redis Cache**します。

  ![][1]

DNS ホスト名を入力します。 フォーム `<name>.redis.cache.windows.net` が表示されます。 クリックして **作成**します。

  ![][2]


キャッシュを作成したら、Azure ポータルでそのキャッシュをクリックして設定を表示します。 下にあるリンクをクリックして **キー** プライマリ キーをコピーします。 このキーは、要求を認証するために必要です。

  ![][4]


## 非 SSL エンドポイントを有効にする


下にあるリンクをクリックして **ポート**, 、] をクリック **いいえ** 「SSL 経由でのみアクセスを許可する」にします。 これにより、キャッシュに対して非 SSL ポートが有効になります。 現在、Jedis クライアントは SSL をサポートしていません。

  ![][3]


## キャッシュに何か追加し、取得する

    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    /* Make sure you turn on non-SSL port in Azure Redis using the Configuration section in the Azure Portal */
    public class App
    {
      public static void main( String[] args )
      {
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6379);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## 次のステップ

- [キャッシュ診断を有効にする](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) できるように [モニター](https://msdn.microsoft.com/library/azure/dn763945.aspx) 、キャッシュの正常性。
- 公式の読み取り [Redis ドキュメント](http://redis.io/documentation)します。


<!--Image references-->
[1]: ./media/cache-java-get-started/cache01.png
[2]: ./media/cache-java-get-started/cache02.png
[3]: ./media/cache-java-get-started/cache03.png
[4]: ./media/cache-java-get-started/cache04.png

