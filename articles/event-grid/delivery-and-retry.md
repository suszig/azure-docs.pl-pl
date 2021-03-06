---
title: "Azure dostarczania zdarzeń siatki i spróbuj ponownie"
description: "Opisuje sposób siatki zdarzeń Azure zapewnia zdarzeń i sposób obsługi niedostarczone wiadomości."
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: article
ms.date: 01/30/2018
ms.author: tomfitz
ms.openlocfilehash: cdf6a4e999d55196e8f4eac5695163a7e5a933de
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
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

Siatka zdarzeń dodaje małe losowe do wszystkich interwałów ponów próbę. Po upływie godziny dostarczania zdarzeń próba zostanie ponowiona godzinę.

## <a name="retry-duration"></a>Spróbuj ponownie czas trwania

Azure siatki zdarzeń wygasa wszystkie zdarzenia, które nie zostały dostarczone w ciągu 24 godzin.

## <a name="next-steps"></a>Kolejne kroki

* Aby wyświetlić stan dostawy zdarzeń, zobacz [dostarczanie komunikatów Monitora zdarzeń siatki](monitor-event-delivery.md).
* Aby obejrzeć wprowadzenie do siatki zdarzeń, zobacz [o siatki zdarzeń](overview.md).
* Aby szybko rozpocząć korzystanie z siatki zdarzeń, zobacz [tworzenie i tras niestandardowych zdarzeń siatki zdarzeń Azure](custom-event-quickstart.md).