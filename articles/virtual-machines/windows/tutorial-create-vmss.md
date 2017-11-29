---
title: "Utwórz zestawy skalowania maszyny wirtualnej dla systemu Windows na platformie Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie i wdrażanie aplikacji wysokiej dostępności na maszynach wirtualnych z systemem Windows przy użyciu zestawu skali maszyny wirtualnej"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: 
ms.topic: article
ms.date: 08/11/2017
ms.author: iainfou
ms.custom: mvc
ms.openlocfilehash: d8f161af7753d2cd93a8683e8a93128144b86079
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="create-a-virtual-machine-scale-set-and-deploy-a-highly-available-app-on-windows"></a>Tworzenie zestawu skalowania maszyny wirtualnej i wdrażanie wysokiej dostępności aplikacji w systemie Windows
Zestaw skali maszyny wirtualnej umożliwia wdrażanie i zarządzanie nimi zestaw identyczne, automatyczne skalowanie maszyn wirtualnych. Można ręcznie skalować liczbę maszyn wirtualnych w zestawie skalowania lub definiowania reguł do skalowania automatycznego na podstawie użycia zasobów, takie jak procesor CPU, pamięci żądanie lub ruchu sieciowego. W tym samouczku możesz wdrożyć skali maszyny wirtualnej w usłudze Azure. Omawiane kwestie:

> [!div class="checklist"]
> * Umożliwia zdefiniowanie witryny usług IIS można skalować niestandardowe rozszerzenie skryptu
> * Tworzenie modułu równoważenia obciążenia dla zestawu skalowania
> * Utwórz zestaw skali maszyny wirtualnej
> * Zwiększ lub Zmniejsz liczbę wystąpień w zestawie skalowania
> * Tworzenie reguł skalowania automatycznego

Dla tego samouczka jest wymagany moduł Azure PowerShell w wersji 3.6 lub nowszej. Uruchom polecenie ` Get-Module -ListAvailable AzureRM`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczne będzie uaktualnienie, zobacz [Instalowanie modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).


## <a name="scale-set-overview"></a>Omówienie zestawu skali
Zestaw skali maszyny wirtualnej umożliwia wdrażanie i zarządzanie nimi zestaw identyczne, automatyczne skalowanie maszyn wirtualnych. Maszyny wirtualne w zestawie skalowania są dystrybuowane w domenach awarii i aktualizacji logikę w co najmniej jednej *umieszczania grupy*. Są to grupy podobnie skonfigurowanych maszyn wirtualnych, podobnie jak [zestawów dostępności](tutorial-availability-sets.md).

Maszyny wirtualne są tworzone zgodnie z potrzebami w zestawie skalowania. Należy zdefiniować regułę automatycznego skalowania do kontrolowania sposobu i czasu maszyny wirtualne są dodawane lub usuwane z zestawu skalowania. Te reguły może wyzwolić na podstawie metryk, takich jak obciążenie procesora CPU, pamięć lub ruchu sieciowego.

Skala ustawia obsługi maksymalnie 1000 maszyn wirtualnych, korzystając z obrazu platformy Azure. W przypadku obciążeń z instalacją znaczących lub wymagania dotyczące dostosowywania maszyny Wirtualnej, warto [utworzyć niestandardowy obraz maszyny Wirtualnej](tutorial-custom-images.md). Możesz utworzyć maksymalnie 300 maszyn wirtualnych w skali ustawić przy użyciu obrazu niestandardowego.


## <a name="create-an-app-to-scale"></a>Utwórz aplikację do skalowania
Przed utworzeniem zestaw skali, Utwórz grupę zasobów o [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Poniższy przykład tworzy grupę zasobów o nazwie *myResourceGroupAutomate* w *EastUS* lokalizacji:

```powershell
New-AzureRmResourceGroup -ResourceGroupName myResourceGroupScaleSet -Location EastUS
```

W starszych samouczka przedstawiono sposób [konfiguracji zautomatyzować maszyny Wirtualnej](tutorial-automate-vm-deployment.md) za pomocą niestandardowego rozszerzenia skryptu. Tworzenie konfiguracji zestaw skali, a następnie zastosować niestandardowe rozszerzenie skryptu, aby zainstalować i skonfigurować usługi IIS:

```powershell
# Create a config object
$vmssConfig = New-AzureRmVmssConfig `
    -Location EastUS `
    -SkuCapacity 2 `
    -SkuName Standard_DS2 `
    -UpgradePolicyMode Automatic

# Define the script for your Custom Script Extension to run
$publicSettings = @{
    "fileUris" = (,"https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/automate-iis.ps1");
    "commandToExecute" = "powershell -ExecutionPolicy Unrestricted -File automate-iis.ps1"
}

# Use Custom Script Extension to install IIS and configure basic website
Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmssConfig `
    -Name "customScript" `
    -Publisher "Microsoft.Compute" `
    -Type "CustomScriptExtension" `
    -TypeHandlerVersion 1.8 `
    -Setting $publicSettings
```

## <a name="create-scale-load-balancer"></a>Tworzenie usługi równoważenia obciążenia skali
Moduł równoważenia obciążenia Azure jest równoważenia obciążenia warstwy 4 (TCP, UDP), który zapewnia wysoką dostępność, przekazując przychodzący ruch między maszynami wirtualnymi w dobrej kondycji. Badanie kondycji modułu równoważenia obciążenia monitoruje danego portu na każdej maszynie Wirtualnej i tylko dystrybuuje ruch do operacyjnej maszyny Wirtualnej. Aby uzyskać więcej informacji, zobacz następny samouczek na [jak załadować saldo maszyn wirtualnych Windows](tutorial-load-balancer.md).

Utwórz moduł równoważenia obciążenia, który ma publicznego adresu IP i rozpowszechnia ruchu w sieci web na porcie 80:

```powershell
# Create a public IP address
$publicIP = New-AzureRmPublicIpAddress `
  -ResourceGroupName myResourceGroupScaleSet `
  -Location EastUS `
  -AllocationMethod Static `
  -Name myPublicIP

# Create a frontend and backend IP pool
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig `
  -Name myFrontEndPool `
  -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name myBackEndPool

# Create the load balancer
$lb = New-AzureRmLoadBalancer `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myLoadBalancer `
  -Location EastUS `
  -FrontendIpConfiguration $frontendIP `
  -BackendAddressPool $backendPool

# Create a load balancer health probe on port 80
Add-AzureRmLoadBalancerProbeConfig -Name myHealthProbe `
  -LoadBalancer $lb `
  -Protocol tcp `
  -Port 80 `
  -IntervalInSeconds 15 `
  -ProbeCount 2

# Create a load balancer rule to distribute traffic on port 80
Add-AzureRmLoadBalancerRuleConfig `
  -Name myLoadBalancerRule `
  -LoadBalancer $lb `
  -FrontendIpConfiguration $lb.FrontendIpConfigurations[0] `
  -BackendAddressPool $lb.BackendAddressPools[0] `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

# Update the load balancer configuration
Set-AzureRmLoadBalancer -LoadBalancer $lb
```

## <a name="create-a-scale-set"></a>Utwórz zestaw skali
Teraz Utwórz zestaw o skali maszyny wirtualnej [AzureRmVmss nowy](/powershell/module/azurerm.compute/new-azurermvm). Poniższy przykład tworzy zestaw o nazwie skalowania *myScaleSet*:

```powershell
# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig `
  -ImageReferencePublisher MicrosoftWindowsServer `
  -ImageReferenceOffer WindowsServer `
  -ImageReferenceSku 2016-Datacenter `
  -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig `
  -AdminUsername azureuser `
  -AdminPassword P@ssword! `
  -ComputerNamePrefix myVM

# Create the virtual network resources
$subnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name "mySubnet" `
  -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork `
  -ResourceGroupName "myResourceGroupScaleSet" `
  -Name "myVnet" `
  -Location "EastUS" `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $subnet
$ipConfig = New-AzureRmVmssIpConfig `
  -Name "myIPConfig" `
  -LoadBalancerBackendAddressPoolsId $lb.BackendAddressPools[0].Id `
  -SubnetId $vnet.Subnets[0].Id

# Attach the virtual network to the config object
Add-AzureRmVmssNetworkInterfaceConfiguration `
  -VirtualMachineScaleSet $vmssConfig `
  -Name "network-config" `
  -Primary $true `
  -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -Name myScaleSet `
  -VirtualMachineScaleSet $vmssConfig
```

Trwa kilka minut, aby utworzyć i skonfigurować zasoby zestaw skali i maszyn wirtualnych.


## <a name="test-your-app"></a>Testowanie aplikacji
Aby zapoznać się z witryny sieci Web usług IIS w akcji, uzyskania publicznego adresu IP z usługi równoważenia obciążenia z [Get-AzureRmPublicIPAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress). Poniższy przykład uzyskuje adres IP dla *myPublicIP* utworzona w ramach zestawu skali:

```powershell
Get-AzureRmPublicIPAddress -ResourceGroupName myResourceGroupScaleSet -Name myPublicIP | select IpAddress
```

Wprowadź publicznego adresu IP w przeglądarce sieci web. Aplikacja jest wyświetlana, łącznie z nazwą hosta maszyny Wirtualnej, ruch do dystrybucji modułu równoważenia obciążenia:

![Uruchomionej witryny usług IIS](./media/tutorial-create-vmss/running-iis-site.png)

Aby wyświetlić zestaw akcji skalowania, możesz można życie odświeżania przeglądarki sieci web, aby zobaczyć Dystrybuuj ruch na wszystkich maszynach wirtualnych z tą aplikacją usługi równoważenia obciążenia.


## <a name="management-tasks"></a>Zadania zarządzania
W całym cyklu życia zestawu skalowania konieczne może być Uruchom jedno lub więcej zadań zarządzania. Ponadto można tworzenia skryptów automatyzujących różnych zadań cyklu życia. Program Azure PowerShell umożliwia szybkie do wykonywania tych zadań. Poniżej przedstawiono kilka typowych zadań.

### <a name="view-vms-in-a-scale-set"></a>Maszyny wirtualne widoku w zestawie skalowania
Aby wyświetlić listę uruchomionych w zestawie skalowania maszyn wirtualnych, użyj [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) w następujący sposób:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Loop through the instanaces in your scale set
for ($i=1; $i -le ($scaleset.Sku.Capacity - 1); $i++) {
    Get-AzureRmVmssVM -ResourceGroupName myResourceGroupScaleSet `
      -VMScaleSetName myScaleSet `
      -InstanceId $i
}
```


### <a name="increase-or-decrease-vm-instances"></a>Zwiększanie lub zmniejszanie wystąpień maszyn wirtualnych
Aby wyświetlić liczbę wystąpień aktualnie zainstalowana w zestawie skalowania, użyj [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) i wykonywać zapytania na *sku.capacity*:

```powershell
Get-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -VMScaleSetName myScaleSet | `
    Select -ExpandProperty Sku
```

Możesz ręcznie zwiększyć lub zmniejszyć liczbę maszyn wirtualnych w skali ustawiony za pomocą [AzureRmVmss aktualizacji](/powershell/module/azurerm.compute/update-azurermvmss). Poniższy przykład ustawia liczbę maszyn wirtualnych w skali, z ustawioną *5*:

```powershell
# Get current scale set
$scaleset = Get-AzureRmVmss `
  -ResourceGroupName myResourceGroupScaleSet `
  -VMScaleSetName myScaleSet

# Set and update the capacity of your scale set
$scaleset.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName myResourceGroupScaleSet `
    -Name myScaleSet `
    -VirtualMachineScaleSet $scaleset
```

Jeśli ustawiona przyjmuje kilka minut, aby zaktualizować określoną liczbę wystąpień w skali sieci.


### <a name="configure-autoscale-rules"></a>Konfigurowanie reguł automatycznego skalowania
Zamiast ręcznie skalowanie liczby wystąpień w skali sieci, należy zdefiniować regułę automatycznego skalowania. Te reguły monitorować wystąpienia w zestawie skali i Odpowiedz, odpowiednio na podstawie metryk i progów, które należy zdefiniować. Limit liczby wystąpień programu przez jedną skaluje poniższy przykład, gdy średni obciążenie procesora CPU jest większy niż 60% w okresie 5 minut. Jeśli w okresie 5 minut średnie obciążenie procesora CPU spadnie poniżej 30%, wystąpienia są skalowane w przez jedno wystąpienie:

```powershell
# Define your scale set information
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"

# Create a scale up rule to increase the number instances after 60% average CPU usage exceeded for a 5 minute period
$myRuleScaleUp = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator GreaterThan `
  -MetricStatistic Average `
  -Threshold 60 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Increase `
  -ScaleActionValue 1

# Create a scale down rule to decrease the number of instances after 30% average CPU usage over a 5 minute period
$myRuleScaleDown = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:05:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  -ScaleActionValue 1

# Create a scale profile with your scale up and scale down rules
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleUp,$myRuleScaleDown `
  -Name "autoprofile"

# Apply the autoscale rules
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```

Aby uzyskać więcej informacji projektu na korzystanie z automatycznego skalowania, zobacz [najlepsze rozwiązania w zakresie skalowania automatycznego](/azure/architecture/best-practices/auto-scaling).


## <a name="next-steps"></a>Następne kroki
W tym samouczku utworzony zestaw skali maszyny wirtualnej. W tym samouczku omówiono:

> [!div class="checklist"]
> * Umożliwia zdefiniowanie witryny usług IIS można skalować niestandardowe rozszerzenie skryptu
> * Tworzenie modułu równoważenia obciążenia dla zestawu skalowania
> * Utwórz zestaw skali maszyny wirtualnej
> * Zwiększ lub Zmniejsz liczbę wystąpień w zestawie skalowania
> * Tworzenie reguł skalowania automatycznego

Przejdź do następnego samouczek, aby dowiedzieć się więcej na temat pojęć dla maszyn wirtualnych równoważenia obciążenia.

> [!div class="nextstepaction"]
> [Równoważyć obciążenie maszyn wirtualnych](tutorial-load-balancer.md)
