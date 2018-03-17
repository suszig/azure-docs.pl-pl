---
title: "Jak zrzutu i przywrócić w bazie danych Azure PostgreSQL"
description: "Opisuje sposób wyodrębnić bazy danych programu PostgreSQL w pliku zrzutu i przywracanie z pliku utworzonego przez pg_dump w bazie danych Azure dla PostgreSQL."
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 02/28/2018
ms.openlocfilehash: f74c60cb99ee5bae1af8e000ebbd21b41600638d
ms.sourcegitcommit: a36a1ae91968de3fd68ff2f0c1697effbb210ba8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/17/2018
---
# <a name="migrate-your-postgresql-database-using-dump-and-restore"></a>Migrowanie bazy danych PostgreSQL przy użyciu zrzutu i przywracania
Można użyć [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) można wyodrębnić bazy danych programu PostgreSQL w pliku zrzutu i [pg_restore](https://www.postgresql.org/docs/9.3/static/app-pgrestore.html) Przywracanie bazy danych programu PostgreSQL z pliku archiwum utworzone przez pg_dump.

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych ten przewodnik, potrzebne są:
- [Azure bazy danych serwera PostgreSQL](quickstart-create-server-database-portal.md) przy użyciu reguł zapory umożliwiających dostęp i bazy danych na jego podstawie.
- [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) i [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html) zainstalowane narzędzia wiersza polecenia

Wykonaj następujące kroki, aby zrzutu i przywrócenia bazy danych programu PostgreSQL:

## <a name="create-a-dump-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Tworzenie pliku zrzutu z wykorzystaniem pg_dump, zawierający dane do załadowania
Aby utworzyć kopię zapasową istniejących PostgreSQL bazy danych na lokalnym lub maszynie wirtualnej, uruchom następujące polecenie:
```bash
pg_dump -Fc -v --host=<host> --username=<name> --dbname=<database name> > <database>.dump
```
Na przykład, jeśli masz lokalny serwer i bazę danych o nazwie **programu testdb** w nim
```bash
pg_dump -Fc -v --host=localhost --username=masterlogin --dbname=testdb > testdb.dump
```

## <a name="restore-the-data-into-the-target-azure-database-for-postrgesql-using-pgrestore"></a>Przywróć dane do docelowej bazy danych platformy Azure dla PostrgeSQL przy użyciu pg_restore
Po utworzeniu docelowej bazy danych, można użyć polecenia pg_restore i -d, parametr — dbname przywrócenie danych do docelowej bazy danych z pliku zrzutu.
```bash
pg_restore -v --no-owner –-host=<server name> --port=<port> --username=<user@servername> --dbname=<target database name> <database>.dump
```
W tym parametru właściciela nie powoduje, że wszystkie obiekty utworzone podczas przywracania do należeć użytkownik określony z nazwą użytkownika —. Aby uzyskać więcej informacji, zobacz oficjalnej dokumentacji PostgreSQL na [pg_restore](https://www.postgresql.org/docs/9.6/static/app-pgrestore.html).

W tym przykładzie Przywróć dane z pliku zrzutu **testdb.dump** do bazy danych **mypgsqldb** na serwerze docelowym **mydemoserver.postgres.database.azure.com**. 
```bash
pg_restore -v --no-owner --host=mydemoserver.postgres.database.azure.com --port=5432 --username=mylogin@mydemoserver --dbname=mypgsqldb testdb.dump
```

## <a name="next-steps"></a>Kolejne kroki
- Aby przeprowadzić migrację bazy danych programu PostgreSQL przy użyciu eksportu i importu, zobacz [migracji PostgreSQL bazy danych przy użyciu eksportowania i importowania](howto-migrate-using-export-and-import.md)