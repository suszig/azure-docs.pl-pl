---
title: "Utwórz maszynę Wirtualną z statycznego publicznego adresu IP - portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć Maszynę wirtualną za pomocą statycznego publicznego adresu IP przy użyciu portalu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e9546bcc-f300-428f-b94a-056c5bd29035
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 53e9b55a82e7750393dfb6c4818681028c5c4d2f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-portal"></a>Utwórz maszynę Wirtualną za pomocą statycznego publicznego adresu IP przy użyciu portalu Azure

> [!div class="op_single_selector"]
> * [Azure portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [Program PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](virtual-network-deploy-static-pip-arm-cli.md)
> * [Szablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klasyczny)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [model wdrażania przy użyciu usługi Azure Resource Manager i model klasyczny](../resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu modelu wdrażania Menedżera zasobów, które firma Microsoft zaleca w przypadku większości nowych wdrożeń zamiast klasycznym modelu wdrażania.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="create-a-vm-with-a-static-public-ip"></a>Utwórz maszynę Wirtualną z statycznego publicznego adresu IP

Aby utworzyć Maszynę wirtualną za pomocą statycznego publicznego adresu IP w portalu Azure, wykonaj następujące kroki:

1. W przeglądarce przejdź do witryny [Azure Portal](https://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.
2. W lewym górnym rogu portalu kliknij **Utwórz zasób**>>**obliczeniowe**>**systemu Windows Server 2012 R2 Datacenter**.
3. W **wybierz model wdrożenia** wybierz **Resource Manager** i kliknij przycisk **Utwórz**.
4. W **podstawy** okienku, wprowadź następujący informacji o maszyny Wirtualnej, a następnie kliknij **OK**.
   
    ![Portal Azure — podstawy](./media/virtual-network-deploy-static-pip-arm-portal/figure1.png)
5. W **wybierz rozmiar** okienku, kliknij przycisk **A1 standardowe** następujące, a następnie kliknij przycisk **wybierz**.
   
    ![Portal Azure — wybierz rozmiar](./media/virtual-network-deploy-static-pip-arm-portal/figure2.png)
6. W **ustawienia** okienku, kliknij przycisk **publicznego adresu IP**, a następnie w **tworzenie publicznego adresu IP** okienku w obszarze **przypisania**, kliknij  **Statyczne** w następujący sposób. A następnie kliknij przycisk **OK**.
   
    ![Portal Azure — tworzenie publicznego adresu IP](./media/virtual-network-deploy-static-pip-arm-portal/figure3.png)
7. W **ustawienia** okienku, kliknij przycisk **OK**.
8. Przegląd **Podsumowanie** okienko następujące, a następnie kliknij przycisk **OK**.
   
    ![Portal Azure — tworzenie publicznego adresu IP](./media/virtual-network-deploy-static-pip-arm-portal/figure4.png)
9. Zobaczysz nowy Kafelek na pulpicie nawigacyjnym.
   
    ![Portal Azure — tworzenie publicznego adresu IP](./media/virtual-network-deploy-static-pip-arm-portal/figure5.png)
10. Po utworzeniu maszyny Wirtualnej **ustawienia** okienku są wyświetlane w następujący sposób:
    
    ![Portal Azure — tworzenie publicznego adresu IP](./media/virtual-network-deploy-static-pip-arm-portal/figure6.png)

