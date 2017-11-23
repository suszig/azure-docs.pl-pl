---
title: Replikowanie aplikacji (Azure do platformy Azure) | Dokumentacja firmy Microsoft
description: "W tym artykule opisano sposób skonfigurowania replikacji maszyn wirtualnych działających w jednym regionie Azure do innego regionu na platformie Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: asgang
ms.openlocfilehash: dc7dff33aa2c3e844c6a91024fcfc98148416f7e
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/22/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Replikowanie maszyn wirtualnych platformy Azure do innego regionu systemu Azure



>[!NOTE]
>
> Replikacja lokacji odzyskiwania maszyn wirtualnych platformy Azure jest obecnie w przeglądzie.

W tym artykule opisano sposób skonfigurowania replikacji maszyn wirtualnych działających w jednym regionie Azure do innego regionu systemu Azure.

## <a name="prerequisites"></a>Wymagania wstępne

* Artykule przyjęto założenie, że już wiedzę na temat odzyskiwania lokacji i magazynu usług odzyskiwania. Musisz mieć "Magazyn usług odzyskiwania" wstępnie utworzone.

    >[!NOTE]
    >
    > Zalecane jest tworzenie magazynu usług odzyskiwania w miejscu, gdzie ma maszyny wirtualne do replikacji. Na przykład jeśli lokalizacja docelowej środkowe stany USA, należy utworzyć magazynu w środkowe stany USA.

* Upewnij się, jeśli używane są zasady grupy zabezpieczeń sieci (NSG) lub serwer proxy zapory do kontrolowania dostępu do wychodzące połączenie z Internetem na maszynach wirtualnych Azure, tym możesz dozwolonych wymagane adresy URL lub adresów IP. Zapoznaj się [sieci wytycznych](./site-recovery-azure-to-azure-networking-guidance.md) więcej szczegółów.

* Jeśli masz usługi ExpressRoute lub połączeń VPN między lokalnymi a lokalizacją źródłową na platformie Azure, wykonaj [zagadnienia dotyczące odzyskiwania lokacji dla platformy Azure lokalnej usługi expressroute / konfiguracji sieci VPN](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentu.

* Twoje konto użytkownika usługi Azure musi mieć niektórych [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) Aby włączyć replikację maszyny wirtualnej platformy Azure.

* Do tworzenia maszyn wirtualnych w lokalizacji docelowej, który ma być używany jako region odzyskiwania po awarii powinna być włączona subskrypcji platformy Azure. Można skontaktuj się z pomocą techniczną, aby włączyć wymagane przydziału.

## <a name="enable-replication-from-azure-site-recovery-vault"></a>Włącz replikację z magazynu usługi Azure Site Recovery
Na tej ilustracji, firma Microsoft będzie replikowanie maszyn wirtualnych uruchomionych w Azja Wschodnia lokalizacji platformy Azure w lokalizacji "Azja południowo-wschodnia". Dostępne są następujące kroki:

 Kliknij przycisk **+ Replikuj** w magazynie, aby włączyć replikację dla maszyn wirtualnych.

1. **Źródło:** odnosi się do punktu pochodzenia maszyn, w tym przypadku jest **Azure**.

2. **Lokalizacja źródłowa:** jest region platformy Azure, z której mają być chronione maszyny wirtualne. Na tej ilustracji lokalizacja źródłowa jest "Azja Wschodnia"

3. **Model wdrażania:** odnosi się do modelu wdrożenia usługi Azure maszyna źródłowa. Można wybrać obu Classic lub Menedżera zasobów i komputery należące do określonego modelu zostaną wyświetlone w celu ochrony w następnym kroku.

      >[!NOTE]
      >
      > Można tylko replikowanie klasyczne maszyny wirtualnej i odzyskać ją jako maszynę wirtualną w klasycznym. Nie można go odzyskać jako maszynę wirtualną Menedżera zasobów.

4. **Grupa zasobów:** jest grupa zasobów, do której należy źródło maszyn wirtualnych. Zostaną wyświetlone wszystkie maszyny wirtualne w wybranej grupy zasobów do ochrony w następnym kroku.

    ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

W **maszyny wirtualnej > Wybierz maszyny wirtualne**, kliknij i zaznacz każdej maszynie, którą chcesz replikować. Możesz wybrać tylko te maszyny, dla których można włączyć replikację. Następnie kliknij przycisk OK.
    ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)


W sekcji Ustawienia można skonfigurować właściwości lokacji docelowej

1. **Lokalizacja docelowa:** jest to lokalizacja, w którym będą replikowane dane źródłowe maszyny wirtualnej. W zależności od lokalizacji wybranych maszyn Site Recovery zapewnia listę regionów odpowiedniego elementu docelowego.

    > [!TIP]
    > Zaleca się zachowanie lokalizacji docelowej takie same jak odzyskiwania usług magazynu.

2. **Docelowa grupa zasobów:** jest grupa zasobów, aby wszystkie zreplikowanej maszyny wirtualne będą należeć. Domyślnie usługi Azure Site Recovery spowoduje utworzenie nowej grupy zasobów w region docelowy z nazwą składającą się z sufiksem "asr". W przypadku grupy zasobów utworzonej za pomocą usługi Azure Site Recovery już istnieje, zostanie ono użyte ponownie. Można również dostosować go, jak pokazano w poniższej sekcji.    
3. **Sieć wirtualna docelowa:** Domyślnie usługi Azure Site Recovery spowoduje utworzenie nowej sieci wirtualnej w regionie docelowych z nazwą składającą się z sufiksem "asr". To zostaną zmapowane do źródłowej sieci i będzie służyć do przyszłych ochrony.

    > [!NOTE]
    > [Sprawdź szczegóły sieci](site-recovery-network-mapping-azure-to-azure.md) Aby dowiedzieć się więcej o mapowania sieci.

4. **Docelowa kont magazynu:** Domyślnie usługi Azure Site Recovery spowoduje utworzenie nowego konta magazynu docelowego mimicking konfigurację magazynu maszyny Wirtualnej źródłowego. W przypadku konta magazynu utworzone za pomocą usługi Azure Site Recovery już istnieje, zostanie ono użyte ponownie.

5. **Buforowanie kont magazynu:** usługi Azure Site Recovery wymaga konta dodatkowego magazynu o nazwie magazynu pamięci podręcznej w regionie źródła. Wszystkie zmiany, które są wykonywane na maszynach wirtualnych źródła są śledzone i wysyłane do konta magazynu pamięci podręcznej przed replikowanie tych do lokalizacji docelowej.

6. **Zestaw dostępności:** Domyślnie usługi Azure Site Recovery spowoduje utworzenie nowej zestawem dostępności w region docelowy z nazwą składającą się z sufiksem "asr". W przypadku, gdy zestaw dostępności utworzone przez usługę Azure Site Recovery już istnieje, zostanie ono użyte ponownie.

7.  **Zasady replikacji:** definiuje ustawienia odzyskiwania punktu przechowywania historii i aplikacji migawki dotyczącej spójności częstotliwości. Domyślnie program Azure Site Recovery utworzy nowe zasady replikacji z ustawieniami domyślnymi 24 godzin przechowywania punktu odzyskiwania i "60 minut częstotliwości migawki dotyczącej spójności aplikacji.

    ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a>Dostosowywanie zasobami obiektów docelowych

W przypadku, gdy chcesz zmienić ustawienia domyślne używane przez usługi Azure Site Recovery, możesz zmienić ustawienia, na podstawie Twoich potrzeb.

1. **Dostosowywanie:** kliknij go, aby zmienić ustawienia domyślne używane przez usługi Azure Site Recovery.

2. **Docelowa grupa zasobów:** grupy zasobów można wybrać z listy grup zasobów istniejących w miejscu docelowym w ramach subskrypcji.

3. **Sieć wirtualna docelowa:** można znaleźć na liście sieci wirtualnej w lokalizacji docelowej.

4. **Zestaw dostępności:** ustawienia zestawy dostępności można dodawać tylko do maszyn wirtualnych, które są częścią dostępności w regionie źródła.

5. **Konta magazynu docelowego:**

![Włącz replikację](./media/site-recovery-replicate-azure-to-azure/customize.PNG) kliknij **tworzenie zasobu docelowego** i włączyć replikację


Gdy są chronione maszyny wirtualne można sprawdzić stan kondycji maszyn wirtualnych w obszarze **zreplikowane elementy**

>[!NOTE]
>Podczas replikacji początkowej może mieć możliwość, że stan czas, aby odświeżyć i widzisz postępu przez pewien czas. Kliknięcie przycisku Odśwież w górnej części bloku, aby pobrać najnowszy stan.
>

![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a>Następne kroki
- [Dowiedz się więcej](site-recovery-test-failover-to-azure.md) uruchomiony test trybu failover.
- [Dowiedz się więcej](site-recovery-failover.md) o różnych typach trybu failover i sposobu ich uruchamiania.
- Dowiedz się więcej o [przy użyciu planów odzyskiwania](site-recovery-create-recovery-plans.md) zmniejszenie RTO.
- Dowiedz się więcej o [ponownej ochrony maszyn wirtualnych platformy Azure](site-recovery-how-to-reprotect.md) po pracy awaryjnej.
