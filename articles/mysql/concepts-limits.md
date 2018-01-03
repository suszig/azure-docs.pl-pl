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
ms.date: 12/09/2017
ms.openlocfilehash: e16982e4e57ba9f2f11e9ee59f88f24b3fe3fe3f
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Ograniczenia dotyczące bazy danych platformy Azure dla programu MySQL
Jest bazą danych Azure dla usługi MySQL w publicznej wersji zapoznawczej. W poniższych sekcjach opisano pojemności, magazynu aparat obsługę, uprawnień, obsługi instrukcji manipulacji danych oraz limity funkcjonalności usługi bazy danych. Zobacz też [ogólne ograniczenia](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) mające zastosowanie do aparatu bazy danych MySQL.

## <a name="service-tier-maximums"></a>Maksymalne wartości warstwy usługi
Bazy danych platformy Azure dla programu MySQL ma wiele warstw do wyboru podczas tworzenia serwera. Aby uzyskać więcej informacji, zobacz [zrozumieć, co jest dostępne w poszczególnych warstwach usług](concepts-service-tiers.md).  

Jest maksymalna liczba połączeń, obliczeniowe jednostek i magazynu w poszczególnych warstwach usług wersji zapoznawczej, następująca: 

|                            |                   |
| :------------------------- | :---------------- |
| **Maksymalna liczba połączeń**        |                   |
| Podstawowe 50 obliczeniowe jednostki     | połączenia o szybkości 50    |
| Podstawowe 100 jednostek obliczeń    | 100 połączeń   |
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

## <a name="storage-engine-support"></a>Obsługa aparatu magazynu

### <a name="supported"></a>Obsługiwane
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [PAMIĘCI](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Nieobsługiwane
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [NALEŻĄCY](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIWUM](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERACYJNA](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Obsługa uprawnień

### <a name="unsupported"></a>Nieobsługiwane
- Administrator roli serwera wiele parametrów i ustawienia mogą przypadkowo obniżają wydajność serwerów lub odwrócić ACID właściwości systemu DBMS. Tak utrzymanie integralności usługi i umowy SLA na poziomie produktu firmy Microsoft firma Microsoft nie ujawniaj roli DBA do klientów. Domyślne konto użytkownika, który jest tworzony podczas tworzenia nowego wystąpienia bazy danych, umożliwia klientom wykonują większość instrukcji DDL i DML w wystąpieniu bazy danych zarządzanych. 
- SUPER uprawnień podobnie [SUPER uprawnień](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) również jest ograniczony.

## <a name="data-manipulation-statement-support"></a>Obsługa instrukcji manipulacji danych

### <a name="supported"></a>Obsługiwane
- PLIKWEJŚCIOWY danych obciążenia — obsługiwane, ale należy określić parametr [LOCAL], który jest przekierowywany do ścieżki UNC (zainstalowane za pomocą XSMB magazynu Azure).

### <a name="unsupported"></a>Nieobsługiwane
- WYBIERZ... DO PLIKWYJŚCIOWY

## <a name="preview-functional-limitations"></a>Ograniczenia funkcjonalności wersji zapoznawczej

### <a name="scale-operations"></a>Operacje skalowania
- Dynamiczne skalowanie serwerów w warstwach usług nie jest obecnie obsługiwane. Oznacza to przełączaniu między Basic i Standard warstwy usług.
- Dynamiczne zwiększanie na żądanie magazynu na serwerze utworzone wcześniej nie jest obecnie obsługiwane.
- Zmniejszenie rozmiaru magazynu serwera nie jest obsługiwana.

### <a name="server-version-upgrades"></a>Uaktualniania wersji
- Automatycznej migracji między wersjami aparatu bazy danych głównych nie jest obecnie obsługiwane.

### <a name="point-in-time-restore"></a>Przywracanie do punktu w czasie
- Przywracanie do warstwy z innej usługi i/lub jednostki obliczeniowe i rozmiaru magazynu nie jest dozwolone.
- Przywracanie usuniętych serwera nie jest obsługiwane.

## <a name="functional-limitations"></a>Ograniczenia funkcjonalności

### <a name="subscription-management"></a>Zarządzanie subskrypcjami
- Dynamicznie przenoszenie serwerów wstępnie utworzone w subskrypcji i grupy zasobów nie jest obecnie obsługiwane.

## <a name="current-known-issues"></a>Obecnie znane problemy:
- Wystąpienie serwera MySQL Wyświetla wersję niewłaściwy serwer, po nawiązaniu połączenia. Aby uzyskać z właściwym serwerem versioning wystąpienia, należy użyć wybierz version(); polecenie w wierszu polecenia programu MySQL.

## <a name="next-steps"></a>Kolejne kroki
- [Co to jest dostępne w poszczególnych warstwach usług](concepts-service-tiers.md)
- [Obsługiwane wersje bazy danych MySQL](concepts-supported-versions.md)
