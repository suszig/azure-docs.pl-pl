---
title: "Azure dostarczania zdarzeń siatki i spróbuj ponownie"
description: "Opisuje sposób siatki zdarzeń Azure zapewnia zdarzeń i sposób obsługi niedostarczone wiadomości."
services: event-grid
author: djrosanova
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/05/2018
ms.author: darosa
ms.openlocfilehash: 4eacb37d6e19b4b69d604aa84fd404479dead1ea
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="event-grid-message-delivery-and-retry"></a>Dostarczanie komunikatów siatki zdarzeń i spróbuj ponownie 

W tym artykule opisano, jak siatki zdarzeń Azure obsługuje zdarzenia podczas dostarczania nie zostaje potwierdzony.

Zdarzenie siatki udostępnia trwałe dostarczania. Zapewnia każdy komunikat co najmniej raz dla każdej subskrypcji. Zdarzenia są wysyłane bezpośrednio do zarejestrowanych elementu webhook każdej subskrypcji. Jeśli elementu webhook nie otrzymanie zdarzenia w ciągu 60 sekund przy pierwszej próbie dostarczenia, siatki zdarzeń ponowi próbę dostarczania zdarzenia.

## <a name="message-delivery-status"></a>Stan dostarczania wiadomości

Siatka zdarzeń używa kody odpowiedzi HTTP do otrzymanie zdarzenia. 

### <a name="success-codes"></a>Kody operacji zakończonych powodzeniem

Następujące kody odpowiedzi HTTP wskazują, że zdarzenia został pomyślnie dostarczony do Twojej elementu webhook. Zdarzenie siatki uwzględnia pełną dostarczania.

- 200 OK
- 202 zaakceptowane

### <a name="failure-codes"></a>Kod błędów

Następujące kody odpowiedzi HTTP wskazują, że próba dostarczania zdarzeń nie powiodła się. Siatka zdarzeń próbuje ponownie wysłać zdarzenia. 

- 400 Niewłaściwe żądanie
- 401 nieautoryzowane
- 404 — Nie odnaleziono
- 408 Limit czasu żądania
- Identyfikator URI 414 zbyt długa
- 500 Wewnętrzny błąd serwera
- 503 Usługa jest niedostępna
- Limit czasu bramy 504

Każdy kod odpowiedzi lub brak odpowiedzi oznacza błąd. Zdarzenie siatki ponownych prób dostarczenia. 

## <a name="retry-intervals"></a>Interwały ponawiania

Siatka zdarzeń używa zasady ponawiania wykładniczego wycofywania w celu dostarczania zdarzeń. Jeśli Twoje elementu webhook nie odpowiada, zwraca kod błędu siatki zdarzeń ponowi próbę dostarczania zgodnie z harmonogramem następujące:

1. 10 sekund
2. 30 sekund
3. 1 minuta
4. 5 minut
5. 10 minut
6. 30 minut
7. 1 godzina

Siatka zdarzeń dodaje małe losowe do wszystkich interwałów ponów próbę.

## <a name="retry-duration"></a>Spróbuj ponownie czas trwania

W wersji zapoznawczej siatki zdarzeń Azure wygasa wszystkie zdarzenia, które nie zostały dostarczone w ciągu dwóch godzin.

## <a name="monitoring"></a>Monitorowanie

Aby wyświetlić stan dostawy zdarzeń, można użyć portalu.

Aby widzieć metryki dla subskrypcji zdarzeń, wyszukaj **subskrypcji zdarzeń** dostępnych usług i zaznacz je.

![Wyszukaj subskrypcji zdarzeń](./media/delivery-and-retry/select-event-subscriptions.png)

Filtruj według typu zdarzenia, subskrypcji i lokalizacji. Wybierz **metryki** subskrypcji do wyświetlenia.

![Filtr subskrypcji zdarzeń](./media/delivery-and-retry/filter-events.png)

Wyświetl metryki zdarzenia tematu i subskrypcji.

![Wyświetlaj metryki zdarzenia](./media/delivery-and-retry/subscription-metrics.png)

Po opublikowaniu niestandardowego tematu, można wyświetlić dla niej metryki. Wybierz grupę zasobów, zawierającą tematu, a następnie wybierz temat.

![Wybierz niestandardowego tematu](./media/delivery-and-retry/select-custom-topic.png)

Wyświetl metryki dla tematu zdarzenie niestandardowe.

![Wyświetlaj metryki zdarzenia](./media/delivery-and-retry/custom-topic-metrics.png)

## <a name="next-steps"></a>Kolejne kroki

* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [o siatki zdarzeń](overview.md).
* Aby szybko rozpocząć korzystanie z siatki zdarzeń, zobacz [tworzenie i tras niestandardowych zdarzeń siatki zdarzeń Azure](custom-event-quickstart.md).