---
title: "Rejestracja źródeł danych w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "W tym artykule omówiono sposób rejestrowania źródeł danych w wykazie danych Azure, w tym pól metadanych wyodrębnione podczas rejestracji."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: bab89906-186f-4d35-9ffd-61b1d903905d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/01/2017
ms.author: maroche
ms.openlocfilehash: 42d7fddd592b9cea8fbfa38bf39ca5def3542d66
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="register-data-sources-in-azure-data-catalog"></a>Rejestracja źródeł danych w wykazie danych Azure
## <a name="introduction"></a>Wprowadzenie
Wykaz danych Azure to usługa w chmurze pełni zarządzana, która służy jako system rejestracji i odnajdywania źródeł danych przedsiębiorstwa. Innymi słowy Data Catalog pomaga użytkownikom odnajdywania, zrozumienia i używania źródeł danych, a także pomaga organizacjom osiąganie większych zysków z ich istniejących danych. Pierwszym krokiem do wprowadzania wykrywalny przez wykaz danych źródła danych jest do zarejestrowania tego źródła danych.

## <a name="register-data-sources"></a>Rejestrowanie źródeł danych (Rejestrowanie źródeł danych)
Rejestracja to proces trwa wyodrębnianie metadanych ze źródła danych i kopiowanie danych do usługi Data Catalog. Dane pozostają w miejscu, w którym aktualnie się znajdują, i podlegają kontroli administratorów i zasadom obowiązującym w danym systemie.

Aby zarejestrować źródła danych, wykonaj następujące czynności:
1. W portalu Azure Data Catalog uruchomić narzędzia rejestracji źródła danych wykazu danych. 
2. Zaloguj się przy użyciu konta służbowego z tych samych poświadczeń usługi Azure Active Directory, których używasz do logowania do portalu.
3. Wybierz źródło danych, które chcesz zarejestrować.

Aby uzyskać bardziej szczegółowe informacje krok po kroku, zobacz [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md) samouczka.

Po źródła danych został zarejestrowany, katalogu śledzi lokalizacji i indeksuje jego metadanych. Użytkownicy mogą wyszukiwanie, przeglądanie i odnajdywanie źródła danych i następnie się z nim połączyć za pomocą aplikacji lub ich wybranych narzędzi za pomocą jego lokalizacji.

## <a name="supported-data-sources"></a>Obsługiwane źródła danych
Aby uzyskać listę aktualnie obsługiwanych źródeł danych, zobacz [DSR wykazu danych](data-catalog-dsr.md).

## <a name="structural-metadata"></a>Metadane strukturalne
Podczas rejestrowania źródła danych, narzędzie do rejestracji wyodrębnia informacje dotyczące struktury obiektów, którą wybierzesz. Te informacje jest określone jako metadane strukturalne.

Dla wszystkich obiektów to metadane strukturalne zawiera lokalizacji obiektu, dzięki czemu użytkownicy, którzy odnajdywanie danych umożliwia łączenie do obiektu w narzędziach klienckich wybranych przez nich informacji. Inne metadane strukturalne zawiera nazwę obiektu i typu i wpisz nazwę atrybutu/kolumny i danych.

## <a name="descriptive-metadata"></a>Metadane opisowe
Oprócz podstawowych metadane strukturalne wyodrębniona ze źródła danych narzędzia rejestracji źródła danych wyodrębnia metadane opisowe. SQL Server Analysis Services i SQL Server Reporting Services metadanych jest pobierana z właściwości opisu udostępniane przez te usługi. Dla programu SQL Server, wartości przy użyciu ms\_opis właściwości rozszerzonej jest wyodrębniana. W przypadku bazy danych Oracle narzędzia rejestracji źródła danych wyodrębnia kolumnie komentarze z wszystkie\_kartę\_widoku komentarze.

Oprócz metadanych opisowych, który został wyodrębniony z źródła danych użytkownicy mogą wprowadzać metadane opisowe przy użyciu narzędzia rejestracji źródła danych. Użytkownicy mogą dodawać tagi i mogą identyfikować ekspertów dla obiektów, jest zarejestrowany. Te metadane opisowe jest kopiowany do usługi Data Catalog wraz z metadane strukturalne.

## <a name="include-previews"></a>Obejmują podglądy
Domyślnie tylko metadane jest wyodrębnione ze źródła danych i skopiowane do usługi Data Catalog, ale opis, którego źródłem danych jest często łatwiejsza po wyświetleniu przykładowych danych, które zawiera.

Za pomocą narzędzia rejestracji źródła danych usługi Data Catalog, mogą obejmować Podgląd migawki danych w każdej tabeli i widoku, który jest zarejestrowany. Jeśli wybierzesz opcję dołączenia podglądy podczas rejestracji, narzędzie do rejestracji zawiera maksymalnie 20 rekordy z każdym tabel i widoków. Ta migawka zostaną skopiowane do katalogu wraz z metadanych strukturalnych i opisowy.

> [!NOTE]
> Szerokie tabele z dużą liczbą kolumn może mieć mniej niż 20 rekordy w ich podglądu.
>
>

## <a name="include-data-profiles"></a>Dołącz profile danych
Zgodnie z tym podglądy zapewniają cenne kontekst dla użytkowników, którzy wyszukiwania źródeł danych w katalogu danych, w tym profilu danych ułatwia zrozumienie źródeł odnalezionych danych.

Za pomocą narzędzia rejestracji źródła danych usługi Data Catalog, mogą obejmować dane profilu dla każdej tabeli i widoku, który jest zarejestrowany. Jeśli chcesz dołączyć profil danych podczas rejestracji, narzędzie do rejestracji zawiera zagregowanych danych statystycznych dotyczących danych w poszczególnych tabel i widoków, w tym:

* Liczba wierszy i rozmiar danych w obiekcie.
* Data ostatniej aktualizacji danych i schematu obiektu.
* Liczba rekordów wartości null i różne wartości w kolumnach.
* Minimum, maksimum średnią i odchylenie standardowe wartości kolumny.

Te statystyki są kopiowane do katalogu wraz z metadanych strukturalnych i opisowy.

> [!NOTE]
> Tekst i Data kolumny nie dołączaj statystyki średniej lub odchylenie standardowe w swoim profilu danych.
>
>

## <a name="update-registrations"></a>Aktualizacja rejestracji
Rejestrowanie źródła danych dzięki wykrywalny w wykazie danych podczas korzystania z metadanych i opcjonalnie Podgląd wyodrębnione podczas rejestracji. Jeśli źródło danych musi zostać zaktualizowany w katalogu (na przykład jeśli schematu obiektu został zmieniony, tabele pierwotnie wykluczone mają zostać uwzględnione lub do zaktualizowania danych, który znajduje się w wersji zapoznawczych), można ponownie uruchomić narzędzia rejestracji źródła danych.

Ponowne rejestrowanie źródła danych już zarejestrowany wykonuje operację "upsert" merge: istniejące obiekty są aktualizowane i nowych obiektów. Wszystkie metadane udostępniane użytkownikom za pomocą portalu wykazu danych zostaną zachowane.

## <a name="summary"></a>Podsumowanie
Ponieważ metadanych strukturalnych i opisowy jest kopiowane ze źródłem danych z usługą wykazu, rejestrowania źródła danych w katalogu danych ułatwia dane do odnalezienia i zrozumienia. Po zarejestrowaniu źródła danych, można dodawać adnotacje, zarządzanie i go odnaleźć za pomocą portalu wykazu danych.

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji na temat rejestrowania źródeł danych, zobacz [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md) samouczka.
