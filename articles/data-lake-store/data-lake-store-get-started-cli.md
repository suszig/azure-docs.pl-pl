---
title: "Rozpoczynanie pracy z usługą Azure Data Lake Store przy użyciu interfejsu wiersza polecenia | Microsoft Docs"
description: "Korzystanie z międzyplatformowego wiersza polecenia platformy Azure w celu utworzenia konta usługi Data Lake Store i wykonywania podstawowych operacji"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 380788f3-041d-4ae5-b6be-37ca74ca333d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/17/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: f7748dba30c6e0332c166feda25f4aaa93c06efa
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Rozpoczynanie pracy z usługą Azure Data Lake Store za pomocą wiersza polecenia platformy Azure
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

Dowiedz się, jak użyć interfejsu wiersza polecenia portalu Azure, aby utworzyć konto usługi Azure Data Lake Store i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake Store, zobacz [Omówienie usługi Data Lake Store](data-lake-store-overview.md).

Interfejs wiersza polecenia platformy Azure został zaimplementowany w środowisku Node.js. Można go używać na dowolnej platformie, która obsługuje środowisko Node.js, w tym w systemie Windows, Mac i Linux. Interfejs wiersza polecenia platformy Azure to rozwiązanie typu open source. Kod źródłowy jest zarządzany w witrynie GitHub pod adresem <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. W tym artykule opisano tylko sposób używania interfejsu wiersza polecenia platformy Azure z usługą Data Lake Store. Ogólne wskazówki na temat używania interfejsu wiersza polecenia platformy Azure można znaleźć w artykule [How to use the Azure CLI (Jak korzystać z interfejsu wiersza polecenia platformy Azure)][azure-command-line-tools].


> [!NOTE]
> Do przekazywania i pobierania dużej ilości danych (dużych i/lub licznych plików) zalecamy skorzystanie z [zestawu SDK dla języka Python](data-lake-store-get-started-python.md), [zestawu SDK programu .NET](data-lake-store-get-started-net-sdk.md) bądź programu [Azure PowerShell](data-lake-store-get-started-powershell.md). Te opcje cechują się lepszą wydajnością, ponieważ umożliwiają równoległe przenoszenie danych dzięki użyciu wielu wątków.
> 
>

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem korzystania z informacji zawartych w tym artykule należy dysponować następującymi elementami:

* **Subskrypcja platformy Azure**. Zobacz temat [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Interfejs wiersza polecenia platformy Azure** — informacje na temat instalacji i konfiguracji można znaleźć w temacie [Instalowanie i konfigurowanie interfejsu wiersza polecenia platformy Azure](../cli-install-nodejs.md). Pamiętaj, aby ponownie uruchomić komputer po zainstalowaniu interfejsu wiersza polecenia.

## <a name="authentication"></a>Authentication

W tym artykule użyto prostszej metody uwierzytelniania w usłudze Data Lake Store, w przypadku której logujesz się jako użytkownik końcowy. Poziom dostępu do konta i systemu plików usługi Data Lake Store jest określany przez poziom dostępu zalogowanego użytkownika. Istnieją jednak inne metody uwierzytelniania w usłudze Data Lake Store: **uwierzytelnianie użytkowników końcowych** i **uwierzytelnianie między usługami**. Instrukcje i dodatkowe informacje na temat uwierzytelniania można znaleźć w artykule [Authenticate with Data Lake Store using Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) (Uwierzytelnianie w usłudze Data Lake Store przy użyciu usługi Azure Active Directory).

## <a name="login-to-your-azure-subscription"></a>Logowanie do subskrypcji platformy Azure

1. Wykonaj kroki opisane w temacie [Connect to an Azure subscription from the Azure Command-Line Interface (Azure CLI)](../xplat-cli-connect.md) (Nawiązywanie połączenia z subskrypcją platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure — Azure CLI) i nawiąż połączenie z subskrypcją za pomocą metody `azure login`.

2. Wyświetl listę subskrypcji skojarzonych z Twoim kontem przy użyciu polecenia `azure account list`.
   
        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false
   
    W powyższych danych wyjściowych subskrypcja **Azure-sub-1** jest włączona, a druga subskrypcja to **Azure-sub-2**. 
3. Wybierz subskrypcję, z którą chcesz pracować. Jeśli chcesz pracować w ramach subskrypcji Azure-sub-2, użyj polecenia `azure account set`.
   
        azure account set Azure-sub-2

## <a name="create-an-azure-data-lake-store-account"></a>Tworzenie konta usługi Azure Data Lake Store
Otwórz wiersz polecenia, powłokę lub sesję terminalu i uruchom następujące polecenia.

1. Włącz tryb usługi Azure Resource Manager za pomocą następującego polecenia:
   
        azure config mode arm
2. Utwórz nową grupę zasobów. W poniższym poleceniu podaj wartości parametrów, których chcesz użyć.
   
        azure group create <resourceGroup> <location>
   
    Jeśli nazwa lokalizacji zawiera spacje, umieść ją w cudzysłowie. Na przykład „Wschodnie stany USA 2”.
3. Utwórz konto usługi Data Lake Store.
   
        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Tworzenie folderów na koncie usługi Data Lake Store
Na koncie usługi Azure Data Lake Store można tworzyć foldery w celu przechowywania danych i zarządzania nimi. Za pomocą następującego polecenia utwórz folder o nazwie „mojnowyfolder” w katalogu głównym usługi Data Lake Store.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Na przykład:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Przekazywanie danych na konto usługi Data Lake Store
Dane można przekazywać na konto usługi Data Lake Store bezpośrednio do katalogu głównego lub do folderu utworzonego w ramach konta. Poniższe fragmenty kodu przedstawiają sposób przekazywania przykładowych danych do folderu (**mojnowyfolder**), który został utworzony w poprzedniej sekcji.

Jeśli szukasz przykładowych danych do przekazania, możesz pobrać folder **Ambulance Data** z [repozytorium Git usługi Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Pobierz plik i zapisz go w katalogu lokalnym na komputerze, na przykład C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Na przykład:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Wyświetlanie listy plików w usłudze Data Lake Store
Użyj poniższego polecenia, aby wyświetlić listę plików na koncie usługi Data Lake Store.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Na przykład:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

Dane wyjściowe będą mieć postać podobną do następującej:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Zmienianie nazwy, pobieranie i usuwanie danych z usługi Data Lake Store
* **Aby zmienić nazwę pliku**, użyj następującego polecenia:
  
        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>
  
    Na przykład:
  
        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv
* **Aby pobrać plik**, użyj następującego polecenia. Upewnij się, że ścieżka docelowa już istnieje.
  
        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>
  
    Na przykład:
  
        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"
* **Aby usunąć plik**, użyj następującego polecenia:
  
        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>
  
    Na przykład:
  
        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv
  
    Po wyświetleniu monitu wpisz **Y**, aby usunąć element.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>Wyświetlanie listy kontroli dostępu do folderu na koncie usługi Data Lake Store
Użyj następującego polecenia, aby wyświetlić listy kontroli dostępu w folderze usługi Data Lake Store. W bieżącej wersji listy kontroli dostępu można ustawić tylko w katalogu głównym usługi Data Lake Store. Tak więc poniższy parametr path będzie zawsze katalogiem głównym (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Na przykład:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Usuwanie konta usługi Data Lake Store
Użyj poniższego polecenia, aby usunąć konto usługi Data Lake Store.

    azure datalake store account delete <dataLakeStoreAccountName>

Na przykład:

    azure datalake store account delete mynewdatalakestore

Po wyświetleniu monitu wpisz **Y**, aby usunąć konto.

## <a name="next-steps"></a>Następne kroki
* [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
* [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../cli-install-nodejs.md

