---
title: Tworzenie, zmienianie lub usuwanie sieci wirtualnej platformy Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak tworzenie, zmienianie lub usuwanie sieci wirtualnej na platformie Azure."
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
ms.openlocfilehash: 6a4a4d29cbfa8a695ed1ad3f007e6ff3e859bda4
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="create-change-or-delete-a-virtual-network"></a>Tworzenie, zmienianie lub usuwanie sieci wirtualnej

Dowiedz się, jak utworzyć i usunąć sieci wirtualnej i zmienić ustawienia, takie jak serwery DNS i przestrzeni adresów IP, do istniejącej sieci wirtualnej.

Sieć wirtualna jest odzwierciedla w chmurze Twoją sieć. Sieć wirtualna jest logiczną izolacją chmury Azure, przeznaczona dla Twojej subskrypcji platformy Azure. Dla każdej sieci wirtualnej, utworzony można:
- Wybierz przestrzeń adresową można przypisać. Przestrzeń adresową składa się z jednego lub więcej zakresów adresów, które są zdefiniowane przy użyciu notacji Classless Inter-Domain Routing (CIDR), takich jak 10.0.0.0/16.
- Wybierz użyć serwera DNS platformy Azure lub serwera DNS. Wszystkie zasoby, które są podłączone do sieci wirtualnej są przypisywane tego serwera DNS do rozpoznawania nazw w sieci wirtualnej.
- Segment sieci wirtualnej do podsieci, każda z własną zakres adresów w przestrzeni adresowej sieci wirtualnej. Aby dowiedzieć się, jak tworzenie, zmienianie i usuwanie podsieci, zobacz [Dodawanie, zmienianie lub usuń podsieci](virtual-network-manage-subnet.md).

W tym artykule wyjaśniono, jak tworzenie, zmienianie i usuwanie sieci wirtualnych za pomocą modelu wdrażania usługi Azure Resource Manager.

## <a name="before"></a>Przed rozpoczęciem

Przed rozpoczęciem zadań, które zostały opisane w tym artykule, należy wykonać następujące wymagania wstępne:

- Jeśli zaczynasz pracę z sieciami wirtualnymi, firma Microsoft zaleca przejrzenie wykonywania w [tworzenie sieci wirtualnej platformy Azure pierwsze](quick-create-portal.md). Tego ćwiczenia ułatwiają zapoznanie się ze sieci wirtualnych.
- Informacje na temat limitów dla sieci wirtualnej, przejrzyj [limity Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Zaloguj się do portalu Azure, narzędzia wiersza polecenia platformy Azure (Azure CLI) lub Azure PowerShell przy użyciu konta platformy Azure. Jeśli nie masz konta platformy Azure, należy zarejestrować się w celu [bezpłatnego konta wersji próbnej](https://azure.microsoft.com/free).
- Jeśli planujesz używać poleceń programu PowerShell, aby wykonać zadania opisane w tym artykule, należy najpierw [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję poleceń cmdlet programu Azure PowerShell zainstalowane. Aby uzyskać pomoc dotyczącą poleceń programu PowerShell w przykładach, wprowadź `get-help <command> -full`.
- Jeśli planujesz używać poleceń interfejsu wiersza polecenia Azure, aby wykonać zadania opisane w tym artykule, należy najpierw [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia Azure, wprowadź `az <command> --help`.


## <a name="create-vnet"></a>Tworzenie sieci wirtualnej

Aby utworzyć sieć wirtualną:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, do którego przypisano uprawnienia roli współautora sieci (co najmniej) dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Kliknij przycisk **nowy** > **sieci** > **sieci wirtualnej**.
3. Na **sieci wirtualnej** bloku, w **wybierz model wdrożenia** pozostaw **Resource Manager** zaznaczone, a następnie kliknij przycisk **Utwórz**.
4. Na **Utwórz sieć wirtualną** bloku, wprowadź lub wybierz wartości poniższych ustawień, a następnie kliknij przycisk **Utwórz**:
    - **Nazwa**: Nazwa musi być unikatowa w [grupy zasobów](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) umożliwia tworzenie sieci wirtualnej w. Nie można zmienić nazwy po utworzeniu sieci wirtualnej. Wraz z upływem czasu, można utworzyć wiele sieci wirtualnych. Nazewnictwa sugestii, zobacz [konwencje nazewnictwa](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Po konwencji nazewnictwa może pomóc ułatwiają zarządzanie wieloma sieciami wirtualnymi.
    - **Przestrzeń adresowa**: Określ przestrzeń adresów w notacji CIDR. Przestrzeń adresowa zdefiniowanych można publicznych lub prywatnych (RFC 1918). Czy przestrzeń adresowa można zdefiniować jako publicznych lub prywatnych, przestrzeń adresowa jest dostępny tylko w ramach sieci wirtualnej z połączonych sieci wirtualnych i sieciami lokalnymi podłączonych do sieci wirtualnej. Nie można dodać następujące przestrzenie adresowe:
        - 224.0.0.0/4 (multiemisji)
        - 255.255.255.255/32 (emisji)
        - 127.0.0.0/8 (sprzężenie zwrotne)
        - 169.254.0.0/16 (Link-local)
        - 168.63.129.16/32 (wewnętrzny serwer DNS)

      Mimo że można zdefiniować tylko jedną przestrzeń adresową, podczas tworzenia sieci wirtualnej, można dodać więcej przestrzenie adresowe po utworzeniu sieci wirtualnej. Aby dowiedzieć się, jak dodać do istniejącej sieci wirtualnej przestrzeni adresowej, zobacz [Dodaj lub Usuń przestrzeń adresową](#add-address-spaces) w tym artykule.

      >[!WARNING]
      >Jeśli sieć wirtualna ma przestrzeni adresów, które nakładają się innej sieci wirtualnej lub lokalnej sieci, nie można połączyć dwie sieci. Przed zdefiniowaniem przestrzeń adresową, rozważ, czy można połączyć sieć wirtualną do innych sieci wirtualnych lub sieciami lokalnymi w przyszłości.
      >
      >

    - **Nazwa podsieci**: nazwy podsieci muszą być unikatowe w ramach sieci wirtualnej. Nie można zmienić nazwy podsieci, po utworzeniu podsieci. Portal wymaga zdefiniowania w jednej podsieci po utworzeniu sieci wirtualnej, nawet jeśli sieć wirtualna nie jest wymagane do ma żadnych podsieci. W portalu można zdefiniować tylko jedną podsieć, podczas tworzenia sieci wirtualnej. Można dodać więcej podsieci do sieci wirtualnej później, po utworzeniu sieci wirtualnej. Aby dodać podsieci do sieci wirtualnej, zobacz [Utwórz podsieć](virtual-network-manage-subnet.md#create-subnet) w [tworzenie, zmienianie i usuwanie podsieci](virtual-network-manage-subnet.md). Można utworzyć sieci wirtualnej, który ma wiele podsieci przy użyciu wiersza polecenia platformy Azure lub programu PowerShell.

      >[!TIP]
      >Czasami Administratorzy utworzyć różne podsieci filtrowanie lub kontrolować ruch routing między podsieciami. Przed zdefiniowaniem podsieci, należy rozważyć sposób można filtrować i kierować ruchem między podsieci. Aby dowiedzieć się więcej na temat filtrowania ruchu między podsieciami, zobacz [sieciowej grupy zabezpieczeń](virtual-networks-nsg.md). Azure automatycznie trasy ruch między podsieciami, ale można zastąpić trasy domyślne platformy Azure. Aby dowiedzieć się, jak zastąpić routingu ruchu podsieci domyślne Azure, zobacz [trasy zdefiniowane przez użytkownika](virtual-networks-udr-overview.md).
      >

    - **Zakres adresów podsieci**: zakres musi być w przestrzeni adresowej dla sieci wirtualnej. Najmniejszą zakres, który można określić jest /29, co umożliwia osiem adresów IP podsieci. Azure rezerwuje pierwszy i ostatni adres w każdej podsieci dla zgodności protokołu. Trzy dodatkowe adresy są zarezerwowane do użycia usługi Azure. W związku z tym sieci wirtualnej z zakresem adresów podsieci /29 zawiera tylko trzy można używać adresów IP. Jeśli planowane jest połączenie wirtualnej sieci do bramy sieci VPN, należy utworzyć podsieć bramy. Dowiedz się więcej o [zagadnienia dotyczące zakresu określonego adresu podsieci bramy](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Zakres adresów można zmienić po utworzeniu podsieci w określonych warunkach. Aby dowiedzieć się, jak zmienić zakres adresów podsieci, zobacz [zmienić ustawienia podsieci](#change-subnet) w [Dodawanie, zmienianie lub usuń podsieci](virtual-network-manage-subnet.md).
    - **Subskrypcja**: Wybierz [subskrypcji](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Nie można użyć tej samej sieci wirtualnej w więcej niż jedną subskrypcją platformy Azure. Można jednak połączyć sieci wirtualnej w ramach jednej subskrypcji do sieci wirtualnych w inne subskrypcje. Aby połączyć sieci wirtualnych w ramach różnych subskrypcji, użyj bramy sieci VPN platformy Azure lub sieci wirtualnej komunikacji równorzędnej. Zasobów platformy Azure, podłączoną do sieci wirtualnej muszą być w tej samej subskrypcji co sieć wirtualna.
    - **Grupa zasobów**: Wybierz istniejący [grupy zasobów](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) lub Utwórz nową. Zasobów platformy Azure, podłączoną do sieci wirtualnej może być w tej samej grupie zasobów co sieć wirtualna lub w innej grupie zasobów.
    - **Lokalizacja**: wybierz pozycję Azure [lokalizacji](https://azure.microsoft.com/regions/), znanej również jako regionu. Sieć wirtualna może być w tylko jednej lokalizacji platformy Azure. Można jednak połączyć sieć wirtualną w jednej lokalizacji do sieci wirtualnej w innej lokalizacji przy użyciu bramy sieci VPN. Zasobów platformy Azure, podłączoną do sieci wirtualnej musi być w tej samej lokalizacji co sieć wirtualna.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|[Tworzenie sieci wirtualnej sieci az](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_vnet_create)|
|PowerShell|[New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Widok sieci wirtualnych i ustawień

Aby wyświetlić ustawienia i sieci wirtualnych:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, do którego przypisano uprawnienia roli współautora sieci (co najmniej) dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu wyszukiwania portalu wprowadź **sieci wirtualnych**. W wynikach wyszukiwania kliknij **sieci wirtualnych**.
3. Na **sieci wirtualnych** bloku, kliknij przycisk sieci wirtualnej, który chcesz wyświetlić ustawienia.
4. Następujące ustawienia są wyświetlane w bloku dla wybranej sieci wirtualnej:
    - **Omówienie**: zawiera informacje o sieci wirtualnej, w tym przestrzeń adresową i serwery DNS. Poniższy zrzut ekranu przedstawia Przegląd ustawień sieci wirtualnej o nazwie **MyVNet**:

        ![Omówienie interfejsu sieciowego](./media/virtual-network-manage-network/vnet-overview.png)

      Na **omówienie** bloku sieci wirtualnej można przenieść do innej grupy zasobów lub subskrypcji. Aby dowiedzieć się, jak przenieść sieć wirtualną, zobacz [przenoszenia zasobów w innej grupie zasobów lub subskrypcji](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Artykuł zawiera listę wymagań wstępnych i sposobu przenoszenia zasobów przy użyciu portalu Azure, programu PowerShell i interfejsu wiersza polecenia Azure. Wszystkie zasoby, które są podłączone do sieci wirtualnej, należy przenieść z siecią wirtualną.
    - **Przestrzeń adresowa**: znajdują się przestrzeni adresowych, które są przypisane do sieci wirtualnej. Aby dowiedzieć się, jak dodać i usunąć przestrzeń adresową, wykonaj kroki [Dodaj lub Usuń przestrzeń adresową](#address-spaces) w tym artykule.
    - **Połączone urządzenia**: wymienione są wszystkie zasoby, które są podłączone do sieci wirtualnej. Na poprzednim zrzucie ekranu pokazano trzy interfejsy sieciowe i jeden moduł równoważenia obciążenia są podłączone do sieci wirtualnej. Wyświetlane są nowe zasoby, które tworzą i połączyć sieć wirtualną. Jeśli usuniesz z zasobem, który został podłączony do sieci wirtualnej, nie będzie widoczny na liście.
    - **Podsieci**: przedstawiono listę podsieci, które istnieją w sieci wirtualnej. Aby dowiedzieć się, jak dodać i usunąć podsieć, zobacz [Utwórz podsieć](virtual-network-manage-subnet.md#create-subnet) i [usunąć podsieć](virtual-network-manage-subnet.md#delete-subnet) w [Dodawanie, zmienianie lub usuń podsieci](virtual-network-manage-subnet.md).
    - **Serwery DNS**: można określić, czy Azure wewnętrznego serwera DNS lub niestandardowy serwer DNS udostępnia rozpoznawanie nazw dla urządzeń, które są podłączone do sieci wirtualnej. Po utworzeniu sieci wirtualnej przy użyciu portalu Azure, serwery DNS platformy Azure są używane do rozpoznawania nazw w sieci wirtualnej domyślnie. Aby zmodyfikować serwery DNS, wykonaj kroki [Dodawanie, zmienianie i usuwanie serwera DNS](#dns-servers) w tym artykule.
    - **Komunikacji równorzędnych**: Jeśli istnieją istniejącego komunikacji równorzędnych w subskrypcji, są one wyświetlane tutaj. Można wyświetlać ustawienia dla istniejącego komunikacji równorzędnych, lub utworzyć, zmienić lub usunąć komunikacji równorzędnych. Aby dowiedzieć się więcej na temat komunikacji równorzędnych, zobacz [równorzędna sieci wirtualnej](virtual-network-peering-overview.md).
    - **Właściwości**: Wyświetla ustawienia dotyczące sieci wirtualnej, w tym identyfikator zasobu sieci wirtualnej i subskrypcji platformy Azure w.
    - **Diagram**: na diagramie przedstawiono wizualną reprezentację wszystkich urządzeń, które są podłączone do sieci wirtualnej. Diagram zawiera niektóre najważniejsze informacje na temat urządzeń. Do zarządzania urządzeniem, w tym widoku diagramu, kliknij urządzenie.
    - **Typowe ustawienia Azure**: Aby dowiedzieć się więcej na temat typowych ustawień platformy Azure, zobacz następujące informacje:
        *   [Dziennik aktywności](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Kontrola dostępu (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Tagi](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Blokady](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Skrypt automatyzacji](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|[Pokaż sieci wirtualnej sieci az](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_vnet_show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Dodaj lub Usuń przestrzeń adresową

Można dodawać i usuwać przestrzeni adresów sieci wirtualnej. Przestrzeń adresowa musi być określony w notacji CIDR, a nie może nakładać się z innymi obszarami adresów w obrębie tej samej sieci wirtualnej. Przestrzeni adresowych, które należy zdefiniować można publicznych lub prywatnych (RFC 1918). Czy przestrzeń adresowa można zdefiniować jako publicznych lub prywatnych, przestrzeń adresowa jest dostępny tylko w ramach sieci wirtualnej z połączonych sieci wirtualnych i sieciami lokalnymi podłączonych do sieci wirtualnej. Nie można dodać następujące przestrzenie adresowe:

- 224.0.0.0/4 (multiemisji)
- 255.255.255.255/32 (emisji)
- 127.0.0.0/8 (sprzężenie zwrotne)
- 169.254.0.0/16 (Link-local)
- 168.63.129.16/32 (wewnętrzny serwer DNS)

Aby dodać lub usunąć przestrzeń adresową:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, do którego przypisano uprawnienia roli współautora sieci (co najmniej) dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu wyszukiwania portalu wprowadź **sieci wirtualnych**. W wynikach wyszukiwania wybierz **sieci wirtualnych**.
3. Na **sieci wirtualnych** bloku, kliknij przycisk sieci wirtualnej, dla której chcesz dodać lub usunąć przestrzeń adresową.
4. Na wirtualnej sieci bloku, w obszarze **ustawienia**, kliknij przycisk **przestrzeni adresów**.
5. W bloku przestrzeni adresowej wykonaj jedną z następujących opcji:
    - **Dodaj przestrzeń adresową**: Wprowadź nowy przestrzeni adresowej. Przestrzeń adresowa nie nakłada się na istniejącą przestrzeń adresową, która jest zdefiniowana dla sieci wirtualnej.
    - **Usuń przestrzeń adresową**: kliknij prawym przyciskiem myszy na przestrzeń adresową, a następnie kliknij przycisk **Usuń**. Jeśli podsieć istnieje w przestrzeni adresowej, nie można usunąć przestrzeni adresowej. Aby usunąć przestrzeń adresową, należy najpierw usunąć wszystkie podsieci (i wszystkie zasoby, które są połączone z podsieciami) który istnieje w przestrzeni adresowej.
6. Kliknij pozycję **Zapisz**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|Resource Manager|[Aktualizacja sieci wirtualnej sieci az](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_vnet_update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Dodawanie, zmienianie lub usuwanie serwera DNS

Wszystkie maszyny wirtualne, które są podłączone do sieci wirtualnej rejestr przy użyciu serwerów DNS, które określają dla sieci wirtualnej. Określony serwer DNS są również używany do rozpoznawania nazw. Każdego interfejsu sieciowego (NIC) na maszynie wirtualnej może mieć własne ustawienia serwera DNS. Jeśli karta sieciowa ma swoje własne ustawienia serwera DNS, zastępują one ustawienia serwera DNS dla sieci wirtualnej. Aby dowiedzieć się więcej o ustawieniach DNS kart interfejsu Sieciowego, zobacz [interfejsu zadań i ustawień sieci](virtual-network-network-interface.md#change-dns-servers). Aby dowiedzieć się więcej na temat rozpoznawania nazw dla maszyn wirtualnych i wystąpień roli w usług Azure Cloud Services, zobacz [rozpoznawanie nazw dla maszyn wirtualnych i wystąpień roli](virtual-networks-name-resolution-for-vms-and-role-instances.md). Aby dodać, zmienić lub usunąć serwer DNS:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, do którego przypisano uprawnienia roli współautora sieci (co najmniej) dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu wyszukiwania portalu wpisz **sieci wirtualnych**. W wynikach wyszukiwania wybierz **sieci wirtualnych**.
3. Na **sieci wirtualnych** bloku, kliknij, aby zmienić ustawienia DNS dla sieci wirtualnej.
4. Na wirtualnej sieci bloku, w obszarze **ustawienia**, kliknij przycisk **serwerów DNS**.
5. Wybierz jedną z następujących opcji w bloku, który wyświetla listę serwerów DNS:
    - **Domyślne (zakładając Azure)**: wszystkie nazwy zasobów i prywatnych adresów IP są automatycznie rejestrowane na serwerach DNS platformy Azure. Można rozwiązać nazwy między wszystkie zasoby, które są podłączone do tej samej sieci wirtualnej. Nie można użyć tej opcji do rozpoznawania nazw w sieciach wirtualnych. Rozpoznawanie nazw w sieciach wirtualnych, należy użyć niestandardowego serwera DNS.
    - **Niestandardowe**: można dodać jeden lub więcej serwerów w granicach Azure dla sieci wirtualnej. Aby dowiedzieć się więcej na temat limity serwera DNS, zobacz [limity Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Istnieją następujące opcje:
        - **Dodaj adres**: dodaje serwer do listy serwerów DNS w sieci wirtualnej. Ta opcja również rejestruje serwer DNS z platformy Azure. Jeśli serwer DNS został już zarejestrowany przy użyciu platformy Azure, można wybrać tego serwera DNS na liście.
        - **Usuń adres**: obok serwera, który chcesz usunąć, kliknij przycisk **X**. Usunięcie serwera spowoduje usunięcie serwera tylko z tej listy sieci wirtualnej. Serwer DNS pozostaje zarejestrowane na platformie Azure do innych sieci wirtualne do użycia.
        - **Zmień kolejność adresów serwerów DNS**: ważne jest, aby sprawdzić, wyświetlić listę serwerów DNS w odpowiedniej kolejności dla danego środowiska. Listy serwera DNS są używane w kolejności, w jakiej zostały określone. Nie działają jako Instalator okrężnego. Jeśli pierwszy serwer DNS na liście jest osiągalna, klient korzysta z tego serwera DNS, niezależnie od tego, czy serwer DNS działa prawidłowo. Usuń wszystkie serwery DNS, które są wyświetlane, a następnie dodaj je ponownie w kolejności, która ma.
        - **Zmień adres**: zaznacz serwer DNS na liście, a następnie wprowadź nową nazwę.
6. Kliknij pozycję **Zapisz**.
7. Ponowne uruchomienie maszyn wirtualnych, które są podłączone do sieci wirtualnej, aby nowe ustawienia serwera DNS są przypisane. Maszyny wirtualne w dalszym ciągu używać ich bieżące ustawienia DNS, dopóki nie zostaną ponownie uruchomione.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|[Aktualizacja sieci wirtualnej sieci az](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_vnet_update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Usunąć sieci wirtualnej

Tylko wtedy, gdy nie ma żadnych zasobów dołączone do niego, można usunąć sieci wirtualnej. Jeśli istnieją zasoby podłączone do żadnej podsieci w sieci wirtualnej, należy najpierw usunąć zasoby, które są podłączone do wszystkich podsieci w sieci wirtualnej. Czynności, które należy wykonać, aby usunąć zasób zależy od zasobu. Aby dowiedzieć się, jak usunąć zasoby, które są podłączone do podsieci, przeczytaj dokumentację dla każdego typu zasobu, który chcesz usunąć. Aby usunąć sieć wirtualną:

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Aby dowiedzieć się więcej na temat Przypisywanie ról i uprawnień do kont, zobacz [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. W polu wyszukiwania portalu wprowadź **sieci wirtualnych**. W wynikach wyszukiwania kliknij **sieci wirtualnych**.
3. Na **sieci wirtualnych** bloku, wybierz sieć wirtualną do usunięcia.
4. W bloku sieci wirtualnej, aby upewnić się, że nie ma żadnych urządzeń podłączony do sieci wirtualnej, a w obszarze **ustawienia**, kliknij przycisk **urządzeń podłączonych**. W przypadku połączonych urządzeń, należy je usunąć przed usunięciem sieci wirtualnej. Jeśli nie ma żadnych podłączonych urządzeń, kliknij przycisk **omówienie**.
5. W górnej części bloku, kliknij **usunąć** ikony.
6. Aby potwierdzić usunięcie sieci wirtualnej, kliknij przycisk **tak**.


**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia platformy Azure|[Usuń sieć wirtualną sieć platformy Azure](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#az_network_vnet_delete)|
|PowerShell|[Remove-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Następne kroki

- Aby utworzyć Maszynę wirtualną, a następnie podłącz je do sieci wirtualnej, zobacz [utworzyć sieć wirtualną i połączyć maszyny wirtualne](quick-create-portal.md#create-virtual-machines).
- Aby filtrować ruch sieciowy między podsieciami sieci wirtualnej, zobacz [Utwórz grupy zabezpieczeń sieci](virtual-networks-create-nsg-arm-pportal.md).
- -To-peer sieci wirtualnej do innej sieci wirtualnej, zobacz [tworzenie sieci wirtualnej komunikacji równorzędnej](virtual-network-create-peering.md#portal).
- Aby dowiedzieć się więcej o opcjach sieci wirtualnej nawiązywania połączenia z siecią lokalną, zobacz [o bramy sieci VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
