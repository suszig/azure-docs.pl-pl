---
title: "Omówienie usługi równoważenia obciążenia Azure | Dokumentacja firmy Microsoft"
description: "Omówienie funkcji równoważenia obciążenia Azure, architektura i implementacji. Dowiedz się, jak działa usługa równoważenia obciążenia i wykorzystać go w chmurze."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
ms.assetid: 0f313dc0-f007-4cee-b2b9-f542357925a3
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: kumud
ms.openlocfilehash: ecf1fc38d2b9fd54fe5b00db616224a0848179fe
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-load-balancer-overview"></a>Omówienie usługi Azure Load Balancer

Usługa Azure Load Balancer zapewnia Twoim aplikacjom wysoką dostępność i wydajność sieci. Jest równoważenia obciążenia warstwy 4 (TCP, UDP), który rozdziela przychodzący ruch między dobrej kondycji wystąpień usługi zdefiniowane w zestawie o zrównoważonym obciążeniu.

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]

Moduł równoważenia obciążenia Azure można skonfigurować w celu:

* Równoważenia obciążenia przychodzącego Internet ruchu do maszyn wirtualnych. Ta konfiguracja jest określana jako [równoważenia obciążenia internetowy](load-balancer-internet-overview.md).
* Ruch Równoważenie obciążenia między maszynami wirtualnymi w sieci wirtualnej, między maszynami wirtualnymi w usługi w chmurze lub między komputerami lokalnymi i maszyn wirtualnych w sieci wirtualnej między lokalizacjami. Ta konfiguracja jest określana jako [równoważenia obciążenia wewnętrznego](load-balancer-internal-overview.md).
* Przesyłanie zewnętrznego ruchu sieciowego do określonej maszyny wirtualnej.

Wszystkie zasoby w chmurze muszą publiczny adres IP, który ma być dostępny z Internetu. Infrastruktury chmury na platformie Azure używa bez obsługi routingu adresów IP do jej zasobów. Azure używa translatora adresów sieciowych (NAT) z publicznych adresów IP do komunikacji z Internetem.

## <a name="load-balancer-features"></a>Funkcje równoważenia obciążenia

* Dystrybucji wyznaczania wartości skrótu

    Moduł równoważenia obciążenia Azure używa algorytmu wyznaczania wartości skrótu dystrybucji. Domyślnie skrót 5-elementowej składa się z źródłowy adres IP, port źródłowy, docelowy adres IP, port docelowy i typ protokołu używa do mapowania ruchu na dostępne serwery. Zapewnia tylko lepkości *w* sesji transportu. Pakiety w tej samej sesji TCP lub UDP zostanie przekierowany do tego samego wystąpienia za punktem końcowym równoważeniem obciążenia. Jeśli klient zamyka i ponownie otwiera połączenie lub zaczyna się nowa sesja w tej samej źródłowy adres IP, zmiany portu źródłowego. Może to spowodować ruchu przejść do innego punktu końcowego w różnych centrach danych.

    Aby uzyskać więcej informacji, zobacz [tryb dystrybucji modułu równoważenia obciążenia](load-balancer-distribution-mode.md). Na poniższym rysunku przedstawiono dystrybucji wyznaczania wartości skrótu:

    ![Dystrybucji wyznaczania wartości skrótu](./media/load-balancer-overview/load-balancer-distribution.png)

    Rysunek — skrót na podstawie dystrybucji

* Przekierowanie portów

    Sterowanie za pośrednictwem komunikacji przychodzącej jak odbywa się Azure oferuje usługi równoważenia obciążenia. Ta komunikacja ruch inicjowane z Internetu hosty, maszyny wirtualne w innych usług w chmurze lub sieci wirtualne. Ten formant jest reprezentowany przez punkt końcowy (nazywanych również wejściowy punkt końcowy).

    Wejściowy punkt końcowy nasłuchuje na porcie publicznego i przekazuje ruch do wewnętrznego portu. Można mapować tych samych portów dla wewnętrznego lub zewnętrznego punktu końcowego lub użyć innego portu dla nich. Na przykład można mieć serwera sieci web skonfigurowane do nasłuchiwania na porcie 81 podczas mapowania publiczny punkt końcowy jest port 80. Tworzenie publiczny punkt końcowy wyzwala utworzenie wystąpienia usługi równoważenia obciążenia.

    Podczas tworzenia przy użyciu portalu Azure, portalu automatycznie tworzy punktów końcowych do maszyny wirtualnej dla protokołu RDP (Remote Desktop) i zdalnego ruch w sesji środowiska Windows PowerShell. Te punkty końcowe umożliwia zdalne administrowanie maszyny wirtualnej za pośrednictwem Internetu.

* Automatyczna ponowna konfiguracja

    Moduł równoważenia obciążenia Azure natychmiast ponownie konfiguruje się wystąpień skalowanie w górę lub w dół. Na przykład tego procesu ponownej konfiguracji odbywa się podczas zwiększyć liczby wystąpień ról sieć web/proces roboczy w usłudze w chmurze lub podczas dodawania kolejnych maszyn wirtualnych do tego samego zestawu z równoważeniem obciążenia.

* Monitorowanie usługi

    Moduł równoważenia obciążenia Azure można badanie kondycji różne wystąpienia serwera. Podczas badania nie odpowiada, usługi równoważenia obciążenia zatrzymuje wysyłanie nowych połączeń do wystąpień złej kondycji. Nie dotyczy istniejących połączeń.

    Obsługiwane są trzy typy sond:

    + **Sondowanie agenta gościa (na platformie jako usługa maszyn wirtualnych tylko):** korzysta z usługi równoważenia obciążenia agenta gościa na maszynie wirtualnej. Agent gościa wykrywa i wysyła odpowiedź HTTP 200 OK tylko wtedy, gdy wystąpienie jest w stanie gotowe (tj. wystąpienie nie jest w stanie tak zajęty, odtwarzanie lub zatrzymywanie). Jeśli agent nie odpowiada OK 200 protokołu HTTP, usługi równoważenia obciążenia oznacza wystąpienia jako odpowiadać i zatrzymuje wysyłania ruchu do tego wystąpienia. Moduł równoważenia obciążenia w dalszym ciągu polecenie ping z wystąpieniem. Agent gościa odpowie 200 protokołu HTTP, usługi równoważenia obciążenia będzie wysyłać ruch do tego wystąpienia ponownie. Podczas korzystania z roli sieci web, kodu witryny sieci Web zwykle działa w w3wp.exe, który nie jest monitorowane przez usługę Azure agenta sieci szkieletowej lub gościa. To oznacza, że błędów w w3wp.exe (np. w odpowiedzi HTTP 500) nie będą raportowane do agenta gościa, a moduł równoważenia obciążenia nie będzie wiedzieć, do wykonania danego wystąpienia poza obrotu.
    + **Niestandardowe badanie HTTP:** sonda zastąpienia domyślnej funkcji badania (agenta gościa). Służy on do tworzenia niestandardowej logiki do ustalenia stanu wystąpienia roli. Moduł równoważenia obciążenia będzie regularnie sondowania punktu końcowego (co 15 sekund, domyślnie). Wystąpienie jest uważana rotacji, gdy odpowiada potwierdzenia TCP lub HTTP 200 przed upływem limitu czasu (domyślnie 31 sekund). Jest to przydatne dla implementacji logiki można usunąć wystąpienia z obrotu modułu równoważenia obciążenia. Na przykład można skonfigurować wystąpienia próbę zwrócenia stanu z systemem innym niż 200, jeśli wystąpienie jest ponad 90% zasobów Procesora. Dla role sieci web, które używają w3wp.exe, możesz również uzyskać automatyczne monitorowania witryny sieci Web, ponieważ błędów w kodzie witryny sieci Web zwraca stan – 200 sondy.
    + **Niestandardowe sondowaniem TCP:** sonda opiera się na pomyślne ustanawiania sesji TCP port sondy zdefiniowane.

    Aby uzyskać więcej informacji, zobacz [schematu LoadBalancerProbe](https://msdn.microsoft.com/library/azure/jj151530.aspx).

* Źródło translatora adresów Sieciowych

    Cały ruch wychodzący do Internetu, która pochodzi z usługi podlega źródła translatora adresów Sieciowych (SNAT) przy użyciu tego samego adresu VIP jako ruchu przychodzącego. SNAT oferuje istotne korzyści:

    + Umożliwia on łatwe uaktualnianie i odzyskiwanie po awarii usługi, ponieważ adres VIP, które mogą być dynamicznie mapowane do innego wystąpienia usługi.
    + Ułatwia ona zarządzanie listę kontroli dostępu (ACL) kontrolą dostępu. Listy kontroli dostępu w przeliczeniu na adresy VIP nie zmieniaj jako skalowania usługi w górę, dół, lub pobrać ponownej instalacji.

    Konfiguracji usługi równoważenia obciążenia obsługuje pełne stożkowy translatora adresów Sieciowych dla protokołu UDP. Pełna stożkowy translatora adresów Sieciowych jest typem translatora adresów Sieciowych, gdzie port umożliwia połączenia przychodzące z dowolnym hostem zewnętrznych (w odpowiedzi na żądanie wychodzących).

    Dla każdego nowego wychodzące połączenie, które inicjuje maszynę wirtualną port wychodzący również jest przydzielany przez moduł równoważenia obciążenia. Hoście zewnętrznym widzi ruch przydzielone wirtualne adresy IP IP portu wirtualnego. W scenariuszach wymagających dużej liczby połączeń wychodzących, zaleca się używania [poziomie wystąpienia publicznego adresu IP](../virtual-network/virtual-networks-instance-level-public-ip.md) adresów maszyn wirtualnych ma dedykowany adres IP ruchu wychodzącego dla SNAT. Zmniejsza ryzyko wyczerpania portu.

    Zobacz [połączeń wychodzących](load-balancer-outbound-connections.md) artykułu, aby uzyskać więcej informacji na ten temat.

### <a name="support-for-multiple-load-balanced-ip-addresses-for-virtual-machines"></a>Obsługa wielu adresów IP z równoważeniem obciążenia dla maszyn wirtualnych
Zestaw maszyn wirtualnych można przypisać więcej niż jeden publiczny adres IP z równoważeniem obciążenia. Z tej możliwości może obsługiwać wiele witryn sieci Web protokołu SSL i/lub wielu odbiorniki grupy dostępności AlwaysOn programu SQL Server na tym samym zestawie maszyn wirtualnych. Aby uzyskać więcej informacji, zobacz [wieloma wirtualnymi adresami IP dla danej usługi chmury](load-balancer-multivip.md).

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="limitations"></a>Ograniczenia

Pul zaplecza modułu równoważenia obciążenia może zawierać żadnych SKU maszyny Wirtualnej, z wyjątkiem warstwy podstawowa.

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej o [internetowy modułu równoważenia obciążenia](load-balancer-internet-overview.md)

- Dowiedz się więcej o [Omówienie usługi równoważenia obciążenia wewnętrznego](load-balancer-internal-overview.md)

- Utwórz [internetowy modułu równoważenia obciążenia](load-balancer-get-started-internet-portal.md)

- Dowiedz się więcej o niektóre inne kluczowe [możliwości w zakresie obsługi](../networking/networking-overview.md) platformy Azure

