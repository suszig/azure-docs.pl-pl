---
title: Wykonywanie kopii zapasowych maszyn wirtualnych platformy Azure | Dokumentacja firmy Microsoft
description: "Odnajdowanie, rejestrowania i utworzyć kopię zapasową maszyn wirtualnych platformy Azure w magazynie usług odzyskiwania."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: kopii zapasowej maszyny wirtualnej. Tworzenie kopii zapasowej maszyny wirtualnej; Kopia zapasowa i odzyskiwanie po awarii; ARM kopii zapasowej maszyny wirtualnej
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 892a88a2bc9d492f8a3afe59c05b4729f4830e6d
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/19/2017
---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a>Tworzenie kopii zapasowych maszyn wirtualnych w magazynie usługi Recovery Services

Ten artykuł zawiera szczegóły dotyczące tworzenia kopii zapasowych maszyn wirtualnych platformy Azure (wdrożone usługi Resource Manager i wdrożone w klasycznej) w magazynie usług odzyskiwania. Większość zadań tworzenia kopii zapasowych maszyn wirtualnych jest przygotowania. Zanim można utworzyć kopię zapasową lub ochrony maszyny Wirtualnej, należy wykonać [wymagania wstępne](backup-azure-arm-vms-prepare.md) Aby przygotować swoje środowisko do ochrony maszyn wirtualnych. Po zakończeniu wymagań wstępnych, można zainicjować operacji tworzenia kopii zapasowej, aby wykonać migawki maszyny Wirtualnej.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Aby uzyskać więcej informacji, zobacz artykuły w [planowania infrastruktury kopii zapasowych maszyn wirtualnych na platformie Azure](backup-azure-vms-introduction.md) i [maszyn wirtualnych platformy Azure](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-the-backup-job"></a>Wyzwolenie zadanie tworzenia kopii zapasowej
Zasady tworzenia kopii zapasowej skojarzonego z magazynu usług odzyskiwania Określa, kiedy i jak często uruchamia tworzenie kopii zapasowej. Domyślnie pierwszą zaplanowaną kopią zapasową jest początkowa kopia zapasowa. Do czasu uruchomienia procesu tworzenia początkowej kopii zapasowej w polu Stan ostatniej kopii zapasowej w bloku **Zadania tworzenia kopii zapasowej** wyświetlany jest komunikat **Ostrzeżenie (oczekiwanie na początkową kopię zapasową)**.

![Oczekiwanie na kopię zapasową](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Jeśli tworzenie początkowej kopii zapasowej nie ma rozpocząć się wkrótce, zalecane jest wykonanie akcji **Wykonaj kopię zapasową teraz**. Poniższa procedura rozpoczyna się na pulpicie nawigacyjnym magazynu. Ta procedura służy do uruchamiania początkowej zadanie tworzenia kopii zapasowej po zakończeniu wszystkie wymagania wstępne. Początkowe zadanie tworzenia kopii zapasowej został już uruchomiony, ta procedura nie jest dostępna. Skojarzonych zasad tworzenia kopii zapasowej określa następne zadanie tworzenia kopii zapasowej.  

Aby uruchomić zadanie tworzenia początkowej kopii zapasowej:

1. Na pulpicie nawigacyjnym magazynu kliknij liczbę w obszarze **Elementy kopii zapasowej** lub kliknij kafelek **Elementy kopii zapasowej**. <br/>
  ![Ikona ustawień](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Zostanie otwarty blok **Elementy kopii zapasowej**.

  ![Tworzenie kopii zapasowych elementów](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. W bloku **Elementy kopii zapasowej** wybierz element.

  ![Ikona Ustawienia](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Zostanie otwarta lista **Elementy kopii zapasowej**. <br/>

  ![Zadanie tworzenia kopii zapasowej zostało wyzwolone](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. Na liście **Elementy kopii zapasowej** kliknij przycisk z wielokropkiem **...**, aby otworzyć menu Kontekst.

  ![Menu Kontekst](./media/backup-azure-vms-first-look-arm/context-menu.png)

  Zostanie wyświetlone menu Kontekst.

  ![Menu Kontekst](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. W menu Kontekst kliknij pozycję **Utwórz teraz kopię zapasową**.

  ![Menu Kontekst](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  Zostanie otwarty blok Utwórz teraz kopię zapasową.

  ![wyświetlanie bloku Utwórz teraz kopię zapasową](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. W bloku Utwórz teraz kopię zapasową kliknij ikonę kalendarza, wybierz ostatni dzień okresu zachowywania tego punktu odzyskiwania przy użyciu kontrolki kalendarza, a następnie kliknij pozycję **Kopia zapasowa**.

  ![ustawianie ostatniego dnia okresu zachowywania punktu odzyskiwania Utwórz teraz punkt odzyskiwania](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Powiadomienia dotyczące wdrożenia informują o wyzwoleniu zadania tworzenia kopii zapasowej, dzięki czemu możesz monitorować postęp zadania na stronie zadań tworzenia kopii zapasowych. W zależności od rozmiaru maszyny wirtualnej tworzenie początkowej kopii zapasowej może potrwać pewien czas.

6. Aby wyświetlić lub śledzić stan początkowej kopii zapasowej, na pulpicie nawigacyjnym magazynu na kafelku **Zadania tworzenia kopii zapasowej** kliknij pozycję **W toku**.

  ![Kafelek Zadania tworzenia kopii zapasowej](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  Zostanie otwarty blok Zadania tworzenia kopii zapasowej.

  ![Kafelek Zadania tworzenia kopii zapasowej](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  W bloku **Zadania tworzenia kopii zapasowej** można sprawdzić stan wszystkich zadań. Sprawdź, czy zadanie tworzenia kopii zapasowej dla maszyny wirtualnej jest nadal w toku, czy zostało zakończone. Po zakończeniu zadania tworzenia kopii zapasowej stan przyjmuje wartość *Ukończono*.

  > [!NOTE]
  > W ramach operacji tworzenia kopii zapasowej usługa Azure Backup wydaje polecenie rozszerzeniu kopii zapasowej w każdej maszynie wirtualnej powodujące opróżnienie wszystkich zapisów i wykonanie spójnej migawki.
  >
  >

## <a name="troubleshooting-errors"></a>Rozwiązywanie problemów z błędami
W razie problemów podczas wykonywania kopii zapasowej maszyny wirtualnej, zobacz [maszyny Wirtualnej artykuł dotyczący rozwiązywania problemów](backup-azure-vms-troubleshoot.md) Aby uzyskać pomoc.

## <a name="next-steps"></a>Następne kroki
Teraz, ochrony maszyny Wirtualnej, zobacz następujące artykuły, aby dowiedzieć się więcej o zadaniach zarządzania maszyny Wirtualnej i przywracanie maszyn wirtualnych.

* [Monitorowanie maszyn wirtualnych i zarządzanie nimi](backup-azure-manage-vms.md)
* [Przywracanie maszyn wirtualnych](backup-azure-arm-restore-vms.md)
