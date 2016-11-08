## Wysyłanie komunikatów do usługi Event Hubs
W tej sekcji przedstawiono tworzenie aplikacji konsoli systemu Windows, która wysyła zdarzenia do Centrum zdarzeń.

1. W programie Visual Studio utwórz nowy projekt aplikacji klasycznej Visual C# za pomocą szablonu projektu **Aplikacja konsoli**. Nazwij projekt **Nadawca**.
   
    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp1.png)
2. W Eksploratorze rozwiązań kliknij rozwiązanie prawym przyciskiem myszy, a następnie kliknij pozycję **Zarządzaj pakietami NuGet dla rozwiązania**. 
3. Kliknij kartę **Przeglądanie**, a następnie wyszukaj ciąg `Microsoft Azure Service Bus`. Pamiętaj, aby określić nazwę projektu (**Nadawca**) w polu **Wersje**. Kliknij pozycję **Zainstaluj** i zaakceptuj warunki użytkowania. 
   
    ![](./media/service-bus-event-hubs-getstarted-send-csharp/create-sender-csharp2.png)
   
    Program Visual Studio pobierze, zainstaluje i doda odniesienia do [pakietu NuGet biblioteki usługi Azure Service Bus](https://www.nuget.org/packages/WindowsAzure.ServiceBus).
4. Dodaj następujące instrukcje `using` w górnej części pliku **Program.cs**:
   
    ```
    using System.Threading;
    using Microsoft.ServiceBus.Messaging;
    ```
5. Dodaj następujące pola do klasy **Program**, zastępując symbole zastępcze nazwą Centrum zdarzeń utworzonego w poprzedniej sekcji oraz zapisanymi wcześniej parametrami połączenia na poziomie przestrzeni nazw.
   
    ```
    static string eventHubName = "{Event Hub name}";
    static string connectionString = "{send connection string}";
    ```
6. Dodaj następującą metodę do klasy **Program**:
   
    ```
    static void SendingRandomMessages()
    {
        var eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, eventHubName);
        while (true)
        {
            try
            {
                var message = Guid.NewGuid().ToString();
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, message);
                eventHubClient.Send(new EventData(Encoding.UTF8.GetBytes(message)));
            }
            catch (Exception exception)
            {
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine("{0} > Exception: {1}", DateTime.Now, exception.Message);
                Console.ResetColor();
            }
   
            Thread.Sleep(200);
        }
    }
    ```
   
    Ta metoda stale wysyła zdarzenia do centrum zdarzeń z opóźnieniem 200 ms.
7. Na koniec dodaj następujące wiersze do metody **Główne**:
   
    ```
    Console.WriteLine("Press Ctrl-C to stop the sender process");
    Console.WriteLine("Press Enter to start now");
    Console.ReadLine();
    SendingRandomMessages();
    ```

<!--HONumber=Sep16_HO3-->


