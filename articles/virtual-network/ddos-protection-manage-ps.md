---
title: "Zarządzanie Azure DDoS ochrony standardowe przy użyciu programu Azure PowerShell | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać Azure DDoS ochrony standardowe przy użyciu programu Azure PowerShell."
services: virtual-network
documentationcenter: na
author: kumudD
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: kumud
ms.openlocfilehash: a1a3688d4ff215d05d2f78cdfa7d402e3fc20be2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-ddos-protection-standard-using-azure-powershell"></a>Zarządzanie Azure DDoS ochrony standardowe przy użyciu programu Azure PowerShell

>[!IMPORTANT]
>Azure DDoS ochrony Standard (Ochrona przed atakami DDoS) jest obecnie w wersji zapoznawczej. Ograniczoną liczbą zasobów platformy Azure, obsługa ochrona przed atakami DDoS, a w niektórych regionów. Musisz [zarejestrować usługi](http://aka.ms/ddosprotection) ograniczone wersji zapoznawczej można pobrać ochrona przed atakami DDoS włączona dla Twojej subskrypcji. Kontaktować się ze przez zespół Azure DDoS rejestracji do przeprowadzenia procesu aktywacji. Ochrona przed atakami DDoS jest dostępna w regionach nam wschodnie, zachodnie nam i zachodnie środkowe Stany. Podczas udostępniania wersji zapoznawczej są nie naliczane opłaty za korzystanie z usługi.

W tym artykule przedstawiono sposób Włącz ochronę przed atakami DDoS, Wyłącz ochronę przed atakami DDoS i użyj telemetrii, aby ograniczyć atak przy użyciu programu Azure PowerShell.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). Jeśli musisz zainstalować lub uaktualnić programu Azure PowerShell, zobacz [modułu instalacji programu Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Login-AzureRmAccount
```

## <a name="enable-ddos-protection"></a>Włącz ochronę przed atakami DDoS

### <a name="create-a-new-virtual-network-and-enable-ddos-protection"></a>Utwórz nową sieć wirtualną, a następnie włącz ochronę przed atakami DDoS

Aby utworzyć sieć wirtualną z włączoną ochroną przed atakami DDoS, uruchom poniższy przykład:

```powershell
New-AzureRmResourceGroup -Name <ResourceGroupName> -Location westcentralus 
$frontendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <frontendSubnet> -AddressPrefix "10.0.1.0/24" 
$backendSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name <backendSubnet> -AddressPrefix "10.0.2.0/24" 
New-AzureRmVirtualNetwork -Name <MyVirtualNetwork> -ResourceGroupName <ResourceGroupName>  -Location westcentralus  -AddressPrefix "10.0.0.0/16" -Subnet $frontendSubnet,$backendSubnet -DnsServer 10.0.1.5,10.0.1.6 -EnableDDoSProtection
```

W tym przykładzie tworzy sieć wirtualną z dwiema podsieciami i dwa serwery DNS. Określanie serwerów DNS w sieci wirtualnej powoduje, że karty sieciowe/maszyn wirtualnych wdrożonych w tej sieci wirtualnej dziedziczą tych serwerów DNS jako domyślne. Ochrona przed atakami DDoS jest włączona dla wszystkich chronionych zasobów w sieci wirtualnej.

### <a name="enable-ddos-protection-on-an-existing-virtual-network"></a>Włącz ochronę przed atakami DDoS na istniejącej sieci wirtualnej

Aby włączyć ochronę przed atakami DDoS na istniejącej sieci wirtualnej, należy uruchomić poniższy przykład:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $true
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName "ResourceGroupName" -ResourceName "ResourceName" -ResourceType Microsoft.Network/virtualNetworks
```

## <a name="disable-ddos-protection-on-a-virtual-network"></a>Wyłącz ochronę przed atakami DDoS w sieci wirtualnej

Aby wyłączyć ochronę przed atakami DDoS w sieci wirtualnej, należy uruchomić poniższy przykład:

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps.enableDdosProtection = $false
Set-AzureRmResource -PropertyObject $vnetProps -ResourceGroupName <RessourceGroupName> -ResourceName <ResourceName> -ResourceType "Microsoft.Network/virtualNetworks"
```

## <a name="review-the-ddos-protection-status-of-virtual-networks"></a>Sprawdź stan ochrony przed atakami DDoS sieci wirtualnych 

```powershell
$vnetProps = (Get-AzureRmResource -ResourceType "Microsoft.Network/virtualNetworks" -ResourceGroup <ResourceGroupName> -ResourceName <ResourceName>).Properties
$vnetProps
```

## <a name="use-ddos-protection-telemetry"></a>Użyj telemetrii ochrona przed atakami DDoS

Dane telemetryczne dotyczące ataku jest zapewniana za pomocą monitora Azure w czasie rzeczywistym. Dane telemetryczne jest dostępna tylko na czas trwania, która publicznego adresu IP podlega środki zaradcze. Dane telemetryczne nie będą widzieć przed lub po skuteczność została osłabiona atak.

### <a name="configure-alerts-on-ddos-protection-metrics"></a>Skonfiguruj alerty na metryki ochrona przed atakami DDoS

Wykorzystanie konfiguracji alertu Azure Monitor, można wybrać dowolną z dostępnymi metrykami ochrona przed atakami DDoS alertu, jeśli istnieje aktywne środki zaradcze podczas ataku.

#### <a name="configure-email-alert-rules-via-azure-powershell"></a>Konfigurowanie reguł alertów e-mail za pomocą programu Azure PowerShell

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

3. Aby utworzyć regułę, musisz mieć najpierw kilku ważnych informacji. 

    - Identyfikator zasobu dla zasobu, aby ustawić alert dla.
    - Definicje metryk dostępnych dla tego zasobu. Jednym ze sposobów Pobierz identyfikator zasobu jest korzystanie z portalu Azure. Zakładając, że zasób został już utworzony, wybierz go w portalu Azure. Na następnej stronie zaznacz *właściwości* w obszarze *ustawienia* sekcji. **Identyfikator ZASOBU** to pole na następnej stronie. Innym sposobem jest użycie [Eksploratora zasobów Azure](https://resources.azure.com/). Przykładowy identyfikator zasobu publicznego adresu IP to:`/subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Network/publicIPAddresses/mypublicip`

    Poniższy przykład konfiguruje alert o publicznego adresu IP skojarzonego z wirtualną siecią. Metryka ma być tworzony alert na **DDoS w ramach ataku lub nie**. Jest to wartość logiczna, 1 lub 0. A **1** oznacza podlegają ataku. A **0** oznacza, że nie jesteś zaatakowane. Alert jest tworzony, gdy są zaatakowane w ciągu ostatnich 5 minut.

    Aby utworzyć elementu webhook lub Wyślij wiadomość e-mail, gdy tworzony jest alert, należy najpierw utworzyć wiadomości e-mail i/lub elementów webhook. Następnie od razu utworzyć regułę później znacznikiem akcje i jak pokazano w poniższym przykładzie. Nie można skojarzyć elementu webhook lub wiadomości e-mail przy użyciu już utworzone zasady przy użyciu programu PowerShell.

    ```powershell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com 
    Add-AzureRmMetricAlertRule -Name <myMetricRuleWithEmail> -Location "West Central US" -ResourceGroup <myresourcegroup> -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypublicip -MetricName "IfUnderDDoSAttack" -Operator GreaterThan -Threshold 0 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail-Description "Under DDoS Attack" 
    ```

4. Aby sprawdzić, czy alerty zostały utworzone prawidłowo analizując poszczególnych reguł.

    ```powershell
    Get-AzureRmAlertRule -Name myMetricRuleWithEmail -ResourceGroup myresourcegroup -DetailedOutput 
    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```

Możesz także dowiedzieć się więcej [konfigurowaniu elementów webhook](../monitoring-and-diagnostics/insights-webhooks-alerts.md) i [aplikacje logiki](../logic-apps/logic-apps-what-are-logic-apps.md) do tworzenia alertów.

## <a name="configure-logging-on-ddos-protection-metrics"></a>Konfiguruj rejestrowanie metryki ochrona przed atakami DDoS

Zapoznaj się [PowerShell szybki start przykłady](../monitoring-and-diagnostics/insights-powershell-samples.md) ułatwiają dostęp i konfigurowanie usługi Azure diagnostyczne za pośrednictwem programu PowerShell.

## <a name="next-steps"></a>Następne kroki

- [Więcej informacji na temat dzienników diagnostycznych platformy Azure](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)
- [Analizowanie dzienników z usługi Azure storage z analizy dzienników](../log-analytics/log-analytics-azure-storage.md)
- [Rozpoczynanie pracy z usługą Event Hubs](../event-hubs/event-hubs-csharp-ephcs-getstarted.md)