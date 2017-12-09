---
title: "Obsługa kolejność zdarzeń i opóźnienie z usługą Azure Stream Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej na temat działania usługi analiza strumienia poza kolejnością lub opóźnione zdarzenia w strumieniach danych."
keywords: "zdarzenia poza kolejnością, późne,"
documentationcenter: 
services: stream-analytics
author: jseb225
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: jeanb
ms.openlocfilehash: f9854172e08785676a7804433d9a559e623a7b05
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="azure-stream-analytics-event-order-consideration"></a>Azure Stream Analytics zdarzeń kolejności brany pod uwagę

## <a name="understand-arrival-time-and-application-time"></a>Dowiedz się, Godzina nadejścia i czas aplikacji.

W strumieniu danych czasowych zdarzeń każdego zdarzenia jest przypisany sygnatury czasowej. Usługa Azure Stream Analytics przypisuje sygnatury czasowej każdego zdarzenia przy użyciu Godzina nadejścia lub czasu aplikacji. Kolumna "System.Timestamp" ma sygnaturę czasową przypisany do zdarzenia. Godzina nadejścia jest przypisywany w źródła danych wejściowych, gdy zdarzenie osiągnie źródła. Godzina nadejścia jest EventEnqueuedTime reakcję Centrum zdarzeń i [obiektu blob Data ostatniej modyfikacji](https://docs.microsoft.com/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) dla obiekt blob danych wejściowych. Czas aplikacji jest przypisany, gdy zostanie wygenerowane zdarzenie i jest częścią ładunku. Do przetworzenia zdarzenia według czasu aplikacji, użyj klauzuli "Sygnatury czasowej przez" w wykonywania zapytania select. Jeśli w klauzuli "Sygnatury czasowej przez" jest nieobecne, zdarzenia są przetwarzane przez Godzina nadejścia. Godzina nadejścia jest możliwy przy użyciu właściwości EventEnqueuedTime dla Centrum zdarzeń i właściwości BlobLastModified dla obiekt blob danych wejściowych. Usługa Azure Stream Analytics generuje dane wyjściowe w kolejności znaczników czasu i udostępnia kilka ustawień radzenia sobie z danych poza kolejnością.


## <a name="azure-stream-analytics-handling-of-multiple-streams"></a>Azure Stream Analytics obsługę wielu strumieni

Zadania usługi analiza strumienia Azure łączy zdarzenia z wielu osi czasu w kilku przypadkach, także w przypadku,

* Generuje danych wyjściowych z wielu partycji. Łączenie zdarzeń z wszystkich partycji musi zapytań, które nie mają explicit "partycja przez PartitionId".
* Złożenie dwóch lub więcej różnych źródeł danych wejściowych.
* Sprzęganie źródeł danych wejściowych.

W scenariuszach, w którym są łączone wiele osi czasu, usługi Azure Stream Analytics będzie generowania danych wyjściowych dla sygnatury czasowej *t1* tylko wtedy, gdy wszystkie źródła, które są łączone są co najmniej w czasie *t1*.
Na przykład, jeśli zapytanie odczytuje z *Centrum zdarzeń* partycji, która ma dwie partycje i jedną z partycji *P1* ma zdarzeń do czasu *t1* i innych partycji  *P2* ma zdarzeń do czasu *t1 + x*, do czasu wygenerowania danych wyjściowych *t1*.
Jednak jeśli został jawnie *"Partycja przez PartitionId"* postępów zarówno partycji w klauzuli, niezależnie.
Późne przyjęcia tolerancja służy do postępowania w przypadku braku danych w niektórych partycji.

## <a name="configuring-late-arrival-tolerance-and-out-of-order-tolerance"></a>Konfigurowanie późne tolerancji przyjęcia i odporność na awarie poza kolejnością
Strumienie wejściowe, które nie znajdują się w kolejności są:
* Sortowane (i w związku z tym **opóźnione**).
* Dostosowane przez system, zgodnie z zasadami określonymi przez użytkownika.

Analiza strumienia zaakceptować późne i poza kolejnością zdarzenia podczas przetwarzania przez **czas aplikacji**. Następujące ustawienia są dostępne w **kolejność zdarzeń** opcji w portalu Azure: 

![Obsługa zdarzeń usługi analiza strumienia](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

**Późne tolerancji przyjęcia**
* To ustawienie ma zastosowanie tylko wtedy, gdy przetwarzania przez czas aplikacji, w przeciwnym razie zostanie zignorowana.
* Jest to maksymalna różnica między Godzina nadejścia i czas aplikacji. Jeśli czas aplikacji przed (Godzina nadejścia - późne okna odbioru), ustawiono (Godzina nadejścia - późne okna odbioru)
* Gdy wiele partycji z tego samego strumienia wejściowego lub wiele strumieni wejściowych są połączone ze sobą, późne tolerancji przyjęcia jest maksymalną ilość czasu oczekiwania każdej partycji dla nowych danych. 

Krótko mówiąc późne okno odbioru jest maksymalne opóźnienie od wygenerowania zdarzenia do odbierania zdarzeń w źródła danych wejściowych.
Dostosowanie oparte na opóźnione tolerancji przyjęcia odbywa się najpierw i poza kolejnością odbywa się obok. **System.Timestamp** kolumny będzie mieć końcowego sygnatury czasowej przypisany do zdarzenia.

**Tolerancja poza kolejnością**
* Zdarzenia przychodzące poza kolejnością, ale w zestawie "poza kolejnością tolerancji" są **ponownie uporządkowane według znaczników czasu**. 
* Zdarzenia przychodzące później niż uszkodzenia są **porzucony albo dostosowana**.
    * **Dostosowana**: dostosowana do wydają się przybyły najnowszych akceptowalnego czasu. 
    * **Porzucone**: odrzucone.

Aby zmienić kolejność zdarzeń w ciągu "poza kolejnością tolerancji", jest wyniki kwerendy **opóźniony o poza kolejnością tolerancji**.

**Przykład**

* Późne tolerancji przyjęcia = 10 minut<br/>
* Poza tolerancji kolejności = 3 minuty<br/>
* Przetwarzanie przez czas aplikacji<br/>
* Wydarzenia:
   * Zdarzenia 1 _czas aplikacji_ = 00:00:00, _Godzina nadejścia_ = 00:10:01 _System.Timestamp_ = 00:00:01, dostosowane, ponieważ (_Godzina nadejścia_  -  _Czas aplikacji_) jest większa niż późne tolerancji przyjęcia.
   * Zdarzenia 2 _czas aplikacji_ = 00:00:01 _Godzina nadejścia_ = 00:10:01 _System.Timestamp_ = 00:00:01, nie dostosowana, ponieważ w ramach spóźnione otrzymanie czas aplikacji okno.
   * Zdarzenia 3 _czas aplikacji_ = 00:10:00, _Godzina nadejścia_ = 00:10:02 _System.Timestamp_ = 00:10:00, nie dostosowana, ponieważ w oknie przyjęcia późne czas aplikacji .
   * Zdarzenie 4 _czas aplikacji_ = 00:09:00, _Godzina nadejścia_ = 00:10:03 _System.Timestamp_ = 00:09:00, zaakceptowane z oryginalnego sygnatury czasowej w granicach poza czas aplikacji kolejność na uszkodzenia.
   * Zdarzenie 5 _czas aplikacji_ = 00:06:00, _Godzina nadejścia_ = 00:10:04 _System.Timestamp_ = 00:07:00, dostosowane, ponieważ czas aplikacji jest starsza niż poza kolejnością Tolerancja.

## <a name="practical-considerations"></a>Kwestie praktyczne
Jak wspomniano powyżej, *późne tolerancji przyjęcia* jest maksymalną różnicę czasu aplikacji i Godzina nadejścia.
Również przetwarzania przez aplikację do czasu, zdarzenia, które są nowsze niż skonfigurowanego *późne tolerancji przyjęcia* są ustawiane przed *tolerancji poza kolejnością* ustawienie jest stosowane. Tak minimalnym późne tolerancji przyjęcia i odporność poza kolejnością jest obowiązującej poza kolejnością.

Poza kolejnością zdarzeń z przyczyn, w tym, w ramach strumienia
* Niedokładność zegara między nadawcy.
* Zmienne opóźnienie między nadawcą i źródło zdarzenia wejściowe.

Późne przyjęcia się dzieje z przyczyn, w tym,
* Nadawców partii i wysłać zdarzenia dla interwału później, po upływie interwału.
* Opóźnienie między wysyłanie zdarzeń przez nadawcę i odbieranie zdarzeń na źródło danych wejściowych.

Podczas konfigurowania *późne tolerancji przyjęcia* i *poza kolejnością tolerancji* dla danego zadania, poprawności, opóźnienia i powyżej czynników należy traktować jako.

Poniżej przedstawiono kilka przykładów

### <a name="example-1"></a>Przykład 1: 
Zapytanie zawiera klauzulę "Partycja przez PartitionId" i w obrębie jednej partycji, zdarzenia są wysyłane przy użyciu metod synchronicznych wysyłania. Synchroniczne wysłać bloku metody do momentu zdarzenia są wysyłane.
W takim przypadku poza kolejnością wynosi zero, ponieważ zdarzenia są wysyłane w kolejności z jawnym potwierdzenie przed wysłaniem następne zdarzenie. Późne przyjęcia jest maksymalne opóźnienie między generowania zdarzenia i wysyłanie zdarzeń + maksymalnego czasu oczekiwania między nadawcą i źródła danych wejściowych

### <a name="example-2"></a>Przykład 2:
Zapytanie zawiera klauzulę "Partycja przez PartitionId" i w obrębie jednej partycji, zdarzenia są wysyłane przy użyciu metody asynchronicznego wysyłania. Metody asynchronicznego wysyłania można inicjować wielu wysyła w tym samym czasie, co może powodować zdarzenia poza kolejnością.
W takim przypadku przyjęcia zarówno poza kolejnością i późne są co najmniej maksymalne opóźnienie między generowania zdarzenia i wysyłanie zdarzeń + maksymalnego czasu oczekiwania między nadawcą i źródła danych wejściowych.

### <a name="example-3"></a>Przykład 3:
Zapytanie nie ma "Partycja przez PartitionId" i istnieją co najmniej dwie partycje.
Konfiguracja jest taki sam jak przykład 2. Jednak brak danych w jednej partycji można opóźnić dane wyjściowe o dodatkowe * późne tolerancji przyjęcia "okna.

## <a name="to-summarize"></a>Podsumowując
* Późne tolerancji przyjęcia poza kolejnością okno powinno być skonfigurowane w oparciu poprawności, opóźnienia i należy również rozważyć, jak zdarzenia są wysyłane.
* Zaleca się, że poza kolejnością na uszkodzenia jest mniejszy niż późne tolerancji przyjęcia.
* W przypadku łączenia wielu osi czasu, Brak danych w jednym źródeł lub partycji może opóźnić danych wyjściowych przez dodatkowe późne przyjęcia tolerancji.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc, spróbuj naszych [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi analiza strumienia](stream-analytics-introduction.md)
* [Wprowadzenie do usługi analiza strumienia](stream-analytics-real-time-fraud-detection.md)
* [Zadania usługi analiza strumienia skali](stream-analytics-scale-jobs.md)
* [Dokumentacja języka zapytania usługi analiza strumienia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Strumienia Analytics management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
