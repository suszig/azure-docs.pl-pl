---
title: "Kopiowanie danych z programu Dynamics CRM i 365 przy użyciu fabryki danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skopiować dane z programu Dynamics CRM i 365 do zbiornika obsługiwane magazyny danych za pomocą działania kopiowania w potoku fabryki danych Azure."
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
ms.date: 11/02/2017
ms.author: jingwang
ms.openlocfilehash: aab7dfff0b77b5f09a1388c9bac9bdd63ebd8b17
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/04/2017
---
# <a name="copy-data-from-dynamics-365dynamics-crm-using-azure-data-factory"></a>Kopiowanie danych z Dynamics 365 / Dynamics CRM przy użyciu fabryki danych Azure

W tym artykule omówiono sposób użycia działanie kopiowania w fabryce danych Azure, aby skopiować dane z Dynamics 365 / Dynamics CRM. Opiera się na [skopiuj omówienie działania](copy-activity-overview.md) artykułu, który przedstawia ogólny przegląd działanie kopiowania.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [działanie kopiowania w wersji 1](v1/data-factory-data-movement-activities.md).

## <a name="supported-scenarios"></a>Obsługiwane scenariusze

Możesz skopiować dane z Dynamics 365 / Dynamics CRM żadnych obsługiwanych ujścia magazynu danych. Lista magazynów danych obsługiwane jako źródła/wychwytywanie przez działanie kopiowania, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats) tabeli.

W szczególności ten łącznik Dynamics obsługuje poniżej Dynamics wersje i typy uwierzytelniania:

| Dynamics wersji | Typy uwierzytelniania | Przykłady połączonej usługi |
|:--- |:--- |:--- |
| Dynamics 365 online <br> Dynamics CRM online | Usługi Office 365 | [Dynamics Online + uwierzytelnianie usługi Office 365](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 lokalnej z IFD <br> Dynamics CRM 2016 na lokalnym IFD <br> Dynamics CRM 2015 lokalnej z IFD | IFD | [Dynamics lokalnej z IFD + IFD uwierzytelniania](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

*IFD jest skrót Internet ukierunkowane wdrożenia.*

> [!NOTE]
> Używanie łącznika Dynamics przechowywania hasła w usłudze Azure Key Vault i umożliwić ADF kopiowania działaniach ściągania stamtąd podczas wykonywania kopii danych. Zobacz, jak skonfigurować w [połączona usługa właściwości](#linked-service-properties) sekcji.

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
| hasło | Określ hasło dla konta użytkownika, określone nazwy użytkownika. Konieczne umieszczanie hasła w usłudze Azure Key Vault i skonfigurować hasło jako "AzureKeyVaultSecret". Aby uzyskać więcej informacji, zobacz [przechowywania poświadczeń w magazynie kluczy](store-credentials-in-key-vault.md) artykułu. | Tak |

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
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
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
| hasło | Określ hasło dla konta użytkownika, określone nazwy użytkownika. Należy pamiętać, że konieczne umieszczanie hasła w usłudze Azure Key Vault i skonfigurować hasło jako "AzureKeyVaultSecret". Aby uzyskać więcej informacji, zobacz [przechowywania poświadczeń w magazynie kluczy](store-credentials-in-key-vault.md) artykułu. | Tak |

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
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Właściwości zestawu danych

Aby uzyskać pełną listę właściwości dostępnych do definiowania zestawów danych i sekcje, zobacz [zestawów danych](concepts-datasets-linked-services.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez zestaw danych Dynamics właściwości.

Aby skopiować dane z programu Dynamics, ustaw właściwość Typ zestawu danych do **DynamicsEntity**. Obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość type zestawu danych: **DynamicsEntity** |Tak |
| Nazwa jednostki | Nazwa logiczna obiektu do pobrania. | Nie (Jeśli określono parametr "zapytania" w źródle działania) |

> [!IMPORTANT]
> **Sekcja "structure" w zestawie danych jest wymagana do Dynamics**, który definiuje typ danych kolumny danych Dynamics, który chcesz skopiować. Dowiedz się więcej o [struktury zestawu danych](concepts-datasets-linked-services.md#dataset-structure) i [mapowanie typu danych dla programu Dynamics](#data-type-mapping-for-dynamics).

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

Pełną listę sekcje i właściwości dostępnych dla definiowania działań, zobacz [potoki](concepts-pipelines-activities.md) artykułu. Ta sekcja zawiera listę obsługiwanych przez źródło Dynamics właściwości.

### <a name="dynamics-as-source"></a>Dynamics jako źródło

Aby skopiować dane z programu Dynamics, należy ustawić typ źródła w przypadku działania kopiowania do **DynamicsSource**. Następujące właściwości są obsługiwane w przypadku działania kopiowania **źródła** sekcji:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **DynamicsSource**  | Tak |
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

## <a name="data-type-mapping-for-dynamics"></a>Mapowanie do programu Dynamics typu danych

Podczas kopiowania danych z programu Dynamics, następujące mapowania są używane z Dynamics typów danych do typów danych tymczasowych fabryki danych Azure. Zobacz [schemat i dane typu mapowania](copy-activity-schema-and-type-mapping.md) Aby poznać sposób działania kopiowania mapowania typu źródłowego: schemat i dane sink.

Skonfiguruj odpowiedni typ danych ADF w strukturze zestawu danych, w oparciu o Twoje źródło danych Dynamics typu przy użyciu poniższej tabeli mapowania:

| Dynamics — typ danych | Typ danych tymczasowych fabryki danych |
|:--- |:--- |
| AttributeTypeCode.BigInt | Długie |
| AttributeTypeCode.Boolean | Wartość logiczna |
| AttributeType.Customer | Identyfikator GUID |
| AttributeType.DateTime | Data i godzina |
| AttributeType.Decimal | Decimal |
| AttributeType.Double | O podwójnej precyzji |
| AttributeType.EntityName | Ciąg |
| AttributeType.Integer | Int32 |
| AttributeType.Lookup | Identyfikator GUID |
| AttributeType.ManagedProperty | Wartość logiczna |
| AttributeType.Memo | Ciąg |
| AttributeType.Money | Decimal |
| AttributeType.Owner | Identyfikator GUID |
| AttributeType.Picklist | Int32 |
| AttributeType.Uniqueidentifier | Identyfikator GUID |
| AttributeType.String | Ciąg |
| AttributeType.State | Int32 |
| AttributeType.Status | Int32 |

> [!NOTE]
> Typ danych Dynamics AttributeType.CalendarRules i AttributeType.PartyList nie są obsługiwane.


## <a name="next-steps"></a>Następne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md##supported-data-stores-and-formats).