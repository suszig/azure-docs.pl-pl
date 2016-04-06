<properties 
    pageTitle="HDInsight の Apache Spark で Azure Event Hubs を使用したストリーミング データの処理 | Microsoft Azure" 
    description="Azure Event Hub にデータ ストリームを送信し、Zeppelin Notebook を使用して Spark でイベントを受信する方法の詳細な手順を説明します" 
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


# Spark ストリーミング: HDInsight での Apache Spark による Azure Event Hubs からのイベントの処理

Spark ストリーミングは、コア Spark API を拡張して、スケーラビリティ、高スループット、フォールト トレランスを備えたストリーム処理アプリケーションを構築します。 多くのソースからデータを取り込むことができます。 この記事では、Event Hubs を使用してデータを取り込みます。 Event Hubs は、スケーラブルなインジェスト システムであり、1 秒間に数百万件のイベントを取り込むことができます。 

このチュートリアルでは、Azure Event Hubs を作成する方法、C# のコンソール アプリケーションを使用して Event Hubs にメッセージを取り込む方法、および HDInsight で Apache Spark 用に構成されている Zeppelin Notebook を使用して並列にメッセージを取得する方法を説明します。

> [AZURE.NOTE] この記事の手順を実行するには、Azure ポータルの両方のバージョンを使用する必要があります。 使用する Event Hub を作成する、 [Azure ポータル](https://manage.windowsazure.com)します。 HDInsight Spark クラスターを操作するには、使用して、 [Azure プレビュー ポータル](https://ms.portal.azure.com/)します。  

**前提条件:**

次のものが必要です。

- Azure サブスクリプション。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- Apache Spark クラスター。 手順については、次を参照してください。 [Azure HDInsight でのクラスターの Apache Spark のプロビジョニング](hdinsight-apache-spark-provision-clusters.md)します。
-  [Azure Event Hub](service-bus-event-hubs-csharp-ephcs-getstarted.md)します。
- Microsoft Visual Studio 2013 がインストールされているワークステーション。 手順については、次を参照してください。 [Visual Studio のインストール](https://msdn.microsoft.com/library/e2h7fzkw.aspx)します。

##<a name="createeventhub"></a>Azure Event Hub の作成

1.  [Azure ポータル](https://manage.windowsazure.com), [ **新規** > **Service Bus** > **Event Hub** > **カスタム作成**します。

2.  **新しいイベント ハブの追加** 画面で、入力、 **イベント ハブ名**, を選択、 **地域** 、ハブを作成し、新しい名前空間を作成するか既存のものを選択します。 クリックして、 **矢印** を続行します。

    ![ウィザード ページ 1](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Create.Event.Hub.png "Create an Azure Event Hub")

    > [AZURE.NOTE] 同じ操作を選択する必要があります **場所** 待機時間とコストを削減する HDInsight の Apache Spark クラスターとします。

3.  **Event Hub の構成** 画面で、入力、 **カウントをパーティション分割** と **メッセージ保有** 数値を使用して、チェック マークをクリックします。 この例では、パーティション カウントに 10 を、メッセージ保持に 1 を使用します。 パーティション数を書き留めておきます。この値は後で必要になります。

    ![ウィザード ページ 2](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Create.Event.Hub2.png "Specify partition size and retention days for Event Hub")

4. 作成したイベント ハブをクリックして、クリックして **構成**, 、し、event hub の 2 つのアクセス ポリシーを作成します。

    <table>
    <tr><th>名前</th><th>アクセス許可</th></tr>
    <tr><td>mysendpolicy</td><td>送信</td></tr>
    <tr><td>myreceivepolicy</td><td>リッスン</td></tr>
    </table>

    アクセス許可の作成後、選択、 **保存** ページの下部にあるアイコン。 これは、送信に使用される共有アクセス ポリシーを作成 (**mysendpolicy**) とリッスン (**myreceivepolicy**) この Event Hub にします。

    ![ポリシー](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policies.png "Create Event Hub policies")

    
5. 同じページで、2 つのポリシーに対して生成されるポリシー キーを記録しておきます。 後で使用するので、これらのキーを保存します。

    ![ポリシー キー](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policy.Keys.png "Save policy keys")

6.  **ダッシュ ボード** ] ページで [ **接続情報** ボトムアップを取得し、2 つのポリシーを使用して Event Hub の接続文字列を保存します。

    ![ポリシー キー](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policy.Connection.Strings.png "Save policy connection strings")

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

##<a name="receivezeppelin"></a>Zeppelin を使用した HDInsight 上の Spark でのメッセージの取得

このセクションで作成、 [Zeppelin](https://zeppelin.incubator.apache.org) ノートブックを HDInsight の Spark クラスターに、Event Hub からメッセージを受信します。

### ストリーミング アプリケーションのリソースを Zeppelin に割り当てる

Zeppelin でストリーミング アプリケーションを作成するとき、次の事項を考慮する必要があります。

* **イベント ハブ パーティションと Zeppelin に割り当てられているコア**します。 前の手順では、いくつかのパーティションを含む Event Hub を作成しました。 下で作成する Zeppelin ストリーミング アプリケーションで、再度、同じ数のパーティションを指定します。 Zeppelin で Event Hub からデータをストリーミングするには、Zeppelin に割り当てるコアの数を Event Hub のパーティションの数の 2 倍にする必要があります。
* **Zeppelin に割り当てられるコアの最小数**します。 下で作成するストリーミング アプリケーションで、アプリケーションでストリーミングしするメッセージを保存する一時テーブルを作成します。 次に、Spark SQL ステートメントを利用し、この一時テーブルからメッセージを読み込みます。 Spark SQL ステートメントを実行するには、少なくとも 2 つのコアを Zeppelin に割り当てる必要があります。

上の 2つの要件を合わせると、次のようになります。

* Zeppelin に割り当てなければならないコアの最小数は 2 です。
* 割り当てられるコアの数は常に Event Hub のパーティションの数の 2 倍にする必要があります。 

Spark クラスターのリソースを割り当てる方法については、次を参照してください。 [HDInsight で Apache Spark クラスターのリソースを管理](hdinsight-apache-spark-resource-manager.md)します。

### Zeppelin を利用してストリーミング アプリケーションを作成する

1.  [Azure ポータル](https://portal.azure.com/), 、スタート ボードで、タイルをクリックして、Spark クラスターの場合、スタート画面に固定表示)。 [クラスターに移動することもできます。 **すべてを参照** > **HDInsight クラスター**します。   

2. Spark クラスター ブレードで、次のようにクリックします。 **クイック リンク**, 、クリックし、 **クラスターのダッシュ ボード** ブレードで、[] をクリック **Zeppelin Notebook**します。 入力を求められたら、クラスターの管理者資格情報を入力します。

    > [AZURE.NOTE] お使いのブラウザーで、次の URL を開くと、クラスターの Zeppelin Notebook も到達可能性があります。 置換 __CLUSTERNAME__ 、クラスターの名前に置き換えます。
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. 新しい Notebook を作成します。 ヘッダー ウィンドウから次のようにクリックします。 **Notebook**, 、ドロップダウン リストから [] をクリックして **Note の新規作成**します。

    ![新しい Zeppelin Notebook を作成します](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.CreateNewNote.png "Create a new Zeppelin notebook")

    同じページで下にある、 **Notebook** 見出し名で始まる新しい notebook が表示されます **Note XXXXXXXXX**します。 新しい Notebook をクリックします。

3. 新しい Notebook の Web ページで、見出しをクリックし、必要に応じて Notebook の名前を変更します。 Enter キーを押して名前の変更を保存します。 また、必ず、notebook のヘッダーを示しています、 **接続** 右上隅にある状態。

    ![Zeppelin Notebook のステータス](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.NewNote.Connected.png "Zeppelin notebook status")

4. 新しい Notebook に既定で作成される空の段落に次のスニペットを貼り付け、Event Hub の構成を使用するようにプレースホルダーを置き換えます。 このスニペットでは、Event Hub からストリームを受信し、登録するストリームという一時テーブルに **mytemptable**します。 次のセクションでは、送信側アプリケーションを開始します。 その後は、テーブルから直接データを読み取ることができます。

    > [AZURE.NOTE] 次のスニペットで **eventhubs.checkpoint.dir** ディレクトリを既定のストレージ コンテナーに設定する必要があります。 ディレクトリが存在しない場合、ストリーミング アプリケーションにより作成されます。 ように、ディレクトリへの完全パスを指定することができますか、"**wasb://container@storageaccount.blob.core.windows.net/mycheckpointdir/**"、または、ディレクトリへの相対パスだけなど"**/mycheckpointdir**"です。

        import org.apache.spark.streaming.{Seconds, StreamingContext}
        import org.apache.spark.streaming.eventhubs.EventHubsUtils
        import sqlContext.implicits._
        
        val ehParams = Map[String, String](
          "eventhubs.policyname" -> "<name of policy with listen permissions>",
          "eventhubs.policykey" -> "<key of policy with listen permissions>",
          "eventhubs.namespace" -> "<service bus namespace>",
          "eventhubs.name" -> "<event hub in the service bus namespace>",
          "eventhubs.partition.count" -> "1",
          "eventhubs.consumergroup" -> "$default",
          "eventhubs.checkpoint.dir" -> "/<check point directory in your storage account>",
          "eventhubs.checkpoint.interval" -> "10"
        )
        
        val ssc =  new StreamingContext(sc, Seconds(10))
        val stream = EventHubsUtils.createUnionStream(ssc, ehParams)
        
        case class Message(msg: String)
        stream.map(msg=>Message(new String(msg))).foreachRDD(rdd=>rdd.toDF().registerTempTable("mytemptable"))

        stream.print
        ssc.start


##<a name="runapps"></a>アプリケーションの実行

1. Zeppelin Notebook から、スニペットを追加した段落を実行します。 キーを押して **shift キーを押しながら ENTER** または **再生** 右上隅にあるボタンをクリックします。

    段落の右上隅にあるステータスが、[準備完了]、[保留中]、[実行中]、[完了] の順に進行します。 出力が同じ段落の下に表示されます。 スクリーンショットは次のようになります。

    ![スニペットの出力](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Zeppelin.Code.Output.png "Output of the snipet")

2. 実行、 **送信者** プロジェクトとキーを押して **Enter** Event Hub にメッセージを送信を開始、コンソール ウィンドウにします。

3. Zeppelin Notebook の新しい段落に次のスニペットを入力し、Spark で受信したメッセージを読み取ります。

        %sql 
        select * from mytemptable limit 10

    次の画面キャプチャで受信したメッセージを表示する、 **mytemptable**します。

    ![Zeppelin でメッセージを受信します](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Zeppelin.Output.png "Receive messages in Zeppelin notebook")

4. Spark SQL インタープリターを再起動して、アプリケーションを終了します。 クリックして、 **インタープリター** 上部にあると、Spark インタープリターのタブをクリックして **再起動**します。

    ![Zeppelin インタープリターを再起動します](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Zeppelin.Restart.Interpreter.png "Restart the Zeppelin intepreter")

##<a name="sparkstreamingha"></a>高可用性のストリーミング アプリケーションの実行

Zeppelin を使用して HDInsight の Spark クラスターでストリーミング データを受け取るのは、アプリケーションのプロトタイプに適した方法です。 ただし、高可用性と復元性を備えた実稼動環境セットアップでストリーミング アプリケーションを実行するには、以下を行う必要があります。

1. クラスターに関連付けられたストレージ アカウントに、依存関係の jar をコピーします。
2. ストリーミング アプリケーションをビルドします。
3. クラスターに RDP 接続し、クラスターのヘッドノードにアプリケーションの jar をコピーします。
3. クラスターに RDP 接続し、クラスター ノードでアプリケーションを実行します。

次の手順およびサンプルのストリーミング アプリケーションを実行する方法についてに GitHub からダウンロードできます。 [https://github.com/hdinsight/hdinsight-spark-examples](https://github.com/hdinsight/hdinsight-spark-examples)します。 


##<a name="seealso"></a>関連項目


* [概要: Azure HDInsight での Apache Spark](hdinsight-apache-spark-overview.md)
* [クイック スタート: HDInsight の Apache Spark のプロビジョニングと Spark SQL を使用した対話型クエリの実行](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)
* [Machine Learning アプリケーションを作成するための HDInsight での Spark の使用](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [BI ツールを使用して HDInsight で Spark を使用して対話型データ分析を実行します。](hdinsight-apache-spark-use-bi-tools.md)
* [Azure HDInsight での Apache Spark クラスターのリソースの管理](hdinsight-apache-spark-resource-manager.md)


[hdinsight-versions]: ../hdinsight-component-versioning/
[hdinsight-upload-data]: ../hdinsight-upload-data/
[hdinsight-storage]: ../hdinsight-use-blob-storage/

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[azure-management-portal]: https://manage.windowsazure.com/
[azure-create-storageaccount]: ../storage-create-storage-account/ 


