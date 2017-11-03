---
title: "Dodaj łącznik programu Outlook pakietu Office 365 w aplikacji logiki | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji logiki z łącznikiem usługi Office 365, aby umożliwić interakcję z usługą Office 365. Na przykład: tworzenie, edytowanie i aktualizowanie kontakty i elementy kalendarza."
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2f6cc2c-bba2-493a-b0ba-841785462a80
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 5335dae62e61659b68e8befb4ed0d404dffb800c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-office-365-outlook-connector"></a>Rozpoczynanie pracy z łącznika programu Outlook pakietu Office 365
Łącznik programu Outlook pakietu Office 365 umożliwia interakcję z programu Outlook w usłudze Office 365. Używaj tego łącznika do tworzenia, edycji i zaktualizuj kontakty i elementy kalendarza i również uzyskać, wysyłania i Odpowiedz na wiadomość e-mail.

W programie Outlook pakietu Office 365 możesz:

* Tworzenie przepływu pracy przy użyciu funkcji poczty e-mail i kalendarza w ramach usługi Office 365. 
* Wyzwalacze są używane do uruchomienia przepływu pracy po nowej wiadomości e-mail, po zaktualizowaniu elementu kalendarza i inne.
* Użyj akcji, aby wysłać wiadomość e-mail, należy utworzyć nowe zdarzenie kalendarza i inne. Na przykład jeśli jest nowy obiekt w usłudze Salesforce (wyzwalacz), należy wysłać wiadomość e-mail programu Office 365 Outlook (działanie). 

W tym temacie opisano sposób korzystania z łącznika programu Outlook pakietu Office 365 w aplikacji logiki, a także wyświetla wyzwalacze i akcje.

> [!NOTE]
> Ta wersja artykułu ma zastosowanie do aplikacji logiki ogólnodostępnej (GA).
> 
> 

Aby dowiedzieć się więcej na temat aplikacji logiki, zobacz [co to jest logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) i [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-office-365"></a>Łączenie z usługą Office 365
Zanim aplikację logiki można uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenia* do usługi. Połączenie stanowi połączenie między aplikacji logiki i innej usługi. Na przykład, aby połączyć program Outlook pakietu Office 365, należy najpierw usługi Office 365 *połączenia*. Aby utworzyć połączenie, wprowadź poświadczenia, zwykle używanego do uzyskania dostępu do usługi, który chcesz połączyć się z. Dlatego w programie Outlook pakietu Office 365 wprowadź poświadczenia konta usługi Office 365, aby utworzyć połączenie.

## <a name="create-the-connection"></a>Tworzenie połączenia
> [!INCLUDE [Steps to create a connection to Office 365](../../includes/connectors-create-api-office365-outlook.md)]
> 
> 

## <a name="use-a-trigger"></a>Użyć wyzwalacza
Wyzwalacz to zdarzenie służy do uruchomienia przepływu pracy zdefiniowanych w aplikacji logiki. Wyzwalacze "sondować" Usługa interwału i częstotliwość. [Dowiedz się więcej o wyzwalaczy](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. W aplikacji logiki wpisz "office 365" Aby uzyskać listę wyzwalaczy:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-trigger.png)
2. Wybierz **Office 365 Outlook - wkrótce uruchamiając zdarzeń nadchodzących**. Jeśli połączenie już istnieje, wybierz kalendarz, z listy rozwijanej.
   
    ![](./media/connectors-create-api-office365-outlook/sample-calendar.png)
   
    Po pojawieniu się zalogować, wprowadź znak w szczegóły, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-office365-outlook.md#create-the-connection) w tym temacie przedstawiono kroki. 
   
   > [!NOTE]
   > W tym przykładzie aplikacja logiki jest uruchamiana po zaktualizowaniu zdarzenia kalendarza. Aby wyświetlić wyniki tego wyzwalacza, Dodaj inną akcję wysyłanej wiadomości tekstowej. Na przykład dodać usługi Twilio *wysyła komunikat* akcji tego teksty podczas zdarzenia kalendarza jest uruchamiana w ciągu 15 minut. 
   > 
   > 
3. Wybierz **Edytuj** przycisk i ustaw **częstotliwość** i **interwał** wartości. Na przykład, jeśli chcesz wyzwalacza do sondowania co 15 minut, następnie ustawić **częstotliwość** do **minutę**i ustaw **interwał** do **15**. 
   
    ![](./media/connectors-create-api-office365-outlook/calendar-settings.png)
4. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Aplikację logiki jest zapisywana i automatycznie włączone.

## <a name="use-an-action"></a>Za pomocą akcji
Akcja jest przeprowadzane przez przepływ pracy zdefiniowanych w aplikacji logiki operacji. [Dowiedz się więcej o akcjach](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Wybierz znak plus. Zobacz kilka opcji: **Dodaj akcję**, **Dodaj warunek**, lub jeden z **więcej** opcje.
   
    ![](./media/connectors-create-api-office365-outlook/add-action.png)
2. Wybierz **Dodaj akcję**.
3. W polu tekstowym wpisz "office 365" Aby uzyskać listę dostępnych akcji.
   
    ![](./media/connectors-create-api-office365-outlook/office365-actions.png) 
4. W tym przykładzie wybierz **Office 365 Outlook - Tworzenie kontaktu**. Jeśli połączenie już istnieje, należy wybrać **identyfikator folderu**, **imię**i inne właściwości:  
   
    ![](./media/connectors-create-api-office365-outlook/office365-sampleaction.png)
   
    Jeśli zostanie wyświetlony monit o informacje dotyczące połączenia, wprowadź szczegóły, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-office365-outlook.md#create-the-connection) w tym temacie opisano te właściwości. 
   
   > [!NOTE]
   > W tym przykładzie utworzymy nowy kontakt w programie Outlook pakietu Office 365. Dane wyjściowe z innego wyzwalacza umożliwia utworzenie kontaktu. Na przykład dodać usług SalesForce *podczas tworzenia obiektu* wyzwalacza. Następnie dodaj Office 365 Outlook *utworzenie kontaktu* akcję, która używa pola SalesForce w celu utworzenia nowego nowy kontakt w usłudze Office 365. 
   > 
   > 
5. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Aplikację logiki jest zapisywana i automatycznie włączone.

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/office365connector/). 

## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md). Eksploruj dostępnych łączników w aplikacjach logiki w naszym [listy interfejsów API](apis-list.md).

