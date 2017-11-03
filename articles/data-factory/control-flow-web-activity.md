---
title: "Działania w fabryce danych Azure w sieci Web | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak aktywności sieci Web, jeden z działaniami przepływu sterowania obsługiwane przez fabrykę danych umożliwia wywołanie punkt końcowy REST z potoku."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2017
ms.author: shlo
ms.openlocfilehash: b5661dea3a63f6e7e5b67261bc9704061ae9c5b6
ms.sourcegitcommit: c50171c9f28881ed3ac33100c2ea82a17bfedbff
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="web-activity-in-azure-data-factory"></a>Działanie sieci Web w fabryce danych Azure
Działanie WebActivity może być używane do wywoływania niestandardowego punktu końcowego REST z potoku usługi Data Factory. Można przekazywać zestawy danych i połączone usługi do zużycia i dostępu przez działanie. 

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz wersji 1 usługi fabryka danych, która jest ogólnie dostępna (GA), zobacz [dokumentacji dla fabryki danych w wersji 1](v1/data-factory-introduction.md).

## <a name="syntax"></a>Składnia

```json
{  
   "name":"MyWebActivity",
   "type":"WebActivity",
   "typeProperties":{  
      "method":"Post",
      "url":"<URLEndpoint>",
      "headers":{  
         "Content-Type":"application/json"
      },
      "authentication":{  
         "type":"ClientCertificate",  
         "pfx":"****",
         "password":"****"
      },
      "datasets":[  
         {  
            "referenceName":"<ConsumedDatasetName>",
            "type":"DatasetReference",
            "parameters":{  
               ...
            }
         }
      ],
      "linkedServices":[  
         {  
            "referenceName":"<ConsumedLinkedServiceName>",
            "type":"LinkedServiceReference"
         }
      ]
   }
}

```

## <a name="type-properties"></a>Właściwości typu

Właściwość | Opis | Dozwolone wartości | Wymagane
-------- | ----------- | -------------- | --------
name | Nazwa działania sieci web | Ciąg | Tak
type | Należy wybrać opcję **WebActivity**. | Ciąg | Tak
— Metoda | Metoda interfejsu API REST dla docelowego punktu końcowego. | Ciąg. <br/><br/>Obsługiwane typy: "GET", "POST", "PUT" | Tak
adres URL | Docelowy punkt końcowy i ścieżki | Ciąg (lub wyrażenie o wartości resultType ciągu) | Tak
Nagłówki | Nagłówki, które są wysyłane do żądania. Na przykład, aby ustawić język i typ na żądanie: `"headers" : { "Accept-Language": "en-us", "Content-Type": "application/json" }`. | Ciąg (lub wyrażenie o wartości resultType ciągu) | Tak, nagłówek Content-type jest wymagany. `"headers":{ "Content-Type":"application/json"}`
Treści | Reprezentuje ładunek, które są wysyłane do punktu końcowego. Wymagany w przypadku metody POST/PUT.  | Ciąg (lub wyrażenie o wartości resultType ciągu). <br/><br/>Zobacz Schemat ładunku żądania w [schematu ładunku żądania](#request-payload-schema) sekcji. | Nie
Uwierzytelnianie | Metoda uwierzytelniania używana do wywoływania punktu końcowego. Obsługiwane typy to "Basic lub ClientCertificate." Aby uzyskać więcej informacji, zobacz [uwierzytelniania](#authentication) sekcji. Jeśli uwierzytelnianie nie jest wymagane, należy wykluczyć tę właściwość. | Ciąg (lub wyrażenie o wartości resultType ciągu) | Nie
Zbiory danych | Lista zestawów danych jest przekazywana do punktu końcowego. | Tablica odwołań do zestawu danych. Może być pustą tablicę. | Tak
linkedServices | Lista usług połączonych przekazany do punktu końcowego. | Tablica odwołań do połączonej usługi. Może być pustą tablicę. | Tak

> [!NOTE]
> Punkty końcowe REST, które wywołuje aktywności sieci web musi zwracać odpowiedzi typu JSON.

## <a name="authentication"></a>Authentication

### <a name="none"></a>Brak
Jeśli uwierzytelnianie nie jest wymagane, nie ma właściwości "uwierzytelnianie".

### <a name="basic"></a>Podstawowa
Określ nazwę użytkownika i hasło w celu korzystania z uwierzytelniania podstawowego. 

```json
"authentication":{  
   "type":"Basic,
   "username":"****",
   "password":"****"
}
```

### <a name="client-certificate"></a>Certyfikat klienta
Określ algorytmem Base64 zawartość pliku PFX oraz hasło. 

```json
"authentication":{  
   "type":"ClientCertificate",
   "pfx":"****",   
   "password":"****"
}
```
## <a name="request-payload-schema"></a>Schemat ładunku żądania
Korzystając z metody POST/PUT, właściwości body reprezentuje ładunek, które są wysyłane do punktu końcowego. Połączone usługi i zestawy danych można przekazać jako częścią ładunku. Oto schemat ładunku: 

```json
{
    "body": {
        "myMessage": "Sample",
        "datasets": [{
            "name": "MyDataset1",
            "properties": {
                ...
            }
        }],
        "linkedServices": [{
            "name": "MyStorageLinkedService1",
            "properties": {
                ...
            }
        }]
    }
} 
```

## <a name="example"></a>Przykład
W tym przykładzie działanie sieci web w potoku wywołuje punkt końcowy REST. Przekazuje ono Azure połączoną usługą SQL i zestawu danych Azure SQL do punktu końcowego. Punkt końcowy REST używa parametrów połączenia Azure SQL w celu połączenia z serwerem Azure SQL i zwraca nazwę wystąpienia programu SQL server. 

### <a name="pipeline-definition"></a>Definicja potoku

```json
{
    "name": "<MyWebActivityPipeline>",
    "properties": {
        "activities": [
            {
                "name": "<MyWebActivity>",
                "type": "WebActivity",
                "typeProperties": {
                    "method": "Post",
                    "url": "@pipeline().parameters.url",
                    "headers": {
                        "Content-Type": "application/json"
                    },
                    "authentication": {
                        "type": "ClientCertificate",
                        "pfx": "*****",
                        "password": "*****"
                    },
                    "datasets": [
                        {
                            "referenceName": "MySQLDataset",
                            "type": "DatasetReference",
                            "parameters": {
                                "SqlTableName": "@pipeline().parameters.sqlTableName"
                            }
                        }
                    ],
                    "linkedServices": [
                        {
                            "referenceName": "SqlLinkedService",
                            "type": "LinkedServiceReference"
                        }
                    ]
                }
            }
        ],
        "parameters": {
            "sqlTableName": {
                "type": "String"
            },
            "url": {
                "type": "String"
            }
        }
    }
}

```

### <a name="pipeline-parameter-values"></a>Wartości parametrów potoku

```json
{
    "sqlTableName": "department",
    "url": "https://adftes.azurewebsites.net/api/execute/running"
}

```

### <a name="web-service-endpoint-code"></a>Kod punkt końcowy usługi sieci Web

```csharp

[HttpPost]
public HttpResponseMessage Execute(JObject payload)
{
    Trace.TraceInformation("Start Execute");

    JObject result = new JObject();
    result.Add("status", "complete");

    JArray datasets = payload.GetValue("datasets") as JArray;
    result.Add("sinktable", datasets[0]["properties"]["typeProperties"]["tableName"].ToString());

    JArray linkedServices = payload.GetValue("linkedServices") as JArray;
    string connString = linkedServices[0]["properties"]["typeProperties"]["connectionString"].ToString();

    System.Data.SqlClient.SqlConnection sqlConn = new System.Data.SqlClient.SqlConnection(connString);

    result.Add("sinkServer", sqlConn.DataSource);

    Trace.TraceInformation("Stop Execute");

    return this.Request.CreateResponse(HttpStatusCode.OK, result);
}

```

## <a name="next-steps"></a>Następne kroki
Zobacz inne działania przepływu sterowania obsługiwane przez fabrykę danych: 

- [Działanie Execute Pipeline](control-flow-execute-pipeline-activity.md)
- [Dla każdego działania](control-flow-for-each-activity.md)
- [Działanie GetMetadata](control-flow-get-metadata-activity.md)
- [Działanie Lookup](control-flow-lookup-activity.md)
