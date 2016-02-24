<properties
    pageTitle="Script Action を使った HDInsight クラスターのカスタマイズ | Microsoft Azure"
    description="Script Action を使って HDInsight クラスターをカスタマイズする方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/13/2015"
    ms.author="nitinme"/>

# Script Action を使用して HDInsight をカスタマイズする (Windows)

[AZURE.INCLUDE [usescriptaction-selector](../../includes/hdinsight-selector-use-script-action.md)]

**スクリプト操作** 呼び出しに使用できる [カスタム スクリプト](hdinsight-hadoop-script-actions.md) 
クラスターの作成中、クラスターに追加のソフトウェアをインストールするためです。

この記事の情報は、Windows ベースの HDInsight クラスターに固有のものです。 タブを使用します。 
この記事は Linux ベースのクラスターに固有のバージョンに切り替えるを選択します。

HDInsight クラスターなどがさまざまな同様に、その他の方法でカスタマイズできます。 
Azure ストレージ アカウントの追加、Hadoop 構成ファイル (core-site.xml を変更します。 
hive-site.xml など)、一般的な場所への共有ライブラリ (Hive、Oozie など) の追加 
でクラスターです。 Azure PowerShell、Azure からこれらのカスタマイズを行うことができます。 
HDInsight .NET SDK または Azure のポータルです。 詳細については、次をご覧ください。 
[HDInsight で Hadoop クラスターを作成][hdinsight-provision-cluster]します。

## クラスターの作成処理での Script Action

Script Action は、クラスターが作成中にのみ使用されます。 次の 
図では、スクリプト アクションの作成プロセス中に実行を示します。

![クラスター作成時の HDInsight クラスターのカスタマイズと段階][img-hdi-cluster-states]

スクリプトを実行して、クラスターは、 **ClusterCustomization** 段階です。 これを 
ステージでは、スクリプトは、システム管理者アカウントのすべての並列実行は、指定しました。 
クラスター内のノードとノードの完全な管理者特権を提供します。

> [AZURE.NOTE] クラスター ノードで管理者権限を持っているため、 
**ClusterCustomization** 段階では、"stopping"のような操作を実行するスクリプトを使用することができます 
Hadoop 関連のサービスを含むサービスを開始します。 そのため、スクリプトの一部として、必要があります。 
Ambari サービスやその他の Hadoop 関連のサービスが前に実行されていることを確認します。 
スクリプトの実行が完了します。 これらのサービスが正常に稼働状態を確認するために必要 
作成中に、クラスターの状態。 任意の構成を変更する場合、 
[クラスターの影響するこれらのサービスは、用意されているヘルパー関数を使用する必要があります。 次に 
ヘルパー関数に関する詳細情報を参照してください [HDInsight の Script Action の開発スクリプト][hdinsight-write-script]します。

出力とエラー ログで、スクリプトが既定のストレージに格納されているためしたアカウントします。 
クラスターに指定します。 ログは、名前のテーブルに格納します。 
**u < \cluster-name-fragment >< \time-stamp > setuplog**します。 これらは、スクリプトから集計のログ 
すべてのノード (ヘッドノードとワーカー ノード)、クラスター内で実行します。

各クラスターは、順番に起動される複数の script action を受け取ることができます、 
指定します。 スクリプトはヘッド ノード、ワーカー ノード、またはその両方で実行できます。

HDInsight は、HDInsight クラスターで、次のコンポーネントをインストールするためのいくつかのスクリプトを提供します。

名前 | スクリプト
----- | -----
**Spark のインストール** | https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 します。 参照してください [HDInsight での Spark クラスターをインストールして使用する][hdinsight-install-spark]です。
**R のインストール** | https://hdiconfigactions.blob.core.windows.net/rconfigactionv02/r-installer-v02.ps1 します。 参照してください [インストールし、HDInsight クラスターで R を使用して][hdinsight-install-r]します。
**Solr のインストール** | https://hdiconfigactions.blob.core.windows.net/solrconfigactionv01/solr-installer-v01.ps1 します。 参照してください [インストールして使用する HDInsight クラスターに Solr](hdinsight-hadoop-solr-install.md)します。
- **の Giraph のインストール** | https://hdiconfigactions.blob.core.windows.net/giraphconfigactionv01/giraph-installer-v01.ps1 します。 参照してください [インストールして使用する HDInsight クラスターに Giraph](hdinsight-hadoop-giraph-install.md)します。



## Azure ポータルを使用してスクリプトを呼び出す

**Azure ポータル**

1. 」の説明に従って、クラスターの作成を開始 [作成の Hadoop クラスターの HDInsight](hdinsight-provision-clusters.md#portal)します。
2. [省略可能な構成の **スクリプトのアクション** ブレードで、をクリックして **スクリプト アクションを追加** 次に示すように、スクリプト アクションの詳細を提供します。

    ![Script Action を使ってクラスターをカスタマイズする](./media/hdinsight-hadoop-customize-cluster/HDI.CreateCluster.8.png "Use Script Action to customize a cluster")

    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>名</td>
            <td>スクリプト アクションの名前を指定します。</td></tr>
        <tr><td>スクリプト URI</td>
            <td>クラスターをカスタマイズするために呼び出されるスクリプトへの URI を指定します。 s</td></tr>
        <tr><td>Head/ワーカー</td>
            <td>ノードを指定 (**ヘッド** または **ワーカー**) カスタマイズ スクリプトが実行されるのです。</b>します。
        <tr><td>パラメーター</td>
            <td>スクリプトで必要な場合は、パラメーターを指定します。</td></tr>
    </table>

    クラスターに複数のコンポーネントをインストールするには、Enter キーを押して複数のスクリプト アクションを追加します。

3. クリックして **選択** スクリプト アクションの構成を保存し、クラスターの作成を続行します。

## Azure PowerShell を使用してスクリプトを呼び出す

次の PowerShell スクリプトでは、Windows ベースの HDInsight クラスターに Spark をインストールする方法を示します。  

    # Provide values for these variables
    $subscriptionID = "<Azure Suscription ID>" # After "Login-AzureRmAccount", use "Get-AzureRmSubscription" to list IDs.

    $nameToken = "<Enter A Name Token>"  # The token is use to create Azure service names.
    $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")
    
    $resourceGroupName = $namePrefix + "rg"
    $location = "EAST US 2" # used for creating resource group, storage account, and HDInsight cluster.
    
    $hdinsightClusterName = $namePrefix + "spark"
    $httpUserName = "admin"
    $httpPassword = "Pass@word111"
    
    $defaultStorageAccountName = "$namePrefix" + "store"
    $defaultBlobContainerName = $hdinsightClusterName
    
    #############################################################
    # Connect to Azure
    #############################################################
    
    Try{
        Get-AzureRmSubscription
    }
    Catch{
        Login-AzureRmAccount
    }
    Select-AzureRmSubscription -SubscriptionId $subscriptionID
    
    #############################################################
    # Prepare the dependent components
    #############################################################
    
    # Create resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    
    # Create storage account
    New-AzureRmStorageAccount -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName -Location $location -Type Standard_LRS
    $defaultStorageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccountName |  %{ $_.Key1 }
    $defaultStorageAccountContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $defaultBlobContainerName -Context $defaultStorageAccountContext
    
    #############################################################
    # Create cluster with Spark
    #############################################################
    
    # Specify the configuration options
    $config = New-AzureRmHDInsightClusterConfig `
                -DefaultStorageAccountName "$defaultStorageAccountName.blob.core.windows.net" `
                -DefaultStorageAccountKey $defaultStorageAccountKey 
                
    
    # Add a script action to the cluster configuration
    $config = Add-AzureRmHDInsightScriptAction `
                -Config $config `
                -Name "Install Spark" `
                -NodeType HeadNode `
                -Uri https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1 `
    
    # Start creating a cluster with Spark installed
    New-AzureRmHDInsightCluster `
            -ResourceGroupName $resourceGroupName `
            -ClusterName $hdinsightClusterName `
            -Location $location `
            -ClusterSizeInNodes 2 `
            -ClusterType Hadoop `
            -OSType Windows `
            -DefaultStorageContainer $defaultBlobContainerName `
            -Config $config


その他のソフトウェアをインストールするには、スクリプトのスクリプト ファイルを置換する必要があります。


入力を求められたら、クラスターの資格情報を入力します。 クラスターが作成されるまでに数分かかる場合があります。

## .NET SDK を使用してスクリプトを呼び出す 

次のサンプルでは、Windows ベースの HDInsight クラスターに Spark をインストールする方法を示します。 その他のソフトウェアをインストールするには、コードのスクリプト ファイルを置換する必要があります。

**Spark で HDInsight クラスターを作成するには** 

1. Visual Studio で、C# コンソール アプリケーションを作成します。
2. NuGet パッケージ マネージャー コンソールから、次のコマンドを実行します。

        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Common.Authentication -Pre

2. Program.cs ファイルで次の using ステートメントを使用します。

        using System;
        using System.Security;
        using Microsoft.Azure.Management.HDInsight;
        using Microsoft.Azure.Management.HDInsight.Models;
        
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Factories;
        using Microsoft.Azure.Common.Authentication.Models;

3. クラスのコードを次のコードに置き換えます。

        private static HDInsightManagementClient _hdiManagementClient;

        private static Guid SubscriptionId = new Guid("<YourAzureSubscriptionID>");
        private const string ResourceGroupName = "<ExistingAzureResourceGroupName>";
        private const string NewClusterName = "<NewAzureHDInsightClusterName>";
        private const int NewClusterNumNodes = <NumberOfClusterNodes>;
        private const string NewClusterLocation = "East US";
        private const string NewClusterVersion = "3.2";
        private const string ExistingStorageName = "<ExistingAzureStorageAccountName>";
        private const string ExistingStorageKey = "<ExistingAzureStorageAccountKey>";
        private const string ExistingContainer = "<ExistingAzureBlobStorageContainer>";
        private const HDInsightClusterType NewClusterType = HDInsightClusterType.Hadoop;
        private const OSType NewClusterOSType = OSType.Windows;
        private const string NewClusterUsername = "<HttpUserName>";
        private const string NewClusterPassword = "<HttpUserPassword>";

        static void Main(string[] args)
        {
            System.Console.WriteLine("Running");

            var tokenCreds = GetTokenCloudCredentials();
            var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);

            _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);

            CreateCluster();

        }

        private static void CreateCluster()
        {
            var parameters = new ClusterCreateParameters
            {
                ClusterSizeInNodes = NewClusterNumNodes,
                Location = NewClusterLocation,
                ClusterType = NewClusterType,
                OSType = NewClusterOSType,
                Version = NewClusterVersion,

                DefaultStorageAccountName = ExistingStorageName,
                DefaultStorageAccountKey = ExistingStorageKey,
                DefaultStorageContainer = ExistingContainer,

                UserName = NewClusterUsername,
                Password = NewClusterPassword,
            };

            ScriptAction sparkScriptAction = new ScriptAction("Install Spark",
                new Uri("https://hdiconfigactions.blob.core.windows.net/sparkconfigactionv03/spark-installer-v03.ps1"), "");

            parameters.ScriptActions.Add(ClusterNodeType.HeadNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });
            parameters.ScriptActions.Add(ClusterNodeType.WorkerNode, new System.Collections.Generic.List<ScriptAction> { sparkScriptAction });

            _hdiManagementClient.Clusters.Create(ResourceGroupName, NewClusterName, parameters);
        }


        public static SubscriptionCloudCredentials GetTokenCloudCredentials(string username = null, SecureString password = null)
        {
            var authFactory = new AuthenticationFactory();

            var account = new AzureAccount { Type = AzureAccount.AccountType.User };

            if (username != null && password != null)
                account.Id = username;

            var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

            var accessToken =
                authFactory.Authenticate(account, env, AuthenticationFactory.CommonAdTenant, password, ShowDialog.Auto)
                    .AccessToken;

            return new TokenCloudCredentials(accessToken);
        }

        public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
        {
            return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
        }

4. キーを押して **f5 キーを押して** アプリケーションを実行します。


## HDInsight クラスターで使用するオープン ソース ソフトウェアのサポート
Microsoft Azure HDInsight サービスは柔軟性に優れたプラットフォームであり、Hadoop を中心に形成されたオープン ソース テクノロジのエコシステムを利用し、クラウド内でビッグ データ アプリケーションを構築できます。 Microsoft Azure では、一般的なレベルのサポートのオープン ソース テクノロジで説明したように、 **サポート範囲** のセクションで、 <a href="http://azure.microsoft.com/support/faq/" target="_blank">Azure サポート FAQ web サイト</a>します。 HDInsight サービスでは、次に説明するいくつかのコンポーネントについてさらに高いレベルのサポートを受けることができます。

HDInsight サービスで利用できるオープン ソース コンポーネントには、2 つの種類があります。

- **組み込みコンポーネント** -これらのコンポーネントは HDInsight クラスターにプレインストールされており、クラスターのコア機能を提供します。 たとえば、YARN リソース マネージャー、Hive クエリ言語 (HiveQL)、Mahout ライブラリなどがこのカテゴリに属します。 クラスタ コンポーネントの完全な一覧が記載されている [HDInsight で提供される Hadoop クラスター バージョンの新機能ですか?](hdinsight-component-versioning.md)</a>します。
- **カスタム コンポーネント** -インストールしたり、コミュニティで入手できるまたはユーザーによって作成された任意のコンポーネントをワークロードで使用する、クラスターのユーザーとして。

組み込みコンポーネントは全面的にサポートされており、これらのコンポーネントに関連する問題の分離と解決については、Microsoft サポートが支援します。

> [AZURE.WARNING] HDInsight クラスターと共に提供されるコンポーネントは完全にサポートしを特定し、これらのコンポーネントに関連する問題を解決するには Microsoft サポートが支援します。
>
> カスタム コンポーネントについては、問題のトラブルシューティングを進めるための支援として、商業的に妥当な範囲のサポートを受けることができます。 これにより問題が解決する場合もあれば、オープン ソース テクノロジに関して、深い専門知識が入手できる場所への参加をお願いすることになる場合もあります。 たとえば、使用できますが、このような数多くのコミュニティ サイトがある: [HDInsight についての MSDN フォーラム](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=hdinsight), 、[http://stackoverflow.com](http://stackoverflow.com)します。 Apache プロジェクトがプロジェクトのサイトを持つも [http://apache.org](http://apache.org), 、たとえば: [Hadoop](http://hadoop.apache.org/), 、[Spark](http://spark.apache.org/)します。

HDInsight サービスでは、カスタム コンポーネントを使用する方法をいくつか用意しています。 コンポーネントの用途やクラスターへのインストール方法にかかわらず、同じレベルのサポートが適用されます。 以下は、HDInsight クラスターでのカスタム コンポーネントの用途として、最も一般的な方法の一覧です。

1. ジョブの送信 - Hadoop や他の種類のジョブを、カスタム コンポーネントを実行または使用するクラスターに送信できます。
2. クラスターのカスタマイズ - クラスター作成時に、追加設定や、クラスター ノードにインストールするカスタム コンポーネントを指定できます。
3. サンプル - よく利用されるカスタム コンポーネントに対しては、それらを HDInsight クラスターで使用する方法について Microsoft やその他の提供者がサンプルを用意している場合があります。 これらのサンプルはサポートなしで提供されます。

## Script Action のスクリプトを開発する

参照してください [HDInsight の Script Action の開発スクリプト][hdinsight-write-script]します。


## 関連項目

- [HDInsight で Hadoop クラスターを作成][hdinsight-provision-cluster] は他のカスタム オプションを使用して HDInsight クラスターを作成する方法について説明します。
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


[img-hdi-cluster-states]: ./media/hdinsight-hadoop-customize-cluster/HDI-Cluster-state.png "Stages during cluster creation"

