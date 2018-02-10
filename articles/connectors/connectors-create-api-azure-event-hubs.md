---
title: "Konfigurowanie monitorowania zdarzeń z usługi Azure Event Hubs Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Monitorowanie strumieni danych w celu odbierania zdarzeń i wysyłania zdarzeń z aplikacji logiki przy użyciu usługi Azure Event Hubs"
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
ms.date: 02/06/2018
ms.author: estfan; LADocs
ms.openlocfilehash: 076f7dd11ca8c153046727861ecb755e88f32b01
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="monitor-receive-and-send-events-with-the-event-hubs-connector"></a>Monitorowanie, odbierania i wysyłania zdarzeń z łącznikiem usługi Event Hubs

Aby skonfigurować monitor zdarzeń, aby aplikację logiki można wykrywać zdarzenia, zdarzenia są rejestrowane i wysyłać zdarzenia, połącz się z [Azure Event Hub](https://azure.microsoft.com/services/event-hubs) z aplikacji logiki. Dowiedz się więcej o [Azure Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) i [cennik działania łączników aplikacji logiki](../logic-apps/logic-apps-pricing.md).

## <a name="prerequisites"></a>Wymagania wstępne

Zanim będzie możliwe użycie łącznika usługi Event Hubs, musi mieć następujące elementy:

* [Przestrzeni nazw usługi Azure Event Hubs i Centrum zdarzeń](../event-hubs/event-hubs-create.md)
* A [aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md)

<a name="permissions-connection-string"></a>

## <a name="connect-to-azure-event-hubs"></a>Połączenie do usługi Azure Event Hubs

Zanim aplikację logiki można uzyskać dostęp do dowolnej usługi, należy utworzyć [ *połączenia* ](./connectors-overview.md) między aplikację logiki i usługi, jeśli nie jest jeszcze. To połączenie autoryzuje aplikację logiki, aby uzyskać dostęp do danych. Dla aplikacji logiki do Centrum zdarzeń sprawdź swoje uprawnienia i pobrać ciągu połączenia dla przestrzeni nazw usługi Event Hubs.

1.  Zaloguj się w witrynie [Azure Portal](https://portal.azure.com "Azure Portal"). 

2.  Przejdź do centrów zdarzeń *przestrzeni nazw*, nie określonym Centrum zdarzeń. Na stronie przestrzeni nazw w obszarze **ustawienia**, wybierz **zasady dostępu współużytkowanego**. W obszarze **oświadczeń**, sprawdź, czy masz **Zarządzaj** uprawnienia dla tej przestrzeni nazw.

    ![Zarządzaj uprawnieniami dla przestrzeni nazw Centrum zdarzeń](./media/connectors-create-api-azure-event-hubs/event-hubs-namespace.png)

3. Jeśli chcesz później ręcznie wprowadzić informacje o połączeniu, należy pobrać ciągu połączenia dla przestrzeni nazw usługi Event Hubs. Wybierz **RootManageSharedAccessKey**. Obok ciągu połączenia klucza podstawowego kliknij przycisk Kopiuj. Zapisz parametry połączenia do późniejszego użycia.

    ![Skopiuj parametry połączenia w przestrzeni nazw usługi Event Hubs](media/connectors-create-api-azure-event-hubs/find-event-hub-namespace-connection-string.png)

    > [!TIP]
    > Aby sprawdzić, czy ciąg połączenia jest skojarzony z przestrzeni nazw usługi Event Hubs lub przy użyciu koncentratora określonego zdarzenia, sprawdź ciąg połączenia dla `EntityPath` parametru. Możesz znaleźć tego parametru, ciąg połączenia jest przeznaczony dla określonego Centrum zdarzeń "entity" i nie jest poprawny ciąg do korzystania z aplikacji logiki.

## <a name="trigger-workflow-when-your-event-hub-gets-new-events"></a>Wyzwalające przepływ pracy, gdy Centrum zdarzeń pobiera nowe zdarzenia

A [ *wyzwalacza* ](../logic-apps/logic-apps-overview.md#logic-app-concepts) jest zdarzeniem, która uruchamia przepływ pracy w aplikacji logiki. Aby uruchomić przepływ pracy, gdy nowe zdarzenia są wysyłane do Centrum zdarzeń, wykonaj następujące kroki dodawania wyzwalacz, który wykryje to zdarzenie.

1. W [portalu Azure](https://portal.azure.com "portalu Azure"), przejdź do istniejących aplikacji logiki lub tworzenie aplikacji logiki puste.

2. W Projektancie aplikacji logiki wprowadź "event hubs" w polu wyszukiwania jako filtr. Wybierz ten wyzwalacz: **podczas zdarzenia są dostępne w Centrum zdarzeń**

   ![Wybierz wyzwalacz po odbiera nowych zdarzeń w Centrum zdarzeń](./media/connectors-create-api-azure-event-hubs/find-event-hubs-trigger.png)

   1. Jeśli masz już połączenie do obszaru nazw usługi Event Hubs, zostanie wyświetlony monit o utworzyć teraz tego połączenia. Nadaj nazwę połączenia, a następnie wybierz obszar nazw usługi Event Hubs, który ma być używany.

      ![Utwórz połączenie z Centrum zdarzeń](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-1.png)

      Lub, aby ręcznie wprowadzić parametry połączenia, wybierz **ręcznie wprowadzić informacje o połączeniu**. 
      Dowiedz się [jak znaleźć parametrów połączenia](#permissions-connection-string).

   2. Teraz wybierz zasady usługi Event Hubs, a następnie wybierz **Utwórz**.

      ![Utwórz połączenie z Centrum zdarzeń, część 2](./media/connectors-create-api-azure-event-hubs/create-event-hubs-connection-2.png)

3. Wybierz Centrum zdarzeń, aby monitorować i skonfigurować interwał i częstotliwość czas wyszukiwania Centrum zdarzeń.

    ![Określ Centrum zdarzeń lub grupy odbiorców](./media/connectors-create-api-azure-event-hubs/select-event-hub.png)

    > [!TIP]
    > Opcjonalnie można wybrać grupy odbiorców do odczytywania zdarzeń, wybierz **Pokaż zaawansowane opcje**.

4. Zapisz aplikację logiki. Na pasku narzędzi projektanta wybierz pozycję **Zapisz**.

Teraz po aplikację logiki sprawdza wybranego Centrum zdarzeń i wyszukuje nowe zdarzenie, wyzwalacz działa akcje w aplikacji logiki znaleziono zdarzenia.

## <a name="send-events-to-your-event-hub-from-your-logic-app"></a>Wysyłanie zdarzeń do Centrum zdarzeń z aplikacji logiki

[*Akcja*](../logic-apps/logic-apps-overview.md#logic-app-concepts) to zadanie wykonywane przez przepływ pracy aplikacji logiki. Po dodaniu wyzwalacza do aplikacji logiki możesz dodać akcję, aby wykonywać operacje na danych generowanych przez ten wyzwalacz. Aby wysłać zdarzenia do Centrum zdarzeń z aplikacji logiki, wykonaj następujące kroki.

1. W Projektancie aplikacji logiki, w obszarze wyzwalacz, wybierz **nowy krok** > **Dodaj akcję**.

2. W polu wyszukiwania wprowadź "event hubs" jako filtr.
Wybierz tę akcję: **usługi Event Hubs — wysyłanie zdarzeń**

   ![Wybierz opcję "Event Hubs — wysyłanie zdarzeń"](./media/connectors-create-api-azure-event-hubs/select-event-hubs-send-event-action.png)

3. Wybierz Centrum zdarzeń, w których można wysłać zdarzenia. Następnie wprowadź zawartości zdarzeń i inne szczegóły.

   ![Wybierz nazwę Centrum zdarzeń i dostarczanie zawartości zdarzeń](./media/connectors-create-api-azure-event-hubs/event-hubs-send-event-action.png)

4. Zapisz aplikację logiki.

Teraz ustawiono akcję, która wysyła zdarzenia z aplikacji logiki. 

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Aby dowiedzieć się więcej o wyzwalacze i akcje zdefiniowane w pliku programu Swagger oraz żadnych limitów, przejrzyj [szczegóły łącznika](/connectors/eventhubs/).

## <a name="get-support"></a>Uzyskiwanie pomocy technicznej

* Jeśli masz pytania, odwiedź [forum usługi Azure Logic Apps](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).
* Aby przesłać pomysły dotyczące funkcji lub zagłosować na nie, odwiedź [witrynę opinii użytkowników usługi Logic Apps](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Kolejne kroki

* Dowiedz się więcej o [inne łączniki dla usługi Azure Logic apps](../connectors/apis-list.md)