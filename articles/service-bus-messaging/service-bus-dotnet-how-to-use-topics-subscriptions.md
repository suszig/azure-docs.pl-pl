---
title: "Rozpoczynanie pracy z tematami i subskrypcjami usługi Azure Service Bus | Microsoft Docs"
description: "Napisz aplikację konsolową w języku C#, która korzysta z tematów i subskrypcji komunikatów usługi Service Bus."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: hero-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/30/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 6efa2cca46c2d8e4c00150ff964f8af02397ef99
ms.openlocfilehash: 9401ada519f600b0d2817f06a396e16607a24129
ms.contentlocale: pl-pl
ms.lasthandoff: 07/01/2017

---
# <a name="get-started-with-service-bus-topics"></a>Wprowadzenie do tematów usługi Service Bus

[!INCLUDE [service-bus-selector-topics](../../includes/service-bus-selector-topics.md)]

## <a name="what-will-be-accomplished"></a>Co zostanie osiągnięte?

Ten samouczek obejmuje następujące kroki:

1. Utworzenie przestrzeni nazw usługi Service Bus za pomocą usługi Azure Portal.
2. Utworzenie tematu usługi Service Bus przy użyciu witryny Azure Portal.
3. Utworzenie subskrypcji tego tematu usługi Service Bus przy użyciu witryny Azure Portal.
4. Napisanie aplikacji konsolowej służącej do wysyłania komunikatu do tematu.
5. Napisanie aplikacji konsolowej służącej do odbierania tego komunikatu z subskrypcji.

## <a name="prerequisites"></a>Wymagania wstępne

1. [Program Visual Studio w wersji 2015 lub nowszej](http://www.visualstudio.com). W przykładach znajdujących się w tym samouczku używany jest program Visual Studio 2017.
2. Subskrypcja platformy Azure.

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. Tworzenie przestrzeni nazw za pomocą usługi Azure Portal

Jeśli przestrzeń nazw obsługi komunikatów usługi Service Bus została już utworzona, przejdź do sekcji [Tworzenie tematu przy użyciu witryny Azure Portal](#2-create-a-topic-using-the-azure-portal).

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="2-create-a-topic-using-the-azure-portal"></a>2. Tworzenie tematu przy użyciu witryny Azure Portal

1. Zaloguj się w witrynie [Azure Portal][azure-portal].
2. W lewym okienku nawigacji portalu kliknij pozycję **Service Bus** (jeśli pozycja **Service Bus** nie jest widoczna, kliknij pozycję **Więcej usług**).
3. Kliknij przestrzeń nazw, w której chcesz utworzyć temat. Zostanie wyświetlony blok Przegląd przestrzeni nazw:
   
    ![Tworzenie tematu][createtopic1]
4. W bloku **Przestrzeń nazw usługi Service Bus** kliknij pozycję **Tematy**, a następnie kliknij przycisk **Dodaj temat**.
   
    ![Wybieranie tematów][createtopic2]
5. Wprowadź nazwę tematu i usuń zaznaczenie opcji **Włącz partycjonowanie**. Pozostaw inne opcje z wartościami domyślnymi.
   
    ![Wybieranie nowych kolejek][createtopic3]
6. W dolnej części bloku kliknij pozycję **Utwórz**.

## <a name="3-create-a-subscription-to-the-topic"></a>3. Tworzenie subskrypcji tematu

1. W okienku zasobów portalu kliknij przestrzeń nazw utworzoną w kroku 1, a następnie kliknij nazwę tematu utworzonego w kroku 2.
2. W górnej części okienka Przegląd kliknij znak plus obok pozycji **Subskrypcja**, aby dodać subskrypcję do tego tematu.

    ![Tworzenie subskrypcji][createtopic4]

3. Wprowadź nazwę subskrypcji. Pozostaw inne opcje z wartościami domyślnymi.

## <a name="4-send-messages-to-the-topic"></a>4. Wysyłanie komunikatów do tematu

Aby wysyłać komunikaty do tematu, napiszemy aplikację konsolową w języku C# za pomocą programu Visual Studio.

### <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej

Uruchom program Visual Studio i utwórz nowy projekt **Aplikacja konsoli (.NET Framework)**.

### <a name="add-the-service-bus-nuget-package"></a>Dodawanie pakietu NuGet usługi Service Bus

1. Kliknij prawym przyciskiem myszy nowo utworzony projekt i wybierz pozycję **Zarządzaj pakietami NuGet**.
2. Kliknij kartę **Przeglądaj**, wyszukaj ciąg **Microsoft Azure Service Bus**, a następnie wybierz element **WindowsAzure.ServiceBus**. Kliknij przycisk **Zainstaluj**, aby ukończyć instalację, a następnie zamknij to okno dialogowe.
   
    ![Wybieranie pakietu NuGet][nuget-pkg]

### <a name="write-some-code-to-send-a-message-to-the-topic"></a>Pisanie kodu służącego do wysyłania komunikatów do tematu

1. Dodaj następującą instrukcję `using` na początku pliku Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
2. Dodaj następujący kod do metody `Main`: Ustaw jako zmienną `connectionString` parametry połączenia, które zostały uzyskane podczas tworzenia przestrzeni nazw, a następnie ustaw jako zmienną `topicName` nazwę tematu użytą podczas tworzenia tematu.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
    var message = new BrokeredMessage("This is a test message!");

    Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
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

    namespace tsend
    {
        class Program
        {
            static void Main(string[] args)
            {
                var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";
                var topicName = "<your topic name>";

                var client = TopicClient.CreateFromConnectionString(connectionString, topicName);
                var message = new BrokeredMessage("This is a test message!");

                Console.WriteLine(String.Format("Message body: {0}", message.GetBody<String>()));
                Console.WriteLine(String.Format("Message id: {0}", message.MessageId));

                client.Send(message);

                Console.WriteLine("Message successfully sent! Press ENTER to exit program");
                Console.ReadLine();
            }
        }
    }
    ```
3. Uruchom program i sprawdź witrynę Azure Portal: kliknij nazwę swojego tematu w bloku **Przegląd** przestrzeni nazw. Zostanie wyświetlony blok **Podstawowe elementy** tematu. W subskrypcjach wyświetlanych w dolnej części bloku wartość pola **Liczba komunikatów** dla każdej subskrypcji powinna teraz wynosić 1. Przy każdym uruchomieniu aplikacji nadawcy bez pobierania komunikatów (zgodnie z opisem w następnej sekcji) ta wartość zwiększa się o 1. Należy również zauważyć, że bieżący rozmiar tematu zwiększa wartość **Bieżące** w bloku **Podstawowe elementy** za każdym razem, kiedy aplikacja dodaje komunikat do tematu/subskrypcji.
   
      ![Rozmiar komunikatu][topic-message]

## <a name="5-receive-messages-from-the-subscription"></a>5. Odbieranie komunikatów z subskrypcji

1. Aby odbierać komunikat (lub komunikaty) wysłane w poprzednim kroku, utwórz nową aplikację konsolową i dodaj odwołanie do pakietu NuGet usługi Service Bus, podobnie jak w przypadku poprzedniej aplikacji nadawcy.
2. Dodaj następującą instrukcję `using` na początku pliku Program.cs.
   
    ```csharp
    using Microsoft.ServiceBus.Messaging;
    ```
3. Dodaj następujący kod do metody `Main`: Ustaw jako zmienną `connectionString` parametry połączenia, które zostały uzyskane podczas tworzenia przestrzeni nazw, a następnie ustaw jako zmienną `topicName` nazwę tematu użytą podczas tworzenia tematu.
   
    ```csharp
    var connectionString = "<your connection string>";
    var topicName = "<your topic name>";
   
    var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
   
    namespace GettingStartedWithTopics
    {
      class Program
      {
        static void Main(string[] args)
        {
          var connectionString = "Endpoint=sb://<your namespace>.servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=<your key>";;
          var topicName = "<your topic name>";
   
          var client = SubscriptionClient.CreateFromConnectionString(connectionString, topicName, "<your subscription name>");
   
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
4. Uruchom program i ponownie sprawdź portal. Zauważ, że wartości **Liczba komunikatów** i **Bieżące** powinny teraz wynosić 0.
   
    ![Długość tematu][topic-message-receive]

Gratulacje! Zostały utworzone temat i subskrypcja, wysłano komunikat i odebrano ten komunikat.

## <a name="next-steps"></a>Następne kroki

Zapoznaj się z naszym [repozytorium GitHub zawierającym przykłady](https://github.com/Azure/azure-service-bus/tree/master/samples), które pokazują niektóre bardziej zaawansowane funkcje obsługi komunikatów usługi Service Bus.

<!--Image references-->

[nuget-pkg]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/nuget-package.png
[topic-message]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message.png
[topic-message-receive]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/topic-message-receive.png
[createtopic1]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic1.png
[createtopic2]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic2.png
[createtopic3]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic3.png
[createtopic4]: ./media/service-bus-dotnet-how-to-use-topics-subscriptions/create-topic4.png
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples
[azure-portal]: https://portal.azure.com

