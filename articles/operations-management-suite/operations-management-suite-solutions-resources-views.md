---
title: "Widoki w rozwiązaniach do zarządzania Operations Management Suite (OMS) | Dokumentacja firmy Microsoft"
description: "Zwykle zawiera co najmniej jeden widok do wizualizacji danych rozwiązania do zarządzania w Operations Management Suite (OMS).  W tym artykule opisano sposób eksportowania widoku utworzone przez projektanta widoku i dołączyć go w rozwiązaniu do zarządzania. "
services: operations-management-suite
documentationcenter: 
author: bwren
manager: jwhit
editor: tysonn
ms.assetid: 570b278c-2d47-4e5a-9828-7f01f31ddf8c
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/11/2017
ms.author: bwren
ms.openlocfilehash: 533b5564a805e0b41f2b1a4ad92e12b133220952
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="views-in-operations-management-suite-oms-management-solutions-preview"></a>Widoki w rozwiązaniach do zarządzania Operations Management Suite (OMS) (wersja zapoznawcza)
> [!NOTE]
> To jest wstępna dokumentacji do tworzenia rozwiązań do zarządzania w OMS, które są obecnie w wersji zapoznawczej. Żadnego schematu opisanych poniżej może ulec zmianie.    
>
>

[Rozwiązania do zarządzania w Operations Management Suite (OMS)](operations-management-suite-solutions.md) zwykle zawiera co najmniej jeden widok do wizualizacji danych.  W tym artykule opisano sposób eksportowania Widok utworzony przez [Widok projektanta](../log-analytics/log-analytics-view-designer.md) i uwzględnić go w rozwiązaniu do zarządzania.  

> [!NOTE]
> Przykłady w tym artykule, użyj parametrów i zmiennych, które są wymagane ani wspólne dla rozwiązań do zarządzania i opisano w [tworzenia rozwiązań do zarządzania w Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)
>
>

## <a name="prerequisites"></a>Wymagania wstępne
W tym artykule przyjęto założenie, że znasz już jak [tworzenie rozwiązania do zarządzania](operations-management-suite-solutions-creating.md) i struktura pliku rozwiązania.

## <a name="overview"></a>Omówienie
Aby uwzględnić widoku w rozwiązaniu do zarządzania, należy utworzyć **zasobów** dla niego w [plik rozwiązania](operations-management-suite-solutions-creating.md).  Dane JSON, które opisano konfigurację szczegółowy widok jest zwykle złożone jednak i nie coś autora rozwiązania typowych będzie można ręcznie utworzyć.  Najczęściej spotykaną metodą jest utworzenie przy użyciu widoku [Widok projektanta](../log-analytics/log-analytics-view-designer.md), go wyeksportować, a następnie dodaj konfigurację szczegółowe do rozwiązania.

Poniżej znajdują się podstawowe kroki, aby dodać widok do rozwiązania.  Każdy krok jest opisany bardziej szczegółowo w poniższych sekcjach.

1. Widok można wyeksportować do pliku.
2. Utwórz zasób widoku w rozwiązaniu.
3. Dodawanie szczegółów widoku.

## <a name="export-the-view-to-a-file"></a>Wyeksportuj do pliku widoku
Postępuj zgodnie z instrukcjami w [Projektant widoków analizy dziennika](../log-analytics/log-analytics-view-designer.md) można wyeksportować do pliku widoku.  Wyeksportowany plik zostanie w formacie JSON o takim samym [elementów jako plik rozwiązania](operations-management-suite-solutions-solution-file.md).  

**Zasobów** element pliku widoku będzie miał zasobu o typie **Microsoft.OperationalInsights/workspaces** który reprezentuje obszar roboczy OMS.  Ten element będzie mieć podelement typu **widoków** który reprezentuje widok i zawiera szczegółowe konfiguracji.  Zostanie Kopiuj szczegóły tego elementu, a następnie skopiować go do rozwiązania.

## <a name="create-the-view-resource-in-the-solution"></a>Utwórz zasób widoku w rozwiązaniu
Dodaj następujący zasób widoku do **zasobów** element pliku rozwiązania.  Używa zmiennych, które są opisane poniżej, że należy również dodać.  Należy pamiętać, że **pulpitu nawigacyjnego** i **OverviewTile** właściwości symboli zastępczych, które spowoduje zastąpienie odpowiednie właściwości z widoku eksportowanego pliku.

    {
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
        "type": "Microsoft.OperationalInsights/workspaces/views",
        "location": "[parameters('workspaceregionId')]",
        "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
        "dependson": [
            ],
        "properties": {
            "Id": "[variables('ViewName')]",
            "Name": "[variables('ViewName')]",
            "DisplayName": "[variables('ViewName')]",
            "Description": "",
            "Author": "[variables('ViewAuthor')]",
            "Source": "Local",
            "Dashboard": ,
            "OverviewTile":
        }
    }

Dodaj następujące zmienne do elementu zmienne pliku rozwiązania i Zastąp wartości jak w przypadku rozwiązania.

    "LogAnalyticsApiVersion": "2015-11-01-preview",
    "ViewAuthor": "Your name."
    "ViewDescription": "Optional description of the view."
    "ViewName": "Provide a name for the view here."


Należy pamiętać, że można skopiować cały widok zasobów z pliku wyeksportowanego widoku, ale musisz wprowadzić następujące zmiany, aby działał w rozwiązaniu.  

* **Typu** widoku zasobu musi zostać zmienione z **widoków** do **Microsoft.OperationalInsights/workspaces**.
* **Nazwa** właściwości dla zasobu widoku musi zostać zmienione, aby zawierał on nazwę obszaru roboczego.
* Zależność od obszaru roboczego musi można usunąć, ponieważ zasób obszaru roboczego nie jest zdefiniowany w rozwiązaniu.
* **Nazwa wyświetlana** właściwość musi zostać dodany do widoku.  **Identyfikator**, **nazwa**, i **DisplayName** musi wszystkie zgodne.
* Nazwy parametrów, musisz zmienić odpowiadające wymagany zestaw parametrów.
* Zmienne należy określić w rozwiązaniu i używane w odpowiednie właściwości.

## <a name="add-the-view-details"></a>Dodawanie widoku szczegółów
Przeglądanie zasobu w widoku wyeksportowanego pliku będzie zawierać dwa elementy w **właściwości** elementu o nazwie **pulpitu nawigacyjnego** i **OverviewTile** zawierających szczegółowe konfiguracji widoku.  Skopiuj następujące dwa elementy i ich zawartość do **właściwości** elementu zasobu widoku w pliku rozwiązania.

## <a name="example"></a>Przykład
Na przykład następujący przykład przedstawia plik prostym rozwiązaniem z widoku.  Wielokropek (...) są wyświetlane dla **pulpitu nawigacyjnego** i **OverviewTile** zawartość ze względów miejsca.

    {
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspaceName": {
                "type": "string"
            },
            "accountName": {
                "type": "string"
            },
            "workspaceRegionId": {
                "type": "string"
            },
            "regionId": {
                "type": "string"
            },
            "pricingTier": {
                "type": "string"
            }
        },
        "variables": {
            "SolutionVersion": "1.1",
            "SolutionPublisher": "Contoso",
            "SolutionName": "Contoso Solution",
            "LogAnalyticsApiVersion": "2015-11-01-preview",
            "ViewAuthor":  "user@contoso.com",
            "ViewDescription":  "This is a sample view.",
            "ViewName":  "Contoso View"
        },
        "resources": [
            {
                "name": "[concat(variables('SolutionName'), '(' ,parameters('workspacename'), ')')]",
                "location": "[parameters('workspaceRegionId')]",
                "tags": { },
                "type": "Microsoft.OperationsManagement/solutions",
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspacename'), '/views/', variables('ViewName'))]"
                ],
                "properties": {
                    "workspaceResourceId": "[concat(resourceGroup().id, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspacename'))]",
                    "referencedResources": [
                    ],
                    "containedResources": [
                        "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/views/', variables('ViewName'))]"
                    ]
                },
                "plan": {
                    "name": "[concat(variables('SolutionName'), '(' ,parameters('workspaceName'), ')')]",
                    "Version": "[variables('SolutionVersion')]",
                    "product": "ContosoSolution",
                    "publisher": "[variables('SolutionPublisher')]",
                    "promotionCode": ""
                }
            },
            {
                "apiVersion": "[variables('LogAnalyticsApiVersion')]",
                "name": "[concat(parameters('workspaceName'), '/', variables('ViewName'))]",
                "type": "Microsoft.OperationalInsights/workspaces/views",
                "location": "[parameters('workspaceregionId')]",
                "id": "[Concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'),'/views/', variables('ViewName'))]",
                "dependson": [
                ],
                "properties": {
                    "Id": "[variables('ViewName')]",
                    "Name": "[variables('ViewName')]",
                    "DisplayName": "[variables('ViewName')]",
                    "Description": "[variables('ViewDescription')]",
                    "Author": "[variables('ViewAuthor')]",
                    "Source": "Local",
                    "Dashboard": ...,
                    "OverviewTile": ...
                }
            }
          ]
    }




## <a name="next-steps"></a>Następne kroki
* Dowiedz się, szczegółowe informacje dotyczące tworzenia [rozwiązań do zarządzania](operations-management-suite-solutions-creating.md).
* Obejmują [elementu runbook usługi Automatyzacja w ramach rozwiązania do zarządzania](operations-management-suite-solutions-resources-automation.md).
