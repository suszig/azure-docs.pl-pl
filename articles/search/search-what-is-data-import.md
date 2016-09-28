<properties
    pageTitle="Przekazywanie danych w usłudze Azure Search | Microsoft Azure | Hostowana usługa wyszukiwania w chmurze"
    description="Dowiedz się, jak przekazywać dane do indeksu w usłudze Azure Search."
    services="search"
    documentationCenter=""
    authors="ashmaka"
    manager=""
    editor=""
    tags=""/>

<tags
    ms.service="search"
    ms.devlang="NA"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="ashmaka"/>


# Przekazywanie danych do usługi Azure Search
> [AZURE.SELECTOR]
- [Omówienie](search-what-is-data-import.md)
- [.NET](search-import-data-dotnet.md)
- [REST](search-import-data-rest-api.md)


## Modele przekazywania danych w usłudze Azure Search
Istnieją dwa sposoby wypełniania indeksu usługi Azure Search danymi. Pierwsza opcja polega na ręcznym wypychaniu danych do indeksu przy użyciu interfejsu [API REST](search-import-data-rest-api.md) lub zestawu [.NET SDK](search-import-data-dotnet.md) usługi Azure Search. Drugą opcją jest [wskazanie obsługiwanego źródła danych](search-indexer-overview.md) indeksowi usługi Azure Search i zezwolenie usłudze Azure Search na automatyczne ściągnięcie danych do usługi wyszukiwania.

Ten przewodnik obejmuje tylko instrukcje dotyczące używania modelu przekazywania danych polegającym na wypychaniu (który jest obsługiwany tylko przez interfejs [API REST](search-import-data-rest-api.md) i zestaw [.NET SDK](search-import-data-dotnet.md)), ale poniżej możesz także dowiedzieć się więcej na temat modelu polegającego na ściąganiu.

### Wypychanie danych do indeksu

Ta metoda obejmuje programowe wysyłanie danych do usługi Azure Search w celu udostępnienia ich do wyszukiwania. W przypadku aplikacji wymagających bardzo niskich opóźnień (np. konieczności synchronizowania operacji wyszukiwania z dynamicznymi bazami danych zapasów), model wypychania jest jedynym rozwiązaniem.

W celu wypychania danych do indeksu możesz użyć interfejsu [API REST](https://msdn.microsoft.com/library/azure/dn798930.aspx) lub zestawu [.NET SDK](search-import-data-dotnet.md). Obecnie nie istnieje wsparcie narzędziowe wypychania danych za pośrednictwem portalu.

To podejście jest bardziej elastyczne niż model polegający na ściąganiu, ponieważ możesz przekazywać dokumenty pojedynczo lub w partiach (maksymalnie 1000 dokumentów na partię lub 16 MB zależnie od tego, który limit zostanie osiągnięty jako pierwszy). Model polegający na wypychaniu umożliwia również przekazywanie dokumentów do usługi Azure Search niezależnie od tego, gdzie znajdują się dane.

### Ściąganie danych do indeksu

Model polegający na ściąganiu obejmuje przeszukiwanie obsługiwanego źródła danych i automatyczne przekazywanie danych do indeksu usługi Azure Search. Dzięki śledzeniu zmian i usuwania istniejących dokumentów oraz rozpoznawaniu nowych dokumentów indeksatory eliminują konieczność aktywnego zarządzania danymi w indeksie.

W usłudze Azure Search ta możliwość jest wdrożona za pomocą *indeksatorów*, które obecnie są dostępne dla [usługi Blob Storage (wersja zapoznawcza)](search-howto-indexing-azure-blob-storage.md), usługi [DocumentDB](http://aka.ms/documentdb-search-indexer), [usługi Azure SQL Database oraz programu SQL Server na maszynach wirtualnych Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md).

Funkcja indeksatora jest widoczna w witrynie [Azure Portal](search-import-data-portal.md) oraz w interfejsie [API REST](https://msdn.microsoft.com/library/azure/dn946891.aspx).



<!--HONumber=Sep16_HO3-->


