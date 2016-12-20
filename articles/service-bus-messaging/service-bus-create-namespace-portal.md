---
title: "Tworzenie przestrzeni nazw usługi Service Bus za pomocą witryny Azure Portal | Microsoft Docs"
description: "Aby rozpocząć pracę z usługą Service Bus, konieczna jest przestrzeń nazw. Poniżej przedstawiono sposób jej utworzenia za pomocą usługi Azure Portal."
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
ms.sourcegitcommit: 0b1f6f7ec47e47f39407cdbfd5efef2a18944ecc
ms.openlocfilehash: 89610512c264cef4bd52728240ddc0a34091edea


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



<!--HONumber=Dec16_HO1-->


