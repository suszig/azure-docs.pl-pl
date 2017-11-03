---
title: "Kopia zapasowa maszyny wirtualnej platformy Azure — często zadawane pytania | Microsoft Docs"
description: "Odpowiedzi na często zadawane pytania dotyczące działania funkcji tworzenia kopii zapasowej maszyny wirtualnej platformy Azure, ograniczeń i konsekwencji zmian zasad"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: azure vm backup, azure vm restore, backup policy
ms.assetid: c4cd7ff6-8206-45a3-adf5-787f64dbd7e1
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: d3178413e894c095235dde067b369e3554375aa6
ms.sourcegitcommit: 804db51744e24dca10f06a89fe950ddad8b6a22d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/30/2017
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Pytania dotyczące usługi tworzenia kopii zapasowej maszyny wirtualnej platformy Azure
W tym artykule znajdują się odpowiedzi na często zadawane pytania pomagające w szybkim poznaniu składników usługi tworzenia kopii zapasowej maszyny wirtualnej platformy Azure. W niektórych odpowiedziach znajdują się linki do artykułów zawierających szczegółowe informacje. Pytania dotyczące usługi Azure Backup można również zadawać na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurowanie kopii zapasowych
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Czy magazyny usługi Recovery Services obsługują klasyczne maszyny wirtualne, czy maszyny wirtualne oparte na usłudze Resource Manager? <br/>
Magazyny usługi Recovery Services obsługują oba modele.  Możliwe jest utworzenie kopii zapasowej klasycznej maszyny wirtualnej (utworzonej w portalu klasycznym) lub maszyny wirtualnej usługi Resource Manager (utworzonej w witrynie Azure Portal) w magazynie usługi Recovery Services.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>Jakie konfiguracje nie są obsługiwane przez funkcję kopia zapasowa maszyny Wirtualnej platformy Azure?
Przejdź przez [obsługiwanych systemów operacyjnych](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) i [kopii zapasowej ograniczenia maszyny Wirtualnej](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Dlaczego nie widzę swojej maszyny wirtualnej w kreatorze konfigurowania kopii zapasowych?
W kreatorze tworzenia kopii zapasowej Konfigurowanie usługi Kopia zapasowa Azure znajduje się tylko maszyny wirtualne, które są:
  * Nie jest już chroniona można zweryfikować stanu kopii zapasowej maszyny wirtualnej, przechodząc do bloku maszyny Wirtualnej i sprawdzanie stanu kopii zapasowej z Menu ustawień. Dowiedz się więcej na temat [sprawdzania stanu tworzenia kopii zapasowych maszyny wirtualnej](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade)
  * Przynależność do tego samego regionu co maszyna wirtualna

## <a name="backup"></a>Tworzenie kopii zapasowych
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Czy dla zadań tworzenia kopii zapasowej na żądanie będzie stosowany ten sam harmonogram przechowywania co dla zaplanowanych zadań tworzenia kopii zapasowej?
Nie. Należy określić zakres przechowywania dla zadania tworzenia kopii zapasowej na żądanie. Domyślnie jest przechowywane przez 30 dni po wyzwoleniu z portalu. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Ostatnio na niektórych maszynach wirtualnych została włączona usługa Azure Disk Encryption. Czy operacje tworzenia kopii zapasowych będą nadal działać?
Musisz nadać usłudze Azure Backup uprawnienia umożliwiające jej uzyskiwanie dostępu do usługi Key Vault. Te uprawnienia możesz podać w programie PowerShell przy użyciu czynności wymienionych w sekcji *Włączenie kopii zapasowej* dokumentacji programu [PowerShell](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Dyski maszyny wirtualnej zostały zmigrowane do dysków zarządzanych. Czy operacje tworzenia kopii zapasowych będą nadal działać?
Tak, kopie zapasowe działają bezproblemowo i nie trzeba ponownie skonfigurować tworzenia kopii zapasowej. 

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Moja maszyna wirtualna jest zamknięta. Będą na żądanie lub zaplanowanych zadań tworzenia kopii zapasowej?
Tak. Nawet w przypadku, gdy maszyna zostanie zamknięta wykonywania kopii zapasowych i punkt odzyskiwania jest oznaczony jako awarii spójne. Aby uzyskać więcej informacji, zobacz sekcję spójności danych w [w tym artykule](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

## <a name="restore"></a>Przywracanie
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Jak wybrać między przywróceniem dysków a pełnym przywróceniem maszyny wirtualnej?
Azure pełne przywracanie maszyny Wirtualnej można traktować jako opcja szybkiego tworzenia. Przywrócenie maszyny Wirtualnej opcja zmiany nazwy dysków pojemnikach używane przez te dyski, publicznych adresów IP i nazw interfejsów sieciowych. Zmiana jest wymagany do zachowania unikatowości zasoby utworzone podczas tworzenia maszyny Wirtualnej. Ale nie doda maszyny Wirtualnej do zestawu dostępności. 

Opcji przywracania dysków można używać w następujących celach:
  * Dostosowywanie maszyny Wirtualnej, który jest tworzony z punktu w czasie konfiguracji jak zmiana rozmiaru
  * Dodawanie konfiguracji, które nie są dostępne w czasie wykonywania kopii zapasowej 
  * Kontrolowanie konwencji nazewnictwa tworzonych zasobów
  * Dodawanie maszyny wirtualnej do zestawu dostępności
  * Dla dowolnej innej konfiguracji można uzyskać tylko za pomocą programu PowerShell/a declarative szablonu definicji
  
### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>Kopie zapasowe niezarządzane dysku maszyny Wirtualnej mogą służyć do przywrócenia po uaktualnieniu dysków do zarządzanych dysków?
Tak, można użyć kopie zapasowe wykonane przed Migrowanie dysków z niezarządzanych do zarządzanych. Domyślnie zadanie przywracania maszyny Wirtualnej spowoduje utworzenie maszyny Wirtualnej z dyskami niezarządzane. Przywracanie dysków funkcji można używać do przywrócenia dysków i używać ich do tworzenia maszyny Wirtualnej na dyskach zarządzanych. 

## <a name="manage-vm-backups"></a>Zarządzanie kopiami zapasowymi maszyn wirtualnych
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Co się stanie po zmianie zasad kopii zapasowych na maszynach wirtualnych?
Podczas stosowania nowej zasady na wirtualne, harmonogram i przechowywania nowych zasad jest zakończony. Jeśli przechowywania zostanie rozszerzony, istniejące punkty odzyskiwania są oznaczone zachować je zgodnie z harmonogramem nowych zasad. W przypadku przechowywania, są oznaczone do oczyszczania w ramach następnego zadania oczyszczania i usuwane. 
