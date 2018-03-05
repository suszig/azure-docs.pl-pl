---
title: "Ograniczenia dotyczące bazy danych platformy Azure dla PostgreSQL"
description: "W tym artykule opisano ograniczeń w bazie danych Azure PostgreSQL, takie jak liczba połączeń i opcje aparatu magazynu."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: ba05308039e9743dd207333476e61a45c0ca166a
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Ograniczenia dotyczące bazy danych platformy Azure dla PostgreSQL
Baza danych Azure PostgreSQL usługi jest w wersji zapoznawczej. W poniższych sekcjach opisano pojemności i limity funkcjonalności usługi bazy danych.

## <a name="pricing-tier-maximums"></a>Maksymalne wartości warstwy cenowej
Bazy danych platformy Azure dla PostgreSQL ma wiele warstw cenowych, które można wybrać podczas tworzenia serwera. Aby uzyskać więcej informacji, zobacz [warstw cenowych w bazie danych Azure dla PostgreSQL](concepts-pricing-tiers.md).  

Istnieje maksymalna liczba połączeń, jednostek obliczeniowych i magazynu w każdej warstwy cenowej w następujący sposób: 

|Warstwa cenowa| Generowanie obliczeniowe| vCore(s)| Maksymalna liczba połączeń |
|---|---|---|---|
|Podstawowa| Gen 4| 1| 50 |
|Podstawowa| Gen 4| 2| 100 |
|Podstawowa| Gen 5| 1| 50 |
|Podstawowa| Gen 5| 2| 100 |
|Ogólne zastosowanie| Gen 4| 2| 150|
|Ogólne zastosowanie| Gen 4| 4| 250|
|Ogólne zastosowanie| Gen 4| 8| 480|
|Ogólne zastosowanie| Gen 4| 16| 950|
|Ogólne zastosowanie| Gen 4| 32| 1500|
|Ogólne zastosowanie| Gen 5| 2| 150|
|Ogólne zastosowanie| Gen 5| 4| 250|
|Ogólne zastosowanie| Gen 5| 8| 480|
|Ogólne zastosowanie| Gen 5| 16| 950|
|Ogólne zastosowanie| Gen 5| 32| 1500|
|Pamięć| Gen 5| 2| 150|
|Pamięć| Gen 5| 4| 250|
|Pamięć| Gen 5| 8| 480|
|Pamięć| Gen 5| 16| 950|
|Pamięć| Gen 5| 32| 1900|

Gdy połączenia przekracza limit, może zostać wyświetlony następujący błąd:
> Błąd krytyczny: Niestety, jeszcze zbyt wielu klientów

Azure system wymaga pięciu połączeń do monitorowania PostgreSQL serwera bazy danych Azure. 

## <a name="functional-limitations"></a>Ograniczenia funkcjonalności
### <a name="scale-operations"></a>Operacje skalowania
1.  Dynamiczne skalowanie serwerów między warstwa cenowa nie jest obecnie obsługiwane. Oznacza to przełączaniu między warstwy Basic, ogólnego przeznaczenia lub zoptymalizowanych pod kątem pamięci.
2.  Zmniejszenie rozmiaru magazynu serwera nie jest obecnie obsługiwane.

### <a name="server-version-upgrades"></a>Uaktualniania wersji
- Automatycznej migracji między wersjami aparatu bazy danych głównych nie jest obecnie obsługiwane.

### <a name="subscription-management"></a>Zarządzanie subskrypcjami
- Dynamicznie przenoszenie serwerów w subskrypcji i grup zasobów nie jest obecnie obsługiwane.

### <a name="point-in-time-restore-pitr"></a>Punkt w — czas restore (PITR)
1.  Podczas korzystania z funkcji PITR tworzona jest nowy serwer jako serwer, który jest oparty na takie same konfiguracje.
2.  Przywracanie usuniętych serwera nie jest obsługiwane.

## <a name="next-steps"></a>Kolejne kroki
- Zrozumienie [co jest dostępne w każdej warstwy cenowej](concepts-pricing-tiers.md)
- Dowiedz się więcej o [obsługiwane wersje PostgreSQL bazy danych](concepts-supported-versions.md)
- Przegląd [jak kopii zapasowej i przywracania serwera w bazie danych Azure dla PostgreSQL przy użyciu portalu Azure](howto-restore-server-portal.md)
