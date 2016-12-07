---
title: "Rozpoczynanie pracy z usługą Event Hubs w języku C# | Microsoft Docs"
description: "Postępuj zgodnie z tym samouczkiem, aby rozpocząć korzystanie z usługi Azure Event Hubs, przesyłanie zdarzeń w języku C# i odbieranie ich w języku Java z użyciem klasy EventProcessorHost."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 059fb733-a397-400e-8e43-0c7ea5930b8b
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/27/2016
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 51d880650ab8059f3346b5c1272c232b49be33e9


---
# <a name="get-started-with-event-hubs"></a>Rozpoczynanie pracy z usługą Event Hubs
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie
Event Hubs to usługa, która przetwarza duże ilości danych zdarzeń (danych telemetrycznych) z podłączonych urządzeń i aplikacji. Po zebraniu danych w usłudze Event Hubs można przechowywać dane przy użyciu klastra magazynu lub przekształcać je za pomocą dostawcy analiz w czasie rzeczywistym. Ta możliwość zbierania i przetwarzania zdarzeń na wielką skalę jest kluczowym składnikiem architektur nowoczesnych aplikacji, w tym Internetu rzeczy (IoT).

W tym samouczku pokazano, jak utworzyć centrum zdarzeń za pomocą klasycznej witryny Azure Portal. Pokazano także, jak zbierać komunikaty w centrum zdarzeń za pomocą aplikacji konsolowej napisanej w języku C# oraz jak pobierać je równolegle przy użyciu biblioteki hosta procesora zdarzeń języka Java.

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* [Program Microsoft Visual Studio](http://visualstudio.com)
* Aktywne konto platformy Azure. <br/>Jeśli go nie masz, możesz utworzyć bezpłatne konto w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji
Teraz wszystko jest gotowe do uruchomienia aplikacji.

1. Uruchom projekt **Receiver**.
   
   ![][21]
2. Uruchom projekt **Sender**.
   
   ![][22]

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz utworzoną działającą aplikację, która tworzy centrum zdarzeń oraz wysyła i odbiera dane, możesz przejść do następujących scenariuszy:

* Kompletna [przykładowa aplikacja korzystająca z usługi Event Hubs][przykładowa aplikacja korzystająca z usługi Event Hubs].
* Przykład [skalowania przetwarzania zdarzeń za pomocą usługi Event Hubs][Skalowanie przetwarzania zdarzeń za pomocą usługi Event Hubs].
* [Przegląd usługi Event Hubs][Przegląd usługi Event Hubs]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[Klasyczna witryna Azure Portal]: https://manage.windowsazure.com/
[Przegląd usługi Event Hubs]: event-hubs-overview.md
[przykładowa aplikacja korzystająca z usługi Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Skalowanie przetwarzania zdarzeń za pomocą usługi Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3



<!--HONumber=Nov16_HO2-->


