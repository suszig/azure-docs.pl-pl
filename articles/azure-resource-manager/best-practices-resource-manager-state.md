---
title: "Złożone wartości przekazywane między szablonami platformy Azure | Dokumentacja firmy Microsoft"
description: "Przedstawia zalecane podejście do danych o stanie udostępniać szablony usługi Azure Resource Manager i połączone przy użyciu obiektów złożonych."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: fd2f5e2d-d56d-4e01-a57d-34f3eaead4a9
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2016
ms.author: tomfitz
ms.openlocfilehash: e163f3c2e9a78b057dc2a7a42924c59d0aac3fab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="share-state-to-and-from-azure-resource-manager-templates"></a>Stan udziału do i z szablonów usługi Azure Resource Manager
W tym temacie przedstawiono najlepsze rozwiązania dotyczące zarządzania i udostępniania stanu w szablonach. Parametry i zmienne, które przedstawiono w tym temacie przedstawiono przykładowe typy obiektów, które można zdefiniować wygodny sposób organizowania wymagań dotyczących wdrożenia. W tych przykładach można zaimplementować obiekty z wartości właściwości odpowiednich dla danego środowiska.

Ten temat jest częścią większej oficjalny dokument. Aby uzyskać pełne papieru, Pobierz [światowej klasy zasobu menedżera szablony zagadnienia i sprawdzonych rozwiązań](http://download.microsoft.com/download/8/E/1/8E1DBEFA-CECE-4DC9-A813-93520A5D7CFE/World Class ARM Templates - Considerations and Proven Practices.pdf).

## <a name="provide-standard-configuration-settings"></a>Określ ustawienia konfiguracji standardowej
Zamiast oferują szablon, który zapewnia elastyczność całkowitej i niezliczonych zmian, wspólnego wzorca ma na celu dostarczenie wybór znanych konfiguracji. W efekcie, użytkownicy mogą wybrać standardowe rozmiary obrazów na koszulki, takich jak piaskownicy, małych, średnich i dużych. Inne przykłady rozmiary obrazów na koszulki są ofert produktów, takich jak community edition lub enterprise edition. W innych przypadkach może być konfiguracje specyficznego dla obciążenia technologii — takie jak ograniczyć mapy lub nie sql.

Z obiektów złożonych mogą tworzyć zmiennych, które zawierają zbiory danych, czasami znana jako "zbiory właściwości" i użyć tych danych do deklaracji zasobów na dysku w szablonie. Takie podejście zapewnia dobre, znanej konfiguracji o różnych rozmiarach, które są wstępnie skonfigurowane dla klientów. Bez znanych konfiguracji użytkowników szablonu musi ustalić klastra zmiany rozmiaru na ich własnych, uwzględnieniu ograniczenia zasobów platformy i wykonywanie obliczeń do identyfikowania wynikowy partycjonowanie konta magazynu i innych zasobów (ze względu na rozmiar klastra i zasoby ograniczenia). Oprócz tworzenia lepsze środowisko dla odbiorcy, kilka konfiguracji znane są łatwiejsze do obsługi i ułatwiają dostarczanie wyższego poziomu gęstości.

Poniższy przykład przedstawia sposób definiowania zmiennych, które zawierają złożone obiekty odpowiadające kolekcji danych. Kolekcje zdefiniuj wartości, które są używane dla rozmiaru maszyny wirtualnej, ustawienia sieciowe, ustawień systemu operacyjnego i ustawienia dostępności.

```json
"variables": {
  "tshirtSize": "[variables(concat('tshirtSize', parameters('tshirtSize')))]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  },
  "tshirtSizeMedium": {
    "vmSize": "Standard_A3",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-8disk-resources.json')]",
    "vmCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1],
      "jumpbox": 0
    }
  },
  "tshirtSizeLarge": {
    "vmSize": "Standard_A4",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-16disk-resources.json')]",
    "vmCount": 3,
    "slaveCount": 2,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 2,
      "pool": "db",
      "map": [0,1,1],
      "jumpbox": 0
    }
  },
  "osSettings": {
    "scripts": [
      "[concat(variables('templateBaseUrl'), 'install_postgresql.sh')]",
      "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/shared_scripts/ubuntu/vm-disk-utils-0.1.sh"
    ],
    "imageReference": {
      "publisher": "Canonical",
      "offer": "UbuntuServer",
      "sku": "14.04.2-LTS",
      "version": "latest"
    }
  },
  "networkSettings": {
    "vnetName": "[parameters('virtualNetworkName')]",
    "addressPrefix": "10.0.0.0/16",
    "subnets": {
      "dmz": {
        "name": "dmz",
        "prefix": "10.0.0.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      },
      "data": {
        "name": "data",
        "prefix": "10.0.1.0/24",
        "vnet": "[parameters('virtualNetworkName')]"
      }
    }
  },
  "availabilitySetSettings": {
    "name": "pgsqlAvailabilitySet",
    "fdCount": 3,
    "udCount": 5
  }
}
```

Zwróć uwagę, że **tshirtSize** zmiennej łączy Rozmiar koszulki udostępnianej parametr (**małych**, **średni**, **duży** ) w tekście **tshirtSize**. Ta zmienna służy do pobierania zmiennej skojarzony obiekt złożony dla tego rozmiar koszulki.

Następnie można odwoływać się tych zmiennych w dalszej części szablonu. Możliwość odwołania o nazwie zmienne i ich właściwości upraszcza składni szablonu i ułatwia zrozumienie kontekstu. W poniższym przykładzie zdefiniowano zasobów do wdrożenia przy użyciu obiektów pokazana wcześniej, aby ustawić wartości. Na przykład rozmiar maszyny Wirtualnej jest ustawiony przez pobranie wartość `variables('tshirtSize').vmSize` podczas wartość na rozmiar dysku jest pobierana z `variables('tshirtSize').diskSize`. Ponadto identyfikator URI dla połączonych szablonu jest ustawiony na wartość dla `variables('tshirtSize').vmTemplate`.

```json
"name": "master-node",
"type": "Microsoft.Resources/deployments",
"apiVersion": "2015-01-01",
"dependsOn": [
    "[concat('Microsoft.Resources/deployments/', 'shared')]"
],
"properties": {
    "mode": "Incremental",
    "templateLink": {
      "uri": "[variables('tshirtSize').vmTemplate]",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "adminPassword": {
        "value": "[parameters('adminPassword')]"
      },
      "replicatorPassword": {
        "value": "[parameters('replicatorPassword')]"
      },
      "osSettings": {
        "value": "[variables('osSettings')]"
      },
      "subnet": {
        "value": "[variables('networkSettings').subnets.data]"
      },
      "commonSettings": {
        "value": {
          "region": "[parameters('region')]",
          "adminUsername": "[parameters('adminUsername')]",
          "namespace": "ms"
        }
      },
      "storageSettings": {
        "value":"[variables('tshirtSize').storage]"
      },
      "machineSettings": {
        "value": {
          "vmSize": "[variables('tshirtSize').vmSize]",
          "diskSize": "[variables('tshirtSize').diskSize]",
          "vmCount": 1,
          "availabilitySet": "[variables('availabilitySetSettings').name]"
        }
      },
      "masterIpAddress": {
        "value": "0"
      },
      "dbType": {
        "value": "MASTER"
      }
    }
  }
}
```

## <a name="pass-state-to-a-template"></a>Stan powodzenia do szablonu
Możesz udostępniać stanu do szablonu za pomocą parametrów, które zapewniają bezpośrednio podczas wdrażania.

W poniższej tabeli wymieniono typowe parametry w szablonach.

| Nazwa | Wartość | Opis |
| --- | --- | --- |
| location |Ciąg z listy ograniczone regiony platformy Azure |Lokalizacja, w których są wdrożone zasoby. |
| storageAccountNamePrefix |Ciąg |Unikatowa nazwa DNS dla konta magazynu rozmieszczenia dysków maszyny Wirtualnej |
| domainName |Ciąg |Nazwa domeny jumpbox publicznie maszyny Wirtualnej w formacie: **{domainName}. { Lokalizacja}.cloudapp.com** na przykład: **mydomainname.westus.cloudapp.azure.com** |
| adminUsername |Ciąg |Nazwa użytkownika dla maszyn wirtualnych |
| adminPassword |Ciąg |Hasło dla maszyn wirtualnych |
| tshirtSize |Rozmiary obrazów na koszulki oferowane ciąg z listy ograniczone |Rozmiar jednostki skalowania o nazwie do udostępniania. Na przykład "Małe", "Medium", "Duże" |
| virtualNetworkName |Ciąg |Nazwa sieci wirtualnej, które użytkownik chce używać. |
| enableJumpbox |Ciąg z listą ograniczone (włączone/wyłączone) |Parametr, który określa, czy włączyć jumpbox dla środowiska. Wartości: "enabled", "wyłączone" |

**TshirtSize** parametru użytego w poprzedniej sekcji jest zdefiniowany jako:

```json
"parameters": {
  "tshirtSize": {
    "type": "string",
    "defaultValue": "Small",
    "allowedValues": [
      "Small",
      "Medium",
      "Large"
    ],
    "metadata": {
      "Description": "T-shirt size of the MongoDB deployment"
    }
  }
}
```

## <a name="pass-state-to-linked-templates"></a>Przekaż stan połączonego szablonów
Podczas nawiązywania połączenia połączonej szablony, można często używać różnych statyczne i wygenerować zmienne.

### <a name="static-variables"></a>Zmienne statyczne
Zmienne statyczne są często używane w celu zapewnienia podstawowej wartości, takie jak adresy URL, które są używane w szablonie.

W poniższym fragmencie szablonu `templateBaseUrl` Określa katalog główny dla szablonu w witrynie GitHub. Następnego wiersza tworzy nową zmienną `sharedTemplateUrl` który łączy podstawowy adres URL ze znanej nazwy szablonu zasobów udostępnionych. Poniżej tego wiersza, zmienna obiekt złożony jest używany do przechowywania Rozmiar koszulki, gdy podstawowy adres URL jest połączony do lokalizacji szablonu znanej konfiguracji i przechowywane w `vmTemplate` właściwości.

Zaletą tej metody jest, że zmiana lokalizacji szablonu wystarczy zmienić zmienna statyczna w jednym miejscu, która przechodzi on w całym szablonów połączonych.

```json
"variables": {
  "templateBaseUrl": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/postgresql-on-ubuntu/",
  "sharedTemplateUrl": "[concat(variables('templateBaseUrl'), 'shared-resources.json')]",
  "tshirtSizeSmall": {
    "vmSize": "Standard_A1",
    "diskSize": 1023,
    "vmTemplate": "[concat(variables('templateBaseUrl'), 'database-2disk-resources.json')]",
    "vmCount": 2,
    "slaveCount": 1,
    "storage": {
      "name": "[parameters('storageAccountNamePrefix')]",
      "count": 1,
      "pool": "db",
      "map": [0,0],
      "jumpbox": 0
    }
  }
}
```

### <a name="generated-variables"></a>Wygenerowany zmiennych
Oprócz zmienne statyczne kilku zmiennych są generowane dynamicznie. W tej sekcji wymieniono niektórych typowych wygenerowanego zmiennych.

#### <a name="tshirtsize"></a>tshirtSize
Znasz tej zmiennej wygenerowanych w powyższych przykładach.

#### <a name="networksettings"></a>networkSettings
W wydajność, możliwości lub szablon rozwiązania zakresami end-to-end połączone szablony zwykle Utwórz zasoby, które istnieją w sieci. Jednym z podejść prostego jest złożone obiekt używany do przechowywania ustawień sieci i przekazują je do połączonego szablonów.

Przykład komunikacji ustawienia sieciowe są widoczne poniżej.

```json
"networkSettings": {
  "vnetName": "[parameters('virtualNetworkName')]",
  "addressPrefix": "10.0.0.0/16",
  "subnets": {
    "dmz": {
      "name": "dmz",
      "prefix": "10.0.0.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    },
    "data": {
      "name": "data",
      "prefix": "10.0.1.0/24",
      "vnet": "[parameters('virtualNetworkName')]"
    }
  }
}
```

#### <a name="availabilitysettings"></a>availabilitySettings
Zasoby utworzone w szablonach połączonego często są umieszczane w zestawie dostępności. W poniższym przykładzie jest określona nazwa zbioru dostępności, a także liczba domeny błędów i Aktualizacja domeny do użycia.

```json
"availabilitySetSettings": {
  "name": "pgsqlAvailabilitySet",
  "fdCount": 3,
  "udCount": 5
}
```

Jeśli potrzebujesz wiele zestawów dostępności (na przykład, jeden dla węzłów głównych) i drugi dla węzłów danych, można użyć jako prefiksu nazwy, określ wiele zestawów dostępności lub postępuj zgodnie z modelem przedstawiona wcześniej do utworzenia zmiennej w określonym Rozmiar koszulki.

#### <a name="storagesettings"></a>storageSettings
Szczegóły magazynu często są współużytkowane z szablonami połączony. W poniższym przykładzie *storageSettings* obiektu zawiera szczegółowe informacje dotyczące nazwy konta i kontener magazynu.

```json
"storageSettings": {
    "vhdStorageAccountName": "[parameters('storageAccountName')]",
    "vhdContainerName": "[variables('vmStorageAccountContainerName')]",
    "destinationVhdsContainer": "[concat('https://', parameters('storageAccountName'), variables('vmStorageAccountDomain'), '/', variables('vmStorageAccountContainerName'), '/')]"
}
```

#### <a name="ossettings"></a>osSettings
Z szablonami połączone konieczne może być przekazać ustawień systemu operacyjnego do różnych typów węzłów w innej konfiguracji znanych typów. Obiekt złożony jest łatwo przechowywać i udostępniać informacje dotyczące systemu operacyjnego i również ułatwia obsługują wiele wyborów systemu operacyjnego dla wdrożenia.

W poniższym przykładzie przedstawiono dla obiekt *osSettings*:

```json
"osSettings": {
  "imageReference": {
    "publisher": "Canonical",
    "offer": "UbuntuServer",
    "sku": "14.04.2-LTS",
    "version": "latest"
  }
}
```

#### <a name="machinesettings"></a>machineSettings
Generowana zmienna, *machineSettings* jest obiekt złożony, zawierające kombinację zmiennych core dla tworzenia maszyny Wirtualnej. Zmienne to nazwa użytkownika administratora i hasła, prefiks dla nazw maszyn wirtualnych i odwołanie do obrazu systemu operacyjnego.

```json
"machineSettings": {
    "adminUsername": "[parameters('adminUsername')]",
    "adminPassword": "[parameters('adminPassword')]",
    "machineNamePrefix": "mongodb-",
    "osImageReference": {
        "publisher": "[variables('osFamilySpec').imagePublisher]",
        "offer": "[variables('osFamilySpec').imageOffer]",
        "sku": "[variables('osFamilySpec').imageSKU]",
        "version": "latest"
    }
},
```

Należy pamiętać, że *osImageReference* pobiera wartości z *osSettings* zmiennej zdefiniowanej w szablonie głównym. Oznacza to, że można łatwo zmienić system operacyjny dla maszyny Wirtualnej — całkowicie lub, w zależności od preferencji konsumenta szablonu.

#### <a name="vmscripts"></a>vmScripts
*VmScripts* obiekt zawiera szczegółowe informacje o skrypty w celu pobierania i wykonywania w wystąpieniu maszyny Wirtualnej, w tym odniesienia i wewnętrznego. Poza odwołaniami obejmują infrastruktury.
Wewnątrz odwołania zawierać zainstalowanego oprogramowania zainstalowanego i konfiguracji.

Możesz użyć *scriptsToDownload* właściwość, aby wyświetlić skryptów do pobrania z maszyną wirtualną. Ten obiekt zawiera również odwołania do argumentów wiersza polecenia dla różnego rodzaju akcje. Czynności te obejmują wykonywanie domyślnej instalacji dla każdego indywidualnego węzła, instalacja wykonywana po wdrożeniu wszystkich węzłów i dodatkowych skryptów, które mogą być specyficzne dla danego szablonu.

W tym przykładzie jest używany do wdrażania bazy danych MongoDB, co wymaga kryterium zapewniających wysoką dostępność szablonu. *ArbiterNodeInstallCommand* został dodany do *vmScripts* zainstalować kryterium.

W sekcji zmiennych jest, gdzie znaleźć definiujące określony tekst można wykonać skryptu za pomocą odpowiednich wartości zmiennych.

```json
"vmScripts": {
    "scriptsToDownload": [
        "[concat(variables('scriptUrl'), 'mongodb-', variables('osFamilySpec').osName, '-install.sh')]",
        "[concat(variables('sharedScriptUrl'), 'vm-disk-utils-0.1.sh')]"
    ],
    "regularNodeInstallCommand": "[variables('installCommand')]",
    "lastNodeInstallCommand": "[concat(variables('installCommand'), ' -l')]",
    "arbiterNodeInstallCommand": "[concat(variables('installCommand'), ' -a')]"
},
```

## <a name="return-state-from-a-template"></a>Zwraca stan z szablonu
Nie tylko możesz przekazać dane do szablonu, możesz również udostępniać danych powrót do wywoływania szablonu. W **generuje** sekcji połączonych szablonu, możesz podać pary klucz wartość, które mogą być używane w szablonie źródłowym.

Poniższy przykład przedstawia sposób przekazywania prywatnego adresu IP generowane w szablonie połączony.

```json
"outputs": {
    "masterip": {
        "value": "[reference(concat(variables('nicName'),0)).ipConfigurations[0].properties.privateIPAddress]",
        "type": "string"
     }
}
```

W szablonie głównym można użyć tych danych przy użyciu następującej składni:

    "[reference('master-node').outputs.masterip.value]"

Można użyć tego wyrażenia w sekcji danych wyjściowych lub sekcji zasobów w szablonie głównym. W sekcji zmiennych nie można użyć wyrażenia, ponieważ zależy od stanu czasu wykonywania. Aby przywrócić tę wartość w szablonie głównym, należy użyć:

```json
"outputs": {
  "masterIpAddress": {
    "value": "[reference('master-node').outputs.masterip.value]",
    "type": "string"
  }
```

Przykład zgodnie z sekcją danych wyjściowych szablonu połączony do zwrócenia dysków z danymi dla maszyny wirtualnej, zobacz [tworzenie wielu dysków danych maszyny wirtualnej](resource-group-create-multiple.md).

## <a name="define-authentication-settings-for-virtual-machine"></a>Zdefiniuj ustawienia uwierzytelniania dla maszyny wirtualnej
Aby określić ustawienia uwierzytelniania dla maszyny wirtualnej, można użyć z tego samego wzorca wcześniej ustawień konfiguracji. Parametr do przekazania do tworzenia typ uwierzytelniania.

```json
"parameters": {
  "authenticationType": {
    "allowedValues": [
      "password",
      "sshPublicKey"
    ],
    "defaultValue": "password",
    "metadata": {
      "description": "Authentication type"
    },
    "type": "string"
  }
}
```

Dodaj zmienne dla typów uwierzytelniania inny i zmienną do przechowywania typ jest używany dla tego wdrożenia na podstawie wartości parametru.

```json
"variables": {
  "osProfile": "[variables(concat('osProfile', parameters('authenticationType')))]",
  "osProfilepassword": {
    "adminPassword": "[parameters('adminPassword')]",
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]"
  },
  "osProfilesshPublicKey": {
    "adminUsername": "notused",
    "computerName": "[parameters('vmName')]",
    "customData": "[base64(variables('customData'))]",
    "linuxConfiguration": {
      "disablePasswordAuthentication": "true",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "[parameters('sshPublicKey')]",
            "path": "/home/notused/.ssh/authorized_keys"
          }
        ]
      }
    }
  }
}
```

Podczas definiowania maszyny wirtualnej, należy ustawić **osProfile** do zmiennej został utworzony.

```json
{
  "type": "Microsoft.Compute/virtualMachines",
  ...
  "osProfile": "[variables('osProfile')]"
}
```

## <a name="next-steps"></a>Następne kroki
* Informacje na temat części szablonu, zobacz [Authoring Azure Resource Manager szablonów](resource-group-authoring-templates.md)
* Aby poznać funkcje, które są dostępne w ramach szablonu, zobacz [funkcje szablonów usługi Azure Resource Manager](resource-group-template-functions.md)
