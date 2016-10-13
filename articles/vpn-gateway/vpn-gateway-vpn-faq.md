<properties 
   pageTitle="Brama sieci VPN sieci wirtualnej — często zadawane pytania | Microsoft Azure"
   description="Brama sieci VPN — często zadawane pytania Często zadawane pytania dotyczące połączeń obejmujących wiele lokalizacji, połączeń w konfiguracji hybrydowej oraz bram sieci VPN usługi Microsoft Azure Virtual Network"
   services="vpn-gateway"
   documentationCenter="na"
   authors="yushwang"
   manager="rossort"
   editor="" />
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/10/2016"
   ms.author="yushwang" />


# Brama sieci VPN — często zadawane pytania

## Łączenie z sieciami wirtualnymi

### Czy można połączyć sieci wirtualne z różnych regionów świadczenia usługi Azure?
Tak. Nie ma żadnych ograniczeń dotyczących regionów. Jedna sieć wirtualna może nawiązać połączenie z inną siecią wirtualną w tym samym lub w innym regionie świadczenia usługi Azure.

### Czy można połączyć sieci wirtualne należące do różnych subskrypcji?
Tak.

### Czy można łączyć się z wieloma lokalizacjami z jednej sieci wirtualnej?

Można nawiązać połączenie z wieloma lokalizacjami za pomocą programu Windows PowerShell oraz interfejsów API REST Azure. Zobacz sekcję często zadawanych pytań dotyczących [połączeń obejmujących wiele lokalizacji i połączeń między sieciami wirtualnymi](#multi-site-and-vnet-to-vnet-connectivity).
## Jakie są dostępne możliwości połączeń obejmujących wiele lokalizacji?

Obsługiwane są następujące połączenia obejmujące wiele lokalizacji:

- [Lokacja-lokacja](vpn-gateway-site-to-site-create.md) — połączenie sieci VPN nawiązywane za pośrednictwem protokołu IPsec (IKE v1 i IKE v2). Ten typ połączenia wymaga urządzenia VPN lub usługi RRAS.

- [Punkt-lokacja](vpn-gateway-point-to-site-create.md) — połączenie sieci VPN nawiązywane za pośrednictwem protokołu SSTP (Secure Socket Tunneling Protocol). To połączenie nie wymaga urządzenia VPN.

- [Sieć wirtualna-sieć wirtualna](virtual-networks-configure-vnet-to-vnet-connection.md) — tego typu połączenie odpowiada konfiguracji typu lokacja-lokacja. Połączenie typu sieć wirtualna-sieć wirtualna to połączenie sieci VPN nawiązywane za pośrednictwem protokołu IPsec (IKE v1 i IKE v2). To połączenie nie wymaga urządzenia VPN.

- [Obejmujące wiele lokalizacji](vpn-gateway-multi-site.md) — konfiguracja typu lokacja-lokacja, która pozwala połączyć z siecią wirtualną wiele lokalizacji lokalnych.

- [ExpressRoute](../expressroute/expressroute-introduction.md) — ExpressRoute to bezpośrednie połączenie z siecią Azure nawiązane z poziomu sieci WAN, a nie z publicznego Internetu. Więcej informacji zawierają tematy [ExpressRoute Technical Overview](../expressroute/expressroute-introduction.md) (Opis techniczny ExpressRoute) i [ExpressRoute FAQ](../expressroute/expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).

Więcej informacji na temat połączeń można znaleźć w artykule [About VPN Gateway](vpn-gateway-about-vpngateways.md) (Informacje o bramie VPN Gateway).

### Jaka jest różnica między połączeniem typu lokacja-lokacja i połączeniem typu punkt-lokacja?

Typ połączenia **lokacja-lokacja** umożliwia połączenie dowolnych lokalnych komputerów z dowolną maszyną wirtualną lub wystąpieniem roli w ramach sieci wirtualnej, w zależności od wybranej konfiguracji routingu. To rozwiązanie doskonale sprawdza się w przypadku zawsze dostępnych połączeń obejmujących wiele lokalizacji; jest to także dobry wybór w przypadku konfiguracji hybrydowych. Ten typ połączenia jest oparty na urządzeniu VPN (sprzętowym lub programowym) z protokołem IPsec, które musi zostać wdrożone na granicy sieci. W celu utworzenia tego typu połączenia niezbędne jest posiadanie wymaganego sprzętu VPN oraz adresu IPv4 dostępnego z zewnątrz.

Połączenia **punkt-lokacja** umożliwiają połączenie się z pojedynczego komputera znajdującego się w dowolnym miejscu z innym dowolnym miejscem w sieci wirtualnej. Korzystają z wewnętrznego klienta VPN systemu Windows. W ramach konfiguracji typu punkt-lokacja należy zainstalować certyfikat i pakiet konfiguracji klienta VPN, który zawiera ustawienia umożliwiające komputerowi połączenie się z dowolną maszyną wirtualną lub wystąpieniem roli w ramach sieci wirtualnej. Doskonale sprawdzają się podczas nawiązywania połączenia z siecią wirtualną spoza obszaru organizacji. To także dobra opcja w sytuacji braku dostępu do sprzętu VPN lub adresu IPv4 dostępnego z zewnątrz, które są wymagane w przypadku połączeń typu lokacja-lokacja. 

Sieć wirtualną można skonfigurować pod kątem równoległego użycia metod lokacja-lokacja i punkt-lokacja, pod warunkiem, że połączenie lokacja-lokacja zostanie utworzone z użyciem bramy sieci VPN opartej na trasach. Typy bramy sieci VPN oparte na trasach są w klasycznym modelu wdrażania nazywane bramami dynamicznymi.

### Co to jest ExpressRoute?

Usługa ExpressRoute umożliwia tworzenie prywatnych połączeń między centrami danych firmy Microsoft i infrastrukturą lokalną lub działającą we wspólnej lokalizacji. Dzięki usłudze ExpressRoute można nawiązywać połączenia z usługami w chmurze firmy Microsoft, takimi jak Microsoft Azure i Office 365, we wspólnej lokalizacji partnera ExpressRoute lub połączyć się bezpośrednio z istniejącej sieci WAN (takiej jak MPLS VPN udostępnianej przez dostawcę usług sieciowych).

Połączenia ExpressRoute zapewniają większe bezpieczeństwo, niezawodność i przepustowość oraz niższe opóźnienia niż w typowe połączenia przez Internet. W niektórych przypadkach użycie połączeń ExpressRoute do celów transferu danych między siecią lokalną i usługą Azure może również przynieść znaczne korzyści finansowe. Po utworzeniu obejmującego wiele lokalizacji połączenia między siecią lokalną i usługą Azure można dokonać migracji do połączenia ExpressRoute przy jednoczesnym zachowaniu sieci wirtualnej bez jakichkolwiek zmian.

Szczegółowe informacje znajdują się w artykule [ExpressRoute FAQ](../expressroute/expressroute-faqs.md) (Usługa ExpressRoute — często zadawane pytania).

## Połączenia typu lokacja-lokacja a urządzenia sieci VPN

### Co należy wziąć pod uwagę przy wyborze urządzenia sieci VPN?

Zweryfikowaliśmy we współpracy z dostawcami urządzeń szereg standardowych urządzeń sieci VPN wykorzystywanych w ramach połączeń typu lokacja-lokacja. Listę znanych zgodnych urządzeń sieci VPN, odpowiednie instrukcje konfiguracji lub przykłady oraz dane techniczne urządzeń można znaleźć [tutaj](vpn-gateway-about-vpn-devices.md). Wszystkie urządzenia z rodzin wymienionych jako rodziny o znanej zgodności powinny współpracować z siecią wirtualną. Aby skonfigurować urządzenie sieci VPN, zapoznaj się z przykładową konfiguracją urządzenia lub łączem odpowiadającym odpowiedniej rodzinie urządzeń.

### Co zrobić, jeśli posiadane urządzenie sieci VPN nie zostało ujęte na liście znanych zgodnych urządzeń?

Jeśli urządzenie, którego chcesz użyć w celu nawiązania połączenia sieci VPN, nie znajduje się na liście znanych zgodnych urządzeń sieci VPN, sprawdź, czy jest ono zgodne z obsługiwanymi opcjami konfiguracji protokołu IPsec/IKE i parametrami wymienionymi [tutaj](vpn-gateway-about-vpn-devices.md#devices-not-on-the-compatible-list). Urządzenia spełniające minimalne wymagania powinny działać z bramami sieci VPN. Dodatkową pomoc oraz instrukcje dotyczące konfiguracji można uzyskać, kontaktując się z producentem urządzenia.

### Dlaczego mój oparty na zasadach tunel VPN przestaje działać, gdy ruch jest w stanie bezczynności?

Jest to oczekiwane zachowanie bram sieci VPN opartych na zasadach (znanych także jako bramy o routingu statycznym). Jeśli ruch za pośrednictwem tunelu będzie pozostawał w stanie bezczynności przez więcej niż 5 minut, nastąpi jego usunięcie. Natychmiast po rozpoczęciu ruchu danych w dowolnym kierunku będzie można ponownie ustanowić tunel.

### Czy można użyć oprogramowania pełniącego rolę sieci VPN do nawiązania połączenia z usługą Azure?

W ramach konfiguracji typu lokacja-lokacja obejmującej wiele lokalizacji obsługiwane są serwery Windows Server 2012 usługi Routing i dostęp zdalny (RRAS).

Inne rozwiązania o charakterze oprogramowania pełniącego rolę sieci VPN powinny współpracować z bramą, o ile są one zgodne ze standardowymi implementacjami protokołu IPsec stosowanymi w branży. W celu uzyskania pomocy technicznej oraz instrukcji konfiguracji należy skontaktować się z dostawcą oprogramowania.

## Połączenia typu punkt-lokacja

### Których systemów operacyjnych można używać z połączeniami typu punkt-lokacja?

Obsługiwane są następujące systemy operacyjne:

- Windows 7 (32-bitowy i 64-bitowy)

- Windows Server 2008 R2 (tylko 64-bitowy)

- Windows 8 (32-bitowy i 64-bitowy)

- Windows 8.1 (32-bitowy i 64-bitowy)

- Windows Server 2012 (tylko 64-bitowy)

- Windows Server 2012 R2 (tylko 64-bitowy)

- Windows 10

### Czy można użyć dowolnego oprogramowania klienta VPN do połączenia punkt-lokacja obsługującego protokół SSTP?

Nie. Obsługa jest ograniczona tylko do wersji systemu operacyjnego Windows wymienionych powyżej.

### Ile punktów końcowych klienta sieci VPN może obejmować konfiguracja punkt-lokacja?

Obsługiwanych jest maksymalnie 128 klientów VPN, które mogą jednocześnie nawiązać połączenie z siecią wirtualną.

### Czy w przypadku połączenia punkt-lokacja można użyć własnego głównego urzędu certyfikacji PKI?

Tak. Wcześniej można było używać tylko certyfikatów głównych z podpisem własnym. Nadal można przesłać 20 certyfikatów głównych.

### Czy można pominąć serwery proxy i zapory, korzystając z funkcji punkt-lokacja?

Tak. Do celów tunelowania przez zaporę jest wykorzystywany protokół SSTP (Secure Socket Tunneling Protocol). Ten tunel zostanie wyświetlony jako połączenie HTTPs.

### Czy w przypadku ponownego uruchomienia komputera klienckiego skonfigurowanego pod kątem połączenia typu punkt-lokacja połączenie z siecią VPN zostanie nawiązane automatycznie?

Domyślnie komputer kliencki nie przywraca automatycznie połączenia z siecią VPN.

### Czy w przypadku połączeń punkt-lokacja jest obsługiwane automatyczne ponowne nawiązywanie połączenia i DDNS na klientach sieci VPN?

Automatyczne ponowne nawiązywanie połączenia i DDNS nie są obecnie obsługiwane w przypadku połączeń VPN typu punkt-lokacja.

### Czy z konfiguracji typu lokacja-lokacja i punkt-lokacja można korzystać równolegle w ramach tej samej sieci wirtualnej?

Tak. Oba te rozwiązania będą działać, o ile zastosowana zostanie brama sieci VPN typu RouteBased. W przypadku klasycznego modelu wdrażania należy użyć bramy dynamicznej. Połączenia typu punkt-lokacja nie są obsługiwane w przypadku bram sieci VPN o statycznym routingu ani bram, których wartość parametru -VpnType to PolicyBased.

### Czy można skonfigurować klienta typu punkt-lokacja pod kątem jednoczesnego nawiązywania połączenia z wieloma sieciami wirtualnymi?

Tak, jest to możliwe. Sieci wirtualne nie mogą jednak mieć nakładających się prefiksów IP, a przestrzenie adresowe w przypadku połączenia punkt-lokacja nie mogą nakładać się między sieciami wirtualnymi.

### Jakiej przepustowości można oczekiwać w przypadku połączeń typu lokacja-lokacja lub punkt-lokacja?

Trudno jest utrzymać dokładną przepływność tuneli VPN. Protokoły IPsec i SSTP należą do niejawnie ciężkich protokołów sieci VPN. Przepływność ograniczają również opóźnienia i przepustowość między lokalizacjami lokalnymi i Internetem.

## Bramy

### Co to jest brama oparta na zasadach (o routingu statycznym)?

Bramy oparte na zasadach wdrażają sieci VPN oparte na zasadach. Sieci VPN oparte na zasadach szyfrują pakiety i kierują je bezpośrednio przez tunele protokołu IPsec na podstawie kombinacji prefiksów adresów między siecią lokalną użytkownika i siecią wirtualną platformy Azure. Zasady (lub selektor ruchu) są zazwyczaj zdefiniowane jako lista dostępu w konfiguracji VPN.

### Co to jest brama oparta na trasach (o routingu dynamicznym)?

Bramy oparte na trasach wdrażają sieci VPN oparte na trasach. Sieci VPN oparte na trasach używają „tras” w funkcji przesyłania dalej IP lub tabeli routingu do kierowania pakietów do odpowiednich interfejsów tuneli. W dalszej kolejności interfejsy tuneli szyfrują lub odszyfrowują pakiety wchodzące do tuneli lub wychodzące z nich. Zasady lub selektor ruchu dla sieci VPN opartych na trasach są skonfigurowane jako każdy z każdym (lub symbole wieloznaczne).

### Czy można użyć adresu IP bramy sieci VPN przed jej utworzeniem?

Nie. Aby uzyskać adres IP bramy, należy ją najpierw utworzyć. Adres IP ulegnie zmianie, jeśli brama VPN zostanie usunięta i ponownie utworzona.

### W jaki sposób następuje uwierzytelnienie tunelu VPN?

Sieć VPN platformy Azure używa uwierzytelniania PSK (klucza wstępnego). Podczas tworzenia tunelu VPN generowany jest klucz wstępny (PSK). Wygenerowany automatycznie klucz wstępny można zmienić na własny klucz przy użyciu polecenia cmdlet Set Pre-Shared Key lub interfejsu API REST programu PowerShell.

### Czy za pomocą interfejsu API i polecenia cmdlet Set Pre-Shared Key można skonfigurować własną bramę sieci VPN opartą na zasadach (o routingu statycznym)?

Tak, interfejs API i polecenie cmdlet Set Pre-Shared Key programu PowerShell pozwalają konfigurować sieci wirtualne platformy Azure oparte na zasadach (o statycznym routingu) i na trasach (o dynamicznym routingu).

### Czy są dostępne inne opcje uwierzytelniania?

Metody uwierzytelniania ograniczają się do stosowania kluczy wstępnych (PSK).

### Co to jest „podsieć bramy” i do czego jest ona potrzebna?

Stosujemy usługę bramy, której działanie umożliwia nawiązywanie połączeń obejmujących wiele lokalizacji. 

Aby skonfigurować bramę sieci VPN, należy utworzyć podsieć bramy dla sieci wirtualnej. Aby podsieć bramy działała prawidłowo, musi nosić nazwę GatewaySubnet. Nie należy nadawać podsieci bramy innej nazwy. Nie należy także wdrażać maszyn wirtualnych ani innych elementów w ramach podsieci bramy.

Minimalny rozmiar podsieci bramy zależy tylko od konfiguracji, którą chcesz utworzyć. Chociaż w przypadku niektórych konfiguracji możesz utworzyć małą podsieć bramy o rozmiarze /29, zaleca się tworzenie podsieci bramy /28 i większych (/28, /27, /26 itp.). 

### Czy można wdrożyć maszyny wirtualne lub wystąpienia roli w ramach podsieci bramy?

Nie.

### Jak określić, który ruch danych przechodzi przez bramę sieci VPN?

W przypadku użycia klasycznego portalu Azure należy dodać każdy z zakresów, który ma zostać wysłany za pośrednictwem bramy sieci wirtualnej, na stronie Sieci w sekcji Sieci lokalne.

### Czy można skonfigurować wymuszone tunelowanie?

Tak. Zobacz artykuł [Configure forced tunneling](vpn-gateway-about-forced-tunneling.md) (Konfiguracja wymuszonego tunelowania).

### Czy można wdrożyć własny serwer sieci VPN na platformie Azure i używać go do nawiązywania połączenia z siecią lokalną?

Tak, można wdrożyć własne bramy lub serwery sieci VPN na platformie Azure, korzystając z portalu Azure Marketplace lub tworząc własne routery sieci VPN. Należy skonfigurować w sieci wirtualnej trasy zdefiniowane przez użytkownika i upewnić się, że ruch jest prawidłowo przekierowywany między sieciami lokalnymi i podsieciami sieci wirtualnej.

### Dlaczego niektóre porty mojej bramy sieci VPN są otwarte?

Są one wymagane do komunikacji infrastruktury platformy Azure. Są one zabezpieczone (zablokowane) z użyciem certyfikatów Azure. Bez prawidłowych certyfikatów podmioty zewnętrzne, w tym klienci tych bram, nie będą mieć możliwości wywarcia jakiegokolwiek wpływu na te punkty końcowe.

Brama sieci VPN to zasadniczo wieloadresowe urządzenie, w przypadku którego jedna karta interfejsu sieciowego korzysta z sieci prywatnej klienta, a do drugiej można uzyskać dostęp z sieci publicznej. Podmioty infrastruktury platformy Azure nie mogą korzystać z sieci prywatnych klientów z przyczyn związanych z zachowaniem zgodności, w związku z czym muszą korzystać z publicznych punktów końcowych do komunikacji w ramach infrastruktury. Publiczne punkty końcowe są okresowo skanowane w ramach inspekcji zabezpieczeń platformy Azure.


### Więcej informacji na temat typów, wymagań i przepustowości bram

Aby uzyskać więcej informacji, zobacz temat [About VPN Gateway Settings](vpn-gateway-about-vpn gateway-settings.md) (Informacje o ustawieniach bramy VPN Gateway).

## Połączenia obejmujące wiele lokacji i połączenia między sieciami wirtualnymi

### Jakiego typu bramy mogą obsługiwać połączenia obejmujące wiele lokacji i połączenia między sieciami wirtualnymi?

Tylko bramy oparte na trasach (o routingu dynamicznym).

### Czy można połączyć sieć wirtualną VPN opartą na trasach z inną siecią wirtualną VPN opartą na zasadach?

Nie, obie sieci wirtualne MUSZĄ korzystać z sieci VPN opartych na trasach (o routingu dynamicznym).

### Czy ruch sieciowy w ramach połączenia między sieciami wirtualnymi jest bezpieczny?

Tak, jest zabezpieczony z użyciem szyfrowania IPsec/IKE.

### Czy ruch sieciowy w ramach połączenia między sieciami wirtualnymi przechodzi przez sieć szkieletową platformy Azure?

Tak.

### Z iloma lokacjami lokalnymi i sieciami wirtualnymi może połączyć się jedna sieć wirtualna?

Maksymalnie z 10 w przypadku podstawowych i standardowych bram o routingu dynamicznym; 30 w przypadku bram sieci VPN o wysokiej wydajności.

### Czy można używać sieci VPN typu punkt-lokacja z siecią wirtualną z wieloma tunelami VPN?

Tak, sieci VPN typu punkt-lokacja (P2S) można używać z bramami sieci VPN łączącymi się z wieloma lokacjami lokalnymi i innymi sieciami wirtualnymi.

### Czy można skonfigurować wiele tuneli między siecią wirtualną i lokalną lokacją z użyciem sieci VPN obejmującej wiele lokalizacji?

Nie, nadmiarowe tunele między siecią wirtualną platformy Azure i lokacją lokalną nie są obsługiwane.

### Czy wśród połączonych sieci wirtualnych i lokacji lokalnych mogą występować nakładające się przestrzenie adresowe?

Nie. Nakładające się przestrzenie adresowe spowodują, że przesłanie pliku konfiguracyjnego sieci lub „tworzenie sieci wirtualnej” nie powiedzie się.

### Czy przy większej liczbie połączeń VPN typu lokacja-lokacja można uzyskać większą przepustowość niż przy pojedynczej sieci wirtualnej?

Nie, wszystkie tunele VPN, w tym połączenia VPN typu punkt-lokacja, współdzielą tę samą bramę sieci VPN platformy Azure i dostępną przepustowość.

### Czy można używać bramy Azure VPN do przekazywania ruchu między lokacjami lokalnymi lub do innej sieci wirtualnej?

**Klasyczny model wdrażania**<br>
Przekazywanie ruchu za pośrednictwem bramy sieci VPN platformy Azure przy użyciu klasycznego modelu wdrażania jest możliwe, ale zależy od statycznie zdefiniowanych przestrzeni adresowych w pliku konfiguracyjnym sieci. Protokół BGP dla klasycznego modelu wdrażania nie jest jeszcze obsługiwany w połączeniu z sieciami wirtualnymi platformy Azure i bramami sieci VPN. Niezastosowanie protokołu BGP powoduje bardzo wysokie ryzyko błędów ręcznego definiowania przestrzeni adresowych przesyłania i nie jest zalecane.<br>
**Model wdrażania usługi Resource Manager**<br>
Jeśli używasz modelu wdrażania usługi Resource Manager, zobacz sekcję [protokołu BGP](#bgp), aby uzyskać więcej informacji.

### Czy platforma Azure generuje taki sam klucz wstępny protokołu IPsec/IKE dla wszystkich połączeń sieci VPN dla danej sieci wirtualnej?

Nie. Platforma Azure domyślnie generuje różne klucze wstępne dla różnych połączeń sieci VPN. Można jednak użyć interfejsu API REST lub polecenia cmdlet Set VPN Gateway Key programu PowerShell, aby ustawić preferowaną wartość klucza. Klucz MUSI mieć postać alfanumerycznego ciągu o długości od 1 do 128 znaków.

### Czy w ramach platformy Azure są naliczane opłaty za ruch danych między sieciami wirtualnymi?

W przypadku ruchu między różnymi sieciami wirtualnymi Azure opłaty są naliczane tylko dla ruchu przekazywanego z jednego regionu świadczenia usługi Azure do innego. Cena została określona na stronie [VPN Gateway Pricing](https://azure.microsoft.com/pricing/details/vpn-gateway/) (Cennik usługi VPN Gateway) dotyczącej platformy Azure.


### Czy można połączyć sieć wirtualną z sieciami VPN wykorzystującymi protokół IPsec z obwodem ExpressRoute?

Tak, takie rozwiązanie jest obsługiwane. Aby uzyskać więcej informacji, zobacz artykuł [Configure ExpressRoute and Site-to-Site VPN connections that coexist](../expressroute/expressroute-howto-coexist-classic.md) (Konfigurowanie obwodu ExpressRoute i współistniejących połączeń sieci VPN typu lokacja-lokacja).

## <a name="bgp"></a>BGP

[AZURE.INCLUDE [vpn-gateway-bgp-faq-include](../../includes/vpn-gateway-bpg-faq-include.md)] 



## Połączenia obejmujące wiele lokalizacji a maszyny wirtualne

### W jaki sposób należy połączyć się z maszyną wirtualną w przypadku, gdy należy ona do sieci wirtualnej, a dysponuję połączeniem obejmującym wiele lokalizacji?

Jest kilka możliwości. W warunkach włączonego protokołu RDP, jeśli utworzono punkt końcowy, można połączyć się z maszyną wirtualną, korzystając z adresu VIP. W takim przypadku należy określić adres VIP i numer portu, z którym ma zostać nawiązane połączenie. Należy skonfigurować port maszyny wirtualnej pod kątem ruchu danych. W tym celu wystarczy zwykle przejść do klasycznego portalu Azure i zapisać ustawienia połączenia RDP na komputerze. Ustawienia zawierają niezbędne informacje o połączeniu.

W przypadku sieci wirtualnej z funkcją łączności obejmującej wiele lokalizacji można połączyć się z maszyną wirtualną przy użyciu wewnętrznego adresu DIP lub prywatnego adresu IP. Z maszyną wirtualną można także nawiązać połączenie z poziomu innej maszyny wirtualnej, która znajduje się w tej samej sieci wirtualnej, korzystając z wewnętrznego adresu DIP. Nie można połączyć się z maszyną wirtualną z użyciem protokołu RDP i adresu DIP, jeśli połączenie jest nawiązywane z lokalizacji spoza sieci wirtualnej. Na przykład jeśli w przypadku dostępności skonfigurowanej sieci wirtualnej typu punkt-lokacja nie zostanie ustanowione połączenie z poziomu komputera, nie będzie możliwe połączenie się z maszyną wirtualną z użyciem adresu DIP.

### Czy jeśli maszyna wirtualna należy do sieci wirtualnej z funkcją łączności obejmującą wiele lokalizacji, to cały ruch z niej będzie przekazywany za pośrednictwem tego połączenia?

Nie. Przez bramę sieci wirtualnej przechodzi tylko ruch, którego docelowy adres IP należy do określonych zakresów adresów IP sieci lokalnej w ramach sieci wirtualnej. Ruch, którego docelowy adres IP należy do sieci wirtualnej, pozostaje w obrębie sieci wirtualnej. Inny ruch jest wysyłany do sieci publicznej za pośrednictwem usługi równoważenia obciążenia lub, w przypadku użycia wymuszonego tunelowania, wysłany przez bramę sieci VPN platformy Azure. Podczas rozwiązywania problemów należy się upewnić, że w procesie uwzględniono wszystkie wymienione w obszarze Sieć lokalna zakresy, które mają zostać wysłane za pośrednictwem bramy. Należy się upewnić, że zakresy adresów sieci lokalnej nie nakładają się z żadnymi zakresami adresów w sieci wirtualnej. Ponadto należy sprawdzić, czy używany serwer DNS rozpoznaje nazwę jako właściwy adres IP.


## Sieć wirtualna — często zadawane pytania

Dodatkowe informacje dotyczące sieci wirtualnej można znaleźć w artykule [Virtual Network FAQ](../virtual-network/virtual-networks-faq.md) (Sieć wirtualna — często zadawane pytania).
 



<!--HONumber=Sep16_HO5-->


