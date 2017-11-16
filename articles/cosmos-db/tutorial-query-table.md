---
title: "Jak wykonać zapytanie dotyczące tabeli danych w usłudze Azure DB rozwiązania Cosmos? | Microsoft Docs"
description: "Dowiedz się, jak dane tabeli zapytania w usłudze Azure DB rozwiązania Cosmos"
services: cosmos-db
documentationcenter: 
author: kanshiG
manager: jhubbard
editor: 
tags: 
ms.assetid: 14bcb94e-583c-46f7-9ea8-db010eb2ab43
ms.service: cosmos-db
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 11/15/2017
ms.author: govindk
ms.openlocfilehash: cbb752692fbd618d9e7e14c8a80b582aad657b38
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/15/2017
---
# <a name="azure-cosmos-db-how-to-query-table-data-by-using-the-table-api"></a>Azure DB rozwiązania Cosmos: Jak wykonać zapytanie tabeli danych przy użyciu interfejsu API tabeli

Azure DB rozwiązania Cosmos [API tabeli](table-introduction.md) obsługuje OData i [LINQ](https://docs.microsoft.com/rest/api/storageservices/fileservices/writing-linq-queries-against-the-table-service) zapytań dotyczących danych klucz wartość (tabeli).  

W tym artykule opisano następujące zadania: 

> [!div class="checklist"]
> * Wykonywanie zapytania na danych przy użyciu interfejsu API tabeli

Zapytania w tym artykule, skorzystaj z poniższego przykładu `People` tabeli:

| PartitionKey | RowKey | Adres e-mail | Numer telefonu |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0101 |
| Smith | Ben | Ben@contoso.com| 425-555-0102 |
| Smith | Jan | Jeff@contoso.com| 425-555-0104 | 

Zobacz [badania tabel i jednostek] (https://docs.microsoft.com/rest/api/storageservices/fileservices/querying-tables-and-entities) Aby uzyskać więcej informacji na temat sposobu zapytania przy użyciu interfejsu API tabeli. 

Aby uzyskać więcej informacji na możliwości premium, które oferuje bazy danych rozwiązania Cosmos Azure, zobacz [interfejsu API Azure rozwiązania Cosmos DB tabeli](table-introduction.md) i [opracowanie przy użyciu interfejsu API tabeli w programie .NET](tutorial-develop-table-dotnet.md). 

## <a name="prerequisites"></a>Wymagania wstępne

Dla tych zapytań do pracy musi mieć konto bazy danych Azure rozwiązania Cosmos i danych jednostki w kontenerze. Nie masz żadnego z tych? Zakończenie [szybkiego startu 5 minutową](create-table-dotnet.md) lub [samouczek developer](tutorial-develop-table-dotnet.md) Tworzenie konta usługi i umieścić w bazie danych.

## <a name="query-on-partitionkey-and-rowkey"></a>Zapytanie dotyczące PartitionKey i RowKey
Ponieważ właściwości PartitionKey i RowKey tworzą klucza podstawowego jednostki, aby zidentyfikować jednostki można użyć następującej składni specjalne: 

**Zapytanie**

```
https://<mytableendpoint>/People(PartitionKey='Harp',RowKey='Walter')  
```
**Wyniki**

| PartitionKey | RowKey | Adres e-mail | Numer telefonu |
| --- | --- | --- | --- |
| Harp | Walter | Walter@contoso.com| 425-555-0104 |

Alternatywnie można określić właściwości jako część `$filter` opcji, jak pokazano w poniższej sekcji. Należy pamiętać, że nazw właściwości kluczy i wartości stałe jest rozróżniana wielkość liter. Właściwości PartitionKey i RowKey są typu ciąg. 

## <a name="query-by-using-an-odata-filter"></a>Zapytanie za pomocą filtru OData
Gdy w przypadku tworzenia ciąg filtru, pamiętać o tych reguł: 

* Aby porównać właściwości na wartość, należy użyć operatorów logicznych w specyfikacji protokołu OData. Należy pamiętać, że nie można porównać właściwości na wartość dynamiczną. Po jednej stronie wyrażenia musi być stałą. 
* Nazwa właściwości, operator i wartość stała muszą być oddzielone spacjami zakodowane w adresie URL. Odstęp jest zakodowane w adresie URL jako `%20`. 
* Wszystkie części ciąg filtru jest rozróżniana wielkość liter. 
* Stała wartość musi być tego samego typu danych jako wartość właściwości w kolejności filtru do zwrócenia prawidłowe wyniki. Aby uzyskać więcej informacji na temat typów obsługiwanych właściwości zobacz [opis modelu danych usługi tabel](https://docs.microsoft.com/rest/api/storageservices/understanding-the-table-service-data-model). 

Poniżej przedstawiono przykładowe zapytanie, które pokazuje, jak do filtrowania według właściwości PartitionKey i poczty E-mail przy użyciu OData `$filter`.

**Zapytanie**

```
https://<mytableapi-endpoint>/People()?$filter=PartitionKey%20eq%20'Smith'%20and%20Email%20eq%20'Ben@contoso.com'
```

Aby uzyskać więcej informacji dotyczących sposobu tworzenia wyrażenia filtru dla różnych typów danych, zobacz [badania tabel i jednostek](https://docs.microsoft.com/rest/api/storageservices/querying-tables-and-entities).

**Wyniki**

| PartitionKey | RowKey | Adres e-mail | Numer telefonu |
| --- | --- | --- | --- |
| Ben |Smith | Ben@contoso.com| 425-555-0102 |

## <a name="query-by-using-linq"></a>Zapytanie za pomocą LINQ 
Możesz także zbadać za pomocą LINQ, co oznacza odpowiedniego wyrażenia zapytania OData. Oto przykład sposobu tworzenia zapytań przy użyciu zestawu .NET SDK:

```csharp
CloudTableClient tableClient = account.CreateCloudTableClient();
CloudTable table = tableClient.GetTableReference("people");

TableQuery<CustomerEntity> query = new TableQuery<CustomerEntity>()
    .Where(
        TableQuery.CombineFilters(
            TableQuery.GenerateFilterCondition(PartitionKey, QueryComparisons.Equal, "Smith"),
            TableOperators.And,
            TableQuery.GenerateFilterCondition(Email, QueryComparisons.Equal,"Ben@contoso.com")
    ));

await table.ExecuteQuerySegmentedAsync<CustomerEntity>(query, null);
```

## <a name="next-steps"></a>Następne kroki

W tym samouczku wykonaniu następujących czynności:

> [!div class="checklist"]
> * Przedstawiono sposób tworzenia zapytań przy użyciu interfejsu API tabeli

Możesz teraz przejść do następnym samouczku informacje na temat dystrybucji danych globalnie.

> [!div class="nextstepaction"]
> [Globalny dystrybucji danych](tutorial-global-distribution-table.md)
