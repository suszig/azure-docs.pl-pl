---
title: "Mapowanie aplikacji w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Wizualną prezentację zależności między składnikami aplikacji etykietą kluczowych wskaźników wydajności i alerty."
services: application-insights
documentationcenter: 
author: SoubhagyaDash
manager: carmonm
ms.assetid: 3bf37fe9-70d7-4229-98d6-4f624d256c36
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mbullwin
ms.openlocfilehash: e1eb2177d6032142781e6e31af6c7f6313d38f4d
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="application-map-in-application-insights"></a>Mapowanie aplikacji w usłudze Application Insights
W [Azure Application Insights](app-insights-overview.md), mapa aplikacji jest układu wizualnego relacji zależności składników aplikacji. Każdego składnika zawiera kluczowych wskaźników wydajności, takich jak ładowania, wydajności błędów i alerty, ułatwiające ustalenie dowolny składnik przyczyną problemu z wydajnością lub błędu. Możesz kliknąć za pośrednictwem z każdego składnika, do bardziej szczegółowych diagnostyki, takich jak zdarzenia usługi Application Insights. Jeśli aplikacja korzysta z usług Azure, możesz również kliknąć za pośrednictwem do diagnostyki Azure, takich jak zalecenia doradcy bazy danych SQL.

Podobnie jak inne wykresy można przypiąć mapę aplikacji do pulpitu nawigacyjnego platformy Azure, gdzie jest pełną funkcjonalność. 

## <a name="open-the-application-map"></a>Otwórz mapę aplikacji
Otwórz mapę z bloku Omówienie aplikacji:

![Otwórz aplikację mapy](./media/app-insights-app-map/01.png)

![Mapa aplikacji](./media/app-insights-app-map/02.png)

Pokazuje mapy:

* Testy dostępności
* Składnik po stronie klienta (monitorowane przy użyciu zestawu SDK JavaScript)
* Składnik po stronie serwera
* Zależności składniki klienta i serwera

Można zwijać i rozwijać grupy łącze zależności:

![Zwiń](./media/app-insights-app-map/03.png)

Jeśli masz wiele zależności jednego typu (SQL, HTTP itp.) są zgrupowane. 

![zależności grupowanych](./media/app-insights-app-map/03-2.png)

## <a name="spot-problems"></a>Dodatkowych problemów
Każdy węzeł ma odpowiednie wskaźniki, jak szybkości ładowania, wydajności i błąd dla tego składnika. 

Ikonami ostrzeżenia zaznacz możliwych problemów. Pomarańczowe ostrzeżenie oznacza, że błędy występują w żądaniach, wyświetleń strony lub wywołania zależności. Czerwony oznacza współczynnik awaryjności powyżej 5%. Jeśli chcesz dostosować tych progów, Otwórz opcje.

![ikony błędu](./media/app-insights-app-map/04.png)

Aktywne alerty również Pokaż zapasową: 

![aktywne alerty](./media/app-insights-app-map/05.png)

Jeśli używasz usług SQL Azure jest ikonę, która pokazuje, kiedy są zalecenia, w jaki sposób można poprawić wydajność. 

![zalecenie Azure](./media/app-insights-app-map/06.png)

Kliknij dowolną ikonę, aby uzyskać więcej szczegółów:

![zalecenie Azure](./media/app-insights-app-map/07.png)

## <a name="diagnostic-click-through"></a>Kliknij diagnostyczne za pośrednictwem
Każdego z węzłów na mapie oferuje docelowe kliknij za pomocą diagnostyki. Opcje się różnić w zależności od typu węzła.

![Opcje serwera](./media/app-insights-app-map/09.png)

Dla składników, które są hostowane na platformie Azure obejmują bezpośrednich łączy do nich.

## <a name="filters-and-time-range"></a>Filtry i zakres czasu
Domyślnie mapy znajduje się podsumowanie wszystkich danych dostępna dla wybranego zakresu czasu. Ale odfiltrowaniu może zawierać tylko nazwy operacji lub zależności.

* Nazwa operacji: dotyczy zarówno wyświetleń strony i typy żądania po stronie serwera. Po wybraniu tej opcji mapy zawiera wskaźnik KPI na węźle serwera/klienta tylko wybranej operacji. Przedstawia on wywoływany w kontekście tych określonych operacji zależności.
* Nazwa podstawowa zależności: dotyczy to również zależności przeglądarki AJAX i zależności po stronie serwera. Jeśli raport dane telemetryczne zależności niestandardowych przy użyciu interfejsu API TrackDependency pojawią się również w tym miejscu. Możesz wybrać zależności do wyświetlenia na mapie. Obecnie to pole wyboru nie filtruje żądania po stronie serwera lub wyświetleń strony po stronie klienta.

![Ustaw filtry](./media/app-insights-app-map/11.png)

## <a name="save-filters"></a>Zapisz filtry
Aby zapisać filtry zostały zastosowane, przypiąć widok filtrowany na [pulpitu nawigacyjnego](app-insights-dashboards.md).

![Przypnij do pulpitu nawigacyjnego](./media/app-insights-app-map/12.png)

## <a name="error-pane"></a>Okienko błędu
Po kliknięciu węzła w mapie okienko błędu jest wyświetlana po prawej stronie podsumowania błędów dla tego węzła. Awarie są najpierw pogrupowane według Identyfikatora operacji i pogrupowane według identyfikatora problemu.

![Okienko błędu](./media/app-insights-app-map/error-pane.png)

Kliknięcie awarii przejście do ostatniej wystąpienia tego błędu.

## <a name="resource-health"></a>Kondycja zasobów
W przypadku niektórych typów zasobów kondycja zasobu jest wyświetlany w górnej części okienka błędu. Na przykład kliknięcie węzła SQL spowoduje wyświetlenie kondycji bazy danych i alerty, które mają być uruchamiane.

![Kondycja zasobów](./media/app-insights-app-map/resource-health.png)

Można kliknąć nazwę zasobu, aby wyświetlić przegląd standardowe metryki dla tego zasobu.

## <a name="end-to-end-system-app-maps"></a>System end-to-end aplikacji mapy

*Wymaga zestawu SDK w wersji 2.3 lub nowszej*

Jeśli aplikacja ma kilka składników — na przykład usługi zaplecza dodatkowo do aplikacji sieci web -, można wyświetlić wszystkich na mapie jednej zintegrowanej aplikacji.

![Ustaw filtry](./media/app-insights-app-map/multi-component-app-map.png)

Mapa aplikacji znajduje węzły serwera, wykonując wszystkie wywołania zależności HTTP między serwerami z zestawem SDK Insights aplikacji, które zostały zainstalowane. Przyjęto, że każdy zasób usługi Application Insights zawiera jeden serwer.

### <a name="multi-role-app-map-preview"></a>Mapa aplikacji usługi roli (wersja zapoznawcza)

Funkcja mapy usługi roli aplikacji w wersji zapoznawczej umożliwiają mapy aplikacji z wieloma serwerami wysyłanie danych do tego samego zasobu usługi Application Insights / klucz instrumentacji. Serwery w mapie są podzielone przez właściwość cloud_RoleName elementów telemetrii. Ustaw *Mapa aplikacji usługi roli* do *na* z bloku Podgląd, aby włączyć tę konfigurację.

Ta metoda może być wskazane w aplikacji micro-services lub w innych sytuacjach, w którym ma zostać korelowanie zdarzeń na wielu serwerach w ramach pojedynczego zasobu usługi Application Insights.

## <a name="video"></a>Połączenia wideo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player] 

## <a name="feedback"></a>Opinia
Prześlij opinię za pośrednictwem portalu opinie.

![Obraz MapLink-1](./media/app-insights-app-map/13.png)


## <a name="next-steps"></a>Następne kroki

* [Witryna Azure Portal](https://portal.azure.com)
