<properties
    pageTitle="Azure リージョン ペアによるビジネス継続性の向上"
    description="リージョン ペアを使用して、データセンターでの障害発生時にアプリケーションの耐障害性を維持します。"
    services="multiple"
    documentationCenter=""
    authors="rboucher"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2015"
    ms.author="robb"/>

# Azure リージョン ペアによる可用性の向上

## Azure リージョン ペアの説明

Azure は、世界中の複数の geo で動作します。 Azure の geo とは、少なくとも 1 つの Azure リージョンを含む、世界の定義済みの地域です。 Azure リージョンは、geo に含まれる領域で、1 つ以上のデータセンターが含まれます。

また、各 Azure リージョンが、そのリージョンと同じ geo 内の別のリージョンと組み合わされています （geo 外のリージョンとペアになっているブラジル南部を除く）。これがリージョン ペアです。


![AzureGeography](./media/best-practices-availability-paired-regions/GeoRegionDataCenter.png)

図 1 – Azure リージョン ペアの図



| [地理的な場所]     |  ペアになっているリージョン  |                  |
| :-------------| :-------------   | :-------------   |
| 北米 | 米国中北部 | 米国中南部 |
| 北米 | 米国東部          | 米国西部          |
| 北米 | 米国東部 2        | 米国中部       |
| ヨーロッパ        | 北ヨーロッパ     | 西ヨーロッパ      |
| アジア          | 東南アジア  | 東アジア        |
| 中国         | 中国東部       | 中国北部      |
| 日本         | 東日本       | 西日本       |
| ブラジル        | ブラジル南部 (1) | 米国中南部 |
| オーストラリア     | オーストラリア東部   | オーストラリア南東部|
| 米国政府 | 米国政府アイオワ州      | 米国政府バージニア州  |

表 1 - Azure リージョン ペアの組み合わせ

> (1) ブラジル南部は、自身の geo 外のリージョンとペアになっているため特殊です。 ブラジル南部のセカンダリ リージョンは米国中南部ですが、米国中南部のセカンダリ リージョンはブラジル南部ではありません。

リージョン ペアの間でワークロードをレプリケートして、Azure の分離と可用性のポリシーを活用することをお勧めします。 たとえば、計画的な Azure システムの更新プログラムは、ペア リージョンに (同時にではなく) 順番にデプロイされます。 つまり、更新プログラムに不具合があっても (めったにありませんが)、両方のリージョンが同時に影響を受けることはありません。 さらに、万一、広範囲にわたって障害が発生した場合は、すべてのペアにおいて、少なくとも一方のリージョンの復旧が優先されます。

## リージョン ペアの例
以下の図 2 は、一対のリージョンを使って障害復旧を行う架空のアプリケーションです。 緑色の番号は、3 つの Azure サービス (Azure Compute、Storage、およびデータベース) のリージョン間アクティビティと、そのアクティビティが、リージョン間でのレプリケートのためにどのように構成されているかを示しています。 リージョン ペアにデプロイするメリットは、オレンジ色の番号で示されています。


![ペア リージョンのメリットの概要](./media/best-practices-availability-paired-regions/PairedRegionsOverview2.png)

図 2 – Azure リージョン ペアの例

## リージョンの境界を越えるアクティビティ
図 2 を参照してください。

![1Green](./media/best-practices-availability-paired-regions/1Green.png) **Azure コンピューティング (PaaS)** – 事前にリソースを確実に利用可能な別のリージョンに、災害時に追加のコンピューティング リソースをプロビジョニングする必要があります。 詳細については、次を参照してください [Azure のビジネス継続性テクニカル ガイダンス。](https://msdn.microsoft.com/library/azure/hh873027.aspx)

![2Green](./media/best-practices-availability-paired-regions/2Green.png) **Azure Storage** -Geo 冗長ストレージ (GRS) が、Azure ストレージ アカウントの作成時に既定で構成します。 GRS を使用すると、データはプライマリ リージョン内で 3 回、ペア リージョンで 3 回、自動的にレプリケートされます。 詳細については、次を参照してください。 [Azure Storage 冗長オプション](../storage/storage-redundancy.md)します。


![3Green](./media/best-practices-availability-paired-regions/3Green.png) **Azure SQL データベース** – Azure SQL Standard Geo レプリケーション、ペア リージョンへのトランザクションの非同期レプリケーションを構成することができます。 Premium geo レプリケーションを使用すると、世界中のすべてのリージョンへのレプリケーションを構成できますが、通常の障害復旧では、これらのリソースをペア リージョンにデプロイすることをお勧めします。 詳細については、次を参照してください  [Azure SQL Database の Geo レプリケーション。](https://msdn.microsoft.com/library/azure/dn783447.aspx)

![4Green](./media/best-practices-availability-paired-regions/4Green.png) **Azure リソース マネージャー (ARM)** -ARM は本質的にリージョン間でのサービス管理のコンポーネントが論理的に切り離さを提供します。 つまり、1 つのリージョンで論理的な障害が発生しても、他のリージョンが影響を受ける可能性はそれほど高くありません。

## ペア リージョンのメリット
図 2 を参照してください。  

![5Orange](./media/best-practices-availability-paired-regions/5Orange.png)
**物理的な分離** – azure がなくても実現できないことも、実用的な一部の地域でリージョンのペアのデータ センター間の距離は 300 マイル以上を推奨します。 物理データセンターを切り離すことで、自然災害、社会不安、停電、および物理ネットワーク障害が両方のリージョンにすぐに影響を及ぼす可能性が少なくなります。 分離は、geo 内の制約 (広さ、電源/ネットワーク インフラストラクチャの可用性、規制など) に左右されます。  

![6Orange](./media/best-practices-availability-paired-regions/6Orange.png)
**プラットフォームによって提供されるレプリケーション** -Geo 冗長ストレージなど一部のサービスは、ペア リージョンへの自動レプリケーションを提供します。

![7Orange](./media/best-practices-availability-paired-regions/7Orange.png)
**リージョン復旧順序** –、広範な停止のすべてのペアの 1 つのリージョンの復旧が優先されます。 ペア リージョンにまたがってデプロイされているアプリケーションについては、そのアプリケーションのリージョンのどちらかが必ず優先的に復旧します。 ペアになっていない複数のリージョンにアプリケーションがデプロイされていると、復旧が遅れる可能性があり、最悪の場合は、回復されてない最後の 2 つになってしまうことがあります。

![8Orange](./media/best-practices-availability-paired-regions/8Orange.png)
**順次更新** – 計画的な Azure システムの更新がロールアウト ペア リージョンに順番に (同じ時間) ではなくダウンタイム、バグ、および不適切な更新プログラムのまれなイベントでの論理エラーの影響を最小限に抑える。


![9Orange](./media/best-practices-availability-paired-regions/9Orange.png)
**データ常駐** – リージョンは、税および法の執行を目的にしたデータ常駐要件を満たすために (ブラジル南部) を除く、ペアとして同じ geo に常駐します。

