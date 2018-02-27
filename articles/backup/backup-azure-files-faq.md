---
title: "Tworzenie kopii zapasowej plików w usłudze Azure Files — często zadawane pytania"
description: "Ten artykuł zawiera szczegółowe informacje dotyczące ochrony plików w usłudze Azure Files. To streszczenie jest wyświetlane w wynikach wyszukiwania."
services: backup
keywords: "Nie dodawaj ani nie edytuj słów kluczowych bez konsultacji z ekspertem SEO."
author: markgalioto
ms.author: markgal
ms.date: 2/21/2018
ms.topic: tutorial
ms.service: backup
ms.workload: storage-backup-recovery
manager: carmonm
ms.openlocfilehash: d37e119709bc9d4643fcaa9512b5209d4139515e
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="questions-about-backing-up-azure-files"></a>Pytania dotyczące tworzenia kopii zapasowej plików w usłudze Azure Files
Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące tworzenia kopii zapasowej plików usługi Azure Files. W niektórych odpowiedziach znajdują się linki do artykułów zawierających szczegółowe informacje. Pytania dotyczące usługi Azure Backup można również zadawać na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

Aby szybko przeskanować sekcje znajdujące się w tym artykule, użyj linków z prawej strony w obszarze **W tym artykule**.

## <a name="configuring-the-backup-job-for-azure-files"></a>Konfigurowanie zadania tworzenia kopii zapasowej dla usługi Azure Files

### <a name="why-cant-i-see-some-of-my-storage-accounts-i-want-to-protect-that-contain-valid-file-shares-br"></a>Dlaczego nie widzę niektórych kont magazynu, które chcę chronić, zawierających prawidłowe udziały plików? <br/>
Usługa Azure Files Backup jest obecnie dostępna w wersji zapoznawczej i umożliwia skonfigurowanie tworzenia kopii zapasowej tylko w przypadku obsługiwanych kont magazynu. Upewnij się, że konto magazynu, którego szukasz, jest obsługiwane.

### <a name="why-cant-i-see-some-of-my-file-shares-in-the-storage-account-when-im-trying-to-configure-backup-br"></a>Gdy próbuję skonfigurować kopię zapasową, nie widzę niektórych udziałów plików na koncie magazynu. Dlaczego? <br/>
Sprawdź, czy udział plików nie jest już chroniony w tym samym magazynie usługi Recovery Services. Upewnij się, że udział plików, który chcesz chronić, nie został ostatnio usunięty.

### <a name="why-cant-i-protect-file-shares-connected-to-a-sync-group-in-azure-files-sync-br"></a>Dlaczego nie mogę chronić udziałów plików połączonych z grupą synchronizacji w usłudze Azure Files Sync? <br/>
Ochrona udziałów plików platformy Azure połączonych z grupami synchronizacji jest dostępna w ograniczonej wersji zapoznawczej. Napisz na adres [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com), podając identyfikator subskrypcji, i poproś o dostęp. 

### <a name="in-which-geos-can-i-back-up-azure-file-shares-br"></a>W których obszarach geograficznych można tworzyć kopie zapasowe udziałów plików usługi Azure Files? <br/>
Tworzenie kopii zapasowej udziałów plików usługi Azure Files jest obecnie dostępne w wersji zapoznawczej i jest możliwe tylko w następujących obszarach geograficznych. 
-   Kanada Środkowa (CNC)
-   Kanada Wschodnia (CE) 
-   Środkowe stany USA (CUS)
-   Azja Wschodnia (EA)
-   Australia Wschodnia (AE) 
-   Indie Środkowe (INC) 
-   Północno-środkowe stany USA (NCUS) 
-   Południowe Zjednoczone Królestwo (UKS) 
-   Zachodnie Zjednoczone Królestwo (UKW) 
-   Zachodnio-środkowe stany USA (WCUS)
-   Zachodnie stany USA 2 (WUS 2)

Tworzenie kopii zapasowej udziałów plików usługi Azure Files będzie dostępne w następujących obszarach geograficznych od *23 lutego*.
-   Australia Południowo-Wschodnia (ASE) 
-   Brazylia Południowa (BRS) 
-   Wschodnie stany USA (EUS) 
-   Wschodnie stany USA 2 (EUS2) 
-   Europa Północna (NE) 
-   Południowo-środkowe stany USA (SCUS) 
-   Azja Południowo-Wschodnia (SEA)
-   Europa Zachodnia (WE) 
-   Zachodnie stany USA (WUS)  

Napisz na adres [AskAzureBackupTeam@microsoft.com](email:askazurebackupteam@microsoft.com) jeśli chcesz używać tej usługi w określonym obszarze geograficznym, którego nie ma na powyższej liście.

### <a name="how-many-file-shares-can-i-protect-in-a-vaultbr"></a>Ile udziałów plików można chronić w magazynie?<br/>
W wersji zapoznawczej można chronić w jednym magazynie udziały plików znajdujące się na maksymalnie 25 kontach magazynu. Maksymalna liczba udziałów plików w jednym magazynie wynosi 200. 

## <a name="backup"></a>Backup

### <a name="how-many-on-demand-backups-can-i-take-per-file-share-br"></a>Ile kopii zapasowych na żądanie można utworzyć na jeden udział plików? <br/>
W danym momencie można mieć maksymalnie 200 migawek na jeden udział plików, włącznie z tymi wykonywanymi przez usługę Azure Backup na podstawie zasad. Jeśli tworzenie kopii zapasowych będzie kończyć się niepowodzeniem z powodu osiągnięcia tego limitu, należy usunąć punkty przywracania utworzone na żądanie.

### <a name="after-enabling-virtual-networks-on-my-storage-account-the-backup-of-file-shares-in-the-account-started-failing-why"></a>Po włączeniu sieci wirtualnych na koncie magazynu tworzenie kopii zapasowej udziału plików na koncie kończy się niepowodzeniem. Dlaczego?
Kopie zapasowe usługi Azure Files nie są obecnie obsługiwane w przypadku kont magazynu, które mają włączone sieci wirtualne. Wyłącz sieci wirtualne na kontach magazynu, których kopię zapasową chcesz utworzyć. 

## <a name="restore"></a>Przywracanie

### <a name="can-i-recover-from-a-deleted-file-share-br"></a>Czy można odzyskać dane z usuniętego udziału plików? <br/>
Podczas próby usunięcia udziału plików zostanie wyświetlona lista kopii zapasowych, które również zostaną usunięte, jeśli przejdziesz dalej, oraz monit o potwierdzenie. Przywrócenie danych z usuniętego udziału plików jest niemożliwe.

### <a name="can-i-restore-from-backups-if-i-stopped-protection-on-a-file-share-br"></a>Czy można przywrócić dane z kopii zapasowych, jeśli zatrzymano ochronę udziału plików? <br/>
Tak. Jeśli podczas zatrzymywania ochrony została wybrana opcja **Zachowaj dane kopii zapasowej**, można przywrócić dane z wszystkich istniejących punktów przywracania.

## <a name="manage-backup"></a>Zarządzanie kopiami zapasowymi

### <a name="can-i-access-the-snapshots-taken-by-azure-backups-and-mount-it-br"></a>Czy mogę uzyskać dostęp do migawek wykonywanych przez usługę Azure Backup i zainstalować je? <br/>
Można uzyskać dostęp do wszystkich migawek wykonanych przez usługę Azure Backup, wyświetlając migawki w portalu, w programie PowerShell lub przy użyciu interfejsu wiersza polecenia. Można zainstalować je przy użyciu opisanej tutaj procedury.

### <a name="what-is-the-maximum-retention-i-can-configure-for-backups-br"></a>Jaki jest maksymalny czas przechowywania, który mogę skonfigurować dla kopii zapasowych? <br/>
Kopie zapasowe udziałów plików usługi Azure Files tworzone codziennie można przechowywać przez maksymalnie 120 dni.

### <a name="what-happens-when-i-change-the-backup-policy-for-a-file-share-br"></a>Co się stanie, jeśli zmienię zasady tworzenia kopii zapasowych udziału plików? <br/>
Po zastosowaniu nowych zasad do udziału plików będzie obowiązywał harmonogram i okres przechowywania określony przez te nowe zasady. Jeśli okres przechowywania zostanie przedłużony, istniejące punkty odzyskiwania zostaną oznaczone, aby przechowywać je zgodnie z nowymi zasadami. W przypadku skrócenia okresu przechowywania zostaną one oznaczone do oczyszczenia w ramach następnego zadania oczyszczania, a następnie usunięte.


## <a name="see-also"></a>Zobacz też
Te informacje dotyczą wyłącznie tworzenia kopii zapasowych plików usługi Azure Files. Aby dowiedzieć się więcej na temat innych obszarów usługi Azure Backup, zobacz inne najczęściej zadawane pytania dotyczące tworzenia kopii zapasowych:
-  [Recovery Services vault FAQ (Magazyn usługi Recovery Services — często zadawane pytania)](backup-azure-backup-faq.md)
-  [Azure VM backup FAQ (Tworzenie kopii zapasowych maszyn wirtualnych — często zadawane pytania)](backup-azure-vm-backup-faq.md)
-  [Azure Backup agent FAQ (Agent usługi Azure Backup — często zadawane pytania)](backup-azure-file-folder-backup-faq.md)
