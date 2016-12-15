---
title: "Tworzenie funkcji przetwarzania zdarzeń | Microsoft Docs"
description: "Korzystanie z usługi Azure Functions w celu utworzenia funkcji języka C#, która jest uruchamiana na podstawie czasomierza zdarzeniowego."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 84bd0373-65e2-4022-bcca-2b9cd9e696f5
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 44e397c7521ba8f0ba11893c364f51177561bee4
ms.openlocfilehash: df3d303ee10fcc982552ea9756eb59198c87b650


---
# <a name="create-an-event-processing-azure-function"></a>Tworzenie funkcji przetwarzania zdarzeń platformy Azure
Usługa Azure Functions to oparte na zdarzeniach środowisko umożliwiające przeprowadzanie obliczeń na żądanie, które pozwala tworzyć zaplanowane lub wyzwalane jednostki kodu implementowane w różnych językach programowania. Aby dowiedzieć się więcej o usłudze Azure Functions, zobacz [Azure Functions — omówienie](functions-overview.md).

W tym temacie opisano tworzenie nowej funkcji w języku C#, która służy do dodawania komunikatów do kolejki magazynu i jest uruchamiana na podstawie czasomierza zdarzeniowego. 

## <a name="prerequisites"></a>Wymagania wstępne
Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. Jeśli nie masz jeszcze konta platformy Azure, zapoznaj się ze środowiskiem [Wypróbuj funkcje](https://functions.azure.com/try) lub [utwórz bezpłatne konto platformy Azure](https://azure.microsoft.com/free/). 

## <a name="create-a-timer-triggered-function-from-the-template"></a>Tworzenie funkcji wyzwalanej przez czasomierz przy użyciu szablonu
Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. Aby utworzyć funkcję, musisz mieć aktywne konto platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, [dostępne są konta bezpłatne](https://azure.microsoft.com/free/). 

1. Przejdź do [portalu Azure Functions](https://functions.azure.com/signin) i zaloguj się przy użyciu konta platformy Azure.
2. Jeśli masz aplikację funkcji, której możesz użyć, wybierz ją w obszarze **Twoje aplikacje funkcji** i kliknij pozycję **Otwórz**. Aby utworzyć nową aplikację funkcji, wpisz unikatową **nazwę** nowej aplikacji funkcji lub zaakceptuj wygenerowaną nazwę, wybierz preferowany **region**, a następnie kliknij pozycję **Utwórz i rozpocznij**. 
3. W aplikacji funkcji kliknij kolejno pozycje **+ Nowa funkcja** > **TimerTrigger — C#** > **Utwórz**. Zostanie utworzona funkcja o domyślnej nazwie uruchamiana raz na minutę, zgodnie z domyślnym harmonogramem. 
   
    ![Tworzenie nowej funkcji wyzwalanej przez czasomierz](./media/functions-create-an-event-processing-function/functions-create-new-timer-trigger.png)
4. W nowej funkcji kliknij kartę **Integracja**, a następnie kliknij kolejno pozycje **Nowe dane wyjściowe** > **Kolejka usługi Azure Storage** > **Wybierz**.
   
    ![Tworzenie nowej funkcji wyzwalanej przez czasomierz](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding.png)
5. W obszarze **Dane wyjściowe usługi Azure Storage Queue** wybierz istniejące **Połączenie konta magazynu** lub utwórz nowe, a następnie kliknij przycisk **Zapisz**. 
   
    ![Tworzenie nowej funkcji wyzwalanej przez czasomierz](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding-2.png)
6. Wróć do karty **Tworzenie** i w oknie **Kod** zastąp istniejący skrypt języka C# następującym kodem:
    ```cs   
    using System;

    public static void Run(TimerInfo myTimer, out string outputQueueItem, TraceWriter log)
    {
        // Add a new scheduled message to the queue.
        outputQueueItem = $"Ping message added to the queue at: {DateTime.Now}.";

        // Also write the message to the logs.
        log.Info(outputQueueItem);
    }
    ```
   
    Kod ten powoduje dodanie do kolejki nowego komunikatu z bieżącą datą i godziną, gdy funkcja zostanie uruchomiona.
7. Kliknij przycisk **Zapisz**. W oknach **Dzienniki**poszukaj informacji o następnym uruchomieniu funkcji.
8. (Opcjonalnie) Przejdź do konta magazynu i sprawdź, czy do kolejki są dodawane komunikaty.
9. Wróć do karty **Integracja** i zmień wartość w polu harmonogramu na `0 0 * * * *`. Funkcja jest teraz uruchamiana co godzinę. 

Jest to bardzo uproszczony przykład powiązania danych wyjściowych wyzwalacza czasomierza i kolejki magazynu. Więcej informacji można znaleźć w tematach [Azure Functions timer trigger](functions-bindings-timer.md) (Wyzwalacz czasomierza usługi Azure Functions) i [Azure Functions triggers and bindings for Azure Storage](functions-bindings-storage.md) (Wyzwalacze i powiązania usługi Azure Functions dla usługi Azure Storage).

## <a name="next-steps"></a>Następne kroki
Poniższe tematy umożliwiają uzyskanie dodatkowych informacji na temat usługi Azure Functions.

* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
  Dokumentacja dla programistów dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.
* [Testowanie usługi Azure Functions](functions-test-a-function.md)  
  Opis różnych narzędzi i technik testowania funkcji.
* [Jak skalować usługę Azure Functions](functions-scale.md)  
  Omówienie planów usług dostępnych w środowisku Azure Functions, w tym planu hostingowego zużycia, oraz sposobu wybierania właściwego planu.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Dec16_HO1-->


