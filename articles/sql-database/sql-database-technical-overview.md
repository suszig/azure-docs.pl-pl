<properties
    pageTitle="SQL Database とは SQL Database の概要 | Microsoft Azure"
    description="SQL Database の概要: クラウド内の Microsoft のリレーショナル データベース管理システム (RDBMS) の技術の詳細と機能について説明します。"
    keywords="introduction to sql,intro to sql,what is sql database,DTU"
    services="sql-database"
    documentationCenter=""
    authors="shontnew"
    manager="jeffreyg"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management"
   ms.date="09/30/2015"
   ms.author="shkurhek"/>

# SQL Database とは SQL Database の概要、技術の詳細、DTU の説明

SQL Database は、ミッション クリティカルな機能を備えた、市場をリードする Microsoft SQL Server エンジンに基づくクラウド内のリレーショナル データベース サービスです。 SQL Database は、予測可能なパフォーマンス、ダウンタイムなしの拡張性、ビジネス継続性、データ保護を提供し、いずれの場合も管理をほとんど必要としません。 仮想マシンとインフラストラクチャの管理に煩わされることなく、迅速なアプリケーション開発および製品化に要する時間の短縮化に専念することができます。 基づいているため、 [SQL Server](https://msdn.microsoft.com/library/bb545450.aspx) エンジンを既存の SQL Server のツール、ライブラリおよび Api を簡単に移動し、クラウドに拡張するための SQL データベースがサポートされます。

この記事では、パフォーマンス、スケーラビリティ、および管理容易性に関連する、SQL Database の中心概念および機能について紹介し、詳細を参照するためのリンクも提供します。 場合に移動する準備ができたら、 [初めて SQL database を作成](sql-database-get-started.md) または [弾力性データベース プールを作成する](sql-database-elastic-pool-portal.md) (分) です。 さらに詳しく知りたい場合は、この 30 分間のビデオをご覧ください。


> [AZURE.VIDEO azurecon-2015-get-started-with-azure-sql-database]


## ダウンタイムなしでのパフォーマンスの調整とスケール
SQL データベースは Basic、Standard、および Premium で利用可能な *サービス階層*します。 各サービス階層では [さまざまなレベルのパフォーマンスと機能](sql-database-service-tiers.md) ライトウェイト ヘビー級のデータベースのワークロードをサポートするためにします。 小さなデータベースで初めてのアプリをビルドするには、月、数ドルの [サービス層を変更する](sql-database-scale-up.md) 手動またはプログラムによっていつでも、アプリとはバイラル世界中で、アプリや、顧客にダウンタイムなし。

特に使用パターンが比較的予測可能である場合、多くのビジネスとアプリについては、データベースを作成し、要求に応じて単一のデータベースのパフォーマンスを調整する能力は十分です。 しかし、使用パターンが予測できない場合、コストおよびビジネス モデルを管理するのが難しくなる可能性があります。 

[エラスティック データベース プール](sql-database-elastic-pool.md) SQL データベースでこの問題が解決します。 概念は単純です。 プールにパフォーマンスを割り当てたら、単一のデータベースのパフォーマンスに対してではなく、プールの全体的なパフォーマンスに対して支払いを行います。 データベースのパフォーマンスを高くしたり低くしたりして調整する必要はありません。 プール内のデータベースと呼ばれる *弾力性データベース*, 、自動的にスケール アップおよび下矢印、要求を満たします。 エラスティック データベースでの使用はプールの上限を超えることはありません。したがってデータベースの使用状況が予測できなくても、コストが予測可能なことに変わりはありません。 詳細は、 [を追加し、プールにデータベースを削除する](sql-database-elastic-pool-portal.md), 、すべて管理するコスト内で何千もに、いくつかのデータベースからアプリをスケーリングします。

単一のデータベースまたはエラスティック データベースのどちらを使用したとしても、それに固定されることはありません。 単一のデータベースをエラスティック データベース プールと組み合わせ、単一のデータベースとプールのサービス階層を変更することで、革新的なデザインを作成することができます。 さらに、Azure の強力さと幅広さを利用して、さまざまな Azure サービスを SQL Database とうまく組み合わせることにより、独自の最新アプリ設計のニーズを満たし、コストとリソースの効率性を向上させ、新たなビジネス チャンスを開くことができます。

しかし、データベースとデータベース プールの相対的なパフォーマンスはどのようにして比較できますか? パフォーマンスの高低を調整するとき、何に基づいて右クリックでストップをかけますか? 答えは、単一のデータベースのデータベース トランザクション ユニット (DTU) とエラスティック データベースとデータベース プールのエラスティック DTU (eDTU) です。

## DTU について

[AZURE.INCLUDE [SQL DB DTU description](../../includes/sql-database-understanding-dtus.md)]

## アプリケーションとビジネスの継続的な稼働

Azure の業界のトップの 99.99% の可用性のサービス レベル アグリーメント [(SLA)](http://azure.microsoft.com/support/legal/sla/), 、Microsoft によって管理されるデータ センターのグローバル ネットワークで、24/7 を実行するアプリを使用します。 すべての SQL Database で、組み込みのデータ保護やフォールト トレランスを利用できます。これらは、本来なら自身で設計、購入、構築、および管理する必要があります。 それでも、ビジネスの要求に応じて、災害やエラーなどが発生した場合にアプリケーションおよびビジネスが迅速に回復するように、追加の保護レイヤーを必要とする場合があります。 各サービス階層には、SQL Database を起動して使用できるようにするためのさまざまな機能メニューが用意されています。 ポイントインタイム リストアを使用すると、データベースを 35 日間分遡って以前の状態に戻すことができます。 さらに、データベースをホストしているデータ センターで障害が発生した場合は、別のリージョンにあるデータベース レプリカにフェールオーバーことができます。 あるいは、レプリカを使用して、アップグレードまたは異なる地域への再配置を行うことができます。

![SQL Database の geo レプリケーション](./media/sql-database-technical-overview/azure_sqldb_map.png)


参照してください [ビジネス継続性](sql-database-business-continuity.md) 詳細については、さまざまなビジネス継続性に関する特徴別のサービス層で使用します。

## データのセキュリティ保護
SQL Server は、SQL データベースはデータを保護する、アクティビティを監視するためのアクセスを制限する機能を備えたに沿った動作する信頼性の高いデータのセキュリティの伝統です。 参照してください [SQL データベースを保護する](sql-database-security.md) の SQL データベースにあるセキュリティ オプションについて簡単に説明します。 参照してください、 [SQL Server データベース エンジンと SQL データベースのセキュリティ センター](https://msdn.microsoft.com/library/bb510589) セキュリティ機能のより包括的に表示します。 アクセス、 [Azure のトラスト センター](http://azure.microsoft.com/support/trust-center/security/) Azure のプラットフォームのセキュリティに関する情報。

## 次のステップ
ここでは、SQL Database を紹介し、「SQL Database とは」という問いに対する答えを示しました、次のステップは以下のとおりです。

- 参照してください、 [料金のページ](http://azure.microsoft.com/pricing/details/sql-database/) 1 つのデータベースと弾力性データベースの料金計算します。

- 最初に [最初のデータベースを作成する](sql-database-get-started.md)です。 初めてのアプリをビルド [c#](sql-database-connect-query.md), 、[Java](sql-database-develop-java-simple-windows.md), 、[Node.js](sql-database-develop-nodejs-simple-windows.md), 、[PHP](sql-database-develop-php-retry-windows.md), 、[Python](sql-database-develop-python-simple-windows.md), 、または [Ruby](sql-database-develop-ruby-simple-linux)します。 


