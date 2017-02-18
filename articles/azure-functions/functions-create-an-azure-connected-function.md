---
title: "Tworzenie funkcji nawiązującej połączenie z usługami Azure | Microsoft Docs"
description: "Tworzenie nieużywającej serwera aplikacji, która nawiązuje połączenie z innymi usługami Azure, za pomocą usługi Azure Functions."
services: functions
documentationcenter: dev-center-name
author: yochay
manager: manager-alias
editor: 
tags: 
keywords: "usługa Azure Functions, funkcje, przetwarzanie zdarzeń, elementy webhook, obliczanie dynamiczne, architektura bez serwera"
ms.assetid: ab86065d-6050-46c9-a336-1bfc1fa4b5a1
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/23/2017
ms.author: rachelap@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: a8f6d111a010666bf4aaaf05e061381cc8fffed0
ms.openlocfilehash: 634781189459f26e2ffa42b25a2ffb229d3371d4


---
# <a name="use-azure-functions-to-create-a-function-that-connects-to-other-azure-services"></a>Tworzenie funkcji nawiązującej połączenie z innymi usługami Azure za pomocą usługi Azure Functions

W tym temacie opisano sposób tworzenia funkcji w usłudze Azure Functions, która nasłuchuje komunikatów w kolejce usługi Azure Storage i kopiuje je do wierszy w tabeli usługi Azure Storage. Funkcja wyzwalana przez czasomierz służy do ładowania komunikatów do kolejki. Druga funkcja odczytuje komunikaty z kolejki i zapisuje je do tabeli. Zarówno kolejka, jak i tabela są tworzone przez usługę Azure Functions na podstawie definicji powiązania. 

Aby było ciekawiej, jedna funkcja została napisana w języku JavaScript, a druga w języku C#. To pokazuje, w jaki sposób aplikacja funkcji może mieć funkcje w różnych językach. 

Ten scenariusz pokazano w [wideo w witrynie Channel 9](https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player).

## <a name="create-a-function-that-writes-to-the-queue"></a>Tworzenie funkcji, która zapisuje do kolejki

Przed połączeniem się z kolejką magazynu należy utworzyć funkcję, która ładuje kolejkę komunikatów. Ta funkcja JavaScript korzysta z wyzwalacza czasomierza, który zapisuje komunikat do kolejki co 10 sekund. Jeśli nie masz jeszcze konta platformy Azure, zapoznaj się ze środowiskiem [Wypróbuj funkcje Azure](https://functions.azure.com/try) lub [utwórz bezpłatne konto platformy Azure](https://azure.microsoft.com/free/).

1. Przejdź do witryny Azure Portal i wyszukaj aplikację funkcji.

2. Kliknij pozycje **Nowa funkcja** > **TimerTrigger-JavaScript**. 

3. Nadaj funkcji nazwę **FunctionsBindingsDemo1**, wprowadź wartość wyrażenia cron `0/10 * * * * *` w polu **Harmonogram**, a następnie kliknij przycisk **Utwórz**.
   
    ![Dodawanie funkcji wyzwalanej czasomierzem](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)

    W ten sposób została utworzona funkcja wyzwalana czasomierzem, uruchamiana co 10 sekund.

5. Na karcie **Programowanie** kliknij przycisk **Dzienniki**, aby wyświetlić aktywność w dzienniku. Zobaczysz wpisy dziennika zapisywane co 10 sekund.
   
    ![Wyświetlanie dziennika w celu sprawdzenia, czy funkcja działa](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

## <a name="add-a-message-queue-output-binding"></a>Dodawanie powiązania wyjściowego kolejki komunikatów

1. Na karcie **Integracja** wybierz elementy **Nowe dane wyjściowe** > **Azure Queue Storage** > **Wybierz**.

    ![Dodawanie funkcji czasomierza wyzwalacza](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

2. Wprowadź wartość `myQueueItem` w polu **Nazwa parametru komunikatu** i `functions-bindings` w polu **Nazwa kolejki**, wybierz istniejące **Połączenie konta magazynu** lub kliknij pozycję **nowe**, aby utworzyć nowe połączenie konta magazynu, a następnie kliknij przycisk **Zapisz**.  

    ![Tworzenie powiązania wyjściowego do kolejki magazynu](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab2.png)

1. Po powrocie na kartę **Programowanie** dołącz następujący kod do funkcji:
   
    ```javascript
   
    function myQueueItem() 
    {
        return {
            msg: "some message goes here",
            time: "time goes here"
        }
    }
   
    ```
2. Znajdź instrukcję *if* około wiersza 9 funkcji i wstaw następujący kod po tej instrukcji.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueueItem = toBeQed;
   
    ```  
   
    Ten kod powoduje utworzenie elementu **myQueueItem** i ustawienie jego właściwości **time** na bieżącą sygnaturę czasową. Następnie nowy element kolejki jest dodawany do powiązania **myQueueItem** kontekstu.

3. Kliknij pozycję **Zapisz i uruchom**.

## <a name="view-storage-updates-by-using-storage-explorer"></a>Wyświetlanie aktualizacji magazynu za pomocą programu Storage Explorer
Aby sprawdzić, czy funkcja działa, wyświetl komunikaty w utworzonej kolejce.  Z kolejką magazynu można połączyć się za pomocą programu Cloud Explorer w programie Visual Studio. Jednak portal ułatwia łączenie się z kontem magazynu za pomocą programu Microsoft Azure Storage Explorer.

1. Na karcie **Integracja** kliknij powiązanie wyjściowe kolejki i pozycję **Dokumentacja**, a następnie wyświetl parametry połączenia dla konta magazynu i skopiuj tę wartość. Ta wartość służy do łączenia się z kontem magazynu.

    ![Pobieranie programu Azure Storage Explorer](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab3.png)


2. Jeśli jeszcze tego nie zrobiono, pobierz i zainstaluj program [Microsoft Azure Storage Explorer](http://storageexplorer.com). 
 
3. W programie Storage Explorer kliknij ikonę Połącz z usługą Azure Storage, wklej parametry połączenia w polu i zakończ pracę kreatora.

    ![Program Storage Explorer dodaje połączenie](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-storage-explorer.png)

4. W obszarze **Local and attached** (Lokalne i dołączone) rozwiń pozycje **Konta magazynu** > Twoje konto magazynu > **Kolejki** > **functions-bindings** i sprawdź, czy komunikaty są zapisywane do kolejki.

    ![Wyświetlanie komunikatów w kolejce](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer.png)

    Jeśli kolejka nie istnieje lub jest pusta, prawdopodobnie występuje problem z powiązaniem lub kodem funkcji.

## <a name="create-a-function-that-reads-from-the-queue"></a>Tworzenie funkcji, która odczytuje z kolejki

Teraz, gdy komunikaty są dodawane do kolejki, możesz utworzyć inną funkcję, która odczytuje z kolejki i zapisuje komunikaty trwale do tabeli usługi Azure Storage.

1. Kliknij pozycje **Nowa funkcja** > **QueueTrigger-CSharp**. 
 
2. Nadaj funkcji nazwę `FunctionsBindingsDemo2`, wprowadź wartość **functions-bindings** w polu **Nazwa kolejki**, wybierz istniejące konto magazynu lub utwórz nowe, a następnie kliknij przycisk **Utwórz**.

    ![Dodawanie funkcji czasomierza kolejki danych wyjściowych](./media/functions-create-an-azure-connected-function/function-demo2-new-function.png) 

3. (Opcjonalnie) Możesz sprawdzić, czy nowa funkcja działa, poprzez wyświetlenie nowej kolejki w programie Storage Explorer, tak jak poprzednio. Możesz także użyć programu Cloud Explorer w programie Visual Studio.  

4. (Opcjonalnie) Odśwież kolejkę **functions-bindings** i zwróć uwagę, że elementy zostały usunięte z kolejki. Usuwanie występuje, ponieważ funkcja jest powiązana z kolejką **functions-bindings** jako wyzwalacz wejściowy i funkcja odczytuje kolejkę. 
 
## <a name="add-a-table-output-binding"></a>Dodawanie powiązania wyjściowego tabeli

1. W obszarze FunctionsBindingsDemo2 kliknij pozycje **Integracja** > **Nowe dane wyjściowe** > **Azure Table Storage** > **Wybierz**.

    ![Dodawanie powiązania do tabeli usługi Azure Storage](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png) 

2. Wprowadź wartość `TableItem` w polu **Nazwa tabeli** i `functionbindings` w polu **Nazwa parametru tabeli**, wybierz **Połączenie konta magazynu** lub utwórz nowe, a następnie kliknij przycisk **Zapisz**.

    ![Konfigurowanie powiązania tabeli magazynu](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab2.png)
   
3. Na karcie **Programowanie** zastąp istniejący kod funkcji następującym:
   
    ```cs
    
    using System;
    
    public static void Run(QItem myQueueItem, ICollector<TableItem> myTable, TraceWriter log)
    {    
        TableItem myItem = new TableItem
        {
            PartitionKey = "key",
            RowKey = Guid.NewGuid().ToString(),
            Time = DateTime.Now.ToString("hh.mm.ss.ffffff"),
            Msg = myQueueItem.Msg,
            OriginalTime = myQueueItem.Time    
        };
        
        // Add the item to the table binding collection.
        myTable.Add(myItem);
    
        log.Verbose($"C# Queue trigger function processed: {myItem.RowKey} | {myItem.Msg} | {myItem.Time}");
    }
    
    public class TableItem
    {
        public string PartitionKey {get; set;}
        public string RowKey {get; set;}
        public string Time {get; set;}
        public string Msg {get; set;}
        public string OriginalTime {get; set;}
    }
    
    public class QItem
    {
        public string Msg { get; set;}
        public string Time { get; set;}
    }
    ```
    Klasa **TableItem** reprezentuje wiersz w tabeli magazynu i element jest dodawany do kolekcji `myTable` obiektów **TableItem**. Należy ustawić właściwości **PartitionKey** i **RowKey**, aby umożliwić wstawienie do tabeli.

4. Kliknij pozycję **Zapisz**.  Na koniec można sprawdzić działanie funkcji, wyświetlając tabelę w programie Storage Explorer lub programie Cloud Explorer programu Visual Studio.

5. (Opcjonalnie) W swoim koncie magazynu w programie Storage Explorer rozwiń pozycje **Tabele** > **functionsbindings** i sprawdź, czy wiersze są dodawane do tabeli. To samo można zrobić w programie Cloud Explorer programu Visual Studio.

    ![Widok wierszy w tabeli](./media/functions-create-an-azure-connected-function/functionsbindings-azure-storage-explorer2.png)

    Jeśli tabela nie istnieje lub jest pusta, prawdopodobnie występuje problem z powiązaniem lub kodem funkcji. 
 
[!INCLUDE [More binding information](../../includes/functions-bindings-next-steps.md)]

## <a name="next-steps"></a>Następne kroki
Poniższe tematy umożliwiają uzyskanie dodatkowych informacji na temat usługi Azure Functions.

* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
  Dokumentacja dla programistów dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.
* [Testowanie usługi Azure Functions](functions-test-a-function.md)  
  Opis różnych narzędzi i technik testowania funkcji.
* [Jak skalować usługę Azure Functions](functions-scale.md)  
  Omówienie planów usług dostępnych w środowisku Azure Functions, w tym planu hostingowego zużycia, oraz sposobu wybierania właściwego planu. 

[!INCLUDE [Getting help note](../../includes/functions-get-help.md)]




<!--HONumber=Feb17_HO1-->


