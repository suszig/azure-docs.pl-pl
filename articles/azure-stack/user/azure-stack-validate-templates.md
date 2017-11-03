---
title: "Sprawdź, czy szablony stosu Azure za pomocą szablonu modułu sprawdzania poprawności | Dokumentacja firmy Microsoft"
description: "Sprawdź szablony do wdrożenia na stosie Azure"
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: d9e6aee1-4cba-4df5-b5a3-6f38da9627a3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: c99e5ebc2612e10f42bddbbd2f1c17d7404305d3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="check-your-templates-for-azure-stack-with-template-validator"></a>Sprawdź, czy szablony Azure stos szablonu modułu sprawdzania poprawności

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Można użyć narzędzia sprawdzania poprawności szablonu, aby sprawdzić, czy z usługi Azure Resource Manager [szablony](azure-stack-arm-templates.md) są gotowe do stosu Azure. Narzędzie sprawdzania poprawności szablonu jest dostępna jako część pakietu narzędzi Azure stosu. Pobieranie narzędzia Azure stosu przy użyciu procedury opisanej w [Pobierz narzędzia z witryny GitHub](azure-stack-powershell-download.md) artykułu. 

Aby sprawdzić poprawność szablony, użyj następujących modułów programu PowerShell i pliku JSON, który znajduje się w **TemplateValidator** i **CloudCapabilities** folderów: 

 - AzureRM.CloudCapabilities.psm1 tworzy plik JSON możliwości chmury reprezentujący usług i wersji w chmurze, takich jak Azure stosu.
 - AzureRM.TemplateValidator.psm1 używa pliku JSON możliwości chmury do testowania szablonów dla wdrożenia w stosie Azure.
 - AzureStackCloudCapabilities_with_AddOns_20170627.json jest domyślny plik możliwości chmury.  Utwórz swój własny lub użyć tego pliku, aby rozpocząć pracę. 

W tym temacie Uruchom sprawdzanie poprawności względem szablonów i opcjonalnie tworzy plik możliwości chmury.

## <a name="validate-templates"></a>Sprawdź poprawność szablonów
W tych kroków sprawdzanie poprawności szablonów za pomocą modułu środowiska AzureRM.TemplateValidator PowerShell. Możesz użyć własnych szablonów lub zweryfikować [szablonów Szybki Start Azure stosu](https://github.com/Azure/AzureStack-QuickStart-Templates).

1.  Zaimportuj moduł programu PowerShell AzureRM.TemplateValidator.psm1:
    
    ```PowerShell
    cd "c:\AzureStack-Tools-master\TemplateValidator"
    Import-Module .\AzureRM.TemplateValidator.psm1
    ```

2.  Uruchom moduł sprawdzania poprawności szablonu:

    ```PowerShell
    Test-AzureRMTemplate -TemplatePath <path to template.json or template folder> `
    -CapabilitiesPath <path to cloudcapabilities.json> `
    -Verbose
    ```

Szablon sprawdzania poprawności ostrzeżeń i błędów są rejestrowane w konsoli programu PowerShell, a także są rejestrowane w pliku HTML w katalogu źródłowym. Przykład danych wyjściowych raportu weryfikacji wygląda następująco:

![przykładowy raport weryfikacji](./media/azure-stack-validate-templates/image1.png)

### <a name="parameters"></a>Parametry

| Parametr | Opis | Wymagane |
| ----- | -----| ----- |
| TemplatePath | Określa ścieżkę do rekursywnie szablonów usługi Resource Manager | Tak | 
| TemplatePattern | Określa nazwę plików szablonów do dopasowania. | Nie |
| CapabilitiesPath | Określa ścieżkę do pliku JSON możliwości w chmurze | Tak | 
| IncludeComputeCapabilities | Obejmuje ocenę zasobów IaaS, takich jak rozmiarów maszyn wirtualnych i rozszerzeń maszyny Wirtualnej | Nie |
| IncludeStorageCapabilities | Obejmuje ocenę zasobów magazynu, takich jak typy jednostki SKU | Nie |
| Raport | Określa nazwę wygenerowanego raportu HTML | Nie |
| Pełne | Dzienniki błędów i ostrzeżeń w konsoli | Nie|


### <a name="examples"></a>Przykłady
W tym przykładzie weryfikuje wszystkie [szablonów Szybki Start Azure stosu](https://github.com/Azure/AzureStack-QuickStart-Templates) pobierana lokalnie, a także weryfikuje rozmiarów maszyn wirtualnych i rozszerzenia możliwościami Azure stosu Development Kit.

```PowerShell
test-AzureRMTemplate -TemplatePath C:\AzureStack-Quickstart-Templates `
-CapabilitiesPath .\TemplateValidator\AzureStackCloudCapabilities_with_AddOns_20170627.json.json `
-TemplatePattern MyStandardTemplateName.json`
-IncludeComputeCapabilities`
-Report TemplateReport.html
```

## <a name="build-cloud-capabilities-file"></a>Tworzenie pliku możliwości w chmurze
Pobranych plików obejmują domyślny *AzureStackCloudCapabilities_with_AddOns_20170627.json* pliku, który zawiera opis wersji usługi dostępne w systemie Azure stosu Development Kit z zainstalowanymi usługami PaaS.  Po zainstalowaniu dodatkowych dostawców zasobów służy modułu programu AzureRM.CloudCapabilities PowerShell tworzenia nowych usług w tym pliku JSON.  

1.  Upewnij się, że masz łączność stosu Azure.  Te czynności można wykonać z hosta zestawu Azure stosu rozwoju lub użyć [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) nawiązywania połączenia ze stacji roboczej. 
2.  Zaimportuj moduł programu AzureRM.CloudCapabilities PowerShell:

    ```PowerShell
    Import-Module .\CloudCapabilities\AzureRM.CloudCapabilities.psm1
    ``` 

3.  Użyj polecenia cmdlet Get-CloudCapabilities, aby pobrać wersje usługi i Utwórz plik JSON możliwości chmury:

    ```PowerShell
    Get-AzureRMCloudCapabilities -Location 'local' -Verbose
    ```             


## <a name="next-steps"></a>Następne kroki
 - [Wdrażanie szablonów Azure stosu](azure-stack-arm-templates.md)
 - [Tworzenie szablonów dla stosu Azure](azure-stack-develop-templates.md)

