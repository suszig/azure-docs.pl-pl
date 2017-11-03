---
title: "Migrowanie maszyn wirtualnych z usług AWS na platformie Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób migracji maszyn wirtualnych uruchomionych w Amazon Web Services (AWS) na platformie Azure, używając usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 6a10cbf1d3c8043ddcac9a725373fc30a3ecee20
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="migrate-amazon-web-services-aws-vms-to-azure"></a>Migrowanie maszyn wirtualnych (AWS) usługi sieci Web firmy Amazon na platformie Azure

W tym samouczku jest przedstawienie sposobu migracji Amazon Web Services (AWS) maszyn wirtualnych (VM) na maszynach wirtualnych platformy Azure przy użyciu usługi Site Recovery. Podczas migrowania EC2 wystąpień na platformie Azure, VMsare traktowany tak, jakby były fizycznych, komputerami lokalnymi. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Przygotowanie zasobów platformy Azure
> * Przygotowanie do migracji wystąpień usług AWS EC2
> * Wdrożenie serwera konfiguracji
> * Włącz replikację dla maszyn wirtualnych
> * Test pracy awaryjnej, aby upewnić się, wszystko działa
> * Uruchom jednorazowe trybu failover na platformie Azure

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).


## <a name="prepare-azure-resources"></a>Przygotowanie zasobów platformy Azure 

Musisz mieć kilka gotowy na platformie Azure dla migrowanych wystąpień EC2 do użycia zasobów. Obejmują one konta magazynu, magazynu i sieci wirtualnej.

### <a name="create-a-storage-account"></a>Tworzenie konta magazynu

Obrazy maszyn replikowanych są przechowywane w magazynie Azure. Maszyny wirtualne platformy Azure są tworzone z magazynu podczas awaryjne z lokalnego do platformy Azure.

1. W [portalu Azure](https://portal.azure.com) menu, kliknij przycisk **nowy** -> **magazynu** -> **konta magazynu**.
2. Wprowadź nazwę konta magazynu. Te samouczki używamy nazwy **awsmigrated2017**. Nazwa musi być unikatowa w obrębie platformy Azure i należeć do zakresu od 3 do 24 znaków tylko cyfry i małe litery.
3. Zachowaj wartości domyślne dla **modelu wdrażania**, **konta rodzaju**, **wydajności**, i **bezpieczny transfer wymagane**.
5. Wybierz domyślną **RA-GRS** dla **replikacji**.
6. Wybierz subskrypcję, której chcesz użyć na potrzeby tego samouczka.
7. Aby uzyskać **grupy zasobów**, wybierz pozycję **Utwórz nowy**. W tym przykładzie używamy **migrationRG** jako nazwy.
8. Wybierz **Europa** jako lokalizacji. 
9. Kliknij pozycję **Utwórz**, aby utworzyć konto magazynu.

### <a name="create-a-vault"></a>Tworzenie magazynu

1. W [portalu Azure](https://portal.azure.com), na lewym pasku nawigacyjnym kliknij **więcej usług** i wyszukaj i wybierz **Magazyny usług odzyskiwania**.
2. Na stronie Magazyny usług odzyskiwania kliknij przycisk **+ Dodaj** w lewym górnym rogu strony.
3. Aby uzyskać **nazwa**, typ *myVault*. 
4. Aby uzyskać **subskrypcji**, wybierz odpowiednią subskrypcję.
4. Aby uzyskać **grupy zasobów**, wybierz pozycję **Użyj istniejącego** i wybierz *migrationRG*. 
5. W **lokalizacji**, wybierz pozycję *Europa*. 
5. Aby szybko uzyskać dostęp do nowego magazynu, z poziomu pulpitu nawigacyjnego, wybierz **Przypnij do pulpitu nawigacyjnego**.
7. Gdy wszystko będzie gotowe, kliknij przycisk **Utwórz**.

Nowy magazyn jest wyświetlany na **pulpitu nawigacyjnego** > **wszystkie zasoby**i w głównym **Magazyny usług odzyskiwania** strony.

### <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure

Podczas tworzenia maszyn wirtualnych Azure po zakończeniu migracji (failover), są one przyłączone do tej sieci.

1. W [portalu Azure](https://portal.azure.com), kliknij przycisk **nowy** > **sieci** >
    **sieci wirtualnej**
3. Aby uzyskać **nazwa**, typ *myMigrationNetwork*.
4. Pozostaw wartość domyślną dla **przestrzeni adresów**.
5. Aby uzyskać **subskrypcji**, wybierz odpowiednią subskrypcję.
6. Dla **grupy zasobów**, wybierz pozycję **Użyj istniejącego** i wybierz polecenie *migrationRG* z listy rozwijanej.
7. Aby uzyskać **lokalizacji**, wybierz pozycję **Europa**. 
8. Pozostaw wartości domyślne dla **podsieci**, oba **nazwa** i **zakres adresów IP**.
9. Pozostaw **punktów końcowych usługi** wyłączone.
10. Gdy wszystko będzie gotowe, kliknij przycisk **Utwórz**.


## <a name="prepare-the-ec2-instances"></a>Przygotowanie wystąpień EC2

Należy przynajmniej jednej maszyny wirtualnej, które chcesz migrować. Te wystąpienia EC2 powinna działać 64-bitowej wersji systemu Windows Server 2008 R2 z dodatkiem SP1 lub nowszy, system Windows Server 2012, Windows Server 2012 R2 lub Red Hat Enterprise Linux 6.7 (tylko w przypadku wystąpienia HVM zwirtualizowanych). Serwer musi mieć tylko Citrix PV lub wa usług AWS sterowniki. Instancje systemem RedHat PV sterowniki nie są obsługiwane.

Musi być zainstalowana usługa mobilności na każdej maszynie Wirtualnej, którą chcesz replikować. Usługa Site Recovery automatycznie instaluje tej usługi, po włączeniu replikacji dla maszyny Wirtualnej. Automatyczną instalację należy przygotować konta w wystąpieniach EC2, które uzyskują dostęp maszyny Wirtualnej odzyskiwania lokacji.

Można użyć domeny lub konta lokalnego. Dla maszyn wirtualnych systemu Linux konto powinno być głównym urzędem certyfikacji na serwer źródłowy z systemem Linux. Dla maszyn wirtualnych systemu Windows, jeśli nie używasz konta domeny, wyłącz kontroli dostępu użytkownika zdalnego na komputerze lokalnym:

  - W rejestrze w obszarze **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, Dodaj wpis DWORD **LocalAccountTokenFilterPolicy** i ustaw wartość na 1.
    
Należy również oddzielnego wystąpienia EC2, która służy jako serwer konfiguracji usługi Site Recovery. To wystąpienie musi działać system Windows Server 2012 R2. 
    

## <a name="prepare-the-infrastructure"></a>Przygotowanie infrastruktury 

Na stronie portalu dla magazynu, wybierz **usługi Site Recovery** z **wprowadzenie** sekcji, a następnie kliknij przycisk **przygotowanie infrastruktury**.

### <a name="1-protection-goal"></a>Cel ochrony 1

Wybierz następujące wartości na **cel ochrony** strony:
   
|    |  | 
|---------|-----------|
| Gdzie znajdują się komputery? | **Lokalne**|
| Gdzie chcesz zreplikować maszyny? |**Na platformie Azure**|
| Czy maszyny są zwirtualizowane? | **Nie zwirtualizowanych / innych**|

Gdy wszystko będzie gotowe, kliknij przycisk **OK** aby przejść do następnej sekcji.

### <a name="2-source-prepare"></a>Przygotowanie źródłowego 2 

Na **Przygotuj źródło** kliknij przycisk **+ serwer konfiguracji**. 

1. Wystąpienie EC2 z systemem Windows Server 2012 R2 umożliwia tworzenie serwera konfiguracji i zarejestrowanie go za pomocą magazynu odzyskiwania.

2. Skonfiguruj serwer proxy w wystąpieniu EC2 maszyny Wirtualnej jest używany jako serwer konfiguracji, dzięki czemu mogą uzyskać dostęp [adresy URL usługi](site-recovery-support-matrix-to-azure.md).

3. Pobierz [Unified Instalatora Microsoft Azure Site Recovery](http://aka.ms/unifiedinstaller_wus) program. Możesz pobrać go na komputerze lokalnym i skopiuj go za pośrednictwem do maszyny Wirtualnej jest używany jako serwer konfiguracji.

4. Polecenie **Pobierz** przycisk, aby pobrać klucz rejestracji magazynu. Skopiuj pobrany plik nad do maszyny Wirtualnej jest używany jako serwer konfiguracji.

5. Na Maszynie wirtualnej, kliknij prawym przyciskiem myszy Instalator pobrany dla **Unified Instalatora Microsoft Azure Site Recovery** i wybierz **Uruchom jako administrator**. 

    1. W **przed rozpoczęciem**, wybierz pozycję **zainstalować serwer konfiguracji i serwera przetwarzania** , a następnie kliknij przycisk **dalej**.
    2. W **licencji na oprogramowanie innych firm**, wybierz pozycję **akceptuję umowę licencyjną innej firmy.** a następnie kliknij przycisk **dalej**.
    3. W **rejestracji**, kliknij przycisk Przeglądaj i przejdź do, gdzie umieścić plik klucza rejestracji magazynu, a następnie kliknij przycisk **dalej**.
    4. W **internetowe**, wybierz pozycję **nawiązywanie połączenia z usługi Azure Site Recovery bez serwera proxy.** a następnie kliknij przycisk **dalej**.
    5. W **Sprawdzanie wymagań wstępnych** strony, uruchamia sprawdza, czy kilka elementów. Po zakończeniu kliknij przycisk **dalej**. 
    6. W **konfiguracji MySQL**, podaj wymagane hasła, a następnie kliknij przycisk **dalej**.
    7. W **szczegóły środowiska**, wybierz pozycję **nr**, nie trzeba chronić maszyn VMware, a następnie kliknij przycisk **dalej**.
    8. W **zainstalować lokalizacji**, kliknij przycisk **dalej** aby zaakceptować ustawienie domyślne.
    9. W **wybór sieci**, kliknij przycisk **dalej** aby zaakceptować ustawienie domyślne.
    10. W **Podsumowanie** kliknij **zainstalować**.
    11. **Postęp instalacji** pokazuje informacje dotyczące skutkującej w procesie instalacji. Po zakończeniu kliknij przycisk **Zakończ**. Uzyskać wyświetla komunikat o konieczności możliwe ponowne uruchomienie komputera, kliknij przycisk **OK**. Możesz również pobrać okno podręczne o hasło połączenia serwera konfiguracji, Kopiuj hasło do Schowka i zapisz go w bezpiecznym miejscu.
    
6. Na Maszynie wirtualnej, należy uruchomić **cspsconfigtool.exe** można utworzyć co najmniej jedno konto zarządzania na serwerze konfiguracji. Upewnij się, że konta zarządzania mają uprawnienia administratora w wystąpieniach EC2, które chcesz migrować. 

Po zakończeniu konfigurowania serwera konfiguracji, wróć do portalu i wybierz serwer, który został utworzony dla **serwera konfiguracji** i kliknij przycisk *OK** aby przejść do kroku 3 Przygotowanie docelowej.

### <a name="3-target-prepare"></a>Przygotowanie docelowy 3 

W tej sekcji możesz wprowadzić informacje dotyczące zasobów, które zostały utworzone w trakcie poszło za pośrednictwem [zasobów Azure przygotowanie](#prepare-azure-resources) sekcji we wcześniejszej części tego samouczka.

1. W **subskrypcji**, wybierz subskrypcję platformy Azure, który był używany przez [przygotowanie Azure](tutorial-prepare-azure.md) samouczka.
2. Wybierz pozycję **Resource Manager** jako model wdrażania.
3. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure. Powinny być zasoby utworzone po przejściu [zasobów Azure przygotowanie](#prepare-azure-resources) sekcji we wcześniejszej części tego samouczka
4. Gdy skończysz, kliknij przycisk **OK**.


### <a name="4-replication-settings-prepare"></a>Ustawienia replikacji 4 przygotowanie 

Należy utworzyć zasady replikacji przed włączeniem replikacji

1. Kliknij przycisk **+ replikacji i skojarz**.
2. W **nazwa**, typ **myReplicationPolicy**.
3. Pozostaw pozostałe ustawienia domyślne i kliknij przycisk **OK** do tworzenia zasad. Nowe zasady jest automatycznie kojarzony z serwerem konfiguracji. 

### <a name="5-deployment-planning-select"></a>5 wdrażania planowania wybierz 

W **czy ukończono Planowanie wdrożenia?**, wybierz pozycję **zrobię to później** z listy rozwijanej, a następnie kliknij przycisk **OK**.

Gdy wszystkie wykonaniu wszystkie sekcje 5 **przygotowanie infrastruktury**, kliknij przycisk **OK**.


## <a name="enable-replication"></a>Włączanie replikacji

Włącz replikację każdej maszyny Wirtualnej, które chcesz migrować. Po włączeniu replikacji usługi Site Recovery automatycznie instaluje usługę mobilności. 

1. Otwórz [portal Azure](htts://portal.azure.com).
1. Na stronie dla magazynu w obszarze **wprowadzenie**, kliknij przycisk **usługi Site Recovery**.
2. W obszarze **dla lokalnych maszyn i maszyny wirtualne Azure**, kliknij przycisk **krok aplikacji 1:Replicate**. Zakończenie strony kreatora o następujące informacje i kliknij przycisk **OK** na każdej stronie po zakończeniu:
    - Skonfiguruj źródło 1:
      
    |  |  |
    |-----|-----|
    | Źródło: | **Lokalnie**|
    | Lokalizacja źródłowa:| Nazwa wystąpienia serwera EC2 konfiguracji.|
    |Typ urządzenia: | **Maszyny fizyczne**|
    | Serwer przetwarzania: | Wybierz serwer konfiguracji z listy rozwijanej.|
    
    - Konfigurowanie docelowej 2
        
    |  |  |
    |-----|-----|
    | Element docelowy: | Pozostaw wartość domyślną.|
    | Subskrypcji: | Wybierz subskrypcję, który był używany.|
    | Grupa zasobów pracy awaryjnej POST:| Użyj grupy zasobów utworzonej w [zasobów Azure przygotowanie](#prepare-azure-resources) sekcji.|
    | Model wdrożenia trybu failover POST: | Wybierz **Menedżera zasobów**|
    | Konto magazynu: | Wybierz konto magazynu, utworzonego w [zasobów Azure przygotowanie](#prepare-azure-resources) sekcji.|
    | Sieć platformy Azure: | Wybierz **Konfiguruj teraz dla wybranych maszyn**|
    | Po pracy w trybie failover sieć platformy Azure: | Wybierz sieć, utworzony w [zasobów Azure przygotowanie](#prepare-azure-resources) sekcji.|
    | Podsieć: | Wybierz **domyślne** z listy rozwijanej.|
    
    - Wybierz 3 maszyn fizycznych
        
        Kliknij przycisk **+ komputera fizycznego** , a następnie wprowadź **nazwa**, **adres IP** i **typ systemu operacyjnego** wystąpienia EC2, które chcesz przeprowadzić migrację i następnie kliknij przycisk **OK**.
        
    - Właściwości 4 Konfiguruj właściwości
        
        Wybierz konto, które utworzono na serwerze konfiguracji z listy rozwijanej, a następnie kliknij przycisk **OK**.
        
    - Ustawienia replikacji Konfigurowanie ustawień replikacji 5
    
        Upewnij się, że jest wybrane w polu listy rozwijanej zasady replikacji **myReplicationPolicy** , a następnie kliknij przycisk **OK**.
        
3. Po ukończeniu pracy kreatora, kliknij przycisk **włączyć replikację**.
        

Możesz śledzić postęp **Włącz ochronę** zadania w **monitorowanie i raporty** > **zadania** > **zadania usługi Site Recovery** . Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.        
        
Po włączeniu replikacji dla maszyny Wirtualnej może zająć 15 minut ani już aby zmiany zaczęły efektu i wyświetlane w portalu.

## <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover

Po uruchomieniu test trybu failover, wykonywane są następujące czynności:

1. Używa do upewnij się, że wszystkie warunki wymagane do trybu failover znajdują się w miejscu Sprawdzanie wymagań wstępnych.
2. Trybu failover przetwarza dane, dzięki czemu można utworzyć maszyny Wirtualnej platformy Azure. Jeśli wybierz najnowszy punkt odzyskiwania, punkt odzyskiwania jest tworzony z danych.
3. Maszyny Wirtualnej platformy Azure jest tworzony przy użyciu danych przetworzonych w poprzednim kroku.

W portalu, uruchom test trybu failover w następujący sposób:

1. Na stronie dla magazynu, przejdź do **chronione elementy** > **elementy replikowane**> kliknij maszynę Wirtualną > **+ testowy tryb Failover**.

2. Wybierz punkt odzyskiwania do użycia dla trybu failover:
    - **Najnowsze przetworzone**: awaryjnie maszyny Wirtualnej do ostatniego punktu odzyskiwania, który był przetwarzany przez usługę Site Recovery. Sygnatura czasowa jest wyświetlany. Ta opcja nie jest czas przetwarzania danych, dzięki czemu oferuje RTO niski (celu czasu odzyskiwania).
    - **Najnowsza wersja aplikacji spójne**: Ta opcja nie powiedzie się za pośrednictwem wszystkich maszyn wirtualnych do ostatniego punktu odzyskiwania zapewniających spójność aplikacji. Sygnatura czasowa jest wyświetlany.
    - **Niestandardowe**: wybrać dowolny punkt odzyskiwania.
3. W **testowy tryb Failover**, wybierz element docelowy Azure sieci, na którą maszyny wirtualne Azure zostaną połączone po pracy awaryjnej. To pole powinno być utworzone w sieci [zasobów Azure przygotowanie](#prepare-azure-resources) sekcji.
4. Kliknij przycisk **OK**, aby rozpocząć tryb failover. Możesz śledzić postępy, klikając na maszynie Wirtualnej, aby otworzyć jego właściwości. Można też kliknąć przycisk **testowy tryb Failover** zadania na stronie dla magazynu w **monitorowanie i raporty** > **zadania** >
   **lokacji Zadania odzyskiwania**.
5. Po zakończeniu pracy awaryjnej, replika maszyny Wirtualnej platformy Azure jest wyświetlana w portalu Azure > **maszyn wirtualnych**. Sprawdź, czy maszyna wirtualna jest odpowiedni rozmiar, który został połączony z siecią prawo, i że jest uruchomiona.
6. Teraz można nawiązać połączenia z zreplikowanej maszyny Wirtualnej na platformie Azure.
7. Aby usunąć utworzony podczas testowania trybu failover maszyny wirtualne platformy Azure, kliknij przycisk **oczyszczania testowy tryb failover** dla planu odzyskiwania. W **uwagi**, zarejestrować i zapisać wszelkie obserwacje związane z testowania trybu failover.

W niektórych scenariuszach pracy awaryjnej wymaga dodatkowego przetwarzania pobierającej około 8-10 minut, aby zakończyć. 


## <a name="migrate-to-azure"></a>Migracja na platformę Azure

Uruchom rzeczywistych wystąpień EC2 w celu przeprowadzenia migracji do maszyn wirtualnych platformy Azure w trybie failover. 

1. W **chronione elementy** > **elementy replikowane** wystąpień usług AWS kliknij > **pracy awaryjnej**.
2. W **pracy awaryjnej** wybierz **punkt odzyskiwania** do pracy w trybie failover. Wybierz najnowszy punkt odzyskiwania.
3. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej** Jeśli chcesz, aby usługi Site Recovery, aby spróbować wykonać zamykania maszyn wirtualnych źródła przed wyzwolenie pracy awaryjnej. Tryb failover trwa nawet w przypadku zamknięcia nie powiedzie się. Możesz śledzić postęp trybu failover **zadania** strony.
4. Sprawdź, czy maszyna wirtualna jest wyświetlana w **elementy replikowane**. 
5. Kliknij prawym przyciskiem myszy każdej maszyny Wirtualnej > **ukończenia migracji**. To kończy proces migracji, zatrzymania replikacji dla maszyny Wirtualnej usług AWS i zatrzymuje rozliczeń usługi Site Recovery dla maszyny Wirtualnej.

    ![Kończenie migracji](./media/tutorial-migrate-aws-to-azure/complete-migration.png)

> [!WARNING]
> **Nie Anuluj trybu failover w toku**: przed uruchomieniem trybu failover zostanie zatrzymana replikację maszyny Wirtualnej. Jeśli anulujesz trybu failover w toku, zatrzymuje trybu failover, ale maszyna wirtualna nie będą replikowane ponownie.  


    

## <a name="next-steps"></a>Następne kroki

W tym temacie kiedy znasz już jak przeprowadzić migrację wystąpienia usług AWS EC2 maszynach wirtualnych platformy Azure. Aby dowiedzieć się więcej o maszynach wirtualnych platformy Azure, nadal samouczków dla maszyn wirtualnych systemu Windows.

> [!div class="nextstepaction"]
> [Samouczki dla maszyny wirtualnej platformy Azure z systemem Windows](../virtual-machines/windows/tutorial-manage-vm.md)
