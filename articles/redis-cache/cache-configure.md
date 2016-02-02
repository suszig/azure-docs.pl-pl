<properties 
    pageTitle="Azure Redis Cache の構成方法"
    description="Azure Redis Cache の既定の Redis 構成を理解し、Azure Redis Cache インスタンスの構成方法について説明します。"
    services="redis-cache"
    documentationCenter="na"
    authors="steved0x"
    manager="dwrede"
    editor="tysonn" />
<tags 
    ms.service="cache"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="cache-redis"
    ms.workload="tbd"
    ms.date="12/11/2015"
    ms.author="sdanie" />


# Azure Redis Cache の構成方法

このトピックでは、Azure Redis Cache インスタンスの構成を確認し、更新する方法と、Azure Redis Cache インスタンス用の既定の Redis サーバー構成について説明します。
>[AZURE.NOTE] 構成およびプレミアムのキャッシュ機能の使用の詳細については、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md), 、[Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md), 、および [Premium Azure Redis Cache 用の仮想ネットワークのサポートを構成する方法](cache-how-to-premium-vnet.md)します。

## Redis Cache の設定の構成

キャッシュにアクセスできる、 [Azure ポータル](https://portal.azure.com) を使用して、 **参照** ブレードです。

![Azure Redis Cache の参照ブレード](./media/cache-configure/IC796920.png)

**[Redis Caches]** をクリックしてキャッシュを表示します。

![Azure Redis Cache の参照キャッシュ リスト](./media/cache-configure/IC796921.png)

キャッシュのプロパティを表示するには、目的のキャッシュを選択します。

![Redis Cache のすべての設定](./media/cache-configure/IC808312.png)

**[設定]** または **[すべての設定]** をクリックしてキャッシュを表示し、構成します。

![Redis Cache の設定](./media/cache-configure/IC808313.png)

## プロパティ

**[プロパティ]** をクリックすると、キャッシュ エンドポイントやポートなど、キャッシュに関する情報を表示できます。

![Redis Cache のプロパティ](./media/cache-configure/IC808314.png)

## アクセス キー

**[アクセス キー]** をクリックすると、キャッシュのアクセス キーを表示したり、再生成したりできます。 これらのキーは、キャッシュに接続するクライアントによって、**[プロパティ]** ブレードのホスト名とポートと共に使用されます。

![Redis Cache のアクセス キー](./media/cache-configure/IC808315.png)

## アクセス ポート

新しいキャッシュでは、SSL を使用しないアクセスが既定で無効になっています。 非 SSL ポートを有効にするには、**[アクセス ポート]** ブレードをクリックし、**[いいえ]** をクリックします。

![Redis Cache のアクセス ポート](./media/cache-configure/IC808316.png)

## [価格レベル]

キャッシュの価格レベルを表示または変更するには、**[価格レベル]** をクリックします。 スケーリングの詳細については、次を参照してください。 [スケール Azure Redis Cache 方法](cache-how-to-scale.md)します。

![Redis Cache 価格レベル](./media/cache-configure/pricing-tier.png)

## 診断

**[診断]** をクリックすると、キャッシュ診断の格納に使用するストレージ アカウントを構成できます。

![Redis Cache の診断](./media/cache-configure/IC808317.png)

詳細については、次を参照してください。 [Azure Redis Cache の監視方法](cache-how-to-monitor.md)します。

## maxmemory-policy と maxmemory-reserved

**[Maxmemory ポリシー]** をクリックしてキャッシュのメモリ ポリシーを構成します。 **maxmemory-policy** 設定は、キャッシュの削除ポリシーを構成し、**maxmemory-reserved** は、キャッシュ以外のプロセスのために予約されたメモリを構成します。

![Redis Cache の Maxmemory ポリシー](./media/cache-configure/IC808318.png)

**[Maxmemory ポリシー]** では、次の削除ポリシーから選択できます。

-   volatile-lru - これが、既定値です。
-   allkeys-lru
-   volatile-random
-   allkeys-random
-   volatile-ttl
-   noeviction

Maxmemory ポリシーの詳細については、次を参照してください。 [削除ポリシー](http://redis.io/topics/lru-cache#eviction-policies)します。

**maxmemory-reserved** 設定は、フェールオーバーに伴うレプリケーションなどのキャッシュ以外の操作のために予約されているメモリの量を、MB 単位で構成するものです。 また、断片化率が高い場合も使用できます。 この値を設定すると、負荷が変化するときでも、Redis サーバーの稼働状態がより安定します。 この値は、書き込みが大量に発生するワークロードに対しては、高く設定する必要があります。 メモリがこのような操作のために予約されていると、キャッシュされたデータの保存に使用できなくなります。
>[AZURE.IMPORTANT] **maxmemory-reserved** 設定は、Standard キャッシュと Premium キャッシュにのみ使用可能です。

## キースペース通知 (詳細設定)

**[詳細設定]** をクリックして Redis キースペース通知を構成します。 キースペース通知により、特定のイベントが発生したときに、クライアントが通知を受信できます。

![Redis Cache の高度な設定](./media/cache-configure/IC808319.png)
>[AZURE.IMPORTANT] キースペース通知と **notify-keyspace-events** 設定は、Standard キャッシュと Premium キャッシュに対してのみ使用可能です。

詳細については、次を参照してください。 [Redis キースペース通知](http://redis.io/topics/notifications)します。 サンプル コードについては、 [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) ファイルで、 [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) サンプルです。

## Redis データの保持

**[Redis データ永続化]** をクリックして、Premium Cache のデータ永続化を有効にする、無効にする、または構成することができます。

![Redis データの保持](./media/cache-configure/redis-cache-persistence-settings.png)

Redis の永続化を有効にするには、**[有効]** をクリックして RDB (Redis データベース) のバックアップを有効にします。 Redis の永続化を無効にするには、**[無効]** をクリックします。

バックアップの間隔を構成するには、ドロップダウン リストから **[バックアップの頻度]** を選択します。 選択肢は、**15 分**、**30 分**、**60 分**、**6 時間**、**12 時間**、**24 時間**です。 前のバックアップ操作が正常に完了するとこの間隔のカウントダウンが開始し、期間が経過すると新しいバックアップが開始されます。

**[ストレージ アカウント]** をクリックして使用するストレージ アカウントを選択し、**[ストレージ キー]** ドロップダウンから使用する**プライマリ キー**または**セカンダリ キー**を選択します。 Cache と同じリージョンのストレージ アカウントを選択する必要があり、また、スループットが高いため **Premium Storage** アカウントを使用することをお勧めします。 永続化アカウントのストレージ キーを再生成する場合は常に、**[ストレージ キー]** ドロップダウンから目的のキーを再選択する必要があります。

**[OK]** をクリックして永続化の構成を保存します。
>[AZURE.IMPORTANT] Redis のデータ永続化は、Premium Cache でのみ使用できます。 詳細については、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md)します。

## Redis クラスター サイズ

クラスタリングが有効になっている実行中の Premium キャッシュに対してシャードを追加または削除するには、**[(プレビュー) Redis クラスター サイズ]** をクリックします。
>[AZURE.NOTE] Azure Redis Cache の Premium レベルは一般公開されていますが、Redis クラスター サイズ機能は現在プレビュー段階であることに注意してください。

![Redis クラスター サイズ](./media/cache-configure/redis-cache-redis-cluster-size.png)

シャード数を変更するには、スライダーを使用するか、**[シャード数]** ボックスに 1 ～ 10 の範囲の数値を入力し、**[OK]** をクリックして保存します。
>[AZURE.IMPORTANT] Redis クラスタリングは、Premium キャッシュでのみ使用できます。 詳細については、次を参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。


## ユーザーとタグ

![Redis Cache のユーザーとタグ](./media/cache-configure/IC808320.png)

**[ユーザー]** セクションでは、Azure ポータルでのロールベースのアクセス制御 (RBAC) をサポートしているため、組織はアクセス管理の要件を簡単かつ正確に満たすことができます。 詳細については、次を参照してください。 [Azure ポータルでのロールベースのアクセス制御](http://go.microsoft.com/fwlink/?LinkId=512803)します。

**[タグ]** セクションでは、リソースを整理できます。 詳細については、次を参照してください。 [タグを使用した Azure リソースの整理に](../resource-group-using-tags.md)します。

## 既定の Redis サーバー構成

新しい Azure Redis Cache インスタンスに適用される既定の Redis 構成値は、次のとおりです。
>[AZURE.NOTE] 使用してこのセクションの設定を変更できない、 `StackExchange.Redis.IServer.ConfigSet` メソッドです。 このセクションのいずれかのコマンドでこのメソッドを呼び出すと、次のような例外がスローされます。  
>
>`StackExchange.Redis.RedisServerException: 不明なコマンド 'CONFIG' のエラーします。`
>  
>**max-memory-policy** などの構成可能な値を、Azure ポータルを使用して構成できます。

| 設定| 既定値| 説明|
|---|---|---|
| databases| 16| 既定のデータベースは DB 0、接続を使用して、接続ごとに別の名前を選択できます。GetDataBase(dbid) dbid は 0 ~ 15 の数値。|
| maxclients| 価格レベルによって異なります<sup>1</sup>| これは、同時に接続が許可されているクライアントの最大数です。制限に達すると、Redis はすべての新しい接続を終了し、エラー 'max number of clients reached' を送信します。|
| maxmemory-policy| volatile-lru| Maxmemory ポリシーは、Redis が選択内容 maxmemory (キャッシュを作成した時に選択したキャッシュのサイズ) に達したときに削除する方法の設定です。Azure Redis Cache の既定の設定は volatile-lru で、LRU アルゴリズムを使用して有効期限が設定されたキーを削除します。この設定は、Azure ポータルで構成できます。詳細については、次を参照してください。 [maxmemory-policy と maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved)します。|
| maxmemory-samples| 3| LRU アルゴリズムと最小 TTL アルゴリズムは精緻なアルゴリズムではなく、(メモリを節約するための) 近似アルゴリズムです。そのため、サンプル サイズも選択して確認できます。既定の Redis インスタンスの場合、キーを 3 つ確認し、直近の使用頻度が比較的低い ものを 1 つ選択します。|
| lua-time-limit| 5,000| Lua スクリプトの最大実行時間 (ミリ秒)。最大実行時間に達した場合は、Redis は、最大許容時間の後もスクリプトが実行中であることをログに記録し、クエリに対してエラーを知らせる応答を開始します。|
| lua-event-limit| 500| これは、スクリプト イベント キューの最大サイズです。|
| client-output-buffer-limit normalclient-output-buffer-limit pubsub| 0 0 032mb 8mb 60| このクライアントの出力バッファー制限を使用して、なんらかの理由 (一般的には、パブリッシャーがメッセージを作成するのと同じ速度で Pub/Sub クライアントがメッセージを利用できないという理由) により、サーバーから十分な速度でデータを読み込んでいないクライアントを強制的に切断することができます。詳細については、次を参照してください。 [http://redis.io/topics/clients](http://redis.io/topics/clients)します。|

<sup>1</sup>`maxclients` は、Azure Redis Cache 価格レベルごとに異なります。

-   Basic キャッシュおよび Standard キャッシュ
    -   C0 (250 MB) キャッシュ - 最大 256 接続
    -   C1 (1 GB) キャッシュ - 最大 1,000 接続
    -   C2 (2.5 GB) キャッシュ - 最大 2,000 接続
    -   C3 (6 GB) キャッシュ - 最大 5,000 接続
    -   C4 (13 GB) キャッシュ - 最大 10,000 接続
    -   C5 (26 GB) キャッシュ - 最大 15,000 接続
    -   C6 (53 GB) キャッシュ - 最大 20,000 接続
-   Premium キャッシュ
    -   P1 (6 ～ 60 GB) - 最大 7,500 接続
    -   P2 (13 ～ 130 GB) - 最大 15,000 接続
    -   P3 (26 ～ 260 GB) - 最大 30,000 接続
    -   P4 (53 ～ 530 GB) - 最大 40,000 接続

## Azure Redis Cache でサポートされない Redis コマンド

>[AZURE.IMPORTANT] Azure Redis Cache インスタンスの構成と管理には Azure ポータルを使用する関係上、次のコマンドは無効です。 それらを呼び出そうとする場合のようなエラー メッセージが表示されるは `"(エラー) エラーが不明な command"`します。
>
>-BGREWRITEAOF
>-BGSAVE
>構成
>デバッグ
>-移行します。
>保存
>シャット ダウン
>-SLAVEOF

Redis コマンドの詳細については、次を参照してください。 [http://redis.io/commands](http://redis.io/commands)します。

## Redis コンソール

**Redis コンソール**を使用して Azure Redis Cache インスタンスにコマンドを安全に発行できます。このコンソールは Standard キャッシュと Premium キャッシュに対して使用できます。
>[AZURE.IMPORTANT] Redis コンソールは、VNET またはクラスタリングとは動作しません。 
>
>-  [VNET](cache-how-to-premium-vnet.md) 、VNET 内のクライアントがキャッシュにアクセスできる専用キャッシュは、VNET の一部とするときにします。 Redis コンソールは、VNET の一部ではない VM でホストされている redis-cli.exe クライアントを使用するため、キャッシュに接続できません。
>-  [クラスタ リング](cache-how-to-premium-clustering.md) -「Redis コンソールは、この時点でクラスタ リングをサポートしていません cli.exe クライアントを使用します。 Redis cli ユーティリティ、 [不安定](http://redis.io/download) GitHub で Redis リポジトリの分岐で起動したときの基本的なサポートを実装して、 `-c` スイッチです。 詳細については、次を参照してください。 [クラスターと再生](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) に [http://redis.io](http://redis.io) で、 [Redis クラスター チュートリアル](http://redis.io/topics/cluster-tutorial)します。

Redis コンソールにアクセスするには、**[Redis Cache]** ブレードの **[コンソール]** をクリックします。

![Redis コンソール](./media/cache-configure/redis-console-menu.png)

キャッシュ インスタンスに対してコマンドを発行するには、目的のコマンドをコンソールに入力します。

![Redis コンソール](./media/cache-configure/redis-console.png)

Azure Redis Cache で無効にする Redis コマンドの一覧を参照してください、以前 [Azure Redis Cache でサポートされない Redis コマンド](#redis-commands-not-supported-in-azure-redis-cache) セクションです。 Redis コマンドの詳細については、次を参照してください。 [http://redis.io/commands](http://redis.io/commands)します。

## 次のステップ

-   Redis コマンドの使用の詳細については、を参照してください [Redis コマンドを実行する方法ですか?](cache-faq.md#how-can-i-run-redis-commands)。.




