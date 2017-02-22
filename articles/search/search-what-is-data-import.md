---
title: "Przekazywanie danych do usługi Azure Search | Microsoft Docs"
description: "Dowiedz się, jak przekazywać dane do indeksu w usłudze Azure Search."
services: search
documentationcenter: 
author: ashmaka
manager: jhubbard
editor: 
tags: 
ms.assetid: aa8d47c1-4ae6-4209-a8ce-48d5a9474707
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 292c9150822363aba3336b1efce579dc5362cb14
ms.openlocfilehash: e522d608e8ff51e00b3c1a461bf9ba909b0105af


---
# <a name="upload-data-to-azure-search"></a>Przekazywanie danych do usługi Azure Search
> [!div class="op_single_selector"]
> * [Omówienie](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

Istnieją dwa sposoby wypełniania indeksu danymi. Pierwsza opcja polega na ręcznym wypychaniu danych do indeksu przy użyciu interfejsu [API REST](search-import-data-rest-api.md) lub zestawu [.NET SDK](search-import-data-dotnet.md) usługi Azure Search. Drugą opcją jest [wskazanie obsługiwanego źródła danych](search-indexer-overview.md) indeksowi i zezwolenie usłudze Azure Search na automatyczne ściągnięcie danych.

## <a name="push-data-to-an-index"></a>Wypychanie danych do indeksu
Ta metoda obejmuje programowe wysyłanie danych do usługi Azure Search w celu udostępnienia ich do wyszukiwania. W przypadku aplikacji wymagających bardzo niskich opóźnień (na przykład w razie konieczności synchronizowania operacji wyszukiwania z dynamicznymi bazami danych zapasów) model wypychania jest jedynym rozwiązaniem.

W celu wypychania danych do indeksu możesz użyć interfejsu [API REST](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) lub zestawu [.NET SDK](search-import-data-dotnet.md). Obecnie nie istnieje wsparcie narzędziowe wypychania danych za pośrednictwem portalu.

To podejście jest bardziej elastyczne niż model polegający na ściąganiu, ponieważ możesz przekazywać dokumenty pojedynczo lub w partiach (maksymalnie 1000 dokumentów na partię lub 16 MB zależnie od tego, który limit zostanie osiągnięty jako pierwszy). Model polegający na wypychaniu umożliwia również przekazywanie dokumentów do usługi Azure Search niezależnie od tego, gdzie znajdują się dane.

Formatem danych zrozumiałym dla usługi Azure Search jest JSON, a wszystkie dokumenty w zestawie danych muszą zawierać pola, które są mapowane na pola zdefiniowane w schemacie indeksu. 

## <a name="pull-data-into-an-index"></a>Ściąganie danych do indeksu
Model polegający na ściąganiu obejmuje przeszukiwanie obsługiwanego źródła danych i automatyczne przekazywanie danych do indeksu. W usłudze Azure Search ta możliwość jest wdrożona za pomocą *indeksatorów*, które obecnie są dostępne dla [usługi Blob Storage](search-howto-indexing-azure-blob-storage.md), [usługi Table Storage](search-howto-indexing-azure-tables.md), [usługi DocumentDB](http://aka.ms/documentdb-search-indexer), [usługi Azure SQL Database oraz programu SQL Server na maszynach wirtualnych Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md). 

Indeksatory łączą indeks ze źródłem danych (zwykle tabelą, widokiem lub równoważną strukturą) i mapują pola źródłowe na równoważne pola w indeksie. W czasie wykonywania zestaw wierszy jest automatycznie przekształcany w dane w formacie JSON i ładowany do określonego indeksu. Wszystkie indeksatory obsługują planowanie, dzięki czemu możesz określić, jak często dane mają być odświeżane. Większość indeksatorów udostępnia śledzenie zmian, jeśli źródło danych obsługuje tę funkcję. Dzięki śledzeniu zmian i usuwania istniejących dokumentów oraz rozpoznawaniu nowych dokumentów indeksatory eliminują konieczność aktywnego zarządzania danymi w indeksie. 

Funkcja indeksatora jest udostępniona w witrynie [Azure Portal](search-import-data-portal.md), interfejsie [API REST](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations) i zestawie [.NET SDK](https://docs.microsoft.com/otnet/api/microsoft.azure.search.iindexersoperations?redirectedfrom=MSDN#microsoft_azure_search_iindexersoperations). 

Zaletą korzystania z portalu jest to, że usługa Azure Search zazwyczaj może wygenerować domyślny schemat indeksu, odczytując metadane zestawu źródła danych. Wygenerowany indeks można modyfikować, dopóki indeks jest przetwarzany, po czym dozwolone są tylko te zmiany schematu, które nie wymagają ponownego indeksowania. Jeśli żądane zmiany wpływają bezpośrednio na schemat, konieczne będzie odbudowanie indeksu. 

Po wypełnieniu indeksu możesz użyć **Eksploratora wyszukiwania** na pasku poleceń portalu jako kroku weryfikacji.

## <a name="query-an-index-using-search-explorer"></a>Tworzenie zapytań względem indeksu za pomocą Eksploratora wyszukiwania

Szybkim sposobem wykonania wstępnego sprawdzenia przekazywania dokumentu jest użycie **Eksploratora wyszukiwania** w portalu. Eksplorator umożliwia wykonywanie zapytań względem indeksu bez konieczności pisania kodu. Środowisko wyszukiwania jest oparte na ustawieniach domyślnych, takich jak [prosta składnia](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) i domyślny parametr zapytania [searchMode](https://docs.microsoft.com/rest/api/searchservice/search-documents). Wyniki są zwracane w formacie JSON, co umożliwia inspekcję całego dokumentu.

> [!TIP]
> Wiele [przykładów kodu usługi Azure Search](https://github.com/Azure-Samples/?utf8=%E2%9C%93&query=search) obejmuje osadzone lub szybko dostępne zestawy danych, oferując łatwy sposób na rozpoczęcie pracy. Portal udostępnia również przykładowy indeksator i źródło danych składające się z małego zestawu danych nieruchomości (o nazwie „realestate-us-sample”). Po uruchomieniu wstępnie skonfigurowanego indeksatora na przykładowym źródle danych indeks zostaje utworzony i załadowany przy użyciu dokumentów, które następnie można odpytywać w Eksploratorze wyszukiwania lub za pomocą napisanego kodu.


<!--HONumber=Jan17_HO2-->


