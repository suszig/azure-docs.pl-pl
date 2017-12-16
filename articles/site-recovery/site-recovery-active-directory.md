---
title: "Ochrona usługi Active Directory i DNS z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak wdrożyć rozwiązanie odzyskiwania po awarii dla usługi Active Directory przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: mayanknayar
manager: rochakm
editor: 
ms.assetid: af1d9b26-1956-46ef-bd05-c545980b72dc
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/15/2017
ms.author: manayar
ms.openlocfilehash: 5db2d424c176428d31fb99fd8288120f18fcac7a
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="protect-active-directory-and-dns-with-azure-site-recovery"></a>Ochrona usługi Active Directory i DNS z usługą Azure Site Recovery
Aplikacje przedsiębiorstwa, takich jak SharePoint, Dynamics AX i SAP są zależne od usługi Active Directory i infrastruktury DNS, aby mógł działać poprawnie. Podczas tworzenia rozwiązanie odzyskiwania po awarii dla aplikacji, często konieczne będzie odzyskanie przed inne składniki aplikacji w celu zapewnienia ich funkcjonalności właściwej aplikacji usługi Active Directory i DNS.

Przy użyciu usługi Site Recovery, można utworzyć plan odzyskiwania ukończenia automatycznego po awarii dla usługi Active Directory. W przypadku wystąpienia zakłóceń można Zainicjuj tryb failover w ciągu kilku sekund z dowolnego miejsca i pobrać i uruchomić usługi Active Directory w kilka minut. Jeśli w lokacji głównej wdrożeniu usługi Active Directory dla wielu zastosowań, takich jak SharePoint i SAP, i chcesz trybu failover całej lokacji, możesz pracy awaryjnej usługi Active Directory najpierw przy użyciu usługi Site Recovery, a następnie przełączyć inne aplikacje korzystające z plany odzyskiwania specyficzne dla aplikacji.

W tym artykule opisano sposób tworzenia rozwiązanie odzyskiwania po awarii dla usługi Active Directory, jak wykonać przełączenia do trybu failover przy użyciu planu odzyskiwania jednym kliknięciem, obsługiwane konfiguracje i wymagania wstępne.  Należy zapoznać się z usługą Active Directory i Azure Site Recovery, przed jego uruchomieniem.

## <a name="prerequisites"></a>Wymagania wstępne
* Magazyn usług odzyskiwania w ramach subskrypcji Microsoft Azure.
* Jeśli przeprowadzasz replikację do platformy Azure, [przygotowanie](tutorial-prepare-azure.md) zasobów platformy Azure, w tym subskrypcji platformy Azure, sieć wirtualną platformy Azure i konto magazynu.
* Przejrzyj wymagania dotyczące pomocy technicznej dla wszystkich składników.

## <a name="replicating-domain-controller"></a>Replikowanie kontrolera domeny

Należy w Instalatorze [replikacji usługi Site Recovery](#enable-protection-using-site-recovery) na co najmniej jednej maszyny wirtualnej hostingu DNS i kontrolera domeny. Jeśli masz [wielu kontrolerów domeny](#environment-with-multiple-domain-controllers) w danym środowisku, oprócz replikacji maszyny wirtualnej kontrolera domeny z usługą Site Recovery również należy skonfigurować [dodatkowy kontroler domeny](#protect-active-directory-with-active-directory-replication)w docelowej lokacji (Azure lub lokalnego pomocniczego centrum danych).

### <a name="single-domain-controller-environment"></a>Środowiska kontrolera domeny pojedynczej
Jeśli masz kilka aplikacji i tylko jednego kontrolera domeny, a chcesz przełączyć całej witryny razem, następnie zaleca się przy użyciu usługi Site Recovery w ramach replikacji kontrolera domeny do lokacji docelowej (Azure lub lokalnego pomocniczego centrum danych). Taki sam replikowane domeny DNS kontrolera maszyny wirtualnej może służyć do [testowanie trybu failover](#test-failover-considerations) również.

### <a name="environment-with-multiple-domain-controllers"></a>Środowisko z wielu kontrolerów domeny
Jeśli masz wiele aplikacji i istnieje więcej niż jeden kontroler domeny w środowisku, lub jeśli planujesz awaryjnie kilka aplikacji w czasie, zalecamy, aby oprócz replikacji maszyny wirtualnej kontrolera domeny z usługą Site Recovery, należy również Konfigurowanie [dodatkowy kontroler domeny](#protect-active-directory-with-active-directory-replication) w docelowej lokacji (Azure lub lokalnego pomocniczego centrum danych). Aby uzyskać [testowanie trybu failover](#test-failover-considerations), można użyć kontrolera domeny zreplikowane przez usługę Site Recovery i pracy awaryjnej, dodatkowy kontroler domeny w lokacji docelowej.

## <a name="enable-protection-using-site-recovery"></a>Włącz ochronę za pomocą usługi Site Recovery
### <a name="protect-the-virtual-machine"></a>Ochrona maszyny wirtualnej
Włącz ochronę maszyny wirtualnej DNS kontrolera domeny w usłudze Site Recovery. Kontroler domeny replikowane za pomocą usługi Site Recovery jest używana do [testowanie trybu failover](#test-failover-considerations). Upewnij się, że spełnia on następujące wymagania:

1. Kontroler domeny jest serwerem wykazu globalnego
2. Kontroler domeny powinien być właścicielem roli FSMO dla ról, które będą potrzebne podczas testowania trybu failover (else te role musi być [zajęte](http://aka.ms/ad_seize_fsmo) po przejściu w tryb failover)

### <a name="configure-virtual-machine-network-settings"></a>Konfigurowanie ustawień sieci maszyny wirtualnej
Dla maszyny wirtualnej DNS kontrolera domeny należy skonfigurować ustawienia sieci w ustawieniach obliczeń i sieci maszyny wirtualnej zreplikowanej w usłudze Site Recovery. To jest dołączony do prawej sieci po przełączeniu maszyny wirtualnej.

## <a name="protect-active-directory-with-active-directory-replication"></a>Ochrona usługi Active Directory z replikacji usługi Active Directory
### <a name="site-to-site-protection"></a>Ochrona lokacja lokacja
Tworzenie kontrolera domeny w lokacji dodatkowej. Podczas podwyższania poziomu serwera do roli kontrolera domeny, określ nazwę tej samej domeny, który jest używany w lokacji głównej. Można użyć **Lokacje usługi Active Directory i usługi** przystawki można skonfigurować ustawienia na obiekt łącza lokacji, do której są dodawane witryny. Konfigurując ustawienia dla łącza lokacji, można kontrolować, kiedy zachodzi replikacja między co najmniej dwóch lokacji i jak często. Aby uzyskać więcej informacji, zobacz [planowanie replikacji między lokacjami](https://technet.microsoft.com/library/cc731862.aspx).

### <a name="site-to-azure-protection"></a>Ochrona lokacji do platformy Azure
Postępuj zgodnie z instrukcjami, aby [utworzyć kontroler domeny w sieci wirtualnej platformy Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Podczas podwyższania poziomu serwera do roli kontrolera domeny, określ tej samej nazwy domeny, który jest używany w lokacji głównej.

Następnie [zmienić konfigurację serwera DNS dla sieci wirtualnej](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), aby użyć serwera DNS platformy Azure.

![Azure Network](./media/site-recovery-active-directory/azure-network.png)

### <a name="azure-to-azure-protection"></a>Ochrona Azure do platformy Azure
Postępuj zgodnie z instrukcjami, aby [utworzyć kontroler domeny w sieci wirtualnej platformy Azure](../active-directory/active-directory-install-replica-active-directory-domain-controller.md). Podczas podwyższania poziomu serwera do roli kontrolera domeny, określ tej samej nazwy domeny, który jest używany w lokacji głównej.

Następnie [zmienić konfigurację serwera DNS dla sieci wirtualnej](../active-directory/active-directory-install-replica-active-directory-domain-controller.md#reconfigure-dns-server-for-the-virtual-network), aby użyć serwera DNS platformy Azure.

## <a name="test-failover-considerations"></a>Zagadnienia dotyczące trybu failover testu
Test pracy awaryjnej w sieci, która jest odizolowana od sieci produkcyjnej, aby uniknąć wpływu na obciążeń produkcyjnych.

Większość aplikacji również wymaga obecności kontrolera domeny i serwer DNS do funkcji. W związku z tym zanim aplikacja przeszła w tryb failover, kontroler domeny musi zostać utworzona w sieci izolowanej służący do testowania trybu failover. Najprostszym sposobem, w tym celu jest replikowanie maszyny wirtualnej DNS kontrolera domeny z usługą Site Recovery. Następnie uruchom test trybu failover maszyny wirtualnej kontrolera domeny, przed uruchomieniem testowy tryb failover planu odzyskiwania dla aplikacji. Oto jak to zrobić:

1. [Replikowanie](site-recovery-replicate-vmware-to-azure.md) maszyny wirtualnej DNS kontrolera domeny za pomocą usługi Site Recovery.
2. Utwórz izolowaną sieć. Żadnej sieci wirtualnej utworzona na platformie Azure, domyślnie jest odizolowana od innych sieci. Zaleca się, że zakres adresów IP dla tej sieci jest taka sama jak w sieci produkcyjnej. Nie włączaj połączenie lokacja lokacja w tej sieci.
3. Podaj adres IP serwera DNS w sieci, utworzony jako oczekiwanego maszyny wirtualnej DNS, aby uzyskać adres IP. Jeśli przeprowadzasz replikację do platformy Azure, należy podać adres IP dla maszyny Wirtualnej, który jest używany w tryb failover w **docelowy adres IP** w **obliczenia i sieć** ustawienia zreplikowanej maszyny wirtualnej.

    ![Sieci testowej Azure](./media/site-recovery-active-directory/azure-test-network.png)

> [!TIP]
> Usługa Site Recovery próbuje utworzyć testowych maszyn wirtualnych w podsieci w tej samej nazwy i przy użyciu tego samego adresu IP, jak te zawarte w **obliczenia i sieć** ustawienia maszyny wirtualnej. Jeśli podsieć o tej samej nazwie nie jest dostępny w sieci wirtualnej platformy Azure, pod warunkiem do testowania trybu failover, testowej maszyny wirtualnej jest tworzona w pierwszej podsieci alfabetycznie. Jeśli docelowy adres IP należy do wybranej podsieci, Usługa Site Recovery próbuje je utworzyć testowej maszyny wirtualnej trybu failover przy użyciu docelowy adres IP. Jeśli docelowy adres IP nie jest częścią wybranej podsieci, testowej trybu failover maszyny wirtualnej pobiera tworzone przy użyciu dalej IP dostępne w wybranej podsieci.
>
>


1. Jeśli przeprowadzasz replikację do innej lokacji lokalnej i korzystania z protokołu DHCP, postępuj zgodnie z instrukcjami, aby [ustawienia DNS i DHCP do testowania trybu failover](site-recovery-test-failover-vmm-to-vmm.md#prepare-dhcp)
2. Wykonaj test trybu failover maszyny wirtualnej kontrolera domeny, uruchom w sieci izolowanej. Użyj najnowszych dostępnych **aplikacji spójne** punktu odzyskiwania maszyny wirtualnej kontrolera domeny do testowania trybu failover.
3. Uruchom test trybu failover planu odzyskiwania, który zawiera maszyny wirtualne w aplikacji.
4. Po zakończeniu testowania, **oczyszczania testowy tryb failover** na maszynie wirtualnej kontrolera domeny. Ten krok powoduje usunięcie kontrolera domeny, który został utworzony do testowania trybu failover.


### <a name="removing-reference-to-other-domain-controllers"></a>Usunięcie odwołania do innych kontrolerów domeny
Podczas wykonywania testowy tryb failover nie Przełącz wszystkie kontrolery domeny w sieci testowej. Aby usunąć odwołanie do innych kontrolerów domeny, które istnieją w środowisku produkcyjnym, konieczne może być [przejmowanie ról FSMO usługi Active Directory](http://aka.ms/ad_seize_fsmo) i [czyszczenie metadanych](https://technet.microsoft.com/library/cc816907.aspx) brakujących kontrolerów domeny.



> [!IMPORTANT]
> Niektóre z opisanych w poniższej sekcji konfiguracji nie są standardowe/domyślne konfiguracje kontrolera domeny. Jeśli nie chcesz wprowadzić te zmiany do poziomu kontrolera domeny w środowisku produkcyjnym, następnie można utworzyć kontroler domeny dedykowanych można użyć usługi Site Recovery testu pracy w trybie Failover i wprowadzić te zmiany w tym.  
>
>

### <a name="issues-because-of-virtualization-safeguards"></a>Problemy z powodu zabezpieczenia wirtualizacji

Począwszy od systemu Windows Server 2012, [w usługach domenowych w usłudze Active Directory są wbudowane dodatkowe zabezpieczenia](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100). Te zabezpieczenia chronić zwirtualizowanych kontrolerów domeny przed wycofywania numerów USN, jak długo podstawowej platformy funkcji hypervisor obsługuje identyfikator generacji maszyny Wirtualnej. Azure obsługuje identyfikator VM-GenerationID, co oznacza, że kontrolery domeny z systemem Windows Server 2012 lub nowszego Azure maszyny wirtualne mają dodatkowe zabezpieczenia.


Podczas generacji maszyny Wirtualnej zostanie zresetowana, bazy danych usług AD DS atrybut invocationID jest również zresetować puli identyfikatorów RID jest odrzucany i folderu SYSVOL jest oznaczony jako nieautorytatywny. Aby uzyskać więcej informacji, zobacz [wprowadzenie do wirtualizacji usług domenowych w usłudze Active Directory](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/introduction-to-active-directory-domain-services-ad-ds-virtualization-level-100) i [bezpieczna wirtualizacja usługi DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/)

Awarii na platformie Azure może spowodować zresetowanie identyfikatora VM-generationid i uruchamiane w dodatkowe zabezpieczenia podczas uruchamiania maszyny wirtualnej kontrolera domeny na platformie Azure. Może to spowodować **znaczne opóźnienie** w użytkownika jest zalogowanie się do maszyny wirtualnej kontrolera domeny. Ponieważ ten kontroler domeny może być używana tylko w test trybu failover, zabezpieczenia wirtualizacji nie jest konieczne. Aby upewnić się, nie powoduje zmiany identyfikatora generacji maszyny Wirtualnej dla maszyny wirtualnej kontrolera domeny, a następnie można zmienić wartość DWORD następujące 4 na lokalnym kontrolerze domeny.


        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\gencounter\Start


#### <a name="symptoms-of-virtualization-safeguards"></a>Objawy zabezpieczenia wirtualizacji

Zabezpieczenia wirtualizacji ma kopać przejściu w tryb failover testu, może wystąpić jeden lub więcej z następujących objawów:  

Zmiany Identyfikatora generacji

![Zmiany Identyfikatora generacji](./media/site-recovery-active-directory/Event2170.png)

Zmiany Identyfikatora wywołania

![Zmiany Identyfikatora wywołania](./media/site-recovery-active-directory/Event1109.png)

Udział Sysvol i Netlogon nie są dostępne

![Udział Sysvol](./media/site-recovery-active-directory/sysvolshare.png)

![NTFRS Sysvol](./media/site-recovery-active-directory/Event13565.png)

Żadnych baz danych DFSR są usuwane.

![Usuwanie bazy danych DFSR](./media/site-recovery-active-directory/Event2208.png)


> [!IMPORTANT]
> Niektóre z opisanych w poniższej sekcji konfiguracji nie są standardowe/domyślne konfiguracje kontrolera domeny. Jeśli nie chcesz wprowadzić te zmiany do poziomu kontrolera domeny w środowisku produkcyjnym, następnie można utworzyć kontroler domeny dedykowanych można użyć usługi Site Recovery testu pracy w trybie Failover i wprowadzić te zmiany w tym.  
>
>


### <a name="troubleshooting-domain-controller-issues-during-test-failover"></a>Rozwiązywanie problemów kontrolera domeny podczas testowania trybu failover


W wierszu polecenia Uruchom następujące polecenie, aby sprawdzić, czy folder SYSVOL i usługa NETLOGON foldery są udostępnione:

    NET SHARE

W wierszu polecenia Uruchom następujące polecenie, aby upewnić się, że kontroler domeny działa poprawnie.

    dcdiag /v > dcdiag.txt

Wyszukaj następujący tekst, aby potwierdzić, że kontroler domeny działa również w dzienniku danych wyjściowych.

* "przekazany test łączności"
* "anonsowanie przekazany test"
* "test przekazany MachineAccount"

Jeśli powyższe warunki są spełnione, istnieje prawdopodobieństwo, że kontroler domeny działa również. Jeśli nie, spróbuj wykonać następujące kroki.


* Czy przywracania autorytatywnego kontrolera domeny.
    * Chociaż jest [nie jest zalecane używanie replikacji usługi FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs/), ale jeśli nadal używają go, wykonaj kroki podane [tutaj](https://support.microsoft.com/kb/290762) celu przywracania autorytatywnego. Możesz przeczytać więcej informacji na temat Burflags omówiono w poprzedniej konsolidacji [tutaj](https://blogs.technet.microsoft.com/janelewis/2006/09/18/d2-and-d4-what-is-it-for/).
    * Jeśli używasz replikacji DFSR, a następnie wykonaj kroki dostępne [tutaj](https://support.microsoft.com/kb/2218556) celu przywracania autorytatywnego. Umożliwia także dostępne funkcje programu Powershell na tym [łącze](https://blogs.technet.microsoft.com/thbouche/2013/08/28/dfsr-sysvol-authoritative-non-authoritative-restore-powershell-functions/) w tym celu.

* Obejście wymogu pierwszej synchronizacji przez ustawienie po klucz rejestru na 0 w lokalnego kontrolera domeny. Jeśli ta DWORD nie istnieje, następnie należy go utworzyć w węźle "Parameters". Więcej informacji na ten temat [tutaj](https://support.microsoft.com/kb/2001093)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Parameters\Repl Perform Initial Synchronizations

* Wyłączyć wymaganie, że serwer wykazu globalnego jest dostępny do sprawdzania poprawności logowania użytkownika przez ustawienie po klucz rejestru do 1 na lokalnym kontrolerze domeny. Jeśli to DWORD nie istnieje, następnie należy go utworzyć w węźle "Lsa". Więcej informacji na ten temat [tutaj](http://support.microsoft.com/kb/241789)

        HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\IgnoreGCFailures



### <a name="dns-and-domain-controller-on-different-machines"></a>DNS i kontrolera domeny na kilka różnych maszyn
DNS nie znajduje się w tej samej maszyny wirtualnej jako kontroler domeny, należy utworzyć Maszynę wirtualną DNS do testowania trybu failover. Jeśli są w tej samej maszyny Wirtualnej, możesz pominąć tę sekcję.

Można użyć nowego serwera DNS i utworzyć wymagane strefy. Na przykład domena usługi Active Directory to contoso.com, można utworzyć strefę DNS o nazwie contoso.com. Wpisy odpowiadające usługi Active Directory musi zostać zaktualizowane w systemie DNS, w następujący sposób:

1. Upewnij się, że te ustawienia są stosowane przed funkcjonuje inne maszyny wirtualne w planie odzyskiwania:

   * Strefa musi nosi nazwę katalogu głównego lasu.
   * Strefa musi być kopią zapasową w pliku.
   * Strefa musi być włączona bezpieczny i niezabezpieczone aktualizacje.
   * Mechanizm rozpoznawania maszyny wirtualnej kontrolera domeny powinny wskazywać adres IP DNS maszyny wirtualnej.
2. Na maszynie wirtualnej kontrolera domeny, uruchom następujące polecenie:

    `nltest /dsregdns`
3. Dodawanie strefy na serwerze DNS, Zezwalaj na aktualizacje niezabezpieczonego i Dodaj wpis dla niego w systemie DNS:

        dnscmd /zoneadd contoso.com  /Primary
        dnscmd /recordadd contoso.com  contoso.com. SOA %computername%.contoso.com. hostmaster. 1 15 10 1 1
        dnscmd /recordadd contoso.com %computername%  A <IP_OF_DNS_VM>
        dnscmd /config contoso.com /allowupdate 1

## <a name="next-steps"></a>Następne kroki
[Dowiedz się więcej](site-recovery-workload.md) o ochronie obciążeń przedsiębiorstwa z usługą Azure Site Recovery.
