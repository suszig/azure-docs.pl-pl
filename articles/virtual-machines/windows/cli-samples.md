---
title: "Azure CLI przykłady Windows | Dokumentacja firmy Microsoft"
description: "Azure CLI przykłady systemu Windows"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: f4b2e8a5583855df7472af3fbef01ac641caf6bf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cli-samples-for-windows-virtual-machines"></a>Maszyny wirtualne platformy Azure CLI przykłady dla systemu Windows

Poniższa tabela zawiera linki do bash skrypty utworzone przy użyciu wiersza polecenia platformy Azure, które wdrażanie maszyn wirtualnych systemu Windows.

| | |
|---|---|
|**Tworzenie maszyn wirtualnych**||
| [Utwórz maszynę wirtualną](./../scripts/virtual-machines-windows-cli-sample-create-vm-quick-create.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy maszynę wirtualną systemu Windows z minimalną konfiguracją. |
| [Utwórz maszynę wirtualną w pełni skonfigurowany](./../scripts/virtual-machines-windows-cli-sample-create-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy grupę zasobów, maszyny wirtualnej i wszystkie powiązane zasoby.|
| [Tworzenie maszyn wirtualnych o wysokiej dostępności](./../scripts/virtual-machines-windows-cli-sample-nlb.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy kilka maszyny wirtualne o wysokiej dostępności i konfiguracji równoważenia obciążenia. |
| [Tworzenie maszyny Wirtualnej, a następnie uruchom skrypt konfiguracji](./../scripts/virtual-machines-windows-cli-sample-create-vm-iis.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy maszynę wirtualną i wykorzystują rozszerzenie Azure niestandardowego skryptu, aby zainstalować usługi IIS. |
| [Utwórz maszynę Wirtualną i uruchomić konfiguracji DSC](./../scripts/virtual-machines-windows-cli-sample-create-iis-using-dsc.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy maszynę wirtualną i używa rozszerzenia konfiguracji żądanego stanu Azure (DSC), aby zainstalować usługi IIS. |
|**Maszyny wirtualne sieci**||
| [Bezpieczny ruch sieciowy między maszynami wirtualnymi](./../scripts/virtual-machines-windows-cli-sample-create-vm-nsg.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy dwie maszyny wirtualne, wszystkich powiązanych zasobów i grup zabezpieczeń sieci wewnętrznych i zewnętrznych (NSG). |
|**Bezpieczne maszyny wirtualne**||
| [Szyfrowanie dysków maszyny Wirtualnej i danych](./../scripts/virtual-machines-windows-cli-sample-encrypt-vm.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy usługi Azure Key Vault, klucz szyfrowania i nazwy głównej usługi, a następnie szyfruje maszyny Wirtualnej. |
|**Monitorowanie maszyn wirtualnych**||
| [Monitor maszyny Wirtualnej w usłudze Operations Management Suite](./../scripts/virtual-machines-windows-cli-sample-create-vm-oms.md?toc=%2fcli%2fazure%2ftoc.json) | Tworzy maszynę wirtualną, a następnie instaluje agenta programu Operations Management Suite i rejestruje maszyny Wirtualnej w obszarze roboczym pakietu OMS.  |
| | |
