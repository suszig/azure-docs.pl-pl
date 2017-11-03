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
ms.date: 05/22/2017
ms.author: cristyg
ms.openlocfilehash: 2090d4c951e2261529bf1b7b361510d5822060a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-container-registry-repositories"></a>Repozytoria rejestru kontenera platformy Azure

Rejestry organizacji IANA kontenera platformy Azure są zgodne z wieloma usługami i orchestrators. Aby ułatwić śledzenie źródła usługi i agentów, na których jest używane ACR, możemy uruchomiony przy użyciu rozwiązania Docker pole nagłówka w pliku Docker.config.



## <a name="viewing-repositories-in-the-portal"></a>Wyświetlanie repozytoria w portalu

Nagłówki ACR wykonaj format:
```
X-Meta-Source-Client: <cloud>/<service>/<optionalservicename>
```

* Chmury: Azure, Azure stosu, lub inne dla instytucji rządowych lub chmury Azure określonego kraju. Mimo że stosu Azure i dla instytucji rządowych chmury nie są obecnie obsługiwane, ten parametr umożliwia wsparcia w przyszłości.
* Usługi: Nazwa usługi.
* Optionalservicename: opcjonalny parametr dla usług z subservices lub Określ jednostki SKU (przykład: aplikacje sieci web odpowiada/app-service/aplikacje sieci web Azure).

Usług partnera i orchestrators zaleca się użycie wartości określonego nagłówka ułatwiające pracę z naszych danych telemetrycznych. Użytkownicy mogą również modyfikować wartości przekazane do nagłówka, jeśli potrzebna jest tak.

Wartości chcemy partnerów ACR na potrzeby wypełnić pole "X-Meta-źródła-Client" są poniżej:

| Nazwa usługi              | Nagłówek                                |
| ------------------------- | ------------------------------------- |
| Azure Container Service   | / compute/azure usługi kontenera platformy Azure — |
| Usługi aplikacji — aplikacje sieci Web    | / app-service/aplikacje sieci web Azure            |
| Usługi aplikacji — aplikacje logiki  | Azure/app-service / — aplikacje logiki          |
| Batch                     | / compute/partii zadań Azure                   |
| Cloud Console             | Azure/chmurze konsoli                   |
| Funkcje                 | Azure/obliczeniowych               |
| Internet rzeczy - Centrum  | Centrum Azure/iot                         |
| HDInsight                 | hdinsight-Azure/danych                  |
| Jenkins                   | Azure/wpięć                         |
| Usługa Machine Learning          | Azure/data/machile-learning           |
| Service Fabric            | / compute /-sieć szkieletowa usług Azure          |
| VSTS                      | Azure/vsts                            |


## <a name="next-steps"></a>Następne kroki
[Dowiedz się więcej o rejestrów i obsługiwanych usług i orchestrators](container-registry-intro.md)
