---
title: "Wystąpień kontenera Azure i kontener aranżacji"
description: "Dowiedz się, jak Azure kontener wystąpień interakcję z orchestrators kontenera."
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/09/2018
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 4954dcb4cb03407b85ad35aec94920e39644844b
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="azure-container-instances-and-container-orchestrators"></a>Wystąpień kontenera Azure i orchestrators kontenera

Ze względu na ich niewielki rozmiar i orientację aplikacji kontenerów dobrze nadają w środowiskach agile dostarczania i architektury mikrousługi systemem. Zadanie automatyzacji i zarządzanie dużą liczbę kontenerów i sposób ich interakcji nosi nazwę *aranżacji*. Kontener popularnych orchestrators obejmują Kubernetes DC/OS i Docker Swarm, które są dostępne w [usługi kontenera platformy Azure](https://docs.microsoft.com/azure/container-service/).

Wystąpień kontenera Azure udostępnia niektóre podstawowe możliwości planowania platform aranżacji, ale nie obejmuje usług wyższa wartość Podaj tymi platformami, a w rzeczywistości może być uzupełniające się z nimi. W tym artykule opisano zakres wystąpień kontenera Azure obsługuje i jakiego orchestrators kontener może korzystać z niego.

## <a name="traditional-orchestration"></a>Tradycyjny aranżacji

Standardowa definicji aranżacji obejmuje następujące zadania:

- **Planowanie**: danego obrazu kontenera i żądanie zasobu, Znajdź odpowiedniego komputera na którym ma być uruchamiany kontenera.
- **Koligacja/przeciwko-affinity**: określić, że zestaw kontenery powinien uruchomione w pobliżu innych (na wydajność) lub wystarczająco liniami (dostępności).
- **Monitorowanie kondycji**: Obejrzyj niepowodzeń kontenera i automatycznie Zaplanuj je ponownie.
- **Tryb failover**: Śledź co działa na każdym komputerze i ponowne planowanie kontenerów z maszyn nie powiodło się do węzłów w prawidłowym stanie.
- **Skalowanie**: Dodawanie lub usuwanie wystąpień kontenera, aby dopasować żądanie, ręcznie lub automatycznie.
- **Sieć**: zapewnienia koordynacji kontenerów do komunikacji między wieloma maszynami hosta sieci nakładki.
- **Odnajdowanie usługi**: Włączanie kontenerów do zlokalizowania wzajemnie automatycznie, nawet podczas przenoszenia między komputerami hostów i zmiana adresów IP.
- **Koordynowane uaktualnień aplikacji**: Zarządzanie uaktualnień kontenera, aby uniknąć aplikacji czas przestoju i włączyć wycofywanie, jeśli jakaś nieprawidłowość.

## <a name="orchestration-with-azure-container-instances-a-layered-approach"></a>Orchestration z wystąpień kontenera platformy Azure: warstwowego podejścia

Wystąpień kontenera Azure umożliwia warstwowego podejścia do aranżacji, podając wszystkie funkcje zarządzania i planowania wymagane do uruchomienia jeden kontener, umożliwiając platformy orchestrator do zarządzania zadaniami wielu kontenera na nim.

Ponieważ wszystkie podstawowej infrastruktury do wystąpień kontenera jest zarządzane przez usługę Azure, platformy orchestrator nie trzeba zajęcie się z znajdowanie maszyny do odpowiedniego hosta, na którym ma być uruchamiany jeden kontener. Elastyczność chmury gwarantuje, że co jest zawsze dostępna. Zamiast tego orchestrator można skoncentrować się na zadaniach, które upraszczają programowanie architektury usługi kontenera, w tym skalowanie i skoordynowany sposób uaktualnienia.

## <a name="potential-scenarios"></a>Potencjalne scenariusze

Podczas integracji programu orchestrator z wystąpień kontenera Azure jest nadal rodzącego, przewidujemy, że mogą pojawić się w kilku różnych środowiskach:

### <a name="orchestration-of-container-instances-exclusively"></a>Orchestration wystąpień kontenera wyłącznie

Ponieważ szybkie rozpoczęcie i naliczać opłaty przez drugą, środowisko wyłącznie w oparciu o wystąpień kontenera Azure oferuje najszybszy sposób rozpoczęcie pracy i postępowania w przypadku obciążeń wysokiej zmiennej.

### <a name="combination-of-container-instances-and-containers-in-virtual-machines"></a>Kombinacja wystąpień kontenera i kontenerów na maszynach wirtualnych

W przypadku obciążeń długotrwałe, stabilna organizowanie kontenery w klastrze dedykowanych maszyn wirtualnych zwykle będzie tańsze niż uruchomienie tego samego kontenery z wystąpień kontenera. Jednak wystąpień kontenera oferuje doskonałe rozwiązanie do szybkiego rozszerzania i instytucje ogólną wydajność na wypadek nieoczekiwanych lub krótkim okresie wzrostów użycia. Zamiast skalowania liczby maszyn wirtualnych w klastrze, wdrażanie dodatkowych kontenerów na tych komputerach, orchestrator można po prostu zaplanować dodatkowe kontenery za pomocą wystąpień kontenera, a następnie usuwania ich, gdy są one już wymagane.

## <a name="sample-implementation-azure-container-instances-connector-for-kubernetes"></a>Przykładowe zastosowanie: Azure łącznik wystąpień kontenera dla Kubernetes

Aby zademonstrować, jak kontenera platformy orchestration można zintegrować z wystąpień kontenera platformy Azure, możemy Rozpoczęto tworzenie [łącznik próbki dla Kubernetes][aci-connector-k8s].

Naśladuje łącznika Kubernetes [kubelet] [ kubelet-doc] przez zarejestrowanie jako węzeł o nieograniczonej pojemności i wysyła tworzenie [stanowiskami] [ pod-doc] jako kontenera grupy wystąpień kontenera platformy Azure.

<!-- ![ACI Connector for Kubernetes][aci-connector-k8s-gif] -->

Łączniki dla innych orchestrators mogą być zbudowane analogicznie zintegrowaną z podstawowych platform połączyć możliwości programu orchestrator interfejsu API z szybkość i łatwość zarządzania kontenerami w wystąpień kontenera platformy Azure.

> [!WARNING]
> Łącznik ACI dla Kubernetes *eksperymentalne* i nie powinna być używana w środowisku produkcyjnym.

## <a name="next-steps"></a>Kolejne kroki

Tworzenie Twojego pierwszego kontenera z wystąpień kontenera Azure za pomocą [Przewodnik Szybki Start](container-instances-quickstart.md).

<!-- IMAGES -->
[aci-connector-k8s-gif]: ./media/container-instances-orchestrator-relationship/aci-connector-k8s.gif

<!-- LINKS -->
[aci-connector-k8s]: https://github.com/virtual-kubelet/virtual-kubelet/tree/master/providers/azure
[kubelet-doc]: https://kubernetes.io/docs/admin/kubelet/
[pod-doc]: https://kubernetes.io/docs/concepts/workloads/pods/pod/