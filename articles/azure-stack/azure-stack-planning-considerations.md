---
title: "Kwestie do uwzględnienia stosu Azure zintegrowanych systemów | Dokumentacja firmy Microsoft"
description: "Dowiedz się, co można zrobić, aby teraz zaplanować i przygotować wielowęzłowego Azure stosu."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 90f8fa1a-cace-4bfa-852b-5abe2b307615
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/16/2017
ms.author: mabrigg
ms.openlocfilehash: b39b3b8f73f9deef48173c712d7966eb13c6ecc3
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
---
# <a name="planning-considerations-for-azure-stack-integrated-systems"></a>Kwestie do uwzględnienia stosu Azure zintegrowanych systemów

*Dotyczy: Azure stosu zintegrowane systemy*

Jeśli interesuje Cię systemu Azure stosu zintegrowane, chcesz poznać niektóre z najważniejszych kwestii dotyczących planowania wdrożenia i jak system dopasowuje się do centrum danych. Ten temat zawiera ogólne omówienie te zagadnienia.

Jeśli zdecydujesz się na wykupienie zintegrowany system, dostawca sprzętu producenta sprzętu (OEM) pomaga przeprowadzają użytkownika przez proces znacznie procesu planowania bardziej szczegółowo. Będą one również wykonywać rzeczywiste wdrożenie.

## <a name="management-considerations"></a>Zagadnienia dotyczące zarządzania

Azure stos jest zapieczętowany system, gdzie infrastruktury jest zablokowana zarówno od uprawnień i sieciowych perspektywy. Listy kontroli dostępu do sieci (ACL) są stosowane do Zablokuj cały ruch przychodzący nieautoryzowanych i wszystkie zbędne komunikacji między składnikami infrastruktury. Utrudnia nieautoryzowanym użytkownikom dostęp do systemu.

Codzienne zarządzanie i operacje jest nie administratora nieograniczony dostęp do infrastruktury. Operatory stosu Azure muszą zarządzać systemu za pośrednictwem portalu administratora lub za pośrednictwem usługi Azure Resource Manager (za pośrednictwem programu PowerShell lub interfejsu API REST). Brak dostępu do systemu, narzędzia do zarządzania, takie jak Menedżer funkcji Hyper-V lub Menedżera klastra trybu Failover nie istnieje. Aby lepiej chronić systemu, oprogramowania innych firm (na przykład agenci) nie można zainstalować wewnątrz składników infrastruktury Azure stosu. Współdziałanie z zewnętrznego oprogramowania do zarządzania i zabezpieczeń odbywa się za pośrednictwem programu PowerShell lub interfejsu API REST.

Gdy wyższy poziom dostępu jest potrzebna do rozwiązywania problemów, które nie są rozpoznawane za pomocą kroków pośrednictwa alertu, należy skontaktować się z pomocy technicznej. Za pomocą techniczną Brak metody zapewnienie tymczasowego pełne uprawnienia dostępu administratora systemu do bardziej zaawansowanych operacji. 

## <a name="deploy-connected-or-disconnected"></a>Wdrażanie połączone lub rozłączone
 
Można wdrożyć stosu Azure albo połączony z Internetem (i na platformie Azure) lub odłączony. Uzyskanie największe korzyści ze stosu Azure, w tym scenariuszy hybrydowych między stosu Azure i platformą Azure, czy chcesz wdrożyć podłączone do platformy Azure. Poniższa tabela ułatwia podsumowanie podstawowych różnic między trybami wdrożenia.

| Obszar | Trybu sieciowego | Trybie rozłączonym |
| -------- | ------------- | ----------|
| Dostawca tożsamości | Azure Active Directory (Azure AD) lub usługi Active Directory Federation Services (AD FS) | Tylko usługi AD FS |
| Syndykacja Marketplace | Pobieranie elementów bezpośrednio z poziomu portalu Azure Marketplace do witryny marketplace w stosie Azure | Wymaga ręcznego zarządzania marketplace w stosie Azure |
| Model licencjonowania | Płatności jako — użytkownik użytku lub na podstawie zdolności produkcyjnych | Na podstawie zdolności tylko|
| Poprawek i aktualizacji  | Pobierz pakiety aktualizacji bezpośrednio na stosie Azure | Wymaga nośnik wymienny i oddzielne podłączonego urządzenia |
| | | |

Nie można zmienić trybu wdrożenia później bez ponownego wdrażania całego systemu.

## <a name="identity-considerations"></a>Zagadnienia dotyczące tożsamości

### <a name="choose-identity-provider"></a>Wybierz dostawcę tożsamości

Należy wziąć pod uwagę dostawcy tożsamości, którego chcesz użyć dla wdrożenia stosu Azure, Azure AD lub AD FS. Nie można przełączyć dostawców tożsamości, po wdrożeniu bez ponownego wdrażania całego systemu.

Wybór dostawcy tożsamości nie ma wpływu na maszyny wirtualne dzierżawców, system obsługi tożsamości i kont, których używają, czy ich może dołączyć do domeny usługi Active Directory itp. To jest oddzielony.

**Ze względu na użycie usługi Azure AD**

- Masz już istniejących inwestycji w usłudze Azure AD (takich jak Azure lub usługi Office 365).
- Chcesz użyć tej samej tożsamości w chmurze Azure oraz Azure stosu.
- Umożliwia obsługę scenariuszy wielodostępność, gdzie mogą hostować innych organizacji dla tego samego wystąpienia usługi Azure stosu.
- Chcesz użyć zarządzania opartego na interfejsie REST katalogu za pomocą usługi Azure AD Graph do świadczenia użytkowników, grup itp., za pośrednictwem interfejsów API.

> [!NOTE]
> Nie można połączyć wdrożenia stosu Azure do wystąpienia usługi Azure AD i istniejącego wystąpienia usług AD FS, w tym samym czasie. Jeśli wdrożysz z usługą Azure AD, a chcesz użyć istniejącego wystąpienia usług AD FS, można było wykonać Federację lokalnego wystąpienia usług AD FS z usługą Azure AD.

**Ze względu na użycie usług AD FS**

- Brak nie lub tylko częściowe łączności z Internetem.
- Istnieją wymagania prawne/suwerenności.
- Chcesz użyć własnego systemu tożsamości (na przykład w sieci firmowej usługi Active Directory) dla operatora i kontami użytkowników. Aby to zrobić, mogą tworzyć federacje z istniejącym wystąpieniem usług AD FS (w systemie Windows Server 2012, 2012 R2 lub 2016), który nie jest obsługiwana przez usługi Active Directory.
- Użytkownik nie ma żadnych istniejących inwestycji Azure, a nie chcesz używać usługi Azure AD.

> [!NOTE]
> Stos Azure można było wykonać Federację, tylko z innego wystąpienia usług AD FS, która nie jest obsługiwana przez usługi Active Directory. Innych dostawców tożsamości nie są obsługiwane. Jeśli masz innych dostawców tożsamości, które będą korzystać z usługi Azure stosu wdrożenia usługi Azure AD na podstawie zamiast tego Rozważ użycie.

### <a name="ad-fs-and-graph-integration"></a>Integracja usług AD FS i wykres

Jeśli wybierzesz do wdrożenia stosu Azure za pomocą usług AD FS jako dostawca tożsamości, można zintegrować wystąpienia usług AD FS na stosie Azure z istniejącym wystąpieniem usług AD FS za pomocą zaufania federacyjnego. Dzięki temu tożsamości w istniejącym lesie usługi Active Directory do uwierzytelniania z zasobami w stosie Azure.

Można również zintegrować usługę wykresu w stosie Azure z istniejącej usługi Active Directory. Pozwala na zarządzanie oparte na rolach kontroli dostępu (RBAC) w stosie Azure. Po oddelegowaniu dostęp do zasobu składnika Wykres wyszukuje konta użytkownika w istniejącym lesie usługi Active Directory przy użyciu protokołu LDAP.

Na poniższym diagramie przedstawiono zintegrowane przepływu ruchu usługi AD FS i Graph.
![Diagram przedstawiający przepływ ruchu wykres i usług AD FS](media/azure-stack-planning-considerations/ADFSIntegration.PNG)

## <a name="licensing-model"></a>Model licencjonowania

Należy określić model licencjonowania, który ma być używany. W przypadku połączonych wdrożenia można płatności jako — użytkownik użycia lub na podstawie zdolności licencjonowania. Płatności — jako — możesz — użycie wymaga połączenia z platformą Azure raportowania użycia, następnie jest on rozliczany za pośrednictwem commerce platformy Azure. Tylko pojemności na podstawie licencji jest obsługiwana w przypadku wdrożenia odłączony od Internetu. Aby uzyskać więcej informacji na temat modeli licencjonowania, zobacz [Microsoft Azure stosu pakowania i cenach](https://azure.microsoft.com/mediahandler/files/resourcefiles/5bc3f30c-cd57-4513-989e-056325eb95e1/Azure-Stack-packaging-and-pricing-datasheet.pdf).

## <a name="naming-decisions"></a>Decyzje dotyczące nazewnictwa

Należy się zastanowić, jak chcesz zaplanować nazw Azure stosu, szczególnie nazwa regionu i nazwy domeny zewnętrznej. Kombinacja tych dwóch nazw jest w pełni kwalifikowaną nazwę (FQDN) wdrożenia stosu Azure dla publicznych punktów końcowych &lt; *region*&gt;&lt;*external_FQDN*  &gt;, na przykład *east.cloud.fabrikam.com*. W tym przykładzie portali stosu Azure będzie dostępna w następujących adresów URL:

- https://Portal.East.cloud.Fabrikam.com
- https://adminportal.East.cloud.Fabrikam.com

W poniższej tabeli przedstawiono te decyzje nazewnictwa domeny.

| Nazwa | Opis | 
| -------- | ------------- | 
|Nazwa regionu | Nazwa pierwszego regionu Azure stosu. Ta nazwa jest używana jako część nazwy FQDN dla publicznych wirtualne adresy IP (VIP) zarządzanych przez stos Azure. Zazwyczaj nazwa regionu powinien być identyfikator lokalizacji fizycznej, takie jak lokalizacja centrum danych. | 
| Nazwy domen zewnętrznych | Nazwa strefy systemu nazw domen (DNS, Domain Name System) dla punktów końcowych z zewnątrz adresów VIP. Używany w nazwie FQDN dla te publiczne adresy VIP. | 
| Nazwy prywatnych domeny (wewnętrzny) | Nazwa domeny (i wewnętrznej strefy DNS), utworzone na stosie Azure infrastruktury zarządzania. 
| | |

## <a name="certificate-requirements"></a>Wymagania dotyczące certyfikatów

Wdrożenia musisz podać certyfikatów Secure Sockets Layer (SSL) dla publicznych punktów końcowych. Na wysokim poziomie certyfikaty mają następujące wymagania:

> [!IMPORTANT]
> Informacje o certyfikacie w tym artykule jest dostępna wyłącznie jako ogólne wytyczne. Przed nabyciem wszelkich certyfikatów stosu Azure skontaktować się z partnerem sprzętu OEM. Użytkownicy muszą podać szczegółowe wskazówki dotyczące certyfikatu i wymagań.

- Można użyć certyfikatu jeden symbol wieloznaczny lub korzystanie z zestawu dedykowanych certyfikatów i użyć symboli wieloznacznych tylko dla punktów końcowych, takie jak magazyn i magazyn kluczy.
- Certyfikaty musi być wystawiony przez publiczny zaufanego urzędu certyfikacji (CA) lub urzędu certyfikacji zarządzany przez klienta.
 
W poniższej tabeli przedstawiono usługi i liczbę publicznych punktów końcowych, które wymagają certyfikatów dla pierwszego wdrożenia stosu Azure. 

| Używany do | Endpoint 
| -------- | ------------- | 
| Usługa Azure Resource Manager (administrator) | adminmanagment. kolumny [region]. [external_domain] |
| Usługa Azure Resource Manager (użytkownika) | Zarządzanie. kolumny [region]. [external_domain] |
| Portal (administrator) | adminportal. kolumny [region]. [external_domain] |
| Portal (użytkownika) | Portal. kolumny [region]. [external_domain] |
| Magazyn kluczy (użytkownika) | &#42;. Magazyn. kolumny [region]. [external_domain] |
| Magazyn kluczy (administrator) | &#42;. adminvault. kolumny [region]. [external_domain] |
| Magazyn | &#42;. Obiekt blob. kolumny [region]. [external_domain]<br>&#42;. Tabela. kolumny [region]. [external_domain]<br>&#42;. kolejki. kolumny [region]. [external_domain]  |
| Wykres ** | wykres. kolumny [region]. [external_domain] |
| USŁUGI AD FS ** | usługi AD FS. kolumny [region]. [external_domain] |
| | |

** Certyfikaty dla punktów końcowych wykresu i usług AD FS są wymagane tylko w przypadku wdrożeń usług AD FS.

Jeśli chcesz użyć certyfikatu jeden symbol wieloznaczny, należy łącznie nazwy alternatywnej sześciu podmiotu (SAN) dla pierwszego wdrożenia stosu Azure. Te SAN są: 

- &#42;. kolumny [region]. [external_domain]
- &#42;. Magazyn. kolumny [region]. [external_domain]
- &#42;. adminvault. kolumny [region]. [external_domain]
- &#42;. Obiekt blob. kolumny [region]. [external_domain]
- &#42;. Tabela. kolumny [region]. [external_domain] 
- &#42;. kolejki. kolumny [region]. [external_domain]

## <a name="time-synchronization"></a>Czas synchronizacji

Serwer czasu stosu Azure należy zsynchronizować z serwerem czas zewnętrzny, który jest rozpoznawalna za pomocą adresu IP. Czas server w sieci firmowej jest wymagane do wdrożenia bez połączenia.

## <a name="network-connectivity"></a>Połączenie sieciowe

### <a name="dns-integration"></a>Integracja z usługą DNS

Do rozpoznawania nazw DNS zewnętrznych ze stosu Azure (na przykład www.bing.com), należy podać serwerów DNS, które stosu Azure służy do przekazywania żądań DNS, dla których nie jest autorytatywny stosu Azure. Jako dane wejściowe wdrożenia musisz podać co najmniej dwóch serwerów do użycia jako usług przesyłania dalej DNS odporność na uszkodzenia.

Do rozpoznawania nazw DNS punktów końcowych stosu Azure z zewnętrznego stosu Azure (na przykład z las firmowy), można zintegrować z serwerów DNS obsługujących zewnętrznych strefę DNS stosu Azure z serwerów DNS obsługujących strefę nadrzędną, który ma być używany. To wymaga rozpoznawania nazw DNS między stosu Azure i istniejących stref DNS w centrum danych. Aby to zrobić, będą używane metody takie jak warunkowego przesyłania dalej lub strefy delegowania. Firma Microsoft zaleca warunkowego przesyłania dalej, jeśli masz bezpośrednią kontrolę nad serwerów DNS obsługujących strefę nadrzędną dla przestrzeni nazw DNS z zewnętrznego stosu Azure. (Jeśli zewnętrznych strefy DNS stosu Azure wygląda jak domeny podrzędnej z nazwą domeny firmy (na przykład azurestack.contoso.com i contoso.com), możesz użyć delegowania strefy.

### <a name="network-infrastructure"></a>Infrastruktura sieciowa

Infrastruktura sieci Azure stosu składa się z kilku sieci logiczne, które są skonfigurowane na przełączników. Na poniższym diagramie przedstawiono te sieci logiczne oraz sposób ich integracji z top-of-rack (TOR), kontrolera zarządzania płytą główną (BMC) i obramowania przełączników (sieci klienta).

![Połączenia diagram i przełącznik sieci logicznej](media/azure-stack-planning-considerations/NetworkDiagram.png)

W poniższej tabeli przedstawiono sieci logiczne i skojarzone zakresy podsieci IPv4, które należy zaplanować.

| Sieć logiczna | Opis | Rozmiar | 
| -------- | ------------- | ------------ | 
| Publiczny adres VIP | Publiczne adresy IP niewielki zestaw usług Azure stosu, z resztą używanych przez maszyny wirtualne dzierżawcy. Infrastruktura stosu Azure używa 32 adresów z tą siecią. Jeśli zamierzasz korzystać z usługi aplikacji i dostawców zasobów SQL, używa więcej 7. | / 26 (62 hostów) - /22 (1022 hostów)<br><br>Zalecane = prefiksie/24 (254 hostów) | 
| Infrastruktura przełączników | Point-to-Point adresy IP do celów routingu dedykowanego przełącznika interfejsów zarządzania i adresy sprzężenia zwrotnego przypisane do tego przełącznika. | /26 | 
| Infrastruktura | Używane składniki wewnętrzne stosu Azure do komunikacji. | /24 |
| Prywatne | Używane dla sieci magazynu i prywatne adresy VIP. | /24 | 
| BMC | Używany do komunikacji ze składnikami bmc na hostach fizycznych. | /27 | 
| | | |

### <a name="uplink-to-border-switches"></a>Pasma przełącznikami obramowania

Będziesz potrzebować pasma skonfigurowany tak, aby przełączniki obramowania w centrum danych. Można przekierować ruch warstwy 3 z przełączników top of rack (TOR), które są częścią systemu Azure stosu zintegrowane przy użyciu jednej z następujących metod:

- Protokół Border Gateway Protocol (BGP) 
- routing statyczny

Firma Microsoft zaleca protokołu BGP, ponieważ dzięki automatycznej aktualizacji tras, na których są publikowane przez oprogramowanie obciążenia równoważenia multiplekser (Programowego MUX) do sieci zewnętrznych. Jest to ważne, jeśli dodasz zakresy publicznych adresów IP po wdrożeniu. Jeśli to zrobisz BGP komunikacji równorzędnej z przełączniki TOR do agregacji przełącznika czy przełączniki TOR są połączone, publiczny adres IP zakresu, Dodaj powdrożeniowe są automatycznie anonsowane do agregacji przełącznika bez konieczności ręcznej interwencji. Jeśli korzystasz z routingiem statycznym, należy ręcznie zaktualizować trasy dla nowych zakresów, za każdym razem Dodaj publiczny blok adresów IP.

### <a name="proxy-server"></a>Serwer proxy

Stos Azure obsługuje tylko serwery przezroczystego obiektu pośredniczącego. Przezroczystego obiektu pośredniczącego przechwytuje żądania w warstwie sieci bez konieczności żadnej konfiguracji specjalnych klienta.

### <a name="publish-azure-stack-services"></a>Publikowanie usług Azure stosu

Należy udostępnić usług Azure stosu użytkowników z zewnętrznego stosu Azure. Stos Azure konfiguruje różnych punktów końcowych dla jego role infrastruktury. Te punkty końcowe są przypisywane wirtualne adresy IP z puli publicznych adresów IP. Wpis DNS jest tworzony dla każdego punktu końcowego w zewnętrznych strefę DNS, który określono w czasie wdrażania. Na przykład aplikacji portal użytkowników jest przypisany wpisu hosta DNS "portalu. <*region*>. <*external_FQDN*>." 

#### <a name="ports-and-urls"></a>Porty i adresy URL

Aby usługi Azure stosu (takich jak portali usługi Azure Resource Manager, DNS, itp) dostępny dla zewnętrznych sieci, musisz zezwolić na ruch przychodzący z tymi punktami końcowymi dla określonych adresów URL, porty i protokoły.
 
W przypadku wdrożenia w przypadku, gdy pasma przezroczystego obiektu pośredniczącego, aby serwer proxy tradycyjnych należy zezwolić na określone porty i adresy URL dla komunikacji wychodzącej. Obejmują one porty i adresy URL dla tożsamości, zespolonego marketplace poprawek i aktualizacji, rejestracji i dane użycia.

Aby uzyskać więcej informacji, zobacz:
- [Przychodzący portów i protokołów](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-protocols-inbound)
- [Adresy URL i portów wychodzących](https://docs.microsoft.com/azure/azure-stack/azure-stack-integrate-endpoints#ports-and-urls-outbound)

### <a name="connect-azure-stack-to-azure"></a>Azure stosu połączenia z platformą Azure

W scenariuszach chmur hybrydowych należy zaplanować, jak chcesz się połączyć stosu Azure na platformie Azure. Istnieją dwie metody obsługiwanych nawiązać sieci wirtualnych Azure stosu sieci wirtualnych na platformie Azure: 
- **Lokacja lokacja**. Połączenie wirtualnej sieci prywatnej (VPN) za pośrednictwem protokołu IPsec (IKE v1 i IKE v2). Ten typ połączenia wymaga urządzenia VPN lub usługi Routing i dostęp zdalny (RRAS). Aby uzyskać więcej informacji na temat bram sieci VPN platformy Azure, zobacz [o bramy sieci VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways). Komunikacja przez ten tunel jest zaszyfrowany i jest bezpieczne. Jednak przepustowość jest ograniczone przez maksymalną przepływność tunelu (100 – 200 MB/s).
- **NAT ruchu wychodzącego**. Domyślnie wszystkie maszyny wirtualne Azure stosu będzie połączeniem do sieci zewnętrznej za pośrednictwem wychodzącego translatora adresów sieciowych. Każda sieć wirtualna, która jest tworzona w stosie usługi Azure pobiera publicznego adresu IP przypisane do niej. Czy maszyny wirtualnej bezpośrednio przypisano publiczny adres IP lub jest za modułem równoważenia obciążenia z publicznym adresem IP, jej wychodzący dostęp za pośrednictwem NAT ruchu wychodzącego przy użyciu adresu VIP sieci wirtualnej. To działanie jest tylko do komunikacji, który jest inicjowane przez maszynę wirtualną i przeznaczonych dla zewnętrznych sieci (w Internecie lub intranecie). Nie można nawiązać połączenia z maszyną wirtualną spoza.

#### <a name="hybrid-connectivity-options"></a>Opcje połączeń hybrydowych

Połączenie hybrydowe z należy wziąć pod uwagę rodzaj wdrożenia, które chcesz zaoferować i którym zostanie wdrożony. Należy wziąć pod uwagę tego, czy należy do izolacji ruchu sieciowego dla poszczególnych dzierżawców i czy masz wdrożenia intranetu lub Internetu.

- **Pojedynczej dzierżawy Azure stosu**. Wdrożenia usługi Azure stosu wygląda, co najmniej z punktu widzenia sieci, jak w przypadku jednego dzierżawcy. Może być się, że dzierżawy wiele subskrypcji, ale jak intranetową usługę, cały ruch przesyłany tej samej sieci. Ruch sieciowy z jedną subskrypcją przesyłane w tym samym połączenia sieciowego w postaci innej subskrypcji i nie muszą być izolowane za pośrednictwem tunelu zaszyfrowane.

- **Wielodostępne Azure stosu**. Wdrożenia stosu Azure, w której ruch każdej subskrypcji dzierżawcy, przeznaczony dla sieci, które są zewnętrzne względem stosu Azure musi być odizolowany od ruchu sieciowego innych dzierżawców.
 
- **Intranet wdrożenia**. Wdrożenia stosu Azure, która znajduje się w intranecie firmowym, zwykle w prywatnej przestrzeni adresów IP i co najmniej jeden zapory. Publiczne adresy IP nie są naprawdę publiczne, ponieważ nie można go skierować bezpośrednio za pośrednictwem publicznej sieci internet.

- **Wdrożenie Internet**. Wdrożenia stosu Azure jest podłączony do publicznego Internetu i używa routingu internetowego publicznego adresu IP adresy dla publicznych zakresu adresów VIP. Wdrożenia można nadal znajdują się za zaporą, ale publicznego zakresu adresów VIP jest bezpośrednio dostępny z publicznego Internetu i Azure.
 
W poniższej tabeli przedstawiono scenariusze łączności hybrydowych, specjalistów, wad i przypadki użycia.

| Scenariusz | Połączenie — metoda | Specjaliści | Wady | Nadaje się do |
| -- | -- | --| -- | --|
| Pojedynczy dzierżawy Azure stosu, wdrożenie w sieci intranet | NAT ruchu wychodzącego | Lepszą przepustowość transferów szybsze. Prosta implementacja; nie wymaganych bram. | Ruch nie jest szyfrowany. Brak izolacji i szyfrowania poza TOR. | Gdzie są równie zaufane wszystkich dzierżaw wdrożeń w przedsiębiorstwie.<br><br>Przedsiębiorstw, które mają obwodu Azure ExpressRoute na platformie Azure. |
| Wielodostępne Azure stosu, wdrożenie w sieci intranet | Sieci VPN typu lokacja lokacja | Ruch z sieci wirtualnej dzierżawcy do lokalizacji docelowej jest bezpieczna. | Przepustowość jest ograniczona przez tunel VPN lokacja lokacja.<br><br>Wymaga bramy sieci wirtualnej i urządzenia sieci VPN w sieci docelowej. | Musi być zabezpieczona przedsiębiorstwach, gdzie niektóre dzierżawy ruchu od pozostałych dzierżawców. |
| Pojedynczy dzierżawy Azure stosu, internet wdrożenia | NAT ruchu wychodzącego | Lepszą przepustowość transferów szybsze. | Ruch nie jest szyfrowany. Brak izolacji i szyfrowania poza TOR. | Hosting scenariuszy, w którym dzierżawcy pobiera własnych wdrożenia stosu Azure i obwodu dedykowanego w środowisku Azure stosu. Na przykład ExpressRoute, jak i przełączania etykiety Multiprotocol (MPLS).
| Wielodostępne Azure stosu, internet wdrożenia | Sieci VPN typu lokacja lokacja | Ruch z sieci wirtualnej dzierżawcy do lokalizacji docelowej jest bezpieczna. | Przepustowość jest ograniczona przez tunel VPN lokacja lokacja.<br><br>Wymaga bramy sieci wirtualnej i urządzenia sieci VPN w sieci docelowej. | Scenariuszach, w którym chce oferują chmury wielodostępne dostawcy hostingu, gdzie dzierżawcy nie ufa siebie i ruchu muszą być szyfrowane.
|  |  |  |  |  |

#### <a name="using-expressroute"></a>Przy użyciu usługi ExpressRoute

Możesz połączyć stosu Azure na platformie Azure za pośrednictwem [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) dla pojedynczej dzierżawy w sieci intranet i scenariuszy z wieloma dzierżawcami. Będziesz potrzebować elastycznie obwodu ExpressRoute za pośrednictwem [dostawca połączenia](https://docs.microsoft.com/azure/expressroute/expressroute-locations).

Na poniższym diagramie przedstawiono ExpressRoute dla scenariusza pojedynczej dzierżawy (gdzie "Połączenie klienta" jest obwodu ExpressRoute).

![Diagram przedstawiający pojedynczej dzierżawy usługi ExpressRoute scenariusza](media/azure-stack-planning-considerations/ExpressRouteSingleTenant.PNG)

Na poniższym diagramie przedstawiono ExpressRoute dla scenariusza wielodostępnej.

![Diagram przedstawiający wielodostępne ExpressRoute scenariusza](media/azure-stack-planning-considerations/ExpressRouteMultiTenant.PNG)

## <a name="external-monitoring"></a>Monitorowania zewnętrznych
Uzyskanie pojedynczego widoku wszystkie alerty z urządzeń i wdrożenia stosu Azure i integrowania alerty istniejących IT usługi przepływu pracy zarządzania dla biletów stosu Azure można zintegrować z zewnętrznego datacenter monitorowanie rozwiązań.

Dołączone do rozwiązania Azure stosu, host cyklu życia sprzętu jest komputera znajdującego się poza stosu Azure, który uruchamia narzędzia do zarządzania dostarczonym przez producenta OEM dla sprzętu. Można użyć tych narzędzi lub innych rozwiązań, które bezpośrednio zintegrować z istniejącymi rozwiązaniami monitorowania w centrum danych.

W poniższej tabeli przedstawiono listę dostępnych opcji.

| Obszar | Rozwiązanie monitorowania zewnętrznych |
| -- | -- |
| Azure stos oprogramowania | - [Pakiet administracyjny stosu Azure dla programu Operations Manager](https://azure.microsoft.com/blog/management-pack-for-microsoft-azure-stack-now-available/)<br>- [Wtyczka Nagios](https://exchange.nagios.org/directory/Plugins/Cloud/Monitoring-AzureStack-Alerts/details)<br>-Opartego na interfejsie REST wywołań interfejsu API | 
| Serwery fizyczne (bmc za pomocą interfejsu IPMI) | -Pakiet administracyjny programu operations Manager dostawcy<br>-Rozwiązanie dostarczonym sprzętowe przez producenta OEM<br>-Dostawca sprzętu wtyczek Nagios | Obsługiwany przez producentów OEM partnera — monitorowanie rozwiązania (włączone) | 
| Urządzenia sieciowe (SNMP) | -Odnajdywania urządzeń sieciowych programu operations Manager<br>-Rozwiązanie dostarczonym sprzętowe przez producenta OEM<br>-Przełącznika Nagios wtyczki |
| Monitorowanie kondycji subskrypcji dzierżawcy | - [Pakiet administracyjny programu System Center dla systemu Windows Azure](https://www.microsoft.com/download/details.aspx?id=50013) | 
|  |  | 

Należy uwzględnić następujące wymagania:
- Rozwiązanie, którego używasz, musi być bez wykorzystania agentów. Nie można zainstalować agentów innych firm wewnątrz składniki stosu Azure. 
- Jeśli chcesz użyć programu System Center Operations Manager wymaga programu Operations Manager 2012 R2 lub Operations Manager 2016.

## <a name="backup-and-disaster-recovery"></a>Kopia zapasowa i odzyskiwanie po awarii

Planowanie tworzenia kopii zapasowej i odzyskiwanie po awarii obejmuje planowanie zarówno podstawowej stosu Azure infrastrukturą służącą do hostowania maszyn wirtualnych IaaS i PaaS usług i aplikacji dzierżawy i danych. Należy zaplanować je oddzielnie.

### <a name="protect-infrastructure-components"></a>Ochrona składników infrastruktury

Stos Azure wykonuje kopię zapasową składników infrastruktury do udziału, który określisz.

- Będziesz potrzebować zewnętrznych udziału plików SMB na istniejącym serwerze plików z systemem Windows lub urządzenia innych firm.
- Należy używać tego samego udziału kopii zapasowych przełączników sieciowych i sprzętowych host cyklu życia. Dostawca sprzętu OEM pomoże zawierają wskazówki dotyczące tworzenia kopii zapasowej i przywracania tych składników, są one zewnętrznego stosu Azure. Możesz teraz odpowiada za uruchamianie tworzenia kopii zapasowej przepływy pracy na podstawie zalecenia producenta OEM.

Jeśli dojdzie do utraty danych w wyniku katastrofy, możesz użyć kopii zapasowej infrastruktury ponowne umieszczanie wdrożenia danych, takich jak wdrożenia danych wejściowych oraz identyfikatorów kont usług, a certyfikat główny urzędu certyfikacji, zasobów federacyjnych (w przypadku wdrożeń rozłączona), planów, oferty, Subskrypcje, przydziały RBAC roli zasad i przypisań i kluczy tajnych magazynu kluczy.
 
### <a name="protect-tenant-applications-on-iaas-virtual-machines"></a>Ochrona aplikacji dzierżawy w przypadku maszyn wirtualnych IaaS

Azure stosu nie kopii dzierżawy aplikacji oraz danych. Należy zaplanować ochrony odzyskiwania kopii zapasowych i odzyskiwaniem po awarii do obiektu docelowego zewnętrznego stosu Azure. Ochrona dzierżawy jest czynnością oparte na dzierżawy. W przypadku maszyn wirtualnych IaaS dzierżawcy mogą używać technologii gościa do ochrony plików, folderów, stanu systemu oraz danych aplikacji. Jednak jako dostawcę enterprise lub usługi, może być konieczne stanowią rozwiązanie tworzenia kopii zapasowych i odzyskiwania w tym samym centrum danych lub zewnętrznie w chmurze.

Do tworzenia kopii zapasowych maszyn wirtualnych systemu Linux lub Windows IaaS, należy użyć kopii zapasowej produkty z dostępu do systemu operacyjnego gościa do ochrony pliku, folderu, stan systemu operacyjnego i danych aplikacji. Kopia zapasowa Azure, System Center Data Center Protection Manager, można użyć lub obsługiwane produkty innych firm.

Replikowanie danych w dodatkowej lokalizacji i organizowania aplikacji w tryb failover w przypadku wystąpienia awarii, można użyć usługi Azure Site Recovery lub obsługiwanych produkty innych firm. (W początkowej wersji systemów zintegrowane usługi Azure Site Recovery nie obsługuje powrót po awarii. Jednak można osiągnąć powrotu po awarii w procesie ręcznego.) Ponadto aplikacje, które obsługują macierzysty replikacji (takich jak Microsoft SQL Server) może replikować danych do innej lokalizacji, w którym jest uruchomiona aplikacja.

> [!IMPORTANT]
> W początkowej wersji systemów zintegrowane firma Microsoft będzie obsługiwać technologii ochrony, które działają na poziomie gościa maszyny wirtualnej IaaS. Nie można zainstalować agentów na serwerach infrastruktury podstawowej.

## <a name="next-steps"></a>Następne kroki

- Informacje o przypadki użycia, zakupu partnerami i dostawcami sprzętu OEM, zobacz [stosu Azure](https://azure.microsoft.com/overview/azure-stack/) stronę produktu.
- Informacji o plan i dostępności geograficznie stosu Azure zintegrowanych systemów, zobacz oficjalny dokument: [stosu Azure: rozszerzenie Azure](https://azure.microsoft.com/resources/azure-stack-an-extension-of-azure/). 
