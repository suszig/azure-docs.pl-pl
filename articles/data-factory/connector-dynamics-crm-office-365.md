---
title: "Kopiowanie danych z i do usługi Dynamics CRM i 365 przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skopiować dane z programu Dynamics CRM i 365 do zbiornika obsługiwanych magazynów danych (lub) z obsługiwanych źródłowych baz danych do usługi Dynamics CRM i 365 za pomocą działania kopiowania w potoku fabryki danych Azure."
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
ms.date: 11/09/2017
ms.author: jingwang
ms.openlocfilehash: ec1b9868ca94392cd00875ef2913d4c14a608110
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="copy-data-fromto-dynamics-365dynamics-crm-using-azure-data-factory"></a>Kopiowanie danych z/do Dynamics 365 / Dynamics CRM przy użyciu fabryki danych Azure

W tym artykule omówiono sposób użycia działanie kopiowania w fabryce danych Azure, aby skopiować dane z i do Dynamics 365 / Dynamics CRM. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [działanie kopiowania w wersji 1](v1/data-factory-data-movement-activities.md).

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Kopiowanie danych z Dynamics 365 / Dynamics CRM żadnych obsługiwanych ujścia magazynu danych lub skopiować dane z dowolnego magazynu danych obsługiwanych źródłowych do Dynamics 365 / Dynamics CRM. Lista magazynów danych obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

Ten łącznik Dynamics obsługuje poniżej Dynamics wersji i typów uwierzytelniania (*IFD jest wdrożenie ukierunkowane Internet skrót*):

| Dynamics wersji | Typy uwierzytelniania | Przykłady połączonej usługi |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM online | Usługi Office 365 | [Dynamics Online + uwierzytelnianie usługi Office 365](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 lokalnej z IFD <br> Dynamics CRM 2016 na lokalnym IFD <br> Dynamics CRM 2015 lokalnej z IFD | IFD | [Dynamics lokalnej z IFD + IFD uwierzytelniania](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

Dynamics 365 w szczególności następujące typy aplikacji są obsługiwane:

- Dynamics 365 sprzedaży
- Dynamics 365 dla klientów usługi
- Dynamics 365 usługi pola
- Dynamics 365 projektu usługi automatyzacji
- Dynamics 365 Marketing

> [!NOTE]
> Używanie łącznika Dynamics przechowywania hasła w usłudze Azure Key Vault i umożliwić ściągania działaniach kopiowania stamtąd podczas wykonywania kopii danych. Zobacz, jak skonfigurować w [połączona usługa właściwości](#linked-service-properties) sekcji.

## <a name="getting-started"></a>Wprowadzenie

Można utworzyć potoku o aktywności kopiowania przy użyciu zestawu .NET SDK, zestaw SDK Python, programu Azure PowerShell, interfejsu API REST lub szablonu usługi Azure Resource Manager. Zobacz [samouczek działania kopiowania](quickstart-create-data-factory-dot-net.md) instrukcje krok po kroku utworzyć potok z działaniem kopiowania.

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek fabryki danych określonej do Dynamics.

## <a name="linked-service-properties"></a>Połączona usługa właściwości

Obsługiwane są następujące właściwości dla połączonej usługi Dynamics:

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 i Dynamics CRM Online

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **Dynamics**. | Tak |
| deploymentType | Typ wdrożenia wystąpienia programu Dynamics. Musi być **"Online"** dla usługi Dynamics Online. | Tak |
| Nazwa_organizacji | Nazwa organizacji Dynamics wystąpienia. | Nie, należy określić, jeśli istnieje kilka wystąpień Dynamics skojarzonych z użytkownikiem. |
| Typ authenticationType | Typ uwierzytelniania, aby połączyć się z serwerem programu Dynamics. Określ **"Usługi Office 365"** dla Dynamics w trybie Online. | Tak |
| nazwa użytkownika | Określ nazwę użytkownika, aby nawiązać połączenie Dynamics. | Tak |
| hasło | Określ hasło dla konta użytkownika, określone nazwy użytkownika. Konieczne umieszczanie hasła w usłudze Azure Key Vault i skonfigurować hasło jako "AzureKeyVaultSecret". Dowiedz się więcej o [przechowywania poświadczeń w magazynie kluczy](store-credentials-in-key-vault.md). | Tak |
| connectVia | [Integrację środowiska uruchomieniowego](concepts-integration-runtime.md) ma być używany do nawiązania połączenia z magazynem danych. Jeśli nie zostanie określony, używa domyślnej środowiska uruchomieniowego integracji Azure. | Brak źródła tak dla obiekt sink |

>[!IMPORTANT]
>Aby skopiować dane do programu Dynamics, jawnie [utworzyć IR Azure](create-azure-integration-runtime.md#create-azure-ir) z lokalizacją w pobliżu Dynamics i skojarz w połączonej usłudze jak w poniższym przykładzie.

**Przykład: Dynamics online przy użyciu uwierzytelniania usługi Office 365**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "organizationName": "orga02d9c75",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 i Dynamics CRM na lokalnym IFD

*Dodatkowe właściwości porównanie z Dyanmics online są "hosta" i "port".*

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **Dynamics**. | Tak |
| deploymentType | Typ wdrożenia wystąpienia programu Dynamics. Musi być **"OnPremisesWithIfd"** dla Dynamics lokalnego z IFD.| Tak |
| **Nazwa hosta** | Nazwa hosta serwera Dynamics lokalnego. | Tak |
| **Port** | Port serwera Dynamics lokalnego. | Nie, domyślną jest 443 |
| Nazwa_organizacji | Nazwa organizacji Dynamics wystąpienia. | Tak |
| Typ authenticationType | Typ uwierzytelniania, aby połączyć się z serwerem programu Dynamics. Określ **"Ifd"** dla Dynamics lokalnego z IFD. | Tak |
| nazwa użytkownika | Określ nazwę użytkownika, aby nawiązać połączenie Dynamics. | Tak |
| hasło | Określ hasło dla konta użytkownika, określone nazwy użytkownika. Należy pamiętać, że konieczne umieszczanie hasła w usłudze Azure Key Vault i skonfigurować hasło jako "AzureKeyVaultSecret". Dowiedz się więcej o [przechowywania poświadczeń w magazynie kluczy](store-credentials-in-key-vault.md). | Tak |
| connectVia | [Integrację środowiska uruchomieniowego](concepts-integration-runtime.md) ma być używany do nawiązania połączenia z magazynem danych. Jeśli nie zostanie określony, używa domyślnej środowiska uruchomieniowego integracji Azure. | Brak źródła tak dla obiekt sink |

>[!IMPORTANT]
>Aby skopiować dane do programu Dynamics, jawnie [utworzyć IR Azure](create-azure-integration-runtime.md#create-azure-ir) z lokalizacją w pobliżu Dynamics i kojarzenie w połączonej usłudze jako poniższym przykładzie.

**Przykład: Dynamics na lokalnym przy użyciu uwierzytelniania IFD IFD**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
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

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje, zobacz [zestawów danych](concepts-datasets-linked-services.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez zestaw danych Dynamics właściwości.

Aby skopiować dane z i do programu Dynamics, ustaw właściwość Typ zestawu danych do **DynamicsEntity**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type zestawu danych: **DynamicsEntity** |Tak |
| Nazwa jednostki | Nazwa logiczna obiektu do pobrania. | Brak źródła (Jeśli określono wartość "query" w źródle działania), tak dla obiekt sink |

> [!IMPORTANT]
>- **Podczas kopiowania danych z programu Dynamics, sekcja "structure" jest wymagana** w zestawie danych Dynamics, który definiuje typ danych kolumny Dynamics dane, które chcesz skopiować. Dowiedz się więcej o [struktury zestawu danych](concepts-datasets-linked-services.md#dataset-structure) i [mapowanie typu danych dla programu Dynamics](#data-type-mapping-for-dynamics).
>- **Podczas kopiowania danych do programu Dynamics, w sekcji "structure" jest opcjonalna** w zestawie danych Dynamics. Kolumny, która ma zostać skopiowany do będzie określana przez schematu źródła danych. Jeśli źródłem jest plik CSV bez nagłówka, w zestawie danych wejściowych, określ "structure" z typu danych kolumny, który mapuje do pól w pliku CSV jedna po drugiej w porządku.

**Przykład:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typePoperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez Dynamics źródłowy i odbiorczy właściwości.

### <a name="dynamics-as-source"></a>Dynamics jako źródło

Aby skopiować dane z programu Dynamics, należy ustawić typ źródła w przypadku działania kopiowania do **DynamicsSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type źródła działania kopiowania: **DynamicsSource**  | Tak |
| query  | FetchXML jest język kwerendy zastrzeżonych, który jest używany w programie Microsoft Dynamics (online & lokalnych). Zobacz w poniższym przykładzie i Dowiedz się więcej o [tworzenia zapytań dotyczących FeachXML](https://msdn.microsoft.com/en-us/library/gg328332.aspx). | Nie (Jeśli określono parametr "Nazwa" w zestawie danych)  |

**Przykład:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>Przykładowe zapytanie FetchXML

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

### <a name="dynamics-as-sink"></a>Dynamics jako odbioru

Aby skopiować dane do programu Dynamics, należy ustawić typ ujścia w działaniu kopiowania do **DynamicsSink**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **zbiornika** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość typu sink działania kopiowania: **DynamicsSink**  | Tak |
| WriteBehavior | Zachowanie zapisu operacji.<br/>Dozwolone wartości to: **"Upsert"**. | Tak |
| writeBatchSize | Liczba wierszy z danymi zapisywanymi w Dynamics w każdej z partii. | Nie (wartość domyślna to 10) |
| ignoreNullValues | Wskazuje, czy mają zostać zignorowane wartości null w danych wejściowych (z wyjątkiem pól klucza) podczas operacji zapisu.<br/>Dozwolone wartości to: **true**, i **false**.<br>— wartość true: pozostaw danych w miejscu docelowym obiekt bez zmian podczas wykonywania operacji upsert/aktualizowania i wstawiania zdefiniowane wartości domyślnej, podczas wykonywania operacji wstawiania.<br/>— wartość false: aktualizować dane w obiekcie docelowym na wartość NULL, podczas wykonywania operacji upsert/aktualizacji i wstawić wartości NULL, podczas wykonywania operacji wstawiania.  | Nie (wartość domyślna to false) |

>[!NOTE]
>Wartość domyślna zbiornika writeBatchSize i skopiuj działania [parallelCopies](copy-activity-performance.md#parallel-copy) dla obiekt sink Dynamics są obie 10, co oznacza 100 rekordów przekazaniu Dynamics jednocześnie.

**Przykład:**

```json
"activities":[
    {
        "name": "CopyToDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Dynamics output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "DynamicsSink",
                "writeBehavior": "Upsert",
                "writeBatchSize": 10,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="data-type-mapping-for-dynamics"></a>Mapowanie do programu Dynamics typu danych

Podczas kopiowania danych z programu Dynamics, następujące mapowania są używane z Dynamics typów danych do typów danych tymczasowych fabryki danych Azure. Zobacz [schemat i dane typu mapowania](copy-activity-schema-and-type-mapping.md) Aby poznać sposób działania kopiowania mapowania typu źródłowego: schemat i dane sink.

Skonfiguruj odpowiedni typ danych fabryki danych w oparciu o Twoje źródło danych Dynamics strukturze zestawu danych typu przy użyciu poniższej tabeli mapowania:

| Dynamics — typ danych | Typ danych tymczasowych fabryki danych | Obsługiwane jako źródło | Obsługiwane jako odbioru |
|:--- |:--- |:--- |:--- |
| AttributeTypeCode.BigInt | Długie | ✓ | ✓ |
| AttributeTypeCode.Boolean | Wartość logiczna | ✓ | ✓ |
| AttributeType.Customer | Identyfikator GUID | ✓ |  |
| AttributeType.DateTime | Data/godzina | ✓ | ✓ |
| AttributeType.Decimal | Decimal | ✓ | ✓ |
| AttributeType.Double | O podwójnej precyzji | ✓ | ✓ |
| AttributeType.EntityName | Ciąg | ✓ | ✓ |
| AttributeType.Integer | Int32 | ✓ | ✓ |
| AttributeType.Lookup | Identyfikator GUID | ✓ |  |
| AttributeType.ManagedProperty | Wartość logiczna | ✓ |  |
| AttributeType.Memo | Ciąg | ✓ | ✓ |
| AttributeType.Money | Decimal | ✓ |  |
| AttributeType.Owner | Identyfikator GUID | ✓ | |
| AttributeType.Picklist | Int32 | ✓ | ✓ |
| AttributeType.Uniqueidentifier | Identyfikator GUID | ✓ | ✓ |
| AttributeType.String | Ciąg | ✓ | ✓ |
| AttributeType.State | Int32 | ✓ |  |
| AttributeType.Status | Int32 | ✓ |  |


> [!NOTE]
> Typ danych Dynamics AttributeType.CalendarRules i AttributeType.PartyList nie są obsługiwane.

## <a name="next-steps"></a>Następne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats).