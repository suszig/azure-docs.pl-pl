---
title: "Zasady zasobów platformy Azure dla konwencji nazewnictwa | Dokumentacja firmy Microsoft"
description: "Opisuje zasady usługi Azure Resource Manager dla konwencji nazewnictwa zasobów."
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
ms.date: 06/27/2017
ms.author: tomfitz
ms.openlocfilehash: 51b3519bbba8cb4c768bfdd7dadf92fced434f22
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="apply-resource-policies-for-names-and-text"></a>Zastosuj zasady zasobów dla nazwy i tekst
W tym temacie przedstawiono kilka [zasad zasobów](resource-manager-policy.md) można zastosować do ustalenia konwencje nazewnictwa i tekst. Te zasady zapewnienia spójności dla nazwy zasobów i wartości tagów. 

## <a name="set-naming-convention-with-wildcard"></a>Ustaw konwencję nazewnictwa z symbolem wieloznacznym
W poniższym przykładzie przedstawiono użycie symbolu wieloznacznego, który jest obsługiwany przez **jak** warunku. Warunek stwierdza, że jeśli nazwa jest zgodna z wzorcem opisane powyżej (namePrefix\*nameSuffix) odrzuciła żądanie, następnie:

```json
{
  "if": {
    "not": {
      "field": "name",
      "like": "namePrefix*nameSuffix"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-naming-convention-with-pattern"></a>Ustaw konwencję nazewnictwa ze wzorcem

Aby określić, że nazwy zasobów zgodne ze wzorcem, użyj warunku dopasowania. Poniższy przykład wymaga do uruchomienia z nazw `contoso` i zawierać sześciu liter dodatkowe:

```json
{
  "if": {
    "not": {
      "field": "name",
      "match": "contoso??????"
    }
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="set-date-pattern-for-tag-value"></a>Ustaw datę wzorzec wartości tagu

Aby wymagać wzorzec Data użyj dwie cyfry, kreski trzech liter, kreska i cztery cyfry:

```json
{
  "if": {
    "field": "tags.date",
    "match": "##-???-####"
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="next-steps"></a>Następne kroki
* Po zdefiniowaniu reguły zasad (jak pokazano w powyższych przykładach), należy utworzyć definicji zasad i przypisać je do zakresu. Zakres może być subskrypcji, grupy zasobów lub zasobów. Aby przypisać zasady za pośrednictwem portalu, zobacz [portal Azure używany do przypisywania i zarządzanie zasadami zasobów](resource-manager-policy-portal.md). Aby przypisać zasady za pomocą interfejsu API REST, programu PowerShell lub interfejsu wiersza polecenia Azure, zobacz [przypisania zasad i zarządzania nimi za pośrednictwem skryptu](resource-manager-policy-create-assign.md). 
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

