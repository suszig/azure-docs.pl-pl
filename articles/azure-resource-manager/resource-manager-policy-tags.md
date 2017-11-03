---
title: "Zasady zasobów platformy Azure dla tagów | Dokumentacja firmy Microsoft"
description: "Przykłady zasad zasobów do zarządzania tagów do zasobów"
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: tomfitz
ms.openlocfilehash: 469bd8d637337e5900ea84c6bfaf88064695fb7e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="apply-resource-policies-for-tags"></a>Zastosuj zasady zasobów tagów

Ten temat zawiera wspólnych zasad zasady, które można zastosować, aby zapewnić spójne używanie tagów do zasobów.

Stosowanie zasad tag do grupy zasobów lub subskrypcji z istniejącymi zasobami nie dotyczy wstecznie zasady do tych zasobów. Aby wymusić zasady na tych zasobów, wyzwolenia aktualizacji do istniejących zasobów. Ten artykuł zawiera przykładowy środowiska PowerShell, służącą do wyzwalania aktualizacji.

## <a name="ensure-all-resources-in-a-resource-group-have-a-tagvalue"></a>Upewnij się, że wszystkie zasoby w grupie zasobów ma tag/wartości

Typowym wymaganiem jest wszystkie zasoby w grupie zasobów określony tag i wartość. To wymaganie jest często potrzebne do śledzenia kosztów działu. Muszą być spełnione następujące warunki:

* Wymaganego tagu i wartości są dołączane do nowych i zaktualizowanych zasobów, które nie mają znacznika.
* Nie można usunąć wymaganego tagu i wartości z żadnych istniejących zasobów.

To wymaganie można dodać, stosując dwa wbudowane zasady grupy zasobów.

| ID | Opis |
| ---- | ---- |
| 2a0e14a6-b0a6-4fab-991a-187a4f81c498 | Stosuje wymaganego tagu i wartość domyślną, gdy nie jest określona przez użytkownika. |
| 1e30110a-5ceb-460c-a204-c1c3969c6d62 | Wymusza wymaganego tagu i jej wartość. |

### <a name="powershell"></a>PowerShell

Poniższy skrypt programu PowerShell przypisuje dwie definicje wbudowanych zasad grupy zasobów. Przed uruchomieniem skryptu, Przypisz wszystkich wymaganych tagów do grupy zasobów. Każdy znacznik na grupy zasobów jest wymagana dla zasobów w grupie. Aby przypisać do wszystkich grup zasobów w ramach subskrypcji, nie zapewniają `-Name` parametr podczas pobierania grup zasobów.

```powershell
$appendpolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '2a0e14a6-b0a6-4fab-991a-187a4f81c498'}
$denypolicy = Get-AzureRmPolicyDefinition | Where-Object {$_.Name -eq '1e30110a-5ceb-460c-a204-c1c3969c6d62'}

$rgs = Get-AzureRMResourceGroup -Name ExampleGroup

foreach($rg in $rgs)
{
    $tags = $rg.Tags
    foreach($key in $tags.Keys){
        $key 
        $tags[$key]
        New-AzureRmPolicyAssignment -Name ("append"+$key+"tag") -PolicyDefinition $appendpolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
        New-AzureRmPolicyAssignment -Name ("denywithout"+$key+"tag") -PolicyDefinition $denypolicy -Scope $rg.ResourceId -tagName $key -tagValue  $tags[$key]
    }
}
```

Po przypisaniu zasad, użytkownik zainicjuje aktualizacji do wszystkich istniejących zasobów do wymuszania zasad tag, które zostały dodane. Poniższy skrypt zachowuje inne tagi, które istniały na zasobach:

```powershell
$group = Get-AzureRmResourceGroup -Name "ExampleGroup" 

$resources = Find-AzureRmResource -ResourceGroupName $group.ResourceGroupName 

foreach($r in $resources)
{
    try{
        $r | Set-AzureRmResource -Tags ($a=if($r.Tags -eq $NULL) { @{}} else {$r.Tags}) -Force -UsePatchSemantics
    }
    catch{
        Write-Host  $r.ResourceId + "can't be updated"
    }
}
```

## <a name="require-tags-for-a-resource-type"></a>Wymagaj tagi dla typu zasobu
Poniższy przykład pokazuje, jak można zagnieżdżać operatorów logicznych, aby wymagać tag aplikacji tylko typu określonego zasobu (w tym przypadku kont magazynu).

```json
{
    "if": {
        "allOf": [
          {
            "not": {
              "field": "tags",
              "containsKey": "application"
            }
          },
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          }
        ]
    },
    "then": {
        "effect": "audit"
    }
}
```

## <a name="require-tag"></a>Wymagaj tag
Następujące zasady odrzucanie żądań, które nie mają tag zawierający klucz "costCenter" (można zastosować dowolną wartość):

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

## <a name="next-steps"></a>Następne kroki
* Po zdefiniowaniu reguły zasad (jak pokazano w powyższych przykładach), należy utworzyć definicji zasad i przypisać je do zakresu. Zakres może być subskrypcji, grupy zasobów lub zasobów. Aby przypisać zasady za pośrednictwem portalu, zobacz [portal Azure używany do przypisywania i zarządzanie zasadami zasobów](resource-manager-policy-portal.md). Aby przypisać zasady za pomocą interfejsu API REST, programu PowerShell lub interfejsu wiersza polecenia Azure, zobacz [przypisania zasad i zarządzania nimi za pośrednictwem skryptu](resource-manager-policy-create-assign.md).
* Aby obejrzeć wprowadzenie do zasad zasobów, zobacz [Przegląd zasad zasobów](resource-manager-policy.md).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

