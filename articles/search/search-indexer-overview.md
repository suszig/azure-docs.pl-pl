<properties
    pageTitle="Indeksatory w usłudze Azure Search | Microsoft Azure | Hostowana usługa wyszukiwania w chmurze"
    description="Usługi Azure SQL Database, DocumentDB i Azure Storage można przeszukiwać w celu wyodrębniania danych z możliwością wyszukiwania i wypełnienia indeksu usługi Azure Search."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="mblythe"
    editor=""
    tags="azure-portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="04/14/2016"
    ms.author="heidist"/>

# Indeksatory w usłudze Azure Search
> [AZURE.SELECTOR]
- [Omówienie](search-indexer-overview.md)
- [Portal](search-import-data-portal.md)
- [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Blob Storage (wersja zapoznawcza)](search-howto-indexing-azure-blob-storage.md)
- [Table Storage (wersja zapoznawcza)](search-howto-indexing-azure-tables.md)

Dostępny w usłudze Azure Search **indeksator** jest przeszukiwarką, która umożliwia wyodrębnienie danych oraz metadanych z możliwością wyszukiwania z zewnętrznego źródła danych i wypełnienie indeksu na podstawie mapowań poszczególnych pól między indeksem a źródłem danych. Ta metoda jest czasami określana jako „model ściągania”, ponieważ usługa pobiera dane bez konieczności pisania kodu, który wypycha dane do indeksu.

Dane można wprowadzać tylko za pomocą indeksatora lub kombinacji metod obejmujących użycie indeksatora w celu załadowania tylko niektórych pól w indeksie.

Indeksatory można uruchamiać na żądanie lub według cyklicznego harmonogramu odświeżania danych uruchamianego co 15 minut. Częstsze aktualizacje wymagają modelu wypychania, który pozwala jednocześnie aktualizować dane w usłudze Azure Search i zewnętrznym źródle danych.

Indeksator pobiera dane ze **źródła danych**, które zawiera na przykład parametry połączenia. Obecnie są obsługiwane następujące źródła danych:

- [Azure SQL Database](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md) (lub program SQL Server na maszynie wirtualnej platformy Azure).
- [DocumentDB](../documentdb/documentdb-search-indexer.md)
- [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) (Obecnie dostępna w wersji zapoznawczej. Umożliwia wyodrębnianie tekstu z plików PDF, dokumentów pakietu Office oraz plików HTML i XML).
- [Azure Table Storage](search-howto-indexing-azure-tables.md) (Obecnie dostępna w wersji zapoznawczej).

Źródła danych są konfigurowane i zarządzane niezależnie od indeksatorów, które z nich korzystają. Oznacza to, że jedno źródło może być używane przez wiele indeksatorów w celu jednoczesnego ładowania kilku indeksów. 

Zarówno [zestaw .NET SDK](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx), jak i [interfejs API REST usługi](https://msdn.microsoft.com/library/azure/dn946891.aspx) obsługują zarządzanie indeksatorami i źródłami danych. 

Można również skonfigurować indeksator w portalu podczas używania kreatora **importu danych**. Aby utworzyć i załadować indeks za pomocą kreatora, przejdź do artykułu [Get started with Azure Search in the portal](search-get-started-portal) (Wprowadzenie do usługi Azure Search w portalu), który zawiera krótki samouczek oraz informacje dotyczące używania przykładowych danych i indeksatora usługi DocumentDB.






<!--HONumber=Jun16_HO2-->


