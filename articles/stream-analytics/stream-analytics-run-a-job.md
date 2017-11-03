---
title: "Jak uruchomić przesyłanie strumieniowe zadania w Stream Analytics | Dokumentacja firmy Microsoft"
description: "Jak uruchomić zadanie przesyłania strumieniowego w Azure Stream Analytics | Learning segmentu ścieżki."
keywords: "zadania przesyłania strumieniowego"
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 9d46950f-2b69-49ce-a567-df558c5dd820
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: fb084f1c6b53e2582849e71271e8114a22dcf05c
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="how-to-run-a-streaming-job-in-azure-stream-analytics"></a>Jak uruchomić zadanie przesyłania strumieniowego w programie Azure Stream Analytics
Gdy zadanie danych wejściowych, zapytań i danych wyjściowych wszystkie określono się, że można uruchomić zadania Stream Analytics.

Aby rozpocząć zadania:

1. W klasycznym portalu Azure, z poziomu pulpitu nawigacyjnego zadania, kliknij przycisk **Start** w dolnej części strony.
   
   ![Uruchom zadanie przycisku](./media/stream-analytics-run-a-job/1-stream-analytics-run-a-job.png)  
   
   W portalu Azure kliknij **Start** w górnej części strony swojego zadania.
   
   ![Azure portalu rozpoczęcia zadania przycisku](./media/stream-analytics-run-a-job/4-stream-analytics-run-a-job.png)  
2. Określ **Start wyjściowy** wartość, aby określić, kiedy to zadanie zostanie uruchomione, generuje danych wyjściowych. Ustawieniem domyślnym dla zadania, które wcześniej nie została uruchomiona jest **czas rozpoczęcia zadania**, co oznacza, że zadanie będzie natychmiast rozpocząć przetwarzania danych. Można również określić **niestandardowy** czas w przeszłości (służący do konsumowania danych historycznych) lub w przyszłości (Aby opóźnić proces przetwarzania do przyszłych czasu). W przypadkach, gdy zadanie została wcześniej uruchomiona i zatrzymana, opcja **czas ostatniego zatrzymania** jest dostępny w celu wznowienia zadania od ostatniego danych wyjściowych i uniknąć utraty danych.  

> [!NOTE]
> Korzystając z partycji, czas ostatniego zatrzymania reprezentuje co najmniej ostatniego dane wyjściowe wszystkich partycji.
   
   ![Przesyłanie strumieniowe zadania czas rozpoczęcia](./media/stream-analytics-run-a-job/2-stream-analytics-run-a-job.png)  
   
   ![Azure portalu Start zadanie przesyłania strumieniowego czasu](./media/stream-analytics-run-a-job/5-stream-analytics-run-a-job.png)  
3. Potwierdź wybór. Stan zadania zmieni się na *uruchamianie* wkrótce zostanie przeniesiony do *systemem* po rozpoczęciu zadania. Możesz monitorować postęp **Start** operacji w **Centrum powiadomień**:
   
   ![postęp zadania przesyłania strumieniowego](./media/stream-analytics-run-a-job/3-stream-analytics-run-a-job.png)  
   
   ![Portal Azure przesyłania strumieniowego postęp zadania](./media/stream-analytics-run-a-job/6-stream-analytics-run-a-job.png)  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

