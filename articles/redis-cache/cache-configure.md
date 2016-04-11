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

クリックして **Redis Caches** してキャッシュを表示します。

![Azure Redis Cache の参照キャッシュ リスト](./media/cache-configure/IC796921.png)

キャッシュのプロパティを表示するには、目的のキャッシュを選択します。

![Redis Cache のすべての設定](./media/cache-configure/IC808312.png)

クリックして **設定** または **すべての設定** を表示して、キャッシュを構成します。

![Redis Cache の設定](./media/cache-configure/IC808313.png)

## プロパティ

クリックして **プロパティ** をキャッシュ エンドポイントやポートを含む、キャッシュに関する情報を表示します。

![Redis Cache のプロパティ](./media/cache-configure/IC808314.png)

## アクセス キー

クリックして **アクセス キー** を表示またはキャッシュのアクセス キーを生成します。 これらのキーがホスト名とポートと共に使用される、 **プロパティ** ブレードで、キャッシュに接続するクライアント。

![Redis Cache のアクセス キー](./media/cache-configure/IC808315.png)

## アクセス ポート

新しいキャッシュでは、SSL を使用しないアクセスが既定で無効になっています。 非 SSL ポートを有効にするにはクリックして **アクセス ポート** クリックしてブレード **なし**します。

![Redis Cache のアクセス ポート](./media/cache-configure/IC808316.png)

## [価格レベル]

クリックして **価格レベル** を表示またはキャッシュの価格レベルを変更します。 スケーリングの詳細については、次を参照してください。 [スケール Azure Redis Cache 方法](cache-how-to-scale.md)します。

![Redis Cache 価格レベル](./media/cache-configure/pricing-tier.png)

## 診断

クリックして **診断** キャッシュ診断の格納に使用されるストレージ アカウントを構成します。

![Redis Cache の診断](./media/cache-configure/IC808317.png)

詳細については、次を参照してください。 [Azure Redis Cache の監視方法](cache-how-to-monitor.md)します。

## maxmemory-policy と maxmemory-reserved

クリックして **Maxmemory ポリシー** キャッシュのメモリ ポリシーを構成します。  **Maxmemory ポリシー** 設定は、キャッシュの削除ポリシーを構成し、 **maxmemory 予約** キャッシュ以外のプロセス用に予約するメモリを構成します。

![Redis Cache の Maxmemory ポリシー](./media/cache-configure/IC808318.png)

**Maxmemory ポリシー** 次の削除ポリシーから選択することができます。

-   volatile-lru - これが、既定値です。
-   allkeys-lru
-   volatile-random
-   allkeys-random
-   volatile-ttl
-   noeviction

Maxmemory ポリシーの詳細については、次を参照してください。 [削除ポリシー](http://redis.io/topics/lru-cache#eviction-policies)します。

 **Maxmemory 予約** 設定がフェールオーバーに伴うレプリケーションなどの非キャッシュ操作のために予約されている mb 単位でメモリの量を構成します。 また、断片化率が高い場合も使用できます。 この値を設定すると、負荷が変化するときでも、Redis サーバーの稼働状態がより安定します。 この値は、書き込みが大量に発生するワークロードに対しては、高く設定する必要があります。 メモリがこのような操作のために予約されていると、キャッシュされたデータの保存に使用できなくなります。

>[AZURE.IMPORTANT]  **Maxmemory 予約** 設定は、標準の利用可能なだけとプレミアムのキャッシュします。

## キースペース通知 (詳細設定)

クリックして **詳細設定** Redis キースペース通知を構成します。 キースペース通知により、特定のイベントが発生したときに、クライアントが通知を受信できます。

![Redis Cache の高度な設定](./media/cache-configure/IC808319.png)

>[AZURE.IMPORTANT] キースペース通知と **通知キースペース イベント** 標準およびプレミアムのキャッシュの設定はのみです。

詳細については、次を参照してください。 [Redis キースペース通知](http://redis.io/topics/notifications)します。 サンプル コードについては、 [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) ファイルで、 [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) サンプルです。

## Redis データの保持

クリックして **データの永続性を Redis** を有効にするのには、無効化、または、プレミアム キャッシュのデータの永続性を構成します。

![Redis データの保持](./media/cache-configure/redis-cache-persistence-settings.png)

Redis の永続性を有効にするにはクリックして **有効** RDB (Redis データベース) のバックアップを有効にします。 Redis の永続性を無効にするには、 **無効になっている**します。

バックアップの間隔を構成するには、選択、 **バックアップ頻度** ドロップ ダウン リストからです。 選択できるオプションは **15 分**, 、**30 分**, 、**60 分**, 、**6 時間**, 、**12 時間**, 、および **24 時間**します。 前のバックアップ操作が正常に完了するとこの間隔のカウントダウンが開始し、期間が経過すると新しいバックアップが開始されます。

クリックして **ストレージ アカウント** を使用して、いずれかを選択するストレージ アカウントを選択、 **主キー** または **セカンダリ キー** から使用する、 **ストレージ キー** ドロップダウンです。 キャッシュと同じリージョンにストレージ アカウントを選択する必要があり、 **Premium Storage** 有してより高いスループットを premium ストレージ アカウントをお勧めします。 目的のキーを選択する必要があります再、ストレージ アカウントのキーに永続化が再生されると、いつでも、 **ストレージ キー** ボックスの一覧です。

をクリックして **OK** 永続化の構成を保存します。

>[AZURE.IMPORTANT] Redis のデータの永続性は、プレミアム キャッシュのできるだけです。 詳細については、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md)します。

## Redis クラスター サイズ

をクリックして **(プレビュー) の Redis クラスター サイズ** を追加またはシャードを削除、実行中のプレミアムのキャッシュを有効になっているクラスター化します。

>[AZURE.NOTE] Azure Redis キャッシュの Premium 階層が一般公開にリリースされているときに、クラスター サイズの Redis 機能は現在プレビュー段階に注意してください。

![Redis クラスター サイズ](./media/cache-configure/redis-cache-redis-cluster-size.png)

シャードの数を変更するには、スライダーを使用してまたは 1 ~ 10 の範囲の数値を入力、 **シャードの数** テキスト ボックスをクリック **OK** を保存します。

>[AZURE.IMPORTANT] Redis クラスタ リングはプレミアムのキャッシュにできるだけです。 詳細については、次を参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。


## ユーザーとタグ

![Redis Cache のユーザーとタグ](./media/cache-configure/IC808320.png)

 **ユーザー** では、簡単かつ正確には、アクセス管理の要件を満たすを支援する、Azure ポータルでのロールベースのアクセス制御 (RBAC) のサポートを説明します。 詳細については、次を参照してください。 [Azure ポータルでのロールベースのアクセス制御](http://go.microsoft.com/fwlink/?LinkId=512803)します。

 **タグ** セクションでは、リソースの整理できます。 詳細については、次を参照してください。 [タグを使用した Azure リソースの整理に](../resource-group-using-tags.md)します。

## 既定の Redis サーバー構成

新しい Azure Redis Cache インスタンスに適用される既定の Redis 構成値は、次のとおりです。

>[AZURE.NOTE] 使用してこのセクションの設定を変更できない、 `StackExchange.Redis.IServer.ConfigSet` メソッドです。 このセクションのいずれかのコマンドでこのメソッドを呼び出すと、次のような例外がスローされます。  
>
>`StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
>  
>などの構成可能値 **最大メモリ ポリシー**, 、Azure ポータルを使用して構成できます。

|設定|既定値|説明|
|---|---|---|
|databases|16|既定のデータベースは DB 0 です。接続を使用して、接続ごとに異なるデータベースを選択できます。GetDataBase(dbid) の dbid は 0 ～ 15 の数値です。|
|maxclients|価格レベルによって異なります<sup>1</sup>|これは、同時に接続が許可されているクライアントの最大数です。 制限に達すると、Redis はすべての新しい接続を終了し、エラー 'max number of clients reached' を送信します。|
|maxmemory-policy|volatile-lru|Maxmemory ポリシーは、maxmemory (キャッシュ作成時に選択したキャッシュのサイズ) に達したときに、Redis が削除する項目を選択する方法についての設定です。 Azure Redis Cache の既定の設定は volatile-lru で、LRU アルゴリズムを使用して有効期限が設定されたキーを削除します。 この設定は、Azure ポータルで構成できます。 詳細については、次を参照してください。 [maxmemory-policy と maxmemory-reserved](#maxmemory-policy-and-maxmemory-reserved)します。|
|maxmemory-samples|3|LRU アルゴリズムと最小 TTL アルゴリズムは精緻なアルゴリズムではなく、(メモリを節約するための) 近似アルゴリズムです。そのため、サンプル サイズも選択して確認できます。 既定の Redis インスタンスの場合、キーを 3 つ確認し、直近の使用頻度が比較的低い ものを 1 つ選択します。|
|lua-time-limit|5,000|Lua スクリプトの最大実行時間 (ミリ秒)。 最大実行時間に達した場合は、Redis は、最大許容時間の後もスクリプトが実行中であることをログに記録し、クエリに対してエラーを知らせる応答を開始します。|
|lua-event-limit|500|これは、スクリプト イベント キューの最大サイズです。|
|client-output-buffer-limit normalclient-output-buffer-limit pubsub|0 0 032mb 8mb 60|このクライアントの出力バッファー制限を使用して、なんらかの理由 (一般的には、パブリッシャーがメッセージを作成するのと同じ速度で Pub/Sub クライアントがメッセージを利用できないという理由) により、サーバーから十分な速度でデータを読み込んでいないクライアントを強制的に切断することができます。 詳細については、次を参照してください。 [http://redis.io/topics/clients](http://redis.io/topics/clients)します。|

<sup>1</sup>`maxclients` は Azure Redis Cache の価格レベルによって異なります。

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

>[AZURE.IMPORTANT] Azure Redis Cache インスタンスの構成と管理が行われるため、Azure ポータルを使用して、次のコマンドは無効です。 これらのコマンドを呼び出そうとすると、`"(error) ERR unknown command"` のようなエラー メッセージを受け取ります。
>
>-  BGREWRITEAOF
>-  BGSAVE
>-  CONFIG
>-  DEBUG
>-  MIGRATE
>-  保存
>-  SHUTDOWN
>-  SLAVEOF

Redis コマンドの詳細については、次を参照してください。 [http://redis.io/commands](http://redis.io/commands)します。

## Redis コンソール

コマンドを使用して、Azure Redis Cache インスタンスを安全に実行、 **Redis コンソール**, 標準使用されている、および Premium をキャッシュします。

>[AZURE.IMPORTANT] VNET またはクラスタ リングでは、Redis コンソールはされていません。 
>
>-  [VNET](cache-how-to-premium-vnet.md) 、VNET 内のクライアントがキャッシュにアクセスできる専用キャッシュは、VNET の一部とするときにします。 Redis コンソールは、VNET の一部ではない VM でホストされている redis-cli.exe クライアントを使用するため、キャッシュに接続できません。
>-  [クラスタ リング](cache-how-to-premium-clustering.md) -「Redis コンソールは、この時点でクラスタ リングをサポートしていません cli.exe クライアントを使用します。 Redis cli ユーティリティ、 [不安定](http://redis.io/download) GitHub で Redis リポジトリの分岐で起動したときの基本的なサポートを実装して、 `-c` スイッチです。 詳細については、次を参照してください。 [クラスターと再生](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) に [http://redis.io](http://redis.io) で、 [Redis クラスター チュートリアル](http://redis.io/topics/cluster-tutorial)します。

Redis コンソールにアクセスするには、クリックして **コンソール** から、 **Redis Cache** ブレードです。

![Redis コンソール](./media/cache-configure/redis-console-menu.png)

キャッシュ インスタンスに対してコマンドを発行するには、目的のコマンドをコンソールに入力します。

![Redis コンソール](./media/cache-configure/redis-console.png)

Azure Redis Cache で無効にする Redis コマンドの一覧を参照してください、以前 [Azure Redis Cache でサポートされない Redis コマンド](#redis-commands-not-supported-in-azure-redis-cache) セクションです。 Redis コマンドの詳細については、次を参照してください。 [http://redis.io/commands](http://redis.io/commands)します。 

## 次のステップ
-   Redis コマンドの使用の詳細については、次を参照してください。 [Redis コマンドを実行する方法ですか?](cache-faq.md#how-can-i-run-redis-commands)します。
