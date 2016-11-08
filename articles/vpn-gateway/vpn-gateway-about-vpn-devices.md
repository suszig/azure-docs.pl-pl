---
title: Informacje o urządzeniach sieci VPN używanych na potrzeby połączeń bramy VPN typu lokacja-lokacja dla sieci wirtualnych Azure | Microsoft Docs
description: Ten artykuł zawiera omówienie urządzeń sieci VPN i parametrów protokołu IPsec dla połączeń bramy sieci VPN S2S oraz linki do przykładów i instrukcji konfiguracji.
services: vpn-gateway
documentationcenter: na
author: yushwang
manager: rossort
editor: ''
tags: azure-resource-manager, azure-service-management

ms.service: vpn-gateway
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/13/2016
ms.author: yushwang;cherylmc

---
# Informacje o urządzeniach sieci VPN używanych na potrzeby połączeń bramy VPN typu lokacja-lokacja
Urządzenie sieci VPN jest niezbędne do skonfigurowania połączenia sieci VPN typu lokacja-lokacja (S2S). Połączeń typu lokacja-lokacja można użyć do tworzenia rozwiązań hybrydowych oraz bezpiecznych połączeń między siecią lokalną i wirtualną. W tym artykule omówiono zgodne urządzenia sieci VPN oraz parametry konfiguracji. 

> [!NOTE]
> Podczas konfigurowania połączenia typu lokacja-lokacja wymagane jest użycie publicznego adresu IPv4 dla urządzenia sieci VPN.                                                                                                                                                                               
> 
> 

Jeśli urządzenie nie jest wyświetlane w tabeli [zweryfikowanych urządzeń sieci VPN](#devicetable), zapoznaj się z sekcją tego artykułu poświęconą [niezweryfikowanym urządzeniom VPN](#additionaldevices). Istnieje możliwość, że urządzenie będzie działało na platformie Azure. W celu uzyskania pomocy technicznej w zakresie urządzenia VPN należy skontaktować się z jego producentem.

**Kwestie, które należy wziąć pod uwagę podczas przeglądania tabeli:**

* Nastąpiła zmiana terminologii w zakresie routingu statycznego i dynamicznego. W publikacjach można z powodzeniem natknąć się na oba warianty pojęć. Nie nastąpiła żadna zmiana w zakresie funkcjonalności — zmieniły się jedynie nazwy.
  * Routing statyczny = PolicyBased
  * Routing dynamiczny = RouteBased
* Specyfikacje dotyczące bramy VPN o wysokiej wydajności i bramy VPN typu RouteBased są takie same, o ile nie określono inaczej. Np. urządzenia sieci VPN zweryfikowane pod kątem poprawności, które są zgodne z bramami sieci VPN typu RouteBased, są również zgodne z bramą VPN Azure o wysokiej wydajności. 

## <a name="devicetable"></a>Zweryfikowane urządzenia sieci VPN
Zweryfikowaliśmy szereg standardowych urządzeń sieci VPN we współpracy z ich dostawcami. Wszystkie urządzenia z rodzin ujętych na poniższej liście powinny współpracować z bramami sieci VPN Azure. Aby sprawdzić typ bramy niezbędnej do utworzenia rozwiązania, które chcesz skonfigurować, zobacz artykuł [Bramy sieci VPN — informacje](vpn-gateway-about-vpngateways.md). 

Aby łatwiej skonfigurować urządzenie sieci VPN, zapoznaj się z linkami odpowiadającymi ich poszczególnym rodzinom. W celu uzyskania pomocy technicznej w zakresie urządzenia VPN należy skontaktować się z jego producentem.

| **Dostawca** | **Rodzina urządzeń** | **Minimalna wersja systemu operacyjnego** | **PolicyBased** | **RouteBased** |
| --- | --- | --- | --- | --- |
| Allied Telesis |Routery sieci VPN z serii AR |2.9.2 |Wkrótce |Niezgodne |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall z serii F |PolicyBased: 5.4.3, RouteBased: 6.2.0 |[Instrukcje dotyczące konfiguracji](https://techlib.barracuda.com/NGF/AzurePolicyBasedVPNGW) |[Instrukcje dotyczące konfiguracji](https://techlib.barracuda.com/NGF/AzureRouteBasedVPNGW) |
| Barracuda Networks, Inc. |Barracuda NextGen Firewall z serii X |Barracuda Firewall 6.5 |[Barracuda Firewall](https://techlib.barracuda.com/BFW/ConfigAzureVPNGateway) |Niezgodne |
| Brocade |Vyatta 5400 vRouter |Virtual Router 6.6R3 GA |[Instrukcje dotyczące konfiguracji](http://www1.brocade.com/downloads/documents/html_product_manuals/vyatta/vyatta_5400_manual/wwhelp/wwhimpl/js/html/wwhelp.htm#href=VPN_Site-to-Site%20IPsec%20VPN/Preface.1.1.html) |Niezgodne |
| Check Point |Security Gateway |R75.40, R75.40VS |[Instrukcje dotyczące konfiguracji](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |[Instrukcje dotyczące konfiguracji](https://supportcenter.checkpoint.com/supportcenter/portal?eventSubmit_doGoviewsolutiondetails=&solutionid=sk101275) |
| Cisco |ASA |8.3 |[Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASA) |Niezgodne |
| Cisco |ASR |IOS 15.1 (PolicyBased), IOS 15.2 (RouteBased) |[Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |[Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ASR) |
| Cisco |ISR |IOS 15.0 (PolicyBased), IOS 15.1 (RouteBased*) |[Przykłady firmy Cisco](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |[Przykłady firmy Cisco*](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Cisco/Current/ISR) |
| Citrix |NetScaler MPX, SDX, VPX |10.1 lub nowsze |[Instrukcje dotyczące integracji](https://docs.citrix.com/en-us/netscaler/11-1/system/cloudbridge-connector-introduction/cloudbridge-connector-azure.html) |Niezgodne |
| Dell SonicWALL |Seria TZ, seria NSA, seria SuperMassive, seria E-Class NSA |SonicOS 5.8.x, [SonicOS 5.9.x](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=850), [SonicOS 6.x](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide/supported-platforms?ParentProduct=646) |[Instrukcje — SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instrukcje — SonicOS 5.9.](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |[Instrukcje — SonicOS 6.2](http://documents.software.dell.com/sonicos/6.2/microsoft-azure-configuration-guide?ParentProduct=646) [Instrukcje — SonicOS 5.9.](http://documents.software.dell.com/sonicos/5.9/microsoft-azure-configuration-guide?ParentProduct=850) |
| F5 |Seria BIG-IP |12.0 |[Instrukcje dotyczące konfiguracji](https://devcentral.f5.com/articles/connecting-to-windows-azure-with-the-big-ip) |[Instrukcje dotyczące konfiguracji](https://devcentral.f5.com/articles/big-ip-to-azure-dynamic-ipsec-tunneling) |
| Fortinet |FortiGate |FortiOS 5.2.7 |[Instrukcje dotyczące konfiguracji](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure) |[Instrukcje dotyczące konfiguracji](http://docs.fortinet.com/d/fortigate-configuring-ipsec-vpn-between-a-fortigate-and-microsoft-azure) |
| Internet Initiative Japan (IIJ) |Seria SEIL |SEIL/X 4.60, SEIL/B1 4.60, SEIL/x86 3.20 |[Instrukcje dotyczące konfiguracji](http://www.iij.ad.jp/biz/seil/ConfigAzureSEILVPN.pdf) |Niezgodne |
| Juniper |SRX |JunOS 10.2 (PolicyBased), JunOS 11.4 (RouteBased) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SRX) |
| Juniper |Seria J |JunOS 10.4r9 (PolicyBased), JunOS 11.4 (RouteBased) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/JSeries) |
| Juniper |ISG |ScreenOS 6.3 (PolicyBased i RouteBased) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/ISG) |
| Juniper |SSG |ScreenOS 6.2 (PolicyBased i RouteBased) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |[Przykłady firmy Juniper](https://github.com/Azure/Azure-vpn-config-samples/tree/master/Juniper/Current/SSG) |
| Microsoft |Routing and Remote Access Service |Windows Server 2012 |Niezgodne |[Przykłady firmy Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=717761) |
| Open Systems AG |Mission Control Security Gateway |Nie dotyczy |[Przewodnik instalacji](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |[Przewodnik instalacji](https://www.open.ch/_pdf/Azure/AzureVPNSetup_Installation_Guide.pdf) |
| Openswan |Openswan |2.6.32 |(Wkrótce) |Niezgodne |
| Palo Alto Networks |Wszystkie urządzenia z systemem PAN-OS |PAN-OS 6.1.5 lub nowszy (PolicyBased), PAN-OS 7.0.5 lub nowszy (RouteBased) |[Instrukcje dotyczące konfiguracji](https://live.paloaltonetworks.com/t5/Configuration-Articles/How-to-Configure-VPN-Tunnel-Between-a-Palo-Alto-Networks/ta-p/59065) |[Instrukcje dotyczące konfiguracji](https://live.paloaltonetworks.com/t5/Integration-Articles/Configuring-IKEv2-VPN-for-Microsoft-Azure-Environment/ta-p/60340) |
| Watchguard |Wszystkie |Fireware XTM v11.x |[Instrukcje dotyczące konfiguracji](http://customers.watchguard.com/articles/Article/Configure-a-VPN-connection-to-a-Windows-Azure-virtual-network/) |Niezgodne |

(*) Routery z serii ISR 7200 obsługują tylko sieci VPN typu PolicyBased.

## <a name="additionaldevices"></a>Niezweryfikowane urządzenia sieci VPN
Jeśli urządzenie nie zostało ujęte w tabeli zweryfikowanych urządzeń sieci VPN, być może będzie działało w ramach połączenia typu lokacja-lokacja. Sprawdź, czy urządzenie sieci VPN spełnia minimalne wymagania opisane w sekcji Wymagania dotyczące bramy artykułu [About VPN Gateways](vpn-gateway-about-vpngateways.md#gateway-requirements) (Informacje o bramach sieci VPN). Urządzenia spełniające minimalne wymagania powinny również działać z bramami sieci VPN. Dodatkową pomoc oraz instrukcje dotyczące konfiguracji można uzyskać, kontaktując się z producentem urządzenia.

## Edytowanie przykładów konfiguracji urządzenia
Po pobraniu dostarczonej przykładowej konfiguracji urządzenia sieci VPN należy zastąpić niektóre z wartości w celu odzwierciedlenia ustawień własnego środowiska. 

**Aby edytować przykładową konfigurację:**

1. Otwórz przykładową konfigurację za pomocą Notatnika. 
2. Wyszukaj i zamień wszystkie ciągi <*text*> z wartościami, które odnoszą się do używanego środowiska. Nie zapomnij o nawiasach < i >. Podczas określenia nazwy należy zwrócić uwagę na to, aby była ona unikatowa. Jeśli polecenie nie działa, zapoznaj się z dokumentacją producenta urządzenia.

| **Przykładowy tekst** | **Zmień na** |
| --- | --- |
| &lt;RP_OnPremisesNetwork&gt; |Nazwę tego obiektu definiuje użytkownik. Przykład: myOnPremisesNetwork |
| &lt;RP_AzureNetwork&gt; |Nazwę tego obiektu definiuje użytkownik. Przykład: myAzureNetwork |
| &lt;RP_AccessList&gt; |Nazwę tego obiektu definiuje użytkownik. Przykład: myAzureAccessList |
| &lt;RP_IPSecTransformSet&gt; |Nazwę tego obiektu definiuje użytkownik. Przykład: myIPSecTransformSet |
| &lt;RP_IPSecCryptoMap&gt; |Nazwę tego obiektu definiuje użytkownik. Przykład: myIPSecCryptoMap |
| &lt;SP_AzureNetworkIpRange&gt; |Określ zakres. Przykład: 192.168.0.0 |
| &lt;SP_AzureNetworkSubnetMask&gt; |Określ maskę podsieci. Przykład: 255.255.0.0 |
| &lt;SP_OnPremisesNetworkIpRange&gt; |Określ zakres lokalny. Przykład: 10.2.1.0 |
| &lt;SP_OnPremisesNetworkSubnetMask&gt; |Określ lokalną maskę podsieci. Przykład: 255.255.255.0 |
| &lt;SP_AzureGatewayIpAddress&gt; |Te informacje dotyczące konkretnej sieci wirtualnej znajdują się w portalu zarządzania usługą, w obszarze **Adres IP bramy**. |
| &lt;SP_PresharedKey&gt; |Te informacje dotyczące konkretnej sieci wirtualnej znajdują się w portalu zarządzania usługą, w obszarze Zarządzaj kluczem. |

## Parametry protokołu IPsec
> [!NOTE]
> Choć wartości wymienione w poniższej tabeli są obsługiwane przez bramę sieci VPN Azure, obecnie nie istnieje sposób na określenie lub wybranie określonej kombinacji z obszaru bramy sieci VPN Azure. Należy określić wszystkie ograniczenia z poziomu lokalnego urządzenia sieci VPN. Ponadto należy określić ograniczenie wartości MSS na 1350.
> 
> 

### Konfigurowanie IKE — faza 1
| **Właściwość** | **PolicyBased** | **Brama sieci VPN typu RouteBased i standardowa lub o wysokiej wydajności** |
| --- | --- | --- |
| Wersja IKE |IKEv1 |IKEv2 |
| Grupa Diffie’ego-Hellmana |Grupa 2 (1024 bity) |Grupa 2 (1024 bity) |
| Metoda uwierzytelniania |Klucz wstępny |Klucz wstępny |
| Algorytmy szyfrowania |AES256 AES128 3DES |AES256 3DES |
| Algorytm skrótu |SHA1(SHA128) |SHA1(SHA128), SHA2(SHA256) |
| Okres istnienia (czas) skojarzenia zabezpieczeń (Security Association — SA) fazy 1 |28 800 sekund |10 800 sekund |

### Konfigurowanie IKE — faza 2
| **Właściwość** | **PolicyBased** | **Brama sieci VPN typu RouteBased i standardowa lub o wysokiej wydajności** |
| --- | --- | --- |
| Wersja IKE |IKEv1 |IKEv2 |
| Algorytm skrótu |SHA1(SHA128) |SHA1(SHA128) |
| Okres istnienia (czas) skojarzenia zabezpieczeń (Security Association — SA) fazy 2 |3600 sekund |3600 sekund |
| Okres istnienia (przepływność) skojarzenia zabezpieczeń (Security Association — SA) fazy 2 |102 400 000 KB |- |
| Oferty szyfrowania i uwierzytelniania skojarzeń zabezpieczeń protokołu IPsec (wymienione w kolejności preferencji) |1. ESP-AES256 2. ESP-AES128 3. ESP-3DES 4. Nie dotyczy |Zobacz *Oferty skojarzeń zabezpieczeń (SA) protokołu IPsec bramy typu RouteBased* (poniżej). |
| Doskonałe utajnienie przekazywania (PFS) |Nie |Nie (*) |
| Wykrywanie nieaktywnych elementów równorzędnych |Nieobsługiwane |Obsługiwane |

(*) Brama Azure jako obiekt odpowiadający protokołu IKE może akceptować PFS DH Group 1, 2, 5, 14, 24.

### Oferty skojarzeń zabezpieczeń (SA) protokołu IPsec bramy typu RouteBased
W poniższej tabeli przedstawiono oferty szyfrowania i uwierzytelniania skojarzeń zabezpieczeń protokołu IPsec. Oferty są wymienione w kolejności preferencji, w jakiej oferta jest przedstawiona lub zaakceptowana.

| **Oferty szyfrowania i uwierzytelniania skojarzeń zabezpieczeń protokołu IPsec** | **Brama Azure jako inicjator** | **Brama Azure jako obiekt odpowiadający** |
| --- | --- | --- |
| 1 |ESP AES_256 SHA |ESP AES_128 SHA |
| 2 |ESP AES_128 SHA |ESP 3_DES MD5 |
| 3 |ESP 3_DES MD5 |ESP 3_DES SHA |
| 4 |ESP 3_DES SHA |AH SHA1 z ESP AES_128 z pustym kluczem HMAC |
| 5 |AH SHA1 z ESP AES_256 z pustym kluczem HMAC |AH SHA1 z ESP 3_DES z pustym kluczem HMAC |
| 6 |AH SHA1 z ESP AES_128 z pustym kluczem HMAC |AH MD5 z ESP 3_DES z pustym kluczem HMAC, brak określonych okresów istnienia |
| 7 |AH SHA1 z ESP 3_DES z pustym kluczem HMAC |AH SHA1 z ESP 3_DES SHA1, brak okresów istnienia |
| 8 |AH MD5 z ESP 3_DES z pustym kluczem HMAC, brak określonych okresów istnienia |AH MD5 z ESP 3_DES MD5, brak okresów istnienia |
| 9 |AH SHA1 z ESP 3_DES SHA1, brak okresów istnienia |ESP DES MD5 |
| 10 |AH MD5 z ESP 3_DES MD5, brak okresów istnienia |ESP DES SHA1, brak okresów istnienia |
| 11 |ESP DES MD5 |AH SHA1 z ESP DES z pustym kluczem HMAC, brak określonych okresów istnienia |
| 12 |ESP DES SHA1, brak okresów istnienia |AH MD5 z ESP DES z pustym kluczem HMAC, brak określonych okresów istnienia |
| 13 |AH SHA1 z ESP DES z pustym kluczem HMAC, brak określonych okresów istnienia |AH SHA1 z ESP DES SHA1, brak okresów istnienia |
| 14 |AH MD5 z ESP DES z pustym kluczem HMAC, brak określonych okresów istnienia |AH MD5 z ESP DES MD5, brak okresów istnienia |
| 15 |AH SHA1 z ESP DES SHA1, brak okresów istnienia |ESP SHA, brak okresów istnienia |
| 16 |AH MD5 z ESP DES MD5, brak okresów istnienia |ESP MD5, brak okresów istnienia |
| 17 |- |AH SHA, brak okresów istnienia |
| 18 |- |AH MD5, brak okresów istnienia |

* Można określić wartość NULL szyfrowania ESP protokołu IPsec z bramami sieci VPN typu RouteBased i bramami sieci VPN o wysokiej wydajności. Szyfrowanie oparte na wartości null nie zapewnia ochrony danych w trakcie ich przesyłania i powinno być używane wyłącznie w przypadku, gdy wymagana jest maksymalna przepustowość i minimalne opóźnienia.  Klienci mogą skorzystać z tej możliwości w kontekście scenariuszy komunikacji między sieciami wirtualnymi lub w przypadku, gdy szyfrowanie jest stosowane w innym obszarze rozwiązania.
* Chcąc korzystać z łączności przez Internet obejmującej wiele lokalizacji, należy użyć ustawień domyślnych usługi Azure VPN Gateway z szyfrowaniem i algorytmami skrótu wymienionymi w tabelach powyżej w celu zapewnienia bezpieczeństwa komunikacji o krytycznym znaczeniu.

<!--HONumber=Sep16_HO3-->


