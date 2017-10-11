---
title: "Uruchom test trybu failover dla replikacji maszyny Wirtualnej platformy Azure z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Zawiera podsumowanie kroków potrzebnych do uruchamiania testowy tryb failover do replikowania maszyn wirtualnych platformy Azure do innego regionu Azure przy użyciu usługi Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: e15c1b0c-5d75-4fdf-acb0-e61def9e9339
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: raynew
ms.openlocfilehash: 8babb0d016729f318442af93596d206c38d91206
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="step-6-run-a-test-failover-for-azure-vm-replication"></a>Krok 6: Uruchom test trybu failover dla replikacji maszyny Wirtualnej Azure

Po włączeniu replikacji dla maszyny wirtualnej platformy Azure (maszyny wirtualne), wykonaj kroki opisane w tym artykule, aby uruchomić test trybu failover z jednego regionu Azure do drugiego za pomocą [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

- Po zakończeniu tego artykułu, powinien potwierdził test trybu failover, że co najmniej jednej maszyny Wirtualnej platformy Azure może przełączyć dodatkowej regionu Azure. 
- Opublikuj wszelkie komentarze u dołu w tym artykule, lub zadać pytania w [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)

>[!NOTE]
>
> Replikacja maszyny Wirtualnej platformy Azure jest obecnie w przeglądzie.


## <a name="before-you-start"></a>Przed rozpoczęciem

- Przed uruchomieniem test trybu failover zaleca się zweryfikować właściwości maszyny Wirtualnej, a wprowadzone zmiany, które należy. można uzyskać dostępu do właściwości maszyny Wirtualnej w **elementy replikowane**. **Essentials** blok zawiera informacje o ustawieniach maszyn i stanu.
- Zalecane jest użycie oddzielnej sieci maszyny Wirtualnej platformy Azure do testowania trybu failover, a nie do sieci (domyślne lub dostosować) który zostało skonfigurowane do pracy awaryjnej w środowisku produkcyjnym.
- Test trybu failover nie powiedzie się na maszynach wirtualnych platformy Azure (wraz z ich magazynem) w regionie pomocniczym platformy Azure. Go nie są replikowane wszystkich zależnych aplikacji lub zasobów. Jeśli aplikacje działające na nie powiodło się przez maszyny wirtualne są zależne inne zasoby, takie jak usługi Active Directory i DNS, należy replikować je, jeśli ich nie jest już dostępne w danym regionie pomocniczym. [Dowiedz się więcej](site-recovery-test-failover-to-azure.md#prepare-active-directory-and-dns)
- Jeśli chcesz uzyskać dostęp replikowanych maszyn wirtualnych po przejściu w tryb failover z lokacji lokalnej, należy [przygotować się do połączenia](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover) do tych maszyn wirtualnych.

## <a name="run-a-test-failover"></a>Wykonywanie próby przejścia w tryb failover

1. W **ustawienia** > **elementy replikowane**, kliknij maszynę Wirtualną **+ testowy tryb Failover** ikony. 

2. W **testowy tryb Failover**, wybierz punkt odzyskiwania do użycia dla trybu failover:

    - **Najnowsze przetworzone**: awaryjnie maszyny Wirtualnej do ostatniego punktu odzyskiwania, który był przetwarzany przez usługę Site Recovery. Sygnatura czasowa jest wyświetlany. Ta opcja nie jest czas przetwarzania danych, dzięki czemu oferuje RTO niski (celu czasu odzyskiwania).
    - **Najnowsza wersja aplikacji spójne**: Ta opcja nie powiedzie się za pośrednictwem wszystkich maszyn wirtualnych do ostatniego punktu odzyskiwania zapewniających spójność aplikacji. Sygnatura czasowa jest wyświetlany. 
    - **Niestandardowe**: wybrać dowolny punkt odzyskiwania.
 
3. Wybierz docelową Azure sieć wirtualną z którą maszyny wirtualne Azure w regionie pomocniczym zostaną połączone, po pracy awaryjnej.
4. Aby uruchomić tryb failover, kliknij przycisk **OK**. Aby śledzić postęp, kliknij maszynę Wirtualną, aby otworzyć jego właściwości. Możesz też kliknąć przycisk **testowy tryb Failover** zadania w nazwie magazynu > **ustawienia** > **zadania** > **zadania usługi Site Recovery**.
5. Po zakończeniu pracy awaryjnej, replika maszyny Wirtualnej platformy Azure jest wyświetlana w portalu Azure > **maszyn wirtualnych**. Upewnij się, że maszyna wirtualna jest odpowiedni rozmiar, który połączył się z odpowiednią siecią i że jest uruchomiona.
6. Aby usunąć maszyn wirtualnych, które zostały utworzone podczas testowania trybu failover, kliknij przycisk **oczyszczania testowy tryb failover** na elemencie replikowanych lub planu odzyskiwania. W **uwagi**, zarejestrować i zapisać wszelkie obserwacje związane z testowania trybu failover. 

[Dowiedz się więcej](site-recovery-test-failover-to-azure.md) o testowy tryb failover.

## <a name="next-steps"></a>Następne kroki

Po przetestowaniu trybu failover, w tym przewodniku została ukończona. Teraz Dowiedz się więcej o uruchamianiu przechodzenia w tryb failover w środowisku produkcyjnym:

- [Dowiedz się więcej](site-recovery-failover.md) o różnych typach trybu failover i sposobu ich uruchamiania.
- Dowiedz się więcej o niepowodzeniu przez wiele maszyn wirtualnych [przy użyciu planu odzyskiwania](site-recovery-create-recovery-plans.md).
- Dowiedz się więcej o [przy użyciu planów odzyskiwania](site-recovery-create-recovery-plans.md).
- Dowiedz się więcej o [ponownej ochrony maszyn wirtualnych platformy Azure](site-recovery-how-to-reprotect.md) po pracy awaryjnej.

