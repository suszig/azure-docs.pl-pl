---
title: "Ograniczenia dotyczące bazy danych platformy Azure dla PostgreSQL | Dokumentacja firmy Microsoft"
description: "Opis ograniczeń w bazie danych Azure PostgreSQL."
services: postgresql
author: kamathsun
ms.author: sukamat
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.topic: article
ms.date: 12/04/2017
ms.openlocfilehash: 6dbed1a834d74047178a9f996683d65520047e66
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/06/2018
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Ograniczenia dotyczące bazy danych platformy Azure dla PostgreSQL
Baza danych Azure PostgreSQL usługi jest w wersji zapoznawczej. W poniższych sekcjach opisano pojemności i limity funkcjonalności usługi bazy danych.

## <a name="service-tier-maximums"></a>Maksymalne wartości warstwy usługi
Bazy danych platformy Azure dla PostgreSQL ma wiele warstw usług, które można wybrać podczas tworzenia serwera. Aby uzyskać więcej informacji, zobacz [zrozumieć, co jest dostępne w poszczególnych warstwach usług](concepts-service-tiers.md).  

Istnieje następująca maksymalną liczbę połączeń, jednostek obliczeniowych i magazynu w poszczególnych warstwach usług w wersji zapoznawczej usługi: 

| | |
| :------------------------- | :---------------- |
| **Maksymalna liczba połączeń**        |                   |
| Podstawowe 50 obliczeniowe jednostki     | 55 połączeń    |
| Podstawowe 100 jednostek obliczeń    | 105 połączeń   |
| Standardowe jednostki 100 obliczeń | 150 połączeń   |
| Standardowa 200 obliczeniowe jednostki | 250 połączeń   |
| Standardowa 400 obliczeniowe jednostki | 480 połączeń   |
| Standardowa 800 obliczeniowe jednostki | 950 połączeń   |
| **Obliczeniowe maksymalna liczba jednostek**      |                   |
| Warstwa Podstawowa usług         | 100 jednostek obliczeń |
| Warstwa Standardowa usług      | 800 obliczeniowe jednostki |
| **Maksymalna liczba magazynu**            |                   |
| Warstwa Podstawowa usług         | 1 TB              |
| Warstwa Standardowa usług      | 1 TB              |

Azure system wymaga pięciu połączeń do monitorowania PostgreSQL serwera bazy danych Azure. Po osiągnięciu zbyt wiele połączeń, może zostać wyświetlony następujący błąd:
> Błąd krytyczny: Niestety, jeszcze zbyt wielu klientów


## <a name="preview-functional-limitations"></a>Ograniczenia funkcjonalności wersji zapoznawczej
### <a name="scale-operations"></a>Operacje skalowania
1.  Dynamiczne skalowanie serwerów w warstwach usług nie jest obecnie obsługiwane. Oznacza to przełączaniu między Basic i Standard warstwy usług.
2.  Dynamiczne zwiększanie na żądanie magazynu na serwerze utworzone wcześniej nie jest obecnie obsługiwane.
3.  Zmniejszenie rozmiaru magazynu serwera nie jest obsługiwana.

### <a name="server-version-upgrades"></a>Uaktualniania wersji
- Automatycznej migracji między wersjami aparatu bazy danych głównych nie jest obecnie obsługiwane.

### <a name="subscription-management"></a>Zarządzanie subskrypcjami
- Dynamicznie przenoszenie serwerów wstępnie utworzone w subskrypcji i grupy zasobów nie jest obecnie obsługiwane.

### <a name="point-in-time-restore"></a>Przywracanie do punktu w czasie
1.  Przywracanie do warstwy z innej usługi i/lub jednostki obliczeniowe i rozmiaru magazynu nie jest dozwolone.
2.  Przywracanie usuniętej serwera nie jest obsługiwane.

## <a name="next-steps"></a>Kolejne kroki
- Zrozumienie [co jest dostępne w każdej warstwy cenowej](concepts-service-tiers.md)
- Zrozumienie [obsługiwane wersje PostgreSQL bazy danych](concepts-supported-versions.md)
- Przegląd [jak kopii zapasowej i przywracania serwera w bazie danych Azure dla PostgreSQL przy użyciu portalu Azure](howto-restore-server-portal.md)
