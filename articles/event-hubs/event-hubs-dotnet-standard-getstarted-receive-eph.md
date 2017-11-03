---
title: "Odbieranie zdarzeń z usługi Azure Event Hubs przy użyciu platformy .NET Standard | Dokumentacja firmy Microsoft"
description: "Rozpocząć odbieranie komunikatów z klasy EventProcessorHost w .NET Standard"
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
ms.openlocfilehash: cc62792dad0284f9514664795fdfb32e94a85943
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-receiving-messages-with-the-event-processor-host-in-net-standard"></a>Rozpocząć odbieranie komunikatów z hosta procesora zdarzeń w .NET Standard

> [!NOTE]
> Ten przykład jest dostępny na [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver).

Ten samouczek przedstawia sposób zapisania odbierająca komunikaty od Centrum zdarzeń za pomocą aplikacji konsoli .NET Core **EventProcessorHost**. Można uruchomić [GitHub](https://github.com/Azure/azure-event-hubs/tree/master/samples/DotNet/Microsoft.Azure.EventHubs/SampleEphReceiver) rozwiązania jako — jest zastępowany ciągi wartości konta koncentratora i przechowywania zdarzeń. Lub może wykonaj kroki opisane w tym samouczku, aby utworzyć własny.

## <a name="prerequisites"></a>Wymagania wstępne

* [Microsoft Visual Studio 2015 lub 2017](http://www.visualstudio.com). Obsługiwane jest również przykłady tego samouczka użyj Visual Studio 2017, ale programu Visual Studio 2015.
* [.NET core Visual Studio 2015 lub narzędzia 2017](http://www.microsoft.com/net/core).
* Subskrypcja platformy Azure.
* Przestrzeń nazw usługi Azure Event Hubs.
* Konto usługi Azure Storage.

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Tworzenie przestrzeni nazw usługi Event Hubs i centrum zdarzeń  

Pierwszym krokiem jest użycie [portalu Azure](https://portal.azure.com) tworzenie przestrzeni nazw dla typu usługi Event Hubs i uzyskać poświadczenia zarządzania, które aplikacja musi łączyć się z Centrum zdarzeń. Aby utworzyć przestrzeń nazw i zdarzenia koncentratora, postępuj zgodnie z procedurą w [w tym artykule](event-hubs-create.md), a następnie kontynuować następujące kroki.  

## <a name="create-an-azure-storage-account"></a>Tworzenie konta usługi Azure Storage  

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).  
2. W okienku nawigacji po lewej stronie portalu kliknij **nowy**, kliknij przycisk **magazynu**, a następnie kliknij przycisk **konta magazynu**.  
3. Wypełnij pola w bloku konto magazynu, a następnie kliknij przycisk **Utwórz**.

    ![Tworzenie konta magazynu][1]

4. Po **wdrożeń zakończyło się pomyślnie** komunikatów, kliknij nazwę nowego konta magazynu. W **Essentials** bloku, kliknij przycisk **obiekty BLOB**. Gdy **usługa Blob** zostanie otwarty blok, kliknij przycisk **+ kontener** u góry. Nadaj nazwę kontenera, a następnie Zamknij **usługa Blob** bloku.  
5. Kliknij przycisk **klucze dostępu** w lewym bloku i skopiuj nazwę kontenera magazynu, konta magazynu i wartości **klucz1**. Zapisz te wartości w Notatniku lub tymczasowej lokalizacji.  

## <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej

Uruchom program Visual Studio. W menu **Plik** kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. Tworzenie aplikacji konsoli .NET Core.

![Nowy projekt][2]

## <a name="add-the-event-hubs-nuget-package"></a>Dodaj pakiet NuGet centra zdarzeń

Dodaj [ `Microsoft.Azure.EventHubs` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs/) i [ `Microsoft.Azure.EventHubs.Processor` ](https://www.nuget.org/packages/Microsoft.Azure.EventHubs.Processor/) biblioteki .NET Standard NuGet pakietów do projektu, wykonaj następujące czynności: 

1. Kliknij prawym przyciskiem myszy nowo utworzony projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
2. Kliknij przycisk **Przeglądaj** kartę, a następnie wyszukaj "Microsoft.Azure.EventHubs" i wybierz **Microsoft.Azure.EventHubs** pakietu. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij to okno dialogowe.
3. Powtórz kroki 1 i 2, a następnie zainstaluj **Microsoft.Azure.EventHubs.Processor** pakietu.

## <a name="implement-the-ieventprocessor-interface"></a>Zaimplementuj interfejs IEventProcessor

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt, kliknij przycisk **Dodaj**, a następnie kliknij przycisk **klasy**. Nazwa nowej klasy **SimpleEventProcessor**.

2. Otwórz plik SimpleEventProcessor.cs i dodaj następujące `using` instrukcje na początku pliku.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

3. Implementowanie `IEventProcessor` interfejsu. Zastąp całą zawartość `SimpleEventProcessor` klasy następującym kodem:

    ```csharp
    public class SimpleEventProcessor : IEventProcessor
    {
        public Task CloseAsync(PartitionContext context, CloseReason reason)
        {
            Console.WriteLine($"Processor Shutting Down. Partition '{context.PartitionId}', Reason: '{reason}'.");
            return Task.CompletedTask;
        }

        public Task OpenAsync(PartitionContext context)
        {
            Console.WriteLine($"SimpleEventProcessor initialized. Partition: '{context.PartitionId}'");
            return Task.CompletedTask;
        }

        public Task ProcessErrorAsync(PartitionContext context, Exception error)
        {
            Console.WriteLine($"Error on Partition: {context.PartitionId}, Error: {error.Message}");
            return Task.CompletedTask;
        }

        public Task ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
        {
            foreach (var eventData in messages)
            {
                var data = Encoding.UTF8.GetString(eventData.Body.Array, eventData.Body.Offset, eventData.Body.Count);
                Console.WriteLine($"Message received. Partition: '{context.PartitionId}', Data: '{data}'");
            }

            return context.CheckpointAsync();
        }
    }
    ```

## <a name="write-a-main-console-method-that-uses-the-simpleeventprocessor-class-to-receive-messages"></a>Napisanie metody konsoli głównej, która używa klasy SimpleEventProcessor do odbierania wiadomości

1. Dodaj następujące instrukcje `using` w górnej części pliku Program.cs.

    ```csharp
    using Microsoft.Azure.EventHubs;
    using Microsoft.Azure.EventHubs.Processor;
    using System.Threading.Tasks;
    ```

2. Dodaj, aby stałe `Program` klasy dla parametrów połączenia Centrum zdarzeń, nazwy Centrum zdarzeń, nazwa kontenera konta magazynu, nazwa konta magazynu i klucz konta magazynu. Dodaj następujący kod, zastępując symbole zastępcze z odpowiednimi wartościami.

    ```csharp
    private const string EhConnectionString = "{Event Hubs connection string}";
    private const string EhEntityPath = "{Event Hub path/name}";
    private const string StorageContainerName = "{Storage account container name}";
    private const string StorageAccountName = "{Storage account name}";
    private const string StorageAccountKey = "{Storage account key}";

    private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);
    ```   

3. Dodaj nową metodę o nazwie `MainAsync` do `Program` klasy, w następujący sposób:

    ```csharp
    private static async Task MainAsync(string[] args)
    {
        Console.WriteLine("Registering EventProcessor...");

        var eventProcessorHost = new EventProcessorHost(
            EhEntityPath,
            PartitionReceiver.DefaultConsumerGroupName,
            EhConnectionString,
            StorageConnectionString,
            StorageContainerName);

        // Registers the Event Processor Host and starts receiving messages
        await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

        Console.WriteLine("Receiving. Press ENTER to stop worker.");
        Console.ReadLine();

        // Disposes of the Event Processor Host
        await eventProcessorHost.UnregisterEventProcessorAsync();
    }
    ```

3. Dodaj następujący wiersz kodu w celu `Main` metody:

    ```csharp
    MainAsync(args).GetAwaiter().GetResult();
    ```

    Oto jak powinien wyglądać plik Program.cs:

    ```csharp
    namespace SampleEphReceiver
    {

        public class Program
        {
            private const string EhConnectionString = "{Event Hubs connection string}";
            private const string EhEntityPath = "{Event Hub path/name}";
            private const string StorageContainerName = "{Storage account container name}";
            private const string StorageAccountName = "{Storage account name}";
            private const string StorageAccountKey = "{Storage account key}";

            private static readonly string StorageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", StorageAccountName, StorageAccountKey);

            public static void Main(string[] args)
            {
                MainAsync(args).GetAwaiter().GetResult();
            }

            private static async Task MainAsync(string[] args)
            {
                Console.WriteLine("Registering EventProcessor...");

                var eventProcessorHost = new EventProcessorHost(
                    EhEntityPath,
                    PartitionReceiver.DefaultConsumerGroupName,
                    EhConnectionString,
                    StorageConnectionString,
                    StorageContainerName);

                // Registers the Event Processor Host and starts receiving messages
                await eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>();

                Console.WriteLine("Receiving. Press ENTER to stop worker.");
                Console.ReadLine();

                // Disposes of the Event Processor Host
                await eventProcessorHost.UnregisterEventProcessorAsync();
            }
        }
    }
    ```

4. Uruchom program i upewnij się, że nie ma w nim żadnych błędów.

Gratulacje! Możesz teraz otrzymali wiadomości z Centrum zdarzeń przy użyciu hosta procesora zdarzeń.

## <a name="next-steps"></a>Następne kroki
Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md)
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

[1]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/event-hubs-python1.png
[2]: ./media/event-hubs-dotnet-standard-getstarted-receive-eph/netcore.png
