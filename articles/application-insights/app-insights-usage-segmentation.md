---
title: "Analiza użytkowników, sesji i zdarzeń w usłudze Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Analiza demograficznych użytkowników aplikacji sieci web."
services: application-insights
documentationcenter: 
author: botatoes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 05/03/2017
ms.author: bwren
ms.openlocfilehash: b154a01d1690bff4950ebc1ff5a5b89894d4d111
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="users-sessions-and-events-analysis-in-application-insights"></a>Analiza użytkownikami, sesjami i zdarzeń w usłudze Application Insights

Dowiedz się, gdy użytkownicy korzystają z aplikacji sieci web, jakie stron one jest najbardziej zainteresowani, gdzie znajdują się użytkownicy, jakie przeglądarek i systemów operacyjnych korzystają. Analizowanie telemetrii działalności biznesowej i użycia za pomocą [Azure Application Insights](app-insights-overview.md).

## <a name="get-started"></a>Rozpoczęcie pracy

Jeśli nie widzisz jeszcze dane użytkowników, sesji lub bloków zdarzenia w portalu usługi Application Insights [Dowiedz się, jak rozpocząć pracę z narzędziami użycia](app-insights-usage-overview.md).

## <a name="the-users-sessions-and-events-segmentation-tool"></a>Narzędzie segmentacji użytkowników, sesji i zdarzenia

Trzy bloków użycia umożliwia tego samego narzędzia selekcji i dane telemetryczne z aplikacji sieci web z trzema perspektyw. Filtrowanie i dzielenia danych, można ujawnić informacjami na temat względną użycie różnych stron i funkcji.

* **Narzędzie Użytkownicy**: używane wiele aplikacji i ich funkcje.  Użytkownicy są liczone przy użyciu anonimowego identyfikatorów przechowywane w plikach cookie przeglądarki. Jedna osoba, za pomocą różnych przeglądarkach lub maszyny będą liczone jako więcej niż jednego użytkownika.
* **Narzędzie sesje**: ile sesji aktywności użytkowników, zostały uwzględnione w pewnych stron i funkcji aplikacji. Sesja jest traktowane po pół godziny czas braku aktywności użytkownika, lub 24 godziny ciągłego użytkowania.
* **Narzędzie zdarzenia**: jak często używane pewnych stron i funkcji aplikacji. Widok strony jest liczony przeglądarką załadowanie strony z aplikacji, pod warunkiem, że [zinstrumentowane on](app-insights-javascript.md). 

    Zdarzenie niestandardowe reprezentuje jedno wystąpienie określonego zdarzenia w aplikacji, często interakcji użytkownika, np. Kliknij przycisk lub zakończenie niektórych zadań. Wstaw kod aplikacji w celu [generowanie zdarzeń niestandardowych](app-insights-api-custom-events-metrics.md#trackevent).

![Użycie narzędzia](./media/app-insights-usage-segmentation/users.png)

## <a name="querying-for-certain-users"></a>Wykonywanie zapytania dla niektórych użytkowników 

Zapoznaj się z różnych grup użytkowników, zmieniając opcje zapytania w górnej części narzędzia użytkowników: 

* Kto używane: Wybierz niestandardowych zdarzeń i wyświetlenia strony. 
* W trakcie: Wybierz zakres czasu. 
* Przez: Wybierz sposób pojemnik danych przez pewien czas lub innej właściwości, takie jak przeglądarki lub Miasto. 
* Dzielenie przez: Wybierz właściwość za pomocą którego podziału lub segmentu danych. 
* Dodaj filtry: Ograniczyć zapytanie do niektórych użytkowników, sesji lub zdarzenia na podstawie ich właściwości, takie jak przeglądarki lub Miasto. 
 
## <a name="saving-and-sharing-reports"></a>Zapisywanie i udostępnianie raportów 
Użytkownicy raportów, prywatne tylko dla Ciebie w sekcji Moje raporty lub udostępnionego można zapisać z inne osoby z dostępem do tego zasobu usługi Application Insights w sekcji udostępnionych raportów.  
 
Podczas zapisywania raportu lub edytowania jego właściwości, wybierz polecenie "Bieżący względny zakres czasu" Aby zapisać raport będzie stale odświeżyć dane, wracając niektórych stałej ilości czasu.  
 
Wybierz opcję "Bieżący bezwzględny zakres czasu" Aby zapisać raport ustalonego zestawu danych. Należy pamiętać, że dane w usłudze Application Insights tylko są przechowywane przez 90 dni, więc jeśli minęło ponad 90 dni od raport z zakresem bezwzględnego czasu został zapisany, raport zostanie wyświetlona pusta. 
 
## <a name="example-instances"></a>Przykład wystąpień

Sekcja wystąpień przykład zawiera informacje o kilka poszczególnych użytkowników, sesji lub zdarzeń, które są dopasowane wg bieżącego zapytania. Biorąc pod uwagę i eksploracji zachowania jednostek, oprócz agregacje, zapewniają wgląd w sposób osoby faktycznie używają Twojej aplikacji. 
 
## <a name="insights"></a>Insights 

Pasek boczny szczegółowe informacje zawiera dużych klastrach użytkowników, które mają wspólne właściwości. Tych klastrów można ujawnić zaskakująco trendy w sposób użytkownicy korzystają z aplikacji. Na przykład, jeśli 40% wszystkich użycia aplikacji pochodzi z osoby za pomocą jednej funkcji.  


## <a name="next-steps"></a>Następne kroki
- Aby umożliwić korzystanie z użycia, Rozpocznij wysyłanie [zdarzeń niestandardowych](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) lub [wyświetlenia strony](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Jeżeli już zdarzeń niestandardowych lub wyświetleń strony, Poznaj narzędzia użycia, aby dowiedzieć się, jak używać usługi przez użytkowników.
    - [Lejki](usage-funnels.md)
    - [Przechowywanie](app-insights-usage-retention.md)
    - [User Flows (Przepływy użytkowników)](app-insights-usage-flows.md)
    - [Skoroszyty](app-insights-usage-workbooks.md)
    - [Dodaj kontekstu użytkownika](app-insights-usage-send-user-context.md)

