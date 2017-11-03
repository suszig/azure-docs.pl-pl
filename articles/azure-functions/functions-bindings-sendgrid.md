---
title: "Azure powiązania funkcji SendGrid | Dokumentacja firmy Microsoft"
description: "Azure odwołania wiązania włączenie funkcji"
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/26/2017
ms.author: rachelap
ms.openlocfilehash: 4cdafbe05e29d8b483c6b0e1daf41a36583d7b5e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-functions-sendgrid-bindings"></a>Powiązania włączenie funkcji platformy Azure

W tym artykule opisano sposób konfigurowania i pracować z SendGrid powiązania usługi Azure Functions. Sendgrid możesz użyć funkcji Azure do wysyłania niestandardowych wiadomości e-mail programowo.

W tym artykule jest informacje dla deweloperów usługi Azure Functions. Jeśli jesteś nowym użytkownikiem usługi Azure Functions, należy uruchomić z następującymi zasobami:

[Tworzenie pierwszej funkcji platformy Azure](functions-create-first-azure-function.md). 
[C#](functions-reference-csharp.md), [F #](functions-reference-fsharp.md), lub [węzła](functions-reference-node.md) odwołuje się do projektanta.

## <a name="functionjson-for-sendgrid-bindings"></a>Function.JSON dla powiązania SendGrid

Środowisko Azure Functions zapewnia SendGrid powiązania danych wyjściowych. Dane wyjściowe SendGrid powiązanie umożliwia tworzenie i wysyłanie wiadomości e-mail programowo. 

Powiązanie SendGrid obsługuje następujące właściwości:

|Właściwość  |Opis  |
|---------|---------|
|**Nazwa**| Wymagana — nazwa zmiennej używane w kodzie funkcji żądania lub treści żądania. Ta wartość jest ```$return``` po tylko jednej wartości zwracanej. |
|**Typ**| Wymagana — musi być ustawiona `sendGrid`.|
|**Kierunek**| Wymagana — musi być ustawiona `out`.|
|**apiKey**| Wymagana — musi być ustawiona nazwa klucza interfejsu API przechowywanych w aplikacji funkcji, ustawień aplikacji. |
|**Aby**| adres e-mail adresata. |
|**z**| adres e-mail nadawcy. |
|**Temat**| temat wiadomości e-mail. |
|**tekst**| Treść wiadomości e-mail. |

Przykład **function.json**:

```json 
{
    "bindings": [
        {
            "name": "$return",
            "type": "sendGrid",
            "direction": "out",
            "apiKey" : "MySendGridKey",
            "to": "{ToEmail}",
            "from": "{FromEmail}",
            "subject": "SendGrid output bindings"
        }
    ]
}
```

> [!NOTE]
> Azure Functions przechowuje informacje dotyczące połączenia, a klucze interfejsu API zgodnie z ustawieniami aplikacji, dzięki czemu nie są sprawdzane w Twoim repozytorium kontroli źródła. Ta akcja chroni poufne informacje.
>
>

## <a name="c-example-of-the-sendgrid-output-binding"></a>C# przykład SendGrid powiązania wyjściowego

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static Mail Run(TraceWriter log, string input, out Mail message)
{
     message = new Mail
    {        
        Subject = "Azure news"          
    };

    var personalization = new Personalization();
    personalization.AddTo(new Email("recipient@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

## <a name="node-example-of-the-sendgrid-output-binding"></a>Przykład węzła SendGrid powiązania wyjściowego

```javascript
module.exports = function (context, input) {    
    var message = {
         "personalizations": [ { "to": [ { "email": "sample@sample.com" } ] } ],
        from: { email: "sender@contoso.com" },        
        subject: "Azure news",
        content: [{
            type: 'text/plain',
            value: input
        }]
    };

    context.done(null, message);
};

```

## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje o innych powiązań i Wyzwalacze dla usługi Azure Functions zobacz 
- [Azure funkcje wyzwalaczy i powiązań dewelopera](functions-triggers-bindings.md)

- [Najlepsze rozwiązania dotyczące usługi Azure Functions](functions-best-practices.md) wymieniono najważniejsze wskazówki podczas tworzenia usługi Azure Functions.

- [Dokumentacja dla deweloperów usługi Azure funkcji](functions-reference.md) programisty dokumentacja dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.
