---
title: 'Witryna Azure Portal: tworzenie bazy danych SQL | Microsoft Docs'
description: "Dowiedz się, jak utworzyć serwer logiczny, regułę zapory na poziomie serwera i bazy danych usługi SQL Database w witrynie Azure Portal. Dowiedz się również, jak wykonywać zapytania do bazy danych Azure SQL Database za pomocą witryny Azure Portal."
keywords: "samouczek usługi sql database, tworzenie bazy danych sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: quick start
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 03/13/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 1c087f3ecbdd6956b50a8ef9b7e81340f83ff0cf
ms.lasthandoff: 03/21/2017


---
# <a name="create-and-query-a-single-azure-sql-database-in-the-azure-portal"></a>Tworzenie pojedynczej bazy danych Azure SQL Database i wysyłanie do niej zapytań w witrynie Azure Portal

Bazy danych Azure SQL Database można utworzyć za pomocą witryny Azure Portal. Ta metoda bazuje na opartym na przeglądarce interfejsie użytkownika umożliwiającym tworzenie i konfigurowanie baz danych Azure SQL Database oraz wszystkich pokrewnych zasobów platformy Azure.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL

Baza danych Azure SQL jest tworzona ze zdefiniowanym zestawem [zasobów obliczeniowych i przechowywania](sql-database-service-tiers.md). Baza danych jest tworzona w [grupie zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) oraz na [serwerze logicznym bazy danych Azure SQL Database](sql-database-features.md). 

Wykonaj te kroki, aby utworzyć bazę danych SQL zawierającą przykładowe dane firmy Adventure Works LT. 

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Bazy danych** wybierz pozycję **SQL Database**.

3. Wypełnij formularz usługi SQL Database wymaganymi informacjami: 
   - Nazwa bazy danych: podaj nazwę bazy danych
   - Subskrypcja: wybierz subskrypcję
   - Grupa zasobów: wybierz nową lub istniejącą
   - Źródło: wybierz pozycję **Przykład (AdventureWorksLT)**
   - Serwer: utwórz nowy serwer (nazwa w polu **Serwer** musi być unikatowa w skali globalnej)
   - Elastyczna pula: na potrzeby tego szybkiego startu wybierz pozycję **Nie teraz**
   - Warstwa cenowa: wybierz pozycję **20 DTU** i **250** GB pamięci masowej
   - Sortowanie: nie można zmienić tej wartości podczas importowania przykładowej bazy danych 
   - Przypnij do pulpitu nawigacyjnego: zaznacz to pole wyboru

      ![tworzenie bazy danych](./media/sql-database-get-started/create-database-s1.png)

4. Na koniec kliknij pozycję **Utwórz**. Aprowizacja zajmuje kilka minut.
5. Po zakończeniu wdrażania bazy danych SQL wybierz opcję **Bazy danych SQL** na pulpicie nawigacyjnym lub wybierz pozycję **Bazy danych SQL** z menu po lewej stronie, a następnie kliknij nową bazę danych na stronie **Bazy danych SQL**. Zostanie otwarta strona przeglądu bazy danych zawierająca w pełni kwalifikowaną nazwę serwera (na przykład **mynewserver20170313.database.windows.net**) i opcje dalszej konfiguracji.

      ![nowa baza danych sql](./media/sql-database-get-started/new-database-s1-overview.png) 

## <a name="create-a-server-level-firewall-rule"></a>Tworzenie reguły zapory na poziomie serwera

Usługa SQL Database tworzy zaporę uniemożliwiającą łączenie się zewnętrznych aplikacji i narzędzi z serwerem i bazą danych. Wykonaj następujące kroki, aby utworzyć [regułę zapory na poziomie serwera SQL Database](sql-database-firewall-configure.md) dla podanego adresu IP w celu włączenia zewnętrznej łączności za pośrednictwem zapory usługi SQL Database. 

1. Kliknij pozycję **Ustaw zaporę serwera** na pasku narzędzi dla bazy danych. Zostanie otwarta strona **Ustawienia zapory** dla serwera SQL Database. 

      ![reguła zapory serwera](./media/sql-database-get-started/server-firewall-rule.png) 

2. Kliknij pozycję **Dodaj adres IP klienta** na pasku narzędzi, a następnie kliknij pozycję **Zapisz**. Dla bieżącego adresu IP zostanie utworzona reguła zapory na poziomie serwera.

3. Kliknij przycisk **OK**, a następnie kliknij przycisk **X**, aby zamknąć stronę Ustawienia zapory.

Teraz można połączyć się z bazą danych i jej serwerem przy użyciu programu SQL Server Management Studio lub innego wybranego narzędzia.

## <a name="query-the-sql-database"></a>Wykonywanie zapytań względem bazy danych SQL

Wykonaj następujące czynności, aby za pomocą edytora zapytań w witrynie Azure Portal utworzyć zapytanie względem bazy danych. 

1. Na stronie SQL Database dla konkretnej bazy danych kliknij pozycję **Narzędzia** na pasku narzędzi. Zostanie otwarta strona wersji zapoznawczej **Edytora zapytań**.

     ![menu narzędzi](./media/sql-database-get-started/tools-menu.png) 

2. Kliknij pozycję **Edytor zapytań (wersja zapoznawcza)**, kliknij pole wyboru **Warunki dotyczące wersji zapoznawczej**, a następnie kliknij przycisk **OK**. Zostanie otwarty edytor zapytań.

3. Kliknij pozycję **Zaloguj**, a następnie po wyświetleniu monitu wybierz pozycję **Uwierzytelnianie programu SQL Server** i podaj identyfikator oraz hasło logowania administratora serwera.
4. Kliknij przycisk **OK**, aby się zalogować.

5. Po uwierzytelnieniu w oknie zapytania wpisz wybrane zapytanie, na przykład takie jak następujące:

   ```
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Kliknij pozycję **Uruchom**, a następnie przejrzyj wyniki zapytania w okienku **Wyniki**.

    ![wyniki edytora zapytań](./media/sql-database-get-started/query-editor-results.png)

7. Kliknij przycisk **X**, aby zamknąć stronę Edytor zapytań.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Opcja **Połącz z** zapewnia szybki start w tej kolekcji, a samouczki w kolekcji samouczków zależą od tego przewodnika Szybki start. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start lub samouczkami, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego szybkiego startu.

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij pozycję **myResourceGroup**. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę **myResourceGroup**, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

- Aby nawiązywać połączenia i wykonywać zapytania za pomocą programu SQL Server Management Studio, zobacz [Connect and query with SSMS](sql-database-connect-query-ssms.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą programu SSMS).
- Aby nawiązywać połączenia przy użyciu programu Visual Studio, zobacz [Connect and query with Visual Studio](sql-database-connect-query.md) (Nawiązywanie połączeń i wykonywanie zapytań przy użyciu programu Visual Studio).
- Opis techniczny usługi SQL Database można znaleźć w temacie [About the SQL Database service](sql-database-technical-overview.md) (Informacje o usłudze SQL Database).

