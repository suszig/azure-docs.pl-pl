<properties
   pageTitle="Wymagania dotyczące technologii QoS dla usługi ExpressRoute | Microsoft Azure"
   description="Ta strona zawiera szczegółowe wymagania dotyczące konfigurowania technologii QoS oraz zarządzania nią na potrzeby obwodów usługi ExpressRoute."
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
   ms.date="04/18/2016"
   ms.author="cherylmc"/>

# Wymagania dotyczące technologii QoS w usłudze ExpressRoute

Program Skype dla firm obejmuje różne obciążenia, które wymagają zróżnicowanej obsługi w technologii QoS. Jeśli planujesz korzystać z usług głosowych za pośrednictwem usługi ExpressRoute, musisz spełnić opisane poniżej wymagania.

![](./media/expressroute-qos/expressroute-qos.png)

>[AZURE.NOTE] Wymagania dotyczące technologii QoS dotyczą tylko komunikacji równorzędnej firmy Microsoft.

Poniższa tabela zawiera listę oznaczeń DSCP używanych przez program Skype dla firm. Więcej informacji znajduje się w artykule [Managing QoS for Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) (Zarządzanie technologią QoS na potrzeby programu Skype dla firm).

| **Klasa ruchu** | **Obsługa (oznaczanie DSCP)** | **Obciążenia programu Skype dla firm** |
|---|---|---|
| **Połączenia głosowe** | EF (46) | Połączenia głosowe Skype/Lync |
| **Interaktywne** | AF41 (34) | Połączenia wideo |
|   | AF21 (18) | Współdzielenie aplikacji | 
|   | CS3 (24) | Sygnalizowanie SIP |
| **Domyślne** | AF11 (10) | Transfer plików|
|   | CS0 (0) | Inne| 


- Sklasyfikuj obciążenia i zaznacz odpowiednie wartości DSCP. Postępuj zgodnie ze wskazówkami podanymi [tutaj](https://technet.microsoft.com/library/gg405409.aspx) dotyczącymi ustawiania oznaczeń DSCP w sieci.

- Skonfiguruj i obsługuj wiele kolejek w technologii QoS w sieci. Połączenia głosowe muszą być klasą autonomiczną i być obsługiwane w sposób określony w RFC 3246. 

- Możesz wybrać mechanizm kolejkowania, zasady wykrywania przeciążenia oraz alokację przepustowości na klasę ruchu. Oznaczanie DSCP dla obciążeń programu Skype dla firm musi jednak zostać zachowane. Jeśli używasz oznaczenia DSCP niewymienionego powyżej, np. AF31 (26), musisz zmodyfikować tę wartość DSCP do 0 przed wysłaniem pakietu do firmy Microsoft. Firma Microsoft wysyła tylko pakiety oznaczone wartościami DSCP pokazanymi w powyższej tabeli. 

## Następne kroki

- Zapoznaj się z wymaganiami dotyczącymi [routingu](expressroute-routing.md) i [translatora adresów sieciowych](expressroute-nat.md).
- Użyj następujących linków przydatnych podczas konfigurowania połączenia za pomocą usługi ExpressRoute.

    - [Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)](expressroute-howto-circuit-classic.md)
    - [Configure routing (Konfigurowanie routingu)](expressroute-howto-routing-classic.md)
    - [Link a VNet to an ExpressRoute circuit (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute)](expressroute-howto-linkvnet-classic.md)



<!--HONumber=jun16_HO2-->


