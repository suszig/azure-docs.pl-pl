---
title: "Skonfigurować słownik biznesowy dla postanowieniom znakowanie w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "Artykule wyróżnianie słownik biznesowy w wykazie danych Azure do definiowania i za pomocą wspólnego słownika biznesowych do tagu zarejestrowanych zasobów danych."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: b3d63dbe-1ae7-499f-bc46-42124e950cd6
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 83ca3b2d89a335a5fd6dddeaca7c11f6d0492234
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-the-business-glossary-for-governed-tagging"></a>Skonfigurować słownik biznesowy dla postanowieniom znakowanie
## <a name="introduction"></a>Wprowadzenie
Wykaz danych Azure umożliwia odnajdywanie źródła danych, więc można łatwo odnaleźć i zrozumieć użycie źródeł danych, które należy wykonywać analizy i podejmować decyzje. Te możliwości utworzyć największy wpływ można odnaleźć i zrozumieć szerokiej gamy dostępnych źródeł danych.

Znakowanie jest jedną funkcję wykazu danych, które ułatwia lepsze zrozumienie zasobów danych. Za pomocą znakowania, słowa kluczowe można skojarzyć z zasobów lub kolumnę, która z kolei ułatwia odnajdywanie zasobów za pomocą wyszukiwania i przeglądania. Znakowanie pomaga również więcej łatwo zrozumieć kontekstu i celem elementu zawartości.

Jednak znakowanie czasami może spowodować problemy z własnych. Przykłady problemów, które mogą stać się znakowanie to:

* Używanie skrótów w niektórych zasobów i rozszerzonej tekstu w innym. Ta niezgodność przeszkadza odnajdywanie zasobów, nawet jeśli celem użycia jej było tagu zasoby z tym samym tagiem.
* Potencjalne zmiany znaczenia, w zależności od kontekstu. Na przykład tag o nazwie *przychodu* na klienta zestawu danych może to oznaczać przychodu przez klienta, ale sam znacznik na kwartał sprzedaży zestawu danych może oznaczać co kwartał przychodu firmy.  

Aby pomóc w pokonywaniu te i inne podobne wyzwania, Data Catalog zawiera słownik biznesowy.

Za pomocą usługi Data Catalog słownik biznesowy, organizacja może dokumentu warunki biznesowe klucza i ich definicje, aby utworzyć wspólnego słownika biznesowych. Ta ładu umożliwia spójności danych użycia w całej organizacji. Po zdefiniowaniu termin w słownik biznesowy, można go przypisać do zasobu danych w katalogu. Takie podejście, *postanowieniom znakowanie*, to te same podejście jako znakowanie.

## <a name="glossary-availability-and-privileges"></a>Słownik dostępności i uprawnień
Słownik biznesowy jest dostępna tylko w Standard Edition usługi Azure Data Catalog. Bezpłatna wersja wykazu danych nie zawiera słownik i nie ma możliwości dla której działalność znakowanie.

Można uzyskać dostęp za pośrednictwem słownik biznesowy **słownik** opcję w menu nawigacji portalu wykazu danych.  

![Uzyskiwanie dostępu do słownik biznesowy](./media/data-catalog-how-to-business-glossary/01-portal-menu.png)

Administratorzy katalogu danych i członkowie roli Administratorzy słownik można tworzenie, edytowanie i usuwanie terminów w słownik biznesowy. Wszyscy użytkownicy wykazu danych mogą wyświetlać definicje terminów i zasobów tagu z terminów.

![Dodawanie nowych słownik](./media/data-catalog-how-to-business-glossary/02-new-term.png)

## <a name="creating-glossary-terms"></a>Tworzenie terminów
Administratorzy katalogu danych i słownik można tworzyć terminów przez kliknięcie **nowy termin** przycisku. Każdy słownik zawiera następujące pola:

* Biznesowe definicję terminu
* Opis, który przechwytuje przeznaczenia lub reguł biznesowych dla zasobów lub kolumny
* Lista uczestników, którzy najczęściej o termin
* Termin nadrzędny, który definiuje hierarchii, w którym jest zorganizowana termin

## <a name="glossary-term-hierarchies"></a>Słownik terminów hierarchii
Za pomocą usługi Data Catalog słownik biznesowy, organizacji można opisać jego słownictwa firm jako hierarchię warunków, a następnie można utworzyć klasyfikację warunki lepiej reprezentuje jego taksonomii biznesowej.

Okres musi być unikatowa na danym poziomie hierarchii. Zduplikowane nazwy są niedozwolone. Nie ma żadnego limitu liczby poziomów w hierarchii, ale hierarchii jest często bardziej zrozumiały gdy istnieją trzy poziomy lub mniej.

Korzystanie z hierarchii w słownik biznesowy jest opcjonalne. Pozostawiając puste pole termin nadrzędnego słownik terminów tworzy listę płaski (z systemem innym niż — hierarchiczna) terminów w słowniku.  

## <a name="tagging-assets-with-glossary-terms"></a>Znakowanie zasobów z terminów
Po zdefiniowaniu terminów w katalogu, środowisko znakowania zasobów jest zoptymalizowany do wyszukiwania słownika, jak użytkownik wpisze tag. Portalu wykazu danych zostanie wyświetlona lista zgodnych terminów do wyboru. Jeśli użytkownik wybierze słownik z listy, warunku jest dodawana do zasobu jako tag (nazywany także tag słownik). Użytkownik może również wybrać do utworzenia nowego tagu, wpisując termin, który nie znajduje się w słowniku (nazywany także tag użytkownika).

![Oznakowanych zasobów danych z jednego użytkownika tagu i dwa tagi słownik](./media/data-catalog-how-to-business-glossary/03-tagged-asset.png)

> [!NOTE]
> Tagi użytkownika są jedynym typem tagu obsługiwane w bezpłatnej wersji wykazu danych.
>
>

### <a name="hover-behavior-on-tags"></a>Umieść kursor w tagi zachowanie
W portalu wykazu danych zachowania wizualnie distinct i znajdują się różne hover są dwa rodzaje znaczników. Po ustawieniu kursora tag użytkownika widać tekst znacznika i użytkownika lub użytkowników, którzy dodano tagu. Po ustawieniu kursora tag słownik umożliwia wyświetlenie definicji słownik terminów i łącza do otwarcia słownik biznesowy, aby wyświetlić pełną definicję terminu.

### <a name="search-filters-for-tags"></a>Filtry wyszukiwania tagów
Słownik tagów i tagi użytkownika są można wyszukiwać i zastosować je jako filtrów w polu wyszukiwania.

## <a name="summary"></a>Podsumowanie
Za pomocą słownik biznesowy Azure Data Catalog i znakowanie której działalność, który umożliwia, można zidentyfikować, zarządzanie i odnaleźć zasoby danych w sposób ciągły. Słownik biznesowy podwyższyć poziom uczenie się ich słownictwa firm przez członków organizacji. Słownik obsługuje również Przechwytywanie łatwy do rozpoznania metadanych, co ułatwia wykrywanie zasobów i zrozumienia.

## <a name="next-steps"></a>Następne kroki
* [Dokumentacja interfejsu API REST na operacje biznesowe słownik](https://msdn.microsoft.com/library/mt708855.aspx)
