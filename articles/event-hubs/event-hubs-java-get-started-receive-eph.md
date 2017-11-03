---
title: "Odbieranie zdarzeń z usługi Azure Event Hubs przy użyciu języka Java | Dokumentacja firmy Microsoft"
description: "Rozpocząć odbieranie z usługi Event Hubs przy użyciu języka Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 38e3be53-251c-488f-a856-9a500f41b6ca
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: 3c1b455e6298367dc50f0943b58f6cf1e7f1c5fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="receive-events-from-azure-event-hubs-using-java"></a>Odbieranie zdarzeń z usługi Azure Event Hubs przy użyciu języka Java


## <a name="introduction"></a>Wprowadzenie
Event Hubs to wysoce skalowalny system przyjmowania może obsługiwać miliony zdarzeń na sekundę, włączanie aplikacji do przetwarzania i analizowanie olbrzymich ilości danych wytworzonych przez podłączone urządzenia i aplikacje. Po zebraniu danych do usługi Event Hubs można przekształcać dane za pomocą dowolnego dostawcy analiz w czasie rzeczywistym lub przechowywać je przy użyciu klastra magazynu.

Aby uzyskać więcej informacji, zobacz [Przegląd usługi Event Hubs][Event Hubs overview].

Ten samouczek pokazuje sposób odbierania zdarzeń do Centrum zdarzeń za pomocą aplikacji konsoli napisanych w języku Java.

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka niezbędne są następujące wymagania wstępne:

* Środowisko projektowe Java. W tym samouczku przyjęto założenie, [Eclipse](https://www.eclipse.org/).
* Aktywne konto platformy Azure. <br/>Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezpłatna wersja próbna platformy Azure</a>.

## <a name="receive-messages-with-eventprocessorhost-in-java"></a>Odbieranie komunikatów EventProcessorHost w języku Java

**EventProcessorHost** jest klasa Java, która upraszcza odbieranie zdarzeń z usługi Event Hubs przez zarządzanie trwałymi punktami kontrolnymi i równoległymi odbiorami z tych usług. Za pomocą klasy EventProcessorHost, można podzielić zdarzenia między wieloma odbiornikami, nawet w przypadku hostowania w różnych węzłach. W tym przykładzie przedstawiono, jak używać klasy EventProcessorHost dla jednego odbiornika.

### <a name="create-a-storage-account"></a>Tworzenie konta magazynu
Aby użyć EventProcessorHost, musi mieć [konta magazynu Azure][Azure Storage account]:

1. Zaloguj się do [portalu Azure][Azure portal]i kliknij przycisk **+ nowy** po lewej stronie ekranu.
2. Kliknij pozycję **Magazyn**, a następnie pozycję **Konto magazynu**. W bloku **Utwórz konto magazynu** wpisz nazwę konta magazynu. Wykonaj pozostałe pola, wybierz odpowiedni region, a następnie kliknij **Utwórz**.
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage2.png)

3. Kliknij nowo utworzone konto magazynu, a następnie kliknij opcję **Zarządzaj kluczami dostępu**:
   
    ![](./media/event-hubs-dotnet-framework-getstarted-receive-eph/create-storage3.png)

    Skopiuj podstawowy klucz dostępu do lokalizacji tymczasowej w dalszej części tego samouczka.

### <a name="create-a-java-project-using-the-eventprocessor-host"></a>Tworzenie projektu języka Java za pomocą hosta EventProcessor
Java biblioteki klienta usługi Event hubs jest dostępny do użytku w projekty Maven z [Maven centralnym repozytorium][Maven Package]i można odwoływać się przy użyciu następujących deklaracji zależności wewnątrz programu Maven plik projektu:    

```xml
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
<dependency>
  <groupId>com.microsoft.azure</groupId>
  <artifactId>azure-eventhubs-eph</artifactId>
  <version>0.14.0</version>
</dependency>
```

Dla różnych typów środowiska kompilacji, można jawnie uzyskać najnowsze wydanych JAR pliki z [Maven centralnym repozytorium] [ Maven Package] lub [wersji punkt dystrybucji w witrynie GitHub ](https://github.com/Azure/azure-event-hubs/releases).  

1. Na potrzeby poniższego przykładu należy w ulubionym środowisku programowania Java utworzyć nowy projekt Maven dla aplikacji konsoli lub powłoki. Klasa jest nazywany `ErrorNotificationHandler`.     
   
    ```java
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
2. Użyj następującego kodu, aby utworzyć nową klasę o nazwie `EventProcessor`.
   
    ```java
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
3. Utwórz jeden więcej klasy o nazwie `EventProcessorSample`, używając następującego kodu.
   
    ```java
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
4. Zamień pola wartości używane podczas tworzenia konta Centrum i przechowywania zdarzeń.
   
    ```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
   
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
   
    final String storageAccountName = "---StorageAccountName----"
    final String storageAccountKey = "---StorageAccountKey----";
    ```

> [!NOTE]
> Instrukcje w tym samouczku obejmują użycie pojedynczego wystąpienia hosta EventProcessorHost. W celu zwiększenia przepływności, zaleca się uruchomienie wielu wystąpień klasy EventProcessorHost, najlepiej na oddzielnych komputerach.  Zapewnia to również nadmiarowości. W tych przypadkach różne wystąpienia automatycznie koordynują się ze sobą w celu równoważenia obciążenia odebranych zdarzeń. Jeśli chcesz, aby wiele odbiorników przetwarzało *wszystkie* zdarzenia, musisz użyć koncepcji **ConsumerGroup**. W przypadku odbierania zdarzeń z różnych komputerów dobrym rozwiązaniem może być określenie nazw wystąpień hosta EventProcessorHost w oparciu o komputery (lub role), w których są one wdrażane.
> 
> 

## <a name="next-steps"></a>Następne kroki
Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md)
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
[Azure Storage account]: ../storage/common/storage-create-storage-account.md
[Azure portal]: https://portal.azure.com
[Maven Package]: https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs-eph%22

<!-- Images -->
[11]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp2.png
[12]: ./media/service-bus-event-hubs-get-started-receive-ephjava/create-eph-csharp3.png
