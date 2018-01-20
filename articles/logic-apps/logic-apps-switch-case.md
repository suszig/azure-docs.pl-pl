---
title: "Switch — instrukcja dla różnych akcji w aplikacjach logiki platformy Azure | Dokumentacja firmy Microsoft"
description: "Wybierz różne akcje do wykonania w aplikacjach logiki na podstawie wyrażenia wartości przy użyciu instrukcji switch"
services: logic-apps
keywords: "Switch — instrukcja"
author: derek1ee
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: LADocs; deli
ms.openlocfilehash: a459fb131cb9f917f2b0cf79679b04da48d76c42
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="perform-different-actions-in-logic-apps-with-a-switch-statement"></a>Wykonaj różne akcje w aplikacjach logiki z instrukcji switch

Podczas tworzenia przepływu pracy, często trzeba wykonywać różne akcje na podstawie wartości obiektu lub wyrażenie. Na przykład można aplikację logiki zachowuje się inaczej w zależności od kodu stanu żądania HTTP lub opcji wybranej w wiadomości e-mail.

Instrukcja switch służy do wdrożenia tych scenariuszy. Aplikację logiki można ocenić tokenu lub wyrażenie i wybrać sprawę z taką samą wartość do wykonywania określonych akcji. Tylko jeden przypadek powinna odpowiadać instrukcji switch.

> [!TIP]
> Podobnie jak wszystkie języki programowania instrukcji switch obsługuje tylko operatory równości. Jeśli potrzebujesz innych operatory relacyjne takich jak "większe niż", użyj instrukcji warunku.
> W celu zapewnienia sposób wykonywania deterministyczna, przypadków musi zawierać wartość unikatowa i statyczne zamiast tokeny dynamicznych lub wyrażenie.

## <a name="prerequisites"></a>Wymagania wstępne

- Aktywna subskrypcja platformy Azure. Jeśli nie masz aktywnych subskrypcji platformy Azure, [utworzyć bezpłatne konto](https://azure.microsoft.com/free/), lub spróbuj [wolne aplikacje logiki dla](https://tryappservice.azure.com/).
- [Podstawową wiedzę na temat dotyczące aplikacji logiki](logic-apps-overview.md)

## <a name="add-a-switch-statement-to-your-workflow"></a>Dodawanie instrukcji switch do przepływu pracy

Aby pokazać, jak działa instrukcji switch, w tym przykładzie jest tworzony aplikacji logiki, który monitoruje plików przekazanych do skrzynki. Po przekazaniu nowe pliki aplikacji logiki wysyła wiadomości e-mail do osoby zatwierdzającej, który wybiera, czy do przesyłania tych plików w programie SharePoint. Aplikacja używa instrukcji switch, który wykonuje różne akcje na podstawie wartości, który wybiera osoby zatwierdzającej.

1. Tworzenie aplikacji logiki, a następnie wybierz tego wyzwalacza: **Dropbox — po utworzeniu pliku**.

   ![Użyj usługi Dropbox — podczas tworzenia pliku wyzwalacza](./media/logic-apps-switch-case/dropbox-trigger.jpg)

2. W obszarze wyzwalacza, Dodaj tę akcję: **Outlook.com — wysyłanie wiadomości e-mail zatwierdzania**

   > [!TIP]
   > Aplikacje logiki obsługują także wysyłania scenariusze wiadomości e-mail zatwierdzania z konta programu Outlook pakietu Office 365.

   - Jeśli nie masz istniejące połączenie, zostanie wyświetlony monit o utworzenie.
   - Wypełnij wymagane pola. Na przykład w obszarze **do**, określ adres e-mail do wysyłania wiadomości e-mail osoby zatwierdzającej.
   - W obszarze **opcji użytkownika**, wprowadź `Approve, Reject`.

   ![Skonfiguruj połączenie](./media/logic-apps-switch-case/send-approval-email-action.jpg)

3. Dodawanie instrukcji switch.

   - Wybierz **+ nowy krok** > **... Więcej** > **dodawanie przypadku przełącznika**. 
   - Na podstawie teraz chcemy wybrać akcję do wykonania `SelectedOptions` dane wyjściowe z *wysyłania wiadomości e-mail zatwierdzania* akcji. 
   Możesz znaleźć tego pola w **dodać zawartość dynamiczną** selektora.
   - Użyj *przypadek 1* do obsługi po wybraniu osoby zatwierdzającej `Approve`.
     - Po zatwierdzeniu skopiować oryginalnego pliku do usługi SharePoint Online z [ **usługi SharePoint Online — Utwórz plik** akcji](../connectors/connectors-create-api-sharepointonline.md).
     - Dodaj inną akcję w przypadku aby powiadomić użytkowników, że nowy plik jest dostępny w programie SharePoint.
   - Dodaj inny przypadek do obsługi, gdy użytkownik wybierze `Reject`.
     - W przypadku odrzucenia, Wyślij wiadomość e-mail z powiadomieniem innych osób zatwierdzających dowie się, że plik został odrzucony, a nie są wymagane nie dalsze akcje.
   - `SelectedOptions`udostępnia tylko dwie opcje, aby firma Microsoft może narazić **domyślne** przypadku puste.

   ![Switch — instrukcja](./media/logic-apps-switch-case/switch.jpg)

   > [!NOTE]
   > Instrukcja switch musi mieć co najmniej jeden przypadek oprócz przypadek domyślny.

4. Po instrukcji switch usunąć oryginalny plik przekazany do skrzynki przez dodanie tej akcji: **Dropbox - Usuń plik**

5. Zapisz aplikację logiki. Testowanie aplikacji, przekazując plik do skrzynki. Wkrótce otrzymasz wiadomości e-mail zatwierdzania. Wybierz opcję, a przyjrzeć się zachowaniu.

   > [!TIP]
   > Sprawdź informacje dotyczące sposobu [Monitoruj aplikacje logiki](logic-apps-monitor-your-logic-apps.md).

## <a name="understand-the-code-behind-switch-statements"></a>Zrozumienie kodzie instrukcji switch

Teraz, że pomyślnie utworzono aplikację logiki, za pomocą instrukcji switch, Przyjrzyjmy się definicji kodu za instrukcji switch.

```json
"Switch": {
    "type": "Switch",
    "expression": "@body('Send_approval_email')?['SelectedOption']",
    "cases": {
        "Case 1" : {
            "case" : "Approved",
            "actions" : {}
        },
        "Case 2" : {
            "case" : "Rejected",
            "actions" : {}
        }
    },
    "default": {
        "actions": {}
    },
    "runAfter": {
        "Send_approval_email": [
            "Succeeded"
        ]
    }
}
```

* `"Switch"`to nazwa instrukcji switch, które można zmienić nazwy dla czytelności. 
* `"type": "Switch"`Wskazuje, czy akcja jest instrukcji switch. 
* `"expression"`jest osoby zatwierdzającej w tym przykładzie wybrano opcję i zostaną ocenione względem każdego przypadku zadeklarowany później w definicji. 
* `"cases"`może zawierać dowolną liczbę przypadków. W przypadku każdego `"Case *"` to domyślna nazwa przypadek, który można zmienić nazwy dla czytelności. 
`"case"`Określa etykiety case, która używa wyrażenie switch do porównania, i musi być wartością stałą i unikatowe. Jeśli żaden z przypadków zgodne wyrażenie switch akcje w ramach `"default"` są wykonywane.

## <a name="get-help"></a>Uzyskiwanie pomocy

Aby zadawać pytania, odpowiadać na nie i patrzeć, co robią inni użytkownicy usługi Azure Logic Apps, odwiedź [forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Aby pomóc w ulepszaniu usługi Azure Logic Apps, przesyłaj pomysły lub głosuj na nie w [witrynie opinii użytkowników usługi Azure Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się, jak [Dodawanie warunków](logic-apps-use-logic-app-features.md)
- Dowiedz się więcej o [błąd i obsługa wyjątków](logic-apps-exception-handling.md)
- Poznaj więcej [obsługi language przepływu pracy](logic-apps-author-definitions.md)