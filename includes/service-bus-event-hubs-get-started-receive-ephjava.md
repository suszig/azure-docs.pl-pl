## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Odbieranie komunikatów EventProcessorHost w języku Java
EventProcessorHost jest klasą języka Java, która upraszcza odbieranie zdarzeń z centrów Event Hubs przez zarządzanie trwałymi punktami kontrolnymi i równoległymi odbiorami z tych centrów. Za pomocą klasy EventProcessorHost można podzielić zdarzenia między wieloma odbiornikami, nawet w przypadku hostowania w różnych węzłach. W tym przykładzie przedstawiono, jak używać klasy EventProcessorHost dla jednego odbiornika.

### <a name="create-a-storage-account"></a>Tworzenie konta magazynu
Aby używać klasy EventProcessorHost, trzeba mieć [konto usługi Azure Storage][konto usługi Azure Storage]:

1. Zaloguj się do [klasycznej witryny Azure Portal][Klasyczna witryna Azure Portal] i kliknij pozycję **NOWE** w dolnej części ekranu.
2. Kliknij opcję **Data Services**, następnie **Storage**, **Szybkie tworzenie**, a następnie wpisz nazwę konta magazynu. Wybierz odpowiedni region, a następnie kliknij opcję **Utwórz konto magazynu**.
   
    ![][11]
3. Kliknij nowo utworzone konto magazynu, a następnie kliknij opcję **Zarządzaj kluczami dostępu**:
   
    ![][12]
   
    Skopiuj podstawowy klucz dostępu, który zostanie użyty później w tym samouczku.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Tworzenie projektu języka Java za pomocą hosta EventProcessor
Bibliotekę klienta Java dla usługi Event Hubs można używać w projektach narzędzia Maven za pomocą usługi [Maven Central Repository][Maven Package] i można się do niej odwołać przy użyciu poniższej deklaracji zależności wewnątrz pliku projektu Maven:    

```XML
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs-eph</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Dla różnych typów środowisk kompilacji można jawnie uzyskać najnowsze wersje plików JAR, korzystając z usługi [Maven Central Repository][Maven Package] lub [punktu dystrybucji wydania w witrynie GitHub](https://github.com/Azure/azure-event-hubs/releases).  

1. Na potrzeby poniższego przykładu należy w ulubionym środowisku programowania Java utworzyć nowy projekt Maven dla aplikacji konsoli lub powłoki. Klasa zostanie nazwana ```ErrorNotificationHandler```.     
   
    ```Java
    import java.util.function.Consumer;
    import com.microsoft.azure.eventprocessorhost.ExceptionReceivedEventArgs;
   
    public class ErrorNotificationHandler implements Consumer<ExceptionReceivedEventArgs>
    {
        @Override
        public void accept(ExceptionReceivedEventArgs t)
        {
            System.out.println("SAMPLE: Host " + t.getHostname() + " received general error notification during " + t.getAction() + ": " + t.getException().toString());
        }
    }
    ```
2. Użyj następującego kodu, aby utworzyć nową klasę o nazwie ```EventProcessor```.
   
    ```Java
    import com.microsoft.azure.eventhubs.EventData;
    import com.microsoft.azure.eventprocessorhost.CloseReason;
    import com.microsoft.azure.eventprocessorhost.IEventProcessor;
    import com.microsoft.azure.eventprocessorhost.PartitionContext;
   
    public class EventProcessor implements IEventProcessor
    {
        private int checkpointBatchingCount = 0;
   
        @Override
        public void onOpen(PartitionContext context) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is opening");
        }
   
        @Override
        public void onClose(PartitionContext context, CloseReason reason) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " is closing for reason " + reason.toString());
        }
   
        @Override
        public void onError(PartitionContext context, Throwable error)
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " onError: " + error.toString());
        }
   
        @Override
        public void onEvents(PartitionContext context, Iterable<EventData> messages) throws Exception
        {
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " got message batch");
            int messageCount = 0;
            for (EventData data : messages)
            {
                System.out.println("SAMPLE (" + context.getPartitionId() + "," + data.getSystemProperties().getOffset() + "," +
                        data.getSystemProperties().getSequenceNumber() + "): " + new String(data.getBody(), "UTF8"));
                messageCount++;
   
                this.checkpointBatchingCount++;
                if ((checkpointBatchingCount % 5) == 0)
                {
                    System.out.println("SAMPLE: Partition " + context.getPartitionId() + " checkpointing at " +
                        data.getSystemProperties().getOffset() + "," + data.getSystemProperties().getSequenceNumber());
                    context.checkpoint(data);
                }
            }
            System.out.println("SAMPLE: Partition " + context.getPartitionId() + " batch size was " + messageCount + " for host " + context.getOwner());
        }
    }
    ```
3. Utwórz ostatnią klasę o nazwie ```EventProcessorSample``` przy użyciu następującego kodu.
   
    ```Java
    import com.microsoft.azure.eventprocessorhost.*;
    import com.microsoft.azure.servicebus.ConnectionStringBuilder;
    import com.microsoft.azure.eventhubs.EventData;
   
    public class EventProcessorSample
    {
        public static void main(String args[])
        {
            final String consumerGroupName = "$Default";
            final String namespaceName = "----ServiceBusNamespaceName-----";
            final String eventHubName = "----EventHubName-----";
            final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
            final String sasKey = "---SharedAccessSignatureKey----";
   
            final String storageAccountName = "---StorageAccountName----";
            final String storageAccountKey = "---StorageAccountKey----";
            final String storageConnectionString = "DefaultEndpointsProtocol=https;AccountName=" + storageAccountName + ";AccountKey=" + storageAccountKey;
   
            ConnectionStringBuilder eventHubConnectionString = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
   
            EventProcessorHost host = new EventProcessorHost(eventHubName, consumerGroupName, eventHubConnectionString.toString(), storageConnectionString);
   
            System.out.println("Registering host named " + host.getHostName());
            EventProcessorOptions options = new EventProcessorOptions();
            options.setExceptionNotification(new ErrorNotificationHandler());
            try
            {
                host.registerEventProcessor(EventProcessor.class, options).get();
            }
            catch (Exception e)
            {
                System.out.print("Failure while registering: ");
                if (e instanceof ExecutionException)
                {
                    Throwable inner = e.getCause();
                    System.out.println(inner.toString());
                }
                else
                {
                    System.out.println(e.toString());
                }
            }
   
            System.out.println("Press enter to stop");
            try
            {
                System.in.read();
                host.unregisterEventProcessor();
   
                System.out.println("Calling forceExecutorShutdown");
                EventProcessorHost.forceExecutorShutdown(120);
            }
            catch(Exception e)
            {
                System.out.println(e.toString());
                e.printStackTrace();
            }
   
            System.out.println("End of sample");
        }
    }
    ```
4. Zastąp następujące pola wartościami użytymi podczas tworzenia centrum zdarzeń i konta magazynu.
   
    ```Java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> Instrukcje w tym samouczku obejmują użycie pojedynczego wystąpienia hosta EventProcessorHost. W celu zwiększenia przepływności zaleca się uruchomienie wielu wystąpień hosta EventProcessorHost. W tych przypadkach różne wystąpienia automatycznie koordynują się ze sobą w celu równoważenia obciążenia odebranych zdarzeń. Jeśli chcesz, aby wiele odbiorników przetwarzało *wszystkie* zdarzenia, musisz użyć koncepcji **ConsumerGroup**. W przypadku odbierania zdarzeń z różnych komputerów dobrym rozwiązaniem może być określenie nazw wystąpień hosta EventProcessorHost w oparciu o komputery (lub role), w których są one wdrażane.
> 
> 

<!-- Links -->
[Przegląd usługi Event Hubs]: ../articles/event-hubs/event-hubs-overview.md
[konto usługi Azure Storage]: ../articles/storage/storage-create-storage-account.md
[Klasyczna witryna Azure Portal]: http://manage.windowsazure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png



<!--HONumber=Nov16_HO4-->


