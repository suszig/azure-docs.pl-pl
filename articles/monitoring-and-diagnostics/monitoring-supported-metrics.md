---
title: "Metryki Monitor Azure - obsługiwanych metryki na typ zasobu | Dokumentacja firmy Microsoft"
description: "Lista dostępnych dla każdego typu zasobu z monitorem Azure metryk."
author: anirudhcavale
manager: ashwink
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/25/2017
ms.author: ancav
ms.openlocfilehash: 05830547a5b8a24a59571edf6dd44d101b660189
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Obsługiwane metryki z monitorem Azure
Azure Monitor udostępnia kilka metod do interakcji z metryk, takich jak wykresy je w portalu, dostępu do nich za pośrednictwem interfejsu API REST lub zapytań je przy użyciu programu PowerShell lub interfejsu wiersza polecenia. Poniżej przedstawiono pełną listę wszystkich metryki obecnie z potoku metryki Azure monitora.

> [!NOTE]
> Inne metryki mogą być dostępne w portalu lub przy użyciu starszej wersji interfejsów API. Ta lista zawiera tylko metryk dostępnych za pośrednictwem potoku metryki skonsolidowanych Azure Monitor. Wykonanie kwerendy i dostępu do metryki z wymiarów, użyj [wersja interfejsu api 2017-05-01-preview](https://docs.microsoft.com/en-us/rest/api/monitor/metricdefinitions)
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|qpu_metric|QPU|Licznik|Średnia|QPU. Zakres 0-100 S1, 0 – 200 S2 i 0-400 dla S4|Nie wymiarów|
|memory_metric|Memory (Pamięć)|Bajty|Średnia|Pamięci. W zakresie 0 – 25 GB na S1, 0 – 50 GB na S2 i 0 – 100 GB dla S4|Nie wymiarów|
|TotalConnectionRequests|Całkowita liczba połączeń żądań|Licznik|Średnia|Całkowita liczba połączeń żądania. Są to przyjęcia.|Nie wymiarów|
|SuccessfullConnectionsPerSec|Udane połączenia na sekundę|CountPerSecond|Średnia|Szybkość zakończeń udane połączenie.|Nie wymiarów|
|TotalConnectionFailures|Całkowita liczba awarii|Licznik|Średnia|Całkowita liczba nieudanych prób połączenia.|Nie wymiarów|
|CurrentUserSessions|Bieżące sesje użytkowników|Licznik|Średnia|Bieżąca liczba ustanowionych sesji użytkownika.|Nie wymiarów|
|QueryPoolBusyThreads|Zapytanie z puli wątków zajęty|Licznik|Średnia|Liczba zajętych wątków w puli wątków zapytania.|Nie wymiarów|
|CommandPoolJobQueueLength|Długość kolejki zadań puli polecenia|Licznik|Średnia|Liczba zadań w kolejce polecenia puli wątków.|Nie wymiarów|
|ProcessingPoolJobQueueLength|Długość kolejki zadań przetwarzania w puli|Licznik|Średnia|Liczba zadań z systemem innym niż — I/O kolejki puli wątków przetwarzania.|Nie wymiarów|
|CurrentConnections|Połączenia: Bieżąca liczba połączeń|Licznik|Średnia|Bieżąca liczba połączeń klienta.|Nie wymiarów|
|CleanerCurrentPrice|Pamięci: Czyszczący bieżąca cena|Licznik|Średnia|Bieżąca cena pamięci $/ byte/godzina znormalizowane do 1000.|Nie wymiarów|
|CleanerMemoryShrinkable|Pamięci: Zmniejszania pamięci czyszczący|Bajty|Średnia|Ilość pamięci w bajtach, mogą ulec czyszcząca przeczyszczanie w tle.|Nie wymiarów|
|CleanerMemoryNonshrinkable|Pamięci: Czyszczący nonshrinkable pamięci|Bajty|Średnia|Ilość pamięci w bajtach, nie może ulec czyszcząca przeczyszczanie w tle.|Nie wymiarów|
|MemoryUsage|Pamięci: Użycie pamięci|Bajty|Średnia|Użycie pamięci przez proces serwera jako używaną przy obliczaniu czyszczący cena pamięci. Taki sam jak licznika Process\PrivateBytes powiększony o rozmiar danych mapowanych na pamięć, ignorując wszystkie pamięci, która została zamapowana lub przydzielone przez aparat analizy w pamięci xVelocity (VertiPaq) przekracza Limit pamięci silnik xVelocity.|Nie wymiarów|
|MemoryLimitHard|Pamięci: Twarde Limit pamięci|Bajty|Średnia|Limit pamięci twardym z pliku konfiguracji.|Nie wymiarów|
|MemoryLimitHigh|Pamięć: Wysoki Limit pamięci|Bajty|Średnia|Limit pamięci wysokiej z pliku konfiguracji.|Nie wymiarów|
|MemoryLimitLow|Pamięci: Niski Limit pamięci|Bajty|Średnia|Limit pamięci z pliku konfiguracji.|Nie wymiarów|
|MemoryLimitVertiPaq|Pamięci: VertiPaq Limit pamięci|Bajty|Średnia|Limit w pamięci z pliku konfiguracji.|Nie wymiarów|
|Przydział|Pamięć: limit przydziału|Bajty|Średnia|Limit przydziału pamięci, w bajtach. Limit przydziału pamięci jest nazywana rezerwacji pamięci grant lub pamięci.|Nie wymiarów|
|QuotaBlocked|Pamięci: Zablokowane przydziału|Licznik|Średnia|Bieżąca liczba żądań przydziału, które są zablokowane, dopóki nie są zwalniane innych przydziałów pamięci.|Nie wymiarów|
|VertiPaqNonpaged|Pamięci: Niestronicowana VertiPaq|Bajty|Średnia|Zablokowane bajtów pamięci zestawu do użycia przez aparat w pamięci roboczego.|Nie wymiarów|
|VertiPaqPaged|Pamięci: Stronicowanej VertiPaq|Bajty|Średnia|Liczba bajtów stronicowanej pamięci dla danych w pamięci.|Nie wymiarów|
|RowsReadPerSec|Przetwarzanie: Wiersze do odczytu na sekundę|CountPerSecond|Średnia|Liczba wierszy odczytywać wszystkie relacyjnych baz danych.|Nie wymiarów|
|RowsConvertedPerSec|Przetwarzanie: Wierszy przekonwertowane na sekundę|CountPerSecond|Średnia|Liczba wierszy przekonwertować podczas przetwarzania.|Nie wymiarów|
|RowsWrittenPerSec|Przetwarzanie: Wierszy, zapisany na sekundę|CountPerSecond|Średnia|Liczba wierszy, zapisany podczas przetwarzania.|Nie wymiarów|
|CommandPoolBusyThreads|Wątki: Polecenie zajęty z puli wątków|Licznik|Średnia|Liczba zajętych wątków w puli wątków polecenia.|Nie wymiarów|
|CommandPoolIdleThreads|Wątków: Polecenie puli wątków bezczynności|Licznik|Średnia|Liczba bezczynności wątków w puli wątków polecenia.|Nie wymiarów|
|LongParsingBusyThreads|Wątków: Podczas analizowania zajęty wątków Long|Licznik|Średnia|Liczba zajętych wątków w puli wątków długo analizy.|Nie wymiarów|
|LongParsingIdleThreads|Wątków: Podczas analizowania bezczynności wątków Long|Licznik|Średnia|Liczba bezczynności wątków w puli wątków długo analizy.|Nie wymiarów|
|LongParsingJobQueueLength|Wątki: Czas analizowania długość kolejki zadań|Licznik|Średnia|Liczba zadań w kolejce długo analizy puli wątków.|Nie wymiarów|
|ProcessingPoolBusyIOJobThreads|Wątki: Przetwarzania puli wątków zajęty zadania we/wy|Licznik|Średnia|Liczba wątków uruchomionych zadań we/wy w puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolBusyNonIOThreads|Wątków: Zajęty z puli wątków nie I/O przetwarzania|Licznik|Średnia|Liczba wątków uruchomionych zadań nie I/O w puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolIOJobQueueLength|Wątki: Przetwarzanie puli długość kolejki zadania we/wy|Licznik|Średnia|Liczba operacji We/Wy zadań w kolejce puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolIdleIOJobThreads|Wątki: Przetwarzania puli wątków bezczynności zadania we/wy|Licznik|Średnia|Liczba wątków bezczynności zadań we/wy w puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolIdleNonIOThreads|Wątki: Przetwarzanie bezczynności z puli wątków nie I/E|Licznik|Średnia|Liczba bezczynności wątków w puli wątków przetwarzania dedykowane dla zadań innych niż — I/O.|Nie wymiarów|
|QueryPoolIdleThreads|Wątków: Wątki bezczynności puli zapytania|Licznik|Średnia|Liczba wątków bezczynności zadań we/wy w puli wątków przetwarzania.|Nie wymiarów|
|QueryPoolJobQueueLength|Wątków: Lengt kolejki zadań puli zapytania|Licznik|Średnia|Liczba zadań w kolejce zapytań puli wątków.|Nie wymiarów|
|ShortParsingBusyThreads|Wątków: Podczas analizowania zajęty wątków krótkiej|Licznik|Średnia|Liczba wątków zajęty w krótkim analizy puli wątków.|Nie wymiarów|
|ShortParsingIdleThreads|Wątków: Podczas analizowania bezczynności wątków krótkiej|Licznik|Średnia|Liczba wątków bezczynności w krótkim analizy puli wątków.|Nie wymiarów|
|ShortParsingJobQueueLength|Wątków: Podczas analizowania długość kolejki zadań krótkiej|Licznik|Średnia|Liczba zadań w kolejce krótkich analizy puli wątków.|Nie wymiarów|
|memory_thrashing_metric|Wykonywania niepotrzebnych replik pamięci|Procent|Średnia|Średnia pamięci wykonywania niepotrzebnych replik.|Nie wymiarów|
|mashup_engine_qpu_metric|Aparat M QPU|Licznik|Średnia|Użycie QPU przez procesy wyszukiwarek zestawu połączonych danych|Nie wymiarów|
|mashup_engine_memory_metric|M aparat pamięci|Bajty|Średnia|Użycie pamięci przez procesy wyszukiwarek zestawu połączonych danych|Nie wymiarów|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalRequests|Brama całkowita liczba żądań|Licznik|Łącznie|Liczba żądań bramy|Lokalizacja, nazwy hosta|
|SuccessfulRequests|Bramy pomyślnych żądań|Licznik|Łącznie|Liczba żądań pomyślnie bramy|Lokalizacja, nazwy hosta|
|UnauthorizedRequests|Brama nieautoryzowanych żądań|Licznik|Łącznie|Liczba żądań nieautoryzowanych bramy|Lokalizacja, nazwy hosta|
|FailedRequests|Żądań zakończonych niepowodzeniem bramy|Licznik|Łącznie|Liczba błędów w żądaniach bramy|Lokalizacja, nazwy hosta|
|OtherRequests|Inne żądania bramy|Licznik|Łącznie|Liczba innych żądań bramy|Lokalizacja, nazwy hosta|
|Czas trwania|Łączny czas realizacji żądania bramy|w milisekundach|Średnia|Ogólny czas trwania z bramy żądań w milisekundach|Lokalizacja, nazwy hosta|
|Pojemność|Wydajność (wersja zapoznawcza)|Procent|Maksymalna|Metryki użycia usługi ApiManagement|Lokalizacja|

## <a name="microsoftautomationautomationaccounts"></a>Microsoft.Automation/automationAccounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalJob|Całkowita liczba zadań|Licznik|Łącznie|Całkowita liczba zadań|Nie wymiarów|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CoreCount|Dedykowany liczby rdzeni|Licznik|Łącznie|Całkowita liczba rdzeni dedykowanego konta usługi partia zadań|Nie wymiarów|
|TotalNodeCount|Liczba dedykowanych węzłów|Licznik|Łącznie|Całkowita liczba dedykowanych węzłów w ramach konta usługi partia zadań|Nie wymiarów|
|LowPriorityCoreCount|Liczby rdzeni LowPriority|Licznik|Łącznie|Całkowita liczba rdzeni niskiego priorytetu konta usługi partia zadań|Nie wymiarów|
|TotalLowPriorityNodeCount|Liczba węzłów o niskim priorytecie|Licznik|Łącznie|Całkowita liczba węzłów niskiego priorytetu w ramach konta usługi partia zadań|Nie wymiarów|
|CreatingNodeCount|Tworzenie liczba węzłów|Licznik|Łącznie|Liczba węzłów tworzona|Nie wymiarów|
|StartingNodeCount|Początkowa liczba węzłów|Licznik|Łącznie|Liczba węzłów uruchamiania|Nie wymiarów|
|WaitingForStartTaskNodeCount|Liczba węzłów zadań Start oczekiwania|Licznik|Łącznie|Liczba węzłów oczekiwanie na zakończenie zadania Start|Nie wymiarów|
|StartTaskFailedNodeCount|Zadania uruchamiania nie powiodło się liczba węzłów|Licznik|Łącznie|Liczba węzłów, gdzie uruchomić zadanie nie powiodło się|Nie wymiarów|
|IdleNodeCount|Liczba węzłów w stanie bezczynności|Licznik|Łącznie|Liczba węzłów bezczynności|Nie wymiarów|
|OfflineNodeCount|Liczba węzłów w trybie offline|Licznik|Łącznie|Liczba węzłów w trybie offline|Nie wymiarów|
|RebootingNodeCount|Liczba węzłów ponowny rozruch|Licznik|Łącznie|Liczba ponowny rozruch węzłów|Nie wymiarów|
|ReimagingNodeCount|Liczba węzłów ponownej instalacji systemu|Licznik|Łącznie|Liczba węzłów, ponownej instalacji systemu|Nie wymiarów|
|RunningNodeCount|Liczba węzłów|Licznik|Łącznie|Liczba uruchomionych węzłach|Nie wymiarów|
|LeavingPoolNodeCount|Pozostawienie liczba węzłów w puli|Licznik|Łącznie|Liczba węzłów, pozostawiając puli|Nie wymiarów|
|UnusableNodeCount|Nie można użyć liczba węzłów|Licznik|Łącznie|Liczba węzłów bezużyteczne|Nie wymiarów|
|PreemptedNodeCount|Wywłaszczone liczba węzłów|Licznik|Łącznie|Liczba węzłów przeniesiona|Nie wymiarów|
|TaskStartEvent|Zadanie rozpoczęcia zdarzenia|Licznik|Łącznie|Całkowita liczba zadań, które zostały uruchomione|Nie wymiarów|
|TaskCompleteEvent|Zadania ukończone zdarzenia|Licznik|Łącznie|Całkowita liczba zadań, które zostały ukończone|Nie wymiarów|
|TaskFailEvent|Zdarzenia błędów zadań|Licznik|Łącznie|Całkowita liczba zadań, które zostały ukończone w stanie niepowodzenia|Nie wymiarów|
|PoolCreateEvent|Pula Tworzenie zdarzenia|Licznik|Łącznie|Całkowita liczba pul, które zostały utworzone|Nie wymiarów|
|PoolResizeStartEvent|Zdarzenia rozpoczęcia zmiany rozmiaru puli|Licznik|Łącznie|Całkowita liczba zmienia rozmiar puli, które zostały uruchomione|Nie wymiarów|
|PoolResizeCompleteEvent|Zakończenie zdarzenia zmiany rozmiaru puli|Licznik|Łącznie|Całkowita liczba zmienia rozmiar puli, które zostały ukończone|Nie wymiarów|
|PoolDeleteStartEvent|Pula Usuń rozpoczęcia zdarzenia|Licznik|Łącznie|Całkowita liczba usuwa puli, które zostały uruchomione|Nie wymiarów|
|PoolDeleteCompleteEvent|Zakończenie zdarzenia Delete puli|Licznik|Łącznie|Całkowita liczba usuwa puli, które zostały ukończone|Nie wymiarów|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|connectedclients|Podłączeni klienci|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed|Całkowita liczba operacji|Licznik|Łącznie||Nie wymiarów|
|Trafienia w pamięci podręcznej|Trafień w pamięci podręcznej|Licznik|Łącznie||Nie wymiarów|
|cachemisses|Chybienia pamięci podręcznej|Licznik|Łącznie||Nie wymiarów|
|getcommands|Pobiera|Licznik|Łącznie||Nie wymiarów|
|setcommands|Zestawy|Licznik|Łącznie||Nie wymiarów|
|evictedkeys|Wykluczone kluczy|Licznik|Łącznie||Nie wymiarów|
|totalkeys|Wszystkie klucze|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys|Wygasłe kluczy|Licznik|Łącznie||Nie wymiarów|
|usedmemory|Używana pamięć|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss|Używana pamięć RSS|Bajty|Maksymalna||Nie wymiarów|
|serverLoad|Obciążenie serwera|Procent|Maksymalna||Nie wymiarów|
|cacheWrite|Pamięć podręczna zapisu|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead|Odczyt z pamięci podręcznej|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime|Procesor CPU|Procent|Maksymalna||Nie wymiarów|
|connectedclients0|Podłączeni klienci (niezależnego fragmentu 0)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed0|Całkowita liczba operacji (identyfikator niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|cachehits0|Trafień w pamięci podręcznej (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|cachemisses0|Chybienia pamięci podręcznej (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|getcommands0|Pobiera (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|setcommands0|Zestawy (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys0|Klucze wykluczonym (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|totalkeys0|Wszystkie klucze (niezależnego fragmentu 0)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys0|Wygasłe kluczy (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|usedmemory0|Używana pamięć (niezależnego fragmentu 0)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss0|Używana pamięć RSS (niezależnego fragmentu 0)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad0|Obciążenia serwera (niezależnego fragmentu 0)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite0|Pamięć podręczna zapisu (niezależnego fragmentu 0)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead0|Odczytu pamięci podręcznej (niezależnego fragmentu 0)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime0|Procesor (niezależnego fragmentu 0)|Procent|Maksymalna||Nie wymiarów|
|connectedclients1|Podłączeni klienci (niezależnego fragmentu 1)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed1|Całkowita liczba operacji (identyfikator niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|cachehits1|Trafień w pamięci podręcznej (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|cachemisses1|Chybienia pamięci podręcznej (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|getcommands1|Pobiera (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|setcommands1|Zestawy (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys1|Klucze wykluczonym (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|totalkeys1|Wszystkie klucze (niezależnego fragmentu 1)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys1|Wygasłe kluczy (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|usedmemory1|Używana pamięć (niezależnego fragmentu 1)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss1|Używana pamięć RSS (niezależnego fragmentu 1)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad1|Obciążenia serwera (niezależnego fragmentu 1)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite1|Pamięć podręczna zapisu (niezależnego fragmentu 1)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead1|Odczytu pamięci podręcznej (niezależnego fragmentu 1)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime1|Procesor (niezależnego fragmentu 1)|Procent|Maksymalna||Nie wymiarów|
|connectedclients2|Podłączeni klienci (niezależnego fragmentu 2)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed2|Całkowita liczba operacji (identyfikator niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|cachehits2|Trafień w pamięci podręcznej (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|cachemisses2|Chybienia pamięci podręcznej (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|getcommands2|Pobiera (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|setcommands2|Zestawy (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys2|Klucze wykluczonym (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|totalkeys2|Wszystkie klucze (niezależnego fragmentu 2)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys2|Wygasłe kluczy (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|usedmemory2|Używana pamięć (niezależnego fragmentu 2)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss2|Używana pamięć RSS (niezależnego fragmentu 2)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad2|Obciążenia serwera (niezależnego fragmentu 2)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite2|Pamięć podręczna zapisu (niezależnego fragmentu 2)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead2|Odczytu pamięci podręcznej (niezależnego fragmentu 2)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime2|Procesor (niezależnego fragmentu 2)|Procent|Maksymalna||Nie wymiarów|
|connectedclients3|Podłączeni klienci (niezależnego fragmentu 3)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed3|Całkowita liczba operacji (identyfikator niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|cachehits3|Trafień w pamięci podręcznej (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|cachemisses3|Chybienia pamięci podręcznej (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|getcommands3|Pobiera (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|setcommands3|Zestawy (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys3|Klucze wykluczonym (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|totalkeys3|Wszystkie klucze (niezależnego fragmentu 3)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys3|Wygasłe kluczy (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|usedmemory3|Używana pamięć (niezależnego fragmentu 3)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss3|Używana pamięć RSS (niezależnego fragmentu 3)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad3|Obciążenia serwera (niezależnego fragmentu 3)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite3|Pamięć podręczna zapisu (niezależnego fragmentu 3)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead3|Odczytu pamięci podręcznej (niezależnego fragmentu 3)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime3|Procesor (niezależnego fragmentu 3)|Procent|Maksymalna||Nie wymiarów|
|connectedclients4|Podłączeni klienci (niezależnego fragmentu 4)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed4|Całkowita liczba operacji (identyfikator niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|cachehits4|Trafień w pamięci podręcznej (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|cachemisses4|Chybienia pamięci podręcznej (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|getcommands4|Pobiera (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|setcommands4|Zestawy (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys4|Klucze wykluczonym (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|totalkeys4|Wszystkie klucze (niezależnego fragmentu 4)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys4|Wygasłe kluczy (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|usedmemory4|Używana pamięć (niezależnego fragmentu 4)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss4|Używana pamięć RSS (niezależnego fragmentu 4)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad4|Obciążenia serwera (niezależnego fragmentu 4)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite4|Pamięć podręczna zapisu (niezależnego fragmentu 4)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead4|Odczytu pamięci podręcznej (niezależnego fragmentu 4)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime4|Procesor (niezależnego fragmentu 4)|Procent|Maksymalna||Nie wymiarów|
|connectedclients5|Podłączeni klienci (niezależnego fragmentu 5)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed5|Całkowita liczba operacji (identyfikator niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|cachehits5|Trafień w pamięci podręcznej (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|cachemisses5|Chybienia pamięci podręcznej (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|getcommands5|Pobiera (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|setcommands5|Zestawy (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys5|Klucze wykluczonym (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|totalkeys5|Wszystkie klucze (niezależnego fragmentu 5)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys5|Wygasłe kluczy (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|usedmemory5|Używana pamięć (niezależnego fragmentu 5)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss5|Używana pamięć RSS (niezależnego fragmentu 5)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad5|Obciążenia serwera (niezależnego fragmentu 5)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite5|Pamięć podręczna zapisu (niezależnego fragmentu 5)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead5|Odczytu pamięci podręcznej (niezależnego fragmentu 5)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime5|Procesor (niezależnego fragmentu 5)|Procent|Maksymalna||Nie wymiarów|
|connectedclients6|Podłączeni klienci (niezależnego fragmentu 6)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed6|Całkowita liczba operacji (identyfikator niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|cachehits6|Trafień w pamięci podręcznej (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|cachemisses6|Chybienia pamięci podręcznej (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|getcommands6|Pobiera (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|setcommands6|Zestawy (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys6|Klucze wykluczonym (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|totalkeys6|Wszystkie klucze (niezależnego fragmentu 6)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys6|Wygasłe kluczy (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|usedmemory6|Używana pamięć (niezależnego fragmentu 6)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss6|Używana pamięć RSS (niezależnego fragmentu 6)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad6|Obciążenia serwera (niezależnego fragmentu 6)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite6|Pamięć podręczna zapisu (niezależnego fragmentu 6)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead6|Odczytu pamięci podręcznej (niezależnego fragmentu 6)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime6|Procesor (niezależnego fragmentu 6)|Procent|Maksymalna||Nie wymiarów|
|connectedclients7|Podłączeni klienci (niezależnego fragmentu 7)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed7|Całkowita liczba operacji (identyfikator niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|cachehits7|Trafień w pamięci podręcznej (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|cachemisses7|Chybienia pamięci podręcznej (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|getcommands7|Pobiera (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|setcommands7|Zestawy (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys7|Klucze wykluczonym (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|totalkeys7|Wszystkie klucze (niezależnego fragmentu 7)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys7|Wygasłe kluczy (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|usedmemory7|Używana pamięć (niezależnego fragmentu 7)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss7|Używana pamięć RSS (niezależnego fragmentu 7)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad7|Obciążenia serwera (niezależnego fragmentu 7)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite7|Pamięć podręczna zapisu (niezależnego fragmentu 7)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead7|Odczytu pamięci podręcznej (niezależnego fragmentu 7)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime7|Procesor (niezależnego fragmentu 7)|Procent|Maksymalna||Nie wymiarów|
|connectedclients8|Podłączeni klienci (niezależnego fragmentu 8)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed8|Całkowita liczba operacji (identyfikator niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|cachehits8|Trafień w pamięci podręcznej (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|cachemisses8|Chybienia pamięci podręcznej (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|getcommands8|Pobiera (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|setcommands8|Zestawy (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys8|Klucze wykluczonym (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|totalkeys8|Wszystkie klucze (niezależnego fragmentu 8)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys8|Wygasłe kluczy (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|usedmemory8|Używana pamięć (niezależnego fragmentu 8)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss8|Używana pamięć RSS (niezależnego fragmentu 8)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad8|Obciążenia serwera (niezależnego fragmentu 8)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite8|Pamięć podręczna zapisu (niezależnego fragmentu 8)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead8|Odczytu pamięci podręcznej (niezależnego fragmentu 8)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime8|Procesor (niezależnego fragmentu 8)|Procent|Maksymalna||Nie wymiarów|
|connectedclients9|Podłączeni klienci (niezależnego fragmentu 9)|Licznik|Maksymalna||Nie wymiarów|
|totalcommandsprocessed9|Całkowita liczba operacji (identyfikator niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|cachehits9|Trafień w pamięci podręcznej (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|cachemisses9|Chybienia pamięci podręcznej (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|getcommands9|Pobiera (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|setcommands9|Zestawy (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys9|Klucze wykluczonym (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|totalkeys9|Wszystkie klucze (niezależnego fragmentu 9)|Licznik|Maksymalna||Nie wymiarów|
|expiredkeys9|Wygasłe kluczy (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|usedmemory9|Używana pamięć (niezależnego fragmentu 9)|Bajty|Maksymalna||Nie wymiarów|
|usedmemoryRss9|Używana pamięć RSS (niezależnego fragmentu 9)|Bajty|Maksymalna||Nie wymiarów|
|serverLoad9|Obciążenia serwera (niezależnego fragmentu 9)|Procent|Maksymalna||Nie wymiarów|
|cacheWrite9|Pamięć podręczna zapisu (niezależnego fragmentu 9)|BytesPerSecond|Maksymalna||Nie wymiarów|
|cacheRead9|Odczytu pamięci podręcznej (niezależnego fragmentu 9)|BytesPerSecond|Maksymalna||Nie wymiarów|
|percentProcessorTime9|Procesor (niezależnego fragmentu 9)|Procent|Maksymalna||Nie wymiarów|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procent procesora CPU|Procent procesora CPU|Procent|Średnia|Procent obliczeniowe przydzielone jednostki, które są obecnie używane przez maszyny wirtualne.|Nie wymiarów|
|Sieci w|Sieci w|Bajty|Łącznie|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący).|Nie wymiarów|
|Sieci limit|Sieci limit|Bajty|Łącznie|Liczba bajtów na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący).|Nie wymiarów|
|Bajty odczytu dysku/s|Czas odczytu dyskowego|BytesPerSecond|Średnia|Średnią liczbę bajtów do odczytu z dysku podczas okresu monitorowania.|Nie wymiarów|
|Bajty zapisu dysku/s|Zapisu na dysku|BytesPerSecond|Średnia|Zapisywane na dysku podczas monitorowania okres średnią liczbę bajtów.|Nie wymiarów|
|Dysk operacje odczytu/s|Dysk operacje odczytu/s|CountPerSecond|Średnia|Odczyt dysku IOPS.|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średnia|Zapisu dyskowego IOPS.|Nie wymiarów|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalCalls|Całkowita liczba wywołań|Licznik|Łącznie|Całkowita liczba połączeń.|Nie wymiarów|
|SuccessfulCalls|Pomyślnych wywołań|Licznik|Łącznie|Liczba pomyślnych wywołań.|Nie wymiarów|
|TotalErrors|Całkowita liczba błędów|Licznik|Łącznie|Całkowita liczba wywołań z odpowiedzi na błąd (4xx kod odpowiedzi HTTP lub 5xx).|Nie wymiarów|
|BlockedCalls|Wywołania zablokowanych|Licznik|Łącznie|Liczba wywołań tej Przekroczono szybkość lub limit przydziału.|Nie wymiarów|
|ServerErrors|Błędy serwera|Licznik|Łącznie|Liczba wywołań z powodu błędu wewnętrznego usługi (5xx kod odpowiedzi HTTP).|Nie wymiarów|
|ClientErrors|Klientów z błędami|Licznik|Łącznie|Liczba wywołań z powodu błędu po stronie klienta (4xx kod odpowiedzi HTTP).|Nie wymiarów|
|DataIn|Dane w|Bajty|Łącznie|Rozmiar danych przychodzących w bajtach.|Nie wymiarów|
|DataOut|Dla danych wychodzących|Bajty|Łącznie|Rozmiar danych wychodzących w bajtach.|Nie wymiarów|
|Opóźnienie|Opóźnienie|W milisekundach|Średnia|Opóźnienie w milisekundach.|Nie wymiarów|
|CharactersTranslated|Znaki translacji|Licznik|Łącznie|Całkowita liczba znaków w przychodzące żądanie tekstu.|Nie wymiarów|
|SpeechSessionDuration|Mowy czas trwania sesji|Sekundy|Łącznie|Łączny czas trwania mowy sesji w sekundach.|Nie wymiarów|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procent procesora CPU|Procent procesora CPU|Procent|Średnia|Wartość procentowa obliczeniowe przydzielone jednostki, które są obecnie używane przez maszyny wirtualne|Nie wymiarów|
|Sieci w|Sieci w|Bajty|Łącznie|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący)|Nie wymiarów|
|Sieci limit|Sieci limit|Bajty|Łącznie|Liczba bajtów na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący)|Nie wymiarów|
|Bajty odczytu dysku|Bajty odczytu dysku|Bajty|Łącznie|Całkowita liczba bajtów do odczytu z dysku podczas okresu monitorowania|Nie wymiarów|
|Bajty zapisu dysku|Bajty zapisu dysku|Bajty|Łącznie|Całkowita liczba bajtów zapisywanych na dysku podczas okresu monitorowania|Nie wymiarów|
|Dysk operacje odczytu/s|Dysk operacje odczytu/s|CountPerSecond|Średnia|Czas odczytu dyskowego IOPS|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średnia|Zapisu dyskowego IOPS|Nie wymiarów|
|Pozostały kredyt Procesora|Pozostały kredyt Procesora|Licznik|Średnia|Całkowita liczba punkty serii|Nie wymiarów|
|Używane środków Procesora|Używane środków Procesora|Licznik|Średnia|Całkowita liczba kredytów używane przez maszynę wirtualną|Nie wymiarów|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procent procesora CPU|Procent procesora CPU|Procent|Średnia|Wartość procentowa obliczeniowe przydzielone jednostki, które są obecnie używane przez maszyny wirtualne|Nie wymiarów|
|Sieci w|Sieci w|Bajty|Łącznie|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący)|Nie wymiarów|
|Sieci limit|Sieci limit|Bajty|Łącznie|Liczba bajtów na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący)|Nie wymiarów|
|Bajty odczytu dysku|Bajty odczytu dysku|Bajty|Łącznie|Całkowita liczba bajtów do odczytu z dysku podczas okresu monitorowania|Nie wymiarów|
|Bajty zapisu dysku|Bajty zapisu dysku|Bajty|Łącznie|Całkowita liczba bajtów zapisywanych na dysku podczas okresu monitorowania|Nie wymiarów|
|Dysk operacje odczytu/s|Dysk operacje odczytu/s|CountPerSecond|Średnia|Czas odczytu dyskowego IOPS|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średnia|Zapisu dyskowego IOPS|Nie wymiarów|
|Pozostały kredyt Procesora|Pozostały kredyt Procesora|Licznik|Średnia|Całkowita liczba punkty serii|Nie wymiarów|
|Używane środków Procesora|Używane środków Procesora|Licznik|Średnia|Całkowita liczba kredytów używane przez maszynę wirtualną|Nie wymiarów|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procent procesora CPU|Procent procesora CPU|Procent|Średnia|Wartość procentowa obliczeniowe przydzielone jednostki, które są obecnie używane przez maszyny wirtualne|Nie wymiarów|
|Sieci w|Sieci w|Bajty|Łącznie|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący)|Nie wymiarów|
|Sieci limit|Sieci limit|Bajty|Łącznie|Liczba bajtów na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący)|Nie wymiarów|
|Bajty odczytu dysku|Bajty odczytu dysku|Bajty|Łącznie|Całkowita liczba bajtów do odczytu z dysku podczas okresu monitorowania|Nie wymiarów|
|Bajty zapisu dysku|Bajty zapisu dysku|Bajty|Łącznie|Całkowita liczba bajtów zapisywanych na dysku podczas okresu monitorowania|Nie wymiarów|
|Dysk operacje odczytu/s|Dysk operacje odczytu/s|CountPerSecond|Średnia|Czas odczytu dyskowego IOPS|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średnia|Zapisu dyskowego IOPS|Nie wymiarów|
|Pozostały kredyt Procesora|Pozostały kredyt Procesora|Licznik|Średnia|Całkowita liczba punkty serii|Nie wymiarów|
|Używane środków Procesora|Używane środków Procesora|Licznik|Średnia|Całkowita liczba kredytów używane przez maszynę wirtualną|Nie wymiarów|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|DCIApiCalls|Wywołania Insights interfejsu API klienta|Licznik|Łącznie||Nie wymiarów|
|DCIMappingImportOperationSuccessfulLines|Mapowanie wierszy pomyślnych operacji importu|Licznik|Łącznie||Nie wymiarów|
|DCIMappingImportOperationFailedLines|Mapowanie operacji importowania nie powiodła się wierszy|Licznik|Łącznie||Nie wymiarów|
|DCIMappingImportOperationTotalLines|Całkowita liczba wierszy operacji importu mapowania|Licznik|Łącznie||Nie wymiarów|
|DCIMappingImportOperationRuntimeInSeconds|Mapowanie importu operacji w czasie wykonywania w sekundach|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundProfileExportSucceeded|Eksportowanie profilu ruchu wychodzącego zakończyło się pomyślnie|Licznik|Łącznie||Nie wymiarów|
|DCIOutboundProfileExportFailed|Eksportowanie profilu ruchu wychodzącego nie powiodło się|Licznik|Łącznie||Nie wymiarów|
|DCIOutboundProfileExportDuration|Czas trwania eksportowania profilu ruchu wychodzącego|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundKpiExportSucceeded|Wychodzące eksportowanie Kpi powiodło się|Licznik|Łącznie||Nie wymiarów|
|DCIOutboundKpiExportFailed|Wychodzące eksportowanie wskaźnika Kpi nie powiodło się|Licznik|Łącznie||Nie wymiarów|
|DCIOutboundKpiExportDuration|Czas trwania wychodzącego eksportu wskaźnika Kpi|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundKpiExportStarted|Rozpoczęto eksportu wychodzącego kluczowego wskaźnika wydajności|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundKpiRecordCount|Liczba rekordów wychodzącego wskaźnika Kpi|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundProfileExportCount|Liczba eksportowania profilu ruchu wychodzącego|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundInitialProfileExportFailed|Eksportowanie wychodzącego początkowej profilu nie powiodło się|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundInitialProfileExportSucceeded|Wychodzące profilu początkowej eksportu|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundInitialKpiExportFailed|Wychodzące początkowej eksportowanie wskaźnika Kpi nie powiodło się|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundInitialKpiExportSucceeded|Wychodzące początkowej eksportu kluczowego wskaźnika wydajności zakończyło się pomyślnie.|Sekundy|Łącznie||Nie wymiarów|
|DCIOutboundInitialProfileExportDurationInSeconds|Wychodzące profilu początkowej eksportu czas w sekundach|Sekundy|Łącznie||Nie wymiarów|
|AdlaJobForStandardKpiFailed|Zadanie Adla dla standardowych wskaźnika Kpi nie powiodło się w sekundach|Sekundy|Łącznie||Nie wymiarów|
|AdlaJobForStandardKpiTimeOut|Zadanie Adla standardowa wskaźnika Kpi limitu czasu w sekundach|Sekundy|Łącznie||Nie wymiarów|
|AdlaJobForStandardKpiCompleted|Zadanie Adla standardowe kluczowego wskaźnika wydajności w sekundach|Sekundy|Łącznie||Nie wymiarów|
|ImportASAValuesFailed|Wartości ASA importu nie powiodła się, liczba|Licznik|Łącznie||Nie wymiarów|
|ImportASAValuesSucceeded|Wartości ASA importowanie zakończyło się pomyślnie, liczba|Licznik|Łącznie||Nie wymiarów|
|DCIProfilesCount|Liczba wystąpień profilu|Licznik|ostatni||Nie wymiarów|
|DCIInteractionsPerMonthCount|Interakcje na liczba miesięcy|Licznik|ostatni||Nie wymiarów|
|DCIKpisCount|Liczba wskaźnika KPI|Licznik|ostatni||Nie wymiarów|
|DCISegmentsCount|Liczba segmentów.|Licznik|ostatni||Nie wymiarów|
|DCIPredictiveMatchPoliciesCount|Liczba dopasowań predykcyjnej|Licznik|ostatni||Nie wymiarów|
|DCIPredictionsCount|Liczba prognozowania|Licznik|ostatni||Nie wymiarów|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|JobEndedSuccess|Zadania zakończone pomyślnie|Licznik|Łącznie|Liczba zadań zakończonych pomyślnie.|Nie wymiarów|
|JobEndedFailure|Zadania zakończone niepowodzeniem|Licznik|Łącznie|Liczba zadań zakończonych niepowodzeniem.|Nie wymiarów|
|JobEndedCancelled|Anulowane zadania|Licznik|Łącznie|Liczba zadań zostało anulowane.|Nie wymiarów|
|JobAUEndedSuccess|Pomyślne czasu Australia|Sekundy|Łącznie|Łączny czas Australia zadań zakończonych pomyślnie.|Nie wymiarów|
|JobAUEndedFailure|Czas Australia nie powiodło się|Sekundy|Łącznie|Łączny czas Australia zadania zakończone niepowodzeniem.|Nie wymiarów|
|JobAUEndedCancelled|Anulowane czasu Australia|Sekundy|Łącznie|Łączny czas Australia anulowane zadania.|Nie wymiarów|

## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.DataLakeStore/accounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalStorage|Całkowita ilość miejsca|Bajty|Maksymalna|Całkowita ilość danych przechowywanych w ramach konta.|Nie wymiarów|
|DataWritten|Dane zapisywane|Bajty|Łącznie|Całkowita ilość danych zapisywane na koncie.|Nie wymiarów|
|DataRead|Odczyt danych|Bajty|Łącznie|Łączna ilość danych do odczytu z konta.|Nie wymiarów|
|WriteRequests|Żądań zapisu|Licznik|Łącznie|Liczba danych żądań zapisu do konta.|Nie wymiarów|
|ReadRequests|Żądań odczytu|Licznik|Łącznie|Liczba danych do odczytu żądania do tego konta.|Nie wymiarów|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent procesora CPU|Procent|Średnia|Procent procesora CPU|Nie wymiarów|
|compute_limit|Obliczenia bazy danych limit jednostki|Licznik|Średnia|Obliczenia bazy danych limit jednostki|Nie wymiarów|
|compute_consumption_percent|Obliczenia bazy danych procent jednostki|Procent|Średnia|Obliczenia bazy danych procent jednostki|Nie wymiarów|
|memory_percent|Procent pamięci|Procent|Średnia|Procent pamięci|Nie wymiarów|
|io_consumption_percent|Procent we/wy|Procent|Średnia|Procent we/wy|Nie wymiarów|
|storage_percent|Procent użycia magazynu|Procent|Średnia|Procent użycia magazynu|Nie wymiarów|
|storage_used|Użyty magazyn|Bajty|Średnia|Użyty magazyn|Nie wymiarów|
|storage_limit|Limit magazynu|Bajty|Średnia|Limit magazynu|Nie wymiarów|
|active_connections|Całkowita liczba aktywnych połączeń|Licznik|Średnia|Całkowita liczba aktywnych połączeń|Nie wymiarów|
|connections_failed|Całkowita liczba połączeń nie powiodło się|Licznik|Średnia|Całkowita liczba połączeń nie powiodło się|Nie wymiarów|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent procesora CPU|Procent|Średnia|Procent procesora CPU|Nie wymiarów|
|compute_limit|Obliczenia bazy danych limit jednostki|Licznik|Średnia|Obliczenia bazy danych limit jednostki|Nie wymiarów|
|compute_consumption_percent|Obliczenia bazy danych procent jednostki|Procent|Średnia|Obliczenia bazy danych procent jednostki|Nie wymiarów|
|memory_percent|Procent pamięci|Procent|Średnia|Procent pamięci|Nie wymiarów|
|io_consumption_percent|Procent we/wy|Procent|Średnia|Procent we/wy|Nie wymiarów|
|storage_percent|Procent użycia magazynu|Procent|Średnia|Procent użycia magazynu|Nie wymiarów|
|storage_used|Użyty magazyn|Bajty|Średnia|Użyty magazyn|Nie wymiarów|
|storage_limit|Limit magazynu|Bajty|Średnia|Limit magazynu|Nie wymiarów|
|active_connections|Całkowita liczba aktywnych połączeń|Licznik|Średnia|Całkowita liczba aktywnych połączeń|Nie wymiarów|
|connections_failed|Całkowita liczba połączeń nie powiodło się|Licznik|Średnia|Całkowita liczba połączeń nie powiodło się|Nie wymiarów|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Próby wysłania wiadomości telemetrii|Licznik|Łącznie|Liczba komunikatów telemetrii urządzenia do chmury próby wysłania do Centrum IoT|Nie wymiarów|
|d2c.telemetry.ingress.SUCCESS|Wysłane wiadomości telemetrii|Licznik|Łącznie|Liczba komunikatów telemetrii urządzenia do chmury pomyślnie wysłane do Centrum IoT|Nie wymiarów|
|c2d.Commands.egress.COMPLETE.SUCCESS|Polecenia zakończona|Licznik|Łącznie|Liczba poleceń chmury do urządzenia, które zakończyło się powodzeniem, urządzenia|Nie wymiarów|
|c2d.Commands.egress.Abandon.SUCCESS|Porzucone poleceń|Licznik|Łącznie|Liczba poleceń chmury do urządzenia porzucone przez urządzenie|Nie wymiarów|
|c2d.Commands.egress.Reject.SUCCESS|Polecenia odrzucone|Licznik|Łącznie|Liczba odrzuconych przez urządzenie poleceń chmury do urządzenia|Nie wymiarów|
|devices.totalDevices|Łączna liczba urządzeń|Licznik|Łącznie|Liczba urządzeń zarejestrowanych do Centrum IoT|Nie wymiarów|
|devices.connectedDevices.allProtocol|Połączonych urządzeń|Licznik|Łącznie|Liczba urządzeń połączonych z Centrum IoT|Nie wymiarów|
|d2c.telemetry.egress.SUCCESS|Dostarczane wiadomości telemetrii|Licznik|Łącznie|Liczba komunikatów pomyślnie zostały zapisane do punktów końcowych (łącznie)|Nie wymiarów|
|d2c.telemetry.egress.dropped|Porzucone komunikaty|Licznik|Łącznie|Liczba wiadomości porzuconych z powodu braku punktu końcowego dostarczania|Nie wymiarów|
|d2c.telemetry.egress.orphaned|Oddzielone wiadomości|Licznik|Łącznie|Aktualna liczba wiadomości nie zgodnych żadnych tras, łącznie z rezerwowego trasy|Nie wymiarów|
|d2c.telemetry.egress.invalid|Nieprawidłowy wiadomości|Licznik|Łącznie|Aktualna liczba wiadomości nie są dostarczane z powodu niezgodności z punktu końcowego|Nie wymiarów|
|d2c.telemetry.egress.Fallback|Wiadomości pasujące rezerwowy warunku|Licznik|Łącznie|Liczba komunikatów zapisanych do rezerwowego punktu końcowego|Nie wymiarów|
|d2c.endpoints.egress.eventHubs|Komunikaty dostarczone do Centrum zdarzeń punkty końcowe|Licznik|Łącznie|Liczba wiadomości zostały pomyślnie zapisane punkty końcowe Centrum zdarzeń|Nie wymiarów|
|d2c.endpoints.latency.eventHubs|Opóźnienie komunikat dla punktów końcowych Centrum zdarzeń|w milisekundach|Średnia|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego Centrum zdarzeń, w milisekundach|Nie wymiarów|
|d2c.endpoints.egress.serviceBusQueues|Komunikaty dostarczone do punktów końcowych z kolejką usługi Service Bus|Licznik|Łącznie|Liczba wiadomości kolejką usługi Service Bus punkty końcowe zostały pomyślnie zapisane|Nie wymiarów|
|d2c.endpoints.latency.serviceBusQueues|Czas oczekiwania na wiadomość dla punktów końcowych z kolejką usługi Service Bus|w milisekundach|Średnia|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego kolejką usługi Service Bus, w milisekundach|Nie wymiarów|
|d2c.endpoints.egress.serviceBusTopics|Komunikaty dostarczone do punktów końcowych temat magistrali usług|Licznik|Łącznie|Liczba wiadomości zostały pomyślnie zapisane punkty końcowe temat magistrali usług|Nie wymiarów|
|d2c.endpoints.latency.serviceBusTopics|Opóźnienie komunikat dla punktów końcowych temat magistrali usług|w milisekundach|Średnia|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego Service Bus tematu w milisekundach|Nie wymiarów|
|d2c.endpoints.egress.builtIn.events|Wiadomość dostarczona do wbudowanej punktu końcowego (wiadomości/zdarzeń)|Licznik|Łącznie|Liczba komunikatów wbudowanym punktem końcowym (wiadomości/zdarzeń) zostały pomyślnie zapisane|Nie wymiarów|
|d2c.endpoints.latency.builtIn.events|Czas oczekiwania wiadomości na wbudowanym punktem końcowym (wiadomości/zdarzeń)|w milisekundach|Średnia|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i transfer danych przychodzących wiadomości na wbudowanym punktem końcowym (wiadomości/zdarzeń), w milisekundach |Nie wymiarów|
|d2c.endpoints.egress.Storage|Komunikaty dostarczone do punktów końcowych magazynu|Licznik|Łącznie|Liczba wiadomości zostały pomyślnie zapisane punkty końcowe usługi storage|Nie wymiarów|
|d2c.endpoints.latency.Storage|Opóźnienie komunikat dla punktów końcowych magazynu|w milisekundach|Średnia|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego magazynu (w milisekundach)|Nie wymiarów|
|d2c.endpoints.egress.Storage.Bytes|Dane zapisywane w pamięci masowej|Bajty|Łącznie|Ilość danych w bajtach, zapisane punkty końcowe usługi storage|Nie wymiarów|
|d2c.endpoints.egress.Storage.blobs|Zapisane do magazynu obiektów blob|Licznik|Łącznie|Liczba zapisywane do punktów końcowych magazynu obiektów blob|Nie wymiarów|
|d2c.Twin.Read.SUCCESS|Pomyślne dwie odczytuje z urządzeń|Licznik|Łącznie|Liczba wszystkich pomyślnych odczyty dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Read.failure|Nie powiodło się dwie odczyty z urządzeń|Licznik|Łącznie|Liczba wszystkich nie odczyty dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Read.size|Rozmiar odpowiedzi odczytów dwie z urządzeń|Bajty|Średnia|Średnia, min i max z wszystkie udane, inicjowanych przez urządzenie dwie odczytów.|Nie wymiarów|
|d2c.Twin.Update.SUCCESS|Pomyślne dwie aktualizacje z urządzeń|Licznik|Łącznie|Liczba wszystkich pomyślnych aktualizacji dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Update.failure|Nie powiodło się dwie aktualizacje z urządzeń|Licznik|Łącznie|Liczba wszystkich nie aktualizacje dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Update.size|Rozmiar aktualizacji dwie z urządzeń|Bajty|Średnia|Średnia, minimum i maksymalny rozmiar wszystkich pomyślnych, inicjowanych przez urządzenie dwie aktualizacji.|Nie wymiarów|
|c2d.Methods.SUCCESS|Pomyślne bezpośrednie wywołania metod|Licznik|Łącznie|Liczba wszystkich pomyślnych wywołań metody bezpośredniego.|Nie wymiarów|
|c2d.Methods.failure|Nie powiodło się bezpośrednie wywołania metod|Licznik|Łącznie|Liczba wszystkich nie wywołania metody bezpośredniego.|Nie wymiarów|
|c2d.Methods.requestSize|Rozmiar żądania bezpośrednie wywołania metod|Bajty|Średnia|Średnia, min i max wszystkich pomyślnych żądań metoda bezpośrednia.|Nie wymiarów|
|c2d.Methods.responseSize|Rozmiar odpowiedzi bezpośrednie wywołania metod|Bajty|Średnia|Średnia, minimum i maksimum wszystkie udane metoda bezpośrednia odpowiedzi.|Nie wymiarów|
|c2d.Twin.Read.SUCCESS|Pomyślne dwie odczytuje z zaplecza|Licznik|Łącznie|Liczba wszystkich pomyślnie zainicjował zakończenia wstecz dwie odczytuje.|Nie wymiarów|
|c2d.Twin.Read.failure|Nie powiodło się dwie odczytuje z zaplecza|Licznik|Łącznie|Liczba wszystkich nie odczyty wstecz zakończenia inicjowane przez dwie.|Nie wymiarów|
|c2d.Twin.Read.size|Rozmiar odpowiedzi odczytów dwie z wewnętrznego|Bajty|Średnia|Średnia, min i max z wszystkie udane, Wstecz zakończenia inicjowane przez dwie odczytów.|Nie wymiarów|
|c2d.Twin.Update.SUCCESS|Pomyślne dwie aktualizacje z wewnętrznego|Licznik|Łącznie|Liczba wszystkich pomyślnych aktualizacji inicjowane zakończenia wstecz dwie.|Nie wymiarów|
|c2d.Twin.Update.failure|Nie powiodło się dwie aktualizacje z wewnętrznego|Licznik|Łącznie|Liczba wszystkich nie powiodło się wstecz zakończenia inicjowane przez dwie aktualizacje.|Nie wymiarów|
|c2d.Twin.Update.size|Rozmiar aktualizacji dwie z wewnętrznego|Bajty|Średnia|Średnia, minimum i maksymalny rozmiar wszystkich pomyślnych, Wstecz zakończenia inicjowane przez dwie aktualizacji.|Nie wymiarów|
|twinQueries.success|Dwie pomyślne zapytania|Licznik|Łącznie|Liczba wszystkich zapytań dwie powiodło się.|Nie wymiarów|
|twinQueries.failure|Nie powiodło się dwie zapytań|Licznik|Łącznie|Liczba wszystkich zapytań dwie nie powiodło się.|Nie wymiarów|
|twinQueries.resultSize|Rozmiar wyników zapytania dwie|Bajty|Średnia|Średnia, minimum i maksymalny rozmiar wyników wszystkich zapytań dwie powiodło się.|Nie wymiarów|
|jobs.createTwinUpdateJob.success|Pomyślne utworzenie kont dwie aktualizacji zadań|Licznik|Łącznie|Liczba wszystkich pomyślnym utworzeniu zadania aktualizacji dwie.|Nie wymiarów|
|jobs.createTwinUpdateJob.failure|Nie powiodło się tworzenie dwie aktualizacji zadań|Licznik|Łącznie|Liczba wszystkich nie powiodło się tworzenie zadań aktualizacji dwie.|Nie wymiarów|
|jobs.createDirectMethodJob.success|Pomyślne tworzenia zadań wywołania — metoda|Licznik|Łącznie|Liczba wszystkich pomyślnym utworzeniu zadania wywołania metody bezpośredniego.|Nie wymiarów|
|jobs.createDirectMethodJob.failure|Nie powiodło się tworzenie zadań wywołania — metoda|Licznik|Łącznie|Liczba wszystkich nie powiodło się tworzenie zadań wywołania metody bezpośredniego.|Nie wymiarów|
|jobs.listJobs.success|Pomyślnych wywołań do listy zadań|Licznik|Łącznie|Liczba wszystkich pomyślnych wywołań do listy zadań.|Nie wymiarów|
|jobs.listJobs.failure|Nie powiodło się wywołania do listy zadań|Licznik|Łącznie|Liczba zakończonych niepowodzeniem wywołania do listy zadań.|Nie wymiarów|
|jobs.cancelJob.success|Anulowane zadania powiodło się|Licznik|Łącznie|Liczba wszystkich pomyślnych wywołań, aby anulować zadanie.|Nie wymiarów|
|jobs.cancelJob.failure|Anulowanie zadania nie powiodło się|Licznik|Łącznie|Liczba wszystkich wywołań nie powiodło się, aby anulować zadanie.|Nie wymiarów|
|jobs.queryJobs.success|Pomyślnie wykonane zadanie zapytań|Licznik|Łącznie|Liczba wszystkich pomyślnych wywołań do zadań zapytania.|Nie wymiarów|
|jobs.queryJobs.failure|Nie powiodło się zadanie odpytuje|Licznik|Łącznie|Liczba wszystkie wywołania zakończone niepowodzeniem zadania zapytania.|Nie wymiarów|
|Jobs.Completed|Zadania ukończone|Licznik|Łącznie|Liczba wszystkich zakończonych zadań.|Nie wymiarów|
|Jobs.failed|Zadania zakończone niepowodzeniem|Licznik|Łącznie|Liczba wszystkie zadania zakończone niepowodzeniem.|Nie wymiarów|
|d2c.telemetry.ingress.sendThrottle|Liczba błędów ograniczania przepustowości|Licznik|Łącznie|Ogranicza liczbę błędów ograniczania przepustowości z powodu przepływności urządzenia|Nie wymiarów|
|dailyMessageQuotaUsed|Całkowita liczba komunikatów używany|Licznik|Średnia|Liczba całkowita liczba komunikatów używany w tej chwili|Nie wymiarów|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|RegistrationAttempts|Prób rejestracji|Licznik|Łącznie|Liczba prób rejestracji urządzenia|Stan ProvisioningServiceName, IotHubName,|
|DeviceAssignments|Urządzenia przypisane|Licznik|Łącznie|Liczba urządzenia przypisane do Centrum IoT|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Poświadczenie prób|Licznik|Łącznie|Liczba podjęto poświadczenia urządzenia|Protokół ProvisioningServiceName, stan,|

## <a name="microsoftdocumentdbdatabaseaccounts-cosmosdb"></a>Microsoft.DocumentDB/databaseAccounts (CosmosDB)
|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalRequests|Całkowita liczba żądań|Licznik|Licznik|Liczba żądań|DatabaseAccount, CollectionName, DatabaseName, Region, StatusCode|
|MongoRequests|MONGO żądań|Licznik|Licznik|Liczba złożonych żądań Mongo|DatabaseAccount, CollectionName, DatabaseName, Region, kod błędu, CommandName|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|INREQS|Przychodzących żądań wysłania|Licznik|Łącznie|Całkowita liczba przychodzących wysyłać żądania dla przestrzeni nazw|Nie wymiarów|
|SUCCREQ|Liczba pomyślnych żądań|Licznik|Łącznie|Całkowita liczba pomyślnych żądań dla przestrzeni nazw|Nie wymiarów|
|FAILREQ|Żądań zakończonych niepowodzeniem|Licznik|Łącznie|Całkowita liczba żądań zakończonych niepowodzeniem w przypadku przestrzeni nazw|Nie wymiarów|
|SVRBSY|Błędy zajęty serwera|Licznik|Łącznie|Błędy zajęty całego serwera w przypadku przestrzeni nazw|Nie wymiarów|
|INTERR|Błędy wewnętrzne serwera|Licznik|Łącznie|Błędy całkowita wewnętrznego serwera w przypadku przestrzeni nazw|Nie wymiarów|
|MISCERR|Inne błędy|Licznik|Łącznie|Całkowita liczba żądań zakończonych niepowodzeniem w przypadku przestrzeni nazw|Nie wymiarów|
|INMSGS|Komunikaty przychodzące (przestarzałe)|Licznik|Łącznie|Całkowita liczba komunikatów przychodzących w przypadku przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Metryka wiadomości przychodzących|Nie wymiarów|
|EHINMSGS|Przychodzące komunikaty|Licznik|Łącznie|Całkowita liczba komunikatów przychodzących w przypadku przestrzeni nazw|Nie wymiarów|
|OUTMSGS|Wysyła komunikaty wychodzące (przestarzałe)|Licznik|Łącznie|Całkowita liczba wychodzących wiadomości w przypadku przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Metryka wysyła komunikaty wychodzące|Nie wymiarów|
|EHOUTMSGS|Wysyła komunikaty wychodzące|Licznik|Łącznie|Całkowita liczba wychodzących wiadomości w przypadku przestrzeni nazw|Nie wymiarów|
|EHINMBS|Przychodzące bajty (przestarzały)|Bajty|Łącznie|Zdarzenia koncentratora przychodzącego komunikatu przepływność dla przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Metryka Bajty przychodzące|Nie wymiarów|
|EHINBYTES|Przychodzące bajty|Bajty|Łącznie|Zdarzenia koncentratora przychodzącego komunikatu przepływność dla przestrzeni nazw|Nie wymiarów|
|EHOUTMBS|Wychodzące bajty (przestarzały)|Bajty|Łącznie|Zdarzenia koncentratora wychodzących wiadomości przepływność dla przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Wychodzące bajty metryki|Nie wymiarów|
|EHOUTBYTES|Wychodzące bajty|Bajty|Łącznie|Zdarzenia koncentratora wychodzących wiadomości przepływność dla przestrzeni nazw|Nie wymiarów|
|EHABL|Archiwum zaległości komunikatów|Licznik|Łącznie|Komunikaty o zdarzeniach Centrum archiwum zaległe dla przestrzeni nazw|Nie wymiarów|
|EHAMSGS|Archiwizowanie komunikatów|Licznik|Łącznie|Centrum zdarzeń zarchiwizowane wiadomości w przestrzeni nazw|Nie wymiarów|
|EHAMBS|Przepływność komunikat archiwum|Bajty|Łącznie|Centrum zdarzeń zarchiwizowane przepływności komunikatu w przestrzeni nazw|Nie wymiarów|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|ObservedMetricValue|Odczytaną wartość metryki|Licznik|Średnia|Wartość obliczana przez skalowania automatycznego podczas wykonywania|MetricTriggerSource|
|MetricThreshold|Próg metryki|Licznik|Średnia|Próg skonfigurowanych skalowania automatycznego podczas automatycznego skalowania.|MetricTriggerRule|
|ObservedCapacity|Obserwowana wydajność|Licznik|Średnia|Pojemność zgłaszane do skalowania automatycznego podczas jej wykonywania.|Nie wymiarów|
|ScaleActionsInitiated|Zainicjowano akcji skalowania|Licznik|Łącznie|Kierunek operacji skalowania.|ScaleDirection|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|RunsStarted|Uruchamia uruchomiona|Licznik|Łącznie|Liczba rozpoczętych przebiegów przepływu pracy.|Nie wymiarów|
|RunsCompleted|Zakończonych uruchomień|Licznik|Łącznie|Liczba ukończonych przebiegów przepływu pracy.|Nie wymiarów|
|RunsSucceeded|Uruchamia powiodło się.|Licznik|Łącznie|Liczba przebiegów przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|RunsFailed|Uruchamia nie powiodło się|Licznik|Łącznie|Liczba przebiegów przepływu pracy zakończonych niepowodzeniem.|Nie wymiarów|
|RunsCancelled|Uruchamia anulowane|Licznik|Łącznie|Liczba anulowanych przebiegów przepływu pracy.|Nie wymiarów|
|RunLatency|Opóźnienia uruchomienia|Sekundy|Średnia|Opóźnienie ukończonych przebiegów przepływu pracy.|Nie wymiarów|
|RunSuccessLatency|Uruchom opóźnienia Powodzenie|Sekundy|Średnia|Opóźnienie przebiegów przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|RunThrottledEvents|Uruchom z ograniczeniem przepustowości zdarzenia|Licznik|Łącznie|Liczba zdarzeń ograniczenia akcji lub wyzwalacza przepływu pracy.|Nie wymiarów|
|RunFailurePercentage|Uruchom procent awarii|Procent|Łącznie|Procent przepływu pracy jest uruchamiane nie powiodło się.|Nie wymiarów|
|ActionsStarted|Akcje uruchomiona |Licznik|Łącznie|Liczba rozpoczętych akcji przepływu pracy.|Nie wymiarów|
|ActionsCompleted|Działania ukończone |Licznik|Łącznie|Liczba ukończonych akcji przepływu pracy.|Nie wymiarów|
|ActionsSucceeded|Akcje powiodło się. |Licznik|Łącznie|Liczba akcji przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|ActionsFailed|Akcje nie powiodło się|Licznik|Łącznie|Liczba akcji przepływu pracy zakończonych niepowodzeniem.|Nie wymiarów|
|ActionsSkipped|Akcje pominięty |Licznik|Łącznie|Liczba pominiętych akcji przepływu pracy.|Nie wymiarów|
|ActionLatency|Opóźnienie akcji |Sekundy|Średnia|Opóźnienie ukończonych akcji przepływu pracy.|Nie wymiarów|
|ActionSuccessLatency|Działanie sukces opóźnienia |Sekundy|Średnia|Opóźnienie akcji przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|ActionThrottledEvents|Akcja ograniczany zdarzenia|Licznik|Łącznie|Liczba zdarzeń ograniczenia akcji przepływu pracy.|Nie wymiarów|
|TriggersStarted|Wyzwalacze uruchomiona |Licznik|Łącznie|Liczba rozpoczętych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggersCompleted|Wyzwalacze ukończone |Licznik|Łącznie|Liczba ukończonych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggersSucceeded|Wyzwalacze powiodło się. |Licznik|Łącznie|Liczba wyzwalaczy przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|TriggersFailed|Wyzwalacze nie powiodło się |Licznik|Łącznie|Liczba wyzwalaczy przepływu pracy zakończonych niepowodzeniem.|Nie wymiarów|
|TriggersSkipped|Wyzwalacze pominięty|Licznik|Łącznie|Liczba pominiętych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggersFired|Wyzwalacze uruchamiany |Licznik|Łącznie|Liczba uaktywnionych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggerLatency|Opóźnienia wyzwalacza |Sekundy|Średnia|Opóźnienie ukończonych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggerFireLatency|Wyzwalacz Fire opóźnienia |Sekundy|Średnia|Opóźnienie uaktywnionych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggerSuccessLatency|Wyzwalacz Powodzenie opóźnienia |Sekundy|Średnia|Opóźnienie wyzwalaczy przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|TriggerThrottledEvents|Wyzwalacz ograniczany zdarzenia|Licznik|Łącznie|Liczba zdarzeń ograniczenia wyzwalacza przepływu pracy.|Nie wymiarów|
|BillableActionExecutions|Wykonaniami rozliczeniowy akcji|Licznik|Łącznie|Liczba pobierania rozliczane wykonania akcji przepływu pracy.|Nie wymiarów|
|BillableTriggerExecutions|Wykonaniami rozliczeniowy wyzwalacza|Licznik|Łącznie|Liczba pobierania rozliczane wykonaniami wyzwalacz przepływu pracy.|Nie wymiarów|
|TotalBillableExecutions|Całkowita liczba wykonaniami rozliczeń|Licznik|Łącznie|Liczba pobierania rozliczane wykonania przepływu pracy.|Nie wymiarów|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|VipAvailability|Dostępność adresu VIP|Licznik|Średnia|Dostępność punktów końcowych VIP, na podstawie wyników badania|Wirtualny adres IP, VipPort|
|DipAvailability|Dostępność DIP|Licznik|Średnia|Dostępność punktów końcowych DIP, na podstawie wyników badania|ProtocolType, DipPort, wirtualny adres IP, VipPort, DipAddress|
|ByteCount|Liczba bajtów|Licznik|Łącznie|Całkowita liczba bajtów przesłanych w określonym przedziale czasu|Wirtualny adres IP, VipPort, kierunku|
|PacketCount|Liczba pakietów|Licznik|Łącznie|Całkowita liczba pakietów wysłanych w ciągu okresu czasu|Wirtualny adres IP, VipPort, kierunku|
|SYNCount|Liczba SYN|Licznik|Łącznie|Całkowita liczba pakietów SYN transmitowane w określonym przedziale czasu|Wirtualny adres IP, VipPort, kierunku|
|SnatConnectionCount|Liczba połączeń SNAT|Licznik|Łącznie|Całkowita liczba nowych połączeń SNAT utworzona w okresie czasu|Element ConnectionState wirtualny adres IP, DipAddress,|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|PacketsInDDoS|Liczba przychodzących pakietów DDoS|CountPerSecond|Średnia|Liczba przychodzących pakietów DDoS|Nie wymiarów|
|PacketsDroppedDDoS|Liczba porzuconych pakietów przychodzących DDoS|CountPerSecond|Średnia|Liczba porzuconych pakietów przychodzących DDoS|Nie wymiarów|
|PacketsForwardedDDoS|Pakiety przychodzące przekazywane przed atakami DDoS|CountPerSecond|Średnia|Pakiety przychodzące przekazywane przed atakami DDoS|Nie wymiarów|
|TCPPacketsInDDoS|Liczba przychodzących pakietów TCP przed atakami DDoS|CountPerSecond|Średnia|Liczba przychodzących pakietów TCP przed atakami DDoS|Nie wymiarów|
|TCPPacketsDroppedDDoS|Pakiety przychodzące TCP porzucony przed atakami DDoS|CountPerSecond|Średnia|Pakiety przychodzące TCP porzucony przed atakami DDoS|Nie wymiarów|
|TCPPacketsForwardedDDoS|Pakiety przychodzące TCP przekazywane przed atakami DDoS|CountPerSecond|Średnia|Pakiety przychodzące TCP przekazywane przed atakami DDoS|Nie wymiarów|
|UDPPacketsInDDoS|Liczba przychodzących pakietów UDP DDoS|CountPerSecond|Średnia|Liczba przychodzących pakietów UDP DDoS|Nie wymiarów|
|UDPPacketsDroppedDDoS|Przychodzących pakietów UDP porzucony przed atakami DDoS|CountPerSecond|Średnia|Przychodzących pakietów UDP porzucony przed atakami DDoS|Nie wymiarów|
|UDPPacketsForwardedDDoS|Przekazywane DDoS przychodzących pakietów UDP|CountPerSecond|Średnia|Przekazywane DDoS przychodzących pakietów UDP|Nie wymiarów|
|BytesInDDoS|Przychodzące bajty przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty przed atakami DDoS|Nie wymiarów|
|BytesDroppedDDoS|Przychodzące bajty porzucony przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty porzucony przed atakami DDoS|Nie wymiarów|
|BytesForwardedDDoS|Przychodzące bajty przekazywane przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty przekazywane przed atakami DDoS|Nie wymiarów|
|TCPBytesInDDoS|Przychodzące bajty TCP przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty TCP przed atakami DDoS|Nie wymiarów|
|TCPBytesDroppedDDoS|Przychodzące bajty TCP porzucony przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty TCP porzucony przed atakami DDoS|Nie wymiarów|
|TCPBytesForwardedDDoS|Przychodzące bajty TCP przekazywane przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty TCP przekazywane przed atakami DDoS|Nie wymiarów|
|UDPBytesInDDoS|Przychodzące bajty UDP DDoS|BytesPerSecond|Średnia|Przychodzące bajty UDP DDoS|Nie wymiarów|
|UDPBytesDroppedDDoS|Przychodzące bajty UDP porzucony przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty UDP porzucony przed atakami DDoS|Nie wymiarów|
|UDPBytesForwardedDDoS|Przychodzące bajty UDP przekazywane przed atakami DDoS|BytesPerSecond|Średnia|Przychodzące bajty UDP przekazywane przed atakami DDoS|Nie wymiarów|
|IfUnderDDoSAttack|W obszarze DDoS ataku lub nie|Licznik|Średnia|W obszarze DDoS ataku lub nie|Nie wymiarów|
|DDoSTriggerTCPPackets|Liczba przychodzących pakietów TCP do wyzwolenia ograniczenie DDoS|CountPerSecond|Średnia|Liczba przychodzących pakietów TCP do wyzwolenia ograniczenie DDoS|Nie wymiarów|
|DDoSTriggerUDPPackets|Liczba przychodzących pakietów UDP do wyzwolenia ograniczenie DDoS|CountPerSecond|Średnia|Liczba przychodzących pakietów UDP do wyzwolenia ograniczenie DDoS|Nie wymiarów|
|VipAvailability|Dostępność|Licznik|Średnia|Średnia dostępność adres IP w przedziale czasu|Port|
|ByteCount|Liczba bajtów|Licznik|Łącznie|Całkowita liczba bajtów przesłanych w określonym przedziale czasu|Portu i kierunek|
|PacketCount|Liczba pakietów|Licznik|Łącznie|Całkowita liczba pakietów wysłanych w ciągu okresu czasu|Portu i kierunek|
|SynCount|Liczba SYN|Licznik|Łącznie|Całkowita liczba pakietów SYN transmitowane w określonym przedziale czasu|Portu i kierunek|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Przepływność|Przepływność|BytesPerSecond|Średnia|Liczba bajtów na sekundę, ma obsłużone bramy aplikacji|Nie wymiarów|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Tunel przepustowości|BytesPerSecond|Średnia|Średnia przepustowość tunelu w bajtach na sekundę|ConnectionName, RemoteIP|
|TunnelPeakBandwidth|Szczytowa tunelu przepustowość|BytesPerSecond|Średnia|Szczytowa przepustowość tunelu w bajtach na sekundę|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tunel wyjście bajtów|Bajty|Średnia|Wychodzące bajty tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tunel bajtów wejściowych|Bajty|Średnia|Przychodzące bajty tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tunel wyjście pakietów|Licznik|Średnia|Wychodzące liczba pakietów tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tunel pakietów wejściowych|Licznik|Średnia|Liczba pakietów przychodząca tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Porzucanie pakietów niezgodność wyjście TS tunelu|Licznik|Średnia|Wychodzące Porzucanie pakietów z niezgodność selektor ruchu tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Porzucanie pakietów niezgodność ruch przychodzący serwera terminali tunelu|Licznik|Średnia|Przychodzące porzucenia pakietów z niezgodność selektor ruchu tunelu w pięciu minut|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|BytesIn|BytesIn|Licznik|Łącznie|Bajty ingressing Azure|Nie wymiarów|
|BytesOut|BytesOut|Licznik|Łącznie|Bajty egressing Azure|Nie wymiarów|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Średnia|Usługa BITS ingressing Azure na sekundę|Nie wymiarów|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Średnia|Usługa BITS egressing Azure na sekundę|Nie wymiarów|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|QpsByEndpoint|Zapytania przez punkt końcowy zwrócił|Licznik|Łącznie|Ile razy punkt końcowy Menedżera ruchu został zwrócony w określonym przedziale czasu|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Stan punktu końcowego przez punkt końcowy|Licznik|Maksymalna|1, jeśli punkt końcowy sondowania stanu jest "włączone", w przeciwnym razie 0.|EndpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Registration.all|Operacje rejestracji|Licznik|Łącznie|Liczba wszystkich operacji pomyślną rejestrację (tworzenie zapytań aktualizacji i usunięć). |Nie wymiarów|
|Registration.Create|Operacje tworzenia rejestracji|Licznik|Łącznie|Liczba wszystkich przypadków pomyślną rejestrację.|Nie wymiarów|
|Registration.Update|Operacje aktualizacji rejestracji|Licznik|Łącznie|Liczba wszystkich aktualizacji pomyślną rejestrację.|Nie wymiarów|
|Registration.Get|Operacje odczytu rejestracji|Licznik|Łącznie|Liczba wszystkich zapytań pomyślną rejestrację.|Nie wymiarów|
|Registration.delete|Operacje usuwania rejestracji|Licznik|Łącznie|Liczba wszystkich usunięć pomyślną rejestrację.|Nie wymiarów|
|przychodzące|Przychodzące komunikaty|Licznik|Łącznie|Liczba wszystkich powiodło się wysyłanie wywołań interfejsu API. |Nie wymiarów|
|Incoming.scheduled|Powiadomienia wypychane zaplanowane wysyłane|Licznik|Łącznie|Powiadomienia wypychane zaplanowane anulowane|Nie wymiarów|
|Incoming.scheduled.Cancel|Powiadomienia wypychane zaplanowane anulowane|Licznik|Łącznie|Powiadomienia wypychane zaplanowane anulowane|Nie wymiarów|
|scheduled.Pending|Oczekujące powiadomienia według harmonogramu|Licznik|Łącznie|Oczekujące powiadomienia według harmonogramu|Nie wymiarów|
|Installation.all|Operacje zarządzania instalacji|Licznik|Łącznie|Operacje zarządzania instalacji|Nie wymiarów|
|Installation.Get|Pobierz operacje instalacji|Licznik|Łącznie|Pobierz operacje instalacji|Nie wymiarów|
|Installation.UPSERT|Utwórz lub zaktualizuj operacje instalacji|Licznik|Łącznie|Utwórz lub zaktualizuj operacje instalacji|Nie wymiarów|
|Installation.Patch|Operacje instalacji poprawki|Licznik|Łącznie|Operacje instalacji poprawki|Nie wymiarów|
|Installation.delete|Usuwanie operacji instalacji|Licznik|Łącznie|Usuwanie operacji instalacji|Nie wymiarów|
|Outgoing.allpns.SUCCESS|Powiadomienia o pomyślnym|Licznik|Łącznie|Liczba wszystkich powiadomień powiodło się.|Nie wymiarów|
|Outgoing.allpns.invalidpayload|Błędy ładunku|Licznik|Łącznie|Liczba wypchnięć, których nie powiodła się, ponieważ systemu powiadomień platformy zwrócił błąd zły ładunku.|Nie wymiarów|
|Outgoing.allpns.pnserror|Zewnętrzne powiadomienia błędów systemu|Licznik|Łącznie|Liczba wypchnięć, których nie powiodła się, ponieważ wystąpił problem podczas komunikacji z systemem powiadomień platformy (z wykluczeniem problemom z uwierzytelnianiem).|Nie wymiarów|
|Outgoing.allpns.channelerror|Błędy kanału|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem, ponieważ kanał był nieprawidłowy nie są skojarzone z aplikacją prawidłowe ograniczenie lub wygasła.|Nie wymiarów|
|Outgoing.allpns.badorexpiredchannel|Błędy nieprawidłowych lub wygasłych kanałów|Licznik|Łącznie|Liczba wypycha zakończonych niepowodzeniem z powodu wygasły lub nieprawidłowy kanał/token/registrationId w rejestracji.|Nie wymiarów|
|Outgoing.wns.SUCCESS|Pomyślne powiadomień WNS|Licznik|Łącznie|Liczba wszystkich powiadomień powiodło się.|Nie wymiarów|
|Outgoing.wns.invalidcredentials|Błędy autoryzacji WNS (nieprawidłowe poświadczenia)|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptowała podanych poświadczeń lub poświadczenia są blokowane. (Windows Live nie rozpoznaje poświadczeń).|Nie wymiarów|
|Outgoing.wns.badchannel|Błąd kanału zły WNS|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ ChannelURI w rejestracji nie został rozpoznany (stanu usługi WNS: nie znaleziono 404).|Nie wymiarów|
|Outgoing.wns.expiredchannel|Błąd kanału wygasł WNS|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ wygasł ChannelURI (stan usługi WNS: 410 Gone).|Nie wymiarów|
|Outgoing.wns.throttled|WNS ograniczany powiadomienia|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ usługi WNS jest ograniczanie tej aplikacji (stan usługi WNS: 406 nie do przyjęcia).|Nie wymiarów|
|Outgoing.wns.tokenproviderunreachable|Błędy autoryzacji WNS (nieosiągalne)|Licznik|Łącznie|Windows Live jest nieosiągalny.|Nie wymiarów|
|Outgoing.wns.invalidtoken|Błędy autoryzacji WNS (nieprawidłowy Token)|Licznik|Łącznie|Token dostarczony do usługi WNS jest nieprawidłowy (stan usługi WNS: 401 nieautoryzowane).|Nie wymiarów|
|Outgoing.wns.wrongtoken|Błędy autoryzacji WNS (nieprawidłowy Token)|Licznik|Łącznie|Dostarczony do WNS token jest prawidłowy, ale dla innej aplikacji (stan usługi WNS: 403 Zabroniony). Może to nastąpić, jeśli ChannelURI w rejestracji jest skojarzony z innej aplikacji. Sprawdź, czy aplikacja kliencka jest skojarzony z tą samą aplikacją, którego poświadczenia znajdują się w Centrum powiadomień.|Nie wymiarów|
|Outgoing.wns.invalidnotificationformat|Format nieprawidłowy powiadomień WNS|Licznik|Łącznie|Format powiadomienia jest nieprawidłowy (stan usługi WNS: 400). Należy pamiętać, że usługi WNS nie odrzucić wszystkie ładunki nieprawidłowy.|Nie wymiarów|
|Outgoing.wns.invalidnotificationsize|Błąd nieprawidłowego rozmiaru powiadomienia usługi WNS|Licznik|Łącznie|Ładunek powiadomienia jest za duży (stan usługi WNS: 413).|Nie wymiarów|
|Outgoing.wns.channelthrottled|Ograniczenie kanału usługi WNS|Licznik|Łącznie|Powiadomienia został porzucony, ponieważ jest ograniczany ChannelURI w rejestracji (nagłówek odpowiedzi usługi WNS: X-WNS-NotificationStatus:channelThrottled).|Nie wymiarów|
|Outgoing.wns.channeldisconnected|Kanał WNS odłączona|Licznik|Łącznie|Powiadomienia został porzucony, ponieważ jest ograniczany ChannelURI w rejestracji (nagłówek odpowiedzi usługi WNS: DeviceConnectionStatus-X-WNS: odłączony).|Nie wymiarów|
|Outgoing.wns.dropped|Powiadomienia porzucone WNS|Licznik|Łącznie|Powiadomienia został porzucony, ponieważ jest ograniczany ChannelURI w rejestracji (X-WNS-NotificationStatus: porzucone, ale nie X-WNS-DeviceConnectionStatus: odłączony).|Nie wymiarów|
|Outgoing.wns.pnserror|Błędy WNS|Licznik|Łącznie|Powiadomienie nie są dostarczane z powodu błędów komunikacji z usługą WNS.|Nie wymiarów|
|Outgoing.wns.authenticationerror|Błędy uwierzytelniania usługi WNS|Licznik|Łącznie|Powiadomienie nie są dostarczane z powodu błędów komunikacji z usługi Windows Live nieprawidłowych poświadczeń lub nieprawidłowy token.|Nie wymiarów|
|Outgoing.apns.SUCCESS|Powiadomienia o pomyślnym APNS|Licznik|Łącznie|Liczba wszystkich powiadomień powiodło się.|Nie wymiarów|
|Outgoing.apns.invalidcredentials|Błędy autoryzacji APNS|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptowała podanych poświadczeń lub poświadczenia są blokowane.|Nie wymiarów|
|Outgoing.apns.badchannel|Błąd kanału zły APNS|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ token jest nieprawidłowy (kod stanu APNS: 8).|Nie wymiarów|
|Outgoing.apns.expiredchannel|APNS wygasł błąd kanału|Licznik|Łącznie|Liczba token, który został unieważniony przez kanał opinii APNS.|Nie wymiarów|
|Outgoing.apns.invalidnotificationsize|Błąd nieprawidłowego rozmiaru powiadomienia usługi APNS|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ ładunku jest zbyt duży (kod stanu APNS: 7).|Nie wymiarów|
|Outgoing.apns.pnserror|Błędy APNS|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem z powodu błędów komunikacji z usługą APNS.|Nie wymiarów|
|Outgoing.gcm.SUCCESS|Pomyślne powiadomienia usługi GCM|Licznik|Łącznie|Liczba wszystkich powiadomień powiodło się.|Nie wymiarów|
|Outgoing.gcm.invalidcredentials|Błędy autoryzacji GCM (nieprawidłowe poświadczenia)|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptowała podanych poświadczeń lub poświadczenia są blokowane.|Nie wymiarów|
|Outgoing.gcm.badchannel|Błąd usługi GCM zły kanału|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ registrationId w rejestracji nie został rozpoznany. (wynik GCM: nieprawidłowy rejestracji).|Nie wymiarów|
|Outgoing.gcm.expiredchannel|Błąd kanału wygasł usługi GCM|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ wygasła registrationId w rejestracji (usługi GCM wynik: NotRegistered).|Nie wymiarów|
|Outgoing.gcm.throttled|GCM ograniczany powiadomienia|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ GCM ograniczany tej aplikacji (kod stanu usługi GCM: 501 599 lub wynik: niedostępny).|Nie wymiarów|
|Outgoing.gcm.invalidnotificationformat|Format nieprawidłowy powiadomienia usługi GCM|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ ładunku nie został poprawnie sformatowany. (wynik GCM: InvalidDataKey lub InvalidTtl).|Nie wymiarów|
|Outgoing.gcm.invalidnotificationsize|Błąd nieprawidłowego rozmiaru powiadomienia usługi GCM|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ ładunku jest zbyt duży (wynik GCM: MessageTooBig).|Nie wymiarów|
|Outgoing.gcm.wrongchannel|Błąd usługi GCM niewłaściwego kanału|Licznik|Łącznie|Liczba wypchnięć, których nie powiodła się, ponieważ nie jest skojarzony z bieżącej aplikacji registrationId w rejestracji (usługi GCM wynik: InvalidPackageName).|Nie wymiarów|
|Outgoing.gcm.pnserror|Błędy usługi GCM|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem z powodu błędów komunikacji z usługą GCM.|Nie wymiarów|
|Outgoing.gcm.authenticationerror|Błędy uwierzytelniania usługi GCM|Licznik|Łącznie|Liczba wypchnięć, których nie powiodła się, ponieważ system powiadomień platformy nie akceptuje poświadczeń dostarczonych poświadczeń jest zablokowany lub SenderId nie została poprawnie skonfigurowana w aplikacji (wynik GCM: MismatchedSenderId).|Nie wymiarów|
|Outgoing.mpns.SUCCESS|Powiadomienia o pomyślnym usługi MPNS|Licznik|Łącznie|Liczba wszystkich powiadomień powiodło się.|Nie wymiarów|
|Outgoing.mpns.invalidcredentials|Nieprawidłowe poświadczenia usługi MPNS|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptowała podanych poświadczeń lub poświadczenia są blokowane.|Nie wymiarów|
|Outgoing.mpns.badchannel|Błąd kanału zły usługi MPNS|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ ChannelURI w rejestracji nie został rozpoznany (stan usługi MPNS: nie znaleziono 404).|Nie wymiarów|
|Outgoing.mpns.throttled|Usługi MPNS ograniczany powiadomienia|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ usługi MPNS jest ograniczanie tej aplikacji (WNS MPNS: 406 nie do przyjęcia).|Nie wymiarów|
|Outgoing.mpns.invalidnotificationformat|Format nieprawidłowy powiadomienia usługi MPNS|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem, ponieważ ładunek powiadomienia był za duży.|Nie wymiarów|
|Outgoing.mpns.channeldisconnected|Odłączony kanału usługi MPNS|Licznik|Łącznie|Liczba wypchnięcia, które zakończyły się niepowodzeniem, ponieważ ChannelURI w rejestracji została rozłączona (stan usługi MPNS: nie można odnaleźć 412).|Nie wymiarów|
|Outgoing.mpns.dropped|Usługi MPNS porzucony powiadomienia|Licznik|Łącznie|Liczba wypchnięć, które zostały porzucone przez usługi MPNS (nagłówek odpowiedzi usługi MPNS: X NotificationStatus: Kolejka zapełniona lub Suppressed).|Nie wymiarów|
|Outgoing.mpns.pnserror|Błędy usługi MPNS|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem z powodu błędów komunikacji z usługi MPNS.|Nie wymiarów|
|Outgoing.mpns.authenticationerror|Błędy uwierzytelniania usługi MPNS|Licznik|Łącznie|Liczba wypycha, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptowała podanych poświadczeń lub poświadczenia są blokowane.|Nie wymiarów|
|notificationhub.pushes|Wszystkie wychodzące powiadomienia|Licznik|Łącznie|Wszystkie wychodzące powiadomienia w Centrum powiadomień|Nie wymiarów|
|Incoming.all.Requests|Wszystkie żądania przychodzące|Licznik|Łącznie|Całkowita liczba żądań przychodzących do Centrum powiadomień|Nie wymiarów|
|Incoming.all.failedrequests|Wszystkich przychodzących żądań zakończonych niepowodzeniem|Licznik|Łącznie|Całkowita liczba nieudanych żądań przychodzących dla Centrum powiadomień|Nie wymiarów|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|SearchLatency|Czas oczekiwania wyszukiwania|Sekundy|Średnia|Opóźnienie średni wyszukiwania dla usługi wyszukiwania|Nie wymiarów|
|SearchQueriesPerSecond|Zapytania wyszukiwania na sekundę|CountPerSecond|Średnia|Zapytania wyszukiwania na sekundę dla usługi wyszukiwania|Nie wymiarów|
|ThrottledSearchQueriesPercentage|Procent zapytania wyszukiwania z ograniczeniem przepustowości|Procent|Średnia|Procent zapytania wyszukiwania, które zostały ograniczany usługi wyszukiwania|Nie wymiarów|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CPUXNS|Użycie procesora CPU na przestrzeń nazw|Procent|Maksymalna|Przestrzeń nazw Procesora użycia metryki dla usługi magistrali — wersja premium|Nie wymiarów|
|WSXNS|Użycie rozmiaru pamięci na przestrzeń nazw|Procent|Maksymalna|Metryki użycia pamięci przestrzeni nazw w premium magistrali usługi|Nie wymiarów|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent użycia procesora CPU|Procent|Średnia|Procent użycia procesora CPU|Nie wymiarów|
|physical_data_read_percent|Procent użycia operacji we/wy na danych|Procent|Średnia|Procent użycia operacji we/wy na danych|Nie wymiarów|
|log_write_percent|Procent we/wy dziennika|Procent|Średnia|Procent we/wy dziennika|Nie wymiarów|
|dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średnia|Procent użycia jednostek DTU|Nie wymiarów|
|Magazyn|Rozmiar całkowitą bazy danych|Bajty|Maksymalna|Rozmiar całkowitą bazy danych|Nie wymiarów|
|connection_successful|Udane połączenia|Licznik|Łącznie|Udane połączenia|Nie wymiarów|
|connection_failed|Połączenia nie powiodło się|Licznik|Łącznie|Połączenia nie powiodło się|Nie wymiarów|
|blocked_by_firewall|Blokowane przez zaporę|Licznik|Łącznie|Blokowane przez zaporę|Nie wymiarów|
|Zakleszczenie|Zakleszczenie|Licznik|Łącznie|Zakleszczenie|Nie wymiarów|
|storage_percent|Procent użycia rozmiaru bazy danych|Procent|Maksymalna|Procent użycia rozmiaru bazy danych|Nie wymiarów|
|xtp_storage_percent|Procent magazynu OLTP w pamięci|Procent|Średnia|Procent magazynu OLTP w pamięci|Nie wymiarów|
|workers_percent|Procent pracowników|Procent|Średnia|Procent pracowników|Nie wymiarów|
|sessions_percent|Procent sesji|Procent|Średnia|Procent sesji|Nie wymiarów|
|dtu_limit|Limit jednostek dtu w warstwie|Licznik|Średnia|Limit jednostek dtu w warstwie|Nie wymiarów|
|dtu_used|Jednostek dtu w warstwie używane|Licznik|Średnia|Jednostek dtu w warstwie używane|Nie wymiarów|
|dwu_limit|Jednostka DWU limit|Licznik|Maksymalna|Jednostka DWU limit|Nie wymiarów|
|dwu_consumption_percent|Procent jednostka DWU|Procent|Maksymalna|Procent jednostka DWU|Nie wymiarów|
|dwu_used|Jednostka DWU używane|Licznik|Maksymalna|Jednostka DWU używane|Nie wymiarów|
|dw_cpu_percent|Poziom węzła DW procent użycia procesora CPU|Procent|Średnia|Poziom węzła DW procent użycia procesora CPU|dw_logical_node_id|
|dw_physical_data_read_percent|Procent dane we/wy poziomu węzła magazynu danych|Procent|Średnia|Procent dane we/wy poziomu węzła magazynu danych|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent użycia procesora CPU|Procent|Średnia|Procent użycia procesora CPU|Nie wymiarów|
|database_cpu_percent|Procent użycia procesora CPU|Procent|Średnia|Procent użycia procesora CPU|DatabaseResourceId|
|physical_data_read_percent|Procent użycia operacji we/wy na danych|Procent|Średnia|Procent użycia operacji we/wy na danych|Nie wymiarów|
|database_physical_data_read_percent|Procent użycia operacji we/wy na danych|Procent|Średnia|Procent użycia operacji we/wy na danych|DatabaseResourceId|
|log_write_percent|Procent we/wy dziennika|Procent|Średnia|Procent we/wy dziennika|Nie wymiarów|
|database_log_write_percent|Procent we/wy dziennika|Procent|Średnia|Procent we/wy dziennika|DatabaseResourceId|
|dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średnia|Procent użycia jednostek DTU|Nie wymiarów|
|database_dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średnia|Procent użycia jednostek DTU|DatabaseResourceId|
|storage_percent|Procent użycia magazynu|Procent|Średnia|Procent użycia magazynu|Nie wymiarów|
|workers_percent|Procent pracowników|Procent|Średnia|Procent pracowników|Nie wymiarów|
|database_workers_percent|Procent pracowników|Procent|Średnia|Procent pracowników|DatabaseResourceId|
|sessions_percent|Procent sesji|Procent|Średnia|Procent sesji|Nie wymiarów|
|database_sessions_percent|Procent sesji|Procent|Średnia|Procent sesji|DatabaseResourceId|
|eDTU_limit|limit liczby jednostek eDTU|Licznik|Średnia|limit liczby jednostek eDTU|Nie wymiarów|
|storage_limit|Limit magazynu|Bajty|Średnia|Limit magazynu|Nie wymiarów|
|eDTU_used|eDTU używane|Licznik|Średnia|eDTU używane|Nie wymiarów|
|storage_used|Użyty magazyn|Bajty|Średnia|Użyty magazyn|Nie wymiarów|
|database_storage_used|Użyty magazyn|Bajty|Średnia|Użyty magazyn|DatabaseResourceId|
|xtp_storage_percent|Procent magazynu OLTP w pamięci|Procent|Średnia|Procent magazynu OLTP w pamięci|Nie wymiarów|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średnia|Procent użycia jednostek DTU|ElasticPoolResourceId|
|database_dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średnia|Procent użycia jednostek DTU|DatabaseResourceId, ElasticPoolResourceId|
|storage_used|Użyty magazyn|Bajty|Średnia|Użyty magazyn|ElasticPoolResourceId|
|database_storage_used|Użyty magazyn|Bajty|Średnia|Użyty magazyn|DatabaseResourceId, ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|UsedCapacity|Używane pojemności|Bajty|Średnia|Konto używane pojemności|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|w milisekundach|Średnia|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|w milisekundach|Średnia|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średnia|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|BlobCapacity|Pojemność obiektów blob|Bajty|Średnia|Ilość miejsca w magazynie używane przez usługę obiektu Blob na koncie magazynu w bajtach.|BlobType|
|Liczba obiektów blob|Liczba obiektów blob|Licznik|Średnia|Liczba obiektów Blob w usłudze obiektów Blob na koncie magazynu.|BlobType|
|ContainerCount|Liczba kontenera obiektów blob|Licznik|Średnia|Liczba kontenerów w usłudze obiektów Blob na koncie magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|w milisekundach|Średnia|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|w milisekundach|Średnia|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średnia|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TableCapacity|Pojemność tabeli|Bajty|Średnia|Ilość miejsca w magazynie używane przez usługę tabeli konta magazynu w bajtach.|Nie wymiarów|
|TableCount|Liczba tabeli|Licznik|Średnia|Liczba tabel w usłudze tabel na koncie magazynu.|Nie wymiarów|
|TableEntityCount|Liczba jednostek tabeli|Licznik|Średnia|Liczba jednostek tabeli w usłudze tabel na koncie magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|w milisekundach|Średnia|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|w milisekundach|Średnia|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średnia|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|QueueCapacity|Pojemność kolejki|Bajty|Średnia|Ilość miejsca w magazynie używane przez usługę kolejki konta magazynu w bajtach.|Nie wymiarów|
|QueueCount|Liczba w kolejce|Licznik|Średnia|Liczba kolejki w usłudze kolejki konta magazynu.|Nie wymiarów|
|QueueMessageCount|Liczba wiadomości w kolejce|Licznik|Średnia|Przybliżoną liczbę wiadomości w kolejce w usłudze kolejki konta magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|w milisekundach|Średnia|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|w milisekundach|Średnia|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średnia|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|FileCapacity|Wydajność pliku|Bajty|Średnia|Ilość miejsca w magazynie używane przez usługę pliku konta magazynu w bajtach.|Nie wymiarów|
|FileCount|Liczba plików|Licznik|Średnia|Liczba plików w usłudze pliku konta magazynu.|Nie wymiarów|
|FileShareCount|Liczba udziału plików|Licznik|Średnia|Liczba plików udziałów w usługa plików konta magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|w milisekundach|Średnia|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|w milisekundach|Średnia|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średnia|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|ResourceUtilization|SU % wykorzystania|Procent|Maksymalna|SU % wykorzystania|Nie wymiarów|
|InputEvents|Zdarzenia wejściowe|Licznik|Łącznie|Zdarzenia wejściowe|Nie wymiarów|
|InputEventBytes|Zdarzenie wejściowe w bajtach|Bajty|Łącznie|Zdarzenie wejściowe w bajtach|Nie wymiarów|
|LateInputEvents|Opóźnione zdarzenia wejściowe|Licznik|Łącznie|Opóźnione zdarzenia wejściowe|Nie wymiarów|
|OutputEvents|Dane wyjściowe zdarzenia|Licznik|Łącznie|Dane wyjściowe zdarzenia|Nie wymiarów|
|ConversionErrors|Błędy konwersji danych|Licznik|Łącznie|Błędy konwersji danych|Nie wymiarów|
|Błędy|Błędy środowiska wykonawczego|Licznik|Łącznie|Błędy środowiska wykonawczego|Nie wymiarów|
|DroppedOrAdjustedEvents|Zdarzenia poza kolejnością|Licznik|Łącznie|Zdarzenia poza kolejnością|Nie wymiarów|
|AMLCalloutRequests|Funkcja żądania|Licznik|Łącznie|Funkcja żądania|Nie wymiarów|
|AMLCalloutFailedRequests|Żądania funkcji zakończonych niepowodzeniem|Licznik|Łącznie|Żądania funkcji zakończonych niepowodzeniem|Nie wymiarów|
|AMLCalloutInputEvents|Zdarzenia — funkcja|Licznik|Łącznie|Zdarzenia — funkcja|Nie wymiarów|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CpuPercentage|Procent użycia procesora CPU|Procent|Średnia|Procent użycia procesora CPU|Wystąpienie|
|MemoryPercentage|Procent pamięci|Procent|Średnia|Procent pamięci|Wystąpienie|
|DiskQueueLength|Długość kolejki dysku|Licznik|Łącznie|Długość kolejki dysku|Wystąpienie|
|HttpQueueLength|Długość kolejki http|Licznik|Łącznie|Długość kolejki http|Wystąpienie|
|BytesReceived|Dane w|Bajty|Łącznie|Dane w|Wystąpienie|
|Żądania|Dla danych wychodzących|Bajty|Łącznie|Dla danych wychodzących|Wystąpienie|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (z wyjątkiem funkcji)

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CpuTime|Czas procesora CPU|Sekundy|Łącznie|Czas procesora CPU|Wystąpienie|
|Żądania|Żądania|Licznik|Łącznie|Żądania|Wystąpienie|
|BytesReceived|Dane w|Bajty|Łącznie|Dane w|Wystąpienie|
|Żądania|Dla danych wychodzących|Bajty|Łącznie|Dla danych wychodzących|Wystąpienie|
|Http101|HTTP 101|Licznik|Łącznie|HTTP 101|Wystąpienie|
|Http2xx|Http 2xx|Licznik|Łącznie|Http 2xx|Wystąpienie|
|Http3xx|Http 3xx|Licznik|Łącznie|Http 3xx|Wystąpienie|
|Http401|HTTP 401|Licznik|Łącznie|HTTP 401|Wystąpienie|
|Http403|HTTP 403|Licznik|Łącznie|HTTP 403|Wystąpienie|
|Http404|HTTP 404|Licznik|Łącznie|HTTP 404|Wystąpienie|
|Http406|HTTP 406|Licznik|Łącznie|HTTP 406|Wystąpienie|
|Http4xx|Http 4xx|Licznik|Łącznie|Http 4xx|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajty|Średnia|Zestaw roboczy pamięci|Wystąpienie|
|AverageMemoryWorkingSet|Pamięć średni zestaw roboczy|Bajty|Średnia|Pamięć średni zestaw roboczy|Wystąpienie|
|AverageResponseTime|Średni czas odpowiedzi|Sekundy|Średnia|Średni czas odpowiedzi|Wystąpienie|
|FunctionExecutionUnits|Funkcja wykonywania jednostki|Licznik|Średnia|Funkcja wykonywania jednostki|Wystąpienie|
|FunctionExecutionCount|Liczba wykonań — funkcja|Licznik|Średnia|Liczba wykonań — funkcja|Wystąpienie|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funkcje)

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|BytesReceived|Dane w|Bajty|Łącznie|Dane w|Wystąpienie|
|Żądania|Dla danych wychodzących|Bajty|Łącznie|Dla danych wychodzących|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajty|Średnia|Zestaw roboczy pamięci|Wystąpienie|
|AverageMemoryWorkingSet|Pamięć średni zestaw roboczy|Bajty|Średnia|Pamięć średni zestaw roboczy|Wystąpienie|
|FunctionExecutionUnits|Funkcja wykonywania jednostki|Licznik|Średnia|Funkcja wykonywania jednostki|Wystąpienie|
|FunctionExecutionCount|Liczba wykonań — funkcja|Licznik|Średnia|Liczba wykonań — funkcja|Wystąpienie|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CpuTime|Czas procesora CPU|Sekundy|Łącznie|Czas procesora CPU|Wystąpienie|
|Żądania|Żądania|Licznik|Łącznie|Żądania|Wystąpienie|
|BytesReceived|Dane w|Bajty|Łącznie|Dane w|Wystąpienie|
|Żądania|Dla danych wychodzących|Bajty|Łącznie|Dla danych wychodzących|Wystąpienie|
|Http101|HTTP 101|Licznik|Łącznie|HTTP 101|Wystąpienie|
|Http2xx|Http 2xx|Licznik|Łącznie|Http 2xx|Wystąpienie|
|Http3xx|Http 3xx|Licznik|Łącznie|Http 3xx|Wystąpienie|
|Http401|HTTP 401|Licznik|Łącznie|HTTP 401|Wystąpienie|
|Http403|HTTP 403|Licznik|Łącznie|HTTP 403|Wystąpienie|
|Http404|HTTP 404|Licznik|Łącznie|HTTP 404|Wystąpienie|
|Http406|HTTP 406|Licznik|Łącznie|HTTP 406|Wystąpienie|
|Http4xx|Http 4xx|Licznik|Łącznie|Http 4xx|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajty|Średnia|Zestaw roboczy pamięci|Wystąpienie|
|AverageMemoryWorkingSet|Pamięć średni zestaw roboczy|Bajty|Średnia|Pamięć średni zestaw roboczy|Wystąpienie|
|AverageResponseTime|Średni czas odpowiedzi|Sekundy|Średnia|Średni czas odpowiedzi|Wystąpienie|
|FunctionExecutionUnits|Funkcja wykonywania jednostki|Licznik|Średnia|Funkcja wykonywania jednostki|Wystąpienie|
|FunctionExecutionCount|Liczba wykonań — funkcja|Licznik|Średnia|Liczba wykonań — funkcja|Wystąpienie|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Żądania|Żądania|Licznik|Łącznie|Żądania|Wystąpienie|
|BytesReceived|Dane w|Bajty|Łącznie|Dane w|Wystąpienie|
|Żądania|Dla danych wychodzących|Bajty|Łącznie|Dla danych wychodzących|Wystąpienie|
|Http101|HTTP 101|Licznik|Łącznie|HTTP 101|Wystąpienie|
|Http2xx|Http 2xx|Licznik|Łącznie|Http 2xx|Wystąpienie|
|Http3xx|Http 3xx|Licznik|Łącznie|Http 3xx|Wystąpienie|
|Http401|HTTP 401|Licznik|Łącznie|HTTP 401|Wystąpienie|
|Http403|HTTP 403|Licznik|Łącznie|HTTP 403|Wystąpienie|
|Http404|HTTP 404|Licznik|Łącznie|HTTP 404|Wystąpienie|
|Http406|HTTP 406|Licznik|Łącznie|HTTP 406|Wystąpienie|
|Http4xx|Http 4xx|Licznik|Łącznie|Http 4xx|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|AverageResponseTime|Średni czas odpowiedzi|Sekundy|Średnia|Średni czas odpowiedzi|Wystąpienie|
|CpuPercentage|Procent użycia procesora CPU|Procent|Średnia|Procent użycia procesora CPU|Wystąpienie|
|MemoryPercentage|Procent pamięci|Procent|Średnia|Procent pamięci|Wystąpienie|
|DiskQueueLength|Długość kolejki dysku|Licznik|Łącznie|Długość kolejki dysku|Wystąpienie|
|HttpQueueLength|Długość kolejki http|Licznik|Łącznie|Długość kolejki http|Wystąpienie|
|ActiveRequests|Aktywne żądania|Licznik|Łącznie|Aktywne żądania|Wystąpienie|
|TotalFrontEnds|Całkowita liczba przedniego kończy się|Licznik|Średnia|Całkowita liczba przedniego kończy się|Wystąpienie|
|SmallAppServicePlanInstances|Pracowników planu usługi aplikacji — małe|Licznik|Średnia|Pracowników planu usługi aplikacji — małe|Wystąpienie|
|MediumAppServicePlanInstances|Pracownicy planu usługi aplikacji — średnia|Licznik|Średnia|Pracownicy planu usługi aplikacji — średnia|Wystąpienie|
|LargeAppServicePlanInstances|Pracownicy planu usługi aplikacji — duże|Licznik|Średnia|Pracownicy planu usługi aplikacji — duże|Wystąpienie|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Łączna|Całkowita liczba procesów roboczych|Licznik|Średnia|Całkowita liczba procesów roboczych|Wystąpienie|
|WorkersAvailable|Dostępne pracowników|Licznik|Średnia|Dostępne pracowników|Wystąpienie|
|WorkersUsed|Używane pracowników|Licznik|Średnia|Używane pracowników|Wystąpienie|

## <a name="next-steps"></a>Następne kroki
* [Przeczytaj informacje o metryki w monitorze Azure](monitoring-overview-metrics.md)
* [Utwórz alerty na metryk](insights-receive-alert-notifications.md)
* [Eksportuj metryk do magazynu, Centrum zdarzeń lub analizy dzienników](monitoring-overview-of-diagnostic-logs.md)
