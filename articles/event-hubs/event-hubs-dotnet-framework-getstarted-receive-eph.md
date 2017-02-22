---
title: "Odbieranie zdarzeń z usługi Azure Event Hubs za pomocą programu .NET Framework | Microsoft Docs"
description: "Postępuj zgodnie z tym samouczkiem, aby odbierać zdarzenia z usługi Azure Event Hubs za pomocą programu .NET Framework."
services: event-hubs
documentationcenter: 
author: jtaubensee
manager: timlt
editor: 
ms.assetid: c4974bd3-2a79-48a1-aa3b-8ee2d6655b28
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/30/2017
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: fe331199333d492dbc42c9125c9da96a44066ee1
ms.openlocfilehash: 4a3656d09f216e5c895bac5d7d680b44d76f4df8


---
# <a name="get-started-with-event-hubs-using-the-net-framework"></a>Rozpoczynanie pracy z usługą Event Hubs przy użyciu programu .NET Framework

## <a name="receive-messages-with-eventprocessorhost"></a>Odbieranie komunikatów za pomocą klasy EventProcessorHost
[EventProcessorHost][EventProcessorHost] jest klasą .NET, która upraszcza odbieranie zdarzeń z usługi Event Hubs przez zarządzanie trwałymi punktami kontrolnymi i równoległymi odbiorami z tej usługi. Za pomocą klasy [EventProcessorHost][EventProcessorHost] można podzielić zdarzenia między wieloma odbiornikami, nawet w przypadku hostowania w różnych węzłach. W tym przykładzie przedstawiono, jak używać klasy [EventProcessorHost][EventProcessorHost] dla jednego odbiornika. W przykładzie [Skalowanie przetwarzania zdarzeń][Scale out Event Processing with Event Hubs] przedstawiono instrukcje korzystania z klasy [EventProcessorHost][EventProcessorHost] z wieloma odbiornikami.

Aby móc korzystać z klasy [EventProcessorHost][EventProcessorHost], trzeba mieć [konto usługi Azure Storage][Azure Storage account]:

1. Zaloguj się do witryny [Azure Portal][Azure portal], a następnie kliknij pozycję **Nowy** w lewym górnym rogu ekranu.
2. Kliknij pozycję **Magazyn**, a następnie pozycję **Konto magazynu**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage1.png)
3. W bloku **Utwórz konto magazynu** wpisz nazwę konta magazynu. Wybierz subskrypcję platformy Azure, grupę zasobów i lokalizację, w której chcesz utworzyć zasób. Następnie kliknij pozycję **Utwórz**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)
4. Na liście kont magazynu kliknij nowo utworzone konto magazynu.
5. W bloku kont magazynu kliknij pozycję **Klucze dostępu**. Skopiuj wartość **klucz1**, aby użyć jej w dalszej części tego samouczka.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)
6. W programie Visual Studio utwórz nowy projekt aplikacji klasycznej Visual C# za pomocą szablonu projektu **Aplikacja konsoli**. Nazwij projekt **Odbiornik**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp1.png)
7. W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakietami NuGet dla rozwiązania**.
8. Kliknij kartę **Przeglądanie**, a następnie wyszukaj ciąg `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Pamiętaj, aby określić nazwę projektu (**Odbiornik**) w polu **Wersje**. Kliknij pozycję **Zainstaluj** i zaakceptuj warunki użytkowania.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-eph-csharp1.png)
   
    Program Visual Studio pobierze, zainstaluje i doda odniesienie do [centrum zdarzeń usługi Azure Service Bus — pakiet NuGet EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) wraz ze wszystkimi jego zależnościami.
9. Kliknij prawym przyciskiem myszy projekt **Odbiornik**, kliknij przycisk **Dodaj**, a następnie kliknij opcję **Klasa**. Nadaj nowej klasie nazwę **SimpleEventProcessor**, a następnie kliknij przycisk **Dodaj**, aby utworzyć klasę.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-receiver-csharp2.png)
10. W górnej części pliku SimpleEventProcessor.cs dodaj następujące instrukcje:
    
     ```csharp
     using Microsoft.ServiceBus.Messaging;
     using System.Diagnostics;
     ```
    
     Następnie zastąp następujący kod treścią klasy:
    
     ```csharp
     class SimpleEventProcessor : IEventProcessor
     {
         Stopwatch checkpointStopWatch;
    
         async Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
         {
             Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
             if (reason == CloseReason.Shutdown)
             {
                 await context.CheckpointAsync();
             }
         }
    
         Task IEventProcessor.OpenAsync(PartitionContext context)
         {
             Console.WriteLine("SimpleEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);
             this.checkpointStopWatch = new Stopwatch();
             this.checkpointStopWatch.Start();
             return Task.FromResult<object>(null);
         }
    
         async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
         {
             foreach (EventData eventData in messages)
             {
                 string data = Encoding.UTF8.GetString(eventData.GetBytes());
    
                 Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
                     context.Lease.PartitionId, data));
             }
    
             //Call checkpoint every 5 minutes, so that worker can resume processing from 5 minutes back if it restarts.
             if (this.checkpointStopWatch.Elapsed > TimeSpan.FromMinutes(5))
             {
                 await context.CheckpointAsync();
                 this.checkpointStopWatch.Restart();
             }
         }
     }
     ```
    
     Klasa ta zostanie wywołana przez klasę **EventProcessorHost** do przetwarzania zdarzeń odebranych z Centrum zdarzeń. Należy pamiętać, że klasa `SimpleEventProcessor` używa stopera, aby okresowo wywoływać metodę punktu kontrolnego w kontekście klasy **EventProcessorHost**. Daje to gwarancję, że jeśli odbiornik zostanie ponownie uruchomiony, nie straci więcej niż pięć minut operacji przetwarzania.
11. W klasie **Program** dodaj następującą instrukcję `using` w górnej części pliku:
    
     ```csharp
     using Microsoft.ServiceBus.Messaging;
     ```
    
     Następnie zastąp metodę `Main` w klasie `Program` następującym kodem, zastępując nazwę Centrum zdarzeń i parametry połączenia na poziomie przestrzeni nazw, które zostały zapisane wcześniej, oraz konto magazynu i klucz skopiowane we wcześniejszych sekcjach. 
    
     ```csharp
     static void Main(string[] args)
     {
       string eventHubConnectionString = "{Event Hub connection string}";
       string eventHubName = "{Event Hub name}";
       string storageAccountName = "{storage account name}";
       string storageAccountKey = "{storage account key}";
       string storageConnectionString = string.Format("DefaultEndpointsProtocol=https;AccountName={0};AccountKey={1}", storageAccountName, storageAccountKey);
    
       string eventProcessorHostName = Guid.NewGuid().ToString();
       EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, eventHubName, EventHubConsumerGroup.DefaultGroupName, eventHubConnectionString, storageConnectionString);
       Console.WriteLine("Registering EventProcessor...");
       var options = new EventProcessorOptions();
       options.ExceptionReceived += (sender, e) => { Console.WriteLine(e.Exception); };
       eventProcessorHost.RegisterEventProcessorAsync<SimpleEventProcessor>(options).Wait();
    
       Console.WriteLine("Receiving. Press enter key to stop worker.");
       Console.ReadLine();
       eventProcessorHost.UnregisterEventProcessorAsync().Wait();
     }
     ```

> [!NOTE]
> Instrukcje w tym samouczku obejmują użycie pojedynczego wystąpienia klasy [EventProcessorHost][EventProcessorHost]. W celu zwiększenia przepływności zaleca się uruchomienie wielu wystąpień klasy [EventProcessorHost][EventProcessorHost], jak przedstawiono w przykładzie [Skalowanie przetwarzania zdarzeń][Skalowanie przetwarzania zdarzeń]. W tych przypadkach różne wystąpienia automatycznie koordynują się ze sobą w celu równoważenia obciążenia odebranych zdarzeń. Jeśli chcesz, aby wiele odbiorników przetwarzało *wszystkie* zdarzenia, musisz użyć koncepcji **ConsumerGroup**. W przypadku odbierania zdarzeń z różnych maszyn dobrym rozwiązaniem może być określenie nazw wystąpień klasy [EventProcessorHost][EventProcessorHost] w oparciu o maszyny (lub role), w których są one wdrażane. Więcej informacji można znaleźć w tematach [Omówienie usługi Event Hubs][Event Hubs Overview] i [Przewodnik programowania w usłudze Event Hubs][Event Hubs Programming Guide].
> 
> 

<!-- Links -->
[Event Hubs Overview]: event-hubs-overview.md
[Event Hubs Programming Guide]: event-hubs-programming-guide.md
[Azure Storage account]: ../storage/storage-create-storage-account.md
[EventProcessorHost]: /dotnet/api/microsoft.servicebus.messaging.eventprocessorhost
[Azure portal]: https://portal.azure.com

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz utworzoną działającą aplikację, która tworzy centrum zdarzeń oraz wysyła i odbiera dane, możesz dowiedzieć się więcej, przechodząc pod jeden z następujących linków:

* [hosta procesora zdarzeń](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost)
* [Przegląd usługi Event Hubs][Event Hubs overview]
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

<!-- Images. -->
[19]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj1.png
[20]: ./media/event-hubs-csharp-ephcs-getstarted/create-eh-proj2.png
[21]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs1.png
[22]: ./media/event-hubs-csharp-ephcs-getstarted/run-csharp-ephcs2.png

<!-- Links -->
[Event Processor Host]: https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost
[Event Hubs overview]: event-hubs-overview.md
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3


<!--HONumber=Feb17_HO1-->


