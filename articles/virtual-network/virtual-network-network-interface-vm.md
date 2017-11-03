---
title: "Interfejsy sieciowe, aby dodać lub usunąć z maszyn wirtualnych platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać interfejsów sieciowych lub usunąć interfejsów sieciowych z maszyn wirtualnych."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: 7df1dfbea8c985907d5330819dc1e7bf1578aafa
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="add-network-interfaces-to-or-remove-from-virtual-machines"></a>Interfejsy sieciowe, aby dodać lub usunąć z maszyny wirtualne

Dodawanie istniejącego interfejsu sieciowego, podczas tworzenia maszyny Wirtualnej lub dodawanie lub usuwanie interfejsów sieciowych z istniejącej maszyny Wirtualnej w stanie zatrzymania (cofnięciu przydziału). Interfejs sieciowy umożliwia Azure maszyny wirtualnej (VM) do komunikacji z Internetem, Azure i lokalnymi zasobami. Maszyna wirtualna może mieć jeden lub więcej interfejsów sieciowych. 

Jeśli potrzebujesz dodać, zmienić lub usunąć adresy IP dla karty sieciowej, przeczytaj [zarządzania adresami IP interfejsu sieci](virtual-network-network-interface-addresses.md) artykułu. Jeśli potrzebne do utworzenia, zmiany lub usuwanie interfejsów sieciowych, przeczytaj [Zarządzanie interfejsów sieciowych](virtual-network-network-interface.md) artykułu.

## <a name="before"></a>Przed rozpoczęciem

Przed wykonaniem kolejnych kroków w dowolnej części tego artykułu, należy wykonać następujące zadania:

- Dowiedz się więcej o ile interfejsów sieciowych rozmiar każdego z systemami Linux i maszyny Wirtualnej systemu Windows obsługuje przeglądając [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykuły rozmiary maszyny Wirtualnej.
- Zaloguj się do platformy Azure [portal](https://portal.azure.com), Azure interfejsu wiersza polecenia (CLI) lub Azure PowerShell przy użyciu konta platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, należy zarejestrować się w celu [bezpłatnego konta wersji próbnej](https://azure.microsoft.com/free).
- Jeśli za pomocą poleceń programu PowerShell do wykonywania zadań w tym artykule [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję apletów poleceń programu Azure PowerShell zainstalowane. Aby uzyskać pomoc dotyczącą poleceń programu PowerShell, wraz z przykładami, wpisz `get-help <command> -full`.
- Jeśli za pomocą poleceń Azure interfejsu wiersza polecenia (CLI) do wykonywania zadań w tym artykule [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia, wpisz `az <command> --help`. Zamiast instalowania interfejsu wiersza polecenia i jego wymagania wstępne, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Ma ona wstępnie zainstalowany interfejs wiersza polecenia platformy Azure skonfigurowany do użycia z Twoim kontem. Aby użyć powłoki chmury, kliknij przycisk powłoki chmury **> _** przycisk w górnej części [portal](https://portal.azure.com).

## <a name="about"></a>O interfejsami sieciowymi i maszyny wirtualne

Możesz dodać (Dołącz) istniejący interfejs sieciowy do maszyny Wirtualnej podczas tworzenia maszyny Wirtualnej, pod warunkiem interfejsu sieciowego nie jest obecnie dołączony do innej maszyny Wirtualnej. Interfejs sieciowy, aby dodać lub usunąć (odłączyć) do interfejsu sieciowego z istniejącej maszyny Wirtualnej, pod warunkiem maszyna wirtualna jest w stanie zatrzymania (cofnięciu przydziału). Jeśli tworzysz Maszynę wirtualną przy użyciu portalu Azure, portal tworzy interfejs sieciowy z ustawieniami domyślnymi. Portalu nie umożliwiają:

- Określ istniejącego interfejsu sieciowego, aby dodać podczas tworzenia maszyny Wirtualnej
- Tworzenie maszyny wirtualnej z wieloma interfejsami sieciowymi
- Określ nazwę dla interfejsu sieciowego (portal tworzy interfejs sieciowy z domyślną nazwą)

Do utworzenia karty sieciowej lub maszyny Wirtualnej z wszystkie poprzednie atrybuty, które nie można użyć portalu dla można użyć programu Azure PowerShell lub interfejsu wiersza polecenia. Przed wykonaniem zadania w poniższych sekcjach należy wziąć pod uwagę następujące ograniczenia i zachowania:

- Co najmniej dwa interfejsy sieci obsługuje wszystkich rozmiarów maszyn wirtualnych, ale niektóre rozmiarów maszyn wirtualnych obsługuje więcej niż dwa interfejsy sieciowe. W przeszłości niektóre rozmiarów maszyn wirtualnych obsługiwany tylko jeden interfejs sieciowy. Aby dowiedzieć się, każdy obsługuje rozmiar maszyny Wirtualnej, przeczytaj interfejsy sieciowe ile [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykuły rozmiary maszyny Wirtualnej. 
- W przeszłości interfejsów sieciowych można być dodane tylko do maszyn wirtualnych obsługiwanych wiele interfejsów sieciowych, które zostały utworzone z co najmniej dwa interfejsy sieci. Nie można dodać karty sieciowej do maszyny Wirtualnej, który został utworzony z jeden interfejs sieciowy, nawet jeśli rozmiar maszyny Wirtualnej obsługuje wiele interfejsów sieciowych. Z drugiej strony można tylko usunąć interfejsów sieciowych z maszyny Wirtualnej z co najmniej trzech interfejsów sieciowych, ponieważ maszyny wirtualne utworzone z siecią co najmniej dwa interfejsy zawsze musiała mieć co najmniej dwa interfejsy sieciowe. Żadna z tych warunków ograniczających już stosowane. Można teraz utworzyć Maszynę wirtualną z dowolnej liczby interfejsów sieciowych (do liczby obsługiwanych przez rozmiar maszyny Wirtualnej) i dodawać i usuwać dowolna liczba interfejsów sieciowych (z maszyn wirtualnych w stanie zatrzymania (cofnięciu przydziału)), jak długo maszyny Wirtualnej ma zawsze co najmniej jeden interfejs sieciowy.
- Domyślnie pierwszy interfejs sieci na maszynie wirtualnej jest zdefiniowany jako *głównej* interfejsu sieciowego. Wszystkie inne interfejsy sieciowe w maszynie Wirtualnej są *dodatkowej* interfejsów sieciowych.
- Podstawowych interfejsów sieciowych przypisanych brama domyślna przez serwery Azure DHCP, ale nie są dodatkowych interfejsów sieciowych. Ponieważ dodatkowych interfejsów sieciowych nie są przypisane bramę domyślną, nie mogli komunikować się z zasobami spoza ich podsieci domyślnie. Aby włączyć dodatkowych interfejsów sieciowych do komunikowania się z zasobami spoza ich podsieci, zobacz [routingu w systemie operacyjnym maszyny wirtualnej z wieloma interfejsami sieciowymi](#routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces).
- Domyślnie ruch wychodzący z maszyny Wirtualnej jest wysłane z adresu IP przypisanego do podstawowej konfiguracji IP interfejsu sieci podstawowej. Możesz kontrolować, który adres IP jest używana dla ruchu wychodzącego w ramach systemu operacyjnego maszyny Wirtualnej, ale domyślnie jest za pośrednictwem interfejsu sieci podstawowej.
- W przeszłości musi mieć interfejsy sieci jednego lub wielu, zostały wszystkich maszyn wirtualnych w ramach tego samego zestawu dostępności. Maszyny wirtualne z dowolnej liczby interfejsy sieciowe można teraz istnieje w tym samym zestawie dostępności, nawet z liczbą obsługiwaną przez rozmiar maszyny Wirtualnej. Można dodać tylko Maszynę wirtualną do zestawu po utworzeniu jednak dostępności. Aby dowiedzieć się więcej na temat zestawów dostępności, przeczytaj [Zarządzaj dostępnością maszyn wirtualnych na platformie Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) artykułu.
- Gdy interfejsy sieciowe w tej samej maszyny Wirtualnej można podłączyć do różnych podsieci w sieci wirtualnej, interfejsy sieciowe muszą połączone z tej samej sieci wirtualnej.
- Dowolnego adresu IP dla żadnej konfiguracji adresu IP z żadnym interfejsem sieciowym podstawowy lub pomocniczy można dodać do puli zaplecza modułu równoważenia obciążenia Azure. W przeszłości tylko podstawowy adres IP dla interfejsu sieci podstawowej można można dodać do puli zaplecza. Aby dowiedzieć się więcej na temat adresów IP i konfiguracje, przeczytaj [Dodaj, Zmień lub Usuń adresy IP](virtual-network-network-interface-addresses.md) artykułu.
- Usuwanie maszyny Wirtualnej nie powoduje usunięcia interfejsów sieciowych, które są dołączone do niego. Po usunięciu maszyny Wirtualnej, interfejsy sieciowe są odłączone od maszyny Wirtualnej. Można dodać interfejsy sieciowe do różnych maszyn wirtualnych, lub usuń je.
- Jeśli interfejs sieciowy ma przypisany prywatny adres IPv6, aby podłączyć go do maszyny Wirtualnej, podczas tworzenia maszyny Wirtualnej Nie można dołączyć karty sieciowej z adresu IPv6 przypisany do maszyny Wirtualnej, po utworzeniu maszyny Wirtualnej. Jeśli podczas tworzenia maszyny wirtualnej można dołączyć karty sieciowej z przypisany prywatny adres IPv6, można dołączyć do maszyny wirtualnej, niezależnie od tego, jak wiele interfejsów sieciowych, rozmiar maszyny Wirtualnej obsługuje tylko ten interfejs sieciowy. Zobacz [sieci adresów IP interfejsu](virtual-network-network-interface-addresses.md) Aby dowiedzieć się więcej o przypisanie adresów IP do interfejsów sieciowych.

## <a name="vm-create"></a>Dodawanie istniejących interfejsów sieciowych do nowej maszyny Wirtualnej

Podczas tworzenia maszyny Wirtualnej za pośrednictwem portalu portal tworzy interfejs sieciowy z ustawieniami domyślnymi i dołącza go do maszyny Wirtualnej dla Ciebie. Nie można dodać istniejących interfejsów sieciowych do nowej maszyny Wirtualnej lub utwórz maszynę Wirtualną z wieloma interfejsami sieciowymi przy użyciu portalu Azure. Możesz to zrobić zarówno przy użyciu interfejsu wiersza polecenia lub środowiska PowerShell. Możesz dodać dowolną liczbę interfejsów sieciowych do maszyny Wirtualnej jako obsługuje tworzonego rozmiar maszyny Wirtualnej. Aby dowiedzieć się więcej, o ile interfejsów sieciowych obsługuje każdego rozmiaru maszyny Wirtualnej, przeczytaj [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykuły rozmiary maszyny Wirtualnej. Interfejsy sieciowe, które dodasz do maszyny Wirtualnej nie może obecnie dołączony do innej maszyny Wirtualnej. Aby dowiedzieć się więcej o tworzeniu interfejsów sieciowych, przeczytaj [Zarządzanie interfejsów sieciowych](virtual-network-network-interface.md#create-a-network-interface) artykułu.

### <a name="routing-within-a-virtual-machine-operating-system-with-multiple-network-interfaces"></a>Routing w systemie operacyjnym maszyny wirtualnej z wieloma interfejsami sieciowymi

Azure przypisuje do pierwszego interfejsu sieciowego (podstawowe) dołączonych do maszyny wirtualnej bramy domyślnej. Azure nie przypisuje bramy domyślnej na interfejsy dodatkowe sieci (pomocniczy) dołączonych do maszyny wirtualnej. Jesteś w związku z tym nie może komunikować się z zasobami spoza podsieci, która jest dodatkowy interfejs sieciowy, domyślnie. Dodatkowych interfejsów sieciowych można jednak komunikować się z zasobami spoza ich podsieci, chociaż kroki, aby umożliwić komunikację są różne dla różnych systemów operacyjnych.

### <a name="windows"></a>Windows

Wykonaj następujące kroki w wierszu polecenia systemu Windows:

1. Uruchom `route print` polecenia, które zwraca dane wyjściowe podobne do następujących danych wyjściowych dla maszyny wirtualnej z dwoma interfejsami sieciowymi dołączone:

    ```
    ===========================================================================
    Interface List
    3...00 0d 3a 10 92 ce ......Microsoft Hyper-V Network Adapter #3
    7...00 0d 3a 10 9b 2a ......Microsoft Hyper-V Network Adapter #4
    ===========================================================================
    ```
 
    W tym przykładzie **Microsoft Hyper-V sieci karty #4** (interfejs 7) jest dodatkowy interfejs sieciowy, który nie ma przypisane do niej bramy domyślnej.

2. W wierszu polecenia Uruchom `ipconfig` polecenie, aby zobaczyć, których adres IP jest przypisany do interfejsu sieciowego dodatkowej. W tym przykładzie 192.168.2.4 jest przypisany do interfejsu 7. Dodatkowy interfejs sieciowy jest zwracane nie adres bramy domyślnej.

3. Aby rozesłać całego ruchu skierowanego do adresów spoza podsieci dodatkowy interfejs sieciowy z bramą podsieci, uruchom następujące polecenie:

    ```
    route add -p 0.0.0.0 MASK 0.0.0.0 192.168.2.1 METRIC 5015 IF 7
    ```

    Adres bramy dla podsieci jest pierwszy adres IP (kończy się rozszerzeniem.1) w zakresie adresów zdefiniowane dla tej podsieci. Jeśli nie chcesz rozsyłać cały ruch spoza podsieci, możesz zamiast tego dodać indywidualnych tras do określonych miejsc docelowych. Na przykład, jeśli chce się przekierowujący ruch z dodatkowy interfejs sieciowy do 192.168.3.0 sieci, wpisz polecenie:

      ```
      route add -p 192.168.3.0 MASK 255.255.255.0 192.168.2.1 METRIC 5015 IF 7
      ```
  
4. Aby potwierdzić łączność z zasobów na 192.168.3.0 sieci, na przykład wprowadź następujące polecenie, aby wykonać polecenie ping 192.168.3.4 przy użyciu interfejsu 7 (192.168.2.4):

    ```
    ping 192.168.3.4 -S 192.168.2.4
    ```

    Może być konieczne otwarcie ICMP przez zaporę systemu Windows, urządzenia, które są ping przy użyciu następującego polecenia:
  
      ```
      netsh advfirewall firewall add rule name=Allow-ping protocol=icmpv4 dir=in action=allow
      ```
  
5. Aby upewnić się, dodano trasę znajduje się w tabeli tras, wprowadź `route print` polecenia, które zwraca dane wyjściowe podobne do następującego tekstu:

    ```
    ===========================================================================
    Active Routes:
    Network Destination        Netmask          Gateway       Interface  Metric
              0.0.0.0          0.0.0.0      192.168.1.1      192.168.1.4     15
              0.0.0.0          0.0.0.0      192.168.2.1      192.168.2.4   5015
    ```

    Na liście z trasy *192.168.1.1* w obszarze **bramy**, trasy, który jest domyślnie dla interfejsu sieci podstawowej. Trasa o *192.168.2.1* w obszarze **bramy**, trasy dodane.

### <a name="linux"></a>Linux

Domyślnie korzysta z hosta słabe routingu, zalecamy ograniczenie dodatkowej ruchu interfejsu sieci między zasobami w tej samej podsieci. Jeśli komunikacja poza podsieć jest wymagana dla dodatkowych interfejsów sieciowych, należy utworzyć reguły routingu zezwalających na maszynie wirtualnej, aby wysyłać i odbierać dane za pośrednictwem w konkretnym interfejsie sieciowym. W przeciwnym razie ruchu, który należy do eth1, na przykład nie może przetworzyć poprawnie trasy zdefiniowanej wartości domyślnej. Aby dowiedzieć się, jak skonfigurować reguły routingu, zobacz [Linux skonfigurować dla wielu kart sieciowych](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics).

> [!WARNING]
> Jeśli interfejs sieciowy ma przypisany prywatny adres IPv6, można dodać interfejs sieciowy do maszyny wirtualnej podczas tworzenia maszyny wirtualnej. Nie można dołączyć więcej niż jeden interfejs sieciowy do maszyny wirtualnej, podczas tworzenia maszyny wirtualnej lub po maszyna wirtualna jest utworzona, tak długo, jak protokołu IPv6, adres jest przypisany do karty sieciowej podłączony do maszyny wirtualnej. Zobacz [sieci adresów IP interfejsu](virtual-network-network-interface-addresses.md) Aby dowiedzieć się więcej o przypisanie adresów IP do interfejsów sieciowych.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Tworzenie maszyny wirtualnej az](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nowe AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Dodaj istniejący interfejs sieciowy do istniejącej maszyny Wirtualnej

Możesz dodać dowolną liczbę interfejsów sieciowych do maszyny Wirtualnej jako obsługuje rozmiar maszyny Wirtualnej, który dodajesz interfejsów sieciowych. Aby dowiedzieć się, każdy obsługuje rozmiar maszyny Wirtualnej, przeczytaj interfejsy sieciowe ile [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykuły rozmiary maszyny Wirtualnej. Maszyna wirtualna ma zostać dodany do interfejsu sieciowego musi obsługiwać liczba interfejsów sieciowych, aby dodać i musi być w stanie zatrzymania (cofnięciu przydziału). Interfejsy sieciowe, które chcesz dodać obecnie nie można dołączyć do innej maszyny Wirtualnej. Interfejsy sieciowe nie można dodać do istniejącej maszyny Wirtualnej przy użyciu portalu Azure. Aby dodać interfejsów sieciowych do istniejącej maszyny Wirtualnej, należy użyć interfejsu wiersza polecenia lub środowiska PowerShell. 

Azure przypisuje do pierwszego interfejsu sieciowego (podstawowe) dołączonych do maszyny wirtualnej bramy domyślnej. Azure nie przypisuje bramy domyślnej na interfejsy dodatkowe sieci (pomocniczy) dołączonych do maszyny wirtualnej. Jesteś w związku z tym nie może komunikować się z zasobami spoza podsieci, która jest dodatkowy interfejs sieciowy, domyślnie. Dodatkowych interfejsów sieciowych można jednak komunikować się z zasobami spoza ich podsieci. Jeśli potrzebujesz, że sieci dodatkowej interfejsy łączności z zasobami spoza ich podsieci, zobacz [routingu w systemie operacyjnym maszyny wirtualnej z wieloma interfejsami sieciowymi](#routing-within-a virtual-machine-operating-system-with-multiple-network-interfaces).

> [!WARNING]
> Jeśli interfejs sieciowy ma przypisany prywatny adres IPv6, nie można dodać do istniejącej maszyny wirtualnej. Można tylko dodać karty sieciowej z przypisany prywatny adres IPv6 do maszyny wirtualnej, podczas tworzenia maszyny wirtualnej. Zobacz [sieci adresów IP interfejsu](virtual-network-network-interface-addresses.md) Aby dowiedzieć się więcej o przypisanie adresów IP do interfejsów sieciowych.

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Dodaj kartę sieciową maszyny wirtualnej az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (odwołanie) lub [szczegółowe kroki](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Dodaj AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (odwołanie) lub [szczegółowe kroki](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Wyświetlanie interfejsów sieciowych do maszyny Wirtualnej

Można wyświetlić interfejsów sieciowych podłączonych do maszyn wirtualnych, aby dowiedzieć się więcej na temat konfiguracji każdego interfejsu sieciowego i adresy IP przypisane do każdego interfejsu sieciowego. 

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, którego przypisano rolę właściciela, współautora lub współautora sieci dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *maszyn wirtualnych*. Gdy **maszyn wirtualnych** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **maszyn wirtualnych** bloku, który jest wyświetlany, kliknij nazwę maszyny wirtualnej, który chcesz wyświetlić interfejsów sieciowych do.
4. W **ustawienia** część bloku maszyny wirtualnej, która pojawia się dla maszyny Wirtualnej, kliknij przycisk **sieci**. Aby dowiedzieć się o ustawienia interfejsu sieciowego i należy je zmienić, przeczytaj [Zarządzanie interfejsów sieciowych](virtual-network-network-interface.md) artykułu. Aby dowiedzieć się więcej na temat dodawania, zmieniania lub usuwania adresów IP przypisanych do interfejsu sieciowego, zobacz [adresów IP zarządzanie](virtual-network-network-interface-addresses.md).

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Pokaż wirtualna az](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Usunąć interfejsu sieciowego z maszyny Wirtualnej

Maszyna wirtualna, który chcesz usunąć (lub odłączyć) karty sieciowej z musi być w stanie zatrzymania (cofnięciu przydziału) i musi mieć co najmniej dwa interfejsy sieci podłączonych do niego. Możesz usunąć wszelkie interfejsu sieciowego, ale maszyna wirtualna zawsze musi mieć co najmniej jeden interfejs sieciowy do niego dołączony. Jeśli usuniesz podstawowy interfejs sieciowy Azure przypisuje atrybut podstawowy interfejs sieciowy, który jest dołączony do maszyny Wirtualnej najdłuższej. 

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, którego przypisano rolę właściciela, współautora lub współautora sieci dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *maszyn wirtualnych*. Gdy **maszyn wirtualnych** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **maszyn wirtualnych** bloku, który jest wyświetlany, kliknij nazwę chcesz usunąć karty sieciowej dla maszyny wirtualnej.
4. W **ustawienia** część bloku maszyny wirtualnej, która pojawia się dla maszyny Wirtualnej, kliknij przycisk **sieci**. Aby dowiedzieć się o ustawienia interfejsu sieciowego i należy je zmienić, przeczytaj [Zarządzanie interfejsów sieciowych](virtual-network-network-interface.md) artykułu. Aby dowiedzieć się więcej na temat dodawania, zmieniania lub usuwania adresów IP przypisanych do interfejsu sieciowego, zobacz [adresów IP zarządzanie](virtual-network-network-interface-addresses.md).
5. Kliknij przycisk **X odłączyć interfejsu sieciowego**.
6. Wybierz interfejs sieciowy, aby odłączyć z listy rozwijanej, a następnie kliknij przycisk **OK**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Usuń kartę sieciową maszyny wirtualnej az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (odwołanie) lub [szczegółowe kroki](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Usuń AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (odwołanie) lub [szczegółowe kroki](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Następne kroki
Aby utworzyć Maszynę wirtualną z wielu interfejsów sieciowych lub adresów IP, przeczytaj następujące artykuły:

**Polecenia**

|Zadanie|Narzędzie|
|---|---|
|Tworzenie maszyny wirtualnej z wieloma kartami sieciowymi|[Interfejs wiersza polecenia](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [środowiska PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Tworzenie jednej maszyny Wirtualnej karty Sieciowej z wielu adresów IPv4|[Interfejs wiersza polecenia](virtual-network-multiple-ip-addresses-cli.md), [środowiska PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Tworzenie jednej maszyny Wirtualnej karty Sieciowej za pomocą prywatnego adresu IPv6 (za równoważenia obciążenia Azure)|[Interfejs wiersza polecenia](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [szablonu usługi Azure Resource Manager](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
