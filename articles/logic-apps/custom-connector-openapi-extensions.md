---
title: "Rozszerzenia OpenAPI dla łączników niestandardowych - Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Rozszerzanie OpenAPI łącznik niestandardowy z zaawansowanych funkcji"
author: ecfan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: logic-apps
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/22/2017
ms.author: LADocs; estfan
ms.openlocfilehash: e8565019bd4631043485224c73c2fe60e633951c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="extend-your-custom-connectors-openapi-with-advanced-functionality"></a>Rozszerzanie OpenAPI łącznik niestandardowy z zaawansowanych funkcji

Aby utworzyć niestandardowe łączniki dla usługi Azure Logic Apps, Flow firmy Microsoft lub PowerApps firmy Microsoft, musisz podać pliku definicji OpenAPI jest dokument czytelną niezależny od języka, który opisuje Twój interfejs API operacji i parametry. Wraz z jego OpenAPI poza pole funkcji możesz również uwzględnić te rozszerzenia OpenAPI podczas tworzenia łączników niestandardowych dla aplikacji logiki i przepływu:

* [`summary`](#summary)
* [`x-ms-summary`](#x-ms-summary)
* [`description`](#description)
* [`x-ms-visibility`](#x-ms-visibility)
* [`x-ms-dynamic-values`](#x-ms-dynamic-values)
* [`x-ms-dynamic-schema`](#x-ms-dynamic-schema)

Poniżej przedstawiono więcej informacji na temat tych rozszerzeń:

<a name="summary"></a>

## <a name="summary"></a>Podsumowanie

Określa tytuł dla akcji (operacji). </br>
Ma zastosowanie do: operacji </br>
Zalecane: Użyj *zdaniu* dla `summary`. </br>
Przykład: "po dodaniu zdarzeń do kalendarza" lub "Wyślij wiadomość e-mail"

![dla każdej operacji "podsumowania"](./media/custom-connector-openapi-extensions/summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "summary": "Send an email",
    /// Other action properties here...
  }
},
```

<a name="x-ms-summary"></a>

## <a name="x-ms-summary"></a>x-ms podsumowanie

Określa tytuł dla jednostki. </br>
Dotyczy: parametry, schemat odpowiedzi </br>
Zalecane: Użyj *title przypadku* dla `x-ms-summary`. </br>
Przykład: "Identyfikator kalendarza", "Temat", "Opis zdarzenia" i tak dalej

!["x-ms Podsumowanie" dla każdej jednostki](./media/custom-connector-openapi-extensions/x-ms-summary.png)

``` json
"actions" {
  "Send_an_email": {
    /// Other action properties here...
    "parameters": [ 
      {
        /// Other parameters here...
        "x-ms-summary": "Subject",
        /// Other parameters here...
      }
    ]
  }
},
```

<a name="description"></a>

## <a name="description"></a>description

Określa pełne wyjaśnienie operacji funkcji lub format jednostki i funkcji. </br>
Dotyczy: schemat odpowiedzi operacji, parametry, </br>
Zalecane: Użyj *zdaniu* dla `description`. </br>
Przykład: "Ta operacja jest wyzwalane po dodaniu nowych zdarzeń do kalendarza", "Określ temat wiadomości e-mail" i tak dalej

!["opis", dla każdej operacji lub jednostki](./media/custom-connector-openapi-extensions/description.png)

``` json
"actions" {
  "Send_an_email": {
     "description": "Specify the subject of the mail",
     /// Other action properties here...
  }
},
```

<a name="visibility"></a>

## <a name="x-ms-visibility"></a>x-ms widoczności

Określa widoczność uwzględniającym działania użytkownika dla jednostki. </br>
Możliwe wartości: `important`, `advanced`, i`internal` </br>
Dotyczy: operacje, parametry, schematów

* `important`operacje i parametry są zawsze wyświetlane użytkownikowi pierwszy.
* `advanced`operacje i parametry są ukryte w menu dodatkowe.
* `internal`operacje i parametry są ukryte przez użytkownika.

> [!NOTE] 
> Dla parametrów, które są `internal` i `required`, możesz **musi** podanie domyślne wartości tych parametrów.

Przykład: **zobaczyć więcej** menu i **Pokaż zaawansowane opcje** menu są ukrywanie `advanced` operacje i parametry.

!["x-ms widoczności" Pokazywanie lub ukrywanie operacje i parametry](./media/custom-connector-openapi-extensions/x-ms-visibility.png)

``` json
"actions" {
  "Send_an_email": {
     /// Other action properties here...
     "parameters": [
         {
           "name": "Subject",
           "type": "string",
           "description": "Specify the subject of the mail",
           "x-ms-summary": "Subject",
           "x-ms-visibility": "important",
           /// Other parameter properties here...
         }
     ]
     /// Other action properties here...
  }
},
```

<a name="x-ms-dynamic-values"></a>

## <a name="x-ms-dynamic-values"></a>x-ms-dynamic wartości

Pokazuje listę wypełnione dla użytkownika, więc użytkownik może wybrać pozycję parametrów wejściowych dla operacji. </br>
Dotyczy: parametry </br>
Sposób użycia: Dodaj `x-ms-dynamic-values` obiektu do definicji wartości parametru. Na przykład, zobacz ten [próbki OpenAPI](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

!["ms-dynamic — wartości x" do wyświetlania listy](./media/custom-connector-openapi-extensions/x-ms-dynamic-values.png)

### <a name="properties-for-x-ms-dynamic-values"></a>Właściwości x-ms-dynamic wartości

| Nazwa | Wymagany lub opcjonalny | Opis | 
| ---- | -------------------- | ----------- | 
| **operationID** | Wymagane | Operacja do wywołania do wypełniania listy | 
| **wartość ścieżki** | Wymagane | Ciąg ścieżki w obiekcie wewnątrz `value-collection` odwołujący się do wartości parametru. </br>Jeśli `value-collection` nie zostanie określona, odpowiedź jest szacowana jako tablicy. | 
| **Tytuł wartości** | Optional (Opcjonalność) | Ciąg ścieżki w obiekcie wewnątrz `value-collection` odwołujący się do opis tej wartości. </br>Jeśli `value-collection` nie zostanie określona, odpowiedź jest szacowana jako tablicy. | 
| **Kolekcja wartości** | Optional (Opcjonalność) | Ciąg ścieżki, której wynikiem jest Tablica obiektów w ładunku odpowiedzi | 
| **Parametry** | Optional (Opcjonalność) | Obiekt, którego właściwości określ parametrów wejściowych wymaganych do wywołania operacji dynamic wartości | 
|||| 

Oto przykład, w którym są wyświetlane właściwości w `x-ms-dynamic-values`:

``` json
"x-ms-dynamic-values": {
  "operationId": "PopulateDropdown",
  "value-path": "name",
  "value-title": "properties/displayName",
  "value-collection": "value",
  "parameters": {
     "staticParameter": "<value>",
     "dynamicParameter": {
        "parameter": "<value-to-pass-to-dynamicParameter>"
     }
  }
}
```

## <a name="example-all-the-openapi-extensions-up-to-this-point"></a>Przykład: Wszystkich OpenAPI rozszerzeń do tego punktu.

``` json
"/api/lists/{listID-dynamic}": {
    "get": {
        "description": "Get items from a single list - uses dynamic values and outputs dynamic schema",
        "summary": "Gets items from the selected list",
        "operationID": "GetListItems",
        "parameters": [
           {
             "name": "listID-dynamic",
             "type": "string",
             "in": "path",
             "description": "Select the list from where you want outputs",
             "required": true,
             "x-ms-summary": "Select List",
             "x-ms-dynamic-values": {
                "operationID": "GetLists",
                "value-path": "id",
                "value-title": "name"
             }
           }
        ]
    }
}
```

<a name="x-ms-dynamic schema"></a>

## <a name="x-ms-dynamic-schema"></a>x-ms-dynamic schematu

Wskazuje, że schemat dla bieżącego parametru lub odpowiedzi jest dynamiczny. Ten obiekt może wywołać operację, która jest zdefiniowana przez wartość tego pola, dynamicznie odkrywania schematu i wyświetlić odpowiedniego interfejsu użytkownika do zbierania danych wprowadzonych przez użytkownika lub Pokaż dostępne pola. 

Dotyczy: parametry, odpowiedź

Sposób użycia: Dodaj `x-ms-dynamic-schema` obiektu do żądania parametr lub odpowiedzi treści definicji. Na przykład, zobacz [próbki OpenAPI](https://procsi.blob.core.windows.net/blog-images/sampleDynamicSwagger.json).

Ten przykład przedstawia, jak zmienia formularza danych wejściowych, na podstawie elementu, który użytkownik wybiera z listy rozwijanej:

!["x-ms dynamicznie schema" dla parametrów dynamicznych](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema.png)

I w tym przykładzie pokazano, jak zmienić dane wyjściowe na podstawie elementu, który użytkownik wybiera z listy rozwijanej. W tej wersji użytkownik wybierze "Samochodów":

!["x-ms dynamiczny — schematu odpowiedzi" dla wybranego elementu "Samochodów"](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output1.png)

W tej wersji użytkownik wybierze "Żywności":

!["x-ms dynamiczny — schematu — odpowiedzi" dla wybranego elementu "Żywności"](./media/custom-connector-openapi-extensions/x-ms-dynamic-schema-output2.png)

### <a name="properties-for-x-ms-dynamic-schema"></a>Właściwości x-ms-dynamic schematu

| Nazwa | Wymagany lub opcjonalny | Opis | 
| ---- | -------------------- | ----------- | 
| **operationID** | Wymagane | Operacja do wywołania dla pobierania schematu | 
| **Parametry** | Wymagane | Obiekt, którego właściwości określ parametrów wejściowych wymaganych do wywołania operacji dynamic schematu | 
| **wartość ścieżki** |Optional (Opcjonalność) | Ciąg ścieżki, która odwołuje się do właściwości o schemacie. </br>Jeśli nie zostanie określony, odpowiedź zakłada się, że zawiera schematu w właściwości obiektu głównego. | 
|||| 

Oto przykład parametru dynamicznego:

``` json
{
  "name": "dynamicListSchema",
  "in": "body",
  "description": "Dynamic schema for items in the selected list",
  "schema": {
    "type": "object",
    "x-ms-dynamic-schema": {
        "operationID": "GetListSchema",
        "parameters": {
          "listID": {
            "parameter": "listID-dynamic"
          }
        },
        "value-path": "items"
    }
  }
}
```

Oto przykład dynamiczne odpowiedzi:

``` json
"DynamicResponseGetListSchema": {
   "type": "object",
   "x-ms-dynamic-schema": {
      "operationID": "GetListSchema",
      "parameters": {
         "listID": {
            "parameter": "listID-dynamic"
         }
      },
      "value-path": "items"
    }
}
```

## <a name="next-steps"></a>Następne kroki

* [Opisz swoje niestandardowych interfejsów API i łączniki](../logic-apps/custom-connector-api-postman-collection.md)
* [Logic Apps: Zarejestrować Twojego łącznika](../logic-apps/logic-apps-custom-connector-register.md)
* [Przepływ: Zarejestrować Twojego łącznika](https://ms.flow.microsoft.com/documentation/register-custom-api/#register-your-custom-connector)