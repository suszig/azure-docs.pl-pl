---
title: "Tworzenie zapytań względem indeksu usługi Azure Search przy użyciu witryny Azure Portal | Microsoft Docs"
description: "Generowanie zapytań wyszukiwania w Eksploratorze wyszukiwania witryny Azure Portal."
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a23372112e17703a3399e1bdc9eaf73b85a1f80d


---
# <a name="query-your-azure-search-index-using-the-azure-portal"></a>Tworzenie zapytań względem indeksu usługi Azure Search przy użyciu witryny Azure Portal
> [!div class="op_single_selector"]
> * [Omówienie](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

W tym przewodniku opisano sposób tworzenia zapytań kierowanych do indeksu usługi Azure Search w witrynie Azure Portal.

Przed rozpoczęciem pracy z tym przewodnikiem należy [utworzyć indeks usługi Azure Search](search-what-is-an-index.md) i [wypełnić go danymi](search-what-is-data-import.md).

## <a name="i-go-to-your-azure-search-blade"></a>I. Przejdź do bloku usługi Azure Search
1. W menu po lewej stronie w witrynie [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) kliknij pozycję „Wszystkie zasoby”
2. Wybierz swoją usługę Azure Search

## <a name="ii-select-the-index-you-would-like-to-search"></a>II. Wybierz indeks, który chcesz przeszukać
1. Z kafelka „Indeksy” wybierz indeks, który chcesz przeszukać.

![](./media/search-explorer/pick-index.png)

## <a name="iii-click-on-the-search-explorer-tile"></a>III. Kliknij kafelek „Eksplorator wyszukiwania”
![](./media/search-explorer/search-explorer-tile.png)

## <a name="iii-start-searching"></a>III. Rozpocznij wyszukiwanie
1. Aby przeszukać indeks usługi Azure Search, zacznij pisać w polu „ *Ciąg zapytania* ”, a następnie naciśnij przycisk „**Wyszukaj**”.
   
   * Podczas używania Eksploratora wyszukiwania możesz określić dowolne [parametry zapytania](https://msdn.microsoft.com/library/dn798927.aspx)
2. Wyniki zapytania będą wyświetlane w sekcji „ *Wyniki* ” jako nieprzetworzone dane JSON, które będą odbierane w treści odpowiedzi HTTP podczas generowania żądań wyszukiwania do interfejsu API REST usługi Azure Search.
3. Ciąg zapytania jest automatycznie przekształcany do odpowiedniego adresu URL żądania w celu przesłania żądania HTTP do interfejsu API REST usługi Azure Search

![](./media/search-explorer/search-bar.png)




<!--HONumber=Nov16_HO2-->


