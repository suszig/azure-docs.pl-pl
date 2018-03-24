---
title: Kierować ruchem sieciowym - portalu Azure | Dokumentacja firmy Microsoft
description: Dowiedz się, jak można kierować ruchem sieciowym z tabelą tras za pomocą portalu Azure.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 980cf7b59ed16778bbb6cd1b657e3522407c79c9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/23/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-portal"></a>Kierować ruchem sieciowym z tabelą tras za pomocą portalu Azure

Azure automatycznie tras ruchu między wszystkich podsieci w sieci wirtualnej, domyślnie. Można utworzyć trasy do przesłonięcia Azure routing domyślny. Możliwość tworzenia niestandardowych tras jest przydatne, jeśli na przykład chcesz kierować ruchem między podsieciami przez urządzenie wirtualne sieci (NVA). W tym artykule dowiesz się, jak:

> [!div class="checklist"]
> * Utwórz tabelę tras
> * Utwórz trasę
> * Tworzenie sieci wirtualnej z wieloma podsieciami
> * Skojarz tabelę tras z podsiecią
> * Utwórz NVA, który przekierowuje ruch
> * Wdrażanie maszyn wirtualnych (VM) w różnych podsieciach
> * Kierować ruchem z jednej podsieci do drugiego za pomocą NVA

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure. 

Zaloguj się do portalu Azure pod adresem http://portal.azure.com.

## <a name="create-a-route-table"></a>Utwórz tabelę tras

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **tabeli tras**.
3. Wprowadź, lub wybierz poniższe informacje, zaakceptuj wartość domyślną dla pozostałe ustawienia, a następnie wybierz **Utwórz**:

    |Ustawienie|Wartość|
    |---|---|
    |Name (Nazwa)|myRouteTablePublic|
    |Subskrypcja| Wybierz subskrypcję.|
    |Grupa zasobów | Wybierz **Utwórz nowy** , a następnie wprowadź *myResourceGroup*.|
    |Lokalizacja|Wschodnie stany USA|
 
    ![Utwórz tabelę tras](./media/tutorial-create-route-table-portal/create-route-table.png) 

## <a name="create-a-route"></a>Utwórz trasę

1. W *wyszukiwania zasobów, usługi i dokumenty* w górnej części portalu pozycję Rozpocznij wpisywanie *myRouteTablePublic*. Gdy **myRouteTablePublic** pojawia się w wynikach wyszukiwania, wybierz go.
2. W obszarze **ustawienia**, wybierz pozycję **tras** , a następnie wybierz **+ Dodaj**, jak pokazano na poniższej ilustracji:

    ![Dodaj trasę](./media/tutorial-create-route-table-portal/add-route.png) 
 
3. W obszarze **Dodaj trasę**wprowadź, lub wybierz poniższe informacje, zaakceptuj wartość domyślną dla pozostałych ustawień, a następnie wybierz **Utwórz**:

    |Ustawienie|Wartość|
    |---|---|
    |Nazwa trasy|ToPrivateSubnet|
    |Prefiks adresu| 10.0.1.0/24|
    |Typ następnego skoku | Wybierz **urządzenie wirtualne**.|
    |Adres następnego skoku| 10.0.2.4|

## <a name="associate-a-route-table-to-a-subnet"></a>Skojarz tabelę tras z podsiecią

Aby móc skojarzyć tabelę tras z podsiecią, należy utworzyć sieć wirtualną i podsieć, a następnie tabelę tras z podsiecią można skojarzyć:

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz **sieci**, a następnie wybierz **sieci wirtualnej**.
3. W obszarze **Utwórz sieć wirtualną**wprowadź, lub wybierz poniższe informacje, zaakceptuj wartość domyślną dla pozostałych ustawień, a następnie wybierz **Utwórz**:

    |Ustawienie|Wartość|
    |---|---|
    |Name (Nazwa)|myVirtualNetwork|
    |Przestrzeń adresowa| 10.0.0.0/16|
    |Subskrypcja | Wybierz subskrypcję.|
    |Grupa zasobów|Wybierz **Użyj istniejącego** , a następnie wybierz **myResourceGroup**.|
    |Lokalizacja|Wybierz *wschodnie stany USA*|
    |Nazwa podsieci|Publiczne|
    |Zakres adresów|10.0.0.0/24|
    
4. W **wyszukiwania zasobów, usługi i dokumenty** w górnej części portalu pozycję Rozpocznij wpisywanie *myVirtualNetwork*. Gdy **myVirtualNetwork** pojawia się w wynikach wyszukiwania, wybierz go.
5. W obszarze **ustawienia**, wybierz pozycję **podsieci** , a następnie wybierz **+ podsieci**, jak pokazano na poniższej ilustracji:

    ![Dodawanie podsieci](./media/tutorial-create-route-table-portal/add-subnet.png) 

6. Wybierz lub wprowadź następujące informacje, a następnie wybierz **OK**:

    |Ustawienie|Wartość|
    |---|---|
    |Name (Nazwa)|Prywatne|
    |Przestrzeń adresowa| 10.0.1.0/24|

7. Wykonaj kroki 5 i 6 ponownie, podając następujące informacje:

    |Ustawienie|Wartość|
    |---|---|
    |Name (Nazwa)|DMZ|
    |Przestrzeń adresowa| 10.0.2.0/24|

8. **MyVirtualNetwork - podsieci** pojawia się po ukończeniu poprzedniego kroku. W obszarze **ustawienia**, wybierz pozycję **podsieci** , a następnie wybierz **publicznego**.
9. Jak pokazano na poniższej ilustracji, wybierz **tabeli tras**, wybierz pozycję **MyRouteTablePublic**, a następnie wybierz **zapisać**:

    ![Skojarz tabeli tras](./media/tutorial-create-route-table-portal/associate-route-table.png) 

## <a name="create-an-nva"></a>Utwórz NVA

NVA jest maszynę Wirtualną, która pełni funkcję sieci, takich jak routing, zapory lub Optymalizacja sieci WAN.

1. Wybierz **+ Utwórz zasób** w górnym lewym rogu portalu Azure.
2. Wybierz pozycję **Wystąpienia obliczeniowe**, a następnie wybierz pozycję **Windows Server 2016 Datacenter**. Można wybrać innego systemu operacyjnego, ale pozostałe kroki zakłada się wybrano **systemu Windows Server Datacenter 2016**. 
3. Wybierz lub wprowadź następujące informacje dotyczące **podstawy**, a następnie wybierz pozycję **OK**:

    |Ustawienie|Wartość|
    |---|---|
    |Name (Nazwa)|myVmNva|
    |Nazwa użytkownika|Wprowadź nazwę użytkownika wybrane.|
    |Hasło|Wprowadź hasło wybrane. Hasło musi mieć co najmniej 12 znaków i spełniać [zdefiniowane wymagania dotyczące złożoności](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
    |Grupa zasobów| Wybierz **Użyj istniejącego** , a następnie wybierz *myResourceGroup*.|
    |Lokalizacja|Wybierz **wschodnie stany USA**.|
4. Wybierz rozmiar maszyny Wirtualnej, w obszarze **wybierz rozmiar**.
5. Wybierz lub wprowadź następujące informacje dotyczące **ustawienia**, a następnie wybierz pozycję **OK**:

    |Ustawienie|Wartość|
    |---|---|
    |Sieć wirtualna|myVirtualNetwork — Jeśli nie jest zaznaczona, zaznacz **sieci wirtualnej**, a następnie wybierz pozycję **myVirtualNetwork** w obszarze **sieci wirtualnej wybierz**.|
    |Podsieć|Wybierz **podsieci** , a następnie wybierz **DMZ** w obszarze **Wybierz podsieć**. |
    |Publiczny adres IP| Wybierz **publicznego adresu IP** i wybierz **Brak** w obszarze **wybierz publiczny adres IP**. Nie publicznego adresu IP nie przypisano do tej maszyny Wirtualnej nie należy łączyć się z Internetem.
6. W obszarze **Utwórz** w **Podsumowanie**, wybierz pozycję **Utwórz** rozpocząć wdrażanie maszyny Wirtualnej.

    Maszyna wirtualna ma kilka minut na utworzenie. Nie należy kontynuować do następnego kroku, dopóki Azure zakończy tworzenie maszyny Wirtualnej i otwiera okno informacje o Maszynie wirtualnej.

7. W polu otwarty dla maszyny Wirtualnej po jej utworzeniu, w obszarze **ustawienia**, wybierz pozycję **sieci**, a następnie wybierz **myvmnva158** (Azure utworzonych dla interfejsu sieciowego z Maszyna wirtualna ma inną liczbę po **myvmnva**), jak pokazano na poniższej ilustracji:

    ![Sieć maszyn wirtualnych](./media/tutorial-create-route-table-portal/virtual-machine-networking.png) 

8. Dla karty sieciowej można było przesłać wysyłane do niego, ruch sieciowy, który nie jest przeznaczone do adresu IP, przesyłanie dalej IP musi być włączony dla interfejsu sieciowego. W obszarze **ustawienia**, wybierz pozycję **konfiguracje adresów IP**, wybierz pozycję **włączone** dla **przesyłanie dalej IP**, a następnie wybierz **Zapisz** , jak pokazano na poniższej ilustracji:

    ![Włącz przesyłanie dalej IP](./media/tutorial-create-route-table-portal/enable-ip-forwarding.png) 

## <a name="create-virtual-machines"></a>Tworzenie maszyn wirtualnych

Utwórz dwie maszyny wirtualne w sieci wirtualnej, dzięki czemu można zweryfikować tego ruchu z *publicznego* podsieci jest kierowany do *prywatnej* podsieci za pośrednictwem NVA w kolejnym kroku. Wykonaj kroki 1 – 6 z [utworzyć NVA](#create-a-network-virtual-appliance). Użyj tych samych ustawień w kroku 3 i 5, z wyjątkiem następujących zmian:

|Nazwa maszyny wirtualnej      |Podsieć      | Publiczny adres IP     |
|--------- | -----------|---------              |
| myVmPublic  | Publiczne     | Zaakceptuj domyślną portalu |
| myVmPrivate | Prywatne    | Zaakceptuj domyślną portalu |

Można utworzyć *myVmPrivate* maszyny Wirtualnej, podczas gdy platforma Azure tworzy *myVmPublic* maszyny Wirtualnej. Nie należy kontynuować następujące kroki, dopóki platforma Azure zakończy tworzenie obie maszyny wirtualne.

## <a name="route-traffic-through-an-nva"></a>Kierować ruchem za pośrednictwem NVA

1. W *wyszukiwania* w górnej części portalu pozycję Rozpocznij wpisywanie *myVmPrivate*. Gdy **myVmPrivate** maszyny Wirtualnej jest wyświetlana w wynikach wyszukiwania wybierz go.
2. Tworzenie połączenia pulpitu zdalnego z *myVmPrivate* maszyny Wirtualnej, wybierając **Connect**, jak pokazano na poniższej ilustracji:

    ![Łączenie z maszyną wirtualną ](./media/tutorial-create-route-table-portal/connect-to-virtual-machine.png)  

3. Aby nawiązać połączenie z maszyną Wirtualną, Otwórz pobrany plik RDP. Po wyświetleniu monitu wybierz **Connect**.
4. Wprowadź nazwę użytkownika i hasło określone podczas tworzenia maszyny Wirtualnej (musisz wybrać **więcej opcji**, następnie **korzystała z innego konta**, aby określić poświadczenia zostały wprowadzone podczas tworzenia maszyny Wirtualnej), następnie wybierz **OK**.
5. Podczas procesu logowania może pojawić się ostrzeżenie o certyfikacie. Wybierz **tak** Aby nawiązać połączenie.
6. W kolejnym kroku polecenia tracert.exe służy do testowania, routingu. Tracert używa kontrolki komunikat protokołu protokołu ICMP (Internet), której odmówiono przez zaporę systemu Windows. Włącz ICMP przez zaporę systemu Windows, wprowadzając następujące polecenie z programu PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Chociaż tracert służy do testowania routingu w tym artykule, nie zaleca się stosowanie protokołu ICMP przez zaporę systemu Windows dla wdrożeń produkcyjnych.
7. Włączone przesyłanie dalej IP w obrębie platformy Azure dla interfejsu sieciowego maszyny Wirtualnej w [fowarding włączyć IP](#enable-ip-forwarding). W ramach maszyny Wirtualnej systemu operacyjnego lub aplikacji działających w Maszynie wirtualnej, musi także do przesyłania dalej ruchu sieciowego. Włącz przesyłanie dalej IP w ramach systemu operacyjnego *myVmNva* maszyny Wirtualnej, wykonując następujące kroki z *myVmPrivate* maszyny Wirtualnej:

    Usługi pulpitu zdalnego *myVmNva* przy użyciu następującego polecenia w wierszu polecenia:

    ``` 
    mstsc /v:myvmnva
    ```
    
    Aby włączyć IP przekazywania w ramach systemu operacyjnego, wprowadź następujące polecenie w programie PowerShell:

    ```powershell
    Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
    ```
    
    Uruchom ponownie maszynę Wirtualną, co spowoduje także odłączenie sesji usług pulpitu zdalnego.
8. Podczas nadal podłączony do *myVmPrivate* maszynę Wirtualną, utwórz sesję pulpitu zdalnego do *myVmPublic* maszyny Wirtualnej za pomocą następującego polecenia, po *myVmNva* ponownego uruchomienia maszyny Wirtualnej:

    ``` 
    mstsc /v:myVmPublic
    ```
    
    Włącz ICMP przez zaporę systemu Windows, wprowadzając następujące polecenie z programu PowerShell:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

9. Aby przetestować routingu ruchu sieciowego do *myVmPrivate* maszyny Wirtualnej z *myVmPublic* maszyny Wirtualnej, wprowadź następujące polecenie z programu PowerShell:

    ```
    tracert myVmPrivate
    ```

    Odpowiedź jest podobny do poniższego przykładu:
    
    ```
    Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
    over a maximum of 30 hops:
        
    1    <1 ms     *        1 ms  10.0.2.4
    2     1 ms     1 ms     1 ms  10.0.1.4
        
    Trace complete.
    ```
      
    Widać, że pierwszym przeskoku jest 10.0.2.4, czyli NVA prywatnego adresu IP. Drugi przeskok jest 10.0.1.4, prywatnego adresu IP *myVmPrivate* maszyny Wirtualnej. Trasy dodane do *myRouteTablePublic* tabeli tras i powiązanych z *publicznego* podsieci spowodował Azure, aby kierować ruchem przez analizę NVA, a nie bezpośrednio do *prywatnego* podsieci.
10.  Zamykanie sesji usług pulpitu zdalnego do *myVmPublic* maszyny Wirtualnej, co spowoduje pozostawienie połączenie *myVmPrivate* maszyny Wirtualnej.
11. Aby przetestować routingu ruchu sieciowego do *myVmPublic* maszyny Wirtualnej z *myVmPrivate* maszyny Wirtualnej, wprowadź następujące polecenie w wierszu polecenia:

    ```
    tracert myVmPublic
    ```

    Odpowiedź jest podobny do poniższego przykładu:

    ```
    Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
    over a maximum of 30 hops:
    
    1     1 ms     1 ms     1 ms  10.0.0.4
    
    Trace complete.
    ```

    Widać, że ruch jest kierowany bezpośrednio z *myVmPrivate* maszyny Wirtualnej, aby *myVmPublic* maszyny Wirtualnej. Domyślnie ruch trasy Azure bezpośrednio między podsieciami.
12. Zamykanie sesji usług pulpitu zdalnego do *myVmPrivate* maszyny Wirtualnej.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy nie jest już potrzebny, Usuń grupy zasobów i wszystkie zasoby, które zawiera: 

1. Wprowadź *myResourceGroup* w **wyszukiwania** pole w górnej części portalu. Po wyświetleniu **myResourceGroup** w wynikach wyszukiwania wybierz go.
2. Wybierz pozycję **Usuń grupę zasobów**.
3. Wprowadź *myResourceGroup* dla **typu nazwa grupy zasobów:** i wybierz **usunąć**.

## <a name="next-steps"></a>Kolejne kroki

W tym artykule możesz utworzyć tabelę tras i skojarzone go do podsieci. Utworzono NVA proste, który kierowany ruch z publicznego podsieci prywatne podsieci. Wdrażanie różnych NVAs wstępnie skonfigurowane, wykonujących funkcji sieciowych, takich jak zapory i Optymalizacja sieci WAN z [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Przed wdrożeniem tabel tras do użytku produkcyjnego, zaleca się, że należy dokładnie zapoznać się z [Routing na platformie Azure](virtual-networks-udr-overview.md), [tabel tras Zarządzaj](manage-route-table.md), i [Azure ogranicza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).


Podczas wdrażania można wielu zasobów platformy Azure w ramach sieci wirtualnej, zasobów w przypadku niektórych usług Azure PaaS nie można wdrożyć w sieci wirtualnej. Możesz nadal ograniczyć dostęp do zasobów pewnych usług Azure PaaS ruch tylko z podsieci sieci wirtualnej jednak. Przejdź do następnego samouczkiem, aby dowiedzieć się, jak ograniczyć dostęp do sieci Azure PaaS zasobów.

> [!div class="nextstepaction"]
> [Ograniczenie dostępu do sieci do PaaS zasobów](tutorial-restrict-network-access-to-resources.md)
