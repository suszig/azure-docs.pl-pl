---
title: "Wprowadzenie do kolejek usługi Azure Service Bus | Microsoft Docs"
description: "Napisz aplikację konsolową w języku C#, która korzysta z kolejek komunikatów w usłudze Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 68a34c00-5600-43f6-bbcc-fea599d500da
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/26/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: b309108b4edaf5d1b198393aa44f55fc6aca231e
ms.openlocfilehash: 99a377db6341d90d263b98e14227db61dd9beabd
ms.contentlocale: pl-pl
ms.lasthandoff: 08/15/2017

---
# <a name="get-started-with-service-bus-queues"></a>Wprowadzenie do kolejek usługi Service Bus
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Co zostanie osiągnięte?
Ten samouczek obejmuje następujące kroki:

1. Utworzenie przestrzeni nazw usługi Service Bus za pomocą usługi Azure Portal.
2. Utworzenie kolejki usługi Service Bus przy użyciu witryny Azure Portal.
3. Napisanie aplikacji konsolowej służącej do wysyłania komunikatu.
4. Napisanie aplikacji konsolowej służącej do odbierania komunikatów wysyłanych w poprzednim kroku.

## <a name="prerequisites"></a>Wymagania wstępne
1. [Program Visual Studio w wersji 2015 lub nowszej](http://www.visualstudio.com). W przykładach znajdujących się w tym samouczku używany jest program Visual Studio 2017.
2. Subskrypcja platformy Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Tworzenie przestrzeni nazw za pomocą usługi Azure Portal
Jeśli przestrzeń nazw obsługi komunikatów usługi Service Bus została już utworzona, przejdź do sekcji [Tworzenie kolejki za pomocą witryny Azure Portal](#2-create-a-queue-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Tworzenie kolejki za pomocą usługi Azure Portal
Jeśli kolejka usługi Service Bus została już utworzona, przejdź do sekcji [Wysyłanie komunikatów do kolejki](#3-send-messages-to-the-queue).

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Wysyłanie komunikatów do kolejki
Aby wysyłać komunikaty do kolejki, napiszemy aplikację konsolową w języku C# za pomocą programu Visual Studio.

### <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej

Uruchom program Visual Studio i utwórz nowy projekt **Aplikacja konsoli (.NET Framework)**.

### <a name="add-the-service-bus-nuget-package"></a>Dodawanie pakietu NuGet usługi Service Bus
1. Kliknij prawym przyciskiem myszy nowo utworzony projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
2. Kliknij kartę **Przeglądaj**, wyszukaj ciąg **Microsoft Azure Service Bus**, a następnie wybierz element **WindowsAzure.ServiceBus**. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij to okno dialogowe.
   
    ![Wybieranie pakietu NuGet][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Pisanie kodu służącego do wysyłania komunikatów do kolejki
1. Dodaj następującą instrukcję `using` na początku pliku Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Dodaj następujący kod do metody `Main`: Ustaw jako zmienną `connectionString` parametry połączenia, które zostały uzyskane podczas tworzenia przestrzeni nazw, a następnie ustaw jako zmienną `queueName` nazwę kolejki użytą podczas tworzenia kolejki.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

    client.Send(message);

    Console.WriteLine("Message successfully sent! Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Oto jak powinien wyglądać plik Program.cs:
   
    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;

    namespace qsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var queueName = "<your queue name>";

                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Uruchom program i sprawdź witrynę Azure Portal: kliknij nazwę swojej kolejki w bloku **Przegląd** przestrzeni nazw. Zostanie wyświetlony blok **Podstawowe elementy** kolejki. Zauważ, że wartość **Liczba aktywnych komunikatów** powinna teraz wynosić 1. Przy każdym uruchomieniu aplikacji nadawcy bez pobierania komunikatów ta wartość zwiększa się o 1. Należy również pamiętać, że bieżący rozmiar kolejki rośnie za każdym razem, gdy aplikacja dodaje komunikat do kolejki.
   
      ![Rozmiar komunikatu][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Odbieranie komunikatów z kolejki

1. Aby odbierać komunikaty wysłane w poprzednim kroku, utwórz nową aplikację konsolową i dodaj odwołanie do pakietu NuGet usługi Service Bus, podobnie jak w przypadku poprzedniej aplikacji nadawcy.
2. Dodaj następującą instrukcję `using` na początku pliku Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Dodaj następujący kod do metody `Main`: Ustaw jako zmienną `connectionString` parametry połączenia, które zostały uzyskane podczas tworzenia przestrzeni nazw, a następnie ustaw jako zmienną `queueName` nazwę kolejki użytą podczas tworzenia kolejki.
   
    ```csharp
    var connectionString = "<your connection string>";
    var queueName = "<your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
    Console.WriteLine("Press ENTER to exit program");
    Console.ReadLine();
    ```
   
    Oto jak powinien wyglądać plik Program.cs:
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var queueName = "<your queue name>";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });

          Console.WriteLine("Press ENTER to exit program");   
          Console.ReadLine();
        }
      }
    }
    ```
4. Uruchom program i ponownie sprawdź portal. Zauważ, że wartości **Liczba aktywnych komunikatów** i **Bieżące** powinny teraz wynosić 0.
   
    ![Długość kolejki][queue-message-receive]

Gratulacje! Utworzono kolejkę, wysłano komunikat i odebrano komunikat.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z naszym [repozytorium GitHub zawierającym przykłady](https://github.com/Azure/azure-service-bus/tree/master/samples), które pokazują niektóre bardziej zaawansowane funkcje obsługi komunikatów usługi Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

