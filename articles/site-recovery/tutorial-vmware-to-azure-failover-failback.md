---
title: "Tryb failover, a kończyć się niepowodzeniem z powrotem maszyn wirtualnych VMware i serwery fizyczne replikowane do platformy Azure z usługą Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak działają maszyny wirtualne VMware i serwerów fizycznych do platformy Azure i powrót po awarii do lokacji lokalnej, z usługą Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/01/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 28a14a9b28dfe9c2014add9b9f691bce6ba91a4c
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/01/2017
---
# <a name="fail-over-and-fail-back-vmware-vms-and-physical-servers-replicated-to-azure"></a>Tryb failover, a kończyć się niepowodzeniem z powrotem maszyn wirtualnych VMware i serwery fizyczne replikowane do platformy Azure

W tym samouczku opisano w tryb failover maszyny Wirtualnej VMware do platformy Azure. Po przez Ciebie nie powiodła się, możesz powrót po awarii do lokacji sieci lokalnej, gdy jest ona dostępna. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Sprawdź, czy właściwości maszyny Wirtualnej VMware, aby sprawdzić, który jest zgodny z wymaganiami platformy Azure
> * Uruchom tryb failover na platformie Azure
> * Tworzenie serwera przetwarzania i główny serwer docelowy do powrotu po awarii
> * Włącz ponownie ochronę maszyn wirtualnych platformy Azure do lokacji lokalnej
> * Przełączyć się z platformy Azure do środowiska lokalnego
> * Włącz ponownie ochronę lokalnych maszyn wirtualnych, aby uruchomić ponownie replikację do platformy Azure

To jest piąty samouczek w serii. Ten samouczek zakłada, zostały już wykonane zadania w poprzednim samouczki.

1. [Przygotowywanie platformy Azure](tutorial-prepare-azure.md)
2. [Przygotowywanie lokalnego wdrożenia oprogramowania VMware](tutorial-prepare-on-premises-vmware.md)
3. [Konfigurowanie odzyskiwania po awarii](tutorial-vmware-to-azure.md)
4. [Uruchamianie próbnego odzyskiwania po awarii](tutorial-dr-drill-azure.md)

## <a name="preparing-for-failover-and-failback"></a>Przygotowanie do trybu failover i powrotu po awarii

Upewnij się, że nie ma żadnych migawek na maszynie Wirtualnej. Lokalnej maszyny Wirtualnej jest wyłączony podczas zastosowania.
Pomaga to zapewnić spójność danych podczas replikacji. Nigdy nie włączaj maszyny Wirtualnej po zakończeniu zastosowania. Użyj tego samego serwera głównego celu włączyć ją ponownie do grupy replikacji. Użycie innego głównego serwera docelowego do Włącz ponownie ochronę grupy replikacji, serwer nie może dostarczyć wspólnego punktu w czasie.

Tryb failover i powrotu po awarii składa się z czterech etapów:

1. **Awaryjnie Azure**: awaryjnie maszyny z lokacji lokalnej na platformie Azure.
2. **Włącz ponownie ochronę maszyny wirtualne Azure**: Włącz ponownie ochronę maszyn wirtualnych platformy Azure, aby rozpoczynały się replikację z powrotem do maszyn wirtualnych VMware lokalnymi.
3. **Przełączyć się na lokalnych**: tryb failover, awarii z platformy Azure.
4. **Ponownej ochrony maszyn wirtualnych lokalnymi**: po danych nie powiodło się ponownie, włącz ponownie ochronę lokalnych maszyn wirtualnych, których nie powiodła się wstecz do aby rozpoczynały się replikację do platformy Azure.

## <a name="verify-vm-properties"></a>Sprawdź właściwości maszyny Wirtualnej

Sprawdź właściwości maszyny Wirtualnej, a następnie upewnij się, że maszyna wirtualna jest zgodna z [wymagania dotyczące usługi Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

1. W **chronione elementy**, kliknij przycisk **elementy replikowane** > maszyny Wirtualnej.

2. W **elementu zreplikowane** okienku znajduje się podsumowanie informacji o maszyny Wirtualnej, stan kondycji i najnowsze dostępne punkty odzyskiwania. Kliknij przycisk **właściwości** można zobaczyć więcej szczegółów.

3. W **obliczenia i sieć**, możesz zmodyfikować nazwę platformy Azure, grupy zasobów, rozmiar docelowy [zestawu dostępności](../virtual-machines/windows/tutorial-availability-sets.md), i [zarządzane ustawienia dysku](#managed-disk-considerations)

4. Można wyświetlać i modyfikować ustawień sieciowych, takich jak sieć/podsieć, w którym maszyny Wirtualnej Azure zostaną umieszczone po pracy awaryjnej i adres IP, który zostanie przypisany do niej.

5. W **dysków**, można wyświetlić informacje dotyczące systemu operacyjnego i dysków z danymi na Maszynie wirtualnej.

## <a name="run-a-failover-to-azure"></a>Uruchom tryb failover na platformie Azure

1. W **ustawienia** > **elementy replikowane** kliknij maszynę Wirtualną > **pracy awaryjnej**.

2. W **pracy awaryjnej** wybierz **punkt odzyskiwania** do trybu failover. Można użyć jednej z następujących opcji:
   - **Najnowsze** (domyślnie): Ta opcja najpierw przetwarza wszystkie dane wysyłane do usługi Site Recovery. Ponieważ maszyny Wirtualnej platformy Azure utworzonych po trybu failover ma wszystkie dane, które zostało replikowane do usługi Site Recovery pracę w trybie failover zostało wyzwolone zapewnia najniższy RPO (cel punktu odzyskiwania).
   - **Najnowsze przetworzone**: Ta opcja nie powiedzie się za pośrednictwem maszyn wirtualnych do ostatniego punktu odzyskiwania przetworzone przez usługę Site Recovery. Ta opcja umożliwia RTO niski (celu czasu odzyskiwania), ponieważ nie jest czas przetwarzania nieprzetworzonych danych.
   - **Najnowsza wersja aplikacji spójne**: Ta opcja nie powiedzie się na maszynie Wirtualnej, aby punkt najnowszych odzyskiwania zapewniających spójność aplikacji przetworzone przez usługę Site Recovery.
   - **Niestandardowe**: Określ punkt odzyskiwania.

3. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej** próbuje wykonać zamykania maszyn wirtualnych źródła przed wyzwolenie pracy awaryjnej. Tryb failover trwa nawet w przypadku zamknięcia nie powiedzie się. Możesz śledzić postęp trybu failover **zadania** strony.

4. Jeśli przygotowanym połączenie z maszyną Wirtualną Azure, połącz się go zweryfikować po przejściu w tryb failover.

5. Po sprawdzeniu, **zatwierdzić** pracy awaryjnej. Spowoduje to usunięcie wszystkich punktów odzyskiwania.

> [!WARNING]
> **Nie Anuluj trybu failover w toku**: przed uruchomieniem trybu failover zostanie zatrzymana replikację maszyny Wirtualnej.
> Jeśli anulujesz trybu failover w toku, zatrzymuje trybu failover, ale maszyna wirtualna nie będą replikowane ponownie.

W niektórych scenariuszach pracy awaryjnej wymaga dodatkowego przetwarzania pobierającej około 8-10 minut, aby zakończyć. Można zauważyć już test czas pracy awaryjnej dla serwerów fizycznych, VMware Linux maszyn maszyn wirtualnych VMware, które nie mają umożliwia usługi DHCP i maszyny wirtualne VMware, które nie mają następujące sterowniki rozruchu: storvsc, magistralę maszyny wirtualnej, storflt, intelide, atapi.

## <a name="create-a-process-server-in-azure"></a>Tworzenie serwera przetwarzania na platformie Azure

Serwer przetwarzania odbiera dane z maszyny Wirtualnej Azure, a następnie wysyła je do lokacji lokalnej. Sieciach z małym opóźnieniami jest wymagany między serwerem przetwarzania i chronione maszyny Wirtualnej.

- Do celów testowych Jeśli masz połączenie Azure ExpressRoute, można użyć lokalnego serwera procesu, który jest automatycznie instalowany na serwerze konfiguracji.
- Jeśli masz połączenie sieci VPN lub używasz powrotu po awarii w środowisku produkcyjnym, należy skonfigurować maszyny Wirtualnej platformy Azure jako serwera opartego na platformie Azure do powrotu po awarii.
- Aby skonfigurować serwer przetwarzania na platformie Azure, postępuj zgodnie z instrukcjami [w tym artykule](site-recovery-vmware-setup-azure-ps-resource-manager.md).

## <a name="configure-the-master-target-server"></a>Skonfigurować główny serwer docelowy

Domyślnie główny serwer docelowy jest uruchamiane na serwerze konfiguracji lokalnej. Na potrzeby tego samouczka używamy Domyślny wzorzec. Główny serwer docelowy odbiera dane powrotu po awarii.

W przypadku maszyny Wirtualnej na hoście ESXi, który jest zarządzany przez serwer vCenter, główny serwer docelowy musi mieć dostęp do magazynu danych maszyny Wirtualnej (VMDK), można zapisać danych replikowanych do dysków maszyny Wirtualnej. Upewnij się, że maszyna wirtualna magazynu danych został zainstalowany na hoście główny cel z dostępem do odczytu/zapisu.

Jeśli maszyna wirtualna znajduje się na ESXi, który nie jest zarządzany przez serwer vCenter, Usługa Site Recovery tworzy nową maszynę Wirtualną podczas zastosowania. Maszyna wirtualna jest tworzony na hoście ESX, na którym utworzyć głównego celu.
Dysk twardy maszyny wirtualnej musi być w magazynie danych, który jest dostępny dla hosta, na którym uruchomiony jest główny serwer docelowy.

Jeśli maszyna wirtualna nie używa vCenter, należy zakończyć odnajdywania hosta, na którym jest uruchomiona główny serwer docelowy, zanim zostanie Włącz ponownie ochronę maszyny. Dotyczy to zbyt niepowodzeniem ponownie fizycznych serwerów. Innym rozwiązaniem, jeśli istnieje lokalne maszyny Wirtualnej, jest go usunąć przed wykonaniem powrotu po awarii. Powrót po awarii wynikiem jest utworzenie nowej maszyny Wirtualnej na tym samym hoście jako główny serwer docelowy host ESX. Gdy użytkownik powrót po awarii do alternatywnej lokalizacji, dane zostaną odzyskane do tego samego magazynu danych i tego samego hosta ESX, który jest używany przez lokalny główny serwer docelowy.

Nie można użyć narzędzia Storage vMotion na głównym serwerze docelowym. Jeśli to zrobisz, nie będzie działać powrotu po awarii, ponieważ dyski nie są dostępne. Wyklucz serwery docelowe głównej listy vMotion.

## <a name="reprotect-azure-vms"></a>Włącz ponownie ochronę maszyny wirtualne platformy Azure

W tej procedurze zakłada lokalnej maszyny Wirtualnej nie jest dostępna, a w przypadku zapewnienia ochrony nieaktywnemu do lokalizacji alternatywnej.

1. W **ustawienia** > **elementy replikowane**, kliknij prawym przyciskiem myszy maszynę Wirtualną, która nie powiodła się za pośrednictwem > **ponownego włączenia ochrony**.
2. W **ponownego włączenia ochrony**, upewnij się, że **Azure do środowiska lokalnego**, jest zaznaczone.
3. Określ lokalny główny serwer docelowy, a serwer przetwarzania.

4. W **Datastore**, wybierz główny cel magazynu danych, do której chcesz odzyskać dyski lokalną. Użyj tej opcji, po usunięciu lokalnej maszyny Wirtualnej i należy utworzyć nowe dyski. To ustawienie jest ignorowane, jeśli dyski już istnieje, ale należy określić wartość.
5. Wybierz dysk przechowywania głównego celu. Zasady powrotu po awarii jest automatycznie wybierany.
6. Kliknij przycisk **OK** zacząć zastosowania. Zadanie rozpoczyna się replikacja maszyny wirtualnej z platformy Azure do lokacji lokalnej. Postęp można śledzić na **zadania** kartę.

> [!NOTE]
> Jeśli chcesz odzyskać maszyny Wirtualnej platformy Azure do istniejącej lokalnej maszyny Wirtualnej, instalacji magazynu danych na lokalnej maszynie wirtualnej z dostępem do odczytu/zapisu, na hoście ESXi serwera głównego celu.


## <a name="run-a-failover-from-azure-to-on-premises"></a>Uruchom tryb failover z platformy Azure do środowiska lokalnego

Aby zreplikować powrót do lokalnej, zasady powrotu po awarii jest używany. Ta zasada jest tworzone automatycznie podczas tworzenia zasady replikacji w przypadku replikacji do platformy Azure:

- Zasady są automatycznie skojarzone z serwerem konfiguracji.
- Nie można modyfikować zasady.
- Wartości zasad są następujące:
    - Cel punktu odzyskiwania próg = 15 minut
    - Przechowywania punktu odzyskiwania = 24 godziny
    - Częstotliwość migawek spójności aplikacji = 60 minut

Uruchom tryb failover w następujący sposób:

1. Na **elementy replikowane** strony, kliknij prawym przyciskiem myszy maszynę > **nieplanowany tryb Failover**.
2. W **potwierdzić trybu Failover**, sprawdź, czy kierunek trybu failover z platformy Azure.

3. Wybierz punkt odzyskiwania, który ma być używany do pracy awaryjnej. Punkt odzyskiwania zapewniających spójność aplikacji występuje przed najnowszy punkt w czasie, a spowoduje utratę danych. Po uruchomieniu trybu failover usługi Site Recovery wyłączania maszyn wirtualnych platformy Azure i rozruch lokalnej maszyny Wirtualnej. Nie będzie można pewien Przestój, więc należy wybrać odpowiedni czas.
4. Kliknij prawym przyciskiem myszy komputer, a następnie kliknij przycisk **zatwierdzić**. Spowoduje to zainicjowanie zadania, które usuwa maszynach wirtualnych platformy Azure.
5. Upewnij się, że zgodnie z oczekiwaniami została zamknięta maszynach wirtualnych platformy Azure.


## <a name="reprotect-on-premises-machines-to-azure"></a>Włącz ponownie ochronę maszyny lokalnej na platformie Azure

Dane powinno być teraz Wstecz w swojej witrynie lokalnego, ale nie przeprowadza replikacji do platformy Azure. Możesz rozpocząć replikację do platformy Azure ponownie w następujący sposób:

1. W magazynie > **ustawienia** > **elementy replikowane**, wybierz przycisk Wstecz nie powiodło się maszyny wirtualne, które nie zostały ponownie, a następnie kliknij przycisk **ponownego włączenia ochrony**.
2. Wybierz serwer przetwarzania, który służy do wysyłania danych do platformy Azure, a następnie kliknij przycisk **OK**.

Po zakończeniu przełączonej maszyny Wirtualnej są replikowane do platformy Azure i uruchomieniu trybu failover zgodnie z wymaganiami.
