---
title: "Dowiedz się, jak używać łącznika usługi Azure Service Bus w aplikacji logiki | Dokumentacja firmy Microsoft"
description: "Tworzenie aplikacji logiki z usługi aplikacji Azure. Podłącz do usługi Azure Service Bus do wysyłania i odbierania wiadomości. Można wykonać akcje, takie jak wysyłanie do kolejki, Wyślij do tematu odbierania z kolejki i otrzymywać subskrypcji."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: d6d14f5f-2126-4e33-808e-41de08e6721f
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 08/02/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 1e2ce06f5993280dbdb67121849591e67f7979e9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-azure-service-bus-connector"></a>Rozpoczynanie pracy z łącznika usługi Azure Service Bus
Podłącz do usługi Azure Service Bus do wysyłania i odbierania wiadomości. Można wykonać akcje, takie jak wysyłanie do kolejki, Wyślij do tematu odbierania z kolejki i otrzymywać subskrypcji.

Aby użyć [każdy łącznik](apis-list.md), należy najpierw utworzyć aplikację logiki. Możesz rozpocząć pracę przez [teraz tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Połączenia magistrali usług
Zanim aplikację logiki można uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć połączenie z usługą. A [połączenia](connectors-overview.md) udostępnia łączność między aplikacji logiki i innej usługi.  

> [!INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]
> 
> 

## <a name="use-a-service-bus-trigger"></a>Użyj wyzwalacz usługi Service Bus
Wyzwalacz to zdarzenie służy do uruchomienia przepływu pracy zdefiniowanych w aplikacji logiki. [Dowiedz się więcej o wyzwalaczy](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).  

> [!INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]
> 
> 

## <a name="use-a-service-bus-action"></a>Za pomocą akcji usługi Service Bus
Akcja jest przeprowadzane przez przepływ pracy zdefiniowanych w aplikacji logiki operacji. [Dowiedz się więcej o akcjach](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

[!INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/servicebus/). 

## <a name="next-steps"></a>Następne kroki
[Tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

