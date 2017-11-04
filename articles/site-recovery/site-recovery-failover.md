---
title: "Tryb failover w usłudze Site Recovery | Dokumentacja firmy Microsoft"
description: "Usługa Azure Site Recovery koordynuje replikacji, trybu failover i odzyskiwania maszyn wirtualnych i serwerów fizycznych. Więcej informacji na temat trybu failover do platformy Azure lub dodatkowego centrum danych."
services: site-recovery
documentationcenter: 
author: prateek9us
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 09/25/2017
ms.author: pratshar
ms.openlocfilehash: 9c00cf88fa8b754c92cfd0f01be61a596d04d7c6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="failover-in-site-recovery"></a>Praca w trybie failover w usłudze Site Recovery
W tym artykule opisano, jak do pracy awaryjnej maszyn wirtualnych i fizycznych serwerów są chronione przez usługę Site Recovery.

## <a name="prerequisites"></a>Wymagania wstępne
1. Przed wykonaniem trybu failover, wykonaj [testowanie trybu failover](site-recovery-test-failover-to-azure.md) aby upewnić się, że wszystko działa zgodnie z oczekiwaniami.
1. [Przygotowanie sieci](site-recovery-network-design.md) w lokalizacji docelowej, przed wykonaniem trybu failover.  

Skorzystaj z poniższej tabeli, aby dowiedzieć się o opcjach trybu failover udostępniane przez usługi Azure Site Recovery. Te opcje są także wyświetlane w scenariuszach różnych trybu failover.

| Scenariusz | Wymagania aplikacji odzyskiwania | Przepływ pracy dla funkcji Hyper-V | Przepływ pracy programu VMware
|---|--|--|--|
|Planowany tryb failover z powodu przestoju nadchodzących centrum danych| Zerowej utraty danych dla aplikacji, podczas planowanych działań| Dla funkcji Hyper-V funkcja automatycznego odzyskiwania systemu replikuje dane z częstotliwością kopiowania określone przez użytkownika. Planowany tryb Failover jest używany do zastąpienia częstotliwość i replikować końcowe zmiany przed trybu failover jest inicjowana. <br/> <br/> 1.    Planowanie okna obsługi, zgodnie z procesem zarządzania zmianami Twojej firmy. <br/><br/> 2. powiadamiający użytkowników o nadchodzących przestoju. <br/><br/> 3. Przełącz do trybu offline aplikacji dla użytkownika.<br/><br/>4 zainicjować planowane trybu Failover przy użyciu portalu usługi ASR. Na lokalnej maszynie wirtualnej jest automatycznie zamykana.<br/><br/>Skuteczne stosowanie utraty danych = 0 <br/><br/>Dziennik punktów odzyskiwania jest również udostępniany w okna przechowywania dla użytkownika, który chce używać starszych punktów odzyskiwania. (przechowywania 24 godziny dla funkcji Hyper-V).| Dla VMware funkcja automatycznego odzyskiwania systemu replikuje dane stale przy użyciu punktu dystrybucji list CRL. Pracy awaryjnej zapewnia możliwość pracy awaryjnej do najnowszych danych (w tym zamykania aplikacji post)<br/><br/> 1. Planowanie okna obsługi, zgodnie z procesem zarządzania zmianami <br/><br/>2. powiadamiać użytkowników o nadchodzących przestoju <br/><br/>3.  Przełącz do trybu offline aplikacji dla użytkownika. <br/><br/>4.  Zainicjuj planowanego trybu Failover przy użyciu portalu usługi ASR do ostatniego punktu, gdy aplikacja jest w trybie offline. Użyj opcji "Nieplanowanego trybu Failover" w portalu i wybierz najnowszy punkt do trybu failover. Na lokalnej maszynie wirtualnej jest automatycznie zamykana.<br/><br/>Skuteczne stosowanie utraty danych = 0 <br/><br/>Dziennik punktów odzyskiwania w oknie przechowywania jest dostępna dla klienta, który chce używać starszych punktów odzyskiwania. (72 godzin przechowywania dla VMware).
|Tryb failover z powodu przestoju nieplanowane centrum danych (naturalnego lub IT po awarii) | Minimalna utrata danych dla aplikacji | 1. zainicjowanie planu BCP organizacji <br/><br/>2. Zainicjuj nieplanowanego trybu Failover przy użyciu portalu usługi ASR do najnowszej lub punktu z okna przechowywania (dziennik).| 1. Zainicjuj planu BCP Twojej organizacji. <br/><br/>2.  Zainicjuj nieplanowanego trybu Failover przy użyciu portalu usługi ASR do najnowszej lub punktu z okna przechowywania (dziennik).


## <a name="run-a-failover"></a>Tryb failover
W tej procedurze opisano sposób uruchamiania trybu failover dla [planu odzyskiwania](site-recovery-create-recovery-plans.md). Zamiast tego można uruchomić trybu failover dla jednej maszyny wirtualnej lub serwerze fizycznym z **elementy replikowane** strony


![Tryb failover](./media/site-recovery-failover/Failover.png)

1. Wybierz **plany odzyskiwania** > *recoveryplan_name*. Kliknij przycisk **trybu Failover**
2. Na **pracy awaryjnej** ekranu wybierz **punkt odzyskiwania** do pracy w trybie failover. Można użyć jednej z następujących opcji:
    1.  **Najnowsze** (domyślnie): Ta opcja uruchamia zadanie przy pierwszym przetwarzania danych, które zostało wysłane do usługi Site Recovery. Przetwarzanie danych tworzy punkt odzyskiwania dla każdej maszyny wirtualnej. Ten punkt odzyskiwania jest używany przez maszynę wirtualną w trybie failover. Ta opcja umożliwia najniższy RPO (cel punktu odzyskiwania) jako maszyny wirtualnej utworzone po trybu failover zawiera wszystkie dane, która została zreplikowana do usługi Site Recovery wyzwolenia pracy awaryjnej.
    1.  **Najnowsze przetworzone**: Ta opcja awaryjnie wszystkie maszyny wirtualne w planie odzyskiwania do ostatniego punktu odzyskiwania, które zostały już przetworzone przez usługę Site Recovery. Podczas wykonywania testu pracy w trybie failover maszyny wirtualnej, wyświetlane są również sygnaturę czasową ostatniego punktu odzyskiwania przetworzone. Jeśli przeprowadzasz trybu failover planu odzyskiwania, można przejść do poszczególnych maszyn wirtualnych i przyjrzyj się **najnowsze punkty odzyskiwania** kafelkiem, aby uzyskać te informacje. Ponieważ nie jest czas przetwarzania nieprzetworzonych danych, ta opcja umożliwia niski opcja pracy awaryjnej RTO (celu czasu odzyskiwania).
    1.  **Najnowsza wersja aplikacji spójne**: Ta opcja awaryjnie wszystkie maszyny wirtualne w planie odzyskiwania do ostatniego punktu odzyskiwania zapewniających spójność aplikacji, które zostały już przetworzone przez usługę Site Recovery. Podczas wykonywania testu pracy w trybie failover maszyny wirtualnej, wyświetlane są również sygnaturę czasową punktu najnowszych odzyskiwania zapewniających spójność aplikacji. Jeśli przeprowadzasz trybu failover planu odzyskiwania, można przejść do poszczególnych maszyn wirtualnych i przyjrzyj się **najnowsze punkty odzyskiwania** kafelkiem, aby uzyskać te informacje.
    1.  **Najnowsze wielu maszyn wirtualnych przetwarzane**: Ta opcja jest dostępna tylko dla planów odzyskiwania, które mają co najmniej jedną maszynę wirtualną z wielu maszyn wirtualnych na. Maszyny wirtualne, które są częścią grupy replikacji trybu failover do najnowszego wspólnej odzyskiwania zapewniających spójność wielu maszyn wirtualnych punktu. Inne maszyny wirtualne w tryb failover ich najnowszym przetworzonych punktem odzyskiwania.  
    1.  **Najnowsze wielu maszyn wirtualnych całej aplikacji**: Ta opcja jest dostępna tylko dla planów odzyskiwania, które mają co najmniej jedną maszynę wirtualną z ON spójności wielu maszyn wirtualnych. Maszyny wirtualne, które są częścią grupy replikacji trybu failover do najnowszej wspólnej odzyskiwania zapewniających spójność aplikacji obejmujących wiele maszyn punktu. Inne maszyny wirtualne w tryb failover ich najnowszy punkt odzyskiwania zapewniających spójność aplikacji.
    1.  **Niestandardowe**: Jeśli przeprowadzasz testowy tryb failover maszyny wirtualnej, a następnie można użyć tej opcji do trybu failover do danego punktu.

    > [!NOTE]
    > Opcja Wybierz punkt odzyskiwania jest dostępna tylko w przypadku, gdy za pośrednictwem kończą się niepowodzeniem na platformie Azure.
    >
    >


1. Jeśli niektóre maszyny wirtualne w planie odzyskiwania zostały awaryjnie podczas poprzedniego uruchomienia, a teraz maszyny wirtualne są aktywne w miejscu źródłowym i docelowym, można użyć **zmianę kierunku** opcję, aby określić kierunek, w którym powinno się zdarzyć pracy awaryjnej.
1. Jeśli użytkownik jest przechodzenie w tryb failover na platformie Azure i szyfrowanie danych jest włączone dla chmury (dotyczy tylko w przypadku ochrony maszyn wirtualnych funkcji Hyper-v z serwera programu VMM), w **klucza szyfrowania** wybierz certyfikat, który został wystawiony, włączenie szyfrowania danych podczas instalacji na serwerze programu VMM.
1. Wybierz **Zamknij maszynę przed rozpoczęciem pracy awaryjnej** Jeśli chcesz, aby usługi Site Recovery, aby spróbować wykonać zamykania maszyn wirtualnych źródła przed wyzwolenie pracy awaryjnej. Tryb failover trwa nawet w przypadku zamknięcia nie powiedzie się.  

    > [!NOTE]
    > Jeśli maszyny wirtualne funkcji Hyper-v są chronione, opcja zamknięcie również próbuje synchronizować dane lokalne, które nie zostały wysłane do usługi przed wyzwolenie pracy awaryjnej.
    >
    >

1. Możesz śledzić postęp trybu failover **zadania** strony. Nawet jeśli występują błędy podczas nieplanowanego trybu failover, planu odzyskiwania uruchamia do czasu ukończenia.
1. Po przejściu w tryb failover Sprawdź poprawność maszyny wirtualnej po zalogowaniu się do niego. Jeśli chcesz przejść do innego punktu odzyskiwania dla maszyny wirtualnej, a następnie można użyć **zmienić punktu odzyskiwania** opcji.
1. Po uzyskaniu nieudane przez maszynę wirtualną, możesz **zatwierdzić** pracy awaryjnej. Zatwierdzanie powoduje usunięcie wszystkich punktów odzyskiwania dostępnych w usłudze i **zmienić punktu odzyskiwania** opcja nie będzie już dostępna.

## <a name="planned-failover"></a>Planowany tryb failover
Maszyny wirtualne/serwery fizyczne chronione za pomocą usługi Site Recovery również obsługa **planowanego trybu failover**. Planowany tryb failover to zero danych utraty pracy awaryjnej opcja. Po wyzwoleniu planowanego trybu failover, najpierw zamknij źródłowych maszynach wirtualnych, synchronizację danych jeszcze do synchronizacji i następnie wyzwoleniu trybu failover.

> [!NOTE]
> Podczas możesz trybu failover funkcji Hyper-v maszyn wirtualnych z jednej lokalnej lokacji do innej lokacji lokalnej, aby wrócić do lokacji głównej lokalnymi należy najpierw **replikacji odwrotnej** maszyny wirtualnej z powrotem do lokacji głównej i wyzwolić tryb failover. Jeśli podstawowa maszyna wirtualna nie jest dostępne, przed uruchomieniem do **replikacji odwrotnej** niezbędnych do przywrócenia maszyny wirtualnej z kopii zapasowej.   
>
>

## <a name="failover-job"></a>Zadanie trybu failover

![Tryb failover](./media/site-recovery-failover/FailoverJob.png)

Po wyzwoleniu trybu failover obejmuje następujące kroki:

1. Sprawdzanie wymagań wstępnych: ten krok zapewnia, że spełniono wszystkie warunki wymagane do trybu failover
1. Tryb failover: Ten krok przetwarza dane i powoduje gotowy, że maszyna wirtualna platformy Azure można utworzyć poza jego. Jeśli wybrano **najnowsze** punkt odzyskiwania, w tym kroku tworzy punkt odzyskiwania z danych, która została wysłana do usługi.
1. Start: Ten krok umożliwia utworzenie maszyny wirtualnej platformy Azure przy użyciu danych przetworzonych w poprzednim kroku.

> [!WARNING]
> **Nie Anuluj toku pracy awaryjnej**: przed rozpoczęciem pracy awaryjnej zatrzymania replikacji dla maszyny wirtualnej. Jeśli użytkownik **anulować** w zadaniu postępu zatrzymuje pracy awaryjnej, ale maszyna wirtualna nie zostanie uruchomiona do replikacji. Nie można ponownie rozpocząć replikacji.
>
>

## <a name="time-taken-for-failover-to-azure"></a>Czas pracy w trybie Failover na platformie Azure

W niektórych przypadkach pracy awaryjnej maszyn wirtualnych wymaga bardzo pośredniego kroku, który zazwyczaj trwa około 8 do 10 minut, aby zakończyć. W następujących przypadkach będzie wyższa niż zwykle czas potrzebny do trybu failover:

* Maszyny wirtualne VMware przy użyciu usługi mobilności w wersji starszej niż 9,8
* Serwerów fizycznych 
* Maszyny wirtualne VMware systemu Linux
* Maszyny wirtualne funkcji Hyper-V chronione jako serwerów fizycznych
* Maszyny wirtualne VMware, gdy następujące sterowniki nie są dostępne jako rozruchu sterowników 
    * storvsc 
    * magistralę maszyny wirtualnej 
    * storflt 
    * Intelide 
    * ATAPI
* Adresy IP maszyn wirtualnych VMware, które nie mają włączone niezależnie od tego, czy używasz DHCP lub statyczna usługi DHCP

We wszystkich innych przypadkach ten krok pośrednie nie jest wymagane, a czas pracy w trybie failover jest znacznie niższa. 





## <a name="using-scripts-in-failover"></a>Za pomocą skryptów w tryb Failover
Można zautomatyzować niektóre działania podczas wykonywania pracy awaryjnej. Za pomocą skryptów lub [elementy runbook automatyzacji Azure](site-recovery-runbook-automation.md) w [planów odzyskiwania](site-recovery-create-recovery-plans.md) w tym celu.

## <a name="other-considerations"></a>Inne zagadnienia
* **Litera dysku** — Aby zachować litery dysku w przypadku maszyn wirtualnych po pracy awaryjnej można ustawić **zasad sieci SAN** dla maszyny wirtualnej do **OnlineAll**. [Dowiedz się więcej](https://support.microsoft.com/en-us/help/3031135/how-to-preserve-the-drive-letter-for-protected-virtual-machines-that-are-failed-over-or-migrated-to-azure).



## <a name="next-steps"></a>Następne kroki

> [!WARNING]
> Gdy ma przejścia w tryb failover maszyny wirtualne i lokalnego centrum danych jest dostępna, wykonaj następujące czynności [ **Wznów** ](site-recovery-how-to-reprotect.md) maszyn wirtualnych VMware z powrotem do lokalnego centrum danych.

Użyj [ **planowanego trybu failover** ](site-recovery-failback-from-azure-to-hyper-v.md) opcji w celu **powrotu po awarii** maszyn wirtualnych funkcji Hyper-v do lokalnej z platformy Azure.

Jeśli nie powiodło się za pośrednictwem funkcji Hyper-v maszyny wirtualnej do innego lokalnego centrum danych zarządzanych przez serwer VMM i centrum danych podstawowych jest dostępna, następnie użyj **replikacja odwrotna** opcję, aby rozpocząć replikację na centrum danych podstawowych.
