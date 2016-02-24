<properties
    pageTitle="Azure SQL Database によるバックアップのトラブルシューティングと復元"
    description="Azure SQL Database のバックアップおよびレプリカを使用した、クラウド データベースのエラーおよび障害からの回復方法について説明します。"
    services="sql-database"
    documentationCenter=""
    authors="dalechen"
    manager="msmets"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/10/2015"
    ms.author="daleche"/>

# 過去のある時点へのデータベースの復元、削除したデータベースの復元、またはデータ センターの障害からの回復

SQL データベースは、障害やユーザー エラーから回復できるように、データベースのレプリカを保持します。 使用可能なオプションは、データベースのサービス層と選択したオプションによって異なります。 参照してください、 [ビジネス継続性の概要](sql-database-business-continuity.md) 詳細と設計に関する考慮事項です。

##過去のある時点にデータベースを復元するには
1.   [Azure ポータル](https://azure.microsoft.com/), 、クリックして **SQL データベース**します。
2.  一覧から、データベースを選択し、クリックして **復元**します。
3.  データベースの新しい名前を入力、日付を選択およびクリックしてから、復元時間 **を作成します。**
4.  新しいデータベースを参照する場合、必要に応じてアプリの調整を行います。 参照してください [ユーザー エラーからデータベースを回復](sql-database-user-error-recovery.md)します。

##誤って削除したデータベースを復元するには
1.   [Azure ポータル](https://azure.microsoft.com/), 、クリックして **の SQL server**します。
2.  一覧からデータベースがホストされているサーバーを選択します。
3.  [サーバー] ブレードで、下へスクロールし、クリックして **削除済みデータベース**します。
4.  クリックして復元するデータベースを選択して **作成**します。
5.  新しいデータベースを参照する場合、必要に応じてアプリの調整を行います。 参照してください [ユーザー エラーからデータベースを回復](sql-database-user-error-recovery.md)します。

##地域のデータ センターの障害から回復するには
Standard および Premium のデータベースで geo レプリケートされたセカンダリを設定している場合は、そのセカンダリを使用して回復することができます。 これにより、データが失われる可能性の少ないデータベースの復元が可能になります。 参照してください [障害から Azure SQL データベースの回復](sql-database-disaster-recovery.md) 詳細です。

Azure では、別のリージョンでのすべてのデータベースのバックアップ (地理冗長バックアップ) も提供しています。 geo リストアと呼ばれるこれらのバックアップから新しいデータベースを作成できますが、この方法のみに頼ると、データが失われる可能性があります。

**geo リストアを使用してデータベースを回復するには：**

-  [Azure ポータル](https://azure.microsoft.com/), 、] をクリックして **新規**, 、] をクリックして **データとストレージ**, をクリックして **SQL データベース**, 、し、[ **バックアップ** 、データベースのソースとして。 参照してください [障害から Azure SQL データベースの回復](sql-database-disaster-recovery.md) 詳細です。

