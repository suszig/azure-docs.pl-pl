---
title: "Tryb failover, a kończyć się niepowodzeniem z powrotem maszyn wirtualnych funkcji Hyper-V replikowany do platformy Azure z usługą Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak działają maszyny wirtualne funkcji Hyper-V do platformy Azure i powrót po awarii do lokacji lokalnej, z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72065d01-ed0f-430e-b117-a5885cecd1db
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/15/2017
ms.author: raynew
ms.openlocfilehash: e1cc21661450a983c25b24fe2a6228e26ceecec6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="fail-over-and-fail-back-hyper-v-vms-replicated-to-azure"></a>Tryb failover, a kończyć się niepowodzeniem z powrotem maszyn wirtualnych funkcji Hyper-V zreplikowanej w systemie Azure

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi zarządza i organizuje replikację, tryb failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM).

W tym samouczku opisano w tryb failover maszyny Wirtualnej funkcji Hyper-V do platformy Azure. Po przez Ciebie nie powiodła się, możesz powrót po awarii do lokacji sieci lokalnej, gdy jest ona dostępna. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tryb failover maszyn wirtualnych funkcji Hyper-V z lokalnego do platformy Azure
> * Nie powiodło się platformy Azure z lokalnymi, a następnie uruchom ponownie replikację do platformy Azure

## <a name="overview"></a>Omówienie
Tryb failover i powrotu po awarii ma dwa etapy:

1. **Awaryjnie Azure**: awaryjnie maszyny z lokacji lokalnej na platformie Azure.
2. **Nie powiodło się platformy Azure z lokalnymi**: Uruchom planowanego trybu failover z platformy Azure do środowiska lokalnego. Po planowanego trybu failover możesz włączyć replikację odwrotną rozpocząć replikację z lokalnego do platformy Azure ponownie. 


## <a name="fail-over-to-azure"></a>Przełączyć na platformie Azure

### <a name="failover-prerequisites"></a>Wymagania wstępne dotyczące trybu failover

Do ukończenia pracy awaryjnej:

- Upewnij się, że udało Ci się ukończyć [wyszczególniania odzyskiwania po awarii](tutorial-dr-drill-azure.md) do sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami.
- Opcjonalnie można przygotować nawiązywania połączenia z maszynami wirtualnymi Azure przed przejścia w tryb failover.
- Przed uruchomieniem trybu failover, sprawdź właściwości maszyny Wirtualnej.

#### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Przygotowanie do połączenia z maszynami wirtualnymi Azure po przejściu do trybu failover

Aby nawiązać połączenie przy użyciu protokołu RDP po pracy awaryjnej maszyn wirtualnych platformy Azure, wykonaj następujące czynności:

##### <a name="azure-vms-running-windows"></a>Maszyny wirtualne platformy Azure, systemem Windows

1. Aby uzyskać dostęp do maszyn wirtualnych platformy Azure za pośrednictwem Internetu, należy włączyć protokół RDP na lokalnej maszynie Wirtualnej przed trybu failover. Upewnij się, że TCP i UDP reguły są dodawane do **publicznego** profilu oraz że RDP jest dozwolone w **zapory systemu Windows** > **dozwolone aplikacje** we wszystkich profilach.
2. Aby uzyskać dostęp za pośrednictwem połączenia VPN lokacja lokacja, należy włączyć RDP na maszynie lokalnej. RDP powinno być dozwolone w **zapory systemu Windows** -> **dozwolone aplikacje i funkcje** dla **domeny i prywatnej** sieci. Sprawdź, czy zasady sieci SAN systemu operacyjnego są ustawione na **OnlineAll**. [Dowiedz się więcej](https://support.microsoft.com/kb/3031135). Powinien istnieć nie oczekujące aktualizacje systemu Windows na maszynie Wirtualnej, gdy użytkownik zainicjuje tryb failover. Jeśli, nie będzie można logować się do maszyny wirtualnej do momentu ukończenia aktualizacji. 
3. Na maszynie Wirtualnej Azure z systemem Windows po w tryb failover, sprawdź **diagnostyki rozruchu** Aby wyświetlić zrzut ekranu maszyny wirtualnej. Jeśli nie możesz połączyć, sprawdź, czy maszyna wirtualna działa i przejrzyj te [porady dotyczące rozwiązywania problemów](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


##### <a name="azure-vms-running-linux"></a>Maszyny wirtualne platformy Azure, systemem Linux

1. Na maszynie lokalnej przed trybu failover Sprawdź, czy Usługa Secure Shell jest ustawiony na automatyczne uruchomienie przy rozruchu systemu. Sprawdź, czy reguły zapory zezwalają na połączenie SSH.
2. Na maszynie Wirtualnej Azure po pracy awaryjnej zezwalać na połączenia przychodzące do portu SSH dla zasad grupy zabezpieczeń sieci w trybie Failover maszyny Wirtualnej i podsieci platformy Azure, do którego jest podłączony.  [Dodaj publiczny adres IP](site-recovery-monitoring-and-troubleshooting.md#adding-a-public-ip-on-a-resource-manager-virtual-machine) dla maszyny Wirtualnej. Możesz sprawdzić **diagnostyki rozruchu** Aby wyświetlić zrzut ekranu maszyny wirtualnej.


#### <a name="verify-vm-properties"></a>Sprawdź właściwości maszyny Wirtualnej

Sprawdź właściwości maszyny Wirtualnej, a następnie upewnij się, że maszyna wirtualna jest zgodna z [wymagania dotyczące usługi Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. W **chronione elementy**, kliknij przycisk **elementy replikowane** > maszyny Wirtualnej.
2. W **elementu zreplikowane** okienku znajduje się podsumowanie informacji o maszyny Wirtualnej, stan kondycji i najnowsze dostępne punkty odzyskiwania. Kliknij przycisk **właściwości** można zobaczyć więcej szczegółów.
3. W **obliczenia i sieć**, możesz zmodyfikować nazwę platformy Azure, grupy zasobów, rozmiar docelowy [zestawu dostępności](../virtual-machines/windows/tutorial-availability-sets.md), i [zarządzane ustawienia dysku](#managed-disk-considerations)
4. Można wyświetlać i modyfikować ustawień sieciowych, takich jak sieć/podsieć, w którym maszyny Wirtualnej Azure zostaną umieszczone po pracy awaryjnej i adres IP, który zostanie przypisany do niej.
5. W **dysków**, można wyświetlić informacje dotyczące systemu operacyjnego i dysków z danymi na Maszynie wirtualnej.


### <a name="run-a-failover-from-on-premises-to-azure"></a>Uruchom tryb failover z lokalnej na platformie Azure

Zwykłe i planowanego trybu failover można uruchomić na maszynach wirtualnych funkcji Hyper-V

- Użyj regularne pracy awaryjnej dla wystąpienia nieoczekiwanych awarii. Po uruchomieniu tej pracy awaryjnej usługi Site Recovery tworzy maszyny Wirtualnej platformy Azure i uprawnień go w górę. Możesz uruchomić tryb failover przed określony punkt odzyskiwania. W zależności od tego punktu odzyskiwania, którego używasz, może wystąpić utrata danych.
- Planowany tryb failover może służyć konserwacji lub podczas oczekiwanych awarii. Ta opcja umożliwia zerowej utraty danych. Po wyzwoleniu planowanego trybu failover, źródłowe maszyny wirtualne są zamknięte. Niezsynchronizowane dane są synchronizowane i trybu failover zostanie wywołany.

W tej procedurze opisano sposób uruchamiania regularne trybu failover.


1. W **ustawienia** > **elementy replikowane** kliknij maszynę Wirtualną > **pracy awaryjnej**.
2. W **pracy awaryjnej** wybierz **punkt odzyskiwania** do trybu failover. Można użyć jednej z następujących opcji:
    - **Najnowsze** (domyślnie): Ta opcja najpierw przetwarza wszystkie dane wysyłane do usługi Site Recovery. Ponieważ maszyny Wirtualnej platformy Azure utworzonych po trybu failover ma wszystkie dane, które zostało replikowane do usługi Site Recovery pracę w trybie failover zostało wyzwolone zapewnia najniższy RPO (cel punktu odzyskiwania).
    - **Najnowsze przetworzone**: Ta opcja nie powiedzie się za pośrednictwem maszyn wirtualnych do ostatniego punktu odzyskiwania przetworzone przez usługę Site Recovery. Ta opcja umożliwia RTO niski (celu czasu odzyskiwania), ponieważ nie jest czas przetwarzania nieprzetworzonych danych.
    - **Najnowsza wersja aplikacji spójne**: Ta opcja nie powiedzie się na maszynie Wirtualnej, aby punkt najnowszych odzyskiwania zapewniających spójność aplikacji przetworzone przez usługę Site Recovery. 
    - **Niestandardowe**: Określ punkt odzyskiwania.
3. Jeśli powrotu po awarii na maszynach wirtualnych funkcji Hyper-V w chmurach System Center VMM do platformy Azure, i szyfrowanie danych jest włączone dla chmury, w **klucza szyfrowania**, wybierz certyfikat, który został wystawiony, włączenie szyfrowania danych podczas instalacji dostawcy na Serwer programu VMM.
4. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej** Jeśli chcesz, aby usługi Site Recovery, aby spróbować wykonać zamykania maszyn wirtualnych źródła przed wyzwolenie pracy awaryjnej. Usługa Site Recovery również spróbować zsynchronizować danych lokalnych, które jeszcze nie został wysłany na platformie Azure, aby mogło nastąpić wyzwolenie pracy awaryjnej. Należy pamiętać, że czy tryb failover będzie kontynuowane, nawet w przypadku zamknięcia nie powiedzie się. Możesz śledzić postęp trybu failover **zadania** strony.
5. Jeśli przygotowanym połączenie z maszyną Wirtualną Azure, połącz się go zweryfikować po przejściu w tryb failover.
6. Po sprawdzeniu, **zatwierdzić** pracy awaryjnej. Spowoduje to usunięcie wszystkich punktów odzyskiwania.

> [!WARNING]
> **Nie Anuluj trybu failover w toku**: przed uruchomieniem trybu failover zostanie zatrzymana replikację maszyny Wirtualnej. Jeśli anulujesz trybu failover w toku, zatrzymuje trybu failover, ale maszyna wirtualna nie będą replikowane ponownie.  


## <a name="fail-back-from-azure-to-on-premises"></a>Nie powiodło się platformy Azure z lokalnymi

### <a name="prerequisites-for-failback"></a>Wymagania wstępne dotyczące powrotu po awarii

Aby ukończyć powrotu po awarii, upewnij się, że serwera lokacji głównej program VMM server/funkcji Hyper-V jest podłączony do usługi Site Recovery.


### <a name="run-a-failback-and-reprotect-on-premises-vms"></a>Uruchom powrotu po awarii i włącz ponownie ochronę maszyn wirtualnych lokalnie

Przełączyć się z platformy Azure do lokacji lokalnej i Rozpocznij replikację maszyn wirtualnych z lokacji lokalnej do platformy Azure.

1. W **ustawienia** > **elementy replikowane** kliknij maszynę Wirtualną > **planowanego trybu Failover**.
2. W **potwierdzić planowanego trybu Failover**Sprawdź kierunek pracy awaryjnej (na platformie Azure), a wybierz lokalizacja źródłowa i docelowa. 
3. W **synchronizacji danych**, określ, jak mają być synchronizowane:
    - **Synchronizowanie danych przed pracy awaryjnej (należy zsynchronizować tylko zmiany różnicowe)**— ta opcja minimalizuje przestojów maszyn wirtualnych, ponieważ synchronizacji bez zamykania maszyny Wirtualnej. Oto, czego:
        1. Tworzy migawkę maszyny Wirtualnej platformy Azure i kopiuje go do lokalnego hosta funkcji Hyper-V. Maszyna wirtualna przechowuje działające na platformie Azure.
        2. Zamyka maszyny Wirtualnej Azure, dzięki czemu ma nowych zmian występuje. Ostateczny zestaw zmiany różnicowe są przekazywane do serwera lokalnego, a lokalnej maszyny Wirtualnej jest uruchomiona.
    - **Synchronizacja danych w trybie failover tylko (pełnego pobierania)**— Użyj tej opcji, jeśli działała na platformie Azure przez długi czas. Ta opcja jest szybsze, ponieważ będziemy spodziewać się wielu zmian dysku, a nie poświęcić czas na obliczenia sumy kontrolnej. Ta opcja wykonuje pobierania dysku. Jest również przydatne w przypadku lokalnej maszyny Wirtualnej został usunięty.
4. Jeśli powrotu po awarii na maszynach wirtualnych funkcji Hyper-V w chmurach System Center VMM do platformy Azure, i szyfrowanie danych jest włączone dla chmury, w **klucza szyfrowania**, wybierz certyfikat, który został wystawiony, włączenie szyfrowania danych podczas instalacji dostawcy na Serwer programu VMM.
5. Inicjuj tryb failover. Możesz śledzić postęp trybu failover **zadania** kartę.
6. Jeśli został wybrany do synchronizacji danych przed przejście w tryb failover, po ukończeniu synchronizacji początkowej danych i wszystko jest gotowe do zamykania maszyn wirtualnych platformy Azure kliknij **zadania** > planowany tryb failover Nazwa zadania > **ukończenia pracy awaryjnej**. Zamyka maszyny Wirtualnej Azure, przesyła najnowsze zmiany w lokalnej instalacji programu i uruchamia lokalnej maszyny Wirtualnej.
7. Zaloguj się na lokalnej maszynie Wirtualnej, aby Sprawdź, czy jest dostępna, zgodnie z oczekiwaniami.
8. Lokalnej maszyny Wirtualnej jest teraz w stanie oczekiwania zatwierdzenia. Kliknij przycisk **zatwierdzania**, aby zatwierdzić pracy awaryjnej. To spowoduje usunięcie maszyn wirtualnych platformy Azure i jego dysków i przygotowuje lokalnej maszyny Wirtualnej do replikacji odwrotnej.
9. Aby rozpocząć replikację lokalnej maszyny Wirtualnej do platformy Azure, należy włączyć **odwrócić replikację**.


> [!NOTE]
> Replikacja odwrotna replikuje tylko zmiany, które wystąpiły, ponieważ był wyłączony w maszynie Wirtualnej platformy Azure i są wysyłane tylko zmiany różnicowe.

