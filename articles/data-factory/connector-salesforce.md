---
title: "Kopiowanie danych z i do usług Salesforce przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skopiować dane z Salesforce do zbiornika obsługiwanych magazynów danych (lub) obsługiwanej źródłowej magazyny danych do usługi Salesforce za pomocą działania kopiowania w potoku fabryki danych Azure."
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
ms.openlocfilehash: 7cd86922b0445fc81766ca54080e2fd3e64a6c61
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/11/2018
---
# <a name="copy-data-fromto-salesforce-using-azure-data-factory"></a>Kopiowanie danych z i do usług Salesforce przy użyciu fabryki danych Azure
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-salesforce-connector.md)
> * [Wersja 2 — wersja zapoznawcza](connector-salesforce.md)

W tym artykule omówiono sposób użycia działanie kopiowania w fabryce danych Azure, aby skopiować dane z i do usług Salesforce. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [łącznika usług Salesforce w wersji 1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Można skopiować dane z usługi Salesforce do dowolnego magazynu danych obsługiwanych zbiornika lub lub skopiować dane z dowolnego źródła obsługiwanych magazynu danych do usługi Salesforce. Lista magazynów danych, które są obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

W szczególności tego łącznika usług Salesforce obsługuje:

- Następujące wersje usług Salesforce: **Developer Edition w wersji Professional, Enterprise Edition albo nieograniczone Edition**.
- Kopiowanie danych z i do usług Salesforce **produkcji, piaskownicy i domeny niestandardowej**.

## <a name="prerequisites"></a>Wymagania wstępne

* Uprawnienia interfejsu API musi być włączony w usłudze Salesforce. Zobacz [jak włączyć dostęp do interfejsu API w usłudze Salesforce przez zestaw uprawnień?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Limity żądań usług SalesForce

Usługa SalesForce obejmuje limity dla całkowita liczba żądań interfejsu API i równoczesnych żądań interfejsu API. Pamiętaj o następujących kwestiach:

- Jeśli liczba jednoczesnych żądań przekracza limit, ograniczanie występuje i zobacz losowe awarie.
- Jeśli całkowita liczba żądań przekracza limit, konta usług Salesforce jest zablokowany przez 24 godziny.

W obu przypadkach może być również wyświetlony błąd "REQUEST_LIMIT_EXCEEDED". Zobacz sekcję "Limity żądań interfejsu API" w [Salesforce Developer limity](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artykułu, aby uzyskać szczegółowe informacje.

## <a name="getting-started"></a>Wprowadzenie

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek fabryki danych określonej do łącznika usług Salesforce.

## <a name="linked-service-properties"></a>Połączona usługa właściwości

Obsługiwane są następujące właściwości usługi Salesforce połączone:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type |Właściwość type musi mieć ustawioną: **Salesforce**. |Yes |
| environmentUrl | Określ wystąpienie adres URL usługi Salesforce. <br> -Domyślna to `"https://login.salesforce.com"`. <br> -Aby skopiować dane z piaskownicy, określ `"https://test.salesforce.com"`. <br> -Aby skopiować dane z domeny niestandardowej, na przykład określić, `"https://[domain].my.salesforce.com"`. |Nie |
| nazwa użytkownika |Określ nazwę użytkownika dla konta użytkownika. |Yes |
| hasło |Określ hasło dla konta użytkownika.<br/><br/>Można wybrać Oznacz to pole jako SecureString Zapisz w bezpiecznej lokalizacji w ADF lub przechowywania haseł w usłudze Azure Key Vault i umożliwić działaniach kopiowania ściągnięcia stamtąd podczas wykonywania kopii danych — Dowiedz się więcej o [przechowywania poświadczeń w magazynie kluczy](store-credentials-in-key-vault.md). |Yes |
| securityToken |Określ tokenu zabezpieczającego dla konta użytkownika. Zobacz [uzyskać token zabezpieczeń](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) instrukcje dotyczące resetowania/Get tokenu zabezpieczającego. Aby dowiedzieć się więcej o tokeny zabezpieczające ogólnie rzecz biorąc, zobacz [zabezpieczeń i interfejsu API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm).<br/><br/>Można wybrać Oznacz to pole jako SecureString Zapisz w bezpiecznej lokalizacji w ADF lub przechowywać tokenu zabezpieczającego w usłudze Azure Key Vault i umożliwić działaniach kopiowania ściągnięcia stamtąd podczas wykonywania kopii danych — Dowiedz się więcej o [przechowywania poświadczeń w magazynie kluczy](store-credentials-in-key-vault.md). |Yes |
| connectVia | [Integrację środowiska uruchomieniowego](concepts-integration-runtime.md) ma być używany do nawiązania połączenia z magazynem danych. Jeśli nie zostanie określony, używa domyślnej środowiska uruchomieniowego integracji Azure. | Nie dla źródła tak dla obiekt sink Jeśli źródło jest połączona usługa nie ma IR. |

>[!IMPORTANT]
>Podczas kopiowania danych **do** Salesforce, domyślnego środowiska uruchomieniowego integracji Azure nie może służyć do wykonywania kopii. W innym słowie, jeśli połączone źródła usługa nie ma określonej IR, jawnie [utworzyć IR Azure](create-azure-integration-runtime.md#create-azure-ir) z lokalizacją w pobliżu Salesforce i kojarzenie w usług Salesforce połączonej usługi, jak w poniższym przykładzie.

**Przykład: przechowywanie poświadczeń w ADF**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            },
            "securityToken": {
                "type": "SecureString",
                "value": "<security token>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Przykład: przechowywania poświadczeń w usłudze Azure Key Vault**

```json
{
    "name": "SalesforceLinkedService",
    "properties": {
        "type": "Salesforce",
        "typeProperties": {
            "username": "<username>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of password in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            },
            "securityToken": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name of security token in AKV>",
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

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje, zobacz [zestawów danych](concepts-datasets-linked-services.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez zestaw danych Salesforce.

Aby skopiować dane z i do usług Salesforce, ustaw właściwość Typ zestawu danych do **SalesforceObject**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **SalesforceObject**  | Yes |
| objectApiName | Nazwa obiektu Salesforce można pobrać danych z. | Brak źródła tak dla obiekt sink |

> [!IMPORTANT]
> Część "__c" Nazwa interfejsu API jest wymagany dla dowolnych niestandardowych obiektów.

![Nazwa danych fabryki — połączenie usługi Salesforce — interfejs API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Przykład:**

```json
{
    "name": "SalesforceDataset",
    "properties": {
        "type": "SalesforceObject",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "objectApiName": "MyTable__c"
        }
    }
}
```

>[!NOTE]
>Do tyłu zgodności, podczas kopiowania danych z usług Salesforce za pomocą poprzedniego zestawu danych typu "RelationalTable" będzie kontynuować pracę, gdy są zalecane, aby przełączyć się do nowego typu "SalesforceObject".

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type zestawu danych: **RelationalTable** | Yes |
| tableName | Nazwa tabeli w usłudze Salesforce. | Nie (Jeśli określono parametr "zapytania" w źródle działania) |

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez usługi Salesforce źródłowy i odbiorczy właściwości.

### <a name="salesforce-as-source"></a>SalesForce jako źródło

Aby skopiować dane z witryny Salesforce, Ustaw typ źródła w przypadku działania kopiowania do **SalesforceSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type źródła działania kopiowania: **SalesforceSource** | Yes |
| query |Użyj niestandardowych zapytania można odczytać danych. Można użyć zapytania SQL 92 lub [Salesforce obiektu Query Language (SOQL)](https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql.htm) zapytania. Na przykład: `select * from MyTable__c`. | Nie (Jeśli określono parametr "Nazwa_tabeli" w zestawie danych) |

> [!IMPORTANT]
> Część "__c" Nazwa interfejsu API jest wymagany dla dowolnych niestandardowych obiektów.

![Nazwa danych fabryki — połączenie usługi Salesforce — interfejs API](media/copy-data-from-salesforce/data-factory-salesforce-api-name-2.png)

**Przykład:**

```json
"activities":[
    {
        "name": "CopyFromSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Salesforce input dataset name>",
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
                "type": "SalesforceSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

>[!NOTE]
>Do tyłu zgodności, podczas kopiowania danych z usług Salesforce przy użyciu poprzedniej źródła kopii typu "RelationalSource" będzie kontynuować pracę, gdy są zalecane, aby przełączyć się do nowego typu "SalesforceSource".

### <a name="salesforce-as-sink"></a>SalesForce jako odbioru

Aby skopiować dane do usługi Salesforce, należy ustawić typ ujścia w działaniu kopiowania do **SalesforceSink**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **zbiornika** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość typu sink działania kopiowania: **SalesforceSink** | Yes |
| WriteBehavior | Zachowanie zapisu dla tej operacji.<br/>Dozwolone wartości to: **Wstaw**, i **Upsert**. | Nie (wartość domyślna to Insert) |
| externalIdFieldName | Nazwa pola identyfikator zewnętrznego dla operacji upsert. Określone pole musi być zdefiniowany jako "Zewnętrzne pole Id" w obiekcie Salesforce i go nie może mieć wartości NULL w odpowiednich danych wejściowych. | Tak, aby "Upsert" |
| writeBatchSize | Liczba wierszy z danymi zapisywanymi w Salesforce w każdej z partii. | Nie (wartość domyślna to 5000) |
| ignoreNullValues | Wskazuje, czy zignorowanie wartości null z danych wejściowych podczas zapisu danych operacji.<br/>Dozwolone wartości to: **true**, i **false**.<br>- **wartość true,**: pozostaw danych w miejscu docelowym obiekt bez zmian podczas wykonywania operacji upsert/aktualizowania i wstawiania zdefiniowane wartości domyślnej, podczas wykonywania operacji wstawiania.<br/>- **FALSE**: aktualizować dane w obiekcie docelowym na wartość NULL, podczas wykonywania operacji upsert/aktualizacji i wstawić wartości NULL, podczas wykonywania operacji wstawiania. | Nie (wartość domyślna to false) |

### <a name="example-salesforce-sink-in-copy-activity"></a>Przykład: W przypadku działania kopiowania obiektu sink usług Salesforce

```json
"activities":[
    {
        "name": "CopyToSalesforce",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<Salesforce output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "<source type>"
            },
            "sink": {
                "type": "SalesforceSink",
                "writeBehavior": "Upsert",
                "externalIdFieldName": "CustomerId__c",
                "writeBatchSize": 10000,
                "ignoreNullValues": true
            }
        }
    }
]
```

## <a name="query-tips"></a>Wskazówki zapytania

### <a name="retrieving-data-from-salesforce-report"></a>Pobieranie danych z raportu usług Salesforce

Można pobrać dane z raportów usług Salesforce, określając kwerendy w postaci `{call "<report name>"}`. Przykład: `"query": "{call \"TestReport\"}"`.

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Przywracanie usuniętych rekordów z Kosza usług Salesforce

Kwerenda nietrwałego usuniętych rekordów z Kosza usług Salesforce, można określić **"IsDeleted = 1"** w zapytaniu. Na przykład:

* Aby odpytać tylko usunięte rekordy, określ "Wybierz * z MyTable__c **gdzie IsDeleted = 1**"
* Aby wysyłać zapytania o wszystkie rekordy tym usuniętych i istniejących, określ "Wybierz * z MyTable__c **gdzie IsDeleted = 0 lub IsDeleted = 1**"

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Podczas pobierania danych przy użyciu where klauzuli według kolumny daty i godziny

Gdy Określ SOQL lub SQL zapytanie, zwrócić uwagę różnica format daty/godziny. Na przykład:

* **Przykładowe SOQL**:`SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= @{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-ddTHH:mm:ssZ')} AND LastModifiedDate < @{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-ddTHH:mm:ssZ')}`
* **Przykładowe SQL**:`SELECT * FROM Account WHERE LastModifiedDate >= {ts'@{formatDateTime(pipeline().parameters.StartTime,'yyyy-MM-dd HH:mm:ss')}'} AND LastModifiedDate < {ts'@{formatDateTime(pipeline().parameters.EndTime,'yyyy-MM-dd HH:mm:ss')}'}"`

## <a name="data-type-mapping-for-salesforce"></a>Mapowanie dla usług Salesforce typu danych

Podczas kopiowania danych z usług Salesforce, z typów danych Salesforce są używane następujące mapowania do typów danych tymczasowych fabryki danych Azure. Zobacz [schemat i dane typu mapowania](copy-activity-schema-and-type-mapping.md) Aby poznać sposób działania kopiowania mapowania typu źródłowego: schemat i dane sink.

| Typ danych witryny SalesForce | Typ danych tymczasowych fabryki danych |
|:--- |:--- |
| Automatyczny numer |Ciąg |
| Pole wyboru |Wartość logiczna |
| Waluta |O podwójnej precyzji |
| Date |Data/godzina |
| Data/godzina |Data/godzina |
| Adres e-mail |Ciąg |
| Identyfikator |Ciąg |
| Relacja wyszukiwania |Ciąg |
| Lista wyboru wielokrotnego wyboru |Ciąg |
| Liczba |O podwójnej precyzji |
| Procent |O podwójnej precyzji |
| Numer telefonu |Ciąg |
| Lista wyboru |Ciąg |
| Tekst |Ciąg |
| Obszar tekstu |Ciąg |
| Obszar tekstu (Liczba długa) |Ciąg |
| Obszar tekstu (zaawansowana) |Ciąg |
| Tekst (zaszyfrowane) |Ciąg |
| Adres URL |Ciąg |

## <a name="next-steps"></a>Kolejne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats).