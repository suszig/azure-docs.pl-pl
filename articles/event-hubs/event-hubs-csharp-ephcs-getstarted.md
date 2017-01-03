---
title: "Rozpoczynanie pracy z usługą Event Hubs w języku C# | Microsoft Docs"
description: "Postępuj zgodnie z tym samouczkiem, aby rozpocząć korzystanie z usługi Azure Event Hubs w języku C# z użyciem hosta procesora zdarzeń."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: 2ec2378a-34f7-44c3-b976-cc444c98c338
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/07/2016
ms.author: jotaub;sethm
translationtype: Human Translation
ms.sourcegitcommit: 015dbab81f807cd05d1fedb97f5fc62a012f2084
ms.openlocfilehash: 54aaf68864fc83501b860ecc3daef1c0216c0140


---
# <a name="get-started-with-event-hubs"></a>Rozpoczynanie pracy z usługą Event Hubs
[!INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## <a name="introduction"></a>Wprowadzenie
Event Hubs to usługa, która przetwarza duże ilości danych zdarzeń (danych telemetrycznych) z podłączonych urządzeń i aplikacji. Po zebraniu danych w usłudze Event Hubs można przechowywać dane przy użyciu klastra magazynu lub przekształcać je za pomocą dostawcy analiz w czasie rzeczywistym. Ta możliwość zbierania i przetwarzania zdarzeń na wielką skalę jest kluczowym składnikiem architektur nowoczesnych aplikacji, w tym Internetu rzeczy (IoT).

W tym samouczku pokazano, jak utworzyć centrum zdarzeń za pomocą witryny Azure Portal. Pokazano także, jak zbierać komunikaty w centrum zdarzeń za pomocą aplikacji konsolowej napisanej w języku C# oraz jak pobierać je równolegle przy użyciu biblioteki [hosta procesora zdarzeń][Event Processor Host] języka C#.

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

* [Program Microsoft Visual Studio](http://visualstudio.com)
* Aktywne konto platformy Azure. Jeśli go nie masz, możesz utworzyć bezpłatne konto w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/free/).

[!INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[!INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[!INCLUDE [service-bus-event-hubs-get-started-receive-ephcs](../../includes/service-bus-event-hubs-get-started-receive-ephcs.md)]

## <a name="run-the-applications"></a>Uruchamianie aplikacji
Teraz wszystko jest gotowe do uruchomienia aplikacji.

1. Z poziomu programu Visual Studio otwórz projekt **Receiver** utworzony wcześniej.
2. Kliknij prawym przyciskiem myszy rozwiązanie **Receiver**, kliknij polecenie **Dodaj**, a następnie kliknij polecenie **Istniejący projekt**.
3. Znajdź istniejący plik Sender.csproj, a następnie kliknij go dwukrotnie, aby dodać go do rozwiązania.
4. Ponownie kliknij prawym przyciskiem myszy rozwiązanie **Receiver**, a następnie kliknij pozycję **Właściwości**. Zostanie wyświetlona strona właściwości rozwiązania **Receiver**.
5. Kliknij pozycję **Projekt startowy**, a następnie kliknij przycisk **Wiele projektów startowych**. Ustaw pole **Akcja** dla projektów **Receiver** i **Sender** na **Uruchom**.
   
    ![][19]
6. Kliknij pozycję **Zależności projektu**. W polu **Projekty** kliknij pozycję **Sender**. W polu **Zależny od** zaznacz pole wyboru projektu **Receiver**.
   
    ![][20]
7. Kliknij przycisk **OK**, aby odrzucić okno dialogowe **Właściwości**.
8. Naciśnij klawisz F5, aby uruchomić projekt **Receiver** z poziomu programu Visual Studio, a następnie zaczekaj na uruchomienie odbiorników dla wszystkich partycji.
   
   ![][21]
9. Projekt **Sender** zostanie uruchomiony automatycznie. Naciśnij klawisz **Enter** w oknie konsoli i zobacz zdarzenia wyświetlane w oknie odbiornika.
   
   ![][22]

Naciśnij klawisze **Ctrl + C** w oknie **Sender**, aby zakończyć aplikację Sender, a następnie naciśnij klawisz **Enter** w oknie Receiver, aby zamknąć tę aplikację.

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz utworzoną działającą aplikację, która tworzy centrum zdarzeń oraz wysyła i odbiera dane, możesz przejść do następujących scenariuszy:

* Kompletna [przykładowa aplikacja korzystająca z usługi Event Hubs][sample application that uses Event Hubs].
* [hosta procesora zdarzeń](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* Przykład [skalowania w poziomie dotyczącego przetwarzania zdarzeń za pomocą usługi Event Hubs][Scale out Event Processing with Event Hubs].
* [Przegląd usługi Event Hubs][Event Hubs overview]

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[queued messaging solution]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md




<!--HONumber=Dec16_HO5-->


