---
title: Tworzenie funkcji na platformie Azure wyzwalanej przez komunikaty kolejki | Microsoft Docs
description: "Utwórz za pomocą usługi Azure Functions funkcję niewymagającą użycia serwera wywoływaną za pomocą komunikatów przesyłanych do kolejki usługi Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 0b609bc0-c264-4092-8e3e-0784dcc23b5d
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: d1ddfbe9a0a0c7c7e0a060776938bd68a87e1ba5
ms.contentlocale: pl-pl
ms.lasthandoff: 06/26/2017

---
# <a name="add-messages-to-an-azure-storage-queue-using-functions"></a>Dodawanie komunikatów do kolejki usługi Azure Storage przy użyciu funkcji

W usłudze Azure Functions powiązania danych wejściowych i wyjściowych zapewniają deklaratywną metodę łączenia z danymi usług zewnętrznych z funkcji. W tym temacie dowiesz się, jak zaktualizować istniejącą funkcję, dodając powiązanie danych wyjściowych, które wysyła komunikaty do kolejki usługi magazynu Azure.  

![Wyświetlanie komunikatu w dziennikach.](./media/functions-integrate-storage-queue-output-binding/functions-integrate-storage-binding-in-portal.png)

## <a name="prerequisites"></a>Wymagania wstępne 

[!INCLUDE [Previous topics](../../includes/functions-quickstart-previous-topics.md)]

* Zainstaluj program [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)] 

## <a name="add-binding"></a>Dodawanie powiązania danych wyjściowych
 
1. Rozwiń aplikację funkcji i funkcję.

2. Wybierz pozycje **Integracja** i **+ Nowe dane wyjściowe**, a następnie wybierz pozycję **Azure Queue Storage** i kliknij przycisk **Wybierz**.
    
    ![Dodaj powiązanie danych wyjściowych kolejki magazynu do funkcji w witrynie Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding.png)

3. Użyj ustawień określonych w tabeli i wybierz przycisk **Zapisz**: 

    ![Dodaj powiązanie danych wyjściowych kolejki magazynu do funkcji w witrynie Azure Portal.](./media/functions-integrate-storage-queue-output-binding/function-add-queue-storage-output-binding-2.png)

    | Ustawienie      |  Sugerowana wartość   | Opis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nazwa kolejki**   | myqueue-items    | Nazwa kolejki, z którą zostanie nawiązane połączenie na koncie magazynu. |
    | **Połączenie konta magazynu** | AzureWebJobStorage | Możesz skorzystać z połączenia konta magazynu już używanego przez aplikację funkcji lub utworzyć nowe.  |
    | **Nazwa parametru komunikatu** | outQueueItem | Nazwa parametru powiązania danych wyjściowych. | 

Po zdefiniowaniu powiązania danych wyjściowych musisz zaktualizować kod, tak aby stosować powiązanie do dodawania komunikatów do kolejki.  

## <a name="update-the-function-code"></a>Aktualizacja kodu funkcji

1. Wybierz funkcję,której kod ma zostać wyświetlony w edytorze. 

2. W przypadku funkcji C# zaktualizuj definicję funkcji w następujący sposób, aby dodać parametr powiązania magazynu **outQueueItem**. Pomiń ten krok w przypadku funkcji JavaScript.

    ```cs   
    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, 
        ICollector<string> outQueueItem, TraceWriter log)
    {
        ....
    }
    ```

3. Dodaj następujący kod do funkcji tuż przed powrotem z metody. Użyj fragmentu kodu odpowiedniego dla języka danej funkcji.

    ```javascript
    context.bindings.outQueueItem = "Name passed to the function: " + 
                (req.query.name || req.body.name);
    ```

    ```cs
    outQueueItem.Add("Name passed to the function: " + name);     
    ```

4. Wybierz przycisk **Zapisz**, aby zapisać zmiany.

Wartość przekazana do wyzwalacza HTTP jest uwzględniona w komunikacie dodanym do kolejki.
 
## <a name="test-the-function"></a>Testowanie funkcji 

1. Po zapisaniu zmian w kodzie wybierz przycisk **Uruchom**. 

    ![Dodaj powiązanie danych wyjściowych kolejki magazynu do funkcji w witrynie Azure Portal.](./media/functions-integrate-storage-queue-output-binding/functions-test-run-function.png)

2. Sprawdź dzienniki, aby upewnić się, że funkcja zakończyła się pomyślnie. Środowisko uruchomieniowe funkcji utworzy nową kolejkę o nazwie **outqueue** w koncie magazynu przy pierwszym użyciu powiązania danych wyjściowych.

Następnie możesz nawiązać połączenie z kontem magazynu, aby sprawdzić nową kolejkę i dodany do niej komunikat. 

## <a name="connect-to-the-queue"></a>Połączenie z kolejką

Pomiń pierwsze trzy kroki, jeśli już zainstalowano program Storage Explorer i połączono go z kontem magazynu.    

1. W funkcji wybierz przycisk **Integracja** i nowe powiązanie danych wyjściowych usługi **Azure Queue Storage**, a następnie rozwiń pozycję **Dokumentacja**. Skopiuj wartości **Nazwa konta** i **Klucz konta**. Te poświadczenia służą do nawiązywania połączenia z kontem magazynu.
 
    ![Uzyskiwanie poświadczeń połączenia konta magazynu.](./media/functions-integrate-storage-queue-output-binding/function-get-storage-account-credentials.png)

2. Uruchom narzędzie [Microsoft Azure Storage Explorer](http://storageexplorer.com/), wybierz ikonę połączenia po lewej stronie, wybierz pozycję **Użyj klucza i nazwy konta magazynu** i wybierz przycisk **Dalej**.

    ![Uruchamianie narzędzia Storage Account Explorer.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-1.png)
    
3. Wklej pozycje **Nazwa konta** i **Klucz konta** z kroku 1 w odpowiednie pola, a następnie wybierz przyciski **Dalej** i **Połącz**. 
  
    ![Wklej poświadczenia magazynu i nawiąż połączenie.](./media/functions-integrate-storage-queue-output-binding/functions-storage-manager-connect-2.png)

4. Rozwiń powiązane konto magazynu, kliknij prawym przyciskiem myszy pozycję **Kolejki** i sprawdź, czy istnieje kolejka o nazwie **myqueue-items**. Komunikat powinien być już także widoczny w kolejce.  
 
    ![Tworzenie kolejki magazynu.](./media/functions-integrate-storage-queue-output-binding/function-queue-storage-output-view-queue.png)
 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Następne kroki

Dodano powiązanie danych wyjściowych do istniejącej funkcji. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Aby uzyskać więcej informacji na temat tworzenia powiązań z kolejką magazynu, zobacz [Powiązania kolejki magazynu w usłudze Azure Functions](functions-bindings-storage-queue.md). 




