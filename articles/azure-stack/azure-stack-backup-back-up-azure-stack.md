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
ms.openlocfilehash: 955b286967ca2bc8450e8988ec16c6a5c352aa8a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="back-up-azure-stack"></a>Tworzenie kopii zapasowej Azure stosu

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Wykonaj kopię zapasową na żądanie w stosie Azure przy użyciu kopii zapasowej w miejscu. Jeśli musisz włączyć usługę kopia zapasowa infrastruktury, zobacz [włączenia kopii zapasowej Azure stosu z portalu administracyjnego](azure-stack-backup-enable-backup-console.md).

> [!Note]  
>  Zawiera narzędzia stosu Azure **Start AzSBackup** polecenia cmdlet. Aby uzyskać instrukcje na temat instalowania narzędzi, zobacz [rozpocząć pracę przy użyciu programu PowerShell w stosie Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-configure-quickstart).

## <a name="start-azure-stack-backup"></a>Kopia zapasowa Azure stosu

Otwórz program Windows PowerShell z podwyższonym poziomem uprawnień monitem w środowisku zarządzania operatora i uruchom następujące polecenia:

```powershell
    cd C:\tools\AzureStack-Tools-master\Connect
    Import-Module .\AzureStack.Connect.psm1

    cd C:\tools\AzureStack-Tools-master\Infrastructure
    Import-Module .\AzureStack.Infra.psm1 
    
    Start-AzSBackup -Location $location.Name
```

## <a name="confirm-backup-completed-in-the-administration-portal"></a>Potwierdź kopii zapasowej zostało ukończone w portalu administracyjnym

1. Otwieranie portalu administracyjnego platformy Azure stosu w [https://adminportal.local.azurestack.external](https://adminportal.local.azurestack.external).
2. Wybierz **więcej usług** > **infrastruktura kopii zapasowej**. Wybierz **konfiguracji** w **infrastruktura kopii zapasowej** bloku.
3. Znajdź **nazwa** i **Data ukończenia** kopii zapasowej w **dostępnych kopii zapasowych** listy.
4. Sprawdź **stanu** jest **zakończyło się pomyślnie**.

Można również potwierdzić tworzenie kopii zapasowej zakończone z portalu administracyjnego. Przejdź do`\MASBackup\<datetime>\<backupid>\BackupInfo.xml`

## <a name="next-steps"></a>Następne kroki

- Dowiedz się więcej na temat przepływu pracy do odzyskiwania od zdarzenia utraty danych. Zobacz [odzyskać przed utratą danych w wyniku katastrofy](azure-stack-backup-recover-data.md).
