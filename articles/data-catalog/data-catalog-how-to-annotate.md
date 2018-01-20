---
title: "Jak dodawać adnotacje do źródeł danych | Dokumentacja firmy Microsoft"
description: "Artykule wyróżnianie jak do adnotacji zasobów danych w wykazie danych Azure, w tym przyjaznych nazw, tagów, opisów i ekspertów."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 5a7e6bb2-863c-4eca-b614-1c814920d9ed
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: f9dd68380773e3cdb93cf1033ccd1b55d9f69305
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-annotate-data-sources"></a>Jak dodawać adnotacje do źródeł danych
## <a name="introduction"></a>Wprowadzenie
**Microsoft Azure Data Catalog** to usługa w chmurze pełni zarządzana, która służy jako system rejestracji i system odnajdowania źródeł danych przedsiębiorstwa. Innymi słowy Data Catalog jest pomaganie osób odnajdywania, zrozumienia i używania źródeł danych i pomaganie organizacjom pozwala zwiększyć wartość uzyskiwaną z istniejących danych. Po zarejestrowaniu źródła danych z wykazu danych metadanych jest kopiowany i indeksowania przez usługę, ale wątku nie zakończyć. Data Catalog pozwala użytkownikom na podać własne metadane opisowe — takie jak tagi — aby uzupełnić metadane wyodrębnione ze źródła danych i źródła danych bardziej zrozumiałe dla pracowników i opisy.

## <a name="annotation-and-crowdsourcing"></a>Adnotacja i crowdsourcing
Każdy ma opinii. I to jest to przydatne.
Wykaz danych rozpoznaje, że różni użytkownicy mają różnych perspektyw w źródłach danych przedsiębiorstwa i czy każdy z tych perspektywy może być przydatna. Rozważmy następujący scenariusz:

* Administrator systemu wie umowę dotyczącą poziomu usług dla serwerów i usług obsługujących źródła danych.
* Administrator bazy danych wie, harmonogram tworzenia kopii zapasowych dla każdej bazy danych i przetwarzania ETL dozwolonych systemu windows.
* Właściciela systemu wie procesu dla użytkowników zażądać dostępu do źródła danych.
* Steward danych wie, sposób mapowania zasoby i atrybutów w źródle danych do modelu danych przedsiębiorstwa.
* Analityk wie, jak dane są używane w ramach procesów biznesowych, które obsługuje on.

Każdy z tych perspektywy jest przydatna, a Data Catalog używa podejście do metadanych, które umożliwia każdą z nich przechwycone i używać w celu zapewnienia pełnego obrazu zarejestrowanych źródeł danych. Za pomocą portalu wykazu danych, każdy użytkownik można dodawać i edytować własne adnotacje, będąc możliwość wyświetlania adnotacje udostępnione przez innych użytkowników.

## <a name="different-types-of-annotations"></a>Różne typy adnotacji
Wykaz danych obsługuje następujące typy adnotacji:

| Adnotacja | Uwagi |
| --- | --- |
| Przyjazna nazwa |Przyjazne nazwy mogą być dostarczane na poziomie zasobów danych, aby zasobów danych, co ułatwia zrozumienie. Przyjazne nazwy są najbardziej przydatne, gdy nazwa obiektu źródłowego jest one niezrozumiałe, skróconej lub w przeciwnym razie bezużyteczne dla użytkowników. |
| Opis |Opisy można podać podczas zasobu danych i atrybutu / poziomy kolumny. Opisy są adnotacje dowolnych krótki tekst, które opisują użytkownika perspektywy na zasobu danych lub jej użycia. |
| Znaczników (tagów użytkownika) |Tagi mogą być dostarczane na zasobu danych i atrybutu / poziomy kolumny. Tagi użytkownika są zdefiniowane przez użytkownika etykiet, które mogą służyć do klasyfikowania atrybuty lub zasobów danych. |
| Tagi (Słownik znaczniki) |Tagi mogą być dostarczane na zasobu danych i atrybutu / poziomy kolumny. Słownik tagi są centralnego definiowania terminów, które może służyć do klasyfikowania zasobów danych lub atrybutów przy użyciu wspólnej taksonomii biznesowej. Aby uzyskać więcej informacji, zobacz [How to set up the Business Glossary for Governed Tagging](data-catalog-how-to-business-glossary.md) (Konfigurowanie słownika biznesowego na potrzeby znakowania zarządzanego). |
| Ekspertów |Eksperci mogą być dostarczane na poziomie zasobów danych. Ekspertów zidentyfikować użytkowników lub grup z perspektywy ekspertów danych i może służyć jako punkty kontaktu dla użytkowników, którzy źródeł danych zarejestrowanych i masz pytania, które nie są odbierane przez istniejących adnotacji. |
| Żądaj dostępu |Informacje o żądaniu dostępu mogą być dostarczane na poziomie zasobów danych. Te informacje są dla użytkowników, którzy nie jeszcze mają uprawnienia dostępu do źródła danych odnajdywania. Użytkownicy, można wprowadzić adres e-mail użytkownika lub grupę, która udziela dostępu do adresu URL procesu lub narzędzie, które użytkownicy muszą uzyskać dostęp, lub można wprowadzić sam proces jako tekst. |
| Dokumentacja |Dokumentacja mogą być dostarczane na poziomie zasobów danych. Dokumentacja zasobów jest informacji tekstu sformatowanego zawierające łącza i obrazki, a które zapewniają informacje nie są przekazywane za pośrednictwem opisy i tagów. |

## <a name="annotating-multiple-assets"></a>Dodawanie adnotacji do wielu zasobów
Podczas wybierania wielu zasobów danych w portalu wykazu danych, użytkownicy może dodawać adnotacje do wszystkich wybranych środków w ramach jednej operacji. Adnotacje będą stosowane do wszystkich wybranych zasobów, co ułatwia wybieranie i podaj opis spójne i zestawów tagów i ekspertów zasobów powiązanych danych w.

> [!NOTE]
> Znaczniki i ekspertów można również podać po narzędzia rejestracji źródła rejestrowania zasoby danych przy użyciu danych wykazu danych.
>
>

Po wybraniu wielu tabel i widoków, tylko kolumny, czy wszystkie wybrane dane, które są wspólne dla zasobów będą wyświetlane w portalu wykazu danych. Dzięki temu użytkownicy mogą podać znaczniki wraz z opisami dla wszystkich kolumn o takiej samej nazwie dla wszystkich wybranych zasobów.

## <a name="annotations-and-discovery"></a>Adnotacje i odnajdywania
Tak samo, jak metadanych wyodrębnione ze źródła danych podczas rejestracji zostanie dodany do usługi Data Catalog indeksu wyszukiwania, metadane dostarczone przez użytkownika są również indeksowane. To oznacza, że nie tylko adnotacje ułatwić użytkownikom zrozumienie, dane, które stwierdzonych, adnotacje również ułatwić użytkownikom odnaleźć zasoby danych adnotacjami wyszukiwanie przy użyciu terminów, które warto do nich.

## <a name="summary"></a>Podsumowanie
Rejestrowanie źródła danych z wykazu danych sprawia, że wykrywalny danych przez skopiowanie metadanych strukturalnych i opisowy ze źródła danych do usługi wykazu. Po zarejestrowaniu źródła danych, użytkownicy mogą podać adnotacje, aby ułatwić odnajdywanie i zrozumienie z portalu wykazu danych.

## <a name="see-also"></a>Zobacz także
* [Rozpoczynanie pracy z usługą Azure Data Catalog](data-catalog-get-started.md) samouczek krok po kroku szczegółowe informacje o tym, jak dodawać adnotacje do źródeł danych.
