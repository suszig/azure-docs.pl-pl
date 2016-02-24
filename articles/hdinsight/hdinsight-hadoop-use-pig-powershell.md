<properties
   pageTitle="HDInsight での Hadoop Pig と PowerShell の使用 | Microsoft Azure"
   description="Azure PowerShell を使用して HDInsight で Pig ジョブを Hadoop クラスターに送信する方法を説明します。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="paulettm"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="12/04/2015"
   ms.author="larryfr"/>

#PowerShell を使用した Pig ジョブの実行

[AZURE.INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

このドキュメントでは、Azure PowerShell を使用して HDInsight クラスターで Pig ジョブを Hadoop に送信する方法を説明します。 Pig では map 関数や reduce 関数ではなく、データ変換をモデル化する言語 (Pig Latin) を使用して MapReduce ジョブを記述できます。

> [AZURE.NOTE] このドキュメントには、この例で使用する Pig Latin ステートメントの実行の詳細な説明は行いません。 この例で使用される Pig Latin については「[HDInsight での Pig Latin と Hadoop の使用](hdinsight-use-pig.md)」をご覧ください。

##<a id="prereq"></a>前提条件

この記事の手順を完了するには、次のものが必要です。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **Azure PowerShell を実行できるワークステーション**します。 参照してください [Azure PowerShell 1.0 をインストールし、大きい](hdinsight-administer-use-powershell.md#install-azure-powershell-10-and-greater)します。


##<a id="powershell"></a>PowerShell を使用した Pig ジョブの実行

Azure PowerShell には *コマンドレット* HDInsight で Pig ジョブをリモートで実行するためのです。 これは、HDInsight クラスター上で実行する [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) への REST 呼び出し (旧称: Templeton) を内部的に使用することで機能します。

リモート HDInsight クラスターで Pig ジョブを実行するときに次のコマンドレットを使用します。

* **ログイン AzureRmAccount**: Azure サブスクリプションに Azure PowerShell の認証

* **新しい AzureRmHDInsightPigJobDefinition**: 新たに作成 *ジョブ定義* 指定された Pig Latin ステートメントを使用して、

* **開始 AzureRmHDInsightJob**: ジョブ定義を HDInsight に送信、ジョブを開始および返します、 *ジョブ* ジョブの状態を確認するために使用できるオブジェクト

* **待機 AzureRmHDInsightJob**: ジョブ オブジェクトを使用して、ジョブの状態を確認します。 ジョブの完了を待機するか、待機時間が上限に達します。

* **Get AzureRmHDInsightJobOutput**: ジョブの出力を取得するために使用

これらのコマンドレットを使用して、HDInsight クラスターでジョブを実行するための手順を以下に示します。

1. として次のコードを保存して、エディターを使用して **pigjob.ps1**します。 置き換える必要があります **CLUSTERNAME** 、HDInsight クラスターの名前に置き換えます。

        #Login to your Azure subscription
        Login-AzureRmAccount
        #Get credentials for the admin/HTTPs account
        $creds=Get-Credential

        #Specify the cluster name
        $clusterName = "CLUSTERNAME"
        #Where the output will be saved
        $statusFolder = "/tutorial/pig/status"
        
        #Get the cluster info so we can get the resource group, storage, etc.
        $clusterInfo = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        $resourceGroup = $clusterInfo.ResourceGroup
        $storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
        $container=$clusterInfo.DefaultStorageContainer
        $storageAccountKey=Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
            -ResourceGroupName $resourceGroup `
            | %{ $_.Key1 }

        #Store the Pig Latin into $QueryString
        $QueryString =  "LOGS = LOAD 'wasb:///example/data/sample.log';" +
        "LEVELS = foreach LOGS generate REGEX_EXTRACT(`$0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;" +
        "FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;" +
        "GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;" +
        "FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;" +
        "RESULT = order FREQUENCIES by COUNT desc;" +
        "DUMP RESULT;"

        #Create a new HDInsight Pig Job definition
        $pigJobDefinition = New-AzureRmHDInsightPigJobDefinition `
            -Query $QueryString `

        # Start the Pig job on the HDInsight cluster
        Write-Host "Start the Pig job ..." -ForegroundColor Green
        $pigJob = Start-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobDefinition $pigJobDefinition `
            -ClusterCredential $creds

        # Wait for the Pig job to complete
        Write-Host "Wait for the Pig job to complete ..." -ForegroundColor Green
        Wait-AzureRmHDInsightJob `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds

        # Display the output of the Pig job.
        Write-Host "Display the standard output ..." -ForegroundColor Green
        Get-AzureRmHDInsightJobOutput `
            -ClusterName $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds

2. Azure PowerShell コマンド プロンプトを開きます。 ディレクトリの場所を変更、 **pigjob.ps1** ファイルをし、次のコマンドを使用してスクリプトを実行します。

        .\pigjob.ps1
        
    最初に、Azure サブスクリプションへのログインが求められます。 次に、HDInsight クラスターの HTTPS/管理者アカウント名とパスワードの入力が求められます。

7. ジョブが完了すると、次のような情報が返されます。

        Start the Pig job ...
        Wait for the Pig job to complete ...

        Cluster         : CLUSTERNAME.
        HttpEndpoint    : CLUSTERNAME.azurehdinsight.net
        State           : SUCCEEDED
        JobId           : job_1444852971289_0018
        ParentId        :
        PercentComplete : 100% complete
        ExitValue       : 0
        User            : admin
        Callback        :
        Completed       : done
        
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

##<a id="troubleshooting"></a>トラブルシューティング

ジョブの完了時に情報が返されない場合は、処理中にエラーが発生した可能性があります。 このジョブに関するエラー情報を表示するには、末尾に次のコマンドを追加、 **pigjob.ps1** ファイル、保存し、再度実行します。

    # Print the output of the Pig job.
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -DefaultContainer $container `
            -DefaultStorageAccountName $storageAccountName `
            -DefaultStorageAccountKey $storageAccountKey `
            -HttpCredential $creds
            -DisplayOutputType StandardError

これにより、ジョブの実行時にサーバー上の STDERR に書き込まれた情報が返されるため、ジョブ失敗の特定に役立ちます。

##<a id="summary"></a>概要

このように、Azure PowerShell を使用すると、HDInsight クラスターで簡単に Pig ジョブを実行し、ジョブ ステータスを監視し、出力を取得できます。

##<a id="nextsteps"></a>次のステップ

HDInsight での Pig に関する全般的な情報:

* [HDInsight での Pig と Hadoop の使用](hdinsight-use-pig.md)

HDInsight での Hadoop のその他の使用方法に関する情報

* [HDInsight での Hive と Hadoop の使用](hdinsight-use-hive.md)

* [HDInsight での MapReduce と Hadoop の使用](hdinsight-use-mapreduce.md)

