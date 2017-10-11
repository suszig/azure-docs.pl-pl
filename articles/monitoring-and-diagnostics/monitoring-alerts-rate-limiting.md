---
title: "Oceń ograniczanie dla programu SMS, wiadomości e-mail i elementów webhook | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Azure ogranicza liczbę możliwe powiadomień programu SMS, wiadomości e-mail lub elementu webhook z grupy akcji."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: ancav
ms.openlocfilehash: bde645624ab1860d19ba18470f55845855a7d1fb
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="rate-limiting-for-sms-messages-emails-and-webhook-posts"></a>Oceń ograniczenie dla elementu webhook, wiadomości e-mail i wiadomości SMS wpisów
Limitów szybkości jest zawieszenie powiadomienia, gdy zbyt wiele powiadomienia są wysyłane na adres telefonu komórkowego lub adresu e-mail. Limitów szybkości zapewnia, że alerty są łatwe w zarządzaniu i możliwością wykonania akcji.

Zasady programu SMS i wiadomości e-mail są takie same. Próg limitu szybkości jest:

 - **SMS**: 10 wiadomości w ciągu godziny.
 - **Wiadomości e-mail**: 100 wiadomości w ciągu godziny.

## <a name="rate-limit-rules"></a>Reguły limit szybkości
- Numer telefonu lub adres e-mail jest szybkość ograniczone, gdy odbiera komunikaty więcej niż próg.
- Numer telefonu lub adres e-mail może być częścią grupy akcji na wiele subskrypcji. Stosuje limitów szybkości dla wszystkich subskrypcji. Ma to zastosowanie zaraz po osiągnięciu progu, nawet jeśli komunikaty są wysyłane z wieloma subskrypcjami.  
- Jeśli numer telefonu lub adres e-mail jest szybkość ograniczone, dodatkowe powiadomienie jest wysyłane do komunikowania się limitów szybkości. Stany powiadomienie po wygaśnięciu limitów szybkości.

## <a name="rate-limit-of-webhooks"></a>Limit szybkości elementów webhook ##
Brak nie szybkość ograniczenia w przypadku elementów webhook.

## <a name="next-steps"></a>Następne kroki ##
* Dowiedz się więcej o [SMS alertów zachowanie](monitoring-sms-alert-behavior.md).
* Pobierz [Przegląd alertów dotyczących działań w dzienniku](monitoring-overview-alerts.md)i dowiedzieć się, jak otrzymywać alerty.  
* Dowiedz się, jak [skonfigurować alerty, gdy powiadomienie usługi kondycji jest przesyłana](monitoring-activity-log-alerts-on-service-notifications.md).
