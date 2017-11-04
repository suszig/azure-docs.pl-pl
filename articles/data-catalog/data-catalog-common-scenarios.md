---
title: Typowe scenariusze Azure Data Catalog | Dokumentacja firmy Microsoft
description: "Przegląd typowe scenariusze dotyczące usługi Azure Data Catalog, łącznie z rejestracji i odnajdywania źródeł danych wysokiej wartości, włączanie samoobsługowa analiza biznesowa i przechwytywanie istniejących informacji na temat źródeł danych i procesów."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 60930d78-d2d4-4d5d-9651-bdda50b0da0e
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 10/15/2017
ms.author: maroche
ms.openlocfilehash: 8f333b96165144eb46e730a7a1dff52d064532bb
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/03/2017
---
# <a name="azure-data-catalog-common-scenarios"></a>Azure Data Catalog common scenarios (Typowe scenariusze dla usługi Azure Data Catalog)
W tym artykule przedstawiono typowe scenariusze, w którym Azure Data Catalog może pomóc zwiększyć wartość uzyskiwaną z jego istniejących źródeł danych organizacji.

## <a name="scenario-1-registration-of-central-data-sources"></a>Scenariusz 1: Rejestracja źródeł danych w centralnej
Organizacje często mają wiele źródeł danych wysokiej wartości. Te źródła danych zawierają biznesowych online transakcji przetwarzania systemów (OLTP), magazynów danych i baz danych analizy/analiz biznesowych. Liczba systemów i nakładanie się między nimi, zwykle zwiększa rozmiar w czasie rozwoju potrzeb biznesowych i rozwoju firmy się za pośrednictwem, na przykład fuzje i przejęcia.

Może być trudne dla członków organizacji wiedzieć, gdzie można znaleźć danych w ramach tych źródeł danych. Wszystkie zbyt popularne są pytania podobne do poniższych:

* Trzy HR systemów używane w firmie, które należy użyć do utworzenia tego typu raportu?
* Gdzie powinien przejść do pobrania certyfikowanych numery sprzedaży roku podatkowym właśnie zakończono?
* Kto I zapytaj, czy co to jest proces, który należy użyć do uzyskania dostępu do magazynu danych?
* Nie wiadomo, czy numery te są prawidłowe. Kto można I uzyskać szczegółowe informacje o na jak te dane mają zostać użyta przed I udostępniania tego pulpitu nawigacyjnego z mojego zespołu?

Do tych i innych pytań usługa Azure Data Catalog można podać odpowiedzi. Źródła danych centralnej, wysokiej wartości, dział IT, które są używane w organizacji są często logicznej punkt początkowy podczas wypełniania wykazu. Mimo że każdy użytkownik może zarejestrować źródła danych, o kick-started ze źródłami danych, które są najbardziej prawdopodobną podania wartości do największej liczby użytkowników wykazu pomaga wspomóc przyjęcie i korzystania z systemu. 

Jeśli rozpoczęciem pracy z usługą Azure Data Catalog, zidentyfikowanie i zarejestrowanie najważniejszych źródeł danych używanych przez wiele różnych zespołów konsumenci danych może być pierwszym krokiem do osiągnięcia sukcesu.

Ten scenariusz przedstawia również możliwość dodawać adnotacje do źródeł danych wysokiej wartości, aby łatwiej zrozumieć i uzyskać dostęp. Jeden kluczowym aspektem tej pracy jest zawierają informacje, w jaki sposób użytkownicy mogą żądać dostępu do źródła danych. W wykazie danych Azure musisz podać adres e-mail użytkownika lub zespół, który jest odpowiedzialny za kontrolowanie dostępu do źródła danych, linki do istniejących narzędzi i dokumentacji lub dowolny tekst opisujący procesu żądania dostępu. Informacje te pomagają elementów członkowskich, który odnajdywanie zarejestrowanych źródeł danych, ale który jeszcze nie masz uprawnień dostępu do danych, aby łatwo uzyskać dostęp przy użyciu procesów, które są zdefiniowane i kontrolowane przez właścicieli źródła danych.

## <a name="scenario-2-self-service-business-intelligence"></a>Scenariusz 2: Samoobsługowa analiza biznesowa
Mimo że tradycyjnych rozwiązań analizy biznesowej firmy nadal nieoceniony część krajobrazów danych w wielu organizacjach, zmiany w tempie firm wprowadził samoobsługi BI bardziej ważne. Przy użyciu samodzielnego analizy Biznesowej, pracownicy przetwarzający informacje i analitycy można utworzyć własne raporty, skoroszyty i pulpity nawigacyjne bez polegania na centralnej zespół IT lub jest ograniczone przez harmonogram tego zespołu IT i dostępności.

W scenariuszach samoobsługi BI użytkownicy często połączyć dane z wielu źródeł, z których wiele może nie była wcześniej używana do analizy Biznesowej i analiz. Mimo że niektóre z tych źródeł danych może już być znane, może być wyzwaniem, aby dowiedzieć się, co zrobić, aby zlokalizować i ocena potencjalne źródła danych dla danego zadania.

Zazwyczaj ten proces odnajdywania jest ręczne: analityków umożliwia zidentyfikowanie osoby, które pracować z danymi poszukiwanego elementu równorzędnego połączenia sieciowego. Po źródła danych zostanie odnaleziony i jest używany, proces powtarza się ponownie dla każdego kolejnych samoobsługi BI wysiłku, z wieloma użytkownikami wykonywania nadmiarowe ręczny proces odnajdowania.

W wykazie danych Azure organizacja może przerwać ten cykl wysiłku. Po odnalezieniu źródła danych za pośrednictwem tradycyjnych środków, analityka można zarejestrować go, aby był łatwo wykrywalny przez innych użytkowników w przyszłości. Chociaż analityka można dodać więcej wartości przez dodawanie adnotacji do zarejestrowanych zasobów danych, ta adnotacja nie musi odbywać się w tym samym czasie jako rejestracji. Użytkownicy może przyczynić się wraz z upływem czasu, jako ich zezwolenie harmonogramy stopniowo dodanie wartości do źródeł danych zarejestrowanych w wykazie.

Tego organicznym rozwój zawartości wykazu jest fizyczną uzupełnienie początkowych rejestracji źródła danych centralnej. Wstępne wypełnianie katalogu z danymi, które są potrzebne w przypadku wielu użytkowników może być motivator początkowego użycia i odnajdywania. Umożliwienie użytkownikom się zarejestrować i adnotacje dodatkowych źródeł może być sposób chronić je i innych członków organizacji zaangażowane.

Warto zauważyć, że chociaż w tym scenariuszu skupiono się w szczególności na BI samoobsługi, te same wzorców i wyzwania dotyczą na dużą skalę firmowej BI projektów również. Za pomocą usługi Data Catalog, organizacja może zwiększyć wszelkie działania polega na ręczny proces odnajdowania źródła danych.

## <a name="scenario-3-capturing-tribal-knowledge"></a>Scenariusz 3: Przechwycenie "plemiennej" wiedzy
Jak sprawdzić, jakie dane należy wykonać swoją pracę i gdzie można znaleźć danych?

Jeśli jesteś w swojej pracy przez pewien czas, prawdopodobnie tylko wiedzieć. Już przeszli stopniowego uczenia procesu, a w czasie uzyskanych o źródłach danych, które są klucza w codziennej pracy.

Gdy pracownika dołącza zespołu, jak tej osoby sprawdzić, jakie dane są wymagane dla zadania i gdzie można znaleźć go?

Prawdopodobieństwo są nowej osoby dostarczany za pomocą tych pytań.

To przeniesienie trwającą "plemiennej" wiedzy jest częścią procesu odnajdywania źródła danych w organizacjach dużych i małych. Utworzone więcej członków zespołu wyższych i doświadczonym wiedzy całościowo i nowszej członków zespołu uzyskanych poprosić, gdy mają oni pytania. Najbardziej najważniejsze informacje często istnieje tylko w głowic kilka kluczowych pracowników i osób na urlopie lub pozostaw zespołu, wystąpi organizacji.

Ekspertów danych należy zwykle działań zmierzających do dokumentu wiedzą, udostępnianie go za pośrednictwem poczty e-mail lub w dokumentach programu Word w witrynie programu SharePoint team. Chociaż takie podejście może być przydatna, podaj nowy problem odnajdywania: jak osoby się dowiedzieć, jakie dokumentacji istnieje i gdzie można znaleźć go?

W wykazie danych Azure Twoja organizacja ma pojedynczej, centralnej lokalizacji do przechowywania i udostępniania wiedzy pochodzącej z wewnątrz firmy i dokonywania ułatwieniu. W wykazie danych ekspertów z danych bezpośrednio adnotacji zasobów danych i udostępniają linki do dokumentacji istniejących. Jeśli członków organizacji korzystają z katalogu, aby odnaleźć źródła danych, ich znajdują się nie tylko źródła sam, ale także wiedzy, które wcześniej istniały tylko u ekspertów z danej organizacji.
