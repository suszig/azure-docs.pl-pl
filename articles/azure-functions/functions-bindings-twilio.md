---
title: "Powiązanie usługi Twilio funkcji platformy Azure | Dokumentacja firmy Microsoft"
description: "Zrozumienie sposobu korzystania z usługi Azure Functions powiązania usługi Twilio."
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: "funkcje usługi Azure, funkcje, przetwarzania zdarzeń, dynamiczne obliczeń niekorzystającą architektury"
ms.assetid: a60263aa-3de9-4e1b-a2bb-0b52e70d559b
ms.service: functions
ms.devlang: multiple
ms.topic: reference
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/20/2016
ms.author: wesmc
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e8c5e8f2dfedae26486e1c8afbe0cec3f3228e86
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="send-sms-messages-from-azure-functions-using-the-twilio-output-binding"></a>Wysyłanie SMS komunikaty z usługi Azure Functions przy użyciu usługi Twilio powiązania wyjściowego
[!INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

W tym artykule opisano sposób konfigurowania i korzystania z usługi Azure Functions powiązania usługi Twilio. 

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

Azure Functions obsługuje usługi Twilio output powiązań, aby umożliwić funkcji do wysyłania wiadomości SMS tekstu przy użyciu kilku wierszy kodu i [usługi Twilio](https://www.twilio.com/) konta. 

## <a name="functionjson-for-the-twilio-output-binding"></a>Function.JSON dla usługi Twilio powiązania wyjściowego
Plik function.json zawiera następujące właściwości:

|Właściwość  |Opis  |
|---------|---------|
|**Nazwa**| Nazwa zmiennej używane w kodzie funkcja wiadomości SMS usługi Twilio. |
|**Typ**| należy wybrać opcję `twilioSms`.|
|**accountSid**| Ta wartość musi mieć ustawioną nazwę ustawienia aplikacji obsługującej Twojego identyfikatora Sid konta usługi Twilio.|
|**parametr authToken**| Ta wartość musi mieć ustawioną Nazwa ustawienia aplikacji, który zawiera token uwierzytelniania usługi Twilio.|
|**Aby**| Ta wartość jest równa numer telefonu, który tekst wiadomości SMS są wysyłane do.|
|**z**| Ta wartość jest równa numer telefonu, który tekst wiadomości SMS są wysyłane z.|
|**Kierunek**| należy wybrać opcję `out`.|
|**treści**| Ta wartość może być używana twardego kodu wiadomość SMS, jeśli nie trzeba ustawić ją dynamicznie w kodzie dla funkcji. |

Przykład function.json:

```json
{
  "type": "twilioSms",
  "name": "message",
  "accountSid": "TwilioAccountSid",
  "authToken": "TwilioAuthToken",
  "to": "+1704XXXXXXX",
  "from": "+1425XXXXXXX",
  "direction": "out",
  "body": "Azure Functions Testing"
}
```


## <a name="example-c-queue-trigger-with-twilio-output-binding"></a>Przykład C# kolejki wyzwalacza z usługi Twilio powiązania wyjściowego
#### <a name="synchronous"></a>Synchroniczne
Ten kod synchroniczne przykład wyzwalacza kolejki usługi Azure Storage używa parametru wyjściowego do wysyłania wiadomości SMS do klienta, który zamówienia.

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static void Run(string myQueueItem, out SMSMessage message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    message = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    message.Body = msg;
    message.To = order.mobileNumber;
}
```

#### <a name="asynchronous"></a>Asynchroniczne
Tej asynchronicznej przykładowy kod służący do wyzwalacz kolejki usługi Azure Storage wysyła wiadomość SMS do klienta, który zamówienia.

```cs
#r "Newtonsoft.Json"
#r "Twilio.Api"

using System;
using Newtonsoft.Json;
using Twilio;

public static async Task Run(string myQueueItem, IAsyncCollector<SMSMessage> message,  TraceWriter log)
{
    log.Info($"C# Queue trigger function processed: {myQueueItem}");

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    dynamic order = JsonConvert.DeserializeObject(myQueueItem);
    string msg = "Hello " + order.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the SMSMessage variable.
    SMSMessage smsText = new SMSMessage();

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    smsText.Body = msg;
    smsText.To = order.mobileNumber;

    await message.AddAsync(smsText);
}
```

## <a name="example-nodejs-queue-trigger-with-twilio-output-binding"></a>Przykład Node.js kolejki wyzwalacza z usługi Twilio powiązania wyjściowego
W tym przykładzie Node.js wysyła wiadomość SMS do klienta, który zamówienia.

```javascript
module.exports = function (context, myQueueItem) {
    context.log('Node.js queue trigger function processed work item', myQueueItem);

    // In this example the queue item is a JSON string representing an order that contains the name of a 
    // customer and a mobile number to send text updates to.
    var msg = "Hello " + myQueueItem.name + ", thank you for your order.";

    // Even if you want to use a hard coded message and number in the binding, you must at least 
    // initialize the message binding.
    context.bindings.message = {};

    // A dynamic message can be set instead of the body in the output binding. In this example, we use 
    // the order information to personalize a text message to the mobile number provided for
    // order status updates.
    context.bindings.message = {
        body : msg,
        to : myQueueItem.mobileNumber
    };

    context.done();
};
```

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]

