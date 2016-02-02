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
> [AZURE.NOTE] この記事の手順に従うには、両方のバージョンの Azure ポータルを使用する必要があります。 使用する Event Hub を作成する、 [Azure ポータル](https://manage.windowsazure.com)します。 HDInsight Spark クラスターを操作するには、使用して、 [Azure プレビュー ポータル](https://ms.portal.azure.com/)します。  

**前提条件:**

次のものが必要です。

- Azure サブスクリプション。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- Apache Spark クラスター。 手順については、次を参照してください。 [Azure HDInsight でのクラスターの Apache Spark のプロビジョニング](hdinsight-apache-spark-provision-clusters.md)します。
- An [Azure Event Hub](service-bus-event-hubs-csharp-ephcs-getstarted.md).
- Microsoft Visual Studio 2013 がインストールされているワークステーション。 手順については、次を参照してください。 [Visual Studio のインストール](https://msdn.microsoft.com/library/e2h7fzkw.aspx)します。

## <a name="createeventhub"></a>Azure Event Hub を作成します。

1. [Azure ポータル](https://manage.windowsazure.com), [ **新規** > **Service Bus** > **Event Hub** > **カスタム作成**します。

2. **[新しい Event Hub の追加]** 画面で **Event Hub 名**を入力し、ハブを作成する **[リージョン]** を選択して、新しい名前空間を作成するか、既存の名前空間を選択します。 **矢印**をクリックして続行します。

    ![ウィザード ページ 1](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Create.Event.Hub.png "Create an Azure Event Hub")
    > [AZURE.NOTE] 待機時間とコストを削減するために、HDInsight の Apache Spark クラスターと同じ **[場所]** を選択する必要があります。

3. **[Event Hub の構成]** 画面で、**[パーティション数]** と **[メッセージのリテンション期間]** の値を入力して、チェック マークをクリックします。 この例では、パーティション カウントに 10 を、メッセージ保持に 1 を使用します。 パーティション数を書き留めておきます。この値は後で必要になります。

    ![ウィザード ページ 2](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Create.Event.Hub2.png "Specify partition size and retention days for Event Hub")

4. 作成した Event Hub をクリックし、**[構成]** をクリックして、Event Hub 用に 2 つのアクセス ポリシーを作成します。

   <table>
    <tr><th>名前</th><th>アクセス許可</th></tr>
    <tr><td>mysendpolicy</td><td>送信</td></tr>
    <tr><td>myreceivepolicy</td><td>リッスン</td></tr>
    </table>

    アクセス許可の作成後、ページの下部にある **[保存]** アイコンをクリックします。 これにより、このイベント ハブに対する送信 (**mysendpolicy**) とリッスン (**myreceivepolicy**) に使用する共有アクセス ポリシーが作成されます。

    ![ポリシー](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policies.png "Create Event Hub policies")

5. 同じページで、2 つのポリシーに対して生成されるポリシー キーを記録しておきます。 後で使用するので、これらのキーを保存します。

    ![ポリシー キー](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policy.Keys.png "Save policy keys")

6. **[ダッシュボード]** ページで、下部にある **[接続情報]** をクリックして、2 つのポリシーを使用する Event Hub に接続文字列を取得し、保存します。

    ![ポリシー キー](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Policy.Connection.Strings.png "Save policy connection strings")

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

## <a name="receivezeppelin"></a>Zeppelin を使用した HDInsight での Spark でメッセージを受信します。

このセクションで作成、 [Zeppelin](https://zeppelin.incubator.apache.org) ノートブックを HDInsight の Spark クラスターに、Event Hub からメッセージを受信します。

### ストリーミング アプリケーションのリソースを Zeppelin に割り当てる

Zeppelin でストリーミング アプリケーションを作成するとき、次の事項を考慮する必要があります。

* **Zeppelin に割り当てられる Event Hub パーティションとコア**。 前の手順では、いくつかのパーティションを含む Event Hub を作成しました。 下で作成する Zeppelin ストリーミング アプリケーションで、再度、同じ数のパーティションを指定します。 Zeppelin で Event Hub からデータをストリーミングするには、Zeppelin に割り当てるコアの数を Event Hub のパーティションの数の 2 倍にする必要があります。
* **Zeppelin に割り当てるコアの最小数**。 下で作成するストリーミング アプリケーションで、アプリケーションでストリーミングしするメッセージを保存する一時テーブルを作成します。 次に、Spark SQL ステートメントを利用し、この一時テーブルからメッセージを読み込みます。 Spark SQL ステートメントを実行するには、少なくとも 2 つのコアを Zeppelin に割り当てる必要があります。

上の 2つの要件を合わせると、次のようになります。

* Zeppelin に割り当てなければならないコアの最小数は 2 です。
* 割り当てられるコアの数は常に Event Hub のパーティションの数の 2 倍にする必要があります。

Spark クラスターのリソースを割り当てる方法については、次を参照してください。 [HDInsight で Apache Spark クラスターのリソースを管理](hdinsight-apache-spark-resource-manager.md)します。

### Zeppelin を利用してストリーミング アプリケーションを作成する

1. [Azure ポータル](https://portal.azure.com/), 、スタート ボードで、タイルをクリックして、Spark クラスターの場合、スタート画面に固定表示)。 **[すべて参照]** > **[HDInsight クラスター]** でクラスターに移動することもできます。

2. Spark クラスター ブレードで、**[クイック リンク]** をクリックし、**[クラスター ダッシュボード]** ブレードで **[Zeppelin Notebook]** をクリックします。 入力を求められたら、クラスターの管理者資格情報を入力します。
    > [AZURE.NOTE] ブラウザーで次の URL を開き、クラスターの Zeppelin Notebook にアクセスすることもできます。 __CLUSTERNAME__ をクラスターの名前に置き換えます。
    >
    > `https://CLUSTERNAME.azurehdinsight.net/zeppelin`

2. 新しい Notebook を作成します。 ヘッダー ウィンドウで **[Notebook]** をクリックし、ドロップダウンから **[Note の新規作成]** をクリックします。

    ![新しい Zeppelin Notebook を作成します](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.CreateNewNote.png "Create a new Zeppelin notebook")

    同じページの **[Notebook]** 見出しの下に、**Note XXXXXXXXX** で名前が始まる新しい Notebook が表示されます。 新しい Notebook をクリックします。

3. 新しい Notebook の Web ページで、見出しをクリックし、必要に応じて Notebook の名前を変更します。 Enter キーを押して名前の変更を保存します。 また、Notebook のヘッダーの右上隅に **[接続]** というステータスが表示されることを確認します。

    ![Zeppelin Notebook のステータス](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.NewNote.Connected.png "Zeppelin notebook status")

4. 新しい Notebook に既定で作成される空の段落に次のスニペットを貼り付け、Event Hub の構成を使用するようにプレースホルダーを置き換えます。 このスニペットでは、Event Hub からストリームを受け取り、**mytemptable** という名前の一時テーブルにストリームを登録します。 次のセクションでは、送信側アプリケーションを開始します。 その後は、テーブルから直接データを読み取ることができます。
 > [AZURE.NOTE] 次のスニペットでは、**eventhubs.checkpoint.dir** を既定のストレージ コンテナー内のディレクトリに設定する必要があります。 ディレクトリが存在しない場合、ストリーミング アプリケーションにより作成されます。 ように、ディレクトリへの完全パスを指定することができますか、"**wasb://container@storageaccount.blob.core.windows.net/mycheckpointdir/**"、または、ディレクトリへの相対パスだけなど"**/mycheckpointdir**"です。

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



## <a name="runapps"></a>アプリケーションを実行します。

1. Zeppelin Notebook から、スニペットを追加した段落を実行します。 **Shift + Enter** キーを押すか、または右上隅にある **[プレイ]** ボタンをクリックします。

    段落の右上隅にあるステータスが、[準備完了]、[保留中]、[実行中]、[完了] の順に進行します。 出力が同じ段落の下に表示されます。 スクリーンショットは次のようになります。

    ![スニペットの出力](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Zeppelin.Code.Output.png "Output of the snipet")

2. **Sender** プロジェクトを実行し、コンソール ウィンドウで **Enter** キーを押して Event Hub へのメッセージ送信を開始します。

3. Zeppelin Notebook の新しい段落に次のスニペットを入力し、Spark で受信したメッセージを読み取ります。

        %sql 
        select * from mytemptable limit 10

    次のスクリーン キャプチャは、**mytemptable** で受信したメッセージです。

    ![Zeppelin でメッセージを受信します](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Streaming.Event.Hub.Zeppelin.Output.png "Receive messages in Zeppelin notebook")

4. Spark SQL インタープリターを再起動して、アプリケーションを終了します。 上部の **[インタープリター]** タブをクリックし、Spark インタープリターの **[再起動]** をクリックします。

    ![Zeppelin インタープリターを再起動します](./media/hdinsight-apache-spark-csharp-apache-zeppelin-eventhub-streaming/HDI.Spark.Zeppelin.Restart.Interpreter.png "Restart the Zeppelin intepreter")

## <a name="sparkstreamingha"></a>高可用性のストリーミング アプリケーションを実行します。

Zeppelin を使用して HDInsight の Spark クラスターでストリーミング データを受け取るのは、アプリケーションのプロトタイプに適した方法です。 ただし、高可用性と復元性を備えた実稼動環境セットアップでストリーミング アプリケーションを実行するには、以下を行う必要があります。

1. クラスターに関連付けられたストレージ アカウントに、依存関係の jar をコピーします。
2. ストリーミング アプリケーションをビルドします。
3. クラスターに RDP 接続し、クラスターのヘッドノードにアプリケーションの jar をコピーします。
3. クラスターに RDP 接続し、クラスター ノードでアプリケーションを実行します。

次の手順およびサンプルのストリーミング アプリケーションを実行する方法についてに GitHub からダウンロードできます。 [https://github.com/hdinsight/hdinsight-spark-examples](https://github.com/hdinsight/hdinsight-spark-examples)します。


## <a name="seealso"></a>関連項目

* [Azure HDInsight での Apache Spark の概要:](hdinsight-apache-spark-overview.md)
* [クイック スタート: HDInsight と Spark SQL を使用して対話型クエリの実行での Apache Spark のプロビジョニング](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)
* [Machine learning アプリケーションを構築するための HDInsight の Spark を使用します。](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [BI ツールを使用して HDInsight で Spark を使用して対話型データ分析を実行します。](hdinsight-apache-spark-use-bi-tools.md)
* [Azure HDInsight で Apache Spark クラスターのリソースを管理します。](hdinsight-apache-spark-resource-manager.md)



[hdinsight-versions]: ../hdinsight-component-versioning/ 
[hdinsight-upload-data]: ../hdinsight-upload-data/ 
[hdinsight-storage]: ../hdinsight-use-blob-storage/ 
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/ 
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/ 
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/ 
[azure-management-portal]: https://manage.windowsazure.com/ 
[azure-create-storageaccount]: ../storage-create-storage-account/ 

