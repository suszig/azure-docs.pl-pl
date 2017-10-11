---
title: "Dodaj łącznik usługi Twilio w aplikacje logiki platformy Azure | Dokumentacja firmy Microsoft"
description: "Omówienie łącznika usługi Twilio z parametrami interfejsu API REST"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 43116187-4a2f-42e5-9852-a0d62f08c5fc
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 09/19/2016
ms.author: mandia; ladocs
ms.openlocfilehash: a790ac51b0fea7e3fa379d20e0e094e7ce0d7696
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-twilio-connector"></a>Rozpoczynanie pracy z łącznikiem usługi Twilio
Nawiązać połączenia z usługi Twilio do wysyłania i odbierania globalne wiadomości SMS i MMS, adresu IP. Za pomocą usługi Twilio można:

* Tworzenie sieci przepływu biznesowe na podstawie danych otrzymywanych z usługi Twilio. 
* Użyj akcje, które pobierają wiadomości, lista wiadomości i inne. Te akcje uzyskać odpowiedzi, a następnie udostępnić dane wyjściowe do innych działań. Na przykład gdy pojawi się nowy komunikat usługi Twilio, można ten komunikat i używać go w przepływie pracy usługi Service Bus. 

Rozpoczynanie pracy przez tworzenie aplikacji logiki; zobacz [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-twilio"></a>Utwórz połączenie z usługi Twilio
Po dodaniu tego łącznika do aplikacji logiki, wprowadź następujące wartości usługi Twilio:

| Właściwość | Wymagane | Opis |
| --- | --- | --- |
| Identyfikator konta |Tak |Wprowadź identyfikator konta usługi Twilio |
| Token dostępu |Tak |Wprowadź token dostępu usługi Twilio |

> [!INCLUDE [Steps to create a connection to Twilio](../../includes/connectors-create-api-twilio.md)]
> 
> 

Jeśli nie masz tokenu dostępu usługi Twilio, zobacz [tożsamości użytkowników i tokenów dostępu](https://www.twilio.com/docs/api/chat/guides/identity).

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/twilio/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).