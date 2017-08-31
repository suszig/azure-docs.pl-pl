---
title: "Tworzenie środowiska usługi Azure Time Series Insights | Microsoft Docs"
description: "W tym samouczku dowiesz się, jak utworzyć środowisko usługi Time Series, połączyć je ze źródłem zdarzeń i przygotować się do analizowania danych zdarzeń w kilka minut."
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 1cc1ee946d8eb2214fd05701b495bbce6d471a49
ms.openlocfilehash: eb710795916a2d7beea75a6408a0982fb4dc8750
ms.contentlocale: pl-pl
ms.lasthandoff: 04/25/2017

---

# <a name="create-a-new-time-series-insights-environment-in-the-azure-portal"></a>Tworzenie nowego środowiska usługi Time Series Insights w witrynie Azure Portal

Środowisko usługi Time Series Insights jest zasobem platformy Azure, który umożliwia pozyskiwanie i magazynowanie danych. Klienci mogą aprowizować środowiska z uwzględnieniem wymaganej wydajności za pośrednictwem witryny Azure Portal.

## <a name="steps-to-create-the-environment"></a>Procedura tworzenia środowiska

Aby utworzyć środowisko, wykonaj następujące kroki:

1.  Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2.  Kliknij znak plus („+”) w lewym górnym rogu.
3.  Wyszukaj nazwę „Time Series Insights” za pomocą pola wyszukiwania.

  ![Tworzenie środowiska usługi Time Series Insights](media/get-started/getstarted-create-environment1.png)

4.  Wybierz pozycję „Time Series Insights” i kliknij przycisk „Utwórz”.

  ![Tworzenie grupy zasobów usługi Time Series Insights](media/get-started/getstarted-create-environment2.png)

5.  Podaj nazwę środowiska. Nazwa ta będzie reprezentować środowisko w [eksploratorze szeregów czasowych](https://insights.timeseries.azure.com).
6.  Wybierz subskrypcję. Musi ona zawierać źródło zdarzeń. Usługa Time Series Insights może automatycznie wykrywać zasoby usługi Azure IoT Hub i centrum zdarzeń istniejące w tej samej subskrypcji.
7.  Wybierz lub utwórz grupę zasobów. Grupa zasobów jest kolekcją używanych razem zasobów platformy Azure.
8.  Wybierz lokalizację hostowania. Aby uniknąć przenoszenia danych między centrami danych, wybierz lokalizację, która zawiera źródło zdarzeń.
9.  Wybierz warstwę cenową.
10. Wybierz wydajność. Wydajność środowiska można zmienić po jego utworzeniu.
11. Utwórz środowisko. Środowisko można również przypiąć do pulpitu nawigacyjnego, aby mieć do niego łatwy dostęp po każdym zalogowaniu.

  ![Przypinanie usługi Time Series Insights do pulpitu nawigacyjnego](media/get-started/getstarted-create-environment3.png)

## <a name="next-steps"></a>Następne kroki

* [Definiowanie zasad dostępu do danych](time-series-insights-data-access.md) umożliwiających uzyskiwanie dostępu do środowiska w [Portalu usługi Time Series Insights](https://insights.timeseries.azure.com)
* [Tworzenie źródła zdarzeń](time-series-insights-add-event-source.md)
* [Wysyłanie zdarzeń](time-series-insights-send-events.md) do źródła zdarzeń

