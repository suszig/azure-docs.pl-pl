---
title: Tworzenie, zmienianie lub usuwanie Azure publicznego adresu IP | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak tworzenie, zmienianie lub usuwanie publicznego adresu IP."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: e52dc76608a83d446ccc8503d17445a8d6a61ae4
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Tworzenie, zmienianie lub usuwanie publicznego adresu IP

Więcej informacji na temat publiczny adres IP i tworzenie, zmienianie i usunąć jeden. Publiczny adres IP jest zasób o jego ustawienia można skonfigurować. Przypisywanie publicznego adresu IP do innych zasobów platformy Azure umożliwia:
- Przychodzące do zasobów, takich jak maszyny wirtualne platformy Azure, zestawy skalowania maszyny wirtualnej Azure bramy sieci VPN platformy Azure, bramy aplikacji i usług równoważenia obciążenia Azure internetowy łączności z Internetem. Zasoby platformy Azure nie może otrzymywać komunikacji przychodzącej z Internetu bez przypisanej publicznego adresu IP. Podczas niektórych zasobów platformy Azure są z założenia dostępne za pośrednictwem publicznych adresów IP, inne zasoby musi mieć publicznych adresów IP przypisanych do nich mają być dostępne z Internetu.
- Łączność wychodząca z Internetem przy użyciu wartości prognozowanych adresu IP. Na przykład maszyny wirtualne mogą komunikować się ruch wychodzący do Internetu bez publicznego adresu IP przypisane do niej, ale jej adres jest adresem sieci przetłumaczony przez platformę Azure nieprzewidywalne publicznego adresu. Przypisywanie publicznego adresu IP do zasobu umożliwia wiedzieć, który adres IP jest używana dla połączenia wychodzące. Chociaż przewidywalne, można zmienić adres, w zależności od wybranej metody przypisania. Aby uzyskać więcej informacji, zobacz [tworzenie publicznego adresu IP](#create-a-public-ip-address). Aby dowiedzieć się więcej na temat połączenia wychodzące z zasobów platformy Azure, przeczytaj [zrozumieć połączeń wychodzących](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed wykonaniem kolejnych kroków w dowolnej części tego artykułu, należy wykonać następujące zadania:

- Przegląd [Azure ogranicza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artykułu, aby dowiedzieć się więcej na temat limitów dla publicznych adresów IP.
- Zaloguj się do platformy Azure [portal](https://portal.azure.com), Azure interfejsu wiersza polecenia (CLI) lub Azure PowerShell przy użyciu konta platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, należy zarejestrować się w celu [bezpłatnego konta wersji próbnej](https://azure.microsoft.com/free).
- Jeśli za pomocą poleceń programu PowerShell do wykonywania zadań w tym artykule [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję apletów poleceń programu Azure PowerShell zainstalowane. Aby uzyskać pomoc dotyczącą poleceń programu PowerShell, wraz z przykładami, wpisz `get-help <command> -full`.
- Jeśli za pomocą poleceń Azure interfejsu wiersza polecenia (CLI) do wykonywania zadań w tym artykule [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia, wpisz `az <command> --help`. Zamiast instalowania interfejsu wiersza polecenia i jego wymagania wstępne, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Ma ona wstępnie zainstalowany interfejs wiersza polecenia platformy Azure skonfigurowany do użycia z Twoim kontem. Aby użyć powłoki chmury, kliknij przycisk powłoki chmury **> _** przycisk w górnej części [portal](https://portal.azure.com).

Publiczne adresy IP ma nominalnego opłat. Aby wyświetlić ceny, przeczytaj [cennik adres IP](https://azure.microsoft.com/pricing/details/ip-addresses) strony. 

## <a name="create-a-public-ip-address"></a>Tworzenie publicznego adresu IP

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *publicznego adresu ip*. Gdy **publicznego adresu IP, adresy** pojawia się w wynikach wyszukiwania kliknij ją.
3. Kliknij przycisk **+ Dodaj** w **publicznego adresu IP** wyświetlonym bloku.
4. Wprowadź lub wybierz wartości dla następujących ustawień w **tworzenie publicznego adresu IP** bloku, zostanie wyświetlone, następnie kliknij przycisk **Utwórz**:

    |Ustawienie|Wymagana?|Szczegóły|
    |---|---|---|
    |SKU|Tak|Wszystkie publiczne adresy IP utworzone przed wprowadzeniem jednostki SKU **podstawowe** SKU publicznych adresów IP.  Jednostka SKU nie można zmienić po utworzeniu publicznego adresu IP. Autonomicznej maszyny wirtualnej, maszyn wirtualnych w zestawie dostępności lub zestawy skalowania maszyny wirtualnej można użyć, Basic lub Standard jednostki SKU.  Mieszanie jednostki SKU między maszynami wirtualnymi w obrębie zestawów dostępności lub zestawy skalowania jest niedozwolone. **Podstawowe** SKU: w przypadku tworzenia publicznego adresu IP w region, który obsługuje stref dostępności **strefy dostępności** mają ustawioną wartość *Brak* domyślnie. Można wybrać strefy dostępności, aby zagwarantować dla określonej strefy dla publicznego adresu IP. **Standardowe** SKU: A standardowy SKU publicznego adresu IP może być skojarzona z maszyną wirtualną lub fronton usługi równoważenia obciążenia. Jeśli tworzysz publicznego adresu IP w region, który obsługuje stref dostępności **strefy dostępności** mają ustawioną wartość *Strefowo nadmiarowy* domyślnie. Aby uzyskać więcej informacji na temat stref dostępności, zobacz **strefy dostępności** ustawienie. Standardowy SKU jest wymagany, jeśli można skojarzyć adres usługi równoważenia obciążenia standardowego. Aby dowiedzieć się więcej na temat równoważenia obciążenia standardowego, zobacz [modułu równoważenia obciążenia Azure standardowy SKU](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Standardowy SKU jest w wersji zapoznawczej. Przed utworzeniem standardowy SKU publicznego adresu IP, należy najpierw wykonać kroki w [zarejestrować standardowe wersji zapoznawczej SKU](#register-for-the-standard-sku-preview) i tworzenie publicznego adresu IP w obsługiwanych lokalizacji (regionu). Aby uzyskać listę obsługiwanych lokalizacji, zobacz [dostępność w danym regionie](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) i monitorowanie [aktualizuje sieci wirtualnej Azure](https://azure.microsoft.com/updates/?product=virtual-network) strony region dodatkowego pomocy technicznej. Po przypisaniu standardowego publicznego adresu IP jednostki SKU do interfejsu sieciowego maszyny wirtualnej musisz jawnie zezwolić na ruch do miejsca przeznaczenia przy użyciu [sieciowej grupy zabezpieczeń](security-overview.md#network-security-groups). Próba komunikacji z zasobem będzie kończyć się niepowodzeniem do momentu utworzenia i skojarzenia sieciowej grupy zabezpieczeń, a następnie jawnego zezwolenia na żądany ruch.|
    |Nazwa|Tak|Nazwa musi być unikatowa w ramach grupy zasobów, którą wybierzesz.|
    |Wersji protokołu IP|Tak| Wybierz IPv4 lub IPv6. Podczas publicznych adresów IPv4 mogą być przypisane do wielu zasobów platformy Azure, publiczny adres IP protokołu IPv6 można przypisać tylko do modułu równoważenia obciążenia internetowy. Moduł równoważenia obciążenia można zrównoważeniu ruch IPv6 do maszyn wirtualnych platformy Azure. Dowiedz się więcej o [IPv6 ruchu do maszyn wirtualnych równoważenia obciążenia](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). W przypadku wybrania **standardowy SKU**, nie trzeba wybrać opcję *IPv6*. Można tworzyć tylko adresu IPv4 przy użyciu **standardowy SKU**.|
    |Przypisywanie adresów IP|Tak|**Dynamiczne:** dynamicznych adresów są przypisane tylko po publiczny adres IP jest skojarzony z karty sieciowej podłączonej do maszyny wirtualnej i maszynie wirtualnej jest uruchomiona po raz pierwszy. Jeśli interfejs sieciowy jest dołączony do maszyny wirtualnej zostanie zatrzymana (cofnięciu przydziału), można zmienić adresów dynamicznych. Adres jest taka sama, jeśli maszyna wirtualna jest ponowny rozruch lub zatrzymana (ale nie alokację). **Statyczne:** przypisywania adresów statycznych po utworzeniu publicznego adresu IP. Statyczne adresy nie należy zmieniać, nawet wtedy, gdy maszyna wirtualna przechodzi w stan zatrzymania (cofnięciu przydziału). Adres zwolnieniu tylko po usunięciu interfejsu sieciowego. Metoda przydziału można zmienić po utworzeniu interfejsu sieciowego. W przypadku wybrania *IPv6* dla **IP w wersji**, Metoda przydziału jest *dynamiczne*. W przypadku wybrania *standardowe* dla **SKU**, Metoda przydziału jest *statycznych*.|
    |Limit czasu bezczynności (w minutach)|Nie|Ile minut, aby utrzymać otwarte połączenie TCP lub HTTP bez polegania na klientach, aby wysyłać komunikaty utrzymywania aktywności. W przypadku wybrania protokołu IPv6 dla **IP w wersji**, nie można zmienić tej wartości. |
    |Etykieta nazwy DNS|Nie|Muszą być unikatowe w lokalizacji platformy Azure, Utwórz nazwę w (za pośrednictwem wszystkie subskrypcje i wszystkich klientów). Azure automatycznie rejestruje nazwę i adres IP w jego systemie DNS, możesz nawiązać połączenie zasób o takiej nazwie. Azure dołącza podsieci domyślnej, takich jak *location.cloudapp.azure.com* (Jeśli lokalizacja jest lokalizacją wybrania) na nazwę podasz, do utworzenia w pełni kwalifikowanej nazwy DNS. Jeśli wybierzesz opcję utworzenia obie wersje adres, tą samą nazwą DNS jest przypisany do adresów IPv4 i IPv6. Domyślne Azure DNS zawiera zarówno IPv4 A i IPv6 AAAA rejestruje nazwę i odpowiada zarówno rekordy po wyszukiwana jest nazwa DNS. Klient będzie preferował adresów (IPv4 lub IPv6) do komunikacji z. Zamiast lub oprócz przy użyciu etykieta nazwy DNS z sufiksem domyślna służy usługa Azure DNS do konfigurowania nazwy DNS z sufiksem niestandardowego, który jest rozpoznawany jako publiczny adres IP. Aby uzyskać więcej informacji, zobacz [użycia usługi Azure DNS za pomocą usługi Azure publicznego adresu IP](../dns/dns-custom-domain.md?toc=%2fazure%2fvirtual-network%2ftoc.json#public-ip-address).|
    |Utwórz adres IPv6 (lub IPv4)|Nie| Określa, czy jest wyświetlany IPv6 lub protokół IPv4 jest zależna od wybierz dla **IP w wersji**. Na przykład w przypadku wybrania **IPv4** dla **IP w wersji**, **IPv6** jest wyświetlany w tym miejscu. W przypadku wybrania *standardowe* dla **SKU**, nie masz opcję, aby utworzyć adres IPv6.
    |Nazwa (tylko widoczne, gdy zaznaczono **utworzyć adres IPv6 (lub IPv4)** wyboru)|Tak, w przypadku wybrania **utworzyć IPv6** (lub IPv4) pole wyboru.|Nazwa musi być inna niż nazwa wprowadzona dla pierwszego **nazwa** na tej liście. Jeśli użytkownik chce utworzyć adres IPv6 i IPv4, portal tworzy dwa oddzielne zasoby publicznych adresów IP adres, z każdej wersji adresu IP przypisane do niej.|
    |Przypisywanie adresów IP (tylko widoczne, gdy zaznaczono **utworzyć adres IPv6 (lub IPv4)** wyboru)|Tak, w przypadku wybrania **utworzyć IPv6** (lub IPv4) pole wyboru.|Jeśli pole wyboru **utworzyć adres IPv4**, można wybrać metodę przypisania. Jeśli pole wyboru **utworzyć adres IPv6**, nie można wybrać metodę przypisania jako musi być **dynamiczne**.|
    |Subskrypcja|Tak|Musi istnieć w tym samym [subskrypcji](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) jako zasób, aby skojarzyć publiczny adres IP.|
    |Grupa zasobów|Tak|W tym samym lub różnych, może istnieć [grupy zasobów](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) jako zasób, aby skojarzyć publiczny adres IP.|
    |Lokalizacja|Tak|Musi istnieć w tym samym [lokalizacji](https://azure.microsoft.com/regions), również określone jako region, jako zasób, którą chcesz skojarzyć z publicznym adresem IP adres.|
    |Dostępność strefy| Nie | To ustawienie jest wyświetlany tylko w przypadku wybrania obsługiwanych lokalizacji. Aby uzyskać listę obsługiwanych lokalizacji, zobacz [omówienie stref dostępności](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dostępność strefy są obecnie w wersji zapoznawczej. Przed wybraniem strefy lub strefowo nadmiarowy opcji, należy najpierw wykonaj kroki [zarejestrować w wersji zapoznawczej stref dostępności](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#get-started-with-the-availability-zones-preview). W przypadku wybrania **podstawowe** jednostka SKU, *Brak* jest automatycznie wybrana. Jeśli wolisz zagwarantować dla określonej strefy, można wybrać określonej strefy. Wybór albo nie jest strefowo nadmiarowy. W przypadku wybrania **standardowe** SKU: Strefowo nadmiarowy jest automatycznie wybrana i sprawia, że ścieżki danych odporność na awarie strefy. Jeśli wolisz zagwarantować dla określonej strefy, który nie był odporny na awarie strefy, można wybrać określonej strefy.
  

**Polecenia**

Chociaż portalu udostępnia opcję, aby utworzyć dwa zasoby publicznych adresów IP address (jeden adres IPv4 i IPv6 jeden), następujących poleceń programu PowerShell i interfejsu wiersza polecenia Utwórz jeden zasób z adresem dla jednej wersji adresu IP lub innych. Jeśli chcesz, aby dwa zasoby publicznych adresów IP address, jeden dla każdej wersji adresu IP musi Uruchom polecenie dwa razy, określając inne nazwy i wersje zasoby publicznych adresów IP. 

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Tworzenie sieci az publicznego ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nowe AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Wyświetlanie, zmieniać ustawienia lub usuwanie publicznego adresu IP

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *publicznego adresu ip*. Gdy **publicznego adresu IP, adresy** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **publicznego adresu IP, adresy** bloku, który jest wyświetlany, kliknij nazwę publicznego adresu IP, które chcesz wyświetlić, Zmień ustawienia i usuwania.
4. W wyświetlonym bloku dla publicznych adresów IP wykonaj jedną z poniższych opcji, w zależności od tego, czy chcesz wyświetlić, usunąć lub zmienić publicznego adresu IP.
    - **Widok**: **omówienie** bloku pokazuje ustawień klucza publicznego adresu IP, takich jak interfejs sieciowy jest ona skojarzona (Jeśli adres jest skojarzony z karty sieciowej). Portalu nie wyświetla wersję adresu (IPv4 lub IPv6). Aby wyświetlić informacje o wersji, polecenie programu PowerShell lub interfejsu wiersza polecenia do wyświetlania publicznego adresu IP. Wersja adresów IP w przypadku protokołu IPv6, przypisany adres nie jest wyświetlana w portalu, programu PowerShell lub interfejsu wiersza polecenia. 
    - **Usuń**: można usunąć publicznego adresu IP, kliknij przycisk **usunąć** w **omówienie** części bloku. Jeśli adres jest obecnie skojarzony z konfiguracją protokołu IP, nie można usunąć. Jeśli adres jest obecnie skojarzony z konfiguracją, kliknij przycisk **Utwórz skojarzenie** można usunąć skojarzenia adresu z konfiguracji protokołu IP.
    - **Zmień**: kliknij **konfiguracji**. Zmień ustawienia, korzystając z informacji w kroku 4 [tworzenie publicznego adresu IP](#create-a-public-ip-address) sekcji tego artykułu. Aby zmienić przypisanie adresu IPv4 ze statycznego na dynamiczny, musi najpierw skojarzenie publiczny adres IPv4, z którą skojarzona jest do konfiguracji adresu IP. Można zmienić metodę przypisania do dynamicznego i kliknij przycisk **skojarzyć** można skojarzyć adres IP adres do tej samej konfiguracji IP, inną konfigurację lub pozostawić Usunięto skojarzenie. Aby usunąć skojarzenie publicznego adresu IP, w **omówienie** kliknij **Utwórz skojarzenie**.

>[!WARNING]
>Zmiana metody przypisywania ze statycznego na dynamiczny, utracisz adres IP, który został przypisany do publicznego adresu IP. Gdy Azure publicznych serwerów DNS Obsługa mapowanie między statyczne lub dynamiczne adresy i wszelkie etykieta nazwy DNS (Jeśli zdefiniowany jest jeden), dynamicznego adresu IP, można zmienić po uruchomieniu maszyny wirtualnej po jest w stanie zatrzymania (cofnięciu przydziału). Aby uniemożliwić zmianę adresu, Przypisz statyczny adres IP.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[AZ sieci publicznego adresu ip listy](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) publiczne adresy IP, [az sieci publicznego adresu ip Pokazywanie](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) pokazanie ustawień; [az sieci ip publicznego aktualizacji](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) zaktualizować; [usunąć publicznej sieci az ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) do usunięcia|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) można pobrać obiektu publiczny adres IP i wyświetlić jej ustawienia [AzureRmPublicIpAddress zestaw](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) można zaktualizować ustawień; [AzureRmPublicIpAddress Usuń](/powershell/module/azurerm.network/remove-azurermpublicipaddress) do usunięcia|

## <a name="register-for-the-standard-sku-preview"></a>Zarejestruj się w celu standardowego Podgląd jednostki SKU

> [!NOTE]
> Funkcje w wersji zapoznawczej nie może mieć taki sam poziom dostępności i niezawodności, zgodnie z funkcji, które są zwykle dostępności wersji. Funkcje w wersji zapoznawczej nie są obsługiwane, mogą mieć ograniczone możliwości i mogą nie być dostępne we wszystkich lokalizacjach platformy Azure. 

Przed utworzeniem standardowy SKU publiczny adres IP, najpierw należy zarejestrować skorzystania z wersji zapoznawczej. Wykonaj poniższe kroki, aby zarejestrować się w wersji zapoznawczej:

1. Instalowanie i konfigurowanie usługi Azure [PowerShell](/powershell/azure/install-azurerm-ps).
2. Uruchom `Get-Module -ListAvailable AzureRM` polecenie, aby zobaczyć, jakie wersja modułu AzureRM jest zainstalowana. Musisz zainstalować wersję 4.4.0 lub nowszym. Jeśli nie chcesz, można zainstalować najnowszą wersję ze [galerii programu PowerShell](https://www.powershellgallery.com/packages/AzureRM).
3. Logowanie do platformy Azure z `login-azurermaccount` polecenia.
4. Wprowadź następujące polecenie, aby zarejestrować się w wersji zapoznawczej:
   
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

5. Upewnij się, że są zarejestrowane dla wersji zapoznawczej, wprowadzając następujące polecenie:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

## <a name="next-steps"></a>Następne kroki
Podczas tworzenia następujących zasobów platformy Azure, przypisz publicznych adresów IP:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) lub [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) maszyny wirtualne
- [Moduł równoważenia obciążenia Azure połączonych z Internetem](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Brama aplikacji Azure](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Połączenie lokacja lokacja za pomocą bramy sieci VPN platformy Azure](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Zestaw skali maszyny wirtualnej platformy Azure](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
