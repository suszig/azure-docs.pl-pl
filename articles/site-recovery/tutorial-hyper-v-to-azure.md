---
title: "Konfigurowanie odzyskiwania po awarii dla maszyn wirtualnych funkcji Hyper-V lokalnymi (bez VMM) na platformie Azure za pomocą usługi Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnymi (bez VMM) na platformie Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 12/31`/2017
ms.author: raynew
ms.openlocfilehash: 633c14bd25bc8a1419196b2c76ca94c26db68991
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="set-up-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>Konfigurowanie odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do strategii odzyskiwania po awarii poprzez organizowanie replikacji, trybu failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM) i zarządzanie nimi.

W tym samouczku przedstawiono sposób konfigurowania odzyskiwania po awarii maszyn wirtualnych funkcji Hyper-V lokalnego do platformy Azure. Samouczek ma zastosowanie w przypadku maszyn wirtualnych funkcji Hyper-V, które nie są zarządzane przez System Center Virtual Machine Manager (VMM). Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wybierz źródła replikacji i docelowej.
> * Konfigurowanie środowiska źródłowego replikacji, łącznie z lokalnymi składnikami usługi Site Recovery i środowiska docelowego replikacji.
> * Utwórz zasady replikacji.
> * Włącz replikację maszyny Wirtualnej.

To jest trzeci samouczek w serii. Ten samouczek zakłada, zostały już wykonane zadania w poprzednim samouczki:

1. [Przygotowywanie platformy Azure](tutorial-prepare-azure.md)
2. [Przygotuj lokalną funkcji Hyper-V](tutorial-prepare-on-premises-hyper-v.md)

Przed rozpoczęciem warto [Przejrzyj architektura](concepts-hyper-v-to-azure-architecture.md) w tym scenariuszu odzyskiwania po awarii.

## <a name="select-a-replication-goal"></a>Wybierz cel replikacji


1. W **wszystkie usługi** > **Magazyny usług odzyskiwania**, kliknij nazwę magazynu, możemy przygotowany w poprzednim samouczek, **ContosoVMVault**.
2. W **wprowadzenie**, kliknij przycisk **usługi Site Recovery**. Następnie kliknij przycisk **Przygotowywanie infrastruktury**
3. W **cel ochrony** > **których komputery znajdujące się**, wybierz pozycję **lokalnymi**.
4. W **gdzie chcesz zreplikować maszyny**, wybierz pozycję **do platformy Azure**.
5. W **są maszynach zwirtualizowanych**, wybierz pozycję **tak, z funkcją Hyper-V**.
6. W **korzystasz z programu System Center VMM**, wybierz pozycję **nr**. Następnie kliknij przycisk **OK**.

    ![Celem replikacji](./media/tutorial-hyper-v-to-azure/replication-goal.png)

## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

Aby skonfigurować środowisko źródłowe, możesz dodać hosty Hyper-V do lokacji funkcji Hyper-V, pobranie i zainstalowanie dostawcy usługi Azure Site Recovery i agent usług odzyskiwania Azure i zarejestrować lokacji funkcji Hyper-V w magazynie. 

1. W **przygotowanie infrastruktury**, kliknij przycisk **źródła**.
2. Kliknij przycisk **+ lokacja funkcji Hyper-V**i określ nazwę lokacji utworzyliśmy poprzedniej samouczka **ContosoHyperVSite**.
3. Kliknij przycisk **+ serwer funkcji Hyper-V**.
4. Pobierz plik Instalatora dostawcy.
5. Pobierz klucz rejestracji magazynu. Należy to po uruchomieniu Instalatora dostawcy. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

    ![Pobierz dostawcę](./media/tutorial-hyper-v-to-azure/download.png)
    

### <a name="install-the-provider"></a>Zainstaluj dostawcę

Uruchom plik Instalatora dostawcy (AzureSiteRecoveryProvider.exe) na każdym funkcji Hyper-V host zostanie dodany do **ContosoHyperVSite** lokacji. Instalacja dostawcy usługi Azure Site Recovery i agent usług odzyskiwania, na każdym hoście funkcji Hyper-V.

1. W kreatorze Instalacja dostawcy usługi Azure Site Recovery > **Microsoft Update**, zgadzaj się na Użyj usługi Microsoft Update do sprawdzania aktualizacji dostawcy.
2. W **instalacji**, zaakceptuj domyślną lokalizację instalacji dostawcy i agenta i kliknij przycisk **zainstalować**.
3. Po zakończeniu instalacji w Kreator rejestracji Microsoft Azure Site Recovery > **ustawienia magazynu**, kliknij przycisk **Przeglądaj**i w **plik klucza**, wybierz plik klucza magazynu, który pobrana. 
4. Określ subskrypcję usługi Azure Site Recovery, nazwę magazynu (**ContosoVMVault**), a lokacja funkcji Hyper-V (**ContosoHyperVSite**) do której należy serwer funkcji Hyper-V.
5. W **ustawienia serwera Proxy**, wybierz pozycję **Połącz bezpośrednio z usługi Azure Site Recovery bez serwera proxy**.
6. W **rejestracji**po serwer jest zarejestrowany w magazynie, kliknij przycisk **Zakończ**.

Metadane z serwera funkcji Hyper-V są pobierane przez usługę Azure Site Recovery, a serwer jest wyświetlany w **infrastruktura usługi Site Recovery** > **hosty funkcji Hyper-V**. To może potrwać do 30 minut.


## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

Wybierz i zweryfikować zasobów docelowych. 

1. Kliknij przycisk **przygotowanie infrastruktury** > **docelowej**.
2. Wybierz subskrypcji i grupy zasobów **ContosoRG**, w której maszyny wirtualne Azure zostanie utworzony po pracy awaryjnej.
3. Wybierz **Resource Manager "** modelu wdrażania.

Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.


## <a name="set-up-a-replication-policy"></a>Konfigurowanie zasad replikacji

1. Kliknij przycisk **przygotowanie infrastruktury** > **ustawienia replikacji** > **+ Utwórz i skojarz**.
2. W **Utwórz i Skojarz zasady**, podaj nazwę zasad, **ContosoReplicationPolicy**.
3. Pozostaw ustawienia domyślne i kliknij przycisk **OK**.
    - **Częstotliwość kopiowania** wskazuje, że delta danych (po replikacji początkowej) zostanie zreplikowany co pięć minut.
    - **Przechowywania punktu odzyskiwania** oznacza, że okna przechowywania dla każdego punktu odzyskiwania będzie dwa dwóch godzin.
    - **Częstotliwość migawek spójności aplikacji** wskazuje, że punkty odzyskiwana zawierające migawki spójne z aplikacji będą tworzone co godzinę.
    - **Czas rozpoczęcia replikacji początkowej**, wskazuje, czy Replikacja początkowa rozpocznie się natychmiast.
4. Po utworzeniu zasad, kliknij przycisk **OK**. Podczas tworzenia nowych zasad, zostaną one automatycznie skojarzone z określonej lokacji funkcji Hyper-V (**ContosoHyperVSite**)

    ![Zasady replikacji](./media/tutorial-hyper-v-to-azure/replication-policy.png)


## <a name="enable-replication"></a>Włączanie replikacji


1. W **Replikowanie aplikacji**, kliknij przycisk **źródła**. 
2. W **źródła**, wybierz pozycję **ContosoHyperVSite** lokacji. Następnie kliknij przycisk **OK**.
3. W **docelowej**, sprawdź Azure jako element docelowy subskrypcji magazynu i **Resource Manager** modelu wdrażania.
4. Wybierz **contosovmsacct1910171607** utworzyliśmy w poprzednich instrukcji dla replikowanych danych, konta magazynu i **ContosoASRnet** sieci, w którą maszyny wirtualne Azure zostaną umieszczone po pracy awaryjnej.
5. W **maszyn wirtualnych** > **wybierz**, wybierz maszynę Wirtualną, którą chcesz replikować. Następnie kliknij przycisk **OK**.

 Możesz śledzić postęp **Włącz ochronę** akcji w **zadania** > **zadania usługi Site Recovery**. Po **zakończenia ochrony** zadanie zostało ukończone, Replikacja początkowa została zakończona i maszyna wirtualna jest gotowa do pracy awaryjnej.

## <a name="next-steps"></a>Kolejne kroki
[Uruchamianie próbnego odzyskiwania po awarii](tutorial-dr-drill-azure.md)
