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
ms.date: 12/15/2017
ms.author: jdial
ms.openlocfilehash: 225dd64c46bf9af3e058bbe3cfacf8f8a693b565
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="add-network-interfaces-to-or-remove-network-interfaces-from-virtual-machines"></a>Dodawanie interfejsów sieciowych lub usuwanie interfejsów sieciowych z maszyn wirtualnych

Dodawanie istniejącego interfejsu sieciowego, podczas tworzenia maszyny Wirtualnej lub dodawanie lub usuwanie interfejsów sieciowych z istniejącej maszyny Wirtualnej w stanie zatrzymania (cofnięciu przydziału). Interfejs sieciowy umożliwia Azure maszyny wirtualnej (VM) do komunikacji z Internetem, Azure i lokalnymi zasobami. Maszyna wirtualna może mieć jeden lub więcej interfejsów sieciowych. 

Jeśli potrzebujesz dodać, zmienić lub usunąć adresy IP dla karty sieciowej, przeczytaj [zarządzania adresami IP interfejsu sieci](virtual-network-network-interface-addresses.md) artykułu. Jeśli potrzebne do utworzenia, zmiany lub usuwanie interfejsów sieciowych, przeczytaj [Zarządzanie interfejsów sieciowych](virtual-network-network-interface.md) artykułu.

## <a name="before"></a>Przed rozpoczęciem

Przed wykonaniem kolejnych kroków w dowolnej części tego artykułu, należy wykonać następujące zadania:

- Zaloguj się do platformy Azure [portal](https://portal.azure.com), Azure interfejsu wiersza polecenia (CLI) lub Azure PowerShell przy użyciu konta platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, należy zarejestrować się w celu [bezpłatnego konta wersji próbnej](https://azure.microsoft.com/free).
- Jeśli za pomocą poleceń programu PowerShell do wykonywania zadań w tym artykule [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję apletów poleceń programu Azure PowerShell zainstalowane. Aby uzyskać pomoc dotyczącą poleceń programu PowerShell, wraz z przykładami, wpisz `get-help <command> -full`. Zamiast instalowania programu Azure PowerShell, można użyć powłoki chmury Azure. Powłoka chmury Azure jest bezpłatna programu PowerShell, który można uruchomić bezpośrednio z poziomu portalu Azure. Ma on Azure PowerShell wstępnie zainstalowane i skonfigurowane do użycia z Twoim kontem. Aby użyć powłoki chmury, kliknij przycisk powłoki chmury **> _** przycisk w górnej części [portal](https://portal.azure.com) i wybierz polecenie programu PowerShell w lewym górnym rogu, po wyświetleniu okna powłoki.
- Jeśli za pomocą poleceń Azure interfejsu wiersza polecenia (CLI) do wykonywania zadań w tym artykule [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia, wpisz `az <command> --help`. Zamiast instalowania interfejsu wiersza polecenia i jego wymagania wstępne, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Ma ona wstępnie zainstalowany interfejs wiersza polecenia platformy Azure skonfigurowany do użycia z Twoim kontem. Aby użyć powłoki chmury, kliknij przycisk powłoki chmury **> _** przycisk w górnej części [portal](https://portal.azure.com) i wybierz Bash w lewym górnym rogu, po wyświetleniu okna powłoki.

## <a name="vm-create"></a>Dodawanie istniejących interfejsów sieciowych do nowej maszyny Wirtualnej

Podczas tworzenia maszyny Wirtualnej za pośrednictwem portalu portal tworzy interfejs sieciowy z ustawieniami domyślnymi i dołącza go do maszyny Wirtualnej dla Ciebie. Nie można dodać istniejących interfejsów sieciowych do nowej maszyny Wirtualnej lub utwórz maszynę Wirtualną z wieloma interfejsami sieciowymi przy użyciu portalu Azure. Możesz to zrobić zarówno przy użyciu interfejsu wiersza polecenia lub środowiska PowerShell. Przed użyciem programu PowerShell lub interfejsu wiersza polecenia, można jednak utworzyć Maszynę wirtualną z istniejącego interfejsu sieciowego, warto zapoznać się z [ograniczenia](#constraints). Jeśli tworzysz maszynę wirtualną z wieloma interfejsami sieciowymi, należy również skonfigurować system operacyjny prawidłowe korzystanie, po utworzeniu maszyny Wirtualnej. Aby uzyskać więcej informacji, zobacz Konfigurowanie [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) lub [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) wiele interfejsów sieciowych.

**Polecenia** przed utworzeniem maszyny Wirtualnej, należy utworzyć interfejsu sieciowego, wykonując kroki w [utworzyć interfejs sieciowy](virtual-network-network-interface.md#create-a-network-interface).

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Tworzenie maszyny wirtualnej az](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_create)|
|PowerShell|[New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Dodaj do istniejącej maszyny Wirtualnej karty sieciowej

1. Zaloguj się w witrynie Azure Portal.
2. W polu wyszukiwania w górnej części portalu, wyszukaj nazwę maszyny wirtualnej, które chcesz dodać do interfejsu sieciowego, lub Wyszukaj maszynę Wirtualną, klikając **wszystkie usługi**, następnie **maszyn wirtualnych**. Po znalezieniu maszyny Wirtualnej, kliknij go. Maszyna wirtualna ma zostać dodany do interfejsu sieciowego musi obsługiwać liczba interfejsów sieciowych, który chcesz dodać. Aby dowiedzieć się, każdy obsługuje rozmiar maszyny Wirtualnej, przeczytaj interfejsy sieciowe ile [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykuły rozmiary maszyny Wirtualnej.  
3. Kliknij przycisk **omówienie**w obszarze **ustawienia**. Kliknij przycisk **zatrzymać**i zaczekaj, aż do **stan** maszyny wirtualnej zmienia się na *zatrzymane (cofnięciu przydziału)*. 
4. Kliknij przycisk **sieci**w obszarze **ustawienia**.
5. Kliknij przycisk **interfejsu sieciowego Attach**. Z listy istniejących interfejsów sieciowych, które aktualnie nie są dołączone do innej maszyny Wirtualnej kliknij przycisk interfejsu sieciowego, który chcesz dołączyć. Interfejs sieciowy, którą wybierzesz nie można przyspieszyć sieci włączone, nie może mieć przypisanego adresu IPv6 i musi znajdować się w tej samej sieci wirtualnej jako sieci wirtualnej karcie sieciowej jest obecnie dołączony do maszyny Wirtualnej znajduje się w. Jeśli nie masz istniejący interfejs sieciowy, należy najpierw go utworzyć. Aby utworzyć interfejsu sieciowego, kliknij przycisk **interfejsu sieciowego Utwórz**. Aby dowiedzieć się więcej na temat tworzenia interfejsu sieciowego, zobacz [utworzyć interfejs sieciowy](virtual-network-network-interface.md#create-a-network-interface). Aby dowiedzieć się więcej o dodatkowe ograniczenia, dodając interfejsów sieciowych do maszyny wirtualnej, zobacz [ograniczenia](#constraints).
6. Kliknij przycisk **OK**.
7. Kliknij przycisk **omówienie**w obszarze **ustawienia**. Kliknij przycisk **Start** można uruchomić maszyny wirtualnej.
8. Skonfiguruj system operacyjny maszyny Wirtualnej prawidłowe korzystanie z wieloma interfejsami sieciowymi. Aby uzyskać więcej informacji, zobacz Konfigurowanie [Linux](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) lub [Windows](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-guest-os-for-multiple-nics) wiele interfejsów sieciowych.

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Dodaj kartę sieciową maszyny wirtualnej az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_add) (odwołanie) lub [szczegółowe kroki](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Dodaj AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (odwołanie) lub [szczegółowe kroki](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Wyświetlanie interfejsów sieciowych do maszyny Wirtualnej

Można wyświetlić interfejsów sieciowych podłączonych do maszyn wirtualnych, aby dowiedzieć się więcej na temat konfiguracji każdego interfejsu sieciowego i adresy IP przypisane do każdego interfejsu sieciowego. 

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, którego przypisano rolę właściciela, współautora lub współautora sieci dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *maszyn wirtualnych*. Gdy **maszyn wirtualnych** pojawia się w wynikach wyszukiwania kliknij ją.
3. Kliknij nazwę maszyny Wirtualnej chcesz wyświetlić interfejsów sieciowych.
4. W **ustawienia** dla wybranego maszyny Wirtualnej, kliknij przycisk **sieci**. Informacje na temat ustawienia interfejsu sieciowego oraz sposobu zmiany ich, zobacz [Zarządzanie interfejsów sieciowych](virtual-network-network-interface.md). Aby dowiedzieć się więcej na temat dodawania, zmieniania lub usuwania adresów IP przypisanych do interfejsu sieciowego, zobacz [adresów IP zarządzanie](virtual-network-network-interface-addresses.md).

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Pokaż wirtualna az](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Usunąć interfejsu sieciowego z maszyny Wirtualnej

1. Zaloguj się w witrynie Azure Portal.
2. W polu wyszukiwania w górnej części portalu, wyszukaj nazwę maszyny wirtualnej do usunięcia (odłączyć) interfejsu sieciowego lub Wyszukaj maszynę Wirtualną, klikając **wszystkie usługi**, następnie **maszyn wirtualnych**. Po znalezieniu maszyny Wirtualnej, kliknij go.
3. Kliknij przycisk **omówienie**w obszarze **ustawienia**. Kliknij przycisk **zatrzymać**i zaczekaj, aż do **stan** maszyny wirtualnej zmienia się na *zatrzymane (cofnięciu przydziału)*. 
4. Kliknij przycisk **sieci**w obszarze **ustawienia**.
5. Kliknij przycisk **interfejsu sieciowego Detach**. Z listy interfejsów sieciowych podłączonych do maszyny wirtualnej kliknij przycisk interfejsu sieciowego, który chcesz odłączyć. Jeśli tylko jeden interfejs sieciowy jest wyświetlana, nie można odłączyć, ponieważ maszyna wirtualna zawsze musi mieć co najmniej jeden interfejs sieciowy do niego dołączony.
6. Kliknij przycisk **OK**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Usuń kartę sieciową maszyny wirtualnej az](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#az_vm_nic_remove) (odwołanie) lub [szczegółowe kroki](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Usuń AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (odwołanie) lub [szczegółowe kroki](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Następne kroki
Aby utworzyć Maszynę wirtualną z wielu interfejsów sieciowych lub adresów IP, przeczytaj następujące artykuły:

**Polecenia**

|Zadanie|Narzędzie|
|---|---|
|Tworzenie maszyny wirtualnej z wieloma kartami sieciowymi|[Interfejs wiersza polecenia](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [środowiska PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Tworzenie jednej maszyny Wirtualnej karty Sieciowej z wielu adresów IPv4|[Interfejs wiersza polecenia](virtual-network-multiple-ip-addresses-cli.md), [środowiska PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Tworzenie jednej maszyny Wirtualnej karty Sieciowej za pomocą prywatnego adresu IPv6 (za równoważenia obciążenia Azure)|[Interfejs wiersza polecenia](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [szablonu usługi Azure Resource Manager](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="constraints"></a>Ograniczenia

- Maszyna wirtualna musi mieć co najmniej jeden interfejs sieciowy do niego dołączony.
- Maszyna wirtualna może mieć tylko jako wiele sieci interfejsy do niego dołączony jako obsługuje rozmiar maszyny Wirtualnej. Aby dowiedzieć się więcej, o ile interfejsów sieciowych obsługuje każdego rozmiaru maszyny Wirtualnej, zobacz [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) i [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) rozmiarów maszyn wirtualnych. Wszystkich rozmiarów obsługuje co najmniej dwa interfejsy sieciowe.
- Interfejsy sieciowe, które dodasz do maszyny Wirtualnej nie może obecnie dołączony do innej maszyny Wirtualnej. Aby dowiedzieć się więcej o tworzeniu interfejsów sieciowych, zobacz [utworzyć interfejs sieciowy](virtual-network-network-interface.md#create-a-network-interface).
- W przeszłości interfejsów sieciowych można być dodane tylko do maszyn wirtualnych obsługiwanych wiele interfejsów sieciowych, które zostały utworzone z co najmniej dwa interfejsy sieci. Nie można dodać karty sieciowej do maszyny Wirtualnej, który został utworzony z jeden interfejs sieciowy, nawet jeśli rozmiar maszyny Wirtualnej obsługuje wiele interfejsów sieciowych. Z drugiej strony można tylko usunąć interfejsów sieciowych z maszyny Wirtualnej z co najmniej trzech interfejsów sieciowych, ponieważ maszyny wirtualne utworzone z siecią co najmniej dwa interfejsy zawsze musiała mieć co najmniej dwa interfejsy sieciowe. Żadna z tych warunków ograniczających już stosowane. Można teraz utworzyć Maszynę wirtualną z dowolnej liczby interfejsów sieciowych (do liczby obsługiwanych przez rozmiar maszyny Wirtualnej).
- Domyślnie pierwszy interfejs sieciowy dołączony do maszyny Wirtualnej jest zdefiniowany jako *głównej* interfejsu sieciowego. Wszystkie inne interfejsy sieciowe w maszynie Wirtualnej są *dodatkowej* interfejsów sieciowych.
- Chociaż można kontrolować, które interfejs sieciowy wysłany ruch wychodzący, domyślnie, cały ruch wychodzący z maszyny Wirtualnej jest wysłane z adresu IP przypisanego do podstawowej konfiguracji IP interfejsu sieci podstawowej.
- W przeszłości musi mieć interfejsy sieci jednego lub wielu, zostały wszystkich maszyn wirtualnych w ramach tego samego zestawu dostępności. Maszyny wirtualne z dowolnej liczby interfejsy sieciowe można teraz istnieje w tym samym zestawie dostępności, nawet z liczbą obsługiwaną przez rozmiar maszyny Wirtualnej. Można dodać tylko Maszynę wirtualną do zestawu po utworzeniu jednak dostępności. Aby dowiedzieć się więcej na temat zestawów dostępności, przeczytaj [Zarządzaj dostępnością maszyn wirtualnych na platformie Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) artykułu.
- Gdy interfejsy sieciowe w tej samej maszyny Wirtualnej można podłączyć do różnych podsieci w sieci wirtualnej, interfejsy sieciowe muszą połączone z tej samej sieci wirtualnej.
- Dowolnego adresu IP dla żadnej konfiguracji adresu IP z żadnym interfejsem sieciowym podstawowy lub pomocniczy można dodać do puli zaplecza modułu równoważenia obciążenia Azure. W przeszłości tylko podstawowy adres IP dla interfejsu sieci podstawowej można można dodać do puli zaplecza. Aby dowiedzieć się więcej na temat adresów IP i konfiguracje, przeczytaj [Dodaj, Zmień lub Usuń adresy IP](virtual-network-network-interface-addresses.md) artykułu.
- Usuwanie maszyny Wirtualnej nie powoduje usunięcia interfejsów sieciowych, które są dołączone do niego. Po usunięciu maszyny Wirtualnej, interfejsy sieciowe są odłączone od maszyny Wirtualnej. Można dodać interfejsy sieciowe do różnych maszyn wirtualnych, lub usuń je.
- Jeśli interfejs sieciowy ma przypisany prywatny adres IPv6, należy dodać (Dołącz) do maszyny Wirtualnej podczas tworzenia maszyny Wirtualnej. Nie można dodać karty sieciowej z adresu IPv6 przypisany do maszyny Wirtualnej, po utworzeniu maszyny Wirtualnej. Jeśli dodasz karty sieciowej z przypisany prywatny adres IPv6, podczas tworzenia maszyny wirtualnej, można tylko dodać ten interfejs sieciowy do maszyny wirtualnej, niezależnie od tego, jak wiele interfejsów sieciowych obsługuje rozmiar maszyny Wirtualnej. Zobacz [sieci adresów IP interfejsu](virtual-network-network-interface-addresses.md) Aby dowiedzieć się więcej o przypisanie adresów IP do interfejsów sieciowych.
- Podobny do protokołu IPv6, nie można dołączyć karty sieciowej z siecią przyspieszonego włączona do maszyny Wirtualnej po utworzeniu maszyny Wirtualnej. Co więcej Aby skorzystać z przyspieszonego sieci, musi również wykonaj kroki w ramach systemu operacyjnego maszyny Wirtualnej. Aby dowiedzieć się więcej o sieci przyspieszonego i inne ograniczenia, podczas korzystania z niego, zobacz akcelerowanego sieci dla [Windows](create-vm-accelerated-networking-powershell.md) lub [Linux](create-vm-accelerated-networking-cli.md) maszyn wirtualnych.
