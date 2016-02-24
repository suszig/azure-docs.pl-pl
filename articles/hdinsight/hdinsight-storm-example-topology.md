<properties
 pageTitle="HDInsight での Apache Storm トポロジの例 | Microsoft Azure"
 description="基本的な C# や Java トポロジなど、HDInsight で Apache Storm を使用して作成、テストされる Storm トポロジの例の一覧と、Event Hubs の使用。"
 services="hdinsight"
 documentationCenter=""
 authors="Blackmist"
 manager="paulettm"
 editor="cgronlun"
    tags="azure-portal"/>

<tags
 ms.service="hdinsight"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="big-data"
 ms.date="12/04/2015"
 ms.author="larryfr"/>

# HDInsight での Apache Storm の Storm トポロジとコンポーネントの例

HDInsight での Apache Storm の使用に関してマイクロソフトが作成して管理している例の一覧を次に示します。 これらの例では、基本的な C# および Java トポロジの作成から、Event Hubs、DocumentDB、Power BI、SQL Database、HDInsight での HBase、Azure Storage などの Azure サービスの使用まで、さまざまなトピックがカバーされています。 一部の例では、SignalR や Socket.IO など、Azure 以外、さらにはマイクロソフト以外のテクノロジを使用する方法についても説明されています。

| 説明                                                                                             | 対象                                         | 言語/フレームワーク         |
|:--------------------------------------------------------------------------------------------------------|:-----------------------------------------------------|:---------------------------|
| [Event Hub のスパウトとボルトのソース](https://github.com/apache/storm/tree/master/external/storm-eventhubs) | Event Hub のスパウトとボルトのソース | Java |
| [HDInsight で Apache Storm の Java ベース トポロジを開発します。][5797064f]                                 | Maven                                                | Java                       |
| [Visual Studio を使用した HDInsight で Apache Storm の c# トポロジを開発します。][16fce2d1]                     | HDInsight Tools for Visual Studio                    | C#、Java                   |
| [C# Storm トポロジで複数のデータ ストリームを作成します。][ec5a4064]                                         | 複数のストリーム                                     | C#                         |
| [HDInsight で Storm の使用の Twitter のトレンディング トピックします。][3c86c7c8]                                   | Trident                                              | Java、Trident              |
| [(C#) で Storm の使用の Azure Event Hubs からのイベントの処理][844d1d81]                                | Event Hubs                                           | C#、Java                |
| [HDInsight で Storm を使用して Azure Event Hubs のイベントを処理する (Java)](hdinsight-storm-develop-java-event-hub-topology.md) | Event Hubs | Java |
| [Power Bi (プレビュー) を使用して、Storm トポロジからデータを視覚化するには][94d15238]                              | Power BI                                             | C#                         |
| [Storm と HBase を HDInsight を使用したセンサー データの分析][ab894747]                                     | Event Hubs、HBase、Socket.IO、Web ダッシュボード          | C#、Java、JavaScript、HTML |
| [HDInsight で Storm を使用して Event Hubs からの車両センサー データを処理します。][246ee964]                        | Event Hubs、DocumentDb、Azure Storage Blob (WASB)    | C#、Java                   |
| [抽出、変換、および読み込み (ETL) Azure Event Hubs から HBase、HDInsight で Storm を使用するには][b4b68194] | Event Hubs、HBase                                    | C#                         |
| [HDInsight で Storm から Azure サービスを操作するためのテンプレート c# Storm トポロジ プロジェクト][ce0c02a2]  | Event Hubs、DocumentDb、SQL Database、HBase、SignalR | C#、Java                   |
| [HDInsight で Storm を使用して Azure Event Hubs から読み取るのための拡張性ベンチマーク][d6c540e3]           | メッセージ スループット、Event Hubs、SQL Database         | C#、Java                   |
| [HDInsight での Storm および HBase を使用した関連付け](hdinsight-storm-correlation-topology.md) | HBase | C# |
| [HDInsight の Storm での Python の使用](hdinsight-storm-develop-python-topology.md) | Java および Clojure ベースの Storm トポロジによる Python コンポーネント | Python |

## 次のステップ

* [HDInsight で Apache Storm を使用します。][2b8c3488]

* [展開し、HDInsight での Storm の Storm トポロジを管理する方法について説明します。][6eb0d3b8]

  [2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Learn how to create a Storm on HDInsight cluster and use the Storm Dashboard to deploy example topologies."
  [6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Learn how to deploy and manage topologies using the web-based Storm Dashboard and Storm UI or the HDInsight Tools for Visual Studio."
  [16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Learn how to create C# Storm topologies by using the HDInsight Tools for Visual Studio."
  [5797064f]: hdinsight-storm-develop-java-topology.md "Learn how to create Storm topologies in Java, using Maven, by creating a basic wordcount topology."
  [94d15238]: hdinsight-storm-power-bi-topology.md "Demonstrates how to write data to Power BI from a C# topology, then create a chart and dashboard from the data."
  [ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Demonstrates a basic Storm topology that performs a wordcount, implemented in C#. This also demonstrates how to create multiple data streams within a C# topology."
  [844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Learn how to read and write data from Azure Event Hubs with Storm on HDInsight."
  [ab894747]: hdinsight-storm-sensor-data-analysis.md "Learn how to use Apache Storm on HDInsight to process sensor data from Azure Event Hubs, visualize it using D3.js, and (optionally,) store it to HBase."
  [3c86c7c8]: hdinsight-storm-twitter-trending.md "Learn how to use Trident to create a Storm topology that determines trending topics (based on hashtags,) on Twitter."
  [246ee964]: hdinsight-storm-iot-eventhub-documentdb.md "Learn how to use a Storm topology to read messages from Azure Event Hubs, read documents from Azure DocumentDB for data referencing and save data to Azure Storage."
  [d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Several topologies to demonstrate throughput when reading from Azure Event Hubs and storing to SQL Database using Apache Storm on HDInsight."
  [b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Learn how to read data from Azure Event Hubs, aggregate & transform the data, then store it to HBase on HDInsight."
  [ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "This project contains templates for spouts, bolts and topologies to interact with various Azure services like Event Hubs, DocumentDB, and SQL Database."
 
