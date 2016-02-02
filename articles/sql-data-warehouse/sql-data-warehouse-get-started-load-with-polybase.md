<properties
   pageTitle="SQL Data Warehouse の PolyBase チュートリアル | Microsoft Azure"
   description="データ ウェアハウジングのシナリオに沿って、PolyBase の概要と、その使用方法を学習します。"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sahaj08"
   manager="barbkess"
   editor="jrowlandjones"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="11/19/2015"
   ms.author="sahaj08;barbkess"/>



# SQL Data Warehouse で PolyBase によってデータを読み込む

> [AZURE.SELECTOR]
- [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
- [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
- [BCP](sql-data-warehouse-load-with-bcp.md)


このチュートリアルでは、AzCopy と PolyBase を使用して SQL Data Warehouse にデータを読み込む方法を説明します。 このチュートリアルを行うと、以下の作業方法がわかります。

- AzCopy を使用して Azure Blob Storage にデータをコピーする
- 外部データを定義するデータベース オブジェクトを作成する
- データを読み込む T-SQL クエリを実行する

>[AZURE.VIDEO loading-data-with-polybase-in-azure-sql-data-warehouse]

## 前提条件

このチュートリアルを進めるには、次のものが必要です。

- SQL Data Warehouse データベース
- 種類が Standard ローカル冗長ストレージ (Standard-LRS)、Standard geo 冗長ストレージ (Standard-GRS)、または Standard 読み取りアクセス geo 冗長ストレージ (Standard-RAGRS) である Azure ストレージ アカウント
- AzCopy コマンド ライン ユーティリティ ダウンロードし、インストール、 [最新バージョンの AzCopy][] Microsoft Azure ストレージ ツールと共にインストールされます。

    ![Azure Storage Tools](./media/sql-data-warehouse-get-started-load-with-polybase/install-azcopy.png)


## 手順 1: Azure Blob Storage にサンプル データを追加する

データを読み込むには、Azure Blob Storage にサンプル データを配置する必要があります。 この手順では、Azure Storage BLOB にサンプル データを設定します。 その後、PolyBase を使用して、このサンプル データを SQL Data Warehouse データベースに読み込みます。

### A.サンプル テキスト ファイルを準備する

サンプル テキスト ファイルを準備するには:

1. メモ帳を開き、データの次の行を新しいファイルにコピーします。 これをローカル一時ディレクトリに %temp%\DimDate2.txt として保存します。

    ```
    20150301,1,3
    20150501,2,4
    20151001,4,2
    20150201,1,3
    20151201,4,2
    20150801,3,1
    20150601,2,4
    20151101,4,2
    20150401,2,4
    20150701,3,1
    20150901,3,1
    20150101,1,3
    ```

### B.BLOB サービス エンドポイントを検索する

BLOB サービス エンドポイントを検索するには:

1. Azure クラシック ポータルで、**[参照]**、**[ストレージ アカウント]** の順に選択します。
2. 使用するストレージ アカウントをクリックします。
3. ストレージ アカウントのブレードで、BLOB をクリックします。

    ![Click Blobs](./media/sql-data-warehouse-get-started-load-with-polybase/click-blobs.png)

1. 後の操作のために、BLOB サービス エンドポイントの URL を保存しておきます。

    ![Blob service endpoint](./media/sql-data-warehouse-get-started-load-with-polybase/blob-service.png)

### C.Azure ストレージ キーを検索する

Azure ストレージ キーを検索するには:

1. ホーム画面で、**[参照]**、**[ストレージ アカウント]** の順に選択します。
2. 使用するストレージ アカウントをクリックします。
3. **[すべての設定]**、**[アクセス キー]** の順に選択します。
4. いずれかのアクセス キーをクリップボードにコピーするために、コピー ボックスをクリックします。

    ![Copy Azure storage key](./media/sql-data-warehouse-get-started-load-with-polybase/access-key.png)

### D.サンプル ファイルを Azure Blob Storage にコピーする

Azure Blob Storage にデータをコピーするには:

1. コマンド プロンプトを開き、ディレクトリを AzCopy インストール ディレクトリに変更します。 このコマンドによって、64 ビットの Windows クライアント上の既定のインストール ディレクトリに移動します。

    ```
    cd /d "%ProgramFiles(x86)%\Microsoft SDKs\Azure\AzCopy"
    ```

1. 次のコマンドを実行してファイルをアップロードします。Blob サービス エンドポイントの URL を指定 <blob service endpoint URL> と Azure ストレージ アカウントのキーを <azure_storage_account_key>します。

    ```
    .\AzCopy.exe /Source:C:\Temp\ /Dest:<blob service endpoint URL> /datacontainer/datedimension/ /DestKey:<azure_storage_account_key> /Pattern:DimDate2.txt
    ```

関連項目 [AzCopy コマンド ライン ユーティリティの概要][]します。

### E.Blob Storage コンテナーを調べる

Blob Storage にアップロードしたファイルを表示するには:

1. BLOB サービス ブレードに戻ります。
2. [コンテナー] の下の **[データコンテナー]** をダブルクリックします。
3. データへのパスをたどるために **datedimension** フォルダーをクリックすると、アップロードした **DimDate2.txt** ファイルが表示されます。
4. プロパティを表示するには、**DimDate2.txt** をクリックします。
5. BLOB プロパティ ブレードでは、ファイルをダウンロードまたは削除することができます。

    ![View Azure storage blob](./media/sql-data-warehouse-get-started-load-with-polybase/view-blob.png)


## 手順 2: サンプル データ用の外部テーブルを作成する

このセクションでは、サンプル データを定義する外部テーブルを作成します。

PolyBase では、外部テーブルを使用して、Azure Blob Storage または Hadoop 内のデータにアクセスします。 データは SQL Data Warehouse 内に保存されないため、PolyBase は、データベース スコープの資格情報を使用して、外部データへの認証を処理します。

この手順の例では、以下の Transact-SQL ステートメントを使用して、外部テーブルを作成します。

- [Create Master Key (TRANSACT-SQL)][] スコープ資格情報、データベースのシークレットを暗号化します。
- [作成データベース スコープ資格情報 (TRANSACT-SQL)][] 、Azure ストレージ アカウントの認証情報を指定します。
- [外部データ ソースの作成 (TRANSACT-SQL)][] Azure blob ストレージの場所を指定します。
- [Create の External File Format (TRANSACT-SQL)][] 、データの形式を指定します。
- [Create External Table (TRANSACT-SQL)][] テーブルの定義とデータの場所を指定します。

SQL Data Warehouse データベースに対して、このクエリを実行します。 Azure Blob Storage 内の DimDate2.txt サンプル データを指す dbo スキーマに、DimDate2External という名前の外部テーブルが作成されます。


```
-- A: Create a master key.
-- Only necessary if one does not already exist.
-- Required to encrypt the credential secret in the next step.

CREATE MASTER KEY;


-- B: Create a database scoped credential
-- Provide your Azure storage account key. The identity is associated with the credential. -- It is not used for authentication to Azure storage.

CREATE DATABASE SCOPED CREDENTIAL AzureStorageCredential 
WITH 
    IDENTITY = 'user', 
    SECRET = '<azure_storage_account_key>'
;


-- C: Create an external data source
-- Specify the blob service endpoint and the name of the database-scoped credential.

CREATE EXTERNAL DATA SOURCE AzureStorage 
WITH (  
    TYPE = Hadoop, 
    LOCATION = 'wasbs://datacontainer@pbdemostorage.blob.core.windows.net',
    CREDENTIAL = AzureStorageCredential
); 


-- D: Create an external file format
-- Specify the way the sample data is formatted in the Azure storage blobs.

CREATE EXTERNAL FILE FORMAT TextFile 
WITH (
    FORMAT_TYPE = DelimitedText, 
    FORMAT_OPTIONS (FIELD_TERMINATOR = ',')
);


-- E: Create the external table
-- Specify the fields and data types for the table. This needs to match the data
-- in the sample file. Also specify the path to the data from the root directory
-- of the data source.

CREATE EXTERNAL TABLE dbo.DimDate2External (
    DateId INT NOT NULL, 
    CalendarQuarter TINYINT NOT NULL, 
    FiscalQuarter TINYINT NOT NULL
)
WITH (
    LOCATION='datedimension/', 
    DATA_SOURCE=AzureStorage, 
    FILE_FORMAT=TextFile
);


-- Run a PolyBase query to verify the external table

SELECT count(*) FROM dbo.DimDate2External;
```

## 手順 4: SQL Data Warehouse にデータを読み込む

外部テーブルが作成されたので、新しいテーブルにデータを読み込むか、既存のテーブルに挿入することができます。

- 新しいテーブルにデータを読み込む、実行、 [CREATE TABLE AS の SELECT (TRANSACT-SQL)][] ステートメントです。 新しいテーブルには、クエリで指定された列があります。 列のデータ型は、外部テーブル定義内のデータ型と一致します。
- 既存のテーブルにデータを読み込む、使用、 [を挿入しています.SELECT (TRANSACT-SQL)][] ステートメントです。

```
-- Load the data from Azure blob storage to SQL Data Warehouse

CREATE TABLE dbo.DimDate2
WITH 
(   
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = ROUND_ROBIN
)
AS 
SELECT * FROM [dbo].[DimDate2External];
```


Visual Studio の SQL Server オブジェクト エクスプローラーで、外部ファイル形式、外部データ ソース、および DimDate2External テーブルを表示できます。

![View external table](./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png)

## 手順 5: 新たに読み込んだデータの統計を作成する

SQL Data Warehouse は、統計の自動作成または自動更新を行いません。 そのため、高いクエリ パフォーマンスを実現するには、最初の読み込み後に各テーブルの各列についての統計を作成することが重要です。 また、データが大幅に変更された後で統計を更新することも重要です。

この例では、新しい DimDate2External テーブルについての単一列統計を作成します。

```
CREATE STATISTICS [DateId] on [DimDate2] ([DateId]);
CREATE STATISTICS [CalendarQuarter] on [DimDate2] ([CalendarQuarter]);
create statistics [FiscalQuarter] on [DimDate2] ([FiscalQuarter]);
```

詳細については、次を参照してください。 [統計情報 []][]します。


## 次のステップ

参照してください、 [PolyBase ガイド][] PolyBase を使用するソリューションを開発する際に知っておくべきについてさらにします。





[1]: ./media/sql-data-warehouse-get-started-load-with-polybase/external-table.png 
[polybase in sql data warehouse tutorial]: sql-data-warehouse-load-with-polybase.md 
[load data with bcp]: sql-data-warehouse-load-with-bcp.md 
[load with polybase]: sql-data-warehouse-load-with-polybase.md 
[solution partners]: sql-data-warehouse-solution-partners.md 
[development overview]: sql-data-warehouse-overview-develop.md 
[statistics]: sql-data-warehouse-develop-statistics.md 
[polybase guide]: sql-data-warehouse-load-polybase-guide.md 
[getting started with the azcopy command-line utility]: ../storage/storage-use-azcopy.md 
[latest version of azcopy]: ../storage/storage-use-azcopy.md 
[supported source/sink]: https://msdn.microsoft.com/library/dn894007.aspx 
[copy activity]: https://msdn.microsoft.com/library/dn835035.aspx 
[sql server destination adapter]: https://msdn.microsoft.com/library/ms141095.aspx 
[ssis]: https://msdn.microsoft.com/library/ms141026.aspx 
[create external data source (transact-sql)]: https://msdn.microsoft.com/library/dn935022.aspx 
[create external file format (transact-sql)]: https://msdn.microsoft.com/library/dn935026.aspx 
[create external table (transact-sql)]: https://msdn.microsoft.com/library/dn935021.aspx 
[drop external data source (transact-sql)]: https://msdn.microsoft.com/library/mt146367.aspx 
[drop external file format (transact-sql)]: https://msdn.microsoft.com/library/mt146379.aspx 
[drop external table (transact-sql)]: https://msdn.microsoft.com/library/mt130698.aspx 
[create table as select (transact-sql)]: https://msdn.microsoft.com/library/mt204041.aspx 
[insert...select (transact-sql)]: https://msdn.microsoft.com/library/ms174335.aspx 
[create master key (transact-sql)]: https://msdn.microsoft.com/library/ms174382.aspx 
[create credential (transact-sql)]: https://msdn.microsoft.com/library/ms189522.aspx 
[create database scoped credential (transact-sql)]: https://msdn.microsoft.com/library/mt270260.aspx 
[drop credential (transact-sql)]: https://msdn.microsoft.com/library/ms189450.aspx 

