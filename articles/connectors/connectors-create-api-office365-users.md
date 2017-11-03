---
title: "Dodaj użytkowników usługi Office 365 łącznika w aplikacjach logiki | Dokumentacja firmy Microsoft"
description: "Omówienie łącznika użytkowników usługi Office 365 z parametrami interfejsu API REST"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b2146481-9105-4f56-b4c2-7ae340cb922f
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 330f733440932a769eb0fe6031cd0d947a820080
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-office-365-users-connector"></a>Rozpoczynanie pracy z łącznikiem użytkowników usługi Office 365
Podłącz do użytkowników usługi Office 365 można pobrać profilów, Wyszukaj użytkowników i inne. Użytkowników usługi Office 365 możesz:

* Tworzenie sieci przepływu biznesowe na podstawie danych, który można uzyskać od użytkowników usługi Office 365. 
* Użyj akcji, które uzyskać bezpośrednich podwładnych pobrać menedżera profilu użytkownika i inne. Te akcje uzyskać odpowiedzi, a następnie udostępnić dane wyjściowe do innych działań. Na przykład uzyskać osoby bezpośrednich podwładnych, a następnie pobierać te informacje i zaktualizować bazę danych SQL Azure. 

Rozpoczynanie pracy przez teraz tworzenie aplikacji logiki, zobacz [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-office-365-users"></a>Utwórz połączenie użytkowników usługi Office 365
Po dodaniu tego łącznika do aplikacji logiki, należy zalogować się do konta użytkowników usługi Office 365 i Zezwalaj aplikacjom logiki do łączenia się z kontem.

> [!INCLUDE [Steps to create a connection to Office 365 Users](../../includes/connectors-create-api-office365users.md)]
> 
> 

Po utworzeniu połączenie, wprowadź właściwości użytkowników usługi Office 365, takich jak identyfikator użytkownika. **Dokumentacji interfejsu API REST** w tym temacie opisano te właściwości.

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/officeusers/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).