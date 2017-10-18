---
title: "Prywatne rejestry kontenerów platformy Docker na platformie Azure"
description: "Wprowadzenie do usługi Azure Container Registry dostarczającej oparte na chmurze, zarządzane rejestry prywatne platformy Docker."
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: ee2b652b-fb7c-455b-8275-b8d4d08ffeb3
ms.service: container-registry
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/11/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 664696d2f355609c76477765c2238c6d62253482
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Wprowadzenie do prywatnych rejestrów kontenerów platformy Docker na platformie Azure

Usługa Azure Container Registry to zarządzana usługa [rejestru platformy Docker](https://docs.docker.com/registry/) oparta na oprogramowaniu typu open-source Docker Registry 2.0. Twórz i obsługuj rejestry kontenerów platformy Azure w celu przechowywania prywatnych obrazów [kontenerów platformy Docker](https://www.docker.com/what-docker) i zarządzania nimi. Używaj rejestrów kontenerów na platformie Azure przy użyciu istniejących potoków programowania i wdrażania kontenerów oraz korzystaj z wiedzy społeczności platformy Docker.

Aby uzyskać ogólne informacje o platformie Docker i kontenerach, zobacz [podręcznik użytkownika platformy Docker](https://docs.docker.com/engine/userguide/).

## <a name="use-cases"></a>Przypadki zastosowań
Ściąganie obrazów z rejestru kontenerów platformy Azure do różnych celów wdrażania:

* **Skalowalne systemy organizowania** zarządzające konteneryzowanymi aplikacjami w klastrach hostów, włączając w to rozwiązania [DC/OS](https://docs.mesosphere.com/), [Docker Swarm](https://docs.docker.com/swarm/) i [Kubernetes](http://kubernetes.io/docs/).
* **Usługi platformy Azure** obsługujące kompilowanie i uruchamianie aplikacji na dużą skalę, w tym usługi [Container Service](../container-service/index.yml), [App Service](/app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/) i inne.

Deweloperzy mogą również przeprowadzać wypychanie do rejestru kontenerów w ramach przepływu pracy opracowywania kontenera. Na przykład mogą kierować dane do rejestru kontenerów z poziomu narzędzia integracji ciągłej lub narzędzia do wdrażania, takiego jak usługa [Visual Studio Team Services](https://www.visualstudio.com/docs/overview) lub [Jenkins](https://jenkins.io/).

## <a name="key-concepts"></a>Kluczowe pojęcia
* **Rejestr** — utwórz przynajmniej jeden rejestr kontenerów w subskrypcji platformy Azure. Każdy rejestr jest wspierany przez standardowe [konto magazynu](../storage/common/storage-introduction.md) platformy Azure w tej samej lokalizacji. Utwórz rejestr w tej samej lokalizacji platformy Azure, w której znajdują się wdrożenia, aby korzystać z lokalnego, bliskiego sieciowo magazynu obrazów kontenerów. W pełni kwalifikowana nazwa rejestru ma format `myregistry.azurecr.io`.

  Możesz [kontrolować dostęp](container-registry-authentication.md) do rejestru kontenerów za pomocą [nazwy głównej usługi](../active-directory/active-directory-application-objects.md) wspieranej przez usługę Azure Active Directory lub podanego konta administratora. Uruchom standardowe polecenie `docker login` w celu uwierzytelnienia za pomocą rejestru.

* **Rejestr zarządzany** — podczas tworzenia rejestru możesz zdecydować się na utworzenie rejestru zarządzanego lub rejestru bazującego na własnym koncie magazynu. Rejestry zarządzane oferują dodatkowe możliwości dla trzech jednostek SKU — Podstawowej, Standardowej i Premium. Obrazy w tych jednostkach SKU są przechowywane na kontach usługi Azure Storage zarządzanych przez usługę Azure Container Registry, która podnosi niezawodność i udostępnia nowe funkcje. Nowe możliwości obejmują integrację elementów webhook, uwierzytelnianie repozytorium za pomocą usługi Azure Active Directory oraz obsługę funkcji usuwania.

* **Repozytorium** — rejestr zawiera przynajmniej jedno repozytorium stanowiące grupę obrazów kontenerów. Usługa Azure Container Registry obsługuje wielopoziomowe przestrzenie nazw repozytoriów. Dzięki wielopoziomowym przestrzeniom nazw można grupować kolekcje obrazów związanych z określoną aplikacją lub kolekcje aplikacji związanych z określonymi zespołami programistycznymi lub operacyjnymi. Na przykład:

  * `myregistry.azurecr.io/aspnetcore:1.0.1` reprezentuje obraz całej firmy
  * `myregistry.azurecr.io/warrantydept/dotnet-build` reprezentuje obraz używany do tworzenia aplikacji platformy .NET współdzielony przez dział gwarancji
  * `myregistry.azurecr.io/warrantydept/customersubmissions/web` reprezentuje obraz sieci Web zgrupowany w aplikacji zgłoszeń klientów i należący do działu gwarancji

* **Obraz** — każdy obraz jest przechowywany w repozytorium i jest migawką tylko do odczytu kontenera platformy Docker. Rejestry kontenerów platformy Azure mogą obejmować zarówno obrazy systemu Windows, jak i Linux. Możesz kontrolować nazwy obrazów wszystkich wdrożeń kontenera. Użyj standardowych [poleceń platformy Docker](https://docs.docker.com/engine/reference/commandline/), aby wypchnąć obrazy do repozytorium lub aby ściągnąć je z repozytorium.

* **Kontener** — mianem kontenera określa się aplikację wraz z jej zależnościami, opakowaną w kompletny system plików, włączając w to kod, środowisko uruchomieniowe, narzędzia systemowe i biblioteki. Uruchom kontenery platformy Docker w oparciu o obrazy systemu Windows lub Linux, które zostały ściągnięte z rejestru kontenerów. Kontenery uruchomione na jednym komputerze współdzielą jądro systemu operacyjnego. Kontenery platformy Docker są całkowicie przenośne na wszystkie główne dystrybucje systemu Linux oraz systemy macOS i Windows.

## <a name="next-steps"></a>Następne kroki
* [Create a container registry using the Azure portal](container-registry-get-started-portal.md) (Tworzenie rejestru kontenerów za pomocą witryny Azure Portal)
* [Create a container registry using the Azure CLI](container-registry-get-started-azure-cli.md) (Tworzenie rejestru kontenerów za pomocą interfejsu wiersza polecenia platformy Azure)
* [Push your first image using the Docker CLI](container-registry-get-started-docker-cli.md) (Wypychanie pierwszego obrazu za pomocą interfejsu wiersza polecenia platformy Docker)
* Aby skompilować przepływ pracy integracji ciągłej i wdrożenia przy użyciu usług Visual Studio Team Services, Azure Container Service i Azure Container Registry, zobacz [Ciągła integracja/ciągłe dostarczanie z usługami Docker Swarm i VSTS](../container-service/dcos-swarm/container-service-docker-swarm-setup-ci-cd.md).