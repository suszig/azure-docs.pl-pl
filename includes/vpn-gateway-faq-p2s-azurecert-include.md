[!INCLUDE [P2S FAQ All](vpn-gateway-faq-p2s-all-include.md)]

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Czy w przypadku połączenia punkt-lokacja można użyć własnego głównego urzędu certyfikacji PKI?

Tak. Wcześniej można było używać tylko certyfikatów głównych z podpisem własnym. Nadal można przesłać 20 certyfikatów głównych.

### <a name="what-tools-can-i-use-to-create-certificates"></a>Jakie narzędzia można użyć w celu utworzenia certyfikatów?

Można użyć rozwiązania infrastruktura PKI przedsiębiorstwa (wewnętrznej infrastruktury PKI), programu Azure PowerShell, MakeCert i biblioteki OpenSSL.

### <a name="certsettings"></a>Czy istnieją instrukcje dotyczące parametry i ustawienia certyfikatu?

* **Wewnętrzny rozwiązania infrastruktury kluczy publicznych/Enterprise PKI:** czynności, aby [generowania certyfikatów](../articles/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert).

* **Program Azure PowerShell:** zobacz [programu Azure PowerShell](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md) artykułu dla czynności.

* **MakeCert:** zobacz [MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md) artykułu dla czynności.

* **Biblioteki OpenSSL:** 

    * Podczas eksportowania certyfikatów, należy przekonwertować certyfikatu głównego w formacie Base64.

    * Dla certyfikatu klienta:

      * Podczas tworzenia klucza prywatnego, określ długość jako 4096.
      * Podczas tworzenia certyfikatu dla *-rozszerzenia* parametru, określ *usr_cert*.