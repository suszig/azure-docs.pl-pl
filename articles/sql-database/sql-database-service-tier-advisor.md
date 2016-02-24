<properties 
   pageTitle="Azure SQL Database の価格レベルの提案" 
   description="Azure ポータルで価格レベルを変更するとき、価格レベルの提案により、既存の Azure SQL Database のワークロードを実行するのに最適なレベルが提案されます。 価格レベルは、SQL Database のサービス レベルとパフォーマンス レベルを説明します。" 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="12/01/2015"
   ms.author="sstein"/>

# SQL Database の価格レベルの提案

 既存の Azure SQL Database のワークロードを実行するのに最適な価格レベルとパフォーマンス レベルを提案する価格レベルの提案が用意されています。

> [AZURE.NOTE] 価格レベルの提案がのみ対象 Web と Business データベースとエラスティック データベース プール--でのみ使用可能、 [Azure ポータル](https://portal.azure.com/)します。


価格レベルの提案は、次のタスクの実行中に取得します。

- [SQL Database のサービス階層とパフォーマンス レベル (価格レベル) を変更する](sql-database-scale-up.md)
- [Azure SQL サーバーを V12 にアップグレードする](sql-database-v12-upgrade.md)
- データベースができる場合は、V12 サーバー - を参照 [弾力性データベース プールが役立つ](sql-database-elastic-pool-portal.md#recommended-elastic-database-pools), 、サーバー ブレード推奨されるプールを示すメッセージが表示されます。 メッセージをクリックすると、推奨されるプールが作成されます。
- [エラスティック データベース プールの作成](sql-database-elastic-pool.md#elastic-database-pool-pricing-tier-recommendations)





## 概要

SQL Database サービスでは、SQL Database の過去のリソースの使用状況を評価して、現在のパフォーマンス要件と機能要件が分析されます。 データベースのサイズに基づいており、有効になっているさらに、最小限許容可能なサービス階層が決定されます [ビジネス継続性](sql-database-business-continuity.md) 機能します。 

この情報を分析することにより、データベースの一般的なワークロードを実行し、現在の機能セットを維持するために最適なサービス階層とパフォーマンス レベルが提案されます。

- 過去 15 ～ 30 日間の履歴データ (リソースの使用状況、データベース サイズ、データベース アクティビティ) が検査され、使用されたリソースの量と、現在利用可能なサービス階層とパフォーマンス レベルの実際の制限とが比較されます。
- データは 15 秒間隔で分析されます。それぞれの間隔の結果セットは、その結果セットのワークロードを処理するために最適な既存のサービス階層とパフォーマンス レベルに分類されます。
- これらの 15 秒間隔のサンプルはより大きな 15 ～ 30 日間隔の分析に集計され、履歴のワークロードの 95% を最適に処理できるサービス階層とパフォーマンス レベルが提案されます。

### 推奨事項

データベースの使用量に基づく提案は、現在次の 2 つのカテゴリに分類されます。


| 推奨 | 説明 |
| :--- | :--- |
| アップグレード | 新しいレベルにアップグレードします。 |
| 使用不可 | データベースは、最小のワークロードまたは約 14 日間のアクティビティを必要とします。 有効な提案を行ううえで十分なデータがありません。 |

## 価格レベルの提案を受ける

既存の Web または Business データベースを選択し、[価格レベルの提案を取得、 **価格レベル** を並べて表示します。 (価格レベルの提案も利用可能な場合に、 [V12 にアップグレードの Azure SQL server](sql-database-v12-upgrade.md).)

1. [Azure ポータル](https://portal.azure.com/)にサインインします。
2. クリックして **参照** > **SQL データベース**します。
4.  **SQL データベース** ブレードで、データベースに対する推奨設定を表示する] をクリックします。

    ![データベースの選択][1]

5. [データベース] ブレードで、選択、 **価格レベル** を並べて表示します。

    ![価格レベル][2]


7. クリックすると、 **価格レベル** タイルに表示されます、 **価格レベルをお勧め** ブレードで、提案されたレベルをクリックして、クリックして、 **選択** ] ボタンをクリックすると、そのレベルに変更します。

    ![プレビューにサインアップする][4]

8. 必要に応じて、クリックして **使用状況の詳細を表示** を開くには、 **価格レベルの推奨事項の詳細** ブレードで、データベースで、現在と推奨される階層、および過去のリソース利用状況の分析グラフ間での機能の比較に対して提案されるレベルを表示できます。

    ![プレビューにサインアップする][5]



## 概要

価格レベルの提案では、自動的に各 SQL データベースの製品利用統計情報データが収集され、データベースの実際のパフォーマンス ニーズと機能要件に基づいてサービス階層とパフォーマンス レベルの最適な組み合わせが提案されます。 クリックして、 **価格レベル** 価格レベルの提案を表示するデータベース ブレードで並べて表示します。



## 次のステップ

データベースの内容によっては、アップグレードまたはダウングレードをすぐに実行することはできません。 ポータルでは、通知が、データベースの新しい階層に移行として提供されますか、クエリを実行して、アップグレードの状態を監視することができます、 [sys.dm_operation_status (Azure SQL データベース)](https://msdn.microsoft.com/library/dn270022.aspx) SQL データベース サーバーの master データベースで表示します。


<!--Image references-->
[1]: ./media/sql-database-service-tier-advisor/select-database.png
[2]: ./media/sql-database-service-tier-advisor/pricing-tier.png
[3]: ./media/sql-database-service-tier-advisor/preview-sign-up.png
[4]: ./media/sql-database-service-tier-advisor/choose-pricing-tier.png
[5]: ./media/sql-database-service-tier-advisor/usage-details.png


 
