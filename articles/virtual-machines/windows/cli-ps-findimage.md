---
title: Wybierz obrazy maszyny Wirtualnej systemu Windows na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak używać programu Azure PowerSHell w celu określenia wydawcy, oferty, jednostki SKU i wersji dla obrazów maszyn wirtualnych w witrynie Marketplace."
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 188b8974-fabd-4cd3-b7dc-559cbb86b98a
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: c9b35ff5f3fbd33639805b5a4f105df32562a691
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-find-windows-vm-images-in-the-azure-marketplace-with-azure-powershell"></a>Jak znaleźć obrazów maszyn wirtualnych systemu Windows w portalu Azure Marketplace przy użyciu programu Azure PowerShell

W tym temacie opisano sposób użycia programu Azure PowerShell można znaleźć obrazów maszyn wirtualnych w portalu Azure Marketplace. Dzięki tym informacjom można określić obrazu z witryny Marketplace, podczas tworzenia maszyny Wirtualnej systemu Windows.

Upewnij się, że zainstalowane i skonfigurowane najnowszej [modułu Azure PowerShell](/powershell/azure/install-azurerm-ps).



## <a name="table-of-commonly-used-windows-images"></a>Tabela często używane obrazów systemu Windows
| PublisherName | Oferta | SKU |
|:--- |:--- |:--- |:--- |
| MicrosoftWindowsServer |WindowsServer |Centrum danych 2016 |
| MicrosoftWindowsServer |WindowsServer |2016-Datacenter-Server-Core |
| MicrosoftWindowsServer |WindowsServer |2016 centrum danych z kontenerów |
| MicrosoftWindowsServer |WindowsServer |2016-Nano serwer |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |
| MicrosoftWindowsServer |WindowsServer |2008 R2 SP1 |
| MicrosoftDynamicsNAV |DynamicsNAV |2017 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2016 |
| MicrosoftSQLServer |SQL2016 WS2016 |Enterprise |
| MicrosoftSQLServer |SQL2014SP2 WS2012R2 |Enterprise |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |

## <a name="find-specific-images"></a>Znajdowanie określonych obrazów


Podczas tworzenia nowej maszyny wirtualnej przy użyciu usługi Azure Resource Manager w niektórych przypadkach należy określić obraz za pomocą kombinacji następujących właściwości obrazu:

* Wydawca
* Oferta
* SKU

Na przykład użyć tych wartości za pomocą [AzureRMVMSourceImage zestaw](/powershell/module/azurerm.compute/set-azurermvmsourceimage) polecenia cmdlet programu PowerShell, lub za pomocą szablonu grupy zasobów, w którym należy określić typ maszyna wirtualna ma zostać utworzony.

Jeśli trzeba określić te wartości, możesz uruchomić [Get-AzureRMVMImagePublisher](/powershell/module/azurerm.compute/get-azurermvmimagepublisher), [Get-AzureRMVMImageOffer](/powershell/module/azurerm.compute/get-azurermvmimageoffer), i [Get AzureRMVMImageSku](/powershell/module/azurerm.compute/get-azurermvmimagesku) poleceń cmdlet, aby przejść obrazów. Należy określić te wartości:

1. Wyświetl listę wydawców obrazów.
2. Dla danego wydawcy wyświetl listę ofert.
3. Dla danej oferty wyświetl listę wersji SKU.

Najpierw wyświetl listę wydawców za pomocą następujących poleceń:

```azurepowershell-interactive
$locName="<Azure location, such as West US>"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName
```

Wprowadź nazwę wybranego wydawcy i uruchom następujące polecenia:

```azurepowershell-interactive
$pubName="<publisher>"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Wprowadź nazwę wybranej oferty i uruchom następujące polecenia:

```azurepowershell-interactive
$offerName="<offer>"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Z danych wyjściowych `Get-AzureRMVMImageSku` polecenie ma wszystkie informacje, należy określić obraz dla nowej maszyny wirtualnej.

Poniżej przedstawiono pełny przykład:

```azurepowershell-interactive
$locName="West US"
Get-AzureRMVMImagePublisher -Location $locName | Select PublisherName

```

Dane wyjściowe:

```
PublisherName
-------------
a10networks
aiscaler-cache-control-ddos-and-url-rewriting-
alertlogic
AlertLogic.Extension
Barracuda.Azure.ConnectivityAgent
barracudanetworks
basho
boxless
bssw
Canonical
...
```

Dla wydawcy „MicrosoftWindowsServer”:

```azurepowershell-interactive
$pubName="MicrosoftWindowsServer"
Get-AzureRMVMImageOffer -Location $locName -Publisher $pubName | Select Offer
```

Dane wyjściowe:

```
Offer
-----
Windows-HUB
WindowsServer
WindowsServer-HUB
```

Dla oferty „WindowsServer”:

```azurepowershell-interactive
$offerName="WindowsServer"
Get-AzureRMVMImageSku -Location $locName -Publisher $pubName -Offer $offerName | Select Skus
```

Dane wyjściowe:

```
Skus
----
2008-R2-SP1
2008-R2-SP1-smalldisk
2012-Datacenter
2012-Datacenter-smalldisk
2012-R2-Datacenter
2012-R2-Datacenter-smalldisk
2016-Datacenter
2016-Datacenter-Server-Core
2016-Datacenter-Server-Core-smalldisk
2016-Datacenter-smalldisk
2016-Datacenter-with-Containers
2016-Nano-Server
```

Skopiuj z tej listy nazwę wybranej wersji SKU — w ten sposób uzyskasz wszystkie informacje, jakie należy podać w poleceniu cmdlet programu PowerShell `Set-AzureRMVMSourceImage` lub szablonie grupy zasobów.

## <a name="next-steps"></a>Następne kroki
Teraz można dokładnie wybrać obraz, który ma być używany. Aby szybko utworzyć maszynę wirtualną, korzystając z informacji obrazu, który właśnie odnaleziony, zobacz [Utwórz maszynę wirtualną z systemem Windows przy użyciu programu PowerShell](quick-create-powershell.md).
