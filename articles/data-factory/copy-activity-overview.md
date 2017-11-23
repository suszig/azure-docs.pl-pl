---
title: "Aktywność kopiowania w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej o działanie kopiowania w fabryce danych Azure, której można skopiować dane z magazynu danych obsługiwanych źródłowych w magazynie danych obsługiwanych ujścia."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: jingwang
ms.openlocfilehash: a2f370998ea219f9d36a6cda26405b6023666f92
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="copy-activity-in-azure-data-factory"></a>Działanie kopiowania w fabryce danych Azure

## <a name="overview"></a>Omówienie

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-data-movement-activities.md)
> * [Wersja 2 — wersja zapoznawcza](copy-activity-overview.md)

W fabryce danych Azure działanie kopiowania służy do kopiowania danych między danych magazynów znajduje się w sieci lokalnej i w chmurze. Po skopiowaniu danych można go dalej transformacji i analizy. Działanie kopiowania umożliwia także publikowanie transformacji i wyniki analizy do analizy biznesowej (BI) i użycie aplikacji.

![Rola działanie kopiowania](media/copy-activity-overview/copy-activity.png)

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [działanie kopiowania w wersji 1](v1/data-factory-data-movement-activities.md).

Działanie kopiowania jest wykonywana na [integrację środowiska uruchomieniowego](concepts-integration-runtime.md). W scenariuszu kopii dla innych danych można użyć różnych wersji środowiska uruchomieniowego integracji:

* Jeśli kopiowanie danych między danymi przechowuje oba są publicznie, działanie kopiowania może upoważnionego przez **środowiska uruchomieniowego integracji Azure**, która jest bezpieczne, niezawodne i skalowalne, i [globalnie dostępną](concepts-integration-runtime.md#integration-runtime-location).
* Podczas kopiowania danych z/do magazynów danych znajdujących się na lokalnych lub w sieci przy użyciu kontroli dostępu (na przykład sieci wirtualnej Azure), należy skonfigurować **hosta samodzielnego zintegrowanego środowiska uruchomieniowego** upoważnienie kopię danych.

Środowiska uruchomieniowego integracji muszą być skojarzone z każdym źródłowy i odbiorczy magazynu danych. Dowiedz się więcej informacji na temat aktywności kopiowania [Określa, które IR, aby użyć](concepts-integration-runtime.md#determining-which-ir-to-use).

Działanie kopiowania przechodzi przez następujące etapy, aby skopiować dane ze źródła do ujścia. Usługa obsługującego działanie kopiowania:

1. Odczytuje dane z magazynu danych źródłowych.
2. Wykonuje serializacji/deserializacji, kompresji i dekompresji, mapowanie kolumn itp. Tak, aby te operacje konfiguracji wejściowy zestaw danych, wyjściowy zestaw danych i działanie kopiowania.
3. Zapisuje dane w magazynie danych zbiornika i docelowego.

![Omówienie działania kopiowania](media/copy-activity-overview/copy-activity-overview.png)

## <a name="supported-data-stores-and-formats"></a>Magazyny danych obsługiwane i formaty

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores.md)]

### <a name="supported-file-formats"></a>Obsługiwane formaty plików

Można użyć do działania kopiowania **skopiuj pliki jako — jest** między dwoma magazynów danych opartych na plikach, w których przypadku danych jest kopiowana wydajnie bez żadnych serializacji/deserializacji.

Działanie kopiowania obsługuje również odczytywanie z oraz zapisywanie do plików w określonym formacie: **tekst JSON, Avro, ORC i Parquet**i koder-dekoder kompresji **GZip, Deflate, BZip2 i ZipDeflate** są obsługiwane. Zobacz [obsługiwane formaty plików i kompresji](supported-file-formats-and-compression-codecs.md) ze szczegółami.

Na przykład można wykonywać następujące działania kopiowania:

* Kopiowanie danych w lokalnym programie SQL Server i zapisać w formacie ORC Azure Data Lake Store.
* Skopiuj pliki w formacie tekstowym (CSV) z lokalnego systemu plików i zapisać w formacie Avro obiektów Blob platformy Azure.
* Skopiuj pliki zip z lokalnego systemu plików, a następnie dekompresja ziemi do usługi Azure Data Lake Store.
* Kopiowanie danych w formacie tekstowym skompresowane (CSV) GZip z obiektów Blob platformy Azure i zapisu do bazy danych SQL Azure.

## <a name="supported-regions"></a>Obsługiwane regiony

Usługa obsługującego działanie kopiowania jest dostępna globalnie w regionach i lokalizacji geograficznych wymienionych w [lokalizacji środowiska uruchomieniowego integracji Azure](concepts-integration-runtime.md#integration-runtime-location). Ogólnie dostępna topologia zapewnia przepływ danych wydajne, pozwalający na uniknięcie zwykle przeskoków między regionu. Zobacz [usług według regionu](https://azure.microsoft.com/regions/#services) dostępność fabryki danych i przenoszenie danych w regionie.

## <a name="configuration"></a>Konfiguracja

Aby użyć działania kopiowania w fabryce danych Azure, musisz:

1. **Tworzenie połączonej usługi magazynu danych źródła i ujścia magazynu danych.** Zobacz sekcję "Połączone usługi właściwości" artykułu łącznika na sposób konfigurowania i obsługiwanych właściwości. Można znaleźć na liście obsługiwanych łącznika w [obsługiwane formaty i magazyny danych](#supported-data-stores-and-formats) sekcji.
2. **Tworzenie zestawów danych dla źródłowy i odbiorczy.** Zapoznaj się ze źródłem i sink sekcji "Zestawu danych właściwości" artykuły łącznika na sposób konfigurowania i obsługiwanych właściwości.
3. **Utworzyć potok z działania kopiowania.** Następna sekcja zawiera przykład.  

### <a name="syntax"></a>Składnia

Następujący szablon działania kopiowania zawiera stanowi wyczerpującej listy obsługiwanych właściwości. Określ te, które pasują do danego scenariusza.

```json
"activities":[
    {
        "name": "CopyActivityTemplate",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<source dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<sink dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>",
                <properties>
            },
            "sink": {
                "type": "<sink type>"
                <properties>
            },
            "translator":
            {
                "type": "TabularTranslator",
                "columnMappings": "<column mapping>"
            },
            "cloudDataMovementUnits": <number>,
            "parallelCopies": <number>,
            "enableStaging": true/false,
            "stagingSettings": {
                <properties>
            },
            "enableSkipIncompatibleRow": true/false,
            "redirectIncompatibleRowSettings": {
                <properties>
            }
        }
    }
]
```

### <a name="syntax-details"></a>Szczegóły składni

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type działania kopiowania: **kopiowania** | Tak |
| Dane wejściowe | Określ zestaw danych został utworzony wskazującą na źródło danych. Działanie kopiowania obsługuje tylko jeden danych wejściowych. | Tak |
| wyjścia | Określ zestaw danych został utworzony wskazującą na obiekt sink danych. Działanie kopiowania obsługuje tylko pojedynczego wyjścia. | Tak |
| typeProperties | Grupa właściwości, aby skonfigurować działanie Kopiuj. | Tak |
| źródło | Określ typ źródła kopiowania i odpowiednie właściwości na temat pobierania danych.<br/><br/>Dowiedz się więcej szczegółów w sekcji "Kopiuj właściwości działania" w artykule łącznika na liście [obsługiwane formaty i magazyny danych](#supported-data-stores-and-formats). | Tak |
| obiekt sink | Określ typ ujścia kopiowania i odpowiednie właściwości na temat zapisywania danych.<br/><br/>Dowiedz się więcej szczegółów w sekcji "Kopiuj właściwości działania" w artykule łącznika na liście [obsługiwane formaty i magazyny danych](#supported-data-stores-and-formats). | Tak |
| Translator | Określ mapowania kolumn jawne ze źródła do zbiornika. Ma zastosowanie, gdy domyślne zachowanie kopiowania nie może spełnić potrzeby.<br/><br/>Dowiedz się więcej szczegółów z [schemat i dane mapowania typu](copy-activity-schema-and-type-mapping.md). | Nie |
| cloudDataMovementUnits | Określ powerfulness z [środowiska uruchomieniowego integracji Azure](concepts-integration-runtime.md) upoważnienie kopię danych.<br/><br/>Dowiedz się więcej szczegółów z [jednostki przepływu danych w chmurze](copy-activity-performance.md). | Nie |
| parallelCopies | Określanie równoległości interesujące Kopiuj działania do użycia podczas odczytu danych z źródła i zapisywania danych do zbiornika.<br/><br/>Dowiedz się więcej szczegółów z [równoległych kopii](copy-activity-performance.md#parallel-copy). | Nie |
| enableStaging<br/>stagingSettings | Wybierz etap przejściowy danych w magazynie obiektów blob aa zamiast bezpośrednio kopii danych ze źródła do zbiornika.<br/><br/>Poznaj przydatne w scenariuszach i szczegóły konfiguracji z [przemieszczane kopiowania](copy-activity-performance.md#staged-copy). | Nie |
| enableSkipIncompatibleRow<br/>redirectIncompatibleRowSettings| Wybierz sposób obsługi niezgodne wierszy podczas kopiowania danych ze źródła do zbiornika.<br/><br/>Dowiedz się więcej szczegółów z [odporność na uszkodzenia](copy-activity-fault-tolerance.md). | Nie |

## <a name="monitoring"></a>Monitorowanie

Szczegóły wykonywania działania kopiowania i charakterystyki wydajności są zwracane w przypadku działania kopiowania wynik uruchomienia -> sekcji danych wyjściowych. Poniżej znajduje się lista wyczerpana. Dowiedz się, jak monitorować działania Uruchom z [szybkiego startu w sekcji monitorowanie](quickstart-create-data-factory-dot-net.md#monitor-a-pipeline-run). Możesz porównać wydajności i konfiguracji scenariusz działania kopiowania [dotyczące wydajności](copy-activity-performance.md#performance-reference) z testów wewnętrznych.

| Nazwa właściwości  | Opis | Jednostka |
|:--- |:--- |:--- |
| DataRead | Rozmiar danych ze źródła do odczytu | Wartość Int64 w bajtach |
| DataWritten | Rozmiar danych zapisywane do zbiornika | Wartość Int64 w bajtach |
| rowsCopied | Liczba wierszy kopiowane (nie dotyczy binarne kopii). | Wartości Int64 (Brak jednostki) |
| rowsSkipped | Liczba wierszy niezgodne pomijany. Można włączyć funkcję przez zestaw "enableSkipIncompatibleRow" na wartość true. | Wartości Int64 (Brak jednostki) |
| Przepływność | Współczynnik szybkości transferu danych | Liczba zmiennoprzecinkowa w KB/s |
| copyDuration | Czas trwania kopii | Wartość Int32 w sekundach |
| sqlDwPolyBase | Jeśli program PolyBase jest używany podczas kopiowania danych do usługi SQL Data Warehouse. | Wartość logiczna |
| redshiftUnload | Jeśli zwolnienie jest używany podczas kopiowania danych z Redshift. | Wartość logiczna |
| hdfsDistcp | Jeśli narzędzia DistCp jest używany podczas kopiowania danych z systemu plików HDFS. | Wartość logiczna |
| effectiveIntegrationRuntime | Pokaż Runtime(s) integracji używanego do uruchamiania w formacie działania pozwalają `<IR name> (<region if it's Azure IR>)`. | Tekst (ciąg) |
| usedCloudDataMovementUnits | Skuteczne chmury jednostki przepływu danych podczas kopiowania. | Wartość Int32 |
| redirectRowPath | Ścieżka do dziennika pominiętych wierszy niezgodne w magazynie obiektów blob, należy skonfigurować w obszarze "redirectIncompatibleRowSettings". Zobacz w poniższym przykładzie. | Tekst (ciąg) |
| billedDuration | Czas trwania opłaty są naliczane za przenoszenia danych. | Wartość Int32 w sekundach |

```json
"output": {
    "dataRead": 1024,
    "dataWritten": 2048,
    "rowsCopies": 100,
    "rowsSkipped": 2,
    "throughput": 1024.0,
    "copyDuration": 3600,
    "redirectRowPath": "https://<account>.blob.core.windows.net/<path>/<pipelineRunID>/",
    "redshiftUnload": true,
    "sqlDwPolyBase": true,
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)",
    "usedCloudDataMovementUnits": 8,
    "billedDuration": 28800
}
```

## <a name="schema-and-data-type-mapping"></a>Mapowanie typu danych i schematu

Zobacz [schemat i dane mapowania typu](copy-activity-schema-and-type-mapping.md), która opisuje sposób działania kopiowania mapowania danych źródłowych do zbiornika.

## <a name="fault-tolerance"></a>Odporność na uszkodzenia

Domyślnie działanie kopiowania zatrzymuje kopiowanie danych i zwraca błąd, po napotkaniu niezgodne dane między źródłowy i odbiorczy. Można jawnie skonfigurować Aby pominąć i zalogować się niezgodne wiersze i kopiować tylko tych zgodne danych do utworzenia kopii zakończyło się pomyślnie. Zobacz [działanie kopiowania odporność na uszkodzenia](copy-activity-fault-tolerance.md) na więcej szczegółów.

## <a name="performance-and-tuning"></a>Wydajności i dostosowywanie

Zobacz [wydajności działania kopiowania i dostrajania przewodnik](copy-activity-performance.md), która opisuje kluczowych czynników wpływających na wydajność przepływu danych (działanie kopiowania) w fabryce danych Azure. Również zawiera listę obserwowana wydajność podczas testowania wewnętrznego oraz opisano różne sposoby optymalizacji wydajności działania kopiowania.

## <a name="next-steps"></a>Następne kroki
Zobacz następujące Przewodniki Szybki Start, samouczki i przykłady:

- [Kopiowanie danych z jednej lokalizacji do innej lokalizacji, w tym samym magazynie obiektów Blob Azure](quickstart-create-data-factory-dot-net.md)
- [Kopiowanie danych z magazynu obiektów Blob Azure do bazy danych SQL Azure](tutorial-copy-data-dot-net.md)
- [Kopiowanie danych z lokalnego programu SQL Server na platformie Azure](tutorial-hybrid-copy-powershell.md)
