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
ms.date: 11/13/2017
ms.author: gamonroy
ms.custom: mvc
ms.openlocfilehash: 9fba9fdda3503ec80fede845466858825e3677a5
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Wprowadzenie do usługi kontenera platformy Azure (AKS)

Usługa kontenera platformy Azure (AKS) ułatwia tworzenie, konfigurowanie i Zarządzanie klastrem maszyn wirtualnych, które są wstępnie skonfigurowane do uruchamiania aplikacji konteneryzowanych. Umożliwia to używanie posiadanych umiejętności lub sięganie po duży i rosnący zasób wiedzy społeczności w celu wdrażania opartych na kontenerze aplikacji platformy Microsoft Azure i zarządzania nimi.

Za pomocą AKS, możliwość korzystania z funkcji korporacyjnej Azure, zachowując przenośność aplikacji za pośrednictwem Kubernetes i Docker format obrazu.

## <a name="managed-kubernetes-in-azure"></a>Zarządzane Kubernetes na platformie Azure

AKS zmniejsza się złożoność i nakłady operacyjne zarządzania klastra Kubernetes przenosząc znacznie że odpowiedzialność na platformie Azure. Hostowana usługa Kubernetes, Azure uchwytów krytyczne zadania, takie jak monitorowanie kondycji i konserwacji dla Ciebie. Ponadto płacisz tylko za węzłów agenta w ramach klastrów, nie dla serwerów głównych. Jako usługa zarządzana Kubernetes AKS zawiera:

> [!div class="checklist"]
> * Automatyczne uaktualnienia wersji Kubernetes i stosowanie poprawek
> * Skalowanie klastra łatwe
> * Samonaprawiania płaszczyzny obsługiwanego formantu (wzorce)
> * Oszczędności — płać tylko w przypadku uruchamiania agenta węzłów puli

Przy użyciu obsługi zarządzania węzłów w klastrze AKS platformy Azure nie trzeba ręcznie, wykonywanie wielu zadań, takich jak uaktualnienia klastra. Ponieważ Azure obsługuje te zadania konserwacyjne krytyczne dla Ciebie, AKS nie zapewnia bezpośredni dostęp (takich jak przy użyciu protokołu SSH) do klastra.

## <a name="using-azure-container-service-aks"></a>Przy użyciu usługi kontenera platformy Azure (AKS)
Celem AKS jest zapewnienie kontener Środowisko hostingu za pomocą open source, narzędzia i technologie, które są obecnie popularnością wśród klientów. W tym celu uwidaczniamy standardowe punkty końcowe interfejsu API rozwiązania Kubernetes. Za pomocą tych standardowych punktów końcowych można wykorzystać dowolne oprogramowanie, które jest w stanie komunikować się z klastrem usługi Kubernetes. Możesz wybrać narzędzie [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) lub [draft](https://github.com/Azure/draft).

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