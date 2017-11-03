---
title: "Analizować wzorce nawigacji użytkownika z przepływów użytkownika w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Przeanalizuj, jak użytkownicy nawigują między stronami i funkcje aplikacji sieci web."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 08/02/2017
ms.author: cfreeman
ms.openlocfilehash: d17ed3dff08f00a1d6a2108608e42b29f95fbd84
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="analyze-user-navigation-patterns-with-user-flows-in-application-insights"></a>Analizować wzorce nawigacji użytkownika z przepływów użytkownika w usłudze Application Insights

![Narzędzia przepływu użytkownika Insights aplikacji](./media/app-insights-usage-flows/flows.png)

Narzędzia przepływu użytkownika wizualizuje, jak użytkownicy nawigują między stronami i funkcje witryny. Ten program jest doskonały dla udzielenie odpowiedzi na pytania, takich jak:
* Jak użytkownicy opuścić stronę w witrynie?
* Co użytkownicy kliknij na stronie w witrynie?
* Gdzie są miejsc, w których użytkownicy churn — najbardziej z witryny?
* Czy istnieją miejsca, w którym użytkownicy powtórzyć tę samą akcję samodzielnego?

Narzędzia przepływu użytkownika rozpoczyna się od strony początkowej widoku lub zdarzenie, które określisz. Podana tego widoku lub niestandardowych zdarzeń, przepływa użytkownika zawiera wyświetleń strony i niestandardowych zdarzeń wysłanych przez użytkowników natychmiast później podczas sesji, dwa kroki później, itd. Wiersze z różnymi grubość zawierają ile razy każda ścieżka użytego przez użytkowników. Specjalne węzły "Sesja została zakończona" Pokaż, ilu użytkowników wysyłane nie wyświetleń strony lub niestandardowych zdarzeń po poprzednim węźle wyróżnianie, gdzie użytkownicy prawdopodobnie pozostałych lokacji.



> [!NOTE]
> Zasób usługi Application Insights musi zawierać wyświetleń strony lub zdarzeń niestandardowych w celu za pomocą narzędzia przepływu użytkownika. [Dowiedz się, jak skonfigurować aplikację do zbierania wyświetleń strony automatycznie z zestawu SDK usługi Application Insights JavaScript](app-insights-javascript.md).
> 
> 

## <a name="start-by-choosing-an-initial-page-view-or-custom-event"></a>Rozpocznij, wybierając pozycję Wyświetl stronę początkową lub zdarzenie niestandardowe

![Wybierz zdarzenie początkowej dla przepływu użytkownika](./media/app-insights-usage-flows/flows-initial-event.png)

Aby rozpocząć, udzielenie odpowiedzi na pytania za pomocą narzędzia przepływu użytkownika, wybierz widoku strony początkowej lub niestandardowe zdarzenie jako punkt początkowy wizualizacji:
1. Kliknij łącze w "co zrobić użytkownicy po...?" tytuł, lub kliknij przycisk Edytuj. 
2. Wybierz widok strony lub niestandardowe zdarzenie z listy rozwijanej "Początkowego event".
3. Kliknij przycisk "Utwórz wykresu".

Kolumna "Krok 1" wizualizację pokazuje, co użytkowników czy najczęściej zaraz po zdarzeniu początkowej uporządkowane od najbardziej do najmniej występująca góry do dołu. "Krok 2" i kolumny kolejnych Pokaż, co użytkownicy czy, tworzenie obrazu wszystkich użytkowników sposoby przeszedł za pośrednictwem swojej witryny.

Domyślnie narzędzie użytkownika przepływów losowo próbek tylko ostatnich 24 godzinach wyświetleń strony i niestandardowe zdarzenie z witryny. Można zwiększyć zakres czasu i zmienić saldo wydajności i dokładność losowych próbek w menu Edycja.

Jeśli strona widoków i zdarzeń niestandardowych nie są istotne dla Ciebie, kliknij przycisk "X" w węzłach, które chcesz ukryć. Po wybraniu węzły, które chcesz ukryć, kliknij przycisk "Utwórz wykresu" poniżej wizualizacji. Aby wyświetlić wszystkie węzły, które zostały ukryte, kliknij przycisk Edytuj, a następnie przyjrzeć się w sekcji "Wykluczone zdarzenia".

Jeśli wyświetleń strony lub niestandardowych zdarzeń brakuje który powinny być widoczne na wizualizację:
* Sprawdź sekcję "Wykluczone zdarzenia" w menu Edycja.
* Formant "Poziom szczegółowości" w menu Edycja aby obejmował zdarzenia rzadziej do wizualizacji.
* Jeśli widok strony lub niestandardowe zdarzenie oczekiwane jest rzadko wysyłane przez użytkowników, spróbuj zwiększyć zakres czasu wizualizacji, w menu Edycja.
* Wyświetlenie strony upewnij się, że lub skonfigurowano niestandardowe zdarzenie oczekiwane mają zostać zebrane przez zestaw SDK usługi Application Insights w kodzie źródłowym witryny. [Dowiedz się więcej o zbieraniu niestandardowego zdarzenia.](app-insights-api-custom-events-metrics.md)

Jeśli chcesz zobaczyć więcej czynności do wizualizacji, za pomocą suwaka "Liczba kroków" w menu Edycja.

## <a name="after-visiting-a-page-or-feature-where-do-users-go-and-what-do-they-click"></a>Po odwiedzeniu strony lub funkcji, użytkownicy gdzie i co użytkownik klika polecenie?

![Zapoznaj się z przepływów użytkownika, aby zrozumieć, gdy użytkownik kliknie](./media/app-insights-usage-flows/flows-one-step.png)

Początkowej zdarzenie w przypadku widoku strony, pierwsza kolumna wizualizacji ("krok 1") jest szybko poznać użytkowników czy natychmiast po odwiedzania strony. Spróbuj otworzyć witryny w oknie obok wizualizacji przepływu użytkownika. Porównaj Twoje oczekiwania interakcji użytkowników z stronę, aby listę zdarzeń, w kolumnie "Krok 1". Często elementu interfejsu użytkownika na stronie wydaje się nieważny do zespołu można między najczęściej używanych na stronie. Można go doskonały punkt początkowy ulepszenia projektu do witryny.

Początkowa zdarzenie w przypadku niestandardowych zdarzeń, pierwsza kolumna zawiera użytkowników czy tylko po wykonaniu tej akcji. Podobnie jak w przypadku wyświetleń strony, należy wziąć pod uwagę czy obserwowanych zachowania użytkowników zgodne oczekiwań i celów Twojego zespołu. Jeśli wybrane zdarzenie początkowej jest "Dodaje element do koszyka", na przykład znaleźć czy "Przejdź do wyewidencjonowania" i "Zakupu ukończone" są wyświetlane w krótkim czasie wizualizacji. Jeśli zachowania użytkownika znacznie różni się od Twoich oczekiwań, użyj wizualizacji, aby zrozumieć, jak użytkownicy uzyskują "spowodowane" bieżący projekt sieci Web.

## <a name="where-are-the-places-that-users-churn-most-from-your-site"></a>Gdzie są miejsc, w których użytkownicy churn — najbardziej z witryny?

Obserwowanie "Sesja została zakończona" węzły wyświetlane wysokiej w górę w kolumnie wizualizacji, szczególnie w strumieniu. Oznacza to, w przypadku wielu użytkowników, prawdopodobnie churned z lokacji po wykonaniu powyższa ścieżka stron i interakcje interfejsu użytkownika. Czasami przenoszenie oczekiwano — po zakończeniu zakupu w witrynie handlu elektronicznego, na przykład -, ale zazwyczaj zmian jest znak problemy związane z projektem, niska wydajność lub inne problemy z witryny można zwiększyć.

Należy pamiętać, że "sesja została zakończona" węzły są oparte tylko na dane telemetryczne zebrane przez ten zasób usługi Application Insights. Jeśli usługi Application Insights nie odbiera dane telemetryczne dla niektórych interakcji użytkownika, użytkownicy nadal można mieć interakcji z witryny w sposób, po przepływu użytkownika narzędzia mówi sesja została zakończona.

## <a name="are-there-places-where-users-repeat-the-same-action-over-and-over"></a>Czy istnieją miejsca, w którym użytkownicy powtórzyć tę samą akcję samodzielnego?

Poszukaj widok strony lub niestandardowe zdarzenie powtarzające się w kolejnych krokach wizualizację przez wielu użytkowników. Zwykle oznacza to, że użytkownicy wykonują powtarzających się czynności w witrynie. Odnalezienie powtarzania pomyśleć o zmianę projektu witryny sieci lub dodanie nowych funkcji, aby zmniejszyć powtarzania. Jeśli znajdziesz użytkowników wykonywania powtarzających się czynności w każdym wierszu elementu tabeli, na przykład dodać funkcje edycji zbiorczej.

## <a name="common-questions"></a>Często zadawane pytania

### <a name="why-do-steps-appear-disconnected"></a>Dlaczego kroki są wyświetlane bez połączenia?

![Użytkownik przepływów mających odłączonego kroki](./media/app-insights-usage-flows/flows-disconnected.png)

Odłączeniu czynności (kolumn) w wizualizacji przepływu użytkownika oznacza to, że żaden z przez użytkowników między kroki nie był częste, który będzie wyświetlany. Aby wyświetlić mniej częste zdarzenia na wizualizację kroki wyświetlane połączenie, suwak "Poziom szczegółowości" w menu Edycja.

### <a name="does-the-initial-event-represent-the-first-time-the-event-appears-in-a-session-or-any-time-it-appears-in-a-session"></a>Nie odpowiadają początkowej zdarzeń po raz pierwszy w sesji pojawi się zdarzenie lub dowolnym momencie, w jakiej występuje w sesji?

Początkowa zdarzeń na wizualizację reprezentuje tylko wysłał tego widoku strony lub niestandardowe zdarzenie podczas sesji po raz pierwszy. Jeśli użytkownicy mogą wysyłać zdarzenia początkowej wiele razy w sesji, a następnie kolumny "Krok 1" wyświetlane są tylko zachowanie użytkowników po *pierwszy* wystąpienie zdarzenia początkowej, nie wszystkie wystąpienia.

## <a name="next-steps"></a>Następne kroki

* [Przegląd wykorzystania](app-insights-usage-overview.md)
* [Użytkownikami, sesjami i zdarzenia](app-insights-usage-segmentation.md)
* [Przechowywanie](app-insights-usage-retention.md)
* [Dodawanie zdarzeń niestandardowych do aplikacji](app-insights-api-custom-events-metrics.md)