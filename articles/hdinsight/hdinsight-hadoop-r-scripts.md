<properties
    pageTitle="HDInsight で R を使用してクラスターをカスタマイズする | Microsoft Azure"
    description="スクリプトの操作を使用して R をインストールする方法について説明し、HDInsight クラスターでの R を使用します。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/02/2015"
    ms.author="jgao"/>

# HDInsight Hadoop クラスターに R をインストールして使用する


Script Action を使用して Windows ベースの HDInsight クラスターを R でカスタマイズする方法と、HDInsight クラスターで R を使用する方法について説明します。 Linux ベースのクラスターでの R の使用方法の詳細については、次を参照してください。 [インストールし、HDinsight Hadoop クラスター (Linux) での R を使用して](hdinsight-hadoop-r-scripts-linux.md)します。
 
Azure HDInsight のクラスター (Hadoop、Storm、HBase、Spark) の任意の種類に R をインストールするにを使用して *Script Action*します。 HDInsight クラスターで R をインストールするサンプル スクリプトは読み取り専用の Azure ストレージ blob から入手 [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)します。 

**関連記事:**

- [HDInsight Hadoop クラスターに R をインストールして使用する (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [HDInsight で Hadoop クラスターを作成](hdinsight-provision-clusters.md): HDInsight クラスターの作成に関する一般情報
- [Script Action を使って HDInsight クラスターのカスタマイズ][hdinsight-cluster-customize]: Script Action を使って HDInsight クラスターをカスタマイズする方法の一般情報
- [HDInsight 用の Script Action スクリプトの開発](hdinsight-hadoop-script-actions.md)

## R とは

この <a href="http://www.r-project.org/" target="_blank">統計計算用 R プロジェクト</a> 統計計算用のオープン ソースの言語および環境です。 R は、数百の組み込み統計関数と、関数型プログラミングとオブジェクト指向のプログラミングの特徴を結合した独自のプログラミング言語を提供します。 また、広範なグラフィカル機能も提供します。 R は、さまざまな分野におけるほぼすべての統計学専門家や科学者に推奨されるプログラミング環境です。

R は Azure BLOB Storage (WASB) と互換性があるので、そこに格納されているデータは HDInsight 上の R を使用して処理できます。  

## R のインストール

A [サンプル スクリプト](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1) HDInsight に R をインストールするクラスターが Azure ストレージの読み取り専用 blob から入手できます。 このセクションでは、Azure ポータルを使用してクラスターを作成する際に、サンプル スクリプトを使用する方法について説明します。

> [AZURE.NOTE] サンプル スクリプトは、HDInsight クラスター version 3.1 で導入されました。 HDInsight クラスター バージョンの詳細については、次を参照してください。 [HDInsight クラスター バージョン](../hdinsight-component-versioning/)です。

1. ポータルから HDInsight クラスターを作成するときにをクリックして **オプションの構成**, 、] をクリックし、 **Script Action**します。
2.  **Script Action** ] ページで、次の値を入力します。

    ![Script Action を使ってクラスターをカスタマイズする](./media/hdinsight-hadoop-r-scripts/hdi-r-script-action.png "Use Script Action to customize a cluster")

    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>名前</td>
            <td>たとえば、スクリプト アクションの名前を指定します。 <b>R のインストール</b>.</td></tr>
        <tr><td>スクリプト URI</td>
            <td>たとえば、クラスターをカスタマイズするために呼び出されるスクリプトへの URI を指定します。 <i>https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1</i></td></tr>
        <tr><td>ノードの種類</td>
            <td>カスタマイズ スクリプトが実行されるノードを指定します。 選択することができます。 <b>すべてのノード</b>。 <b>ヘッド ノードのみ]</b>, or <b>ワーカー ノード</b> のみです。
        <tr><td>パラメーター</td>
            <td>スクリプトで必要な場合は、パラメーターを指定します。 ただし、R をインストールするスクリプトではパラメーターが必要ないため、空白のままにすることができます。</td></tr>
    </table>

    複数のスクリプト操作を追加して、クラスターに複数のコンポーネントをインストールすることができます。 スクリプトを追加した後、チェック マークをオンにして、クラスターの作成を開始します。

Azure PowerShell や HDInsight .NET SDK を使用して、HDInsight に R をインストールするためにスクリプトを使用することもできます。 これらの手順については、この記事の後半で説明します。

## R スクリプトの実行
このセクションでは、HDInsight で Hadoop クラスター上の R スクリプトを実行する方法について説明します。

1. **クラスターへのリモート デスクトップ接続を確立**: ポータルから、r をインストールするには、作成したクラスターのリモート デスクトップを有効にして、クラスターに接続します。 手順については、「[RDP を使用した HDInsight クラスターへの接続](hdinsight-administer-use-management-portal.md#rdp)」をご覧ください。

2. **R コンソールを開く**: ヘッド ノードのデスクトップに R コンソールへのリンクは、R をインストールします。 それをクリックして、R コンソールを開きます。

3. **R スクリプトの実行**: R スクリプトは、貼り付け、選択して、ENTER キーを押してして、R コンソールから直接実行できます。 1 ～ 100 の数値を生成し、その結果に 2 を乗算する、単純なスクリプト例を示します。

        library(rmr2)
        library(rhdfs)
        ints = to.dfs(1:100)
        calc = mapreduce(input = ints, map = function(k, v) cbind(v, 2*v))
        from.dfs(calc)

最初の 2 行は、R と共にインストールされる RHadoop ライブラリを呼び出します。最後の行は、結果をコンソールに出力します。 出力は次のようになります。

    [1,]  1 2
    [2,]  2 4
    .
    .
    .
    [98,]  98 196
    [99,]  99 198
    [100,] 100 200


## Azure PowerShell を使用した R のインストール

参照してください [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell)します。  このサンプルでは、Azure PowerShell を使用して Spark をインストールする方法を示します。 使用するスクリプトをカスタマイズする必要があります。 [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1)します。

## .NET SDK を使用した R のインストール

参照してください [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell)します。 このサンプルでは、.NET SDK を使用して Spark をインストールする方法を示します。 使用するスクリプトをカスタマイズする必要があります。 [https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1](https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps11)します。


## 関連項目

- [HDInsight Hadoop クラスターに R をインストールして使用する (Linux)](hdinsight-hadoop-r-scripts-linux.md)
- [HDInsight で Hadoop クラスターを作成](hdinsight-provision-clusters.md): HDInsight クラスターの作成に関する一般情報
- [Script Action を使って HDInsight クラスターのカスタマイズ][hdinsight-cluster-customize]: Script Action を使って HDInsight クラスターをカスタマイズする方法の一般情報
- [HDInsight 用の Script Action スクリプトの開発](hdinsight-hadoop-script-actions.md)
- [インストールして HDInsight クラスターで Spark を使用する][hdinsight-install-spark]: Spark のインストールに関するスクリプト アクションのサンプル
- [HDInsight クラスターに Giraph をインストール](hdinsight-hadoop-giraph-install.md): Giraph のインストールに関するスクリプト アクションのサンプル
- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install-linux.md): Solr のインストールに関するスクリプト アクションのサンプルです。

[powershell-install-configure]: ../install-configure-powershell.md
[hdinsight-provision]: ../hdinsight-provision-clusters/
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster-linux.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install-linux.md

