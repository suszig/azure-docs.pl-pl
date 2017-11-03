---
title: "Tworzenie, zmienianie lub usuwanie równorzędna sieci wirtualnej platformy Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć, zmienić lub usunąć sieci wirtualnej komunikacji równorzędnej."
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
ms.date: 09/25/2017
ms.author: jdial;anavin
ms.openlocfilehash: 9fcfca3de6204581936a2bacfd86e84fd373190a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Tworzenie, zmienianie lub usunąć sieci wirtualnej komunikacji równorzędnej

Dowiedz się, jak utworzyć, zmienić lub usunąć sieci wirtualnej komunikacji równorzędnej. Sieci wirtualnej komunikacji równorzędnej umożliwia łączenie z sieciami wirtualnymi w sieci Azure sieci szkieletowej. Po połączyć za pomocą, sieci wirtualne są nadal zarządzane jako oddzielne zasoby. Jeśli nie masz doświadczenia w obsłudze sieci wirtualnej komunikacji równorzędnej, zaleca się odczytu [komunikacji równorzędnej omówienie sieci wirtualnej](virtual-network-peering-overview.md) i wykonując [tworzenie sieci wirtualnej komunikacji równorzędnej samouczek](virtual-network-create-peering.md), przed wykonaniem zadania w tym artykule.

Łączenie sieci wirtualnych za pomocą komunikacji równorzędnej w tym samym regionie jest ogólnie dostępne. Łączenie sieci wirtualnych za pomocą komunikacji równorzędnej w różnych regionach jest obecnie dostępne w wersji zapoznawczej w następujących regionach: Zachodnio-środkowe stany USA, Kanada Środkowa i Zachodnie stany USA 2. Możesz [zarejestrować subskrypcję w celu korzystania z wersji zapoznawczej](virtual-network-create-peering.md).

> [!WARNING]
> Połączenia sieci wirtualnych za pomocą komunikacji równorzędnej, które utworzono w tym scenariuszu, mogą nie mieć takiego samego poziomu dostępności i niezawodności jak scenariusze w wersji ogólnodostępnej. Połączenia sieci wirtualnych za pomocą komunikacji równorzędnej mogą mieć ograniczone możliwości i mogą nie być dostępne we wszystkich regionach platformy Azure. Najbardziej aktualne powiadomienia dotyczące dostępności i stanu tej funkcji można znaleźć na stronie [aktualizacji usługi Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).
>

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed wykonaniem kroków w żadnej sekcji tego artykułu, należy wykonać następujące zadania:

- Przegląd [Azure ogranicza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artykułu, aby dowiedzieć się więcej na temat limitów dla komunikacji równorzędnej.
- Zaloguj się do portalu Azure, Azure interfejsu wiersza polecenia (CLI) lub Azure PowerShell przy użyciu konta platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, należy zarejestrować się w celu [bezpłatnego konta wersji próbnej](https://azure.microsoft.com/free).
- Jeśli za pomocą poleceń programu PowerShell do wykonywania zadań w tym artykule [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję poleceń cmdlet programu Azure PowerShell zainstalowane. Aby uzyskać pomoc dotyczącą poleceń programu PowerShell, wraz z przykładami, wpisz `get-help <command> -full`.
- Jeśli za pomocą poleceń Azure interfejsu wiersza polecenia (CLI) do wykonywania zadań w tym artykule [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia, wpisz `az <command> --help`. Zamiast instalowania interfejsu wiersza polecenia i jego wymagania wstępne, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Powłoka chmury ma Azure CLI wstępnie zainstalowane i skonfigurowane do użycia z Twoim kontem. Aby użyć powłoki chmury, kliknij przycisk powłoki chmury **> _** przycisk w górnej części [portal](https://portal.azure.com). 

## <a name="create-a-peering"></a>Utwórz element równorzędny

>[!NOTE]
>Przed utworzeniem komunikacji równorzędnej, upewnij się, należy po zapoznaniu się z [wymagań i ograniczeń](#requirements-and-constraints) i [wymagane uprawnienia](#permissions).
>

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta przypisanego niezbędnych [roli lub uprawnienia](#permissions).
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *sieci wirtualnych*. Gdy **sieci wirtualnych** pojawia się w wynikach wyszukiwania kliknij ją. Nie zaznaczaj **sieci wirtualnych (klasyczne)** Jeśli wygląda na to, na liście, ponieważ nie można utworzyć komunikacji równorzędnej z sieci wirtualnej wdrożone za pośrednictwem klasycznego modelu wdrażania.
3. W **sieci wirtualnych** bloku, który jest wyświetlany, kliknij przycisk chcesz utworzyć komunikacji równorzędnej dla sieci wirtualnej.
4. W okienku jest wyświetlana w przypadku wybrania sieci wirtualnej, kliknij przycisk **komunikacji równorzędnych** w **ustawienia** sekcji.
5. Kliknij przycisk **+ Dodaj**. 
6. <a name="add-peering"></a>W **dodać równorzędna** bloku, wprowadź lub wybierz wartości poniższych ustawień:
    - **Nazwa:** nazwa dla komunikacji równorzędnej musi być unikatowa w ramach sieci wirtualnej.
    - **Model wdrażania sieci wirtualnej:** wybierz sieci wirtualnej, aby równorzędny model wdrożenia, które zostało wdrożone za pośrednictwem.
    - **Sprawdzić mój identyfikator zasobu:** Jeśli masz dostęp do odczytu do sieci wirtualnej, aby równorzędny, pozostaw to pole wyboru niezaznaczone. Jeśli nie masz dostęp do odczytu do sieci wirtualnej lub subskrypcji, który chcesz równorzędny, zaznacz to pole wyboru. Wprowadź pełny identyfikator zasobu sieci wirtualnej, aby równorzędny w **identyfikator zasobu** pola, które wystąpiły, gdy zaznaczono pole. Możesz wprowadzić identyfikator zasobu musi być sieci wirtualnej, który istnieje w tej samej Azure [region](https://azure.microsoft.com/regions) jako tej sieci wirtualnej. Jeśli chcesz wybrać sieć wirtualną w innym regionie [zarejestrować subskrypcji dla podglądu.](virtual-network-create-peering.md) Pełny identyfikator zasobu wygląda podobnie do /subscriptions/<Id>/providers/Microsoft.Network/virtualNetworks/ <-nazwa grupy zasobów-> /resourceGroups/ < nazwa wirtualnej sieci >. Identyfikator zasobu można uzyskać sieci wirtualnej, wyświetlając właściwości dla sieci wirtualnej. Aby dowiedzieć się wyświetlić właściwości dla sieci wirtualnej, zobacz [Zarządzanie sieciami wirtualnymi](virtual-network-manage-network.md#view-vnet).
    - **Subskrypcja:** wybierz [subskrypcji](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) chcesz elementu równorzędnego z sieci wirtualnej. Co najmniej jedna subskrypcja znajdują się w zależności od tego, jak wiele subskrypcji Twoje konto ma uprawnienia do odczytu. Jeśli zaznaczono **identyfikator zasobu** pole wyboru, to ustawienie jest niedostępne. Tak długo, jak obie sieci wirtualne utworzone za pomocą Menedżera zasobów można elementów równorzędnych sieci wirtualnych w ramach różnych subskrypcji. Możliwość elementów równorzędnych w subskrypcjach utworzone przez różne modele wdrażania jest w wersji zapoznawczej. Zarejestruj się w wersji zapoznawczej przed utworzeniem komunikacji równorzędnej między sieciami wirtualnymi, wdrażać przy użyciu różne modele wdrażania znajdujące się w różnych subskrypcji. Dowiedz się więcej o tym, jak zarejestrować w wersji zapoznawczej i [elementów równorzędnych sieci wirtualnych utworzonych przez różne modele wdrażania w ramach różnych subskrypcji](create-peering-different-deployment-models-subscriptions.md).
    - **Sieć wirtualna:** wybierz sieć wirtualną, aby równorzędny. Możesz wybrać sieć wirtualną, która została utworzona za pośrednictwem albo modelu wdrożenia usługi Azure. Jeśli chcesz wybrać sieć wirtualną w innym regionie [zarejestrować subskrypcji dla podglądu.](virtual-network-create-peering.md) Musi mieć dostęp do odczytu do sieci wirtualnej, aby były widoczne na liście. Jeśli sieć wirtualna jest na liście, ale wygaszone, prawdopodobnie przestrzeni adresowej dla sieci wirtualnej nakłada się przestrzeni adresowej tej sieci wirtualnej. Jeśli nakładania się przestrzenie adresowe sieci wirtualnej, ich nie można połączyć za pomocą. Jeśli zaznaczono **identyfikator zasobu** pole wyboru, to ustawienie jest niedostępne.
    - **Zezwalaj na dostęp do sieci wirtualnej:** wybierz **włączone** (ustawienie domyślne), jeśli chcesz umożliwić komunikację między dwiema sieciami wirtualnymi. Włączenie komunikacji między sieciami wirtualnymi umożliwia zasoby podłączone do jednej sieci wirtualnej do komunikowania się ze sobą przy tym samym przepustowości i opóźnień, jakby były one podłączone do tej samej sieci wirtualnej. Cała komunikacja między zasobami w dwóch sieci wirtualnych jest za pośrednictwem sieci prywatnej platformy Azure. **VirtualNetwork** domyślne znaczniki dla grup zabezpieczeń sieci obejmuje sieci wirtualnej i połączyć za pomocą sieci wirtualnej. Aby dowiedzieć się więcej na temat znaczników domyślnych grup zabezpieczeń sieci, przeczytaj [Przegląd grup zabezpieczeń sieci](virtual-networks-nsg.md#default-tags) artykułu.  Wybierz **wyłączone** Jeśli nie chcesz, aby przepływ ruchu do peered sieci wirtualnej. Możesz wybrać pozycję **wyłączone** już połączyć za pomocą sieci wirtualnej z inną siecią wirtualną, ale czasami chcesz wyłączyć przepływu ruchu między dwiema sieciami wirtualnymi. Może się okazać się, że włączenie/wyłączenie funkcji jest wygodniejsze niż usunięcia i ponownego utworzenia komunikacji równorzędnych. Gdy to ustawienie jest wyłączone, ruch nie przepływa między sieciami wirtualnymi peered.
    - **Zezwalaj na związanego z przekazywaniem ruchu:** zaznacz to pole, aby zezwolić na ruch kierowany do sieci wirtualnej peered (ruch w sieci wirtualnej peered) do przepływu z tą siecią wirtualną. Przekazywanie ruchu jest wspólne, po wdrożeniu urządzenie sieci wirtualnej w sieci wirtualnej jest równorzędna z i utworzyć trasy zdefiniowane przez użytkownika, aby przesyłał dalej ruch przez urządzenie wirtualne sieci. Jeżeli pole to unchecked (ustawienie domyślne), ruch przesyłany dalej z peered sieci wirtualnej nie może przepływać do tej sieci wirtualnej. Chociaż ruchu przekazywane przez komunikację równorzędną pozwala na włączenie tej funkcji, nie powoduje utworzenia trasy zdefiniowane przez użytkownika ani wirtualnych urządzeń sieciowych. Trasy zdefiniowane przez użytkownika i wirtualnych urządzeń sieciowych są tworzone oddzielnie. Dowiedz się więcej o [trasy zdefiniowane przez użytkownika](virtual-networks-udr-overview.md).
    - **Zezwalaj na bramy przesyłania:** zaznacz to pole wyboru, jeśli masz bramy sieci wirtualnej, dołączony do tej sieci wirtualnej i chcesz zezwolić na ruch z sieci wirtualnej peered przepływają przez bramę. Na przykład ta sieć wirtualna może zostać dołączony do sieci lokalnej za pośrednictwem bramy sieci wirtualnej. Zaznaczenie tego pola wyboru zezwala na ruch z sieci wirtualnej peered przepływ bram dołączony do tej sieci wirtualnej. Jeśli zaznaczysz to pole peered sieci wirtualnej nie może mieć skonfigurowaną bramę. Peered sieć wirtualna musi mieć **Użyj bramy zdalnego** zaznaczone pole wyboru, podczas konfigurowania komunikacji równorzędnej z innych sieci wirtualnej z tą siecią wirtualną. Pozostawienie to niezaznaczone pole (ustawienie domyślne), ruch z przepływem wciąż połączyć za pomocą sieci wirtualnej z tą siecią wirtualną, ale nie można wykonać przepływu za pośrednictwem bramy sieci wirtualnej dołączony do tej sieci wirtualnej. Dowiedz się więcej o [bram sieci wirtualnej](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti). 
    
        Nie można włączyć tę opcję, jeśli w przypadku komunikacji równorzędnej sieć wirtualną (Resource Manager) z siecią wirtualną (klasyczny). Chociaż ruch przepływa między dwiema sieciami wirtualnymi, (klasyczne) ruchu w sieci wirtualnej nie może przepływać przez bramę sieci dołączony do sieci wirtualnej (Resource Manager).

    - **Użyj bramy zdalnego:** zaznacz to pole, aby zezwolić na ruch z tą siecią wirtualną przepływają przez bramę sieci wirtualnej dołączony do sieci wirtualnej jest równorzędna z. Na przykład sieci wirtualnej, który jest równorzędna z ma bramy sieci VPN dołączony umożliwiający komunikację z siecią lokalną.  Zaznaczenie tego pola wyboru zezwala na ruch z tą siecią wirtualną przepływ za pośrednictwem bramy sieci VPN, dołączony do peered sieci wirtualnej. Jeśli zaznaczysz to pole musi mieć do niego dołączony bramę sieci wirtualnej peered sieci wirtualnej i musi mieć **Zezwalaj bramy przesyłania** zaznaczono element checkbox. Jeśli pole to unchecked (ustawienie domyślne), ruch z sieci wirtualnej peered nadal mogą przepływać do tej sieci wirtualnej, ale nie może przepływu za pośrednictwem bramy sieci wirtualnej dołączony do tego wirtualnego sieci. 
    
        Nie można włączyć tę opcję, jeśli w przypadku komunikacji równorzędnej sieć wirtualną (Resource Manager) z siecią wirtualną (klasyczny). Chociaż ruch przepływa między dwiema sieciami wirtualnymi, wirtualnych ruchu sieciowego (Resource Manager) nie można wykonać przepływu przez bramę sieci dołączony do sieci wirtualnej (wdrożenia klasyczne).
7. Kliknij przycisk **OK** przycisk, aby dodać podsieci do wybrania sieci wirtualnej.

### <a name="commands"></a>Polecenia

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[równorzędna az sieci wirtualne sieci równorzędne — tworzenie](/cli/azure/network/vnet/peering#create?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Dodaj AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|


### <a name="scenarios"></a>Scenariusze

Równorzędne sieci wirtualne tworzy się między sieciami wirtualnymi utworzonymi za pomocą tych samych lub różnych modeli wdrażania istniejących w tej samej lub w różnych subskrypcjach. Wykonaj krok samouczka dla jednego z następujących scenariuszy:
 
|Model wdrażania platformy Azure  | Subskrypcja  |
|---------|---------|
|Resource Manager — w obu przypadkach |[Ta sama](virtual-network-create-peering.md)|
| |[Różne](create-peering-different-subscriptions.md)|
|Jedna sieć — Resource Manager, druga — model klasyczny     |[Ta sama](create-peering-different-deployment-models.md)|
| |[Różne](create-peering-different-deployment-models-subscriptions.md)|

## <a name="view-or-change-peering-settings"></a>Wyświetl lub zmień ustawienia komunikacji równorzędnej

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta przypisanego niezbędnych [roli lub uprawnienia](#permissions).
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *sieci wirtualnych*. Gdy **sieci wirtualnych** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **sieci wirtualnych** bloku, który jest wyświetlany, kliknij przycisk chcesz utworzyć komunikacji równorzędnej dla sieci wirtualnej.
4. W okienku jest wyświetlana w przypadku wybrania sieci wirtualnej, kliknij przycisk **komunikacji równorzędnych** w **ustawienia** sekcji.
5. Kliknij przycisk komunikacji równorzędnej, który chcesz wyświetlić lub zmienić ustawienia.
6. Zmień odpowiednie ustawienia. Przeczytaj informacje o opcje dla każdego ustawienia w [krok 6](#add-peering) tworzenie komunikacji równorzędnej sekcji tego artykułu. 

    >[!NOTE]
    >Przed utworzeniem komunikacji równorzędnej, upewnij się, należy po zapoznaniu się z [wymagań i ograniczeń](#requirements-and-constraints) i [wymagane uprawnienia](#permissions).
    >

7. Kliknij pozycję **Zapisz**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[AZ sieci wirtualnej komunikacji równorzędnej listy](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#list) do komunikacji równorzędnych listy sieci wirtualnej [az sieci vnet show komunikacji równorzędnej](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#show) do wyświetlenia określonej komunikacji równorzędnej, ustawienia i [az sieci wirtualnej komunikacji równorzędnej aktualizacji](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#update) zmiany ustawienia komunikacji równorzędnej.|
|PowerShell|[Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) można pobrać ustawień komunikacji równorzędnej widoku i [AzureRmVirtualNetworkPeering zestaw](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) Aby zmienić ustawienia.|

## <a name="delete-a-peering"></a>Usuń element równorzędny
Po usunięciu komunikacji równorzędnej, ruch z sieci wirtualnej jest już przepływ peered sieci wirtualnej. Sieci wirtualne są wdrażane za pomocą Menedżera zasobów są połączyć za pomocą, każdej sieci wirtualnej ma równorzędna do innych sieci wirtualnej. Chociaż usuwanie komunikacji równorzędnej z jedną sieć wirtualną wyłączenie komunikacji między sieciami wirtualnymi, nie są usuwane komunikacji równorzędnej z innych sieci wirtualnej. Jest w stanie komunikacji równorzędnej dla komunikacji równorzędnej, który istnieje w sieci wirtualnej **Rozłączono**. Nie można odtworzyć komunikację równorzędną dopóki nie zostaną utworzone ponownie komunikację równorzędną w pierwszej sieci wirtualnej i stanu komunikacji równorzędnej dla obu zmiany sieci wirtualnych *połączony*. 

Jeśli chcesz sieci wirtualne do komunikowania się czasami, ale nie zawsze, zamiast usuwania komunikacji równorzędnej, można ustawić **Zezwalaj na dostęp do sieci wirtualnej** ustawienie **wyłączone** zamiast tego. Aby dowiedzieć się, jak to zrobić, przeczytaj kroku 6 procedury [utworzyć komunikacji równorzędnej](#create-peering) sekcji tego artykułu. Może się okazać wyłączania i włączania łatwiejsze niż usunięcie i ponowne utworzenie komunikacji równorzędnych dostępu do sieci.

1. Zaloguj się do [portal](https://portal.azure.com) przy użyciu konta przypisanego niezbędnych [roli lub uprawnienia](#permissions).
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *sieci wirtualnych*. Gdy **sieci wirtualnych** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **sieci wirtualnych** bloku, który jest wyświetlany, kliknij przycisk chcesz usunąć komunikacji równorzędnej z sieci wirtualnej.
4. W wyświetlonym bloku dla wybranych sieci wirtualnej, kliknij przycisk **komunikacji równorzędnych** w obszarze **ustawienia**.
5. Na liście komunikacji równorzędnych, która jest wyświetlana w bloku komunikacji równorzędnych, kliknij prawym przyciskiem myszy komunikację równorzędną, aby usunąć, kliknij przycisk **usunąć**, następnie **tak** można usunąć komunikacji równorzędnej z pierwszej sieci wirtualnej.
6. Wykonaj poprzednie kroki, aby usunąć komunikacji równorzędnej z innych sieci wirtualnej w komunikacji równorzędnej.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[AZ sieci wirtualnej komunikacji równorzędnej usunięcie](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Usuń AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="requirements-and-constraints"></a>Wymagania i ograniczenia 

- Sieci wirtualne, które możesz elementu równorzędnego musi mieć-nakładającymi się obszarami adresów IP.
- Nie można dodać obszary adresów, lub usuń przestrzeni adresów z sieci wirtualnej, po sieci wirtualnej jest połączyć za pomocą z innej sieci wirtualnej. Aby dodać lub usunąć przestrzenie adresowe, usunąć komunikację równorzędną, dodać lub usunąć przestrzeni adresowych, następnie utworzyć je ponownie komunikację równorzędną. Aby dodać obszary adresów lub usunąć przestrzenie adresowe sieci wirtualnych, przeczytaj [tworzenie, zmienianie i usuwanie sieci wirtualnych](virtual-network-manage-network.md#add-address-spaces) artykułu.
- Można równorzędne dwie sieci wirtualne wdrożone za pośrednictwem Menedżera zasobów lub sieci wirtualnej wdrożone za pomocą Menedżera zasobów z siecią wirtualną wdrożone za pośrednictwem klasycznego modelu wdrażania. Nie można równorzędne dwie sieci wirtualne utworzone za pośrednictwem klasycznego modelu wdrażania. Jeśli nie masz doświadczenia w obsłudze modele wdrażania platformy Azure, przeczytaj [modele wdrażania zrozumieć Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu. Do połączenia dwóch sieci wirtualnych utworzonych za pomocą klasycznego modelu wdrażania można użyć usługi [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
- W przypadku łączenia dwóch sieci wirtualnych utworzonych w usłudze Resource Manager za pomocą komunikacji równorzędnej należy skonfigurować komunikację równorzędną dla każdej objętej nią sieci wirtualnej. 
    - *Zainicjowano:* podczas tworzenia równorzędna do drugiej sieci wirtualnej z pierwszej sieci wirtualnej, stan komunikacji równorzędnej jest *zainicjowano*. 
    - *Połączony:* podczas tworzenia komunikacji równorzędnej z drugiej sieci wirtualnej do pierwszej sieci wirtualnej, jego stan komunikacji równorzędnej jest *połączony*. Jeśli możesz wyświetlić stan komunikacji równorzędnej dla pierwszej sieci wirtualnej, zobacz jego stan zmienił się z *zainicjowano* do *połączony*. Komunikację równorzędną nie zostanie pomyślnie nawiązane momentu komunikacji równorzędnej stan dla obu komunikacji równorzędnych sieci wirtualnych *połączony*.
- Podczas komunikacji równorzędnej sieci wirtualnej utworzonej za pomocą Menedżera zasobów z siecią wirtualną, która została utworzona za pośrednictwem klasycznego modelu wdrażania, można skonfigurować tylko komunikacji równorzędnej dla sieci wirtualnej wdrożone za pomocą Menedżera zasobów. Nie można skonfigurować komunikację równorzędną sieci wirtualnej (wdrożenia klasyczne) lub między dwiema sieciami wirtualnej wdrożone za pośrednictwem klasycznego modelu wdrażania. Podczas tworzenia komunikacji równorzędnej z sieci wirtualnej (Resource Manager) do sieci wirtualnej (klasyczne), stan komunikacji równorzędnej jest *aktualizacji*, wkrótce zmienia się *połączony*.
- Komunikacja równorzędna została ustanowiona między dwiema sieciami wirtualnymi. Komunikacji równorzędnych nie są przechodnie. Jeśli utworzysz komunikacji równorzędnych między:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Nie ma żadnych komunikacji równorzędnej między VirtualNetwork1 i VirtualNetwork3 za pośrednictwem VirtualNetwork2. Jeśli chcesz utworzyć sieć wirtualną komunikacji równorzędnej między VirtualNetwork1 i VirtualNetwork3, należy utworzyć komunikacji równorzędnej między VirtualNetwork1 i VirtualNetwork3.
- Nie można rozpoznać nazwy w połączyć za pomocą sieci wirtualnych za pomocą domyślnego rozwiązania nazwa platformy Azure. Rozpoznawanie nazw w innych sieciach wirtualnych, należy użyć niestandardowego serwera DNS. Aby dowiedzieć się, jak skonfigurować serwer DNS, przeczytaj [rozpoznawanie nazw przy użyciu serwera DNS](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) artykułu.
- Zasoby w obie sieci wirtualne w komunikacji równorzędnej może komunikować się ze sobą przy tym samym przepustowości i opóźnień tak, jakby były w tej samej sieci wirtualnej. Rozmiar każdej maszyny wirtualnej ma jednak własną maksymalną przepustowość sieci. Aby dowiedzieć się więcej o maksymalnej przepustowości dla różnych rozmiarów maszyn wirtualnych, przeczytaj artykuły dotyczące rozmiarów maszyn wirtualnych w systemach [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) i [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Można elementów równorzędnych sieci wirtualnych wdrożone za pomocą Menedżera zasobów, które znajdują się w tych samych lub różnych subskrypcji.
- Można elementów równorzędnych sieci wirtualnych wdrożonych przez różne modele wdrażania znajdujących się w tej samej lub różnych subskrypcji (wersja zapoznawcza). 
- Subskrypcje, które są obie sieci wirtualne w musi być skojarzony z tej samej dzierżawy usługi Azure Active Directory. Jeśli nie masz już dzierżawę AD, możesz szybko [utworzyć](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Można użyć [bramy sieci VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) Aby połączyć dwie sieci wirtualne, które istnieją w ramach różnych subskrypcji skojarzonych z różnych dzierżawców usługi Active Directory.
- Sieć wirtualną można połączyć za pomocą do innej sieci wirtualnej, a także być podłączony do innej sieci wirtualnej z bramą sieci wirtualnej platformy Azure. Jeśli sieci wirtualne są połączone za pośrednictwem komunikacji równorzędnej i bramy, ruchu między sieciami wirtualnymi przechodzi przez konfiguracji komunikacji równorzędnej, a nie bramy.
- Istnieje nominalna opłata za ruch przychodzący i wychodzący w wirtualnych sieciach równorzędnych. Aby uzyskać więcej informacji, odwiedź [stronę cennika](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="permissions"></a>Uprawnienia

Konta, które służy do tworzenia sieci wirtualnej komunikacji równorzędnej musi mieć ról lub uprawnień. Na przykład jeśli zostały równorzędna dwie sieci wirtualne o nazwach myVnetA i myVnetB, Twoje konto musi zostać przypisane następujące minimalne roli lub uprawnienia dla każdej sieci wirtualnej:
    
|Sieć wirtualna|Model wdrażania|Rola|Uprawnienia|
|---|---|---|---|
|myVnetA|Resource Manager|[Współautor sieci](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Wdrożenie klasyczne|[Współautor klasycznej sieci](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Nie dotyczy|
|myVnetB|Resource Manager|[Współautor sieci](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Wdrożenie klasyczne|[Współautor klasycznej sieci](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Dowiedz się więcej o [wbudowane role](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) i przypisywanie określonych uprawnień do [role niestandardowe](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (tylko Resource Manager).

## <a name="next-steps"></a>Następne kroki

Dowiedz się jak, utworzyć [topologię sieciową typu gwiazda](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
