<properties
   pageTitle="Nawiązywanie połączenia z usługą SQL Data Warehouse przy użyciu programu Visual Studio | Microsoft Azure"
   description="Wprowadzenie do nawiązywania połączenia z usługą SQL Data Warehouse i uruchamiania niektórych zapytań."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/13/2016"
   ms.author="sonyama;barbkess"/>

# Nawiązywanie połączenia z usługą SQL Data Warehouse przy użyciu programu Visual Studio

> [AZURE.SELECTOR]
- [Visual Studio](sql-data-warehouse-get-started-connect.md)
- [SQLCMD](sql-data-warehouse-get-started-connect-sqlcmd.md)
- [AAD](sql-data-warehouse-get-started-connect-aad-authentication.md)

W tym przewodniku przedstawiono sposób nawiązywania połączenia z usługą Azure SQL Data Warehouse w zaledwie kilka minut przy użyciu rozszerzenia SQL Server Data Tools (SSDT) w programie Visual Studio. Po nawiązaniu połączenia uruchomisz proste zapytanie.

## Wymagania wstępne

+ Przykładowe dane AdventureWorksDW w usłudze SQL Data Warehouse. Aby je utworzyć, zobacz artykuł [Tworzenie bazy danych w usłudze SQL Data Warehouse][].
+ Narzędzia SQL Server Data Tools dla Visual Studio Aby uzyskać instrukcje instalacji i informacje na temat dostępnych opcji, zobacz artykuł [Instalowanie programu Visual Studio i narzędzi SSDT][].

## Krok 1: znajdowanie w pełni kwalifikowanej nazwy serwera Azure SQL

Twoja baza danych usługi SQL Data Warehouse jest skojarzona z serwerem SQL platformy Azure. Do nawiązania połączenia z bazą danych potrzebna jest w pełni kwalifikowana nazwa serwera (**nazwa_serwera**.database.windows.net*).

Aby znaleźć w pełni kwalifikowaną nazwę serwera:

1. Przejdź do [Portalu Azure][].
2. Kliknij przycisk **Bazy danych SQL** i kliknij bazę danych, z którą chcesz się połączyć. W tym przykładzie użyto przykładowej bazy danych AdventureWorksDW.
3. Znajdź pełną nazwę serwera.

    ![Pełna nazwa serwera][1]

## Krok 2: nawiązywanie połączenia z usługą SQL Data Warehouse

1. Otwórz program Visual Studio 2013 lub 2015.
2. Otwórz Eksplorator obiektów SQL Server. W tym celu wybierz pozycje **Widok**  >  **Eksplorator obiektów SQL Server**.

    ![Eksplorator obiektów SQL Server][2]

3. Kliknij **ikonę dodawania serwera SQL**.

    ![Dodawanie serwera SQL][3]

4. Wypełnij pola w oknie łączenia z serwerem.

    ![Łączenie z serwerem][4]

    - **Nazwa serwera**. Wprowadź wcześniej ustaloną **nazwę serwera**.
    - **Uwierzytelnianie**. Wybierz opcję **Uwierzytelnianie na serwerze SQL Server** lub **Zintegrowane uwierzytelnianie usługi Active Directory**.
    - **Nazwa użytkownika** i **Hasło**. Wprowadź nazwę użytkownika i hasło, jeżeli powyżej wybrano uwierzytelnianie na serwerze SQL Server.
    - Kliknij przycisk **Połącz**.

5. W celach poznawczych rozwiń węzeł serwera Azure SQL. Możesz przejrzeć skojarzone z serwerem bazy danych. Rozwiń węzeł AdventureWorksDW, aby zobaczyć tabele w przykładowej bazie danych.

    ![Poznawanie bazy danych AdventureWorksDW][5]

## Krok 3: uruchamianie przykładowego zapytania

Teraz, po nawiązaniu połączenia z bazą danych, napiszemy zapytanie.

1. Kliknij prawym przyciskiem myszy bazę danych w Eksploratorze obiektów SQL Server.

2. Wybierz pozycję **Nowe zapytanie**. Otworzy się okno nowego zapytania.

    ![Nowe zapytanie][6]

3. Skopiuj to zapytanie TSQL do okna zapytania:

    ```sql
    SELECT COUNT(*) FROM dbo.FactInternetSales;
    ```

4. Uruchom zapytanie. W tym celu kliknij zieloną strzałkę lub użyj następującego skrótu: `CTRL`+`SHIFT`+`E`.

    ![Uruchamianie zapytania][7]

5. Przejrzyj wyniki zapytania. W tym przykładzie tabela FactInternetSales ma 60398 wierszy.

    ![Wyniki zapytania][8]

## Następne kroki

Teraz, kiedy umiesz nawiązywać połączenia i wykonywać zapytania, spróbuj wykonać [wizualizację danych przy użyciu usługi Power BI][].

Aby skonfigurować środowisko do uwierzytelniania systemu Windows, zobacz artykuł [Nawiązywanie połączeń z usługami SQL Database lub SQL Data Warehouse przy użyciu uwierzytelniania usługi Azure Active Directory][].

<!--Arcticles-->
[Tworzenie bazy danych w usłudze SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md
[Instalowanie programu Visual Studio i narzędzi SSDT]: sql-data-warehouse-install-visual-studio.md
[Nawiązywanie połączeń z usługami SQL Database lub SQL Data Warehouse przy użyciu uwierzytelniania usługi Azure Active Directory]: ../sql-data-warehouse/sql-data-warehouse-get-started-connect-aad-authentication.md
[wizualizację danych przy użyciu usługi Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md  

<!--Other-->
[Portalu Azure]: https://portal.azure.com

<!--Image references-->

[1]: ./media/sql-data-warehouse-get-started-connect/get-server-name.png
[2]: ./media/sql-data-warehouse-get-started-connect/open-ssdt.png
[3]: ./media/sql-data-warehouse-get-started-connect/add-server.png
[4]: ./media/sql-data-warehouse-get-started-connect/connection-dialog.png
[5]: ./media/sql-data-warehouse-get-started-connect/explore-sample.png
[6]: ./media/sql-data-warehouse-get-started-connect/new-query2.png
[7]: ./media/sql-data-warehouse-get-started-connect/run-query.png
[8]: ./media/sql-data-warehouse-get-started-connect/query-results.png



<!--HONumber=Jun16_HO2-->


