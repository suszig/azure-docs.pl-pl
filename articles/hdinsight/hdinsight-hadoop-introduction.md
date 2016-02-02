<properties
    pageTitle="クラウドでの Hadoop とはHDInsight の概要 | Microsoft Azure"
    description="クラウドでの Hadoop と、HDInsight でのその管理方法についてHadoop コンポーネントとビッグ データ分析について紹介します。"
    keywords="big data analysis,introduction to hadoop,what is hadoop,hadoop in the cloud"
    services="hdinsight"
    documentationCenter=""
    authors="cjgronlund"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/02/2015"
   ms.author="cgronlun"/>



# クラウドでの Hadoop とはHDInsight での Hadoop コンポーネントを使用したビッグ データ分析について説明します

ここでは、Azure HDInsight の Hadoop、エコシステム、ビッグ データについて概説します。HDInsight の Hadoop の概要、Hadoop のコンポーネント、一般的な用語、ビッグ データ分析のためのシナリオについて説明します。 また、HDInsight のクラウドで Hadoop を使用するためのチュートリアル、ドキュメント、およびリソースについても紹介します。

## HDInsight のクラウドでの Hadoop とは

Azure HDInsight は、管理された Apache Hadoop クラスターをクラウドにデプロイしてプロビジョニングするサービスです。ビッグ データの処理、分析、およびレポート生成を目的として設計された、信頼性と可用性の高いソフトウェア フレームワークを提供します。 HDInsight は、**Hortonworks Data Platform (HDP)** Hadoop ディストリビューションを使用します。 Hadoop という語は、多くの場合、複数のコンポーネントで構成された Hadoop のエコシステム全体を指し、このエコシステムには Apache HBase、Apache Spark、Apache Storm に加え、Hadoop という領域に関係するその他のテクノロジが含まれます。 参照してください [HDInsight の Hadoop エコシステムの概要](#overview) 以下詳細です。


## ビッグ データとは

ビッグ データとは、絶えず急速なペースで増加し、さまざまな非構造化形式と意味的文脈から成る巨大なデータ群を指しています。

ビッグ データは、Twitter フィードのテキストから、工業機器のセンサー情報、オンライン カタログでの消費者の閲覧、購入行動に関する情報に至るまでの、大規模なデジタル情報を表します。 ビッグ データの種類には、履歴データ (つまり、保存されたデータ) とリアルタイム データ (つまり、ソースから直接ストリーミングされるデータ) があります。

ビッグ データから実用的な情報や洞察を導くうえで必要なのは、関連するデータを収集して適切な質問をすることだけではありません。データは、アクセス可能であり、クリーンアップと分析を行った後実用的な方法で定義される必要があります。 そこで、HDInsight の Hadoop のビッグ データ分析の出番となります。


## <a name="overview"></a>HDInsight の Hadoop エコシステムの概要

HDInsight は、急激に拡大を続けている Apache Hadoop テクノロジ スタックの Microsoft Azure 上のクラウド実装であり、ビッグ データ分析用の信頼できるソリューションです。 これには、Apache Spark、HBase、Storm、Pig、Hive、Sqoop、Oozie、Ambari などの実装が含まれます。 また、HDInsight は、Power BI、Excel、SQL Server Analysis Services、SQL Server Reporting Services などのビジネス インテリジェンス (BI) ツールとも統合されます。

### Linux 上のクラスター

Azure HDInsight は、**Linux** のクラウドの Hadoop クラスターをデプロイおよびプロビジョニングします。 詳細については、下の表を参照してください。

 カテゴリ| Linux での Hadoop
---------| -------------------
 **クラスターの OS**| Ubuntu 12.04 Long Term Support (LTS)
 **クラスターの種類**| Hadoop、Spark、HBase、Storm
 **デプロイメント**| Azure ポータル、Azure CLI、Azure PowerShell
 **クラスター UI**| Ambari
 **リモート アクセス**| Secure Shell (SSH)、REST API、ODBC、JDBC



### Hadoop、HBase、Spark、Storm、およびカスタマイズしたクラスター

HDInsight には、Apache Hadoop、Spark、HBase、または Storm というクラスター構成が用意されています。 か、 [スクリプトのアクションでクラスターをカスタマイズする](hdinsight-hadoop-customize-cluster-linux.md)します。

* **Hadoop** (「クエリ」ワークロード): 信頼性の高いデータ ストレージと [HDFS](#HDFS), 、シンプルな [MapReduce](#mapreduce) のプログラミング モデルを処理し、同時にデータを分析します。

* * *<a target="_blank" href="http://spark.apache.org/">Apache Spark</a>* *: 並列処理のフレームワーク、ビッグ データ分析アプリケーションのパフォーマンスを向上するメモリ内処理をサポートするデータのストリーミング データ、SQL の動作が浮かぶし、機械学習します。 を参照してください [の概要: HDInsight での Apache Spark は何ですか?](hdinsight-apache-spark-overview.md)

* * *<a target="_blank" href="http://hbase.apache.org/">HBase</a>* * ("NoSQL"ワークロード): 大量の非構造化および半構造化データ - 十億行のランダム アクセスと強力な一貫性を提供する Hadoop に構築される NoSQL データベースが数百万列というをタイムアウトします。 参照してください [HDInsight HBase の概要](hdinsight-hbase-overview.md)します。

* * *<a  target="_blank" href="https://storm.incubator.apache.org/">Apache Storm</a>* * (「ストリーム」ワークロード): 大量のデータ ストリームを高速に処理するための分散型リアルタイム計算システムです。 Storm は、HDInsight で管理されるクラスターとして提供されます。 参照してください [Storm と Hadoop を使用してセンサー データの分析](hdinsight-storm-sensor-data-analysis.md)します。

#### カスタマイズ スクリプトの例

スクリプト アクションは、クラスターのプロビジョニング中に実行されるスクリプトです。スクリプト アクションを使用して、クラスターに追加コンポーネントをインストールできます。 Linux ベースのクラスターでは Bash スクリプトです。

HDInsight チームによって提供されるスクリプトの例は次のとおりです。

* [色合い](hdinsight-hadoop-hue-linux.md): クラスターとの対話に使用される web アプリケーションのセットです。 Linux クラスターのみ。

* [Giraph](hdinsight-hadoop-giraph-install-linux.md): グラフの処理やユーザーの間のリレーションシップをモデル化する処理です。

* [R](hdinsight-hadoop-r-scripts-linux.md): オープン ソース言語および machine learning で使用される統計計算用の環境です。

* [Solr](hdinsight-hadoop-solr-install-linux.md): データに対してフルテキスト検索ができるエンタープライズ規模の検索プラットフォームです。

独自の Script Action の開発方法の詳細については、次を参照してください。 [HDInsight での Script Action 開発](hdinsight-hadoop-script-actions-linux.md)します。

## Hadoop コンポーネントおよびユーティリティとは

HDInsight クラスターには、次のコンポーネントとユーティリティが含まれています。

* * *[Ambari](#ambari)* *: クラスターのプロビジョニング、管理、および監視します。

* * *[Avro](#avro)* * (Microsoft .NET Library for Avro): Microsoft .NET 環境向けのデータのシリアル化します。

* * *[Hive と HCatalog](#hive)* *: Structured Query Language (SQL) に似たクエリ処理、テーブルおよびストレージの管理レイヤー。

* * *[Mahout](#mahout)* *: 機械学習します。

* * *[MapReduce および YARN](#mapreduce)* *: 分散処理およびリソース管理。

* * *[Oozie](#oozie)* *: ワークフロー管理。

* * *[Phoenix](#phoenix)* *: HBase の上のリレーショナル データベース レイヤーです。

* * *[Pig](#pig)* *: MapReduce 変換のためのシンプルなスクリプト作成します。

* * *[Sqoop](#sqoop)* *: データのインポートし、エクスポートします。

* * *[Tez](#tez)* *: 規模で効率的に実行するデータ量の多い処理します。

* * *[ZooKeeper](#zookeeper)* *: 分散システムにおけるプロセスの調整します。

> [AZURE.NOTE] 特定のコンポーネントに関する情報とバージョン情報では、「 [HDInsight で提供される Hadoop クラスター バージョンの新機能ですか? ][component-versioning]

### <a name="ambari"></a>Ambari

Apache Ambari は、Apache Hadoop クラスターのプロビジョニング、管理、および監視を目的としています。 演算子ツールの直観的なコレクションおよび Hadoop の複雑さが見えないようにする信頼性の高い、一連の API が含まれており、クラスターの操作を単純化しています。 Linux ベースの HDInsight クラスターは、Ambari Web UI と Ambari REST API の両方を提供し、Windows ベースのクラスターは、REST API のサブセットを提供します。

参照してください [Ambari を使用した HDInsight の管理のクラスター](hdinsight-hadoop-manage-ambari.md) (Linux のみ)、 [Ambari API を使用した HDInsight のモニターの Hadoop クラスター](hdinsight-monitor-use-ambari-api.md), 、および <a target="_blank" href="https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md">Apache Ambari API リファレンス</a>します。

### <a name="avro"></a>Avro (Microsoft .NET Library for Avro)

Microsoft .NET Library for Avro は、Microsoft.NET 環境向けのシリアル化のために、Apache Avro のコンパクトなバイナリ データ交換形式を実装します。使用して <a target="_blank" href="http://www.json.org/">JavaScript Object Notation (JSON)</a> 別の言語の相互運用性を保証する言語に依存しないスキーマを定義する 1 つの言語でシリアル化された意味データの読み取り可能です。形式の詳細については記載されて、 <a target=_"blank" href="http://avro.apache.org/docs/current/spec.html">Apache Avro の仕様</a>します。
Avro ファイルの形式は、分散 MapReduce プログラミング モデルをサポートしています。 ファイル内の任意のポイントを検索し、特定のブロックから読み取りを開始できるという意味で、ファイルは "分割可能" です。 方法については、次を参照してください。 [Avro for Microsoft .NET ライブラリを使用してデータをシリアル化](hdinsight-dotnet-avro-serialization.md)します。


### <a name="hdfs"></a>HDFS

Hadoop 分散ファイル システム (HDFS) は、MapReduce と YARN と共に Hadoop エコシステムの中核を構成する分散ファイル システムです。 HDFS は、HDInsight の Hadoop クラスターの標準ファイル システムです。

### <a name="hive"></a>Hive と HCatalog

<a target="_blank" href="http://hive.apache.org/">Apache Hive</a> はデータ ウェアハウス ソフトウェアで使用すると、クエリおよび HiveQL と呼ばれる SQL に似た言語を使用して分散ストレージ内の大規模なデータセットを管理する Hadoop に組み込み済みです。 Hive は、Pig と同様、MapReduce 上の抽象化部分です。 Hive を実行すると、クエリを一連の MapReduce ジョブに変換します。 Hive は、概念的には Pig よりもリレーショナル データベース管理システムに近いため、より構造化されたデータでの使用に適しています。 非構造化データを操作する場合は Pig が適しています。 参照してください [HDInsight での Hadoop と Hive を使用して](hdinsight-use-hive.md)します。

<a target="_blank" href="https://cwiki.apache.org/confluence/display/Hive/HCatalog/">Apache HCatalog</a> は Hadoop のデータのリレーショナル ビューに表示されるテーブルおよびストレージの管理レイヤー。 HCatalog では、Hive SerDe (シリアライザー - デシリアライザー) を書き込める任意の形式で、ファイルの読み取りおよび書き込みを実行できます。

### <a name="mahout"></a>Mahout

<a target="_blank" href="https://mahout.apache.org/">Apache Mahout</a> は Hadoop 上で実行される機械学習アルゴリズムのスケーラブルなライブラリです。 統計の原則を使用して、機械学習アプリケーションは、データから学習すること、および過去の結果から将来の動作を決めることをシステムに教えます。 参照してください [Hadoop Mahout を使用した映画のリコメンデーションを生成](hdinsight-mahout.md)します。

### <a name="mapreduce"></a>MapReduce および YARN

Hadoop MapReduce は、大規模なデータセットを並列処理するアプリケーションを記述するためのソフトウェア フレームワークです。 MapReduce ジョブは、大規模なデータセットを分割し、データを処理のためにキーと値のペアに整理します。

Apache YARN は、次世代の MapReduce (MapReduce 2.0、または MRv2) で、リソース管理とジョブ スケジューリング/監視という JobTracker の 2 つの主要なタスクを別個のエンティティに分けたものです。

MapReduce の詳細については、次を参照してください。 <a target="_blank" href="http://wiki.apache.org/hadoop/MapReduce">MapReduce</a> 参照してください。 YARN の詳細については、次を参照してください。 <a target="_blank" href="http://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html">Apache Hadoop NextGen MapReduce (YARN)</a>します。

### <a name="oozie"></a>Oozie

<a target="_blank" href="http://oozie.apache.org/">Apache Oozie</a> は Hadoop ジョブを管理するワークフローおよび調整システムです。 Hadoop スタックと統合されていて、MapReduce、Pig、Hive、Sqoop の Hadoop ジョブをサポートしています。 Java プログラムやシェル スクリプトのような、システム特有のジョブのスケジュールを設定するのに使用することもできます。 参照してください [と Hadoop の時間ベースの Oozie コーディネーターの使用](hdinsight-use-oozie-coordinator-time.md)します。

### <a name="phoenix"></a>Phoenix

<a  target="_blank" href="http://phoenix.apache.org/">Apache Phoenix</a> は HBase 上のリレーショナル データベース レイヤーです。 Phoenix には JDBC ドライバーが含まれており、ユーザーはこれを使用して SQL テーブルを直接クエリおよび管理できます。 Phoenix は、クエリおよびその他のステートメントをネイティブ NoSQL の API 呼び出しに変換します。MapReduce を使用しないため、NoSQL ストア上でアプリケーションを高速化できます。 参照してください [HBase クラスターで使用する Apache Phoenix および SQuirreL](hdinsight-hbase-phoenix-squirrel.md)します。


### <a name="pig"></a>Pig

<a  target="_blank" href="http://pig.apache.org/">Apache Pig</a> ができるように Pig Latin と呼ばれるシンプルなスクリプト言語を使用して、大規模なデータセットに対して複雑な MapReduce 変換を実行する高レベル プラットフォームです。 Pig は、Hadoop 内で実行できるように Pig Latin スクリプトを変換します。 ユーザー定義関数 (UDF) を作成して Pig Latin を拡張できます。 参照してください [Hadoop、Apache ログ ファイルでの Pig の使用](hdinsight-use-pig.md)します。

### <a name="sqoop"></a>Sqoop

<a  target="_blank" href="http://sqoop.apache.org/">Apache Sqoop</a> 一括転送する Hadoop とリレーショナル データベース、SQL の間でデータまたはその他の構造化データ ストアでは、できるだけ効率的にするツールです。 参照してください [Hadoop と Sqoop の使用](hdinsight-use-sqoop.md)します。

### <a name="tez"></a>Tez

<a  target="_blank" href="http://tez.apache.org/">Apache Tez</a> は一般的なデータ処理の複雑な有向非循環グラフを実行する Hadoop YARN 上に構築されたアプリケーション フレームワークです。 これは、MapReduce の後を継ぐ柔軟で強力なフレームワークであり、Hive などのデータ量の多い処理を、より効率的かつ大規模に実行することができます。 参照してください ["の Hive の使用と HiveQL の"パフォーマンス向上のため Apache Tez を使用して](hdinsight-use-hive.md#usetez)します。


### <a name="zookeeper"></a>ZooKeeper

<a  target="_blank" href="http://zookeeper.apache.org/">Apache ZooKeeper</a> データ レジスタ (znode) の共有階層名前空間を使用して、大規模な分散システムのプロセスを調整します。 znode には、状態、場所、構成など、プロセスを調整するのに必要な少量のメタ情報が含まれます。

## <a name="advantage"></a>クラウドで Hadoop の利点

HDInsight の Hadoop は、Azure クラウド エコシステムの一部としていくつかのメリットを提供します。その一部を次に示します。

* Hadoop クラスターの自動プロビジョニング。 HDInsight クラスターは、手動で Hadoop クラスターを構成するよりも作成がはるかに簡単です。 詳細については、「 [HDInsight のプロビジョニングの Hadoop クラスター](hdinsight-hadoop-provision-linux-clusters.md)します。

* 最新の Hadoop コンポーネント。 詳細については、「 [HDInsight で提供される Hadoop クラスター バージョンの新機能ですか? ][component-versioning]します。

* クラスターの高い可用性と信頼性。 参照してください [HDInsight クラスターの Hadoop の可用性と信頼性](hdinsight-high-availability-linux.md) 詳細です。

* Hadoop 互換オプションの Azure BLOB ストレージによって実現される効率的で経済的なデータ ストレージ。 参照してください [Azure Blob ストレージの使用 HDInsight での Hadoop](hdinsight-hadoop-use-blob-storage.md) 詳細です。

* 含むその他の Azure サービスとの統合 [Web アプリの](../documentation/services/app-service/web/) と [SQL Database](../documentation/services/sql-database/)します。

* 低い導入コスト。 開始、 [無料評価版](/pricing/free-trial/), も参照してください [HDInsight の料金詳細](/pricing/details/hdinsight/)します。


詳細については、HDInsight で Hadoop 上の利点をご覧ください、  [HDInsight ][marketing-page]します。



## <a id="resources"></a>ビッグ データ分析、Hadoop、HDInsight に関する詳細な学習用リソース

クラウドおよびビッグ データ分析での Hadoop への入門は、次のリソースを使用して作成されました。


### HDInsight の Hadoop ドキュメント

* [HDInsight ドキュメント](http://azure.microsoft.com/documentation/services/hdinsight/): Azure HDInsight のドキュメント ページ。 記事、ビデオ、およびその他のリソースへのリンク。

* [Linux で HDInsight の概要](hdinsight-hadoop-linux-tutorial-get-started.md): Linux で HDInsight Hadoop クラスターをプロビジョニングしてサンプルの Hive クエリを実行するためのクイック スタート チュートリアル。

* [での Linux ベースの Storm の使用開始](hdinsight-apache-storm-tutorial-get-started-linux.md): HDInsight クラスターで Storm をプロビジョニングして Storm トポロジのサンプルを実行するためのクイック スタート チュートリアル。

* [Linux での HDInsight のプロビジョニング](hdinsight-hadoop-provision-linux-clusters.md): Azure ポータル、Azure CLI または Azure PowerShell での Linux で HDInsight Hadoop クラスターをプロビジョニングする方法について説明します。

* [Linux で HDInsight の使用](hdinsight-hadoop-linux-information.md): Azure でプロビジョニングされたクラスターの Hadoop Linux の使用に関するクイック ヒントを取得します。

* [Ambari を使用した HDInsight の管理のクラスター](hdinsight-hadoop-manage-ambari.md): 監視および Ambari Web または Ambari REST API を使用して HDInsight クラスターで Linux ベースの Hadoop を管理する方法について説明します。


### Apache Hadoop

* <a target="_blank" href="http://hadoop.apache.org/">Apache Hadoop</a>: フレームワークとなる複数のコンピューターで大規模なデータセットの分散処理するため、Apache Hadoop ソフトウェア ライブラリについて説明します。

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.0.4/hdfs_design.html">HDFS</a>: アーキテクチャおよび Hadoop 分散ファイル システム、Hadoop アプリケーションで使用される主要なストレージ システムの設計方法について説明します。

* <a target="_blank" href="http://hadoop.apache.org/docs/r1.2.1/mapred_tutorial.html">MapReduce チュートリアル</a>: 大量データを並列の計算ノードから成る大規模なクラスターを迅速に処理する Hadoop アプリケーションを記述するためのプログラミング フレームワークについて説明します。

### Azure 上の SQL Database:

* [Azure SQL Database](/documentation/services/sql-database/): ドキュメント、チュートリアル、および SQL Database のビデオです。

* [SQL Database、Azure ポータルに](sql-database-manage-portal.md): クラウドで SQL データベースを管理するための軽量で使いやすいデータベース管理ツールです。

* [SQL データベースの adventure Works](http://msftdbprodsamples.codeplex.com/releases/view/37304): SQL データベースのサンプル データベースのダウンロード ページです。

### Microsoft ビジネス インテリジェンス (Windows 上の HDInsight 用)

Power Query アドインまたは Microsoft Hive ODBC ドライバーを使用すれば、Excel、PowerPivot、SQL Server Analysis Services、SQL Server Reporting Services などの使い慣れたビジネス インテリジェンス (BI) ツールを HDInsight と連携して、データの取得、分析、レポート生成を行うことができます。

これらの BI ツールは、ビッグ データ分析に有用です。

* [Excel から Power Query を使用した Hadoop への接続](hdinsight-connect-excel-power-query.md): Excel 用 Microsoft Power Query を使用して、HDInsight クラスターに関連付けられているデータを格納する Azure ストレージ アカウントに Excel を接続する方法について説明します。

* [Excel から Microsoft Hive ODBC ドライバーでの Hadoop への接続](hdinsight-connect-excel-hive-ODBC-driver.md): Microsoft Hive ODBC ドライバーを使用した HDInsight からデータをインポートする方法について説明します。

* [Microsoft クラウド プラットフォーム](http://www.microsoft.com/server-cloud/solutions/business-intelligence/default.aspx): Office 365 の Power BI について学習し、SQL Server 評価版をダウンロードし、SharePoint Server 2013 と SQL Server BI をセットアップします。

* <a target="_blank" href="http://msdn.microsoft.com/library/hh231701.aspx">詳細については、SQL Server Analysis Services</a>します。

* <a target="_blank" href="http://msdn.microsoft.com/library/ms159106.aspx">SQL Server Reporting Services について学習</a>します。


### Hadoop のビッグ データ分析のソリューションを試してみる (Windows 上の HDInsight 用)

組織のビッグ データ分析を使用して、ビジネスの洞察を得ましょう。 次に例をいくつか示します。

* [分析 HVAC センサー データ](hdinsight-hive-analyze-sensor-data.md): HDInsight (Hadoop) で Hive を使用してセンサー データを分析する方法について説明し、Microsoft Excel でデータを視覚化します。 このサンプルでは、Hive を使用して、HVAC システムによって生成された履歴データを処理し、設定した温度を正確に維持できないシステムを識別する方法について説明します。

* [Web サイトのログを分析する HDInsight での Hive を使用して](hdinsight-hive-analyze-website-log.md): HDInsight で HiveQL を使用して、外部の web サイトと、ユーザーが経験した web サイト エラーの概要から 1 日の訪問の頻度を把握する web サイトのログを分析する方法について説明します。

* [センサー データを分析してリアルタイムでの Storm と HBase を HDInsight (Hadoop) で](hdinsight-storm-sensor-data-analysis.md): Azure Event Hubs からセンサー データを HDInsight で Storm クラスターを使用し、web ベースのダッシュ ボードのほぼリアルタイムの情報として処理されたセンサー データを表示するソリューションを作成する方法について説明します。



[marketing-page]: ../services/hdinsight/ 
[component-versioning]: hdinsight-component-versioning.md 
[zookeeper]: http://zookeeper.apache.org/ 

