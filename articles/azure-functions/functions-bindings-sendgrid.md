---
title: "Powiązania włączenie funkcji platformy Azure"
description: "Odwołanie powiązania włączenie funkcji platformy Azure."
services: functions
documentationcenter: na
author: tdykstra
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 11/29/2017
ms.author: tdykstra
ms.openlocfilehash: f24c2aecf44dd44fec05dc9a4d156ff408b0c953
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="azure-functions-sendgrid-bindings"></a>Powiązania włączenie funkcji platformy Azure

W tym artykule opisano sposób wysyłania wiadomości e-mail przy użyciu [SendGrid](https://sendgrid.com/docs/User_Guide/index.html) powiązania usługi Azure Functions. Środowisko Azure Functions obsługuje powiązanie danych wyjściowych SendGrid.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="example"></a>Przykład

Zapoznaj się z przykładem specyficzny dla języka:

* [Prekompilowane C#](#c-example)
* [Skryptu C#](#c-script-example)
* [JavaScript](#javascript-example)

### <a name="c-example"></a>Przykład C#

W poniższym przykładzie przedstawiono [wstępnie skompilowana funkcja C#](functions-dotnet-class-library.md) aby wyzwolić kolejki usługi Service Bus używa i SendGrid powiązania wyjściowego.

```cs
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    message = new SendGridMessage();
    message.AddTo(email.To);
    message.AddContent("text/html", email.Body);
    message.SetFrom(new EmailAddress(email.From));
    message.SetSubject(email.Subject);
}

public class OutgoingEmail
{
    public string To { get; set; }
    public string From { get; set; }
    public string Subject { get; set; }
    public string Body { get; set; }
}
```

Można pominąć ustawienie atrybutu `ApiKey` właściwości, jeśli masz klucz interfejsu API w polu Ustawienie aplikacji o nazwie "AzureWebJobsSendGridApiKey".

### <a name="c-script-example"></a>Przykładowy skrypt w języku C#

W poniższym przykładzie przedstawiono dane wyjściowe SendGrid powiązanie w *function.json* pliku i [funkcji skryptu C#](functions-reference-csharp.md) używającą powiązania.

W tym miejscu jest powiązanie danych *function.json* pliku:

```json 
{
    "bindings": [
        {
            "name": "message",
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

[Konfiguracji](#configuration) sekcji opisano te właściwości.

Oto kod skryptu C#:

```csharp
#r "SendGrid"
using System;
using SendGrid.Helpers.Mail;

public static void Run(TraceWriter log, string input, out Mail message)
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

### <a name="javascript-example"></a>Przykład JavaScript

W poniższym przykładzie przedstawiono dane wyjściowe SendGrid powiązanie w *function.json* pliku i [funkcji JavaScript](functions-reference-node.md) używającą powiązania.

W tym miejscu jest powiązanie danych *function.json* pliku:

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

[Konfiguracji](#configuration) sekcji opisano te właściwości.

Oto kod JavaScript:

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

## <a name="attributes"></a>Atrybuty

Dla [wstępnie skompilowana C#](functions-dotnet-class-library.md) funkcje, używają [SendGrid](https://github.com/Azure/azure-webjobs-sdk-extensions/blob/master/src/WebJobs.Extensions.SendGrid/SendGridAttribute.cs) atrybut, który jest zdefiniowany w pakiecie NuGet [Microsoft.Azure.WebJobs.Extensions.SendGrid](http://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.SendGrid).

Informacje o właściwości atrybutów, które można skonfigurować, zobacz [konfiguracji](#configuration). Oto `SendGrid` przykład atrybutu w podpisie metody:

```csharp
[FunctionName("SendEmail")]
public static void Run(
    [ServiceBusTrigger("myqueue", AccessRights.Manage, Connection = "ServiceBusConnection")] OutgoingEmail email,
    [SendGrid(ApiKey = "CustomSendGridKeyAppSettingName")] out SendGridMessage message)
{
    ...
}
```

Pełny przykład, zobacz [wstępnie skompilowana C# przykład](#c-example).

## <a name="configuration"></a>Konfiguracja

W poniższej tabeli opisano powiązania właściwości konfiguracyjne, które można ustawić w *function.json* pliku i `SendGrid` atrybutu.

|Właściwość Function.JSON | Właściwość atrybutu |Opis|
|---------|---------|----------------------|
|**Typ**|| Wymagana — musi być ustawiona `sendGrid`.|
|**Kierunek**|| Wymagana — musi być ustawiona `out`.|
|**Nazwa**|| Wymagana — nazwa zmiennej używane w kodzie funkcji żądania lub treści żądania. Ta wartość jest ```$return``` po tylko jednej wartości zwracanej. |
|**apiKey**|**ApiKey**| Nazwa ustawienia aplikacji, który zawiera klucz interfejsu API. Jeśli nie jest ustawiony, ustawienie domyślna aplikacja jest "AzureWebJobsSendGridApiKey".|
|**Aby**|**Do**| adres e-mail adresata. |
|**z**|**Z**| adres e-mail nadawcy. |
|**Temat**|**Temat**| temat wiadomości e-mail. |
|**tekst**|**Tekst**| Treść wiadomości e-mail. |

[!INCLUDE [app settings to local.settings.json](../../includes/functions-app-settings-local.md)]

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Dowiedz się więcej o usługę Azure functions wyzwalaczy i powiązań](functions-triggers-bindings.md)