---
title: "Zasady zasobów platformy Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób upewnij się, że właściwości spójne zasobów są ustawiane podczas wdrażania przy użyciu zasad usługi Azure Resource Manager. Zasady można stosować na grupy zasobów lub subskrypcji."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: abde0f73-c0fe-4e6d-a1ee-32a6fce52a2d
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/09/2017
ms.author: tomfitz
ms.openlocfilehash: cfdbf35b76b6a7f3cddb2deb35dfc475e0fc600f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="resource-policy-overview"></a>Przegląd zasad zasobów
Zasady zasobów umożliwiają ustalenie Konwencji zasobów w organizacji. Definiowanie Konwencji, można kontrolować koszty i zarządzania zasobami. Na przykład można określić, czy dozwolone są tylko niektóre typy maszyn wirtualnych. Alternatywnie można wymagać, że wszystkie zasoby mają określony tag. Zasady są dziedziczone przez wszystkie zasoby podrzędne. Tak Jeśli zasady są stosowane do grupy zasobów, ma zastosowanie do wszystkich zasobów w danej grupie zasobów.

Istnieją dwa pojęć dotyczących zasad:

* Definicja zasad - opisano gdy zasady są wymuszane i jaka akcja ma być
* przypisanie zasad — w przypadku zastosowania definicji zasad do zakresu (subskrypcji lub grupy zasobów)

Ten temat koncentruje się na definicji zasad. Informacje o przypisywaniu zasad, zobacz [portal Azure używany do przypisywania i zarządzanie zasadami zasobów](resource-manager-policy-portal.md) lub [przypisania zasad i zarządzania nimi za pośrednictwem skryptu](resource-manager-policy-create-assign.md).

Zasady są oceniane podczas tworzenia i aktualizowania zasobów (PUT i poprawka operacji).

## <a name="how-is-it-different-from-rbac"></a>Czym się różni od RBAC?
Istnieje kilka podstawowych różnic między zasadami i kontroli dostępu opartej na rolach (RBAC). RBAC koncentruje się na **użytkownika** działań w innych zakresach. Na przykład możesz są dodawane do roli współautora dla grupy zasobów na żądany zakres, należy wprowadzić zmiany w tej grupie zasobów. Zasady koncentruje się na **zasobów** właściwości podczas wdrażania. Na przykład za pomocą zasad, można kontrolować typów zasobów, które mogą być udostępniane. Alternatywnie można ograniczyć lokalizacje, w których można udostępnić zasoby. W odróżnieniu od RBAC, zasady są domyślnie Zezwól jawne i odmawianie systemu. 

Aby użyć zasad, użytkownik musi zostać uwierzytelniony za pośrednictwem RBAC. W szczególności, Twoje konto musi:

* `Microsoft.Authorization/policydefinitions/write`uprawnienia do definiowania zasad
* `Microsoft.Authorization/policyassignments/write`uprawnienie do przypisywania zasad 

Te uprawnienia nie są uwzględnione w **współautora** roli.

## <a name="built-in-policies"></a>Wbudowane zasady

Platforma Azure udostępnia definicje zasad wbudowanych, których może zmniejszyć liczbę zasad, które należy zdefiniować. Przed kontynuowaniem definicje zasad, należy rozważyć, czy wbudowanych zasad już zawiera definicję, które są potrzebne. Wbudowane zasady definicje są:

* Dozwolonych lokalizacji
* Typy zasobów dozwolonych
* Dozwolone konto magazynu jednostki SKU
* Dozwolone jednostki SKU maszyny wirtualnej
* Zastosowanie tagu i domyślne wartości
* Wymuszanie tagu i wartości
* Niedozwolone typy zasobów
* Wymaga programu SQL Server w wersji 12.0
* Wymagaj szyfrowania konta magazynu

Można przypisać dowolną z tych zasad za pomocą [portal](resource-manager-policy-portal.md), [PowerShell](resource-manager-policy-create-assign.md#powershell), lub [interfejsu wiersza polecenia Azure](resource-manager-policy-create-assign.md#azure-cli).

## <a name="policy-definition-structure"></a>Struktura definicji zasad
JSON służy do tworzenia definicji zasad. Definicja zasad zawiera elementy dla:

* Tryb
* parameters
* Nazwa wyświetlana
* description
* Reguła zasad
  * Ocena logiczne
  * Efekt

W poniższym przykładzie przedstawiono zasady określające, gdzie są wdrażane zasoby:

```json
{
  "properties": {
    "mode": "all",
    "parameters": {
      "allowedLocations": {
        "type": "array",
        "metadata": {
          "description": "The list of locations that can be specified when deploying resources",
          "strongType": "location",
          "displayName": "Allowed locations"
        }
      }
    },
    "displayName": "Allowed locations",
    "description": "This policy enables you to restrict the locations your organization can specify when deploying resources.",
    "policyRule": {
      "if": {
        "not": {
          "field": "location",
          "in": "[parameters('allowedLocations')]"
        }
      },
      "then": {
        "effect": "deny"
      }
    }
  }
}
```

## <a name="mode"></a>Tryb

Firma Microsoft zaleca, aby ustawić `mode` do `all`. Jeśli zostanie ustawiona **wszystkie**, grupy zasobów i wszystkie typy zasobów są oceniane pod kątem zasad. Portal używa **wszystkie** dla wszystkich zasad. Jeśli używasz programu PowerShell lub interfejsu wiersza polecenia Azure, należy określić `mode` parametru i wartości **wszystkich**.
 
Wcześniej zasady został oceniony na tylko typy zasobów, które obsługiwane tagów i lokalizacji. `indexed` Tryb nadal to zachowanie. Jeśli używasz **wszystkie** tryb, również są analizowane zasady na typy zasobów, które nie obsługują tagów i lokalizacji. [Podsieć sieci wirtualnej](https://github.com/Azure/azure-policy-samples/tree/master/samples/Network/enforce-nsg-on-subnet) jest przykładem nowo dodanego typu. Ponadto są oceniane grup zasobów, gdy tryb jest ustawiony na **wszystkich**. Można na przykład [wymusić tagów w grupie zasobów](https://github.com/Azure/azure-policy-samples/tree/master/samples/ResourceGroup/enforce-resourceGroup-tags). 

## <a name="parameters"></a>Parametry
Przy użyciu parametrów upraszcza zarządzanie zasadami dzięki zmniejszeniu liczby definicje zasad. Zdefiniuj zasady dla właściwości zasobów (na przykład ograniczenie lokalizacji wdrożonym zasobów) i obejmują parametry w definicji. Następnie ponowne użycie tej definicji zasad dla różnych scenariuszy przez przekazywanie różne wartości (na przykład określenie jeden zestaw lokalizacji dla subskrypcji) podczas przypisywania zasad.

Deklarowanie parametrów, podczas tworzenia definicji zasad.

```json
"parameters": {
  "allowedLocations": {
    "type": "array",
    "metadata": {
      "description": "The list of allowed locations for resources.",
      "displayName": "Allowed locations"
    }
  }
}
```

Typ parametru może być ciągiem lub tablicą. Właściwość metadanych służy do narzędzi, takich jak portalu Azure do wyświetlania informacji przyjazną dla użytkownika. 

W regule zasad możesz odwołania do parametrów przy użyciu następującej składni: 

```json
{ 
    "field": "location",
    "in": "[parameters('allowedLocations')]"
}
```

## <a name="display-name-and-description"></a>Nazwę wyświetlaną i opis

Możesz użyć **displayName** i **opis** do identyfikowania definicji zasad i podanie gdy jest używana w kontekście.

## <a name="policy-rule"></a>Reguła zasad

Reguły składa się z **Jeśli** i **następnie** bloków. W **Jeśli** bloku, należy zdefiniować co najmniej jeden warunek, które określają, gdy zasady są wymuszane. Operatory logiczne można zastosować do tych warunków, aby precyzyjnie zdefiniować scenariusz dla zasad. W **następnie** bloku, zdefiniuj efekt, która jest wywoływana po **Jeśli** warunki są spełnione.

```json
{
  "if": {
    <condition> | <logical operator>
  },
  "then": {
    "effect": "deny | audit | append"
  }
}
```

### <a name="logical-operators"></a>Operatory logiczne
Operatory logiczne obsługiwane są:

* `"not": {condition  or operator}`
* `"allOf": [{condition or operator},{condition or operator}]`
* `"anyOf": [{condition or operator},{condition or operator}]`

**Nie** składni odwraca wynik w warunku. **Wszystkie** składni (podobny do logicznego **i** operacji) wymaga wszystkie warunki, które muszą być spełnione. **AnyOf** składni (podobny do logicznego **lub** operacji) wymaga co najmniej jeden warunków.

Operatory logiczne można zagnieżdżać. W poniższym przykładzie przedstawiono **nie** operacja, która jest zagnieżdżona w **wszystkie** operacji. 

```json
"if": {
  "allOf": [
    {
      "not": {
        "field": "tags",
        "containsKey": "application"
      }
    },
    {
      "field": "type",
      "equals": "Microsoft.Storage/storageAccounts"
    }
  ]
},
```

### <a name="conditions"></a>Warunki
Warunek jest czy **pola** spełnia określone kryteria. Są obsługiwane warunki:

* `"equals": "value"`
* `"like": "value"`
* `"match": "value"`
* `"contains": "value"`
* `"in": ["value1","value2"]`
* `"containsKey": "keyName"`
* `"exists": "bool"`

Korzystając z **jak** warunku, możesz podać symbol wieloznaczny (*) w wartości.

Korzystając z **odpowiada** warunku, podaj `#` do reprezentowania cyfrę, `?` literę i znak reprezentują rzeczywiste znaku. Aby uzyskać przykłady, zobacz [stosowania zasad zasobów dla nazwy i tekst](resource-manager-policy-naming-convention.md).

### <a name="fields"></a>Pola
Warunki są utworzone za pomocą pola. Pole reprezentuje właściwości w ładunku żądania zasobu, który jest używany do opisu stanu typu zasobu.  

Obsługiwane są następujące pola:

* `name`
* `kind`
* `type`
* `location`
* `tags`
* `tags.*` 
* Aliasy właściwości — Aby uzyskać listę, zobacz [aliasy](#aliases).

### <a name="effect"></a>Efekt
Zasady obsługuje trzy rodzaje efekt — `deny`, `audit`, `append`, `AuditIfNotExists`, i `DeployIfNotExists`. 

* **Odmów** generuje zdarzenie w dzienniku inspekcji i niepowodzenia żądania
* **Inspekcja** generuje to zdarzenie ostrzegawcze w dzienniku inspekcji, ale nie wystąpi niepowodzenie żądania
* **Dołącz** dodaje zestaw określonych pól na żądanie 
* **AuditIfNotExists** -włączyć inspekcję, jeśli zasób nie istnieje.
* **DeployIfNotExists** — wdrażanie zasobu, jeśli jeszcze nie istnieje. Obecnie ten efekt jest obsługiwana tylko przy użyciu wbudowanych zasad.

Aby uzyskać **Dołącz**, należy podać następujące informacje:

```json
"effect": "append",
"details": [
  {
    "field": "field name",
    "value": "value of the field"
  }
]
```

Wartość może być ciągiem lub obiekt do formatu JSON. 

Z **AuditIfNotExists** i **DeployIfNotExists**, ocenę istnienia zasobu podrzędnego i stosowanie reguły, jeśli ten zasób nie istnieje. Można na przykład wymagać, że dla wszystkich sieci wirtualnych jest wdrażany obserwatora sieciowego.

Przykład inspekcję, gdy rozszerzenie maszyny wirtualnej nie została wdrożona, zobacz [rozszerzeń maszyny Wirtualnej inspekcji](https://github.com/Azure/azure-policy-samples/blob/master/samples/Compute/audit-vm-extension/azurepolicy.json).

## <a name="aliases"></a>Aliasy

Aliasy właściwości umożliwia dostęp do właściwości specyficzne dla typu zasobu. Aliasy pozwalają ograniczyć, jakie wartości lub warunki są dozwolone dla właściwości w zasobie. Każdy alias mapuje ścieżek w różnych wersjach interfejsu API dla typu zasobu. Podczas oceny zasad aparat zasad pobiera ścieżki właściwości dla tej wersji interfejsu API.

**Microsoft.Cache/Redis**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Cache/Redis/enableNonSslPort | Ustaw czy bez użycia protokołu ssl serwera Redis portu (6379) jest włączona. |
| Microsoft.Cache/Redis/shardCount | Ustaw liczbę fragmentów do utworzenia na pamięć podręczna klastra Premium.  |
| Microsoft.Cache/Redis/sku.capacity | Ustaw rozmiar pamięci podręcznej Redis do wdrożenia.  |
| Microsoft.Cache/Redis/sku.family | Ustaw rodziny SKU do użycia. |
| Microsoft.Cache/Redis/sku.name | Ustaw typ pamięci podręcznej Redis do wdrożenia. |

**Microsoft.Cdn/profiles**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.CDN/profiles/sku.name | Ustaw nazwę warstwy cenowej. |

**Microsoft.Compute/disks**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Compute/imageOffer | Ustaw oferta obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imagePublisher | Ustaw wydawcy obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imageSku | Jednostka SKU obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej zestawu. |
| Microsoft.Compute/imageVersion | Ustaw wersję obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |


**Microsoft.Compute/virtualMachines**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Compute/imageId | Ustaw identyfikator obraz używany do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imageOffer | Ustaw oferta obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imagePublisher | Ustaw wydawcy obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imageSku | Jednostka SKU obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej zestawu. |
| Microsoft.Compute/imageVersion | Ustaw wersję obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/licenseType | Ustaw, że obraz lub dysk jest licencjonowane lokalnymi. Ta wartość jest używana tylko dla obrazów systemu operacyjnego Windows Server.  |
| Microsoft.Compute/virtualMachines/imageOffer | Ustaw oferta obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/virtualMachines/imagePublisher | Ustaw wydawcy obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/virtualMachines/imageSku | Jednostka SKU obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej zestawu. |
| Microsoft.Compute/virtualMachines/imageVersion | Ustaw wersję obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/virtualMachines/osDisk.Uri | Ustaw identyfikator URI dysku vhd. |
| Microsoft.Compute/virtualMachines/sku.name | Ustaw rozmiar maszyny wirtualnej. |

**Microsoft.Compute/virtualMachines/extensions**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Compute/virtualMachines/extensions/publisher | Ustaw nazwę wydawcy rozszerzenia. |
| Microsoft.Compute/virtualMachines/extensions/type | Ustaw typ rozszerzenia. |
| Microsoft.Compute/virtualMachines/extensions/typeHandlerVersion | Ustaw wersję rozszerzenia. |

**Microsoft.Compute/virtualMachineScaleSets**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Compute/imageId | Ustaw identyfikator obraz używany do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imageOffer | Ustaw oferta obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imagePublisher | Ustaw wydawcy obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/imageSku | Jednostka SKU obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej zestawu. |
| Microsoft.Compute/imageVersion | Ustaw wersję obrazu platformy lub obrazu z witryny marketplace użyty do utworzenia maszyny wirtualnej. |
| Microsoft.Compute/licenseType | Ustaw, że obraz lub dysk jest licencjonowane lokalnymi. Ta wartość jest używana tylko dla obrazów systemu operacyjnego Windows Server. |
| Microsoft.Compute/VirtualMachineScaleSets/computerNamePrefix | Ustaw prefiks nazwy komputera dla wszystkich maszyn wirtualnych w zestawie skalowania. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl | Ustaw identyfikator URI obiektu blob obrazu użytkownika. |
| Microsoft.Compute/VirtualMachineScaleSets/osdisk.vhdContainers | Ustawianie adresów URL kontenera, które są używane do przechowywania dysków systemu operacyjnego dla zestawu skalowania. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.name | Ustaw rozmiar maszyn wirtualnych w zestawie skalowania. |
| Microsoft.Compute/VirtualMachineScaleSets/sku.tier | Ustawienie warstwy maszyn wirtualnych w zestawie skalowania. |
  
**Microsoft.Network/applicationGateways**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Network/applicationGateways/sku.name | Ustawianie rozmiaru bramy. |

**Microsoft.Network/virtualNetworkGateways**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Network/virtualNetworkGateways/gatewayType | Ustaw typ tej bramy sieci wirtualnej. |
| Microsoft.Network/virtualNetworkGateways/sku.name | Ustaw nazwę jednostki SKU bramy. |

**Microsoft.Sql/servers**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Sql/servers/version | Ustaw wersję serwera. |

**Microsoft.Sql/databases**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Sql/servers/databases/edition | Ustaw wersję bazy danych. |
| Microsoft.Sql/servers/databases/elasticPoolName | Zestaw nazwę puli elastycznej bazy danych jest. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveId | Ustawienia poziomu identyfikatorów celu skonfigurowanej usługi bazy danych. |
| Microsoft.Sql/servers/databases/requestedServiceObjectiveName | Ustaw nazwę celu poziomu usługi skonfigurowane bazy danych.  |

**Microsoft.Sql/elasticpools**

| Alias | Opis |
| ----- | ----------- |
| serwery/elasticpools | Microsoft.Sql/servers/elasticPools/dtu | Wartość całkowita udostępnionych jednostek DTU dla puli elastycznej bazy danych. |
| serwery/elasticpools | Microsoft.Sql/servers/elasticPools/edition | Ustaw edition puli elastycznej. |

**Microsoft.Storage/storageAccounts**

| Alias | Opis |
| ----- | ----------- |
| Microsoft.Storage/storageAccounts/accessTier | Ustawianie warstwy dostępu używany na potrzeby rozliczeń. |
| Microsoft.Storage/storageAccounts/accountType | Nazwa jednostki SKU zestawu. |
| Microsoft.Storage/storageAccounts/enableBlobEncryption | Określ, czy usługa szyfruje dane, jak są przechowywane w usłudze magazyn obiektów blob. |
| Microsoft.Storage/storageAccounts/enableFileEncryption | Określ, czy usługa szyfruje dane, jak są przechowywane w usłudze magazyn plików. |
| Microsoft.Storage/storageAccounts/sku.name | Nazwa jednostki SKU zestawu. |
| Microsoft.Storage/storageAccounts/supportsHttpsTrafficOnly | Ustaw, aby zezwolić tylko na ruch protokołu https do usługi Magazyn. |

## <a name="policy-sets"></a>Ustawia zasady

Zestawy zasady umożliwiają grupowanie kilku definicje pokrewnych zasad. Zestaw zasad upraszcza przypisania i zarządzania, ponieważ możesz pracować z grupy jako pojedynczy element. Na przykład można grupować wszystkie powiązane zasady znakowania w zestawie w ramach jednych zasad. Zamiast przypisywać każdej zasady pojedynczo, należy zastosować zestaw zasad.
 
Poniższy przykład przedstawia sposób tworzenia zasad, ustaw obsługi dwa tagi (costCenter i productName). Używa dwóch wbudowane zasady stosowania domyślną wartość tagu i wymuszanie wartość tagu. Zestaw zasad deklaruje dwa parametry costCenterValue i productNameValue do ponownego użycia. Odwołuje się dwie definicje zasad wbudowane w wiele razy z innymi parametrami. Dla każdego parametru albo zapewnia stałą wartość, jak pokazano na tagName lub ustawić parametr z zasad, jak pokazano na tagValue.

```json
{
    "properties": {
        "displayName": "Billing Tags Policy",
        "policyType": "Custom",
        "description": "Specify cost Center tag and product name tag",
        "parameters": {
            "costCenterValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for Cost Center tag"
                }
            },
            "productNameValue": {
                "type": "String",
                "metadata": {
                    "description": "required value for product Name tag"
                }
            }
        },
        "policyDefinitions": [
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "costCenter"
                    },
                    "tagValue": {
                        "value": "[parameters('costCenterValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            },
            {
                "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/2a0e14a6-b0a6-4fab-991a-187a4f81c498",
                "parameters": {
                    "tagName": {
                        "value": "productName"
                    },
                    "tagValue": {
                        "value": "[parameters('productNameValue')]"
                    }
                }
            }
        ]
    },
    "id": "/subscriptions/<subscription-id>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicy",
    "type": "Microsoft.Authorization/policySetDefinitions",
    "name": "billingTagsPolicy"
}
```

Dodaj zasady ustawiony za pomocą **AzureRMPolicySetDefinition nowy** polecenia programu PowerShell.

Dla operacji REST, należy użyć **2017-06-01-preview** wersja interfejsu API, jak pokazano w poniższym przykładzie:

```
PUT /subscriptions/<subId>/providers/Microsoft.Authorization/policySetDefinitions/billingTagsPolicySet?api-version=2017-06-01-preview
```

## <a name="next-steps"></a>Następne kroki
* Po zdefiniowaniu reguły zasad, należy przypisać do zakresu. Aby przypisać zasady za pośrednictwem portalu, zobacz [portal Azure używany do przypisywania i zarządzanie zasadami zasobów](resource-manager-policy-portal.md). Aby przypisać zasady za pomocą interfejsu API REST, programu PowerShell lub interfejsu wiersza polecenia Azure, zobacz [przypisania zasad i zarządzania nimi za pośrednictwem skryptu](resource-manager-policy-create-assign.md).
* Na przykład zasady, zobacz [repozytorium GitHub zasad zasobów platformy Azure](https://github.com/Azure/azure-policy-samples).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).
* Schemat zasad jest opublikowana w [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

