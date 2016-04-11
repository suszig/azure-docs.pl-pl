<properties
    pageTitle="Linux ベースの HDInsight (Hadoop) に Giraph をインストールして使用する | Microsoft Azure"
    description="スクリプト アクションを使用して Linux ベースの HDInsight クラスターに Giraph をインストールする方法について説明します。 スクリプト アクションでは、クラスター構成を変更するか、サービスとユーティリティをインストールすることで、クラスターを作成時にカスタマイズできます。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="larryfr"/>

# HDInsight Hadoop クラスターに Giraph をインストールし、Giraph を使用して大規模なグラフを処理する

使用してあらゆる種類の Azure HDInsight で hadoop クラスターに Giraph をインストールできる **Script Action** クラスターのカスタマイズ。 Script Action では、クラスターの作成時のみ、スクリプトを実行してクラスターをカスタマイズできます。 詳細については、次を参照してください。 [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster-linux.md)します。

このトピックでは、Script Action を使用して Giraph をインストールする方法を学習します。 Giraph をインストールした後は、大規模なグラフを処理する多くの一般的なアプリケーションで Giraph を使用する方法についても説明します。

> [AZURE.NOTE] この記事の情報は、Linux ベースの HDInsight クラスターに固有です。 Windows ベースのクラスターを扱う方法については、次を参照してください [HDInsight Hadoop クラスター (Windows) での Giraph のインストール。](hdinsight-hadoop-giraph-install.md)

## <a name="whatis"></a>Giraph とは

[Apache Giraph](http://giraph.apache.org/) は、Hadoop でグラフの処理を実行するために使用でき、Azure HDInsight で使用できます。 グラフでは、オブジェクト間の関係 (インターネットのような大規模ネットワークでのルーター間の接続など) や、ソーシャル ネットワークでの人々の関係 (ソーシャル グラフとも呼ばれる) をモデル化します。 グラフの処理により、次のようなグラフ内のオブジェクト間の関係について推論できます。

- 現在の関係に基づいて潜在的な友人を識別する
- ネットワーク内の 2 台のコンピューター間の最短ルートを識別する
- Web ページのページ順位を計算する

##スクリプトの機能

このスクリプトは、次のアクションを実行します。

* Giraph を `/usr/hdp/current/giraph` にインストールする。
* `/example/jars/giraph-examples.jar` のように、`giraph-examples.jar` ファイルをクラスターの既定のストレージ (WASB) にコピーする。

## <a name="install"></a>スクリプト アクションを使用した Giraph のインストール

HDInsight クラスターに Giraph をインストールするサンプル スクリプトは読み取り専用の Azure ストレージ blob から入手 [https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh)します。 このセクションでは、Azure クラシック ポータルを使用してクラスターを作成する際に、サンプル スクリプトを使用する方法について説明します。

> [AZURE.NOTE] このスクリプトを使用してクラスターを作成するのに Azure PowerShell または HDInsight .NET SDK を使用することもできます。 これらのメソッドの使用に関する詳細については、次を参照してください。 [Script Action を使用する HDInsight のカスタマイズ クラスタ](hdinsight-hadoop-customize-cluster-linux.md)します。

1. 」の手順を使用して、クラスターの作成を開始 [作成の Linux ベースの HDInsight クラスター](hdinsight-provision-linux-clusters.md#portal), が、作成を完了できません。

2.  **オプションの構成** ブレードで、 **Script Action**, 、し、次の情報を提供します。

    * __名前__: スクリプト アクションのフレンドリ名を入力します。
    * __スクリプト URI__: https://hdiconfigactions.blob.core.windows.net/linuxgiraphconfigactionv01/giraph-installer-v01.sh
    * __HEAD__: このチェック ボックス
    * __ワーカー__: このチェック ボックス
    * __ZOOKEEPER__: Zookeeper ノードにインストールするには、このオプションをオンにします。
    * __パラメーター__: このフィールドは空白

3. 下部にある、 **Script Action**, を使用して、 **選択** 構成を保存] ボタンをクリックします。 最後に、使用して、 **選択** の下部にあるボタン、 **オプションの構成** オプションの構成情報の保存先のブレードです。

4. 」の説明に従って、クラスターの作成を続行 [作成の Linux ベースの HDInsight クラスター](hdinsight-hadoop-create-linux-clusters-portal.md)します。

## <a name="usegiraph"></a>HDInsight で Giraph を使用する方法

クラスターの作成が完了したら、次の手順を使用して、Giraph に含まれているサンプル SimpleShortestPathsComputation を実行します。 これにより、グラフのオブジェクト間の最短パスを見つけるための基本的な <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> が実装されます。

1. SSH を使用して HDInsight クラスターに接続します。

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    HDInsight での SSH の使用方法の詳細については、以下の記事を参照してください。

    * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

1. 次の使用という名前の新しいファイルを作成して __tiny_graph.txt__:

        nano tiny_graph.txt

    このファイルの内容として以下を使用します。

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    このデータ形式を使用して、有向グラフ内のオブジェクト間の関係を示して [\_id \_value、[dest \_id]、[edge \_value],...]。 各行は、間にリレーションシップを表す、 **\_id** オブジェクトと 1 つ以上 **dest \_id** オブジェクトです。  **Edge \_value** (重み) の強さまたは距離の間の接続と見なすことができます **source_id** と **dest \_id**します。

    この値 (重み) を使用し、オブジェクト間の距離に応じて線を引くと、先のデータは次の図のようになります。

    ![円で表した tiny_graph.txt (線はオブジェクト間の距離)](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph.png)

2. ファイルを保存するには使用 __Ctrl + X__, 、し、 __Y__, 、最後に __Enter__ ファイル名を受け入れます。

3. 次のコマンドを使用して、HDInsight クラスターのプライマリ ストレージにデータを格納します。

        hadoop fs -copyFromLocal tiny_graph.txt /example/data/tiny_graph.txt

3. クラスターのヘッド ノードの完全修飾ドメイン名 (FQDN) を取得するには、次を使用します。

        hostname -f
        
4. 次のコマンドを使用して、サンプル SimpleShortestPathsComputation を実行します。 置換 __ヘッドノード__ 前の手順から返される、FQDN を使用します。

         hadoop jar /usr/hdp/current/giraph/giraph-examples.jar org.apache.giraph.GiraphRunner org.apache.giraph.examples.SimpleShortestPathsComputation -ca mapred.job.tracker=HEADNODE:9010 -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat -vip /example/data/tiny_graph.txt -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat -op /example/output/shortestpaths -w 2

    このコマンドで使用されるパラメーターを次の表に示します。

  	| パラメーター | 実行内容 |
  	| --------- | ------------ |
  	| `jar /usr/hdp/current/giraph/giraph-examples.jar` | サンプルを含む jar ファイル。 |
  	| `org.apache.giraph.GiraphRunner` | サンプルを開始するために使用するクラス。 |
  	| `org.apache.giraph.examples.SimpleShortestPathsCoputation` | 実行されるサンプル。 この場合は、ID 1 とグラフ内の他のすべての ID の間の最短パスを計算します。 |
  	| `-ca mapred.job.tracker=HEADNODE:9010` | クラスターのヘッド ノード。 |
  	| `-vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFromat` | 入力データに使用する入力形式。 |
  	| `-vip /example/data/tiny_graph.txt` | 入力データ ファイル。 |
  	| `-vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat` | 出力形式。 この場合は、ID と値をプレーンテキストとして出力します。 |
  	| `-op /example/output/shortestpaths` | 出力場所。 |
  	| `-w 2` | 使用する worker の数。 この場合は 2 です。 |

    Giraph サンプルで使用されるその他のパラメーターと、これらの詳細については、次を参照してください。、 [Giraph クイック スタート](http://giraph.apache.org/quick_start.html)します。

5. ジョブが完了すると、結果は、 __wasb:////例/アウト/shotestpaths__ ディレクトリ。 作成されたファイルが開始されます __一部-m -__ と 1 つ目、2 などのファイルを示す数値を終了します。 次のコマンドを使用して、出力を表示します。

        hadoop fs -text /example/output/shortestpaths/*

    出力は次のようになります。

        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    SimpleShortestPathComputation サンプルは、オブジェクト ID 1 から開始して他のオブジェクトへの最短パスを見つけるようにハードコーディングされています。 したがって、出力は `destination_id distance` として読み取られます。distance はオブジェクト ID 1 とターゲット ID の間を結ぶエッジの値 (重み) です。

    これを視覚化して、ID 1 と他のすべてのオブジェクトの間で最短パスを結ぶことにより、結果を検証できます。 ID 1 と ID 4 の最短パスは 5 です。 これは、<span style="color:orange">ID 1 と 3</span>、<span style="color:red">ID 3 と 4</span> を結ぶ距離の合計です。

    ![オブジェクトを円で表し、最短パスで結んだ図](./media/hdinsight-hadoop-giraph-install-linux/giraph-graph-out.png)


## 次のステップ

- [インストールして HDInsight クラスターで色合いを使用する](hdinsight-hadoop-hue-linux.md)です。 Hue は、Pig ジョブや Hive ジョブの作成、実行、保存や、HDInsight クラスターの既定のストレージの参照を容易にする Web UI です。

- [インストールして HDInsight クラスターで Spark を使用する](hdinsight-hadoop-spark-install-linux.md): を使用する方法についてはクラスターのカスタマイズをインストールして HDInsight Hadoop クラスターで Spark を使用します。 Spark はビッグ データ分析アプリケーションのパフォーマンスを向上するメモリ内の処理をサポートする、オープン ソースの並列処理のフレームワークです。

- [HDInsight クラスターで R をインストール](hdinsight-hadoop-r-scripts-linux.md): を使用する方法についてはクラスターのカスタマイズをインストールして HDInsight Hadoop クラスターで R を使用します。 R は、統計計算用のオープン ソースの言語および環境です。 R は、数百の組み込み統計関数と、関数型プログラミングとオブジェクト指向のプログラミングの特徴を結合した独自のプログラミング言語を提供します。 また、広範なグラフィカル機能も提供します。

- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Solr をインストールします。 Solr は、格納されたデータに対して強力な検索操作を実行することができます。


