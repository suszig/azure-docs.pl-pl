---
title: "Zarządzanie Azure DDoS ochrony standardowe przy użyciu programu Azure PowerShell | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać Azure DDoS ochrony standardowe przy użyciu programu Azure PowerShell."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2017
ms.author: jdial
ms.openlocfilehash: baac97db61b84000557e7150a64ffb64d81ce00c
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Zarządzanie Azure DDoS ochrony standardowe przy użyciu programu Azure PowerShell

Dowiedz się, jak włączyć i wyłączyć atakach typu protection service (DDoS) i użyj telemetrii, aby ograniczyć takiego ataku z standardowe ochrony przed atakami DDoS Azure. Standardowa ochrony przed atakami DDoS chroni zasobów platformy Azure, takich jak maszyny wirtualne, usługi równoważenia obciążenia i bram aplikacji, które mają Azure [publicznego adresu IP](virtual-network-public-ip-address.md) przypisane do niej. Aby dowiedzieć się więcej na temat ochrony przed atakami DDoS — standardowe i jego możliwości, zobacz [standardowe ochrony przed atakami DDoS — omówienie](ddos-protection-overview.md). 

>[!IMPORTANT]
>Azure DDoS ochrony Standard (Ochrona przed atakami DDoS) jest obecnie w wersji zapoznawczej. Ochrona przed atakami DDoS obsługują ograniczoną liczbę zasobów platformy Azure i jest dostępna tylko w niektórych regionach. Aby uzyskać listę dostępnych regionów, zobacz [standardowe ochrony przed atakami DDoS — omówienie](ddos-protection-overview.md). Musisz [zarejestrować usługi](http://aka.ms/ddosprotection) ograniczone wersji zapoznawczej można pobrać ochrona przed atakami DDoS włączona dla Twojej subskrypcji. Po zarejestrowaniu, otrzymasz wiadomość przez zespół Azure DDoS, który przeprowadzi Cię przez proces aktywacji.


## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie. Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Jeśli musisz zainstalować lub uaktualnić programu Azure PowerShell, zobacz [modułu instalacji programu Azure PowerShell](/powershell/azure/install-azurerm-ps).

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection-standard---new-virtual-network"></a>Włącz Standard ochrony przed atakami DDoS — nowej sieci wirtualnej

Aby utworzyć sieć wirtualną z włączoną ochroną przed atakami DDoS, uruchom poniższy przykład:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -EnableDDoSProtection
```

W tym przykładzie tworzy sieć wirtualną z dwiema podsieciami i dwa serwery DNS. Określanie serwerów DNS w sieci wirtualnej powoduje, że karty sieciowe/maszyn wirtualnych wdrożonych w tej sieci wirtualnej dziedziczą tych serwerów DNS jako domyślne. Ochrona przed atakami DDoS jest włączona dla wszystkich chronionych zasobów w sieci wirtualnej.

> [!WARNING]
> Po wybraniu regionu, wybierz region obsługiwanych z listy w [standardowe ochrony przed atakami DDoS Azure — omówienie](ddos-protection-overview.md).

## <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Włącz ochronę przed atakami DDoS na istniejącej sieci wirtualnej

Aby włączyć ochronę przed atakami DDoS na istniejącej sieci wirtualnej, należy uruchomić poniższy przykład:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks -Force
```

> [!WARNING]
> Sieć wirtualna musi istnieć w obsługiwanym regionie. Aby uzyskać listę obsługiwanych regionów, zobacz [standardowe ochrony przed atakami DDoS Azure — omówienie](ddos-protection-overview.md).

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Wyłącz ochronę przed atakami DDoS w sieci wirtualnej

Aby wyłączyć ochronę przed atakami DDoS w sieci wirtualnej, należy uruchomić poniższy przykład:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks" -Force
```

## <a name="review-the-ddos-protection-status-of-a-virtual-network"></a>Informacje dotyczące stanu ochrony przed atakami DDoS sieci wirtualnej 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>Użyj telemetrii ochrona przed atakami DDoS

Dane telemetryczne dotyczące ataku jest zapewniana za pomocą monitora Azure w czasie rzeczywistym. Dane telemetryczne jest dostępna tylko na czas trwania, która publicznego adresu IP podlega środki zaradcze. Dane telemetryczne nie będą widzieć przed lub po skuteczność została osłabiona atak.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>Skonfiguruj alerty na metryki ochrona przed atakami DDoS

Wykorzystanie konfiguracji alertu Azure Monitor, można wybrać dowolną z dostępnymi metrykami ochrona przed atakami DDoS alertu, jeśli istnieje aktywne środki zaradcze podczas ataku.

#### <a name="configure-email-alert-rules-via-azure"></a>Konfigurowanie reguł alertów e-mail za pomocą usługi Azure

1. Pobranie listy subskrypcji posiadanego. Sprawdź, użytkownik pracuje z prawej subskrypcji. Jeśli nie ustawiono właściwy przy użyciu dane wyjściowe z Get-AzureRmSubscription. 

    ```powershell
    Get-AzureRmSubscription 
    Get-AzureRmContext 
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```

2. Aby wyświetlić listę istniejących reguł dla grupy zasobów, użyj następującego polecenia: 

    ```powershell
    Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
    ```

3. Aby utworzyć regułę, należy najpierw mają następujące informacje: 

    - Identyfikator zasobu dla zasobu, aby ustawić alert dla.
    - Definicje metryk dostępnych dla tego zasobu. Jednym ze sposobów Pobierz identyfikator zasobu jest korzystanie z portalu Azure. Zakładając, że zasób został już utworzony, wybierz go w portalu Azure. Na następnej stronie zaznacz *właściwości* w obszarze *ustawienia* sekcji. **Identyfikator ZASOBU** to pole na następnej stronie. Innym sposobem jest użycie [Eksploratora zasobów Azure](https://resources.azure.com/). Przykładowy identyfikator zasobu publicznego adresu IP to:`/subscriptions/<Id>/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`

    Poniższy przykład tworzy alert dla publicznego adresu IP skojarzonego z zasobów w sieci wirtualnej. Metryka ma być tworzony alert na **DDoS w ramach ataku lub nie**. Jest to wartość logiczna, 1 lub 0. A **1** oznacza podlegają ataku. A **0** oznacza, że nie jesteś zaatakowane. Alert jest tworzony podczas atak uruchamiana w ciągu ostatnich 5 minut.

    Aby utworzyć elementu webhook lub Wyślij wiadomość e-mail, gdy tworzony jest alert, należy najpierw utworzyć poczty e-mail i/lub elementu webhook. Po utworzeniu wiadomości e-mail lub elementu webhook, od razu utworzyć reguły z `-Actions` tagów, jak pokazano w poniższym przykładzie. Nie można skojarzyć elementu webhook lub wiadomości e-mail przy użyciu istniejących reguł przy użyciu programu PowerShell.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/<Id>/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail -Description "Under DDoS Attack"
    ```

4. Sprawdź, czy alert poprawnie utworzony przez regułę:

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    ```

Możesz także dowiedzieć się więcej [konfigurowaniu elementów webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md?toc=%2fazure%2fvirtual-network%2ftoc.json) i [aplikacje logiki](../logic-apps/logic-apps-what-are-logic-apps.md) do tworzenia alertów.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfiguruj rejestrowanie metryki ochrona przed atakami DDoS

Zapoznaj się [PowerShell szybki start przykłady](../monitoring-and-diagnostics/insights-powershell-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json) ułatwiają dostęp i konfigurowanie usługi Azure diagnostyczne za pośrednictwem programu PowerShell.

## <a name="next-steps"></a>Następne kroki

- [Więcej informacji na temat dzienników diagnostycznych platformy Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Analizowanie dzienników z usługi Azure storage z analizy dzienników](../log-analytics/log-analytics-azure-storage.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Rozpoczynanie pracy z usługą Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md?toc=%2fazure%2fvirtual-network%2ftoc.json)