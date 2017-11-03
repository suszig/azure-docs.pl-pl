---
title: "Co to są powiadomienia o kondycji usługi | Dokumentacja firmy Microsoft"
description: "Powiadomienia o kondycji usługi umożliwiają wyświetlanie usługi kondycji, który wiadomości przez Microsoft Azure."
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
ms.openlocfilehash: d85281c02b792921f12cc62e6d60bef3e7c13b3f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="service-health-notifications"></a>Powiadomienia o kondycji usługi
## <a name="overview"></a>Omówienie

W tym artykule przedstawiono sposób wyświetlania powiadomień o kondycji usługi przy użyciu portalu Azure.

Powiadomienia o kondycji usługi umożliwiają wyświetlanie komunikatów o kondycji usługi opublikowana przez zespół Azure, który może mieć wpływ na zasoby w ramach Twojej subskrypcji. Te powiadomienia są podklasą klasy działania dziennika zdarzeń i można także znaleźć w bloku dziennika aktywności. Powiadomienia o kondycji usługi może być informacyjne lub można wykonać w zależności od tej klasy.

Istnieje pięć klas powiadomień o kondycji usługi:  

- **Wymagana akcja:** od czasu do czasu firma Microsoft mogą pojawić się coś nietypowego się tak zdarzyć na Twoim koncie. Firma Microsoft może być konieczne we współpracy z Tobą, aby rozwiązać ten problem. Firma Microsoft będzie wysyłać powiadomienia albo określających działania należy podjąć lub więcej informacji na temat skontaktuj się z Azure engineering lub pomocy technicznej.  
- **Odzyskiwanie asystowaną:** wystąpiło zdarzenie i inżynierów potwierdzeniu, że nadal występują wpływu. Współpraca z Tobą bezpośrednio, aby wyświetlić swoje usługi w celu przywrócenia należy zespołu inżynieryjnego.  
- **Zdarzenie:** usługi wpływające na zdarzenie aktualnie ma wpływ na co najmniej jeden z zasobów w ramach subskrypcji.  
- **Konserwacja:** jest powiadomienie informujące o działanie zaplanowanej konserwacji, które mogą mieć wpływ na co najmniej jeden z zasobów w ramach Twojej subskrypcji.  
- **Informacje o:** od czasu do czasu firma Microsoft może wysyłać powiadomienia przekazują do użytkownika o potencjalnych funkcje optymalizacji, które mogą pomóc zwiększenie wykorzystanie zasobów.  
- **Zabezpieczenia:** pilnych zabezpieczeń powiązane informacje dotyczące Twojego solution(s) działających na platformie Azure.

Każde powiadomienie o kondycji usługi prowadzi szczegółowe informacje o zakres i znaczenie dla zasobów. Zawiera szczegółowe informacje:

Nazwa właściwości | Opis
-------- | -----------
Kanały | Jest jednym z następujących wartości: "Administrator", "Operacji"
correlationId | Jest zazwyczaj identyfikator GUID w postaci ciągu. Zdarzenia z tym należy do tego samego działania pełny zwykle udostępnianie tego samego correlationId.
eventDataId | Jest unikatowy identyfikator zdarzenia
EventName | Jest to tytuł zdarzenia
poziom | Poziom zdarzenia. Jedną z następujących wartości: "Krytyczne", "Błąd", "Ostrzeżenie", "Informacyjny" i "Pełne"
resourceProviderName | Nazwa dostawcy zasobu dla zasobu wpływ na
Typ zasobu| Typ zasobu wpływ na zasób
Podstan | Zazwyczaj wywołań REST odpowiedni kod stanu HTTP, ale można również uwzględnić inne parametry opisujące podstanu, takich jak te wartości typowych: OK (kod stanu HTTP: 200), utworzony (kod stanu HTTP: 201), akceptowane (kod stanu HTTP: 202), nie zawartości (kod stanu HTTP: 204), nieprawidłowe żądanie (kod stanu HTTP: 400), nie znaleziono (kod stanu HTTP: 404), konflikt (kod stanu HTTP : 409), wewnętrzny błąd serwera (kod stanu HTTP: 500), Usługa niedostępna (kod stanu HTTP: 503), limit czasu bramy (kod stanu HTTP: 504).
eventTimestamp | Sygnatura czasowa po zdarzeniu został wygenerowany przez usługę Azure przetwarzania żądania odpowiednie zdarzenie.
submissionTimestamp |   Znacznik czasu w momencie zdarzenia stały się dostępne na potrzeby zapytań.
subscriptionId | Subskrypcja platformy Azure, w którym zarejestrowano to zdarzenie
status | Ciąg opisujący stan operacji. Niektóre typowe wartości to: pracy w toku, zakończyło się pomyślnie, nie powiodło się, aktywne, rozwiązane.
operationName | Nazwa operacji.
category | "ServiceHealth"
resourceId | Identyfikator zasobu ryzyko zasobu.
Properties.Title | Tytuł zlokalizowane dla tej komunikacji. Językiem domyślnym będzie angielski.
Properties.Communication | Szczegóły zlokalizowanych komunikacji z kodu znaczników HTML. Domyślnym będzie angielski.
Properties.incidentType | Możliwe wartości: AssistedRecovery, ActionRequired, informacje, zdarzenie, obsługi, zabezpieczeń
Properties.trackingId | Identyfikuje incydent, który jest skojarzony to zdarzenie. Umożliwia korelowanie zdarzeń związanych ze zdarzeniem.
Properties.impactedServices | Zmienionym obiektu blob JSON opisującą usług i regionów, które mają wpływ zdarzenia. Lista usług, z których każda ma elementy ServiceName i listę ImpactedRegions, z których każda ma RegionName.
Properties.defaultLanguageTitle | Komunikat w języku angielskim
Properties.defaultLanguageContent | Komunikat w języku angielskim jako kod znaczników html i zwykły tekst
Properties.Stage | Możliwe wartości AssistedRecovery, ActionRequired, informacje, zdarzenie, zabezpieczeń: są aktywne, rozwiązane. W konserwacji są: aktywny, planowane, w toku, anulowane, Rescheduled, rozwiązane, Zakończ
Properties.communicationId | Komunikacja to zdarzenie jest skojarzony.


## <a name="viewing-your-service-health-notifications-in-the-azure-portal"></a>Wyświetlanie powiadomienia usługi kondycji w portalu Azure
1.  W [portal](https://portal.azure.com), przejdź do **Monitor** usługi

    ![Monitorowanie](./media/monitoring-service-notifications/home-monitor.png)
2.  Kliknij przycisk **Monitor** opcję, aby otworzyć blok monitora. Ten blok gromadzi wszystkie ustawienia monitorowania i dane użytkownika w jednym skonsolidowanym widoku. Na początku widoczna jest sekcja **Dziennik aktywności**.

3.  Teraz kliknij **powiadomień usługi** sekcji

    ![Monitorowanie](./media/monitoring-service-notifications/service-health-summary.png)
4.  Kliknij dowolną pozycje, aby wyświetlić więcej szczegółów

5. Polecenie **+ Dodaj działanie dziennika alertu** operację, aby otrzymywać powiadomienia, aby upewnić się, zostanie wyświetlone powiadomienie powiadomień usługi przyszłości tego typu. Aby dowiedzieć się więcej na temat konfigurowania alertów na powiadomienia usługi [kliknij tutaj](monitoring-activity-log-alerts-on-service-notifications.md)

## <a name="next-steps"></a>Następne kroki:
Odbieranie [alertów powiadomienia, gdy powiadomienie o kondycji usługi](monitoring-activity-log-alerts-on-service-notifications.md) jest przesyłana  
Dowiedz się więcej o [alertów dotyczących działań w Dzienniku](monitoring-activity-log-alerts.md)
