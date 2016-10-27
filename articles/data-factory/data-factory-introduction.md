<properties 
    pageTitle="Wprowadzenie do usługi Data Factory — usługi integracji danych | Microsoft Azure" 
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
    ms.date="09/22/2016" 
    ms.author="spelluru"/>


# Wprowadzenie do usługi Azure Data Factory, usługa integracji danych w chmurze

## Czym jest usługa Azure Data Factory? 
Data Factory jest usługą integracji danych w chmurze, która służy do aranżacji i automatyzacji **przenoszenia** i **przekształcania** danych. Przy użyciu usługi Data Factory można tworzyć rozwiązania integracji danych, które umożliwiają ich pozyskiwanie z różnych magazynów danych, przekształcanie i przetwarzanie oraz publikowanie wyników w magazynach danych. 

Usługa Data Factory pozwala tworzyć potoki danych służące do przenoszenia i przekształcania danych, a następnie uruchamiać te potoki zgodnie z zaplanowanym harmonogramem (co godzinę, codziennie, co tydzień itp.). Usługa ta udostępnia również rozbudowane wizualizacje umożliwiające wyświetlanie elementów powiązanych i zależności między potokami danych oraz monitorowanie wszystkich potoków danych w jednym zintegrowanym widoku, który ułatwia wykrywanie problemów i konfigurowanie alertów monitorowania.

![Diagram: Omówienie usługi Data Factory, usługi integracji danych](./media/data-factory-introduction/what-is-azure-data-factory.png)
**Rysunek 1.** Pozyskiwanie danych z różnych źródeł danych, przygotowywanie, przekształcanie i analizowanie danych, a następnie publikowanie danych gotowych do użytku.

## Potoki i działania
W ramach rozwiązania fabryki danych jest tworzony co najmniej jeden **potok** danych. Potoki oznaczają logiczne grupy działań. Służą do grupowania działań w jednostki, które umożliwiają wykonanie zadania. 

**Działania** definiują akcje do wykonania na danych. Można na przykład użyć działania kopiowania w celu skopiowania danych z jednego magazynu danych do drugiego. Podobnie można użyć działania programu Hive, które uruchomi zapytanie programu Hive w klastrze usługi Azure HDInsight, aby przekształcić lub przeanalizować dane. Usługa Data Factory obsługuje dwa typy działań — w zakresie przekształcania oraz przenoszenia danych. 
  
## Działania dotyczące przenoszenia danych 
[AZURE.INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Szczegółowe informacje znajdują się w artykule [Data Movement Activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych). 

## Działania dotyczące przekształcania danych
[AZURE.INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Szczegółowe informacje znajdują się w artykule [Data Transformation Activities](data-factory-data-transformation-activities.md) (Działania przekształcania danych).

Jeśli musisz przenieść dane do/z magazynu danych nieobsługiwanego przez działanie kopiowania lub przekształcić dane za pomocą własnej logiki, utwórz **niestandardowe działanie platformy .NET**. Aby uzyskać szczegółowe informacje na temat tworzenia i używania niestandardowego działania, zobacz artykuł [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) (Korzystanie z niestandardowych działań w potoku usługi Azure Data Factory).

## Połączone usługi
Połączone usługi definiują informacje wymagane przez usługę Data Factory do łączenia z zasobami zewnętrznymi. (Przykłady: usługa Azure Storage, lokalny serwer SQL Server, usługa Azure HDInsight). Połączone usługi w usłudze Fabryka danych służą do dwóch celów:

- Reprezentowanie **magazynu danych**, w tym między innymi lokalnego serwera SQL Server, bazy danych Oracle, udziału plików lub konta usługi Azure Blob Storage. Listę obsługiwanych magazynów danych można znaleźć w sekcji [Data movement activities](data-factory-data-movement-activities.md) (Działania przenoszenia danych). 
- Reprezentowanie **zasobu obliczeniowego**, który może hostować wykonywanie działania. Na przykład działanie HDInsightHive jest wykonywane w klastrze HDInsight na platformie Hadoop. Listę obsługiwanych środowisk obliczeniowych można znaleźć w sekcji [Data transformation activities](data-factory-data-transformation-activities.md) (Działania przekształcania danych). 

## Zestawy danych 
Połączone usługi łączą magazyny danych z fabryką danych Azure. Zestawy danych reprezentują struktury danych w magazynach danych. Na przykład połączona usługa Azure Storage udostępnia informacje usłudze Data Factory, które umożliwiają połączenie z kontem usługi Azure Storage. Zestaw danych usługi Azure Blob określa kontener obiektów blob i folder w usłudze Azure Blob Storage, z którego potok powinien odczytywać dane. Podobnie połączona usługa SQL Azure udostępnia informacje o połączeniu dla bazy danych SQL Azure, a zestaw danych SQL Azure określa tabelę zawierającą dane.   

## Relacje między obiektami usługi Data Factory
Usługa Data Factory obejmuje kilka kluczowych jednostek, które współpracują w celu definiowania danych wejściowych i wyjściowych, zdarzeń przetwarzania oraz harmonogramu i zasobów potrzebnych do wykonywania żądanego przepływu danych.

![Diagram: Data Factory, usługa integracji danych w chmurze — najważniejsze pojęcia](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Rysunek 2.** Relacje między elementami Zestaw danych, Działanie, Potok i Połączona usługa

Te cztery proste pojęcia — połączone usługi, zestawy danych, działania i potoki — umożliwiają rozpoczęcie pracy. Możesz [utworzyć swój pierwszy potok](data-factory-build-your-first-pipeline.md). 

## Obsługiwane regiony
Obecnie można tworzyć fabryki danych w regionach **Zachodnie stany USA**, **Wschodnie stany USA** oraz **Europa Północna**. Jednak fabryka danych może mieć dostęp do magazynów danych i usług obliczeniowych w innych regionach świadczenia usługi Azure, aby przenosić dane między magazynami danych lub przetwarzać dane przy użyciu usług obliczeniowych. 

Sama usługa Fabryka danych Azure nie przechowuje żadnych danych. Usługa umożliwia tworzenie przepływów opartych na danych do aranżowania przenoszenia danych między [obsługiwanymi magazynami danych](data-factory-data-movement-activities.md#supported-data-stores) oraz przetwarzania danych przy użyciu [usług obliczeniowych](data-factory-compute-linked-services.md) w innych regionach lub w środowisku lokalnym. Umożliwia także [monitorowanie przepływów pracy i zarządzanie nimi](data-factory-monitor-manage-pipelines.md) przy użyciu zarówno mechanizmów programowych, jaki i interfejsu użytkownika. 

Chociaż usługa Azure Data Factory jest dostępna tylko w regionach **Zachodnie stany USA**, **Wschodnie stany USA** i **Europa Północna**, to usługa zapewniająca przenoszenie danych w usłudze Data Factory jest dostępna [globalnie](data-factory-data-movement-activities.md#global) w kilku regionach. W przypadku gdy magazyn danych znajduje się za zaporą, dane są przenoszone przez [bramę zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) zainstalowaną w środowisku lokalnym. 

Załóżmy na przykład, że środowiska obliczeniowe, takie jak klaster usługi Azure HDInsight i usługa Azure Machine Learning, są uruchamiane z regionu Europa Zachodnia. Można utworzyć i wykorzystać wystąpienie usługi Azure Data Factory w regionie Europa Północna oraz użyć go do planowania zadań wykonywanych w środowiskach obliczeniowych w regionie Europa Zachodnia. Wyzwolenie zadania w środowisku obliczeniowym zajmuje usłudze Data Factory kilka milisekund, ale czas uruchomienia zadania w środowisku obliczeniowym nie zmienia się.

W przyszłości zamierzamy udostępnić usługę Fabryka danych Azure w każdym regionie geograficznym obsługiwanym przez platformę Azure.
  
## Następne kroki
Wykonaj instrukcje krok po kroku w następujących samouczkach, aby dowiedzieć się, jak tworzyć fabryki danych z potokami danych. 

Samouczek | Opis
-------- | -----------
[Tworzenie potoku danych, który przetwarza dane przy użyciu klastra usługi Hadoop](data-factory-build-your-first-pipeline.md) | W tym samouczku utworzysz pierwszą fabrykę danych platformy Azure przy użyciu potoku danych, który **przetwarza dane**, uruchamiając skrypt Hive w klastrze Azure HDInsight (Hadoop). |
[Tworzenie potoku danych do przenoszenia danych między dwoma magazynami danych w chmurze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) | W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z magazynu obiektów blob do bazy danych SQL.
[Tworzenie potoku danych do przenoszenia danych między lokalnym magazynem danych i magazynem danych w chmurze przy użyciu bramy zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) | W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z **lokalnej** bazy danych programu SQL Server do obiektu blob platformy Azure. W ramach tego przewodnika zainstalujesz i skonfigurujesz bramę zarządzania danymi na tym komputerze. 



<!--HONumber=Oct16_HO3-->


