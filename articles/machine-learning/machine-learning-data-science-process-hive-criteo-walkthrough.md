<properties 
    pageTitle="Cortana Analytics Process の活用: 1 TB Criteo データセットで HDInsight Hadoop クラスターを使用する | Microsoft Azure" 
    description="HDInsight Hadoop クラスターを用いたエンド ツー エンドのシナリオに Adva nced Analytics Process and Technology (ADAPT) を使用し、大量の (1 TB) 公開されている使用可能なデータセットを使用してモデルを構築してデプロイします。" 
    services="machine-learning,hdinsight" 
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
    ms.date="10/18/2015" 
    ms.author="ginathan;bradsev" /> 

# Cortana Analytics Process の活用 - 1 TB (テラバイト) データセットで Azure HDInsight Hadoop クラスターを使用する

このチュートリアルで紹介の Cortana Analytics プロセスを使用して、エンド ツー エンドでの [Azure HDInsight Hadoop クラスター](http://azure.microsoft.com/services/hdinsight/) 格納、探索、特徴エンジニア リング、およびのパブリックに利用可能ないずれかのサンプル データをダウン [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) データセット。 Azure Machine Learning を使い、このデータに基づいてバイナリ分類モデルを構築します。 また、これらのモデルのいずれかを Web サービスとして発行する方法も示します。

IPython Notebook を使用して、このチュートリアルで説明するタスクを実行することもできます。 この方法を試してみたいユーザーを参照してください、 [Hive ODBC の接続を使用した Criteo チュートリアル](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) トピックです。


## <a name="dataset"></a>Criteo データセットの説明

Criteo データは、gzip で圧縮された約 370 GB の TSV ファイル (非圧縮で ~1.3TB) のクリック予測データセットで、43 億以上のレコードが含まれています。 24 日間から取得されますによって提供されるデータ] をクリックして [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/)します。 データ サイエンティストが使いやすいように、実験のために公開されていたデータを解凍しました。

このデータセットの各レコードには、40 の列があります。 

- 最初の列は、ユーザーが追加をクリックする (値 1) またはクリックしない (値 0) ことを示すラベル列です。 
- 次の 13 の列は数値列です。 
- 最後の 26 の列はカテゴリ列です。 

列は匿名で、"Col1" (ラベル列) から "Col40" (最後のカテゴリ列) まで一連の列挙型名を使用します。            

次に、このデータセットから最初の 20 列の 2 つの観測 (行) を抜粋して示します。

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10   Col11   Col12   Col13   Col14   Col15           Col16           Col17           Col18           Col19       Col20   

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

不足値がこのデータセットの数値列とカテゴリ列の両方にあります。 不足値を処理するための単純なメソッドについて以下で説明します。 データの詳細については、Hive テーブルにデータを格納するときに以下で説明します。

**定義:** *クリックスルー率 (CTR):* これは、データ内のクリックの割合。 この Criteo データセットの CTR は、約 3.3% または 0.033 です。

## <a name="mltasks"></a>予測タスクの例
このチュートリアルでは、次の 2 つのサンプルの予測問題に対処します。

1. **二項分類**: が追加をユーザーがクリックしたかどうかを予測します。
    - クラス 0: クリックしない
    - クラス 1: クリックする

2. **回帰**: ユーザーの特徴から広告クリックの確率を予測します。


## <a name="setup"></a>データ サイエンス用に HDInsight Hadoop クラスターをセット アップする

**注:** これは、通常、 **Admin** タスクです。

HDInsight クラスターを使用して予測分析ソリューションを構築するために、3 つの手順で Azure のデータ サイエンス環境をセット アップします。

1. [ストレージ アカウントの作成](storage-whatis-account.md): このストレージ アカウントは Azure Blob ストレージにデータを格納するために使用します。 ここには、HDInsight クラスターで使用するデータが格納されます。

2. [データ サイエンス用の Azure HDInsight Hadoop クラスターのカスタマイズ](machine-learning-data-science-customize-hadoop-cluster.md): この手順では、すべてのノードにインストールされている 64 ビット Anaconda Python 2.7 を使用、Azure HDInsight Hadoop クラスターを作成します。 HDInsight クラスターをカスタマイズするときに実行する、2 つの重要な手順があります (このトピックで説明します)。

    * 作成時に、手順 1. で作成したストレージ アカウントを HDInsight クラスターにリンクする必要があります。 このストレージ アカウントは、クラスター内で処理可能なデータにアクセスするために使用されます。

    * 作成したら、クラスターのヘッド ノードへのリモート アクセスを有効にする必要があります。 ここで指定するリモート アクセス資格情報を覚えておいてください (作成時にクラスターに指定したものとは異なります)。以下で必要になります。

3. [Azure ML ワークスペースを作成する](machine-learning-create-workspace.md): とダウン サンプリング、HDInsight クラスターで初期データの探索後は、機械学習モデルを構築するためこの Azure Machine Learning ワークスペースを使用します。

## <a name="getdata"></a>公開されているソースからデータを取得して使用する

 [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) リンクをクリックし、使用、条件を受信し、名前を指定するデータセットにアクセスすることができます。 この画面のスナップショットを以下に示します。

![Criteo 条項に同意する](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

をクリックして **Continue to Download** 詳しく、データセットとその可用性に関するします。

パブリックなデータが存在する [Azure blob ストレージ](storage-dotnet-how-to-use-blobs.md) 場所: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/です。 "wasb" は、Azure BLOB ストレージの場所を表します。 

1. このパブリック BLOB ストレージ内のデータは、解凍されたデータの 3 つのサブフォルダーで構成されています。
        
    1. サブ フォルダー *raw/数/* day\_20 に day\_00 からのデータの最初の 21 日が含まれています
    2. サブ フォルダー *生/トレーニング/* day\_21、データの 1 日で構成されています
    3. サブ フォルダー *生/テスト/* 2 日間のデータ、day\_22 および day\_23 で構成されています

2. 方のための生の gzip データから開始する、これらも利用可能なフォルダーにはメイン *生/* day_nn.gz (NN は 00 ~ 23) として。

ローカルのダウンロードを必要としない別の手法でこのデータにアクセスして探索し、モデリングする方法については、Hive テーブルを作成するときに、このチュートリアルの後半で説明します。

## <a name="login"></a>クラスター ヘッドノードにログインする

クラスターのヘッドノードにログインを使用して、 [Azure Management](manage.windowsazure.com) ポータル クラスターを検索します。 左側の HDInsight の象のアイコンをクリックし、クラスターの名前をダブルクリックします。 移動し、 **構成** タブで、ページの下部にある [接続] アイコンをダブルクリックし、されたら、リモート アクセスの資格情報を入力します。 入力すると、クラスターのヘッドノードに移動します。 

クラスター ヘッドノードに初めてログインすると、通常、次のような画面が表示されます。

![クラスターにログインする](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)


左側に、"Hadoop コマンド ライン" が表示されます。これは、データ探索用のマイクロソフトの主力製品です。 2 つの便利な URL "Hadoop Yarn のステータス" と "Hadoop 名前ノード" も表示されます。 Yarn ステータスの URL はジョブの進行状況を示し、名前ノードの URL はクラスター構成の詳細を示します。 

これで、チュートリアルの最初の部分 (Hive を使用したデータ探索と Azure Machine Learning のデータの準備) を設定して開始する準備ができました、 

## <a name="hive-db-tables"></a>Hive データベースとテーブルを作成する

Criteo データセットの Hive テーブルを作成するには、開く、 ***Hadoop コマンド ライン*** ヘッド ノードのデスクトップでのコマンドを入力して Hive ディレクトリを入力し、

    cd %hive_home%\bin

**重要な注意事項**: **このチュートリアルでは、上記の Hive bin からすべての Hive コマンドを実行/ディレクトリ プロンプトです。これは自動的にすべてのパスの問題に対処します。用語「Hive ディレクトリ プロンプト」を使用して"Hive bin/ディレクトリ プロンプト"、「Hadoop コマンドライン」と同じ意味で。** 

**重要なメモ 2**: **次時は、常に Hive クエリを実行するには** 
        cd %hive_home%\bin
        hive

Hive REPL が "hive >" 記号と共に表示されたら、単にクエリをカット アンド ペーストして実行します。

以下のコードは、データベース "criteo" を作成した後、次の 4 つのテーブルを生成します。 


*  *カウントを生成するためのテーブル* day\_20 に日 day\_00 上に構築されました。 
*  *トレーニング データセットとして使用するためのテーブル* day\_21、上に構築し、 
* 2 つ *テスト データセットとして使用するテーブル* それぞれ day\_22 と day\_23 に基づいて構築されています。 

いずれかの日が休日であり、モデルがクリックスルー率から休日と休日以外の違いを検出できるかどうかを判断するため、テスト データセットは 2 つの異なるテーブルに分割します。

スクリプト [サンプル & #95";"ハイブ"&"を作成します。 #95 (& a) #95; criteo & #95; データベースと #95; と & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) 利便性の下に表示されます。

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

単に Azure BLOB ストレージ (wasb) の場所を指す場合、これらのテーブルはすべて外部にあることに注意してください。 

**任意の Hive クエリを実行するには 2 つの方法があります。**

1. **Hive REPL コマンドラインを使用して**: 最初に、"hive"コマンドを発行してコピーしして Hive REPL コマンドラインで上記のクエリを貼り付けます。 これを行うには、次のコマンドを実行します。

        cd %hive_home%\bin
        hive

    REPL コマンド ラインで、クエリをカット アンド ペースして実行します。

2. **クエリをファイルに保存し、コマンドを実行する**: 2 つ目は、.hql ファイルにクエリを保存する ([サンプル & #95";"ハイブ"&"#95; を作成 (& a) #95; criteo & #95; データベースと #95; と & #95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) およびクエリを実行するには、次のコマンドを発行します。

        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql


### データベースとテーブルの作成を確認する

次に、Hive bin/ ディレクトリ プロンプトから次のコマンドを発行してデータベースの作成を確認します。

        hive -e "show databases;"

次のように表示されます。

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

これは、新しいデータベース "criteo" の作成を確認します。

作成されたテーブルを表示するには、Hive bin/ ディレクトリ プロンプトから次のコマンドを発行します。

        hive -e "show tables in criteo;"

次の出力が表示されます。

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

##<a name="exploration"></a>Hive でデータを探索する

これで、Hive でいくつかの基本的なデータ探索を行う準備ができました。 まず、トレーニングとテストのデータ テーブルでサンプル数をカウントすることから始めます。

### トレーニングのサンプル数

内容 [サンプル & #95; hive と #95; カウント & #95; トレーニング & #95; テーブル & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) を以下に示します。

        SELECT COUNT(*) FROM criteo.criteo_train;

次が生成されます。

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

代わりに、Hive bin/ ディレクトリ プロンプトから次のコマンドを発行することもできます。

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### 2 つのテスト データセットのテスト サンプル数

2 つのテスト データセットのサンプル数をカウントします。 内容 [サンプル & #95; hive と #95; カウント & #95; criteo & #95; テストと #95; 日 & #95; 22 & #95; テーブル & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) 以下に示します。

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

次が生成されます。
    
        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

通常どおり、次のコマンドを発行して Hive bin/ ディレクトリ プロンプトからスクリプトを呼び出すこともできます。

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

最後に、day\_23 に基づくテスト データセットにあるテスト例の数を確認します。

これを行うコマンドは、上記と同様です (を参照してください [サンプル & #95; hive と #95; カウント & #95; criteo & #95; テストと #95; 日 & #95; 23 & #95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql))。

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

次のように表示されます。
    
        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### トレーニング データセット内のラベルの分布

対象はトレーニング データセット内のラベルの分布です。 内容を表示してこれを参照する [サンプル & #95; hive と #95; criteo & #95; ラベルと #95; 配布 & #95; トレーニング & #95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

ラベルの分布が生成されます。

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

正の値のラベルの割合が約 3.3% であることに注意してください (元のデータセットと一致します)。
        
### トレーニング データセット内の一部の数値変数のヒストグラム分布

Hive のネイティブ"histogram\_numeric"関数を使用して数値変数の分布がどのように知ることができます。 内容 [サンプル & #95; hive と #95; criteo & #95; ヒストグラム & #95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql) 以下に示します。

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM 
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

次のように生成されます。

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

Hive の LATERAL VIEW - explode の組み合わせは、通常のリストの代わりに、SQL に似た出力を生成する機能があります。 上の表では、最初の列は bin center に対応し、2 番目の列は bin frequency に対応しています。

### トレーニング データセット内の一部の数値変数の百分位を概算する

概算の百分位の計算も数値変数の対象です。 Hive のネイティブ"percentile\_approx"がこれを実行します。 内容 [サンプル & #95; hive と #95; criteo & #95。 概数 & #95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) は。

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

次が生成されます。

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

百分位の分布が任意の数値変数のヒストグラム分布に密接に関連していることがわかります。        

### トレーニング データセット内の一部のカテゴリ列の一意の値の数を検索する

データの探索を続けると、一部のカテゴリ列が一意の値の数を取得することがわかります。 内容を表示するのには、私たち [サンプル & #95; hive と #95; criteo & #95; 一意 & #95; 値 & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

次が生成されます。

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

Col15 は 19 M 個の一意の値を持つことに注意してください。 "one-hot エンコーディング" のような単純な手法を使用して、このような高次元カテゴリ変数をエンコードすることはできません。 具体的には、について説明しを示しますと呼ばれる強力で信頼性の高い方法 [カウントを使用した学習](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) この問題を効率的に取り組むためです。 

他のカテゴリ列の一意の値の数を確認すると、このサブ セクションが完了します。 内容 [サンプル & #95; hive と #95; criteo & #95; 一意 & #95; 値 & #95; 複数 & #95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) は。

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)), 
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

次が生成されます。 

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

ここでも、Col20 以外のすべての列が、多くの一意の値を持つことがわかります。 

### トレーニング データセット内のカテゴリ変数ペアの同時発生カウント

カテゴリ変数ペアの同時発生カウントも対象です。 これを判断するコードを使用 [サンプル & #95; hive と #95; criteo & #95; ペア (& a) #95; カテゴリ & #95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

発生カウントの順序を逆にして上位 15 を示します。 次のように表示されます。

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <a name="downsample"></a> Azure Machine Learning のデータセットのダウンサンプリング

データセットを探索し、この種類の任意の変数 (組み合わせを含む) の探索を行う方法を示すと、データ セットをダウンサンプリングして Azure Machine Learning にモデルを構築できるようになります。 重点を置く問題を思い出してください。属性のサンプル セット (Col2 ～ Col40 の特徴の値) がある場合、Col1 が 0 (クリックする) か、1 (クリックしない) かを予測します。 

トレーニングとテストのデータセットを元のサイズの 1% にダウンサンプリングするには、Hive のネイティブ RAND() 関数を使用します。 次のスクリプトでは、 [サンプル & #95; hive と #95; criteo & #95; 低解像度処理 & #95; トレーニング & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) トレーニング データセットに対して行われます。

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

次が生成されます。

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

スクリプト [サンプル & #95; hive と #95; criteo & #95; 低解像度処理 & #95; テストと #95; 日 & #95; 22 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) day\_22、テスト データには。

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

次が生成されます。

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


最後に、スクリプト [サンプル & #95; hive と #95; criteo & #95; 低解像度処理 & #95; テストと #95; 日 & #95; 23 & #95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) day\_23、テスト データには。

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

次が生成されます。

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

これで、ダウンサンプリングされたトレーニングとテストのデータセットを使用して Azure Machine Learning でモデルを構築する準備ができました。

Azure Machine Learning に移動する前に、カウント テーブルに関する最後の重要なコンポーネントがあります。 次のサブ セクションでは、これについて説明します。

##<a name="count"></a>カウント テーブルの簡単な説明

説明したように、いくつかのカテゴリ変数は非常に高次元です。 このチュートリアルでは強力な手法と呼ばれる [カウントを使用した学習](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) を効率的で信頼性の高い方法でこれらの変数をエンコードします。 この手法の詳細については、リンクをクリックしてください。 

**注:** でこのチュートリアルでは、高次元カテゴリ特徴のコンパクトな表現を生成するカウント テーブルの使用について説明します。 これは間違いなくカテゴリの特徴をエンコードする唯一の方法その他の方法の詳細については、関心のある方をチェック_アウトできます [1 つのホット-エンコード](http://en.wikipedia.org/wiki/One-hot) と [特徴ハッシュ](http://en.wikipedia.org/wiki/Feature_hashing)します。 

カウント データのカウント テーブルを作成するには、raw/count フォルダーのデータを使用します。 モデリング セクションでは、カテゴリ特徴のカウント テーブルを最初から構築する方法と、探査のために事前に構築されたカウント テーブルを使用する別の方法を示します。 以下では、"事前に構築されたカウント テーブル" に言及する場合は、提供されているカウント テーブルを使用することを意味します。 次に、これらのテーブルにアクセスする方法の詳細について説明します。

## <a name="aml"></a>Azure Machine Learning でモデルをビルドする

Azure Machine Learning のモデル構築プロセスは、次の手順を実行します。

1. [Azure Machine Learning に Hive テーブルからデータを取得する](#step1)
2. [実験を作成する: データを整理し、学習者を選択し、カウント テーブルを特徴付ける](#step2)
3. [モデルをトレーニングする](#step3)
4. [テスト データでモデルにスコアを付ける](#step4)
5. [モデルを評価する](#step5)
6. [使用する Web サービスとしてモデルを発行する](#step6)

これで、Azure Machine Learning Studio でモデルを構築する準備ができました。 ダウンサンプリングされたデータは、クラスター内に Hive テーブルとして保存されます。 Azure Machine Learning を使用して **リーダー** モジュールこのデータを読み取る。 このクラスターのストレージ アカウントにアクセスするための資格情報は、以下のとおりです。

### <a name="step1"></a> 手順 1: リーダー モジュールを使用して、Azure Machine Learning に Hive テーブルからデータを取得し、機械学習の実験用に選ぶ

まず、選択、 **+ 新規** ]-> [ **実験** ]-> [ **空白の実験**します。 その後から、 **検索** ボックスで、左上の「リーダー」を検索します。 ドラッグ アンド ドロップ、 **リーダー** モジュールを実験キャンバス (画面の中央部分) のデータ アクセスのため、モジュールを使用します。

これは、どのような **リーダー** Hive テーブルからデータを取得中のようになります。

![リーダーによるデータの取得](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

 **リーダー** モジュール、グラフィックに用意されているパラメーターの値が提供する必要がある値の並べ替えの例にすぎません。 パラメーターのセットを入力する方法の一般的なガイダンスをここでは、 **リーダー** モジュールです。

1. Hive クエリ] を選択して **データ ソース**
2.  **Hive データベース クエリ** ボックス、単純な SELECT * FROM < your\_database\_name.your\_table\_name > で十分です。
3. **Hcatalog サーバー URI**: クラスターが"abc"としている場合、これは単に: https://abc.azurehdinsight.net
4. **Hadoop ユーザー アカウント名**: クラスターの設定時に選択したユーザー名。 (リモート アクセスのユーザー名ではありません)。
5. **Hadoop ユーザー アカウントのパスワード**: クラスターの設定時に選択した上記のユーザー名のパスワード。 (リモート アクセスのパスワードではありません)。
6. **出力データの場所**:"Azure"を選択します
7. **Azure ストレージ アカウント名**: クラスターに関連付けられているストレージ アカウント
8. **Azure ストレージ アカウント キー**: クラスターに関連付けられているストレージ アカウントのキー。
9. **Azure コンテナー名**: クラスター名は"abc"としている場合、これは、単に"abc"と、通常です。 


1 回、 **リーダー** (に表示される緑色のチェック マーク モジュール)、データの取得が完了すると (任意の名前) をデータセットとしてこのデータを保存します。 次のように表示されます。

![リーダーによるデータの保存](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

出力ポートを右クリックして、 **リーダー** モジュールです。 これがわかります、 **データセットとして保存** オプションと **視覚化** オプション。  **視覚化** オプションをクリックするで役に立ちます概要統計情報を右側のパネルのデータの 100 行が表示されます。 データを保存するには選択 **データセットとして保存** 指示に従います。

機械学習の実験で使用する保存されたデータセットを選択するを使用してデータセットを検索、 **検索** ボックスの下に表示します。 データセットに付けた名前の一部を入力してデータセットにアクセスし、このデータセットをメイン パネルにドラッグします。 メイン パネル上にドロップしたら、機械学習のモデリングで使用するために選択します。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

***重要な注意事項:*** **これトレーニングとテスト データセットの両方を行います。また、この用途で付けたデータベース名とテーブル名を必ず使用してください。図の値は、説明の目的でのみ使用されている例です。**
 
### <a name="step2"></a> 手順 2: Azure Machine Learning Studio で簡単な実験を作成して、クリックする / クリックしないを予測します。

この Azure ML の実験内容は次のとおりです。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

次に、この実験の主要コンポーネントを見てみましょう。 まず、保存したトレーニング データセットとテスト データセットを実験キャンバスにドラッグする必要があります。

#### 見つからないデータのクリーンアップ

 **見つからないデータのクリーンアップ** モジュールは、その名前が示すとおり: ユーザー指定可能な方法で不足しているデータをクリーンアップします。 このモジュールは次のように表示されます。

![見つからないデータのクリーンアップ](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

ここでは、すべての不足値を 0 に置き換えることにしました。 他のオプションは、モジュールのドロップダウン リストに表示されます。

#### データに対する特徴エンジニアリング

カテゴリ型の特徴を持つ大規模なデータセットの場合、数百万単位の一意の値が含まれる可能性があります。 このような高次元のカテゴリ型の特徴を持つ場合、ホット エンコーディングなどの単純な方法を使用することは、まったく不可能です。 このチュートリアルでは、組み込みの Azure Machine Learning モジュールを利用するカウント特徴を使い、このような高次元のカテゴリ型変数をコンパクトな表現で生成する方法について説明します。 最終的に、モデル サイズは小さくなり、トレーニング時間は短縮され、他の技術を使用する場合と同等のパフォーマンス メトリックになります。

##### カウント変換の構築

カウント特徴を構築するには、使用して、 **カウント変換の構築** Azure Machine Learning で使用可能なモジュール。 次のようなモジュールです。


![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)


**重要な注意事項** : で、 **列のカウント** ボックス、のでカウントを実行するこれらの列を入力しました。 通常、これらは (前述した) 高次元カテゴリ列です。 このチュートリアルの初めに説明したように、Criteo データセットには Col15 から Col40 の 26 列のカテゴリ列があります。 ここでは、それらすべての列を使用し、インデックスを設定します (以下のようにコンマで区切った 15 から 40)。

(大規模なデータセットに適している) MapReduce モードでモジュールを使用するには、HDInsight Hadoop クラスター (前述の特徴の探索に使用したクラスターをこの用途に再使用することもできます) へのアクセス権とその資格情報が必要です。 上の図は、入力する値の例です (図に指定されている値は、実際の状況に合わせて置き換えてください)。 

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

上の図では、入力 BLOB の場所を入力する方法を示しています。 この場所には、カウント テーブルを構築するために予約されているデータがあります。


モジュールを右クリックしを選択して用のトランス フォームは後で保存できるこのモジュールの実行が完了した後、 **変換として保存** オプション。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

前述の実験アーキテクチャで説明したように、データセット "ytransform2" は保存したカウント変換と正確に対応しています。 この実験の残りの部分でとみなし、リーダーが使用される、 **カウント変換の構築** トレーニングとテストのデータセットの機能をモジュールには、一部のデータの数を生成し、その後で使用カウントを生成するようなカウントすることができます。

##### トレーニング データセットとテスト データセットの一部に含めるカウント特徴を選択する

特徴のトレーニングに含めるし、データセットを使用してテストをユーザーが選択できる、カウント変換が準備ができたら、 **カウント テーブル パラメーターの変更** モジュールです。 以下のモジュールは完全を期すために紹介しましたが、わかりやすくするために、実際には実験に使用しません。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

図のように、この例では log-odds のみを使用し、バック オフ列を無視することにしました。 また、ごみ箱のしきい値、平滑化のために追加する擬似的な過去の例の数、Laplacian ノイズを使用するかどうかなどのパラメーターを設定することもできます。 これらはいずれも高度な特徴です。このような特徴を初めて生成する場合は、この既定値を参考に利用することができます。

##### カウント特徴を生成する前のデータ変換

ここでは、実際にカウント特徴を生成する前に、トレーニング データとテスト データの変換について重要な点に注目します。 2 つ **R スクリプトの実行** カウント変換をデータに適用する前に使用されているモジュールです。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

1 つ目の R スクリプトは次のとおりです。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)


この R スクリプトでは、列を "Col1" から "Col40" という名前に変更します。 これは、カウント変換には、この形式の名前が使用されるためです。

2 つ目の R スクリプトでは、負のクラスをダウンサンプリングして、正のクラスと負のクラス (それぞれクラス 1 と 0) の分布のバランスを取ります。 次の R スクリプトは、その方法を示しています。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

このシンプルな R スクリプトで、"pos\_neg\_ratio"を使用して、正と負のクラス間のバランス量を設定します。 クラスの不均衡を改善すると、通常、クラスの分布が偏っているという分類の問題がある場合にパフォーマンスが改善されるため、この処理は重要です (正のクラスが 3.3%、負のクラスが 96.7% の例を思い出してください)。

##### カウント変換をデータに適用する

最後に、使用して、 **変換の適用** モジュールをトレーニングのカウント変換を適用し、テスト用のデータセットです。 このモジュールは、1 つの入力として保存済みのカウント変換を使用し、もう 1 つの入力としてトレーニング データセットまたはテスト データセットを使用して、カウント特徴を含むデータを返します。 次に例を示します。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### カウント特徴の概要を示す抜粋

この例のカウント特徴がどのような内容か見てみましょう。 次に、カウント特徴の抜粋を示します。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

この抜粋では、カウントしている列について、関連するバックオフだけでなく、カウントと対数オッズを取得しています。 

これらの変換後のデータセットを使用して、Azure Machine Learning モデルを構築する準備ができました。 次のセクションでは、その方法を説明します。

#### Azure Machine Learning モデルの構築

##### 学習者の選択

まず、学習者を選択する必要があります。 2 つのクラスのブースト デシジョン ツリーを学習者として使用します。 この学習者の既定のオプションを次に示します。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

実験のため、単純に既定値を選択します。 通常、既定値には意味があり、パフォーマンスの簡単なベースラインを得るために適しています。 ベースラインがあれば、パラメーターを整理してパフォーマンスを改善できます。

#### モデルをトレーニングする

トレーニング、単を呼び出します、 **モデルのトレーニング** モジュールです。 2 つの入力は、2 クラス ブースト デシジョン ツリー学習者とトレーニング データセットです。 次に例を示します。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)


#### モデルにスコアを付ける

トレーニング済みのモデルを用意すると、テスト データセットにスコアを付け、パフォーマンスを評価することができます。 使用して、これを行って、 **モデルのスコア付け** モジュールに、次に、 **モデルの評価** モジュール。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)


### <a name="step5"></a> 手順 5: モデルを評価する

最後に、モデルのパフォーマンスを分析してみましょう。 通常、2 つのクラス (二項) 分類の問題の効果的な手段は、AUC です。 関連付けますを視覚化する、 **モデルのスコア付け** モジュールを **モデルの評価** このモジュールです。 クリックすると **視覚化** 上、 **モデルの評価** モジュールようなグラフィックを生成するの下。

![評価モジュール BDT モデル](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

二項 (または 2 つのクラス) 分類の問題で、予測精度の効果的な手段は 曲線下面積 (AUC) です。 次に、このモデルをテスト データセットで使用した結果を示します。 これを取得するには、右の出力ポートをクリックして、 **モデルの評価** モジュールし **視覚化**します。

![視覚化のモデルの評価モジュール](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <a name="step6"></a> 手順 6: Web サービスとしてモデルを発行する
最小限の労力で Azure Machine Learning モデルを Web サービスとして発行することは、モデルを広く利用できるようにするために重要です。 発行すると、誰でも予測が必要な入力データを使用して Web サービスを呼び出すことができます。また、Web サービスはそのモデルを使用して予測を返します。

これを行うには、まずトレーニング済みのモデルをトレーニング済みのモデル オブジェクトとして保存します。 右クリックし、これは、 **モデルのトレーニング** モジュールを使用して、 **[トレーニング済みモデルとして保存** オプション。

次に、Web サービスの入力ポートと出力ポートを作成する必要があります。 

* 入力ポートは、予測が必要なデータと同じ形式のデータを受け入れます。 
* 出力ポートは、スコア付けラベルと関連する確率を返します。

#### 入力ポートにいくつかのデータ行を選択する

使用すると便利です、 **SQL 変換の適用** モジュールの入力ポート データとして処理するためにだけ 10 行を選択します。 次のように、SQL クエリを使用して、入力ポート用にこれらの行のデータのみを選択します。

![入力ポート データ](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### Web サービス
これで、Web サービスの発行のために使用できる小規模の実験を行う準備ができました。

#### Web サービスの入力データを生成する

手順 0 として、カウント テーブルが大きいため、いくつかの行のテスト データを取り出し、カウント特徴を使用してそれから出力データを生成します。 これは、Web サービスの入力データ形式として機能します。 これは次のように表示されます。

![BDT 入力データを作成する](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

注: 入力データ形式は、ここでは使っての出力、 **カウント フィーチャライザー** モジュールです。 この実験を実行しているが完了すると後からの出力を保存、 **カウント フィーチャライザー** データセットとモジュールです。 

**重要な注意事項:** このデータセットは、web サービスの入力データに使用されます。 

#### Web サービスの公開のためのスコア付け実験

最初に、次のように表示されます。 重要な構造は、 **モデルのスコア付け** トレーニング済みのモデル オブジェクトと、いくつかの行の入力データを使用して、前の手順で生成したを受け入れるモジュール、 **カウント フィーチャライザー** モジュールです。 "プロジェクト列" を使用して、スコア付けラベルとスコア付け確率を予測します。 

![プロジェクト列](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

通知方法、 **プロジェクト列** 'フィルター' のデータ データセットからのモジュールを使用できます。 以下に内容を説明します。

![プロジェクト列モジュールでのフィルター処理](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

クリックするだけの青色の入力と出力ポートを取得する **web サービスを準備する** 右下にあります。 クリックして、web サービスを公開することは、この実験を実行しても、 **WEB サービスの発行** 、右下にあるアイコン次に示します。

![Web サービスを発行する](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)


Web サービスが公開されると、次のページにリダイレクトされます。

![](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

Web サービス用の 2 つのリンクが左側に表示されます。

*  **要求/応答** サービス (または RRS) は 1 つの予測向けのもので、このワーク ショップで使用されます。 
*  **バッチ実行** サービス (BES) バッチ予測に使用され、入力データが Azure Blob ストレージに存在する予測に使用する必要があります。

リンクをクリックすると **要求/応答** 事前に定義されたコードを c#、python、R でのご紹介するページにはこのコードは、web サービスを呼び出す際に簡単に使用できます。 このページの API キーを認証に使用する必要があります。 

次の python コードを IPython Notebook の新しいセルにコピーすると便利です。 

以下に、python コードのセグメントと適切な API キーを示します。

![Python コード](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)


既定の API キーは Web サービスの API キーに置き換わりました。 クリックすると **実行** notebook ipython には、このセルには、次の応答が生成されます。

![iPython の応答](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

質問した 2 つのテスト サンプルに対して (python スクリプトの JSON フレームワークで)、"Scored Labels (スコア付けラベル), Scored Probabilities (スコア付け確率)" の形式で回答が返されました。 この場合は、事前に定義されたコード (すべての数値列に 0、すべてのカテゴリ列に文字列 "値") を指定する既定値を選択します。

以上で、Azure Machine Learning を使用して大規模なデータセットを処理する方法の総合的なチュートリアルは終了です。 ここでは、テラバイト単位のデータから始めて、予測モデルを構築し、クラウドに Web サービスとしてデプロイしました。


