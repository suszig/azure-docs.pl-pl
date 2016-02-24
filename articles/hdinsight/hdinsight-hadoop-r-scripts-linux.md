<properties
    pageTitle="Linux ベースの HDInsight での R のインストール | Microsoft Azure"
    description="R をインストールして使用し、Linux ベースの Hadoop クラスターをカスタマイズする方法について説明します。"
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

# HDInsight Hadoop クラスターに R をインストールして使用する

使用してあらゆる種類の HDInsight で hadoop クラスターに R をインストールする **Script Action** クラスターのカスタマイズ。 これにより、R を使用して強力な MapReduce/YARN プログラミング フレームワークをデプロイし、HDInsight に展開される Hadoop クラスター上で大量のデータを処理できます。

> [AZURE.NOTE] このドキュメントの手順では、Linux ベースの HDInsight クラスターが必要です。 Windows ベースのクラスターでの R の使用方法の詳細については、次を参照してください。 [インストールし、HDinsight Hadoop クラスター (Windows) での R を使用して](hdinsight-hadoop-r-scripts.md)します。

## R とは

この <a href="http://www.r-project.org/" target="_blank">統計計算用 R プロジェクト</a> 統計計算用のオープン ソースの言語および環境です。 R は、数百の組み込み統計関数と、関数型プログラミングとオブジェクト指向のプログラミングの特徴を結合した独自のプログラミング言語を提供します。 また、広範なグラフィカル機能も提供します。 R は、さまざまな分野におけるほぼすべての統計学専門家や科学者に推奨されるプログラミング環境です。

作成時に Script Action を使用してカスタマイズされた HDInsight の Hadoop クラスター上で R スクリプトを実行し、R 環境をインストールできます。 R は Azure BLOB ストレージ (WASB) と互換性があるので、そこに格納されているデータは HDInsight 上の R を使用して処理できます。

## スクリプトの機能

HDInsight クラスターへの R のインストールに使用するスクリプト アクションでは、次の Ubuntu パッケージがインストールされ、基本的な R インストールが提供されます。

* [r 基本](http://packages.ubuntu.com/precise/r-base): 基本の GNU R パッケージ
* [r 基本 dev](http://packages.ubuntu.com/precise/r-base-dev): 補助 GNU R パッケージ

次の RHadoop パッケージもインストールされ、MapReduce および HDFS との統合が実現します。

* [rmr2](https://github.com/RevolutionAnalytics/rmr2): により、R 開発者は、Hadoop MapReduce の使用
* [rhdfs](https://github.com/RevolutionAnalytics/rhdfs): R 開発者 Hadoop HDFS (HDInsight の WASB) を使用することができます

さらに、次の R パッケージがインストールされます。

| R パッケージ | 提供内容 |
| --------- | ---------------- |
| [rJava](https://cran.r-project.org/web/packages/rJava/index.html) | Java インターフェイスへの低レベル R |
| [Rcpp](https://cran.r-project.org/web/packages/Rcpp/index.html) | R と C++ の統合 |
| [RJSONIO](https://cran.r-project.org/web/packages/RJSONIO/index.html) | R オブジェクトと JSON の間のシリアル化/逆シリアル化 |
| [bitops](https://cran.r-project.org/web/packages/bitops/index.html) | 整数ベクトルのビット演算機能。 |
| [digest](https://cran.r-project.org/web/packages/digest/index.html) | R オブジェクトの暗号化ハッシュ ダイジェストの作成 |
| [functional](https://cran.r-project.org/web/packages/functional/index.html) | Curry、Compose などの高階関数 |
| [reshape2](https://cran.r-project.org/web/packages/reshape2/index.html) | データの柔軟な再構築と集計 |
| [stringr](https://cran.r-project.org/web/packages/stringr/index.html) | 一般的な文字列操作用の単純かつ整合性のあるラッパー |
| [plyr](https://cran.r-project.org/web/packages/plyr/index.html) | データの分割、適用、組み合わせのためのツール |
| [caTools](https://cran.r-project.org/web/packages/caTools/index.html) | 移動ウィンドウ統計、GIF、Base64、ROC AUC などのためのツール |
| [stringdist](https://cran.r-project.org/web/packages/stringdist/index.html) | 近似文字列マッチング関数および文字列距離関数 |

## スクリプト アクションを使用した R のインストール

 [Https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh) スクリプト アクションは、HDInsight クラスターで R をインストールするために使用します。 このセクションでは、Azure ポータルを使用してクラスターをプロビジョニングする際にこのスクリプトを使用する方法について説明します。

> [AZURE.NOTE] このスクリプトを使用してクラスターを作成するのに Azure PowerShell または HDInsight .NET SDK を使用することもできます。 これらのメソッドの使用に関する詳細については、次を参照してください。 [Script Action を使用する HDInsight のカスタマイズ クラスタ](hdinsight-hadoop-customize-cluster-linux.md)します。

1. 」の手順を使用してプロビジョニングを開始 [プロビジョニング Linux ベースの HDInsight クラスター](hdinsight-hadoop-provision-linux-clusters.md#portal), が、プロビジョニングを完了できません。

2.  **オプションの構成** ブレードで、 **Script Action**, 、し、次の情報を提供します。

    * __名前__: スクリプト アクションのフレンドリ名を入力します。
    * __スクリプト URI__: https://hdiconfigactions.blob.core.windows.net/linuxrconfigactionv01/r-installer-v01.sh
    * __HEAD__: このチェック ボックス
    * __ワーカー__: このチェック ボックス
    * __ZOOKEEPER__: Zookeeper ノードにインストールするには、このオプションをオンにします。
    * __パラメーター__: このフィールドは空白

3. 下部にある、 **Script Action**, を使用して、 **選択** 構成を保存] ボタンをクリックします。 最後に、使用して、 **選択** の下部にあるボタン、 **オプションの構成** オプションの構成情報の保存先のブレードです。

4. 」の説明に従って、クラスターのプロビジョニングを引き続き [プロビジョニング Linux ベースの HDInsight クラスター](hdinsight-hadoop-provision-linux-clusters.md#portal)します。

## R スクリプトの実行

クラスターがプロビジョニングを完了したら、次の手順に従い、R を使用してクラスター上で MapReduce 操作を実行します。

1. SSH を使用して HDInsight クラスターに接続します。

        ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net

    HDInsight での SSH の使用方法の詳細については、以下の記事を参照してください。

    * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)

    * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)

2. `username@hn0-CLUSTERNAME:~$` プロンプトで次のコマンドを入力し、R の対話型セッションを開始します。

        R

3. 次の R プログラムを入力します。 このプログラムでは 1 ～ 100 の数値を生成してから 2 倍します。

        library(rmr2)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))

    最初の行では、RHadoop ライブラリの rmr2 を呼び出しています。これは、MapReduce 操作で使用します。

    2 行目では、1 ～ 100 の値を生成し、`to.dfs` を使用して Hadoop ファイル システムに格納しています。

    3 行目では、rmr2 で提供される機能を使用して MapReduce プロセスを作成し、処理を開始しています。 処理が始まると、複数の行がスクロール表示されます。

4. 次に、以下のコードを使用して、MapReduce 出力の格納先になっていた一時パスを確認します。

        print(calc())

    この結果は、`/tmp/file5f615d870ad2` のようになります。 実際の出力を表示するには、次のコマンドを使用します。

        print(from.dfs(calc))

    出力は次のようになります。

        [1,]  1 2
        [2,]  2 4
        .
        .
        .
        [98,]  98 196
        [99,]  99 198
        [100,] 100 200

5. R を終了するには、次のように入力します。

        q()


## 次のステップ

- [インストールして HDInsight クラスターで色合いを使用する](hdinsight-hadoop-hue-linux.md)です。 Hue は、Pig ジョブや Hive ジョブの作成、実行、保存や、HDInsight クラスターの既定のストレージの参照を容易にする Web UI です。

- [インストールして HDInsight クラスターで Spark を使用する][hdinsight-install-spark] を使用する方法についてのクラスターをインストールして HDInsight Hadoop クラスターで Spark を使用してカスタマイズします。 Spark はビッグ データ分析アプリケーションのパフォーマンスを向上するメモリ内の処理をサポートする、オープン ソースの並列処理のフレームワークです。

- [HDInsight クラスターに Giraph をインストール](hdinsight-hadoop-giraph-install.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Giraph をインストールします。 Giraph は、Hadoop でグラフの処理を実行するために使用でき、Azure HDInsight で使用できます。

- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Solr をインストールします。 Solr は、格納されたデータに対して強力な検索操作を実行することができます。

- [HDInsight クラスターでの色合いのインストール](hdinsight-hadoop-hue-linux.md)します。 クラスターのカスタマイズを使用して、HDInsight Hadoop クラスターに Hue をインストールします。 Hue は Hadoop クラスターとの情報のやりとりに使用される一連の Web アプリケーションです。

[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install-linux.md

