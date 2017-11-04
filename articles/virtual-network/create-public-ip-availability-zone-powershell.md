---
title: "Utwórz zoned adres publiczny adres IP przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "Utwórz publiczny adres IP w strefie dostępności przy użyciu programu PowerShell."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/25/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: bfeba57036338b4e325d2f122443f2cde0373eed
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-public-ip-address-in-an-availability-zone-with-powershell"></a>Tworzenie publicznego adresu IP w strefie dostępności przy użyciu programu PowerShell

Można wdrożyć publicznego adresu IP w strefie dostępności Azure (wersja zapoznawcza). Strefa dostępności jest fizycznie oddzielnych stref w regionie platformy Azure. Instrukcje:

> * Tworzenie publicznego adresu IP w strefie dostępności
> * Zidentyfikuj powiązane zasoby utworzone w strefie dostępności
  

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

W tym artykule wymaga wersji 4.4.0 lub wyższej modułu AzureRM zainstalowane. Aby dowiedzieć się, jaka wersja jest używana, uruchom polecenie `Get-Module -ListAvailable AzureRM`. Aby zainstalować lub uaktualnić należy zainstalować najnowszą wersję modułu AzureRM z [galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM).

> [!NOTE]
> Dostępność strefy są w wersji zapoznawczej i są gotowe do programowania i testowania scenariuszy. Obsługa jest dostępna dla wybierz zasobów platformy Azure i regiony i rodziny rozmiar maszyny Wirtualnej. Więcej informacji na temat rozpocząć, które zasobów platformy Azure, regiony i rodziny rozmiar maszyny Wirtualnej można spróbować stref dostępności przy, zobacz [Przegląd stref dostępności](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview). Aby uzyskać pomoc techniczną, możesz skorzystać z witryny [StackOverflow](https://stackoverflow.com/questions/tagged/azure-availability-zones) lub [otworzyć bilet pomocy technicznej platformy Azure](../azure-supportability/how-to-create-azure-support-request.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się do subskrypcji platformy Azure za pomocą polecenia `Login-AzureRmAccount` i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```powershell
Login-AzureRmAccount
```
## <a name="create-resource-group"></a>Tworzenie grupy zasobów

Utwórz grupę zasobów platformy Azure za pomocą polecenia [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Grupa zasobów to logiczny kontener przeznaczony do wdrażania zasobów platformy Azure i zarządzania nimi. W tym przykładzie grupy zasobów o nazwie *myResourceGroup* jest tworzony w *westeurope* regionu. *westeurope* jest jednym z regiony platformy Azure, które obsługuje stref dostępności w wersji zapoznawczej.

```powershell
New-AzureRmResourceGroup -Name AzTest -Location westeurope
```

## <a name="create-a-zonal-public-ip-address"></a>Utwórz zonal publicznego adresu IP

Utwórz publiczny adres IP w strefie dostępności przy użyciu następującego polecenia:

```powershell
    New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroup `
        -Name myPublicIp `
        -Location westeurope `
        -AllocationMethod Static `
        -Zone 2
```

> [!NOTE]
> Po przypisaniu standardowego publicznego adresu IP jednostki SKU do interfejsu sieciowego maszyny wirtualnej musisz jawnie zezwolić na ruch do miejsca przeznaczenia przy użyciu [sieciowej grupy zabezpieczeń](security-overview.md#network-security-groups). Próba komunikacji z zasobem będzie kończyć się niepowodzeniem do momentu utworzenia i skojarzenia sieciowej grupy zabezpieczeń, a następnie jawnego zezwolenia na żądany ruch.

## <a name="get-zone-information-about-a-public-ip-address"></a>Pobierz informacje strefy publicznego adresu IP

Uzyskaj informacje o strefie publicznego adresu IP za pomocą następującego polecenia:

```powershell
Get-AzureRmPublicIpAddress ` 
    -ResourceGroup myResourceGroup `
    -Name myPublicIp
```

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [stref dostępności](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview)
- Dowiedz się więcej o [publicznych adresów IP](virtual-network-public-ip-address.md?toc=%2fazure%2fvirtual-network%2ftoc.json) 