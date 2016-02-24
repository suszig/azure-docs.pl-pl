<properties
    pageTitle="Azure HDInsight への Spark クラスターのプロビジョニングと、Zeppelin および Jupyter から Spark SQL を使用した対話型の分析 | Microsoft Azure"
    description="HDInsight に Apache Spark クラスターをすばやくプロビジョニングし、Zeppelin および Jupyter Notebook から Spark SQL を使用して対話型クエリを実行する手順を説明します。"
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


# 概要: Azure HDInsight の Apache Spark のプロビジョニングと Spark SQL を使用した対話型クエリの実行

[AZURE.INCLUDE [hdinsight-azure-preview-portal](../../includes/hdinsight-azure-preview-portal.md)]

* [HDInsight の Apache Spark のプロビジョニングと Spark SQL を使用した対話型クエリの実行](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql-v1.md)

簡易作成] オプションを使用した HDInsight での Apache Spark クラスターをプロビジョニングする方法について説明し、web ベースを使用して [Zeppelin](https://zeppelin.incubator.apache.org) と [Jupyter](https://jupyter.org) ノート パソコンで Spark クラスターに対して Spark SQL 対話型クエリを実行します。


   ![HDInsight で Apache Spark を使用します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.GetStartedFlow.Spark.png  "Get started using Apache Spark in HDInsight tutorial. Steps illustrated: create a storage account; provision a cluster; run Spark SQL statements")

**前提条件:**

このチュートリアルを開始する前に、Azure サブスクリプションが必要です。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。


## <a name="provision"></a>HDInsight の Spark クラスターをプロビジョニングします。

このセクションでは、Spark バージョン 1.3.1 に基づいて HDInsight バージョン 3.2 クラスターをプロビジョニングします。 HDInsight バージョンとその Sla については、次を参照してください。 [HDInsight コンポーネントのバージョン](hdinsight-component-versioning.md)です。

>[AZURE.NOTE] この記事の手順では、基本的な構成設定を使用して HDInsight での Apache Spark クラスターを作成します。 (追加の記憶域、Azure の仮想ネットワークまたはメタストアを使用して、Hive 用) などの他のクラスター構成設定の詳細については、次を参照してください。 [HDInsight クラスターのプロビジョニングのカスタム オプションを使用して](hdinsight-apache-spark-provision-clusters.md)します。


**Spark クラスターをプロビジョニングするには**

1. サインイン、 [Azure ポータル](https://ms.portal.azure.com/)します。

2. クリックして **新規**, 、] をクリックして **データ + 分析**, 、] をクリックし、 **HDInsight**します。

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.CreateCluster.1.png "Creating a new cluster in the Azure Portal")

3. 入力、 **クラスター名**, [ **Hadoop** の **クラスターの種類**, との間、 **クラスターのオペレーティング システム** ドロップ ダウン メニューで、 **Windows Server 2012 R2 Datacenter**します。 クラスターを使用できる場合は、クラスター名の横に緑色のチェック マークが表示されます。

    ![クラスターの名前と種類の入力](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.CreateCluster.2.png "Enter cluster name and type")

4. 複数のサブスクリプションの場合はクリックして、 **サブスクリプション** エントリをクラスターで使用する Azure サブスクリプションを選択します。

5. クリックして **リソース グループ** を既存のリソース グループの一覧を表示し、クラスターを作成する場所を選択します。 またはをクリックして **新規作成** し、新しいリソース グループの名前を入力します。 新しいグループ名を使用できる場合は、緑のチェック マークが表示されます。

    > [AZURE.NOTE] このエントリは、いずれかが使用可能な場合に、既存のリソース グループのいずれかに既定値です。

6. クリックして **資格情報**, 、し、入力、 **クラスター ログイン ユーザー名** と **クラスター ログインのパスワード**します。 クラスター ノードでリモート デスクトップを有効にする **リモート デスクトップを有効にする**, 、] をクリックして **はい**, 、し、必要な値を指定します。 このチュートリアルではリモート デスクトップは必要ないため、この手順はスキップできます クリックして **選択** 下部の資格情報の構成を保存します。

    ![クラスターの資格情報の指定](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.CreateCluster.3.png "Provide cluster credentials")

7. クリックして **データソース** クラスターの場合、既存のデータ ソースを選択するか、新しいものを作成します。 HDInsight で Hadoop クラスターをプロビジョニングするときに、Azure ストレージ アカウントを指定します。 Hadoop 分散ファイルシステム (HDFS) と同様、このアカウントの特定の Blob Storage コンテナーが、既定のファイル システムとして設定されます。 既定では、HDInsight クラスターは、指定されたストレージ アカウントと同じデータ センターにプロビジョニングされます。 詳細については、次を参照してください [HDInsight で使用する Azure Blob ストレージ。][hdinsight-storage]

    ![[データ ソース] ブレード](./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.CreateCluster.4.png "Provide data source configuration")

    現在、HDInsight クラスターのデータ ソースとして Azure Storage アカウントを選択できます。 エントリを理解するために、次を使用して、 **データソース** ブレードです。

    - **選択方法**: これを設定 **すべてのサブスクリプションから** すべてのサブスクリプションからストレージ アカウントの参照を有効にします。 これを設定 **アクセス キー** を入力する場合、 **ストレージ名** と **アクセス キー** の既存のストレージ アカウント。

    - **ストレージ アカウントを選択]、[新規作成**: クリックして **ストレージ アカウントを選択** を参照して、クラスターに関連付ける既存のストレージ アカウントを選択します。 または、[ **新規作成** 新しいストレージ アカウントを作成します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。

    - **既定のコンテナーを選択して**: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、コンテナーが特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。

    - **場所**: リージョン、ストレージ アカウントには、やで作成されます。

        > [AZURE.IMPORTANT] HDInsight クラスターの場所を設定も既定のデータ ソースの場所を選択します。 クラスターと既定のデータ ソースは、同じリージョンに存在する必要があります。

    クリックして **選択** をデータ ソースの構成を保存します。

8. クリックして **ノード料金レベル** このクラスター用に作成するノードに関する情報を表示します。 クラスターで必要な worker ノードの数を設定します。 クラスターの推定コストがブレード内に表示されます。

    ![[ノード価格レベル] ブレード](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.CreateCluster.5.png "Specify number of cluster nodes")

    をクリックして **選択** 価格構成ノードを保存します。

9.  **新しい HDInsight クラスター** ブレードで、いることを確認 **スタート画面にピン** クリックして選択すると、 **作成**します。 これでクラスターが作成され、Azure ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターがプロビジョニング中であることを示し、プロビジョニングが完了すると、[HDInsight] アイコンを表示するように変化します。

    | プロビジョニング中 | プロビジョニング完了 |
    | ------------------ | --------------------- |
    | ![スタート画面のプロビジョニング中インジケーター](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/provisioning.png) | ![プロビジョニングされたクラスターのタイル](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/provisioned.png) |

    > [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または **通知** プロビジョニング プロセスをチェックするページの左にあるエントリです。

10. プロビジョニングが完了したら、スタート画面で Spark クラスター用のタイルをクリックして、クラスター ブレードを起動します。


## <a name="zeppelin"></a>Zeppelin notebook を使用して対話型 Spark SQL クエリを実行します。

クラスターをプロビジョニングした後、Web ベースの Zeppelin Notebook を使用して、Spark HDInsight クラスターに対して Spark SQL の対話型クエリを実行できます。 このセクションでは、クラスターにおいて既定で使用できるサンプル データ ファイル (hvac.csv) を使用していくつかの対話型 Spark SQL クエリを実行します。

>[AZURE.NOTE] 以下の手順で作成した notebook も、クラスターにおいて既定で使用できます。 Zeppelin を起動した後は、名前でこの notebook を検索 **Zeppelin HVAC tutorial**します。

1.  [Azure ポータル](https://portal.azure.com/), 、スタート ボードで、タイルをクリックして、Spark クラスターの場合、スタート画面に固定表示)。 [クラスターに移動することもできます。 **すべてを参照** > **HDInsight クラスター**します。   

2. Spark クラスター ブレードで、次のようにクリックします。 **クイック リンク**, 、クリックし、 **クラスターのダッシュ ボード** ブレードで、[] をクリック **Zeppelin Notebook**します。 入力を求められたら、クラスターの管理者資格情報を入力します。

    > [AZURE.NOTE] お使いのブラウザーで、次の URL を開くと、クラスターの Zeppelin Notebook も到達可能性があります。 置換 __CLUSTERNAME__ 、クラスターの名前に置き換えます。
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. 新しい Notebook を作成します。 ヘッダー ウィンドウから次のようにクリックします。 **Notebook**, 、クリックして **Note の新規作成**します。

    ![新しい Zeppelin Notebook を作成します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.CreateNewNote.png "Create a new Zeppelin notebook")

    同じページで下にある、 **Notebook** 見出し名で始まる新しい notebook が表示されます **Note XXXXXXXXX**します。 新しい Notebook をクリックします。

3. 新しい Notebook の Web ページで、見出しをクリックし、必要に応じて Notebook の名前を変更します。 Enter キーを押して名前の変更を保存します。 また、必ず、notebook のヘッダーを示しています、 **接続** 右上隅にある状態。

    ![Zeppelin Notebook のステータス](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.NewNote.Connected.png "Zeppelin notebook status")

4. サンプル データを一時テーブルに読み込みます。 HDInsight でサンプル データ ファイルでの Spark クラスターをプロビジョニングするときに **hvac.csv**, 、関連付けられたストレージ アカウントにコピー **\HdiSamples\SensorSampleData\hvac**します。

    新しい Notebook に既定で作成される空の段落に、次のコードを貼り付けます。

        // Create an RDD using the default Spark context, sc
        val hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")

        // Define a schema
        case class Hvac(date: String, time: String, targettemp: Integer, actualtemp: Integer, buildingID: String)

        // Map the values in the .csv file to the schema
        val hvac = hvacText.map(s => s.split(",")).filter(s => s(0) != "Date").map(
            s => Hvac(s(0),
                    s(1),
                    s(2).toInt,
                    s(3).toInt,
                    s(6)
            )
        ).toDF()

        // Register as a temporary table called "hvac"
        hvac.registerTempTable("hvac")

    キーを押して **shift キーを押しながら ENTER** キーボードまたは、[、 **再生** コードを実行する段落のボタンをクリックします。 段落の右上隅にあるステータスが、[準備完了]、[保留中]、[実行中]、[完了] の順に進行します。 出力が同じ段落の下に表示されます。 スクリーンショットは次のようになります。

    ![生データから一時テーブルを作成します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Note.LoadDataIntoTable.png "Create a temporary table from raw data")

    各段落にタイトルを指定することもできます。 右上隅をクリックして、 **設定** アイコンをクリックして **タイトルを表示する**です。

5. に対して Spark SQL ステートメントを実行することができます、 **hvac** テーブルです。 次のクエリを新しい段落に貼り付けます。 このクエリは、ビル ID と、特定の日の各ビルの目標温度と実温度の差を取得します。 キーを押して **shift キーを押しながら ENTER**します。

        %sql
        select buildingID, (targettemp - actualtemp) as temp_diff, date
        from hvac
        where date = "6/1/13"

     **%Sql** 先頭にあるステートメントには、Spark SQL インタープリターを使用する notebook がように指示します。 定義済みのインタープリターを見ることができます、 **インタープリター** notebook のヘッダーでタブをクリックします。

    次のスクリーンショットでは出力を示します。

    ![Notebook を使用して Spark SQL ステートメントを実行します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Note.SparkSQLQuery1.png "Run a Spark SQL statement using the notebook")

    表示オプション (四角で囲ってある部分) をクリックして、同じ出力の異なる表現に切り替えることができます。 クリックして **設定** 出力の構成をキーと値を選択します。 上記の画面キャプチャを使用して **buildingID** 、キーの平均として **temp_diff** 値として。

6. クエリの変数を使用して Spark SQL ステートメントを実行することもできます。 次のコード例は、変数を定義する方法を示しています。 **Temp**, 、でクエリを実行する値を含むクエリでします。 初めてクエリを実行すると、変数に指定した値がドロップダウンに自動的に設定されます。

        %sql
        select buildingID, date, targettemp, (targettemp - actualtemp) as temp_diff
        from hvac
        where targettemp > "${Temp = 65,65|75|85}"

    新しい段落とキーを押してこのコード例をコピーして貼り付けて **shift キーを押しながら ENTER**します。 次のスクリーンショットでは出力を示します。

    ![Notebook を使用して Spark SQL ステートメントを実行します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Note.SparkSQLQuery2.png "Run a Spark SQL statement using the notebook")

    後続のクエリでは、ドロップダウンから新しい値を選択し、クエリを再実行できます。 クリックして **設定** 出力の構成をキーと値を選択します。 上記の画面キャプチャを使用して **buildingID** の平均値、キーとして **temp_diff** 値として、および **targettemp** グループとします。

7. Spark SQL インタープリターを再起動して、アプリケーションを終了します。 クリックして、 **インタープリター** 上部にあると、Spark インタープリターのタブをクリックして **再起動**します。

    ![Zeppelin インタープリターを再起動します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Zeppelin.Restart.Interpreter.png "Restart the Zeppelin intepreter")

## <a name="jupyter"></a>Jupyter notebook を使用して Spark SQL クエリを実行します。

このセクションでは、Jupyter Notebook を使用して、Spark クラスターに対して Spark SQL クエリを実行します。

>[AZURE.NOTE] 以下の手順で作成した notebook も、クラスターにおいて既定で使用できます。 Jupyter を起動した後は、名前でこの notebook を検索 **HVACTutorial.ipynb**します。

1.  [Azure ポータル](https://portal.azure.com/), 、スタート ボードで、タイルをクリックして、Spark クラスターの場合、スタート画面に固定表示)。 [クラスターに移動することもできます。 **すべてを参照** > **HDInsight クラスター**します。   

2. Spark クラスター ブレードで、次のようにクリックします。 **クイック リンク**, 、クリックし、 **クラスターのダッシュ ボード** ブレードで、[] をクリック **Jupyter Notebook**します。 入力を求められたら、クラスターの管理者資格情報を入力します。

    > [AZURE.NOTE] お使いのブラウザーで、次の URL を開くと、クラスターの Jupyter Notebook も到達可能性があります。 置換 __CLUSTERNAME__ 、クラスターの名前に置き換えます。
    >
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`

2. 新しい Notebook を作成します。 クリックして **新規**, 、] をクリックし、 **[python2]**します。

    ![新しい Jupyter Notebook を作成します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Note.Jupyter.CreateNotebook.png "Create a new Jupyter notebook")

3. Untitled.pynb という名前の新しい Notebook が作成されて開かれます。 上部の Notebook 名をクリックし、わかりやすい名前を入力します。

    ![Notebook の名前を指定します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Note.Jupyter.Notebook.Name.png "Provide a name for the notebook")

4. 必要なモジュールをインポートし、Spark コンテキストと SQL コンテキストを作成します。 次のコード例を空のセルに貼り付けて、キーを押します **shift キーを押しながら ENTER**します。

        from pyspark import SparkContext
        from pyspark.sql import SQLContext
        from pyspark.sql.types import *

        # Create Spark and SQL contexts
        sc = SparkContext('spark://headnodehost:7077', 'pyspark')
        sqlContext = SQLContext(sc)

    Jupyter でジョブを実行するたびに、web ブラウザー ウィンドウのタイトルが表示されます、 **(ビジー)** ステータスと notebook のタイトル。 横に塗りつぶされた円も表示されます、 **python2** 右上隅にあるテキスト。 ジョブが完了すると、白抜きの円に変化します。

     ![Jupyter Notebook ジョブのステータス](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Jupyter.Job.Status.png "Status of a Jupyter notebook job")

4. サンプル データを一時テーブルに読み込みます。 HDInsight でサンプル データ ファイルでの Spark クラスターをプロビジョニングするときに **hvac.csv**, 、関連付けられたストレージ アカウントにコピー **\HdiSamples\SensorSampleData\hvac**します。

    空のセルに、次のコード例を貼り付け **shift キーを押しながら ENTER**します。 このコード例では、という一時テーブルにデータを登録 **hvac**します。

        # Load the data
        hvacText = sc.textFile("wasb:///HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create the schema
        hvacSchema = StructType([StructField("date", StringType(), False),StructField("time", StringType(), False),StructField("targettemp", IntegerType(), False),StructField("actualtemp", IntegerType(), False),StructField("buildingID", StringType(), False)])

        # Parse the data in hvacText
        hvac = hvacText.map(lambda s: s.split(",")).filter(lambda s: s[0] != "Date").map(lambda s:(str(s[0]), str(s[1]), int(s[2]), int(s[3]), str(s[6]) ))

        # Create a data frame
        hvacdf = sqlContext.createDataFrame(hvac,hvacSchema)

        # Register the data fram as a table to run queries against
        hvacdf.registerAsTable("hvac")

        # Run queries against the table and display the data
        data = sqlContext.sql("select buildingID, (targettemp - actualtemp) as temp_diff, date from hvac where date = \"6/1/13\"")
        data.show()

5. ジョブが正常に完了すると、次の出力が表示されます。

        buildingID temp_diff date  
        4          8         6/1/13
        3          2         6/1/13
        7          -10       6/1/13
        12         3         6/1/13
        7          9         6/1/13
        7          5         6/1/13
        3          11        6/1/13
        8          -7        6/1/13
        17         14        6/1/13
        16         -3        6/1/13
        8          -8        6/1/13
        1          -1        6/1/13
        12         11        6/1/13
        3          14        6/1/13
        6          -4        6/1/13
        1          4         6/1/13
        19         4         6/1/13
        19         12        6/1/13
        9          -9        6/1/13
        15         -10       6/1/13

6. カーネルを再起動してアプリケーションを終了します。 上部のメニュー バーからをクリックして **カーネル**, 、] をクリックして **再起動**, 、クリックして **再起動** 、プロンプトで再びします。

    ![Jupyter カーネルを再起動します](./media/hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql/HDI.Spark.Jupyter.Restart.Kernel.png "Restart the Jupyter Kernel")


## <a name="seealso"></a>関連項目


* [概要: Azure HDInsight での Apache Spark](hdinsight-apache-spark-overview.md)
* [HDInsight クラスターでの Spark のプロビジョニング](hdinsight-apache-spark-provision-clusters.md)
* [HDInsight と BI ツールで Spark を使用した対話型データ分析の実行](hdinsight-apache-spark-use-bi-tools.md)
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

