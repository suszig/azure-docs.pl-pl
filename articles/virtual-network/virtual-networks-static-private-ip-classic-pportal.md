---
title: "Konfigurowanie prywatnych adresów IP dla maszyn wirtualnych (klasyczne) — portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować prywatnych adresów IP maszyn wirtualnych (klasyczne) za pomocą portalu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bde6de3495c2909b63b1f85e420a4ff5e7ac2c1a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Konfigurowanie prywatnych adresów IP dla maszyny wirtualnej (klasyczne) przy użyciu portalu Azure

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

W tym artykule opisano klasyczny model wdrażania. Możesz również [Zarządzanie statycznego prywatnego adresu IP w modelu wdrażania usługi Resource Manager](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Poniższe kroki przykładowe oczekiwać środowisku niezłożonym już utworzone. Wykonanie kroków wyświetlaną w tym dokumencie, najpierw utworzyć środowisko testowe opisane w [utworzyć sieć wirtualną](virtual-networks-create-vnet-classic-pportal.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Sposób określania statycznego prywatnego adresu IP, podczas tworzenia maszyny Wirtualnej
Aby utworzyć Maszynę wirtualną o nazwie *DNS01* w *frontonu* podsieci sieci wirtualnej o nazwie *TestVNet* z statycznego prywatnego adresu IP z *192.168.1.101*, wykonaj następujące czynności:

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **nowy** > **obliczeniowe** > **systemu Windows Server 2012 R2 Datacenter**, zwróć uwagę, że **wybierz model wdrożenia** listy już pokazuje **klasycznego**, a następnie kliknij przycisk **Utwórz**.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. W **Utwórz maszynę Wirtualną** bloku, wprowadź nazwę maszyny Wirtualnej, która ma zostać utworzony (*DNS01* w naszym scenariuszu), konto administratora lokalnego i hasło.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Kliknij przycisk **konfiguracji opcjonalnej** > **sieci** > **sieci wirtualnej**, a następnie kliknij przycisk **TestVNet**. Jeśli **TestVNet** jest niedostępny, upewnij się, że używasz *środkowe stany USA* lokalizacji i utworzeniu środowisko testowe opisane na początku tego artykułu.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. W **sieci** bloku, upewnij się, że jest obecnie wybranej podsieci *frontonu*, następnie kliknij przycisk **adresów IP**w obszarze **przypisywania adresów IP**kliknij **statycznych**, a następnie wprowadź *192.168.1.101* dla **adres IP** jak pokazano poniżej.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Kliknij przycisk **OK** w **adresów IP** bloku, następnie kliknij przycisk **OK** w **sieci** bloku, a następnie kliknij przycisk **OK** w **opcjonalne config** bloku.
7. W **Utwórz maszynę Wirtualną** bloku, kliknij przycisk **Utwórz**. Zwróć uwagę, poniżej kafelków wyświetlanych na pulpicie nawigacyjnym.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak pobrać statycznych prywatne informacje o adresie IP dla maszyny Wirtualnej
Aby wyświetlić statyczny prywatne informacje o adresie IP dla maszyny Wirtualnej utworzone za pomocą kroków opisanych powyżej, wykonaj poniższe kroki.

1. W portalu Azure, Azure, kliknij polecenie **PRZEGLĄDAJ wszystko** > **maszyn wirtualnych (klasyczne)** > **DNS01** > **wszystkie ustawienia** > **adresów IP** i zwróć uwagę przypisywania adresów IP i adres IP, jak pokazano poniżej.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak usunąć statycznego prywatnego adresu IP z maszyny Wirtualnej
Aby usunąć statycznego prywatnego adresu IP z maszyny Wirtualnej utworzone powyżej, wykonaj poniższe kroki.

1. Z **adresów IP** bloku przedstawionych powyżej, kliknij przycisk **dynamiczne** z prawej strony **przypisywania adresów IP**, następnie kliknij przycisk **zapisać**, a następnie kliknij przycisk **Tak**.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak dodać statycznego prywatnego adresu IP do istniejącej maszyny Wirtualnej
Aby dodać statycznego prywatnego adresu IP do maszyny Wirtualnej utworzone za pomocą kroków opisanych powyżej, wykonaj następujące czynności:

1. Z **adresów IP** bloku przedstawionych powyżej, kliknij przycisk **statycznych** z prawej strony **przypisywania adresów IP**.
2. Typ *192.168.1.101* dla **adres IP**, następnie kliknij przycisk **zapisać**, a następnie kliknij przycisk **tak**.

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [zastrzeżone publicznego adresu IP](virtual-networks-reserved-public-ip.md) adresów.
* Dowiedz się więcej o [poziomie wystąpienia publicznego adresu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresów.
* Zapoznaj się [zastrzeżone interfejsów API REST IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).

