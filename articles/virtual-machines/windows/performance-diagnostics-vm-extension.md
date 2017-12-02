---
title: "Rozszerzenia maszyny Wirtualnej diagnostyki Azure wydajności dla systemu Windows | Dokumentacja firmy Microsoft"
description: "Wprowadza rozszerzenia maszyny Wirtualnej diagnostyki Azure wydajności dla systemu Windows."
services: virtual-machines-windows'
documentationcenter: 
author: genlin
manager: cshepard
editor: na
tags: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 09/29/2017
ms.author: genli
ms.openlocfilehash: d9384af2cf1d8b3f55f9ec2316046536634c124e
ms.sourcegitcommit: 80eb8523913fc7c5f876ab9afde506f39d17b5a1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/02/2017
---
# <a name="azure-performance-diagnostics-vm-extension-for-windows"></a>Rozszerzenia maszyny Wirtualnej diagnostyki Azure wydajności dla systemu Windows

## <a name="summary"></a>Podsumowanie
Rozszerzenie maszyny Wirtualnej diagnostyki wydajności Azure pomaga diagnostycznych zbieranie danych dotyczących wydajności z maszyn wirtualnych systemu Windows, przeprowadza analizę i udostępnia raport, wyniki i zalecenia pomogą zidentyfikować i rozwiązać problemy z wydajnością na maszynie wirtualnej. To rozszerzenie instaluje narzędzie rozwiązywania problemów [PerfInsights](http://aka.ms/perfinsights).

## <a name="prerequisites"></a>Wymagania wstępne
### <a name="operating-systems"></a>Systemy operacyjne
To rozszerzenie można zainstalować w systemie Windows Server 2008 R2, 2012, 2012 R2, 2016; Systemy operacyjne Windows 8.1 i Windows 10.

## <a name="extension-schema"></a>Rozszerzenie schematu
Następujące JSON zawiera schemat rozszerzenia diagnostyki Azure wydajności. To rozszerzenie wymaga nazwy i klucza konta magazynu do magazynu dane wyjściowe diagnostyki i raportów. Te wartości są poufne i powinien być przechowywany w chronionym ustawienia konfiguracji. Dane Azure ustawienia rozszerzenia chronione maszyny Wirtualnej jest szyfrowany i odszyfrowane tylko na docelowej maszynie wirtualnej. Należy pamiętać, że storageAccountName i storageAccountKey jest rozróżniana wielkość liter. Inne wymagane parametry są wymienione w poniższej sekcji.

```JSON
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
```

### <a name="property-values"></a>Wartości właściwości

|   **Nazwa**   |**Wartość / przykład**|       **Opis**      |
|--------------|-------------------|----------------------------|
|apiVersion|2015-06-15|Wersja interfejsu API
|Wydawcy|Microsoft.Azure.Performance.Diagnostics|Przestrzeń nazw wydawcy rozszerzenia
|type|AzurePerformanceDiagnostics|Typ rozszerzenia maszyny Wirtualnej
|typeHandlerVersion|1.0|Wersja rozszerzenia obsługi
|performanceScenario|podstawowe|Scenariuszu wydajności do przechwycenia danych. Prawidłowe wartości to: **podstawowe**, **vmslow**, **azurefiles**, i **niestandardowych**.
|traceDurationInSeconds|300|Czas trwania śledzenia, jeśli nie wybrano opcji śledzenia.
|perfCounterTrace|P|Opcję w celu włączenia śledzenia licznika wydajności. Prawidłowe wartości to **p** lub wartość pusta. Jeśli nie chcesz przechwytywać ślad, pozostaw wartość jako pusty.
|networkTrace|n|Opcję, aby włączyć program Netmon śledzenia. Prawidłowe wartości to  **n**  lub wartość pusta. Jeśli nie chcesz przechwytywać ślad, pozostaw wartość jako pusty.
|xperfTrace|x|Opcję, aby włączyć program XPerf śledzenia. Prawidłowe wartości to **x** lub wartość pusta. Jeśli nie chcesz przechwytywać ślad, pozostaw wartość jako pusty.
|storPortTrace|s|Opcję, aby włączyć StorPort śledzenia. Prawidłowe wartości to s lub wartość pustą. Jeśli nie chcesz przechwytywać ślad, pozostaw wartość jako pusty.
|srNumber|123452016365929|Numer biletu pomocy technicznej, jeśli jest dostępna. Pozostaw pole puste, jeśli nie masz.
|storageAccountName|mojekontomagazynu|Nazwa konta magazynu do przechowywania dzienników diagnostyki i wyników.
|storageAccountKey|lDuVvxuZB28NNP... hAiRF3voADxLBTcc ==|Klucz konta magazynu.

## <a name="install-the-extension"></a>Instalowanie rozszerzenia

Wykonaj następujące kroki, aby zainstalować rozszerzenie maszyny Wirtualnej na maszynach wirtualnych systemu Windows:

1. Zaloguj się w witrynie [Azure Portal](http://portal.azure.com).
2. Wybierz maszynę wirtualną, której chcesz zainstalować tego rozszerzenia.

    ![Wybierz maszynę wirtualną](media/performance-diagnostics-vm-extension/select-the-virtual-machine.png)
3. Wybierz **rozszerzenia** bloku i kliknij przycisk **Dodaj** przycisku.

    ![Wybieranie rozszerzenia](media/performance-diagnostics-vm-extension/select-extensions.png)
4. Wybierz rozszerzenie diagnostyki wydajności platformy Azure, zapoznaj się z warunkami i kliknij przycisk **Utwórz** przycisku.

    ![Tworzenie rozszerzenia diagnostyki Azure wydajności](media/performance-diagnostics-vm-extension/create-azure-performance-diagnostics-extension.png)
5. Podaj wartości parametrów dla instalacji i kliknij przycisk **OK** do zainstalowania rozszerzenia. Więcej informacji na temat obsługiwanych scenariuszy rozwiązywania problemów można znaleźć [tutaj](how-to-use-perfInsights.md#supported-troubleshooting-scenarios). 

    ![Instalowanie rozszerzenia](media/performance-diagnostics-vm-extension/install-the-extension.png)
6. Po pomyślnej instalacji pojawić się, że komunikat informujący o inicjowania obsługi administracyjnej zakończyło się pomyślnie.

    ![Inicjowanie obsługi administracyjnej zakończyło się pomyślnie wiadomości](media/performance-diagnostics-vm-extension/provisioning-succeeded-message.png)

    > [!NOTE]
    > Wykonanie rozszerzenia zaczyna się po udostępnianie jest zakończyło się pomyślnie i zajmuje kilka minut lub mniej, aby zakończyć wykonywanie podstawowych scenariuszy. W innych sytuacjach działa przez czas określony podczas instalacji.

## <a name="remove-the-extension"></a>Usuń rozszerzenie
Aby usunąć rozszerzenia z maszyny wirtualnej, wykonaj następujące kroki:

1. Zaloguj się do [portalu Azure](http://portal.azure.com), wybierz maszynę wirtualną, której chcesz usunąć to rozszerzenie, a następnie wybierz rozszerzenia bloku. 
2. Kliknij przycisk (**...** ) dla rozszerzenia diagnostyki wydajności wpisu z listy i kliknij przycisk Odinstaluj.

    ![Odinstaluj rozszerzenie](media/performance-diagnostics-vm-extension/uninstall-the-extension.png)

    > [!NOTE]
    > Możesz również wybierz wpis rozszerzenia i wybierz opcję Odinstaluj.

## <a name="template-deployment"></a>Wdrażanie szablonu
Rozszerzenia maszyny Wirtualnej platformy Azure można wdrożyć przy użyciu szablonów usługi Azure Resource Manager. Schematu JSON szczegółowo opisane w poprzedniej sekcji można w szablonie usługi Azure Resource Manager rozszerzenie Azure wydajności diagnostyki są uruchamiane podczas wdrażania szablonu usługi Azure Resource Manager. Poniżej przedstawiono przykładowy szablon, który może być używany z wdrażania szablonu:

````
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "vmName": {
      "type": "string",
      "defaultValue": "yourVMName"
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus"
    },
    "storageAccountName": {
      "type": "securestring"
      "defaultValue": "yourStorageAccount"
    },
    "storageAccountKey": {
      "type": "securestring"
      "defaultValue": "yourStorageAccountKey"
    },
    "performanceScenario": {
      "type": "string",
      "defaultValue": "basic"
    },
    "srNumber": {
      "type": "string",
      "defaultValue": ""
    },
    "traceDurationInSeconds": {
      "type": "int",
    "defaultValue": 300
    },
    "perfCounterTrace": {
      "type": "string",
      "defaultValue": "p"
    },
    "networkTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "xperfTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "storPortTrace": {
      "type": "string",
      "defaultValue": ""
    },
    "requestTimeUtc": {
      "type": "string",
      "defaultValue": "10/2/2017 11:06:00 PM"
    }       
  },
  "resources": [
    {
      "name": "[concat(parameters('vmName'),'/AzurePerformanceDiagnostics')]",
      "type": "Microsoft.Compute/virtualMachines/extensions",
      "location": "[parameters('location')]",
      "apiVersion": "2015-06-15",
      "properties": {
        "publisher": "Microsoft.Azure.Performance.Diagnostics",
        "type": "AzurePerformanceDiagnostics",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "performanceScenario": "[parameters('performanceScenario')]",
                  "traceDurationInSeconds": "[parameters('traceDurationInSeconds')]",
                  "perfCounterTrace": "[parameters('perfCounterTrace')]",
                  "networkTrace": "[parameters('networkTrace')]",
                  "xperfTrace": "[parameters('xperfTrace')]",
                  "storPortTrace": "[parameters('storPortTrace')]",
            "srNumber": "[parameters('srNumber')]",
            "requestTimeUtc":  "[parameters('requestTimeUtc')]"
        },
          "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]"        
            }
      }
    }
  ]
}
````

## <a name="powershell-deployment"></a>Wdrożenie programu PowerShell
`Set-AzureRmVMExtension` Polecenia może służyć do wdrożenia rozszerzenie maszyny wirtualnej diagnostyki Azure wydajności istniejącej maszyny wirtualnej. Przed uruchomieniem polecenia, konfiguracje publicznymi i prywatnymi muszą być przechowywane w tablicy skrótów programu PowerShell.

PowerShell

````
$PublicSettings = @{ "performanceScenario":"basic","traceDurationInSeconds":300,"perfCounterTrace":"p","networkTrace":"","xperfTrace":"","storPortTrace":"","srNumber":"","requestTimeUtc":"2017-09-28T22:08:53.736Z" }
$ProtectedSettings = @{"storageAccountName":"mystorageaccount","storageAccountKey":"mystoragekey"}

Set-AzureRmVMExtension -ExtensionName "AzurePerformanceDiagnostics" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Microsoft.Azure.Performance.Diagnostics" `
    -ExtensionType "AzurePerformanceDiagnostics" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
````

## <a name="information-on-the-data-captured"></a>Informacje na temat danych przechwyconych
Narzędzie PerfInsights gromadzi różne dzienniki, konfigurację, dane diagnostyczne itp. w zależności od wybranego scenariusza. Aby uzyskać więcej informacji na dane zbierane z jednego scenariusza, odwiedź stronę [dokumentacji PerfInsights](http://aka.ms/perfinsights).

## <a name="view-and-share-the-results"></a>Wyświetlanie i udostępnianie wyników

Dane wyjściowe rozszerzenia są przechowywane znajduje się w folderze o nazwie domyślnej log_collection w obszarze dysk Temp (zazwyczaj D:\log_collection). W tym folderze widać plików zip zawierający dzienników diagnostycznych i raport o wyniki i zalecenia.

Utworzony plik zip jest również przekazać do konta magazynu podana podczas instalacji i jest udostępniana przez 30 dni przy użyciu [udostępnionego sygnatur dostępu (SAS)](../../storage/common/storage-dotnet-shared-access-signature-part-1.md). Plik tekstowy o nazwie *zipfilename*_saslink.txt również jest tworzony w folderze log_collection. Ten plik zawiera łącze SAS utworzone, aby pobrać plik zip. Każdy, kto ma to łącze jest w stanie pobrać plik zip.

Firma Microsoft może używać tego łącza SAS, aby pobrać dane diagnostyczne w celu dokładniejszego zbadania przez niego obsługuje pracuje biletu pomocy technicznej.

Aby wyświetlić raport, po prostu Wyodrębnij plik zip i Otwórz **PerfInsights Report.html** pliku.

Można również pobrać plik zip bezpośrednio z portalu, wybierając rozszerzenie.

![Wyświetl szczegółowy stan](media/performance-diagnostics-vm-extension/view-detailed-status.png)

> [!NOTE]
> Łącze SAS wyświetlana w portalu może nie działać czasami z powodu źle sformułowany adres Url (spowodowane znaki specjalne) podczas kodowania i dekodowania operacji. Obejście jest uzyskanie link bezpośrednio z pliku *_saslink.txt z maszyny Wirtualnej.

## <a name="troubleshoot-and-support"></a>Rozwiązywanie problemów i obsługa techniczna
### <a name="troubleshoot"></a>Rozwiązywanie problemów

- Stan wdrożenia rozszerzenia (w obszarze powiadomień) mogą być wyświetlane "Wdrożenie w toku", mimo że rozszerzenia pomyślnie zainicjowano obsługę administracyjną.

    Ten problem można zignorować tak długo, jak stan rozszerzenia wskazuje, że rozszerzenie zostanie pomyślnie zainicjowana.
- Niektóre problemy podczas instalacji można rozwiązać, przy użyciu dzienników rozszerzenia. Dane wyjściowe wykonania rozszerzenia jest rejestrowany pliki znajdujące się w następującym katalogu:

        C:\Packages\Plugins\Microsoft.Azure.Performance.Diagnostics.AzurePerformanceDiagnostics

### <a name="support"></a>Pomoc techniczna

Jeśli potrzebujesz więcej pomocy w dowolnym momencie, w tym artykule, możesz skontaktować się ekspertów platformy Azure na [fora MSDN Azure i przepełnienie stosu](https://azure.microsoft.com/support/forums/). Alternatywnie można pliku zdarzenia pomocy technicznej platformy Azure. Przejdź do [witrynę pomocy technicznej platformy Azure](https://azure.microsoft.com/support/options/) i wybierz Uzyskaj pomoc techniczną. Aby uzyskać informacje o korzystaniu z platformy Azure obsługuje, przeczytaj [pomocy technicznej Microsoft Azure — często zadawane pytania](https://azure.microsoft.com/support/faq/).
