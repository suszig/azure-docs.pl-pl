---
title: "Utwórz definicję OpenAPI dla funkcji | Dokumentacja firmy Microsoft"
description: "Tworzenie definicji OpenAPI, która umożliwia inne aplikacje i usługi do wywołania funkcji na platformie Azure."
services: functions
keywords: "Aplikacje w chmurze OpenAPI, Swagger, i usługi w chmurze"
documentationcenter: 
author: mgblythe
manager: cfowler
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/15/2017
ms.author: mblythe; glenga
ms.custom: mvc
ms.openlocfilehash: 2bf1a3e80e96d76b15340f87166b2b4762271cf3
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="create-an-openapi-definition-for-a-function"></a>Utwórz definicję OpenAPI dla funkcji
Przy użyciu definicji OpenAPI często opisano interfejsów API REST (wcześniej znane jako [Swagger](http://swagger.io/) pliku). Ta definicja zawiera informacje, jakie operacje są dostępne w interfejsie API i struktury danych żądań i odpowiedzi dla interfejsu API.

W tym samouczku utworzysz funkcję, która określa, czy awaryjnego naprawiania na turbiny knie jest ekonomiczne. Następnie można utworzyć definicji OpenAPI dla aplikacji funkcja tak, aby funkcja może zostać wywołana z innych aplikacji i usług.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie funkcji na platformie Azure
> * Generowanie definicji OpenAPI przy użyciu narzędzia OpenAPI
> * Należy zmodyfikować definicję do udostępnienia dodatkowych metadanych
> * Testowanie definicji przez wywołanie funkcji

## <a name="create-a-function-app"></a>Tworzenie aplikacji funkcji

Do obsługi wykonywania funkcji potrzebna jest aplikacja funkcji. Funkcja umożliwia aplikacji grupy funkcje jako Jednostka logiki aby ułatwić zarządzanie, wdrażania, skalowanie i udostępnianie zasobów. 

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]


## <a name="create-the-function"></a>Tworzenie funkcji

W tym samouczku korzysta z funkcji wyzwalane HTTP, który przyjmuje dwa parametry: szacowany czas na dokonanie turbiny napraw (w godzinach); i pojemność turbiny (w kilowatach). Następnie oblicza ilość naprawy koszt, oraz przychodów turbiny można utworzyć w okresie 24 godzin.

1. Rozwiń aplikacji funkcji i wybierz  **+**  znajdujący się obok **funkcji**. Jeśli jest to pierwsza funkcja w aplikacji funkcji, wybierz pozycję **Funkcja niestandardowa**. Spowoduje to wyświetlenie pełnego zestawu szablonów funkcji. 

    ![Strona szybkiego rozpoczynania pracy z usługą Functions w witrynie Azure Portal](media/functions-openapi-definition/add-first-function.png)

2. W polu wyszukiwania wpisz `http` , a następnie wybierz **C#** szablonu wyzwalacza HTTP. 
 
    ![Wybierz wyzwalacza HTTP](./media/functions-openapi-definition/select-http-trigger-portal.png)

3. Typ `TurbineRepair` dla funkcji **nazwa**, wybierz `Function` dla  **[poziom uwierzytelniania](functions-bindings-http-webhook.md#http-auth)**, a następnie wybierz **Utwórz**.  

    ![Tworzenie funkcji wyzwalane HTTP](./media/functions-openapi-definition/select-http-trigger-portal-2.png)

1. Zastąp zawartość pliku run.csx następującym kodem, a następnie kliknij przycisk **zapisać**:

    ```c#
    using System.Net;

    const double revenuePerkW = 0.12; 
    const double technicianCost = 250; 
    const double turbineCost = 100;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {   

        //Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();
        int hours = data.hours;
        int capacity = data.capacity;

        //Formulas to calculate revenue and cost
        double revenueOpportunity = capacity * revenuePerkW * 24;  
        double costToFix = (hours * technicianCost) +  turbineCost;
        string repairTurbine;

        if (revenueOpportunity > costToFix){
            repairTurbine = "Yes";
        }
        else {
            repairTurbine = "No";
        }

        return req.CreateResponse(HttpStatusCode.OK, new{
            message = repairTurbine,
            revenueOpportunity = "$"+ revenueOpportunity,
            costToFix = "$"+ costToFix         
        }); 
    }
    ```
    Ten kod funkcja zwraca komunikat z `Yes` lub `No` wskazująca, czy awaryjnego naprawiania jest ekonomiczne, a także możliwość przychodu, który reprezentuje turbiny oraz koszt ustalenie turbiny. 

1. Aby przetestować funkcji, kliknij przycisk **testu** na prawym końcu do zwiększenia karcie testów. Wprowadź następującą wartość dla **treść żądania**, a następnie kliknij przycisk **Uruchom**.

    ```json
    {
    "hours": "6",
    "capacity": "2500"
    }
    ```

    ![Testowanie funkcji w portalu Azure](media/functions-openapi-definition/test-function.png)

    Następująca wartość jest zwracana w treści odpowiedzi.

    ```json
    {"message":"Yes","revenueOpportunity":"$7200","costToFix":"$1600"}
    ```

Zostanie zainstalowana funkcja, która określa efektywności kosztowej awaryjnego naprawy. Następnie generowanie i zmodyfikować definicję OpenAPI dla funkcji aplikacji.

## <a name="generate-the-openapi-definition"></a>Generowanie definicji OpenAPI

Teraz możesz przystąpić do generowania definicji OpenAPI. Ta definicja mogą być używane przez innych technologii firmy Microsoft, takich jak aplikacje interfejsu API [rozwiązania PowerApps](functions-powerapps-scenario.md) i [Microsoft Flow](../azure-functions/app-service-export-api-to-powerapps-and-flow.md), jak również jako trzeci narzędzi dla deweloperów firmy, takich jak [Postman](https://www.getpostman.com/docs/importing_swagger) i [wiele pakietów więcej](http://swagger.io/tools/).

1. Wybierz tylko *zleceń* czy Twój interfejs API obsługuje (w tym WPISIE przypadków). Dzięki temu można czyszczący wygenerowanego definicji interfejsu API.

    1. Na **integracji** nową funkcję wyzwalacza HTTP, Zmień na karcie **metod HTTP dozwolone** do **wybrane metody**

    1. W **metod HTTP wybrane**, usuń zaznaczenie opcji każdego z wyjątkiem **POST**, następnie kliknij przycisk **zapisać**.

        ![Wybrane metody HTTP](media/functions-openapi-definition/selected-http-methods.png)
        
1. Kliknij nazwę funkcji aplikacji (takich jak **funkcja Pokaz energii**) > **funkcji platformy** > **definicji interfejsu API**.

    ![Definicja interfejsu API](media/functions-openapi-definition/api-definition.png)

1. Na **definicji interfejsu API** , kliknij pozycję **funkcja**.

    ![Źródło definicji interfejsu API](media/functions-openapi-definition/api-definition-source.png)

    Ten krok powoduje włączenie zestaw opcji OpenAPI dla funkcji aplikacji, w tym punkt końcowy do obsługi plików OpenAPI z domeny aplikacji funkcji, wbudowanego kopię [Edytor OpenAPI](http://editor.swagger.io)i generatora szablonu definicji interfejsu API.

1. Kliknij przycisk **API generowania definicji szablonu** > **zapisać**.

    ![Generuj szablon definicji interfejsu API](media/functions-openapi-definition/generate-template.png)

    Azure skanuje aplikacji funkcji dla funkcji wyzwalacza HTTP i używa informacje w functions.json do generowania definicji OpenAPI. Oto definicji, która jest generowana:

    ```yaml
    swagger: '2.0'
    info:
    title: function-demo-energy.azurewebsites.net
    version: 1.0.0
    host: function-demo-energy.azurewebsites.net
    basePath: /
    schemes:
    - https
    - http
    paths:
    /api/TurbineRepair:
        post:
        operationId: /api/TurbineRepair/post
        produces: []
        consumes: []
        parameters: []
        description: >-
            Replace with Operation Object
            #http://swagger.io/specification/#operationObject
        responses:
            '200':
            description: Success operation
        security:
            - apikeyQuery: []
    definitions: {}
    securityDefinitions:
    apikeyQuery:
        type: apiKey
        name: code
        in: query
    ```

    Ta definicja jest określane jako _szablonu_ ponieważ wymaga większej ilości metadanych jako pełnej definicji OpenAPI. Będzie zmodyfikować definicję w następnym kroku.

## <a name="modify-the-openapi-definition"></a>Należy zmodyfikować definicję OpenAPI
Teraz, gdy masz definicji szablonu zmodyfikuj go, aby zapewnić dodatkowe metadane na temat operacji interfejsu API i struktury danych. W **definicji interfejsu API**, usuń definicję wygenerowane z `post` na końcu definicji Wklej poniższą zawartość, a następnie kliknij przycisk **zapisać**.

```yaml
    post:
      operationId: CalculateCosts
      description: Determines if a technician should be sent for repair
      summary: Calculates costs
      x-ms-summary: Calculates costs
      x-ms-visibility: important
      produces:
        - application/json
      consumes:
        - application/json
      parameters:
        - name: body
          in: body
          description: Hours and capacity used to calculate costs
          x-ms-summary: Hours and capacity
          x-ms-visibility: important
          required: true
          schema:
            type: object
            properties:
              hours:
                description: The amount of effort in hours required to conduct repair
                type: number
                x-ms-summary: Hours
                x-ms-visibility: important
              capacity:
                description: The max output of a turbine in kilowatts
                type: number
                x-ms-summary: Capacity
                x-ms-visibility: important
      responses:
        200:
          description: Message with cost and revenue numbers
          x-ms-summary: Message
          schema:
           type: object
           properties:
            message:
              type: string
              description: Returns Yes or No depending on calculations
              x-ms-summary: Message 
            revenueOpportunity:
              type: string
              description: The revenue opportunity cost
              x-ms-summary: RevenueOpportunity 
            costToFix:
              type: string
              description: The cost in $ to fix the turbine
              x-ms-summary: CostToFix
      security:
        - apikeyQuery: []
definitions: {}
securityDefinitions:
  apikeyQuery:
    type: apiKey
    name: code
    in: query
```

W takim przypadku należy można wkleić tylko aktualizacja metadanych, ale ważne jest, aby zrozumieć typy zmiany wprowadzone do domyślnego szablonu:

+ Określić czy interfejsu API tworzy i wykorzystuje dane w formacie JSON.

+ Określono wymaganych parametrów z ich nazwy i typy danych.

+ Wartości zwracane dla pomyślnej odpowiedzi, należy określić z ich nazwy i typy danych.

+ Podana przyjazna podsumowań i opisy dla interfejsu API, a jego operacji i parametry. Jest to istotne dla osób, które będą używać tej funkcji.

+ Dodane x-ms podsumowanie x-ms widoczność, które są używane w interfejsie użytkownika dla Flow firmy Microsoft i Logic Apps. Aby uzyskać więcej informacji, zobacz [OpenAPI rozszerzeń dla niestandardowych interfejsów API w Microsoft Flow](https://preview.flow.microsoft.com/documentation/customapi-how-to-swagger/).

> [!NOTE]
> Firma Microsoft pozostawione definicji zabezpieczeń z domyślną metodą uwierzytelniania klucza interfejsu API. Ta sekcja definicji zmieniłby użycie innego typu uwierzytelniania.

Aby uzyskać więcej informacji na temat definiowania operacje interfejsu API, zobacz [otwarty interfejs API specyfikacji](https://swagger.io/specification/#operationObject).

## <a name="test-the-openapi-definition"></a>Testowanie definicji OpenAPI
Przed użyciem definicji interfejsu API jest warto przetestować go w interfejsie użytkownika funkcji platformy Azure.

1. Na **Zarządzaj** kartę funkcji w obszarze **klucze hosta**, kopiowania **domyślne** klucza.

    ![Skopiuj klucz interfejsu API](media/functions-openapi-definition/copy-api-key.png)

    > [!NOTE]
    >Użyj tego klucza do testowania i można także używać, gdy wywołanie interfejsu API z usługi aplikacji lub usługi.

1. Wróć do definicji interfejsu API: **funkcja Pokaz energii** > **funkcji platformy** > **definicji interfejsu API**.

1. W okienku po prawej stronie kliknij **Uwierzytelnij**, wprowadź klucz interfejsu API, który możesz skopiować i kliknij przycisk **Uwierzytelnij**.

    ![Uwierzytelniania za pomocą klucza interfejsu API](media/functions-openapi-definition/authenticate-api-key.png)

1. Przewiń w dół i kliknij przycisk **spróbuj wykonać tę operację**.

    ![Spróbuj wykonać tę operację](media/functions-openapi-definition/try-operation.png)

1. Wprowadź wartości w polach **godziny** i **pojemności**.

    ![Wprowadź parametry](media/functions-openapi-definition/parameters.png)

    Zwróć uwagę, jak interfejsu użytkownika używa opisy z definicji interfejsu API.

1. Kliknij przycisk **Wyślij żądanie**, następnie kliknij przycisk **Pretty** kartę, aby wyświetlić dane wyjściowe.

    ![Wyślij żądanie](media/functions-openapi-definition/send-request.png)

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie funkcji na platformie Azure
> * Generowanie definicji OpenAPI przy użyciu narzędzia OpenAPI
> * Należy zmodyfikować definicję do udostępnienia dodatkowych metadanych
> * Testowanie definicji przez wywołanie funkcji

Przejdź do następnego tematu informacje na temat tworzenia aplikacji rozwiązanie PowerApps, który używa definicji OpenAPI, który został utworzony.
> [!div class="nextstepaction"]
> [Wywoływanie funkcji z rozwiązania PowerApps](functions-powerapps-scenario.md)
