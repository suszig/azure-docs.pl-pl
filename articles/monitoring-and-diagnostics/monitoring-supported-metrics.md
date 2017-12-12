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
ms.openlocfilehash: f678bba89adf1450bd6a08909fdad51424a210e8
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Obsługiwane metryki z monitorem Azure
Azure Monitor udostępnia kilka metod do interakcji z metryk, takich jak wykresy je w portalu, dostępu do nich za pośrednictwem interfejsu API REST lub zapytań je przy użyciu programu PowerShell lub interfejsu wiersza polecenia. Poniżej przedstawiono pełną listę wszystkich metryki obecnie z potoku metryki Azure monitora.

> [!NOTE]
> Inne metryki mogą być dostępne w portalu lub przy użyciu starszej wersji interfejsów API. Ta lista zawiera tylko metryk dostępnych za pośrednictwem potoku metryki skonsolidowanych Azure Monitor. Wykonanie kwerendy i dostępu do metryki z wymiarów, użyj [wersja interfejsu api 2017-05-01-preview](https://docs.microsoft.com/rest/api/monitor/metricdefinitions)
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|qpu_metric|QPU|Licznik|Średni|QPU. Zakres 0-100 S1, 0 – 200 S2 i 0-400 dla S4|Nie wymiarów|
|memory_metric|Memory (Pamięć)|Bajty|Średni|Pamięci. W zakresie 0 – 25 GB na S1, 0 – 50 GB na S2 i 0 – 100 GB dla S4|Nie wymiarów|
|TotalConnectionRequests|Całkowita liczba połączeń żądań|Licznik|Średni|Całkowita liczba połączeń żądania. Są to przyjęcia.|Nie wymiarów|
|SuccessfullConnectionsPerSec|Udane połączenia na sekundę|CountPerSecond|Średni|Szybkość zakończeń udane połączenie.|Nie wymiarów|
|TotalConnectionFailures|Całkowita liczba awarii|Licznik|Średni|Całkowita liczba nieudanych prób połączenia.|Nie wymiarów|
|CurrentUserSessions|Bieżące sesje użytkowników|Licznik|Średni|Bieżąca liczba ustanowionych sesji użytkownika.|Nie wymiarów|
|QueryPoolBusyThreads|Zapytanie z puli wątków zajęty|Licznik|Średni|Liczba zajętych wątków w puli wątków zapytania.|Nie wymiarów|
|CommandPoolJobQueueLength|Długość kolejki zadań puli polecenia|Licznik|Średni|Liczba zadań w kolejce polecenia puli wątków.|Nie wymiarów|
|ProcessingPoolJobQueueLength|Długość kolejki zadań przetwarzania w puli|Licznik|Średni|Liczba zadań z systemem innym niż — I/O kolejki puli wątków przetwarzania.|Nie wymiarów|
|CurrentConnections|Połączenia: Bieżąca liczba połączeń|Licznik|Średni|Bieżąca liczba połączeń klienta.|Nie wymiarów|
|CleanerCurrentPrice|Pamięci: Czyszczący bieżąca cena|Licznik|Średni|Bieżąca cena pamięci $/ byte/godzina znormalizowane do 1000.|Nie wymiarów|
|CleanerMemoryShrinkable|Pamięci: Zmniejszania pamięci czyszczący|Bajty|Średni|Ilość pamięci w bajtach, mogą ulec czyszcząca przeczyszczanie w tle.|Nie wymiarów|
|CleanerMemoryNonshrinkable|Pamięci: Czyszczący nonshrinkable pamięci|Bajty|Średni|Ilość pamięci w bajtach, nie może ulec czyszcząca przeczyszczanie w tle.|Nie wymiarów|
|MemoryUsage|Pamięci: Użycie pamięci|Bajty|Średni|Użycie pamięci przez proces serwera jako używaną przy obliczaniu czyszczący cena pamięci. Taki sam jak licznika Process\PrivateBytes powiększony o rozmiar danych mapowanych na pamięć, ignorując wszystkie pamięci, która została zamapowana lub przydzielone przez aparat analizy w pamięci xVelocity (VertiPaq) przekracza Limit pamięci silnik xVelocity.|Nie wymiarów|
|MemoryLimitHard|Pamięci: Twarde Limit pamięci|Bajty|Średni|Limit pamięci twardym z pliku konfiguracji.|Nie wymiarów|
|MemoryLimitHigh|Pamięć: Wysoki Limit pamięci|Bajty|Średni|Limit pamięci wysokiej z pliku konfiguracji.|Nie wymiarów|
|MemoryLimitLow|Pamięci: Niski Limit pamięci|Bajty|Średni|Limit pamięci z pliku konfiguracji.|Nie wymiarów|
|MemoryLimitVertiPaq|Pamięci: VertiPaq Limit pamięci|Bajty|Średni|Limit w pamięci z pliku konfiguracji.|Nie wymiarów|
|Przydział|Pamięć: limit przydziału|Bajty|Średni|Limit przydziału pamięci, w bajtach. Limit przydziału pamięci jest nazywana rezerwacji pamięci grant lub pamięci.|Nie wymiarów|
|QuotaBlocked|Pamięci: Zablokowane przydziału|Licznik|Średni|Bieżąca liczba żądań przydziału, które są zablokowane, dopóki nie są zwalniane innych przydziałów pamięci.|Nie wymiarów|
|VertiPaqNonpaged|Pamięci: Niestronicowana VertiPaq|Bajty|Średni|Zablokowane bajtów pamięci zestawu do użycia przez aparat w pamięci roboczego.|Nie wymiarów|
|VertiPaqPaged|Pamięci: Stronicowanej VertiPaq|Bajty|Średni|Liczba bajtów stronicowanej pamięci dla danych w pamięci.|Nie wymiarów|
|RowsReadPerSec|Przetwarzanie: Wiersze do odczytu na sekundę|CountPerSecond|Średni|Liczba wierszy odczytywać wszystkie relacyjnych baz danych.|Nie wymiarów|
|RowsConvertedPerSec|Przetwarzanie: Wierszy przekonwertowane na sekundę|CountPerSecond|Średni|Liczba wierszy przekonwertować podczas przetwarzania.|Nie wymiarów|
|RowsWrittenPerSec|Przetwarzanie: Wierszy, zapisany na sekundę|CountPerSecond|Średni|Liczba wierszy, zapisany podczas przetwarzania.|Nie wymiarów|
|CommandPoolBusyThreads|Wątki: Polecenie zajęty z puli wątków|Licznik|Średni|Liczba zajętych wątków w puli wątków polecenia.|Nie wymiarów|
|CommandPoolIdleThreads|Wątków: Polecenie puli wątków bezczynności|Licznik|Średni|Liczba bezczynności wątków w puli wątków polecenia.|Nie wymiarów|
|LongParsingBusyThreads|Wątków: Podczas analizowania zajęty wątków Long|Licznik|Średni|Liczba zajętych wątków w puli wątków długo analizy.|Nie wymiarów|
|LongParsingIdleThreads|Wątków: Podczas analizowania bezczynności wątków Long|Licznik|Średni|Liczba bezczynności wątków w puli wątków długo analizy.|Nie wymiarów|
|LongParsingJobQueueLength|Wątki: Czas analizowania długość kolejki zadań|Licznik|Średni|Liczba zadań w kolejce długo analizy puli wątków.|Nie wymiarów|
|ProcessingPoolBusyIOJobThreads|Wątki: Przetwarzania puli wątków zajęty zadania we/wy|Licznik|Średni|Liczba wątków uruchomionych zadań we/wy w puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolBusyNonIOThreads|Wątków: Zajęty z puli wątków nie I/O przetwarzania|Licznik|Średni|Liczba wątków uruchomionych zadań nie I/O w puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolIOJobQueueLength|Wątki: Przetwarzanie puli długość kolejki zadania we/wy|Licznik|Średni|Liczba operacji We/Wy zadań w kolejce puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolIdleIOJobThreads|Wątki: Przetwarzania puli wątków bezczynności zadania we/wy|Licznik|Średni|Liczba wątków bezczynności zadań we/wy w puli wątków przetwarzania.|Nie wymiarów|
|ProcessingPoolIdleNonIOThreads|Wątki: Przetwarzanie bezczynności z puli wątków nie I/E|Licznik|Średni|Liczba bezczynności wątków w puli wątków przetwarzania dedykowane dla zadań innych niż — I/O.|Nie wymiarów|
|QueryPoolIdleThreads|Wątków: Wątki bezczynności puli zapytania|Licznik|Średni|Liczba wątków bezczynności zadań we/wy w puli wątków przetwarzania.|Nie wymiarów|
|QueryPoolJobQueueLength|Wątków: Lengt kolejki zadań puli zapytania|Licznik|Średni|Liczba zadań w kolejce zapytań puli wątków.|Nie wymiarów|
|ShortParsingBusyThreads|Wątków: Podczas analizowania zajęty wątków krótkiej|Licznik|Średni|Liczba wątków zajęty w krótkim analizy puli wątków.|Nie wymiarów|
|ShortParsingIdleThreads|Wątków: Podczas analizowania bezczynności wątków krótkiej|Licznik|Średni|Liczba wątków bezczynności w krótkim analizy puli wątków.|Nie wymiarów|
|ShortParsingJobQueueLength|Wątków: Podczas analizowania długość kolejki zadań krótkiej|Licznik|Średni|Liczba zadań w kolejce krótkich analizy puli wątków.|Nie wymiarów|
|memory_thrashing_metric|Wykonywania niepotrzebnych replik pamięci|Procent|Średni|Średnia pamięci wykonywania niepotrzebnych replik.|Nie wymiarów|
|mashup_engine_qpu_metric|Aparat M QPU|Licznik|Średni|Użycie QPU przez procesy wyszukiwarek zestawu połączonych danych|Nie wymiarów|
|mashup_engine_memory_metric|M aparat pamięci|Bajty|Średni|Użycie pamięci przez procesy wyszukiwarek zestawu połączonych danych|Nie wymiarów|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalRequests|Brama całkowita liczba żądań|Licznik|Łącznie|Liczba żądań bramy|Lokalizacja, nazwy hosta|
|SuccessfulRequests|Bramy pomyślnych żądań|Licznik|Łącznie|Liczba żądań pomyślnie bramy|Lokalizacja, nazwy hosta|
|UnauthorizedRequests|Brama nieautoryzowanych żądań|Licznik|Łącznie|Liczba żądań nieautoryzowanych bramy|Lokalizacja, nazwy hosta|
|FailedRequests|Żądań zakończonych niepowodzeniem bramy|Licznik|Łącznie|Liczba błędów w żądaniach bramy|Lokalizacja, nazwy hosta|
|OtherRequests|Inne żądania bramy|Licznik|Łącznie|Liczba innych żądań bramy|Lokalizacja, nazwy hosta|
|Czas trwania|Łączny czas realizacji żądania bramy|Milisekundy|Średni|Ogólny czas trwania z bramy żądań w milisekundach|Lokalizacja, nazwy hosta|
|Pojemność|Wydajność (wersja zapoznawcza)|Procent|Maksimum|Metryki użycia usługi ApiManagement|Lokalizacja|

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
|connectedclients|Połączeni klienci|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed|Łączna liczba operacji|Licznik|Łącznie||Nie wymiarów|
|Trafienia w pamięci podręcznej|Trafienia w pamięci podręcznej|Licznik|Łącznie||Nie wymiarów|
|cachemisses|Chybienia w pamięci podręcznej|Licznik|Łącznie||Nie wymiarów|
|getcommands|Pobrania|Licznik|Łącznie||Nie wymiarów|
|setcommands|Zestawy|Licznik|Łącznie||Nie wymiarów|
|evictedkeys|Wykluczone klucze|Licznik|Łącznie||Nie wymiarów|
|totalkeys|Całkowita liczba kluczy|Licznik|Maksimum||Nie wymiarów|
|expiredkeys|Wygasłe klucze|Licznik|Łącznie||Nie wymiarów|
|usedmemory|Użyta pamięć|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss|Używana pamięć RSS|Bajty|Maksimum||Nie wymiarów|
|serverLoad|Obciążenie serwera|Procent|Maksimum||Nie wymiarów|
|cacheWrite|Zapis w pamięci podręcznej|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead|Odczyt pamięci podręcznej|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime|Procesor CPU|Procent|Maksimum||Nie wymiarów|
|connectedclients0|Podłączeni klienci (niezależnego fragmentu 0)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed0|Całkowita liczba operacji (identyfikator niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|cachehits0|Trafień w pamięci podręcznej (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|cachemisses0|Chybienia pamięci podręcznej (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|getcommands0|Pobiera (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|setcommands0|Zestawy (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys0|Klucze wykluczonym (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|totalkeys0|Wszystkie klucze (niezależnego fragmentu 0)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys0|Wygasłe kluczy (niezależnego fragmentu 0)|Licznik|Łącznie||Nie wymiarów|
|usedmemory0|Używana pamięć (niezależnego fragmentu 0)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss0|Używana pamięć RSS (niezależnego fragmentu 0)|Bajty|Maksimum||Nie wymiarów|
|serverLoad0|Obciążenia serwera (niezależnego fragmentu 0)|Procent|Maksimum||Nie wymiarów|
|cacheWrite0|Pamięć podręczna zapisu (niezależnego fragmentu 0)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead0|Odczytu pamięci podręcznej (niezależnego fragmentu 0)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime0|Procesor (niezależnego fragmentu 0)|Procent|Maksimum||Nie wymiarów|
|connectedclients1|Podłączeni klienci (niezależnego fragmentu 1)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed1|Całkowita liczba operacji (identyfikator niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|cachehits1|Trafień w pamięci podręcznej (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|cachemisses1|Chybienia pamięci podręcznej (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|getcommands1|Pobiera (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|setcommands1|Zestawy (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys1|Klucze wykluczonym (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|totalkeys1|Wszystkie klucze (niezależnego fragmentu 1)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys1|Wygasłe kluczy (niezależnego fragmentu 1)|Licznik|Łącznie||Nie wymiarów|
|usedmemory1|Używana pamięć (niezależnego fragmentu 1)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss1|Używana pamięć RSS (niezależnego fragmentu 1)|Bajty|Maksimum||Nie wymiarów|
|serverLoad1|Obciążenia serwera (niezależnego fragmentu 1)|Procent|Maksimum||Nie wymiarów|
|cacheWrite1|Pamięć podręczna zapisu (niezależnego fragmentu 1)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead1|Odczytu pamięci podręcznej (niezależnego fragmentu 1)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime1|Procesor (niezależnego fragmentu 1)|Procent|Maksimum||Nie wymiarów|
|connectedclients2|Podłączeni klienci (niezależnego fragmentu 2)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed2|Całkowita liczba operacji (identyfikator niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|cachehits2|Trafień w pamięci podręcznej (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|cachemisses2|Chybienia pamięci podręcznej (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|getcommands2|Pobiera (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|setcommands2|Zestawy (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys2|Klucze wykluczonym (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|totalkeys2|Wszystkie klucze (niezależnego fragmentu 2)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys2|Wygasłe kluczy (niezależnego fragmentu 2)|Licznik|Łącznie||Nie wymiarów|
|usedmemory2|Używana pamięć (niezależnego fragmentu 2)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss2|Używana pamięć RSS (niezależnego fragmentu 2)|Bajty|Maksimum||Nie wymiarów|
|serverLoad2|Obciążenia serwera (niezależnego fragmentu 2)|Procent|Maksimum||Nie wymiarów|
|cacheWrite2|Pamięć podręczna zapisu (niezależnego fragmentu 2)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead2|Odczytu pamięci podręcznej (niezależnego fragmentu 2)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime2|Procesor (niezależnego fragmentu 2)|Procent|Maksimum||Nie wymiarów|
|connectedclients3|Podłączeni klienci (niezależnego fragmentu 3)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed3|Całkowita liczba operacji (identyfikator niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|cachehits3|Trafień w pamięci podręcznej (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|cachemisses3|Chybienia pamięci podręcznej (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|getcommands3|Pobiera (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|setcommands3|Zestawy (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys3|Klucze wykluczonym (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|totalkeys3|Wszystkie klucze (niezależnego fragmentu 3)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys3|Wygasłe kluczy (niezależnego fragmentu 3)|Licznik|Łącznie||Nie wymiarów|
|usedmemory3|Używana pamięć (niezależnego fragmentu 3)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss3|Używana pamięć RSS (niezależnego fragmentu 3)|Bajty|Maksimum||Nie wymiarów|
|serverLoad3|Obciążenia serwera (niezależnego fragmentu 3)|Procent|Maksimum||Nie wymiarów|
|cacheWrite3|Pamięć podręczna zapisu (niezależnego fragmentu 3)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead3|Odczytu pamięci podręcznej (niezależnego fragmentu 3)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime3|Procesor (niezależnego fragmentu 3)|Procent|Maksimum||Nie wymiarów|
|connectedclients4|Podłączeni klienci (niezależnego fragmentu 4)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed4|Całkowita liczba operacji (identyfikator niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|cachehits4|Trafień w pamięci podręcznej (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|cachemisses4|Chybienia pamięci podręcznej (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|getcommands4|Pobiera (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|setcommands4|Zestawy (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys4|Klucze wykluczonym (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|totalkeys4|Wszystkie klucze (niezależnego fragmentu 4)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys4|Wygasłe kluczy (niezależnego fragmentu 4)|Licznik|Łącznie||Nie wymiarów|
|usedmemory4|Używana pamięć (niezależnego fragmentu 4)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss4|Używana pamięć RSS (niezależnego fragmentu 4)|Bajty|Maksimum||Nie wymiarów|
|serverLoad4|Obciążenia serwera (niezależnego fragmentu 4)|Procent|Maksimum||Nie wymiarów|
|cacheWrite4|Pamięć podręczna zapisu (niezależnego fragmentu 4)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead4|Odczytu pamięci podręcznej (niezależnego fragmentu 4)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime4|Procesor (niezależnego fragmentu 4)|Procent|Maksimum||Nie wymiarów|
|connectedclients5|Podłączeni klienci (niezależnego fragmentu 5)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed5|Całkowita liczba operacji (identyfikator niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|cachehits5|Trafień w pamięci podręcznej (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|cachemisses5|Chybienia pamięci podręcznej (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|getcommands5|Pobiera (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|setcommands5|Zestawy (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys5|Klucze wykluczonym (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|totalkeys5|Wszystkie klucze (niezależnego fragmentu 5)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys5|Wygasłe kluczy (niezależnego fragmentu 5)|Licznik|Łącznie||Nie wymiarów|
|usedmemory5|Używana pamięć (niezależnego fragmentu 5)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss5|Używana pamięć RSS (niezależnego fragmentu 5)|Bajty|Maksimum||Nie wymiarów|
|serverLoad5|Obciążenia serwera (niezależnego fragmentu 5)|Procent|Maksimum||Nie wymiarów|
|cacheWrite5|Pamięć podręczna zapisu (niezależnego fragmentu 5)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead5|Odczytu pamięci podręcznej (niezależnego fragmentu 5)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime5|Procesor (niezależnego fragmentu 5)|Procent|Maksimum||Nie wymiarów|
|connectedclients6|Podłączeni klienci (niezależnego fragmentu 6)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed6|Całkowita liczba operacji (identyfikator niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|cachehits6|Trafień w pamięci podręcznej (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|cachemisses6|Chybienia pamięci podręcznej (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|getcommands6|Pobiera (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|setcommands6|Zestawy (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys6|Klucze wykluczonym (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|totalkeys6|Wszystkie klucze (niezależnego fragmentu 6)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys6|Wygasłe kluczy (niezależnego fragmentu 6)|Licznik|Łącznie||Nie wymiarów|
|usedmemory6|Używana pamięć (niezależnego fragmentu 6)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss6|Używana pamięć RSS (niezależnego fragmentu 6)|Bajty|Maksimum||Nie wymiarów|
|serverLoad6|Obciążenia serwera (niezależnego fragmentu 6)|Procent|Maksimum||Nie wymiarów|
|cacheWrite6|Pamięć podręczna zapisu (niezależnego fragmentu 6)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead6|Odczytu pamięci podręcznej (niezależnego fragmentu 6)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime6|Procesor (niezależnego fragmentu 6)|Procent|Maksimum||Nie wymiarów|
|connectedclients7|Podłączeni klienci (niezależnego fragmentu 7)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed7|Całkowita liczba operacji (identyfikator niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|cachehits7|Trafień w pamięci podręcznej (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|cachemisses7|Chybienia pamięci podręcznej (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|getcommands7|Pobiera (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|setcommands7|Zestawy (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys7|Klucze wykluczonym (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|totalkeys7|Wszystkie klucze (niezależnego fragmentu 7)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys7|Wygasłe kluczy (niezależnego fragmentu 7)|Licznik|Łącznie||Nie wymiarów|
|usedmemory7|Używana pamięć (niezależnego fragmentu 7)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss7|Używana pamięć RSS (niezależnego fragmentu 7)|Bajty|Maksimum||Nie wymiarów|
|serverLoad7|Obciążenia serwera (niezależnego fragmentu 7)|Procent|Maksimum||Nie wymiarów|
|cacheWrite7|Pamięć podręczna zapisu (niezależnego fragmentu 7)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead7|Odczytu pamięci podręcznej (niezależnego fragmentu 7)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime7|Procesor (niezależnego fragmentu 7)|Procent|Maksimum||Nie wymiarów|
|connectedclients8|Podłączeni klienci (niezależnego fragmentu 8)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed8|Całkowita liczba operacji (identyfikator niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|cachehits8|Trafień w pamięci podręcznej (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|cachemisses8|Chybienia pamięci podręcznej (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|getcommands8|Pobiera (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|setcommands8|Zestawy (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys8|Klucze wykluczonym (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|totalkeys8|Wszystkie klucze (niezależnego fragmentu 8)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys8|Wygasłe kluczy (niezależnego fragmentu 8)|Licznik|Łącznie||Nie wymiarów|
|usedmemory8|Używana pamięć (niezależnego fragmentu 8)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss8|Używana pamięć RSS (niezależnego fragmentu 8)|Bajty|Maksimum||Nie wymiarów|
|serverLoad8|Obciążenia serwera (niezależnego fragmentu 8)|Procent|Maksimum||Nie wymiarów|
|cacheWrite8|Pamięć podręczna zapisu (niezależnego fragmentu 8)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead8|Odczytu pamięci podręcznej (niezależnego fragmentu 8)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime8|Procesor (niezależnego fragmentu 8)|Procent|Maksimum||Nie wymiarów|
|connectedclients9|Podłączeni klienci (niezależnego fragmentu 9)|Licznik|Maksimum||Nie wymiarów|
|totalcommandsprocessed9|Całkowita liczba operacji (identyfikator niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|cachehits9|Trafień w pamięci podręcznej (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|cachemisses9|Chybienia pamięci podręcznej (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|getcommands9|Pobiera (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|setcommands9|Zestawy (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|evictedkeys9|Klucze wykluczonym (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|totalkeys9|Wszystkie klucze (niezależnego fragmentu 9)|Licznik|Maksimum||Nie wymiarów|
|expiredkeys9|Wygasłe kluczy (niezależnego fragmentu 9)|Licznik|Łącznie||Nie wymiarów|
|usedmemory9|Używana pamięć (niezależnego fragmentu 9)|Bajty|Maksimum||Nie wymiarów|
|usedmemoryRss9|Używana pamięć RSS (niezależnego fragmentu 9)|Bajty|Maksimum||Nie wymiarów|
|serverLoad9|Obciążenia serwera (niezależnego fragmentu 9)|Procent|Maksimum||Nie wymiarów|
|cacheWrite9|Pamięć podręczna zapisu (niezależnego fragmentu 9)|BytesPerSecond|Maksimum||Nie wymiarów|
|cacheRead9|Odczytu pamięci podręcznej (niezależnego fragmentu 9)|BytesPerSecond|Maksimum||Nie wymiarów|
|percentProcessorTime9|Procesor (niezależnego fragmentu 9)|Procent|Maksimum||Nie wymiarów|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.ClassicCompute/virtualMachines

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procentowe użycie procesora CPU|Procentowe użycie procesora CPU|Procent|Średni|Procent obliczeniowe przydzielone jednostki, które są obecnie używane przez maszyny wirtualne.|Nie wymiarów|
|Sieć — wejście|Sieć — wejście|Bajty|Łącznie|Liczba bajtów odebranych na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch przychodzący).|Nie wymiarów|
|Sieć — wyjście|Sieć — wyjście|Bajty|Łącznie|Liczba bajtów na wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący).|Nie wymiarów|
|Bajty odczytu dysku/s|Czas odczytu dyskowego|BytesPerSecond|Średni|Średnią liczbę bajtów do odczytu z dysku podczas okresu monitorowania.|Nie wymiarów|
|Bajty zapisu dysku/s|Zapisu na dysku|BytesPerSecond|Średni|Zapisywane na dysku podczas monitorowania okres średnią liczbę bajtów.|Nie wymiarów|
|Operacje odczytu dysku/s|Operacje odczytu dysku/s|CountPerSecond|Średni|Odczyt dysku IOPS.|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średni|Zapisu dyskowego IOPS.|Nie wymiarów|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalCalls|Łączna liczba wywołań|Licznik|Łącznie|Łączna liczba wywołań.|Nie wymiarów|
|SuccessfulCalls|Wywołania zakończone powodzeniem|Licznik|Łącznie|Liczba wywołań zakończonych powodzeniem.|Nie wymiarów|
|TotalErrors|Łączna liczba błędów|Licznik|Łącznie|Łączna liczba wywołań z odpowiedzią oznaczającą błąd (odpowiedź HTTP o kodzie 4xx lub 5xx).|Nie wymiarów|
|BlockedCalls|Zablokowane wywołania|Licznik|Łącznie|Liczba wywołań, które przekraczają limit szybkości lub przydziału.|Nie wymiarów|
|ServerErrors|Błędy serwera|Licznik|Łącznie|Liczba wywołań z błędem wewnętrznym usługi (odpowiedź HTTP o kodzie 5xx).|Nie wymiarów|
|ClientErrors|Błędy klienta|Licznik|Łącznie|Liczba wywołań z błędem po stronie klienta (odpowiedź HTTP o kodzie 4xx).|Nie wymiarów|
|DataIn|Dane wejściowe|Bajty|Łącznie|Rozmiar danych przychodzących (w bajtach).|Nie wymiarów|
|DataOut|Dane wyjściowe|Bajty|Łącznie|Rozmiar danych wychodzących (w bajtach).|Nie wymiarów|
|Opóźnienie|Opóźnienie|W milisekundach|Średni|Opóźnienie w milisekundach.|Nie wymiarów|
|CharactersTranslated|Znaki translacji|Licznik|Łącznie|Całkowita liczba znaków w przychodzące żądanie tekstu.|Nie wymiarów|
|SpeechSessionDuration|Mowy czas trwania sesji|Sekundy|Łącznie|Łączny czas trwania mowy sesji w sekundach.|Nie wymiarów|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procentowe użycie procesora CPU|Procentowe użycie procesora CPU|Procent|Średni|Procent przydzielonych jednostek obliczeniowych, które są obecnie używane przez maszyny wirtualne|Nie wymiarów|
|Sieć — wejście|Sieć — wejście|Bajty|Łącznie|Liczba bajtów odebranych ze wszystkich interfejsów sieciowych przez maszyny wirtualne (ruch przychodzący)|Nie wymiarów|
|Sieć — wyjście|Sieć — wyjście|Bajty|Łącznie|Liczba bajtów wysłanych we wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący)|Nie wymiarów|
|Bajty odczytane z dysku|Bajty odczytane z dysku|Bajty|Łącznie|Całkowita liczba bajtów odczytanych z dysku w okresie monitorowania|Nie wymiarów|
|Bajty zapisane na dysku|Bajty zapisane na dysku|Bajty|Łącznie|Całkowita liczba bajtów zapisanych na dysku w okresie monitorowania|Nie wymiarów|
|Operacje odczytu dysku/s|Operacje odczytu dysku/s|CountPerSecond|Średni|Operacje we/wy odczytu dysku|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średni|Operacje we/wy zapisu dysku|Nie wymiarów|
|Pozostały kredyt Procesora|Pozostały kredyt Procesora|Licznik|Średni|Całkowita liczba punkty serii|Nie wymiarów|
|Używane środków Procesora|Używane środków Procesora|Licznik|Średni|Całkowita liczba kredytów używane przez maszynę wirtualną|Nie wymiarów|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procentowe użycie procesora CPU|Procentowe użycie procesora CPU|Procent|Średni|Procent przydzielonych jednostek obliczeniowych, które są obecnie używane przez maszyny wirtualne|Nie wymiarów|
|Sieć — wejście|Sieć — wejście|Bajty|Łącznie|Liczba bajtów odebranych ze wszystkich interfejsów sieciowych przez maszyny wirtualne (ruch przychodzący)|Nie wymiarów|
|Sieć — wyjście|Sieć — wyjście|Bajty|Łącznie|Liczba bajtów wysłanych we wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący)|Nie wymiarów|
|Bajty odczytane z dysku|Bajty odczytane z dysku|Bajty|Łącznie|Całkowita liczba bajtów odczytanych z dysku w okresie monitorowania|Nie wymiarów|
|Bajty zapisane na dysku|Bajty zapisane na dysku|Bajty|Łącznie|Całkowita liczba bajtów zapisanych na dysku w okresie monitorowania|Nie wymiarów|
|Operacje odczytu dysku/s|Operacje odczytu dysku/s|CountPerSecond|Średni|Operacje we/wy odczytu dysku|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średni|Operacje we/wy zapisu dysku|Nie wymiarów|
|Pozostały kredyt Procesora|Pozostały kredyt Procesora|Licznik|Średni|Całkowita liczba punkty serii|Nie wymiarów|
|Używane środków Procesora|Używane środków Procesora|Licznik|Średni|Całkowita liczba kredytów używane przez maszynę wirtualną|Nie wymiarów|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Procentowe użycie procesora CPU|Procentowe użycie procesora CPU|Procent|Średni|Procent przydzielonych jednostek obliczeniowych, które są obecnie używane przez maszyny wirtualne|Nie wymiarów|
|Sieć — wejście|Sieć — wejście|Bajty|Łącznie|Liczba bajtów odebranych ze wszystkich interfejsów sieciowych przez maszyny wirtualne (ruch przychodzący)|Nie wymiarów|
|Sieć — wyjście|Sieć — wyjście|Bajty|Łącznie|Liczba bajtów wysłanych we wszystkich interfejsach sieciowych przez maszyny wirtualne (ruch wychodzący)|Nie wymiarów|
|Bajty odczytane z dysku|Bajty odczytane z dysku|Bajty|Łącznie|Całkowita liczba bajtów odczytanych z dysku w okresie monitorowania|Nie wymiarów|
|Bajty zapisane na dysku|Bajty zapisane na dysku|Bajty|Łącznie|Całkowita liczba bajtów zapisanych na dysku w okresie monitorowania|Nie wymiarów|
|Operacje odczytu dysku/s|Operacje odczytu dysku/s|CountPerSecond|Średni|Operacje we/wy odczytu dysku|Nie wymiarów|
|Operacje zapisu dysku/s|Operacje zapisu dysku/s|CountPerSecond|Średni|Operacje we/wy zapisu dysku|Nie wymiarów|
|Pozostały kredyt Procesora|Pozostały kredyt Procesora|Licznik|Średni|Całkowita liczba punkty serii|Nie wymiarów|
|Używane środków Procesora|Używane środków Procesora|Licznik|Średni|Całkowita liczba kredytów używane przez maszynę wirtualną|Nie wymiarów|

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
|DCIProfilesCount|Liczba wystąpień profilu|Licznik|Ostatnia||Nie wymiarów|
|DCIInteractionsPerMonthCount|Interakcje na liczba miesięcy|Licznik|Ostatnia||Nie wymiarów|
|DCIKpisCount|Liczba wskaźnika KPI|Licznik|Ostatnia||Nie wymiarów|
|DCISegmentsCount|Liczba segmentów.|Licznik|Ostatnia||Nie wymiarów|
|DCIPredictiveMatchPoliciesCount|Liczba dopasowań predykcyjnej|Licznik|Ostatnia||Nie wymiarów|
|DCIPredictionsCount|Liczba prognozowania|Licznik|Ostatnia||Nie wymiarów|

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
|TotalStorage|Całkowita ilość miejsca|Bajty|Maksimum|Całkowita ilość danych przechowywanych w ramach konta.|Nie wymiarów|
|DataWritten|Dane zapisywane|Bajty|Łącznie|Całkowita ilość danych zapisywane na koncie.|Nie wymiarów|
|Odczyt danych|Odczyt danych|Bajty|Łącznie|Łączna ilość danych do odczytu z konta.|Nie wymiarów|
|WriteRequests|Żądań zapisu|Licznik|Łącznie|Liczba danych żądań zapisu do konta.|Nie wymiarów|
|ReadRequests|Żądań odczytu|Licznik|Łącznie|Liczba danych do odczytu żądania do tego konta.|Nie wymiarów|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent procesora CPU|Procent|Średni|Procent procesora CPU|Nie wymiarów|
|compute_limit|Obliczenia bazy danych limit jednostki|Licznik|Średni|Obliczenia bazy danych limit jednostki|Nie wymiarów|
|compute_consumption_percent|Obliczenia bazy danych procent jednostki|Procent|Średni|Obliczenia bazy danych procent jednostki|Nie wymiarów|
|memory_percent|Procent pamięci|Procent|Średni|Procent pamięci|Nie wymiarów|
|io_consumption_percent|Procent we/wy|Procent|Średni|Procent we/wy|Nie wymiarów|
|storage_percent|Procent użycia magazynu|Procent|Średni|Procent użycia magazynu|Nie wymiarów|
|storage_used|Użyty magazyn|Bajty|Średni|Użyty magazyn|Nie wymiarów|
|storage_limit|Limit magazynu|Bajty|Średni|Limit magazynu|Nie wymiarów|
|active_connections|Całkowita liczba aktywnych połączeń|Licznik|Średni|Całkowita liczba aktywnych połączeń|Nie wymiarów|
|connections_failed|Całkowita liczba połączeń nie powiodło się|Licznik|Średni|Całkowita liczba połączeń nie powiodło się|Nie wymiarów|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent procesora CPU|Procent|Średni|Procent procesora CPU|Nie wymiarów|
|compute_limit|Obliczenia bazy danych limit jednostki|Licznik|Średni|Obliczenia bazy danych limit jednostki|Nie wymiarów|
|compute_consumption_percent|Obliczenia bazy danych procent jednostki|Procent|Średni|Obliczenia bazy danych procent jednostki|Nie wymiarów|
|memory_percent|Procent pamięci|Procent|Średni|Procent pamięci|Nie wymiarów|
|io_consumption_percent|Procent we/wy|Procent|Średni|Procent we/wy|Nie wymiarów|
|storage_percent|Procent użycia magazynu|Procent|Średni|Procent użycia magazynu|Nie wymiarów|
|storage_used|Użyty magazyn|Bajty|Średni|Użyty magazyn|Nie wymiarów|
|storage_limit|Limit magazynu|Bajty|Średni|Limit magazynu|Nie wymiarów|
|active_connections|Całkowita liczba aktywnych połączeń|Licznik|Średni|Całkowita liczba aktywnych połączeń|Nie wymiarów|
|connections_failed|Całkowita liczba połączeń nie powiodło się|Licznik|Średni|Całkowita liczba połączeń nie powiodło się|Nie wymiarów|

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
|d2c.endpoints.latency.eventHubs|Opóźnienie komunikat dla punktów końcowych Centrum zdarzeń|Milisekundy|Średni|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego Centrum zdarzeń, w milisekundach|Nie wymiarów|
|d2c.endpoints.egress.serviceBusQueues|Komunikaty dostarczone do punktów końcowych z kolejką usługi Service Bus|Licznik|Łącznie|Liczba wiadomości kolejką usługi Service Bus punkty końcowe zostały pomyślnie zapisane|Nie wymiarów|
|d2c.endpoints.latency.serviceBusQueues|Czas oczekiwania na wiadomość dla punktów końcowych z kolejką usługi Service Bus|Milisekundy|Średni|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego kolejką usługi Service Bus, w milisekundach|Nie wymiarów|
|d2c.endpoints.egress.serviceBusTopics|Komunikaty dostarczone do punktów końcowych temat magistrali usług|Licznik|Łącznie|Liczba wiadomości zostały pomyślnie zapisane punkty końcowe temat magistrali usług|Nie wymiarów|
|d2c.endpoints.latency.serviceBusTopics|Opóźnienie komunikat dla punktów końcowych temat magistrali usług|Milisekundy|Średni|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego Service Bus tematu w milisekundach|Nie wymiarów|
|d2c.endpoints.egress.builtIn.events|Wiadomość dostarczona do wbudowanej punktu końcowego (wiadomości/zdarzeń)|Licznik|Łącznie|Liczba komunikatów wbudowanym punktem końcowym (wiadomości/zdarzeń) zostały pomyślnie zapisane|Nie wymiarów|
|d2c.endpoints.latency.builtIn.events|Czas oczekiwania wiadomości na wbudowanym punktem końcowym (wiadomości/zdarzeń)|Milisekundy|Średni|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i transfer danych przychodzących wiadomości na wbudowanym punktem końcowym (wiadomości/zdarzeń), w milisekundach |Nie wymiarów|
|d2c.endpoints.egress.Storage|Komunikaty dostarczone do punktów końcowych magazynu|Licznik|Łącznie|Liczba wiadomości zostały pomyślnie zapisane punkty końcowe usługi storage|Nie wymiarów|
|d2c.endpoints.latency.Storage|Opóźnienie komunikat dla punktów końcowych magazynu|Milisekundy|Średni|Średnie opóźnienie między komunikat przychodzący z Centrum IoT i komunikat przychodzący do punktu końcowego magazynu (w milisekundach)|Nie wymiarów|
|d2c.endpoints.egress.Storage.Bytes|Dane zapisywane w pamięci masowej|Bajty|Łącznie|Ilość danych w bajtach, zapisane punkty końcowe usługi storage|Nie wymiarów|
|d2c.endpoints.egress.Storage.blobs|Zapisane do magazynu obiektów blob|Licznik|Łącznie|Liczba zapisywane do punktów końcowych magazynu obiektów blob|Nie wymiarów|
|d2c.Twin.Read.SUCCESS|Pomyślne dwie odczytuje z urządzeń|Licznik|Łącznie|Liczba wszystkich pomyślnych odczyty dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Read.failure|Nie powiodło się dwie odczyty z urządzeń|Licznik|Łącznie|Liczba wszystkich nie odczyty dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Read.size|Rozmiar odpowiedzi odczytów dwie z urządzeń|Bajty|Średni|Średnia, min i max z wszystkie udane, inicjowanych przez urządzenie dwie odczytów.|Nie wymiarów|
|d2c.Twin.Update.SUCCESS|Pomyślne dwie aktualizacje z urządzeń|Licznik|Łącznie|Liczba wszystkich pomyślnych aktualizacji dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Update.failure|Nie powiodło się dwie aktualizacje z urządzeń|Licznik|Łącznie|Liczba wszystkich nie aktualizacje dwie inicjowanych przez urządzenie.|Nie wymiarów|
|d2c.Twin.Update.size|Rozmiar aktualizacji dwie z urządzeń|Bajty|Średni|Średnia, minimum i maksymalny rozmiar wszystkich pomyślnych, inicjowanych przez urządzenie dwie aktualizacji.|Nie wymiarów|
|c2d.Methods.SUCCESS|Pomyślne bezpośrednie wywołania metod|Licznik|Łącznie|Liczba wszystkich pomyślnych wywołań metody bezpośredniego.|Nie wymiarów|
|c2d.Methods.failure|Nie powiodło się bezpośrednie wywołania metod|Licznik|Łącznie|Liczba wszystkich nie wywołania metody bezpośredniego.|Nie wymiarów|
|c2d.Methods.requestSize|Rozmiar żądania bezpośrednie wywołania metod|Bajty|Średni|Średnia, min i max wszystkich pomyślnych żądań metoda bezpośrednia.|Nie wymiarów|
|c2d.Methods.responseSize|Rozmiar odpowiedzi bezpośrednie wywołania metod|Bajty|Średni|Średnia, minimum i maksimum wszystkie udane metoda bezpośrednia odpowiedzi.|Nie wymiarów|
|c2d.Twin.Read.SUCCESS|Pomyślne dwie odczytuje z zaplecza|Licznik|Łącznie|Liczba wszystkich pomyślnie zainicjował zakończenia wstecz dwie odczytuje.|Nie wymiarów|
|c2d.Twin.Read.failure|Nie powiodło się dwie odczytuje z zaplecza|Licznik|Łącznie|Liczba wszystkich nie odczyty wstecz zakończenia inicjowane przez dwie.|Nie wymiarów|
|c2d.Twin.Read.size|Rozmiar odpowiedzi odczytów dwie z wewnętrznego|Bajty|Średni|Średnia, min i max z wszystkie udane, Wstecz zakończenia inicjowane przez dwie odczytów.|Nie wymiarów|
|c2d.Twin.Update.SUCCESS|Pomyślne dwie aktualizacje z wewnętrznego|Licznik|Łącznie|Liczba wszystkich pomyślnych aktualizacji inicjowane zakończenia wstecz dwie.|Nie wymiarów|
|c2d.Twin.Update.failure|Nie powiodło się dwie aktualizacje z wewnętrznego|Licznik|Łącznie|Liczba wszystkich nie powiodło się wstecz zakończenia inicjowane przez dwie aktualizacje.|Nie wymiarów|
|c2d.Twin.Update.size|Rozmiar aktualizacji dwie z wewnętrznego|Bajty|Średni|Średnia, minimum i maksymalny rozmiar wszystkich pomyślnych, Wstecz zakończenia inicjowane przez dwie aktualizacji.|Nie wymiarów|
|twinQueries.success|Dwie pomyślne zapytania|Licznik|Łącznie|Liczba wszystkich zapytań dwie powiodło się.|Nie wymiarów|
|twinQueries.failure|Nie powiodło się dwie zapytań|Licznik|Łącznie|Liczba wszystkich zapytań dwie nie powiodło się.|Nie wymiarów|
|twinQueries.resultSize|Rozmiar wyników zapytania dwie|Bajty|Średni|Średnia, minimum i maksymalny rozmiar wyników wszystkich zapytań dwie powiodło się.|Nie wymiarów|
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
|dailyMessageQuotaUsed|Całkowita liczba komunikatów używany|Licznik|Średni|Liczba całkowita liczba komunikatów używany w tej chwili. Jest to wartość zbiorcza, która jest resetowany do zera w formacie UTC 00:00 każdego dnia.|Nie wymiarów|

## <a name="microsoftdevicesprovisioningservices"></a>Microsoft.Devices/provisioningServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|RegistrationAttempts|Prób rejestracji|Licznik|Łącznie|Liczba prób rejestracji urządzenia|Stan ProvisioningServiceName, IotHubName,|
|DeviceAssignments|Urządzenia przypisane|Licznik|Łącznie|Liczba urządzenia przypisane do Centrum IoT|ProvisioningServiceName, IotHubName|
|AttestationAttempts|Poświadczenie prób|Licznik|Łącznie|Liczba podjęto poświadczenia urządzenia|Protokół ProvisioningServiceName, stan,|

<a id="cosmosdb"></a>

## <a name="microsoftdocumentdbdatabaseaccounts-azure-cosmos-db"></a>Microsoft.DocumentDB/databaseAccounts (rozwiązania Azure Cosmos DB)
|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TotalRequests|Łączna liczba żądań|Licznik|Licznik|Liczba żądań|DatabaseAccount, CollectionName, DatabaseName, Region, StatusCode|
|MongoRequests|MONGO żądań|Licznik|Licznik|Liczba złożonych żądań Mongo|DatabaseAccount, CollectionName, DatabaseName, Region, kod błędu, CommandName|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|INREQS|Przychodzących żądań wysłania|Licznik|Łącznie|Całkowita liczba przychodzących wysyłać żądania dla przestrzeni nazw|Nie wymiarów|
|SUCCREQ|Żądania zakończone powodzeniem|Licznik|Łącznie|Łączna liczba udanych żądań dla przestrzeni nazw|Nie wymiarów|
|FAILREQ|Żądania zakończone niepowodzeniem|Licznik|Łącznie|Łączna liczba nieudanych żądań dla przestrzeni nazw|Nie wymiarów|
|SVRBSY|Błędy typu Serwer zajęty|Licznik|Łącznie|Łączna liczba błędów zajętego serwera dla przestrzeni nazw|Nie wymiarów|
|INTERR|Wewnętrzne błędy serwera|Licznik|Łącznie|Łączna liczba wewnętrznych błędów serwera dla przestrzeni nazw|Nie wymiarów|
|MISCERR|Inne błędy|Licznik|Łącznie|Łączna liczba nieudanych żądań dla przestrzeni nazw|Nie wymiarów|
|INMSGS|Komunikaty przychodzące (przestarzałe)|Licznik|Łącznie|Całkowita liczba komunikatów przychodzących w przypadku przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Metryka wiadomości przychodzących|Nie wymiarów|
|EHINMSGS|Wiadomości przychodzące|Licznik|Łącznie|Łączna liczba komunikatów przychodzących dla przestrzeni nazw|Nie wymiarów|
|OUTMSGS|Wysyła komunikaty wychodzące (przestarzałe)|Licznik|Łącznie|Całkowita liczba wychodzących wiadomości w przypadku przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Metryka wysyła komunikaty wychodzące|Nie wymiarów|
|EHOUTMSGS|Wiadomości wychodzące|Licznik|Łącznie|Łączna liczba komunikatów wychodzących dla przestrzeni nazw|Nie wymiarów|
|EHINMBS|Przychodzące bajty (przestarzały)|Bajty|Łącznie|Zdarzenia koncentratora przychodzącego komunikatu przepływność dla przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Metryka Bajty przychodzące|Nie wymiarów|
|EHINBYTES|Przychodzące bajty|Bajty|Łącznie|Przepływność komunikatów przychodzących centrum zdarzeń usługi Event Hubs w przestrzeni nazw|Nie wymiarów|
|EHOUTMBS|Wychodzące bajty (przestarzały)|Bajty|Łącznie|Zdarzenia koncentratora wychodzących wiadomości przepływność dla przestrzeni nazw. Ta metryka jest przestarzały. Zamiast tego użyj Wychodzące bajty metryki|Nie wymiarów|
|EHOUTBYTES|Wychodzące bajty|Bajty|Łącznie|Przepływność komunikatów wychodzących centrum zdarzeń usługi Event Hubs w przestrzeni nazw|Nie wymiarów|
|EHABL|Archiwizuj komunikaty listy prac|Licznik|Łącznie|Zarchiwizowane komunikaty centrum zdarzeń usługi Event Hubs na liście prac dla przestrzeni nazw|Nie wymiarów|
|EHAMSGS|Archiwizowanie komunikatów|Licznik|Łącznie|Zarchiwizowane komunikaty centrum zdarzeń usługi Event Hubs w przestrzeni nazw|Nie wymiarów|
|EHAMBS|Przepływność archiwizowania komunikatów|Bajty|Łącznie|Przepływność zarchiwizowanych komunikatów centrum zdarzeń usługi Event Hubs w przestrzeni nazw|Nie wymiarów|

## <a name="microsoftinsightsautoscalesettings"></a>Microsoft.Insights/AutoscaleSettings

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|ObservedMetricValue|Odczytaną wartość metryki|Licznik|Średni|Wartość obliczana przez skalowania automatycznego podczas wykonywania|MetricTriggerSource|
|MetricThreshold|Próg metryki|Licznik|Średni|Próg skonfigurowanych skalowania automatycznego podczas automatycznego skalowania.|MetricTriggerRule|
|ObservedCapacity|Obserwowana wydajność|Licznik|Średni|Pojemność zgłaszane do skalowania automatycznego podczas jej wykonywania.|Nie wymiarów|
|ScaleActionsInitiated|Zainicjowano akcji skalowania|Licznik|Łącznie|Kierunek operacji skalowania.|ScaleDirection|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|RunsStarted|Przebiegi rozpoczęte|Licznik|Łącznie|Liczba rozpoczętych przebiegów przepływu pracy.|Nie wymiarów|
|RunsCompleted|Przebiegi ukończone|Licznik|Łącznie|Liczba ukończonych przebiegów przepływu pracy.|Nie wymiarów|
|RunsSucceeded|Przebiegi zakończone powodzeniem|Licznik|Łącznie|Liczba przebiegów przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|RunsFailed|Przebiegi zakończone niepowodzeniem|Licznik|Łącznie|Liczba przebiegów przepływu pracy zakończonych niepowodzeniem.|Nie wymiarów|
|RunsCancelled|Przebiegi anulowane|Licznik|Łącznie|Liczba anulowanych przebiegów przepływu pracy.|Nie wymiarów|
|RunLatency|Opóźnienie przebiegów|Sekundy|Średni|Opóźnienie ukończonych przebiegów przepływu pracy.|Nie wymiarów|
|RunSuccessLatency|Opóźnienie przebiegów zakończonych powodzeniem|Sekundy|Średni|Opóźnienie przebiegów przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|RunThrottledEvents|Zdarzenia ograniczenia przebiegu|Licznik|Łącznie|Liczba zdarzeń ograniczenia akcji lub wyzwalacza przepływu pracy.|Nie wymiarów|
|RunFailurePercentage|Procent niepowodzeń przebiegów|Procent|Łącznie|Procent przebiegów przepływu pracy zakończonych niepowodzeniem.|Nie wymiarów|
|ActionsStarted|Rozpoczęte akcje |Licznik|Łącznie|Liczba rozpoczętych akcji przepływu pracy.|Nie wymiarów|
|ActionsCompleted|Ukończone akcje |Licznik|Łącznie|Liczba ukończonych akcji przepływu pracy.|Nie wymiarów|
|ActionsSucceeded|Akcje zakończone powodzeniem |Licznik|Łącznie|Liczba akcji przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|ActionsFailed|Akcje zakończone niepowodzeniem|Licznik|Łącznie|Liczba akcji przepływu pracy zakończonych niepowodzeniem.|Nie wymiarów|
|ActionsSkipped|Pominięte akcje |Licznik|Łącznie|Liczba pominiętych akcji przepływu pracy.|Nie wymiarów|
|ActionLatency|Opóźnienie akcji |Sekundy|Średni|Opóźnienie ukończonych akcji przepływu pracy.|Nie wymiarów|
|ActionSuccessLatency|Opóźnienie akcji – powodzenie |Sekundy|Średni|Opóźnienie akcji przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|ActionThrottledEvents|Zdarzenia ograniczenia akcji|Licznik|Łącznie|Liczba zdarzeń ograniczenia akcji przepływu pracy.|Nie wymiarów|
|TriggersStarted|Rozpoczęte wyzwalacze |Licznik|Łącznie|Liczba rozpoczętych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggersCompleted|Ukończone wyzwalacze |Licznik|Łącznie|Liczba ukończonych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggersSucceeded|Udane wyzwalacze |Licznik|Łącznie|Liczba wyzwalaczy przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|TriggersFailed|Nieudane wyzwalacze |Licznik|Łącznie|Liczba wyzwalaczy przepływu pracy zakończonych niepowodzeniem.|Nie wymiarów|
|TriggersSkipped|Pominięte wyzwalacze|Licznik|Łącznie|Liczba pominiętych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggersFired|Uruchomione wyzwalacze |Licznik|Łącznie|Liczba uaktywnionych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggerLatency|Opóźnienie wyzwalacza |Sekundy|Średni|Opóźnienie ukończonych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggerFireLatency|Opóźnienie uruchomienia wyzwalacza |Sekundy|Średni|Opóźnienie uaktywnionych wyzwalaczy przepływu pracy.|Nie wymiarów|
|TriggerSuccessLatency|Opóźnienie udanego wyzwalacza |Sekundy|Średni|Opóźnienie wyzwalaczy przepływu pracy zakończonych powodzeniem.|Nie wymiarów|
|TriggerThrottledEvents|Zdarzenia ograniczenia wyzwalacza|Licznik|Łącznie|Liczba zdarzeń ograniczenia wyzwalacza przepływu pracy.|Nie wymiarów|
|BillableActionExecutions|Rozliczane wykonania akcji|Licznik|Łącznie|Liczba rozliczanych wykonań akcji przepływu pracy.|Nie wymiarów|
|BillableTriggerExecutions|Rozliczane wykonania wyzwalaczy|Licznik|Łącznie|Liczba rozliczanych wykonań wyzwalaczy przepływu pracy.|Nie wymiarów|
|TotalBillableExecutions|Łączna liczba rozliczanych wykonań|Licznik|Łącznie|Liczba rozliczanych wykonań przepływu pracy.|Nie wymiarów|

## <a name="microsoftnetworkloadbalancers"></a>Microsoft.Network/loadBalancers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|VipAvailability|Dostępność adresu VIP|Licznik|Średni|Dostępność punktów końcowych VIP, na podstawie wyników badania|Wirtualny adres IP, VipPort|
|DipAvailability|Dostępność DIP|Licznik|Średni|Dostępność punktów końcowych DIP, na podstawie wyników badania|ProtocolType, DipPort, wirtualny adres IP, VipPort, DipAddress|
|ByteCount|Liczba bajtów|Licznik|Łącznie|Całkowita liczba bajtów przesłanych w określonym przedziale czasu|Wirtualny adres IP, VipPort, kierunku|
|PacketCount|Liczba pakietów|Licznik|Łącznie|Całkowita liczba pakietów wysłanych w ciągu okresu czasu|Wirtualny adres IP, VipPort, kierunku|
|SYNCount|Liczba SYN|Licznik|Łącznie|Całkowita liczba pakietów SYN transmitowane w określonym przedziale czasu|Wirtualny adres IP, VipPort, kierunku|
|SnatConnectionCount|Liczba połączeń SNAT|Licznik|Łącznie|Całkowita liczba nowych połączeń SNAT utworzona w okresie czasu|Element ConnectionState wirtualny adres IP, DipAddress,|

## <a name="microsoftnetworkpublicipaddresses"></a>Microsoft.Network/publicIPAddresses

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|PacketsInDDoS|Liczba przychodzących pakietów DDoS|CountPerSecond|Średni|Liczba przychodzących pakietów DDoS|Nie wymiarów|
|PacketsDroppedDDoS|Liczba porzuconych pakietów przychodzących DDoS|CountPerSecond|Średni|Liczba porzuconych pakietów przychodzących DDoS|Nie wymiarów|
|PacketsForwardedDDoS|Pakiety przychodzące przekazywane przed atakami DDoS|CountPerSecond|Średni|Pakiety przychodzące przekazywane przed atakami DDoS|Nie wymiarów|
|TCPPacketsInDDoS|Liczba przychodzących pakietów TCP przed atakami DDoS|CountPerSecond|Średni|Liczba przychodzących pakietów TCP przed atakami DDoS|Nie wymiarów|
|TCPPacketsDroppedDDoS|Pakiety przychodzące TCP porzucony przed atakami DDoS|CountPerSecond|Średni|Pakiety przychodzące TCP porzucony przed atakami DDoS|Nie wymiarów|
|TCPPacketsForwardedDDoS|Pakiety przychodzące TCP przekazywane przed atakami DDoS|CountPerSecond|Średni|Pakiety przychodzące TCP przekazywane przed atakami DDoS|Nie wymiarów|
|UDPPacketsInDDoS|Liczba przychodzących pakietów UDP DDoS|CountPerSecond|Średni|Liczba przychodzących pakietów UDP DDoS|Nie wymiarów|
|UDPPacketsDroppedDDoS|Przychodzących pakietów UDP porzucony przed atakami DDoS|CountPerSecond|Średni|Przychodzących pakietów UDP porzucony przed atakami DDoS|Nie wymiarów|
|UDPPacketsForwardedDDoS|Przekazywane DDoS przychodzących pakietów UDP|CountPerSecond|Średni|Przekazywane DDoS przychodzących pakietów UDP|Nie wymiarów|
|BytesInDDoS|Przychodzące bajty przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty przed atakami DDoS|Nie wymiarów|
|BytesDroppedDDoS|Przychodzące bajty porzucony przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty porzucony przed atakami DDoS|Nie wymiarów|
|BytesForwardedDDoS|Przychodzące bajty przekazywane przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty przekazywane przed atakami DDoS|Nie wymiarów|
|TCPBytesInDDoS|Przychodzące bajty TCP przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty TCP przed atakami DDoS|Nie wymiarów|
|TCPBytesDroppedDDoS|Przychodzące bajty TCP porzucony przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty TCP porzucony przed atakami DDoS|Nie wymiarów|
|TCPBytesForwardedDDoS|Przychodzące bajty TCP przekazywane przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty TCP przekazywane przed atakami DDoS|Nie wymiarów|
|UDPBytesInDDoS|Przychodzące bajty UDP DDoS|BytesPerSecond|Średni|Przychodzące bajty UDP DDoS|Nie wymiarów|
|UDPBytesDroppedDDoS|Przychodzące bajty UDP porzucony przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty UDP porzucony przed atakami DDoS|Nie wymiarów|
|UDPBytesForwardedDDoS|Przychodzące bajty UDP przekazywane przed atakami DDoS|BytesPerSecond|Średni|Przychodzące bajty UDP przekazywane przed atakami DDoS|Nie wymiarów|
|IfUnderDDoSAttack|W obszarze DDoS ataku lub nie|Licznik|Średni|W obszarze DDoS ataku lub nie|Nie wymiarów|
|DDoSTriggerTCPPackets|Liczba przychodzących pakietów TCP do wyzwolenia ograniczenie DDoS|CountPerSecond|Średni|Liczba przychodzących pakietów TCP do wyzwolenia ograniczenie DDoS|Nie wymiarów|
|DDoSTriggerUDPPackets|Liczba przychodzących pakietów UDP do wyzwolenia ograniczenie DDoS|CountPerSecond|Średni|Liczba przychodzących pakietów UDP do wyzwolenia ograniczenie DDoS|Nie wymiarów|
|VipAvailability|Dostępność|Licznik|Średni|Średnia dostępność adres IP w przedziale czasu|Port|
|ByteCount|Liczba bajtów|Licznik|Łącznie|Całkowita liczba bajtów przesłanych w określonym przedziale czasu|Portu i kierunek|
|PacketCount|Liczba pakietów|Licznik|Łącznie|Całkowita liczba pakietów wysłanych w ciągu okresu czasu|Portu i kierunek|
|SynCount|Liczba SYN|Licznik|Łącznie|Całkowita liczba pakietów SYN transmitowane w określonym przedziale czasu|Portu i kierunek|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Przepływność|Przepływność|BytesPerSecond|Średni|Liczba bajtów na sekundę, ma obsłużone bramy aplikacji|Nie wymiarów|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.Network/virtualNetworkGateways

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TunnelAverageBandwidth|Tunel przepustowości|BytesPerSecond|Średni|Średnia przepustowość tunelu w bajtach na sekundę|ConnectionName, RemoteIP|
|TunnelPeakBandwidth|Szczytowa tunelu przepustowość|BytesPerSecond|Średni|Szczytowa przepustowość tunelu w bajtach na sekundę|ConnectionName, RemoteIP|
|TunnelEgressBytes|Tunel wyjście bajtów|Bajty|Średni|Wychodzące bajty tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelIngressBytes|Tunel bajtów wejściowych|Bajty|Średni|Przychodzące bajty tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelEgressPackets|Tunel wyjście pakietów|Licznik|Średni|Wychodzące liczba pakietów tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelIngressPackets|Tunel pakietów wejściowych|Licznik|Średni|Liczba pakietów przychodząca tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelEgressPacketDropTSMismatch|Porzucanie pakietów niezgodność wyjście TS tunelu|Licznik|Średni|Wychodzące Porzucanie pakietów z niezgodność selektor ruchu tunelu w pięciu minut|ConnectionName, RemoteIP|
|TunnelIngressPacketDropTSMismatch|Porzucanie pakietów niezgodność ruch przychodzący serwera terminali tunelu|Licznik|Średni|Przychodzące porzucenia pakietów z niezgodność selektor ruchu tunelu w pięciu minut|ConnectionName, RemoteIP|

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|BytesIn|BytesIn|Licznik|Łącznie|Bajty ingressing Azure|Nie wymiarów|
|BytesOut|BytesOut|Licznik|Łącznie|Bajty egressing Azure|Nie wymiarów|
|BitsInPerSecond|BitsInPerSecond|CountPerSecond|Średni|Usługa BITS ingressing Azure na sekundę|Nie wymiarów|
|BitsOutPerSecond|BitsOutPerSecond|CountPerSecond|Średni|Usługa BITS egressing Azure na sekundę|Nie wymiarów|

## <a name="microsoftnetworktrafficmanagerprofiles"></a>Microsoft.Network/trafficManagerProfiles

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|QpsByEndpoint|Zapytania przez punkt końcowy zwrócił|Licznik|Łącznie|Ile razy punkt końcowy Menedżera ruchu został zwrócony w określonym przedziale czasu|EndpointName|
|ProbeAgentCurrentEndpointStateByProfileResourceId|Stan punktu końcowego przez punkt końcowy|Licznik|Maksimum|1, jeśli punkt końcowy sondowania stanu jest "włączone", w przeciwnym razie 0.|EndpointName|

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Registration.all|Operacje rejestracji|Licznik|Łącznie|Liczba wszystkich operacji rejestracji zakończonych powodzeniem (operacje tworzenia, aktualizacje, zapytania i operacje usuwania). |Nie wymiarów|
|Registration.Create|Operacje tworzenia rejestracji|Licznik|Łącznie|Liczba wszystkich operacji tworzenia rejestracji zakończonych powodzeniem.|Nie wymiarów|
|Registration.Update|Operacje aktualizacji operacji|Licznik|Łącznie|Liczba wszystkich aktualizacji rejestracji zakończonych powodzeniem.|Nie wymiarów|
|Registration.Get|Operacje odczytu operacji|Licznik|Łącznie|Liczba wszystkich zapytań dotyczących rejestracji zakończonych powodzeniem.|Nie wymiarów|
|Registration.delete|Operacje usuwania rejestracji|Licznik|Łącznie|Liczba wszystkich operacji usuwania rejestracji zakończonych powodzeniem.|Nie wymiarów|
|przychodzące|Wiadomości przychodzące|Licznik|Łącznie|Liczba wszystkich operacji wysyłania wywołania interfejsu API zakończonych powodzeniem. |Nie wymiarów|
|Incoming.scheduled|Wysłano zaplanowane powiadomienia wypychane|Licznik|Łącznie|Anulowano zaplanowane powiadomienia wypychane|Nie wymiarów|
|Incoming.scheduled.Cancel|Anulowano zaplanowane powiadomienia wypychane|Licznik|Łącznie|Anulowano zaplanowane powiadomienia wypychane|Nie wymiarów|
|scheduled.Pending|Oczekujące zaplanowane powiadomienia|Licznik|Łącznie|Oczekujące zaplanowane powiadomienia|Nie wymiarów|
|Installation.all|Operacje zarządzania instalacją|Licznik|Łącznie|Operacje zarządzania instalacją|Nie wymiarów|
|Installation.Get|Pobierz operacje instalacji|Licznik|Łącznie|Pobierz operacje instalacji|Nie wymiarów|
|Installation.UPSERT|Utwórz lub zaktualizuj operacje instalacji|Licznik|Łącznie|Utwórz lub zaktualizuj operacje instalacji|Nie wymiarów|
|Installation.Patch|Operacje instalacji poprawki|Licznik|Łącznie|Operacje instalacji poprawki|Nie wymiarów|
|Installation.delete|Usuń operacje instalacji|Licznik|Łącznie|Usuń operacje instalacji|Nie wymiarów|
|Outgoing.allpns.SUCCESS|Powiadomienia zakończone powodzeniem|Licznik|Łącznie|Liczba wszystkich powiadomień zakończonych powodzeniem.|Nie wymiarów|
|Outgoing.allpns.invalidpayload|Błędy ładunku|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy zwrócił błąd oznaczający nieprawidłowy ładunek.|Nie wymiarów|
|Outgoing.allpns.pnserror|Błędy zewnętrznych systemów powiadamiania|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ wystąpił problem podczas komunikowania się z systemem powiadomień platformy (nie obejmuje problemów z uwierzytelnianiem).|Nie wymiarów|
|Outgoing.allpns.channelerror|Błędy kanału|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ kanał był nieprawidłowy i nie został skojarzony z poprawną aplikacją (ograniczoną lub wygasłą).|Nie wymiarów|
|Outgoing.allpns.badorexpiredchannel|Błędy nieprawidłowych lub wygasłych kanałów|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ kanał/token/identyfikator rejestracji w rejestracji wygasł lub był nieprawidłowy.|Nie wymiarów|
|Outgoing.wns.SUCCESS|Powiadomienia usługi WNS zakończone powodzeniem|Licznik|Łącznie|Liczba wszystkich powiadomień zakończonych powodzeniem.|Nie wymiarów|
|Outgoing.wns.invalidcredentials|Błędy autoryzacji usługi WNS (nieprawidłowe poświadczenia)|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptował podanych poświadczeń lub poświadczenia zostały zablokowane. (Windows Live nie rozpoznaje poświadczeń).|Nie wymiarów|
|Outgoing.wns.badchannel|Błąd nieprawidłowego kanału usługi WNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ identyfikator ChannelURI w rejestracji nie został rozpoznany (stan usługi WNS: 404 Nie znaleziono).|Nie wymiarów|
|Outgoing.wns.expiredchannel|Błąd wygasłego kanału usługi WNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ identyfikator ChannelURI wygasł (stan usługi WNS: 410 Przeniesiono).|Nie wymiarów|
|Outgoing.wns.throttled|Powiadomienia usługi WNS z ograniczoną przepływnością|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ usługa WNS ogranicza tę aplikację (stan usługi WNS: 406 Niedozwolone).|Nie wymiarów|
|Outgoing.wns.tokenproviderunreachable|Błędy autoryzacji usługi WNS (niedostępna)|Licznik|Łącznie|Nie można połączyć się z usługą Windows Live.|Nie wymiarów|
|Outgoing.wns.invalidtoken|Błędy autoryzacji usługi WNS (nieprawidłowy token)|Licznik|Łącznie|Token przekazany do usługi WNS jest nieprawidłowy (stan usługi: 401 Brak autoryzacji).|Nie wymiarów|
|Outgoing.wns.wrongtoken|Błędy autoryzacji usługi WNS (niepoprawny token)|Licznik|Łącznie|Dostarczony do WNS token jest prawidłowy, ale dla innej aplikacji (stan usługi WNS: 403 Zabroniony). Może to nastąpić, jeśli ChannelURI w rejestracji jest skojarzony z innej aplikacji. Sprawdź, czy aplikacja kliencka jest skojarzony z tą samą aplikacją, którego poświadczenia znajdują się w Centrum powiadomień.|Nie wymiarów|
|Outgoing.wns.invalidnotificationformat|Nieprawidłowy format powiadomienia usługi WNS|Licznik|Łącznie|Format powiadomienia jest nieprawidłowy (stan usługi WNS: 400). Należy pamiętać, że usługi WNS nie odrzucić wszystkie ładunki nieprawidłowy.|Nie wymiarów|
|Outgoing.wns.invalidnotificationsize|Błąd nieprawidłowego rozmiaru powiadomienia usługi WNS|Licznik|Łącznie|Ładunek powiadomienia jest zbyt duży (stan usługi WNS: 413).|Nie wymiarów|
|Outgoing.wns.channelthrottled|Ograniczono przepływność kanału usługi WNS|Licznik|Łącznie|Powiadomienie zostało porzucone, ponieważ identyfikator ChannelURI w rejestracji jest ograniczany (nagłówek odpowiedzi usługi WNS: X-WNS-NotificationStatus: channelThrottled).|Nie wymiarów|
|Outgoing.wns.channeldisconnected|Rozłączono kanał usługi WNS|Licznik|Łącznie|Powiadomienie zostało porzucone, ponieważ identyfikator ChannelURI w rejestracji jest ograniczany (nagłówek odpowiedzi usługi WNS: X-WNS-DeviceConnectionStatus: rozłączono).|Nie wymiarów|
|Outgoing.wns.dropped|Porzucone powiadomienia usługi WNS|Licznik|Łącznie|Powiadomienie zostało porzucone, ponieważ identyfikator ChannelURI w rejestracji jest ograniczany (X-WNS-NotificationStatus: porzucono, ale nie X-WNS-DeviceConnectionStatus: rozłączono).|Nie wymiarów|
|Outgoing.wns.pnserror|Błędy usługi WNS|Licznik|Łącznie|Nie dostarczono powiadomienia z powodu błędów podczas komunikowania się z usługą WNS.|Nie wymiarów|
|Outgoing.wns.authenticationerror|Błędy uwierzytelniania usługi WNS|Licznik|Łącznie|Nie dostarczono powiadomienia z powodu błędów podczas komunikowania się z błędnym tokenem lub nieprawidłowymi poświadczeniami usługi Windows Live.|Nie wymiarów|
|Outgoing.apns.SUCCESS|Powiadomienia usługi APNS zakończone powodzeniem|Licznik|Łącznie|Liczba wszystkich powiadomień zakończonych powodzeniem.|Nie wymiarów|
|Outgoing.apns.invalidcredentials|Błędy autoryzacji usługi APNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptował podanych poświadczeń lub poświadczenia zostały zablokowane.|Nie wymiarów|
|Outgoing.apns.badchannel|Błąd nieprawidłowego kanału usługi APNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ token jest nieprawidłowy (kod stanu usługi APNS: 8).|Nie wymiarów|
|Outgoing.apns.expiredchannel|Błąd nieprawidłowego kanału usługi APNS|Licznik|Łącznie|Liczba tokenów unieważnionych przy użyciu kanału sprzężenia zwrotnego usługi APNS.|Nie wymiarów|
|Outgoing.apns.invalidnotificationsize|Błąd nieprawidłowego rozmiaru powiadomienia usługi APNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ ładunek powiadomienia był zbyt duży (kod stanu usługi APNS: 7).|Nie wymiarów|
|Outgoing.apns.pnserror|Błędy usługi APNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem z powodu błędów podczas komunikowania się z usługą APNS.|Nie wymiarów|
|Outgoing.gcm.SUCCESS|Powiadomienia usługi GCM zakończone powodzeniem|Licznik|Łącznie|Liczba wszystkich powiadomień zakończonych powodzeniem.|Nie wymiarów|
|Outgoing.gcm.invalidcredentials|Błędy autoryzacji usługi GCM (nieprawidłowe poświadczenia)|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptował podanych poświadczeń lub poświadczenia zostały zablokowane.|Nie wymiarów|
|Outgoing.gcm.badchannel|Błąd nieprawidłowego kanału usługi GCM|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ identyfikator rejestracji wygasł (wynik usługi GSM: nieprawidłowa rejestracja).|Nie wymiarów|
|Outgoing.gcm.expiredchannel|Błąd wygasłego kanału usługi GCM|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ identyfikator rejestracji wygasł (wynik usługi GSM: NotRegistered).|Nie wymiarów|
|Outgoing.gcm.throttled|Powiadomienia usługi GCM z ograniczoną przepływnością|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ usługa GCM ograniczyła tę aplikację (kod stanu usługi GCM: 501–599 lub wynik: Niedostępne).|Nie wymiarów|
|Outgoing.gcm.invalidnotificationformat|Nieprawidłowy format powiadomienia usługi GCM|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ ładunek został nieprawidłowo sformatowany (stan usługi GCM: InvalidDataKey lub InvalidTtl).|Nie wymiarów|
|Outgoing.gcm.invalidnotificationsize|Błąd nieprawidłowego rozmiaru powiadomienia usługi GCM|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ ładunek powiadomienia był zbyt duży (wynik usługi GCM: MessageTooBig).|Nie wymiarów|
|Outgoing.gcm.wrongchannel|Błąd nieprawidłowego kanału usługi GCM|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ identyfikator rejestracji nie został skojarzony z bieżącą aplikacją (wynik usługi GSM: InvalidpackageName).|Nie wymiarów|
|Outgoing.gcm.pnserror|Błędy usługi GCM|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem z powodu błędów podczas komunikowania się z usługą GCM.|Nie wymiarów|
|Outgoing.gcm.authenticationerror|Błędy uwierzytelniania usługi GCM|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptował podanych poświadczeń, poświadczenia zostały zablokowane lub element SenderId został nieprawidłowo skonfigurowany w aplikacji (wynik usługi GSM: MismatchedSenderId).|Nie wymiarów|
|Outgoing.mpns.SUCCESS|Powiadomienia usługi MPNS zakończone powodzeniem|Licznik|Łącznie|Liczba wszystkich powiadomień zakończonych powodzeniem.|Nie wymiarów|
|Outgoing.mpns.invalidcredentials|Nieprawidłowe poświadczenia usługi MPNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptował podanych poświadczeń lub poświadczenia zostały zablokowane.|Nie wymiarów|
|Outgoing.mpns.badchannel|Błąd nieprawidłowego kanału usługi MPNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ identyfikator ChannelURI w rejestracji nie został rozpoznany (stan usługi MPNS: 404 Nie znaleziono).|Nie wymiarów|
|Outgoing.mpns.throttled|Powiadomienia usługi MPNS z ograniczoną przepływnością|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ usługa MPNS ogranicza tę aplikację (WNS MPNS: 406 Niedozwolone).|Nie wymiarów|
|Outgoing.mpns.invalidnotificationformat|Nieprawidłowy format powiadomienia usługi MPNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ ładunek powiadomienia był zbyt duży.|Nie wymiarów|
|Outgoing.mpns.channeldisconnected|Rozłączono kanał usługi MPNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ identyfikator ChannelURI w rejestracji został rozłączony (stan usługi MPNS: 412 Nie znaleziono).|Nie wymiarów|
|Outgoing.mpns.dropped|Porzucone powiadomienia usługi MPNS|Licznik|Łącznie|Liczba wypchnięć porzuconych przez usługę MPNS (nagłówek odpowiedzi MPNS: X-NotificationStatus: QueueFull lub Suppressed).|Nie wymiarów|
|Outgoing.mpns.pnserror|Błędy usługi MPNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem z powodu błędów podczas komunikowania się z usługą MPNS.|Nie wymiarów|
|Outgoing.mpns.authenticationerror|Błędy uwierzytelniania usługi MPNS|Licznik|Łącznie|Liczba wypchnięć, które zakończyły się niepowodzeniem, ponieważ system powiadomień platformy nie zaakceptował podanych poświadczeń lub poświadczenia zostały zablokowane.|Nie wymiarów|
|notificationhub.pushes|Wszystkie wychodzące powiadomienia|Licznik|Łącznie|Wszystkie wychodzące powiadomienia w Centrum powiadomień|Nie wymiarów|
|Incoming.all.Requests|Wszystkie żądania przychodzące|Licznik|Łącznie|Całkowita liczba żądań przychodzących do Centrum powiadomień|Nie wymiarów|
|Incoming.all.failedrequests|Wszystkich przychodzących żądań zakończonych niepowodzeniem|Licznik|Łącznie|Całkowita liczba nieudanych żądań przychodzących dla Centrum powiadomień|Nie wymiarów|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|SearchLatency|Czas oczekiwania wyszukiwania|Sekundy|Średni|Opóźnienie średni wyszukiwania dla usługi wyszukiwania|Nie wymiarów|
|SearchQueriesPerSecond|Zapytania wyszukiwania na sekundę|CountPerSecond|Średni|Zapytania wyszukiwania na sekundę dla usługi wyszukiwania|Nie wymiarów|
|ThrottledSearchQueriesPercentage|Procent zapytań wyszukiwania z ograniczoną przepustowością|Procent|Średni|Procent zapytania wyszukiwania, które zostały ograniczany usługi wyszukiwania|Nie wymiarów|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CPUXNS|Użycie procesora na przestrzeń nazw|Procent|Maksimum|Metryka użycia procesora przestrzeni nazw w usłudze Service Bus w warstwie Premium|Nie wymiarów|
|WSXNS|Użycie rozmiaru pamięci na przestrzeń nazw|Procent|Maksimum|Metryka użycia pamięci przestrzeni nazw w usłudze Service Bus w warstwie Premium|Nie wymiarów|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent użycia procesora CPU|Procent|Średni|Procent użycia procesora CPU|Nie wymiarów|
|physical_data_read_percent|Procent użycia operacji we/wy na danych|Procent|Średni|Procent użycia operacji we/wy na danych|Nie wymiarów|
|log_write_percent|Procent we/wy dziennika|Procent|Średni|Procent we/wy dziennika|Nie wymiarów|
|dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średni|Procent użycia jednostek DTU|Nie wymiarów|
|magazyn|Rozmiar całkowitą bazy danych|Bajty|Maksimum|Rozmiar całkowitą bazy danych|Nie wymiarów|
|connection_successful|Udane połączenia|Licznik|Łącznie|Udane połączenia|Nie wymiarów|
|connection_failed|Połączenia nie powiodło się|Licznik|Łącznie|Połączenia nie powiodło się|Nie wymiarów|
|blocked_by_firewall|Blokowane przez zaporę|Licznik|Łącznie|Blokowane przez zaporę|Nie wymiarów|
|Zakleszczenie|Zakleszczenie|Licznik|Łącznie|Zakleszczenie|Nie wymiarów|
|storage_percent|Procent użycia rozmiaru bazy danych|Procent|Maksimum|Procent użycia rozmiaru bazy danych|Nie wymiarów|
|xtp_storage_percent|Procent magazynu OLTP w pamięci|Procent|Średni|Procent magazynu OLTP w pamięci|Nie wymiarów|
|workers_percent|Procent pracowników|Procent|Średni|Procent pracowników|Nie wymiarów|
|sessions_percent|Procent sesji|Procent|Średni|Procent sesji|Nie wymiarów|
|dtu_limit|Limit jednostek dtu w warstwie|Licznik|Średni|Limit jednostek dtu w warstwie|Nie wymiarów|
|dtu_used|Jednostek dtu w warstwie używane|Licznik|Średni|Jednostek dtu w warstwie używane|Nie wymiarów|
|dwu_limit|Jednostka DWU limit|Licznik|Maksimum|Jednostka DWU limit|Nie wymiarów|
|dwu_consumption_percent|Procent jednostka DWU|Procent|Maksimum|Procent jednostka DWU|Nie wymiarów|
|dwu_used|Jednostka DWU używane|Licznik|Maksimum|Jednostka DWU używane|Nie wymiarów|
|dw_cpu_percent|Poziom węzła DW procent użycia procesora CPU|Procent|Średni|Poziom węzła DW procent użycia procesora CPU|dw_logical_node_id|
|dw_physical_data_read_percent|Procent dane we/wy poziomu węzła magazynu danych|Procent|Średni|Procent dane we/wy poziomu węzła magazynu danych|dw_logical_node_id|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|cpu_percent|Procent użycia procesora CPU|Procent|Średni|Procent użycia procesora CPU|Nie wymiarów|
|database_cpu_percent|Procent użycia procesora CPU|Procent|Średni|Procent użycia procesora CPU|DatabaseResourceId|
|physical_data_read_percent|Procent użycia operacji we/wy na danych|Procent|Średni|Procent użycia operacji we/wy na danych|Nie wymiarów|
|database_physical_data_read_percent|Procent użycia operacji we/wy na danych|Procent|Średni|Procent użycia operacji we/wy na danych|DatabaseResourceId|
|log_write_percent|Procent we/wy dziennika|Procent|Średni|Procent we/wy dziennika|Nie wymiarów|
|database_log_write_percent|Procent we/wy dziennika|Procent|Średni|Procent we/wy dziennika|DatabaseResourceId|
|dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średni|Procent użycia jednostek DTU|Nie wymiarów|
|database_dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średni|Procent użycia jednostek DTU|DatabaseResourceId|
|storage_percent|Procent użycia magazynu|Procent|Średni|Procent użycia magazynu|Nie wymiarów|
|workers_percent|Procent pracowników|Procent|Średni|Procent pracowników|Nie wymiarów|
|database_workers_percent|Procent pracowników|Procent|Średni|Procent pracowników|DatabaseResourceId|
|sessions_percent|Procent sesji|Procent|Średni|Procent sesji|Nie wymiarów|
|database_sessions_percent|Procent sesji|Procent|Średni|Procent sesji|DatabaseResourceId|
|eDTU_limit|limit liczby jednostek eDTU|Licznik|Średni|limit liczby jednostek eDTU|Nie wymiarów|
|storage_limit|Limit magazynu|Bajty|Średni|Limit magazynu|Nie wymiarów|
|eDTU_used|eDTU używane|Licznik|Średni|eDTU używane|Nie wymiarów|
|storage_used|Użyty magazyn|Bajty|Średni|Użyty magazyn|Nie wymiarów|
|database_storage_used|Użyty magazyn|Bajty|Średni|Użyty magazyn|DatabaseResourceId|
|xtp_storage_percent|Procent magazynu OLTP w pamięci|Procent|Średni|Procent magazynu OLTP w pamięci|Nie wymiarów|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średni|Procent użycia jednostek DTU|ElasticPoolResourceId|
|database_dtu_consumption_percent|Procent użycia jednostek DTU|Procent|Średni|Procent użycia jednostek DTU|DatabaseResourceId, ElasticPoolResourceId|
|storage_used|Użyty magazyn|Bajty|Średni|Użyty magazyn|ElasticPoolResourceId|
|database_storage_used|Użyty magazyn|Bajty|Średni|Użyty magazyn|DatabaseResourceId, ElasticPoolResourceId|

## <a name="microsoftstoragestorageaccounts"></a>Microsoft.Storage/storageAccounts

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|UsedCapacity|Używane pojemności|Bajty|Średni|Konto używane pojemności|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|Milisekundy|Średni|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|Milisekundy|Średni|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średni|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountsblobservices"></a>Microsoft.Storage/storageAccounts/blobServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|BlobCapacity|Pojemność obiektów blob|Bajty|Średni|Ilość miejsca w magazynie używane przez usługę obiektu Blob na koncie magazynu w bajtach.|BlobType|
|Liczba obiektów blob|Liczba obiektów blob|Licznik|Średni|Liczba obiektów Blob w usłudze obiektów Blob na koncie magazynu.|BlobType|
|ContainerCount|Liczba kontenera obiektów blob|Licznik|Średni|Liczba kontenerów w usłudze obiektów Blob na koncie magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|Milisekundy|Średni|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|Milisekundy|Średni|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średni|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountstableservices"></a>Microsoft.Storage/storageAccounts/tableServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|TableCapacity|Pojemność tabeli|Bajty|Średni|Ilość miejsca w magazynie używane przez usługę tabeli konta magazynu w bajtach.|Nie wymiarów|
|TableCount|Liczba tabeli|Licznik|Średni|Liczba tabel w usłudze tabel na koncie magazynu.|Nie wymiarów|
|TableEntityCount|Liczba jednostek tabeli|Licznik|Średni|Liczba jednostek tabeli w usłudze tabel na koncie magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|Milisekundy|Średni|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|Milisekundy|Średni|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średni|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountsqueueservices"></a>Microsoft.Storage/storageAccounts/queueServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|QueueCapacity|Pojemność kolejki|Bajty|Średni|Ilość miejsca w magazynie używane przez usługę kolejki konta magazynu w bajtach.|Nie wymiarów|
|QueueCount|Liczba w kolejce|Licznik|Średni|Liczba kolejki w usłudze kolejki konta magazynu.|Nie wymiarów|
|QueueMessageCount|Liczba wiadomości w kolejce|Licznik|Średni|Przybliżoną liczbę wiadomości w kolejce w usłudze kolejki konta magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|Milisekundy|Średni|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|Milisekundy|Średni|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średni|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstoragestorageaccountsfileservices"></a>Microsoft.Storage/storageAccounts/fileServices

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|FileCapacity|Wydajność pliku|Bajty|Średni|Ilość miejsca w magazynie używane przez usługę pliku konta magazynu w bajtach.|Nie wymiarów|
|FileCount|Liczba plików|Licznik|Średni|Liczba plików w usłudze pliku konta magazynu.|Nie wymiarów|
|FileShareCount|Liczba udziału plików|Licznik|Średni|Liczba plików udziałów w usługa plików konta magazynu.|Nie wymiarów|
|Transakcje|Transakcje|Licznik|Łącznie|Liczba żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API. Liczba ta obejmuje udane i nieudane żądania, a także żądań, które wyprodukowane błędy. Wymiar wartość ResponseType używany dla wielu różnych typ odpowiedzi.|Wartość ResponseType, GeoType, Nazwa_funkcji_api|
|Zdarzenia związane z transferem danych przychodzących|Zdarzenia związane z transferem danych przychodzących|Bajty|Łącznie|Ilość danych wejściowych, w bajtach. Liczba ta obejmuje transfer danych przychodzących z zewnętrznego klienta do usługi Azure Storage, a także ruch przychodzący w obrębie platformy Azure.|GeoType, Nazwa_funkcji_api|
|Transfer danych wychodzących|Transfer danych wychodzących|Bajty|Łącznie|Ilość danych wyjściowych, w bajtach. Liczba ta obejmuje wyjście z zewnętrznego klienta do usługi Azure Storage, a także Wyjście w obrębie platformy Azure. W związku z tym ta liczba nie odzwierciedla wyjście rozliczeniowy.|GeoType, Nazwa_funkcji_api|
|SuccessServerLatency|Powodzenie opóźnienia serwera|Milisekundy|Średni|Średni czas oczekiwania, używane przez usługi Azure Storage do przetwarzania żądanie zakończy się powodzeniem (w milisekundach). Ta wartość nie obejmuje opóźnienia sieci, określona w AverageE2ELatency.|GeoType, Nazwa_funkcji_api|
|SuccessE2ELatency|Powodzenie opóźnienie E2E|Milisekundy|Średni|Średnie opóźnienie end-to-end pomyślnych żądań wysyłanych do usługi magazynu lub określonej operacji interfejsu API (w milisekundach). Ta wartość obejmuje wymagany czas przetwarzania w ramach usługi Azure Storage do odczytu żądania, wysyłania odpowiedzi i otrzyma odpowiedź.|GeoType, Nazwa_funkcji_api|
|Dostępność|Dostępność|Procent|Średni|Procent dostępności usługi magazynu lub określonej operacji interfejsu API. Dostępność jest obliczana przez pobranie wartość TotalBillableRequests i podzielenie go przez liczbę odpowiednie żądania, w tym te, które tworzone nieoczekiwane błędy. Wszystkie powodować nieoczekiwane błędy zmniejszona dostępność usługi magazynu lub określonej operacji interfejsu API.|GeoType, Nazwa_funkcji_api|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|ResourceUtilization|% wykorzystania SU|Procent|Maksimum|% wykorzystania SU|Nie wymiarów|
|InputEvents|Zdarzenia wejściowe|Licznik|Łącznie|Zdarzenia wejściowe|Nie wymiarów|
|InputEventBytes|Zdarzenia wejściowe (bajty)|Bajty|Łącznie|Zdarzenia wejściowe (bajty)|Nie wymiarów|
|LateInputEvents|Opóźnione zdarzenia wejściowe|Licznik|Łącznie|Opóźnione zdarzenia wejściowe|Nie wymiarów|
|OutputEvents|Zdarzenia wyjściowe|Licznik|Łącznie|Zdarzenia wyjściowe|Nie wymiarów|
|ConversionErrors|Błędy konwersji danych|Licznik|Łącznie|Błędy konwersji danych|Nie wymiarów|
|Błędy|Błędy w czasie wykonywania|Licznik|Łącznie|Błędy w czasie wykonywania|Nie wymiarów|
|DroppedOrAdjustedEvents|Zdarzenia poza kolejnością|Licznik|Łącznie|Zdarzenia poza kolejnością|Nie wymiarów|
|AMLCalloutRequests|Żądania funkcji|Licznik|Łącznie|Żądania funkcji|Nie wymiarów|
|AMLCalloutFailedRequests|Żądania funkcji zakończone niepowodzeniem|Licznik|Łącznie|Żądania funkcji zakończone niepowodzeniem|Nie wymiarów|
|AMLCalloutInputEvents|Zdarzenia funkcji|Licznik|Łącznie|Zdarzenia funkcji|Nie wymiarów|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CpuPercentage|Procent użycia procesora CPU|Procent|Średni|Procent użycia procesora CPU|Wystąpienie|
|MemoryPercentage|Procent pamięci|Procent|Średni|Procent pamięci|Wystąpienie|
|DiskQueueLength|Długość kolejki dysku|Licznik|Łącznie|Długość kolejki dysku|Wystąpienie|
|HttpQueueLength|Długość kolejki http|Licznik|Łącznie|Długość kolejki http|Wystąpienie|
|BytesReceived|Dane wejściowe|Bajty|Łącznie|Dane wejściowe|Wystąpienie|
|Żądania|Dane wyjściowe|Bajty|Łącznie|Dane wyjściowe|Wystąpienie|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (z wyjątkiem funkcji)

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CpuTime|Czas procesora CPU|Sekundy|Łącznie|Czas procesora CPU|Wystąpienie|
|Żądania|Żądania|Licznik|Łącznie|Żądania|Wystąpienie|
|BytesReceived|Dane wejściowe|Bajty|Łącznie|Dane wejściowe|Wystąpienie|
|Żądania|Dane wyjściowe|Bajty|Łącznie|Dane wyjściowe|Wystąpienie|
|Http101|HTTP 101|Licznik|Łącznie|HTTP 101|Wystąpienie|
|Http2xx|Http 2xx|Licznik|Łącznie|Http 2xx|Wystąpienie|
|Http3xx|Http 3xx|Licznik|Łącznie|Http 3xx|Wystąpienie|
|Http401|HTTP 401|Licznik|Łącznie|HTTP 401|Wystąpienie|
|Http403|HTTP 403|Licznik|Łącznie|HTTP 403|Wystąpienie|
|Http404|HTTP 404|Licznik|Łącznie|HTTP 404|Wystąpienie|
|Http406|HTTP 406|Licznik|Łącznie|HTTP 406|Wystąpienie|
|Http4xx|HTTP 4xx|Licznik|Łącznie|HTTP 4xx|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajty|Średni|Zestaw roboczy pamięci|Wystąpienie|
|AverageMemoryWorkingSet|Pamięć średni zestaw roboczy|Bajty|Średni|Pamięć średni zestaw roboczy|Wystąpienie|
|AverageResponseTime|Średni czas odpowiedzi|Sekundy|Średni|Średni czas odpowiedzi|Wystąpienie|
|FunctionExecutionUnits|Funkcja wykonywania jednostki|Licznik|Średni|Funkcja wykonywania jednostki|Wystąpienie|
|FunctionExecutionCount|Liczba wykonań funkcji|Licznik|Średni|Liczba wykonań funkcji|Wystąpienie|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funkcje)

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|BytesReceived|Dane wejściowe|Bajty|Łącznie|Dane wejściowe|Wystąpienie|
|Żądania|Dane wyjściowe|Bajty|Łącznie|Dane wyjściowe|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajty|Średni|Zestaw roboczy pamięci|Wystąpienie|
|AverageMemoryWorkingSet|Pamięć średni zestaw roboczy|Bajty|Średni|Pamięć średni zestaw roboczy|Wystąpienie|
|FunctionExecutionUnits|Funkcja wykonywania jednostki|Licznik|Średni|Funkcja wykonywania jednostki|Wystąpienie|
|FunctionExecutionCount|Liczba wykonań funkcji|Licznik|Średni|Liczba wykonań funkcji|Wystąpienie|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|CpuTime|Czas procesora CPU|Sekundy|Łącznie|Czas procesora CPU|Wystąpienie|
|Żądania|Żądania|Licznik|Łącznie|Żądania|Wystąpienie|
|BytesReceived|Dane wejściowe|Bajty|Łącznie|Dane wejściowe|Wystąpienie|
|Żądania|Dane wyjściowe|Bajty|Łącznie|Dane wyjściowe|Wystąpienie|
|Http101|HTTP 101|Licznik|Łącznie|HTTP 101|Wystąpienie|
|Http2xx|Http 2xx|Licznik|Łącznie|Http 2xx|Wystąpienie|
|Http3xx|Http 3xx|Licznik|Łącznie|Http 3xx|Wystąpienie|
|Http401|HTTP 401|Licznik|Łącznie|HTTP 401|Wystąpienie|
|Http403|HTTP 403|Licznik|Łącznie|HTTP 403|Wystąpienie|
|Http404|HTTP 404|Licznik|Łącznie|HTTP 404|Wystąpienie|
|Http406|HTTP 406|Licznik|Łącznie|HTTP 406|Wystąpienie|
|Http4xx|HTTP 4xx|Licznik|Łącznie|HTTP 4xx|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|MemoryWorkingSet|Zestaw roboczy pamięci|Bajty|Średni|Zestaw roboczy pamięci|Wystąpienie|
|AverageMemoryWorkingSet|Pamięć średni zestaw roboczy|Bajty|Średni|Pamięć średni zestaw roboczy|Wystąpienie|
|AverageResponseTime|Średni czas odpowiedzi|Sekundy|Średni|Średni czas odpowiedzi|Wystąpienie|
|FunctionExecutionUnits|Funkcja wykonywania jednostki|Licznik|Średni|Funkcja wykonywania jednostki|Wystąpienie|
|FunctionExecutionCount|Liczba wykonań funkcji|Licznik|Średni|Liczba wykonań funkcji|Wystąpienie|

## <a name="microsoftwebhostingenvironmentsmultirolepools"></a>Microsoft.Web/hostingEnvironments/multiRolePools

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Żądania|Żądania|Licznik|Łącznie|Żądania|Wystąpienie|
|BytesReceived|Dane wejściowe|Bajty|Łącznie|Dane wejściowe|Wystąpienie|
|Żądania|Dane wyjściowe|Bajty|Łącznie|Dane wyjściowe|Wystąpienie|
|Http101|HTTP 101|Licznik|Łącznie|HTTP 101|Wystąpienie|
|Http2xx|Http 2xx|Licznik|Łącznie|Http 2xx|Wystąpienie|
|Http3xx|Http 3xx|Licznik|Łącznie|Http 3xx|Wystąpienie|
|Http401|HTTP 401|Licznik|Łącznie|HTTP 401|Wystąpienie|
|Http403|HTTP 403|Licznik|Łącznie|HTTP 403|Wystąpienie|
|Http404|HTTP 404|Licznik|Łącznie|HTTP 404|Wystąpienie|
|Http406|HTTP 406|Licznik|Łącznie|HTTP 406|Wystąpienie|
|Http4xx|HTTP 4xx|Licznik|Łącznie|HTTP 4xx|Wystąpienie|
|Http5xx|Błędy HTTP serwera|Licznik|Łącznie|Błędy HTTP serwera|Wystąpienie|
|AverageResponseTime|Średni czas odpowiedzi|Sekundy|Średni|Średni czas odpowiedzi|Wystąpienie|
|CpuPercentage|Procent użycia procesora CPU|Procent|Średni|Procent użycia procesora CPU|Wystąpienie|
|MemoryPercentage|Procent pamięci|Procent|Średni|Procent pamięci|Wystąpienie|
|DiskQueueLength|Długość kolejki dysku|Licznik|Łącznie|Długość kolejki dysku|Wystąpienie|
|HttpQueueLength|Długość kolejki http|Licznik|Łącznie|Długość kolejki http|Wystąpienie|
|ActiveRequests|Aktywne żądania|Licznik|Łącznie|Aktywne żądania|Wystąpienie|
|TotalFrontEnds|Całkowita liczba przedniego kończy się|Licznik|Średni|Całkowita liczba przedniego kończy się|Wystąpienie|
|SmallAppServicePlanInstances|Pracowników planu usługi aplikacji — małe|Licznik|Średni|Pracowników planu usługi aplikacji — małe|Wystąpienie|
|MediumAppServicePlanInstances|Pracownicy planu usługi aplikacji — średnia|Licznik|Średni|Pracownicy planu usługi aplikacji — średnia|Wystąpienie|
|LargeAppServicePlanInstances|Pracownicy planu usługi aplikacji — duże|Licznik|Średni|Pracownicy planu usługi aplikacji — duże|Wystąpienie|

## <a name="microsoftwebhostingenvironmentsworkerpools"></a>Microsoft.Web/hostingEnvironments/workerPools

|Metryka|Nazwa wyświetlana metryki|Jednostka|Typ agregacji|Opis|Wymiary|
|---|---|---|---|---|---|
|Łączna|Całkowita liczba procesów roboczych|Licznik|Średni|Całkowita liczba procesów roboczych|Wystąpienie|
|WorkersAvailable|Dostępne pracowników|Licznik|Średni|Dostępne pracowników|Wystąpienie|
|WorkersUsed|Używane pracowników|Licznik|Średni|Używane pracowników|Wystąpienie|

## <a name="next-steps"></a>Następne kroki
* [Przeczytaj informacje o metryki w monitorze Azure](monitoring-overview-metrics.md)
* [Utwórz alerty na metryk](insights-receive-alert-notifications.md)
* [Eksportuj metryk do magazynu, Centrum zdarzeń lub analizy dzienników](monitoring-overview-of-diagnostic-logs.md)
