---
title: "Replikowanie maszyn wirtualnych platformy Azure między regiony platformy Azure na potrzeby odzyskiwania po awarii: Azure do platformy Azure | Dokumentacja firmy Microsoft"
description: "Przedstawiono czynności wymagane do replikowania maszyn wirtualnych platformy Azure między regiony platformy Azure (Azure-Azure) z usługą Azure Site Recovery na potrzeby odzyskiwania po awarii."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: 9ca33057f6030fdcc233f6053fdc392d62f8f9f4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Replikowanie maszyn wirtualnych Azure między regionami z usługą Azure Site Recovery

>[!NOTE]
>
> Azure Site Recovery replikacji maszyn wirtualnych platformy Azure (VM) jest obecnie w przeglądzie.

W tym artykule opisano sposób replikowania maszyn wirtualnych platformy Azure między regiony platformy Azure przy użyciu [usługi Site Recovery](site-recovery-overview.md) w portalu Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="disaster-recovery-in-azure"></a>Odzyskiwanie po awarii na platformie Azure

Funkcje i możliwości wbudowanego infrastruktury platformy Azure przyczyniają się do strategii niezawodny i refs Dostępność zadań uruchamianych na maszynach wirtualnych platformy Azure. Istnieje jednak wiele przyczyn, czego potrzebujesz do planu odzyskiwania po awarii między regiony platformy Azure:

- Musisz spełnia wytycznych dotyczących zgodności określonych aplikacji i obciążeń, które wymagają ciągłość prowadzenia działalności biznesowej i strategia odzyskiwania (BCDR).
- Ma możliwość ochrony i odzyskiwania maszyn wirtualnych platformy Azure w zależności od decyzji biznesowych, a nie tylko oparte na wbudowanych funkcji platformy Azure.
- Należy przetestować tryb failover i odzyskiwania zgodnie z potrzebami działalności biznesowej i zgodności bez wpływu na produkcję.
- Musisz przełączyć się do regionu odzyskiwania w przypadku awarii i powrót po awarii do oryginalnego źródła region bezproblemowo.

Użyj usługi Site Recovery dla maszyny Wirtualnej Azure-Azure replikacji ułatwia wykonywanie tych zadań.


## <a name="why-use-site-recovery"></a>Dlaczego warto używać usługi Site Recovery?      

Usługa Site Recovery zapewnia prosty sposób replikowania maszyn wirtualnych platformy Azure między regionami:

- **Automatyczne wdrażanie**. W przeciwieństwie do modelu typu aktywny aktywny replikacji nie istnieje potrzeba infrastruktury kosztowne i złożone w regionie pomocniczym. Po włączeniu replikacji Site Recovery automatycznie tworzy wymagane zasoby w regionie docelowym na podstawie ustawień regionu źródła.
- **Kontrolowanie regionów**. Z usługą Site Recovery może replikować z dowolnym regionie na dowolnym regionie, w ramach kontynencie. To porównać z magazynu geograficznie nadmiarowego dostęp do odczytu, która asynchronicznie replikuje dane między standard [łączyć regionów](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) tylko. Dostęp do odczytu magazynu geograficznie nadmiarowego umożliwia dostęp tylko do odczytu do danych w obszarze docelowym.
- **Automatyczne replikacji**. Usługa Site Recovery zapewnia zautomatyzowane replikacji ciągłej. Tryb failover i powrotu po awarii może być uruchomiona za pomocą jednego kliknięcia.
- **RTO i RPO**. Odzyskiwanie lokacji korzysta z infrastruktury sieci platformy Azure, łączącego regiony, aby zachować bardzo niskich RTO i cel punktu odzyskiwania.
- **Testowanie**. Testowanie odzyskiwania po awarii można uruchomić z na żądanie testu pracy w trybie Failover, gdy potrzebne bez wpływu na Twoje obciążeń produkcyjnych lub trwającej replikacji.
- **Plany odzyskiwania**. Plany odzyskiwania służy do organizowania trybu failover i powrotu po awarii całej aplikacji uruchomionych na wiele maszyn wirtualnych. Funkcja planu odzyskiwania ma sformatowanego najwyższej jakości integracji z elementu runbook usługi Automatyzacja Azure.


## <a name="deployment-summary"></a>Podsumowanie wdrożenia

Poniżej przedstawiono podsumowanie co należy zrobić w celu skonfigurowania replikacji maszyn wirtualnych między regiony platformy Azure:

1. Utwórz magazyn usługi Recovery Services. Magazyn zawiera ustawienia konfiguracji i organizuje replikację.
2. Włącz replikację maszyn wirtualnych platformy Azure.
3. Uruchom test trybu failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.

>[!IMPORTANT]
>
> Możesz sprawdzić [macierzy obsługi replikacji maszyny Wirtualnej Azure](./site-recovery-support-matrix-azure-to-azure.md).

>[!IMPORTANT]
>
> Aby uzyskać informacje na temat konfigurowania wychodzące połączenie sieciowe wymagane dla maszyn wirtualnych Azure Site Recovery replikacji, zobacz [sieci wytycznych](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Przed rozpoczęciem

* Twoje konto użytkownika usługi Azure musi mieć niektórych [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) Aby włączyć replikację maszyny wirtualnej platformy Azure.
* Do tworzenia maszyn wirtualnych w lokalizacji docelowej, który ma być używany jako region odzyskiwania po awarii powinna być włączona subskrypcji platformy Azure. Skontaktuj się z pomocą techniczną, aby włączyć wymagane przydziału.

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> Zaleca się tworzenie magazynu usług odzyskiwania w miejscu, gdzie ma maszyny wirtualne do replikacji. Na przykład, jeśli lokalizacja docelowej centralną USA, utwórz magazyn w **środkowe stany USA**.

## <a name="enable-replication"></a>Włączanie replikacji

W **Magazyny usług odzyskiwania**, kliknij nazwę magazynu. W magazynie, kliknij przycisk **+ Replikuj** przycisku.

### <a name="step-1-configure-the-source"></a>Krok 1. Skonfiguruj źródło
1. W **źródła**, wybierz pozycję **Azure — wersja ZAPOZNAWCZA**.
2. W **lokalizacja źródłowa**, wybierz źródło region platformy Azure, w którym są uruchomione maszyny wirtualne.
3. Wybierz model wdrażania maszyn wirtualnych: **Resource Manager** lub **klasycznego**.
4. Wybierz **źródłowa grupa zasobów** dla maszyn wirtualnych Menedżera zasobów lub **usługi w chmurze** klasycznych maszyn wirtualnych.

    ![Skonfiguruj źródło](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>Krok 2. Wybierz maszyny wirtualne

* Wybierz maszyny wirtualne, chcesz replikować, a następnie kliknij przycisk **OK**.

    ![Wybierz maszyny wirtualne](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>Krok 3. Konfigurowanie ustawień

1. Kliknij, aby zastąpić domyślne ustawienia docelowego i określ ustawienia wyboru **Dostosuj**. Aby uzyskać więcej informacji, zobacz [dostosować zasoby docelowe](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Konfigurowanie ustawień](./media/site-recovery-azure-to-azure/settings.png)


2. Domyślnie usługa Site Recovery tworzy zasady replikacji, która przyjmuje migawki spójne z aplikacjami co 4 godziny i przechowuje punkty odzyskiwania przez 24 godziny. Aby utworzyć zasady z różnymi ustawieniami, kliknij przycisk **Dostosuj** obok **zasad replikacji**.

    ![Dostosuj zasady](./media/site-recovery-azure-to-azure/customize-policy.png)

3. Aby rozpocząć Inicjowanie obsługi zasobów docelowych, kliknij **Utwórz zasoby docelowe**. Inicjowanie obsługi administracyjnej ma minuty. Nie zamykaj bloku podczas inicjowania obsługi lub należy zacząć od początku.

4. Aby wyzwolić replikację wybranej maszyny wirtualnej, kliknij przycisk **włączyć replikację**.

5. Możesz śledzić postęp **Włącz ochronę** zadania w **ustawienia** > **zadania** > **zadania usługi Site Recovery**.

6. W **ustawienia** > **elementy replikowane**, można wyświetlić stan maszyn wirtualnych i postęp replikacji początkowej. Kliknij maszynę Wirtualną, aby przejść do jego ustawień.

## <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover

Po skonfigurowaniu wszystko, co uruchomić testowy tryb failover, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami:

1. Do pracy awaryjnej z jednego komputera, w **ustawienia** > **elementy replikowane**, kliknij maszynę Wirtualną **+ testowy tryb Failover** ikony.

2. Do trybu failover planu odzyskiwania w **ustawienia** > **plany odzyskiwania**, kliknij prawym przyciskiem myszy planu **testowy tryb Failover**. Aby utworzyć plan odzyskiwania, [wykonaj te instrukcje](site-recovery-create-recovery-plans.md). 

3. W **testowy tryb Failover**, wybierz docelową sieć wirtualna platformy Azure które maszyny wirtualne platformy Azure są połączone po pracy awaryjnej.

4. Aby uruchomić tryb failover, kliknij przycisk **OK**. Aby śledzić postęp, kliknij maszynę Wirtualną, aby otworzyć jego właściwości. Można też kliknąć przycisk **testowy tryb Failover** zadania w nazwie magazynu > **ustawienia** > **zadania** > **zadania usługi Site Recovery**.

5. Po zakończeniu pracy awaryjnej, replika maszyny Azure pojawia się w portalu Azure > **maszyn wirtualnych**. Upewnij się, że maszyna wirtualna jest odpowiedni rozmiar, który połączył się z odpowiednią siecią i że jest uruchomiona.

6. Aby usunąć maszyn wirtualnych, które zostały utworzone podczas testowania trybu failover, kliknij przycisk **oczyszczania testowy tryb failover** na elemencie replikowanych lub planu odzyskiwania. W **uwagi**, zarejestrować i zapisać wszelkie obserwacje związane z testowania trybu failover. 

[Dowiedz się więcej](site-recovery-test-failover-to-azure.md) o testowy tryb failover.


## <a name="next-steps"></a>Następne kroki

Po przetestowaniu wdrożenia:

- [Dowiedz się więcej](site-recovery-failover.md) o różnych typach trybu failover i sposobach ich uruchamiania.
- Dowiedz się więcej o [przy użyciu planów odzyskiwania](site-recovery-create-recovery-plans.md) zmniejszenie RTO.
