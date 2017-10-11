---
title: "Utwórz kopię zapasową stanu systemu Windows Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się tworzyć kopie zapasowe stanu systemu Windows Server i/lub Windows komputerów Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "jak tworzyć kopie zapasowe; tworzenie kopii zapasowych; tworzenie kopii zapasowych plików i folderów"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: 6fbd96935f444d8b0c6d068ebd0d28e612f19816
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>Wykonaj kopię zapasową stanu systemu Windows podczas wdrażania usługi Resource Manager
W tym artykule opisano sposób wykonywania kopii zapasowej stanu systemu Windows Server na platformie Azure. Ten samouczek zawiera podstawowe informacje.

Jeśli chcesz dowiedzieć się więcej o usłudze Azure Backup, przeczytaj to [omówienie](backup-introduction-to-azure-backup.md).

Jeśli nie masz subskrypcji platformy Azure, utwórz [bezpłatne konto](https://azure.microsoft.com/free/) umożliwiające dostęp do dowolnej usługi Azure.

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu usługi Recovery Services
Aby utworzyć kopię zapasową plików i folderów, należy utworzyć magazyn usługi Recovery Services w regionie, w którym chcesz przechowywać dane. Należy również określić sposób replikowania magazynu.

### <a name="to-create-a-recovery-services-vault"></a>Aby utworzyć magazyn usługi Recovery Services
1. Jeśli nie zostało to wcześniej zrobione, zaloguj się witryny [Azure Portal](https://portal.azure.com/) przy użyciu subskrypcji platformy Azure.
2. W menu Centrum kliknij pozycję **Więcej usług**, na liście zasobów wpisz łańcuch **Recovery Services**, a następnie kliknij pozycję **Magazyny usługi Recovery Services**.

    ![Tworzenie magazynu usługi Recovery Services — krok 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Jeśli w ramach subskrypcji istnieją magazyny usług odzyskiwania, zostaną one wyświetlone.
3. W menu **Magazyny usługi Recovery Services** kliknij pozycję **Dodaj**.

    ![Tworzenie magazynu Usług odzyskiwania — krok 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Zostanie otwarty blok magazynu Usług odzyskiwania i pojawi się monit o podanie wartości w polach **Nazwa**, **Subskrypcja**, **Grupa zasobów** i **Lokalizacja**.

    ![Tworzenie magazynu usługi Recovery Services — krok 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. W polu **Nazwa** wprowadź przyjazną nazwę identyfikującą magazyn. Nazwa musi być unikalna w tej subskrypcji platformy Azure. Wpisz nazwę o długości od 2 do 50 znaków. Musi ona rozpoczynać się od litery i może zawierać tylko litery, cyfry i łączniki.

5. W sekcji **Subskrypcja** użyj menu rozwijanego, aby wybrać subskrypcję platformy Azure. Jeśli używasz tylko jednej subskrypcji, zostanie ona wyświetlona i możesz przejść do następnego kroku. Jeśli nie masz pewności, której subskrypcji użyć, wybierz domyślną (lub sugerowaną). Większa liczba opcji do wyboru jest dostępna tylko w przypadku, gdy konto organizacji jest skojarzone z wieloma subskrypcjami platformy Azure.

6. W sekcji **Grupa zasobów**:

    * wybierz pozycję **Utwórz nową**, jeśli chcesz utworzyć grupę zasobów.
    Lub
    * wybierz pozycję **Użyj istniejącej** i kliknij menu rozwijane, aby wyświetlić listę dostępnych grup zasobów.

  Pełne informacje na temat grup zasobów można znaleźć w temacie [Omówienie usługi Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).

7. Kliknij pozycję **Lokalizacja**, aby wybrać region geograficzny magazynu. Ten wybór określa region geograficzny, do którego wysyłane są dane kopii zapasowej.

8. W dolnej części bloku magazynu usług Recovery Services kliknij pozycję **Utwórz**.

    Utworzenie magazynu usług Recovery Services może potrwać kilka minut. Monitoruj powiadomienia o stanie wyświetlane w prawej górnej części obszaru portalu. Po utworzeniu magazynu pojawi się on na liście magazynów Usług odzyskiwania. Jeśli po kilku minutach nie widzisz swojego magazynu, kliknij pozycję **Odśwież**.

    ![Klikanie pozycji Odśwież](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Po wyświetleniu magazynu na liście magazynów usług Recovery Services możesz rozpocząć ustawianie nadmiarowości przechowywania.

### <a name="set-storage-redundancy-for-the-vault"></a>Ustawianie nadmiarowości przechowywania dla magazynu
Po utworzeniu magazynu usług Recovery Services upewnij się, że nadmiarowość magazynu została skonfigurowana w preferowany sposób.

1. W bloku **Magazyny usług Recovery Services** kliknij nowy magazyn.

    ![Wybieranie nowego magazynu z listy magazynów usług Recovery Services](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Wybranie magazynu spowoduje zwężenie bloku **Magazyn usług Recovery Services** oraz otwarcie bloków Ustawienia (*o nazwie magazynu wskazanego w górnej części*) i szczegółów magazynu.

    ![Wyświetlanie konfiguracji przechowywania dla nowego magazynu](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. W bloku ustawień nowego magazynu użyj pionowego suwaka, aby przewinąć w dół do sekcji Zarządzanie, a następnie kliknij pozycję **Infrastruktura zapasowa**.
    Zostanie otwarty blok Infrastruktura zapasowa.
3. W bloku Infrastruktura zapasowa kliknij pozycję **Konfiguracja kopii zapasowej** w celu otwarcia bloku **Konfiguracja kopii zapasowej**.

    ![Ustawianie konfiguracji przechowywania dla nowego magazynu](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Wybierz odpowiednią opcję replikacji dla magazynu.

    ![Opcje konfiguracji usługi Storage](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Domyślnie magazyn jest nadmiarowy geograficznie. Jeśli używasz platformy Azure jako punktu końcowego podstawowego magazynu kopii zapasowych, kontynuuj korzystanie z magazynu **geograficznie nadmiarowego**. Jeśli nie używasz platformy Azure jako punktu końcowego podstawowego magazynu kopii zapasowych, wybierz pozycję **Lokalnie nadmiarowy**, co zmniejszy koszty magazynów platformy Azure. Więcej informacji o opcjach magazynu [geograficznie nadmiarowego](../storage/common/storage-redundancy.md#geo-redundant-storage) i [lokalnie nadmiarowego](../storage/common/storage-redundancy.md#locally-redundant-storage) można znaleźć w tym [omówieniu nadmiarowości magazynu](../storage/common/storage-redundancy.md).

Teraz, po utworzeniu magazynu, należy go skonfigurować do tworzenia kopii zapasowej stanu systemu Windows.

## <a name="configure-the-vault"></a>Konfigurowanie magazynu
1. W bloku Magazyn usługi Recovery Services (dla właśnie utworzonego magazynu) w sekcji Wprowadzenie kliknij pozycję **Kopia zapasowa**, a następnie w bloku **Wprowadzenie do kopii zapasowej** wybierz pozycję **Cel kopii zapasowej**.

    ![Otwieranie bloku celu kopii zapasowej](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    Zostanie otwarty blok **Cel kopii zapasowej**.

    ![Otwieranie bloku celu kopii zapasowej](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. Z menu **Gdzie działa Twoje obciążenie?** wybierz pozycję **Lokalnie**.

    Pozycję **Lokalnie** trzeba wybrać, ponieważ Twój serwer lub komputer z systemem Windows jest maszyną fizyczną spoza platformy Azure.

3. Z **co chcesz utworzyć kopię zapasową?** menu, wybierz opcję **stanu systemu**i kliknij przycisk **OK**.

    ![Konfigurowanie plików i folderów](./media/backup-azure-system-state/backup-goal-system-state.png)

    Po kliknięciu przycisku OK obok pozycji **Cel kopii zapasowej** pojawi się znacznik wyboru i zostanie otwarty blok **Przygotowywanie infrastruktury**.

    ![Cel kopii zapasowej został skonfigurowany, następnym krokiem jest przygotowanie infrastruktury](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. W bloku **Przygotowywanie infrastruktury** kliknij pozycję **Pobierz agenta dla systemu Windows Server lub klienta systemu Windows**.

    ![Przygotowywanie infrastruktury](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Jeśli używasz systemu Windows Server Essential, wybierz opcję pobrania agenta dla systemu Windows Server Essential. Zostanie wyświetlone menu rozwijane z monitem o uruchomienie lub zapisanie pliku MARSAgentInstaller.exe.

    ![Okno dialogowe MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. W menu rozwijanym pobierania kliknij pozycję **Zapisz**.

    Domyślnie plik **MARSagentinstaller.exe** jest zapisywany w folderze Pobrane. Po ukończeniu pobierania pojawi się okno podręczne z pytaniem, czy chcesz uruchomić Instalatora, czy otworzyć folder.

    ![Przygotowywanie infrastruktury](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Nie musisz jeszcze instalować agenta. Agenta możesz zainstalować po pobraniu poświadczeń magazynu.

6. W bloku **Przygotowywanie infrastruktury** kliknij pozycję **Pobierz**.

    ![Pobieranie poświadczeń magazynu](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Poświadczenia magazynu zostaną pobrane do folderu Pobrane. Po zakończeniu pobierania poświadczeń magazynu zobaczysz okno podręczne z pytaniem, czy chcesz otworzyć poświadczenia, czy je zapisać. Kliknij pozycję **Zapisz**. Jeśli przypadkowo klikniesz pozycję **Otwórz**, zaczekaj, aż działanie okna dialogowego, które spróbuje otworzyć poświadczenia magazynu, zakończy się niepowodzeniem. Poświadczeń magazynu nie da się otworzyć. Przejdź do następnego kroku. Poświadczenia magazynu znajdują się w folderze Pobrane.   

    ![Zakończenie pobierania poświadczeń magazynu](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a>Instalowanie i rejestrowanie agenta

> [!NOTE]
> Opcja włączania kopii zapasowych za pośrednictwem witryny Azure Portal jest jeszcze niedostępna. Aby utworzyć kopię zapasową stanu systemu Windows Server, użyj agenta usług odzyskiwania Microsoft Azure.
>

1. Zlokalizuj i kliknij dwukrotnie plik **MARSagentinstaller.exe** w folderze Pobrane (lub innej lokalizacji).

    Instalator wyświetli serię komunikatów w miarę wypakowywania, instalowania i rejestrowania agenta usługi Recovery Services.

    ![Uruchamianie Instalatora agenta usługi Recovery Services — poświadczenia](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Wykonaj kroki kreatora instalacji agenta usługi Microsoft Azure Recovery Services. Aby zakończyć pracę kreatora, wykonaj następujące czynności:

   * Wybierz lokalizację folderu instalacji i folderu pamięci podręcznej.
   * Podaj informacje o serwerze proxy, jeśli korzystasz z niego do łączenia się z Internetem.
   * Podaj swoją nazwę użytkownika i hasło, jeśli korzystasz z uwierzytelnionego serwera proxy.
   * Udostępnianie pobranych poświadczeń magazynu
   * Zapisz hasło szyfrowania w bezpiecznej lokalizacji.

     > [!NOTE]
     > Jeśli utracisz lub zapomnisz hasło, firma Microsoft nie będzie mogła pomóc w odzyskaniu danych kopii zapasowej. Zapisz plik w bezpiecznej lokalizacji. Jest ono wymagane do przywrócenia kopii zapasowej.
     >
     >

Agent jest teraz zainstalowany, a maszyna zarejestrowana w magazynie. Wszystko jest gotowe do skonfigurowania kopii zapasowej i zaplanowania jej tworzenia.

## <a name="back-up-windows-server-system-state-preview"></a>Wykonaj kopię zapasową stanu systemu Windows Server (wersja zapoznawcza)
Początkowa kopia zapasowa obejmuje trzy zadania:

* Włącz kopii zapasowej stanu systemu przy użyciu agenta usługi Kopia zapasowa Azure
* Planowanie tworzenia kopii zapasowej
* Tworzenie kopii zapasowej plików i folderów po raz pierwszy

Aby utworzyć początkową kopię zapasową, użyj agenta usługi Microsoft Azure Recovery Services.

### <a name="to-enable-system-state-backup-using-the-azure-backup-agent"></a>Aby włączyć za pomocą usługi Kopia zapasowa Azure agenta kopii zapasowej stanu systemu

1. W sesji programu PowerShell uruchom następujące polecenie, aby zatrzymać aparat kopia zapasowa Azure.

  ```
  PS C:\> Net stop obengine
  ```

2. Otwórz rejestr systemu Windows.

  ```
  PS C:\> regedit.exe
  ```

3. Dodaj następujący klucz rejestru z określoną wartością DWord.

  | Ścieżka rejestru | Klucz rejestru | Wartość DWord |
  |---------------|--------------|-------------|
  | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | TurnOffSSBFeature | 2 |

4. Uruchom ponownie Aparat kopii zapasowej, wykonując następujące polecenie w wierszu polecenia z pełnymi uprawnieniami.

  ```
  PS C:\> Net start obengine
  ```

### <a name="to-schedule-the-backup-job"></a>Aby zaplanować zadanie tworzenia kopii zapasowej

1. Otwórz agenta usługi Microsoft Azure Recovery Services. Aby go znaleźć, wyszukaj na maszynie łańcuch **Microsoft Azure Backup**.

    ![Uruchamianie agenta usługi Azure Recovery Services](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. W agencie usługi Recovery Services kliknij pozycję **Zaplanuj wykonywanie kopii zapasowej**.

    ![Planowanie tworzenia kopii zapasowej systemu Windows Server](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Na stronie Wprowadzenie Kreatora harmonogramu kopii zapasowej kliknij przycisk **Dalej**.

4. Na stronie Wybieranie elementów do wykonania kopii zapasowej kliknij pozycję **Dodaj elementy**.

5. Wybierz **stanu systemu** , a następnie kliknij przycisk **OK**.

6. Kliknij przycisk **Dalej**.

7. Automatycznie ustawiono harmonogramu kopii zapasowej stanu systemu i przechowywania kopii zapasowej każdą niedzielę o 21:00:00 czasu lokalnego, a okres przechowywania wynosi 60 dni.

   > [!NOTE]
   > Automatycznie skonfigurowano zasad przechowywania i kopii zapasowych stanu systemu. Po utworzeniu kopii zapasowej plików i folderów oprócz stanu systemu Windows Server, należy określić tylko zasady kopii zapasowych pliku z poziomu Kreatora tworzenia kopii zapasowej i przechowywania. 
   >

8. Przejrzyj informacje na stronie Potwierdzenie, a następnie kliknij przycisk **Zakończ**.

9. Po ukończeniu harmonogramu tworzenia kopii zapasowej przez kreatora kliknij przycisk **Zamknij**.

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a>Aby utworzyć kopię zapasową stanu systemu Windows Server po raz pierwszy

1. Upewnij się, że nie ma żadnych oczekujących aktualizacji dla systemu Windows Server, które wymagają ponownego uruchomienia.

2. W agencie Usług odzyskiwania kliknij pozycję **Wykonaj kopię zapasową teraz**, aby zakończyć początkowe umieszczanie za pośrednictwem sieci.

    ![Natychmiastowe tworzenie kopii zapasowej systemu Windows Server](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Na stronie Potwierdzenie przejrzyj ustawienia, które zostaną użyte przez Kreatora natychmiastowego tworzenia kopii zapasowej do utworzenia kopii zapasowej maszyny. Następnie kliknij pozycję **Utwórz kopię zapasową**.

4. Kliknij przycisk **Zamknij**, aby zamknąć kreatora. Jeśli zamkniesz kreatora przed zakończeniem procesu tworzenia kopii zapasowej, kreator będzie nadal działać w tle.

5. Po utworzeniu kopii zapasowej plików i folderów na serwerze, oprócz stanu systemu Windows Server, kreator Utwórz kopię zapasową teraz tylko wykona kopię zapasową plików. Aby wykonać ad hoc stanu systemu wykonaj kopię zapasową, użyj następującego polecenia programu PowerShell:

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  Po zakończeniu tworzenia początkowej kopii zapasowej w konsoli usługi Backup zostanie wyświetlony stan **Ukończono zadanie**.

  ![Początkowa replikacja została zakończona](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a>Często zadawane pytania

Następujące pytania i odpowiedzi zawierają dodatkowe informacje.

### <a name="what-is-the-staging-volume"></a>Co to jest woluminem przemieszczania?

Wielkość przemieszczania stanowi pośredni lokalizacji, gdzie dostępna natywnie, kopia zapasowa systemu Windows Server przygotuje kopii zapasowej stanu systemu. Agent usługi Kopia zapasowa Azure, a następnie kompresuje i szyfruje pośredniego kopia zapasowa i wysyła je za pośrednictwem bezpiecznego protokołu HTTPS na skonfigurowanym magazynu usług odzyskiwania. **Stanowczo zaleca się, że ustanowić woluminu tymczasowe na woluminie z systemem innym niż systemu Windows. Jeśli zauważysz problemy z kopii zapasowych stanu systemu, Sprawdzanie lokalizacji woluminu przemieszczania jest pierwszym krokiem rozwiązywania problemów.** 

### <a name="how-can-i-change-the-staging-volume-path-specified-in-the-azure-backup-agent"></a>Jak zmienić ścieżkę woluminu przemieszczania określone w agenta usługi Kopia zapasowa Azure?

Wolumin przemieszczania znajduje się w folderze z pamięcią podręczną domyślnie. 

1. Aby zmienić tę lokalizację, wpisz następujące polecenie (w wierszu polecenia z podwyższonym poziomem uprawnień):
  ```
  PS C:\> Net stop obengine
  ```

2. Następnie zaktualizuj następujące wpisy rejestru ze ścieżką do nowego folderu przemieszczania woluminu.

  |Ścieżka rejestru|Klucz rejestru|Wartość|
  |-------------|------------|-----|
  |HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | SSBStagingPath | Nowa lokalizacja woluminu przemieszczania |

Ścieżka przemieszczania jest rozróżniana wielkość liter i muszą być dokładnie odpowiadać tej samej jako co istnieje na serwerze. 

3. Po zmianie ścieżki woluminu przemieszczania, uruchom ponownie Aparat kopii zapasowej:
  ```
  PS C:\> Net start obengine
  ```
4. Aby pobrać zmieniona ścieżka, otwórz agenta usług odzyskiwania Microsoft Azure i Wyzwól ad hoc kopię zapasową stanu systemu.

### <a name="why-is-the-system-state-default-retention-set-to-60-days"></a>Dlaczego ustawiono przechowywania domyślnego stanu systemu do 60 dni

Użytkowania kopii zapasowej stanu systemu jest taka sama jak ustawienie "okresu istnienia reliktu" dla roli usługi Active Directory systemu Windows Server. Wartość domyślna dla wpisu okresu istnienia reliktu wynosi 60 dni. Tę wartość można ustawić dla obiektu konfiguracji usługi Directory (NTDS).

### <a name="how-do-i-change-the-default-backup-and-retention-policy-for-system-state"></a>Jak zmienić domyślny kopii zapasowej i zasad przechowywania dla stanu systemu?

Aby zmienić domyślne kopii zapasowej i zasad przechowywania dla stanu systemu:
1. Zatrzymaj Aparat kopii zapasowej. Uruchom następujące polecenie w wierszu polecenia z podwyższonym poziomem uprawnień.

  ```
  PS C:\> Net stop obengine
  ```

2. Dodaj lub zaktualizuj następujących wpisach kluczy rejestru w HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.

  |Nazwa rejestru|Opis|Wartość|
  |-------------|-----------|-----|
  |SSBScheduleTime|Służy do konfigurowania tworzenia kopii zapasowej. Domyślna to 21: 00 czasu lokalnego.|DWord: Format hh: mm (dziesiętna) na przykład 2130 dla 21:30:00 czasu lokalnego|
  |SSBScheduleDays|Służy do konfigurowania dni, kiedy należy wykonać kopii zapasowej stanu systemu w określonym czasie. Poszczególnych cyfr określ dni tygodnia. 0 oznacza niedzielę, 1 oznacza poniedziałek, i tak dalej. Dzień domyślny dla kopii zapasowej oznacza niedzielę.|DWord: dni tygodnia uruchamianie tworzenia kopii zapasowej (dziesiętna), na przykład 1230 harmonogramy tworzenia kopii zapasowych w poniedziałek, Wtorek, środę i niedzielę.|
  |SSBRetentionDays|Służy do konfigurowania dni przechowywania kopii zapasowej. Wartość domyślna to 60. Maksymalna dozwolona wartość to 180.|DWord: Dni przechowywania kopii zapasowych (dziesiętna).|

3. Użyj następującego polecenia, aby ponownie uruchomić aparatu tworzenia kopii zapasowej.
    ```
    PS C:\> Net start obengine
    ```

4. Otwórz agenta usług odzyskiwania Microsoft.

5. Kliknij przycisk **harmonogram tworzenia kopii zapasowych** , a następnie kliknij przycisk **dalej** do momentu wyświetlenia wprowadzonych zmian.

6. Kliknij przycisk **Zakończ** Aby zastosować zmiany.


## <a name="questions"></a>Pytania?
Jeśli masz pytania lub jeśli brakuje Ci jakiejś funkcji, [prześlij nam opinię](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej o [tworzeniu kopii zapasowej maszyn z systemem Windows](backup-configure-vault.md).
* Teraz, gdy utworzono kopię zapasową plików i folderów, możesz [zarządzać swoimi magazynami i serwerami](backup-azure-manage-windows-server.md).
* Jeśli chcesz przywrócić kopię zapasową, w tym artykule znajdziesz informacje dotyczące [przywracania plików na maszynę z systemem Windows](backup-azure-restore-windows-server.md).
