---
title: "Wpływ użycia szczegółowych informacji w aplikacji Azure | Dokumentacja firmy Microsoft"
description: "Analizowanie jak inne właściwości potencjalnie wpływ szybkości konwersji dla części aplikacji."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 01/25/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: d76db02647ce878343f60fc84cf063c5b7833438
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="impact-analysis-with-application-insights"></a>Analiza wpływu z usługą Application Insights

Wpływ analizuje, jak czasy ładowania i inne właściwości wpływ szybkości konwersji dla poszczególnych części aplikacji. Aby umieścić dokładniej, odnajduje jak **żadnego wymiaru** z **widok strony**, **zdarzenie niestandardowe**, lub **żądania** ma wpływ na użytkowanie różne **widok strony** lub **niestandardowe zdarzenie**. 

![Narzędzie wpływu](./media/app-insights-usage-impact/0001-impact.png)

## <a name="still-not-sure-what-impact-does"></a>Nadal nie masz pewności co wpływ oznacza?

Jest jednym ze sposobów wziąć pod uwagę wpływ jako ultimate narzędzia określania argumentów innej osobie w zespole o jak wpływających na powolność w pewien aspekt witryny czy użytkownicy trzymaj. Podczas gdy użytkownicy może tolerować pewnego powolność, wpływ udostępnia wgląd w jak najlepiej równoważyć optymalizacji i wydajność, aby zmaksymalizować konwersji użytkownika.

Ale analizowanie wydajności jest tylko podzestaw możliwości wpływu na. Ponieważ wpływ obsługuje niestandardowe zdarzenia i wymiarów, odpowiadanie na pytania, tak jak wybór użytkownika przeglądarki użytymi z różnym tempie konwersji są zaledwie kilku kliknięć.

![Zrzut ekranu konwersji przez przeglądarki](./media/app-insights-usage-impact/0004-browsers.png)

> [!NOTE]
> Zasób usługi Application Insights musi zawierać wyświetleń strony lub zdarzeń niestandardowych w celu użycia narzędzia efektów. [Dowiedz się, jak skonfigurować aplikację do zbierania wyświetleń strony automatycznie z zestawu SDK usługi Application Insights JavaScript](app-insights-javascript.md). Należy również pamiętać, że ponieważ analizowania korelacji, rozmiar próbki jest ważne.
>
>

## <a name="is-page-load-time-impacting-how-many-people-convert-on-my-page"></a>Jest czas ładowania strony wpływające na ile osób Konwertuj na stronie?

Aby rozpocząć, udzielenie odpowiedzi na pytania za pomocą narzędzia wpływ, wybierz widok strony początkowej, niestandardowe zdarzenie lub żądanie.

![Narzędzie wpływu](./media/app-insights-usage-impact/0002-dropdown.png)

1. Wybierz widok strony z **dla widoku strony** listy rozwijanej.
2. Pozostaw **analizowanie jak jego** listy rozwijanej na domyślny wybór **czas trwania** (w tym kontekście **czas trwania** jest aliasem **czas ładowania strony**.)
3. Dla **ma wpływ na wykorzystanie** listy rozwijanej wybierz zdarzenie niestandardowe. To zdarzenie powinna odpowiadać elementu interfejsu użytkownika w widoku strony, które wybrano w kroku 1.

![Zrzut ekranu przedstawiający wyników](./media/app-insights-usage-impact/0003-results.png)

W tym wystąpieniu jako **stronę produktu** czas ładowania zwiększa współczynnik konwersji do **produktu zakupu kliknięty** przestanie działać. W oparciu o dystrybucji powyżej, czas trwania obciążenia optymalne strony 3.5 sekund można docelowe uzyskanie potencjalnych współczynnik konwersji 55%. Dalsze ulepszenia wydajności, aby skrócić czas ładowania poniżej sekund 3.5 nie obecnie skorelowany z konwersji dodatkowe korzyści.

## <a name="what-if-im-tracking-page-views-or-load-times-in-custom-ways"></a>Co zrobić, jeśli monitorowane wyświetleń strony lub załadować razy w niestandardowy sposób?

Wpływ obsługuje standardowe i niestandardowe właściwości i pomiarów. Użyć dowolnej nazwy. Zamiast czas trwania Aby uzyskać bardziej szczegółowe należy używać filtrów zdarzeń podstawowego i pomocniczego.

## <a name="do-users-from-different-countries-or-regions-convert-at-different-rates"></a>Użytkownicy z różnych krajów lub regionów należy konwertować w różnym tempie?

1. Wybierz widok strony z **dla widoku strony** listy rozwijanej.
2. Wybierz "Kraju lub regionu" w **analizowanie jak jego** listy rozwijanej
3. Aby uzyskać **ma wpływ na wykorzystanie** listy rozwijanej wybierz zdarzenie niestandardowe, który odpowiada elementu interfejsu użytkownika w widoku strony wybranymi w kroku 1.

W takim przypadku wyniki nie pasuje do modelu ciągłego osi x tak samo jak w pierwszym przykładzie. Zamiast tego są prezentowane wizualizacji podobny do segmentowanych lejka. Sortuj według **użycia** Aby wyświetlić zmiany konwersji na Twoje niestandardowe zdarzenia na podstawie kraju.


## <a name="how-does-the-impact-tool-calculate-these-conversion-rates"></a>Jak narzędzie wpływ obliczyć tych wskaźników konwersji?

Pod maską, narzędzie wpływ zależy od [współczynnik korelacji] (https://en.wikipedia.org/wiki/Pearson_correlation_coefficient). Wyniki są obliczane od -1 do 1 na -1 reprezentujący zero korelacji i 1 reprezentujący dodatnią korelacji.

Podział podstawowe działania analizy wpływu wygląda następująco:

Let _A_ = strony głównej widoku/niestandardowe zdarzenie/żądanie należy wybrać w pierwszej listy rozwijanej. (**Dla widoku strony**).

Let _B_ = zdarzeń widoku/niestandardowe strony dodatkowej, należy wybrać (**ma wpływ na wykorzystanie**).

Wpływ analizuje próbkę wszystkie sesje użytkowników w wybranym zakresie czasu. Dla każdej sesji wygląda dla każdego wystąpienia _A_.

Sesje następnie jest dzielony na dwóch różnych rodzajów z _subsessions_ na podstawie jednej z dwóch sytuacji:

- Sesję przekonwertowanego podrzędną składa się z sesją kończąc _B_ zdarzeń i obejmują wszystkie _A_ zdarzeń występujących w systemach wcześniejszych niż _B_.
- Nieprzekonwertowane subsession występuje podczas wszystkich _A_obiektu wystąpią, unikając terminal _B_.

Ostatecznie obliczania wpływ zależy czy możemy analizowanie Metryka lub wymiaru. Dla wszystkich metryki _A_obiektu w sesję podrzędną zostały uśrednione. Natomiast dla wymiarów wartość każdego _A_ wspiera _1/N_ wartość przypisana do _B_ gdzie _N_ jest liczba _A_obiektu w sesję nadrzędną.

## <a name="next-steps"></a>Kolejne kroki

- Aby umożliwić korzystanie z użycia, Rozpocznij wysyłanie [zdarzeń niestandardowych](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) lub [wyświetlenia strony](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Jeżeli już zdarzeń niestandardowych lub wyświetleń strony, Poznaj narzędzia użycia, aby dowiedzieć się, jak używać usługi przez użytkowników.
    - [Lejki](usage-funnels.md)
    - [Przechowywanie](app-insights-usage-retention.md)
    - [User Flows (Przepływy użytkowników)](app-insights-usage-flows.md)
    - [Skoroszyty](app-insights-usage-workbooks.md)
    - [Dodaj kontekstu użytkownika](app-insights-usage-send-user-context.md)