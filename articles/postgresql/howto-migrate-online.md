---
title: "Migracja online bazą danych Azure PostgreSQL | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak wykonać migrację online wyodrębniając bazy danych programu PostgreSQL w pliku zrzutu Przywracanie bazy danych programu PostgreSQL z pliku archiwum utworzony przez pg_dump w bazie danych Azure, PostgreSQL i konfigurowanie ładowania początkowego, jak i ciągłych Synchronizacja danych ze źródłowej bazy danych do docelowej bazy danych przy użyciu replikacji Attunity Migrations firmy Microsoft."
services: postgresql
author: HJToland3
ms.author: jtoland
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 01/03/2018
ms.openlocfilehash: cf17ad87b33ed6196d86cebaf6efe699a542bf39
ms.sourcegitcommit: 9ea2edae5dbb4a104322135bef957ba6e9aeecde
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/03/2018
---
# <a name="online-migration-to-azure-database-for-postgresql"></a>Migracja online bazą danych Azure PostgreSQL
Można przeprowadzić migrację istniejącej bazy danych PostgreSQL do bazy danych platformy Azure dla PostgreSQL przy użyciu replikacji Attunity Migrations firmy Microsoft, wspólnie sponsorowany, wspólnego ofertę z firmy Microsoft, który został dostarczony wraz z usługi migracji bazy danych Azure i Attunity bez ponoszenia dodatkowych kosztów do klientów firmy Microsoft. Replikacja Attunity Migrations Microsoft umożliwia również minimalna przestój w przypadku migracji bazy danych, a źródłowej bazy danych w dalszym ciągu działać podczas procesu migracji.

Replikacja Attunity jest narzędzie replikacji danych synchronizacji danych między z różnych źródeł i obiekty docelowe, propagowanie skrypcie tworzenia schematu i dane skojarzone z każdej tabeli bazy danych. Replikacja Attunity nie propaguje innych artefakty (na przykład SP, wyzwalaczy, funkcji, itp.) lub convert, na przykład kodu PL/SQL hostowanych w takich artefaktów do T-SQL.

> [!NOTE]
> Podczas replikowania Attunity obsługuje szeroką gamę scenariuszy migracji, replikacja Attunity Migrations Microsoft skupia się na obsługę konkretnego podzestawu par źródłowy i docelowy.

Zawiera omówienie procesu migracji online:

1. **Migrowanie schematu źródła PostgreSQL** bazy danych Azure dla PostgreSQL przy użyciu [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) i [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) z parametrem - n.

2. **Konfigurowanie ładowania początkowego i synchronizacji danych ciągłego źródłowej bazy danych do docelowej bazy danych** przy użyciu replikacji Attunity Migrations firmy Microsoft. Wykonanie tej tak minimalizuje czas, który źródłowej bazy danych musi być ustawiona jako tylko do odczytu podczas przygotowań do przełącznika aplikacji docelowej bazy danych PostgreSQL na platformie Azure.

Aby uzyskać więcej informacji o replikacji Attunity oferty Migrations firmy Microsoft należy wyświetlić następujące zasoby:
 - Replikacja Attunity Migrations Microsoft [strony sieci web](https://aka.ms/attunity-replicate).
 - [Pobierz](http://discover.attunity.com/download-replicate-microsoft-lp6657.html) replikacja Attunity migracji firmy Microsoft.
 - Aby uzyskać szczegółowe instrukcje na temat używania Attunity do migracji z PostgreSQL do bazy danych platformy Azure dla PostgreSQL dotyczą [Przewodnik po migracji bazy danych](https://datamigration.microsoft.com/scenario/postgresql-to-azurepostgresql).