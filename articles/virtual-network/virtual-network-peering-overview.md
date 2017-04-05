---
title: "Wirtualne sieci równorzędne platformy Azure | Microsoft Docs"
description: "Dowiedz się więcej na temat wirtualnych sieci równorzędnych na platformie Azure."
services: virtual-network
documentationcenter: na
author: NarayanAnnamalai
manager: jefco
editor: tysonn
ms.assetid: eb0ba07d-5fee-4db0-b1cb-a569b7060d2a
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/17/2016
ms.author: narayan
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: 6fbcdcf77f46a3c643e8fedc1d112588cbd7befc
ms.lasthandoff: 04/03/2017


---
# <a name="virtual-network-peering"></a>Wirtualne sieci równorzędne
Komunikacja równorzędna sieci wirtualnych umożliwia łączenie dwóch sieci wirtualnych w tym samym regionie za pośrednictwem sieci szkieletowej platformy Azure. Po połączeniu za pomocą komunikacji równorzędnej dwie sieci wirtualne są traktowane jako jedna do celów związanych z łącznością. Te dwie sieci są nadal zarządzane jako oddzielne zasoby, ale maszyny wirtualne w wirtualnych sieciach równorzędnych mogą komunikować się bezpośrednio przy użyciu prywatnych adresów IP.

Ruch między maszynami wirtualnymi w wirtualnych sieciach równorzędnych odbywa się za pośrednictwem infrastruktury platformy Azure, tak jak ruch między maszynami wirtualnymi w tej samej sieci wirtualnej. Korzystanie z komunikacji równorzędnej w sieci wirtualnej zapewnia m.in. następujące korzyści:

* Połączenie o małych opóźnieniach i dużej przepustowości między zasobami w różnych sieciach wirtualnych.
* Możliwość korzystania z zasobów, takich jak urządzenia sieciowe i bramy sieci VPN, jako punktów tranzytowych w wirtualnych sieciach równorzędnych.
* Możliwość połączenia przy użyciu komunikacji równorzędnej dwóch sieci wirtualnych utworzonych za pomocą modelu wdrażania przy użyciu usługi Azure Resource Manager lub połączenia przy użyciu komunikacji równorzędnej jednej sieci wirtualnej utworzonej za pomocą klasycznego modelu wdrażania. Zapoznaj się z artykułem [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md) (Informacje na temat modeli wdrażania platformy Azure), aby dowiedzieć się więcej o różnicach między tymi dwoma modelami wdrażania.

Wymagania i kluczowe aspekty komunikacji równorzędnej w sieci wirtualnej:

* Wirtualne sieci równorzędne muszą istnieć w tym samym regionie platformy Azure.
* Wirtualne sieci równorzędne muszą mieć nienakładające się przestrzenie adresów IP.
* Komunikacja równorzędna sieci wirtualnych odbywa się między dwiema sieciami wirtualnymi, ale nie istnieje żadna pochodna relacja przechodnia między tymi sieciami. Na przykład jeśli zachodzi komunikacja równorzędna między sieciami VNetA i VNetB oraz komunikacja równorzędna między sieciami VNetB i VNetC, *nie* zachodzi komunikacja równorzędna między sieciami VNetA i VNetC.
* Komunikacja równorzędna może zostać nawiązana między sieciami wirtualnymi istniejącymi w dwóch różnych subskrypcjach, o ile połączenie za pomocą komunikacji równorzędnej zostanie autoryzowane przez uprawnionego użytkownika w obu subskrypcjach, a subskrypcje są skojarzone z tą samą dzierżawą usługi Active Directory.
* Komunikacja równorzędna może zostać nawiązana między sieciami wirtualnymi, jeśli obie sieci zostały utworzone za pomocą modelu wdrażania przy użyciu usługi Resource Manager lub jeśli jedna z nich została utworzona w ten sposób, a druga za pomocą klasycznego modelu wdrażania. Nie można jednak nawiązać komunikacji równorzędnej między dwiema sieciami wirtualnymi utworzonymi za pomocą klasycznego modelu wdrażania. W przypadku nawiązywania komunikacji równorzędnej między sieciami wirtualnymi utworzonymi za pomocą różnych modeli wdrażania obie te sieci muszą istnieć w *tej samej* subskrypcji. Możliwość nawiązywania komunikacji równorzędnej w *różnych* subskrypcjach między sieciami wirtualnymi utworzonymi za pomocą różnych modeli wdrażania istnieje w wersji **zapoznawczej**. Więcej szczegółowych informacji zawiera artykuł [Create a virtual network peering using Powershell](virtual-networks-create-vnetpeering-arm-ps.md) (Tworzenie wirtualnych sieci równorzędnych za pomocą programu Powershell).
* Chociaż komunikacja między maszynami wirtualnymi w wirtualnych sieciach równorzędnych nie ma żadnych dodatkowych ograniczeń przepustowości, należy pamiętać, że nadal obowiązuje ograniczenie maksymalnej przepustowości sieci uzależnione od rozmiaru maszyny wirtualnej. Aby dowiedzieć się więcej o maksymalnej przepustowości dla różnych rozmiarów maszyn wirtualnych, przeczytaj artykuły dotyczące rozmiarów maszyn wirtualnych w systemach [Windows](../virtual-machines/windows/sizes.md) i [Linux](../virtual-machines/linux/sizes.md).

![Podstawowa komunikacja równorzędna w sieci wirtualnej](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Łączność
Po nawiązaniu komunikacji równorzędnej między dwiema sieciami wirtualnymi maszyny wirtualne lub role usługi Cloud Services w sieci wirtualnej mogą nawiązywać bezpośrednie połączenia z innymi zasobami połączonymi z wirtualną siecią równorzędną. Te dwie sieci wirtualne mają pełną łączność na poziomie IP.

Opóźnienie sieci podczas komunikacji dwustronnej między dwiema maszynami wirtualnymi w wirtualnych sieciach równorzędnych jest takie samo jak w przypadku komunikacji dwustronnej w pojedynczej sieci wirtualnej. Przepływność sieci zależy od przepustowości dozwolonej dla maszyny wirtualnej proporcjonalnie do jej rozmiaru. Nie ma żadnych dodatkowych ograniczeń przepustowości w obrębie komunikacji równorzędnej.

Ruch między maszynami wirtualnymi w wirtualnych sieciach równorzędnych odbywa się bezpośrednio za pomocą infrastruktury zaplecza platformy Azure, a nie przy użyciu bramy.

Maszyny wirtualne połączone z siecią wirtualną mogą uzyskiwać dostęp do wewnętrznych punktów końcowych z równoważeniem obciążenia (ILB) w równorzędnej sieci wirtualnej. Sieciowe grupy zabezpieczeń można stosować w dowolnej sieci wirtualnej, aby w razie potrzeby blokować dostęp do innych sieci wirtualnych lub podsieci.

Podczas konfigurowania komunikacji równorzędnej można otworzyć lub zamknąć reguły NSG między sieciami wirtualnymi. Jeśli otworzysz pełną łączność między wirtualnymi sieciami równorzędnymi (jest to opcja domyślna), możesz zastosować grupy zabezpieczeń sieci do określonych podsieci lub maszyn wirtualnych w celu zablokowania lub odmowy konkretnego dostępu. Aby dowiedzieć się więcej na temat sieciowych grup zabezpieczeń, zapoznaj się z artykułem [Sieciowe grupy zabezpieczeń](virtual-networks-nsg.md).

Usługa rozpoznawania wewnętrznych nazw DNS na platformie Azure dla maszyn wirtualnych nie działa w wirtualnych sieciach równorzędnych. Maszyny wirtualne mają wewnętrzne nazwy DNS rozpoznawalne tylko w lokalnej sieci wirtualnej. Można jednak skonfigurować maszyny wirtualne połączone z równorzędnymi sieciami wirtualnymi jako serwery DNS dla sieci wirtualnej. Aby uzyskać więcej szczegółowych informacji, zapoznaj się z artykułem [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) (Rozpoznawanie nazw za pomocą własnego serwera DNS).

## <a name="service-chaining"></a>Tworzenie łańcuchów usług
Istnieje możliwość skonfigurowania tras zdefiniowanych przez użytkownika prowadzących do maszyn wirtualnych w wirtualnych sieciach równorzędnych jako adresów IP „kolejnego przeskoku”, jak przedstawiono na diagramie w dalszej części tego artykułu. Umożliwia to tworzenie łańcuchów usług, dzięki czemu można kierować ruch z jednej sieci wirtualnej do urządzenia wirtualnego działającego w wirtualnej sieci równorzędnej za pomocą tras zdefiniowanych przez użytkownika.

Można również skutecznie tworzyć środowiska typu gwiazdy, w których serwer centralny jest hostem składników infrastruktury, takich jak sieciowe urządzenie wirtualne. Wszystkie wirtualne sieci-satelity mogą następnie łączyć się z nim za pomocą komunikacji równorzędnej oraz kierować część ruchu do urządzeń działających w sieci wirtualnej serwera centralnego. Krotko mówiąc, komunikacja równorzędna sieci wirtualnych umożliwia użycie adresu IP kolejnego przeskoku w trasie zdefiniowanej przez użytkownika jako adresu IP maszyny wirtualnej w wirtualnej sieci równorzędnej. Aby uzyskać dodatkowe informacje dotyczące tras zdefiniowanych przez użytkownika, zobacz artykuł o [trasach definiowanych przez użytkownika](virtual-networks-udr-overview.md).

## <a name="gateways-and-on-premises-connectivity"></a>Bramy i łączność lokalna
Każda sieć wirtualna, niezależnie od tego, czy jest połączona za pomocą komunikacji równorzędnej z inną siecią wirtualną, może mieć własną bramę i używać jej do łączenia się z siecią lokalną. Użytkownicy mogą również konfigurować [połączenia między sieciami wirtualnymi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) przy użyciu bram, nawet jeśli te sieci wirtualne są połączone za pomocą komunikacji równorzędnej.

Po skonfigurowaniu obu opcji łączności między sieciami wirtualnymi ruch między tymi sieciami wirtualnymi jest oparty na konfiguracji komunikacji równorzędnej (to znaczy odbywa się za pośrednictwem sieci szkieletowej platformy Azure).

Gdy sieci wirtualne są połączone za pomocą komunikacji równorzędnej, użytkownicy mogą również skonfigurować bramę w wirtualnej sieci równorzędnej jako punkt tranzytowy do sieci lokalnej. W tym przypadku sieć wirtualna korzystająca z bramy zdalnej nie może mieć własnej bramy. Sieć wirtualna może mieć tylko jedną bramę. Brama może być lokalna lub zdalna (w wirtualnej sieci równorzędnej), jak przedstawiono na poniższej ilustracji:

![Tranzyt w komunikacji równorzędnej w sieci wirtualnej](./media/virtual-networks-peering-overview/figure02.png)

Przesyłanie danych za pomocą bramy nie jest obsługiwane w relacji komunikacji równorzędnej między sieciami wirtualnymi utworzonymi za pomocą różnych modeli wdrażania. Aby przesyłanie danych za pomocą bramy działało, obie sieci wirtualne będące w relacji komunikacji równorzędnej muszą zostać utworzone za pomocą usługi Resource Manager.

W przypadku połączenia za pomocą komunikacji równorzędnej sieci wirtualnych współużytkujących jedno połączenie usługi Azure ExpressRoute ruch między nimi jest oparty na relacji komunikacji równorzędnej (to znaczy odbywa się za pośrednictwem sieci szkieletowej platformy Azure). Bramy lokalne nadal mogą być używane w poszczególnych sieciach wirtualnych do łączenia się z obwodem lokalnym. Można również użyć bramy współdzielonej i skonfigurować tranzyt dla łączności lokalnej.

## <a name="provisioning"></a>Inicjowanie obsługi
Komunikacja równorzędna w sieci wirtualnej jest operacją wymagającą odpowiednich uprawnień. Jest to oddzielna funkcja w przestrzeni nazw VirtualNetworks. Użytkownik może uzyskać określone uprawnienia do autoryzowania komunikacji równorzędnej. Użytkownik mający dostęp do odczytu i zapisu do sieci wirtualnej automatycznie dziedziczy te uprawnienia.

Użytkownik będący administratorem lub użytkownikiem mającym uprawienia do komunikacji równorzędnej może zainicjować operację komunikacji równorzędnej w innej sieci wirtualnej. Jeśli po drugiej stronie istnieje zgodne żądanie komunikacji równorzędnej i spełnione są wymagania, zostanie nawiązane połączenie za pomocą komunikacji równorzędnej.

Aby uzyskać więcej informacji na temat nawiązywania połączenia między dwiema sieciami wirtualnych za pomocą komunikacji równorzędnej sieci wirtualnych, zapoznaj się z artykułami opisanymi w sekcji [Następne kroki](#next-steps) w tym artykule.

## <a name="limits"></a>Limity
Istnieją limity liczby dozwolonych połączeń za pomocą komunikacji równorzędnej dla jednej sieci wirtualnej. Więcej informacji zawiera temat [Limity dotyczące sieci platformy Azure](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Cennik
Istnieje nominalna opłata za ruch przychodzący i wychodzący podczas korzystania z komunikacji równorzędnej sieci wirtualnych. Więcej informacji zawiera [strona cennika](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Następne kroki
Dowiedz się, jak utworzyć komunikację równorzędną sieci wirtualnych, korzystając z następujących narzędzi:

* [Witryna Azure Portal](virtual-networks-create-vnetpeering-arm-portal.md)
* [Azure PowerShell](virtual-networks-create-vnetpeering-arm-ps.md)
* [Szablon usługi Azure Resource Manager](virtual-networks-create-vnetpeering-arm-template-click.md)

