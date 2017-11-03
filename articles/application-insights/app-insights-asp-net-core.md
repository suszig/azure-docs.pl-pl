---
title: Azure Application Insights dla platformy ASP.NET Core | Dokumentacja firmy Microsoft
description: "Monitorowanie aplikacji sieci web dla dostępności, wydajności i użycia."
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: 74f99dd6f31ecff7c838d8f710a7fe4279ce0ea9
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="application-insights-for-aspnet-core"></a>Usługa Application Insights dla aplikacji ASP.NET Core
[Usługa Application Insights](app-insights-overview.md) umożliwia monitorowanie aplikacji sieci web, dostępności, wydajności i użycia. Dzięki uzyskiwanym opiniom dotyczącym wydajności i skuteczności aplikacji możesz dokonać opartych na informacjach wyborów dotyczących kierunku projektu w każdym cyklu życia.

![Przykład](./media/app-insights-asp-net-core/sample.png)

Będziesz potrzebować subskrypcji z [Microsoft Azure](http://azure.com). Zaloguj się przy użyciu konta Microsoft, które możesz mieć dla systemu Windows, usługi XBox Live lub innych usług w chmurze firmy Microsoft. Zespół może być organizacyjnej subskrypcji Azure: Poproś właściciela o należy dodać do niej przy użyciu konta Microsoft.

## <a name="getting-started"></a>Wprowadzenie

* W Eksploratorze rozwiązań programu Visual Studio, kliknij prawym przyciskiem myszy projekt i wybierz **Konfiguruj usługę Application Insights**, lub **Dodaj > usługi Application Insights**. [Dowiedz się więcej](app-insights-asp-net.md).
* Jeśli nie widzisz tych poleceń menu, wykonaj [ręcznego pobierania Przewodnik wprowadzający](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Getting-Started). Należy to zrobić, jeśli projekt został utworzony przy użyciu wersji programu Visual Studio przed 2017 r.

## <a name="using-application-insights"></a>Korzystanie z usługi Application Insights
Zaloguj się do [portalu Microsoft Azure](https://portal.azure.com), wybierz pozycję **wszystkie zasoby** lub **usługi Application Insights**, a następnie wybierz zasób został utworzony w celu monitorowania aplikacji.

W osobnym oknie przeglądarki należy użyć aplikacji przez pewien czas. Zostanie wyświetlone dane znajdujące się na wykresach usługi Application Insights. (Może być konieczne kliknij przycisk Odśwież.) Będzie niewielką ilość danych podczas projektujesz, ale te wykresy naprawdę pochodzić aktywności podczas publikowania aplikacji i wielu użytkowników. 

Na stronie Przegląd znajdują się wykresy wydajności: czas odpowiedzi serwera, czas ładowania strony i liczby żądań zakończonych niepowodzeniem. Kliknij przycisk dowolnego wykresu, aby zobaczyć więcej wykresów i danych.

Widoki w portalu można podzielić na trzy główne kategorie:

* [Eksploratora metryk](app-insights-metrics-explorer.md) przedstawia wykres i tabelę metryki i liczby, takie jak czas reakcji, awariami lub metryki utworzone samodzielnie z [interfejsu API](app-insights-api-custom-events-metrics.md). Filtrowanie i segmentu danych przez wartości właściwości, aby uzyskać lepsze zrozumienie aplikacji i jej użytkowników.
* [Eksplorator wyszukiwania](app-insights-diagnostic-search.md) zawiera listę poszczególnych zdarzeń, takich jak określone żądania, wyjątki, ślady dziennika lub zdarzenia utworzone samodzielnie z [interfejsu API](app-insights-api-custom-events-metrics.md). Filtrowanie i wyszukiwanie w zdarzeniach i nawigowanie między powiązanych zdarzeń do badania problemów.
* [Analiza](app-insights-analytics.md) pozwala uruchamiać zapytania SQL przypominającej za pośrednictwem telemetrii i jest zaawansowanym narzędziem analityczne i diagnostycznych.

## <a name="alerts"></a>Alerty
* Automatycznie Pobierz [aktywne alerty diagnostycznych](app-insights-proactive-diagnostics.md) który informujące o nietypowych zmian awariami i innych metryk.
* Konfigurowanie [testów dostępności](app-insights-monitor-web-app-availability.md) do testowania witryny sieci Web stale z lokalizacji na całym świecie i otrzymywać wiadomości e-mail, gdy tylko jedną testowania kończy się niepowodzeniem.
* Konfigurowanie [metryki alerty](app-insights-monitor-web-app-availability.md) wiedzieć, jeśli metryk, takich jak czas odpowiedzi lub stawki wyjątek Przejdź poza akceptowalnych limitów.

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="open-source"></a>Oprogramowanie typu „open source”
[Przeczytaj i brać udział w kodzie](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)


## <a name="next-steps"></a>Następne kroki
* [Dodaj telemetrię do stron sieci web](app-insights-javascript.md) monitorowanie strony użycia i wydajności.
* [Monitor zależności](app-insights-asp-net-dependencies.md) aby zobaczyć, jeśli REST, SQL lub innych zasobów zewnętrznych są spowolnieniem użytkownik.
* [Za pomocą interfejsu API](app-insights-api-custom-events-metrics.md) wysyłanie własnych zdarzenia i metryki dla bardziej szczegółowy widok wydajności i użycia aplikacji.
* [Badania dostępności](app-insights-monitor-web-app-availability.md) Sprawdź aplikację stale z całego świata. 

