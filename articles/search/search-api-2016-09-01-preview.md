---
title: "2016 wersja interfejsu API REST usługi Azure Search-09-01-Preview | Dokumentacja firmy Microsoft"
description: "Azure wyszukiwania usługi REST wersja interfejsu API 2016-09-01-Preview obejmuje eksperymentalne funkcje, takie jak synonimy i moreLikeThis wyszukiwania."
services: search
documentationcenter: na
author: mhko
manager: jlembicz
editor: 
ms.assetid: 3dba3bf8-9c83-42f6-82bc-04727bd11037
ms.service: search
ms.devlang: rest-api
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: search
ms.date: 10/25/2017
ms.author: nateko
ms.openlocfilehash: 082c207f892fcc277d30d66c6165dd9920d3ab27
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-search-service-rest-api-version-2016-09-01-preview"></a>Interfejsu API REST usługi Azure Search: Wersji 2016-09-01-Preview
W tym artykule jest dokumentacji dla `api-version=2016-09-01-Preview`. Ta wersja zapoznawcza rozszerza bieżącej wersji ogólnie dostępna, [interfejsu api-version = 2016-09-01](https://msdn.microsoft.com/library/dn798935.aspx), podając następujące funkcje eksperymentalne:

* [Synonimy interfejsu API](search-synonyms.md) do przekazywania synonim map i rozszerzyć wyszukiwania.
* [`moreLikeThis`parametr zapytania](search-more-like-this.md) można znaleźć dokumenty, które mają zastosowanie do określonego dokumentu.

Sprawdź, czy docelowy wersja interfejsu API `api-version=2016-09-01-Preview` próby te funkcje eksperymentalne. Poniższy przykład przedstawia sposób Podgląd wersja interfejsu api jest określona w podejmowaniu więcej — podobne tego zapytania.

    GET https://[service name].search.windows.net/indexes/[index name]/docs?moreLikeThis=a1&api-version=2016-09-01-Preview

> [!NOTE]
> Funkcje w wersji zapoznawczej są dostępne do testowania i doświadczenia z celem zbierania opinii i mogą ulec zmianie. **Zdecydowanie odradzamy przy użyciu podglądu interfejsów API w aplikacji produkcyjnych.**

Usługa Azure Search jest dostępna w różnych wersjach. Zapoznaj się z [przechowywanie wersji usługi wyszukiwania](http://msdn.microsoft.com/library/azure/dn864560.aspx) szczegółowe informacje.
