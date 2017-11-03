---
title: "Łącznik SMTP w programie Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji logiki z usługi aplikacji Azure. Utwórz połączenie SMTP, aby wysyłać wiadomości e-mail."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d4141c08-88d7-4e59-a757-c06d0dc74300
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/15/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1cf96bbf8bd215d7ddb3c99860a5cb4e668be3c2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-smtp-connector"></a>Rozpoczynanie pracy z łącznikiem SMTP
Utwórz połączenie SMTP, aby wysyłać wiadomości e-mail.

Aby użyć [każdy łącznik](apis-list.md), należy najpierw utworzyć aplikację logiki. Możesz rozpocząć pracę przez [teraz tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Nawiązać SMTP
Zanim aplikację logiki można uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenia* do usługi. A [połączenia](connectors-overview.md) udostępnia łączność między aplikacji logiki i innej usługi. Na przykład, aby nawiązać połączenie SMTP, należy najpierw SMTP *połączenia*. Aby utworzyć połączenie, wprowadź poświadczenia, zwykle używanego do uzyskania dostępu do usługi, gdy połączysz się. Tak w tym przykładzie SMTP, wprowadź poświadczenia do połączenia nazwa, adres serwera SMTP i dane logowania użytkownika, aby utworzyć połączenie SMTP.  

### <a name="create-a-connection-to-smtp"></a>Utwórz połączenie SMTP
> [!INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]
> 
> 

## <a name="use-an-smtp-trigger"></a>Użyj wyzwalacz SMTP
Wyzwalacz to zdarzenie służy do uruchomienia przepływu pracy zdefiniowanych w aplikacji logiki. [Dowiedz się więcej o wyzwalaczy](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

W tym przykładzie, ponieważ SMTP nie ma wyzwalacz własnych, użyjemy **Salesforce — po utworzeniu obiektu** wyzwalacza. Wyzwalacz aktywuje po utworzeniu nowego obiektu w usłudze Salesforce. W naszym przykładzie skonfigurujemy Twoje go tak, aby zawsze nowego potencjalnego klienta jest tworzony w Salesforce, *wysyłania wiadomości e-mail* akcja jest wykonywana za pośrednictwem łącznika SMTP powiadomienie potencjalnego nowego klienta tworzona.

1. Wprowadź *salesforce* w polu wyszukiwania w Projektancie aplikacji logiki wybiorą **Salesforce — po utworzeniu obiektu** wyzwalacza.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  
2. **Podczas tworzenia obiektu** formant jest wyświetlany.
   ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  
3. Wybierz **typ obiektu** następnie wybierz *prowadzić* z listy obiektów. W tym kroku wskazujesz, tworzysz wyzwalacz, który powiadomi aplikację logiki, zawsze, gdy nowy realizacji jest tworzony w usłudze Salesforce.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  
4. Wyzwalacz został utworzony.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Za pomocą akcji SMTP
Akcja jest przeprowadzane przez przepływ pracy zdefiniowanych w aplikacji logiki operacji. [Dowiedz się więcej o akcjach](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

Teraz, wyzwalacz został dodany, wykonaj następujące kroki, aby dodać akcję SMTP, która zostanie przeprowadzona po utworzeniu nowego potencjalnego klienta w usłudze Salesforce.

1. Wybierz **+ nowy krok** Aby dodać akcję chcesz wykonać po utworzeniu nowego potencjalnego klienta.  
   ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  
2. Wybierz **Dodaj akcję**. Służy to pole wyszukiwania, gdzie możesz wyszukać dowolną akcję użytkownik chce przejąć.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  
3. Wprowadź *smtp* do wyszukania akcji związanych z SMTP.  
4. Wybierz **SMTP — Wyślij wiadomość E-mail** jako akcję wykonywaną, gdy jest tworzony nowy potencjalnych klientów. Zostanie otwarty blok kontroli akcji. Należy ustanowić połączenie smtp w bloku projektanta, jeśli nie zostało zrobione to wcześniej.  
   ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    
5. Wprowadź informacje żądane poczty e-mail w **SMTP — Wyślij wiadomość E-mail** bloku.  
   ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  
6. Zapisz swoją pracę, aby aktywować przepływ pracy.  

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/smtpconnector/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).