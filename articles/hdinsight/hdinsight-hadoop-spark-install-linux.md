<properties 
    pageTitle="スクリプト アクションを使用した Linux ベースの HDInsight (Hadoop) での Apache Spark のインストール | Microsoft Azure" 
    description="スクリプト アクションを使用して Linux ベースの HDInsight クラスターに Spark をインストールする方法について説明します。スクリプト アクションでは、クラスター構成を変更するか、サービスとユーティリティをインストールすることで、クラスターを作成時にカスタマイズできます。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="Blackmist" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/04/2015" 
    ms.author="larryfr"/>


# HDInsight Hadoop クラスターで Spark をインストールして使用する

このドキュメントでは、Script Action を使用して、Spark をインストールする方法を学習します。 Script Action では、クラスターの作成時のみ、スクリプトを実行してクラスターをカスタマイズできます。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ ][hdinsight-cluster-customize]します。 Spark をインストールした後で、HDInsight クラスターで Spark クエリを実行する方法も学習します。
> [AZURE.NOTE] HDInsight は、Spark をクラスターの種類としても提供します。つまり、Hadoop クラスターを変更しなくても、Spark クラスターを直接プロビジョニングすることができるようになりました。 ただし現在、これは Windows ベースのクラスターに限定されています。 Spark クラスターの種類を使用して、Spark バージョン 1.3.1 の HDInsight バージョン 3.2 クラスター (Windows ベース) を取得します。 詳細については、次を参照してください。 [HDInsight での Apache Spark を使ってみる](hdinsight-apache-spark-zeppelin-notebook-jupyter-spark-sql.md)します。

## <a name="whatis"></a>Spark とは何ですか。

<a href="http://spark.apache.org/docs/latest/index.html" target="_blank">Apache Spark</a> ビッグ データ分析アプリケーションのパフォーマンスを向上するメモリ内をサポートしているオープン ソースの並列処理のフレームワークを処理します。 Spark のメモリ内の計算能力により、機械学習とグラフ計算における反復的なアルゴリズムに対して、Spark は適切な選択肢となります。

Spark を使用して、従来のディスク ベースのデータ処理を実行することもできます。 Spark は、中間段階でのディスクへの書き込みを回避することで、従来の MapReduce フレームワークを向上しています。 また、Spark は Hadoop Distributed File System (HDFS) と Azure BLOB ストレージと互換性があるため、既存のデータは Spark を通じて簡単に処理できます。

このトピックでは、HDInsight クラスターをカスタマイズして Spark をインストールする方法について説明します。

## <a name="whatis"></a>Spark のバージョンをインストールできますか。

このトピックでは、Script Action のカスタム スクリプトを使用して、HDInsight クラスターに Spark をインストールします。 このスクリプトでは、Spark 1.5.1 をインストールします。

このスクリプトを変更するか、独自のスクリプトを作成してその他の Spark バージョンをインストールできます。

## スクリプトの機能

このスクリプトのインストールには、Spark バージョン 1.5.1 `/usr/hdp/current/spark`します。
> [AZURE.WARNING] HDInsight クラスターに既定でいくつかの Spark 1.3.1 バイナリがインストールされている場合があります。 これらは使用しないでください。今後の更新プログラムで HDInsight クラスターのイメージから削除される予定です。

## <a name="install"></a>スクリプト アクションを使用して Spark をインストールします。

HDInsight クラスターで Spark をインストールするサンプル スクリプトは読み取り専用の Azure ストレージ blob から入手 [https://hdiconfigactions.blob.core.windows.net/linuxsparkconfigactionv02/spark-installer-v02.sh](https://hdiconfigactions.blob.core.windows.net/linuxsparkconfigactionv02/spark-installer-v02.sh)します。 このセクションでは、Azure ポータルを使用してクラスターを作成する際に、サンプル スクリプトを使用する方法について説明します。
> [AZURE.NOTE] Azure PowerShell または HDInsight .NET SDK を使用し、このスクリプトを使用してクラスターを作成することもできます。 これらのメソッドの使用に関する詳細については、次を参照してください。 [Script Action を使用する HDInsight のカスタマイズ クラスタ](hdinsight-hadoop-customize-cluster-linux.md)します。

1. 」の手順を使用して、クラスターの作成を開始 [作成の Linux ベースの HDInsight クラスター](hdinsight-hadoop-create-linux-clusters-portal.md), が、作成を完了できません。

2. **[オプションの構成]** ブレードで **[スクリプト アクション]** を選択し、以下の情報を指定します。

    * __[名前]__: スクリプト アクションの表示名を入力します。
    * __スクリプト URI__: https://hdiconfigactions.blob.core.windows.net/linuxsparkconfigactionv02/spark-installer-v02.sh
    * __[ヘッド]__: このオプションをオンにします。
    * __[worker]__: このオプションをオフにします。
    * __[ZooKeeper]__: このオプションをオフにします。
    * __[パラメーター]__: このフィールドは空のままにします。
    > [AZURE.NOTE] Spark スクリプトの例では、コンポーネントをヘッド ノードにのみインストールしているため、他のノードの種類をオフにすることができます。

3. **[スクリプト アクション]** の下部で、**[選択]** を使用して構成を保存します。 最後に、**[オプションの構成]** ブレードの下部にある **[選択]** を使用して、オプションの構成情報を保存します。

4. 」の説明に従って、provisining クラスターを引き続き [作成の Linux ベースの HDInsight クラスター](hdinsight-provision-linux-clusters.md#portal)します。

## <a name="usespark"></a>HDInsight での Spark の使用方法

Spark は Scala、Python、および Java で API を提供します。 対話型の Spark シェルを使用して Spark クエリを実行することもできます。 クラスターの作成が完了したら、次を使用して HDInsight クラスターに接続します。

    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

HDInsight での SSH の使用方法の詳細については、以下の記事を参照してください。

* [Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-unix.md)

* [Windows から Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-windows.md)

接続されたら、Spark の使用に関する具体的な手順について、以下のセクションを参照します。

- [Spark シェルを使用して、対話型クエリを実行するには](#sparkshell)
- [Spark シェルを使用して Spark SQL クエリを実行するには](#sparksql)
- [スタンドアロンの Scala プログラムを使用します。](#standalone)

### <a name="sparkshell"></a>Spark シェルを使用して、対話型クエリを実行するには

1. 次のコマンドを実行して、Spark シェルを起動します。

         /usr/hdp/current/spark/bin/spark-shell --master yarn

    コマンドの実行が完了すると、Scala プロンプトが表示されます。

         scala>

5. Scala プロンプトで、下記のように Spark クエリを入力します。 このクエリは、クラスターに関連付けられている Azure BLOB ストレージの /example/data/gutenberg/ で利用可能な davinci.txt ファイル内の各単語が出現する回数をカウントします。

        val file = sc.textFile("/example/data/gutenberg/davinci.txt")
        val counts = file.flatMap(line => line.split(" ")).map(word => (word, 1)).reduceByKey(_ + _)
        counts.toArray().foreach(println)

6. 出力は次のようになります。

        (felt,,1)
        (axle-tree,1)
        (deals,9)
        (virtuous,4)

7. :q を入力して、Scala プロンプトを終了します。

        :q


### <a name="sparksql"></a>Spark シェルを使用して Spark SQL クエリを実行するには

Spark SQL では、Spark を使用して構造化照会言語 (SQL)、HiveQL、Scala で表されるリレーショナル クエリを実行することができます。 このセクションでは、Spark を使用した Hive テーブルのサンプルにおける Hive クエリの実行について確認します。 このセクションで使用する Hive テーブル (**hivesampletable**) は、クラスターを作成する際に既定で使用できます。

1. 次のコマンドを実行して、Spark シェルを起動します。

         /usr/hdp/current/spark/bin/spark-shell --master yarn

    コマンドの実行が完了すると、Scala プロンプトが表示されます。

         scala>

2. Scala プロンプトで Hive のコンテキストを設定します。 これは、Spark を使用して Hive クエリを操作するために必要になります。

        val hiveContext = new org.apache.spark.sql.hive.HiveContext(sc)

    > [AZURE.NOTE]  `sc` このステートメントでは、Spark シェルを開始するときに設定されている既定の Spark コンテキスト。

5. Hive コンテキストを使用して Hive クエリを実行し、コンソールに出力します。 クエリは、特定のメーカーのデバイス上のデータを取得し、レコードの数 20 までに制限します。

        hiveContext.sql("""SELECT * FROM hivesampletable WHERE devicemake LIKE "HTC%" LIMIT 20""").collect().foreach(println)

6. 出力次のように表示されます。

        [820,11:35:17,en-US,Android,HTC,Inspire 4G,Louisiana,UnitedStates, 2.7383836,0,1]
        [1055,17:24:08,en-US,Android,HTC,Incredible,Ohio,United States,18.0894738,0,0]
        [1067,03:42:29,en-US,Windows Phone,HTC,HD7,District Of Columbia,United States,null,0,0]

7. :q を入力して、Scala プロンプトを終了します。

        :q


### <a name="standalone"></a>スタンドアロンの Scala プログラムを使用します。

このセクションでは、サンプル データ ファイル (/example/data/gutenberg/davinci.txt) 内の文字 a と b を含む行の数をカウントする Scala アプリケーションを作成します。

1. 次のコマンドを使用して Scala Build Tool をインストールします。

        echo "deb http://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
        sudo apt-get update
        sudo apt-get install sbt

    メッセージが表示されたら、__Y__ キーを押して続行します。

2. Scala プロジェクトのディレクトリ構造を作成します。

        mkdir -p SimpleScalaApp/src/main/scala

3. __simple.sbt__ という名前の新しいファイルを作成します。このファイルには、このプロジェクトの構成情報が含まれます。

     cd SimpleScalaApp
     nano simple.sbt

 このファイルの内容として、次のコードを使用します。

     name := "SimpleApp"
    
     version := "1.0"
    
     scalaVersion := "2.10.4"
    
     libraryDependencies += "org.apache.spark" %% "spark-core" % "1.2.0"

 > [AZURE.NOTE] 各エントリの間には、空の行を必ず保持してください。

 __Ctrl + X__ キーを押した後、__Y__ キーと __Enter__ キーを押してファイルを保存します。

4. 次のコマンドを使用して、__SimpleScalaApp/src/main/scala__ ディレクトリに __SimpleApp.scala__ という名前の新しいファイルを作成します。

     nano src/main/scala/SimpleApp.scala

 このファイルの内容として、次のコードを使用します。

     /* SimpleApp.scala */
     import org.apache.spark.SparkContext
     import org.apache.spark.SparkContext._
     import org.apache.spark.SparkConf
    
     object SimpleApp {
       def main(args: Array[String]) {
         val logFile = "/example/data/gutenberg/davinci.txt"         //Location of the sample data file on Azure Blob storage
         val conf = new SparkConf().setAppName("SimpleApplication")
         val sc = new SparkContext(conf)
         val logData = sc.textFile(logFile, 2).cache()
         val numAs = logData.filter(line => line.contains("a")).count()
         val numBs = logData.filter(line => line.contains("b")).count()
         println("Lines with a: %s, Lines with b: %s".format(numAs, numBs))
       }
     }

 __Ctrl + X__ キーを押してから、__Y__ キーと __Enter__ キーを押してファイルを保存します。

5. __SimpleScalaApp__ ディレクトリで次のコマンドを使用し、アプリケーションをビルドして jar ファイルに格納します。

        sbt package

    アプリケーションがコンパイルされると、__SimpleScalaApp/target/scala-2.10** ディレクトリに **simpleapp_2.10-1.0.jar** ファイルが作成されます。

6. 次のコマンドを使用して、SimpleApp.scala プログラムを実行します。

        /usr/hdp/current/spark/bin/spark-submit --class "SimpleApp" --master yarn target/scala-2.10/simpleapp_2.10-1.0.jar

4. プログラムの実行が完了すると、コンソールに出力が表示されます。

        Lines with a: 21374, Lines with b: 11430


## 次のステップ

- [HDInsight での色合いがクラスターのインストールおよび使用](hdinsight-hadoop-hue-linux.md)します。 Hue は、Pig ジョブや Hive ジョブの作成、実行、保存のほか、HDInsight クラスターの既定のストレージの参照を容易にする Web UI です。

- [インストール HDInsight に R をクラスターの ][hdinsight-install-r] はクラスターのカスタマイズを使用してインストールして HDInsight Hadoop クラスターで R を使用する方法について説明します。 R は、統計計算用のオープン ソースの言語および環境です。 R は、数百の組み込み統計関数と、関数型プログラミングとオブジェクト指向のプログラミングの特徴を結合した独自のプログラミング言語を提供します。 また、広範なグラフィカル機能も提供します。

- [HDInsight クラスターでの Giraph のインストール](hdinsight-hadoop-giraph-install-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Giraph をインストールします。 Giraph は、Hadoop でグラフの処理を実行するために使用でき、Azure HDInsight で使用できます。

- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Solr をインストールします。 Solr は、格納されたデータに対して強力な検索操作を実行することができます。

- [HDInsight クラスターでのインストールの色合い](hdinsight-hadoop-hue-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Hue をインストールします。 Hue は Hadoop クラスターとの情報のやりとりに使用される一連の Web アプリケーションです。




[hdinsight-install-r]: hdinsight-hadoop-r-scripts-linux.md 
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md 

