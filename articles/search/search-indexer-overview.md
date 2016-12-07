---
title: "Indeksatory w usłudze Azure Search | Microsoft Docs"
description: "Usługi Azure SQL Database, DocumentDB i Azure Storage można przeszukiwać w celu wyodrębniania danych z możliwością wyszukiwania i wypełnienia indeksu usługi Azure Search."
services: search
documentationcenter: 
author: HeidiSteen
manager: jhubbard
editor: 
tags: azure-portal
ms.assetid: 34a7694c-8fd9-46b1-8900-cefdd7236323
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 10/27/2016
ms.author: heidist
translationtype: Human Translation
ms.sourcegitcommit: 2cddd262f912efff029bb05f0f3f3551409167ed
ms.openlocfilehash: 4c969499f562d8cef9c09ebd909508b1c0542c57

---

# <a name="indexers-in-azure-search"></a>Indeksatory w usłudze Azure Search
> [!div class="op_single_selector"]
>
> * [Omówienie](search-indexer-overview.md)
> * [Portal](search-import-data-portal.md)
> * [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
> * [DocumentDB](../documentdb/documentdb-search-indexer.md)
> * [Blob Storage (wersja zapoznawcza)](search-howto-indexing-azure-blob-storage.md)
> * [Table Storage (wersja zapoznawcza)](search-howto-indexing-azure-tables.md)
>
>

Dostępny w usłudze Azure Search **indeksator** jest przeszukiwarką, która umożliwia wyodrębnienie danych oraz metadanych z możliwością wyszukiwania z zewnętrznego źródła danych i wypełnienie indeksu na podstawie mapowań poszczególnych pól między indeksem a źródłem danych. Ta metoda jest czasami określana jako „model ściągania”, ponieważ usługa pobiera dane bez konieczności pisania kodu, który wypycha dane do indeksu.

Dane można wprowadzać tylko za pomocą indeksatora lub kombinacji metod obejmujących użycie indeksatora w celu załadowania tylko niektórych pól w indeksie.

Indeksatory można uruchamiać na żądanie lub według cyklicznego harmonogramu odświeżania danych uruchamianego co 15 minut. Częstsze aktualizacje wymagają modelu wypychania, który pozwala jednocześnie aktualizować dane w usłudze Azure Search i zewnętrznym źródle danych.

## <a name="approaches-for-creating-and-managing-indexers"></a>Podejścia do tworzenia indeksatorów i zarządzania nimi
Tworzenie ogólnie dostępnych indeksatorów, takich jak Azure SQL czy DocumentDB, i zarządzanie nimi może odbywać się przy użyciu następujących podejść:

* [Portal > Kreator importowania danych ](search-get-started-portal.md)
* [Interfejs API REST usługi](https://msdn.microsoft.com/library/azure/dn946891.aspx)
* [Zestaw SDK platformy .NET](https://msdn.microsoft.com/library/azure/microsoft.azure.search.iindexersoperations.aspx)

## <a name="basic-configuration-steps"></a>Podstawowe kroki konfiguracji
Indeksatory oferują funkcje, które są unikatowe dla źródła danych. W związku z tym niektóre aspekty konfiguracji indeksatora lub źródła danych różnią się w zależności od typu indeksatora. Wszystkie indeksatory korzystają jednak z takich samych kompozycji i wymagań. Kroki, które są wspólne dla wszystkich indeksatorów, znajdują się poniżej.

### <a name="step-1-create-an-index"></a>Krok 1. Tworzenie indeksu
Indeksator będzie automatyzować niektóre zadania związane z wprowadzaniem danych, ale nie dotyczy to tworzenia indeksu. Jako warunek wstępny należy posiadać wstępnie zdefiniowany indeks z polami, które odpowiadają polom w zewnętrznym źródle danych. Aby uzyskać więcej informacji dotyczących tworzenia struktury indeksu, zobacz [Create an Index (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn798941.aspx) (Tworzenie indeksu — interfejs API REST usługi Azure Search).

### <a name="step-2-create-a-data-source"></a>Krok 2. Tworzenie źródła danych
Indeksator pobiera dane ze **źródła danych**, które zawiera na przykład parametry połączenia. Obecnie są obsługiwane następujące źródła danych:

* [Usługa Azure SQL Database lub program SQL Server na maszynie wirtualnej platformy Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [DocumentDB](../documentdb/documentdb-search-indexer.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) używany do wyodrębniania tekstu z plików PDF, dokumentów pakietu Office oraz plików HTML i XML
* [Azure Table Storage](search-howto-indexing-azure-tables.md)

Źródła danych są konfigurowane i zarządzane niezależnie od indeksatorów, które z nich korzystają. Oznacza to, że jedno źródło może być używane przez wiele indeksatorów w celu jednoczesnego ładowania kilku indeksów.

### <a name="step-3create-and-schedule-the-indexer"></a>Krok 3. Tworzenie indeksatora i ustawianie jego harmonogramu
Definicja indeksatora to konstrukcja określająca indeks, źródło danych i harmonogram. Indeksator może odwoływać się do źródła danych z innej usługi pod warunkiem, że źródło danych jest z tej samej subskrypcji. Aby uzyskać więcej informacji dotyczących tworzenia struktury indeksatora, zobacz [Create Indexer (Azure Search REST API)](https://msdn.microsoft.com/library/azure/dn946899.aspx) (Tworzenie indeksatora — interfejs API REST usługi Azure Search).

## <a name="next-steps"></a>Następne kroki
Teraz, gdy masz już podstawowe informacje, następnym krokiem jest przegląd wymagań i zadań specyficznych dla poszczególnych typów źródeł danych.

* [Usługa Azure SQL Database lub program SQL Server na maszynie wirtualnej platformy Azure](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [DocumentDB](../documentdb/documentdb-search-indexer.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) używany do wyodrębniania tekstu z plików PDF, dokumentów pakietu Office oraz plików HTML i XML
* [Azure Table Storage](search-howto-indexing-azure-tables.md)
* [Indeksowanie obiektów blob plików CSV za pomocą indeksatora obiektów blob usługi Azure Search (wersja zapoznawcza)](search-howto-index-csv-blobs.md)
* [Indeksowanie obiektów blob plików JSON za pomocą indeksatora obiektów blob usługi Azure Search (wersja zapoznawcza)](search-howto-index-json-blobs.md)



<!--HONumber=Nov16_HO3-->


