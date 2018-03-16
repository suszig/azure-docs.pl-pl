---
title: "Nawiązać połączenia z usługi GitHub z usługą Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Automatyczne przepływy pracy dla usługi GitHub z usługi Azure Logic Apps"
services: logic-apps
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 03/02/2018
ms.author: estfan; ladocs
ms.openlocfilehash: 830c3e6b5462c5d2d917388c0d0924e444342f93
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="connect-to-github"></a>Nawiązać połączenia z usługi GitHub

GitHub jest oparte na sieci web Git repozytorium hostingu usługi, która oferuje wszystkie kontrolki rozproszonej poprawki i funkcje zarządzania (SCM) kodu źródłowego w usłudze Git i inne funkcje.

Aby rozpocząć pracę z łącznikiem usługi GitHub [najpierw utworzyć aplikację logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Utwórz połączenie z usługi GitHub

Do korzystania z łącznika usługi GitHub w aplikacji logiki, należy najpierw utworzyć *połączenia* , a następnie podaj szczegóły tych właściwości: 

| Właściwość | Wymagane | Opis | 
| -------- | -------- | ----------- | 
| Token | Yes | Wprowadź swoje poświadczenia usługi GitHub. |

Po utworzeniu połączenia, można wykonać akcje i nasłuchiwania wyzwalaczy opisane w tym artykule.

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyzwalacze i akcje zdefiniowane w Swagger i żadnych limitów Przejrzyj [szczegóły łącznika](/connectors/github/).

## <a name="find-more-connectors"></a>Znajdź więcej łączników

* Przegląd [lista łączników](apis-list.md).