---
title: "Wymagania dotyczące technologii QoS dla usługi ExpressRoute | Microsoft Docs"
description: "Ta strona zawiera szczegółowe wymagania dotyczące konfigurowania technologii QoS oraz zarządzania nią na potrzeby obwodów usługi ExpressRoute."
documentationcenter: na
services: expressroute
author: cherylmc
manager: carmonm
editor: 
ms.assetid: db1c1447-0283-4a09-907b-ae481adc40c7
ms.service: expressroute
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 9e74127e3140ec9973753fb5f29151c406326c36


---
# <a name="expressroute-qos-requirements"></a>Wymagania dotyczące technologii QoS w usłudze ExpressRoute
Program Skype dla firm obejmuje różne obciążenia, które wymagają zróżnicowanej obsługi w technologii QoS. Jeśli planujesz korzystać z usług głosowych za pośrednictwem usługi ExpressRoute, musisz spełnić opisane poniżej wymagania.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> Wymagania dotyczące technologii QoS dotyczą tylko komunikacji równorzędnej firmy Microsoft. Wartości DSCP w ruchu sieciowym otrzymane w publicznej komunikacji równorzędnej Azure i prywatnej komunikacji równorzędnej Azure zostaną zresetowane do wartości 0. 
> 
> 

Poniższa tabela zawiera listę oznaczeń DSCP używanych przez program Skype dla firm. Więcej informacji znajduje się w artykule [Managing QoS for Skype for Business](https://technet.microsoft.com/library/gg405409.aspx) (Zarządzanie technologią QoS na potrzeby programu Skype dla firm).

| **Klasa ruchu** | **Obsługa (oznaczanie DSCP)** | **Obciążenia programu Skype dla firm** |
| --- | --- | --- |
| **Połączenia głosowe** |EF (46) |Połączenia głosowe Skype/Lync |
| **Interaktywne** |AF41 (34) |Połączenia wideo |
| AF21 (18) |Współdzielenie aplikacji | |
| **Domyślne** |AF11 (10) |Transfer plików |
| CS0 (0) |Inne | |

* Sklasyfikuj obciążenia i zaznacz odpowiednie wartości DSCP. Postępuj zgodnie ze wskazówkami podanymi [tutaj](https://technet.microsoft.com/library/gg405409.aspx) dotyczącymi ustawiania oznaczeń DSCP w sieci.
* Skonfiguruj i obsługuj wiele kolejek w technologii QoS w sieci. Połączenia głosowe muszą być klasą autonomiczną i być obsługiwane w sposób określony w RFC 3246. 
* Możesz wybrać mechanizm kolejkowania, zasady wykrywania przeciążenia oraz alokację przepustowości na klasę ruchu. Oznaczanie DSCP dla obciążeń programu Skype dla firm musi jednak zostać zachowane. Jeśli używasz oznaczenia DSCP niewymienionego powyżej, np. AF31 (26), musisz zmodyfikować tę wartość DSCP do 0 przed wysłaniem pakietu do firmy Microsoft. Firma Microsoft wysyła tylko pakiety oznaczone wartościami DSCP pokazanymi w powyższej tabeli. 

## <a name="next-steps"></a>Następne kroki
* Zapoznaj się z wymaganiami dotyczącymi [routingu](expressroute-routing.md) i [translatora adresów sieciowych](expressroute-nat.md).
* Użyj następujących linków przydatnych podczas konfigurowania połączenia za pomocą usługi ExpressRoute.
  
  * [Create an ExpressRoute circuit (Tworzenie obwodu usługi ExpressRoute)](expressroute-howto-circuit-classic.md)
  * [Configure routing (Konfigurowanie routingu)](expressroute-howto-routing-classic.md)
  * [Link a VNet to an ExpressRoute circuit (Łączenie sieci wirtualnej z obwodem usługi ExpressRoute)](expressroute-howto-linkvnet-classic.md)




<!--HONumber=Nov16_HO2-->


