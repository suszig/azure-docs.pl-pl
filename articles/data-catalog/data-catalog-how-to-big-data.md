---
title: "Jak pracować z danych big data źródeł danych | Dokumentacja firmy Microsoft"
description: "Artykule wyróżnianie wzorców korzystania z danych big data źródeł danych, łącznie z magazynu obiektów Blob Azure, usługa Azure Data Lake i system plików HDFS Hadoop przy użyciu usługi Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 826676600094b956ff84cc88c61e667841043837
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Jak pracować ze źródłami danych big data w wykazie danych Azure
## <a name="introduction"></a>Wprowadzenie
**Microsoft Azure Data Catalog** to usługa w chmurze pełni zarządzana, która służy jako system rejestracji i system odnajdowania źródeł danych przedsiębiorstwa. Jest wszystkie o osobach myśl odnajdywania, zrozumienia i umożliwia źródła danych i pomaganie organizacjom osiąganie większych zysków z ich istniejących źródeł danych, włącznie z danymi big.

**Wykaz danych Azure** obsługuje rejestrację obiektów blob Azure blogu magazynu i katalogi, a także system plików HDFS Hadoop plików i katalogów. Częściowo ustrukturyzowanych natury tych źródeł danych zapewnia dużą elastyczność. Jednak można pobrać wartości najbardziej zarejestrować je za pomocą **Azure Data Catalog**, użytkowników należy wziąć pod uwagę organizowania źródeł danych.

## <a name="directories-as-logical-data-sets"></a>Katalogi jako logiczne zestawów danych
Typowe wzorzec służący do organizowania źródeł danych big data jest zaliczenie katalogów logicznej zestawów danych. Katalogi najwyższego poziomu są używane do definiowania zestawu danych, podczas podfoldery Definiuj partycje, a pliki, które zawierają przechowywać dane.

Przykładem tego wzorca może być:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

W tym przykładzie vehicle_maintenance_events i location_tracking_events reprezentują logiczne zestawów danych. Każdy z tych folderów zawiera pliki danych, które są zorganizowane według rok i miesiąc w podfolderach. Każdy z tych folderów potencjalnie może zawierać, setek lub tysięcy plików.

W tym wzorcu rejestrowania poszczególnych plików z **Azure Data Catalog** prawdopodobnie nie ma sensu. Zamiast tego należy zarejestrować katalogi, które reprezentują zestawów danych, które być przydatne do pracy z danymi użytkowników.

## <a name="reference-data-files"></a>Odwołanie do danych plików
Uzupełniające wzorzec jest do przechowywania zestawów danych referencyjnych jako pojedyncze pliki. Te zestawy danych mogą być uważane za "mała" strony danych big data, a często są podobne do wymiarów w modelu danych analitycznych. Odwołanie do danych plików zawiera rekordy, które są używane do zapewnienia kontekstu zbiorczego pliki danych przechowywanych w magazynie danych big data w innym miejscu.

Przykładem tego wzorca może być:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Pracując naukowca analityka lub dane z danymi zawartymi w większych struktur katalogów danych w tych plikach odwołania mogą służyć do zapewnienia bardziej szczegółowe informacje dla jednostek, które są określane tylko według nazwy lub Identyfikatora większy zestaw danych.

W tym wzorcu warto zarejestrować odwołania poszczególnych plików danych z **Azure Data Catalog**. Każdy plik reprezentuje zestaw danych, a każdej z nich może być adnotowany i odnalezione pojedynczo.

## <a name="alternate-patterns"></a>Alternatywne wzorce
Wzorce opisanych w poprzedniej sekcji są tylko dwa różne sposoby, które mogą być organizowane przechowywania danych big data, ale każda implementacja jest inna. Niezależnie od tego, jak strukturę źródeł danych, podczas rejestrowania źródła danych big data z **Azure Data Catalog**, skupić się na rejestracji pliki i katalogi, które reprezentują zbiory danych, które mają wartość innym użytkownikom w ramach sieci organizacja. Rejestrowanie wszystkich plików i katalogów można zajmowały katalogu, dzięki czemu przeszkodę użytkownikom znajdowanie potrzebnych informacji.

## <a name="summary"></a>Podsumowanie
Rejestrowanie źródeł danych z **Azure Data Catalog** łatwiej je do odnalezienia i zrozumienia. Rejestrowanie i dodawanie adnotacji do danych big data pliki i katalogi, które reprezentują logiczne zestawów danych, można ułatwić użytkownikom znalezienia i użycia potrzebnych im źródeł danych big data.
