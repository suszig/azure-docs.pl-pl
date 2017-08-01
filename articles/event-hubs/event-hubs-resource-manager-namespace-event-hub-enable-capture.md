---
title: "Tworzenie przestrzeni nazw usługi Azure Event Hubs i włączanie funkcji przechwytywania przy użyciu szablonu | Microsoft Docs"
description: "Tworzenie przestrzeni nazw usługi Azure Event Hubs z jednym centrum zdarzeń i włączanie funkcji przechwytywania przy użyciu szablonu usługi Azure Resource Manager"
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
editor: 
ms.assetid: 8bdda6a2-5ff1-45e3-b696-c553768f1090
ms.service: event-hubs
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 06/28/2017
ms.author: shvija;sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: 857267f46f6a2d545fc402ebf3a12f21c62ecd21
ms.openlocfilehash: f19a3d9b323d75ae23480d0699d55b79bb7d2e84
ms.contentlocale: pl-pl
ms.lasthandoff: 06/28/2017


---
# <a name="create-an-event-hubs-namespace-with-an-event-hub-and-enable-capture-using-an-azure-resource-manager-template"></a>Tworzenie przestrzeni nazw usługi Event Hubs z centrum zdarzeń i włączanie funkcji przechwytywania przy użyciu szablonu usługi Azure Resource Manager
W tym artykule przedstawiono sposób używania szablonu usługi Azure Resource Manager umożliwiającego utworzenie przestrzeni nazw usługi Event Hubs z jednym wystąpieniem centrum zdarzeń oraz włączenie funkcji przechwytywania w tym centrum zdarzeń. W tym artykule opisano, jak wskazać wdrażane zasoby oraz jak podać parametry realizacji wdrożenia. Można użyć tego szablonu na potrzeby własnych wdrożeń lub dostosować go do konkretnych potrzeb.

Aby uzyskać więcej informacji na temat tworzenia szablonów, zobacz [Tworzenie szablonów usługi Azure Resource Manager][Authoring Azure Resource Manager templates].

Aby uzyskać więcej informacji na temat praktycznych rozwiązań i wzorców dotyczących konwencji nazewnictwa zasobów platformy Azure, zobacz [Azure Resources Naming Conventions (Konwencje nazewnictwa zasobów platformy Azure)][Azure Resources Naming Conventions].

Aby uzyskać kompletny szablon, zobacz temat [Event hub and enable Capture template (Szablon dotyczący centrum zdarzeń i włączania funkcji przechwytywania)][Event Hub and enable Capture template] w witrynie GitHub.

> [!NOTE]
> Aby sprawdzić najnowsze szablony, odwiedź galerię [Szablony szybkiego startu platformy Azure][Azure Quickstart Templates] i wyszukaj hasło Event Hubs.
> 
> 

## <a name="what-will-you-deploy"></a>Co chcesz wdrożyć?
Ten szablon umożliwia wdrożenie przestrzeni nazw usługi Event Hubs z centrum zdarzeń oraz włączenie [funkcji przechwytywania usługi Event Hubs](event-hubs-capture-overview.md).

[Event Hubs](event-hubs-what-is-event-hubs.md) to usługa służąca do przetwarzania zdarzeń, która dostarcza zdarzenia i dane telemetryczne do platformy Azure w bardzo dużej skali, z małymi opóźnieniami i wysoką niezawodnością. Funkcja przechwytywania usługi Event Hubs pozwala automatycznie dostarczać strumień danych usługi Event Hubs do wybranego wystąpienia usługi Azure Blob Storage w wyznaczonym okresie lub przy określonym interwale rozmiaru.

Aby automatycznie uruchomić wdrożenie, kliknij poniższy przycisk:

[![Wdrażanie na platformie Azure](./media/event-hubs-resource-manager-namespace-event-hub/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-eventhubs-create-namespace-and-enable-capture%2Fazuredeploy.json)

## <a name="parameters"></a>Parametry
Przy użyciu usługi Azure Resource Manager można zdefiniować parametry dla wartości, które mają zostać uwzględnione podczas wdrażania szablonu. Szablon zawiera sekcję o nazwie `Parameters` obejmującą wszystkie wartości parametrów. Parametr powinien obejmować wartości, które różnią się w zależności od wdrażanego projektu lub środowiska, w którym odbywa się wdrożenie. Nie należy definiować parametrów dla wartości, które pozostają niezmienione. Każda wartość parametru używana w szablonie definiuje wdrażane zasoby.

Szablon zawiera definicje następujących parametrów.

### <a name="eventhubnamespacename"></a>eventHubNamespaceName
Nazwa tworzonej przestrzeni nazw usługi Event Hubs.

```json
"eventHubNamespaceName":{  
     "type":"string",
     "metadata":{  
         "description":"Name of the EventHub namespace"
      }
}
```

### <a name="eventhubname"></a>eventHubName
Nazwa centrum zdarzeń utworzonego w przestrzeni nazw usługi Event Hubs.

```json
"eventHubName":{  
    "type":"string",
    "metadata":{  
        "description":"Name of the event hub"
    }
}
```

### <a name="messageretentionindays"></a>messageRetentionInDays
Liczba dni przechowywania komunikatów w centrum zdarzeń. 

```json
"messageRetentionInDays":{
    "type":"int",
    "defaultValue": 1,
    "minValue":"1",
    "maxValue":"7",
    "metadata":{
       "description":"How long to retain the data in event hub"
     }
 }
```

### <a name="partitioncount"></a>partitionCount
Liczba partycji w utworzonym centrum zdarzeń.

```json
"partitionCount":{
    "type":"int",
    "defaultValue":2,
    "minValue":2,
    "maxValue":32,
    "metadata":{
        "description":"Number of partitions chosen"
    }
 }
```

### <a name="captureenabled"></a>captureEnabled
Włączanie funkcji przechwytywania w centrum zdarzeń.

```json
"captureEnabled":{
    "type":"string",
    "defaultValue":"true",
    "allowedValues": [
    "false",
    "true"],
    "metadata":{
        "description":"Enable or disable the Capture for your event hub"
    }
 }
```
### <a name="captureencodingformat"></a>captureEncodingFormat
Format kodowania na potrzeby serializowania danych zdarzeń.

```json
"captureEncodingFormat":{
    "type":"string",
    "defaultValue":"Avro",
    "allowedValues":[
    "Avro"],
    "metadata":{
        "description":"The encoding format in which Capture serializes the EventData"
    }
}
```

### <a name="capturetime"></a>captureTime
Przedział czasu, w którym funkcja przechwytywania usługi Event Hubs pobiera dane do usługi Azure Blob Storage.

```json
"captureTime":{
    "type":"int",
    "defaultValue":300,
    "minValue":60,
    "maxValue":900,
    "metadata":{
         "description":"the time window in seconds for the capture"
    }
}
```

### <a name="capturesize"></a>captureSize
Interwał rozmiaru, w którym funkcja przechwytywania pobiera dane do usługi Azure Blob Storage.

```json
"captureSize":{
    "type":"int",
    "defaultValue":314572800,
    "minValue":10485760,
    "maxValue":524288000,
    "metadata":{
        "description":"The size window in bytes for capture"
    }
}
```

### <a name="destinationstorageaccountresourceid"></a>destinationStorageAccountResourceId
Przechwytywanie danych na odpowiednim koncie usługi Storage wymaga identyfikatora zasobu konta usługi Azure Storage.

```json
 "destinationStorageAccountResourceId":{
    "type":"string",
    "metadata":{
        "description":"Your existing Storage account resource id where you want the blobs be captured"
    }
 }
```

### <a name="blobcontainername"></a>blobContainerName
Kontener obiektów blob, w którym będą przechwytywane dane zdarzeń.

```json
 "blobContainerName":{
    "type":"string",
    "metadata":{
        "description":"Your existing storage container in which you want the blobs captured"
    }
}
```


### <a name="apiversion"></a>apiVersion
Wersja interfejsu API szablonu.

```json
 "apiVersion":{  
    "type":"string",
    "defaultValue":"2015-08-01",
    "metadata":{  
        "description":"ApiVersion used by the template"
    }
 }
```

## <a name="resources-to-deploy"></a>Zasoby wymagające wdrożenia
Tworzy przestrzeń nazw typu **EventHubs** z jednym centrum zdarzeń oraz włącza funkcję przechwytywania.

```json
"resources":[  
      {  
         "apiVersion":"[variables('ehVersion')]",
         "name":"[parameters('eventHubNamespaceName')]",
         "type":"Microsoft.EventHub/Namespaces",
         "location":"[variables('location')]",
         "sku":{  
            "name":"Standard",
            "tier":"Standard"
         },
         "resources":[  
            {  
               "apiVersion":"[variables('ehVersion')]",
               "name":"[parameters('eventHubName')]",
               "type":"EventHubs",
               "dependsOn":[  
                  "[concat('Microsoft.EventHub/namespaces/', parameters('eventHubNamespaceName'))]"
               ],
               "properties":{  
                  "path":"[parameters('eventHubName')]",
                  "MessageRetentionInDays":"[parameters('messageRetentionInDays')]",
                  "PartitionCount":"[parameters('partitionCount')]",
                  "CaptureDescription":{
                        "enabled":"[parameters('captureEnabled')]",
                        "encoding":"[parameters('captureEncodingFormat')]",
                        "intervalInSeconds":"[parameters('captureTime')]",
                        "sizeLimitInBytes":"[parameters('captureSize')]",
                        "destination":{
                            "name":"EventHubCapture.AzureBlockBlob",
                            "properties":{
                                "StorageAccountResourceId":"[parameters('destinationStorageAccountResourceId')]",
                                "BlobContainer":"[parameters('blobContainerName')]"
                            }
                        } 
                  }

               }

            }
         ]
      }
   ]
```

## <a name="commands-to-run-deployment"></a>Polecenia umożliwiające uruchomienie wdrożenia
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

## <a name="powershell"></a>PowerShell
```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName \<resource-group-name\> -TemplateFile https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json
```

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure
```cli
azure config mode arm

azure group deployment create \<my-resource-group\> \<my-deployment-name\> --template-uri [https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/201-eventhubs-create-namespace-and-enable-capture/azuredeploy.json][]
```
## <a name="next-steps"></a>Następne kroki

Funkcję przechwytywania usługi Event Hubs można również skonfigurować za pośrednictwem witryny [Azure Portal](https://portal.azure.com). Aby uzyskać więcej informacji, zobacz [Enable Event Hubs Capture using the Azure portal (Włączanie funkcji przechwytywania usługi Event Hubs przy użyciu witryny Azure Portal)](event-hubs-capture-enable-through-portal.md).

Następujące linki pozwalają dowiedzieć się więcej na temat usługi Event Hubs:

* [Omówienie usługi Event Hubs](event-hubs-what-is-event-hubs.md)
* [Tworzenie centrum zdarzeń](event-hubs-create.md)
* [Event Hubs — często zadawane pytania](event-hubs-faq.md)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/resource-group-authoring-templates.md
[Azure Quickstart Templates]:  https://azure.microsoft.com/documentation/templates/?term=event+hubs
[Using Azure PowerShell with Azure Resource Manager]: ../powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../xplat-cli-azure-resource-manager.md
[Azure Resources Naming Conventions]: https://azure.microsoft.com/documentation/articles/guidance-naming-conventions/
[Event hub and enable Capture template]: https://github.com/Azure/azure-quickstart-templates/tree/master/201-eventhubs-create-namespace-and-enable-capture

