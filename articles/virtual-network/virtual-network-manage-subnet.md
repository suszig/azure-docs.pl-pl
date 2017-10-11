---
title: "Dodawanie, zmienianie lub usuwanie podsieć sieci wirtualnej platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak dodać, zmienić lub usunąć podsieć sieci wirtualnej na platformie Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.openlocfilehash: 85ba6ef3e51c339a77eb9b4198c4f87e2a64cf09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Dodawanie, zmienianie lub usuwanie podsieć sieci wirtualnej

Dowiedz się, jak dodać, zmienić lub usunąć podsieć sieci wirtualnej. 

Jeśli nie znasz z sieciami wirtualnymi, aby dodać, zmienić lub usunąć podsieć, zaleca się przeczytanie [Omówienie usługi Azure Virtual Network](virtual-networks-overview.md) i [tworzenie, zmienianie lub usuwanie sieci wirtualnej](virtual-network-manage-network.md). Wszystkie zasoby Azure wdrożony w sieci wirtualnej są wdrażane w podsieci sieci wirtualnej. Na ogół wiele podsieci są tworzone w ramach sieci wirtualnej do:
- **Filtrować ruch między podsieciami**. Sieciowe grupy zabezpieczeń można stosować do podsieci do filtrowania ruchu przychodzącego i wychodzącego ruchu sieciowego dla wszystkich zasobów (np. maszyny wirtualne), które znajdują się w sieci wirtualnej. Aby dowiedzieć się więcej o sposobie tworzenia sieciowej grupy zabezpieczeń, zobacz [Utwórz grupy zabezpieczeń sieci](virtual-networks-create-nsg-arm-pportal.md).
- **Kontrolowanie routing między podsieciami**. Azure tworzy trasy domyślne, tak aby ruch jest automatycznie przesyłany między podsieciami. Trasy domyślne Azure można zastąpić, tworząc trasy zdefiniowane przez użytkownika. Aby dowiedzieć się więcej na temat trasy zdefiniowane przez użytkownika, zobacz [utworzyć trasy zdefiniowane przez użytkownika](virtual-network-create-udr-arm-ps.md). 

W tym artykule omówiono Dodawanie, zmienianie i usuwanie podsieci dla sieci wirtualnych, które zostały utworzone przy użyciu modelu wdrażania usługi Azure Resource Manager.
 
## <a name="before"></a>Przed rozpoczęciem

Przed rozpoczęciem zadań, które zostały opisane w tym artykule, należy wykonać następujące wymagania wstępne:

- Jeśli zaczynasz pracę z sieciami wirtualnymi, firma Microsoft zaleca przejrzenie wykonywania w [tworzenie sieci wirtualnej platformy Azure pierwsze](virtual-network-get-started-vnet-subnet.md). Tego ćwiczenia ułatwiają zapoznanie się ze sieci wirtualnych.
- Informacje na temat limitów dla sieci wirtualnej, przejrzyj [limity Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Zaloguj się do portalu Azure, narzędzia wiersza polecenia platformy Azure (Azure CLI) lub Azure PowerShell przy użyciu konta platformy Azure. Jeśli nie masz konta platformy Azure, należy zarejestrować się w celu [bezpłatnego konta wersji próbnej](https://azure.microsoft.com/free).
- Jeśli planujesz używać poleceń programu PowerShell, aby wykonać zadania opisane w tym artykule, należy najpierw [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję poleceń cmdlet programu Azure PowerShell zainstalowane. Aby uzyskać pomoc dotyczącą poleceń programu PowerShell w przykładach, wprowadź `get-help <command> -full`.
- Jeśli planujesz używać poleceń interfejsu wiersza polecenia Azure, aby wykonać zadania opisane w tym artykule, należy albo:
    - [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane.
    - Użyć powłoki chmury Azure. Zamiast instalowania interfejsu wiersza polecenia i jego zależności, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Ma ona wstępnie zainstalowany interfejs wiersza polecenia platformy Azure skonfigurowany do użycia z Twoim kontem. Aby użyć powłoki chmury, kliknij przycisk powłoki chmury (**> _**) ikonę w górnej części portalu Azure. 

  Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia Azure, wprowadź `az <command> --help`.

## <a name="create-subnet"></a>Dodaj podsieć

Aby dodać podsieci:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, do którego przypisano uprawnienia roli współautora sieci (co najmniej) dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu wyszukiwania portalu wprowadź **sieci wirtualnych**. W wynikach wyszukiwania kliknij **sieci wirtualnych**.
3. Na **sieci wirtualnych** bloku, kliknij, aby dodać podsieci do sieci wirtualnej.
4. W bloku sieci wirtualnej, kliknij **podsieci**.
5. Kliknij przycisk **+ podsieci**.
6. Na **Dodaj podsieć** bloku, wprowadź wartości dla następujących parametrów:
    - **Nazwa**: Nazwa musi być unikatowa w ramach sieci wirtualnej.
    - **Zakres adresów**: zakres musi być unikatowa w ramach przestrzeni adresowej dla sieci wirtualnej. Zakres nie może nakładać się na inne zakresy adresów podsieci w sieci wirtualnej. Przestrzeń adresowa należy określić przy użyciu notacji Classless Inter-Domain Routing (CIDR). Na przykład w sieci wirtualnej z 10.0.0.0/16 przestrzeni adresów, można zdefiniować przestrzeni adresowej podsieci 10.0.0.0/24. Najmniejszą zakres, który można określić jest /29, co umożliwia osiem adresów IP podsieci. Azure rezerwuje pierwszy i ostatni adres w każdej podsieci dla zgodności protokołu. Trzy dodatkowe adresy są zarezerwowane do użycia usługi Azure. W związku z tym Definiowanie podsieci z /29 adresów zakresu wyników w trzech można używać adresów IP w podsieci. Jeśli planowane jest połączenie wirtualnej sieci do bramy sieci VPN, należy utworzyć podsieć bramy. Dowiedz się więcej o [zagadnienia dotyczące zakresu określonego adresu podsieci bramy](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Po dodaniu podsieci w określonych warunkach, można zmienić zakresu adresów. Aby dowiedzieć się, jak zmienić zakres adresów podsieci, zobacz [zmienić ustawienia podsieci](#change-subnet) w tym artykule.
    - **Grupy zabezpieczeń sieci**: Opcjonalnie można skojarzyć istniejącej grupy zabezpieczeń sieci dla podsieci do kontrolowania filtrowanie dla podsieci ruchu przychodzącego i wychodzącego ruchu sieciowego. Grupy zabezpieczeń sieci musi istnieć w tej samej subskrypcji i lokalizacji co sieć wirtualna. On również należy utworzyć przy użyciu modelu wdrażania usługi Resource Manager. Aby dowiedzieć się więcej o sposobie tworzenia sieciowej grupy zabezpieczeń, zobacz [sieciowej grupy zabezpieczeń](virtual-networks-create-nsg-arm-pportal.md).
    - **Tabela tras**: Opcjonalnie można skojarzyć istniejącą tabelę tras dla podsieci do kontrolowania routingu ruchu sieciowego z innymi sieciami. Tabela tras musi istnieć w tej samej subskrypcji i lokalizacji co sieć wirtualna. On również należy utworzyć przy użyciu modelu wdrażania usługi Resource Manager. Aby dowiedzieć się więcej na temat sposobu tworzenia tabel tras, zobacz [trasy zdefiniowane przez użytkownika](virtual-network-create-udr-arm-ps.md).
    - **Użytkownicy**: dostęp do tej podsieci można kontrolować za pomocą wbudowanych ról lub własne niestandardowe role. Aby dowiedzieć się więcej na temat Przypisywanie ról i użytkownikom uzyskiwanie dostępu do tej podsieci, zobacz [umożliwia zarządzanie dostępem do zasobów platformy Azure przypisania roli](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. Aby dodać podsieci do wybranej sieci wirtualnej, kliknij przycisk **OK**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|[Utwórz az podsieci sieci wirtualnej](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nowy AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json), [AzureRmVirtualNetworkSubnetConfig Dodaj](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Zmień ustawienia podsieci

Można zmienić grupy zabezpieczeń sieci, tabele tras i dostęp użytkownika do podsieci za zarządzanie zasobami, które znajdują się w podsieci. Aby dowiedzieć się więcej o tych ustawieniach w [Dodaj podsieć](#create-subnet), zobacz krok 6. Jeśli chcesz zmienić przestrzeni adresowej podsieci, należy najpierw usunąć wszystkie zasoby, które znajdują się w podsieci. Czynności, które należy wykonać, aby usunąć zasób zależy od zasobu. Aby dowiedzieć się, jak usunąć zasoby, które znajdują się w podsieci, przeczytaj dokumentację dla każdego typu zasobu, który chcesz usunąć. Aby zmienić zakres adresów w podsieci:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, do którego przypisano uprawnienia roli współautora sieci (co najmniej) dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu wyszukiwania portalu wprowadź **sieci wirtualnych**. W wynikach wyszukiwania kliknij **sieci wirtualnych**.
3. Na **sieci wirtualnych** bloku, kliknij przycisk sieci wirtualnej, dla którego chcesz zmienić zakres adresów podsieci.
4. Kliknij przycisk podsieci, dla którego chcesz zmienić zakres adresów.
5. W bloku podsieci w **zakres adresów** wprowadź nowy zakres adresów. Zakres musi być unikatowa w ramach przestrzeni adresowej dla sieci wirtualnej. Zakres nie może nakładać się na inne zakresy adresów podsieci w sieci wirtualnej. Przestrzeń adresowa należy określić przy użyciu notacji CIDR. Na przykład w sieci wirtualnej z 10.0.0.0/16 przestrzeni adresów, można zdefiniować przestrzeni adresowej podsieci 10.0.0.0/24. Najmniejszą zakres, który można określić jest /29, co umożliwia osiem adresów IP podsieci. Azure rezerwuje pierwszy i ostatni adres w każdej podsieci dla zgodności protokołu. Trzy dodatkowe adresy są zarezerwowane do użycia usługi Azure. Dzięki temu podsieci z /29 zakres adresów zawiera trzy można używać adresów IP. Jeśli planowane jest połączenie wirtualnej sieci do bramy sieci VPN, należy utworzyć podsieć bramy. Dowiedz się więcej o [zagadnienia dotyczące zakresu określonego adresu podsieci bramy](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Zakres adresów można zmienić po utworzeniu podsieci w określonych warunkach. Aby dowiedzieć się, jak zmienić zakres adresów podsieci, zobacz [zmienić ustawienia podsieci](#change-subnet) w tym artykule.
6. Kliknij pozycję **Zapisz**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|[Aktualizacja podsieci sieci wirtualnej sieci az](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Zestaw AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Usuń podsieć

Tylko wtedy, gdy nie ma żadnych zasobów w podsieci, można usunąć podsieci. Jeśli dostępne są zasoby w tej podsieci, należy usunąć zasoby, które znajdują się w podsieci, aby można było usunąć podsieci. Czynności, które należy wykonać, aby usunąć zasób zależy od zasobu. Aby dowiedzieć się, jak usunąć zasoby, które znajdują się w podsieci, przeczytaj dokumentację dla każdego typu zasobu, który chcesz usunąć. Aby usunąć podsieć:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, do którego przypisano uprawnienia roli współautora sieci (co najmniej) dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu wyszukiwania portalu wprowadź **sieci wirtualnych**. W wynikach wyszukiwania kliknij **sieci wirtualnych**.
3. Na **sieci wirtualnych** bloku, kliknij do usunięcia z podsiecią sieci wirtualnej.
4. Na wirtualnej sieci bloku, w obszarze **ustawienia**, kliknij przycisk **podsieci**.
5. Na liście podsieci jest wyświetlana w bloku podsieci, kliknij prawym przyciskiem myszy tej podsieci, które chcesz usunąć, kliknij przycisk **usunąć**, a następnie kliknij przycisk **tak** Aby usunąć podsieć.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|[AZ sieci usunięcie sieci wirtualnej](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Usuń AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Następne kroki

Aby utworzyć maszynę wirtualną w podsieci, zobacz [tworzenie sieci wirtualnej i wdrażanie maszyn wirtualnych w podsieci](virtual-network-get-started-vnet-subnet.md#create-vms).
