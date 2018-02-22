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
ms.openlocfilehash: c1e490aacdc9f712c6e186e56fe35fd5872fb575
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
[Zasady usługi Azure](/azure/azure-policy/) firmowej standardów pomocy należy upewnić się, wszystkie zasoby w subskrypcji. Zasady umożliwiają zmniejszenie kosztów przez ograniczenie opcje wdrażania tych typów zasobów i jednostki magazynowe, które zostały zatwierdzone. Definiowanie reguł i akcji dla zasobów, a te reguły są automatycznie wprowadzane podczas wdrażania. Na przykład można kontrolować typów zasobów, które zostały wdrożone. Lub, można ograniczyć lokalizacje zatwierdzone dla zasobów. Niektóre zasady dotyczące akcji odmowy, a niektóre zasady dotyczące — Konfiguracja inspekcji akcji.

Zasady są uzupełniające do kontroli dostępu opartej na rolach (RBAC). RBAC koncentruje się na dostęp użytkowników, i jest domyślnie odmowy jawne i zezwolić systemu. Zasady koncentruje się na właściwości zasobu, podczas i po wdrożeniu. Jego domyślny Zezwalaj jawne i odmawianie systemu.

Istnieją dwa pojęć z zasadami — definicje zasad i przypisań zasad. Definicję zasad w tym artykule opisano warunków zarządzania, które mają zostać wymuszone. Przypisanie zasad umieszcza definicję zasad w akcji dla określonego zakresu.

![Przypisać zasady](./media/resource-manager-governance-policy/policy-concepts.png)

Platforma Azure udostępnia kilka definicje zasad wbudowanych, których można używać bez żadnych modyfikacji. Należy podać wartości parametrów, aby określić wartości, które są dozwolone w zakresie Twoich obowiązków. Jeśli definicji wbudowanych zasad nie spełniają wymagań, możesz [utworzyć zasady niestandardowe definicje](../articles/azure-policy/create-manage-policy.md).

### <a name="who-can-create-and-assign-policies"></a>Kto może utworzyć i przypisać zasady

Korzystanie z zasad wymaga uwierzytelnienia przy użyciu RBAC. Konkretnie dane konto wymaga:

* Uprawnienia `Microsoft.Authorization/policydefinitions/write` do definiowania zasad.
* Uprawnienia `Microsoft.Authorization/policyassignments/write` do przypisywania zasad.

Te uprawnienia nie są uwzględniane w **współautora** roli.
