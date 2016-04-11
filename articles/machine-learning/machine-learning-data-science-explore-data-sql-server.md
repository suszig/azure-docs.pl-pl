<properties 
    pageTitle="Azure の SQL Server Virtual Machine に格納されているデータを探索する | Microsoft Azure" 
    description="Azure の SQL Server VM に格納されているデータを探索する方法について説明します。" 
    services="machine-learning" 
    documentationCenter="" 
    authors="bradsev" 
    manager="paulettm" 
    editor="cgronlun" />

<tags 
    ms.service="machine-learning" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2015" 
    ms.author="fashah;garye;bradsev" /> 

#Azure の SQL Server Virtual Machine に格納されているデータを探索する

これは、 **メニュー** ツールを使用して、記憶域のさまざまな環境からデータを探索する方法を説明するトピックへのリンク。 このタスクは、Cortana Analytics Process (CAP) の 1 ステップです。

[AZURE.INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

##はじめに

このドキュメントでは、Azure の SQL Server VM に格納されているデータを探索する方法について説明します。 これは、SQL を使用してデータを処理するか、Python などのプログラミング言語を使用して実行できます。


> [AZURE.NOTE] このドキュメントのサンプル SQL ステートメントでは、データが SQL Server であると仮定します。 存在しない場合は、クラウド データ サイエンス プロセス マップを参照して、SQL Server へデータを移動する方法を確認してください。



## <a name="sql-dataexploration"></a>SQL スクリプトを使用して SQL データを探索する

SQL Server のデータ ストアの探索に使用できるいくつかのサンプル SQL スクリプトを次に示します。

1. 1 日ごとの所見の数を取得する

    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 

2. カテゴリ列内のレベルを取得する

    `select  distinct <column_name> from <databasename>`

3. 2 つのカテゴリ列の組み合わせ内のレベルの数を取得する 

    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`

4. 数値型列の分布を取得する

    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [AZURE.NOTE] 使用できる実用的な例、 [NYC タクシー データセット](http://www.andresmh.com/nyctaxitrips/) 」というタイトルの IPNB を参照して [NYC データの処理 IPython Notebook と SQL Server を使用して](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) エンド ツー エンド チュートリアルについては、です。

##<a name="python"></a>Python を使用して SQL データを探索する

説明に従って、Python を使用して Azure blob でデータの処理と似ています Python を使用してデータを探索し、データが SQL Server の特徴を生成する [Azure Blob データを処理するデータ サイエンス環境で](machine-learning-data-science-process-data-blob.md)します。 データは、データベースから pandas データ フレームに読み込む必要があります。その後、さらに処理することができます。 このセクションでは、データベースに接続して、データ フレームにデータを読み込むプロセスについて記載します。

次の接続文字列形式を使用して pyodbc を使用し Python から SQL Server データベースに接続することができます (サーバー名、データベース名、ユーザー名およびパスワードは使用する特定の値に置き換えてください)。

    #Set up the SQL Azure connection
    import pyodbc   
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

 [Pandas ライブラリ](http://pandas.pydata.org/) Python では、データを操作するため、Python プログラミングのデータ構造とデータ解析ツールの豊富なセットを提供します。 次のコードは、SQL Server データベースから返される結果を Pandas データ フレームに読み取ります。

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

トピックで説明されているように Pandas データ フレームを操作できるようになりました [Azure Blob データを処理するデータ サイエンス環境で](machine-learning-data-science-process-data-blob.md)します。

## Cortana Analytics Process の活用例

公開されているデータセットを使用して、Cortana Analytics プロセスのチュートリアルの例は、次を参照してください。 [の動作の Cortana Analytics プロセス: SQL Server を使用して](machine-learning-data-science-process-sql-walkthrough.md)します。

 

