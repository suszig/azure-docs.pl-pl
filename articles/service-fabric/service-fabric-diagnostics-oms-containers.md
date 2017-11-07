---
title: "Monitorowanie kontenerów na sieć szkieletowa usług Azure z usługą OMS dziennika Analytics | Dokumentacja firmy Microsoft"
description: "Użyj analizy dzienników OMS monitorowania kontenery uruchamianych w klastrach sieć szkieletowa usług Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/1/2017
ms.author: dekapur
ms.openlocfilehash: 17121041520160d0d76832bbdbe74ad6a649fdd8
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="monitor-containers-with-oms-log-analytics"></a>Kontenery monitora z OMS analizy dzienników
 
W tym artykule opisano kroki wymagane do skonfigurowania kontenera monitorowania dla klastra. Aby uzyskać więcej informacji o tym, zobacz [monitorowania kontenerów w sieci szkieletowej usług](service-fabric-diagnostics-event-analysis-oms.md#monitoring-containers). Aby wyświetlić to samouczek krok po kroku, można również wykonać [kontenerami Monitor systemu Windows w sieci szkieletowej usług za pomocą pakietu OMS](service-fabric-tutorial-monitoring-wincontainers.md).

## <a name="set-up-the-container-monitoring-solution"></a>Skonfiguruj kontener monitorowania rozwiązania

> [!NOTE]
> Możesz muszą OMS analizy dzienników dla klastra jak również zostały wdrożone na węzły Agent pakietu OMS. Jeśli nie wykonaj kroki opisane w [analizy dzienników OMS konfiguracji](service-fabric-diagnostics-oms-setup.md) i [dodać do klastra Agent pakietu OMS](service-fabric-diagnostics-oms-agent.md) pierwszy.

1. Po skonfigurowaniu klastra z analizy dzienników OMS i Agent pakietu OMS wdrażanie kontenerów. Poczekaj, aż kontenerów do wdrożenia przed przejściem do następnego kroku.

2. W portalu Azure Marketplace, wyszukaj *rozwiązanie monitorowanie kontenera* i wybierz polecenie **rozwiązanie monitorowanie kontenera** zasobów, które zostaną wyświetlone w obszarze monitorowanie i zarządzanie kategorii.

    ![Dodawanie rozwiązania kontenerów](./media/service-fabric-diagnostics-event-analysis-oms/containers-solution.png)

3. Tworzenie rozwiązania wewnątrz tego samego obszaru roboczego, który został już utworzony dla klastra. Ta zmiana automatyczne wyzwolenie procesu agenta, aby rozpocząć gromadzenie danych docker na kontenerów. W około 15 minut lub to powinna zostać wyświetlona światła się z dziennikami przychodzące i statystyka rozwiązania.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o aranżacji kontenera w sieci szkieletowej usług - [sieci szkieletowej usług i kontenerów](service-fabric-containers-overview.md)
* Pobierz zapoznaniu się z [wyszukiwania i badania dziennika](../log-analytics/log-analytics-log-searches.md) funkcje dostępne w ramach analizy dzienników
* Skonfiguruj OMS, aby skonfigurować [automatycznego alerty](../log-analytics/log-analytics-alerts.md) reguły, aby pomóc w wykrywaniu i Diagnostyka