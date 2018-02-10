---
title: "Opis połączeń wychodzących na platformie Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak Azure umożliwia maszyny wirtualne do komunikowania się z publicznej usługi internetowe."
services: load-balancer
documentationcenter: na
author: KumudD
manager: jeconnoc
editor: 
ms.assetid: 5f666f2a-3a63-405a-abcd-b2e34d40e001
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/05/2018
ms.author: kumud
ms.openlocfilehash: e1a2b4c281194ec4c70e4d9fe1bc97c5aa61436e
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="understanding-outbound-connections-in-azure"></a>Informacje o połączeniach wychodzących na platformie Azure

[!INCLUDE [load-balancer-basic-sku-include.md](../../includes/load-balancer-basic-sku-include.md)]


Platforma Azure udostępnia łączność wychodząca wdrożeń klienta za pomocą kilku różnych mechanizmów.  W tym artykule opisano scenariusze są, gdy mają one zastosowanie, jak działają i jak nimi zarządzać.

Wdrażanie na platformie Azure mogą komunikować się z punktami końcowymi poza platformą Azure w publicznej przestrzeni adresów IP. Gdy wystąpienie inicjuje przepływu wychodzącego do miejsca docelowego w publicznej przestrzeni adresów IP, Azure dynamicznie mapuje prywatnego adresu IP do publicznego adresu IP.  Po utworzeniu tego mapowania zwracany ruchu dla tego ruchu wychodzącego przepływu zdalnych zapewnia także łączność prywatnego adresu IP pochodzenie przepływu.

Azure używa translatora adresów sieciowych źródła (SNAT) do tej funkcji.  W przypadku wielu prywatnych adresów IP są maskaradę za jeden publiczny adres IP, korzysta z usługi Azure [portu translacji adresów (PAWEŁ)](#pat) do zamaskowane pod prywatnych adresów IP.  Porty efemeryczne są używane do PAWEŁ i są [wstępnie przydzielonych](#preallocatedports) na podstawie rozmiaru puli.

Dostępnych jest wiele [scenariusze wychodzących](#scenarios). Te scenariusze można łączyć, zgodnie z potrzebami. Przeglądać je uważnie, aby zrozumieć możliwości, ograniczenia i wzorce, ponieważ mają one zastosowanie do modelu wdrażania oraz scenariusz aplikacji.  Przejrzyj wskazówki dotyczące [Zarządzanie tych scenariuszy](#snatexhaust).

## <a name="scenarios"></a>Omówienie scenariusza

Platforma Azure ma dwa modele wdrażania najważniejszych: usługi Azure Resource Manager i model klasyczny. Moduł równoważenia obciążenia i powiązane zasoby są jawnie zdefiniowane przy użyciu [zasobów usługi Azure Resource Manager](#arm).  W przypadku wdrożeń klasycznych abstrakcyjnej pojęcie usługi równoważenia obciążenia i express podobną funkcję za pomocą definicji punktów końcowych [usługi w chmurze](#classic). Zastosowanie [scenariusze](#scenarios) wdrożenia zależnych na których wdrożenie jest używany model.

### <a name="arm"></a>Azure Resource Manager

Platforma Azure udostępnia obecnie trzech różnych metod na osiągnięcie łączność wychodząca dla zasobów usługi Azure Resource Manager.  [Klasycznym](#classic) wdrożenia mają podzbiór tych scenariuszy.

| Scenariusz | Metoda | Opis |
| --- | --- | --- |
| [1. Maszyna wirtualna o wystąpieniu poziomu publicznego adresu IP (z lub bez modułu równoważenia obciążenia)](#ilpip) | SNAT, port maskaradę nieużywane. |Azure używa publicznego adresu IP, przypisane do konfiguracji IP karty sieciowej dla wystąpienia.  Wystąpienie ma wszystkie dostępne porty efemeryczne. |
| [2. Publiczny moduł równoważenia obciążenia skojarzone z maszyną Wirtualną (nie wystąpienia poziomu publicznego adresu IP w wystąpieniu)](#lb) | SNAT z portu maskaradę (PAT) przy użyciu frontend(s) modułu równoważenia obciążenia |Azure udostępnia publiczne usługi równoważenia obciążenia frontend(s) z wielu prywatnego adresu IP, adresy i używa porty efemeryczne frontend(s) do PAWEŁ publiczny adres IP. |
| [3. Maszyna wirtualna autonomiczne (Brak usługi równoważenia obciążenia, żaden adres publiczny adres IP na poziomie wystąpienia)](#defaultsnat) | SNAT z portu maskaradę (PAT) | Azure automatycznie wyznacza publicznego adresu IP dla SNAT, współużytkuje ten publiczny adres IP z wielu prywatnych adresów IP do zestawu dostępności i używane porty efemeryczne ten publiczny adres IP.  W tym scenariuszu scenariusza powrotu w scenariuszach poprzedzających 1 i 2 i nie zaleca się, jeśli potrzebujesz wgląd i większą kontrolę. |

Jeśli nie chcesz, aby maszyny Wirtualnej do komunikowania się z punktami końcowymi poza platformą Azure w publicznej przestrzeni adresów IP, można użyć grup zabezpieczeń sieci (NSG) w celu zablokowania dostępu zgodnie z potrzebami.  Za pomocą grup NSG omówiono bardziej szczegółowo w [uniemożliwia nawiązanie połączenia wychodzącego](#preventoutbound).  Projekt, wskazówki i implementacji szczegóły dotyczące projektowania i zarządzania nimi sieci wirtualnej, bez żadnych wychodzący dostęp jest poza zakres tego artykułu.

### <a name="classic"></a>Klasyczny (usługi w chmurze)

Dostępne w przypadku wdrożeń Classic scenariusze są podzbiorem dostępne dla scenariuszy [usługi Azure Resource Manager](#arm) wdrożeń i podstawowa usługi równoważenia obciążenia.

Klasycznym maszyna wirtualna ma tego samego trzy podstawowe scenariusze, zgodnie z opisem dla zasobów usługi Azure Resource Manager ([1](#ilpip), [2](#lb), [3](#defaultsnat)). Klasycznym roli procesu roboczego sieci Web ma tylko dwa scenariusze ([2](#lb), [3](#defaultsnat)).  [Środki zaradcze strategii](#snatexhaust) również mieć tego samego różnice.

Algorytm używany do [przydzielone porty efemeryczne](#ephemeralprots) PAWEŁ wdrożeń klasycznego jest taka sama jak w przypadku wdrożenia zasobów usługi Azure Resource Manager.  

### <a name="ilpip"></a>Scenariusz 1: Maszyna wirtualna adres publiczny adres IP na poziomie wystąpienia

W tym scenariuszu maszyna wirtualna ma wystąpienie poziomu publicznego adresu IP (ILPIP) przypisane do niej. Jeśli chodzi o połączeń wychodzących, nie ma znaczenia, czy maszyna wirtualna jest równoważone, czy nie.  Ten scenariusz ma pierwszeństwo przed jej innym osobom. W przypadku ILPIP maszyna wirtualna używa ILPIP dla wszystkich przepływów wychodzących.  

Port zamaskowana (PAWEŁ) nie jest używana, a maszyna wirtualna ma wszystkie porty efemeryczne dostępne do użycia.

Jeśli aplikacja inicjuje wiele przepływów wychodzących i występują wyczerpania portu SNAT, można rozważyć przypisywanie [ILPIP złagodzić ograniczenia SNAT](#assignilpip). Przegląd [wyczerpania Zarządzanie SNAT](#snatexhaust) go całkowicie.

### <a name="lb"></a>Scenariusz 2: VM równoważeniem obciążenia bez wystąpienia poziomu publicznego adresu IP

W tym scenariuszu maszyna wirtualna jest częścią publicznego puli zaplecza modułu równoważenia obciążenia.  Maszyna wirtualna nie ma publicznego adresu IP przypisane do niej. Zasób usługi równoważenia obciążenia musi być skonfigurowany z reguły modułu równoważenia obciążenia, aby utworzyć łącze między publicznego frontonu IP z puli zaplecza. Jeśli nie wykonasz tę konfigurację reguły, zachowanie jest opisany w scenariuszu dla [autonomiczny maszynę Wirtualną za pomocą wystąpienia poziomu publiczny adres IP nie](#defaultsnat).  Nie jest konieczne, aby reguła mogła mieć odbiornik roboczych w puli zaplecza lub sondy kondycji powiodło się.

Gdy maszyna wirtualna z równoważeniem obciążenia tworzy przepływu wychodzącego, Azure tłumaczy prywatnej źródłowy adres IP przepływu wychodzącego do publicznego adresu IP publiczny moduł równoważenia obciążenia serwera sieci Web. Azure źródła sieci adresu tłumaczenia (SNAT) korzysta z tej funkcji oraz [portu translacji adresów (PAWEŁ)](#pat) do zamaskowane pod wiele prywatnych adresów IP za pomocą publicznego adresu IP. Porty efemeryczne publicznego frontonu adresu IP usługi równoważenia obciążenia są używane do rozróżniania poszczególnych przepływów pochodzi przez maszynę Wirtualną. Dynamicznie używa SNAT [wstępnie przydzielonych portów tymczasowych](#preallocatedports) utworzenia przepływów wychodzących. W tym kontekście tymczasowych portów używanych do SNAT są określane jako porty SNAT.

Są wstępnie przydzielonych portów SNAT, zgodnie z opisem w [SNAT zrozumienia i PAWEŁ](#snat) sekcji i są ograniczone zasób, który może być wyczerpany. Ważne jest, aby zrozumieć, jak są one [używane](#pat). Przegląd [wyczerpania Zarządzanie SNAT](#snatexhaust) zrozumienie, jak projektować pod kątem i łagodzenia odpowiednio do potrzeb.

Gdy [wielu adresów IP (publicznymi) są skojarzone z podstawowe usługi równoważenia obciążenia](load-balancer-multivip-overview.md), wszelkie publiczny adres IP na te adresy są [wybrano kandydatem do przepływów wychodzących i jeden](#multivipsnat).  

Można użyć [analizy dzienników dla usługi równoważenia obciążenia](load-balancer-monitor-log.md) i [alertu dzienniki zdarzeń, aby monitorować SNAT portu wiadomości wyczerpania](load-balancer-monitor-log.md#alert-event-log) do monitorowania kondycji połączenia wychodzące z podstawowe usługi równoważenia obciążenia.

### <a name="defaultsnat"></a>Scenariusz 3: Autonomiczny VM bez wystąpienia poziomu publicznego adresu IP

Maszyna wirtualna nie jest częścią puli usługi równoważenia obciążenia Azure i nie ma adresu wystąpienia poziomu publicznego adresu IP (ILPIP) przypisane do niej. Gdy maszyna wirtualna tworzy przepływu wychodzącego, Azure tłumaczy prywatnej źródłowy adres IP przepływu wychodzącego do źródłowego publicznego adresu IP. Publiczny adres IP używany dla tego przepływu ruchu wychodzącego nie można skonfigurować, nie będą uwzględniane względem subskrypcji publicznego adresu IP limit zasobów. Platforma Azure korzysta źródła sieci adresu tłumaczenia (SNAT) z portu maskaradę ([PAT](#pat)) do tej funkcji. Ten scenariusz jest podobny do [Scenariusz 2](#lb), z wyjątkiem jest nie kontroluje adres IP używany.  Jest to rezerwowego scenariusz dla po scenariusze 1 i 2 scenariusza nie istnieją.  W tym scenariuszu nie jest zalecane, jeśli chcesz kontrolować wychodzący adres.

Są wstępnie przydzielonych portów SNAT, zgodnie z opisem w [SNAT zrozumienia i PAWEŁ](#snat) sekcji i są ograniczone zasób, który może być wyczerpany. Ważne jest, aby zrozumieć, jak są one [używane](#pat). Przegląd [wyczerpania Zarządzanie SNAT](#snatexhaust) zrozumienie, jak projektować pod kątem i łagodzenia odpowiednio do potrzeb.

### <a name="combinations"></a>Łączna scenariuszy z wieloma,

Scenariusze opisane w sekcjach poprzedzających można łączyć, aby osiągnąć określony wynik.  Występują wiele scenariuszy, w kolejności zastosowanie: [scenariusz 1](#ilpip) ma pierwszeństwo przed [Scenariusz 2](#lb) i [3](#defaultsnat) (usługi Azure Resource Manager tylko), a [Scenariusz 2](#lb) zastępuje [Scenariusz 3](#defaultsnat) (usługi Azure Resource Manager i model klasyczny).

Przykładem jest wdrożenie usługi Azure Resource Manager, gdzie aplikacja opiera się głównie na połączenia wychodzące do ograniczonej liczby miejsc docelowych, ale także odebrał przepływów przychodzących za pośrednictwem frontonu modułu równoważenia obciążenia. W takim przypadku można łączyć scenariusze 1 i 2 do zwolnienia.  Przegląd [wyczerpania Zarządzanie SNAT](#snatexhaust) dodatkowe wzorców.

### <a name="multivipsnat"></a>Frontends wiele przepływów wychodzących

Podstawowe usługi równoważenia obciążenia wybierze jeden frontonu być przepływów wychodzących używanych podczas [wiele frontends IP (publicznymi)](load-balancer-multivip-overview.md) nadają się do przepływów wychodzących.  Wybranie tej opcji nie można skonfigurować, algorytm wybór należy traktować jako losowych.  Możesz wyznaczyć określonego adresu IP dla ruchu wychodzącego, zgodnie z opisem w [łączyć scenariusze](#combinations).

## <a name="snat"></a>Opis SNAT i Marta

### <a name="pat"></a>Port zamaskowana SNAT (PAWEŁ)

Gdy zasób publiczny moduł równoważenia obciążenia jest powiązana z wystąpień maszyn wirtualnych, każde źródło połączenia wychodzącego jest napisany od nowa. Źródło jest napisany od nowa z przestrzeń adresów IP prywatnej sieci wirtualnej adres publiczny adres IP frontonu modułu równoważenia obciążenia. W publicznej przestrzeni adresów IP 5-elementowej przepływu (źródłowy adres IP, port źródłowy, protokół transportu IP, docelowy adres IP, port docelowy) muszą być unikatowe.  Porty efemeryczne są używane do osiągnięcia tego celu po ponowne zapisywanie prywatnej źródłowy adres IP, ponieważ wiele przepływów pochodzi z jednego publicznego adresu IP.  Te porty efemeryczne są nazywane SNAT portów. 

Jeden port SNAT jest używany na przepływ pojedynczego adresu IP, portu i protokołu. Wiele przepływów do docelowego adresu IP, portu i protokołu zajmują jednego portu SNAT każdego przepływu. Dzięki temu, że strumienie są unikatowe, jeśli pochodzi z tego samego publicznego adresu IP do tego samego adresu IP, portu i protokołu. 

Wiele przepływów, każdy z nich do innego adresu IP, portu i protokołu będą współużytkować jeden port SNAT. Docelowy adres IP, port oraz Protokół zapewnia przepływów unikatowość bez konieczności stosowania dodatkowych źródła porty mają zostać użyte w celu rozróżnienia przepływów w publicznej przestrzeni adresów IP.

Gdy wyczerpania zasobów portu SNAT przepływów wychodzących niepowodzeniem, dopóki SNAT porty są wydawane przez przepływów. Moduł równoważenia obciążenia zwraca SNAT portów, gdy przepływ zamyka i używa [limit czasu bezczynności 4-minutowy](#idletimeout) dla odzyskiwania porty SNAT z bezczynności przepływów.

Przegląd [Zarządzanie SNAT](#snatexhaust) sekcji wzorców ograniczyć warunki, które często doprowadzić do wyczerpania portu SNAT.

### <a name="preallocatedports"></a>Wstępne przydzielanie portów efemerycznych dla portu maskaradę SNAT (PAWEŁ)

Azure używa algorytmu, aby określić liczbę SNAT wstępnie przydzielonych portów dostępnych na podstawie rozmiaru puli wewnętrznej bazy danych przy użyciu portu zamaskowana SNAT ([PAT](#pat)).  Porty SNAT są porty efemeryczne, które są dostępne do publicznego źródłowego adresu IP.

Azure preallocates SNAT portów z konfiguracją protokołu IP karty sieciowej z każdej maszyny Wirtualnej. Po dodaniu konfiguracji IP do puli, porty SNAT są wstępnie przydzielonych dla tej konfiguracji IP na podstawie rozmiaru puli wewnętrznej bazy danych. Klasycznym ról procesu roboczego sieci Web alokacja jest dla każdego wystąpienia roli.  Po utworzeniu przepływów wychodzących [PAT](#pat) dynamicznie zużywa (w granicach przydzielonych wstępnie) i zwalnia te porty, po zamknięciu przepływ lub [limitów czasu bezczynności](#ideltimeout).

W poniższej tabeli przedstawiono preallocations portu SNAT warstw rozmiary puli zaplecza:

| Rozmiar puli (wystąpień maszyny Wirtualnej) | Przydzielony wstępnie SNAT portów w każdej konfiguracji adresu IP|
| --- | --- |
| 1 - 50 | 1024 |
| 51 - 100 | 512 |
| 101 - 200 | 256 |
| 201 - 400 | 128 |
| 401 - 800 | 64 |
| 801 - 1,000 | 32 |

Należy pamiętać, że liczba dostępnych portów SNAT nie przekłada się bezpośrednio do liczby połączeń. Jeden port SNAT mogą być ponownie używane dla wielu celów unikatowy. Porty są używane tylko, jeśli jest to niezbędne do zapewnienia przepływów unikatowy. Zapoznaj się [jak zarządzać zasobem wyczerpującymi](#snatexhaust) wskazówki dotyczące projektowania i środki zaradcze, a także opisujące sekcji [PAT](#pat).

Zmiana rozmiaru puli wewnętrznej bazy danych może mieć wpływ na niektóre przepływów ustalonych. Jeśli rozmiar puli zaplecza zwiększa i przechodzi do następnej warstwy, połowy Twojej przydzielony wstępnie porty SNAT odzyskane podczas przejścia do następnej warstwy większych puli wewnętrznej bazy danych. Przepływów, które są skojarzone z portem SNAT regeneracji będzie limitu czasu i należy ustanowić. Próby połączenia powiedzie się natychmiast tak długo, jak są dostępne porty przydzielony wstępnie.

Jeśli zmniejsza rozmiar puli zaplecza i zwiększyć liczbę dostępnych portów SNAT przejścia do dolnej warstwy. W takim przypadku istniejące przydzielonych SNAT portów i nie ma wpływu na ich odpowiednich przepływów.

## <a name="snatexhaust"></a>Zarządzanie wyczerpania portu SNAT (PAWEŁ)

[Porty efemeryczne](#preallocatedports) używany do [PAT](#pat) są zużywalnymi zasobami, zgodnie z opisem w [autonomicznej maszyny Wirtualnej, bez wystąpienia poziomu publicznego adresu IP](#defaultsnat) i [równoważeniem obciążenia maszyny Wirtualnej bez Wystąpienie poziomu publicznego adresu IP](#lb).

Jeśli znasz są inicjowanie liczbę wychodzących połączeń TCP lub UDP do tego samego adresu IP i portu i obserwować niepowodzeniem połączenia wychodzące lub zalecana przy pomocy technicznej są wyczerpujące porty SNAT (wstępnie przydzielonych [porty efemeryczne](#preallocatedports)używane przez [PAT](#pat)), masz kilka opcji ogólnych środki zaradcze.  Przejrzyj te opcje i zdecyduj, jaki jest dostępny i najlepsze w przypadku danego scenariusza.  Jest możliwe jeden lub więcej może ułatwić zarządzanie tego scenariusza.

Problemów z zrozumienie zachowania połączeń wychodzących można użyć adresu IP statystyk stosu (netstat) lub może być przydatne do przestrzegania zachowania połączeń przy użyciu przechwytywania pakietów.  Możesz wykonać te przechwytywania pakietów w system operacyjny gościa wystąpienia, lub użyj [obserwatora sieciowego dla przechwytywania pakietów](../network-watcher/network-watcher-packet-capture-manage-portal.md).

### <a name="connectionreuse"></a>Modyfikowanie aplikacji do ponownego użycia połączenia 
Można zmniejszyć zapotrzebowanie na porty efemeryczne używane w przypadku SNAT ponowne użycie połączenia w aplikacji.  Jest to szczególnie istotne dla protokołów, takich jak HTTP/1.1, gdzie ponownego użycia połączenia jest ustawieniem domyślnym.  I innych protokołów przy użyciu protokołu HTTP jako transportu (np. REST) mogą korzystać z kolei.  Ponowne użycie zawsze jest lepszym rozwiązaniem niż poszczególnych, atomic połączeń TCP dla każdego żądania i powoduje więcej wydajności, bardzo wydajny transakcji protokołu TCP.

### <a name="connection pooling"></a>Modyfikowanie aplikacji do korzystania z puli połączeń
Można wdrożyć połączenia buforowanie schemat w aplikacji, które żądania są dystrybuowane wewnętrznie przez ustalony zbiór połączeń (każdego ponowne użycie w miarę możliwości).  Tego ograniczenia liczby tymczasowych porty używane i tworzy środowisku bardziej przewidywalne.  To również zwiększyć przepływność żądania, zezwalając wiele równoczesnych operacji, gdy blokuje pojedynczego połączenia w przypadku odpowiedzi operacji.  Pula połączeń może już istnieć w ramach używanej do tworzenia aplikacji lub ustawienia konfiguracji dla aplikacji.  To można używać razem z ponownego użycia połączenia i wiele żądań używanie stały, przewidywalnych liczby portów do tego samego adresu IP i portu podczas również korzystających z bardzo skuteczne stosowanie transakcji TCP zmniejszenia opóźnienia i zasobów wykorzystanie.  Transakcje UDP można również korzystać jako zarządzanie numer protokołu UDP przepływów z kolei można uniknąć warunków spalin i zarządzać nimi SNAT użycie portu.

### <a name="retry logic"></a>Modyfikowanie aplikacji do korzystania z mniej aktywnego Logika ponawiania
Gdy [wstępnie przydzielonych portów tymczasowych](#preallocatedports) używany do [PAT](#pat) są wyczerpane lub aplikacji występują błędy, życie aktywnego lub atak metodą prób bez zanikania i wycofywania logiki spowodować wyczerpanie wystąpienia lub utrwalić. Można zmniejszyć zapotrzebowanie na porty efemeryczne przy użyciu mniej agresywne logiki ponawiania próby.   Porty efemeryczne ma 4-minutowy limit czasu bezczynności (nie można dostosować), a jeśli ponowne próby są zbyt agresywnie, wyczerpanie ma możliwość wyczyszczone samodzielnie.  W związku z tym biorąc pod uwagę, jak i jak często aplikacja ponowi próbę transakcji jest krytyczne projektu.

### <a name="assignilpip"></a>Przypisz publicznego adresu IP wystąpienia poziomu do każdej maszyny Wirtualnej
Spowoduje to zmianę Twojego scenariusza [poziomie wystąpienia publicznego adresu IP do maszyny Wirtualnej](#ilpip).  Wszystkie porty efemeryczne publicznego adresu IP używane dla każdej maszyny Wirtualnej są dostępne dla maszyny Wirtualnej (w przeciwieństwie do scenariuszy, w którym porty efemeryczne publicznego adresu IP są udostępniane wszystkie maszyny Wirtualnej skojarzone z puli zaplecza odpowiednich).  Brak możliwości wziąć pod uwagę, takie jak dodatkowych kosztów publicznych adresów IP i potencjalnego wpływu listę dozwolonych podobnej dużą liczbę pojedyncze adresy IP.

>[!NOTE] 
>Ta opcja nie jest dostępna dla roli proces roboczy sieci Web.

### <a name="idletimeout"></a>Użyj keepalives, aby zresetować wychodzącego limit czasu bezczynności

Połączenia wychodzące mieć 4-minutowy limit czasu bezczynności. To nie jest zmieniane. Jednak można używać transportu (np. TCP utrzymywania aktywności) lub keepalives warstwy aplikacji, aby odświeżyć przepływu bezczynności i zresetować ten limit czasu bezczynności, jeśli jest wymagane.

## <a name="discoveroutbound"></a>Odnajdywanie publiczny adres IP używany przez danego maszyny Wirtualnej
Istnieje wiele sposobów, aby określić źródłowy publiczny adres IP połączeń wychodzących. OpenDNS zapewnia usługę, które mogą być prezentowane z publicznego adresu IP maszyny Wirtualnej. Używanie polecenia nslookup, możesz wysłać zapytanie DNS dla nazwy myip.opendns.com OpenDNS program rozpoznawania nazw. Usługa zwraca źródłowy adres IP użyty do wysłania zapytania. Podczas wykonywania następującego zapytania z maszyny Wirtualnej, odpowiedź jest publiczny adres IP używany dla tej maszyny Wirtualnej.

    nslookup myip.opendns.com resolver1.opendns.com

## <a name="preventoutbound"></a>Uniemożliwia nawiązanie połączenia wychodzącego
Czasami jest niepożądanych dla maszyny Wirtualnej, aby można było utworzyć przepływ wychodzący może być wymagane do zarządzania, które miejsc docelowych jest osiągalna z przepływów wychodzących lub które miejsc docelowych może rozpocząć przepływów przychodzących. W takim przypadku należy użyć [grup zabezpieczeń sieci (NSG)](../virtual-network/virtual-networks-nsg.md) do zarządzania miejsc docelowych, które maszyny Wirtualnej można również uzyskać dostęp publiczny lokalizację docelową zainicjować przepływów przychodzących. Po zastosowaniu grupy NSG z maszyną wirtualną z równoważeniem obciążenia, należy zwrócić uwagę na [domyślne znaczniki](../virtual-network/virtual-networks-nsg.md#default-tags) i [domyślne zasady](../virtual-network/virtual-networks-nsg.md#default-rules).

Należy się upewnić, czy maszyny Wirtualnej może odbierać żądań sondy kondycji z modułu równoważenia obciążenia Azure. Jeśli grupy NSG blokuje żądania sondy kondycji z AZURE_LOADBALANCER domyślny znacznik, sondy kondycji Twojej maszyny Wirtualnej nie powiedzie się i maszyny Wirtualnej jest oznaczony w dół. Moduł równoważenia obciążenia zatrzymuje wysyłanie nowych przepływów z tą maszyną Wirtualną.

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej o [podstawowe usługi równoważenia obciążenia](load-balancer-overview.md).
- Dowiedz się więcej o [grup zabezpieczeń sieci](../virtual-network/virtual-networks-nsg.md).
- Dowiedz się więcej o niektóre inne kluczowe [możliwości w zakresie obsługi](../networking/networking-overview.md) na platformie Azure.
