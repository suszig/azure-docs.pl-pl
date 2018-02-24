---
title: "Kopiowanie danych z usługi ServiceNow przy użyciu fabryki danych Azure (wersja Beta) | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skopiować dane z usługi ServiceNow do zbiornika obsługiwane magazyny danych za pomocą działania kopiowania w potoku fabryki danych Azure."
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
ms.date: 02/22/2018
ms.author: jingwang
ms.openlocfilehash: 64b0982ab1d0b212120d962d4c47a1b8db8ca025
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="copy-data-from-servicenow-using-azure-data-factory-beta"></a>Kopiowanie danych z usługi ServiceNow przy użyciu fabryki danych Azure (wersja Beta)

W tym artykule omówiono sposób użycia działanie kopiowania w fabryce danych Azure, aby skopiować dane z usługi ServiceNow. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [działanie kopiowania w wersji 1](v1/data-factory-data-movement-activities.md).

> [!IMPORTANT]
> Ten łącznik jest obecnie w wersji Beta. Możesz wypróbować jej możliwości i przekaż nam swoją opinię. Nie należy używać go w środowisku produkcyjnym.

## <a name="supported-capabilities"></a>Obsługiwane możliwości

Możesz skopiować dane z usługi ServiceNow żadnych obsługiwanych ujścia magazynu danych. Lista magazynów danych, które są obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

Fabryka danych Azure oferuje wbudowane sterowników, aby umożliwić łączność, w związku z tym nie trzeba ręcznie zainstalowania sterownika korzystania z tego łącznika.

## <a name="getting-started"></a>Wprowadzenie

[!INCLUDE [data-factory-v2-connector-get-started-2](../../includes/data-factory-v2-connector-get-started-2.md)]

Poniższe sekcje zawierają szczegółowe informacje o właściwościach, które są używane do definiowania jednostek fabryki danych określonej do łącznika usługi ServiceNow.

## <a name="linked-service-properties"></a>Połączona usługa właściwości

Obsługiwane są następujące właściwości usługi ServiceNow połączone:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **ServiceNow** | Yes |
| endpoint | Punkt końcowy serwera usługi ServiceNow (`http://ServiceNowData.com`).  | Yes |
| authenticationType | Typ uwierzytelniania do użycia. <br/>Dozwolone wartości to: **podstawowe**, **OAuth2** | Yes |
| nazwa użytkownika | Nazwa użytkownika używana do łączenia się z serwerem usługi ServiceNow uwierzytelniania Basic i protokołu OAuth2.  | Nie |
| hasło | Hasło odpowiadający nazwie użytkownika do uwierzytelniania Basic i OAuth2. Zaznacz to pole jako SecureString Zapisz w bezpiecznej lokalizacji w fabryce danych lub [odwołania klucz tajny przechowywane w usłudze Azure Key Vault](store-credentials-in-key-vault.md). | Nie |
| clientId | Identyfikator klienta do uwierzytelniania protokołu OAuth2.  | Nie |
| clientSecret | Klucz tajny klienta do uwierzytelniania protokołu OAuth2. Zaznacz to pole jako SecureString Zapisz w bezpiecznej lokalizacji w fabryce danych lub [odwołania klucz tajny przechowywane w usłudze Azure Key Vault](store-credentials-in-key-vault.md). | Nie |
| useEncryptedEndpoints | Określa, czy punkty końcowe źródła danych są szyfrowane przy użyciu protokołu HTTPS. Wartość domyślna to true.  | Nie |
| useHostVerification | Określa, czy nazwa hosta w certyfikacie serwera, aby dopasować nazwę hosta serwera podczas nawiązywania połączenia za pośrednictwem protokołu SSL. Wartość domyślna to true.  | Nie |
| usePeerVerification | Określa, czy można zweryfikować tożsamości serwera podczas nawiązywania połączenia za pośrednictwem protokołu SSL. Wartość domyślna to true.  | Nie |

**Przykład:**

```json
{
    "name": "ServiceNowLinkedService",
    "properties": {
        "type": "ServiceNow",
        "typeProperties": {
            "endpoint" : "http://ServiceNowData.com",
            "authenticationType" : "Basic",
            "username" : "<username>",
            "password": {
                 "type": "SecureString",
                 "value": "<password>"
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje, zobacz [zestawów danych](concepts-datasets-linked-services.md) artykułu. Ta sekcja zawiera listę właściwości obsługiwanych przez usługi ServiceNow zestawu danych.

Aby skopiować dane z usługi ServiceNow, ustaw właściwość Typ zestawu danych do **ServiceNowObject**. Nie ma dodatkowych właściwości określonego typu w tego typu dataset.

**Przykład**

```json
{
    "name": "ServiceNowDataset",
    "properties": {
        "type": "ServiceNowObject",
        "linkedServiceName": {
            "referenceName": "<ServiceNow linked service name>",
            "type": "LinkedServiceReference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Właściwości działania kopiowania

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez usługi ServiceNow źródła właściwości.

### <a name="servicenow-as-source"></a>Usługi ServiceNow jako źródło

Aby skopiować dane z usługi ServiceNow, należy ustawić typ źródła w przypadku działania kopiowania do **ServiceNowSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type źródła działania kopiowania: **ServiceNowSource** | Yes |
| query | Użyj niestandardowych zapytania SQL można odczytać danych. Na przykład: `"SELECT * FROM Actual.alm_asset"`. | Yes |

Należy pamiętać, że podczas określania schematu i kolumn dla usługi ServiceNow w zapytaniu:

- **Schemat:** Określ schematu jako `Actual` lub `Display` kwerendę usługi ServiceNow, które można przyjrzeć się go jako parametru `sysparm_display_value` jako PRAWDA lub FAŁSZ, gdy wywołanie [interfejsy API restful ServiceNow](https://developer.servicenow.com/app.do#!/rest_api_doc?v=jakarta&id=r_AggregateAPI-GET). 
- **Kolumna:** nazwa kolumny dla wartości rzeczywistych w obszarze `Actual` jest scehma `[columne name]_value`, natomiast w przypadku wartości wyświetlanej w obszarze `Display` schemat jest `[columne name]_display_value`. Uwaga nazwy kolumny musi mapy do schematu, używany w zapytaniu.

**Przykładowe zapytanie:** 
 `SELECT col_value FROM Actual.alm_asset` lub `SELECT col_display_value FROM Display.alm_asset`

**Przykład:**

```json
"activities":[
    {
        "name": "CopyFromServiceNow",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<ServiceNow input dataset name>",
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
                "type": "ServiceNowSource",
                "query": "SELECT * FROM Actual.alm_asset"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

## <a name="next-steps"></a>Kolejne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats).
