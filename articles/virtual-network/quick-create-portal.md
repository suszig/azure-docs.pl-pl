---
title: Tworzenie sieci wirtualnej na platformie Azure - Portal | Dokumentacja firmy Microsoft
description: "Dowiedz się szybko utworzyć sieć wirtualną przy użyciu portalu Azure. Sieć wirtualna umożliwia wiele typów zasobów platformy Azure do prywatnie komunikują się ze sobą."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 8b6a4abdb7677417462392feade0c7cfdf99246f
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/09/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Tworzenie sieci wirtualnej przy użyciu witryny Azure Portal

W tym artykule należy Dowiedz się, jak utworzyć sieć wirtualną. Po utworzeniu sieci wirtualnej, wdrożenie dwóch maszyn wirtualnych w sieci wirtualnej i prywatnie komunikacji między nimi, a z Internetem.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.
3. Wprowadź, lub wybierz poniższe informacje, a następnie wybierz **Utwórz**:
    - **Nazwa**: *myVirtualNetwork*
    - **Przestrzeń adresowa**: zaakceptuj wartość domyślną. Przestrzeń adresowa jest określone w notacji CIDR.
    - **Subskrypcja**: Wybierz subskrypcję.
    - **Grupa zasobów**: Wybierz **Utwórz nowy** , a następnie wprowadź *myResourceGroup*.
    - **Lokalizacja**: Wybierz * wschodnie stany USA **.
    - **Nazwa podsieci**: zaakceptuj wartość domyślną.
    - **Podsieci, zakresu adresów**: zaakceptuj wartość domyślną.
    - **Punkty końcowe usługi**: zaakceptuj wartość domyślną.

    ![Podaj podstawowe informacje o sieci wirtualnej](./media/quick-create-portal/virtual-network.png)

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Sieć wirtualna umożliwia kilka typów zasobów platformy Azure do prywatnie komunikowania się ze sobą i z Internetu. Jeden typ zasobów, które można wdrożyć w sieci wirtualnej jest maszyną wirtualną.

1. Wybierz **+ Utwórz zasób** na górnego, lewego rogu portalu Azure.
2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**.
3. Wprowadź, lub wybierz następujące informacje, a następnie wybierz **OK**:
    - **Nazwa**: *myVm1*
    - **Nazwa użytkownika**: Wprowadź nazwę użytkownika wybrane.
    - **Hasło**: Wprowadź hasło wybrane. Hasło musi mieć co najmniej 12 znaków i spełniać [zdefiniowane wymagania dotyczące złożoności](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).
    - **Subskrypcja**: Wybierz subskrypcję.
    - **Grupa zasobów**: Wybierz **Użyj istniejącego** i wybierz **myResourceGroup**.
    - **Lokalizacja**: Wybierz *wschodnie stany USA*.

    ![Podaj podstawowe informacje o maszynie wirtualnej](./media/quick-create-portal/virtual-machine-basics.png)
4. Wybierz rozmiar maszyny wirtualnej, a następnie wybierz **wybierz**.
5. W obszarze **ustawienia**, *myVirtualNetwork* powinna już być zaznaczona dla **sieci wirtualnej**, ale jeśli nie, wybierz **sieci wirtualnej** , następnie wybierz *myVirtualNetwork*. Pozostaw *domyślne* wybranego dla **podsieci**, a następnie wybierz **OK**.

    ![Wybierz sieć wirtualną](./media/quick-create-portal/virtual-machine-network-settings.png)
6. Na **Podsumowanie** wybierz pozycję **Utwórz** rozpocząć wdrażanie maszyny wirtualnej. Maszyna wirtualna zajmuje kilka minut, aby wdrożyć. 
7. Wykonaj kroki 1 – 6 ponownie, ale w kroku 3, Nazwa maszyny wirtualnej *myVm2*.

## <a name="connect-to-a-virtual-machine"></a>Połącz z maszyną wirtualną

1. Po *myVm1* jest tworzony zdalnie nawiązać z nim. W górnej części portalu Azure, wprowadź *myVm1*. Gdy **myVm1** pojawia się w wynikach wyszukiwania, wybierz go. Wybierz **Connect** przycisku.

    ![Omówienie maszyny wirtualnej](./media/quick-create-portal/virtual-machine-overview.png)

2. Po wybraniu **Connect** przycisku pliku Remote Desktop Protocol (RDP) jest tworzony i pobrana na komputer.  
3. Otwórz plik rdp pobranego. Po wyświetleniu monitu wybierz **Connect**. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny wirtualnej (musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić podczas obliczania wprowadzone poświadczenia możesz utworzono maszynę wirtualną), wybierz przycisk OK. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Wybierz **tak** lub **Kontynuuj** Aby nawiązać połączenie.

## <a name="communicate-between-vms"></a>Komunikację między maszynami wirtualnymi

1. W wierszu polecenia wprowadź `ping myvm2`. Polecenie ping nie powiedzie się, ponieważ używa polecenia ping protokołu ICMP i ICMP nie jest dozwolone przy użyciu systemu Windows, zapory, domyślnie. Aby umożliwić *myVm2* na polecenie ping *myVm1* w kolejnym kroku, wprowadź następujące polecenie w wierszu polecenia:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

2. Zamknij Podłączanie pulpitu zdalnego do *myVm1*. 

3. Wykonaj kroki [nawiązywanie połączenia z maszyną wirtualną](#connect-to-a-virtual-machine), ale nawiązać *myVm2*. W wierszu polecenia wprowadź `ping myvm1`.

    Możesz pomyślnie wykonać polecenie ping *myVm1* maszynę wirtualną z *myVm2* maszyny wirtualnej ponieważ:

    - Protokół ICMP jest dozwolone przez zaporę systemu Windows na *myVm1* maszyny wirtualnej w poprzednim kroku.
    - Domyślnie program Azure umożliwia cały ruch sieciowy między zasobami w tej samej sieci wirtualnej.

## <a name="communicate-to-the-internet"></a>Komunikować się z Internetem

1. Podczas nadal połączony *myVm2* maszyny wirtualnej, w wierszu polecenia wprowadź `ping bing.com`.

    Otrzymasz cztery odpowiedzi z bing.com. 

    Możesz pomyślnie wykonać polecenie ping z zasobu internetowego *myVm2* maszyny wirtualnej, ponieważ wszystkie maszyny wirtualne mogą komunikować się ruch wychodzący do Internetu, domyślnie.

2. Zakończenia sesji usług pulpitu zdalnego.

## <a name="communicate-from-the-internet"></a>Komunikować się z Internetu

1. Pobierz publiczny adres IP *myVm1* maszyny wirtualnej. Obraz wyświetlany w obszarze krok 1 [nawiązywanie połączenia z maszyną wirtualną](#connect-to-a-virtual-machine), zobacz publicznego adresu IP. Na ilustracji adres jest *13.90.241.247*. Adres dla maszyny wirtualnej jest inny. 

2. Z komputera, zbadaj publiczny adres IP Twojego *myVm1* maszyny wirtualnej. Polecenie ping nie powiedzie się, nawet jeśli ICMP jest otwarty przez zaporę systemu Windows.

    Polecenie ping nie powiedzie się, ponieważ cały ruch do maszyn wirtualnych systemu Windows, z wyjątkiem połączeń pulpitu zdalnego za pośrednictwem portu 3389, jest odmowa przez platformę Azure, domyślnie. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupy zasobów i wszystkie zasoby, które zawiera:

1. Wprowadź *myResourceGroup* w **wyszukiwania** pole w górnej części portalu. Po wyświetleniu **myResourceGroup** w wynikach wyszukiwania wybierz go.
2. Wybierz pozycję **Usuń grupę zasobów**.
3. Wprowadź *myResourceGroup* dla **typu nazwa grupy zasobów:** i wybierz **usunąć**.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule została wdrożona domyślna sieci wirtualnej z jedną podsiecią. Aby dowiedzieć się, jak utworzyć sieć wirtualną niestandardowe z wieloma podsieciami, nadal samouczek dotyczący tworzenia niestandardowych sieci wirtualnej.

> [!div class="nextstepaction"]
> [Tworzenie niestandardowych sieci wirtualnej](virtual-networks-create-vnet-arm-pportal.md)
