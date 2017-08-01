
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
ms.topic: get-started-article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.translationtype: HT
ms.sourcegitcommit: 0425da20f3f0abcfa3ed5c04cec32184210546bb
ms.openlocfilehash: 8c06a90f58cf56ebb4e75e7567e237de7414a300
ms.contentlocale: pl-pl
ms.lasthandoff: 07/20/2017

---
# <a name="questions-about-the-azure-vm-backup-service"></a>Pytania dotyczące usługi tworzenia kopii zapasowej maszyny wirtualnej platformy Azure
W tym artykule znajdują się odpowiedzi na często zadawane pytania pomagające w szybkim poznaniu składników usługi tworzenia kopii zapasowej maszyny wirtualnej platformy Azure. W niektórych odpowiedziach znajdują się linki do artykułów zawierających szczegółowe informacje. Pytania dotyczące usługi Azure Backup można również zadawać na [forum dyskusyjnym](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Konfigurowanie kopii zapasowych
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Czy magazyny usługi Recovery Services obsługują klasyczne maszyny wirtualne, czy maszyny wirtualne oparte na usłudze Resource Manager? <br/>
Magazyny usługi Recovery Services obsługują oba modele.  Możliwe jest utworzenie kopii zapasowej klasycznej maszyny wirtualnej (utworzonej w portalu klasycznym) lub maszyny wirtualnej usługi Resource Manager (utworzonej w witrynie Azure Portal) w magazynie usługi Recovery Services.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup-"></a>Jakie konfiguracje nie są obsługiwane przez funkcję tworzenia kopii zapasowej maszyny wirtualnej platformy Azure?
Zapoznaj się z informacjami w tematach [Obsługiwane systemy operacyjne](backup-azure-arm-vms-prepare.md#supported-operating-system-for-backup) i [Limitations of VM backup (Ograniczenia tworzenia kopii zapasowej maszyny wirtualnej)](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm)

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>Dlaczego nie widzę swojej maszyny wirtualnej w kreatorze konfigurowania kopii zapasowych?
W kreatorze konfigurowania kopii zapasowych usługi Azure Backup na liście są wyświetlane tylko maszyny wirtualne spełniające następujące warunki:
* Brak ochrony — stan tworzenia kopii zapasowych maszyny wirtualnej można zweryfikować, przechodząc do bloku maszyny wirtualnej i sprawdzając stan Kopia zapasowa w menu Ustawienia bloku. Dowiedz się więcej na temat [sprawdzania stanu tworzenia kopii zapasowych maszyny wirtualnej](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-management-blade)
* Przynależność do tego samego regionu co maszyna wirtualna

## <a name="backup"></a>Tworzenie kopii zapasowych
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>Czy dla zadań tworzenia kopii zapasowej na żądanie będzie stosowany ten sam harmonogram przechowywania co dla zaplanowanych zadań tworzenia kopii zapasowej?
Nie. Należy określić zakres przechowywania dla zadania tworzenia kopii zapasowej na żądanie. Domyślnie dane będą przechowywane przez 30 dni po wyzwoleniu z portalu. 

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Ostatnio na niektórych maszynach wirtualnych została włączona usługa Azure Disk Encryption. Czy operacje tworzenia kopii zapasowych będą nadal działać?
Musisz nadać usłudze Azure Backup uprawnienia umożliwiające jej uzyskiwanie dostępu do usługi Key Vault. Te uprawnienia możesz podać w programie PowerShell przy użyciu czynności wymienionych w sekcji *Włączenie kopii zapasowej* dokumentacji programu [PowerShell](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Dyski maszyny wirtualnej zostały zmigrowane do dysków zarządzanych. Czy operacje tworzenia kopii zapasowych będą nadal działać?
Tak, kopie zapasowe będą tworzone bez problemów. Nie trzeba ich ponownie konfigurować. 

## <a name="restore"></a>Przywracanie
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>Jak wybrać między przywróceniem dysków a pełnym przywróceniem maszyny wirtualnej?
Pełne przywracanie maszyny wirtualnej na platformie Azure można traktować jak opcję szybkiego utworzenia przywracanej maszyny wirtualnej. Użycie opcji przywrócenia maszyny wirtualnej spowoduje zmianę nazw dysków, kontenerów używanych przez dyski, publicznych adresów IP oraz interfejsów sieciowych, aby zapewnić unikatowość zasobów tworzonych podczas tworzenia maszyny wirtualnej. Dodatkowo w takim przypadku maszyna wirtualna nie zostanie dodana do zestawu dostępności. 

Opcji przywracania dysków można używać w następujących celach:
* Dostosowanie utworzonej maszyny wirtualnej na podstawie konfiguracji z punktu w czasie, np. zmiana rozmiaru na podstawie konfiguracji kopii zapasowej
* Dodawanie konfiguracji, które nie są obecne w momencie tworzenia kopii zapasowej 
* Kontrolowanie konwencji nazewnictwa tworzonych zasobów
* Dodawanie maszyny wirtualnej do zestawu dostępności
* Korzystasz z konfiguracji, którą można uzyskać jedynie przy użyciu programu PowerShell lub definicji deklaracyjnego szablonu

## <a name="manage-vm-backups"></a>Zarządzanie kopiami zapasowymi maszyn wirtualnych
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>Co się stanie po zmianie zasad kopii zapasowych na maszynach wirtualnych?
Po zastosowaniu nowych zasad dla maszyn wirtualnych będzie stosowany harmonogram i okres przechowywania z nowych zasad. Jeśli okres przechowywania zostanie przedłużony, istniejące punkty odzyskiwania zostaną oznaczone, aby zachować je zgodnie z nowymi zasadami. W przypadku skrócenia okresu przechowywania zostaną one oznaczone do oczyszczenia w ramach następnego zadania oczyszczania i zostaną usunięte. 

