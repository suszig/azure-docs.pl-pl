---
title: "Obsługa kolejność zdarzeń i opóźnienie z usługą Azure Stream Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się więcej na temat działania usługi analiza strumienia poza kolejnością lub opóźnione zdarzenia w strumieniach danych."
keywords: "zdarzenia poza kolejnością, późne,"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 04/20/2017
ms.author: samacha
ms.openlocfilehash: cf57bd12a62b3de8ac49b26ce7cdc40aec0b6738
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-stream-analytics-event-order-handling"></a>Obsługa kolejność zdarzeń w usłudze Azure Stream Analytics

W strumieniu danych czasowych zdarzeń każdego zdarzenia jest przypisany sygnatury czasowej. Usługa Azure Stream Analytics przypisuje sygnatury czasowej każdego zdarzenia przy użyciu Godzina nadejścia lub czasu aplikacji. Kolumna "System.Timestamp" ma sygnaturę czasową przypisany do zdarzenia. Godzina nadejścia jest przypisywany w źródła danych wejściowych, gdy zdarzenie osiągnie źródła. Godzina nadejścia jest EventEnqueuedTime reakcję Centrum zdarzeń i [obiektu blob Data ostatniej modyfikacji](https://docs.microsoft.com/en-us/dotnet/api/microsoft.windowsazure.storage.blob.blobproperties.lastmodified?view=azurestorage-8.1.3) dla obiekt blob danych wejściowych. Czas aplikacji jest przypisany, gdy zostanie wygenerowane zdarzenie i jest częścią ładunku. Do przetworzenia zdarzenia według czasu aplikacji, użyj klauzuli "Sygnatury czasowej przez" w wykonywania zapytania select. Jeśli w klauzuli "Sygnatury czasowej przez" jest nieobecne, zdarzenia są przetwarzane przez Godzina nadejścia. Godzina nadejścia jest możliwy przy użyciu właściwości EventEnqueuedTime dla Centrum zdarzeń i właściwości BlobLastModified dla obiekt blob danych wejściowych. Usługa Azure Stream Analytics generuje dane wyjściowe w kolejności znaczników czasu i udostępnia kilka ustawień radzenia sobie z danych poza kolejnością.

![Obsługa zdarzeń usługi analiza strumienia](media/stream-analytics-event-handling/stream-analytics-event-handling.png)

Strumienie wejściowe, które nie znajdują się w kolejności są:
* Sortowane (i w związku z tym **opóźnione**).
* Dostosowane przez system, zgodnie z zasadami określonymi przez użytkownika.

Analiza strumienia zaakceptować późne i poza kolejnością zdarzenia podczas przetwarzania przez czas aplikacji.

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

Późne tolerancji przyjęcia = 10 minut<br/>
Poza tolerancji kolejności = 3 minuty<br/>
Przetwarzanie przez czas aplikacji<br/>

Wydarzenia:

Zdarzenia 1 _czas aplikacji_ = 00:00:00, _Godzina nadejścia_ = 00:10:01 _System.Timestamp_ = 00:00:01, dostosowane, ponieważ (_Godzina nadejścia_  -  _Czas aplikacji_) jest większa niż późne tolerancji przyjęcia.

Zdarzenia 2 _czas aplikacji_ = 00:00:01 _Godzina nadejścia_ = 00:10:01 _System.Timestamp_ = 00:00:01, nie dostosowana, ponieważ w ramach spóźnione otrzymanie czas aplikacji okno.

Zdarzenia 3 _czas aplikacji_ = 00:10:00, _Godzina nadejścia_ = 00:10:02 _System.Timestamp_ = 00:10:00, nie dostosowana, ponieważ w oknie przyjęcia późne czas aplikacji .

Zdarzenie 4 _czas aplikacji_ = 00:09:00, _Godzina nadejścia_ = 00:10:03 _System.Timestamp_ = 00:09:00, zaakceptowane z oryginalnego sygnatury czasowej w granicach poza czas aplikacji kolejność na uszkodzenia.

Zdarzenie 5 _czas aplikacji_ = 00:06:00, _Godzina nadejścia_ = 00:10:04 _System.Timestamp_ = 00:07:00, dostosowane, ponieważ czas aplikacji jest starsza niż poza kolejnością Tolerancja.



## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dodatkową pomoc, spróbuj naszych [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi analiza strumienia](stream-analytics-introduction.md)
* [Wprowadzenie do usługi analiza strumienia](stream-analytics-real-time-fraud-detection.md)
* [Zadania usługi analiza strumienia skali](stream-analytics-scale-jobs.md)
* [Dokumentacja języka zapytania usługi analiza strumienia](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Strumienia Analytics management REST API reference](https://msdn.microsoft.com/library/azure/dn835031.aspx)
