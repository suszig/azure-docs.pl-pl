---
title: "Jak dokumentować źródła danych | Dokumentacja firmy Microsoft"
description: "Artykule wyróżnianie jak dokumentować zasobów danych w wykazie danych Azure."
services: data-catalog
documentationcenter: 
author: spelluru
manager: NA
editor: 
tags: 
ms.assetid: 053b1701-b848-4ada-b726-6f485caa9961
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: spelluru
ms.openlocfilehash: 1901ac534c46e4de0446543de4a40de040b8d51e
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="document-data-sources"></a>Dokumentowanie źródeł danych
## <a name="introduction"></a>Wprowadzenie
**Microsoft Azure Data Catalog** to usługa w chmurze pełni zarządzana, która służy jako system rejestracji i system odnajdowania źródeł danych przedsiębiorstwa. Innymi słowy **Azure Data Catalog** wszystkim chodzi o pomoc osób odnajdywania, *zrozumieć*i używać źródeł danych i który pomaga organizacjom osiąganie większych zysków z ich istniejących danych.

Jeśli źródło danych jest zarejestrowana w usłudze **Azure Data Catalog**, jego metadanych jest kopiowany i indeksowania przez usługę, ale wątku nie zakończyć. **Wykaz danych Azure** pozwala użytkownikom na ich własnych pełną dokumentację opisujące użycia i typowe scenariusze dla źródła danych.

W [jak dodawać adnotacje do źródeł danych](data-catalog-how-to-annotate.md), możesz dowiedzieć się, że ekspertów znających źródła danych może dodawać adnotacje do go z tagami i opis. **Azure Data Catalog** portal zawiera edytor tekstu sformatowanego, dzięki czemu użytkownicy mogą pełni dokumentu zasobów danych i kontenerów. Edytor obejmuje formatowanie akapitów, takich jak nagłówki, formatowania tekstu, listy punktowane, numerowane i tabele.

Znaczniki i opisy są bardzo proste adnotacji. Jednak aby konsumenci danych lepiej zrozumieć użycie źródła danych i scenariuszy biznesowych dla źródła danych, eksperta zapewniają pełne, szczegółowe dokumentacji. To proste dokumentować źródła danych. Wybierz kontener lub zasobów danych, a następnie wybierz pozycję **dokumentacji**.

![](media/data-catalog-documentation/data-catalog-documentation.png)

## <a name="documenting-data-assets"></a>Dokumentowanie zasobów danych
Zaletą **Azure Data Catalog** dokumentacji umożliwia użycie wykazu danych jako repozytorium zawartości można utworzyć pełną udostępniono elementów zawartości danych. Można sprawdzić szczegółowe zawierającego opis kontenery i tabele. Jeśli masz już zawartości w innej repozytorium zawartości, takich jak SharePoint lub udziału plików, można dodać łącza dokumentacji zasobów do odwołania do tego istniejącej zawartości. Ta funkcja umożliwia istniejące dokumenty mogą szybciej odnajdywać.

> [!NOTE]
> Dokumentacja nie jest uwzględniony w indeksie wyszukiwania.
>
>

![](media/data-catalog-documentation/data-catalog-documentation2.png)

Poziom dokumentacji mogą należeć do zakresu od opisu właściwości i wartość kontener zasobów danych, aby uzyskać szczegółowy opis schemat tabeli w kontenerze. Poziomu z dokumentacją dostarczoną powinien być regulowane przez potrzeb biznesowych. Jednak ogólnie rzecz biorąc, poniżej przedstawiono kilka zalet i wad dokumentowanie zasobów danych:

* Tylko kontenerem dokumentów: cała zawartość znajduje się w jednym miejscu, ale może być brak niezbędne informacje dla użytkowników podjąć świadomej decyzji.
* Dokument tylko tabele: zawartość jest specyficzne dla tego obiektu, ale użytkownicy korzystają z wielu miejscach dokumentów.
* Kontenery i tabele dokumentów: najbardziej kompleksowe rozwiązanie, ale może powodować więcej obsługi dokumentów.

## <a name="summary"></a>Podsumowanie
Dokumentowanie źródeł danych za pomocą **Azure Data Catalog** można utworzyć udostępniono o danych w stopniu szczegółowości.  Przy użyciu łącza, można połączyć się z zawartością przechowywaną w istniejących repozytorium zawartości, które połączono z istniejących dokumentów i zasobów danych. Gdy użytkownicy odnajdywania zasobów danych, mogą oni mieć kompletny zestaw dokumentacji.
