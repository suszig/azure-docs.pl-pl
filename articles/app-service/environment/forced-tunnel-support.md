---
title: "Konfigurowanie wymuszonego tunelowania środowiska Azure App Service Environment"
description: "Umożliwianie działania środowiska App Service Environment w przypadku wymuszonego tunelowania ruchu wychodzącego"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 384cf393-5c63-4ffb-9eb2-bfd990bc7af1
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/10/2017
ms.author: ccompy
ms.custom: mvc
ms.openlocfilehash: 4caaf0df3f1dd4b2cb9b76283a6beed897531c1c
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="configure-your-app-service-environment-with-forced-tunneling"></a>Konfigurowanie wymuszonego tunelowania środowiska App Service Environment

Środowisko Azure App Service Environment to wdrożenie usługi Azure App Service w należącym do klienta wystąpieniu sieci Azure Virtual Network. Wielu klientów konfiguruje sieci wirtualne jako rozszerzenia sieci lokalnych za pomocą sieci VPN lub połączeń usługi Azure ExpressRoute. Ze względu na zasady firmowe lub inne ograniczenia zabezpieczeń konfigurują oni dla tras wysyłanie całego ruchu wychodzącego do środowiska lokalnego, zanim trafi on do Internetu. Zmiana routingu sieci wirtualnej w taki sposób, aby ruch wychodzący z sieci wirtualnej przechodził przez sieć VPN lub połączenie usługi ExpressRoute ze środowiskiem lokalnym, jest nazywana wymuszaniem tunelowania. 

Wymuszanie tunelowania może powodować problemy ze środowiskiem App Service Environment. Środowisko App Service Environment jest zależne od szeregu elementów zewnętrznych, które wymieniono w dokumencie [App Service Environment network architecture][network] (Architektura sieci w środowisku App Service Environment). Domyślnie środowisko App Service Environment wymaga, aby cała komunikacja wychodząca przechodziła przez wirtualny adres IP, dla którego zainicjowano obsługę tego środowiska.

Trasy mają kluczowe znaczenie dla wymuszonego tunelowania i sposobu jego obsługi. W sieci wirtualnej platformy Azure routing odbywa się na podstawie najdłuższego dopasowania prefiksu (LPM, Longest Prefix Match). Jeśli istnieje więcej niż jedna trasa z tym samym dopasowaniem LPM, wybór trasy odbywa się według następującej kolejności:

* Trasa zdefiniowana przez użytkownika
* Trasa protokołu BGP (jeśli używane są połączenia ExpressRoute)
* Trasa systemowa

Aby dowiedzieć się więcej na temat routingu w sieci wirtualnej, przeczytaj [User-defined routes and IP forwarding][routes] (Trasy zdefiniowane przez użytkownika i przesyłanie pakietów IP dalej). 

Jeśli środowisko App Service Environment ma działać w sieci wirtualnej z wymuszaniem tunelowania, dostępne są dwie opcje:

* Zapewnienie środowisku App Service Environment bezpośredniego dostępu do Internetu.
* Zmiana punktu końcowego ruchu wychodzącego dla środowiska App Service Environment.

## <a name="enable-your-app-service-environment-to-have-direct-internet-access"></a>Zapewnianie środowisku App Service Environment bezpośredniego dostępu do Internetu

Co można zrobić, aby środowisko App Service Environment działało, gdy dla sieci wirtualnej skonfigurowano połączenie usługi ExpressRoute:

* Skonfiguruj w usłudze ExpressRoute anonsowanie adresów 0.0.0.0/0. Domyślnie powoduje to wymuszenie tunelowania całego ruchu wychodzącego do środowiska lokalnego.
* Utwórz trasę zdefiniowaną przez użytkownika. Zastosuj ją do podsieci zawierającej środowisko App Service Environment z prefiksem adresu 0.0.0.0/0 i następnym przeskokiem typu Internet.

Po wprowadzeniu tych dwóch zmian ruch kierowany do Internetu pochodzący z podsieci środowiska App Service Environment nie będzie w sposób wymuszony kierowany do połączenia usługi ExpressRoute i środowisko ASE będzie działać.

> [!IMPORTANT]
> Trasy zdefiniowane przez użytkownika muszą być wystarczająco szczegółowe, aby miały pierwszeństwo względem wszelkich tras anonsowanych przez konfigurację usługi ExpressRoute. W poprzednim przykładzie użyto szerokiego zakresu adresów 0.0.0.0/0. Może to zostać przypadkowo zastąpione przez anonsy tras z bardziej szczegółowymi zakresami adresów.
>
> Środowiska App Service Environment nie są obsługiwane w przypadku konfiguracji usługi ExpressRoute z anonsowaniem krzyżowym tras ze ścieżki publicznej komunikacji równorzędnej do ścieżki prywatnej komunikacji równorzędnej. Konfiguracje usługi ExpressRoute ze skonfigurowaną publiczną komunikacją równorzędną otrzymują anonsy tras od firmy Microsoft. Te anonse zawierają duży zestaw zakresów adresów IP platformy Microsoft Azure. W przypadku anonsowania krzyżowego zakresów adresów w ścieżce prywatnej komunikacji równorzędnej wszystkie pakiety sieciowe wychodzące z podsieci środowiska App Service Environment będą w sposób wymuszony tunelowane do lokalnej infrastruktury sieciowej klienta. Ten przepływ sieciowy nie jest obecnie domyślnie obsługiwany w przypadku środowisk App Service Environment. Jednym z rozwiązań tego problemu jest zatrzymanie anonsowania krzyżowego tras ze ścieżki publicznej komunikacji równorzędnej do ścieżki prywatnej komunikacji równorzędnej. Innym jest umożliwienie środowisku App Service Environment działania w przypadku konfiguracji wymuszonego tunelowania.

## <a name="change-the-egress-endpoint-for-your-app-service-environment"></a>Zmienianie punktu końcowego ruchu wychodzącego dla środowiska App Service Environment ##

W tej sekcji opisano, jak umożliwić środowisku App Service Environment działanie w przypadku konfiguracji wymuszonego tunelowania przez zmianę punktu końcowego ruchu wychodzącego używanego przez to środowisko. Jeśli ruch wychodzący ze środowiska App Service Environment jest w sposób wymuszony tunelowany do sieci lokalnej, należy zezwolić na pochodzenie tego ruchu z adresów IP innych niż wirtualny adres IP tego środowiska.

Środowisko App Service Environment nie tylko jest zależne od elementów zewnętrznych, ale również musi nasłuchiwać w oczekiwaniu na ruch przychodzący i odpowiadać na taki ruch. Te odpowiedzi nie mogą być odsyłane spod innego adresu, ponieważ spowoduje to przerwanie toku komunikacji TCP. Zmiana punktu końcowego ruchu wychodzącego dla środowiska App Service Environment wymaga wykonania trzech kroków:

1. Ustaw tabelę tras w taki sposób, aby ruch przychodzący związany z zarządzaniem mógł powracać spod tego samego adresu IP.

2. Dodaj adresy IP, które mają być używane do obsługi ruchu wychodzącego, do zapory środowiska App Service Environment.

3. Skonfiguruj trasy, aby tunelować ruch wychodzący ze środowiska App Service Environment.

   ![Przepływ sieciowy z wymuszonym tunelowaniem][1]

Po włączeniu i potwierdzeniu działania środowiska App Service Environment można skonfigurować dla niego inne adresy ruchu wychodzącego. Ewentualnie można je skonfigurować w trakcie wdrażania tego środowiska.

### <a name="change-the-egress-address-after-the-app-service-environment-is-operational"></a>Zmienianie adresu ruchu wychodzącego po potwierdzeniu działania środowiska App Service Environment ###
1. Uzyskaj adresy IP, które mają być używane jako adresy IP ruchu wychodzącego środowiska App Service Environment. Jeśli jest używane wymuszanie tunelowania, te adresy pochodzą z translatora adresów sieciowych (NAT) lub są adresami IP bramy. Jeśli ruch wychodzący środowiska App Service Environment ma być kierowany przez urządzenie NVA, adres ruchu wychodzącego to publiczny adres IP urządzenia NVA.

2. Skonfiguruj adresy ruchu wychodzącego w informacjach o konfiguracji środowiska App Service Environment. Przejdź do witryny resource.azure.com, a następnie do obszaru Subscription/<subscription id>/resourceGroups/<ase resource group>/providers/Microsoft.Web/hostingEnvironments/<ase name>. Znajdziesz tam informacje w formacie JSON z opisem Twojego środowiska App Service Environment. Upewnij się, że u góry jest wyświetlana informacja **read/write** (odczyt/zapis). Wybierz pozycję **Edit** (Edytuj). Przewiń do samego dołu i zmień ustawienie pozycji **userWhitelistedIpRanges** z wartości **null** na wartość podobną do następującej. Użyj adresów, które chcesz ustawić jako zakres adresów ruchu wychodzącego. 

        "userWhitelistedIpRanges": ["11.22.33.44/32", "55.66.77.0/24"] 

   Wybierz pozycję **PUT** u góry. Ta opcja wyzwala operację skalowania środowiska App Service Environment i dostosowanie zapory.
 
3. Utwórz lub edytuj tabelę tras i dodaj reguły, aby zezwolić na dostęp do/z adresów zarządzania zamapowanych na lokalizację środowiska App Service Environment. Aby znaleźć adresy zarządzania, zobacz [App Service Environment management addresses][management] (Adresy zarządzania środowiska App Service Environment).

4. Dostosuj trasy zastosowane do podsieci środowiska App Service Environment za pomocą tabeli tras lub tras BGP. 

Jeśli środowisko App Service Environment przestanie reagować z poziomu portalu, występuje problem dotyczący wprowadzonych zmian. Problem może polegać na tym, że lista adresów ruchu wychodzącego jest niepełna, występuje utrata ruchu lub ruch jest blokowany. 

### <a name="create-a-new-app-service-environment-with-a-different-egress-address"></a>Tworzenie nowego środowiska App Service Environment z innym adresem ruchu wychodzącego ###

Jeśli w używanej sieci wirtualnej skonfigurowano już wymuszanie tunelowania całego ruchu, należy wykonać dodatkowe kroki, aby utworzyć takie środowisko App Service Environment, które zostanie pomyślnie włączone. W trakcie tworzenia środowiska App Service Environment należy umożliwić używanie innego punktu końcowego ruchu wychodzącego. Aby to zrobić, należy utworzyć środowisko App Service Environment za pomocą szablonu określającego dozwolone adresy ruchu wychodzącego.

1. Uzyskaj adresy IP, które mają być używane jako adresy ruchu wychodzącego środowiska App Service Environment.

2. Utwórz wstępnie podsieć, której będzie używać środowisko App Service Environment. Jest ona potrzebna, aby można było skonfigurować trasy, a także dlatego, że wymaga jej szablon.

3. Utwórz tabelę tras z adresami IP zarządzania zamapowanymi na lokalizację środowiska App Service Environment. Przypisz ją do środowiska App Service Environment.

4. Postępuj zgodnie z instrukcjami podanymi w temacie [Create an App Service Environment with a template][template] (Tworzenie środowiska App Service Environment przy użyciu szablonu). Uzyskaj odpowiedni szablon.

5. Edytuj sekcję „resources” (zasoby) w pliku azuredeploy.json. Uwzględnij wiersz elementu **userWhitelistedIpRanges** z własnymi wartościami, podobny do następującego:

       "userWhitelistedIpRanges":  ["11.22.33.44/32", "55.66.77.0/30"]

Jeśli ta sekcja jest skonfigurowana poprawnie, środowisko App Service Environment powinno zostać uruchomione bez żadnych problemów. 


<!--IMAGES-->
[1]: ./media/forced-tunnel-support/forced-tunnel-flow.png

<!--Links-->
[management]: ./management-addresses.md
[network]: ./network-info.md
[routes]: ../../virtual-network/virtual-networks-udr-overview.md
[template]: ./create-from-template.md
