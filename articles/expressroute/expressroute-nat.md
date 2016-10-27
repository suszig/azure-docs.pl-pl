<properties
   pageTitle="Wymagania dotyczące translatora adresów sieciowych dla obwodów usługi ExpressRoute | Microsoft Azure"
   description="Ta strona zawiera szczegółowe wymagania dotyczące konfigurowania translatora adresów sieciowych oraz zarządzania nim na potrzeby obwodów usługi ExpressRoute."
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
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# Wymagania dotyczące translatora adresów sieciowych w usłudze ExpressRoute

Aby połączyć się z usługami w chmurze firmy Microsoft przy użyciu usługi ExpressRoute, należy skonfigurować translatory adresów sieciowych oraz zarządzać nimi. Niektórzy dostawcy połączenia oferują konfigurowanie translatora adresów sieciowych oraz zarządzanie nim jako usługę zarządzaną. Skontaktuj się z dostawcą połączenia, aby sprawdzić, czy taka usługa jest oferowana. Jeśli nie, musisz spełnić wymagania opisane poniżej. 

Przejrzyj stronę [ExpressRoute circuits and routing domains](expressroute-circuit-peerings.md) (Obwody i domeny routingu usługi ExpressRoute), która zawiera omówienie różnych domen routingu. Aby spełnić wymagania dotyczące publicznego adresu IP dla publicznej komunikacji równorzędnej Azure i komunikacji równorzędnej Microsoft, warto skonfigurować translator adresów sieciowych między siecią a firmą Microsoft. W tej sekcji przedstawiono szczegółowy opis infrastruktury translatora adresów sieciowych, którego należy skonfigurować.

## Wymagania dotyczące translatora adresów sieciowych dla publicznej komunikacji równorzędnej Azure

Ścieżka publicznej komunikacji równorzędnej Azure umożliwia łączenie ze wszystkimi usługami obsługiwanymi na platformie Azure za pośrednictwem ich publicznych adresów IP. Dotyczy to usług wymienionych w temacie [ExpessRoute FAQ](expressroute-faqs.md) (ExpessRoute — często zadawane pytania) i wszystkich usług obsługiwanych przez niezależnych dostawców oprogramowania na platformie Microsoft Azure. Połączenie z usługami Microsoft Azure w publicznej komunikacji równorzędnej jest zawsze inicjowane z sieci użytkownika do sieci Microsoft. Ruch skierowany do platformy Microsoft Azure w publicznej komunikacji równorzędnej musi zostać podłączony do funkcji SNAT i uzyskać prawidłowe publiczne adresy IPv4, zanim wejdzie do sieci firmy Microsoft. Poniższy rysunek przedstawia, w jaki sposób na wysokim poziomie można skonfigurować translatora adresów sieciowych i spełnić powyższe wymaganie.

![](./media/expressroute-nat/expressroute-nat-azure-public.png) 

### Anonse puli adresów IP translatora adresów sieciowych oraz tras

Należy zadbać o to, by ruch wchodził na ścieżkę publicznej komunikacji równorzędnej Azure z prawidłowym publicznym adresem IPv4. Firma Microsoft musi mieć możliwość weryfikacji własności puli adresów translatora adresów sieciowych IPv4 względem regionalnego rejestru internetowego routingu (RIR) lub internetowego rejestru routingu (IRR). Nastąpi sprawdzenie w oparciu o numer AS połączony za pomocą komunikacji równorzędnej oraz adresy IP używane do translatora adresów sieciowych. Informacje dotyczące rejestrów routingu znajdują się w temacie [ExpressRoute routing requirements](expressroute-routing.md) (Wymagania dotyczące routingu w usłudze ExpressRoute).
 
Nie ma żadnych ograniczeń w zakresie długości prefiksu IP translatora adresów sieciowych anonsowanego za pośrednictwem komunikacji równorzędnej. Należy monitorować pulę translatora adresów sieciowych i zadbać o to, by nie zabrakło jego sesji.

>[AZURE.IMPORTANT] Pula adresów IP translatora adresów sieciowych anonsowana do firmy Microsoft nie może być anonsowana do Internetu. Spowodowałoby to przerwanie łączności z innymi usługami firmy Microsoft.

## Wymagania dotyczące translatora adresów sieciowych dla komunikacji równorzędnej firmy Microsoft

Ścieżka komunikacji równorzędnej firmy Microsoft umożliwia nawiązanie połączenia z usługami w chmurze firmy Microsoft, które nie są obsługiwane przez ścieżkę publicznej komunikacji równorzędnej Azure. Lista usług obejmuje usługi Office 365, np. Exchange Online, SharePoint Online, Skype dla firm i CRM Online. Firma Microsoft planuje obsługę dwukierunkowej łączności w oparciu o komunikację równorzędną firmy Microsoft. Ruch skierowany usług w chmurze firmy Microsoft musi zostać podłączony do funkcji SNAT i uzyskać prawidłowe publiczne adresy IPv4, zanim wejdzie do sieci firmy Microsoft. Ruch skierowany do sieci użytkownika z usług w chmurze firmy Microsoft należy przetworzyć za pomocą translatora SNAT, zanim wejdzie do sieci użytkownika. Poniższy rysunek przedstawia w jaki sposób na wysokim poziomie powinien być skonfigurowany translator adresów sieciowych na potrzeby komunikacji równorzędnej firmy Microsoft.
 
![](./media/expressroute-nat/expressroute-nat-microsoft.png) 


#### Ruch pochodzący z sieci skierowany do firmy Microsoft

- Należy zadbać o to, by ruch wchodził na ścieżkę komunikacji równorzędnej Microsoft z prawidłowym publicznym adresem IPv4. Firma Microsoft musi mieć możliwość weryfikacji właściciela puli adresów translatora adresów sieciowych IPv4 względem regionalnego rejestru internetowego routingu (RIR) lub internetowego rejestru routingu (IRR). Nastąpi sprawdzenie w oparciu o numer AS połączony za pomocą komunikacji równorzędnej oraz adresy IP używane do translatora adresów sieciowych. Informacje dotyczące rejestrów routingu znajdują się w temacie [ExpressRoute routing requirements](expressroute-routing.md) (Wymagania dotyczące routingu w usłudze ExpressRoute).

- Adresów IP używanych do konfiguracji publicznej komunikacji równorzędnej Azure oraz innych obwodów usługi ExpressRoute nie można anonsować do firmy Microsoft za pośrednictwem sesji protokołu BGP. Nie ma żadnych ograniczeń w zakresie długości prefiksu IP translatora adresów sieciowych anonsowanego za pośrednictwem komunikacji równorzędnej.

    >[AZURE.IMPORTANT] Pula adresów IP translatora adresów sieciowych anonsowana do firmy Microsoft nie może być anonsowana do Internetu. Spowodowałoby to przerwanie łączności z innymi usługami firmy Microsoft.

#### Ruch pochodzący z firmy Microsoft skierowany do sieci

- Niektóre scenariusze wymagają od firmy Microsoft zainicjowania połączenia z punktami końcowymi usługi obsługiwanymi w ramach sieci użytkownika. Typowym przykładem takiego scenariusza jest łączność z serwerami usług AD FS obsługiwanymi w sieci użytkownika z poziomu usługi Office 365. W takich przypadkach należy przefiltrować odpowiednie prefiksy z sieci użytkownika do komunikacji równorzędnej firmy Microsoft. 

- Należy użyć funkcji SNAT względem ruchu z firmy Microsoft skierowanego do adresów IP w ramach sieci użytkownika. 

## Następne kroki

- Zapoznaj się z wymaganiami dotyczącymi [routingu](expressroute-routing.md) i technologii [QoS](expressroute-qos.md).
- Informacje o przepływach pracy można znaleźć w artykule [ExpressRoute circuit provisioning workflows and circuit states](expressroute-workflows.md) (Przepływy pracy inicjowania obsługi obwodu i stany obwodu usługi ExpressRoute).
- Skonfiguruj połączenie usługi ExpressRoute.

    - [Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)](expressroute-howto-circuit-classic.md)
    - [Configure routing (Konfigurowanie routingu)](expressroute-howto-routing-classic.md)
    - [Link a VNet to an ExpressRoute circuit (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute)](expressroute-howto-linkvnet-classic.md)




<!--HONumber=Oct16_HO3-->


