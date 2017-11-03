---
title: "Wysyłanie zdarzeń do usługi Azure Event Hubs przy użyciu języka Java | Dokumentacja firmy Microsoft"
description: "Wprowadzenie do wysyłania do usługi Event Hubs przy użyciu języka Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: b31771001989e20b88bc8d7bca1afceb58ec197c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="send-events-to-azure-event-hubs-using-java"></a>Wysyłanie zdarzeń do usługi Azure Event Hubs przy użyciu języka Java

## <a name="introduction"></a>Wprowadzenie
Event Hubs to wysoce skalowalny system przyjmowania może obsługiwać miliony zdarzeń na sekundę, włączanie aplikacji do przetwarzania i analizowanie olbrzymich ilości danych wytworzonych przez podłączone urządzenia i aplikacje. Po zebraniu danych do Centrum zdarzeń, można przekształcić i przechowywanie danych przy użyciu dowolnego dostawcy analiz w czasie rzeczywistym lub magazynu klastra.

Aby uzyskać więcej informacji, zobacz [Przegląd usługi Event Hubs][Event Hubs overview].

W tym samouczku przedstawiono sposób wysyłania zdarzeń do Centrum zdarzeń za pomocą aplikacji konsoli w języku Java. Aby odbierać zdarzenia przy użyciu biblioteki hosta procesora zdarzeń Java, zobacz [w tym artykule](event-hubs-java-get-started-receive-eph.md), lub kliknij odpowiedni język odbierania w tabeli po lewej stronie zawartości.

W celu ukończenia tego samouczka potrzebne następujące elementy:

* Środowisko projektowe Java. W tym samouczku przyjęto założenie, [Eclipse](https://www.eclipse.org/).
* Aktywne konto platformy Azure. <br/>Jeśli go nie masz, możesz utworzyć bezpłatne konto próbne w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz artykuł <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Bezpłatna wersja próbna platformy Azure</a>.

## <a name="send-messages-to-event-hubs"></a>Wysyłanie komunikatów do usługi Event Hubs
Java biblioteki klienta usługi Event hubs jest dostępny do użytku w projekty Maven z [Maven centralnym repozytorium](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Możesz odwoływać się do tej biblioteki w pliku projektu Maven za pomocą deklaracji następujące zależności:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

Dla różnych typów środowiska kompilacji, można jawnie uzyskać najnowsze wydanych JAR pliki z [Maven centralnym repozytorium](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

Wydawca zdarzeń proste, można zaimportować *com.microsoft.azure.eventhubs* pakiet dla klasy klienta usługi Event Hubs i *com.microsoft.azure.servicebus* pakietu dla klasy narzędzi, takich jak Typowe wyjątki, które są współużytkowane z klienta poczty e-mail usługi Azure Service Bus. 

Na potrzeby poniższego przykładu należy w ulubionym środowisku programowania Java utworzyć nowy projekt Maven dla aplikacji konsoli lub powłoki. Nazwa klasy `Send`.     

```java
import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Zastąp nazwy Centrum przestrzeni nazw i zdarzeń wartości używane podczas tworzenia Centrum zdarzeń.

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Następnie utwórz zdarzenie pojedynczej przez przekształcania ciąg w jego kodowania UTF-8 bajtów. Następnie utwórz nowe wystąpienie klienta usługi Event Hubs z parametrów połączenia i wysłać wiadomość.   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a>Następne kroki
Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Zdarzenia są rejestrowane za pomocą klasy EventProcessorHost](event-hubs-java-get-started-receive-eph.md)
* [Przegląd usługi Event Hubs][Event Hubs overview]
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md