<properties
    pageTitle="Rozpoczynanie pracy z usługą Event Hubs w języku C# | Microsoft Azure"
    description="Postępuj zgodnie z tym samouczkiem, aby rozpocząć korzystanie z usługi Azure Event Hubs, przesyłanie zdarzeń w języku C# i odbieranie ich w języku Java z użyciem klasy EventProcessorHost."
    services="event-hubs"
    documentationCenter=""
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="event-hubs"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/27/2016"
    ms.author="jotaub;sethm"/>


# Rozpoczynanie pracy z usługą Event Hubs

[AZURE.INCLUDE [service-bus-selector-get-started](../../includes/service-bus-selector-get-started.md)]

## Wprowadzenie

Event Hubs to usługa, która przetwarza duże ilości danych zdarzeń (danych telemetrycznych) z podłączonych urządzeń i aplikacji. Po zebraniu danych w usłudze Event Hubs można przechowywać dane przy użyciu klastra magazynu lub przekształcać je za pomocą dostawcy analiz w czasie rzeczywistym. Ta możliwość zbierania i przetwarzania zdarzeń na wielką skalę jest kluczowym składnikiem architektur nowoczesnych aplikacji, w tym Internetu rzeczy (IoT).

W tym samouczku pokazano, jak utworzyć centrum zdarzeń za pomocą klasycznego portalu Azure. Pokazano także, jak zbierać komunikaty w centrum zdarzeń za pomocą aplikacji konsolowej napisanej w języku C# oraz jak pobierać je równolegle przy użyciu biblioteki hosta procesora zdarzeń języka Java.

Do wykonania kroków tego samouczka niezbędne są następujące elementy:

+ [Program Microsoft Visual Studio](http://visualstudio.com)

+ Aktywne konto platformy Azure. <br/>Jeśli go nie masz, możesz utworzyć bezpłatne konto w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F target="_blank").

[AZURE.INCLUDE [event-hubs-create-event-hub](../../includes/event-hubs-create-event-hub.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-send-csharp](../../includes/service-bus-event-hubs-get-started-send-csharp.md)]

[AZURE.INCLUDE [service-bus-event-hubs-get-started-receive-ephjava](../../includes/service-bus-event-hubs-get-started-receive-ephjava.md)]

## Uruchamianie aplikacji

Teraz wszystko jest gotowe do uruchomienia aplikacji.

1.  Uruchom projekt **Receiver**.

    ![][21]

2.  Uruchom projekt **Sender**.

    ![][22]

## Następne kroki

Teraz, gdy masz utworzoną działającą aplikację, która tworzy centrum zdarzeń oraz wysyła i odbiera dane, możesz przejść do następujących scenariuszy:

- Kompletna [przykładowa aplikacja korzystająca z usługi Event Hubs][].
- Przykład [skalowania przetwarzania zdarzeń za pomocą usługi Event Hubs][].
- [Rozwiązanie do obsługi komunikatów kolejek][] korzystające z funkcji kolejek usługi Service Bus.
- [Przegląd usługi Event Hubs][]

<!-- Images. -->
[21]: ./media/event-hubs-csharp-ephjava-getstarted/ephjava.png
[22]: ./media/event-hubs-csharp-ephjava-getstarted/cs-send.png

<!-- Links -->
[klasycznego portalu Azure]: https://manage.windowsazure.com/
[Przegląd usługi Event Hubs]: event-hubs-overview.md
[przykładowa aplikacja korzystająca z usługi Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[skalowania przetwarzania zdarzeń za pomocą usługi Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Rozwiązanie do obsługi komunikatów kolejek]: ../service-bus/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md
 



<!--HONumber=Sep16_HO3-->


