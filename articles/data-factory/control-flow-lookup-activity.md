---
title: "Działania wyszukiwania w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak działanie wyszukiwania służy do odszukania wartości ze źródła zewnętrznego. Do tych danych wyjściowych mogą także odwoływać się kolejne działania."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: spelluru
ms.openlocfilehash: d498705ef7f714b4f15b8d2722053bf3081b5045
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>Działania wyszukiwania w fabryce danych Azure
Działanie Lookup może być używane do odczytywania lub wyszukiwania rekordu/nazwy tabeli/wartości z dowolnego źródła zewnętrznego. Do tych danych wyjściowych mogą także odwoływać się kolejne działania. 

Następujące źródła danych są obecnie obsługiwane dla wyszukiwania:
- Plik JSON w obiekcie Blob Azure
- Plik JSON lokalnej
- Baza danych SQL Azure (JSON dane skonwertowane z zapytania)
- Magazyn tabel Azure (JSON dane skonwertowane z zapytania)

Działania wyszukiwania jest przydatne, gdy chcesz dynamicznie pobrać listy plików / rekordów/tabel z pliku konfiguracji lub źródła danych. Dane wyjściowe działania dalsze można przez inne działania do wykonywania określonych przetwarzania jedynie tych elementów.

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [dokumentacji V1 fabryki danych](v1/data-factory-introduction.md).


## <a name="example"></a>Przykład
W tym przykładzie działanie kopiowania kopiuje dane z tabeli SQL w bazie danych Azure SQL do magazynu obiektów Blob Azure. Nazwa tabeli SQL jest przechowywana w pliku JSON w magazynie obiektów Blob. Działania wyszukiwania wyszukuje nazwy tabeli w czasie wykonywania. Takie podejście umożliwia JSON można zmodyfikować dynamicznie bez ponownego wdrożenia potoki/zestawów danych. 

### <a name="pipeline"></a>Potok
Ten potok zawiera dwa działania: **odszukać** i **kopiowania**. 

- Działania wyszukiwania jest skonfigurowany do używania LookupDataset, która odwołuje się do lokalizacji w magazynie obiektów Blob Azure. Działania wyszukiwania odczytuje nazwę tabeli SQL z pliku JSON w tej lokalizacji. 
- Działanie kopiowania korzysta z danych wyjściowych działania wyszukiwania (nazwa tabeli SQL). Właściwość tableName w źródłowym zestawie danych (SourceDataset) jest skonfigurowany do używania dane wyjściowe działania wyszukiwania. Działanie kopiowania kopiuje dane z tabeli SQL do lokalizacji w magazynie obiektów Blob Azure, który jest określony przez SinkDataset. 


```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
                    }
                 ],
                "inputs": [ 
                    { 
                        "referenceName": "SourceDataset", 
                        "type": "DatasetReference" 
                    } 
                ],
                "outputs": [ 
                    { 
                        "referenceName": "SinkDataset", 
                        "type": "DatasetReference" 
                    } 
                ]
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Zestaw danych wyszukiwania
Zestaw danych wyszukiwania odwołuje się do pliku sourcetable.json w folderze wyszukiwania w usłudze Azure Storage, określony przez AzureStorageLinkedService. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-the-copy-activity"></a>Źródło danych dla działania kopiowania
Dane wyjściowe działania wyszukiwania, czyli nazwa tabeli SQL korzysta z zestawu źródła danych. Działanie kopiowania kopiuje dane z tej tabeli SQL do lokalizacji w magazynie obiektów Blob Azure określonym przez obiekt sink zestawu danych. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-the-copy-activity"></a>Obiekt sink zestawu danych dla aktywności kopiowania
Działanie kopiowania kopiuje dane z tabeli SQL filebylookup.csv pliku w folderze csv w magazynie Azure, określony przez AzureStorageLinkedService. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
            "format": {
                "type": "TextFormat"                                                                    
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="azure-storage-linked-service"></a>Połączona usługa Azure Storage
To konto magazynu zawiera plik JSON nazw tabel SQL. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Połączona usługa Azure SQL Database
Ta baza danych Azure SQL zawiera dane, które ma zostać skopiowany do magazynu obiektów blob. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": {
                "value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="sourcetablejson"></a>SourceTable.JSON

#### <a name="set-of-objects"></a>Zestaw obiektów

```json
{
  "Id": "1",
  "tableName": "Table1",
}
{
   "Id": "2",
  "tableName": "Table2",
}
```
#### <a name="array-of-objects"></a>Tablica obiektów

```json
[ 
    {
        "Id": "1",
          "tableName": "Table1",
    }
    {
        "Id": "2",
        "tableName": "Table2",
    }
]
```



## <a name="type-properties"></a>Właściwości typu
Nazwa | Opis | Typ | Wymagane
---- | ----------- | ---- | --------
Zestaw danych | Atrybut zestawu danych jest zapewnienie odwołania do zestawu danych do wyszukiwania. Obecnie typy obsługiwanych zestawu danych to:<ul><li>FileShareDataset</li><li>AzureBlobDataset</li><li>AzureSqlTableDataset</li><li>AzureTableDataset</li> | Para klucza i wartości | Tak
źródło | Właściwości źródła specyficzne dla zestawu danych, taki jak źródło działania kopiowania | Para klucza i wartości | Nie
firstRowOnly | Zwraca pierwszy wiersz lub wszystkie wiersze. | Wartość logiczna | Nie

## <a name="next-steps"></a>Następne kroki
Zobacz inne działania przepływu sterowania obsługiwane przez fabrykę danych: 

- [Działanie Execute Pipeline](control-flow-execute-pipeline-activity.md)
- [Dla każdego działania](control-flow-for-each-activity.md)
- [Działanie GetMetadata](control-flow-get-metadata-activity.md)
- [Działania w sieci Web](control-flow-web-activity.md)
