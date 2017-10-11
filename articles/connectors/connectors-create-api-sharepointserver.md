---
title: "Używany jest łącznik serwera programu SharePoint w aplikacji logiki | Dokumentacja firmy Microsoft"
description: "Rozpoczynanie pracy przy użyciu łącznika serwera programu SharePoint w aplikacji logiki"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 0238a060-d592-4719-b7a2-26064c437a1a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 0f3274816e279a1aa57febaa2f8294914900799a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-sharepoint-connector"></a>Rozpoczynanie pracy z łącznikiem programu SharePoint
Łącznik programu SharePoint umożliwia pracę z listy w programie SharePoint.

Rozpoczynanie pracy przez tworzenie aplikacji logiki; zobacz [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-sharepoint"></a>Utwórz połączenie programu SharePoint
Do korzystania z łącznika programu SharePoint, należy najpierw utworzyć **połączenia** następnie podaj szczegóły tych właściwości: 

| Właściwość | Wymagane | Opis |
| --- | --- | --- |
| Token |Tak |Podaj poświadczenia programu SharePoint |

Aby nawiązać połączenie **SharePoint**, wprowadź swoją tożsamość, (nazwa użytkownika i hasło, inteligentne poświadczeń karty, itp.) w programie SharePoint. Po zostały uwierzytelniono, można przystąpić do korzystania z łącznika programu SharePoint w aplikacji logiki. 

W Projektancie aplikacji logiki, wykonaj następujące kroki, aby zalogować się do programu SharePoint do utworzenia połączenia **połączenia** do użycia w aplikacji logiki:

1. Wprowadź w polu wyszukiwania programu SharePoint i poczekaj, aż wyszukiwania zwrócić wszystkie wpisy z programem SharePoint w nazwie:   
   ![Konfigurowanie programu SharePoint][1]  
2. Wybierz **SharePoint — po utworzeniu pliku**   
3. Wybierz **Zaloguj się do programu SharePoint**:   
   ![Konfigurowanie programu SharePoint][2]    
4. Podaj poświadczenia programu SharePoint, aby zalogować się do uwierzytelniania za pomocą programu SharePoint   
   ![Konfigurowanie programu SharePoint][3]     
5. Po zakończeniu uwierzytelniania będzie zostanie przekierowany do aplikacji logiki do ukończenia jej przez skonfigurowanie SharePoint **podczas tworzenia pliku** okna dialogowego.          
   ![Konfigurowanie programu SharePoint][4]  
6. Następnie można dodać inne wyzwalacze i akcje, które należy wykonać aplikację logiki.   
7. Zapisz swoją pracę, wybierając **zapisać** na pasku menu powyżej.  

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/sharepoint/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).

[1]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig1.png  
[2]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig2.png 
[3]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig3.png
[4]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig4.png
[5]: ../../includes/media/connectors-create-api-sharepointonline/connectionconfig5.png
