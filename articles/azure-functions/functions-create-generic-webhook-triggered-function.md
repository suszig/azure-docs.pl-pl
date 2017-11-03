---
title: "Tworzenie funkcji platformy Azure wyzwalane przez ogólny element webhook | Dokumentacja firmy Microsoft"
description: "Użyj usługi Azure Functions, aby utworzyć pliki funkcji, który jest wywoływany przez element webhook na platformie Azure."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: f283f8d79c5ae5fb6a72c84c9e9edb7bb8de4a83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Utwórz funkcję wyzwalane przez ogólny element webhook

Usługa Azure Functions umożliwia wykonywanie kodu w środowisku bezserwerowym bez konieczności uprzedniego tworzenia maszyny wirtualnej lub publikowania aplikacji sieci Web. Na przykład można skonfigurować funkcję, która ma być wyzwalany przez alertu zgłoszonego przez Azure Monitor. W tym temacie przedstawiono sposób wykonania kodu C#, gdy grupa zasobów jest dodawane do subskrypcji.   

![Ogólny element webhook wyzwalane funkcji w portalu Azure](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Wymagania wstępne 

W celu ukończenia tego samouczka:

+ Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Tworzenie aplikacji funkcji platformy Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Następnie należy utworzyć funkcję w nowej aplikacji funkcji.

## <a name="create-function"></a>Utwórz funkcję ogólny element webhook wyzwalane

1. Rozwiń aplikację funkcji i kliknij przycisk **+** obok pozycji **Funkcje**. Jeśli ta funkcja jest pierwsza z nich w aplikacji funkcji, wybierz **Niestandardowa funkcja**. Spowoduje to wyświetlenie pełnego zestawu szablonów funkcji.

    ![Strona szybkiego rozpoczynania pracy z usługą Functions w witrynie Azure Portal](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Wybierz **ogólny element WebHook - C#** szablonu. Wpisz nazwę dla funkcji języka C#, a następnie wybierz **Utwórz**.

     ![Utwórz funkcję ogólny element webhook wyzwalane w portalu Azure](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. W nowych funkcji, kliknij przycisk **adres URL funkcji <> / Get**, następnie skopiuj i Zapisz wartość. Ta wartość służy do konfigurowania elementu webhook. 

    ![Sprawdzanie kodu funkcji](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
Następnie należy utworzyć punkt końcowy elementu webhook w alertu dziennika aktywności w monitorze Azure. 

## <a name="create-an-activity-log-alert"></a>Utwórz alert dziennika aktywności

1. W portalu Azure, przejdź do **Monitor** usługi, wybierz opcję **alerty**i kliknij przycisk **alert dziennika aktywności Dodaj**.   

    ![Monitorowanie](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Użyj ustawień określonych w tabeli:

    ![Utwórz alert dziennika aktywności](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Ustawienie      |  Sugerowana wartość   | Opis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nazwa alertu dziennika aktywności** | zasobów grupy Tworzenie alertu | Nazwa alertu dziennika aktywności. |
    | **Subskrypcja** | Twoja subskrypcja | Subskrypcja używanego na potrzeby tego samouczka. | 
    |  **Grupa zasobów** | myResourceGroup | Grupy zasobów, które są wdrażane zasoby alertu. Przy użyciu tej samej grupie zasobów co aplikacja funkcja ułatwia oczyszczanie po ukończeniu samouczka. |
    | **Kategoria zdarzenia** | Administracyjne | Ta kategoria zawiera zmiany wprowadzone do zasobów platformy Azure.  |
    | **Typ zasobu** | Grupy zasobów | Filtruje alerty do działań grupy zasobów. |
    | **Grupa zasobów**<br/>i **zasobów** | Wszystkie | Monitoruj wszystkie zasoby. |
    | **Nazwa operacji** | Tworzenie grupy zasobów | Filtry alertów do tworzenia operacji. |
    | **Poziom** | Informacyjny | Obejmują alerty informacyjne poziomu. | 
    | **Stan** | Powodzenie | Filtruje alerty do akcji, które zostały ukończone pomyślnie. |
    | **Grupy akcji** | Nowy | Utwórz nową grupę akcji, który definiuje przyjmuje akcji, gdy zostanie zgłoszony alert. |
    | **Nazwa grupy akcji** | Element webhook — funkcja | Nazwę identyfikującą grupy akcji.  | 
    | **Krótka nazwa** | funcwebhook | Krótka nazwa grupy działań. |  

3. W **akcje**, Dodaj akcję przy użyciu ustawień określonych w tabeli: 

    ![Dodaj grupę](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Ustawienie      |  Sugerowana wartość   | Opis                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nazwa** | CallFunctionWebhook | Nazwa akcji. |
    | **Typ akcji** | Webhook | Odpowiedzi na alert jest, że adres URL elementu Webhook jest wywoływana. |
    | **Szczegóły** | Adres URL funkcji | Wklej adres URL elementu webhook funkcji, które wcześniej zostały skopiowane. |v

4. Kliknij przycisk **OK** Aby utworzyć grupę alertów i akcji.  

Element webhook nosi teraz, po utworzeniu grupy zasobów w ramach subskrypcji. Następnie zaktualizuj kod w funkcji do obsługi danych dziennika JSON w treści żądania.   

## <a name="update-the-function-code"></a>Aktualizacja kodu funkcji

1. Przejdź z powrotem do aplikacji funkcji w portalu, a następnie rozwiń funkcji. 

2. Zastąp kod skryptu C# w funkcji w portalu następujący kod:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get the activityLog object from the JSON in the message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if the resource in the activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about the created resource group to the streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

Teraz można przetestować funkcji przez utworzenie nowej grupy zasobów w ramach subskrypcji.

## <a name="test-the-function"></a>Testowanie funkcji

1. Kliknij ikonę grupy zasobów w lewej części portalu Azure, wybierz opcję **+ Dodaj**, wpisz **Nazwa grupy zasobów**i wybierz **Utwórz** można utworzyć pustej grupy zasobów.
    
    ![Utwórz grupę zasobów.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. Wróć do funkcji, a następnie rozwiń węzeł **dzienniki** okna. Po utworzeniu grupy zasobów, alert dziennika aktywności wyzwala elementu webhook i wykonuje funkcji. Możesz wyświetlić nazwę nowej grupy zasobów zapisywane w dziennikach.  

    ![Dodaj ustawienie aplikacji testu.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (Opcjonalnie) Przejdź wstecz i usunąć utworzoną grupę zasobów. Należy pamiętać, że to działanie nie powoduje wyzwolenia funkcji. Jest to spowodowane Usuń operacje są odfiltrowywane przez alert. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Następne kroki

Funkcja, która jest uruchamiana, gdy żądanie zostanie odebrane z ogólny element webhook został utworzony. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Aby uzyskać więcej informacji na temat wyzwalaczy elementów webhook, zobacz temat [Powiązania protokołu HTTP i elementów webhook w usłudze Azure Functions](functions-bindings-http-webhook.md). Aby dowiedzieć się więcej o wdrażaniu funkcji w języku C#, zobacz [dokumentacja dla deweloperów usług Azure funkcje C# skrypt](functions-reference-csharp.md).

