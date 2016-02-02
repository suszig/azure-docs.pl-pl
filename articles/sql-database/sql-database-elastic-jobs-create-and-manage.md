<properties
    pageTitle="作成して、弾力性データベース ジョブの管理 |以下の Azure"
    description="弾力性データベースのジョブの作成と管理について説明します。"
    services="sql-database"
    documentationCenter=""
    manager="jhubbard"
    authors="ddove"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/04/2015"
    ms.author="ddove; sidneyh"/>


# ポータル (プレビュー) を使用した SQL Database エラスティック ジョブの作成と管理

> [AZURE.SELECTOR]
- [Azure portal](sql-database-elastic-jobs-create-and-manage.md)
- [PowerShell](sql-database-elastic-jobs-powershell.md)



**エラスティック データベース ジョブ**を使用すると、スキーマの変更、資格情報の管理、参照データの更新、パフォーマンス データの収集、テナント (顧客) 製品利用統計情報の収集などの管理操作の実行が簡易になるので、データセット グループを簡単に、また確実に管理できるようになります。 現在、エラスティック データベース ジョブは、Azure ポータルまたは PowerShell コマンドレットから使用できます。 ただし、Azure ポータルの画面が短く内のすべてのデータベース間での実行に限定された機能、 [弾力性データベース プール (プレビュー)](sql-database-elastic-pool.md)します。 カスタム定義のコレクションやシャードを含むデータベースのグループに追加の機能とスクリプトの実行にアクセスする設定 (を使用して作成 [Elastic Database クライアント ライブラリ](sql-database-elastic-scale-introduction.md)), を参照してください [の作成と PowerShell を使用してジョブを管理する](sql-database-elastic-jobs-powershell.md)します。 ジョブの詳細については、次を参照してください。 [弾力性データベース ジョブの概要](sql-database-elastic-jobs-overview.md)します。

## 前提条件

* Azure サブスクリプション。 無料評価版サイトを参照してください。 [無料評価版の 1 か月](http://azure.microsoft.com/pricing/free-trial/)します。
* 弾力性データベース プール。 を参照してください [弾力性データベース プール](sql-database-elastic-pool.md)
* 弾力性データベース ジョブ サービス コンポーネントのインストール。 参照してください [弾力性データベース ジョブ サービスのインストール](sql-database-elastic-jobs-service-installation.md)します。

## ジョブの作成

1. 使用して、 [Azure ポータル](https://portal.azure.com), 、既存の弾力性データベース ジョブ プール] をクリックして **ジョブの作成**します。
2. ジョブ管理データベース (ジョブのメタデータ ストレージ) のデータベース管理者のユーザー名とパスワード (ジョブのインストール時に作成) を入力します。

    ![ジョブに名前を付け、コードに入力するか、貼り付け、][1]
2. **[ジョブの作成]** ブレードで、ジョブの名前を入力します。
3. ターゲット データベースに接続するために、スクリプトを正常に実行できるアクセス許可を持つユーザー名とパスワードを入力します。
4. T-SQL スクリプトを貼り付けるか、入力します。
5. **[保存]** をクリックし、次に **[実行]** をクリックします。

    ![Create jobs and run][5]

## べき等ジョブを実行します

一連のデータベースに対してスクリプトを実行する場合、スクリプトがべき等であることを確認する必要があります。 つまり、スクリプトは、前に不完全な状態で失敗した場合でも、複数回実行される必要があります。 たとえば、スクリプトが失敗すると、ジョブは成功するまで自動的に再試行されます (制限内、再試行ロジックが最終的に再試行を停止するまで)。 これを行うには、"IF EXISTS" 句を使用して、新しいオブジェクトを作成する前に、検出されたすべてのインスタンスを削除します。 例を次に示します。

    IF EXISTS (SELECT name FROM sys.indexes
            WHERE name = N'IX_ProductVendor_VendorID')
    DROP INDEX IX_ProductVendor_VendorID ON Purchasing.ProductVendor;
    GO
    CREATE INDEX IX_ProductVendor_VendorID
    ON Purchasing.ProductVendor (VendorID);

代わりに、新しいインスタンスを作成する前に、"IF NOT EXISTS" 句を使用します。

    IF NOT EXISTS (SELECT name FROM sys.tables WHERE name = 'TestTable')
    BEGIN
     CREATE TABLE TestTable(
      TestTableId INT PRIMARY KEY IDENTITY,
      InsertionTime DATETIME2
     );
    END
    GO
    
    INSERT INTO TestTable(InsertionTime) VALUES (sysutcdatetime());
    GO

このスクリプトは、以前に作成されたテーブルを更新します。

    IF NOT EXISTS (SELECT columns.name FROM sys.columns INNER JOIN sys.tables on columns.object_id = tables.object_id WHERE tables.name = 'TestTable' AND columns.name = 'AdditionalInformation')
    BEGIN
    
    ALTER TABLE TestTable
    
    ADD AdditionalInformation NVARCHAR(400);
    END
    GO
    
    INSERT INTO TestTable(InsertionTime, AdditionalInformation) VALUES (sysutcdatetime(), 'test');
    GO

## ジョブの状態を確認する

ジョブが開始されると、進行状況を確認できます。

1. 弾力性データベース ページ プールで、**[ジョブの管理]** をクリックします。

    ![[ジョブの管理] をクリックします。][2]

2. ジョブの名前 (a) をクリックします。 **[状態]** は、「完了」か「失敗」です。 ジョブの詳細は、作成と実行の日時とともに (b) に表示されます。 下記のリスト (c) は、各データベースに対するスクリプトの進行状況を日時の詳細とともに表示します。

    ![完成したジョブの確認][3]


## 失敗したジョブの確認

ジョブが失敗した場合、その実行ログを確認できます。 失敗したジョブの名前をクリックして詳細を表示します。

![失敗したジョブを確認する][4]


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]



[1]: ./media/sql-database-elastic-jobs-create-and-manage/screen-1.png 
[2]: ./media/sql-database-elastic-jobs-create-and-manage/click-manage-jobs.png 
[3]: ./media/sql-database-elastic-jobs-create-and-manage/running-jobs.png 
[4]: ./media/sql-database-elastic-jobs-create-and-manage/failed.png 
[5]: ./media/sql-database-elastic-jobs-create-and-manage/screen-2.png 

