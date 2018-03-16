---
title: "Omówienie usługi Azure Standard modułu równoważenia obciążenia | Dokumentacja firmy Microsoft"
description: "Przegląd funkcji standardowego modułu równoważenia obciążenia Azure"
services: load-balancer
documentationcenter: na
author: KumudD
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2018
ms.author: kumud
ms.openlocfilehash: 2d7fcb3ee066fa768615fbf643a0c2e1c1d28498
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="azure-load-balancer-standard-overview-preview"></a>Omówienie usługi Azure Standard modułu równoważenia obciążenia (wersja zapoznawcza)

Standardowy SKU usługi równoważenia obciążenia Azure i publicznego adresu IP standardowy SKU razem umożliwiają tworzenie architektur wysoce skalowalne i niezawodne. Aplikacji używających standardowego modułu równoważenia obciążenia można korzystać z nowych funkcji. Małe opóźnienia, wysokiej wydajności i skalowania są dostępne dla miliony przepływów dla wszystkich aplikacji TCP i UDP.

>[!NOTE]
> Standardowy SKU usługi równoważenia obciążenia jest obecnie w przeglądzie. Podczas udostępniania wersji zapoznawczej funkcja może nie mieć taki sam poziom dostępności i niezawodności jako funkcje, które są zwykle dostępności wersji. Aby uzyskać więcej informacji, zobacz [Dodatkowe warunki użytkowania dotyczące wersji zapoznawczych platformy Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Użyj ogólnie dostępna [podstawowy SKU usługi równoważenia obciążenia](load-balancer-overview.md) dla usług produkcji. Do użycia [Podgląd stref dostępności](https://aka.ms/availabilityzones) w tej wersji zapoznawczej wymaga [oddzielne rejestracji](https://aka.ms/availabilityzones), oprócz skorzystania z usługi równoważenia obciążenia [standardowe Podgląd](#preview-sign-up).

## <a name="why-use-load-balancer-standard"></a>Dlaczego warto używać standardowego modułu równoważenia obciążenia?

Używając standardowe usługi równoważenia obciążenia dla pełnego zakresu centrów danych wirtualnych. Małych wdrożeń do dużych i złożonych architektury wielu strefy Użyj standardowego modułu równoważenia obciążenia mógł korzystać z następujących funkcji:

- [Skalowanie przedsiębiorstwa](#enterprisescale) może zostać osiągnięty przy standardowe usługi równoważenia obciążenia. Ta funkcja może być używana z dowolnego wystąpienia maszyny wirtualnej (VM) w ramach sieci wirtualnej, maksymalnie 1000 wystąpień maszyn wirtualnych.

- [Nowych danych diagnostycznych](#diagnosticinsights) można ułatwić zrozumienie, zarządzanie i rozwiązywanie problemów z tym istotnym składnikiem Centrum danych wirtualnego. Pokaż, filtrować i grupować nowe metryki wielowymiarowej do pomiary kondycji ścieżki ciągłymi danymi, należy użyć Monitora Azure (wersja zapoznawcza). Monitorowanie danych z frontonu do maszyny Wirtualnej, sondy kondycji punktu końcowego, prób połączenia TCP oraz połączeń wychodzących.

- [Sieciowe grupy zabezpieczeń](#nsg) są teraz wymagane dla każdego wystąpienia maszyny Wirtualnej skojarzonej z standardowe jednostki SKU usługi równoważenia obciążenia lub publicznego adresu IP SKU standardowa. Grupy zabezpieczeń sieci (NSG) stanowią zwiększone zabezpieczenia dla danego scenariusza.

- [Wysoka niezawodność zapewnienia wysokiej dostępności (HA) porty](#highreliability) i skali dla wirtualnych urządzeń sieciowych (NVAs) i innych scenariuszy aplikacji. Obciążenia porty HA równoważenie wszystkie porty na Azure wewnętrzny załadować równoważenia (ILB) frontonu z pulą wystąpień maszyn wirtualnych.

- [Połączenia wychodzące](#outboundconnections) teraz używać nowego modelu do alokacji portu źródłowego sieci adresu tłumaczenia (SNAT), który zapewnia większą elastyczność i skali.

- [Obciążenia równoważenia standardowa ze strefami dostępności](#availabilityzones) może służyć do tworzenia strefowo nadmiarowy i zonal architektury. Obie architektury te mogą obejmować Równoważenie obciążenia między strefy. Można osiągnąć redundancję strefy bez zależności na rekordy DNS. Pojedynczy adres IP jest strefowo nadmiarowy domyślnie.  Pojedynczy adres IP dostęp można uzyskać żadnej maszyny Wirtualnej w sieci wirtualnej w regionie, który jest we wszystkich strefach dostępności.


Umożliwia standardowe usługi równoważenia obciążenia w konfiguracji publicznego ani wewnętrznego obsługuje następujących scenariuszy podstawowych:

- Ruch przychodzący do dobrej kondycji wystąpień zaplecza Równoważenie obciążenia.
- Port do przodu ruch przychodzący do pojedynczego wystąpienia zaplecza.
- Tłumaczenie ruch wychodzący z prywatnego adresu IP w sieci wirtualnej do publicznego adresu IP.

### <a name = "enterprisescale"></a>Skalowanie przedsiębiorstwa

 Użyj standardowego modułu równoważenia obciążenia do projektowania centrum danych wirtualnego wysokiej wydajności i obsługi dowolnej aplikacji TCP lub UDP. Użyj wystąpień autonomicznych maszyn wirtualnych lub maksymalnie 1000 wystąpień maszyn wirtualnych w puli zaplecza zestawach skali. Nadal używać przekazywania niskim opóźnieniem, wysokiej przepływności, wydajności i skalowanie do milionów przepływów na w pełni zarządzana usługa platformy Azure.
 
Standardowe usługi równoważenia obciążenia można przekazywać ruch do dowolnego wystąpienia maszyny Wirtualnej w sieci wirtualnej w regionie. Rozmiary puli zaplecza może mieć maksymalnie 1000 wystąpień z dowolną kombinację następujących scenariuszy maszyny Wirtualnej:

- Maszyny wirtualne autonomiczny bez zestawów dostępności
- Maszyny wirtualne autonomiczny z zestawów dostępności
- Zestawy skalowania maszyny wirtualnej, maksymalnie 1000 wystąpień
- Ustawia wiele skalowania maszyny wirtualnej
- Mieszanka maszyn wirtualnych i zestawy skalowania maszyny wirtualnej

Istnieje już wymóg zestawów dostępności. Można użyć zestawów dostępności dla korzyści, które zapewniają.

### <a name = "diagnosticinsights"></a>Szczegółowe informacje diagnostyczne

Standardowe usługi równoważenia obciążenia udostępnia nowe funkcje diagnostyczne wielowymiarowych dla publicznych i wewnętrznych konfiguracji usługi równoważenia obciążenia. Te nowe metryki są realizowane za pośrednictwem Monitora Azure (wersja zapoznawcza) i korzystać ze wszystkich możliwości pokrewnych, w tym możliwość integracji z podrzędne konsumentów.

| Metryka | Opis |
| --- | --- |
| Dostępność adresu VIP | Standardowe usługi równoważenia obciążenia stale korzysta ze ścieżki danych z w obrębie regionu frontonu aż do stosu SDN, który obsługuje maszyny Wirtualnej modułu równoważenia obciążenia. Tak długo, jak utrzymać dobrej kondycji wystąpień, pomiar następuje taką samą ścieżkę aplikacji z równoważeniem obciążenia ruchu. Ścieżka danych, który jest używany przez klientów również jest weryfikowana. Miara jest niewidoczny dla aplikacji i nie koliduje z innych operacji.|
| Dostępność DIP | Standardowe usługi równoważenia obciążenia używa rozproszonej kondycję sondowanie usługi, który monitoruje kondycję punkt końcowy aplikacji zgodnie z ustawieniami konfiguracji. Ta metryka zapewnia agregacji lub na punkt końcowy filtrowane widoku każdego punktu końcowego poszczególnych wystąpień w usłudze równoważenia obciążenia w puli.  Widać, jak usługa równoważenia obciążenia widoków kondycji aplikacji wskazywany przez konfigurację sondy kondycji.
| SYN pakietów | Standardowe usługi równoważenia obciążenia nie przerwanie połączenia TCP lub interakcji z przepływów pakietów TCP lub UDP. Przepływy i ich uzgodnienia są zawsze między serwerem źródłowym a wystąpienia maszyny Wirtualnej. Aby lepiej rozwiązać scenariuszy protokołu TCP, możesz wprowadzić użycie SYN liczniki pakietów, aby zrozumieć, jak wiele połączeń TCP prób. Metryka raporty o liczbie pakietów TCP SYN, które zostały odebrane.|
| SNAT połączenia | Standardowe usługi równoważenia obciążenia zgłasza liczbę przepływów wychodzących, które są masqueraded do publicznego adresu IP frontonu. Porty SNAT są zużywalnymi zasobami. Ta metryka może przedstawiać jak silnie aplikacji jest oparte na SNAT dla zdalnych przepływów wychodzących.  Liczniki dla udane i nieudane przepływów wychodzących SNAT są raportowane i może służyć do rozwiązywania oraz zrozumienie kondycji przepływów wychodzących.|
| Liczniki bajtów | Standardowe usługi równoważenia obciążenia zgłosił danych przetwarzanych na frontonu.|
| Liczniki pakietów | Standardowe usługi równoważenia obciążenia raporty pakietów przetwarzanych na frontonu.|

### <a name = "highreliability"></a>Wysoka niezawodność

Skonfiguruj reguły, aby udostępnić skali Twojej aplikacji i można zdecydowanie niezawodnej równoważenia obciążenia. Można skonfigurować reguł dla poszczególnych portów lub porty HA umożliwia zrównoważenie cały ruch niezależnie od tego, numer portu TCP lub UDP.  

Nowa funkcja HA porty służy do odblokowania różnych scenariuszy, w tym dla wewnętrznego NVAs wysokiej dostępności i skalowalności. Funkcja jest przydatna w innych sytuacjach, w którym jest niepraktyczne lub niepożądane, aby określić poszczególnych portów. Porty HA zapewnić nadmiarowość i skali, zezwalając dowolną liczbę wystąpień zgodnie z potrzebami. Konfiguracja nie jest już ograniczona do aktywny/pasywny scenariuszy. Konfiguracje sondy kondycji chronić usługi przesyłania dalej ruchu tylko do wystąpień dobrej kondycji.

Scenariusze obsługiwane dostawcy, odpornych można udostępnić klientom NVA dostawców. Obsługuje wiele aktywnych wystąpień skali i pojedynczego punktu awarii zostanie usunięta. Można skalować do dwóch lub więcej wystąpień, w zależności od możliwości urządzenia. Skontaktuj się z producentem NVA dodatkowych wskazówek dotyczących następujących scenariuszy.

### <a name = "availabilityzones"></a>Dostępność strefy

[!INCLUDE [availability-zones-preview-statement](../../includes/availability-zones-preview-statement.md)]

Wcześniejsze aplikacji odporność dzięki dostępności stref obsługiwanych regionów. Dostępność strefy są obecnie w wersji zapoznawczej w określonych regionach i wymaga dodatkowych uczestnictwa w.

### <a name="automatic-zone-redundancy"></a>Automatic zone-redundancy

Można wybrać, czy usługa równoważenia obciążenia powinno zapewniać strefowo nadmiarowy lub zonal frontonu dla poszczególnych aplikacji. Jest łatwo tworzyć nadmiarowość stref z standardowe usługi równoważenia obciążenia. Pojedynczy adres IP frontonu jest automatycznie strefowo nadmiarowy. Strefowo nadmiarowy frontonu jest obsługiwana przez wszystkie strefy dostępności w regionie jednocześnie. Ścieżka danych strefowo nadmiarowy jest tworzona dla połączeń przychodzących i wychodzących. Nadmiarowość stref na platformie Azure nie wymaga się wiele adresów IP i rekordy DNS. 

Nadmiarowość stref jest dostępna dla publicznego ani wewnętrznego zakończenia przód. Publiczny adres IP, a frontonu prywatnego adresu IP dla wewnętrznego modułu równoważenia obciążenia można strefowo nadmiarowy.

Użyj następującego skryptu do utworzenia strefowo nadmiarowy adresu publicznego adresu IP dla wewnętrznego modułu równoważenia obciążenia. Jeśli używasz istniejących szablonów usługi Resource Manager w konfiguracji, należy dodać **sku** sekcji, aby te szablony.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

Poniższy skrypt umożliwia utworzenie strefowo nadmiarowy IP frontonu dla wewnętrznego modułu równoważenia obciążenia. Jeśli używasz istniejących szablonów usługi Resource Manager w konfiguracji, należy dodać **sku** sekcji, aby te szablony.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zone_redundant_frontend",
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

Jeśli Twoje publicznego adresu IP frontonu strefowo nadmiarowy, połączeń wychodzących, które składają się z wystąpień maszyny Wirtualnej automatycznie staje się strefowo nadmiarowy. Frontonu jest chroniony przed awarią strefy. Z alokacją portów SNAT również przeżyje błąd stref.

#### <a name="cross-zone-load-balancing"></a>Równoważenie obciążenia między strefy

Równoważenie obciążenia między strefy jest dostępna w regionie puli zaplecza i oferuje maksymalną elastyczność wystąpień maszyn wirtualnych. Fronton dostarcza przepływów do żadnej maszyny Wirtualnej w sieci wirtualnej, niezależnie od dostępności strefy wystąpienia maszyny Wirtualnej.

Można również określić określonej strefy dla swoich wystąpień frontonu i zaplecza, aby były wyrównane z ścieżki danych i zasobów z określonej strefy.

Sieci wirtualne i podsieci nigdy nie są ograniczone przez strefy. Wystarczy zdefiniować pulę zaplecza z żądaną wystąpień maszyn wirtualnych i konfiguracji została ukończona.

#### <a name="zonal-deployments"></a>Zonal wdrożenia

Opcjonalnie można wyrównać frontonu do określonej strefy moduł równoważenia obciążenia, definiując zonal frontonu. Zonal frontonu obsługiwanej przez wyznaczony jednej dostępności strefy tylko. Gdy frontonu jest połączone z zonal wystąpień maszyn wirtualnych, można wyrównać zasobów do określonej strefy.

Do publicznego adresu IP utworzonego w określonej strefy zawsze istnieje tylko w tej strefie. Nie jest możliwa zmiana strefy do publicznego adresu IP. Dla adresu publicznego adresu IP, który można dołączyć do zasobów w wielu strefach należy zamiast tego utworzyć strefowo nadmiarowy publicznego adresu IP.

Poniższy skrypt umożliwia utworzenie zonal adres publiczny adres IP 1 strefy dostępności. Jeśli używasz istniejących szablonów usługi Resource Manager w konfiguracji, należy dodać **sku** sekcji, aby te szablony.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "zones": [ "1" ],
            "sku":
            {
                "name": "Standard"
            },
```

Użyj następującego skryptu można utworzyć wewnętrznego modułu równoważenia obciążenia frontonu do 1 strefy dostępności.

Jeśli używasz istniejących szablonów usługi Resource Manager w konfiguracji, należy dodać **sku** sekcji, aby te szablony. Ponadto zdefiniować **stref** właściwości konfiguracji IP frontonu dla zasobu podrzędnego.

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "zonal_frontend_in_az1",
                        "zones": [ "1" ],
                        "properties": {
                            "subnet": {
                                "Id": "[variables('subnetRef')]"
                            },
                            "privateIPAddress": "10.0.0.6",
                            "privateIPAllocationMethod": "Static"
                        }
                    },
                ],
```

Dodaj strefy między równoważenia obciążenia dla pulę zaplecza ustawiając wystąpień maszyn wirtualnych znajdujących się w sieci wirtualnej do puli.

Standardowe usługi równoważenia obciążenia zasobów jest zawsze regionalnego i strefowo nadmiarowy gdzie dostępność strefy są obsługiwane. Można wdrożyć publicznego adresu IP lub wewnętrzny moduł równoważenia obciążenia frontonu nie ma przypisanej strefy w dowolnym regionie. Obsługa dostępności strefy bez wpływu na możliwość wdrażania. Jeśli region uzyska później dostępności stref, wcześniej wdrożona publicznych adresów IP lub wewnętrzny moduł równoważenia obciążenia przodu końców automatycznie stanie się strefowo nadmiarowy. Ścieżka danych strefowo nadmiarowy nie oznacza utraty pakietów 0%.

### <a name = "nsg"></a>Grupy zabezpieczeń sieci

Załadować równoważenia Standard i publicznego adresu IP Standard pełni podłączone do sieci wirtualnej, która wymaga korzystania z grup zabezpieczeń sieci (NSG). Grupy NSG umożliwiać przepływem ruchu listy dozwolonych adresów IP. Grupy NSG można użyć do przejęcia pełnej kontroli nad ruchu dla danego wdrożenia. Nie masz już czekać na innych przepływów ruchu sieciowego zakończyć.

Kojarzenie grupy NSG z podsieciami lub interfejsów sieciowych (NIC) wystąpień maszyn wirtualnych w puli zaplecza. Za pomocą tej konfiguracji standardowe usługi równoważenia obciążenia i publicznego adresu IP Standard, gdy jest używany jako publiczny adres IP poziomie wystąpienia. Grupa NSG musi być jawnie dozwolonego ruchu, który chcesz zezwolić, aby na ten ruch.

Aby dowiedzieć się więcej na temat grup NSG i zastosować je dla danego scenariusza, zobacz [grup zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md).

### <a name ="outboundconnections"></a>Połączenia wychodzące

Standardowe usługi równoważenia obciążenia zapewnia połączeń wychodzących dla maszyn wirtualnych, które znajdują się w sieci wirtualnej podczas SNAT maskaradę portu korzysta z usługi równoważenia obciążenia. Algorytm SNAT maskaradę portu zapewnia zwiększonej niezawodności i skalowalności.

Gdy zasób publiczny moduł równoważenia obciążenia jest powiązana z wystąpień maszyn wirtualnych, każde źródło połączenia wychodzącego jest napisany od nowa. Źródło jest napisany od nowa z przestrzeń adresów IP prywatnej sieci wirtualnej adres publiczny adres IP frontonu modułu równoważenia obciążenia.

Gdy są używane połączenia wychodzące z strefowo nadmiarowy frontonu, połączeń są również strefowo nadmiarowy i alokacji portu SNAT przetrwać błąd stref.

Nowy algorytm w standardowe usługi równoważenia obciążenia preallocates SNAT portów do każdej maszyny wirtualnej karty Sieciowej. Jeśli karta sieciowa zostanie dodany do puli, porty SNAT są wstępnie przydzielonych na podstawie rozmiaru puli. W poniższej tabeli przedstawiono preallocations portu sześciu warstw wielkości puli zaplecza:

| Rozmiar puli (wystąpień maszyny Wirtualnej) | Przydzielony wstępnie liczbę portów SNAT |
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

Porty SNAT bezpośrednio nie wykonuje do liczby połączeń wychodzących. SNAT port mogą być ponownie używane dla wielu celów unikatowy. Aby uzyskać szczegółowe informacje, przejrzyj [połączeń wychodzących](load-balancer-outbound-connections.md) artykułu.

Jeśli rozmiar puli zaplecza zwiększa i przejścia do wyższego poziomu, połowy Twojej przydzielonych portów odzyskane. Połączenia, które są skojarzone z regeneracji przekroczenie limitu czasu i należy ustanowić. Próby połączenia powiedzie się natychmiast. Jeśli zmniejsza rozmiar puli zaplecza i zwiększyć liczbę dostępnych portów SNAT przejścia do dolnej warstwy. W takim przypadku nie wpływa na istniejące połączenia.

Standardowe usługi równoważenia obciążenia ma opcja dodatkowe czynności konfiguracyjne, które może być używana na na podstawie reguł. Można kontrolować, które frontonu służy do maskaradę portu SNAT po wielu zakończenia przód są dostępne.

Gdy tylko obciążenia równoważenia standardowe służy wystąpień maszyn wirtualnych, połączeń wychodzących SNAT nie są dostępne. Możesz przywrócić tę możliwość jawnie również przypisywanie wystąpień maszyn wirtualnych do publiczny moduł równoważenia obciążenia. Publiczne adresy IP można również bezpośrednio przypisać jako poziomie wystąpienia publiczne adresy IP do każdego wystąpienia maszyny Wirtualnej. Ta opcja konfiguracji może być wymagane dla niektórych środowisk aplikacji i systemu operacyjnego. 

### <a name="port-forwarding"></a>Przekierowanie portów

Podstawowe i standardowe moduły równoważenia obciążenia zapewniają możliwość konfigurowania reguł NAT ruchu przychodzącego do mapowania portów frontonu poszczególne wystąpienia zaplecza. Po skonfigurowaniu tych reguł, udostępnić punkty końcowe protokołu Remote Desktop Protocol i SSH lub wykonywać inne scenariusze aplikacji.

Standardowe usługi równoważenia obciążenia w dalszym ciągu umożliwić przekierowania portów za pomocą reguł NAT dla ruchu przychodzącego. W przypadku użycia z strefowo nadmiarowy zakończenia przód, reguł NAT dla ruchu przychodzącego stają się strefowo nadmiarowy i awarie strefy.

### <a name="multiple-front-ends"></a>Wiele zakończenia przód

Skonfiguruj wiele zakończenia przód elastyczność projektu, jeśli aplikacje wymagają wielu pojedyncze adresy IP uwidocznienia, takich jak TLS witryn sieci Web lub punktów końcowych grupy dostępności funkcji AlwaysOn programu SQL. 

Standardowe usługi równoważenia obciążenia w dalszym ciągu zapewniają wiele zakończenia przód wymagających ujawniać punkt końcowy określonej aplikacji na unikatowy adres IP.

Aby uzyskać więcej informacji na temat konfigurowania wielu adresów IP frontonu, zobacz [konfiguracji wielu adresów IP](load-balancer-multivip-overview.md).

## <a name = "sku"></a>O jednostki SKU

Jednostki SKU są dostępne tylko w modelu wdrażania usługi Azure Resource Manager. Tej wersji zapoznawczej wprowadzono dwie jednostki SKU dla zasobów usługi równoważenia obciążenia i publiczny adres IP: Basic i Standard. Jednostki SKU różnią się możliwości, charakterystyki wydajności, ograniczenia i niektóre zachowania wewnętrznych. Maszyny wirtualne można łączyć z jednej jednostki SKU. W przypadku zasobów zarówno usługi równoważenia obciążenia i publiczny adres IP jednostki SKU pozostają opcjonalne atrybuty. W przypadku jednostki SKU zostały pominięte w definicji scenariusz, konfiguracja granicznego przy użyciu podstawowy SKU.

>[!IMPORTANT]
>Jednostka SKU zasobu nie jest modyfikowalna. Nie możesz zmienić numer istniejącego zasobu.  

### <a name="load-balancer"></a>Moduł równoważenia obciążenia

[Istniejący zasób usługi równoważenia obciążenia](load-balancer-overview.md) staje się podstawowy SKU i pozostaje ogólnie dostępna i bez zmian.

Standardowy SKU usługi równoważenia obciążenia jest nowy i obecnie w wersji zapoznawczej. Dodaje wersja interfejsu API dla Microsoft.Network/loadBalancers 1 sierpnia 2017 r. **sku** właściwości w definicji zasobu:

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/loadBalancers",
            "name": "load_balancer_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```
Standardowe usługi równoważenia obciążenia jest automatycznie strefy był odporny w regionach, które oferują stref dostępności. Jeśli usługa równoważenia obciążenia został zadeklarowany zonal, następnie nie jest automatycznie strefy odporność.

### <a name="public-ip"></a>Publiczny adres IP

[Istniejącego publicznego adresu IP zasobu](../virtual-network/virtual-network-ip-addresses-overview-arm.md) staje się podstawowy SKU i pozostaje ogólnie dostępna, biorąc pod uwagę jej możliwości, charakterystyki wydajności i ograniczeń.

Standardowy SKU publicznego adresu IP jest nowy i obecnie w wersji zapoznawczej. Dodaje wersja interfejsu API dla Microsoft.Network/publicIPAddresses 1 sierpnia 2017 r. **sku** właściwości w definicji zasobu:

```json
            "apiVersion": "2017-08-01",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "public_ip_standard",
            "location": "region",
            "sku":
            {
                "name": "Standard"
            },
```

W przeciwieństwie do publicznego adresu IP podstawowe, które oferuje wiele metod alokacji, publicznego adresu IP standardowe zawsze używa statycznych alokacji.

Standardowa publicznego adresu IP jest automatycznie strefy był odporny w regionach, które oferują stref dostępności. Jeśli publiczny adres IP został zadeklarowany zonal, następnie nie jest automatycznie strefy odporność. Zonal publicznego adresu IP nie można zmienić z jedną strefę na inny.

## <a name="migration-between-skus"></a>Migracja między jednostki SKU

Jednostki SKU nie jest modyfikowalna. Wykonaj kroki opisane w tej sekcji, aby przenieść od jednego zasobu jednostki SKU.

### <a name="migrate-from-basic-to-standard-sku"></a>Migracja z podstawowego do wersji Standard

1. Tworzenie nowego zasobu standardowe (Usługa równoważenia obciążenia i publiczne adresy IP, w razie potrzeby). Ponownie utworzyć reguły i definicje sondowania.

2. Utworzyć lub zaktualizować istniejący NSG na karcie Sieciowej lub podsieci do dozwolonego ruchu o zrównoważonym obciążeniu, sondy, a także cały ruch, który chcesz zezwolić.

3. Usuń zasoby podstawowy SKU (Usługa równoważenia obciążenia i publiczne adresy IP, jeśli ma zastosowanie) z wszystkich wystąpień maszyn wirtualnych. Należy również usunięcie wszystkich wystąpień maszyn wirtualnych zestawu dostępności.

4. Dołącz wszystkie wystąpienia maszyny Wirtualnej do nowych zasobów standardowy SKU.

### <a name="migrate-from-standard-to-basic-sku"></a>Migracja z Standard do podstawowy SKU

1. Tworzenie nowego zasobu podstawowego (Usługa równoważenia obciążenia i publiczne adresy IP, w razie potrzeby). Ponownie utworzyć reguły i definicje sondowania. 

2. Usuń zasoby standardowy SKU (Usługa równoważenia obciążenia i publiczne adresy IP, jeśli ma zastosowanie) z wszystkich wystąpień maszyn wirtualnych. Należy również usunięcie wszystkich wystąpień maszyn wirtualnych zestawu dostępności.

3. Dołącz wszystkie wystąpienia maszyny Wirtualnej do nowych zasobów podstawowy SKU.

>[!IMPORTANT]
>
>Istnieją pewne ograniczenia dotyczące użycia Basic i Standard jednostki SKU.
>
>HA portów i informacji diagnostycznych z wersji Standard są dostępne tylko w wersji Standard. Nie można migracji z wersji Standard na podstawowy SKU i również zachować te funkcje.
>
>Dla zasobów usługi równoważenia obciążenia i publiczny adres IP muszą być używane dopasowanie jednostki SKU. Nie może mieć kombinację zasobów podstawowy SKU i wersji Standard. Nie można dołączyć maszyny Wirtualnej, maszynach wirtualnych w zestawie dostępności lub zestawu do obu SKU jednocześnie skalowania maszyn wirtualnych.
>

## <a name="region-availability"></a>Dostępność w danym regionie

Standardowe usługi równoważenia obciążenia jest obecnie dostępny we wszystkich regionach chmury publicznej, z wyjątkiem zachodnie stany USA.

>[!IMPORTANT]
> Dla krótkim czasie dostępu do regionów poza początkowej uruchamiania regionów (wschodnie stany USA 2, środkowe stany USA, Europa Północna, zachodnie centralnej nam, Europa Zachodnia, Azja południowo-wschodnia) wymaga rejestracji subskrypcji dodatkowe funkcje (AllowLBPreviewWave2 i AllowLBPreviewWave3).  [Wykonaj poniższe czynności](#additionalpreviewregions). Wykonaj wszystkie z nich nawet wtedy, gdy zostało wcześniej zarejestrowane w do AllowLBPreview już.
> To wymaganie zostanie usunięta w najbliższych tygodniach.

## <a name="sku-service-limits-and-abilities"></a>Ograniczenia usługi jednostki SKU i możliwości

Azure [ograniczenia usługi dotyczące sieci](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits) zastosować na region na subskrypcję. 

W poniższej tabeli porównano limity i możliwości usługi równoważenia obciążenia — podstawowe i standardowe jednostki SKU:

| Moduł równoważenia obciążenia | Podstawowa | Standardowa (Standard) |
| --- | --- | --- |
| Rozmiar puli zaplecza | do 100 | maksymalnie 1000 |
| Granic puli zaplecza | Zestaw dostępności | sieć wirtualna, region |
| Projekt puli zaplecza | Maszyny wirtualne w zestawie dostępności, skalowania maszyny wirtualnej w zestawie dostępności | Wystąpienie maszyny Wirtualnej w sieci wirtualnej |
| HA portów | Nieobsługiwane | Dostępna |
| Diagnostyka | Ograniczone, publicznego tylko | Dostępna |
| Dostępność adresu VIP  | Nieobsługiwane | Dostępna |
| Szybkie IP mobilności | Nieobsługiwane | Dostępna |
|Scenariusze stref dostępności | Tylko Zonal | Zonal, Strefowo nadmiarowy, równoważenie obciążenia między strefy |
| Algorytm SNAT ruchu wychodzącego | Na żądanie | Wstępnie przydzielonych |
| Wychodzące SNAT frontonu zaznaczenia. | Nie można skonfigurować wiele kandydatów | Opcjonalna konfiguracja, aby zmniejszyć kandydatów |
| Sieciowa grupa zabezpieczeń | Opcjonalnie w podsieci/karty Sieciowej | Wymagane |

W poniższej tabeli porównano limity i możliwości publicznego adresu IP podstawowe i standardowe jednostki SKU:

| Publiczny adres IP | Podstawowa | Standardowa (Standard) |
| --- | --- | --- |
| Scenariusze stref dostępności | Tylko Zonal | Strefowo nadmiarowy (ustawienie domyślne), zonal (opcjonalnie) | 
| Szybkie IP mobilności | Nieobsługiwane | Dostępna |
| Dostępność adresu VIP | Nieobsługiwane | Dostępna |
| Liczniki | Nieobsługiwane | Dostępna |
| Sieciowa grupa zabezpieczeń | Opcjonalnie na karcie interfejsu Sieciowego | Wymagane |


## <a name="preview-sign-up"></a>Podgląd rejestracji

Aby uczestniczyć w wersji Standard usługi równoważenia obciążenia i pomocnika standardowy SKU publicznego adresu IP w wersji zapoznawczej, zarejestruj subskrypcję.  Rejestracja umożliwia Twojej subskrypcji dostęp z programu PowerShell lub 2.0 interfejsu wiersza polecenia platformy Azure. Aby zarejestrować, wykonaj następujące czynności:

>[!NOTE]
>Rejestracji funkcji standardowego modułu równoważenia obciążenia może potrwać do godziny do zaczynają obowiązywać globalnie. Jeśli chcesz użyć obciążenia równoważenia standardowe z [stref dostępności](https://aka.ms/availabilityzones), [oddzielne rejestracji](https://aka.ms/availabilityzones) jest wymagany dla podglądu AZ.

<a name="additionalpreviewregions"></a>
>[!IMPORTANT]
> Dla krótkim czasie dostępu do regionów poza początkowej uruchamiania regionów (wschodnie stany USA 2, środkowe stany USA, Europa Północna, zachodnie centralnej nam, Europa Zachodnia, Azja południowo-wschodnia) wymagają rejestracji subskrypcji dodatkowe funkcje (AllowLBPreviewWave2 i AllowLBPreviewWave3).  Poniższe kroki został zmodyfikowany w celu włączenia funkcji dodatkowych subskrypcji. Wykonaj wszystkie z nich nawet wtedy, gdy zostało wcześniej zarejestrowane w do AllowLBPreview już. To wymaganie zostanie usunięta w najbliższych tygodniach.


### <a name="sign-up-by-using-azure-cli-20"></a>Zaloguj przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0

1. Zarejestruj tę funkcję z dostawcą:

    ```cli
    az feature register --name AllowLBPreview --namespace Microsoft.Network
    az feature register --name AllowLBPreviewWave2 --namespace Microsoft.Network
    az feature register --name AllowLBPreviewWave3 --namespace Microsoft.Network
    ```
    
2. Operacja może potrwać do 10 minut. Można sprawdzić stan operacji za pomocą następującego polecenia:

    ```cli
    az feature list --query "[?name=='Microsoft.Network/AllowLBPreview']" --output json
    az feature list --query "[?name=='Microsoft.Network/AllowLBPreviewWave2']" --output json
    az feature list --query "[?name=='Microsoft.Network/AllowLBPreviewWave3']" --output json
    ```
    
    Gdy stan rejestracji funkcji zwraca "Zarejestrowanej" dla każdego z powyższych funkcji subskrypcji, należy przejść do następnego kroku. Przykład:
   
    ```json
    {
       "id": "/subscriptions/foo/providers/Microsoft.Features/providers/Microsoft.Network/features/AllowLBPreview",
       "name": "Microsoft.Network/AllowLBPreview",
       "properties": {
          "state": "Registered"
       },
       "type": "Microsoft.Features/providers/features"
    }
    ```
    
4. Ukończenie rejestracji Podgląd rejestrując ponownie subskrypcji u dostawcy zasobów:

    ```cli
    az provider register --namespace Microsoft.Network
    ```
    

### <a name="sign-up-by-using-powershell"></a>Zaloguj przy użyciu programu PowerShell

1. Zarejestruj tę funkcję z dostawcą:

    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    Register-AzureRmProviderFeature -FeatureName AllowLBPreviewWave2 -ProviderNamespace Microsoft.Network
    Register-AzureRmProviderFeature -FeatureName AllowLBPreviewWave3 -ProviderNamespace Microsoft.Network
    ```
    
2. Operacja może potrwać do 10 minut. Można sprawdzić stan operacji za pomocą następującego polecenia:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    Get-AzureRmProviderFeature -FeatureName AllowLBPreviewWave2 -ProviderNamespace Microsoft.Network
    Get-AzureRmProviderFeature -FeatureName AllowLBPreviewWave3 -ProviderNamespace Microsoft.Network
    ```

  Gdy stan rejestracji funkcji zwraca "Zarejestrowanej" dla każdego z powyższych funkcji subskrypcji, należy przejść do następnego kroku. Przykład:

    ```
    FeatureName      ProviderName        RegistrationState
    -----------      ------------        -----------------
    AllowLBPreview   Microsoft.Network   Registered
    ```
    
3. Ukończenie rejestracji Podgląd rejestrując ponownie subskrypcji u dostawcy zasobów:

    ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ```
 
## <a name="pricing"></a>Cennik

Karta wersji Standard usługi równoważenia obciążenia jest na podstawie skonfigurowanych reguł i przetworzone dane. Nie jest obciążany okresie używania wersji zapoznawczej. Aby uzyskać więcej informacji, przejrzyj [modułu równoważenia obciążenia](https://aka.ms/lbpreviewpricing) i [publicznego adresu IP](https://aka.ms/lbpreviewpippricing) cennik stron.

Klienci w dalszym ciągu korzystać podstawowy SKU usługi równoważenia obciążenia bez dodatkowych opłat.

## <a name="limitations"></a>Ograniczenia

Następujące ograniczenia obowiązują w czasie Podgląd i mogą ulec zmianie:

- W połączyć za pomocą sieci wirtualne w tej chwili nie można zlokalizować wystąpienia zaplecza modułu równoważenia obciążenia. Wszystkie wystąpienia zaplecza muszą być w tym samym regionie.
- Jednostki SKU nie jest modyfikowalna. Nie możesz zmienić numer istniejącego zasobu.
- Obie wersje produktu może być używany z autonomicznej maszyny Wirtualnej, wystąpień maszyny Wirtualnej w zestawie dostępności lub zestawu skalowania maszyn wirtualnych. Kombinacje maszyny Wirtualnej nie może być używana z obu SKU jednocześnie. Konfigurację, w której znajdują się różne jednostki SKU nie jest dozwolone.
- Wyłącza wystąpienie maszyny Wirtualnej (lub częścią zestawu dostępności) przy użyciu obciążenia równoważenia wzorca wewnętrznego [domyślne połączeń wychodzących SNAT](load-balancer-outbound-connections.md). Można przywrócić tej możliwości do autonomicznej maszyny Wirtualnej, wystąpień maszyn wirtualnych w zestawie dostępności lub zestaw skali maszyny wirtualnej. Można też przywrócić możliwość połączenia wychodzące. Aby przywrócić te możliwości, jednocześnie przypisać publicznego standardowe usługi równoważenia obciążenia, lub publicznego adresu IP standardowe jako poziomie wystąpienia publicznego adresu IP, w tym samym wystąpieniu maszyny Wirtualnej. Po zakończeniu przydziału maskaradę portu SNAT do publicznego adresu IP znajduje się ponownie.
- Wystąpień maszyn wirtualnych może być konieczne można grupować w zbiorów dostępności do osiągnięcia pełnej zaplecza puli skali. Maksymalnie 150 dostępności zestawów i autonomiczne maszyny wirtualne mogą być umieszczane w jednej puli zaplecza.
- Protokół IPv6 nie jest obsługiwany.
- W kontekście dostępności stref frontonu nie jest modyfikowalna z zonal do strefowo nadmiarowy lub na odwrót. Po fronton jest tworzony jako strefowo nadmiarowy, pozostaje strefowo nadmiarowy. Po fronton jest tworzony jako zonal, pozostaje zonal.
- W kontekście dostępności stref zonal adres publiczny adres IP nie można przenieść z jedną strefę na inny.
- [Alerty monitora Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md) nie są obsługiwane w tej chwili.
- Portal nie obsługuje jeszcze szerokim testowaniu regionów.  Użyj narzędzia klienta, takie jak szablony, Azure CLI w wersji 2.0 lub programu PowerShell, aby uniknąć tego problemu.
- [Subskrypcja operacje są przenoszone](../azure-resource-manager/resource-group-move-resources.md) nie są obsługiwane dla standardowych LB jednostki SKU i PIP zasobów.
- Nie jest dostępna w zachodnie stany USA.


## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej o [podstawowe usługi równoważenia obciążenia](load-balancer-overview.md).
- Dowiedz się więcej o [stref dostępności](../availability-zones/az-overview.md).
- Dowiedz się więcej o [grup zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md).
- Dowiedz się więcej o niektóre inne kluczowe [możliwości w zakresie obsługi](../networking/networking-overview.md) na platformie Azure.
- Dowiedz się więcej o [metryki widoczne](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftnetworkloadbalancers) w [Azure Monitor](../monitoring-and-diagnostics/monitoring-overview.md).
