---
title: "Uzyskuj dostęp do sieci wirtualnych sieci wirtualnej komunikacji równorzędnej - portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak połączyć sieci wirtualnej z sieci wirtualnej komunikacji równorzędnej."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ac0c1033546758a77b43298a5fa8cba5f5204650
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-portal"></a>Uzyskuj dostęp do sieci wirtualnych sieci wirtualnej komunikacji równorzędnej przy użyciu portalu Azure

Sieci wirtualne można połączyć ze sobą z sieci wirtualnej komunikacji równorzędnej. Po połączyć się za pomocą sieci wirtualnych, zasobów w obie sieci wirtualne są mogły komunikować się ze sobą, z tym samym opóźnienia i przepustowości tak, jakby był zasoby w tej samej sieci wirtualnej. W tym artykule opisano tworzenie i równorzędna dwie sieci wirtualne. Omawiane kwestie:

> [!div class="checklist"]
> * Utwórz dwie sieci wirtualne
> * Utwórz komunikacji równorzędnej między sieciami wirtualnymi
> * Komunikacja równorzędna testu

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-virtual-networks"></a>Tworzenie sieci wirtualnych

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.
3. Jak pokazano na poniższej ilustracji, wprowadź *myVirtualNetwork1* dla **nazwa**, *10.0.0.0/16* dla **przestrzeni adresów**,  **myResourceGroup** dla **grupy zasobów**, *podsieć1* dla podsieci **nazwa**, 10.0.0.0/24 dla podsieci **zakres adresów** , wybierz pozycję **lokalizacji** i **subskrypcji**Zaakceptuj pozostałe wartości domyślne, a następnie wybierz **Utwórz**:

    ![Tworzenie sieci wirtualnej](./media/tutorial-connect-virtual-networks-portal/create-virtual-network.png)

4. Wykonaj kroki 1 – 3 ponownie, z następującymi zmianami:
    - **Nazwa**: *myVirtualNetwork2*
    - **Grupa zasobów**: Wybierz **Użyj istniejącego** , a następnie wybierz **myResourceGroup**.
    - **Przestrzeń adresowa**: *10.1.0.0/16*
    - **Zakres adresów podsieci**: *10.1.0.0/24*

    Prefiks adresu *myVirtualNetwork2* sieci wirtualnej nie nakłada się w przestrzeni adresowej *myVirtualNetwork1* sieci wirtualnej. Nie można elementów równorzędnych sieci wirtualnych o nakładających się przestrzeni adresowych.

## <a name="peer-virtual-networks"></a>Sieci wirtualne elementów równorzędnych

1. W polu wyszukiwania w górnej części portalu Azure, rozpocznij wpisywanie *MyVirtualNetwork1*. Gdy **myVirtualNetwork1** pojawia się w wynikach wyszukiwania, wybierz go.
2. Wybierz **komunikacji równorzędnych**w obszarze **ustawienia**, a następnie wybierz **+ Dodaj**, jak pokazano na poniższej ilustracji:

    ![Utwórz komunikacji równorzędnej](./media/tutorial-connect-virtual-networks-portal/create-peering.png)

3. Wprowadź lub wybierz informacje wyświetlane na poniższej ilustracji, a następnie wybierz **OK**. Aby wybrać *myVirtualNetwork2* sieci wirtualnej, wybierz opcję **sieci wirtualnej**, a następnie wybierz pozycję *myVirtualNetwork2*.

    ![Ustawienia komunikacji równorzędnej](./media/tutorial-connect-virtual-networks-portal/peering-settings.png)

    **RÓWNORZĘDNA stan** jest *zainicjowano*, jak pokazano na poniższej ilustracji:

    ![Stan komunikacji równorzędnej](./media/tutorial-connect-virtual-networks-portal/peering-status.png)

    Jeśli stan nie jest widoczny, Odśwież przeglądarkę.

4. Wyszukaj *myVirtualNetwork2* sieci wirtualnej. Gdy jest zwracany w wynikach wyszukiwania, zaznacz go.
5. Wykonaj kroki 1 – 3 ponownie, z następującymi zmianami, a następnie wybierz **OK**:
    - **Nazwa**: *myVirtualNetwork2 myVirtualNetwork1*
    - **Sieć wirtualna**: *myVirtualNetwork1*

    **RÓWNORZĘDNA stan** jest *połączony*. Azure również zmienić stan komunikacji równorzędnej *myVirtualNetwork2 myVirtualNetwork1* komunikacji równorzędnej z *zainicjowano* do *połączony.* Sieci wirtualnej komunikacji równorzędnej nie pełni nawiązano momentu komunikacji równorzędnej stan obie sieci wirtualne *połączony.* 

Komunikacji równorzędnych są między dwiema sieciami wirtualnymi, ale nie są przechodnie. Tak, na przykład, jeśli chcesz także elementu równorzędnego *myVirtualNetwork2* do *myVirtualNetwork3*, należy utworzyć dodatkowe komunikacji równorzędnej między sieciami wirtualnymi *myVirtualNetwork2* i *myVirtualNetwork3*. Mimo że *myVirtualNetwork1* jest połączyć za pomocą z *myVirtualNetwork2*, zasobów w ramach *myVirtualNetwork1* tylko dostęp do zasobów w  *myVirtualNetwork3* Jeśli *myVirtualNetwork1* została także połączyć za pomocą z *myVirtualNetwork3*. 

Przed równorzędna sieci wirtualnych w środowisku produkcyjnym, zalecane jest, że należy dokładnie zapoznać się z [Omówienie komunikacji równorzędnej](virtual-network-peering-overview.md), [Zarządzanie równorzędna](virtual-network-manage-peering.md), i [limity sieci wirtualnej ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Chociaż w tym artykule przedstawiono komunikacji równorzędnej między dwie sieci wirtualne w tej samej subskrypcji i lokalizacji, również elementów równorzędnych sieci wirtualnych w [różnych regionach](#register) i [różnych subskrypcji Azure](create-peering-different-subscriptions.md#portal). Można również utworzyć [gwiazdy sieci projektów](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) z komunikacji równorzędnej.

## <a name="test-peering"></a>Komunikacja równorzędna testu

Aby przetestować komunikację sieciową między maszynami wirtualnymi w różnych sieciach wirtualnych za pomocą komunikacji równorzędnej, Wdróż maszynę wirtualną w każdej podsieci, a następnie komunikować się między maszynami wirtualnymi. 

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz maszynę wirtualną w każdej sieci wirtualnej, aby móc weryfikować komunikację między nimi w kolejnym kroku.

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**. Można wybrać innego systemu operacyjnego, ale pozostałe kroki zakłada się wybrano **systemu Windows Server Datacenter 2016**. 
3. Wybierz lub wprowadź następujące informacje dotyczące **podstawy**, a następnie wybierz pozycję **OK**:
    - **Nazwa**: *myVm1*
    - **Grupa zasobów**: Wybierz **Użyj istniejącego** , a następnie wybierz *myResourceGroup*.
    - **Lokalizacja**: Wybierz *wschodnie stany USA*.

    **Nazwy użytkownika** i **hasło** wprowadzeniu są używane w kolejnym kroku. Hasło musi mieć co najmniej 12 znaków i spełniać [zdefiniowane wymagania dotyczące złożoności](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). **Lokalizacji** i **subskrypcji** wybrane musi być taka sama jak lokalizacja i sieć wirtualna jest w subskrypcji. Wybierz opcję tej samej grupy zasobów w sieci wirtualnej została utworzona, ale tej samej grupy zasobów jest zaznaczona w tym artykule nie jest wymagany.
4. Wybierz rozmiar maszyny Wirtualnej, w obszarze **wybierz rozmiar**.
5. Wybierz lub wprowadź następujące informacje dotyczące **ustawienia**, a następnie wybierz pozycję **OK**:
    - **Sieć wirtualna**: Upewnij się, że **myVirtualNetwork1** jest zaznaczone. Jeśli nie, wybierz **sieci wirtualnej** , a następnie wybierz **myVirtualNetwork1** w obszarze **sieci wirtualnej wybierz**.
    - **Podsieci**: Upewnij się, że **podsieć1** jest zaznaczone. Jeśli nie, wybierz **podsieci** , a następnie wybierz **podsieć1** w obszarze **Wybierz podsieć**, jak pokazano na poniższej ilustracji:
    
        ![Ustawienia maszyny wirtualnej](./media/tutorial-connect-virtual-networks-portal/virtual-machine-settings.png)
 
6. W obszarze **Utwórz** w **Podsumowanie**, wybierz pozycję **Utwórz** rozpocząć wdrażanie maszyny wirtualnej.
7. Ponownie wykonaj kroki 1 – 6, ale wprowadzić *myVm2* dla **nazwa** maszyny wirtualnej i wybierz *myVirtualNetwork2* dla **sieci wirtualnej**.

Azure przypisane *10.0.0.4* jako prywatnego adresu IP *myVm1* maszyny wirtualnej i 10.1.0.4 do *myVm2* maszyny wirtualnej, ponieważ zostały one pierwszy dostępny adres IP adresy w *podsieć1* z *myVirtualNetwork1* i *myVirtualNetwork2* wirtualnych sieci odpowiednio.

Maszyny wirtualne potrwać kilka minut, aby utworzyć. Nie Kontynuuj pozostałe kroki aż do utworzenia zarówno maszyn wirtualnych.

### <a name="test-virtual-machine-communication"></a>Testowanie łączności maszyny wirtualnej

1. W *wyszukiwania* w górnej części portalu pozycję Rozpocznij wpisywanie *myVm1*. Gdy **myVm1** pojawia się w wynikach wyszukiwania, wybierz go.
2. Tworzenie połączenia pulpitu zdalnego z *myVm1* maszynę wirtualną, wybierając **Connect**, jak pokazano na poniższej ilustracji:

    ![Nawiązywanie połączenia z maszyną wirtualną](./media/tutorial-connect-virtual-networks-portal/connect-to-virtual-machine.png)  

3. Aby nawiązać połączenie z maszyną Wirtualną, Otwórz pobrany plik RDP. Po wyświetleniu monitu wybierz **Connect**.
4. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny wirtualnej (musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić podczas obliczania wprowadzone poświadczenia możesz utworzono maszynę wirtualną), następnie wybierz **OK**.
5. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Wybierz **tak** Aby nawiązać połączenie.
6. W kolejnym kroku ping jest używany do komunikacji z *myVm2* maszynę wirtualną z *myVmWeb* maszyny wirtualnej. Polecenie ping używa protokołu ICMP, którego odmówiono przez zaporę systemu Windows, domyślnie. Włącz ICMP przez zaporę systemu Windows, wprowadzając następujące polecenie w wierszu polecenia:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Chociaż ping jest używany podczas testowania w tym artykule, nie zaleca się stosowanie protokołu ICMP przez zaporę systemu Windows dla wdrożeń produkcyjnych.

7. Aby nawiązać połączenie *myVm2* maszyny wirtualnej, wprowadź następujące polecenie w wierszu polecenia *myVm1* maszyny wirtualnej:

    ```
    mstsc /v:10.1.0.4
    ```
    
8. Ponieważ włączony ping *myVm1*, można teraz wywołać go za pomocą adresu IP:

    ```
    ping 10.0.0.4
    ```
    
    Otrzymasz cztery odpowiedzi. Jeśli wywołać według nazwy maszyny wirtualnej (*myVm1*), zamiast adresu IP ping nie powiedzie się, ponieważ *myVm1* jest nazwą Nieznany host. Rozpoznawanie nazw domyślne platformy Azure działa między maszynami wirtualnymi w tej samej sieci wirtualnej, ale nie między maszynami wirtualnymi w różnych sieciach wirtualnych. Rozpoznawanie nazw w sieciach wirtualnych, należy najpierw [wdrożenia serwera DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md) lub użyj [prywatnej domen usługi Azure DNS](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

9. Odłączyć swoje sesje protokołu RDP do obu *myVm1* i *myVm2*.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupy zasobów i wszystkie zasoby, które zawiera: 

1. Wprowadź *myResourceGroup* w **wyszukiwania** pole w górnej części portalu. Po wyświetleniu **myResourceGroup** w wynikach wyszukiwania wybierz go.
2. Wybierz pozycję **Usuń grupę zasobów**.
3. Wprowadź *myResourceGroup* dla **typu nazwa grupy zasobów:** i wybierz **usunąć**.

**<a name="register"></a>Zarejestrować w wersji zapoznawczej komunikacji równorzędnej globalnej sieci wirtualnej**

Łączenie sieci wirtualnych za pomocą komunikacji równorzędnej w tym samym regionie jest ogólnie dostępne. Równorzędna sieci wirtualnych w różnych regionach jest obecnie w przeglądzie. Zobacz [aktualizacje sieci wirtualnej](https://azure.microsoft.com/updates/?product=virtual-network) dla dostępnych regionów. -To-peer sieci wirtualnych w regionach, najpierw należy zarejestrować skorzystania z wersji zapoznawczej. Nie można zarejestrować za pomocą portalu, ale można zarejestrować za pomocą [PowerShell](tutorial-connect-virtual-networks-powershell.md#register) lub [interfejsu wiersza polecenia Azure](tutorial-connect-virtual-networks-cli.md#register). Próba elementów równorzędnych sieci wirtualnych w różnych regionach, przed rozpoczęciem możliwości komunikacji równorzędnej kończy się niepowodzeniem.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule przedstawiono sposób połączyć dwie sieci z sieci wirtualnej komunikacji równorzędnej.

Nadal połączyć swojego komputera do sieci wirtualnej za pośrednictwem sieci VPN i interakcji z zasobami w sieci wirtualnej lub połączyć za pomocą sieci wirtualnych.

> [!div class="nextstepaction"]
> [Połączenia komputera z siecią wirtualną](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
