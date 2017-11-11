---
title: "Utwórz kopię zapasową systemu Windows server lub stacji roboczej do platformy Azure (modelu klasycznego) | Dokumentacja firmy Microsoft"
description: "Kopii zapasowych serwerów z systemem Windows lub klientów do magazynu kopii zapasowych na platformie Azure. Przejść przez podstawowe informacje dotyczące ochrony plików i folderów do magazynu kopii zapasowej za pomocą agenta usługi Kopia zapasowa Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: Magazyn kopii zapasowych; Tworzenie kopii zapasowej systemu Windows server; Kopia zapasowa systemu windows;
ms.assetid: 3b543bfd-8978-4f11-816a-0498fe14a8ba
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 717d1f377e1a074984a0332b978754bea26ce144
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/11/2017
---
# <a name="back-up-a-windows-server-or-workstation-to-azure-using-the-classic-portal"></a>Tworzenie kopii zapasowej systemu Windows server lub stacji roboczej przy użyciu klasycznego portalu Azure
> [!div class="op_single_selector"]
> * [Portal klasyczny](backup-configure-vault-classic.md)
> * [Witryna Azure Portal](backup-configure-vault.md)
>
>

W tym artykule opisano procedury, które należy wykonać, aby przygotować swoje środowisko oraz tworzyć kopie zapasowe systemu Windows server (lub stacji roboczej) na platformie Azure. Obejmuje ona również zagadnienia dotyczące wdrażania rozwiązania do tworzenia kopii zapasowej. Jeśli interesuje Cię w trakcie tworzenia kopii zapasowej Azure po raz pierwszy, w tym artykule szybko przeprowadzi Cię przez proces.

Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: Resource Manager i model klasyczny. Ten artykuł dotyczy klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager.

## <a name="before-you-start"></a>Przed rozpoczęciem
Aby utworzyć kopię zapasową serwera lub klienta na platformie Azure, potrzebne jest konto platformy Azure. Jeśli nie masz, możesz utworzyć [bezpłatne konto](https://azure.microsoft.com/free/) w zaledwie kilka minut.

## <a name="create-a-backup-vault"></a>Tworzenie magazynu kopii zapasowych
Aby utworzyć kopię zapasową plików i folderów z serwera lub klienta, musisz utworzyć magazyn kopii zapasowych w regionie geograficznym, w którym chcesz przechowywać dane.

> [!IMPORTANT]
> Począwszy od marca 2017 r. nie można już tworzyć magazynów kopii zapasowych za pomocą klasycznego portalu.
>
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> Po **30 listopada 2017**, nie można utworzyć magazyny kopii zapasowych przy użyciu programu PowerShell. <br/> **W dniu 30 listopada 2017**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>


## <a name="download-the-vault-credential-file"></a>Pobierz plik poświadczeń magazynu
Na lokalnej maszynie wymaga uwierzytelniania z magazynu kopii zapasowych, zanim ją wykonać kopię zapasową danych na platformie Azure. Uwierzytelnianie odbywa się za pośrednictwem *magazynu poświadczeń*. Plik poświadczeń magazynu jest pobierana za pośrednictwem bezpiecznego kanału z klasycznego portalu. Klucz prywatny certyfikatu nie zmieniają się w portalu lub usługi.

### <a name="to-download-the-vault-credential-file-to-a-local-machine"></a>Aby pobrać plik poświadczeń magazynu na komputerze lokalnym
1. W okienku nawigacji po lewej stronie kliknij **usług odzyskiwania**, a następnie wybierz utworzony magazynu kopii zapasowych.

    ![Początkowa replikacja została zakończona](./media/backup-configure-vault-classic/rs-left-nav.png)
2. Na stronie Szybki Start kliknij **poświadczenia magazynu pobierania**.

   Klasyczny portal generuje poświadczenia magazynu przy użyciu kombinacji nazwy magazynu i bieżącą datę. Plik poświadczeń magazynu jest używana tylko podczas rejestracji przepływu pracy i wygasa po 48 godzin.

   Plik poświadczeń magazynu można pobrać z portalu.
3. Kliknij przycisk **zapisać** do Pobierz plik poświadczeń magazynu do konta lokalnego folderu pliki do pobrania. Możesz też wybrać **Zapisz jako** z **zapisać** menu, aby określić lokalizację pliku poświadczeń magazynu.

   > [!NOTE]
   > Upewnij się, że plik poświadczeń magazynu jest zapisywany w lokalizacji dostępnej z komputera. Jeśli jest on przechowywany w bloku komunikatów udziale lub na serwerze plików, sprawdź, czy masz uprawnienia dostępu do niego.
   >
   >

## <a name="download-install-and-register-the-backup-agent"></a>Pobieranie, instalowanie i rejestrowanie agenta kopii zapasowej
Po utworzeniu magazynu kopii zapasowych i Pobierz plik poświadczeń magazynu, na wszystkich maszynach w systemie Windows musi być zainstalowany agent.

### <a name="to-download-install-and-register-the-agent"></a>Aby pobrać, zainstalować i rejestrowanie agenta
1. Kliknij przycisk **usług odzyskiwania**, a następnie wybierz magazyn kopii zapasowych, który ma zostać zarejestrowany na serwerze.
2. Na stronie Szybki Start kliknij agenta **agenta dla systemu Windows Server lub klienta programu System Center Data Protection Manager lub Windows**. Następnie kliknij przycisk **Save** (Zapisz).

    ![Zapisz agenta](./media/backup-configure-vault-classic/agent.png)
3. Po pobraniu pliku MARSagentinstaller.exe, kliknij przycisk **Uruchom** (lub kliknij dwukrotnie **MARSAgentInstaller.exe** z zapisanej lokalizacji).
4. Wybierz folder instalacji i folderu pamięci podręcznej, która jest wymagana dla agenta, a następnie kliknij przycisk **dalej**. Lokalizacja pamięci podręcznej, które określisz musi mieć miejsca równy co najmniej 5 procent danych kopii zapasowej.
5. Możesz połączyć się z Internetem przy użyciu domyślnych ustawień serwera proxy.             Jeśli używasz serwera proxy do łączenia się z Internetem, na stronie Konfiguracja serwera Proxy, zaznacz **użyć niestandardowych ustawień serwera proxy** pole wyboru, a następnie wprowadź szczegóły serwera proxy. Jeśli korzystasz z uwierzytelnionego serwera proxy, wprowadź szczegóły nazwy i hasła użytkownika, a następnie kliknij przycisk **dalej**.
6. Kliknij przycisk **zainstalować** do rozpoczęcia instalacji agenta. Instaluje agenta kopii zapasowej programu Windows PowerShell i .NET Framework 4.5 (Jeśli to nie jest jeszcze zainstalowana) do ukończenia instalacji.
7. Po zainstalowaniu agenta kliknij przycisk **przejść do rejestracji** aby kontynuować przepływ pracy.
8. Na stronie magazyn identyfikacji Wyszukaj i wybierz plik poświadczeń magazynu pobranego wcześniej.

    Plik poświadczeń magazynu jest prawidłowa tylko 48 godzin po pobraniu jej z portalu. Jeśli wystąpi błąd na tej stronie (np. "poświadczenia magazynu wygasł podany plik"), zaloguj się do portalu i ponownie Pobierz plik poświadczeń magazynu.

    Upewnij się, że plik poświadczeń magazynu jest dostępna w lokalizacji dostępnej dla aplikacji Instalatora. Jeśli wystąpią błędy dotyczące dostępu, skopiuj plik poświadczeń magazynu do tymczasowej lokalizacji na tym samym komputerze i spróbuj ponownie wykonać operację.

    Jeśli wystąpi błąd poświadczeń magazynu, takich jak "udostępniono nieprawidłowe poświadczenia magazynu", plik jest uszkodzony lub jest nie ma najnowszych poświadczeń skojarzonych z usługą odzyskiwania. Spróbuj ponownie wykonać operację po pobraniu nowego pliku poświadczeń magazynu z portalu. Ten błąd może wystąpić, gdy użytkownik kliknie **poświadczenia magazynu pobierania** opcję wiele razy pod rząd szybkie. W takim przypadku tylko ostatni plik poświadczeń magazynu jest nieprawidłowa.
9. Na stronie ustawienie szyfrowania możesz wygenerować hasło lub podać hasło (z co najmniej 16 znaków). Pamiętaj, aby zapisać hasła w bezpiecznym miejscu.
10. Kliknij przycisk **Zakończ**. Kreatora rejestrowania serwera rejestruje serwera z kopii zapasowej.

    > [!WARNING]
    > Jeśli utracisz lub zapomnisz hasło, Microsoft nie może pomóc odzyskać dane kopii zapasowej. Właścicielem hasło szyfrowania i Microsoft nie ma wgląd w hasło, którego używasz. Zapisz plik w bezpiecznym miejscu, ponieważ będzie wymagane podczas operacji odzyskiwania.
    >
    >

11. Po klucz szyfrowania jest ustawiona, pozostaw **uruchamianie agenta usług odzyskiwania Azure Microsoft** zaznaczone pole wyboru, a następnie kliknij przycisk **Zamknij**.

## <a name="complete-the-initial-backup"></a>Ukończenie początkowej kopii zapasowej
Tworzenie początkowej kopii zapasowej obejmuje dwa kluczowe zadania:

* Tworzenie harmonogramu tworzenia kopii zapasowych
* Tworzenie kopii zapasowej plików i folderów po raz pierwszy

Po zakończeniu tworzenia początkowej kopii zapasowej zasad tworzenia kopii zapasowej powoduje utworzenie punktów kopii zapasowej, które można użyć, jeśli chcesz odzyskać dane. Zasady tworzenia kopii zapasowej dzieje się tak na podstawie zdefiniowanego harmonogramu.

### <a name="to-schedule-the-backup"></a>Aby zaplanować tworzenie kopii zapasowej
1. Otwórz agenta usługi Kopia zapasowa Microsoft Azure. (Zostanie otwarty automatycznie, gdy zostanie pozostawiony **uruchamianie agenta usług odzyskiwania Azure Microsoft** pole wyboru jest zaznaczone, po zamknięciu kreatora rejestrowania serwera.) Aby go znaleźć, wyszukaj na maszynie łańcuch **Microsoft Azure Backup**.

    ![Uruchamianie agenta usługi Kopia zapasowa Azure](./media/backup-configure-vault-classic/snap-in-search.png)
2. W agenta kopii zapasowej, kliknij przycisk **harmonogram tworzenia kopii zapasowych**.

    ![Planowanie tworzenia kopii zapasowej systemu Windows Server](./media/backup-configure-vault-classic/schedule-backup-close.png)
3. Na stronie Wprowadzenie Kreatora harmonogramu kopii zapasowej kliknij przycisk **Dalej**.
4. Na stronie Wybieranie elementów do wykonania kopii zapasowej kliknij pozycję **Dodaj elementy**.
5. Wybierz pliki i foldery, których kopię zapasową chcesz utworzyć, a następnie kliknij przycisk **OK**.
6. Kliknij przycisk **Dalej**.
7. Na stronie **Określanie harmonogramu tworzenia kopii zapasowych** określ **harmonogram tworzenia kopii zapasowej**, a następnie kliknij przycisk **Dalej**.

    Można zaplanować tworzenie kopii zapasowych codziennie (maksymalnie trzy razy) lub co tydzień.

    ![Elementy związane z tworzeniem kopii zapasowej systemu Windows Server](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

   > [!NOTE]
   > Aby uzyskać więcej informacji o sposobie określania harmonogramu tworzenia kopii zapasowych, zobacz artykuł [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md) (Użycie usługi Azure Backup do zastąpienia infrastruktury taśm).
   >
   >

8. Na stronie **Wybieranie zasady przechowywania** wybierz pozycję **Zasady przechowywania** dla kopii zapasowej.

    Zasady przechowywania służą do określania czasu przechowywania kopii zapasowej. Zamiast określać wspólne zasady dla wszystkich punktów kopii zapasowej, można określić różne zasady przechowywania w zależności od momentu tworzenia kopii zapasowej. Możliwe jest modyfikowanie, zgodnie z potrzebami, zasad przechowywania codziennych, cotygodniowych, comiesięcznych i corocznych kopii zapasowych.
9. Na stronie Wybieranie typu początkowej kopii zapasowej wybierz typ początkowej kopii zapasowej. Pozostaw zaznaczoną opcję **Automatycznie przez sieć**, a następnie kliknij przycisk **Dalej**.

    Kopie zapasowe można tworzyć automatycznie za pośrednictwem sieci lub w trybie offline. W dalszej części tego artykułu opisano proces automatycznego tworzenia kopii zapasowych. Jeśli chcesz tworzyć kopie zapasowe w trybie offline, zapoznaj się z artykułem [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) (Przepływ pracy tworzenia kopii zapasowej w trybie offline w usłudze Azure Backup), aby uzyskać dodatkowe informacje.
10. Przejrzyj informacje na stronie Potwierdzenie, a następnie kliknij przycisk **Zakończ**.
11. Po ukończeniu harmonogramu tworzenia kopii zapasowej przez kreatora kliknij przycisk **Zamknij**.

### <a name="enable-network-throttling-optional"></a>Włącz ograniczanie przepustowości sieci (opcjonalnie)
Agent usługi Kopia zapasowa zawiera ograniczanie przepustowości sieci. Ograniczanie kontrolek z wykorzystaniem przepustowości sieci podczas transferu danych. Ten formant może być przydatne, jeśli chcesz utworzyć kopię zapasową danych podczas godziny pracy, ale nie chcesz procesu tworzenia kopii zapasowej z innych ruch internetowy. Ograniczenie ma zastosowanie do kopii zapasowej i przywracania działań.

**Aby włączyć ograniczenie przepustowości sieci**

1. W agenta kopii zapasowej, kliknij przycisk **Zmień właściwości**.

    ![Zmień właściwości](./media/backup-configure-vault-classic/change-properties.png)
2. Na **ograniczania** wybierz opcję **włączyć użycia przepustowości połączenia internetowego do operacji tworzenia kopii zapasowej** pole wyboru.

    ![Ograniczanie przepustowości sieci](./media/backup-configure-vault-classic/throttling-dialog.png)
3. Po włączeniu ograniczenia przepustowości, należy określić dozwolony przepustowości dla transferu danych kopii zapasowej podczas **godzin pracy** i **godziny wolne**.

    Wartości przepustowości rozpocząć 512 kilobitów na sekundę (KB/s) i można przejść do 1,023 MB na sekundę (MB/s). Można również wyznaczyć rozpoczęcia i zakończenia **godzin pracy**, i dni tygodnia są uważane dniach. Godziny poza wyznaczonych są traktowane jako godzin pracy z systemem innym niż godziny pracy.
4. Kliknij przycisk **OK**.

### <a name="to-back-up-now"></a>Aby utworzyć kopię zapasową teraz
1. W agenta kopii zapasowej, kliknij przycisk **wykonaj kopię zapasową teraz** aby zakończyć początkowe umieszczanie za pośrednictwem sieci.

    ![Natychmiastowe tworzenie kopii zapasowej systemu Windows Server](./media/backup-configure-vault-classic/backup-now.png)
2. Na stronie Potwierdzenie przejrzyj ustawienia, które zostaną użyte przez Kreatora natychmiastowego tworzenia kopii zapasowej do utworzenia kopii zapasowej maszyny. Następnie kliknij pozycję **Utwórz kopię zapasową**.
3. Kliknij przycisk **Zamknij**, aby zamknąć kreatora. Jeśli zrobisz to przed zakończeniem procesu tworzenia kopii zapasowej, kreator będzie nadal działał w tle.

Po zakończeniu tworzenia początkowej kopii zapasowej w konsoli usługi Backup zostanie wyświetlony stan **Ukończono zadanie**.

![Początkowa replikacja została zakończona](./media/backup-configure-vault-classic/ircomplete.png)

## <a name="next-steps"></a>Następne kroki
* Zaloguj się do [bezpłatne konto platformy Azure](https://azure.microsoft.com/free/).

Aby uzyskać dodatkowe informacje na temat tworzenia kopii zapasowych maszyn wirtualnych lub innych obciążeń zobacz:

* [Tworzenie kopii zapasowych maszyn wirtualnych IaaS](backup-azure-vms-prepare.md)
* [Tworzenie kopii zapasowej obciążeń na platformie Azure serwer kopii zapasowej Microsoft Azure](backup-azure-microsoft-azure-backup.md)
* [Tworzenie kopii zapasowej obciążeń na platformie Azure za pomocą programu DPM](backup-azure-dpm-introduction.md)
