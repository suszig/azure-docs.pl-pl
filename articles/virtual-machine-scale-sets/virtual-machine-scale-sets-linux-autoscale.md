---
title: Zestawy skalowania maszyny wirtualnej systemu Linux skalowania automatycznego | Dokumentacja firmy Microsoft
description: "Ustawienia skalowania automatycznego dla systemu Linux zestawu skalowania maszyn wirtualnych przy użyciu wiersza polecenia platformy Azure"
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 83e93d9c-cac0-41d3-8316-6016f5ed0ce4
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: eff4add1cb16fe25022787668dc1d2277845dd95
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="automatically-scale-linux-machines-in-a-virtual-machine-scale-set"></a>Automatycznie skalować maszyny z systemem Linux w zestawie skalowania maszyn wirtualnych
Zestawy skalowania maszyny wirtualnej ułatwiają wdrażanie i zarządzanie maszynami wirtualnymi identyczne jako zestaw. Zestawy skalowania w przypadku aplikacji o dużej skali zapewnić warstwy obliczeniowej wysoce skalowalnemu i dostosowania i obsługują obrazów platformy systemu Windows, Linux platformy obrazów niestandardowych obrazów i rozszerzenia. Aby dowiedzieć się więcej, zobacz [omówienie zestawy skalowania maszyny wirtualnej](virtual-machine-scale-sets-overview.md).

W tym samouczku przedstawiono sposób tworzenia zestawu skalowania maszyn wirtualnych systemu Linux przy użyciu najnowszej wersji Ubuntu Linux. Samouczek przedstawia również sposób automatycznie skalować maszyn w zestawie. Możesz utworzyć skalowalnego definiować skalowania przez utworzenie szablonu usługi Azure Resource Manager i wdrażanie za pomocą interfejsu wiersza polecenia Azure a. Aby uzyskać więcej informacji na temat szablonów, zobacz [Tworzenie szablonów usługi Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). Aby dowiedzieć się więcej na temat skalowania automatycznego zestawy skalowania, zobacz [automatyczne skalowanie i zestawach skali maszyny wirtualnej](virtual-machine-scale-sets-autoscale-overview.md).

W tym samouczku wdrażania następujących zasobów i rozszerzeń:

* Microsoft.Storage/storageAccounts
* Microsoft.Network/virtualNetworks
* Microsoft.Network/publicIPAddresses
* Microsoft.Network/loadBalancers
* Microsoft.Network/networkInterfaces
* Microsoft.Compute/virtualMachines
* Microsoft.Compute/virtualMachineScaleSets
* Microsoft.Insights.VMDiagnosticsSettings
* Microsoft.Insights/autoscaleSettings

Aby uzyskać więcej informacji na temat zasoby usługi Resource Manager, zobacz [usługi Azure Resource Manager, a wdrożenie klasyczne](../azure-resource-manager/resource-manager-deployment-model.md).

Przed rozpoczęciem kroków w tym samouczku [instalowanie interfejsu wiersza polecenia Azure](../cli-install-nodejs.md).

## <a name="step-1-create-a-resource-group-and-a-storage-account"></a>Krok 1: Tworzenie grupy zasobów i konto magazynu

1. **Zaloguj się do platformy Microsoft Azure**  
W interfejsie wiersza polecenia (Bash, terminali, wiersza polecenia), należy włączyć tryb Resource Manager, a następnie [Zaloguj się za pomocą identyfikatora firmy lub szkoły](../xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login). Postępuj zgodnie z monitami w celu obsługi logowania interakcyjnego do konta platformy Azure.

    ```cli   
    azure config mode arm

    azure login
    ```
   
    > [!NOTE]
    > Jeśli masz służbowego lub szkolnego identyfikator i nie mają włączone uwierzytelnianie dwuskładnikowe, użyj `azure login -u` o identyfikatorze można logować się bez sesji interaktywnej. Jeśli nie ma służbowego lub szkolnego identyfikator, możesz [Utwórz identyfikator firmy lub szkoły z osobistego konta Microsoft](../active-directory/active-directory-users-create-azure-portal.md).
    
2. **Utwórz grupę zasobów**  
Należy wdrożyć wszystkie zasoby w grupie zasobów. W tym samouczku, określ nazwę grupy zasobów **vmsstest1**.
   
    ```cli
    azure group create vmsstestrg1 centralus
    ```

3. **Wdrażanie konta magazynu do nowej grupy zasobów**  
To konto magazynu jest przechowywania szablonu. Utwórz konto magazynu o nazwie **vmsstestsa**.
   
    ```cli
    azure storage account create -g vmsstestrg1 -l centralus --kind Storage --sku-name LRS vmsstestsa
    ```

## <a name="step-2-create-the-template"></a>Krok 2: Tworzenie szablonu
Szablon usługi Azure Resource Manager umożliwia do wdrażania i zarządzania zasobami Azure ze sobą przy użyciu opisu JSON zasobów i parametrów skojarzonego wdrożenia.

1. W edytorze Ulubione Utwórz plik VMSSTemplate.json i Dodaj początkowej strukturze JSON do obsługi szablonu.

    ```json
    {
      "$schema":"http://schema.management.azure.com/schemas/2014-04-01-preview/VM.json",
      "contentVersion": "1.0.0.0",
      "parameters": {
      },
      "variables": {
      },
      "resources": [
      ]
    }
    ```

2. Parametry nie są zawsze wymagana, ale umożliwiają wprowadzanie wartości podczas wdrażania szablonu. Dodaj tych parametrów w obszarze parametrów elementu nadrzędnego, który został dodany do szablonu.

    ```json
    "vmName": { "type": "string" },
    "vmSSName": { "type": "string" },
    "instanceCount": { "type": "string" },
    "adminUsername": { "type": "string" },
    "adminPassword": { "type": "securestring" },
    "resourcePrefix": { "type": "string" }
    ```
   
   * Ustaw nazwę oddzielne maszynę wirtualną, która umożliwia dostęp do maszyn w skali.
   * Nazwa konta magazynu, w którym szablon jest przechowywany.
   * Liczba wystąpień maszyn wirtualnych można początkowo utworzyć w skali zestawu.
   * Nazwa i hasło konta administratora na maszynach wirtualnych.
   * Prefiks nazwy zasobów, które są tworzone w celu obsługi zestawu skali.

3. Zmienne można w szablonie, aby określić wartości, które mogą ulec zmianie często lub wartości, które muszą zostać utworzone z kombinacji wartości parametrów. Dodaj te zmienne w obszarze Zmienne elementu nadrzędnego, który został dodany do szablonu.

    ```json
    "dnsName1": "[concat(parameters('resourcePrefix'),'dn1')]",
    "dnsName2": "[concat(parameters('resourcePrefix'),'dn2')]",
    "publicIP1": "[concat(parameters('resourcePrefix'),'ip1')]",
    "publicIP2": "[concat(parameters('resourcePrefix'),'ip2')]",
    "loadBalancerName": "[concat(parameters('resourcePrefix'),'lb1')]",
    "virtualNetworkName": "[concat(parameters('resourcePrefix'),'vn1')]",
    "nicName": "[concat(parameters('resourcePrefix'),'nc1')]",
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',variables('loadBalancerName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/loadBalancerFrontEnd')]",
    "storageAccountSuffix": [ "a", "g", "m", "s", "y" ],
    "diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'a')]",
    "accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/','Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
    "wadlogs": "<WadCfg><DiagnosticMonitorConfiguration>",
    "wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor\\PercentProcessorTime\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"CPU percentage guest OS\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
    "wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
    "wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
    "wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
    ```

   * Nazwy DNS, które są używane przez interfejsy sieciowe.
   * Nazwy adresów IP i prefiksy dla sieci wirtualnej i podsieci.
   * Nazwy i identyfikatory sieci wirtualnej załadować równoważenia i interfejsów sieciowych.
   * Ustawianie nazw kont magazynu dla konta skojarzone z maszyn w skali.
   * Ustawienia rozszerzenia diagnostyki, które jest zainstalowane na maszynie wirtualnej. Aby uzyskać więcej informacji na temat rozszerzenia diagnostyki, zobacz [Utwórz maszynę wirtualną systemu Windows z monitorowania i diagnostyki za pomocą szablonu usługi Resource Manager Azure](../virtual-machines/windows/extensions-diagnostics-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

4. Dodaj zasób konta magazynu w zasoby elementu nadrzędnego, który został dodany do szablonu. Ten szablon używa pętlę do utworzenia zalecane pięć kont magazynu przechowywania dysków systemu operacyjnego i danych diagnostycznych. Ten zestaw kont może obsługiwać maksymalnie 100 maszyn wirtualnych w zestawie skalowania, który jest bieżącym maksymalną. Każde konto magazynu ma nazwę z określeniem litera, która została zdefiniowana w zmiennych, w połączeniu z sufiksem parametry podane dla szablonu.
   
        {
          "type": "Microsoft.Storage/storageAccounts",
          "name": "[concat(parameters('resourcePrefix'), variables('storageAccountSuffix')[copyIndex()])]",
          "apiVersion": "2015-06-15",
          "copy": {
            "name": "storageLoop",
            "count": 5
          },
          "location": "[resourceGroup().location]",
          "properties": { "accountType": "Standard_LRS" }
        },

5. Dodaj zasób sieci wirtualnej. Aby uzyskać więcej informacji, zobacz [dostawcy zasobów sieciowych](../virtual-network/resource-groups-networking.md).

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": { "addressPrefixes": [ "10.0.0.0/16" ] },
        "subnets": [
          {
            "name": "subnet1",
            "properties": { "addressPrefix": "10.0.0.0/24" }
          }
        ]
      }
    },
    ```

6. Dodaj zasoby publicznych adresów IP, które są używane przez usługi równoważenia obciążenia i interfejsu sieciowego.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP1')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName1')]"
        }
      }
    },
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIP2')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "Dynamic",
        "dnsSettings": {
          "domainNameLabel": "[variables('dnsName2')]"
        }
      }
    },
    ```

7. Dodaj zasób usługi równoważenia obciążenia, który jest używany przez zestaw skali. Aby uzyskać więcej informacji, zobacz [Obsługa Menedżera zasobów Azure dla usługi równoważenia obciążenia](../load-balancer/load-balancer-arm.md).

    ```json   
    {
      "apiVersion": "2015-06-15",
      "name": "[variables('loadBalancerName')]",
      "type": "Microsoft.Network/loadBalancers",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
      ],
      "properties": {
        "frontendIPConfigurations": [
          {
            "name": "loadBalancerFrontEnd",
            "properties": {
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses/', variables('publicIP1'))]"
              }
            }
          }
        ],
        "backendAddressPools": [ { "name": "bepool1" } ],
        "inboundNatPools": [
          {
            "name": "natpool1",
            "properties": {
              "frontendIPConfiguration": {
                "id": "[variables('frontEndIPConfigID')]"
              },
              "protocol": "tcp",
              "frontendPortRangeStart": 50000,
              "frontendPortRangeEnd": 50500,
              "backendPort": 22
            }
          }
        ]
      }
    },
    ```

8. Dodaj zasób interfejsu sieci, który jest używany przez oddzielne maszynę wirtualną. Ponieważ maszyny w zestawie skalowania nie są dostępne za pośrednictwem publicznego adresu IP, oddzielnej maszynie wirtualnej jest tworzony w tej samej sieci wirtualnej do dostępu zdalnego do maszyny.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIP2'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIP2'))]"
              },
              "subnet": {
                "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
              }
            }
          }
        ]
      }
    },
    ```

9. Dodaj oddzielnej maszynie wirtualnej w tej samej sieci co zestaw skali.

    ```json
    {
      "apiVersion": "2016-03-30",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[parameters('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": { "vmSize": "Standard_A1" },
        "osProfile": {
          "computername": "[parameters('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "Canonical",
            "offer": "UbuntuServer",
            "sku": "14.04.4-LTS",
            "version": "latest"
          },
          "osDisk": {
            "name": "[concat(parameters('resourcePrefix'), 'os1')]",
            "vhd": {
              "uri":  "[concat('https://',parameters('resourcePrefix'),'a.blob.core.windows.net/vhds/',parameters('resourcePrefix'),'os1.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        }
      }
    },
    ```

10. Dodaj zasób zestawu skalowania maszyny wirtualnej i określ rozszerzenie diagnostyki, który jest zainstalowany na wszystkich maszynach wirtualnych w zestawie skalowania. Wiele ustawień dla tego zasobu przypominają z zasobu maszyny wirtualnej. Główne różnice są element pojemności, który określa liczbę maszyn wirtualnych w zestawie skali i upgradePolicy, która określa, jak zostało zaktualizowane do maszyn wirtualnych. Zestaw skalowania nie jest tworzony, dopóki wszystkie konta magazynu są tworzone za pomocą elementu dependsOn określonej.

    ```json
    {
      "type": "Microsoft.Compute/virtualMachineScaleSets",
      "apiVersion": "2016-03-30",
      "name": "[parameters('vmSSName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "storageLoop",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]",
        "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
      ],
      "sku": {
        "name": "Standard_A1",
        "tier": "Standard",
        "capacity": "[parameters('instanceCount')]"
      },
      "properties": {
        "upgradePolicy": {
          "mode": "Manual"
        },
        "virtualMachineProfile": {
          "storageProfile": {
            "osDisk": {
              "vhdContainers": [
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[0],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[1],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[2],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[3],'.blob.core.windows.net/vmss')]",
                "[concat('https://', parameters('resourcePrefix'), variables('storageAccountSuffix')[4],'.blob.core.windows.net/vmss')]"
              ],
              "name": "vmssosdisk",
              "caching": "ReadOnly",
              "createOption": "FromImage"
            },
            "imageReference": {
              "publisher": "Canonical",
              "offer": "UbuntuServer",
              "sku": "14.04.4-LTS",
              "version": "latest"
            }
          },
          "osProfile": {
            "computerNamePrefix": "[parameters('vmSSName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
          },
          "networkProfile": {
            "networkInterfaceConfigurations": [
              {
                "name": "networkconfig1",
                "properties": {
                  "primary": "true",
                  "ipConfigurations": [
                    {
                      "name": "ip1",
                      "properties": {
                        "subnet": {
                          "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/virtualNetworks/',variables('virtualNetworkName'),'/subnets/subnet1')]"
                        },
                        "loadBalancerBackendAddressPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/backendAddressPools/bepool1')]"
                          }
                        ],
                        "loadBalancerInboundNatPools": [
                          {
                            "id": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Network/loadBalancers/',variables('loadBalancerName'),'/inboundNatPools/natpool1')]"
                          }
                        ]
                      }
                    }
                  ]
                }
              }
            ]
          },
          "extensionProfile": {
            "extensions": [
              {
                "name":"LinuxDiagnostic",
                "properties": {
                  "publisher":"Microsoft.OSTCExtensions",
                  "type":"LinuxDiagnostic",
                  "typeHandlerVersion":"2.1",
                  "autoUpgradeMinorVersion":false,
                  "settings": {
                    "xmlCfg":"[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
                    "storageAccount":"[variables('diagnosticsStorageAccountName')]"
                  },
                  "protectedSettings": {
                    "storageAccountName":"[variables('diagnosticsStorageAccountName')]",
                    "storageAccountKey":"[listkeys(variables('accountid'), '2015-06-15').key1]",
                    "storageAccountEndPoint":"https://core.windows.net"
                  }
                }
              }
            ]
          }
        }
      }
    },
    ```

11. Dodaj zasób autoscaleSettings, który definiuje sposób oparte na wykorzystanie procesora na komputerach w zestawie dopasowuje zestaw skali.

    ```json
    {
      "type": "Microsoft.Insights/autoscaleSettings",
      "apiVersion": "2015-04-01",
      "name": "[concat(parameters('resourcePrefix'),'as1')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      ],
      "properties": {
        "enabled": true,
        "name": "[concat(parameters('resourcePrefix'),'as1')]",
        "profiles": [
          {
            "name": "Profile1",
            "capacity": {
              "minimum": "1",
              "maximum": "10",
              "default": "1"
            },
            "rules": [
              {
                "metricTrigger": {
                  "metricName": "\\Processor\\PercentProcessorTime",
                  "metricNamespace": "",
                  "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]",
                  "timeGrain": "PT1M",
                  "statistic": "Average",
                  "timeWindow": "PT5M",
                  "timeAggregation": "Average",
                  "operator": "GreaterThan",
                  "threshold": 50.0
                },
                "scaleAction": {
                  "direction": "Increase",
                  "type": "ChangeCount",
                  "value": "1",
                  "cooldown": "PT5M"
                }
              }
            ]
          }
        ],
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
      }
    }
    ```
    
    W tym samouczku te wartości są ważne:
    
    * **metricName**  
    Ta wartość jest taka sama jak zdefiniowanego w zmiennej wadperfcounter licznika wydajności. Za pomocą tej zmiennej, zbiera rozszerzenia diagnostyki **Processor\PercentProcessorTime** licznika.
    
    * **metricResourceUri**  
    Ta wartość jest identyfikator zasobu zestawu skali maszyny wirtualnej.
    
    * **ziarnem czasu**  
    Ta wartość jest stopień szczegółowości metryki, które są zbierane. W tym szablonie ustawiono na jedną minutę.
    
    * **Statystyka**  
    Ta wartość określa, jak metryki połączone pomieścić automatycznych akcji skalowania. Możliwe wartości to: średnia, Min, Max. W tym szablonie są zbierane średniego użycia procesora CPU całkowity maszyn wirtualnych.

    * **timeWindow**  
    Ta wartość jest przedział czasu, w którym są zbierane dane wystąpienia. Musi być od 5 minut do 12 godzin.
    
    * **timeAggregation**  
    jego wartość określa, jak powinny być połączone dane, które są zbierane wraz z upływem czasu. Wartość domyślna to średnia. Możliwe wartości to: średnia, co najmniej, maksimum, ostatnich, łączna liczba, liczba.
    
    * **operator**  
    Ta wartość jest operator, który służy do porównywania danych metryki i wartość progową. Możliwe wartości to: równa, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.
    
    * **Próg**  
    Ta wartość jest wyzwalane akcji skalowania. W tym szablonie maszyn są dodawane do skalowania, ustawić, gdy średniego użycia procesora CPU między maszynami w zestawie jest ponad 50%.
    
    * **Kierunek**  
    Ta wartość Określa akcję wykonywaną, gdy uzyskuje się wartość progową. Możliwe wartości to zwiększyć lub zmniejszyć. W tym szablonie jeśli próg wynosi ponad 50% w oknie zdefiniowanego czasu zwiększa się liczba maszyn wirtualnych w zestawie skalowania.

    * **Typ**  
    Ta wartość jest typu akcji, która powinna się odbyć i musi mieć ustawioną ChangeCount.
    
    * **wartość**  
    Ta wartość jest liczba maszyn wirtualnych, które są dodawane lub usuwane z zestawu skalowania. Ta wartość musi wynosić 1 lub większą. Wartość domyślna to 1. W tym szablonie liczba maszyn w skali ustawić zwiększa 1, gdy ze osiągnięty jest próg.

    * **cooldown**  
    Ta wartość jest czas oczekiwania od momentu ostatniej akcji skalowania, zanim nastąpi następnej akcji. Ta wartość musi należeć do zakresu od minutę i jeden tydzień.

12. Zapisz plik szablonu.    

## <a name="step-3-upload-the-template-to-storage"></a>Krok 3: Przekaż szablon do magazynu
Szablon można przekazać tak długo, jak znać nazwy i klucza podstawowego konta magazynu, który został utworzony w kroku 1.

1. W interfejsie wiersza polecenia (Bash, terminali, wiersza polecenia) uruchom następujące polecenia, aby ustawić zmienne środowiskowe wymagane do uzyskania dostępu do konta magazynu:

    ```cli   
    export AZURE_STORAGE_ACCOUNT={account_name}
    export AZURE_STORAGE_ACCESS_KEY={key}
    ```
    
    Klucz można uzyskać, klikając ikonę klucza, podczas wyświetlania zasobów konta magazynu w portalu Azure. Korzystając z wiersza polecenia systemu Windows, wpisz **ustawić** zamiast eksportu.

2. Utwórz kontener do przechowywania szablonu.
   
    ```cli
    azure storage container create -p Blob templates
    ```

3. Przekaż plik szablonu na nowy kontener.
   
    ```cli
    azure storage blob upload VMSSTemplate.json templates VMSSTemplate.json
    ```

## <a name="step-4-deploy-the-template"></a>Krok 4: Wdrażanie szablonu
Teraz, gdy szablon został utworzony, możesz przystąpić do wdrażania zasobów. Użyj tego polecenia, aby rozpocząć proces:

```cli
azure group deployment create --template-uri https://vmsstestsa.blob.core.windows.net/templates/VMSSTemplate.json vmsstestrg1 vmsstestdp1
```

Po naciśnięciu wprowadź, zostanie wyświetlony monit o podanie wartości zmiennych, które zostanie przypisane. Podaj następujące wartości:

```cli
vmName: vmsstestvm1
vmSSName: vmsstest1
instanceCount: 5
adminUserName: vmadmin1
adminPassword: VMpass1
resourcePrefix: vmsstest
```

Powinna ona potrwać około 15 minut dla wszystkich zasobów do wdrożenia pomyślnie.

> [!NOTE]
> Można także użyć możliwości portalu do wdrażania zasobów. Użyj tego linku: https://portal.azure.com/#create/Microsoft.Template/uri/<link to VM Scale Set JSON template>


## <a name="step-5-monitor-resources"></a>Krok 5: Monitorowanie zasobów
Można uzyskać informacji o zestawy skalowania maszyny wirtualnej za pomocą następujących metod:

* Portal Azure — można obecnie uzyskać ograniczoną ilość informacji za pomocą portalu.

* [Eksploratora zasobów Azure](https://resources.azure.com/) — to narzędzie jest najlepszy do eksplorowania bieżącego stanu sieci zestawu skali. Tej ścieżki i powinien zostać wyświetlony widok wystąpienia zestawu skali utworzony:
  
    ```cli
    subscriptions > {your subscription} > resourceGroups > vmsstestrg1 > providers > Microsoft.Compute > virtualMachineScaleSets > vmsstest1 > virtualMachines
    ```

* Azure CLI — Użyj tego polecenia, aby pobrać niektóre informacje:

    ```cli  
    azure resource show -n vmsstest1 -r Microsoft.Compute/virtualMachineScaleSets -o 2015-06-15 -g vmsstestrg1
    ```

* Połączenie z maszyną wirtualną jumpbox podobnie jak inne maszyny, a następnie zdalny dostęp maszyny wirtualne w skali ustawioną monitorowania poszczególnych procesów.

> [!NOTE]
> Zakończenie interfejsu API REST do uzyskiwania informacji na temat zestawów skalowania można znaleźć w [zestawach skali maszyny wirtualnej](https://msdn.microsoft.com/library/mt589023.aspx).

## <a name="step-6-remove-the-resources"></a>Krok 6: Usuń zasoby
Ponieważ naliczane są opłaty za zasoby używane na platformie Azure, zawsze jest dobrym rozwiązaniem, aby usunąć zasoby, które nie są już potrzebne. Nie trzeba osobno usunąć wszystkie zasoby w grupie zasobów. Można usunąć grupy zasobów i wszystkie jej zasoby są automatycznie usuwane.

```cli
azure group delete vmsstestrg1
```

## <a name="next-steps"></a>Następne kroki
* Znajdź przykłady Azure Monitor funkcje w [Azure Monitor i platform interfejsu wiersza polecenia Szybki start — przykłady](../monitoring-and-diagnostics/insights-cli-samples.md)
* Dowiedz się więcej o funkcji powiadomień w [użyć akcji skalowania automatycznego do wysyłania wiadomości e-mail i elementu webhook powiadomień o alertach w monitorze Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md)
* Dowiedz się, jak [dzienników inspekcji używany do wysyłania wiadomości e-mail i elementu webhook powiadomień o alertach w monitorze Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)
* Zapoznaj się z [aplikacja demonstracyjna skalowania automatycznego na Ubuntu 16.04](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) szablonu, który konfiguruje aplikacji Python/bottle do wykonywania funkcji skalowania automatycznego programu zestawach skali maszyny wirtualnej.

