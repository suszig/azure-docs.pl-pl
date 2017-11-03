---
title: "Błędy przydziału platformy Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób rozwiązania błędów qouta zasobów."
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
ms.openlocfilehash: 6a8024a12c4a79e92e37df0a56b6e6bd0cb6a8d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="resolve-errors-for-resource-quotas"></a>Rozwiązywanie błędów dla przydziałami zasobów

W tym artykule opisano błędy przydziału, które można napotkać podczas wdrażania zasobów.

## <a name="symptom"></a>Objaw

Wdrożenie szablonu, która tworzy zasoby, które przekraczają przydziałami Azure można uzyskać Błąd wdrażania, która wygląda następująco:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

Lub może zostać wyświetlony:

```
Code=ResourceQuotaExceeded
Message=Creating the resource of type <resource-type> would exceed the quota of <number>
resources of type <resource-type> per resource group. The current resource count is <number>,
please delete some resources of this type before creating a new one.
```

## <a name="cause"></a>Przyczyna

Przydziały są stosowane dla grupy zasobów, subskrypcje kont i innych zakresów. Na przykład subskrypcji można skonfigurować w celu ograniczenia liczby rdzeni dla regionu. Próba wdrożyć maszynę wirtualną o większej liczby rdzeni niż dozwolone kwota pojawi się komunikat o błędzie informujący, że została przekroczona.
Przydział pełne informacje, zobacz [subskrypcji platformy Azure i usługi limity, przydziały i ograniczenia](../azure-subscription-service-limits.md).

## <a name="solution"></a>Rozwiązanie

### <a name="solution-1"></a>Rozwiązanie 1

Dla wiersza polecenia platformy Azure, użyj `az vm list-usage` polecenie, aby znaleźć przydziały maszyny wirtualnej.

```azurecli
az vm list-usage --location "South Central US"
```

Polecenie to zwraca:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

### <a name="solution-2"></a>Rozwiązanie 2

Dla programu PowerShell, użyj **Get AzureRmVMUsage** polecenie, aby znaleźć przydziały maszyny wirtualnej.

```powershell
Get-AzureRmVMUsage -Location "South Central US"
```

Polecenie to zwraca:

```powershell
Name                             Current Value Limit  Unit
----                             ------------- -----  ----
Availability Sets                            0  2000 Count
Total Regional Cores                         0   100 Count
Virtual Machines                             0 10000 Count
```

### <a name="solution-3"></a>Rozwiązanie 3

Aby zwiększyć limit przydziału, przejdź do portalu, a plik problem pomocy technicznej, aby podnieść limitu przydziału dla regionu, w której chcesz wdrożyć.

> [!NOTE]
> Należy pamiętać, że dla grup zasobów, limitu przydziału dla poszczególnych regionów, nie dla całej subskrypcji. Jeśli zajdzie potrzeba wdrożenia 30 rdzeni zachodnie stany USA, należy poprosić o 30 rdzeni Resource Manager zachodnie stany USA. Jeśli zajdzie potrzeba wdrożenia 30 rdzeni w poszczególnych regionach, do której masz dostęp, należy poprosić dla 30 rdzeni Resource Manager we wszystkich regionach.
>
>