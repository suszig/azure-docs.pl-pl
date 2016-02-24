<properties
    pageTitle="HDFS と互換性のある BLOB ストレージのデータのクエリ | Microsoft Azure"
    description="HDInsight では、Azure BLOB ストレージを HDFS のビッグ データ ストアとして使用します。 BLOB ストレージのデータに対するクエリを実行し、分析結果を格納する方法について説明します。"
    keywords="blob storage,hdfs,structured data,unstructured data"
    services="hdinsight,storage"
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
    ms.date="10/23/2015"
    ms.author="jgao"/>


# HDInsight の Hadoop での HDFS と互換性のある Azure BLOB ストレージの使用

このチュートリアルでは、HDInsight で低コストの Azure BLOB ストレージを使用する方法、Azure ストレージ アカウントと BLOB ストレージ コンテナーを作成する方法、内部のデータのアドレス指定を行う方法について説明します。

Azure BLOB ストレージは、堅牢な汎用ストレージ ソリューションであり、HDInsight とシームレスに統合されます。 Hadoop 分散ファイル システム (HDFS) のインターフェイスを利用して、HDInsight のすべてのコンポーネントが BLOB ストレージの構造化データまたは非構造化データを直接操作できます。

BLOB ストレージにデータを格納した場合、計算で使用する HDInsight クラスターを削除してもユーザー データは失われません。

> [AZURE.NOTE]   *Asv://* 構文は HDInsight バージョン 3.0 クラスターでサポートされていません。 つまり、この明示的に使用する HDInsight バージョン 3.0 クラスターに送信されたジョブ、 *asv://* 構文は失敗します。  *Wasb://* 構文を代わりに使用する必要があります。 また、既存のメタストアにより、asv:// 構文を使用してリソースへの明示的な参照を含むジョブを作成し、HDInsight バージョン 3.0 クラスターに送信すると、そのジョブは失敗します。 これらのメタストアは、wasb:// 構文を使用してリソースをアドレス指定するように再作成する必要があります。

> HDInsight では現在、ブロック blob のみがサポートされています。

> ほとんどの HDFS コマンド (たとえば、 <b>ls</b>。 <b>copyFromLocal</b> と <b>mkdir</b>) も期待どおりに動作します。 などでは、HDFS ネイティブ実装 (と呼ばれる DFS) に固有のコマンドのみ <b>fschk</b> と <b>dfsadmin</b>、Azure Blob ストレージで異なる動作が表示されます。

HDInsight クラスターを作成する方法の詳細については、次を参照してください。 [HDInsight の概要][hdinsight-get-started] または [作成 HDInsight クラスターの][hdinsight-creation]です。


## HDInsight のストレージ アーキテクチャ
次の図は、HDInsight のストレージ アーキテクチャを示しています。

![Hadoop クラスターは、HDFS API を使用して、BLOB ストレージの構造化データおよび非構造化データへのアクセスと保存を実行します。](./media/hdinsight-hadoop-use-blob-storage/HDI.WASB.Arch.png "HDInsight Storage Architecture")

HDInsight では、それぞれのコンピューティング ノードにローカルに割り当てられている分散ファイル システムにアクセスします。 このファイル システムには、完全修飾 URI を使用してアクセスできます。次に例を示します。

    hdfs://<namenodehost>/<path>

さらに、HDInsight では、Azure BLOB ストレージに格納されたデータにアクセスすることもできます。 の構文は次のとおりです。

    wasb[s]://<containername>@<accountname>.blob.core.windows.net/<path>


Hadoop は、既定のファイル システムの概念をサポートしています。 既定のファイル システムは、既定のスキームとオーソリティを意味します。 これは相対パスの解決に使用することもできます。 HDInsight の作成プロセス時に、Azure Storage アカウントと、そのアカウントに対応する特定の Azure BLOB ストレージ コンテナーが既定のファイル システムとして指定されます。

作成プロセス時に、このストレージ アカウントに加えて、同じ Azure サブスクリプションか、別の Azure サブスクリプションに属するストレージ アカウントをさらに追加することもできます。 追加のストレージ アカウントを追加する方法については、次を参照してください。 [作成 HDInsight クラスターの][hdinsight-creation]です。

- **クラスターに接続されているストレージ アカウント内のコンテナー:** これらのコンテナー内の blob へのフル アクセスがあるアカウント名とキーは、作成時に、クラスターに関連付けられて、ためです。

- **パブリック コンテナーまたはクラスターに接続されていないストレージ アカウント内のパブリック blob:** コンテナー内の blob に対する読み取り専用のアクセス許可があります。

    > [AZURE.NOTE]
        > パブリック コンテナーでは、コンテナー メタデータを取得してそのコンテナー内のすべての blob の一覧を取得できます。 パブリック BLOB の場合、正確な URL がわかっているときのみ、その BLOB にアクセスできます。 詳細については、「<a href="http://msdn.microsoft.com/library/windowsazure/dd179354.aspx">コンテナーと BLOB へのアクセスの制限</a>」をご覧ください。

- **クラスターに接続されていないストレージ アカウント内のプライベート コンテナー:** WebHCat ジョブを送信するときに、ストレージ アカウントを定義しない限り、コンテナー内の blob にアクセスすることはできません。 詳しくは、この記事の後半で説明します。


作成プロセスで定義されたストレージ アカウントとそのキーは、クラスター ノードの %HADOOP_HOME%/conf/core-site.xml に格納されます。 HDInsight の既定の動作では、core-site.xml ファイルに定義されたストレージ アカウントが使用されます。 core-site.xml ファイルを編集することは推奨されません。クラスター ヘッドノード (マスター) はいつでも再イメージングしたり移行したりでき、そうなると core-site.xml ファイルに加えた変更がすべて失われるためです。

Hive、MapReduce、Hadoop ストリーミング、Pig など、複数の WebHCat ジョブを利用して、ストレージ アカウントの説明とそのメタデータを伝達できます。 (現在、ストレージ アカウントについては Pig が対応していますが、メタデータについては対応していません)。 [Azure PowerShell を使用して blob のアクセス](#powershell) セクションこの記事では、この機能のサンプルが用意されています。 詳細については、次を参照してください。 [代替のストレージ アカウントとメタストアで HDInsight クラスターの使用](http://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx)します。

BLOB ストレージは、構造化データと非構造化データに使用できます。 BLOB ストレージ コンテナーには、"キーと値のペア" としてデータが格納されます。ディレクトリ階層はありません。 ただし、キー名でスラッシュ (/) を使用すれば、ファイルがディレクトリ階層に保存されているように見せかけることができます。 たとえば、blob のキーがあります *input/log1.txt*します。 実際 *入力* ディレクトリが存在するが、キー名でスラッシュが使用の存在するためにファイル パスの見えます。

###<a id="benefits"></a>Blob ストレージの利点
コンピューティング クラスターとストレージ リソースを同じ場所で併置しないとなるとパフォーマンスの低下が懸念されますが、これは、Azure データ センター内のストレージ アカウント リソースの近くにコンピューティング クラスターを作成することで軽減されます。高速ネットワークが整備されているので、コンピューティング ノードは Azure BLOB ストレージ内のデータに非常に効率的にアクセスできます。

HDFS ではなく、Azure BLOB ストレージにデータを格納することにはいくつかの利点があります。

* **データの再利用と共有:** HDFS のデータは、計算クラスター内に配置されます。 HDFS API を使用してデータを操作できるのは、コンピューティング クラスターへのアクセスが許可されているアプリケーションだけです。 Azure Blob ストレージ内のデータは、HDFS Api を通じて、またはを通じてアクセスできます、 [Blob ストレージ REST Api][blob-storage-restAPI]します。 したがって、さまざまなアプリケーション (その他の HDInsight クラスターを含む) やツールを使用してデータの生成と利用ができます。
* **データのアーカイブ:** ユーザー データを失うことがなく削除しても計算で使用する HDInsight クラスターにより、Azure Blob ストレージにデータを格納します。
* **データ ストレージ コスト:** DFS に長期間よりも高額計算クラスターのコストが Azure Blob ストレージ コンテナーのコストよりも多いので、Azure Blob ストレージにデータを格納するデータを格納します。 さらに、コンピューティング クラスターを生成するたびにデータを読み込む必要がないので、データの読み込みコストも節約できます。
* **柔軟なスケール アウト:** が HDFS でスケール アウトされたファイル システムでは、小数点以下桁数は、クラスター用に作成したノードの数によって決まります。 小数点以下桁数を変更すると、Azure Blob ストレージに自動的に取得する機能を拡大/縮小、弾力性に依存するよりも複雑なプロセスになります。
* **Geo レプリケーション:** Azure Blob ストレージ コンテナーは、geo レプリケートできます。 災害発生時には別の拠点でデータを回復でき、データの冗長性が高まりますが、geo レプリケートした別拠点へのフェールオーバーはパフォーマンスに大きな影響を与え、追加コストが発生する可能性もあります。 geo レプリケーションを利用するときは、追加コストがかかっても保護する価値のあるデータかどうかを十分に考慮してください。

MapReduce の一部のジョブやパッケージでは中間結果が生成されますが、Azure BLOB ストレージ コンテナーには保存したくない場合もあります。 このような場合、中間結果データをローカルの HDFS に保存できます。 実際、HDInsight では、Hive ジョブやその他のプロセスで生成される中間結果の一部が DFS に格納されます。



## BLOB コンテナーの作成

作成する最初の blob を使用する、 [Azure ストレージ アカウント][azure-storage-create]します。 その一環として、このアカウントを使用して作成するオブジェクトを格納する Azure データセンターを指定します。 クラスターとストレージ アカウントは、同じデータセンターに置く必要があります。 Hive メタストア SQL Server データベースと Oozie メタストア SQL Server データベースも、同じデータセンターに配置する必要があります。

作成される各 BLOB は、どこにあるとしても、Azure ストレージ アカウント内のコンテナーに属します。 このコンテナーは、HDInsight の外部で作成された既存の BLOB ストレージ コンテナーか、HDInsight クラスター用に作成されたコンテナーであってもかまいません。


既定の Blob コンテナーには、ジョブ履歴やログなどのクラスター固有の情報が格納されます。 既定の BLOB コンテナーと複数の HDInsight クラスターを共有しないでください。 ジョブの履歴が破損する可能性があり、クラスターは不適切な動作をします。 各クラスターで別のコンテナーを使用し、既定のストレージ アカウントではなく、関連するすべてのクラスターのデプロイメントで指定された、リンクされているストレージ アカウントに共有データを格納することをお勧めします。 リンクされたストレージ アカウントを構成する方法については、次を参照してください。 [作成 HDInsight クラスターの][hdinsight-creation]です。 ただし、元の HDInsight クラスターを削除した後でも既定のストレージ コンテナーを再利用できます。 HBase クラスターでは、削除された HBase クラスターで使用される既定の BLOB ストレージ コンテナーを使用して、新しい HBase クラスターを作成することで、HBase テーブルのスキーマとデータを実際に保持できます。


### Azure ポータルの使用

ポータルから HDInsight クラスターを作成する際、既存のストレージ アカウントを使用するか、新しいストレージ アカウントを作成するオプションを利用できます。

![HDInsight Hadoop 作成データ ソース](./media/hdinsight-hadoop-use-blob-storage/hdinsight.provision.data.source.png)

###Azure CLI の使用

あれば [インストールされ、Azure CLI が構成されて](../xplat-cli-install.md), 、ストレージ アカウントとコンテナーを次のコマンドを使用することができます。

    azure storage account create <storageaccountname> --type LRS

> [AZURE.NOTE]  `--type` パラメーターは、ストレージ アカウントのレプリケート方法を示します。 詳細については、次を参照してください。 [Azure Storage のレプリケーション](../storage-redundancy.md)します。 ZRS は、ページ BLOB、ファイル、テーブル、キューをサポートしないため、使用しないでください。

ストレージ アカウントを配置する地理的リージョンを指定するよう要求されます。 ストレージ アカウントは、HDInsight クラスターの作成を計画しているリージョンと同じリージョンに作成する必要があります。

ストレージ アカウントを作成した後は、次のコマンドを使用して、ストレージ アカウント キーを取得します。

    azure storage account keys list <storageaccountname>

コンテナーを作成するには、次のコマンドを使用します。

    azure storage container create <containername> --account-name <storageaccountname> --account-key <storageaccountkey>

### Azure PowerShell の使用

場合する [インストールされ、Azure PowerShell を構成][powershell-install], 、ストレージ アカウントとコンテナーを作成する Azure PowerShell プロンプトで次を使用することができます。

    $SubscriptionID = "<Your Azure Subscription ID>"
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    
    $StorageAccountName = "<New Azure Storage Account Name>"
    $containerName = "<New Azure Blob Container Name>"
    
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionID
    
    # Create resource group
    New-AzureRmResourceGroup -name $ResourceGroupName -Location $Location
    
    # Create default storage account
    New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Location $Location -Type Standard_LRS 
    
    # Create default blob containers
    $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -StorageAccountName $StorageAccountName |  %{ $_.Key1 }
    $destContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    New-AzureStorageContainer -Name $containerName -Context $destContext

## BLOB ストレージ内のファイルの指定

HDInsight から BLOB ストレージ内のファイルにアクセスするための URI スキームは次のとおりです。

    wasb[s]://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>


> [AZURE.NOTE] (HDInsight emulator で実行されている) ストレージ エミュレーターでファイルを指定するための構文は <i>wasb://& lt;ContainerName & gt;@storageemulator</i>します。



URI スキームは、暗号化されていないアクセスを提供 (で、 *wasb:* プレフィックス) と SSL で暗号化されたアクセス (と *wasbs*)。 使用することをお勧め *wasbs* 可能な限り、Azure で同じデータ センター内のデータにアクセスするときにもします。

&lt;BlobStorageContainerName&gt; では、Azure BLOB ストレージでコンテナーの名前を指定します。
&lt;StorageAccountName&gt; では、Azure ストレージ アカウントの名前を指定します。 完全修飾ドメイン名 (FQDN) を指定する必要があります。

&lt;BlobStorageContainerName&gt; と &lt;StorageAccountName&gt; を両方とも指定しない場合は、既定のファイル システムが使用されます。 既定のファイル システム上にあるファイルに関しては、相対パスか絶対パスを使用できます。 たとえば、 *hadoop-examples.jar mapreduce* 、次のいずれかを使用する HDInsight クラスターに付属するファイルを参照できます。

    wasb://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
    wasb:///example/jars/hadoop-mapreduce-examples.jar
    /example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] ファイル名は <i>hadoop-examples.jar</i> HDInsight version 2.1 および 1.6 クラスターでします。


&lt;path&gt; は、ファイルまたはディレクトリの HDFS パス名です。 Azure BLOB ストレージのコンテナーにはキーと値のペアが格納されるだけであるため、階層ファイル システムは存在しません。 ただし、BLOB キー内でスラッシュ (/) を使用すると、ディレクトリの区切りと見なされます。 対応する blob 名などの *hadoop-examples.jar mapreduce* は。

    example/jars/hadoop-mapreduce-examples.jar

> [AZURE.NOTE] HDInsight の外部から blob を使用する場合ほとんどのユーティリティは WASB 形式が認識されず、代わりになどの基本的なパスの形式が要求 `example/jars/hadoop-mapreduce-examples.jar`します。

## Azure CLI を使用した BLOB へのアクセス

BLOB 関連のコマンドを一覧表示するには、次のコマンドを使用します。

    azure storage blob

**Azure CLI を使用してファイルをアップロードする例**

    azure storage blob upload <sourcefilename> <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Azure CLI を使用してファイルをダウンロードする例**

    azure storage blob download <containername> <blobname> <destinationfilename> --account-name <storageaccountname> --account-key <storageaccountkey>

**Azure CLI を使用してファイルを削除する例**

    azure storage blob delete <containername> <blobname> --account-name <storageaccountname> --account-key <storageaccountkey>

**Azure CLI を使用してファイルを一覧表示する例**

    azure storage blob list <containername> <blobname|prefix> --account-name <storageaccountname> --account-key <storageaccountkey>

## Azure PowerShell を使用した BLOB へのアクセス

> [AZURE.NOTE] このセクションのコマンドは、PowerShell を使用して blob に格納されているデータにアクセスする基本的な例を提供します。 HDInsight を操作するためにカスタマイズされているフル機能の例を参照してください、 [HDInsight Tools](https://github.com/Blackmist/hdinsight-tools)します。

BLOB 関連のコマンドレットを一覧表示するには、次のコマンドを使用します。

    Get-Command *blob*

![BLOB 関連の PowerShell コマンドレットの一覧。][img-hdi-powershell-blobcommands]

###ファイルのアップロード

参照してください [データを HDInsight にアップロードする][hdinsight-upload-data]です。

###ファイルをダウンロードする

次のスクリプトは、ブロック BLOB を現在のフォルダーにダウンロードします。 スクリプトを実行する前に、書き込みアクセス許可があるフォルダーにディレクトリを変更します。

    $resourceGroupName = "<AzureResourceGroupName>"
    $storageAccountName = "<AzureStorageAccountName>"   # The storage account used for the default file system specified at creation.
    $containerName = "<BlobStorageContainerName>"  # The default file system container has the same name as the cluster.
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.
    
    # Use Add-AzureAccount if you haven't connected to your Azure subscription
    Login-AzureRmAccount 
    Select-AzureRmSubscription -SubscriptionID "<Your Azure Subscription ID>"
    
    Write-Host "Create a context object ... " -ForegroundColor Green
    $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $storageAccountName | %{ $_.key1 }
    $storageContext = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey  
    
    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $ContainerName -Blob $blob -Context $storageContext -Force
    
    Write-Host "List the downloaded file ..." -ForegroundColor Green
    cat "./$blob"

リソース グループ名とクラスター名を指定して、次のコードを使用できます。

    $resourceGroupName = "<AzureResourceGroupName>"
    $clusterName = "<HDInsightClusterName>"
    $blob = "example/data/sample.log" # The name of the blob to be downloaded.
    
    $cluster = Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $clusterName
    $defaultStorageAccount = $cluster.DefaultStorageAccount -replace '.blob.core.windows.net'
    $defaultStorageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $resourceGroupName -Name $defaultStorageAccount |  %{ $_.Key1 }
    $defaultStorageContainer = $cluster.DefaultStorageContainer
    $storageContext = New-AzureStorageContext -StorageAccountName $defaultStorageAccount -StorageAccountKey $defaultStorageAccountKey 
    
    Write-Host "Download the blob ..." -ForegroundColor Green
    Get-AzureStorageBlobContent -Container $defaultStorageContainer -Blob $blob -Context $storageContext -Force

###ファイルの削除


    Remove-AzureStorageBlob -Container $containerName -Context $storageContext -blob $blob

###ファイルの一覧表示

    Get-AzureStorageBlob -Container $containerName -Context $storageContext -prefix "example/data/"

###定義されていないストレージ アカウントを使用する Hive クエリの実行

この例では、作成プロセス中に定義されていないストレージ アカウントからフォルダーを一覧表示する方法を示します。
    $clusterName ="<HDInsightClusterName>"

    $undefinedStorageAccount = "<UnboundedStorageAccountUnderTheSameSubscription>"
    $undefinedContainer = "<UnboundedBlobContainerAssociatedWithTheStorageAccount>"

    $undefinedStorageKey = Get-AzureStorageKey $undefinedStorageAccount | %{ $_.Primary }

    Use-AzureRmHDInsightCluster $clusterName

    $defines = @{}
    $defines.Add("fs.azure.account.key.$undefinedStorageAccount.blob.core.windows.net", $undefinedStorageKey)

    Invoke-AzureRmHDInsightHiveJob -Defines $defines -Query "dfs -ls wasb://$undefinedContainer@$undefinedStorageAccount.blob.core.windows.net/;"

## 次のステップ

この記事では、HDInsight で HDFS と互換性のある Azure BLOB ストレージを使用する方法と、Azure BLOB ストレージが HDInsight の重要なコンポーネントであることについて説明しました。 これにより、スケーラブルな長期的なアーカイブ データに優れたソリューションで Azure Blob ストレージをビルドして HDInsight を使用して、格納された構造化および非構造化データ内の情報のロックを解除することができます。

詳細については、次を参照してください。

* [Azure HDInsight の概要][hdinsight-get-started]
* [HDInsight へのデータのアップロード][hdinsight-upload-data]
* [HDInsight での Hive の使用][hdinsight-use-hive]
* [HDInsight での Pig の使用][hdinsight-use-pig]

[powershell-install]: ../install-configure-powershell.md
[hdinsight-creation]: hdinsight-provision-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-tutorial-get-started-windows.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[blob-storage-restAPI]: http://msdn.microsoft.com/library/windowsazure/dd135733.aspx
[azure-storage-create]: ../storage-create-storage-account.md

[img-hdi-powershell-blobcommands]: ./media/hdinsight-hadoop-use-blob-storage/HDI.PowerShell.BlobCommands.png
[img-hdi-quick-create]: ./media/hdinsight-hadoop-use-blob-storage/HDI.QuickCreateCluster.png
[img-hdi-custom-create-storage-account]: ./media/hdinsight-hadoop-use-blob-storage/HDI.CustomCreateStorageAccount.png  

