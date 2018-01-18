---
title: "Konfigurowanie odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnych z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnych z usługą Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 01/15/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 8acc8deff8b635c97e8722d65a728aebf0e49bb3
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Konfigurowanie odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnej

W tym samouczku przedstawiono sposób ustawienia odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnego systemu Windows. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Określ replikacji źródłowych i docelowych.
> * Konfigurowanie środowiska źródłowego replikacji, łącznie z lokalnymi składnikami usługi Site Recovery i środowiska docelowego replikacji.
> * Tworzenie zasad replikacji
> * Włącz replikację dla maszyny Wirtualnej

To jest trzeci samouczek w serii. Ten samouczek zakłada, zostały już wykonane zadania w poprzednim samouczki:

1. [Przygotowywanie platformy Azure](tutorial-prepare-azure.md)
2. [Przygotowywanie lokalnego wdrożenia oprogramowania VMware](tutorial-prepare-on-premises-vmware.md)

Przed rozpoczęciem warto [Przejrzyj architektura](concepts-vmware-to-azure-architecture.md) scenariuszu odzyskiwania po awarii.


## <a name="select-a-replication-goal"></a>Wybierz cel replikacji

1. W **Magazyny usług odzyskiwania**, kliknij nazwę magazynu **ContosoVMVault**.
2. W **wprowadzenie**, kliknij opcję usługi Site Recovery. Następnie kliknij przycisk **przygotowanie infrastruktury**.
3. W **cel ochrony** > **których komputery znajdujące się**, wybierz pozycję **lokalnymi**.
4. W ** gdzie chcesz zreplikować maszyny, wybierz **do platformy Azure**.
5. W **są maszynach zwirtualizowanych**, wybierz pozycję **tak, z programem VMware vSphere Hypervisor**. Następnie kliknij przycisk **OK**.

## <a name="set-up-the-source-environment"></a>Konfigurowanie środowiska źródłowego

Aby skonfigurować środowisko źródłowe, należy jeden wysokiej dostępności, na lokalnej maszynie składników usługi Site Recovery lokalnego hosta. Składniki zawierają konfiguracji serwera, serwer przetwarzania i główny serwer docelowy.

- Serwer konfiguracji służy do koordynowania komunikacji między środowiskiem lokalnym i platformą Azure oraz do zarządzania replikacją danych.
- Serwer przetwarzania działa jako brama replikacji. Odbiera dane replikacji, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła je do usługi Azure Storage. Serwer przetwarzania instaluje usługi mobilności na maszynach wirtualnych, którą chcesz replikować, i przeprowadza automatyczne odnajdywanie maszyn wirtualnych VMware lokalnymi.
- Główny serwer docelowy obsługuje replikację danych podczas powrotu po awarii z platformy Azure.

Aby skonfigurować serwer konfiguracji jako wysokiej dostępności maszyny Wirtualnej VMware, Pobierz przygotować szablon pakietu OVF i zaimportuj szablon do VMware można utworzyć maszyny Wirtualnej. Po skonfigurowaniu serwera konfiguracji, Zarejestruj w magazynie. Po rejestracji usługi Site Recovery odnajduje lokalne maszyny wirtualne VMware.

### <a name="download-the-vm-template"></a>Pobierz szablon maszyny Wirtualnej

1. W magazynie, przejdź do **przygotowanie infrastruktury** > **źródła**.
2. W **Przygotuj źródło**, kliknij przycisk **+ serwer konfiguracji**.
3. W **Dodaj serwer**, sprawdź, czy **serwera konfiguracji na potrzeby VMware** pojawia się w **typ serwera**.
4. Pobierz szablon Open Virtualization Format (OVF) dla serwera konfiguracji.

  > [!TIP]
  Można pobrać najnowszej wersji szablonu serwera konfiguracji bezpośrednio z [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

## <a name="import-the-template-in-vmware"></a>Importowanie szablonu w środowisku programu VMware

1. Logowania się do programu VMware vCenter server lub vSphere ESXi hosta, za pomocą VMWare vSphere Client.
2. Na **pliku** menu, wybierz opcję **wdrażanie szablonu OVF**, aby uruchomić Kreatora wdrażania pakietu OVF szablonu.  

     ![Szablon OVF](./media/tutorial-vmware-to-azure/vcenter-wizard.png)

3. W **wybierz źródło**, określ lokalizację pobranego pakietu OVF.
4. W **Przejrzyj szczegóły**, kliknij przycisk **dalej**.
5. W **wybierz nazwę i folder**, i **konfiguracji wybierz**, zaakceptuj ustawienia domyślne.
6. W **Wybieranie magazynu**, wybrać najlepszą wydajność **grubości udostępniania wczesny mają być wyzerowane** w **formatu dysku wirtualnego wybierz**.
4. W pozostałej części stron kreatora Zaakceptuj ustawienia domyślne.
5. W **gotowa do wykonania**:
  - Aby skonfigurować maszynę Wirtualną przy użyciu ustawień domyślnych, wybierz **włączają po wdrożeniu** > **Zakończ**.
  - Jeśli chcesz dodać dodatkowy interfejs sieciowy, wyczyść **włączają po wdrożeniu**, a następnie wybierz **Zakończ**. Domyślnie szablon serwera konfiguracji jest wdrażany z jednej karty Sieciowej, ale można dodać dodatkowe karty sieciowe, po wdrożeniu.

  
## <a name="add-an-additional-adapter"></a>Dodaj dodatkowe karty

Jeśli chcesz dodać dodatkowe karty Sieciowej na serwerze konfiguracji, należy to zrobić, aby zarejestrować serwer w magazynie. Dodawanie dodatkowych kart sieciowych nie jest obsługiwana po rejestracji.

1. W vSphere spisu klienta, kliknij prawym przyciskiem myszy maszynę Wirtualną, a następnie wybierz **Edytuj ustawienia**.
2. W **sprzętu**, kliknij przycisk **Dodaj** > **karty Ethernet**. Następnie kliknij przycisk **Next** (Dalej).
3. Wybierz i typ karty i sieci. 
4. Aby Podłącz wirtualną kartę Sieciową, gdy maszyna wirtualna jest włączona, wybierz **na połączenie na mocy**. Kliknij przycisk **dalej** > **Zakończ**, a następnie kliknij przycisk **OK**.


## <a name="register-the-configuration-server"></a>Zarejestruj serwer konfiguracji 

1. Z konsoli klienta VMWare vSphere włączyć na maszynie Wirtualnej.
2. Maszyna wirtualna wykona rozruch do środowiska instalacji systemu Windows Server 2016. Zaakceptuj Umowę licencyjną i określ hasło administratora.
3. Po zakończeniu instalacji, zaloguj się do maszyny Wirtualnej jako administrator.
4. Podczas pierwszego logowania, uruchamia narzędzie konfiguracji Azure lokacji odzyskiwania.
5. Określ nazwę, która służy do rejestrowania serwera konfiguracji z usługą Site Recovery. Następnie kliknij przycisk **Next** (Dalej).
6. Narzędzie sprawdza, czy połączenie maszyny Wirtualnej Azure. Po nawiązaniu połączenia, kliknij przycisk **Zaloguj**, aby zalogować się do subskrypcji platformy Azure. Poświadczenia muszą mieć dostęp do magazynu, w którym chcesz zarejestrować serwer konfiguracji.
7. Narzędzie wykonuje pewne zadania konfiguracji, a następnie wykonuje ponowny rozruch.
8. Ponownie zaloguj się na tym komputerze. Automatycznie uruchomi Kreatora konfiguracji serwera zarządzania.

### <a name="configure-settings-and-connect-to-vmware"></a>Skonfiguruj ustawienia i połącz się VMware

1. W Kreatorze konfiguracji serwera zarządzania > **ustawienia łączności**, wybierz kartę Sieciową, która będzie odbierać ruch związany z replikacją. Następnie kliknij przycisk **Save** (Zapisz). Nie można zmienić to ustawienie jest skonfigurowane.
2. W **magazyn usług odzyskiwania wybierz**, wybierz subskrypcję platformy Azure i odpowiedniej grupy zasobów i magazynu.
3. W **zainstalować oprogramowanie innych firm**, Zaakceptuj agreeemtn licencji i kliknij przycisk **Pobierz i zainstaluj**, aby zainstalować serwer MySQL.
4. Kliknij przycisk **zainstalować VMware PowerLCI**. Upewnij się, że przed tym zamknięciu wszystkich okien przeglądarki. Następnie kliknij przycisk **Kontynuuj**
5. W **sprawdzania poprawności konfiguracji urządzenia**, będzie można zweryfikować wymagania wstępne, przed kontynuowaniem.
6. W **Konfiguruj vCenter Server/vSphere ESXi serwer**, określ nazwę FQDN lub adres IP serwera vCenter lub hostem vSphere, na maszynach wirtualnych, które chcesz replikować znajdują się. Określ port, na którym nasłuchuje serwer i przyjazną nazwę używanego do serwera VMware w magazynie.
7. Określ poświadczenia, które będą używane przez serwer konfiguracji do łączenia się z serwerem VMware. Usługa Site Recovery używa tych poświadczeń do automatycznego wykrywania maszyn wirtualnych VMware, które są dostępne dla replikacji. Kliknij przycisk **Dodaj**, a następnie kliknij przycisk **Kontynuuj**.
8. W **skonfigurować poświadczenia maszyny wirtualnej**, określ nazwę użytkownika i hasło, które będzie używane automatycznie zainstalować usługi mobilności na maszynach, po włączeniu replikacji. W przypadku komputerów z systemem Windows konto musi uprawnienia administratora lokalnego na maszynach, które mają być replikowane. Dla systemu Linux należy podać szczegóły dla konta głównego.
9. Kliknij przycisk **konfiguracji Finalize** do ukończenia rejestracji. 
10. Po zakończeniu rejestracji w portalu Azure, sprawdź, czy serwer konfiguracji i serwera VMware są wyświetlane **źródła** strony w magazynie. Następnie kliknij przycisk **OK** do konfigurowania ustawień obiektu docelowego.


Odzyskiwanie lokacji łączy się przy użyciu określonych ustawień serwerów VMware i odnajduje maszyn wirtualnych.

> [!NOTE]
> Zanim nazwa konta pojawi się w portalu, może minąć 15 minut lub więcej. Aby wykonać natychmiastową aktualizację, kliknij pozycje **Serwery konfiguracji** > ***nazwa serwera*** > **Odśwież serwer**.

## <a name="set-up-the-target-environment"></a>Konfigurowanie środowiska docelowego

Wybierz i zweryfikować zasobów docelowych.

1. Kliknij kolejno pozycje **Przygotowywanie infrastruktury** > **Docelowa** i wybierz subskrypcję platformy Azure do użycia.
2. Określ, czy docelowy modelu wdrażania jest oparte na Menedżera zasobów albo klasycznego.
3. Usługa Site Recovery sprawdza, czy masz co najmniej jedno zgodne konto magazynu Azure i co najmniej jedną sieć platformy Azure.

   ![Obiekt docelowy](./media/tutorial-vmware-to-azure/storage-network.png)

## <a name="create-a-replication-policy"></a>Tworzenie zasad replikacji

1. Otwórz [portalu Azure](https://portal.azure.com) i wybierz polecenie **wszystkie zasoby**.
2. Kliknięcie w magazynie usług odzyskiwania o nazwie **ContosoVMVault**.
3. Aby utworzyć zasady replikacji, kliknij przycisk **infrastruktura usługi Site Recovery** > **zasady replikacji** > **+ zasad replikacji**.
4. W **Utwórz zasady replikacji**, określ nazwę zasady **VMwareRepPolicy**.
5. W **próg RPO**, użyj domyślnej 60 minut. Ta wartość określa, jak często są tworzone punkty odzyskiwania. Alert jest generowany, gdy ciągłej replikacji przekracza ten limit.
6. W **przechowywania punktu odzyskiwania**, jak długo trwa okna przechowywania dla każdego punktu odzyskiwania na użytek domyślny 24 godziny. W tym samouczku będziemy wybierz 72 godzin. Replikowane maszyny wirtualne można odzyskać do dowolnego punktu w oknie.
7. W **częstotliwość migawek spójności aplikacji**, użyj domyślnej 60 minut częstotliwości utworzenia migawki spójne z aplikacjami. Kliknij przycisk **OK** do tworzenia zasad.

   ![Zasady](./media/tutorial-vmware-to-azure/replication-policy.png)

Zasady są automatycznie skojarzone z serwerem konfiguracji. Domyślnie zasady uzgadniania jest tworzony automatycznie powrotu po awarii. Na przykład, jeśli zasady replikacji jest **zasad rep** zasady powrotu po awarii będzie **rep zasad-powrotu po awarii**. Ta zasada nie jest używany, dopiero po zainicjowaniu powrotu po awarii z platformy Azure.

## <a name="enable-replication"></a>Włączanie replikacji

Usługa Site Recovery instaluje usługi mobilności, po włączeniu replikacji dla maszyny Wirtualnej. Może zająć 15 minut ani już aby zmiany zaczęły efektu i wyświetlane w portalu.

Włącz replikację w następujący sposób:

1. Kliknij przycisk **Replikowanie aplikacji** > **źródła**.
2. W **źródła**, wybierz serwer konfiguracji.
3. W **komputera typu**, wybierz pozycję **maszyn wirtualnych**.
4. W **vCenter/vSphere Hypervisor**, wybierz serwer vCenter, który zarządza hostem vSphere lub wybierz host.
5. Wybierz serwer przetwarzania (serwer konfiguracji). Kliknij IThen **OK**.
6. W **docelowego**, wybierz subskrypcji i grupy zasobów, w którym chcesz utworzyć nieudane przez maszyny wirtualne. Wybierz model wdrażania, który ma być używany na platformie Azure (Zarządzanie zasobu lub classic) dla w trybie Failover maszyny wirtualne.
7. Wybierz konto magazynu Azure, którego chcesz użyć do replikacji danych.
8. Wybierz sieć platformy Azure i podsieć, z którą nawiążą połączenie maszyny wirtualne platformy Azure, gdy zostaną uruchomione po przejściu do trybu failover.
9. Wybierz opcję **Konfiguruj teraz dla wybranych maszyn**, aby zastosować ustawienia sieci do wszystkich maszyn wybranych do ochrony. Wybierz opcję **Konfiguruj później**, aby wybrać sieć platformy Azure dla poszczególnych maszyn.
10. W pozycji **Maszyny wirtualne** > **Wybierz maszyny wirtualne** kliknij i zaznacz każdą maszynę, którą chcesz replikować. Możesz wybrać tylko te maszyny, dla których można włączyć replikację. Następnie kliknij przycisk **OK**.
11. W **właściwości** > **skonfigurować właściwości**, wybierz konto, które będzie używane przez serwer przetwarzania Aby automatycznie zainstalować usługi mobilności na maszynie.
12. W **ustawienia replikacji** > **Konfigurowanie ustawień replikacji**, sprawdź, czy jest wybrane zasady replikacji poprawne.
13. Kliknij przycisk **włączyć replikację**.

Możesz śledzić postęp **Włącz ochronę** zadania w **ustawienia** > **zadania** > **zadania usługi Site Recovery**. Po uruchomieniu zadania **Sfinalizuj ochronę** maszyna jest gotowa do przejścia w tryb failover.

Do monitorowania maszyn wirtualnych, możesz dodać, można sprawdzić czas ostatniego odnalezionych dla maszyn wirtualnych w **serwery konfiguracji**
> **Ostatni kontakt w**. Aby dodać maszyn wirtualnych bez oczekiwania na zaplanowanego odnajdywania, zaznacz serwer konfiguracji (nie klikaj pozycji go) i kliknij przycisk **Odśwież**.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Uruchamianie próbnego odzyskiwania po awarii](site-recovery-test-failover-to-azure.md)
