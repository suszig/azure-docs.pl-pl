---
title: "Napisz program, który używa kolejek usługi Azure Service Bus | Microsoft Docs"
description: "Jak napisać aplikację konsolową w języku C# na potrzeby obsługi komunikatów w usłudze Service Bus"
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
ms.date: 03/23/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: f92909e0098a543f99baf3df3197a799bc9f1edc
ms.openlocfilehash: 83649bdad1d369cdfe4edf3c2bdaa67180db8668
ms.contentlocale: pl-pl
ms.lasthandoff: 02/28/2017


---
# <a name="get-started-with-service-bus-queues"></a>Wprowadzenie do kolejek usługi Service Bus
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

## <a name="what-will-be-accomplished"></a>Co zostanie osiągnięte?
W ramach tego samouczka zostaną wykonane następujące czynności:

1. Utworzenie przestrzeni nazw usługi Service Bus za pomocą usługi Azure Portal.
2. Utworzenie kolejki obsługi komunikatów usługi Service Bus za pomocą usługi Azure Portal.
3. Napisanie aplikacji konsolowej służącej do wysyłania komunikatu.
4. Napisanie aplikacji konsolowej służącej do odbierania komunikatów.

## <a name="prerequisites"></a>Wymagania wstępne
1. [Program Visual Studio w wersji 2015 lub nowszej](http://www.visualstudio.com). W przykładach znajdujących się w tym samouczku używany jest program Visual Studio 2015.
2. Subskrypcja platformy Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Tworzenie przestrzeni nazw za pomocą usługi Azure Portal
Jeśli przestrzeń nazw usługi Service Bus została już utworzona, przejdź do sekcji [Tworzenie kolejki za pomocą usługi Azure Portal](#2-create-a-queue-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-queue-using-the-azure-portal"></a>2. Tworzenie kolejki za pomocą usługi Azure Portal
Jeśli kolejka usługi Service Bus została już utworzona, przejdź do sekcji [Wysyłanie komunikatów do kolejki](#3-send-messages-to-the-queue).

[!INCLUDE [service-bus-create-queue-portal](../../includes/service-bus-create-queue-portal.md)]

## <a name="3-send-messages-to-the-queue"></a>3. Wysyłanie komunikatów do kolejki
Aby wysyłać komunikaty do kolejki, zostanie napisana aplikacja konsolowa w języku C# za pomocą programu Visual Studio.

### <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej

- Otwórz program Visual Studio i utwórz nową aplikację konsolową.

### <a name="add-the-service-bus-nuget-package"></a>Dodawanie pakietu NuGet usługi Service Bus
1. Kliknij prawym przyciskiem myszy nowo utworzony projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
2. Kliknij pozycję **Przeglądaj**, wyszukaj ciąg „Microsoft Azure Service Bus”, a następnie wybierz pozycję **Microsoft Azure Service Bus**. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij to okno dialogowe.
   
    ![Wybieranie pakietu NuGet][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-queue"></a>Pisanie kodu służącego do wysyłania komunikatów do kolejki
1. Dodaj następującą instrukcję using na początku pliku Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Dodaj następujący kod do metody `Main`, ustaw zmienną **connectionString** jako parametry połączenia, które zostały uzyskane podczas tworzenia przestrzeni nazw, a następnie ustaw zmienną **queueName** jako nazwę kolejki użytą podczas tworzenia kolejki.
   
    ```csharp
    var connectionString = "<Your connection string>";
    var queueName = "<Your queue name>";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
    var message = new BrokeredMessage("This is a test message!");
    client.Send(message);
    ```
   
    Oto jak powinien wyglądać plik Program.cs.
   
    ```csharp
    using System;
    using Microsoft.ServiceBus.Messaging;
   
    namespace GettingStartedWithQueues
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "<Your connection string>";
                var queueName = "<Your queue name>";
   
                var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
                var message = new BrokeredMessage("This is a test message!");
   
                client.Send(message);
            }
        }
    }
    ```
3. Uruchom program i sprawdź usługę Azure Portal. Kliknij nazwę kolejki w bloku **Przegląd** przestrzeni nazw. Zauważ, że wartość **Liczba aktywnych komunikatów** powinna teraz wynosić 1.
   
      ![Liczba komunikatów][queue-message]

## <a name="4-receive-messages-from-the-queue"></a>4. Odbieranie komunikatów z kolejki
1. Utwórz nową aplikację konsolową i dodaj odwołanie do pakietu NuGet usługi Service Bus, podobnie jak w przypadku poprzedniej aplikacji wysyłającej.
2. Dodaj następującą instrukcję `using` na początku pliku Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Dodaj następujący kod do metody `Main`, ustaw zmienną **connectionString** jako parametry połączenia, które zostały uzyskane podczas tworzenia przestrzeni nazw, a następnie ustaw zmienną **queueName** jako nazwę kolejki użytą podczas tworzenia kolejki.
   
    ```csharp
    var connectionString = "";
    var queueName = "samplequeue";
   
    var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
    client.OnMessage(message =>
    {
      Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
      Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
    });
   
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
          var connectionString = "";
          var queueName = "samplequeue";
   
          var client = QueueClient.CreateFromConnectionString(connectionString, queueName);
   
          client.OnMessage(message =>
          {
            Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
            Console.WriteLine(String.Format("Message id: {0}", message.MessageId));
          });
   
          Console.ReadLine();
        }
      }
    }
    ```
4. Uruchom program i sprawdź portal. Zauważ, że wartość **Długość kolejki** powinna teraz wynosić 0.
   
    ![Długość kolejki][queue-message-receive]

Gratulacje! Utworzono kolejkę, wysłano komunikat i odebrano komunikat.

## <a name="next-steps"></a>Następne kroki
Zapoznaj się z naszym [repozytorium GitHub zawierającym przykłady](https://github.com/Azure-Samples/azure-servicebus-messaging-samples), które pokazują niektóre bardziej zaawansowane funkcje obsługi komunikatów usługi Azure Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-get-started-with-queues/nuget-package.png
[queue-message]: ./media/service-bus-dotnet-get-started-with-queues/queue-message.png
[queue-message-receive]: ./media/service-bus-dotnet-get-started-with-queues/queue-message-receive.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

