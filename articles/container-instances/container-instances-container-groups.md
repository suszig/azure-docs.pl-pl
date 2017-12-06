---
title: "Kontener grupy wystąpień kontenera platformy Azure"
description: "Zrozumienie, jak działają grupy kontenerów w wystąpień kontenera platformy Azure"
services: container-instances
author: seanmck
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 568a99d44a5a32339d438ed1025670d12ecce791
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Kontener grupy wystąpień kontenera platformy Azure

Zasób najwyższego poziomu w wystąpień kontenera Azure jest grupy kontenerów. W tym artykule opisano, co to są grupy kontenerów i jakie typy scenariuszy umożliwiają one.

## <a name="how-a-container-group-works"></a>Jak działa grupa kontenera

Grupy kontenerów jest kolekcją kontenerów, które są planowane na tym samym komputerze-hoście i udostępniać cykl, sieci lokalnej i woluminy magazynu. Jest on podobny do koncepcji *pod* w [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) i [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

Na poniższym diagramie przedstawiono przykład grupy kontenera, który zawiera wiele kontenerów.

![Przykład grupy kontenera][container-groups-example]

Należy pamiętać, że:

- Grupa jest zaplanowane na komputerze hosta.
- Grupa przedstawia jeden publiczny adres IP, z jedną dostępnego portu.
- Grupa składa się z dwóch kontenerów. Jeden kontener nasłuchuje na porcie 80, podczas innych wykrywa port 5000.
- Grupa zawiera dwa udziały plików platformy Azure jako instalacji woluminu i każdego kontenera instaluje jednej akcji lokalnie.

### <a name="networking"></a>Sieć

Kontener grupy mają adres IP i port przestrzeni nazw na ten adres IP. Aby umożliwić klientom zewnętrznych do kontenera w obrębie grupy, musi ujawniać port, na adres IP i z kontenera. Ponieważ kontenery w ramach grupy używają portu przestrzeni nazw, mapowanie portów nie jest obsługiwane. Kontenery w obrębie grupy może nawiązać połączenie wzajemnie za pośrednictwem hosta lokalnego na portach, które mają one dostępne, nawet jeśli te porty nie są widoczne zewnętrznie na adres IP grupy.

### <a name="storage"></a>Magazyn

Można określić zewnętrzne woluminów należy zainstalować w grupie kontenera. Woluminy można mapować do określonej ścieżki w ramach poszczególnych kontenerów w grupie.

## <a name="common-scenarios"></a>Typowe scenariusze

Kontener wielu grupy są przydatne w sytuacjach, w miejsce podział pojedyncze zadanie funkcjonalności do niewielkiej liczby kontener obrazów, które mogą być dostarczane przez różnych zespołów i mieć zasobów oddzielne wymagania. Przykład użycia mogą obejmować:

- Kontener aplikacji i kontener rejestrowania. Kontener rejestrowania zbiera dane wyjściowe dzienników i metryk przechowywanych w ramach aplikacji głównej i zapisuje je do magazynu długoterminowego.
- Aplikacja i monitorowania kontenera. Kontener monitorowania okresowo wysyła żądanie do aplikacji, aby upewnić się, że jest uruchomiona i odpowiada prawidłowo i zgłasza alert, jeśli nie jest.
- Kontener obsługująca aplikacji sieci web i kontener ściąganie najnowsza zawartość z kontroli źródła.

## <a name="next-steps"></a>Następne kroki

Dowiedz się, jak [wdrożenie grupy kontenera wielu](container-instances-multi-container-group.md) z szablonem usługi Azure Resource Manager.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png