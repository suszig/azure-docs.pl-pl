---
title: "Uruchom usługi Azure Functions z zadania usługi analiza strumienia Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak konfigurowanie funkcji Azure ujściem danych wyjściowych do strumienia analityczne zadań."
keywords: "dane wyjściowe, strumieniowego przesyłania danych i funkcji platformy Azure"
documentationcenter: 
services: stream-analytics
author: SnehaGunda
manager: kfile
ms.assetid: 
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 12/19/2017
ms.author: sngun
ms.openlocfilehash: adc147fc9f47e78cc0a2fcaf53f064bcfa5eee2c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="run-azure-functions-with-azure-stream-analytics-jobs"></a>Uruchom usługi Azure Functions z zadania usługi analiza strumienia Azure 
 
> [!IMPORTANT]
> Ta funkcja jest dostępna w wersji zapoznawczej.

Azure Functions można uruchomić z usługą Azure Stream Analytics przez skonfigurowanie usługi Azure Functions wśród sink danych wyjściowych do zadania usługi analiza strumienia. Funkcja Azure jest zdarzeniami, środowisko obliczeń na żądanie, które umożliwia implementowania kodu wyzwalane przez zdarzenia występujące w Azure lub usług innych firm. Możliwość odpowiadanie na wyzwalaczy funkcji Azure ułatwia fizycznych danych wyjściowych do zadania usługi analiza strumienia Azure.

Usługa Azure Stream Analytics wywołuje funkcję Azure za pomocą wyzwalaczy HTTP. Adapter wyjścia funkcji platformy Azure umożliwia użytkownikom nawiązywanie funkcji Azure Stream Analytics tak, aby zdarzenia mogą być wyzwalane opartych na kwerendach Stream Analytics. 

W tym samouczku przedstawiono sposób połączenia usługi analiza strumienia Azure do [pamięć podręczna Redis Azure](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md) przy użyciu [usługi Azure Functions](../azure-functions/functions-overview.md). 

## <a name="configure-stream-analytics-job-to-run-an-azure-function"></a>Skonfiguruj zadania usługi analiza strumienia do uruchomienia funkcji platformy Azure 

W tej sekcji przedstawiono sposób konfigurowania funkcji platformy Azure, który zapisuje dane w pamięci podręcznej Redis Azure uruchomienie zadania usługi analiza strumienia. Zadanie Stream Analytics odczytuje zdarzenia z Centrum zdarzeń, wykonuje zapytania, który wywołuje funkcję platformy Azure. Ta funkcja Azure odczytuje dane z zadania usługi analiza strumienia i zapisuje go w pamięci podręcznej Redis Azure.

![W samouczku ilustrowanie grafiki](./media/stream-analytics-with-azure-functions/image1.png)

Poniższe kroki są wymagane do wykonania tego zadania:
* [Utwórz zadanie usługi Stream Analytics z Centrum zdarzeń jako dane wejściowe.](#create-stream-analytics-job-with-event-hub-as-input)  
* [Tworzenie pamięci podręcznej Azure Redis.](#create-an-azure-redis-cache)  
* [Tworzenie funkcji platformy Azure, który może zapisać danych w pamięci podręcznej Redis.](#create-an-azure-function-that-can-write-data-to-the-redis-cache)    
* [Zaktualizuj analityczne strumienia zadania z funkcji platformy Azure jako dane wyjściowe.](#update-the-stream-analytic-job-with-azure-function-as-output)  
* [Sprawdź, czy pamięć podręczna Redis wyników.](#check-redis-cache-for-results)  

## <a name="create-stream-analytics-job-with-event-hub-as-input"></a>Utwórz zadanie usługi Stream Analytics z Centrum zdarzeń jako dane wejściowe

Postępuj zgodnie z [wykrywanie oszustw w czasie rzeczywistym](stream-analytics-real-time-fraud-detection.md) samouczek tworzenia Centrum zdarzeń, uruchom aplikację generator zdarzenia i utworzyć Azure Stream Analytics (przejść przez kolejne kroki tworzenia zapytania i dane wyjściowe, należy zamiast tego można skonfigurować Środowisko Azure Functions dane wyjściowe w następnej sekcji.)

## <a name="create-an-azure-redis-cache"></a>Tworzenie usługi Azure Redis Cache

1. Tworzenie pamięci podręcznej Redis Azure za pomocą procedury opisanej w [tworzenia pamięci podręcznej](../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache) sekcji tego artykułu pamięci podręcznej Redis.  

2. Po utworzeniu pamięci podręcznej Redis, przejdź do utworzonego pamięci podręcznej > **klucze dostępu** > i zanotuj **parametry połączenia podstawowej**.

   ![Parametry połączenia w pamięci podręcznej redis](./media/stream-analytics-with-azure-functions/image2.png)

## <a name="create-an-azure-function-that-can-write-data-to-the-redis-cache"></a>Tworzenie funkcji platformy Azure, który może zapisać danych w pamięci podręcznej Redis

1. Użyj [tworzenia aplikacji funkcji](../azure-functions/functions-create-first-azure-function.md#create-a-function-app) części dokumentacji usługi Azure Functions do tworzenia aplikacji funkcji platformy Azure i [funkcji wyzwalanej przez HTTP Azure](../azure-functions/functions-create-first-azure-function.md#create-function) (alias elementu Webhook) przy użyciu **CSharp** języka.  

2. Przejdź do **run.csx** funkcji i zaktualizować go następującym kodem (Upewnij się zastąpić "\<ciąg połączenia pamięci podręcznej redis tu\>" tekstu w pamięci podręcznej Redis parametry połączenia podstawowej Czy można pobrać w poprzedniej sekcji):  

   ```c#
   using System;
   using System.Net;
   using System.Threading.Tasks;
   using StackExchange.Redis;
   using Newtonsoft.Json;
   using System.Configuration;

   public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
   {
      log.Info($"C# HTTP trigger function processed a request. RequestUri={req.RequestUri}");
    
      // Get the request body
      dynamic dataArray = await req.Content.ReadAsAsync<object>();

      // Throw an HTTP Request Entity Too Large exception when the incoming batch(dataArray) is greater than 256 KB. Make sure that the size value is consistent with the value entered in the Stream Analytics portal.

      if (dataArray.ToString().Length > 262144)
      {        
         return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
      var connection = ConnectionMultiplexer.Connect("<your redis cache connection string goes here>");
      log.Info($"Connection string.. {connection}");
    
      // Connection refers to a property that returns a ConnectionMultiplexer
      IDatabase db = connection.GetDatabase();
      log.Info($"Created database {db}");
    
      log.Info($"Message Count {dataArray.Count}");

      // Perform cache operations using the cache object. For example, the following code block adds few integral data types to the cache
      for (var i = 0; i < dataArray.Count; i++)
      {
        string time = dataArray[i].time;
        string callingnum1 = dataArray[i].callingnum1;
        string key = time + " - " + callingnum1;
        db.StringSet(key, dataArray[i].ToString());
        log.Info($"Object put in database. Key is {key} and value is {dataArray[i].ToString()}");
       
      // Simple get of data types from the cache
      string value = db.StringGet(key);
      log.Info($"Database got: {value}");
      }

      return req.CreateResponse(HttpStatusCode.OK, "Got");
}    

   ```

   Gdy analiza strumienia Azure odbiera 413 wyjątek (Http żądania jednostki zbyt duża liczba godzin) z funkcji platformy Azure, zmniejsza rozmiar partii, który wysyła do usługi Azure Functions. W funkcji platformy Azure należy użyć poniższego kodu, aby sprawdzić, czy usługi Azure Stream Analytics nie wysyła partie zbyt duży. Upewnij się, że maksymalna liczba i rozmiar wartości partii używany w funkcji są zgodne z wartości wprowadzone w portalu usługi analiza strumienia.

   ```c#
   if (dataArray.ToString().Length > 262144)
      {        
        return new HttpResponseMessage(HttpStatusCode.RequestEntityTooLarge);
      }
   ```

3. W edytorze tekstów wybranych przez użytkownika, należy utworzyć plik JSON o nazwie **project.json** następującym kodem, a następnie zapisz go na komputerze lokalnym. Ten plik zawiera zależności pakietów NuGet, które są wymagane przez funkcję c#.  
   
   ```json
       {
         "frameworks": {
             "net46": {
                 "dependencies": {
                     "StackExchange.Redis":"1.1.603",
                     "Newtonsoft.Json": "9.0.1"
                 }
             }
         }
     }

   ```
 
4. Wróć do portalu Azure > Przejdź do funkcji Azure > z **funkcji platformy** kartę > kliknij **Edytor usług aplikacji** znajdującej się w obszarze **narzędzi programistycznych**. 
 
   ![Ekran edytora usługi aplikacji](./media/stream-analytics-with-azure-functions/image3.png)

5. W edytorze usługi aplikacji, kliknij prawym przyciskiem myszy w katalogu głównym i przekazać **project.json** pliku. Po pomyślnym przekazywania Odśwież stronę, powinien zostać wyświetlony plik wygenerowany automatycznie o nazwie **plikiem project.lock.json**.  Automatycznie wygenerowany plik zawiera odwołania do bibliotek DLL, które są określone w pliku Project.json.  

   ![Przekaż plik project.json](./media/stream-analytics-with-azure-functions/image4.png)

 

## <a name="update-the-stream-analytic-job-with-azure-function-as-output"></a>Aktualizacja zadania strumienia danych analitycznych z funkcji platformy Azure jako dane wyjściowe

1. Otwórz zadania usługi analiza strumienia Azure w portalu Azure.  

2. Przejdź do funkcji Azure > **omówienie** > **dane wyjściowe** > **Dodaj** nowe dane wyjściowe > Wybierz **Azure—funkcja** opcji ujścia. Nowa karta danych wyjściowych funkcji platformy Azure są dostępne z następującymi właściwościami:  

   |**Nazwa właściwości**|**Opis**|
   |---|---|
   |Alias wyjściowy| Przyjazna dla użytkownika nazwa używana w zapytaniu zadanie odwołania do danych wyjściowych. |
   |Opcja importowania| Można użyć funkcji platformy azure z bieżącej subskrypcji lub skonfiguruj ustawienia ręcznie, jeśli funkcja znajduje się w innej subskrypcji. |
   |Aplikacja funkcji| Nazwa aplikacji funkcji platformy Azure. |
   |Funkcja| Nazwa funkcji w funkcji aplikacji (nazwa funkcji run.csx).|
   |Rozmiar partii maksymalna|Ta właściwość jest używana do ustawienia dla każdej partii danych wyjściowych, które są przesyłane do funkcji Azure maksymalny rozmiar. Domyślnie ta wartość jest równa 256 KB.|
   |Maksymalna liczba wsadów|Tej właściwości można określić maksymalną liczbę zdarzeń w każdej z partii, wysyłany do funkcji platformy Azure. Wartość domyślna partii maksymalna liczba to 100. Ta właściwość jest opcjonalna.|
   |Klucz|Ta właściwość umożliwia przy użyciu funkcji platformy Azure z innej subskrypcji. Podaj wartość klucza dostęp do funkcji. Ta właściwość jest opcjonalna.|

3. Podaj nazwę aliasu danych wyjściowych. W tym samouczku będziemy nazwa **saop1** (możesz użyć innej nazwy wybór) i Wypełnij inne szczegóły.  

4. Otwórz zadania usługi analiza strumienia i zaktualizuj zapytanie dla następujących (Upewnij się, że Zamień tekst "saop1", jeśli inne nazwy ujście danych wyjściowych):  

   ```sql
    SELECT 
            System.Timestamp as Time, CS1.CallingIMSI, CS1.CallingNum as CallingNum1, 
            CS2.CallingNum as CallingNum2, CS1.SwitchNum as Switch1, CS2.SwitchNum as Switch2
        INTO saop1
        FROM CallStream CS1 TIMESTAMP BY CallRecTime
           JOIN CallStream CS2 TIMESTAMP BY CallRecTime
            ON CS1.CallingIMSI = CS2.CallingIMSI AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
        WHERE CS1.SwitchNum != CS2.SwitchNum
   ```

5. Uruchom aplikację telcodatagen.exe, uruchamiając następujące polecenie w wierszu polecenia (polecenie przyjmuje format - `telcodatagen.exe [#NumCDRsPerHour] [SIM Card Fraud Probability] [#DurationHours]`)  
   
   **telcodatagen.exe 1000.2 2**
    
6.  Uruchom zadania usługi analiza strumienia.

## <a name="check-redis-cache-for-results"></a>Sprawdź, czy pamięć podręczna Redis wyników

1. Przejdź do portalu Azure i Znajdź pamięć podręczną Redis > Wybierz **konsoli**.  

2. Użyj [poleceń pamięci podręcznej Redis](https://redis.io/commands) Aby sprawdzić, czy dane w pamięci podręcznej Redis. (Polecenie przyjmuje Get format {klucz}). Na przykład:

   **Pobierz "12/19/2017 21:32:24-123414732"**

   To polecenie należy wydrukować wartości dla określonego klucza:

   ![Dane wyjściowe pamięci podręcznej redis](./media/stream-analytics-with-azure-functions/image5.png)

## <a name="known-issues"></a>Znane problemy

* W portalu Azure, podczas próby Resetuj rozmiar partii Max-wartość maksymalna, liczba partii do empty(default), zmienia się do wcześniej wprowadzona wartość podczas zapisywania. Celowo w takim przypadku wprowadź wartości domyślne dla tych pól.

## <a name="next-steps"></a>Kolejne kroki
