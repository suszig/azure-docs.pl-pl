---
title: "Migrowanie lokalnych maszyn do platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób migracji maszyn lokalnych do platformy Azure, za pomocą usługi Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/07/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: ee9397406cbca21d8bd53019d9daac5a037f508c
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="migrate-on-premises-machines-to-azure"></a>Migrowanie maszyn lokalnych do platformy Azure

Oprócz używania [usługi Azure Site Recovery](site-recovery-overview.md) usługi do zarządzania i organizowania odzyskiwania po awarii maszyn lokalnych i maszyn wirtualnych platformy Azure na potrzeby ciągłość prowadzenia działalności biznesowej i odzyskiwania po awarii (BCDR), możesz również użyć witryny Odzyskiwanie do zarządzania migracji maszyn lokalnych do platformy Azure.


Ten samouczek pokazuje, jak przeprowadzić migrację lokalnych maszyn wirtualnych i serwerów fizycznych do platformy Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wybierz cel replikacji
> * Konfigurowanie środowiska źródłowego i docelowego
> * Konfigurowanie zasad replikacji
> * Włączanie replikacji
> * Uruchom migrację test, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami
> * Uruchom jednorazowe trybu failover na platformie Azure

To jest trzeci samouczek w serii. Ten samouczek zakłada, zostały już wykonane zadania w poprzednim samouczki:

1. [Przygotowywanie platformy Azure](tutorial-prepare-azure.md)
2. Przygotuj lokalną [VMware](tutorial-prepare-on-premises-vmware.md) lub serwery funkcji Hyper-V.

Przed rozpoczęciem warto przejrzeć [VMware](concepts-vmware-to-azure-architecture.md) lub [funkcji Hyper-V](concepts-hyper-v-to-azure-architecture.md) architektury odzyskiwania po awarii.


## <a name="prerequisites"></a>Wymagania wstępne

Wyeksportowane przez parawirtualnego systemu sterowników urządzeń nie są obsługiwane.


## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Kliknij pozycję **Nowy** > **Monitorowanie i zarządzanie** > **Backup and Site Recovery**.
3. W **nazwa**, określ przyjazną nazwę **ContosoVMVault**. Jeśli masz więcej niż jedną subskrypcję, wybierz odpowiedni.
4. Utwórz grupę zasobów **ContosoRG**.
5. Określ region platformy Azure. Aby sprawdzić obsługiwane regiony, zobacz sekcję dotyczącą dostępności geograficznej w temacie [Szczegóły cennika usługi Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Aby szybko uzyskać dostęp do magazynu z pulpitu nawigacyjnego, kliknij przycisk **Przypnij do pulpitu nawigacyjnego** , a następnie kliknij przycisk **Utwórz**.

   ![Nowy magazyn](./media/tutorial-migrate-on-premises-to-azure/onprem-to-azure-vault.png)

Nowy magazyn zostanie dodany do **pulpitu nawigacyjnego** w obszarze **wszystkie zasoby**i w głównym **Magazyny usług odzyskiwania** strony.



## <a name="select-a-replication-goal"></a>Wybierz cel replikacji

Wybierz, co chcesz replikować, i miejsce, do którego chcesz przeprowadzać replikację.
1. Kliknij przycisk **Magazyny usług odzyskiwania** > magazynu.
2. W Menu zasobów kliknij **usługi Site Recovery** > **przygotowanie infrastruktury** > **cel ochrony**.
3. W **cel ochrony**, wybierz chcesz przeprowadzić migrację.
    - **VMware**: Wybierz **do platformy Azure** > **tak, z programem VMWare vSphere Hypervisor**.
    - **Komputer fizyczny**: Wybierz **do platformy Azure** > **nie zwirtualizowanych/inne**.
    - **Funkcja Hyper-V**: Wybierz **Azure** > **tak, z funkcją Hyper-V**. Jeśli program VMM zarządza maszynami wirtualnymi funkcji Hyper-V, wybierz **tak**.


## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

- [Konfigurowanie](tutorial-vmware-to-azure.md#set-up-the-source-environment) środowiska źródłowego dla maszyn wirtualnych VMware.
- [Konfigurowanie](tutorial-physical-to-azure.md#set-up-the-source-environment) środowiska źródłowego dla serwerów fizycznych.
- [Konfigurowanie](tutorial-hyper-v-to-azure.md#set-up-the-source-environment) środowiska źródłowego dla maszyn wirtualnych funkcji Hyper-V.

## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

Wybierz i zweryfikować zasobów docelowych.

1. Kliknij kolejno pozycje **Przygotowywanie infrastruktury** > **Docelowa** i wybierz subskrypcję platformy Azure do użycia.
2. Określ model wdrożenia usługi Resource Manager.
3. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

## <a name="set-up-a-replication-policy"></a>Konfigurowanie zasad replikacji

- [Konfigurowanie zasad replikacji](tutorial-vmware-to-azure.md#create-a-replication-policy) w maszynach wirtualnych VMware.
- [Konfigurowanie zasad replikacji](tutorial-physical-to-azure.md#create-a-replication-policy) dla serwerów fizycznych.
- [Konfigurowanie zasad replikacji](tutorial-hyper-v-to-azure.md#set-up-a-replication-policy) dla maszyn wirtualnych funkcji Hyper-V.


## <a name="enable-replication"></a>Włączanie replikacji

- [Włącz replikację](tutorial-vmware-to-azure.md#enable-replication) w maszynach wirtualnych VMware.
- [Włącz replikację](tutorial-physical-to-azure.md#enable-replication) dla serwerów fizycznych.
- [Włącz replikację](tutorial-hyper-v-to-azure.md#enable-replication) dla maszyn wirtualnych funkcji Hyper-V.


## <a name="run-a-test-migration"></a>Uruchom migrację, testu

Uruchom [testowanie trybu failover](tutorial-dr-drill-azure.md) na platformie Azure, aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.


## <a name="migrate-to-azure"></a>Migracja na platformę Azure

Uruchom tryb failover dla maszyn, które chcesz migrować.

1. W **ustawienia** > **elementy replikowane** kliknij maszynę > **pracy awaryjnej**.
2. W **pracy awaryjnej** wybierz **punkt odzyskiwania** do trybu failover. Wybierz najnowszy punkt odzyskiwania.
3. Ustawienie klucza szyfrowania nie jest odpowiedni dla tego scenariusza.
4. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej**. Odzyskiwanie lokacji będzie podejmować próby czy zamykania maszyn wirtualnych źródła aby mogło nastąpić wyzwolenie pracy awaryjnej. Tryb failover trwa nawet w przypadku zamknięcia nie powiedzie się. Możesz śledzić postęp trybu failover **zadania** strony.
5. Sprawdź, czy maszyny Wirtualnej Azure jest wyświetlany na platformie Azure, zgodnie z oczekiwaniami.
6. W **elementy replikowane**, kliknij prawym przyciskiem myszy maszyny Wirtualnej > **przeprowadzenia migracji**. To kończy proces migracji, zatrzymania replikacji dla maszyny Wirtualnej i zatrzymuje rozliczeń usługi Site Recovery dla maszyny Wirtualnej.

    ![Kończenie migracji](./media/tutorial-migrate-on-premises-to-azure/complete-migration.png)


> [!WARNING]
> **Nie Anuluj trybu failover w toku**: zatrzymania replikacji maszyny Wirtualnej przed rozpoczęciem pracy awaryjnej. Jeśli anulujesz trybu failover w toku, zatrzymuje trybu failover, ale maszyna wirtualna nie będą replikowane ponownie.

W niektórych scenariuszach pracy awaryjnej wymaga dodatkowego przetwarzania pobierającej około 8-10 minut, aby zakończyć. Można zauważyć już test czas pracy awaryjnej dla serwerów fizycznych, VMware Linux maszyn maszyn wirtualnych VMware, które nie mają umożliwia usługi DHCP i maszyny wirtualne VMware, które nie mają następujące sterowniki rozruchu: storvsc, magistralę maszyny wirtualnej, storflt, intelide, atapi.


## <a name="next-steps"></a>Kolejne kroki

W tym samouczku zostały zmigrowane lokalnych maszyn wirtualnych do maszyn wirtualnych platformy Azure. Teraz możesz skonfigurować odzyskiwania po awarii dla maszyn wirtualnych platformy Azure.

> [!div class="nextstepaction"]
> [Konfigurowanie odzyskiwania po awarii](site-recovery-azure-to-azure-after-migration.md) dla maszyn wirtualnych Azure po zakończeniu migracji z lokacji lokalnej.
