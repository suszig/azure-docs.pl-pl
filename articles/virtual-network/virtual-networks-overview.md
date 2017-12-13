---
title: "Sieć wirtualna platformy Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat pojęć sieci wirtualnej platformy Azure i funkcje."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: jdial
ms.openlocfilehash: 1bff8e4b052ffe8274c290abdebfbac7bf9a883d
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/13/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

Usługa Microsoft Azure Virtual Network umożliwia zasobów platformy Azure do bezpiecznego komunikowania się z innymi w sieci wirtualnej. Sieć wirtualna jest odzwierciedla w chmurze Twoją sieć. Sieć wirtualna jest logiczną izolacją chmury Azure w wersji dedykowanej do subskrypcji. Możesz połączyć sieci wirtualnych do innych sieci wirtualnych lub do sieci lokalnej. Na poniższej ilustracji przedstawiono niektóre możliwości usługi Azure Virtual Network:

![Diagram sieci](./media/virtual-networks-overview/virtual-network-overview.png)

Aby dowiedzieć się więcej o z następujących funkcji sieci wirtualnej platformy Azure, kliknij przycisk możliwości:
- **[Izolacja:](#isolation)**  sieci wirtualne są odizolowane od siebie. Można utworzyć osobne bloki adresów sieci wirtualnych umożliwiające tworzenie, testowanie i produkcji, które używają tego samego CIDR (na przykład 10.0.0.0/0). Z drugiej strony można utworzyć wiele sieci wirtualnych, które używają różnych bloków adresów CIDR i połączyć ze sobą sieci. Sieć wirtualną można podzielić na wiele podsieci. Platforma Azure udostępnia rozpoznawania nazw wewnętrznych dla zasobów wdrożony w sieci wirtualnej. Jeśli to konieczne, można skonfigurować sieć wirtualną do użycia zamiast rozpoznawania nazw wewnętrznych Azure własne serwery DNS.
- **[Komunikacja z Internetem:](#internet)**  zasoby, takie jak maszyn wirtualnych wdrożonych w sieci wirtualnej mają dostęp do Internetu, domyślnie. Można również włączyć dostęp przychodzący do określonych zasobów, zgodnie z potrzebami.
- **[Komunikacja zasobów platformy Azure:](#within-vnet)**  zasobów Azure wdrożony w sieci wirtualnej mogą komunikować się ze sobą przy użyciu prywatnych adresów IP, nawet jeśli zasoby są wdrożone w różnych podsieciach. Platforma Azure udostępnia domyślny routing między podsieciami, połączonych sieci wirtualnych i sieci lokalnej, więc nie trzeba konfigurować i zarządzać nimi trasy. W razie potrzeby można dostosować routingu platformy Azure.
- **[Połączenie wirtualnej sieci:](#connect-vnets)**  sieci wirtualnej mogą zostać połączone ze sobą, włączanie zasobów w dowolnej sieci wirtualnej do komunikowania się z zasobami w dowolnej sieci wirtualnej.
- **[Połączenie lokalne:](#connect-on-premises)**  sieci wirtualnej możliwe połączenie się z siecią lokalną, włączanie zasobów do komunikacji między sobą.
- **[Filtrowanie ruchu:](#filtering)**  można filtrować ruch sieciowy do i z zasobów w sieci wirtualnej przez źródłowy adres IP i port docelowy adres IP i portu i protokołu.
- **[Routing:](#routing)**  Opcjonalnie można zastąpić routingu przez skonfigurowanie własnego trasy domyślnej platformy Azure lub przez propagowanie BGP kieruje za pośrednictwem bramy sieci.

## <a name = "isolation"></a>Izolacja sieci i segmentacji

Można zaimplementować wiele sieci wirtualnych w każdym Azure [subskrypcji](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) i Azure [region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Każda sieć wirtualna jest odizolowana od innych sieci wirtualnych. Dla każdej sieci wirtualnej można:
- Określ niestandardowe prywatnych przestrzeni adresów IP przy użyciu prywatnych i publicznych adresów (RFC 1918). Azure przypisuje zasobów w sieci wirtualnej prywatnego adresu IP z przestrzeń adresów, które można przypisać.
- Segment sieci wirtualnej do co najmniej jednej podsieci i przydzielić część przestrzeni adresowej sieci wirtualnej dla każdej podsieci.
- Używanie rozpoznawania nazw platformy Azure lub Podaj własny serwer DNS do użytku przez zasobów w sieci wirtualnej. Aby dowiedzieć się więcej na temat rozpoznawania nazw w sieciach wirtualnych, zobacz [rozpoznawanie nazw dla zasobów w sieciach wirtualnych](virtual-networks-name-resolution-for-vms-and-role-instances.md) artykułu.

## <a name = "internet"></a>Komunikacja z Internetem
Wszystkie zasoby w sieci wirtualnej mogą komunikować się ruch wychodzący do Internetu. Domyślnie prywatnego adresu IP zasobu jest źródłowy adres sieciowy translacji (SNAT) do publicznego adresu IP wybrana przez infrastrukturę platformy Azure. Aby dowiedzieć się więcej na temat wychodzące połączenie z Internetem, przeczytaj [Opis połączeń wychodzących na platformie Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) artykułu. Aby zapobiec wychodzące połączenie z Internetem, można zaimplementować trasy niestandardowe lub filtrowanie ruchu.

Do komunikacji przychodzącej zasobów platformy Azure z Internetu lub komunikacji ruch wychodzący do Internetu bez SNAT, musi on mieć przypisaną publicznego adresu IP. Aby dowiedzieć się więcej na temat publiczne adresy IP, przeczytaj [publicznego adresu IP, adresy](virtual-network-public-ip-address.md) artykułu.

## <a name="within-vnet"></a>Zabezpieczenia komunikacji między zasobami Azure

Można wdrożyć maszyn wirtualnych w ramach sieci wirtualnej. Maszyny wirtualne komunikować się z innych zasobów w sieci wirtualnej za pośrednictwem karty sieciowej. Aby dowiedzieć się więcej na temat interfejsów sieciowych, zobacz [interfejsy sieciowe](virtual-network-network-interface.md).

Można także wdrożyć kilka typów zasobów platformy Azure do sieci wirtualnej, takie jak środowiska usługi Azure App Service i zestawy skalowania maszyny wirtualnej Azure. Aby uzyskać pełną listę zasobów platformy Azure, można wdrożyć w sieci wirtualnej, zobacz [integracji z usługą sieci wirtualnej dla usługi Azure](virtual-network-for-azure-services.md).

Niektórych zasobów nie można wdrożyć w sieci wirtualnej, ale umożliwiają ograniczenie komunikacji do zasobów tylko sieci wirtualnej. Aby dowiedzieć się więcej o tym, jak ograniczyć dostęp do zasobów, zobacz [punktów końcowych usługi sieci wirtualnej](virtual-network-service-endpoints-overview.md). 

## <a name="connect-vnets"></a>Łączenie sieci wirtualnej

Sieci wirtualne można połączyć ze sobą, włączanie zasobów w każdej sieci wirtualnej do komunikowania się ze sobą przy użyciu sieci wirtualnej komunikacji równorzędnej. Przepustowości i opóźnień w komunikacji między zasobami w różnych sieciach wirtualnych jest taka sama, jak gdyby zasoby w tej samej sieci wirtualnej. Aby dowiedzieć się więcej na temat komunikacji równorzędnej, przeczytaj [równorzędna sieci wirtualnej](virtual-network-peering-overview.md) artykułu.

## <a name="connect-on-premises"></a>Do połączenia z siecią lokalną

Sieci lokalnej można nawiązać sieć wirtualną przy użyciu dowolnej kombinacji następujących opcji:
- **Punkt lokacja wirtualnej sieci prywatnej (VPN):** określonych między sieci wirtualnej i jeden komputer w sieci. Każdy komputer, który chce nawiązać połączenie z siecią wirtualną należy skonfigurować połączenie niezależnie. Tego typu połączenia jest doskonałym, jeśli tylko rozpoczniesz pracę z platformą Azure, lub dla deweloperów, ponieważ wymaga żadnych zmian w istniejącej sieci. Połączenie korzysta z protokołu SSTP zapewnienie szyfrowaną komunikację przez Internet między systemem i siecią wirtualną. Opóźnienie dla sieci VPN punkt lokacja jest nieprzewidziany, ponieważ dane są przesyłane za pośrednictwem Internetu.
- **Sieć VPN lokacja lokacja:** między urządzenie sieci VPN i bramy sieci VPN Azure wdrożony w sieci wirtualnej. Ten typ połączenia pozwala dowolnego zasobu lokalnego zezwalają na dostęp do sieci wirtualnej. Połączenie jest IPSec i IKE sieci VPN, który zapewnia szyfrowaną komunikację przez Internet między urządzeniem lokalnymi i bramy sieci VPN platformy Azure. Czas oczekiwania na połączenie lokacja lokacja jest nieprzewidywalne, ponieważ dane są przesyłane za pośrednictwem Internetu.
- **Usługa Azure ExpressRoute:** ustanowić między siecią a Azure, za pośrednictwem partner usługi ExpressRoute. To połączenie jest prywatne. Ruch nie przechodzą przez Internet. Czas oczekiwania na połączenie ExpressRoute jest przewidywalne, ponieważ ruchu nie przechodzenie przez Internet.

Aby dowiedzieć się więcej na temat wszystkich poprzednich opcji połączenia, zobacz [Diagram topologii połączenia](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).

## <a name="filtering"></a>Filtrowanie ruchu sieciowego
Można filtrować ruch sieciowy między podsieciami przy użyciu jednego lub obu z następujących opcji:
- **Sieciowe grupy zabezpieczeń:** sieciowej grupy zabezpieczeń może zawierać wiele reguł zabezpieczeń dla ruchu przychodzącego i wychodzącego umożliwiające filtrować ruch źródłowy i docelowy adres IP, portu i protokołu. Grupa zabezpieczeń sieci można stosować do każdego interfejsu sieciowego w maszynie wirtualnej. Można także zastosować sieciową grupę zabezpieczeń do podsieci karty sieciowej, lub innych zasobów platformy Azure jest. Aby dowiedzieć się więcej na temat grup zabezpieczeń sieci, zobacz [sieciowej grupy zabezpieczeń](security-overview.md#network-security-groups).
- **Sieci wirtualnych urządzeń:** urządzenie sieci wirtualnej jest maszyną wirtualną z oprogramowaniem, która pełni funkcję sieci, takie jak zapory. Wyświetlanie listy dostępnej sieci wirtualnych urządzeń w [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). Urządzenie wirtualne sieci są również dostępne zawierających Optymalizacja sieci WAN i innych sieci ruchu funkcji. Wirtualnych urządzeń sieciowych zwykle są używane przez użytkownika lub trasy protokołu BGP. Urządzenie wirtualne sieci służy również do filtrowania ruchu między sieciami wirtualnymi.

## <a name="routing"></a>Kierować ruchem sieciowym

Platforma Azure tworzy tabele tras, które umożliwiają zasoby podłączone do żadnej podsieci w dowolnej sieci wirtualnej do komunikowania się ze sobą i Internet, domyślnie. Można zaimplementować jedną lub obie z poniższych opcji, aby zastąpić trasy domyślne, które tworzy Azure:
- **Trasy zdefiniowane przez użytkownika:** tworzenia tabel tras niestandardowych trasy tego formantu, których ruch jest kierowany do dla każdej podsieci. Aby dowiedzieć się więcej na temat trasy zdefiniowane przez użytkownika, zobacz [trasy zdefiniowane przez użytkownika](virtual-networks-udr-overview.md#user-defined).
- **Trasy protokołu BGP:** czy możesz nawiązać połączenie sieci wirtualnej sieci lokalnej przy użyciu połączenia bramy sieci VPN platformy Azure lub usługi ExpressRoute, można propagować trasy protokołu BGP do sieci wirtualne.

## <a name="pricing"></a>Cennik

Jest bezpłatna dla sieci wirtualnych, podsieci, tabele tras lub sieciowej grupy zabezpieczeń. Wychodzące użycia przepustowości połączenia internetowego, publicznego adresu IP adresów sieci wirtualnej komunikacji równorzędnej, bram sieci VPN i ExpressRoute każdego mają swoje własne cennik struktury. Widok [sieci wirtualnej](https://azure.microsoft.com/pricing/details/virtual-network), [bramy sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway), i [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) cennik strony, aby uzyskać więcej informacji.

## <a name="faq"></a>Często zadawane pytania

Aby zapoznać się z często zadawane pytania dotyczące sieci wirtualnej platformy Azure, zobacz [sieć wirtualna — często zadawane pytania](virtual-networks-faq.md) artykułu.

## <a name="next-steps"></a>Następne kroki

- Tworzenie pierwszej sieci wirtualnej i wdrażanie kilka maszyn wirtualnych, wykonując kroki opisane w [tworzenie pierwszej sieci wirtualnej](virtual-network-get-started-vnet-subnet.md).
- Utwórz połączenie punkt lokacja sieci wirtualnej, wykonując kroki opisane w [skonfigurować połączenie punkt lokacja](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Dowiedz się więcej o niektóre inne kluczowe [sieci możliwości](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) platformy Azure.
