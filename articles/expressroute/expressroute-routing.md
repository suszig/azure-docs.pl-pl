<properties
   pageTitle="Wymagania dotyczące routingu dla usługi ExpressRoute | Microsoft Azure"
   description="Ta strona zawiera szczegółowe wymagania dotyczące konfigurowania routingu oraz zarządzania nim na potrzeby obwodów usługi ExpressRoute."
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="rossort"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/16/2016"
   ms.author="ganesr"/>



# Wymagania dotyczące routingu w usłudze ExpressRoute  

Aby połączyć się z usługami w chmurze firmy Microsoft przy użyciu usługi ExpressRoute, konieczne będzie skonfigurowanie routingu oraz zarządzanie nim. Niektórzy dostawcy połączenia oferują konfigurowanie routingu oraz zarządzanie nim jako usługą zarządzaną. Skontaktuj się z dostawcą połączenia, aby sprawdzić, czy taka usługa jest oferowana. Jeśli nie, musisz spełnić wymagania opisane poniżej. 

Opis sesji routingu, które należy skonfigurować w celu ułatwienia łączności, znajduje się w artykule [Circuits and routing domains](expressroute-circuit-peerings.md) (Obwody i domeny routingu).

**Uwaga:** firma Microsoft nie obsługuje protokołów nadmiarowości routerów (np. HSRP, VRRP) dla konfiguracji wysokiej dostępności. Polegamy na nadmiarowej parze sesji protokołu BGP na komunikację równorzędną w celu zapewnienia wysokiej dostępności.

## Adresy IP dla komunikacji równorzędnej

Należy zarezerwować kilka bloków adresów IP, aby skonfigurować routing między siecią i routerami MSEE. Ta sekcja zawiera listę wymagań i opis zasad dotyczących sposobu pozyskiwania i użycia tych adresów IP.

### Adresy IP dla prywatnej komunikacji równorzędnej Azure

Do konfigurowania komunikacji równorzędnej można użyć prywatnych lub publicznych adresów IP. Zakres adresów używany do konfigurowania tras nie może pokrywać się z zakresami adresów używanymi do tworzenia sieci wirtualnych na platformie Azure. 

 - Należy zarezerwować podsieć /29 lub dwie podsieci /30 dla interfejsów routingu.
 - Podsieci używane do routingu mogą być prywatnymi adresami IP lub publicznymi adresami IP.
 - Podsieci nie mogą powodować konfliktu z zakresem zarezerwowanym przez klienta do użycia w chmurze firmy Microsoft.
 - Jeśli zostanie użyta podsieć /29, zostanie ona podzielona na dwie podsieci /30. 
     - Pierwsza podsieć /30 zostanie użyta do linku podstawowego, a druga do linku dodatkowego.
     - Dla każdej podsieci /30 należy użyć pierwszego adresu IP podsieci /30 na routerze. Firma Microsoft użyje drugiego adresu IP podsieci /30 do skonfigurowania sesji protokołu BGP.
     - Aby zapewnić ważność [umowy SLA dotyczącej dostępności](https://azure.microsoft.com/support/legal/sla/), musisz skonfigurować obie sesje protokołu BGP.  

#### Przykład prywatnej komunikacji równorzędnej

Jeśli do skonfigurowania komunikacji równorzędnej zdecydujesz się użyć podsieci a.b.c.d/29, zostanie ona podzielona na dwie podsieci /30. W poniższym przykładzie przedstawiono, jak używa się podsieci a.b.c.d/29. 

Podsieć a.b.c.d/29 zostanie podzielona na podsieci a.b.c.d/30 i a.b.c.d+4/30 i przekazana firmie Microsoft za pośrednictwem interfejsów API obsługi administracyjnej. Użytkownik użyje podsieci a.b.c.d+1 jako adresu IP VRF usługi Primary PE, a firma Microsoft użyje podsieci a.b.c.d+2 jako adresu IP VRF podstawowego rozwiązania MSEE. Użytkownik użyje podsieci a.b.c.d+5 jako adresu IP VRF dodatkowej usługi PE, a firma Microsoft użyje podsieci a.b.c.d+6 jako adresu IP VRF dodatkowego rozwiązania MSEE.

Rozważ sytuację, w której w celu skonfigurowania prywatnej komunikacji równorzędnej wybierzesz podsieć 192.168.100.128/29. 192.168.100.128/29 obejmuje adresy od 192.168.100.128 do 192.168.100.135, z których:

- Adres 192.168.100.128/30 zostanie przypisany do linku link1, podczas gdy dostawca będzie używać 192.168.100.129, a firma Microsoft będzie używać 192.168.100.130.
- Adres 192.168.100.132/30 zostanie przypisany do linku link2, podczas gdy dostawca będzie używać adresu 192.168.100.133, a firma Microsoft będzie używać adresu 192.168.100.134.

### Adresy IP dla publicznej komunikacji równorzędnej Azure i komunikacji równorzędnej firmy Microsoft

Do konfigurowania sesji protokołu BGP należy użyć posiadanych publicznych adresów IP. Firma Microsoft musi mieć możliwość weryfikacji własności adresów IP za pomocą rejestrów internetowego routingu i internetowych rejestrów routingu. 

- Należy użyć unikatowej podsieci /29 lub dwóch podsieci /30 do skonfigurowania komunikacji równorzędnej protokołu BGP dla każdego wystąpienia komunikacji równorzędnej na obwód usługi ExpressRoute (jeśli używasz więcej niż jednego obwodu). 
- Jeśli zostanie użyta podsieć /29, zostanie ona podzielona na dwie podsieci /30. 
    - Pierwsza podsieć /30 zostanie użyta do linku podstawowego, a druga do linku dodatkowego.
    - Dla każdej podsieci /30 należy użyć pierwszego adresu IP podsieci /30 na routerze. Firma Microsoft użyje drugiego adresu IP podsieci /30 do skonfigurowania sesji protokołu BGP.
    - Aby zapewnić ważność [umowy SLA dotyczącej dostępności](https://azure.microsoft.com/support/legal/sla/), musisz skonfigurować obie sesje protokołu BGP.

Zadbaj o to, by adres IP i numer AS zostały zarejestrowane na Ciebie w jednym z wymienionych poniżej rejestrów.

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)


## Wymiana tras dynamicznych

Wymiana routingu będzie odbywać się za pośrednictwem protokołu eBGP. Sesje eBGP są ustanawiane między rozwiązaniami MSEE a routerami użytkownika. Uwierzytelnianie sesji BGP nie jest wymagane. W razie potrzeby można skonfigurować skrót MD5. Informacje na temat konfigurowania sesji BGP znajdują się w artykułach [Configure routing](expressroute-howto-routing-classic.md) (Konfigurowanie routingu) i [Circuit provisioning workflows and circuit states](expressroute-workflows.md) (Przepływy pracy inicjowania obsługi obwodu i stany obwodu).

## Numery systemu autonomicznego

Firma Microsoft będzie używać numeru AS 12076 do publicznej i prywatnej komunikacji równorzędnej Azure oraz komunikacji równorzędnej Microsoft. Zarezerwowaliśmy numery AS od 65515 do 65520 do użytku wewnętrznego. Obsługiwane są zarówno 16-, jak i 32-bitowe numery AS. Po stronie równorzędnej (klient lub dostawca) numer AS może być publicznym adresem ASN, jeśli można zweryfikować, że należy do Ciebie, lub prywatnym numerem ASN.

Nie istnieją żadne wymagania związane z symetrią transferu danych w podstawowych i dodatkowych ścieżkach dowolnego obwodu. Ścieżki przekazywania dalej i ścieżki zwracania mogą przechodzić różne pary routerów. Trasy identyczne muszą być anonsowane ze strony podstawowej lub dodatkowej we wszystkich danych parach obwodów należących do Ciebie. Metryki tras nie muszą być identyczne.

## Agregacja tras i limity prefiksów

Firma Microsoft obsługuje do 4000 prefiksów anonsowanych nam za pośrednictwem prywatnej komunikacji równorzędnej Azure. Tę liczbę można zwiększyć do 10 000 prefiksów, jeśli zostanie włączony dodatek Premium usługi ExpressRoute. Akceptujemy do 200 prefiksów na sesję protokołu BGP dla publicznej komunikacji równorzędnej Azure i komunikacji równorzędnej Microsoft. 

Sesja protokołu BGP zostanie przerwana, jeśli liczba prefiksów przekroczy limit. Będziemy akceptować domyślne trasy tylko dla linku prywatnej komunikacji równorzędnej. Dostawca lub klient musi odfiltrować trasę domyślną i prywatne adresy IP (RFC 1918) z anonsów BGP do ścieżek publicznej komunikacji równorzędnej Azure i komunikacji równorzędnej Microsoft. 

## Routing tranzytowy i routing obejmujący wiele regionów

Usługi ExpressRoute nie można skonfigurować jako routera tranzytowego. W kwestii usług routingu tranzytowego trzeba polegać na dostawcy połączenia.

## Anonsowanie tras domyślnych

Trasy domyślne są dozwolone tylko w sesjach prywatnej komunikacji równorzędnej Azure. W takim przypadku firma Microsoft będzie kierować cały ruch ze skojarzonych sieci wirtualnych do sieci użytkownika. Anonsowanie tras domyślnych do prywatnej komunikacji równorzędnej spowoduje zablokowanie ścieżki internetowej z platformy Azure. Aby przekierować ruch do i z Internetu w przypadku usług hostowanych na platformie Azure, konieczne jest zastosowanie krawędzi sieci firmowej. 

 Aby można było włączyć łączność z innymi usługami i usługami infrastruktury Azure, musi być spełniony jeden z poniższych warunków:

 - W publicznej komunikacji równorzędnej Azure jest włączone przekierowywanie ruchu do publicznych punktów końcowych.
 - Używasz routingu zdefiniowanego przez użytkownika, aby zezwolić na połączenie z Internetem dla każdej podsieci wymagającej takiego połączenia.

**Uwaga:** anonsowanie tras domyślnych spowoduje awarię aktywacji licencji maszyn wirtualnych systemu Windows i innych systemów. Aby obejść ten problem, postępuj zgodnie z instrukcjami zamieszczonymi [tutaj](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx).

## Obsługa protokołu BGP społeczności (wersja zapoznawcza)


W tej sekcji przedstawiono sposób korzystania z protokołu BGP społeczności w usłudze ExpressRoute. Firma Microsoft będzie anonsować trasy w ścieżkach publicznej komunikacji równorzędnej oraz komunikacji równorzędnej firmy Microsoft za pomocą tras oznaczonych odpowiednimi wartościami społeczności. Uzasadnienie takiego postępowania oraz szczegółowe informacje dotyczące wartości społeczności zostały opisane poniżej. Firma Microsoft nie uzna jednak żadnych wartości społeczności oznaczonych do tras anonsowanych do firmy Microsoft.

Jeśli łączysz się z firmą Microsoft za pośrednictwem usługi ExpressRoute w dowolnej lokalizacji komunikacji równorzędnej w regionie geopolitycznym, będziesz mieć dostęp do wszystkich usług w chmurze firmy Microsoft we wszystkich regionach w ramach granic geopolitycznych. 

Na przykład po połączeniu się z firmą Microsoft w Amsterdamie za pośrednictwem usługi ExpressRoute będziesz mieć dostęp do wszystkich usług w chmurze Microsoft obsługiwanych w Europie Północnej i Europie Zachodniej. 

Zapoznaj się ze stroną [ExpressRoute partners and peering locations](expressroute-locations.md) (Partnerzy i lokalizacje komunikacji równorzędnej usługi ExpressRoute), która zawiera szczegółową listę regionów geopolitycznych, powiązanych regionów świadczenia usługi Azure i odpowiadających im lokalizacji komunikacji równorzędnej usługi ExpressRoute.

Możesz kupić więcej niż jeden obwód usługi ExpressRoute na region geopolityczny. Wiele ofert połączeń daje znaczące korzyści w zakresie wysokiej dostępności z powodu nadmiarowości geograficznej. Jeśli masz wiele obwodów usługi ExpressRoute, otrzymasz ten sam zestaw prefiksów anonsowanych z firmy Microsoft w publicznej komunikacji równorzędnej i w ścieżkach komunikacji równorzędnej Microsoft. Oznacza to, że będziesz mieć wiele ścieżek ze swojej sieci do firmy Microsoft. Może to teoretycznie spowodować podjęcie nieoptymalnych decyzji w zakresie routingu w ramach sieci. W efekcie połączenia z różnymi usługami mogą nie być optymalne. 

Firma Microsoft będzie oznaczać prefiksy anonsowane za pośrednictwem publicznej komunikacji równorzędnej oraz komunikacji równorzędnej Microsoft odpowiednimi wartościami protokołu BGP społeczności wskazującymi region, w którym są hostowane prefiksy. Wartości społeczności gwarantują, że zostaną podjęte odpowiednie decyzje w kwestii routingu i klienci będą mieli zapewniony [optymalny routing](expressroute-optimize-routing.md).

| **Region geopolityczny** | **Region świadczenia usługi Microsoft Azure** | **Wartość społeczności BGP** |
|---|---|---|
| **Ameryka Północna** |    |  |
|    | Wschodnie stany USA | 12076:51004 |
|    | Wschodnie stany USA 2 | 12076:51005 |
|    | Zachodnie stany USA | 12076:51006 |
|    | Zachodnie stany USA 2 | 12076:51026 |
|    | Środkowo-zachodnie stany USA | 12076:51027 |
|    | Środkowo-północne stany USA | 12076:51007 |
|    | Środkowo-południowe stany USA | 12076:51008 |
|    | Środkowe stany USA | 12076:51009 |
|    | Kanada Środkowa | 12076:51020 |
|    | Kanada Wschodnia | 12076:51021 |
| **Ameryka Południowa** |  |  |
|    | Brazylia Południowa | 12076:51014 |
| **Europa** |    |  |
|    | Europa Północna | 12076:51003 |
|    | Europa Zachodnia | 12076:51002 |
| **Azja i Pacyfik** |    |   |
|    | Azja Wschodnia | 12076:51010 |
|    | Azja Południowo-Wschodnia | 12076:51011 |
| **Japonia** |     |   |
|    | Japonia Wschodnia | 12076:51012 |
|    | Japonia Zachodnia | 12076:51013 |
| **Australia** |    |   | 
|    | Australia Wschodnia | 12076:51015 |
|    | Australia Południowo-Wschodnia | 12076:51016 |
| **Indie** |    |   |
|    | Indie Południowe | 12076:51019 |
|    | Indie Zachodnie | 12076:51018 |
|    | Indie Środkowe | 12076:51017 |

Wszystkie trasy anonsowane przez firmę Microsoft zostaną oznaczone odpowiednią wartością społeczności. 

>[AZURE.IMPORTANT] Globalne prefiksy zostaną oznaczone odpowiednią wartością społeczności i będą anonsowane tylko po włączeniu dodatku Premium usługi ExpressRoute.


Oprócz tego firma Microsoft oznaczy również prefiksy w oparciu o usługę, do której się odnoszą. Dotyczy to tylko komunikacji równorzędnej firmy Microsoft. Poniższa tabela zawiera mapowanie usługi do wartości społeczności BGP.

| **Usługa** | **Wartość społeczności BGP** |
|---|---|
| **Exchange** | 12076:5010 |
| **Sharepoint** | 12076:5020 |
| **Skype dla firm** | 12076:5030 |
| **CRM Online** | 12076:5040 |
| **Inne usługi Office 365** | 12076:5100 |

>[AZURE.NOTE] Firma Microsoft nie uznaje żadnych wartości społeczności BGP ustawionych na trasach anonsowanych do firmy Microsoft.

## Następne kroki

- Skonfiguruj połączenie usługi ExpressRoute.

    - [Create an ExpressRoute circuit for the classic deployment model](expressroute-howto-circuit-classic.md) (Tworzenie obwodu usługi ExpressRoute dla klasycznego modelu wdrażania) lub [Create and modify an ExpressRoute circuit using Azure Resource Manager](expressroute-howto-circuit-arm.md) (Tworzenie i modyfikowanie obwodu usługi ExpressRoute za pomocą usługi Azure Resource Manager)
    - [Configure routing for the classic deployment model](expressroute-howto-routing-classic.md) (Konfigurowanie routingu dla klasycznego modelu wdrażania) lub [Configure routing for the Resource Manager deployment model](expressroute-howto-routing-arm.md) (Konfigurowanie routingu dla modelu wdrażania usługi Resource Manager)
    - [Link a classic VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-classic.md) (Łączenie klasycznej sieci wirtualnej z obwodem usługi ExpressRoute) lub [Link a Resource Manager VNet to an ExpressRoute circuit](expressroute-howto-linkvnet-arm.md) (Łączenie sieci wirtualnej usługi Resource Manager z obwodem usługi ExpressRoute)





<!--HONumber=Sep16_HO3-->


