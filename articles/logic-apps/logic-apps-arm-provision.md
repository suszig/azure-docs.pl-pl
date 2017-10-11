---
title: "Tworzenie aplikacji logiki na platformie Azure przy użyciu szablonu | Dokumentacja firmy Microsoft"
description: "Wdrażanie aplikacji logiki do definiowania przepływów pracy za pomocą szablonu usługi Azure Resource Manager."
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7574cc7c-e5a1-4b7c-97f6-0cffb1a5d536
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/27/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 161adeacd6da2b15225c8a4ddae171e19e539967
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-logic-app-using-a-template"></a>Tworzenie aplikacji logiki przy użyciu szablonu
Szablony umożliwiają szybkie użyć różnych łączników w aplikacji logiki. Aplikacje logiki zawiera szablony usługi Azure Resource Manager umożliwia tworzenie aplikacji logiki, który może służyć do definiowania przepływów pracy firmowych. Można zdefiniować zasobów, do których są wdrażane, a także sposób definiowania parametrów podczas wdrażania aplikacji logiki. Możesz użyć tego szablonu dla scenariuszy biznesowych lub dostosować go do potrzeb.

Więcej szczegółów na właściwości aplikacji logiki, zobacz [API zarządzania przepływu pracy aplikacji logiki](https://msdn.microsoft.com/library/azure/mt643788.aspx). 

Aby uzyskać przykłady samą definicję, zobacz [definicjami aplikacji logiki autora](logic-apps-author-definitions.md). 

Aby uzyskać więcej informacji na temat tworzenia szablonów, zobacz [Authoring Azure Resource Manager szablony](../azure-resource-manager/resource-group-authoring-templates.md).

Zakończenie szablonu, zobacz [szablon aplikacji logiki](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## <a name="what-you-deploy"></a>Możesz wdrożyć
W przypadku tego szablonu wdrażania aplikacji logiki.

Aby automatycznie uruchamiać wdrożenie, przycisk:  

[![Wdrażanie na platformie Azure](media/logic-apps-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### <a name="testuri"></a>testUri
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/en-us/status/feed/"
      }

## <a name="resources-to-deploy"></a>Zasoby wymagające wdrożenia
### <a name="logic-app"></a>Aplikacji logiki
Tworzenie aplikacji logiki.

Szablony używa wartości parametru Nazwa aplikacji logiki. Ustawia lokalizację aplikacji logiki do tej samej lokalizacji co grupy zasobów. 

Tej konkretnej definicji jest uruchamiana raz na godzinę, a następnie wysyła pakiet usługi ping do lokalizacji określonej w **testUri** parametru. 

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## <a name="commands-to-run-deployment"></a>Polecenia umożliwiające uruchomienie wdrożenia
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### <a name="powershell"></a>PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup



