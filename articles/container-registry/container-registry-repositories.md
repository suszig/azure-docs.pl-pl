---
title: Repozytoria rejestru kontenera platformy Azure | Dokumentacja firmy Microsoft
description: "Jak używać repozytoria rejestru kontenera Azure obrazy usługi Docker"
services: container-registry
documentationcenter: 
author: cristy
manager: balans
editor: dlepow
ms.service: container-registry
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: cristyg
ms.openlocfilehash: d737df98fee955b57166cb44f45f61dff906514e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-container-registry-repositories"></a>Repozytoria rejestru kontenera platformy Azure

Rejestru kontenera platformy Azure umożliwia przechowywanie kontener obrazów w repozytoriów. Dzięki przechowywaniu obrazów w repozytoriów, może mieć grup obrazów (lub wersji obrazów) w izolowanym środowisku. Można określić te repozytoria po naciśnięciu obrazów do rejestru.


## <a name="prerequisites"></a>Wymagania wstępne
* **Usługa Azure Container Registry** — Tworzy rejestr kontenera w subskrypcji platformy Azure. Na przykład użyj witryny [Azure Portal](container-registry-get-started-portal.md) lub [interfejsu wiersza polecenia platformy Azure w wersji 2.0](container-registry-get-started-azure-cli.md).
* **Interfejs wiersza polecenia platformy Docker** — Aby skonfigurować lokalny komputer jako hosta platformy Docker i uzyskać dostęp do poleceń interfejsu wiersza polecenia platformy Docker, zainstaluj [aparat platformy Docker](https://docs.docker.com/engine/installation/).
* **Ściąganie obrazu** — ściąganie obrazu z publicznego rejestru Centrum Docker tagu go i wypchnąć go do rejestru. Aby uzyskać wskazówki dotyczące wypychania i ściągania obrazów, zobacz [obraz Docker wypychania do rejestru prywatnej Azure](container-registry-get-started-docker-cli.md).


## <a name="viewing-repositories-in-the-portal"></a>Wyświetlanie repozytoria w portalu

Gdy obrazy zostały przekazane do rejestru kontenera, można wyświetlić listę repozytoria hosting obrazów w portalu Azure.

Po wykonaniu kroków w [Push Docker obrazu do rejestru prywatnej Azure](container-registry-get-started-docker-cli.md) artykułu, po wykonaniu obrazu Nginx w rejestrze kontenera. W ramach instrukcji należy określić przestrzeń nazw dla obrazu. W poniższym przykładzie polecenie wypchnięcia obrazu NGinx w repozytorium "Przykłady":

```
docker push myregistry.azurecr.io/samples/nginx
```
 Usługa Azure Container Registry obsługuje wielopoziomowe przestrzenie nazw repozytoriów. Ta funkcja pozwala na grupowanie kolekcji obrazów związanych z określoną aplikacją lub kolekcji aplikacji związanych z określonymi zespołami programistycznymi lub operacyjnymi. Aby uzyskać więcej informacji na temat repozytoria w rejestrach kontenera, zobacz [rejestrów kontenera prywatnego Docker na platformie Azure](container-registry-intro.md).

Aby wyświetlić repozytoria rejestru kontenera:

1. Logowanie do witryny Azure Portal
2. Na **rejestru kontenera Azure** bloku, wybierz rejestru chcesz sprawdzić
3. W bloku rejestru kliknij **repozytoria** umożliwia wyświetlenie listy wszystkich repozytoria i obrazów
4. (Opcjonalnie) Wybierz określony obraz, aby zobaczyć tagów

![Repozytoria w portalu](./media/container-registry-repositories/container-registry-repositories.png)


## <a name="next-steps"></a>Następne kroki
Teraz, kiedy znasz już podstawy, możesz zacząć korzystać z rejestru! Na przykład rozpocznij wdrażanie obrazów kontenera do klastra usługi [Azure Container Service](https://azure.microsoft.com/documentation/services/container-service/).
