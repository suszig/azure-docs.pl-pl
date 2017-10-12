---
title: "Wprowadzenie do usługi Data Factory — usługi integracji danych | Microsoft Docs"
description: "Dowiedz się, czym jest usługa Azure Data Factory: usługa integracji danych w chmurze, która służy do aranżacji i automatyzacji przenoszenia i przekształcania danych."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: cec68cb5-ca0d-473b-8ae8-35de949a009e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/14/2017
ms.author: shlo
robots: noindex
ms.openlocfilehash: 8d4d574dee4b993d8a464482b244e1f63ade8a57
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-azure-data-factory"></a>Wprowadzenie do usługi Azure Data Factory 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](data-factory-introduction.md)
> * [Wersja 2 — wersja zapoznawcza](../introduction.md)

> [!NOTE]
> Ten artykuł dotyczy wersji 1 usługi Data Factory, która jest ogólnie dostępna (GA). Jeśli używasz wersji 2 usługi Data Factory, która jest w wersji zapoznawczej, zobacz [Introduction to Data Factory V2 (Wprowadzenie do usługi Data Factory w wersji 2)](../introduction.md).


## <a name="what-is-azure-data-factory"></a>Czym jest usługa Azure Data Factory?
W jaki sposób istniejące dane są wykorzystywane w biznesowej rzeczywistości opartej na danych big data? Czy można wzbogacić informacje wygenerowane w chmurze danymi referencyjnymi, pochodzącymi z lokalnych — lub innych, różnorodnych — źródeł danych? Na przykład firma tworząca gry zbiera wiele dzienników generowanych przez gry w chmurze. Chce analizować te dzienniki w celu uzyskania wglądu w preferencje klientów, dane demograficzne, sposób użytkowania itp., aby zidentyfikować szanse sprzedaży dodatkowej i wiązanej, opracować nowe ciekawe funkcje pomagające w rozwoju firmy i zapewnić lepsze doświadczenia klientom. 

Do analizy tych dzienników firma musi użyć danych referencyjnych, np. informacji o kliencie, grze i kampanii marketingowej, które znajdują się w lokalnym magazynie danych. W związku z tym firma chce pozyskać dane dzienników z magazynu danych w chmurze oraz dane referencyjne z lokalnego magazynu danych. Następnie dane mają zostać przetworzone za pomocą usługi Hadoop w chmurze (Azure HDInsight), a dane wynikowe mają zostać opublikowane w magazynie danych w chmurze, takim jak Azure SQL Data Warehouse, lub lokalnym magazynie danych, np. SQL Server. Ten przepływ pracy ma być uruchamiany raz na tydzień. 

Potrzebna jest platforma umożliwiająca firmie utworzenie przepływu pracy, który pozyska dane zarówno z lokalnego magazynu danych, jak i magazynu danych w chmurze, przekształci lub przetworzy dane przy użyciu istniejących usług obliczeniowych, np. usługi Hadoop, i opublikuje wyniki w lokalnym magazynie danych lub magazynie danych w chmurze do użycia przez aplikacje analizy biznesowej. 

![Omówienie usługi Data Factory](media/data-factory-introduction/what-is-azure-data-factory.png) 

Azure Data Factory to platforma umożliwiająca realizację takich scenariuszy. Jest to **oparta na chmurze usługa integracji danych, za pomocą której można tworzyć oparte na danych przepływy pracy w chmurze służące do organizowania i automatyzowania przenoszenia i przekształcania danych**. Za pomocą usługi Azure Data Factory można tworzyć oparte na danych przepływy pracy (nazywane potokami) i ustalać ich harmonogram. Te przepływy mogą pozyskiwać dane z różnych magazynów danych, przetwarzać/przekształcać je za pomocą usług obliczeniowych, takich jak Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics i Azure Machine Learning, a następnie publikować dane wyjściowe w magazynach danych, np. Azure SQL Data Warehouse, do użycia przez aplikacje analizy biznesowej.  

To bardziej platforma, która najpierw wyodrębnia i ładuje, a następnie przekształca i ładuje, niż tradycyjna platforma wyodrębniania, przekształcania i ładowania. Wykonywane przekształcenia mają na celu przekształcenie/przetworzenie danych za pomocą usług obliczeniowych, a nie dodanie kolumn pochodnych, zliczenie liczby wierszy, posortowanie danych itp. 

Obecnie dane używane w usłudze Azure Data Factory i generowane przez nią to dane z **określonego przedziału czasu** (godzina, dzień, tydzień itd.). Na przykład potok może odczytać dane wejściowe, przetworzyć je i wygenerować dane wyjściowe raz dziennie. Przepływ pracy można także uruchomić tylko jeden raz.  
  

## <a name="how-does-it-work"></a>Jak to działa? 
Potoki (oparte na danych przepływy pracy) w usłudze Azure Data Factory zwykle wykonują następujące trzy kroki:

![Trzy etapy działania usługi Azure Data Factory](media/data-factory-introduction/three-information-production-stages.png)

### <a name="connect-and-collect"></a>Łączenie i zbieranie
Dane używane w przedsiębiorstwach mają różne typy i znajdują się w rozproszonych źródłach. Pierwszy etap tworzenia systemu uzyskiwania informacji polega na połączeniu wszystkich wymaganych źródeł danych i systemów przetwarzania, takich jak usługi SaaS, udziały plików, serwery FTP oraz usługi sieci Web, i przeniesieniu danych w miarę potrzeb do centralnej lokalizacji w celu ich dalszego przetwarzania.

Firmy, które nie korzystają z usługi Data Factory, muszą tworzyć niestandardowe składniki umożliwiające przepływ danych lub projektować własne usługi, aby zintegrować źródła danych i systemy przetwarzania. Takie podejście jest kosztowne, a integracja i utrzymanie systemów stwarza trudności. Ponadto rozwiązanie to rzadko zapewnia mechanizmy kontrolne oraz funkcje monitorowania i wysyłania alertów na poziomie korporacyjnym. Możliwości te są natomiast dostępne w przypadku w pełni zarządzanej usługi.

Usługa Data Factory udostępnia działanie kopiowania w potoku danych, które pozwala przenosić dane z lokalnych magazynów danych i źródeł danych w chmurze do centralnego magazynu danych w chmurze w celu przeprowadzenia kolejnych etapów analizy. Na przykład można pobrać dane z usługi Azure Data Lake Store i przekształcić je później za pomocą usługi obliczeniowej Azure Data Lake Analytics. Można też pobrać dane z usługi Azure Blob Storage, aby przekształcić je przy użyciu klastra usługi Azure HDInsight na platformie Hadoop.

### <a name="transform-and-enrich"></a>Przekształcanie i wzbogacanie
Po umieszczeniu danych w centralnym magazynie danych w chmurze zebrane dane należy przetworzyć lub przekształcić za pomocą usług obliczeniowych, takich jak HDInsight Hadoop, Spark, Data Lake Analytics i Machine Learning. Przekształcone dane powinny być generowane niezawodnie i zgodnie z możliwym do utrzymania i kontrolowania harmonogramem, aby zapewnić środowiskom produkcyjnym zaufane dane. 

### <a name="publish"></a>Publikowanie 
Przekształcone dane można dostarczać z chmury do źródeł lokalnych, takich jak program SQL Server, lub zachować te dane w źródłach magazynowania w chmurze do użycia przez narzędzia analizy biznesowej i narzędzia analityczne oraz inne aplikacje.

## <a name="key-components"></a>Główne składniki
Subskrypcja platformy Azure może zawierać jedno lub więcej wystąpień usługi Azure Data Factory (lub fabryk danych). Usługa Azure Data Factory składa się z czterech kluczowych składników. Ich współdziałanie pozwala udostępnić platformę umożliwiającą tworzenie opartych na danych przepływów pracy wraz z etapami służącymi do przenoszenia i przekształcania danych. 

### <a name="pipeline"></a>Potok
Fabryka danych może obejmować jeden lub wiele potoków. Potok jest grupą działań, które umożliwiają wykonanie zadania. Na przykład potok może zawierać grupę działań, które pozwalają pozyskać dane z obiektu blob platformy Azure, a następnie uruchomić zapytanie programu Hive w klastrze usługi HDInsight w celu podzielenia danych. Zaletą korzystania z potoku jest możliwość zarządzania zestawem działań. Na przykład zamiast zarządzać poszczególnymi działaniami można wdrożyć i zaplanować potok. 

### <a name="activity"></a>Działanie
Potok może obejmować jedno lub wiele działań. Działania definiują akcje do wykonania na danych. Można na przykład użyć działania kopiowania w celu skopiowania danych z jednego magazynu danych do drugiego. Podobnie można użyć działania programu Hive, które uruchomi zapytanie programu Hive w klastrze usługi Azure HDInsight, aby przekształcić lub przeanalizować dane. Usługa Data Factory obsługuje dwa typy działań — w zakresie przekształcania oraz przenoszenia danych.

### <a name="data-movement-activities"></a>Działania dotyczące przenoszenia danych
Działanie kopiowania w usłudze Data Factory kopiuje dane z magazynu danych źródła do magazynu danych ujścia. Usługa Data Factory obsługuje następujące magazyny danych. Dane z dowolnego źródła można zapisać do dowolnego ujścia. Kliknij magazyn danych, aby dowiedzieć się, jak kopiować dane do i z tego magazynu.

[!INCLUDE [data-factory-supported-data-stores](../../../includes/data-factory-supported-data-stores.md)]

Aby uzyskać więcej informacji, zobacz artykuł [Działania dotyczące przenoszenia danych](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Działania dotyczące przekształcania danych
[!INCLUDE [data-factory-transformation-activities](../../../includes/data-factory-transformation-activities.md)]

Aby uzyskać więcej informacji, zobacz artykuł [Działania dotyczące przekształcania danych](data-factory-data-transformation-activities.md).

### <a name="custom-net-activities"></a>Niestandardowe działania programu .NET
Jeśli musisz przenieść dane do/z magazynu danych nieobsługiwanego przez działanie kopiowania lub przekształcić dane za pomocą własnej logiki, utwórz **niestandardowe działanie platformy .NET**. Aby uzyskać szczegółowe informacje na temat tworzenia i używania niestandardowego działania, zobacz artykuł [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) (Korzystanie z niestandardowych działań w potoku usługi Azure Data Factory).

### <a name="datasets"></a>Zestawy danych
Każde działanie pobiera 0 lub więcej zestawów danych jako dane wejściowe i generuje co najmniej jeden zestaw danych jako dane wyjściowe. Zestawy danych reprezentują struktury w magazynach danych. Struktury te po prostu wskazują na dane, które mają być używane w działaniach jako dane wejściowe lub wyjściowe. Na przykład zestaw danych usługi Azure Blob określa kontener obiektów blob i folder w usłudze Azure Blob Storage, z których potok ma odczytywać dane. Alternatywnie zestaw danych tabeli SQL Azure określa tabelę, w której działanie zapisuje dane wyjściowe. 

### <a name="linked-services"></a>Połączone usługi
Połączone usługi działają podobnie do parametrów połączenia, umożliwiając definiowanie informacji wymaganych przez usługę Data Factory do nawiązywania połączeń z zasobami zewnętrznymi. Mechanizm ten działa następująco: połączona usługa zawiera definicję połączenia ze źródłem danych, a zestaw danych reprezentuje strukturę danych. Na przykład połączona usługa Azure Storage określa parametry połączenia, które umożliwiają połączenie z kontem usługi Azure Storage. Natomiast zestaw danych usługi Azure Blob określa kontener obiektów blob oraz folder, który zawiera dane.   

Połączone usługi w usłudze Fabryka danych służą do dwóch celów:

* Reprezentowanie **magazynu danych**, w tym między innymi lokalnego serwera SQL Server, bazy danych Oracle, udziału plików lub konta usługi Azure Blob Storage. Listę obsługiwanych magazynów danych można znaleźć w sekcji [Data movement activities](#data-movement-activities) (Działania przenoszenia danych).
* Reprezentowanie **zasobu obliczeniowego**, który może hostować wykonywanie działania. Na przykład działanie HDInsightHive jest wykonywane w klastrze HDInsight na platformie Hadoop. Listę obsługiwanych środowisk obliczeniowych można znaleźć w sekcji [Data transformation activities](#data-transformation-activities) (Działania przekształcania danych).

### <a name="relationship-between-data-factory-entities"></a>Relacje między obiektami usługi Data Factory
![Diagram: Data Factory, usługa integracji danych w chmurze — najważniejsze pojęcia](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Rysunek 2.** Relacje między elementami Zestaw danych, Działanie, Potok i Połączona usługa

## <a name="supported-regions"></a>Obsługiwane regiony
Obecnie można tworzyć fabryki danych w regionach **Zachodnie stany USA**, **Wschodnie stany USA** oraz **Europa Północna**. Jednak fabryka danych może mieć dostęp do magazynów danych i usług obliczeniowych w innych regionach świadczenia usługi Azure, aby przenosić dane między magazynami danych lub przetwarzać dane przy użyciu usług obliczeniowych.

Sama usługa Fabryka danych Azure nie przechowuje żadnych danych. Usługa umożliwia tworzenie opartych na danych przepływów pracy do aranżowania przenoszenia danych między [obsługiwanymi magazynami danych](#data-movement-activities) oraz przetwarzania danych przy użyciu [usług obliczeniowych](#data-transformation-activities) w innych regionach lub w środowisku lokalnym. Umożliwia także [monitorowanie przepływów pracy i zarządzanie nimi](data-factory-monitor-manage-pipelines.md) przy użyciu zarówno mechanizmów programowych, jaki i interfejsu użytkownika.

Chociaż usługa Data Factory jest dostępna tylko w regionach **Zachodnie stany USA**, **Wschodnie stany USA** i **Europa Północna**, to usługa zapewniająca przenoszenie danych w usłudze Data Factory jest dostępna [globalnie](data-factory-data-movement-activities.md#global) w kilku regionach. Jeśli magazyn danych znajduje się za zaporą, dane są przenoszone przez [bramę zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) zainstalowaną w środowisku lokalnym.

Załóżmy na przykład, że środowiska obliczeniowe, takie jak klaster usługi Azure HDInsight i usługa Azure Machine Learning, są uruchamiane z regionu Europa Zachodnia. Można utworzyć i wykorzystać wystąpienie usługi Azure Data Factory w regionie Europa Północna oraz użyć go do planowania zadań wykonywanych w środowiskach obliczeniowych w regionie Europa Zachodnia. Wyzwolenie zadania w środowisku obliczeniowym zajmuje usłudze Data Factory kilka milisekund, ale czas uruchomienia zadania w środowisku obliczeniowym nie zmienia się.

## <a name="get-started-with-creating-a-pipeline"></a>Rozpoczynanie tworzenia potoku
Potoki danych w usłudze Azure Data Factory można tworzyć za pomocą następujących narzędzi lub interfejsów API: 

- Azure Portal
- Visual Studio
- PowerShell
- Interfejs API .NET
- Interfejs API REST
- Szablon usługi Azure Resource Manager 

Wykonaj instrukcje krok po kroku w następujących samouczkach, aby dowiedzieć się, jak tworzyć fabryki danych z potokami danych:

| Samouczek | Opis |
| --- | --- |
| [Przenoszenie danych między dwoma magazynami danych w chmurze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z magazynu obiektów blob do bazy danych SQL. |
| [Przekształcanie danych przy użyciu klastra Hadoop](data-factory-build-your-first-pipeline.md) |W tym samouczku utworzysz pierwszą fabrykę danych platformy Azure przy użyciu potoku danych, który **przetwarza dane**, uruchamiając skrypt Hive w klastrze Azure HDInsight (Hadoop). |
| [Przenoszenie danych między lokalnym magazynem danych i magazynem danych w chmurze przy użyciu bramy zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) |W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z **lokalnej** bazy danych programu SQL Server do obiektu blob platformy Azure. W ramach tego przewodnika zainstalujesz i skonfigurujesz bramę zarządzania danymi na tym komputerze. |
