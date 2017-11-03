---
title: "Zadania usługi analiza strumienia opis monitorowania | Dokumentacja firmy Microsoft"
description: "Opis zadania usługi analiza strumienia monitorowania"
keywords: monitor kwerendy
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 5f5cc00f-4a7b-491e-89e1-dbafea46d399
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 12545dd3875e81f8f2248acceb66d2d840cf6702
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="understand-stream-analytics-job-monitoring-and-how-to-monitor-queries"></a>Informacje o monitorowaniu zadania usługi analiza strumienia i jak monitorować zapytań

## <a name="introduction-the-monitor-page"></a>Wprowadzenie: Strona monitorowania
Azure portalu, zarówno powierzchni metryki wydajności, który może służyć do monitorowania i rozwiązywania problemów z wydajność zapytań i zadania. Aby wyświetlić te metryki, przejdź do zadania usługi analiza strumienia są wyświetlenie metryki i wyświetlić **monitorowanie** sekcji na stronie przeglądu.  

![Monitorowanie łącza](./media/stream-analytics-monitoring/02-stream-analytics-monitoring-block.png)

Zostanie wyświetlone okno, jak pokazano:

![Monitorowanie zadań pulpitu nawigacyjnego](./media/stream-analytics-monitoring/01-stream-analytics-monitoring.png)  

## <a name="metrics-available-for-stream-analytics"></a>Metryki dostępne dla usługi analiza strumienia
| Metryka                 | Definicja                               |
| ---------------------- | ---------------------------------------- |
| SU % wykorzystania       | Wykorzystanie jednostek przesyłania strumieniowego przypisany do zadania z kartę Skala zadania. Jeżeli ten wskaźnik osiągnie 80%, lub powyżej, istnieje wysokie prawdopodobieństwo, czy przetwarzanie zdarzeń może zostać opóźnione lub zatrzymana czyni postępy. |
| Zdarzenia wejściowe           | Ilość danych odebranych przez zadanie usługi Stream Analytics, liczbę zdarzeń. Może być używany do sprawdzania, czy zdarzenia są wysyłane do źródła danych wejściowych. |
| Dane wyjściowe zdarzenia          | Ilość danych przesyłanych przez zadanie usługi Stream Analytics do obiektu docelowego dane wyjściowe w liczby zdarzeń. |
| Zdarzenia poza kolejnością.    | Liczba zdarzeń odebranych poza kolejnością, które zostały porzucone lub podane skorygowaną sygnatury czasowej, opierając się na zasadzie kolejności zdarzeń. Może to mieć wpływ konfigurację ustawienia porządku poza tolerancji. |
| Błędy konwersji danych | Liczba błędów konwersji danych poniesionych przez zadanie usługi Stream Analytics. |
| Błędy środowiska wykonawczego         | Całkowita liczba błędów, które mają miejsce podczas wykonywania zadania usługi analiza strumienia. |
| Opóźnione zdarzenia wejściowe      | Liczba zdarzeń przybywających późne ze źródła, którego albo została porzucona lub ich sygnatury czasowej została zmieniona na prawidłową, na podstawie konfiguracji zasady kolejność zdarzeń ustawienie późne przyjęcia tolerancji. |
| Funkcja żądania      | Liczba wywołań funkcji usługi Azure Machine Learning (jeśli istnieje). |
| Żądania funkcji zakończonych niepowodzeniem | Liczba zakończonych niepowodzeniem wywołania funkcji usługi Azure Machine Learning (jeśli istnieje). |
| Zdarzenia — funkcja        | Liczba zdarzeń wysłanych do funkcji usługi Azure Machine Learning (jeśli istnieje). |
| Zdarzenie wejściowe w bajtach      | Ilość danych odebranych przez zadanie usługi Stream Analytics, w bajtach. Może być używany do sprawdzania, czy zdarzenia są wysyłane do źródła danych wejściowych. |


## <a name="customizing-monitoring-in-the-azure-portal"></a>Dostosowywanie monitorowania w portalu Azure
Można dopasować typ wykresu, metryki wyświetlane i zakres w ustawieniach Edytuj wykres czasu. Aby uzyskać więcej informacji, zobacz [jak dostosować monitorowanie](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

  ![Wykres monitorowania godziny zapytań](./media/stream-analytics-monitoring/08-stream-analytics-monitoring.png)  


## <a name="latest-output"></a>Najnowsze dane wyjściowe
Innego interesujące punkt danych, aby monitorować zadanie po raz ostatni dane wyjściowe, wyświetlany na stronie przeglądu.
Ten czas jest czas aplikacji (tj. czas za pomocą sygnatury czasowej ze źródła danych zdarzeń) najnowsze dane wyjściowe zadania.

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

