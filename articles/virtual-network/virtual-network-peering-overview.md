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
ms.date: 06/06/2017
ms.author: narayan
ms.translationtype: Human Translation
ms.sourcegitcommit: 138f04f8e9f0a9a4f71e43e73593b03386e7e5a9
ms.openlocfilehash: bfba85160cd02baa337881653dbe6582d10ba073
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017


---
# <a name="virtual-network-peering"></a>Wirtualne sieci równorzędne
Wirtualne sieci równorzędne umożliwiają połączenie dwóch sieci wirtualnych z tego samego regionu za pośrednictwem sieci szkieletowej platformy Azure. Po nawiązaniu połączenia równorzędnego dwie sieci wirtualne są traktowane jako jedna sieć. Dwie sieci wirtualne są nadal zarządzane jako oddzielne zasoby, ale maszyny wirtualne w tych wirtualnych sieciach równorzędnych mogą komunikować się bezpośrednio przy użyciu prywatnych adresów IP.

Ruch między maszynami wirtualnymi w wirtualnych sieciach równorzędnych odbywa się za pośrednictwem infrastruktury Azure — tak jak ruch między maszynami wirtualnymi w tej samej sieci wirtualnej. Korzystanie z wirtualnych sieci równorzędnych zapewnia m.in. następujące korzyści:

* Połączenie o małych opóźnieniach i dużej przepustowości między zasobami w różnych sieciach wirtualnych.
* Możliwość używania zasobów (np. urządzeń sieciowych i bram sieci VPN) jako punktów tranzytowych w wirtualnych sieciach równorzędnych.
* Możliwość połączenia dwóch sieci wirtualnych utworzonych za pomocą modelu wdrażania przy użyciu usługi Azure Resource Manager lub połączenia jednej sieci wirtualnej utworzonej za pomocą usługi Resource Manager z drugą siecią wirtualną utworzoną za pomocą klasycznego modelu wdrażania. Zapoznaj się z artykułem [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Informacje na temat modeli wdrażania platformy Azure), aby dowiedzieć się więcej o różnicach między tymi dwoma modelami wdrażania.

Wymagania i kluczowe aspekty wirtualnych sieci równorzędnych:

* Wirtualne sieci równorzędne muszą znajdować się w tym samym regionie platformy Azure. Sieci wirtualne z różnych regionów platformy Azure można połączyć za pomocą usługi [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* Wirtualne sieci równorzędne muszą mieć nienakładające się przestrzenie adresów IP.
* Nie można dodać ani usunąć przestrzeni adresowych z sieci wirtualnej po połączeniu sieci wirtualnej z inną siecią wirtualną za pomocą komunikacji równorzędnej.
* Wirtualne sieci równorzędne obejmują dwie sieci wirtualne. Nie istnieje żadna pochodna relacja przechodnia między tymi sieciami. Na przykład: jeśli wirtualna sieć A jest połączona z wirtualną siecią B, a wirtualna sieć B jest połączona z wirtualną siecią C, to sieć A *nie* jest połączona z siecią C.
* Komunikacja równorzędna może zostać nawiązana między sieciami wirtualnymi istniejącymi w dwóch różnych subskrypcjach, o ile połączenie zostanie autoryzowane przez uprawnionego użytkownika w obu subskrypcjach, a subskrypcje będą skojarzone z tą samą dzierżawą usługi Active Directory. Do łączenia sieci wirtualnych w ramach subskrypcji skojarzonych z różnymi dzierżawcami usługi Active Directory można użyć usługi [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V).
* Komunikacja równorzędna może zostać nawiązana między sieciami wirtualnymi, jeśli obie sieci zostały utworzone za pomocą modelu wdrażania przy użyciu usługi Resource Manager lub jeśli jedna z nich została utworzona w ten sposób, a druga za pomocą klasycznego modelu wdrażania. Nie można jednak nawiązać komunikacji równorzędnej między dwiema sieciami wirtualnymi utworzonymi za pomocą klasycznego modelu wdrażania. W przypadku nawiązywania komunikacji równorzędnej między sieciami wirtualnymi utworzonymi za pomocą różnych modeli wdrażania obie te sieci muszą istnieć w *tej samej* subskrypcji. Możliwość nawiązywania komunikacji równorzędnej w *różnych* subskrypcjach między sieciami wirtualnymi utworzonymi za pomocą różnych modeli wdrażania istnieje w wersji **zapoznawczej**. Więcej szczegółowych informacji zawiera artykuł [Create a virtual network peering](virtual-network-create-peering.md#different-subscriptions-different-deployment-models) (Tworzenie wirtualnych sieci równorzędnych).
* Chociaż komunikacja między maszynami wirtualnymi w wirtualnych sieciach równorzędnych nie ma żadnych dodatkowych ograniczeń co do przepustowości, należy pamiętać, że nadal obowiązuje ograniczenie maksymalnej przepustowości sieci uzależnione od rozmiaru maszyny wirtualnej. Aby dowiedzieć się więcej o maksymalnej przepustowości dla różnych rozmiarów maszyn wirtualnych, przeczytaj artykuły dotyczące rozmiarów maszyn wirtualnych w systemach [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) i [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

![Podstawowe wirtualne sieci równorzędne](./media/virtual-networks-peering-overview/figure01.png)

## <a name="connectivity"></a>Łączność
Po połączeniu dwóch sieci wirtualnych za pomocą komunikacji równorzędnej maszyny wirtualne lub role usług w chmurze w sieci wirtualnej mogą łączyć się bezpośrednio z innymi zasobami w sieci wirtualnej połączonej za pomocą komunikacji równorzędnej. Te dwie sieci wirtualne mają pełną łączność na poziomie adresów IP.

Opóźnienie sieciowe podczas komunikacji dwustronnej między dwiema maszynami wirtualnymi w wirtualnych sieciach równorzędnych jest takie samo jak w przypadku komunikacji dwustronnej w lokalnej sieci wirtualnej. Przepływność sieci zależy od przepustowości dozwolonej dla maszyny wirtualnej proporcjonalnie do jej rozmiaru. Nie ma żadnych dodatkowych ograniczeń przepustowości w obrębie komunikacji równorzędnej.

Ruch między maszynami wirtualnymi w wirtualnych sieciach równorzędnych odbywa się bezpośrednio za pomocą infrastruktury zaplecza platformy Azure, a nie przy użyciu bramy.

Maszyny wirtualne połączone z siecią wirtualną mogą uzyskiwać dostęp do wewnętrznych punktów końcowych z równoważeniem obciążenia w równorzędnej sieci wirtualnej. Sieciowe grupy zabezpieczeń można stosować w dowolnej sieci wirtualnej, aby w razie potrzeby zablokować dostęp do innych sieci wirtualnych lub podsieci.

Podczas konfigurowania wirtualnych sieci równorzędnych można otwierać i zamykać reguły grupy zabezpieczeń sieci między sieciami wirtualnymi. W przypadku otwarcia pełnej łączności między równorzędnymi sieciami wirtualnymi (jest to opcja domyślna) można zablokować lub odmówić określonego rodzaju dostępu do określonych podsieci lub maszyn wirtualnych. Aby dowiedzieć się więcej na temat sieciowych grup zabezpieczeń, zapoznaj się z artykułem [Omówienie sieciowych grup zabezpieczeń](virtual-networks-nsg.md).

Usługa rozpoznawania wewnętrznych nazw DNS na platformie Azure dla maszyn wirtualnych nie działa w sieciach wirtualnych połączonych za pomocą komunikacji równorzędnej. Maszyny wirtualne mają wewnętrzne nazwy DNS rozpoznawalne tylko w lokalnej sieci wirtualnej. Można jednak skonfigurować maszyny wirtualne połączone z wirtualnymi sieciami równorzędnymi jako serwery DNS dla sieci wirtualnej. Aby uzyskać więcej szczegółowych informacji, zapoznaj się z artykułem [Name resolution using your own DNS server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) (Rozpoznawanie nazw za pomocą własnego serwera DNS).

## <a name="service-chaining"></a>Tworzenie łańcuchów usług
Użytkownicy mogą konfigurować trasy zdefiniowane przez użytkownika prowadzące do maszyn wirtualnych w wirtualnych sieciach równorzędnych jako adresy IP „kolejnego przeskoku”, aby umożliwić tworzenie łańcucha usług. Tworzenie łańcucha usług umożliwia bezpośrednie kierowanie ruchu z jednej sieci wirtualnej do urządzenia wirtualnego w wirtualnej sieci równorzędnej przy użyciu tras zdefiniowanych przez użytkownika.

Można również skutecznie tworzyć środowiska typu gwiazdy, w których serwer centralny jest hostem składników infrastruktury, takich jak sieciowe urządzenie wirtualne. Następnie wszystkie sieci wirtualne typu gwiazda można połączyć za pomocą komunikacji równorzędnej z centralną siecią wirtualną. Ruch może przepływać za pośrednictwem wirtualnych urządzeń sieciowych działających w centralnej sieci wirtualnej. Krotko mówiąc, wirtualne sieci równorzędne umożliwiają użycie jako adresu IP kolejnego przeskoku w trasie zdefiniowanej przez użytkownika adresu IP maszyny wirtualnej w wirtualnej sieci równorzędnej. Aby dowiedzieć się więcej o trasach definiowanych przez użytkownika, zapoznaj się z omówieniem [tras definiowanych przez użytkownika](virtual-networks-udr-overview.md).

## <a name="gateways-and-on-premises-connectivity"></a>Bramy i łączność lokalna
Każda sieć wirtualna — niezależnie od tego, czy jest połączona za pomocą komunikacji równorzędnej z inną siecią wirtualną — może mieć własną bramę i używać jej do łączenia się z lokalną infrastrukturą sieciową. Użytkownicy mogą również konfigurować [połączenia między sieciami wirtualnymi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json) przy użyciu bram, nawet jeśli te sieci wirtualne są połączone za pomocą komunikacji równorzędnej.

Po skonfigurowaniu obu opcji łączności między sieciami wirtualnymi ruch między tymi sieciami wirtualnymi jest oparty na konfiguracji komunikacji równorzędnej (to znaczy odbywa się za pośrednictwem sieci szkieletowej platformy Azure).

Gdy sieci wirtualne są połączone za pomocą komunikacji równorzędnej, użytkownicy mogą również skonfigurować bramę w wirtualnej sieci równorzędnej jako punkt tranzytowy do infrastruktury lokalnej. W tym przypadku sieć wirtualna korzystająca z bramy zdalnej nie może mieć własnej bramy. Jedna sieć wirtualna może mieć tylko jedną bramę. Brama może być lokalna lub zdalna (w wirtualnej sieci równorzędnej), jak przedstawiono na poniższej ilustracji:

![Tranzyt w komunikacji równorzędnej w sieci wirtualnej](./media/virtual-networks-peering-overview/figure02.png)

Przesyłanie danych za pomocą bramy nie jest obsługiwane w relacji komunikacji równorzędnej między sieciami wirtualnymi utworzonymi za pomocą różnych modeli wdrażania. Aby przesyłanie danych za pomocą bramy działało, obie sieci wirtualne będące w relacji komunikacji równorzędnej muszą zostać utworzone za pomocą usługi Resource Manager.

W przypadku połączenia za pomocą komunikacji równorzędnej sieci wirtualnych współużytkujących jedno połączenie sługi Azure ExpressRoute ruch między nimi jest oparty na relacji komunikacji równorzędnej (to znaczy odbywa się za pośrednictwem sieci szkieletowej platformy Azure). Użytkownicy mogą nadal korzystać z bram lokalnych w poszczególnych sieciach wirtualnych, aby łączyć się z obwodem lokalnym. Można również użyć bramy współdzielonej i skonfigurować tranzyt dla łączności lokalnej.

## <a name="provisioning"></a>Inicjowanie obsługi
Wirtualne sieci równorzędne wymagają odpowiednich uprawnień. Jest to oddzielna funkcja w przestrzeni nazw VirtualNetworks. Użytkownik może uzyskać określone uprawnienia do autoryzowania komunikacji równorzędnej. Użytkownik mający dostęp do odczytu i zapisu do sieci wirtualnej automatycznie dziedziczy te uprawnienia.

Użytkownik będący administratorem lub użytkownikiem mającym uprawienia do komunikacji równorzędnej może zainicjować operację komunikacji równorzędnej w innej sieci wirtualnej. Jeśli po drugiej stronie istnieje zgodne żądanie komunikacji równorzędnej i spełnione są pozostałe wymagania, połączenie za pomocą komunikacji równorzędnej zostanie nawiązane.

## <a name="limits"></a>Limity
Istnieją limity liczby dozwolonych połączeń za pomocą komunikacji równorzędnej dla jednej sieci wirtualnej. Więcej informacji zawiera temat [Limity dotyczące sieci platformy Azure](../azure-subscription-service-limits.md#networking-limits).

## <a name="pricing"></a>Cennik
Istnieje nominalna opłata za ruch przychodzący i wychodzący w wirtualnych sieciach równorzędnych. Aby uzyskać więcej informacji, odwiedź [stronę cennika](https://azure.microsoft.com/pricing/details/virtual-network).

## <a name="next-steps"></a>Następne kroki

* Ukończ [samouczek dotyczący wirtualnych sieci równorzędnych](virtual-network-create-peering.md)
* Zapoznaj się ze wszystkimi [ustawieniami wirtualnych sieci równorzędnych oraz sposobami ich zmiany](virtual-network-manage-peering.md).

