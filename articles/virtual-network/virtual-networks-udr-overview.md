---
title: Co to są trasy zdefiniowane przez użytkownika i przesyłanie dalej IP?
description: Dowiedz się, w jaki sposób można korzystać z tras zdefiniowanych przez użytkownika i przesyłania dalej IP, aby przekazywać ruch w sieci do wirtualnych urządzeń sieciowych na platformie Azure.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn

ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial

---
# Co to są trasy zdefiniowane przez użytkownika i przesyłanie dalej IP?
Po dodaniu maszyny wirtualnej do sieci wirtualnej na platformie Azure można zauważyć, że maszyny wirtualne mogą automatycznie komunikować się ze sobą za pośrednictwem sieci. Nie ma potrzeby określania bramy, nawet gdy maszyny wirtualne znajdują się w różnych podsieciach. Dotyczy to także komunikacji z maszyn wirtualnych do publicznej sieci Internet, a nawet do sieci lokalnej, gdy obecne jest połączenie hybrydowe z platformy Azure do własnego centrum danych.

Taki przepływ komunikacji jest możliwy, ponieważ platforma Azure korzysta z szeregu tras systemowych do definiowania przepływu ruchu w sieci IP. Trasy systemowe sterują przepływem komunikacji według następujących scenariuszy:

* Z tej samej podsieci.
* Z jednej podsieci do drugiej w ramach sieci wirtualnej.
* Z maszyny wirtualnej do sieci Internet.
* Z sieci wirtualnej do innej sieci wirtualnej za pośrednictwem bramy sieci VPN.
* Z sieci wirtualnej do sieci lokalnej za pośrednictwem bramy sieci VPN.

Na poniższym rysunku przedstawiono prostą konfigurację obejmującą sieć wirtualną, dwie podsieci i kilka maszyn wirtualnych wraz trasami systemowymi, które umożliwiają ruch pakietów IP.

![Trasy systemowe platformy Azure](./media/virtual-networks-udr-overview/Figure1.png)

Chociaż korzystanie z tras systemowych automatycznie umożliwia ruch sieciowy we wdrożeniu, istnieją przypadki, w których użytkownik chce sterować przekazywaniem pakietów przez urządzenia wirtualne. Można to zrobić, tworząc trasy definiowane przez użytkownika, które wskazują kolejny krok na drodze pakietu do określonej podsieci, aby zamiast tego przeszły do urządzeń wirtualnych, i włączając funkcję przesyłania dalej IP dla maszyny wirtualnej uruchomionej jako urządzenie wirtualne.

Na poniższym rysunku przedstawiono przykład tras zdefiniowanych przez użytkownika i przesyłania dalej IP, wymuszający przechodzenie pakietów wysłanych z jednej podsieci do drugiej przez urządzenie wirtualne w trzeciej podsieci.

![Trasy systemowe platformy Azure](./media/virtual-networks-udr-overview/Figure2.png)

> [!IMPORTANT]
> Trasy zdefiniowane przez użytkownika dotyczą tylko ruchu wychodzącego z podsieci. Na przykład nie można utworzyć trasy, aby określić, w jaki sposób ruch z sieci Internet trafia do podsieci. Ponadto urządzenie, które przekazuje ruch dalej, nie może znajdować się w tej samej podsieci, z której pochodzą dane. Dla urządzeń zawsze należy utworzyć oddzielne podsieci. 
> 
> 

## Zasób trasy
Pakiety są przesyłane za pośrednictwem sieci TCP/IP w oparciu o tabelę tras zdefiniowaną w każdym węźle w sieci fizycznej. Tabela tras jest kolekcją indywidualnych tras, która w oparciu o docelowy adres IP umożliwia podjęcie decyzji, dokąd należy przekazywać pakiety. Trasa składa się z następujących elementów:

| Właściwość | Opis | Ograniczenia | Zagadnienia do rozważenia |
| --- | --- | --- | --- |
| Przedrostek adresu |Docelowy adres CIDR, do którego zostanie zastosowana trasa, na przykład 10.1.0.0/16. |Musi to być prawidłowy zakres adresów CIDR reprezentujący adresy w publicznej sieci Internet, sieci wirtualnej platformy Azure lub lokalnym centrum danych. |Upewnij się, że **przedrostek adresu** nie zawiera **adresu następnego skoku**, ponieważ w przeciwnym razie pakiety wpadną w pętlę, przechodząc od źródła do adresu następnego skoku i nigdy nie docierając do miejsca docelowego. |
| Typ następnego skoku |Typ skoku platformy Azure, dokąd pakiet powinien zostać przesłany. |Musi mieć jedną z następujących wartości: <br/> **Sieć wirtualna** Reprezentuje lokalną sieć wirtualną. Na przykład jeśli dwie podsieci 10.1.0.0/16 i 10.2.0.0/16 znajdują się w tej samej sieci wirtualnej, trasa dla każdej podsieci w tabeli tras będzie miała wartość następnego skoku *Sieć wirtualna*. <br/> **Brama sieci wirtualnej** Reprezentuje usługę Azure S2S VPN Gateway. <br/> **Internet**. Reprezentuje domyślną bramę sieci Internet dostarczoną przez infrastrukturę platformy Azure. <br/> **Urządzenie wirtualne**. Reprezentuje urządzenie wirtualne dodane do Twojej sieci wirtualnej platformy Azure. <br/> **Brak**. Reprezentuje czarną dziurę. Pakiety przekazywane do czarnej dziury nie zostaną w ogóle przekazane. |Należy rozważyć użycie typu **Brak**, aby zatrzymać pakiety w drodze do zadanego miejsca docelowego. |
| Adres następnego skoku |Adres następnego skoku zawiera adres IP, do którego powinien zostać przekazany pakiet. Wartości następnego skoku są dozwolone tylko w przypadku tras, dla których typem następnego skoku jest *Urządzenie wirtualne*. |Muszą być dostępnymi adresami IP. |Jeśli adres IP reprezentuje maszynę wirtualną, upewnij się, że funkcja [Przesyłanie dalej IP](#IP-forwarding) platformy Azure dla maszyny wirtualnej jest włączona. |

W programie Azure PowerShell niektóre wartości „NextHopType” mają inne nazwy:

* Sieć wirtualna to VnetLocal,
* Brama sieci wirtualnej to VirtualNetworkGateway,
* Urządzenie wirtualne to VirtualAppliance,
* Internet to Internet
* Brak to None.

### Trasy systemowe
Każda podsieć utworzona w sieci wirtualnej jest automatycznie skojarzona z tabelą tras, która zawiera następujące reguły dotyczące tras systemowych:

* **Reguła lokalnej sieci wirtualnej**: ta reguła jest tworzona automatycznie dla każdej podsieci w sieci wirtualnej. Określa, że istnieje bezpośrednie połączenie między maszynami wirtualnymi w sieci wirtualnej i nie ma żadnego pośredniego następnego skoku.
* **Reguła lokalna**: ta reguła dotyczy całego ruchu kierowanego do zakresu adresów lokalnych i jako miejsca docelowego następnego skoku używa bramy sieci VPN.
* **Reguła sieci Internet**: ta reguła obsługuje cały ruch kierowany do publicznej sieci Internet i jako miejsca docelowego następnego skoku dla pakietów kierowanych do sieci Internet używa dostarczonej przez infrastrukturę bramy sieci Internet.

### Trasy definiowane przez użytkownika
Dla większości środowisk wystarczające są trasy systemowe zdefiniowane już przez platformę Azure. Może jednak zajść konieczność utworzenia tabeli tras i dodania co najmniej jednej trasy w szczególnych przypadkach, takich jak:

* Wymuszanie tunelowania do sieci Internet za pośrednictwem sieci lokalnej.
* Korzystanie z urządzeń lokalnych w środowisku platformy Azure.

W przypadku powyższych scenariuszy należy utworzyć tabelę tras i dodać do niej trasy zdefiniowane przez użytkownika. Jednocześnie może występować wiele tabel tras, a ta sama tabela może być skojarzona z większą liczbą podsieci. Każda podsieć może być skojarzona tylko z jedną tabelą tras. Wszystkie maszyny wirtualne i usługi chmury w podsieci używają tabeli tras związanej z tą podsiecią.

Jeśli z podsiecią nie jest skojarzona tabela tras, korzysta ona z tras systemowych. Jeśli skojarzenie istnieje, routing odbywa się w oparciu o najdłuższe dopasowanie prefiksu (Longest Prefix Match, LPM) wybierane spośród tras definiowanych przez użytkownika i tras systemowych. Jeśli istnieje więcej niż jedna trasa z tym samym dopasowaniem LPM, wybór trasy odbywa się według następującej kolejności:

1. Trasa zdefiniowana przez użytkownika
2. Trasa protokołu BGP (jeśli używane są połączenia ExpressRoute)
3. Trasa systemowa

Informacje na temat tworzenia tras definiowanych przez użytkownika można znaleźć w artykule [Sposób tworzenia tras i włączanie funkcji przesyłania dalej IP na platformie Azure](virtual-network-create-udr-arm-template.md).

> [!IMPORTANT]
> Trasy zdefiniowane przez użytkownika są stosowane tylko w odniesieniu do maszyn wirtualnych i usług w chmurze. Na przykład jeśli pomiędzy siecią lokalną a platformą Azure ma zostać wstawione urządzenie wirtualne zapory, konieczne jest utworzenie trasy zdefiniowanej przez użytkownika dla tabel tras platformy Azure, która spowoduje przekazanie całego ruchu kierowanego do lokalnej przestrzeni adresowej do urządzenia wirtualnego. Można również dodać trasę zdefiniowaną przez użytkownika do podsieci GatewaySubnet, aby przekazywać cały ruch z sieci lokalnej do platformy Azure za pośrednictwem urządzenia wirtualnego. Ta możliwość została ostatnio dodana.
> 
> 

### Trasy protokołu BGP
Jeśli między siecią lokalną a platformą Azure istniej połączenie ExpressRoute, można włączyć protokół BGP w celu propagowania tras z sieci lokalnej do platformy Azure. Te trasy protokołu BGP są używane w taki sam sposób, jak trasy systemowe i trasy definiowane przez użytkownika w każdej podsieci platformy Azure. Więcej informacji można znaleźć w artykule [ExpressRoute — wprowadzenie](../expressroute/expressroute-introduction.md).

> [!IMPORTANT]
> W środowisku Azure można skonfigurować wymuszanie tunelowania przez sieć lokalną, tworząc trasę definiowaną przez użytkownika dla podsieci 0.0.0.0/0, korzystającą z bramy sieci VPN w następnym skoku. Metoda ta działa jednak tylko w przypadku korzystania z bramy sieci VPN, a nie połączeń ExpressRoute. W przypadku połączeń ExpressRoute wymuszanie tunelowania jest konfigurowane za pomocą protokołu BGP.
> 
> 

## Przesyłanie dalej IP
Jak opisano powyżej, jednym z głównych powodów tworzenia tras definiowanych przez użytkownika jest przesyłanie ruchu sieciowego do urządzenia wirtualnego. Urządzenie wirtualne to po prostu maszyna wirtualna, na której działa aplikacja służąca do obsługi ruchu sieciowego w określony sposób, na przykład zapora lub urządzenie NAT.

Ta maszyna wirtualna musi mieć zdolność odbierania ruchu przychodzącego, który nie jest skierowany do niej samej. Aby umożliwić maszynie wirtualnej odbieranie ruchu kierowanego do innych miejsc docelowych, konieczne jest włączenie dla tej maszyny wirtualnej funkcji przesyłania dalej IP. Jest to ustawienie platformy Azure, a nie systemu operacyjnego gościa.

## Następne kroki
* Dowiedz się, w jaki sposób można [tworzyć trasy w modelu wdrożenia usługi Resource Manager](virtual-network-create-udr-arm-template.md) i kojarzyć je z podsieciami. 
* Dowiedz się, w jaki sposób można [tworzyć trasy w klasycznym modelu wdrożenia](virtual-network-create-udr-classic-ps.md) i kojarzyć je z podsieciami.

<!--HONumber=sep16_HO1-->


