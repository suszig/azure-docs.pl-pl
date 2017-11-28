---
title: "Powrót po awarii w usłudze Azure Site Recovery dla maszyny wirtualnej funkcji Hyper-v | Dokumentacja firmy Microsoft"
description: "Usługa Azure Site Recovery koordynuje replikacji, trybu failover i odzyskiwania maszyn wirtualnych i serwerów fizycznych. Więcej informacji na temat powrotu po awarii z platformy Azure do lokalnego centrum danych."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/28/2017
ms.author: ruturajd
ms.openlocfilehash: 00fc7588834de016a6d6118474a033b8cd3a0117
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="failback-in-site-recovery-for-hyper-v-virtual-machines"></a>Powrót po awarii w usłudze Site Recovery dla maszyny wirtualnej funkcji Hyper-V

W tym artykule opisano sposób powrotu po awarii maszyny wirtualne chronione przez usługę Site Recovery.

## <a name="prerequisites"></a>Wymagania wstępne
1. Upewnij się, że serwer lokacji głównej VMM serwera/funkcji Hyper-V jest połączony.
2. Użytkownik powinien wykonał **zatwierdzić** na maszynie wirtualnej.

## <a name="why-is-there-no-button-called-failback"></a>Dlaczego jest ma przycisku o nazwie powrotu po awarii
W portalu Brak nie jawnego gestu o nazwie powrotu po awarii. Powrót po awarii jest to krok, gdzie możesz wrócić do lokacji głównej. Zgodnie z definicją pracy awaryjnej jest podczas pracy awaryjnej należy maszyn wirtualnych z primary(on-premises) lokacji odzyskiwania (Azure), a powrotu po awarii jest podczas pracy awaryjnej należy maszyn wirtualnych z odzyskiwania z powrotem do podstawowej.

Po zainicjowaniu tryb failover bloku informuje o kierunku zadania. Jeśli atrybut kierunku ma z platformy Azure do środowiska lokalnego, jest powrót po awarii.

## <a name="why-is-there-only-a-planned-failover-gesture-to-failback"></a>Dlaczego jest tylko gestu planowany tryb failover do powrotu po awarii
Azure to środowisko o wysokiej dostępności i maszyn wirtualnych będą zawsze dostępne. Powrót po awarii jest planowane działanie decyzję dotyczącą podjąć przestoju małych obciążeń można uruchomić ponownie uruchamiane lokalnie. To oczekuje bez utraty danych. Dlatego jest dostępna tylko gestu planowanego trybu failover, które wyłączyć maszyny wirtualne na platformie Azure, pobrać najnowsze zmiany i upewnij się, że istnieje nie powoduje utraty danych.

## <a name="do-i-need-a-process-server-in-azure-to-failback-to-hyper-v"></a>Należy serwer przetwarzania na platformie Azure do powrotu po awarii do funkcji Hyper-v?
Nie, serwer przetwarzania jest wymagany tylko wtedy, gdy chronisz maszyny wirtualne VMware. Żadne dodatkowe składniki są wymagane do wdrożenia, gdy ochrona/powrotu po awarii maszyn wirtualnych funkcji Hyper-v.

## <a name="initiate-failback"></a>Zainicjuj powrotu po awarii
Po trybu failover z serwera podstawowego do lokalizacji dodatkowej replikowane maszyny wirtualne nie są chronione przez usługę Site Recovery i dodatkowej lokalizacji obecnie działa jako aktywnej lokalizacji. Postępuj zgodnie z poniższymi procedurami, aby powrót po awarii do oryginalnego lokacji głównej. W tej procedurze opisano sposób uruchamiania planowany tryb failover planu odzyskiwania. Alternatywnie można uruchomić trybu failover dla jednej maszyny wirtualnej na **maszyn wirtualnych** kartę.

1. Wybierz **plany odzyskiwania** > *recoveryplan_name*. Kliknij przycisk **pracy awaryjnej** > **planowanego trybu Failover**.
2. Na ** potwierdzić planowanego trybu Failover ** wybierz lokalizacja źródłowa i docelowa. Należy pamiętać, kierunku trybu failover. Jeśli pracy awaryjnej z podstawowej działał jako oczekiwać i wszystkich maszyn wirtualnych znajdują się w lokalizacji dodatkowej, jest to wyłącznie w celach informacyjnych.
3. W przypadku powrotu po awarii z platformy Azure, wybierz ustawienia w **synchronizacji danych**:

   * **Synchronizowanie danych przed pracy awaryjnej (tylko zmiany różnicowe Synchronizuj)**— tej opcji minimalizuje przestojów maszyn wirtualnych, jak synchronizacji bez zamykania je. Wykonuje następujące zadania:
     * Faza 1: Wykonuje migawkę maszyny wirtualnej na platformie Azure i kopiuje go do lokalnego hosta funkcji Hyper-V. Komputer nadal działają na platformie Azure.
     * Faza 2: Zamyka maszyny wirtualnej platformy Azure, dzięki czemu ma nowych zmian występuje. Ostateczny zestaw zmian, które zmiany są przekazywane do serwera lokalnego i na lokalnej maszynie wirtualnej jest uruchomiona.

    - **Synchronizacja danych w trybie failover tylko (pełnego pobierania)**— Użyj tej opcji, jeśli działała na platformie Azure przez długi czas. Ta opcja jest szybsze, ponieważ oczekuje się, że większość dysku został zmieniony i nie chcemy poświęcić czas obliczenia sumy kontrolnej. Wykonuje operację pobierania na dysku. Jest również przydatne w przypadku lokalnego maszyny wirtualnej został usunięty.

    >[!NOTE]
    >Firma Microsoft zaleca, możesz użyć tej opcji, jeśli działała Azure jakiś czas (miesiąc lub więcej) lub lokalnej maszyny wirtualnej został usunięty. Ta opcja nie wykonać wszelkie obliczenia sumy kontrolnej.


4. Jeśli szyfrowanie danych jest włączone dla chmury, w **klucza szyfrowania** wybierz certyfikat, który został wystawiony, włączenie szyfrowania danych podczas instalacji dostawcy na serwerze programu VMM.
5. Inicjuj tryb failover. Możesz śledzić postęp trybu failover **zadania** kartę.
6. Jeśli wybrano opcję, aby zsynchronizować dane przed przejście w tryb failover, po ukończeniu synchronizacji początkowej danych i wszystko jest gotowe wyłączyć maszyny wirtualne na platformie Azure, kliknij przycisk **zadania** Nazwa zadania planowany tryb failover **Przejdź w tryb Failover**. Zamyka maszyny platformy Azure, przesyła najnowsze zmiany na lokalnej maszynie wirtualnej i uruchomieniu maszyny Wirtualnej lokalną.
7. Możesz teraz dziennika na maszynie wirtualnej, aby go zweryfikować jest dostępna, zgodnie z oczekiwaniami.
8. Maszyna wirtualna jest w stan oczekiwania na zatwierdzenie. Kliknij przycisk **zatwierdzania** do zatwierdzenia pracy awaryjnej.
9. Teraz, aby dokończyć kliknij powrotu po awarii **odwrócić replikację** do uruchomienia ochrony maszyny wirtualnej w lokacji głównej.

## <a name="failback-to-an-alternate-location"></a>Powrót po awarii do lokalizacji alternatywnej
Jeśli została wdrożona ochrona między [lokacji funkcji Hyper-V i Azure](site-recovery-hyper-v-site-to-azure.md) masz możliwość powrotu po awarii z platformy Azure do lokalizacji alternatywnej lokalnymi. Jest to przydatne, jeśli chcesz skonfigurować nowy sprzęt lokalnymi. Oto jak to zrobić.

1. Jeśli podczas konfigurowania nowego sprzętu systemu Windows Server 2012 R2 i roli Hyper-V należy zainstalować na serwerze.
2. Utwórz przełącznik sieci wirtualnej o tej samej nazwie, która była na oryginalnym serwerze.
3. Wybierz **chronione elementy** -> **grupy ochrony**  ->  <ProtectionGroupName>  ->  <VirtualMachineName> chcesz wykonaj powrót po awarii, a następnie wybierz **planowane Tryb failover**.
4. W **potwierdzić planowanego trybu Failover** wybierz **Utwórz lokalną maszynę wirtualną, jeśli nie istnieje**.
5. W **nazwy hosta** wybierz nowy serwer hosta funkcji Hyper-V, na którym chcesz umieścić maszynę wirtualną.
6. W synchronizacji danych zaleca się wybierz opcję **synchronizować dane przed przejście w tryb failover**. Minimalizuje czas przestoju w przypadku maszyn wirtualnych, jak synchronizacji bez zamykania je. Wykonuje następujące zadania:

   * Faza 1: Wykonuje migawkę maszyny wirtualnej na platformie Azure i kopiuje go do lokalnego hosta funkcji Hyper-V. Komputer nadal działają na platformie Azure.
   * Faza 2: Zamyka maszyny wirtualnej platformy Azure, dzięki czemu ma nowych zmian występuje. Ostateczny zestaw zmian są przekazywane do serwera lokalnego i zostanie uruchomiona na lokalnej maszynie wirtualnej.
7. Kliknij znacznik wyboru, aby rozpocząć tryb failover (powrót po awarii).
8. Po zakończeniu synchronizacji początkowej i wszystko jest gotowe wyłączyć maszynę wirtualną na platformie Azure, kliknij przycisk **zadania** > <planned failover job> > **ukończenia pracy awaryjnej**. Zamyka maszyny platformy Azure, przesyła najnowsze zmiany na lokalnej maszynie wirtualnej i uruchamia go.
9. Możesz zalogować się na maszynie wirtualnej lokalnego, aby sprawdzić, czy wszystko działa zgodnie z oczekiwaniami. Następnie kliknij przycisk **zatwierdzić** na zakończenie pracy awaryjnej.
10. Kliknij przycisk **odwrócić replikację** aby rozpocząć ochronę na lokalnej maszynie wirtualnej.

    > [!NOTE]
    > Jeśli anulujesz zadaniem powrotu po awarii, gdy znajduje się w kroku synchronizacji danych, lokalnej maszyny Wirtualnej będzie w stanie uszkodzenia. To dlatego synchronizacji danych kopiuje najnowsze dane z dysków maszyny Wirtualnej platformy Azure na lokalnych dyskach danych, a dopiero po zakończeniu synchronizacji, dane dysku mogą nie być w stanie spójności. Jeśli lokalnych na maszynie Wirtualnej rozruchu po synchronizacji danych zostało anulowane, może nie uruchomić. Ponownie uruchomienie trybu failover w celu wykonania synchronizacji danych.

## <a name="time-taken-to-failback"></a>Czas potrzebny na powrót po awarii
Czas potrzebny na zakończenie synchronizacji danych i rozruchu maszyny wirtualnej zależy od różnych czynników. Aby dać wgląd w terminie, firma Microsoft opisano, co się dzieje podczas synchronizacji danych.

Synchronizacja danych tworzy migawkę dysków maszyny wirtualnej i rozpoczyna się sprawdzanie blok po bloku i oblicza jego sumy kontrolnej. To obliczeniowe sumy kontrolnej są wysyłane do środowiska lokalnego do porównania z lokalnymi sumy kontrolnej z tym samym bloku. W przypadku sum kontrolnych są zgodne, nie jest przenoszona bloku danych. Jeśli nie jest zgodny, bloku danych jest przenoszona do środowiska lokalnego. Ten czas transferu jest zależna od przepustowość. Szybkość sumy kontrolnej to kilka GB na min. 

W celu przyspieszenia pobierania danych, można skonfigurować agenta MARS do użycia większej liczby wątków na parallalize pobierania. Zapoznaj się [dokumentu tutaj](https://support.microsoft.com/en-us/help/3056159/how-to-manage-on-premises-to-azure-protection-network-bandwidth-usage) dotyczące zmieniania wątki pobierania agenta.


## <a name="next-steps"></a>Następne kroki

Po zakończeniu zadaniem powrotu po awarii **zatwierdzić** maszyny wirtualnej. Zatwierdzania usuwa maszyny wirtualnej platformy Azure i jego dysków i przygotowuje maszynę Wirtualną, która ma być chroniony ponownie.

Po **zatwierdzić**, można zainicjować *odwrócić replikację*. Spowoduje to uruchomienie ochrony maszyny wirtualnej z lokalnego do platformy Azure. Należy pamiętać, że to tylko zreplikuje zmiany, ponieważ maszyna wirtualna została wyłączona na platformie Azure i dlatego wysyła różnicowej zmienia tylko.
