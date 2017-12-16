---
title: "Zarządzanie magazynami kopia zapasowa Azure i serwerami przy użyciu klasycznego modelu wdrażania usługi Azure | Dokumentacja firmy Microsoft"
description: "Użyj tego samouczka, aby dowiedzieć się, jak zarządzać magazynami kopia zapasowa Azure i serwerami."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: cwatson
ms.openlocfilehash: 3a54abe5b86aab41544e61d311ecfab19b577aa7
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/15/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a>Zarządzanie magazynami i serwerami usługi Azure Backup przy użyciu klasycznego modelu wdrażania
> [!div class="op_single_selector"]
> * [Resource Manager](backup-azure-manage-windows-server.md)
> * [Wdrożenie klasyczne](backup-azure-manage-windows-server-classic.md)
>
>

W tym artykule znajdziesz Przegląd zadań zarządzania kopiami zapasowymi dostępnych za pośrednictwem klasycznego portalu Azure i agenta kopii zapasowej Microsoft Azure.

> [!IMPORTANT]
> Platforma Azure ma dwa różne modele wdrażania do tworzenia i pracy z zasobami: [Resource Manager i Model Klasyczny](../azure-resource-manager/resource-manager-deployment-model.md). W tym artykule omówiono przy użyciu klasycznego modelu wdrożenia. Firma Microsoft zaleca, aby w przypadku większości nowych wdrożeń korzystać z modelu opartego na programie Resource Manager.

> [!IMPORTANT]
> Magazyny kopii zapasowych możesz teraz uaktualnić do magazynów usługi Recovery Services. Więcej szczegółów znajduje się w artykule [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uaktualnianie magazynu kopii zapasowych do magazynu usługi Recovery Services). Firma Microsoft zachęca do przeprowadzenia uaktualnienia magazynów kopii zapasowych do magazynów usługi Recovery Services.<br/> Po 30 listopada 2017 nie można utworzyć magazyny kopii zapasowych przy użyciu programu PowerShell. **W dniu 30 listopada 2017**:
>- Wszystkie pozostałe magazyny kopii zapasowych zostaną automatycznie uaktualnione do magazynów usługi Recovery Services.
>- Nie będzie możliwe uzyskanie dostępu do danych kopii zapasowych w portalu klasycznym. Zamiast tego należy użyć witryny Azure Portal, aby uzyskać dostęp do danych kopii zapasowych w magazynach usługi Recovery Services.
>

## <a name="management-portal-tasks"></a>Zadania portalu zarządzania
1. Zaloguj się do [portalu zarządzania](https://manage.windowsazure.com).
2. Kliknij przycisk **usług odzyskiwania**, następnie kliknij nazwę magazynu kopii zapasowych, aby wyświetlić stronę Szybki Start.

    ![Usługi odzyskiwania](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

Wybierając opcje u góry strony Szybki Start widać dostępnych zadań zarządzania.

![Zarządzanie karty](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a>Pulpit nawigacyjny
Wybierz **pulpitu nawigacyjnego** przejrzeć obciążenie serwera. **Przegląd wykorzystania** obejmuje:

* Liczba serwerów z systemem Windows zarejestrowane w chmurze
* Liczba maszyn wirtualnych platformy Azure chroniona w chmurze
* Całkowita ilość miejsca, które są używane w systemie Azure
* Stan ostatnich zadań

W dolnej części pulpitu nawigacyjnego można wykonywać następujące zadania:

* **Zarządzanie certyfikatami** — Jeśli certyfikat został użyty do zarejestrować serwer, a następnie użyć tej funkcji można zaktualizować certyfikat. Jeśli używane są poświadczenia magazynu, nie używaj **Zarządzaj certyfikatem**.
* **Usuń** — usuwa bieżący magazyn kopii zapasowych. Jeśli magazyn kopii zapasowych jest już używana, można usunąć go w celu zwolnienia miejsca do magazynowania. **Usuń** jest włączona tylko po wszystkie zarejestrowane serwery zostały usunięte z magazynu.

![Zadania tworzenia kopii zapasowej pulpitu nawigacyjnego](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a>Zarejestrowanych elementów
Wybierz **zarejestrowane elementy** Aby wyświetlić nazwy serwerów, które są zarejestrowane w tym magazynie.

![Zarejestrowanych elementów](./media/backup-azure-manage-windows-server-classic/registered-items.png)

**Typu** filtru wartości domyślnych do maszyny wirtualnej platformy Azure. Zaznacz, aby wyświetlić nazwy serwerów, które są zarejestrowane w tym magazynie **systemu Windows server** z menu rozwijanego.

W tym miejscu można wykonywać następujące zadania:

* **Zezwalaj na ponowną rejestrację** — po wybraniu tej opcji można użyć serwera **Kreatora rejestracji** w agencie kopia zapasowa Microsoft Azure lokalnego, aby zarejestrować serwer z kopią zapasową magazynu po raz drugi. Może być konieczne ponowne zarejestrowanie z powodu błędu w certyfikacie lub jeśli serwer będzie musiała zostać również przebudowany.
* **Usuń** — usuwa serwer z magazynu kopii zapasowych. Wszystkie przechowywane dane skojarzone z serwerem zostaną natychmiast usunięte.

    ![Zarejestrowanych elementów zadań](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a>Elementy chronione
Wybierz **chronione elementy** Aby wyświetlić elementy, które kopii zapasowej z serwerów.

![Elementy chronione](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a>Konfigurowanie
Z **Konfiguruj** można wybrać opcję nadmiarowość magazynu odpowiednie karty. Najlepszy moment, aby wybrać opcję nadmiarowość magazynu jest odpowiednia, po utworzeniu magazynu i przed wszystkie komputery są zarejestrowane do niego.

> [!WARNING]
> Po zarejestrowaniu w magazynie, opcji nadmiarowość magazynu jest zablokowany i nie może być modyfikowany.
>
>

![Konfigurowanie](./media/backup-azure-manage-windows-server-classic/configure.png)

Zobacz ten artykuł, aby uzyskać więcej informacji [nadmiarowość magazynu](../storage/common/storage-redundancy.md).

## <a name="microsoft-azure-backup-agent-tasks"></a>Zadania agenta usługi Kopia zapasowa Microsoft Azure
### <a name="console"></a>Konsola
Otwórz **agenta usługi Kopia zapasowa Microsoft Azure** (można go znaleźć, wyszukaj na maszynie łańcuch *kopia zapasowa Microsoft Azure*).

![Agent usługi Kopia zapasowa](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

Z **akcje** dostępne po prawej stronie konsoli agenta kopii zapasowej można wykonywać na następujące zadania zarządzania:

* Zarejestruj serwer
* Harmonogram tworzenia kopii zapasowych
* Wykonaj kopię zapasową teraz
* Zmień właściwości

![Akcje konsoli agentów.](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> Aby **odzyskać dane**, zobacz [przywrócić pliki do systemu Windows server lub komputer kliencki z systemem Windows](backup-azure-restore-windows-server.md).
>
>

### <a name="modify-an-existing-backup"></a>Modyfikowanie istniejącej kopii zapasowej
1. W agencie kopia zapasowa Microsoft Azure kliknij **harmonogram tworzenia kopii zapasowych**.

    ![Planowanie kopii zapasowych serwera systemu Windows](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. W **Kreatora kopii zapasowych harmonogram** pozostaw **wprowadzanie zmian do elementów kopii zapasowych lub razy** zaznaczono opcję i kliknij przycisk **dalej**.

    ![Modyfikowanie zaplanowanego tworzenia kopii zapasowej](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. Jeśli chcesz dodać lub zmienić elementy, na **Wybieranie elementów do wykonania kopii zapasowej** ekranu kliknij **Dodaj elementy**.

    Można również ustawić **ustawienia wykluczania** na tej stronie kreatora. Jeśli chcesz wykluczyć pliki lub typów plików zapoznać się z procedurą dodawania [ustawienia wykluczania](#exclusion-settings).
4. Wybierz pliki i foldery, które chcesz utworzyć kopię zapasową, a następnie kliknij przycisk **OK**.

    ![Dodaj elementy](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. Określ **harmonogram tworzenia kopii zapasowych** i kliknij przycisk **dalej**.

    Można zaplanować codziennie (maksymalnie 3 razy dziennie) lub cotygodniowe kopie zapasowe.

    ![Określ harmonogram tworzenia kopii zapasowej](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > Określanie harmonogramu tworzenia kopii zapasowych omówiono szczegółowo w tym [artykułu](backup-azure-backup-cloud-as-tape.md).
   >
   >
6. Wybierz **zasady przechowywania** kopii zapasowej i kliknij **dalej**.

    ![Wybierz zasady przechowywania](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. Na **potwierdzenie** ekranu Przejrzyj informacje i kliknij przycisk **Zakończ**.
8. Po zakończeniu pracy Kreatora tworzenia **harmonogram tworzenia kopii zapasowych**, kliknij przycisk **Zamknij**.

    Po zmodyfikowaniu ochrony, można potwierdzić poprawnie wyzwalają kopii zapasowych, przechodząc do **zadania** kartę i potwierdzenie, że zmiany zostaną odzwierciedlone w zadania tworzenia kopii zapasowej.

### <a name="enable-network-throttling"></a>Włącz ograniczanie przepustowości sieci
Agent usługi Kopia zapasowa Azure udostępnia na karcie ograniczenia przepustowości, dzięki czemu można kontrolować sposób używania przepustowości sieci podczas transferu danych. Ten formant może być przydatne, jeśli chcesz utworzyć kopię zapasową danych podczas godziny pracy, ale nie chcesz procesu tworzenia kopii zapasowej z innych ruch internetowy. Ograniczanie danych transfer ma zastosowanie do kopii zapasowej i przywracania działań.  

Aby włączyć ograniczenie przepustowości:

1. W **agent usługi Kopia zapasowa**, kliknij przycisk **Zmień właściwości**.
2. Wybierz **włączyć użycia przepustowości połączenia internetowego do operacji tworzenia kopii zapasowej** wyboru.

    ![Ograniczanie przepustowości sieci](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. Po włączeniu ograniczenia przepustowości określić dozwolonych przepustowością transferu danych kopii zapasowej podczas **godzin pracy** i **godziny wolne**.

    Wartości przepustowości rozpoczynały 512 kilobajtach na sekundę (KB/s) i można przejść do 1023 MB na sekundę (MB/s). Można również wyznaczyć rozpoczęcia i zakończenia **godzin pracy**, i dni tygodnia, są traktowane jako pracy dni. Czas poza godzinami pracy wyznaczonych jest uważany za godzin wolnych.
4. Kliknij przycisk **OK**.

## <a name="exclusion-settings"></a>Ustawienia wykluczania
1. Otwórz **agenta usługi Kopia zapasowa Microsoft Azure** (można go znaleźć, wyszukaj na maszynie łańcuch *kopia zapasowa Microsoft Azure*).

    ![Otwórz agenta kopii zapasowej](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. W agencie kopia zapasowa Microsoft Azure kliknij **harmonogram tworzenia kopii zapasowych**.

    ![Planowanie kopii zapasowych serwera systemu Windows](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. W pozostaw Kreatora kopii zapasowych harmonogram **wprowadzanie zmian do elementów kopii zapasowych lub razy** zaznaczono opcję i kliknij przycisk **dalej**.

    ![Modyfikowanie harmonogramu](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. Kliknij przycisk **ustawienia wykluczenia**.

    ![Wybierz elementy do wykluczenia](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. Kliknij przycisk **Dodaj wykluczenia**.

    ![Dodaj wykluczenia](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. Wybierz lokalizację, a następnie kliknij **OK**.

    ![Wybierz lokalizację do wykluczenia](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. Dodaj rozszerzenie pliku w **typ pliku** pola.

    ![Wyklucz według typu pliku](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    Dodawanie rozszerzenia MP3

    ![Przykład typu pliku](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    Aby dodać inne rozszerzenie, kliknij przycisk **Dodawanie wykluczenia** i wprowadź inne rozszerzenie typu pliku (Dodawanie rozszerzenia .jpeg).

    ![Innym przykładem typu pliku](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. Po dodaniu wszystkich rozszerzeń, kliknij przycisk **OK**.
9. Kontynuuj pracę Kreatora harmonogramu kopii zapasowej, klikając **dalej** do momentu **stronę potwierdzenia**, następnie kliknij przycisk **Zakończ**.

    ![Potwierdzenie wyłączenia](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a>Następne kroki
* [Przywracanie systemu Windows Server lub klienta systemu Windows z platformy Azure](backup-azure-restore-windows-server.md)
* Aby dowiedzieć się więcej na temat tworzenia kopii zapasowej Azure, zobacz [Omówienie programu Kopia zapasowa Azure](backup-introduction-to-azure-backup.md)
* Odwiedź stronę [Forum kopii zapasowej platformy Azure](http://go.microsoft.com/fwlink/p/?LinkId=290933)
