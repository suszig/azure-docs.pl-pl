<properties
    pageTitle="HDInsight での Apache Storm の概要 |Microsoft Azure"
    description="Apache Storm の概要と、HDInsight で Storm を使用した、クラウドでのリアルタイムのデータ分析ソリューションを構築する方法について説明します。"
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
   ms.date="11/06/2015"
   ms.author="larryfr"/>

#HDInsight での Apache Storm の概要: Hadoop のリアルタイム分析

HDInsight で Apache Storm を使用して、Azure 環境内で分散型のリアルタイム分析ソリューションを作成できます。 [Apache Hadoop](http://hadoop.apache.org)します。

##Apache Storm とは

Apache Storm は、Hadoop を使用して、リアルタイムでデータを処理できる、フォールト トレランスに優れたオープン ソースの分散型計算システムです。 また、Storm のソリューションは、最初に正常に処理されなかったデータを再生する機能を備え、保証されたデータ処理を実現します。

##HDInsight で Apache Storm を使用する理由

HDInsight の Apache Storm  は、管理されるクラスターとして、Azure 環境に統合されて提供されます。 次の主要な利点を提供します。

* 稼働時間 99.9% の SLA を使用した管理されたサービスとして実行します。

* 好みの言語を使用しますで書かれた Storm コンポーネントのサポートを提供 **Java**, 、**c#**, 、および **Python。**

    * さまざまなプログラミング言語をサポートしています Java を使用するデータの読み取り、C を使用して。#
    
        > [AZURE.NOTE] C# トポロジは、Windows ベースの HDInsight クラスターでのみサポートされます。

    * 使用して、 **Trident** 「1 回」のメッセージ、「トランザクション」のデータストア永続化、および一般的なストリーム分析操作のセットの処理をサポートする Storm トポロジを作成する Java インターフェイス

* 組み込みのスケールアップとスケールダウン機能 が含まれています。Storm トポロジの実行に影響を及ぼさずに、HDInsight クラスターを拡張します。

* Event Hub、Azure Virtual Network、SQL Database、Blob ストレージ、DocumentDB などの他の Azure のサービスと統合します。

    * Azure Virtual Network を使用して複数の HDInsight クラスターの機能を組み合わせることができます。HDInsight、HBase、Hadoop クラスターを使用する分析パイプラインを作成します。

リアルタイムの分析ソリューションの Apache Storm を使用している企業の一覧は、次を参照してください。 [Companies Using Apache Storm](https://storm.apache.org/documentation/Powered-By.html)します。

Storm の使用を開始するを参照してください。 [で Storm の使用開始][gettingstarted]します。

###プロビジョニングのしやすさ

HDInsight クラスター上に新しい Storm を数分でプロビジョニングできます。 クラスター名、サイズ、管理者アカウント、ストレージ アカウントを指定します。 Azure は、トポロジのサンプルや Web 管理ダッシュボードを含む、クラスターを作成します。

> [AZURE.NOTE] 使用して、Storm クラスターをプロビジョニングすることもできます。、 [Azure CLI](../xplat-cli-install.md) または [Azure PowerShell](../powershell-install-configure.md)します。

要求を送信すると、15 分以内には、新しい Storm クラスターが実行され、最初のリアルタイム分析のパイプラインの準備が完了します。

###使いやすさ

__HDInsight での Linux ベースの Storm クラスターため__, 、SSH と使用方法を使用してクラスターに接続できる、 `storm` の開始およびトポロジを管理するコマンドです。 さらに、Ambari を使用して Storm サービスを監視し、Storm UI を使用して実行中のトポロジを監視および管理できます。

Linux ベースの Storm クラスターを扱う方法の詳細については、次を参照してください。 [Linux ベースの HDInsight での Apache Storm を使ってみる](hdinsight-apache-storm-tutorial-get-started-linux.md)します。

__HDInsight での Windows ベースの Storm クラスターため__, 、HDInsight Tools for Visual Studio では、c# とハイブリッド C# で作成できます。/Java ベース トポロジと、HDInsight クラスターの Storm に送信するにします。  

![Storm プロジェクトの作成](./media/hdinsight-storm-overview/createproject.png)

HDInsight Tools for Visual Studio は、クラスター上の Storm トポロジを監視し、管理することができるインターフェイスも提供しています。

![Storm の管理](./media/hdinsight-storm-overview/stormview.png)

HDInsight Tools を使用して Storm アプリケーションを作成する例は、次を参照してください。 [HDInsight Tools for Visual Studio で開発 c# Storm トポロジ](hdinsight-storm-develop-csharp-visual-studio-topology.md)します。

Visual Studio の HDInsight ツールに関する詳細については、次を参照してください。 [Visual Studio の HDInsight ツールを使い始める](../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md)します。

HDInsight クラスター上の各 Storm には、クラスターで実行されている Storm トポロジを送信、監視、管理することができる Web ベースの Storm ダッシュボードも用意されています。

![Storm ダッシュボード](./media/hdinsight-storm-overview/dashboard.png)

Storm ダッシュ ボードの使用に関する詳細については、次を参照してください。 [展開し、HDInsight で Apache Storm トポロジを管理](hdinsight-storm-deploy-monitor-topology.md)します。

HDInsight での storm にも介して Azure Event Hubs と簡単に統合が用意されています、 **Event Hub スパウト**します。 これは、各 storm クラスターで利用可能な **%STORM_HOME%\examples\eventhubspout\eventhubs-storm-spout-0.9-jar-with-dependencies.jar**します。 Storm トポロジでのこのスパウトの使用例については、次のドキュメントをご覧ください。

* [Azure Event Hubs を使用する C# トポロジの開発](hdinsight-storm-develop-csharp-event-hub-topology.md)

* [Azure Event Hubs を使用する Java トポロジの開発](hdinsight-storm-develop-java-event-hub-topology.md)

###信頼性

Apache Storm ストームは、データ分析が多数のノードにまたがる場合でも、各着信メッセージを完全に処理することを常に保証します。

 **Nimbus ノード** Hadoop JobTracker に同様の機能を提供し、その他のノードを使用してクラスターにタスクを割り当てます **Zookeeper**します。 Zookeeper ノードは、クラスターの調整を提供し、Nimbus 間の通信を容易にするため、 **スーパーバイザー** ワーカー ノードで処理します。 処理中のノードの 1 つがダウンした場合、Nimbus ノードに通知され、タスクと関連付けられているデータが別のノードに割り当てられます。

Apache Storm の既定の構成では、Nimbus ノードは 1 つだけです。 HDInsight の Storm では、2 つの Nimbus ノードを実行します。 プライマリ ノードが失敗すると、プライマリ ノードが復旧中は、HDInsight クラスターはセカンダリ ノードに切り替わります。

![Nimbus、Zookeeper、スーパーバイザのダイアグラム](./media/hdinsight-storm-overview/nimbus.png)

###スケール

作成中にクラスター内のノード数を指定できますが、ワークロードに一致するようにクラスターを拡大、縮小できます。 すべての HDInsight クラスターで、データの処理中でも、クラスター内のノード数を変更できます。

> [AZURE.NOTE] スケーリングによって追加された新しいノードを利用するには、クラスター サイズを増やす前に開始されたトポロジを再調整する必要があります。

###サポート

HDInsight の Storm には、完全なエンタープライズ レベルの 24 時間 365 日のサポートが付属します。 HDInsight の Storm では、99.9% の SLA も保証されています。 つまり、クラスターは、99.9% 以上の時間において外部に接続されることを保証します。

##リアルタイム分析の一般的なユース ケース

以下に、HDInsight で Apache Storm を使用する一般的なシナリオをいくつか示します。 現実のシナリオについては、読み取り [企業に Storm を使用しているどの](https://storm.incubator.apache.org/documentation/Powered-By.html)します。

* モノのインターネット(IoT)
* 不正行為の検出
* ソーシャル分析
* Extract Transform Load (ETL)
* ネットワーク監視
* 検索
* Mobile Engagement


##HDInsight Storm のデータの処理方法

Apache Storm は **トポロジ** HDInsight や Hadoop でよく使用することが MapReduce ジョブではなく。 HDInsight クラスターでの Storm には、2 種類ノードにはが含まれています。 実行するヘッド ノード **Nimbus** および実行するワーカー ノード **スーパーバイザー**します。

* **Nimbus**: Hadoop の JobTracker と同様は、クラスター全体のコードの配布、仮想マシンへのタスクの割り当て、および障害の監視を担当します。 HDInsight は 2 つの Nimbus ノードを提供するため、HDInsight の Storm クラスターに単一障害点が生じません。

* **スーパーバイザー**: 各ワーカー ノードの supervisor はの開始と停止 **ワーカー プロセス** ノード上。

* **ワーカー プロセス**: のサブセットを実行、 **トポロジ**します。 実行中のトポロジは、クラスター内の多くのワーカー プロセスに分散されます。

* **トポロジ**: グラフを処理する計算を定義 **ストリーム** データ。 MapReduce ジョブとは異なり、トポロジは停止されるまで実行されます。

* **ストリーム**: バインドされていない集めた **組**します。 ストリームがによって生成される **スパウト** と **ボルト**, 、によって使用されていると **ボルト**します。

* **組**: 動的に型指定された値の名前付きのリスト。

* **スパウト**: データ ソースからデータを使用し、1 つまたは複数の出力 **ストリーム**します。

    > [AZURE.NOTE] 多くの場合、データは、Kafka などのキュー、Azure Service Bus キュー、Event hubs から読み取られます。 システムが停止した場合、キューが確実にデータを保持します。

* **ボルト**: 消費 **ストリーム**, 、に対して処理を実行 **組**, を発する場合もあり、 **ストリーム**します。 ボルトは、キュー、HDInsight HBase、BLOB、その他のデータ ストアなどの、外部ストレージへのデータの書き込みも処理します。

* **Apache Thrift**: 拡張性の高い、多言語サービス開発のためのソフトウェア フレームワークです。 これを使用すると、C++、Java、Python、PHP、Ruby、Erlang、Perl、Haskell、C#、Cocoa、JavaScript、Node.js、Smalltalk、その他の言語の間で動作するサービスを構築できます。

    * **Nimbus** Thrift サービスでは、および **トポロジ** ため、さまざまなプログラミング言語を使用してトポロジを開発することは Thrift の定義では、です。

Storm コンポーネントの詳細については、次を参照してください。、 [Storm チュートリアル][apachetutorial] apache.org にします。


##使用できるプログラミング言語

HDInsight クラスターの Stormでは、C#、Java、Python のサポートを提供します。

### C (&) #35；

HDInsight Tools for Visual Studio では、.NET 開発者は、C# のトポロジを設計し、実装できます。 Java と C# のコンポーネントを使用するハイブリッド トポロジを作成することもできます。

詳細については、次を参照してください。 [Visual Studio を使用した HDInsight での Apache Storm の c# を開発トポロジ](hdinsight-storm-develop-csharp-visual-studio-topology.md)します。

###Java

目にすることがあるほとんどの Java の例は、プレーンな Java か Trident です。 Trident は、結合、集計、グループ化、フィルタリングなどの実行を容易にするための、高レベルの抽象概念です。 ただし、Trident は、タプルのバッチに作用し、ここでは未加工の Java ソリューションが 1 回に 1 タプルずつストリームを処理します。

Trident の詳細については、次を参照してください。、 [Trident のチュートリアル](https://storm.incubator.apache.org/documentation/Trident-tutorial.html) apache.org にします。

Java および Trident トポロジの例については、次を参照してください。、 [Storm トポロジの例の一覧](hdinsight-storm-example-topology.md) または HDInsight クラスターの storm starter 例に示します。

Storm starter サンプル ディレクトリにある、_ _/usr/hdp/current/storm-client/contrib/storm-starter__ Linux ベースのクラスターで、 **%storm_home%\contrib\storm-starter** ディレクトリに Windows ベースのクラスター。

##一般的な開発パターンの例

###メッセージの処理の保証

Storm は、さまざまなレベルでメッセージの処理を保証します。 たとえば、基本的な Storm アプリケーションは "最低 1 回" の処理を保証できますが、Trident は "厳密に 1 回" の処理を保証できます。

詳細については、次を参照してください。 [データ処理の保証](https://storm.apache.org/about/guarantees-data-processing.html) apache.org にします。

###IBasicBolt

入力タプルを読み込み、0 または複数組、および出力し、確認、実行メソッドの最後にすぐに入力タプルのパターンは非常に一般的なと Storm を提供、 [IBasicBolt](https://storm.apache.org/apidocs/backtype/storm/topology/IBasicBolt.html) インターフェイスこのパターンを自動化します。

###結合

2 つのデータ ストリームの結合は、アプリケーションによって異なります。 たとえば、複数のストリームの各タプルを 1 つの新しいストリームに結合したり、特定のウィンドウに対してのみタプルのバッチを結合したりする場合があります。 どちらにしてへの参加を使用して実行することができます [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29), 、これは、ボルトへのタプルのルーティング方法を定義します。

コンポーネント「1」、「2」、「3」からに発信されるタプルのルーティングに fieldsGrouping を使用する次の Java の例、 **MyJoiner** ボルトです。

    builder.setBolt("join", new MyJoiner(), parallelism) .fieldsGrouping("1", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("2", new Fields("joinfield1", "joinfield2")) .fieldsGrouping("3", new Fields("joinfield1", "joinfield2"));

###バッチ処理

バッチ処理はいくつかの方法で実行できます。 基本的な Storm Java トポロジでは、単純なカウンターを使用して、X 個のタプルをバッチ処理してから発したり、tick タプルと呼ばれる内部の時間調整メカニズムを使用して X 秒ごとにバッチを発したりできます。

Tick タプルを使用しての例は、次を参照してください。 [Storm と HDInsight を使用したセンサー データを分析する](hdinsight-storm-sensor-data-analysis.md)です。

Trident を使用している場合、これはタプルのバッチ処理を基盤とします。

###キャッシュ

メモリ内キャッシュは、頻繁に使用されるアセットをメモリ内に保持することで、処理速度を上げるためのメカニズムとしてよく使用されます。 トポロジは、複数のノードと各ノード内の複数のプロセス間で分散型なので使用を検討する必要があります [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) のキャッシュの検索に使用されるフィールドを含むタプルが常に同じプロセスにルーティングされることを確認します。 これにより、プロセス間でのキャッシュ エントリの重複が回避されます。

###トップ N のストリーミング

トポロジが、Twitter のトレンドのトップ 5 など、"トップ" N の値の計算に依存する場合、トップ N の値を同時に計算し、計算の出力をグローバル値に結合する必要があります。 使用してこれ行う [fieldsGrouping](http://javadox.com/org.apache.storm/storm-core/0.9.1-incubating/backtype/storm/topology/InputDeclarer.html#fieldsGrouping%28java.lang.String,%20backtype.storm.tuple.Fields%29) にルーティングする方法、並列のボルトにフィールドの値で、データがパーティション分割)、フィールドで、トップ N の値で決定するボルトにルーティングします。

この例は、次を参照してください。、 [RollingTopWords](https://github.com/nathanmarz/storm-starter/blob/master/src/jvm/storm/starter/RollingTopWords.java) 例です。

##次のステップ

HDInsight で Apache Storm を使用したリアルタイム分析ソリューションの詳細について学習します。

* [HDInsight での Storm の使用][gettingstarted]

* [HDInsight 上の Storm に関するトポロジ例](hdinsight-storm-example-topology.md)

[stormtrident]: https://storm.incubator.apache.org/documentation/Trident-API-Overview.html
[samoa]: http://yahooeng.tumblr.com/post/65453012905/introducing-samoa-an-open-source-platform-for-mining
[apachetutorial]: https://storm.incubator.apache.org/documentation/Tutorial.html
[gettingstarted]: hdinsight-apache-storm-tutorial-get-started-linux.md

