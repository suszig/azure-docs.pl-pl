---
title: "Indeksowanie obiektów blob JSON z indeksatora obiektów blob Azure Search"
description: "Indeksowanie obiektów blob JSON z indeksatora obiektów blob Azure Search"
services: search
documentationcenter: 
author: chaosrealm
manager: pablocas
editor: 
ms.assetid: 57e32e51-9286-46da-9d59-31884650ba99
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 09/07/2017
ms.author: eugenesh
ms.openlocfilehash: bf4d3a517e1308a142d21cffff64f3c6e104eb62
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="indexing-json-blobs-with-azure-search-blob-indexer"></a>Indeksowanie obiektów blob JSON z indeksatora obiektów blob Azure Search
W tym artykule przedstawiono sposób konfigurowania usługi Azure Search indeksatora obiektu blob do wyodrębniania zawartości strukturalnych z obiektów blob, które zawierają JSON.

## <a name="scenarios"></a>Scenariusze
Domyślnie [indeksatora obiektów blob Azure Search](search-howto-indexing-azure-blob-storage.md) analizuje obiekty BLOB JSON jako pojedynczy fragmentów tekstu. Często chcesz zachować struktury dokumentów JSON. Na przykład, dla danego dokumentu JSON

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Możesz przeanalizować go do usługi Azure Search dokumentu z pola "tagi", "datePublished" i "text".

Alternatywnie, jeśli obiektów blob zawiera **Tablica obiektów JSON**, może być każdy element tablicy, tak aby stać się oddzielny dokument usługi Azure Search. Na przykład, dla danego obiektu blob z JSON:  

    [
        { "id" : "1", "text" : "example 1" },
        { "id" : "2", "text" : "example 2" },
        { "id" : "3", "text" : "example 3" }
    ]

można wypełnić indeksu usługi Azure Search przy użyciu trzech oddzielnych dokumentów, każdy z pola "id" i "tekst".

> [!IMPORTANT]
> Podczas analizowania funkcji tablicy JSON jest obecnie w przeglądzie. Jest on dostępny tylko w wersji interfejsu API REST **2016-09-01-Preview**. Należy pamiętać, Podgląd interfejsy API są przeznaczone do testowania i ocenie, a nie powinna być używana w środowisku produkcyjnym.
>
>

## <a name="setting-up-json-indexing"></a>Konfigurowanie indeksowania JSON
Indeksowanie obiektów blob JSON jest podobny do wyodrębniania zwykłego dokumentu. Najpierw Utwórz źródło danych, dokładnie w normalny sposób: 

    POST https://[service name].search.windows.net/datasources?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
        "name" : "my-blob-datasource",
        "type" : "azureblob",
        "credentials" : { "connectionString" : "DefaultEndpointsProtocol=https;AccountName=<account name>;AccountKey=<account key>;" },
        "container" : { "name" : "my-container", "query" : "optional, my-folder" }
    }   

Następnie utwórz indeksu wyszukiwania docelowego, jeśli nie został wcześniej. 

Na koniec Utwórz indeksator i ustawić `parsingMode` parametr `json` (do indeksowania każdy obiekt blob jako pojedynczego dokumentu) lub `jsonArray` (jeśli obiektów blob zawiera tablice notacji JSON i trzeba każdy element tablicy powinien być traktowany jako osobny dokument):

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } }
    }

Jeśli to konieczne, użyj **mapowań pól** do pobrania właściwości dokumentu JSON źródłowej używana do wypełniania indeksu wyszukiwania docelowym, jak pokazano w następnej sekcji.

> [!IMPORTANT]
> Jeśli używasz `json` lub `jsonArray` analizy tryb usługi Azure Search przyjęto założenie, że wszystkie obiekty BLOB w źródle danych zawierają JSON. Daj nam znać, jeśli zachodzi potrzeba obsługi różnych obiektów blob JSON i z systemem innym niż JSON w tym samym źródle danych, na [witryny UserVoice](https://feedback.azure.com/forums/263029-azure-search).
>
>

## <a name="using-field-mappings-to-build-search-documents"></a>Tworzenie dokumentów wyszukiwania przy użyciu mapowań pól
Obecnie usługi Azure Search nie można indeksować dowolnych dokumentów JSON bezpośrednio, ponieważ obsługuje on typów danych tylko typy pierwotne, tablic ciągów i punkty GeoJSON. Można jednak użyć **mapowań pól** wybrać części dokumentu JSON, a "Podnieś" je do pól najwyższego poziomu dokumentu wyszukiwania. Aby poznać podstawy mapowania pól, zobacz temat [mapowań pól indeksator usługi Azure Search zestawiania różnice między źródłami danych i indeksów wyszukiwania](search-indexer-field-mappings.md).

Powracające do dokumentu JSON naszym przykładzie:

    {
        "article" : {
             "text" : "A hopefully useful article explaining how to parse JSON blobs",
            "datePublished" : "2016-04-13"
            "tags" : [ "search", "storage", "howto" ]    
        }
    }

Załóżmy, że masz indeksem wyszukiwania z następującymi polami: `text` typu `Edm.String`, `date` typu `Edm.DateTimeOffset`, i `tags` typu `Collection(Edm.String)`. Do mapowania użytkownika JSON do żądanego kształtu, użyj następującego mapowania pól:

    "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
      ]

Nazwy pól źródła w mapowania są określane za pomocą [wskaźnika JSON](http://tools.ietf.org/html/rfc6901) notacji. Można rozpoczynać się kreską ukośną do odwoływania się do katalogu głównego dokumentu JSON, a następnie wybierz odpowiednie właściwości (na poziomie dowolnego zagnieżdżenia) przy użyciu ścieżki rozdzielonych ukośnikiem do przodu.

Ponadto mogą odwoływać się do poszczególnych elementów przy użyciu liczony od zera indeks. Na przykład wybierz pierwszy element tablicy "tagi" w powyższym przykładzie, należy użyć mapowania pól, jak to:

    { "sourceFieldName" : "/article/tags/0", "targetFieldName" : "firstTag" }

> [!NOTE]
> Jeśli nazwa pola źródła w ścieżce mapowania pola odwołuje się do właściwości, która nie istnieje w formacie JSON, że to mapowanie zostanie pominięty bez błędu. Można to zrobić, aby firma Microsoft obsługuje dokumenty z innym schematem (co jest typowe przypadek użycia). Ponieważ nie ma żadnych weryfikacji, należy zwrócić uwagę, aby uniknąć błędów pisowni w specyfikacji mapowania pól.
>
>

Jeśli dokumentów JSON zawierają tylko proste właściwości najwyższego poziomu, mogą nie być potrzebne mapowań pól w ogóle. Na przykład jeśli Twoje JSON wygląda tak, najwyższego poziomu właściwości "text", "datePublished" i "tagów" bezpośrednio mapy do odpowiednich pól w indeksie wyszukiwania:

    {
       "text" : "A hopefully useful article explaining how to parse JSON blobs",
       "datePublished" : "2016-04-13"
       "tags" : [ "search", "storage", "howto" ]    
     }

Poniżej przedstawiono pełną indeksatora ładunku z mapowania pól:

    POST https://[service name].search.windows.net/indexers?api-version=2016-09-01
    Content-Type: application/json
    api-key: [admin key]

    {
      "name" : "my-json-indexer",
      "dataSourceName" : "my-blob-datasource",
      "targetIndexName" : "my-target-index",
      "schedule" : { "interval" : "PT2H" },
      "parameters" : { "configuration" : { "parsingMode" : "json" } },
      "fieldMappings" : [
        { "sourceFieldName" : "/article/text", "targetFieldName" : "text" },
        { "sourceFieldName" : "/article/datePublished", "targetFieldName" : "date" },
        { "sourceFieldName" : "/article/tags", "targetFieldName" : "tags" }
        ]
    }

## <a name="indexing-nested-json-arrays"></a>Indeksowanie tablice zagnieżdżone JSON
Co zrobić, jeśli chcesz indeksu jest Tablica obiektów JSON, ale tablicy jest zagnieżdżony gdzieś w dokumencie? Można wybrać, które właściwości zawiera przy użyciu tablicy `documentRoot` właściwości konfiguracji. Jeśli na przykład obiektów blob wyglądać następująco:

    {
        "level1" : {
            "level2" : [
                { "id" : "1", "text" : "Use the documentRoot property" },
                { "id" : "2", "text" : "to pluck the array you want to index" },
                { "id" : "3", "text" : "even if it's nested inside the document" }  
            ]
        }
    }

Indeks tablicy zawarte w przy użyciu tej konfiguracji `level2` właściwości:

    {
        "name" : "my-json-array-indexer",
        ... other indexer properties
        "parameters" : { "configuration" : { "parsingMode" : "jsonArray", "documentRoot" : "/level1/level2" } }
    }

## <a name="help-us-make-azure-search-better"></a>Pomóż nam udoskonalić usługę Azure Search
Jeśli masz żądania funkcji lub pomysły dotyczące ulepszeń dotrzeć do nas w naszej [witryny UserVoice](https://feedback.azure.com/forums/263029-azure-search/).
