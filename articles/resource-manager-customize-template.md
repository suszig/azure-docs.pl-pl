<properties
    pageTitle="Dostosowywanie wyeksportowanego szablonu usługi Azure Resource Manager | Microsoft Azure"
    description="Dodawanie parametrów do wyeksportowanego szablonu usługi Azure Resource Manager i ponowne wdrażanie szablonu przy użyciu programu Azure PowerShell lub interfejsu wiersza polecenia platformy Azure."
    services="azure-resource-manager"
    documentationCenter=""
    authors="tfitzmac"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.workload="multiple"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/01/2016"
    ms.author="tomfitz"/>


# Dostosowywanie wyeksportowanego szablonu usługi Azure Resource Manager

W tym artykule opisano sposób modyfikowania wyeksportowanego szablonu w celu przekazania dodatkowych wartości jako parametrów. Opiera się on na krokach wykonanych w artykule [Eksportowanie szablonu usługi Resource Manager](resource-manager-export-template.md), ale wykonanie w pierwszej kolejności czynności opisanych w tym artykule nie jest konieczne. Wymagany szablon oraz skrypty można znaleźć w tym artykule.

## Wyświetlanie wyeksportowanego szablonu

Jeśli wykonano czynności opisane w artykule [Eksportowanie szablonu usługi Resource Manager](resource-manager-export-template.md), otwórz pobrany szablon. Jest to szablon o nazwie **template.json**. Jeśli masz program Visual Studio lub Visual Code, możesz użyć dowolnego z nich, aby edytować szablon. W przeciwnym razie możesz użyć dowolnego edytora JSON lub edytora tekstów.

Jeśli nie wykonano czynności opisanych w poprzednim przewodniku, utwórz plik o nazwie **template.json** i dodaj do tego pliku następującą zawartość z wyeksportowanego szablonu.

```
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "virtualNetworks_VNET_name": {
            "defaultValue": "VNET",
            "type": "String"
        },
        "storageAccounts_storagetf05092016_name": {
            "defaultValue": "storagetf05092016",
            "type": "String"
        }
    },
    "variables": {},
    "resources": [
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetworks_VNET_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "10.0.0.0/16"
                    ]
                },
                "subnets": [
                    {
                        "name": "default",
                        "properties": {
                            "addressPrefix": "10.0.0.0/24"
                        }
                    }
                ]
            },
            "dependsOn": []
        },
        {
            "comments": "Generalized from resource in walkthrough.",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccounts_storagetf05092016_name')]",
            "apiVersion": "2015-06-15",
            "location": "northeurope",
            "tags": {},
            "properties": {
                "accountType": "Standard_RAGRS"
            },
            "dependsOn": []
        }
    ]
}
```

Szablon template.json będzie działać prawidłowo, jeśli dla każdego wdrożenia chcesz utworzyć ten sam typ konta magazynu w tym samym regionie z siecią wirtualną, która używa tego samego prefiksu adresu i prefiksu podsieci. Usługa Resource Manager oferuje jednak opcje, dzięki którym można wdrażać szablony ze znacznie większą elastycznością. Podczas wdrażania możesz na przykład określić typ tworzonego konta magazynu lub wartości, które mają zostać użyte jako prefiks adresu sieci wirtualnej i prefiks podsieci.

## Dostosowywanie szablonu

W tej sekcji do wyeksportowanego szablonu zostaną dodane parametry umożliwiające ponowne użycie szablonu podczas wdrażania tych zasobów w innych środowiskach. Do szablonu zostaną dodane również pewne funkcje mające na celu ograniczenie prawdopodobieństwa wystąpienia błędu podczas wdrażania szablonu. Nie będzie już trzeba odgadywać unikatowej nazwy konta magazynu. Zamiast tego unikatowa nazwa zostanie utworzona przez szablon. Wartości, które można określić dla typu konta magazynu, zostaną ograniczone tylko do prawidłowych opcji.

1. Aby móc przekazać wartości, które można określić podczas wdrażania, zastąp sekcję **parameters** poniższymi definicjami parametrów. Zwróć uwagę na wartości parametrów **allowedValues** dla zmiennej **storageAccount_accountType**. W przypadku niezamierzonego wprowadzenia nieprawidłowej wartości ten błąd zostanie rozpoznany przed rozpoczęciem wdrażania. Zauważ również, że podawany jest tylko prefiks nazwy konta magazynu, a jego długość jest ograniczona do 11 znaków. Ograniczenie długości prefiksu do 11 znaków pozwala mieć pewność, że pełna nazwa nie przekroczy maksymalnej liczby znaków dla konta magazynu. Prefiks umożliwia zastosowanie konwencji nazewnictwa do kont magazynu. Sposób tworzenia unikatowej nazwy przedstawiono w kolejnym kroku.

        "parameters": {
          "storageAccount_prefix": {
            "type": "string",
            "maxLength": 11
          },
          "storageAccount_accountType": {
            "defaultValue": "Standard_RAGRS",
            "type": "string",
            "allowedValues": [
              "Standard_LRS",
              "Standard_ZRS",
              "Standard_GRS",
              "Standard_RAGRS",
              "Premium_LRS"
            ]
          },
          "virtualNetwork_name": {
            "type": "string"
          },
          "addressPrefix": {
            "defaultValue": "10.0.0.0/16",
            "type": "string"
          },
          "subnetName": {
            "defaultValue": "subnet-1",
            "type": "string"
          },
          "subnetAddressPrefix": {
            "defaultValue": "10.0.0.0/24",
            "type": "string"
          }
        },

2. Sekcja **variables** szablonu jest obecnie pusta. Zastąp tę sekcję poniższym kodem. Jako autor szablonu w sekcji **variables** możesz tworzyć wartości, które upraszczają składnię w pozostałej części szablonu. Zmienna **storageAccount_name** łączy prefiks z parametru z unikatowym ciągiem generowanym na podstawie identyfikatora grupy zasobów.

        "variables": {
          "storageAccount_name": "[concat(parameters('storageAccount_prefix'), uniqueString(resourceGroup().id))]"
        },

3. Aby użyć tych parametrów i zmiennej w definicjach zasobów, zastąp sekcję **resources** poniższymi definicjami. Zauważ, że rzeczywiste zmiany w definicjach zasobów, oprócz wartości przypisanej do właściwości zasobu, są niewielkie. Właściwości są dokładnie takie same, jak właściwości z wyeksportowanego szablonu. Właściwości są po prostu przypisywane do wartości parametrów zamiast do zakodowanych wartości. Za pomocą wyrażenia **resourceGroup().location**dla lokalizacji zasobów ustawiono użycie tej samej lokalizacji, w której znajduje się grupa zasobów. Wyrażenie **variables** odwołuje się do zmiennej utworzonej dla nazwy konta magazynu.

        "resources": [
          {
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('virtualNetwork_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "properties": {
              "addressSpace": {
                "addressPrefixes": [
                  "[parameters('addressPrefix')]"
                ]
              },
              "subnets": [
                {
                  "name": "[parameters('subnetName')]",
                  "properties": {
                    "addressPrefix": "[parameters('subnetAddressPrefix')]"
                  }
                }
              ]
            },
            "dependsOn": []
          },
          {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccount_name')]",
            "apiVersion": "2015-06-15",
            "location": "[resourceGroup().location]",
            "tags": {},
            "properties": {
                "accountType": "[parameters('storageAccount_accountType')]"
            },
            "dependsOn": []
          }
        ]

## Naprawianie pliku parametrów

Pobrany plik parametrów nie jest już zgodny z parametrami w szablonie. Użycie pliku parametrów nie jest konieczne, ale może on uprościć proces w przypadku ponownego wdrożenia środowiska. Użyjesz wartości domyślnych, które zostały zdefiniowane w szablonie dla wielu parametrów, aby w pliku parametrów były wymagane tylko dwie wartości.

Zastąp zawartość pliku parameters.json następującym kodem:

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageAccount_prefix": {
      "value": "storage"
    },
    "virtualNetwork_name": {
      "value": "VNET"
    }
  }
}
```

Zaktualizowany plik parametrów zawiera wartości tylko dla parametrów, które nie mają wartości domyślnej. Możesz podać wartości innych parametrów, jeśli chcesz użyć wartości innej niż wartość domyślna.

## Wdrażanie szablonu

Dostosowany szablon i pliki parametrów możesz wdrożyć przy użyciu programu Azure PowerShell interfejsu wiersza polecenia platformy Azure. Jeśli to konieczne, zainstaluj program [Azure PowerShell](powershell-install-configure.md) lub [interfejs wiersza polecenia platformy Azure](xplat-cli-install.md). Możesz użyć skryptów pobranych z szablonem podczas eksportowania oryginalnego szablonu lub napisać własny skrypt do wdrażania szablonu.
W tym artykule przedstawiono obie opcje.

2. W celu wdrożenia przy użyciu własnego skryptu użyj jednej z poniższych procedur.

     W programie PowerShell uruchom następujące polecenia:

        # login
        Add-AzureRmAccount

        # create a new resource group
        New-AzureRmResourceGroup -Name ExportGroup -Location "West Europe"

        # deploy the template to the resource group
        New-AzureRmResourceGroupDeployment -Name ExampleDeployment -ResourceGroupName ExportGroup -TemplateFile {path-to-file}\template.json -TemplateParameterFile {path-to-file}\parameters.json

     W interfejsie wiersza polecenia platformy Azure uruchom następujące polecenia:

        azure login

        azure group create -n ExportGroup -l "West Europe"

        azure group deployment create -f {path-to-file}\azuredeploy.json -e {path-to-file}\parameters.json -g ExportGroup -n ExampleDeployment

3. Jeśli pobrano wyeksportowany szablon i skrypty, znajdź plik **deploy.ps1** (dla programu PowerShell) lub plik **deploy.sh** (dla interfejsu wiersza polecenia platformy Azure).

     W programie PowerShell uruchom następujące polecenia:

        Get-Item deploy.ps1 | Unblock-File

        .\deploy.ps1

     W interfejsie wiersza polecenia platformy Azure uruchom następujące polecenia:

        .\deploy.sh

## Następne kroki

- [Przewodnik po szablonie usługi Resource Manager](resource-manager-template-walkthrough.md) opiera się na informacjach uzyskanych w tym artykule w celu utworzenia szablonu dla bardziej złożonego rozwiązania. Pomaga lepiej zrozumieć dostępne zasoby oraz sposób określania wartości, które należy podać.
- Aby poznać sposób eksportowania szablonu za pomocą programu PowerShell, zobacz [Using Azure PowerShell with Azure Resource Manager](powershell-azure-resource-manager.md) (Używanie programu Azure PowerShell z usługą Azure Resource Manager).
- Aby poznać sposób eksportowania szablonu za pomocą interfejsu wiersza polecenia platformy Azure, zobacz [Use the Azure CLI for Mac, Linux, and Windows with Azure Resource Manager](xplat-cli-azure-resource-manager.md) (Używanie interfejsu wiersza polecenia platformy Azure na komputerach Mac i komputerach z systemem Linux oraz Windows z usługą Azure Resource Manager).
- Aby uzyskać więcej informacji o strukturze szablonów, zobacz [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager).



<!--HONumber=Sep16_HO3-->


