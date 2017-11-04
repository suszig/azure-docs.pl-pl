---
title: "Diagnozowanie problemów z wydajnością przy użyciu usługi Azure Application Insights | Dokumentacja firmy Microsoft"
description: "Samouczek można znaleźć i diagnozowanie problemów z wydajnością w aplikacji przy użyciu usługi Azure Application Insights."
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 0edec15c7f14ee5338555b03700b7be32c3a1023
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="find-and-diagnose-performance-issues-with-azure-application-insights"></a>Znajdź i diagnozowanie problemów z wydajnością z usługą Azure Application Insights

Azure Application Insights zbiera dane telemetryczne z aplikacji w taki sposób, aby ułatwić analizowanie jego działania i wydajności.  Te informacje służy do identyfikowania problemów, które mogą być wykonywane lub do identyfikowania ulepszenia do aplikacji, która będzie większość użytkowników wpływu.  Ten samouczek przedstawia proces analizowania wydajności składniki serwera aplikacji i perspektywy klienta.  Omawiane kwestie:

> [!div class="checklist"]
> * Zidentyfikuj wydajność operacji po stronie serwera
> * Analiza operacji serwera, aby określić główną przyczynę niską wydajność
> * Zidentyfikuj najwolniejszych operacji po stronie klienta
> * Analizowanie szczegóły wyświetleń strony przy użyciu języka zapytań


## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/downloads/) z następującymi pakietami roboczymi:
    - Tworzenie aplikacji na platformie ASP.NET i aplikacji internetowych
    - Tworzenie aplikacji na platformie Azure
- Wdrażanie aplikacji .NET na platformie Azure i [włączyć zestaw SDK usługi Application Insights](app-insights-asp-net.md).
- [Włącz profilera usługi Application Insights](app-insights-profiler.md#installation) dla aplikacji.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).

## <a name="identify-slow-server-operations"></a>Zidentyfikuj operacji serwera powolne
Usługa Application Insights zbiera szczegóły wydajności dla różnych operacji w aplikacji.  Identyfikując te operacje z najdłuższym okresem można diagnozowania potencjalnych problemów lub najlepiej docelowych z bieżących prac, aby zwiększyć ogólną wydajność aplikacji.

1. Wybierz **usługi Application Insights** , a następnie wybierz subskrypcję.  
1. Aby otworzyć **wydajności** panelu wybierz opcję **wydajności** w obszarze **zbadaj** menu lub kliknij przycisk **czas odpowiedzi serwera** wykresu .

    ![Wydajność](media/app-insights-tutorial-performance/performance.png)

2. **Wydajności** panelu przedstawia średnią i liczba czas trwania każdej operacji dla aplikacji.  Te informacje służą do identyfikacji te operacje większość wpływ na użytkowników. W tym przykładzie **UZYSKAĆ klientów, uzyskać szczegółowe informacje** i **głównej GET/indeksu** prawdopodobnie nadają się do sprawdzania, czy ze względu na ich czas trwania stosunkowo wysokie i liczba wywołań.  Inne operacje może mieć wyższej czas trwania, ale rzadko wywołane, więc wpływu ich poprawy jest niewielka.  

    ![Wydajność w panelu](media/app-insights-tutorial-performance/performance-blade.png)

3. Wykres przedstawia obecnie Średni czas trwania wszystkie operacje w czasie.  Dodaj operacje, które są zainteresowani przez przypinanie je do wykresu.  Oznacza to, czy niektóre pików warto badania.  Izolowanie ją zmniejszając przedział czasu wykresu.

    ![Operacje numeru PIN](media/app-insights-tutorial-performance/pin-operations.png)

4.  Kliknij operację, aby wyświetlić jego wydajności w panelu po prawej stronie. Pokazuje podział czasu trwania dla innego żądania.  Użytkownicy zazwyczaj zauważyć powolne działanie na około pół sekundy, więc zmniejsza okno do żądań za pośrednictwem 500 milisekund.  

    ![Czas trwania dystrybucji](media/app-insights-tutorial-performance/duration-distribution.png)

5.  W tym przykładzie widać, że duża liczba żądań są przejęcia sekundy do przetworzenia. Szczegóły dotyczące tej operacji można wyświetlić, klikając na **szczegóły operacji**.

    ![Szczegóły operacji](media/app-insights-tutorial-performance/operation-details.png)

6.  Informacje, które zostały zebrane wykonanej do tej pory tylko potwierdza, czy jest niska wydajność, ale tak, aby uzyskać dostęp do jego główną przyczynę.  **Profilera** ułatwia to pokazując rzeczywisty kod, który został uruchomiony dla operacji i czas wymagany do każdego kroku. Niektóre operacje nie mogą mieć śledzenia, ponieważ profiler jest uruchamiany okresowo.  Wraz z upływem czasu więcej operacji powinny mieć śladów.  Aby uruchomić profilera dla tej operacji, kliknij przycisk **ślady profilera**.
5.  Śledzenie są wyświetlane poszczególne zdarzenia dla każdej operacji, można zdiagnozować główną przyczynę na czas trwania operacji ogólnej.  Kliknij jeden z najwyższym przykładach, w których ma najdłuższy czasu trwania.
6.  Kliknij przycisk **Pokaż ścieżkę aktywną** aby wyróżnić określonej ścieżki zdarzeń, które najczęściej współtworzyć całkowity czas trwania operacji.  W tym przykładzie widać, że najwolniejszych wywołań pochodzi z *FabrikamFiberAzureStorage.GetStorageTableData* metody. Element, który przyjmuje większość czasu jest *CloudTable.CreateIfNotExist* metody. Jeśli ten wiersz kodu jest wykonywane za każdym razem, gdy funkcja jest wywoływana, zostaną użyte wywołanie niepotrzebnych sieci i zasobów Procesora. Najlepszy sposób, aby naprawić kod jest włączanie dla tego wiersza, niektóre metody uruchamiania, który tylko wykonywane na raz. 

    ![Szczegóły profilera](media/app-insights-tutorial-performance/profiler-details.png)

7.  **Wydajności Porada** w górnej części ekranu obsługuje oceny, jaka jest nadmierne czasu trwania z powodu oczekiwania.  Kliknij przycisk **oczekiwania** łącze dokumentację dotyczącą interpretowanie różne typy zdarzeń.

    ![Porada wydajności](media/app-insights-tutorial-performance/performance-tip.png)

8.  W celu dalszej analizy, możesz kliknąć **Pobierz śledzenia etl** pobrać śledzenia w Visual Studio.

## <a name="use-analytics-data-for-server"></a>Użyj analizy danych dla serwera
Application Insights Analytics zapewnia rozwinięty język zapytań umożliwiający analizowanie wszystkich danych zbieranych przez usługę Application Insights.  Można to wykonać szczegółowa analiza na dane żądanie i wydajności.

1. Wróć do panelu szczegółów operacji, a następnie kliknij przycisk Analytics.

    ![Przycisk Analytics](media/app-insights-tutorial-performance/server-analytics-button.png)

2. Application Insights Analytics otwiera za pomocą kwerendy dla każdego z widoków w panelu.  Można uruchamiać te zapytania, zgodnie z ich lub zmodyfikować je do wymagań.  Pierwsze zapytanie Wyświetla czas trwania tej operacji w czasie.

    ![Analiza](media/app-insights-tutorial-performance/server-analytics.png)


## <a name="identify-slow-client-operations"></a>Zidentyfikuj operacje klienta powolne
Oprócz Identyfikacja procesy serwera w celu optymalizacji, usługi Application Insights można analizować perspektywy przeglądarki klienta.  Może to ułatwić zidentyfikowanie potencjalnych ulepszeń składników klienta, a nawet pokazane problemy z innej przeglądarki lub różnych lokalizacjach.

1. Wybierz **przeglądarki** w obszarze **zbadaj** aby otworzyć przeglądarkę podsumowania.  Zapewnia to podsumowanie visual telemetries różnych aplikacji z perspektywy przeglądarki.

    ![Podsumowanie w przeglądarce](media/app-insights-tutorial-performance/browser-summary.png)

2.  Przewiń w dół do **jakie są moje najwolniejszych stron?**.  Lista stron w aplikacji, który miał najdłużej dla klientów do załadowania.  Priorytety tych stron, które ma największy wpływ na użytkownika, można użyć tych informacji.
3.  Kliknij jeden z strony, aby otworzyć **widok strony** panelu.  W tym przykładzie **/FabrikamProd** strona jest wyświetlana nadmiernego Średni czas trwania.  **Widok strony** panelu szczegółowe informacje na temat tej strony, w tym podział zakresy inny czas trwania.

    ![Widok strony](media/app-insights-tutorial-performance/page-view.png)

4.  Kliknij najwyższy czas trwania, aby sprawdzić szczegóły te żądania.  Następnie kliknij przycisk pojedynczej żądania, aby wyświetlić szczegóły żądania strony, takich jak typ przeglądarki i jego lokalizacji klienta.  Te informacje mogą pomóc w określeniu, czy istnieją problemy wydajności odnoszących się do poszczególnych typów klientów.

    ![Szczegóły żądania](media/app-insights-tutorial-performance/request-details.png)

## <a name="use-analytics-data-for-client"></a>Dane analityczne użycia dla klienta
Jak dane zbierane wydajności serwera usługi Application Insights udostępnia wszystkie dane klienta szczegółowa analiza przy użyciu Analytics.

1. Wróć do przeglądarki, podsumowanie i kliknij ikonę Analytics.

    ![Ikona analityka](media/app-insights-tutorial-performance/client-analytics-icon.png)

2. Application Insights Analytics otwiera za pomocą kwerendy dla każdego z widoków w panelu. Pierwsze zapytanie Wyświetla czas trwania dla widoków innej strony wraz z upływem czasu.

    ![Analiza](media/app-insights-tutorial-performance/client-analytics.png)

3.  Diagnostyka inteligentne jest funkcją analizy Insights aplikacji, która identyfikuje unikatowy wzorce w danych.  Po kliknięciu kropką inteligentne diagnostyki w wykres liniowy tego samego zapytania jest uruchamiany bez rekordy, które spowodowały anomalii.  W sekcji komentarzy zapytania są wyświetlane szczegóły tej dokumentacji, aby można było zidentyfikować właściwości tych widoków strony, które powodują nadmierne czas trwania.

    ![Diagnostyka inteligentne](media/app-insights-tutorial-performance/client-smart-diagnostics.png)


## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już identyfikowanie wyjątki czasu wykonywania, przejdź do następnego samouczkiem, aby dowiedzieć się, jak tworzyć alerty w odpowiedzi na błędy.

> [!div class="nextstepaction"]
> [Alert po wystąpieniu kondycji aplikacji](app-insights-tutorial-alert.md)
