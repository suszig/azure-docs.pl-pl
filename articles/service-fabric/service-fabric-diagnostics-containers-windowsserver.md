---
title: "Usługa Azure kontenery sieci szkieletowej, monitorowania i diagnostyki | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak monitorowanie i diagnozowanie kontenery zorkiestrowana na usługi sieć szkieletowa usług Microsoft Azure z rozwiązaniem kontenery w OMS."
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
ms.date: 10/15/2017
ms.author: dekapur
ms.openlocfilehash: 319ee2c0f7492389bc1767aa2669dd273f8cfa1b
ms.sourcegitcommit: a7c01dbb03870adcb04ca34745ef256414dfc0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="monitoring-windows-server-containers-with-oms"></a>Monitorowanie kontenery systemu Windows Server z usługą OMS

## <a name="oms-containers-solution"></a>Rozwiązanie kontenery OMS

Analiza dzienników Operations Management Suite (OMS) ma rozwiązania kontenerów, który może służyć do monitorowania kontenerów. Obok rozwiązania sieci szkieletowej usług to rozwiązanie to doskonałe narzędzie do monitorowania wdrożenia kontenera zorkiestrowana w sieci szkieletowej usług. Poniżej przedstawiono prosty przykład pulpitu nawigacyjnego w rozwiązaniu wygląda następująco:

![Pulpit nawigacyjny podstawowe OMS](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Zbiera także różne rodzaje dzienniki, które można zbadać za pomocą narzędzia analizy dzienników OMS i może służyć do wizualizacji ani metryki zdarzenia są generowane. Typy dziennika, które są zbierane są:

1. ContainerInventory: zawiera informacje o lokalizacji kontenera, nazwy i obrazów
2. ContainerImageInventory: informacje o wdrożonej obrazów, w tym identyfikatory lub rozmiary
3. ContainerLog: dzienniki błędu, dzienniki docker (stdout itp.) i innych pozycji
4. ContainerServiceLog: docker demon poleceń, które zostały uruchomione
5. O wydajności: liczniki wydajności w tym kontenerze procesora cpu, pamięć, ruch sieciowy na dysku We/Wy i metryki niestandardowe z maszyn hosta

W tym artykule opisano kroki wymagane do skonfigurowania kontenera monitorowania dla klastra. Aby dowiedzieć się więcej o rozwiązaniu kontenery w OMS, zapoznaj się ich [dokumentacji](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. Konfigurowanie klastra sieci szkieletowej usług

Tworzenie klastra przy użyciu szablonu usługi Azure Resource Manager znaleziono [tutaj](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Windows). Upewnij się dodać unikatową nazwę obszaru roboczego OMS. Ten szablon domyślnie wdrażanie klastra w kompilacji w wersji zapoznawczej usługi sieci szkieletowej (v255.255), co oznacza, że nie można używać w środowisku produkcyjnym i nie można uaktualnić do innej wersji platformy Service Fabric. Jeśli zdecydujesz się użyć tego szablonu do długoterminowych lub produkcji korzystać, Zmień wersję numeru wersji stabilnej.

Po skonfigurowaniu klastra upewnij się, że zainstalowano odpowiedni certyfikat, i upewnij się, że jesteś w stanie nawiązać połączenia z klastrem.

Upewnij się, że grupy zasobów jest skonfigurowany poprawnie przez nagłówek [portalu Azure](https://portal.azure.com/) w celu znalezienia wdrożenia. Grupa zasobów powinien zawierać wszystkie zasoby sieci szkieletowej usług, a także mieć rozwiązania analizy dzienników, a także rozwiązania sieci szkieletowej usług.

Do modyfikowania istniejącego klastra sieci szkieletowej usług:
* Potwierdź, że jest włączona diagnostyki (Jeśli nie, włącz je za pomocą [Trwa aktualizacja zestawu skali maszyny wirtualnej](/rest/api/virtualmachinescalesets/create-or-update-a-set))
* Dodaj obszar roboczy OMS przez utworzenie rozwiązania "Analytics sieci szkieletowej usług" za pośrednictwem portalu Azure Marketplace
* Edytuj źródeł danych rozwiązania sieci szkieletowej usług, aby pobrać dane z odpowiednich tabel usługi Azure Storage (Konfigurowanie przez WAD) w grupie zasobów, który znajduje się w klastrze
* Dodanie agenta jako [rozszerzenia zestawu skali maszyny wirtualnej](/powershell/module/azurerm.compute/add-azurermvmssextension) za pomocą programu PowerShell lub aktualizowania maszyny wirtualnej (tego samego łącza jak powyżej, aby zmodyfikować szablon usługi Resource Manager) zestawu skalowania

## <a name="2-deploy-a-container"></a>2. Wdrażanie kontenera

Gdy klaster będzie gotowy i potwierdzeniu, że można do niego dostęp, należy wdrożyć kontener do niego. Jeśli wybrano opcję Użyj wersji zapoznawczej zgodnie z ustawieniami w szablonie, można również zapoznać się usługi sieć szkieletowa nowego rozwiązania docker compose funkcji. Przy tym pamiętać, że kontener jest wdrażany do klastra, po raz pierwszy go może zająć kilka minut pobranie obrazu w zależności od rozmiaru.

## <a name="3-add-the-containers-solution"></a>3. Dodaj rozwiązanie kontenerów

W portalu Azure, utwórz zasób kontenerów (w obszarze monitorowanie i zarządzanie kategorii) za pośrednictwem portalu Azure Marketplace. 

![Dodawanie rozwiązania kontenerów](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

W kroku tworzenia żądania obszarem roboczym pakietu OMS. Wybierz jedną, która została utworzona przy użyciu wdrażania powyżej. Ten krok dodaje rozwiązania kontenery w obszarze roboczym pakietu OMS i jest automatyczne wykrywany przez agenta pakietu OMS wdrożone przez szablon. Agent rozpocznie zbieranie danych o procesach kontenery w klastrze, a w około 10 – 15 minut, powinna zostać wyświetlona światła rozwiązania się z danymi, tak jak obraz pulpitu nawigacyjnego powyżej.

## <a name="4-next-steps"></a>4. Następne kroki

OMS oferuje różnych narzędzi, w obszarze roboczym, aby w przypadku bardziej użyteczna dla Ciebie. Zapoznaj się z następujących opcji, aby dostosować do własnych potrzeb w zakresie:
- Pobierz zapoznaniu się z [wyszukiwania i badania dziennika](../log-analytics/log-analytics-log-searches.md) funkcje dostępne w ramach analizy dzienników
- Konfigurowanie agenta pakietu OMS do pobrania konkretnych licznikach wydajności (Przejdź do strony głównej obszaru roboczego > Ustawienia > danych > liczników wydajności systemu Windows)
- Skonfiguruj OMS, aby skonfigurować [automatycznego alerty](../log-analytics/log-analytics-alerts.md) reguły, aby pomóc w wykrywaniu i Diagnostyka