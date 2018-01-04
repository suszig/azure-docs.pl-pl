---
title: "Minimalizująca przestoje migracji do bazy danych Azure dla programu MySQL | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano sposób przeprowadzenia migracji minimalnym czasem przestojów bazy danych programu MySQL do bazy danych platformy Azure dla programu MySQL i konfigurowanie ładowania początkowego i synchronizacji danych ciągłego źródłowej bazy danych do docelowej bazy danych przy użyciu replikacji Attunity firmy Microsoft Migracji."
services: mysql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 429f5c9f939a802184a6513a63fb9115abf4b235
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Minimalizująca przestoje migracji do bazy danych Azure dla programu MySQL
Można przeprowadzić migrację istniejącej bazy danych MySQL do bazy danych platformy Azure dla programu MySQL przy użyciu replikacji Attunity Migrations firmy Microsoft, wspólnie sponsorowany, wspólnego ofertę z firmy Microsoft, który został dostarczony wraz z usługi migracji bazy danych Azure na nie i Attunity dodatkowych kosztów do klientów firmy Microsoft. Replikacja Attunity Migrations Microsoft umożliwia również minimalna przestój w przypadku migracji bazy danych, a źródłowej bazy danych w dalszym ciągu działać podczas procesu migracji.

Replikacja Attunity jest narzędzie replikacji danych synchronizacji danych między z różnych źródeł i obiekty docelowe, propagowanie skrypcie tworzenia schematu i dane skojarzone z każdej tabeli bazy danych. Replikacja Attunity nie propaguje innych artefakty (na przykład SP, wyzwalaczy, funkcji, itp.) lub convert, na przykład kodu PL/SQL hostowanych w takich artefaktów do T-SQL.

> [!NOTE]
> Podczas replikowania Attunity obsługuje szeroką gamę scenariuszy migracji, replikacja Attunity Migrations Microsoft skupia się na obsługę konkretnego podzestawu par źródłowy i docelowy.

Zawiera omówienie procesu migracji minimalnym czasem przestojów:

1. **Migrowanie schematu źródła MySQL** do bazy danych dla bazy danych MySQL przy użyciu usługi Azure [MySQL Workbench](https://www.mysql.com/products/workbench/).

2. **Ustawienie ładowania początkowego i przeprowadź synchronizację z ciągłymi danymi ze źródłowej bazy danych do docelowej bazy danych** przy użyciu replikacji Attunity Migrations firmy Microsoft. Dzięki temu minimalizuje czas, który źródłowej bazy danych musi być ustawiona jako tylko do odczytu podczas przygotowywania do przełączania aplikacji docelowej bazy danych MySQL na platformie Azure.

Aby uzyskać więcej informacji o replikacji Attunity oferty Migrations firmy Microsoft należy wyświetlić następujące zasoby:
 - Replikacja Attunity Migrations Microsoft [strony sieci web](https://aka.ms/attunity-replicate).
 - [Pobierz](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) replikacja Attunity migracji firmy Microsoft.
 - Replikacja Attunity [społeczności](https://microsoft.attunity.com/), przewodnik Szybki Start, samouczki i pomocy technicznej.
 - Aby uzyskać szczegółowe instrukcje na temat używania Attunity migrację z programu MySQL do bazy danych platformy Azure dla programu MySQL, zapoznaj się [Przewodnik po migracji bazy danych](https://datamigration.microsoft.com/scenario/mysql-to-azuremysql).