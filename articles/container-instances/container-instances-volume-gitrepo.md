---
title: "Zainstalować wolumin gitRepo wystąpień kontenera platformy Azure"
description: "Informacje o sposobie instalowania woluminu gitRepo w klonowania repozytorium Git do swoich wystąpień kontenera"
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 9acde9259fcb392458e7b2fa7d3369776978285e
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Zainstalować wolumin gitRepo w wystąpień kontenera platformy Azure

Dowiedz się, jak zainstalować *gitRepo* woluminu sklonować repozytorium Git do swoich wystąpień kontenera.

> [!NOTE]
> Instalowanie *gitRepo* wolumin jest obecnie ograniczone do kontenerów systemu Linux. Podczas gdy pracujemy, aby udostępnić wszystkie funkcje na potrzeby kontenerów systemu Windows, bieżące różnice dotyczące platform możesz znaleźć w temacie [Limity przydziałów i dostępność regionów dla usługi Azure Container Instances](container-instances-quotas.md).

## <a name="gitrepo-volume"></a>gitRepo woluminu

*GitRepo* woluminu instaluje katalogu i klonów określonego repozytorium Git do niego przy uruchamianiu kontenera. Za pomocą *gitRepo* woluminu w swoich wystąpień kontenera, można uniknąć dodawania kod w ten sposób w aplikacji.

Podczas instalowania *gitRepo* woluminu, można ustawić właściwości trzy do konfigurowania woluminu:

| Właściwość | Wymagane | Opis |
| -------- | -------- | ----------- |
| `repository` | Yes | Pełny adres URL, w tym `http://` lub `https://`, można sklonować repozytorium Git.|
| `directory` | Nie | Katalog, w którym można sklonować repozytorium. Ścieżka nie może zawierać ani rozpoczynać się od "`..`".  Jeśli określisz "`.`", repozytorium został sklonowany w katalogu woluminu. W przeciwnym razie repozytorium Git został sklonowany w podkatalogu o podanej nazwie w katalogu woluminu. |
| `revision` | Nie | Skrót zatwierdzenia poprawki w klonowania. Jeśli nie zostanie podany, `HEAD` został sklonowany poprawki. |

## <a name="mount-a-gitrepo-volume"></a>Zainstalować wolumin gitRepo

Aby zainstalować *gitRepo* woluminu w wystąpieniu kontenera, należy wdrożyć przy użyciu [szablonu usługi Azure Resource Manager](/azure/templates/microsoft.containerinstance/containergroups).

Najpierw należy wypełnić `volumes` tablicy w grupie kontenera `properties` sekcji szablonu. Następnie dla każdego kontenera w grupie kontenera, w którym chcesz zainstalować *gitRepo* woluminu, wypełnij `volumeMounts` tablicy w `properties` sekcja definicji kontenera.

Na przykład następujący szablon usługi Resource Manager tworzy grupę kontenera składające się z jeden kontener. Kontener klonów określonego przez dwie repozytoriów GitHub *gitRepo* bloków woluminu. Drugi wolumin zawiera dodatkowe właściwości określenia katalogu sklonować do oraz skrót zatwierdzenia określoną poprawkę sklonować.

[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

Wynikowa struktura katalogów dwóch repozytoriów sklonowany zdefiniowane w powyższych szablonu jest:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Aby zapoznać się przykładem kontenera wystąpienia wdrożenia z szablonem usługi Azure Resource Manager, zobacz [wdrożenia kontenera wielu grup wystąpień kontenera Azure](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Kolejne kroki

Dowiedz się, jak zainstalować inne typy woluminu w wystąpień kontenera platformy Azure:

* [Instalowanie udziału plików na platformę Azure w wystąpień kontenera platformy Azure](container-instances-volume-azure-files.md)
* [Zainstalować emptyDir woluminu w wystąpień kontenera platformy Azure](container-instances-volume-emptydir.md)
* [Zainstalować tajny woluminu w wystąpień kontenera platformy Azure](container-instances-volume-secret.md)
