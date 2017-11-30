---
title: "Replikowanie maszyn wirtualnych Azure w regionie pomocniczym z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób replikowania maszyn wirtualnych platformy Azure z jednego regionu Azure systemu do innego regionu, przy użyciu usługi Azure Site Recovery."
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
ms.date: 11/29/2017
ms.author: asgang
ms.openlocfilehash: 114390712f5e5bf62ec515db62469e09361b8f85
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/30/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a>Replikowanie maszyn wirtualnych platformy Azure do innego regionu systemu Azure


W tym artykule opisano sposób replikowania maszyn wirtualnych platformy Azure (maszyny wirtualne) w jednym regionie Azure, do dodatkowej regionu Azure, przy użyciu usługi Azure Site Recovery.

>[!NOTE]
>
> Azure replikację maszyny Wirtualnej z usługą Site Recovery jest obecnie w przeglądzie.

## <a name="prerequisites"></a>Wymagania wstępne

* Magazyn usług odzyskiwania powinien mieć w miejscu. Zaleca się utworzenie magazynu w region docelowy, z którym chcesz maszyny wirtualne do replikacji.
* Jeśli używane są zasady grupy zabezpieczeń sieci (NSG) lub serwer proxy zapory do kontrolowania dostępu do wychodzące połączenie z Internetem na maszynach wirtualnych Azure, upewnij się, musisz zezwolić odpowiednie adresy URL lub adresów IP. [Dowiedz się więcej](./site-recovery-azure-to-azure-networking-guidance.md).
* Jeśli masz usługi ExpressRoute lub połączenie sieci VPN między lokalnymi a lokalizacją źródłową na platformie Azure, [informacje o sposobie konfigurowania tych](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration).
* Wymaga konta użytkownika platformy Azure [określonych uprawnień](../site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines), aby włączyć replikację maszyny Wirtualnej platformy Azure.
Do tworzenia maszyn wirtualnych w lokalizacji docelowej, który ma być używany jako region odzyskiwania po awarii powinna być włączona subskrypcji platformy Azure. Skontaktuj się z pomocą techniczną, aby włączyć wymagane przydziału.

## <a name="enable-replication"></a>Włączanie replikacji

W tej procedurze Azja Wschodnia jest używany jako lokalizacji źródłowej i południowo Azja Wschodnia jako element docelowy.

1. Kliknij przycisk **+ Replikuj** w magazynie, aby włączyć replikację dla maszyn wirtualnych.
2. Sprawdź, czy **źródła:** ustawiono **Azure**.
3. Ustaw **lokalizacja źródłowa** Azja Wschodnia.
4. W **modelu wdrażania**, wybierz pozycję **klasycznego** lub **Resource Manager**.
5. W **grupy zasobów**, wybierz grupę, do której należy źródło maszyn wirtualnych. Wszystkie maszyny wirtualne w wybranej grupy zasobów są wyświetlane.

    ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

6. W **maszyny wirtualnej > Wybierz maszyny wirtualne**, kliknij i zaznacz każdej maszyny Wirtualnej, którą chcesz replikować. Możesz wybrać tylko te maszyny, dla których można włączyć replikację. Następnie kliknij przycisk **OK**.

    ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)
    
7. W obszarze **ustawienia** > **lokalizacji docelowej**, określ, do której są replikowane źródła danych maszyny Wirtualnej. Usługa Site Recovery zawiera listę regionów odpowiedniego elementu docelowego, w zależności od regionu wybranych maszyn wirtualnych.
8. Usługa Site Recovery ustawia domyślne ustawienia obiektu docelowego. Te można modyfikować zgodnie z wymaganiami.

    - **Docelowa grupa zasobów**. Domyślnie usługi Site Recovery tworzy nową grupę zasobów w obszarze docelowym wraz z sufiksem "asr". Jeśli grupa utworzony zasób już istnieje, zostanie on użyty ponownie.
    - **Wirtualne sieci docelowej**. Domyślnie usługa Site Recovery tworzy nową sieć wirtualną w regionie docelowej, wraz z sufiksem "asr". Ta sieć jest zamapowana na sieć źródła. [Dowiedz się więcej](site-recovery-network-mapping-azure-to-azure.md) o mapowania sieci.
    - **Docelowa kont magazynu**. Domyślnie usługa Site Recovery tworzy nowe konto magazynu docelowego odpowiadający konfiguracji magazynu maszyny Wirtualnej źródłowego. Jeśli istnieje już utworzonego konta, zostanie on użyty ponownie.
    - **Buforowanie kont magazynu**. Usługa Azure Site Recovery tworzy konto magazynu pamięci podręcznej dodatkowe region źródła. Wszystkie zmiany na źródłowe maszyny wirtualne są śledzone i wysyłane do konta magazynu pamięci podręcznej przed replikacji do lokalizacji docelowej.
    - **Zestaw dostępności**. Domyślnie usługa Site Recovery tworzy dostępność nowych ustawiony w regionie docelowych z sufiksem "asr". Jeśli utworzony zestaw już istnieje, zostanie on użyty ponownie.
    - **Zasady replikacji**. Usługa Site Recovery definiuje ustawienia historii przechowywania punktu odzyskiwania i częstotliwości migawki dotyczącej spójności aplikacji. Domyślnie usługa Site Recovery tworzy nowe zasady replikacji z ustawieniami domyślnymi 24 godzin przechowywania punktów odzyskiwania i 60 minut częstotliwości migawki dotyczącej spójności aplikacji.

    ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)
9. Kliknij przycisk **włączania replikacji** celu rozpoczęcia ochrony maszyn wirtualnych.

## <a name="customize-target-resources"></a>Dostosowywanie zasobami obiektów docelowych

1. Zmodyfikuj dowolny z tych wartości domyślnych docelowych:

    - **Docelowa grupa zasobów**. Wybierz dowolną grupę zasobów z listy wszystkich grup zasobów w lokalizacji docelowej, w ramach subskrypcji.
    - **Wirtualne sieci docelowej**. Wybierz z listy sieci wirtualnych w lokalizacji docelowej.
    - **Zestaw dostępności** ustawienia zestawy dostępności można dodawać tylko do maszyn wirtualnych znajdujących się w zestawie w regionie źródła.
    - **Docelowa kont magazynu**: Dodaj wszystkie konta, które jest dostępne.

        ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/customize.PNG)

2. Polecenie **tworzenie zasobu docelowego** > **włączania replikacji**. Podczas replikacji początkowej stan maszyny Wirtualnej może potrwać pewien czas, aby odświeżyć. Kliknij przycisk **Odśwież** można pobrać najnowszy stan.

    ![Włączanie replikacji](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)
    
3. Po maszyny wirtualne są chronione, Sprawdź kondycję maszyn wirtualnych **elementy replikowane**.



## <a name="next-steps"></a>Następne kroki
[Dowiedz się](../azure-to-azure-tutorial-dr-drill.md) jak uruchomić test trybu failover.

