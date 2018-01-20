---
title: "Korzystanie z łącznika zapas czasu w aplikacjach logiki platformy Azure | Dokumentacja firmy Microsoft"
description: "Nawiązać zapas czasu w aplikacjach logiki"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 234cad64-b13d-4494-ae78-18b17119ba24
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 04ea4508495b227d6ace4a3105f283c474c51d14
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-slack-connector"></a>Rozpoczynanie pracy z Slack łącznika
Slack jest narzędziem do komunikacji zespołowej, które łączy całą komunikację zespołową w jednym miejscu. Umożliwia natychmiastowe wyszukiwanie i dostęp do komunikacji z dowolnego miejsca. 

Rozpoczynanie pracy przez tworzenie aplikacji logiki teraz; zobacz [tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-slack"></a>Utwórz połączenie zapas czasu
Do korzystania z łącznika Slack, należy najpierw utworzyć **połączenia** następnie podaj szczegóły tych właściwości: 

| Właściwość | Wymagane | Opis |
| --- | --- | --- |
| Token |Yes |Podaj poświadczenia usługi Slack |

Wykonaj następujące kroki, aby zalogować się do zapas czasu i zakończyć konfigurację Slack **połączenia** w aplikacji logiki:

1. Wybierz **cyklu**
2. Wybierz **częstotliwość** , a następnie wprowadź **interwał**
3. Wybierz **Dodaj akcję**  
   ![Skonfiguruj zapas czasu][1]  
4. W polu wyszukiwania wprowadź zapas czasu i poczekaj, aż wyszukiwania zwrócić wszystkie wpisy z zapas czasu w nazwie
5. Wybierz **Slack - Post wiadomości**
6. Wybierz **Zaloguj się do zapas czasu**:  
   ![Skonfiguruj zapas czasu][2]
7. Podaj poświadczenia Slack logować się do autoryzowania aplikacji    
   ![Skonfiguruj zapas czasu][3]  
8. Możesz zostanie przekierowany do dziennika w organizacji na stronie. **Autoryzowanie** zapas czasu wchodzić w interakcje z aplikacji logiki:      
   ![Skonfiguruj zapas czasu][5] 
9. Po zakończeniu autoryzacji użytkownik będzie przekierowany do aplikacji logiki do ukończenia jej przez skonfigurowanie **Slack — pobranie wszystkich wiadomości** sekcji. Dodawanie innych wyzwalacze i akcje, które są potrzebne.  
   ![Skonfiguruj zapas czasu][6]
10. Zapisz swoją pracę, wybierając **zapisać** na pasku menu powyżej.

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/slack/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).

[1]: ./media/connectors-create-api-slack/connectionconfig1.png
[2]: ./media/connectors-create-api-slack/connectionconfig2.png 
[3]: ./media/connectors-create-api-slack/connectionconfig3.png
[4]: ./media/connectors-create-api-slack/connectionconfig4.png
[5]: ./media/connectors-create-api-slack/connectionconfig5.png
[6]: ./media/connectors-create-api-slack/connectionconfig6.png
