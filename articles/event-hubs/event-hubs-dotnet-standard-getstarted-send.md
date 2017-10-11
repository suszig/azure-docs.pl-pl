---
title: "Wysyłanie zdarzeń do usługi Azure Event Hubs przy użyciu platformy .NET Standard | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do wysyłania zdarzeń do usługi Event Hubs w .NET Standard"
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2017
ms.author: sethm
ms.openlocfilehash: 8af9d70965c1c9ad8c49b7d2bb04244fc207058d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-sending-messages-to-azure-event-hubs-in-net-standard"></a>Rozpoczynanie pracy wysyłanie komunikatów do usługi Azure Event Hubs w .NET Standard

> [!NOTE]
> Ten przykład jest dostępny na [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender).

W tym samouczku przedstawiono sposób tworzenia aplikacji konsoli .NET Core wysyłanej zestaw komunikatów do Centrum zdarzeń. Można uruchomić [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleSender) rozwiązania jako-zastępuje `EhConnectionString` i `EhEntityPath` ciągów, wartościami Centrum zdarzeń. Lub może wykonaj kroki opisane w tym samouczku, aby utworzyć własny.

## <a name="prerequisites"></a>Wymagania wstępne

* [Microsoft Visual Studio 2015 lub 2017](http://www.visualstudio.com). Obsługiwane jest również przykłady tego samouczka użyj Visual Studio 2017, ale programu Visual Studio 2015.
* [.NET core Visual Studio 2015 lub narzędzia 2017](http://www.microsoft.com/net/core).
* Subskrypcja platformy Azure.
* Koncentrator przestrzeni nazw zdarzenia.

Do wysyłania komunikatów do Centrum zdarzeń, użyjemy programu Visual Studio do pisania aplikacji konsolowej C#.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Tworzenie przestrzeni nazw usługi Event Hubs i centrum zdarzeń

Pierwszym krokiem jest użycie [portalu Azure](https://portal.azure.com) tworzenie przestrzeni nazw dla typu Centrum zdarzeń i uzyskać poświadczenia zarządzania, które aplikacja musi łączyć się z Centrum zdarzeń. Aby utworzyć przestrzeń nazw i Centrum zdarzeń, wykonaj procedurę opisaną w [w tym artykule](event-hubs-create.md), a następnie kontynuować następujące kroki.

## <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej

Uruchom program Visual Studio. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. Tworzenie aplikacji konsoli .NET Core.

![Nowy projekt][1]

## <a name="add-the-event-hubs-nuget-package"></a>Dodaj pakiet NuGet centra zdarzeń

Dodaj [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) .NET Standard biblioteki pakiet NuGet do projektu, wykonując następujące czynności: 

1. Kliknij prawym przyciskiem myszy nowo utworzony projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
2. Kliknij przycisk **Przeglądaj** kartę, a następnie wyszukaj "Microsoft.Azure.EventHubs" i wybierz **Microsoft.Azure.EventHubs** pakietu. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij to okno dialogowe.

## <a name="write-some-code-to-send-messages-to-the-event-hub"></a>Napisanie kodu do wysyłania komunikatów do Centrum zdarzeń

1. Dodaj następujące instrukcje `using` w górnej części pliku Program.cs.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Dodaj, aby stałe `Program` klasy dla usługi Event Hubs połączenia ciągu i jednostek ścieżki (nazwa Centrum zdarzeń poszczególnych). Zastąp symbole zastępcze w nawiasach odpowiednie wartości, które zostały uzyskane podczas tworzenia Centrum zdarzeń.

    ```csharp
    private static EventHubClient eventHubClient;
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    ```

3. Dodaj nową metodę o nazwie `MainAsync` do `Program` klasy, w następujący sposób:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
        // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
        // we are using the connection string from the namespace.
        var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
        {
            EntityPath = EhEntityPath
        };

        eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

        await SendMessagesToEventHub(100);

        await eventHubClient.CloseAsync();

        Console.WriteLine("Press ENTER to exit.");
        Console.ReadLine();
    }
    ```

4. Dodaj nową metodę o nazwie `SendMessagesToEventHub` do `Program` klasy, w następujący sposób:

    ```csharp
    // Creates an event hub client and sends 100 messages to the event hub.
    private static async Task SendMessagesToEventHub(int numMessagesToSend)
    {
        for (var i = 0; i < numMessagesToSend; i++)
        {
            try
            {
                var message = $"Message {i}";
                Console.WriteLine($"Sending message: {message}");
                await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
            }

            await Task.Delay(10);
        }

        Console.WriteLine($"{numMessagesToSend} messages sent.");
    }
    ```

5. Dodaj następujący kod do `Main` metoda `Program` klasy.

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

   Oto jak powinien wyglądać plik Program.cs.

    ```csharp
    namespace SampleSender
    {
        using System;
        using System.Text;
        using System.Threading.Tasks;
        using Microsoft.Azure.EventHubs;

        public class Program
        {
            private static EventHubClient eventHubClient;
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                // Creates an EventHubsConnectionStringBuilder object from the connection string, and sets the EntityPath.
                // Typically, the connection string should have the entity path in it, but for the sake of this simple scenario
                // we are using the connection string from the namespace.
                var connectionStringBuilder = new EventHubsConnectionStringBuilder(EhConnectionString)
                {
                    EntityPath = EhEntityPath
                };

                eventHubClient = EventHubClient.CreateFromConnectionString(connectionStringBuilder.ToString());

                await SendMessagesToEventHub(100);

                await eventHubClient.CloseAsync();

                Console.WriteLine("Press ENTER to exit.");
                Console.ReadLine();
            }

            // Creates an event hub client and sends 100 messages to the event hub.
            private static async Task SendMessagesToEventHub(int numMessagesToSend)
            {
                for (var i = 0; i < numMessagesToSend; i++)
                {
                    try
                    {
                        var message = $"Message {i}";
                        Console.WriteLine($"Sending message: {message}");
                        await eventHubClient.SendAsync(new EventData(Encoding.UTF8.GetBytes(message)));
                    }
                    catch (Exception exception)
                    {
                        Console.WriteLine($"{DateTime.Now} > Exception: {exception.Message}");
                    }

                    await Task.Delay(10);
                }

                Console.WriteLine($"{numMessagesToSend} messages sent.");
            }
        }
    }
    ```

6. Uruchom program i upewnij się, że nie ma w nim żadnych błędów.

Gratulacje! Wysłano komunikaty do centrum zdarzeń.

## <a name="next-steps"></a>Następne kroki
Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Odbieranie zdarzeń z usługi Event Hubs](event-hubs-dotnet-standard-getstarted-receive-eph.md)
* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md)
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-send/netcore.png
