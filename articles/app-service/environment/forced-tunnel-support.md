---
title: "Konfigurowanie środowiska usługi aplikacji Azure jako życie tunelowane"
description: "Włącz środowiska usługi aplikacji do pracy w przypadku ruchu wychodzącego życie tunelowane"
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
ms.openlocfilehash: f5f099042cefe666e22a9d561afeb4584db3d92c
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurowanie środowiska usługi aplikacji przy użyciu tunelowania wymuszonego

Środowiska usługi aplikacji to wdrożenie usługi Azure App Service w wystąpieniu klienta sieci wirtualnej platformy Azure. Wielu klientów, skonfigurować ich sieci wirtualnych za rozszerzeń swoich sieciach lokalnych, z sieci VPN lub rozwiązania Azure ExpressRoute połączenia. Ze względu na zasady firmowe lub inne ograniczenia zabezpieczeń skonfigurowano trasy do wysłania wszystkich ruch wychodzący lokalnej przed korzystaniem z niego z Internetem. Zmiana routingu sieci wirtualnej, aby ruch wychodzący z siecią wirtualną przechodzi przez połączenie sieci VPN i ExpressRoute lokalnymi nazywa się wymuszanie tunelowania. 

Wymuszanie tunelowania może powodować problemy dla środowiska usługi aplikacji. Środowiska usługi aplikacji zawiera szereg zależności zewnętrznych, które są wymienione w [architektury sieci środowiska usługi aplikacji] [ network] dokumentu. Środowiska usługi aplikacji, domyślnie wymaga, aby cała komunikacja wychodząca przechodzi przez VIP jest udostępniane z środowiska usługi aplikacji.

Trasy są krytyczne aspekt jakie wymuszanie tunelowania jest oraz sposób postępowania z nim. W sieci wirtualnej platformy Azure routing odbywa się oparciu o najdłuższe dopasowanie prefiksu (LPM). Jeśli istnieje więcej niż jedna trasa z tym samym dopasowaniem LPM, wybór trasy odbywa się w następującej kolejności:

* Trasy zdefiniowane przez użytkownika (przez)
* Trasa protokołu BGP (jeśli używane są połączenia ExpressRoute)
* Trasa systemowa

Aby dowiedzieć się więcej na temat routingu w sieci wirtualnej, przeczytaj [trasy zdefiniowane przez użytkownika i przesyłanie dalej IP][routes]. 

Jeśli chcesz środowiska usługi aplikacji do działania w sieci wirtualnej tunelowania wymuszonego, dostępne są dwie opcje:

* Włącz środowiska usługi aplikacji na bezpośredni dostęp do Internetu.
* Zmień punkt końcowy wyjście dla środowiska usługi aplikacji.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Włącz środowiska usługi aplikacji na bezpośredni dostęp do Internetu

Dla środowiska usługi aplikacji pracę podczas sieci wirtualnej jest skonfigurowana z połączenia ExpressRoute można:

* Konfigurowanie usługi ExpressRoute anonsowanie 0.0.0.0/0. Domyślnie go wymusić tuneli wszystkie wychodzący ruch lokalnymi.
* Utwórz przez. Zastosować go do podsieci, która zawiera środowiska usługi aplikacji z prefiksem adresu 0.0.0.0/0 i typ następnego przeskoku w Internecie.

Jeśli te dwie zmiany, ruch kierowany przez internet pochodzący z podsieci środowiska usługi aplikacji nie jest wymuszone połączenie ExpressRoute, i działa środowiska usługi aplikacji.

> [!IMPORTANT]
> Trasy zdefiniowane w przez musi być wystarczająco konkretny, aby mają pierwszeństwo względem dowolnego trasy anonsowane przez konfiguracji usługi ExpressRoute. W poprzednim przykładzie użyto 0.0.0.0/0 szerokiego zakresu adresów. Go może potencjalnie być przypadkowo zastąpione anonsów tras, korzystających z bardziej szczegółowych zakresów adresów.
>
> Środowiska usługi aplikacji nie są obsługiwane w przypadku konfiguracji usługi ExpressRoute, które między anonsować tras z publicznej komunikacji równorzędnej ścieżki do ścieżki prywatnej komunikacji równorzędnej. Konfiguracji usługi ExpressRoute z publicznej komunikacji równorzędnej skonfigurowane otrzymywać anonsów tras od firmy Microsoft. Anonse zawiera zbiór duże zakresy adresów IP firmy Microsoft Azure. W przypadku zakresów adresów między anonsowany w ścieżce prywatnej komunikacji równorzędnej, wszystkie pakiety wychodzącego z podsieci środowiska usługi aplikacji są życie tunel do infrastruktury sieci lokalnej klienta. Ten przepływ sieci nie jest obecnie obsługiwane przez wartość domyślną środowiska usługi App Service. Jedno rozwiązanie tego problemu jest zatrzymanie tras między reklamy ze ścieżki publicznej komunikacji równorzędnej do ścieżki prywatnej komunikacji równorzędnej. Innym rozwiązaniem jest umożliwienie środowiska usługi aplikacji do pracy w konfiguracji tunelowania wymuszonego.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Zmiana punktu końcowego wyjście środowiska usługi aplikacji ##

W tej sekcji opisano sposób włączania środowiska usługi aplikacji na działanie w ramach konfiguracji tunelowania wymuszonego, zmieniając wyjście punktu końcowego używane przez środowisko usługi aplikacji. Jeśli ruch wychodzący z środowiska usługi aplikacji jest życie tunneled do sieci lokalnej, należy zezwolić ruch do źródła z adresów IP innego niż adres VIP środowiska usługi aplikacji.

Środowiska usługi aplikacji nie tylko zależności zewnętrzne, ale również musi nasłuchiwania dla ruchu przychodzącego i odpowiadać na takie ruchu. Nie można wysłać odpowiedzi z innego adresu, ponieważ który dzieli TCP. Istnieją trzy kroki wymagane do zmiany wyjście punktu końcowego dla środowiska usługi aplikacji:

1. Ustaw tabelę tras, aby upewnić się, że ruch związany z zarządzaniem przychodzącej można powrocie z tego samego adresu IP.

2. Dodaj adresy IP, które mają być używane za wyjście do zapory środowiska usługi aplikacji.

3. Ustaw tras na ruch wychodzący z środowiska usługi aplikacji, aby być tunel.

   ![Wymuszone tunelu sieci przepływu][1]

Środowiska usługi aplikacji można skonfigurować przy użyciu różnych wyjście adresów po środowiska usługi aplikacji jest już w górę i operacyjne lub mogą być ustawione podczas wdrażania środowiska usługi aplikacji.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Zmień adres Wyjście po działa środowiska usługi aplikacji ###
1. Uzyskaj adresy IP, które mają zostać użyte jako wyjście adresów IP dla środowiska usługi aplikacji. Jeśli podczas wykonywania wymuszanie tunelowania, te adresy pochodzą z translatorami adresów sieciowych lub adresów IP bramy. Można przekierować ruch wychodzący środowiska usługi aplikacji za pośrednictwem NVA należy adres wyjście jest publiczny adres IP analizę NVA.

2. Ustaw adres wyjście w informacje o konfiguracji środowiska usługi aplikacji. Przejdź do resource.azure.com i przejdź do subskrypcji /<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Następnie można zobaczyć JSON, który opisuje środowiska usługi aplikacji. Upewnij się, że wyświetlany jest tekst **odczytu/zapisu** u góry. Wybierz **Edytuj**. Przewiń w dół, a następnie zmień **userWhitelistedIpRanges** wartość z **null** podobną następujące. Użyj adresów, które chcesz ustawić jako zakres adresów wyjście. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Wybierz **PUT** u góry. Ta opcja wyzwala operację skalowania środowiska usługi aplikacji i dostosowuje zapory.
 
3. Utwórz lub Edytuj tabelę tras i wypełnić reguły, aby zezwolić na dostęp z adresów zarządzania, które mapują do lokalizacji środowiska usługi aplikacji. Aby znaleźć adres zarządzania, zobacz [adresy zarządzania środowiska usługi aplikacji][management].

4. Dostosuj trasy stosowana do podsieci środowiska usługi aplikacji z tabelą tras lub trasy protokołu BGP. 

Jeśli środowisko usługi aplikacji przestanie odpowiadać z portalu, występuje problem ze zmianami. Może to oznaczać listy adresów wyjście nie zostało ukończone, ruch zostało utracone lub ruch został zablokowany. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Tworzenie nowego środowiska usługi aplikacji za pomocą różnych wyjście adresu ###

Jeśli w Twojej sieci wirtualnej jest już skonfigurowana tak, aby wymusić tunelu cały ruch, należy wykonać dodatkowe kroki, aby utworzyć środowiska usługi aplikacji, dzięki czemu może ona pomyślnie. Należy korzystać z innym punktem końcowym wyjście podczas tworzenia środowiska usługi aplikacji. Aby to zrobić, należy utworzyć środowiska usługi aplikacji z szablonu, który określa adresy dozwolonych wyjście.

1. Uzyskaj adresy IP do użycia jako adresy wyjście dla środowiska usługi aplikacji.

2. Utwórz wstępnie podsieci, które mają być używane przez środowisko usługi aplikacji. Będzie potrzebny, dzięki czemu można ustawić tras, a także ponieważ szablon wymaga.

3. Utwórz tabelę tras z zarządzaniem adresów IP, które są mapowane do lokalizacji środowiska usługi aplikacji. Przypisz je do środowiska usługi aplikacji.

4. Postępuj zgodnie z instrukcjami w [tworzenie środowiska usługi aplikacji przy użyciu szablonu][template]. Rozwiń odpowiedni szablon.

5. Edytuj plik azuredeploy.json sekcji "zasoby". Uwzględnij wiersz dla **userWhitelistedIpRanges** własnymi wartościami następująco:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Jeśli ta sekcja jest prawidłowo skonfigurowany, środowiska usługi aplikacji powinny rozpoczynać żadnych problemów. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
