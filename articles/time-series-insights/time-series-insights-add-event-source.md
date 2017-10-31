---
title: "Dodawanie źródła zdarzeń do środowiska usługi Azure Time Series Insights | Microsoft Docs"
description: "W tym samouczku omówiono łączenie źródła zdarzeń ze środowiskiem usługi Time Series Insights"
keywords: 
services: time-series-insights
documentationcenter: 
author: op-ravi
manager: santoshb
editor: cgronlun
ms.assetid: 
ms.service: tsi
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/21/2017
ms.author: omravi
ms.openlocfilehash: 123ecca28f0d970851487827d0d18e244ce6d98e
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="create-an-event-source-for-your-time-series-insights-environment-using-the-azure-portal"></a>Tworzenie źródła zdarzeń dla środowiska usługi Time Series Insights przy użyciu witryny Azure Portal

Źródło zdarzeń usługi Time Series Insights pochodzi od brokera zdarzeń, takiego jak usługa Azure Event Hubs. Usługa Time Series Insights łączy się bezpośrednio ze źródłami zdarzeń, umożliwiając pozyskiwanie strumienia danych bez konieczności pisania kodu przez użytkowników. Obecnie usługa Time Series Insights obsługuje centra zdarzeń Azure Event Hub i Azure IoT Hub. W przyszłości zostanie dodanych więcej źródeł zdarzeń.

## <a name="steps-to-add-an-event-source-to-your-environment"></a>Procedura dodawania źródła zdarzeń do środowiska

1.  Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2.  W menu po lewej stronie w witrynie Azure Portal kliknij pozycję „Wszystkie zasoby”.
3.  Wybierz środowisko usługi Time Series Insights.

  ![Tworzenie źródła zdarzeń usługi Time Series Insights](media/add-event-source/getstarted-create-event-source-1.png)

4.  Wybierz pozycję „Źródła zdarzeń” i kliknij przycisk „+ Dodaj”.

  ![Tworzenie źródła zdarzeń usługi Time Series Insights — szczegóły](media/add-event-source/getstarted-create-event-source-2.png)

5.  Podaj nazwę źródła zdarzeń. Nazwa ta zostanie skojarzona ze wszystkimi zdarzeniami pochodzącymi z tego źródła zdarzeń i będzie dostępna w czasie wykonywania zapytań.
6.  Wybierz centrum zdarzeń z listy zasobów Centrum zdarzeń w bieżącej subskrypcji. W przeciwnym razie wybierz opcję importowania „Podaj ręcznie ustawienia centrum zdarzeń”, aby określić centrum zdarzeń w innej subskrypcji. Zdarzenia muszą być publikowane w formacie JSON.
7.  Wybierz zasady, które mają uprawnienia do odczytu w centrum zdarzeń.
8.  Wskaż grupę odbiorców centrum zdarzeń.

  > [!IMPORTANT]
  > Sprawdź, czy ta grupa odbiorców nie jest używana przez żadną inną usługę (np. zadanie usługi Stream Analytics lub drugie środowisko usługi Time Series Insights). Używanie grupy odbiorców przez inne usługi ma negatywny wpływ na operacje odczytu w bieżącym środowisku i tych usługach. Użycie grupy odbiorców „$Default” może potencjalnie spowodować jej ponowne użycie przez inne czytniki.

9.  Kliknij przycisk „Utwórz”.

Po utworzeniu źródła zdarzeń usługa Time Series Insights automatycznie rozpocznie przesyłanie strumieni danych do środowiska.

## <a name="next-steps"></a>Następne kroki

* [Wysyłanie zdarzeń](time-series-insights-send-events.md) do źródła zdarzeń
* Wyświetlanie środowiska w [Portalu usługi Time Series Insights](https://insights.timeseries.azure.com)
