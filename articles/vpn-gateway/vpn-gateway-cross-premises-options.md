<properties 
   pageTitle="Informacje o bezpiecznej łączności dla sieci wirtualnych obejmującej wiele lokalizacji | Microsoft Azure"
   description="Informacje o typach obejmujących wiele lokalizacji bezpiecznych połączeń dla sieci wirtualnych, w tym połączeń typu lokacja-lokacja, punkt-lokacja i ExpressRoute."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="cherylmc" />

# Informacje o bezpiecznej łączności dla sieci wirtualnych obejmującej wiele lokalizacji

W tym artykule omówiono różne sposoby nawiązywania połączenia z witryny lokalnej do sieci wirtualnej platformy Azure. Ten artykuł ma zastosowanie w obu modelach wdrażania — przy użyciu usługi Resource Manager i klasycznym. Jeśli szukasz diagramów połączeń bramy sieci VPN, zobacz artykuł [Azure VPN Gateway connection topolgies](vpn-gateway-topology.md) (Topologie połączeń bramy sieci VPN platformy Azure).

Dostępne są trzy typy połączeń: lokacja-lokacja, punkt-lokacja i ExpressRoute. Wybór opcji może zależeć od różnych czynników, np.:


- Jakiego rodzaju przepływności wymaga rozwiązanie?
- Czy chcesz komunikować się za pośrednictwem publicznej sieci Internet z użyciem bezpiecznego połączenia sieci VPN, czy też połączenia prywatnego?
- Czy dysponujesz publicznym adresem IP, którego możesz użyć?
- Czy planujesz użycie urządzenia sieci VPN? Jeśli tak, to czy jest ono zgodne?
- Czy zamierzasz połączyć tylko kilka komputerów, czy też chcesz utworzyć trwałe połączenie dla witryny?
- Jakiego rodzaju brama sieci VPN jest wymagana dla rozwiązania, które chcesz utworzyć?

W poniższej tabeli znajdują się informacje pomocne podczas podejmowania decyzji co do najlepszej opcji łączności dla rozwiązania.

[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]                                                                    

## Połączenia typu lokacja-lokacja

Połączenie sieci VPN typu lokacja-lokacja służy do utworzenia bezpiecznego połączenia między witryną lokalną i siecią wirtualną. Aby utworzyć połączenie typu lokacja-lokacja, urządzenie sieci VPN w sieci lokalnej jest konfigurowane pod kątem utworzenia bezpiecznego połączenia z usługą Azure VPN Gateway. Po utworzeniu połączenia zasoby sieci lokalnej i zasoby znajdujące się w sieci wirtualnej mogą bezpiecznie komunikować się w sposób bezpośredni. Połączenia lokacja-lokacja nie wymagają ustanowienia osobnego połączenia dla każdego komputera klienckiego w sieci lokalnej w celu uzyskania dostępu do zasobów w sieci wirtualnej.

**Z połączenia typu lokacja-lokacja należy skorzystać w następujących okolicznościach:**

- Chcesz utworzyć rozwiązanie hybrydowe.
- Chcesz korzystać z połączenia między witryną lokalną i siecią wirtualną bez konieczności konfiguracji po stronie klienta.
- Zależy Ci na trwałości połączenia. 

**Wymagania**

- Lokalne urządzenie sieci VPN musi mieć adres IPv4 dostępny z Internetu. Nie może on znajdować się za translatorem adresów sieciowych.
- Wymagane jest posiadanie zgodnego urządzenia VPN. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje). 
- Używane urządzenie sieci VPN musi być zgodne z typem bramy wymaganym w przypadku danego rozwiązania. Zobacz artykuł [About VPN Gateway](vpn-gateway-about-vpngateways.md) (Brama sieci VPN — informacje).
- Jednostka SKU bramy również będzie mieć wpływ na zagregowaną przepustowość. Zobacz sekcję [Gateway SKUs](vpn-gateway-about-vpngateways.md#gwsku) (Jednostki SKU bramy), aby uzyskać więcej informacji. 

**Dostępne modele wdrażania i metody nawiązywania połączeń typu lokacja-lokacja**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)] 


## Połączenia typu punkt-lokacja

Połączenie sieci VPN typu punkt-lokacja również umożliwia utworzenie bezpiecznego połączenia z siecią wirtualną. W konfiguracji punkt-lokacja połączenie zostaje ustawione osobno na każdym komputerze klienckim, który ma zostać połączony z siecią wirtualną. Połączenia typu punkt-lokacja nie wymagają urządzenia sieci VPN. Połączenia tego typu używają klientów sieci VPN, które są zainstalowane na każdym z komputerów klienckich. Sieć VPN jest tworzona poprzez ręczne zainicjowanie połączenia z lokalnego komputera klienckiego.

Konfiguracje punkt-lokacja i lokacja-lokacja mogą być używane równolegle, ale — w przeciwieństwie do połączeń lokacja-lokacja — połączenia punkt-lokacja nie mogą zostać skonfigurowane pod kątem ich równoczesnego użycia z połączeniami ExpressRoute z tą samą siecią wirtualną.

**Z połączenia typu punkt-lokacja należy skorzystać w następujących okolicznościach:**

- Chcesz skonfigurować tylko kilka klientów pod kątem połączenia z siecią wirtualną.

- Chcesz nawiązać połączenie z siecią wirtualną z lokalizacji zdalnej. Przykładem jest połączenie nawiązane z kawiarni lub sali konferencyjnej.

- Dysponujesz połączeniem lokacja-lokacja, jednak w przypadku niektórych z Twoich klientów wymagane jest nawiązanie połączenia z lokalizacji zdalnej.

- Nie masz dostępu do urządzenia sieci VPN, które spełnia minimalne wymagania dotyczące połączenia lokacja-lokacja.

- Nie masz dla lokalnego urządzenia sieci VPN adresu IPv4 dostępnego z Internetu.

**Dostępne modele wdrażania i metody nawiązywania połączeń typu punkt-lokacja**

[AZURE.INCLUDE [vpn-gateway-table-point-to-site](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## Połączenia ExpressRoute

Usługa ExpressRoute platformy Azure umożliwia tworzenie prywatnych połączeń między centrami danych Azure i infrastrukturą lokalną lub wspólnym środowiskiem. Połączenia ExpressRoute nie są realizowane za pośrednictwem publicznej sieci Internet i oferują większą niezawodność i szybkość oraz mniejsze opóźnienia i lepsze zabezpieczenia niż typowe połączenia przez Internet.

W niektórych przypadkach użycie połączeń ExpressRoute do celów transferu danych między siecią lokalną i usługą Azure może również przynieść znaczne korzyści finansowe. Dzięki usłudze ExpressRoute można nawiązywać połączenia z platformą Azure w lokalizacji ExpressRoute (infrastrukturze dostawcy usługi Exchange) lub połączyć się bezpośrednio z platformą Azure z istniejącej sieci WAN (takiej jak sieć VPN MPLS) udostępnionej przez dostawcę usług sieciowych.

Więcej informacji na temat połączeń ExpressRoute zawiera artykuł [Technical Overview](../expressroute/expressroute-introduction.md) (Opis techniczny).


## Następne kroki

- Aby uzyskać więcej informacji na temat bramy sieci VPN, zobacz artykuł [About VPN Gateway](vpn-gateway-about-vpngateways.md) (Brama sieci VPN — informacje), [często zadawane pytania](vpn-gateway-vpn-faq.md) dotyczące bramy sieci VPN oraz artykuł [Planning and Design](vpn-gateway-plan-design.md) (Planowanie i projektowanie).

- Więcej informacji na temat połączeń ExpressRoute zawiera artykuł [Technical Overview](../expressroute/expressroute-introduction.md) (Opis techniczny), [FAQ](../expressroute/expressroute-faqs.md) (Często zadawane pytania) oraz [Workflows](../expressroute/expressroute-workflows.md) (Przepływy pracy).







<!--HONumber=jun16_HO2-->


