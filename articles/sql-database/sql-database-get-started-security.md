<properties
    pageTitle="Samouczek usługi SQL Database: wprowadzenie do zabezpieczeń"
    description="Dowiedz się, jak tworzyć konta służące do uzyskiwania dostępu do bazy danych i zarządzania nią."
    keywords=""
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
    ms.date="05/03/2016"
    ms.author="carlrab"/>

# Samouczek usługi SQL Database: tworzenie kont użytkowników bazy danych służących do uzyskiwania dostępu do bazy danych i zarządzania nią za pomocą portalu Azure

Z tego samouczka nauczysz się, jak używać portalu Azure do wykonywania następujących czynności:

- Logowanie do usługi SQL Database za pomocą loginu podmiotu zabezpieczeń na poziomie serwera
- Tworzenie konta użytkownika usługi SQL Database
- Przyznawanie uprawnień właściciela bazy danych kontu użytkownika usługi SQL Database wewnątrz bazy danych użytkownika
- Nawiązywanie połączenia z bazą danych SQL przy użyciu konta użytkownika, które nie jest podmiotem zabezpieczeń na poziomie serwera 

[AZURE.INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]


[AZURE.INCLUDE [Create SQL Database logical server](../../includes/sql-database-sql-server-management-studio-connect-server-principal.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-create-new-database-user.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-grant-database-user-dbo-permissions.md)]


[AZURE.INCLUDE [Create SQL Database database](../../includes/sql-database-sql-server-management-studio-connect-user.md)]


## Następne kroki
Po ukończeniu tego samouczka usługi SQL Database, utworzeniu konta użytkownika i przyznaniu mu uprawnień właściciela bazy danych możesz pogłębić wiedzę o [zabezpieczeniach usługi SQL Database](sql-database-manage-logins.md).





<!--HONumber=Jun16_HO2-->


