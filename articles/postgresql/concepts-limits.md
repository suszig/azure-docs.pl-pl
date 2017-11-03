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
ms.date: 06/01/2017
ms.openlocfilehash: 38988fc5c0dc05331ea078534cd1a05e9eca2493
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Ograniczenia dotyczące bazy danych platformy Azure dla PostgreSQL
Baza danych Azure PostgreSQL usługi jest w wersji zapoznawczej. W poniższych sekcjach opisano pojemności i limity funkcjonalności usługi bazy danych.

## <a name="service-tier-maximums"></a>Maksymalne wartości warstwy usługi
Bazy danych platformy Azure dla PostgreSQL ma wiele warstw usług, które można wybrać podczas tworzenia serwera. Aby uzyskać więcej informacji, zobacz [zrozumieć, co jest dostępne w poszczególnych warstwach usług](concepts-service-tiers.md).  

Istnieje następująca maksymalną liczbę połączeń, jednostek obliczeniowych i magazynu w poszczególnych warstwach usług w wersji zapoznawczej usługi: 

|                            |                   |
| :------------------------- | :---------------- |
| **Maksymalna liczba połączeń**        |                   |
| Podstawowe 50 obliczeniowe jednostki     | połączenia o szybkości 50    |
| Podstawowe 100 jednostek obliczeń    | połączenia o szybkości 100   |
| Standardowe jednostki 100 obliczeń | połączenia o szybkości 200   |
| Standardowa 200 obliczeniowe jednostki | 300 połączeń   |
| Standardowa 400 obliczeniowe jednostki | 400 połączeń   |
| Standardowa 800 obliczeniowe jednostki | połączenia o szybkości 500   |
| **Obliczeniowe maksymalna liczba jednostek**      |                   |
| Warstwa Podstawowa usług         | 100 jednostek obliczeń |
| Warstwa Standardowa usług      | 800 obliczeniowe jednostki |
| **Maksymalna liczba magazynu**            |                   |
| Warstwa Podstawowa usług         | 1 TB              |
| Warstwa Standardowa usług      | 1 TB              |

Po osiągnięciu zbyt wiele połączeń, może zostać wyświetlony następujący błąd:
> Błąd krytyczny: Niestety, jeszcze zbyt wielu klientów

## <a name="preview-functional-limitations"></a>Ograniczenia funkcjonalności wersji zapoznawczej
### <a name="scale-operations"></a>Operacje skalowania
1.  Dynamiczne skalowanie serwerów w warstwach usług nie jest obecnie obsługiwane. Oznacza to przełączaniu między Basic i Standard warstwy usług.
2.  Dynamiczne zwiększanie na żądanie magazynu na serwerze utworzone wcześniej nie jest obecnie obsługiwane.
3.  Zmniejszenie rozmiaru magazynu serwera nie jest obsługiwana.

### <a name="server-version-upgrades"></a>Uaktualniania wersji
- Automatycznej migracji między wersjami aparatu bazy danych głównych nie jest obecnie obsługiwane.

### <a name="subscription-management"></a>Zarządzanie subskrypcją
- Dynamicznie przenoszenie serwerów wstępnie utworzone w subskrypcji i grupy zasobów nie jest obecnie obsługiwane.

### <a name="point-in-time-restore"></a>Przywracanie do punktu w czasie
1.  Przywracanie do warstwy z innej usługi i/lub jednostki obliczeniowe i rozmiaru magazynu nie jest dozwolone.
2.  Przywracanie usuniętej serwera nie jest obsługiwane.

## <a name="next-steps"></a>Następne kroki
- Zrozumienie [co jest dostępne w każdej warstwy cenowej](concepts-service-tiers.md)
- Zrozumienie [obsługiwane wersje PostgreSQL bazy danych](concepts-supported-versions.md)
- Przegląd [jak kopii zapasowej i przywracania serwera w bazie danych Azure dla PostgreSQL przy użyciu portalu Azure](howto-restore-server-portal.md)
