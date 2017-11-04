---
title: "Terminologia usługi Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera wprowadzenie do pojęć i terminów używanych w dokumentacji usługi Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6fec74d9-4a3c-4b4b-88ba-cad5ad143331
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/01/2017
ms.author: maroche
ms.openlocfilehash: 8cb24357bb24c48dceda714a040427fcc0c0ba4d
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-terminology"></a>Terminologia usługi Azure Data Catalog
## <a name="catalog"></a>Katalogu
Azure Data Catalog to repozytorium metadanych opartych na chmurze danych może być zarejestrowany zasoby oraz źródeł danych. Katalog służy jako lokalizację magazynu centralnego metadane strukturalne wyodrębnione ze źródeł danych oraz metadanych opisowych dodana przez użytkownika.

## <a name="data-source"></a>Źródło danych
Źródło danych jest system lub kontenera, który zarządza zasobów danych. Przykłady obejmują baz danych programu SQL Server, Oracle — bazy danych, bazy danych SQL Server Analysis Services, (wielowymiarowym lub tabelarycznym) i serwerów SQL Server Reporting Services.

## <a name="data-asset"></a>Zasobów danych
Zasoby danych są obiektów zawartych w źródeł danych, które mogą być rejestrowane w katalogu. Przykłady obejmują tabel programu SQL Server i widoków, Oracle tabel i widoków, SQL Server Analysis Services miar, wymiarów i wskaźników KPI i raportów usług SQL Server Reporting Services.

## <a name="data-asset-location"></a>Lokalizacja zasobów danych
Katalog przechowywana lokalizacja źródła danych lub zasobów danych, która może służyć do połączenia ze źródłem za pomocą aplikacji klienckiej. Format i szczegółowe informacje o lokalizacji różnić w zależności od typu źródła danych. Na przykład tabeli programu SQL Server można zidentyfikować za pomocą czterech części nazwy — nazwa serwera, nazwa bazy danych, nazwę schematu, nazwa obiektu —, podczas gdy raport usług raportowania serwera SQL może zostać zidentyfikowane na podstawie jego adresu URL.

## <a name="structural-metadata"></a>Metadane strukturalne
Metadane strukturalne jest metadanych wyodrębnione ze źródła danych, które opisano strukturę zasobów danych. W tym lokalizacji zasobów, jego nazwa obiektu i typu oraz dodatkowe właściwości określonego typu. Na przykład metadane strukturalne tabele i widoki zawiera nazwy i typy danych dla kolumny obiektu.

## <a name="descriptive-metadata"></a>Metadane opisowe
Metadane opisowe są metadane opisujące przeznaczenie lub celem zasobów danych. Zwykle opisowymi metadanymi jest dodawany przez użytkowników katalogu za pomocą portalu wykazu danych Azure, ale jego również można wyodrębnić ze źródła danych podczas rejestracji. Na przykład narzędzie rejestracji usługi Azure Data Catalog wyodrębnić opisy z właściwości Opis usług SQL Server Analysis Services i SQL Server Reporting Services i z [ms_description właściwość rozszerzona](https://technet.microsoft.com/library/ms190243.aspx) w bazach danych programu SQL Server, jeśli te właściwości są wypełnione wartościami.

## <a name="request-access"></a>Żądania dostępu
Metadane opisowe zasobu danych mogą zawierać informacje dotyczące żądania dostępu do zasobów danych ani źródła danych. Te informacje są prezentowane z lokalizacji zasobów danych i może zawierać co najmniej jeden z następujących opcji:

* Adres e-mail użytkownika lub zespół odpowiedzialny za udzielanie dostępu do źródła danych.
* Adres URL udokumentowane proces, który użytkownicy są zobowiązani do uzyskania dostępu do źródła danych.
* Adres URL tożsamościami i dostępem narzędzie do zarządzania (takich jak Microsoft Identity Manager) używany do uzyskania dostępu do źródła danych.
* Wpis niezależnej w tym artykule opisano, jak użytkownicy będą mogli uzyskać dostępu do źródła danych.

## <a name="preview"></a>Wersja zapoznawcza
Podgląd w usłudze Azure Data Catalog jest to migawka maksymalnie 20 rekordów, które mogą być wyodrębnione ze źródła danych podczas rejestracji i przechowywane w katalogu z metadanymi zasobów danych. Podgląd może pomóc użytkowników, którzy odnajdywania zasobów danych lepiej zrozumieć jego funkcji i celów. Innymi słowy wyświetlać dane przykładowe przydatne może być więcej niż zobaczenia tylko nazwy kolumn i typy danych.
Podglądy są obsługiwane tylko dla tabel i widoków, a musi być w sposób jawny wybrany przez użytkownika podczas rejestracji.

## <a name="data-profile"></a>Dane profilu
Profil danych w usłudze Azure Data Catalog jest migawką tabeli i na poziomie kolumny metadanych dotyczących zasobów zarejestrowanych danych, która może być wyodrębnione ze źródła danych podczas rejestracji i przechowywane w katalogu z metadanymi zasobów danych. Może pomóc w profilu danych użytkowników, którzy odnajdywania zasobów danych lepiej zrozumieć jego funkcji i celów. Podobne do wersji zapoznawczych, profile danych musi być jawnie wybrane przez użytkownika podczas rejestracji.

> [!NOTE]
> Wyodrębnianie profilu danych może być kosztowna operacja dla dużych tabel i widoków i może znacznie zwiększyć czas wymagany do rejestrowania źródła danych.
>
>

## <a name="user-perspective"></a>Perspektywy użytkownika.
W wykazie danych Azure dowolny użytkownik zapewnia metadane opisowe dla zasobu zarejestrowanych danych. Każdy użytkownik ma różne perspektywą danych i jego użycia. Na przykład administrator odpowiedzialny za serwer może Podaj szczegóły umowy dotyczącej poziomu usług (SLA) lub kopia zapasowa systemu windows; steward danych może zapewnić linki do dokumentacji w firmie przetwarza obsługuje danych; i analityka może podać opis warunków, które są najbardziej odpowiednie do innych analityków i może być najbardziej przydatna dla tych użytkowników, którzy potrzebują do odnalezienia i zrozumienia danych.

Każdy z perspektywy te są z założenia cenne, a w wykazie danych Azure każdy użytkownik może udostępnić informacje, które są przydatne do nich, podczas wszyscy użytkownicy mogą używać tych informacji zrozumieć dane, a jej celem.

## <a name="expert"></a>Ekspert
Ekspert jest użytkownik, który został zidentyfikowany jako mający świadomych perspektywy "ekspertów" dla zasobu danych. Każdy użytkownik, można dodać siebie lub inny użytkownik jako ekspert dla zasobu. Są wyświetlane jako eksperta nie daje żadnych dodatkowych uprawnień w wykazie danych Azure; Umożliwia użytkownikom łatwe lokalizowanie tych perspektywy, które są najbardziej mogą być użyteczne podczas przeglądania metadane opisowe zasobów.

## <a name="owner"></a>Właściciel
Właściciel jest użytkownik, który ma dodatkowe uprawnienia do zarządzania zasobu danych w wykazie danych Azure. Użytkownicy mogą przejąć prawo własności zarejestrowanych zasobów danych i właścicieli można dodać innym użytkownikom jako współwłaściciele. Aby uzyskać więcej informacji, zobacz [jak zarządzać zasobami danych](data-catalog-how-to-manage.md)  

> [!NOTE]
> Prawo własności i zarządzania są dostępne tylko w Standard Edition usługi Azure Data Catalog.
>
>

## <a name="registration"></a>Rejestracja
Czynność wyodrębniania danych zasobów metadanych źródła danych oraz skopiować go do usługi Azure Data Catalog jest rejestracja. Następnie można adnotacji i odnalezionych zasobów danych, które zostały zarejestrowane.

## <a name="see-also"></a>Zobacz też
* [Co to jest usługa Azure Data Catalog?](data-catalog-what-is-data-catalog.md) — Ten artykuł zawiera omówienie usługi Azure Data Catalog, wartość, która zapewnia i scenariusze, które obsługuje.
* [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md) — w tym artykule przedstawiono samouczek end-to-end, pokazujący, jak używać usługi Azure Data Catalog odnajdywanie źródła danych.  
