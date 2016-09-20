<properties
   pageTitle="Rozpoczynanie pracy z usługą Data Lake Store | Azure"
   description="Tworzenie konta usługi Data Lake Store i wykonywanie podstawowych operacji przy użyciu programu Azure PowerShell"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="07/07/2016"
   ms.author="nitinme"/>

# Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu programu Azure PowerShell

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [Zestaw SDK .NET](data-lake-store-get-started-net-sdk.md)
- [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
- [Interfejs API REST](data-lake-store-get-started-rest-api.md)
- [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak przy użyciu programu Azure PowerShell utworzyć konto usługi Azure Data Lake Store i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake Store, zobacz [Omówienie usługi Data Lake Store](data-lake-store-overview.md).

## Wymagania wstępne

Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

- **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Włączenie subskrypcji platformy Azure** w celu uzyskania publicznej wersji zapoznawczej usługi Data Lake Store. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup).


##Instalowanie programu Azure PowerShell 1.0 lub nowszego

Zobacz sekcję wymagań wstępnych w temacie [Używanie programu Azure PowerShell z usługą Azure Resource Manager](../powershell-azure-resource-manager.md#prerequisites).

## Tworzenie konta usługi Azure Data Lake Store

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

2. Utwórz konto usługi Azure Data Lake Store. Wybrana nazwa konta może zawierać tylko małe litery i cyfry.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Tworzenie konta usługi Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Create an Azure Data Lake Store account")

3. Sprawdź, czy konto zostało utworzone pomyślnie.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    Dane wyjściowe powinny mieć wartość **True**.

## Tworzenie struktur katalogów w usłudze Azure Data Lake Store

W ramach konta usługi Azure Data Lake Store można tworzyć katalogi w celu przechowywania danych i zarządzania nimi.

1. Określ katalog główny.

        $myrootdir = "/"

2. Utwórz nowy katalog o nazwie **mynewdirectory** w określonym katalogu głównym.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory

3. Sprawdź, czy nowy katalog został utworzony pomyślnie.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    Dane wyjściowe powinny mieć postać podobną do następujących:

    ![Sprawdzanie katalogu](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Verify Directory")


## Przekazywanie danych do usługi Azure Data Lake Store

Dane można przekazywać do usługi Data Lake Store bezpośrednio do katalogu głównego lub do katalogu utworzonego w ramach konta. Poniższe fragmenty kodu przedstawiają sposób przekazywania przykładowych danych do katalogu (**mynewdirectory**), który został utworzony w poprzedniej sekcji.

Jeśli szukasz przykładowych danych do przekazania, możesz pobrać folder **Ambulance Data** z [repozytorium Git usługi Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Pobierz plik i zapisz go w katalogu lokalnym na komputerze, na przykład C:\sampledata.\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## Zmienianie nazwy, pobieranie i usuwanie danych z usługi Data Lake Store

Aby zmienić nazwę pliku, użyj następującego polecenia:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Aby pobrać plik, użyj następującego polecenia.

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Aby usunąć plik, użyj następującego polecenia:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Po wyświetleniu monitu wpisz **Y**, aby usunąć element. Jeśli masz więcej niż jeden plik do usunięcia, możesz podać wszystkie ścieżki oddzielone przecinkami.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## Usuwanie konta usługi Azure Data Lake Store

Użyj poniższego polecenia, aby usunąć konto usługi Data Lake Store.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Po wyświetleniu monitu wpisz **Y**, aby usunąć konto.


## Następne kroki

- [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
- [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)



<!--HONumber=sep16_HO1-->


