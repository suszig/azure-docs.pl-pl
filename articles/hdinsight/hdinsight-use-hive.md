<properties
    pageTitle="Hive と HiveQL の使用方法の説明 |Microsoft Azure"
    description="Apache Hive と HDInsight での Hadoop との使用方法について説明します。 Hive ジョブを実行し、HiveQL を使用して Apache log4j サンプル ファイルを分析する方法を選択します。"
    keywords="hiveql,what is hive"
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
    ms.date="12/03/2015"
    ms.author="larryfr"/>

# HDInsight で Hadoop と共に Hive と HiveQL を使用して Apache log4j サンプル ファイルを分析する

[AZURE.INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]


このチュートリアルでは、HDInsight の Hadoop での Apache Hive の使用方法と、Hive ジョブの実行方法の選択について説明します。 また、HiveQL と、Apache log4j サンプル ファイルを分析する方法についても説明します。

##<a id="why"></a>Hive の説明と使用する理由
[Apache Hive](http://hive.apache.org/) はデータ ウェアハウス システムでは、Hadoop の HiveQL (SQL に似たクエリの言語) を使用してデータを集約、照会、およびデータの分析をできるようにします。 Hive を使用することで、データを対話的に探索したり、再利用可能なバッチ処理ジョブを作成したりすることができます。

Hive では、大規模な非構造化データに構造を投影することができます。 構造を定義したら、Hive を使用することで、Java や MapReduce の知識がなくてもそのデータを照会できます。 **HiveQL** (Hive クエリ言語) では、T-SQL に似たステートメントを含むクエリを作成することができます。

Hive は、フィールドが特定の文字で区切られるテキスト ファイルなどの構造化データや半構造化データを操作する方法を認識しています。 Hive は、カスタムもサポートしています。 **シリアライザー/デシリアライザー (SerDe)** 複雑なまたは不規則な構造化されたデータ。 詳細については、次を参照してください。 [HDInsight でのカスタム JSON SerDe を使用する方法](http://blogs.msdn.com/b/bigdatasupport/archive/2014/06/18/how-to-use-a-custom-json-serde-with-microsoft-azure-hdinsight.aspx)します。

Hive では拡張も **ユーザー定義関数 (UDF)**します。 UDF では、HiveQL で簡単にモデル化されない機能やロジックを実装することができます。 Hive での UDF の使用例については、以下を参照してください。

* [HDInsight における Python と Hive および Pig の使用](hdinsight-python.md)

* [HDInsight における C#、Hive、Pig の使用](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

* [HDInsight にカスタムの Hive UDF を追加する方法](http://blogs.msdn.com/b/bigdatasupport/archive/2014/01/14/how-to-add-custom-hive-udfs-to-hdinsight.aspx)


## Hive の内部テーブルと外部テーブル

Hive の内部テーブルと外部テーブルについて知っておく必要のある事項がいくつかあります。

-  **CREATE TABLE** コマンドは、内部テーブルを作成します。 データ ファイルは既定のコンテナーに配置する必要があります。
-  **CREATE TABLE** コマンドでは、データ ファイルを移動する、ある/hive/warehouse/<TableName> をクリックします。
-  **CREATE EXTERNAL TABLE** コマンドは、外部テーブルを作成します。 データ ファイルは既定のコンテナーの外部に配置することもできます。
-  **CREATE EXTERNAL TABLE** コマンドは、データ ファイルを移動しません。
-  **CREATE EXTERNAL TABLE** コマンドの場所にフォルダーは許容されません。 チュートリアルで sample.log ファイルのコピーを作成しているのは、これが理由です。

詳細については、次を参照してください。 [HDInsight: Hive の内部と外部テーブル (入門)][cindygross-hive-tables]します。


##<a id="data"></a>サンプル データ Apache log4j ファイルについて

この例では、 *log4j* 格納されているサンプル ファイル **/example/data/sample.log** 、blob ストレージ コンテナーにします。 ファイル内の各ログは、タイプと重要度を表す `[LOG LEVEL]` フィールドを含むフィールド行で構成されています。以下に例を示します。

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

前の例では、ログ レベルは ERROR です。

> [AZURE.NOTE] 使用して log4j ファイルを生成することも、 [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) ログ ツールと、そのファイルを blob コンテナーにアップロードします。 参照してください [データを HDInsight にアップロード](hdinsight-upload-data.md) についてです。 HDInsight と共に Azure Blob ストレージを使用する方法の詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージの使用](../hdinsight-use-blob-storage.md)します。

サンプル データは、HDInsight が既定のファイル システムとして使用する Azure BLOB ストレージに格納されています。 HDInsight を使用して blob に格納されているファイルにアクセスできる、 **wasb** プレフィックス。 たとえば、sample.log ファイルにアクセスするには、次の構文を使用します。

    wasb:///example/data/sample.log

使用してファイルにアクセスする Azure Blob ストレージが HDInsight の既定のストレージであるため、またできます **/example/data/sample.log** HiveQL からです。

> [AZURE.NOTE] 構文は **wasb:///**, 、HDInsight クラスターの既定のストレージ コンテナーに格納されているファイルにアクセスするために使用します。 アドレスを指定して、コンテナー名とストレージ アカウント、たとえば、データをアクセスするには、クラスターをプロビジョニングして、これらのアカウントに格納されているファイルにアクセスするときに、追加のストレージ アカウントを指定した場合 **wasb://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**します。

##<a id="job"></a>サンプル ジョブ: 区切られたデータへの列の投影

次の HiveQL ステートメントに格納されている区切りデータに列を投影、 **wasb:////例/データ** ディレクトリ。

    DROP TABLE log4jLogs;
    CREATE EXTERNAL TABLE log4jLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
    STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
    SELECT t4 AS sev, COUNT(*) AS count FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log' GROUP BY t4;

前の例では、HiveQL ステートメントは次のアクションを実行します。

* **DROP TABLE**: テーブルが既に存在する場合は、テーブルとデータ ファイルを削除します。
* **CREATE EXTERNAL TABLE**: 新たに作成 **外部** ハイブ内のテーブルです。 外部テーブルは、Hive にテーブル定義のみを格納し、データは元の場所に元の形式で残します。
* **ROW FORMAT**: Hive にデータの形式です。 ここでは、各ログのフィールドは、スペースで区切られています。
* **STORED AS TEXTFILE LOCATION**: (example/data ディレクトリ) が格納されている Hive にデータを格納し、テキストとして格納されています。 データは 1 つのファイルに格納することも、ディレクトリ内の複数のファイルに分散することもできます。
* **選択**: すべての行の数を選択、列 **t4** 値が含まれています **[ERROR]**します。 これには、値が返さ **3** がこの値を含む 3 行あるためです。
* **INPUT__FILE__NAME のように '%.log'** -Hive にだけで終わるファイルからデータを返すする必要があります。 ログです。 これにより、検索はデータを含む sample.log ファイルに制限され、定義したスキーマに一致しない他のサンプル データ ファイルのデータを返すことができなくなります。

> [AZURE.NOTE] データの自動アップロード処理などの外部ソースから、または別の MapReduce 操作によって更新する基になるデータが予想され、最新のデータを使用するための Hive クエリをする場合に、外部テーブルを使用する必要があります。
>
> 外部テーブルを削除しても **いない** データの削除、テーブル定義のみが削除されます。

外部テーブルを作成した後、次のステートメントは、作成に使用する、 **内部** テーブルです。

    CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
    STORED AS ORC;
    INSERT OVERWRITE TABLE errorLogs
    SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]' AND INPUT__FILE__NAME LIKE '%.log';

これらのステートメントは次のアクションを実行します。

* **テーブル IF NOT EXISTS の作成**: が既に存在しない場合は、テーブルを作成します。  **外部** キーワードが使用されていない、これは内部テーブルであり、Hive データ ウェアハウスに格納され、Hive によって完全に管理されます。
* **STORED AS ORC**: Optimized Row Columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。
* **INSERT OVERWRITE ...選択**: 行を選択、 **log4jLogs** を含むテーブル **[ERROR]**, へのデータを挿入し、 **errorLogs** テーブルです。

> [AZURE.NOTE] 外部テーブルとは異なりは基になるデータの削除内部テーブルを削除してもします。

##<a id="usetez"></a>パフォーマンスを改善するための Apache Tez の使用方法

[Apache Tez](http://tez.apache.org) 規模で遥かに効率的に実行する Hive などのデータの処理を要するアプリケーションを可能にするフレームワークです。 HDInsight の最新リリースでは、Hive は Tez 上での実行がサポートされます。 Linux ベースの HDInsight クラスターでは、Tez は既定で有効になっています。

> [AZURE.NOTE] Tez は、現在無効 Windows ベースの HDInsight クラスターの既定では、有効にする必要があります。 Tez を活用するために、Hive クエリに次の値を設定する必要があります。
>
> ```set hive.execution.engine=tez;```
>
>これは、クエリの先頭に配置することで、クエリ単位で送信できます。 また、クラスターの作成時に構成値を設定することで、この機能が既定で有効になるようにクラスターを設定できます。 詳細についてを検索できます [HDInsight クラスターのプロビジョニング](hdinsight-provision-clusters.md)します。

 [Hive on tez」設計ドキュメント](https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez) の詳細については、実装の選択肢の数を含めるし、構成をチューニングします。


##<a id="run"></a>HiveQL ジョブの実行方法の選択

HDInsight では、さまざまな方法を使用して HiveQL ジョブを実行できます。 次の表を使用して、適切な方法を判別してから、該当するチュートリアルのリンクをクリックしてください。

| **これを使用して** する場合は.                                                     | .. 有無 **インタラクティブ** シェル | ...**バッチ** 処理 | .. 使用これ **クラスターのオペレーティング システム** | .. 使用元この **クライアントのオペレーティング システム** |
|:--------------------------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [Hive ビュー](hdinsight-hadoop-use-hive-ambari-view.md) | ✔ | ✔ | Linux | 任意 (ブラウザー ベース) |
| [(SSH セッションからの) Beeline コマンド](hdinsight-hadoop-use-hive-beeline.md)                                         |              ✔              |            ✔            | Linux                                     | Linux、Unix、Mac OS X、または Windows        |
| [(SSH セッションからの) Hive コマンド](hdinsight-hadoop-use-hive-ssh.md)                                         |              ✔              |            ✔            | Linux                                     | Linux、Unix、Mac OS X、または Windows        |
| [Curl](hdinsight-hadoop-use-hive-curl.md)                                       |           &nbsp;            |            ✔            | Linux または Windows                          | Linux、Unix、Mac OS X、または Windows        |
| [クエリ コンソール](hdinsight-hadoop-use-hive-query-console.md)                     |           &nbsp;            |            ✔            | Windows                                   | 任意 (ブラウザー ベース)                            |
| [HDInsight Tools for Visual Studio](hdinsight-hadoop-use-hive-visual-studio.md) |           &nbsp;            |            ✔            | Linux または Windows                          | Windows                                  |
| [Windows PowerShell](hdinsight-hadoop-use-hive-powershell.md)                   |           &nbsp;            |            ✔            | Linux または Windows                          | Windows                                  |
| [リモート デスクトップ](hdinsight-hadoop-use-hive-remote-desktop.md)                   |              ✔              |            ✔            | Windows                                   | Windows                                  |

## オンプレミスの SQL Server Integration Services を利用した Azure HDInsight での Hive ジョブの実行

SQL Server Integration Services (SSIS) を利用して Hive ジョブを実行することもできます。 Azure Feature Pack for SSIS には、HDInsight の Hive ジョブと連動する次のコンポーネントがあります。


- [Azure HDInsight Hive タスク][hivetask]
- [Azure サブスクリプション接続マネージャー][connectionmanager]


SSIS を Azure Feature Pack について学ぶこと [ここ][ssispack]します。


##<a id="nextsteps"></a>次のステップ

これで、Hive と、HDInsight での Hadoop との使用方法に関する説明は終わりです。次のリンクを使用して、Azure HDInsight を操作するその他の方法について調べることもできます。


- [HDInsight へのデータのアップロード][hdinsight-upload-data]
- [HDInsight での Pig の使用][hdinsight-use-pig]
- [HDInsight での MapReduce ジョブの使用][hdinsight-use-mapreduce]

[check]: ./media/hdinsight-use-hive/hdi.checkmark.png

[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-sdk-documentation]: http://msdnstage.redmond.corp.microsoft.com/library/dn479185.aspx

[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[apache-tez]: http://tez.apache.org
[apache-hive]: http://hive.apache.org/
[apache-log4j]: http://en.wikipedia.org/wiki/Log4j
[hive-on-tez-wiki]: https://cwiki.apache.org/confluence/display/Hive/Hive+on+Tez
[import-to-excel]: http://azure.microsoft.com/documentation/articles/hdinsight-connect-excel-power-query/
[hivetask]: http://msdn.microsoft.com/library/mt146771(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx

[hdinsight-use-pig]: hdinsight-use-pig.md
[hdinsight-use-oozie]: hdinsight-use-oozie.md
[hdinsight-analyze-flight-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md


[hdinsight-storage]: ../hdinsight-use-blob-storage.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-get-started]: ../hdinsight-get-started.md

[Powershell-install-configure]: ../install-configure-powershell.md
[powershell-here-strings]: http://technet.microsoft.com/library/ee692792.aspx

[image-hdi-hive-powershell]: ./media/hdinsight-use-hive/HDI.HIVE.PowerShell.png
[img-hdi-hive-powershell-output]: ./media/hdinsight-use-hive/HDI.Hive.PowerShell.Output.png
[image-hdi-hive-architecture]: ./media/hdinsight-use-hive/HDI.Hive.Architecture.png


[cindygross-hive-tables]: http://blogs.msdn.com/b/cindygross/archive/2013/02/06/hdinsight-hive-internal-and-external-tables-intro.aspx


