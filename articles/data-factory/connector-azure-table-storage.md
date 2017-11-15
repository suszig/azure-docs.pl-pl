---
title: "Kopiowanie danych do/z magazynem tabel Azure przy użyciu fabryki danych | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skopiować dane z magazynów obsługiwanych źródłowych do magazynu tabel platformy Azure (lub) z tabeli magazynu sklepów zbiornika obsługiwane przy użyciu fabryki danych."
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
ms.date: 11/07/2017
ms.author: jingwang
ms.openlocfilehash: ca5f8e43b6667aa1c2e3ac38e7ea00b5bd86b72f
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/13/2017
---
# <a name="copy-data-to-or-from-azure-table-using-azure-data-factory"></a>Kopiowanie danych do i z tabel Azure przy użyciu fabryki danych Azure
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-azure-table-connector.md)
> * [Wersja 2 — wersja zapoznawcza](connector-azure-table-storage.md)

W tym artykule omówiono sposób używania działania kopiowania w fabryce danych Azure można skopiować danych do i z tabel Azure. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [łącznika magazynu tabel platformy Azure w wersji 1](v1/data-factory-azure-table-connector.md).

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Kopiowanie danych z dowolnego źródła obsługiwanych magazynu danych do tabeli platformy Azure lub skopiować dane z tabel Azure do żadnych obsługiwanych ujścia magazynu danych. Lista magazynów danych, które są obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

W szczególności ten łącznik tabel Azure obsługuje kopiowanie danych przy użyciu zarówno **klucz konta** i **sygnatury dostępu Współdzielonego usługi** uwierzytelnienia (Shared Access Signature).

## <a name="get-started"></a>Rozpoczęcie pracy
Można utworzyć potoku o aktywności kopiowania przy użyciu zestawu .NET SDK, zestaw SDK Python, programu Azure PowerShell, interfejsu API REST lub szablonu usługi Azure Resource Manager. Zobacz [samouczek działania kopiowania](quickstart-create-data-factory-dot-net.md) instrukcje krok po kroku utworzyć potok z działaniem kopiowania.

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek fabryki danych określonej do magazynu tabel platformy Azure.

## <a name="linked-service-properties"></a>Połączona usługa właściwości

### <a name="using-account-key"></a>Przy użyciu klucza konta

Połączoną usługą magazynu Azure można utworzyć przy użyciu klucza konta, co zapewnia fabryka danych z globalnego dostępu do magazynu Azure. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **AzureStorage** |Tak |
| Parametry połączenia | Podaj informacje wymagane do połączenia z magazynem platformy Azure dla właściwości connectionString. Zaznacz to pole jako SecureString. |Tak |
| connectVia | [Integrację środowiska uruchomieniowego](concepts-integration-runtime.md) ma być używany do nawiązania połączenia z magazynem danych. (Jeśli w magazynie danych znajduje się w sieci prywatnej), można użyć środowiska uruchomieniowego integracji Azure lub Self-hosted integracji w czasie wykonywania. Jeśli nie zostanie określony, używa domyślnej środowiska uruchomieniowego integracji Azure. |Nie |

**Przykład:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "type": "SecureString",
                "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-service-sas-authentication"></a>Przy użyciu sygnatury dostępu Współdzielonego usługi uwierzytelniania

Można również utworzyć połączoną usługą magazynu Azure, używając dostępu sygnatury dostępu Współdzielonego, który zapewnia usłudze fabryka danych z ograniczonej/czas-powiązane z dostępem do określonego/wszystkie zasoby w magazynie.

Dostęp do sygnatury dostępu Współdzielonego umożliwiają dostęp delegowany do zasobów na koncie magazynu. Umożliwia przyznanie klienta ograniczone uprawnienia do obiektów na koncie magazynu w określonym przedziale czasu i z określonym zestawem uprawnień, bez konieczności udostępniania kluczy dostępu konta. Sygnatury dostępu Współdzielonego to identyfikator URI, który obejmuje w jego parametrów zapytania, wszystkie informacje niezbędne do uwierzytelniony dostęp do zasobów magazynu. Aby uzyskać dostęp do zasobów magazynu przy użyciu sygnatury dostępu Współdzielonego, klient musi tylko Przekaż sygnatury dostępu Współdzielonego do odpowiedniego konstruktora lub metody. Aby uzyskać szczegółowe informacje na temat sygnatury dostępu Współdzielonego, zobacz [sygnatury dostępu współdzielonego: opis modelu sygnatur dostępu Współdzielonego](../storage/common/storage-dotnet-shared-access-signature-part-1.md)

> [!IMPORTANT]
> Azure obsługuje teraz tylko w fabryce danych **sygnatury dostępu Współdzielonego usługi** , ale nie SAS konta. Zobacz [typy z sygnatury dostępu współdzielonego](../storage/common/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures) szczegóły dotyczące tych dwóch typów oraz sposobu tworzenia. Adres URL SAS generable z portalu Azure lub Eksploratora usługi Storage jest SAS konta, który nie jest obsługiwany.
>

Aby używać uwierzytelniania sygnatury dostępu Współdzielonego usługi, obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **AzureStorage** |Tak |
| sasUri | Określ udostępniony URI sygnatury dostępu do zasobów usługi Azure Storage, takich jak obiektów blob, kontenera lub tabeli. Zaznacz to pole jako SecureString. |Tak |
| connectVia | [Integrację środowiska uruchomieniowego](concepts-integration-runtime.md) ma być używany do nawiązania połączenia z magazynem danych. (Jeśli w magazynie danych znajduje się w sieci prywatnej), można użyć środowiska uruchomieniowego integracji Azure lub Self-hosted integracji w czasie wykonywania. Jeśli nie zostanie określony, używa domyślnej środowiska uruchomieniowego integracji Azure. |Nie |

**Przykład:**

```json
{
    "name": "AzureStorageLinkedService",
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "sasUri": {
                "type": "SecureString",
                "value": "<SAS URI of the Azure Storage resource>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

Podczas tworzenia **identyfikatora URI połączenia SAS**, biorąc pod uwagę następujące kwestie:

- Ustaw odpowiednie odczytu/zapisu **uprawnienia** na obiektach w oparciu używania połączonej usługi (Odczyt, zapis, Odczyt/zapis) w fabryce danych.
- Ustaw **czas wygaśnięcia** odpowiednio. Upewnij się, że dostęp do obiektów usługi Azure Storage nie wygasa w aktywnym okresie potoku.
- Identyfikator URI utworzony na poziomie tabeli po prawej, oparte na potrzeby.

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje zobacz artykuł zestawów danych. Ta sekcja zawiera listę właściwości obsługiwanych przez zestaw danych z tabel Azure.

Aby skopiować dane do/z tabel Azure, ustaw właściwość Typ zestawu danych do **AzureTable**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type zestawu danych: **AzureTable** |Tak |
| tableName |Nazwa tabeli w wystąpieniu bazy danych w tabeli platformy Azure, odnoszący się do połączonej usługi. |Tak |

**Przykład:**

```json
{
    "name": "AzureTableDataset",
    "properties":
    {
        "type": "AzureTable",
        "linkedServiceName": {
            "referenceName": "<Azure Storage linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable"
        }
    }
}
```

### <a name="schema-by-data-factory"></a>Schemat fabryka danych

Dla magazynów danych bez schematu, takie jak tabel Azure usługi fabryka danych z wnioskuje schemat w jednym z następujących sposobów:

1. Jeśli określisz struktury danych za pomocą **struktury** tej struktury Schema honoruje właściwości w definicji zestawu danych, usługi fabryka danych. W tym przypadku jeśli wiersza nie zawiera wartości dla kolumny, wartość null podano dla niego.
2. Jeśli nie określisz struktury danych za pomocą **struktury** właściwości w definicji zestawu danych, fabryki danych wnioskuje schemat za pomocą pierwszego wiersza w danych. W takim przypadku jeśli pierwszy wiersz zawiera pełną schematu, niektóre kolumny zostaną pominięte w wyniku operacji kopiowania.

W związku z tym dla źródeł danych bez schematu, najlepszym rozwiązaniem jest zdefiniowanie struktury danych przy użyciu **struktury** właściwości.

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez tabel Azure źródłowy i odbiorczy właściwości.

### <a name="azure-table-as-source"></a>Tabeli platformy Azure jako źródła

Aby skopiować dane z tabel Azure, należy ustawić typ źródła w przypadku działania kopiowania do **AzureTableSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type źródła działania kopiowania: **AzureTableSource** |Tak |
| azureTableSourceQuery |Użyj zapytania niestandardowe tabeli platformy Azure, aby odczytać danych. Przykłady w następnej sekcji. |Nie |
| azureTableSourceIgnoreTableNotFound |Wskazuje, czy swallow wyjątek tabela nie istnieje.<br/>Dozwolone wartości to: **True**, i **False** (ustawienie domyślne). |Nie |

### <a name="azuretablesourcequery-examples"></a>Przykłady azureTableSourceQuery

W przypadku tabel Azure kolumny typu string:

```json
"azureTableSourceQuery": "$$Text.Format('PartitionKey ge \\'{0:yyyyMMddHH00_0000}\\' and PartitionKey le \\'{0:yyyyMMddHH00_9999}\\'', <datetime parameter>)"
```

W przypadku tabel Azure kolumny typu Data/Godzina:

```json
"azureTableSourceQuery": "$$Text.Format('DeploymentEndTime gt datetime\\'{0:yyyy-MM-ddTHH:mm:ssZ}\\' and DeploymentEndTime le datetime\\'{1:yyyy-MM-ddTHH:mm:ssZ}\\'', <datetime parameter>, <datetime parameter>)"
```

### <a name="azure-table-as-sink"></a>Tabeli platformy Azure jako odbioru

Aby skopiować dane do tabeli platformy Azure, należy ustawić typ ujścia w działaniu kopiowania do **AzureTableSink**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **zbiornika** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość typu sink działania kopiowania: **AzureTableSink** |Tak |
| azureTableDefaultPartitionKeyValue |Domyślna wartość klucza partycji, które mogą być używane przez obiekt sink. |Nie |
| azureTablePartitionKeyName |Określ nazwę kolumny, których wartości są używane jako klucze partycji. Jeśli nie zostanie określony, "AzureTableDefaultPartitionKeyValue" jest używana jako klucza partycji. |Nie |
| azureTableRowKeyName |Określ nazwę kolumny, których wartości kolumn używanych jako klucz wiersza. Jeśli nie zostanie określony, użyj identyfikatora GUID dla każdego wiersza. |Nie |
| azureTableInsertType |Tryb do wstawiania danych do tabeli platformy Azure. Ta właściwość określa, czy wartości zastąpienia lub scalić zostać istniejących wierszy w tabeli wyników ze zgodnymi kluczami partycji i wiersza. <br/><br/>Dozwolone wartości to: **scalania** (ustawienie domyślne) i **Zastąp**. <br/><br> To ustawienie jest stosowane na poziomie wiersza, a nie na poziomie tabeli, a żadna z tych opcji usuwa wiersze w tabeli danych wyjściowych, które nie istnieją w danych wejściowych. Aby dowiedzieć się więcej na temat działania tych ustawień (scalania i Zastąp), zobacz [wstawienia lub scalania jednostki](https://msdn.microsoft.com/library/azure/hh452241.aspx) i [wstawienia lub Zastąp jednostki](https://msdn.microsoft.com/library/azure/hh452242.aspx) tematów. |Nie |
| writeBatchSize |Wstawia dane do tabeli platformy Azure, gdy zostaje trafiony writeBatchSize lub writeBatchTimeout.<br/>Dozwolone wartości to: liczba całkowita (liczba wierszy) |Nie (domyślna to 10 000) |
| writeBatchTimeout |Wstawia dane do tabeli platformy Azure, gdy zostaje trafiony writeBatchSize lub writeBatchTimeout.<br/>Dozwolone wartości to: timespan. Przykład: "00:20:00" (20 minut) |Nie (wartość domyślna wynosi 90 s - magazynu klienta domyślny limit czasu) |

**Przykład:**

```json
"activities":[
    {
        "name": "CopyToAzureTable",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Azure Table output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "AzureTableSink",
                "azureTablePartitionKeyName": "<column name>",
                "azureTableRowKeyName": "<column name>"
            }
        }
    }
]
```

### <a name="azuretablepartitionkeyname"></a>azureTablePartitionKeyName

Mapowanie kolumny źródłowej do przy użyciu właściwości "translator", zanim będzie możliwe użycie kolumna docelowa jako azureTablePartitionKeyName kolumna docelowa.

W poniższym przykładzie kolumna źródłowa DivisionID jest zamapowany na kolumny docelowej DivisionID.

```json
"translator": {
    "type": "TabularTranslator",
    "columnMappings": "DivisionID: DivisionID, FirstName: FirstName, LastName: LastName"
}
```

"DivisionID" jest określony jako klucza partycji.

```json
"sink": {
    "type": "AzureTableSink",
    "azureTablePartitionKeyName": "DivisionID"
}
```

## <a name="data-type-mapping-for-azure-table"></a>Mapowanie typu danych dla tabeli platformy Azure

Podczas kopiowania danych z/do tabel Azure, następujące mapowania są używane z tabel Azure typów danych do typów danych tymczasowych fabryki danych Azure. Zobacz [schemat i dane typu mapowania](copy-activity-schema-and-type-mapping.md) Aby poznać sposób działania kopiowania mapowania typu źródłowego: schemat i dane sink.

Podczas przenoszenia danych do i z tabel Azure, następujące [mapowania zdefiniowane przez usługę Azure tabeli](https://msdn.microsoft.com/library/azure/dd179338.aspx) są używane z typów OData tabeli platformy Azure na typ architektury .NET i na odwrót.

| Typ danych tabeli platformy Azure | Typ danych tymczasowych fabryki danych | Szczegóły |
|:--- |:--- |:--- |
| Edm.Binary |Byte] |Tablica bajtów do 64 KB. |
| Edm.Boolean |wartość logiczna |Wartość logiczna. |
| Edm.DateTime |Data/godzina |Wartość 64-bitowa, wyrażone jako uniwersalny czas koordynowany (UTC). Obsługiwanym zakresem DateTime zaczyna się od 12:00, a 1 stycznia, 1601 r. N.E. (R), CZAS UTC. Zakres kończy się po 31 grudnia 9999 r. |
| Edm.Double |O podwójnej precyzji |64-bitowej zmiennej punktu wartości. |
| Edm.Guid |Identyfikator GUID |Globalnie unikatowy identyfikator 128-bitowego. |
| Edm.Int32 |Int32 |32-bitową liczbę całkowitą. |
| Edm.Int64 |Int64 |64-bitową liczbę całkowitą. |
| Edm.String |Ciąg |Wartość algorytmem UTF-16. Ciągi może być maksymalnie 64 KB. |

## <a name="next-steps"></a>Następne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats).