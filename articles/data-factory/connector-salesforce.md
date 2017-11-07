---
title: "Kopiowanie danych z usług Salesforce przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skopiować dane z usługi Salesforce do zbiornika obsługiwane magazyny danych za pomocą działania kopiowania w potoku fabryki danych Azure."
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
ms.date: 08/30/2017
ms.author: jingwang
ms.openlocfilehash: 7978e955bf5516a853443555ab10a69dcf22d63f
ms.sourcegitcommit: 38c9176c0c967dd641d3a87d1f9ae53636cf8260
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="copy-data-from-salesforce-using-azure-data-factory"></a>Kopiowanie danych z usług Salesforce przy użyciu fabryki danych Azure
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Wersja 1 — ogólnie dostępna](v1/data-factory-salesforce-connector.md)
> * [Wersja 2 — wersja zapoznawcza](connector-salesforce.md)

W tym artykule omówiono sposób użycia działanie kopiowania w fabryce danych Azure, aby skopiować dane z bazy danych usług Salesforce. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [łącznika usług Salesforce w wersji 1](v1/data-factory-salesforce-connector.md).

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Możesz skopiować dane z bazy danych witryny Salesforce żadnych obsługiwanych ujścia magazynu danych. Lista magazynów danych, które są obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

W szczególności tego łącznika usług Salesforce obsługuje następujące wersje usług Salesforce: **Developer Edition w wersji Professional, Enterprise Edition albo nieograniczone Edition**. I umożliwia kopiowanie danych z usług Salesforce **produkcji, piaskownicy i domeny niestandardowej**.

## <a name="prerequisites"></a>Wymagania wstępne

* Uprawnienia interfejsu API musi być włączony w usłudze Salesforce. Zobacz [jak włączyć dostęp do interfejsu API w usłudze Salesforce przez zestaw uprawnień?](https://www.data2crm.com/migration/faqs/enable-api-access-salesforce-permission-set/)

## <a name="salesforce-request-limits"></a>Limity żądań usług SalesForce

Usługa SalesForce obejmuje limity dla całkowita liczba żądań interfejsu API i równoczesnych żądań interfejsu API. Pamiętaj o następujących kwestiach:

- Jeśli liczba jednoczesnych żądań przekracza limit, ograniczanie występuje i zobacz losowe awarie.
- Jeśli całkowita liczba żądań przekracza limit, konta usług Salesforce jest zablokowany przez 24 godziny.

W obu przypadkach może być również wyświetlony błąd "REQUEST_LIMIT_EXCEEDED". Zobacz sekcję "Limity żądań interfejsu API" w [Salesforce Developer limity](http://resources.docs.salesforce.com/200/20/en-us/sfdc/pdf/salesforce_app_limits_cheatsheet.pdf) artykułu, aby uzyskać szczegółowe informacje.

## <a name="getting-started"></a>Wprowadzenie
Można utworzyć potoku o aktywności kopiowania przy użyciu zestawu .NET SDK, zestaw SDK Python, programu Azure PowerShell, interfejsu API REST lub szablonu usługi Azure Resource Manager. Zobacz [samouczek działania kopiowania](quickstart-create-data-factory-dot-net.md) instrukcje krok po kroku utworzyć potok z działaniem kopiowania.

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek fabryki danych określonej do łącznika usług Salesforce.

## <a name="linked-service-properties"></a>Połączona usługa właściwości

Obsługiwane są następujące właściwości usługi Salesforce połączone:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type |Właściwość type musi mieć ustawioną: **Salesforce**. |Tak |
| environmentUrl | Określ wystąpienie adres URL usługi Salesforce. <br><br> -Domyślna to `"https://login.salesforce.com"`. <br> -Aby skopiować dane z piaskownicy, określ `"https://test.salesforce.com"`. <br> -Aby skopiować dane z domeny niestandardowej, na przykład określić, `"https://[domain].my.salesforce.com"`. |Nie |
| nazwa użytkownika |Określ nazwę użytkownika dla konta użytkownika. |Tak |
| hasło |Określ hasło dla konta użytkownika. |Tak |
| securityToken |Określ tokenu zabezpieczającego dla konta użytkownika. Zobacz [uzyskać token zabezpieczeń](https://help.salesforce.com/apex/HTViewHelpDoc?id=user_security_token.htm) instrukcje dotyczące resetowania/Get tokenu zabezpieczającego. Aby dowiedzieć się więcej o tokeny zabezpieczające ogólnie rzecz biorąc, zobacz [zabezpieczeń i interfejsu API](https://developer.salesforce.com/docs/atlas.en-us.api.meta/api/sforce_api_concepts_security.htm). |Tak |

**Przykład:**

```json
{
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
        }
    },
    "name": "SalesforceLinkedService"
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje zobacz artykuł zestawów danych. Ta sekcja zawiera listę właściwości obsługiwanych przez zestaw danych Salesforce.

Aby skopiować dane z witryny Salesforce, ustaw właściwość Typ zestawu danych do **RelationalTable**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type zestawu danych: **RelationalTable** | Tak |
| tableName | Nazwa tabeli w bazie danych usług Salesforce. | Nie (Jeśli określono parametr "zapytania" w źródle działania) |

> [!IMPORTANT]
> Część "__c" Nazwa interfejsu API jest wymagany dla dowolnych niestandardowych obiektów.

![Nazwa danych fabryki — połączenie usługi Salesforce — interfejs API](media/copy-data-from-salesforce/data-factory-salesforce-api-name.png)

**Przykład:**

```json
{
    "name": "SalesforceDataset",
    "properties":
    {
        "type": "RelationalTable",
        "linkedServiceName": {
            "referenceName": "<Salesforce linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "tableName": "MyTable__c"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwane przez źródło Salesforce.

### <a name="salesforce-as-source"></a>SalesForce jako źródło

Aby skopiować dane z witryny Salesforce, Ustaw typ źródła w przypadku działania kopiowania do **RelationalSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type źródła działania kopiowania: **RelationalSource** | Tak |
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
                "type": "RelationalSource",
                "query": "SELECT Col_Currency__c, Col_Date__c, Col_Email__c FROM AllDataType__c"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="query-tips"></a>Wskazówki zapytania

### <a name="retrieving-data-from-salesforce-report"></a>Pobieranie danych z raportu usług Salesforce

Można pobrać dane z raportów usług Salesforce, określając kwerendy w postaci `{call "<report name>"}. Example: `"zapytania": "{call \"TestReport\"}" ".

### <a name="retrieving-deleted-records-from-salesforce-recycle-bin"></a>Przywracanie usuniętych rekordów z Kosza usług Salesforce

Kwerenda nietrwałego usuniętych rekordów z Kosza usług Salesforce, można określić **"IsDeleted = 1"** w zapytaniu. Na przykład:

* Aby odpytać tylko usunięte rekordy, określ "Wybierz * z MyTable__c **gdzie IsDeleted = 1**"
* Aby wysyłać zapytania o wszystkie rekordy tym usuniętych i istniejących, określ "Wybierz * z MyTable__c **gdzie IsDeleted = 0 lub IsDeleted = 1**"

### <a name="retrieving-data-using-where-clause-on-datetime-column"></a>Podczas pobierania danych przy użyciu where klauzuli według kolumny daty i godziny

Gdy Określ SOQL lub SQL zapytanie, zwrócić uwagę różnica format daty/godziny. Na przykład:

* **Przykładowe SOQL**:`$$Text.Format('SELECT Id, Name, BillingCity FROM Account WHERE LastModifiedDate >= {0:yyyy-MM-ddTHH:mm:ssZ} AND LastModifiedDate < {1:yyyy-MM-ddTHH:mm:ssZ}', <datetime parameter>, <datetime parameter>)`
* **Przykładowe SQL**:`$$Text.Format('SELECT * FROM Account WHERE LastModifiedDate >= {{ts\\'{0:yyyy-MM-dd HH:mm:ss}\\'}} AND LastModifiedDate < {{ts\\'{1:yyyy-MM-dd HH:mm:ss}\\'}}', <datetime parameter>, <datetime parameter>)`

## <a name="data-type-mapping-for-salesforce"></a>Mapowanie dla usług Salesforce typu danych

Podczas kopiowania danych z usług Salesforce, z typów danych Salesforce są używane następujące mapowania do typów danych tymczasowych fabryki danych Azure. Zobacz [schemat i dane typu mapowania](copy-activity-schema-and-type-mapping.md) Aby poznać sposób działania kopiowania mapowania typu źródłowego: schemat i dane sink.

| Typ danych witryny SalesForce | Typ danych tymczasowych fabryki danych |
|:--- |:--- |
| Automatyczny numer |Ciąg |
| Pole wyboru |Wartość logiczna |
| Waluta |O podwójnej precyzji |
| Date |Data i godzina |
| Data i godzina |Data i godzina |
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
| ADRES URL |Ciąg |


## <a name="next-steps"></a>Następne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md##supported-data-stores-and-formats).