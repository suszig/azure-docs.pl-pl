---
title: "Monitorowanie klastra usługi Azure Container Service przy użyciu rozwiązania Sysdig"
description: "Monitorowanie klastra usługi Azure Container Service przy użyciu rozwiązania Sysdig."
services: container-service
author: sauryadas
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: d694744665ef6399560fc12c6976c2d88d232148
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-sysdig"></a>Monitorowanie klastra usługi Azure Container Service przy użyciu rozwiązania Sysdig

W tym artykule wdrożymy agentów rozwiązania Sysdig we wszystkich węzłach agentów w klastrze usługi Azure Container Service. Ta konfiguracja wymaga konta z rozwiązaniem Sysdig. 

## <a name="prerequisites"></a>Wymagania wstępne
[Wdróż](container-service-deployment.md) i [połącz](../container-service-connect.md) klaster skonfigurowany przez usługę Azure Container Service. Przegląd [interfejsu użytkownika platformy Marathon](container-service-mesos-marathon-ui.md). Przejdź do witryny [http://app.sysdigcloud.com](http://app.sysdigcloud.com) i skonfiguruj konto chmury Sysdig. 

## <a name="sysdig"></a>Sysdig
Sysdig jest usługą monitorowania, która umożliwia monitorowanie kontenerów w klastrze. Usługa Sysdig pomaga w rozwiązywaniu problemów, a także oferuje podstawowe metryki monitorowania procesora, sieci, pamięci i operacji we/wy. Usługa Sysdig ułatwia sprawdzanie, które kontenery pracują najciężej lub po prostu używają największej części pamięci i mocy procesora. Widok ten jest dostępny w sekcji przeglądu zapewnianej obecnie w wersji beta. 

![Interfejs użytkownika usługi Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Konfigurowanie wdrożenia usługi Sysdig przy użyciu usługi Marathon
Te kroki przedstawiają metodę konfigurowania i wdrażania aplikacji usługi Sysdig w klastrze przy użyciu usługi Marathon. 

Przejdź do interfejsu użytkownika rozwiązania DC/OS przez stronę [http://localhost:80/](http://localhost:80/). W interfejsie użytkownika rozwiązania DC/OS wybierz pozycję „Universe” (Wszechświat) w lewym dolnym rogu i wyszukaj ciąg „Sysdig”.

![Usługa Sysdig we wszechświecie rozwiązania DC/OS](./media/container-service-monitoring-sysdig/sysdig1.png)

Ukończenie konfiguracji wymaga konta w chmurze Sysdig lub bezpłatnego konta próbnego. Po zalogowaniu w witrynie sieci Web chmury Sysdig kliknij swoją nazwę użytkownika, a na stronie powinien zostać wyświetlony klucz dostępu. 

![Klucz interfejsu API usługi Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

Następnie wprowadź klucz dostępu do konfiguracji usługi Sysdig we wszechświecie DC/OS. 

![Konfiguracja usługi Sysdig we wszechświecie DC/OS](./media/container-service-monitoring-sysdig/sysdig3.png)

Teraz ustaw wystąpienia na 10 000 000, dzięki czemu przy każdym dodaniu nowego węzła do klastra usługa Sysdig automatycznie wdroży agenta w tym nowym węźle. Jest to rozwiązanie tymczasowe, które zapewnia wdrożenie usługi Sysdig we wszystkich nowych agentach w klastrze. 

![Konfiguracja usługi Sysdig w wystąpieniach wszechświata DC/OS](./media/container-service-monitoring-sysdig/sysdig4.png)

Po zainstalowaniu pakietu przejdź z powrotem do interfejsu użytkownika usługi Sysdig, a będziesz mógł zbadać różne metryki użycia dla kontenerów w klastrze. 

Możesz także zainstalować pulpity nawigacyjne określone dla rozwiązań Mesos i Marathon, korzystając z [Kreatora nowego pulpitu nawigacyjnego](https://app.sysdigcloud.com/#/dashboards/new).
