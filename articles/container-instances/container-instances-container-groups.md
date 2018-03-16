---
title: "Kontener grupy wystąpień kontenera platformy Azure"
description: "Zrozumienie, jak działają grupy kontenerów w wystąpień kontenera platformy Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 12/19/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 7e8a5014ce9168ba3d67d175935649bfd9fec511
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="container-groups-in-azure-container-instances"></a>Kontener grupy wystąpień kontenera platformy Azure

Zasób najwyższego poziomu w wystąpień kontenera Azure jest *grupy kontenerów*. W tym artykule opisano, co to są grupy kontenerów i jakiego rodzaju scenariusze, które umożliwiają one.

## <a name="how-a-container-group-works"></a>Jak działa grupa kontenera

Grupy kontenerów jest kolekcją kontenerów, które są planowane na tym samym komputerze-hoście. Kontenery w grupie kontenera udostępniania cykl, sieci lokalnej i woluminy magazynu. Jest on podobny do koncepcji *pod* w [Kubernetes] [ kubernetes-pod] i [DC/OS][dcos-pod].

Na poniższym diagramie przedstawiono przykład grupy kontenera, który zawiera wiele kontenerów.

![Diagram grupy kontenera][container-groups-example]

Ten przykład grupy kontenerów:

* Zaplanowano na komputerze hosta.
* Przedstawia jeden publiczny adres IP, z jedną dostępnego portu.
* Składa się z dwóch kontenerów. Jeden kontener nasłuchuje na porcie 80, podczas innych wykrywa port 5000.
* Zawiera dwie udziały plików platformy Azure jako instalacji woluminu i każdego kontenera instaluje jednej akcji lokalnie.

> [!NOTE]
> Kontener wielu grup są obecnie ograniczone do kontenerów systemu Linux. Podczas gdy pracujemy, aby udostępnić wszystkie funkcje na potrzeby kontenerów systemu Windows, bieżące różnice dotyczące platform możesz znaleźć w temacie [Limity przydziałów i dostępność regionów dla usługi Azure Container Instances](container-instances-quotas.md).

### <a name="deployment"></a>Wdrożenie

**Grupy kontenerów** ma alokacji zasobów minimalną 1 vCPU i 1 GB pamięci. Poszczególne **kontenery** można udostępnić przy użyciu mniejszej niż 1 vCPU i 1 GB pamięci. W ramach kontenera grupy dystrybucji zasobów można dostosować, aby wiele kontenerów w granicach określonych na poziomie grupy kontenera. Na przykład dwa kontenery z 0,5 vCPU znajdującej się w obrębie grupy kontenera przydzielone 1 vCPU.

### <a name="networking"></a>Networking

Kontener grupy mają adres IP i port przestrzeni nazw na ten adres IP. Aby umożliwić klientom zewnętrznych do kontenera w obrębie grupy, musi ujawniać port, na adres IP i z kontenera. Ponieważ kontenery w ramach grupy używają portu przestrzeni nazw, mapowanie portów nie jest obsługiwane. Kontenery w obrębie grupy może nawiązać połączenie wzajemnie za pośrednictwem hosta lokalnego na portach, które mają one dostępne, nawet jeśli te porty nie są widoczne zewnętrznie na adres IP grupy.

### <a name="storage"></a>Magazyn

Można określić zewnętrzne woluminów należy zainstalować w grupie kontenera. Woluminy można mapować do określonej ścieżki w ramach poszczególnych kontenerów w grupie.

## <a name="common-scenarios"></a>Typowe scenariusze

Kontener wielu grupy są przydatne w sytuacjach, w miejsce podział pojedyncze zadanie funkcjonalności do niewielkiej liczby kontener obrazów, które mogą być dostarczane przez różnych zespołów i mieć zasobów oddzielne wymagania.

Przykład użycia mogą obejmować:

* Kontener aplikacji i kontener rejestrowania. Kontener rejestrowania zbiera dane wyjściowe dzienników i metryk przechowywanych w ramach aplikacji głównej i zapisuje je do magazynu długoterminowego.
* Aplikacja i monitorowania kontenera. Kontener monitorowania okresowo wysyła żądanie do aplikacji, aby upewnić się, że jest uruchomiona i odpowiada prawidłowo i zgłasza alert, jeśli nie jest.
* Kontener obsługująca aplikacji sieci web i kontener ściąganie najnowsza zawartość z kontroli źródła.

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak [wdrożenie grupy kontenera wielu](container-instances-multi-container-group.md) z szablonem usługi Azure Resource Manager.

<!-- IMAGES -->
[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png

<!-- LINKS - External -->
[dcos-pod]: https://dcos.io/docs/1.10/deploying-services/pods/
[kubernetes-pod]: https://kubernetes.io/docs/concepts/workloads/pods/pod/
