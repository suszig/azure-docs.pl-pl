---
redirect_url: /azure/sql-data-warehouse/sql-data-warehouse-load-with-data-factory
title: "Ładowanie danych przy użyciu usługi Azure Data Factory | Microsoft Docs"
description: "Dowiedz się, jak załadować dane przy użyciu usługi Azure Data Factory"
services: sql-data-warehouse
documentationcenter: NA
author: twounder
manager: jhubbard
editor: 
tags: azure-sql-data-warehouse
ms.assetid: ac7ddaa7-a3a5-4e15-b3cf-c696d2d105df
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: mausher;barbkess
translationtype: Human Translation
ms.sourcegitcommit: c0e2324a2b2e6294df6e502f2e7a0ae36ff94158
ms.openlocfilehash: 2f0aa3ab44813529525108758785ea3ceb65311b


---
# <a name="load-data-with-azure-data-factory"></a>Ładowanie danych przy użyciu usługi Azure Data Factory
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)  
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)  
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)  
> * [BCP](sql-data-warehouse-load-with-bcp.md)  
> 
> 

W tym samouczku przedstawiono, jak utworzyć potok w usłudze Azure Data Factory, aby przenieść dane z rozszerzenia Azure Storage Blob do usługi Azure SQL Data Warehouse. Następujące kroki obejmują:

* Konfigurowanie przykładowych danych w rozszerzeniu Azure Storage Blob.
* Łączenie zasobów z usługą Azure Data Factory.
* Tworzenie potoku do przenoszenia danych z obiektów blob magazynu do usługi SQL Data Warehouse.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Loading-Azure-SQL-Data-Warehouse-with-Azure-Data-Factory/player]
> 
> 

## <a name="before-you-begin"></a>Przed rozpoczęciem
Aby zapoznać się z usługą Azure Data Factory, zobacz artykuł [Wprowadzenie do usługi Azure Data Factory][Introduction to Azure Data Factory].

### <a name="create-or-identify-resources"></a>Tworzenie lub identyfikowanie zasobów
Przed rozpoczęciem tego samouczka trzeba mieć następujące zasoby:

* **Azure Storage Blob**: w tym samouczku używamy rozszerzenia Azure Storage Blob jako źródła danych dla potoku usługi Azure Data Factory, zatem potrzebny będzie dostęp do takiego obiektu w celu przechowywania przykładowych danych. Jeśli nie masz jeszcze takiego obiektu, zapoznaj się z artykułem [Tworzenie konta magazynu][Create a storage account].
* Usługa **SQL Data Warehouse**: w tym samouczku przenosimy dane z rozszerzenia Azure Storage Blob do usługi SQL Data Warehouse, dlatego potrzebujemy mieć magazyn danych online z załadowanymi danymi przykładowymi AdventureWorksDW. Jeśli nie masz jeszcze magazynu danych, dowiedz się, jak [aprowizować magazyn][Create a SQL Data Warehouse]. Jeśli masz magazyn danych, ale bez przykładowych danych, możesz [załadować je ręcznie][Load sample data into SQL Data Warehouse].
* **Azure Data Factory**: usługa Azure Data Factory uzupełnia rzeczywiste obciążenie, więc potrzebna jest taka, której będzie można użyć do utworzenia potoku przenoszenia danych. Jeśli jeszcze jej nie masz, informacje na temat jej tworzenia można znaleźć w kroku 1 w temacie [Rozpoczynanie pracy z usługą Azure Data Factory (Edytor usługi Data Factory)][Get started with Azure Data Factory (Data Factory Editor)].
* **AZCopy**: program AZCopy jest potrzebny do skopiowania przykładowych danych z lokalnego klienta do rozszerzenia Azure Storage Blob. Aby uzyskać instrukcje instalacji, zobacz [dokumentację programu AZCopy][AZCopy documentation].

## <a name="step-1-copy-sample-data-to-azure-storage-blob"></a>Krok 1: kopiowanie przykładowych danych do rozszerzenia Azure Storage Blob
Gdy wszystkie elementy są gotowe, możesz przystąpić do kopiowania przykładowych danych do rozszerzenia Azure Storage Blob.

1. [Pobierz przykładowe dane][Download sample data]. Dane te uzupełnią przykładowe dane AdventureWorksDW dodatkowymi danymi sprzedaży z trzech lat.
2. Użyj tego polecenia programu AZCopy, aby skopiować dane z trzech lat do rozszerzenia Azure Storage Blob.
   
    ````
    AzCopy /Source:<Sample Data Location>  /Dest:https://<storage account>.blob.core.windows.net/<container name> /DestKey:<storage key> /Pattern:FactInternetSales.csv
    ````

## <a name="step-2-connect-resources-to-azure-data-factory"></a>Krok 2: łączenie zasobów z usługą Azure Data Factory
Teraz, gdy dane znajdują się na miejscu, możemy utworzyć potok usługi Azure Data Factory, aby przenieść dane z usługi Azure Blob Storage do usługi SQL Data Warehouse.

Aby rozpocząć, otwórz witrynę [Azure Portal][Azure portal] i wybierz fabrykę danych z menu po lewej stronie.

### <a name="step-21-create-linked-service"></a>Krok 2.1: tworzenie usługi połączonej
Połącz swoje konto usługi Azure Storage i usługę SQL Data Warehouse z fabryką danych.  

1. Najpierw rozpocznij proces rejestracji. W tym celu kliknij sekcję „Połączone usługi” w fabryce danych, a następnie kliknij pozycję „Nowy magazyn danych”. Wybierz nazwę, pod którą ma być zarejestrowany magazyn Azure, wybierz Azure Storage jako typ, a następnie wprowadź nazwę konta i klucz do niego.
2. Aby zarejestrować usługę SQL Data Warehouse, przejdź do sekcji „Utwórz i wdróż”, wybierz opcję „Nowy magazyn danych”, a następnie „Azure SQL Data Warehouse”. Skopiuj dane i wklej w tym szablonie, a następnie uzupełnij go swoimi danymi.
   
    ```JSON
    {
        "name": "<Linked Service Name>",
        "properties": {
            "description": "",
            "type": "AzureSqlDW",
            "typeProperties": {
                 "connectionString": "Data Source=tcp:<server name>.database.windows.net,1433;Initial Catalog=<server name>;Integrated Security=False;User ID=<user>@<servername>;Password=<password>;Connect Timeout=30;Encrypt=True"
             }
        }
    }
    ```

### <a name="step-22-define-the-dataset"></a>Krok 2.2: definiowanie zestawu danych
Po utworzeniu połączonych usług trzeba zdefiniować zestawy danych.  W tym przypadku oznacza to zdefiniowanie struktury danych, które są przenoszone z magazynu do magazynu danych.  Więcej informacji na temat tworzenia

1. Uruchom ten proces, przechodząc do sekcji „Utwórz i wdróż” w fabryce danych.
2. Kliknij pozycję „Nowy zestaw danych”, a następnie pozycję „Azure Blob Storage”, aby połączyć magazyn z fabryką danych.  Możesz użyć poniższego skryptu, aby zdefiniować dane w usłudze Azure Blob Storage:
   
    ```JSON
    {
        "name": "<Dataset Name>",
        "properties": {
            "type": "AzureBlob",
            "linkedServiceName": "<linked storage name>",
            "typeProperties": {
                "folderPath": "<containter name>",
                "fileName": "FactInternetSales.csv",
                "format": {
                "type": "TextFormat",
                "columnDelimiter": ",",
                "rowDelimiter": "\n"
                }
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval": 1
            },
            "policy": {
                "externalData": {
                    "retryInterval": "00:01:00",
                    "retryTimeout": "00:10:00",
                    "maximumRetry": 3
                }
            }
        }
    }
    ```
3. Teraz zdefiniujemy także zestaw danych dla usługi SQL Data Warehouse. Rozpoczynamy w taki sam sposób, klikając pozycję „Nowy zestaw danych”, a następnie „Azure SQL Data Warehouse”.
   
    ```JSON
    {
        "name": "DWDataset",
        "properties": {
            "type": "AzureSqlDWTable",
            "linkedServiceName": "AzureSqlDWLinkedService",
            "typeProperties": {
                "tableName": "FactInternetSales"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```

## <a name="step-3-create-and-run-your-pipeline"></a>Krok 3: tworzenie i uruchamianie potoku
Na koniec skonfigurujemy i uruchomimy potok w usłudze Azure Data Factory.  Ta operacja spowoduje rzeczywiste przeniesienie danych.  Pełen przegląd operacji, które można wykonać przy użyciu usługi SQL Data Warehouse i usługi Azure Data Factory, znajdziesz [tutaj][Move data to and from Azure SQL Data Warehouse using Azure Data Factory].

W sekcji „Utwórz i wdróż” kliknij pozycję „Więcej poleceń”, a następnie pozycję „Nowy potok”.  Po utworzeniu potoku można użyć poniższego kodu, aby przenieść dane do magazynu danych:

```JSON
{
    "name": "<Pipeline Name>",
    "properties": {
        "description": "<Description>",
        "activities": [
          {
            "type": "Copy",
            "typeProperties": {
                "source": {
                    "type": "BlobSource",
                    "skipHeaderLineCount": 1
                },
                "sink": {
                    "type": "SqlDWSink",
                    "writeBatchSize": 0,
                    "writeBatchTimeout": "00:00:10"
                }
            },
            "inputs": [
              {
                "name": "<Storage Dataset>"
              }
            ],
            "outputs": [
              {
                "name": "<Data Warehouse Dataset>"
              }
            ],
            "policy": {
                "timeout": "01:00:00",
                "concurrency": 1
            },
            "scheduler": {
                "frequency": "Hour",
                "interval": 1
            },
            "name": "Sample Copy",
            "description": "Copy Activity"
          }
        ],
        "start": "<Date YYYY-MM-DD>",
        "end": "<Date YYYY-MM-DD>",
        "isPaused": false
    }
}
```

## <a name="next-steps"></a>Następne kroki
Aby dowiedzieć się więcej, zapoznaj się z tymi artykułami:

* [Ścieżka szkoleniowa usługi Azure Data Factory][Azure Data Factory learning path].
* [Łącznik usługi Azure SQL Data Warehouse][Azure SQL Data Warehouse Connector]. Jest to podstawy temat referencyjny dotyczący używania usługi Azure Data Factory z usługą Azure SQL Data Warehouse.

Te tematy zawierają szczegółowe informacje na temat usługi Azure Data Factory. Omówiono w nich usługi Azure SQL Database lub HDInsight, ale informacje dotyczą także usługi SQL Data Warehouse.

* [Samouczek: rozpoczynanie pracy z usługą Azure Data Factory][Tutorial: Get started with Azure Data Factory] Jest to podstawowy samouczek dotyczący przetwarzania danych przy użyciu usługi Azure Data Factory. W tym samouczku opisano tworzenie pierwszego potoku, który używa usługi HDInsight do transformacji i analizy dzienników sieci Web co miesiąc. Zauważ, że w tym samouczku nie ma czynności kopiowania.
* [Samouczek: kopiowanie danych z usługi Azure Storage Blob do usługi Azure SQL Database][Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]. W tym samouczku opisano tworzenie potoku w usłudze Azure Data Factory w celu skopiowania danych z rozszerzenia Azure Storage Blob do usługi Azure SQL Database.

<!--Image references-->

<!--Article references-->
[AZCopy documentation]: ../storage/storage-use-azcopy.md
[Azure SQL Data Warehouse Connector]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[BCP]: sql-data-warehouse-load-with-bcp.md
[Create a SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Create a storage account]: ../storage/storage-create-storage-account.md#create-a-storage-account
[Data Factory]: sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Get started with Azure Data Factory (Data Factory Editor)]: ../data-factory/data-factory-build-your-first-pipeline-using-editor.md
[Introduction to Azure Data Factory]: ../data-factory/data-factory-introduction.md
[Load sample data into SQL Data Warehouse]: sql-data-warehouse-load-sample-databases.md
[Move data to and from Azure SQL Data Warehouse using Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[PolyBase]: sql-data-warehouse-get-started-load-with-polybase.md
[Tutorial: Copy data from Azure Storage Blob to Azure SQL Database]: ../data-factory/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[Tutorial: Get started with Azure Data Factory]: ../data-factory/data-factory-build-your-first-pipeline.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory learning path]: https://azure.microsoft.com/documentation/learning-paths/data-factory
[Azure portal]: https://portal.azure.com
[Download sample data]: https://migrhoststorage.blob.core.windows.net/adfsample/FactInternetSales.csv



<!--HONumber=Jan17_HO5-->


