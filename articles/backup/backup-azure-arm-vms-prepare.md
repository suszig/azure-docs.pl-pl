---
title: 'Kopia zapasowa Azure: Przygotowanie do tworzenia kopii zapasowych maszyn wirtualnych | Dokumentacja firmy Microsoft'
description: "Upewnij się, że środowisko jest przygotowane do tworzenia kopii zapasowych maszyn wirtualnych na platformie Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Tworzenie kopii zapasowych; Tworzenie kopii zapasowej;
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/3/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 9b3584a93766be6052c822f40328169910de26c7
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Przygotowywanie środowiska do tworzenia kopii zapasowych maszyn wirtualnych wdrożonych przez program Resource Manager

Ten artykuł zawiera kroki przygotowania środowiska do tworzenia kopii zapasowych wdrożonych przez Menedżera zasobów maszyny wirtualnej (VM). Kroki opisane w procedurach za pomocą portalu Azure.  

Usługa Kopia zapasowa Azure ma dwa typy magazynów (kopia zapasowa magazynów i magazyny usług odzyskiwania) dla ochrony maszyn wirtualnych. Magazyn kopii zapasowych chroni maszyn wirtualnych wdrożonych przy użyciu klasycznego modelu wdrożenia. Chroni magazyn usług odzyskiwania **maszyn wirtualnych zarówno wdrożone w klasycznej lub wdrożeniu usługi Resource Manager**. Ochronę maszyny Wirtualnej wdrożone usługi Resource Manager, należy użyć magazynu usług odzyskiwania.

> [!NOTE]
> Platforma Azure ma dwa modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [Resource Manager i model klasyczny](../azure-resource-manager/resource-manager-deployment-model.md). 

Zanim można chronić lub utworzyć kopię zapasową wdrożonych przez Menedżera zasobów maszyny wirtualnej (VM), upewnij się, że istnieją następujące wymagania wstępne:

* Tworzenie magazynu usług odzyskiwania (lub Zidentyfikuj istniejącego magazynu usług odzyskiwania) *w tej samej lokalizacji co maszyna wirtualna*.
* Wybierz scenariusz, definiowanie zasad tworzenia kopii zapasowej i zdefiniuj elementy do ochrony.
* Sprawdź instalację agenta maszyny Wirtualnej na maszynie wirtualnej.
* Sprawdź łączność z siecią
* Dla maszyn wirtualnych systemu Linux, w przypadku, gdy chcesz dostosować środowisko tworzenia kopii zapasowej dla aplikacji spójne tworzenie kopii zapasowych, zapoznaj się z wykonaj [kroki, aby skonfigurować skrypty migawki przed i po wykonaniu migawki](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)

Jeśli znasz te warunki już istnieje w danym środowisku, a następnie przejdź do [kopii zapasowych maszyn wirtualnych artykuł](backup-azure-arm-vms.md). Jeśli chcesz skonfigurować lub sprawdzanie wszystkich wymagań wstępnych w tym artykule poprowadzi Cię przez kroki, aby przygotować tej wstępnie wymaganego.

##<a name="supported-operating-system-for-backup"></a>Obsługiwany system operacyjny do utworzenia kopii zapasowej
 * **Linux**: usługa Azure Backup obsługuje [dystrybucje zalecane dla platformy Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) z wyjątkiem systemu operacyjnego Linux Core. _Innych Bring-Your-właścicielem — dystrybucje systemu Linux mogą również działać, dopóki agent maszyny Wirtualnej jest dostępne na maszynie wirtualnej i obsługę języka Python istnieje. Jednak firma Microsoft nie zatwierdza tych dystrybucji dla kopii zapasowej._
 * **Windows Server**: wersje starsze niż Windows Server 2008 R2 nie są obsługiwane.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Ograniczenia w przypadku tworzenia kopii zapasowej i przywracanie maszyny Wirtualnej
Aby przygotować środowisko, należy zapoznać się z ograniczeniami.

* Tworzenie kopii zapasowych maszyn wirtualnych z więcej niż 16 dysków danych nie jest obsługiwane.
* Tworzenie kopii zapasowych maszyn wirtualnych z danymi dysku o rozmiarze przekraczającym 1023GB nie jest obsługiwane.

> [!NOTE]
> Dysponujemy prywatną wersją zapoznawczą obsługującą kopie zapasowe maszyn wirtualnych z niezarządzanymi dyskami większymi niż 1 TB. Aby uzyskać szczegółowe informacje, zobacz [prywatnej wersji zapoznawczej do obsługi kopii zapasowych dużych dysków maszyny Wirtualnej](https://gallery.technet.microsoft.com/Instant-recovery-point-and-25fe398a)
>
>

* Tworzenie kopii zapasowych maszyn wirtualnych z zastrzeżonego adresu IP i nie zdefiniowanych punktów końcowych nie jest obsługiwane.
* Kopia zapasowa maszyn wirtualnych jest szyfrowana przy użyciu tylko BEK nie jest obsługiwane. Kopia zapasowa szyfrowane przy użyciu szyfrowania LUKS maszyn wirtualnych systemu Linux nie jest obsługiwane.
* Kopia zapasowa maszyn wirtualnych zawierających Volumes(CSV) udostępnionych klastra lub skalę limit konfiguracji serwera plików nie jest zalecane, ponieważ wymagają one, obejmujące wszystkich maszyn wirtualnych uwzględnione w konfiguracji klastra podczas zadania migawki. Kopia zapasowa Azure nie obsługuje wielu maszyn wirtualnych. 
* Dane kopii zapasowej nie zawiera dyski sieciowe zainstalowane podłączona do maszyny Wirtualnej.
* Zamiana istniejącej maszyny wirtualnej podczas przywracania nie jest obsługiwana. Jeśli podjęto próbę przywrócenia maszyny Wirtualnej, gdy maszyna wirtualna istnieje, operacja przywracania kończy się niepowodzeniem.
* Region między kopii zapasowej i przywracania nie są obsługiwane.
* Można tworzyć kopie zapasowe maszyn wirtualnych we wszystkich regionach publicznej platformy Azure (zobacz [Lista kontrolna](https://azure.microsoft.com/regions/#services) obsługiwanych regionów). Jeśli obecnie jest obsługiwany region, którego szukasz, nie zostanie wyświetlony na liście rozwijanej podczas tworzenia magazynu.
* Przywracanie kontrolera domeny (DC) maszyny Wirtualnej, która jest częścią konfiguracji kontrolera domeny na wielu jest obsługiwane tylko za pomocą programu PowerShell. Przeczytaj więcej na temat [Przywracanie kontrolera domeny, kontrolera domeny na wielu](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Przywracanie maszyn wirtualnych, które mają następujące konfiguracje sieciowe specjalne jest obsługiwane tylko za pomocą programu PowerShell. Maszyny wirtualne utworzone za pomocą przepływu pracy przywracania w interfejsie użytkownika nie będą miały te konfiguracje sieci po zakończeniu operacji przywracania. Aby dowiedzieć się więcej, zobacz [przywracanie maszyn wirtualnych z konfiguracjami sieci specjalne](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).

  * Maszyny wirtualne znajdujące się w konfiguracji usługi równoważenia obciążenia (wewnętrznych i zewnętrznych)
  * Maszyny wirtualne z wielu zastrzeżonych adresów IP
  * Maszyny wirtualne z wieloma kartami sieciowymi

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Tworzenie magazynu usługi Recovery Services dla maszyny wirtualnej
Magazyn usług odzyskiwania jest jednostka, która przechowuje kopie zapasowe i punkty odzyskiwania, które zostały utworzone w czasie. Magazyn usług odzyskiwania zawiera również zasady tworzenia kopii zapasowej skojarzonego z chronionych maszyn wirtualnych.

Aby utworzyć magazyn usługi Recovery Services:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. W menu Centrum kliknij opcję **Przeglądaj** i na liście zasobów wpisz ciąg **Usługi odzyskiwania**. Po rozpoczęciu pisania zawartość listy będzie filtrowana w oparciu o wpisywane dane. Kliknij opcję **Magazyn Usług odzyskiwania**.

    ![Kliknij przycisk Przeglądaj, a następnie wpisz usług odzyskiwania. Wyświetlenie opcji magazynu usług odzyskiwania, kliknij go, aby otworzyć blok magazyn usług odzyskiwania.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Zostanie wyświetlona lista magazynów usług odzyskiwania.
3. W menu **Magazyny usługi Recovery Services** kliknij pozycję **Dodaj**.

    ![Tworzenie magazynu Usług odzyskiwania — krok 2](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    Zostanie otwarty blok magazynu Usług odzyskiwania i pojawi się monit o podanie wartości w polach **Nazwa**, **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**.

    ![Tworzenie magazynu Usług odzyskiwania — krok 5](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Nazwa musi być unikalna w tej subskrypcji platformy Azure. Wpisz nazwę o długości od 2 do 50 znaków. Musi ona rozpoczynać się od litery i może zawierać tylko litery, cyfry i łączniki.
5. Kliknij pozycję **Subskrypcja**, aby wyświetlić listę dostępnych subskrypcji. Jeśli nie masz pewności, której subskrypcji użyć, wybierz domyślną (lub sugerowaną). Większa liczba opcji do wyboru będzie dostępna tylko w przypadku, gdy konto organizacji jest skojarzone z wieloma subskrypcjami platformy Azure.
6. Kliknij pozycję **Grupa zasobów**, aby wyświetlić listę dostępnych grup zasobów, lub kliknij pozycję **Nowa**, aby utworzyć nową grupę zasobów. Pełne informacje na temat grup zasobów można znaleźć w temacie [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
7. Kliknij pozycję **Lokalizacja**, aby wybrać region geograficzny magazynu. Magazyn **musi** znajdować się w tym samym regionie, w którym znajdują się maszyny wirtualne, które mają być chronione.

   > [!IMPORTANT]
   > Jeśli nie wiesz, w jakiej lokalizacji znajdują się Twoje maszyny wirtualne, zamknij okno dialogowe tworzenia magazynu, a następnie przejdź do listy maszyn wirtualnych w portalu. Jeśli Twoje maszyny wirtualne znajdują się w wielu regionach, należy utworzyć magazyn Usług odzyskiwania w każdym regionie. Przed przejściem do następnej lokalizacji utwórz magazyn w pierwszej lokalizacji. Nie ma potrzeby określania kont magazynu do przechowywania kopii zapasowych danych — magazyn Usług odzyskiwania i usługa Azure Backup określają je automatycznie.
   >
   >

8. Kliknij przycisk **Utwórz**. Utworzenie magazynu Usług odzyskiwania może zająć trochę czasu. Monitoruj powiadomienia o stanie wyświetlane w prawej górnej części obszaru portalu. Po utworzeniu magazynu pojawi się on na liście magazynów Usług odzyskiwania. Jeśli nie widzisz magazynu, kliknij przycisk **Odśwież** do

    ![Lista magazynów kopii zapasowych](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Magazyn został już utworzony, więc teraz zajmiemy się skonfigurowaniem jego replikacji.

## <a name="set-storage-replication"></a>Konfigurowanie replikacji magazynu
Dla opcji replikacji magazynu można wybrać magazynowanie nadmiarowe geograficznie lub lokalnie. Domyślnie magazyn jest nadmiarowy geograficznie. Pozostaw tę opcję ustawioną na magazyn geograficznie nadmiarowy, jeśli jest to Twoja podstawowa kopia zapasowa. Wybierz magazyn lokalnie nadmiarowy, jeśli chcesz skorzystać z tańszej, ale mniej trwałej opcji.

Aby edytować ustawienia replikacji magazynu:

1. Na **Magazyny usług odzyskiwania** bloku, wybierz magazyn.
    Po kliknięciu magazynie, blok ustawień (*mającego nazwę magazynu u góry*) i magazynu zawiera szczegóły blok zostanie otwarta.

    ![Wybierz z listy magazynów kopii zapasowych magazynu](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Na **ustawienia** bloku, przewiń w dół za pomocą suwaka pionowego **Zarządzaj** sekcji. Kliknij przycisk **infrastruktura kopii zapasowej** aby otworzyć jego blok. W **ogólne** kliknij sekcję **konfiguracji kopii zapasowej** aby otworzyć jego blok. W bloku **Konfiguracja kopii zapasowej** wybierz opcję replikacji swojego magazynu. Domyślnie magazyn jest nadmiarowy geograficznie. Jeśli zmienisz typ replikacji magazynu, kliknij przycisk **zapisać**.

    ![Lista magazynów kopii zapasowych](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Jeśli używasz platformy Azure jako punktu końcowego podstawowego magazynu kopii zapasowych, kontynuuj korzystanie z magazynu geograficznie nadmiarowego. Jeśli używasz platformy Azure jako punktu końcowego magazynu kopii zapasowych innego niż podstawowy, następnie wybierz magazyn lokalnie nadmiarowy. Więcej informacji o opcjach magazynu [geograficznie nadmiarowego](../storage/common/storage-redundancy.md#geo-redundant-storage) i [lokalnie nadmiarowego](../storage/common/storage-redundancy.md#locally-redundant-storage) można znaleźć w temacie [Azure Storage replication overview](../storage/common/storage-redundancy.md) (Omówienie replikacji usługi Azure Storage).
    Po wybraniu opcji magazynu dla swojego magazynu możesz skojarzyć z nim maszynę wirtualną. Aby rozpocząć kojarzenie, należy odnaleźć i zarejestrować maszyny wirtualne Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Wybieranie celu tworzenia kopii zapasowej, ustawianie zasad i definiowanie elementów podlegających ochronie
Przed zarejestrowaniem maszyny wirtualnej w magazynie uruchom proces wykrywania, aby mieć pewność, że zostały znalezione wszystkie nowe maszyny wirtualne dodane do subskrypcji. Proces wyszukuje na platformie Azure listę maszyn wirtualnych w ramach subskrypcji wraz z dodatkowymi informacjami, takimi jak nazwa usługi w chmurze i region. W portalu Azure scenariusz odnosi się do elementów, które mają zostać umieszczone w magazynie usług odzyskiwania. Zasady są harmonogramem określającym częstotliwość i czas tworzenia punktów odzyskiwania. Zasady zawierają także zakres przechowywania dla punktów odzyskiwania.

1. Jeśli Twój magazyn Usług odzyskiwania jest już otwarty, przejdź do kroku 2. Jeśli nie masz magazyn usług odzyskiwania, Otwórz, następnie otwórz [portalu Azure](https://portal.azure.com/) i w menu centralnym kliknij **więcej usług**.

   * Na liście zasobów wpisz **Usługi odzyskiwania**.
   * Po rozpoczęciu pisania zawartość listy będzie filtrowana w oparciu o wpisywane dane. Po wyświetleniu pozycji **Magazyny Usług odzyskiwania** kliknij ją.

     ![Tworzenie magazynu Usług odzyskiwania — krok 1](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     Zostanie wyświetlona lista magazynów Usług odzyskiwania. Jeśli w Twojej subskrypcji nie żadnego magazynu, ta lista będzie pusta.

    ![Widok listy magazynów usługi Recovery Services](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * Z listy magazynów usług odzyskiwania wybierz magazyn, aby otworzyć jego pulpitu nawigacyjnego.

     Zostanie otwarty blok ustawień i na pulpicie nawigacyjnym magazynu wybranego magazynu.

     ![Otwarcie bloku magazynu](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. W menu pulpitu nawigacyjnego magazynu kliknij pozycję **Kopia zapasowa**, aby otworzyć blok Kopia zapasowa.

    ![Otwarcie bloku Kopia zapasowa](./media/backup-azure-arm-vms-prepare/backup-button.png)

    Zostaną otwarte bloki Kopia zapasowa i Cel kopii zapasowej.

    ![Otwarcie bloku scenariusza](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. W bloku celu kopii zapasowej, należy ustawić **gdzie działa Twoje obciążenie** na platformie Azure i **co chcesz utworzyć kopię zapasową** do maszyny wirtualnej, następnie kliknij przycisk **OK**.

    Spowoduje to zarejestrowanie rozszerzenia maszyny wirtualnej w magazynie. Blok Cel kopii zapasowej zostanie zamknięty, po czym zostanie otwarty blok **Zasady tworzenia kopii zapasowych**.

    ![Otwarcie bloku scenariusza](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. W bloku zasad tworzenia kopii zapasowych wybierz zasady tworzenia kopii zapasowej, które mają zostać zastosowane do magazynu.

    ![Wybór zasad tworzenia kopii zapasowej](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    Szczegóły domyślnych zasad znajdują się w menu rozwijanym. Aby utworzyć nowe zasady, wybierz opcję **Utwórz nowy** z menu rozwijanego. Instrukcje dotyczące definiowania zasad tworzenia kopii zapasowych można znaleźć w sekcji [Definiowanie zasad tworzenia kopii zapasowej](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Kliknij przycisk **OK**, aby skojarzyć zasady tworzenia kopii zapasowych z magazynem.

    Blok Zasady tworzenia kopii zapasowej zostanie zamknięty, po czym zostanie otwarty blok **Wybieranie maszyn wirtualnych**.
5. W bloku **Wybieranie maszyn wirtualnych** wybierz maszyny wirtualne do skojarzenia z określonymi zasadami i kliknij przycisk **OK**.

    ![Wybór obciążenia](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    Wybrana maszyna wirtualna jest weryfikowana. Jeżeli nie maszyn wirtualnych, które może zobaczyć, sprawdź, czy istnieje w tej samej lokalizacji platformy Azure, co magazyn usług odzyskiwania i nie są już chronione w innym magazynie. Lokalizacja magazynu usługi Recovery Services jest wyświetlana na pulpicie nawigacyjnym magazynu.

6. Skoro zdefiniowano wszystkie ustawienia magazynu, w bloku Kopia zapasowa kliknij przycisk **Włącz kopię zapasową**. Ta czynność spowoduje wdrożenie zasad w odniesieniu do magazynu i maszyn wirtualnych. Nie powoduje ona utworzenia początkowego punktu odzyskiwania maszyny wirtualnej.

    ![Włączenie kopii zapasowej](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Po pomyślnym włączeniu kopii zapasowej zasady tworzenia kopii zapasowych zostaną wykonane zgodnie z harmonogramem. Jeśli chcesz wygenerować zadanie tworzenia kopii zapasowej na żądanie do kopii zapasowych maszyn wirtualnych, zobacz [wyzwalając zadanie tworzenia kopii zapasowej](./backup-azure-arm-vms.md#triggering-the-backup-job).

Jeśli masz problemy z zarejestrowaniem maszyny wirtualnej, zobacz poniższe informacje na temat instalowania agenta maszyny Wirtualnej oraz łączność sieciową. Prawdopodobnie nie potrzebujesz następujących informacji w przypadku ochrony maszyn wirtualnych utworzonych na platformie Azure. Jednak po migracji maszyn wirtualnych na platformie Azure, następnie należy poprawnie zainstalowano agenta maszyny Wirtualnej i że maszyna wirtualna może komunikować się z siecią wirtualną.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Instalowanie agenta maszyny wirtualnej na maszynie wirtualnej
Aby rozszerzenie usługi Backup mogło działać, na maszynie wirtualnej Azure musi być zainstalowany agent maszyny wirtualnej. Jeśli maszyna wirtualna została utworzona z poziomu galerii Azure, następnie Agent maszyny Wirtualnej jest już obecny w maszynie wirtualnej. Informacje dotyczące sytuacji, w którym są *nie* korzystanie z maszyn wirtualnych utworzonych z poziomu galerii Azure — na przykład w przypadku migracji maszyny Wirtualnej z lokalnego centrum danych. W takim przypadku agenta maszyny Wirtualnej musi być zainstalowany, aby chronić maszyny wirtualnej. Dowiedz się więcej o [agenta maszyny Wirtualnej](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Jeśli masz problemy z tworzeniem kopii zapasowej maszyny wirtualnej Azure, sprawdź, czy agent maszyny wirtualnej Azure został poprawnie zainstalowany na maszynie wirtualnej (patrz w tabeli poniżej). Poniższa tabela zawiera dodatkowe informacje na temat agenta maszyny wirtualnej dla maszyn wirtualnych systemów Windows i Linux.

| **Operacja** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalowanie agenta maszyny wirtualnej |Pobierz i zainstaluj [plik MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Do ukończenia procesu instalacji niezbędne są uprawnienia administratora. |<li> Zainstaluj najnowszą [agenta systemu Linux](../virtual-machines/linux/agent-user-guide.md). Do ukończenia procesu instalacji niezbędne są uprawnienia administratora. Zaleca się zainstalowanie agenta z repozytorium dystrybucji. Firma Microsoft **nie jest zalecane** Instalowanie agenta maszyny Wirtualnej systemu Linux bezpośrednio z usługi github.  |
| Aktualizowanie agenta maszyny wirtualnej |Aktualizowanie agenta maszyny wirtualnej jest równie proste, jak ponowne zainstalowanie [plików binarnych agenta maszyny wirtualnej](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Upewnij się, że żadna operacja tworzenia kopii zapasowej nie jest uruchomiona podczas aktualizowania agenta maszyny wirtualnej. |Postępuj zgodnie z instrukcjami dotyczącymi [aktualizowania agenta maszyny wirtualnej systemu Linux ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Firma Microsoft zaleca aktualizacji agenta z repozytorium dystrybucji. Firma Microsoft **nie jest zalecane** aktualizacji agenta maszyny Wirtualnej systemu Linux bezpośrednio z usługi github.<br>Upewnij się, że podczas aktualizowania agenta maszyny wirtualnej żadna operacja tworzenia kopii zapasowej nie jest uruchomiona. |
| Sprawdzanie poprawności instalacji agenta maszyny wirtualnej |<li>Przejdź do folderu *C:\WindowsAzure\Packages* w maszynie wirtualnej Azure. <li>Powinien znajdować się w nim plik WaAppAgent.exe.<li> Kliknij plik prawym przyciskiem myszy, przejdź do opcji **Właściwości**, a następnie wybierz kartę **Szczegóły**. W polu Wersja produktu powinna znajdować się wartość 2.6.1198.718 lub wyższa. |Nie dotyczy |

### <a name="backup-extension"></a>Rozszerzenie kopii zapasowej
Po zainstalowaniu agenta na maszynie wirtualnej usługa Azure Backup instaluje rozszerzenie kopii zapasowej do agenta maszyny wirtualnej. Usługa Kopia zapasowa Azure bezproblemowo uaktualnia i poprawek zapasowy numer wewnętrzny.

Rozszerzenie kopii zapasowej jest instalowane przez usługę Backup niezależnie od tego, czy maszyna wirtualna jest uruchomiona. Uruchomiona maszyna wirtualna zapewnia największe prawdopodobieństwo uzyskania punktu odzyskiwania spójnego z aplikacją. Jednak usługa Azure Backup będzie kontynuować tworzenie kopii zapasowej maszyny wirtualnej nawet w sytuacji, gdy jest ona wyłączona i nie można zainstalować rozszerzenia. To tzw. „maszyna wirtualna offline”. W takim przypadku punkt odzyskiwania będzie *awaryjnie spójny*.

## <a name="network-connectivity"></a>Połączenie sieciowe
Aby zarządzać migawek maszyny Wirtualnej, zapasowy numer wewnętrzny musi mieć łączność Azure publicznych adresów IP. Bez prawo łączność z Internetem limit czasu żądania HTTP maszyny wirtualnej i tworzenia kopii zapasowej nie powiedzie się. Jeśli wdrożenie obowiązują ograniczenia dostępu w (za pośrednictwem sieciową grupę zabezpieczeń (NSG), na przykład), wybierz jedną z następujących opcji do prezentowania wyczyść ścieżki dla kopii zapasowej ruchu:

* [Lista dozwolonych adresów IP centrum danych Azure zakresów](http://www.microsoft.com/en-us/download/details.aspx?id=41653) — zobacz artykuł instrukcje w sposób do listy dozwolonych adresów IP.
* Wdrażanie serwera proxy HTTP dla routingu ruchu.

Podczas wybierania opcji, kompromisy należą do zakresu od możliwości zarządzania, kontrolę i kosztów.

| Opcja | Zalety | Wady |
| --- | --- | --- |
| Lista dozwolonych adresów IP, zakresów |Brak dodatkowych kosztów.<br><br>Do otwierania dostępu w grupy NSG, użyj <i>AzureNetworkSecurityRule zestaw</i> polecenia cmdlet. |Złożone, aby zarządzać jako objęte wpływem zakresów IP ulec zmianie.<br><br>Zapewnia dostęp do całej platformy Azure i nie tylko magazynu. |
| Serwer proxy HTTP |Dozwolone adresy URL kontrolę na serwerze proxy za pośrednictwem magazynu.<br>Pojedynczy punkt Internet dostęp do maszyn wirtualnych.<br>Nie może ulec zmiany adresu IP platformy Azure. |Dodatkowe koszty uruchomioną maszynę Wirtualną z oprogramowaniem serwera proxy. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Zakresy IP dozwolonych centrum danych Azure
* Do listy dozwolonych zakresy IP centrum danych Azure, zobacz [witryny sieci Web Azure](http://www.microsoft.com/en-us/download/details.aspx?id=41653) szczegółowe informacje na temat zakresów adresów IP i instrukcje.
* Można użyć znaczników usługi umożliwia nawiązywanie połączeń z magazynem programu przy użyciu określonego regionu [tagi usługi](../virtual-network/security-overview.md#service-tags). Upewnij się, że tej reguły, która umożliwia uzyskanie dostępu do konta magazynu ma priorytet wyższy niż reguła blokuje dostęp do Internetu. 

  ![Grupy NSG z tagami magazynu dla regionu](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

> [!WARNING]
> Tagi magazynu są dostępne tylko w określonych regionach i są w wersji zapoznawczej. Lista regionów, można znaleźć [usługi tagi dla magazynu](../virtual-network/security-overview.md#service-tags)

### <a name="using-an-http-proxy-for-vm-backups"></a>Za pomocą serwera proxy HTTP dla kopii zapasowych maszyn wirtualnych
Podczas tworzenia kopii zapasowej maszyny Wirtualnej, tworzenia kopii zapasowej rozszerzenia na maszynie Wirtualnej wysyła migawki polecenia zarządzania do magazynu Azure przy użyciu interfejsu API protokołu HTTPS. Kierować ruchem zapasowy numer wewnętrzny, za pośrednictwem serwera proxy HTTP, ponieważ jest jedynym składnikiem skonfigurowany do uzyskiwania dostępu do publicznej sieci Internet.

> [!NOTE]
> Nie ma żadnych zalecenia dotyczące oprogramowania serwera proxy, który ma zostać użyty. Upewnij się, że możesz wybrać serwer proxy, który jest zgodny z poniższych czynności konfiguracyjne.
>
>

Obraz przykład poniżej przedstawiono kroki trzech konfiguracji koniecznych do używania serwera proxy HTTP:

* Maszyna wirtualna aplikacji kieruje cały ruch HTTP do publicznego Internetu za pośrednictwem serwera Proxy maszyny Wirtualnej.
* Maszyna wirtualna serwera proxy zezwala na ruch przychodzący z maszyn wirtualnych w sieci wirtualnej.
* Grupy zabezpieczeń sieci (NSG) o nazwie NF blokady musi zabezpieczeń reguły zezwalanie Internet ruch wychodzący z maszyny Wirtualnej serwera Proxy.

Aby użyć serwera proxy HTTP do komunikacji z Internetem, wykonaj następujące kroki:

#### <a name="step-1-configure-outgoing-network-connections"></a>Krok 1. Konfigurowanie połączeń wychodzących sieci
###### <a name="for-windows-machines"></a>W przypadku komputerów z systemem Windows
Będzie to ustawienia konfiguracji serwera proxy dla lokalnego konta systemowego.

1. Pobierz [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Uruchom następujące polecenia z podwyższonym poziomem uprawnień wiersza

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Otworzy się okno programu internet explorer.
3. Przejdź do pozycji Narzędzia -> Opcje internetowe -> połączenia -> Ustawienia sieci LAN.
4. Sprawdź ustawienia serwera proxy dla konta System. Ustaw Proxy adresu IP i portu.
5. Zamknij program Internet Explorer.

Zostanie utworzenie konfiguracji serwera proxy dla komputera i będą używane dla każdego wychodzącego ruchu HTTP/HTTPS.

Jeśli skonfigurowano serwer proxy dla bieżącego konta użytkownika (nie lokalnego konta systemowego), użyj następującego skryptu, aby zastosować je do SYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Jeśli zauważysz "(407) uwierzytelniania serwera Proxy wymagane" w dzienniku serwera proxy, sprawdź, czy uwierzytelnianie jest poprawnie skonfigurowana.
>
>

###### <a name="for-linux-machines"></a>Dla maszyn z systemem Linux
Dodaj następujący wiersz do ```/etc/environment``` pliku:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Dodaj następujące wiersze do ```/etc/waagent.conf``` pliku:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>Krok 2. Zezwalaj na połączenia przychodzące na serwerze proxy:
1. Na serwerze proxy otwórz Zaporę systemu Windows. Najprostszym sposobem dostęp zapory jest do wyszukiwania Zapora systemu Windows z zabezpieczeniami zaawansowanymi.
2. W oknie dialogowym zapory systemu Windows, kliknij prawym przyciskiem myszy **reguły ruchu przychodzącego** i kliknij przycisk **nową regułę...** .
3. W **Kreatora nowej reguły przychodzącej**, wybierz **niestandardowy** opcja dla **typ reguły** i kliknij przycisk **dalej**.
4. Na stronie, aby wybrać **Program**, wybierz **wszystkie programy** i kliknij przycisk **dalej**.
5. Na **protokoły i porty** , wprowadź następujące informacje i kliknij przycisk **dalej**:

   * Aby uzyskać *protokół typu* wybierz *TCP*
   * dla *portów lokalnych* wybierz *określonych portów*, w poniższym polu Określ ```<Proxy Port>``` który został skonfigurowany.
   * Aby uzyskać *port zdalny* wybierz *wszystkie porty*

     W pozostałej części kreatora kliknij przycisk aż do zakończenia i nadaj nazwę tej reguły.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>Krok 3. Dodaj regułę wyjątku do grupy NSG:
W wierszu polecenia programu PowerShell systemu Azure wprowadź następujące polecenie:

Polecenie dodaje wyjątek do grupy NSG. Ten wyjątek zezwala na ruch TCP z dowolnego portu na 10.0.0.5 do dowolnego adresu internetowego na porcie 80 (HTTP) lub 443 (HTTPS). Jeśli potrzebujesz określonego portu w publicznej sieci Internet, należy dodać tego portu do ```-DestinationPortRange``` również.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```


*Kroki przy użyciu określonej nazwy i wartości w tym przykładzie. Użyj nazwy i wartości dla wdrożenia, wprowadzając, lub wycinanie i wklejanie informacji w kodzie.*

Teraz, gdy wiesz, że masz połączenie z siecią, można przystąpić do tworzenia kopii zapasowych maszyny Wirtualnej. Zobacz [kopię zapasową wdrożonych przez Menedżera zasobów maszyn wirtualnych](backup-azure-arm-vms.md).

## <a name="questions"></a>Pytania?
Jeśli masz pytania lub jeśli brakuje Ci jakiejś funkcji, [prześlij nam opinię](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Następne kroki
Teraz, kiedy przygotowania środowiska do tworzenia kopii zapasowej maszyny Wirtualnej, następnym krokiem logicznej jest utworzenie kopii zapasowej. Planowania artykuł zawiera bardziej szczegółowe informacje o tworzeniu kopii zapasowych maszyn wirtualnych.

* [Tworzenie kopii zapasowych maszyn wirtualnych](backup-azure-arm-vms.md)
* [Zaplanuj infrastrukturę kopii zapasowej maszyny Wirtualnej](backup-azure-vms-introduction.md)
* [Zarządzanie kopii zapasowych maszyn wirtualnych](backup-azure-manage-vms.md)
