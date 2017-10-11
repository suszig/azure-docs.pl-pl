---
title: "Automatyczne skalowanie i maszyny wirtualnej skalowanie zestawów | Dokumentacja firmy Microsoft"
description: "Informacje o używaniu diagnostyki i skalowania automatycznego zasobów do automatycznego skalowania maszyn wirtualnych w zestawie skalowania."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: adegeo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06ff9d9ae1dd8256f0d22c1a60ed6a85554f1f17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-automatic-scaling-and-virtual-machine-scale-sets"></a>Jak używać automatyczne skalowanie i zestawy skalowania maszyny wirtualnej
Automatyczne skalowanie maszyn wirtualnych w zestawie skalowania jest utworzenia lub usunięcia maszyn w zestawie, zgodnie z potrzebami, aby dopasować wymagania dotyczące wydajności. Wraz z rozwojem działalności, aplikacja może wymagać dodatkowych zasobów, aby umożliwić skutecznie wykonywania zadań.

Automatyczne skalowanie jest zautomatyzowany proces czy pomaga upraszczają zarządzanie. Przez zmniejszenie nakładów pracy, nie trzeba było stale monitorowania wydajności systemu lub zdecydować, jak zarządzać zasobami. Skalowanie jest procesem elastycznej. Miarę wzrostu obciążenia można dodać więcej zasobów. I jak żądanie zmniejsza, można usunąć zasoby, aby zminimalizować koszty i Obsługa poziomów wydajności.

Skonfiguruj automatyczne skalowanie w skali skonfigurowane przy użyciu szablonu usługi Azure Resource Manager, programu Azure PowerShell, interfejsu wiersza polecenia Azure lub portalu Azure.

## <a name="set-up-scaling-by-using-resource-manager-templates"></a>Ustawianie skalowania przy użyciu szablonów usługi Resource Manager
Zamiast wdrażania i zarządzania nimi każdego zasobu aplikacji oddzielnie, należy użyć szablonu, który wdraża wszystkie zasoby w jednej, skoordynowanej operacji. W szablonie są zdefiniowane zasoby aplikacji i parametrów wdrożenia są określone dla różnych środowisk. Szablon składa się z kodu JSON i wyrażeń, które służy do tworzenia wartości na potrzeby wdrożenia. Aby dowiedzieć się więcej, zobacz [szablonów Authoring Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

W szablonie należy określić pojemność elementu:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

Pojemność określa liczbę maszyn wirtualnych w zestawie. Wydajność można zmienić ręcznie przez wdrożenie szablonu, podając inną wartość. Jeśli wdrażasz szablon można zmienić tylko pojemność może zawierać tylko element SKU o pojemności zaktualizowane.

Pojemność zestawu skalowania można automatycznie dostosowywany przy użyciu kombinacji **autoscaleSettings** zasobu i rozszerzenia diagnostyki.

### <a name="configure-the-azure-diagnostics-extension"></a>Skonfiguruj rozszerzenie diagnostyki Azure
Automatyczne skalowanie jest możliwe tylko jeśli kolekcji metryki zakończy się pomyślnie na każdej maszynie wirtualnej w zestawie skalowania. Rozszerzenia diagnostyki Azure oferuje możliwości monitorowania i diagnostyki, które zaspokoi potrzeby kolekcji metryki automatycznego skalowania zasobu. W ramach szablonu usługi Resource Manager można zainstalować rozszerzenia.

W tym przykładzie przedstawiono zmienne, które są używane w szablonie, aby skonfigurować rozszerzenie diagnostyki:

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

Parametry są podane podczas wdrażania szablonu. W tym przykładzie podano nazwę konta magazynu (w którym są przechowywane dane) i nazwy zestawu skali (w którym dane są zbierane). Również w tym przykładzie systemu Windows Server zbieranych licznika wydajności liczba wątków. Wszystkie liczniki wydajności dostępne w systemie Windows lub Linux może służyć do zbierania informacji diagnostycznych i może być uwzględniony w konfiguracji rozszerzenia.

W tym przykładzie przedstawiono definicję rozszerzenia w szablonie:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Insights.VMDiagnosticsSettings",
      "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
          "storageAccount": "[variables('diagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
          "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
          "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
          "storageAccountEndPoint": "https://core.windows.net"
        }
      }
    }
  ]
}
```

Po uruchomieniu rozszerzenia diagnostyki, dane są przechowywane i zbierane w tabeli, w ramach konta magazynu, który określisz. W **WADPerformanceCounters** tabeli, możesz znaleźć zebranych danych:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a>Konfigurowanie zasobów autoScaleSettings
Zasób autoscaleSettings używa tych informacji z rozszerzenia diagnostyki umożliwia określenie, czy można zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w zestawie skalowania.

W tym przykładzie pokazano konfigurację zasobu w szablonie:

```json
{
  "type": "Microsoft.Insights/autoscaleSettings",
  "apiVersion": "2015-04-01",
  "name": "[concat(parameters('resourcePrefix'),'as1')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
  ],
  "properties": {
    "enabled": true,
    "name": "[concat(parameters('resourcePrefix'),'as1')]",
    "profiles": [
      {
        "name": "Profile1",
        "capacity": {
          "minimum": "1",
          "maximum": "10",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 650
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 550
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ],
    "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
  }
}
```

W powyższym przykładzie dwie reguły są tworzone dla Definiowanie automatycznych akcji skalowania. Pierwsza reguła definiuje akcji skalowania w poziomie, a drugą regułę definiuje akcji skalowania w. Te wartości są dostępne w zasadach:

| Reguła | Opis |
| ---- | ----------- |
| metricName        | Ta wartość jest taka sama jak licznika wydajności, które zdefiniowano w zmiennej wadperfcounter rozszerzenia diagnostyki. W powyższym przykładzie wątku licznik jest używany.    |
| metricResourceUri | Ta wartość jest identyfikator zasobu zestawu skali maszyny wirtualnej. Ten identyfikator zawiera nazwę grupy zasobów, nazwę dostawcy zasobów i nazwę zestaw skalowania skalowania. |
| Ziarnem czasu         | Ta wartość jest stopień szczegółowości metryki, które są zbierane. W powyższym przykładzie dane są zbierane w odstępach jednej minuty. Ta wartość jest używana z timeWindow. |
| Statystyka         | Ta wartość określa, jak metryki połączone pomieścić automatycznych akcji skalowania. Możliwe wartości to: średnia, Min, Max. |
| timeWindow        | Ta wartość jest przedział czasu, w którym są zbierane dane wystąpienia. Musi być od 5 minut do 12 godzin. |
| timeAggregation   | Ta wartość określa, jak powinny być połączone dane, które są zbierane wraz z upływem czasu. Wartość domyślna to średnia. Możliwe wartości to: średnia, co najmniej, maksimum, ostatnich, łączna liczba, liczba. |
| Operator          | Ta wartość jest operator, który służy do porównywania danych metryki i wartość progową. Możliwe wartości to: równa, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual. |
| Próg         | Ta wartość jest wartością wyzwala akcji skalowania. Pamiętaj zapewnić wystarczającą różnicę wartości progowych dla **skalowalnego w poziomie** i **w skali** akcje. Jeśli ustawisz takie same wartości dla obu akcje systemu oszacowano stałej zmiany, które uniemożliwiają wdrożenie akcji skalowania. Na przykład ustawienie zarówno do 600 wątków w poprzednim przykładzie nie działa. |
| Kierunek         | Ta wartość Określa akcję wykonywaną, gdy uzyskuje się wartość progową. Możliwe wartości to zwiększyć lub zmniejszyć. |
| type              | Ta wartość jest typu akcji, która powinna się odbyć i musi mieć ustawioną ChangeCount. |
| wartość             | Ta wartość jest liczba maszyn wirtualnych, które zostały dodane do lub usunięte z zestawu skalowania. Ta wartość musi wynosić 1 lub większą. |
| cooldown          | Ta wartość jest czas oczekiwania od momentu ostatniej akcji skalowania, zanim nastąpi następnej akcji. Ta wartość musi należeć do zakresu od minutę i jeden tydzień. |

W zależności od licznika wydajności są używane, niektórych elementów w konfiguracji szablonu są używane w inny sposób. W powyższym przykładzie licznika wydajności to liczba wątków, wartość progowa jest 650 dla akcji skalowania w poziomie, a wartość progowa jest 550 dla akcji skalowania w. Użycie licznika, takie jak czas procesora (%), wartość progowa jest równa wartości procentowej użycia procesora CPU, która określa akcji skalowania.

Po wyzwoleniu akcji skalowania, takich jak wysokie obciążenie, zwiększa się na podstawie wartości w szablonie pojemność zestawu. Na przykład w skali zestawu, którego pojemność ustawiono 3 i wartość akcji skalowania jest równa 1:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

Gdy bieżącego obciążenia powoduje, że liczba wątków średni przejść powyżej wartości progowej 650:

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

A **skalowalnego w poziomie** wyzwoleniu powodujący stanie można zwiększyć o jeden zestaw akcji:

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

Wynik jest, że maszyna wirtualna została dodana do zestawu skalowania:

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

Po upływie cooldown pięć minut Jeśli średnia liczba wątków na maszynach pozostaje ponad 600 innej maszyny jest dodane do zestawu. Jeśli liczba wątków średni pozostaje poniżej 550, pojemność zestawu skalowania, zostanie zmniejszona jedną i maszynie jest usuwane z zestawu.

## <a name="set-up-scaling-using-azure-powershell"></a>Ustawianie skalowania przy użyciu programu Azure PowerShell

Aby wyświetlić przykłady przy użyciu programu PowerShell, aby skonfigurować funkcję skalowania automatycznego, obejrzyj [Azure PowerShell Monitor szybki start przykłady](../monitoring-and-diagnostics/insights-powershell-samples.md).

## <a name="set-up-scaling-using-azure-cli"></a>Ustawianie skalowania przy użyciu wiersza polecenia platformy Azure

Aby wyświetlić przykłady przy użyciu wiersza polecenia platformy Azure, aby skonfigurować funkcję skalowania automatycznego, obejrzyj [Azure Monitor i platform interfejsu wiersza polecenia Szybki start przykłady](../monitoring-and-diagnostics/insights-cli-samples.md).

## <a name="set-up-scaling-using-the-azure-portal"></a>Ustawianie skalowania przy użyciu portalu Azure

Aby zapoznać się z przykładem przy użyciu portalu Azure, aby skonfigurować funkcję skalowania automatycznego, obejrzyj [tworzenia zestawu skalowania maszyn wirtualnych przy użyciu portalu Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="investigate-scaling-actions"></a>Zbadaj akcji skalowania

* **Witryna Azure Portal**  
Obecnie można uzyskać ograniczone informacje za pomocą portalu.

* **Eksplorator zasobów Azure**  
To narzędzie jest najlepsze dla eksploracji bieżącego stanu sieci zestawu skali. Tej ścieżki i powinien zostać wyświetlony widok wystąpienia zestawu skali utworzony:  
**Subskrypcje > {subskrypcji} > resourceGroups > {grupie zasobów} > dostawców > Microsoft.Compute > virtualMachineScaleSets > {zestawie skali} > maszyn wirtualnych**

* **Azure PowerShell**  
Użyj tego polecenia, aby uzyskać pewne informacje:

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* Połączenie z maszyną wirtualną jumpbox podobnie jak inne maszyny, a następnie zdalny dostęp maszyny wirtualne w skali ustawioną monitorowania poszczególnych procesów.

## <a name="next-steps"></a>Następne kroki
* Przyjrzyj się [automatycznie skalować maszyny w zestawie skalowania maszyn wirtualnych](virtual-machine-scale-sets-windows-autoscale.md) Aby wyświetlić przykład sposobu tworzenia skali ustawiony za pomocą automatyczne skalowanie skonfigurowane.

* Znajdź przykłady Azure Monitor funkcje w [Azure PowerShell Monitor szybki start — przykłady](../monitoring-and-diagnostics/insights-powershell-samples.md)

* Dowiedz się więcej o funkcji powiadomień w [użyć akcji skalowania automatycznego do wysyłania wiadomości e-mail i elementu webhook powiadomień o alertach w monitorze Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).

* Więcej informacji na temat sposobu [dzienników inspekcji używany do wysyłania wiadomości e-mail i elementu webhook powiadomień o alertach w monitorze Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)

* Dowiedz się więcej o [zaawansowanych scenariuszy skalowania automatycznego](virtual-machine-scale-sets-advanced-autoscale.md).
