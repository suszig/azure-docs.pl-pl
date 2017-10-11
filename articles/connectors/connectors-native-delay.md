---
title: "Dodaj opóźnienia w aplikacjach logiki | Dokumentacja firmy Microsoft"
description: "Przegląd opóźnienia i opóźnienie — do działań i jak z nich korzystać z aplikacji logiki platformy Azure."
services: 
documentationcenter: 
author: jeffhollan
manager: erikre
editor: 
tags: connectors
ms.assetid: 915f48bf-3bd8-4656-be73-91a941d0afcd
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2016
ms.author: jehollan
ms.openlocfilehash: 5f4f7052d48b4ca4ed91212d970551141e78e852
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-delay-and-delay-until-actions"></a>Rozpoczynanie pracy z opóźnieniem i opóźnienie — do działania
Za pomocą opóźnienie i "opóźnienie-dopóki" akcje, ukończeniem scenariuszy przepływu pracy.

Można na przykład:

* Poczekaj na dzień do wysyłania aktualizacji stanu za pośrednictwem poczty e-mail.
* Opóźnienie przepływu pracy do czasu wywołania HTTP czasu, aby zakończyć działanie przed wznawianie i pobierania wynik.

Aby rozpocząć, korzystając z akcji opóźnienia w aplikacji logiki, zobacz [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="use-the-delay-actions"></a>Akcje opóźnienia
Akcja jest operacja odbywa się przez przepływ pracy, który jest zdefiniowany w aplikacji logiki. [Dowiedz się więcej o akcjach](connectors-overview.md).

Oto przykład sekwencji sposób używania opóźnienia kroku w aplikacji logiki:

1. Po dodaniu wyzwalacza, kliknij przycisk **nowy krok** Aby dodać akcję.
2. Wyszukaj **opóźnienie** można wyświetlić akcje opóźnienia. W tym przykładzie firma Microsoft będzie wybierać **opóźnienie**.
   
    ![Opóźnienie akcji](./media/connectors-native-delay/using-action-1.png)
3. Ukończ wszelkie właściwości akcji, aby skonfigurować opóźnienie.
   
    ![Opóźnienie konfiguracji](./media/connectors-native-delay/using-action-2.png)
4. Kliknij przycisk **zapisać** do publikowania i aktywować aplikację logiki.

## <a name="action-details"></a>Szczegóły akcji
Wyzwalacz cyklu ma następujące właściwości, które można skonfigurować.

### <a name="delay-action"></a>Opóźnienie akcji
Ta akcja opóźnienia uruchomienia na określonym przedziale czasu.
A * oznacza, że jest polem wymaganym.

| Nazwa wyświetlana | Nazwa właściwości | Opis |
| --- | --- | --- |
| Liczba * |Liczba |Liczba jednostek czasu opóźnienia |
| Jednostka * |jednostki |Jednostka czasu: `Second`, `Minute`, `Hour`, lub`Day` |

<br>

### <a name="delay-until-action"></a>Opóźnienie — do akcji
Ta akcja opóźnienia uruchomienia aż do określonej daty/godziny.
A * oznacza, że jest polem wymaganym.

| Nazwa wyświetlana | Nazwa właściwości | Opis |
| --- | --- | --- |
| Rok * |sygnatura czasowa |Roku, aby opóźnić do (GMT) |
| Miesiąc * |sygnatura czasowa |Miesiąc opóźnienia do (GMT) |
| Dzień * |sygnatura czasowa |Dzień opóźnienia do (GMT) |

<br>

## <a name="next-steps"></a>Następne kroki
Teraz, wypróbuj platformy i [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md). Dostępne łączniki w aplikacjach logiki można eksplorować analizując naszych [listy interfejsów API](apis-list.md).

