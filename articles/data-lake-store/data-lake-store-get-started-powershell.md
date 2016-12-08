---
title: "Wprowadzenie do usługi Data Lake Store | Microsoft Docs"
description: "Tworzenie konta usługi Data Lake Store i wykonywanie podstawowych operacji przy użyciu programu Azure PowerShell"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/21/2016
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: c157da7bf53e2d0762624e8e71e56e956db04a24
ms.openlocfilehash: 7663670bc4fe0612b9a22545f0fc036add1c48cd


---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu programu Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Dowiedz się, jak przy użyciu programu Azure PowerShell utworzyć konto usługi Azure Data Lake Store i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake Store, zobacz [Omówienie usługi Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Program Azure PowerShell 1.0 lub nowszy**. Zobacz artykuł [Instalowanie i konfigurowanie programu Azure PowerShell](../powershell-install-configure.md).

## <a name="authentication"></a>Authentication
W tym artykule użyto prostszej metody uwierzytelniania w usłudze Data Lake Store, w przypadku której jest wyświetlany monit o wprowadzenie poświadczeń konta platformy Azure. Poziom dostępu do konta i systemu plików usługi Data Lake Store jest określany przez poziom dostępu zalogowanego użytkownika. Istnieją jednak inne metody uwierzytelniania w usłudze Data Lake Store: **uwierzytelnianie użytkowników końcowych** i **uwierzytelnianie między usługami**. Instrukcje i dodatkowe informacje na temat uwierzytelniania można znaleźć w artykule [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) (Uwierzytelnianie w usłudze Data Lake Store przy użyciu usługi Azure Active Directory).

## <a name="create-an-azure-data-lake-store-account"></a>Tworzenie konta usługi Azure Data Lake Store
1. Na pulpicie otwórz nowe okno programu Windows PowerShell, a następnie wprowadź poniższy fragment kodu, aby zalogować się do konta platformy Azure, skonfigurować subskrypcję i zarejestrować dostawcę usługi Data Lake Store. Po wyświetleniu monitu zaloguj się jako jeden z administratorów/właścicieli subskrypcji:
   
        # Log in to your Azure account
        Login-AzureRmAccount
   
        # List all the subscriptions associated to your account
        Get-AzureRmSubscription
   
        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>
   
        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Konto usługi Azure Data Lake Store jest skojarzone z grupą zasobów platformy Azure. Rozpocznij od utworzenia grupy zasobów platformy Azure.
   
        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"
   
    ![Tworzenie grupy zasobów platformy Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Create an Azure Resource Group")
3. Utwórz konto usługi Azure Data Lake Store. Wybrana nazwa konta może zawierać tylko małe litery i cyfry.
   
        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"
   
    ![Tworzenie konta usługi Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")
4. Sprawdź, czy konto zostało utworzone pomyślnie.
   
        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName
   
    Dane wyjściowe powinny mieć wartość **True**.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Tworzenie struktur katalogów w usłudze Azure Data Lake Store
W ramach konta usługi Azure Data Lake Store można tworzyć katalogi w celu przechowywania danych i zarządzania nimi.

1. Określ katalog główny.
   
        $myrootdir = "/"
2. Utwórz nowy katalog o nazwie **mynewdirectory** w określonym katalogu głównym.
   
        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Sprawdź, czy nowy katalog został utworzony pomyślnie.
   
        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir
   
    Dane wyjściowe powinny mieć postać podobną do następujących:
   
    ![Sprawdzanie katalogu](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Przekazywanie danych do usługi Azure Data Lake Store
Dane można przekazywać do usługi Data Lake Store bezpośrednio do katalogu głównego lub do katalogu utworzonego w ramach konta. Poniższe fragmenty kodu przedstawiają sposób przekazywania przykładowych danych do katalogu (**mynewdirectory**), który został utworzony w poprzedniej sekcji.

Jeśli szukasz przykładowych danych do przekazania, możesz pobrać folder **Ambulance Data** z [repozytorium Git usługi Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Pobierz plik i zapisz go w katalogu lokalnym na komputerze, na przykład C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Zmienianie nazwy, pobieranie i usuwanie danych z usługi Data Lake Store
Aby zmienić nazwę pliku, użyj następującego polecenia:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Aby pobrać plik, użyj następującego polecenia.

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Aby usunąć plik, użyj następującego polecenia:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Po wyświetleniu monitu wpisz **Y**, aby usunąć element. Jeśli masz więcej niż jeden plik do usunięcia, możesz podać wszystkie ścieżki oddzielone przecinkami.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Usuwanie konta usługi Azure Data Lake Store
Użyj poniższego polecenia, aby usunąć konto usługi Data Lake Store.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Po wyświetleniu monitu wpisz **Y**, aby usunąć konto.

## <a name="next-steps"></a>Następne kroki
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
* [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)




<!--HONumber=Nov16_HO4-->


