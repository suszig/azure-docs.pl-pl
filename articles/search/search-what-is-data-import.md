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
ms.date: 12/09/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 72cc0d9ff35ff656a6134b52812b64c39a295a6f
ms.openlocfilehash: 53786d60d9971d9f976bf0f3ef4e40346c3101f4


---
# <a name="upload-data-to-azure-search"></a>Przekazywanie danych do usługi Azure Search
> [!div class="op_single_selector"]
> * [Omówienie](search-what-is-data-import.md)
> * [.NET](search-import-data-dotnet.md)
> * [REST](search-import-data-rest-api.md)
> 
> 

## <a name="data-upload-models-in-azure-search"></a>Modele przekazywania danych w usłudze Azure Search
Istnieją dwa sposoby wypełniania indeksu usługi Azure Search danymi. Pierwsza opcja polega na ręcznym wypychaniu danych do indeksu przy użyciu interfejsu [API REST](search-import-data-rest-api.md) lub zestawu [.NET SDK](search-import-data-dotnet.md) usługi Azure Search. Drugą opcją jest [wskazanie obsługiwanego źródła danych](search-indexer-overview.md) indeksowi usługi Azure Search i zezwolenie usłudze Azure Search na automatyczne ściągnięcie danych do usługi wyszukiwania.

Ten przewodnik obejmuje tylko instrukcje dotyczące używania modelu przekazywania danych polegającym na wypychaniu (który jest obsługiwany tylko przez interfejs [API REST](search-import-data-rest-api.md) i zestaw [.NET SDK](search-import-data-dotnet.md)), ale poniżej możesz także dowiedzieć się więcej na temat modelu polegającego na ściąganiu.

## <a name="push-data-to-an-index"></a>Wypychanie danych do indeksu
Ta metoda obejmuje programowe wysyłanie danych do usługi Azure Search w celu udostępnienia ich do wyszukiwania. W przypadku aplikacji wymagających bardzo niskich opóźnień (np. konieczności synchronizowania operacji wyszukiwania z dynamicznymi bazami danych zapasów), model wypychania jest jedynym rozwiązaniem.

W celu wypychania danych do indeksu możesz użyć interfejsu [API REST](https://docs.microsoft.com/rest/api/searchservice/AddUpdate-or-Delete-Documents) lub zestawu [.NET SDK](search-import-data-dotnet.md). Obecnie nie istnieje wsparcie narzędziowe wypychania danych za pośrednictwem portalu.

To podejście jest bardziej elastyczne niż model polegający na ściąganiu, ponieważ możesz przekazywać dokumenty pojedynczo lub w partiach (maksymalnie 1000 dokumentów na partię lub 16 MB zależnie od tego, który limit zostanie osiągnięty jako pierwszy). Model polegający na wypychaniu umożliwia również przekazywanie dokumentów do usługi Azure Search niezależnie od tego, gdzie znajdują się dane.

## <a name="pull-data-into-an-index"></a>Ściąganie danych do indeksu
Model polegający na ściąganiu obejmuje przeszukiwanie obsługiwanego źródła danych i automatyczne przekazywanie danych do indeksu usługi Azure Search. Dzięki śledzeniu zmian i usuwania istniejących dokumentów oraz rozpoznawaniu nowych dokumentów indeksatory eliminują konieczność aktywnego zarządzania danymi w indeksie.

W usłudze Azure Search ta możliwość jest wdrożona za pomocą *indeksatorów*, które obecnie są dostępne dla [usługi Blob Storage (wersja zapoznawcza)](search-howto-indexing-azure-blob-storage.md), usługi [DocumentDB](http://aka.ms/documentdb-search-indexer), [usługi Azure SQL Database oraz programu SQL Server na maszynach wirtualnych Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md).

Funkcja indeksatora jest widoczna w witrynie [Azure Portal](search-import-data-portal.md) oraz w interfejsie [API REST](https://docs.microsoft.com/rest/api/searchservice/Indexer-operations).




<!--HONumber=Dec16_HO2-->


