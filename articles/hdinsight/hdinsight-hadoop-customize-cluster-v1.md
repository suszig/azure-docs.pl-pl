<properties 
    pageTitle="Script Action を使った HDInsight クラスターのカスタマイズ | Microsoft Azure" 
    description="Script Action を使って HDInsight クラスターをカスタマイズする方法について説明します。" 
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
    ms.date="11/29/2015" 
    ms.author="nitinme"/> 

# Script Action を使って HDInsight クラスターをカスタマイズする

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [Script Action を使って HDInsight クラスターをカスタマイズする](hdinsight-hadoop-customize-cluster.md)

HDInsight という構成オプションを提供する **Script Action** プロビジョニング処理中に、クラスターで実行するカスタマイズを定義するカスタム スクリプトを呼び出します。 これらのスクリプトを使用して、クラスター上に追加のソフトウェアをインストールしたり、クラスター上のアプリケーションの構成を変更したりできます。 


> [AZURE.NOTE] Script Action は HDInsight クラスター version 3.1 以降では、Windows オペレーティング システムでのみサポートされます。  HDInsight クラスター バージョンの詳細については、次を参照してください。 [HDInsight クラスター バージョン](hdinsight-component-versioning.md)です。
> 
> Script Action は標準の Azure HDInsight のサブスクリプションの一部として追加料金なしで利用可能です。

HDInsight クラスターは、その他さまざまな方法でカスタマイズできます。たとえば、Azure ストレージ アカウントの追加、Hadoop 構成ファイルの変更 (core-site.xml、hive-site.xml など)、クラスター内の一般的な場所への共有ライブラリの追加 (Hive、Oozie など) といった方法があります。 これらのカスタマイズは、Azure PowerShell、Azure HDInsight .NET SDK、Azure クラシック ポータルから実行できます。 詳細については、次を参照してください。 [カスタム オプションを使用した HDInsight のプロビジョニングの Hadoop クラスター][hdinsight-provision-cluster]します。

## クラスターのプロビジョニング処理での Script Action

Script Action は、クラスターが作成中にのみ使用されます。 次の図は、プロビジョニング処理中に Script Action が実行された場合を示しています。

![クラスター プロビジョニング時の HDInsight クラスターのカスタマイズと段階][img-hdi-cluster-states] 

スクリプトを実行して、クラスターは、 **ClusterCustomization** 段階です。 この段階でスクリプトは、システム管理者アカウントで、ノードで完全な管理者権限を持ち、クラスター内のすべての指定したノードで並列的に実行されます。 

> [AZURE.NOTE] クラスター ノードで管理者権限を持っているため、 **ClusterCustomization** 段階では、停止や、Hadoop 関連のサービスを含むサービスの開始などの操作を実行するスクリプトを使用することができます。 そのため、スクリプトの一部として、スクリプトの完了前に Ambari サービスや他の Hadoop 関連のサービスが動作していることを確認する必要があります。 これらのサービスでは、クラスターの作成時にクラスターが正常に稼動しているか確認する必要があります。 これらのサービスに影響するクラスター上の構成を変更する場合は、用意されているヘルパー関数を使用する必要があります。 ヘルパー関数の詳細については、次を参照してください。 [HDInsight の Script Action の開発スクリプト][hdinsight-write-script]します。

スクリプトの出力とエラー ログは、クラスター用に指定した既定のストレージ アカウントに格納されます。 名前のテーブルにログが格納されている **u < \cluster-name-fragment >< \time-stamp > setuplog**します。 これらは、クラスター内のすべてのノード (ヘッドノードとワーカー ノード) で実行されるスクリプトから取得される集計ログです。


各クラスターは、指定された順序で呼び出される複数の Script Action を受け取ることができます。 スクリプトはヘッド ノード、ワーカー ノード、またはその両方で実行できます。 

## Script Action のスクリプトを呼び出す

Script Action のスクリプトは、Azure クラシック ポータル、Azure PowerShell、または HDInsight .NET SDK から使用できます。

HDInsight は、HDInsight クラスターで、次のコンポーネントをインストールするためのいくつかのスクリプトを提供します。

名前 | スクリプト
----- | -----
**Spark のインストール** | https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 します。 参照してください [HDInsight での Spark クラスターをインストールして使用する][hdinsight-install-spark]です。
**R のインストール** | https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1 します。 参照してください [インストールし、HDInsight クラスターで R を使用して][hdinsight-install-r]します。
**Solr のインストール** | https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 します。 参照してください [インストールして使用する HDInsight クラスターに Solr](hdinsight-hadoop-solr-install.md)します。
- **の Giraph のインストール** | https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 します。 参照してください [インストールして使用する HDInsight クラスターに Giraph](hdinsight-hadoop-giraph-install.md)します。



**Azure クラシック ポータルから以下の操作を実行します。**

1. 使用してプロビジョニングを開始、 **カスタム作成** オプション」の説明に従って [カスタム オプションを使用してクラスターをプロビジョニング](hdinsight-provision-clusters.md#portal)します。 
2.  **Script Action** ページ、ウィザードのをクリックして **[スクリプト アクションの追加** 次に示すように、スクリプト アクションの詳細を提供します。

    ![Script Action を使ってクラスターをカスタマイズする](./media/hdinsight-hadoop-customize-cluster-v1/HDI.CustomProvision.Page6.png "Use Script Action to customize a cluster")
    
    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>名前</td>
            <td>スクリプト アクションの名前を指定します。</td></tr>
        <tr><td>スクリプト URI</td>
            <td>クラスターのカスタマイズのために呼び出されるスクリプトへの URI を指定します。 s</td></tr>
        <tr><td>ノードの種類</td>
            <td>カスタマイズ スクリプトが実行されるノードを指定します。 選択する <b>のすべてのノード</b>, 、<b>ヘッド ノードのみ</b>, 、または <b>ワーカー ノードのみ</b>します。
        <tr><td>パラメーター</td>
            <td>スクリプトで必要な場合は、パラメーターを指定します。</td></tr>
    </table>

    複数のスクリプト操作を追加して、クラスターに複数のコンポーネントをインストールすることができます。 

3. チェック マークをクリックして、クラスターのプロビジョニングを開始します。 
  
**Azure PowerShell コマンドレットから**

HDInsight の Azure PowerShell コマンドを使用して、1 つまたは複数の Script Action を実行します。 使用することができます、 **<a href = "http://msdn.microsoft.com/library/dn858088.aspx" target="_blank">Add-azurehdinsightscriptaction</a>** コマンドレットをカスタム スクリプトを使用します。 これらのコマンドレットを使用するには Azure PowerShell をインストールし、構成しておく必要があります。 HDInsight の Azure PowerShell コマンドレットを実行するワークステーションの構成方法の詳細については、次を参照してください。 [をインストールし、Azure PowerShell を構成][powershell-install-configure]します。

次の Azure PowerShell コマンドを使用して、HDInsight クラスターのデプロイ時に、1 つの Script Action を実行します。

    $config = New-AzureHDInsightClusterConfig –ClusterSizeInNodes 4

    $config = Add-AzureHDInsightScriptAction -Config $config –Name MyScriptActionName –Uri http://uri.to/scriptaction.ps1 –Parameters MyScriptActionParameter -ClusterRoleCollection HeadNode,DataNode

    New-AzureHDInsightCluster -Config $config

次の Azure PowerShell コマンドを使用して、HDInsight クラスターのデプロイ時に、複数の Script Action を実行します。

    $config = New-AzureHDInsightClusterConfig –ClusterSizeInNodes 4

    $config = Add-AzureHDInsightScriptAction -Config $config –Name MyScriptActionName1 –Uri http://uri.to/scriptaction1.ps1 –Parameters MyScriptAction1Parameters -ClusterRoleCollection HeadNode,DataNode | Add-AzureHDInsightScriptAction -Config $config –Name MyScriptActionName2 –Uri http://uri.to/scriptaction2.ps1 -Parameters MyScriptAction2Parameters -ClusterRoleCollection HeadNode

    New-AzureHDInsightCluster -Config $config

**HDInsight .NET SDK から**

HDInsight .NET SDK は、カスタム スクリプトを呼び出す <a href="http://msdn.microsoft.com/library/microsoft.windowsazure.management.hdinsight.clusterprovisioning.data.scriptaction.aspx" target="_blank">ScriptAction</a> クラスを提供します。 HDInsight .NET SDK を使用するには:

1. Visual Studio アプリケーションを作成し、NuGet から SDK をインストールします。  **ツール** ] メニューのをクリックして **Nuget パッケージ マネージャー**, 、クリックして **パッケージ マネージャー コンソール**します。 コンソールで次のコマンドを実行して、パッケージをインストールします。

        Install-Package Microsoft.WindowsAzure.Management.HDInsight

2. SDK を使用してクラスターを作成します。 手順については、次を参照してください。 [.NET SDK を使用して HDInsight クラスターのクラスター](hdinsight-provision-clusters.md#sdk)します。

3. 使用して、 **ScriptAction** クラスを次に示すように、カスタム スクリプトを呼び出します。

        
        var clusterInfo = new ClusterCreateParameters()
        {
            // Provide the cluster information, like
            // name, Storage account, credentials,
            // cluster size, and version            
            ...
            ...
        };

        // Add the script action to install Spark
        clusterInfo.ConfigActions.Add(new ScriptAction(
            "MyScriptActionName", // Name of the config action
            new ClusterNodeType[] { ClusterNodeType.HeadNode }, // List of nodes to install the component on
            new Uri("http://uri.to/scriptaction.ps1"), // Location of the script to install the component
            "MyScriptActionParameter" //Parameters, if any, required by the script
        ));



## HDInsight クラスターで使用するオープン ソース ソフトウェアのサポート
Microsoft Azure HDInsight サービスは柔軟性に優れたプラットフォームであり、Hadoop を中心に形成されたオープン ソース テクノロジのエコシステムを利用し、クラウド内でビッグ データ アプリケーションを構築できます。 Microsoft Azure では、一般的なレベルのサポートのオープン ソース テクノロジで説明したように、 **サポート範囲** のセクションで、 <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure サポート FAQ web サイト</a>します。 HDInsight サービスでは、次に説明するいくつかのコンポーネントについてさらに高いレベルのサポートを受けることができます。

HDInsight サービスで利用できるオープン ソース コンポーネントには、2 つの種類があります。

- **組み込みコンポーネント** -これらのコンポーネントは HDInsight クラスターにプレインストールされており、クラスターのコア機能を提供します。 たとえば、YARN リソース マネージャー、Hive クエリ言語 (HiveQL)、Mahout ライブラリなどがこのカテゴリに属します。 クラスタ コンポーネントの完全な一覧は、「[HDInsight で提供される Hadoop クラスター バージョンの新機能](hdinsight-component-versioning.md)」から入手できます。
- **カスタム コンポーネント** -インストールしたり、コミュニティで入手できるまたはユーザーによって作成された任意のコンポーネントをワークロードで使用する、クラスターのユーザーとして。

組み込みコンポーネントは全面的にサポートされており、これらのコンポーネントに関連する問題の分離と解決については、Microsoft サポートが支援します。

カスタム コンポーネントについては、問題のトラブルシューティングを進めるための支援として、商業的に妥当な範囲のサポートを受けることができます。 これにより問題が解決する場合もあれば、オープン ソース テクノロジに関して、深い専門知識が入手できる場所への参加をお願いすることになる場合もあります。 たとえば、<a href ="https://social.msdn.microsoft.com/Forums/azure/home?forum=hdinsight" target="_blank">HDInsight についての MSDN フォーラム</a>や <a href="http://stackoverflow.com" target="_blank">Stack Overflow</a> などの数多くのコミュニティ サイトが利用できます。 また、Apache プロジェクトにも、<a href="http://apache.org" target="_blank">Apache.org</a> に <a href="http://hadoop.apache.org/" target="_blank">Hadoop</a> や <a href="http://spark.apache.org/" target="_blank">Spark</a> などのプロジェクト サイトがあります。

HDInsight サービスでは、カスタム コンポーネントを使用する方法をいくつか用意しています。 コンポーネントの用途やクラスターへのインストール方法にかかわらず、同じレベルのサポートが適用されます。 以下は、HDInsight クラスターでのカスタム コンポーネントの用途として、最も一般的な方法の一覧です。

1. ジョブの送信 - Hadoop や他の種類のジョブを、カスタム コンポーネントを実行または使用するクラスターに送信できます。
2. クラスターのカスタマイズ - クラスター作成時に、追加設定や、クラスター ノードにインストールするカスタム コンポーネントを指定できます。
3. サンプル - よく利用されるカスタム コンポーネントに対しては、それらを HDInsight クラスターで使用する方法について Microsoft やその他の提供者がサンプルを用意している場合があります。 これらのサンプルはサポートなしで提供されます。

## Script Action のスクリプトを開発する

参照してください [HDInsight の Script Action の開発スクリプト][hdinsight-write-script]します。 


## 関連項目

- [カスタム オプションを使用した hdinsight の Hadoop クラスターのプロビジョニング][hdinsight-provision-cluster] は他のカスタム オプションを使用して HDInsight クラスターをプロビジョニングする方法について説明します。
- [HDInsight 用の Script Action スクリプトの開発][hdinsight-write-script]
- [HDInsight クラスターで Spark をインストールして使用する][hdinsight-install-spark]
- [HDInsight クラスターに R をインストールして使用する][hdinsight-install-r]
- [インストールして HDInsight クラスターで Solr を使用する](hdinsight-hadoop-solr-install.md)です。
- [インストールし、HDInsight クラスターに Giraph を使用して](hdinsight-hadoop-giraph-install.md)します。

[hdinsight-install-spark]: hdinsight-hadoop-spark-install.md
[hdinsight-install-r]: hdinsight-hadoop-r-scripts.md
[hdinsight-write-script]: hdinsight-hadoop-script-actions.md
[hdinsight-provision-cluster]: hdinsight-provision-clusters.md
[powershell-install-configure]: ../install-configure-powershell.md


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster-v1/HDI-Cluster-state.png "Stages during cluster provisioning"
 

