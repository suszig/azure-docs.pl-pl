---
title: Azure niestandardowe rozszerzenie skryptu dla systemu Windows | Dokumentacja firmy Microsoft
description: "Automatyzowanie zadań konfiguracji maszyny Wirtualnej systemu Windows przy użyciu rozszerzenia niestandardowego skryptu"
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f4181fee-7a9d-4a1c-b517-52956f5b7fa1
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/16/2017
ms.author: nepeters
ms.openlocfilehash: 4117b6020d2d75a953fd5f032b378e49d2c752ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="custom-script-extension-for-windows"></a>Niestandardowe rozszerzenie skryptu dla systemu Windows

Niestandardowe rozszerzenie skryptu pobiera i uruchamia skrypty na maszynach wirtualnych Azure. To rozszerzenie jest przydatne w przypadku konfiguracji wdrożenia post, instalacja oprogramowania lub dowolnej innej konfiguracji / zadanie zarządzania. Skryptów można pobrać z magazynu Azure lub usługi GitHub lub dostarczone do portalu Azure pod adresem rozszerzenia czas wykonywania. Rozszerzenie skryptu niestandardowego integruje się z szablonów usługi Azure Resource Manager i mogą być także uruchamiane przy użyciu wiersza polecenia platformy Azure, programu PowerShell, portalu Azure lub interfejsu API REST dla maszyny wirtualnej Azure.

Ten dokument zawiera szczegóły dotyczące sposobu używania niestandardowe rozszerzenie skryptu przy użyciu modułu Azure PowerShell, szablony usługi Azure Resource Manager i szczegóły dotyczące rozwiązywania problemów z w systemach Windows.

## <a name="prerequisites"></a>Wymagania wstępne

### <a name="operating-system"></a>System operacyjny

Niestandardowy skrypt rozszerzenia dla systemu Windows można uruchamiać przed dla klienta systemu Windows 10, Windows Server 2008 R2, 2012 i 2012 R2, 2016 zwalnia.

### <a name="script-location"></a>Lokalizacja skryptu

Skrypt musi być przechowywany w magazynie obiektów Blob Azure lub w innej lokalizacji dostępny za pośrednictwem prawidłowy adres URL.

### <a name="internet-connectivity"></a>Połączenie z Internetem

Niestandardowego skryptu rozszerzenia dla systemu Windows wymaga, aby docelowa maszyna wirtualna jest połączony z Internetem. 

## <a name="extension-schema"></a>Rozszerzenie schematu

Następujące JSON zawiera schemat niestandardowe rozszerzenie skryptu. Rozszerzenie wymaga lokalizacji skryptu (magazynu Azure lub innej lokalizacji z prawidłowym adresem URL), a polecenie do wykonania. Jeśli jako źródło skryptu, za pomocą usługi Azure Storage, usługa Azure storage konta nazwy i klucza konta jest wymagany. Te elementy powinny traktowane jako poufne dane i określony w konfiguracji chronionych ustawień rozszerzenia. Dane Azure ustawienia rozszerzenia chronione maszyny Wirtualnej jest szyfrowany i odszyfrowane tylko na docelowej maszynie wirtualnej.

```json
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines/extensions",
    "name": "config-app",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
        "[variables('musicstoresqlName')]"
    ],
    "tags": {
        "displayName": "config-app"
    },
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.9",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "fileUris": [
                "script location"
            ]
        },
        "protectedSettings": {
            "commandToExecute": "myExecutionCommand",
            "storageAccountName": "myStorageAccountName",
            "storageAccountKey": "myStorageAccountKey"
        }
    }
}
```

### <a name="property-values"></a>Wartości właściwości

| Nazwa | Wartość / przykład |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| Wydawcy | Microsoft.Compute |
| type | Rozszerzenia |
| typeHandlerVersion | 1.9 |
| fileUris (np.) | https://RAW.githubusercontent.com/Microsoft/DotNet-Core-Sample-Templates/Master/DotNet-Core-Music-Windows/scripts/Configure-Music-App.ps1 |
| commandToExecute (np.) | PowerShell - ExecutionPolicy Unrestricted - pliku skonfigurować muzyka app.ps1 |
| storageAccountName (np.) | examplestorageacct |
| storageAccountKey (np.) | TmJK/1N3AbAZ3q / + hOXoi/l73zOqsaxXDhqa9Y83/v5UpXQp2DQIBuv2Tifp60cE/OaHsJZmQZ7teQfczQj8hg == |

**Uwaga** — te nazwy właściwości jest uwzględniana wielkość liter. Użyj nazwy, jak pokazano powyżej, aby uniknąć problemów dotyczących wdrożenia.

## <a name="template-deployment"></a>Wdrażanie na podstawie szablonu

Rozszerzenia maszyny Wirtualnej platformy Azure można wdrożyć przy użyciu szablonów usługi Azure Resource Manager. Schematu JSON szczegółowo opisane w poprzedniej sekcji można w szablonie usługi Azure Resource Manager niestandardowe rozszerzenie skryptu są uruchamiane podczas wdrażania szablonu usługi Azure Resource Manager. Przykładowy szablon, który zawiera niestandardowe rozszerzenie skryptu można znaleźć w tym miejscu [GitHub](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="powershell-deployment"></a>Wdrożenie programu PowerShell

`Set-AzureRmVMCustomScriptExtension` Polecenia można dodać rozszerzenia niestandardowego skryptu do istniejącej maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz [AzureRmVMCustomScriptExtension zestawu ](https://docs.microsoft.com/en-us/powershell/resourcemanager/azurerm.compute/v2.1.0/set-azurermvmcustomscriptextension).
```powershell
Set-AzureRmVMCustomScriptExtension -ResourceGroupName myResourceGroup `
    -VMName myVM `
    -Location myLocation `
    -FileUri myURL `
    -Run 'myScript.ps1' `
    -Name DemoScriptExtension
```

## <a name="troubleshoot-and-support"></a>Rozwiązywanie problemów i obsługa techniczna

### <a name="troubleshoot"></a>Rozwiązywanie problemów

Dane dotyczące stanu wdrożenia rozszerzenia może zostać pobrany z portalu Azure i przy użyciu modułu Azure PowerShell. Aby wyświetlić stan wdrożenia rozszerzeń dla danej maszyny Wirtualnej, uruchom następujące polecenie.

```powershell
Get-AzureRmVMExtension -ResourceGroupName myResourceGroup -VMName myVM -Name myExtensionName
```

Dane wyjściowe wykonania rozszerzenia jest rejestrowane w plikach znajdują się w następującym katalogu na docelowej maszynie wirtualnej.
```cmd
C:\WindowsAzure\Logs\Plugins\Microsoft.Compute.CustomScriptExtension
```

Określone pliki są pobierane do następującego katalogu na docelowej maszynie wirtualnej.
```cmd
C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.*\Downloads\<n>
```
gdzie `<n>` jest dziesiętną liczbą całkowitą, która może zmienić pomiędzy wykonaniami rozszerzenia.  `1.*` Wartość odpowiada bieżącej rzeczywiste, `typeHandlerVersion` wartość rozszerzenia.  Na przykład może być rzeczywiste katalogu `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2`.  

Podczas wykonywania `commandToExecute` polecenia rozszerzenia spowoduje ustawienie tego katalogu (np. `...\Downloads\2`) jako bieżący katalog roboczy. Dzięki temu używanie ścieżek względnych, aby zlokalizować pliki pobrane za pośrednictwem `fileURIs` właściwości. Poniższa tabela przykłady.

Ponieważ ścieżka bezwzględna pobierania może się różnić w czasie, zaleca się opt ścieżek względnych skryptów i plików w `commandToExecute` string, jeśli to możliwe. Na przykład:
```json
    "commandToExecute": "powershell.exe . . . -File './scripts/myscript.ps1'"
```

Informacje o ścieżce po pierwszy segment identyfikatora URI został zachowany na potrzeby plików pobranych za pośrednictwem `fileUris` listy właściwości.  Jak pokazano w poniższej tabeli, pobierane pliki są mapowane do pobierania podkatalogów w celu odzwierciedlenia struktury `fileUris` wartości.  

#### <a name="examples-of-downloaded-files"></a>Przykłady pobrane pliki

| Identyfikator URI w fileUris | Względne położenie pobrany | Bezwzględna pobrać lokalizację * |
| ---- | ------- |:--- |
| `https://someAcct.blob.core.windows.net/aContainer/scripts/myscript.ps1` | `./scripts/myscript.ps1` |`C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\scripts\myscript.ps1`  |
| `https://someAcct.blob.core.windows.net/aContainer/topLevel.ps1` | `./topLevel.ps1` | `C:\Packages\Plugins\Microsoft.Compute.CustomScriptExtension\1.8\Downloads\2\topLevel.ps1` |

\*Jako powyżej, ścieżki bezwzględnej katalogów zmieni się w okresie istnienia maszyny wirtualnej, ale nie w ramach pojedynczego uruchomienia rozszerzenia CustomScript.

### <a name="support"></a>Pomoc techniczna

Jeśli potrzebujesz więcej pomocy w dowolnym momencie, w tym artykule, możesz skontaktować się ekspertów platformy Azure na [MSDN Azure i przepełnienie stosu forach] (https://azure.microsoft.com/en-us/support/forums/). Alternatywnie można pliku zdarzenia pomocy technicznej platformy Azure. Przejdź do [witrynę pomocy technicznej platformy Azure](https://azure.microsoft.com/en-us/support/options/) i wybierz Uzyskaj pomoc techniczną. Aby uzyskać informacje o korzystaniu z platformy Azure obsługuje, przeczytaj [pomocy technicznej Microsoft Azure — często zadawane pytania](https://azure.microsoft.com/en-us/support/faq/).
