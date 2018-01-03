---
title: "Konfigurowanie odzyskiwania po awarii lokalnych maszyn wirtualnych z funkcją Hyper-V w chmurach VMM do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować odzyskiwania po awarii lokalnych maszyn wirtualnych z funkcją Hyper-V w chmurach programu System Center VMM do platformy Azure, z usługą Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 12/31/2017
ms.author: raynew
ms.openlocfilehash: 5f364c6f8a88ad53c12909f0e9f10afcce5ef8af
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-in-vmm-clouds-to-azure"></a>Konfigurowanie odzyskiwania po awarii lokalnych maszyn wirtualnych z funkcją Hyper-V w chmurach VMM do platformy Azure

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do strategii odzyskiwania po awarii poprzez organizowanie replikacji, trybu failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM) i zarządzanie nimi.

W tym samouczku przedstawiono sposób konfigurowania odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure. Samouczek ma zastosowanie w przypadku maszyn wirtualnych funkcji Hyper-V, które są zarządzane przez System Center Virtual Machine Manager (VMM). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wybierz źródła replikacji i docelowej.
> * Konfigurowanie środowiska źródłowego replikacji, łącznie z lokalnymi składnikami usługi Site Recovery i środowiska docelowego replikacji.
> * Skonfigurowanie mapowania sieci, do mapowania sieci maszyny Wirtualnej VMM i sieci wirtualnych platformy Azure.
> * Tworzenie zasad replikacji
> * Włącz replikację dla maszyny Wirtualnej

To jest trzeci samouczek w serii. Ten samouczek zakłada, zostały już wykonane zadania w poprzednim samouczki:

1. [Przygotowywanie platformy Azure](tutorial-prepare-azure.md)
2. [Przygotuj lokalną funkcji Hyper-V](tutorial-prepare-on-premises-hyper-v.md)

Przed rozpoczęciem warto [Przejrzyj architektura](concepts-hyper-v-to-azure-architecture.md) w tym scenariuszu odzyskiwania po awarii.



## <a name="select-a-replication-goal"></a>Wybierz cel replikacji

1. W **wszystkie usługi** > **Magazyny usług odzyskiwania**, kliknij nazwę magazynu używamy w tych samouczkach **ContosoVMVault**.
2. W **wprowadzenie**, kliknij przycisk **usługi Site Recovery**. Następnie kliknij przycisk **Przygotowywanie infrastruktury**
3. W **cel ochrony** > **których komputery znajdujące się**, wybierz pozycję **lokalnymi**.
4. W **gdzie chcesz zreplikować maszyny**, wybierz pozycję **do platformy Azure**.
5. W **są maszynach zwirtualizowanych**, wybierz pozycję **tak, z funkcją Hyper-V**.
6. W **korzystasz z programu System Center VMM**, wybierz pozycję **tak**. Następnie kliknij przycisk **OK**.

    ![Celem replikacji](./media/tutorial-hyper-v-vmm-to-azure/replication-goal.png)



## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

Po skonfigurowaniu środowiska źródłowego Zainstaluj dostawcę usługi Azure Site Recovery i agent usług odzyskiwania Azure i zarejestrować lokalnych serwerów w magazynie. 

1. W **przygotowanie infrastruktury**, kliknij przycisk **źródła**.
2. W obszarze **Przygotowywanie źródła** kliknij pozycję **+ VMM**, aby dodać serwer programu VMM. W **Dodaj serwer**, sprawdź, czy **serwera programu System Center VMM** pojawia się w **typ serwera**.
3. Pobierz Instalator dostawcy usługi Microsoft Azure Site Recovery.
4. Pobierz klucz rejestracji magazynu. Należy to po uruchomieniu Instalatora dostawcy. Klucz jest ważny przez pięć dni po jego wygenerowaniu.
5. Pobierz agenta usług odzyskiwania.

    ![Do pobrania](./media/tutorial-hyper-v-vmm-to-azure/download-vmm.png)

### <a name="install-the-provider-on-the-vmm-server"></a>Instalowanie dostawcy na serwerze VMM

1. W kreatorze Instalacja dostawcy usługi Azure Site Recovery > **Microsoft Update**, zgadzaj się na Użyj usługi Microsoft Update do sprawdzania aktualizacji dostawcy.
2. W **instalacji**, zaakceptuj domyślną lokalizację instalacji dostawcy i kliknij przycisk **zainstalować**. 
3. Po zakończeniu instalacji w Kreator rejestracji Microsoft Azure Site Recovery > **ustawienia magazynu**, kliknij przycisk **Przeglądaj**i w **plik klucza**, wybierz plik klucza magazynu, który pobrana.
4. Określ subskrypcję usługi Azure Site Recovery oraz nazwę magazynu (**ContosoVMVault**). Określ przyjazną nazwę dla serwera VMM, aby zidentyfikować go w magazynie.
5. W **ustawienia serwera Proxy**, wybierz pozycję **Połącz bezpośrednio z usługi Azure Site Recovery bez serwera proxy**.
6. Zaakceptuj lokalizację domyślną dla certyfikatu, który jest używany do szyfrowania danych. Zaszyfrowane dane zostaną odszyfrowane, podczas przejścia w tryb failover.
7. W **Synchronizuj metadane chmury**, wybierz pozycję **Synchronizuj metadane chmury do portalu usługi Site Recovery**. To działanie ma miejsce tylko raz na każdym serwerze. Następnie kliknij przycisk **zarejestrować**.
8. Po serwer jest zarejestrowany w magazynie, kliknij przycisk **Zakończ**.

Po zakończeniu rejestracji, metadane z serwera są pobierane przez usługę Azure Site Recovery, a serwer programu VMM są wyświetlane w **infrastruktura usługi Site Recovery**.

### <a name="install-the-recovery-services-agent"></a>Zainstaluj agenta usług odzyskiwania

Zainstaluj agenta na każdym hoście funkcji Hyper-V zawierające maszyny wirtualne mają być replikowane.

1. W Kreatorze instalacji agenta usług odzyskiwania Microsoft Azure > **Sprawdzanie wymagań wstępnych**, kliknij przycisk **dalej**. Wszystkie brakujące wymagania wstępne zostaną zainstalowane automatycznie.
2. W **ustawienia instalacji**, zaakceptuj lokalizację instalacji i lokalizację pamięci podręcznej. Pamięć podręczna potrzebuje co najmniej 5 GB przestrzeni dyskowej. Firma Microsoft zaleca dysku z najmniej 600 GB wolnego miejsca. Następnie kliknij pozycję **Zainstaluj**.
3. W **instalacji**, po zakończeniu instalacji, kliknij przycisk **Zamknij** aby zakończyć pracę kreatora.

    ![Zainstaluj agenta](./media/tutorial-hyper-v-vmm-to-azure/mars-install.png)
    

## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

1. Kliknij przycisk **przygotowanie infrastruktury** > **docelowej**.
2. Wybierz subskrypcji i grupy zasobów (**ContosoRG**), w której maszyny wirtualne Azure zostanie utworzony po pracy awaryjnej.
3. Wybierz **Resource Manager "** modelu wdrażania.

Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.


## <a name="configure-network-mapping"></a>Konfiguracja mapowania sieci

1. W obszarze **Infrastruktura usługi Site Recovery** > **Mapowania sieci** > **Mapowanie sieci** kliknij ikonę **+ Mapowanie sieci**.
2. W **Dodaj mapowanie sieci**, wybierz źródłowy serwer programu VMM. Wybierz **Azure** jako element docelowy.
3. Sprawdź subskrypcję i model wdrożenia po przejściu do trybu failover.
4. W **Sieć źródłowa**, wybierz sieć źródłową lokalnej maszyny Wirtualnej.
5. W **Sieć docelowa**, wybierz sieć platformy Azure, w których repliki maszyn wirtualnych Azure zostaną umieszczone podczas są tworzone po pracy awaryjnej. Następnie kliknij przycisk **OK**.

    ![Mapowanie sieci](./media/tutorial-hyper-v-vmm-to-azure/network-mapping-vmm.png)

## <a name="set-up-a-replication-policy"></a>Konfigurowanie zasad replikacji

1. Kliknij przycisk **przygotowanie infrastruktury** > **ustawienia replikacji** > **+ Utwórz i skojarz**.
2. W **Utwórz i Skojarz zasady**, podaj nazwę zasad, **ContosoReplicationPolicy**.
3. Pozostaw ustawienia domyślne i kliknij przycisk **OK**.
    - **Częstotliwość kopiowania** wskazuje, że delta danych (po replikacji początkowej) zostanie zreplikowany co pięć minut.
    - **Przechowywania punktu odzyskiwania** oznacza, że okna przechowywania dla każdego punktu odzyskiwania będzie dwa dwóch godzin.
    - **Częstotliwość migawek spójności aplikacji** wskazuje, że punkty odzyskiwana zawierające migawki spójne z aplikacji będą tworzone co godzinę.
    - **Czas rozpoczęcia replikacji początkowej**, wskazuje, czy Replikacja początkowa rozpocznie się natychmiast.
    - **Szyfruj dane przechowywane na platformie Azure** — wartość domyślna **poza** ustawienie wskazuje, że przechowywanych danych na platformie Azure nie jest zaszyfrowany.
4. Po utworzeniu zasad, kliknij przycisk **OK**. Jeśli utworzysz nowe zasady, zostaną one automatycznie skojarzone z chmurą VMM.

## <a name="enable-replication"></a>Włączanie replikacji

1. W **Replikowanie aplikacji**, kliknij przycisk **źródła**. 
2. W **źródła**, wybierz chmurę VMM. Następnie kliknij przycisk **OK**.
3. W **docelowej**, sprawdź Azure jako element docelowy subskrypcji magazynu i wybierz **Resource Manager** modelu.
4. Wybierz **contosovmsacct1910171607** konta magazynu i **ContosoASRnet** sieć platformy Azure.
5. W **maszyn wirtualnych** > **wybierz**, wybierz maszynę Wirtualną, którą chcesz replikować. Następnie kliknij przycisk **OK**.

 Możesz śledzić postęp **Włącz ochronę** akcji w **zadania** > **zadania usługi Site Recovery**. Po **zakończenia ochrony** zadanie zostało ukończone, Replikacja początkowa została zakończona i maszyna wirtualna jest gotowa do pracy awaryjnej.


## <a name="next-steps"></a>Kolejne kroki
[Uruchamianie próbnego odzyskiwania po awarii](tutorial-dr-drill-azure.md)
