---
title: "Serwer kopii zapasowej Azure umożliwia tworzenie kopii zapasowej obciążeń do klasycznego portalu Azure | Dokumentacja firmy Microsoft"
description: "Upewnij się, że środowisko jest poprawnie przygotowany do tworzenia kopii zapasowych obciążeń przy użyciu serwera usługi Kopia zapasowa Azure"
services: backup
documentationcenter: 
author: pvrk
manager: shivamg
editor: 
keywords: Serwer kopii zapasowej systemu Azure; Magazyn kopii zapasowych
ms.assetid: d86450e8-da63-4283-8fd7-2ee629fa14ab
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: masaran;trinadhk;pullabhk;markgal
ms.openlocfilehash: ffef289e154986e4b08a072d3a95f77818fb9c35
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/03/2017
---
# <a name="preparing-to-back-up-workloads-using-azure-backup-server"></a>Przygotowywanie do tworzenia kopii zapasowych obciążeń przy użyciu usługi Azure Backup Server
> [!div class="op_single_selector"]
> * [Azure Backup Server](backup-azure-microsoft-azure-backup.md)
> * [SCDPM](backup-azure-dpm-introduction.md)
> * [Serwer kopii zapasowej systemu Azure (klasyczne)](backup-azure-microsoft-azure-backup-classic.md)
> * [SCDPM (klasyczne)](backup-azure-dpm-introduction-classic.md)
>
>

Ten artykuł dotyczy przygotowywanie środowiska do tworzenia kopii zapasowych obciążeń przy użyciu serwera usługi Kopia zapasowa Azure. Serwer kopii zapasowej Azure umożliwia ochronę obciążeń aplikacji, takich jak maszyn wirtualnych funkcji Hyper-V, programu Microsoft SQL Server, SharePoint Server, Microsoft Exchange i klientów systemu Windows z poziomu pojedynczej konsoli.

> [!WARNING]
> Serwer kopii zapasowej systemu Azure dziedziczy funkcjonalność programu Data Protection Manager (DPM) do utworzenia kopii zapasowej obciążeń. Odwołania do dokumentacji programu DPM dla niektórych z tych funkcji można znaleźć. Jednak serwer kopii zapasowej Azure zapewnia ochronę na taśmie lub nie integracji z programem System Center.
>
>

## <a name="1-windows-server-machine"></a>1. Komputer z systemem Windows Server
![step1](./media/backup-azure-microsoft-azure-backup/step1.png)

Pierwszy krok w kierunku uruchamianie serwera kopii zapasowej Azure i przeprowadzanie ma komputerze z systemem Windows Server.

| Lokalizacja | Minimalne wymagania | Dodatkowe instrukcje |
| --- | --- | --- |
| Azure |Maszyny wirtualne IaaS platformy Azure<br><br>A2 Standard: 2 rdzenie, 3.5GB pamięci RAM |Można uruchomić przy użyciu prostego galerii obrazu systemu Windows Server 2012 R2 Datacenter. [Ochrona za pomocą serwera kopii zapasowej Azure (DPM) obciążenia IaaS](https://technet.microsoft.com/library/jj852163.aspx) ma wiele wszystkie szczegóły. Upewnij się, przeczytaj artykuł na temat całkowicie przed wdrożeniem maszynę. |
| Lokalnie |Maszyna wirtualna funkcji Hyper-V,<br> Maszyna wirtualna oprogramowania VMWare,<br> lub hosta fizycznego.<br><br>2 rdzenie i 4GB pamięci RAM |Użytkownik może deduplikacja magazynu programu DPM, za pomocą deduplikacji serwera systemu Windows. Dowiedz się więcej na temat [programu DPM i deduplikacji](https://technet.microsoft.com/library/dn891438.aspx) współpracują ze sobą po wdrożeniu na maszynach wirtualnych funkcji Hyper-V. |

> [!NOTE]
> Zalecane jest zainstalowanie serwera usługi Kopia zapasowa Azure na komputerze z systemem Windows Server 2012 R2 Datacenter. Automatycznie obejmuje wiele wymagań wstępnych do najnowszej wersji systemu operacyjnego Windows.
>
>

Jeśli planujesz przyłączyć serwer kopii zapasowej Azure do domeny, zaleca się, że dołączysz serwerze fizycznym lub maszynie wirtualnej do domeny przed zainstalowaniem oprogramowania serwera kopii zapasowej Azure. Przenoszenie serwera kopii zapasowej Azure do nowej domeny, po wdrożeniu jest *nieobsługiwane*.

## <a name="2-backup-vault"></a>2. Magazyn kopii zapasowych
![step2](./media/backup-azure-microsoft-azure-backup/step2.png)

Wyślij dane kopii zapasowej na platformie Azure lub schowaj lokalnie, należy zarejestrować serwer kopii zapasowej Azure do magazynu. Jeśli dla nowych użytkowników kopia zapasowa Azure i chcesz używać serwera usługi Kopia zapasowa Azure, zobacz wersji portalu Azure w tym artykule — [przygotowanie do tworzenia kopii zapasowych obciążeń przy użyciu serwera usługi Kopia zapasowa Azure](backup-azure-microsoft-azure-backup.md).

> [!IMPORTANT]
> Począwszy od marca 2017 r. nie można już tworzyć magazynów kopii zapasowych za pomocą klasycznego portalu.
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> Po 15 października 2017 r. nie będzie można tworzyć magazynów kopii zapasowych za pomocą programu PowerShell. **Do 1 listopada 2017 r.**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>



## <a name="3-software-package"></a>3. Pakiet oprogramowania
![krok 3](./media/backup-azure-microsoft-azure-backup/step3.png)

### <a name="downloading-the-software-package"></a>Pobieranie pakietu oprogramowania
Podobnie jak magazyn poświadczeń, można pobrać kopia zapasowa Microsoft Azure dla obciążeń aplikacji z **strony Szybki Start** magazynu kopii zapasowych.

1. Kliknij przycisk **dla obciążeń aplikacji (dysku na dysk w chmurze)**. Spowoduje to przejście do strony Centrum pobierania, z której można pobrać pakietu oprogramowania.

    ![Ekran powitalny kopia zapasowa Microsoft Azure](./media/backup-azure-microsoft-azure-backup/dpm-venus1.png)
2. Kliknij pozycję **Pobierz**.

    ![Pobierz center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter1.png)
3. Zaznacz wszystkie pliki, a następnie kliknij przycisk **dalej**. Pobieranie wszystkich plików przychodzących na stronie pobierania kopia zapasowa Microsoft Azure i umieść wszystkie pliki w tym samym folderze.
   ![Pobierz center 1](./media/backup-azure-microsoft-azure-backup/downloadcenter.png)

    Ponieważ rozmiar pobierania wszystkich plików jednocześnie > sieci 3G, na 10 MB/s pobrać link, który może potrwać do 60 minut do pobrania zakończyć.

### <a name="extracting-the-software-package"></a>Wyodrębnianie pakietu oprogramowania
Po pobraniu wszystkich plików, kliknij przycisk **MicrosoftAzureBackupInstaller.exe**. Spowoduje to uruchomienie **Kreatora instalacji programu Microsoft Azure Backup** do wyodrębnienia plików instalacyjnych do lokalizacji określonej przez użytkownika. Kontynuuj pracę kreatora i kliknij przycisk **wyodrębnić** przycisk, aby rozpocząć proces wyodrębniania.

> [!WARNING]
> Co najmniej 4GB wolnego miejsca jest wymagane do wyodrębnienia plików instalacyjnych.
>
>

![Kreator tworzenia kopii zapasowej Microsoft Azure](./media/backup-azure-microsoft-azure-backup/extract/03.png)

Po zakończeniu procesu wyodrębniania pole wyboru, aby uruchomić świeżo wyodrębnionego *setup.exe* aby rozpocząć instalowanie serwer kopii zapasowej Microsoft Azure i kliknij pozycję **Zakończ** przycisku.

### <a name="installing-the-software-package"></a>Instalowanie pakietu oprogramowania
1. Kliknij przycisk **kopia zapasowa Microsoft Azure** można uruchomić Kreatora instalacji.

    ![Kreator tworzenia kopii zapasowej Microsoft Azure](./media/backup-azure-microsoft-azure-backup/launch-screen2.png)
2. Na ekranie powitalnym kliknij przycisk **dalej** przycisku. Powoduje to przejście do *wymagań wstępnych sprawdza* sekcji. Na tym ekranie kliknij **Sprawdź** przycisk, aby określić, czy zostały spełnione sprzętowe i programowe wymagania wstępne dotyczące serwera usługi Kopia zapasowa Azure. Jeśli wszystkie wymagania wstępne zostały spełnione pomyślnie, pojawi się komunikat wskazujący, że komputer spełnia wymagania. Polecenie **dalej** przycisku.

    ![Sprawdź serwera kopii zapasowej Azure — Zapraszamy i wymagania wstępne](./media/backup-azure-microsoft-azure-backup/prereq/prereq-screen2.png)
3. Serwer kopii zapasowej Microsoft Azure wymaga programu SQL Server Standard, a serwer kopii zapasowej Azure pakiet instalacyjny zawiera powiązane potrzebne odpowiednie pliki binarne programu SQL Server. Przy uruchamianiu nową instalację serwera usługi Kopia zapasowa Azure, należy wybrać opcję **zainstalować nowe wystąpienie programu SQL Server w przypadku takiej konfiguracji** i kliknij przycisk **Sprawdź i zainstaluj** przycisku. Po pomyślnym zainstalowaniu wymagań wstępnych kliknij przycisk **dalej**.

    ![Serwera kopii zapasowej Azure — sprawdzenie programu SQL Server](./media/backup-azure-microsoft-azure-backup/sql/01.png)

    Jeśli wystąpi błąd zgodnie z zaleceniami ponowne uruchomienie komputera, to zrobić i kliknij przycisk **Sprawdź ponownie**.

   > [!NOTE]
   > Serwer kopii zapasowej systemu Azure nie będzie działać przy użyciu zdalnego wystąpienia programu SQL Server. Wystąpienie używane przez serwer kopii zapasowej Azure musi być kontem lokalnym.
   >
   >

4. Podaj lokalizację instalacji plików serwera kopia zapasowa Microsoft Azure i kliknij przycisk **dalej**.

    ![PreReq2 kopia zapasowa Microsoft Azure](./media/backup-azure-microsoft-azure-backup/space-screen.png)

    Pliki tymczasowe lokalizacji jest wymagana do kopii zapasowej Azure. Upewnij się, że pliki tymczasowe lokalizacja jest co najmniej 5% danych planowanych do wykonania kopii zapasowej do chmury. Do ochrony dysku oddzielnych dyskach należy skonfigurować po zakończeniu instalacji. Aby uzyskać więcej informacji na temat pul magazynów, zobacz [Konfigurowanie pul magazynów i dysku magazynu](https://technet.microsoft.com/library/hh758075.aspx).
5. Podaj silne hasło dla kont użytkowników lokalnych z ograniczeniami, a następnie kliknij przycisk **dalej**.

    ![PreReq2 kopia zapasowa Microsoft Azure](./media/backup-azure-microsoft-azure-backup/security-screen.png)
6. Wybierz, czy chcesz użyć *Microsoft Update* Wyszukaj aktualizacje, a następnie kliknij przycisk **dalej**.

   > [!NOTE]
   > Zaleca się o przekierowania do Microsoft Update zapewnia bezpieczeństwo i ważne aktualizacje systemu Windows i innych produktów, takich jak Microsoft Azure Utwórz kopię zapasową serwera usługi Windows Update.
   >
   >

    ![PreReq2 kopia zapasowa Microsoft Azure](./media/backup-azure-microsoft-azure-backup/update-opt-screen2.png)
7. Przegląd *Podsumowanie ustawień* i kliknij przycisk **zainstalować**.

    ![PreReq2 kopia zapasowa Microsoft Azure](./media/backup-azure-microsoft-azure-backup/summary-screen.png)
8. Instalacja odbywa się w fazach. W pierwszej fazie agenta usług odzyskiwania Microsoft Azure jest zainstalowany na serwerze. Kreator sprawdza również połączenie z Internetem. Jeśli dostępny jest połączenie z Internetem można kontynuować instalacji, jeśli nie, należy podać szczegóły serwera proxy, aby nawiązać połączenie z Internetem.

    Następnym krokiem jest skonfigurowanie agenta usług odzyskiwania Microsoft Azure. W ramach konfiguracji należy zapewnić, że jesteś poświadczenia magazynu, aby zarejestrować komputer w magazynie kopii zapasowej. Zostanie również podać hasło do szyfrowania/odszyfrowywania danych przesyłanych między Azure i lokalnej. Można automatycznie wygenerować hasło lub podać własne minimalna hasło 16 znaków. Kontynuować pracę kreatora, dopóki agent został skonfigurowany.

    ![Azure PreReq2 Server kopii zapasowej](./media/backup-azure-microsoft-azure-backup/mars/04.png)
9. Po pomyślnym zakończeniu rejestracji serwera usługi Kopia zapasowa Microsoft Azure, Kreatora konfiguracji ogólnej przechodzą do instalacji i konfiguracji programu SQL Server i składniki serwera kopii zapasowej Azure. Po zakończeniu instalacji składników programu SQL Server, są zainstalowane składniki serwera kopii zapasowej Azure.

    ![Azure Backup Server](./media/backup-azure-microsoft-azure-backup/final-install/venus-installation-screen.png)

Po ukończeniu kroku instalacji produktu ikony pulpitu będzie utworzono również. Wystarczy dwukrotnie kliknąć ikonę, aby uruchomić produktu.

### <a name="add-backup-storage"></a>Dodawanie magazynu kopii zapasowej
Pierwszej kopii zapasowej jest przechowywany w magazynie dołączonym do serwera kopii zapasowej Azure. Aby uzyskać więcej informacji na temat dodawania dysków, zobacz [Konfigurowanie pul magazynów i dysku magazynu](https://technet.microsoft.com/library/hh758075.aspx).

> [!NOTE]
> Należy dodać magazyn kopii zapasowych, nawet wtedy, gdy planujesz do wysyłania danych do platformy Azure. W bieżącym architektury serwera kopii zapasowej Azure przechowuje magazynu kopii zapasowych Azure *drugi* kopię danych podczas Magazyn lokalny zawiera pierwszy (i obowiązkowe) kopii zapasowej.  
>
>

## <a name="4-network-connectivity"></a>4. Połączenie sieciowe
![step4](./media/backup-azure-microsoft-azure-backup/step4.png)

Serwer kopii zapasowej systemu Azure wymaga połączenia z usługą kopia zapasowa Azure produktu działało poprawnie. Aby zweryfikować, czy komputer ma łączność z platformy Azure, użyj ```Get-DPMCloudConnection``` polecenia w konsoli programu PowerShell serwera kopii zapasowej Azure. Jeśli dane wyjściowe polecenia ma wartość PRAWDA, a następnie istnieje połączenie, #else nie ma żadnej łączności.

W tym samym czasie subskrypcji platformy Azure musi być w dobrej kondycji. Aby sprawdzić stan subskrypcji i zarządzania nim, zaloguj się do [portal subskrypcji](https://account.windowsazure.com/Subscriptions).

Znając stanu łączności Azure i subskrypcji Azure, można użyć w poniższej tabeli, aby sprawdzić wpływ na funkcje wykonywania kopii zapasowej i przywracania.

| Stan łączności | Subskrypcja platformy Azure | Kopia zapasowa Azure | Wykonywanie kopii zapasowej na dysku | Przywróć z platformy Azure | Przywróć z dysku |
| --- | --- | --- | --- | --- | --- |
| połączone |Aktywne |Dozwolone |Dozwolone |Dozwolone |Dozwolone |
| połączone |Ważność |Zatrzymane |Zatrzymane |Dozwolone |Dozwolone |
| połączone |Anulowana |Zatrzymane |Zatrzymane |Punkty odzyskiwania zatrzymane, a Azure usunięte |Zatrzymane |
| Utratą połączenia > 15 dni |Aktywne |Zatrzymane |Zatrzymane |Dozwolone |Dozwolone |
| Utratą połączenia > 15 dni |Ważność |Zatrzymane |Zatrzymane |Dozwolone |Dozwolone |
| Utratą połączenia > 15 dni |Anulowana |Zatrzymane |Zatrzymane |Punkty odzyskiwania zatrzymane, a Azure usunięte |Zatrzymane |

### <a name="recovering-from-loss-of-connectivity"></a>Odzyskiwanie z utraty połączenia
Jeśli zapora lub serwer proxy, który uniemożliwia dostęp do platformy Azure, musisz dozwolonych następujące adresy domeny w profilu zapory/serwera proxy:

* www.msftncsi.com
* \*.Microsoft.com
* \*.WindowsAzure.com
* \*.microsoftonline.com
* \*.windows.net

Gdy łączność Azure została przywrócona do komputera serwera usługi Kopia zapasowa Azure, operacje, które mogą być wykonywane są określane przez stan subskrypcji platformy Azure. Powyżej tabela zawiera szczegółowe informacje o operacjach dozwolone, gdy komputer jest "połączony".

### <a name="handling-subscription-states"></a>Obsługa stany subskrypcji
Jest to możliwe, należy subskrypcji platformy Azure z *wygasłe* lub *Deprovisioned* stan *Active* stanu. Jednak ma wpływ na niektóre na zachowanie produktu podczas stan nie jest *Active*:

* A *Deprovisioned* subskrypcji utraci funkcje dla okresu, w którym jest ona anulowana. Na włączanie *Active*, funkcje produktu wykonywania kopii zapasowej i przywracania jest przywrócona. Można również pobrać dane kopii zapasowej na dysku lokalnym Jeśli znajdowały się od okresu przechowywania wystarczająco duża. Jednak dane kopii zapasowej na platformie Azure po nieodwracalnie wprowadza subskrypcji *Deprovisioned* stanu.
* *Wygasłe* subskrypcji tylko utraci funkcje dla dopóki umożliwiono jego *Active* ponownie. Wszelkich kopii zapasowych zaplanowanych dla okresu subskrypcji została *wygasłe* nie zostaną uruchomione.

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Jeśli kopia zapasowa Microsoft Azure serwera zakończy się niepowodzeniem z błędami fazy instalacji (lub kopii zapasowej lub przywracania), zapoznaj się to [dokumentu kody błędów](https://support.microsoft.com/kb/3041338) Aby uzyskać więcej informacji.
Można także odwoływać się do [— często zadawane pytania dotyczące usługi Kopia zapasowa Azure](backup-azure-backup-faq.md)

## <a name="next-steps"></a>Następne kroki
Aby wyświetlić szczegółowe informacje dotyczące [przygotowywanie środowiska dla programu DPM](https://technet.microsoft.com/library/hh758176.aspx) w witrynie Microsoft TechNet. Zawiera informacje o obsługiwanych konfiguracjach, na których można wdrożyć i używany serwer kopii zapasowej Azure.

Skorzystaj z tych artykułów, aby lepiej zrozumieć ochrony obciążenia przy użyciu serwera kopia zapasowa Microsoft Azure.

* [Kopia zapasowa programu SQL Server](backup-azure-backup-sql.md)
* [Kopia zapasowa programu SharePoint server](backup-azure-backup-sharepoint.md)
* [Alternatywny serwer kopii zapasowej](backup-azure-alternate-dpm-server.md)
