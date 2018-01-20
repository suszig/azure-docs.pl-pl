---
title: "Zarządzać zasobami danych w usłudze Azure Data Catalog | Dokumentacja firmy Microsoft"
description: "Artykuł prezentuje sposób kontrolowania widoczności i własności zasobów danych zarejestrowane w usłudze Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 623f5ed4-8da7-48f5-943a-448d0b7cba69
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/18/2018
ms.author: maroche
ms.openlocfilehash: 5a4b2b5734bf8bfbbc45a65b02362d1fa37b1a87
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/19/2018
---
# <a name="manage-data-assets-in-azure-data-catalog"></a>Zarządzać zasobami danych w wykazie danych Azure
## <a name="introduction"></a>Wprowadzenie
Azure Data Catalog jest przeznaczona dla źródła danych odnajdywania, dzięki czemu można łatwo odnaleźć i zrozumieć użycie źródeł danych, które należy wykonywać analizy i podejmować decyzje. Te możliwości odnajdywania należy największy wpływ, gdy inni użytkownicy mogą odnaleźć i zrozumieć szerokiej gamy dostępnych źródeł danych. Z tych elementów, pamiętając domyślne zachowanie usługi Data Catalog jest dla wszystkich źródeł danych zarejestrowanych były widoczne i wykrywalny przez wszystkich użytkowników w katalogu.

Wykaz danych nie uzyskania dostępu do samych danych. Dostęp do danych jest kontrolowany przez właściciela źródła danych. Data Catalog można odnaleźć źródła danych i wyświetlać metadane dotyczące źródeł, które są zarejestrowane w wykazie.

Mogą wystąpić sytuacje, jednak gdzie źródeł danych tylko powinny być widoczne, dla określonych użytkowników lub do członków określonych grup. W takich sytuacjach użytkownicy mogą przejąć na własność zasobów danych zarejestrowanych w wykazie i następnie ustawić widoczność zasobów, w których są właścicielami.

> [!NOTE]
> Funkcje opisane w tym artykule jest dostępna tylko w Standard Edition usługi Azure Data Catalog. Bezpłatna wersja nie udostępnia możliwości posiadania i ograniczenie widoczność zasobów danych.
>
>

## <a name="manage-ownership-of-data-assets"></a>Zarządzanie własności zasobów danych
Domyślnie nieposiadanej są zasobów danych, które są zarejestrowane w wykazie danych. Każdy użytkownik z uprawnieniami, aby uzyskać dostęp do katalogu mogą odnaleźć i Adnotuj tych zasobów. Użytkownicy mogą przejąć na własność zasobów danych go i następnie ograniczyć widoczność zasobów, w których są właścicielami.

Gdy zasobu danych w katalogu danych jest właścicielem, tylko użytkownicy uwierzytelnieni właściciele mogą odnaleźć zasobu i wyświetlić jej metadane, a tylko właściciele można usunąć elementu zawartości z katalogu.

> [!NOTE]
> Własność w wykazie danych dotyczy tylko metadane są przechowywane w katalogu. Własność nie przyznaje wszystkie uprawnienia w źródle danych.
>
>

### <a name="take-ownership"></a>Przejmij na własność
Użytkownicy mogą przejąć prawo własności zasobów danych, wybierając **Przejmij na własność** opcji w portalu wykazu danych. Aby przejąć na własność zasobów danych nieposiadanej są wymagane żadne specjalne uprawnienia. Każdy użytkownik może przejąć na własność zasobów danych go.

### <a name="add-owners-and-co-owners"></a>Dodaj właścicieli i współwłaścicieli
Jeśli ma już właściciela zasobów danych, innych użytkowników nie wystarczy przejąć na własność. Muszą zostać dodane jako współwłaściciele przez istniejące właściciela. Wszelkie właściciela można dodać dodatkowych użytkowników lub grup zabezpieczeń jako współwłaściciele.

> [!NOTE]
> Jest najlepszym rozwiązaniem ma co najmniej dwóch osób jako właściciele żadnych zasobów należących do danych.
>
>

### <a name="remove-owners"></a>Usuń właścicieli
Podobnie jak wszelkie właściciel zasobu może dodawać współwłaścicieli, wszelkie właściciel zasobu można usunąć wszelkie współwłaściciel.

Właściciel zasobu, która usuwa go lub siebie jako właściciela nie może dłużej zarządzać elementu zawartości. Jeśli właściciel zasobu usuwa go lub siebie jako właściciela, nie ma żadnych wspólnej właścicieli zasobu przekształcenia go do stanu.

## <a name="control-visibility"></a>Widoczność formantu
Właściciele danych zasobów można kontrolować widoczność zasobów danych, w których są właścicielami. Aby ograniczyć widoczność jako domyślny, w której wszyscy użytkownicy wykazu danych może odnalezienia i wyświetlenia zasobów danych, właściciel zasobu może zmienić ustawienie widoczności z **wszyscy** do **właściciele i następujący użytkownicy** we właściwościach elementu zawartości. Następnie można dodać właścicieli, konkretnych użytkowników i grup zabezpieczeń.

> [!NOTE]
> Jeśli to możliwe, uprawnienia własności i widoczności zasobu powinny być przypisane do grup zabezpieczeń, a nie do poszczególnych użytkowników.
>
>

## <a name="catalog-administrators"></a>Administratorzy katalogu
Administratorzy katalogu danych są niejawnie współwłaściciele wszystkich zasobów w katalogu. Właściciele zawartości nie można usunąć widoczność administratorów, a administratorzy mogą zarządzać własności i widoczności dla wszystkich zasobów danych w katalogu.

## <a name="summary"></a>Podsumowanie
Model crowdsourcingu w postaci usługi Data Catalog do odnajdywania zasobów metadane i dane zezwala wszystkim użytkownikom katalogu przyczyniają się do odnajdywania. Standard Edition Data Catalog jest przeznaczona dla własności i zarządzania, aby ograniczyć widoczność oraz korzystanie z zasobów określonych danych.
