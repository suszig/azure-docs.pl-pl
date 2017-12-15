---
title: "Przechowywania poświadczeń w usłudze Azure Key Vault | Dokumentacja firmy Microsoft"
description: "Informacje o sposobie przechowywania poświadczeń dla magazynów danych używany w usługi Azure key vault, która fabryka danych Azure mogą automatycznie pobierać w czasie wykonywania."
services: data-factory
author: linda33wj
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/14/2017
ms.author: jingwang
ms.openlocfilehash: 145c2bc0556010389e78e523fde6fd4b9063f930
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/14/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Przechowywania poświadczeń w usłudze Azure Key Vault

Można zapisać poświadczeń magazyny danych w [usługi Azure Key Vault](../key-vault/key-vault-whatis.md). Fabryka danych Azure pobiera poświadczeń podczas wykonywania działania, który używa magazynu danych.

Obecnie [łącznika Dynamics](connector-dynamics-crm-office-365.md), [łącznika usług Salesforce](connector-salesforce.md) i kilka nowo włączyć obsługę łączniki tej funkcji. Oczekiwany więcej przesyłanych później. Możesz sprawdzić każdego tematu łącznika na szczegóły. Dla pola tajne, które obsługują tę funkcję, zobaczysz notatki w zawierający opis komunikat "*można Oznacz to pole jako SecureString Zapisz w bezpiecznej lokalizacji w ADF, lub przechowywanie haseł w usługa Azure Key Vault oraz umożliwić ściągania działaniach kopiowania z tego miejsca Podczas wykonywania kopii danych — Dowiedz się więcej z magazynu poświadczeń w magazynie kluczy.* "

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [dokumentacji dla fabryki danych version1](v1/data-factory-introduction.md).

## <a name="prerequisites"></a>Wymagania wstępne

Ta funkcja opiera się na tożsamości usługi fabryka danych. Dowiedz się, jak to działa z [tożsamości usługi fabryka danych](data-factory-service-identity.md) i upewnij się, że skojarzona jeden ma fabrykę danych.

## <a name="steps"></a>Kroki

Aby odwołać poświadczenia przechowywane w usłudze Azure Key Vault, musisz:

1. [Pobieranie tożsamości usługi fabryka danych](data-factory-service-identity.md#retrieve-service-identity) przez skopiowanie wartości "Usługa tożsamości aplikacji Identyfikatora" generowane wraz z fabryką.
2. Udzielać dostępu tożsamości usługi Magazyn kluczy Azure. W magazynie kluczy, -> dostępu sterowania -> Dodaj -> wyszukiwania IDENTYFIKATORA aplikacji tożsamości usługi do dodania co najmniej **czytnika** uprawnienia. Umożliwia to wyznaczonych fabryki klucza tajnego w magazynie kluczy dostępu do.
3. Tworzenie połączonej usługi wskazujący magazyn kluczy Azure. Zapoznaj się [usługi Azure Key Vault połączona usługa](#azure-key-vault-linked-service).
4. Tworzenie usługi magazynu połączone danych, w których odwołanie magazynu odpowiadający mu klucz tajny przechowywanych w. Zapoznaj się [odwołania poświadczeń przechowywanych w magazynie kluczy](#reference-credential-stored-in-key-vault).

## <a name="azure-key-vault-linked-service"></a>Usługa Azure Key Vault połączone

Obsługiwane są następujące właściwości dla usługi Azure Key Vault połączone:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Właściwość type musi mieć ustawioną: **AzureKeyVault**. | Tak |
| BaseUrl | Podaj adres URL magazynu kluczy Azure. | Tak |

**Przykład:**

```json
{
    "name": "AzureKeyVaultLinkedService",
    "properties": {
        "type": "AzureKeyVault",
        "typeProperties": {
            "baseUrl": "https://<azureKeyVaultName>.vault.azure.net"
        }
    }
}
```

## <a name="reference-credential-stored-in-key-vault"></a>Odwołanie poświadczeń przechowywanych w magazynie kluczy

Po skonfigurowaniu pola w połączonej usłudze odwołujące się do magazynu kluczy klucz tajny, obsługiwane są następujące właściwości:

| Właściwość | Opis | Wymagane |
|:--- |:--- |:--- |
| type | Musi mieć ustawioną właściwość Typ pola: **AzureKeyVaultSecret**. | Tak |
| secretName | Nazwa klucza tajnego w magazynie kluczy azure. | Tak |
| secretVersion | Wersja klucza tajnego w magazynie kluczy azure.<br/>Jeśli nie zostanie określony, zawsze używa najnowszą wersję klucza tajnego.<br/>Jeśli zostanie określona, następnie go systemu danej wersji.| Nie |
| sklep | Odwołuje się do usługi Azure Key Vault połączone, które służy do przechowywania poświadczeń. | Tak |

**Przykład: (zobacz sekcję "password")**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "typeProperties": {
            "deploymentType": "<>",
            "organizationName": "<>",
            "authenticationType": "<>",
            "username": "<>",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "<secret name in AKV>",
                "store":{
                    "referenceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="next-steps"></a>Następne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md#supported-data-stores-and-formats).