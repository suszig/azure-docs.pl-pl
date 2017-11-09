---
title: "Poczekaj działania w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Działanie oczekiwania wstrzymuje wykonywanie potoku w określonym okresie."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2017
ms.author: shlo
ms.openlocfilehash: 3ff06ea07fcf5e391783575adf9dd5d99255eced
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/09/2017
---
# <a name="wait-activity-in-azure-data-factory"></a>Poczekaj na działanie w fabryce danych Azure
Gdy używasz działanie oczekiwania w potoku potoku czeka na określonym przedziale czasu, przed kontynuowaniem wykonywania kolejnych działań. 

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [dokumentacji V1 fabryki danych](v1/data-factory-introduction.md).

## <a name="syntax"></a>Składnia

```json
{
    "name": "MyWaitActivity",
    "type": "Wait",
    "typeProperties": {
        "waitTimeInSeconds": 1
    }
}

```

## <a name="type-properties"></a>Właściwości typu

Właściwość | Opis | Dozwolone wartości | Wymagane
-------- | ----------- | -------------- | --------
name | Nazwa `Wait` działania. | Ciąg | Tak
type | Należy wybrać opcję **oczekiwania**. | Ciąg | Tak
waitTimeInSeconds | Liczba sekund, przez potok czeka przed kontynuowaniem przetwarzania. | Liczba całkowita | Tak

## <a name="example"></a>Przykład

> [!NOTE]
> Ta sekcja zawiera definicje JSON i przykładowe polecenia programu PowerShell do uruchamiania potoku. Przewodnik krok po kroku instrukcje, aby utworzyć potok fabryki danych przy użyciu programu Azure PowerShell i JSON definicji, zobacz [samouczek: tworzenie fabryki danych przy użyciu programu Azure PowerShell](quickstart-create-data-factory-powershell.md).

### <a name="pipeline-with-wait-activity"></a>W potoku z działania oczekiwania
W tym przykładzie potoku ma dwa działania: **do momentu** i **oczekiwania**. Poczekaj chwilę skonfigurowano działanie oczekiwania. Potok uruchamia działanie sieci Web w pętli z jednej sekundzie czas między każdym uruchomieniu oczekiwania. 

```json
{
    "name": "DoUntilPipeline",
    "properties": {
        "activities": [
            {
                "type": "Until",
                "typeProperties": {
                    "expression": {
                        "value": "@equals('Failed', coalesce(body('MyUnauthenticatedActivity')?.status, actions('MyUnauthenticatedActivity')?.status, 'null'))",
                        "type": "Expression"
                    },
                    "timeout": "00:00:01",
                    "activities": [
                        {
                            "name": "MyUnauthenticatedActivity",
                            "type": "WebActivity",
                            "typeProperties": {
                                "method": "get",
                                "url": "https://www.fake.com/",
                                "headers": {
                                    "Content-Type": "application/json"
                                }
                            },
                            "dependsOn": [
                                {
                                    "activity": "MyWaitActivity",
                                    "dependencyConditions": [ "Succeeded" ]
                                }
                            ]
                        },
                        {
                            "type": "Wait",
                            "typeProperties": {
                                "waitTimeInSeconds": 1
                            },
                            "name": "MyWaitActivity"
                        }
                    ]
                },
                "name": "MyUntilActivity"
            }
        ]
    }
}

```

## <a name="next-steps"></a>Następne kroki
Zobacz inne działania przepływu sterowania obsługiwane przez fabrykę danych: 

- [Jeśli warunek działania](control-flow-if-condition-activity.md)
- [Działanie Execute Pipeline](control-flow-execute-pipeline-activity.md)
- [Dla każdego działania](control-flow-for-each-activity.md)
- [Działanie GetMetadata](control-flow-get-metadata-activity.md)
- [Działanie Lookup](control-flow-lookup-activity.md)
- [Działania w sieci Web](control-flow-web-activity.md)
- [Do działania](control-flow-until-activity.md)