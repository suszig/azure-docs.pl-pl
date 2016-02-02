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

## <a name="hivetable"></a>生データを Hive テーブルとして保存します。

このセクションでは使用して、 [Jupyter](https://jupyter.org) notebook を生のサンプル データを処理して、Hive テーブルとして保存するジョブを実行する HDInsight での Apache Spark クラスターに関連付けられています。 サンプル データは、すべてのクラスターにおいて既定で使用できる .csv ファイル (hvac.csv) です。

データを Hive テーブルとして保存した後、次のセクションでは、Power BI や Tableau などの BI ツールを使用して Hive テーブルに接続します。

1. [Azure ポータル](https://portal.azure.com/), 、スタート ボードで、タイルをクリックして、Spark クラスターの場合、スタート画面に固定表示)。 **[すべて参照]** > **[HDInsight クラスター]** でクラスターに移動することもできます。

2. Spark クラスター ブレードで、**[クイック リンク]** をクリックし、**[クラスター ダッシュボード]** ブレードで **[Jupyter Notebook]** をクリックします。 入力を求められたら、クラスターの管理者資格情報を入力します。
    > [AZURE.NOTE] ブラウザーで次の URL を開き、クラスターの Jupyter Notebook にアクセスすることもできます。 __CLUSTERNAME__ をクラスターの名前に置き換えます。
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. 新しい Notebook を作成します。 **[新規]** をクリックし、**[Python 2]** をクリックします。

    ![新しい Jupyter Notebook を作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Note.Jupyter.CreateNotebook.png "Create a new Jupyter notebook")

3. Untitled.pynb という名前の新しい Notebook が作成されて開かれます。 上部の Notebook 名をクリックし、わかりやすい名前を入力します。

    ![Notebook の名前を指定します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Note.Jupyter.Notebook.Name.png "Provide a name for the notebook")

4. 必要なモジュールをインポートし、Spark コンテキストと Hive コンテキストを作成します。 次のスニペットを空のセルに貼り付けて、**Shift + Enter** キーを押します。

     from pyspark import SparkContext
     from pyspark.sql import *
     from pyspark.sql import HiveContext
    
     # Create Spark and Hive contexts
     sc = SparkContext('spark://headnodehost:7077', 'pyspark')
     hiveCtx = HiveContext(sc)

 Jupyter でジョブを実行するたびに、Web ブラウザー ウィンドウのタイトルに **[(ビジー)]** ステータスと Notebook のタイトルが表示されます。 また、右上隅にある **Python 2** というテキストの横に塗りつぶされた円も表示されます。 ジョブが完了すると、白抜きの円に変化します。

  ![Jupyter Notebook ジョブのステータス](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Jupyter.Job.Status.png "Status of a Jupyter notebook job")

4. サンプル データを一時テーブルに読み込みます。 HDInsight の Spark クラスターをプロビジョニングすると、サンプル データ ファイル **hvac.csv** が関連するストレージ アカウントの **\HdiSamples\SensorSampleData\hvac** にコピーされます。

 次のスニペットを空のセルに貼り付けて、**Shift + Enter** キーを押します。 このスニペットは、**hvac** という Hive テーブルにデータを登録します。

     # Create an RDD from sample data
     hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")
    
     # Parse the data and create a schema
     hvacParts = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date")
     hvac = hvacParts.map(lambda p: {"Date": str(p[0]), "Time": str(p[1]), "TargetTemp": int(p[2]), "ActualTemp": int(p[3]), "BuildingID": int(p[6])})
    
     # Infer the schema and create a table       
     hvacTable = hiveCtx.inferSchema(hvac)
     hvacTable.registerAsTable("hvactemptable")
     hvacTable.saveAsTable("hvac")

5. テーブルが正常に作成されたことを確認します。 Notebook の空のセルに次のスニペットをコピーして、**Shift + Enter** キーを押します。

        hiveCtx.sql("SHOW TABLES").show()

    出力は次のようになります。

        tableName       isTemporary
        hvactemptable   true       
        hivesampletable false      
        hvac            false

    **isTemporary** 列が false の Hive テーブルだけがメタストアに格納されて、BI ツールからアクセスできるようになります。 このチュートリアルでは、作成したばかりの **hvac** テーブルに接続します。

6. テーブルに目的のデータが含まれることを確認します。 Notebook の空のセルに次のスニペットをコピーして、**Shift + Enter** キーを押します。

        hiveCtx.sql("SELECT * FROM hvac LIMIT 10").show()

7. カーネルを再起動して Notebook を終了します。 上部のメニュー バーから、**[カーネル]** をクリックし、**[再起動]** をクリックして、プロンプトで再び **[再起動]** をクリックします。

    ![Jupyter カーネルを再起動します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Jupyter.Restart.Kernel.png "Restart the Jupyter Kernel")

## <a name="powerbi"></a>Power BI を使用して Hive テーブル内のデータを分析するには

Hive テーブルとしてデータを保存した後は、Power BI を使用してデータに接続し、視覚化してレポートやダッシュボードなどを作成できます。

1. Sign in to [Power BI](http://www.powerbi.com/).

2. [ようこそ] 画面で、**[データベースとその他]** をクリックします。

    ![Power BI にデータを取得します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Get.Data.png "Get data into Power BI")

3. 次の画面で、**[Spark]**、**[接続]** の順にクリックします。

4. [Azure HDInsight の Spark] ページで、Spark クラスターに接続するための値を指定し、**[接続]** をクリックします。

    ![HDInsight の Spark クラスターに接続します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Connect.Spark.png "Connect to a Spark cluster on HDInsight")

    接続が確立されると、Power BI は HDInsight の Spark クラスターからデータのインポートを開始します。

5. Power BI は、データをインポートして新しいダッシュボードを表示します。 新しいデータ セットが **[データセット]** 見出しにも追加されます。 ダッシュボードの Spark タイルをクリックし、ワークシートを開いてデータを視覚化します。

    ![Power BI ダッシュボードの Spark タイル](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Tile.png "Spark tile on Power BI dashboard")

6. 右側の **[フィールド]** 一覧に、前に作成した **hvac** テーブルが含まれることに注意してください。 テーブルを展開し、Notebook で定義したフィールドを表示します。

      ![Hive テーブルを一覧表示します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Display.Tables.png "List Hive tables")

7. 各ビルの目標温度と実際の温度の差を示す表示を作成します。 そのためにはドラッグ アンド ドロップ、 **BuildingID** フィールド **軸**, 、および **ActualTemp**/**TargetTemp** フィールド **値**します。

    ![グラフを作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Visual.1.png "Create visualizations")

    また、データの表示方法として **[領域マップ]** (赤で示したもの) を選択します。

8. 既定では、**ActualTemp** および **TargetTemp** の合計が表示されます。 どちらのフィールドについても、ドロップダウンから **[平均]** を選択して、両方のビルの実際温度と目標温度の平均を表示します。

    ![グラフを作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Visual.2.png "Create visualizations")

9. データの表示は次のようになります。 グラフの上にカーソルを移動すると、関連データを含むツール ヒントが表示されます。

    ![グラフを作成します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.PowerBI.Visual.3.png "Create visualizations")

10. 上部メニューの **[保存]** をクリックし、レポート名を指定します。 表示を固定することもできます。 表示を固定するとダッシュボードに保存されて、最新の値を一目で追跡できるようになりす。

    同じデータセットの表示をいくつでも追加して、データのスナップショット用にダッシュボードに固定できます。 また、HDInsight の Spark クラスターは Power BI に直接接続されます。 つまり、Power BI には常にクラスターの最新データが提供され、データセットを定期的に更新する必要はありません。

## <a name="tableau"></a>Tableau Desktop を使用して Hive テーブル内のデータを分析するには

1. Tableau Desktop を起動します。 左側のウィンドウの接続先サーバー一覧で **[Spark SQL]** をクリックします。

2. Spark SQL 接続ダイアログ ボックスで、次の値を指定して、**[OK]** をクリックします。

    ![Spark クラスターに接続します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Connect.png "Connect to a Spark cluster")

    認証のドロップ ダウン リスト **Windows** **Azure HDInsight サービス** オプションとして、インストールした場合にのみ、 [Microsoft Spark ODBC ドライバー](http://go.microsoft.com/fwlink/?LinkId=616229) コンピューターにします。

3. 次の画面で、**[スキーマ]** ドロップダウンの **[検索]** アイコンをクリックし、**[デフォルト]** をクリックします。

    ![スキーマを検索します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Find.Schema.png "Find schema")

4. **[テーブル]** フィールドで **[検索]** アイコンをクリックし、クラスターで使用可能なすべての Hive テーブルを一覧表示します。 Notebook を使用して前に作成した **hvac** テーブルが表示されます。

    ![テーブルを検索します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Find.Table.png "Find tables")

5. テーブルをドラッグして右側上部のボックスにドロップします。 Tableau はデータをインポートし、赤い四角で強調表示されているようにスキーマを表示します。

    ![Tableau にテーブルを追加します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Drag.Table.png "Add tables to Tableau")

6. 左下の **[Sheet1]** タブをクリックします。 すべてのビルの各日の目標温度と実際の温度の平均を表示するグラフを作成します。 ドラッグ **日付** と **の ID を作成** に **列** と **実際の温度**/**Temp をターゲット** に **行**します。 **[マーク]** で **[領域]** を選択して領域マップ グラフを使用します。

     ![グラフのフィールドを追加します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Drag.Fields.png "Add fields for visualization")

7. 既定では、温度フィールドは集計として表示されます。 代わりに平均温度を表示する場合は、次のようにしてドロップダウンから行うことができます。

    ![平均温度を取得します](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Temp.Avg.png "Take average of temperature")

8. 一方の温度を他の温度にスーパーインポーズして、温度の違いを見やすくすることもできます。 マウスを下部領域マップの隅に移動し、上記の赤い丸で囲んだハンドル形状にします。 マップを上部の他のマップにドラッグし、マウスが上記の赤い四角で囲んだ形状になったら放します。

    ![マップをマージします](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Merge.png "Merge maps")

     グラフが次のように変わります。

    ![グラフ](./media/hdinsight-apache-spark-use-bi-tools/HDI.Spark.Tableau.Final.Visual.png "Visualization")

9. **[保存]** をクリックしてワークシートを保存します。 ダッシュボードを作成して 1 つまたは複数のシートを追加できます。

## <a name="seealso"></a>関連項目

* [Azure HDInsight での Apache Spark の概要:](hdinsight-apache-spark-overview.md)
* [クイック スタート: HDInsight と Spark SQL を使用して対話型クエリの実行での Apache Spark のプロビジョニング](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)
* [Machine learning アプリケーションを構築するための HDInsight の Spark を使用します。](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [リアルタイム ストリーミング アプリケーションを構築するための HDInsight の Spark を使用します。](hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming.md)
* [Azure HDInsight で Apache Spark クラスターのリソースを管理します。](hdinsight-apache-spark-resource-manager.md)



[hdinsight-versions]: ../hdinsight-component-versioning/ 
[hdinsight-upload-data]: ../hdinsight-upload-data/ 
[hdinsight-storage]: ../hdinsight-use-blob-storage/ 
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/ 
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/ 
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/ 
[azure-management-portal]: https://manage.windowsazure.com/ 
[azure-create-storageaccount]: ../storage-create-storage-account/ 

