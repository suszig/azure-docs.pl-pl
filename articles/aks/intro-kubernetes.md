---
title: "Wprowadzenie do usługi Azure Container Service dla rozwiązania Kubernetes | Microsoft Docs"
description: "Usługa Azure Container Service dla rozwiązania Kubernetes ułatwia wdrażanie aplikacji opartych na kontenerach i zarządzanie nimi na platformie Azure."
services: container-service
documentationcenter: 
author: gabrtv
manager: timlt
editor: 
tags: aks, azure-container-service
keywords: Kubernetes, Docker, Containers, Microservices, Azure
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/24/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: a8ac18464d0efcc0db96e1667f18f2f853208573
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Wprowadzenie do usługi kontenera platformy Azure (AKS)

Usługa kontenera platformy Azure (AKS) ułatwia tworzenie, konfigurowanie i Zarządzanie klastrem maszyn wirtualnych, które są wstępnie skonfigurowane do uruchamiania aplikacji konteneryzowanych. Umożliwia to używanie posiadanych umiejętności lub sięganie po duży i rosnący zasób wiedzy społeczności w celu wdrażania opartych na kontenerze aplikacji platformy Microsoft Azure i zarządzania nimi.

Za pomocą AKS, możliwość korzystania z funkcji korporacyjnej Azure, zachowując przenośność aplikacji za pośrednictwem Kubernetes i Docker format obrazu.

## <a name="using-azure-container-service-aks"></a>Przy użyciu usługi kontenera platformy Azure (AKS)
Naszym celem z AKS jest zapewnienie kontener Środowisko hostingu za pomocą open source, narzędzia i technologie, które są obecnie popularnością wśród klientów. W tym celu uwidaczniamy standardowe punkty końcowe interfejsu API rozwiązania Kubernetes. Za pomocą tych standardowych punktów końcowych można wykorzystać dowolne oprogramowanie, które jest w stanie komunikować się z klastrem usługi Kubernetes. Możesz wybrać narzędzie [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) lub [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Tworzenie klastra Kubernetes przy użyciu usługi kontenera platformy Azure (AKS)
Aby rozpocząć korzystanie z AKS, wdrożenie klastra AKS z [interfejsu wiersza polecenia Azure](./kubernetes-walkthrough.md) lub za pośrednictwem portalu (wyszukiwanie witryny Marketplace w celu **usługi kontenera platformy Azure**). Jeśli jesteś użytkownikiem zaawansowanym, który musi mieć większą kontrolę nad szablonami usługi Azure Resource Manager, możesz użyć projektu [acs-engine](https://github.com/Azure/acs-engine) typu open source do kompilowania własnego niestandardowego klastra Kubernetes i wdrażania go za pomocą interfejsu wiersza polecenia `az`.

### <a name="using-kubernetes"></a>Korzystanie z rozwiązania Kubernetes
Narzędzie Kubernetes automatyzuje proces wdrażania i skalowania aplikacji konteneryzowanych oraz zarządzania nimi. Narzędzie to obejmuje bogaty zestaw funkcji, m.in.:
* automatyczne pakowanie pudełek,
* mechanizm samonaprawiania
* skalowanie w poziomie,
* odnajdywanie usług i równoważenie obciążenia,
* zautomatyzowane wprowadzanie i wycofywanie zmian,
* zarządzanie kluczami tajnymi i konfiguracją,
* aranżacja magazynu,
* wykonywanie partii zadań.

## <a name="videos"></a>Filmy wideo

Usługą kontenera Azure (AKS) - Azure piątek, październik 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Narzędzia do projektowania i wdrażania aplikacji na Kubernetes - Azure OpenDev czerwca 2017:

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Dowiedz się więcej na temat wdrażania i zarządzania nimi AKS z AKS — Szybki Start.

> [!div class="nextstepaction"]
> [Samouczek AKS](./kubernetes-walkthrough.md)