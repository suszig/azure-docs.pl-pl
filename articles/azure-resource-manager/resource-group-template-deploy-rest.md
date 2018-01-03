---
title: "Wdrażanie zasobów przy użyciu interfejsu API REST i szablon | Dokumentacja firmy Microsoft"
description: "Użyj Menedżera zasobów Azure i interfejsu REST API usługi Resource Manager wdrażania zasobów na platformie Azure. Zasoby są zdefiniowane w szablonie usługi Resource Manager."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 1d8fbd4c-78b0-425b-ba76-f2b7fd260b45
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/10/2017
ms.author: tomfitz
ms.openlocfilehash: 46856a25fb57bb2c5a3c1aeae13c11655e1a58a5
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-resource-manager-rest-api"></a>Deploy resources with Resource Manager templates and Resource Manager REST API (Wdrażanie zasobów za pomocą szablonów usługi Resource Manager i interfejsu API REST usługi Resource Manager)
> [!div class="op_single_selector"]
> * [Program PowerShell](resource-group-template-deploy.md)
> * [Interfejs wiersza polecenia platformy Azure](resource-group-template-deploy-cli.md)
> * [Portal](resource-group-template-deploy-portal.md)
> * [Interfejs API REST](resource-group-template-deploy-rest.md)
> 
> 

W tym artykule wyjaśniono, jak wdrażanie zasobów na platformie Azure za pomocą interfejsu REST API usługi Resource Manager z szablonami usługi Resource Manager.  

> [!TIP]
> Aby uzyskać pomoc dotyczącą debugowania — błąd podczas wdrażania zobacz:
> 
> * [Wyświetl operacje wdrażania](resource-manager-deployment-operations.md) Aby dowiedzieć się o wprowadzenie informacje ułatwiające rozwiązywania problemów dotyczących błędu
> * [Rozwiąż typowe błędy podczas wdrażania zasobów na platformie Azure za pomocą Menedżera zasobów Azure](resource-manager-common-deployment-errors.md) Aby dowiedzieć się, jak rozwiązać typowe błędy wdrażania
> 
> 

Szablon może być lokalny plik lub pliku zewnętrznego, który jest dostępny za pomocą identyfikatora URI. Gdy w szablonie znajduje się na koncie magazynu, można ograniczyć dostęp do szablonu, a także dostarczają token sygnatury dostępu Współdzielonego dostępu współdzielonego podczas wdrażania.

[!INCLUDE [resource-manager-deployments](../../includes/resource-manager-deployments.md)]

## <a name="deploy-with-the-rest-api"></a>Rozmieszczanie za pomocą interfejsu API REST
1. Ustaw [wspólnych parametrów i nagłówki](https://docs.microsoft.com/rest/api/index), łącznie z tokenami uwierzytelniania.
2. Jeśli nie masz istniejącej grupy zasobów, należy utworzyć grupę zasobów. Podaj identyfikator subskrypcji, nazwę nowej grupy zasobów i lokalizacji, która należy do rozwiązania. Aby uzyskać więcej informacji, zobacz [Utwórz grupę zasobów](https://docs.microsoft.com/rest/api/resources/resourcegroups#ResourceGroups_CreateOrUpdate).
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>?api-version=2015-01-01
          <common headers>
          {
            "location": "West US",
            "tags": {
               "tagname1": "tagvalue1"
            }
          }
3. Sprawdzanie poprawności wdrożenia przed wykonaniem ją, uruchamiając [weryfikowanie wdrożenia szablonu](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Validate) operacji. Podczas testowania wdrożenia, należy podać parametry, dokładnie tak jak w przypadku wykonywania wdrożenia (pokazano w następnym kroku).
4. Tworzenie wdrożenia. Podaj identyfikator subskrypcji, nazwę grupy zasobów, nazwę wdrożenia i łącza do szablonu. Aby uzyskać informacje o pliku szablonu, zobacz [pliku parametrów](#parameter-file). Aby uzyskać więcej informacji na temat interfejsu API REST, aby utworzyć grupę zasobów, zobacz [Utwórz wdrożenie szablonu](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_CreateOrUpdate). Powiadomienie **tryb** ustawiono **przyrostowe**. Uruchamiania ukończenia wdrożenia, należy ustawić **tryb** do **Complete**. Należy zachować ostrożność podczas korzystania z trybu pełne, jak może przypadkowo usunięte zasoby, które nie znajdują się w szablonie.
   
        PUT https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
          <common headers>
          {
            "properties": {
              "templateLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/template.json",
                "contentVersion": "1.0.0.0"
              },
              "mode": "Incremental",
              "parametersLink": {
                "uri": "http://mystorageaccount.blob.core.windows.net/templates/parameters.json",
                "contentVersion": "1.0.0.0"
              }
            }
          }
   
      Do rejestrowania zawartości odpowiedzi i żądania zawartości, należy włączyć **debugSetting** w żądaniu.
   
        "debugSetting": {
          "detailLevel": "requestContent, responseContent"
        }
   
      Aby użyć tokenu sygnatury dostępu Współdzielonego dostępu współdzielonego, można skonfigurować konta magazynu. Aby uzyskać więcej informacji, zobacz [Delegowanie dostępu z sygnaturą dostępu współdzielonego](https://docs.microsoft.com/rest/api/storageservices/delegating-access-with-a-shared-access-signature).
5. Pobieranie stanu wdrażania szablonu. Aby uzyskać więcej informacji, zobacz [uzyskać informacje na temat wdrażania szablonu](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get).
   
          GET https://management.azure.com/subscriptions/<YourSubscriptionId>/resourcegroups/<YourResourceGroupName>/providers/Microsoft.Resources/deployments/<YourDeploymentName>?api-version=2015-01-01
           <common headers>

## <a name="parameter-file"></a>Plik parametrów

[!INCLUDE [resource-manager-parameter-file](../../includes/resource-manager-parameter-file.md)]

## <a name="next-steps"></a>Kolejne kroki
* Informacje na temat obsługi operacji asynchronicznych REST, zobacz [śledzić operacje asynchroniczne Azure](resource-manager-async-operations.md).
* Na przykład wdrażania zasobów za pomocą biblioteki klienta .NET, zobacz [wdrażanie zasobów przy użyciu bibliotek .NET oraz szablonu](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Aby określić parametry w szablonie, zobacz [tworzenia szablonów](resource-group-authoring-templates.md#parameters).
* Aby uzyskać wskazówki dotyczące wdrażania rozwiązania w różnych środowiskach, zobacz [Development and test environments in Microsoft Azure](solution-dev-test-environments.md) (Środowiska projektowe i testowe na platformie Microsoft Azure).
* Aby uzyskać instrukcje dla przedsiębiorstw dotyczące użycia usługi Resource Manager w celu efektywnego zarządzania subskrypcjami, zobacz [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md) (Szkielet platformy Azure dla przedsiębiorstwa — narzucony nadzór subskrypcji).

