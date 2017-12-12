---
title: "Konfigurowanie odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnych z usługą Azure Site Recovery | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować odzyskiwania po awarii do platformy Azure dla maszyn wirtualnych VMware lokalnych z usługą Azure Site Recovery."
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 5810ff908d48fc4ff742d734e7c2457fdfe8cb03
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/11/2017
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

Aby skonfigurować środowisko źródłowe, możesz pobrać plik Instalatora Unified odzyskiwania lokacji. Możesz uruchomić Instalatora, aby zainstalować składniki usługi Site Recovery lokalnymi, rejestrowanie serwerów VMware w magazynie i odnajdywanie maszyn wirtualnych lokalnie.

### <a name="verify-on-premises-site-recovery-requirements"></a>Sprawdź wymagania dotyczące odzyskiwania lokacji lokalnej

Należy składnikami usługi Site Recovery lokalnego hosta VMware jednej, wysokiej dostępności, lokalnej maszyny Wirtualnej. Składniki zawierają konfiguracji serwera, serwer przetwarzania i główny serwer docelowy.

- Serwer konfiguracji służy do koordynowania komunikacji między środowiskiem lokalnym i platformą Azure oraz do zarządzania replikacją danych.
- Serwer przetwarzania działa jako brama replikacji. Odbiera dane replikacji, optymalizuje je przy użyciu pamięci podręcznej, kompresji i szyfrowania, a następnie wysyła je do usługi Azure Storage. Serwer przetwarzania instaluje usługi mobilności na maszynach wirtualnych, którą chcesz replikować, i przeprowadza automatyczne odnajdywanie maszyn wirtualnych VMware lokalnymi.
- Główny serwer docelowy obsługuje replikację danych podczas powrotu po awarii z platformy Azure.

Maszyna wirtualna powinna spełniać następujące wymagania.

| **Wymaganie** | **Szczegóły** |
|-----------------|-------------|
| Liczba rdzeni procesora CPU| 8 |
| Pamięć RAM | 12 GB |
| Liczba dysków | 3 - dysk systemu operacyjnego, dysku pamięci podręcznej serwera przetwarzania, dysk przechowywania (w przypadku powrotu po awarii) |
| Wolne miejsce na dysku (pamięć podręczna serwera przetwarzania) | 600 GB |
| Wolne miejsce na dysku (dysk przechowywania) | 600 GB |
| Wersja systemu operacyjnego | Windows Server 2012 R2 |
| Ustawienia regionalne systemu operacyjnego | Angielski (en-us) |
| Wersja programu VMware vSphere PowerCLI | [PowerCLI 6.0](https://my.vmware.com/web/vmware/details?productId=491&downloadGroup=PCLI600R1 "PowerCLI 6.0") |
| Role systemu Windows Server | Nie włączaj tych ról: Active Directory Domain Services, Internetowe usługi informacyjne funkcji Hyper-V |
| Typ karty Sieciowej | VMXNET3 |
| Typ adresu IP | Statyczny |
| Porty | 443 (organizowanie kanału sterowania)<br/>9443 (transport danych)|

Ponadto: 
- Upewnij się, że zegara systemowego na maszynie Wirtualnej jest zsynchronizowany z czasem serwera. Czas musi być synchronizowane w ciągu 15 minut. Jeśli jest ona większa instalacja zakończy się niepowodzeniem.
Instalacja nie powiedzie się.
- Upewnij się, że serwer konfiguracji maszyny Wirtualnej można uzyskać dostępu do tych adresów URL:

    [!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]
    
- Upewnij się, że reguły zapory oparte na adresie IP umożliwia komunikację z platformą Azure.
    - Zezwalaj na [zakresy IP centrum danych Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653)portu 443 (HTTPS) i portu 9443 (replikacja danych).
    - Zezwalaj na zakresy adresów IP dla regionu Azure Twojej subskrypcji i zachodnie stany USA (używanych do zarządzania tożsamości i kontroli dostępu).


### <a name="download-the-site-recovery-unified-setup-file"></a>Pobierz plik Instalatora Unified usługi Site Recovery

1. W magazynie > **przygotowanie infrastruktury**, kliknij przycisk **źródła**.
1. W **Przygotuj źródło**, kliknij przycisk **+ serwer konfiguracji**.
2. W **Dodaj serwer**, sprawdź, czy **serwera konfiguracji** pojawia się w **typ serwera**.
3. Pobierz plik instalacyjny instalacja Unified usługi Site Recovery.
4. Pobierz klucz rejestracji magazynu. Należy to po uruchomieniu Instalatora Unified. Klucz jest ważny przez pięć dni po jego wygenerowaniu.

   ![Konfiguracja źródła](./media/tutorial-vmware-to-azure/source-settings.png)

### <a name="set-up-the-configuration-server"></a>Konfigurowanie serwera konfiguracji

1. Uruchom plik instalacyjny ujednoliconego instalatora.
2. W **przed rozpoczęciem**, wybierz pozycję **zainstalować serwer konfiguracji i serwera przetwarzania** kliknięcie **dalej**.

3. W **licencji na oprogramowanie innych firm**, kliknij przycisk **akceptuję** Aby pobrać i zainstalować MySQL, a następnie przycisk **dalej**.

4. W obszarze **Rejestracja** wybierz klucz rejestracji pobrany z magazynu.

5. W obszarze **Ustawienia internetowe** określ, jak dostawca działający na serwerze konfiguracji ma się łączyć z usługą Azure Site Recovery przez Internet.

   - Jeśli chcesz się połączyć z serwerem proxy aktualnie skonfigurowany na komputerze, wybierz opcję **nawiązywanie połączenia z usługi Azure Site Recovery przy użyciu serwera proxy**.
   - Jeśli chcesz, aby dostawca łączył się bezpośrednio, wybierz opcję **Połącz bezpośrednio z usługi Azure Site Recovery bez serwera proxy**.
   - Jeśli istniejący serwer proxy wymaga uwierzytelnienia, lub jeśli chcesz użyć niestandardowego serwera proxy dla połączenia dostawcy, wybierz opcję **Połącz przy użyciu niestandardowych ustawień serwera proxy**i określ adres, port oraz poświadczenia.

   ![Zapora](./media/tutorial-vmware-to-azure/combined-wiz4.png)

6. W kroku **Sprawdzanie wymagań wstępnych** Instalator przeprowadza kontrolę w celu upewnienia się, że można uruchomić instalację. Jeśli zostanie wyświetlone ostrzeżenie dotyczące **kontroli synchronizacji czasu globalnego**, sprawdź, czy czas zegara systemowego (ustawienia **Data i godzina**) jest taki sam jak dla strefy czasowej.

   ![Wymagania wstępne](./media/tutorial-vmware-to-azure/combined-wiz5.png)

7. W obszarze **Konfiguracja programu MySQL** utwórz poświadczenia do logowania się do zainstalowanego wystąpienia serwera programu MySQL.

8. W **szczegóły środowiska**, wybierz pozycję **tak** można chronić maszyny wirtualne VMware. Instalator sprawdza, czy zainstalowano PowerCLI 6.0.

9. W obszarze **Lokalizacja instalacji** wybierz, gdzie mają zostać zainstalowane pliki binarne i gdzie ma być przechowywana pamięć podręczną. Na wybranym dysku musi być co najmniej 5 GB dostępnego miejsca, ale zalecamy dysk pamięci podręcznej z co najmniej 600 GB wolnego miejsca.

10. W obszarze **Wybór sieci** określ odbiornik (kartę sieciową i port SSL), za pomocą którego serwer konfiguracji będzie wysyłać i odbierać dane replikacji. Port 9443 jest domyślnym portem używanym do wysyłania i odbierania ruchu związanego z replikacją, ale możesz zmienić ten numer portu tak, aby odpowiadał wymaganiom Twojego środowiska. Możemy również otworzyć port 443, który jest używany do organizowania operacji replikacji. Nie należy używać portu 443 do wysyłania i odbierania ruchu związanego z replikacją.

11. W obszarze **Podsumowanie** przejrzyj informacje i kliknij przycisk **Zainstaluj**. Instalator instaluje serwer konfiguracji i rejestruje go usługi Azure Site Recovery.

    ![Podsumowanie](./media/tutorial-vmware-to-azure/combined-wiz10.png)

    Po zakończeniu instalacji generowane jest hasło. Będzie ono potrzebne po włączeniu replikacji, dlatego skopiuj je i przechowuj w bezpiecznym miejscu. Serwer jest wyświetlany na **ustawienia** > **serwerów** okienko w magazynie.

### <a name="configure-automatic-discovery"></a>Konfigurowanie automatycznego odnajdowania

Aby odnaleźć maszyn wirtualnych, serwer konfiguracji musi łączyć się z lokalnymi serwerami VMware. Do celów tego samouczka należy dodać serwer vCenter lub hostach vSphere, przy użyciu konta z uprawnieniami administratora na serwerze. Utworzono tego konta w [samouczek poprzedniej](tutorial-prepare-on-premises-vmware.md). 

Aby dodać konto:

1. Na serwerze konfiguracji maszyny Wirtualnej, należy uruchomić **CSPSConfigtool.exe**. Jest on dostępny jako skrót na pulpicie i znajduje się w folderze *lokalizacja instalacji*\home\svsystems\bin.

2. Kliknij pozycje **Zarządzaj kontami** > **Dodaj konto**.

   ![Dodawanie konta](./media/tutorial-vmware-to-azure/credentials1.png)

3. W obszarze **Szczegóły konta** dodaj konto, które będzie używane do automatycznego wykrywania.

   ![Szczegóły](./media/tutorial-vmware-to-azure/credentials2.png)

Aby dodać serwer VMware:

1. Otwórz [portalu Azure](https://portal.azure.com) i wybierz polecenie **wszystkie zasoby**.
2. Kliknięcie w magazynie usług odzyskiwania o nazwie **ContosoVMVault**.
3. Kliknij przycisk **lokacji odzyskiwania** > **przygotowanie infrastruktury** > **źródła**
4. Wybierz **+ vCenter**, aby połączyć się z hostem ESXi vCenter, jak serwer lub vSphere.
5. W **dodać vCenter**, określ przyjazną nazwę dla serwera. Następnie określ adres IP lub nazwę FQDN.
6. Pozostaw portu 443, ustawić, chyba że serwerów VMware nasłuchiwać żądań na innym porcie.
7. Wybierz konto do użycia podczas połączenia z serwerem. Kliknij przycisk **OK**.

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
