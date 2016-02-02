<properties
    pageTitle="Azure SQL Database でデータベースのパフォーマンスのトラブルシューティングを行います。"
    description="データベース パフォーマンスのトラブルシューティングを行うための簡単な手順。"
    services="sql-database"
    documentationCenter=""
    authors="v-shysun"
    manager="msmets"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/17/2015"
    ms.author="v-shysun"/>


# Azure SQL Database を使用してデータベースのパフォーマンスのトラブルシューティングを行う

変更することができます、 [サービス層](sql-database-service-tiers.md) 単一データベースまたは増加の弾力性データベースの Edtu のプールのパフォーマンスが向上するには、いつでも、向上させ、まずクエリのパフォーマンスを最適化する機会を特定することがあります。 データベースのパフォーマンスが悪い一般的な理由は、インデックスの欠落や最適化が不十分なクエリです。

## データベースのパフォーマンスを評価し、チューニングする手順

1.  [Azure ポータル](https://portal.azure.com), 、クリックして **SQL データベース**, 、データベースを選択し、監視グラフを使用して、リソースの上限に近づいていますを検索します。 DTU 消費は、既定で表示されます。 **[編集]** をクリックして、表示される時間の範囲と値を変更します。
2.  使用して [Query Performance Insight](sql-database-query-performance.md) Dtu を使用してクエリを評価し、使用する [Index Advisor](sql-database-index-advisor.md) を推奨し、インデックスを作成します。
3.  動的管理ビュー (DMV)、拡張イベント (Xevent)、および SSMS のクエリ ストアを使用すると、リアルタイムでパフォーマンス パラメーターを取得できます。 参照してください、 [パフォーマンス ガイダンス トピック「](sql-database-performance-guidance.md) 詳細な監視とチューニングに関するヒント。

## その他のリソースを含むデータベースのパフォーマンスを改善する手順

1.  1 つのデータベースについては [サービス階層の変更](sql-database-scale-up.md) データベースのパフォーマンスを向上させるためにオンデマンドです。
2.  複数のデータベースには、使用を検討して [エラスティック データベース プール](sql-database-elastic-pool-guidance.md) リソースを自動的に拡張します。

パフォーマンスの問題が続く場合は、サポートに連絡してサポート ケースを登録してください。





