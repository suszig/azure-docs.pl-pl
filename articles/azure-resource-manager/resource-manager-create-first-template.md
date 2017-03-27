---
title: "Tworzenie pierwszego szablonu usługi Azure Resource Manager | Microsoft Docs"
description: "Przewodnik krok po kroku dotyczący tworzenia pierwszego szablonu usługi Azure Resource Manager. Przewodnik pokazuje sposób użycia odwołania do szablonu dla konta magazynu w celu utworzenia szablonu."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/21/2017
ms.topic: get-started-article
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: f861f2ba0376f00ff21e03cbb81a04a266f38630
ms.lasthandoff: 03/22/2017

---

# <a name="create-your-first-azure-resource-manager-template"></a>Tworzenie pierwszego szablonu usługi Azure Resource Manager
W tym temacie szczegółowo omówiono kroki tworzenia pierwszego szablonu usługi Azure Resource Manager. Szablony usługi Resource Manager są plikami JSON definiującymi zasoby, które należy wdrożyć dla danego rozwiązania. Aby zrozumieć pojęcia związane z wdrażaniem rozwiązań platformy Azure i zarządzaniem nimi, zobacz [Usługa Azure Resource Manager — omówienie](resource-group-overview.md). Jeśli masz istniejące zasoby i chcesz uzyskać szablon dla tych zasobów, zobacz [Eksportowanie szablonu usługi Azure Resource Manager z istniejących zasobów](resource-manager-export-template.md).

Aby utworzyć i sprawdzić szablony, potrzebujesz edytora plików JSON. [Visual Studio Code](https://code.visualstudio.com/) to lekki edytor kodu typu open-source dla wielu platform. Obsługuje tworzenie i edytowanie szablonów usługi Resource Manager za pomocą rozszerzenia. W tym temacie założono, że używasz programu VS Code. Jeśli jednak masz inny edytor plików JSON (np. program Visual Studio), możesz użyć tego edytora.

## <a name="get-vs-code-and-extension"></a>Pobieranie narzędzia VS Code i rozszerzenia
1. W razie potrzeby zainstaluj narzędzie VS Code ze strony [https://code.visualstudio.com/](https://code.visualstudio.com/).

2. Zainstaluj rozszerzenie [Narzędzia usługi Azure Resource Manager](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools), przechodząc do funkcji szybkiego otwierania (CTRL+P) i uruchamiając: 

  ```
  ext install msazurermtools.azurerm-vscode-tools
  ```

3. Uruchom ponownie program VS Code po wyświetleniu monitu, aby włączyć rozszerzenie.

## <a name="create-blank-template"></a>Tworzenie pustego szablonu

Zacznijmy od pustego szablonu, który obejmuje tylko podstawowe sekcje.

1. Utwórz plik. 

2. Skopiuj i wklej następującą składnię JSON do pliku:

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

3. Zapisz plik jako **azuredeploy.json**. 

## <a name="add-storage-account"></a>Dodaj konto magazynu
1. Aby zdefiniować konto magazynu do wdrożenia, należy dodać konto magazynu do sekcji **resources** szablonu. Aby znaleźć wartości dostępne dla konta magazynu, zobacz [odwołanie do szablonu kont magazynu](/azure/templates/microsoft.storage/storageaccounts). Skopiuj kod JSON wyświetlany dla konta magazynu. 

3. Wklej kod JSON do sekcji **resources** szablonu, jak pokazano w poniższym przykładzie: 

  ```json
  {
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {  },
    "variables": {  },
    "resources": [
      {
        "name": "string",
        "type": "Microsoft.Storage/storageAccounts",
        "apiVersion": "2016-05-01",
        "sku": {
          "name": "string"
        },
        "kind": "string",
        "location": "string",
        "tags": {},
        "properties": {
          "customDomain": {
            "name": "string",
            "useSubDomain": boolean
          },
          "encryption": {
            "services": {
              "blob": {
                "enabled": boolean
              }
            },
            "keySource": "Microsoft.Storage"
          },
          "accessTier": "string"
        }
      }
    ],
    "outputs": {  }
  }
  ```

  Poprzedni przykład zawiera wiele wartości symboli zastępczych i kilka właściwości, których możesz nie potrzebować na swoim koncie magazynu.

## <a name="set-values-for-storage-account"></a>Ustawianie wartości dla konta magazynu

Teraz możesz ustawić wartości konta magazynu. 

1. Przyjrzyj się ponownie [odwołaniu do szablonu kont magazynu](/azure/templates/microsoft.storage/storageaccounts), do którego skopiowano kod JSON. Istnieje kilka tabel, które opisują właściwości i zapewniają dostępne wartości. 

2. Zauważ, że w elemencie **properties** elementy **customDomain**, **encryption** i **accessTier** są oznaczone jako niewymagane. Wartości te mogą być ważne w danych scenariuszach, ale w celu uproszczenia tego przewodnika usuńmy je.

  ```json
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "string"
      },
      "kind": "string",
      "location": "string",
      "tags": {},
      "properties": {
      }
    }
  ],
  ```

3. Obecnie element **kind** jest ustawiony na wartość symbolu zastępczego („string”). Program VS Code zawiera wiele funkcji, które ułatwiają zrozumienie wartości do wykorzystania w szablonie. Zauważ, że program VS Code wskazuje, że ta wartość jest nieprawidłowa. Jeśli zatrzymasz wskaźnik myszy nad elementem „string”, program VS Code zasugeruje, że prawidłowe wartości dla elementu **kind** to `Storage` lub `BlobStorage`. 

  ![wyświetlanie sugerowanych wartości programu VS Code](./media/resource-manager-create-first-template/vs-code-show-values.png)

  Aby zobaczyć dostępne wartości, usuń znaki między podwójnymi cudzysłowami i naciśnij klawisze **Ctrl+spacja**. Wybierz pozycję **Magazyn** z listy dostępnych opcji.
  
  ![wyświetlanie funkcji intellisense](./media/resource-manager-create-first-template/intellisense.png)

  Jeśli nie używasz programu VS Code, zobacz stronę odwołania do szablonu kont magazynu. Zauważ, że opis zawiera listę tych samych prawidłowych wartości. Ustaw element na opcję **Magazyn**.

  ```json
  "kind": "Storage",
  ```

Twój szablon wygląda teraz następująco:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "string",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-template-function"></a>Dodawanie funkcji szablonu

W szablonie używa się funkcji, aby uprościć jego składnię i pobierać wartości, które są dostępne wyłącznie w momencie wdrażania szablonu. Aby uzyskać pełny zestaw funkcji szablonu, zobacz [Funkcje szablonu usługi Azure Resource Manager](resource-group-template-functions.md).

Aby określić wdrożenie konta magazynu do tej samej lokalizacji, w której znajduje się grupa zasobów, ustaw właściwość **location** na:

```json
"location": "[resourceGroup().location]",
```

Ponownie program VS Code zapewni pomoc w postaci sugerowanych dostępnych funkcji. 

![wyświetlanie funkcji](./media/resource-manager-create-first-template/show-functions.png)

Zauważ, że funkcja jest ujęta w nawiasy kwadratowe. Funkcja [resourceGroup](resource-group-template-functions.md#resourcegroup) zwraca obiekt z właściwością o nazwie `location`. Grupa zasobów zawiera wszystkie zasoby dotyczące danego rozwiązania. Możesz zakodować właściwość lokalizacji na wartość taką jak „Środkowe stany USA”, ale będzie konieczna ręczna zmiana szablonu w celu jego ponownego wdrożenia w innej lokalizacji. Użycie funkcji `resourceGroup` ułatwia ponowne wdrażanie tego szablonu w innej grupie zasobów w innej lokalizacji.



Twój szablon wygląda teraz następująco:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {  },
  "variables": {  },
  "resources": [
    {
      "name": "string",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "string"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="add-parameters-and-variables"></a>Dodawanie parametrów i zmiennych
Pozostały już tylko dwie wartości do ustawienia w tym szablonie — **name** i **sku.name**. W przypadku tych właściwości należy dodać parametry, które umożliwiają konfigurowanie tych wartości podczas wdrożenia. 

Nazwy kont magazynu podlegają kilku ograniczeniom, które utrudniają ich skonfigurowanie. Nazwa musi mieć od 3 do 24 znaków długości, zawierać wyłącznie cyfry i małe litery oraz musi być unikatowa. Zamiast próbowania odgadnięcia unikatowej wartości spełniającej te ograniczenia, możesz użyć funkcji [uniqueString](resource-group-template-functions.md#uniquestring), aby wygenerować wartość skrótu. Aby wartość skrótu była znacząca, dodaj prefiks, dzięki któremu zidentyfikujesz ją jako konto magazynu po wdrożeniu. 

1. Aby przekazać nazwie prefiks spełniający stosowane konwencje nazewnictwa, przejdź do sekcji **parameters** szablonu. Dodaj parametr do szablonu, który akceptuje prefiks dla nazwy konta magazynu:

  ```json
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    }
  },
  ```

  Długość prefiksu jest ograniczona do 11 znaków, ponieważ funkcja `uniqueString` zwraca 13 znaków, a nazwa nie może przekraczać 24 znaków. Jeśli nie przekażesz wartości parametru podczas wdrożenia, zostanie użyta wartość domyślna.

2. Przejdź do sekcji **variables** szablonu. Aby skonstruować nazwę z prefiksu i unikatowego ciągu, dodaj następującą zmienną:

  ```json
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  ```

3. W sekcji **resources** ustaw nazwę konta magazynu na tę zmienną.

  ```json
  "name": "[variables('storageName')]",
  ```

3. Aby włączyć przekazywanie w innych jednostkach SKU dla konta magazynu, przejdź do sekcji **parameters**. Po parametrze dla prefiksu nazwy magazynu dodaj parametr, który określa dozwolone wartości SKU i wartość domyślną. Możesz znaleźć dozwolone wartości na stronie z odwołaniem do szablonu lub w programie VS Code. W poniższym przykładzie uwzględnia się wszystkie prawidłowe wartości dla jednostki SKU. Niemniej możesz ograniczyć dozwolone wartości tylko do tych typów jednostek SKU, które chcesz wdrażać przy użyciu tego szablonu.

  ```json
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  ```

3. Zmień właściwość SKU, aby używać wartości z innego parametru:

  ```json
  "sku": {
    "name": "[parameters('storageSKU')]"
  },
  ```    

4. Zapisz plik.

Twój szablon wygląda teraz następująco:

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageNamePrefix": {
      "type": "string",
      "maxLength": 11,
      "defaultValue": "storage",
      "metadata": {
        "description": "The value to use for starting the storage account name."
      }
    },
    "storageSKU": {
      "type": "string",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS",
        "Premium_LRS"
      ],
      "defaultValue": "Standard_LRS",
      "metadata": {
        "description": "The type of replication to use for the storage account."
      }
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storageNamePrefix'), uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "name": "[variables('storageName')]",
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2016-05-01",
      "sku": {
        "name": "[parameters('storageSKU')]"
      },
      "kind": "Storage",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
      }
    }
  ],
  "outputs": {  }
}
```

## <a name="next-steps"></a>Następne kroki
* Szablon jest gotowy. Możesz rozpocząć wdrażanie szablonu do subskrypcji. Aby wdrożyć szablon, zobacz [Wdrażanie zasobów na platformie Azure](resource-manager-quickstart-deploy.md).
* Aby uzyskać więcej informacji o strukturze szablonu, zobacz [Tworzenie szablonów usługi Azure Resource Manager](resource-group-authoring-templates.md).
* Aby wyświetlić pełną listę szablonów dla wielu różnych rozwiązań, zobacz [Szablony szybkiego startu platformy Azure](https://azure.microsoft.com/documentation/templates/).

