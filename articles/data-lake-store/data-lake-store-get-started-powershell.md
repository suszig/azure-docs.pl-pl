<properties 
   pageTitle="Data Lake Store の使用 | Azure" 
   description="Azure PowerShell を使用して、Data Lake Store アカウントを作成し、基本的な操作を実行する" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="12/04/2015"
   ms.author="nitinme"/>

# Azure PowerShell で Azure Data Lake Analytics の使用を開始する

> [AZURE.SELECTOR]
- [ポータルの使用](data-lake-store-get-started-portal.md)
- [PowerShell の使用](data-lake-store-get-started-powershell.md)
- [.NET SDK の使用](data-lake-store-get-started-net-sdk.md)
- [Azure CLI の使用](data-lake-store-get-started-cli.md)
- [Node.js の使用](data-lake-store-manage-use-nodejs.md)

Azure PowerShell を使用して、Azure Data Lake Store アカウントを作成し、フォルダーの作成、データ ファイルのアップロードとダウンロード、アカウントの削除などの基本操作を行う方法について説明します。湖のデータ ストアの詳細については、次を参照してください。 [データ湖ストアの概要](data-lake-store-overview.md)します。

## 前提条件

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure サブスクリプションを有効にする** 湖のデータ ストアのパブリック プレビューします。 参照してください [指示](data-lake-store-get-started-portal.md#signup)します。


##Azure PowerShell 1.0 以上をインストールする

最初に、バージョン 0.9x の Azure PowerShell をアンインストールする必要があります。 インストールされている PowerShell のバージョンを確認するには、PowerShell ウィンドウから次のコマンドを実行します。

    Get-Module *azure*
    
以前のバージョンをアンインストールするには、実行 **プログラムと機能** PowerShell 1.0 よりも前である場合は、インストールされているバージョンを削除し、コントロール パネルでします。 

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

## Azure Data Lake Store アカウントを作成する

1. デスクトップで、新しい Azure PowerShell ウィンドウを開き、次のスニペットを入力して Azure アカウントにログインし、サブスクリプションを設定して、Data Lake Store プロバイダーを登録します。 ログインを求められたら、必ず、サブスクリプションの管理者または所有者としてログインしてください。

        # Log in to your Azure account
        Login-AzureRmAccount
        
        # List all the subscriptions associated to your account
        Get-AzureRmSubscription
        
        # Select a subscription 
        Set-AzureRmContext -SubscriptionId <subscription ID>
        
        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore" 


2. Azure Data Lake Store アカウントは、Azure リソース グループに関連付けられます。 まず、Azure リソース グループを作成します。

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Azure リソース グループを作成する](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")

2. Azure Data Lake Store アカウントを作成します。 指定する名前には、小文字と数字のみを含める必要があります。

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Azure Data Lake Store アカウントを作成する](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")

3. アカウントが正常に作成されたことを確認します。

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    この出力になります。 **True**します。

## Azure Data Lake Store でディレクトリ構造を作成する

Azure Data Lake Store アカウントにディレクトリを作成し、データを管理したり格納したりすることができます。 

1. ルート ディレクトリを指定します。

        $myrootdir = "/"

2. という新しいディレクトリを作成 **mynewdirectory** 指定したルートの下です。

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. 新しいディレクトリが正常に作成されたことを確認します。

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    出力は次のように表示されます。

    ![ディレクトリを確認する](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")


## Azure Data Lake Store にデータをアップロードする

データは、ルート レベルで直接 Data Lake Store に、またはアカウント内で作成したディレクトリにアップロードすることができます。 次のスニペット ディレクトリにいくつかのサンプル データをアップロードする方法を示して (**mynewdirectory**) 前のセクションで作成します。

アップロードするには、いくつかのサンプル データを探している場合は取得できます、 **救急データ** フォルダーから、 [Azure Data Lake Git リポジトリ](https://github.com/MicrosoftBigData/AzureDataLake/tree/master/SQLIPSamples/SampleData/AmbulanceData)します。 ファイルをダウンロードし、C:\sampledata\ など、コンピューター上のローカル ディレクトリに格納します。

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## Data Lake Store からデータの名前変更、ダウンロード、および削除を行う

ファイルの名前を変更するには、次のコマンドを使用します。

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

ファイルをダウンロードするには、次のコマンドを使用します。

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

ファイルを削除するには、次のコマンドを使用します。

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv 
    
求められたら、入力 **Y** アイテムを削除します。 削除する複数のファイルがある場合は、すべてのパスをコンマで区切って指定できます。

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## Azure Data Lake Store アカウントを削除する

Data Lake Store アカウントを削除するには、以下のコマンドを使用します。

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

求められたら、入力 **Y** 、アカウントを削除します。


## Data Lake Store アカウントの他の作成方法

- [Azure プレビュー ポータルで Azure Data Lake Store の使用を開始する](data-lake-store-get-started-portal.md)
- [.NET SDK で Azure Data Lake Store の使用を開始する](data-lake-store-get-started-net-sdk.md)
- [Azure CLI で Azure Data Lake Store の使用を開始する](data-lake-store-get-started-cli.md)


## 次のステップ

- [Data Lake Store のデータをセキュリティで保護する](data-lake-store-secure-data.md)
- [Data Lake Store で Azure Data Lake Analytics を使用する](data-lake-analytics-get-started-portal.md)
- [Data Lake Store で Azure HDInsight を使用する](data-lake-store-hdinsight-hadoop-use-portal.md)



