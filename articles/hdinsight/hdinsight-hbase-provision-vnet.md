<properties
    pageTitle="Virtual Network での HBase クラスターのプロビジョニング | Microsoft Azure"
    description="Azure HDInsight での HBase の使用の概要 Azure Virtual Network での HDInsight HBase クラスターの作成方法について説明します。"
    keywords=""
    services="hdinsight,virtual-network"
    documentationCenter=""
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="11/18/2015"
   ms.author="jgao"/>

# Azure Virtual Network での HBase クラスターのプロビジョニング 

Azure HDInsight HBase クラスターを作成する方法について、 [Azure Virtual Network][1]します。

[AZURE.INCLUDE [hdinsight-azure-preview-portal](../../includes/hdinsight-azure-preview-portal.md)]

* [Azure Virtual Network での HBase クラスターのプロビジョニング](hdinsight-hbase-provision-vnet-v1.md)

アプリケーションが HBase と直接通信できるように、仮想ネットワーク統合を使用して、HBase クラスターをアプリケーションと同じ仮想ネットワークにデプロイできます。 次の利点があります。

- Web アプリケーションと HBase クラスターのノード間の直接接続により、HBase Java リモート プロシージャ コール (RPC) API を使用した通信が可能になります。
- トラフィックが複数のゲートウェイやロード バランサーを経由しないためパフォーマンスが向上します。
- 機密情報は、パブリック エンドポイントに公開されることなく、より安全な方法で処理できます。

##前提条件
このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

- **Azure PowerShell を実行できるワークステーション**します。 参照してください [のインストールおよび使用 Azure PowerShell](http://azure.microsoft.com/documentation/videos/install-and-use-azure-powershell/)します。 手順については、次を参照してください。 [をインストールし、Azure PowerShell を構成](../install-configure-powershell.md)します。 Azure PowerShell スクリプトを実行するには、Azure PowerShell を管理者として実行し、実行ポリシーを設定する必要があります *RemoteSigned*します。 参照してください [Set-executionpolicy コマンドレットを使用して][2]します。

    Azure PowerShell スクリプトを実行する前に、次のコマンドレットを使用して Azure サブスクリプションに接続されていることを確認します。

        Add-AzureAccount

    Azure サブスクリプションが複数ある場合は、次のコマンドレットを使用して、現在のサブスクリプションを設定します。

        Select-AzureSubscription <AzureSubscriptionName>


## HBase クラスターを仮想ネットワークへプロビジョニングする

アプリケーションは、通常、多数のコンポーネントで構成されます。 このチュートリアルでは、以下を使用します。

- Azure 仮想ネットワーク
- Azure ストレージ アカウント
- Azure HDInsight HBase クラスター
- (省略可能) DNS サーバーとして機能する Azure 仮想マシン

Azure リソース マネージャーを使用すると、アプリケーション内の複数リソースを 1 つのグループと見なして作業できます。 アプリケーションのこれらすべてのリソースを、1 回の連携した操作でデプロイ、更新、または削除できます。 デプロイにはテンプレートを使用しますが、このテンプレートは、テスト、ステージング、運用環境などのさまざまな環境に使用できます。 グループ全体のロールアップ コストを表示すると、組織の課金ついて明確に把握できます。

**リソース グループを作成するには**

1. サインイン、 [Azure ポータル](https://portal.azure.com)します。
2. クリックして **新規**, 、] をクリックして **管理**, 、] をクリックし、 **リソース グループ**します。
3. 次の値を入力または選択します。

    - **リソース グループ名**: リソース グループの名前を入力します。
    - **サブスクリプション**: このリソース グループに使用される Azure サブスクリプションを選択します。
    - **リソース グループの場所**: Azure データ センターを選択します。  この場所は、HDInsight クラスターの場所と一致させる必要はありません。

4. クリックして **作成**します。

HBase クラスターをプロビジョニングする前に、Azure 仮想ネットワークが必要になります。

**Azure ポータルを使用して仮想ネットワークを作成するには**

1. サインイン、 [ポータル](https://portal.azure.com)します。
2. クリックして **新規**, 、] をクリックして **ネットワーク**, 、] をクリックし、 **仮想ネットワークの**です。
3.  **展開モデルを選択して**, [ **クラシック** Windows ベースの HDInsight クラスターを使用する場合は、選択 **リソース マネージャー** Linux ベースの HDInsight クラスターを使用する場合。 最後に、クリックして **作成**します。

    > [AZURE.NOTE] Windows ベースのクラスターには、v1 が必要な Linux ベースのクラスターには、v2 が必要とする (クラシック) 仮想ネットワーク (Azure リソース マネージャー)、仮想ネットワークです。 ネットワークの種類が正しくない場合、クラスターの作成には使用できません。
    >
    > 作成を計画しているクラスターでは使用できない Virtual Network 上にリソースがある場合、クラスターで使用できる新しい Virtual Network を作成し、それを互換性のない Virtual Network に接続できます。 その後、必要なネットワーク バージョンでクラスターを作成し、2 つは結合されているので別のネットワークのリソースにアクセスが可能となります。 クラシックと新しい仮想ネットワークを接続する方法の詳細については、次を参照してください。 [従来の Vnet を新しい Vnet に接続する](../virtual-network/virtual-networks-arm-asm-s2s.md)です。
    
4. 次の値を入力または選択します。

    - **名前**: 仮想ネットワークの名前。
    - **アドレス空間**: アドレス空間に、クラスター内のすべてのノードのアドレスを提供するのに十分な仮想ネットワークを選択します。 そうでないと、プロビジョニングは失敗します。 このチュートリアルでは、既定値を使用できます。 クリックして **OK** 変更を保存します。
    
        > [AZURE.NOTE] 複数の HDInsight クラスターにこの仮想ネットワークを使用する場合は、各クラスターに単一のサブネットを指定する強くお勧めします。
         
    - **リソース グループ**: チュートリアルの前半で作成したリソース グループを選択します。
    - **サブスクリプション**: この仮想ネットワークに使用するように Azure サブスクリプションを選択します。
    - **場所** の場所には、作成する HBase クラスターと同じである必要があります。
    
        > [AZURE.NOTE] > Azure HDInsight は場所ベースの仮想ネットワークのみをサポートしがアフィニティ グループ ベースの仮想ネットワーク内で現在使用できません。

5. クリックして **作成**します。

既定では、仮想ネットワークは、Azure によって提供される内部ドメイン ネーム システム (DNS) サーバーを使用します。 カスタム DNS サーバーを使用した、より高度なネットワーク構成もサポートされています。 詳細については、次を参照してください。 [名前解決 (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)します。

**(省略可能) DNS サーバー仮想マシンを仮想ネットワークに追加するには**

DNS サーバーはオプションですが、場合によっては必要になります。  手順が記載されて [2 つの Azure 仮想ネットワーク間の DNS の構成][hdinsight-hbase-replication-dns]します。 基本的には次の手順を実行する必要があります。

1. Azure 仮想マシンを仮想ネットワークに追加する
2. 仮想マシンに静的 IP アドレスを設定する
3. 仮想マシンの DNS サーバー ロールを追加する
4. 仮想ネットワークへ DNS サーバーを割り当てる

**Azure ポータルを使用して HBase クラスターをプロビジョニングするには**

> [AZURE.NOTE] Azure PowerShell を使用して、新しい HBase クラスターのプロビジョニング方法の詳細については、次を参照してください。 [Azure PowerShell を使用して HBase クラスターのプロビジョニング](#powershell)します。


**HDInsight クラスターを作成するには**

1. サインイン、 [Azure ポータル](https://portal.azure.com)します。
2. クリックして **新規**, 、] をクリックして **データ分析**, 、] をクリックし、 **HDInsight**します。

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-provision-clusters/HDI.CreateCluster.1.png "Creating a new cluster in the Azure Portal")

3. 次の値を入力または選択します。

  - **クラスター名**: クラスターの名前を入力します。 その名前を使用できる場合は、クラスター名の横に緑色のチェック マークが表示されます。
  - **クラスターの種類**: 選択 **HBase**します。
  - **オペレーティング システムをクラスター**: 選択 **Windows Server 2012 R2 Datacenter**します。
  - **サブスクリプション**: 選択に使用される Azure のサブスクリプションは、このクラスターをプロビジョニングします。
  - **リソース グループ**: チュートリアルの前半で作成したリソース グループを選択します。
  - **資格情報**: ユーザー名とパスワード、Hadoop ユーザー (HTTP ユーザー) を構成します。 クラスターのリモート デスクトップを有効にする場合は、リモート デスクトップ ユーザーのユーザー名とパスワード、アカウントの有効期限を構成する必要があります。 クリックして **選択** 下部には、変更を保存します。
  - **データ ソース**: 既存を選択するか、クラスターの既定のファイル システムとして使用される新しい Azure ストレージ アカウントを作成します。 既定のコンテナーの既定の名前は、クラスター名です。  ストレージ アカウントの場所は、クラスターの場所も決定します。
  - **ノード料金レベル**: learning または評価の目的のためには、コストを最小限に 1 の地域ノードを選択します。

    - **選択方法**: これを設定 **すべてのサブスクリプションから** すべてのサブスクリプションからストレージ アカウントの参照を有効にします。 これを設定 **アクセス キー** を入力する場合、 **ストレージ名** と **アクセス キー** の既存のストレージ アカウント。
    - **ストレージ アカウントを選択]、[新規作成**: クリックして **ストレージ アカウントを選択** を参照して、クラスターに関連付ける既存のストレージ アカウントを選択します。 または、[ **新規作成** 新しいストレージ アカウントを作成します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。
    - **既定のコンテナーを選択して**: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。
    - **場所**: リージョン、ストレージ アカウントには、やで作成されます。 この場所は、クラスターの場所を決定します。  クラスターとその既定のストレージ アカウントは、同じデータセンター内に配置されている必要があります。

  - **ノードの価格レベルの**: クラスターに必要なワーカー ノードの数を設定します。 クラスターの推定コストがブレード内に表示されます。
    - **オプションの構成**: このチュートリアルでのみ構成する必要 **仮想ネットワーク**します。  このチュートリアルで先ほど作成した仮想ネットワークを選択します。 サブネットも選択していることを確認します。

4. クリックして **作成**します。


新しい HBase クラスターの使用を開始するには」に記載の手順を使用することができます [HBase を HDInsight での Hadoop を使い始める](../hdinsight-hbase-get-started.md)します。

##HBase Java RPC API を使用して、仮想ネットワーム内でプロビジョニングされた HBase クラスターへ接続する

1.  サービスとしてのインフラストラクチャ (IaaS) 仮想マシンを同じ Azure 仮想ネットワークと同じサブネットに対してプロビジョニングします。 それにより、仮想マシンと HBase クラスターはどちらも同じ内部 DNS サーバーを使用してホスト名を解決します。 これを行うには、選択する必要があります、 **ギャラリーから** にして、データ センターの代わりに仮想ネットワークを選択します。 手順については、次を参照してください。 [Windows Server を実行している仮想マシンを作成する](../virtual-machines-windows-tutorial.md)です。 標準の Windows Server 2012 イメージとサイズの小さい VM で十分です。

2.  Java アプリケーションを使用して HBase にリモートで接続する場合は、完全修飾ドメイン名 (FQDN) を使用する必要があります。 これを確認するには、HBase クラスターの接続固有の DNS サフィックスを取得する必要があります。 それには、Curl を使用して Ambari を照会するか、リモート デスクトップを使用してクラスターに接続します。

    * **Curl** -次のコマンドを使用します。

            curl -u <username>:<password> -k https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/hbase/components/hbrest

        返された JavaScript Object Notation (JSON) データで、"host_name" エントリを見つけます。 これには、クラスターのノードの FQDN が含まれています。 次に例を示します。

            ...
            "host_name": "wordkernode0.<clustername>.b1.cloudapp.net
            ...

        クラスター名で始まるドメイン名の部分は、DNS サフィックスです。 たとえば、mycluster.b1.cloudapp.net です。

    * **Azure PowerShell** -を登録する次の Azure PowerShell スクリプトを使用して、 **Get-clusterdetail** 関数で、DNS サフィックスを返すために使用できます。

            function Get-ClusterDetail(
                [String]
                [Parameter( Position=0, Mandatory=$true )]
                $ClusterDnsName,
                [String]
                [Parameter( Position=1, Mandatory=$true )]
                $Username,
                [String]
                [Parameter( Position=2, Mandatory=$true )]
                $Password,
                [String]
                [Parameter( Position=3, Mandatory=$true )]
                $PropertyName
                )
            {
            <#
                .SYNOPSIS
                 Displays information to facilitate an HDInsight cluster-to-cluster scenario within the same virtual network.
                .Description
                 This command shows the following 4 properties of an HDInsight cluster:
                 1. ZookeeperQuorum (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.quorum".
                 2. ZookeeperClientPort (supports only HBase type cluster)
                    Shows the value of HBase property "hbase.zookeeper.property.clientPort".
                 3. HBaseRestServers (supports only HBase type cluster)
                    Shows a list of host FQDNs that run the HBase REST server.
                 4. FQDNSuffix (supports all cluster types)
                    Shows the FQDN suffix of hosts in the cluster.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperQuorum
                 This command shows the value of HBase property "hbase.zookeeper.quorum".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName ZookeeperClientPort
                 This command shows the value of HBase property "hbase.zookeeper.property.clientPort".
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName HBaseRestServers
                 This command shows a list of host FQDNs that run the HBase REST server.
                .EXAMPLE
                 Get-ClusterDetail -ClusterDnsName {clusterDnsName} -Username {username} -Password {password} -PropertyName FQDNSuffix
                 This command shows the FQDN suffix of hosts in the cluster.
            #>

                $DnsSuffix = ".azurehdinsight.net"

                $ClusterFQDN = $ClusterDnsName + $DnsSuffix
                $webclient = new-object System.Net.WebClient
                $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)

                if($PropertyName -eq "ZookeeperQuorum")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'
                }
                if($PropertyName -eq "ZookeeperClientPort")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.property.clientPort"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    Write-host $JsonObject.items[0].properties.'hbase.zookeeper.property.clientPort'
                }
                if($PropertyName -eq "HBaseRestServers")
                {
                    $Url1 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.rest.port"
                    $Response1 = $webclient.DownloadString($Url1)
                    $JsonObject1 = $Response1 | ConvertFrom-Json
                    $PortNumber = $JsonObject1.items[0].properties.'hbase.rest.port'

                    $Url2 = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/hbase/components/hbrest"
                    $Response2 = $webclient.DownloadString($Url2)
                    $JsonObject2 = $Response2 | ConvertFrom-Json
                    foreach ($host_component in $JsonObject2.host_components)
                    {
                        $ConnectionString = $host_component.HostRoles.host_name + ":" + $PortNumber
                        Write-host $ConnectionString
                    }
                }
                if($PropertyName -eq "FQDNSuffix")
                {
                    $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/yarn/components/resourcemanager"
                    $Response = $webclient.DownloadString($Url)
                    $JsonObject = $Response | ConvertFrom-Json
                    $FQDN = $JsonObject.host_components[0].HostRoles.host_name
                    $pos = $FQDN.IndexOf(".")
                    $Suffix = $FQDN.Substring($pos + 1)
                    Write-host $Suffix
                }
            }

        Azure PowerShell スクリプトを実行すると、コマンドを使用して次を使用して DNS サフィックスを返す、 **Get-clusterdetail** 関数です。 このコマンドを使用する場合は、HDInsight HBase のクラスター名、管理者名、および管理者のパスワードを指定します。

            Get-ClusterDetail -ClusterDnsName <yourclustername> -PropertyName FQDNSuffix -Username <clusteradmin> -Password <clusteradminpassword>

        これは、DNS サフィックスを返します。 たとえば、 **yourclustername.b4.internal.cloudapp.net**します。

    > [AZURE.NOTE] リモート デスクトップを使用して、HBase クラスターが (ヘッド ノードにする接続されます) と実行に接続する **ipconfig** DNS サフィックスを取得するコマンド プロンプトからです。 リモート デスクトップ プロトコル (RDP) を有効にする RDP を使用して、クラスターに接続する手順については、次を参照してください。 [Azure ポータルを使用した HDInsight の Hadoop の管理クラスター][hdinsight-admin-portal]します。
    >
    > ![hdinsight.hbase.dns.surffix][img-dns-surffix]


<!--
3.  Change the primary DNS suffix configuration of the virtual machine. This enables the virtual machine to automatically resolve the host name of the HBase cluster without explicit specification of the suffix. For example, the *workernode0* host name will be correctly resolved to workernode0 of the HBase cluster.

    To make the configuration change:

    1. RDP into the virtual machine.
    2. Open **Local Group Policy Editor**. The executable is gpedit.msc.
    3. Expand **Computer Configuration**, expand **Administrative Templates**, expand **Network**, and then click **DNS Client**.
    - Set **Primary DNS Suffix** to the value obtained in step 2:

        ![hdinsight.hbase.primary.dns.suffix][img-primary-dns-suffix]
    4. Click **OK**.
    5. Reboot the virtual machine.
-->

仮想マシンが HBase クラスターと通信できることを確認するには、仮想マシンからコマンド `ping headnode0.<dns suffix>` を使用します。 たとえば、「ping headnode0.mycluster.b1.cloudapp.net」のように入力します。

この情報を使用して、Java アプリケーションで、」の手順を行うことができる [を Maven で HBase を HDInsight (Hadoop) で使用する Java アプリケーションを構築](hdinsight-hbase-build-java-maven.md) 、アプリケーションを作成します。 アプリケーションをリモート HBase サーバーに接続するのには、変更、 **hbase-site.xml** Zookeeper の FQDN を使用するには、この例ではファイルです。 次に例を示します。

    <property>
        <name>hbase.zookeeper.quorum</name>
        <value>zookeeper0.<dns suffix>,zookeeper1.<dns suffix>,zookeeper2.<dns suffix></value>
    </property>

> [AZURE.NOTE] Azure での名前解決の詳細については、独自の DNS サーバーを使用する方法などの仮想ネットワークを参照してください [名前解決 (DNS)](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)します。

##Azure PowerShell を使用して HBase クラスターをプロビジョニングする

**Azure PowerShell を使用して HBase クラスターをプロビジョニングするには**

1. Azure PowerShell Integrated Scripting Environment (ISE) を開きます。
2. 次のコマンドをコピーしてスクリプト ウィンドウに貼り付けます。

        $hbaseClusterName = "<HBaseClusterName>"
        $hadoopUserName = "<HBaseClusterUsername>"
        $hadoopUserPassword = "<HBaseClusterUserPassword>"
        $location = "<HBaseClusterLocation>"  #i.e. "West US"
        $clusterSize = <HBaseClusterSize>  
        $resourceGroup = "<AzureResourceGroupName>"
        $vnetID = "<AzureVirtualNetworkID>"
        $subNetName = "<AzureVirtualNetworkSubNetName>"
        $storageAccountName = "<AzureStorageAccountName>" # Do not use the full name here
        $storageAccountKey = "<AzureStorageAccountKey>"
        $storageContainerName = "<AzureBlobStorageContainer>"

        $password = ConvertTo-SecureString $hadoopUserPassword -AsPlainText -Force
        $creds = New-Object System.Management.Automation.PSCredential ($hadoopUserName, $password)

        New-AzureHDInsightCluster -ResourceGroupName $resourceGroup `
                                  -ClusterName $hbaseClusterName `
                                        -ClusterType HBase `
                                        -Location $location `
                                        -ClusterSizeInNodes $clusterSize `
                                  -HttpCredential $creds `
                                        -VirtualNetworkId $vnetID `
                                        -SubnetName $subNetName `
                                        -DefaultStorageAccountName "$storageAccountName.blob.core.windows.net" `
                                        -DefaultStorageAccountKey $storageAccountKey `
                                  -DefaultStorageContainer $storageContainerName


3. クリックして **スクリプトの実行**, 、またはキーを押します **f5 キーを押して**します。
4. クラスターを検証するために、Azure ポータルからクラスターを確認するか、下部のウィンドウから次の Azure PowerShell コマンドレットを実行することができます。

    Get-AzureHDInsightCluster

##次のステップ

このチュートリアルでは、HBase クラスターのプロビジョニング方法を学習しました。 詳細については、次を参照してください。

- [HDInsight の使用](../hdinsight-get-started.md)
- [HDInsight での HBase レプリケーションの構成](hdinsight-hbase-geo-replication.md)
- [Provision Hadoop clusters in HDInsight (HDInsight での Hadoop クラスターのプロビジョニング)](hdinsight-provision-clusters.md)
- [Get started using HBase with Hadoop in HDInsight (HDInsight の Hadoop 環境で HBase を使用する)](../hdinsight-hbase-get-started.md)
- [Analyze real-time Twitter sentiment with HBase in HDInsight (HDInsight 環境の HBase で Twitter のセンチメントをリアルタイム分析)](../hdinsight-hbase-twitter-sentiment.md)
- [Virtual Network の概要][vnet-overview]


[1]: http://azure.microsoft.com/services/virtual-network/
[2]: http://technet.microsoft.com/library/ee176961.aspx
[3]: http://technet.microsoft.com/library/hh847889.aspx

[hbase-get-started]: ../hdinsight-hbase-get-started.md
[hbase-twitter-sentiment]: ../hdinsight-hbase-twitter-sentiment.md
[vnet-overview]: ../virtual-network/virtual-networks-overview.md
[vm-create]: ../virtual-machines-windows-tutorial.md

[azure-portal]: https://portal.azure.com
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md#rdp

[hdinsight-powershell-reference]: https://msdn.microsoft.com/library/dn858087.aspx


[twitter-streaming-api]: https://dev.twitter.com/docs/streaming-apis
[twitter-statuses-filter]: https://dev.twitter.com/docs/api/1.1/post/statuses/filter


[powershell-install]: ../install-configure-powershell.md


[hdinsight-customize-cluster]: hdinsight-hadoop-customize-cluster.md
[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-storage-powershell]: ../hdinsight-use-blob-storage.md#powershell
[hdinsight-analyze-flight-delay-data]: hdinsight-analyze-flight-delay-data.md
[hdinsight-storage]: ../hdinsight-use-blob-storage.md
[hdinsight-use-sqoop]: hdinsight-use-sqoop.md
[hdinsight-power-query]: hdinsight-connect-excel-power-query.md
[hdinsight-hive-odbc]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-hbase-replication-dns]: hdinsight-hbase-geo-replication-configure-DNS.md

[img-dns-surffix]: ./media/hdinsight-hbase-provision-vnet/DNSSuffix.png
[img-primary-dns-suffix]: ./media/hdinsight-hbase-provision-vnet/PrimaryDNSSuffix.png
[img-provision-cluster-page1]: ./media/hdinsight-hbase-provision-vnet/hbasewizard1.png "Provision details for the new HBase cluster"
[img-provision-cluster-page5]: ./media/hdinsight-hbase-provision-vnet/hbasewizard5.png "Use Script Action to customize an HBase cluster"

[azure-preview-portal]: https://portal.azure.com


