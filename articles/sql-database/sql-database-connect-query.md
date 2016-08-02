<properties
    pageTitle="Łączenie z bazą danych SQL Database z użyciem zapytania C# | Microsoft Azure"
    description="Napisz program w języku C#, aby wykonywać zapytania i nawiązać połączenie z bazą danych SQL. Informacje na temat adresów IP, parametrów połączenia, bezpiecznego logowania i bezpłatnego programu Visual Studio."
    services="sql-database"
    keywords="c# database query, c# query, connect to database, SQL C#"
    documentationCenter=""
    authors="MightyPen"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="get-started-article"
    ms.date="04/25/2016"
    ms.author="annemill"/>


# Nawiązywanie połączenia z bazą danych SQL Database za pomocą programu Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-database-connect-query.md)
- [SSMS](sql-database-connect-query-ssms.md)
- [Excel](sql-database-connect-excel.md)

Dowiedz się, jak połączyć się z bazą danych usługi Azure SQL Database w programie Visual Studio. 

## Wymagania wstępne


Do nawiązania połączenia z bazą danych SQL Database z użyciem programu Visual Studio potrzebne są: 


- Konto i subskrypcja platformy Azure. Po zarejestrowaniu się możesz skorzystać z [bezpłatnej wersji próbnej](https://azure.microsoft.com/pricing/free-trial/).


- Demonstracyjna baza danych **AdventureWorksLT** w usłudze Azure SQL Database.
 - [Utwórz demonstracyjną bazę danych](sql-database-get-started.md) w ciągu kilku minut.


- Visual Studio 2013, aktualizacja 4 (lub nowsza). Firma Microsoft udostępnia teraz program Visual Studio Community *bezpłatnie*.
 - [Visual Studio Community, pobierz](http://www.visualstudio.com/products/visual-studio-community-vs)
 - [Więcej opcji bezpłatnego programu Visual Studio](http://www.visualstudio.com/products/free-developer-offers-vs.aspx)
 - Możesz również skorzystać z instrukcji zawartych w [kroku](#InstallVSForFree) znajdującym się w dalszej części tematu, w którym opisano sposób, w jaki [portal Azure](https://portal.azure.com/) pomaga zainstalować program Visual Studio.


<a name="InstallVSForFree" id="InstallVSForFree"></a>

&nbsp;

## Krok 1: Zainstaluj bezpłatny program Visual Studio Community


Aby w razie potrzeby zainstalować program Visual Studio, możesz:

- Bezpłatnie zainstalować program Visual Studio Community, wyszukując za pomocą wyszukiwarki strony sieci Web produktów Visual Studio, na których udostępniono bezpłatne materiały do pobrania i inne opcje, lub
- Skorzystać z [portalu Azure](https://portal.azure.com/) i przejść do strony sieci Web z materiałami do pobrania opisanymi w dalszej części tematu.


### Program Visual Studio w portalu Azure


1. Zaloguj się w [portalu Azure](https://portal.azure.com/), http://portal.azure.com/.

2. Kliknij przycisk **PRZEGLĄDAJ* WSZYSTKIE**  >  **bazy danych SQL**. Zostanie otwarty blok wyszukiwania baz danych.

3. W polu tekstowym filtru u góry rozpocznij wpisywanie nazwy użytkownika bazy danych **AdventureWorksLT**.

4. Gdy zostanie wyświetlony wiersz bazy danych na serwerze, kliknij go. Zostanie otwarty blok bazy danych.

5. Aby ułatwić sobie pracę, kliknij kontrolkę Minimalizuj we wszystkich pozostałych blokach.

6. Kliknij przycisk **Otwórz w programie Visual Studio** znajdujący się u góry w bloku bazy danych. Zostanie otwarty nowy blok programu Visual Studio, który zawiera linki do zainstalowania lokalizacji dla programu Visual Studio.

    ![Przycisk Otwórz w programie Visual Studio][20-OpenInVisualStudioButton]

7. Kliknij link **Społeczność (bezpłatnie)** lub inny podobny link. Zostanie dodana nowa strona sieci Web.

8. Użyj linków na nowej strony sieci Web, aby zainstalować program Visual Studio.

9. Po zainstalowaniu programu Visual Studio w bloku **Otwórz w programie Visual Studio** kliknij przycisk **Otwórz w programie Visual Studio**. Zostanie otwarty program Visual Studio.

10. Program Visual Studio monituje o wypełnienie pól parametrów połączenia w oknie dialogowym w celu przekazania parametrów do okienka **Eksplorator obiektów SQL Server**.
 - Wybierz **uwierzytelnianie programu SQL Server**, a nie **uwierzytelnianie systemu Windows**.
 - Pamiętaj, aby określić bazę danych **AdventureWorksLT** (**Opcje**  >  **Właściwości połączenia** w oknie dialogowym).

11. W **Eksploratorze obiektów SQL Server** rozwiń węzeł bazy danych.


## Krok 2: Wykonaj przykładowe zapytania

Po nawiązaniu połączenia z serwerem logicznym można połączyć się z bazą danych i uruchomić przykładowe zapytanie. 

1. W **Eksploratorze obiektów** przejdź do bazy danych na serwerze, do którego masz uprawnienia, takiej jak przykładowa baza danych **AdventureWorks**.
2. Kliknij prawym przyciskiem myszy bazę danych, a następnie wybierz pozycję **Nowe zapytanie**.

    ![Nowe zapytanie. Łączenie z serwerem usługi SQL Database: SQL Server Management Studio](./media/sql-database-connect-query-ssms/4-run-query.png)

3. W oknie zapytania skopiuj i wklej następujący kod.

        SELECT
        CustomerId
        ,Title
        ,FirstName
        ,LastName
        ,CompanyName
        FROM SalesLT.Customer;

4. Kliknij przycisk **Wykonaj**.  Poniższy zrzut ekranu przedstawia pomyślnie wykonane zapytanie.

    ![Powodzenie. Łączenie z serwerem usługi SQL Database: SVisual Studio](./media/sql-database-connect-query-ssms/5-success.png)

## Następne kroki

[Connect to SQL Database by using .NET (C#) (Łączenie z bazą danych usługi SQL Database przy użyciu platformy .NET (C#))](sql-database-develop-dotnet-simple.md) 


<!-- Image references. -->

[20-OpenInVisualStudioButton]: ./media/sql-database-connect-query/connqry-free-vs-e.png




<!--HONumber=Jun16_HO2-->


