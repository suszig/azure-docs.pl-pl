<properties 
   pageTitle="Azure PowerShell で Azure Data Lake Analytics の使用を開始する | Azure" 
   description="Azure PowerShell を使用して、Data Lake Store アカウントを作成し、U-SQL を使用して Data Lake Analytics ジョブを作成してから、ジョブを送信する方法について説明します。 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/01/2015"
   ms.author="jgao"/>


# チュートリアル: Azure PowerShell で Azure Data Lake Analytics の使用を開始する

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure PowerShell を使用して Azure Data Lake 分析アカウントを作成、Data Lake 分析を定義する方法を説明します。
ジョブの [U SQL](data-lake-analytics-u-sql-get-started.md), 、Data Lake Analtyic アカウントにジョブを送信します。 サブスクリプションの 
Data Lake 分析に関する情報を参照してください [Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。

このチュートリアルをタブ区切り値 (TSV) ファイルに読み取り、コンマに変換ジョブを開発します 
区切り (CSV) ファイルです。 サポートされている他のツールを使用する同じチュートリアルを読み進めるには、このセクションの上部にあるタブをクリックします。

**基本的な Data Lake Analytics のプロセス:**

![Azure Data Lake Analytics プロセスのフロー図](./media/data-lake-analytics-get-started-portal/data-lake-analytics-process.png)

1. Data Lake Analytics アカウントを作成します。
2. ソース データを準備します。 Data Lake Analytic ジョブでは、Azure Data Lake Store アカウントまたは Azure Blob Storage アカウントからデータを読み取ることができます。
3. U-SQL スクリプトを開発します。
4. ジョブ (U-SQL スクリプト) を Data Lake Analytics アカウントに送信します。 指示に従ってデータを処理するジョブは、ソース データから読み取ります 
U SQL スクリプトにし、データストア Lake アカウント、または Blob ストレージ アカウントへの出力を保存します。


**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure PowerShell を実行できるワークステーション**。 参照してください [Azure PowerShell 1.0 をインストールし、大きい](data-lake-analytics-manage-use-powershell.md#install-azure-powershell-10-and-greater)します。

## Data Lake Analytics アカウントを作成する

ジョブを実行するには、Data Lake Analytics アカウントが必要です。 Data Lake Analytics アカウントを作成するには、以下を指定する必要があります。

- **Azure リソース グループ**: Data Lake Analytics アカウントは、Azure リソース グループ内に作成する必要があります。 [Azure リソース マネージャー](resource-group-overview.md) グループとして、アプリケーション内のリソースを使用することができます。 アプリケーションのこれらすべてのリソースを、1 回の連携した操作でデプロイ、更新、または削除できます。

    サブスクリプションのリソース グループを列挙するには:

        Get-AzureRmResourceGroup

    新しいリソース グループを作成するには:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"

- **Data Lake Analytics アカウント名**
- **場所**: Data Lake Analytics をサポートするいずれかの Azure データ センター。
- **既定の Data Lake アカウント**: 各 Data Lake Analytics アカウントには既定の Data Lake アカウントがあります。

    新しい Data Lake アカウントを作成するには:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

    > [AZURE.NOTE] Data Lake アカウント名には小文字と数字のみを含める必要があります。



**Data Lake Analytics アカウントを作成するには**

1. Windows ワークステーションで、PowerShell ISE を開きます。
2. 次のスクリプトを実行します。

     $resourceGroupName = "<ResourceGroupName>"
     $dataLakeStoreName = "<DataLakeAccountName>"
     $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
     $location = "East US 2"
    
     Write-Host "Create a resource group ..." -ForegroundColor Green
     New-AzureRmResourceGroup `
         -Name  $resourceGroupName `
         -Location $location
    
     Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
     New-AzureRmDataLakeStoreAccount `
         -ResourceGroupName $resourceGroupName `
         -Name $dataLakeStoreName `
         -Location $location 
    
     Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
     New-AzureRmDataLakeAnalyticsAccount `
         -Name $dataLakeAnalyticsName `
         -ResourceGroupName $resourceGroupName `
         -Location $location `
         -DefaultDataLake $dataLakeStoreName
    
     Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
     Get-AzureRmDataLakeAnalyticsAccount `
         -ResourceGroupName $resourceGroupName `
         -Name $dataLakeAnalyticsName  


## Data Lake へのデータのアップロード

このチュートリアルでは、いくつかの検索ログを処理します。 検索ログは、Data Lake Store または Azure BLOB ストレージに格納できます。

サンプルの検索ログ ファイルは、パブリック Azure BLOB コンテナーにコピーされました。 次の PowerShell スクリプトを使用して、ファイルをワークステーションにダウンロードしてから、Data Lake Analytics アカウントの既定の Data Lake Store アカウントにファイルをアップロードします。

    $dataLakeStoreName = "<The default Data Lake Store account name>"
    
    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file. 
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.
    
    # Create the temp location  
    New-Item -Path $localFolder -ItemType Directory -Force 
    
    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder
    
    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

次の PowerShell スクリプトは、Data Lake Analytics アカウントの既定の Data Lake Store 名を取得する方法を示しています。


    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount

>[AZURE.NOTE] Azure ポータルでは、既定の Data Lake Store アカウントにサンプル データ ファイルをコピーするためのユーザー インターフェイスが提供されます。 手順については、次を参照してください。 [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md#upload-data-to-the-default-data-lake-store-account)します。

Data Lake Analytics は、Azure BLOB ストレージにもアクセスできます。 Azure Blob ストレージにデータをアップロードする、次を参照してください。 [Azure Storage で Azure PowerShell を使用して](storage-powershell-guide-full.md)します。

## Data Lake Analytics ジョブを送信する

Data Lake Analtyics ジョブは U-SQL 言語で記述されます。 U SQL の詳細については、次を参照してください。 [U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md) と [U SQL 言語リファレンス](http://go.microsoft.com/fwlink/?LinkId=691348)します。

**Data Lake Analytics ジョブ スクリプトを作成するには**

- 次の U-SQL スクリプトでテキスト ファイルを作成し、ワークステーションにテキスト ファイルを保存します。

      @searchlog =
          EXTRACT UserId          int,
                  Start           DateTime,
                  Region          string,
                  Query           string,
                  Duration        int?,
                  Urls            string,
                  ClickedUrls     string
          FROM "/Samples/Data/SearchLog.tsv"
          USING Extractors.Tsv();
    
      OUTPUT @searchlog   
          TO "/Output/SearchLog-from-Data-Lake.csv"
      USING Outputters.Csv();

  この U-SQL スクリプトでは、**Extractors.Tsv()** を使用してソース データ ファイルを読み取ってから、**Outputters.Csv()** を使用して csv ファイルを作成します。

  ソース ファイルを別の場所にコピーしない限り、2 つのパスを変更しないでください。 存在しない場合、Data Lake Analytics は出力フォルダーを作成します。

  既定の Data Lake アカウントに格納されたファイルの相対パスを使用する方が簡単です。 絶対パスを使用することもできます。 たとえば、次のように入力します。

      adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

  絶対パスを使用して、リンクされたストレージ アカウント内のファイルにアクセスする必要があります。 リンクされた Azure ストレージ アカウントに格納されているファイルの構文は以下のとおりです。

      wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

  >[AZURE.NOTE] パブリック BLOB またはパブリック コンテナーのアクセス許可を持つ Azure BLOB コンテナーは、現在サポートされていません。    


**ジョブを送信するには**

1. Windows ワークステーションで、PowerShell ISE を開きます。
2. 次のスクリプトを実行します。

     $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
     $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"
    
     Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript 
    
     While (($t = Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId).State -ne "Ended"){
         Write-Host "Job status: "$t.State"..."
         Start-Sleep -seconds 5
     }
    
     Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

 このスクリプトでは、U-SQL スクリプト ファイルが c:\tutorials\data-lake-analytics\copyFile.usql に格納されます。 適宜、ファイル パスを更新してください。

ジョブが完了したら、以下のコマンドレットを使用し、ファイルをリストしてダウンロードできます。

    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"
    
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount
    
    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"
    
    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## 関連項目

- 他のツールを使用する同じチュートリアルを表示するには、ページの上部にあるタブ セレクターをクリックします。
- Complexed クエリを表示するには、次を参照してください。 [分析の web サイトを Azure Data Lake 分析を使用してログに記録](data-lake-analytics-analyze-weblogs.md)します。
- U SQL アプリケーションの開発開始するを参照してください。 [Data Lake Tools を使用して、Visual Studio の開発の U SQL スクリプト](data-lake-analytics-data-lake-tools-get-started.md)します。
- U SQL については、次を参照してください。 [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)します。
- 管理タスクを参照してください。 [管理 Azure Data Lake 分析 Azure ポータルを使用して](data-lake-analytics-manage-use-portal.md)します。
- Data Lake 分析の概要を取得するには、次を参照してください。 [Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。






