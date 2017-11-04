---
title: "Sieć wirtualna platformy Azure | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat pojęć sieci wirtualnej platformy Azure i funkcje."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 9633de4b-a867-4ddf-be3c-a332edf02e24
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/23/2017
ms.author: jdial
ms.openlocfilehash: 6d6afd2b9b956138ed400fbd6cabd3b480fde0f0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-virtual-network"></a>Azure Virtual Network

Usługi sieci wirtualnej platformy Azure pozwala bezpiecznie łączyć z zasobami Azure ze sobą w sieciach wirtualnych (sieci wirtualne). Sieci wirtualnej jest odzwierciedla w chmurze Twoją sieć. Sieci wirtualnej jest logiczną izolacją chmury Azure w wersji dedykowanej do subskrypcji. Sieci wirtualne mogą łączyć się z siecią lokalną. Na poniższej ilustracji przedstawiono niektóre możliwości usługi Azure Virtual Network:

![Diagram sieciowy](./media/virtual-networks-overview/virtual-network-overview.png)

Aby dowiedzieć się więcej o z następujących funkcji sieci wirtualnej platformy Azure, kliknij przycisk możliwości:
- **[Izolacja:](#isolation)**  sieci wirtualne są odizolowane od siebie. Można utworzyć oddzielne sieci wirtualnych do tworzenia, testowania i produkcji, korzystające z tego samego bloków adresów CIDR. Z drugiej strony możesz utworzyć wiele sieci wirtualnych, użyj innego bloków adresów CIDR i połączyć ze sobą sieci. Sieć wirtualną można podzielić na wiele podsieci. Platforma Azure udostępnia rozpoznawania nazw wewnętrznych dla maszyn wirtualnych i usług w chmurze wystąpień roli podłączone do sieci wirtualnej. Opcjonalnie można skonfigurować sieć wirtualną do użycia zamiast rozpoznawania nazw wewnętrznych Azure własne serwery DNS.
- **[Połączenie z Internetem:](#internet)**  wszystkie maszyny wirtualne Azure (VM) i usługi w chmurze wystąpień roli podłączone do sieci wirtualnej mają dostęp do Internetu, domyślnie. Można również włączyć dostęp przychodzący do określonych zasobów, zgodnie z potrzebami.
- **[Łączność zasobów platformy Azure:](#within-vnet)**  zasobów platformy Azure, takich jak usługi w chmurze i maszyn wirtualnych można podłączyć do tej samej sieci wirtualnej. Zasoby można połączyć ze sobą przy użyciu prywatnych adresów IP, nawet jeśli znajdują się w różnych podsieciach. Platforma Azure udostępnia domyślny routing między podsieciami, sieci wirtualnych i sieci lokalnej, więc nie trzeba konfigurować i zarządzać nimi trasy.
- **[Łączność sieci wirtualnej:](#connect-vnets)**  sieci wirtualne mogą zostać połączone ze sobą, włączanie zasoby podłączone do żadnych sieci wirtualnej do komunikowania się z dowolnego zasobu w innych sieci wirtualnej.
- **[Połączenie lokalne:](#connect-on-premises)**  sieci wirtualnych można podłączyć do sieci lokalnej za pośrednictwem sieci prywatnej połączeń między siecią a Azure lub za pośrednictwem połączenia sieci VPN typu lokacja lokacja za pośrednictwem Internetu.
- **[Filtrowanie ruchu:](#filtering)**  ruchu sieciowego wystąpień roli maszyny Wirtualnej i usługi w chmurze można użyć do filtrowania ruchu przychodzącego i wychodzącego przez źródłowy adres IP i port docelowy adres IP i portu i protokołu.
- **[Routing:](#routing)**  można opcjonalnie zastąpić domyślne Azure routingu, konfigurowanie własnego trasy lub użyj trasy protokołu BGP za pośrednictwem bramy sieci.

## <a name = "isolation"></a>Izolacja sieci i segmentacji

Można zaimplementować wiele sieci wirtualnych w każdym Azure [subskrypcji](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) i Azure [region](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region). Każda sieć wirtualna jest odizolowana od innych sieci wirtualnych. Dla każdej sieci wirtualnej można:
- Określ niestandardowe prywatnych przestrzeni adresów IP przy użyciu prywatnych i publicznych adresów (RFC 1918). Przypisuje Azure zasoby podłączone do sieci wirtualnej prywatnego adresu IP z przestrzeń adresów, które można przypisać.
- Segment sieci wirtualnej do co najmniej jednej podsieci i przydzielić część przestrzeni adresowej sieci wirtualnej dla każdej podsieci.
- Użyj rozpoznawania nazw platformy Azure lub Podaj własny serwer DNS do użytku przez zasoby podłączone do sieci wirtualnej. Aby dowiedzieć się więcej na temat rozpoznawania nazw w sieci wirtualnych, przeczytaj [rozpoznawanie nazw dla maszyn wirtualnych i usług w chmurze](virtual-networks-name-resolution-for-vms-and-role-instances.md) artykułu.

## <a name = "internet"></a>Połączenie z Internetem
Wszystkie zasoby podłączone do sieci wirtualnej domyślnie mają łączność wychodząca z Internetem. Prywatny adres IP zasobu jest źródłowy adres sieciowy translacji (SNAT) do publicznego adresu IP przez infrastrukturę platformy Azure. Aby dowiedzieć się więcej na temat wychodzące połączenie z Internetem, przeczytaj [Opis połączeń wychodzących na platformie Azure](..\load-balancer\load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json#standalone-vm-with-no-instance-level-public-ip-address) artykułu. Łączność domyślne można zmienić dzięki implementacji niestandardowego routingu i filtrowanie ruchu.

Do komunikacji przychodzącej zasobów platformy Azure z Internetu lub komunikacji ruch wychodzący do Internetu bez SNAT, musi on mieć przypisaną publicznego adresu IP. Aby dowiedzieć się więcej na temat publiczne adresy IP, przeczytaj [publicznego adresu IP, adresy](virtual-network-public-ip-address.md) artykułu.

## <a name="within-vnet"></a>Łączenie zasobów platformy Azure
Możesz połączyć kilka zasobów platformy Azure do sieci wirtualnej, takie jak maszyn wirtualnych (VM), usługi w chmurze środowiska usługi App Service i zestawy skalowania maszyny wirtualnej. Połączenie maszyn wirtualnych do podsieci w sieci wirtualnej za pośrednictwem interfejsu sieciowego (NIC). Aby dowiedzieć się więcej na temat kart sieciowych, przeczytaj [interfejsy sieciowe](virtual-network-network-interface.md) artykułu.

## <a name="connect-vnets"></a>Łączenie sieci wirtualnej

Sieci wirtualne można połączyć ze sobą, włączanie zasoby podłączone do jednej sieci wirtualnej do komunikują się ze sobą za pośrednictwem sieci wirtualnych. Do sieci wirtualnych się ze sobą łączyć, można użyć jednego lub obu z następujących opcji:
- **Komunikacja równorzędna:** umożliwia zasoby podłączone do różnych sieci wirtualnych platformy Azure w ramach tej samej lokalizacji platformy Azure do komunikowania się ze sobą. Przepustowości i opóźnień między sieciami wirtualnymi jest taka sama, jak gdyby zasoby były podłączone do tej samej sieci wirtualnej. Aby dowiedzieć się więcej na temat komunikacji równorzędnej, przeczytaj [równorzędna sieci wirtualnej](virtual-network-peering-overview.md) artykułu.
- **Połączenia do wirtualnymi:** umożliwia zasoby podłączone do różnych sieć wirtualną Azure w ramach tego samego lub innego lokalizacji platformy Azure. W przeciwieństwie do komunikacji równorzędnej, przepustowości jest ograniczona między sieciami wirtualnymi, ponieważ ruch musi przepływać przez bramę sieci VPN platformy Azure. Aby dowiedzieć się więcej o nawiązywaniu połączeń sieci wirtualnych z połączeniem do wirtualnymi, przeczytaj [skonfigurować połączenia do wirtualnymi](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu.

## <a name="connect-on-premises"></a>Do połączenia z siecią lokalną

Sieci lokalnej można nawiązać sieć wirtualną przy użyciu dowolnej kombinacji następujących opcji:
- **Punkt lokacja wirtualnej sieci prywatnej (VPN):** między jednym komputerze podłączone do sieci i sieci wirtualnej. Tego typu połączenia jest doskonałym, jeśli tylko rozpoczniesz pracę z platformą Azure, lub dla deweloperów, ponieważ wymaga żadnych zmian w istniejącej sieci. Połączenie korzysta z protokołu SSTP zapewnienie szyfrowaną komunikację przez Internet między Komputerem a sieci wirtualnej. Opóźnienie dla sieci VPN punkt lokacja jest nieprzewidziany, ponieważ dane są przesyłane za pośrednictwem Internetu.
- **Sieć VPN lokacja lokacja:** między urządzenie sieci VPN i bramy sieci VPN platformy Azure. Ten typ połączenia pozwala dowolnego zasobu lokalnego zezwalają na dostęp do sieci wirtualnej. Połączenie jest IPSec i IKE sieci VPN, który zapewnia szyfrowaną komunikację przez Internet między urządzeniem lokalnymi i bramy sieci VPN platformy Azure. Czas oczekiwania na połączenie lokacja lokacja jest nieprzewidywalne, ponieważ dane są przesyłane za pośrednictwem Internetu.
- **Usługa Azure ExpressRoute:** ustanowić między siecią a Azure, za pośrednictwem partner usługi ExpressRoute. To połączenie jest prywatne. Ruch nie przechodzą przez Internet. Czas oczekiwania na połączenie ExpressRoute jest przewidywalne, ponieważ ruchu nie przechodzenie przez Internet.

Aby dowiedzieć się więcej na temat wszystkich poprzednich opcji połączenia, przeczytaj [Diagram topologii połączenia](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams) artykułu.

## <a name="filtering"></a>Filtrowanie ruchu sieciowego
Można filtrować ruch sieciowy między podsieciami przy użyciu jednego lub obu z następujących opcji:
- **Sieciowe grupy zabezpieczeń (NSG):** każda grupa NSG może zawierać wiele reguł zabezpieczeń dla ruchu przychodzącego i wychodzącego umożliwiające filtrować ruch źródłowy i docelowy adres IP, portu i protokołu. Grupy NSG można zastosować do każdej karty Sieciowej na maszynie wirtualnej. Można także zastosować grupy NSG do podsieci karty Sieciowej, lub innych zasobów platformy Azure jest połączona. Aby dowiedzieć się więcej na temat grup NSG, przeczytaj [sieciowej grupy zabezpieczeń](virtual-networks-nsg.md) artykułu.
- **Sieci wirtualne urządzenia (NVA):** NVA jest uruchomione oprogramowanie, które wykonuje funkcję sieci, takie jak Zapora maszyny Wirtualnej. Wyświetlanie listy dostępnych NVAs w [portalu Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). NVAs są również dostępne, które zapewniają funkcje ruchu Optymalizacja sieci WAN i innych sieci. NVAs zwykle są używane przez użytkownika lub trasy protokołu BGP. NVA można również użyć do filtrowania ruchu między sieciami wirtualnymi.

## <a name="routing"></a>Kierować ruchem sieciowym

Platforma Azure tworzy tabele tras, które umożliwiają zasoby podłączone do żadnej podsieci w dowolnej sieci wirtualnej do komunikowania się ze sobą, domyślnie. Można zaimplementować jedną lub obie z poniższych opcji, aby zastąpić trasy domyślne, które tworzy Azure:
- **Trasy zdefiniowane przez użytkownika:** tworzenia tabel tras niestandardowych trasy tego formantu, których ruch jest kierowany do dla każdej podsieci. Aby dowiedzieć się więcej o trasach definiowanych przez użytkownika, przeczytaj artykuł [Trasy definiowane przez użytkownika](virtual-networks-udr-overview.md).
- **Trasy protokołu BGP:** połączenie Twojej sieci wirtualnej sieci lokalnej przy użyciu połączenia bramy sieci VPN platformy Azure lub usługi ExpressRoute, podczas rozpowszechniania tras BGP do Twojej sieci wirtualnych.

## <a name="pricing"></a>Cennik

Jest bezpłatna dla sieci wirtualnych, podsieci, tabele tras lub sieciowej grupy zabezpieczeń. Wychodzące użycia przepustowości połączenia internetowego, publicznego adresu IP adresów sieci wirtualnej komunikacji równorzędnej, bram sieci VPN i ExpressRoute każdego mają swoje własne cennik struktury. Widok [sieci wirtualnej](https://azure.microsoft.com/pricing/details/virtual-network), [bramy sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway), i [ExpressRoute](https://azure.microsoft.com/pricing/details/expressroute) cennik strony, aby uzyskać więcej informacji.

## <a name="faq"></a>Często zadawane pytania

Aby zapoznać się z często zadawane pytania dotyczące sieci wirtualnej, zobacz [często zadawane pytania dotyczące sieci wirtualnych](virtual-networks-faq.md) artykułu.


## <a name="next-steps"></a>Następne kroki

- Tworzenie pierwszej sieci wirtualnej i połączyć kilka maszyn wirtualnych, wykonując kroki opisane w [tworzenie pierwszej sieci wirtualnej](virtual-network-get-started-vnet-subnet.md) artykułu.
- Utwórz połączenie punkt lokacja sieci wirtualnej, wykonując kroki opisane w [skonfigurować połączenie punkt lokacja](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artykułu.
- Dowiedz się więcej o niektóre inne kluczowe [sieci możliwości](../networking/networking-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) platformy Azure.
