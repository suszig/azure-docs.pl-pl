### <a name="supportedclientos"></a>Których systemów operacyjnych klienta można używać z połączeniami typu punkt-lokacja?

Obsługiwane są następujące systemy operacyjne klientów:

* Windows 7 (32-bitowy i 64-bitowy)
* Windows Server 2008 R2 (tylko 64-bitowy)
* Windows 8 (32-bitowy i 64-bitowy)
* Windows 8.1 (32-bitowy i 64-bitowy)
* Windows Server 2012 (tylko 64-bitowy)
* Windows Server 2012 R2 (tylko 64-bitowy)
* Windows Server 2016 (tylko 64-bitowy)
* Windows 10
* OSX 10.11 dla komputerów Mac (El Capitan)
* macOS 10.12 dla komputerów Mac (Sierra)

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Ile punktów końcowych klienta sieci VPN może obejmować konfiguracja punkt-lokacja?

Obsługiwanych jest maksymalnie 128 klientów VPN, które mogą jednocześnie nawiązać połączenie z siecią wirtualną.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Czy można pominąć serwery proxy i zapory, korzystając z funkcji punkt-lokacja?

Platforma Azure obsługuje dwa typy połączeń punkt-lokacja w sieci VPN:

* Protokół Secure Socket Tunneling Protocol (SSTP). Protokół SSTP jest własnym rozwiązaniem firmy Microsoft bazującym na protokole SSL. Może przechodzić przez zapory, ponieważ większość zapór otwiera port TCP 443, z którego korzysta protokół SSL.

* Sieć VPN z protokołem IKEv2. Sieć VPN z protokołem IKEv2 to oparte na standardach rozwiązanie sieci VPN IPsec, które korzysta z portów UDP 500 i 4500 i protokołu IP nr 50. Zapory nie zawsze otwierają te porty, więc istnieje możliwość, że sieć VPN z protokołem IKEv2 nie będzie mogła nawiązywać połączeń przez serwery proxy i zapory.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Czy w przypadku ponownego uruchomienia komputera klienckiego skonfigurowanego pod kątem połączenia typu punkt-lokacja połączenie z siecią VPN zostanie nawiązane automatycznie?

Domyślnie komputer kliencki nie przywraca automatycznie połączenia z siecią VPN.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Czy w przypadku połączeń punkt-lokacja jest obsługiwane automatyczne ponowne nawiązywanie połączenia i DDNS na klientach sieci VPN?

Automatyczne ponowne nawiązywanie połączenia i DDNS nie są obecnie obsługiwane w przypadku połączeń VPN typu punkt-lokacja.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Czy z konfiguracji typu lokacja-lokacja i punkt-lokacja można korzystać równolegle w ramach tej samej sieci wirtualnej?

Tak. W przypadku modelu wdrażania przy użyciu usługi Resource Manager dla bramy trzeba mieć sieć VPN opartą na trasach. W przypadku klasycznego modelu wdrażania należy użyć bramy dynamicznej. Nie obsługujemy połączeń typu punkt-lokacja dla bram VPN Gateway z routingiem statycznym lub bram VPN Gateway opartych na zasadach.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Czy można skonfigurować klienta typu punkt-lokacja pod kątem jednoczesnego nawiązywania połączenia z wieloma sieciami wirtualnymi?

Nie. Klient połączenia typu punkt-lokacja może łączyć się wyłącznie z zasobami w sieci wirtualnej, w której znajduje się brama sieci wirtualnej.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Jakiej przepływności można oczekiwać w przypadku połączeń typu lokacja-lokacja lub punkt-lokacja?

Trudno jest utrzymać dokładną przepływność tuneli VPN. Protokoły IPsec i SSTP należą do niejawnie ciężkich protokołów sieci VPN. Przepływność ograniczają również opóźnienia i przepustowość między lokalizacjami lokalnymi i Internetem. W przypadku bramy VPN Gateway z samymi połączeniami typu punkt-lokacja IKEv2 przez sieć VPN całkowita przepływność, której można oczekiwać, zależy od jednostki SKU bramy. Aby uzyskać więcej informacji o przepływności, zobacz [Gateway SKUs](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) (Jednostki SKU bramy).

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Czy mogę używać dowolnego programowego klienta sieci VPN dla połączeń typu punkt-lokacja obsługujących protokoły SSTP i/lub IKEv2?

Nie. Możesz używać wyłącznie natywnego klienta sieci VPN w systemie Windows dla protokołu SSTP i natywnego klienta sieci VPN na komputerach Mac dla protokołu IKEv2. Zapoznaj się z listą obsługiwanych systemów operacyjnych klienta.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>Czy platforma Azure obsługuje sieć VPN z protokołem IKEv2 w systemie Windows?

Użytkownicy mogą łączyć się z platformą Azure za pomocą wbudowanego klienta VPN systemu Windows, który obsługuje protokół IKEv2. Połączenia z użyciem protokołu IKEv2 z urządzenia z systemem Windows nie będą jednak działać w następującym scenariuszu:

  Gdy urządzenie użytkownika zawiera dużą liczbę zaufanych certyfikatów głównych, rozmiar ładunku komunikatu podczas wymiany IKE jest duży i powoduje fragmentację warstwy IP. Fragmenty są odrzucane na końcu platformy Azure, co powoduje niepowodzenie połączenia. Dokładna liczba certyfikatów, przy której występuje ten problem, jest trudna do oszacowania. W związku z tym połączenia z użyciem protokołu IKEv2 z urządzeń z systemem Windows nie mają gwarancji działania. W przypadku skonfigurowania protokołów SSTP i IKEv2 w środowisku mieszanym (składającym się z urządzeń z systemami Windows i Mac) profil sieci VPN systemu Windows zawsze próbuje najpierw tunelowania protokołu IKEv2. Jeśli to się nie powiedzie z powodu problemu opisanego w tym miejscu, nastąpi powrót do protokołu SSTP.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>Jakie platformy, oprócz systemów Windows i Mac, platforma Azure obsługuje dla sieci VPN P2S?

Platforma Azure obsługuje tylko systemy Windows i Mac dla sieci VPN typu punkt-lokacja.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enable-radius-andor-ikev2-vpn-on-it"></a>Mam już wdrożoną usługę Azure VPN Gateway. Na nim można włączyć usługi RADIUS i/lub IKEv2 sieci VPN?

Tak, aby umożliwić te nowe funkcje w bramach już wdrożony przy użyciu programu Powershell lub w portalu Azure, pod warunkiem, że jednostka SKU, używanej bramy obsługuje usługi RADIUS i/lub IKEv2. Na przykład podstawowa jednostka SKU bramy sieci VPN nie obsługuje usługi RADIUS lub IKEv2.
