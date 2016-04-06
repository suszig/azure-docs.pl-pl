<properties 
    pageTitle="HDInsight の Hadoop クラスターに Giraph をインストールして使用する | Microsoft Azure" 
    description="Giraph を使用して HDInsight クラスターをカスタマイズする方法について説明します。 スクリプトを使用して Giraph をインストールするために、Script Action の構成オプションを使用します。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="paulettm" 
    editor="cgronlun"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/02/2015" 
    ms.author="nitinme"/>

# HDInsight Hadoop クラスターに Giraph をインストールし、Giraph を使用して大規模なグラフを処理する

Script Action を使用して Windows ベースの HDInsight クラスターを Giraph でカスタマイズする方法と、Giraph を使用して大規模なグラフを処理する方法について説明します。 Linux ベースのクラスターに Giraph を使用する方法の詳細については、次を参照してください。 [HDInsight Hadoop クラスター (Linux) での Giraph のインストール](hdinsight-hadoop-giraph-install-linux.md)します。
 
使用して Azure HDInsight のクラスター (Hadoop、Storm、HBase、Spark) の任意の型に Giraph をインストールできる *Script Action*します。 HDInsight クラスターに Giraph をインストールするサンプル スクリプトは読み取り専用の Azure ストレージ blob から入手 [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)します。 サンプル スクリプトは、HDInsight クラスター version 3.1 でのみ機能します。 HDInsight クラスター バージョンの詳細については、次を参照してください。 [HDInsight クラスター バージョン](hdinsight-component-versioning.md)です。

**関連記事:**

- [HDInsight クラスターに Giraph をインストール](hdinsight-hadoop-giraph-install.md): Azure ポータルを使用して Giraph のインストール
- [HDInsight Hadoop クラスターに Giraph をインストールする (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [HDInsight で Hadoop クラスターを作成](hdinsight-provision-clusters.md): HDInsight クラスターの作成に関する一般情報です。
- [Script Action を使って HDInsight クラスターのカスタマイズ][hdinsight-cluster-customize]: Script Action を使って HDInsight クラスターをカスタマイズする方法の一般的な情報です。
- [HDInsight 用の Script Action スクリプトの開発](hdinsight-hadoop-script-actions.md)します。


## Giraph とは

<a href="http://giraph.apache.org/" target="_blank">Apache Giraph</a> は、Hadoop でグラフの処理を実行するために使用でき、Azure HDInsight で使用できます。 グラフでは、オブジェクト間の関係 (インターネットのような大規模ネットワークでのルーター間の接続など) や、ソーシャル ネットワークでの人々の関係 (ソーシャル グラフとも呼ばれる) をモデル化します。 グラフの処理により、次のようなグラフ内のオブジェクト間の関係について推論できます。

- 現在の関係に基づいて潜在的な友人を識別する
- ネットワーク内の 2 台のコンピューター間の最短ルートを識別する
- Web ページのページ順位を計算する
   
## ポータルを使用して Giraph をインストールする

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [HDInsight クラスターでの Giraph のインストール](hdinsight-hadoop-giraph-install.md)

1. 使用して、クラスターの作成を開始、 **カスタム作成** オプション」の説明に従って [カスタム オプションを使用した hdinsight クラスターの作成の Hadoop](hdinsight-provision-clusters.md#portal)します。 
2.  **Script Action** ページ、ウィザードのをクリックして **[スクリプト アクションの追加** 次に示すように、スクリプト アクションの詳細を提供します。

    ![Script Action を使ってクラスターをカスタマイズする](./media/hdinsight-hadoop-giraph-install-v1/hdi-script-action-giraph.png "Use Script Action to customize a cluster")
    
    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>名前</td>
            <td>スクリプト アクションの名前を指定します。 たとえば、 <b>Install Giraph</b>します。</td></tr>
        <tr><td>スクリプト URI</td>
            <td>クラスターをカスタマイズするために呼び出すスクリプトの Uniform Resource Identifier (URI) を指定します。 たとえば、 <i>https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1</i></td></tr>
        <tr><td>ノードの種類</td>
            <td>カスタマイズ スクリプトが実行されるノードを指定します。 選択する <b>のすべてのノード</b>, 、<b>ヘッド ノードのみ</b>, 、または <b>ワーカー ノードのみ</b>します。
        <tr><td>パラメーター</td>
            <td>スクリプトで必要な場合は、パラメーターを指定します。 Giraph をインストールするスクリプトではパラメーターが必要ないため、空白のままにすることができます。</td></tr>
    </table>    

    複数のスクリプト操作を追加して、クラスターに複数のコンポーネントをインストールすることができます。 スクリプトの追加後、チェックマークをクリックしてクラスターの作成を開始します。

Azure PowerShell または HDInsight .NET SDK を使用して、HDInsight に Giraph をインストールするためにスクリプトを使用することもできます。 これらの手順については、このトピックの後半で説明します。

## Giraph の使用

SimpleShortestPathsComputation サンプルを使用して、グラフのオブジェクト間の最短パスを見つけるための基本的な <a href = "http://people.apache.org/~edwardyoon/documents/pregel.pdf">Pregel</a> の実装を示します。 次の手順を使用して、サンプル データとサンプル jar ファイルをアップロードし、SimpleShortestPathsComputation サンプルでジョブを実行して、結果を表示します。

1. サンプル データ ファイルを Azure Blob ストレージにアップロードします。 という名前の新しいファイルを作成してローカルのワークステーションに **tiny_graph.txt**します。 このファイルには、次の行を含めます。

        [0,0,[[1,1],[3,3]]]
        [1,0,[[0,1],[2,2],[3,1]]]
        [2,0,[[1,2],[4,4]]]
        [3,0,[[0,3],[1,1],[4,4]]]
        [4,0,[[3,4],[2,4]]]

    tiny_graph.txt ファイルを HDInsight クラスターのプライマリ ストレージにアップロードします。 データをアップロードする方法の手順については、次を参照してください。 [HDInsight での Hadoop ジョブ用データのアップロード](hdinsight-upload-data.md)します。

    このデータ形式を使用して、有向グラフ内のオブジェクト間の関係を示して [\_id \_value、[dest \_id]、[edge \_value],...]。 各行は、間にリレーションシップを表す、 **\_id** オブジェクトと 1 つ以上 **dest \_id** オブジェクトです。  **Edge \_value** (重み) の強さまたは距離の間の接続と見なすことができます **source_id** と **dest \_id**します。

    この値 (重み) を使用し、オブジェクト間の距離に応じて線を引くと、先のデータは次の図のようになります。

    ![円で表した tiny_graph.txt (線はオブジェクト間の距離)](./media/hdinsight-hadoop-giraph-install-v1/giraph-graph.png)

    

4. SimpleShortestPathsComputation サンプルを実行します。 tiny_graph.txt ファイルを入力として使用し、次の Azure PowerShell コマンドレットを使用してサンプルを実行します。 これはインストールして構成したことが必要です。 [Azure PowerShell][powershell-install]します。

        $clusterName = "clustername"
        # Giraph examples jar
        $jarFile = "wasb:///example/jars/giraph-examples.jar"
        # Arguments for this job
        $jobArguments = "org.apache.giraph.examples.SimpleShortestPathsComputation",
                        "-ca", "mapred.job.tracker=headnodehost:9010",
                        "-vif", "org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat",
                        "-vip", "wasb:///example/data/tiny_graph.txt",
                        "-vof", "org.apache.giraph.io.formats.IdWithValueTextOutputFormat",
                        "-op",  "wasb:///example/output/shortestpaths",
                        "-w", "2"
        # Create the definition
        $jobDefinition = New-AzureHDInsightMapReduceJobDefinition
          -JarFile $jarFile
          -ClassName "org.apache.giraph.GiraphRunner"
          -Arguments $jobArguments
        
        # Run the job, write output to the Azure PowerShell window
        $job = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $jobDefinition
        Write-Host "Wait for the job to complete ..." -ForegroundColor Green
        Wait-AzureHDInsightJob -Job $job
        Write-Host "STDERR"
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardError
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $job.JobId -StandardOutput

    上記の例では置換 **clustername** Giraph をインストールした HDInsight クラスターの名前に置き換えます。

5. 結果を表示します。 2 つの出力ファイルに結果を格納は、ジョブが完了した後、 __wasb:////例/アウト/shotestpaths__ フォルダーです。 ファイルと呼びます __一部 m-00001__ と __一部 m-00002__します。 出力をダウンロードして表示するには、次の手順を実行します。

        $subscriptionName = "<SubscriptionName>"       # Azure subscription name
        $storageAccountName = "<StorageAccountName>"   # Azure Storage account name
        $containerName = "<ContainerName>"             # Blob storage container name

        # Select the current subscription
        Select-AzureSubscription $subscriptionName
        
        # Create the Storage account context object
        $storageAccountKey = Get-AzureStorageKey $storageAccountName | %{ $_.Primary }
        $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey

        # Download the job output to the workstation
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00001 -Context $storageContext -Force
        Get-AzureStorageBlobContent -Container $containerName -Blob example/output/shortestpaths/part-m-00002 -Context $storageContext -Force

    これが作成されます、 __例/出力/shortestpaths__ ワークステーション、およびその場所への出力ファイルのダウンロード、2 つの現在のディレクトリ内のディレクトリ構造です。

    使用して、 __Cat__ コマンドレットを使用、ファイルの内容を表示します。 

        Cat example/output/shortestpaths/part*

    出力は次のようになります。


        0   1.0
        4   5.0
        2   2.0
        1   0.0
        3   1.0

    The SimpleShortestPathComputation example is hard coded to start with object ID 1 and find the shortest path to other objects. So the output should be read as `destination_id distance`, where distance is the value (or weight) of the edges traveled between object ID 1 and the target ID.
    
    Visualizing this, you can verify the results by traveling the shortest paths between ID 1 and all other objects. Note that the shortest path between ID 1 and ID 4 is 5. This is the total distance between <span style="color:orange">ID 1 and 3</span>, and then <span style="color:red">ID 3 and 4</span>.

    ![Drawing of objects as circles with shortest paths drawn between](./media/hdinsight-hadoop-giraph-install-v1/giraph-graph-out.png) 







## Aure PowerShell を使用して Giraph をインストールする

参照してください [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md#call_scripts_using_powershell)します。  このサンプルでは、Azure PowerShell を使用して Spark をインストールする方法を示します。 使用するスクリプトをカスタマイズする必要があります。 [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)します。

## .NET SDK を使用して Giraph をインストールする

参照してください [をカスタマイズする HDInsight クラスターの Script Action を使って](hdinsight-hadoop-customize-cluster.md#call_scripts_using_azure_powershell)します。 このサンプルでは、.NET SDK を使用して Spark をインストールする方法を示します。 使用するスクリプトをカスタマイズする必要があります。 [https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1](https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1)します。


## 関連項目

- [HDInsight クラスターに Giraph をインストール](hdinsight-hadoop-giraph-install.md): Azure ポータルを使用して Giraph のインストール
- [HDInsight Hadoop クラスターに Giraph をインストールする (Linux)](hdinsight-hadoop-giraph-install-linux.md)
- [HDInsight で Hadoop クラスターを作成](hdinsight-provision-clusters.md): HDInsight クラスターの作成に関する一般情報です。
- [Script Action を使って HDInsight クラスターのカスタマイズ][hdinsight-cluster-customize]: Script Action を使って HDInsight クラスターをカスタマイズする方法の一般的な情報です。
- [HDInsight 用の Script Action スクリプトの開発](hdinsight-hadoop-script-actions.md)します。
- [インストールして HDInsight クラスターで Spark を使用する][hdinsight-install-spark]: Spark のインストールに関するスクリプト アクションのサンプルです。
- [HDInsight クラスターで R をインストール][hdinsight-install-r]: R. のインストールに関するスクリプト アクションのサンプル
- [HDInsight クラスターに Solr をインストール](hdinsight-hadoop-solr-install.md): Solr のインストールに関するスクリプト アクションのサンプルです。

[tools]: https://github.com/Blackmist/hdinsight-tools
[aps]: http://azure.microsoft.com/documentation/articles/install-configure-powershell/

[powershell-install]: ../powershell-install-configure.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-cluster-customize]: hdinsight-hadoop-customize-cluster.md
 

