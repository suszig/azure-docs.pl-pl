---
title: Tworzenie kopii zapasowej Azure stosu | Dokumentacja firmy Microsoft
description: "Wykonaj kopię zapasową na żądanie w stosie Azure przy użyciu kopii zapasowej w miejscu."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9565DDFB-2CDB-40CD-8964-697DA2FFF70A
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: mabrigg
ms.openlocfilehash: daea97c0f5ee6ef855dc50c1ed6c7934aa85a1c4
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/10/2018
---
# <a name="back-up-azure-stack"></a>Tworzenie kopii zapasowej Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Wykonaj kopię zapasową na żądanie w stosie Azure przy użyciu kopii zapasowej w miejscu. Jeśli musisz włączyć usługę kopia zapasowa infrastruktury, zobacz [włączenia kopii zapasowej Azure stosu z portalu administracyjnego](azure-stack-backup-enable-backup-console.md).

## <a name="start-azure-stack-backup"></a>Kopia zapasowa Azure stosu

Otwórz program Windows PowerShell z podniesionego wiersza, a następnie uruchom następujące polecenia:

   ```powershell
   Start-AzSBackup -Location $location.Name
   ```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Potwierdź kopii zapasowej zostało ukończone w portalu administracyjnym

1. Otwieranie portalu administracyjnego platformy Azure stosu w [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Wybierz **więcej usług** > **infrastruktura kopii zapasowej**. Wybierz **konfiguracji** w **infrastruktura kopii zapasowej** bloku.
3. Znajdź **nazwa** i **Data ukończenia** kopii zapasowej w **dostępnych kopii zapasowych** listy.
4. Sprawdź **stanu** jest **zakończyło się pomyślnie**.

Można również potwierdzić tworzenie kopii zapasowej zakończone z portalu administracyjnego. Przejdź do`\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się więcej na temat przepływu pracy do odzyskiwania od zdarzenia utraty danych. Zobacz [odzyskać przed utratą danych w wyniku katastrofy](azure-stack-backup-recover-data.md).
