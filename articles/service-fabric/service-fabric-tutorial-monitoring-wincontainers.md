---
title: "Monitorowania i diagnostyki dla kontenerów systemu Windows na platformie Azure Service Fabric | Dokumentacja firmy Microsoft"
description: "Konfigurowanie monitorowania i diagnostyki dla kontenera systemu Windows zorkiestrowana na sieć szkieletowa usług Azure."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/20/2017
ms.author: dekapur
ms.custom: mvc
ms.openlocfilehash: 69a59ea9fb93f6e9f3f3eea66b1a9e973b1b4eea
ms.sourcegitcommit: f67f0bda9a7bb0b67e9706c0eb78c71ed745ed1d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/20/2017
---
# <a name="monitor-windows-containers-on-service-fabric-using-oms"></a>Monitor kontenery systemu Windows w sieci szkieletowej usług za pomocą pakietu OMS

Jest to część trzy samouczka i przeprowadzi Cię przez proces konfigurowania OMS do monitorowania kontenerów Windows zorkiestrowana w sieci szkieletowej usług.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Skonfiguruj OMS dla klastra sieci szkieletowej usług
> * Umożliwia wyświetlanie obszarem roboczym pakietu OMS i rejestruje zapytania z kontenerów i węzłów
> * Konfigurowanie agenta pakietu OMS do pobrania kontenera i metryki węzła

## <a name="prerequisites"></a>Wymagania wstępne
Przed rozpoczęciem tego samouczka, wykonaj następujące czynności:
- Klaster na platformie Azure, lub [utworzyć w tym samouczku](service-fabric-tutorial-create-vnet-and-windows-cluster.md)
- [Wdrażanie aplikacji konteneryzowanych do niego](service-fabric-host-app-in-a-container.md)

## <a name="setting-up-oms-with-your-cluster-in-the-resource-manager-template"></a>Konfigurowanie OMS z klastrem w szablonie usługi Resource Manager

W przypadku gdy używasz [dostarczonego szablonu](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/Tutorial) w pierwszej części tego samouczka, powinny zawierać następujące dodatki do ogólnego szablonu usługi sieć szkieletowa usługi Azure Resource Manager. W przypadku przypadku posiadania klastra własne tego chcesz skonfigurowanej na potrzeby monitorowania kontenerów OMS:
* Wprowadź następujące zmiany w szablonie usługi Resource Manager.
* Wdróż je za pomocą programu PowerShell do uaktualnienia klastra przez [wdrażanie szablonu](https://docs.microsoft.com/en-us/azure/service-fabric/service-fabric-cluster-creation-via-arm). Usługa Azure Resource Manager realizuje, czy zasób istnieje, więc będzie wprowadzana jego uaktualnienie.

### <a name="adding-oms-to-your-cluster-template"></a>Dodawanie do szablonu klastra OMS

Wprowadź następujące zmiany w Twojej *template.json*:

1. Dodaj lokalizację obszar roboczy OMS i nazw użytkownika *parametry* sekcji:
    
    ```json
    "omsWorkspacename": {
      "type": "string",
      "defaultValue": "[toLower(concat('sf',uniqueString(resourceGroup().id)))]",
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

    Aby zmienić wartość używaną do albo dodać takie same parametry dla Twojego *template.parameters.json* i zmień wartości używane.

2. Dodaj nazwę rozwiązania i rozwiązania pod kątem Twojej *zmienne*: 
    
    ```json
    "omsSolutionName": "[Concat('ServiceFabric', '(', parameters('omsWorkspacename'), ')')]",
    "omsSolution": "ServiceFabric"
    ```

3. Dodaj OMS Microsoft Monitoring Agent jako rozszerzenie maszyny wirtualnej. Znajdowanie zasobów zestawach skali maszyn wirtualnych: *zasobów* > *"apiVersion": "[variables('vmssApiVersion')]"*. W obszarze *właściwości* > *virtualMachineProfile* > *extensionProfile* > *rozszerzenia*, Dodaj następujący opis rozszerzenia w obszarze *ServiceFabricNode* rozszerzenia: 
    
    ```json
    {
        "name": "[concat(variables('vmNodeType0Name'),'OMS')]",
        "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
                "workspaceId": "[reference(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')), '2015-11-01-preview').customerId]"
            },
            "protectedSettings": {
                "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename')),'2015-11-01-preview').primarySharedKey]"
            }
        }
    },
    ```

4. Dodaj obszar roboczy OMS jako pojedynczego zasobu. W *zasobów*po skali maszyny wirtualnej ustawia zasobów, Dodaj następujący kod:
    
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
            },
            {
                "apiVersion": "2015-11-01-preview",
                "name": "System",
                "type": "datasources",
                "dependsOn": [
                    "[concat('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
                ],
                "kind": "WindowsEvent",
                "properties": {
                    "eventLogName": "System",
                    "eventTypes": [
                        {
                            "eventType": "Error"
                        },
                        {
                            "eventType": "Warning"
                        },
                        {
                            "eventType": "Information"
                        }
                    ]
                }
            }
        ]
    },
    {
        "apiVersion": "2015-11-01-preview",
        "location": "[parameters('omsRegion')]",
        "name": "[variables('omsSolutionName')]",
        "type": "Microsoft.OperationsManagement/solutions",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('OMSWorkspacename'))]"
        ],
        "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('omsWorkspacename'))]"
        },
        "plan": {
            "name": "[variables('omsSolutionName')]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('omsSolution'))]",
            "promotionCode": ""
        }
    },
    ```

[W tym miejscu](https://github.com/ChackDan/Service-Fabric/blob/master/ARM%20Templates/Tutorial/azuredeploy.json) jest to szablon próbki (używanych w części, jeden z tego samouczka), który zawiera wszystkie te zmiany, które mogą odwoływać się zgodnie z potrzebami. Te zmiany zostaną dodane obszaru roboczego analizy dzienników OMS do grupy zasobów. Obszar roboczy zostanie skonfigurowana w celu odebrania zdarzeń platformy sieci szkieletowej usług z magazynu tabel skonfigurowano [Windows Azure Diagnostics](service-fabric-diagnostics-event-aggregation-wad.md) agenta. Agent pakietu OMS (Microsoft Monitoring Agent) również został dodany do każdego węzła w klastrze jako rozszerzenie maszyny wirtualnej - oznacza to, że skalowanie klastra agenta jest automatycznie skonfigurowana na każdej maszynie i podłączonymi do tego samego obszaru roboczego.

Wdrażanie szablonu z nowe zmiany, aby uaktualnić bieżącego klastra. Po zakończeniu to powinna zostać wyświetlona OMS zasoby w grupie zasobów. Gdy klaster będzie gotowy, wdrażania konteneryzowanych aplikacji do niego. W następnym kroku skonfigurujemy monitorowania kontenerów.

## <a name="add-the-container-monitoring-solution-to-your-oms-workspace"></a>Dodaj rozwiązanie monitorowanie kontenera na obszar roboczy OMS

Aby skonfigurować rozwiązanie kontenera, w obszarze roboczym, wyszukaj *rozwiązanie monitorowanie kontenera* i Utwórz zasób kontenerów (w obszarze monitorowanie i zarządzanie kategorii).

![Dodawanie rozwiązania kontenerów](./media/service-fabric-tutorial-monitoring-wincontainers/containers-solution.png)

Po wyświetleniu monitu o *obszarem roboczym pakietu OMS*, wybierz obszar roboczy, który został utworzony w grupie zasobów i kliknij przycisk **Utwórz**. Spowoduje to dodanie *rozwiązanie monitorowanie kontenera* do swojego obszaru roboczego, automatycznie spowoduje agent pakietu OMS wdrożone przez szablon rozpocząć zbieranie dzienników docker i statystyki. 

Przejdź z powrotem do użytkownika *grupy zasobów*, gdzie powinien zostać wyświetlony nowo dodanego rozwiązanie monitorowania. Jeśli klikniesz przycisk do niego, strona docelowa powinny być widoczne można liczbę obrazów kontenera masz uruchomiony. 

*Należy pamiętać, że został uruchomiony 5 wystąpień kontenera mojej firmy fabrikam z [część druga](service-fabric-host-app-in-a-container.md) samouczka*

![Strona docelowa rozwiązania kontenera](./media/service-fabric-tutorial-monitoring-wincontainers/solution-landing.png)

Kliknięcie przycisku do **kontenera monitory** spowoduje przejście do bardziej szczegółowych pulpitu nawigacyjnego, dzięki czemu można przewijać wielu panele, a także wykonywać zapytania w analizy dzienników. 

*Należy pamiętać, że począwszy od września 2017 r. rozwiązania przechodzi przez niektóre aktualizacje - Ignoruj wszystkie błędy, które mogą wystąpić o zdarzeniach Kubernetes możemy pracy dotyczące integrowania wielu orchestrators w tym samym rozwiązaniu.*

Ponieważ agent jest pobieranie dzienników docker, domyślne przedstawiający *stdout* i *stderr*. Przewiń w prawo, zobaczysz kontener obrazu spisu, stanu, metryki i przykładowe zapytania, które można uruchomić, aby uzyskać bardziej użyteczne dane. 

![Pulpit nawigacyjny rozwiązania kontenera](./media/service-fabric-tutorial-monitoring-wincontainers/container-metrics.png)

Klikając pozycję w każdym z tych zespołów spowoduje przejście do zapytania analizy dzienników, które generuje wartość wyświetlana. Zmień zapytanie w celu  *\**  aby zobaczyć wszystkie rodzaje dzienników, które są odbierany. W tym miejscu można wysyłać zapytania filtra dla kontenera, wydajność, dzienniki, lub Sprawdź zdarzenia platformy Service Fabric. Agentów są również stale emitowanie pulsu z każdego węzła, który można przyjrzeć się upewnić, że dane są nadal zbierane z wszystkich maszynach w przypadku zmiany konfiguracji klastra.   

![Kontener zapytania](./media/service-fabric-tutorial-monitoring-wincontainers/query-sample.png)

## <a name="configure-oms-agent-to-pick-up-performance-counters"></a>Konfigurowanie agenta pakietu OMS do pobrania liczniki wydajności

Inną zaletą używania agent pakietu OMS jest możliwość zmiany do odebrania przez możliwości interfejsu użytkownika pakietu OMS liczniki wydajności, zamiast konieczności skonfigurowania agenta diagnostyki Azure i wykonaj Menedżera zasobów na podstawie szablonu uaktualnienia zawsze. Aby to zrobić, kliknij **portalu OMS** na strony docelowej rozwiązania monitorowanie kontenera (lub usługi sieć szkieletowa).

![Portal pakietu OMS](./media/service-fabric-tutorial-monitoring-wincontainers/oms-portal.png)

To spowoduje przejście do obszaru roboczego w portalu OMS, w którym można wyświetlać rozwiązań, tworzyć niestandardowe pulpity nawigacyjne, a także skonfigurować agent pakietu OMS. 
* Polecenie **kółka koło zębate** w prawym górnym rogu ekranu, aby otworzyć *ustawienia* menu.
* Polecenie **połączonych źródeł** > **serwerów z systemem Windows** Aby sprawdzić, czy masz *połączone 5 komputerów z systemem Windows*.
* Polecenie **danych** > **liczników wydajności systemu Windows** do wyszukiwania i dodać nowe liczniki wydajności. W tym miejscu zostanie wyświetlona lista zaleceń z usługą OMS liczników wydajności, można zebrać, a także opcję, aby wyszukać innych liczników. Kliknij przycisk **Dodaj wybrane liczniki wydajności** aby rozpocząć zbieranie metryk sugerowane.

    ![Liczniki wydajności](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters.png)

W portalu Azure **Odśwież** rozwiązania monitorowanie kontenera w kilka minut, a użytkownik powinien rozpoczynać się w temacie *wydajność komputera* dane o użyciu. Pomoże to zrozumieć, jak są używane zasoby. Można również użyć tych metryk podjęcie odpowiednich decyzji dotyczących skalowanie klastra lub upewnij się, jeśli klaster jest równoważenia obciążenia, zgodnie z oczekiwaniami.

*Uwaga: Upewnij się, że filtry czasu są ustawione właściwie można korzystać z tych metryk.* 

![Liczniki wydajności 2](./media/service-fabric-tutorial-monitoring-wincontainers/perf-counters2.png)


## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Skonfiguruj OMS dla klastra sieci szkieletowej usług
> * Umożliwia wyświetlanie obszarem roboczym pakietu OMS i rejestruje zapytania z kontenerów i węzłów
> * Konfigurowanie agenta pakietu OMS do pobrania kontenera i metryki węzła

Teraz, gdy zdefiniowano monitorowania konteneryzowanych aplikacji, wykonaj następujące czynności:

* Konfigurowanie OMS przez klaster systemu Linux następujące kroki podobnie jak powyżej. Odwołanie [ten szablon](https://github.com/ChackDan/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Samples/Linux) Aby wprowadzić zmiany w szablonie usługi Resource Manager.
* Skonfiguruj OMS, aby skonfigurować [automatycznego alerty](../log-analytics/log-analytics-alerts.md) do pomocy w wykrywaniu i informacji diagnostycznych.
* Eksplorowanie usługi sieć szkieletowa lista [zalecane liczniki wydajności](service-fabric-diagnostics-event-generation-perf.md) można skonfigurować dla klastrów.
* Pobierz zapoznaniu się z [wyszukiwania i badania dziennika](../log-analytics/log-analytics-log-searches.md) funkcje dostępne w ramach analizy dzienników.