---
title: Tworzenie przestrzeni nazw usługi Service Bus za pomocą usługi Azure Portal | Microsoft Docs
description: Aby rozpocząć pracę z usługą Service Bus, konieczna jest przestrzeń nazw. Poniżej przedstawiono sposób jej utworzenia za pomocą usługi Azure Portal.
services: service-bus
documentationcenter: .net
author: jtaubensee
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 08/22/2016
ms.author: jotaub

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


<!--HONumber=Oct16_HO3-->


