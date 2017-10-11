---
title: "Utwórz zestaw skali maszyny wirtualnej platformy Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie i wdrażanie skali maszyny wirtualnej systemu Linux lub Windows Azure ustawiony za pomocą interfejsu wiersza polecenia Azure, programu PowerShell, szablonu lub Visual Studio."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 07/21/2017
ms.author: adegeo
ms.openlocfilehash: 32af01aa545c541688128a7ae6bbb82a0e046f2d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-deploy-a-virtual-machine-scale-set"></a>Tworzenie i wdrażanie zestaw skali maszyny wirtualnej
Zestawy skalowania maszyny wirtualnej ułatwiają wdrażanie i zarządzanie maszynami wirtualnymi identyczne jako zestaw. Zestawy skalowania w przypadku aplikacji o dużej skali zapewnić warstwy obliczeniowej wysoce skalowalnemu i dostosowania i obsługują obrazów platformy systemu Windows, Linux platformy obrazów niestandardowych obrazów i rozszerzenia. Aby uzyskać więcej informacji na temat zestawów skalowania, zobacz [zestawy skalowania maszyny wirtualnej](virtual-machine-scale-sets-overview.md).

W tym samouczku przedstawiono sposób tworzenia zestawu skali maszyny wirtualnej **bez** przy użyciu portalu Azure. Aby uzyskać informacje o tym, jak korzystać z portalu Azure, zobacz [tworzenie skalowania maszyny wirtualnej zestawu z portalu Azure](virtual-machine-scale-sets-portal-create.md).

>[!NOTE]
>Aby uzyskać więcej informacji na temat zasobów usługi Azure Resource Manager, zobacz [usługi Azure Resource Manager, a wdrożenie klasyczne](../azure-resource-manager/resource-manager-deployment-model.md).

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Jeśli używasz interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub Azure PowerShell do utworzenia zestawu skalowania, należy najpierw zalogować się do subskrypcji.

Aby uzyskać więcej informacji o sposobie instalowania, konfigurowania i logowanie do platformy Azure z wiersza polecenia platformy Azure lub programu PowerShell, zobacz [wprowadzenie Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) lub [wprowadzenie do poleceń cmdlet programu Azure PowerShell](/powershell/azure/overview).

```azurecli
az login
```

```powershell
Login-AzureRmAccount
```

## <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Najpierw należy utworzyć grupę zasobów skojarzonego z zestawu skalowania maszyn wirtualnych.

```azurecli
az group create --location westus2 --name MyResourceGroup1
```

```powershell
New-AzureRmResourceGroup -Location westus2 -Name MyResourceGroup1
```

## <a name="create-from-azure-cli"></a>Tworzenie z interfejsu wiersza polecenia platformy Azure

Z wiersza polecenia platformy Azure można utworzyć ustawianie przy minimalnym nakładzie pracy skali maszyny wirtualnej. W przypadku pominięcia wartości domyślne, są one udostępniane dla Ciebie. Na przykład jeśli nie określisz żadnych informacji sieci wirtualnej, sieci wirtualnej jest tworzony automatycznie. W przypadku pominięcia następujące części, ich są tworzone automatycznie: 
- Moduł równoważenia obciążenia
- Sieć wirtualna
- Publiczny adres IP

W przypadku wybrania obrazu maszyny wirtualnej, który ma być używany w zestawie skalowania maszyn wirtualnych, masz kilka opcji:

- URN  
Identyfikator zasobu:  
**Win2012R2Datacenter**

- URN alias  
Przyjazna nazwa URN:  
**MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest**

- Identyfikator zasobu niestandardowego  
Ścieżka do zasobów platformy Azure:  
**/Subscriptions/Subscription-GUID/resourceGroups/MyResourceGroup/Providers/Microsoft.COMPUTE/images/MyImage**

- Zasób sieci Web  
Ścieżka do identyfikatora URI HTTP:  
**http://contoso.blob.Core.Windows.NET/vhds/osdiskimage.VHD**

>[!TIP]
>Można uzyskać listę dostępnych obrazów z `az vm image list`.

Aby utworzyć zestaw skali maszyny wirtualnej, należy określić następujące czynności:

- Grupa zasobów 
- Nazwa
- Obraz systemu operacyjnego
- Informacje dotyczące uwierzytelniania 
 
Poniższy przykład tworzy zestaw skalowania podstawowej maszyny wirtualnej (ten krok może potrwać kilka minut).

```azurecli
az vmss create --resource-group MyResourceGroup1 --name MyScaleSet --image UbuntuLTS --authentication-type password --admin-username azureuser --admin-password P@ssw0rd!
```

Po zakończeniu działania polecenia trzeba będzie teraz Twojego zestaw utworzony skalowania maszyny wirtualnej. Należy uzyskać adres IP maszyny wirtualnej, dzięki czemu można łączyć się go. Wiele różnych informacji o maszynie wirtualnej (łącznie z adresem IP) można uzyskać za pomocą następującego polecenia. 

```azurecli
az vmss list-instance-connection-info --resource-group MyResourceGroup1 --name MyScaleSet
```

## <a name="create-from-powershell"></a>Utwórz na podstawie programu PowerShell

PowerShell jest bardziej skomplikowany w użyciu niż wiersza polecenia platformy Azure. Interfejsu wiersza polecenia Azure zawiera ustawienia domyślne dla zasobów związanych z siecią (na przykład usługi równoważenia obciążenia, adresów IP i sieci wirtualne), natomiast nie obsługuje programu PowerShell. Odwołanie do obrazu przy użyciu programu PowerShell jest zbyt nieco bardziej skomplikowane. Możesz uzyskać obrazów za pomocą następujących poleceń cmdlet:

1. Get-AzureRMVMImagePublisher
2. Get-AzureRMVMImageOffer
3. Get-AzureRmVMImageSku

Pracy polecenia cmdlet mogą być przekazywane w potoku w sekwencji. Poniżej przedstawiono przykład sposobu uzyskania wszystkie obrazy dla **zachodnie stany USA 2** region z wydawcą o nazwie **microsoft** w nim.

```powershell
Get-AzureRMVMImagePublisher -Location WestUS2 | Where-Object PublisherName -Like *microsoft* | Get-AzureRMVMImageOffer | Get-AzureRmVMImageSku | Select-Object PublisherName, Offer, Skus
```

```
PublisherName              Offer                    Skus
-------------              -----                    ----
microsoft-ads              linux-data-science-vm    linuxdsvm
microsoft-ads              standard-data-science-vm standard-data-science-vm
MicrosoftAzureSiteRecovery Process-Server           Windows-2012-R2-Datacenter
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Enterprise
MicrosoftBizTalkServer     BizTalk-Server           2013-R2-Standard
MicrosoftBizTalkServer     BizTalk-Server           2016-Developer
MicrosoftBizTalkServer     BizTalk-Server           2016-Enterprise
...
```

Przepływ pracy tworzenia zestawu skali maszyny wirtualnej jest następujący:

1. Utwórz obiekt konfiguracji, który przechowuje informacje o zestawie skali.
2. Odwoływać się do obrazu podstawowego systemu operacyjnego.
3. Skonfiguruj ustawienia systemu operacyjnego: uwierzytelnianie, prefiks nazwy maszyny Wirtualnej i użytkownika/hasło.
4. Konfigurowanie sieci.
5. Utwórz zestaw skali.

W tym przykładzie tworzy zestaw na komputerze, na którym został zainstalowany program Windows Server 2016 podstawowe skalowania dwa wystąpienia.

```powershell
# Resource group name from above
$rg = "MyResourceGroup1"
$location = "WestUS2"

# Create a config object
$vmssConfig = New-AzureRmVmssConfig -Location $location -SkuCapacity 2 -SkuName Standard_A0  -UpgradePolicyMode Automatic

# Reference a virtual machine image from the gallery
Set-AzureRmVmssStorageProfile $vmssConfig -ImageReferencePublisher MicrosoftWindowsServer -ImageReferenceOffer WindowsServer -ImageReferenceSku 2016-Datacenter -ImageReferenceVersion latest

# Set up information for authenticating with the virtual machine
Set-AzureRmVmssOsProfile $vmssConfig -AdminUsername azureuser -AdminPassword P@ssw0rd! -ComputerNamePrefix myvmssvm

# Create the virtual network resources

## Basics
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "my-subnet" -AddressPrefix 10.0.0.0/24
$vnet = New-AzureRmVirtualNetwork -Name "my-network" -ResourceGroupName $rg -Location $location -AddressPrefix 10.0.0.0/16 -Subnet $subnet

## Load balancer
$publicIP = New-AzureRmPublicIpAddress -Name "PublicIP" -ResourceGroupName $rg -Location $location -AllocationMethod Static -DomainNameLabel "myuniquedomain"
$frontendIP = New-AzureRmLoadBalancerFrontendIpConfig -Name "LB-Frontend" -PublicIpAddress $publicIP
$backendPool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name "LB-backend"
$probe = New-AzureRmLoadBalancerProbeConfig -Name "HealthProbe" -Protocol Tcp -Port 80 -IntervalInSeconds 15 -ProbeCount 2
$inboundNATRule1= New-AzureRmLoadBalancerRuleConfig -Name "webserver" -FrontendIpConfiguration $frontendIP -Protocol Tcp -FrontendPort 80 -BackendPort 80 -IdleTimeoutInMinutes 15 -Probe $probe -BackendAddressPool $backendPool
$inboundNATPool1 = New-AzureRmLoadBalancerInboundNatPoolConfig -Name "RDP" -FrontendIpConfigurationId $frontendIP.Id -Protocol TCP -FrontendPortRangeStart 53380 -FrontendPortRangeEnd 53390 -BackendPort 3389

New-AzureRmLoadBalancer -ResourceGroupName $rg -Name "LB1" -Location $location -FrontendIpConfiguration $frontendIP -LoadBalancingRule $inboundNATRule1 -InboundNatPool $inboundNATPool1 -BackendAddressPool $backendPool -Probe $probe

## IP address config
$ipConfig = New-AzureRmVmssIpConfig -Name "my-ipaddress" -LoadBalancerBackendAddressPoolsId $backendPool.Id -SubnetId $vnet.Subnets[0].Id -LoadBalancerInboundNatPoolsId $inboundNATPool1.Id

# Attach the virtual network to the IP object
Add-AzureRmVmssNetworkInterfaceConfiguration -VirtualMachineScaleSet $vmssConfig -Name "network-config" -Primary $true -IPConfiguration $ipConfig

# Create the scale set with the config object (this step might take a few minutes)
New-AzureRmVmss -ResourceGroupName $rg -Name "MyScaleSet1" -VirtualMachineScaleSet $vmssConfig
```

### <a name="using-a-custom-virtual-machine-image"></a>Przy użyciu obrazu niestandardowego maszyny wirtualnej
Jeśli tworzysz skali ustawiony na podstawie własny obraz niestandardowy, zamiast odwołujące się do obrazu maszyny wirtualnej z galerii _AzureRmVmssStorageProfile zestaw_ polecenie będzie wyglądać następująco:
```PowerShell
Set-AzureRmVmssStorageProfile -OsDiskCreateOption FromImage -ManagedDisk PremiumLRS -OsDiskCaching "None" -OsDiskOsType Linux -ImageReferenceId (Get-AzureRmImage -ImageName $VMImage -ResourceGroupName $rg).id
```

## <a name="create-from-a-template"></a>Tworzenie na podstawie szablonu

Można wdrożyć skali maszyny wirtualnej ustawić przy użyciu szablonu usługi Azure Resource Manager. Można utworzyć własny szablon lub użyj jednej z [repozytorium szablonu](https://azure.microsoft.com/resources/templates/?term=vmss). Szablony te można wdrożyć bezpośrednio do subskrypcji platformy Azure.

>[!NOTE]
>Aby utworzyć własny szablon, należy utworzyć plik tekstowy w formacie JSON. Aby uzyskać ogólne informacje o sposobie tworzenia i dostosowywania szablonu, zobacz [szablonów usługi Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md).

Dostępny jest przykładowy szablon [w serwisie GitHub](https://github.com/gatneil/mvss/tree/minimum-viable-scale-set). Aby uzyskać więcej informacji dotyczących sposobu tworzenia i korzystać z tej próbki, zobacz [zestaw wielkości co najmniej](.\virtual-machine-scale-sets-mvss-start.md).

## <a name="create-from-visual-studio"></a>Utwórz w programie Visual Studio

Z programem Visual Studio można Tworzenie projektu grupy zasobów platformy Azure i dodać zestawu skalowania maszyny wirtualnej szablonu do niego. Można wybrać, czy chcesz importować go z usługi GitHub lub Galeria aplikacji sieci Web platformy Azure. Wdrożenie skryptu PowerShell zostanie również wygenerowany automatycznie. Aby uzyskać więcej informacji, zobacz [tworzenie skalowania maszyny wirtualnej zestawu z programem Visual Studio](virtual-machine-scale-sets-vs-create.md).

## <a name="create-from-the-azure-portal"></a>Tworzenie przy użyciu portalu Azure

Azure portal udostępnia wygodny sposób, aby szybko utworzyć zestaw skali. Aby uzyskać więcej informacji, zobacz [tworzenie skalowania maszyny wirtualnej zestawu z portalu Azure](virtual-machine-scale-sets-portal-create.md).

## <a name="next-steps"></a>Następne kroki

Dowiedz się więcej o [dysków z danymi](virtual-machine-scale-sets-attached-disks.md).

Dowiedz się, jak [zarządzania aplikacjami](virtual-machine-scale-sets-deploy-app.md).
