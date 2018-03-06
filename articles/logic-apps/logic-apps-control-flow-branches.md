---
title: "Równoległe gałęzie - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Utwórz lub przyłącz równoległych gałęziach w aplikacjach logiki"
services: logic-apps
keywords: "gałęzie, przetwarzanie równoległe"
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
ms.openlocfilehash: 9a836b707a576b9a938f43397ef35c00aeb476bf
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2018
---
# <a name="create-or-join-parallel-branches-in-your-logic-app"></a>Utwórz lub przyłącz równoległych gałęziach w aplikacji logiki

Domyślnie akcje w aplikacji logiki są wykonywane sekwencyjnie. Aby wykonać akcje niezależne, w tym samym czasie, można utworzyć [równoległych gałęziach](#parallel-branches), a następnie [join gałęzi](#join-branches) dalszej części z przepływu. 

> [!TIP] 
> Jeśli masz wyzwalacz, który odbiera tablicy i chcesz uruchomić przepływ pracy dla każdego elementu tablicy, możesz *debatch* tablicy z [ **SplitOn** wyzwolenia właściwości](../logic-apps/logic-apps-workflow-actions-triggers.md#split-on-debatch).

## <a name="prerequisites"></a>Wymagania wstępne

* Subskrypcja platformy Azure. Jeśli nie masz subskrypcji, [zarejestruj się w celu założenia bezpłatnego konta platformy Azure](https://azure.microsoft.com/free/). 

* Podstawową wiedzę na temat o [sposób tworzenia aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="parallel-branches"></a>

## <a name="add-a-parallel-branch"></a>Dodaj gałąź równoległych

Wykonanie kroków niezależne, w tym samym czasie, możesz dodać równoległych gałęziach obok istniejących kroku. 

![Wykonanie kroków równolegle](media/logic-apps-control-flow-branches/parallel.png)

Aplikację logiki czeka na wszystkie gałęzie zakończyć działanie przed kontynuowaniem przepływu pracy.
Równoległe gałęzie Uruchom tylko wtedy, gdy ich `runAfter` Stan kroku Zakończono nadrzędnej odpowiada wartości właściwości. Na przykład zarówno `branchAction1` i `branchAction2` są ustawione na uruchamianie tylko wtedy, gdy `parentAction` zakończeniu z `Succeded` stanu.

> [!NOTE]
> Przed rozpoczęciem aplikację logiki musi mieć już krok, którym można dodać równoległych gałęziach.

1. W <a href="https://portal.azure.com" target="_blank">portalu Azure</a>, Otwórz aplikację logiki w Projektancie aplikacji logiki.

2. Myszą strzałkę powyżej kroku gdzie chcesz dodać równoległych gałęziach.

3. Wybierz **plus** logowania (**+**), wybierz **Dodaj gałąź równoległych**i wybierz element, który chcesz dodać.

   ![Dodaj gałąź równoległych](media/logic-apps-control-flow-branches/add-parallel-branch.png)

   Pojawi się z wybranego elementu w gałęzi równoległej.

4. Dla każdej gałęzi równoległe Dodaj czynności, które mają. Aby dodać kolejne akcji do równoległej gałęzi, przenieś wskaźnik myszy w obszarze Akcja której chcesz dodać kolejne działania. Wybierz **plus** (**+**) logowania i krok, który chcesz dodać.

   ![Dodaj kolejny krok do gałęzi równoległych](media/logic-apps-control-flow-branches/add-sequential-action-parallel-branch.png)

5. Do scalenia gałęzi wstecz razem [join równoległych gałęziach](#join-branches). 

<a name="parallel-json"></a>

## <a name="parallel-branch-definition-json"></a>Definicja równoległej gałęzi (JSON)

Podczas pracy w widoku kodu można zdefiniować równoległych struktury w definicji JSON aplikację logiki, na przykład:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

<a name="join-branches"></a>

## <a name="join-parallel-branches"></a>Dołącz równoległych gałęziach

Aby scalić równoległych gałęziach ze sobą, Dodaj krok na dole wszystkie gałęzie. Ten krok działa po wszystkich równoległych gałęziach zakończenia uruchamiania.

![Dołącz równoległych gałęziach](media/logic-apps-control-flow-branches/join.png)

1. W [portalu Azure](https://portal.azure.com), znajdowanie i otwieranie aplikacji logiki w Projektancie aplikacji logiki. 

2. W obszarze równoległych gałęziach, które chcesz dołączyć Dodaj krok, który chcesz wykonać.

   ![Dodaj krok, na której jest przyłączany równoległych gałęziach](media/logic-apps-control-flow-branches/join-steps.png)

   Równoległych gałęziach są teraz scalone.

<a name="join-json"></a>

## <a name="join-definition-json"></a>Dołącz do definicji (JSON)

Podczas pracy w widoku kodu, można zdefiniować struktury sprzężenia w definicji JSON aplikację logiki, na przykład:

``` json
{
  "triggers": {
    "myTrigger": { }
  },
  "actions": {
    "parentAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {}
    },
    "branchAction1": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "branchAction2": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "parentAction": [
          "Succeeded"
        ]
      }
    },
    "joinAction": {
      "type": "<action-type>",
      "inputs": { },
      "runAfter": {
        "branchAction1": [
          "Succeeded"
        ],
        "branchAction2": [
          "Succeeded"
        ]
      }
    }
  },
  "outputs": {}
}
```

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania, odwiedź [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Aby przesłać lub oddawać głosy na funkcje i sugestie, odwiedź stronę [witrynę opinii użytkowników usługi Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

* [Wykonanie kroków na podstawie warunku (warunkowe instrukcje)](../logic-apps/logic-apps-control-flow-conditional-statement.md)
* [Wykonanie kroków na podstawie różnych wartości (instrukcji switch)](../logic-apps/logic-apps-control-flow-switch-statement.md)
* [Uruchom i powtórz kroki (pętle)](../logic-apps/logic-apps-control-flow-loops.md)
* [Wykonanie kroków na podstawie stanu akcji grupowanych (zakresy)](../logic-apps/logic-apps-control-flow-run-steps-group-scopes.md)