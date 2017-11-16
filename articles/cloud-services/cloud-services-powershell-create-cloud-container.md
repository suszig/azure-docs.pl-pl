---
title: "Utwórz kontener usługi chmury przy użyciu programu PowerShell | Dokumentacja firmy Microsoft"
description: "W tym artykule wyjaśniono, jak utworzyć kontener usługi chmury przy użyciu programu PowerShell. Kontener obsługuje role sieci web i proces roboczy."
services: cloud-services
documentationcenter: .net
author: cawaMS
manager: timlt
editor: 
ms.assetid: c8f32469-610e-4f37-a3aa-4fac5c714e13
ms.service: cloud-services
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: na
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: cd703feb7bf5af765fc3a5448464499aa7b48d6a
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/16/2017
---
# <a name="use-an-azure-powershell-command-to-create-an-empty-cloud-service-container"></a>Użyj polecenia programu Azure PowerShell, aby utworzyć kontener usługi chmury pusty
W tym artykule wyjaśniono, jak szybko utworzyć kontener usługi w chmurze przy użyciu poleceń cmdlet programu Azure PowerShell. Wykonaj poniższe kroki:

1. Zainstaluj polecenia cmdlet programu PowerShell usługi Microsoft Azure z [programu Azure PowerShell pobierze](http://aka.ms/webpi-azps) strony.
2. Otwórz wiersz polecenia programu PowerShell.
3. Użyj [Add-AzureAccount](https://msdn.microsoft.com/library/dn495128.aspx) do logowania.

   > [!NOTE]
   > Dodatkowe instrukcje dotyczące instalowania polecenia cmdlet programu Azure PowerShell i nawiązywania połączenia z subskrypcją platformy Azure można znaleźć w temacie [jak instalowanie i konfigurowanie programu Azure PowerShell](/powershell/azure/overview).
   >
   >
4. Użyj **New-AzureService** , aby utworzyć kontener usługi chmury Azure puste.

    ```
    New-AzureService [-ServiceName] <String> [-AffinityGroup] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
    New-AzureService [-ServiceName] <String> [-Location] <String> [[-Label] <String>] [[-Description] <String>] [[-ReverseDnsFqdn] <String>] [<CommonParameters>]
   ```
5. Wykonaj w tym przykładzie do wywołania polecenia cmdlet:

   ```
   New-AzureService -ServiceName "mytestcloudservice" -Location "Central US" -Label "mytestcloudservice"
   ```

Aby uzyskać więcej informacji na temat tworzenia usługi w chmurze Azure Uruchom polecenie:

```
Get-help New-AzureService
```

### <a name="next-steps"></a>Następne kroki
* Aby zarządzać wdrażaniem usługi chmury, zapoznaj się [Get-AzureService](https://msdn.microsoft.com/library/azure/dn495131.aspx), [Remove-AzureService](https://msdn.microsoft.com/library/azure/dn495120.aspx), i [Set-AzureService](https://msdn.microsoft.com/library/azure/dn495242.aspx) poleceń. Mogą również dotyczyć [sposób konfigurowania usługi w chmurze](cloud-services-how-to-configure-portal.md) Aby uzyskać więcej informacji.
* Aby opublikować projekt usługi w chmurze na platformie Azure, zobacz **PublishCloudService.ps1** przykładowy kod z [ciągłego dostarczania dla usługi w chmurze na platformie Azure](cloud-services-dotnet-continuous-delivery.md).
