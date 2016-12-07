---
title: "Tworzenie funkcji platformy Azure powiązanej z usługą platformy Azure | Microsoft Docs"
description: "Tworzenie funkcji platformy Azure — aplikacji niekorzystającej z serwera — która współdziała z innymi usługami platformy Azure."
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
ms.date: 10/25/2016
ms.author: rachelap@microsoft.com
translationtype: Human Translation
ms.sourcegitcommit: a0a46708645be91f89b0a6ae9059468bc84aeb11
ms.openlocfilehash: c6905452951910d3c62bc5152741a8ead26196ef


---
# <a name="create-an-azure-function-which-binds-to-an-azure-service"></a>Tworzenie funkcji platformy Azure powiązanej z usługą platformy Azure
[!INCLUDE [Getting Started Note](../../includes/functions-getting-started.md)]

Ten krótki film wideo zawiera informacje na temat tworzenia funkcji platformy Azure, która nasłuchuje komunikatów za pomocą kolejki platformy Azure i kopiuje je do obiektu Blob platformy Azure.

## <a name="watch-the-video"></a>Obejrzyj film
>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-an-Azure-Function-which-binds-to-an-Azure-service/player]
>
>

## <a name="create-an-input-queue-trigger-function"></a>Tworzenie funkcji wyzwalacza danych wejściowych w kolejce
Celem tej funkcji jest pisanie komunikatu do kolejki co 10 sekund. W tym celu należy utworzyć kolejki komunikatów i funkcji, a następnie dodać kod, aby zapisywać komunikaty w nowo tworzonych kolejkach.

1. Przejdź do witryny Azure Portal i wyszukaj aplikację funkcji platformy Azure.
2. Kliknij pozycję **Nowa funkcja** > **TimerTrigger — Node**. Nadaj funkcji nazwę **FunctionsBindingsDemo1**.
3. Wprowadź wartość „0/10 * * * * *” zgodnie z harmonogramem. Ta wartość jest podawana w postaci wyrażenia cron. Spowoduje to zaplanowanie uruchamiania czasomierza co 10 sekund.
4. Kliknij przycisk **Utwórz**, aby utworzyć funkcję.
   
    ![Dodawanie funkcji czasomierza wyzwalacza](./media/functions-create-an-azure-connected-function/new-trigger-timer-function.png)
5. Sprawdź, czy funkcja działa, wyświetlając działanie w dzienniku. Być może trzeba będzie kliknąć link **Dzienniki** w prawym górnym rogu, aby wyświetlić okienko dziennika.
   
   ![Sprawdzanie, czy funkcja działa przez wyświetlenie działania w dzienniku](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-view-log.png)

### <a name="add-a-message-queue"></a>Dodawanie kolejki komunikatów
1. Przejdź na kartę **Integracja**.
2. Wybierz kolejno pozycje **Nowe dane wyjściowe** > **Kolejka usługi Azure Storage** > **Wybierz**.
3. Wprowadź wartość **myQueueItem** w polu tekstowym **Nazwa parametru komunikatu**.
4. Wybierz konto magazynu lub kliknij pozycję **Nowe** w celu utworzenia konta magazynu, jeśli jeszcze go nie masz.
5. Wprowadź wartość **functions-bindings** w polu tekstowym **Nazwa kolejki**.
6. Kliknij pozycję **Zapisz**.  
   
   ![Dodawanie funkcji czasomierza wyzwalacza](./media/functions-create-an-azure-connected-function/functionsbindingsdemo1-integrate-tab.png)

### <a name="write-to-the-message-queue"></a>Zapisywanie w kolejce komunikatów
1. Wróć na kartę **Opracowywanie**, a następnie dodaj następujący kod do funkcji po istniejącym kodzie:
   
    ```javascript
   
    function myQueueItem() 
      {
        return {
        msg: "some message goes here",
        time: "time goes here"
      }
    }
   
    ```
2. Modyfikowanie istniejącego kodu funkcji w celu wywołania kodu dodanego w kroku 1 Wstaw następujący kod około wiersza 9 funkcji po instrukcji *if*.
   
    ```javascript
   
    var toBeQed = myQueueItem();
    toBeQed.time = timeStamp;
    context.bindings.myQueue = toBeQed;
   
    ```
   
    Ten kod powoduje utworzenie elementu **myQueueItem** i ustawienie jego właściwości **time** na bieżącą sygnaturę czasową. Następnie nowy element jest dodawany do powiązania myQueue kontekstu.
3. Kliknij pozycję **Zapisz i uruchom**.
4. Sprawdź, czy kod działa, wyświetlając kolejkę w programie Visual Studio.
   
   * Otwórz program Visual Studio, a następnie przejdź do pozycji **Widok** > **Cloud** **Explorer**.
   * Znajdź konto magazynu i kolejkę **functions-bindings** używaną podczas tworzenia kolejki myQueue. Powinny być widoczne wiersze danych dziennika. Być może konieczne będzie zalogowanie do platformy Azure za pośrednictwem programu Visual Studio.  

## <a name="create-an-output-queue-trigger-function"></a>Tworzenie funkcji wyzwalacza danych wyjściowych w kolejce
1. Kliknij kolejno pozycje **Nowa funkcja** > **QueueTrigger — C#**. Nadaj funkcji nazwę **FunctionsBindingsDemo2**. Zwróć uwagę, że w aplikacji funkcji można łączyć różne języki (w tym przypadku Node i C#).
2. Wprowadź wartość **functions-bindings** w polu **Nazwa kolejki**.
3. Wybierz konto magazynu do użycia lub utwórz nowe.
4. Kliknij przycisk **Utwórz**
5. Sprawdź, czy nowa funkcja działa, wyświetlając dziennik funkcji i program Visual Studio w celu wyszukania aktualizacji. Dziennik funkcji pokazuje, że funkcja działa i elementy są usuwane z kolejki. Ponieważ funkcja jest powiązana z kolejką danych wyjściowych **functions-bindings** jako wyzwalacz danych wejściowych, odświeżenie kolejki **functions-bindings** w programie Visual Studio powinno ujawnić, czy elementy zostały usunięte. Elementy zostały usunięte z kolejki.   
   
   ![Dodawanie funkcji czasomierza kolejki danych wyjściowych](./media/functions-create-an-azure-connected-function/functionsbindingsdemo2-integrate-tab.png)   

### <a name="modify-the-queue-item-type-from-json-to-object"></a>Modyfikowanie typu elementu kolejki z formatu JSON na obiekt
1. Zastąp kod funkcji **FunctionsBindingsDemo2** następującym kodem:    
   
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
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.Msg} | {myQueueItem.Time}");
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
   
    Ten kod powoduje dodanie dwóch klas, **TableItem** i **QItem**, które służą do odczytu i zapisu w kolejkach. Dodatkowo funkcja **Run** została zmodyfikowana, aby akceptować parametry **QItem** i **TraceWriter** parametru, zamiast elementów **string** i **TraceWriter**. 
2. Kliknij przycisk **Zapisz**.
3. Sprawdź, czy kod działa, sprawdzając dziennik. Zauważ, że funkcje platformy Azure automatycznie przeprowadzają serializację i deserializację obiektu, aby ułatwić dostęp do kolejki w sposób skoncentrowany na obiekcie w celu przejścia wokół danych. 

## <a name="store-messages-in-an-azure-table"></a>Przechowywanie komunikatów w tabeli platformy Azure
Gdy kolejki już działają razem, należy dodać tabelę platformy Azure w celu stałego przechowywania danych kolejki.

1. Przejdź na kartę **Integracja**.
2. Utwórz tabelę usługi Azure Storage dla danych wyjściowych i nadaj jej nazwę **myTable**.
3. Udziel odpowiedzi **functionsbindings** na pytanie o tabele, w których powinny być zapisywane dane.
4. Zmień ustawienie **PartitionKey** z **{project-id}** na **{partition}**.
5. Wybierz konto magazynu lub utwórz nowe.
6. Kliknij pozycję **Zapisz**.
7. Przejdź do karty **Opracowywanie**.
8. Utwórz klasę **TableItem**, która będzie reprezentować tabelę platformy Azure, i zmodyfikuj funkcję Run, aby akceptowała nowo utworzony obiekt TableItem. Pamiętaj, że aby funkcja działała, należy użyć właściwości **PartitionKey** i **RowKey**.
   
    ```cs
   
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
   
      log.Verbose($"C# Queue trigger function processed: {myQueueItem.RowKey} | {myQueueItem.Msg} | {myQueueItem.Time}");
    }
   
    public class TableItem
    {
      public string PartitionKey {get; set;}
      public string RowKey {get; set;}
      public string Time {get; set;}
      public string Msg {get; set;}
      public string OriginalTime {get; set;}
    }
    ```
9. Kliknij pozycję **Zapisz**.
10. Sprawdź, czy kod działa, wyświetlając dzienniki funkcji i w programie Visual Studio. Aby sprawdzić w programie Visual Studio, przy użyciu programu **Cloud Explorer** przejdź do tabeli platformy Azure **functionbindings** i sprawdź, czy zawiera ona wiersze.

[!INCLUDE [Getting Started Note](../../includes/functions-bindings-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->


