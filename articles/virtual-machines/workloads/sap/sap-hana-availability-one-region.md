---
title: "SAP HANA dostępności w ramach jednego regionu Azure | Dokumentacja firmy Microsoft"
description: Operacje SAP HANA na maszynach wirtualnych Azure natywnego
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: 
author: msjuergent
manager: patfilot
editor: 
tags: azure-resource-manager
keywords: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/5/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 28fe9bc7c8cb1d59df404b7dd7429def54648a18
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/08/2018
---
# <a name="sap-hana-availability-within-one-azure-region"></a>SAP HANA dostępności w ramach jednego regionu Azure
W tej sekcji przedstawiono kilka scenariuszy, w których opisano scenariusze dostępności w ramach jednego regionu Azure. Platforma Azure ma wiele regionów, które są rozkładane całego świata. Z listy regiony platformy Azure, można znaleźć [regiony platformy Azure](https://azure.microsoft.com/regions/) artykułu. Wdrażanie SAP HANA na maszynach wirtualnych w ramach jednego regionu Azure, firma Microsoft oferuje wdrożenie jednej maszyny Wirtualnej przy użyciu wystąpienia HANA. Lub potrzeby zwiększonej dostępności można wdrożyć dwóch maszyn wirtualnych z dwoma wystąpieniami HANA w [zestawu dostępności Azure](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) są za pomocą replikacji systemu HANA do celów dostępności. Platforma Azure ma publicznej wersji zapoznawczej programu [stref dostępności Azure](https://docs.microsoft.com/azure/availability-zones/az-overview). Te strefy dostępności nie będą jeszcze omówiona szczegółowo. Z wyjątkiem niektórych ogólne opinią wokół użycie zestawów dostępności w zależności od dostępności strefy.

Jaka jest różnica między zestawami dostępności Azure i stref dostępności? Regiony platformy Azure, gdzie dostępność strefy będą mieć możliwość regiony mieć wiele centrów danych, niezależne w dostarczaniu źródła zasilania, chłodzenia i sieci. Przyczyna oferty różnych stref w pojedynczym regionie Azure jest umożliwiają wdrażanie aplikacji w tow lub trzy strefy dostępności oferowanych. Przy założeniu, że problemy w źródłami zasilania i/lub sieci mających wpływ na jednego infrastruktury dostępność strefy, wdrażania aplikacji w obrębie regionu Azure nadal jest w pełni funkcjonalna. Po pewnym czasie niektórych zmniejszenie pojemności, ponieważ niektóre maszyny wirtualne w jednej strefie mogą zostać utracone. Ale maszyn wirtualnych w dwie strefy są nadal uruchomione i działają prawidłowo. 
 
Azure zestawu dostępności jest możliwość grupę logiczną, która na platformie Azure umożliwia upewnij się, że zasoby maszyny Wirtualnej, które można umieścić w są odizolowane od siebie, gdy są wdrożone w centrum danych Azure awarii. Maszyny wirtualne platformy Azure umieszczone w zestawie dostępności korzystają z wielu serwerów fizycznych, regałów obliczeniowych, jednostek magazynowych i przełączników sieciowych. Lub jak niektóre inne dokumentacji platformy Azure, jest ona określana jako rozmieszczanie w różnych [aktualizacji i domen błędów](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability). Rozmieszczanie te są zwykle w obrębie centrum danych Azure. Przy założeniu, że problemy w źródłami zasilania i/lub sieci mających wpływ na centrum danych, które są wdrożone, czy pojemność w jednym regionie Azure wpłynęłoby.

Umieszczanie centrów danych, reprezentujące stref dostępności Azure stanowi kompromis między dostarczania opóźnienie sieci między usługami wdrożone w różnych strefach, które są obsługiwane w większości aplikacji i niektóre odległości między centrach danych. Tak, aby catastrophes fizycznych w idealnym przypadku będzie nie wpływu zasilania i podaj sieci i infrastruktury dla wszystkich stref dostępności w tym regionie. Jednak jako pomnikowe catastrophes fizyczną wykazało stref dostępności może nie zawsze być możliwość zapewnienia dostępności w ramach jednego regionu zgodnie z potrzebami. Zastanów się huragan Maria trafień Wyspy Portoryko na 2017-08/20 i zasadniczo spowodował bliskie 100% czarne, poza na Wyspy szeroki mil 90.   
  


## <a name="single-vm-scenario"></a>Jeden scenariusz maszyny Wirtualnej
W tym scenariuszu utworzono maszynę wirtualną platformy Azure dla wystąpienia programu SAP HANA. Usługa Azure Premium Storage są używane do obsługi dysku systemu operacyjnego i wszystkich dysków danych. Umowa SLA czasu z wartością 99,9% przez platformę Azure i SLA inne składniki platformy Azure jest wystarczające do pełnienia jego dostępność SLA wobec klientów. W tym scenariuszu użytkownik nie ma potrzeby wykorzystać Azure zestawu dostępności dla maszyn wirtualnych warstwy systemu DBMS. W tym scenariuszu użytkownik korzysta z dwóch różnych funkcji:

- Azure automatycznie wirtualna ponownego uruchomienia (również określany jako naprawą usługi Azure)
- SAP HANA automatycznego ponownego uruchamiania

Automatyczne i ponowne uruchamianie usługi Azure maszyny Wirtualnej lub "naprawą usługi" jest funkcji na platformie Azure, która działa na dwa poziomy:

- Sprawdzanie kondycji maszyn wirtualnych hostowanych na hoście serwera hosta serwera Azure
- Monitorowanie kondycji i dostępności hosta serwera Azure kontrolera sieci szkieletowej

Dla każdej maszyny Wirtualnej hostowanej na hoście serwera Azure funkcji sprawdzania kondycji monitoruje kondycję hostowanej wirtualne. W przypadku maszyn wirtualnych można podzielić na — dobrej kondycji, można zainicjować ponownego uruchomienia maszyny wirtualnej przez agenta hosta platformy Azure, który umożliwia sprawdzenie kondycji maszyny wirtualnej. Kontroler sieci szkieletowej Azure jest sprawdzanie kondycji hosta, sprawdzając wiele różnych parametrów sygnalizujące problemy z sprzętu hosta, ale również kontrole dostępności hosta za pośrednictwem sieci. Wskazanie problemy z hostem może prowadzić do akcji, takich jak:

- Ponownie hosta, a następnie ponowne uruchomienie maszyn wirtualnych, które były uruchomione na hoście, jeśli host sygnały zła Kondycja
- Ponowne uruchomienie komputera hosta i ponownie uruchomić wirtualne, które były wcześniej obsługiwane na hoście, na hoście działa prawidłowo w przypadku, gdy host nie jest w dobrej kondycji po ponownym rozruchu. W tym przypadku jest host będzie można oznaczone nie działa prawidłowo i nie jest używany dla dalszego wdrożeń do chwili wyczyszczone lub wymiany.
- Natychmiastowego ponownego uruchomienia maszyny wirtualne na hoście działa prawidłowo w przypadku, gdy zła host ma problemy w procesie ponownego uruchomienia. 

Z hosta i maszyny Wirtualnej monitorowanie oferowane przez Azure maszynach wirtualnych platformy Azure, które ponoszą na problemy dotyczące hosta są automatycznie uruchamiane ponownie w dobrej kondycji host platformy Azure 

Funkcja drugi zależne w takiej sytuacji jest fakt, że automatycznego uruchamiania usługi HANA, który jest uruchamiany w ramach wirtualna uruchomić ją ponownie po ponownym uruchomieniu maszyny wirtualnej. [HANA usługi automatycznego ponownego uruchamiania](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/cf10efba8bea4e81b1dc1907ecc652d3.html) można skonfigurować za pomocą usług programu alarmowego różnych usług HANA.

W tym scenariuszu jednej maszyny Wirtualnej można uzyskać zwiększona przez dodanie węzła zimnych trybu failover do konfiguracji programu SAP HANA. Lub jest ona wywoływana w dokumentacji programu SAP HANA jako [hosta automatycznej pracy w trybie Failover](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/ae60cab98173431c97e8724856641207.html). Ta konfiguracja może być uzasadniona w sytuacji wdrożenia lokalnego, gdy sprzęt serwera jest ograniczony i dedykować węzła pojedynczego serwera jako węzeł hosta automatycznej pracy w trybie Failover dla zestawu hostów produkcyjnych. Jednak w sytuacjach, takich jak Azure, w którym podstawowej infrastruktury Azure zapewnia serwer docelowy dobrej kondycji powiodło się ponowne uruchomienie maszyny wirtualnej, scenariusz SAP HANA hosta automatycznej pracy w trybie Failover nie ma sensu do wdrożenia. 

W związku z tym nie mamy żadnych architektura referencyjna struktury przewiduje węzła wstrzymania dla HANA hosta automatycznej pracy w trybie Failover. Dotyczy to również w przypadku konfiguracji skalowania w poziomie SAP HANA.


## <a name="availability-scenarios-involving-two-different-vms"></a>Dostępność scenariusze obejmujące dwóch różnych maszyn wirtualnych
Za pomocą dwóch maszyn wirtualnych platformy Azure w ramach zestawami dostępności Azure umożliwiają Zwiększ wydłużyć czas między tych dwóch maszyn wirtualnych, jeśli te maszyny Wirtualne są umieszczane w zestawie Azure dostępności w ramach jednego regionu platformy Azure. Podstawowej instalacji platformy Azure będzie wyglądała grafiki pokazane: ![dwóch maszyn wirtualnych z warstwami wszystkie](./media/sap-hana-availability-one-region/two_vm_all_shell.PNG)

Aby zilustrować dostępności różnych scenariuszy, kilku warstw powyżej są wycinanie i grafiki ograniczone do warstwy maszyn wirtualnych, hostów, zestawy dostępności i regiony platformy Azure. Rola w scenariuszach opisano nie są odtwarzane sieci wirtualnych platformy Azure, grupy zasobów i subskrypcje.

### <a name="replicating-backups-to-second-virtual-machine"></a>Replikowanie kopii zapasowych do drugiej maszyny wirtualnej
Jedną z najbardziej podstawowe konfiguracje jest kopii zapasowych, szczególnie kopie zapasowe dziennika transakcji wysłane z jednej maszyny Wirtualnej do innej maszyny wirtualnej Azure. Jest to możliwe dowolnego typu usługi Azure Storage. Użytkownik będzie odpowiedzialny za wykonywanie skryptów kopii zaplanowanych kopii zapasowych dokonywanych na pierwszej maszynie Wirtualnej do drugiej maszyny Wirtualnej. W przypadku używania wymagające wystąpienia drugiej maszyny Wirtualnej, będzie potrzebny do przywrócenia pełnej, przyrostowej/różnicowej i kopie zapasowe dziennika transakcji do punktu, które są potrzebne. Architektura powinien wyglądać tak: ![dwóch maszyn wirtualnych z replikacji magazynu](./media/sap-hana-availability-one-region/two_vm_storage_replication.PNG) 

Ta konfiguracja nie nadaje się zbyt służące osiągnięciu dużą RPO i RTO czasów. Szczególnie RTO razy może się pogorszyć ze względu na potrzeby pełnego przywrócenia pełnej bazy danych z skopiowanych kopii zapasowych. Jednak taka konfiguracja jest możliwe pozwoli na odzyskanie usunięcia niezamierzone danych na głównym wystąpień. w przypadku takiej konfiguracji jest możliwe w dowolnym momencie przywrócić pewnym wyodrębniania danych i zaimportuj usuniętych danych do wystąpienia głównego. Dlatego go warto używać taka metoda kopii zapasowej w połączeniu z innymi funkcjami wysokiej dostępności. W czasie, gdy tylko kopie zapasowe są kopiowane, może zostać wyświetlony wraz z maszyny Wirtualnej mniejsze niż główny SAP HANA wystąpień jest uruchomiony w. Jednak należy pamiętać o tym, które mniejszych maszyn wirtualnych ma mniejszą liczbę wirtualnych dysków twardych, które można dołączyć. Sprawdź [rozmiary maszyn wirtualnych systemu Linux na platformie Azure](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) limitów poszczególnych typów maszyny Wirtualnej.

### <a name="using-sap-hana-system-replication-without-automatic-failover"></a>Za pomocą replikacji systemu SAP HANA bez automatycznej pracy awaryjnej
W następujących scenariuszach są przy użyciu replikacji w systemie SAP HANA. SAP wystawiony dokumentację można znaleźć począwszy od tego artykułu [replikacji systemu](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.01/en-US/b74e16a9e09541749a745f41246a065e.html). Między dwoma maszyn wirtualnych Azure w pojedynczym regionie Azure istnieją dwa różne konfiguracje, które mają pewne różnice w celu czasu odzyskiwania. Ogólnie rzecz biorąc scenariusze z nie ma automatycznej pracy awaryjnej nie może być zbyt odpowiednie w scenariuszach, w ramach jednego regionu Azure. Przyczyny, która jest, że w większości przypadków awarii w infrastrukturze Azure naprawą usługi Azure będzie ponowne uruchomienie podstawowej maszyny Wirtualnej na innego hosta. Istnieją tylko przypadki krawędzi, gdy taka konfiguracja może pomóc w przypadku pewnych scenariuszy awarii. Lub czasami jako klient chcesz zrealizować szczególnie wokół wydajności.

#### <a name="using-hana-system-replication-without-auto-failover-and-without-data-pre-load"></a>Za pomocą replikacji systemu HANA bez automatycznej pracy awaryjnej i bez wstępnego ładowania danych 
Jest scenariusz, w których replikacji systemu SAP HANA są używane na potrzeby przenoszenia danych w sposób synchroniczne do osiągnięcia odzyskiwania punktu cel (RPO) 0. Z drugiej strony, masz wystarczająco długie odzyskiwania czasu cel (RTO), tak, który nie ma potrzeby trybu failover lub wstępnego ładowania danych do wystąpienia pamięci podręcznej HANA. W takim przypadku masz możliwości dysków dalsze ekonomii do konfiguracji przez:

- W drugim maszynę Wirtualną, która przyjmuje większość pamięci maszyny wirtualnej, można uruchomić inne wystąpienie SAP HANA. Zazwyczaj takie wystąpienie może być wystąpienie, które w przypadku awaryjnej do drugiej maszyny Wirtualnej może zostać zamknięty. Dlatego, że replikowane dane mogą zostać załadowane do pamięci podręcznej docelowego wystąpienia HANA w drugiej maszyny Wirtualnej.
- Możesz użyć mniejszego rozmiaru maszyny Wirtualnej jako drugi maszyny Wirtualnej. W przypadku trybu failover konieczne będzie kroku przed ręcznego przełączania trybu failover gdzie zmieniania rozmiaru maszyny Wirtualnej do rozmiaru źródłowej maszyny Wirtualnej. Scenariusz wygląda następująco:

![Dwie maszyny wirtualne z replikacji magazynu](./media/sap-hana-availability-one-region/two_vm_HSR_sync_nopreload.PNG)

> [!NOTE]
> Wstępne ładowanie danych w celu replikacji systemu HANA, nawet jeśli nie potrzebujesz co najmniej 64 GB pamięci RAM i poza za mało pamięci, aby zachować dane magazynu wierszy w pamięci obiektu docelowego.

#### <a name="using-hana-system-replication-without-auto-failover-and-with-data-pre-load"></a>Za pomocą replikacji systemu HANA bez automatycznej pracy awaryjnej i wstępnego ładowania danych
Różnica scenariusza wprowadzone przed jest wstępnie załadować danych, które zostaną zreplikowane do wystąpienia HANA w drugiej maszyny Wirtualnej. To wyeliminować dwóch korzyści, jakie mogą mieć ze scenariuszem nie wstępnego ładowania danych. W takim przypadku nie można uruchomić innego systemu SAP HANA na drugiej maszyny Wirtualnej. Nie można użyć mniejszego rozmiaru maszyny Wirtualnej. W związku z tym jest to scenariusz trudno jest implementowana przy użyciu klientów


### <a name="using-sap-hana-system-replication-with-automatic-failover"></a>Za pomocą replikacji systemu SAP HANA z automatycznej pracy awaryjnej

Konfiguracja standardowa dostępności w ramach jednego regionu Azure, większość klientów wdrażania z SAP HANA, jest to konfiguracja, w której dwie maszyny wirtualne Azure systemem SLES Linux mają zdefiniowane klastra pracy awaryjnej. Klaster systemu Linux SLES opiera się na [rozrusznik](http://www.linux-ha.org/wiki/Pacemaker) framework w połączeniu z [STONITH](http://linux-ha.org/wiki/STONITH) urządzenia. Z perspektywy SAP HANA używany tryb replikacji jest synchronizowane i skonfigurowano automatycznej pracy awaryjnej. W drugiej maszyny Wirtualnej z wystąpieniem SAP HANA działa jako węzeł rezerwy dynamicznej, który odbiera strumień synchroniczne zmian rekordów z podstawowego wystąpienia SAP HANA. Jak transakcji uzyskać przekazanych przez aplikację w węźle podstawowym HANA, węzeł podstawowy HANA oczekuje potwierdzić zatwierdzenia aplikacji, aż do węzła pomocniczego SAP HANA potwierdzone po otrzymaniu rekordu zatwierdzania. SAP HANA dwóch różnych Replikacja synchroniczna tryby. Szczegółowe informacje i różnice w tych dwóch trybów Replikacja synchroniczna, przeczytaj artykuł na temat [tryby replikacji dla replikacji systemu SAP HANA](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/en-US/c039a1a5b8824ecfa754b55e0caffc01.html)

Ogólna konfiguracja wygląda

![Dwie maszyny wirtualne z replikacji magazynu i pracy awaryjnej](./media/sap-hana-availability-one-region/two_vm_HSR_sync_auto_pre_preload.PNG)

To rozwiązanie jest wybierany, ponieważ pozwala na osiągnięcie RPO = 0 i bardzo niskim RTO razy. Skonfiguruj połączenie klienta SAP HANA w taki sposób, aby klienci SAP HANA używać wirtualnego adresu IP do nawiązania połączenia konfiguracji replikacji systemu HANA. Eliminuje to konieczność zmiany konfiguracji aplikacji w przypadku trybu failover w węźle pomocniczym. W tym rozwiązaniu jednostki SKU maszyny Wirtualnej platformy Azure dla podstawowej lub pomocniczej muszą być takie same.  


## <a name="next-steps"></a>Następne kroki
Aby uzyskać wskazówki krok po kroku dotyczące sposobu konfigurowania takiej konfiguracji, na platformie Azure, zapoznaj się z artykułami:

- [Instalator programu SAP HANA System replikacji na maszynach wirtualnych platformy Azure](sap-hana-high-availability.md)
- [Twoje SAP na wysoką dostępność SAP HANA za pomocą replikacji systemu Azure — część 4 —](https://blogs.sap.com/2018/01/08/your-sap-on-azure-part-4-high-availability-for-sap-hana-using-system-replication/)

Aby uzyskać więcej informacji na temat dostępności SAP HANA w regionach platformy Azure, przeczytaj:

- [SAP HANA dostępność w regionach platformy Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/sap-hana-availability-across-regions) 

