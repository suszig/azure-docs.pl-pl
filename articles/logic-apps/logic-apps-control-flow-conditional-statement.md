---
title: "Warunkowe instrukcje - uruchamianie kroków na podstawie warunku - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Wykonanie kroków w aplikacji logiki tylko, po spełnieniu warunku. Utwórz drzewa decyzyjne, które uruchamiania przepływów pracy na podstawie określonych warunków."
services: logic-apps
keywords: warunkowe instrukcje, drzew decyzyjnych
documentationcenter: 
author: ecfan
manager: anneta
editor: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 486c1053f42ed3becc2c4b60accc993db7f24baa
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="conditional-statements-run-steps-based-on-a-condition-in-logic-apps"></a>Warunkowe instrukcje: wykonanie kroków na podstawie warunku w aplikacjach logiki

Aby wykonać kroki tylko po przekazywanie określony warunek, należy użyć *instrukcji warunkowej*. Ta struktura porównuje dane w przepływie pracy przed określone wartości lub pól. Można zdefiniować wykonania różnych kroków do czy danych spełnia warunek. Można zagnieżdżać warunków wewnątrz siebie nawzajem.

Na przykład załóżmy, że masz aplikację logiki, która wysyła zbyt dużo żądań wiadomości e-mail, gdy nowe elementy wyświetlane na kanału informacyjnego RSS witryny sieci Web. Możesz dodać instrukcji warunkowej do wysyłania wiadomości e-mail, tylko wtedy, gdy nowy element zawiera określony ciąg znaków. 

> [!TIP]
> Aby wykonać inne czynności na podstawie różnych określonej wartości, należy użyć [ *instrukcja switch* ](../logic-apps/logic-apps-control-flow-switch-statement.md) zamiast tego.

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, [zarejestruj się w celu założenia bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/).

* Podstawową wiedzę na temat o [sposób tworzenia aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md)

* Na przykład w tym artykule, wykonaj [tworzenie aplikacji logiki tej próbki](../logic-apps/quickstart-create-first-logic-app-workflow.md) przy użyciu konta usługi Outlook.com lub Office 365 Outlook.

## <a name="add-a-condition"></a>Dodaj warunek

1. W <a href="https://portal.azure.com" target="_blank">portalu Azure</a>, Otwórz aplikację logiki w Projektancie aplikacji logiki.

2. Dodaj warunek w dowolnej lokalizacji. 

   Aby dodać warunek między krokami, wskaźnika na strzałkę której chcesz dodać warunek. Wybierz **znak plus** (**+**) wyświetlony, a następnie wybierz **Dodaj warunek**. Na przykład:

   ![Dodaj warunek między krokami](./media/logic-apps-control-flow-conditional-statement/add-condition.png)

   Jeśli chcesz dodać warunek na końcu przepływu pracy, w dolnej części aplikacji logiki, wybierz **+ nowy krok** > **Dodaj warunek**.

3. W obszarze **warunku**, Utwórz warunku. 

   1. W polu po lewej stronie Określ dane lub pola, które chcesz porównać.

      Z **dodać zawartość dynamiczną** listy, możesz wybrać istniejące pola z aplikacji logiki.

   2. W środkowym listy wybierz operacji do wykonania. 
   3. W prawym polu Określ wartość lub pola kryterium.

   Na przykład:

   ![Edytuj warunek w trybie podstawowym](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode.png)

   Oto pełny warunek:

   ![Pełny warunek](./media/logic-apps-control-flow-conditional-statement/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Aby utworzyć warunek bardziej zaawansowanych lub za pomocą wyrażeń, wybierz **edytowanie w trybie zaawansowanym**. Można użyć wyrażenia zdefiniowane przez [język definicji przepływu pracy](../logic-apps/logic-apps-workflow-definition-language.md).
   > 
   > Na przykład:
   >
   > ![Edytuj warunek w kodzie](./media/logic-apps-control-flow-conditional-statement/edit-condition-advanced-mode.png)

5. W obszarze **tak, jeśli** i **nr IF**, dodaj kroki do wykonania oparte na Określa, czy warunek jest spełniony. Na przykład:

   ![Stan tak i ścieżek](./media/logic-apps-control-flow-conditional-statement/condition-yes-no-path.png)

   > [!TIP]
   > Możesz przeciągnąć istniejących działań do **tak, jeśli** i **nr IF** ścieżki.

6. Zapisz aplikację logiki.

Teraz tej aplikacji logiki tylko wysyła poczty, gdy nowe elementy do źródła danych RSS spełniają warunek.

## <a name="json-definition"></a>Definicja formatu JSON

Teraz, gdy utworzono aplikację logiki, za pomocą instrukcji warunkowej, Przyjrzyjmy się definicję wysokiego poziomu kodu za instrukcji warunkowej.

``` json
"actions": {
  "myConditionName": {
    "type": "If",
    "expression": "@contains(triggerBody()?['summary'], 'Microsoft')",
    "actions": {
      "Send_an_email": {
        "inputs": { },
        "runAfter": {}
      }
    },
    "runAfter": {}
  }
},
```

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania, odwiedź [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Aby przesłać lub oddawać głosy na funkcje i sugestie, odwiedź stronę [witrynę opinii użytkowników usługi Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

* [Wykonanie kroków na podstawie różnych wartości (instrukcji switch)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uruchom i powtórz kroki (pętle)](../logic-apps/logic-apps-control-flow-loops.md)
* [Uruchom lub scalania czynności równoległe (gałęzi)](../logic-apps/logic-apps-control-flow-branches.md)
* [Wykonanie kroków na podstawie stanu akcji grupowanych (zakresy)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)