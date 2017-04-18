---
title: "Wprowadzenie do usługi Azure Data Catalog | Microsoft Docs"
description: "W tym artykule omówiono usługę Microsoft Azure Data Catalog i jej funkcje oraz problemy, które ma ona rozwiązywać. Usługa Data Catalog oferuje możliwości dostępne dla każdego użytkownika — od analityków, przez analityków danych, do deweloperów — służące do rejestrowania, odnajdywania, zrozumienia i używania źródeł danych."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: cc733907-17ec-4153-9f0c-5b3754b2db19
ms.service: data-catalog
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 04/06/2017
ms.author: maroche
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: fb8f43f5bb5725da30e67cdf5d7b066fe40ed003
ms.lasthandoff: 04/07/2017


---
# <a name="what-is-azure-data-catalog"></a>Co to jest usługa Azure Data Catalog?
Azure Data Catalog to w pełni zarządzana usługa w chmurze umożliwiająca użytkownikom odnajdywanie potrzebnych im źródeł danych i zrozumienie ich działania. Usługa ta ułatwia także organizacjom osiąganie większych zysków z ich istniejących inwestycji. Usługa Data Catalog oferuje możliwości dostępne dla każdego użytkownika — od analityków, przez analityków danych, do programistów — służące do odnajdywania, zrozumienia i używania źródeł danych. Usługa Data Catalog zawiera model crowdsourcingu w postaci metadanych oraz adnotacji i umożliwia wszystkim użytkownikom dzielenie się swoją wiedzą w celu tworzenia społeczności i kultury danych.

## <a name="discovery-challenges-for-data-consumers"></a>Wyzwania dotyczące odnajdywania dla konsumentów danych
Tradycyjnie wykrywanie firmowych źródeł danych było organicznym procesem opartym na wiedzy pochodzącej z wewnątrz firmy. Stawiało to wiele wyzwań przed firmami, które chciały uzyskać najlepsze efekty podczas pracy ze swoimi zasobami informacyjnymi.

* Użytkownicy nie dowiedzą o istnieniu źródeł danych, dopóki nie zaczną ich używać w ramach innego procesu — nie ma centralnej lokalizacji, w której źródła danych są rejestrowane.
* Jeśli użytkownik nie zna lokalizacji źródła danych, nie może nawiązać połączenia z danymi za pomocą aplikacji klienckiej. Środowisko konsumowania danych wymaga, aby użytkownik znał parametry połączenia lub ścieżkę.
* Jeśli użytkownik nie zna lokalizacji dokumentacji źródła danych, nie ma informacji o zamierzonych sposobach użycia danych. Źródła danych i dokumentacja znajdują się w różnych miejscach i są używane w różnych środowiskach.
* Jeśli użytkownik ma pytania dotyczące zasobu informacyjnego, musi zlokalizować specjalistę lub zespół odpowiedzialny za te dane i skontaktować się z nimi w trybie offline. Nie istnieje jawne połączenie między danymi i z użytkownikami korzystającymi z perspektywy ekspertów.
* Jeśli użytkownik nie ma wiedzy na temat procesu żądania dostępu do źródła danych, odnajdywanie źródła danych i jego dokumentacji nadal nie umożliwia mu dostępu do wymaganych danych.

## <a name="discovery-challenges-for-data-producers"></a>Wyzwania dotyczące odnajdywania dla producentów danych
Gdy konsumenci danych stawiają czoła tym wyzwaniom, użytkownicy odpowiedzialni za tworzenie i utrzymywanie zasobów informacyjnych mają inne wyzwania.

* Dodawanie adnotacji do źródła danych z opisowymi metadanymi jest często niepotrzebnym wysiłkiem; aplikacje klienckie zwykle ignorują opisy przechowywane w źródle danych.
* Tworzenie dokumentacji dla źródeł danych również jest często niepotrzebnym wysiłkiem — utrzymywanie synchronizacji dokumentacji ze źródłem danych stanowi ciągły proces, a użytkownicy wykazują brak zaufania do dokumentacji ze względu na to, że jest ona często postrzegana jako nieaktualna.
* Nieustanne wyzwanie stanowi również ograniczanie dostępu do źródła danych i zapewnienie, że konsumenci danych wiedzą, w jaki sposób zarządzać do nich dostępu.

Tworzenie dokumentacji źródła danych i jej utrzymywanie to złożony i czasochłonny proces. Częściej nawet większym wyzwaniem jest zapewnienie łatwości dostępu do dokumentacji dla wszystkich użytkowników źródła danych.

Po połączeniu wszystkie te wyzwania stanowią dużą przeszkodę dla przedsiębiorstw, które zachęcają i promują używanie danych firmowych i zrozumienie ich.

## <a name="azure-data-catalog-can-help"></a>Usługa Azure Data Catalog może pomóc
Usługa Data Catalog została zaprojektowana po to, aby rozwiązywać powyższe problemy i umożliwić przedsiębiorstwom osiągnięcie jak największych korzyści z istniejących zasobów informacyjnych. Usługa Data Catalog pomaga rozwiązywać problemy dzięki ułatwieniu odnajdywania źródeł danych i pomocy w ich rozumieniu przez użytkowników potrzebujących danych, którymi zarządzają.

Usługa Data Catalog udostępnia usługę w chmurze, w której można zarejestrować źródło danych. Dane pozostają w istniejącej lokalizacji, ale kopia metadanych jest dodawana do usługi Data Catalog wraz z odwołaniem do lokalizacji źródła danych. Te metadane są również indeksowane, aby można było łatwo je odnaleźć za pomocą wyszukiwania oraz uczynić je zrozumiałymi dla użytkowników, które je odnajdą.

Po zarejestrowaniu źródła danych jego metadane mogą zostać następnie wzbogacone przez użytkownika, który wykonał rejestrację, lub przez innych użytkowników w przedsiębiorstwie. Każdy użytkownik może dodawać adnotacje do źródła danych, podając opisy, tagi lub inne metadane, takie jak dokumentacja i procesy służące do żądania dostępu do źródła danych. Te metadane opisowe uzupełniają metadane strukturalne (takie jak nazwy kolumn i typy danych) zarejestrowane ze źródła danych.

Wykrywanie źródeł danych i ich zrozumienie to główny cel rejestrowania źródeł. Gdy użytkownicy w przedsiębiorstwie potrzebują danych do pracy (co może dotyczyć analizy biznesowej, projektowania aplikacji, analizy danych lub wszystkich innych zadań, w ramach których wymagane są odpowiednie dane), mogą użyć środowiska odnajdywania usługi Data Catalog, aby szybko znaleźć dane zgodne ze swoimi potrzebami, dokonać analizy danych w celu określenia, czy są one odpowiednie do realizowanego celu, oraz użyć danych przez otwarcie źródła danych w wybranym narzędziu. Jednocześnie usługa Data Catalog pozwala użytkownikom współtworzyć zawartość dzięki tworzeniu tagów i dokumentacji oraz dodawaniu adnotacji do źródeł danych, które zostały już zarejestrowane, a także dzięki rejestrowaniu nowych źródeł danych, które można odnaleźć, zrozumieć i użyć w ramach społeczności użytkowników wykazu.

![Możliwości usługi Data Catalog](./media/data-catalog-what-is-data-catalog/data-catalog-capabilities.png)

## <a name="get-started-with-data-catalog"></a>Wprowadzenie do usługi Data Catalog
Aby poznać możliwości usługi Data Catalog już dziś, odwiedź witrynę sieci Web [www.azuredatacatalog.com](https://www.azuredatacatalog.com).

Przewodnik z wprowadzeniem jest dostępny [tutaj](data-catalog-get-started.md).

## <a name="learn-more-about-data-catalog"></a>Dowiedz się więcej na temat usługi Data Catalog
Aby dowiedzieć się więcej o możliwościach wykazu danych, zobacz:

* [Jak rejestrować źródła danych](data-catalog-how-to-register.md)
* [Jak odnajdować źródła danych](data-catalog-how-to-discover.md)
* [Jak dodawać adnotacje do źródeł danych](data-catalog-how-to-annotate.md)
* [Jak dokumentować źródła danych](data-catalog-how-to-documentation.md)
* [Jak nawiązywać połączenie ze źródłami danych](data-catalog-how-to-connect.md)
* [Jak pracować z danymi big data](data-catalog-how-to-big-data.md)
* [Jak zarządzać zasobami danych](data-catalog-how-to-manage.md)
* [Jak skonfigurować słownik biznesowy](data-catalog-how-to-business-glossary.md)
* [Często zadawane pytania](data-catalog-frequently-asked-questions.md)

