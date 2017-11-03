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
ms.date: 09/26/2017
ms.author: jingwang
ms.openlocfilehash: 193d7c77f01384106b3e0d932d02ba6cdff9e750
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="store-credential-in-azure-key-vault"></a>Przechowywania poświadczeń w usłudze Azure Key Vault

Można zapisać poświadczeń magazyny danych w [usługi Azure Key Vault](../key-vault/key-vault-whatis.md). Fabryka danych Azure pobiera poświadczeń podczas wykonywania działania, który używa magazynu danych. Obecnie tylko [łącznika Dynamics](connector-dynamics-crm-office-365.md) obsługuje tę funkcję.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [documenttion dla fabryki danych version1](v1/data-factory-introduction.md).

## <a name="steps"></a>Kroki

Podczas tworzenia fabryki danych, można utworzyć tożsamości usługi oraz tworzenie fabryki. Tożsamość usługi jest zarejestrowany do usługi Azure Directory działania aplikacji zarządzanej i reprezentuje tej fabryki danych.

- Podczas tworzenia fabryki danych za pośrednictwem **portalu Azure lub programu PowerShell**, tożsamość usługi będą zawsze tworzone automatycznie od publicznej wersji zapoznawczej.
- Podczas tworzenia fabryki danych za pośrednictwem **SDK**, tożsamość usługi zostanie utworzona tylko w przypadku określenia "tożsamości = FactoryIdentity() nowy" w obiekcie fabrykę do tworzenia. Zobacz przykład z [.NET — Szybki Start — tworzenie fabryki danych](quickstart-create-data-factory-dot-net.md#create-a-data-factory).
- Podczas tworzenia fabryki danych za pośrednictwem **interfejsu API REST**, tożsamość usługi zostanie utworzona tylko, jeśli określono sekcję "identity" w treści żądania. Zobacz przykład z [REST Szybki Start — tworzenie fabryki danych](quickstart-create-data-factory-rest-api.md#create-a-data-factory).

Aby odwołać poświadczenia przechowywane w usłudze Azure Key Vault, musisz:

1. Skopiuj "Tożsamości identyfikator aplikacji usługi" generowane wraz z fabryką danych. Zapoznaj się [pobrać tożsamość usługi](#retrieve-service-identity).
2. Udzielać dostępu tożsamości usługi Magazyn kluczy Azure. W magazynie kluczy, -> dostępu sterowania -> Dodaj -> Wyszukaj ten identyfikator aplikacji tożsamości usługi, dodać uprawnienia czytnika. Umożliwia to wyznaczonych fabryki klucza tajnego w magazynie kluczy dostępu do.
3. Tworzenie połączonej usługi wskazujący magazyn kluczy Azure. Zapoznaj się [usługi Azure Key Vault połączona usługa](#azure-key-vault-linked-service).
4. Tworzenie usługi magazynu połączone danych, w których odwołanie magazynu odpowiadający mu klucz tajny przechowywanych w. Zapoznaj się [odwołania poświadczeń przechowywanych w magazynie kluczy](#reference-credential-stored-in-key-vault).

## <a name="retrieve-service-identity"></a>Pobieranie tożsamości usługi

Możesz pobrać z portalu Azure lub programistycznie tożsamości usługi. W poniższych sekcjach przedstawiono kilka przykładów.

>[!TIP]
> Jeśli nie widzisz tożsamość usługi [Generowanie tożsamość usługi](#generate-service-identity) , aktualizując fabryce.

### <a name="using-azure-portal"></a>Korzystanie z witryny Azure Portal

Można znaleźć informacje o tożsamości usługi z portalu Azure -> fabrykę danych -> Ustawienia -> Właściwości:

- IDENTYFIKATOR TOŻSAMOŚCI USŁUGI
- DZIERŻAWA TOŻSAMOŚCI USŁUGI
- **Identyfikator aplikacji tożsamości usługi** > skopiować tę wartość, aby udzielić dostępu w magazynie kluczy

![Pobieranie tożsamości usługi](media/store-credentials-in-key-vault/retrieve-service-identity-portal.png)

### <a name="using-powershell"></a>Korzystanie z programu PowerShell

Tożsamość usługi identyfikator podmiotu zabezpieczeń i identyfikator dzierżawcy zostaną zwrócone w wyniku pobrania fabryki danych określonego w następujący sposób:

```powershell
PS C:\WINDOWS\system32> (Get-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName>).Identity

PrincipalId                          TenantId
-----------                          --------
765ad4ab-XXXX-XXXX-XXXX-51ed985819dc 72f988bf-XXXX-XXXX-XXXX-2d7cd011db47
```

Skopiuj identyfikator podmiotu zabezpieczeń, a następnie uruchom poniżej polecenia usługi Azure Active Directory z identyfikator podmiotu zabezpieczeń jako parametr, aby uzyskać **ApplicationId**, którym można udzielić dostępu w magazynie kluczy:

```powershell
PS C:\WINDOWS\system32> Get-AzureRmADServicePrincipal -ObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc

ServicePrincipalNames : {76f668b3-XXXX-XXXX-XXXX-1b3348c75e02, https://identity.azure.net/P86P8g6nt1QxfPJx22om8MOooMf/Ag0Qf/nnREppHkU=}
ApplicationId         : 76f668b3-XXXX-XXXX-XXXX-1b3348c75e02
DisplayName           : ADFV2DemoFactory
Id                    : 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
Type                  : ServicePrincipal
```

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
| Magazyn | Odwołuje się do usługi Azure Key Vault połączone, które służy do przechowywania poświadczeń. | Tak |

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

## <a name="generate-service-identity"></a>Generowanie tożsamości usługi

Jeśli okaże się fabrykę danych nie ma usługi tożsamość skojarzoną po [pobrać tożsamość usługi](#retrieve-service-identity) instrukcji, można wygenerować jedną, aktualizując fabryka danych z inicjatora tożsamości programowo.

> [!NOTE]
> - **Nie można zmienić tożsamość usługi**. Aktualizowanie fabryki danych, które mają już tożsamość usługi nie będą miały wpływu, tożsamość usługi pozostaną niezmienione.
> - **Nie można usunąć tożsamości usługi**. Jeśli zaktualizujesz fabryki danych, które mają już tożsamości usługi bez określania "identity" parametru w obiekt fabryki lub bez określania sekcji "identity" w treści żądania REST, wystąpi błąd.

W poniższych sekcjach przedstawiono kilka przykładów na generowanie tożsamość usługi fabrycznej, jeśli nie istnieje.

### <a name="using-powershell"></a>Korzystanie z programu PowerShell

Wywołanie **AzureRmDataFactoryV2 zestaw** polecenie ponownie, a następnie zostanie wyświetlony pola "Identity" nowo tworzone:

```powershell
PS C:\WINDOWS\system32> Set-AzureRmDataFactoryV2 -ResourceGroupName <resourceGroupName> -Name <dataFactoryName> -Location <region>

DataFactoryName   : ADFV2DemoFactory
DataFactoryId     : /subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory
ResourceGroupName : <resourceGroupName>
Location          : East US
Tags              : {}
Identity          : Microsoft.Azure.Management.DataFactory.Models.FactoryIdentity
ProvisioningState : Succeeded
```

### <a name="using-rest-api"></a>Korzystanie z interfejsu API REST

Wywołanie poniżej interfejsu API z sekcją "identity" w treści żądania:

```
PATCH https://management.azure.com/subscriptions/<subsID>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<data factory name>?api-version=2017-09-01-preview
```

**Treść żądania**: Dodaj "identity": {"type": "SystemAssigned"}.

```json
{
    "name": "<dataFactoryName>",
    "location": "<region>",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
```

**Odpowiedź**: tożsamość usługi jest tworzony automatycznie i odpowiednio zawiera sekcja "identity".

```json
{
    "name": "ADFV2DemoFactory",
    "tags": {},
    "properties": {
        "provisioningState": "Succeeded",
        "loggingStorageAccountKey": "**********",
        "createTime": "2017-09-26T04:10:01.1135678Z",
        "version": "2017-09-01-preview"
    },
    "identity": {
        "type": "SystemAssigned",
        "principalId": "765ad4ab-XXXX-XXXX-XXXX-51ed985819dc",
        "tenantId": "72f988bf-XXXX-XXXX-XXXX-2d7cd011db47"
    },
    "id": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/ADFV2DemoFactory",
    "type": "Microsoft.DataFactory/factories",
    "location": "EastUS"
}
```

### <a name="using-sdk"></a>Przy użyciu zestawu SDK

Wywołaj funkcję create_or_update fabryka danych z tożsamością = FactoryIdentity() nowe. Przykładowy kod przy użyciu platformy .NET:

```csharp
Factory dataFactory = new Factory
{
    Location = <region>,
    Identity = new FactoryIdentity()
};
client.Factories.CreateOrUpdate(resourceGroup, dataFactoryName, dataFactory);
```

## <a name="next-steps"></a>Następne kroki
Lista magazynów danych obsługiwane jako źródła i wychwytywanie przez działanie kopiowania w fabryce danych Azure, zobacz [obsługiwane magazyny danych](copy-activity-overview.md##supported-data-stores-and-formats).