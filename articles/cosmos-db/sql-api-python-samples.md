---
title: "Przykłady Python interfejsu API SQL dla bazy danych Azure rozwiązania Cosmos | Dokumentacja firmy Microsoft"
description: "Znaleźć przykłady Python w usłudze github dla typowych zadań w usłudze Azure DB rozwiązania Cosmos, w tym operacji CRUD."
keywords: "Przykłady dla języka Python"
services: cosmos-db
author: moderakh
manager: jhubbard
editor: monicar
documentationcenter: python
ms.assetid: 7f4f8db3-e9db-4645-92ef-7819d486a349
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/24/2016
ms.author: moderakh
ms.openlocfilehash: 7f6300b5105b6b54cc27cde4cb95234d7dbf6a98
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-cosmos-db-python-examples"></a>Przykłady rozwiązania Cosmos DB Python platformy Azure
> [!div class="op_single_selector"]
> * [Przykłady .NET](sql-api-dotnet-samples.md)
> * [Przykłady środowiska node.js](sql-api-nodejs-samples.md)
> * [Przykłady dla języka Python](sql-api-python-samples.md)
> * [Galeria próbki kodu platformy Azure](https://azure.microsoft.com/resources/samples/?sort=0&service=cosmos-db)
> 
> 

[!INCLUDE [cosmos-db-sql-api](../../includes/cosmos-db-sql-api.md)]

Przykładowe rozwiązania, wykonujących operacje CRUD i innymi typowymi operacjami zasobów bazy danych Azure rozwiązania Cosmos znajdują się w [azure-documentdb-python](https://github.com/Azure/azure-documentdb-python/tree/master/samples) repozytorium GitHub. Ten artykuł zawiera:

* Pliki projektu łącza do zadań w każdym przykładzie Python. 
* Linki do pokrewnych interfejsu API odwoływać się do zawartości.

**Wymagania wstępne**

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

- Możesz [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio): Your Visual Studio subskrypcji otrzymasz kredyt, co miesiąc, używanego programu płatnych usług Azure.

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

Należy również [zestaw SDK Python](sql-api-sdk-python.md). 
   
   > [!NOTE]
   > Każda próbka jest autonomicznym środowiskiem, konfiguruje się i czyści po samej siebie. Tak, przykłady wystawiać wielu wywołań [document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html). Zawsze, gdy jest to realizowane subskrypcji jest opłaty naliczane za godzinę użycia na warstwę wydajności kolekcji tworzona. 
   > 
   > 

## <a name="database-examples"></a>Przykłady bazy danych
[Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement/Program.py) pliku [DatabaseManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/DatabaseManagement) projektu przedstawiono sposób wykonywania następujących zadań:

| Zadanie | Dokumentacja interfejsu API |
| --- | --- |
| [Tworzenie bazy danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L65-L76) |[document_client. Metody CreateDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Zapytania konta bazy danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L49-L62) |[document_client. QueryDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Przeczytaj przez identyfikator bazy danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L79-L96) |[document_client. ReadDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Lista baz danych, konta](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L99-L110) |[document_client. ReadDatabases](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |
| [Usuwanie bazy danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/DatabaseManagement/Program.py#L113-L126) |[document_client. Metody DeleteDatabase](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html) |

## <a name="collection-examples"></a>Przykłady kolekcji
[Program.py](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement/Program.py) pliku [CollectionManagement](https://github.com/Azure/azure-documentdb-python/tree/master/samples/CollectionManagement) projektu przedstawiono sposób wykonywania następujących zadań:

| Zadanie | Dokumentacja interfejsu API |
| --- | --- |
| [Tworzenie kolekcji](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L84-L135) |[document_client. CreateCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Odczytaj listę wszystkich kolekcji w bazie danych](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L198-L225) |[document_client. ListCollections](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Pobierz kolekcję według Identyfikatora](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L178-L195) |[document_client. ReadCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Pobierz warstwę wydajności kolekcji](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L139-L161) |[DocumentQueryable.QueryOffers](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Zmień warstwę wydajności kolekcji](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L163-L175) |[document_client. ReplaceOffer](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |
| [Usuwanie kolekcji](https://github.com/Azure/azure-documentdb-python/blob/d78170214467e3ab71ace1a7400f5a7fa5a7b5b0/samples/CollectionManagement/Program.py#L212-L225) |[document_client. DeleteCollection](http://azure.github.io/azure-documentdb-python/api/pydocumentdb.document_client.html#CreateCollection) |

