---
title: "Diagnozowanie i rozwiązywanie problemów | Dokumentacja firmy Microsoft"
description: "W tym samouczku opisano sposób diagnozowania i rozwiązywania problemów w środowisku Insights serii czasu"
keywords: 
services: time-series-insights
documentationcenter: 
author: venkatgct
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/24/2017
ms.author: venkatja
ms.openlocfilehash: 4e10a009eb67706d927ece5692134d802094cdf9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="diagnose-and-solve-problems-in-your-time-series-insights-environment"></a>Diagnozowanie i rozwiązywanie problemów w środowisku Insights serii czasu

## <a name="i-dont-see-my-data"></a>Moje dane nie są widoczne
Poniżej przedstawiono przyczyny, dlaczego może nie być wyświetlana danych w środowisku w [portal Azure czas serii Insights](https://insights.timeseries.azure.com).

### <a name="your-event-source-doesnt-have-data-in-json-format"></a>Źródła zdarzeń nie zawiera danych w formacie JSON
Azure czas serii Insights obsługuje obecnie tylko dane JSON. Przykłady JSON dla [kształtów JSON obsługiwany](time-series-insights-send-events.md#supported-json-shapes).

### <a name="when-you-registered-your-event-source-you-didnt-provide-the-key-that-has-the-required-permission"></a>Po zarejestrowaniu źródła zdarzeń nie dostarczył klucz, który ma wymagane uprawnienia
* Centrum IoT należy podać klucz, który ma **usługa połączyć** uprawnienia.

   ![Uprawnienia do połączenia z usługi Centrum IoT](media/diagnose-and-solve-problems/iothub-serviceconnect-permissions.png)

   Jak pokazano na poprzedniej ilustracji, albo zasad **iothubowner** i **usługi** będzie działać, ponieważ mają **usługa połączyć** uprawnienia.
* Do Centrum zdarzeń, musisz podać klucz, który ma **nasłuchiwania** uprawnienia.

   ![Uprawnienie nasłuchiwania Centrum zdarzeń](media/diagnose-and-solve-problems/eventhub-listen-permissions.png)

   Jak pokazano na poprzedniej ilustracji, albo zasad **odczytu** i **zarządzanie** będzie działać, ponieważ mają **nasłuchiwania** uprawnienia.

### <a name="the-provided-consumer-group-is-not-exclusive-to-time-series-insights"></a>Grupy odbiorców podana nie jest zarezerwowana do szczegółowych danych serii czasu
Centrum IoT i Centrum zdarzeń podczas rejestracji wymagane do określenia grupy odbiorców, które mają być używane do odczytywania danych. Ta grupa użytkowników nie muszą zostać udostępnione. Jeśli jest on udostępniony podstawowej Centrum zdarzeń automatycznie rozłącza jeden czytników losowo.

## <a name="i-see-my-data-but-theres-a-lag"></a>Dane są widoczne, ale istnieje opóźnienie
Poniżej przedstawiono przyczyny, dlaczego napotkać częściowe dane w danym środowisku, w [portalu czasu serii Insights](https://insights.timeseries.azure.com).

### <a name="your-environment-is-getting-throttled"></a>Środowisko jest wprowadzenie ograniczany.
Limit ograniczania są wymuszane na podstawie typu jednostki SKU w środowisku i pojemności. Wszystkie źródła zdarzeń w środowisku udostępnianie tej pojemności. Jeśli źródło zdarzenia z Centrum IoT i Centrum zdarzeń jest przekazywanie danych poza granicami wymuszone, zobacz ograniczania przepustowości i opóźnienia.

Na poniższym diagramie przedstawiono środowisku czasu serii Insights SKU S1 i pojemności 3. Może on ruch przychodzący 3 miliony zdarzeń na dzień.

![Obecna pojemność jednostki SKU środowiska](media/diagnose-and-solve-problems/environment-sku-current-capacity.png)

Załóżmy, że to środowisko jest wprowadzania komunikaty z Centrum zdarzeń z prędkości wejściowej pokazano na poniższym diagramie:

![Transfer danych przychodzących przykład szybkość Centrum zdarzeń](media/diagnose-and-solve-problems/eventhub-ingress-rate.png)

Jak pokazano na diagramie, codzienne prędkości wejściowej jest ~ 67,000 wiadomości. Ta stawka tłumaczy około 46 wiadomości co minutę. Jeśli każdy komunikat Centrum zdarzeń jest jako spłaszczone do jednego zdarzenia Insights serii czasu, to środowisko widzi nie ograniczania. Jeśli każdy komunikat Centrum zdarzeń jest jako spłaszczone do 100 zdarzeń Insights serii czasu, następnie 4,600 zdarzenia powinny być pozyskanych co minutę. Środowisku S1 SKU o pojemności 3 można tylko 2100 zdarzeń wejściowych co minutę (1 miliona zdarzeń na dzień = 700 zdarzenia na minutę na poziomie jednostki 3 = 2100 zdarzenia na minutę). W związku z tym zostanie wyświetlony opóźnienia z powodu ograniczania. 

Ogólny zrozumieć sposób działania spłaszczania logiki, zobacz [kształtów JSON obsługiwany](time-series-insights-send-events.md#supported-json-shapes).

#### <a name="recommended-steps"></a>Zalecane czynności
Aby usunąć opóźnienie, zwiększyć pojemność jednostki SKU środowiska. Aby uzyskać więcej informacji, zobacz [sposób skalowania środowiska czasu serii Insights](time-series-insights-how-to-scale-your-environment.md).

### <a name="youre-pushing-historical-data-and-causing-slow-ingress"></a>Wypychanie danych historycznych i powodujące powolne wejściowych
Jeśli łączysz istniejące źródło zdarzenia jest prawdopodobne, że z Centrum IoT i Centrum zdarzeń zawiera już dane w nim. Dlatego środowiska uruchamia ściąganie danych z początku okres przechowywania komunikatów źródło zdarzenia. 

To zachowanie jest zachowaniem domyślnym i nie może zostać zastąpiona. Można Uwzględnij, ograniczania i może upłynąć trochę czasu, aby odnaleźć wprowadzania danych historycznych.

#### <a name="recommended-steps"></a>Zalecane czynności
Aby usunąć opóźnienie, należy wykonać następujące czynności:
1. Zwiększ pojemność jednostki SKU na maksymalną dozwoloną wartość (10, w tym przypadku). Po zwiększeniu możliwości uruchamiania procesu wejściowych Przechwytywanie się znacznie szybciej. Można zwizualizować, jak szybko jest przechwytywanie za pomocą wykresu dostępności w [portalu czasu serii Insights](https://insights.timeseries.azure.com). Naliczane są opłaty za zwiększenia pojemności.
2. Po zawiera opóźnienie zmniejszyć pojemność jednostki SKU do szybkość normalne wejściowych.

## <a name="my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Źródła zdarzeń *nazwa właściwości sygnatury czasowej* ustawienie nie działa
Upewnij się, że nazw i wartości są zgodne z następujących reguł:
* Nazwa właściwości sygnatury czasowej jest _z uwzględnieniem wielkości liter_.
* Wartość właściwości sygnatury czasowej pochodzące ze źródła zdarzenia w postaci ciągu JSON, powinien mieć format _RRRR-MM-Ddtgg. FFFFFFFK_. Na przykład taki ciąg "2008-04-12T12:53Z".
