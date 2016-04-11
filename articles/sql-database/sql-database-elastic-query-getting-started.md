<properties
    pageTitle="シャーディング (水平的パーティション分割) のためのエラスティック クエリの概要 | Microsoft Azure"
    description="データベース間データベース クエリを使用する方法"
    services="sql-database"
    documentationCenter=""  
    manager="jeffreyg"
    authors="sidneyh"/>

<tags
    ms.service="sql-database"
    ms.workload="sql-database"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/15/2015"
    ms.author="SilviaDoomra" />

# シャーディング (水平的パーティション分割) のためのエラスティック クエリの概要

Azure SQL Database 用の エラスティック データベース クエリ (プレビュー) を使用すると、1 つの接続ポイントで複数のデータベースにまたがる T-SQL クエリを実行することができます。 エラスティック データベース クエリ機能の詳細については、次を参照してください、 [機能の概要ページ](sql-database-elastic-query-overview.md)します。

このトピックで使用したサンプルを拡張する [エラスティック データベース ツールの概要](sql-database-elastic-scale-get-started.md)します。 終了すると、多くの関連するデータベースにまたがるクエリを実行するために Azure SQL Database を構成および使用する方法を習得できます。
## 前提条件

ダウンロードし、実行、 [エラスティック データベース ツールのサンプルの概要](sql-database-elastic-scale-get-started.md)します。

## サンプル アプリケーションを使用してシャード マップ マネージャーを作成する

ここでは、シャード マップ マネージャーといくつかのシャードを作成し、シャードにデータを挿入します。 シャードがすでにセットアップされ、シャード データが取り込まれている場合は、以下の手順を省略して、次のセクションに進むことができます。

1. ビルドおよび実行する、 **エラスティック データベース ツールの概要** サンプル アプリケーションです。 手順の手順で 7 まで [をダウンロードして、サンプル アプリケーションを実行](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools)します。 手順 7 を終了すると、次のコマンド プロンプトが表示されます。

    ![コマンド プロンプト][1]

2.  コマンド ウィンドウに「1」のように入力し、キーを押して **Enter**します。 シャード マップ マネージャーが作成され、2 つのシャードがサーバーに追加されます。 入力「3」し、キーを押します **Enter**; 4 回の操作を繰り返します。 これにより、サンプルのデータ行がシャードに挿入されます。
3.   [Azure ポータル](https://portal.azure.com) 、v12 サーバーに次の 3 つの新しいデータベースを表示する必要があります。

    ![Visual Studio の確認][2]

    この時点で、複数のデータベースにまたがるクエリは、エラスティック データベース クライアント ライブラリを介してサポートされます。 たとえば、コマンド ウィンドウのオプション 4 を使用するとします。 マルチ シャード クエリの結果は常に、 **UNION ALL** すべてのシャードからの結果。

    次のセクションでは、複数のシャードにまたがって実行されるデータの高度なクエリをサポートするサンプル データベース エンドポイントを作成します。

## エラスティック クエリ データベースを作成する

1. 開いている、 [Azure ポータル](https://portal.azure.com) してログインします。
2. シャード セットアップと同じサーバーに新しい Azure SQL Database を作成します。 このデータベースに "ElasticDBQuery" という名前を付けます。 価格レベルについては、プレミアム プランのいずれかを選択する必要があります。 エラスティック データベース クエリは現在、プレミアム レベルでのみ使用できます。

    ![Azure ポータルと価格レベル][3]

    注: 既存のプレミアム データベースを使用することができます。 そうする場合、その既存のプレミアム データベースを、クエリの実行対象とするシャードに含めることはできません。 このデータベースは、エラスティック データベース クエリのメタデータ オブジェクトを作成するために使用されます。


## データベース オブジェクトを作成する

### データベース スコープのマスター キーと資格情報

これらを使用し、次の手順に従ってシャード マップ マネージャーとシャードに接続します。

1. SQL Server Management Studio または Visual Studio の SQL Server Data Tools を開きます。
2. ElasticDBQuery データベースに接続し、次の T-SQL コマンドを実行します。

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    "username"、"password"はの手順 6 で使用するログイン情報と同じである必要があります [をダウンロードし、サンプル アプリケーションを実行する](sql-database-elastic-scale-get-started.md#Getting-started-with-elastic-database-tools) で [エラスティック データベース ツールの概要](sql-database-elastic-scale-get-started.md)します。

### 外部データ ソース

外部データ ソースを作成するには、ElasticDBQuery データベースで、次のコマンドを実行します。

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
      SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 エラスティック データベース ツールのサンプルを使用してシャード マップとシャード マップ マネージャーを作成した場合、"CustomerIDShardMap" はシャード マップの名前となります。 ただし、このサンプルでカスタム セットアップを使用した場合は、アプリケーションで選択するシャード マップ名となります。

### 外部テーブル

シャード上の Customers テーブルと一致する外部テーブルを作成するには、ElasticDBQuery データベースに対して次のコマンドを実行します。

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## サンプルのエラスティック データベース T-SQL クエリを実行する

外部データ ソースと外部テーブルを定義すると、外部テーブルに対して完全に T-SQL を使用できるようになります。

ElasticDBQuery データベースでこのクエリを実行します。

    select count(CustomerId) from [dbo].[Customers]

クエリはすべてのシャードからの結果を集計し、次のように出力を表示します。

![出力の詳細][4]

## エラスティック データベース クエリの結果を Excel にインポートする

 クエリの結果は Excel ファイルにインポートすることができます。

1. Excel 2013 を起動します。
2.  移動し、 **データ** リボンです。
3.  クリックして **その他のソース** ] をクリック **SQL Server から**します。

    ![他のソースから Excel へのインポート][5]
4.   **データ接続ウィザード** サーバー名とログインの資格情報を入力します。 クリックして **次**します。
5.  ダイアログ ボックスで **データを含むデータベースを選択して**, を選択、 **ElasticDBQuery** データベースです。
6.  選択、 **顧客** リスト ビューでテーブルを **次**します。 クリックして **完了**します。
7.   **データのインポート** フォームの **ブックでこのデータを表示する方法を選択**, [ **テーブル** ] をクリック **[ok]**します。

すべての行を **顧客** さまざまなシャードに格納されているテーブルが Excel シートを作成します。

## 次のステップ
これで Excel の強力なデータ視覚化機能を使用できるようになりました。 サーバー名、データベース名、および資格情報が含まれる接続文字列を使用して、BI とデータ統合ツールをエラスティック クエリ データベースに接続することができます。 ご使用のツールのデータ ソースとして SQL Server がサポートされていることを確認してください。 エラスティック クエリ データベースおよび外部テーブル (ツールを使用して接続するその他の SQL Server データベースおよび SQL Server テーブルのような) を参照できます。

### コスト
Elastic Database クエリ機能を使用する場合に追加の料金は発生しません。 ただし、この時点で、この機能はエンド ポイントとして動作するプレミアム データベースでしか利用できません。ただし、シャードのサービス階層に制限はありません。

料金情報については、次を参照してください。 [SQL Database の料金詳細](http://azure.microsoft.com/pricing/details/sql-database/)します。


[AZURE.INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->


