---
title: Monitor klastra Azure DC/OS - Datadog | Dokumentacja firmy Microsoft
description: "Monitorowanie klastra usługi kontenera platformy Azure z Datadog. Użyj interfejsu użytkownika sieci web platformy DC/OS, aby wdrożyć agentów Datadog do klastra."
services: container-service
documentationcenter: 
author: sauryadas
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Kontenery, Azure DC/OS, rozwiązanie Docker Swarm"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure
ms.date: 07/28/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 9dd451f994940d7cc3a59bd7fd08a8f067345e34
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/18/2017
---
# <a name="monitor-an-azure-container-service-dcos-cluster-with-datadog"></a>Monitor klastra usługi kontenera platformy Azure DC/OS z Datadog
W tym artykule wdrożymy Datadog agentów dla wszystkich węzłów w klastrze usługi kontenera platformy Azure agenta. Musisz mieć konto z Datadog dla tej konfiguracji. 

## <a name="prerequisites"></a>Wymagania wstępne
[Wdróż](container-service-deployment.md) i [połącz](../container-service-connect.md) klaster skonfigurowany przez usługę Azure Container Service. Przegląd [interfejsu użytkownika platformy Marathon](container-service-mesos-marathon-ui.md). Przejdź do [http://datadoghq.com](http://datadoghq.com) skonfigurować konto Datadog. 

## <a name="datadog"></a>Datadog
Datadog jest usługą monitorowania, która gromadzi dane monitorowania z kontenerów w ramach klastra usługi kontenera platformy Azure. Datadog ma pulpitu nawigacyjnego Docker integracji umożliwia wyświetlenie określonych metryk w kontenerów. Metryki zebrane z kontenerów są zorganizowane według Procesora, pamięci, sieci i we/wy. Datadog dzieli metryki na kontenery i obrazów. Przykładem jak wygląda interfejs użytkownika użycia procesora CPU jest poniżej.

![Datadog interfejsu użytkownika](./media/container-service-monitoring/datadog4.png)

## <a name="configure-a-datadog-deployment-with-marathon"></a>Konfigurowanie wdrożenia Datadog przy użyciu platformy Marathon
Te kroki opisano sposób konfigurowania i wdrażania aplikacji Datadog do klastra przy użyciu platformy Marathon. 

Dostęp za pośrednictwem interfejsu użytkownika platformy DC/OS [http://localhost:80 /](http://localhost:80/). Raz w Interfejsie użytkownika DC/OS przejdź do "Universe", który znajduje się w lewym dolnym rogu okna, następnie odszukaj "Datadog" i kliknij przycisk Zainstaluj.

![Pakiet Datadog w Uniwersum DC/OS](./media/container-service-monitoring/datadog1.png)

Teraz w celu ukończenia konfiguracji konieczne będzie konto Datadog lub bezpłatne konto próbne. Gdy jest zalogowany Datadog wyglądu witryny sieci Web w lewo i przejdź do integracji -> następnie [interfejsów API](https://app.datadoghq.com/account/settings#api). 

![Klucz interfejsu API Datadog](./media/container-service-monitoring/datadog2.png)

Następnie wprowadź klucz interfejsu API w konfiguracji Datadog w Uniwersum DC/OS. 

![Konfiguracja Datadog na całym świecie DC/OS](./media/container-service-monitoring/datadog3.png) 

W powyższej konfiguracji wystąpienia są ustawione na 10000000 to zawsze, gdy nowy węzeł zostanie dodany do klastra Datadog automatycznie wdraża agenta do tego węzła. To rozwiązanie tymczasowe. Po zainstalowaniu pakietu, należy przejść z powrotem do witryny sieci Web Datadog i Znajdź "[pulpity nawigacyjne](https://app.datadoghq.com/dash/list)." Z tego miejsca pojawi się niestandardowy i integracja z pulpitów nawigacyjnych. [Pulpitu nawigacyjnego Docker](https://app.datadoghq.com/screen/integration/docker) będą miały metryki kontenera potrzebne do monitorowania sieci klastra. 

