---
title: "Zainstalować emptyDir woluminu w wystąpień kontenera platformy Azure"
description: "Dowiedz się, jak można zainstalować woluminu emptyDir do udostępniania danych między kontenery w grupie kontenera w wystąpień kontenera platformy Azure"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 01b44f357d160a48852ee843cd8bebd138e95dfc
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Zainstalować emptyDir woluminu w wystąpień kontenera platformy Azure

Dowiedz się, jak zainstalować *emptyDir* woluminu do udostępniania danych między kontenery w grupie kontenera w wystąpień kontenera platformy Azure.

> [!NOTE]
> Instalowanie *emptyDir* wolumin jest obecnie ograniczone do kontenerów systemu Linux. Podczas gdy pracujemy, aby udostępnić wszystkie funkcje na potrzeby kontenerów systemu Windows, bieżące różnice dotyczące platform możesz znaleźć w temacie [Limity przydziałów i dostępność regionów dla usługi Azure Container Instances](container-instances-quotas.md).

## <a name="emptydir-volume"></a>emptyDir woluminu

*EmptyDir* wolumin zawiera katalog zapisywalny dostępne dla każdego kontenera w grupie kontenera. Kontenery w grupie można odczytywanie i zapisywanie tych samych plików w woluminie i może być instalowany przy użyciu tego samego lub innego ścieżek w każdego kontenera.

Przykładowe używa *emptyDir* woluminu:

* Miejsce na pliki tymczasowe
* Tworzenie punktów kontrolnych podczas długotrwałych zadań
* Zapisz dane pobierane przez kontener boczną i obsługiwanej przez kontener aplikacji

Dane w *emptyDir* woluminu jest zachowywane przez kontener awarii (Crash). Kontenery, które zostaną ponownie uruchomione, jednak nie ma gwarancji do utrwalenia danych w *emptyDir* woluminu.

## <a name="mount-an-emptydir-volume"></a>Zainstalować woluminu emptyDir

Można zainstalować woluminu emptyDir w wystąpieniu kontenera, należy wdrożyć przy użyciu [szablonu usługi Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Najpierw należy wypełnić `volumes` tablicy w grupie kontenera `properties` sekcji szablonu. Następnie dla każdego kontenera w grupie kontenera, w którym chcesz zainstalować *emptyDir* woluminu, wypełnij `volumeMounts` tablicy w `properties` sekcja definicji kontenera.

Na przykład następujący szablon usługi Resource Manager tworzy grupę kontenera składające się z dwóch kontenerów, każdy z których instalacji *emptyDir* woluminu:

[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Aby zapoznać się przykładem kontenera wystąpienia wdrożenia z szablonem usługi Azure Resource Manager, zobacz [wdrożenia kontenera wielu grup wystąpień kontenera Azure](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak zainstalować inne typy woluminu w wystąpień kontenera platformy Azure:

* [Instalowanie udziału plików na platformę Azure w wystąpień kontenera platformy Azure](container-instances-volume-azure-files.md)
* [Zainstalować wolumin gitRepo w wystąpień kontenera platformy Azure](container-instances-volume-gitrepo.md)
* [Zainstalować tajny woluminu w wystąpień kontenera platformy Azure](container-instances-volume-secret.md)