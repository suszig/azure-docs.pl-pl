---
title: "Wprowadzenie do usługi Data Factory — usługi integracji danych | Microsoft Docs"
description: "Dowiedz się, czym jest usługa Azure Data Factory: usługa integracji danych w chmurze, która służy do aranżacji i automatyzacji przenoszenia i przekształcania danych."
keywords: "integracja danych, integracja danych w chmurze, czym jest usługa azure data factory"
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
ms.date: 04/21/2017
ms.author: shlo
translationtype: Human Translation
ms.sourcegitcommit: 260208e7c7a08110eb3c885ef86ec4c18ff42fc9
ms.openlocfilehash: 40552b5d3cea5b04826c08e7b4b1d046a9fcefba
ms.lasthandoff: 04/23/2017


---
# <a name="introduction-to-azure-data-factory-service-a-data-integration-service-in-the-cloud"></a>Wprowadzenie do usługi Azure Data Factory, usługa integracji danych w chmurze
## <a name="what-is-azure-data-factory"></a>Czym jest usługa Azure Data Factory?
W jaki sposób istniejące dane są wykorzystywane w biznesowej rzeczywistości opartej na danych big data? Czy można wzbogacić informacje wygenerowane w chmurze danymi referencyjnymi, pochodzącymi z lokalnych — lub innych, różnorodnych — źródeł danych? Widać, że potrzebna jest platforma, która umożliwia agregowanie i przetwarzanie danych z różnych źródeł. Azure Data Factory jest usługą integracji danych w chmurze, która pozwala organizować i automatyzować **przenoszenie** i **przekształcanie** danych. Można tworzyć rozwiązania integracji danych, które umożliwiają pozyskiwanie danych wejściowych z różnych magazynów danych, przekształcanie i przetwarzanie tych danych oraz publikowanie danych wyjściowych w innych magazynach danych. 

![Diagram: Omówienie usługi Data Factory, usługi integracji danych](./media/data-factory-introduction/what-is-azure-data-factory.png)

**Rysunek 1.** Pozyskiwanie danych z różnych źródeł danych, przygotowywanie, przekształcanie i analizowanie danych, a następnie publikowanie danych gotowych do użytku.


## <a name="what-does-it-offer"></a>Jakie są korzyści? 
Tradycyjne podejście do integracji danych jest oparte na tworzeniu procesów wyodrębniania, przekształcania i ładowania (ETL, Extract-Transform-Load), które umożliwiają wyodrębnianie danych z różnych źródeł danych w organizacji, przekształcanie danych zgodnie ze schematem docelowym magazynu danych przedsiębiorstwa oraz ich ładowanie do magazynu danych przedsiębiorstwa, jak pokazano na poniższej ilustracji. W efekcie magazyn danych przedsiębiorstwa staje się pojedynczym źródłem prawdziwych danych, z którego korzystają rozwiązania analizy biznesowej.

![Tradycyjne procesy ETL](media/data-factory-introduction/traditional-etl.png)
**Tradycyjne procesy ETL**

Ilość danych używanych obecnie przez przedsiębiorstwa rośnie wykładniczo, a wraz z nią — jak pokazano na poniższej ilustracji — zwiększa się ich różnorodność i złożoność. Dane te — bardziej zróżnicowane niż kiedykolwiek wcześniej — mają różne formaty, pochodzą z infrastruktury lokalnej lub chmury i są gromadzone z różną szybkością. Dane muszą być przetwarzane w różnych lokalizacjach geograficznych przy użyciu oprogramowania typu open source, produktów komercyjnych oraz niestandardowych usług przetwarzania. Takie rozwiązanie jest kosztowne, a jego integracja i utrzymanie stwarza trudności. Potrzeba zapewnienia elastyczności w celu dostosowania się do obecnych zmian w środowisku danych big data stanowi okazję do rozszerzenia tradycyjnego magazynu danych przedsiębiorstwa o możliwości wymagane od nowoczesnego systemu uzyskiwania informacji. Usługa Azure Data Factory jest platformą kompozycyjną, umożliwiającą korzystanie z funkcji tradycyjnego magazynu danych przedsiębiorstwa oraz możliwości oferowanych przez zmieniające się środowisko danych. Pozwala to przedsiębiorstwom podejmować decyzje oparte na wszystkich dostępnych danych.

![Nowe środowisko danych big data](media/data-factory-introduction/new-big-data-landscape.png)
**Nowe środowisko danych big data**

Dzięki usłudze Azure Data Factory firmy mogą wykorzystać tę różnorodność przy użyciu platformy, która pozwala **tworzyć potoki uzyskiwania informacji za pomocą usług magazynowania, przenoszenia i przetwarzania danych** oraz zarządzać zaufanymi zasobami danych.

Usługa Azure Data Factory zapewnia następujące możliwości:
- **Łatwe korzystanie z różnych systemów magazynowania i przetwarzania danych**. 

    Dane używane w przedsiębiorstwach mają różne typy i znajdują się w rozproszonych źródłach. Pierwszy etap tworzenia systemu uzyskiwania informacji polega na połączeniu wszystkich wymaganych źródeł danych i systemów przetwarzania, takich jak usługi SaaS, udziały plików, serwery FTP oraz usługi sieci Web, i przeniesieniu danych w miarę potrzeb do centralnej lokalizacji w celu ich dalszego przetwarzania.

    Firmy, które nie korzystają z usługi Data Factory, muszą tworzyć niestandardowe składniki umożliwiające przepływ danych lub projektować własne usługi, aby zintegrować źródła danych i systemy przetwarzania. Takie podejście jest kosztowne, a integracja i utrzymanie systemów stwarza trudności. Ponadto rozwiązanie to rzadko zapewnia mechanizmy kontrolne oraz funkcje monitorowania i wysyłania alertów na poziomie korporacyjnym. Możliwości te są natomiast dostępne w przypadku w pełni zarządzanej usługi.

    Usługa Data Factory udostępnia działanie kopiowania w potoku danych, które pozwala przenosić dane z lokalnych magazynów danych i źródeł danych w chmurze do centralnego magazynu danych w chmurze w celu przeprowadzenia kolejnych etapów analizy. Na przykład można pobrać dane z usługi Azure Data Lake Store i przekształcić je później za pomocą usługi obliczeniowej Azure Data Lake Analytics. Można też pobrać dane z usługi Azure Blob Storage, aby przekształcić je przy użyciu klastra usługi Azure HDInsight na platformie Hadoop.
- **Przekształcanie danych w wiarygodne informacje**. 

    Po przeniesieniu danych do centralnego magazynu w chmurze warto utworzyć i wdrożyć potoki umożliwiające niezawodne uzyskiwanie przekształconych danych zgodnie z łatwym w zarządzaniu harmonogramem w celu zapewnienia dopływu wiarygodnych informacji do środowisk produkcyjnych. Przekształcanie danych w usłudze Azure Data Factory jest możliwe dzięki działaniom przekształcania, takim jak Hive, Pig, MapReduce i wykonywanie wsadowe w usłudze Azure Machine Learning, oraz niestandardowym działaniom C# uruchomionym w klastrze usługi Azure HDInsight na platformie Hadoop lub na maszynach wirtualnych usługi Azure Machine Learning albo w puli maszyn wirtualnych usługi Azure Batch.
- **Centralne monitorowanie potoków danych**.

    W przypadku korzystania ze zróżnicowanych danych istotny jest niezawodny i pełny wgląd w usługi magazynowania, przetwarzania i przenoszenia danych. Usługa Data Factory umożliwia szybką i dokładną ocenę kondycji potoku danych, identyfikowanie problemów i wprowadzanie poprawek, jeśli to konieczne. Możesz wizualnie śledzić pochodzenie danych i relacje między danymi w dowolnych źródłach. Przeglądaj pełną historię wykonywania zadań, kondycji systemu i zależności za pomocą jednego pulpitu nawigacyjnego do monitorowania.

## <a name="how-does-it-work"></a>Jak to działa? 
W usłudze Azure Data Factory są trzy etapy uzyskiwania informacji:

![Trzy etapy uzyskiwania informacji](media/data-factory-introduction/three-information-production-stages.png)

- **Łączenie i zbieranie**: dane z różnych źródeł są gromadzone w jednym miejscu.
- **Przekształcanie i wzbogacanie**: zebrane dane są przetwarzane lub przekształcane.
- **Publikowanie**: Dane są publikowane do użycia przez narzędzia analizy biznesowej, narzędzia analityczne i inne aplikacje.

## <a name="key-components"></a>Główne składniki
Subskrypcja platformy Azure może zawierać jedno lub więcej wystąpień usługi Azure Data Factory (lub fabryk danych). Usługa Azure Data Factory składa się z czterech kluczowych składników. Ich współdziałanie pozwala udostępnić platformę umożliwiającą tworzenie zarówno prostych, jak i złożonych aranżacji przenoszenia i przekształcania danych.

### <a name="activity"></a>Działanie
Działania definiują akcje do wykonania na danych. Można na przykład użyć działania kopiowania w celu skopiowania danych z jednego magazynu danych do drugiego. Podobnie można użyć działania programu Hive, które uruchomi zapytanie programu Hive w klastrze usługi Azure HDInsight, aby przekształcić lub przeanalizować dane. Usługa Data Factory obsługuje dwa typy działań — w zakresie przekształcania oraz przenoszenia danych.

Poszczególne działania mogą — ale nie muszą — korzystać z wejściowych zestawów danych, pozwalając uzyskać co najmniej jeden wyjściowy zestaw danych. 


### <a name="data-movement-activities"></a>Działania dotyczące przenoszenia danych
Działanie kopiowania w usłudze Data Factory kopiuje dane z magazynu danych źródła do magazynu danych ujścia. Usługa Data Factory obsługuje następujące magazyny danych. Dane z dowolnego źródła można zapisać do dowolnego ujścia. Kliknij magazyn danych, aby dowiedzieć się, jak kopiować dane do i z tego magazynu.

[!INCLUDE [data-factory-supported-data-stores](../../includes/data-factory-supported-data-stores.md)]

Aby uzyskać więcej informacji, zobacz artykuł [Działania dotyczące przenoszenia danych](data-factory-data-movement-activities.md).

### <a name="data-transformation-activities"></a>Działania dotyczące przekształcania danych
[!INCLUDE [data-factory-transformation-activities](../../includes/data-factory-transformation-activities.md)]

Aby uzyskać więcej informacji, zobacz artykuł [Działania dotyczące przekształcania danych](data-factory-data-transformation-activities.md).

Jeśli musisz przenieść dane do/z magazynu danych nieobsługiwanego przez działanie kopiowania lub przekształcić dane za pomocą własnej logiki, utwórz **niestandardowe działanie platformy .NET**. Aby uzyskać szczegółowe informacje na temat tworzenia i używania niestandardowego działania, zobacz artykuł [Use custom activities in an Azure Data Factory pipeline](data-factory-use-custom-activities.md) (Korzystanie z niestandardowych działań w potoku usługi Azure Data Factory).

### <a name="pipeline"></a>Potok
Potok jest grupą działań, które umożliwiają wykonanie zadania. Na przykład potok może zawierać grupę działań, które pozwalają pozyskać dane z obiektu blob platformy Azure, a następnie uruchomić zapytanie programu Hive w klastrze usługi HDInsight w celu podzielenia danych dzienników. Zaletą korzystania z potoku jest możliwość zarządzania zestawem działań. Na przykład zamiast zarządzać poszczególnymi działaniami można wdrożyć i zaplanować potok.

### <a name="datasets"></a>Zestawy danych
Zestawy danych reprezentują struktury w magazynach danych. Struktury te po prostu wskazują na dane, które mają być używane w działaniach jako dane wejściowe lub wyjściowe. Na przykład zestaw danych usługi Azure Blob określa kontener obiektów blob i folder w usłudze Azure Blob Storage, z których potok ma odczytywać dane. 

### <a name="linked-services"></a>Połączone usługi
Połączone usługi działają podobnie do parametrów połączenia, umożliwiając definiowanie informacji wymaganych przez usługę Data Factory do nawiązywania połączeń z zasobami zewnętrznymi. Mechanizm ten działa następująco: zestaw danych reprezentuje strukturę danych, a połączona usługa zawiera definicję połączenia ze źródłem danych.  Połączone usługi w usłudze Fabryka danych służą do dwóch celów:

* Reprezentowanie **magazynu danych**, w tym między innymi lokalnego serwera SQL Server, bazy danych Oracle, udziału plików lub konta usługi Azure Blob Storage. Listę obsługiwanych magazynów danych można znaleźć w sekcji [Data movement activities](#data-movement-activities) (Działania przenoszenia danych).
* Reprezentowanie **zasobu obliczeniowego**, który może hostować wykonywanie działania. Na przykład działanie HDInsightHive jest wykonywane w klastrze HDInsight na platformie Hadoop. Listę obsługiwanych środowisk obliczeniowych można znaleźć w sekcji [Data transformation activities](#data-transformation-activities) (Działania przekształcania danych).

### <a name="relationship-between-data-factory-entities"></a>Relacje między obiektami usługi Data Factory
![Diagram: Data Factory, usługa integracji danych w chmurze — najważniejsze pojęcia](./media/data-factory-introduction/data-integration-service-key-concepts.png)
**Rysunek 2.** Relacje między elementami Zestaw danych, Działanie, Potok i Połączona usługa

## <a name="supported-regions"></a>Obsługiwane regiony
Obecnie można tworzyć fabryki danych w regionach **Zachodnie stany USA**, **Wschodnie stany USA** oraz **Europa Północna**. Jednak fabryka danych może mieć dostęp do magazynów danych i usług obliczeniowych w innych regionach świadczenia usługi Azure, aby przenosić dane między magazynami danych lub przetwarzać dane przy użyciu usług obliczeniowych.

Sama usługa Fabryka danych Azure nie przechowuje żadnych danych. Usługa umożliwia tworzenie przepływów opartych na danych do aranżowania przenoszenia danych między [obsługiwanymi magazynami danych](data-factory-data-movement-activities.md#supported-data-stores-and-formats) oraz przetwarzania danych przy użyciu [usług obliczeniowych](data-factory-compute-linked-services.md) w innych regionach lub w środowisku lokalnym. Umożliwia także [monitorowanie przepływów pracy i zarządzanie nimi](data-factory-monitor-manage-pipelines.md) przy użyciu zarówno mechanizmów programowych, jaki i interfejsu użytkownika.

Chociaż usługa Data Factory jest dostępna tylko w regionach **Zachodnie stany USA**, **Wschodnie stany USA** i **Europa Północna**, to usługa zapewniająca przenoszenie danych w usłudze Data Factory jest dostępna [globalnie](data-factory-data-movement-activities.md#global) w kilku regionach. W przypadku gdy magazyn danych znajduje się za zaporą, dane są przenoszone przez [bramę zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) zainstalowaną w środowisku lokalnym.

Załóżmy na przykład, że środowiska obliczeniowe, takie jak klaster usługi Azure HDInsight i usługa Azure Machine Learning, są uruchamiane z regionu Europa Zachodnia. Można utworzyć i wykorzystać wystąpienie usługi Azure Data Factory w regionie Europa Północna oraz użyć go do planowania zadań wykonywanych w środowiskach obliczeniowych w regionie Europa Zachodnia. Wyzwolenie zadania w środowisku obliczeniowym zajmuje usłudze Data Factory kilka milisekund, ale czas uruchomienia zadania w środowisku obliczeniowym nie zmienia się.

W przyszłości zamierzamy udostępnić usługę Fabryka danych Azure w każdym regionie geograficznym obsługiwanym przez platformę Azure.

## <a name="next-steps"></a>Następne kroki
Wykonaj instrukcje krok po kroku w następujących samouczkach, aby dowiedzieć się, jak tworzyć fabryki danych z potokami danych:

| Samouczek | Opis |
| --- | --- |
| [Przenoszenie danych między dwoma magazynami danych w chmurze](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) |W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z magazynu obiektów blob do bazy danych SQL. |
| [Przekształcanie danych przy użyciu klastra Hadoop](data-factory-build-your-first-pipeline.md) |W tym samouczku utworzysz pierwszą fabrykę danych platformy Azure przy użyciu potoku danych, który **przetwarza dane**, uruchamiając skrypt Hive w klastrze Azure HDInsight (Hadoop). |
| [Przenoszenie danych między lokalnym magazynem danych i magazynem danych w chmurze przy użyciu bramy zarządzania danymi](data-factory-move-data-between-onprem-and-cloud.md) |W tym samouczku utworzysz fabrykę danych z potokiem, który **przenosi dane** z **lokalnej** bazy danych programu SQL Server do obiektu blob platformy Azure. W ramach tego przewodnika zainstalujesz i skonfigurujesz bramę zarządzania danymi na tym komputerze. |

