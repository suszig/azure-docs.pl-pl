<properties
   pageTitle="Przewodnik po szablonie usługi Resource Manager | System Microsoft Azure"
   description="Przewodnik krok po kroku po szablonie usługi Resource Manager służący do aprowizacji podstawowej architektury IaaS platformy Azure."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager=""
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/04/2016"
   ms.author="navale;tomfitz"/>
   

# Przewodnik po szablonie usługi Resource Manager

Jednym z pierwszych pytań podczas tworzenia szablonu jest „jak rozpocząć?”. Można rozpocząć od pustego szablonu, utworzyć podstawową strukturę zgodnie z opisem w artykule [Tworzenie szablonu](resource-group-authoring-templates.md#template-format), a następnie dodać zasoby oraz odpowiednie parametry i zmienne. Dobrą alternatywą jest rozpoczęcie od zapoznania się z [galerią Szybki start](https://github.com/Azure/azure-quickstart-templates) i znalezienie scenariuszy, których cel jest podobny do tego, co ma zostać zrobione. Możliwe jest scalenie kilku szablonów lub edytowane istniejącego szablonu w taki sposób, aby odpowiadało to potrzebom własnego konkretnego scenariusza. 

Spójrzmy na wspólną infrastrukturę:

* Dwie maszyny wirtualne używające tego samego konta magazynu znajdujące się w tym samym zestawie dostępności i w tej samej podsieci sieci wirtualnej.
* Jedna karta sieciowa i jeden adres IP dla każdej maszyny wirtualnej.
* Moduł równoważenia obciążenia z regułą równoważenia obciążenia na porcie 80.

![architektura](./media/resource-group-overview/arm_arch.png)

W tym temacie szczegółowo omówiono kroki tworzenia szablonu usługi Resource Manager dla tej infrastruktury. Ostateczny szablon, który zostanie utworzony, jest oparty na szablonie Szybki start o nazwie [2 VMs in a Load Balancer and load balancing rules](https://azure.microsoft.com/documentation/templates/201-2-vms-loadbalancer-lbrules/) (Dwie maszyny wirtualne z modułem równoważenia obciążenia i regułami równoważenia obciążenia).

Jednak nie wszystko naraz — utwórzmy najpierw konto magazynu i wdróżmy je. Następnym krokiem po opanowaniu tworzenia konta magazynu będzie dodanie innych zasobów i ponowne wdrożenie szablonu w celu zakończenia tworzenia infrastruktury.

>[AZURE.NOTE] Podczas tworzenia szablonu można użyć dowolnego typu edytora. Program Visual Studio zawiera narzędzia, które upraszczają proces tworzenia szablonu, ale do ukończenia tego samouczka nie ma potrzeby używania tego programu. Samouczek dotyczący tworzenia wdrożenia aplikacji sieci Web i bazy danych SQL Database za pomocą programu Visual Studio można znaleźć w sekcji [Tworzenie i wdrażanie grup zasobów platformy Azure za pomocą programu Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md). 

## Tworzenie szablonu usługi Resource Manager

Szablon to plik JSON definiujący wszystkie zasoby, które zostaną wdrożone. Umożliwia on również zdefiniowanie parametrów określanych podczas wdrażania, zmiennych tworzonych na podstawie innych wartości i wyrażeń oraz danych wyjściowych z wdrożenia. 

Zacznijmy od najprostszego szablonu:

```json
    {
      "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
      "contentVersion": "1.0.0.0",
      "parameters": {  },
      "variables": {  },
      "resources": [  ],
      "outputs": {  }
    }
 ```

Zapisz ten plik pod nazwą **azuredeploy.json** (należy pamiętać, że szablon może mieć dowolną nazwę, ale musi być plikiem json).

## Tworzenie konta magazynu
W sekcji **resources** dodaj obiekt, który definiuje konto magazynu, w sposób pokazany poniżej. 

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('storageAccountName')]",
    "apiVersion": "2015-06-15",
    "location": "[resourceGroup().location]",
    "properties": {
      "accountType": "Standard_LRS"
    }
  }
]
```

Możesz się zastanawiać, skąd pochodzą te właściwości i wartości. Właściwości **type**, **name**, **apiVersion** i **location** są standardowymi elementami, które są dostępne dla wszystkich typów zasobów. Więcej informacji o wspólnych elementach zawiera sekcja [Zasoby](resource-group-authoring-templates.md#resources). Właściwość **name** jest ustawiana na wartość przekazywaną podczas wdrożenia, a właściwość **location** jest ustawiana jako lokalizacja używana przez grupę zasobów. Sposób określania właściwości **type** i **apiVersion** został przedstawiony w poniższych sekcjach.

Sekcja **properties** zawiera wszystkie właściwości, które są unikatowe dla określonego typu zasobu. Wartości określone w tej sekcji są w pełni zgodne z operacją PUT w interfejsie API REST na potrzeby tworzenia takiego typu zasobu. Podczas tworzenia konta magazynu należy podać wartość właściwości **accountType**. Zwróć uwagę, że w [interfejsie API REST na potrzeby tworzenia konta usługi Storage](https://msdn.microsoft.com/library/azure/mt163564.aspx) sekcja właściwości operacji REST zawiera również właściwość **accountType**, a dozwolone wartości są udokumentowane. W tym przykładzie typ konta został ustawiony na wartość **Standard_LRS**, ale można określić inną wartość lub zezwolić użytkownikom na przekazywanie typu konta jako parametru.

Wróćmy teraz do sekcji **parameters** i zobaczmy, jak zdefiniować nazwę konta magazynu. Aby dowiedzieć się więcej na temat użycia parametrów, zobacz [Parametry](resource-group-authoring-templates.md#parameters). 

```json
"parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
}
```
W tym miejscu zdefiniowany został parametr typu ciąg (string), w którym będzie przechowywana nazwa konta magazynu. Wartość tego parametru zostanie podana podczas wdrażania szablonu.

## Wdrażanie szablonu
Mamy pełny szablon służący do tworzenia nowego konta magazynu. Tak jak pamiętasz, szablon został zapisany w pliku **azuredeploy.json**:

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters" : {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Storage Account Name"
      }
    }
  },  
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('storageAccountName')]",
      "apiVersion": "2015-06-15",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "Standard_LRS"
      }
    }
  ]
}
```

Istnieje kilka metod wdrażania szablonu, co zostało opisane w artykule dotyczącym [wdrażania zasobów](resource-group-template-deploy.md). Aby wdrożyć szablon przy użyciu programu Azure PowerShell, użyj następującego polecenia:

```powershell
# create a new resource group
New-AzureRmResourceGroup -Name ExampleResourceGroup -Location "West Europe"

# deploy the template to the resource group
New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExampleResourceGroup -TemplateFile azuredeploy.json
```

Aby natomiast wdrożyć szablon przy użyciu interfejsu wiersza polecenia platformy Azure, użyj następującego polecenia:

```
azure group create -n ExampleResourceGroup -l "West Europe"

azure group deployment create -f azuredeploy.json -g ExampleResourceGroup -n ExampleDeployment
```

Jesteś teraz dumnym właścicielem konta magazynu!

W następnych krokach zostaną dodane wszystkie zasoby wymagane do wdrożenia architektury opisanej na początku tego samouczka. Te zasoby zostaną dodane w tym samym szablonie, na którym odbywała się wcześniej praca.

## Zestaw dostępności
Po definicji konta magazynu należy dodać zestaw dostępności dla maszyn wirtualnych. W tym przypadku nie są wymagane żadne dodatkowe właściwości, więc jego definicja jest dość prosta. Zobacz artykuł z opisem [interfejsu API REST na potrzeby tworzenia zestawu dostępności](https://msdn.microsoft.com/library/azure/mt163607.aspx), aby wyświetlić pełną sekcję właściwości, jeśli chcesz zdefiniować wartości liczby domen aktualizacji i domen błędów.

```json
{
   "type": "Microsoft.Compute/availabilitySets",
   "name": "[variables('availabilitySetName')]",
   "apiVersion": "2015-06-15",
   "location": "[resourceGroup().location]",
   "properties": {}
}
```

Należy zauważyć, że właściwość **name** jest ustawiona na wartość zmiennej. W przypadku tego szablonu nazwa zestawu dostępności jest wymagana w kilku różnych miejscach. Szablon można łatwiej obsługiwać, definiując tą wartość raz i używając jej w kilku miejscach.

Wartość określona dla właściwości **type** zawiera zarówno dostawcę zasobów, jak i typ zasobu. W przypadku zestawów dostępności dostawca zasobów to **Microsoft.Compute**, a typ zasobu to **availabilitySets**. Listę dostępnych dostawców zasobów można uzyskać, uruchamiając następujące polecenie programu PowerShell:

```powershell
    Get-AzureRmResourceProvider -ListAvailable
```

Jeśli natomiast używasz interfejsu wiersza polecenia platformy Azure, możesz uruchomić następujące polecenie:
```
    azure provider list
```
Biorąc pod uwagę, że w tym temacie operacje tworzenia będą przeprowadzane za pomocą kont magazynu, maszyn wirtualnych i sieci wirtualnych, praca będzie odbywać się z następującymi zasobami:

- Microsoft.Storage
- Microsoft.Compute
- Microsoft.Network

Aby wyświetlić typy zasobów dla określonego dostawcy, uruchom następujące polecenie programu PowerShell:

```powershell
    (Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Compute).ResourceTypes
```

Natomiast w przypadku interfejsu wiersza polecenia platformy Azure następujące polecenie zwróci dostępne typy w formacie JSON i zapisze je do pliku.

```
    azure provider show Microsoft.Compute --json > c:\temp.json
```

Powinien zostać wyświetlony typ **availabilitySets** jako jeden z typów w ramach zasobu **Microsoft.Compute**. Pełna nazwa typu to **Microsoft.Compute/availabilitySets**. Możliwe jest określenie nazwy typu zasobu dla każdego z zasobów w szablonie.

## Publiczny adres IP
Zdefiniuj publiczny adres IP. Informacje o właściwościach, które należy ustawić, możesz sprawdzić w artykule dotyczącym [interfejsu API REST dla publicznych adresów IP](https://msdn.microsoft.com/library/azure/mt163590.aspx).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[parameters('publicIPAddressName')]",
  "location": "[resourceGroup().location]",
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('dnsNameforLBIP')]"
    }
  }
}
```

Metoda alokacji została ustawiona na wartość **Dynamic** (Dynamiczna), ale możesz ustawić ją na żądaną wartość lub zdefiniować w taki sposób, aby akceptowała wartość parametru. Użytkownikom szablonu umożliwiono przekazywanie wartości dla etykiety nazwy domeny.

Zobaczmy teraz, w jaki sposób można określić wartość właściwości **apiVersion**. Określane wartości są po prostu zgodne z wersją interfejsu API REST, którego chcesz używać podczas tworzenia zasobu. W związku z tym typ tego zasobu można określić na podstawie dokumentacji interfejsu API REST. Można również uruchomić następujące polecenie programu PowerShell dla określonego typu.

```powershell
    ((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Network).ResourceTypes | Where-Object ResourceTypeName -eq publicIPAddresses).ApiVersions
```
Polecenie to zwraca następujące wartości:

    2015-06-15
    2015-05-01-preview
    2014-12-01-preview

Aby sprawdzić wersje interfejsu API za pomocą interfejsu wiersza polecenia platformy Azure, uruchom takie same polecenie **azure provider show**, które zostało przedstawione wcześniej.

Podczas tworzenia nowego szablonu wybierz najnowszą wersję interfejsu API.

## Sieć wirtualna i podsieć
Utwórz sieć wirtualną z jedną podsiecią. Wszystkie właściwości, które należy ustawić, można znaleźć w sekcji [Interfejs API REST dla sieci wirtualnych](https://msdn.microsoft.com/library/azure/mt163661.aspx).

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/virtualNetworks",
   "name": "[parameters('vnetName')]",
   "location": "[resourceGroup().location]",
   "properties": {
     "addressSpace": {
       "addressPrefixes": [
         "10.0.0.0/16"
       ]
     },
     "subnets": [
       {
         "name": "[variables('subnetName')]",
         "properties": {
           "addressPrefix": "10.0.0.0/24"
         }
       }
     ]
   }
}
```

## Moduł równoważenia obciążenia
Teraz zostanie utworzony zewnętrzny moduł równoważenia obciążenia. Ponieważ ten moduł równoważenia obciążenia używa publicznego adresu IP, należy zadeklarować zależność w publicznym adresie IP w sekcji **dependsOn**. Oznacza to, że moduł równoważenia obciążenia nie zostanie wdrożony do momentu zakończenia wdrażania publicznego adresu IP. Jeśli ta zależność nie zostanie zdefiniowana, zostanie wyświetlony błąd, ponieważ usługa Resource Manager podejmie próbę równoległego wdrażania zasobów i spróbuje ustawić moduł równoważenia obciążenia na publiczny adres IP, który jeszcze nie istnieje. 

Zostanie również utworzona pula adresów zaplecza, kilka reguł ruchu przychodzącego translatora adresów sieciowych dla protokołu RDP w ramach maszyn wirtualnych, a także reguła równoważenia obciążenia z sondowaniem TCP na porcie 80 w ramach tej definicji zasobu. Wszystkie właściwości można znaleźć w artykule dotyczącym [interfejsu API REST dla modułu równoważenia obciążenia](https://msdn.microsoft.com/library/azure/mt163574.aspx).

```json
{
   "apiVersion": "2015-06-15",
   "name": "[parameters('lbName')]",
   "type": "Microsoft.Network/loadBalancers",
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
   ],
   "properties": {
     "frontendIPConfigurations": [
       {
         "name": "LoadBalancerFrontEnd",
         "properties": {
           "publicIPAddress": {
             "id": "[variables('publicIPAddressID')]"
           }
         }
       }
     ],
     "backendAddressPools": [
       {
         "name": "BackendPool1"
       }
     ],
     "inboundNatRules": [
       {
         "name": "RDP-VM0",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50001,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       },
       {
         "name": "RDP-VM1",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "protocol": "tcp",
           "frontendPort": 50002,
           "backendPort": 3389,
           "enableFloatingIP": false
         }
       }
     ],
     "loadBalancingRules": [
       {
         "name": "LBRule",
         "properties": {
           "frontendIPConfiguration": {
             "id": "[variables('frontEndIPConfigID')]"
           },
           "backendAddressPool": {
             "id": "[variables('lbPoolID')]"
           },
           "protocol": "tcp",
           "frontendPort": 80,
           "backendPort": 80,
           "enableFloatingIP": false,
           "idleTimeoutInMinutes": 5,
           "probe": {
             "id": "[variables('lbProbeID')]"
           }
         }
       }
     ],
     "probes": [
       {
         "name": "tcpProbe",
         "properties": {
           "protocol": "tcp",
           "port": 80,
           "intervalInSeconds": 5,
           "numberOfProbes": 2
         }
       }
     ]
   }
}
```

## Interfejs sieciowy
Zostaną utworzone dwa interfejsy sieciowe — po jednym dla każdej maszyny wirtualnej. Zamiast konieczności dołączania zduplikowanych wpisów dla interfejsów sieciowych można użyć funkcji [copyIndex()](resource-group-create-multiple.md) w celu iteracji przez pętlę kopiowania (nazywaną nicLoop) i utworzenia kilku interfejsów sieciowych zgodnie z definicją w zmiennych `numberOfInstances`. Interfejs sieciowy jest zależny od utworzenia sieci wirtualnej i modułu równoważenia obciążenia. Interfejs sieciowy używa podsieci zdefiniowanej podczas tworzenia sieci wirtualnej i identyfikatora modułu równoważenia obciążenia do skonfigurowania puli adresów modułu równoważenia obciążenia i reguł ruchu przychodzącego translatora adresów sieciowych.
Wszystkie właściwości można znaleźć w artykule dotyczącym [interfejsu API REST dla interfejsów sieciowych](https://msdn.microsoft.com/library/azure/mt163668.aspx).

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Network/networkInterfaces",
   "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
   "location": "[resourceGroup().location]",
   "copy": {
     "name": "nicLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "dependsOn": [
     "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]",
     "[concat('Microsoft.Network/loadBalancers/', parameters('lbName'))]"
   ],
   "properties": {
     "ipConfigurations": [
       {
         "name": "ipconfig1",
         "properties": {
           "privateIPAllocationMethod": "Dynamic",
           "subnet": {
             "id": "[variables('subnetRef')]"
           },
           "loadBalancerBackendAddressPools": [
             {
               "id": "[concat(variables('lbID'), '/backendAddressPools/BackendPool1')]"
             }
           ],
           "loadBalancerInboundNatRules": [
             {
               "id": "[concat(variables('lbID'),'/inboundNatRules/RDP-VM', copyindex())]"
             }
           ]
         }
       }
     ]
   }
}
```

## Maszyna wirtualna
Zostaną utworzone dwie maszyny wirtualne przy użyciu funkcji copyIndex(), tak jak miało to miejsce w przypadku tworzenia [interfejsów sieciowych](#network-interface).
Proces tworzenia maszyny wirtualnej jest zależny od konta magazynu, interfejsu sieciowego i zestawu dostępności. Ta maszyna wirtualna zostanie utworzona na podstawie obrazu z witryny Marketplace zgodnie z definicją we właściwości `storageProfile` — element `imageReference` służy do definiowania wydawcy obrazu, oferty, jednostki SKU i wersji. Na koniec zostanie skonfigurowany profil diagnostyczny w celu włączenia diagnostyki dla maszyny wirtualnej. 

Aby znaleźć odpowiednie właściwości dla obrazu z witryny Marketplace, postępuj zgodnie z informacjami znajdującymi się w artykule dotyczącym [wybierania obrazów maszyny wirtualnej systemu Linux](./virtual-machines/virtual-machines-linux-cli-ps-findimage.md) lub [wybierania obrazów maszyny wirtualnej systemu Windows](./virtual-machines/virtual-machines-windows-cli-ps-findimage.md).

```json
{
   "apiVersion": "2015-06-15",
   "type": "Microsoft.Compute/virtualMachines",
   "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
   "copy": {
     "name": "virtualMachineLoop",
     "count": "[variables('numberOfInstances')]"
   },
   "location": "[resourceGroup().location]",
   "dependsOn": [
     "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
     "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
     "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
   ],
   "properties": {
     "availabilitySet": {
       "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
     },
     "hardwareProfile": {
       "vmSize": "[parameters('vmSize')]"
     },
     "osProfile": {
       "computerName": "[concat(parameters('vmNamePrefix'), copyIndex())]",
       "adminUsername": "[parameters('adminUsername')]",
       "adminPassword": "[parameters('adminPassword')]"
     },
     "storageProfile": {
       "imageReference": {
         "publisher": "[parameters('imagePublisher')]",
         "offer": "[parameters('imageOffer')]",
         "sku": "[parameters('imageSKU')]",
         "version": "latest"
       },
       "osDisk": {
         "name": "osdisk",
         "vhd": {
           "uri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/vhds/','osdisk', copyindex(), '.vhd')]"
         },
         "caching": "ReadWrite",
         "createOption": "FromImage"
       }
     },
     "networkProfile": {
       "networkInterfaces": [
         {
           "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
         }
       ]
     },
     "diagnosticsProfile": {
       "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net')]"
       }
     }
}
```

>[AZURE.NOTE] W przypadku obrazów opublikowanych przez **innych dostawców** konieczne będzie określenie innej właściwości o nazwie `plan`. Przykład można znaleźć w [tym szablonie](https://github.com/Azure/azure-quickstart-templates/tree/master/checkpoint-single-nic) z galerii Szybki start. 

Zakończono definiowanie zasobów dla szablonu.

## Parametry

W sekcji parameters zdefiniuj wartości, które można określić podczas wdrażania szablonu. Zdefiniuj parametry tylko dla wartości, które powinny być zmieniane podczas wdrażania. Możliwe jest podanie wartości domyślnej parametru, która zostanie użyta, jeśli żadna wartość nie zostanie podana podczas wdrażania. Można również zdefiniować dozwolone wartości w sposób pokazany dla parametru **imageSKU**.

```json
"parameters": {
    "storageAccountName": {
      "type": "string",
      "metadata": {
        "description": "Name of storage account"
      }
    },
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Admin username"
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Admin password"
      }
    },
    "dnsNameforLBIP": {
      "type": "string",
      "metadata": {
        "description": "DNS for Load Balancer IP"
      }
    },
    "vmNamePrefix": {
      "type": "string",
      "defaultValue": "myVM",
      "metadata": {
        "description": "Prefix to use for VM names"
      }
    },
    "imagePublisher": {
      "type": "string",
      "defaultValue": "MicrosoftWindowsServer",
      "metadata": {
        "description": "Image Publisher"
      }
    },
    "imageOffer": {
      "type": "string",
      "defaultValue": "WindowsServer",
      "metadata": {
        "description": "Image Offer"
      }
    },
    "imageSKU": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter"
      ],
      "metadata": {
        "description": "Image SKU"
      }
    },
    "lbName": {
      "type": "string",
      "defaultValue": "myLB",
      "metadata": {
        "description": "Load Balancer name"
      }
    },
    "nicNamePrefix": {
      "type": "string",
      "defaultValue": "nic",
      "metadata": {
        "description": "Network Interface name prefix"
      }
    },
    "publicIPAddressName": {
      "type": "string",
      "defaultValue": "myPublicIP",
      "metadata": {
        "description": "Public IP Name"
      }
    },
    "vnetName": {
      "type": "string",
      "defaultValue": "myVNET",
      "metadata": {
        "description": "VNET name"
      }
    },
    "vmSize": {
      "type": "string",
      "defaultValue": "Standard_D1",
      "metadata": {
        "description": "Size of the VM"
      }
    }
  }
```

## Zmienne

W sekcji variables można zdefiniować wartości, które są używane w więcej niż jednym miejscu w szablonie, lub wartości, które są tworzone na podstawie innych wyrażeń lub zmiennych. Zmienne są często używane w celu uproszczenia składni szablonu.

```json
"variables": {
    "availabilitySetName": "myAvSet",
    "subnetName": "Subnet-1",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
    "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
    "numberOfInstances": 2,
    "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
    "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LoadBalancerFrontEnd')]",
    "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/BackendPool1')]",
    "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
  }
```

Zakończono tworzenie szablonu! Możesz porównać swój szablon z pełnym szablonem znajdującym się w [galerii Szybki start](https://github.com/Azure/azure-quickstart-templates) pod nazwą [2 VMs with load balancer and load balancer rules template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-2-vms-loadbalancer-lbrules) (Dwie maszyny wirtualne z modułem równoważenia obciążenia i regułami modułu równoważenia obciążenia). Twój szablon może być nieco inny ze względu na użycie różnych numerów wersji. 

Możesz ponownie wdrożyć szablon przy użyciu tych samych poleceń, które zostały użyte podczas wdrażania konta magazynu. Nie ma potrzeby usuwania konta magazynu przed ponownym wdrażaniem, ponieważ usługa Resource Manager pominie proces ponownego tworzenia zasobów, które już istnieją i nie uległy zmianie.

## Następne kroki

- [Azure Resource Manager Template Visualizer (ARMViz)](http://armviz.io/#/) to doskonałe narzędzie do wizualizacji szablonów ARM, ponieważ mogą one stać się zbyt duże, by były zrozumiałe tylko na podstawie odczytu pliku JSON.
- Aby uzyskać więcej informacji o strukturze szablonu, zobacz [Tworzenie szablonów usługi Azure Resource Manager](resource-group-authoring-templates.md).
- Aby dowiedzieć się więcej o wdrażaniu szablonu, zobacz [Wdrażanie grupy zasobów za pomocą szablonu usługi Azure Resource Manager](resource-group-template-deploy.md).



<!--HONumber=Sep16_HO3-->


