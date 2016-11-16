---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell | Microsoft Docs"
description: "Dowiedz się, jak wykorzystać program Azure PowerShell do utworzenia konta usługi Data Lake Store, utworzenia zadania usługi Data Lake Analytics za pomocą języka U-SQL i przesłania zadania. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/21/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 73d3e5577d0702a93b7f4edf3bf4e29f55a053ed
ms.openlocfilehash: 59efa050944059c737654a3f039a058c50865ea6


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Samouczek: wprowadzenie do pracy z usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Dowiedz się, jak wykorzystać program Azure PowerShell do tworzenia kont usługi Azure Data Lake Analytics, definiowania zadań usługi Data Lake Analytics za pomocą języka [U-SQL](data-lake-analytics-u-sql-get-started.md) oraz wysyłania zadań do kont usługi Data Lake Analytics. Więcej informacji na temat usługi Data Lake Analytics można znaleźć w artykule [Azure Data Lake Analytics overview](data-lake-analytics-overview.md) (Omówienie usługi Azure Data Lake Analytics).

W ramach tego samouczka utworzysz zadanie, które odczytuje zawartość pliku z wartościami rozdzielanymi tabulatorami (TSV) i konwertuje je do pliku z wartościami rozdzielanymi przecinkami (CSV). Aby wykonać kroki opisane w tym samouczku, korzystając z innych obsługiwanych narzędzi, kliknij odpowiednią kartę w górnej części tej sekcji.

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Stacja robocza z programem Azure PowerShell**. Zobacz artykuł [Instalowanie i konfigurowanie programu Azure PowerShell](../powershell-install-configure.md).

## <a name="create-data-lake-analytics-account"></a>Tworzenie konta usługi Data Lake Analytics
Aby można było uruchomić jakiekolwiek zadanie, musisz mieć konto usługi Data Lake Analytics. Aby utworzyć takie konto, należy określić następujące elementy:

* **Grupa zasobów Azure**: konto usługi Data Lake Analytics należy utworzyć w grupie zasobów Azure. Usługa [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) umożliwia pracę z zasobami w aplikacji jak z grupą. Wszystkie zasoby aplikacji można wdrożyć, zaktualizować lub usunąć w jednej, skoordynowanej operacji.  

    Aby wyliczyć grupy zasobów w subskrypcji:

        Get-AzureRmResourceGroup

    Aby utworzyć nową grupę zasobów:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"
* **Nazwa konta usługi Data Lake Analytics**
* **Lokalizacja**: centrum danych Azure, które obsługuje usługę Data Lake Analytics.
* **Domyślne konto usługi Data Lake**: każde konto usługi Data Lake Analytics ma domyślne konto usługi Data Lake.

    Aby utworzyć nowe konto usługi Data Lake:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

  > [!NOTE]
  > Nazwa konta usługi Data Lake może zawierać tylko małe litery i cyfry.
  >
  >

**Aby utworzyć konto usługi Data Lake Analytics**

1. Na stacji roboczej systemu Windows otwórz program PowerShell ISE.
2. Uruchom następujący skrypt:

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

## <a name="upload-data-to-data-lake"></a>Przekazywanie danych do usługi Data Lake
W ramach tego samouczka przetworzymy wybrane dzienniki wyszukiwania.  Dziennik wyszukiwania może być przechowywany w usłudze Data Lake Store lub w usłudze Azure Blob Storage.

Przykładowy plik dziennika wyszukiwania został skopiowany do publicznego kontenera usługi Azure Blob. Skorzystaj ze skryptu programu PowerShell w celu pobrania pliku na stację roboczą, a następnie przekazania pliku do domyślnego konta usługi Data Lake Store w ramach swojego konta usługi Data Lake Analytics.

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

Poniższy skrypt programu PowerShell pokazuje, jak pobrać domyślną nazwę usługi Data Lake Store dla konta usługi Data Lake Analytics:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

> [!NOTE]
> Witryna Azure Portal udostępnia interfejs użytkownika umożliwiający skopiowanie przykładowych plików danych do domyślnego konta usługi Data Lake Store. Aby uzyskać instrukcje, zobacz artykuł [Wprowadzenie do pracy z usługą Azure Data Lake Analytics za pomocą witryny Azure Portal](data-lake-analytics-get-started-portal.md#prepare-source-data).
>
>

Usługa Data Lake Analytics może także uzyskiwać dostęp do usługi Azure Blob Storage.  Aby uzyskać informacje na temat przekazywania danych do usługi Azure Blob Storage, zobacz artykuł [Wykorzystanie programu Azure PowerShell do dostępu do usługi Azure Storage](../storage/storage-powershell-guide-full.md).

## <a name="submit-data-lake-analytics-jobs"></a>Przesyłanie zadań usługi Data Lake Analytics
Zadania usługi Data Lake Analytics są napisane w języku U-SQL. Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL](data-lake-analytics-u-sql-get-started.md) i [Dokumentację języka U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Aby utworzyć skrypt zadania usługi Data Lake Analytics**

* Utwórz plik tekstowy zawierający następujący skrypt w języku U-SQL i zapisz go na swojej stacji roboczej:

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

    Ten skrypt U-SQL odczytuje źródłowy plik danych przy użyciu ekstraktora **Extractors.Tsv()**, a następnie tworzy plik csv przy użyciu ekstraktora **Outputters.Csv()**.

    Nie należy modyfikować tych dwóch ścieżek, jeśli plik źródłowy nie został skopiowany do innej lokalizacji.  Jeśli folder wyjściowy nie istnieje, usługa Data Lake Analytics go utworzy.

    Użycie ścieżek względnych jest łatwiejsze w przypadku plików przechowywanych na domyślnych kontach usługi Data Lake. Można także użyć ścieżek bezwzględnych.  Na przykład:

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Aby uzyskać dostęp do plików na połączonych kontach usługi Storage, należy użyć ścieżek bezwzględnych.  Składnia dla plików przechowywanych na połączonym koncie usługi Azure Storage jest następująca:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

  > [!NOTE]
  > Uprawnienia dostępu do kontenerów obiektów Blob platformy Azure z publicznymi obiektami lub publicznymi kontenerami nie są obecnie obsługiwane.    
  >
  >

**Aby przesłać zadanie**

1. Na stacji roboczej systemu Windows otwórz program PowerShell ISE.
2. Uruchom następujący skrypt:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"

        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    W tym skrypcie plik ze skryptem języka U-SQL jest przechowywany jako c:\tutorials\data-lake-analytics\copyFile.usql. Należy zaktualizować odpowiednio ścieżkę pliku.

Po zakończeniu zadania można użyć następujących poleceń cmdlet, aby wyświetlić i pobrać plik:

    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"

    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount

    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Zobacz też
* Aby wyświetlić ten samouczek przy użyciu innych narzędzi, kliknij odpowiedni selektor karty w górnej części strony.
* Aby uzyskać informacje na temat bardziej złożonego zapytania, zobacz temat [Analizowanie dzienników witryn sieci Web przy użyciu usługi Azure Data Lake Analytics](data-lake-analytics-analyze-weblogs.md).
* Aby rozpocząć tworzenie aplikacji w języku U-SQL, zobacz artykuł [Tworzenie skryptów U-SQL przy użyciu narzędzi Data Lake Tools dla Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Aby dowiedzieć się więcej o języku U-SQL, zobacz [Wprowadzenie do języka U-SQL w usłudze Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md).
* Informacje o zadaniach zarządzania znajdziesz w artykule [Zarządzanie usługą Azure Data Lake Analytics przy użyciu witryny Azure Portal](data-lake-analytics-manage-use-portal.md).
* Aby zapoznać się z omówieniem usługi Data Lake Analytics, zobacz [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md).



<!--HONumber=Nov16_HO2-->


