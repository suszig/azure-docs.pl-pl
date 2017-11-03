---
title: "Połączenie maszyn wirtualnych po przejściu w tryb failover lokację dodatkową z usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Wskazówki dotyczące sieci w celu nawiązania maszyn wirtualnych po przejściu w tryb failover lokację dodatkową z usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 67d73590-185c-49b2-a097-597bf54747a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2017
ms.author: raynew
ms.openlocfilehash: 7b27fc568c77b44ab2366d297ca9e7685439143e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="vm-connectivity-after-failover-to-a-secondary-site"></a>Łączność maszyn wirtualnych po przejściu w tryb failover w lokacji dodatkowej

Po przejrzeniu wymagania wstępne dotyczące wdrażania, przeczytaj ten artykuł, aby zaplanować sieci podczas replikacji maszyn wirtualnych funkcji Hyper-V (VM) zarządzane w chmurach programu System Center Virtual Machine Manager (VMM), do lokacji dodatkowej za pomocą [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure. 

Po przeczytaniu tego artykułu zamieść wszelkie komentarze u dołu lub na [forum usług Azure Recovery Services](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Omówienie

Podczas planowania strategii trybu failover i replikacji, na których, jedno z pytań klucza jest sposób nawiązywania połączenia z repliką po pracy awaryjnej. Istnieje kilka opcji: 

- **Użyj innego adresu IP**: można określić, aby użyć innego adresu IP dla zreplikowanej maszyny Wirtualnej. W tym scenariuszu maszyny Wirtualnej pobiera nowego adresu IP po w tryb failover i aktualizowania DNS jest wymagany.
- **Zachować ten sam adres IP**: możesz użyć tego samego adresu IP dla repliki maszyny Wirtualnej. Utrzymywanie tego samego adresu IP adresów upraszcza odzyskiwanie dzięki zmniejszeniu problemów dotyczących sieci po pracy awaryjnej. 

## <a name="retaining-ip-addresses"></a>Zachowanie adresów IP

Jeśli chcesz zachować adresów IP z lokacji głównej po przejściu w tryb failover lokacji dodatkowej, tryb failover pełne podsieci i zaktualizować tras, aby wskazać, do nowej lokalizacji adresów IP lub zamiast wdrażać rozciągnięty podsieci między serwerem podstawowym i lokacji odzyskiwania.

### <a name="stretched-subnet"></a>Rozciągnięty podsieci

W podsieci rozciągnięty podsieć jest dostępna jednocześnie w lokacji głównej i dodatkowej. Po przeniesieniu serwera i jego konfiguracja adresu IP (warstwy 3) do lokacji dodatkowej sieci będzie kierować ruch do nowej lokalizacji automatycznie. 

Z perspektywy warstwy 2 (warstwę łącza danych) należy sprzęt sieciowy, którym może zarządzać rozciągnięty sieci VLAN. Ponadto rozciągnięty sieci VLAN, potencjalne domeny błędów rozszerza do obu lokacji zasadniczo staje się pojedynczym punktem awarii. Jest to mało prawdopodobne, może się zdarzyć, że emisji storm uruchomiona, a nie może być izolowane. 


### <a name="subnet-failover"></a>Tryb failover podsieci

Można uruchomić tryb failover podsieci czerpać korzyści z rozciągnięty podsieci, bez jego faktycznego rozciągania. W tym rozwiązaniu podsieci będą dostępne w lokacji źródłowej lub docelowej, ale nie oba jednocześnie. Aby zachować przestrzeń adresów IP w przypadku trybu failover, można programowo Rozmieść infrastruktury router przenieść podsieci z jednej lokacji. Po czasie pracy awaryjnej podsieci przeniosłaby skojarzone maszyn wirtualnych. Główną wadą jest, że w przypadku awarii, należy przenieść całej podsieci.

### <a name="example"></a>Przykład

Oto przykład podsieci ukończenia pracy awaryjnej. Lokacja główna ma aplikacji uruchomionych w podsieci 192.168.1.0/24. W trybie failover wszystkich maszyn wirtualnych w tej podsieci można w lokacji dodatkowej i zachowywanie ich adresy IP. Trasy muszą zostać zmodyfikowane w celu odzwierciedlenia faktu, że wszystkie maszyny wirtualne maszyny Wirtualnej należących do podsieci 192.168.1.0/24 teraz zostały przeniesione do lokacji dodatkowej.

Poniższej grafice Pokaż podsieci przed i po trybu failover:

- Przed trybu failover 192.168.0.1/24 podsieci jest aktywny w lokacji źródłowej, stanie się aktywne w lokacji dodatkowej po pracy awaryjnej.
- Tras między lokacji głównej i lokacji odzyskiwania, trzeci lokacji i lokacji głównej i trzeci lokacji i lokacji odzyskiwania, należy odpowiednio można modyfikować.

**Przed trybu failover**

![Przed trybu Failover](./media/vmm-to-vmm-walkthrough-network/network-design2.png)

**Po pracy awaryjnej**

![Po pracy awaryjnej](./media/vmm-to-vmm-walkthrough-network/network-design3.png)

Po przejściu w tryb failover Oto, co się stanie:

- Usługa Site Recovery przydziela adres IP dla każdego interfejsu sieciowego na Maszynie wirtualnej, z puli statycznych adresów IP w sieci istotne dla każdego wystąpienia programu VMM.
- Jeśli w puli adresów IP w lokacji dodatkowej jest takie samo jak w lokacji źródłowej, Usługa Site Recovery przydziela ten sam adres IP (of źródłowej maszyny Wirtualnej) do maszyny Wirtualnej repliki. Adres IP jest zastrzeżony w programie VMM, ale nie została ustawiona jako adresu IP trybu failover, na hoście funkcji Hyper-V. Adres IP trybu failover na hoście funkcji Hyper-v ustawiono tuż przed pracy awaryjnej.
- Jeśli ten sam adres IP nie jest dostępna, Usługa Site Recovery przydziela kolejny dostępny adres IP z puli.
- Jeśli maszyny wirtualne korzystania z protokołu DHCP, Usługa Site Recovery nie zarządzania adresami IP. Należy sprawdzić, czy serwera DHCP w lokacji dodatkowej można przydzielić adresu z tego samego zakresu, jak lokacja źródłowa.

### <a name="validate-the-ip-address"></a>Sprawdź poprawność adresu IP

Po włączeniu ochrony dla maszyny Wirtualnej można następujący przykładowy skrypt zweryfikować adres przypisany do maszyny Wirtualnej. Ten sam adres IP zostanie ustawiony jako adresu IP trybu failover i przypisane do maszyny Wirtualnej w trybie failover:

    ```
    $vm = Get-SCVirtualMachine -Name <VM_NAME>
    $na = $vm[0].VirtualNetworkAdapters>
    $ip = Get-SCIPAddress -GrantToObjectID $na[0].id
    $ip.address 
    ```

## <a name="changing-ip-addresses"></a>Zmiana adresów IP

W tym scenariuszu zmieniły się adresy IP maszyn wirtualnych działających w trybie Failover. Wadą tego rozwiązania jest utrzymanie wymagane. Zazwyczaj DNS zostanie zaktualizowany po uruchomieniu maszyn wirtualnych repliki. Wpisy DNS może być konieczne można zmienić lub fluster sieciowego i wpisów pamięci podręcznej zaktualizowane. Może to spowodować Przestój. Można zminimalizować czas przestoju w następujący sposób:

- Użyj niskich wartości TTL dla aplikacji sieci intranet.
- Użyj następującego skryptu w planie odzyskiwania usługi Site Recovery, aby zaktualizować serwer DNS w celu zapewnienia aktualnych aktualizacji. Skrypt nie jest konieczne użycie dynamiczną rejestrację DNS.

    ```
    param(
    string]$Zone,
    [string]$name,
    [string]$IP
    )
    $Record = Get-DnsServerResourceRecord -ZoneName $zone -Name $name
    $newrecord = $record.clone()
    $newrecord.RecordData[0].IPv4Address  =  $IP
    Set-DnsServerResourceRecord -zonename $zone -OldInputObject $record -NewInputObject $Newrecord
    ```
    
### <a name="example"></a>Przykład 

Przyjrzyjmy się scenariusz, w którym planujesz używać różnych adresów IP między serwerem podstawowym i lokacji odzyskiwania. W tym przykładzie mamy różne adresy IP w lokacjach głównych i dodatkowych, a; s innej lokacji z aplikacji, które hostowanej w lokacji podstawowej lub odzyskiwania jest dostępny.

- Przed trybu failover aplikacje są 192.168.1.0/24 hostowanej podsieci w lokacji głównej i są skonfigurowane w podsieci 172.16.1.0/24 w lokacji dodatkowej po przejściu w tryb failover.
- Trasy połączeń i sieci VPN zostały skonfigurowane odpowiednio tak, aby wszystkie trzy witryny mają dostęp do siebie.
- Po przejściu w tryb failover aplikacje zostaną przywrócone w podsieci odzyskiwania. W tym scenariuszu nie istnieje potrzeba do pracy awaryjnej w całej podsieci i ponownie skonfigurować trasy sieci VPN lub sieć nie są wymagane żadne zmiany. Tryb failover, a niektóre aktualizacje DNS upewnij się, że aplikacje będą nadal dostępne.
- Jeśli DNS jest skonfigurowany do zezwalania na aktualizacje dynamiczne, maszyn wirtualnych będzie rejestrują się za pomocą nowego adresu IP, po rozpoczęciu po pracy awaryjnej.

**Przed trybu failover**

![Różne IP — przed trybu Failover](./media/vmm-to-vmm-walkthrough-network/network-design10.png)

**Po pracy awaryjnej**

![IP różne — od trybu Failover](./media/vmm-to-vmm-walkthrough-network/network-design11.png)




