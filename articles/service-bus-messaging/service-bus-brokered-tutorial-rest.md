---
title: Samouczek dotyczący komunikatów obsługiwanych przez brokera w usłudze Service Bus dla usługi REST | Microsoft Docs
description: Samouczek dotyczący komunikatów obsługiwanych przez brokera dla usługi REST.
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/27/2016
ms.author: sethm

---
# <a name="service-bus-brokered-messaging-rest-tutorial"></a>Samouczek dotyczący komunikatów obsługiwanych przez brokera w usłudze Service Bus dla usługi REST
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

Ten samouczek pokazuje sposób tworzenia podstawowej kolejki i tematu/subskrypcji usługi Azure Service Bus opartej na usłudze REST.

## <a name="create-a-namespace"></a>Tworzenie przestrzeni nazw
Pierwszym krokiem jest utworzenie przestrzeni nazw usługi i uzyskanie klucza [sygnatury dostępu współdzielonego](../service-bus/service-bus-sas-overview.md). Przestrzeń nazw wyznacza granice każdej aplikacji uwidacznianej za pośrednictwem usługi Service Bus. Klucz sygnatury dostępu współdzielonego jest automatycznie generowany przez system po utworzeniu przestrzeni nazw usługi. Kombinacja przestrzeni nazw i klucza sygnatury dostępu współdzielonego usługi dostarcza poświadczenia dla usługi Service Bus w celu uwierzytelnienia dostępu do aplikacji.

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="create-a-console-client"></a>Tworzenie klienta konsoli
Kolejki usługi Service Bus umożliwiają przechowywanie komunikatów w kolejce FIFO (pierwszy na wejściu, pierwszy na wyjściu). Tematy i subskrypcje implementują wzorzec publikowania/subskrybowania. Najpierw należy utworzyć temat, a następnie jedną lub większą liczbę subskrypcji skojarzonych z tym tematem. Komunikaty wysłane do tematu są natychmiast wysyłane do subskrybentów tego tematu.

Kod w tym samouczku wykonuje następujące działania.

* Używa podanej przestrzeni nazw oraz klucza [sygnatury dostępu współdzielonego](../service-bus/service-bus-sas-overview.md), aby uzyskać dostęp do zasobów przestrzeni nazw usługi Service Bus.
* Tworzy kolejkę, wysyła komunikat do kolejki i odczytuje komunikat z kolejki.
* Tworzy temat i subskrypcję tego tematu oraz wysyła i odczytuje komunikat z subskrypcji.
* Pobiera z usługi Service Bus wszystkie informacje o kolejce, temacie i subskrypcji, w tym reguły subskrypcji.
* Usuwa zasoby kolejki, tematu i subskrypcji.

Usługa jest usługą sieci Web typu REST, a więc nie korzysta z żadnych specjalnych typów, ponieważ cała wymiana obejmuje ciągi. Oznacza to, że projekt programu Visual Studio nie może odwoływać się do żadnych bibliotek usługi Service Bus.

Po utworzeniu obszaru nazw i uzyskaniu poświadczeń w pierwszym kroku możesz utworzyć aplikację konsolową programu Visual Studio.

### <a name="create-a-console-application"></a>Tworzenie aplikacji konsolowej
1. Uruchom program Visual Studio jako administrator, klikając prawym przyciskiem myszy ikonę programu w menu **Start**, a następnie klikając polecenie **Uruchom jako administrator**.
2. Utwórz nowy projekt aplikacji konsoli. Kliknij menu **Plik** i kliknij pozycję **Nowy**, a następnie kliknij pozycję **Projekt**. W oknie dialogowym **Nowy projekt** kliknij pozycję **Visual C#** (jeśli pozycja **Visual C#** nie jest wyświetlana, sprawdź w obszarze **Inne języki**), wybierz szablon **Aplikacja konsolowa** i nadaj mu nazwę **Microsoft.ServiceBus.Samples**. Użyj domyślnej lokalizacji. Kliknij przycisk **OK**, aby utworzyć projekt.
3. W pliku Program.cs upewnij się, że instrukcje `using` wyglądają następująco:
   
    ```
    using System;
    using System.Globalization;
    using System.IO;
    using System.Net;
    using System.Security.Cryptography;
    using System.Text;
    using System.Xml;
    ```
4. W razie potrzeby zmień nazwę przestrzeni nazw programu z domyślnej programu Visual Studio na `Microsoft.ServiceBus.Samples`.
5. Wewnątrz klasy `Program` dodaj następujące zmienne globalne:
   
    ```
    static string serviceNamespace;
    static string baseAddress;
    static string token;
    const string sbHostName = "servicebus.windows.net";
    ```
6. Wewnątrz metody `Main()` wklej następujący kod:
   
    ```
    Console.Write("Enter your service namespace: ");
    serviceNamespace = Console.ReadLine();
   
    Console.Write("Enter your SAS key: ");
    string SASKey = Console.ReadLine();
   
    baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
    try
    {
        token = GetSASToken("RootManageSharedAccessKey", SASKey);
   
        string queueName = "Queue" + Guid.NewGuid().ToString();
   
        // Create and put a message in the queue
        CreateQueue(queueName, token);
        SendMessage(queueName, "msg1");
        string msg = ReceiveAndDeleteMessage(queueName);
   
        string topicName = "Topic" + Guid.NewGuid().ToString();
        string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
        CreateTopic(topicName);
        CreateSubscription(topicName, subscriptionName);
        SendMessage(topicName, "msg2");
   
        Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));
   
        // Get an Atom feed with all the queues in the namespace
        Console.WriteLine(GetResources("$Resources/Queues"));
   
        // Get an Atom feed with all the topics in the namespace
        Console.WriteLine(GetResources("$Resources/Topics"));
   
        // Get an Atom feed with all the subscriptions for the topic we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions"));
   
        // Get an Atom feed with all the rules for the topic and subscription we just created
        Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));
   
        // Delete the queue we created
        DeleteResource(queueName);
   
        // Delete the topic we created
        DeleteResource(topicName);
   
        // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Topics"));
   
        // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
        Console.WriteLine(GetResources("$Resources/Queues"));
    }
    catch (WebException we)
    {
        using (HttpWebResponse response = we.Response as HttpWebResponse)
        {
            if (response != null)
            {
                Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
            }
            else
            {
                Console.WriteLine(we.ToString());
            }
        }
    }
   
    Console.WriteLine("\nPress ENTER to exit.");
    Console.ReadLine();
    ```

## <a name="create-management-credentials"></a>Tworzenie poświadczeń zarządzania
Następnym krokiem jest napisanie metody, która przetwarza przestrzeń nazw i klucz sygnatury dostępu współdzielonego wprowadzone w poprzednim kroku i zwraca token sygnatury dostępu współdzielonego. W tym przykładzie jest tworzony token sygnatury dostępu współdzielonego, który jest ważny przez jedną godzinę.

### <a name="create-a-getsastoken()-method"></a>Tworzenie metody GetSASToken()
Wklej następujący kod po metodzie `Main()` w klasie `Program`:

```
private static string GetSASToken(string SASKeyName, string SASKeyValue)
{
  TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
  string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
  string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
  HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

  string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
  string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
      WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
  return sasToken;
}
```
## <a name="create-the-queue"></a>Tworzenie kolejki
Następnym krokiem jest napisanie metody, która używa polecenia HTTP PUT typu REST do utworzenia kolejki.

Wklej poniższy kod bezpośrednio po kodzie metody `GetSASToken()` dodanym w poprzednim kroku:

```
// Uses HTTP PUT to create the queue
private static string CreateQueue(string queueName, string token)
{
    // Create the URI of the new queue, note that this uses the HTTPS scheme
    string queueAddress = baseAddress + queueName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating queue {0}", queueAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                          <title type=""text"">" + queueName + @"</title>
                          <content type=""application/xml"">
                            <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                          </content>
                        </entry>";

    byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="send-a-message-to-the-queue"></a>Wysyłanie komunikatu do kolejki
W tym kroku dodawana jest metoda, która używa polecenia HTTP POST typu REST do wysyłania komunikatu do kolejki utworzonej w poprzednim kroku.

1. Wklej poniższy kod bezpośrednio po kodzie metody `CreateQueue()` dodanym w poprzednim kroku:
   
    ```
    // Sends a message to the "queueName" queue, given the name and the value to enqueue
    // Uses an HTTP POST request.
    private static void SendMessage(string queueName, string body)
    {
        string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
        Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
        WebClient webClient = new WebClient();
        webClient.Headers[HttpRequestHeader.Authorization] = token;
   
        webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));
    }
    ```
2. Standardowe właściwości komunikatu obsługiwanego przez brokera są umieszczane w nagłówku HTTP `BrokerProperties`. Właściwości brokera muszą być zserializowane w formacie JSON. Aby określić wartość **TimeToLive** wynoszącą 30 sekund i dodać do komunikatu etykietę „M1”, dodaj następujący kod bezpośrednio przed wywołaniem metody `webClient.UploadData()` pokazanym w poprzednim przykładzie:
   
    ```
    // Add brokered message properties "TimeToLive" and "Label"
    webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
    ```
   
    Zauważ, że właściwości komunikatu obsługiwanego przez brokera zostały i będą dodane. Dlatego żądanie wysłania musi określać wersję interfejsu API, która obsługuje wszystkie właściwości komunikatu obsługiwanego przez brokera będące częścią żądania. Jeśli określona wersja interfejsu API nie obsługuje którejś z właściwości komunikatu obsługiwanego przez brokera, ta właściwość jest ignorowana.
3. Właściwości niestandardowe komunikatu są definiowane jako zestaw par kluczy i wartości. Każda właściwość niestandardowa jest przechowywana we własnym nagłówku TPPT. Aby dodać właściwości niestandardowe „Priority” i „Customer”, dodaj poniższy kod bezpośrednio przed wywołaniem metody `webClient.UploadData()` pokazanym w poprzednim przykładzie:
   
    ```
    // Add custom properties "Priority" and "Customer".
    webClient.Headers.Add("Priority", "High");
    webClient.Headers.Add("Customer", "12345");
    ```

## <a name="receive-and-delete-a-message-from-the-queue"></a>Odbieranie i usuwanie komunikatu z kolejki
Następnym krokiem jest dodanie metody, która używa polecenia HTTP DELETE typu REST do pobierania i usuwania komunikatu z kolejki.

Wklej poniższy kod bezpośrednio po kodzie metody `SendMessage()` dodanym w poprzednim kroku:

```
// Receives and deletes the next message from the given resource (queue, topic, or subscription)
// using the resourceName and an HTTP DELETE request
private static string ReceiveAndDeleteMessage(string resourceName)
{
    string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
    Console.WriteLine("\nRetrieving message from {0}", fullAddress);
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
    string responseStr = Encoding.UTF8.GetString(response);

    Console.WriteLine(responseStr);
    return responseStr;
}
```

## <a name="create-a-topic-and-subscription"></a>Tworzenie tematu i subskrypcji
Następnym krokiem jest napisanie metody, która używa polecenia HTTP PUT typu REST do utworzenia tematu. Następnie napiszesz metodę, która tworzy subskrypcję tego tematu.

### <a name="create-a-topic"></a>Tworzenie tematu
Wklej poniższy kod bezpośrednio po kodzie metody `ReceiveAndDeleteMessage()` dodanym w poprzednim kroku:

```
// Using an HTTP PUT request.
private static string CreateTopic(string topicName)
{
    var topicAddress = baseAddress + topicName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating topic {0}", topicAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

### <a name="create-a-subscription"></a>Tworzenie subskrypcji
Poniższy kod tworzy subskrypcję tematu utworzonego w poprzednim kroku. Dodaj następujący kod bezpośrednio po definicji `CreateTopic()`:

```
private static string CreateSubscription(string topicName, string subscriptionName)
{
    var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
    // Prepare the body of the create queue request
    var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

    byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
    return Encoding.UTF8.GetString(response);
}
```

## <a name="retrieve-message-resources"></a>Pobieranie zasobów komunikatu
W tym kroku należy dodać kod, który pobiera właściwości komunikatu, a następnie usuwa zasoby obsługi komunikatów utworzone w poprzednich krokach.

### <a name="retrieve-an-atom-feed-with-the-specified-resources"></a>Pobieranie źródła danych Atom z określonymi zasobami
Dodaj poniższy kod bezpośrednio po metodzie `CreateSubscription()` dodanej w poprzednim kroku:

```
private static string GetResources(string resourceAddress)
{
    string fullAddress = baseAddress + resourceAddress;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;
    Console.WriteLine("\nGetting resources from {0}", fullAddress);
    return FormatXml(webClient.DownloadString(fullAddress));
}
```

### <a name="delete-messaging-entities"></a>Usuwanie jednostek obsługi komunikatów
Dodaj poniższy kod bezpośrednio po kodzie dodanym w poprzednim kroku:

```
private static string DeleteResource(string resourceName)
{
    string fullAddress = baseAddress + resourceName;
    WebClient webClient = new WebClient();
    webClient.Headers[HttpRequestHeader.Authorization] = token;

    Console.WriteLine("\nDeleting resource at {0}", fullAddress);
    byte[] response = webClient.UploadData(fullAddress, "DELETE", newbyte[0]);
    return Encoding.UTF8.GetString(response);
}
```

### <a name="format-the-atom-feed"></a>Formatowanie źródła danych Atom
Metoda `GetResources()` zawiera wywołanie metody `FormatXml()`, która ponownie formatuje pobrane źródło danych Atom, aby było bardziej czytelne. Poniżej znajduje się definicja metody `FormatXml()`. Dodaj ten kod bezpośrednio po kodzie `DeleteResource()` dodanym w poprzednim kroku:

```
// Formats the XML string to be more human-readable; intended for display purposes
private static string FormatXml(string inputXml)
{
    XmlDocument document = new XmlDocument();
    document.Load(new StringReader(inputXml));

    StringBuilder builder = new StringBuilder();
    using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
    {
        writer.Formatting = Formatting.Indented;
        document.Save(writer);
    }

    return builder.ToString();
}
```

## <a name="build-and-run-the-application"></a>Kompilowanie i uruchamianie aplikacji
Teraz możesz skompilować i uruchomić aplikację. W menu **Kompilacja** w programie Visual Studio kliknij pozycję **Kompiluj rozwiązanie** lub naciśnij klawisze **Ctrl+Shift+B**.

### <a name="run-the-application"></a>Uruchamianie aplikacji
Jeśli nie ma żadnych błędów, naciśnij klawisz F5, aby uruchomić aplikację. Po wyświetleniu monitu wprowadź przestrzeń nazw, nazwę klucza sygnatury dostępu współdzielonego i wartość klucza sygnatury dostępu współdzielonego, które zostały uzyskane w pierwszym kroku.

### <a name="example"></a>Przykład
Poniższy przykład przedstawia kompletny kod, tak jak powinien wyglądać po wykonaniu wszystkich czynności w tym samouczku.

```
using System;
using System.Globalization;
using System.IO;
using System.Net;
using System.Security.Cryptography;
using System.Text;
using System.Xml;

namespace Microsoft.ServiceBus.Samples
{
    class Program
    {
        static string serviceNamespace;
        static string baseAddress;
        static string token;
        const string sbHostName = "servicebus.windows.net";

        static void Main(string[] args)
        {
            Console.Write("Enter your service namespace: ");
            serviceNamespace = Console.ReadLine();

            Console.Write("Enter your SAS key: ");
            string SASKey = Console.ReadLine();

            baseAddress = "https://" + serviceNamespace + "." + sbHostName + "/";
            try
            {
                token = GetSASToken("RootManageSharedAccessKey", SASKey);

                string queueName = "Queue" + Guid.NewGuid().ToString();

                // Create and put a message in the queue
                CreateQueue(queueName, token);
                SendMessage(queueName, "msg1");
                string msg = ReceiveAndDeleteMessage(queueName);

                string topicName = "Topic" + Guid.NewGuid().ToString();
                string subscriptionName = "Subscription" + Guid.NewGuid().ToString();
                CreateTopic(topicName);
                CreateSubscription(topicName, subscriptionName);
                SendMessage(topicName, "msg2");

                Console.WriteLine(ReceiveAndDeleteMessage(topicName + "/Subscriptions/" + subscriptionName));

                // Get an Atom feed with all the queues in the namespace
                Console.WriteLine(GetResources("$Resources/Queues"));

                // Get an Atom feed with all the topics in the namespace
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the subscriptions for the topic we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions"));

                // Get an Atom feed with all the rules for the topic and subscription we just created
                Console.WriteLine(GetResources(topicName + "/Subscriptions/" + subscriptionName + "/Rules"));

                // Delete the queue we created
                DeleteResource(queueName);

                // Delete the topic we created
                DeleteResource(topicName);

                // Get an Atom feed with all the topics in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Topics"));

                // Get an Atom feed with all the queues in the namespace, it shouldn't have the one we created now
                Console.WriteLine(GetResources("$Resources/Queues"));
            }
            catch (WebException we)
            {
                using (HttpWebResponse response = we.Response as HttpWebResponse)
                {
                    if (response != null)
                    {
                        Console.WriteLine(new StreamReader(response.GetResponseStream()).ReadToEnd());
                    }
                    else
                    {
                        Console.WriteLine(we.ToString());
                    }
                }
            }

            Console.WriteLine("\nPress ENTER to exit.");
            Console.ReadLine();
        }

        private static string GetSASToken(string SASKeyName, string SASKeyValue)
        {
            TimeSpan fromEpochStart = DateTime.UtcNow - new DateTime(1970, 1, 1);
            string expiry = Convert.ToString((int)fromEpochStart.TotalSeconds + 3600);
            string stringToSign = WebUtility.UrlEncode(baseAddress) + "\n" + expiry;
            HMACSHA256 hmac = new HMACSHA256(Encoding.UTF8.GetBytes(SASKeyValue));

            string signature = Convert.ToBase64String(hmac.ComputeHash(Encoding.UTF8.GetBytes(stringToSign)));
            string sasToken = String.Format(CultureInfo.InvariantCulture, "SharedAccessSignature sr={0}&sig={1}&se={2}&skn={3}",
                WebUtility.UrlEncode(baseAddress), WebUtility.UrlEncode(signature), expiry, SASKeyName);
            return sasToken;
        }

        // Uses HTTP PUT to create the queue
        private static string CreateQueue(string queueName, string token)
        {
            // Create the URI of the new queue, note that this uses the HTTPS scheme
            string queueAddress = baseAddress + queueName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating queue {0}", queueAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + queueName + @"</title>
                                  <content type=""application/xml"">
                                    <QueueDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(queueAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        // Sends a message to the "queueName" queue, given the name and the value to enqueue
        // Uses an HTTP POST request.
        private static void SendMessage(string queueName, string body)
        {
            string fullAddress = baseAddress + queueName + "/messages" + "?timeout=60&api-version=2013-08 ";
            Console.WriteLine("\nSending message {0} - to address {1}", body, fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            // Add brokered message properties “TimeToLive” and “Label”.
            webClient.Headers.Add("BrokerProperties", "{ \"TimeToLive\":30, \"Label\":\"M1\"}");
            // Add custom properties “Priority” and “Customer”.
            webClient.Headers.Add("Priority", "High");
            webClient.Headers.Add("Customer", "12345");
            webClient.UploadData(fullAddress, "POST", Encoding.UTF8.GetBytes(body));

        }

        // Receives and deletes the next message from the given resource (queue, topic, or subscription)
        // using the resourceName and an HTTP DELETE request.
        private static string ReceiveAndDeleteMessage(string resourceName)
        {
            string fullAddress = baseAddress + resourceName + "/messages/head" + "?timeout=60";
            Console.WriteLine("\nRetrieving message from {0}", fullAddress);
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            string responseStr = Encoding.UTF8.GetString(response);

            Console.WriteLine(responseStr);
            return responseStr;
        }

        // Using an HTTP PUT request.
        private static string CreateTopic(string topicName)
        {
            var topicAddress = baseAddress + topicName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating topic {0}", topicAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + topicName + @"</title>
                                  <content type=""application/xml"">
                                    <TopicDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(topicAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string CreateSubscription(string topicName, string subscriptionName)
        {
            var subscriptionAddress = baseAddress + topicName + "/Subscriptions/" + subscriptionName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nCreating subscription {0}", subscriptionAddress);
            // Prepare the body of the create queue request
            var putData = @"<entry xmlns=""http://www.w3.org/2005/Atom"">
                                  <title type=""text"">" + subscriptionName + @"</title>
                                  <content type=""application/xml"">
                                    <SubscriptionDescription xmlns:i=""http://www.w3.org/2001/XMLSchema-instance"" xmlns=""http://schemas.microsoft.com/netservices/2010/10/servicebus/connect"" />
                                  </content>
                                </entry>";

            byte[] response = webClient.UploadData(subscriptionAddress, "PUT", Encoding.UTF8.GetBytes(putData));
            return Encoding.UTF8.GetString(response);
        }

        private static string GetResources(string resourceAddress)
        {
            string fullAddress = baseAddress + resourceAddress;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;
            Console.WriteLine("\nGetting resources from {0}", fullAddress);
            return FormatXml(webClient.DownloadString(fullAddress));
        }

        private static string DeleteResource(string resourceName)
        {
            string fullAddress = baseAddress + resourceName;
            WebClient webClient = new WebClient();
            webClient.Headers[HttpRequestHeader.Authorization] = token;

            Console.WriteLine("\nDeleting resource at {0}", fullAddress);
            byte[] response = webClient.UploadData(fullAddress, "DELETE", new byte[0]);
            return Encoding.UTF8.GetString(response);
        }

        // Formats the XML string to be more human-readable; intended for display purposes
        private static string FormatXml(string inputXml)
        {
            XmlDocument document = new XmlDocument();
            document.Load(new StringReader(inputXml));

            StringBuilder builder = new StringBuilder();
            using (XmlTextWriter writer = new XmlTextWriter(new StringWriter(builder)))
            {
                writer.Formatting = Formatting.Indented;
                document.Save(writer);
            }

            return builder.ToString();
        }
    }
}
```

## <a name="next-steps"></a>Następne kroki
Zobacz następujące artykuły, aby dowiedzieć się więcej:

* [Omówienie obsługi komunikatów w usłudze Service Bus](service-bus-messaging-overview.md)
* [Podstawy usługi Azure Service Bus](../service-bus/service-bus-fundamentals-hybrid-solutions.md)
* [Samouczek dotyczący przekaźnika usługi Service Bus dla usługi REST](../service-bus-relay/service-bus-relay-rest-tutorial.md)

<!--HONumber=Oct16_HO3-->


