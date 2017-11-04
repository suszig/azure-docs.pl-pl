---
title: "Jak odnajdować źródła danych w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "W tym artykule omówiono sposób odnajdowania zarejestrowanych zasobów danych z wykazu danych Azure, w tym wyszukiwanie i filtrowanie przy użyciu trafień wyróżnienia możliwości portalu wykazu danych Azure."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: f72ae3a3-6573-4710-89a7-f13555e1968c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/01/2017
ms.author: maroche
ms.openlocfilehash: 7c2a1d61bc13ba1cde404e2b732933d2d314e3e3
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="how-to-discover-data-sources-in-azure-data-catalog"></a>Jak odnajdować źródła danych w usłudze Azure Data Catalog
## <a name="introduction"></a>Wprowadzenie
Wykaz danych Azure to usługa w chmurze pełni zarządzana, która służy jako system rejestracji i odnajdywania źródeł danych przedsiębiorstwa. Innymi słowy Data Catalog pomaga użytkownikom odnajdywania, zrozumienia i używania źródeł danych, a także pomaga organizacjom osiąganie większych zysków z ich istniejących danych. Po zarejestrowaniu źródła danych z wykazu danych jego metadane jest indeksowany przez usługę, dzięki czemu można łatwo wyszukiwać odnajdywanie potrzebnych danych.

## <a name="searching-and-filtering"></a>Wyszukiwanie i filtrowanie
Odnajdywania w wykazie danych używa dwóch mechanizmów podstawowego: wyszukiwanie i filtrowanie.

Wyszukiwanie zaprojektowano pod kątem intuicyjnej obsługi i zaawansowanych możliwości. Domyślnie wyszukiwane terminy są dopasowywane do wszystkich właściwości w wykazie, w tym adnotacji wprowadzonych przez użytkownika.

Filtrowanie służy jako uzupełnienie wyszukiwania. Można wybrać określone właściwości, takie jak ekspertów, typ źródła danych typu obiektu i tagów. Wyświetlanie tylko pasujących zasobów danych i ograniczyć wyniki wyszukiwania do dopasowania zasoby.

Przy użyciu kombinacji wyszukiwania i filtrowania, można szybko przejść źródeł danych, które zostały zarejestrowane w wykazie danych odnajdywania źródeł danych, które są potrzebne.

## <a name="search-syntax"></a>Wyszukiwanie w składni
Mimo że domyślne wyszukiwanie niezależnych proste i intuicyjne, umożliwia także składni wyszukiwania w wykazie danych uzyskać większą kontrolę nad wyniki wyszukiwania. Wyszukiwanie w wykazie danych obsługuje następujące techniki:

| Technika | Użycie | Przykład |
| --- | --- | --- |
| Wyszukiwanie podstawowe |Wyszukiwania podstawowego, który korzysta z jednego lub większej liczby terminów wyszukiwania. Wyniki są żadnych zasobów, które pasuje do żadnej właściwości z co najmniej jednego z określonych warunków. |`sales data` |
| Wyznaczanie zakresu właściwości |Zwróć tylko źródła danych, gdy wyszukiwany termin jest zgodny z określonej właściwości. |`name:finance` |
| Operatory logiczne |Poszerzają lub zawężają wyszukiwanie przy użyciu operacji logicznych. |`finance NOT corporate` |
| Grupowanie za pomocą nawiasów |Użyj nawiasów do grupowania części zapytania w celu uzyskania izolacji logicznej, szczególnie w połączeniu z operatorami logicznymi. |`name:finance AND (tags:Q1 OR tags:Q2)` |
| Operatory porównania |Użyj porównań innych niż równość dla właściwości, które mają numeryczne i datowe typy danych. |`modifiedTime > "11/05/2014"` |

Aby uzyskać więcej informacji na temat wyszukiwania w wykazie danych, zobacz [Azure Data Catalog](https://msdn.microsoft.com/library/azure/mt267594.aspx) artykułu.

## <a name="hit-highlighting"></a>Wyróżnianie trafień
Podczas wyświetlania wyników wyszukiwania, wszelkie wyświetlane są wyróżnione właściwości, które pasują do określonych kryteriów wyszukiwania (takich jak nazwa zasobów danych, opisie i tagach), aby była ułatwia zidentyfikowanie przyczyny zasobów podanych danych zwrócił podanym wyszukiwaniem.

> [!NOTE]
> Aby wyłączyć wyróżnianie trafień, należy użyć **zaznacz** przełącznika w portalu wykazu danych.
>
>

Podczas wyświetlania wyników wyszukiwania, może nie zawsze być oczywista Dlaczego zasobu danych są uwzględnione, nawet w przypadku wyróżnianie trafień włączone. Ponieważ wszystkie właściwości są przeszukiwane domyślnie, zasobu danych może być zwrócony z powodu dopasowania właściwości na poziomie kolumny. I ponieważ wielu użytkowników może dodawać adnotacje do zarejestrowanych zasobów danych z tagami i opisy, nie wszystkie metadane mogą być wyświetlane na liście wyników wyszukiwania.

Widok kafelków w domyślnym, zawiera każdego kafelka wyświetlane w wynikach wyszukiwania **widoku wyszukiwany termin jest zgodny** ikony, dzięki czemu można szybko wyświetlić liczbę dopasowań i ich lokalizacji oraz do szybkiego dostępu do nich, jeśli chcesz.

 ![Wyróżnianie trafień i wyszukiwanie ma uwzględniać w portalu wykazu danych Azure](./media/data-catalog-how-to-discover/search-matches.png)

## <a name="summary"></a>Podsumowanie
Ponieważ rejestrowania źródła danych za pomocą usługi Data Catalog kopiuje metadane strukturalne i opisowy ze źródła danych z usługą wykazu, źródła danych staje się łatwiejsze do odnalezienia i zrozumienia. Po źródła danych został zarejestrowany, można go odnaleźć za pomocą filtrowania i wyszukiwanie z poziomu portalu wykazu danych.

## <a name="next-steps"></a>Następne kroki
* Aby uzyskać szczegółowe informacje krok po kroku dotyczące odnajdywania źródeł danych, zobacz [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md).
