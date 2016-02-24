<properties 
   pageTitle="Azure PowerShell を使用する Azure Data Lake Analytics の管理 | Azure" 
   description="Data Lake Analytics のジョブ、データ ソース、ユーザーの管理方法について説明します。 " 
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

# Azure PowerShell を使用する Azure Data Lake Analytics の管理

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure PowerShell を使用して、Azure Data Lake Analytics のアカウント、データ ソース、ユーザー、およびジョブを管理する方法について説明します。 他のツールを使用する管理のトピックを表示する場合は、上のタブ セレクターをクリックします。

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 [Get Azure 無料評価版] https://azure.microsoft.com/en-us/pricing/free-trial/を参照)。


<!-- ################################ -->
<!-- ################################ -->


##Azure PowerShell 1.0 以上をインストールする

最初に 0.9x バージョンをアンインストールする必要があります。

インストールされている PowerShell のバージョンを確認するには:

    Get-Module *azure*
    
以前のバージョンをアンインストールするには、コントロール パネルで [プログラムと機能] を実行します。 

Azure PowerShell をインストールするための主な 2 つのオプションは次のとおりです。 

- [PowerShell Gallery](https://www.powershellgallery.com/)します。 管理者特権の PowerShell ISE または管理者特権の Windows PowerShell コンソールから、次のコマンドを実行します。

        # Install the Azure Resource Manager modules from PowerShell Gallery
        Install-Module AzureRM
        Install-AzureRM
        
        # Install the Azure Service Management module from PowerShell Gallery
        Install-Module Azure
        
        # Import AzureRM modules for the given version manifest in the AzureRM module
        Import-AzureRM
        
        # Import Azure Service Management module
        Import-Module Azure

    詳細については、次を参照してください。 [PowerShell Gallery](https://www.powershellgallery.com/)します。

- [Microsoft Web Platform Installer (WebPI)](http://aka.ms/webpi-azps)します。 Azure PowerShell 0.9.x をインストールしている場合は、0.9.x のアンインストールを要求するメッセージが表示されます Azure PowerShell モジュールを PowerShell ギャラリーからインストールした場合、一貫した Azure PowerShell 環境を保つため、インストーラーにより、インストール前にモジュールを削除することが求められます。 手順については、次を参照してください。 [WebPI を使用して Azure PowerShell 1.0 をインストール](https://azure.microsoft.com/blog/azps-1-0/)します。

WebPI は月次の更新プログラムを受け取ります。 PowerShell ギャラリーは、継続的に更新プログラムを受け取ります。 PowerShell ギャラリーからのインストールを選んだ場合は、これが Azure PowerShell で最新および最良の点について情報を取得できる最初のチャネルになります。

**コマンドレットを一覧表示する**:

    Get-Command *Azure*DataLakeAnalytics*

**Azure に接続する場合に、次のコマンドレットを使用して**:

    Login-AzureRmAccount
    Get-AzureRmSubscription  # for finding the Azure Subscription ID
    Set-AzureRmContext -SubscriptionID <Azure Subscription ID>
    
## アカウントの管理

Data Lake Analytics ジョブを実行するには、Data Lake Analytics アカウントが必要です。 Azure HDInsight とは異なり料は発生しません Analytics アカウントされていないとき 
ジョブを実行します。  ジョブの実行時にのみ課金されます。  詳細については、次を参照してください。 
[Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。  

###アカウントの作成

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeStoreName = "<DataLakeAccountName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $location = "<Microsoft Data Center>"
    
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
        -Name $dataLakeAnalyticsAccountName `
        -ResourceGroupName $resourceGroupName `
        -Location $location `
        -DefaultDataLake $dataLakeStoreName
    
    Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
    Get-AzureRmDataLakeAnalyticsAccount `
        -ResourceGroupName $resourceGroupName `
        -Name $dataLakeAnalyticsAccountName  

Azure リソース グループのテンプレートを使用することもできます。 Data Lake 分析アカウントと依存データ湖ストア アカウントを作成するためのテンプレートが、 [付録 A](#appendix-a)します。 .json テンプレートを使用してファイルにテンプレートを保存してから、以下の PowerShell スクリプトを使用してそれを呼び出します。


    $AzureSubscriptionID = "<Your Azure Subscription ID>"
    
    $ResourceGroupName = "<New Azure Resource Group Name>"
    $Location = "EAST US 2"
    $DefaultDataLakeStoreAccountName = "<New Data Lake Store Account Name>"
    $DataLakeAnalyticsAccountName = "<New Data Lake Analytics Account Name>"
    
    $DeploymentName = "MyDataLakeAnalyticsDeployment"
    $ARMTemplateFile = "E:\Tutorials\ADL\ARMTemplate\azuredeploy.json"  # update the Json template path 
    
    Login-AzureRmAccount
    
    Select-AzureRmSubscription -SubscriptionId $AzureSubscriptionID
    
    # Create the resource group
    New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    # Create the Data Lake Analytics account with the default Data Lake Store account.
    $parameters = @{"adlAnalyticsName"=$DataLakeAnalyticsAccountName; "adlStoreName"=$DefaultDataLakeStoreAccountName}
    New-AzureRmResourceGroupDeployment -Name $DeploymentName -ResourceGroupName $ResourceGroupName -TemplateFile $ARMTemplateFile -TemplateParameterObject $parameters 

 
###アカウントのリスト

現在のサブスクリプション内の Data Lake Analytics アカウントをリストします。

    Get-AzureRmDataLakeAnalyticsAccount
    
出力は次のようになります。

    Id         : /subscriptions/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx/resourceGroups/learn1021rg/providers/Microsoft.DataLakeAnalytics/accounts/learn1021adla
    Location   : eastus2
    Name       : learn1021adla
    Properties : Microsoft.Azure.Management.DataLake.Analytics.Models.DataLakeAnalyticsAccountProperties
    Tags       : {}
    Type       : Microsoft.DataLakeAnalytics/accounts

特定のリソース グループ内の Data Lake Analytics アカウントをリストします。

    Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName

特定の Data Lake Analytics アカウントの詳細を取得します。

    Get-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

特定の Data Lake Analytics アカウントの存在をテストします。

    Test-AzureRmDataLakeAnalyticsAccount -Name $adlAnalyticsAccountName

コマンドレットが返す **True** または **False**します。

###Data Lake Analytics アカウントの削除

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Remove-AzureRmDataLakeAnalyticsAccount -Name $dataLakeAnalyticsAccountName 

Analytics アカウントを削除しても、従属する Data Lake ストレージ アカウントは削除されません。 次の例では、Data Lake Analytics アカウントと既定の Data Lake Store アカウントを削除します。

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount

    Remove-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName 
    Remove-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName

<!-- ################################ -->
<!-- ################################ -->
## アカウント データ ソースの管理

Data Lake Analytics では現在、以下のデータ ソースがサポートされています。

- [Azure Data Lake Storage](data-lake-storage-overview.md)
- [Azure Storage (Azure Storage)](storage-introduction.md)

Analytics アカウントを作成する際は、既定値である Azure データ湖のストレージ アカウントを指定する必要があります。 
入力します。 既定の Data Lake Store アカウントは、ジョブ メタデータとジョブ監査ログの格納に使用されます。 用意した後 
Analytics アカウントを作成するには、追加のデータ湖のストレージ アカウントまたは Azure ストレージ アカウントを追加できます。 

### 既定の Data Lake Store アカウントの検索

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount


### 他の Azure BLOB ストレージ アカウントの追加

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureStorageAccountName = "<AzureStorageAccountName>"
    $AzureStorageAccountKey = "<AzureStorageAccountKey>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -AccountName $dataLakeAnalyticName -AzureBlob $AzureStorageAccountName -AccessKey $AzureStorageAccountKey

### 他の Data Lake Store アカウントの追加

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    $AzureDataLakeName = "<DataLakeStoreName>"
    
    Add-AzureRmDataLakeAnalyticsDataSource -ResourceGroupName $resourceGroupName -AccountName $dataLakeAnalyticName -DataLake $AzureDataLakeName 

### データ ソースの一覧表示:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DataLakeStoreAccounts
    (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.StorageAccounts
    


<!-- ################################ -->
<!-- ################################ -->
## ジョブの管理

ジョブを作成するには、Data Lake Analytics アカウントが必要です。  詳細については、次を参照してください。 [管理 Data Lake 分析アカウント](#manage-data-lake-analytics-accounts)します。

### ジョブのリスト

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName
    
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName -State Running, Queued
    #States: Accepted, Compiling, Ended, New, Paused, Queued, Running, Scheduling, Starting
    
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName -Result Cancelled
    #Results: Cancelled, Failed, None, Successed 
    
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName -Name <Job Name>
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName -Submitter <Job submitter>

    # List all jobs submitted on January 1 (local time)
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName `
        -SubmittedAfter "2015/01/01"
        -SubmittedBefore "2015/01/02"   

    # List all jobs that succeeded on January 1 after 2 pm (UTC time)
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName `
        -State Ended
        -Result Succeeded
        -SubmittedAfter "2015/01/01 2:00 PM -0"
        -SubmittedBefore "2015/01/02 -0"

    # List all jobs submitted in the past hour
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName `
        -SubmittedAfter (Get-Date).AddHours(-1)

### ジョブの詳細の取得

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"
    Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName -JobID <Job ID>
    
### ジョブの送信

    $dataLakeAnalyticsAccountName = "<DataLakeAnalyticsAccountName>"

    #Pass script via path
    Submit-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName `
        -Name $jobName `
        -ScriptPath $scriptPath

    #Pass script contents
    Submit-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName `
        -Name $jobName `
        -Script $scriptContents

> [AZURE.NOTE] ジョブの既定の優先順位は 1000年で、ジョブの並列処理の既定の次数は 1 です。


### ジョブの取り消し

    Stop-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticName `
        -JobID $jobID


## カタログ項目の管理

U-SQL カタログを使用して、U-SQL スクリプトで共有できるように、データとコードを構成します。 カタログでは、Azure Data Lake のデータを使用して可能な限り最高のパフォーマンスを実現できます。 詳細については、次を参照してください。 [U-SQL カタログ](data-lake-analytics-use-u-sql-catalog.md)します。

###カタログ項目のリスト

    #List databases
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -AccountName $adlAnalyticsAccountName `
        -ItemType Database
    
    
    
    #List tables
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -AccountName $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo"

###カタログ項目の詳細の取得 

    #Get a database
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -AccountName $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"
    
    #Get a table
    Get-AzureRmDataLakeAnalyticsCatalogItem `
        -AccountName $adlAnalyticsAccountName `
        -ItemType Table `
        -Path "master.dbo.mytable"

###カタログ アイテムの存在をテストします。

    Test-AzureRmDataLakeAnalyticsCatalogItem  `
        -AccountName $adlAnalyticsAccountName `
        -ItemType Database `
        -Path "master"

###カタログ シークレットの作成
    新しい AzureRmDataLakeAnalyticsCatalogSecret  `
            -AccountName $adlAnalyticsAccountName `
            -DatabaseName"master"'
            -シークレット (Get-credential-username"username"-「、パスワードを入力してください」のメッセージ)

### カタログ シークレットの変更
    Set-AzureRmDataLakeAnalyticsCatalogSecret  `
            -AccountName $adlAnalyticsAccountName `
            -DatabaseName "master" `
            -Secret (Get-Credential -UserName "username" -Message "Enter the password")



###カタログ シークレットの削除
    削除 AzureRmDataLakeAnalyticsCatalogSecret  `
            -AccountName $adlAnalyticsAccountName `
            -DatabaseName"master"


## Azure リソース マネージャー グループの使用

アプリケーションが通常、web アプリ、データベース、データベース サーバー、ストレージ、たとえば、多くのコンポーネントの構成します。
およびサード パーティのサービスです。 Azure リソース マネージャー (ARM) では、アプリケーションでのリソースを操作できます。 
Azure リソース グループと呼ばれるグループです。 デプロイ、更新、監視、すべての削除、 
1 つ、組織的な操作で、アプリケーションのリソース。 展開用テンプレートを使用します。 
テンプレートは、テスト、ステージングと運用環境などのさまざまな環境を操作できます。 課金を明確にすることができます。 
グループ全体のロールアップ コストを表示することによって組織の 詳細については、次を参照してください [Azure。
リソース マネージャーの概要](resource-group-overview.md)します。 

Data Lake Analtyics サービスには、次のコンポーネントを含めることができます。

- Azure Data Lake Analytics アカウント
- 必要な既定の Azure Data Lake Storage アカウント
- 追加の Azure Data Lake Storage アカウント
- 追加の Azure Storage アカウント

管理しやすくするために 1 つの ARM グループの下にこれらすべてのコンポーネントを作成することができます。

![Azure Data Lake Analytics のアカウントとストレージ](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Data Lake Analytics アカウントと従属するストレージ アカウントは同じ Azure データ センターに配置する必要があります。
ただし、ARM グループは別のデータ センターに配置できます。  

##関連項目 

- [Microsoft Azure Data Lake Analytics の概要](data-lake-analytics-overview.md)
- [Azure ポータルで Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-portal.md)
- [Azure ポータルを使用する Azure Data Lake Analytics の管理](data-lake-analytics-use-portal.md)
- [Azure ポータルを使用する Azure Data Lake Analytics ジョブの監視とトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)

##付録 A - Data Lake Analytics ARM テンプレート

以下の ARM テンプレートを使用して、Data Lake Analytics アカウントとそれに従属する Data Lake Store アカウントをデプロイすることができます。  それを json ファイルとして保存してから、PowerShell スクリプトを使用してテンプレートを呼び出します。 詳細については、次をご覧ください。
[Azure リソース マネージャー テンプレートを使用してアプリケーションを配置](resource-group-template-deploy.md#deploy-with-powershell) と [Azure リソース マネージャーの作成テンプレート](resource-group-authoring-templates.md)します。

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "adlAnalyticsName": {
                "type": "string",
                "metadata": {
                    "description": "The name of the Data Lake Analytics account to create."
                }
            },
            "adlStoreName": {
                "type": "string",
                "metadata": {
                    "description": "The name of the Data Lake Store account to create."
                }
            }
        },
        "resources": [{
            "name": "[parameters('adlAnalyticsName')]",
            "type": "Microsoft.DataLakeAnalytics/accounts",
            "location": "East US 2",
            "apiVersion": "2015-10-01-preview",
            "dependsOn": ["[concat('Microsoft.DataLakeStore/accounts/',parameters('adlStoreName'))]"],
            "tags": {
                
            },
            "properties": {
                "defaultDataLakeAccount": "[parameters('adlStoreName')]"
                }
            }
        },
        {
            "name": "[parameters('adlName')]",
            "type": "Microsoft.DataLakeStore/accounts",
            "location": "East US 2",
            "apiVersion": "2015-10-01-preview",
            "dependsOn": [],
            "tags": {
                
            }
        }],
        "outputs": {
            "adlAnalyticsAccount": {
                "type": "object",
                "value": "[reference(resourceId('Microsoft.DataLakeAnalytics/accounts',parameters('adlAnalyticsName')))]"
            },
            "adlStoreAccount": {
                "type": "object",
                "value": "[reference(resourceId('Microsoft.DataLakeStore/accounts',parameters('adlStoreName')))]"
            }
        }
    }
