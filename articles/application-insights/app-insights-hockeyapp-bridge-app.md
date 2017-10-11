---
title: "Eksplorowanie danych HockeyApp w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Analizowanie użycia i wydajności aplikacji platformy Azure za pomocą usługi Application Insights."
services: application-insights
documentationcenter: windows
author: CFreemanwa
manager: carmonm
ms.assetid: 97783cc6-67d6-465f-9926-cb9821f4176e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: bwren
ms.openlocfilehash: 450ca10613d137393090578619f3766734d1d493
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="exploring-hockeyapp-data-in-application-insights"></a>Eksplorowanie danych HockeyApp w usłudze Application Insights
[HockeyApp](https://azure.microsoft.com/services/hockeyapp/) jest zalecane platformą monitorowania na żywo wersje desktop i mobile apps. Z HockeyApp można wysyłać niestandardowych i śledzenia danych telemetrycznych do monitorowania użycia i ułatwić diagnozowanie (oprócz pobierania danych awarii). Ten strumień danych telemetrycznych można zbadać w zaawansowanym [Analytics](app-insights-analytics.md) funkcji [Azure Application Insights](app-insights-overview.md). Ponadto można [eksportowanie niestandardowego i dane telemetryczne śledzenia](app-insights-export-telemetry.md). Aby włączyć te funkcje, należy skonfigurować mostek przekazuje HockeyApp niestandardowe dane do usługi Application Insights.

## <a name="the-hockeyapp-bridge-app"></a>Aplikacji HockeyApp Mostek
Aplikacja Mostek HockeyApp jest funkcja core, która umożliwia dostęp do aplikacji HockeyApp niestandardowych i dane telemetryczne śledzenia w usłudze Application Insights przy użyciu funkcji analizy i eksportu ciągłego. Niestandardowy i śledzenia zebranych zdarzeń wg HockeyApp po utworzeniu aplikacji HockeyApp mostek będzie dostępny z tych funkcji. Zobaczmy, jak ustawić jedną z tych aplikacji mostek.

Otwórz ustawienia konta aplikacji HockeyApp, [tokeny interfejsu API](https://rink.hockeyapp.net/manage/auth_tokens). Utwórz nowy token, albo ponownie użyć już istniejącego. Minimalne uprawnienia wymagane są tylko do odczytu"". Kopiowanie interfejsu API zająć tokenu.

![Pobierz token HockeyApp API](./media/app-insights-hockeyapp-bridge-app/01.png)

Otwórz portal Microsoft Azure i [utworzyć zasobu usługi Application Insights](app-insights-create-new-resource.md). Ustaw typ aplikacji "Platforma HockeyApp Mostek aplikacji":

![Nowy zasób usługi Application Insights](./media/app-insights-hockeyapp-bridge-app/02.png)

Nie należy ustawić nazwę — spowoduje to ustawienie automatycznie na podstawie nazwy aplikacji HockeyApp.

Pola Mostek HockeyApp są wyświetlane. 

![Wprowadź pola Mostek](./media/app-insights-hockeyapp-bridge-app/03.png)

Wprowadź token HockeyApp zanotowaną wcześniej. Ta akcja powoduje wypełnienie menu rozwijanym "Aplikacji HockeyApp" z wszystkich aplikacji HockeyApp. Wybierz ten, który ma być używany, a następnie ukończ pozostałe pola. 

Otwórz nowy zasób. 

Należy pamiętać, że dane zajmuje trochę czasu, aby uruchomić przepływy.

![Oczekiwanie na dane zasobu usługi Application Insights](./media/app-insights-hockeyapp-bridge-app/04.png)

Gotowe. Niestandardowy i śledzenia zbieranych danych w aplikacji zinstrumentowane HockeyApp od tego momentu teraz jest również dostępny w funkcji analizy i eksportu ciągłego usługi Application Insights.

Załóżmy krótko przejrzyj każdą z tych funkcji, które są teraz dostępne dla Ciebie.

## <a name="analytics"></a>Analiza
Analytics to narzędzie zaawansowanych zapytań ad hoc danych, co umożliwia diagnozowanie i analizowania telemetrii i szybko odnaleźć główne przyczyny i wzorce.

![Analiza](./media/app-insights-hockeyapp-bridge-app/05.png)

* [Dowiedz się więcej o analityka](app-insights-analytics-tour.md)

## <a name="continuous-export"></a>Eksport ciągły
Eksport ciągły pozwala na wyeksportowanie danych do kontenera magazynu obiektów Blob Azure. Jest to bardzo przydatne, jeśli chcesz zachować dane przez czas dłuższy niż okres przechowywania obecnie oferowanych przez usługi Application Insights. Można przechowywać dane w magazynie obiektów blob, przetworzyć go do bazy danych SQL lub preferowany rozwiązań magazynowania danych.

[Dowiedz się więcej o eksportu ciągłego](app-insights-export-telemetry.md)

## <a name="next-steps"></a>Następne kroki
* [Stosowanie Analytics do danych](app-insights-analytics-tour.md)

