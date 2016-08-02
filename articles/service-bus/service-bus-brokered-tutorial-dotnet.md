<properties 
    pageTitle="Samouczek dotyczący komunikatów obsługiwanych przez brokera w usłudze Service Bus dla platformy .NET | Microsoft Azure"
    description="Samouczek dotyczący komunikatów obsługiwanych przez brokera dla platformy .NET."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="05/25/2016"
    ms.author="sethm" />

# Samouczek dotyczący komunikatów obsługiwanych przez brokera w usłudze Service Bus dla platformy .NET

Usługa Azure Service Bus udostępnia dwa kompleksowe rozwiązania do obsługi komunikatów — pierwsze za pośrednictwem scentralizowanej usługi „przekaźnika” działającej w chmurze, która obsługuje wiele różnych protokołów i standardów sieci Web, w tym SOAP, WS-* i REST. Klient nie potrzebuje bezpośredniego połączenia z usługą lokalną ani nie musi wiedzieć, gdzie usługa się znajduje. Usługa lokalna nie wymaga otwarcia w zaporze żadnych portów przychodzących.

Drugie rozwiązanie do obsługi komunikatów włącza funkcje komunikatów obsługiwanych przez brokera. Te można traktować jako funkcje asynchronicznej lub oddzielonej obsługi komunikatów, obsługujące scenariusze publikacji/subskrypcji, oddzielenia czasowego i równoważenia obciążenia z wykorzystaniem infrastruktury obsługi komunikatów usługi Service Bus. Komunikacja oddzielona ma wiele zalet, na przykład klienci i serwery mogą nawiązywać połączenie zgodnie z potrzebami i wykonywać ich operacje w sposób asynchroniczny.

Ten samouczek ma umożliwić przegląd oraz zdobycie praktycznego doświadczenia z kolejkami, jednym z podstawowych składników funkcji obsługi komunikatów przez brokera w usłudze Service Bus. Po zakończeniu pracy z sekwencją tematów w tym samouczku będziesz mieć aplikację, która zapełnia listę komunikatów, tworzy kolejkę i wysyła wiadomości do tej kolejki. Na koniec aplikacja odbiera i wyświetla komunikaty z kolejki, a następnie czyści jej zasoby i zamyka. Odpowiedni samouczek, w którym opisano sposób kompilowania aplikacji korzystającej z funkcji obsługi wiadomości przez przekaźnik w usłudze Service Bus, można znaleźć w artykule [Samouczek dotyczący komunikatów obsługiwanych przez przekaźnik w usłudze Service Bus](service-bus-relay-tutorial.md).

## Wprowadzenie i wymagania wstępne

Kolejki oferują dostarczanie komunikatów metodą pierwszy na wejściu — pierwszy na wyjściu (FIFO) do jednego lub większej liczby konkurencyjnych odbiorców. Metoda FIFO oznacza, że komunikaty są zwykle odbierane i przetwarzane przez odbiorców w kolejności, w której zostały dodane do kolejki, a każdy komunikat jest odbierany i przetwarzany przez tylko jednego odbiorcę komunikatów. Najważniejszą korzyścią z używania kolejek jest osiągnięcie *oddzielenia czasowego* składników aplikacji; innymi słowy, producenci i konsumenci nie muszą wysyłać i odbierać komunikatów w tym samym czasie, ponieważ komunikaty są trwale przechowywane w kolejce. Pokrewną korzyścią jest *wyrównywanie obciążenia*, które umożliwia producentom i odbiorcom wysyłanie i odbieranie komunikatów z różną szybkością.

Poniżej przedstawiono niektóre czynności administracyjne i kroki dotyczące wymagań wstępnych, które należy wykonać przed rozpoczęciem samouczka. Pierwszym krokiem jest utworzenie przestrzeni nazw usługi i uzyskanie klucza sygnatury dostępu współdzielonego. Przestrzeń nazw wyznacza granice każdej aplikacji uwidacznianej za pośrednictwem usługi Service Bus. Klucz sygnatury dostępu współdzielonego jest automatycznie generowany przez system po utworzeniu przestrzeni nazw usługi. Kombinacja przestrzeni nazw usługi i klucza sygnatury dostępu współdzielonego dostarcza poświadczenia dla usługi Service Bus do uwierzytelniania dostępu do aplikacji.

### Tworzenie przestrzeni nazw usługi i uzyskiwanie klucza sygnatury dostępu współdzielonego

1. Aby utworzyć przestrzeń nazw usługi, przejdź do [klasycznego portalu Azure][]. Kliknij pozycję **Service Bus** po lewej stronie, a następnie kliknij pozycję **Utwórz**. Wpisz nazwę przestrzeni nazw, a następnie kliknij znacznik wyboru.

1. W oknie głównym portalu kliknij nazwę przestrzeni nazw utworzonej w poprzednim kroku.

1. Kliknij pozycję **Konfiguruj**.

1. Zanotuj klucz podstawowy skojarzony z zasadami **RootManageSharedAccessKey** lub skopiuj go do schowka. Ta wartość zostanie użyta w dalszej części tego samouczka.

Następnym krokiem jest utworzenie projektu programu Visual Studio i napisanie dwóch funkcji pomocnika, które ładują rozdzielaną przecinkami listę komunikatów do silnie typizowanego obiektu [List](https://msdn.microsoft.com/library/6sh2ey19.aspx) klasy [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx) platformy .NET.

### Tworzenie projektu programu Visual Studio

1. Otwórz program Visual Studio jako administrator, klikając prawym przyciskiem myszy ikonę programu w menu Start, a następnie klikając polecenie **Uruchom jako administrator**.

1. Utwórz nowy projekt aplikacji konsoli. Kliknij menu **Plik** i wybierz pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. W oknie dialogowym **Nowy projekt** kliknij pozycję **Visual C#** (jeśli pozycja **Visual C#** nie jest wyświetlana, sprawdź w obszarze **Inne języki**), kliknij szablon **Aplikacja konsoli** i nadaj mu nazwę **QueueSample**. Użyj domyślnej **lokalizacji**. Kliknij przycisk **OK**, aby utworzyć projekt.

1. Użyj menedżera pakietów NuGet, aby dodać biblioteki usługi Service Bus do projektu:
    1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy projekt **QueueSample**, a następnie kliknij pozycję **Zarządzaj pakietami NuGet**.
    2. W oknie dialogowym **Zarządzanie pakietami NuGet** kliknij przycisk **Przeglądaj**, wyszukaj pozycję **Azure Service Bus**, a następnie kliknij pozycję **Zainstaluj**.
<br />
1. W Eksploratorze rozwiązań kliknij dwukrotnie plik Program.cs, aby otworzyć go w edytorze programu Visual Studio. Zmień nazwę przestrzeni nazw z domyślnej nazwy `QueueSample` na `Microsoft.ServiceBus.Samples`.

    ```
    Microsoft.ServiceBus.Samples
    {
        ...
    ```

1. Zmodyfikuj instrukcje `using`, tak jak pokazano w poniższym kodzie.

    ```
    using System;
    using System.Collections.Generic;
    using System.Data;
    using System.IO;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.ServiceBus.Messaging;
    ```

1. Utwórz plik tekstowy o nazwie Data.csv i skopiuj następujący tekst rozdzielony przecinkami.

    ```
    IssueID,IssueTitle,CustomerID,CategoryID,SupportPackage,Priority,Severity,Resolved
    1,Package lost,1,1,Basic,5,1,FALSE
    2,Package damaged,1,1,Basic,5,1,FALSE
    3,Product defective,1,2,Premium,5,2,FALSE
    4,Product damaged,2,2,Premium,5,2,FALSE
    5,Package lost,2,2,Basic,5,2,TRUE
    6,Package lost,3,2,Basic,5,2,FALSE
    7,Package damaged,3,7,Premium,5,3,FALSE
    8,Product defective,3,2,Premium,5,3,FALSE
    9,Product damaged,4,6,Premium,5,3,TRUE
    10,Package lost,4,8,Basic,5,3,FALSE
    11,Package damaged,5,4,Basic,5,4,FALSE
    12,Product defective,5,4,Basic,5,4,FALSE
    13,Package lost,6,8,Basic,5,4,FALSE
    14,Package damaged,6,7,Premium,5,5,FALSE
    15,Product defective,6,2,Premium,5,5,FALSE
    ```

    Zapisz i zamknij plik Data.csv i zapamiętaj lokalizację, w której został zapisany.

1. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu (w tym przykładzie **QueueSample**), kliknij polecenie **Dodaj**, a następnie kliknij pozycję **Istniejący element**.

1. Przejdź do pliku Data.csv, który został utworzony w kroku 6. Kliknij plik, a następnie kliknij pozycję **Dodaj**. Upewnij się, że na liście typów plików jest zaznaczona pozycja **Wszystkie pliki (*.*)**.

### Tworzenie metody, która analizuje listę komunikatów

1. W klasie `Program` przed metodą `Main()` zadeklaruj dwie zmienne, jedną typu **DataTable**, która będzie zawierać listę komunikatów z pliku Data.csv, i drugą typu Obiekt listy, silnie typizowaną do klasy [BrokeredMessage](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx). Druga zmienna jest listą komunikatów obsługiwanych przez brokera, która będzie używana w kolejnych krokach samouczka.

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList;
    ```

1. Poza metodą `Main()` zdefiniuj metodę `ParseCSV()`, która analizuje listę komunikatów w pliku Data.csv i ładuje komunikaty do tabeli [DataTable](https://msdn.microsoft.com/library/azure/system.data.datatable.aspx), jak pokazano tutaj. Metoda zwraca obiekt **DataTable**.

    ```
    static DataTable ParseCSVFile()
    {
        DataTable tableIssues = new DataTable("Issues");
        string path = @"..\..\data.csv";
        try
        {
            using (StreamReader readFile = new StreamReader(path))
            {
                string line;
                string[] row;
    
                // create the columns
                line = readFile.ReadLine();
                foreach (string columnTitle in line.Split(','))
                {
                    tableIssues.Columns.Add(columnTitle);
                }
    
                while ((line = readFile.ReadLine()) != null)
                {
                    row = line.Split(',');
                    tableIssues.Rows.Add(row);
                }
            }
        }
        catch (Exception e)
        {
            Console.WriteLine("Error:" + e.ToString());
        }
    
        return tableIssues;
    }
    ```

1. W metodzie `Main()` dodaj instrukcję, która wywołuje metodę `ParseCSVFile()`:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
    
    }
    ```

### Tworzenie metody, która ładuje listę komunikatów

1. Poza metodą `Main()` zdefiniuj metodę `GenerateMessages()`, która przyjmuje obiekt **DataTable** zwracany przez metodę `ParseCSVFile()` i ładuje tabelę do silnie typizowanej listy komunikatów obsługiwanych przez brokera. Następnie metoda zwraca obiekt **Lista**, jak w poniższym przykładzie. 

    ```
    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
        // Instantiate the brokered list object
        List<BrokeredMessage> result = new List<BrokeredMessage>();
    
        // Iterate through the table and create a brokered message for each row
        foreach (DataRow item in issues.Rows)
        {
            BrokeredMessage message = new BrokeredMessage();
            foreach (DataColumn property in issues.Columns)
            {
                message.Properties.Add(property.ColumnName, item[property]);
            }
            result.Add(message);
        }
        return result;
    }
    ```

1. W metodzie `Main()`, bezpośrednio po wywołaniu metody `ParseCSVFile()`, dodaj instrukcję, która wywołuje metodę `GenerateMessages()` z wartością zwracaną z metody `ParseCSVFile()` jako argumentem:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
    }
    ```

### Uzyskiwanie poświadczeń użytkownika

1. Najpierw utwórz trzy zmienne globalne ciągu do przechowywania tych wartości. Zadeklaruj te zmienne bezpośrednio po poprzednich deklaracjach zmiennych, na przykład:

    ```
    namespace Microsoft.ServiceBus.Samples
    {
        public class Program
        {
    
            private static DataTable issues;
            private static List<BrokeredMessage> MessageList; 

            // Add these variables
            private static string ServiceNamespace;
            private static string sasKeyName = "RootManageSharedAccessKey";
            private static string sasKeyValue;
            …
    ```

1. Następnie utwórz funkcję, która przyjmuje i przechowuje przestrzeń nazw usługi i klucz sygnatury dostępu współdzielonego. Dodaj tę metodę poza metodą `Main()`. Na przykład: 

    ```
    static void CollectUserInput()
    {
        // User service namespace
        Console.Write("Please enter the namespace to use: ");
        ServiceNamespace = Console.ReadLine();
    
        // Issuer key
        Console.Write("Enter the SAS key to use: ");
        sasKeyValue = Console.ReadLine();
    }
    ```

1. W metodzie `Main()` bezpośrednio po wywołaniu metody `GenerateMessages()` dodaj instrukcję, która wywołuje metodę `CollectUserInput()`:

    ```
    public static void Main(string[] args)
    {
    
        // Populate test data
        issues = ParseCSVFile();
        MessageList = GenerateMessages(issues);
        
        // Collect user input
        CollectUserInput();
    }
    ```

### Kompilowanie rozwiązania

W menu **Kompilacja** w programie Visual Studio kliknij pozycję **Kompiluj rozwiązanie** lub naciśnij klawisze **Ctrl+Shift+B**, aby potwierdzić dokładność pracy wykonanej do tej pory.

## Tworzenie poświadczeń zarządzania

W tym kroku należy zdefiniować operacje zarządzania, które będą używane do tworzenia poświadczeń sygnatury dostępu współdzielonego, za pomocą których aplikacja będzie autoryzowana.

1. Z myślą o przejrzystości, ten samouczek umieszcza wszystkie operacje kolejki w oddzielnych metodach. Utwórz metodę asynchroniczną `Queue()` w klasie `Program` po metodzie `Main()`. Na przykład:
 
    ```
    public static void Main(string[] args)
    {
    …
    }
    static async Task Queue()
    {
    }
    ```

1. Następnym krokiem jest utworzenie poświadczeń sygnatury dostępu współdzielonego, używając obiektu [TokenProvider](https://msdn.microsoft.com/library/azure/microsoft.servicebus.tokenprovider.aspx). Metoda tworzenia przyjmuje nazwę i wartość klucza sygnatury dostępu współdzielonego uzyskane w metodzie `CollectUserInput()`. Dodaj następujący kod do metody `Queue()`:

    ```
    static async Task Queue()
    {
        // Create management credentials
        TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName,sasKeyValue);
    }
    ```

2. Utwórz nowy obiekt zarządzania przestrzenią nazw, z identyfikatorem URI zawierającym jako argumenty nazwę przestrzeni nazw i poświadczenia zarządzania uzyskane w poprzednim kroku. Dodaj ten kod bezpośrednio po kodzie dodanym w poprzednim kroku. Pamiętaj, aby zastąpić ciąg `<yourNamespace>` nazwą swojej przestrzeni nazw usługi:
    
    ```
    NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

### Przykład

Na tym etapie kod powinien wyglądać podobnie do następującego:

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
  public class Program
  {
    private static DataTable issues;
    private static List<BrokeredMessage> MessageList;
    private static string ServiceNamespace;
    private static string sasKeyName = "RootManageSharedAccessKey";
    private static string sasKeyValue;

    static void Main(string[] args)
    {
      // Populate test data
      issues = ParseCSVFile();
      MessageList = GenerateMessages(issues);

      // Collect user input
      CollectUserInput();

      // Add this call
      Task.WaitAll(Queue());
    }

    static async Task Queue()
    {
      // Create management credentials
      TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
      NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    }

    static DataTable ParseCSVFile()
    {
      DataTable tableIssues = new DataTable("Issues");
      string path = @"..\..\data.csv";
      try
      {
        using (StreamReader readFile = new StreamReader(path))
        {
          string line;
          string[] row;

          // create the columns
          line = readFile.ReadLine();
          foreach (string columnTitle in line.Split(','))
          {
            tableIssues.Columns.Add(columnTitle);
          }

          while ((line = readFile.ReadLine()) != null)
          {
            row = line.Split(',');
            tableIssues.Rows.Add(row);
          }
        }
      }
      catch (Exception e)
      {
        Console.WriteLine("Error:" + e.ToString());
      }

      return tableIssues;
    }

    static List<BrokeredMessage> GenerateMessages(DataTable issues)
    {
      // Instantiate the brokered list object
      List<BrokeredMessage> result = new List<BrokeredMessage>();

      // Iterate through the table and create a brokered message for each row
      foreach (DataRow item in issues.Rows)
      {
        BrokeredMessage message = new BrokeredMessage();
        foreach (DataColumn property in issues.Columns)
        {
          message.Properties.Add(property.ColumnName, item[property]);
        }
        result.Add(message);
      }
      return result;
    }

    static void CollectUserInput()
    {
      // User service namespace
      Console.Write("Please enter the service namespace to use: ");
      ServiceNamespace = Console.ReadLine();

      // Issuer key
      Console.Write("Please enter the issuer key to use: ");
      sasKeyValue = Console.ReadLine();
    }
  }
}
```

## Wysyłanie komunikatów do kolejki

W tym kroku zostanie utworzona kolejka, a następnie komunikaty znajdujące się na liście komunikatów obsługiwanych przez brokera zostaną wysłane do tej kolejki.

### Tworzenie kolejki i wysyłanie komunikatów do kolejki

1. Najpierw utwórz kolejkę. Nazwij ją na przykład `myQueue` i zadeklaruj bezpośrednio po operacji zarządzania dodanej w metodzie `Queue()` w ostatnim kroku:

    ```
    QueueDescription myQueue;

    if (namespaceClient.QueueExists("IssueTrackingQueue"))
    {
        namespaceClient.DeleteQueue("IssueTrackingQueue");
    }

    myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
    ```

1. W metodzie `Queue()` utwórz obiekt fabryki obsługi komunikatów z nowo utworzonym identyfikatorem URI usługi Service Bus jako argumentem. Dodaj poniższy kod bezpośrednio po operacjach zarządzania dodanych w ostatnim kroku. Pamiętaj, aby zastąpić ciąg `<yourNamespace>` nazwą swojej przestrzeni nazw usługi:

    ```
    MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);
    ```

1. Następnie utwórz obiekt kolejki przy użyciu obiektu klasy [QueueClient](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.queueclient.aspx). Dodaj poniższy kod bezpośrednio po kodzie dodanym w ostatnim kroku:

    ```
    QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");
    ```

1. Następnie dodaj kod, który przetwarza w pętli utworzoną wcześniej listę komunikatów obsługiwanych przez brokera, wysyłając każdy z nich do kolejki. Dodaj poniższy kod bezpośrednio po instrukcji `CreateQueueClient()` dodanej w poprzednim kroku:
    
    ```
    // Send messages
    Console.WriteLine("Now sending messages to the queue.");
    for (int count = 0; count < 6; count++)
    {
        var issue = MessageList[count];
        issue.Label = issue.Properties["IssueTitle"].ToString();
        await myQueueClient.SendAsync(issue);
        Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
    }
    ```

## Odbieranie komunikatów z kolejki

W tym kroku zostanie odebrana lista komunikatów z kolejki utworzonej w poprzednim kroku.

### Tworzenie odbiornika i odbieranie komunikatów z kolejki

W metodzie `Queue()` iteruj po kolejce i odbieraj komunikaty przy użyciu metody [QueueClient.ReceiveAsync](https://msdn.microsoft.com/library/azure/dn130423.aspx) drukującej każdy komunikat do konsoli. Dodaj poniższy kod bezpośrednio po kodzie dodanym w poprzednim kroku:

```
Console.WriteLine("Now receiving messages from Queue.");
BrokeredMessage message;
while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
    {
        Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
        message.Complete();
    
        Console.WriteLine("Processing message (sleeping...)");
        Thread.Sleep(1000);
    }
```

Należy pamiętać, że element `Thread.Sleep` jest używany tylko na potrzeby symulacji przetwarzania komunikatów i prawdopodobnie nie będzie potrzebny w rzeczywistej aplikacji obsługi komunikatów.

### Zakończenie metody kolejki i czyszczenie zasobów

Bezpośrednio po poprzednim kodzie dodaj następujący kod, aby wyczyścić zasoby fabryki komunikatów i kolejki:

```
factory.Close();
myQueueClient.Close();
namespaceClient.DeleteQueue("IssueTrackingQueue");
```

### Wywoływanie metody kolejki

Ostatnim krokiem jest dodanie instrukcji, która wywołuje metodę `Queue()` z metody `Main()`: Dodaj następujący wyróżniony wiersz kodu na końcu metody Main():
    
```
public static void Main(string[] args)
{
    // Collect user input
    CollectUserInput();
    
    // Populate test data
    issues = ParseCSVFile();
    MessageList = GenerateMessages(issues);
    
    // Add this call
    Queue();
}
```

### Przykład

Poniższy kod zawiera kompletną aplikację **QueueSample**.

```
using System;
using System.Collections.Generic;
using System.Data;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceBus.Messaging;

namespace Microsoft.ServiceBus.Samples
{
    public class Program
    {
        private static DataTable issues;
        private static List<BrokeredMessage> MessageList;

        // Add these variables
        private static string ServiceNamespace;
        private static string sasKeyName = "RootManageSharedAccessKey";
        private static string sasKeyValue;

        static void Main(string[] args)
        {

            // Populate test data
            issues = ParseCSVFile();
            MessageList = GenerateMessages(issues);

            // Collect user input
            CollectUserInput();

            Queue();

        }

        static async Task Queue()
        {
            // Create management credentials
            TokenProvider credentials = TokenProvider.CreateSharedAccessSignatureTokenProvider(sasKeyName, sasKeyValue);
            NamespaceManager namespaceClient = new NamespaceManager(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueDescription myQueue;

            if (namespaceClient.QueueExists("IssueTrackingQueue"))
            {
                namespaceClient.DeleteQueue("IssueTrackingQueue");
            }
            
            myQueue = namespaceClient.CreateQueue("IssueTrackingQueue");
            
            MessagingFactory factory = MessagingFactory.Create(ServiceBusEnvironment.CreateServiceUri("sb", "<yourNamespace>", string.Empty), credentials);

            QueueClient myQueueClient = factory.CreateQueueClient("IssueTrackingQueue");

            // Send messages
            Console.WriteLine("Now sending messages to the queue.");
            for (int count = 0; count < 6; count++)
            {
                var issue = MessageList[count];
                issue.Label = issue.Properties["IssueTitle"].ToString();
                await myQueueClient.SendAsync(issue);
                Console.WriteLine(string.Format("Message sent: {0}, {1}", issue.Label, issue.MessageId));
            }

            Console.WriteLine("Now receiving messages from Queue.");
            BrokeredMessage message;
            while ((message = await myQueueClient.ReceiveAsync(new TimeSpan(hours: 0, minutes: 1, seconds: 5))) != null)
            {
                Console.WriteLine(string.Format("Message received: {0}, {1}, {2}", message.SequenceNumber, message.Label, message.MessageId));
                message.Complete();

                Console.WriteLine("Processing message (sleeping...)");
                Thread.Sleep(1000);
            }

            factory.Close();
            myQueueClient.Close();
            namespaceClient.DeleteQueue("IssueTrackingQueue");


        }

        static void CollectUserInput()
        {
            // User service namespace
            Console.Write("Please enter the namespace to use: ");
            ServiceNamespace = Console.ReadLine();

            // Issuer key
            Console.Write("Enter the SAS key to use: ");
            sasKeyValue = Console.ReadLine();
        }

        static List<BrokeredMessage> GenerateMessages(DataTable issues)
        {
            // Instantiate the brokered list object
            List<BrokeredMessage> result = new List<BrokeredMessage>();

            // Iterate through the table and create a brokered message for each row
            foreach (DataRow item in issues.Rows)
            {
                BrokeredMessage message = new BrokeredMessage();
                foreach (DataColumn property in issues.Columns)
                {
                    message.Properties.Add(property.ColumnName, item[property]);
                }
                result.Add(message);
            }
            return result;
        }

        static DataTable ParseCSVFile()
        {
            DataTable tableIssues = new DataTable("Issues");
            string path = @"..\..\data.csv";
            try
            {
                using (StreamReader readFile = new StreamReader(path))
                {
                    string line;
                    string[] row;

                    // create the columns
                    line = readFile.ReadLine();
                    foreach (string columnTitle in line.Split(','))
                    {
                        tableIssues.Columns.Add(columnTitle);
                    }

                    while ((line = readFile.ReadLine()) != null)
                    {
                        row = line.Split(',');
                        tableIssues.Rows.Add(row);
                    }
                }
            }
            catch (Exception e)
            {
                Console.WriteLine("Error:" + e.ToString());
            }

            return tableIssues;
        }
    }
}
```

## Kompilowanie i uruchamianie aplikacji QueueSample

Po ukończeniu powyższych kroków możesz skompilować i uruchomić aplikację **QueueSample**.

### Kompilowanie aplikacji QueueSample

W programie Visual Studio w menu **Kompilacja** kliknij pozycję **Kompiluj rozwiązanie** lub naciśnij klawisze **Ctrl+Shift+B**. Jeśli występują błędy, sprawdź, czy kod jest poprawny, korzystając z kompletnego przykładu przedstawionego na końcu poprzedniego kroku.

## Następne kroki

W tym samouczku przedstawiono sposób kompilowania aplikacji i usługi klienckiej usługi Service Bus przy użyciu możliwości komunikatów obsługiwanych przez brokera usługi Service Bus. Aby skorzystać z podobnego samouczka dotyczącego [obsługi komunikatów przez przekaźnik](service-bus-messaging-overview.md#Relayed-messaging) w usłudze Service Bus, zobacz [Samouczek dotyczący komunikatów obsługiwanych przez przekaźnik w usłudze Service Bus](service-bus-relay-tutorial.md).

Aby dowiedzieć się więcej na temat usługi [Service Bus](https://azure.microsoft.com/services/service-bus/), zobacz następujące tematy:

- [Omówienie obsługi komunikatów w usłudze Service Bus](service-bus-messaging-overview.md)
- [Podstawy usługi Service Bus](service-bus-fundamentals-hybrid-solutions.md)
- [Architektura usługi Service Bus](service-bus-architecture.md)

[klasycznego portalu Azure]: http://manage.windowsazure.com



<!--HONumber=Jun16_HO2-->


