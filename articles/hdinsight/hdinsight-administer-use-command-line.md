<properties
    pageTitle="Azure CLI を使用した Hadoop クラスターの管理 |Microsoft Azure"
    description="Azure CLI を使用して HDInsight で Hadoop クラスターを管理する方法"
    services="hdinsight"
    editor="cgronlun"
    manager="paulettm"
    authors="mumian"
    tags="azure-portal"
    documentationCenter=""/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/16/2015"
    ms.author="jgao"/>

# Azure CLI を使用した HDInsight での Hadoop クラスターの管理

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

使用する方法について、 [Azure コマンド ライン インターフェイス](xplat-cli-install.md) Azure HDInsight で Hadoop クラスターを管理します。 Azure CLI は Node.js で実装されます。 Windows、Mac、Linux など、Node.js をサポートするいずれのプラットフォームでも使用できます。

この記事では、HDInsight での Azure CLI の使用についてのみ説明します。 Azure CLI を使用する方法の一般的なガイドラインを参照してください。 [をインストールし、Azure cli][azure-command-line-tools]します。

##前提条件

この記事を読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **Azure CLI** -を参照してください [をインストールし、Azure CLI の構成](../xplat-cli-install.md) インストールと構成情報。
- **Azure への接続**, 、次のコマンドを使用します。

        azure login

    職場または学校のアカウントを使用した認証の詳細については、次を参照してください。 [Azure CLI から Azure サブスクリプションへの接続](xplat-cli-connect.md)します。
    
- **Azure リソース マネージャー モードに切り替える**, 、次のコマンドを使用します。

        azure config mode arm

ヘルプを取得する、 **-h** 切り替えます。  次に例を示します。

    azure hdinsight cluster create -h
    
##クラスターの作成

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

HDInsight クラスターを作成するには、Azure リソース マネージャー (ARM)、および Azure BLOB ストレージ アカウントが必要です。 HDInsight クラスターを作成するには、以下を指定する必要があります。

- **Azure リソース グループ**: A Data Lake 分析アカウントは Azure リソース グループ内に作成する必要があります。 Azure リソース マネージャーを使用すると、アプリケーション内の複数リソースを 1 つのグループと見なして作業できます。 アプリケーションのこれらすべてのリソースを、1 回の連携した操作でデプロイ、更新、または削除できます。 

    サブスクリプションのリソース グループをリストするには:
    
        azure group list 
    
    新しいリソース グループを作成するには:
    
        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **HDInsight クラスター名**

- **場所**: HDInsight クラスターをサポートする Azure データ センターのいずれかです。 サポートされている場所の一覧は、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。

- **既定のストレージ アカウント**: HDInsight は既定のファイル システムとして Azure Blob ストレージ コンテナーを使用します。 HDInsight クラスターを作成するには Azure ストレージ アカウントが必要です。

    新しい Azure ストレージ アカウントを作成するには:
    
        azure storage account create "<Azure Storage Account Name>" -g "<Resource Group Name>" -l "<Azure Location>" --type LRS

    > [AZURE.NOTE] ストレージ アカウントは、データ センターでの HDInsight に接続する必要があります。
    > ZRS ではテーブルがサポートされないため、ストレージ アカウントの種類を ZRS にすることはできません。

    Azure ポータルを使用して Azure ストレージ アカウントの作成方法の詳細については、次を参照してください。 [の作成、管理、またはストレージ アカウントを削除][azure-create-storageaccount]します。
    
    既にストレージ アカウントを持っていて、アカウント名とアカウント キーがわからない場合は、次のコマンドを使ってその情報を取得できます。
    
        -- Lists Storage accounts
        azure storage account list
        -- Shows a Storage account
        azure storage account show "<Storage Account Name>"
        -- Lists the keys for a Storage account
        azure storage account keys list "<Storage Account Name>" -g "<Resource Group Name>"

    Azure ポータルを使用して情報を取得の詳細については、の「表示、コピーおよび再生成するストレージ アクセス キー」のセクションを参照して [の作成、管理、またはストレージ アカウントを削除][azure-create-storageaccount]します。

- **(省略可能)既定の Blob コンテナー**: **azure hdinsight クラスターを作成** が存在しない場合、コマンドは、コンテナーを作成します。 コンテナーを事前に作成する場合は、次のコマンドを使用できます。

    azure ストレージ コンテナー作成 - アカウント名]<Storage Account Name>"-アカウント キー <Storage Account Key> [ContainerName]

ストレージ アカウントを用意したら、クラスターを作成する準備は整いました。

    azure hdinsight cluster create --resource-group <Resource Group Name> --clusterName <Cluster Name> --location <Location> --osType <Windows | Linux> --version <Cluster Version> --clusterType <Hadoop | HBase | Spark | Storm> --storageAccountName <Default Storage Account Name> --storageAccountKey <Storage Account Key> --storageContainer <Default Storage Container> --username <HDInsight Cluster Username> --password <HDInsight Cluster Password> --sshUserName <SSH Username> --sshPassword <SSH User Password> --workerNodeCount <Number of Worker Nodes>


##構成ファイルを使用するクラスターの作成
通常は、HDInsight クラスターを作成し、そのクラスター上でジョブを実行してから、クラスターを削除してコストを削減します。 コマンド ライン インターフェイスでは構成をファイルに保存することもできるため、クラスターを作成するたびにその構成を再利用できます。  

    azure hdinsight config create [options ] <Config File Path> <overwirte>
    azure hdinsight config add-config-values [options] <Config File Path>
    azure hdinsight config add-script-action [options] <Config File Path>

例: クラスターを作成するときに実行するスクリプト アクションを含む構成ファイルの作成

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <Script Action URI> --name myScriptAction --parameters "-param value"

##スクリプト アクションを使用したクラスターの作成

たとえば次のようになります。

    azure hdinsight cluster create -g mahirg001 -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01
    
一般的なスクリプト アクションの情報を参照してください。 [カスタマイズを使用した hdinsight Script Action (Linux)](hdinsight-hadoop-customize-cluster-linux.md)します。

##クラスターの一覧と詳細の表示
クラスターの一覧と詳細を表示するには、次のコマンドを使用します。

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![HDI.CLIListCluster][image-cli-clusterlisting]


##クラスターの削除
クラスターを削除するには、次のコマンドを使用します。

    azure hdinsight cluster delete <Cluster Name>

##クラスターのスケール

Hadoop クラスターのサイズを変更するには:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## クラスターの HTTP アクセスの有効化/無効化

    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## クラスターの RDP アクセスの有効化/無効化

    azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
    azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


##次のステップ
この記事では、さまざまな HDInsight クラスター管理タスクを実行する方法について説明しました。 詳細については、次の記事を参照してください。

* [Azure ポータルを使用した HDInsight の管理] [hdinsight-admin-portal]
* [Azure PowerShell を使用した HDInsight の管理] [hdinsight-admin-powershell]
* [Azure HDInsight の概要] [hdinsight-get-started]
* [Azure CLI の使い方] [azure-command-line-tools]


[azure-command-line-tools]: ../xplat-cli.md
[azure-create-storageaccount]: ../storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: ../hdinsight-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/HDI.CLIListClusters.png "List and show clusters"

