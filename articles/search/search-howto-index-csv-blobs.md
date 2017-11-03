---
title: "Indeksowanie obiektów blob CSV z indeksatora obiektów blob Azure Search | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak indeksowanie obiektów blob CSV z usługi Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: ed3c9cff-1946-4af2-a05a-5e0b3d61eb25
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 12/15/2016
ms.author: eugenesh
ms.openlocfilehash: af9da85c37211d2436c23cc05400031c661ef51e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="indexing-csv-blobs-with-azure-search-blob-indexer"></a>Indeksowanie obiektów blob CSV z indeksatora obiektów blob Azure Search
Domyślnie [indeksatora obiektów blob Azure Search](search-howto-indexing-azure-blob-storage.md) analizuje rozdzielany tekst obiekty BLOB jako pojedynczy fragmentów tekstu. Jednak z obiektami blob zawierający dane w formacie CSV, często zachodzi potrzeba Traktuj każdego wiersza w obiekcie blob jako osobny dokument. Na przykład, dla danego tekstu rozdzielanego następujące: 

    id, datePublished, tags
    1, 2016-01-12, "azure-search,azure,cloud" 
    2, 2016-07-07, "cloud,mobile" 

należy przeanalizować go do 2 dokumentów zawierających każdego pola "tagi", "datePublished" i "id".

W tym artykule dowiesz się, jak analizować obiektów blob CSV z indeksatora obiektów blob Azure Search. 

> [!IMPORTANT]
> Ta funkcja jest obecnie w przeglądzie. Jest on dostępny tylko w wersji interfejsu API REST **2015-02-28-Preview**. Należy pamiętać, Podgląd interfejsy API są przeznaczone do testowania i ocenie, a nie powinna być używana w środowisku produkcyjnym. 
> 
> 

## <a name="setting-up-csv-indexing"></a>Konfigurowanie indeksowania CSV
Do indeksu CSV obiektów blob, Utwórz lub zaktualizuj definicję indeksatora `delimitedText` podczas analizowania trybu:  

    {
      "name" : "my-csv-indexer",
      ... other indexer properties
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "firstLineContainsHeaders" : true } }
    }

Aby uzyskać więcej szczegółów na tworzenie interfejsu API indeksatora, zapoznaj się [Utwórz indeksator](search-api-indexers-2015-02-28-preview.md#create-indexer).

`firstLineContainsHeaders`Wskazuje, że w pierwszym wierszu (niepustych) każdy obiekt blob zawiera nagłówki.
Jeśli obiekty BLOB nie zawierają wiersz nagłówka początkowej, nagłówki powinny określony w konfiguracji indeksatora: 

    "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } } 

Aktualnie obsługiwana jest tylko kodowania UTF-8. Ponadto tylko przecinkami `','` znak jest obsługiwany jako ogranicznik. Jeśli potrzebujesz pomocy technicznej dla innych kodowania lub ograniczniki, prosimy o kontakt na [witryny UserVoice](https://feedback.azure.com/forums/263029-azure-search).

> [!IMPORTANT]
> Gdy używasz tekstu rozdzielanego podczas analizowania trybu usługi Azure Search zakłada, że wszystkie obiekty BLOB w źródle danych będą CSV. Jeśli potrzebujesz do obsługi różnych CSV i CSV z systemem innym niż obiekty BLOB w tym samym źródle danych, prosimy o kontakt na [witryny UserVoice](https://feedback.azure.com/forums/263029-azure-search).
> 
> 

## <a name="request-examples"></a>Przykładowe żądanie
Umieszczanie to wszystkie ze sobą, poniżej przedstawiono przykłady pełną ładunku. 

Źródło danych: 

    POST https://[service name].search.windows.net/datasources?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "<optional, my-folder>" }
    }   

Indeksator:

    POST https://[service name].search.windows.net/indexers?api-version=2015-02-28-Preview
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-csv-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "parameters" : { "configuration" : { "parsingMode" : "delimitedText", "delimitedTextHeaders" : "id,datePublished,tags" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomóż nam udoskonalić usługę Azure Search
Jeśli masz żądania funkcji lub pomysły dotyczące ulepszenia należy dotrzeć do nas w naszej [witryny UserVoice](https://feedback.azure.com/forums/263029-azure-search/).

