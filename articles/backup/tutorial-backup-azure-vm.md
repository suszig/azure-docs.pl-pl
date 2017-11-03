---
title: "Wykonywanie kopii zapasowych maszyn wirtualnych platformy Azure na platformie Azure na dużą skalę | Dokumentacja firmy Microsoft"
description: "Ten samouczek szczegóły wykonywanie kopii zapasowych wielu maszyn wirtualnych platformy Azure w magazynie usług odzyskiwania."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: kopii zapasowej maszyny wirtualnej. Tworzenie kopii zapasowej maszyny wirtualnej; Kopia zapasowa i odzyskiwanie po awarii
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: 
ms.openlocfilehash: db4e1392acaeb2431d29a851113b7bc5a6dc1e9d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="back-up-azure-virtual-machines-in-azure-at-scale"></a>Tworzenie kopii zapasowej maszyn wirtualnych platformy Azure na platformie Azure w dużej skali

Ten samouczek zawiera szczegóły dotyczące tworzenia kopii zapasowych maszyn wirtualnych platformy Azure w magazynie usług odzyskiwania. Większość zadań tworzenia kopii zapasowych maszyn wirtualnych jest przygotowania. Przed utworzeniem kopii zapasowej (lub chronić) maszyny wirtualnej, należy wykonać [wymagania wstępne](backup-azure-arm-vms-prepare.md) Aby przygotować swoje środowisko do ochrony maszyn wirtualnych. 

> [!IMPORTANT]
> W tym samouczku założono, że utworzono już grupę zasobów i maszyny wirtualnej platformy Azure.

## <a name="create-a-recovery-services-vault"></a>Tworzenie magazynu Usług odzyskiwania

A [magazyn usług odzyskiwania](backup-azure-recovery-services-vault-overview.md) jest kontenerem, który przechowuje punkty odzyskiwania dla elementów, których powstaje kopia zapasowa. Magazyn usług odzyskiwania jest zasobem platformy Azure, które mogą być wdrożone i zarządzane w ramach grupy zasobów platformy Azure. W tym samouczku utworzysz magazyn usług odzyskiwania w tej samej grupie zasobów co chroniona maszyna wirtualna.


Po raz pierwszy używasz kopia zapasowa Azure, należy zarejestrować dostawcę usług odzyskiwania Azure w ramach subskrypcji. Jeśli dostawca został już zarejestrowany w ramach subskrypcji, przejdź do następnego kroku.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices
```

Utwórz magazyn usługi Recovery Services za pomocą polecenia **New-AzureRmRecoveryServicesVault**. Należy określić nazwę grupy zasobów i lokalizacja używana podczas konfigurowania maszyny wirtualnej, który chcesz utworzyć kopię zapasową. 

```powershell
New-AzureRmRecoveryServicesVault -Name myRSvault -ResourceGroupName "myResourceGroup" -Location "EastUS"
```

Wiele poleceń cmdlet narzędzia Kopia zapasowa Azure wymaga obiektu magazynu usług odzyskiwania jako danych wejściowych. Z tego powodu jest wygodne do przechowywania obiektów magazynu usług odzyskiwania kopii zapasowej w zmiennej. Następnie użyj **AzureRmRecoveryServicesBackupProperties zestaw** można ustawić **- BackupStorageRedundancy** opcji w celu [magazynu geograficznie nadmiarowego (GRS)](../storage/common/storage-redundancy.md#geo-redundant-storage). 

```powershell
$vault1 = Get-AzureRmRecoveryServicesVault –Name myRSVault
Set-AzureRmRecoveryServicesBackupProperties  -vault $vault1 -BackupStorageRedundancy GeoRedundant
```

## <a name="back-up-azure-virtual-machines"></a>Tworzenie kopii zapasowych maszyn wirtualnych platformy Azure

Przed rozpoczęciem tworzenia początkowej kopii zapasowej, należy ustawić kontekst magazynu. Kontekst magazynu jest typ danych chronionych w magazynie. Podczas tworzenia magazynu usług odzyskiwania jest dostarczany z domyślną ochronę i zasady przechowywania. Domyślne zasady ochrony wyzwala zadanie tworzenia kopii zapasowej każdego dnia o określonej godzinie. Domyślne zasady przechowywania zachowuje punkt odzyskiwania codziennie przez 30 dni. W tym samouczku Zaakceptuj domyślne zasady. 

Użyj  **[AzureRmRecoveryServicesVaultContext zestaw](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)**  można ustawić kontekstu magazynu. Po ustawieniu kontekst magazynu ma zastosowanie do wszystkich kolejnych poleceń cmdlet. 

```powershell
Get-AzureRmRecoveryServicesVault -Name myRSVault | Set-AzureRmRecoveryServicesVaultContext
```

Użyj  **[AzureRmRecoveryServicesBackupItem kopii zapasowej](https://docs.microsoft.com/powershell/module/azurerm.recoveryservices.backup/backup-azurermrecoveryservicesbackupitem)**  do wyzwalania zadania tworzenia kopii zapasowej. Zadanie tworzenia kopii zapasowej tworzy punkt odzyskiwania. Jeśli jest początkowa kopia zapasowa, punkt odzyskiwania jest pełna kopia zapasowa. Tworzenie kolejnych kopii zapasowych tworzenia przyrostowych kopii.

```powershell
$namedContainer = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered -FriendlyName "V2VM"
$item = Get-AzureRmRecoveryServicesBackupItem -Container $namedContainer -WorkloadType AzureVM
$job = Backup-AzureRmRecoveryServicesBackupItem -Item $item
```

## <a name="delete-the-recovery-services-vault"></a>Usuwanie magazynu usług odzyskiwania

Aby usunąć magazyn usług odzyskiwania, musi najpierw usunąć punktów odzyskiwania w magazynie, a następnie wyrejestrowanie magazynu. Następujące polecenia szczegółowo opisano następujące kroki. 


```powershell
$Cont = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureVM -Status Registered
$PI = Get-AzureRmRecoveryServicesBackupItem -Container $Cont[0] -WorkloadType AzureVm
Disable-AzureRmRecoveryServicesBackupProtection -RemoveRecoveryPoints $PI[0]
Unregister-AzureRmRecoveryServicesBackupContainer -Container $namedContainer
Remove-AzureRmRecoveryServicesVault -Vault $vault1
```

## <a name="troubleshooting-errors"></a>Rozwiązywanie problemów z błędami
W razie problemów podczas wykonywania kopii zapasowej maszyny wirtualnej, zobacz [kopii zapasowych maszyn wirtualnych platformy Azure Rozwiązywanie problemów z artykułu](backup-azure-vms-troubleshoot.md) Aby uzyskać pomoc.

## <a name="next-steps"></a>Następne kroki
Teraz, został zabezpieczony maszyn wirtualnych, zobacz następujące artykuły, aby dowiedzieć się więcej o zadaniach zarządzania i przywracanie maszyn wirtualnych z punktu odzyskiwania.

* Aby zmodyfikować zasady tworzenia kopii zapasowych, zobacz [AzureRM.RecoveryServices.Backup użyj poleceń cmdlet narzędzia do tworzenia kopii zapasowych maszyn wirtualnych](backup-azure-vms-automation.md#create-a-protection-policy).
* [Monitorowanie maszyn wirtualnych i zarządzanie nimi](backup-azure-manage-vms.md)
* [Przywracanie maszyn wirtualnych](backup-azure-arm-restore-vms.md)
