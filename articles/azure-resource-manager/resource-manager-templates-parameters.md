---
title: "Sekcji parametr szablonu usługi Azure Resource Manager | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano w sekcji parametrów szablonów usługi Azure Resource Manager za pomocą składni deklaratywnej JSON."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/11/2017
ms.author: tomfitz
ms.openlocfilehash: 7d0f53751bf529d52c156a8b9319b10560eb8997
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/12/2017
---
# <a name="parameters-section-of-azure-resource-manager-templates"></a>Sekcja parametrów szablonów usługi Azure Resource Manager
W sekcji Parametry szablonu można określić wartości, które można wprowadzić podczas wdrażania zasobów. Wartości tych parametrów umożliwiają dostosowanie wdrożenie, podając wartości, które są dostosowane określonym środowisku (na przykład deweloperów, testowego i produkcyjnego). Nie musisz podać parametry w szablonie, ale bez parametrów szablonu będzie zawsze wdrażać te same zasoby z tej samej nazwy, lokalizacji i właściwości.

Istnieje ograniczenie maksymalnie 255 parametrów w szablonie. Liczba parametrów można zmniejszyć przy użyciu obiektów, które zawierają wiele właściwości, jako Pokaż w tym artykule.

## <a name="define-and-use-a-parameter"></a>Definiowanie i korzystanie z parametrem

Poniższy przykład przedstawia definicję parametru proste. Określa nazwę parametru i określa, że ma wartość ciągu. Parametr akceptuje tylko wartości odpowiednich dla jego przeznaczenia. Określa wartość domyślną, jeśli wartość nie zostanie podana podczas wdrażania. Ponadto ten parametr zawiera opis jej użycia. 

```json
"parameters": {
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
}
```

W szablonie odwołują się wartości dla parametru przy użyciu następującej składni:

```json
"resources": [
  {
    "type": "Microsoft.Storage/storageAccounts",
    "sku": {
      "name": "[parameters('storageSKU')]"
    },
    ...
  }
]
```

## <a name="available-properties"></a>Dostępne właściwości

Poprzednim przykładzie pokazano tylko niektóre właściwości, które można użyć w sekcji parametrów. Dostępne właściwości to:

```json
"parameters": {
    "<parameter-name>" : {
        "type" : "<type-of-parameter-value>",
        "defaultValue": "<default-value-of-parameter>",
        "allowedValues": [ "<array-of-allowed-values>" ],
        "minValue": <minimum-value-for-int>,
        "maxValue": <maximum-value-for-int>,
        "minLength": <minimum-length-for-string-or-array>,
        "maxLength": <maximum-length-for-string-or-array-parameters>,
        "metadata": {
            "description": "<description-of-the parameter>" 
        }
    }
}
```

| Nazwa elementu | Wymagane | Opis |
|:--- |:--- |:--- |
| Nazwa parametru |Tak |Nazwa parametru. Musi być prawidłowym identyfikatorem języka JavaScript. |
| type |Tak |Typ wartości parametru. Dozwolonymi typami i wartości są **ciąg**, **secureString**, **int**, **bool**, **obiektu**, **secureObject**, i **tablicy**. |
| Wartość domyślna |Nie |Wartość domyślna parametru, jeśli wartość nie zostanie podana dla parametru. |
| allowedValues |Nie |Tablica dozwolonych wartości tego parametru upewnić się, że podano wartość prawej strony. |
| Wartość MinValue |Nie |Minimalna wartość parametrów typu int, ta wartość jest włącznie. |
| MaxValue |Nie |Maksymalna wartość dla parametrów typu int, ta wartość jest włącznie. |
| Element minLength |Nie |Minimalna długość ciągu, secureString i parametrów typu tablicy, ta wartość jest włącznie. |
| Element maxLength |Nie |Maksymalna długość ciągu, secureString i parametrów typu tablicy, ta wartość jest włącznie. |
| description |Nie |Opis parametru, który będzie wyświetlany użytkownikom za pośrednictwem portalu. |

## <a name="template-functions-with-parameters"></a>Funkcje szablonów z parametrami

Podczas tworzenia wartości domyślnej dla parametru, możesz użyć większości funkcji szablonu. Inną wartość parametru służy do tworzenia wartości domyślnej. Następujący szablon przedstawia użycie funkcji wartość domyślna:

```json
"parameters": {
  "siteName": {
    "type": "string",
    "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]",
    "metadata": {
      "description": "The site name. To use the default value, do not specify a new value."
    }
  },
  "hostingPlanName": {
    "type": "string",
    "defaultValue": "[concat(parameters('siteName'),'-plan')]",
    "metadata": {
      "description": "The host name. To use the default value, do not specify a new value."
    }
  }
}
```

Nie można użyć `reference` funkcji w sekcji parametrów. Parametry są oceniane przed wdrożeniem więc `reference` funkcji nie można uzyskać stanu środowiska uruchomieniowego zasobu. 

## <a name="objects-as-parameters"></a>Obiekty jako parametrów

Może być łatwiejsze do organizowania powiązanych wartości, przekazując je jako obiekt. Takie podejście zmniejsza się liczba parametrów w szablonie.

Zdefiniuj dany parametr w szablonie i określ obiekt JSON zamiast pojedynczej wartości podczas wdrażania. 

```json
"parameters": {
  "VNetSettings": {
    "type": "object",
    "defaultValue": {
      "name": "VNet1",
      "addressPrefixes": [
        {
          "name": "firstPrefix",
          "addressPrefix": "10.0.0.0/22"
        }
      ],
      "subnets": [
        {
          "name": "firstSubnet",
          "addressPrefix": "10.0.0.0/24"
        },
        {
          "name": "secondSubnet",
          "addressPrefix": "10.0.1.0/24"
        }
      ]
    }
  }
},
```

Następnie odwoływać właściwości parametru za pomocą operatora kropki.

```json
"resources": [
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[parameters('VNetSettings').name]",
    "location":"[resourceGroup().location]",
    "properties": {
      "addressSpace":{
        "addressPrefixes": [
          "[parameters('VNetSettings').addressPrefixes[0].addressPrefix]"
        ]
      },
      "subnets":[
        {
          "name":"[parameters('VNetSettings').subnets[0].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[0].addressPrefix]"
          }
        },
        {
          "name":"[parameters('VNetSettings').subnets[1].name]",
          "properties": {
            "addressPrefix": "[parameters('VNetSettings').subnets[1].addressPrefix]"
          }
        }
      ]
    }
  }
]
```

## <a name="recommendations"></a>Zalecenia
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

* Zdefiniuj wartości domyślne parametrów (z wyjątkiem hasła i klucze SSH). Podając wartości domyślnej parametru staje się opcjonalne podczas wdrażania. Wartość domyślna może być pustym ciągiem. 
   
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

* Użyj **SecureString** dla wszystkich haseł i kluczy tajnych. W przypadku przekazania danych poufnych w obiekcie JSON, użyj **secureObject** typu. Nie można odczytać parametrów szablonu z typami secureString lub secureObject po wdrożeniu zasobów. 
   
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
   
   Typ zasobu jest obsługiwana w ograniczonej liczby miejsc, można określić prawidłowej lokalizacji bezpośrednio w szablonie. Jeśli musisz użyć **lokalizacji** parametru udostępniać tę wartość parametru możliwie zasobów, które mogą być w tej samej lokalizacji. Takie podejście minimalizuje liczbę razy, które użytkownicy są proszeni o dostarczenie informacji o lokalizacji.
* Unikaj używania parametr lub zmienna dla wersji interfejsu API dla typu zasobu. Właściwości zasobów i wartości może się różnić przez numer wersji. IntelliSense w edytorze kodu nie można ustalić prawidłowego schematu, gdy parametr lub zmienna ma wartość wersja interfejsu API. Zamiast tego wersji interfejsu API zakodowane w szablonie.
* Unikaj określania nazwy parametru w szablonie pasujący parametr w poleceniu wdrożenia. Menedżer zasobów jest rozpoznawany jako ten konflikt nazw, dodając przyrostek **FromTemplate** do parametru szablonu. Na przykład, jeśli zawiera parametr o nazwie **ResourceGroupName** w szablonie, powoduje konflikt z **ResourceGroupName** parametru w [AzureRmResourceGroupDeployment nowy](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) polecenia cmdlet. Podczas wdrażania, monit o podanie wartości **ResourceGroupNameFromTemplate**.

## <a name="example-templates"></a>Przykład szablonów

Te szablony przykładowe pokazują niektóre scenariusze korzystania z parametrów. Ich do testowania, jak parametry są obsługiwane w różnych scenariuszy wdrażania.

|Szablon  |Opis  |
|---------|---------|
|[Parametry funkcji dla wartości domyślnych](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterswithfunctions.json) | Pokazuje, jak używać funkcji szablonu, definiując wartości domyślne parametrów. Szablon nie wdrażać żadnych zasobów. Konstruuje wartości parametrów, a zwraca tych wartości. |
|[Parametr obiektu](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/parameterobject.json) | Pokazuje, dla parametru za pomocą obiektu. Szablon nie wdrażać żadnych zasobów. Konstruuje wartości parametrów, a zwraca tych wartości. |

## <a name="next-steps"></a>Następne kroki

* Aby wyświetlić pełną listę szablonów dla wielu różnych rozwiązań, zobacz [Szablony szybkiego startu platformy Azure](https://azure.microsoft.com/documentation/templates/).
* Jak wartości parametrów wejściowych podczas wdrażania, zobacz [wdrażania aplikacji przy użyciu szablonu usługi Azure Resource Manager](resource-group-template-deploy.md). 
* Aby uzyskać więcej informacji o funkcje, których można użyć z w ramach szablonu, zobacz [funkcje szablonów usługi Azure Resource Manager](resource-group-template-functions.md).
* Aby dowiedzieć się, jak za pomocą obiektu parametrów, zobacz [obiekt używany jako parametr szablonu usługi Azure Resource Manager](/azure/architecture/building-blocks/extending-templates/objects-as-parameters).