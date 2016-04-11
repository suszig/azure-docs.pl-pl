<properties 
    pageTitle="Azure Redis Cache Premium レベルの概要" 
    description="Premium レベルの Azure Redis Cache インスタンス向けの Redis の永続化、Redis クラスタリング、および VNET サポートの作成と管理方法について説明します。" 
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
    ms.date="10/02/2015" 
    ms.author="sdanie"/>

# Azure Redis Cache Premium レベルの概要
Azure Redis Cache は、データへの超高速アクセスを提供することで、拡張性や応答性の高いアプリケーションの作成を可能にするマネージ キャッシュです。 

新しい Premium レベルは、Standard 階層の全機能に加えて、パフォーマンスの向上、ワークロードの増加、障害復旧、セキュリティの強化などを備えたエンタープライズ対応のレベルです。 Premium キャッシュ レベルの追加機能の詳細をお知りになりたい場合は、このドキュメントを引き続きお読みください。

## Standard/Basic レベルを超えるパフォーマンス
**Standard/Basic レベルを超えるパフォーマンス。** Premium レベルのキャッシュは、高速プロセッサを備え、Basic/Standard レベルと比較して優れたパフォーマンスを発揮するハードウェア上にデプロイされます。 Premium レベルのキャッシュは、スループットが高く、待機時間が低くなっています。 

**同じサイズのキャッシュでも Standard レベルと比べて Premium のスループットの方が高い。** 例: 53 GB のキャッシュ P4 のスループットの (Premium) が 150 C6 K と比較して 1 秒あたりの 250 K 要求 (標準)。

参照してください、 [Azure Redis キャッシュに関する FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) サイズ、スループット、およびプレミアム キャッシュの帯域幅の詳細についてです。

## Redis データの保持
Premium レベルでは、Azure Storage アカウント内のキャッシュ データを永続化できます。 Basic/Standard のキャッシュでは、データはすべてメモリ内にのみ格納されます。 基盤となるインフラストラクチャに問題が発生した場合、データが失われる可能性があります。 Premium レベルの Resis データの保持機能を使用して、データ損失に対する復元性を高めることをお勧めします。 Azure Redis Cache は RDB と AOF (近日対応予定) のオプション [Redis の永続化](http://redis.io/topics/persistence)します。 

永続化の構成方法の詳細については、次を参照してください。 [Premium Azure Redis Cache 用の永続化を構成する方法](cache-how-to-premium-persistence.md)します。

##Redis クラスター
53 GB を超えるキャッシュを作成するか、複数の Redis ノード間でデータを共有する場合、Premium レベルで利用可能な Redis クラスタリングを使用することができます。 各ノードは、Azure によって管理される高可用性対応のプライマリ/レプリカ キャッシュのペアで構成されています。 

**Redis クラスタリングでは、スケールとスループットを最大限に利用できます。** クラスター内のシャード (ノード) の数を増やすと、スループットは直線的に増加します。 例: 10 個のシャードの P4 クラスターを作成する場合は、使用可能なスループットの 250 K * 10 = 1 秒あたりの 250万要求します。 参照してください、 [Azure Redis キャッシュに関する FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) サイズ、スループット、およびプレミアム キャッシュの帯域幅の詳細についてです。

クラスタ リングで開始するを参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md)します。

##強化されたセキュリティと分離
Basic/Standard レベルで作成されたキャッシュは、パブリック インターネット上でアクセス可能です。 キャッシュへのアクセスは、アクセス キーに基づいて制限されます。 Premium レベルでは、さらに、指定されたネットワーク内のクライアントだけがキャッシュにアクセスできるようにすることが可能になります。 Redis Cache を配置する、 [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/)します。 サブネット、アクセス制御ポリシー、およびその他の Redis へのアクセスをさらに制限する機能を始め、VNet のすべての機能を使用できます。

詳細については、次を参照してください。 [Premium Azure Redis Cache 用の仮想ネットワークのサポートを構成する方法](cache-how-to-premium-vnet.md)します。

## 次のステップ

キャッシュを作成し、Premium レベルの新機能を確認します。

-   [How to configure persistence for a Premium Azure Redis Cache (Premium Azure Redis Cache の永続性の構成方法)](cache-how-to-premium-persistence.md)
-   [Premium Azure Redis Cache の Virtual Network のサポートを構成する方法](cache-how-to-premium-vnet.md)
-   [How to configure clustering for a Premium Azure Redis Cache (Premium Azure Redis Cache のクラスタリングの構成方法)](cache-how-to-premium-clustering.md)
  



