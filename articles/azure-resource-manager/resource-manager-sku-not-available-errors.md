---
title: "Jednostka SKU Azure nie jest dostępna błędy | Dokumentacja firmy Microsoft"
description: "Opisuje sposób rozwiązywania jednostka SKU nie jest dostępna wystąpił błąd podczas wdrażania."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: support-article
ms.date: 09/13/2017
ms.author: tomfitz
ms.openlocfilehash: 25cea4ae23471d182105ca3f720aaf74f81bf8c4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-sku-not-available"></a>Rozwiązywanie błędów dla jednostki SKU nie jest dostępna

W tym artykule opisano sposób rozwiązywania **SkuNotAvailable** błędu.

## <a name="symptom"></a>Objaw

W przypadku wdrażania zasobów (zazwyczaj maszyny wirtualnej), pojawi się następujący kod błędu i komunikat o błędzie:

```
Code: SkuNotAvailable
Message: The requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy to a different location.
```

## <a name="cause"></a>Przyczyna

Ten błąd jest wyświetlany, gdy zasób SKU wybrano (takich jak rozmiar maszyny Wirtualnej) nie jest dostępny w wybranej lokalizacji.

## <a name="solution"></a>Rozwiązanie

Aby rozwiązać ten problem, należy określić, które jednostki SKU są dostępne w regionie. Aby znaleźć dostępne jednostki SKU, można użyć programu PowerShell, portalu lub operacji REST.

### <a name="solution-1"></a>Rozwiązanie 1

Użyj [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) polecenia programu PowerShell. Filtrowanie wyników według lokalizacji. Musi mieć najnowszą wersję programu PowerShell dla tego polecenia.

```powershell
Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
```

Wyniki obejmują listę jednostek SKU dla lokalizacji i zastosowanie jakiekolwiek ograniczenia dotyczące tej wersji produktu.

```powershell
ResourceType                Name      Locations Restriction                      Capability Value
------------                ----      --------- -----------                      ---------- -----
availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
virtualMachines      Standard_A0 southcentralus
virtualMachines      Standard_A1 southcentralus
virtualMachines      Standard_A2 southcentralus
```

### <a name="solution-2"></a>Rozwiązanie 2

W przypadku interfejsu wiersza polecenia Azure, użyj `az vm list-skus` polecenia. Następnie można użyć `grep` lub podobne narzędzia do filtrowania wyników.

```bash
$ az vm list-skus --output table
ResourceType      Locations           Name                    Capabilities                       Tier      Size           Restrictions
----------------  ------------------  ----------------------  ---------------------------------  --------  -------------  ---------------------------
availabilitySets  eastus              Classic                 MaximumPlatformFaultDomainCount=3
avilabilitySets   eastus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  eastus2             Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  westus              Aligned                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Classic                 MaximumPlatformFaultDomainCount=3
availabilitySets  centralus           Aligned                 MaximumPlatformFaultDomainCount=3
```

### <a name="solution-3"></a>Rozwiązanie 3

Użyj [portal](https://portal.azure.com). Zaloguj się do portalu, a następnie dodaj zasobów za pomocą interfejsu. Jak ustawić wartości, zobaczysz dostępne jednostki SKU dla tego zasobu. Nie ma potrzeby wdrażania.

![dostępne jednostki SKU](./media/resource-manager-sku-not-available-errors/view-sku.png)

### <a name="solution-4"></a>Rozwiązanie 4

Użyj interfejsu API REST dla maszyn wirtualnych. Wyślij żądanie następujące:

```HTTP 
GET
https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
```

Zwraca dostępne jednostki SKU i regiony w następującym formacie:

```json
{
  "value": [
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A0",
      "tier": "Standard",
      "size": "A0",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    {
      "resourceType": "virtualMachines",
      "name": "Standard_A1",
      "tier": "Standard",
      "size": "A1",
      "locations": [
        "eastus"
      ],
      "restrictions": []
    },
    ...
  ]
}
```

Jeśli nie można odnaleźć odpowiedniej jednostki SKU w tym regionie lub alternatywne regionu, który spełnia firmy musi Prześlij [żądania SKU](https://aka.ms/skurestriction) pomocy technicznej platformy Azure.