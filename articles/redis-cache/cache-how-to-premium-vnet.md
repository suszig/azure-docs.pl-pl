<properties 
    pageTitle="Premium Azure Redis Cache の Virtual Network のサポートを構成する方法" 
    description="Premium レベル Azure Redis Cache インスタンスの Virtual Network のサポートを作成および管理する方法" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/14/2015" 
    ms.author="sdanie"/>


# Premium Azure Redis Cache の Virtual Network のサポートを構成する方法

Azure Redis Cache には、新しい Premium レベルなど、キャッシュのサイズと機能を柔軟に選択できるさまざまなキャッシュ サービスがあります。

Azure Redis Cache の Premium レベルには、クラスタリング、永続性、および Virtual Network (VNET) のサポートが含まれています。 VNET は、クラウド内のユーザー独自のネットワークを表すものです。 VNET を使用して Azure Redis Cache インスタンスを構成する場合、パブリックにアドレスを指定することはできず、VNET 内のクライアントからのみ指定できます。 この記事では、Azure Redis Cache インスタンスの Virtual Network のサポートを構成する方法について説明します。

詳細については、その他の premium のキャッシュ機能は、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md) と [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。

## VNET を選ぶ理由

[Azure 仮想ネットワーク (VNET)](https://azure.microsoft.com/services/virtual-network/) 展開が強化されたセキュリティと、Azure Redis Cache、だけでなく、アクセス制御ポリシー、サブネットの分離を提供し、さらにその他の機能が Azure Redis Cache へのアクセスを制限します。

## Virtual Network のサポート

Virtual Network (VNET) のサポートは、キャッシュの作成中に **[Redis Cache の新規作成]** ブレードで構成します。 キャッシュを作成するにサインインする、 [Azure ポータル](https://portal.azure.com) ] をクリック **新規**]-> [**データ + ストレージ**>**Redis Cache**します。

![Redis Cache の作成][redis-cache-new-cache-menu]

VNET のサポートを構成するには、まず **[料金レベルの選択]** ブレードで **[Premium]** キャッシュのいずれかを選択します。

![価格レベルの選択][redis-cache-premium-pricing-tier]

Azure Redis Cache VNET の統合は、**[Virtual Network]** ブレードで構成します。 ここでは、既存の従来型 VNET を選択できます。 新しい VNET を使用する」の手順に従います [Azure ポータルを使用して仮想ネットワーク (従来のもの) の作成](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) にし、戻り値と、 **Redis キャッシュの仮想ネットワーク** ブレードをオンにします。
>[AZURE.NOTE] Azure Redis Cache は、クラシック VNET に対応しています。 従来の VNET を作成する方法の詳細については、次を参照してください。 [Azure ポータルを使用して仮想ネットワーク (従来のもの) の作成](../virtual-network/virtual-networks-create-vnet-classic-pportal.md)します。 ARM VNET を従来の Vnet を接続する方法については、次を参照してください。 [従来の Vnet を新しい Vnet に接続する](../virtual-network/virtual-networks-arm-asm-s2s.md)します。

![Virtual Network][redis-cache-vnet]

**[Virtual Network]** ブレードの **[Virtual Network]** をクリックして選択し、VNET を構成します。

![Virtual Network][redis-cache-vnet-select]

目的の VNET をクリックして選択します。

![Virtual Network][redis-cache-vnet-subnet]

[サブネット] をクリックして目的のサブネットを選択します。

![Virtual Network][redis-cache-vnet-ip]

**[静的 IP アドレス]** フィールドは省略可能です。 ここでなにも指定しない場合は、選択したサブネットから選択されます。 特定の静的 IP が必要な場合は、目的の **[静的 IP アドレス]** を入力し、**[OK]** をクリックして VNET の構成を保存します。 選択した静的 IP が既に使用中の場合、エラー メッセージが表示されます。

キャッシュを作成すると、同じ VNET 内のクライアントからのみアドレスを指定できます。
>[AZURE.IMPORTANT] Azure Redis キャッシュ インスタンスは、VNET を使用する場合にアクセスするに最初のパラメーターとして、VNET 内のキャッシュの静的 IP アドレスを渡すし、に渡す、 `sslhost` によってキャッシュのエンドポイントを持つパラメーター。 次の例では、静的 IP アドレスは `10.10.1.5` 、キャッシュ エンドポイントが `contoso5.redis.cache.windows.net`します。

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("10.10.1.5,sslhost=contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=password");
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

## Azure Redis Cache VNET についてよく寄せられる質問 (FAQ)

次の一覧は、Azure Redis Cache のスケーリングに関するよく寄せられる質問への回答です。

## Azure Redis Cache と VNET に構成の誤りがあると、一般的にどのような問題がありますか

Azure Redis Cache が正常に動作しなくなる可能性がある一般的な構成の誤りを次の一覧に示します。

-   DNS へのアクセスがありません。 VNET 内の Azure Redis Cache インスタンスは、キャッシュの監視およびランタイム システムの一部で DNS へのアクセスを必要とします。 キャッシュ インスタンスに DNS へのアクセスがない場合、監視が機能せず、キャッシュは正しく機能しません。
-   クライアントから Redis への接続に使用する TCP ポート (6379 または 6380) がブロックされています。
-   Virtual Network からの送信 HTTPS トラフィックがブロックまたは遮断されています。 Azure Redis Cache は、Azure サービス (特に Storage) に対する送信 HTTPS トラフィックを使用します。
-   サブネット内の Redis ロール インスタンス VM が相互に通信できません。 Redis ロール インスタンスは、使用されているポートのいずれかで TCP を使用して相互に通信できるようにする必要があります。ポートは変化する可能性がありますが、最低でも Redis CSDEF ファイルで使用されているすべてのポートを使用できると仮定できます。
-   TCP/HTTP ポート 16001 で Azure Load Balancer から Redis VM への接続がブロックされています。 Azure Redis Cache は、既定の Azure Load Balancer プローブが有効なロール インスタンスを判断する処理に依存しています。 既定の Load Balancer プローブは、ポート 16001 で Azure ゲスト エージェントに ping を送信することで判断します。 ILB から転送されるトラフィックを受信するローテーションには、ping に反応したロール インスタンスのみが組み込まれます。 ポートのブロックが原因で ping が失敗するため、ローテーション内にインスタンスがない場合、ILB は受信 TCP 接続を一切受け付けません。
-   SSL 公開キーに使用されるクライアント アプリケーションの Web トラフィックがブロックされています。 ポート 6380 を使用して Redis に接続し、SSL サーバー認証を実行する際に SSL 証明書の検証を実行するには、(Virtual Network 内の) Redis のクライアントがパブリック インターネットに対して HTTP トラフィックを通信できる状態で、CA 証明書と証明書失効リストをダウンロードできる必要があります。
-   Azure ロード バランサーがポート 1300 x には、TCP を使用してクラスターでの Redis Vm に接続していることを禁止 (13000、13001 などです)。 または 1500 x (15000、15001 などです。)。 VNET は、これらのポートを開く Load Balancer プローブを含む csdef ファイルで構成されています。 Azure Load Balancer は、NSG から許可されている必要があります。既定の NSG は、タグ AZURE_LOADBALANCER を使用してこの処理を実行します。 Azure Load Balancer には、168.63.126.16 の静的 IP アドレスが割り当てられています。 詳細については、を参照してください [ネットワーク セキュリティ グループ (NSG) は何ですか?](../virtual-network/virtual-networks-nsg.md)。.

## Standard キャッシュまたは Basic キャッシュで VNET を使用できますか

VNET は Premium キャッシュでのみ使用できます。

## 次のステップ

Premium キャッシュ機能をさらに使用する方法を学習します。

-   [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md)
-   [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)









[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-vnet/redis-cache-new-cache-menu.png 
[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-vnet/redis-cache-premium-pricing-tier.png 
[redis-cache-vnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet.png 
[redis-cache-vnet-select]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-select.png 
[redis-cache-vnet-ip]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-ip.png 
[redis-cache-vnet-subnet]: ./media/cache-how-to-premium-vnet/redis-cache-vnet-subnet.png 

