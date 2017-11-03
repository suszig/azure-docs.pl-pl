---
title: "Sposób użycia funkcji Azure SendGrid | Dokumentacja firmy Microsoft"
description: "Przedstawia sposób użycia SendGrid w funkcji platformy Azure"
services: functions
documentationcenter: na
author: rachelappel
manager: cfowler
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/31/2017
ms.author: rachelap
ms.openlocfilehash: 563bdac30176de131f6d8fd194713f482ceb75d9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-sendgrid-in-azure-functions"></a>Jak używać SendGrid w funkcji platformy Azure

## <a name="sendgrid-overview"></a>Omówienie SendGrid

Azure Functions obsługuje SendGrid output powiązania, aby umożliwić funkcji do wysyłania wiadomości e-mail przy użyciu kilku wierszy kodu i konta SendGrid.

Aby użyć interfejsu API SendGrid w funkcji platformy Azure, musisz mieć [konta SendGrid](http://SendGrid.com). Ponadto musi mieć klucz interfejsu API SendGrid. Zaloguj się do swojego konta SendGrid, a następnie kliknij przycisk **ustawienia** następnie **klucz interfejsu API** do wygenerowania klucza interfejsu API. Zachowanie dostęp do tego klucza, jak używasz w nadchodzących kroku.

Teraz można przystąpić do tworzenia aplikacji funkcji platformy Azure.

## <a name="create-an-azure-function-app"></a>Tworzenie aplikacji funkcji platformy Azure 

Aplikacje funkcji platformy Azure są kontenerami dla co najmniej jedną funkcję platformy Azure. Środowisko Azure functions to właśnie tę — funkcja. Każda funkcja Azure jest powiązany jeden wyzwalacz, który jest zdarzenie, które powoduje, że funkcja Uruchom.
Każdej funkcji mogą zawierać dowolną liczbę danych wejściowych lub wyjściowych powiązania. Powiązania są usług używanych w funkcji. SendGrid jest powiązania wyjściowego, który służy do wysyłania wiadomości e-mail. 

1. Zaloguj się do portalu Azure i [tworzenie aplikacji platformy Azure funkcja](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) lub Otwórz istniejącą aplikację w funkcji. 
2. Tworzenie funkcji platformy Azure. Aby zachować prosty, wybierz ręczne wyzwalacza i C#. 

 ![Tworzenie funkcji platformy Azure](./media/functions-how-to-use-sendgrid/functions-new-function-manual-trigger-page.png)

## <a name="configure-sendgrid-for-use-in-an-azure-function-app"></a>Konfigurowanie SendGrid do użycia w aplikacji Azure — funkcja

Klucz interfejsu API SendGrid muszą być przechowywane jako ustawienie aplikacji w celu używania go w funkcji. W polu ApiKey nie jest rzeczywista klucza interfejsu API SendGrid, ale ustawienie aplikacji definiować reprezentujące rzeczywistego klucza interfejsu API. Przechowywanie klucza w ten sposób jest zabezpieczeń, ponieważ jest oddzielony od dowolnego kodu lub pliki, które może być wyewidencjonowany do kontroli kodu źródłowego.

- Utwórz **AppSettings** klucza w aplikacji funkcji **ustawienia aplikacji**.

 ![Tworzenie funkcji platformy Azure](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-api-key-app-settings.png)

## <a name="configure-sendgrid-output-bindings"></a>Skonfiguruj SendGrid powiązania danych wyjściowych

SendGrid jest dostępna jako platformy Azure funkcja powiązania wyjściowego. Aby utworzyć SendGrid powiązania wyjściowego:

1. Przejdź do **integracji** kartę funkcji w portalu Azure.
2. Kliknij przycisk **nowe dane wyjściowe** utworzyć SendGrid powiązania wyjściowego.
3. Wypełnij **klucz interfejsu API** i **Nazwa parametru komunikatu** właściwości. Jeśli chcesz, możesz teraz wprowadzić inne właściwości lub ich zamiast tego kodu. Te ustawienia mogą być używane jako domyślne.

 ![Skonfiguruj SendGrid powiązania danych wyjściowych](./media/functions-how-to-use-sendgrid/functions-configure-sendgrid-output-bindings.png)

Dodawanie powiązania funkcji tworzy plik o nazwie **function.json** w folderze z funkcji. Ten plik zawiera te same informacje, które widać w funkcji Azure **integracji** karcie, ale w formacie Json formatowania. Ustawienie **ApiKey**, **komunikat**, i **z** pól utworzyć następujące wpisy w **function.json** pliku: 

```json
{
  "bindings": [    
    {
      "type": "sendGrid",
      "name": "message",
      "apiKey": "SendGridKey",
      "direction": "out",
      "from": "azure@contoso.com"
    }
  ],
  "disabled": false
}
```

Jeśli wolisz, użytkownik może modyfikować tego pliku samodzielnie bezpośrednio.

Teraz, gdy zostanie utworzony i skonfigurowany aplikacji funkcji i funkcji, można napisać kod, aby wysłać wiadomość e-mail.

## <a name="write-code-that-creates-and-sends-email"></a>Pisanie kodu, który tworzy i wysyła wiadomości e-mail

Interfejs API SendGrid zawiera wszystkie polecenia, które należy utworzyć i wysłać wiadomość e-mail.  

- Zastąp kod w funkcji z następującym kodem:

```cs
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
    // change to email of recipient
    personalization.AddTo(new Email("MoreEmailPlease@contoso.com"));   

    Content content = new Content
    {
        Type = "text/plain",
        Value = input
    };
    message.AddContent(content);
    message.AddPersonalization(personalization);
}
```

Zwróć uwagę, pierwszy wiersz zawiera ```#r``` dyrektywy, który odwołuje się do zestawu SendGrid. Następnie można użyć ```using``` instrukcji, aby łatwo uzyskiwać dostęp do obiektów w tej przestrzeni nazw. W kodzie, tworzenie wystąpień ```Mail```, ```Personalization```, i ```Content``` obiektów z interfejsu API SendGrid tworzące wiadomości e-mail. Po powrocie komunikat SendGrid dostarcza ją. 

Sygnatura funkcji zawiera również dodatkowe parametru typu out ```Mail``` o nazwie ```message```. Zarówno wejściowa i wyjściowa express powiązań, same jako parametry funkcji w kodzie. 

2. Testowanie kodu, klikając **testu** i wprowadzania komunikatu do **treść żądania** pola, klikając **Uruchom** przycisku.

 ![Testowanie kodu](./media/functions-how-to-use-sendgrid/functions-develop-test-sendgrid.png)

3. Sprawdź pocztę e-mail, aby sprawdzić, czy SendGrid wysyłane wiadomości e-mail. Należy przejść do adresu w kodzie z kroku 1 i zawiera komunikat z **treść żądania**.

## <a name="next-steps"></a>Następne kroki
W tym artykule wykazało, jak używać usługi SendGrid do tworzenia i wysyłania wiadomości e-mail. Aby dowiedzieć się więcej na temat korzystania z usługi Azure Functions w aplikacjach, zobacz następujące tematy: 

- [Najlepsze rozwiązania dotyczące usługi Azure Functions](functions-best-practices.md) wymieniono najważniejsze wskazówki podczas tworzenia usługi Azure Functions.

- [Dokumentacja dla deweloperów usługi Azure funkcji](functions-reference.md) programisty dokumentacja dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.

- [Testowanie usługi Azure Functions](functions-test-a-function.md) opis różnych narzędzi i technik testowania funkcji.