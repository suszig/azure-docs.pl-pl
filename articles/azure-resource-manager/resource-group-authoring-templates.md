---
title: "Strukturę szablonu usługi Azure Resource Manager i składni | Dokumentacja firmy Microsoft"
description: "Opis struktury i właściwości szablonów usługi Azure Resource Manager za pomocą składni deklaratywnej JSON."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 19694cb4-d9ed-499a-a2cc-bcfc4922d7f5
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/14/2017
ms.author: tomfitz
ms.openlocfilehash: dc9b64062d7f68c83aa090eec96744819a5ca423
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="understand-the-structure-and-syntax-of-azure-resource-manager-templates"></a>Struktura i składni szablonów usługi Azure Resource Manager
W tym temacie opisano strukturę szablonu usługi Azure Resource Manager. Stanowi różne sekcje szablonu i właściwości, które są dostępne w tych sekcjach. Szablon składa się z kodu JSON i wyrażeń, które służy do tworzenia wartości na potrzeby wdrożenia. Samouczek krok po kroku dotyczące tworzenia szablonu, zobacz [Tworzenie pierwszego szablonu usługi Azure Resource Manager](resource-manager-create-first-template.md).

## <a name="template-format"></a>Format szablonu
W swojej najprostszej strukturze szablonu zawiera następujące elementy:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
    "parameters": {  },
    "variables": {  },
    "resources": [  ],
    "outputs": {  }
}
```

| Nazwa elementu | Wymagane | Opis |
|:--- |:--- |:--- |
| $schema |Tak |Lokalizacja pliku schematu JSON, który zawiera opis wersji języka szablonu. Użyj adres URL wyświetlany w poprzednim przykładzie. |
| contentVersion |Tak |Wersja szablonu (na przykład 1.0.0.0). Musisz podać wartości dla tego elementu. Podczas wdrażania zasobów przy użyciu szablonu, ta wartość może służyć do upewnij się, że używany jest odpowiedniego szablonu. |
| Parametry |Nie |Wartości, które są podane podczas wdrażania jest wykonywany w celu dostosowania wdrożenia zasobów. |
| zmienne |Nie |Wartości, które są używane jako fragmenty JSON w szablonie, aby uprościć wyrażeń języka szablonu. |
| Zasoby |Tak |Typy zasobów, które są wdrożone lub zaktualizowane w grupie zasobów. |
| dane wyjściowe |Nie |Wartości, które są zwracane po wdrożeniu. |

Każdy element zawiera właściwości, które można ustawić. Poniżej przedstawiono przykład zawierający pełnej składni szablonu:

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "",
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
    },
    "variables": {  
        "<variable-name>": "<variable-value>",
        "<variable-name>": { 
            <variable-complex-type-value> 
        }
    },
    "resources": [
      {
          "condition": "<boolean-value-whether-to-deploy>",
          "apiVersion": "<api-version-of-resource>",
          "type": "<resource-provider-namespace/resource-type-name>",
          "name": "<name-of-the-resource>",
          "location": "<location-of-resource>",
          "tags": {
              "<tag-name1>": "<tag-value1>",
              "<tag-name2>": "<tag-value2>"
          },
          "comments": "<your-reference-notes>",
          "copy": {
              "name": "<name-of-copy-loop>",
              "count": "<number-of-iterations>",
              "mode": "<serial-or-parallel>",
              "batchSize": "<number-to-deploy-serially>"
          },
          "dependsOn": [
              "<array-of-related-resource-names>"
          ],
          "properties": {
              "<settings-for-the-resource>",
              "copy": [
                  {
                      "name": ,
                      "count": ,
                      "input": {}
                  }
              ]
          },
          "resources": [
              "<array-of-child-resources>"
          ]
      }
    ],
    "outputs": {
        "<outputName>" : {
            "type" : "<type-of-output-value>",
            "value": "<output-value-expression>"
        }
    }
}
```

Omówione części szablonu bardziej szczegółowo w dalszej części tego tematu.

## <a name="expressions-and-functions"></a>Wyrażeń i funkcji
Podstawowa składnia szablonu jest JSON. Jednak wyrażeń i funkcji rozszerzyć dostępnych w szablonie wartości JSON.  Wyrażenia są zapisywane w literałach ciągu JSON, których pierwszy i ostatnie znaki są nawiasy: `[` i `]`odpowiednio. Wartość wyrażenia jest oceniane podczas wdrażania szablonu. Podczas zapisywania jako literału ciągu, wynik obliczania wyrażenia może być innego typu JSON, takich jak tablicy lub liczba całkowita, w zależności od rzeczywistej wyrażenia.  Mieć literałem rozpoczynać nawiasu `[`, ale nie została ona interpretowana jako wyrażenie, Dodaj dodatkowe nawiasu zacząć ciąg z `[[`.

Zazwyczaj umożliwia wyrażenia funkcji wykonywać operacje związane z konfigurowaniem wdrażania. Po prostu, tak jak w języku JavaScript, wywołania funkcji są sformatowane jako `functionName(arg1,arg2,arg3)`. Możesz odwoływać się do właściwości przy użyciu operatorów kropka i [Indeks].

Poniższy przykład przedstawia użycie kilku funkcji podczas tworzenia wartości:

```json
"variables": {
    "location": "[resourceGroup().location]",
    "usernameAndPassword": "[concat(parameters('username'), ':', parameters('password'))]",
    "authorizationHeader": "[concat('Basic ', base64(variables('usernameAndPassword')))]"
}
```

Aby uzyskać pełną listę funkcji szablonu, zobacz [funkcje szablonu usługi Azure Resource Manager](resource-group-template-functions.md). 

## <a name="parameters"></a>Parametry
W sekcji Parametry szablonu można określić wartości, które można wprowadzić podczas wdrażania zasobów. Wartości tych parametrów umożliwiają dostosowanie wdrożenie, podając wartości, które są dostosowane określonym środowisku (na przykład deweloperów, testowego i produkcyjnego). Nie musisz podać parametry w szablonie, ale bez parametrów szablonu będzie zawsze wdrażać te same zasoby z tej samej nazwy, lokalizacji i właściwości.

Definiuje parametry o następującej strukturze:

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
| type |Tak |Typ wartości parametru. Zobacz listę dozwolonych typów pod tą tabelą. |
| Wartość domyślna |Nie |Wartość domyślna parametru, jeśli wartość nie zostanie podana dla parametru. |
| allowedValues |Nie |Tablica dozwolonych wartości tego parametru upewnić się, że podano wartość prawej strony. |
| Wartość MinValue |Nie |Minimalna wartość parametrów typu int, ta wartość jest włącznie. |
| MaxValue |Nie |Maksymalna wartość dla parametrów typu int, ta wartość jest włącznie. |
| Element minLength |Nie |Minimalna długość ciągu, secureString i parametrów typu tablicy, ta wartość jest włącznie. |
| Element maxLength |Nie |Maksymalna długość ciągu, secureString i parametrów typu tablicy, ta wartość jest włącznie. |
| Opis elementu |Nie |Opis parametru, który będzie wyświetlany użytkownikom za pośrednictwem portalu. |

Są dozwolonymi typami i wartości:

* **ciąg**
* **secureString**
* **int**
* **wartość logiczna**
* **obiekt** 
* **secureObject**
* **Tablica**

Aby określić parametr jako opcjonalne, należy podać właściwość defaultValue (może być ciągiem pustym). 

Jeśli określono nazwę parametru w szablonie parametr w poleceniu, aby wdrożyć szablon jest zgodny, jest potencjalnych niejednoznaczności wartości podane przez użytkownika. Menedżer zasobów usuwa to pomyłka, dodając przyrostek **FromTemplate** do parametru szablonu. Na przykład, jeśli zawiera parametr o nazwie **ResourceGroupName** w szablonie, powoduje konflikt z **ResourceGroupName** parametru w [AzureRmResourceGroupDeployment nowy](/powershell/module/azurerm.resources/new-azurermresourcegroupdeployment) polecenia cmdlet. Podczas wdrażania, monit o podanie wartości **ResourceGroupNameFromTemplate**. Ogólnie rzecz biorąc należy unikać to pomyłka nie nadawanie nazw parametrów o takiej samej nazwie jako parametry używane dla operacji wdrożenia.

> [!NOTE]
> Wszystkie hasła, klucze i innych informacji poufnych, należy użyć **secureString** typu. W przypadku przekazania danych poufnych w obiekcie JSON, użyj **secureObject** typu. Nie można odczytać parametrów szablonu z typami secureString lub secureObject po wdrożeniu zasobów. 
> 
> Na przykład następujący wpis w historii wdrożenia zawiera wartość dla ciągu lub obiektu, ale nie dla secureString i secureObject.
>
> ![Pokaż wartości wdrożenia](./media/resource-group-authoring-templates/show-parameters.png)  
>

Poniższy przykład przedstawia sposób definiowania parametrów:

```json
"parameters": {
    "siteName": {
        "type": "string",
        "defaultValue": "[concat('site', uniqueString(resourceGroup().id))]"
    },
    "hostingPlanName": {
        "type": "string",
        "defaultValue": "[concat(parameters('siteName'),'-plan')]"
    },
    "skuName": {
        "type": "string",
        "defaultValue": "F1",
        "allowedValues": [
          "F1",
          "D1",
          "B1",
          "B2",
          "B3",
          "S1",
          "S2",
          "S3",
          "P1",
          "P2",
          "P3",
          "P4"
        ]
    },
    "skuCapacity": {
        "type": "int",
        "defaultValue": 1,
        "minValue": 1
    }
}
```

Jak wartości parametrów wejściowych podczas wdrażania, zobacz [wdrażania aplikacji przy użyciu szablonu usługi Azure Resource Manager](resource-group-template-deploy.md). 

## <a name="variables"></a>Zmienne
W sekcji variables można skonstruować wartości, które mogą być używane w szablonie. Nie trzeba zdefiniować zmienne, ale one często uprościć szablonu zmniejszając złożonych wyrażeń.

Można zdefiniować zmienne o następującej strukturze:

```json
"variables": {
    "<variable-name>": "<variable-value>",
    "<variable-name>": { 
        <variable-complex-type-value> 
    }
}
```

Poniższy przykład przedstawia sposób zdefiniowania zmiennej, który jest tworzony z dwóch wartości parametrów:

```json
"variables": {
    "connectionString": "[concat('Name=', parameters('username'), ';Password=', parameters('password'))]"
}
```

W kolejnym przykładzie pokazano zmiennej, która jest typem złożonym JSON i zmienne, które są tworzone na podstawie innych zmiennych:

```json
"parameters": {
    "environmentName": {
        "type": "string",
        "allowedValues": [
          "test",
          "prod"
        ]
    }
},
"variables": {
    "environmentSettings": {
        "test": {
            "instancesSize": "Small",
            "instancesCount": 1
        },
        "prod": {
            "instancesSize": "Large",
            "instancesCount": 4
        }
    },
    "currentEnvironmentSettings": "[variables('environmentSettings')[parameters('environmentName')]]",
    "instancesSize": "[variables('currentEnvironmentSettings').instancesSize]",
    "instancesCount": "[variables('currentEnvironmentSettings').instancesCount]"
}
```

## <a name="resources"></a>Zasoby
W sekcji zasobów można zdefiniować zasoby, które są wdrożone lub aktualizowane. W tej sekcji można uzyskać skomplikowane, ponieważ należy zrozumieć typy, które jest wdrażany w celu zapewnienia właściwych wartości. Dla określonych zasobów wartości (apiVersion, typ i właściwości), które należy ustawić, zobacz [zdefiniować zasoby w szablonach usługi Azure Resource Manager](/azure/templates/). 

Można zdefiniować zasoby o następującej strukturze:

```json
"resources": [
  {
      "condition": "<boolean-value-whether-to-deploy>",
      "apiVersion": "<api-version-of-resource>",
      "type": "<resource-provider-namespace/resource-type-name>",
      "name": "<name-of-the-resource>",
      "location": "<location-of-resource>",
      "tags": {
          "<tag-name1>": "<tag-value1>",
          "<tag-name2>": "<tag-value2>"
      },
      "comments": "<your-reference-notes>",
      "copy": {
          "name": "<name-of-copy-loop>",
          "count": "<number-of-iterations>",
          "mode": "<serial-or-parallel>",
          "batchSize": "<number-to-deploy-serially>"
      },
      "dependsOn": [
          "<array-of-related-resource-names>"
      ],
      "properties": {
          "<settings-for-the-resource>",
          "copy": [
              {
                  "name": ,
                  "count": ,
                  "input": {}
              }
          ]
      },
      "resources": [
          "<array-of-child-resources>"
      ]
  }
]
```

| Nazwa elementu | Wymagane | Opis |
|:--- |:--- |:--- |
| Warunek | Nie | Wartość logiczna wskazująca, czy zasób jest wdrażany. |
| apiVersion |Tak |Wersja interfejsu API REST do użycia podczas tworzenia zasobu. |
| type |Tak |Typ zasobu. Ta wartość jest kombinacją przestrzeń nazw dostawcy zasobów i typu zasobu (takich jak **magazyn.Microsoft/kontamagazynu**). |
| name |Tak |Nazwa zasobu. Nazwa musi występować po zdefiniowane w RFC3986 ograniczenia składnika identyfikatora URI. Ponadto usług platformy Azure, które udostępniają poza strony zweryfikować nazwę, aby zapewnić, że nazwa zasobu nie jest próba podszywają się pod innego tożsamości. |
| location |Zmienia się |Obsługiwane lokalizacje geograficzne podane zasobu. Można wybrać dowolny z dostępnych lokalizacji, ale zazwyczaj warto wybrać, który znajduje się w pobliżu użytkowników. Zazwyczaj również dobrym rozwiązaniem jest umieszczenie zasoby, które współdziałają ze sobą w tym samym regionie. Większość typów zasobów wymaga lokalizacji, ale nie wymagają lokalizację niektórych typów (takich jak przypisanie roli). Zobacz [Ustaw lokalizację zasobów w szablonach usługi Azure Resource Manager](resource-manager-template-location.md). |
| tags |Nie |Tagi, które są skojarzone z zasobem. Zobacz [tagów zasobów w szablonach usługi Azure Resource Manager](resource-manager-template-tags.md). |
| Komentarze |Nie |Notatki za dokumentację zasobów w szablonie |
| Kopiuj |Nie |Jeśli wymagane jest więcej niż jedno wystąpienie, liczba zasobów do utworzenia. Domyślnym trybem jest równoległe. Określ tryb serial gdy nie ma wszystkich lub zasoby w celu wdrożenia w tym samym czasie. Aby uzyskać więcej informacji, zobacz [utworzyć wiele wystąpień zasobów usługi Azure Resource Manager](resource-group-create-multiple.md). |
| dependsOn |Nie |Zasoby, które należy wdrożyć przed wdrożeniem tego zasobu. Menedżer zasobów ocenia zależności między zasobami i wdraża je w odpowiedniej kolejności. Zasoby nie są zależne od siebie, są wdrożone równolegle. Wartość może być rozdzielaną przecinkami listą zasobu nazwy lub unikatowych identyfikatorów zasobów. Tylko listy zasobów, które są wdrażane w tym szablonie. Zasoby, które nie są zdefiniowane w tym szablonie musi już istnieć. Unikaj Dodawanie zależności niepotrzebne, jak mogą spowalniać wdrożenia i utworzyć zależności cykliczne. Aby uzyskać wskazówki dotyczące zależności ustawienia, zobacz [Definiowanie zależności w szablonach usługi Azure Resource Manager](resource-group-define-dependencies.md). |
| properties |Nie |Ustawienia konfiguracji określonych zasobów. Wartości właściwości są takie same jak wartości podane w treści żądania dla operacji interfejsu API REST (metody PUT) do utworzenia zasobu. Można również określić tablicy kopiowania, aby utworzyć wiele wystąpień właściwości. Aby uzyskać więcej informacji, zobacz [utworzyć wiele wystąpień zasobów usługi Azure Resource Manager](resource-group-create-multiple.md). |
| Zasoby |Nie |Zasoby podrzędne, które zależą od zasobu został określony. Podaj tylko typy zasobów, które są dozwolone w schemacie zasobu nadrzędnego. Pełny typ zasobu podrzędnych obejmuje nadrzędny typ zasobu, takich jak **Microsoft.Web/sites/extensions**. Zależność od zasobu nadrzędnego nie jest oznaczany. Jawnie zdefiniuj tej zależności. |

Sekcja zasobów zawiera zasoby w celu wdrożenia. W ramach każdego zasobu można również zdefiniować tablicę zasoby podrzędne. W związku z tym sekcji zasobów można mieć struktury, takiej jak:

```json
"resources": [
  {
      "name": "resourceA",
  },
  {
      "name": "resourceB",
      "resources": [
        {
            "name": "firstChildResourceB",
        },
        {   
            "name": "secondChildResourceB",
        }
      ]
  },
  {
      "name": "resourceC",
  }
]
```      

Aby uzyskać więcej informacji na temat definiowania zasoby podrzędne, zobacz [Ustaw nazwę i typ zasobu podrzędnego w szablonie usługi Resource Manager](resource-manager-template-child-resource.md).

**Warunku** element określa, czy zasób jest wdrażany. Wartość dla tego elementu jest rozpoznawany jako PRAWDA lub FAŁSZ. Na przykład aby określić, czy nowe konto magazynu jest wdrażany, należy użyć:

```json
{
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2017-06-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "[variables('storageAccountType')]"
    },
    "kind": "Storage",
    "properties": {}
}
```

Na przykład za pomocą nowego lub istniejącego zasobu, zobacz [nowy lub istniejący szablon warunku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResources.NewOrExisting.json).

Aby określić, czy maszyna wirtualna jest wdrażany za pomocą hasła lub klucza SSH, należy zdefiniować dwie wersje maszyny wirtualnej w szablonie i użyj **warunku** rozróżnianie użycia. Podaj parametr, który określa scenariusza wdrażania.

```json
{
    "condition": "[equals(parameters('passwordOrSshKey'),'password')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'password')]",
    "properties": {
        "osProfile": {
            "computerName": "[variables('vmName')]",
            "adminUsername": "[parameters('adminUsername')]",
            "adminPassword": "[parameters('adminPassword')]"
        },
        ...
    },
    ...
},
{
    "condition": "[equals(parameters('passwordOrSshKey'),'sshKey')]",
    "apiVersion": "2016-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('vmName'),'ssh')]",
    "properties": {
        "osProfile": {
            "linuxConfiguration": {
                "disablePasswordAuthentication": "true",
                "ssh": {
                    "publicKeys": [
                        {
                            "path": "[variables('sshKeyPath')]",
                            "keyData": "[parameters('adminSshKey')]"
                        }
                    ]
                }
            }
        },
        ...
    },
    ...
}
``` 

Przykład wdrożenia maszyny wirtualnej za pomocą hasła lub klucza SSH, zobacz [nazwa użytkownika lub SSH szablonu warunku](https://github.com/rjmax/Build2017/blob/master/Act1.TemplateEnhancements/Chapter05.ConditionalResourcesUsernameOrSsh.json).

## <a name="outputs"></a>dane wyjściowe
W sekcji danych wyjściowych można określić wartości, które są zwracane z wdrożenia. Na przykład można zwrócić identyfikator URI do uzyskania dostępu do zasobu wdrożone.

W poniższym przykładzie przedstawiono struktura definicji danych wyjściowych:

```json
"outputs": {
    "<outputName>" : {
        "type" : "<type-of-output-value>",
        "value": "<output-value-expression>"
    }
}
```

| Nazwa elementu | Wymagane | Opis |
|:--- |:--- |:--- |
| outputName |Tak |Nazwa wartości danych wyjściowych. Musi być prawidłowym identyfikatorem języka JavaScript. |
| type |Tak |Typ wartości danych wyjściowych. Dane wyjściowe wartości obsługuje te same typy tablic jako parametrów wejściowych szablonu. |
| wartość |Tak |Wyrażenia języka szablonu, który jest obliczany i zwracany, jako wartość wyjściowa. |

W poniższym przykładzie przedstawiono wartość, która jest zwracana w sekcji danych wyjściowych.

```json
"outputs": {
    "siteUri" : {
        "type" : "string",
        "value": "[concat('http://',reference(resourceId('Microsoft.Web/sites', parameters('siteName'))).hostNames[0])]"
    }
}
```

Aby uzyskać więcej informacji na temat pracy z danych wyjściowych, zobacz [udostępniania stanu w szablonach usługi Azure Resource Manager](best-practices-resource-manager-state.md).

## <a name="template-limits"></a>Limity szablonu

Limit rozmiaru szablon 1 MB, a każdy plik parametrów do 64 KB. Limit 1 MB jest stosowana do stanu końcowego szablonu zostanie rozwinięty definicje interakcyjnych zasobów i wartości zmiennych i parametrów. 

Możesz również są ograniczone do:

* Parametry 256
* zmienne 256
* 800 zasoby (w tym liczba kopii)
* 64 wartości danych wyjściowych
* 24 576 znaków w wyrażeniu szablonu

Niektóre limity szablonu może przekroczyć przy użyciu szablonu zagnieżdżonego. Aby uzyskać więcej informacji, zobacz [przy użyciu szablonów połączonych w przypadku wdrażania zasobów Azure](resource-group-linked-templates.md). Aby zmniejszyć liczbę parametrów, zmiennych lub dane wyjściowe, można połączyć kilka wartości do obiektu. Aby uzyskać więcej informacji, zobacz [obiektów jako parametry](resource-manager-objects-as-parameters.md).

## <a name="next-steps"></a>Następne kroki
* Aby wyświetlić pełną listę szablonów dla wielu różnych rozwiązań, zobacz [Szablony szybkiego startu platformy Azure](https://azure.microsoft.com/documentation/templates/).
* Aby uzyskać więcej informacji o funkcje, których można użyć z w ramach szablonu, zobacz [funkcje szablonów usługi Azure Resource Manager](resource-group-template-functions.md).
* Aby połączyć wiele szablonów podczas wdrażania, zobacz [za pomocą szablonów połączonych z usługą Azure Resource Manager](resource-group-linked-templates.md).
* Może być konieczne użycie zasobów, które istnieją w innej grupie zasobów. Ten scenariusz jest typowy podczas pracy z kontami magazynu lub sieci wirtualne, które są współdzielone przez wiele grup zasobów. Aby uzyskać więcej informacji, zobacz [funkcja resourceId](resource-group-template-functions-resource.md#resourceid).
