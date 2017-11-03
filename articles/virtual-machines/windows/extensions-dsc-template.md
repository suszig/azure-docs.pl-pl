---
title: "Żądany stan konfiguracji szablonu usługi Resource Manager | Dokumentacja firmy Microsoft"
description: "Definicja szablonu usługi Resource Manager konfiguracji żądanego stanu systemu Azure wraz z przykładami i rozwiązywania problemów"
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: b5402e5a-1768-4075-8c19-b7f7402687af
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 09/15/2016
ms.author: zachal
ms.openlocfilehash: 4292f9d8cd181073fdf0adff99fcb9624e0e9f55
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="windows-vmss-and-desired-state-configuration-with-azure-resource-manager-templates"></a>VMSS systemu Windows i konfiguracji żądanego stanu przy użyciu szablonów usługi Azure Resource Manager
W tym artykule opisano szablonu usługi Resource Manager dla [konfiguracji żądanego stanu rozszerzenia obsługi](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

## <a name="template-example-for-a-windows-vm"></a>Przykład szablonu maszyny wirtualnej systemu Windows
Poniższy fragment przechodzi w sekcji zasobów szablonu.

```json
            "name": "Microsoft.Powershell.DSC",
            "type": "extensions",
             "location": "[resourceGroup().location]",
             "apiVersion": "2015-06-15",
             "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": true,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "dscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }

```

## <a name="template-example-for-windows-vmss"></a>Przykład szablonu VMSS systemu Windows
Węzeł VMSS ma sekcję "właściwości" od "VirtualMachineProfile", "extensionProfile" atrybutu. DSC zostanie dodany w obszarze "rozszerzenia". 

```json
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": true,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
        }
```

## <a name="detailed-settings-information"></a>Informacje szczegółowe ustawienia
Następujące schemat jest przeznaczony dla części ustawień rozszerzenia usługi Konfiguracja DSC Azure w szablonie usługi Azure Resource Manager.

```json

"settings": {
  "wmfVersion": "latest",
  "configuration": {
    "url": "http://validURLToConfigLocation",
    "script": "ConfigurationScript.ps1",
    "function": "ConfigurationFunction"
  },
  "configurationArguments": {
    "argument1": "Value1",
    "argument2": "Value2"
  },
  "configurationData": {
    "url": "https://foo.psd1"
  },
  "privacy": {
    "dataCollection": "enable"
  },
  "advancedOptions": {
    "downloadMappings": {
      "customWmfLocation": "http://myWMFlocation"
    }
  } 
},
"protectedSettings": {
  "configurationArguments": {
    "parameterOfTypePSCredential1": {
      "userName": "UsernameValue1",
      "password": "PasswordValue1"
    },
    "parameterOfTypePSCredential2": {
      "userName": "UsernameValue2",
      "password": "PasswordValue2"
    }
  },
  "configurationUrlSasToken": "?g!bber1sht0k3n",
  "configurationDataUrlSasToken": "?dataAcC355T0k3N"
}

```

## <a name="details"></a>Szczegóły
| Nazwa właściwości | Typ | Opis |
| --- | --- | --- |
| settings.wmfVersion |Ciąg |Określa wersję Windows Management Framework, która ma zostać zainstalowana na maszynie Wirtualnej. Ustawienie tej właściwości "najnowsze" spowoduje zainstalowanie najnowszych wersji platformy WMF. Tylko bieżące możliwe wartości dla tej właściwości to **"4.0", "5.0", "5.0PP' i"r"**. Te wartości można podlegają aktualizacji. Wartość domyślna to "najnowsze". |
| Settings.Configuration.URL |Ciąg |Określa adres URL lokalizacji, z którego można pobrać pliku zip konfiguracji DSC. Podany adres URL wymaga tokenu sygnatury dostępu Współdzielonego w celu uzyskania dostępu, należy ustawić właściwość protectedSettings.configurationUrlSasToken wartość token sygnatury dostępu Współdzielonego. Ta właściwość jest wymagana, jeśli zdefiniowano settings.configuration.script i/lub settings.configuration.function. |
| Settings.Configuration.Script |Ciąg |Określa nazwę pliku skryptu, który zawiera definicję konfiguracji DSC. Ten skrypt musi być w folderze głównym pliku zip pobranego z adresu URL określonego przez właściwość configuration.url. Ta właściwość jest wymagana, jeśli zdefiniowano settings.configuration.url i/lub settings.configuration.script. |
| Settings.Configuration.Function |Ciąg |Określa nazwę konfiguracji DSC. Konfigurację o nazwie muszą być zawarte w skrypcie zdefiniowane przez configuration.script. Ta właściwość jest wymagana, jeśli zdefiniowano settings.configuration.url i/lub settings.configuration.function. |
| settings.configurationArguments |Collection |Definiuje parametry, których chcesz przekazać do konfiguracji DSC. Ta właściwość nie jest zaszyfrowany. |
| settings.configurationData.url |Ciąg |Określa adres URL do pobrania pliku (psd1) danych konfiguracji do użycia jako dane wejściowe dla danej konfiguracji DSC. Podany adres URL wymaga tokenu sygnatury dostępu Współdzielonego w celu uzyskania dostępu, należy ustawić właściwość protectedSettings.configurationDataUrlSasToken wartość token sygnatury dostępu Współdzielonego. |
| settings.privacy.dataEnabled |Ciąg |Włącza lub wyłącza gromadzenia danych telemetrii. Są to tylko możliwe wartości dla tej właściwości **"Włącz", "Wyłączone", ", lub $null**. Puste ani mieć wartości null, pozostawiając ta właściwość umożliwia telemetrii. Wartość domyślna to ". [Więcej informacji](https://blogs.msdn.microsoft.com/powershell/2016/02/02/azure-dsc-extension-data-collection-2/) |
| settings.advancedOptions.downloadMappings |Collection |Definiuje alternatywnej lokalizacji, z których można pobrać WMF. [Więcej informacji](http://blogs.msdn.com/b/powershell/archive/2015/10/21/azure-dsc-extension-2-2-amp-how-to-map-downloads-of-the-extension-dependencies-to-your-own-location.aspx) |
| protectedSettings.configurationArguments |Collection |Definiuje parametry, których chcesz przekazać do konfiguracji DSC. Ta właściwość jest zaszyfrowany. |
| protectedSettings.configurationUrlSasToken |Ciąg |Określa tokenu sygnatury dostępu Współdzielonego dostępu do adresu URL, zdefiniowane przez configuration.url. Ta właściwość jest zaszyfrowany. |
| protectedSettings.configurationDataUrlSasToken |Ciąg |Określa tokenu sygnatury dostępu Współdzielonego dostępu do adresu URL, zdefiniowane przez configurationData.url. Ta właściwość jest zaszyfrowany. |

## <a name="settings-vs-protectedsettings"></a>Ustawienia programu vs. ProtectedSettings
Wszystkie ustawienia są zapisywane w pliku tekstowym ustawień na maszynie Wirtualnej.
Właściwości w obszarze "ustawienia" są właściwości publiczne, ponieważ nie są szyfrowane w pliku tekstowym ustawienia.
Właściwości w obszarze "protectedSettings" są szyfrowane przy użyciu certyfikatu i nie są wyświetlane w postaci zwykłego tekstu, w tym pliku na maszynie Wirtualnej.

Jeśli konfiguracja wymaga poświadczeń, mogły one zostać uwzględnione w protectedSettings:

```json
"protectedSettings": {
    "configurationArguments": {
        "parameterOfTypePSCredential1": {
               "userName": "UsernameValue1",
               "password": "PasswordValue1"
        }
    }
}
```

## <a name="example"></a>Przykład
Poniższy przykład pochodzi z sekcji "Wprowadzenie" [strony Przegląd programu obsługi rozszerzenia DSC](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
W tym przykładzie używane szablony Menedżera zasobów zamiast polecenia cmdlet, aby wdrożyć rozszerzenie. Zapisz konfigurację "IisInstall.ps1", umieść ją w. Plik ZIP i przekazać plik w dostępny adres URL. W tym przykładzie użyto magazynu obiektów blob platformy Azure, ale istnieje możliwość pobrania. Pliki ZIP z dowolnego miejsca i dowolnego.

W szablonie usługi Azure Resource Manager następujący kod powoduje, że maszyna wirtualna może pobrać poprawnego pliku i uruchom odpowiednią funkcję programu PowerShell:

```json
"settings": {
    "configuration": {
        "url": "https://demo.blob.core.windows.net/",
        "script": "IisInstall.ps1",
        "function": "IISInstall"
    }
    } 
},
"protectedSettings": {
    "configurationUrlSasToken": "odLPL/U1p9lvcnp..."
}
```

## <a name="updating-from-the-previous-format"></a>Aktualizowanie z poprzedniego formatu
Wszystkie ustawienia w poprzednim formacie (zawierający właściwości publiczne ModulesUrl, ConfigurationFunction, właściwości lub SasToken) automatycznie dostosowania do bieżącego formatu i uruchom tak samo jak przed.

Następujące schemat jest jakie poprzednie ustawienia schematu wyszukiwanego, takich jak:

```json
"settings": {
    "WMFVersion": "latest",
    "ModulesUrl": "https://UrlToZipContainingConfigurationScript.ps1.zip",
    "SasToken": "SAS Token if ModulesUrl points to private Azure Blob Storage",
    "ConfigurationFunction": "ConfigurationScript.ps1\\ConfigurationFunction",
    "Properties":  {
        "ParameterToConfigurationFunction1":  "Value1",
        "ParameterToConfigurationFunction2":  "Value2",
        "ParameterOfTypePSCredential1": {
            "UserName": "UsernameValue1",
            "Password": "PrivateSettingsRef:Key1" 
        },
        "ParameterOfTypePSCredential2": {
            "UserName": "UsernameValue2",
            "Password": "PrivateSettingsRef:Key2"
        }
    }
},
"protectedSettings": { 
    "Items": {
        "Key1": "PasswordValue1",
        "Key2": "PasswordValue2"
    },
    "DataBlobUri": "https://UrlToConfigurationDataWithOptionalSasToken.psd1"
}
```

Oto, jak poprzedni format dostosowuje się do bieżącego formatu:

| Nazwa właściwości | Odpowiednik w poprzednim schematu |
| --- | --- |
| settings.wmfVersion |Ustawienia. WMFVersion |
| Settings.Configuration.URL |Ustawienia. ModulesUrl |
| Settings.Configuration.Script |Pierwsza część ustawień. ConfigurationFunction (przed "\\\\") |
| Settings.Configuration.Function |Druga część ustawień. ConfigurationFunction (po "\\\\") |
| settings.configurationArguments |Ustawienia. Właściwości |
| settings.configurationData.url |protectedSettings.DataBlobUri (bez tokenu sygnatury dostępu Współdzielonego) |
| settings.privacy.dataEnabled |Ustawienia. Privacy.DataEnabled |
| settings.advancedOptions.downloadMappings |Ustawienia. AdvancedOptions.DownloadMappings |
| protectedSettings.configurationArguments |protectedSettings.Properties |
| protectedSettings.configurationUrlSasToken |Ustawienia. SasToken |
| protectedSettings.configurationDataUrlSasToken |Token sygnatury dostępu Współdzielonego z protectedSettings.DataBlobUri |

## <a name="troubleshooting---error-code-1100"></a>Rozwiązywanie problemów — kod błędu: 1100
Kod błędu 1100 wskazuje, że istnieje problem z danych wejściowych użytkownika do rozszerzenia usługi Konfiguracja DSC.
Tekst tych błędów jest zmienną i mogą ulec zmianie.
Poniżej przedstawiono niektóre błędy, które może napotkać oraz jak je naprawić.

### <a name="invalid-values"></a>Nieprawidłowe wartości
"Jest Privacy.dataCollection"{0}". Tylko możliwe wartości to ","Włącz"i"Wyłącz"" "jest WmfVersion"{0}". Tylko możliwe wartości to... i "najnowsze" "

Problem: Podana wartość nie jest dozwolona.

Rozwiązanie: Zmień nieprawidłową wartość na prawidłową wartość. Poniższa tabela w sekcji szczegółów.

### <a name="invalid-url"></a>Nieprawidłowy adres URL
"Jest ConfigurationData.url"{0}". Nie jest prawidłowym adresem URL""jest DataBlobUri "{0}". Nie jest prawidłowym adresem URL""jest Configuration.url "{0}". Nie jest prawidłowym adresem URL"

Problem: A pod warunkiem, że adres URL jest nieprawidłowy.

Rozwiązanie: Sprawdź wszystkie podane adresami URL. Upewnij się, że wszystkie adresy URL rozwiązania do prawidłowej lokalizacji, czy rozszerzenia dostęp do komputera zdalnego.

### <a name="invalid-configurationargument-type"></a>Nieprawidłowy typ ConfigurationArgument
"ConfigurationArguments nieprawidłowy typ" {0}

Problem: Właściwość ConfigurationArguments nie można rozpoznać obiektu Hashtable. 

Rozwiązanie: Upewnij się z właściwości ConfigurationArguments obiektu Hashtable. Postępuj zgodnie z formatu podano w przykładzie poprzedzających. Zwróć uwagę na cudzysłowy, przecinki oraz nawiasy klamrowe.

### <a name="duplicate-configurationarguments"></a>Zduplikowana ConfigurationArguments
"Znaleziono zduplikowane argumentów"{0}"w publicznych i chronionych configurationArguments"

Problem: ConfigurationArguments w ustawieniach publicznego i ConfigurationArguments w chronionych ustawień zawiera właściwości o tej samej nazwie.

Rozwiązanie: Usuń jeden z tych właściwości.

### <a name="missing-properties"></a>Brak właściwości
"Configuration.function wymaga configuration.url lub configuration.module określono"

"Configuration.url wymaga się, że określono tego configuration.script"

"Configuration.script wymaga się, że określono tego configuration.url"

"Configuration.url wymaga się, że określono tego configuration.function"

"ConfigurationUrlSasToken wymaga się, że określono tego configuration.url"

"ConfigurationDataUrlSasToken wymaga się, że określono tego configurationData.url"

Problem: Zdefiniowanych właściwości wymaga innej właściwości, która nie istnieje.

Rozwiązania: 

* Podaj brakuje właściwości.
* Usuń właściwość, która wymaga brakuje właściwości.

## <a name="next-steps"></a>Następne kroki
Dowiedz się więcej o konfiguracji DSC i zestawach skali maszyn wirtualnych [za pomocą zestawów skali maszyny wirtualnej z rozszerzenia DSC Azure](../../virtual-machine-scale-sets/virtual-machine-scale-sets-dsc.md)

Więcej szczegółów znajduje się na [zarządzania poświadczeniami bezpiecznego DSC](extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aby uzyskać więcej informacji dotyczących obsługi rozszerzenia usługi Konfiguracja DSC Azure, zobacz [wprowadzenie do obsługi rozszerzenia konfiguracji żądanego stanu Azure](extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Aby uzyskać więcej informacji o konfiguracji DSC środowiska PowerShell [odwiedź Centrum dokumentacji programu PowerShell](https://msdn.microsoft.com/powershell/dsc/overview). 

