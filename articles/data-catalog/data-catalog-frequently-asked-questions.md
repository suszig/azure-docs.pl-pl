---
title: "Wykaz danych Azure — często zadawane pytania | Dokumentacja firmy Microsoft"
description: "Często zadawane pytania dotyczące usługi Azure Data Catalog, w tym funkcji Odnajdywanie źródła danych, adnotacja i zarządzania."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5c7e209a-458c-4bb4-96bb-7ed178f9528a
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 11/13/2017
ms.author: maroche
ms.openlocfilehash: d1fd6b0bd47188570553e0e3ad1de527a35e98f6
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/14/2017
---
# <a name="azure-data-catalog-frequently-asked-questions"></a>Wykaz danych Azure — często zadawane pytania
Ten artykuł zawiera odpowiedzi na często zadawane pytania dotyczące usługi Azure Data Catalog.

## <a name="what-is-azure-data-catalog"></a>Co to jest usługa Azure Data Catalog?
Wykazu danych to w pełni zarządzana usługa, hostowanej na platformie Microsoft Azure, która służy jako system rejestracji i odnajdywania źródeł danych przedsiębiorstwa. Data Catalog każdego użytkownika, od analityków, do programistów i analityków danych, można zarejestrować, odnajdywanie zrozumieć i używania źródeł danych.

## <a name="what-customer-challenges-does-it-solve"></a>Jaką klient będzie wymagał robi to rozwiązuje?
Wykazu danych adresów wyzwania źródło danych odnajdywania i "ciemny dane", dzięki czemu użytkownicy mogą odnaleźć i zrozumieć źródeł danych przedsiębiorstwa.

## <a name="what-are-its-target-audiences"></a>Jakie są jej docelowi?
Data Catalog jest przeznaczona dla użytkowników Pomoc techniczna i nietechniczna, w tym:

* Dane deweloperów i specjalistów BI i analiza: odpowiedzialnego za utworzenie danych i ich analiza zawartość innym użytkownikom korzystać osoby.
* Dane stewards: osób, które mają wiedzą na temat danych, co oznacza i jak jest przeznaczona do użycia.
* Konsumenci danych: osób, które muszą być w stanie łatwo wykryć, zrozumieć i nawiąż połączenie z danymi potrzebują do wykonania zadania, przy użyciu narzędzia do siebie.
* Centralna IT: osoby, który należy setki źródeł danych wykrywalny przez użytkowników biznesowych i muszą zachować nadzoru nad sposobu używania danych i przez kogo.

## <a name="what-is-its-availability-by-region"></a>Co to jest jego dostępność według regionu?
Usługi wykazu danych są obecnie dostępne w następujących centrach danych:

* Zachodnie stany USA
* Wschodnie stany USA
* Europa Zachodnia
* Europa Północna
* Australia Wschodnia
* Azja Południowo-Wschodnia

## <a name="what-are-its-limits-on-the-number-of-data-assets"></a>Jakie są jej ograniczenia liczby zasobów danych?
Bezpłatna wersja Data Catalog jest ograniczona do 5000 zarejestrowanych zasobów danych.

Standard Edition Data Catalog obsługuje do 100 000 zarejestrowanych zasobów danych.

Dowolnego obiektu zarejestrowany w wykazie danych, takich jak tabele, liczby widoków, plików i raportów, jako zasobów danych.

## <a name="what-are-its-supported-data-source-and-asset-types"></a>Co to są typy źródła i zasobów jego obsługiwane dane?
Aby uzyskać listę aktualnie obsługiwanych źródeł danych, zobacz [DSR wykazu danych](data-catalog-dsr.md).

## <a name="how-do-i-request-support-for-another-data-source"></a>Jak zażądać pomocy technicznej przez inne źródło danych?
Aby przesłać żądania funkcji i inne opinie, przejdź do [wykaz danych na forum opinii Azure](https://feedback.azure.com/forums/906052-data-catalog/category/320788-data-sources).

## <a name="how-do-i-get-started-with-data-catalog"></a>Jak rozpocząć pracę z wykazu danych?
To najlepszy sposób, aby rozpocząć pracę, przechodząc do [wprowadzenie do korzystania z usługi Data Catalog](data-catalog-get-started.md). W tym artykule jest na trasie przegląd możliwości usługi.

## <a name="how-do-i-register-my-data"></a>Jak zarejestrować dane?
Aby zarejestrować dane w katalogu danych:
1. W portalu Azure Data Catalog w **publikowania** obszaru, uruchom narzędzie rejestracji usługi Azure Data Catalog. 
2. W narzędziu rejestracji źródła danych usługi Data Catalog Zaloguj się przy użyciu tych samych poświadczeń umożliwia dostęp do portalu wykazu danych.
3. Wybierz źródło danych i określonych zasobów, które chcesz zarejestrować.

## <a name="what-properties-does-it-extract-for-data-assets-that-are-registered"></a>Właściwości, które powoduje ona rozpakowania zasobów danych, które są zarejestrowane w?
Określone właściwości różnią się od źródła danych do źródła danych, ale ogólnie rzecz biorąc, Usługa publikowania w wykazie danych wyodrębnia następujące informacje:

* Nazwa zasobu
* Typ zasobu
* Opis elementu zasobów
* Nazwy atrybutu/kolumny
* Typy danych atrybutu/kolumny
* Opis atrybutu/kolumny

> [!IMPORTANT]
> Rejestrowania zasobów danych za pomocą usługi Data Catalog nie Przenieś lub skopiuj dane do chmury. Rejestrowanie zasobów ze źródła danych kopiuje metadane zasobów na platformie Azure, ale dane pozostają w istniejącej lokalizacji źródła danych. Wyjątkiem od tej reguły jest, jeśli chcesz przekazać rekordy Podgląd lub profilu danych podczas rejestrowania zasoby. Jeśli dołączysz podglądu, maksymalnie 20 rekordów zostały skopiowane z poszczególnych zasobów i przechowywane jako migawka w wykazie danych. Po dołączeniu profilu danych agregują informacje jest obliczana i zawarte w metadanych, które są przechowywane w katalogu. Łączny informacje mogą uwzględniać rozmiar tabel, procent wartości null w kolumnie lub wartości minimalne, maksymalne i średnie dla kolumn. 
>
>

> [!NOTE]
> Dla źródeł danych, takich jak SQL Server Analysis Services ma najwyższej jakości **opis** właściwości, narzędzia rejestracji źródła danych usługi Data Catalog wyodrębnia wartości tej właściwości. Relacyjnych baz danych programu SQL Server, w której brakuje najwyższej jakości **opis** właściwości narzędzia rejestracji źródła danych usługi Data Catalog wyodrębnianie wartości z **ms_description** rozszerzone właściwości obiekty i kolumn. Aby uzyskać więcej informacji, zobacz [przy użyciu właściwości rozszerzone w obiektach bazy danych](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx).
>
>

## <a name="how-long-should-it-take-for-newly-registered-assets-to-appear-in-the-catalog"></a>Jak długo powinien upłynąć do nowo zarejestrowanych zasobów pojawią się w katalogu?
Po zarejestrowaniu zasoby z wykazu danych może być przez okres 5 – 10 sekund, zanim pojawią się w portalu wykazu danych.

## <a name="how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>Jak dodawać adnotacje i uzupełnić metadane dla moich zarejestrowanych zasobów danych?
Najprostszym sposobem zapewnienia metadanych zarejestrowanych zasobów jest wybierz element zawartości, w portalu wykazu danych, a następnie wprowadź wartości w okienku właściwości lub okienku schematu dla wybranego obiektu.

Można też podać niektóre metadane, takie jak ekspertów i tagi, podczas procesu rejestracji. Wartości podane w Usługa publikowania w wykazie danych mają zastosowanie do wszystkich zasobów jest zarejestrowany w tym czasie. Zaznacz, aby wyświetlić obiekty ostatnio zarejestrowany w portalu dla dodatkowych adnotacji **Wyświetl Portal** przycisk na ekranie końcowym narzędzia rejestracji źródła danych wykazu danych.

## <a name="how-do-i-delete-my-registered-data-objects"></a>Jak usunąć dane zarejestrowane obiekty?
Można usunąć obiektu z usługi Data Catalog zaznaczając obiekt, w portalu, a następnie klikając polecenie **usunąć** przycisku. Usuwanie obiektu usuwa metadanych z wykazu danych, ale nie ma wpływu na źródle danych.

## <a name="what-is-an-expert"></a>Co to jest eksperta?
Ekspert jest osoba, która ma perspektywy informacji dotyczących obiektu danych. Każdy obiekt może mieć wielu ekspertów. Ekspert musi być "właściciela" dla obiektu, ale jest po prostu kogoś, kto wie jak dane i powinna być używana.

## <a name="how-do-i-share-information-with-the-data-catalog-team-if-i-encounter-problems"></a>Jak udostępnić informacje z zespołem usługi Data Catalog I wystąpienia problemów?
Zgłaszanie problemów, udostępniać informacje i zadać pytania, przejdź do [forum usługi Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).

## <a name="does-the-catalog-work-with-another-data-source-that-im-interested-in"></a>Katalog działa z innego źródła danych, które interesuje mnie?
Aktywnie pracujemy nad dodanie większej liczby źródeł danych w wykazie danych. Jeśli chcesz Zobacz określonego źródła danych obsługiwane, sugerować go (lub głosowych działem pomocy technicznej, jeśli została już sugerowane), przechodząc do [wykaz danych na forum opinii Azure](https://feedback.azure.com/forums/906052-data-catalog).

## <a name="how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>Jak Azure Data Catalog powiązany jest Data Catalog w usłudze Power BI dla Office 365?
Azure Data Catalog można traktować jako ewolucji wykaz danych w usłudze Power BI. Począwszy od spring 2017 r Azure Data Catalog służy do Włącz udostępnianie i odnajdywanie zapytań w programie Excel 2016 i dodatku Power Query dla programu Excel. Możliwości usługi Data Catalog w programie Excel są dostępne dla użytkowników z licencjami usługi Power BI Pro.

## <a name="what-permissions-do-i-need-to-register-assets-with-data-catalog"></a>Jakie uprawnienia należy zarejestrować zasobów za pomocą usługi Data Catalog?
Aby uruchomić narzędzie rejestracji usługi Data Catalog, musisz mieć uprawnienia na źródło danych umożliwia odczytanie metadanych ze źródła. Aby dołączyć również Podgląd, musi mieć uprawnienia, które można odczytać danych z obiektów jest zarejestrowany.

Wykaz danych umożliwia także administratorom wykazu ograniczyć, którzy użytkownicy i grupy może dodawać metadane do katalogu. Aby uzyskać dodatkowe informacje, zobacz [jak bezpieczny dostęp do katalogu danych i zasobów danych](data-catalog-how-to-secure-catalog.md).

## <a name="will-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>Wykaz danych będą dostępne dla lokalnego wdrożenia?
Wykaz danych to usługa w chmurze, którego można używać zarówno w chmurze, jak i dla lokalnego źródła danych do dostarczania hybrydowego odnajdywanie źródła danych. Obecnie nie ma żadnych planów dla wersji usługi Data Catalog, która uruchamia lokalnie.

## <a name="can-i-extract-more-or-richer-metadata-from-the-data-sources-i-register"></a>Czy można wyodrębnić metadanych więcej lub bardziej rozbudowane, ze źródeł danych, które można zarejestrować?
Aktywnie pracujemy nad rozszerzyć możliwości usługi Data Catalog. Jeśli chcesz mieć dodatkowe metadane wyodrębnione ze źródła danych podczas rejestracji, sugerować go (lub głos go, jeśli została już sugerowane) w [wykaz danych na forum opinii Azure](https://feedback.azure.com/forums/906052-data-catalog). 

Jeśli chcesz uwzględnić kolumny/schematu metadanych, podglądy lub danych profilów dla źródeł danych, w którym metadanych nie jest wyodrębniany przez narzędzia rejestracji źródła danych, można użyć interfejsu API Data Catalog do dodania tych metadanych. Aby uzyskać dodatkowe informacje, zobacz [interfejsu API REST usługi Azure Data Catalog](https://docs.microsoft.com/rest/api/datacatalog/).

## <a name="how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>Jak ograniczyć widoczność zarejestrowanych zasobów danych, tak aby tylko wybrani użytkownicy może wykrywać je?
Wybierz zasoby danych w wykazie danych, a następnie kliknij przycisk **Przejmij na własność** przycisku. Właściciele zasobów danych w katalogu danych można zmienić ustawień widoczności albo zezwalająca wszystkim użytkownikom ograniczyć widoczność do określonych użytkowników lub odnajdywać zasoby należące do firmy. Aby uzyskać dodatkowe informacje, zobacz [zarządzać zasobami danych w usłudze Azure Data Catalog](data-catalog-how-to-manage.md).

## <a name="how-do-i-update-the-registration-for-a-data-asset-so-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>Jak zaktualizować rejestracji dla zasobów danych, tak aby zmiany w źródle danych są uwzględniane w katalogu?
Aby zaktualizować metadane dla zasobów danych, które są już zarejestrowane w katalogu, po prostu ponownie zarejestrować źródła danych, który zawiera zasoby. Żadnych zmian w źródle danych, takich jak kolumny są dodane lub usunięte z tabel lub widoków, są aktualizowane w katalogu, ale adnotacji dostarczone przez użytkowników są przechowywane.

## <a name="my-question-isnt-answered-here-where-can-i-go-for-answers"></a>Mojego pytania nie jest tutaj odpowiedzi. Gdzie można przejść do odpowiedzi?
Przejdź do [forum usługi Azure Data Catalog](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409). Monitów zostanie ustalone, sposób ich tutaj.
