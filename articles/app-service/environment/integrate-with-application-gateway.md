---
title: Integrowanie z ASE ILB z bramy aplikacji Azure
description: "Przewodnik dotyczący sposobu integracji aplikacji w Twojej ASE ILB z bramy aplikacji Azure"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a6a74f17-bb57-40dd-8113-a20b50ba3050
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: ccompy
ms.openlocfilehash: eedad8824add7fe425d34975dab640fbee82c2bc
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/19/2017
---
# <a name="integrating-your-ilb-ase-with-an-application-gateway"></a>Integrowanie z ASE ILB z bramy aplikacji #

[Środowiska usługi aplikacji Azure (ASE)](./intro.md) wdrożenie usługi Azure App Service w podsieci sieci wirtualnej Azure klienta. Może on zostać wdrożony z punktu końcowego publicznej lub prywatnej w celu uzyskania dostępu do aplikacji. Wdrożenie ASE z prywatnej punktu końcowego jest nazywany ASE ILB.  
Brama aplikacji w usłudze Azure to urządzenie wirtualne, które oferuje usługa równoważenia obciążenia warstwy 7, odciążanie protokołu SSL i ochrony zapory aplikacji sieci Web. Można go nasłuchiwać na publiczny adres i trasy ruch pakietów IP punktu końcowego aplikacji. Poniższe informacje zawierają opis sposobu integracji zapory aplikacji sieci Web skonfigurowano aplikację na elemencie ASE ILB Application Gateway.  

Integracja bramy aplikacji z ILB ASE znajduje się na poziomie aplikacji.  Po skonfigurowaniu bramy aplikacji z Twojej ASE ILB są to zrobić dla określonych aplikacji w Twojej ASE ILB. Dzięki temu hosting bezpieczne aplikacje wielodostępne w jednym ASE ILB.  

![Wskazuje aplikacji na elemencie ASE ILB bramy aplikacji][1]

Korzystając z tego przewodnika, wykonasz następujące czynności:

* Tworzenie bramy aplikacji
* Brama aplikacji, aby wskazywała aplikację na Twojej ILB ASE
* Skonfiguruj aplikację, aby uwzględnić nazwy domeny niestandardowej
* Edytuj publicznej nazwy hosta DNS, który wskazuje bramy aplikacji

Aby zintegrować bramy aplikacji z Twojej ASE ILB, potrzebne są:

* ILB ASE
* Aplikacja uruchomiona w ILB ASE
* Nazwa domeny routingu internetowego ma być używany z aplikacji w ILB ASE
* adres ILB, używany przez Twoje ASE ILB (jest to w portalu ASE w obszarze **Ustawienia -> adresy IP**)

    ![Adresy IP używane przez użytkownika ILB ASE][9]
    
* publicznej nazwy DNS, która jest później używany wskaż bramy aplikacji 

Dla szczegółowe informacje na temat tworzenia ASE ILB odczytu dokumentu [tworzenie i używanie ILB ASE][ilbase]

W tym przewodniku przyjęto założenie, że ma bramę aplikacji w tej samej sieci wirtualnej Azure, wdrożonej ASE na. Przed rozpoczęciem tworzenia bramy aplikacji, wybierz lub Utwórz podsieć, która będzie używany do hostowania bramy aplikacji. Należy użyć podsieci, która jest nie jedną o nazwie GatewaySubnet lub w podsieci używanej przez ILB ASE.
Jeśli brama aplikacji w GatewaySubnet będzie on mógł późniejszego utworzenia bramy sieci wirtualnej. Możesz również nie można wstawić go do podsieci, używany przez Twoje ASE ILB jako ASE jest jedynym elementem, który może znajdować się w jego podsieci.

## <a name="steps-to-configure"></a>Kroki, aby skonfigurować ##

1. W portalu Azure, przejdź do **nowy > Sieć > bramy aplikacji** 
    1. Podaj:
        1. Nazwa bramy aplikacji
        1. Wybierz zapory aplikacji sieci Web
        1. Wybierz tej samej subskrypcji, które są używane dla sieci wirtualnej ASE
        1. Utwórz lub wybierz grupę zasobów
        1. Wybierz lokalizację, w której znajduje się w sieci wirtualnej ASE

    ![Nowej podstawy tworzenia bramy aplikacji][2]   
    1. W zestawie obszaru ustawienia:
        1. Sieć wirtualna ASE
        1. Podsieć bramy aplikacji musi być wdrożony do. Czy używana wartość GatewaySubnet uniemożliwi tworzenie bramy sieci VPN
        1. Wybierz publiczny
        1. Wybierz publiczny adres IP. Jeśli nie masz jednej następnie utworzyć w tej chwili
        1. Skonfiguruj HTTP lub HTTPS. Jeśli konfigurowany dla protokołu HTTPS, należy podać certyfikat PFX
        1. Wybierz ustawienia fireway aplikacji sieci Web. W tym miejscu można włączyć zapory, a także ustawić albo wykrywania lub zapobiegania jak widać dopasowanie.

    ![Nowe ustawienia tworzenia bramy aplikacji][3]
    
    1. Przejrzyj podsumowanie sekcji wybierz **Ok**. Może upłynąć trochę ponad 30 minut dla bramy sieci aplikacji w taki sposób, aby zakończyć instalację.  

2. Po zakończeniu instalacji bramy aplikacji przejdź do portalu usługi Application Gateway. Wybierz **puli zaplecza**.  Dodaj adres ILB dla Twojego ASE ILB.

    ![Konfigurowanie puli wewnętrznej bazy danych][4]

3. Po zakończeniu przetwarzania konfigurowania puli wewnętrznej bazy danych użytkownika, wybierz **sondy kondycji**. Utwórz badanie kondycji dla nazwy domeny, który ma być używany dla aplikacji. 

    ![Konfigurowanie sond kondycji][5]
    
4. Po zakończeniu przetwarzania konfigurowania sieci sondy kondycji wybierz **ustawienia HTTP**.  Edytuj istniejące ustawienie istnieje, wybierz **sondowania niestandardowych użyj**i wybierz sondowania został skonfigurowany.

    ![Skonfiguruj ustawienia protokołu HTTP][6]
    
5. Przejdź na bramie aplikacji **omówienie**i skopiuj publiczny adres IP używany dla bramy aplikacji.  Ustaw ten adres IP jako rekord A dla nazwy domeny aplikacji lub użyj nazwy DNS dla tego adresu w rekord CNAME.  Możliwe jest łatwiejsze wybierz publiczny adres IP i skopiuj go z adresu publicznego adresu IP interfejsu użytkownika, a nie skopiuj go z łącza w sekcji Przegląd bramy aplikacji. 

    ![Portal bramy aplikacji][7]

6. Ustaw nazwę domeny niestandardowej dla aplikacji w Twojej ASE ILB.  Przejdź do aplikacji w portalu, a w obszarze Ustawienia zaznacz **domen niestandardowych**

![Ustaw niestandardową nazwę domeny w aplikacji][8]

Brak informacji na temat ustawiania niestandardowych nazw domen dla aplikacji sieci web w tym miejscu [ustawienie niestandardowych nazw domen dla aplikacji sieci web][custom-domain]. Różnica w stosunku do aplikacji, w tym dokumencie i ASE ILB jest, że nie ma żadnych Weryfikacja nazwy domeny.  Ponieważ jesteś właścicielem DNS, który zarządza punkty końcowe aplikacji, możesz umieścić dowolne w nim. Niestandardowa nazwa domeny dodane w tym przypadku musi być w systemie DNS, ale można skonfigurować za pomocą aplikacji nadal jest konieczne. 

Po zakończeniu instalacji i może krótkim czasie DNS zmiany są propagowane, a następnie uzyskać dostęp do aplikacji, nazwy domeny niestandardowej, który został utworzony. 


<!--IMAGES-->
[1]: ./media/integrate-with-application-gateway/appgw-highlevel.png
[2]: ./media/integrate-with-application-gateway/appgw-createbasics.png
[3]: ./media/integrate-with-application-gateway/appgw-createsettings.png
[4]: ./media/integrate-with-application-gateway/appgw-backendpool.png
[5]: ./media/integrate-with-application-gateway/appgw-healthprobe.png
[6]: ./media/integrate-with-application-gateway/appgw-httpsettings.png
[7]: ./media/integrate-with-application-gateway/appgw-publicip.png
[8]: ./media/integrate-with-application-gateway/appgw-customdomainname.png
[9]: ./media/integrate-with-application-gateway/appgw-iplist.png

<!--LINKS-->
[appgw]: http://docs.microsoft.com/azure/application-gateway/application-gateway-introduction
[custom-domain]: ../app-service-web-tutorial-custom-domain.md
[ilbase]: ./create-ilb-ase.md
