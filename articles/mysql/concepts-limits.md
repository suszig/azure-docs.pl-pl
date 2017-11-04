---
title: "Ograniczenia dotyczące bazy danych platformy Azure dla programu MySQL | Dokumentacja firmy Microsoft"
description: W tym artykule opisano ograniczenia wersji zapoznawczej w bazie danych Azure dla programu MySQL.
services: mysql
author: jasonh
ms.author: kamathsun
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 10/26/2017
ms.openlocfilehash: b3fba38cacf5b5abcdea7f0def8c1d39e653f0a8
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="limitations-in-azure-database-for-mysql-preview"></a>Ograniczenia dotyczące bazy danych platformy Azure dla programu MySQL (wersja zapoznawcza)
Jest bazą danych Azure dla usługi MySQL w publicznej wersji zapoznawczej. W poniższych sekcjach opisano pojemności i limity funkcjonalności usługi bazy danych. Zobacz też [ogólne ograniczenia](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) mające zastosowanie do aparatu bazy danych MySQL.

## <a name="service-tier-maximums"></a>Maksymalne wartości warstwy usługi
Bazy danych platformy Azure dla programu MySQL ma wiele warstw do wyboru podczas tworzenia serwera. Aby uzyskać więcej informacji, zobacz [zrozumieć, co jest dostępne w poszczególnych warstwach usług](concepts-service-tiers.md).  

Jest maksymalna liczba połączeń, obliczeniowe jednostek i magazynu w poszczególnych warstwach usług wersji zapoznawczej, następująca: 

|                            |                   |
| :------------------------- | :---------------- |
| **Maksymalna liczba połączeń**        |                   |
| Podstawowe 50 obliczeniowe jednostki     | połączenia o szybkości 50    |
| Podstawowe 100 jednostek obliczeń    | połączenia o szybkości 100   |
| Standardowe jednostki 100 obliczeń | połączenia o szybkości 200   |
| Standardowa 200 obliczeniowe jednostki | 400 połączeń   |
| Standardowa 400 obliczeniowe jednostki | 800 połączeń   |
| Standardowa 800 obliczeniowe jednostki | połączenia 1600  |
| **Obliczeniowe maksymalna liczba jednostek**      |                   |
| Warstwa Podstawowa usług         | 100 jednostek obliczeń |
| Warstwa Standardowa usług      | 800 obliczeniowe jednostki |
| **Maksymalna liczba magazynu**            |                   |
| Warstwa Podstawowa usług         | 1 TB              |
| Warstwa Standardowa usług      | 1 TB              |

Po osiągnięciu zbyt wiele połączeń, może zostać wyświetlony następujący błąd:
> Błąd 1040 (08004): Zbyt wiele połączeń

## <a name="preview-functional-limitations"></a>Ograniczenia funkcjonalności wersji zapoznawczej

### <a name="scale-operations"></a>Operacje skalowania
- Dynamiczne skalowanie serwerów w warstwach usług nie jest obecnie obsługiwane. Oznacza to przełączaniu między Basic i Standard warstwy usług.
- Dynamiczne zwiększanie na żądanie magazynu na serwerze utworzone wcześniej nie jest obecnie obsługiwane.
- Zmniejszenie rozmiaru magazynu serwera nie jest obsługiwana.

### <a name="server-version-upgrades"></a>Uaktualniania wersji
- Automatycznej migracji między wersjami aparatu bazy danych głównych nie jest obecnie obsługiwane.

### <a name="subscription-management"></a>Zarządzanie subskrypcją
- Dynamicznie przenoszenie serwerów wstępnie utworzone w subskrypcji i grupy zasobów nie jest obecnie obsługiwane.

### <a name="point-in-time-restore"></a>Przywracanie do punktu w czasie
- Przywracanie do warstwy z innej usługi i/lub jednostki obliczeniowe i rozmiaru magazynu nie jest dozwolone.
- Przywracanie usuniętej serwera nie jest obsługiwane.

## <a name="next-steps"></a>Następne kroki
- [Co to jest dostępne w poszczególnych warstwach usług](concepts-service-tiers.md)
- [Obsługiwane wersje bazy danych MySQL](concepts-supported-versions.md)
