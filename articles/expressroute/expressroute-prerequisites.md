<properties
   pageTitle="Wymagania wstępne dotyczące wdrożenia usługi ExpressRoute | Microsoft Azure"
   description="Ta strona zawiera listę wymagań, które należy spełnić przed zamówieniem obwodu usługi Azure ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/19/2016"
   ms.author="cherylmc"/>


# Wymagania wstępne usługi ExpressRoute i lista kontrolna  

Aby połączyć się z usługami w chmurze firmy Microsoft za pomocą usługi ExpressRoute, należy sprawdzić, czy zostały spełnione wymagania wymienione w poniższych sekcjach.

## Konto platformy Azure

- Prawidłowe i aktywne konto platformy Microsoft Azure. Jest ono wymagane do instalacji obwodu usługi ExpressRoute. Obwody usługi ExpressRoute to zasoby w ramach subskrypcji platformy Azure. Subskrypcja platformy Azure jest wymagana, nawet jeśli łączność jest ograniczona do usług w chmurze firmy Microsoft poza platformą Azure, takich jak usługi Office 365 i CRM online.
- Aktywna subskrypcja usługi Office 365 (w przypadku korzystania z usług Office 365). Więcej informacji znajduje się w sekcji w tym artykule dotyczącej [określonych wymagań dotyczących usługi Office 365](#office-365-specific-requirements).

## Dostawca połączenia
- W celu połączenia z chmurą firmy Microsoft można współpracować z [partnerem połączenia usługi ExpressRoute](expressroute-locations.md#partners). Połączenie między siecią lokalną a firmą Microsoft można skonfigurować na [trzy sposoby](expressroute-introduction.md#howtoconnect). 
- Jeśli dostawca nie jest partnerem połączenia usługi ExpressRoute, można nadal połączyć się z chmurą firmy Microsoft za pośrednictwem [dostawcy serwera Exchange w chmurze](expressroute-locations.md#nonpartners).

## Wymagania dotyczące sieci
- **Łączność nadmiarowa**: nie ma wymagania nadmiarowości względem fizycznej łączności między użytkownikiem a dostawcą. Firma Microsoft nie wymaga konfigurowania nadmiarowych sesji protokołu BGP między routerami firmy Microsoft i routerami komunikami równorzędnej, nawet jeśli istnieje tylko [jedno fizyczne połączenie z serwerem Exchange w chmurze](expressroute-faqs.md#onep2plink). 
- **Routing**: w zależności od sposobu połączenia z chmurą firmy Microsoft użytkownik lub jego dostawca musi skonfigurować sesje protokołu BGP oraz nimi zarządzać na potrzeby [domen routingu](expressroute-circuit-peerings.md). Niektórzy dostawcy połączenia Ethernet lub dostawcy usług serwera Exchange w chmurze mogą oferować zarządzanie przy użyciu protokołu BGP w ramach usługi dodatkowej.
- **Translator adresów sieciowych**: firma Microsoft akceptuje tylko publiczne adresy IP za pośrednictwem komunikacji równorzędnej firmy Microsoft. W przypadku korzystania z prywatnych adresów IP w sieci lokalnej użytkownik lub jego dostawca muszą przełożyć prywatne adresy IP na publiczne [przy użyciu translatora adresów sieciowych](expressroute-nat.md).
- **Technologia QoS**: program Skype dla firm oferuje różne usługi (np. połączenia głosowe, wideo, usługi tekstowe), które wymagają zróżnicowanej obsługi w technologii QoS. Użytkownik i jego dostawca powinni postępować zgodnie z [wymaganiami technologii QoS](expressroute-qos.md).
- **Bezpieczeństwo sieci**: podczas łączenia z usługą Microsoft Cloud za pośrednictwem usługi ExpressRoute należy mieć na uwadze [bezpieczeństwo sieci](../best-practices-network-security.md).
 
## Office 365

Jeśli zamierzasz włączyć usługę Office 365 w ramach usługi ExpressRoute, przejrzyj następujące dokumenty, by uzyskać więcej informacji o wymaganiach dotyczących usługi Office 365.


- [Omówienie usługi ExpressRoute dla usługi Office 365](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Routing za pomocą usługi ExpressRoute dla usługi Office 365](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Zakresy adresów URL i IP dla usługi Office 365](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [Planowanie sieci i dostrajanie wydajności dla usługi Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [Narzędzia i kalkulatory przepustowości sieci](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Integracja usługi Office 365 ze środowiskiem lokalnym](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## CRM Online 
Jeśli zamierzasz włączyć usługę CRM Online w ramach usługi ExpressRoute, przejrzyj następujące dokumenty, by uzyskać więcej informacji o usłudze CRM Online.

- [Zakresy adresów URL i IP](https://support.microsoft.com/kb/2655102) [usługi CRM Online](https://support.microsoft.com/kb/2728473)

## Następne kroki

- Więcej informacji na temat usługi ExpressRoute znajduje się w artykule [ExpressRoute FAQ](expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).
- Znajdź dostawcę połączenia usługi ExpressRoute. Zobacz artykuł [ExpressRoute partners and peering locations](expressroute-locations.md) (Partnerzy i lokalizacje komunikacji równorzędnej usługi ExpressRoute).
- Zapoznaj się z wymaganiami dotyczącymi [routingu](expressroute-routing.md), [translatora adresów sieciowych](expressroute-nat.md) i [technologii QoS](expressroute-qos.md).
- Skonfiguruj połączenie usługi ExpressRoute.
    - [Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)](expressroute-howto-circuit-classic.md)
    - [Configure routing (Konfigurowanie routingu)](expressroute-howto-routing-classic.md)
    - [Link a VNet to an ExpressRoute circuit (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute)](expressroute-howto-linkvnet-classic.md)




<!--HONumber=jun16_HO2-->


