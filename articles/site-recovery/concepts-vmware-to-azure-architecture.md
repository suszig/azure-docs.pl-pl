---
title: "VMware do platformy Azure replikacji architektury w usłudze Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera omówienie składników i architektury używane podczas replikowania lokalnych maszyn wirtualnych VMware do platformy Azure z usługą Azure Site Recovery"
author: rayne-wiselman
ms.service: site-recovery
ms.topic: article
ms.date: 01/15/2018
ms.author: raynew
ms.openlocfilehash: 7999f23d167c6e8a7bcaf3a817e0cd2e80a1d649
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="vmware-to-azure-replication-architecture"></a>VMware do platformy Azure replikacji architektury

W tym artykule opisano architektura i procesy stosowane podczas replikacji, pracy awaryjnej i odzyskiwania maszyn wirtualnych VMware (VM) między lokacji oprogramowania VMware lokalną i platformą Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) usługi.


## <a name="architectural-components"></a>Składniki architektury

Następujące tabeli i grafika Podaj ogólny widok składniki używane do replikacji maszyn wirtualnych VMware do platformy Azure.  

**Składnik** | **Wymaganie** | **Szczegóły**
--- | --- | ---
**Azure** | Subskrypcja platformy Azure, konto magazynu Azure i sieć platformy Azure. | Replikowane dane z lokalnych maszyn wirtualnych są przechowywane na koncie magazynu. Maszyny wirtualne platformy Azure są tworzone przy użyciu zreplikowanych danych podczas uruchamiania błędu za pośrednictwem z lokalnej na platformie Azure. Maszyny wirtualne platformy Azure nawiązują połączenie z siecią wirtualną platformy Azure, gdy są tworzone.
**Komputer serwera konfiguracji** | Pojedynczy lokalnego komputera. Firma Microsoft zaleca się, że uruchamiasz go jako maszyny Wirtualnej VMware, które można wdrożyć z pobranego szablonu OVF.<br/><br/> Komputer uruchamia wszystkie składniki usługi Site Recovery lokalnie, w tym konfiguracji serwera, serwer przetwarzania i główny serwer docelowy. | **Serwer konfiguracji**: współrzędne komunikacji między lokalną i platformą Azure i zarządza replikacji danych.<br/><br/> **Serwer przetwarzania**: instalowana domyślnie na serwerze konfiguracji. Go odbiera dane replikacji, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania i wysyła go do magazynu Azure. Serwer przetwarzania instaluje usługi mobilności na maszynach wirtualnych, którą chcesz replikować, i przeprowadza automatyczne odnajdywanie komputerów lokalnych. Wraz z rozwojem wdrożenia, możesz dodać dodatkowe, oddzielne procesu serwerów do obsługi większych ilości ruchu replikacji.<br/><br/>  **Główny serwer docelowy**: instalowana domyślnie na serwerze konfiguracji. Obsługuje on replikację danych podczas powrotu po awarii z platformy Azure. W przypadku dużych wdrożeń można dodać dodatkowe, oddzielne główny serwer docelowy do powrotu po awarii.
**Serwery VMware** | Maszyny wirtualne VMware znajdują się na serwerach ESXi vSphere lokalnymi. Firma Microsoft zaleca serwer vCenter do zarządzania hostami. | Podczas wdrażania usługi Site Recovery należy dodać serwery VMware do magazynu usług odzyskiwania.
**Zreplikowane maszyny** | Usługa mobilności jest zainstalowana na każdej maszynie Wirtualnej VMware, które są replikowane. | Zaleca się Zezwalaj automatycznej instalacji z serwera przetwarzania. Możesz też ręcznie zainstalować usługę lub użyć metody wdrażania automatycznego, takie jak System Center Configuration Manager.

**Architektura VMware–Azure**

![Składniki](./media/concepts-vmware-to-azure-architecture/arch-enhanced.png)

## <a name="replication-process"></a>Proces replikacji

1.  Należy przygotować zasobów platformy Azure i lokalnymi składniki.
2.  W magazynie usług odzyskiwania należy określić ustawienia źródła replikacji. W ramach tego procesu można skonfigurować lokalnego serwera konfiguracji. Aby wdrożyć ten serwer jako maszyny Wirtualnej VMware, pobierz go przygotować szablon pakietu OVF, a go zaimportować do programu VMware można utworzyć maszyny Wirtualnej.
3. Określ ustawienia replikacji obiektu docelowego, Utwórz zasady replikacji i włączyć replikację dla maszyn wirtualnych VMware.
4.  Replikacja maszyny, zgodnie z zasadami replikacji i początkowej kopii danych maszyny Wirtualnej są replikowane do magazynu Azure.
5.  Po zakończeniu replikacji początkowej, rozpoczyna replikację zmian różnicowych na platformie Azure. Śledzone zmiany dla maszyny są przechowywane w pliku hrl.
    - Maszyny komunikacji z serwerem konfiguracji na porcie 443 protokołu HTTPS dla ruchu przychodzącego dla zarządzania replikacji.
    - Maszyny wysyłanie danych replikacji na serwer przetwarzania na porcie HTTPS 9443 dla ruchu przychodzącego (może być modyfikowany).
    - Serwer konfiguracji synchronizuje replikację z platformą Azure za pośrednictwem portu HTTPS 443 dla danych wychodzących.
    - Serwer przetwarzania odbiera dane z maszyn źródłowych, optymalizuje je i szyfruje, a następnie wysyła do usługi Azure Storage za pośrednictwem portu 443 dla danych wychodzących.
    - Włączenie spójności wielu maszyn wirtualnych, maszyny w grupie replikacji komunikują się ze sobą za pośrednictwem portu 20004. Funkcja spójności wielu maszyn wirtualnych jest używana, jeśli wiele maszyn zostanie połączonych w grupę replikacji, która współużytkuje punkty odzyskiwania spójne na poziomie awarii i aplikacji. Jest to przydatne, jeśli maszyny obsługują to samo obciążenie i muszą być spójne.
6.  Ruch są replikowane do magazynu Azure publiczne punkty końcowe, za pośrednictwem Internetu. Alternatywnie można użyć zure ExpressRoute [publicznej komunikacji równorzędnej](../expressroute/expressroute-circuit-peerings.md#azure-public-peering). Replikowanie ruchu za pośrednictwem sieci VPN typu lokacja-lokacja z lokacji lokalnej platformy Azure nie jest obsługiwane.


**VMware do procesu replikacji Azure**

![Proces replikacji](./media/concepts-vmware-to-azure-architecture/v2a-architecture-henry.png)

## <a name="failover-and-failback-process"></a>Proces pracy w trybie failover i podczas powrotu po awarii

Po skonfigurowaniu replikacji i uruchomieniu wyszczególniania odzyskiwania po awarii (test trybu failover) do sprawdzenia, czy wszystko działa zgodnie z oczekiwaniami, można uruchomić trybu failover i powrotu po awarii na potrzeby.

1. W tryb failover jednego komputera lub utworzyć plany odzyskiwania do trybu failover wiele maszyn wirtualnych.
2. Po uruchomieniu trybu failover, maszynach wirtualnych platformy Azure są tworzone na podstawie zreplikowane dane w magazynie Azure.
3. Po wyzwalają początkowej trybu failover, Zatwierdź go uruchomić uzyskiwania dostępu do obciążenia z maszyny Wirtualnej platformy Azure.

Po ponownym udostępnieniu głównej lokacji lokalnej można do niej powrócić po awarii.
1. Należy skonfigurować infrastrukturę powrotu po awarii, w tym:
    - **Tymczasowy serwer przetwarzania na platformie Azure**: aby awarii z platformy Azure, należy skonfigurować maszyny Wirtualnej platformy Azure do działania jako serwer przetwarzania, do obsługi replikacji z platformy Azure. Po zakończeniu powrotu po awarii można usunąć tę maszynę wirtualną.
    - **Połączenia sieci VPN**: aby wykonaj powrót po awarii, należy połączenie sieci VPN (lub usługa Azure ExpressRoute) z sieci platformy Azure do lokacji lokalnej.
    - **Oddzielne główny serwer docelowy**: Domyślnie główny serwer docelowy, który został zainstalowany na serwerze konfiguracji w programie VMware lokalnej maszyny Wirtualnej, obsługuje powrót po awarii. Jednak niepowodzenie wstecz dużych ilości ruchu należy należy skonfigurować oddzielny lokalny główny serwer docelowy do tego celu.
    - **Zasady powrotu po awarii**: aby móc przeprowadzić ponowną replikację do lokacji lokalnej, należy utworzyć zasady powrotu po awarii. To został utworzony automatycznie po utworzeniu zasad replikacji z lokalnej na platformie Azure.
2. Po składniki zostały spełnione, powrotu po awarii w trzech etapach:
    - Etap 1: Włącz ponownie ochronę maszyn wirtualnych platformy Azure, dzięki czemu mają być replikowane z platformy Azure do maszyn wirtualnych VMware lokalnymi.
    - Etap 2: Uruchomić trybu failover do lokacji lokalnej.
    - Etap 3: Po obciążeń powiodły się z powrotem, należy ponownie włączyć replikowania lokalnych maszyn wirtualnych.

**VMware powrotu po awarii z platformy Azure**

![Powrót po awarii](./media/concepts-vmware-to-azure-architecture/enhanced-failback.png)


## <a name="next-steps"></a>Kolejne kroki

Postępuj zgodnie z [w tym samouczku](tutorial-vmware-to-azure.md) umożliwiające VMware do platformy Azure replikacji.
