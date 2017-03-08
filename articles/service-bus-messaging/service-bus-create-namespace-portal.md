---
title: "Tworzenie przestrzeni nazw usługi Service Bus w witrynie Azure Portal | Microsoft Docs"
description: "Informacje dotyczące sposobu tworzenia przestrzeni nazw usługi Service Bus za pomocą witryny Azure Portal."
services: service-bus-messaging
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: 
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 11/30/2016
ms.author: jotaub
translationtype: Human Translation
ms.sourcegitcommit: 4a972b9b8b52a90f27afda98d8bdc661016d1fe1
ms.openlocfilehash: ba7093f8a2c06ab4cecf11207174a4871435ae64
ms.lasthandoff: 02/28/2017


---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Tworzenie przestrzeni nazw usługi Service Bus za pomocą witryny Azure Portal
Przestrzeń nazw jest wspólnym kontenerem dla wszystkich składników służących do obsługi komunikatów. W jednej przestrzeni nazw może znajdować się wiele kolejek i tematów, a przestrzenie nazw często pełnią rolę kontenerów aplikacji. Aktualnie istnieją dwa różne sposoby tworzenia przestrzeni nazw usługi Service Bus.

1. Azure Portal (w tym artykule)
2. [Szablony usługi Resource Manager][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Tworzenie przestrzeni nazw w witrynie Azure Portal
[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Gratulacje! Utworzono przestrzeń nazw obsługi komunikatów usługi Service Bus.

## <a name="next-steps"></a>Następne kroki
Zapoznaj się z [przykładami w naszym repozytorium GitHub][github-samples]. Przedstawiono w nich niektóre bardziej zaawansowane funkcje obsługi komunikatów w usłudze Azure Service Bus.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

