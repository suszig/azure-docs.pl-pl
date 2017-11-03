---
title: Tworzenie maszyny Wirtualnej w portalu Azure | Dokumentacja firmy Microsoft
description: "Utwórz maszynę wirtualną systemu Windows w portalu Azure."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1871f823-ebd7-4eff-9a22-8e2411555595
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2017
ms.author: cynthn
ms.openlocfilehash: 0981872ff819fdf49a9cc97afce3c212013ce76b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-machine-running-windows-in-the-azure-portal"></a>Utwórz maszynę wirtualną z systemem Windows w portalu Azure
> [!div class="op_single_selector"]
> * [Witryna Azure Portal](tutorial.md)
> * [Środowiska PowerShell: Wdrożenia klasycznego](create-powershell.md)
>
>

<br>

> [!IMPORTANT]
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../../../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager. Dowiedz się, jak [wykonaj te czynności przy użyciu modelu wdrażania usługi Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) przy użyciu **portalu Azure**.

Ten samouczek przedstawia sposób tworzenia maszyny wirtualnej platformy Azure (VM) systemu Windows w portalu Azure. Na przykład użyjemy obrazu systemu Windows Server, ale jest to tylko jeden z wielu obrazów oferowanych na platformie Azure. Należy pamiętać, że obrazy dostępne do wyboru zależą subskrypcji. Przykładowo obrazy komputerowe systemu Windows mogą być dostępne dla subskrybentów MSDN.

W tej sekcji przedstawia sposób użycia **pulpitu nawigacyjnego** w portalu Azure wybierz, a następnie utworzyć maszyny wirtualnej.

Można również tworzyć maszyny wirtualne przy użyciu [własnych obrazów](createupload-vhd.md). Aby dowiedzieć się więcej na temat tego i innych metod, zobacz [różne sposoby tworzenia maszyny wirtualnej systemu Windows](../../virtual-machines-windows-creation-choices.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

<!-- 02/27/2017 Video removed as it was based on the classic portal. -->

## <a id="createvirtualmachine"></a>Utwórz maszynę wirtualną
[!INCLUDE [virtual-machines-create-WindowsVM](../../../../includes/virtual-machines-create-windowsvm.md)]

## <a name="next-steps"></a>Następne kroki
* Dowiedz się, jak [utworzyć Maszynę wirtualną przy użyciu modelu wdrażania usługi Resource Manager](../../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) w portalu Azure.
* Zaloguj się do maszyny wirtualnej. Aby uzyskać instrukcje, zobacz [Zaloguj się do maszyny wirtualnej z systemem Windows Server](connect-logon.md).
* Dołączanie dysku do przechowywania danych. Możesz dołączyć zarówno puste dyski i dyski, które zawierają dane. Aby uzyskać instrukcje, zobacz [dołączenie dysku danych do maszyny wirtualnej systemu Windows utworzone z klasycznym modelu wdrażania](attach-disk.md).
