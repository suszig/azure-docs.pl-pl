<properties
   pageTitle="Omówienie użycia protokołu BGP z bramami sieci VPN na platformie Azure | Microsoft Azure"
   description="Ten artykuł zawiera omówienie użycia protokołu BGP z bramami sieci VPN na platformie Azure."
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor=""
   tags=""/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="06/16/2016"
   ms.author="yushwang"/>


# Omówienie użycia protokołu BGP z bramami sieci VPN na platformie Azure

W tym artykule omówiono obsługę protokołu BGP (Border Gateway Protocol) w bramach sieci VPN na platformie Azure.

## BGP — informacje

BGP to standardowy protokół routingu używany często w Internecie do wymiany informacji o routingu i osiągalności między dwiema lub wieloma sieciami. W przypadku jego użycia w kontekście sieci wirtualnych Azure, protokół BGP umożliwia bramom sieci VPN na platformie Azure i lokalnym urządzeniom sieci VPN (nazywanym elementami równorzędnymi lub sąsiednimi BGP), przeprowadzaną za pośrednictwem bram lub routerów wymianę „tras” zawierających przeznaczone dla obu bram informacje na temat dostępności i osiągalności tych prefiksów. Protokół BGP umożliwia również włączenie routingu tranzytowego między wieloma sieciami poprzez propagowanie tras, których brama BGP uczy się od jednego elementu równorzędnego BGP, we wszystkich innych elementach równorzędnych BGP.
 
### Dlaczego warto używać protokołu BGP?

Protokół BGP stanowi funkcję opcjonalną, której można używać z bramami sieci VPN na platformie Azure opartymi na trasie. Przed włączeniem funkcji należy również upewnić się, że lokalne urządzenia sieci VPN obsługują protokół BGP. Bram sieci VPN na platformie Azure i lokalnych urządzeń sieci VPN można używać także bez protokołu BGP. Odpowiada to użyciu tras statycznych (bez stosowania protokołu BGP) *w porównaniu do* użycia routingu dynamicznego z zastosowaniem protokołu BGP między sieciami i platformą Azure.

Protokół BGP oferuje kilka zalet i nowych możliwości:

#### Obsługa automatycznych i elastycznych aktualizacji prefiksów

W przypadku użycia protokołu BGP wystarczy zadeklarować 	prefiks o minimalnej długości dla określonego elementu równorzędnego BGP za pośrednictwem tunelu VPN S2S wykorzystującego protokół IPsec. Może to być nawet sam prefiks hosta (/32) adresu IP elementu równorzędnego BGP dla lokalnego urządzenia sieci VPN. Można określić, które prefiksy sieci lokalnej mają być ogłaszane na platformie Azure w celu umożliwienia dostępu sieci wirtualnej Azure.
    
Można również ogłaszać dłuższe prefiksy, które mogą obejmować niektóre spośród prefiksów adresu sieci wirtualnej użytkownika, takie jak duża przestrzeń prywatnych adresów IP (np. 10.0.0.0/8). Należy pamiętać, że prefiksy nie mogą być takie same, jak prefiksy w sieci wirtualnej użytkownika. Trasy identyczne z prefiksami w sieci wirtualnej zostaną odrzucone.

>[AZURE.IMPORTANT] Obecnie anonsowanie trasy domyślnej (0.0.0.0/0) do bram sieci VPN platformy Azure będzie blokowane. Po włączeniu tej możliwości zostaną udostępnione dalsze aktualizacje.

#### Obsługa wielu tuneli między siecią wirtualną i lokacją lokalną z funkcją automatycznego trybu failover w oparciu o protokół BGP

Można utworzyć wiele połączeń między swoją siecią wirtualną na platformie Azure i lokalnymi urządzeniami sieci VPN w tej samej lokalizacji. Ta funkcja pozwala korzystać z wielu tuneli (ścieżek) między dwiema sieciami w konfiguracji aktywne/aktywne. Jeśli jeden z tuneli zostanie rozłączony, odpowiednie trasy zostaną wycofane za pośrednictwem protokołu BGP, ruch natomiast zostanie automatycznie przesunięty do pozostałych tuneli.
    
Na poniższym diagramie przedstawiono prosty przykład tej konfiguracji charakteryzującej się wysoką dostępnością:
    
![Wiele aktywnych ścieżek](./media/vpn-gateway-bgp-overview/multiple-active-tunnels.png)

#### Obsługa routingu tranzytowego między sieciami lokalnymi i wieloma sieciami wirtualnymi na platformie Azure

Protokół BGP umożliwia uzyskiwanie i propagowanie przez wiele bram prefiksów z różnych sieci, bez względu na to, czy są połączone bezpośrednio lub pośrednio. Umożliwia to realizowany przy użyciu bram sieci VPN na platformie Azure routing tranzytowy między lokalnymi lokacjami lub w obrębie wielu sieci wirtualnych Azure.
    
Na poniższym diagramie przedstawiono przykład topologii z wieloma przeskokami z wielu ścieżek, która umożliwia przesyłanie ruchu między dwiema sieciami lokalnymi za pośrednictwem bram sieci VPN na platformie Azure w ramach usługi Microsoft Networks:

![Przesyłanie z wieloma przeskokami](./media/vpn-gateway-bgp-overview/full-mesh-transit.png)

## Protokół BGP — często zadawane pytania


[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 




## Następne kroki

Opis procedury konfigurowania protokołu BGP pod kątem połączeń obejmujących wiele lokalizacji i połączeń między sieciami wirtualnymi zawiera artykuł [Getting started with BGP on Azure VPN gateways](./vpn-gateway-bgp-resource-manager-ps.md) (Rozpoczęcie pracy z protokołem BGP dla bram sieci VPN na platformie Azure).




<!--HONumber=Sep16_HO3-->


