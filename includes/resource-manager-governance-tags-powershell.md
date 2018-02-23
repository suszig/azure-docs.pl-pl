---
title: Plik dyrektywy include
description: Plik dyrektywy include
services: azure-resource-manager
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: include
ms.date: 02/16/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: 87b9bd74fc59c86bf177e45c18bfc4e104d9c996
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
Aby dodać dwa tagi do grupy zasobów, należy użyć:

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ Dept="IT"; Environment="Test" }
```

Załóżmy, że chcesz dodać tag trzecich. Za każdym razem, gdy stosujesz tagi do zasobu lub grupy zasobów, istniejące tagi tego zasobu lub tej grupy zasobów są zastępowane. Aby dodać nowy znacznik bez utraty istniejących tagi, należy pobrać istniejące znaczniki, Dodaj nowy znacznik i zastosuj je ponownie zbiór znaczników:

```powershell
$tags = (Get-AzureRmResourceGroup -Name myResourceGroup).Tags
$tags += @{Project="Documentation"}
Set-AzureRmResourceGroup -Tag $tags -Name myResourceGroup
```

Zasoby nie dziedziczy tagi grupy zasobów. Obecnie grupa zasobów zawiera trzy znaczniki, ale zasoby nie ma żadnych znaczników. Aby zastosować wszystkie tagi z grupy zasobów, do jej zasobów i zachować istniejące znaczniki na zasoby, które nie są duplikatami, użyj następującego skryptu:

```powershell
$group = Get-AzureRmResourceGroup myResourceGroup
if ($group.Tags -ne $null) {
    $resources = $group | Find-AzureRmResource
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
        foreach ($key in $group.Tags.Keys)
        {
            if (($resourcetags) -AND ($resourcetags.ContainsKey($key))) { $resourcetags.Remove($key) }
        }
        $resourcetags += $group.Tags
        Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
    }
}
```

Alternatywnie można zastosować tagi z grupy zasobów do zasobów bez zachować istniejące znaczniki:

```powershell
$g = Get-AzureRmResourceGroup -Name myResourceGroup
Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
```

Aby połączyć kilka wartości w jeden tag, użyj ciągu JSON.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ CostCenter="{`"Dept`":`"IT`",`"Environment`":`"Test`"}" }
```

Aby usunąć wszystkie tagi, należy przekazać tablicy skrótów puste.

```powershell
Set-AzureRmResourceGroup -Name myResourceGroup -Tag @{ }
```
