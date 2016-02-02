<properties
    pageTitle="Virtual Network を使用した HDInsight の拡張 | Microsoft Azure"  
    description="Azure Virtual Network を使用して HDInsight を他のクラウド リソースまたはデータ センター内のリソースに接続する方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="paulettm"
    editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="11/18/2015"
   ms.author="larryfr"/>



# Azure Virtual Network を使用した HDInsight 機能の拡張

Azure Virtual Network では、Hadoop ソリューションを拡張して、SQL Server などのオンプレミスのリソースに組み込むことや、クラウドのリソース間にセキュリティで保護されたプライベート ネットワークを作成することができます。
> [AZURE.NOTE] HDInsight ではアフィニティ ベースの Azure の仮想ネットワークはサポートされていません。 HDInsight を使用するときは、場所ベースの仮想ネットワークを使用する必要があります。


## <a id="whatis"></a>Azure の仮想ネットワークとは

[Azure Virtual Network](http://azure.microsoft.com/documentation/services/virtual-network/) ソリューションに必要なリソースを含む、セキュリティで保護された永続的なネットワークを作成することができます。 仮想ネットワークでは、次のことが可能になります。

* プライベート ネットワーク (クラウドのみ) 内でのクラウド リソース間の接続

    ![クラウドのみの構成の図](media/hdinsight-extend-hadoop-virtual-network/cloud-only.png)

    Virtual Network を使用して Azure サービスと Azure HDInsight を連携させることで、以下のシナリオが実現できるようになります。

    * Azure Web サイトから、または Azure の仮想マシンで実行しているサービスから、**HDInsight サービスまたはジョブを呼び出す**。

    * HDInsight と Azure SQL Database、または SQL Server や仮想マシンで実行されている他のデータ ストレージ ソリューションとの間で、**データを直接転送する**。

    * **複数の HDInsight サーバーを結合して単一のソリューションにする。** たとえば、HDInsight Storm サーバーを使用して受信データを処理し、処理したデータを HDInsight HBase サーバーに格納します。 今後 MapReduce を使用して分析するために、生データを HDInsight Hadoop サーバーに格納することもできます。

* 仮想プライベート ネットワーク (VPN) を使用したローカル データセンター ネットワークへのクラウド リソースの接続 (サイト間またはポイント対サイト)。

    サイト間構成では、ハードウェア VPN を使用するか、ルーティングとリモート アクセス サービスを使用して、データセンターから複数のリソースを Azure 仮想ネットワークに接続できます。

    ![サイト間構成の図](media/hdinsight-extend-hadoop-virtual-network/site-to-site.png)

    ポイント対サイト構成では、ソフトウェア VPN を使用して、特定のリソースを Azure の仮想ネットワークに接続できます。

    ![ポイント対サイト構成の図](media/hdinsight-extend-hadoop-virtual-network/point-to-site.png)

    Virtual Network を使用してクラウドとデータ センターをリンクすることで、クラウドのみの構成と同様のシナリオを実現できます。 ただし、クラウド内のリソースの操作に制限されるのではなく、データ センターのリソースも使用できます。

    * HDInsight とデータ センター間で**データを直接転送する**。 たとえば、Sqoop を使用して SQL Server に、または SQL Server からデータを転送したり、基幹業務 (LOB) アプリケーションで生成されたデータを読み取ったりします。

    * **LOB アプリケーションから HDInsight サービスを呼び出す。** たとえば、HBase Java API を使用して HDInsight HBase クラスターにデータを格納したり、クラスターからデータを取得したりします。

仮想ネットワーク機能、利点、および機能の詳細については、次を参照してください。、 [Azure 仮想ネットワークの概要](../virtual-network/virtual-networks-overview.md)します。
> [AZURE.NOTE] HDInsight クラスターをプロビジョニングする前に、Azure Virtual Network を作成する必要があります。 詳細については、次を参照してください。 [仮想ネットワークの構成タスク](http://azure.microsoft.com/documentation/services/virtual-network/)します。
>
> Azure HDInsight は場所ベースの仮想ネットワークのみをサポートし、アフィニティ グループ ベースの仮想ネットワークは現在取り扱っていません。 
>
> 各クラスターには単一のサブネットを指定することを強くお勧めします。 
>
> Windows ベースのクラスターでは、v1 (クラシック) Virtual Network が必要で、Linux ベースのクラスターでは、v2 (Azure リソース マネージャー) Virtual Network が必要です。 ネットワークの種類が正しくない場合、クラスターの作成には使用できません。
>
> 作成を計画しているクラスターでは使用できない Virtual Network 上にリソースがある場合、クラスターで使用できる新しい Virtual Network を作成し、それを互換性のない Virtual Network に接続できます。 その後、必要なネットワーク バージョンでクラスターを作成し、2 つは結合されているので別のネットワークのリソースにアクセスが可能となります。 クラシックと新しい仮想ネットワークを接続する方法の詳細については、次を参照してください。 [従来の Vnet を新しい Vnet に接続する](../virtual-network/virtual-networks-arm-asm-s2s.md)します。

仮想ネットワークでの HDInsight クラスターのプロビジョニング方法の詳細については、次を参照してください。 [HDInsight のプロビジョニングの Hadoop クラスター](hdinsight-provision-clusters.md)します。

## <a id="tasks"></a>タスクと情報

このセクションでは、仮想ネットワークで HDInsight を使用する場合の一般的なタスクに関する情報と、必要になる可能性がある情報を記載しています。

### FQDN の決定

HDInsight クラスターには、Virtual Network インターフェイスを表す特定の完全修飾ドメイン名 (FQDN) が割り当てられます。 これは、仮想ネットワーク上の他のリソースからクラスターに接続するときに使用する必要があるアドレスです。 FQDN を確認するには、次の URL を使用して Ambari 管理サービスを照会します。

    https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/<servicename>/components/<componentname>

> [AZURE.NOTE] HDInsight での Ambari の使用に関する詳細については、次を参照してください。 [Ambari API を使用した HDInsight のモニターの Hadoop クラスター](hdinsight-monitor-use-ambari-api.md)します。

クラスター名およびクラスターで実行するサービスとコンポーネント (YARN リソース マネージャーなど) を指定する必要があります。
> [AZURE.NOTE] 返されるデータは、コンポーネントに関する多くの情報を含む JavaScript Object Notation (JSON) ドキュメントです。 FQDN のみを抽出する、を取得する JSON のパーサーを使用する必要があります `/api/v1/clusters/&lt;clustername&gt;.azurehdinsight.net/hosts/&lt;hostname&gt;/host_components [0]。HostRoles.host_name` 値。

たとえば、HDInsight Hadoop クラスターから FQDN を取得するには、次のいずれかのメソッドを使用して、YARN リソース マネージャーのデータを取得します。

* [Azure PowerShell](../powershell-install-configure.md)

      $ClusterDnsName = <clustername>
      $Username = <cluster admin username>
      $Password = <cluster admin password>
      $DnsSuffix = ".azurehdinsight.net"
      $ClusterFQDN = $ClusterDnsName + $DnsSuffix
    
      $webclient = new-object System.Net.WebClient
      $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)
    
      $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/services/yarn/     components/resourcemanager"
      $Response = $webclient.DownloadString($Url)
      $JsonObject = $Response | ConvertFrom-Json
      $FQDN = $JsonObject.host_components[0].HostRoles.host_name
      Write-host $FQDN

* [cURL](http://curl.haxx.se/) and [jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/services/yarn/components/resourcemanager | jq .host_components[0].HostRoles.host_name


### HBase への接続

Java API を使用して HBase にリモート接続するには、HBase クラスターの ZooKeeper クォーラム アドレスを確認し、これをアプリケーションで指定する必要があります。

ZooKeeper クォーラム アドレスを取得するには、次のいずれかのメソッドを使用して、Ambari 管理サービスに問い合わせます。

* [Azure PowerShell](../powershell-install-configure.md)

      $ClusterDnsName = <clustername>
      $Username = <cluster admin username>
      $Password = <cluster admin password>
      $DnsSuffix = ".azurehdinsight.net"
      $ClusterFQDN = $ClusterDnsName + $DnsSuffix
    
      $webclient = new-object System.Net.WebClient
      $webclient.Credentials = new-object System.Net.NetworkCredential($Username, $Password)
    
      $Url = "https://" + $ClusterFQDN + "/ambari/api/v1/clusters/" + $ClusterFQDN + "/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum"
      $Response = $webclient.DownloadString($Url)
      $JsonObject = $Response | ConvertFrom-Json
      Write-host $JsonObject.items[0].properties.'hbase.zookeeper.quorum'

* [cURL](http://curl.haxx.se/) and [jq](http://stedolan.github.io/jq/)

        curl -G -u <username>:<password> "https://<clustername>.azurehdinsight.net/ambari/api/v1/clusters/<clustername>.azurehdinsight.net/configurations?type=hbase-site&tag=default&fields=items/properties/hbase.zookeeper.quorum" | jq .items[0].properties[]


> [AZURE.NOTE] HDInsight での Ambari の使用に関する詳細については、次を参照してください。 [Ambari API を使用した HDInsight のモニターの Hadoop クラスター](hdinsight-monitor-use-ambari-api.md)します。

クォーラムの情報を取得したら、それをクライアント アプリケーションで使用します。

たとえば、HBase API を使用する Java アプリケーションでは、**hbase-site.xml** ファイルをプロジェクトに追加し、次のようにそのファイル内にクォーラム情報を指定します。

```
<configuration>
  <property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
  </property>
  <property>
    <name>hbase.zookeeper.quorum</name>
    <value>zookeeper0.address,zookeeper1.address,zookeeper2.address</value>
  </property>
  <property>
    <name>hbase.zookeeper.property.clientPort</name>
    <value>2181</value>
  </property>
</configuration>
```

### ネットワーク接続の確認

SQL Server などの一部のサービスでは、着信ネットワーク接続数が制限される場合があります。 そのようなサービスでは HDInsight が正常に機能しません。

HDInsight からサービスへのアクセスで問題が発生した場合は、サービスのドキュメントを参照して、ネットワーク アクセスが有効になっていることを確認してください。 ネットワーク アクセスを確認するもう 1 つの方法では、同じ仮想ネットワーク上に Azure の仮想マシンを作成し、クライアント ユーティリティを使用して、VM から仮想ネットワーク経由でサービスに接続できることを確認します。

## <a id="nextsteps"></a>次のステップ

次の例は、Azure Virtual Network で HDInsight を使用する方法を示しています。

* [Storm と HBase を HDInsight を使用したセンサー データを分析](hdinsight-storm-sensor-data-analysis.md) -仮想ネットワークでの Storm と HBase クラスターを構成する方法と、リモートで Storm から HBase にデータを記述する方法を示しています。

* [HDInsight のプロビジョニングの Hadoop クラスター](hdinsight-hadoop-provision-linux-clusters.md) -Azure の仮想ネットワークの使用に関する情報を含めて、Hadoop クラスターのプロビジョニングに関する情報を提供します。

* [HDInsight での Hadoop での Sqoop の使用](hdinsight-use-sqoop-mac-linux.md) -Sqoop を使用して、仮想ネットワーク経由で SQL Server からデータを転送する情報を提供します。

Azure の仮想ネットワークに関する詳細についてを参照してください、 [Azure 仮想ネットワークの概要](../virtual-network/virtual-networks-overview.md)します。





