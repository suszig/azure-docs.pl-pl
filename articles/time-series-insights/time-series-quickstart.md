---
title: Szybki Start - Eksplorator Azure Insights serii czasu | Dokumentacja firmy Microsoft
description: "Ta opcja szybkiego startu pokazano, jak rozpocząć pracę z Eksploratora Azure czas serii wgląd w przeglądarce sieci web do wizualizacji dużych ilości danych IoT. Samouczek najważniejsze funkcje w środowisku demonstracyjnej."
services: time-series-insights
ms.service: time-series-insights
author: MarkMcGeeAtAquent
ms.author: v-mamcge
manager: jhubbard
editor: MarkMcGeeAtAquent, jasonwhowell, kfile, MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.topic: quickstart
ms.workload: big-data
ms.custom: mvc
ms.date: 11/15/2017
ms.openlocfilehash: b1f2881da21849c3ac09b008640fc9f72dc158dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="quickstart-explore-azure-time-series-insights"></a>Szybki Start: Poznaj Insights serii czasu Azure
Tego przewodnika Szybki Start pokazano, jak rozpocząć pracę z Eksploratora Azure czas serii wgląd w środowisku pokaz wolne. Możesz dowiedzieć się, jak można używać przeglądarki sieci web wizualizacji dużych ilości danych IoT i samouczek kluczowych funkcji usługi Eksplorator Insights serii czasu. 

Azure Time Series Insights to w pełni zarządzana usługa związana z analizą, przechowywaniem i wizualizacją, dzięki której z łatwością można badać i analizować w tym samym momencie miliardy zdarzeń IoT. Umożliwia globalny widoku danych, co pozwala szybko sprawdzić rozwiązania IoT i uniknąć kosztownych przestojów urządzeniom krytycznym, pomaga odnajdywanie ukryte trendów wykrywania anomalii i przeprowadzanie analizy głównej przyczyny problemu w najbliższym czasie rzeczywistym.  Jeśli tworzysz aplikację, która wymaga danych serii godzina magazynu lub zapytania można tworzyć przy użyciu interfejsów API REST czasu serii szczegółowych informacji.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

## <a name="explore-time-series-insights-explorer-in-a-demo-environment"></a>Poznaj Eksploratora czasu serii wgląd w środowisku pokaz

1. W przeglądarce przejdź do [https://insights.timeseries.azure.com/demo](https://insights.timeseries.azure.com/demo). 

2. W przypadku wyświetlenia monitu zaloguj się do Eksploratora Insights serii czasu przy użyciu poświadczeń konta platformy Azure. 
 
3. Zostanie wyświetlona strona szybki przegląd Insights serii czasu. Kliknij przycisk **dalej** zacząć krótki przewodnik.

   ![Kliknij przycisk Dalej](media/quickstart/quickstart1.png)

4. **Panelu wyboru czas** jest wyświetlany. Wybierz przedział czasu do wizualizacji za pomocą tego panelu.

   ![Panel wyboru czasu](media/quickstart/quickstart2.png)

5. Kliknij i przeciągnij w regionie, a następnie kliknij przycisk **wyszukiwania** przycisku.
 
   ![Wybierz przedział czasu](media/quickstart/quickstart3.png) 

   Czas serii Insights Wyświetla wizualizację wykresu dla określony przedział czasu. Można wykonywać różne akcje w ramach z wykres liniowy, takich jak filtrowanie, przypinanie, sortowania i stosu. 

   Aby powrócić do **panelu wyboru czas**, kliknij strzałkę w dół, jak pokazano:

   ![Wykres](media/quickstart/quickstart4.png)

6. Kliknij przycisk **Dodaj** w **panelu warunki** można dodać nowego terminu wyszukiwania.

   ![Dodaj element](media/quickstart/quickstart5.png)

7. Na wykresie, można wybrać region, kliknij prawym przyciskiem myszy obszar i wybierz **Eksploruj zdarzenia**.
 
   ![Eksploruj zdarzenia](media/quickstart/quickstart6.png)

   Siatka danych pierwotnych jest wyświetlana w regionie, w którym są eksploracji:

   ![Widok siatki](media/quickstart/quickstart7.png)

8. Edytowanie warunków w celu zmiany wartości na wykresie, a następnie dodaj inny termin do cross korelując różnych typów wartości:

   ![Dodaj warunek](media/quickstart/quickstart8.png)

9. Wprowadź warunku filtru w **filtrować serii...**  pole do filtrowania serii ad hoc. W przypadku szybkiego startu, wprowadź **Station5** do korelując między temperatury i wykorzystania dla tej stacji.
 
   ![Filtr serii](media/quickstart/quickstart9.png)

Po zakończeniu procedury szybkiego startu, możesz eksperymentować z zestawem danych przykładowych, aby utworzyć różne wizualizacje. 

### <a name="next-steps"></a>Następne kroki
Wszystko jest gotowe do tworzenia własnego środowiska Insights serii czasu:
> [!div class="nextstepaction"]
> [Planowanie środowiska Insights serii czasu](time-series-insights-environment-planning.md)
