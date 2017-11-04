---
title: "Jak źródeł danych profilu danych"
description: "Artykule wyróżnianie sposobu uwzględniania poziomie tabeli i kolumny dane profilów rejestrowania źródeł danych w usłudze Azure Data Catalog i sposobu używania profilów danych, aby zrozumieć źródeł danych."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 94a8274b-5c9c-4962-a4b1-2fed38a3d919
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/01/2017
ms.author: spelluru
ms.openlocfilehash: 848d244a2a6c3420d1a11005e1b02f5671d7912a
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="data-profile-data-sources"></a>Źródła danych profilu danych
## <a name="introduction"></a>Wprowadzenie
**Microsoft Azure Data Catalog** to usługa w chmurze pełni zarządzana, która służy jako system rejestracji i system odnajdowania źródeł danych przedsiębiorstwa. Innymi słowy **Azure Data Catalog** wszystkim chodzi o pomaga osób odnajdywania, zrozumienia i używania źródeł danych oraz organizacjom osiąganie większych zysków z ich istniejących danych. Jeśli źródło danych jest zarejestrowana w usłudze **Azure Data Catalog**, jego metadanych jest kopiowany i indeksowania przez usługę, ale wątku nie zakończyć.

**Danych profilowania** funkcji **Azure Data Catalog** sprawdza dane z obsługiwanych źródeł danych w katalogu i zbiera dane statystyczne i informacje o tych danych. Jest łatwy do profil danych. Podczas rejestrowania zasobów danych, wybierz **obejmują dane profilu** w narzędzia rejestracji źródła danych.

## <a name="what-is-data-profiling"></a>Co to jest danych profilowania
Dane profilowania sprawdza danych w źródle danych, jest zarejestrowany i zbiera dane statystyczne i informacje o tych danych. Podczas odnajdowania źródła danych te statystyki może pomóc w określeniu przydatności danych, aby rozwiązać problemy biznesowe.

<!-- In [How to discover data sources](data-catalog-how-to-discover.md), you learn about **Azure Data Catalog's** extensive search capabilities including searching for data assets that have a profile. See [How to include a data profile when registering a data source](#howto). -->

Następujące źródła danych obsługuje dane profilowania:

* SQL Server (łącznie z bazy danych SQL Azure i usługi Azure SQL Data Warehouse) tabel i widoków
* Oracle tabele i widoki
* Teradata tabele i widoki
* Tabele programu hive

Takie jak profile danych podczas rejestrowania zasobów danych ułatwia użytkownikom odpowiedzi na pytania dotyczące źródeł danych, takich jak:

* Czy można używać do rozwiązywania problemów z firmą?
* Danych jest zgodna z określonym standardów lub wzorce?
* Jakie są anomalii źródła danych?
* Co to są potencjalnych problemów o włączenie tych danych do mojej aplikacji?

> [!NOTE]
> Można również dodać dokumentacji do zasobu opisujących sposób danych może być zintegrowane z aplikacji. Zobacz [jak dokumentować źródła danych](data-catalog-how-to-documentation.md).
>
>

<a name="howto"/>

## <a name="how-to-include-a-data-profile-when-registering-a-data-source"></a>Włączanie profilu danych podczas rejestrowania źródła danych
Jest łatwy do profil źródła danych. Podczas rejestrowania źródła danych w **obiekty do zarejestrowania** panelu Narzędzia rejestracji źródła danych, wybierz **obejmują dane profilu**.

![](media/data-catalog-data-profile/data-catalog-register-profile.png)

Aby dowiedzieć się więcej na temat rejestrowania źródeł danych, zobacz [rejestrowania źródeł danych](data-catalog-how-to-register.md) i [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md).

## <a name="filtering-on-data-assets-that-include-data-profiles"></a>Filtrowanie zasobów danych, które dotyczą profili danych
Aby odnaleźć zasoby danych, które obejmują profilu danych, można dołączyć `has:tableDataProfiles` lub `has:columnsDataProfiles` jako jeden z warunkami wyszukiwania.

> [!NOTE]
> Wybieranie **obejmują dane profilu** w źródle danych narzędzia rejestracji zawiera tabelę i informacje o profilu na poziomie kolumny. Jednak interfejsu API Data Catalog pozwala zasoby danych mają być zarejestrowany tylko jeden zestaw uwzględnione informacje o profilu.
>
>

## <a name="viewing-data-profile-information"></a>Wyświetlanie informacji o profilu danych
Po znalezieniu źródła danych odpowiednie za pomocą profilu można wyświetlić szczegóły danych profilu. Aby wyświetlić dane profilu, zaznacz zasobu danych i wybierz polecenie **profilu danych** w oknie portalu wykazu danych.

![](media/data-catalog-data-profile/data-catalog-view.png)

Dane profilu w **Azure Data Catalog** zawiera tabela i kolumna profilu informacje w tym:

### <a name="object-data-profile"></a>Obiekt danych profilu
* Liczba wierszy
* Rozmiar tabeli
* Podczas ostatniej aktualizacji obiektu

### <a name="column-data-profile"></a>Kolumny danych profilu
* Typ danych kolumny
* Liczba wartości odrębnych
* Liczba wierszy z wartości NULL
* Minimalna, maksymalna, średnia i odchylenie standardowe dla wartości w kolumnie

## <a name="summary"></a>Podsumowanie
Dane profilowania zawiera dane statystyczne i informacje o zarejestrowanych zasobów danych, aby pomóc w określeniu przydatność danych do rozwiązywania problemów biznesowych. Wraz z adnotacji i dokumentowanie źródeł danych, danych profilów można umożliwić użytkownikom lepiej zrozumieć dane.

## <a name="see-also"></a>Zobacz też
* [Jak rejestrować źródła danych](data-catalog-how-to-register.md)
* [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md)
