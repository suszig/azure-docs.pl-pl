---
title: Skalowanie w pionie zestawy skalowania maszyny wirtualnej platformy Azure | Dokumentacja firmy Microsoft
description: "Sposób pionowo skalowania maszyny wirtualnej w odpowiedzi na monitorowanie alertów w usłudze Automatyzacja Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: madhana
editor: 
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: guybo
ms.openlocfilehash: 9159a5a9041864fe06785829121233379c46bb03
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Pionowy automatycznie skalowana za pomocą zestawów skali maszyny wirtualnej
W tym artykule opisano sposób skalowanie w pionie Azure [zestawy skalowania maszyny wirtualnej](https://azure.microsoft.com/services/virtual-machine-scale-sets/) z lub bez reprovisioning. Pionowy skalowania maszyn wirtualnych, które nie znajdują się w zestawy skalowania można znaleźć w temacie [skalowanie w pionie maszyny wirtualnej platformy Azure w usłudze Automatyzacja Azure](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Skalować w pionie, znanej także jako *skalowanie w górę* i *dół*, oznacza zwiększenie lub zmniejszenie rozmiarów maszyn wirtualnych (VM) w odpowiedzi na obciążenie. Porównaj te z [skalowanie w poziomie](virtual-machine-scale-sets-autoscale-overview.md), nazywany również *skalowanie w poziomie* i *skalować w*, gdzie liczby maszyn wirtualnych jest zmianie w zależności od obciążenia.

Reprovisioning oznacza usunięcie istniejącej maszyny Wirtualnej i zastąpienie go innym. Zwiększyć lub zmniejszyć rozmiar maszyn wirtualnych w zestawie skalowania maszyn wirtualnych, w niektórych przypadkach chcesz zmienić rozmiar istniejących maszyn wirtualnych i Zachowaj dane, a w innych przypadkach należy wdrożyć nowe maszyny wirtualne o nowy rozmiar. W tym dokumencie opisano w obu przypadkach.

Skalowanie w pionie mogą być przydatne podczas:

* Usługa oparta na maszynach wirtualnych jest wykorzystywane w obszarze (na przykład w weekendy). Zmniejszenie rozmiaru maszyny Wirtualnej można zmniejszyć koszty miesięcznych.
* Zwiększanie rozmiaru maszyny Wirtualnej, aby poradzić sobie z większą żądanie bez tworzenia dodatkowych maszyn wirtualnych.

Można skonfigurować skalowanie w pionie być wyzwalane na podstawie alertów według metryki z zestawu skali maszyny Wirtualnej. Gdy alert jest aktywny go generowane elementu webhook tego wyzwalaczy, ustawione przez element runbook, które mogą być skalowane na skalę w górę lub w dół. Skalowanie w pionie można skonfigurować, wykonaj następujące czynności:

1. Utwórz konto usługi Automatyzacja Azure z funkcji Uruchom jako.
2. Importowanie elementów runbook skali pionowej automatyzacji Azure dla zestawy skalowania maszyny Wirtualnej w ramach subskrypcji.
3. Dodawanie elementu webhook do elementu runbook.
4. Dodaj alert do sieci zestawu skali maszyny Wirtualnej za pomocą powiadomień elementu webhook.

> [!NOTE]
> Skalowanie w pionie automatyczne może nastąpić w określonym zakresie rozmiarów maszyn wirtualnych. Porównanie specyfikacji każdego rozmiaru przed podjęciem decyzji o skali: od jednej do innego (większą liczbę nie zawsze wskazuje większy rozmiar maszyny Wirtualnej). Możesz skalować między parami następujących rozmiarów:
> 
> | Rozmiary maszyn wirtualnych, skalowanie pary |  |
> | --- | --- |
> | Standardowa_A0 |Standardowa_A11 |
> | Standardowa_D1 |Standardowa_D14 |
> | Standardowa_DS1 |Standardowa_DS14 |
> | Standard_D1v2 |Standard_D15v2 |
> | Standardowa_G1 |Standard_G5 |
> | Standardowa_GS1 |Standard_GS5 |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Utwórz konto usługi Automatyzacja Azure z funkcji Uruchom jako
W pierwszej kolejności należy wykonać to utworzyć konto usługi Automatyzacja Azure, które będą obsługiwać elementów runbook, używana do skalowania wystąpienia zestawu skali maszyny Wirtualnej. Ostatnio [usługi Automatyzacja Azure](https://azure.microsoft.com/services/automation/) wprowadzono funkcję "Konto Uruchom jako", co sprawia, że ustawienia zapasowej główną usługi dla automatycznie uruchomione elementy runbook w imieniu użytkownika bardzo proste. Możesz przeczytać więcej na temat tego artykułu poniżej:

* [Uwierzytelnianie elementów Runbook przy użyciu konta Uruchom jako platformy Azure](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Importowanie elementów runbook skali pionowej automatyzacji Azure w ramach subskrypcji
Elementy runbook, konieczne jest skalowanie w pionie z zestawy skalowania maszyny Wirtualnej zostały już opublikowane w galerii elementu Runbook automatyzacji Azure. Aby zaimportować je do subskrypcji wykonaj kroki opisane w tym artykule:

* [Galeria elementów Runbook i modułów dla usługi Automatyzacja Azure](../automation/automation-runbook-gallery.md)

Wybierz opcję Galerii przeglądania z menu elementów Runbook:

![Elementy Runbook do zaimportowania][runbooks]

Wyświetlane są elementy runbook, które muszą zostać zaimportowane. Wybierz element runbook, czy ma pionowego, skalowania z lub bez reprovisioning — na podstawie:

![Galeria elementów Runbook][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Dodawanie elementu webhook z elementem runbook
Po zaimportowaniu elementów runbook, które będą potrzebne, aby dodać elementu webhook do elementu runbook, mogą być wyzwalane przez alert z zestawu skali maszyny Wirtualnej. W tym artykule opisano szczegółów tworzenia elementu webhook dla elementu Runbook:

* [Azure automatyzacji elementów webhook](../automation/automation-webhooks.md)

> [!NOTE]
> Upewnij się, że należy skopiować identyfikator URI elementu webhook przed zamknięciem okna dialogowego elementu webhook, ponieważ będzie on potrzebny w następnej sekcji.
> 
> 

## <a name="add-an-alert-to-your-vm-scale-set"></a>Dodaj alert do zestawu skali maszyny Wirtualnej
Poniżej znajduje się skrypt programu PowerShell, który pokazuje, jak dodać alert do zestawu skali maszyny Wirtualnej. Zapoznaj się z artykułem następujące nazwy metryki wyzwalać alert na: [metryki wspólnej Skalowanie automatyczne Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> Zalecane jest, aby skonfigurować okno czasu alertu, aby uniknąć wyzwalania skalowanie w pionie i wszystkie skojarzone przerw w obsłudze, zbyt często. Należy wziąć pod uwagę okna o co najmniej 20-30 minut lub dłużej. Należy wziąć pod uwagę skalowanie, aby uniknąć przerw w poziomie.
> 
> 

Aby uzyskać więcej informacji na temat tworzenia alertów można znaleźć w następujących artykułach:

* [Przykładów dla platformy Azure Monitor PowerShell szybki start](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Przykładów dla platformy Azure CLI wieloplatformowych Monitor szybki start](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Podsumowanie
W tym artykule pokazano proste przykłady skalowania pionowej. Te bloki konstrukcyjne — konto automatyzacji, elementy runbook, elementów webhook, alerty — może się łączyć szeroki zakres zdarzeń dostosowany zbiór akcji.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
