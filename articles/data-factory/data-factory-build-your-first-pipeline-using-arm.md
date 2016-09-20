<properties
    pageTitle="Tworzenie pierwszej fabryki danych (szablon usługi Resource Manager) | Microsoft Azure"
    description="W tym samouczku przedstawiono tworzenie przykładowego potoku usługi Fabryka danych Azure przy użyciu szablonu usługi Azure Resource Manager."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/01/2016"
    ms.author="spelluru"/>

# Samouczek: tworzenie pierwszej fabryki danych Azure przy użyciu szablonu usługi Azure Resource Manager
> [AZURE.SELECTOR]
- [Omówienie samouczka](data-factory-build-your-first-pipeline.md)
- [Korzystanie z Edytora fabryki danych](data-factory-build-your-first-pipeline-using-editor.md)
- [Korzystanie z programu PowerShell](data-factory-build-your-first-pipeline-using-powershell.md)
- [Korzystanie z programu Visual Studio](data-factory-build-your-first-pipeline-using-vs.md)
- [Korzystanie z szablonu usługi Resource Manager](data-factory-build-your-first-pipeline-using-arm.md)
- [Korzystanie z interfejsu API REST](data-factory-build-your-first-pipeline-using-rest-api.md)


W tym artykule opisano, jak używać szablonu usługi Azure Resource Manager do tworzenia pierwszej fabryki danych Azure. 


## Wymagania wstępne
Oprócz wymagań wstępnych wymienionych w artykule Omówienie samouczka wymagane jest:

- Przeczytanie artykułu [Omówienie samouczka](data-factory-build-your-first-pipeline.md) oraz wykonanie kroków wymagań wstępnych. 
- **Zainstalowanie programu Azure PowerShell**. Postępuj zgodnie z instrukcjami w artykule [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Instalowanie i konfigurowanie programu Azure PowerShell), aby zainstalować najnowszą wersję programu Azure PowerShell na komputerze.
- Ten artykuł nie zawiera omówienia koncepcyjnego usługi Fabryka danych Azure. Szczegółowe omówienie tej usługi znajduje się w artykule [Introduction to Azure Data Factory](data-factory-introduction.md) (Wprowadzenie do usługi Fabryka danych Azure). 
- Artykuł [Authoring Azure Resource Manager Templates](../resource-group-authoring-templates.md) (Tworzenie szablonów usługi Azure Resource Manager) zawiera informacje dotyczące szablonów usługi Azure Resource Manager. 

> [AZURE.IMPORTANT]
> Aby można było wykonać instrukcje przedstawione w tym artykule, wykonaj kroki wymagań wstępnych w artykule [Omówienie samouczka](data-factory-build-your-first-pipeline.md). 

## Tworzenie szablonu usługi Resource Manager

Utwórz plik JSON o nazwie **ADFTutorialARM.json** w folderze **C:\ADFGetStarted** o następującej zawartości: 

Ten szablon umożliwia tworzenie następujących jednostek usługi Fabryka danych.

1. **Fabryka danych** o nazwie **TutorialDataFactoryARM**. Fabryka danych może obejmować jeden lub wiele potoków. Potok może obejmować jedno lub wiele działań. Na przykład działanie kopiowania w celu kopiowania danych ze źródła do docelowego magazynu danych oraz działanie programu Hive w usłudze HDInsight w celu uruchamiania skryptu programu Hive służącego do przekształcania danych wejściowych w dane wyjściowe produktu. 
2. Dwie **połączone usługi**: **StorageLinkedService** i **HDInsightOnDemandLinkedService**. Te połączone usługi łączą konto usługi Azure Storage oraz klaster usługi Azure HDInsight na żądanie z fabryką danych. Konto usługi Azure Storage będzie przechowywać dane wejściowe i wyjściowe dla potoku w tym przykładzie. Połączona usługa HDInsight służy do uruchamiania skryptu programu Hive określonego w działaniu potoku w tym przykładzie. Trzeba zidentyfikować usługi magazynu danych/usługi obliczeniowe, które są używane w tym scenariuszu oraz połączyć te usługi z fabryką danych przez utworzenie połączonych usług. 
3. Dwa **zestawy danych** (wejściowych/wyjściowych): **AzureBlobInput** oraz **AzureBlobOutput**. Te zestawy danych reprezentują dane wejściowe i wyjściowe do przetwarzania Hive. Te zestawy danych dotyczą elementu **StorageLinkedService** utworzonego wcześniej w ramach tego samouczka. Połączona usługa wskazuje konto usługi Azure Storage, a zestawy danych określają kontener, folder i nazwę pliku w magazynie, w którym przechowywane są dane wejściowe i wyjściowe.   

Kliknij kartę **Korzystanie z Edytora fabryki danych**, aby przejść do artykułu zawierającego szczegóły na temat właściwości JSON użytych w tym szablonie.

> [AZURE.IMPORTANT] Zmień wartości zmiennych **storageAccountName** i **storageAccountKey**. Zmień również parametr **dataFactoryName**, ponieważ nazwa musi być unikatowa.


    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "TutorialDataFactoryARM",
            "storageAccountName":  "<AZURE STORAGE ACCOUNT NAME>" ,
            "storageAccountKey":  "<AZURE STORAGE ACCOUNT KEY>",
            "apiVersion": "2015-10-01",
            "storageLinkedServiceName": "AzureStorageLinkedService",
            "hdInsightOnDemandLinkedServiceName": "HDInsightOnDemandLinkedService",
            "blobInputDataset": "AzureBlobInput",
            "blobOutputDataset": "AzureBlobOutput",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "westus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "linkedservices",
                        "name": "[variables('storageLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "AzureStorage",
                            "typeProperties": {
                                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',variables('storageAccountName'),';AccountKey=',variables('storageAccountKey'))]"
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedservices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "linkedservices",
                        "name": "[variables('hdInsightOnDemandLinkedServiceName')]",
                        "apiVersion": "[variables('apiVersion')]",
                        "properties": {
                            "type": "HDInsightOnDemand",
                            "typeProperties": {
                                "clusterSize": 4,
                                "version":  "3.2",
                                "timeToLive": "00:05:00",
                                "osType": "windows",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                            }
                        }
                    },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "datasets",
                        "name": "[variables('blobInputDataset')]",
                        "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "type": "AzureBlob",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                                "typeProperties": {
                                    "fileName": "input.log",
                                    "folderPath": "adfgetstarted/inputdata",
                                    "format": {
                                        "type": "TextFormat",
                                        "columnDelimiter": ","
                                    }
                                },
                                "availability": {
                                    "frequency": "Month",
                                    "interval": 1
                                },
                                "external": true,
                                "policy": {}
                            }
                        },
                    {
                        "dependsOn": [
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                            "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]"
                        ],
                        "type": "datasets",
                        "name": "[variables('blobOutputDataset')]",
                        "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "published": false,
                                "type": "AzureBlob",
                                "linkedServiceName": "[variables('storageLinkedServiceName')]",
                                "typeProperties": {
                                    "folderPath": "adfgetstarted/partitioneddata",
                                    "format": {
                                        "type": "TextFormat",
                                        "columnDelimiter": ","
                                    }
                                },
                                "availability": {
                                    "frequency": "Month",
                                    "interval": 1
                                }
                            }
                        },
                        {
                            "dependsOn": [
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('storageLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/linkedServices/', variables('hdInsightOnDemandLinkedServiceName'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/datasets/', variables('blobInputDataset'))]",
                                "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'), '/datasets/', variables('blobOutputDataset'))]"
                            ],
                            "type": "datapipelines",
                            "name": "[variables('dataFactoryName')]",
                            "apiVersion": "[variables('apiVersion')]",
                            "properties": {
                                "description": "My first Azure Data Factory pipeline",
                                "activities": [
                                    {
                                        "type": "HDInsightHive",
                                        "typeProperties": {
                                            "scriptPath": "adfgetstarted/script/partitionweblogs.hql",
                                            "scriptLinkedService": "[variables('storageLinkedServiceName')]",
                                            "defines": {
                                                "inputtable": "[concat('wasb://adfgetstarted@', variables('storageAccountName'), '.blob.core.windows.net/inputdata')]",
                                                "partitionedtable": "[concat('wasb://adfgetstarted@', variables('storageAccountName'), '.blob.core.windows.net/partitioneddata')]"
                                            }
                                        },
                                        "inputs": [
                                            {
                                                "name": "AzureBlobInput"
                                            }
                                        ],
                                        "outputs": [
                                            {
                                                "name": "AzureBlobOutput"
                                            }
                                        ],
                                        "policy": {
                                            "concurrency": 1,
                                            "retry": 3
                                        },
                                        "scheduler": {
                                            "frequency": "Month",
                                            "interval": 1
                                        },
                                        "name": "RunSampleHiveActivity",
                                        "linkedServiceName": "HDInsightOnDemandLinkedService"
                                    }
                                ],
                                "start": "2016-04-01T00:00:00Z",
                                "end": "2016-04-02T00:00:00Z",
                                "isPaused": false
                            }
                        }
                ]
            }
        ]
    }

Kliknij kartę **Korzystanie z Edytora fabryki danych**, aby przejść do artykułu zawierającego szczegóły na temat właściwości JSON użytych w tym szablonie.

Pamiętaj o następujących kwestiach: 

- Usługa Fabryka danych tworzy klaster usługi HDInsight **oparty na systemie Windows** za pomocą powyższego kodu JSON. Możliwe jest również utworzenie klastra usługi HDInsight **opartego na systemie Linux**. Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie). 
- Możesz użyć **własnego klastra usługi HDInsight** zamiast klastra usługi HDInsight na żądanie. Szczegółowe informacje znajdują się w artykule [HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-linked-service) (Połączona usługa HDInsight).
- Klaster usługi HDInsight tworzy **kontener domyślny** w magazynie obiektów blob określonym w kodzie JSON (**linkedServiceName**). Usługa HDInsight nie powoduje usunięcia tego kontenera w przypadku usunięcia klastra. To zachowanie jest celowe. W przypadku połączonej usługi HDInsight na żądanie klaster usługi HDInsight jest tworzony za każdym razem, gdy trzeba przetworzyć wycinek — o ile w tym momencie nie istnieje aktywny klaster (**timeToLive**) — i zostaje usunięty po zakończeniu przetwarzania.

    Po przetworzeniu większej liczby wycinków w usłudze Azure Blob Storage będzie widocznych wiele kontenerów. Jeśli nie są potrzebne do rozwiązywania problemów z zadaniami, można je usunąć, aby zmniejszyć koszt przechowywania. Nazwy tych kontenerów są zgodne ze wzorcem: „adf**twojanazwafabrykidanych**-**nazwapołączonejusługi**-znacznikdatygodziny”. Aby usunąć kontenery z magazynu obiektów blob Azure, użyj takich narzędzi, jak [Microsoft Storage Explorer](http://storageexplorer.com/).

Szczegółowe informacje znajdują się w artykule [On-demand HDInsight Linked Service](data-factory-compute-linked-services.md#azure-hdinsight-on-demand-linked-service) (Połączona usługa HDInsight na żądanie).

> [AZURE.NOTE] Inny przykład szablonu usługi Resource Manager do tworzenia fabryki danych Azure znajduje się w witrynie [Github](https://github.com/Azure/azure-quickstart-templates/blob/master/101-data-factory-blob-to-sql/azuredeploy.json).  

## Tworzenie fabryki danych

1. Uruchom program **Azure PowerShell** i uruchom następujące polecenie. 
    - Uruchom polecenie **Login-AzureRmAccount** i wprowadź nazwę użytkownika oraz hasło, których używasz do logowania się w witrynie Azure Portal.  
    - Uruchom następujące polecenie, aby wybrać subskrypcję, w ramach której chcesz utworzyć fabrykę danych.
            Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
1. Uruchom następujące polecenie, aby wdrożyć jednostki usługi Data Factory przy użyciu szablonu usługi Resource Manager utworzonego w kroku 1. 

        New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFTutorialARM.json

## Monitorowanie potoku
 
1.  Po zalogowaniu się do witryny [Azure Portal](https://portal.azure.com/) kliknij przycisk **Przeglądaj** i wybierz opcję **Fabryki danych**.
        ![Przeglądaj -> Fabryki danych](./media/data-factory-build-your-first-pipeline-using-arm/BrowseDataFactories.png)
2.  W bloku **Fabryki danych** kliknij utworzoną przez siebie fabrykę danych (**TutorialFactoryARM**).   
2.  W bloku **Fabryka danych** dla swojej fabryki danych kliknij opcję **Diagram**.
        ![Kafelek diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramTile.png)
4.  Na stronie **Widok diagramu** zostanie wyświetlony przegląd potoków i zestawów danych używanych w tym samouczku.
    
    ![Widok diagramu](./media/data-factory-build-your-first-pipeline-using-arm/DiagramView.png) 
8. Na stronie Widok diagramu kliknij dwukrotnie zestaw danych **AzureBlobOutput**. Zostanie wyświetlony wycinek, który jest obecnie przetwarzany.

    ![Zestaw danych](./media/data-factory-build-your-first-pipeline-using-arm/AzureBlobOutput.png)
9. Po zakończeniu przetwarzania wycinek zostanie wyświetlony w stanie **Gotowe**. Tworzenie klastra usługi HDInsight na żądanie zwykle trwa trochę czasu (około 20 minut). 

    ![Zestaw danych](./media/data-factory-build-your-first-pipeline-using-arm/SliceReady.png) 
10. Gdy wycinek będzie w stanie **Gotowe**, sprawdź folder **partitioneddata** w kontenerze **adfgetstarted** w magazynie obiektów blob pod kątem danych wyjściowych.  

Zobacz artykuł [Monitor datasets and pipeline](data-factory-monitor-manage-pipelines.md) (Monitorowanie zestawów danych i potoku), aby uzyskać instrukcje dotyczące korzystania z bloków w witrynie Azure Portal w celu monitorowania potoku i zestawów danych utworzonych przez siebie w ramach tego samouczka.

Do monitorowania potoków danych możesz też użyć aplikacji Monitorowanie i zarządzanie. Szczegółowe informacje dotyczące korzystania z aplikacji znajdują się w artykule [Monitor and manage Azure Data Factory pipelines using Monitoring App](data-factory-monitor-manage-app.md) (Monitorowanie potoków usługi Fabryka danych Azure oraz zarządzanie nimi za pomocą aplikacji do monitorowania). 

> [AZURE.IMPORTANT] Po pomyślnym przetworzeniu wycinka plik wejściowy zostanie usunięty. Tak więc, jeśli chcesz ponownie uruchomić wycinek lub ponownie wykonać instrukcje z tego samouczka, przekaż plik wejściowy (input.log) do folderu inputdata kontenera adfgetstarted.

## Szablon usługi Resource Manager do tworzenia bramy
Poniżej przedstawiono przykładowy szablon usługi Resource Manager do tworzenia logicznej bramy w tle. Musisz zainstalować bramę na komputerze lokalnym lub maszynie wirtualnej IaaS platformy Azure i zarejestrować bramę w usłudze Data Factory przy użyciu klucza. Szczegółowe informacje znajdują się w artykule [Move data between on-premises and cloud](data-factory-move-data-between-onprem-and-cloud.md) (Przenoszenie danych między komputerem lokalnym i chmurą).

    {
        "contentVersion": "1.0.0.0",
        "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "parameters": {
        },
        "variables": {
            "dataFactoryName":  "GatewayUsingArmDF",
            "apiVersion": "2015-10-01",
            "singleQuote": "'"
        },
        "resources": [
            {
                "name": "[variables('dataFactoryName')]",
                "apiVersion": "[variables('apiVersion')]",
                "type": "Microsoft.DataFactory/datafactories",
                "location": "eastus",
                "resources": [
                    {
                        "dependsOn": [ "[concat('Microsoft.DataFactory/dataFactories/', variables('dataFactoryName'))]" ],
                        "type": "gateways",
                        "apiVersion": "[variables('apiVersion')]",
                        "name": "GatewayUsingARM",
                        "properties": {
                            "description": "my gateway"
                        }
                    }            
                ]
            }
        ]
    }

Ten szablon służy do tworzenia fabryki danych o nazwie GatewayUsingArmDF z bramą o nazwie: GatewayUsingARM. 

## Zobacz też
| Temat | Opis |
| :---- | :---- |
| [Działania przekształcania danych](data-factory-data-transformation-activities.md) | Ten artykuł zawiera listę działań przekształcania danych (takich jak przekształcenie programu Hive w usłudze HDInsight używane w tym samouczku) obsługiwanych w usłudze Fabryka danych Azure. |
| [Planowanie i wykonywanie](data-factory-scheduling-and-execution.md) | W tym artykule wyjaśniono aspekty planowania i wykonywania modelu aplikacji usługi Fabryka danych Azure. |
| [Potoki](data-factory-create-pipelines.md) | Ten artykuł ułatwia zapoznanie się z potokami i działaniami w usłudze Azure Data Factory oraz ze sposobem konstruowania za ich pomocą przepływów pracy typu end-to-end opartych na danych na potrzeby scenariusza lub firmy. |
| [Zestawy danych](data-factory-create-datasets.md) | Ten artykuł ułatwia zapoznanie się z zestawami danych w usłudze Azure Data Factory.
| [Monitorowanie potoków i zarządzanie nimi za pomocą aplikacji do monitorowania](data-factory-monitor-manage-app.md) | Ten artykuł zawiera instrukcje dotyczące monitorowania i debugowania potoków oraz zarządzania nimi przy użyciu aplikacji do monitorowania i zarządzania. 

  




<!--HONumber=sep16_HO1-->


