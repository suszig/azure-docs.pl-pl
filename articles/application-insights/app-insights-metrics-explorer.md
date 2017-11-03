---
title: "Eksploracja metryki w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Jak interpretować wykresy Eksploratora metryk i dostosowywanie bloków Eksploratora metryk."
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 1f471176-38f3-40b3-bc6d-3f47d0cbaaa2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: mbullwin
ms.openlocfilehash: 01b45323b74b54da157f4e9f1af783759c121be1
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="exploring-metrics-in-application-insights"></a>Eksploracja metryki w usłudze Application Insights
Metryki w [usługi Application Insights] [ start] są mierzone wartości i liczby zdarzeń, które są wysyłane w danych telemetrycznych z aplikacji. Pomagają wykrywać problemy z wydajnością i obserwować trendy w sposobu używania aplikacji. Brak szeroką gamę standardowe metryki i można również tworzyć własne niestandardowych metryk i zdarzeń.

Liczby metryki i zdarzenia są wyświetlane na wykresach zagregowane wartości, takie jak sum, średnie lub liczby.

Oto przykładowe zbiór wykresów:

![](./media/app-insights-metrics-explorer/01-overview.png)

Wykresy metryki wszędzie możesz znaleźć w portalu usługi Application Insights. W większości przypadków można dostosować, a następnie można dodać więcej wykresów do bloku. W bloku Przegląd kliknij, aby bardziej szczegółowe wykresy, (które mają tytułów, na przykład "Serwery") lub kliknij przycisk **Eksploratora metryk** aby otworzyć nowy blok, w którym można utworzyć niestandardowe wykresy.

## <a name="time-range"></a>Przedział czasu
Możesz zmienić zakres czasu, wykresy lub siatki w bloku.

![Otwarcie bloku Przegląd aplikacji w portalu Azure](./media/app-insights-metrics-explorer/03-range.png)

Jeśli spodziewasz części danych, które jeszcze nie pojawił się, kliknij przycisk Odśwież. Wykresy odświeżania się w odstępach czasu, ale interwałów są dłużej większych przedziałów czasu. Może upłynąć trochę czasu dla danych do trybu za pośrednictwem potoku analizy na wykresie.

Aby powiększyć część wykresu, przeciągnij nad nim:

![Przeciągnij przez część wykresu.](./media/app-insights-metrics-explorer/12-drag.png)

Kliknij przycisk Cofnij Powiększ go przywrócić.

## <a name="granularity-and-point-values"></a>Poziom szczegółowości i punkt wartości
Umieść kursor myszy na wykresie, aby wyświetlić wartości metryk w tym momencie.

![Umieść kursor myszy nad wykresu](./media/app-insights-metrics-explorer/02-focus.png)

Wartość metryki w określonym punkcie jest agregowana w przedziałach próbkowania.

Interwał próbkowania lub "stopnia szczegółowości" jest wyświetlany w górnej części bloku.

![Nagłówek bloku.](./media/app-insights-metrics-explorer/11-grain.png)

Można dostosować poziom szczegółowości w bloku zakres czasu:

![Nagłówek bloku.](./media/app-insights-metrics-explorer/grain.png)

Dostępne szczegółowości zależą od wybranego przedziału czasu. Jawne szczegółowości są dostępne opcje alternatywne na "Automatyczny" szczegółowości dla zakresu czasu.


## <a name="editing-charts-and-grids"></a>Edytowanie siatki i wykresy
Aby dodać nowy wykres do bloku:

![W Eksploratorze metryk wybierz polecenie Dodaj wykresu](./media/app-insights-metrics-explorer/04-add.png)

Wybierz **Edytuj** istniejącego lub nowego wykresu do edycji co zawiera:

![Wybierz co najmniej jedną metrykę](./media/app-insights-metrics-explorer/08-select.png)

Więcej niż jedna Metryka można wyświetlić na wykresie, chociaż istnieją ograniczenia dotyczące połączenia, które mogą być jednocześnie wyświetlane. Gdy tylko zostanie wybrana jedna metryka, niektóre inne są wyłączone.

Jeśli zostanie na stałe [metryki niestandardowe] [ track] w aplikacji (wywołań TrackMetric i TrackEvent) zostaną wyświetlone tutaj.

## <a name="segment-your-data"></a>Segment danych
Można podzielić metrykę przez właściwość — na przykład do porównania wyświetleń strony na klientach w różnych systemach operacyjnych.

Wybierz wykres lub siatkę, przełącz się na grupowanie, a następnie wybierz właściwość do grupy przez:

![Wybierz grupowania na, a następnie ustaw wybierz właściwość Group By](./media/app-insights-metrics-explorer/15-segment.png)

> [!NOTE]
> Gdy używasz grupowania typów obszarów i wykres słupkowy Podaj skumulowany wyświetlania. Jest to odpowiedni który metoda agregacji Sum. Ale typ agregacji w przypadku średniej, wybierz typy ekranu wiersza lub siatki.
>
>

Jeśli zostanie na stałe [metryki niestandardowe] [ track] w swojej aplikacji i zawierają wartości właściwości, można wybrać właściwość na liście.

Wykres jest za mały dla segmentu danych? Dostosować wysokość:

![Suwak](./media/app-insights-metrics-explorer/18-height.png)

## <a name="aggregation-types"></a>Typy agregacji
Legenda po stronie domyślnie zwykle zawiera wartości zagregowanych w okresie wykresu. Jeśli w ustawieniu kursora na wykresie, będzie wyświetlana wartość w tym momencie.

Każdy punkt danych na wykresie jest sumą wartości danych otrzymanych w poprzednim interwale próbkowania lub "stopnia szczegółowości". Stopień szczegółowości jest wyświetlany w górnej części bloku i zależy od ogólnej skali czasu wykresu.

Metryki może być agregowany na różne sposoby:

* **Liczba** jest liczbą zdarzeń odebranych w interwale próbkowania. Służy do zdarzeń, takich jak żądania. Zmiany w wysokość wykresu wskazuje zmienności częstotliwość, z jaką odbywa się zdarzenia. Jednak wartość liczbową zmienia się po zmianie interwału próbkowania.
* **Suma** dodaje wartości punktów danych odebranych za pośrednictwem interwał próbkowania lub okres wykresu.
* **Średnia** dzieli sumy przez liczbę punktów danych odebranych za pośrednictwem interwał.
* **Unikatowy** liczby są używane do liczby użytkowników i kont. Dla interwału próbkowania lub w okresie wykres na ilustracji przedstawiono liczba różnych użytkowników, w tym czasie.
* **%**-Procent wersje każdego agregacji są używane tylko w przypadku wykresów segmentu. Łączną zawsze dodaje do 100%, a wykres przedstawia względny udział różnych składników łącznie.

    ![Wartość procentowa agregacji](./media/app-insights-metrics-explorer/percentage-aggregation.png)

### <a name="change-the-aggregation-type"></a>Zmień typ agregacji

![Edytuj wykres, a następnie wybierz agregacji](./media/app-insights-metrics-explorer/05-aggregation.png)

Domyślną metodą wszystkie metryki jest wyświetlana podczas tworzenia nowego wykresu lub kiedy są wyczyszczone wszystkie metryki:

![Odznacz wszystkie metryki, aby zobaczyć wartości domyślne](./media/app-insights-metrics-explorer/06-total.png)

## <a name="pin-y-axis"></a>Numer PIN osi y 
Domyślnie wykres przedstawia wartości osi Y, zaczynając od zera do maksymalnej wartości zakresu danych, aby zapewnić wizualną reprezentację quantum wartości. Jednak w niektórych przypadkach więcej niż quantum mogą być interesujące wizualnie sprawdzić drobne zmiany w wartości. Dostosowań, takich jak użycie tej osi y zakresu funkcja edycji, aby przypiąć osi y minimalną i maksymalną wartość w odpowiednim miejscu.
Kliknij pole wyboru "Zaawansowane ustawienia", aby wyświetlić ustawienia zakresu osi y

![Kliknij przycisk Zaawansowane ustawienia, wybierz zakres niestandardowy i określić wartości maksymalnej min](./media/app-insights-metrics-explorer/y-axis-range.png)

## <a name="filter-your-data"></a>Filtrowanie danych
Aby wyświetlić tylko metryki wybrany zestaw wartości właściwości:

![Kliknij przycisk Filtr, rozwiń właściwością, a niektóre wartości](./media/app-insights-metrics-explorer/19-filter.png)

Jeśli nie wybierzesz jakiekolwiek wartości dla określonej właściwości, jest taka sama jak wybranie wszystkich: Brak bez filtru dla tej właściwości.

Zwróć uwagę, liczby zdarzeń, które będą widoczne obok każdej wartości właściwości. Po wybraniu wartości jedną właściwość liczby równolegle z wartości innych właściwości zostaną skorygowane.

Filtry są stosowane do wszystkich schematów w bloku. Jeśli mają inną filtry stosowane do różnych wykresów, tworzyć i zapisywać bloków różne metryki. Jeśli chcesz, możesz przypinać wykresów z różnych bloków do pulpitu nawigacyjnego, tak, aby były widoczne obok siebie.

### <a name="remove-bot-and-web-test-traffic"></a>Usuń ruchu testu sieci web i bot
Użyj filtru **ruch rzeczywisty lub syntetyczny** i sprawdź **rzeczywistych**.

Można również filtrować według **źródło ruchu syntetycznego**.

### <a name="to-add-properties-to-the-filter-list"></a>Aby dodać właściwości do listy filtrów
Czy chcesz filtrować dane telemetryczne w kategorii wybranej przez użytkownika? Na przykład może być rozdzielają użytkowników na różne kategorie, a chcesz podzielić dane według tych kategorii.

[Utwórz własną właściwość](app-insights-api-custom-events-metrics.md#properties). Ustaw go w [inicjatora Telemetrii](app-insights-api-custom-events-metrics.md#defaults) aby były wyświetlane wszystkie dane telemetryczne — tym standardowych danych telemetrycznych wysłanych przez różnych modułach zestawu SDK.

## <a name="edit-the-chart-type"></a>Edytuj typ wykresu
Należy zauważyć, że można przełączać się między siatki i wykresy:

![Wybierz wykres lub siatkę, a następnie wybierz typ wykresu](./media/app-insights-metrics-explorer/16-chart-grid.png)

## <a name="save-your-metrics-blade"></a>Zapisz z bloku metryk
Po utworzeniu niektórych typów wykresów, zapisać je jako ulubione. Czy mają być udostępniane innym członkom zespołu można wybrać, jeśli używasz konta organizacyjnego.

![Wybierz ulubione](./media/app-insights-metrics-explorer/21-favorite-save.png)

Aby ponownie wyświetlić bloku **przejdź do bloku omówienie** , a następnie otwórz Ulubione:

![W bloku omówienie wybierz Ulubione](./media/app-insights-metrics-explorer/22-favorite-get.png)

Jeśli została wybrana opcja zakresu względne czasu, po zapisaniu, bloku będzie zaktualizowano o najnowsze metryki. Jeśli wybrano zakresu czasu bezwzględnego, jego wyświetli te same dane zawsze.

## <a name="reset-the-blade"></a>Resetowanie bloku
Jeśli Edycja bloku, a następnie chcesz wrócić do oryginalnego zapisano zestaw, kliknij Resetuj.

![Na liście przyciski w górnej części Metryka Explorer](./media/app-insights-metrics-explorer/17-reset.png)

## <a name="live-metrics-stream"></a>Strumień na żywo metryk

Znacznie więcej natychmiastowego widoku telemetrii, otwórz [strumień na żywo](app-insights-live-stream.md). Większość metryki potrwać kilka minut się z powodu proces agregacji. Z kolei metryki na żywo są zoptymalizowane dla małych opóźnieniach. 

## <a name="set-alerts"></a>Ustawianie alertów
Aby otrzymywać powiadomienia pocztą e-mail o nietypowych wartościach dowolnej metryki, należy dodać alert. Można wybrać do wysyłania wiadomości e-mail do administratorów, kont lub na adresy e-mail określone.

![W Eksploratorze metryk Wybierz reguły alertów, Dodaj alertu](./media/app-insights-metrics-explorer/appinsights-413setMetricAlert.png)

[Dowiedz się więcej o alertach][alerts].


## <a name="continuous-export"></a>Ciągły eksport
Jeśli chcesz, aby dane stale wyeksportowane, dzięki czemu użytkownik może przetwarzać zewnętrznie, należy rozważyć użycie [Eksport ciągły](app-insights-export-telemetry.md).

### <a name="power-bi"></a>Power BI
Jeśli chcesz bardziej szczegółowego widoków danych, możesz [eksportowania do usługi Power BI](http://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx).

## <a name="analytics"></a>Analiza
[Analiza](app-insights-analytics.md) bardziej elastyczne sposób analizowania telemetrii przy użyciu języka zaawansowanych zapytań. Użyj go, jeśli chcesz połączyć obliczeniowe wyniki metryki lub wykonaj szczegółowy eksploracji ostatnie wydajności aplikacji. 

Z wykresu metryki można kliknij ikonę Analytics można pobrać bezpośrednio na równoważne zapytania Analytics.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
*Nie widzę żadnych danych na wykresie.*

* Filtry są stosowane do wszystkich schematów w bloku. Upewnij się, że podczas jest koncentrujących się na jeden wykres, nie został ustawiony filtr, który nie obejmuje wszystkie dane na innym.

    Jeśli chcesz ustawić różnych filtrów w różnych wykresów, je utworzyć w różnych bloków, zapisać je jako osobne ulubionych. Jeśli chcesz, można przypiąć je do pulpitu nawigacyjnego, aby były widoczne obok siebie.
* Jeśli grupa wykresu przez właściwość, która nie jest zdefiniowana na Metryka będą nic na wykresie. Spróbuj "Grupuj według", lub wybierz właściwość grupowania.
* Dane dotyczące wydajności (procesora CPU, szybkość We/Wy i tak dalej) jest dostępna dla usług sieci web Java, aplikacji klasycznych systemu Windows, [sieci web IIS na aplikacje i usługi Jeśli Zainstaluj monitor stanu](app-insights-monitor-performance-live-website-now.md), i [usługi w chmurze Azure](app-insights-azure.md). Nie jest dostępna dla witryn sieci Web Azure.

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Następne kroki
* [Monitorowanie użycia za pomocą usługi Application Insights](app-insights-web-track-usage.md)
* [W wyszukiwaniu diagnostycznych](app-insights-diagnostic-search.md)

<!--Link references-->

[alerts]: app-insights-alerts.md
[start]: app-insights-overview.md
[track]: app-insights-api-custom-events-metrics.md
