---
title: "Testowanie trybu failover (VMM do programu VMM) w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Usługa Azure Site Recovery koordynuje replikację, tryb failover i odzyskiwania maszyn wirtualnych i serwerów fizycznych. Więcej informacji na temat trybu failover do platformy Azure lub dodatkowego centrum danych."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: ponatara
ms.openlocfilehash: 2730cebc1cdc47db283ae851560d93fdbf50ee48
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="test-failover-vmm-to-vmm-in-site-recovery"></a>Testowanie trybu failover (VMM do programu VMM) w usłudze Site Recovery


Ten artykuł zawiera informacje i instrukcje dotyczące wykonywania test trybu failover lub szczegółowego odzyskiwanie po awarii, maszynach wirtualnych (VM) i serwerów fizycznych, które są chronione za pomocą usługi Azure Site Recovery. Użyjesz zarządzanych przez program System Center Virtual Machine Manager VMM lokalnej lokacji jako lokacji odzyskiwania.

Możesz uruchomić test trybu failover, aby zweryfikować strategii replikacji lub wykonaj wyszczególniania odzyskiwania po awarii, bez utraty danych lub przestoju. Test trybu failover nie ma żadnego wpływu na trwającej replikacji lub w środowisku produkcyjnym. Możesz uruchomić ją na maszyny wirtualnej lub a [planu odzyskiwania](site-recovery-create-recovery-plans.md). Gdy jest wyzwalają test trybu failover, należy określić podłączoną testowych maszyn wirtualnych do sieci. Możesz śledzić postęp testowania trybu failover na **zadania** strony.  

Jeśli masz wszelkie komentarze lub pytania zamieścić je w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="prepare-the-infrastructure-for-test-failover"></a>Przygotowanie infrastruktury do testowania trybu failover
Aby uruchomić test trybu failover za pomocą istniejącej sieci, należy przygotować usługi Active Directory, DHCP i DNS w sieci.

Jeśli chcesz uruchomić test trybu failover przy użyciu opcji, aby automatycznie utworzyć sieci maszyny Wirtualnej, należy dodać krok wykonywany ręcznie przed grupy 1 w planie odzyskiwania, który będzie używać do testowania trybu failover. Następnie należy dodać zasoby infrastruktury automatycznie utworzone sieci przed uruchomieniem testowania trybu failover.

### <a name="things-to-note"></a>Rzeczy do uwzględnienia
Jeśli przeprowadzasz replikację do lokacji dodatkowej, typ sieci, który używa maszyny repliki nie musi być zgodny typ sieci logicznej używanej do testowania trybu failover, ale niektóre kombinacje może nie działać. Jeśli replika używa protokołu DHCP i izolacji opartej na sieci VLAN, sieć maszyny Wirtualnej repliki nie wymaga puli statycznych adresów IP. Do testowania trybu failover przy użyciu wirtualizacja sieci systemu Windows nie będzie działać, ponieważ nie są dostępne żadne pule adresów. 

Ponadto testowania trybu failover nie będzie działać, jeśli sieć repliki korzysta Brak izolacji, a sieci testowej wirtualizacja sieci systemu Windows. Jest to spowodowane Brak izolacji sieci nie ma podsieci, wymagane do utworzenia sieci wirtualizacja sieci systemu Windows.

Jak maszyny wirtualne repliki są podłączone do mapowanej sieci maszyn wirtualnych po pracy awaryjnej zależy od konfiguracji sieci maszyny Wirtualnej w konsoli programu VMM.

#### <a name="vm-network-configured-with-no-isolation-or-vlan-isolation"></a>Sieć maszyny Wirtualnej skonfigurowaną bez izolacji i izolacji sieci VLAN
DHCP jest zdefiniowana dla sieci maszyny Wirtualnej, maszyna wirtualna repliki jest podłączony do Identyfikatora sieci VLAN za pomocą ustawień, które są określone dla lokacji sieciowej w sieci logiczne skojarzone. Maszyna wirtualna otrzymuje adres IP z dostępnego serwera DHCP. 

Nie trzeba definiować puli statycznych adresów IP sieci docelowej maszyny Wirtualnej. Użycie puli statycznych adresów IP dla sieci maszyny Wirtualnej maszyny wirtualnej repliki jest podłączony do Identyfikatora sieci VLAN za pomocą ustawień, które są określone dla lokacji sieciowej w sieci logiczne skojarzone.

Maszyna wirtualna otrzymuje adres IP z puli, która jest zdefiniowana dla sieci maszyny Wirtualnej. Jeśli puli statycznych adresów IP nie jest zdefiniowana w sieci docelowej maszyny Wirtualnej, przydzielanie adresów IP zakończy się niepowodzeniem. Utwórz pulę adresów IP na serwerach VMM źródłowych i docelowych, które będą używane do ochrony i odzyskiwania.

#### <a name="vm-network-with-windows-network-virtualization"></a>Sieci Vmnetwork z wirtualizacją sieci systemu Windows
Jeśli sieć wirtualna jest skonfigurowane z wirtualizacją sieci systemu Windows, należy zdefiniować pulę statycznych dla docelowej sieci maszyny Wirtualnej, niezależnie od tego, czy źródłowej sieci maszyny Wirtualnej jest skonfigurowana do używania protokołu DHCP lub statyczna pula adresów IP. 

Po zdefiniowaniu DHCP docelowym serwerze VMM działa jako serwer DHCP i udostępnia adres IP z puli, która jest zdefiniowana w sieci docelowej maszyny Wirtualnej. Jeśli korzystanie z puli statycznych adresów IP jest zdefiniowana na serwerze źródłowym, docelowym serwerze VMM przydziela adres IP z puli. W obu przypadkach przydzielanie adresów IP zakończy się niepowodzeniem, jeśli nie zdefiniowano puli statycznych adresów IP.


### <a name="prepare-dhcp"></a>Przygotowanie DHCP
W przypadku maszyn wirtualnych testowy tryb failover korzystać z protokołu DHCP, Utwórz test serwera DHCP w sieci izolowanej na potrzeby testowania trybu failover.

### <a name="prepare-active-directory"></a>Przygotowanie usługi Active Directory
Aby uruchomić test trybu failover do testowania aplikacji, należy kopię w środowisku produkcyjnym usługi Active Directory w środowisku testowym. Aby uzyskać więcej informacji, przejrzyj [test pracy awaryjnej zagadnienia dotyczące usługi Active Directory](site-recovery-active-directory.md#test-failover-considerations).

### <a name="prepare-dns"></a>Przygotowanie DNS
Przygotuj serwer DNS do testowania trybu failover w następujący sposób:

* **DHCP**: maszyn wirtualnych, użycie protokołu DHCP, adres IP testu DNS należy zaktualizować na serwerze DHCP testu. Jeśli używasz sieci typu wirtualizacja sieci systemu Windows, serwer VMM działa jako serwer DHCP. Adres IP serwera DNS powinna zostać uaktualnione w testowej sieci trybu failover. W przypadku maszyn wirtualnych rejestrują się do odpowiedniego serwera DNS.
* **Statyczny adres**: maszyny wirtualnej użycie statycznego adresu IP, adres IP serwera DNS testu powinien zostać zaktualizowany w testowej sieci trybu failover. Konieczne może być zaktualizować DNS przy użyciu adresu IP maszyn wirtualnych testu. Poniższy przykładowy skrypt można użyć w tym celu:

        Param(
        [string]$Zone,
        [string]$name,
        [string]$IP
        )
        $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
        $newrecord = $record.clone()
        $newrecord.RecordData[0].IPv4Address  =  $IP
        Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord



## <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover
Ta procedura opisuje sposób testować tryb failover planu odzyskiwania. Alternatywnie można uruchomić trybu failover dla jednej maszyny wirtualnej na **maszyn wirtualnych** kartę.

![Test pracy awaryjnej bloku](./media/site-recovery-test-failover-vmm-to-vmm/TestFailover.png)

1. Wybierz **plany odzyskiwania** > *recoveryplan_name*. Kliknij przycisk **pracy awaryjnej** > **testowanie trybu Failover**.
1. Na **testowego trybu Failover** bloku, określ, jak maszyny wirtualne powinny być podłączone do sieci po testowania trybu failover. Aby uzyskać więcej informacji, zobacz [opcje sieciowe](#network-options-in-site-recovery).
1. Śledzić postęp trybu failover na **zadania** kartę.
1. Po zakończeniu trybu failover Sprawdź pomyślnie uruchomić maszyny wirtualnej.
1. Gdy wszystko będzie gotowe, kliknij przycisk **oczyszczania testowy tryb failover** dla planu odzyskiwania. W **uwagi**, zarejestrować i zapisać wszelkie obserwacje związane z testowania trybu failover. Ten krok polega na usunięciu maszyn wirtualnych i sieci, które zostały utworzone podczas testowania trybu failover.


## <a name="network-options-in-site-recovery"></a>Opcje sieciowe w usłudze Site Recovery

Po uruchomieniu test trybu failover, użytkownik jest proszony o wybierz ustawienia sieci dla maszyny repliki testu. Istnieje wiele opcji.  

| **Opcja pracy awaryjnej testu** | **Opis** | **Sprawdzanie trybu failover** | **Szczegóły** |
| --- | --- | --- | --- |
| **Przełączyć dodatkowej lokacji programu VMM — bez sieci** |Nie zaznaczaj sieci maszyny Wirtualnej. |Tryb failover sprawdza, czy utworzony testowe maszyny.<br/><br/>Testowej maszyny wirtualnej jest tworzony na hoście, jeżeli istnieje maszyny wirtualnej repliki. Nie jest dodawany do chmury, w którym znajduje się maszyna wirtualna repliki. |<p>Maszyna przełączona w tryb failover nie jest połączona z żadną siecią.<br/><br/>Komputer można podłączyć do sieci maszyny Wirtualnej po jego utworzeniu. |
| **Przełączyć dodatkowej lokacji programu VMM — sieci** |Wybierz istniejąca sieć maszyny Wirtualnej. |Tryb failover sprawdza, czy maszyny wirtualne są tworzone. |Testowej maszyny wirtualnej jest tworzony na hoście, jeżeli istnieje maszyny wirtualnej repliki. Nie jest dodawany do chmury, w którym znajduje się maszyna wirtualna repliki.<br/><br/>Utwórz sieć maszyny Wirtualnej, która jest odizolowana od produkcyjnego środowiska sieciowego.<br/><br/>Jeśli używasz sieci opartej na sieci VLAN, zalecamy utworzenie oddzielnych sieci logicznej (nie używane w środowisku produkcyjnym) w programie VMM w tym celu. Ta sieć logiczna jest używana do tworzenia sieci maszyny Wirtualnej dla testu pracy w trybie Failover.<br/><br/>Sieć logiczna powinna być skojarzona z co najmniej jedną z kart sieciowych wszystkich serwerów funkcji Hyper-V, które obsługują maszyny wirtualne.<br/><br/>Dla sieci logicznych VLAN powinna zostać odizolowana Lokacje sieciowe, które dodajesz do sieci logicznej.<br/><br/>Jeśli używasz sieć logiczną na podstawie wirtualizacja sieci systemu Windows Azure Site Recovery automatycznie tworzy izolowane sieci maszyny Wirtualnej. |
| **Tryb failover do lokacji dodatkowej programu VMM — tworzenie sieci** |Sieci testowej tymczasowy jest tworzony automatycznie zgodnie z ustawieniem w **sieci logicznej** i jej lokacji związane z siecią. |Tryb failover sprawdza, czy maszyny wirtualne są tworzone. |Użyj tej opcji, jeśli plan odzyskiwania korzysta z więcej niż jedną sieć maszyny Wirtualnej. Jeśli używasz wirtualizacji sieci systemu Windows, ta opcja może automatycznie tworzyć sieci maszyn wirtualnych z tymi samymi ustawieniami (podsieci i pule adresów IP) w sieci maszyny wirtualnej repliki. Te sieci maszyn wirtualnych są automatycznie czyszczone po zakończeniu testowania trybu failover.</p><p>Testowej maszyny wirtualnej jest tworzony na hoście, jeżeli istnieje maszyny wirtualnej repliki. Nie jest dodawany do chmury, w którym znajduje się maszyna wirtualna repliki. |

> [!TIP]
> Adres IP podany dla maszyny wirtualnej podczas testowania trybu failover jest ten sam adres IP, który otrzyma maszyny wirtualnej dla planowanego lub nieplanowanego trybu failover (przy założeniu, że adres IP jest dostępny w testowej sieci trybu failover). Jeśli ten sam adres IP nie jest dostępny w testowej sieci trybu failover, maszyna wirtualna odbiera innego adresu IP, który jest dostępny w testowej sieci trybu failover.
>
>


## <a name="test-failover-to-a-production-network-on-a-recovery-site"></a>Testowanie trybu failover do środowiska produkcyjnego w lokacji odzyskiwania
Firma Microsoft zaleca podczas prowadzenia test trybu failover, wybór sieci, która różni się od sieci produkcyjnej odzyskiwania lokacji podane w [mapowania sieci](https://docs.microsoft.com/azure/site-recovery/site-recovery-network-mapping). Jednak jeśli na pewno chcesz zweryfikować łączności sieciowej na całej trasie na maszynie wirtualnej przełączona w tryb failover, weź pod uwagę następujące kwestie:

* Upewnij się, że podstawowa maszyna wirtualna jest zamknięta podczas testowania trybu failover podczas wykonywania. Jeśli nie zostanie dwóch maszyn wirtualnych o tej samej tożsamości będzie uruchomiony w tej samej sieci, w tym samym czasie. Taka sytuacja może spowodować niepożądane skutki.
* Czyszczenie testowych maszyn wirtualnych w tryb failover, wszelkie zmiany wprowadzone w testowej maszyny wirtualnej w tryb failover zostaną utracone. Te zmiany nie są replikowane do podstawowej maszyny wirtualnej.
* Ten sposób prowadzenia testów prowadzi do przestojów w przypadku aplikacji produkcyjnej. Poproś użytkowników aplikacji, nie należy używać aplikacji, gdy Trwa przechodzenie do szczegółów odzyskiwania po awarii.  


## <a name="next-steps"></a>Następne kroki
Po pomyślnym uruchomieniu test trybu failover, można spróbować [pracy awaryjnej](site-recovery-failover.md).
