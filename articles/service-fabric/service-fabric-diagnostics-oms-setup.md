---
title: "Azure Service Fabric — Konfigurowanie monitorowania z OMS Log Analytics | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak skonfigurować OMS do wizualizacji i analizowania zdarzeń monitorowania klastrów sieci szkieletowej usług Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/31/2017
ms.author: dekapur
ms.openlocfilehash: 61182668b2677f19edbb736505d4892150890fed
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/17/2018
---
# <a name="set-up-oms-log-analytics-for-a-cluster"></a>Konfigurowanie OMS analizy dzienników dla klastra

Obszar roboczy OMS za pośrednictwem usługi Azure Resource Manager lub z portalu Azure Marketplace można skonfigurować. Użyj pierwszej, jeśli chcesz zachować szablonu wdrożenia do użycia w przyszłości. Wdrażanie za pośrednictwem portalu Marketplace jest łatwiejsze, jeśli masz już wdrożone z włączoną diagnostykę klastra.

> [!NOTE]
> Musisz mieć diagnostykę włączone dla klastra, aby wyświetlić klastra / zdarzenia na poziomie platformy aby można było pomyślnie skonfigurować OMS do monitorowania klastra.

## <a name="deploying-oms-using-a-resource-manager-template"></a>Wdrażanie pakietu OMS przy użyciu szablonu usługi Resource Manager

W przypadku wdrażania klastra przy użyciu szablonu usługi Resource Manager, szablon, należy utworzyć nowy obszar roboczy OMS Dodaj rozwiązania sieci szkieletowej usługi do niego i skonfiguruj go można odczytać danych z tabel do przechowywania odpowiednie.

[W tym miejscu](https://azure.microsoft.com/resources/templates/service-fabric-oms/) jest przykładowy szablon, który można używać i modyfikować zgodnie z wymaganiami. Więcej szablonów, które zapewniają różne opcje dla Konfigurowanie obszarem roboczym pakietu OMS można znaleźć w [szablonów usługi Service Fabric i OMS](https://azure.microsoft.com/resources/templates/?term=service+fabric+OMS).

Główne zmiany są następujące:

1. Dodaj `omsWorkspaceName` i `omsRegion` do parametry. Oznacza to, dodając następujący fragment kodu do parametrów zdefiniowanych w Twojej *template.json* pliku. Możesz zmodyfikować wartości domyślne, zgodnie z własnymi potrzebami. Należy również dodać dwa nowe parametry w Twojej *parameters.JSON następującym kodem* do definiowania ich wartości dla wdrażania zasobów:
    
    ```json
    "omsWorkspacename": {
        "type": "string",
        "defaultValue": "sfomsworkspace",
        "metadata": {
            "description": "Name of your OMS Log Analytics Workspace"
        }
    },
    "omsRegion": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "West Europe",
            "East US",
            "Southeast Asia"
        ],
        "metadata": {
            "description": "Specify the Azure Region for your OMS workspace"
        }
    }
    ```

    `omsRegion` Wartości muszą być zgodne z określonego zestawu wartości. Należy wybrać jedną, która jest najbardziej zbliżony do wdrożenia klastra.

2. Jeśli wszystkie dzienniki aplikacji będą wysyłane do OMS, upewnij się, że `applicationDiagnosticsStorageAccountType` i `applicationDiagnosticsStorageAccountName` są dołączone jako parametry w szablonie. Jeśli nie, dodaj je do sekcji variables tak jak i Edytuj ich wartości zgodnie z potrzebami. Możesz również uwzględnić je jako parametry, jeśli chcesz, zgodny z formatem użyta powyżej.

    ```json
    "applicationDiagnosticsStorageAccountType": "Standard_LRS",
    "applicationDiagnosticsStorageAccountName": "[toLower(concat('oms', uniqueString(resourceGroup().id), '3' ))]"
    ```

3. Dodaj rozwiązanie usługi sieć szkieletowa OMS do zmiennych do szablonu:

    ```json
    "solution": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "solutionName": "ServiceFabric"
    ```

4. Dodanie poniższego na końcu sekcji zasobów, po którym jest zadeklarowany jako zasób klastra usługi sieć szkieletowa usług.

    ```json
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[parameters('omsWorkspacename')]",
        "type": "Microsoft.OperationalInsights/workspaces",
        "properties": {
            "sku": {
                "name": "Free"
            }
        },
        "resources": [
            {
                "apiVersion": "2015-11-01-preview",
                "name": "[concat(variables('applicationDiagnosticsStorageAccountName'),parameters('omsWorkspacename'))]",
                "type": "storageinsightconfigs",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]",
                    "[concat('Microsoft.Storage/storageAccounts/', variables('applicationDiagnosticsStorageAccountName'))]"
                ],
                "properties": {
                    "containers": [ ],
                    "tables": [
                        "WADServiceFabric*EventTable",
                        "WADWindowsEventLogsTable",
                        "WADETWEventTable"
                    ],
                    "storageAccount": {
                        "id": "[resourceId('Microsoft.Storage/storageaccounts/', variables('applicationDiagnosticsStorageAccountName'))]",
                        "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('applicationDiagnosticsStorageAccountName')),'2015-06-15').key1]"
                    }
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('solution')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "id": "[resourceId('Microsoft.OperationsManagement/solutions/', variables('solution'))]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('solution')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('solutionName'))]",
            "promotionCode": ""
        }
    }
    ```

5. Wdrażanie szablonu uaktualnienie Menedżera zasobów do klastra. Jest to realizowane przy użyciu `New-AzureRmResourceGroupDeployment` interfejsu API w module środowiska AzureRM PowerShell. Należy na przykład polecenie:

    ```powershell
    New-AzureRmResourceGroupDeployment -ResourceGroupName "sfcluster1" -TemplateFile "<path>\template.json" -TemplateParameterFile "<path>\parameters.json"
    ``` 

    Usługa Azure Resource Manager będzie mógł wykryć, że jest to aktualizacja do istniejącego zasobu. Będzie przetwarzać tylko zmiany między szablonu zwiększają istniejącego wdrożenia i nowy szablon, pod warunkiem.

## <a name="deploying-oms-using-azure-marketplace"></a>Wdrażanie pakietu OMS przy użyciu portalu Azure Marketplace

Jeśli wolisz Dodaj obszar roboczy OMS po wdrożeniu klastra, przejdź do portalu Azure Marketplace, (w portalu) i poszukaj *"Analytics sieci szkieletowej usług"*.

1. Polecenie **nowy** w menu nawigacji po lewej stronie. 

2. Wyszukaj *usługi Analytics sieci szkieletowej*. Kliknij zasób, który jest wyświetlany.

3. Polecenie **tworzenie**

    ![OMS rz Analytics w portalu Marketplace](media/service-fabric-diagnostics-event-analysis-oms/service-fabric-analytics.png)

4. W oknie tworzenia usługi sieć szkieletowa Analytics kliknij **wybierz obszar roboczy** dla *obszarem roboczym pakietu OMS* pola, a następnie **Utwórz nowy obszar roboczy**. Wypełnij odpowiednie wpisy — jedynym wymaganiem jest subskrypcji dla klastra sieci szkieletowej usług i obszarem roboczym pakietu OMS musi być taka sama. Po sprawdzeniu poprawności wpisy obszar roboczy OMS zostanie uruchomione w celu wdrożenia. Ten trwa tylko kilka minut.

5. Gdy skończysz, kliknij przycisk **Utwórz** ponownie w dolnej części okna Tworzenie usługi Analytics sieci szkieletowej. Upewnij się, że nowy obszar roboczy zostaną wyświetlone w obszarze *obszarem roboczym pakietu OMS*. Spowoduje to dodanie rozwiązania do obszaru roboczego, który został utworzony.

6. Obszar roboczy nadal musi być połączona z danych diagnostycznych z klastra. Przejdź do utworzenia rozwiązania analizy sieci szkieletowej usług w grupy zasobów. Powinny pojawić się *ServiceFabric (\<nameOfOMSWorkspace\>)*. Kliknij rozwiązanie, aby przejść do strony Przegląd, z którym można zmienić ustawienia rozwiązania, ustawienia obszaru roboczego i przejdź do portalu OMS.

7. W menu nawigacji po lewej stronie kliknij **dzienników kont magazynu**w obszarze *źródeł danych obszaru roboczego*.

8. Na *dzienniki konta magazynu* kliknij przycisk **Dodaj** u góry, aby dodać dzienników z klastra do obszaru roboczego.

9. Kliknij przycisk do **konta magazynu** można dodać odpowiednie konta tworzone w klastrze. Jeśli zostanie użyta domyślna nazwa, nosi nazwę konta magazynu *sfdg\<resourceGroupName\>*. Można również to potwierdzić, sprawdzając szablonu usługi Azure Resource Manager używane do wdrażania klastra, sprawdzając wartość używaną do `applicationDiagnosticsStorageAccountName`. Może być również konieczne przewiń w dół i kliknij przycisk **załadować więcej** Jeśli nazwa nie jest wyświetlany. Kliknij nazwę konta magazynu prawo górę aby go wybrać.

10. Następnie należy określić *— typ danych*, powinien być ustawiony na **zdarzenia sieci szkieletowej usług**.

11. *Źródła* automatycznie powinien być ustawiony na *WADServiceFabric\*EventTable*.

12. Kliknij przycisk **OK** nawiązać obszaru roboczego dzienników z klastra.

    ![Dodaj dzienniki konta magazynu z usługą OMS](media/service-fabric-diagnostics-event-analysis-oms/add-storage-account.png)

Konto powinna zostać wyświetlona jako część programu *dzienniki konta magazynu* w źródłach danych obszaru roboczego.

Z tym zostało dodane rozwiązania analizy sieci szkieletowej usług w obszarze roboczym analizy dzienników OMS, który jest teraz prawidłowo podłączone do sieci klastra platformy oraz tabeli dziennika aplikacji. Możesz dodać dodatkowe źródła do obszaru roboczego w taki sam sposób.

## <a name="next-steps"></a>Następne kroki
* [Wdrażanie agenta pakietu OMS](service-fabric-diagnostics-oms-agent.md) na węzły do zbierania liczników wydajności i zbieranie statystyk docker i dzienniki dla kontenerów
* Pobierz zapoznaniu się z [wyszukiwania i badania dziennika](../log-analytics/log-analytics-log-searches.md) funkcje dostępne w ramach analizy dzienników
* [Projektant widoków umożliwia tworzenie niestandardowych widoków w analizy dzienników](../log-analytics/log-analytics-view-designer.md)
