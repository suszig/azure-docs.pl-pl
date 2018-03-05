---
title: "Dodawanie warunków i uruchomić przepływ pracy - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Kontroluje sposób uruchamiania przepływów pracy w aplikacjach logiki platformy Azure, dodając logikę warunkową, wyzwalacze, akcje i parametry."
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: e4e24de4-049a-4b3a-a14c-3bf3163287a8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: estfan; LADocs
ms.openlocfilehash: cca13db916dc2560ff78d477a0faa241e032ab27
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/03/2018
---
# <a name="use-logic-apps-features"></a>Korzystanie z funkcji Logic Apps

W [poprzedniego tematu](../logic-apps/quickstart-create-first-logic-app-workflow.md), utworzyć pierwszą aplikację logiki. Aby kontrolować przepływu pracy aplikacji logiki, można określić różne ścieżki dla aplikacji logiki do uruchomienia i sposobu przetwarzania danych w macierzy, kolekcji i partie. W przepływie pracy aplikacji logiki można uwzględnić następujące elementy:

* Warunki i [przełącznika instrukcje](../logic-apps/logic-apps-switch-case.md) let aplikację logiki, uruchom różnych akcji w oparciu o Określa, czy zostały spełnione określone warunki.

* [Pętle](../logic-apps/logic-apps-loops-and-scopes.md) let aplikację logiki cyklicznie uruchamiany kroki. Na przykład można powtórzyć akcje dotyczące tablicy korzystając z **For_each** pętli. Lub akcji można powtórzyć, dopóki spełniony jest warunek, korzystając z **do momentu** pętli.

* [Zakresy](../logic-apps/logic-apps-loops-and-scopes.md) let uruchamianym serii akcji, na przykład, aby zaimplementować obsługi wyjątków.

* [Debatching](../logic-apps/logic-apps-loops-and-scopes.md) pozwala uruchomić oddzielne przepływy pracy dla elementów w tablicy, korzystając z aplikacji logiki **SplitOn** polecenia.

W tym temacie przedstawiono inne pojęć dotyczących tworzenia aplikacji logiki:

* Kod widoku do edycji istniejących aplikacji logiki
* Opcje uruchamiania przepływu pracy

## <a name="conditions-run-steps-only-after-meeting-a-condition"></a>Warunki: Uruchamianie kroków tylko po spełniających warunek

Aby aplikację logiki, uruchom kroki tylko wtedy, gdy dane spełniają określone kryteria, można dodać warunek, który porównuje dane w przepływie pracy przed określonych pól lub wartości.

Na przykład załóżmy, że masz aplikację logiki, która wysyła zbyt dużo żądań wiadomości e-mail Posty na kanału informacyjnego RSS witryny sieci Web. Tak, aby aplikację logiki wysyła wiadomości e-mail, tylko wtedy, gdy nowy wpis należy do jednej konkretnej kategorii można dodać warunek.

1. W [portalu Azure](https://portal.azure.com), znajdowanie i otwieranie aplikacji logiki w Projektancie aplikacji logiki.

2. Dodaj warunek do dowolnej lokalizacji przepływu pracy. 

   Aby dodać warunek między krokami istniejących w przepływie pracy aplikacji logiki, wskaźnika na strzałkę której chcesz dodać warunek. 
   Wybierz **znak plus** (**+**), a następnie wybierz **Dodaj warunek**. Na przykład:

   ![Dodaj warunek do aplikacji logiki](./media/logic-apps-use-logic-app-features/add-condition.png)

   > [!NOTE]
   > Jeśli chcesz dodać warunek na koniec bieżącego przepływu pracy, przejdź do dolnej części aplikacji logiki i wybierz **+ nowy krok**.

3. Teraz można określić warunku. Określ pola źródłowego, który chcesz ocenić, operacji do wykonania i wartość docelowa lub pola. Aby dodać warunek istniejących pól, wybierz z **listy zawartości dynamicznej Dodaj**.

   Na przykład:

   ![Edytuj warunek w trybie podstawowym](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode.png)

   Oto pełny warunek:

   ![Pełny warunek](./media/logic-apps-use-logic-app-features/edit-condition-basic-mode-2.png)

   > [!TIP]
   > Aby zdefiniować warunku w kodzie, wybierz **edytowanie w trybie zaawansowanym**. Na przykład:
   > 
   > ![Edytuj warunek w kodzie](./media/logic-apps-use-logic-app-features/edit-condition-advanced-mode.png)

4. W obszarze **tak, jeśli** i **nr IF**, dodaj kroki do wykonania oparte na Określa, czy warunek jest spełniony.

   Na przykład:

   ![Stan tak i ścieżek](./media/logic-apps-use-logic-app-features/condition-yes-no-path.png)

   > [!TIP]
   > Możesz przeciągnąć istniejących działań do **tak, jeśli** i **nr IF** ścieżki.

5. Gdy wszystko będzie gotowe, Zapisz aplikację logiki.

Teraz możesz otrzymywać wiadomości e-mail, tylko wtedy, gdy wpisów spełnienia warunku.

## <a name="repeat-actions-over-a-list-with-foreach"></a>Powtórz akcje listy za pomocą instrukcji forEach

Pętli forEach Określa tablicę powtórzenie akcji za pośrednictwem. Jeśli nie jest tablicą, przepływ zakończy się niepowodzeniem. Na przykład jeśli masz action1, który generuje tablicę komunikatów i chcesz wysłać wiadomości, można uwzględnić tej instrukcji forEach we właściwościach akcji: `forEach : "@action('action1').outputs.messages"`

## <a name="edit-the-code-definition-for-a-logic-app"></a>Edytowanie definicji kodu dla aplikacji logiki

Mimo że Projektant aplikacji logiki, można bezpośrednio edytować kod, który definiuje aplikacji logiki.

1. Na pasku poleceń Wybierz **widok Kod**.

    Pełna Edytor otwiera się i przedstawiono definicję edytowanej.

    ![Widok kodu](media/logic-apps-use-logic-app-features/codeview.png)

    W edytorze tekstu można skopiuj i Wklej dowolną liczbę akcji w obrębie tej samej aplikacji logiki lub między aplikacji logiki. 
    Można również łatwo Dodaj lub Usuń całą sekcję z definicji i definicji mogą również współużytkować z innymi osobami.

2. Aby zapisać zmiany, wybierz **zapisać**.

## <a name="parameters"></a>Parametry

Niektóre z funkcji Logic Apps są dostępne tylko w widoku kodu, na przykład parametry. Parametry ułatwiają ponowne użycie wartości w całej aplikacji logiki. Na przykład jeśli masz adres e-mail, która ma zostać użyta w kilka akcji, należy zdefiniować tego adresu e-mail jako parametr.

Parametry są odpowiednie w ściąganie wartości, które mogą znacznie zmieniają. Są szczególnie przydatne, gdy należy zastąpić parametry w różnych środowiskach. Aby dowiedzieć się, jak zastąpić parametry opartych na środowisku, zobacz [Tworzenie definicji aplikacji logiki](../logic-apps/logic-apps-author-definitions.md) i [dokumentacja interfejsu API REST](https://docs.microsoft.com/rest/api/logic).

Ten przykład przedstawia sposób aktualizacji istniejących aplikacji logiki, dzięki czemu można używać parametrów dla wyszukiwanego terminu.

1. W widoku kodu, Znajdź `parameters : {}` obiekt, a następnie dodaj `currentFeedUrl` obiektu:

        "currentFeedUrl" : {
            "type" : "string",
            "defaultValue" : "http://rss.cnn.com/rss/cnn_topstories.rss"
        }

2. Przejdź do `When_a_feed-item_is_published` akcji, Znajdź `queries` sekcji, a wartość kwerendy: `"feedUrl": "#@{parameters('currentFeedUrl')}"` 

    Aby przyłączyć się co najmniej dwa ciągi, umożliwia także `concat` funkcji. 
    Na przykład `"@concat('#',parameters('currentFeedUrl'))"` działa tak samo jak powyżej.

3.  Gdy wszystko będzie gotowe, wybierz pozycję **Zapisz**. 

    Teraz możesz zmienić danych RSS witryny sieci Web przez przekazanie do innego adresu URL za pośrednictwem `currentFeedURL` obiektu.

Dowiedz się więcej o [sposobu tworzenia definicji aplikacji logiki](../logic-apps/logic-apps-author-definitions.md).

## <a name="start-logic-app-workflows"></a>Uruchomić przepływy pracy aplikacji logiki

Masz różne opcje uruchamiania przepływu pracy zdefiniowanych w aplikacji logiki. Zawsze można uruchomić przepływu pracy na żądanie w [portalu Azure].

### <a name="recurrence-triggers"></a>Wyzwalacze cyklu

Uruchamia wyzwalacz cyklu w odstępach czasu, który określisz. Jeśli wyzwalacz logikę warunkową, wyzwalacz Określa, czy przepływ pracy musi być uruchamiane. Wskazuje wyzwalacz przepływu pracy powinno być ono uruchomione przez zwrócenie `200` kod stanu. Gdy przepływ pracy nie trzeba uruchamiać, wyzwalacz zwraca `202` kod stanu.

### <a name="callback-using-rest-apis"></a>Wywołanie zwrotne za pomocą interfejsów API REST

Aby uruchomić przepływ pracy, usługi mogą wywoływać punkt końcowy aplikacji logiki. Aby uruchomić ten rodzaj logiki aplikacji na żądanie, wybierz **Uruchom teraz** na pasku poleceń. Zobacz [uruchomić przepływy pracy, wywołując logiki punkty końcowe aplikacji jako wyzwalacze](../logic-apps/logic-apps-http-endpoint.md). 

<!-- Shared links -->
[portalu Azure]: https://portal.azure.com

## <a name="next-steps"></a>Kolejne kroki

* [Instrukcji Switch](../logic-apps/logic-apps-switch-case.md) 
* [Pętle, zakresy i usuwanie partii](../logic-apps/logic-apps-loops-and-scopes.md)
* [Tworzenie definicji aplikacji logiki](../logic-apps/logic-apps-author-definitions.md)