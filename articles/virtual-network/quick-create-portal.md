---
title: Tworzenie sieci wirtualnej platformy Azure - Portal | Dokumentacja firmy Microsoft
description: "Dowiedz się szybko utworzyć sieć wirtualną przy użyciu portalu Azure. Sieć wirtualna umożliwia zasobów platformy Azure, takich jak maszyny wirtualne do komunikowania się przez użytkowników ze sobą i z Internetu."
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
ms.date: 03/09/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: c8f2cbe6b7377772e019a4ff90f91355ba0815ae
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/12/2018
---
# <a name="create-a-virtual-network-using-the-azure-portal"></a>Tworzenie sieci wirtualnej przy użyciu witryny Azure Portal

Sieci wirtualnej umożliwia zasobów platformy Azure, takich jak maszyn wirtualnych (VM) do prywatnie komunikowania się ze sobą i z Internetu. W tym artykule należy Dowiedz się, jak utworzyć sieć wirtualną. Po utworzeniu sieci wirtualnej, możesz wdrożyć dwóch maszyn wirtualnych w sieci wirtualnej. Następnie podłącz do jednej maszyny Wirtualnej z Internetu, a prywatnie komunikacji między dwiema maszynami wirtualnymi.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się do witryny Azure Portal pod adresem https://portal.azure.com.

## <a name="create-a-virtual-network"></a>Tworzenie sieci wirtualnej

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.
3. Wprowadź, lub wybierz poniższe informacje, zaakceptuj ustawienia domyślne dla pozostałych ustawień, a następnie wybierz **Utwórz**:

    |Ustawienie|Wartość|
    |---|---|
    |Name (Nazwa)|myVirtualNetwork|
    |Subskrypcja| Wybierz subskrypcję.|
    |Grupa zasobów| Wybierz **Utwórz nowy** , a następnie wprowadź *myResourceGroup*.|
    |Lokalizacja| Wybierz **wschodnie stany USA**.|

    ![Podaj podstawowe informacje o sieci wirtualnej](./media/quick-create-portal/create-virtual-network.png)

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz dwie maszyny wirtualne w sieci wirtualnej:

### <a name="create-the-first-vm"></a>Tworzenie pierwszej maszyny Wirtualnej

1. Wybierz **+ Utwórz zasób** na górnego, lewego rogu portalu Azure.
2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**.
3. Wprowadź, lub wybierz poniższe informacje, zaakceptuj ustawienia domyślne dla pozostałych ustawień, a następnie wybierz **OK**:

    |Ustawienie|Wartość|
    |---|---|
    |Name (Nazwa)|myVm1|
    |Nazwa użytkownika| Wprowadź nazwę użytkownika wybrane.|
    |Hasło| Wprowadź hasło wybrane. Hasło musi mieć co najmniej 12 znaków i spełniać [zdefiniowane wymagania dotyczące złożoności](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Subskrypcja| Wybierz subskrypcję.|
    |Grupa zasobów| Wybierz **Użyj istniejącego** i wybierz **myResourceGroup**.|
    |Lokalizacja| Wybierz **wschodnie stany USA**|

    ![Podstawowe informacje dotyczące maszyny wirtualnej](./media/quick-create-portal/virtual-machine-basics.png)

4. Wybierz rozmiar maszyny wirtualnej, a następnie wybierz **wybierz**.
5. W obszarze **ustawienia**, Zaakceptuj wszystkie ustawienia domyślne, a następnie wybierz **OK**.

    ![Ustawienia maszyny wirtualnej](./media/quick-create-portal/virtual-machine-settings.png)

6. W obszarze **Utwórz** z **Podsumowanie**, wybierz pozycję **Utwórz** rozpocząć wdrażanie maszyny Wirtualnej. Maszyna wirtualna ma kilka minut, aby wdrożyć. 

### <a name="create-the-second-vm"></a>Tworzenie drugiej maszyny Wirtualnej

Wykonaj kroki 1 – 6 ponownie, ale w kroku 3, Nazwa maszyny Wirtualnej *myVm2*.

## <a name="connect-to-a-vm-from-the-internet"></a>Połączenie z maszyną wirtualną z Internetu

1. Po *myVm1* jest utworzony, się z nim połączyć. W górnej części portalu Azure, wprowadź *myVm1*. Gdy **myVm1** pojawia się w wynikach wyszukiwania, wybierz go. Wybierz **Connect** przycisku.

    ![Połącz z maszyną wirtualną](./media/quick-create-portal/connect-to-virtual-machine.png)

2. Po wybraniu **Connect** przycisku pliku Remote Desktop Protocol (RDP) jest tworzony i pobrana na komputer.  
3. Otwórz plik rdp pobranego. Po wyświetleniu monitu wybierz **Connect**. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny Wirtualnej. Musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić poświadczenia zostały wprowadzone podczas tworzenia maszyny Wirtualnej. 
4. Kliknij przycisk **OK**.
5. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Jeśli zostanie wyświetlone ostrzeżenie, wybierz **tak** lub **Kontynuuj**, aby nawiązać połączenie.

## <a name="communicate-privately-between-vms"></a>Prywatnie komunikacji między maszynami wirtualnymi

1. Z programu PowerShell, wprowadź `ping myvm2`. Polecenie ping nie powiedzie się, ponieważ używa polecenia ping protokołu komunikacyjnego sterowania Internetem (ICMP) i ICMP nie jest dozwolona przez zaporę systemu Windows, domyślnie.
2. Aby umożliwić *myVm2* na polecenie ping *myVm1* w kolejnym kroku, wprowadź następujące polecenie z programu PowerShell, który umożliwia ICMP dla ruchu przychodzącego przez zaporę systemu Windows:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

3. Zamknij Podłączanie pulpitu zdalnego do *myVm1*. 

4. Wykonaj kroki [połączenie z maszyną wirtualną z Internetu](#connect-to-a-vm-from-the-internet) ponownie nawiązać połączenie, ale *myVm2*. W wierszu polecenia wprowadź `ping myvm1`.

    Pojawi się odpowiedzi od *myVm1*, ponieważ dozwolone ICMP przez zaporę systemu Windows na *myVm1* maszyny Wirtualnej w poprzednim kroku.

5. Zamknij Podłączanie pulpitu zdalnego do *myVm2*.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupy zasobów i wszystkie zasoby, które zawiera:

1. Wprowadź *myResourceGroup* w **wyszukiwania** pole w górnej części portalu. Po wyświetleniu **myResourceGroup** w wynikach wyszukiwania wybierz go.
2. Wybierz pozycję **Usuń grupę zasobów**.
3. Wprowadź *myResourceGroup* dla **typu nazwa grupy zasobów:** i wybierz **usunąć**.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule tworzone są domyślne sieci wirtualnej i dwie maszyny wirtualne. Połączone z jedną maszynę Wirtualną z Internetu i prywatnie przekazywane między maszyny Wirtualnej i inną maszynę Wirtualną. Aby dowiedzieć się więcej na temat ustawień sieci wirtualnej, zobacz [Zarządzanie sieci wirtualnej](manage-virtual-network.md).

Domyślnie program Azure umożliwia nieograniczony prywatnej komunikacji między maszynami wirtualnymi, ale tylko maszyn wirtualnych systemu Windows umożliwia przychodzących połączeń pulpitu zdalnego z Internetu. Aby dowiedzieć się ograniczyć różnego rodzaju komunikacji sieciowej do i z maszyn wirtualnych, przejdź do następnego samouczka.

> [!div class="nextstepaction"]
> [Filtrowanie ruchu sieciowego](virtual-networks-create-nsg-arm-pportal.md)