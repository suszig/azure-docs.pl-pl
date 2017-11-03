---
title: "Rozpoczynanie pracy z magazynem kolejek i Visual Studio połączone usługi (platformy ASP.NET Core) | Dokumentacja firmy Microsoft"
description: "Jak rozpocząć pracę przy użyciu magazynu kolejek Azure w projekcie platformy ASP.NET Core w programie Visual Studio"
services: storage
documentationcenter: 
author: kraigb
manager: ghogen
editor: 
ms.assetid: 04977069-5b2d-4cba-84ae-9fb2f5eb1006
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: vs-getting-started
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: kraigb
ms.openlocfilehash: 394344c0e126472b97c2e8f721c8c8d6514a17dc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-queue-storage-and-visual-studio-connected-services-aspnet-core"></a>Rozpoczynanie pracy z magazynem kolejek i Visual Studio połączone usługi (platformy ASP.NET Core)
[!INCLUDE [storage-try-azure-tools-queues](../../includes/storage-try-azure-tools-queues.md)]

## <a name="overview"></a>Omówienie
W tym artykule opisano, jak rozpocząć pracę przy użyciu magazynu kolejek Azure w programie Visual Studio po utworzony lub odwołanie do konta magazynu Azure w projekcie platformy ASP.NET Core za pomocą programu Visual Studio **dodać usług połączonych** okna dialogowego. **Dodać usług połączonych** operacji instaluje odpowiednie pakiety NuGet dostęp do magazynu Azure do projektu i dodaje ten ciąg połączenia dla konta magazynu do plików konfiguracji projektu.

Magazyn kolejek Azure to usługa do przechowywania dużej liczby komunikatów, które są dostępne z dowolnego miejsca na świecie za pośrednictwem uwierzytelnionego połączenia za pomocą protokołu HTTP lub HTTPS. Pojedynczy komunikat z kolejki może być maksymalnie 64 kilobajty (KB), rozmiar, a kolejka może zawierać miliony komunikatów — maksymalnie całkowitego limitu pojemności konta magazynu.

Aby rozpocząć pracę, należy najpierw utworzyć kolejce platformy Azure na koncie magazynu. Poniżej opisano sposób tworzenia kolejki w kodzie. Możemy również opisano sposób wykonywania kolejki podstawowe operacje, takie jak dodawanie, modyfikowanie, Odczyt i usuwanie wiadomości w kolejce. Przykłady są napisane w języku C\# kodu i używanie biblioteki klienta magazynu Azure dla platformy .NET. Aby uzyskać więcej informacji na temat platformy ASP.NET, zobacz [ASP.NET](http://www.asp.net).

**Uwaga:** niektórych interfejsów API, które wykonywania wywołań do magazynu Azure w ASP.NET Core są asynchroniczne. Zobacz [programowanie asynchroniczne z Async i Await](http://msdn.microsoft.com/library/hh191443.aspx) Aby uzyskać więcej informacji. Poniższy kod przyjęto założenie, że są używane metody programowania asynchronicznego.

* Zobacz [Rozpoczynanie pracy z magazynem kolejek Azure przy użyciu platformy .NET](../storage/queues/storage-dotnet-how-to-use-queues.md) uzyskać więcej informacji o programowo manipulowanie kolejek.
* Zobacz [dokumentacji magazynu](https://azure.microsoft.com/documentation/services/storage/) ogólne informacje na temat usługi Azure Storage.
* Zobacz [dokumentacji usługi w chmurze](https://azure.microsoft.com/documentation/services/cloud-services/) ogólne informacje dotyczące usług w chmurze Azure.
* Zobacz [ASP.NET](http://www.asp.net) Aby uzyskać więcej informacji na temat programowania aplikacji ASP.NET.

## <a name="access-queues-in-code"></a>Uzyskiwanie dostępu do kolejek w kodzie
Aby uzyskać dostęp do kolejki w projektach platformy ASP.NET Core, musisz obejmują następujące elementy do dowolnego języka C# plik źródłowy, który uzyskuje dostęp do magazynu kolejek Azure.

1. Upewnij się, że deklaracje przestrzeni nazw w górnej części pliku C# Uwzględnij je **przy użyciu** instrukcje.
   
        using Microsoft.Framework.Configuration;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Queue;
        using System.Threading.Tasks;
        using LogLevel = Microsoft.Framework.Logging.LogLevel;
2. Pobierz **CloudStorageAccount** obiekt, który reprezentuje informacje o koncie magazynu. Użyj następującego kodu można pobrać parametry połączenia magazynu, a informacje o koncie magazynu z konfiguracji usługi Azure.
   
         CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
           CloudConfigurationManager.GetSetting("<storage-account-name>_AzureStorageConnectionString"));
3. Pobierz **CloudQueueClient** obiektu odwołują się obiekty w koncie magazynu.  
   
        // Create the CloudQueueClient object for the storage account.
        CloudQueueClient queueClient = storageAccount.CreateCloudQueueClient();
4. Pobierz **CloudQueue** obiekt do odwoływania się do określonej kolejki.
   
        // Get a reference to the CloudQueue named "messageQueue"
        CloudQueue messageQueue = queueClient.GetQueueReference("messageQueue");

**Uwaga:** korzystać ze wszystkich powyższych kodu przed kod w następujących przykładach.

### <a name="create-a-queue-in-code"></a>Tworzenie kolejki w kodzie
Aby utworzyć kolejkę Azure w kodzie, wystarczy dodać wywołanie **CreateIfNotExistsAsync**.

    // Create the CloudQueue if it does not exist.
    await messageQueue.CreateIfNotExistsAsync();

## <a name="add-a-message-to-a-queue"></a>Dodaj komunikat do kolejki
Aby wstawić komunikat do istniejącej kolejki, Utwórz nową **CloudQueueMessage** obiekt, a następnie wywołaj **AddMessageAsync** metody.

A **CloudQueueMessage** obiektu można utworzyć z ciągu (w formacie UTF-8) lub tablicy bajtów.

Oto przykład, która wstawia komunikat "Hello, World".

    // Create a message and add it to the queue.
    CloudQueueMessage message = new CloudQueueMessage("Hello, World");
    await messageQueue.AddMessageAsync(message);

## <a name="read-a-message-in-a-queue"></a>Przeczytaj wiadomość w kolejce
Możesz uzyskać wgląd w komunikat z przodu kolejki bez jego usuwania z kolejki, wywołując **PeekMessageAsync** metody.

    // Peek the next message in the queue. 
    CloudQueueMessage peekedMessage = await messageQueue.PeekMessageAsync();


## <a name="read-and-remove-a-message-in-a-queue"></a>Przeczytaj i usuwanie wiadomości w kolejce
Można usunąć kodu (dequeue) wiadomość z kolejki w dwóch krokach.

1. Wywołanie **GetMessageAsync** do pobrania następnej wiadomości w kolejce. Komunikat zwrócony z **GetMessageAsync** staje się niewidoczny dla innego kodu odczytującego komunikaty z tej kolejki. Domyślnie komunikat pozostanie niewidoczny przez 30 sekund.
2. Aby zakończyć usuwanie komunikatu z kolejki, należy wywołać **DeleteMessageAsync**.

Ten dwuetapowy proces usuwania komunikatów gwarantuje, że jeśli kod nie będzie w stanie przetworzyć komunikatu z powodu awarii sprzętu lub oprogramowania, inne wystąpienie kodu będzie w stanie uzyskać ten sam komunikat i ponowić próbę. Poniższy kod wywołania **DeleteMessageAsync** natychmiast po przetworzeniu komunikatu.

    // Get the next message in the queue.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();

    // Process the message in less than 30 seconds.

    // Then delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);

## <a name="leverage-additional-options-for-dequeuing-messages"></a>Wykorzystanie dodatkowych opcji usuwania komunikatów
Istnieją dwa sposoby dostosowania pobierania komunikatów z kolejki.
Po pierwsze można uzyskać komunikaty zbiorczo (do 32). Po drugie można ustawić dłuższy lub krótszy limit czasu niewidoczności, dzięki czemu kod będzie mieć więcej lub mniej czasu na pełne przetworzenie każdego komunikatu. Poniższy przykład kodu wykorzystuje metodę **GetMessages**, aby pobrać 20 komunikatów w jednym wywołaniu. Następnie przetwarza każdy komunikat przy użyciu pętli **foreach**. Ustawia również limitu czasu niewidoczności na 5 minut dla każdego komunikatu. Należy pamiętać, że 5 minut rozpocząć dla wszystkich wiadomości w tym samym czasie, więc po 5 minut przekazany po wywołaniu **GetMessages**, wszystkie komunikaty, które nie zostały usunięte stają się widoczne ponownie.

    // Retrieve 20 messages at a time, keeping those messages invisible for 5 minutes, 
    //   delete each message after processing.

    foreach (CloudQueueMessage message in messageQueue.GetMessages(20, TimeSpan.FromMinutes(5)))
    {
        // Process all messages in less than 5 minutes, deleting each message after processing.
        queue.DeleteMessage(message);
    }

## <a name="get-the-queue-length"></a>Pobieranie długości kolejki
Możesz uzyskać szacunkową liczbę komunikatów w kolejce. **FetchAttributes** metody prosi usługę kolejki o pobranie atrybutów kolejki, w tym liczby komunikatów. **ApproximateMethodCount** właściwość zwraca ostatnią wartość pobraną przez **FetchAttributes** bez wywoływania usługi kolejki.

    // Fetch the queue attributes.
    messageQueue.FetchAttributes();

    // Retrieve the cached approximate message count.
    int? cachedMessageCount = messageQueue.ApproximateMessageCount;

    // Display the number of messages.
    Console.WriteLine("Number of messages in queue: " + cachedMessageCount);

## <a name="use-the-async-await-pattern-with-common-queue-apis"></a>Użyj wzorca Async-Await z kolejką wspólnych interfejsów API
Ten przykład przedstawia sposób użycia wzorca Async-Await z kolejką wspólnych interfejsów API. Przykład wywołuje wersji async każdego z danej metody. Można to zaobserwować przez Async poprawkę po każdej metody. Jeśli zostanie użyta metoda asynchroniczna, wzorzec Async-Await zawiesi lokalne wykonanie do czasu ukończenia wywołania. Takie zachowanie umożliwia wykonywanie innych zadań, co pomaga uniknąć wąskich gardeł zmniejszających wydajność i poprawia ogólną szybkość reakcji aplikacji bieżącego wątku. Aby uzyskać więcej informacji o wykorzystaniu wzorca Async-Await w programie .NET, zobacz [Async i Await (C# i Visual Basic)](https://msdn.microsoft.com/library/hh191443.aspx)

    // Create a message to add to the queue.
    CloudQueueMessage cloudQueueMessage = new CloudQueueMessage("My message");

    // Async enqueue the message.
    await messageQueue.AddMessageAsync(cloudQueueMessage);
    Console.WriteLine("Message added");

    // Async dequeue the message.
    CloudQueueMessage retrievedMessage = await messageQueue.GetMessageAsync();
    Console.WriteLine("Retrieved message with content '{0}'", retrievedMessage.AsString);

    // Async delete the message.
    await messageQueue.DeleteMessageAsync(retrievedMessage);
    Console.WriteLine("Deleted message");
## <a name="delete-a-queue"></a>Usuwanie kolejki
Aby usunąć kolejkę i wszystkie zawarte w niej komunikaty, wywołaj metodę **Delete** na obiekcie kolejki.

    // Delete the queue.
    messageQueue.Delete();


## <a name="next-steps"></a>Następne kroki
[!INCLUDE [vs-storage-dotnet-queues-next-steps](../../includes/vs-storage-dotnet-queues-next-steps.md)]

