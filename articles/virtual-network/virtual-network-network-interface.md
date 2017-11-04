---
title: Tworzenie, zmienianie lub usuwanie interfejsu sieci platformy Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się interfejs sieciowy oraz sposobu tworzenia, zmienić ustawienia i usunąć jeden."
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
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 7dafb491cec908ffbb3683991919654f3d3eb452
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-change-or-delete-a-network-interface"></a>Tworzenie, zmienianie lub usuwanie interfejsu sieciowego

Dowiedz się, jak utworzyć, Zmień ustawienia i usunąć interfejsu sieciowego. Interfejs sieciowy umożliwia maszynie wirtualnej platformy Azure do komunikowania się z Internetem, Azure i lokalnymi zasobami. Podczas tworzenia maszyny wirtualnej przy użyciu portalu Azure, portal tworzy jeden interfejs sieciowy z ustawieniami domyślnymi dla Ciebie. Można zamiast tego do tworzenia interfejsów sieciowych z użyciem ustawień niestandardowych i Dodaj jeden lub więcej interfejsów sieciowych do maszyny wirtualnej, po jego utworzeniu. Można również zmienić domyślne ustawienia interfejsu sieciowego dla istniejącego interfejsu sieciowego. W tym artykule wyjaśniono, jak utworzyć niestandardowe ustawienia interfejsu sieciowego, zmień istniejące ustawienia, takie jak przypisywanie (sieciowej grupy zabezpieczeń) filtru sieci, przypisanie podsieci, ustawienia serwera DNS i przesyłanie dalej IP i usunąć interfejsu sieciowego.

Jeśli potrzebujesz dodać, zmienić lub usunąć adresy IP dla karty sieciowej, przeczytaj [adresów IP zarządzanie](virtual-network-network-interface-addresses.md) artykułu. Jeśli potrzebujesz interfejsów sieciowych, dodawanie lub usuwanie interfejsów sieciowych z maszyn wirtualnych, przeczytaj [Dodawanie lub usuwanie interfejsów sieciowych](virtual-network-network-interface-vm.md) artykułu.


## <a name="before-you-begin"></a>Przed rozpoczęciem

Przed wykonaniem kolejnych kroków w dowolnej części tego artykułu, należy wykonać następujące zadania:

- Przegląd [Azure ogranicza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artykułu, aby dowiedzieć się więcej na temat limitów dla interfejsów sieciowych.
- Zaloguj się do platformy Azure [portal](https://portal.azure.com), Azure interfejsu wiersza polecenia (CLI) lub Azure PowerShell przy użyciu konta platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, należy zarejestrować się w celu [bezpłatnego konta wersji próbnej](https://azure.microsoft.com/free).
- Jeśli za pomocą poleceń programu PowerShell do wykonywania zadań w tym artykule [Instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję apletów poleceń programu Azure PowerShell zainstalowane. Aby uzyskać pomoc dotyczącą poleceń programu PowerShell, wraz z przykładami, wpisz `get-help <command> -full`.
- Jeśli za pomocą poleceń Azure interfejsu wiersza polecenia (CLI) do wykonywania zadań w tym artykule [Instalowanie i Konfigurowanie interfejsu wiersza polecenia Azure](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Upewnij się, że masz najnowszą wersję interfejsu wiersza polecenia Azure zainstalowane. Aby uzyskać pomoc dotyczącą poleceń interfejsu wiersza polecenia, wpisz `az <command> --help`. Zamiast instalowania interfejsu wiersza polecenia i jego wymagania wstępne, można użyć powłoki chmury Azure. Usługa Azure Cloud Shell jest bezpłatną powłoką Bash, którą można uruchamiać bezpośrednio w witrynie Azure Portal. Ma ona wstępnie zainstalowany interfejs wiersza polecenia platformy Azure skonfigurowany do użycia z Twoim kontem. Aby użyć powłoki chmury, kliknij przycisk powłoki chmury **> _** przycisk w górnej części [portal](https://portal.azure.com).

## <a name="create-a-network-interface"></a>Tworzenie interfejsu sieciowego

Podczas tworzenia maszyny wirtualnej przy użyciu portalu Azure, portal tworzy interfejs sieciowy z ustawieniami domyślnymi dla Ciebie. Jeśli wolisz określić wszystkie ustawienia interfejsu sieciowego, można utworzyć niestandardowe ustawienia interfejsu sieciowego i dołączyć interfejsu sieciowego z maszyną wirtualną, podczas tworzenia maszyny wirtualnej (przy użyciu programu PowerShell lub interfejsu wiersza polecenia Azure). Można również utworzyć interfejsu sieciowego i dodać go do istniejącej maszyny wirtualnej (przy użyciu programu PowerShell lub interfejsu wiersza polecenia Azure). Aby dowiedzieć się, jak utworzyć maszynę wirtualną z istniejącego interfejsu sieciowego lub Dodaj do lub usuwanie interfejsów sieciowych z istniejących maszyn wirtualnych, przeczytaj [Dodawanie lub usuwanie interfejsów sieciowych](virtual-network-network-interface-vm.md) artykułu. Przed utworzeniem karty sieciowej, musisz mieć istniejące [sieci wirtualnej](virtual-networks-create-vnet-arm-pportal.md) w tej samej subskrypcji i lokalizacji tworzenia interfejsu sieciowego w.

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *interfejsy sieciowe*. Gdy **interfejsy sieciowe** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **interfejsy sieciowe** bloku, który jest wyświetlany, kliknij przycisk **+ Dodaj**.
4. W **interfejsu sieciowego Utwórz** bloku, który pojawia się, wprowadź, lub wybierz wartości poniższych ustawień, a następnie kliknij przycisk **Utwórz**:

    |Ustawienie|Wymagana?|Szczegóły|
    |---|---|---|
    |Nazwa|Tak|Nazwa musi być unikatowa w ramach grupy zasobów, którą wybierzesz. Wraz z upływem czasu najprawdopodobniej będziesz mieć kilka interfejsów sieciowych w ramach subskrypcji platformy Azure. Odczyt [konwencje nazewnictwa](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) artykułu dla wnioski podczas tworzenia konwencji nazewnictwa Zarządzanie kilka interfejsów łatwiejsze sieciowych. Nie można zmienić nazwę, po utworzeniu interfejsu sieciowego.|
    |Sieć wirtualna|Tak|Wybierz sieć wirtualną dla interfejsu sieciowego. Interfejs sieciowy można przypisać tylko do sieci wirtualnej, który istnieje w tej samej subskrypcji i lokalizacji co interfejsu sieciowego. Po utworzeniu karty sieciowej nie można zmienić sieci wirtualnej, który jest przypisany do. Dodaj interfejs sieciowy do maszyny wirtualnej muszą także istnieć w tej samej lokalizacji, a subskrypcja jako interfejs sieciowy.|
    |Podsieć|Tak|Wybierz podsieć w wybranej sieci wirtualnej. Można zmienić podsieci, w której interfejsu sieciowego jest przypisany do po jego utworzeniu.|
    |Przypisanie prywatnego adresu IP|Tak| W tym ustawieniu, czy wybierana metoda przydziału dla adresu IPv4. Wybierz jedną z następujących metod przypisania: **dynamicznej:** po wybraniu tej opcji Azure automatycznie przypisuje adres z przestrzeni adresowej podsieci wybrane. Azure może przydzielić inny adres karty sieciowej, po uruchomieniu maszyny wirtualnej, w którym się po przejściu w stan zatrzymania (cofnięciu przydziału). Ten adres jest taka sama, jeśli maszyna wirtualna zostanie ponownie uruchomiony bez konieczności został zatrzymany (cofnięciu przydziału). **Statyczne:** po wybraniu tej opcji, należy ręcznie przypisać dostępne z adresu IP w przestrzeni adresowej podsieci wybrane. Statyczne adresy nie należy zmieniać dopóki je zmienić lub interfejsu sieciowego są usuwane. Metoda przydziału można zmienić po utworzeniu interfejsu sieciowego. Serwer Azure DHCP przypisuje ten adres interfejsu sieciowego w systemie operacyjnym maszyny wirtualnej.|
    |Grupy zabezpieczeń sieci|Nie| Pozostaw ustawioną **Brak**, wybierz istniejący [sieciowej grupy zabezpieczeń](virtual-networks-nsg.md), lub [Utwórz grupę zabezpieczeń sieci](virtual-networks-create-nsg-arm-pportal.md). Grupy zabezpieczeń sieci włączyć do filtrowania ruchu sieciowego do i z karty sieciowej. Zero lub jeden sieciowej grupy zabezpieczeń można stosować do interfejsu sieciowego. Zero lub jedną grupę zabezpieczeń sieci można również będą stosowane do podsieci, w której przydzielono interfejsu sieciowego. Po zastosowaniu do karty sieciowej i podsieci, w której interfejsu sieciowego jest przypisany do grupy zabezpieczeń sieci wystąpić czasami nieoczekiwane wyniki. Aby rozwiązać grup zabezpieczeń sieci dotyczą interfejsy sieciowe i podsieci, przeczytaj [Rozwiązywanie problemów z grup zabezpieczeń sieci](virtual-network-nsg-troubleshoot-portal.md#nsg) artykułu.|
    |Subskrypcja|Tak|Wybierz jedną z platformy Azure [subskrypcje](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Maszyny wirtualnej, dołączenie do interfejsu sieciowego i sieci wirtualnej, gdy nawiązujesz połączenie, musi istnieć w tej samej subskrypcji.|
    |Prywatny adres IP (IPv6)|Nie| Jeśli zaznaczysz to pole wyboru, adres IPv6 jest przypisany do interfejsu sieciowego, oprócz adres IPv4 przypisany do interfejsu sieciowego. Zobacz [IPv6](#IPv6) sekcji tego artykułu, aby uzyskać ważne informacje na temat używania protokołu IPv6 z interfejsami sieciowymi. Nie można wybrać metodę przypisania dla adresu IPv6. Jeśli chcesz przypisać adres IPv6 jest przypisany za pomocą metody dynamicznej.
    |Nazwa IPv6 (tylko jest wyświetlane, gdy **prywatny adres IP (IPv6)** jest zaznaczone pole wyboru) |Tak, jeśli **prywatny adres IP (IPv6)** jest zaznaczone pole wyboru.| Ta nazwa jest przypisany do dodatkowej konfiguracji IP interfejsu sieciowego. Dowiedz się więcej o konfiguracji adresów IP w [wyświetlić ustawienia interfejsu sieciowego](#view-network-interface-settings) sekcji tego artykułu.|
    |Grupa zasobów|Tak|Wybierz istniejący [grupy zasobów](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) lub utwórz taki pakiet. Interfejs sieciowy może istnieć w grupie zasobów tego samego lub innego niż maszynę wirtualną, którą należy dołączyć, lub sieci wirtualnej można je połączyć.|
    |Lokalizacja|Tak|Maszyny wirtualnej, możesz dołączyć do interfejsu sieciowego i połącz go z sieci wirtualnej, musi istnieć w tym samym [lokalizacji](https://azure.microsoft.com/regions), nazywany również regionu.|

Portalu nie zapewnia możliwość przypisania publicznego adresu IP do interfejsu sieciowego, podczas tworzenia, mimo że portalu tworzenie publicznego adresu IP i przypisz je do karty sieciowej, podczas tworzenia maszyny wirtualnej za pomocą portalu. Aby dowiedzieć się, jak dodać publicznego adresu IP do interfejsu sieciowego po jej utworzeniu, przeczytaj [adresów IP zarządzanie](virtual-network-network-interface-addresses.md) artykułu. Jeśli chcesz utworzyć interfejsu sieciowego z publicznym adresem IP, musi użyć interfejsu wiersza polecenia lub programu PowerShell do tworzenia interfejsu sieciowego.

>[!Note]
> Azure przypisuje adres MAC do interfejsu sieciowego tylko wtedy, gdy interfejs sieciowy jest dołączony do maszyny wirtualnej i uruchomieniu maszyny wirtualnej po raz pierwszy. Nie można określić adres MAC, który przypisuje Azure do interfejsu sieciowego. Adres MAC jest przypisana do interfejsu sieciowego, dopóki interfejs sieciowy została usunięta lub prywatnego adresu IP przypisanego do podstawowej konfiguracji IP podstawowy interfejs sieciowy zostanie zmieniona. Aby dowiedzieć się więcej na temat adresów IP i konfiguracje adresów IP, przeczytaj [adresów IP zarządzanie](virtual-network-network-interface-addresses.md) artykułu.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Utwórz az kart interfejsu sieciowego](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nowe AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-network-interface-settings"></a>Wyświetl ustawienia interfejsu sieciowego

Możesz wyświetlić i zmienić większość ustawień interfejsu sieciowego po jego utworzeniu.

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *interfejsy sieciowe*. Gdy **interfejsy sieciowe** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **interfejsy sieciowe** bloku, który jest wyświetlany, kliknij przycisk interfejsu sieciowego, aby wyświetlić lub zmienić ustawienia.
4. W wyświetlonym bloku dla interfejsu sieciowego, który wybrano przedstawiono następujące ustawienia:
    - **Omówienie:** informacje na temat interfejsu sieciowego, takie jak adresy IP przypisane go, wirtualne sieci/podsieci interfejsu sieciowego jest przypisany do oraz maszyny wirtualnej karcie sieciowej jest dołączony do (jeśli jest on dołączony do jednego). Na poniższej ilustracji przedstawiono omówienie ustawień karty sieciowej o nazwie **mywebserver256**: ![omówienie interfejsu sieciowego](./media/virtual-network-network-interface/nic-overview.png) interfejsu sieciowego można przenieść do innej grupy zasobów lub subskrypcji, klikając (**zmienić**) obok pozycji **grupy zasobów** lub **Nazwa subskrypcji**. Jeśli przenosisz interfejsu sieciowego, należy przenieść wszystkie zasoby związane z interfejsu sieciowego z nim. Jeśli interfejs sieciowy jest dołączony do maszyny wirtualnej, na przykład, musisz również przenieść maszynę wirtualną i innych zasobów związanych z maszyny wirtualnej. Aby przenieść interfejsu sieciowego, przeczytaj [przenieść zasobu do nowej grupy zasobów lub subskrypcji](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal) artykułu. Artykuł zawiera listę wymagań wstępnych i sposobu przenoszenia zasobów za pomocą portalu Azure, programu PowerShell i interfejsu wiersza polecenia Azure.
    - **Konfiguracje adresów IP:** prywatnych i publicznych adresów IPv4 i IPv6 przypisany do konfiguracji adresów IP są wyświetlane tutaj. Jeśli adres IPv6 jest przypisany do konfiguracji adresu IP, adres nie jest wyświetlana. Dowiedz się więcej o konfiguracji adresów IP oraz jak dodawać i usuwać IP adresów w [adresy IP, skonfiguruj dla interfejsu sieci platformy Azure](virtual-network-network-interface-addresses.md) artykułu. Przesyłanie dalej IP i przypisanie podsieci są również skonfigurowane w tej sekcji. Aby dowiedzieć się więcej o tych ustawieniach, przeczytaj [włączać lub wyłączać przesyłanie dalej IP](#enable-or-disable-ip-forwarding) i [zmienić przypisanie podsieci](#change-subnet-assignment) sekcje w tym artykule.
    - **Serwery DNS:** można określić, który serwer DNS interfejsu sieciowego jest przypisany przez serwery Azure DHCP. Interfejs sieciowy można dziedziczy ustawienia sieci wirtualnej, który interfejsu sieciowego jest przypisany do lub mieć ustawienia niestandardowe, zastępuje ustawienie dla sieci wirtualnej, który jest przypisany do. Aby zmodyfikować, co jest wyświetlane, wykonaj kroki [serwerów DNS zmiany](#change-dns-servers) sekcji tego artykułu.
    - **Grupy zabezpieczeń sieci (NSG):** Wyświetla co grupa NSG jest skojarzona z interfejsu sieciowego (jeśli istnieje). Grupy NSG zawiera reguły ruchu przychodzącego i wychodzącego do filtrowania ruchu sieciowego dla interfejsu sieciowego. Jeśli grupa NSG jest skojarzona z interfejsu sieciowego, jest wyświetlana nazwa skojarzone NSG. Aby zmodyfikować, co jest wyświetlane, wykonaj kroki [Zarządzanie skojarzenia grupy zabezpieczeń sieci](virtual-network-manage-nsg-arm-portal.md#manage-associations) artykułu.
    - **Właściwości:** Wyświetla klucz Ustawienia dotyczące interfejsu sieciowego, łącznie z jej adres MAC (pusty w przypadku interfejsu sieciowego nie jest dołączony do maszyny wirtualnej), a subskrypcja istnieje on w.
    - **Reguły efektywnym elementem systemu zabezpieczeń:** reguły zabezpieczeń są wyświetlane, jeśli interfejs sieciowy jest dołączony do uruchomionej maszyny wirtualnej, a grupa NSG jest skojarzona z interfejsu sieciowego i/lub jest przypisany do podsieci. Aby dowiedzieć się więcej o to, co jest wyświetlane, przeczytaj [Rozwiązywanie problemów z grup zabezpieczeń sieci](virtual-network-nsg-troubleshoot-portal.md#nsg) artykułu. Aby dowiedzieć się więcej na temat grup NSG, przeczytaj [sieciowej grupy zabezpieczeń](virtual-networks-nsg.md) artykułu.
    - **Skuteczne tras:** wymienione są trasy, jeśli interfejs sieciowy jest dołączony do uruchomionej maszyny wirtualnej. Trasy są kombinacją trasy domyślne Azure, wszelkie trasy zdefiniowane przez użytkownika (przez) i wszelkie trasy protokołu BGP, które mogą wystąpić dla podsieci, w której interfejsu sieciowego jest przypisany do. Aby dowiedzieć się więcej o to, co jest wyświetlane, przeczytaj [Rozwiązywanie problemów z tras](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface) artykułu. Aby dowiedzieć się więcej o domyślnym Azure i Udr, przeczytaj [trasy zdefiniowane przez użytkownika](virtual-networks-udr-overview.md) artykułu.
    - **Typowe ustawienia usługi Azure Resource Manager:** Aby dowiedzieć się więcej na temat typowych ustawień usługi Azure Resource Manager, przeczytaj [dziennik aktywności](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [(IAM) kontroli dostępu](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [tagi](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [blokuje](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), i [skryptu automatyzacji](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group) artykułów.

**Polecenia**

Jeśli adres IPv6 jest przypisany do interfejsu sieciowego, dane wyjściowe programu PowerShell zwraca fakt, że adres, ale nie zwraca przypisany adres. Podobnie, interfejsu wiersza polecenia zwraca fakt, że ten adres jest przypisane, ale zwraca *null* w danych wyjściowych dla adresu.

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Lista kart sieci az](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list) Aby wyświetlić interfejsów sieciowych w subskrypcji; [Pokaż kart sieciowych az](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show) Aby wyświetlić ustawienia interfejsu sieciowego|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) Aby wyświetlić interfejsów sieciowych w subskrypcji lub Wyświetl ustawienia interfejsu sieciowego|

## <a name="change-dns-servers"></a>Zmień serwerów DNS

Serwer DNS jest przypisany przez serwer Azure DHCP do interfejsu sieciowego w systemie operacyjnym maszyny wirtualnej. Serwer DNS przypisany jest niezależnie od ustawienia serwera DNS dla karty sieciowej. Aby dowiedzieć się więcej na temat ustawień rozpoznawania nazwy dla interfejsu sieciowego, zobacz [rozpoznawanie nazw dla maszyn wirtualnych](virtual-networks-name-resolution-for-vms-and-role-instances.md). Interfejs sieciowy może dziedziczyć ustawień sieci wirtualnej lub użyj własne ustawienia unikatowe, które zastępują ustawienia dla sieci wirtualnej.

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *interfejsy sieciowe*. Gdy **interfejsy sieciowe** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **interfejsy sieciowe** bloku, który jest wyświetlany, kliknij przycisk interfejsu sieciowego, aby wyświetlić lub zmienić ustawienia.
4. W bloku dla wybranego interfejsu sieciowego, kliknij **serwerów DNS** w obszarze **ustawienia**.
5. Kliknij opcję:
    - **Dziedzicz sieci wirtualnej (ustawienie domyślne)**: Wybierz tę opcję, aby odziedziczyć ustawienia serwera DNS, które są zdefiniowane dla interfejsu sieciowego jest przypisany do sieci wirtualnej. Na poziomie sieci wirtualnej jest zdefiniowany niestandardowy serwer DNS lub serwer DNS platformy Azure. Serwer DNS platformy Azure można rozpoznać nazwy hostów dla zasobów przypisanych do tej samej sieci wirtualnej. Nazwa FQDN musi być używany do rozpoznania zasobów przydzielonych do różnych sieciach wirtualnych.
    - **Niestandardowe**: można skonfigurować własny serwer DNS do rozpoznawania nazw między wieloma sieciami wirtualnymi. Wprowadź adres IP serwera, który ma być używany jako serwer DNS. Adres serwera DNS, które określisz jest przypisane tylko do tego interfejsu sieciowego i zastępuje wszelkie ustawienia DNS dla sieci wirtualnej przydzielonej do interfejsu sieciowego.
6. Kliknij pozycję **Zapisz**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Aktualizacja kart sieciowych az](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Zestaw AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="enable-or-disable-ip-forwarding"></a>Włączać lub wyłączać przesyłanie dalej IP

Przesyłanie dalej IP umożliwia interfejs sieciowy jest dołączony do maszyny wirtualnej:
- Odbieranie ruchu sieciowego, które nie są przeznaczone do jednego z adresów IP przypisany do żadnej konfiguracji adresów IP przypisanych do interfejsu sieciowego.
- Wyślij ruch sieciowy za pomocą adresu IP źródła innego niż ten, który został przypisany do jednej konfiguracji IP interfejsu sieciowego.

Ustawienie musi być włączony dla każdego interfejsu sieciowego, który jest dołączony do maszyny wirtualnej, która odbiera ruch, który musi maszyny wirtualnej do przesyłania dalej. Maszyny wirtualnej może przekazywać ruch, czy ma ona wiele interfejsów sieciowych lub do niego dołączony jednym interfejsem sieciowym. Przesyłanie dalej IP jest ustawienie platformy Azure, maszyny wirtualnej muszą także uruchamiania aplikacji możliwość przesyłania ruchu, takie jak zapora, Optymalizacja sieci WAN i aplikacje równoważenia obciążenia. Po uruchomieniu aplikacji sieciowych maszyny wirtualnej maszyny wirtualnej jest często określany jako urządzenie wirtualne sieci. Można wyświetlić listę gotowe do wdrożenia sieci wirtualnych urządzeń w [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Przesyłanie dalej IP jest zwykle używany z tras zdefiniowanych przez użytkownika. Aby dowiedzieć się więcej o trasach definiowanych przez użytkownika, przeczytaj artykuł [Trasy definiowane przez użytkownika](virtual-networks-udr-overview.md).

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *interfejsy sieciowe*. Gdy **interfejsy sieciowe** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **interfejsy sieciowe** bloku, który jest wyświetlany, kliknij przycisk interfejsu sieciowego, aby włączyć lub wyłączyć przekazywanie IP.
4. W bloku dla wybranego interfejsu sieciowego, kliknij **konfiguracje adresów IP** w **ustawienia** sekcji.
5. Kliknij przycisk **włączone** lub **wyłączone** (ustawienie domyślne) Aby zmienić to ustawienie.
6. Kliknij pozycję **Zapisz**.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Aktualizacja kart sieciowych az](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Zestaw AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet-assignment"></a>Zmień przypisanie podsieci

Można zmienić podsieci, ale nie sieci wirtualnej, przypisane do karty sieciowej.

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *interfejsy sieciowe*. Gdy **interfejsy sieciowe** pojawia się w wynikach wyszukiwania kliknij ją.
3. W **interfejsy sieciowe** bloku, który jest wyświetlany, kliknij przycisk interfejsu sieciowego, aby wyświetlić lub zmienić ustawienia.
4. Kliknij przycisk **konfiguracje adresów IP** w obszarze **ustawienia** w bloku dla wybranego interfejsu sieciowego. Jeśli prywatne adresy IP dla konfiguracji IP wyświetlane **(statyczny)** obok nich, należy zmienić metoda przypisywania adresów IP na dynamiczny, wykonując kroki, które należy wykonać. Wszystkich prywatnych adresów IP musi być przypisany za pomocą metody dynamiczne przydzielanie, aby zmienić przypisanie podsieci interfejsu sieciowego. Jeśli adresy są przypisane przy użyciu metody dynamicznej, przejdź do kroku 5. Adresy IPv4, są przypisane przy użyciu metody statyczne przypisania, należy wykonać następujące kroki, aby zmienić metodę przypisania do dynamicznego:
    - Kliknij pozycję konfiguracji IP, aby zmienić metody przypisywania adresów IPv4 dla z listy konfiguracje adresów IP.
    - W wyświetlonym bloku do konfiguracji protokołu IP, kliknij przycisk **dynamiczne** dla **przypisania** metody. Nie można przypisać adresów IPv6 za pomocą metody statyczne przypisania.
    - Kliknij pozycję **Zapisz**.
5. Wybierz podsieć, które mają zostać podłączone do interfejsu sieciowego, z **podsieci** listy rozwijanej.
6. Kliknij pozycję **Zapisz**. Nowe dynamiczne adresy przypisywane są zakres adresów podsieci dla nowej podsieci. Po przypisaniu interfejsu sieciowego do nowych podsieci, można przypisać statycznego adresu IPv4 z nowy zakres adresów podsieci po wybraniu. Aby dowiedzieć się więcej o dodawanie, zmienianie i usuwanie adresów IP dla karty sieciowej, przeczytaj [adresów IP zarządzanie](virtual-network-network-interface-addresses.md) artykułu.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Aktualizacja konfiguracji adresu ip karty sieciowej sieci az](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Zestaw AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-a-network-interface"></a>Usunąć interfejsu sieciowego

Można usunąć interfejsu sieciowego, dopóki nie jest dołączony do maszyny wirtualnej. Jeśli nie został dołączony do maszyny wirtualnej, należy najpierw umieścić maszyny wirtualnej w stanie zatrzymania (cofnięciu przydziału), a następnie odłączyć interfejsu sieciowego z maszyny wirtualnej, aby można było usunąć interfejsu sieciowego. Aby odłączyć interfejsu sieciowego z maszyny wirtualnej, wykonaj kroki [odłączyć interfejsu sieciowego z maszyny wirtualnej](virtual-network-network-interface-vm.md#vm-remove-nic) sekcji [Dodawanie lub usuwanie interfejsów sieciowych](virtual-network-network-interface-vm.md) artykułu. Trwa usuwanie maszyny wirtualnej Odłącza wszystkie interfejsy sieciowe podłączone do niego, ale nie powoduje usunięcia interfejsów sieciowych.

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, który jest przypisane (co najmniej) uprawnienia roli współautora sieci dla Twojej subskrypcji. Odczyt [wbudowanych ról dla kontroli dostępu opartej na rolach na platformie Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artykuł, aby dowiedzieć się więcej o przypisywanie ról i uprawnień do kont.
2. W polu zawierająca tekst, który *wyszukiwania zasobów* w górnej części portalu Azure, wpisz *interfejsy sieciowe*. Gdy **interfejsy sieciowe** pojawia się w wynikach wyszukiwania kliknij ją.
3. Kliknij prawym przyciskiem myszy interfejs sieciowy, aby usunąć, a następnie kliknij przycisk **usunąć**.
4. Kliknij przycisk **tak** aby potwierdzić usunięcie interfejsu sieciowego.

Po usunięciu interfejsu sieciowego są wydawane adresy MAC lub adres IP przypisane do niej.

**Polecenia**

|Narzędzie|Polecenie|
|---|---|
|Interfejs wiersza polecenia|[Usuń kartę sieciową sieci az](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Usuń AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Następne kroki
Aby utworzyć maszynę wirtualną z sieci wielu interfejsów lub adresy IP, przeczytaj następujące artykuły:

**Polecenia**

|Zadanie|Narzędzie|
|---|---|
|Tworzenie maszyny wirtualnej z wieloma kartami sieciowymi|[Interfejs wiersza polecenia](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [środowiska PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Tworzenie jednej maszyny Wirtualnej karty Sieciowej z wielu adresów IPv4|[Interfejs wiersza polecenia](virtual-network-multiple-ip-addresses-cli.md), [środowiska PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Tworzenie jednej maszyny Wirtualnej karty Sieciowej za pomocą prywatnego adresu IPv6 (za równoważenia obciążenia Azure)|[Interfejs wiersza polecenia](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [szablonu usługi Azure Resource Manager](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
