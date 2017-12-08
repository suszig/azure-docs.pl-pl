---
title: "Za pomocą szablonów usługi Azure Resource Manager do tworzenia i konfigurowania obszaru roboczego analizy dzienników | Dokumentacja firmy Microsoft"
description: "Szablony usługi Azure Resource Manager umożliwia tworzenie i konfigurowanie analizy dzienników obszarów roboczych."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: jochan
editor: 
ms.assetid: d21ca1b0-847d-4716-bb30-2a8c02a606aa
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: json
ms.topic: article
ms.date: 10/16/2017
ms.author: richrund
ms.openlocfilehash: 7f522a672d1691990bec3e63a41b2ed7e81058ad
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/17/2017
---
# <a name="manage-log-analytics-using-azure-resource-manager-templates"></a>Zarządzanie za pomocą szablonów usługi Azure Resource Manager analizy dzienników
Można użyć [szablonów usługi Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md) do tworzenia i konfigurowania analizy dzienników obszarów roboczych. Zadania, które można wykonywać za pomocą szablonów należą:

* Tworzenie obszaru roboczego
* Dodaj rozwiązanie
* Tworzenie zapisanych wyszukiwań
* Tworzenie grupy komputerów
* Włącz zbieranie dzienników usług IIS z komputerów z zainstalowanym agentem systemu Windows
* Zebrać liczników wydajności z komputerów z systemami Linux i Windows
* Zbierać zdarzenia z dziennika systemowego na komputerach z systemem Linux 
* Zbieranie zdarzeń z dzienników zdarzeń systemu Windows
* Zbieranie dzienników zdarzeń niestandardowych
* Dodaj agenta analizy dziennika do maszyny wirtualnej platformy Azure
* Konfigurowanie analizy dzienników do indeksowania danych zbieranych za pomocą diagnostyki Azure

Ten artykuł zawiera szablon przykłady ilustrujące część konfiguracji, który można wykonać za pomocą szablonów.

## <a name="api-versions"></a>Wersje interfejsu API
Przykład, w tym artykule [uaktualnione obszaru roboczego analizy dzienników](log-analytics-log-search-upgrade.md).  Użyć starszej wersji obszaru roboczego, musisz zmienić składnię zapytania dla starszej wersji języka i Zmień wersję interfejsu API dla każdego zasobu.  W poniższej tabeli wymieniono wersja interfejsu API dla zasobów używanych w tym przykładzie.

| Zasób | Typ zasobu | Starszych wersji interfejsu API | Uaktualnionej wersji interfejsu API |
|:---|:---|:---|:---|
| Obszar roboczy   | Obszary robocze    | 2015-11-01-preview | 2017-03-15-preview |
| Wyszukiwanie      | savedSearches | 2015-11-01-preview | 2017-03-15-preview |
| Źródło danych | źródła danych   | 2015-11-01-preview | 2015-11-01-preview |
| Rozwiązanie    | rozwiązania     | 2015-11-01-preview | 2015-11-01-preview |


## <a name="create-and-configure-a-log-analytics-workspace"></a>Tworzenie i konfigurowanie obszaru roboczego analizy dzienników
Poniższy przykład szablonu ilustruje sposób:

1. Tworzenie obszaru roboczego, w tym ustawienia przechowywania danych
2. Dodawanie rozwiązania do obszaru roboczego
3. Tworzenie zapisanych wyszukiwań
4. Tworzenie grupy komputerów
5. Włącz zbieranie dzienników usług IIS z komputerów z zainstalowanym agentem systemu Windows
6. Zbierania z komputerów z systemem Linux liczników wydajności dysku logicznego (% użytych węzłów i; Wolne megabajty; % Używane miejsce; Transfery dyskowe/s; Odczyty dysku/s; Zapisy dysku/s)
7. Zbieraj zdarzenia dziennika systemowego z komputerów z systemem Linux
8. Zbieranie zdarzeń błędu i ostrzeżenia w dzienniku zdarzeń aplikacji z komputerów z systemem Windows
9. Zbieraj dane licznika wydajności dostępna pamięć (MB) z komputerów z systemem Windows
10. Zbieranie dzienników niestandardowych 
11. Zbieranie dzienników usług IIS i dzienniki zdarzeń systemu Windows zapisywane przez diagnostycznych platformy Azure na konto magazynu

```
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "workspaceName"
      }
    },
    "serviceTier": {
      "type": "string",
      "allowedValues": [
        "Free",
        "Standalone",
        "PerNode"
      ],
      "metadata": {
        "description": "Service Tier: Free, Standalone, or PerNode"
    }
      },
    "dataRetention": {
      "type": "int",
      "defaultValue": 30,
      "minValue": 7,
      "maxValue": 730,
      "metadata": {
        "description": "Number of days of retention. Free plans can only have 7 days, Standalone and OMS plans include 30 days for free"
      }
    },
    "location": {
      "type": "string",
      "allowedValues": [
        "East US",
        "West Europe",
        "Southeast Asia",
        "Australia Southeast"
      ]
    },
    "applicationDiagnosticsStorageAccountName": {
        "type": "string",
        "metadata": {
          "description": "Name of the storage account with Azure diagnostics output"
        }
    },
    "applicationDiagnosticsStorageAccountResourceGroup": {
        "type": "string",
        "metadata": {
          "description": "The resource group name containing the storage account with Azure diagnostics output"
        }
    }
  },
  "variables": {
    "Updates": {
      "Name": "[Concat('Updates', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "Updates"
    },
    "AntiMalware": {
      "Name": "[concat('AntiMalware', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "AntiMalware"
    },
    "SQLAssessment": {
      "Name": "[Concat('SQLAssessment', '(', parameters('workspaceName'), ')')]",
      "GalleryName": "SQLAssessment"
    },
    "diagnosticsStorageAccount": "[resourceId(parameters('applicationDiagnosticsStorageAccountResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('applicationDiagnosticsStorageAccountName'))]"
  },
  "resources": [
    {
      "apiVersion": "2017-03-15-preview",
      "type": "Microsoft.OperationalInsights/workspaces",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "properties": {
        "sku": {
          "Name": "[parameters('serviceTier')]"
        },
    "retention": "[parameters('dataRetention')]"
      },
      "resources": [
        {
          "apiVersion": "2017-03-15-preview",
          "name": "VMSS Queries2",
          "type": "savedSearches",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "Category": "VMSS",
            "ETag": "*",
            "DisplayName": "VMSS Instance Count",
            "Query": "Event | where Source == "ServiceFabricNodeBootstrapAgent" | summarize AggregatedValue = count() by Computer",
            "Version": 1
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsEvent1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsEvent",
          "properties": {
            "eventLogName": "Application",
            "eventTypes": [
              {
                "eventType": "Error"
              },
              {
                "eventType": "Warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleWindowsPerfCounter1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "WindowsPerformanceCounter",
          "properties": {
            "objectName": "Memory",
            "instanceName": "*",
            "intervalSeconds": 10,
            "counterName": "Available MBytes"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleIISLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "IISLogs",
          "properties": {
            "state": "OnPremiseEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslog",
          "properties": {
            "syslogName": "kern",
            "syslogSeverities": [
              {
                "severity": "emerg"
              },
              {
                "severity": "alert"
              },
              {
                "severity": "crit"
              },
              {
                "severity": "err"
              },
              {
                "severity": "warning"
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleSyslogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxSyslogCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerf1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceObject",
          "properties": {
            "performanceCounters": [
              {
                "counterName": "% Used Inodes"
              },
              {
                "counterName": "Free Megabytes"
              },
              {
                "counterName": "% Used Space"
              },
              {
                "counterName": "Disk Transfers/sec"
              },
              {
                "counterName": "Disk Reads/sec"
              },
              {
                "counterName": "Disk Writes/sec"
              }
            ],
            "objectName": "Logical Disk",
            "instanceName": "*",
            "intervalSeconds": 10
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleLinuxPerfCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "LinuxPerformanceCollection",
          "properties": {
            "state": "Enabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleCustomLog1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLog",
          "properties": {
            "customLogName": "sampleCustomLog1",
            "description": "test custom log datasources",
            "inputs": [
              {
                "location": {
                  "fileSystemLocations": {
                    "windowsFileTypeLogPaths": [ "e:\\iis5\\*.log" ],
                    "linuxFileTypeLogPaths": [ "/var/logs" ]
                  }
                },
                "recordDelimiter": {
                  "regexDelimiter": {
                    "pattern": "\\n",
                    "matchIndex": 0,
                    "matchIndexSpecified": true,
                    "numberedGroup": null
                  }
                }
              }
            ],
            "extractions": [
              {
                "extractionName": "TimeGenerated",
                "extractionType": "DateTime",
                "extractionProperties": {
                  "dateTimeExtraction": {
                    "regex": null,
                    "joinStringRegex": null
                  }
                }
              }
            ]
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "type": "datasources",
          "name": "sampleCustomLogCollection1",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "kind": "CustomLogCollection",
          "properties": {
            "state": "LinuxLogsEnabled"
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "name": "[concat(parameters('applicationDiagnosticsStorageAccountName'),parameters('workspaceName'))]",
          "type": "storageinsightconfigs",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "containers": [ 
              "wad-iis-logfiles" 
            ],
            "tables": [
              "WADWindowsEventLogsTable"
            ],
            "storageAccount": {
              "id": "[variables('diagnosticsStorageAccount')]",
              "key": "[listKeys(variables('diagnosticsStorageAccount'),'2015-06-15').key1]"
            }
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('Updates').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('Updates').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('Updates').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('Updates').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('AntiMalware').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('AntiMalware').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('AntiMalware').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('AntiMalware').GalleryName)]",
            "promotionCode": ""
          }
        },
        {
          "apiVersion": "2015-11-01-preview",
          "location": "[parameters('location')]",
          "name": "[variables('SQLAssessment').Name]",
          "type": "Microsoft.OperationsManagement/solutions",
          "id": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.OperationsManagement/solutions/', variables('SQLAssessment').Name)]",
          "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          ],
          "properties": {
            "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'))]"
          },
          "plan": {
            "name": "[variables('SQLAssessment').Name]",
            "publisher": "Microsoft",
            "product": "[Concat('OMSGallery/', variables('SQLAssessment').GalleryName)]",
            "promotionCode": ""
          }
        }
      ]
    }
  ],
  "outputs": {
    "workspaceName": {
      "type": "string",
      "value": "[parameters('workspaceName')]"
    },
    "provisioningState": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').provisioningState]"
    },
    "source": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').source]"
    },
    "customerId": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').customerId]"
    },
    "pricingTier": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').sku.name]"
    },
    "retentionInDays": {
      "type": "int",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').retentionInDays]"
    },
    "portalUrl": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-11-01-preview').portalUrl]"
    }
  }
}

```
### <a name="deploying-the-sample-template"></a>Wdrażanie przykładowy szablon
Aby wdrożyć przykładowy szablon:

1. Na przykład zapisać przykładowe dołączone w pliku,`azuredeploy.json` 
2. Edytuj szablon do konfiguracji, który ma
3. Użyj programu PowerShell lub wiersza polecenia do wdrożenia szablonu

#### <a name="powershell"></a>PowerShell
`New-AzureRmResourceGroupDeployment -Name <deployment-name> -ResourceGroupName <resource-group-name> -TemplateFile azuredeploy.json`

#### <a name="command-line"></a>Wiersz polecenia
```
azure config mode arm
azure group deployment create <my-resource-group> <my-deployment-name> --TemplateFile azuredeploy.json
```


## <a name="example-resource-manager-templates"></a>Szablony przykład Resource Manager
Galerii szablonów Szybki Start Azure zawiera kilka szablonów dla analizy dziennika, w tym:

* [Wdrażanie maszyny wirtualnej z systemem Windows z rozszerzeniem wirtualna analizy dzienników](https://azure.microsoft.com/documentation/templates/201-oms-extension-windows-vm/)
* [Wdróż maszynę wirtualną z systemem Linux z rozszerzeniem wirtualna analizy dzienników](https://azure.microsoft.com/documentation/templates/201-oms-extension-ubuntu-vm/)
* [Monitor usługi Azure Site Recovery przy użyciu istniejący obszar roboczy analizy dzienników](https://azure.microsoft.com/documentation/templates/asr-oms-monitoring/)
* [Monitorowanie aplikacji sieci Web platformy Azure przy użyciu istniejący obszar roboczy analizy dzienników](https://azure.microsoft.com/documentation/templates/101-webappazure-oms-monitoring/)
* [Monitor SQL Azure za pomocą istniejący obszar roboczy analizy dzienników](https://azure.microsoft.com/documentation/templates/101-sqlazure-oms-monitoring/)
* [Wdrażanie klastra usługi sieć szkieletowa usług i monitorować je z istniejącym obszarem roboczym analizy dzienników](https://azure.microsoft.com/documentation/templates/service-fabric-oms/)
* [Wdrażanie klastra usługi sieć szkieletowa usług i utworzyć obszaru roboczego analizy dzienników, aby ją monitorować](https://azure.microsoft.com/documentation/templates/service-fabric-vmss-oms/)

## <a name="next-steps"></a>Następne kroki
* [Wdrażanie agentów w maszynach wirtualnych platformy Azure przy użyciu szablonów usługi Resource Manager](log-analytics-azure-vm-extension.md)

