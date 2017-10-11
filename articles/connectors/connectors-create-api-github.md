---
title: "Łącznik usługi GitHub w programie Azure Logic Apps | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji logiki z usługi aplikacji Azure. GitHub to repozytorium Git opartych na sieci web usługi hosta. Zapewnia ona wszystkich wersji rozproszonej i kontroli źródła kodu zarządzania (SCM) funkcji Git, a także dodawanie własnych funkcji."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 8f873e6c-f4c0-4c2e-a5bd-2e953efe5e2b
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: d4614b0ceff0ec0d36dbb1a136551f985f2fc1a1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-the-github-connector"></a>Rozpoczynanie pracy z łącznikiem usługi GitHub
GitHub to repozytorium Git opartych na sieci web usługi hosta. Zapewnia ona wszystkich wersji rozproszonej i kontroli źródła kodu zarządzania (SCM) funkcji Git, a także dodawanie własnych funkcji.

Rozpoczynanie pracy przez teraz tworzenie aplikacji logiki, zobacz [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="create-a-connection-to-github"></a>Utwórz połączenie z usługi GitHub
Do tworzenia aplikacji logiki z usługi GitHub, należy najpierw utworzyć **połączenia** następnie podaj szczegóły następujące właściwości: 

| Właściwość | Wymagane | Opis |
| --- | --- | --- |
| Token |Tak |Podaj poświadczenia usługi GitHub |

Po utworzeniu połączenia, można go wykonać akcje i będzie nasłuchiwać wyzwalaczy opisane w tym artykule. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/github/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).