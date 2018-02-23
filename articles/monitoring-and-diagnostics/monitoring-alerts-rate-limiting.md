---
title: "Oceń ograniczanie dla programu SMS, wiadomości e-mail, powiadomień wypychanych w aplikacji platformy Azure i elementów webhook | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Azure ogranicza liczbę możliwych programu SMS, wiadomości e-mail, powiadomień wypychanych lub elementu webhook aplikacji Azure z grupy akcji."
author: dkamstra
manager: chrad
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/16/2018
ms.author: dukek
ms.openlocfilehash: a4f97cc79945d266edd0af577e37fc9da2aa97bf
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="rate-limiting-for-sms-messages-emails-azure-app-push-notifications-and-webhook-posts"></a>Oceń ograniczanie dla wiadomości SMS, wiadomości e-mail, powiadomień wypychanych w aplikacji platformy Azure i elementu webhook wpisów
Limitów szybkości jest zawieszenie powiadomienia, gdy zbyt wiele powiadomienia są wysyłane do konkretny numer telefonu, adres e-mail lub urządzenia. Limitów szybkości zapewnia, że alerty są łatwe w zarządzaniu i możliwością wykonania akcji.

Progi limit szybkości są:

 - **SMS**: nie więcej niż 1 SMS co 5 minut.
 - **Wiadomości e-mail**: nie więcej niż 100 wiadomości e-mail w ciągu godziny.
 
 Inne akcje nie są ograniczone szybkości.

## <a name="rate-limit-rules"></a>Reguły limit szybkości
- Numer telefonu lub adres e-mail jest szybkość ograniczone, gdy odbiera komunikaty więcej niż próg.
- Numer telefonu lub adres e-mail może być częścią grupy akcji na wiele subskrypcji. Stosuje limitów szybkości dla wszystkich subskrypcji. Ma to zastosowanie zaraz po osiągnięciu progu, nawet jeśli komunikaty są wysyłane z wieloma subskrypcjami.
- Jeśli adres e-mail jest ograniczona, dodatkowe powiadomienie jest wysyłane do komunikowania się limitów szybkości. Stany powiadomienie po wygaśnięciu limitów szybkości.

## <a name="next-steps"></a>Kolejne kroki ##
* Dowiedz się więcej o [SMS alertów zachowanie](monitoring-sms-alert-behavior.md).
* Pobierz [Przegląd alertów dotyczących działań w dzienniku](monitoring-overview-alerts.md)i dowiedzieć się, jak otrzymywać alerty.  
* Dowiedz się, jak [skonfigurować alerty, gdy powiadomienie usługi kondycji jest przesyłana](monitoring-activity-log-alerts-on-service-notifications.md).
