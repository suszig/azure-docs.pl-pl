---
title: "Migrowanie bazy danych za pomocą importowania i eksportowania w bazie danych Azure dla PostgreSQL | Dokumentacja firmy Microsoft"
description: "Opisuje sposób wyodrębnić bazy danych programu PostgreSQL do pliku skryptu i zaimportuj dane do docelowej bazy danych z tego pliku."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 11/03/2017
ms.openlocfilehash: ddbfd9ef8b2ae4c3c851afc18b010b234b654c81
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/13/2018
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Migracji PostgreSQL bazy danych przy użyciu eksportowania i importowania
Można użyć [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) można wyodrębnić bazy danych programu PostgreSQL do pliku skryptu i [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) do importowania danych do docelowej bazy danych z tego pliku.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych ten przewodnik, potrzebne są:
- [Azure bazy danych serwera PostgreSQL](quickstart-create-server-database-portal.md) przy użyciu reguł zapory umożliwiających dostęp i bazy danych na jego podstawie.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) zainstalowane narzędzie wiersza polecenia
- [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) zainstalowane narzędzie wiersza polecenia

Wykonaj następujące kroki, aby wyeksportować i zaimportować bazy danych programu PostgreSQL.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Tworzenie pliku skryptu za pomocą pg_dump, zawierający dane do załadowania
Aby wyeksportować istniejący PostgreSQL bazy danych lokalnej lub w maszynie Wirtualnej do pliku skryptu sql, uruchom następujące polecenie w istniejącym środowisku:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Na przykład, jeśli masz lokalny serwer i bazę danych o nazwie **programu testdb** w niej:
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postgresql"></a>Importowanie danych w docelowej bazie danych Azure dla PostgreSQL
Można użyć psql wiersza polecenia i parametru--dbname (-d) do importowania danych do bazy danych PostgreSQL serwera i ładowanie danych z pliku sql Azure.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
W tym przykładzie użyto psql narzędzie i plik skryptu o nazwie **testdb.sql** z poprzedniego kroku do importowania danych do bazy danych **mypgsqldb** na serwerze docelowym  **mypgserver 20170401.postgres.database.azure.com**.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Kolejne kroki
- Aby przeprowadzić migrację bazy danych programu PostgreSQL przy użyciu zrzutu i przywracania, zobacz [migracji za pomocą zrzutu i przywracania bazy danych PostgreSQL](howto-migrate-using-dump-and-restore.md)
