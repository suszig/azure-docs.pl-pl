---
title: "Migrowanie lokalnych maszyn do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób migracji maszyn lokalnych do platformy Azure, za pomocą usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: ddb412fd-32a8-4afa-9e39-738b11b91118
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/27/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: e268a6d53a9f0b001ad0da8c9ce7ea1a9046960c
ms.sourcegitcommit: 29bac59f1d62f38740b60274cb4912816ee775ea
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrowanie maszyn lokalnych do platformy Azure

[Usługi Azure Site Recovery](../site-recovery-overview.md) usługi zarządza i organizuje replikację, tryb failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM).

Ten samouczek pokazuje, jak przeprowadzić migrację lokalnych maszyn wirtualnych i serwerów fizycznych do platformy Azure z usługą Site Recovery. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wymagań wstępnych dotyczących wdrożenia
> * Tworzenie magazynu usług odzyskiwania Site Recovery
> * Wdrażanie serwerów zarządzania lokalnymi
> * Skonfiguruj zasady replikacji i włączyć replikację
> * Uruchom wyszczególniania odzyskiwania po awarii, aby upewnić się, wszystko działa
> * Uruchom jednorazowe trybu failover na platformie Azure

## <a name="overview"></a>Omówienie

Można dokonać migracji maszyny przez włączenie dla niego replikację i awaryjne na platformie Azure.


## <a name="prerequisites"></a>Wymagania wstępne

Oto, co należy zrobić, w tym samouczku.

- [Przygotowanie](../tutorial-prepare-azure.md) zasobów platformy Azure, w tym subskrypcji platformy Azure, sieć wirtualną platformy Azure i konto magazynu.
- [Przygotowanie](../tutorial-prepare-on-premises-vmware.md) VMware lokalnych serwerów VMware i maszyn wirtualnych.
- Należy pamiętać, że wyeksportowane przez parawirtualnego systemu sterowników urządzeń nie są obsługiwane.


## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

[!INCLUDE [site-recovery-create-vault](../../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Wybierz cel ochrony

Wybierz, co chcesz replikować, i miejsce, do którego chcesz przeprowadzać replikację.
1. Kliknij przycisk **Magazyny usług odzyskiwania** > magazynu.
2. W Menu zasobów kliknij **usługi Site Recovery** > **przygotowanie infrastruktury** > **cel ochrony**.
3. W **cel ochrony**, wybierz opcję:
    - **VMware**: Wybierz **do platformy Azure** > **tak, z programem VMWare vSphere Hypervisor**.
    - **Komputer fizyczny**: Wybierz **do platformy Azure** > **nie zwirtualizowanych/inne**.
    - **Funkcja Hyper-V**: Wybierz **Azure** > **tak, z funkcją Hyper-V**.


## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

- [Konfigurowanie](../tutorial-vmware-to-azure.md#set-up-the-source-environment) środowiska źródłowego dla maszyn wirtualnych VMware.
- [Konfigurowanie](../tutorial-physical-to-azure.md#set-up-the-source-environment) środowiska źródłowego dla serwerów fizycznych.
- [Konfigurowanie](../tutorial-hyper-v-to-azure.md#set-up-the-source-environment) środowiska źródłowego dla maszyn wirtualnych funkcji Hyper-V.

## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

Wybierz i zweryfikować zasobów docelowych.

1. Kliknij kolejno pozycje **Przygotowywanie infrastruktury** > **Docelowa** i wybierz subskrypcję platformy Azure do użycia.
2. Określ model wdrożenia docelowego.
3. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

## <a name="create-a-replication-policy"></a>Tworzenie zasad replikacji

- [Konfigurowanie zasad replikacji](../tutorial-vmware-to-azure.md#create-a-replication-policy) w maszynach wirtualnych VMware.


## <a name="enable-replication"></a>Włączanie replikacji

- [Włącz replikację](../tutorial-vmware-to-azure.md#enable-replication) w maszynach wirtualnych VMware.


## <a name="run-a-test-migration"></a>Uruchom migrację, testu

Uruchom [testowanie trybu failover](../tutorial-dr-drill-azure.md) na platformie Azure, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.


## <a name="migrate-to-azure"></a>Migracja na platformę Azure

Uruchom tryb failover dla maszyn, które chcesz migrować.

1. W **ustawienia** > **elementy replikowane** kliknij maszynę > **pracy awaryjnej**.
2. W **pracy awaryjnej** wybierz **punkt odzyskiwania** do trybu failover. Wybierz najnowszy punkt odzyskiwania.
3. Ustawienie klucza szyfrowania nie jest odpowiedni dla tego scenariusza.
4. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej** Jeśli chcesz, aby usługi Site Recovery, aby spróbować wykonać zamykania maszyn wirtualnych źródła przed wyzwolenie pracy awaryjnej. Tryb failover trwa nawet w przypadku zamknięcia nie powiedzie się. Możesz śledzić postęp trybu failover **zadania** strony.
5. Sprawdź, czy maszyny Wirtualnej Azure jest wyświetlany na platformie Azure, zgodnie z oczekiwaniami.
6. W **elementy replikowane**, kliknij prawym przyciskiem myszy maszyny Wirtualnej > **przeprowadzenia migracji**. To kończy proces migracji, zatrzymania replikacji dla maszyny Wirtualnej i zatrzymuje rozliczeń usługi Site Recovery dla maszyny Wirtualnej.

    ![Kończenie migracji](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Nie Anuluj trybu failover w toku**: zatrzymania replikacji maszyny Wirtualnej przed rozpoczęciem pracy awaryjnej. Jeśli anulujesz trybu failover w toku, zatrzymuje trybu failover, ale maszyna wirtualna nie będą replikowane ponownie.

W niektórych scenariuszach pracy awaryjnej wymaga dodatkowego przetwarzania pobierającej około 8-10 minut, aby zakończyć. Można zauważyć już test czas pracy awaryjnej dla serwerów fizycznych, VMware Linux maszyn maszyn wirtualnych VMware, które nie mają umożliwia usługi DHCP i maszyny wirtualne VMware, które nie mają następujące sterowniki rozruchu: storvsc, magistralę maszyny wirtualnej, storflt, intelide, atapi.


## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Replikowanie maszyn wirtualnych platformy Azure do innego regionu po migracji na platformie Azure](site-recovery-azure-to-azure-after-migration.md)
