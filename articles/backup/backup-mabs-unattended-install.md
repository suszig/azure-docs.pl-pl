---
title: Podczas instalacji dyskretnej serwer kopii zapasowej Azure w wersji 2 | Dokumentacja firmy Microsoft
description: "Za pomocą skryptu programu PowerShell do przeprowadzenia instalacji dyskretnej v2 serwer kopii zapasowej Azure. Ten rodzaj instalacji jest również nazywany instalacji nienadzorowanej."
services: backup
documentationcenter: " "
author: markgalioto
manager: carmonm
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/30/2017
ms.author: markgal;masaran
ms.openlocfilehash: 91778a67f9ef523aa87b7918197e0d0ded0f5702
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="run-an-unattended-installation-of-azure-backup-server-v2"></a>Uruchamianie nienadzorowanej instalacji serwera usługi Kopia zapasowa Azure w wersji 2

Dowiedz się, jak uruchomić nienadzorowaną instalację serwera usługi Kopia zapasowa Azure w wersji 2. 

Te kroki nie są stosowane, jeśli instalujesz serwer kopii zapasowej Azure w wersji 1.

## <a name="install-backup-server-v2"></a>Zainstaluj serwer zapasowy v2

1. Na serwerze, który jest hostem serwera usługi Kopia zapasowa Azure w wersji 2 Utwórz plik tekstowy. (Można utworzyć pliku, w Notatniku lub w innym edytorze tekstów.) Zapisz plik jako MABSSetup.ini. 

2. Wklej następujący kod w pliku MABSSetup.ini. Zamień tekst w nawiasie (\< \>) z wartościami z używanego środowiska. Następujący tekst jest przykładem:

  ```
  [OPTIONS]
  UserName=administrator
  CompanyName=<Microsoft Corporation>
  SQLMachineName=localhost
  SQLInstanceName=<SQL instance name>
  SQLMachineUserName=administrator
  SQLMachinePassword=<admin password>
  SQLMachineDomainName=<machine domain>
  ReportingMachineName=localhost
  ReportingInstanceName=<reporting instance name>
  SqlAccountPassword=<admin password>
  ReportingMachineUserName=<username>
  ReportingMachinePassword=<reporting admin password>
  ReportingMachineDomainName=<domain>
  VaultCredentialFilePath=<vault credential full path and complete name>
  SecurityPassphrase=<passphrase>
  PassphraseSaveLocation=<passphrase save location>
  UseExistingSQL=<1/0 use or do not use existing SQL>
  ```

3. Zapisz plik. Następnie w wierszu polecenia z podwyższonym poziomem uprawnień na serwerze instalacji, wprowadź polecenie:

  ```
  start /wait <cdlayout path>/Setup.exe /i  /f <.ini file path>/setup.ini /L <log path>/setup.log
  ```

Można użyć tych flag do instalacji:</br>
**/f**: ścieżka pliku ini</br>
**/l**: ścieżka dziennika</br>
**/i**: ścieżka instalacji</br>
**/x**: Odinstaluj ścieżki</br>

## <a name="next-steps"></a>Następne kroki
Po zainstalowaniu serwera kopii zapasowej, Dowiedz się, jak przygotować serwer lub Włącz ochronę obciążeń.

- [Przygotowanie serwera kopii zapasowej obciążeń](backup-azure-microsoft-azure-backup.md)
- [Użyj kopii zapasowej serwera, aby utworzyć kopię zapasową serwera VMware](backup-azure-backup-server-vmware.md)
- [Utwórz kopię zapasową serwera umożliwia tworzenie kopii zapasowych programu SQL Server](backup-azure-sql-mabs.md)
- [Dodaj nowoczesnych magazynu kopii zapasowej do tworzenia kopii zapasowej serwera](backup-mabs-add-storage.md)
