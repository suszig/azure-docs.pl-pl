<properties 
   pageTitle="Azure Storage のスケーラビリティおよびパフォーマンスのターゲット | Microsoft Azure"
   description="Standard Storage アカウントと Premium Storage アカウントの両方の容量、要求レート、および送受信の帯域幅を含む、Azure Storage のスケーラビリティとパフォーマンスのターゲットについて説明します。 各 Azure Storage サービス内にあるパーティションのパフォーマンス ターゲットを理解します。"
   services="storage"
   documentationCenter="na"
   authors="robinsh"
   manager="carmonm"
   editor="na" />
<tags 
   ms.service="storage"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="storage"
   ms.date="12/04/2015"
   ms.author="robinsh" />

# Azure Storage のスケーラビリティおよびパフォーマンスのターゲット

## 概要

このトピックでは、Microsoft Azure Storage のスケーラビリティとパフォーマンスについて説明します。 その他の Azure の制限の概要については、次を参照してください。 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../azure-subscription-service-limits.md)します。

>[AZURE.NOTE] すべてのストレージ アカウントは、新しいフラット ネットワーク トポロジで実行され、作成時に関係なく、以下に示すスケーラビリティおよびパフォーマンスのターゲットをサポートします。 Azure ストレージのフラット ネットワーク アーキテクチャおよびスケーラビリティの詳細については、次を参照してください。 [Microsoft Azure Storage: A 高可用クラウド ストレージ サービス高い整合性を持つ](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)です。

<!-- -->

>[AZURE.IMPORTANT] ここのスケーラビリティおよびパフォーマンスのターゲットは、ハイエンドのターゲットが達成可能です。 いかなる場合でも、ストレージ アカウントで達成される要求レートおよび帯域幅は、格納されたオブジェクトのサイズ、使用されているアクセス パターン、およびアプリケーションで実行されているワークロードの種類によって異なります。 必ずサービスをテストして、パフォーマンスがユーザー要件を満たしているかどうかを確認してください。 可能であれば、トラフィック量の急増を回避し、トラフィックがパーティション間でうまく分散されるようにしてください。

>ワークロードがアプリケーションのパーティションで処理できる上限に達すると、Azure Storage はエラー コード 503 (サーバーがビジー状態) またはエラー コード 500 (操作タイムアウト) の応答を返しはじめます。 このような状況になった場合、アプリケーションで指数関数的バックオフによる再試行ポリシーを使用する必要があります。 指数関数的バックオフによって、そのパーティションへの負荷が減少し、そのパーティションへのトラフィック量の増加が緩和されます。

アプリケーションで必要とされるスケーラビリティが、単一ストレージ アカウントあたりのスケーラビリティ ターゲットを超えている場合、複数のストレージ アカウントを使用し、それらのストレージ アカウント間でデータが分割されるようにアプリケーションを構築できます。 参照してください [ストレージの料金詳細](http://azure.microsoft.com/pricing/details/storage/) について」を参照します。


## Blob、キュー、テーブル、およびファイルのスケーラビリティ ターゲット

[AZURE.INCLUDE [azure-storage-limits](../../includes/azure-storage-limits.md)]

## 仮想マシンのディスクのスケーラビリティ ターゲット 

[AZURE.INCLUDE [azure-storage-limits-vm-disks](../../includes/azure-storage-limits-vm-disks.md)]

参照してください [仮想マシンのサイズ](../virtual-machines/virtual-machines-size-specs.md) さらに詳細です。

### Standard Storage アカウント

[AZURE.INCLUDE [azure-storage-limits-vm-disks-standard](../../includes/azure-storage-limits-vm-disks-standard.md)]

### Premium Storage アカウント

[AZURE.INCLUDE [azure-storage-limits-vm-disks-premium](../../includes/azure-storage-limits-vm-disks-premium.md)]

## Azure リソース マネージャーのスケーラビリティ ターゲット

[AZURE.INCLUDE [azure-storage-limits-azure-resource-manager](../../includes/azure-storage-limits-azure-resource-manager.md)]

## Azure Storage 内のパーティション

Azure Storage に格納されているデータを含むすべてのオブジェクト (BLOB、メッセージ、エンティティ、およびファイル) はパーティションに属し、パーティション キーによって識別されます。 Azure Storage では、BLOB、メッセージ、エンティティ、およびファイルのトラフィック ニーズに合わせて、どのようにこれらのオブジェクトの負荷をサーバー間で分散するかがパーティションによって決まります。 パーティション キーはストレージ アカウント内で一意であり、BLOB、メッセージ、またはエンティティを見つけるために使用されます。

上記の表 [標準ストレージ アカウントのスケーラビリティ ターゲット](#scalability-targets-for-standard-storage-accounts) サービスごとに 1 つのパーティションにおけるパフォーマンス ターゲットの一覧が表示されます。

パーティションは、各ストレージ サービスの負荷分散およびスケーラビリティに以下のような影響を与えます。

- **Blob**: blob のパーティション キーは、コンテナー名 + blob 名。 これは、各 BLOB にそれぞれのパーティションが存在するということです。 これによって BLOB を多数のサーバーに分散できるため、BLOB へのアクセスのスケールアウトが可能になります。 BLOB は BLOB コンテナーに論理的にグループ化できますが、このグループ化によってパーティション分割は影響を受けません。

- **ファイル**: ファイルのパーティション キーは、アカウント名 + ファイル名を共有します。 これは、ファイル共有内のすべてのファイルも、1 つのパーティションに存在することを意味します。

- **メッセージ**: メッセージのパーティション キーには、キュー名があるため、キュー内のすべてのメッセージは 1 つのパーティションにグループ化し、1 つのサーバーで処理されます。 ストレージ アカウント内のキューがどんなに多くとも、負荷が分散されるように、異なるキューが別のサーバーによって処理されることがあります。

- **エンティティ**: パーティション キーのエンティティは、テーブル名 + パーティション キー、パーティション キーは、必要な値をユーザー定義 **PartitionKey** エンティティのプロパティです。  

    同じパーティション キー値を持つすべてのエンティティは、同じパーティションにグループ化され、同じパーティション サーバーに格納されます。 これは、アプリケーションを設計する際に理解しておくべき重要なポイントです。 アプリケーションでは、エンティティを複数のパーティションに分散することで得られるスケーラビリティ上のメリットと、エンティティを 1 つのパーティションにグループ化することで得られるデータ アクセスのメリットのバランスをとってください。 

    テーブル内のエンティティのセットを単一のパーティションにグループ化する主な利点は、パーティションが 1 台のサーバー上にあるため、同じパーティション内のエンティティすべてにアトミック バッチ操作を実行できるということです。 そのため、バッチ操作を実行する場合は、同じパーティション キーでエンティティをグループ化することを検討します。

    一方、同じテーブル内にあっても、異なるパーティションに属しているエンティティは、異なるサーバー間で負荷分散できるため、大きなテーブルの処理が可能になり、スケーラビリティもさらに大きくなります。

## 関連項目

- [Storage の料金詳細](http://azure.microsoft.com/pricing/details/storage/)
- [Azure サブスクリプションとサービスの制限、クォータ、制約](../azure-subscription-service-limits.md)
- [Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ](storage-premium-storage-preview-portal/)
- [Azure ストレージのレプリケーション](storage-redundancy.md)
- [Microsoft Azure Storage のパフォーマンスとスケーラビリティに対するチェック リスト](storage-performance-checklist.md)
- [Microsoft Azure Storage: 強力な整合性を備えた高可用クラウド ストレージ サービス](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)


