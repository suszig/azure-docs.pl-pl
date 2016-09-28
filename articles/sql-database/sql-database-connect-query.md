<properties
    pageTitle="Łączenie z bazą danych SQL Database z użyciem zapytania C# | Microsoft Azure"
    description="Napisz program w języku C#, aby wykonywać zapytania i nawiązać połączenie z bazą danych SQL. Informacje na temat adresów IP, parametrów połączenia, bezpiecznego logowania i bezpłatnego programu Visual Studio."
    services="sql-database"
    keywords="c# zapytanie bazy danych, zapytanie c# , nawiązywanie połączenia z bazą danych, SQL C#"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="08/17/2016"
    ms.author="stevestein"/>




# Nawiązywanie połączenia z bazą danych SQL Database za pomocą programu Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Dowiedz się, jak nawiązać połączenie z bazą danych SQL Azure w programie Visual Studio. 

## Wymagania wstępne


Do nawiązania połączenia z bazą danych SQL za pomocą programu Visual Studio potrzebne są następujące elementy: 


- Baza danych SQL, z którą chcesz nawiązać połączenie. W tym artykule użyto przykładowej bazy danych **AdventureWorks**. Aby uzyskać przykładową bazę danych AdventureWorks, zobacz [Tworzenie demonstracyjnej bazy danych](sql-database-get-started.md).


- Visual Studio 2013, aktualizacja 4 (lub nowsza). Firma Microsoft udostępnia teraz program Visual Studio Community *bezpłatnie*.
 - [Visual Studio Community, pobierz](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Więcej opcji bezpłatnego programu Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)




## Otwieranie programu Visual Studio z poziomu witryny Azure Portal


1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

2. Kliknij kolejno pozycje **Więcej usług** > **Bazy danych SQL**
3. Otwórz blok bazy danych **AdventureWorks**, znajdując i klikając bazę danych *AdventureWorks*.

6. Kliknij przycisk **Narzędzia** u góry bloku bazy danych:

    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/tools.png)

7. Kliknij pozycję **Otwórz w programie Visual Studio** (jeśli potrzebujesz programu Visual Studio, kliknij link pobierania):

    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/open-in-vs.png)


8. Program Visual Studio zostanie otwarty z oknem **Łączenie z serwerem** już skonfigurowanym do połączenia z wybranymi w portalu serwerem i bazą danych.  (Kliknij pozycję **Opcje**, aby upewnić się, że jest ustawione połączenie z odpowiednią bazą danych). Wpisz hasło administratora serwera i kliknij pozycję **Połącz**.


    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/connect.png)


8. Jeśli nie masz skonfigurowanej reguły zapory dla adresu IP komputera, może zostać wyświetlony komunikat *Nie można się połączyć*. Aby utworzyć regułę zapory, zobacz [Configure an Azure SQL Database server-level firewall rule using the Azure Portal](sql-database-configure-firewall-settings.md) (Konfigurowanie reguły zapory poziomu serwera usługi Azure SQL Database).


9. Po pomyślnym nawiązaniu połączenia zostanie otwarte okno **Eksplorator obiektów SQL Server** z połączeniem z bazą danych.

    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/sql-server-object-explorer.png)


## Uruchamianie przykładowego zapytania

Teraz, gdy połączenie z bazą danych jest już nawiązane, poniższe kroki wyjaśnią, jak uruchomić przykładowe zapytanie:

2. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query/new-query.png)

3. W oknie zapytania skopiuj i wklej następujący kod.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Kliknij przycisk **Wykonaj**, aby uruchomić zapytanie:

    ![Powodzenie. Łączenie z serwerem usługi SQL Database: SVisual Studio](./media/sql-database-connect-query/run-query.png)

## Następne kroki

- Podczas otwierania baz danych SQL w programie Visual Studio używane są narzędzia SQL Server Data Tools. Aby uzyskać więcej informacji, zobacz [Narzędzia SQL Server Data Tools](https://msdn.microsoft.com/library/hh272686.aspx).
- Aby połączyć się z bazą danych SQL przy użyciu kodu, zobacz [Connect to SQL Database by using .NET (C#) ](sql-database-develop-dotnet-simple.md)(Łączenie z bazą danych usługi SQL Database przy użyciu platformy .NET [C#]).






<!--HONumber=Sep16_HO3-->


