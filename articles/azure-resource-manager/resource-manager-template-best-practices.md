---
title: "Najlepsze rozwiązania dotyczące tworzenia szablonów usługi Resource Manager | Dokumentacja firmy Microsoft"
description: "Wytyczne dotyczące uproszczenia szablonów usługi Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 31b10deb-0183-47ce-a5ba-6d0ff2ae8ab3
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: tomfitz
ms.openlocfilehash: a23301ba88279af3f7bf4d353ae808e9eeb0900d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="best-practices-for-creating-azure-resource-manager-templates"></a>Najlepsze rozwiązania dotyczące tworzenia szablonów usługi Azure Resource Manager
Te wskazówki ułatwiają tworzenie szablonów usługi Azure Resource Manager, które są niezawodne i łatwe w użyciu. Wytyczne są tylko sugestie. Nie są one wymagania, oprócz wskazanych wyjątków. Scenariusz może wymagać zmiany jednego z następujących metod lub przykłady.

## <a name="resource-names"></a>Nazwy zasobów
Ogólnie rzecz biorąc pracować z trzech rodzajów nazw zasobów w Menedżerze zasobów:

* Nazwy zasobów, które muszą być unikatowe.
* Nazwy zasobów, które nie muszą być unikatowe, ale zdecydować się na Podaj nazwę, która może pomóc w identyfikacji zasobu na podstawie kontekstu.
* Nazwy zasobów, które mogą być ogólne.

 Aby uzyskać informacje o ograniczeniach nazw zasobów, zobacz [konwencje nazewnictwa dla zasobów platformy Azure zalecane](../guidance/guidance-naming-conventions.md).

### <a name="unique-resource-names"></a>Nazwy zasobów unikatowy
Musisz podać nazwę zasobu unikatowy dla dowolnego typu zasobu, która zawiera punkt końcowy dostępu do danych. Niektóre typowe typy zasobów, które wymagają unikatową nazwę obejmują:

* Usługa Azure Storage<sup>1</sup> 
* Funkcje aplikacji internetowych w usłudze Azure App Service
* Oprogramowanie SQL Server
* W usłudze Azure Key Vault
* Azure Redis Cache
* Azure Batch
* Azure Traffic Manager
* Azure Search
* Usługa Azure HDInsight

<sup>1</sup> nazwy konta magazynu musi być także małe litery, 24 znaków lub mniej, a nie ma żadnych łączników.

Jeśli podasz nazwę zasobu parametr, Podaj unikatową nazwę podczas wdrażania zasobu. Opcjonalnie można utworzyć zmiennej, która używa [uniqueString()](resource-group-template-functions-string.md#uniquestring) funkcji, aby wygenerować nazwy. 

Można również dodać prefiksu lub sufiksu domeny do **uniqueString** wynik. Modyfikowanie unikatową nazwę może pomóc więcej łatwo zidentyfikować z nazwy typu zasobu. Na przykład można wygenerować unikatowej nazwy dla konta magazynu przy użyciu następującą zmienną:

```json
"variables": {
    "storageAccountName": "[concat(uniqueString(resourceGroup().id),'storage')]"
}
```

### <a name="resource-names-for-identification"></a>Nazwy zasobów do identyfikacji
Niektóre typy zasobów, które mają nazwy, ale ich nazwy nie muszą być unikatowe. W przypadku tych typów zasobów można Podaj nazwę, która identyfikuje kontekst zasobów, jak i typ zasobu. Wprowadź opisową nazwę, która pomaga w identyfikacji zasobu na liście zasobów. Jeśli musisz użyć inną nazwę zasobu dla różnych wdrożeń, można użyć parametru dla nazwy:

```json
"parameters": {
    "vmName": { 
        "type": "string",
        "defaultValue": "demoLinuxVM",
        "metadata": {
            "description": "The name of the VM to create."
        }
    }
}
```

Jeśli nie trzeba przekazać nazwę podczas wdrażania, można użyć zmiennej: 

```json
"variables": {
    "vmName": "demoLinuxVM"
}
```

Można także użyć wartości stałe:

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  "name": "demoLinuxVM",
  ...
}
```

### <a name="generic-resource-names"></a>Nazwy zasobów ogólnych
Dla typów zasobów, które przede wszystkim uzyskują dostęp za pomocą innego zasobu można użyć nazwy ogólnej, który jest ustalony w szablonie. Na przykład można ustawić nazwę standardowego, ogólne reguły zapory na serwerze SQL server:

```json
{
    "type": "firewallrules",
    "name": "AllowAllWindowsAzureIps",
    ...
}
```

## <a name="parameters"></a>Parametry
Poniższe informacje mogą być przydatne podczas pracy z parametrami:

* Zminimalizować korzystanie z parametrów. Jeśli to możliwe, należy użyć zmiennej lub wartość literału. Parametry tylko dla tych scenariuszy:
   
   * Ustawienia, które chcesz użyć zmian zgodnie ze środowiska (jednostka SKU, rozmiar, pojemności).
   * Nazwy zasobów, które chcesz określić łatwo zidentyfikować.
   * Wartości, które są często używane do wykonywania innych zadań (takie jak nazwa użytkownika administratora).
   * Klucze tajne (takie jak hasła).
   * Numer lub tablicy wartości, które do użycia podczas tworzenia wielu wystąpień typu zasobu.
* Użyj formatu camelcase dla nazw parametrów.
* Podaj opis każdego parametru w metadanych:

   ```json
   "parameters": {
       "storageAccountType": {
           "type": "string",
           "metadata": {
               "description": "The type of the new storage account created to store the VM disks."
           }
       }
   }
   ```

* Zdefiniuj wartości domyślne parametrów (z wyjątkiem hasła i klucze SSH):
   
   ```json
   "parameters": {
        "storageAccountType": {
            "type": "string",
            "defaultValue": "Standard_GRS",
            "metadata": {
                "description": "The type of the new storage account created to store the VM disks."
            }
        }
   }
   ```

* Użyj **SecureString** dla wszystkich hasła i klucze tajne: 
   
   ```json
   "parameters": {
       "secretValue": {
           "type": "securestring",
           "metadata": {
               "description": "The value of the secret to store in the vault."
           }
       }
   }
   ```

* Jeśli to możliwe, nie używaj parametru do określenia lokalizacji. Zamiast tego należy użyć **lokalizacji** właściwości grupy zasobów. Za pomocą **resourceGroup () .location** wyrażenie dla wszystkich zasobów, zasobów w szablonie są wdrażane w tej samej lokalizacji co grupa zasobów:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         ...
     }
   ]
   ```
   
   Typ zasobu jest obsługiwana w ograniczonej liczby miejsc, można określić prawidłowej lokalizacji bezpośrednio w szablonie. Jeśli musisz użyć **lokalizacji** parametru udostępniać tę wartość parametru możliwie zasobów, które mogą być w tej samej lokalizacji. Pozwala to zmniejszyć liczbę razy, które użytkownicy są proszeni o dostarczenie informacji o lokalizacji.
* Unikaj używania parametr lub zmienna dla wersji interfejsu API dla typu zasobu. Właściwości zasobów i wartości może się różnić przez numer wersji. IntelliSense w edytorze kodu nie można ustalić prawidłowego schematu, gdy parametr lub zmienna ma wartość wersja interfejsu API. Zamiast tego wersji interfejsu API zakodowane w szablonie.

## <a name="variables"></a>Zmienne
Następujące informacje mogą być pomocne podczas pracy ze zmiennymi:

* Użyj zmienne dla wartości, które trzeba użyć więcej niż raz w szablonie. Jeśli wartość jest używana tylko raz, wartość ustalony ułatwia szablonu do odczytu.
* Nie można użyć [odwołania](resource-group-template-functions-resource.md#reference) działać w **zmienne** sekcji szablonu. **Odwołania** funkcja pochodzi wartość ze stanu środowiska uruchomieniowego zasobu. Zmienne są jednak rozpoznane podczas początkowej analizy szablonu. Konstrukcja wartości wymagających **odwołania** bezpośrednio w funkcji **zasobów** lub **generuje** sekcji szablonu.
* Obejmują zmienne dla nazw zasobów, które muszą być unikatowe, zgodnie z opisem w [nazw zasobów](#resource-names).
* Zmienne można grupować w obiektów złożonych. Użyj **variable.subentry** format, aby odwołać się do wartości z obiektu złożonego. Grupowania zmiennych może ułatwić śledzenie powiązanych zmiennych. On również poprawia czytelność szablonu. Oto przykład:
   
   ```json
   "variables": {
       "storage": {
           "name": "[concat(uniqueString(resourceGroup().id),'storage')]",
           "type": "Standard_LRS"
       }
   },
   "resources": [
     {
         "type": "Microsoft.Storage/storageAccounts",
         "name": "[variables('storage').name]",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "sku": {
             "name": "[variables('storage').type]"
         },
         ...
     }
   ]
   ```
   
   > [!NOTE]
   > Obiekt złożony nie może zawierać wyrażenia, który odwołuje się do wartości z obiektu złożonego. W tym celu zdefiniowane osobnej zmiennej.
   > 
   > 
   
     Przykłady zaawansowanych za pomocą obiektu złożonego jako zmiennych, zobacz [udostępniania stanu w szablonach usługi Azure Resource Manager](best-practices-resource-manager-state.md).

## <a name="resources"></a>Zasoby
Poniższe informacje mogą być przydatne podczas pracy z zasobami:

* Aby ułatwić innych uczestników zrozumienie przeznaczenia zasobu, należy określić **komentarze** dla każdego zasobu w szablonie:
   
   ```json
   "resources": [
     {
         "name": "[variables('storageAccountName')]",
         "type": "Microsoft.Storage/storageAccounts",
         "apiVersion": "2016-01-01",
         "location": "[resourceGroup().location]",
         "comments": "This storage account is used to store the VM disks.",
         ...
     }
   ]
   ```

* Tagów można użyć w celu dodania metadanych do zasobów. Umożliwia dodawanie informacji o zasobach w metadanych. Na przykład można dodać metadanych do rejestrowania szczegółów rozliczeń dla zasobu. Aby uzyskać więcej informacji, zobacz [organizowania zasobów na platformie Azure przy użyciu tagów](resource-group-using-tags.md).
* Jeśli używasz *publiczny punkt końcowy* w szablonie (np. Azure Blob magazynu publiczny punkt końcowy), *czy nie kodowane* przestrzeni nazw. Użyj **odwołania** funkcji można dynamicznie pobrać przestrzeni nazw. Tej metody można użyć do wdrożenia szablonu w środowiskach różnych publicznej przestrzeni nazw ręcznie zmieniając punktu końcowego w szablonie. Ustaw wersję interfejsu API do tej samej wersji, którego używasz konta magazynu w szablonie:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Jeśli konto magazynu jest wdrażana w tym samym szablonie, który tworzysz, nie należy określić przestrzeń nazw dostawcy podczas odwoływać się do zasobu. Jest to uproszczoną składnię:
   
   ```json
   "osDisk": {
       "name": "osdisk",
       "vhd": {
           "uri": "[concat(reference(variables('storageAccountName'), '2016-01-01').primaryEndpoints.blob, variables('vmStorageAccountContainerName'), '/',variables('OSDiskName'),'.vhd')]"
       }
   }
   ```
   
   Jeśli masz inne wartości w szablonie, które są skonfigurowane do korzystania z publicznej przestrzeni nazw, zmiany tych wartości, aby odzwierciedlić takie same **odwołania** funkcji. Na przykład można ustawić **storageUri** właściwości profilu diagnostyki maszyny wirtualnej:
   
   ```json
   "diagnosticsProfile": {
       "bootDiagnostics": {
           "enabled": "true",
           "storageUri": "[reference(concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName')), '2016-01-01').primaryEndpoints.blob]"
       }
   }
   ```
   
   Możesz także odwoływać się do istniejącego konta magazynu, który znajduje się w innej grupie zasobów:

   ```json
   "osDisk": {
       "name": "osdisk", 
       "vhd": {
           "uri":"[concat(reference(resourceId(parameters('existingResourceGroup'), 'Microsoft.Storage/storageAccounts/', parameters('existingStorageAccountName')), '2016-01-01').primaryEndpoints.blob,  variables('vmStorageAccountContainerName'), '/', variables('OSDiskName'),'.vhd')]"
       }
   }
   ```

* Przypisz publiczne adresy IP do maszyny wirtualnej, tylko wtedy, gdy aplikacja wymaga on. Aby połączyć się z maszyną wirtualną (VM) do debugowania lub zarządzania lub celów administracyjnych, użyj reguły NAT ruchu przychodzącego, bramy sieci wirtualnej lub jumpbox.
   
     Aby uzyskać więcej informacji na temat podłączania do maszyn wirtualnych zobacz:
   
   * [Uruchamianie maszyn wirtualnych dla architektury N-warstwowa na platformie Azure](../guidance/guidance-compute-n-tier-vm.md)
   * [Konfigurowanie dostępu do usługi WinRM dla maszyn wirtualnych w usłudze Azure Resource Manager](../virtual-machines/windows/winrm.md)
   * [Dostęp do zewnętrznych do maszyny Wirtualnej za pomocą portalu Azure](../virtual-machines/windows/nsg-quickstart-portal.md)
   * [Dostęp do zewnętrznych do maszyny Wirtualnej za pomocą programu PowerShell](../virtual-machines/windows/nsg-quickstart-powershell.md)
   * [Dostęp do zewnętrznych sieci maszyny Wirtualnej systemu Linux przy użyciu wiersza polecenia platformy Azure](../virtual-machines/virtual-machines-linux-nsg-quickstart.md)
* **DomainNameLabel** właściwości dla publicznych adresów IP muszą być unikatowe. **DomainNameLabel** wartość musi mieć długość od 3 do 63 znaków i postępuj zgodnie z regułami określonymi przez tego wyrażenia regularnego: `^[a-z][a-z0-9-]{1,61}[a-z0-9]$`. Ponieważ **uniqueString** funkcja generuje ciąg, który jest 13 znaków **dnsPrefixString** parametr jest ograniczone do 50 znaków:

   ```json
   "parameters": {
       "dnsPrefixString": {
           "type": "string",
           "maxLength": 50,
           "metadata": {
               "description": "The DNS label for the public IP address. It must be lowercase. It should match the following regular expression, or it will raise an error: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$"
           }
       }
   },
   "variables": {
       "dnsPrefix": "[concat(parameters('dnsPrefixString'),uniquestring(resourceGroup().id))]"
   }
   ```

* Gdy hasło jest dodawanie do rozszerzenia niestandardowego skryptu, użyj **commandToExecute** właściwości w **protectedSettings** właściwości:
   
   ```json
   "properties": {
       "publisher": "Microsoft.Azure.Extensions",
       "type": "CustomScript",
       "typeHandlerVersion": "2.0",
       "autoUpgradeMinorVersion": true,
       "settings": {
           "fileUris": [
               "[concat(variables('template').assets, '/lamp-app/install_lamp.sh')]"
           ]
       },
       "protectedSettings": {
           "commandToExecute": "[concat('sh install_lamp.sh ', parameters('mySqlPassword'))]"
       }
   }
   ```
   
   > [!NOTE]
   > Aby upewnić się, że hasła są szyfrowane, gdy są one przekazywane jako parametry do maszyn wirtualnych i rozszerzenia, należy użyć **protectedSettings** właściwości odpowiednich rozszerzeń.
   > 
   > 

## <a name="outputs"></a>dane wyjściowe
Jeśli używasz szablonu do utworzenia publiczne adresy IP obejmują **generuje** sekcja, która zwraca szczegółowe informacje dotyczące adresu IP i w pełni kwalifikowaną nazwę (FQDN). Można użyć wartości danych wyjściowych można łatwo pobrać szczegółowe informacje dotyczące publicznych adresów IP i nazw FQDN po wdrożeniu. Gdy odwoływać się do zasobu, należy użyć wersja interfejsu API, który został użyty do utworzenia go: 

```json
"outputs": {
    "fqdn": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').dnsSettings.fqdn]",
        "type": "string"
    },
    "ipaddress": {
        "value": "[reference(resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName')), '2016-07-01').ipAddress]",
        "type": "string"
    }
}
```

## <a name="single-template-vs-nested-templates"></a>Jednego szablonu, a zagnieżdżone szablony
Do wdrażania rozwiązania, użyj jednego szablonu lub szablonu głównego z wielu szablonów zagnieżdżonych. Zagnieżdżone szablony są wspólne dla bardziej zaawansowanych scenariuszy. Przy użyciu szablonu zagnieżdżonych oferuje następujące korzyści:

* Można podzielić rozwiązania do elementów docelowych.
* Zagnieżdżone szablony z różnych szablonów głównego można użyć ponownie.

Jeśli wybierzesz zagnieżdżone szablony, poniższe wskazówki ułatwiają normalizacji projektu szablonu. Niniejsze wytyczne dotyczą [wzorce projektowania szablonów usługi Azure Resource Manager](best-practices-resource-manager-design-templates.md). Zaleca się projekt, który ma następujące szablony:

* **Główny szablon** (azuredeploy.json). Użycie parametrów wejściowych.
* **Udostępnione zasoby szablonu**. Umożliwia wdrażanie zasobów udostępnionych, korzystających z innych zasobów (na przykład wirtualnych sieci i dostępność zestawy). Użyj **dependsOn** wyrażenia, aby upewnić się, że ten szablon jest wdrażany przed innych szablonów.
* **Opcjonalne zasoby szablonu**. Umożliwia warunkowo wdrażanie zasobów na podstawie parametru (na przykład jumpbox).
* **Szablon elementu członkowskiego zasobów**. Każdy typ wystąpienie w warstwie aplikacji ma własny konfiguracji. W ramach warstwy można zdefiniować typy innego wystąpienia. (Na przykład pierwsze wystąpienie jest tworzony klaster, a dodatkowe wystąpienia są dodawane do istniejącego klastra.) Każdy typ wystąpienia ma własny szablon wdrożenia.
* **Skrypty**. Powszechnie wielokrotnego użytku skrypty są stosowane do każdego wystąpienia typu (na przykład, inicjowanie i formatowanie dodatkowe dyski). Niestandardowych skryptów utworzonych w celu dostosowania określonych są różne, na podstawie typu wystąpienia.

![Szablon zagnieżdżony](./media/resource-manager-template-best-practices/nestedTemplateDesign.png)

Aby uzyskać więcej informacji, zobacz [używać szablonów połączonych z usługą Azure Resource Manager](resource-group-linked-templates.md).

## <a name="conditionally-link-to-nested-templates"></a>Warunkowo łącza do szablonów zagnieżdżonych
Można użyć parametru warunkowo połączyć zagnieżdżone szablony. Parametr staje się częścią identyfikatora URI dla szablonu:

```json
"parameters": {
    "newOrExisting": {
        "type": "String",
        "allowedValues": [
            "new",
            "existing"
        ]
    }
},
"variables": {
    "templatelink": "[concat('https://raw.githubusercontent.com/Contoso/Templates/master/',parameters('newOrExisting'),'StorageAccount.json')]"
},
"resources": [
    {
        "apiVersion": "2015-01-01",
        "name": "nestedTemplate",
        "type": "Microsoft.Resources/deployments",
        "properties": {
            "mode": "incremental",
            "templateLink": {
                "uri": "[variables('templatelink')]",
                "contentVersion": "1.0.0.0"
            },
            "parameters": {
            }
        }
    }
]
```

## <a name="template-format"></a>Format szablonu
Jest dobrym rozwiązaniem do przekazania do szablonu przez moduł weryfikacji JSON. Moduł weryfikacji ułatwia Usuń nadmiarowe przecinkami, nawiasy i nawiasy, które może spowodować wystąpienie błędu podczas wdrażania. Spróbuj [JSONLint](http://jsonlint.com/) lub pakiet linter dla Ulubione edycji środowiska (Sublime tekstu Visual Studio Code, Atom, Visual Studio).

Jest również dobrym pomysłem jest format JSON sieci w celu zwiększenia czytelności. Pakiet programu formatującego JSON można użyć dla Edytora lokalnych. W programie Visual Studio, aby sformatować dokumentu, naciśnij klawisz **Ctrl + K, Ctrl + D**. W programie Visual Studio Code, naciśnij klawisz **Alt + Shift + F**. Edytor lokalnych nie formatowania dokumentu, można użyć [online program formatujący](https://www.bing.com/search?q=json+formatter).

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać wskazówki dotyczące projektowania rozwiązania dla maszyn wirtualnych, zobacz [Uruchom Maszynę wirtualną systemu Windows na platformie Azure](../guidance/guidance-compute-single-vm.md) i [Uruchom Maszynę wirtualną systemu Linux na platformie Azure](../guidance/guidance-compute-single-vm-linux.md).
* Aby uzyskać instrukcje na temat konfigurowania konta magazynu, zobacz [Lista kontrolna wydajności i skalowalności magazynu Azure](../storage/common/storage-performance-checklist.md).
* Aby dowiedzieć się, jak przedsiębiorstwa można użyć Menedżera zasobów wydajne zarządzanie subskrypcjami, zobacz [szkieletu Azure enterprise: ładu przetestowanego subskrypcji](resource-manager-subscription-governance.md).

