---
title: "Jak napisać zapytań w Stream Analytics | Dokumentacja firmy Microsoft"
description: "Zapisywanie zapytań w Stream Analytics i dane zapytań | Learning segmentu ścieżki."
keywords: "jak napisać zapytanie danych zapytania, napisz zapytanie, Pisanie zapytań"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 0e9cdadd-0ee0-4bee-b65b-4a06fb863c95
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 215b774c20d80a67b1cefa2634131bd44860c692
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-write-queries-in-stream-analytics"></a>Jak napisać zapytanie w Stream Analytics
Pisanie zapytań dla strumienia przetwarzania logiki w usłudze Azure Stream Analytics jest implementowany jako "zapytania stały", zdefiniowanej przed zadanie uruchamia i wykonać na danych, ponieważ osiągnie zadania. Przekształcenia danych jest wyrażona w język kwerendy przypominającego SQL, które przede wszystkim podzbiór T-SQL wraz z dodawanej rozszerzeń języka, na przykład [okien](https://msdn.microsoft.com/library/azure/dn835019.aspx) stosowany w celu semantyki danych czasowych.

## <a name="writing-queries"></a>Pisanie zapytań:
1. W Twojej zadania usługi analiza strumienia w portalu Azure kliknij **zapytania**.
   
    ![Wybierz zapytanie](./media/stream-analytics-write-queries/1-stream-analytics-write-queries.png)  
   
    W portalu Azure kliknij **zapytania**.
   
    ![Wybierz Podgląd kwerendy](./media/stream-analytics-write-queries/query-preview-portal.png)  
2. Nowe zadania ma szablonu zapytania do pomogą Ci rozpocząć pracę. Szablon kwerendy wykonuje zapytanie "przekazujący" projektów wszystkie pola z zdarzenia wejściowe w danych wyjściowych.  
   
   * Jeśli zdefiniowano co najmniej jeden dane wejściowe i wyjściowe dla zadania, można zastąpić symbolu zastępczego "[YourOutputAlias]" i pola "[YourInputAlias]" z aliasami programu przychodzących i wychodzących, które mają używać najpierw. Ponadto nadal tworzyć i przetestować zapytanie w klasycznym portalu Azure bez zdefiniowania wejściami i wyjściami w zadaniu.
   * Jeśli chcesz wykonać przetwarzanie więcej niż proste przekazującego, można edytować definicji zapytania. Aby rozpocząć tworzenie zapytań, Przyjrzyjmy się niektóre typowe zapytania są przechwytywane wzorce [tutaj](stream-analytics-stream-analytics-query-patterns.md).  
   
   ![Dane zapytania okna](./media/stream-analytics-write-queries/2-stream-analytics-write-queries.png)  

## <a name="to-validate-query-data-is-working"></a>Aby sprawdzić poprawność danych zapytania działa:
Można sprawdzić, czy zapytanie działa zgodnie z oczekiwaniami przez uruchomienie jej w przeglądarce za pośrednictwem jednego lub więcej lokalne pliki w formacie JSON zawierający dane testowe. To nie będzie uruchomić zadanie lub mają wpływ na wszystkie rozliczeń.

> [!NOTE]
> Obecnie testowanie zapytań w przeglądarce nie jest obsługiwana w portalu Azure.  
> 
> 

1. Upewnij się, że nie ma żadnych błędów w zapytaniu (w przeciwnym razie przycisk Test będzie wyłączony), a następnie kliknij przycisk Test.  
   
   ![Dane zapytania testowego](./media/stream-analytics-write-queries/3-stream-analytics-write-queries.png)  
2. Monit, aby określić pliki dla poszczególnych składników, do którego odwołuje się zapytanie. W tym przykładzie zapytanie szablonu pozostanie jako — jest, więc okna dialogowego monit o danych wejściowych o nazwie "yourinputalias".
   
   ![Badanie danych zapytania](./media/stream-analytics-write-queries/4-stream-analytics-write-queries.png)  
3. Przejdź do pliku testu. Kilka przykładowych plików są dostępne na [github](https://github.com/Azure/azure-stream-analytics/tree/master/Sample Data) i może również pobierać dane przykładowe z własnych danych wejściowych strumienia danych za pośrednictwem funkcji przykładowych danych na karcie dane wejściowe.
   
   ![Zapytanie danych wejściowych](./media/stream-analytics-write-queries/5-stream-analytics-write-queries.png)  
4. Po zamknięciu okna dialogowego, zapytanie zostanie uruchomiony za pośrednictwem danych testowych i będą widoczne wyniki w dolnej części strony zapytania.
   
   ![Podsumowanie zapytania](./media/stream-analytics-write-queries/6-stream-analytics-write-queries.png)  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

