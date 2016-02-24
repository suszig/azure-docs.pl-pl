<properties 
    pageTitle="Premium Azure Redis Cache のデータ永続化の構成方法" 
    description="Premium レベルの Azure Redis Cache インスタンス用にデータの永続化を構成して管理する方法について説明します" 
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
    ms.date="12/03/2015" 
    ms.author="sdanie"/>

# Premium Azure Redis Cache のデータ永続化の構成方法

Azure Redis Cache には、新しい Premium レベルなど、キャッシュのサイズと機能を柔軟に選択できるさまざまなキャッシュ サービスがあります。

Azure Redis Cache の Premium レベルには、クラスタリング、永続性、および Virtual Network のサポートが含まれています。 この記事では、Premium Azure Redis Cache インスタンスで永続化を構成する方法について説明します。

詳細については、その他の premium のキャッシュ機能は、次を参照してください。 [Premium Azure Redis Cache のクラスタ リングを構成する方法](cache-how-to-premium-clustering.md) と [Premium Azure Redis Cache 用の仮想ネットワークのサポートを構成する方法](cache-how-to-premium-vnet.md)します。

## データの永続化とは
Redis の永続化を使用すると、Redis に格納されたデータを保持できます。 また、スナップショットを取得したりデータをバックアップしたりして、ハードウェア障害のときに読み込むことができます。 これは、Basic レベルや Standard レベルにはない大きな利点です。Basic/Standard レベルでは、すべてのデータはメモリに格納され、Cache ノードがダウンするような障害時にはデータが失われる可能性があります。 

Azure Redis Cache は、Redis の永続化を使用して、 [RDB モデル](http://redis.io/topics/persistence), 、場所データを Azure ストレージ アカウントに格納します。 永続化を構成すると、Azure Redis Cache は、Redis Cache のスナップショットを構成可能なバックアップ頻度に基づいて Redis バイナリ形式でディスクに保存します。 プライマリとレプリカの両方のキャッシュが無効になるような致命的なイベントが発生した場合、最新のスナップショットを使用してキャッシュが再構築されます。

永続化を構成することができます、 **新しい Redis Cache** ブレード キャッシュの作成中にし、[、 **設定** 既存の premium のブレードをキャッシュします。

## Premium キャッシュの作成

キャッシュを作成し、永続化を設定してにサインインする、 [Azure ポータル](https://portal.azure.com) ] をクリック **新規**]-> [**データ + ストレージ**>**Redis Cache**します。

![Redis Cache の作成][redis-cache-new-cache-menu]

永続化を設定するには、最初の 1 つ選択、 **Premium** でのキャッシュを **価格レベルを選択して** ブレードです。

![価格レベルの選択][redis-cache-premium-pricing-tier]

クリックして、premium 価格レベルを選択すると、 **Redis の永続化**します。

![Redis の永続化][redis-cache-persistence]

次のセクションの手順は、新しい Premium キャッシュに Redis の永続化を構成する方法を示します。 Redis の永続化が構成されると、クリックして **作成** の Redis の永続化によって、新しい premium キャッシュを作成します。

## Redis の永続化を構成する

Redis の永続化が構成されている、 **データの永続性を Redis** ブレードです。 新規キャッシュでは、前のセクションで説明したように、このブレードにはキャッシュの作成プロセス中にアクセスします。 既存のキャッシュで、 **データの永続性を Redis** からブレードにアクセス、 **設定** 、キャッシュのブレードです。

![Redis の設定][redis-cache-settings]

Redis の永続性を有効にするにはクリックして **有効** RDB (Redis データベース) のバックアップを有効にします。 以前に有効にプレミアム キャッシュの Redis の永続性を無効にするには、 **無効になっている**します。

バックアップの間隔を構成するには、選択、 **バックアップ頻度** ドロップ ダウン リストからです。 選択できるオプションは **15 分**, 、**30 分**, 、**60 分**, 、**6 時間**, 、**12 時間**, 、および **24 時間**します。 前のバックアップ操作が正常に完了するとこの間隔のカウントダウンが開始し、期間が経過すると新しいバックアップが開始されます。

クリックして **ストレージ アカウント** を使用して、いずれかを選択するストレージ アカウントを選択、 **主キー** または **セカンダリ キー** から使用する、 **ストレージ キー** ドロップダウンです。 キャッシュと同じリージョンにストレージ アカウントを選択する必要があり、 **Premium Storage** 有してより高いスループットを premium ストレージ アカウントをお勧めします。 

>[AZURE.IMPORTANT] 永続化アカウントのストレージ キーが再生成された場合は、目的のキーを再選択してください、 **ストレージ キー** ボックスの一覧です。

![Redis の永続化][redis-cache-persistence-selected]

をクリックして **OK** 永続化の構成を保存します。

バックアップ間隔が経過すると、次のバックアップ (または新しいキャッシュの最初のバックアップ) が開始されます。



## 永続化の FAQ

次の一覧は、Azure Redis Cache の永続化に関するよく寄せられる質問への回答です。

## 以前に作成したキャッシュで永続化を有効にできますか

はい、Redis の永続化はキャッシュの作成時と、既存の Premium キャッシュの両方に構成できます。

## キャッシュの作成後にバックアップ頻度を変更できますか

[はい]、バックアップの頻度を変更できます、 **データの永続性を Redis** ブレードです。 手順については、次を参照してください。 [永続化の Redis 構成](#configure-redis-persistence)します。

## バックアップ間隔を 60 分に設定した場合、バックアップの間隔が 60 分より長くなるのはなぜですか

バックアップ間隔は、前のバックアップ プロセスが正常に完了するまでは開始しません。 バックアップ間隔を 60 分に設定し、バックアップ プロセスが正常に完了するのに 15 分かかる場合、次のバックアップは、前回のバックアップの開始時刻から 75 分経つまで開始しません。

## 新しいバックアップが作成されると、古いバックアップはどうなりますか

最新のものを除くすべてのバックアップは自動的に削除されます。 この削除はすぐに行われないことがありますが、古いバックアップは無期限には保持されません。

## 次のステップ
Premium キャッシュ機能をさらに使用する方法を学習します。

-   [How to configure clustering for a Premium Azure Redis Cache (Premium Azure Redis Cache のクラスタリングの構成方法)](cache-how-to-premium-clustering.md)
-   [Premium Azure Redis Cache の Virtual Network のサポートを構成する方法](cache-how-to-premium-vnet.md)
  
<!-- IMAGES -->

[redis-cache-new-cache-menu]: ./media/cache-how-to-premium-persistence/redis-cache-new-cache-menu.png

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-persistence-selected]: ./media/cache-how-to-premium-persistence/redis-cache-persistence-selected.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
