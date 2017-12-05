---
title: "Odzyskiwanie plików z platformy Azure z systemem Windows Server | Dokumentacja firmy Microsoft"
description: "Ten samouczek zawiera szczegóły Odzyskiwanie elementów z platformy Azure z systemem Windows Server."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
keywords: "systemu Windows server, wykonaj kopię zapasową; Przywracanie plików systemu windows server; wykonywanie kopii zapasowych i odzyskiwania po awarii"
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2017
ms.author: saurabhsensharma;markgal;
ms.custom: mvc
ms.openlocfilehash: b5f77ec04ef6d267583a6dc6a4476f118a4d0f74
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/05/2017
---
# <a name="recover-files-from-azure-to-a-windows-server"></a>Odzyskiwanie plików z platformy Azure z systemem Windows Server

Kopia zapasowa Azure umożliwia odzyskanie poszczególnych elementów kopii zapasowych w systemie Windows Server. Odzyskanie poszczególnych plików jest przydatne, jeśli musi szybko przywrócić pliki, które zostaną przypadkowo usunięte. W tym samouczku opisano, jak agent agenta usług odzyskiwania Microsoft Azure (MARS) umożliwia odzyskanie elementów z kopii zapasowych, zostały już wykonane na platformie Azure. Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Rozpocząć odzyskiwania poszczególne elementy 
> * Wybierz punkt odzyskiwania 
> * Przywracanie elementów z punktu odzyskiwania

Ten samouczek zakłada wykonano już kroki [kopii zapasowych systemu Windows Server na platformie Azure](backup-configure-vault.md) i mieć co najmniej jedną kopię zapasową plików systemu Windows Server na platformie Azure.

## <a name="initiate-recovery-of-individual-items"></a>Rozpocząć odzyskiwania poszczególne elementy

Kreator interfejsu użytkownika pomocne, o nazwie kopia zapasowa Microsoft Azure jest instalowany z agenta usług odzyskiwania Azure firmy Microsoft (MARS). Kreator tworzenia kopii zapasowej Microsoft Azure działa w agencie usług odzyskiwania Azure firmy Microsoft (MARS) można pobrać danych kopii zapasowej z punktów odzyskiwania przechowywanych na platformie Azure. Użyj kreatora Kopia zapasowa Microsoft Azure, aby zidentyfikować pliki lub foldery, które mają być przywrócone do systemu Windows Server. 

1. Otwórz **kopia zapasowa Microsoft Azure** przystawki. Aby go znaleźć, wyszukaj na maszynie łańcuch **Microsoft Azure Backup**.

    ![Oczekiwanie na kopię zapasową](./media/tutorial-backup-restore-files-windows-server/mars.png)

2. W kreatorze kliknij **odzyskać dane** w **okienka Akcje** konsoli agenta, aby uruchomić **odzyskać dane** kreatora.

    ![Oczekiwanie na kopię zapasową](./media/tutorial-backup-restore-files-windows-server/mars-recover-data.png)

3. Na **wprowadzenie** wybierz pozycję **tego serwera (nazwa serwera)** i kliknij przycisk **dalej**.

4. Na **wybierz tryb odzyskiwania** wybierz **poszczególnych plików i folderów** , a następnie kliknij przycisk **dalej** do rozpoczęcia procesu wyboru punktu odzyskiwania.
 
5. Na **Wybierz wolumin i data** Wybierz wolumin, który zawiera pliki lub foldery, które chcesz przywrócić, a następnie kliknij przycisk **instalacji**. Wybierz datę i wybierz godzinę z menu rozwijanego, który odpowiada punkt odzyskiwania. Daty w **bold** wskazać dostępność co najmniej jeden punkt odzyskiwania, w tym dniu.

    ![Oczekiwanie na kopię zapasową](./media/tutorial-backup-restore-files-windows-server/mars-select-date.png)
 
    Po kliknięciu **instalacji**, kopia zapasowa Azure udostępnia punktu odzyskiwania jako dysk. Przeglądanie i odzyskać pliki z dysku.

## <a name="restore-items-from-a-recovery-point"></a>Przywracanie elementów z punktu odzyskiwania

1. Gdy wolumin odzyskiwania zostanie zainstalowany, kliknij przycisk **Przeglądaj** Otwórz Eksploratora Windows i Znajdź pliki i foldery, które chcesz odzyskać. 

    ![Oczekiwanie na kopię zapasową](./media/tutorial-backup-restore-files-windows-server/mars-browse-recover.png)

    Można otworzyć pliki bezpośrednio z odzyskiwania woluminu i zweryfikować pliki.

2. W Eksploratorze Windows skopiuj pliki lub foldery, które chcesz przywrócić, a następnie wklej je do dowolnej żądanej lokalizacji na serwerze.

    ![Oczekiwanie na kopię zapasową](./media/tutorial-backup-restore-files-windows-server/mars-final.png)

3. Po zakończeniu przywracania plików i/lub folderów, na **przeglądania i pliki odzyskiwania** strony **odzyskać dane** kreatora, kliknij przycisk **Odinstaluj**. 

    ![Oczekiwanie na kopię zapasową](./media/tutorial-backup-restore-files-windows-server/unmount-and-confirm.png)

4.  Kliknij przycisk **tak** potwierdzenie dezinstalacji woluminu.

    Gdy migawka zostanie odinstalowane, **zadanie zakończone** pojawia się w **zadania** okienko w konsoli agenta.

## <a name="next-steps"></a>Następne kroki

Na tym kończy się samouczków na wykonywanie kopii zapasowych i przywracanie danych serwera systemu Windows Azure. Aby dowiedzieć się więcej na temat usługi Kopia zapasowa Azure, zobacz przykładowe programu PowerShell do utworzenia kopii zapasowej zaszyfrowanego maszyn wirtualnych.

> [!div class="nextstepaction"]
> [Tworzenie kopii zapasowej zaszyfrowanego maszyny Wirtualnej](./scripts/backup-powershell-sample-backup-encrypted-vm.md)
