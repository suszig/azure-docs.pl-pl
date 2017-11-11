---
title: "Konfigurowanie środowiska usługi aplikacji Azure jako życie tunelowane"
description: "Włącz Twojej ASE do pracy w przypadku ruchu wychodzącego życie tunelowane"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: ccompy
ms.openlocfilehash: cd89bd23074dec1de6fa0e8784d42a5c539938b1
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurowanie środowiska usługi aplikacji przy użyciu tunelowania wymuszonego

Środowisko usługi aplikacji (ASE) to wdrożenie usługi Azure App Service w sieci wirtualnej platformy Azure (VNet) klienta. Wielu klientów, skonfigurować ich sieci wirtualnych za rozszerzeń swoich sieciach lokalnych, sieci VPN lub ExpressRoute połączenia. Ze względu na zasady firmowe lub inne ograniczenia zabezpieczeń skonfigurowano trasy do wysłania wszystkich ruch wychodzący lokalnej przed korzystaniem z niego z Internetem. Zmiana routingu sieci wirtualnej, aby ruch wychodzący z sieci wirtualnej przechodzi przez połączenie sieci VPN i ExpressRoute lokalnymi nazywa się wymuszanie tunelowania.  

Wymuszanie tunelowania może powodować problemy dla ASE. ASE ma liczbę zależności zewnętrznych, które są wymienione w tym [architektury sieci ASE] [ network] dokumentu. ASE, domyślnie wymaga, aby cała komunikacja wychodząca przechodzi przez VIP jest udostępniane z ASE.

Trasy są krytyczne aspekt, co wymuszonego tunelowania i sposób postępowania z nim. W sieci wirtualnej platformy Azure routing odbywa się na Najdłuższy prefiks dopasowania (LPM) Wybierane na podstawie.  Jeśli istnieje więcej niż jedna trasa z tym samym dopasowaniem LPM, następnie wybór trasy odbywa się w następującej kolejności:

1. Trasa zdefiniowana przez użytkownika
1. Trasa protokołu BGP (jeśli używane są połączenia ExpressRoute)
1. Trasa systemowa

Aby dowiedzieć się więcej na temat routingu w sieci wirtualnej, przeczytaj [zdefiniowane przez użytkownika tras i przesyłania dalej IP][routes]. 

Jeśli chcesz, aby Twoje ASE na działanie w ramach tunelowania wymuszonego sieci wirtualnej, dostępne są dwie opcje:

1. Włącz Twojej ASE na bezpośredni dostęp do Internetu
1. Zmiana punktu końcowego wyjście z ASE

## <a name="enable-your-ase-to-have-direct-internet-access"></a>Włącz Twojej ASE na bezpośredni dostęp do Internetu

Dla Twojego ASE pracę podczas sieci wirtualnej jest skonfigurowana z usługi ExpressRoute można:

* Konfigurowanie usługi ExpressRoute anonsowanie 0.0.0.0/0. Domyślnie go wymusić tuneli wszystkie wychodzący ruch lokalnymi.
* Utwórz przez. Zastosować go do podsieci, która zawiera ASE z prefiksem adresu 0.0.0.0/0 i typ następnego przeskoku w Internecie.

Jeśli te dwie zmiany, ruch kierowany przez internet pochodzący z podsieci ASE nie jest wymuszone usługi ExpressRoute i ASE działa.

> [!IMPORTANT]
> Trasy zdefiniowane w przez musi być wystarczająco konkretny, aby mają pierwszeństwo względem dowolnego trasy anonsowane przez konfiguracji usługi ExpressRoute. W poprzednim przykładzie użyto 0.0.0.0/0 szerokiego zakresu adresów. Go może potencjalnie być przypadkowo zastąpione anonsów tras, korzystających z bardziej szczegółowych zakresów adresów.
>
> ASEs nie są obsługiwane w przypadku konfiguracji usługi ExpressRoute, które między anonsować tras z publicznej komunikacji równorzędnej ścieżki do ścieżki prywatnej komunikacji równorzędnej. Konfiguracji usługi ExpressRoute z publicznej komunikacji równorzędnej skonfigurowane otrzymywać anonsów tras od firmy Microsoft. Anonse zawiera zbiór duże zakresy adresów IP firmy Microsoft Azure. W przypadku zakresów adresów między anonsowany w ścieżce prywatnej komunikacji równorzędnej, wszystkie pakiety wychodzącego z podsieci ASE są życie tunel do infrastruktury sieci lokalnej klienta. Ten przepływ sieci nie jest obecnie obsługiwane przez ASEs wartość domyślną. Jedno rozwiązanie tego problemu jest zatrzymanie tras między reklamy ze ścieżki publicznej komunikacji równorzędnej do ścieżki prywatnej komunikacji równorzędnej.  Inne rozwiązanie jest umożliwienie Twojej ASE do pracy w konfiguracji tunelowania wymuszonego.

## <a name="change-the-egress-endpoint-for-your-ase"></a>Zmiana punktu końcowego wyjście z ASE ##

Ta sekcja opisuje sposób włączania ASE na działanie w ramach konfiguracji tunelowania wymuszonego, zmieniając wyjście punktu końcowego używane przez ASE. Jeśli ruch wychodzący z ASE jest wymuszenie tunneled do sieci lokalnej, należy zezwolić na ruch z źródła z adresów IP innego niż adres ASE VIP.

ASE nie tylko ma zależności zewnętrznych, ale go również musi nasłuchiwać na ruch przychodzący do zarządzania ASE. ASE musi być w stanie odpowiedzieć na taki ruch i nie można wysłać odpowiedzi z innego adresu, jako który dzieli TCP.  W związku z tym są trzy kroki wymagane do zmiany wyjście punktu końcowego dla ASE.

1. Ustaw tabelę tras, aby upewnić się, że ruch związany z zarządzaniem przychodzącej można powrocie z tego samego adresu IP
1. Dodawanie adresów IP, która posłuży za wyjście do zapory ASE
1. Ustaw tras na ruch wychodzący z ASE, aby być tunelowane

![Wymuszone tunelu sieci przepływu][1]

ASE można skonfigurować przy użyciu różnych wyjście adresów po ASE jest już w górę i operacyjne lub mogą być ustawione podczas wdrażania ASE.  

### <a name="changing-the-egress-address-after-the-ase-is-operational"></a>Zmiana adresu Wyjście po działa ASE ###
1. Uzyskaj adresy IP, który ma być używany jako wyjście adresów IP dla sieci ASE. Jeśli przeprowadzasz wymuszone tunelowanie, będzie to Twoja NAT lub bramy adresów IP, a następnie.  Jeśli do kierowania ruchu wychodzącego ASE za pośrednictwem NVA należy adres wyjście będzie publicznego adresu IP z analizę NVA.
2. Ustaw adres wyjście w ASE informacje o konfiguracji. Przejdź do resource.azure.com i przejdź do: subskrypcji /<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name> , a następnie widać json, opisujący ASE użytkownika.  Upewnij się, że wyświetlany jest tekst odczytu/zapisu u góry.  Kliknij przycisk Edytuj przewijania w dół i zmień userWhitelistedIpRanges z  

       "userWhitelistedIpRanges": null 
      
  aby przypominać następujące czynności. Użyj adresów, które chcesz ustawić jako zakres adresów wyjście. 

      "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

  Kliknij przycisk PUT u góry. To wyzwala na Twoje ASE operację skalowania i Dostosuj zapory.
   
3. Utwórz lub Edytuj tabelę tras i wypełnić reguły, aby zezwolić na dostęp z adresów zarządzania, które mapują do lokalizacji ASE.  W tym miejscu są adresy zarządzania [adresy zarządzania środowiska usługi aplikacji][management] 

4. Dostosuj trasy stosowana do podsieci ASE z tabelą tras lub trasy protokołu BGP.  

Jeśli ASE przestanie odpowiadać z portalu, oznacza to, że problem ze zmianami.  Można go listy adresów wyjście nie zostało ukończone, ruch zostało utracone lub ruch został zablokowany.  

### <a name="create-a-new-ase-with-a-different-egress-address"></a>Utwórz nowy ASE z adresem różnych wyjście  ###

W przypadku, gdy sieci wirtualnej jest już skonfigurowana tak, aby wymusić tunelu cały ruch, należy wykonać kilka dodatkowych czynności, można utworzyć użytkownika ASE taki sposób, że może ona pomyślnie. Oznacza to, że należy włączyć korzystanie z innym punktem końcowym wyjście podczas tworzenia ASE.  Aby to zrobić, należy utworzyć ASE z szablonu, który określa adresy dozwolonych wyjście.

1. Uzyskaj adresy IP w celu można użyć jako adresy wyjście z ASE.
1. Utwórz wstępnie podsieci, które mają być używane przez ASE. Jest to niezbędne do pozwalają na ustawienie tras, a także ponieważ wymaga szablonu.  
1. Utwórz tabelę tras z zarządzania adresów IP, mapy do lokalizacji ASE i przypisz go do Twojego ASE
1. Postępuj zgodnie z instrukcjami w tym miejscu [tworzenie ASE z szablonem][template]i rozwiń odpowiedni szablon
1. Przeprowadź edycję sekcji "zasoby" plik azuredeploy.json. Uwzględnij wiersz dla **userWhitelistedIpRanges** własnymi wartościami, takich jak:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Jeśli to jest prawidłowo skonfigurowany, ASE należy uruchomić bez żadnych problemów.  


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
