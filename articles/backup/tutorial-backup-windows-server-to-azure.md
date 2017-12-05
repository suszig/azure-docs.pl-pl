---
title: Tworzenie kopii zapasowej systemu Windows Server na platformie Azure | Dokumentacja firmy Microsoft
description: "Ten samouczek szczegóły tworzenie kopii zapasowych serwerów z systemem Windows lokalnymi w magazynie usług odzyskiwania."
services: back up
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: "Windows server, wykonaj kopię zapasową; Tworzenie kopii zapasowej systemu windows server; wykonywanie kopii zapasowych i odzyskiwania po awarii"
ms.assetid: 
ms.service: back up
ms.workload: storage-back up-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/23/2017
ms.author: saurabhsensharma;markgal;
ms.custom: mvc
ms.openlocfilehash: 0dbf3850c7fcccb1a02e70a19b498522a4ce0e79
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="back-up-windows-server-to-azure"></a>Tworzenie kopii zapasowych serwera z systemem Windows na platformie Azure


Kopia zapasowa Azure umożliwia ochronę serwera z systemem Windows przed uszkodzeń, ataków i awarii. Kopia zapasowa Azure stanowi uproszczone narzędzie znany jako agenta usług odzyskiwania Azure firmy Microsoft (MARS). MARS agent jest zainstalowany w systemie Windows Server w celu ochrony plików i folderów oraz informacje o konfiguracji serwera za pomocą stanu systemu Windows Server. W tym samouczku opisano, jak można użyć agenta MARS, aby utworzyć kopię zapasową serwera z systemem Windows Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności: 


> [!div class="checklist"]
> * Pobierz i skonfiguruj agenta MARS
> * Skonfiguruj kopię zapasową razy i harmonogram przechowywania kopii zapasowych serwera
> * Wykonaj kopię zapasową ad hoc


## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Zaloguj się w witrynie Azure Portal pod adresem http://portal.azure.com.

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

Przed utworzeniem kopii zapasowej systemu Windows Server, należy utworzyć miejsce na kopie zapasowe, lub przywrócić punkty przechowywania. A [magazyn usług odzyskiwania](backup-azure-recovery-services-vault-overview.md) to kontener na platformie Azure, która przechowuje kopie zapasowe z serwera z systemem Windows. Wykonaj poniższe kroki, aby utworzyć magazyn usług odzyskiwania w portalu Azure. 

1. W menu po lewej stronie wybierz **wszystkie usługi** i na liście usług wpisz **usług odzyskiwania**. Kliknij przycisk **Magazyny usług odzyskiwania**.

   ![Otwórz magazyn usług odzyskiwania](./media/tutorial-backup-windows-server-to-azure/full-browser-open-rs-vault_2.png)

2.  W menu **Magazyny usługi Recovery Services** kliknij pozycję **Dodaj**.

   ![Podaj informacje dotyczące magazynu](./media/tutorial-backup-windows-server-to-azure/provide-vault-detail-2.png)

3.  W **magazyn usług odzyskiwania** menu

    - Typ *myRecoveryServicesVault* w **nazwa**.
    - Bieżącej subskrypcji identyfikator jest wyświetlany w **subskrypcji**.
    - Aby uzyskać **grupy zasobów**, wybierz pozycję **Użyj istniejącego** i wybierz polecenie *myResourceGroup*. Jeśli *myResourceGroup* nie istnieje, wybierz **Utwórz nowy** i typ *myResourceGroup*. 
    - Z **lokalizacji** menu rozwijanego wybierz *Europa*.
    - Kliknij przycisk **Utwórz** utworzyć magazyn usług odzyskiwania.
 
Po utworzeniu magazynu pojawi się on na liście magazynów Usług odzyskiwania.

## <a name="download-recovery-services-agent"></a>Pobierz agenta usług odzyskiwania

Agent usług odzyskiwania Azure firmy Microsoft (MARS) tworzy skojarzenie między serwerem z systemem Windows i magazynu usług odzyskiwania. W poniższej procedurze wyjaśniono, jak pobrać agenta do serwera.

1.  Wybierz z listy magazynów usług odzyskiwania **myRecoveryServicesVault** aby otworzyć jego pulpitu nawigacyjnego.

   ![Podaj informacje dotyczące magazynu](./media/tutorial-backup-windows-server-to-azure/open-vault-from-list.png)

2.  W menu nawigacyjnym magazynu, kliknij polecenie **kopii zapasowej**.

3.  Na **cel kopii zapasowej** menu:

    - dla **gdzie działa Twoje obciążenie?**, wybierz pozycję**lokalnymi**, 
    - dla **co chcesz utworzyć kopię zapasową?**, wybierz pozycję **pliki i foldery** i **stanu systemu** 

    ![Podaj informacje dotyczące magazynu](./media/tutorial-backup-windows-server-to-azure/backup-goal.png)
    
4.  Kliknij przycisk **przygotowanie infrastruktury** otworzyć **przygotowanie infrastruktury** menu.
5.  Na **przygotowanie infrastruktury** menu, kliknij przycisk **Pobierz agenta dla systemu Windows Server lub klienta systemu Windows** do pobrania *MARSAgentInstaller.exe*. 

    ![Przygotowywanie infrastruktury](./media/tutorial-backup-windows-server-to-azure/prepare-infrastructure.png)

    Instalator otworzy w przeglądarce oddzielne i pliki do pobrania **MARSAgentInstaller.exe**.
 
6.  Przed uruchomieniem pobrany plik, kliknij przycisk **Pobierz** przycisk w bloku Przygotowywanie infrastruktury, aby pobrać i zapisać **poświadczenia magazynu** pliku. Ten plik jest wymagany do połączenia agenta MARS z magazynu usług odzyskiwania.

    ![Przygotowywanie infrastruktury](./media/tutorial-backup-windows-server-to-azure/download-vault-credentials.png)
 
## <a name="install-and-register-the-agent"></a>Instalowanie i rejestrowanie agenta

1. Zlokalizuj i kliknij dwukrotnie pobrany **MARSagentinstaller.exe**.
2. **Kreatora instalacji agenta usług odzyskiwania Microsoft Azure** pojawi się. Podczas wykonywania kroków kreatora, należy podać następujące informacje, gdy zostanie wyświetlony monit i kliknij przycisk **zarejestrować**.
    - Lokalizacja folderu instalacji i pamięci podręcznej.
    - Informacje o serwerze proxy, jeśli używasz serwera proxy do łączenia się z Internetem.
    - Użytkownik nazwy i hasła szczegóły korzystania z uwierzytelnionego serwera proxy.

    ![Przygotowywanie infrastruktury](./media/tutorial-backup-windows-server-to-azure/mars-installer.png) 

3. Po zakończeniu kreatora, kliknij przycisk **przejść do rejestracji** i podaj **poświadczenia magazynu** pliku pobranego w poprzedniej procedurze.
 
4. Po wyświetleniu monitu podaj hasło szyfrowania do szyfrowania kopii zapasowych z systemu Windows Server. Zapisz hasło w bezpiecznej lokalizacji, jak Microsoft nie można odzyskać hasło, w przypadku utraty.

5. Kliknij przycisk **Zakończ**. 

## <a name="configure-backup-and-retention"></a>Konfigurowanie kopii zapasowej i przechowywania

Agent usług odzyskiwania Microsoft Azure umożliwia planowanie kopii zapasowych na platformie Azure, występują w systemie Windows Server. Wykonaj następujące czynności na serwerze, gdzie został pobrany agenta.

1. Otwórz agenta usługi Microsoft Azure Recovery Services. Aby go znaleźć, wyszukaj na maszynie łańcuch **Microsoft Azure Backup**.

2.  W konsoli agenta usług odzyskiwania kliknij **harmonogram tworzenia kopii zapasowych** w obszarze **okienka Akcje**.

    ![Przygotowywanie infrastruktury](./media/tutorial-backup-windows-server-to-azure/mars-schedule-backup.png)

3. Kliknij przycisk **dalej** można przejść do **wybierz elementy do kopii zapasowej** strony.

4. Kliknij przycisk **Dodaj elementy** z wyświetlonym oknie dialogowym wybierz **stanu systemu** i pliki lub foldery, które chcesz utworzyć kopię zapasową. Następnie kliknij przycisk **OK**.

5. Kliknij przycisk **Dalej**.

6. Na **Określanie harmonogramu tworzenia kopii zapasowej (stan systemu)** określić godzinę dnia lub tygodnia, gdy kopie zapasowe trzeba być wywołany dla stanu systemu, a następnie kliknij pozycję **dalej** 

7.  Na **Wybieranie zasady przechowywania (stan systemu)** , wybierz zasady przechowywania dla kopii zapasowej stanu systemu i kliknij przycisk **dalej**
8. Podobnie wybierz zasady harmonogram i przechowywania kopii zapasowej dla wybranych plików i folderów. 
8.  Na **wybierz początkowa kopia zapasowa typu** pozostaw opcję **automatycznie przez sieć** zaznaczone, a następnie kliknij przycisk **dalej**.
9.  Na **potwierdzenie** , przejrzyj informacje, a następnie kliknij przycisk **Zakończ**.
10. Po ukończeniu harmonogramu tworzenia kopii zapasowej przez kreatora kliknij przycisk **Zamknij**.

## <a name="perform-an-ad-hoc-back-up"></a>Wykonaj kopię zapasową ad hoc

Uruchomienie zadania tworzenia kopii zapasowej zostało ustanowione harmonogramu. Jednak nie ma kopii zapasowej serwera. Go awaryjnego odzyskiwania najlepszym rozwiązaniem jest uruchamianie tworzenia kopii zapasowej na żądanie w celu zapewnienia odporności danych serwera.

1.  W konsoli agenta usług odzyskiwania Microsoft Azure, kliknij polecenie **wykonaj kopię zapasową teraz**.

    ![Przygotowywanie infrastruktury](./media/tutorial-backup-windows-server-to-azure/backup-now.png)

2.  Na **wykonaj kopię zapasową teraz** kreatora, wybierz jedną z **pliki i foldery** lub **stanu systemu** , który chcesz utworzyć kopię zapasową, a następnie kliknij przycisk **dalej** 
3. Na **potwierdzenie** Przejrzyj ustawienia, które **wykonaj kopię zapasową teraz** kreator używa do tworzenia kopii zapasowej serwera. Następnie kliknij pozycję **Utwórz kopię zapasową**.
4.  Kliknij przycisk **Zamknij**, aby zamknąć kreatora. Jeśli Kreator zostanie zamknięty przed zakończeniem plik kopii zapasowej procesu, Kreator będzie nadal uruchomione w tle.
4.  Po zakończeniu tworzenia początkowej kopii zapasowej **zadanie zostało ukończone** stan jest wyświetlany w **zadania** okienku konsoli agenta MARS.


## <a name="next-steps"></a>Następne kroki

W tym samouczku użyto portalu Azure, aby: 
 
> [!div class="checklist"] 
> * Tworzenie magazynu Usług odzyskiwania 
> * Pobierz agenta usług odzyskiwania Microsoft Azure 
> * Zainstaluj agenta 
> * Skonfigurowanie usługi Kopia zapasowa systemu Windows Server 
> * Wykonaj kopię zapasową na żądanie 

Następny samouczek, aby odzyskać pliki z platformy Azure do systemu Windows Server w dalszym ciągu

> [!div class="nextstepaction"] 
> [Przywróć pliki z platformy Azure do systemu Windows Server](./tutorial-backup-restore-files-windows-server.md) 

