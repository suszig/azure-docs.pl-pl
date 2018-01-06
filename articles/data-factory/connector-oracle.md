---
title: "Kopiowanie danych do/z programem Oracle przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skopiować dane sklepach obsługiwanej źródłowej do bazy danych programu Oracle (lub) z bazy danych Oracle sklepów zbiornika obsługiwane przy użyciu fabryki danych."
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
ms.date: 01/05/2018
ms.author: jingwang
ms.openlocfilehash: ff5dc0d2c5f744cb42da715713977fdc89a96edf
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="copy-data-from-and-to-oracle-using-azure-data-factory"></a>Kopiowanie danych z i do Oracle przy użyciu fabryki danych Azure
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-onprem-oracle-connector.md)
> * [Wersja 2 — wersja zapoznawcza](connector-oracle.md)

W tym artykule omówiono sposób użycia działanie kopiowania w fabryce danych Azure, aby skopiować dane z i do bazy danych programu Oracle. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [łącznika Oracle w wersji 1](v1/data-factory-onprem-oracle-connector.md).

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Możesz skopiować dane z bazy danych programu Oracle do żadnych obsługiwanych ujścia magazynu danych lub skopiować dane z dowolnego źródła obsługiwanych magazynu danych do bazy danych programu Oracle. Lista magazynów danych, które są obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

W szczególności ten łącznik Oracle obsługuje następujące wersje bazy danych Oracle i obsługuje uwierzytelnianie podstawowe lub identyfikator OID.

- R1 Oracle 12c (12.1)
- Oracle 11g R1, R2 (11.1, 11.2)
- Oracle 10g R1, R2 (10.1, 10.2)
- Oracle 9i R1, R2 (9.0.1, 9.2)
- Oracle 8i R3 (8.1.7)

## <a name="prerequisites"></a>Wymagania wstępne

Aby skopiować dane z/do bazy danych programu Oracle, który nie jest dostępny publicznie, należy skonfigurować środowisko uruchomieniowe Self-hosted integracji. Zobacz [środowiska uruchomieniowego integracji Self-hosted](create-self-hosted-integration-runtime.md) artykułu, aby uzyskać więcej informacji o integracji środowiska uruchomieniowego. Środowiska uruchomieniowego integracji oferuje wbudowane Sterownik Oracle, w związku z tym nie trzeba ręcznie zainstalowania sterownika podczas kopiowania danych z/na Oracle.

## <a name="getting-started"></a>Wprowadzenie
Można utworzyć potoku o aktywności kopiowania przy użyciu zestawu .NET SDK, zestaw SDK Python, programu Azure PowerShell, interfejsu API REST lub szablonu usługi Azure Resource Manager. Zobacz [samouczek działania kopiowania](quickstart-create-data-factory-dot-net.md) instrukcje krok po kroku utworzyć potok z działaniem kopiowania.

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek fabryki danych określonej do łącznika programu Oracle.

## <a name="linked-service-properties"></a>Połączona usługa właściwości

Obsługiwane są następujące właściwości dla Oracle połączone usługi:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **Oracle** | Yes |
| Parametry połączenia | Podaj informacje wymagane do nawiązania połączenia z wystąpieniem bazy danych programu Oracle. Zaznacz to pole jako SecureString.<br><br>**Obsługiwany typ połączenia**: mogą być używane **Oracle SID** lub **nazwa usługi Oracle** Aby zidentyfikować bazy danych:<br>— Za pomocą identyfikatora SID:`Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;`<br>-Przy użyciu nazwy usługi:`Host=<host>;Port=<port>;ServiceName=<sid>;User Id=<username>;Password=<password>;` | Yes |
| connectVia | [Integrację środowiska uruchomieniowego](concepts-integration-runtime.md) ma być używany do nawiązania połączenia z magazynem danych. (Jeśli w magazynie danych jest dostępny publicznie) można użyć środowiska uruchomieniowego integracji Self-hosted lub środowiska uruchomieniowego integracji Azure. Jeśli nie zostanie określony, używa domyślnej środowiska uruchomieniowego integracji Azure. |Nie |

**Przykład:**

```json
{
    "name": "OracleLinkedService",
    "properties": {
        "type": "Oracle",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "Host=<host>;Port=<port>;Sid=<sid>;User Id=<username>;Password=<password>;"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje zobacz artykuł zestawów danych. Ta sekcja zawiera listę właściwości obsługiwanych przez zestaw danych Oracle.

Aby skopiować dane z/do Oracle, ustaw właściwość Typ zestawu danych do **OracleTable**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type zestawu danych: **OracleTable** | Yes |
| tableName |Nazwa tabeli w bazie danych programu Oracle, odnoszący się do połączonej usługi. | Yes |

**Przykład:**

```json
{
    "name": "OracleDataset",
    "properties":
    {
        "type": "OracleTable",
        "linkedServiceName": {
            "referenceName": "<Oracle linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez Oracle źródłowy i odbiorczy właściwości.

### <a name="oracle-as-source"></a>Oracle jako źródło

Aby skopiować dane z bazy danych Oracle, należy ustawić typ źródła w przypadku działania kopiowania do **OracleSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type źródła działania kopiowania: **OracleSource** | Yes |
| oracleReaderQuery | Użyj niestandardowych zapytania SQL można odczytać danych. Na przykład: `"SELECT * FROM MyTable"`. | Nie |

Jeśli nie określisz "oracleReaderQuery" kolumny zdefiniowane w sekcji "structure" w zestawie danych są używane do utworzenia kwerendy (`select column1, column2 from mytable`) w celu uruchomienia bazy danych Oracle. Jeśli definicji zestawu danych nie ma "structure", wszystkie kolumny są wybierane w tabeli.

**Przykład:**

```json
"activities":[
    {
        "name": "CopyFromOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Oracle input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "OracleSource",
                "oracleReaderQuery": "SELECT * FROM MyTable"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="oracle-as-sink"></a>Oracle jako odbioru

Aby skopiować dane do bazy danych Oracle, należy ustawić typ ujścia w działaniu kopiowania do **OracleSink**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **zbiornika** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość typu sink działania kopiowania: **OracleSink** | Yes |
| writeBatchSize | Wstawia dane do tabeli SQL, gdy writeBatchSize osiągnie rozmiar buforu.<br/>Dozwolone wartości to: liczba całkowita (liczba wierszy). |Nie (domyślna to 10 000) |
| writeBatchTimeout | Czas na ukończenie zanim upłynie limit czasu operacji wstawiania wsadowego oczekiwania.<br/>Dozwolone wartości to: Timespan. Przykład: 00:30:00 (30 minut). | Nie |
| preCopyScript | Określ zapytanie SQL dla aktywności kopiowania do wykonania przed zapisaniem danych do bazy danych Oracle w każdym przebiegu. Ta właściwość służy do oczyszczania danych wstępnie załadowane. | Nie |

**Przykład:**

```json
"activities":[
    {
        "name": "CopyToOracle",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Oracle output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "OracleSink"
            }
        }
    }
]
```

## <a name="data-type-mapping-for-oracle"></a>Mapowanie dla programu Oracle typu danych

Podczas kopiowania danych z/na Oracle, następujące mapowania są używane z typów danych Oracle do typów danych tymczasowych fabryki danych Azure. Zobacz [schemat i dane typu mapowania](copy-activity-schema-and-type-mapping.md) Aby poznać sposób działania kopiowania mapowania typu źródłowego: schemat i dane sink.

| Typ danych Oracle | Typ danych tymczasowych fabryki danych |
|:--- |:--- |
| BPLIK |Byte] |
| OBIEKT BLOB |Byte]<br/>(obsługiwana tylko w bazie danych Oracle 10 GB/s i nowsze) |
| CHAR |Ciąg |
| CLOB |Ciąg |
| DATE |Data/godzina |
| FLOAT |Decimal, ciąg (jeśli precyzja > 28) |
| LICZBA CAŁKOWITA |Decimal, ciąg (jeśli precyzja > 28) |
| DŁUGA |Ciąg |
| LONG RAW |Byte] |
| NCHAR |Ciąg |
| NCLOB |Ciąg |
| NUMER |Decimal, ciąg (jeśli precyzja > 28) |
| NVARCHAR2 |Ciąg |
| NIEPRZETWORZONE |Byte] |
| ROWID |Ciąg |
| ZNACZNIK CZASU |Data/godzina |
| SYGNATURA CZASOWA Z LOKALNEJ STREFIE CZASOWEJ |Ciąg |
| SYGNATURA CZASOWA ZE STREFĄ CZASOWĄ |Ciąg |
| LICZBA CAŁKOWITA BEZ ZNAKU |Liczba |
| VARCHAR2 |Ciąg |
| XML |Ciąg |

> [!NOTE]
> Typ danych roku INTERWAŁ miesiąca i INTERWAŁ dzień do drugiego nie są obsługiwane.


## <a name="next-steps"></a>Kolejne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md##supported-data-stores-and-formats).