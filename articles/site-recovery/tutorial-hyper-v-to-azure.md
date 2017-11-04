---
title: "Konfigurowanie odzyskiwania po awarii dla maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure z usługą Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 45e9b4dc-20ca-4c14-bee3-cadb8d9b8b24
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/18/2017
ms.author: raynew
ms.openlocfilehash: 4d43fb03ce1c54a47315b8c3a5c83ec2082bcab9
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Konfigurowanie odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do strategii odzyskiwania po awarii poprzez organizowanie replikacji, trybu failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM) i zarządzanie nimi.

W tym samouczku przedstawiono sposób konfigurowania odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure. Samouczek ma zastosowanie w przypadku maszyn wirtualnych funkcji Hyper-V, które są zarządzane w chmurach programu System Center Virtual Machine Manager (VMM) i tymi, które nie są. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wymagań wstępnych platformy Azure i lokalnymi
> * Tworzenie magazynu usług odzyskiwania Site Recovery 
> * Ustaw źródła i docelowymi środowisk replikacji 
> * Skonfigurować mapowanie sieci (Jeśli funkcja Hyper-V jest zarządzana przez program System Center VMM)
> * Tworzenie zasad replikacji
> * Włącz replikację dla maszyny Wirtualnej


## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- Upewnij się, że rozumiesz [architektura scenariusza i składniki](concepts-hyper-v-to-azure-architecture.md).
- Przegląd [spełnić wymagania dotyczące](site-recovery-support-matrix-to-azure.md) dla wszystkich składników.
- Upewnij się, że chcesz replikować maszyny wirtualne są zgodne z [wymagania maszyny Wirtualnej Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Aby dowiedzieć się, planowania pojemności:
    - Użyj [Planisty wydajności funkcji Hyper-V](http://aka.ms/asr-capacity-planner-excel)Aby ustalić częstotliwość zmian.
    - Użyj [planowania pojemności odzyskiwania lokacji](http://aka.ms/asr-capacity-planner-excel) do analizowania przez środowisko źródłowe, oszacować przepustowość i określania elementów docelowych wymagania.
- Przygotuj Azure. Potrzebujesz subskrypcji platformy Azure, sieć wirtualną platformy Azure i konto magazynu.
- Przygotowanie lokalnymi hostami funkcji Hyper-V i serwerach VMM w razie potrzeby.





### <a name="set-up-an-azure-account"></a>Konfigurowanie konta platformy Azure

Firma Microsoft [konta Azure](http://azure.microsoft.com/).

- Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- Dowiedz się więcej o [cenach usługi Site Recovery](site-recovery-faq.md#pricing)i uzyskać [szczegóły cennika](https://azure.microsoft.com/pricing/details/site-recovery/).
- Sprawdzić, które [są obsługiwane regiony](https://azure.microsoft.com/pricing/details/site-recovery/) Site Recovery.

### <a name="verify-azure-account-permissions"></a>Sprawdź uprawnienia konta platformy Azure

Upewnij się, że konto platformy Azure z uprawnieniami ma replikować maszyny wirtualne.

- Przegląd [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) trzeba replikować maszyny do platformy Azure
- Sprawdź i zmodyfikuj [dostępu opartej na rolach](../active-directory/role-based-access-control-configure.md) uprawnienia. 


### <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure

Konfigurowanie [sieć platformy Azure](../virtual-network/virtual-network-get-started-vnet-subnet.md).

- Maszyny wirtualne platformy Azure są umieszczane w tej sieci, gdy są tworzone po pracy awaryjnej.
- Sieć powinna znajdować się w tym samym regionie co magazyn usługi Recovery Services.


### <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure Storage

Konfigurowanie [konto magazynu Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Usługa Site Recovery replikuje maszyny lokalnej do magazynu Azure. Maszyny wirtualne platformy Azure są tworzone z magazynu po pracy awaryjnej.
- Konto magazynu musi być w tym samym regionie co magazyn usług odzyskiwania.
- Konto magazynu może być standard lub [premium](../virtual-machines/windows/premium-storage.md).
- Skonfigurowanie konta premium potrzebne dodatkowe konta standardowego dla danych dziennika.

### <a name="prepare-hyper-v-hosts"></a>Przygotowywanie hostów funkcji Hyper-V

1. Sprawdź, które spełniają hosty funkcji Hyper-V [spełnić wymagania dotyczące](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
2. Upewnij się, że hosty mogą uzyskiwać dostęp do tych adresów URL:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Wszystkie reguły zapory oparte na adresie IP powinna zezwalać na komunikację z platformą Azure.
    - Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu 443 protokołu HTTPS.
    - Zezwalaj na zakresy adresów IP dla regionu Azure Twojej subskrypcji i zachodnie stany USA (używanych do zarządzania tożsamości i kontroli dostępu).

### <a name="prepare-vmm-servers"></a>Przygotowanie serwerów programu VMM

Hosty funkcji Hyper-V są zarządzane przez program VMM, należy przygotować na lokalnym serwerze VMM. 

- Sprawdź, czy serwer VMM spełnia [spełnić wymagania dotyczące](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers).
- Upewnij się, że serwer VMM ma co najmniej jedna chmura z co najmniej jedną grupę hostów. Host funkcji Hyper-V, na którym są uruchomione maszyny wirtualne powinien znajdować się w chmurze.
- Serwer VMM musi mieć dostęp do Internetu z dostępem do następujących adresów URL:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
    - Wszystkie reguły zapory oparte na adresie IP powinna zezwalać na komunikację z platformą Azure.
    - Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu 443 protokołu HTTPS.
    - Zezwól na użycie zakresów adresów IP dla regionu platformy Azure Twojej subskrypcji oraz regionu Zachodnie stany USA (służy do kontrolowania dostępu i zarządzania tożsamościami).
- Przygotuj serwer programu VMM do mapowania sieci.


#### <a name="prepare-vmm-for-network-mapping"></a>Przygotowanie programu VMM do mapowania sieci

Jeśli używasz programu VMM, [mapowania sieci](site-recovery-network-mapping.md) mapowania między lokalnymi sieciami maszyny Wirtualnej VMM i sieci wirtualnych platformy Azure. Mapowanie gwarantuje, że maszynach wirtualnych platformy Azure są połączone siecią prawo podczas są tworzone po pracy awaryjnej.

Przygotowanie programu VMM do mapowania sieci w następujący sposób:

1. Upewnij się, że masz [sieć logiczna VMM](https://docs.microsoft.com/system-center/vmm/network-logical) która jest skojarzona z chmury, w którym znajdują się hosty funkcji Hyper-V.
2. Upewnij się, masz [sieci maszyny Wirtualnej](https://docs.microsoft.com/system-center/vmm/network-virtual) połączony z siecią logiczną.
3. Połączenie maszyn wirtualnych z siecią maszyny Wirtualnej.

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]


## <a name="select-a-protection-goal"></a>Wybierz cel ochrony

Wybierz, co ma replicate i gdzie jej do replikacji.

1. W magazynie, kliknij przycisk **usługi Site Recovery** > **przygotowanie infrastruktury** > **cel ochrony**.
2. W **cel ochrony**, wybierz pozycję **do platformy Azure**> **tak, z funkcją Hyper-V**. 
    - Jeśli hosty funkcji Hyper-V nie są zarządzane przez program VMM, wybierz **nr** aby upewnić się, że nie używasz programu VMM.
    - Jeśli hosty są zarządzane w chmurach programu VMM, wybierz **tak**.

## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

Po skonfigurowaniu środowiska źródłowego Zainstaluj dostawcę usługi Azure Site Recovery i agent usług odzyskiwania Azure i zarejestrować lokalnych serwerów w magazynie. 

1. W **przygotowanie infrastruktury**, kliknij przycisk **źródła**. 
    - Jeśli nie używasz programu VMM, kliknij przycisk **+ lokacja funkcji Hyper-V**i określ nazwę lokacji. Następnie kliknij przycisk **+ serwer funkcji Hyper-V**i Dodaj hosta lub klastra do lokacji.
    - Jeśli korzystasz z programu VMM, w **Przygotuj źródło**, kliknij przycisk **+ VMM** Aby dodać serwer programu VMM. W **Dodaj serwer**, sprawdź, czy **serwera programu System Center VMM** pojawia się w **typ serwera**.
2. Pobierz składniki dostawca i agent, w zależności od środowiska.
    - Dla funkcji Hyper-V tylko Pobierz plik instalacyjny dostawcy. Możesz uruchomić plik na każdym hoście funkcji Hyper-V, aby zainstalować agenta i dostawcy.
        
        ![Dostawca bez programu VMM](./media/tutorial-hyper-v-to-azure/download-no-vmm.png)
    
    - Dla funkcji Hyper-V z programem VMM Pobierz dostawca i agent oddzielnie. Uruchom instalację dostawcy na serwerze programu VMM. Uruchom instalację agenta na każdym hoście funkcji Hyper-V.
    
        ![Dostawca i agent programu VMM](./media/tutorial-hyper-v-to-azure/download-vmm.png)
    
3. Pobierz klucz rejestracji magazynu. Należy to po uruchomieniu Instalatora dostawcy. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

### <a name="install-components"></a>Zainstaluj składniki

Zainstaluj składniki jako summmarized w tabeli. 

**Składnik** | **Szczegóły** | **Funkcja Hyper-V tylko** | **Funkcja Hyper-V w programie VMM**
--- | --- | --- | ---
**Dostawca usługi Azure Site Recovery** | Organizuje replikację do platformy Azure | Zainstaluj na każdym hoście funkcji Hyper-V | Zainstaluj na serwerze programu VMM
**Agent usług odzyskiwania** | Obsługuje replikację danych | Zainstaluj na każdym hoście funkcji Hyper-V | Zainstaluj na hoście funkcji Hyper-V


#### <a name="install-the-provider-on-hyper-v-without-vmm"></a>Zainstaluj dostawcę w ramach funkcji Hyper-V bez programu VMM

Zainstaluj dostawcę na każdym hoście funkcji Hyper-V, dodane do lokacji funkcji Hyper-V. Jeśli instalujesz w klastrze funkcji Hyper-V, uruchom Instalatora na każdym węźle klastra. Instalowanie i rejestrowanie w każdym węźle klastra funkcji Hyper-V zapewnia ochronę maszyn wirtualnych, nawet w przypadku migrowania między węzłami.

1. W usłudze **Microsoft Update** można włączyć aktualizacje, aby aktualizacje dostawcy były instalowane zgodnie z zasadami Microsoft Update.
2. W obszarze **Instalacja** zaakceptuj lub zmodyfikuj domyślną lokalizację instalacji dostawcy i kliknij przycisk **Zainstaluj**.
4. W **ustawienia magazynu**, kliknij przycisk **Przeglądaj** wybierz pobrany plik klucza magazynu. Określ subskrypcję usługi Azure Site Recovery, nazwę magazynu i lokacji funkcji Hyper-V, do której należy serwer funkcji Hyper-V.
5. W **ustawienia serwera Proxy**, określ, jak dostawca uruchomiony na serwerze programu VMM lub hosta funkcji Hyper-V łączy się z usługi Site Recovery za pośrednictwem Internetu.
    * Bezpośrednie połączenia wybierz **Połącz bezpośrednio z usługi Azure Site Recovery bez serwera proxy**.
    * Dla serwera proxy, zaznacz **nawiązywanie połączenia z usługi Azure Site Recovery przy użyciu serwera proxy**. Określ adres serwera proxy, port oraz poświadczenia, jeśli to konieczne.
6. Po serwer jest zarejestrowany w magazynie, kliknij przycisk **Zakończ**.

Metadane z serwera funkcji Hyper-V są pobierane przez usługę Azure Site Recovery, a serwer jest wyświetlany w **infrastruktura usługi Site Recovery** > **hosty funkcji Hyper-V**.


#### <a name="install-the-recovery-services-agent-on-hyper-v-host-without-vmm"></a>Zainstaluj agenta usług odzyskiwania na hoście funkcji Hyper-V bez programu VMM

Jeśli używasz programu VMM w danym wdrożeniu należy uruchomić Instalator agenta usług odzyskiwania na każdym hoście funkcji Hyper-V.

1. W Kreatorze instalacji > **Sprawdzanie wymagań wstępnych**, kliknij przycisk **dalej**. Wszystkie brakujące wymagania wstępne zostaną zainstalowane automatycznie.

    ![Wymagania wstępne dotyczące agenta Usług odzyskiwania](./media/tutorial-hyper-v-to-azure/hyperv-agent-prerequisites.png)
3. W obszarze **Ustawienia instalacji** zaakceptuj lub zmodyfikuj lokalizację instalacji i lokalizację pamięci podręcznej. Pamięć podręczna potrzebuje co najmniej 5 GB przestrzeni dyskowej. Firma Microsoft zaleca dysku z najmniej 600 GB wolnego miejsca. Następnie kliknij pozycję **Zainstaluj**.
4. W **instalacji**, po zakończeniu instalacji, kliknij przycisk **Zamknij** aby zakończyć pracę kreatora.

##### <a name="set-up-internet-access-via-a-proxy"></a>Konfigurowanie dostępu do Internetu za pośrednictwem serwera proxy

Agenta usług odzyskiwania na hosta funkcji Hyper-V musi mieć dostęp do Internetu dla platformy Azure w przypadku replikacji maszyny Wirtualnej. Jeśli uzyskujesz dostęp do Internetu za pośrednictwem serwera proxy, skonfiguruj go w następujący sposób:

1. Otwórz przystawkę MMC usługi Microsoft Azure Backup na hoście funkcji Hyper-V. Domyślnie skrót do usługi Kopia zapasowa Microsoft Azure jest dostępna na pulpicie lub w C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. W przystawce kliknij pozycję **Zmień właściwości**.
3. Na **konfiguracji serwera Proxy** karcie, określ informacje dotyczące serwera proxy.

    ![Rejestracja agenta MARS](./media/tutorial-hyper-v-to-azure/mars-proxy.png)
4. Sprawdź, czy agent może osiągnąć [wymagane adresy URL](#prepare-hyper-v-hosts).

#### <a name="install-the-provider-on-the-vmm-server-hyper-v-with-vmm"></a>Zainstaluj dostawcę na serwerze programu VMM (Hyper-V z programem VMM)

1. W usłudze **Microsoft Update** można włączyć aktualizacje, aby aktualizacje dostawcy były instalowane zgodnie z zasadami Microsoft Update.
2. W obszarze **Instalacja** zaakceptuj lub zmodyfikuj domyślną lokalizację instalacji dostawcy i kliknij przycisk **Zainstaluj**. 
3. Teraz można zarejestrować serwera VMM w magazynie. W **ustawienia magazynu**, kliknij przycisk **Przeglądaj** wybierz pobrany plik klucza magazynu. Określ subskrypcję usługi Azure Site Recovery oraz nazwę magazynu.

    ![Rejestracja serwera](./media/tutorial-hyper-v-to-azure/provider-vault-settings.png)

4. W **ustawienia serwera Proxy**, określ, jak dostawca uruchomiony na serwerze programu VMM lub hosta funkcji Hyper-V łączy się z usługi Site Recovery za pośrednictwem Internetu.

    * Bezpośrednie połączenia wybierz **Połącz bezpośrednio z usługi Azure Site Recovery bez serwera proxy**.
    * Dla serwera proxy, zaznacz **nawiązywanie połączenia z usługi Azure Site Recovery przy użyciu serwera proxy**. Określ adres serwera proxy, port oraz poświadczenia, jeśli to konieczne.
    - Konto Uruchom jako programu VMM (DRAProxyAccount) jest automatycznie utworzone przy użyciu określonych poświadczeń serwera proxy. Skonfiguruj serwer proxy tak, aby to konto mogło być pomyślnie uwierzytelnione. Ustawienia konta Uruchom jako można modyfikować w konsoli programu VMM > **ustawienia** > **zabezpieczeń** > **konta Uruchom jako**. Uruchom ponownie usługę VMM, aby zaktualizować zmiany.
5. W **szyfrowanie danych**, określ, czy zaszyfrowane wszystkie dane przesyłane do platformy Azure. Po wybraniu tej opcji odzyskiwania lokacji wystawia certyfikat. Będziesz potrzebować tego certyfikatu do odszyfrowywania danych w przyszłości.
6. W **serwera VMM**, określ przyjazną nazwę identyfikującą serwer VMM w magazynie. W konfiguracji klastra określ nazwę roli klastra VMM. W **Synchronizuj metadane chmury z magazynem**wybierz, czy chcesz synchronizować metadane dla wszystkich chmur na serwerze programu VMM w magazynie. To działanie ma miejsce tylko raz na każdym serwerze. Jeśli nie chcesz synchronizować wszystkich chmur, można zaznaczać tego ustawienia i synchronizować poszczególne chmury indywidualnie we właściwościach chmury w konsoli programu VMM.

Po zakończeniu rejestracji, metadane z serwera są pobierane przez usługę Azure Site Recovery, a serwer programu VMM są wyświetlane w **infrastruktura usługi Site Recovery**.






## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

Wybierz i zweryfikować zasobów docelowych. 

1. Kliknij przycisk **przygotowanie infrastruktury** > **docelowej**.
2. Wybierz subskrypcji i grupy zasobów, w którym zostanie utworzona maszynach wirtualnych Azure po pracy awaryjnej. Wybierz model wdrażania, który ma być używany na platformie Azure dla maszyn wirtualnych.

3. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

## <a name="configure-network-mapping-with-vmm"></a>Konfigurowanie mapowania sieci (w programie VMM)

Jeśli używasz programu VMM, należy skonfigurować mapowanie sieci.

1. W obszarze **Infrastruktura usługi Site Recovery** > **Mapowania sieci** > **Mapowanie sieci** kliknij ikonę **+ Mapowanie sieci**.
2. W **Dodaj mapowanie sieci**, wybierz źródłowy serwer programu VMM. Wybierz **Azure** jako element docelowy.
3. Sprawdź subskrypcję i model wdrożenia po przejściu do trybu failover.
4. W **Sieć źródłowa**, wybierz sieć źródłową lokalnej maszyny Wirtualnej.
5. W **Sieć docelowa**, wybierz sieć platformy Azure, w których repliki maszyn wirtualnych Azure zostaną umieszczone podczas są tworzone po pracy awaryjnej. Następnie kliknij przycisk **OK**.

    ![Mapowanie sieci](./media/tutorial-hyper-v-to-azure/network-mapping-vmm.png)

## <a name="create-a-replication-policy"></a>Tworzenie zasad replikacji

1. Kliknij przycisk **przygotowanie infrastruktury** > **ustawienia replikacji** > **+ Utwórz i skojarz**.
2. W obszarze **Utwórz i skojarz zasady** określ nazwę zasad.
3. W obszarze **Częstotliwość kopiowania** określ, jak często mają być replikowane dane przyrostowe po replikacji początkowej (co 30 sekund, 5 lub 15 minut).

    > [!NOTE]
    >  Częstotliwość 30-sekundowa nie jest obsługiwana w przypadku replikowania do usługi Premium Storage. To ograniczenie wynika z liczby migawek na obiekt blob (100) obsługiwanych przez usługę Premium Storage. [Dowiedz się więcej](../virtual-machines/windows/premium-storage.md#snapshots-and-copy-blob).

4. W **przechowywania punktu odzyskiwania**, określ czas przechowywania okno będzie (w godzinach) dla każdego punktu odzyskiwania. Chronione maszyny można odzyskać do dowolnego punktu w tym oknie.
5. W obszarze **Częstotliwość migawek spójności aplikacji** określ, jak często (1–12 godzin) będą tworzone punkty odzyskiwania zawierające migawki spójne z aplikacjami. Funkcja Hyper-V wykorzystuje dwa typy migawek:
    - **Standardowa migawka**: jest przyrostową migawką całej maszyny wirtualnej.
    - **Migawki dotyczącej spójności aplikacji**: tworzy migawkę w momencie danych aplikacji wewnątrz maszyny Wirtualnej. Usługa kopiowania woluminów w tle (VSS) zapewnia, że aplikacje są w spójnym stanie podczas wykonywania migawki. Włączanie migawki spójne z aplikacjami wpływa na wydajność aplikacji w źródle maszyn wirtualnych. Wartość, która jest mniejsza niż liczba punktów odzyskiwania dodatkowe, które można skonfigurować.
6. W obszarze **Czas rozpoczęcia replikacji początkowej** określ, kiedy rozpoczyna się replikacja początkowa. Replikacja odbywa się za pośrednictwem przepustowości połączenia z Internetem, dlatego warto zaplanować ją poza godzinami szczytu.
7. W obszarze **Szyfrowanie danych przechowywanych na platformie Azure** określ, czy należy szyfrować dane w stanie spoczynku w usłudze Azure Storage. Następnie kliknij przycisk **OK**.

    ![Zasady replikacji](./media/tutorial-hyper-v-to-azure/replication-policy.png)


Podczas tworzenia nowej zasady zostaną one automatycznie skojarzone z chmury VMM lub lokacji funkcji Hyper-V.

## <a name="enable-replication"></a>Włączanie replikacji


1. Kliknij przycisk **Replikowanie aplikacji** > **źródła**. 
2. W **źródła**, wybierz serwer VMM/lokacji funkcji Hyper-V/chmura. Następnie kliknij przycisk **OK**.
3. W **docelowego**, sprawdź Azure jako element docelowy, subskrypcja magazynu i modelu, który chcesz używać po w tryb failover na platformie Azure.
4. Wybierz konto magazynu dla replikowanych danych i sieć platformy Azure, w którym maszyny wirtualne Azure zostaną umieszczone po pracy awaryjnej.
5. W **maszyn wirtualnych** > **wybierz**, wybierz maszyny wirtualne mają być replikowane. Następnie kliknij przycisk **OK**.

 Możesz śledzić postęp **Włącz ochronę** akcji w **zadania** > **zadania usługi Site Recovery**. Po **zakończenia ochrony** zadanie zostało ukończone, Replikacja początkowa została zakończona i maszyna wirtualna jest gotowa do pracy awaryjnej.

## <a name="next-steps"></a>Następne kroki
[Uruchamianie próbnego odzyskiwania po awarii](tutorial-dr-drill-azure.md)
