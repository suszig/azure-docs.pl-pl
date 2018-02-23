---
title: "Konfigurowanie prywatnych adresów IP dla maszyn wirtualnych - portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować prywatnych adresów IP maszyn wirtualnych za pomocą portalu Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c2679c7cb75c438402f3ab64c3e14c964cb8a85d
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-using-the-azure-portal"></a>Konfigurowanie prywatnych adresów IP dla maszyny wirtualnej przy użyciu portalu Azure

> [!div class="op_single_selector"]
> * [Azure portal](virtual-networks-static-private-ip-arm-pportal.md)
> * [Program PowerShell](virtual-networks-static-private-ip-arm-ps.md)
> * [Interfejs wiersza polecenia platformy Azure](virtual-networks-static-private-ip-arm-cli.md)
> * [Portalu Azure (klasyczne)](virtual-networks-static-private-ip-classic-pportal.md)
> * [PowerShell (klasyczny)](virtual-networks-static-private-ip-classic-ps.md)
> * [Interfejs wiersza polecenia platformy Azure (klasyczne)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

W tym artykule opisano model wdrażania usługi Resource Manager. Możesz również [Zarządzanie statycznego prywatnego adresu IP w klasycznym modelu wdrażania](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Następujące przykładowe oczekiwać środowisku niezłożonym już utworzone. Wykonanie kroków wyświetlaną w tym dokumencie, najpierw utworzyć środowisko testowe opisane w [utworzyć sieć wirtualną](virtual-networks-create-vnet-arm-pportal.md).

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Jak utworzyć Maszynę wirtualną do testowania prywatnych adresów IP
Nie można ustawić statycznego prywatnego adresu IP podczas tworzenia maszyny wirtualnej w tryb wdrażania usługi Resource Manager przy użyciu portalu Azure. Należy najpierw utworzyć maszynę Wirtualną, a następnie ustaw jej prywatnego adresu IP statycznej.

Aby utworzyć Maszynę wirtualną o nazwie *DNS01* w *frontonu* podsieci sieci wirtualnej o nazwie *TestVNet*, wykonaj następujące kroki:

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **Utwórz zasób** > **obliczeniowe** > **systemu Windows Server 2012 R2 Datacenter**, zwróć uwagę, że **wybierz wdrożenie model** listy już pokazuje **Menedżera zasobów**, a następnie kliknij przycisk **Utwórz**, jak pokazano na poniższej ilustracji.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. W **podstawy** okienku, wprowadź nazwę maszyny Wirtualnej, aby utworzyć (*DNS01* w scenariuszu), konto administratora lokalnego i hasło, jak pokazano na poniższej ilustracji.
   
    ![Okienko podstawy](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Upewnij się, że **lokalizacji** jest *środkowe stany USA*, następnie kliknij przycisk **wybierz istniejącą** w obszarze **grupy zasobów**, następnie kliknij przycisk **grupy zasobów** ponownie, następnie kliknij przycisk *TestRG*, a następnie kliknij przycisk **OK**.
   
    ![Okienko podstawy](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. W **wybierz rozmiar** okienku wybierz **A1 standardowe**, a następnie kliknij przycisk **wybierz**.
   
    ![Wybierz rozmiar okienka](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. W **ustawienia** okienka, pamiętaj, właściwości są konfigurowane przy użyciu następujących wartości, a następnie kliknij przycisk **OK**.
   
    -**Konto magazynu**: *vnetstorage*
   
   * **Sieci**: *TestVNet*
   * **Podsieci**: *frontonu*
     
     ![Wybierz rozmiar okienka](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. W **Podsumowanie** okienku, kliknij przycisk **OK**. Zobaczysz Kafelek następujące wyświetlane na pulpicie nawigacyjnym.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Jak pobrać statycznych prywatne informacje o adresie IP dla maszyny Wirtualnej
Aby wyświetlić statyczny prywatne informacje o adresie IP dla maszyny Wirtualnej utworzone za pomocą kroków opisanych powyżej, należy wykonać następujące kroki.

1. W portalu Azure kliknij **PRZEGLĄDAJ wszystko** > **maszyn wirtualnych** > **DNS01** > **wszystkie ustawienia**  >  **Interfejsy sieciowe** a następnie kliknij polecenie tylko interfejsu sieciowego na liście.
   
    ![Wdrażanie maszyny Wirtualnej kafelka](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. W **interfejsu sieciowego** okienku, kliknij przycisk **wszystkie ustawienia** > **adresów IP** i zwróć uwagę, **przypisania** i  **Adres IP** wartości.
   
    ![Wdrażanie maszyny Wirtualnej kafelka](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Jak dodać statycznego prywatnego adresu IP do istniejącej maszyny Wirtualnej
Aby dodać statycznego prywatnego adresu IP do maszyny Wirtualnej utworzone za pomocą kroków opisanych powyżej, wykonaj następujące kroki:

1. Z **adresów IP** okienku przedstawionych powyżej, kliknij polecenie **statycznych** w obszarze **przypisania**.
2. Typ *192.168.1.101* dla **adres IP**, a następnie kliknij przycisk **zapisać**.
   
    ![Tworzenie maszyny Wirtualnej w portalu Azure](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Jeśli po kliknięciu przycisku **zapisać**, można zauważyć, że przypisanie nadal jest ustawione **dynamiczne**, oznacza to wpisany adres IP jest już używana. Spróbuj innego adresu IP.
> 
> 

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Jak usunąć statycznego prywatnego adresu IP z maszyny Wirtualnej
Aby usunąć statycznego prywatnego adresu IP z maszyny Wirtualnej utworzone powyżej, należy wykonać następujący krok:

Z **adresów IP** okienko przedstawionych powyżej, kliknij przycisk **dynamiczne** w obszarze **przypisania**, a następnie kliknij przycisk **Zapisz**.

## <a name="next-steps"></a>Kolejne kroki
* Dowiedz się więcej o [zastrzeżone publicznego adresu IP](virtual-networks-reserved-public-ip.md) adresów.
* Dowiedz się więcej o [poziomie wystąpienia publicznego adresu IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adresów.
* Zapoznaj się [zastrzeżone interfejsów API REST IP](https://msdn.microsoft.com/library/azure/dn722420.aspx).

