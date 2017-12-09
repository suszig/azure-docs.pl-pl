---
title: "Rozszerzenie maszyny wirtualnej Azure Agent obserwatorów sieci dla systemu Windows | Dokumentacja firmy Microsoft"
description: "Wdróż agenta monitora sieci na maszynie wirtualnej z systemem Windows przy użyciu rozszerzenia maszyny wirtualnej."
services: virtual-machines-windows
documentationcenter: 
author: dennisg
manager: amku
editor: 
tags: azure-resource-manager
ms.assetid: 27e46af7-2150-45e8-b084-ba33de8c5e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: dennisg
ms.openlocfilehash: 508b3755556bcae6aa2c7d17a2d86a1430a8109a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/08/2017
---
# <a name="network-watcher-agent-virtual-machine-extension-for-windows"></a>Rozszerzenie maszyny wirtualnej obserwatorów agenta sieciowe dla systemu Windows

## <a name="overview"></a>Omówienie

[Azure obserwatora sieciowego](https://review.docs.microsoft.com/azure/network-watcher/) jest sieci performance monitoring, Diagnostyka i analiza usługa, która umożliwia monitorowanie sieci platformy Azure. Rozszerzenie maszyny wirtualnej sieci obserwatorów agenta jest wymagane dla niektórych funkcji obserwatora sieciowego na maszynach wirtualnych Azure. Dotyczy to również Przechwytywanie ruchu sieciowego na żądanie i inne zaawansowane funkcje.

Ten dokument zawiera szczegóły dotyczące obsługiwanych platform i opcje wdrażania dla rozszerzenia maszyny wirtualnej sieci obserwatorów agenta dla systemu Windows.

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="operating-system"></a>System operacyjny

Rozszerzenie sieci obserwatorów agenta dla systemu Windows mogą być uruchamiane na systemie Windows Server 2008 R2, 2012 i 2012 R2, 2016 wersje. Należy pamiętać, że na serwerze Nano nie jest obsługiwana w tej chwili.

### <a name="internet-connectivity"></a>Łączność z Internetem

Niektóre funkcje Agent monitora sieci wymaga, czy docelowa maszyna wirtualna jest połączony z Internetem. Bez możliwości nawiązywania połączeń wychodzących niektóre funkcje sieciowe obserwatorów agenta nieprawidłowe działanie lub stała się niedostępna. Aby uzyskać więcej informacji, zobacz [dokumentacji obserwatora sieciowego](../../network-watcher/network-watcher-monitoring-overview.md).

## <a name="extension-schema"></a>Rozszerzenie schematu

Następujące JSON zawiera schemat rozszerzenia Agent monitora sieci. Rozszerzenie nie wymaga nie obsługuje wszystkie ustawienia dostarczone przez użytkownika w tej chwili i zależy od konfiguracji domyślnej.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentWindows",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Wartości właściwości

| Nazwa | Wartość / przykład |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Wydawcy | Microsoft.Azure.NetworkWatcher |
| type | NetworkWatcherAgentWindows |
| typeHandlerVersion | 1.4 |


## <a name="template-deployment"></a>Wdrażanie na podstawie szablonu

Rozszerzenia maszyny Wirtualnej platformy Azure można wdrożyć przy użyciu szablonów usługi Azure Resource Manager. Schematu JSON szczegółowo opisane w poprzedniej sekcji można w szablonie usługi Azure Resource Manager rozszerzenia sieci obserwatorów agenta są uruchamiane podczas wdrażania szablonu usługi Azure Resource Manager.

## <a name="powershell-deployment"></a>Wdrożenie programu PowerShell

`Set-AzureRmVMExtension` Polecenia może służyć do wdrożenia sieci obserwatorów agenta rozszerzenie maszyny wirtualnej na istniejącej maszyny wirtualnej.

```powershell
Set-AzureRmVMExtension -ResourceGroupName "myResourceGroup1" `
                       -Location "WestUS" `
                       -VMName "myVM1" `
                       -Name "networkWatcherAgent" `
                       -Publisher "Microsoft.Azure.NetworkWatcher" `
                       -Type "NetworkWatcherAgentWindows" `
                       -TypeHandlerVersion "1.4"
```

## <a name="troubleshooting-and-support"></a>Rozwiązywanie problemów i pomoc techniczna

### <a name="troubleshooting"></a>Rozwiązywanie problemów

Dane dotyczące stanu wdrożenia rozszerzenia może zostać pobrany z portalu Azure i przy użyciu modułu Azure PowerShell. Aby wyświetlić stan wdrożenia rozszerzeń dla danej maszyny Wirtualnej, uruchom następujące polecenie przy użyciu modułu Azure PowerShell.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup1 -VMName myVM1 -Name networkWatcherAgent
```

Dane wyjściowe wykonania rozszerzenia jest rejestrowany pliki znajdujące się w następującym katalogu:

```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Azure.NetworkWatcher.NetworkWatcherAgentWindows\
```

### <a name="support"></a>Pomoc techniczna

Jeśli potrzebujesz więcej pomocy w dowolnym momencie, w tym artykule, można zapoznać się z dokumentacją Podręcznik użytkownika obserwatora sieci lub skontaktuj się z ekspertami Azure na [fora MSDN Azure i przepełnienie stosu](https://azure.microsoft.com/en-us/support/forums/). Alternatywnie można pliku zdarzenia pomocy technicznej platformy Azure. Przejdź do [witrynę pomocy technicznej platformy Azure](https://azure.microsoft.com/en-us/support/options/) i wybierz Uzyskaj pomoc techniczną. Aby uzyskać informacje o korzystaniu z platformy Azure obsługuje, przeczytaj [pomocy technicznej Microsoft Azure — często zadawane pytania](https://azure.microsoft.com/en-us/support/faq/).
