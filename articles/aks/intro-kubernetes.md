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
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="introduction-to-azure-container-service-aks"></a>Wprowadzenie do usługi Azure Container Service (AKS)

Usługa Azure Container Service (AKS) upraszcza tworzenie i konfigurację klastra maszyn wirtualnych, które są wstępnie skonfigurowane do uruchamiania konteneryzowanych aplikacji, oraz zarządzanie nim. Umożliwia to używanie posiadanych umiejętności lub sięganie po duży i rosnący zasób wiedzy społeczności w celu wdrażania opartych na kontenerze aplikacji platformy Microsoft Azure i zarządzania nimi.

Za pomocą usługi AKS możesz korzystać z funkcji klasy korporacyjnej platformy Azure, zachowując jednocześnie przenośność aplikacji dzięki usłudze Kubernetes i formatowi obrazów Docker.

## <a name="managed-kubernetes-in-azure"></a>Usługa Managed Kubernetes na platformie Azure

Usługa AKS zmniejsza złożoność i nakłady operacyjne związane z zarządzaniem klastrem Kubernetes, przenosząc znaczną część tej odpowiedzialności na platformę Azure. Jako hostowana usługa Kubernetes, platforma Azure obsługuje krytyczne zadania, takie jak monitorowanie kondycji i konserwacja. Ponadto płacisz tylko za węzły agenta w ramach Twoich klastrów, a nie za wzorce. Jako zarządzana usługa Kubernetes, usługa AKS zapewnia:

> [!div class="checklist"]
> * automatyczne poprawki i uaktualnienia wersji platformy Kubernetes,
> * łatwe skalowanie klastra,
> * samonaprawialną hostowaną warstwę kontroli (wzorce),
> * oszczędności — płacisz tylko za uruchomione węzły puli agentów.

Dzięki temu, że platforma Azure obsługuje zarządzanie węzłami w klastrze AKS, nie musisz już ręcznie wykonywać wielu zadań, takich jak uaktualnianie klastra. Ponieważ platforma Azure wykonuje te krytyczne zadania konserwacji za Ciebie, usługa AKS nie zapewnia bezpośredniego dostępu (jak w przypadku użycia protokołu SSH) do klastra.

## <a name="using-azure-container-service-aks"></a>Korzystanie z usługi Azure Container Service (AKS)
Celem usługi Azure Container Service jest zapewnienie środowiska hostingu kontenerów za pomocą narzędzi i technologii typu open source, które już dziś są popularne wśród naszych klientów. W tym celu uwidaczniamy standardowe punkty końcowe interfejsu API rozwiązania Kubernetes. Za pomocą tych standardowych punktów końcowych można wykorzystać dowolne oprogramowanie, które jest w stanie komunikować się z klastrem usługi Kubernetes. Możesz wybrać narzędzie [kubectl](https://kubernetes.io/docs/user-guide/kubectl-overview/), [helm](https://helm.sh/) lub [draft](https://github.com/Azure/draft).

## <a name="creating-a-kubernetes-cluster-using-azure-container-service-aks"></a>Tworzenie klastra Kubernetes przy użyciu usługi Azure Container Service (AKS)
Aby rozpocząć korzystanie z usługi AKS, musisz wdrożyć klaster usługi AKS przy użyciu [interfejsu wiersza polecenia platformy Azure](./kubernetes-walkthrough.md) lub za pośrednictwem portalu (wyszukaj w witrynie Marketplace termin **Azure Container Service**). Jeśli jesteś użytkownikiem zaawansowanym, który musi mieć większą kontrolę nad szablonami usługi Azure Resource Manager, możesz użyć projektu [acs-engine](https://github.com/Azure/acs-engine) typu open source do kompilowania własnego niestandardowego klastra Kubernetes i wdrażania go za pomocą interfejsu wiersza polecenia `az`.

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

Usługa Azure Container Service (AKS) — Piątek z Azure, październik 2017:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Container-Orchestration-Simplified-with-Managed-Kubernetes-in-Azure-Container-Service-AKS/player]
>
>

Narzędzia do tworzenia i wdrażania aplikacji w systemie Kubernetes (Azure OpenDev, czerwiec 2017 r.):

> [!VIDEO https://channel9.msdn.com/Events/AzureOpenDev/June2017/Tools-for-Developing-and-Deploying-Applications-on-Kubernetes/player]
>
>

Dowiedz się więcej na temat wdrażania usługi AKS i zarządzania nią z przewodnika Szybki start po usłudze AKS.

> [!div class="nextstepaction"]
> [Samouczek usługi AKS](./kubernetes-walkthrough.md)