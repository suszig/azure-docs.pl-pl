---
title: "Jak... w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Często zadawane pytania w usłudze Application Insights."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 48b2b644-92e4-44c3-bc14-068f1bbedd22
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: bwren
ms.openlocfilehash: ef63e06c0621753e0a706d6efb709b943e38ee42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="how-do-i--in-application-insights"></a>Jak mogę (...) w usłudze Application Insights?
## <a name="get-an-email-when-"></a>Otrzymasz wiadomość e-mail, gdy...
### <a name="email-if-my-site-goes-down"></a>Wiadomość e-mail, jeśli Moja witryna jest wyłączona
Ustaw [dostępności testu sieci web](app-insights-monitor-web-app-availability.md).

### <a name="email-if-my-site-is-overloaded"></a>Wyślij wiadomość e-mail, jeśli Moja witryna jest przeciążony
Ustaw [alert](app-insights-alerts.md) na **czas odpowiedzi serwera**. Próg między 1 i 2 sekundy powinny działać.

![](./media/app-insights-how-do-i/030-server.png)

Aplikacji mogą być również wyświetlane znaki obciążenie zwracając kod błędu. Ustaw alert na **żądań zakończonych niepowodzeniem**.

Jeśli chcesz ustawić alert na **wyjątki serwera**, może być konieczne przeprowadzenie [niektóre dodatkowe ustawienia](app-insights-asp-net-exceptions.md) Aby wyświetlić dane.

### <a name="email-on-exceptions"></a>Wyślij wiadomość e-mail na wyjątki
1. [Konfigurowanie monitorowania wyjątków](app-insights-asp-net-exceptions.md)
2. [Ustawić alert](app-insights-alerts.md) na wyjątku liczba metryki

### <a name="email-on-an-event-in-my-app"></a>Wyślij wiadomość e-mail na zdarzenia w mojej aplikacji
Załóżmy, że chcesz otrzymasz wiadomość e-mail, gdy wystąpi określone zdarzenie. Usługi Application Insights nie obsługuje tej funkcji bezpośrednio, ale może [Wyślij alert, gdy metryki przekracza próg](app-insights-alerts.md).

Alerty można ustawić na [metryki niestandardowe](app-insights-api-custom-events-metrics.md#trackmetric), ale nie niestandardowych zdarzeń. Napisanie kodu, aby zwiększyć metrykę, gdy wystąpi zdarzenie:

    telemetry.TrackMetric("Alarm", 10);

Lub:

    var measurements = new Dictionary<string,double>();
    measurements ["Alarm"] = 10;
    telemetry.TrackEvent("status", null, measurements);

Ponieważ alerty mają dwa stany, konieczne będzie wysyłać niską wartość, gdy należy wziąć pod uwagę w alercie, aby zostać zakończone:

    telemetry.TrackMetric("Alarm", 0.5);

Tworzenie wykresu w [Eksploratora metryk](app-insights-metrics-explorer.md) wyświetlić Twojej alarmu:

![](./media/app-insights-how-do-i/010-alarm.png)

Teraz ustawić alert, aby zostać wywołane podczas Metryka wykraczają poza wartość mid przez krótki czas:

![](./media/app-insights-how-do-i/020-threshold.png)

Ustaw okres uśredniania do minimum.

Będziesz otrzymywać wiadomości e-mail, zarówno Metryka systemowi powyżej i poniżej progu.

Niektóre kwestie do rozważenia:

* Alert ma dwa stany ("alertu" i "dobrej kondycji"). Stan jest oceniane tylko wtedy, gdy otrzyma metryki.
* Wiadomości e-mail są wysyłane tylko wtedy, gdy stan zmieni się. Jest to Dlaczego należy wysyłać wysokiej i niskiej wartości metryki.
* Aby ocenić ten alert, średnia jest pobierana odebranej wartości w poprzednim okresie. Występuje za każdym razem, gdy Metryka zostanie odebrana, więc częściej niż okres, w którym można ustawić można wysłać wiadomości e-mail.
* Ponieważ wiadomości e-mail są wysyłane zarówno w "alertu" i "dobrej kondycji", można rozważyć ponowne myśląc jednorazowej wydarzenia jako warunek dwustanowy. Na przykład zamiast "zadanie ukończone" zdarzenia, mieć warunek "zadanie w toku", gdzie otrzymywać wiadomości e-mail na początku i na końcu zadania.

### <a name="set-up-alerts-automatically"></a>Automatyczne konfigurowanie alertów
[Użyj programu PowerShell, aby utworzyć nowe alerty](app-insights-alerts.md#automation)

## <a name="use-powershell-to-manage-application-insights"></a>Za pomocą programu PowerShell do zarządzania usługi Application Insights
* [Tworzenie nowych zasobów](app-insights-powershell-script-create-resource.md)
* [Utwórz nowe alerty](app-insights-alerts.md#automation)

## <a name="separate-telemetry-from-different-versions"></a>Oddzielne dane telemetryczne z różnych wersji

* Wiele ról w aplikacji: Użyj pojedynczego zasobu usługi Application Insights i odfiltrować cloud_Rolename. [Dowiedz się więcej](app-insights-monitor-multi-role-apps.md)
* Oddzielanie programowanie, badanie i wersje: Korzystanie z różnych zasobów usługi Application Insights. Wybierz klucze Instrumentacji z pliku web.config. [Dowiedz się więcej](app-insights-separate-resources.md)
* Raportowanie kompilacji wersji: Dodawanie właściwości przy użyciu inicjatora telemetrii. [Dowiedz się więcej](app-insights-separate-resources.md)

## <a name="monitor-backend-servers-and-desktop-apps"></a>Monitorowanie serwerów wewnętrznej bazy danych i aplikacji klasycznych
[Użyć modułu programu Windows Server SDK](app-insights-windows-desktop.md).

## <a name="visualize-data"></a>Wizualizuj dane
#### <a name="dashboard-with-metrics-from-multiple-apps"></a>Pulpit nawigacyjny z metryki z wielu aplikacji
* W [Explorer Metryka](app-insights-metrics-explorer.md)należy dostosować wykres i zapisać ją jako ulubioną. Przypiąć go do pulpitu nawigacyjnego platformy Azure.

#### <a name="dashboard-with-data-from-other-sources-and-application-insights"></a>Pulpit nawigacyjny z danymi z innych źródeł i usługi Application Insights
* [Eksportuj dane telemetryczne do usługi Power BI](app-insights-export-power-bi.md).

Lub

* Użyj programu SharePoint jako pulpitu nawigacyjnego, wyświetlanie danych w części sieci web programu SharePoint. [Za pomocą Eksport ciągły i Stream Analytics można wyeksportować do bazy danych SQL](app-insights-code-sample-export-sql-stream-analytics.md).  Użyj PowerView Sprawdź bazy danych i tworzenie składnika web part programu SharePoint dla PowerView.

<a name="search-specific-users"></a>

### <a name="filter-out-anonymous-or-authenticated-users"></a>Filtrowanie anonimowe lub uwierzytelnionych użytkowników
Jeśli użytkownicy logują się, możesz ustawić [uwierzytelniony identyfikator użytkownika](app-insights-api-custom-events-metrics.md#authenticated-users). (Go nie jest realizowane automatycznie.)

Następnie możesz:

* Wyszukaj identyfikatorów określonego użytkownika

![](./media/app-insights-how-do-i/110-search.png)

* Metryki filtr do anonimowych lub uwierzytelnionych użytkowników

![](./media/app-insights-how-do-i/115-metrics.png)

## <a name="modify-property-names-or-values"></a>Modyfikowanie nazwy właściwości lub wartości
Utwórz [filtru](app-insights-api-filtering-sampling.md#filtering). Dzięki temu można zmodyfikować lub będzie filtrować dane telemetryczne przed wysłaniem go z aplikacji do usługi Application Insights.

## <a name="list-specific-users-and-their-usage"></a>Listy poszczególnych użytkowników i ich użycia
Jeśli chcesz, aby [wyszukiwanie określonych użytkowników](#search-specific-users), można ustawić [uwierzytelniony identyfikator użytkownika](app-insights-api-custom-events-metrics.md#authenticated-users).

Jeśli chcesz listę użytkowników z danymi, takich jak strony przeglądania lub częstotliwość logowania, dostępne są dwie opcje:

* [Identyfikator użytkownika uwierzytelnionego zestawu](app-insights-api-custom-events-metrics.md#authenticated-users), [wyeksportować do bazy danych](app-insights-code-sample-export-sql-stream-analytics.md) i użyć odpowiedniego narzędzia do analizowania danych użytkownika.
* Jeśli masz niewielką liczbę użytkowników, wysyłanie zdarzeń niestandardowych lub metryki, przy użyciu potrzebne dane jako nazwa metryki wartość lub zdarzenie, a ustawienie użytkownika jako właściwość. Do analizowania wyświetleń strony, Zastąp standardowe wywołanie trackPageView JavaScript. Do analizowania telemetrii po stronie serwera, użyj inicjatora telemetrii, aby dodać identyfikator użytkownika do wszystkie dane telemetryczne serwera. Następnie można filtru i segmentu metryki i wyszukiwania identyfikatora użytkownika.

## <a name="reduce-traffic-from-my-app-to-application-insights"></a>Zmniejszenie ruchu z mojej aplikacji do usługi Application Insights
* W [ApplicationInsights.config](app-insights-configuration-with-applicationinsights-config.md), wyłącz wszystkie moduły nie jest konieczne, takie zbierającym licznika wydajności.
* Użyj [próbkowania i filtrowanie](app-insights-api-filtering-sampling.md) w zestawie SDK.
* Na stronach sieci web limitu liczby wywołań Ajax zgłoszone dla każdego widoku strony. We fragmencie kodu skryptu po `instrumentationKey:...` , Wstaw: `,maxAjaxCallsPerView:3` (lub odpowiednią ilość).
* Jeśli używasz [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric), obliczeniowe agregacji partie wartości metryki przed wysłaniem wynik. Brak przeciążenia TrackMetric() udostępniający tego.

Dowiedz się więcej o [ceny i przydziały](app-insights-pricing.md).

## <a name="disable-telemetry"></a>Wyłączanie telemetrii
Aby **dynamicznie zatrzymywania i uruchamiania** zbierania i przekazywania danych telemetrycznych z serwera:

```

    using  Microsoft.ApplicationInsights.Extensibility;

    TelemetryConfiguration.Active.DisableTelemetry = true;
```



Aby **wyłączyć wybranego standardowe moduły zbierające** — na przykład liczniki wydajności, żądań HTTP lub zależności - Usuń lub komentarz w odpowiednich wierszach [ApplicationInsights.config](app-insights-api-custom-events-metrics.md). Można to zrobisz, na przykład, jeśli chcesz wysyłać dane TrackRequest.

## <a name="view-system-performance-counters"></a>Przeglądanie liczników wydajności systemu
Wśród metryk, które można wyświetlić w Eksploratorze metryk są zestawem systemu liczników wydajności. Jest wstępnie zdefiniowanych bloku zatytułowany **serwerów** wyświetlający kilka z nich.

![Otwórz zasobu usługi Application Insights i kliknij pozycję serwery](./media/app-insights-how-do-i/121-servers.png)

### <a name="if-you-see-no-performance-counter-data"></a>Jeśli zostanie wyświetlony nie danych licznika wydajności
* **Serwer usług IIS** na własnym komputerze lub na maszynie Wirtualnej. [Zainstaluj Monitor stanu](app-insights-monitor-performance-live-website-now.md).
* **Witryny sieci web Azure** — nie obsługujemy jeszcze liczników wydajności. Istnieje kilka miar, który można uzyskać w ramach standardowego witryny sieci web platformy Azure w Panelu sterowania.
* **Serwer UNIX** - [zainstalować collectd](app-insights-java-collectd.md)

### <a name="to-display-more-performance-counters"></a>Aby wyświetlić więcej liczniki wydajności
* Najpierw [Dodaj nowy wykres](app-insights-metrics-explorer.md) i zobacz, czy licznik jest podstawowego zestawu, które są oferowane.
* Jeśli nie, [dodać licznik do zestawu zebrane przez modułu licznika wydajności](app-insights-performance-counters.md).
