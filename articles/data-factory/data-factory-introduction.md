<properties 
    pageTitle="Czym jest usługa Data Factory? Usługa integracji danych | Microsoft Azure" 
    description="Dowiedz się, czym jest usługa Azure Data Factory: usługa integracji danych w chmurze, która służy do aranżacji i automatyzacji przenoszenia i przekształcania danych." 
    keywords="integracja danych, integracja danych w chmurze, czym jest usługa azure data factory"
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="07/12/2016" 
    ms.author="spelluru"/>

# Wprowadzenie do usługi Azure Data Factory, usługa integracji danych w chmurze

## Czym jest usługa Azure Data Factory? 
Fabryka danych jest usługą integracji danych w chmurze, która służy do aranżacji i automatyzacji przenoszenia i przekształcania danych. Podobnie jak w przypadku prawdziwej fabryki wyposażonej w sprzęt, który pobiera surowce i przekształca je w gotowe towary, usługa Fabryka danych aranżuje istniejące usługi, w ramach których zbierane są oryginalne dane, które są następnie przekształcane w gotowe do użycia informacje. 

Usługa Fabryka danych korzysta z lokalnych i dostępnych w chmurze źródeł danych oraz z modelu SaaS w celu pozyskiwania, przygotowywania, przekształcania, analizowania i publikowania danych.  Usługa Fabryka danych służy do łączenia usług w potoki zarządzanego przepływu danych w celu przekształcania danych przy użyciu takich usług, jak [HDInsight Azure (Hadoop)](http://azure.microsoft.com/documentation/services/hdinsight/) i [Azure Batch](https://azure.microsoft.com/documentation/services/batch/) na potrzeby wykonywania obliczeń na danych big data, oraz przy użyciu usługi [Azure Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/) do zarządzania rozwiązaniami analitycznymi.  Usługa Fabryka danych wykracza poza jedynie tabelaryczny widok monitorowania, umożliwiając korzystanie z rozbudowanych wizualizacji, aby szybko wyświetlać elementy powiązane oraz zależności między potokami danych. Pozwala na monitorowanie wszystkich potoków przepływu danych z jednego zintegrowanego widoku, który ułatwia wykrywanie problemów i konfigurowanie alertów monitorowania.

![Diagram: Omówienie usługi Data Factory, usługi integracji danych](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Rysunek 1.** Zbieranie danych z wielu różnych lokalnych źródeł danych, pozyskiwanie ich i przygotowywanie, organizowanie i analizowanie przy użyciu szerokiej oferty przekształceń, a następnie publikowanie danych gotowych do użytku.

Usługi Fabryka danych można używać w dowolnym czasie do zbierania danych w różnej postaci i o różnej wielkości, przekształcania ich i publikowania w celu uzyskania szczegółowej analizy — wszystko w oparciu o niezawodny harmonogram. Usługa Fabryka danych umożliwia tworzenie potoków przepływu danych o wysokiej dostępności działających w wielu scenariuszach i zaspokajających potrzeby analityczne różnych branż.  Sprzedawcy internetowi używają jej do generowania spersonalizowanych [rekomendacji produktów](data-factory-product-reco-usecase.md) w oparciu o zachowanie klientów podczas przeglądania. Producenci gier komputerowych używają jej, aby uzyskać informacje na temat [skuteczności kampanii marketingowych](data-factory-customer-profiling-usecase.md). Zapoznaj się z artykułem [Customer Case Studies](data-factory-customer-case-studies.md) (Analizy przypadków klientów) i dowiedz się bezpośrednio od naszych klientów, jak i dlaczego korzystają z usługi Fabryka danych. 

> [AZURE.VIDEO azure-data-factory-overview]

## Kluczowe pojęcia

Usługa Fabryka danych Azure obejmuje kilka kluczowych jednostek, które współpracują w celu definiowania danych wejściowych i wyjściowych, zdarzeń przetwarzania oraz harmonogramu i zasobów potrzebnych do wykonywania żądanego przepływu danych.

![Diagram: Data Factory, usługa integracji danych w chmurze — najważniejsze pojęcia](./media/data-factory-introduction/data-integration-service-key-concepts.png)

**Rysunek 2.** Relacje między elementami Zestaw danych, Działanie, Potok i Połączona usługa


### Działania
Działania definiują akcje do wykonania na danych. Każde działanie pobiera 0 lub więcej [zestawów danych](data-factory-create-datasets.md) w formie danych wejściowych i generuje co najmniej jeden zestaw danych w formie danych wyjściowych. Działanie jest jednostką aranżacji w usłudze Fabryka danych Azure. Można na przykład użyć [działania kopiowania](data-factory-data-movement-activities.md) do aranżacji kopiowania danych z jednego zestawu danych do drugiego. Podobnie można użyć [działania programu Hive](data-factory-data-transformation-activities.md), które uruchomi zapytanie programu Hive w klastrze usługi Azure HDInsight, aby przekształcić lub przeanalizować dane. Usługa Fabryka danych Azure udostępnia szeroki zakres działań w zakresie przekształcania, analizy oraz przenoszenia danych. 

### Potoki
[Potoki](data-factory-create-pipelines.md) oznaczają logiczne grupy działań. Służą do grupowania działań w jednostki, które umożliwiają wykonanie zadania. Na przykład sekwencja kilku działań przekształcania może być potrzebna do czyszczenia danych w pliku dziennika.  Taka sekwencja może obejmować złożony harmonogram i zależności, które wymagają aranżacji i automatyzacji. Wszystkie te działania można pogrupować w jeden potok o nazwie „CleanLogFiles”.  Potok „CleanLogFiles” można następnie wdrożyć, zaplanować lub usunąć jako jedną jednostkę zamiast zarządzania poszczególnymi działaniami niezależnie.

### Zestawy danych
[Zestawy danych](data-factory-create-datasets.md) to nazwane odwołania/wskaźniki do danych, których chcesz użyć jako danych wejściowych lub wyjściowych działania. Zestawy danych identyfikują struktury danych w różnych magazynach danych, z uwzględnieniem tabel, plików, folderów i dokumentów.

### Połączona usługa
Połączone usługi definiują informacje potrzebne w usłudze Fabryka danych do łączenia z zasobami zewnętrznymi.  Połączone usługi w usłudze Fabryka danych służą do dwóch celów:

- Do reprezentowania magazynu danych, w tym między innymi, lokalnej bazy danych SQL Server, bazy danych Oracle, udziału plików lub konta usługi Azure Blob Storage.   Jak wspomniano powyżej, zestawy danych reprezentują struktury w ramach magazynów danych połączonych z usługą Fabryka danych za pośrednictwem połączonej usługi.
- Do reprezentowania zasobu obliczeniowego, który może obsługiwać wykonywanie działania.  Na przykład „Działanie HDInsightHive” jest wykonywane w klastrze HDInsight na platformie Hadoop.

Te cztery proste pojęcia — zestawy danych, działania, potoki i połączone usługi — umożliwiają rozpoczęcie pracy!  Możesz [utworzyć pierwszy potok](data-factory-build-your-first-pipeline.md) od podstaw lub wdrożyć gotowy przykład, postępując zgodnie z instrukcjami w artykule [Data Factory Samples](data-factory-samples.md) (Przykłady usługi Fabryka danych). 

## Obsługiwane regiony
Obecnie można tworzyć fabryki danych w regionach **Zachodnie stany USA**, **Wschodnie stany USA** oraz **Europa Północna**. Jednak fabryka danych może mieć dostęp do magazynów danych i usług obliczeniowych w innych regionach świadczenia usługi Azure, aby przenosić dane między magazynami danych lub przetwarzać dane przy użyciu usług obliczeniowych. 

Sama usługa Fabryka danych Azure nie przechowuje żadnych danych. Usługa umożliwia tworzenie przepływów opartych na danych do aranżowania przenoszenia danych między [obsługiwanymi magazynami danych](data-factory-data-movement-activities.md#supported-data-stores) oraz przetwarzania danych przy użyciu [usług obliczeniowych](data-factory-compute-linked-services.md) w innych regionach lub w środowisku lokalnym. Umożliwia także [monitorowanie przepływów pracy i zarządzanie nimi](data-factory-monitor-manage-pipelines.md) przy użyciu zarówno mechanizmów programowych, jaki i interfejsu użytkownika. 

Należy pamiętać, że chociaż usługa Fabryka danych Azure jest dostępna tylko w regionach **Zachodnie stany USA**, **Wschodnie stany USA** i w **Europa Północna**, jednak usługa zapewniająca przenoszenie danych w usłudze Fabryka danych jest dostępna [globalnie](data-factory-data-movement-activities.md#global) w kilku regionach. W przypadku gdy magazyn danych znajduje się za zaporą, dane są przenoszone przez [bramę zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) zainstalowaną w środowisku lokalnym. 

Załóżmy na przykład, że środowiska obliczeniowe, takie jak klaster usługi Azure HDInsight i usługa Azure Machine Learning są uruchamiane z regionu Europy Zachodniej. Można utworzyć i wykorzystać wystąpienie usługi Fabryka danych Azure w Europie Północnej oraz użyć go do planowania zadań wykonywanych w środowiskach obliczeniowych w Europie Zachodniej. Wyzwolenie zadania w środowisku obliczeniowym zajmuje usłudze Fabryka danych kilka milisekund, ale czas wykonywania zadania w środowisku obliczeniowym nie zmienia się.

W przyszłości zamierzamy udostępnić usługę Fabryka danych Azure w każdym regionie geograficznym obsługiwanym przez platformę Azure.
  
## Następne kroki
Wykonaj instrukcje krok po kroku w następujących samouczkach, aby dowiedzieć się, jak tworzyć fabryki danych z potokami danych. 

Samouczek | Opis
-------- | -----------
[Tworzenie potoku danych, który przetwarza dane przy użyciu klastra usługi Hadoop](data-factory-build-your-first-pipeline.md) | W tym samouczku utworzysz pierwszą fabrykę danych Azure przy użyciu potoku danych, który **przetwarza dane**, uruchamiając skrypt Hive w klastrze Azure HDInsight (Hadoop). |
[Tworzenie potoku danych do przenoszenia danych między dwoma magazynami danych w chmurze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z magazynu obiektów blob do bazy danych SQL.
[Tworzenie potoku danych do przenoszenia danych między lokalnym magazynem danych i magazynem danych w chmurze przy użyciu bramy zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) |  W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z **lokalnej** bazy danych programu SQL Server do obiektu blob Azure. W ramach tego przewodnika zainstalujesz i skonfigurujesz bramę zarządzania danymi na tym komputerze. 


<!--HONumber=sep16_HO1-->


