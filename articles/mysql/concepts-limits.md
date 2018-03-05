---
title: "Ograniczenia dotyczące bazy danych platformy Azure dla programu MySQL"
description: "W tym artykule opisano ograniczenia w bazie danych Azure dla programu MySQL, takie jak liczba połączeń i opcje aparatu magazynu."
services: mysql
author: kamathsun
ms.author: sukamat
manager: kfile
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: 85e57170c1cbd977d2de6e7e614916333c79e047
ms.sourcegitcommit: c765cbd9c379ed00f1e2394374efa8e1915321b9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/28/2018
---
# <a name="limitations-in-azure-database-for-mysql"></a>Ograniczenia dotyczące bazy danych platformy Azure dla programu MySQL
Jest bazą danych Azure dla usługi MySQL w publicznej wersji zapoznawczej. W poniższych sekcjach opisano pojemności, magazynu aparat obsługę, uprawnień, obsługi instrukcji manipulacji danych oraz limity funkcjonalności usługi bazy danych. Zobacz też [ogólne ograniczenia](https://dev.mysql.com/doc/mysql-reslimits-excerpt/5.6/en/limits.html) mające zastosowanie do aparatu bazy danych MySQL.

## <a name="service-tier-maximums"></a>Maksymalne wartości warstwy usługi
Bazy danych platformy Azure dla programu MySQL ma wiele warstw do wyboru podczas tworzenia serwera. Aby uzyskać więcej informacji, zobacz [bazy danych Azure dla programu MySQL warstw cenowych](concepts-pricing-tiers.md).  

Jest maksymalna liczba połączeń, obliczeniowe jednostek i magazynu w poszczególnych warstwach usług wersji zapoznawczej, następująca: 

|**Warstwa cenowa**| **Generowanie obliczeniowe**|**vCore(s)**| **Maksymalna liczba połączeń**|
|---|---|---|---|
|Podstawowa| Gen 4| 1| 50|
|Podstawowa| Gen 4| 2| 100|
|Podstawowa| Gen 5| 1| 50|
|Podstawowa| Gen 5| 2| 100|
|Ogólne zastosowanie| Gen 4| 2| 200|
|Ogólne zastosowanie| Gen 4| 4| 400|
|Ogólne zastosowanie| Gen 4| 8| 800|
|Ogólne zastosowanie| Gen 4| 16| 1600|
|Ogólne zastosowanie| Gen 4| 32| 3200|
|Ogólne zastosowanie| Gen 5| 2| 200|
|Ogólne zastosowanie| Gen 5| 4| 400|
|Ogólne zastosowanie| Gen 5| 8| 800|
|Ogólne zastosowanie| Gen 5| 16| 1600|
|Ogólne zastosowanie| Gen 5| 32| 3200|
|Pamięć| Gen 5| 2| 600|
|Pamięć| Gen 5| 4| 1250|
|Pamięć| Gen 5| 8| 2500|
|Pamięć| Gen 5| 16| 5000|
|Pamięć| Gen 5| 32| 10 000| 

Po osiągnięciu zbyt wiele połączeń, może zostać wyświetlony następujący błąd:
> Błąd 1040 (08004): Zbyt wiele połączeń

## <a name="storage-engine-support"></a>Obsługa aparatu magazynu

### <a name="supported"></a>Obsługiwane
- [InnoDB](https://dev.mysql.com/doc/refman/5.7/en/innodb-introduction.html)
- [MEMORY](https://dev.mysql.com/doc/refman/5.7/en/memory-storage-engine.html)

### <a name="unsupported"></a>Nieobsługiwane
- [MyISAM](https://dev.mysql.com/doc/refman/5.7/en/myisam-storage-engine.html)
- [NALEŻĄCY](https://dev.mysql.com/doc/refman/5.7/en/blackhole-storage-engine.html)
- [ARCHIWUM](https://dev.mysql.com/doc/refman/5.7/en/archive-storage-engine.html)
- [FEDERACYJNA](https://dev.mysql.com/doc/refman/5.7/en/federated-storage-engine.html)

## <a name="privilege-support"></a>Obsługa uprawnień

### <a name="unsupported"></a>Nieobsługiwane
- Administrator roli: wiele parametrów serwera i ustawienia mogą przypadkowo obniżają wydajność serwerów lub odwrócić ACID właściwości systemu DBMS. Tak, aby zachować integralności usługi i umowy SLA na poziomie produktu, tego usługa nie uwidacznia roli Administrator. Domyślne konto użytkownika, który jest tworzony podczas tworzenia nowego wystąpienia bazy danych, umożliwia użytkownikowi wykonują większość instrukcji DDL i DML w wystąpieniu bazy danych zarządzanych. 
- Uprawnienie administratora: Podobnie [SUPER uprawnień](https://dev.mysql.com/doc/refman/5.7/en/privileges-provided.html#priv_super) również jest ograniczony.

## <a name="data-manipulation-statement-support"></a>Obsługa instrukcji manipulacji danych

### <a name="supported"></a>Obsługiwane
- PLIKWEJŚCIOWY danych obciążenia — obsługiwane, ale należy określić parametr [LOCAL], który jest przekierowywany do ścieżki UNC (zainstalowane za pomocą XSMB magazynu Azure).

### <a name="unsupported"></a>Nieobsługiwane
- WYBIERZ... DO PLIKWYJŚCIOWY

## <a name="preview-functional-limitations"></a>Ograniczenia funkcjonalności wersji zapoznawczej

### <a name="scale-operations"></a>Operacje skalowania
- Dynamiczne skalowanie serwerów między warstwa cenowa nie jest obecnie obsługiwane. Oznacza to przełączaniu między Basic, ogólnego przeznaczenia i zoptymalizowanych pod kątem pamięci, warstw cenowych.
- Zmniejszenie rozmiaru magazynu serwera nie jest obsługiwana.

### <a name="server-version-upgrades"></a>Uaktualniania wersji
- Automatycznej migracji między wersjami aparatu bazy danych głównych nie jest obecnie obsługiwane.

### <a name="point-in-time-restore"></a>Przywracanie do punktu w czasie
- Przywracanie do warstwy z innej usługi i/lub jednostki obliczeniowe i rozmiaru magazynu nie jest dozwolone.
- Przywracanie usuniętych serwera nie jest obsługiwane.

## <a name="functional-limitations"></a>Ograniczenia funkcjonalności

### <a name="subscription-management"></a>Zarządzanie subskrypcjami
- Dynamicznie przenoszenie serwerów wstępnie utworzone w subskrypcji i grupy zasobów nie jest obecnie obsługiwane.

## <a name="current-known-issues"></a>Obecnie znane problemy
- Wystąpienie serwera MySQL Wyświetla wersję niewłaściwy serwer, po nawiązaniu połączenia. Aby uzyskać z właściwym serwerem versioning wystąpienia, należy użyć wybierz version(); polecenie w wierszu polecenia programu MySQL.

## <a name="next-steps"></a>Kolejne kroki
- [Co to jest dostępne w poszczególnych warstwach usług](concepts-pricing-tiers.md)
- [Obsługiwane wersje bazy danych MySQL](concepts-supported-versions.md)
