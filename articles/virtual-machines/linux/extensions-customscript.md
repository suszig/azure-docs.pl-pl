---
title: "Uruchamianie niestandardowych skryptów na maszynach wirtualnych systemu Linux na platformie Azure | Dokumentacja firmy Microsoft"
description: "Automatyzowanie zadań konfiguracji maszyny Wirtualnej systemu Linux przy użyciu niestandardowe rozszerzenie skryptu"
services: virtual-machines-linux
documentationcenter: 
author: danielsollondon
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cf17ab2b-8d7e-4078-b6df-955c6d5071c2
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/26/2017
ms.author: danis
ms.openlocfilehash: 3f8eeb71027d9bdd5c72570bd07d673136646e6c
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/08/2017
---
# <a name="using-the-azure-custom-script-extension-with-linux-virtual-machines"></a>Przy użyciu rozszerzenia Azure niestandardowego skryptu z maszyn wirtualnych systemu Linux
Niestandardowe rozszerzenie skryptu pobiera i uruchamia skrypty na maszynach wirtualnych Azure. To rozszerzenie jest przydatne w przypadku konfiguracji wdrożenia post, instalacja oprogramowania lub dowolnej innej konfiguracji / zadanie zarządzania. Skrypty można pobrać z magazynu Azure lub inne dostępne miejsce w Internecie lub dostarczony do rozszerzenia, czas wykonywania. Rozszerzenie skryptu niestandardowego integruje się z szablonów usługi Azure Resource Manager i mogą być także uruchamiane przy użyciu wiersza polecenia platformy Azure, programu PowerShell, portalu Azure lub interfejsu API REST dla maszyny wirtualnej Azure.

Ten dokument zawiera szczegóły dotyczące używania niestandardowe rozszerzenie skryptu z wiersza polecenia platformy Azure i szablonu usługi Azure Resource Manager, a także szczegółowe kroki rozwiązywania problemów w systemach Linux.

## <a name="extension-configuration"></a>Konfiguracja rozszerzenia
Niestandardowe rozszerzenie skryptu konfiguracji określa lokalizację i polecenia do uruchomienia. Tej konfiguracji mogą być przechowywane w plikach konfiguracji określone w wierszu polecenia lub w szablonie usługi Azure Resource Manager. Poufne dane mogą być przechowywane w chronionej konfigurację, która zostaje zaszyfrowany i odszyfrowane tylko na maszynie wirtualnej. Chronione konfiguracja jest użyteczna, jeśli wykonanie polecenia obejmuje tajemnice, takie jak hasła.

### <a name="public-configuration"></a>Publiczna Konfiguracja
Schemat:

**Uwaga** — te nazwy właściwości jest uwzględniana wielkość liter. Użyj nazwy, jak pokazano poniżej, aby uniknąć problemów dotyczących wdrożenia.

* **commandToExecute**: (wymagane, string) wykonywania skryptu punktu wejścia
* **fileUris**: (opcjonalne, tablicy ciągów) w adresach URL dla plików do pobrania.
* **Sygnatura czasowa** (opcjonalne, liczba całkowita) to pole służy tylko do wyzwolenia Uruchom ponownie skryptu, zmieniając wartość tego pola.

```json
{
  "fileUris": ["<url>"],
  "commandToExecute": "<command-to-execute>"
}
```

### <a name="protected-configuration"></a>Konfiguracja chronionych
Schemat:

**Uwaga** — te nazwy właściwości jest uwzględniana wielkość liter. Użyj nazwy, jak pokazano poniżej, aby uniknąć problemów dotyczących wdrożenia.

* **commandToExecute**: (opcjonalne, ciąg) wykonywania skryptu punktu wejścia. Zamiast tego użyj tego pola, jeśli polecenie zawiera klucze tajne, takie jak hasła.
* **storageAccountName**: (opcjonalne, ciąg) nazwa konta magazynu. Jeśli określisz poświadczeń magazynu fileUris wszystkie muszą być adresami URL dla obiektów blob Azure.
* **storageAccountKey**: (opcjonalne, ciąg) klucz dostępu konta magazynu.

```json
{
  "commandToExecute": "<command-to-execute>",
  "storageAccountName": "<storage-account-name>",
  "storageAccountKey": "<storage-account-key>"
}
```

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure
Uruchom niestandardowe rozszerzenie skryptu za pomocą interfejsu wiersza polecenia Azure, należy utworzyć plik konfiguracji lub pliki zawierające co najmniej uri pliku i wykonywania polecenia skryptu.

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

Ustawienia można opcjonalnie określić w poleceniu jako ciąg w formacie JSON. Dzięki temu konfiguracji należy określić podczas wykonywania i bez pliku osobną konfiguracją.

```azurecli
az vm extension set '
  --resource-group exttest `
  --vm-name exttest `
  --name customScript `
  --publisher Microsoft.Azure.Extensions `
  --settings '{"fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],"commandToExecute": "./test.sh"}'
```

### <a name="azure-cli-examples"></a>Przykłady Azure CLI

**Przykład 1** -publicznej konfiguracji z pliku skryptu.

```json
{
  "fileUris": ["https://raw.githubusercontent.com/neilpeterson/test-extension/master/test.sh"],
  "commandToExecute": "./test.sh"
}
```

Polecenia interfejsu wiersza polecenia platformy Azure:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Przykład 2** -publicznej konfiguracji z żadnego pliku skryptu.

```json
{
  "commandToExecute": "apt-get -y update && apt-get install -y apache2"
}
```

Polecenia interfejsu wiersza polecenia platformy Azure:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json
```

**Przykład 3** — plik konfiguracji publicznego służy do określania pliku skryptu identyfikatora URI, a plik konfiguracji chronionych służy do określania polecenie do wykonania.

Plik konfiguracji publicznego:

```json
{
  "fileUris": ["https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"]
}
```

Plik chroniony konfiguracji:  

```json
{
  "commandToExecute": "./hello.sh <password>"
}
```

Polecenia interfejsu wiersza polecenia platformy Azure:

```azurecli
az vm extension set --resource-group myResourceGroup --vm-name myVM --name customScript --publisher Microsoft.Azure.Extensions --settings ./script-config.json --protected-settings ./protected-config.json
```

## <a name="resource-manager-template"></a>Szablon usługi Resource Manager
Niestandardowe rozszerzenie skryptu Azure może działać w czasie wdrażania maszyny wirtualnej przy użyciu szablonu usługi Resource Manager. Aby to zrobić, należy dodać prawidłowo sformatowane JSON do szablonu wdrożenia.

### <a name="resource-manager-examples"></a>Przykłady usługi Resource Manager
**Przykład 1** -publicznej konfiguracji.

```json
{
    "name": "scriptextensiondemo",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "2015-06-15",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', parameters('scriptextensiondemoName'))]"
    ],
    "tags": {
        "displayName": "scriptextensiondemo"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
      "settings": {
        "fileUris": [
          "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
        ],
        "commandToExecute": "sh hello.sh"
      }
    }
}
```

**Przykład 2** — wykonanie polecenia w chronionym konfiguracji.

```json
{
  "name": "config-app",
  "type": "extensions",
  "location": "[resourceGroup().location]",
  "apiVersion": "2015-06-15",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', concat(variables('vmName'),copyindex()))]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Azure.Extensions",
    "type": "CustomScript",
    "typeHandlerVersion": "2.0",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://gist.github.com/ahmetalpbalkan/b5d4a856fe15464015ae87d5587a4439/raw/466f5c30507c990a4d5a2f5c79f901fa89a80841/hello.sh"
      ]              
    },
    "protectedSettings": {
      "commandToExecute": "sh hello.sh <password>"
    }
  }
}
```

Zobacz .net Core utworów muzycznych magazynu pokaz pełny przykład - [pokaz magazynu utworów muzycznych](https://github.com/neilpeterson/nepeters-azure-templates/tree/master/dotnet-core-music-linux-vm-sql-db).

## <a name="troubleshooting"></a>Rozwiązywanie problemów
Po uruchomieniu niestandardowe rozszerzenie skryptu skrypt jest tworzony lub pobrany do katalogu podobny do poniższego przykładu. Dane wyjściowe polecenia jest także zapisane w tym katalogu w `stdout` i `stderr` pliku.

```bash
/var/lib/waagent/custom-script/download/0/
```

Rozszerzenie skryptu Azure tworzy dziennika, który można znaleźć tutaj.

```bash
/var/log/azure/custom-script/handler.log
```

Stan wykonywania niestandardowe rozszerzenie skryptu można również pobrać z wiersza polecenia platformy Azure.

```azurecli
az vm extension list -g myResourceGroup --vm-name myVM
```

Dane wyjściowe wyglądają następująco następujący tekst:

```azurecli
info:    Executing command vm extension get
+ Looking up the VM "scripttst001"
data:    Publisher                   Name                                      Version  State
data:    --------------------------  ----------------------------------------  -------  ---------
data:    Microsoft.Azure.Extensions  CustomScript                              2.0      Succeeded
data:    Microsoft.OSTCExtensions    Microsoft.Insights.VMDiagnosticsSettings  2.3      Succeeded
info:    vm extension get command OK
```

## <a name="next-steps"></a>Następne kroki
Aby uzyskać informacje na inne rozszerzenia skryptu maszyny Wirtualnej, zobacz [omówienie rozszerzenie skryptu Azure dla systemu Linux](extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

