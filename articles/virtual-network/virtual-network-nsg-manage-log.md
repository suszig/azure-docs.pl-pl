---
title: "Monitorowanie działań, zdarzenia i liczniki dla grup NSG | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak włączyć operacyjne rejestrowanie dla grup NSG, zdarzenia i liczniki"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 2e699078-043f-48bd-8aa8-b011a32d98ca
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/31/2017
ms.author: jdial
ms.openlocfilehash: 6beb9ae1b64e27df0a4eefefd592c7850efc7d2d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="log-analytics-for-network-security-groups-nsgs"></a>Usługa Log Analytics dla sieciowych grup zabezpieczeń

Grupy NSG można włączyć następujące kategorie dzienników diagnostycznych:

* **Zdarzenie:** zawiera wpisy, dla których NSG reguły są stosowane do maszyn wirtualnych i wystąpień ról na podstawie adresu MAC. Stan zasady te są gromadzone co 60 sekund.
* **Licznik reguł:** reguł zawiera wpisy dla ile razy każda grupa NSG jest stosowana do odmowy lub zezwolić na ruch.

> [!NOTE]
> Dzienniki diagnostyczne są dostępne tylko dla grup NSG wdrożone za pośrednictwem modelu wdrażania usługi Azure Resource Manager. Nie można włączyć rejestrowania diagnostycznego w celu grup NSG wdrożone za pośrednictwem klasycznego modelu wdrażania. W celu lepszego zrozumienia dwóch modeli, odwołanie [modele wdrażania Azure opis](../resource-manager-deployment-model.md) artykułu.

Rejestrowanie aktywności (wcześniej znane jako inspekcji lub operacyjne dzienniki) jest domyślnie włączone dla grup NSG, został utworzony za pomocą obu modelu wdrożenia usługi Azure. Aby ustalić, jakie operacje zostały zakończone na grup NSG w dzienniku aktywności, Wyszukaj wpisy zawierające następujących zasobów: 

- Microsoft.ClassicNetwork/networkSecurityGroups 
- Microsoft.ClassicNetwork/networkSecurityGroups/securityRules
- Microsoft.Network/networkSecurityGroups
- Microsoft.Network/networkSecurityGroups/securityRules 

Odczyt [Omówienie dziennika aktywności platformy Azure](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) artykuł, aby dowiedzieć się więcej o Dzienniki aktywności. 

## <a name="enable-diagnostic-logging"></a>Włączanie rejestrowania diagnostycznego

Należy włączyć rejestrowanie diagnostyczne *każdego* NSG, które mają być zbierane dane. [Omówienie programu Azure dzienników diagnostycznych](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) wyjaśniono wysyłania dzienników diagnostycznych. Jeśli nie masz istniejącej grupy NSG, wykonaj kroki [Utwórz grupę zabezpieczeń sieci](virtual-networks-create-nsg-arm-pportal.md) artykuł, aby go utworzyć. Można włączyć NSG diagnostycznych rejestrowanie przy użyciu dowolnej z następujących metod:

### <a name="azure-portal"></a>Azure Portal

Aby włączyć rejestrowanie, zaloguj się za pomocą portalu [portal](https://portal.azure.com). Kliknij przycisk **wszystkie usługi**, wpisz *sieciowej grupy zabezpieczeń*. Wybierz grupy NSG, aby włączyć rejestrowanie. Postępuj zgodnie z instrukcjami dla zasobów obliczeniowych z systemem innym niż w [Włączanie dzienników diagnostycznych w portalu](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artykułu. Wybierz **NetworkSecurityGroupEvent**, **NetworkSecurityGroupRuleCounter**, lub oba rodzaje dzienników.

### <a name="powershell"></a>PowerShell

Aby włączyć rejestrowanie za pomocą programu PowerShell, postępuj zgodnie z instrukcjami [Włączanie dzienników diagnostycznych za pomocą programu PowerShell](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artykułu. Oceny przed wprowadzeniem polecenia z tego artykułu następujące informacje:

- Można określić wartość dla `-ResourceId` parametru zastępując następujące [tekst], odpowiednio, następnie wpisując polecenie `Get-AzureRmNetworkSecurityGroup -Name [nsg-name] -ResourceGroupName [resource-group-name]`. Identyfikator dane wyjściowe polecenia wygląda podobnie do */subscriptions/ [Nazwa subskrypcji Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.
- Jeśli chcesz zbierać dane z kategorii dziennika Dodaj `-Categories [category]` na końcu polecenia w artykule, w którym kategorii jest *NetworkSecurityGroupEvent* lub *NetworkSecurityGroupRuleCounter*. Jeśli nie używasz `-Categories` parametru, zbierania danych jest włączona dla dziennika kategorii.

### <a name="azure-command-line-interface-cli"></a>Azure interfejsu wiersza polecenia (CLI)

Aby korzystać z interfejsu wiersza polecenia, aby włączyć rejestrowanie, postępuj zgodnie z instrukcjami [Włączanie dzienników diagnostycznych za pomocą interfejsu wiersza polecenia](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-resource-diagnostic-logs) artykułu. Oceny przed wprowadzeniem polecenia z tego artykułu następujące informacje:

- Można określić wartość dla `-ResourceId` parametru zastępując następujące [tekst], odpowiednio, następnie wpisując polecenie `azure network nsg show [resource-group-name] [nsg-name]`. Identyfikator dane wyjściowe polecenia wygląda podobnie do */subscriptions/ [Nazwa subskrypcji Id]/resourceGroups/[resource-group]/providers/Microsoft.Network/networkSecurityGroups/[NSG]*.
- Jeśli chcesz zbierać dane z kategorii dziennika Dodaj `-Categories [category]` na końcu polecenia w artykule, w którym kategorii jest *NetworkSecurityGroupEvent* lub *NetworkSecurityGroupRuleCounter*. Jeśli nie używasz `-Categories` parametru, zbierania danych jest włączona dla dziennika kategorii.

## <a name="logged-data"></a>Zarejestrowane dane

Dane w formacie JSON jest przeznaczony dla obu dzienników. Określone dane, które są przeznaczone dla każdego typu dziennika znajduje się w następujących sekcjach:

### <a name="event-log"></a>Dziennik zdarzeń
Ten dziennik zawiera informacje o NSG, które zasady są stosowane do maszyn wirtualnych i wystąpień roli usługi, na podstawie adresu MAC w chmurze. Rejestrowane są następujące dane przykładowe dla każdego zdarzenia:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupEvent",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION-ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupEvents",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-B791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "priority":1000,
        "type":"allow",
        "conditions":{
            "protocols":"6",
            "destinationPortRange":"3389-3389",
            "sourcePortRange":"0-65535",
            "sourceIP":"0.0.0.0/0",
            "destinationIP":"0.0.0.0/0"
            }
        }
}
```

### <a name="rule-counter-log"></a>Zasada dziennika liczników

Ten dziennik zawiera informacje o poszczególnych regułach stosowane do zasobów. Następujące przykładowe dane jest rejestrowane za każdym razem, których dotyczy reguła:

```json
{
    "time": "[DATE-TIME]",
    "systemId": "007d0441-5d6b-41f6-8bfd-930db640ec03",
    "category": "NetworkSecurityGroupRuleCounter",
    "resourceId": "/SUBSCRIPTIONS/[SUBSCRIPTION ID]/RESOURCEGROUPS/[RESOURCE-GROUP-NAME]TESTRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/[NSG-NAME]",
    "operationName": "NetworkSecurityGroupCounters",
    "properties": {
        "vnetResourceGuid":"{5E8AEC16-C728-441F-B0CA-791E1DBC2F4}",
        "subnetPrefix":"192.168.1.0/24",
        "macAddress":"00-0D-3A-92-6A-7C",
        "primaryIPv4Address":"192.168.1.4",
        "ruleName":"UserRule_default-allow-rdp",
        "direction":"In",
        "type":"allow",
        "matchedConnections":125
        }
}
```

## <a name="view-and-analyze-logs"></a>Wyświetlanie i analizowanie dzienników

Aby dowiedzieć się, jak wyświetlać dane dziennika aktywności, przeczytaj [Omówienie dziennika aktywności platformy Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artykułu. Aby dowiedzieć się jak wyświetlać dane dziennika diagnostycznego, przeczytaj [Omówienie programu Azure dzienników diagnostycznych](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) artykułu. Po wysłaniu danych diagnostycznych do analizy dzienników można użyć [analytics sieciowej grupy zabezpieczeń usługi Azure](../log-analytics/log-analytics-azure-networking-analytics.md) rozszerzone szczegółowe informacje o rozwiązania do zarządzania (wersja zapoznawcza). 
