---
title: "Jak utworzyć zadanie przetwarzania analizy danych dla usługi Stream Analytics | Dokumentacja firmy Microsoft"
description: "Utwórz zadanie przetwarzania analizy danych dla usługi Stream Analytics | Learning segmentu ścieżki."
keywords: przetwarzanie analizy danych
documentationcenter: 
services: stream-analytics
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: e825fbcf-69e9-443f-b402-3b7a4568f415
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 05fdf1e20efd129cdfc27e1d37bc9e124edf5dcd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Jak utworzyć zadanie przetwarzania analizy danych dla usługi analiza strumienia
Zasób najwyższego poziomu w Azure Stream Analytics jest zadania usługi analiza strumienia.  Składa się z co najmniej jedno źródło danych wejściowych, zapytania, przedstawiając przekształcania danych i co najmniej jeden wyjściowy obiektów docelowych, które wyniki są zapisywane do. Razem te umożliwiają użytkownikowi wykonywać analizy danych przetwarzania do strumieniowego przesyłania danych.

Aby rozpocząć korzystanie z usługi Stream Analytics, Rozpocznij od utworzenia nowego zadania usługi analiza strumienia.  Należy zauważyć, że ta akcja nie daje żadnych skutków rozliczeń do momentu rozpoczęcia zadania.

1. Zaloguj się na online [klasycznego portalu Azure](http://manage.windowsazure.com) lub [portalu Azure](https://portal.azure.com/).
2. W portalu: **kliknij nowy**, następnie kliknij przycisk **usług danych** lub **analizy danych** w zależności od portalu, a następnie kliknij przycisk **Azure Stream Analytics**lub **strumienia Analytics** , a następnie **szybkie tworzenie**.
   
   ![Kreator zadania przetwarzania analizy danych](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  
   
   ![Utwórz zadanie przetwarzania analizy danych](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  
3. Określ odpowiednią konfigurację zadania usługi analiza strumienia.
   
   * W **Nazwa zadania** wprowadź nazwę identyfikującą zadania usługi analiza strumienia. Gdy **Nazwa zadania** jest weryfikowane, w polu Nazwa zadania zostanie wyświetlony zielony znacznik wyboru. **Nazwa zadania** może zawierać tylko znaki alfanumeryczne oraz '-' znaków i musi zawierać od 3 do 63 znaków.
   * Użyj **Region** w portalu Azure lub **lokalizacji** w portalu Azure, aby określić lokalizację geograficzną, w którym chcesz uruchomić zadanie.
   * Jeśli używasz portalu Azure, wybierz lub Utwórz konto magazynu do użycia jako **konto magazynu monitorowania regionalnego**. To konto magazynu jest używany do przechowywania danych monitorowania dla wszystkich zadań usługi Stream Analytics uruchomione w tym regionie.
   * Jeśli przy użyciu portalu Azure, określić nową lub istniejącą **grupy zasobów** do przechowywania powiązane zasoby aplikacji.
4. Po skonfigurowaniu opcji zadania Stream Analytics kliknij **utworzyć zadania usługi analiza strumienia**. Może upłynąć kilka minut, aż zadanie usługi analiza strumienia, który ma zostać utworzony. Aby sprawdzić stan, można monitorować postęp w Centrum powiadomień.
   
   ![Centrum powiadomień zadania przetwarzania analizy danych](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  
   
   ![Azure w portalu analityka danych przetwarzania zadania tworzenia zadania](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  
5. Nowe zadanie będzie wyświetlany stan **utworzony**. Zwróć uwagę, że **Start** przycisk jest niedostępny. Przed rozpoczęciem zadania, należy skonfigurować dane wejściowe zadania, zapytań i danych wyjściowych.
   
   ![Zadanie przetwarzania analizy danych stanu](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  
   
   ![Azure w portalu analityka danych przetwarzania stanu zadania](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## <a name="next-steps"></a>Następne kroki
* [Wprowadzenie do usługi Azure Stream Analytics](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-real-time-fraud-detection.md)
* [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
* [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)

