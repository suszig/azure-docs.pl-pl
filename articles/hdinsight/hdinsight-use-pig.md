<properties
   pageTitle="HDInsight での Hadoop Pig の使用 | Microsoft Azure"
   description="HDInsight で Pig と Hadoop を使用する方法を説明します。"
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


# HDInsight での Pig と Hadoop の使用

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

[Apache Pig](http://pig.apache.org/) と呼ばれる手続き型言語を使用して Hadoop のプログラムを作成するためのプラットフォームは、 *Pig Latin*します。 Pig は、*MapReduce* ソリューションを作成するために Java の代わりに使用され、Azure HDInsight に含まれています。

この記事では、HDInsight での Pig の使用方法を説明します。

## <a id="why"></a>Pig を使用する理由

Hadoop での MapReduce を使用したデータ処理における課題の 1 つは、map と reduce 関数のみを使用する処理ロジックの実装です。 複雑な処理では、多くの場合、望ましい結果を得るために、一緒にチェーンされている複数の MapReduce 操作に処理を分割する必要があります。

Pig では、map と reduce 関数のみの使用を強制するのではなく、データ フローに使用される一連の変換として処理を定義し、目的の出力を生成することができます。

Pig Latin 言語では、生の入力から 1 つ以上の変換を介して目的の出力を生成するデータ フローを記述できます。 Pig Latin プログラムはこの一般的なパターンに従います。

- **Load**: 操作対象のデータをファイル システムから読み取ります。
- **Transform**: データを操作します。
- **Dump または store**: データを画面に出力します。または、処理できるように保存します。

Pig Latin ではユーザー定義関数 (UDF) もサポートされています。これを使用して、Pig Latin でのモデル化が困難なロジックを実装する外部コンポーネントを呼び出すことができます。

Pig Latin の詳細については、次を参照してください。 [Pig Latin Reference Manual 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) と [Pig Latin Reference Manual 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html)します。

Pig での UDF の使用例については、以下のドキュメントを参照してください。

* [HDInsight での Pig の使用 DataFu](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu Apache で管理されている便利な Udf のコレクションは、

* [Python と Pig および HDInsight での Hive を使用します。](hdinsight-python.md)

* [C# と Hive および HDInsight での Pig の使用します。](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>サンプル データについて

この例では、*log4j* サンプル ファイル (BLOB ストレージ コンテナーの **/example/data/sample.log** に格納されている) を使用します。 ファイル内の各ログを含むフィールド行から成る、 `[LOG LEVEL]` 型と、重要度を次に例を表示するフィールド。

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

前の例では、ログ レベルは ERROR です。
> [AZURE.NOTE] 使用して log4j ファイルを生成することも、 [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) ログ ツールと、そのファイルを blob にアップロードします。 参照してください [データを HDInsight にアップロード](hdinsight-upload-data.md) についてです。 HDInsight と共に Azure ストレージ内の blob を使用する方法の詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージの使用](../hdinsight-use-blob-storage.md)します。

サンプル データは、HDInsight が Hadoop クラスターの既定のファイル システムとして使用する Azure BLOB ストレージに格納されています。 HDInsight では、**wasb** プレフィックスを使用して、BLOB に格納されたファイルにアクセスすることができます。 たとえば、sample.log ファイルにアクセスするには、次の構文を使用します。

    wasb:///example/data/sample.log

WASB が HDInsight の既定のストレージであるため、Pig Latin から **/example/data/sample.log** を使用してファイルにアクセスすることもできます。
> [AZURE.NOTE] 構文は **wasb:///**, 、HDInsight クラスターの既定のストレージ コンテナーに格納されているファイルにアクセスするために使用します。 たとえばコンテナー名とストレージ アカウント アドレスを指定することによって、データにアクセスする場合は、クラスターをプロビジョニングして、これらのアカウントに格納されているファイルにアクセスするときに、追加のストレージ アカウントを指定したことができます: **wasb://mycontainer@mystorage.blob.core.windows.net/example/data/sample.log**します。


## <a id="job"></a>サンプル ジョブについて

次の Pig Latin ジョブでは、HDInsight クラスターの既定のストレージから **sample.log** ファイルを読み込みます。 次に、一連の変換を実行します。その結果、入力データの各ログ レベルの出現回数がカウントされます。 この結果は STDOUT にダンプされます。

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

次の図は、データへの各変換の詳細を示しています。

![変換のグラフィカル表示][image-hdi-pig-data-transformation]

## <a id="run"></a>Pig Latin ジョブを実行します。

HDInsight では、さまざまな方法を使用して Pig Latin ジョブを実行できます。 次の表を使用して、適切な方法を判別してから、該当するチュートリアルのリンクをクリックしてください。

| 使用する**方法**| **対話型**シェルの有無| **バッチ**処理の有無| 使用する**クラスターのオペレーティング システム**| 使用元の**クライアントのオペレーティング システム**|
|:--------------------------------------------------------------|:---------------------------:|:-----------------------:|:------------------------------------------|:-----------------------------------------|
| [SSH](hdinsight-hadoop-use-pig-ssh.md)| ✔| ✔| Linux| Linux、Unix、Mac OS X、または Windows|
| [Curl](hdinsight-hadoop-use-pig-curl.md)| &nbsp;| ✔| Linux または Windows| Linux、Unix、Mac OS X、または Windows|
| [.NET SDK for Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md)| &nbsp;| ✔| Linux または Windows| Windows (現時点)|
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md)| &nbsp;| ✔| Linux または Windows| Windows|
| [リモート デスクトップ](hdinsight-hadoop-use-pig-remote-desktop.md)| ✔| ✔| Windows| Windows|


## オンプレミスの SQL Server Integration Services を利用した Azure HDInsight での Pig ジョブの実行

SQL Server Integration Services (SSIS) を利用して Pig ジョブを実行することもできます。 Azure Feature Pack for SSIS には、HDInsight の Pig ジョブと連動する次のコンポーネントがあります。


- [Azure の HDInsight Pig タスク ][pigtask]
- [Azure サブスクリプションの接続マネージャー ][connectionmanager]


SSIS を Azure Feature Pack について学ぶこと [ここ ][ssispack]します。


## <a id="nextsteps"></a>次のステップ

これで、HDInsight で Pig を使用する方法に関する説明は終わりです。次のリンクを使用して、Azure HDInsight を操作するその他の方法について調べることもできます。

* [[Hdinsight でのデータのアップロード] を HDInsight にデータをアップロードします。][hdinsight-upload-data]
* [[Hdinsight を使用して hive] HDInsight での Hive を使用します。][hdinsight-use-hive]
* [[Hdinsight での mapreduce の使用] を HDInsight で MapReduce ジョブを使用します。][hdinsight-use-mapreduce]


[check]: ./media/hdinsight-use-pig/hdi.checkmark.png 
[apachepig-home]: http://pig.apache.org/ 
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html 
[curl]: http://curl.haxx.se/ 
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx 
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx 
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx 
[hdinsight-storage]: ../hdinsight-use-blob-storage.md 
[hdinsight-upload-data]: hdinsight-upload-data.md 
[hdinsight-get-started]: ../hdinsight-get-started.md 
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md 
[hdinsight-use-hive]: hdinsight-use-hive.md 
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md 
[hdinsight-provision]: hdinsight-provision-clusters.md 
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk 
[powershell-install-configure]: ../install-configure-powershell.md 
[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx 
[image-hdi-log4j-sample]: ./media/hdinsight-use-pig/HDI.wholesamplefile.png 
[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif 
[image-hdi-pig-powershell]: ./media/hdinsight-use-pig/hdi.pig.powershell.png 
[image-hdi-pig-architecture]: ./media/hdinsight-use-pig/HDI.Pig.Architecture.png 

