## Odbieranie komunikatów za pomocą klasy EventProcessorHost

[EventProcessorHost][] jest klasą .NET, która upraszcza odbieranie zdarzeń z usługi Event Hubs przez zarządzanie trwałymi punktami kontrolnymi i równoległymi odbiorami z tych usług. Za pomocą klasy [EventProcessorHost][] można podzielić zdarzenia między wieloma odbiornikami, nawet w przypadku hostowania w różnych węzłach. W tym przykładzie przedstawiono instrukcje użycia klasy [EventProcessorHost][] dla jednego odbiornika. W przykładzie [Skalowanie przetwarzania zdarzeń][] przedstawiono instrukcje korzystania z klasy [EventProcessorHost][] z wieloma odbiornikami.

Aby móc korzystać z klasy [EventProcessorHost][], trzeba mieć [Konto usługi Azure Storage][]:

1. Zaloguj się do witryny [Azure Portal][], a następnie kliknij pozycję **Nowe** w lewym górnym rogu ekranu.

2. Kliknij pozycję **Dane i magazyn**, a następnie pozycję **Konto magazynu**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage1.png)

3. W bloku **Utwórz konto magazynu** wpisz nazwę konta magazynu. Wybierz subskrypcję platformy Azure, grupę zasobów i lokalizację, w której chcesz utworzyć zasób. Następnie kliknij pozycję **Utwórz**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage2.png)

4. Na liście kont magazynu kliknij nowo utworzone konto magazynu.

5. W bloku kont magazynu kliknij pozycję **Klucze dostępu**. Skopiuj wartość **klucz1**, aby użyć jej w dalszej części tego samouczka.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-storage3.png)

4. W programie Visual Studio utwórz nowy projekt aplikacji klasycznej Visual C# za pomocą szablonu projektu **Aplikacja konsoli**. Nazwij projekt **Odbiornik**.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp1.png)

5. W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakietami NuGet dla rozwiązania**.

6. Kliknij kartę **Przeglądanie**, a następnie wyszukaj ciąg `Microsoft Azure Service Bus Event Hub - EventProcessorHost`. Pamiętaj, aby określić nazwę projektu (**Odbiornik**) w polu **Wersje**. Kliknij pozycję **Zainstaluj** i zaakceptuj warunki użytkowania.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-eph-csharp1.png)

    Program Visual Studio pobierze, zainstaluje i doda odniesienie do [centrum zdarzeń usługi Azure Service Bus — pakiet NuGet EventProcessorHost](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost) wraz ze wszystkimi jego zależnościami.

7. Kliknij prawym przyciskiem myszy projekt **Odbiornik**, kliknij przycisk **Dodaj**, a następnie kliknij opcję **Klasa**. Nadaj nowej klasie nazwę **SimpleEventProcessor**, a następnie kliknij przycisk **Dodaj**, aby utworzyć klasę.

    ![](./media/service-bus-event-hubs-getstarted-receive-ephcs/create-receiver-csharp2.png)

8. W górnej części pliku SimpleEventProcessor.cs dodaj następujące instrukcje:

    ```
    using Microsoft.ServiceBus.Messaging;
    using System.Diagnostics;
    ```

    Następnie zastąp następujący kod treścią klasy:

    ```
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

9. W klasie **Program** dodaj następującą instrukcję `using` w górnej części pliku:

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

    Następnie zastąp metodę `Main` w klasie `Program` następującym kodem, zastępując nazwę Centrum zdarzeń i parametry połączenia na poziomie przestrzeni nazw, które zostały zapisane wcześniej, oraz konto magazynu i klucz skopiowane we wcześniejszych sekcjach. 

    ```
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

> [AZURE.NOTE] Instrukcje w tym samouczku obejmują użycie pojedynczego wystąpienia klasy [EventProcessorHost][]. W celu zwiększenia przepływności zaleca się uruchomienie wielu wystąpień klasy [EventProcessorHost][], jak przedstawiono w przykładzie [Skalowanie przetwarzania zdarzeń][]. W tych przypadkach różne wystąpienia automatycznie koordynują się ze sobą w celu równoważenia obciążenia odebranych zdarzeń. Jeśli chcesz, aby wiele odbiorników przetwarzało *wszystkie* zdarzenia, musisz użyć koncepcji **ConsumerGroup**. W przypadku odbierania zdarzeń z różnych komputerów dobrym rozwiązaniem może być określenie nazw wystąpień klasy [EventProcessorHost][] w oparciu o komputery (lub role), w których są one wdrażane. Więcej informacji można znaleźć w tematach [Omówienie usługi Event Hubs][] i [Przewodnik programowania w usłudze Event Hubs][].

<!-- Links -->
[Omówienie usługi Event Hubs]: event-hubs-overview.md
[Przewodnik programowania w usłudze Event Hubs]: event-hubs-programming-guide.md
[Skalowanie przetwarzania zdarzeń]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
[Konto usługi Azure Storage]: ../storage/storage-create-storage-account.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[Azure Portal]: https://portal.azure.com

<!--HONumber=Sep16_HO3-->


