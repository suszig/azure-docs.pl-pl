---
title: "Konfigurowanie źródłowych i docelowych replikacji funkcji Hyper-V do platformy Azure (bez programu System Center VMM) z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Przedstawiono kroki, aby skonfigurować ustawienia źródłowe i docelowe dla replikacji maszyn wirtualnych funkcji Hyper-V do magazynu Azure z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: d2010d85-77fd-4dea-84f3-1c960ed4c63f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/22/2017
ms.author: raynew
ms.openlocfilehash: b38eb3a011d46f2239891ea1d1bcac2a4059a866
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-replication-to-azure"></a>Krok 8: Konfigurowanie źródłowych i docelowych replikacji funkcji Hyper-V w systemie Azure

W tym artykule opisano sposób konfigurowania ustawień źródłowym i docelowym, gdy Replikowanie lokalnych maszyn wirtualnych funkcji Hyper-V (bez programu System Center VMM) na platformie Azure, przy użyciu [usługi Azure Site Recovery](site-recovery-overview.md) w portalu Azure.

Opublikuj komentarze i pytania w dolnej części tego artykułu lub na [Forum usług odzyskiwania Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

Konfigurowanie lokacji funkcji Hyper-V, zainstaluj dostawcę usługi Azure Site Recovery i agent usług odzyskiwania Azure na hostach funkcji Hyper-V i zarejestrować witrynę w magazynie.

1. W **przygotowanie infrastruktury**, kliknij przycisk **źródła**. Aby dodać nową lokację funkcji Hyper-V jako kontener dla hostów funkcji Hyper-V lub klastry, kliknij przycisk **+ lokacja funkcji Hyper-V**.

    ![Konfiguracja źródła](./media/hyper-v-site-walkthrough-source-target/set-source1.png)
2. W **lokacji funkcji Hyper-V Utwórz**, określ nazwę lokacji. Następnie kliknij przycisk **OK**. Teraz, wybierz lokację, możesz utworzyć i kliknij przycisk **+ serwer funkcji Hyper-V** Aby dodać serwer do lokacji.

    ![Konfiguracja źródła](./media/hyper-v-site-walkthrough-source-target/set-source2.png)

3. W **Dodaj serwer** > **typ serwera**, sprawdź, czy **serwera funkcji Hyper-V** jest wyświetlany.

    - Upewnij się, że spełnia serwera funkcji Hyper-V, które chcesz dodać [wymagania wstępne](#on-premises-prerequisites)i jest w stanie uzyskać dostępu do określonych adresów URL.
    - Pobierz plik instalacyjny dostawcy usługi Azure Site Recovery. Możesz uruchomić ten plik, aby zainstalować agenta usług odzyskiwania i dostawcy na każdym hoście funkcji Hyper-V.

    ![Konfiguracja źródła](./media/hyper-v-site-walkthrough-source-target/set-source3.png)


## <a name="install-the-provider-and-agent"></a>Zainstalowany dostawca i agent

1. Uruchom plik Instalatora na każdym hoście, dodane do lokacji funkcji Hyper-V dostawcę. Jeśli instalujesz w klastrze funkcji Hyper-V, uruchom Instalatora na każdym węźle klastra. Instalowanie i rejestrowanie w każdym węźle klastra funkcji Hyper-V zapewnia ochronę maszyn wirtualnych, nawet w przypadku migrowania między węzłami.
2. W usłudze **Microsoft Update** można włączyć aktualizacje, aby aktualizacje dostawcy były instalowane zgodnie z zasadami Microsoft Update.
3. W obszarze **Instalacja** zaakceptuj lub zmodyfikuj domyślną lokalizację instalacji dostawcy i kliknij przycisk **Zainstaluj**.
4. W **ustawienia magazynu**, kliknij przycisk **Przeglądaj** wybierz pobrany plik klucza magazynu. Określ subskrypcję usługi Azure Site Recovery, nazwę magazynu i lokacji funkcji Hyper-V, do której należy serwer funkcji Hyper-V.

    ![Rejestracja serwera](./media/hyper-v-site-walkthrough-source-target/provider3.png)

5. W **ustawienia serwera Proxy**, określ, jak dostawca uruchomiony na hostach funkcji Hyper-V łączy się z usługi Azure Site Recovery za pośrednictwem Internetu.

    * Jeśli chcesz, aby dostawca łączył się bezpośrednio, wybierz opcję **Połącz bezpośrednio z usługą Azure Site Recovery bez serwera proxy**.
    * Jeśli istniejący serwer proxy wymaga uwierzytelniania lub chcesz użyć niestandardowego serwera proxy dla połączenia dostawcy, wybierz opcję **nawiązywanie połączenia z usługi Azure Site Recovery przy użyciu serwera proxy**.
    * Jeśli używasz serwera proxy:
        - Określ adres, port i poświadczenia
        - Upewnij się, że adresy URL opisane w [wymagania wstępne](#prerequisites) są dozwolone przez serwer proxy.

    ![Internet](./media/hyper-v-site-walkthrough-source-target/provider7.png)

6. Po zakończeniu instalacji kliknij przycisk **zarejestrować** Aby zarejestrować serwer w magazynie.

    ![Lokalizacja instalacji](./media/hyper-v-site-walkthrough-source-target/provider2.png)

7. Po zakończeniu rejestracji metadane z serwera funkcji Hyper-V są pobierane przez usługę Azure Site Recovery, a serwer jest wyświetlany w **infrastruktura usługi Site Recovery** > **hosty funkcji Hyper-V**.


## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

Określ konto magazynu Azure dla replikacji i sieć platformy Azure, z którą maszyny wirtualne Azure będą się łączyć po pracy awaryjnej.

1. Kliknij przycisk **przygotowanie infrastruktury** > **docelowej**.
2. Wybierz subskrypcji i grupy zasobów, w którym chcesz utworzyć maszynach wirtualnych Azure po pracy awaryjnej. Wybierz model wdrażania, który ma być używany na platformie Azure (Zarządzanie zasobu lub classic) dla maszyn wirtualnych.

3. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

    - Jeśli nie masz konta magazynu, kliknij przycisk **i magazyn** utworzyć wbudowanego konta Menedżera zasobów. 
    - Jeśli nie masz sieć platformy Azure, kliknij przycisk **+ sieć** utworzyć wbudowany sieci przy użyciu usługi Resource Manager.






## <a name="next-steps"></a>Następne kroki

Przejdź do [krok 9: Konfigurowanie zasad replikacji](hyper-v-site-walkthrough-replication.md)
