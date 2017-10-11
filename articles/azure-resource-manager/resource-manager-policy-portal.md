---
title: "Portal Azure — zasady zasobów | Dokumentacja firmy Microsoft"
description: "Opisuje sposób portalu Azure umożliwia tworzenie i zarządzanie zasadami Menedżera zasobów. Zasady można stosować na grupy zasobów lub subskrypcji."
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
ms.date: 03/30/2017
ms.author: tomfitz
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a>Przypisz i zarządzanie zasadami zasobów za pomocą portalu Azure
Azure portal umożliwia przypisanie zasad zasobów do grupy zasobów i subskrypcje. Interfejs użytkownika ułatwia wybierz zasady, które chcesz przypisać i podaj wartości parametrów dla tej zasady dostosować ustawienia zasad. 

Aby przypisać zasad za pośrednictwem portalu, definicji zasad musi już istnieć w ramach subskrypcji. Subskrypcja obejmuje kilka definicje wbudowanych zasad, które są gotowe do przypisania do grupy zasobów lub subskrypcji. Widzisz tych zasad wbudowanych i niestandardowych zasad, zdefiniowanych w przypadku użycia portalu do przypisywania zasad. Aby obejrzeć wprowadzenie do zasad i sposób definiowania zasad niestandardowych, zobacz [Przegląd zasad zasobów](resource-manager-policy.md).

Zasady są dziedziczone przez wszystkie zasoby podrzędne. Tak Jeśli zasady są stosowane do grupy zasobów, ma zastosowanie do wszystkich zasobów w danej grupie zasobów. W tym artykule określenie **zakres** odwołuje się do grupy zasobów lub subskrypcji, która jest przypisana zasad. 

Zasady są oceniane podczas tworzenia i aktualizowania zasobów (PUT i poprawka operacji).

## <a name="assign-a-policy"></a>Przypisać zasady

1. Aby przypisać zasady do grupy zasobów lub subskrypcji, wybierz tej grupy zasobów lub subskrypcji. W ustawieniach, wybierz **zasad**.

   ![Wybieranie zasad](./media/resource-manager-policy-portal/select-policies.png)

2. Aby utworzyć przypisanie zasad dla tego zakresu, wybierz **Dodaj przypisanie**.

   ![Dodaj przypisania](./media/resource-manager-policy-portal/add-assignment.png)

3. Wybierz zasady, którą chcesz przypisać. Nawet jeśli nie dodano żadnych definicji zasad do subskrypcji, zobaczysz wbudowanych zasad, które są dostępne do przypisania. Zasady te wbudowane obejmują wiele typowych scenariuszy.

   ![Wybierz definicję](./media/resource-manager-policy-portal/select-definition.png)

4. Po wybraniu zasad, zobacz temat Opis zasad i wszelkie parametry dla tej zasady. Na przykład poniższy obraz przedstawia **dozwolone lokalizacje** parametr, który jest wymagany dla zasad, która ogranicza dostępnych lokalizacji.

   ![Pokaż parametry](./media/resource-manager-policy-portal/show-parameters.png)

5. Za pomocą interfejsu użytkownika wybierz wartości w celu określenia parametrów zasad (np. lokalizacje, które można użyć do wdrożenia).

   ![Wybierz wartości parametrów](./media/resource-manager-policy-portal/select-parameters.png)

6. Należy podać wartości innych parametrów. Zakres jest automatycznie przypisywany oparte na wybranej podczas uruchamiania przypisania zasad bloku. Po zakończeniu wybierz przycisk **OK**.

   ![Zdefiniuj parametry](./media/resource-manager-policy-portal/define-parameters.png)

  Zasady zostały przypisane do określonego zakresu.

## <a name="view-policy-assignments"></a>Wyświetl przypisania zasad

Po przypisaniu zasad, zobacz na liście zasad dla tego zakresu. **Szczegóły** karta zawiera podsumowanie przypisania zasad.

![Pokaż szczegóły](./media/resource-manager-policy-portal/show-details.png)

**Zasada przypisania** karcie są wyświetlane JSON dla definicji zasad.

![Pokaż reguły przypisania](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Zmień istniejące przypisania zasad

Aby zmienić zasady, wybierz **edytować przypisanie** lub **usunąć**

![edytowanie lub usuwanie przypisania](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Przypisanie zasad niestandardowych

Jeśli zasady niestandardowe zdefiniowane w ramach subskrypcji, te zasady są dostępne do przypisania za pośrednictwem portalu. Te zasady są poprzedzone znakiem **[Custom]**

![zasady niestandardowe](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Następne kroki
* Informacje na temat składni JSON do definiowania zasad, zobacz [Przegląd zasad zasobów](resource-manager-policy.md).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).
* Schemat zasad jest opublikowana w [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

