---
title: "Pierwsze spojrzenie: ochrona maszyn wirtualnych platformy Azure przy użyciu magazynu usługi Recovery Services | Microsoft Docs"
description: "Ochrona maszyn wirtualnych platformy Azure przy użyciu magazynu usługi Recovery Services. Tworzenie kopii zapasowych maszyn wirtualnych wdrożonych przez usługę Resource Manager, maszyn wirtualnych wdrożonych klasycznie i maszyn wirtualnych usługi Premium Storage pozwala na ochronę danych. Utwórz i zarejestruj magazyn usługi Recovery Services. Rejestruj maszyny wirtualne, twórz zasady i chroń maszyny wirtualne na platformie Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: 
keyword: backups; vm backup
ms.assetid: 45e773d6-c91f-4501-8876-ae57db517cd1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/10/2016
ms.author: markgal; jimpark
translationtype: Human Translation
ms.sourcegitcommit: d18cd2c117ced64e407e87bcc96da38b0351a341
ms.openlocfilehash: 0b3409074e0b4929fdf1f5a6b915e3814facedf6


---
# <a name="first-look-protect-azure-vms-with-a-recovery-services-vault"></a>Pierwsze spojrzenie: ochrona maszyn wirtualnych platformy Azure przy użyciu magazynu usługi Recovery Services
> [!div class="op_single_selector"]
> * [Ochrona maszyn wirtualnych przy użyciu magazynu usługi Recovery Services](backup-azure-vms-first-look-arm.md)
> * [Ochrona maszyn wirtualnych przy użyciu magazynu kopii zapasowych](backup-azure-vms-first-look.md)
>
>

Ten samouczek zawiera kroki prowadzące do utworzenia magazynu usługi Recovery Services oraz utworzenia kopii zapasowej maszyny wirtualnej Azure. Magazyny usługi Recovery Services chronią:

* Maszyny wirtualne wdrożone przez program Resource Manager platformy Azure
* Klasyczne maszyny wirtualne
* Maszyny wirtualne magazynu w warstwie Standard
* Maszyny wirtualne magazynu w warstwie Premium
* Maszyny wirtualne szyfrowane przy użyciu usługi Azure Disk Encryption (BEK i KEK)

Dodatkowe informacje na temat ochrony maszyn wirtualnych usługi Premium Storage można znaleźć w sekcji [Tworzenie kopii zapasowej i przywracanie maszyn wirtualnych usługi Premium Storage](backup-introduction-to-azure-backup.md#using-premium-storage-vms-with-azure-backup).

> [!NOTE]
> W tym samouczku zakładamy, że masz już maszynę wirtualną w subskrypcji platformy Azure i zostały podjęte działania umożliwiające usłudze tworzenia kopii zapasowej dostęp do maszyny wirtualnej.
>
>

[!INCLUDE [learn-about-Azure-Backup-deployment-models](../../includes/backup-deployment-models.md)]

W oparciu o liczbę maszyn wirtualnych do objęcia ochroną możesz rozpocząć w różnych punktach— jeśli chcesz tworzyć kopie zapasowe wielu maszyn wirtualnych w ramach jednej operacji, przejdź do magazynu usług Recovery Services i uruchom tworzenie kopii zapasowej na pulpicie nawigacyjnym magazynu. Jeśli masz pojedynczą maszynę wirtualną, dla której chcesz utworzyć kopię zapasową, możesz ją utworzyć bezpośrednio z bloku zarządzania maszyną wirtualną.

## <a name="configure-backup-from-vm-management-blade"></a>Tworzenie kopii zapasowej z bloku zarządzania maszyną wirtualną
1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. W menu Centrum kliknij opcję **Więcej usług** i na liście zasobów wpisz ciąg **Maszyny wirtualne**.  Zostanie wyświetlona lista maszyn wirtualnych. Z listy maszyn wirtualnych wybierz maszynę wirtualną, dla której chcesz utworzyć kopię zapasową. Spowoduje to otwarcie bloku zarządzania maszyną wirtualną.
 ![Blok zarządzania maszyną wirtualną](./media/backup-azure-vms-first-look-arm/vm-management-blade.png)

3. W bloku zarządzania maszyną wirtualną kliknij pozycję „Kopia zapasowa” po lewej stronie w obszarze Ustawienia.
![Opcja Kopia zapasowa w bloku zarządzania maszyny wirtualnej](./media/backup-azure-vms-first-look-arm/backup-option-vm-management-blade.png)

4. Spowoduje to otwarcie bloku włączenia kopii zapasowej. W tym bloku są oczekiwane dwie wartości wejściowe. Są to: magazyn usług Recovery Services — zasób usługi Azure Backup używany do przechowywania kopii zapasowych maszyn wirtualnych — oraz zasady kopii zapasowych — zasady usługi Backup określające harmonogram tworzenia kopii zapasowych i czas ich przechowywania. Ten blok zawiera opcje domyślne. Można je dostosować zgodnie z wymaganiami kopii zapasowej.

  ![Kreator włączania kopii zapasowej](./media/backup-azure-vms-first-look-arm/vm-blade-enable-backup.png)

5. W przypadku magazynu usług Recovery Services można istniejący magazyn lub utworzyć nowy. Nowo tworzony magazyn powstaje w tej samej grupie zasobów co maszyna wirtualna, a jego lokalizacja jest taka sama jak lokalizacja maszyny wirtualnej. Jeśli chcesz utworzyć magazyn usług Recovery Services z różnymi wartościami, [utwórz magazyn usług Recovery Services](backup-azure-vms-first-look-arm.md#create-a-recovery-services-vault-for-a-vm) przed kliknięciem opcji Kopia zapasowa w kroku 3, a następnie wybierz to ustawienie w tym bloku.

6. W bloku zasad tworzenia kopii zapasowej wybierz zasady tworzenia kopii zapasowej, które mają zostać zastosowane do magazynu, i kliknij przycisk **OK**.
    ![Wybieranie zasad kopii zapasowych](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    Szczegóły domyślnych zasad znajdują się w obszarze szczegółów. Aby utworzyć zasady, wybierz opcję **Utwórz nowy** z menu rozwijanego. Menu rozwijane zawiera także opcję przełączenia godziny wykonywania migawki. Instrukcje dotyczące definiowania zasad tworzenia kopii zapasowych można znaleźć w sekcji [Definiowanie zasad tworzenia kopii zapasowej](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Po kliknięciu przycisku **OK** zasady kopii zapasowych zostaną skojarzone z maszyną wirtualną.

7. Kliknij pozycję „Włącz wykonywanie kopii zapasowej”, aby skonfigurować kopię zapasową na maszynie wirtualnej. Spowoduje to wyzwolenie wdrożenia.
![Przycisk Włącz wykonywanie kopii zapasowej](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-button.png)

8. Postęp konfiguracji można śledzić za pośrednictwem powiadomień.
![Powiadomienia dotyczące włączania kopii zapasowej](./media/backup-azure-vms-first-look-arm/vm-management-blade-enable-backup-notification.png)

9. Po ukończeniu wdrażania na potrzeby konfigurowania kopii zapasowej kliknięcie pozycji „Kopia zapasowa” w bloku zarządzania maszyną wirtualną spowoduje przejście do bloku Element kopii zapasowej odpowiadającemu kopii zapasowej maszyny wirtualnej.
![Widok elementu kopii zapasowej maszyny wirtualnej](./media/backup-azure-vms-first-look-arm/backup-item-view.png)

## <a name="configure-backup-from-recovery-services-vault-view"></a>Konfigurowanie kopii zapasowej w widoku magazynu usługi Recovery Services
Poniżej ogólnie przedstawiono kroki, które zostaną wykonane.  

1. Utwórz magazyn usługi Recovery Services dla maszyny wirtualnej.
2. Za pomocą portalu Azure wybierz Scenariusz, ustaw Zasady i zidentyfikuj elementy do ochrony.
3. Uruchom proces tworzenia początkowej kopii zapasowej.

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Tworzenie magazynu usługi Recovery Services dla maszyny wirtualnej
Magazyn usługi Recovery Services jest jednostką, która przechowuje wszystkie utworzone dotychczas kopie zapasowe i punkty odzyskiwania. Magazyn usługi Recovery Services zawiera także zasady tworzenia kopii zapasowej stosowane do chronionych maszyn wirtualnych.

> [!NOTE]
> Wykonywanie kopii zapasowych maszyn wirtualnych jest procesem lokalnym. Nie można utworzyć kopii zapasowych maszyn wirtualnych z jednej lokalizacji w magazynie usługi Recovery Services w innej lokalizacji. Z tego powodu w każdej lokalizacji platformy Azure zawierającej maszyny wirtualne, których kopie zapasowe mają być wykonywane, musi istnieć co najmniej jeden magazyn usługi Recovery Services.
>
>

Aby utworzyć magazyn usługi Recovery Services:

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
2. W menu Centrum kliknij opcję **Przeglądaj** i na liście zasobów wpisz ciąg **Usługi odzyskiwania**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Kliknij opcję **Magazyn Usług odzyskiwania**.

    ![Tworzenie magazynu Usług odzyskiwania — krok 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

    Zostanie wyświetlona lista magazynów usługi Recovery Services.
3. W menu **Magazyny Usług odzyskiwania** kliknij pozycję **Dodaj**.

    ![Tworzenie magazynu Usług odzyskiwania — krok 2](./media/backup-azure-vms-first-look-arm/rs-vault-menu.png)

    Zostanie otwarty blok magazynu Usług odzyskiwania i pojawi się monit o podanie wartości w polach **Nazwa**, **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**.

    ![Tworzenie magazynu Usług odzyskiwania — krok 5](./media/backup-azure-vms-first-look-arm/rs-vault-attributes.png)
4. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Nazwa musi być unikalna w tej subskrypcji platformy Azure. Wpisz nazwę o długości od 2 do 50 znaków. Musi ona rozpoczynać się od litery i może zawierać tylko litery, cyfry i łączniki.
5. Kliknij pozycję **Subskrypcja**, aby wyświetlić listę dostępnych subskrypcji. Jeśli nie masz pewności, której subskrypcji użyć, wybierz domyślną (lub sugerowaną). Większa liczba opcji do wyboru jest dostępna tylko w przypadku, gdy konto organizacji jest skojarzone z wieloma subskrypcjami platformy Azure.
6. Kliknij pozycję **Grupa zasobów**, aby wyświetlić listę dostępnych grup zasobów, lub kliknij pozycję **Nowa**, aby utworzyć grupę zasobów. Pełne informacje na temat grup zasobów można znaleźć w temacie [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md)
7. Kliknij pozycję **Lokalizacja**, aby wybrać region geograficzny magazynu. Magazyn **musi** znajdować się w tym samym regionie, w którym znajdują się maszyny wirtualne, które mają być chronione.

   > [!IMPORTANT]
   > Jeśli nie wiesz, w jakiej lokalizacji znajdują się Twoje maszyny wirtualne, zamknij okno dialogowe tworzenia magazynu, a następnie przejdź do listy maszyn wirtualnych w portalu. Jeśli Twoje maszyny wirtualne znajdują się w wielu regionach, utwórz magazyn usługi Recovery Services w każdym regionie. Przed przejściem do następnej lokalizacji utwórz magazyn w pierwszej lokalizacji. Nie ma potrzeby określania kont magazynu do przechowywania kopii zapasowych danych — magazyn usługi Recovery Services i usługa Azure Backup określają je automatycznie.
   >
   >
8. Kliknij przycisk **Utwórz**. Utworzenie magazynu usługi Recovery Services może zająć trochę czasu. Monitoruj powiadomienia o stanie wyświetlane w prawej górnej części obszaru portalu. Po utworzeniu magazynu pojawi się on na liście magazynów usługi Recovery Services.

    ![Lista magazynów kopii zapasowych](./media/backup-azure-vms-first-look-arm/rs-list-of-vaults.png)

Magazyn został już utworzony, więc teraz zajmiemy się skonfigurowaniem jego replikacji.

### <a name="set-storage-replication"></a>Konfigurowanie replikacji magazynu
Dla opcji replikacji magazynu można wybrać magazynowanie nadmiarowe geograficznie lub lokalnie. Domyślnie magazyn jest nadmiarowy geograficznie. Pozostaw tę opcję ustawioną na magazyn geograficznie nadmiarowy, jeśli jest to Twoja podstawowa kopia zapasowa. Wybierz magazyn lokalnie nadmiarowy, jeśli chcesz skorzystać z tańszej, ale mniej trwałej opcji. Więcej informacji o opcjach magazynu [geograficznie nadmiarowego](../storage/storage-redundancy.md#geo-redundant-storage) i [lokalnie nadmiarowego](../storage/storage-redundancy.md#locally-redundant-storage) można znaleźć w temacie [Azure Storage replication overview](../storage/storage-redundancy.md) (Omówienie replikacji usługi Azure Storage).

Aby edytować ustawienia replikacji magazynu:

1. Wybierz swój magazyn, aby otworzyć jego pulpit nawigacyjny i blok Ustawienia. Jeśli blok **Ustawienia** nie zostanie otwarty, kliknij przycisk **Wszystkie ustawienia** na pulpicie nawigacyjnym magazynu.
2. W bloku **Ustawienia** kliknij opcję **Infrastruktura kopii zapasowej** > **Konfiguracja kopii zapasowej**, aby otworzyć blok **Konfiguracja kopii zapasowej**. W bloku **Konfiguracja kopii zapasowej** wybierz opcję replikacji swojego magazynu.

    ![Lista magazynów kopii zapasowych](./media/backup-azure-vms-first-look-arm/choose-storage-configuration-rs-vault.png)

    Po wybraniu opcji magazynu dla swojego magazynu możesz skojarzyć z nim maszynę wirtualną. Aby rozpocząć kojarzenie, należy odnaleźć i zarejestrować maszyny wirtualne Azure.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Wybieranie celu tworzenia kopii zapasowej, ustawianie zasad i definiowanie elementów podlegających ochronie
Przed zarejestrowaniem maszyny wirtualnej w magazynie uruchom proces wykrywania, aby mieć pewność, że zostały znalezione wszystkie nowe maszyny wirtualne dodane do subskrypcji. Proces wyszukuje na platformie Azure listę maszyn wirtualnych w ramach subskrypcji wraz z dodatkowymi informacjami, takimi jak nazwa usługi w chmurze i region. W portalu Azure scenariusz odnosi się do elementów, które mają zostać umieszczone w magazynie usług odzyskiwania. Zasady są harmonogramem określającym częstotliwość i czas tworzenia punktów odzyskiwania. Zasady zawierają także zakres przechowywania dla punktów odzyskiwania.

1. Jeśli Twój magazyn usługi Recovery Services jest już otwarty, przejdź do kroku 2. Jeśli Twój magazyn usługi Recovery Services nie jest otwarty, ale masz otwartą witrynę Azure Portal, w menu Centrum kliknij opcję **Przeglądaj**.

   * Na liście zasobów wpisz **Usługi odzyskiwania**.
   * Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Po wyświetleniu pozycji **Magazyny Usług odzyskiwania** kliknij ją.

     ![Tworzenie magazynu Usług odzyskiwania — krok 1](./media/backup-azure-vms-first-look-arm/browse-to-rs-vaults.png) <br/>

     Zostanie wyświetlona lista magazynów usługi Recovery Services.
   * Wybierz magazyn z listy magazynów usługi Recovery Services.

     Zostanie otwarty pulpit nawigacyjny wybranego magazynu.

     ![Otwarcie bloku magazynu](./media/backup-azure-vms-first-look-arm/vault-settings.png)
2. W menu pulpitu nawigacyjnego magazynu kliknij opcję **Kopia zapasowa**, aby otworzyć blok Kopia zapasowa.

    ![Otwarcie bloku Kopia zapasowa](./media/backup-azure-vms-first-look-arm/backup-button.png)

    Po otwarciu bloku usługa Backup wyszukuje wszystkie nowe maszyny wirtualne w subskrypcji.

    ![Odnajdywanie maszyn wirtualnych](./media/backup-azure-vms-first-look-arm/discovering-new-vms.png)
3. W bloku Kopia zapasowa kliknij opcję **Cel kopii zapasowej**, aby otworzyć blok Cel kopii zapasowej.

    ![Otwarcie bloku scenariusza](./media/backup-azure-vms-first-look-arm/select-backup-goal-one.png)
4. W bloku Cel kopii zapasowej ustaw opcję **Gdzie jest uruchomione Twoje obciążenie** na Azure oraz opcję **Co ma zawierać kopia zapasowa** na Maszyna wirtualna, a następnie kliknij przycisk **OK**.

    Blok Cel kopii zapasowej zostanie zamknięty, po czym zostanie otwarty blok zasad tworzenia kopii zapasowej.

    ![Otwarcie bloku scenariusza](./media/backup-azure-vms-first-look-arm/select-backup-goal-two.png)
5. W bloku zasad tworzenia kopii zapasowej wybierz zasady tworzenia kopii zapasowej, które mają zostać zastosowane do magazynu, i kliknij przycisk **OK**.

    ![Wybór zasad tworzenia kopii zapasowej](./media/backup-azure-vms-first-look-arm/setting-rs-backup-policy-new.png)

    Szczegóły domyślnych zasad znajdują się w obszarze szczegółów. Aby utworzyć zasady, wybierz opcję **Utwórz nowy** z menu rozwijanego. Menu rozwijane zawiera także opcję przełączenia godziny wykonywania migawki na 19: 00. Instrukcje dotyczące definiowania zasad tworzenia kopii zapasowych można znaleźć w sekcji [Definiowanie zasad tworzenia kopii zapasowej](backup-azure-vms-first-look-arm.md#defining-a-backup-policy). Po kliknięciu przycisku **OK** zasady tworzenia kopii zapasowej zostaną skojarzone z magazynem.

    Następnie wybierz maszyny wirtualne, które mają zostać skojarzone z magazynem.
6. Wybierz maszyny wirtualne, które mają zostać skojarzone z określonymi zasadami, a następnie kliknij przycisk **Wybierz**.

    ![Wybór obciążenia](./media/backup-azure-vms-first-look-arm/select-vms-to-backup-new.png)

    Jeśli nie widzisz wymaganej maszyny wirtualnej, sprawdź, czy istnieje ona w tej samej lokalizacji platformy Azure, co lokalizacja magazynu Usług odzyskiwania.
7. Skoro zdefiniowano wszystkie ustawienia magazynu, w bloku Kopia zapasowa kliknij przycisk **Włącz kopię zapasową** u dołu strony. Ta czynność spowoduje wdrożenie zasad w odniesieniu do magazynu i maszyn wirtualnych.

    ![Włączenie kopii zapasowej](./media/backup-azure-vms-first-look-arm/enable-backup-settings-new.png)

## <a name="initial-backup"></a>Początkowa kopia zapasowa
Wdrożenie zasad tworzenia kopii zapasowej na maszynie wirtualnej nie oznacza wykonania kopii zapasowej danych. Domyślnie pierwszą zaplanowaną kopią zapasową (zgodnie z definicją w zasadach tworzenia kopii zapasowej) jest początkowa kopia zapasowa. Do czasu uruchomienia procesu tworzenia początkowej kopii zapasowej w polu Stan ostatniej kopii zapasowej w bloku **Zadania tworzenia kopii zapasowej** wyświetlany jest komunikat **Ostrzeżenie (oczekiwanie na początkową kopię zapasową)**.

![Oczekiwanie na kopię zapasową](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Jeśli tworzenie początkowej kopii zapasowej nie ma rozpocząć się wkrótce, zalecane jest wykonanie akcji **Wykonaj kopię zapasową teraz**.

Aby uruchomić akcję **Wykonaj kopię zapasową teraz**:

1. Na pulpicie nawigacyjnym magazynu na kafelku **Kopia zapasowa** kliknij przycisk **Maszyny wirtualne Azure** <br/>
    ![Ikona Ustawienia](./media/backup-azure-vms-first-look-arm/rs-vault-in-dashboard-backup-vms.png)

    Zostanie otwarty blok **Elementy kopii zapasowej**.
2. W bloku **Elementy kopii zapasowej** kliknij prawym przyciskiem myszy magazyn, którego kopię zapasową chcesz utworzyć, a następnie kliknij przycisk **Wykonaj kopię zapasową teraz**.

    ![Ikona Ustawienia](./media/backup-azure-vms-first-look-arm/back-up-now.png)

    Zostanie wyzwolone zadanie tworzenia kopii zapasowej. <br/>

    ![Zadanie tworzenia kopii zapasowej zostało wyzwolone](./media/backup-azure-vms-first-look-arm/backup-triggered.png)
3. Aby zobaczyć, czy tworzenie początkowej kopii zapasowej zostało ukończone, na pulpicie nawigacyjnym magazynu na kafelku **Zadania tworzenia kopii zapasowej** kliknij przycisk **Maszyny wirtualne Azure**.

    ![Kafelek Zadania tworzenia kopii zapasowej](./media/backup-azure-vms-first-look-arm/open-backup-jobs.png)

    Zostanie otwarty blok Zadania tworzenia kopii zapasowej.
4. W bloku Zadania tworzenia kopii zapasowej można sprawdzić stan wszystkich zadań.

    ![Kafelek Zadania tworzenia kopii zapasowej](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view.png)

   > [!NOTE]
   > W ramach operacji tworzenia kopii zapasowej usługa Azure Backup wydaje polecenie rozszerzeniu kopii zapasowej w każdej maszynie wirtualnej powodujące opróżnienie wszystkich zapisów i wykonanie spójnej migawki.
   >
   >

    Po zakończeniu zadania tworzenia kopii zapasowej stan przyjmuje wartość *Ukończono*.

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>Instalowanie agenta maszyny wirtualnej na maszynie wirtualnej
Te informacje są dostarczane w przypadku, gdy są wymagane. Aby rozszerzenie usługi Backup mogło działać, na maszynie wirtualnej Azure musi być zainstalowany agent maszyny wirtualnej. Jeśli jednak maszyna wirtualna została utworzona z poziomu galerii Azure, agent maszyny wirtualnej znajduje się już na maszynie wirtualnej. Maszyny wirtualne migrowane z lokalnych centrów danych nie będą miały zainstalowanego agenta maszyny wirtualnej. W takim przypadku konieczne jest zainstalowanie agenta maszyny wirtualnej. Jeśli masz problemy z tworzeniem kopii zapasowej maszyny wirtualnej Azure, sprawdź, czy agent maszyny wirtualnej Azure został poprawnie zainstalowany na maszynie wirtualnej (patrz w tabeli poniżej). Jeśli tworzysz niestandardową maszynę wirtualną, przed jej udostępnieniem [upewnij się, że pole wyboru **Zainstaluj agenta maszyny wirtualnej** jest zaznaczone](../virtual-machines/virtual-machines-windows-classic-agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Dowiedz się więcej o [agencie maszyny wirtualnej](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) i [sposobie jego instalowania](../virtual-machines/virtual-machines-windows-classic-manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Poniższa tabela zawiera dodatkowe informacje na temat agenta maszyny wirtualnej dla maszyn wirtualnych systemów Windows i Linux.

| **Operacja** | **Windows** | **Linux** |
| --- | --- | --- |
| Instalowanie agenta maszyny wirtualnej |<li>Pobierz i zainstaluj [plik MSI agenta](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Do ukończenia procesu instalacji niezbędne są uprawnienia administratora. <li>[Zaktualizuj właściwości maszyny wirtualnej](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby wskazać, że agent jest zainstalowany. |<li> Zainstaluj najnowszą wersję [agenta systemu Linux](https://github.com/Azure/WALinuxAgent) z usługi GitHub. Do ukończenia procesu instalacji niezbędne są uprawnienia administratora. <li> [Zaktualizuj właściwości maszyny wirtualnej](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx), aby wskazać, że agent jest zainstalowany. |
| Aktualizowanie agenta maszyny wirtualnej |Aktualizowanie agenta maszyny wirtualnej jest równie proste, jak ponowne zainstalowanie [plików binarnych agenta maszyny wirtualnej](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Upewnij się, że żadna operacja tworzenia kopii zapasowej nie jest uruchomiona podczas aktualizowania agenta maszyny wirtualnej. |Postępuj zgodnie z instrukcjami dotyczącymi [aktualizowania agenta maszyny wirtualnej systemu Linux ](../virtual-machines/virtual-machines-linux-update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br>Upewnij się, że podczas aktualizowania agenta maszyny wirtualnej żadna operacja tworzenia kopii zapasowej nie jest uruchomiona. |
| Sprawdzanie poprawności instalacji agenta maszyny wirtualnej |<li>Przejdź do folderu *C:\WindowsAzure\Packages* w maszynie wirtualnej Azure. <li>Powinien znajdować się w nim plik WaAppAgent.exe.<li> Kliknij plik prawym przyciskiem myszy, przejdź do opcji **Właściwości**, a następnie wybierz kartę **Szczegóły**. W polu Wersja produktu powinna znajdować się wartość 2.6.1198.718 lub wyższa. |Nie dotyczy |

### <a name="backup-extension"></a>Rozszerzenie kopii zapasowej
Po zainstalowaniu agenta na maszynie wirtualnej usługa Azure Backup instaluje rozszerzenie kopii zapasowej do agenta maszyny wirtualnej. Usługa Azure Backup płynnie uaktualnia rozszerzenia kopii zapasowej i wprowadza do nich poprawki bez dodatkowej interwencji użytkownika.

Rozszerzenie kopii zapasowej jest instalowane przez usługę Backup niezależnie od tego, czy maszyna wirtualna jest uruchomiona. Uruchomiona maszyna wirtualna zapewnia największe prawdopodobieństwo uzyskania punktu odzyskiwania spójnego z aplikacją. Jednak usługa Azure Backup będzie kontynuować tworzenie kopii zapasowej maszyny wirtualnej nawet w sytuacji, gdy jest ona wyłączona i nie można zainstalować rozszerzenia. To tzw. „maszyna wirtualna offline”. W takim przypadku punkt odzyskiwania będzie *awaryjnie spójny*.

## <a name="troubleshooting-information"></a>Informacje dotyczące rozwiązywania problemów
W przypadku wystąpienia problemów z wykonaniem którychkolwiek zadań z tego artykułu zapoznaj się z tematem [Troubleshooting guidance](backup-azure-vms-troubleshoot.md) (Wskazówki dotyczące rozwiązywania problemów).

## <a name="pricing"></a>Cennik
Za kopię zapasową maszyny wirtualnej Azure zostanie naliczona opłata oparta na modelu wystąpień chronionych. Dowiedz się więcej na temat [cennika kopii zapasowych](https://azure.microsoft.com/pricing/details/backup/)

## <a name="questions"></a>Pytania?
Jeśli masz pytania lub jeśli brakuje Ci jakiejś funkcji, [prześlij nam opinię](http://aka.ms/azurebackup_feedback).



<!--HONumber=Dec16_HO2-->


