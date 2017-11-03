---
title: "Tworzenie i używanie wewnętrzny moduł równoważenia obciążenia z środowiska usługi aplikacji | Dokumentacja firmy Microsoft"
description: Tworzenie i korzystanie z ILB ASE
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: ad9a1e00-d5e5-413e-be47-e21e5b285dbf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: ccompy
ms.openlocfilehash: c0f942cb11edc6d6212914b3134e25c3fbc3d3ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>Przy użyciu wewnętrznego modułu równoważenia obciążenia z środowiska usługi aplikacji

> [!NOTE] 
> Ten artykuł dotyczy v1 środowiska usługi aplikacji. Istnieje nowsza wersja środowiska usługi aplikacji jest łatwiejsza w użyciu, który jest uruchamiany na bardziej zaawansowanych infrastruktury. Aby dowiedzieć się więcej o nowy początek wersji z [wprowadzenie do środowiska usługi aplikacji](intro.md).
>

Funkcji środowiska usługi aplikacji (ASE) to opcja Premium usługi Azure App Service, która zapewnia możliwość rozszerzonej konfiguracji, która nie jest dostępna w sygnatury wielodostępnej. Funkcja ASE wdraża zasadniczo usłudze Azure App Service w Twojej Network(VNet) wirtualnych Azure. Aby uzyskać lepsze zrozumienie możliwości oferowane przez odczytać środowiska usługi App Service [co to jest środowisko usługi aplikacji] [ WhatisASE] dokumentacji. Jeśli nie znasz zalet działających w sieci wirtualnej odczytu [często zadawane pytania sieci wirtualnych Azure][virtualnetwork]. 

## <a name="overview"></a>Omówienie
ASE można wdrożyć z dostępnym punkcie końcowym internet lub z adresu IP w sieci wirtualnej. Aby ustawić adres IP na adres sieci wirtualnej, należy wdrożyć Twojej ASE z wewnętrznego Balancer(ILB) obciążenia. Po skonfigurowaniu sieci ASE ILB zawierają:

* własnych domeny lub poddomeny. Aby ułatwić, tym dokumencie przyjęto założenie domeny podrzędnej, ale można skonfigurować w obu przypadkach. 
* certyfikat używany do obsługi protokołu HTTPS
* Zarządzanie systemem DNS z domeny podrzędnej. 

W zamian można wykonać czynności takich jak:

* Host intranet aplikacji, takich jak aplikacje biznesowe, bezpiecznie w chmurze, dostępny za pośrednictwem witryny do witryny lub sieci VPN usługi ExpressRoute
* aplikacje hosta w chmurze, które nie są wymienione w publicznych serwerów DNS
* Tworzenie aplikacji zaplecza internet samodzielnie, których fronton aplikacji bezpiecznego można zintegrować z

#### <a name="disabled-functionality"></a>Funkcje wyłączone
Istnieje kilka kwestii, które nie można wykonać przy użyciu ASE ILB. Czynności te obejmują:

* przy użyciu IPSSL
* Przypisywanie adresów IP do określonych aplikacji
* kupowanie i używa certyfikatu z aplikacją za pośrednictwem portalu. Oczywiście nadal można uzyskać certyfikatów bezpośrednio z urzędu certyfikacji i używać go z aplikacji, ale nie za pośrednictwem portalu Azure.

## <a name="creating-an-ilb-ase"></a>Tworzenie ILB ASE
Tworzenie ASE ILB nie jest znacznie różni się od tworzenia ASE normalnie. Omówienie więcej danych na temat tworzenia ASE odczytu [tworzenie środowiska usługi aplikacji][HowtoCreateASE]. Proces tworzenia ASE ILB jest taka sama, między tworzenia sieci wirtualnej podczas tworzenia ASE lub wybierz istniejące sieci wirtualnej. Aby utworzyć ASE ILB: 

1. W portalu Azure wybierz **nowy -> sieci Web i mobilność -> środowisko usługi aplikacji**
2. Wybierz subskrypcję
3. Wybierz lub Utwórz grupę zasobów
4. Wybierz lub Utwórz sieć wirtualną
5. Utwórz podsieć, w przypadku wybrania sieci wirtualnej
6. Wybierz **wirtualnego lokalizacji-> Konfiguracja sieci wirtualnej** i Ustaw typ adresu VIP do wewnętrznego
7. Podaj nazwę domeny podrzędnej (będzie poddomeny używane w przypadku aplikacji utworzone w tym ASE)
8. Wybierz przycisk Ok, a następnie utwórz

![][1]

W bloku sieci wirtualnej jest opcja Konfiguracja sieci wirtualnej. Pozwala to wybrać między zewnętrznego adresu VIP lub wewnętrznego adresu VIP. Wartość domyślna to zewnętrznego. Jeśli masz ustawioną zewnętrzne sieci ASE użyje internet dostępne wirtualne adresy IP. W przypadku wybrania wewnętrzne Twojej ASE zostaną skonfigurowane ILB na adres IP w sieci wirtualnej. 

Po wybraniu wewnętrzne, możliwość dodawania więcej adresów IP do sieci ASE zostaną usunięte i zamiast tego należy podać poddomeny ASE. W elemencie ASE z zewnętrznego adresu VIP nazwę ASE służy w poddomenie aplikacje utworzone w tym ASE. Jeśli została wywołana z ASE ***contosotest*** i aplikacji w tym ASE została wywołana ***test*** poddomeny może być w formacie ***contosotest.p.azurewebsites.net*** i adres URL dla tej aplikacji będą ***mytest.contosotest.p.azurewebsites.net***. Jeśli ustawisz typ adresu VIP do wewnętrznego nazwę ASE nie jest używany podczas poddomeny dla ASE. Należy jawnie określić poddomeny. Jeśli Twoje domeny podrzędnej ***contoso.corp.net*** i wprowadzone aplikacji, w tym o nazwie ASE ***timereporting*** adres URL dla tej aplikacji może być ***timereporting.contoso.corp.net***.

## <a name="apps-in-an-ilb-ase"></a>Aplikacje w elemencie ASE ILB
Tworzenie aplikacji w elemencie ASE ILB jest taka sama, jak podczas tworzenia aplikacji w elemencie ASE normalnie. 

1. W portalu Azure wybierz **nowy -> sieci Web i mobilność -> sieci Web** lub **Mobile** lub **aplikacji interfejsu API**
2. Wprowadź nazwę aplikacji
3. Wybieranie subskrypcji
4. Wybierz lub Utwórz grupę zasobów
5. Wybierz lub Utwórz Plan(ASP) usługi aplikacji. Jeśli tworzenie nowych stron ASP wybierz Twoje ASE jako lokalizację i wybierz puli procesów roboczych ma strona ASP ma być utworzony w. Po utworzeniu ASP należy wybrać Twojej ASE jako lokalizację i puli procesów roboczych. Po określeniu nazwy aplikacji przekonasz się, że poddomeny pod nazwą aplikacji zostało zastąpione przez poddomeny dla Twojego ASE. 
6. Wybierz opcję Utwórz. Należy wybrać **Przypnij do pulpitu nawigacyjnego** pole wyboru, jeśli chcesz, aby aplikacja wyświetlani na pulpicie nawigacyjnym. 

![][2]

W obszarze nazwy aplikacji Nazwa poddomeny pobiera zaktualizowana w celu odzwierciedlenia poddomeny ASE użytkownika. 

## <a name="post-ilb-ase-creation-validation"></a>Sprawdzanie poprawności tworzenia ILB ASE POST
ASE ILB są nieco inne niż z systemem innym niż - ILB ASE. Jak już zauważyć trzeba zarządzać własną DNS należy również podać swój własny certyfikat dla połączeń HTTPS. 

Po utworzeniu sieci ASE zauważysz poddomeny pokazuje poddomeny, zostanie określony, a nowy element **ustawienie** menu o nazwie **certyfikatu ILB**. ASE jest tworzony z certyfikatu z podpisem własnym, co pozwala na łatwiejsze testowanie HTTPS. Portalu będzie informujące, że musisz dostarczyć własny certyfikat dla protokołu HTTPS, ale to dysk, trzeba mieć certyfikat, który łączy się z Twojej domeny podrzędnej. 

![][3]

Jeśli są po prostu testuje rzeczy, a nie wiadomo, jak utworzyć certyfikat, można użyć aplikacji konsoli MMC programu IIS do utworzenia certyfikatu z podpisem własnym. Po jego utworzeniu można wyeksportować go jako plik PFX i przekaż go w Interfejsie użytkownika ILB certyfikatu. Po uzyskaniu dostępu do witryny zabezpieczonej z certyfikatu z podpisem własnym, przeglądarka zapewni ostrzeżenie, że uzyskują dostęp do witryny nie jest zabezpieczone z powodu braku możliwości zweryfikowania poprawności certyfikatu. Aby uniknąć tego ostrzeżenia, należy poprawnie podpisane certyfikat, który jest zgodny z poddomeny i ma łańcuch zaufania, który jest rozpoznawany przez przeglądarkę.

![][6]

Jeśli chcesz spróbować przepływ z własnych certyfikatów i testowanie protokołów HTTP i HTTPS dostęp do Twojego ASE:

1. Przejdź do ASE interfejsu użytkownika, po utworzeniu ASE **ASE -> Ustawienia -> ILB certyfikatów**
2. Określ certyfikat ILB, wybierając plik pfx certyfikatu i podaj hasło. Ten krok zajmuje chwilę do przetwarzania i wyświetlany jest komunikat, który operacja skalowania jest w toku.
3. Pobierz adres ILB dla Twojego ASE (**ASE -> Właściwości -> wirtualnego adresu IP**)
4. Tworzenie aplikacji sieci web w ASE po utworzeniu 
5. Tworzenie maszyny Wirtualnej, jeśli nie masz w tej sieci VNET (nie w tej samej podsieci co podziału ASE lub elementy)
6. Ustaw DNS z domeny podrzędnej. Można użyć symbolu wieloznacznego z Twojej domeny podrzędnej w systemie DNS lub jeśli chcesz to proste testy, należy edytować plik hosts na maszynie Wirtualnej można ustawić nazwy aplikacji sieci web do adresu VIP. Jeśli Twoje ASE ma nazwę poddomeny. ilbase.com i zostanie wykonane mytestapp aplikacji sieci web tak, aby będzie na mytestapp.ilbase.com następnie ustawić go w pliku hostów. (W systemie Windows w pliku hosts jest w C:\Windows\System32\drivers\etc\)
7. Korzystanie z przeglądarki na tej maszynie Wirtualnej, przejdź do http://mytestapp.ilbase.com (lub niezależnie od nazwę aplikacji sieci web jest z Twojej domeny podrzędnej)
8. Korzystanie z przeglądarki na tej maszynie Wirtualnej, przejdź do https://mytestapp.ilbase.com konieczne będzie akceptować braku zabezpieczeń, jeśli przy użyciu certyfikatu z podpisem własnym. 

Adres IP dla sieci ILB wymienionym we właściwościach wirtualnego adresu IP

![][4]

## <a name="using-an-ilb-ase"></a>Przy użyciu ILB ASE
#### <a name="network-security-groups"></a>Grupy zabezpieczeń sieci
ASE ILB umożliwia izolacja sieci dla aplikacji, ponieważ aplikacje nie są dostępne lub nawet znane przez internet. Jest to doskonały dla hostingu witryn intranetowych, takich jak aplikacje biznesowe. Gdy musisz ograniczyć dostęp nawet dalsze nadal służy Groups(NSGs) zabezpieczeń sieci do kontroli dostępu na poziomie sieci. 

Jeśli chcesz bardziej ograniczyć dostęp za pomocą grup NSG musisz upewnij się, że nie przerwanie komunikacji ASE wymaga, aby działać. Mimo że dostępu HTTP/HTTPS jest tylko za pośrednictwem ILB używane przez ASE ASE nadal zależy od zasobu poza siecią wirtualną. Aby zobaczyć, jakie dostępu do sieci jest nadal wymagana poszukać informacji w dokumencie dotyczącym [kontrolowanie ruch przychodzący do środowiska usługi aplikacji] [ ControlInbound] i zarządzania dokumentami na [szczegółów konfiguracji sieci dla środowiska usługi App Service z ExpressRoute][ExpressRoute]. 

Konfigurowanie grup NSG należy znać adres IP, który jest używany przez usługę Azure do zarządzania z ASE. Ten adres IP jest również wychodzący adres IP z sieci ASE, jeśli żądania internetowe. Wychodzący adres IP dla sieci ASE pozostanie statycznych przez cały okres istnienia sieci ASE. Jeśli Usuń i Utwórz ponownie Twoje ASE, otrzymasz nowy adres IP. Aby znaleźć ten adres IP, przejdź do **Ustawienia -> właściwości** i Znajdź **wychodzący adres IP**. 

![][5]

#### <a name="general-ilb-ase-management"></a>Zarządzanie ogólne ILB ASE
Zarządzanie ASE ILB przede wszystkim jest taka sama jak zarządzanie ASE normalnie. Musisz skalowanie w górę Twojej pule procesów roboczych, aby obsługiwać więcej wystąpień ASP i skalowanie w górę serwerów frontonu do obsługi zwiększonej ilości ruchu HTTP/HTTPS. Dla ogólnych informacji o zarządzaniu konfiguracji ASE odczytać dokument na [Konfigurowanie środowiska usługi aplikacji][ASEConfig]. 

Elementy na dodatkowe zarządzanie to zarządzanie certyfikatami i zarządzania DNS. Należy uzyskać i przekazać certyfikat używany do obsługi protokołu HTTPS, po utworzeniu ILB ASE i zastąp go przed jego wygaśnięciem. Ponieważ Azure jest właścicielem domeny podstawowej firma Microsoft może zapewnić certyfikaty dla ASEs z zewnętrznego adresu VIP. Ponieważ może to być dowolne poddomeny używane przez ASE ILB, należy dostarczyć własny certyfikat dla protokołu HTTPS. 

#### <a name="dns-configuration"></a>Konfiguracja DNS
Kiedy za pomocą zewnętrznego wirtualnego adresu IP serwera DNS jest zarządzane przez usługę Azure. Wszystkich aplikacji utworzonych w Twojej ASE jest automatycznie dodawany do usługi Azure DNS, który jest publicznym systemie DNS. W elemencie ASE ILB trzeba zarządzać własną DNS. Dla danej domeny podrzędnej, takich jak contoso.corp.net należy utworzyć rekordy, które wskazują na Twój adres ILB A systemu DNS:

    * 
    publikowanie *.SCM ftp 


## <a name="getting-started"></a>Wprowadzenie
Wprowadzenie do środowiska usługi App Service, zobacz [wprowadzenie do środowiska usługi App Service][WhatisASE]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: app-service-app-service-environment-intro.md
[HowtoCreateASE]: app-service-web-how-to-create-an-app-service-environment.md
[ControlInbound]: app-service-app-service-environment-control-inbound-traffic.md
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[ASEAutoscale]: app-service-environment-auto-scale.md
[ExpressRoute]: app-service-app-service-environment-network-configuration-expressroute.md
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: app-service-web-configure-an-app-service-environment.md
