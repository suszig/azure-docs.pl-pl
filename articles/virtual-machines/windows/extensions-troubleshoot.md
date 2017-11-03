---
title: "Rozwiązywanie problemów z błędami rozszerzenia maszyny Wirtualnej systemu Windows | Dokumentacja firmy Microsoft"
description: "Więcej informacji na temat rozwiązywania problemów z błędami rozszerzenia maszyny Wirtualnej systemu Windows Azure"
services: virtual-machines-windows
documentationcenter: 
author: kundanap
manager: timlt
editor: 
tags: top-support-issue,azure-resource-manager
ms.assetid: 878ab9b6-c3e6-40be-82d4-d77fecd5030f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 03/29/2016
ms.author: kundanap
ms.openlocfilehash: 4dba196e1b838f2092b30972fb070514bd2a4b25
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-azure-windows-vm-extension-failures"></a>Rozwiązywanie problemów z błędami rozszerzenia maszyny Wirtualnej systemu Windows Azure
[!INCLUDE [virtual-machines-common-extensions-troubleshoot](../../../includes/virtual-machines-common-extensions-troubleshoot.md)]

## <a name="viewing-extension-status"></a>Wyświetlanie stanu rozszerzenia
Szablony usługi Azure Resource Manager można wykonać z programu Azure PowerShell. Po wykonaniu szablonu z Eksploratora zasobów Azure lub narzędzia wiersza polecenia można wyświetlić stan rozszerzenia.

Oto przykład:

Azure PowerShell:

      Get-AzureRmVM -ResourceGroupName $RGName -Name $vmName -Status

Oto przykładowe dane wyjściowe:

      Extensions:  {
      "ExtensionType": "Microsoft.Compute.CustomScriptExtension",
      "Name": "myCustomScriptExtension",
      "SubStatuses": [
        {
          "Code": "ComponentStatus/StdOut/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "    Directory: C:\\temp\\n\\n\\nMode                LastWriteTime     Length Name
              \\n----                -------------     ------ ----                              \\n-a---          9/1/2015   2:03 AM         11
              test.txt                          \\n\\n",
                      "Time": null
          },
        {
          "Code": "ComponentStatus/StdErr/succeeded",
          "DisplayStatus": "Provisioning succeeded",
          "Level": "Info",
          "Message": "",
          "Time": null
        }
    }
  ]

## <a name="troubleshooting-extension-failures"></a>Rozwiązywanie problemów z błędami rozszerzenia
### <a name="re-running-the-extension-on-the-vm"></a>Ponowne uruchomienie rozszerzenia na maszynie Wirtualnej
Jeśli skrypty są uruchomione na maszynie Wirtualnej za pomocą niestandardowego rozszerzenia skryptu, można uruchomić czasami wystąpił błąd, gdy maszyna wirtualna została utworzona pomyślnie, ale skryptu nie powiodło się. W obszarze tych warunków zalecanym sposobem odzyskania tego błędu jest Usuń rozszerzenie i ponownie uruchom szablon ponownie.
Uwaga: W przyszłości tej funkcji można rozszerzyć do konieczności odinstalowywania rozszerzenia.

#### <a name="remove-the-extension-from-azure-powershell"></a>Usuń rozszerzenie z programu Azure PowerShell
    Remove-AzureRmVMExtension -ResourceGroupName $RGName -VMName $vmName -Name "myCustomScriptExtension"

Po usunięciu rozszerzenia szablon może być ponowne wykonanie uruchamiania skryptów na maszynie Wirtualnej.

