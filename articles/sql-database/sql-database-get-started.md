<properties
    pageTitle="Samouczek usługi SQL Database: tworzenie bazy danych SQL | Microsoft Azure"
    description="Dowiedz się, jak skonfigurować logiczny serwer usługi SQL Database, regułę zapory serwera, bazę danych SQL, dane przykładowe, połączenie z narzędziami klienckimi, użytkowników i regułę zapory bazy danych."
    keywords="sql database tutorial, create a sql database"
    services="sql-database"
    documentationCenter=""
    authors="carlrabeler"
    manager="jhubbard"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="04/14/2016"
    ms.author="carlrab"/>

# Samouczek usługi SQL Database: tworzenie bazy danych SQL w ciągu kilku minut za pomocą portalu Azure

**Pojedyncza baza danych**

> [AZURE.SELECTOR]
- [Portal Azure](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Z tego samouczka nauczysz się używać portalu Azure do wykonywania następujących czynności:

- Tworzenie logicznego serwera usługi SQL Database do hostowania baz danych SQL
- Tworzenie bazy danych SQL, która nie zawiera żadnych danych, zawiera dane przykładowe lub dane z kopii zapasowej bazy danych SQL.
- Tworzenie reguły zapory na poziomie serwera dla pojedynczego adresu IP lub zakresu adresów IP.

Skorzystaj z tych linków, aby wykonać te same zadania przy użyciu języka [C#](sql-database-get-started-csharp.md) lub programu [PowerShell](sql-database-get-started-powershell.md).

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-create-new-server-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-portal.md)]

[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-server-firewall-portal.md)]

## Następne kroki
Po ukończeniu tego samouczka usługi SQL Database i utworzeniu bazy danych z przykładowymi danymi możesz przystąpić do jej eksplorowania przy użyciu ulubionych narzędzi.

- Jeśli znasz język Transact-SQL i program SQL Server Management Studio, dowiedz się, jak [nawiązać połączenie z bazą danych SQL i odpytywać ją w programie SSMS](sql-database-connect-query-ssms.md).

- Jeśli posługujesz się programem Excel, naucz się [nawiązywać połączenie z bazą danych SQL w programie Excel](sql-database-connect-excel.md).

- Jeśli masz zamiar programować, wybierz język programowania, przeglądając [biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md)

- Jeśli chcesz przenieść lokalne bazy danych programu SQL Server do systemu Azure, zobacz artykuł [Migrating a database to Azure SQL Database](sql-database-cloud-migrate.md) (Migracja bazy danych do usługi Azure SQL Database), aby dowiedzieć się więcej.


## Dowiedz się więcej

[Co to jest SQL Database?](sql-database-technical-overview.md)




<!--HONumber=Jun16_HO2-->


