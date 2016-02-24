<properties 
    pageTitle="HDInsight の Apache Spark での BI ツールの使用 | Microsoft Azure" 
    description="Apache Spark で Notebook を使用して生データのスキーマを作成し、Hive テーブルとして保存した後、BI ツールを Hive テーブルに使用してデータを分析する手順を説明します" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/30/2015" 
    ms.author="nitinme"/>


# Azure HDInsight の Apache Spark での BI ツールの使用

Azure HDInsight の Apache Spark を使用して以下のことを行う方法を説明します。

* 生のサンプル データを取得し、Hive テーブルとして保存します
* Power BI や Tableau などの BI ツールを使用して、データを分析および視覚化します

**前提条件:**

次のものが必要です。

- Azure サブスクリプション。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- Apache Spark クラスター。 手順については、次を参照してください。 [Azure HDInsight でのクラスターの Apache Spark のプロビジョニング](hdinsight-apache-spark-provision-clusters.md)します。
- Microsoft Spark ODBC ドライバー (HDInsight の Spark で Tableau を使用するために必要) がインストールされたコンピューター。 ドライバーをインストールする [ここ](http://go.microsoft.com/fwlink/?LinkId=616229)します。
- などの BI ツール [Power BI](http://www.powerbi.com/) または [Tableau Desktop](http://www.tableau.com/products/desktop)します。 Power BI の無料プレビュー サブスクリプションを取得する [http://www.powerbi.com/](http://www.powerbi.com/)します。

##<a name="hivetable"></a>生データを Hive テーブルとして保存します。

このセクションでは使用して、 [Jupyter](https://jupyter.org) notebook を生のサンプル データを処理して、Hive テーブルとして保存するジョブを実行する HDInsight での Apache Spark クラスターに関連付けられています。 サンプル データは、すべてのクラスターにおいて既定で使用できる .csv ファイル (hvac.csv) です。

データを Hive テーブルとして保存した後、次のセクションでは、Power BI や Tableau などの BI ツールを使用して Hive テーブルに接続します。

1.  [Azure ポータル](https://portal.azure.com/), 、スタート ボードで、タイルをクリックして、Spark クラスターの場合、スタート画面に固定表示)。 [クラスターに移動することもできます。 **すべてを参照** > **HDInsight クラスター**します。   

2. Spark クラスター ブレードで、次のようにクリックします。 **クイック リンク**, 、クリックし、 **クラスターのダッシュ ボード** ブレードで、[] をクリック **Jupyter Notebook**します。 入力を求められたら、クラスターの管理者資格情報を入力します。

    > [AZURE.NOTE] お使いのブラウザーで、次の URL を開くと、クラスターの Jupyter Notebook も到達可能性があります。 置換 __CLUSTERNAME__ 、クラスターの名前に置き換えます。
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. 新しい Notebook を作成します。 クリックして **新規**, 、順にクリック **python2**します。

    ![新しい Jupyter Notebook を作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Note.Jupyter.CreateNotebook.png "Create a new Jupyter notebook")

3. Untitled.pynb という名前の新しい Notebook が作成されて開かれます。 上部の Notebook 名をクリックし、わかりやすい名前を入力します。

    ![Notebook の名前を指定します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Note.Jupyter.Notebook.Name.png "Provide a name for the notebook")

4. 必要なモジュールをインポートし、Spark コンテキストと Hive コンテキストを作成します。 次のスニペットを空のセルに貼り付けてキーを押します **shift キーを押しながら ENTER**します。

        from pyspark import SparkContext
        from pyspark.sql import *
        from pyspark.sql import HiveContext

        # Create Spark and Hive contexts
        sc = SparkContext('spark://headnodehost:7077', 'pyspark')
        hiveCtx = HiveContext(sc)

    毎回 Jupyter でジョブを実行すると、web ブラウザーのウィンドウのタイトルが表示されます、 **(ビジー)** ステータスと notebook のタイトル。 横に塗りつぶされた円も表示されます、 **python2** 右上隅にあるテキスト。 ジョブが完了すると、白抜きの円に変化します。

     ![Jupyter Notebook ジョブのステータス](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Jupyter.Job.Status.png "Status of a Jupyter notebook job")

4. サンプル データを一時テーブルに読み込みます。 HDInsight でサンプル データ ファイルでの Spark クラスターをプロビジョニングするときに **hvac.csv**, 、関連付けられたストレージ アカウントにコピー **\HdiSamples\SensorSampleData\hvac**します。

    空のセルに貼り付けて、次のスニペットとキーを押して **shift キーを押しながら ENTER**します。 このスニペットは、という Hive テーブルにデータを登録 **hvac**します。


        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")
        
        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date")
        hvac = hvacParts.map(lambda p: {"Date": str(p[0]), "Time": str(p[1]), "TargetTemp": int(p[2]), "ActualTemp": int(p[3]), "BuildingID": int(p[6])})
        
        # Infer the schema and create a table       
        hvacTable = hiveCtx.inferSchema(hvac)
        hvacTable.registerAsTable("hvactemptable")
        hvacTable.saveAsTable("hvac")

5. テーブルが正常に作成されたことを確認します。 ノートブックに空のセルにコピー、次のスニペットとキーを押して **shift キーを押しながら ENTER**します。

        hiveCtx.sql("SHOW TABLES").show()

    出力は次のようになります。

        tableName       isTemporary
        hvactemptable   true       
        hivesampletable false      
        hvac            false

    False のテーブルだけ、 **isTemporary** 列は hive テーブルをメタストアに格納され、BI ツールからアクセスできます。 このチュートリアルでは接続は、 **hvac** 作成したテーブルです。

6. テーブルに目的のデータが含まれることを確認します。 ノートブックに空のセルにコピー、次のスニペットとキーを押して **shift キーを押しながら ENTER**します。

        hiveCtx.sql("SELECT * FROM hvac LIMIT 10").show()
    
7. カーネルを再起動して Notebook を終了します。 上部のメニュー バーからをクリックして **カーネル**, 、] をクリックして **再起動**, 、クリックして **再起動** 、プロンプトで再びします。

    ![Jupyter カーネルを再起動します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Jupyter.Restart.Kernel.png "Restart the Jupyter Kernel")

##<a name="powerbi"></a>Power BI を使用して Hive テーブル内のデータを分析するには

Hive テーブルとしてデータを保存した後は、Power BI を使用してデータに接続し、視覚化してレポートやダッシュボードなどを作成できます。

1. サインイン [Power BI](http://www.powerbi.com/)します。

2. [ようこそ] 画面で、次のようにクリックします。 **データベースとその他**します。

    ![Power BI にデータを取得します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Get.Data.png "Get data into Power BI")

3. 次の画面をクリックして **Spark** ] をクリックし、 **接続**します。

4. Azure HDInsight のページでの Spark、Spark クラスターに接続し、クリックしての値を指定します。 **接続**します。

    ![HDInsight の Spark クラスターに接続します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Connect.Spark.png "Connect to a Spark cluster on HDInsight")

    接続が確立されると、Power BI は HDInsight の Spark クラスターからデータのインポートを開始します。

5. Power BI は、データをインポートして新しいダッシュボードを表示します。 下に新しいデータ セットが追加も、 **データセット** 見出しです。 ダッシュボードの Spark タイルをクリックし、ワークシートを開いてデータを視覚化します。

    ![Power BI ダッシュボードの Spark タイル](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Tile.png "Spark tile on Power BI dashboard")

6. 注意して、 **フィールド** 一覧に、右、 **hvac** 前に作成したテーブルです。 テーブルを展開し、Notebook で定義したフィールドを表示します。

      ![Hive テーブルを一覧表示します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Display.Tables.png "List Hive tables")

7. 各ビルの目標温度と実際の温度の差を示す表示を作成します。 そのためにはドラッグ アンド ドロップ、 **BuildingID** フィールド **軸**, 、および **ActualTemp**/**TargetTemp** フィールド **値**です。

    ![グラフを作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Visual.1.png "Create visualizations")

    また、[ **領域マップ** (赤で示した) をデータを視覚化します。

8. 既定では、合計が表示の **ActualTemp** と **TargetTemp**します。 どちらのフィールドについて、ドロップダウン リストから選択 **平均** 両方のビルの実際の値の平均温度と目標温度を取得します。

    ![グラフを作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Visual.2.png "Create visualizations")

9. データの表示は次のようになります。 グラフの上にカーソルを移動すると、関連データを含むツール ヒントが表示されます。

    ![グラフを作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Visual.3.png "Create visualizations")

10. をクリックして **保存** 上部のメニューからレポート名を指定します。 表示を固定することもできます。 表示を固定するとダッシュボードに保存されて、最新の値を一目で追跡できるようになりす。 

    同じデータセットの表示をいくつでも追加して、データのスナップショット用にダッシュボードに固定できます。 また、HDInsight の Spark クラスターは Power BI に直接接続されます。 つまり、Power BI には常にクラスターの最新データが提供され、データセットを定期的に更新する必要はありません。

##<a name="tableau"></a>Tableau Desktop を使用して Hive テーブル内のデータを分析するには
    
1. Tableau Desktop を起動します。 接続先サーバーの一覧から、左のウィンドウで **Spark SQL**します。

2. Spark SQL 接続ダイアログ ボックスで、次のように、値を指定し、 **OK**します。

    ![Spark クラスターに接続します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Connect.png "Connect to a Spark cluster")

    認証のドロップ ダウン リスト **Windows** **Azure HDInsight サービス** オプションとして、インストールした場合にのみ、 [Microsoft Spark ODBC ドライバー](http://go.microsoft.com/fwlink/?LinkId=616229) コンピューターにします。

3. 次の画面から、 **スキーマ** ドロップダウンをクリック、 **検索** アイコンをクリックして **既定**します。

    ![スキーマを検索します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Find.Schema.png "Find schema")

4.  **テーブル** フィールドで、をクリックして、 **検索** アイコンをもう一度クラスターで利用可能なすべての Hive テーブルを一覧表示します。 確認する必要があります、 **hvac** notebook を使用して前に作成したテーブルです。

    ![テーブルを検索します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Find.Table.png "Find tables")

5. テーブルをドラッグして右側上部のボックスにドロップします。 Tableau はデータをインポートし、赤い四角で強調表示されているようにスキーマを表示します。

    ![Tableau にテーブルを追加します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Drag.Table.png "Add tables to Tableau")

6. クリックして、 **Sheet1** 左下にあるタブをクリックします。 すべてのビルの各日の目標温度と実際の温度の平均を表示するグラフを作成します。 ドラッグ **日付** と **の ID を作成** に **列** と **実際の温度**/**Temp をターゲット** に **行**します。  **マーク**, [ **領域** 領域マップ グラフを使用します。

     ![グラフのフィールドを追加します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Drag.Fields.png "Add fields for visualization")

7. 既定では、温度フィールドは集計として表示されます。 代わりに平均温度を表示する場合は、次のようにしてドロップダウンから行うことができます。

    ![平均温度を取得します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Temp.Avg.png "Take average of temperature")

8. 一方の温度を他の温度にスーパーインポーズして、温度の違いを見やすくすることもできます。 マウスを下部領域マップの隅に移動し、上記の赤い丸で囲んだハンドル形状にします。 マップを上部の他のマップにドラッグし、マウスが上記の赤い四角で囲んだ形状になったら放します。

    ![マップをマージします](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Merge.png "Merge maps")

     グラフが次のように変わります。

    ![グラフ](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Final.Visual.png "Visualization")
     
9. クリックして **保存** ワークシートを保存します。 ダッシュボードを作成して 1 つまたは複数のシートを追加できます。

##<a name="seealso"></a>関連項目

* [概要: Azure HDInsight での Apache Spark](hdinsight-apache-spark-overview.md)
* [クイック スタート: HDInsight の Apache Spark のプロビジョニングと Spark SQL を使用した対話型クエリの実行](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)
* [Machine Learning アプリケーションを作成するための HDInsight での Spark の使用](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [リアルタイム ストリーミング アプリケーションを作成するための HDInsight での Spark の使用](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)
* [Azure HDInsight での Apache Spark クラスターのリソースの管理](hdinsight-apache-spark-resource-manager.md)


[hdinsight-versions]: ../hdinsight-component-versioning/
[hdinsight-upload-data]: ../hdinsight-upload-data/
[hdinsight-storage]: ../hdinsight-use-blob-storage/


[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: ../storage-create-storage-account/ 

