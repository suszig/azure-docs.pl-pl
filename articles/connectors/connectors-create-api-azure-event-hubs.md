---
title: "Konfigurowanie Monitora zdarzeń w usłudze Azure Event Hubs dla usługi Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Monitorowanie strumieni danych w celu odbierania zdarzeń i wysyłania zdarzeń dla usługi Azure Logic Apps w usłudze Azure Event Hubs"
services: logic-apps
keywords: "strumień danych, monitor zdarzeń, usługa event hubs"
author: ecfan
manager: anneta
editor: 
documentationcenter: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/31/2017
ms.author: estfan; LADocs
ms.openlocfilehash: a7f31c2c17d326d58ede0bb00cdc0f701069ea14
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Monitorowanie, odbierania i wysyłania zdarzeń z łącznikiem usługi Event Hubs

Aby skonfigurować monitor zdarzeń, aby aplikację logiki można wykrywać zdarzenia, zdarzenia są rejestrowane i wysyłać zdarzenia, połącz się z [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) z aplikacji logiki. Dowiedz się więcej o [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md).

## <a name="requirements"></a>Wymagania

* Musisz mieć [centra zdarzeń w przestrzeni nazw i Centrum zdarzeń](../event-hubs/event-hubs-create.md) na platformie Azure. Dowiedz się [sposobu tworzenia centra zdarzeń w przestrzeni nazw i Centrum zdarzeń](../event-hubs/event-hubs-create.md). 

* Aby użyć [każdy łącznik](https://docs.microsoft.com/azure/connectors/apis-list) w aplikacji logiki, należy najpierw utworzyć aplikację logiki. Dowiedz się [sposób tworzenia aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

<a name="permissions-connection-string"></a>
## <a name="check-event-hubs-namespace-permissions-and-find-the-connection-string"></a>Sprawdź uprawnienia przestrzeni nazw usługi Event Hubs i Znajdź parametry połączenia

Aplikację logiki, dostęp do dowolnej usługi, należy utworzyć [ *połączenia* ](./connectors-overview.md) między aplikację logiki i usługi, jeśli nie jest jeszcze. To połączenie autoryzuje aplikację logiki, aby uzyskać dostęp do danych.
Centrum zdarzeń dostępu do aplikacji logiki, musisz mieć **Zarządzaj** uprawnienia i parametry połączenia dla przestrzeni nazw usługi Event Hubs.

Sprawdź swoje uprawnienia i pobrać ciągu połączenia, wykonaj następujące kroki.

1.  Zaloguj się w witrynie [Azure Portal](https://portal.azure.com "Azure Portal"). 

2.  Przejdź do centrów zdarzeń *przestrzeni nazw*, nie określonym Centrum zdarzeń. W bloku przestrzeni nazw w obszarze **ustawienia**, wybierz **zasady dostępu współużytkowanego**. W obszarze **oświadczeń**, sprawdź, czy masz **Zarządzaj** uprawnienia dla tej przestrzeni nazw.

    ![Zarządzaj uprawnieniami dla przestrzeni nazw Centrum zdarzeń](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3.  Aby skopiować parametry połączenia dla przestrzeni nazw usługi Event Hubs, wybierz opcję **RootManageSharedAccessKey**. Obok ciągu połączenia klucza podstawowego kliknij przycisk Kopiuj.

    ![Skopiuj parametry połączenia w przestrzeni nazw usługi Event Hubs](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Aby sprawdzić, czy ciąg połączenia jest skojarzony z przestrzeni nazw usługi Event Hubs lub z określonym Centrum zdarzeń, sprawdź ciąg połączenia dla `EntityPath` parametru. Możesz znaleźć tego parametru, ciąg połączenia jest przeznaczony dla określonego Centrum zdarzeń "entity" i nie jest poprawny ciąg do korzystania z aplikacji logiki.

4.  Teraz po wyświetleniu monitu o poświadczenia, po dodaniu usługi Event Hubs wyzwalacza lub akcji do aplikacji logiki, możesz nawiązać przestrzeni nazw usługi Event Hubs. Nadaj nazwę połączenia, wprowadź parametry połączenia, który został skopiowany, a wybierz **Utwórz**.

    ![Wprowadź parametry połączenia dla przestrzeni nazw usługi Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Po utworzeniu połączenia, nazwa połączenia powinny być wyświetlane w zdarzeniu koncentratory wyzwalacza lub akcji. 
    Następnie możesz nadal z pozostałe kroki w aplikacji logiki.

    ![Połączenia nazw centra zdarzeń utworzony](./media/connectors-create-api-azure-event-hubs/event-hubs-connection-created.png)

## <a name="start-workflow-when-your-event-hub-receives-new-events"></a>Uruchamianie przepływu pracy w Centrum zdarzeń odebrania nowych zdarzeń

A [ *wyzwalacza* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) jest zdarzeniem, która uruchamia przepływ pracy w aplikacji logiki. Aby uruchomić przepływ pracy, gdy nowe zdarzenia są wysyłane do Centrum zdarzeń, wykonaj następujące kroki dodawania wyzwalacz, który wykryje to zdarzenie.

1.  W [portalu Azure](https://portal.azure.com "portalu Azure"), przejdź do istniejących aplikacji logiki lub tworzenie aplikacji logiki puste.

2.  W polu wyszukiwania dla projektanta aplikacji logiki, wprowadź `event hubs` filtru. Wybierz ten wyzwalacz: **podczas zdarzenia są dostępne w Centrum zdarzeń**

    ![Wybierz wyzwalacz po odbiera nowych zdarzeń w Centrum zdarzeń](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

    Jeśli masz już połączenie do obszaru nazw usługi Event Hubs, zostanie wyświetlony monit o utworzyć teraz tego połączenia. Nadaj nazwę połączenia, a następnie wprowadź parametry połączenia dla przestrzeni nazw usługi Event Hubs. 
    Dowiedz się, jeśli to konieczne, [jak znaleźć parametrów połączenia](#permissions-connection-string).

    ![Wprowadź parametry połączenia dla przestrzeni nazw usługi Event Hubs](./media/connectors-create-api-azure-event-hubs/event-hubs-connection.png)

    Po utworzeniu połączenia, ustawienia **podczas zdarzenia w dostępnych w Centrum zdarzeń** wyzwalacza są wyświetlane.

    ![Ustawienia wyzwalacza po odbiera nowych zdarzeń w Centrum zdarzeń](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger.png)

3.  Wprowadź lub wybierz nazwę Centrum zdarzeń, które chcesz monitorować. Wybierz częstotliwość i interwał, jak często chcesz sprawdzić Centrum zdarzeń.

    > [!TIP]
    > Opcjonalnie można wybrać grupy odbiorców do odczytywania zdarzeń, wybierz **Pokaż zaawansowane opcje**. 

    ![Określ Centrum zdarzeń lub grupy odbiorców](./media/connectors-create-api-azure-event-hubs/event-hubs-trigger-details.png)

    Wyzwalacz zostało teraz skonfigurowane do uruchomienia przepływu pracy aplikacji logiki. 
    Aplikację logiki sprawdza określonym Centrum zdarzeń na podstawie harmonogramu, który zostanie ustawiona. 
    Jeśli aplikacja znajdzie nowe zdarzenia zdarzeń koncentratora, wyzwalacz uruchamia inne akcje lub wyzwala w aplikacji logiki.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Wysyłanie zdarzeń do Centrum zdarzeń z aplikacji logiki

[*Akcja*](../logic-apps/logic-apps-overview.md#logic-app-concepts) to zadanie wykonywane przez przepływ pracy aplikacji logiki. Po dodaniu wyzwalacza do aplikacji logiki możesz dodać akcję, aby wykonywać operacje na danych generowanych przez ten wyzwalacz. Aby wysłać zdarzenia do Centrum zdarzeń z aplikacji logiki, wykonaj następujące kroki.

1.  W Projektancie aplikacji logiki, zgodnie z wyzwalaczem aplikacji logiki, wybierz **nowy krok** > **Dodaj akcję**.

    ![Wybierz pozycję "Nowy krok", następnie "Dodaj akcję"](./media/connectors-create-api-azure-event-hubs/add-action.png)

    Teraz można znaleźć i wybierz akcję do wykonania. 
    Chociaż można wybrać żadnych akcji, na przykład chcemy akcji usługi Event Hubs wysyłać zdarzenia.

2.  W polu wyszukiwania wprowadź `event hubs` filtru.
Wybierz tę akcję: **wysyłania zdarzeń**

    ![Wybierz opcję "Event Hubs — wysyłanie zdarzeń" akcji](./media/connectors-create-api-azure-event-hubs/find-event-hubs-action.png)

3.  Wprowadź wymagane szczegóły zdarzenia, takie jak nazwa dla Centrum zdarzeń, które chcesz wysłać zdarzenia. Wprowadź inne opcjonalne szczegóły dotyczące zdarzenia, takie jak zawartość dla tego zdarzenia.

    > [!TIP]
    > Aby określić partycji Centrum zdarzeń opcjonalnie, gdzie można wysłać zdarzenia, wybierz **Pokaż zaawansowane opcje**. 

    ![Wprowadź nazwę Centrum zdarzeń i szczegóły zdarzenia opcjonalne](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

6.  Zapisz zmiany.

    ![Zapisywanie aplikacji logiki](./media/connectors-create-api-azure-event-hubs/save-logic-app.png)

    Akcja zostało teraz skonfigurowane do wysłania zdarzeń z aplikacji logiki. 

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/eventhubs/). 

## <a name="get-help"></a>Uzyskiwanie pomocy

Aby zadawać pytania, odpowiadać na nie i patrzeć, co robią inni użytkownicy usługi Azure Logic Apps, odwiedź [forum usługi Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

Aby pomóc w ulepszaniu usługi Logic Apps, przesyłaj pomysły lub głosuj na nie w [witrynie opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

*  [Znajdź inne łączniki dla usługi Azure Logic apps](./apis-list.md)