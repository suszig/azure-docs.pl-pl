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
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: be5839e04fae457b889db11dffe56f31afe723a5
ms.lasthandoff: 03/28/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Tworzenia bazy danych SQL platformy Azure w witrynie Azure Portal

Ten samouczek Szybki start zawiera szczegółowe instrukcje dotyczące tworzenia bazy danych SQL na platformie Azure.  Usługa Azure SQL Database to oferta typu „baza danych jako usługa”, która pozwala na uruchamianie i skalowanie baz danych SQL Server o wysokiej dostępności w chmurze.  Ten przewodnik Szybki start pokazuje, jak rozpocząć pracę od utworzenia nowej bazy danych SQL za pomocą witryny Azure Portal.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL

Baza danych Azure SQL jest tworzona ze zdefiniowanym zestawem [zasobów obliczeniowych i przechowywania](sql-database-service-tiers.md). Baza danych jest tworzona w [grupie zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) oraz na [serwerze logicznym bazy danych Azure SQL Database](sql-database-features.md). 

Wykonaj te kroki, aby utworzyć bazę danych SQL zawierającą przykładowe dane firmy Adventure Works LT. 

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Bazy danych** wybierz pozycję **SQL Database**.

    ![tworzenie bazy danych 1](./media/sql-database-get-started/create-database-1.png)

3. Wypełnij formularz Baza danych SQL w sposób pokazany na wcześniejszej ilustracji, używając następujących informacji: 
   - Nazwa bazy danych: wpisz **mySampleDatabase**
   - Grupa zasobów: wpisz **myResourceGroup**
   - Źródło: wybierz pozycję **Przykład (AdventureWorksLT)**

4. Kliknij pozycję **Serwer**, aby utworzyć i skonfigurować nowy serwer dla nowej bazy danych. Wypełnij **formularz Nowy serwer** określający globalnie unikatową nazwę serwera, podaj nazwę dla identyfikatora logowania administratora serwera, a następnie podaj dowolnie wybrane hasło. 

    ![tworzenie serwera bazy danych](./media/sql-database-get-started/create-database-server.png)
5. Kliknij pozycję **Wybierz**.

6. Kliknij pozycję **Warstwa cenowa**, aby określić warstwę usługi i poziom wydajności dla nowej bazy danych. Na potrzeby tego przewodnika Szybki start wybierz **20 jednostek DTU** i **250** GB magazynu

    ![tworzenie bazy danych s1](./media/sql-database-get-started/create-database-s1.png)

7. Kliknij przycisk **Zastosuj**.  

8. Kliknij pozycję **Utwórz**, aby aprowizować bazę danych. Aprowizacja zajmuje kilka minut. 

9. Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.

    ![powiadomienie](./media/sql-database-get-started/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Tworzenie reguły zapory na poziomie serwera

Usługa SQL Database tworzy zaporę na poziomie serwera, która uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na tym serwerze, chyba że zostanie utworzona reguła zapory otwierająca zaporę dla konkretnych adresów IP. Wykonaj następujące kroki, aby utworzyć [regułę zapory na poziomie serwera usługi SQL Database](sql-database-firewall-configure.md) dla podanego adresu IP klienta i włączyć zewnętrzną łączność przez zaporę usługi SQL Database wyłącznie dla konkretnego adresu IP. 

1. Po zakończeniu wdrażania kliknij pozycję **Bazy danych SQL** w menu po lewej stronie i kliknij nową bazę danych, **mySampleDatabase**, na stronie **Bazy danych SQL**. Zostanie otwarta strona przeglądu bazy danych zawierająca w pełni kwalifikowaną nazwę serwera (na przykład **mynewserver20170327.database.windows.net**) i opcje dalszej konfiguracji.

      ![reguła zapory serwera](./media/sql-database-get-started/server-firewall-rule.png) 

2. Kliknij pozycję **Ustaw zaporę serwera** na pasku narzędzi, tak jak pokazano to na wcześniejszej ilustracji. Zostanie otwarta strona **Ustawienia zapory** dla serwera SQL Database. 

3. Kliknij pozycję **Dodaj adres IP klienta** na pasku narzędzi, a następnie kliknij pozycję **Zapisz**. Dla bieżącego adresu IP zostanie utworzona reguła zapory na poziomie serwera.

      ![ustawianie reguły zapory serwera](./media/sql-database-get-started/server-firewall-rule-set.png) 

4. Kliknij przycisk **OK**, a następnie kliknij przycisk **X**, aby zamknąć stronę **Ustawienia zapory**.

Teraz można połączyć się z bazą danych i jej serwerem przy użyciu programu SQL Server Management Studio lub innego wybranego narzędzia.

## <a name="query-the-sql-database"></a>Wykonywanie zapytań względem bazy danych SQL

Przy tworzeniu baza danych SQL została wypełniona danymi z przykładowej bazy danych **AdventureWorksLT** (była to jedna z opcji wybrana wcześniej w tym przewodniku Szybki start w interfejsie użytkownika do tworzenia). Teraz użyjemy wbudowanego narzędzia zapytań w witrynie Azure Portal do uruchamiania zapytań o dane. 

1. Na stronie SQL Database dla konkretnej bazy danych kliknij pozycję **Narzędzia** na pasku narzędzi. Zostanie otwarta strona **Narzędzia**.

     ![menu narzędzi](./media/sql-database-get-started/tools-menu.png) 

2. Kliknij pozycję **Edytor zapytań (wersja zapoznawcza)**, kliknij pole wyboru **Warunki dotyczące wersji zapoznawczej**, a następnie kliknij przycisk **OK**. Zostanie otwarta strona Edytor zapytań.

3. Kliknij pozycję **Zaloguj**, a następnie po wyświetleniu monitu wybierz pozycję **Uwierzytelnianie programu SQL Server** i podaj utworzony wcześniej identyfikator oraz hasło logowania administratora serwera.

    ![logowanie](./media/sql-database-get-started/login.png) 

4. Kliknij przycisk **OK**, aby się zalogować.

5. Po uwierzytelnieniu w okienku edytora zapytań wpisz następujące zapytanie.

   ```
   SELECT pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Kliknij pozycję **Uruchom**, a następnie przejrzyj wyniki zapytania w okienku **Wyniki**.

    ![wyniki edytora zapytań](./media/sql-database-get-started/query-editor-results.png)

7. Kliknij pozycję **X**, aby zamknąć stronę **Edytor zapytań**, i ponownie kliknij pozycję **X**, aby zamknąć stronę **Narzędzia**.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start lub samouczkami, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego szybkiego startu.

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij pozycję **myResourceGroup**. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę **myResourceGroup**, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

- Aby nawiązywać połączenia i wykonywać zapytania za pomocą programu SQL Server Management Studio, zobacz [Connect and query with SSMS](sql-database-connect-query-ssms.md) (Nawiązywanie połączeń i wykonywanie zapytań za pomocą programu SSMS).
- Aby nawiązywać połączenia przy użyciu programu Visual Studio, zobacz [Connect and query with Visual Studio](sql-database-connect-query.md) (Nawiązywanie połączeń i wykonywanie zapytań przy użyciu programu Visual Studio).
- Opis techniczny usługi SQL Database można znaleźć w temacie [About the SQL Database service](sql-database-technical-overview.md) (Informacje o usłudze SQL Database).

