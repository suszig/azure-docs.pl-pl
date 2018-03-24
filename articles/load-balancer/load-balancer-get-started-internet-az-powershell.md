---
title: Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP za pomocą programu PowerShell | Dokumentacja firmy Microsoft
description: Dowiedz się, jak utworzyć publiczny standardowe usługi równoważenia obciążenia z strefowo nadmiarowy frontonu adres publiczny adres IP za pomocą programu PowerShell
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2018
ms.author: kumud
ms.openlocfilehash: 2849ef9b862b4471f348ac83a4a18eb9a3a4d42a
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
#  <a name="create-a-public-load-balancer-standard-with-zone-redundant-public-ip-address-frontend-using-powershell"></a>Utwórz publiczny obciążenia standardowego równoważenia z strefowo nadmiarowy frontonu adres publiczny adres IP za pomocą programu PowerShell

W tym artykule opisano przez proces tworzenia publiczny [standardowe usługi równoważenia obciążenia](https://aka.ms/azureloadbalancerstandard) z strefowo nadmiarowy frontonu przy użyciu adresu publicznego adresu IP standardowa.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="register-for-availability-zones-preview"></a>Zarejestruj się, aby Podgląd stref dostępności

W tym artykule wymaga wersji 4.4.0 lub wyższej modułu AzureRM zainstalowane. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `Get-Module -ListAvailable AzureRM`. Aby zainstalować lub uaktualnić należy zainstalować najnowszą wersję modułu AzureRM z [galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM).

> [!NOTE]
> Dostępność strefy są w wersji zapoznawczej i są gotowe do programowania i testowania scenariuszy. Obsługa jest dostępna dla wybierz zasobów platformy Azure i regiony i rodziny rozmiar maszyny Wirtualnej. Więcej informacji na temat rozpocząć, które zasobów platformy Azure, regiony i rodziny rozmiar maszyny Wirtualnej można spróbować stref dostępności przy, zobacz [Przegląd stref dostępności](https://docs.microsoft.com/azure/availability-zones/az-overview). Aby uzyskać pomoc techniczną, możesz skorzystać z witryny [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) lub [otworzyć bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).  

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów, przy użyciu następującego polecenia:

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location westeurope
```

## <a name="create-a-public-ip-standard"></a>Utwórz publiczny Standard IP 
Tworzenie publicznego adresu IP standardowe przy użyciu następującego polecenia:

```powershell
$publicIp = New-AzureRmPublicIpAddress -ResourceGroupName myResourceGroup -Name 'myPublicIP' `
  -Location westeurope -AllocationMethod Static -Sku Standard 
```

## <a name="create-a-front-end-ip-configuration-for-the-website"></a>Utwórz konfigurację IP frontonu witryny sieci Web

Tworzenie konfiguracji IP frontonu przy użyciu następującego polecenia:

```powershell
$feip = New-AzureRmLoadBalancerFrontendIpConfig -Name 'myFrontEndPool' -PublicIpAddress $publicIp
```

## <a name="create-the-back-end-address-pool"></a>Tworzenie puli adresów zaplecza

Utwórz pulę adresów zaplecza za pomocą następującego polecenia:

```powershell
$bepool = New-AzureRmLoadBalancerBackendAddressPoolConfig -Name 'myBackEndPool'
```

## <a name="create-a-load-balancer-probe-on-port-80"></a>Utwórz sondę modułu równoważenia obciążenia na porcie 80

Utwórz sondy kondycji na porcie 80 dla usługi równoważenia obciążenia za pomocą następującego polecenia:

```powershell
$probe = New-AzureRmLoadBalancerProbeConfig -Name 'myHealthProbe' -Protocol Http -Port 80 `
  -RequestPath / -IntervalInSeconds 360 -ProbeCount 5
```

## <a name="create-a-load-balancer-rule"></a>Tworzenie reguły modułu równoważenia obciążenia
 Utwórz regułę modułu równoważenia obciążenia za pomocą następującego polecenia:

```powershell
   $rule = New-AzureRmLoadBalancerRuleConfig -Name HTTP -FrontendIpConfiguration $feip -BackendAddressPool  $bepool -Probe $probe -Protocol Tcp -FrontendPort 80 -BackendPort 80
```

## <a name="create-a-load-balancer"></a>Tworzenie modułu równoważenia obciążenia
Utwórz obciążenia równoważenia standardowe przy użyciu następującego polecenia:

```powershell
$lb = New-AzureRmLoadBalancer -ResourceGroupName myResourceGroup -Name 'MyLoadBalancer' -Location westeurope `
  -FrontendIpConfiguration $feip -BackendAddressPool $bepool `
  -Probe $probe -LoadBalancingRule $rule -Sku Standard
```

## <a name="next-steps"></a>Kolejne kroki
- Dowiedz się, jak [tworzenie publicznego adresu IP w strefie dostępności](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)



