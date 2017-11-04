---
title: "Konfigurowanie odzyskiwania po awarii do platformy Azure na serwerach fizycznych na lokalnych z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować odzyskiwania po awarii do platformy Azure dla systemu Windows i Linux serwerów lokalnych, z usługą Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 805f6946-c6da-491f-980e-bf724bebdf0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: raynew
ms.openlocfilehash: ceb4b13e326b24360799c1a7a25fe48f213fabd7
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Konfigurowanie odzyskiwania po awarii do platformy Azure dla lokalnych serwerów fizycznych

[Usługi Azure Site Recovery](site-recovery-overview.md) usługi przyczynia się do strategii odzyskiwania po awarii poprzez organizowanie replikacji, trybu failover i powrotu po awarii maszyn lokalnych i Azure maszynach wirtualnych (VM) i zarządzanie nimi.

W tym samouczku przedstawiono sposób konfigurowania odzyskiwania po awarii lokalnego systemu Windows i Linux serwerów fizycznych do platformy Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Konfigurowanie wymagań wstępnych platformy Azure i lokalnymi
> * Tworzenie magazynu usług odzyskiwania Site Recovery 
> * Ustaw źródła i docelowymi środowisk replikacji
> * Tworzenie zasad replikacji
> * Włącz replikację dla serwera

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

- Upewnij się, że rozumiesz [architektura scenariusza i składniki](concepts-physical-to-azure-architecture.md).
- Przegląd [spełnić wymagania dotyczące](site-recovery-support-matrix-to-azure.md) dla wszystkich składników.
- Upewnij się, że spełniają serwerów, które chcesz replikować [wymagania maszyny Wirtualnej Azure](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Przygotuj Azure. Potrzebujesz subskrypcji platformy Azure, sieć wirtualną platformy Azure i konto magazynu.
- Przygotowanie konta do automatycznej instalacji usługi mobilności na każdym serwerze, który chcesz replikować.



### <a name="set-up-an-azure-account"></a>Konfigurowanie konta platformy Azure

Firma Microsoft [konta Azure](http://azure.microsoft.com/).

- Możesz rozpocząć od [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).
- Dowiedz się więcej o [cenach usługi Site Recovery](site-recovery-faq.md#pricing)i uzyskać [szczegóły cennika](https://azure.microsoft.com/pricing/details/site-recovery/).
- Sprawdzić, które [są obsługiwane regiony](https://azure.microsoft.com/pricing/details/site-recovery/) Site Recovery.

### <a name="verify-azure-account-permissions"></a>Sprawdź uprawnienia konta platformy Azure

Upewnij się, że konto Azure ma uprawnienia do replikacji maszyn wirtualnych na platformie Azure.

- Przegląd [uprawnienia](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) trzeba replikować maszyny do platformy Azure.
- Sprawdź i zmodyfikuj [dostępu opartej na rolach](../active-directory/role-based-access-control-configure.md) uprawnienia. 



### <a name="set-up-an-azure-network"></a>Konfiguracja sieci platformy Azure

Konfigurowanie [sieć platformy Azure](../virtual-network/virtual-network-get-started-vnet-subnet.md).

- Maszyny wirtualne platformy Azure są umieszczane w tej sieci, gdy są tworzone po pracy awaryjnej.
- Sieć powinna znajdować się w tym samym regionie co magazyn usług odzyskiwania


## <a name="set-up-an-azure-storage-account"></a>Konfigurowanie konta usługi Azure Storage

Konfigurowanie [konto magazynu Azure](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Usługa Site Recovery replikuje maszyny lokalnej do magazynu Azure. Maszyny wirtualne platformy Azure są tworzone z magazynu po pracy awaryjnej.
- Konto magazynu musi być w tym samym regionie co magazyn usług odzyskiwania.
- Konto magazynu może być standard lub [premium](../virtual-machines/windows/premium-storage.md).
- Skonfigurowanie konta premium, należy również dodatkowe konta standardowego dla danych dziennika.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Przygotowanie do instalacji usługi mobilności konta

Musi być zainstalowana usługa mobilności na każdym serwerze, który chcesz replikować. Usługa Site Recovery automatycznie instaluje tej usługi, po włączeniu replikacji dla serwera. Aby automatycznie zainstalować, należy przygotować konta używanego do uzyskiwania dostępu do serwera usługi Site Recovery.

- Korzystając z domeny lub lokalnego konta
- Dla maszyn wirtualnych systemu Windows, jeśli nie używasz konta domeny, wyłącz kontroli dostępu użytkownika zdalnego na komputerze lokalnym. Aby to zrobić, w rejestrze, w obszarze **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, Dodaj wpis DWORD **LocalAccountTokenFilterPolicy**, o wartości 1.
- Aby dodać wpis rejestru, aby wyłączyć to ustawienie z interfejsu wiersza polecenia, wpisz:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- Dla systemu Linux konto powinno być głównym urzędem certyfikacji na serwer źródłowy z systemem Linux.


## <a name="create-a-vault"></a>Tworzenie magazynu

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Wybierz cel ochrony

Wybierz elementy do replikacji i jej do replikacji.

1. Kliknij przycisk **Magazyny usług odzyskiwania** > magazynu.
2. W Menu zasobów kliknij **usługi Site Recovery** > **przygotowanie infrastruktury** > **cel ochrony**.
3. W **cel ochrony**, wybierz pozycję **do platformy Azure** > **nie zwirtualizowanych/inne**.

## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

Konfigurowanie serwera konfiguracji, zarejestruj go w magazynie i odnajdywanie maszyn wirtualnych.

1. Kliknij przycisk **lokacji odzyskiwania** > **przygotowanie infrastruktury** > **źródła**.
2. Jeśli nie masz serwera konfiguracji, kliknij przycisk **+ serwer konfiguracji**.
3. W **Dodaj serwer**, sprawdź, czy **serwera konfiguracji** pojawia się w **typ serwera**.
4. Pobierz plik instalacyjny instalacja Unified usługi Site Recovery.
5. Pobierz klucz rejestracji magazynu. Należy to po uruchomieniu Instalatora Unified. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

   ![Konfiguracja źródła](./media/tutorial-physical-to-azure/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Zarejestruj serwer konfiguracji w magazynie

Przed rozpoczęciem, wykonaj następujące czynności: 

- Na komputerze z serwerem konfiguracji, upewnij się, że zegar jest zsynchronizowany z [serwer czasu](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Powinna być zgodna. Jeśli jest 15 minut przed lub za, Instalator może zakończyć się niepowodzeniem.
- Upewnij się, że komputer ma dostęp do tych adresów URL:[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

- Reguły zapory oparte na adresie IP powinna zezwalać na komunikację z platformą Azure.
- Zezwól na użycie [zakresów adresów IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653) oraz portu 443 protokołu HTTPS.
- Zezwól na użycie zakresów adresów IP dla regionu platformy Azure Twojej subskrypcji oraz regionu Zachodnie stany USA (służy do kontrolowania dostępu i zarządzania tożsamościami).

Uruchom Instalatora Unified jako administratora lokalnego, aby zainstalować serwer konfiguracji. Serwer przetwarzania i główny serwer docelowy są również instalowane domyślnie na serwerze konfiguracji.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

Po zakończeniu rejestracji serwer konfiguracji jest wyświetlany na **ustawienia** > **serwerów** strony w magazynie.


## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

Wybierz i zweryfikować zasobów docelowych.

1. Kliknij kolejno pozycje **Przygotowywanie infrastruktury** > **Docelowa** i wybierz subskrypcję platformy Azure do użycia.
2. Określ model wdrożenia docelowego.
3. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

   ![docelowy](./media/tutorial-physical-to-azure/network-storage.png)


## <a name="create-a-replication-policy"></a>Tworzenie zasad replikacji

1. Aby utworzyć nowe zasady replikacji, kliknij przycisk **infrastruktura usługi Site Recovery** > **zasady replikacji** > **+ zasad replikacji**.
2. W **Utwórz zasady replikacji**, określ nazwę zasady.
3. W **próg RPO**, określ limit punktu odzyskiwania celu (RPO). Ta wartość określa, jak często są tworzone punkty odzyskiwania danych. Alert jest generowany, gdy ciągłej replikacji przekracza ten limit.
4. W **przechowywania punktu odzyskiwania**, określić czas (w godzinach) okna przechowywania dla każdego punktu odzyskiwania. Replikowane maszyny wirtualne można odzyskać do dowolnego punktu w oknie. Do 24 godzin przechowywania jest obsługiwane na komputerach, replikowane do magazynu w warstwie premium i 72 godziny dla magazynu w warstwie standardowa.
5. W **częstotliwość migawek spójności aplikacji**, określ częstotliwość (w minutach) będą tworzone punkty odzyskiwana zawierające migawki spójne z aplikacjami. Kliknij przycisk **OK** do tworzenia zasad.

    ![Zasady replikacji](./media/tutorial-physical-to-azure/replication-policy.png)


Zasady są automatycznie skojarzone z serwerem konfiguracji. Domyślnie zasady uzgadniania jest tworzony automatycznie powrotu po awarii. Na przykład, jeśli zasady replikacji jest **zasad rep** następnie zasady powrotu po awarii **rep zasad-powrotu po awarii** jest tworzony. Ta zasada nie jest używany, dopiero po zainicjowaniu powrotu po awarii z platformy Azure.

## <a name="enable-replication"></a>Włączanie replikacji

Włącz replikację dla każdego serwera.

- Usługa Site Recovery instaluje usługi mobilności, gdy replikacja jest włączona.
- Po włączeniu replikacji dla serwera może zająć 15 minut lub dłużej, aby zmiany zostały wprowadzone i wyświetlane w portalu.

1. Kliknij przycisk **Replikowanie aplikacji** > **źródła**.
2. W **źródła**, wybierz serwer konfiguracji.
3. W **komputera typu**, wybierz pozycję **maszyn fizycznych**.
4. Wybierz serwer przetwarzania (serwer konfiguracji). Następnie kliknij przycisk **OK**.
5. W **docelowego**, wybierz subskrypcji i grupy zasobów, w którym chcesz utworzyć maszynach wirtualnych Azure po pracy awaryjnej. Wybierz model wdrażania, który ma być używany na platformie Azure (Zarządzanie zasobu lub classic).
6. Wybierz konto magazynu Azure, którego chcesz użyć do replikacji danych. 
7. Wybierz sieć platformy Azure i podsieć, z którą nawiążą połączenie maszyny wirtualne platformy Azure, gdy zostaną uruchomione po przejściu do trybu failover.
8. Wybierz opcję **Konfiguruj teraz dla wybranych maszyn**, aby zastosować ustawienia sieci do wszystkich maszyn wybranych do ochrony. Wybierz opcję **Konfiguruj później**, aby wybrać sieć platformy Azure dla poszczególnych maszyn. 
9. W **maszyn fizycznych**i kliknij przycisk **+ maszyny fizycznej**. Określ nazwę i adres IP. Wybierz system operacyjny maszyny, które mają być replikowane. Trwa kilka minut, aż serwery odnalezione i wyświetlane. 
10. W **właściwości** > **skonfigurować właściwości**, wybierz konto, które będzie używane przez serwer przetwarzania Aby automatycznie zainstalować usługi mobilności na maszynie.
11. W **ustawienia replikacji** > **Konfigurowanie ustawień replikacji**, sprawdź, czy jest wybrane zasady replikacji poprawne. 
12. Kliknij przycisk **włączyć replikację**. Możesz śledzić postęp **Włącz ochronę** zadania w **ustawienia** > **zadania** > **zadania usługi Site Recovery**. Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.


Do monitorowania serwerów, możesz dodać, można sprawdzić czas ostatniego odnalezionych ich w **serwery konfiguracji** > **ostatniego kontaktu w**. Aby dodać komputery bez oczekiwania na godziny zaplanowanego odnajdywania, zaznacz serwer konfiguracji (nie klikaj pozycji go) i kliknij przycisk **Odśwież**.

## <a name="next-steps"></a>Następne kroki

[Uruchamianie próbnego odzyskiwania po awarii](tutorial-dr-drill-azure.md)
