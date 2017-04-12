---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu programu PowerShell | Microsoft Docs"
description: "Tworzenie konta usługi Data Lake Store i wykonywanie podstawowych operacji przy użyciu programu Azure PowerShell"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: c00aa4a2e79522a6817a135965f6c218b08e26f8
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu programu Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Zestaw SDK platformy .NET](data-lake-store-get-started-net-sdk.md)
> * [Zestaw SDK Java](data-lake-store-get-started-java-sdk.md)
> * [Interfejs API REST](data-lake-store-get-started-rest-api.md)
> * [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Dowiedz się, jak przy użyciu programu Azure PowerShell utworzyć konto usługi Azure Data Lake Store i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików danych, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake Store, zobacz [Omówienie usługi Data Lake Store](data-lake-store-overview.md).

## <a name="prerequisites"></a>Wymagania wstępne
Przed przystąpieniem do wykonania kroków opisanych w tym samouczku należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Program Azure PowerShell 1.0 lub nowszy**. Zobacz artykuł [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs).

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

    ![Tworzenie grupy zasobów platformy Azure](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Tworzenie grupy zasobów platformy Azure")
3. Utwórz konto usługi Azure Data Lake Store. Wybrana nazwa konta może zawierać tylko małe litery i cyfry.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Tworzenie konta usługi Azure Data Lake Store](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Tworzenie konta usługi Azure Data Lake Store")
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

    ![Weryfikowanie katalogu](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Weryfikowanie katalogu")

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

## <a name="performance-guidance-while-using-powershell"></a>Wskazówki dotyczące wydajności podczas korzystania z programu PowerShell

Poniżej przedstawiono najważniejsze ustawienia, których dostosowanie pozwala uzyskać najlepszą wydajność podczas pracy z usługą Data Lake Store przy użyciu programu PowerShell:

| Właściwość            | Domyślne | Opis |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Ten parametr umożliwia wybór liczby wątków równoległych na potrzeby przekazywania lub pobierania każdego pliku. Ta wartość liczbowa określa maksymalną liczbę wątków, jakie można przydzielić na plik, jednak zależnie od scenariusza może być dostępna mniejsza liczba wątków (np. w przypadku przekazywania pliku o rozmiarze 1 KB uzyskasz jeden wątek, nawet jeśli poprosisz o 20).  |
| ConcurrentFileCount | 10      | Ten parametr jest przeznaczony konkretnie na potrzeby przekazywania lub pobierania folderów. Określa liczbę współbieżnych plików, które można przekazać lub pobrać. Ta wartość liczbowa określa maksymalną liczbę współbieżnych plików, które można jednocześnie przekazać lub pobrać, jednak zależnie od scenariusza współbieżność może być mniejsza (np. w przypadku przekazywania dwóch plików uzyskasz dwa współbieżne przekazania plików, nawet jeśli poprosisz o 15). |

**Przykład**

To polecenie pobiera pliki z usługi Azure Data Lake Store na dysk lokalny użytkownika przy użyciu 20 wątków na plik i 100 plików współbieżnych.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-the-value-to-set-for-these-parameters"></a>Jak określić wartości, jakie należy nadać tym parametrom?

Oto kilka użytecznych wskazówek.

* **Krok 1. Określanie łącznej liczby wątków** — należy rozpocząć od obliczenia łącznej liczby wątków do użycia. Zasadniczo powinno się używać 6 wątków na każdy rdzeń fizyczny.

        Total thread count = total physical cores * 6

    **Przykład**

    Przy założeniu, że polecenia programu PowerShell są uruchamiane z poziomu maszyny wirtualnej D14 o 16 rdzeniach

        Total thread count = 16 cores * 6 = 96 threads


* **Krok 2. Obliczanie wartości parametru PerFileThreadCount** — wartość parametru PerFileThreadCount obliczamy na podstawie rozmiaru plików. W przypadku plików mniejszych niż 2,5 GB nie trzeba zmieniać tego parametru, ponieważ domyślna wartość wynosząca 10 jest wystarczająca. W przypadku plików większych niż 2,5 GB należy użyć 10 wątków jako podstawy dla pierwszych 2,5 GB i dodać 1 wątek na każde dodatkowe 256 MB rozmiaru pliku. Podczas kopiowania folderu zawierającego pliki o szerokim zakresie rozmiarów warto podzielić je na grupy złożone z plików o podobnym rozmiarze. Różne rozmiary plików mogą spowodować utratę optymalnej wydajności. Jeśli pogrupowanie plików o podobnym rozmiarze jest niemożliwe, wartość parametru PerFileThreadCount należy ustawić na podstawie największego rozmiaru pliku.

        PerFileThreadCount = 10 threads for the first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Przykład**

    Przy założeniu, że masz 100 plików o rozmiarach od 1 GB do 10 GB, w równaniu użyjemy 10 GB jako największego rozmiaru plików. Równanie będzie wyglądać następująco.

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **Krok 3. Obliczanie wartości parametru ConcurrentFileCount** — do obliczenia wartości parametru ConcurrentFileCount należy użyć łącznej liczby wątków i wartości parametru PerFileThreadCount z zastosowaniem poniższego równania.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Przykład**

    Na podstawie użytych przez nas przykładowych wartości

        96 = 40 * ConcurrentFileCount

    Wartość parametru **ConcurrentFileCount** wynosi zatem **2,4**, co możemy zaokrąglić do **2**.

### <a name="further-tuning"></a>Dalsze dostosowywanie

Ze względu na zakres rozmiarów plików, z jakimi można pracować, może być konieczne dalsze dostosowanie. Powyższe obliczenie sprawdza się w przypadku, gdy wszystkie pliki lub ich większość ma rozmiar ok. 10 GB. Jeśli natomiast istnieje wiele różnych rozmiarów plików, z czego wiele plików jest mniejszych, można zmniejszyć wartość parametru PerFileThreadCount. Dzięki zmniejszeniu wartości parametru PerFileThreadCount można zwiększyć wartość parametru ConcurrentFileCount. Przy założeniu, że większość naszych plików jest mniejszych (ok. 5 GB), możemy ponownie wykonać nasze obliczenie:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

Teraz wartość parametru **ConcurrentFileCount** wynosi 96/20, czyli 4,8, co można zaokrąglić do **4**.

Te ustawienia można jeszcze bardziej dostosować, zwiększając lub zmniejszając wartość parametru **PerFileThreadCount** zależnie od rozkładu rozmiarów plików.

### <a name="limitation"></a>Ograniczenia

* **Liczba plików jest mniejsza niż wartość parametru ConcurrentFileCount**: jeśli liczba przekazywanych plików jest mniejsza niż obliczona wartość parametru **ConcurrentFileCount**, należy zmniejszyć wartość parametru **ConcurrentFileCount** tak, aby była równa liczbie plików. Przy użyciu pozostałych wątków można zwiększyć wartość parametru **PerFileThreadCount**.

* **Zbyt wiele wątków**: jeśli liczba wątków zostanie nadmiernie zwiększona bez zwiększenia rozmiaru klastra, istnieje ryzyko obniżonej wydajności. Podczas przełączania kontekstu na procesorze mogą wystąpić problemy z rywalizacją o zasoby.

* **Niewystarczająca współbieżność**: jeśli współbieżność nie jest wystarczająca, to klaster może być za mały. Liczbę węzłów w klastrze można zwiększyć, co pozwoli uzyskać większą współbieżność.

* **Błędy ograniczania przepływności**: błędy ograniczania przepływności mogą wystąpić wówczas, gdy współbieżność jest zbyt wysoka. W przypadku błędów ograniczania przepływności należy albo zmniejszyć współbieżność, albo skontaktować się z nami.

## <a name="next-steps"></a>Następne kroki
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
* [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)


