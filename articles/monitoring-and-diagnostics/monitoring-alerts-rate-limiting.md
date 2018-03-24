---
title: Oceń ograniczanie dla programu SMS, wiadomości e-mail, powiadomień wypychanych w aplikacji platformy Azure i elementów webhook | Dokumentacja firmy Microsoft
description: Dowiedz się, jak Azure ogranicza liczbę możliwych programu SMS, wiadomości e-mail, powiadomień wypychanych lub elementu webhook aplikacji Azure z grupy akcji.
author: dkamstra
manager: chrad
editor: ''
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: ''
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/12/2018
ms.author: dukek
ms.openlocfilehash: 9216a64dbd8201ff09ea5c9283b4db465682a3bd
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Oceń ograniczanie dla głosu, programu SMS, wiadomości e-mail, powiadomień wypychanych w aplikacji platformy Azure i elementu webhook wpisów
Limitów szybkości jest zawieszenie powiadomienia, gdy zbyt wiele są wysyłane do konkretny numer telefonu, adres e-mail lub urządzenia. Limitów szybkości zapewnia, że alerty są łatwe w zarządzaniu i możliwością wykonania akcji.

Progi limit szybkości są:

 - **SMS**: nie więcej niż 1 SMS co 5 minut.
 - **Głos**: głosu nie więcej niż 1 wywołać co 5 minut.
 - **Wiadomości e-mail**: nie więcej niż 100 wiadomości e-mail w ciągu godziny.
 
 Inne akcje nie są ograniczone szybkości.

## <a name="rate-limit-rules"></a>Reguły limit szybkości
- Numer telefonu lub adres e-mail jest szybkość ograniczone, gdy odbiera komunikaty więcej niż próg.
- Numer telefonu lub adres e-mail może być częścią grupy akcji na wiele subskrypcji. Stosuje limitów szybkości dla wszystkich subskrypcji. Ma to zastosowanie zaraz po osiągnięciu progu, nawet jeśli komunikaty są wysyłane z wieloma subskrypcjami.
- Jeśli adres e-mail jest ograniczona, dodatkowe powiadomienie jest wysyłane do komunikowania się limitów szybkości. Stany poczty e-mail, po wygaśnięciu limitów szybkości.

## <a name="next-steps"></a>Kolejne kroki ##
* Dowiedz się więcej o [SMS alertów zachowanie](monitoring-sms-alert-behavior.md).
* Pobierz [Przegląd alertów dotyczących działań w dzienniku](monitoring-overview-alerts.md)i dowiedzieć się, jak otrzymywać alerty.  
* Dowiedz się, jak [skonfigurować alerty, gdy powiadomienie usługi kondycji jest przesyłana](monitoring-activity-log-alerts-on-service-notifications.md).
