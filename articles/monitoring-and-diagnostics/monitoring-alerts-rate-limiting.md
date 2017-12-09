---
title: "Oceń ograniczanie dla programu SMS, wiadomości e-mail, powiadomień wypychanych w aplikacji platformy Azure i elementów webhook | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak Azure ogranicza liczbę możliwych programu SMS, wiadomości e-mail, powiadomień wypychanych lub elementu webhook aplikacji Azure z grupy akcji."
author: dukek
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
ms.date: 12/8/2017
ms.author: dukek
ms.openlocfilehash: c76bf5cf51f18a32b33060d528c64d119e31dbbd
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/09/2017
---
# <a name="rate-limiting-for-sms-messages-emails-azure-app-push-notifications-and-webhook-posts"></a>Oceń ograniczanie dla wiadomości SMS, wiadomości e-mail, powiadomień wypychanych w aplikacji platformy Azure i elementu webhook wpisów
Limitów szybkości jest zawieszenie powiadomienia, gdy zbyt wiele powiadomienia są wysyłane do konkretny numer telefonu, adres e-mail lub urządzenia. Limitów szybkości zapewnia, że alerty są łatwe w zarządzaniu i możliwością wykonania akcji.

Progi limit szybkości są:

 - **SMS**: nie więcej niż 1 SMS co 5 minut.
 - **Wiadomości e-mail**: 100 wiadomości w ciągu godziny.
 - **Powiadomienia wypychane aplikacji dla platformy Azure**: Brak nie szybkość ograniczanie dla powiadomień wypychanych.
 - **Elementów Webhook**: Brak nie szybkość ograniczanie dla elementów webhook.

## <a name="rate-limit-rules"></a>Reguły limit szybkości
- Numer telefonu lub adres e-mail jest szybkość ograniczone, gdy odbiera komunikaty więcej niż próg.
- Numer telefonu lub adres e-mail może być częścią grupy akcji na wiele subskrypcji. Stosuje limitów szybkości dla wszystkich subskrypcji. Ma to zastosowanie zaraz po osiągnięciu progu, nawet jeśli komunikaty są wysyłane z wieloma subskrypcjami.  
- Jeśli adres e-mail jest ograniczona, dodatkowe powiadomienie jest wysyłane do komunikowania się limitów szybkości. Stany powiadomienie po wygaśnięciu limitów szybkości.

## <a name="next-steps"></a>Następne kroki ##
* Dowiedz się więcej o [SMS alertów zachowanie](monitoring-sms-alert-behavior.md).
* Pobierz [Przegląd alertów dotyczących działań w dzienniku](monitoring-overview-alerts.md)i dowiedzieć się, jak otrzymywać alerty.  
* Dowiedz się, jak [skonfigurować alerty, gdy powiadomienie usługi kondycji jest przesyłana](monitoring-activity-log-alerts-on-service-notifications.md).
