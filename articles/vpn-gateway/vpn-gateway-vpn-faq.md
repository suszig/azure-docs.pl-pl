---
title: "Brama sieci VPN sieci wirtualnej — często zadawane pytania | Microsoft Docs"
description: "Brama sieci VPN — często zadawane pytania Często zadawane pytania dotyczące połączeń obejmujących wiele lokalizacji, połączeń w konfiguracji hybrydowej oraz bram sieci VPN usługi Microsoft Azure Virtual Network"
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: 
ms.assetid: 6ce36765-250e-444b-bfc7-5f9ec7ce0742
ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2016
ms.author: yushwang
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: e7d0fa43001268fc4747bbf40d3dc209aa037a67


---
# <a name="vpn-gateway-faq"></a>Brama sieci VPN — często zadawane pytania
## <a name="connecting-to-virtual-networks"></a>Łączenie z sieciami wirtualnymi
### <a name="can-i-connect-virtual-networks-in-different-azure-regions"></a>Czy można połączyć sieci wirtualne z różnych regionów świadczenia usługi Azure?
Tak. Nie ma żadnych ograniczeń dotyczących regionów. Jedna sieć wirtualna może nawiązać połączenie z inną siecią wirtualną w tym samym lub w innym regionie świadczenia usługi Azure. 

### <a name="can-i-connect-virtual-networks-in-different-subscriptions"></a>Czy można połączyć sieci wirtualne należące do różnych subskrypcji?
Tak.

### <a name="can-i-connect-to-multiple-sites-from-a-single-virtual-network"></a>Czy można łączyć się z wieloma lokalizacjami z jednej sieci wirtualnej?
Można nawiązać połączenie z wieloma lokalizacjami za pomocą programu Windows PowerShell oraz interfejsów API REST Azure. Zobacz sekcję często zadawanych pytań dotyczących [połączeń obejmujących wiele lokalizacji i połączeń między sieciami wirtualnymi](#multi-site-and-vnet-to-vnet-connectivity).

## <a name="what-are-my-cross-premises-connection-options"></a>Jakie są dostępne możliwości połączeń obejmujących wiele lokalizacji?
Obsługiwane są następujące połączenia obejmujące wiele lokalizacji:

* [Lokacja-lokacja](vpn-gateway-site-to-site-create.md) — połączenie sieci VPN nawiązywane za pośrednictwem protokołu IPsec (IKE v1 i IKE v2). Ten typ połączenia wymaga urządzenia VPN lub usługi RRAS.
* [Punkt-lokacja](vpn-gateway-point-to-site-create.md) — połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). To połączenie nie wymaga urządzenia VPN.
* [Sieć wirtualna-sieć wirtualna](virtual-networks-configure-vnet-to-vnet-connection.md) — tego typu połączenie odpowiada konfiguracji typu lokacja-lokacja. Połączenie typu sieć wirtualna-sieć wirtualna to połączenie sieci VPN nawiązywane za pośrednictwem protokołu IPsec (IKE v1 i IKE v2). To połączenie nie wymaga urządzenia VPN.
* [Obejmujące wiele lokalizacji](vpn-gateway-multi-site.md) — konfiguracja typu lokacja-lokacja, która pozwala połączyć z siecią wirtualną wiele lokalizacji lokalnych.
* [ExpressRoute](../expressroute/expressroute-introduction.md) — ExpressRoute to bezpośrednie połączenie z siecią Azure nawiązane z poziomu sieci WAN, a nie z publicznego Internetu. Więcej informacji zawierają tematy [ExpressRoute Technical Overview](../expressroute/expressroute-introduction.md) (Opis techniczny ExpressRoute) i [ExpressRoute FAQ](../expressroute/expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).

Więcej informacji na temat połączeń można znaleźć w artykule [About VPN Gateway](vpn-gateway-about-vpngateways.md) (Informacje o bramie VPN Gateway).

### <a name="what-is-the-difference-between-a-site-to-site-connection-and-point-to-site"></a>Jaka jest różnica między połączeniem typu lokacja-lokacja i połączeniem typu punkt-lokacja?
Typ połączenia **lokacja-lokacja** umożliwia połączenie dowolnych lokalnych komputerów z dowolną maszyną wirtualną lub wystąpieniem roli w ramach sieci wirtualnej, w zależności od wybranej konfiguracji routingu. To rozwiązanie doskonale sprawdza się w przypadku zawsze dostępnych połączeń obejmujących wiele lokalizacji; jest to także dobry wybór w przypadku konfiguracji hybrydowych. Ten typ połączenia jest oparty na urządzeniu VPN (sprzętowym lub programowym) z protokołem IPsec, które musi zostać wdrożone na granicy sieci. W celu utworzenia tego typu połączenia niezbędne jest posiadanie wymaganego sprzętu VPN oraz adresu IPv4 dostępnego z zewnątrz.

Połączenia **punkt-lokacja** umożliwiają połączenie się z pojedynczego komputera znajdującego się w dowolnym miejscu z innym dowolnym miejscem w sieci wirtualnej. Korzystają z wewnętrznego klienta VPN systemu Windows. W ramach konfiguracji typu punkt-lokacja należy zainstalować certyfikat i pakiet konfiguracji klienta VPN, który zawiera ustawienia umożliwiające komputerowi połączenie się z dowolną maszyną wirtualną lub wystąpieniem roli w ramach sieci wirtualnej. Doskonale sprawdzają się podczas nawiązywania połączenia z siecią wirtualną spoza obszaru organizacji. To także dobra opcja w sytuacji braku dostępu do sprzętu VPN lub adresu IPv4 dostępnego z zewnątrz, które są wymagane w przypadku połączeń typu lokacja-lokacja.

Sieć wirtualną można skonfigurować pod kątem równoległego użycia metod lokacja-lokacja i punkt-lokacja, pod warunkiem, że połączenie lokacja-lokacja zostanie utworzone z użyciem bramy sieci VPN opartej na trasach. Typy bramy sieci VPN oparte na trasach są w klasycznym modelu wdrażania nazywane bramami dynamicznymi.

### <a name="what-is-expressroute"></a>Co to jest ExpressRoute?
Usługa ExpressRoute umożliwia tworzenie prywatnych połączeń między centrami danych firmy Microsoft i infrastrukturą lokalną lub działającą we wspólnej lokalizacji. Dzięki usłudze ExpressRoute można nawiązywać połączenia z usługami w chmurze firmy Microsoft, takimi jak Microsoft Azure i Office 365, we wspólnej lokalizacji partnera ExpressRoute lub połączyć się bezpośrednio z istniejącej sieci WAN (takiej jak MPLS VPN udostępnianej przez dostawcę usług sieciowych).

Połączenia ExpressRoute zapewniają większe bezpieczeństwo, niezawodność i przepustowość oraz niższe opóźnienia niż w typowe połączenia przez Internet. W niektórych przypadkach użycie połączeń ExpressRoute do celów transferu danych między siecią lokalną i usługą Azure może również przynieść znaczne korzyści finansowe. Po utworzeniu obejmującego wiele lokalizacji połączenia między siecią lokalną i usługą Azure można dokonać migracji do połączenia ExpressRoute przy jednoczesnym zachowaniu sieci wirtualnej bez jakichkolwiek zmian.

Szczegółowe informacje znajdują się w artykule [ExpressRoute FAQ](../expressroute/expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).

## <a name="site-to-site-connections-and-vpn-devices"></a>Połączenia typu lokacja-lokacja a urządzenia sieci VPN
### <a name="what-should-i-consider-when-selecting-a-vpn-device"></a>Co należy wziąć pod uwagę przy wyborze urządzenia sieci VPN?
Zweryfikowaliśmy we współpracy z dostawcami urządzeń szereg standardowych urządzeń sieci VPN wykorzystywanych w ramach połączeń typu lokacja-lokacja. Listę znanych zgodnych urządzeń sieci VPN, odpowiednie instrukcje konfiguracji lub przykłady oraz dane techniczne urządzeń można znaleźć [tutaj](vpn-gateway-about-vpn-devices.md). Wszystkie urządzenia z rodzin wymienionych jako rodziny o znanej zgodności powinny współpracować z siecią wirtualną. Aby skonfigurować urządzenie sieci VPN, zapoznaj się z przykładową konfiguracją urządzenia lub łączem odpowiadającym odpowiedniej rodzinie urządzeń.

### <a name="what-do-i-do-if-i-have-a-vpn-device-that-isnt-in-the-known-compatible-device-list"></a>Co zrobić, jeśli posiadane urządzenie sieci VPN nie zostało ujęte na liście znanych zgodnych urządzeń?
Jeśli urządzenie, którego chcesz użyć w celu nawiązania połączenia sieci VPN, nie znajduje się na liście znanych zgodnych urządzeń sieci VPN, sprawdź, czy jest ono zgodne z obsługiwanymi opcjami konfiguracji protokołu IPsec/IKE i parametrami wymienionymi [tutaj](vpn-gateway-about-vpn-devices.md). Urządzenia spełniające minimalne wymagania powinny działać z bramami sieci VPN. Dodatkową pomoc oraz instrukcje dotyczące konfiguracji można uzyskać, kontaktując się z producentem urządzenia.

### <a name="why-does-my-policy-based-vpn-tunnel-go-down-when-traffic-is-idle"></a>Dlaczego mój oparty na zasadach tunel VPN przestaje działać, gdy ruch jest w stanie bezczynności?
Jest to oczekiwane zachowanie bram sieci VPN opartych na zasadach (znanych także jako bramy o routingu statycznym). Jeśli ruch za pośrednictwem tunelu będzie pozostawał w stanie bezczynności przez więcej niż 5 minut, nastąpi jego usunięcie. Natychmiast po rozpoczęciu ruchu danych w dowolnym kierunku będzie można ponownie ustanowić tunel.

### <a name="can-i-use-software-vpns-to-connect-to-azure"></a>Czy można użyć oprogramowania pełniącego rolę sieci VPN do nawiązania połączenia z usługą Azure?
W ramach konfiguracji typu lokacja-lokacja obejmującej wiele lokalizacji obsługiwane są serwery Windows Server 2012 usługi Routing i dostęp zdalny (RRAS).

Inne rozwiązania o charakterze oprogramowania pełniącego rolę sieci VPN powinny współpracować z bramą, o ile są one zgodne ze standardowymi implementacjami protokołu IPsec stosowanymi w branży. W celu uzyskania pomocy technicznej oraz instrukcji konfiguracji należy skontaktować się z dostawcą oprogramowania.

## <a name="point-to-site-connections"></a>Połączenia typu punkt-lokacja
### <a name="what-operating-systems-can-i-use-with-point-to-site"></a>Których systemów operacyjnych można używać z połączeniami typu punkt-lokacja?
Obsługiwane są następujące systemy operacyjne:

* Windows 7 (32-bitowy i 64-bitowy)
* Windows Server 2008 R2 (tylko 64-bitowy)
* Windows 8 (32-bitowy i 64-bitowy)
* Windows 8.1 (32-bitowy i 64-bitowy)
* Windows Server 2012 (tylko 64-bitowy)
* Windows Server 2012 R2 (tylko 64-bitowy)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Czy można użyć dowolnego oprogramowania klienta VPN do połączenia punkt-lokacja obsługującego protokół SSTP?
Nie. Obsługa jest ograniczona tylko do wersji systemu operacyjnego Windows wymienionych powyżej.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Ile punktów końcowych klienta sieci VPN może obejmować konfiguracja punkt-lokacja?
Obsługiwanych jest maksymalnie 128 klientów VPN, które mogą jednocześnie nawiązać połączenie z siecią wirtualną.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Czy w przypadku połączenia punkt-lokacja można użyć własnego głównego urzędu certyfikacji PKI?
Tak. Wcześniej można było używać tylko certyfikatów głównych z podpisem własnym. Nadal można przesłać 20 certyfikatów głównych.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Czy można pominąć serwery proxy i zapory, korzystając z funkcji punkt-lokacja?
Tak. Do celów tunelowania przez zaporę jest wykorzystywany protokół SSTP (Secure Socket Tunneling Protocol). Ten tunel zostanie wyświetlony jako połączenie HTTPs.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Czy w przypadku ponownego uruchomienia komputera klienckiego skonfigurowanego pod kątem połączenia typu punkt-lokacja połączenie z siecią VPN zostanie nawiązane automatycznie?
Domyślnie komputer kliencki nie przywraca automatycznie połączenia z siecią VPN.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Czy w przypadku połączeń punkt-lokacja jest obsługiwane automatyczne ponowne nawiązywanie połączenia i DDNS na klientach sieci VPN?
Automatyczne ponowne nawiązywanie połączenia i DDNS nie są obecnie obsługiwane w przypadku połączeń VPN typu punkt-lokacja.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Czy z konfiguracji typu lokacja-lokacja i punkt-lokacja można korzystać równolegle w ramach tej samej sieci wirtualnej?
Tak. Oba te rozwiązania będą działać, o ile zastosowana zostanie brama sieci VPN typu RouteBased. W przypadku klasycznego modelu wdrażania należy użyć bramy dynamicznej. Połączenia typu punkt-lokacja nie są obsługiwane w przypadku bram sieci VPN o statycznym routingu ani bram, których wartość parametru -VpnType to PolicyBased.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Czy można skonfigurować klienta typu punkt-lokacja pod kątem jednoczesnego nawiązywania połączenia z wieloma sieciami wirtualnymi?
Tak, jest to możliwe. Sieci wirtualne nie mogą jednak mieć nakładających się prefiksów IP, a przestrzenie adresowe w przypadku połączenia punkt-lokacja nie mogą nakładać się między sieciami wirtualnymi.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Jakiej przepustowości można oczekiwać w przypadku połączeń typu lokacja-lokacja lub punkt-lokacja?
Trudno jest utrzymać dokładną przepływność tuneli VPN. Protokoły IPsec i SSTP należą do niejawnie ciężkich protokołów sieci VPN. Przepływność ograniczają również opóźnienia i przepustowość między lokalizacjami lokalnymi i Internetem.

## <a name="gateways"></a>Bramy
### <a name="what-is-a-policy-based-static-routing-gateway"></a>Co to jest brama oparta na zasadach (o routingu statycznym)?
Bramy oparte na zasadach wdrażają sieci VPN oparte na zasadach. Sieci VPN oparte na zasadach szyfrują pakiety i kierują je bezpośrednio przez tunele protokołu IPsec na podstawie kombinacji prefiksów adresów między siecią lokalną użytkownika i siecią wirtualną platformy Azure. Zasady (lub selektor ruchu) są zazwyczaj zdefiniowane jako lista dostępu w konfiguracji VPN.

### <a name="what-is-a-route-based-dynamic-routing-gateway"></a>Co to jest brama oparta na trasach (o routingu dynamicznym)?
Bramy oparte na trasach wdrażają sieci VPN oparte na trasach. Sieci VPN oparte na trasach używają „tras” w funkcji przesyłania dalej IP lub tabeli routingu do kierowania pakietów do odpowiednich interfejsów tuneli. W dalszej kolejności interfejsy tuneli szyfrują lub odszyfrowują pakiety wchodzące do tuneli lub wychodzące z nich. Zasady lub selektor ruchu dla sieci VPN opartych na trasach są skonfigurowane jako każdy z każdym (lub symbole wieloznaczne).

### <a name="can-i-get-my-vpn-gateway-ip-address-before-i-create-it"></a>Czy można użyć adresu IP bramy sieci VPN przed jej utworzeniem?
Nie. Aby uzyskać adres IP bramy, należy ją najpierw utworzyć. Adres IP ulegnie zmianie, jeśli brama VPN zostanie usunięta i ponownie utworzona.

### <a name="how-does-my-vpn-tunnel-get-authenticated"></a>W jaki sposób następuje uwierzytelnienie tunelu VPN?
Sieć VPN platformy Azure używa uwierzytelniania PSK (klucza wstępnego). Podczas tworzenia tunelu VPN generowany jest klucz wstępny (PSK). Wygenerowany automatycznie klucz wstępny można zmienić na własny klucz przy użyciu polecenia cmdlet Set Pre-Shared Key lub interfejsu API REST programu PowerShell.

### <a name="can-i-use-the-set-pre-shared-key-api-to-configure-my-policy-based-static-routing-gateway-vpn"></a>Czy za pomocą interfejsu API i polecenia cmdlet Set Pre-Shared Key można skonfigurować własną bramę sieci VPN opartą na zasadach (o routingu statycznym)?
Tak, interfejs API i polecenie cmdlet Set Pre-Shared Key programu PowerShell pozwalają konfigurować sieci wirtualne platformy Azure oparte na zasadach (o statycznym routingu) i na trasach (o dynamicznym routingu).

### <a name="can-i-use-other-authentication-options"></a>Czy są dostępne inne opcje uwierzytelniania?
Metody uwierzytelniania ograniczają się do stosowania kluczy wstępnych (PSK).

### <a name="what-is-the-gateway-subnet-and-why-is-it-needed"></a>Co to jest „podsieć bramy” i do czego jest ona potrzebna?
Stosujemy usługę bramy, której działanie umożliwia nawiązywanie połączeń obejmujących wiele lokalizacji.

Aby skonfigurować bramę sieci VPN, należy utworzyć podsieć bramy dla sieci wirtualnej. Aby podsieć bramy działała prawidłowo, musi nosić nazwę GatewaySubnet. Nie należy nadawać podsieci bramy innej nazwy. Nie należy także wdrażać maszyn wirtualnych ani innych elementów w ramach podsieci bramy.

Minimalny rozmiar podsieci bramy zależy tylko od konfiguracji, którą chcesz utworzyć. Chociaż w przypadku niektórych konfiguracji możesz utworzyć małą podsieć bramy o rozmiarze /29, zaleca się tworzenie podsieci bramy /28 i większych (/28, /27, /26 itp.).

### <a name="can-i-deploy-virtual-machines-or-role-instances-to-my-gateway-subnet"></a>Czy można wdrożyć maszyny wirtualne lub wystąpienia roli w ramach podsieci bramy?
Nie.

### <a name="how-do-i-specify-which-traffic-goes-through-the-vpn-gateway"></a>Jak określić, który ruch danych przechodzi przez bramę sieci VPN?
W przypadku użycia klasycznej witryny Azure Portal należy dodać każdy z zakresów, który ma zostać wysłany za pośrednictwem bramy sieci wirtualnej, na stronie Sieci w sekcji Sieci lokalne.

### <a name="can-i-configure-forced-tunneling"></a>Czy można skonfigurować wymuszone tunelowanie?
Tak. Zobacz artykuł [Configure forced tunneling](vpn-gateway-about-forced-tunneling.md) (Konfiguracja wymuszonego tunelowania).

### <a name="can-i-set-up-my-own-vpn-server-in-azure-and-use-it-to-connect-to-my-on-premises-network"></a>Czy można wdrożyć własny serwer sieci VPN na platformie Azure i używać go do nawiązywania połączenia z siecią lokalną?
Tak, można wdrożyć własne bramy lub serwery sieci VPN na platformie Azure, korzystając z portalu Azure Marketplace lub tworząc własne routery sieci VPN. Należy skonfigurować w sieci wirtualnej trasy zdefiniowane przez użytkownika i upewnić się, że ruch jest prawidłowo przekierowywany między sieciami lokalnymi i podsieciami sieci wirtualnej.

### <a name="why-are-certain-ports-opened-on-my-vpn-gateway"></a>Dlaczego niektóre porty mojej bramy sieci VPN są otwarte?
Są one wymagane do komunikacji infrastruktury platformy Azure. Są one zabezpieczone (zablokowane) z użyciem certyfikatów Azure. Bez prawidłowych certyfikatów podmioty zewnętrzne, w tym klienci tych bram, nie będą mieć możliwości wywarcia jakiegokolwiek wpływu na te punkty końcowe.

Brama sieci VPN to zasadniczo wieloadresowe urządzenie, w przypadku którego jedna karta interfejsu sieciowego korzysta z sieci prywatnej klienta, a do drugiej można uzyskać dostęp z sieci publicznej. Podmioty infrastruktury platformy Azure nie mogą korzystać z sieci prywatnych klientów z przyczyn związanych z zachowaniem zgodności, w związku z czym muszą korzystać z publicznych punktów końcowych do komunikacji w ramach infrastruktury. Publiczne punkty końcowe są okresowo skanowane w ramach inspekcji zabezpieczeń platformy Azure.

### <a name="more-information-about-gateway-types-requirements-and-throughput"></a>Więcej informacji na temat typów, wymagań i przepustowości bram
Aby uzyskać więcej informacji, zobacz temat [About VPN Gateway Settings](vpn-gateway-about-vpn-gateway-settings.md) (Informacje o ustawieniach bramy VPN Gateway).

## <a name="multi-site-and-vnet-to-vnet-connectivity"></a>Połączenia obejmujące wiele lokacji i połączenia między sieciami wirtualnymi
### <a name="which-type-of-gateways-can-support-multi-site-and-vnet-to-vnet-connectivity"></a>Jakiego typu bramy mogą obsługiwać połączenia obejmujące wiele lokacji i połączenia między sieciami wirtualnymi?
Tylko bramy oparte na trasach (o routingu dynamicznym).

### <a name="can-i-connect-a-vnet-with-a-routebased-vpn-type-to-another-vnet-with-a-policybased-vpn-type"></a>Czy można połączyć sieć wirtualną VPN opartą na trasach z inną siecią wirtualną VPN opartą na zasadach?
Nie, obie sieci wirtualne MUSZĄ korzystać z sieci VPN opartych na trasach (o routingu dynamicznym).

### <a name="is-the-vnet-to-vnet-traffic-secure"></a>Czy ruch sieciowy w ramach połączenia między sieciami wirtualnymi jest bezpieczny?
Tak, jest zabezpieczony z użyciem szyfrowania IPsec/IKE.

### <a name="does-vnet-to-vnet-traffic-travel-over-the-azure-backbone"></a>Czy ruch sieciowy w ramach połączenia między sieciami wirtualnymi przechodzi przez sieć szkieletową platformy Azure?
Tak.

### <a name="how-many-on-premises-sites-and-virtual-networks-can-one-virtual-network-connect-to"></a>Z iloma lokacjami lokalnymi i sieciami wirtualnymi może połączyć się jedna sieć wirtualna?
Maksymalnie z 10 w przypadku podstawowych i standardowych bram o routingu dynamicznym; 30 w przypadku bram sieci VPN o wysokiej wydajności.

### <a name="can-i-use-point-to-site-vpns-with-my-virtual-network-with-multiple-vpn-tunnels"></a>Czy można używać sieci VPN typu punkt-lokacja z siecią wirtualną z wieloma tunelami VPN?
Tak, sieci VPN typu punkt-lokacja (P2S) można używać z bramami sieci VPN łączącymi się z wieloma lokacjami lokalnymi i innymi sieciami wirtualnymi.

### <a name="can-i-configure-multiple-tunnels-between-my-virtual-network-and-my-on-premises-site-using-multi-site-vpn"></a>Czy można skonfigurować wiele tuneli między siecią wirtualną i lokalną lokacją z użyciem sieci VPN obejmującej wiele lokalizacji?
Nie, nadmiarowe tunele między siecią wirtualną platformy Azure i lokacją lokalną nie są obsługiwane.

### <a name="can-there-be-overlapping-address-spaces-among-the-connected-virtual-networks-and-on-premises-local-sites"></a>Czy wśród połączonych sieci wirtualnych i lokacji lokalnych mogą występować nakładające się przestrzenie adresowe?
Nie. Nakładające się przestrzenie adresowe spowodują, że przesłanie pliku konfiguracyjnego sieci lub „tworzenie sieci wirtualnej” nie powiedzie się.

### <a name="do-i-get-more-bandwidth-with-more-site-to-site-vpns-than-for-a-single-virtual-network"></a>Czy przy większej liczbie połączeń VPN typu lokacja-lokacja można uzyskać większą przepustowość niż przy pojedynczej sieci wirtualnej?
Nie, wszystkie tunele VPN, w tym połączenia VPN typu punkt-lokacja, współdzielą tę samą bramę sieci VPN platformy Azure i dostępną przepustowość.

### <a name="can-i-use-azure-vpn-gateway-to-transit-traffic-between-my-on-premises-sites-or-to-another-virtual-network"></a>Czy można używać bramy Azure VPN do przekazywania ruchu między lokacjami lokalnymi lub do innej sieci wirtualnej?
**Klasyczny model wdrażania**<br>
Przekazywanie ruchu za pośrednictwem bramy sieci VPN platformy Azure przy użyciu klasycznego modelu wdrażania jest możliwe, ale zależy od statycznie zdefiniowanych przestrzeni adresowych w pliku konfiguracyjnym sieci. Protokół BGP dla klasycznego modelu wdrażania nie jest jeszcze obsługiwany w połączeniu z sieciami wirtualnymi platformy Azure i bramami sieci VPN. Niezastosowanie protokołu BGP powoduje bardzo wysokie ryzyko błędów ręcznego definiowania przestrzeni adresowych przesyłania i nie jest zalecane.<br>
**Model wdrażania usługi Resource Manager**<br>
Jeśli używasz modelu wdrażania usługi Resource Manager, zobacz sekcję [protokołu BGP](#bgp), aby uzyskać więcej informacji.

### <a name="does-azure-generate-the-same-ipsecike-pre-shared-key-for-all-my-vpn-connections-for-the-same-virtual-network"></a>Czy platforma Azure generuje taki sam klucz wstępny protokołu IPsec/IKE dla wszystkich połączeń sieci VPN dla danej sieci wirtualnej?
Nie. Platforma Azure domyślnie generuje różne klucze wstępne dla różnych połączeń sieci VPN. Można jednak użyć interfejsu API REST lub polecenia cmdlet Set VPN Gateway Key programu PowerShell, aby ustawić preferowaną wartość klucza. Klucz MUSI mieć postać alfanumerycznego ciągu o długości od 1 do 128 znaków.

### <a name="does-azure-charge-for-traffic-between-virtual-networks"></a>Czy w ramach platformy Azure są naliczane opłaty za ruch danych między sieciami wirtualnymi?
W przypadku ruchu między różnymi sieciami wirtualnymi Azure opłaty są naliczane tylko dla ruchu przekazywanego z jednego regionu świadczenia usługi Azure do innego. Cena została określona na stronie [VPN Gateway Pricing](https://azure.microsoft.com/pricing/details/vpn-gateway/) (Cennik usługi VPN Gateway) dotyczącej platformy Azure.

### <a name="can-i-connect-a-virtual-network-with-ipsec-vpns-to-my-expressroute-circuit"></a>Czy można połączyć sieć wirtualną z sieciami VPN wykorzystującymi protokół IPsec z obwodem ExpressRoute?
Tak, takie rozwiązanie jest obsługiwane. Aby uzyskać więcej informacji, zobacz artykuł [Configure ExpressRoute and Site-to-Site VPN connections that coexist](../expressroute/expressroute-howto-coexist-classic.md) (Konfigurowanie obwodu ExpressRoute i współistniejących połączeń sieci VPN typu lokacja-lokacja).

## <a name="a-namebgpabgp"></a><a name="bgp"></a>BGP
[!INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)]

## <a name="cross-premises-connectivity-and-vms"></a>Połączenia obejmujące wiele lokalizacji a maszyny wirtualne
### <a name="if-my-virtual-machine-is-in-a-virtual-network-and-i-have-a-cross-premises-connection-how-should-i-connect-to-the-vm"></a>W jaki sposób należy połączyć się z maszyną wirtualną w przypadku, gdy należy ona do sieci wirtualnej, a dysponuję połączeniem obejmującym wiele lokalizacji?
Jest kilka możliwości. W warunkach włączonego protokołu RDP, jeśli utworzono punkt końcowy, można połączyć się z maszyną wirtualną, korzystając z adresu VIP. W takim przypadku należy określić adres VIP i numer portu, z którym ma zostać nawiązane połączenie. Należy skonfigurować port maszyny wirtualnej pod kątem ruchu danych. W tym celu wystarczy zwykle przejść do klasycznej witryny Azure Portal i zapisać ustawienia połączenia RDP na komputerze. Ustawienia zawierają niezbędne informacje o połączeniu.

W przypadku sieci wirtualnej z funkcją łączności obejmującej wiele lokalizacji można połączyć się z maszyną wirtualną przy użyciu wewnętrznego adresu DIP lub prywatnego adresu IP. Z maszyną wirtualną można także nawiązać połączenie z poziomu innej maszyny wirtualnej, która znajduje się w tej samej sieci wirtualnej, korzystając z wewnętrznego adresu DIP. Nie można połączyć się z maszyną wirtualną z użyciem protokołu RDP i adresu DIP, jeśli połączenie jest nawiązywane z lokalizacji spoza sieci wirtualnej. Na przykład jeśli w przypadku dostępności skonfigurowanej sieci wirtualnej typu punkt-lokacja nie zostanie ustanowione połączenie z poziomu komputera, nie będzie możliwe połączenie się z maszyną wirtualną z użyciem adresu DIP.

### <a name="if-my-virtual-machine-is-in-a-virtual-network-with-cross-premises-connectivity-does-all-the-traffic-from-my-vm-go-through-that-connection"></a>Czy jeśli maszyna wirtualna należy do sieci wirtualnej z funkcją łączności obejmującą wiele lokalizacji, to cały ruch z niej będzie przekazywany za pośrednictwem tego połączenia?
Nie. Przez bramę sieci wirtualnej przechodzi tylko ruch, którego docelowy adres IP należy do określonych zakresów adresów IP sieci lokalnej w ramach sieci wirtualnej. Ruch, którego docelowy adres IP należy do sieci wirtualnej, pozostaje w obrębie sieci wirtualnej. Inny ruch jest wysyłany do sieci publicznej za pośrednictwem usługi równoważenia obciążenia lub, w przypadku użycia wymuszonego tunelowania, wysłany przez bramę sieci VPN platformy Azure. Podczas rozwiązywania problemów należy się upewnić, że w procesie uwzględniono wszystkie wymienione w obszarze Sieć lokalna zakresy, które mają zostać wysłane za pośrednictwem bramy. Należy się upewnić, że zakresy adresów sieci lokalnej nie nakładają się z żadnymi zakresami adresów w sieci wirtualnej. Ponadto należy sprawdzić, czy używany serwer DNS rozpoznaje nazwę jako właściwy adres IP.

## <a name="virtual-network-faq"></a>Sieć wirtualna — często zadawane pytania
Dodatkowe informacje dotyczące sieci wirtualnej można znaleźć w artykule [Virtual Network FAQ](../virtual-network/virtual-networks-faq.md) (Sieć wirtualna — często zadawane pytania).



<!--HONumber=Dec16_HO1-->


