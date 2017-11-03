---
title: "Uzyskiwanie metadanych działania w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak SQL Server działania dotyczącego procedury składowanej umożliwia wywołanie procedury przechowywanej w bazie danych SQL Azure lub usługi Azure SQL Data Warehouse z potoku fabryki danych."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: shlo
ms.openlocfilehash: 99182b2ed91f6d60f499be0078077bf52fe8b366
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Uzyskiwanie metadanych działania w fabryce danych Azure
Działanie GetMetadata umożliwia pobieranie metadanych dowolnych danych z usługi Azure Data Factory. To działanie jest obsługiwana tylko dla fabryki danych w wersji 2. Mogą być używane w następujących scenariuszach:

- Sprawdź poprawność informacji o metadanych danych
- Wyzwalanie potoku, gdy dane są gotowe / dostępne

Następujące funkcje są dostępne w przepływie sterowania:
- Dane wyjściowe działania GetMetadata można używać w wyrażeniach warunkowego do wykonywania sprawdzania poprawności.
- Potok mogą być wyzwalane, gdy warunek jest spełniony za pośrednictwem czy — do pętli

Dane wyjściowe działania GetMetadata przyjmuje zestawu danych jako dane wejściowe wymagane i dostępne jako informacje metadanych danych wyjściowych. Aktualnie obsługiwana jest tylko zestaw danych obiektów blob platformy Azure. Pola metadanych obsługiwane są rozmiar, struktury i lastModified czasu.  

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [dokumentacji V1 fabryki danych](v1/data-factory-introduction.md).


## <a name="syntax"></a>Składnia

### <a name="get-metadata-activity-definition"></a>Pobierz definicję metadanych działania:
W poniższym przykładzie działania GetMetadata zwraca metadane dotyczące danych reprezentowanego przez MyDataset. 

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```
### <a name="dataset-definition"></a>Definicja zestawu danych:

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "Filename": "file.json",
            "format":{
                "type":"JsonFormat"
                "nestedSeperator": ","
            }
        }
    }
}
```

### <a name="output"></a>Dane wyjściowe
```json
{
    "size": 1024,
    "structure": [
        {
            "name": "id",
            "type": "Int64"
        }, 
    ],
    "lastModified": "2016-07-12T00:00:00Z"
}
```

## <a name="type-properties"></a>Właściwości typu
Obecnie GetMetadata działania można pobrać następujące typy informacji metadanych z zestawem danych usługi Azure storage.

Właściwość | Opis | Dozwolone wartości | Wymagane
-------- | ----------- | -------------- | --------
listy pól | Wyświetla listę typów wymaganych informacji o metadanych.  | <ul><li>Rozmiar</li><li>Struktura</li><li>Ostatnia modyfikacja</li></ul> |    Nie<br/>W przypadku braku działania zwraca wszystkie informacje o 3 obsługiwanych metadanych. 
Zestaw danych | Zestaw danych odwołania, których działanie metadanych ma być pobrana przez działanie GetMetadata. <br/><br/>Typ aktualnie obsługiwany zestaw danych jest obiektów Blob platformy Azure. Dostępne są następujące dwie właściwości sub: <ul><li><b>nazwę odwołania</b>: odwołanie do istniejącego zestawu danych obiektów Blob Azure</li><li><b>Typ</b>: ponieważ odwołuje się zestaw danych jest typu "DatasetReference"</li></ul> |    <ul><li>Ciąg</li><li>DatasetReference</li></ul> | Tak

## <a name="next-steps"></a>Następne kroki
Zobacz inne działania przepływu sterowania obsługiwane przez fabrykę danych: 

- [Działanie Execute Pipeline](control-flow-execute-pipeline-activity.md)
- [Dla każdego działania](control-flow-for-each-activity.md)
- [Działanie Lookup](control-flow-lookup-activity.md)
- [Działania w sieci Web](control-flow-web-activity.md)