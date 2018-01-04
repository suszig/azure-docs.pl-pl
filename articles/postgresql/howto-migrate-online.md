---
title: "Migracja minimalnym czasem przestojów bazą danych Azure PostgreSQL | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak przeprowadzić migrację minimalnym czasem przestojów wyodrębniania bazy danych programu PostgreSQL w pliku zrzutu, przywracanie bazy danych programu PostgreSQL z pliku archiwum utworzone przez pg_dump w bazie danych Azure dla PostgreSQL i konfigurowania ładowania początkowego i Synchronizacja danych ciągłego źródłowej bazy danych do docelowej bazy danych przy użyciu replikacji Attunity Migrations firmy Microsoft."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/04/2018
ms.openlocfilehash: 8c98c58042e7f1d1726eaad6ee03d1531b6c910e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql"></a>Migracja minimalnym czasem przestojów PostgreSQL bazą danych Azure
Można przeprowadzić migrację istniejącej bazy danych PostgreSQL do bazy danych platformy Azure dla PostgreSQL przy użyciu replikacji Attunity Migrations firmy Microsoft, wspólnie sponsorowany, wspólnego ofertę z firmy Microsoft, który został dostarczony wraz z usługi migracji bazy danych Azure i Attunity bez ponoszenia dodatkowych kosztów do klientów firmy Microsoft. Replikacja Attunity Migrations Microsoft umożliwia również minimalna przestój w przypadku migracji bazy danych, a źródłowej bazy danych w dalszym ciągu działać podczas procesu migracji.

Replikacja Attunity jest narzędzie replikacji danych synchronizacji danych między z różnych źródeł i obiekty docelowe, propagowanie skrypcie tworzenia schematu i dane skojarzone z każdej tabeli bazy danych. Replikacja Attunity nie propaguje innych artefakty (na przykład SP, wyzwalaczy, funkcji, itp.) lub convert, na przykład kodu PL/SQL hostowanych w takich artefaktów do T-SQL.

> [!NOTE]
> Podczas replikowania Attunity obsługuje szeroką gamę scenariuszy migracji, replikacja Attunity Migrations Microsoft skupia się na obsługę konkretnego podzestawu par źródłowy i docelowy.

Zawiera omówienie procesu migracji minimalnym czasem przestojów:

1. **Migrowanie schematu źródła PostgreSQL** do bazy danych dla bazy danych PostgreSQL za pomocą usługi Azure [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) polecenia z parametrem - n, a następnie za pomocą [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) polecenia.

2. **Ustawienie ładowania początkowego i przeprowadź synchronizację z ciągłymi danymi ze źródłowej bazy danych do docelowej bazy danych** przy użyciu replikacji Attunity Migrations firmy Microsoft. Dzięki temu minimalizuje czas, który źródłowej bazy danych musi być ustawiona jako tylko do odczytu przygotowując się do aplikacji docelowej bazy danych PostgreSQL przełączyć się na platformie Azure.

Aby uzyskać więcej informacji o replikacji Attunity oferty Migrations firmy Microsoft należy wyświetlić następujące zasoby:
 - Replikacja Attunity Migrations Microsoft [strony sieci web](https://aka.ms/attunity-replicate).
 - [Pobierz](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) replikacja Attunity migracji firmy Microsoft.
 - Replikacja Attunity [społeczności](https://microsoft.attunity.com/), przewodnik Szybki Start, samouczki i pomocy technicznej.
 - Aby uzyskać szczegółowe instrukcje na temat używania Attunity do migracji z PostgreSQL do bazy danych platformy Azure dla PostgreSQL dotyczą [Przewodnik po migracji bazy danych](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).