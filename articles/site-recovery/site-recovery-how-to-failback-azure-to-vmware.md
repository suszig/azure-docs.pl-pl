---
title: "Jak się nie powieść platformy Azure z powrotem do programu VMware | Dokumentacja firmy Microsoft"
description: "Po przełączeniu maszyn wirtualnych na platformie Azure można zainicjować powrotu po awarii, aby przywrócić maszyn wirtualnych do lokalnego. Dowiedz się więcej czynności w celu wykonaj powrót po awarii."
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
ms.date: 06/05/2017
ms.author: ruturajd
ms.openlocfilehash: 1ca34b262a51b694cb9541750588bbea139eeae1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="fail-back-from-azure-to-an-on-premises-site"></a>Nie powiodło się platformy Azure z lokacją lokalną

W tym artykule opisano, jak niepowodzenie wstecz maszyn wirtualnych z maszyn wirtualnych platformy Azure do lokacji lokalnej. Postępuj zgodnie z instrukcjami w tym artykule, aby zakończyć się niepowodzeniem z powrotem z maszyn wirtualnych VMware lub systemem Windows lub Linux, serwerów fizycznych, po zostały one przejścia w tryb failover z lokalnej lokacji do Azure za pomocą [VMware replikowanie maszyn wirtualnych i serwerów fizycznych do platformy Azure z usługą Azure Site Recovery](site-recovery-vmware-to-azure-classic.md) samouczka.

> [!WARNING]
> Nie można wykonać powrotu po awarii po jednej [ukończone migracji](site-recovery-migrate-to-azure.md#what-do-we-mean-by-migration), przenieść maszynę wirtualną do innej grupy zasobów lub usunąć maszyny wirtualnej platformy Azure. Wyłączenie ochrony maszyny wirtualnej, nie możesz powrotu po awarii.

> [!NOTE]
> Jeśli ma przejścia w tryb failover maszyny wirtualne VMware można powrotu po awarii do hosta funkcji Hyper-v.

## <a name="overview-of-failback"></a>Omówienie powrotu po awarii
Oto, jak działa powrotu po awarii. Po za pośrednictwem powiodły się do platformy Azure, możesz powrót po awarii do lokacji sieci lokalnej w kilku etapach:

1. [Włącz ponownie ochronę](site-recovery-how-to-reprotect.md) maszyn wirtualnych na platformie Azure, aby uruchamiają są replikowane do maszyn wirtualnych VMware w lokacji lokalnej. W ramach tego procesu również należy:
    1. Skonfiguruj lokalny główny serwer docelowy: Windows głównego celu dla maszyny wirtualnej systemu Windows i [główny cel Linux](site-recovery-how-to-install-linux-master-target.md) dla maszyn wirtualnych systemu Linux.
    2. Konfigurowanie [serwera przetwarzania](site-recovery-vmware-setup-azure-ps-resource-manager.md).
    3. Zainicjuj [Wznów](site-recovery-how-to-reprotect.md). Spowoduje to wyłączyć na lokalnej maszynie wirtualnej i zsynchronizuj dane platformy Azure maszyny wirtualnej z dyskami lokalnymi.
5. Po replikowania maszyn wirtualnych na platformie Azure do lokacji sieci lokalnej, zainicjowaniu błędu za pośrednictwem z platformy Azure do lokacji lokalnej.

Po danych nie powiodło się ponownie, Wznów lokalnych maszyn wirtualnych, które nie powiodło się do, aby rozpoczynały się replikację do platformy Azure.

Aby szybko zapoznać Obejrzyj film następujący temat do trybu failover z platformy Azure do lokacji lokalnej.
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/VMware-to-Azure-with-ASR-Video5-Failback-from-Azure-to-On-premises/player]

### <a name="fail-back-to-the-original-or-alternate-location"></a>Powrót po awarii do lokalizacji oryginalnej lub alternatywnej

Jeśli nie powiodło się na maszynie wirtualnej VMware, mogą nie do tej samej maszyny wirtualnej lokalnego źródła Jeśli nadal istnieje. W tym scenariuszu tylko zmiany są replikowane ponownie. Ten scenariusz jest nazywany odzyskiwanie do oryginalnej lokalizacji. Jeśli nie istnieje na lokalnej maszynie wirtualnej, scenariusz jest odzyskiwania do lokalizacji alternatywnej.

> [!NOTE]
> Można tylko powrotu po awarii do oryginalnego serwera vCenter i konfiguracji. Nie można wdrożyć nowy serwer konfiguracji i korzystania z niego powrotu po awarii. Ponadto nie można dodać nowego vCenter z istniejącym serwerem konfiguracji i powrotu po awarii do nowego vCenter.

#### <a name="original-location-recovery"></a>Odzyskiwanie do oryginalnej lokalizacji

Jeśli nie zostanie ponownie oryginalna maszyna wirtualna, wymagane są następujące warunki:
* Jeśli maszyna wirtualna jest zarządzana przez serwer vCenter, hostów ESX główny cel powinien mieć dostęp do magazynu danych maszyny wirtualnej.
* Jeśli maszyna wirtualna znajduje się na hoście ESX, ale nie jest zarządzana przez vCenter, dysku twardego maszyny wirtualnej musi być w magazynu danych, dostępnym dla głównego celu hosta.
* Jeśli Twoja maszyna wirtualna znajduje się na hoście ESX i nie używa vCenter, następnie należy wykonać odnajdywanie hostów ESX głównego elementu docelowego przed możesz Włącz ponownie ochronę. Dotyczy to powrót fizycznych serwerów powrotu po awarii zbyt.
* Powrót do wirtualnej sieci SAN (vSAN) lub dysk, który oparty na urządzenia niesformatowanego mapowania (RDM), jeśli dyski już istnieją i są połączone z na lokalnej maszynie wirtualnej może zakończyć się niepowodzeniem.

#### <a name="alternate-location-recovery"></a>Odzyskiwanie do lokalizacji alternatywnej
Jeśli nie istnieje na lokalnej maszynie wirtualnej przed ponownej ochrony maszyny wirtualnej, scenariusz jest nazywany odzyskiwania do lokalizacji alternatywnej. Przepływ pracy ponownej ochrony ponownie tworzy na lokalnej maszynie wirtualnej. Spowoduje to również pobierania danych.

* Przechodzenia wstecz do alternatywnej lokalizacji maszyny wirtualnej zostaną odzyskane do tego samego hosta ESX, na którym wdrożono główny serwer docelowy. Magazyn danych, który jest używany do tworzenia dysku będzie tego samego magazynu danych, które zostały wybrane podczas ponownej ochrony maszyny wirtualnej.
* Może się nie powieść kopii tylko do systemu plików maszyny wirtualnej (VMFS) lub sieci vSAN magazynu danych. Jeśli masz RDM ponownej ochrony i powrotu po awarii nie będzie działać.
* Ponownej ochrony obejmuje jedną początkowy transfer dużej ilości danych następuje zmiany. Ten proces nie istnieje, ponieważ maszyna wirtualna nie istnieje lokalnie. Pełnych danych musi być replikowane ponownie. Ta ponownej ochrony również zajmie więcej czasu niż odzyskiwanie do oryginalnej lokalizacji.
* Do dysków na podstawie RDM nie może zakończyć się niepowodzeniem. Na VMFS/vSAN magazyn danych można tworzyć tylko nowe dyski maszyny wirtualnej (VMDKs).

Można symulować komputera fizycznego, gdy Failover na platformie Azure, Utwórz kopię tylko jako maszyny wirtualnej VMware (zwaną także P2A2V). Ten przepływ podlega odzyskiwaniu do lokalizacji alternatywnej.

* Serwera fizycznego systemu Windows Server 2008 R2 z dodatkiem SP1, jeśli chroniony Failover na platformie Azure, nie można przełączyć ponownie.
* Upewnij się, można odnaleźć co najmniej jeden główny serwer docelowy i niezbędne hostów ESX/ESXi, do których należy wykonaj powrót po awarii.

## <a name="have-you-completed-reprotection"></a>Czy ukończono ponowne włączenie ochrony?
Przed kontynuowaniem należy wykonać kroki ponownej ochrony, aby maszyny wirtualne są w stanie zreplikowane, a można zainicjować trybu failover do lokacji lokalnej. Aby uzyskać więcej informacji, zobacz [jak włączyć ją ponownie z platformy Azure do środowiska lokalnego](site-recovery-how-to-reprotect.md).

## <a name="prerequisites"></a>Wymagania wstępne

> [!IMPORTANT]
> Podczas pracy w trybie failover na platformie Azure lokalnej lokacji może nie być dostępna i dlatego serwer konfiguracji mogą być dostępne Cofanie lub zamknięcie. Podczas ponownej ochrony i powrotu po awarii lokalnego serwera konfiguracji powinna być uruchomiona i jest niepodłączony OK.

* Serwer konfiguracji jest wymagana lokalnie czy powrotu po awarii. Serwer powinien być w stanie uruchomienia i połączone z tą usługą w taki sposób, że jego kondycja jest dobra. Podczas powrotu po awarii maszyna wirtualna musi istnieć w bazie danych konfiguracji serwera lub powrotu po awarii nie powiodło się. W związku z tym upewnij się, wykonanie zaplanowanego tworzenia kopii zapasowych serwera. Jeśli wystąpił awarii, należy przywrócić działanie serwera przy użyciu tego samego adresu IP do powrotu po awarii do pracy.
* Główny serwer docelowy nie powinna mieć żadnych migawek przed wyzwalania ponownej ochrony/powrotu po awarii.

## <a name="steps-to-fail-back"></a>Kroki, aby wykonaj powrót po awarii

> [!IMPORTANT]
> Przed rozpoczęciem powrotu po awarii, upewnij się, że zostały wykonane ponowne włączenie ochrony maszyn wirtualnych. Maszyny wirtualne powinny znajdować się w stanie chronionym i ich kondycję powinna być **OK**. Aby Włącz ponownie ochronę maszyny wirtualnej, przeczytaj [jak włączyć ją ponownie](site-recovery-how-to-reprotect.md).

1. Na stronie zreplikowanych elementów wybierz maszynę wirtualną, a następnie kliknij prawym przyciskiem myszy, aby zaznaczyć **nieplanowany tryb Failover**.
2. W **potwierdzić trybu Failover**Sprawdź kierunek pracy awaryjnej (na platformie Azure), a następnie wybierz punkt odzyskiwania (Najnowsza lub najnowszą wersję aplikacji spójne), który ma być używany do pracy awaryjnej. Punkt spójne aplikacji znajduje się za ostatniego punktu w czasie i powoduje utratę danych.
3. Podczas pracy w trybie failover Usługa Site Recovery zamyka maszyny wirtualne na platformie Azure. Po sprawdzeniu, że powrót po awarii została ukończona, zgodnie z oczekiwaniami, można sprawdzić, zamknij maszyny wirtualne na platformie Azure.

### <a name="to-what-recovery-point-can-i-fail-back-the-virtual-machines"></a>Do jakich punktu odzyskiwania można I wykonaj powrót po awarii maszyn wirtualnych?

Podczas powrotu po awarii dostępne są dwie opcje, aby powrócić planu maszyny wirtualnej do odzyskiwania danych.

W przypadku wybrania najnowsze przetworzone punktu w czasie wszystkich maszyn wirtualnych będzie można przełączyć do ich najnowszego dostępnego punktu w czasie. W przypadku braku grupy replikacji w ramach planu odzyskiwania, a następnie każdej maszyny wirtualnej w grupie replikacji zostaną przełączone awaryjnie do jego niezależnie od ostatniego punktu w czasie.

Nie można przerwać powrót maszyny wirtualnej do momentu składa się z co najmniej jeden punkt odzyskiwania. Nie można przerwać wstecz planu odzyskiwania, dopóki wszystkie maszyny wirtualne mają co najmniej jednego punktu odzyskiwania.

> [!NOTE]
> Najnowszy punkt odzyskiwania jest punkt odzyskiwania spójna w razie awarii.

W przypadku wybrania punktu odzyskiwania na poziomie aplikacji awarii jednej maszyny wirtualnej zostanie odzyskana do jego najnowszego punktu odzyskiwania zapewniających spójność aplikacji. W przypadku planu odzyskiwania z grupą replikacji każdej grupy replikacji odzyska do wspólnego punktu odzyskiwania.
Należy pamiętać, że punktów odzyskiwania zapewniających spójność aplikacji może być za w czasie i może być utrata danych.

### <a name="what-happens-to-vmware-tools-post-failback"></a>Co się dzieje VMware narzędzia post powrotu po awarii?

W trybie failover do platformy Azure narzędzi VMware nie może być uruchomiona na maszynie wirtualnej Azure. W przypadku maszyny wirtualnej systemu Windows funkcja automatycznego odzyskiwania systemu wyłącza narzędzia VMware w trybie failover. W przypadku maszyny wirtualnej systemu Linux funkcja automatycznego odzyskiwania systemu odinstalowuje narzędzia VMware w trybie failover.

Podczas powrotu po awarii maszyny wirtualnej systemu Windows narzędzia VMware są ponownie włączyć po awarii. Podobnie dla maszyny wirtualnej systemu linux, narzędzi VMware zostaną ponownie zainstalowane na komputerze podczas powrotu po awarii.

## <a name="next-steps"></a>Następne kroki

Po zakończeniu powrotu po awarii, musisz zatwierdzić maszynę wirtualną, aby upewnić się, że odzyskane maszynach wirtualnych platformy Azure są usuwane.

### <a name="commit"></a>Zatwierdzenie
Zatwierdzenia jest wymagana do nie można usunąć za pośrednictwem maszyny wirtualnej z platformy Azure.
Kliknij prawym przyciskiem myszy chronionego elementu, a następnie kliknij przycisk **zatwierdzić**. Zadania spowoduje usunięcie nie powiodło się na maszynach wirtualnych platformy Azure.

### <a name="reprotect-from-on-premises-to-azure"></a>Włącz ponownie ochronę z lokalnej na platformie Azure

Po zakończeniu przekazywania maszyny wirtualnej jest do lokacji lokalnej, ale nie będzie można go chronić. Aby uruchomić ponownie replikację do platformy Azure, wykonaj następujące czynności:

1. W **magazynu** > **ustawienie** > **elementy replikowane**, wybierz maszyny wirtualne, które nie zostały ponownie, a następnie kliknij przycisk **ponownego włączenia ochrony**.
2. Należy podać wartość serwera przetwarzania, który ma służyć do wysyłania danych do usługi Azure.
3. Kliknij przycisk **OK** do rozpoczęcia zadań ponownej ochrony.

> [!NOTE]
> Po rozruchu maszyny wirtualnej lokalnymi, dopiero po pewnym czasie dla agenta zarejestrować serwer konfiguracji (maksymalnie 15 minut). W tym czasie ponownej ochrony kończy się niepowodzeniem i zwraca komunikat o błędzie informujący, że agent nie jest zainstalowany. Poczekaj kilka minut, a następnie spróbuj ponownie ponownej ochrony.

Po zakończeniu działania zadań ponownej ochrony, maszyna wirtualna wykonuje replikację do platformy Azure, a możliwość pracy w trybie failover.

## <a name="common-issues"></a>Typowe problemy
Upewnij się, że vCenter jest w stanie połączenia przed wykonaniem powrotu po awarii. W przeciwnym razie rozłączanie dysków i dołączać do maszyny wirtualnej zakończy się niepowodzeniem.
