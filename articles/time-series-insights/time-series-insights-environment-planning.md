---
title: "Planowanie skali środowiska Azure czas serii Insights | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób planowania środowiska Azure Insights serii czas, tym pojemności, przechowywanie danych, ruch przychodzący wydajność i monitorowania, należy stosować najlepsze rozwiązania."
services: time-series-insights
ms.service: time-series-insights
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: MicrosoftDocs/tsidocs
ms.reviewer: v-mamcge, jasonh, kfile, anshan
ms.devlang: csharp
ms.workload: big-data
ms.topic: article
ms.date: 11/15/2017
ms.openlocfilehash: 5fb158ba162dd199f419f9568de08a7a18c833dd
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="plan-your-azure-time-series-insights-environment"></a>Planowanie środowiska Azure czas serii Insights

W tym artykule opisano sposób planowania środowiska Azure czas serii Insights na podstawie szybkość oczekiwanego wejściowych i wymagań dotyczących przechowywania danych.

## <a name="best-practices"></a>Najlepsze praktyki

Aby rozpocząć pracę z informacjami dotyczącymi czasu serii, najlepiej Jeśli wiadomo, ile danych spodziewasz się do dystrybuowania za minutę również, jak długo ma być do przechowywania danych.  

Aby uzyskać więcej informacji o pojemności i przechowywania dla obu SKU czasu serii szczegółowe informacje, zobacz [Insights serii czasu cennik](https://azure.microsoft.com/pricing/details/time-series-insights/).

Weź pod uwagę następujące atrybuty do planu najlepsze środowisko dla długoterminowym sukcesie: 
- Pojemność magazynu
- Okres przechowywania danych
- Ruch przychodzący wydajność 

## <a name="understand-storage-capacity"></a>Zrozumienie pojemność magazynu
Domyślnie czas serii Insights zachowuje danych, w oparciu o ilość miejsca w magazynie, które zostały udostępnione (jednostek raza ilość pamięci masowej na jednostkę) i transfer danych przychodzących.

## <a name="understand-data-retention"></a>Zrozumienie przechowywania danych
Można skonfigurować środowisko czasu serii Insights **czas przechowywania danych** ustawienie umożliwiające maksymalnie 400 dni przechowywania.  Czas serii wgląd w dwóch trybach, który optymalizuje zapewniających środowiska zawiera najbardziej aktualne dane (na domyślnie), a innego, który jest zoptymalizowany do zapewnienia przechowywania limitów, gdy transfer danych przychodzących jest wstrzymana, jeśli ogólną pojemność środowisko zostaje trafiony.  Można dostosować, przechowywania i przełączania między dwa tryby na stronie konfiguracji w środowisku, w portalu Azure.

Można skonfigurować maksymalnie 400 dni przechowywania danych w środowisku Insights serii czasu.

## <a name="configure-data-retention"></a>Konfigurowanie przechowywania danych

1. W [portalu Azure](https://portal.azure.com), wybierz środowisko Insights serii czasu.

2. Na **strony środowiska czasu serii Insights**w obszarze **ustawienia** nagłówek, wybierz **Konfiguruj**. 

3. W **czas przechowywania danych (w dniach)** wprowadź wartość z zakresu od 1 do 400.

   ![Konfigurowanie przechowywania](media/environment-mitigate-latency/configure-retention.png)

## <a name="understand-ingress-capacity"></a>Zrozumienie ruch przychodzący wydajność

Obszar innych skupić się na potrzeby planowania jest pojemność ruch przychodzący, która jest pochodną alokacji na minutę. 

Z punktu widzenia ograniczania przepustowości ingressed danych o rozmiarze pakiecie 32 KB jest traktowany jako 32 zdarzenia, każdy o rozmiarze 1 KB. Maksymalnego dozwolonego rozmiaru zdarzenia wynosi 32 KB; pakiety danych jest większy niż 32 KB są obcinane.

Poniższa tabela zawiera podsumowanie pojemności wejściowych dla każdej jednostki SKU:

|SKU  |Liczba zdarzeń na miesiąc na jednostkę  |Zdarzenia rozmiar na miesiąc na jednostkę  |Liczba zdarzeń na minutę na jednostkę  | Rozmiar na minutę na jednostkę   |
|---------|---------|---------|---------|---------|
|S1     |   30 milionów     |  30 GB     |  700    |  700 KB   |
|S2     |   300 milionów    |   300 GB   | 7,000   | 7000 KB  |

Można zwiększyć pojemność S1 lub S2 SKU do 10 jednostek w jednym środowisku. Nie można migrować z środowisku S1 S2 lub środowiska S2 S1. 

Ruch przychodzący pojemności najpierw należy określić całkowita wymagane na podstawie-miesięczny transfer danych przychodzących. Następnie należy określić, jakie użytkownika na minutę wymaga są, jak jest to, gdzie ograniczania przepustowości i opóźnień pełnić rolę.

Jeśli kolekcji znajdują się w sieci przychodzący danych trwających mniej niż 24 godziny, czasu serii wgląd można "przechwycenie" szybkością wejściowych 2 x stawki wymienionych powyżej. 

Na przykład jeśli masz jednej jednostki SKU S1 i danych wejściowych z szybkością 700 zdarzenia na minutę, kolekcji poniżej 1 godziny szybkością 1400 zdarzeń lub mniej, nie byłoby bez zauważalnego opóźnienia dla danego środowiska. Jednak po przekroczeniu 1400 zdarzenia na minutę przez więcej niż jedną godzinę, będzie prawdopodobnie mają opóźnienia do danych, które wizualizowanego i jest dostępny dla zapytań w danym środowisku. 

Może nie wiedzieć z wyprzedzeniem ilość danych można było się spodziewać push. W takim przypadku można znaleźć dane telemetryczne dla [Centrum IoT Azure](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics) i [Azure Event Hubs](https://blogs.msdn.microsoft.com/cloud_solution_architect/2016/05/25/using-the-azure-rest-apis-to-retrieve-event-hub-metrics/) w portalu Azure. Ten telemetrii ułatwia określenie sposobu udostępnienia środowiska. Użyj **metryki** strony w portalu Azure źródła odpowiednie zdarzenie wyświetlić jego telemetrii. Jeśli znasz Twoje metryki źródła zdarzeń można efektywniej planowanie i udostępnić środowiska Insights serii czasu.

## <a name="calculate-ingress-requirements"></a>Oblicz wymagania wejściowych

- Upewnij się, czy pojemność transfer danych przychodzących jest powyżej średnia szybkość na minutę oraz czy środowisko jest wystarczająco duża do obsługi sieci przychodzący przewidywanego odpowiednikiem 2 x możliwości poniżej 1 godziny.

- Po wystąpieniu nagłego transfer danych przychodzących który ostatnio przez czas dłuższy niż 1 godzina; Użyj szybkość, z kolekcji jako Twoja średnia i środowisko o pojemności do obsługi kolekcji częstotliwość udostępnić.
 
## <a name="mitigate-throttling-and-latency"></a>Ograniczenia przepustowości i opóźnień

Aby dowiedzieć się, jak zapobiec ograniczania przepustowości i opóźnień, zobacz [zmniejszyć opóźnienia i ograniczania przepustowości](time-series-insights-environment-mitigate-latency.md). 

## <a name="next-steps"></a>Następne kroki
- [Jak dodać źródła zdarzenia Centrum zdarzeń](time-series-insights-how-to-add-an-event-source-eventhub.md)
- [Jak dodać źródła zdarzenia Centrum IoT](time-series-insights-how-to-add-an-event-source-iothub.md)
