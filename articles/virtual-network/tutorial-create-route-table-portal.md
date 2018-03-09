---
title: "Kierować ruchem sieciowym - portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można kierować ruchem sieciowym z tabelą tras za pomocą portalu Azure."
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/05/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 4cc814e1fd3ee8979662695f9dec3dcce335dc2a
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Kierować ruchem sieciowym z tabelą tras za pomocą portalu Azure

Azure automatycznie tras ruchu między wszystkich podsieci w sieci wirtualnej, domyślnie. Można utworzyć trasy do przesłonięcia Azure routing domyślny. Możliwość tworzenia niestandardowych tras jest przydatne, jeśli na przykład chcesz kierować ruchem między podsieciami przez zaporę. W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz tabelę tras
> * Utwórz trasę
> * Skojarz tabelę tras z podsiecią sieci wirtualnej
> * Routing testu
> * Rozwiązywanie problemów z routingiem

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-route-table"></a>Utwórz tabelę tras

Azure tras ruchu między wszystkich podsieci w sieci wirtualnej, domyślnie. Aby dowiedzieć się więcej na temat trasy domyślne platformy Azure, zobacz [tras systemowych](virtual-networks-udr-overview.md). Aby zastąpić domyślny Azure routingu, Utwórz tabelę tras, która zawiera trasy i skojarz tabeli tras z podsiecią sieci wirtualnej.

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **tabeli tras**.
3. Wybierz użytkownika **subskrypcji** i wybierz lub wprowadź następujące informacje, a następnie wybierz **Utwórz**:
 
    ![Utwórz tabelę tras](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Utwórz trasę

Tabela tras zawiera zero lub więcej trasy. 

1. W *wyszukiwania zasobów, usługi i dokumenty* w górnej części portalu pozycję Rozpocznij wpisywanie *myRouteTablePublic*. Gdy **myRouteTablePublic** pojawia się w wynikach wyszukiwania, wybierz go.
2. W obszarze **ustawienia**, wybierz pozycję **tras** , a następnie wybierz **+ Dodaj**, jak pokazano na poniższej ilustracji:

    ![Dodaj trasę](./media/tutorial-create-route-table-portal/add-route.png) 
 
4. W obszarze **Dodaj trasę**, wybierz lub wprowadź następujące informacje, a następnie wybierz **OK**:
    - **Nazwa trasy**: *ToPrivateSubnet*
    - **Prefiks adresu**: 10.0.1.0/24
    - **Następnego przeskoku typu**: Wybierz **urządzenie wirtualne**.
    - **Następnego przeskoku**: 10.0.2.4

    Cały ruch kierowany do prefiksu adresu 10.0.1.0/24 przez urządzenie wirtualne sieci przy użyciu adresu IP 10.0.2.4 kieruje trasy. Urządzenie wirtualne sieci i podsieci z prefiksem adresu określonego są tworzone w kolejnych krokach. Trasy zastępuje domyślne Azure routingu, który kieruje ruchem między podsieciami bezpośrednio. Każda marszruta Określa typ następnego przeskoku. Typ następnego przeskoku nakazuje Azure sposób kierowania ruchu. W tym przykładzie Typ następnego przeskoku jest *VirtualAppliance*. Aby dowiedzieć się więcej na temat wszystkich dostępnych typów następnego przeskoku platformy Azure i kiedy należy używać ich, zobacz [następnego przeskoku typy](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Skojarz tabelę tras z podsiecią

Aby móc skojarzyć tabelę tras z podsiecią, należy utworzyć sieć wirtualną i podsieć:

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.
3. W obszarze **Utwórz sieć wirtualną**, wybrać, lub wprowadź następujące informacje, a następnie wybierz **Utwórz**:
    
    - **Nazwa**: *myVirtualNetwork*
    - **Przestrzeń adresowa**: *10.0.0.0/16*
    - **Subskrypcja**: Wybierz subskrypcję.
    - **Grupa zasobów**: Wybierz **Użyj istniejącego** i wybierz **myResourceGroup**.
    - **Lokalizacja**: Wybierz *wschodnie stany USA*
    - **Nazwa podsieci**: *publiczny*
    - **Zakres adresów:** *10.0.0.0/24*

4. W **wyszukiwania zasobów, usługi i dokumenty** w górnej części portalu pozycję Rozpocznij wpisywanie *myVirtualNetwork*. Gdy **myVirtualNetwork** pojawia się w wynikach wyszukiwania, wybierz go.
5. Dodaj dwa dodatkowe podsieci do sieci wirtualnej. W obszarze **ustawienia**, wybierz pozycję **podsieci** , a następnie wybierz **+ podsieci**, jak pokazano na poniższej ilustracji:

    ![Dodawanie podsieci](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Wybierz lub wprowadź następujące informacje, a następnie wybierz **OK**:
    - **Nazwa**: prywatne
    - **Przestrzeń adresowa**: *10.0.1.0/24*

7. Wykonaj kroki 5 i 6 ponownie, podając następujące informacje:
    - **Nazwa**: DMZ
    - **Przestrzeń adresowa**: *10.0.2.0/24*

Możesz skojarzyć tabelę tras na zero lub więcej podsieci. Podsieć może mieć zero lub jedną tabelę tras skojarzony. Ruch wychodzący z podsieci jest kierowany na podstawie trasy domyślne platformy Azure i niestandardowych tras, dodane do tabeli tras, który należy powiązać z podsiecią. Skojarz *myRouteTablePublic* tabeli tras do *publicznego* podsieci:

1. **MyVirtualNetwork - podsieci** pojawia się po ukończeniu poprzedniego kroku. W obszarze **ustawienia**, wybierz pozycję **podsieci** , a następnie wybierz **publicznego**.
2. Jak pokazano na poniższej ilustracji, wybierz **tabeli tras**, a następnie wybierz pozycję **MyRouteTablePublic**.

    ![Skojarz tabeli tras](./media/tutorial-create-route-table-portal/associate-route-table.png) 

3. Wybierz pozycję **Zapisz**.

## <a name="test-routing"></a>Routing testu

Aby przetestować routingu, należy utworzyć maszynę wirtualną, która służy jako urządzenie wirtualne sieci, że trasy, który został utworzony w poprzednim kroku kieruje za pośrednictwem. Po utworzeniu urządzenie wirtualne sieci, będzie wdrożyć maszynę wirtualną do *publicznego* i *prywatnej* podsieci. Następnie będzie kierować ruch z *publicznego* podsieci do *prywatnej* podsieci za pośrednictwem sieci urządzenia wirtualnego.

### <a name="create-a-network-virtual-appliance"></a>Utwórz urządzenie sieci wirtualnej

W poprzednim kroku utworzono trasę określone urządzenie wirtualne sieci jako typ następnego przeskoku. Maszynę wirtualną działającą aplikację sieciową jest często określany jako urządzenie wirtualne sieci. W środowiskach produkcyjnych, którą należy wdrożyć urządzenie wirtualne sieci jest często wstępnie skonfigurowane maszyny wirtualnej. Wiele sieci wirtualnych urządzeń są dostępne z [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). W tym artykule podstawowa maszyna wirtualna jest utworzona.

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**. Można wybrać innego systemu operacyjnego, ale pozostałe kroki zakłada się wybrano **systemu Windows Server Datacenter 2016**. 
3. Wybierz lub wprowadź następujące informacje dotyczące **podstawy**, a następnie wybierz pozycję **OK**:
    - **Nazwa**: *myVmNva*
    - **Grupa zasobów**: Wybierz **Użyj istniejącego** , a następnie wybierz *myResourceGroup*.
    - **Lokalizacja**: Wybierz *wschodnie stany USA*.

    **Nazwy użytkownika** i **hasło** wprowadzeniu są używane w kolejnym kroku. Hasło musi mieć co najmniej 12 znaków i spełniać [zdefiniowane wymagania dotyczące złożoności](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm). **Lokalizacji** i **subskrypcji** wybrane musi być taka sama jak lokalizacja i sieć wirtualna jest w subskrypcji. Wybierz opcję tej samej grupy zasobów w sieci wirtualnej została utworzona, ale tej samej grupy zasobów jest zaznaczona w tym samouczku nie jest wymagany.
4. Wybierz rozmiar maszyny Wirtualnej, w obszarze **wybierz rozmiar**.
5. Wybierz lub wprowadź następujące informacje dotyczące **ustawienia**, a następnie wybierz pozycję **OK**:
    - **Sieć wirtualna**: Upewnij się, że **myVirtualNetwork** jest zaznaczone. Jeśli nie, wybierz **sieci wirtualnej**, a następnie wybierz pozycję **myVirtualNetwork** w obszarze **sieci wirtualnej wybierz**.
    - **Podsieci**: Wybierz **podsieci** , a następnie wybierz **DMZ** w obszarze **Wybierz podsieć**.
    - **Publiczny adres IP**: Wybierz **publicznego adresu IP** i wybierz **Brak** w obszarze **wybierz publiczny adres IP**. Nie publicznego adresu IP nie przypisano do tej maszyny wirtualnej nie należy łączyć się z Internetem.
6. W obszarze **Utwórz** w **Podsumowanie**, wybierz pozycję **Utwórz** rozpocząć wdrażanie maszyny wirtualnej.

Maszyna wirtualna ma kilka minut na utworzenie. Nie należy kontynuować do następnego kroku, dopóki Azure zakończy tworzenie maszyny wirtualnej i otwiera okno informacje o maszynie wirtualnej.

Podczas tworzenia maszyny wirtualnej Azure ono również utworzone [interfejsu sieciowego](virtual-network-network-interface.md) w *DMZ* podsieci i dołączyć interfejsu sieciowego z maszyną wirtualną. Przesyłanie dalej IP musi być włączony dla każdego interfejsu sieci platformy Azure, który przesyła dalej ruch kierowany do dowolnego adresu IP, który nie jest przypisany do interfejsu sieciowego.

1. W polu otwarty dla maszyny wirtualnej po jej utworzeniu, w obszarze **ustawienia**, wybierz pozycję **sieci**, a następnie wybierz **myvmnva158** (interfejs sieciowy Azure utworzone dla maszyny wirtualnej ma inną liczbę po **myvmnva**), jak pokazano na poniższej ilustracji:

    ![Sieć maszyny wirtualnej](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

    Podczas tworzenia urządzenia wirtualnego sieci w *DMZ* podsieci, Azure automatycznie utworzony interfejs sieciowy, dołączyć interfejsu sieciowego z maszyną wirtualną i przypisany prywatny adres IP  *10.0.2.4* do interfejsu sieciowego. 

2. W obszarze **ustawienia**, wybierz pozycję **konfiguracje adresów IP**, wybierz pozycję **włączone** dla **przesyłanie dalej IP**, a następnie wybierz **Zapisz** , jak pokazano na poniższej ilustracji:

    ![Włącz przesyłanie dalej IP](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

### <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz dwie maszyny wirtualne w sieci wirtualnej, aby móc weryfikować tego ruchu z *publicznego* podsieci jest kierowany do *prywatnej* podsieci za pośrednictwem sieci urządzenia wirtualnego w kolejnym kroku.

Wykonaj kroki 1 – 6 z [utworzyć urządzenie wirtualne sieci](#create-a-network-virtual-appliance). Użyj tych samych ustawień w kroku 3 i 5, z wyjątkiem następujących zmian:

|Maszyna wirtualna   |Name (Nazwa)      |Podsieć      | Publiczny adres IP     |
|---------         |--------- | -----------|---------              |
|Maszyna wirtualna 1 | myVmWeb  | Publiczne     | Zaakceptuj domyślną portalu |
|Maszyna wirtualna 2 | myVmMgmt | Prywatne    | Zaakceptuj domyślną portalu |

Można utworzyć *myVmMgmt* maszyny wirtualnej, podczas gdy platforma Azure tworzy *myVmWeb* maszyny wirtualnej. Nie należy kontynuować następujące kroki, dopóki platforma Azure zakończy tworzenie obydwie maszyny wirtualne.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>Kierować ruchem przez urządzenie wirtualne sieci

1. W *wyszukiwania* w górnej części portalu pozycję Rozpocznij wpisywanie *myVmMgmt*. Gdy **myVmMgmt** pojawia się w wynikach wyszukiwania, wybierz go.
2. Tworzenie połączenia pulpitu zdalnego z *myVmMgmt* maszynę wirtualną, wybierając **Connect**, jak pokazano na poniższej ilustracji:

    ![Nawiązywanie połączenia z maszyną wirtualną](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Aby nawiązać połączenie z maszyną Wirtualną, Otwórz pobrany plik RDP. Po wyświetleniu monitu wybierz **Connect**.
4. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny wirtualnej (musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić podczas obliczania wprowadzone poświadczenia możesz utworzono maszynę wirtualną), następnie wybierz **OK**.
5. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Wybierz **tak** Aby nawiązać połączenie.
6. W kolejnym kroku polecenia tracert.exe służy do testowania, routingu. Tracert korzysta z protokołu ICMP, którego odmówiono przez zaporę systemu Windows. Włącz ICMP przez zaporę systemu Windows, wprowadzając następujące polecenie w wierszu polecenia:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

    Chociaż tracert służy do testowania routingu w tym artykule, nie zaleca się stosowanie protokołu ICMP przez zaporę systemu Windows dla wdrożeń produkcyjnych.
7. Włączone przesyłanie dalej IP w obrębie platformy Azure dla interfejsu sieciowego maszyny wirtualnej w [fowarding włączyć IP](#enable-ip-forwarding). Na maszynie wirtualnej systemu operacyjnego lub aplikacji działających w maszynie wirtualnej muszą także do przesyłania dalej ruchu sieciowego. Podczas wdrażania urządzenia wirtualnego sieci w środowisku produkcyjnym urządzenia zwykle filtry, dzienniki lub wykonuje innej funkcji przed przesłaniem ruchu. W tym artykule, system operacyjny po prostu przekazuje cały ruch, który odbiera. Włącz przesyłanie dalej IP w ramach systemu operacyjnego *myVmNva* , wykonując kroki opisane w *myVmMgmt* maszyny wirtualnej:

    Usługi pulpitu zdalnego *myVmNva* maszyny wirtualnej za pomocą następującego polecenia w wierszu polecenia:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Aby włączyć IP przekazywania w ramach systemu operacyjnego, wprowadź następujące polecenie w programie PowerShell:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Uruchom ponownie maszynę wirtualną, która spowoduje także odłączenie sesji usług pulpitu zdalnego.
8. Podczas nadal połączony *myVmMgmt* maszyny wirtualnej, po *myVmNva* ponownego uruchamiania maszyny wirtualnej, utwórz sesję pulpitu zdalnego do *myVmWeb* maszyny wirtualnej za pomocą następującego polecenia:

    ``` 
    mstsc /v:myVmWeb
    ```
    
    Włącz ICMP przez zaporę systemu Windows, wprowadzając następujące polecenie w wierszu polecenia:

    ```
    netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
    ```

9. Aby przetestować routingu ruchu sieciowego do *myVmMgmt* maszynę wirtualną z *myVmWeb* maszyny wirtualnej, wprowadź następujące polecenie w wierszu polecenia:

    ```
    tracert myvmmgmt
    ```

    Odpowiedź jest podobny do poniższego przykładu:
    
    ```
    Tracing route to myvmmgmt.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    Widać, że pierwszym przeskoku jest 10.0.2.4, który jest prywatny adres IP urządzenia wirtualnego w sieci. Drugi przeskok jest 10.0.1.4, prywatnego adresu IP *myVmMgmt* maszyny wirtualnej. Trasy dodane do *myRouteTablePublic* tabeli tras i powiązanych z *publicznego* podsieci spowodował Azure, aby kierować ruchem przez analizę NVA, a nie bezpośrednio do *prywatnego* podsieci.
10.  Zamykanie sesji usług pulpitu zdalnego do *myVmWeb* maszyny wirtualnej, co spowoduje pozostawienie połączenie *myVmMgmt* maszyny wirtualnej.
11. Aby przetestować routingu ruchu sieciowego do *myVmWeb* maszynę wirtualną z *myVmMgmt* maszyny wirtualnej, wprowadź następujące polecenie w wierszu polecenia:

    ```
    tracert myvmweb
    ```

    Odpowiedź jest podobny do poniższego przykładu:

    ```
    Tracing route to myvmweb.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Widać, że ruch jest kierowany bezpośrednio z *myVmMgmt* maszyny wirtualnej do *myVmWeb* maszyny wirtualnej. Domyślnie ruch trasy Azure bezpośrednio między podsieciami.
12. Zamykanie sesji usług pulpitu zdalnego do *myVmMgmt* maszyny wirtualnej.

## <a name="troubleshoot-routing"></a>Rozwiązywanie problemów z routingiem

Jak przedstawiono w poprzednich krokach Azure stosuje trasy domyślne, które można opcjonalnie zastąpić własnego trasy. Czasami ruchu mogą być kierowane zgodnie z oczekiwaniami, należy. Można użyć [następnego przeskoku](../network-watcher/network-watcher-check-next-hop-portal.md) możliwość obserwatora sieciowego Azure określają sposób Azure routingu ruchu między dwiema maszynami wirtualnymi. 

1. W *wyszukiwania* w górnej części portalu pozycję Rozpocznij wpisywanie *obserwatora sieciowego*. Gdy **obserwatora sieciowego** pojawia się w wynikach wyszukiwania, wybierz go.
2. W obszarze **narzędzia diagnostyczne sieci**, wybierz pozycję **następnego przeskoku**.
3. Aby przetestować routingu ruchu z *myVmWeb* (10.0.0.4) maszyny wirtualnej do *myVmMgmt* (10.0.1.4) maszyny wirtualnej, wybierz subskrypcję, wprowadź informacje wyświetlane na poniższej ilustracji (sieci **Interfejsu sieciowego** nazwa różni się nieznacznie), a następnie wybierz **następnego przeskoku**:

    ![Następny przeskok](./media/tutorial-create-route-table-portal/next-hop.png)  

    **Wynik** dane wyjściowe informuje, że następnego przeskoku adresu IP dla ruchu z *myVmWeb* do *myVmMgmt* jest 10.0.2.4 ( *myVmNva* Maszyna wirtualna), czy typ następnego przeskoku jest *VirtualAppliance*, oraz że tabeli tras, która powoduje występowanie routingu jest *myRouteTablePublic*.

Skuteczne trasy dla każdego interfejsu sieciowego są kombinacją trasy domyślne platformy Azure i wszelkie tras zdefiniowanych. Aby wyświetlić wszystkie trasy dla karty sieciowej na maszynie wirtualnej, wykonaj następujące kroki:

1. W *wyszukiwania* w górnej części portalu pozycję Rozpocznij wpisywanie *myVmWeb*. Gdy **myVmWeb** pojawia się w wynikach wyszukiwania, wybierz go.
2. W obszarze **ustawienia**, wybierz pozycję **sieci**, a następnie wybierz **myvmweb369** (interfejs sieciowy Azure utworzony dla maszyny wirtualnej ma inną liczbę po **myvmweb**).
3. W obszarze **pomocy technicznej i rozwiązywania problemów**, wybierz pozycję **skuteczne tras**, jak pokazano na poniższej ilustracji:

    ![Skuteczne tras](./media/tutorial-create-route-table-portal/effective-routes.png) 

    Zobacz Azure tras domyślnych i trasy dodane w *myRouteTablePublic* tabeli tras. Aby dowiedzieć się więcej na temat sposobu Azure wybiera trasę z listy tras, zobacz [jak Azure wybiera trasę](virtual-networks-udr-overview.md#how-azure-selects-a-route).

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupy zasobów i wszystkie zasoby, które zawiera: 

1. Wprowadź *myResourceGroup* w **wyszukiwania** pole w górnej części portalu. Po wyświetleniu **myResourceGroup** w wynikach wyszukiwania wybierz go.
2. Wybierz pozycję **Usuń grupę zasobów**.
3. Wprowadź *myResourceGroup* dla **typu nazwa grupy zasobów:** i wybierz **usunąć**.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule możesz utworzyć tabelę tras i skojarzone go do podsieci. Utworzono sieci urządzenie wirtualne, które kierowany ruch z publicznego podsieci prywatne podsieci. Podczas wdrażania można wielu zasobów platformy Azure w ramach sieci wirtualnej, zasobów w przypadku niektórych usług Azure PaaS nie można wdrożyć w sieci wirtualnej. Możesz nadal ograniczyć dostęp do zasobów pewnych usług Azure PaaS ruch tylko z podsieci sieci wirtualnej jednak. Przejdź do następnego samouczkiem, aby dowiedzieć się, jak ograniczyć dostęp do sieci Azure PaaS zasobów.

> [!div class="nextstepaction"]
> [Ograniczenie dostępu do sieci do PaaS zasobów](virtual-network-service-endpoints-configure.md#azure-portal)
