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
ms.openlocfilehash: c9120babaa5f6da4f33bd60ba27434e24cb2f45e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="get-started-with-the-github-connector"></a>Rozpoczynanie pracy z łącznikiem usługi GitHub
GitHub to repozytorium Git opartych na sieci web usługi hosta. Zapewnia ona wszystkich wersji rozproszonej i kontroli źródła kodu zarządzania (SCM) funkcji Git, a także dodawanie własnych funkcji.

Rozpoczynanie pracy przez teraz tworzenie aplikacji logiki, zobacz [tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="create-a-connection-to-github"></a>Utwórz połączenie z usługi GitHub
Do tworzenia aplikacji logiki z usługi GitHub, należy najpierw utworzyć **połączenia** następnie podaj szczegóły następujące właściwości: 

| Właściwość | Wymagane | Opis |
| --- | --- | --- |
| Token |Yes |Podaj poświadczenia GitHub |

Po utworzeniu połączenia, można go wykonać akcje i będzie nasłuchiwać wyzwalaczy opisane w tym artykule. 

> [!INCLUDE [Steps to create a connection to GitHub](../../includes/connectors-create-api-github.md)]
> 

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/github/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).