---
title: "Projektowanie infrastruktury sieci, odzyskiwania po awarii z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule omówiono zagadnienia dotyczące projektowania sieci dla usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: prateek9us
manager: jwhit
editor: 
ms.assetid: ce787731-d930-4f00-a309-e2fbc2e1f53b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 10/19/2017
ms.author: pratshar
ms.openlocfilehash: 5b6fb7bac852b29663866e99758241bd5a7ab59e
ms.sourcegitcommit: 76a3cbac40337ce88f41f9c21a388e21bbd9c13f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="designing-your-network-for-disaster-recovery"></a>Projektowanie sieci w celu odzyskiwania po awarii

W tym artykule omówiono zagadnienia dotyczące sieci, korzystając z [usługi Azure Site Recovery](site-recovery-overview.md) dla awaryjnego odzyskiwania z lokalnego do platformy Azure lub dodatkowej lokalnej lokacji. Koncentruje się on na Definiowanie zakresów adresów IP i podsieci po przejściu w tryb failover dodatkowej lokalizacji.

## <a name="overview"></a>Omówienie

Usługa Site Recovery jest usługą Microsoft Azure, która organizuje ochrony i odzyskiwania zwirtualizowanych aplikacji na potrzeby odzyskiwania po awarii ciągłości biznesowej (BCDR).

W świecie łączności 24/7, należy przechowywać infrastruktury firm i aplikacji i jest uruchomiona. Celem BCDR polega na przywróceniu składników nie powiodło się, dzięki czemu organizacji można szybko przywrócić normalne działanie. Opracowywanie strategii odzyskiwania danych na wypadek prawdopodobne zdarzenia jest bardzo trudne. Jest to spowodowane przewidywania przyszłych, szczególnie w przypadku mało prawdopodobne zdarzenia związanego z używaniem trudności. I z powodu wysokich kosztów obsługi odpowiednie środki ochrony przed dalekosiężną catastrophes.

Kluczowe znaczenie podczas planowania BCDR, cel czas odzyskiwania (RTO) i cel punktu odzyskiwania (RPO) musi być zdefiniowany w planie BCDR. Po awarii centrum danych, lecz można szybko (niska RTO) Przełącz do trybu online ich replikowanych maszyn wirtualnych znajdujących się w centrum danych w dodatkowej lub Microsoft Azure z utraty minimalną ilość danych (RPO niski).

Tryb failover jest możliwe przez usługę Site Recovery, początkowo kopiuje wyznaczonych maszyn wirtualnych z centrum danych pierwotnych do centrum danych w dodatkowej lub na platformie Azure (w zależności od scenariusza), a następnie okresowo odświeża replik. Podczas planowania infrastruktury projektu sieci należy traktować jako potencjalne "wąskie gardło", które mogą uniemożliwić z firmy spotkania RTO i cele odzyskiwania.  

Gdy administratorzy planują wdrożenie rozwiązania odzyskiwania po awarii, jest jedno z pytań kluczowych w swoich umysły, jak maszyny wirtualnej będzie dostępny po zakończeniu pracy awaryjnej. Usługa Site Recovery umożliwia administratorowi wybierz sieć, do której maszyny wirtualne byłyby one połączone do po pracy awaryjnej. Jeśli lokacja główna jest Azure lub jest lokalna lokacja zarządzana przez serwer VMM, następnie jest to osiągane przy użyciu mapowania sieci. Dowiedz się więcej o [mapowanie sieci w Azure do platformy Azure DR](site-recovery-network-mapping-azure-to-azure.md) i [mapowanie sieci z programu VMM](site-recovery-network-mapping.md)


Podczas projektowania sieci dla lokacji odzyskiwania, administrator ma dwie opcje:

* Użyj różnych zakresów adresów IP dla sieci w lokacji odzyskiwania. W tym scenariuszu maszynę wirtualną po zakończeniu pracy awaryjnej pobierze nowy adres IP, a administrator będzie musiał wykonać aktualizacji w systemie DNS. 
* Użyj tego samego zakresu adresów IP dla sieci w lokacji odzyskiwania. W niektórych scenariuszach administratorzy wolą zachowuje adresy IP, które mogą się w lokacji głównej, nawet po pracy awaryjnej. W normalnych scenariuszu administrator musi zaktualizować tras, aby wskazać, do nowej lokalizacji adresów IP. Jednak w scenariuszu wdrożonym rozciągnięty podsieci między serwerem podstawowym i lokacji odzyskiwania, zachowując adresów IP dla maszyn wirtualnych staje się atrakcyjną opcję. Rozciąganie podsieci z siecią lokalną sieć platformy Azure lub między dwiema sieciami Azure nie jest możliwe.  

Gdy administratorzy planują wdrożenie rozwiązania odzyskiwania po awarii, jedno z pytań klucza ich pamiętać jest jak aplikacje będą dostępne po zakończeniu pracy awaryjnej. Nowoczesne aplikacje prawie zawsze są zależne od sieci, w pewnym stopniu, więc fizycznie przeniesienie reprezentuje usługę z jednej lokacji do innej sieci żądanie. Istnieją dwa sposoby główne, które ten problem wynika z rozwiązania w zakresie odzyskiwania po awarii. Pierwszym sposobem jest obsługa stałe adresy IP. Niezależnie od usługi, przenoszenie i serwerami hostingu znajdujące się w różnych lokalizacjach fizycznych aplikacje zająć konfiguracji adresu IP z nimi do nowej lokalizacji. Druga metoda obejmuje całkowicie zmiana adresu IP podczas przejścia w odzyskanej lokacji. Każde podejście jest kilka zmian implementacji, które zestawiono poniżej.

Podczas projektowania sieci dla lokacji odzyskiwania, administrator ma dwie opcje:

## <a name="option-1-retain-ip-addresses"></a>Opcja 1: Zachowaj adresów IP
Z punktu widzenia procesu odzyskiwania po awarii przy użyciu protokołu IP stałe adresy wydaje się być metoda najłatwiejsza do zastosowania, ale ma numer potencjalne problemy, co w praktyce stanowi najmniej popularnych podejście. Usługa Azure Site Recovery zapewnia możliwość przechowywania adresów IP we wszystkich scenariuszach. Przed jedną decyduje o tym zachować IP, odpowiednie należy zwrócić uwagę na ograniczenia, które nakłada się na możliwości trybu failover. Daj nam przyjrzeć się czynników, które mogą ułatwić podjęcie decyzji, aby zachować adresy IP, czy nie. Można to osiągnąć na dwa sposoby, za pomocą rozciągnięty podsieci lub wykonując awarię pełne podsieci.

### <a name="stretched-subnet"></a>Rozciągnięty podsieci
W tym miejscu podsieci jest udostępniana jednocześnie w serwerze podstawowym i lokalizacje odzyskiwania po awarii. Proste oznacza drugiej lokacji można przenieść serwer i konfigurację protokołu IP (warstwy 3) i sieci będzie kierować ruch do nowej lokalizacji automatycznie. Jest to prosta radzenia sobie z z perspektywy serwera, ale ma wiele wyzwań:

* Z punktu widzenia warstwy 2 (warstwę łącza danych) wymaga sprzęt sieciowy, którym może zarządzać rozciągnięty sieci VLAN, ale to stała mniejszy problem teraz jest powszechnie dostępna. Problem drugi i trudniej jest rozciągnięty sieci VLAN potencjalnych domeny błędów jest rozszerzony do obu lokacji zasadniczo staje się pojedynczym punktem awarii. Jest to mało prawdopodobnego zdarzenia, miało to miejsce, że emisji storm uruchomiony, ale nie może być izolowane. Firma Microsoft ma znaleziono mieszanych opinie o tym problemie ostatniego i umieścić wiele implementacji pomyślne również "nie możemy zastosować tę technologię tutaj".
* Rozciągnięty podsieci nie jest możliwe, jeśli używasz Microsoft Azure jako lokacja DR.

### <a name="subnet-failover"></a>Tryb failover podsieci
Istnieje możliwość wykonania podsieci w tryb failover czerpać korzyści z rozwiązania rozciągnięty podsieci bez rozciąganie podsieci w wielu lokacjach. W tym miejscu wszelkie danej podsieci będą obecne w lokacji 1 lub 2 lokacji, ale nigdy nie w obu lokacjach jednocześnie. Aby zachować przestrzeń adresów IP w przypadku trybu failover, jest możliwe programowo przygotowanie infrastruktury router przenieść podsieci z jednej lokacji. W scenariuszu trybu failover, które podsieci były przenoszone z skojarzony chronionych maszyn wirtualnych. Główną wadą tego podejścia to w przypadku awarii, należy przenieść całej podsieci. Może to być OK, ale mogą wpłynąć na poziom szczegółowości trybu failover.

Przeanalizujmy jak fikcyjnej organizacji o nazwie Contoso jest w stanie replikowanie jego maszyn wirtualnych na lokalizację odzyskiwania podczas Niepowodzenie w całej podsieci. Załóżmy pierwszy przyjrzeć się jak firma Contoso jest możliwość zarządzania swoje podsieci podczas replikowania maszyn wirtualnych między dwiema lokalnymi lokalizacji, a następnie omówiono sposób działania podczas pracy awaryjnej podsieci [Azure jest używana jako witryna odzyskiwania po awarii](#failover-to-azure).

#### <a name="fail-over-from-on-premises-to-azure"></a>Przełączyć się z lokalnymi na platformie Azure 
Usługa Azure Site Recovery umożliwia platformie Azure można użyć jako lokacja odzyskiwania po awarii dla maszyn wirtualnych.  

Przeanalizujmy scenariusz, w którym fikcyjnej firmy o nazwie Woodgrove Bank ma hosting aplikacji biznesowych z infrastruktury lokalnej i obsługujące ich aplikacji dla urządzeń przenośnych na platformie Azure. Łączność między maszynami wirtualnymi banku Woodgrove na serwerach Azure i lokalnymi są udostępniane przez lokacja lokacja (S2S) wirtualnej sieci prywatnej (VPN) lub usługi ExpressRoute. Sieci VPN typu lokacja lokacja umożliwia banku Woodgrove sieci wirtualnej na platformie Azure, aby była widoczna jako rozszerzenie sieci lokalnej banku Woodgrove. Ta komunikacja jest włączone przez sieć VPN lokacja lokacja między krawędzi banku Woodgrove i sieć wirtualna platformy Azure. Teraz Woodgrove chce używać usługi Site Recovery w celu replikowania jego obciążeń systemu Azure regionu podstawowego do innego regionu systemu Azure. Ta opcja spełnia potrzeby Woodgrove, chce ekonomiczny opcji odzyskiwania po awarii i umożliwia przechowywanie danych w środowiskach chmury publicznej. Woodgrove ma radzenia sobie z aplikacji i konfiguracji, które zależą od stałe adresy IP, więc wymaganie, aby zachować adresów IP dla swoich aplikacji po awarii do innego regionu na platformie Azure.

Woodgrove postanawia przypisywania adresów IP z zakresu adresów IP (172.16.1.0/24, 172.16.2.0/24) do jej zasobów, które działają na platformie Azure.

Woodgrove można było replikowanie jego maszyn wirtualnych na platformie Azure przy zachowaniu adresów IP sieci wirtualnej platformy Azure musi zostać utworzona. Tak, aby aplikacje można przełączyć się z lokacją lokalną Azure bezproblemowo powinno być rozszerzenie sieci lokalnej. Azure umożliwia dodanie połączenie sieci VPN typu lokacja lokacja, a także punkt lokacja, do sieci wirtualnych utworzonych na platformie Azure. Podczas konfigurowania połączenia lokacja lokacja, sieć platformy Azure pozwala kierować ruchem do lokalizacji lokalnego (Azure wywołuje ona sieci lokalnej) tylko wtedy, gdy zakres adresów IP różni się od lokalnymi zakres adresów IP, ponieważ nie obsługuje platformy Azure Rozciąganie podsieci.  Oznacza to, że jeśli 192.168.1.0/24 podsieci lokalnej, nie można dodać 192.168.1.0/24 sieci lokalnej w sieci platformy Azure. Oczekiwany jest Azure nie może ustalić, że nie ma żadnych aktywnych maszyn wirtualnych w podsieci i że podsieć jest tworzony tylko do celów odzyskiwania po awarii. Aby można było poprawnie kierować ruchem sieciowym spoza sieci platformy Azure podsieci w sieci i sieci lokalnej nie może powodować konfliktu.

![Przed podsieci trybu Failover](./media/site-recovery-network-design/network-design7.png)

Przed trybu failover

Aby ułatwić Woodgrove spełnić ich wymagań biznesowych, musimy zaimplementować następujące przepływy pracy:

* Tworzenie dodatkowych sieci, Daj nam go wywoływać odzyskiwania sieci, gdzie zostałyby utworzone przełączona w tryb failover maszyny wirtualnej.
* Aby upewnić się, że adres IP dla maszyny Wirtualnej jest zachowywany po przejściu w tryb failover, przejdź na kartę Konfiguracja we właściwościach maszyny Wirtualnej, określ tego samego adresu IP, że maszyna wirtualna ma lokalnego, a następnie kliknij przycisk Zapisz. Gdy maszyna wirtualna przeszła w tryb failover, usługi Azure Site Recovery przypisze podany adres IP do maszyny wirtualnej.

![Właściwości sieci](./media/site-recovery-network-design/network-design8.png)

Po wyzwoleniu pracy awaryjnej i maszyny wirtualne są tworzone w sieci odzyskiwania z żądany adres IP, można nawiązać połączenie z tą siecią przy użyciu [sieci wirtualnej do sieci wirtualnej połączenia](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md). Jeśli jest to wymagane, ta akcja umożliwia pisanie skryptów.  Jak wspomniano w poprzedniej sekcji o pracy awaryjnej podsieci, nawet w przypadku trybu failover na platformie Azure, trasy musi być odpowiednio zmodyfikowany w celu odzwierciedlenia tej 192.168.1.0/24 ma teraz przeniesione do platformy Azure.

![Po podsieci w tryb Failover](./media/site-recovery-network-design/network-design9.png)

Po pracy awaryjnej

Jeśli nie ma sieci Azure, jak pokazano na powyższym rysunku. Po przejściu w tryb failover, można utworzyć połączenie VPN lokacja lokacja między "Lokacji głównej" i "Sieć odzyskiwania".  


#### <a name="fail-over-to-a-secondary-on-premises-site"></a>Tryb failover do dodatkowej lokacji lokalnymi
Daj nam przyjrzeć się scenariusza gdy chcemy, aby zachować IP poszczególnych maszyn wirtualnych i awaryjnej pełną podsieci razem. Lokacja główna ma aplikacji uruchomionych w podsieci 192.168.1.0/24. W przypadku przejście w tryb failover wszystkich maszyn wirtualnych, które są częścią tej podsieci będzie można przełączyć do odzyskiwania lokacji i zachować swoje adresy IP. Trasy będą musiały być odpowiednio zmodyfikowany w celu odzwierciedlenia faktu, że wszystkich maszyn wirtualnych należących do podsieci 192.168.1.0/24 zostały przeniesione do lokacji odzyskiwania.

Na poniższej ilustracji tras między lokacji głównej i lokacji odzyskiwania, trzeci lokacji i lokacji głównej i trzeci lokacji i lokacji odzyskiwania należy odpowiednio można modyfikować.

Następujące obrazy Pokaż podsieci przed pracy awaryjnej. 192.168.0.1/24 podsieci jest aktywny w lokacji głównej przed pracy awaryjnej i staje się aktywny, lokacja odzyskiwania po przejściu w tryb failover

![Przed trybu Failover](./media/site-recovery-network-design/network-design2.png)

Przed trybu failover

Na rysunku poniżej przedstawiono sieci i podsieci po pracy awaryjnej.

![Po pracy awaryjnej](./media/site-recovery-network-design/network-design3.png)

Po pracy awaryjnej

Jeśli dodatkowej lokacji lokalnej i korzystania z serwera programu VMM do zarządzania nim, następnie podczas włączania ochrony określonej maszyny wirtualnej, Usługa Site Recovery spowoduje przydzielenie zasobów sieciowych zgodnie z poniższym przepływie pracy:

* Usługa Site Recovery przydziela adres IP dla każdego interfejsu sieciowego na maszynie wirtualnej z zdefiniowane w odpowiednich sieci dla każdego wystąpienia programu System Center VMM puli statycznych adresów IP.
* Jeśli administrator definiuje tej samej puli adresów IP dla sieci w lokacji odzyskiwania, co dla puli adresów IP w sieci w lokacji głównej podczas przydzielania adres IP do maszyny wirtualnej repliki, Usługa Site Recovery może przydzielić ten sam adres IP z podstawowej maszyny wirtualnej.  Adres IP jest zastrzeżony w programie VMM, ale nie ustawiono jako adresu IP trybu failover na hoście funkcji Hyper-v. Adres IP trybu failover na hoście funkcji Hyper-v ustawiono tuż przed pracy awaryjnej.


Jeśli ten sam adres IP nie jest dostępna, Usługa Site Recovery przydziela niektóre inne dostępny adres IP z puli adresów IP zdefiniowanych.

Po włączeniu maszyny Wirtualnej do ochrony można następujący przykładowy skrypt Sprawdź adres IP, która została przydzielona do maszyny wirtualnej. Tego samego adresu IP może ustawić jako adresu IP trybu Failover i przypisane do maszyny Wirtualnej w trybie failover:

        $vm = Get-SCVirtualMachine -Name <VM_NAME>
        $na = $vm[0].VirtualNetworkAdapters>
        $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
        $ip.address  

> [!NOTE]
> W tym scenariuszu, gdy maszyny wirtualne korzystania z protokołu DHCP zarządzania adresami IP jest całkowicie poza kontrolą usługi Site Recovery. Administrator musi zapewnić, że serwer DHCP obsługuje adresy IP w lokacji odzyskiwania może stanowić z tego samego zakresu z lokacji głównej.
>
>



## <a name="option-2-changing-ip-addresses"></a>Opcja 2: Zmiana adresów IP
Takie podejście jest prawdopodobnie najbardziej rozpowszechnionych oparte na co zaobserwowano. Ma on postać zmiana adresu IP każdej maszyny wirtualnej, który jest używany w tryb failover. Wadą tego podejścia wymaga przychodzące sieć "learn" aplikacji, który był w IPx jest teraz na IPy. Nawet jeśli IPx i IPy nazwy logiczne, wpisy DNS zwykle mają zostać zmienione lub opróżnionych w całej sieci i wpisów pamięci podręcznej w tabelach sieci muszą zostać zaktualizowane lub opróżnione, przestoju można uznać jak zostało do infrastruktury DNS w zależności od Konfigurowanie. Te problemy można zminimalizować przy użyciu niskich wartości TTL w przypadku aplikacji intranetowych i [usługi Azure Traffic Manager z usługą Site Recovery](http://azure.microsoft.com/blog/2015/03/03/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/), dla aplikacji internetowych

### <a name="changing-the-ip-addresses---illustration"></a>Zmiana adresów IP - ilustracji
Daj nam przyjrzeć się scenariusza której planujesz używać różnych adresów IP między serwerem podstawowym i lokacji odzyskiwania. W poniższym przykładzie firma Microsoft również mieć innej lokacji z którym aplikacje hostowane w podstawowej lub odzyskiwania lokacji są dostępne.

![Różne IP — przed trybu Failover](./media/site-recovery-network-design/network-design10.png)


W powyższym rysunku są niektóre aplikacje hostowane w podsieci 192.168.1.0/24 podsieci w lokacji głównej i skonfigurowano została znaleziona w lokacji odzyskiwania w 172.16.1.0/24 podsieci po przejściu w tryb failover. Trasy połączeń i sieci VPN zostały skonfigurowane odpowiednio tak, aby wszystkie trzy witryny mają dostęp do siebie.

Jako obraz poniżej przedstawia po awarii jednego lub więcej aplikacji zostaną przywrócone w podsieci odzyskiwania. W takim przypadku możemy są nie ograniczone do pracy awaryjnej w całej podsieci, w tym samym czasie. Aby zmienić konfigurację trasy sieci VPN lub sieć nie są konieczne nie zmiany. Tryb failover i niektóre aktualizacje DNS będzie upewnij się, czy aplikacje będą nadal dostępne. Jeśli system DNS jest skonfigurowany, aby zezwalać na aktualizacje dynamiczne maszyn wirtualnych będzie rejestrują się za pomocą nowego adresu IP, po rozpoczęciu po przejściu w tryb failover.

![IP różne — od trybu Failover](./media/site-recovery-network-design/network-design11.png)


Po niepowodzeniu w tryb failover maszyny wirtualnej repliki może być adres IP, który nie jest taki sam jak adres IP podstawowej maszyny wirtualnej. Maszyny wirtualne spowoduje zaktualizowanie serwera DNS, które używają po rozpoczęciu. Wpisy DNS zwykle mają zostać zmienione lub opróżnionych w całej sieci, a wpisów pamięci podręcznej w tabelach sieci muszą zostać zaktualizowane lub opróżnione, tak nie jest rzadko stoi przestojów podczas zmiany stanu została wykonana. Ten problem można zminimalizować przez:

* Przy użyciu niskich wartości TTL dla aplikacji sieci intranet.
* Za pomocą Menedżera ruchu Azure z usługą Site Recovery dla aplikacji internetowych.
* Przy użyciu następującego skryptu w ramach planu odzyskiwania, aby zaktualizować serwer DNS w celu zapewnienia aktualnych aktualizacji (skrypt nie jest wymagane skonfigurowanie rejestracji dynamicznych DNS)

        param(
        string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord

### <a name="changing-the-ip-addresses--disaster-recovery-to-azure"></a>Zmiana adresu IP adresów — odzyskiwanie po awarii na platformie Azure
[Sieci infrastruktury Instalatora platformy Microsoft Azure jako lokacja odzyskiwania po awarii](http://azure.microsoft.com/blog/2014/09/04/networking-infrastructure-setup-for-microsoft-azure-as-a-disaster-recovery-site/) wpis w blogu wyjaśniono, jak skonfigurować wymaganej infrastruktury sieci Azure, przy zachowaniu adresów IP nie jest wymagana. Rozpoczyna się od opisu aplikacji, a następnie zobacz, jak skonfigurować lokalne sieci na platformie Azure i następnie kończąc jak przeprowadzić test trybu failover i planowanego trybu failover.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o [mapowania sieci](site-recovery-network-mapping.md).
