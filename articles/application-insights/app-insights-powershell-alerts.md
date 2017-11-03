---
title: "Ustawianie alertów w usłudze Application Insights przy użyciu programu Powershell | Dokumentacja firmy Microsoft"
description: "Zautomatyzować konfigurację Application Insights, aby otrzymywać wiadomości e-mail o zmianach metryki."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: mbullwin
ms.openlocfilehash: b90a540afd1c2815db8f5a99ee210ce21ea4d874
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="use-powershell-to-set-alerts-in-application-insights"></a>Ustawianie alertów w usłudze Application Insights przy użyciu programu PowerShell
Można zautomatyzować konfigurowanie [alerty](app-insights-alerts.md) w [usługi Application Insights](app-insights-overview.md).

Ponadto można [Ustaw elementów webhook można zautomatyzować Twojej odpowiedzi na alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

> [!NOTE]
> Jeśli chcesz utworzyć zasobów i alertów w tym samym czasie, należy wziąć pod uwagę [przy użyciu szablonu usługi Azure Resource Manager](app-insights-powershell.md).
>
>

## <a name="one-time-setup"></a>Jednorazowej konfiguracji
Jeśli nie użyto programu PowerShell z subskrypcją platformy Azure przed:

Zainstaluj moduł Azure Powershell na komputerze, na którym chcesz uruchomić skrypty.

* Zainstaluj [Instalatora platformy sieci Web firmy Microsoft (w wersji 5 lub nowszej)](http://www.microsoft.com/web/downloads/platform.aspx).
* Użyć go do zainstalowania programu Microsoft Azure Powershell

## <a name="connect-to-azure"></a>Nawiązywanie połączenia z usługą Azure
Uruchom program Azure PowerShell i [nawiązać połączenia z subskrypcją](/powershell/azure/overview):

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a>Uzyskiwanie alertów
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Dodawanie alertu
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a>Przykład 1
Wyślij mi wiadomość e-mail, jeśli odpowiedzi serwera na żądania HTTP, średnio ponad 5 minut, jest mniejsza niż 1 sekunda. Moje zasobu usługi Application Insights jest nazywany IceCreamWebApp i jest w grupie zasobów firmy Fabrikam. Jestem właścicielem subskrypcji platformy Azure.

Identyfikator GUID jest Identyfikatorem subskrypcji (nie klucza instrumentacji aplikacji).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if the server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Przykład 2
Używana aplikacja, I użyj [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) zgłoszenia metrykę o nazwie "salesPerHour." Wyślij wiadomość e-mail do moich współpracowników, gdy "salesPerHour" spada poniżej 100, średnio ponad 24 godzin.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Ta zasada może służyć do metryki zgłoszone za pomocą [parametru pomiaru](app-insights-api-custom-events-metrics.md#properties) wywołania innego śledzenia, takie jak TrackEvent lub trackPageView.

## <a name="metric-names"></a>Nazwy metryki
| Nazwa metryki | Nazwa ekranu | Opis |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Wyjątki przeglądarki |Liczba nieprzechwyconych wyjątków zgłoszonych w przeglądarce. |
| `basicExceptionServer.count` |Wyjątki serwera |Liczba nieobsługiwanych wyjątków zgłoszonych przez aplikację |
| `clientPerformance.clientProcess.value` |Czas przetwarzania klienta |Czas między odebraniem ostatniego bajtu dokumentu do momentu załadowania modelu DOM. Żądania asynchroniczne mogą nadal być przetwarzane. |
| `clientPerformance.networkConnection.value` |Czas połączenia sieciowego podczas ładowania strony |Czas przeglądarki do nawiązania połączenia z siecią. Może być równa 0, jeśli w pamięci podręcznej. |
| `clientPerformance.receiveRequest.value` |Odbieranie czas odpowiedzi |Czas między przeglądarki, wysyłając żądanie do uruchamiania odpowiedź. |
| `clientPerformance.sendRequest.value` |Wyślij czas żądania |Czas trwania przeglądarki można wysłać żądania. |
| `clientPerformance.total.value` |Czas ładowania strony przeglądarki |Czas od wysłania żądania użytkownika do modelu DOM, arkuszy stylów, skryptów i obrazów są ładowane. |
| `performanceCounter.available_bytes.value` |Dostępna pamięć |Pamięć fizyczna dostępna natychmiast dla procesów lub do wykorzystania przez system. |
| `performanceCounter.io_data_bytes_per_sec.value` |Szybkość przetwarzania We/Wy |Całkowita liczba bajtów na sekundę, Odczyt i zapis do plików, sieci i urządzeń. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |szybkość wyjątku |Wyjątków zgłaszanych na sekundę. |
| `performanceCounter.percentage_processor_time.value` |Proces Procesora |Procent czasu wykonywania wszystkich wątków procesów używanych przez procesor do wykonywania instrukcji dla procesu aplikacji. |
| `performanceCounter.percentage_processor_total.value` |Czas procesora |Procent czasu, jaki procesor zużywa w Aktywne wątki. |
| `performanceCounter.process_private_bytes.value` |Bajtów prywatnych procesu |Pamięć przypisana wyłącznie do procesów monitorowanej aplikacji. |
| `performanceCounter.request_execution_time.value` |Czas wykonania żądania programu ASP.NET |Czas wykonania ostatniego żądania. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET żądań w kolejce wykonywania |Długość kolejki żądań aplikacji. |
| `performanceCounter.requests_per_sec.value` |Współczynnik żądań ASP.NET |Częstotliwość wszystkich żądań do aplikacji w ciągu sekundy z platformy ASP.NET. |
| `remoteDependencyFailed.durationMetric.count` |Błędy zależności |Liczba nieudanych wywołań zasobów zewnętrznych aplikacji serwera. |
| `request.duration` |Czas odpowiedzi serwera |Czas między odebraniem żądania HTTP i zakończeniem wysyłania odpowiedzi. |
| `request.rate` |Współczynnik żądań |Częstotliwość wszystkich żądań do aplikacji na sekundę. |
| `requestFailed.count` |Żądań zakończonych niepowodzeniem |Liczba żądań HTTP które wywołały kod odpowiedzi > = 400 |
| `view.count` |Liczba wyświetleń strony |Liczba żądań użytkowników klientów dla strony sieci web. Ruchu syntetycznego jest odfiltrowana. |
| {niestandardowe metryki nazwę} |{Nazwa metryki} |Wartość metryki zgłoszone przez [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) lub [pomiarów parametru wywołania śledzenia](app-insights-api-custom-events-metrics.md#properties). |

Metryki są przesyłane przez różnych telemetrii moduły:

| Metryki grupy | Moduł zbierający |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>wyświetl |[Kod JavaScript przeglądarki](app-insights-javascript.md) |
| performanceCounter |[Wydajność](app-insights-configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Zależność](app-insights-configuration-with-applicationinsights-config.md) |
| żądanie,<br/>requestFailed |[Żądanie serwera](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>elementów webhook
Możesz [zautomatyzować Twojej odpowiedzi na alert](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure będzie wywoływać adres sieci web wybranych przez użytkownika, gdy zostanie zgłoszony alert.

## <a name="see-also"></a>Zobacz też
* [Skrypt w celu skonfigurowania usługi Application Insights](app-insights-powershell-script-create-resource.md)
* [Tworzenie usługi Application Insights i zasobów testu sieci web za pomocą szablonów](app-insights-powershell.md)
* [Automatyzowanie sprzężenia Diagnostyka pakietu Microsoft Azure do usługi Application Insights](app-insights-powershell-azure-diagnostics.md)
* [Twoje odpowiedzi na alert automatyzacji](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
