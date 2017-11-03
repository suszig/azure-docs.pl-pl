---
title: "Azure DB rozwiązania Cosmos jako wartość klucza magazynu — omówienie koszt | Dokumentacja firmy Microsoft"
description: "Więcej informacji o niskich kosztów korzystania z bazy danych Azure rozwiązania Cosmos jako magazyn wartości klucza."
keywords: "magazyn kluczy i wartości"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: 
documentationcenter: 
ms.assetid: 7f765c17-8549-4509-9475-46394fc3a218
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: mimig
ms.openlocfilehash: 33eef1b51a5ee00b0fa67096030ed9ce92cf768e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-cosmos-db-as-a-key-value-store--cost-overview"></a>Azure DB rozwiązania Cosmos jako magazyn wartości klucza — koszt — omówienie

Azure DB rozwiązania Cosmos jest usługą bazy danych globalnie rozproszone i wiele modeli łatwe tworzenie aplikacji wysokiej dostępności, dużej skali. Domyślnie program Azure DB rozwiązania Cosmos automatycznie indeksuje wszystkie dane, które go wysyła strumień, wydajnie. Umożliwia to szybkie i spójne [SQL](documentdb-sql-query.md) (i [JavaScript](programming.md)) kwerendy dla każdego typu danych. 

Tym artykule opisano koszt bazy danych Azure rozwiązania Cosmos do zapisu proste i operacje odczytu, gdy jest używany jako magazyn kluczy i wartości. Zapisu operacji zalicza się wstawienia, zastępuje, usuwa i upserts dokumentów. Oprócz zapewnienia wysokiej dostępności 99,99%, gwarantowane oferty Azure DB rozwiązania Cosmos < opóźnieniem 10 ms dla odczytów i < 15 ms opóźnień (indeksowane), zapisuje na 99-ty percentyl. 

## <a name="why-we-use-request-units-rus"></a>Dlaczego używamy jednostek żądania (RUs)

Azure wydajności bazy danych rozwiązania Cosmos jest oparta na ilość elastycznie [jednostek żądania](request-units.md) (RU) dla partycji. Udostępnianie w drugiego stopnia szczegółowości i została zakupiona RUs na sekundę i RUs na minutę ([nie należy mylić z rozliczeniami co godzinę](https://azure.microsoft.com/pricing/details/cosmos-db/)). RUs należy traktować jako walutę upraszczającym udostępniania przepływności wymagane dla aplikacji. Klientów nie trzeba myśleć o rozróżnianie między odczytu i zapisu pojemności jednostki. Model jednej waluty RUs tworzy wydajności, aby udostępnić elastycznie pojemności między odczyty i zapisy. Ten model elastycznie pojemności umożliwia usłudze zapewniają przewidywalną i spójny przepływności gwarancji, małych opóźnień i wysokiej dostępności. Na koniec używamy RU produktywnością modelu, ale każdy elastycznie RU również ma zdefiniowanych ilość zasobów (pamięć, Core). RU/s nie jest tylko IOPS.

Jako system globalnie rozproszoną bazę danych DB rozwiązania Cosmos jest tylko Azure usługa, która zapewnia SLA na opóźnienia, przepływności i spójności oprócz wysokiej dostępności. Przepływność, które należy udostępnić jest stosowany do poszczególnych regionów skojarzonych z Twoim kontem bazy danych DB rozwiązania Cosmos. Dla odczytów, DB rozwiązania Cosmos oferuje wiele dobrze zdefiniowany [poziomy spójności](consistency-levels.md) dla można wybrać. 

Poniższa tabela przedstawia liczbę RUs wymaganych do wykonania odczytu i zapisu na podstawie rozmiaru dokumentu o rozmiarze 1 KB i 100KBs transakcji.

|Rozmiar elementu|Odczyt 1|1 zapisu|
|-------------|------|-------|
|1 KB|1 RU|5 RUs|
|100 KB|10 RUs|50 RUs|

## <a name="cost-of-reads-and-writes"></a>Koszt odczyty i zapisy

Jeśli dostarczasz 1 000 RU/s, ta ilość 3,6 m RU na godzinę i będzie koszt $0.08 godzinę (w Stanach Zjednoczonych i Europie). Rozmiar dokumentu o rozmiarze 1KB, to oznacza, że będzie można korzystać z odczyty 3,6 m lub zapisuje 0,72 m (3.6mRU / 5) przy użyciu sieci udostępnionej przepływności. Znormalizowane do milionów odczyty i zapisy, kosztów byłoby odczyty /m 0,022 $ (0.08 $ / 3,6) i zapisuje 0.111 $/ m (0.08 $ / 0,72). Koszt milionów staje się minimalny, jak pokazano w poniższej tabeli.

|Rozmiar elementu|1 mln odczytu|1 mln zapisu|
|-------------|-------|--------|
|1 KB|$0.022|$0.111|
|100 KB|$0.222|$1.111|


Większość podstawowych obiektów blob lub obiekt Magazyny usług opłatą $0,40 na milionów transakcji odczytu i 5 USD na transakcję milionów zapisu. Jeśli używane optymalnie, rozwiązania Cosmos bazy danych może być 98% tańszy niż te rozwiązania, (dla transakcji o rozmiarze 1KB).

## <a name="next-steps"></a>Następne kroki

Wkrótce nowe artykuły dotyczące optymalizacji Inicjowanie obsługi zasobów bazy danych Azure rozwiązania Cosmos. W międzyczasie możesz użyć naszych [Kalkulator RU](https://www.documentdb.com/capacityplanner).

