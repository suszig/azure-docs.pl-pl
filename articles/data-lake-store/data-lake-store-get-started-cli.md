<properties
   pageTitle="Rozpoczynanie pracy z usługą Data Lake Store z użyciem międzyplatformowego interfejsu wiersza polecenia | Microsoft Azure"
   description="Korzystanie z międzyplatformowego wiersza polecenia platformy Azure w celu utworzenia konta usługi Data Lake Store i wykonywania podstawowych operacji"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="paulettm"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="04/07/2016"
   ms.author="nitinme"/>

# Rozpoczynanie pracy z usługą Azure Data Lake Store za pomocą wiersza polecenia platformy Azure

> [AZURE.SELECTOR]
- [Portal](data-lake-store-get-started-portal.md)
- [PowerShell](data-lake-store-get-started-powershell.md)
- [Zestaw .NET SDK](data-lake-store-get-started-net-sdk.md)
- [Zestaw Java SDK](data-lake-store-get-started-java-sdk.md)
- [Interfejs API REST](data-lake-store-get-started-rest-api.md)
- [Interfejs wiersza polecenia platformy Azure](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Dowiedz się, jak użyć interfejsu wiersza polecenia portalu Azure, aby utworzyć konto usługi Azure Data Lake Store i wykonywać podstawowe operacje, takie jak tworzenie folderów, przekazywanie i pobieranie plików, usuwanie konta itp. Aby uzyskać więcej informacji o usłudze Data Lake Store, zobacz [Omówienie usługi Data Lake Store](data-lake-store-overview.md).

Interfejs wiersza polecenia platformy Azure został zaimplementowany w środowisku Node.js. Można go używać na dowolnej platformie, która obsługuje środowisko Node.js, w tym w systemie Windows, Mac i Linux. Interfejs wiersza polecenia platformy Azure to rozwiązanie typu open source. Kod źródłowy jest zarządzany w witrynie GitHub pod adresem <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. W tym artykule opisano tylko sposób używania interfejsu wiersza polecenia platformy Azure z usługą Data Lake Store. Ogólne wskazówki na temat używania interfejsu wiersza polecenia platformy Azure można znaleźć w artykule [Jak korzystać z interfejsu wiersza polecenia platformy Azure] [azure-command-line-tools].


##Wymagania wstępne

Przed rozpoczęciem korzystania z informacji zawartych w tym artykule należy dysponować następującymi elementami:

- **Subskrypcja platformy Azure**. Zobacz artykuł [Uzyskiwanie bezpłatnej wersji próbnej platformy Azure](https://azure.microsoft.com/pricing/free-trial/).
- **Włączenie subskrypcji platformy Azure** w celu uzyskania publicznej wersji zapoznawczej usługi Data Lake Store. Zobacz [instrukcje](data-lake-store-get-started-portal.md#signup).
- **Interfejs wiersza polecenia platformy Azure** — informacje na temat instalacji i konfiguracji można znaleźć w temacie [Instalowanie i konfigurowanie interfejsu wiersza polecenia platformy Azure](../xplat-cli-install.md). Pamiętaj, aby ponownie uruchomić komputer po zainstalowaniu interfejsu wiersza polecenia.

##Logowanie do subskrypcji platformy Azure

Wykonaj kroki opisane w temacie [Nawiązywanie połączenia z subskrypcją platformy Azure za pomocą interfejsu wiersza polecenia platformy Azure (Azure CLI)](../xplat-cli-connect.md) i nawiąż połączenie z subskrypcją za pomocą metody __logowania__.


## Tworzenie konta usługi Azure Data Lake Store

Otwórz wiersz polecenia, powłokę lub sesję terminalu i uruchom następujące polecenia.

1. Zaloguj się do subskrypcji platformy Azure:

        azure login

    Zostanie wyświetlony monit o otwarcie strony sieci Web i wprowadzenie kodu uwierzytelniania. Postępuj zgodnie z instrukcjami na stronie, aby zalogować się do subskrypcji platformy Azure.

2. Włącz tryb usługi Azure Resource Manager za pomocą następującego polecenia:

        azure config mode arm


3. Lista subskrypcji platformy Azure dla Twojego konta.

        azure account list


4. Jeśli masz wiele subskrypcji Azure, użyj następujących poleceń, aby ustawić subskrypcję, której będą używać polecenia interfejsu wiersza polecenia platformy Azure:

        azure account set <subscriptionname>

5. Utwórz nową grupę zasobów. W poniższym poleceniu podaj wartości parametrów, których chcesz użyć.

        azure group create <resourceGroup> <location>

    Jeśli nazwa lokalizacji zawiera spacje, umieść ją w cudzysłowie. Na przykład „Wschodnie stany USA 2”.

5. Utwórz konto usługi Data Lake Store.

        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## Tworzenie folderów na koncie usługi Data Lake Store

Na koncie usługi Azure Data Lake Store można tworzyć foldery w celu przechowywania danych i zarządzania nimi. Za pomocą następującego polecenia utwórz folder o nazwie „mojnowyfolder” w katalogu głównym usługi Data Lake Store.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Na przykład:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## Przekazywanie danych na konto usługi Data Lake Store

Dane można przekazywać na konto usługi Data Lake Store bezpośrednio do katalogu głównego lub do folderu utworzonego w ramach konta. Poniższe fragmenty kodu przedstawiają sposób przekazywania przykładowych danych do folderu (**mojnowyfolder**), który został utworzony w poprzedniej sekcji.

Jeśli szukasz przykładowych danych do przekazania, możesz pobrać folder **Ambulance Data** z [repozytorium Git usługi Azure Data Lake](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData). Pobierz plik i zapisz go w katalogu lokalnym na komputerze, na przykład C:\sampledata.\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Na przykład:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## Wyświetlanie listy plików w usłudze Data Lake Store

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

## Zmienianie nazwy, pobieranie i usuwanie danych z usługi Data Lake Store

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

## Wyświetlanie listy kontroli dostępu do folderu na koncie usługi Data Lake Store

Użyj następującego polecenia, aby wyświetlić listy kontroli dostępu w folderze usługi Data Lake Store. W bieżącej wersji listy kontroli dostępu można ustawić tylko w katalogu głównym usługi Data Lake Store. Tak więc poniższy parametr path będzie zawsze katalogiem głównym (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Na przykład:

    azure datalake store permissions show mynewdatalakestore /


## Usuwanie konta usługi Data Lake Store

Użyj poniższego polecenia, aby usunąć konto usługi Data Lake Store.

    azure datalake store account delete <dataLakeStoreAccountName>

Na przykład:

    azure datalake store account delete mynewdatalakestore

Po wyświetleniu monitu wpisz **Y**, aby usunąć konto.


## Następne kroki

- [Zabezpieczanie danych w usłudze Data Lake Store](data-lake-store-secure-data.md)
- [Korzystanie z usługi Azure Data Lake Analytics z usługą Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Korzystanie z usługi Azure HDInsight z usługą Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)


[azure-command-line-tools]: ../xplat-cli-install.md



<!--HONumber=Jun16_HO2-->


