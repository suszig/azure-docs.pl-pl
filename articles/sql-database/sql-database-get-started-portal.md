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
ms.custom: quick start create
ms.workload: data-management
ms.tgt_pltfrm: portal
ms.devlang: na
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 90761317944b935b76df65d25a2035bfabe71c09
ms.lasthandoff: 04/22/2017


---
# <a name="create-an-azure-sql-database-in-the-azure-portal"></a>Tworzenia bazy danych SQL platformy Azure w witrynie Azure Portal

Ten samouczek Szybki start zawiera szczegółowe instrukcje dotyczące tworzenia bazy danych SQL na platformie Azure. Usługa Azure SQL Database to oferta typu „baza danych jako usługa”, która pozwala na uruchamianie i skalowanie baz danych SQL Server o wysokiej dostępności w chmurze. Ten przewodnik Szybki start pokazuje, jak rozpocząć pracę od utworzenia bazy danych SQL za pomocą witryny Azure Portal.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL

Baza danych Azure SQL jest tworzona ze zdefiniowanym zestawem [zasobów obliczeniowych i przechowywania](sql-database-service-tiers.md). Baza danych jest tworzona w [grupie zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md) oraz na [serwerze logicznym bazy danych Azure SQL Database](sql-database-features.md). 

Wykonaj te kroki, aby utworzyć bazę danych SQL zawierającą przykładowe dane firmy Adventure Works LT. 

1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

2. Na stronie **Nowy** wybierz pozycję **Bazy danych**, a następnie na stronie **Bazy danych** wybierz pozycję **SQL Database**.

    ![tworzenie bazy danych 1](./media/sql-database-get-started-portal/create-database-1.png)

3. Wypełnij formularz Baza danych SQL w sposób pokazany na wcześniejszej ilustracji, używając następujących informacji:     
   - Nazwa bazy danych: **mySampleDatabase**
   - Grupa zasobów: **myResourceGroup**
   - Źródło: **Przykład (AdventureWorksLT)**

   > [!IMPORTANT]
   > Musisz wybrać przykładową bazę danych w tym formularzu, ponieważ jest ona używana w pozostałej części tego przewodnika Szybki start.
   > 

4. Kliknij pozycję **Serwer**, wypełnij **formularz Nowy serwer** określający globalnie unikatową nazwę serwera, podaj nazwę dla identyfikatora logowania administratora serwera, a następnie podaj dowolnie wybrane hasło. 

   > [!IMPORTANT]
   > Login i hasło administratora serwera określone w tym miejscu będą wymagane do logowania do serwera i jego baz danych w późniejszej części tego przewodnika Szybki start. Zapamiętaj lub zapisz te informacje do wykorzystania w przyszłości. 
   >  

    ![tworzenie serwera bazy danych](./media/sql-database-get-started-portal/create-database-server.png)
5. Po wypełnieniu formularza kliknij pozycję **Wybierz**.

6. Kliknij pozycję **Warstwa cenowa**, aby określić warstwę usługi i poziom wydajności dla nowej bazy danych. Użyj suwaka, aby wybrać pozycję **20 DTU** i **250** GB pamięci masowej. Aby uzyskać więcej informacji o jednostkach DTU, zobacz [Co to jest jednostka DTU?](sql-database-what-is-a-dtu.md).

    ![tworzenie bazy danych s1](./media/sql-database-get-started-portal/create-database-s1.png)

7. Po wybraniu liczby jednostek DTU kliknij przycisk **Zastosuj**.  

8. Teraz po uzupełnieniu formularza SQL Database kliknij przycisk **Utwórz**, aby aprowizować bazę danych. Aprowizacja zajmuje kilka minut. 

9. Na pasku narzędzi kliknij pozycję **Powiadomienia**, aby monitorować proces wdrażania.

    ![powiadomienie](./media/sql-database-get-started-portal/notification.png)


## <a name="create-a-server-level-firewall-rule"></a>Tworzenie reguły zapory na poziomie serwera

Usługa SQL Database tworzy zaporę na poziomie serwera, która uniemożliwia zewnętrznym aplikacjom i narzędziom łączenie się z serwerem i wszelkimi bazami danych na tym serwerze, chyba że zostanie utworzona reguła zapory otwierająca zaporę dla konkretnych adresów IP. Wykonaj następujące kroki, aby utworzyć [regułę zapory na poziomie serwera usługi SQL Database](sql-database-firewall-configure.md) dla podanego adresu IP klienta i włączyć zewnętrzną łączność przez zaporę usługi SQL Database wyłącznie dla konkretnego adresu IP. 

> [!NOTE]
> Usługa SQL Database nawiązuje komunikację na porcie 1433. Jeśli próbujesz nawiązać połączenie z sieci firmowej, ruch wychodzący na porcie 1433 może być zablokowany przez firmową zaporę. Jeśli zachodzi taka sytuacja, nie będzie można nawiązać połączenia z serwerem Azure SQL Database, chyba że dział IT otworzy port 1433.
>

1. Po zakończeniu wdrażania kliknij pozycję **Bazy danych SQL** w menu po lewej stronie i kliknij bazę danych **mySampleDatabase** na stronie Bazy danych SQL. Zostanie otwarta strona przeglądu bazy danych zawierająca w pełni kwalifikowaną nazwę serwera (na przykład **mynewserver20170411.database.windows.net**) i opcje dalszej konfiguracji.

   > [!IMPORTANT]
   > Ta w pełni kwalifikowana nazwa serwera będzie potrzebna do nawiązania połączenia z serwerem i jego bazami danych w kolejnych przewodnikach Szybki start.
   > 

      ![nazwa serwera](./media/sql-database-get-started-portal/server-name.png) 

2. Kliknij pozycję **Ustaw zaporę serwera** na pasku narzędzi, tak jak pokazano to na wcześniejszej ilustracji. Zostanie otwarta strona **Ustawienia zapory** dla serwera SQL Database. 

      ![reguła zapory serwera](./media/sql-database-get-started-portal/server-firewall-rule.png) 


3. Kliknij pozycję **Dodaj adres IP klienta** na pasku narzędzi, aby dodać bieżący adres IP do nowej reguły zapory. Reguła zapory może otworzyć port 1433 dla pojedynczego adresu IP lub zakresu adresów IP.

4. Kliknij pozycję **Zapisz**. Dla bieżącego adresu IP zostanie utworzona reguła zapory na poziomie serwera otwierająca port 1433 na serwerze logicznym.

      ![ustawianie reguły zapory serwera](./media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Kliknij przycisk **OK**, a następnie zamknij stronę **Ustawienia zapory**.

Teraz można połączyć się z serwerem usługi SQL Database i jego bazami danych przy użyciu programu SQL Server Management Studio lub innego wybranego narzędzia z tego adresu IP przy użyciu poprzednio utworzonego konta administratora serwera.

> [!IMPORTANT]
> Domyślnie dostęp za pośrednictwem zapory usługi SQL Database jest włączony dla wszystkich usług platformy Azure. Kliknij przycisk **WYŁ.** na tej stronie, aby wyłączyć tę opcję dla wszystkich usług platformy Azure.

## <a name="query-the-sql-database"></a>Wykonywanie zapytań względem bazy danych SQL

Teraz, po utworzeniu przykładowej bazy danych na platformie Azure, użyjemy wbudowanego narzędzia do obsługi zapytań w witrynie Azure Portal, aby potwierdzić, że możesz nawiązać połączenie z bazą danych i wysłać zapytanie dotyczące danych. 

1. Na stronie SQL Database dla konkretnej bazy danych kliknij pozycję **Narzędzia** na pasku narzędzi. Zostanie otwarta strona **Narzędzia**.

     ![menu narzędzi](./media/sql-database-get-started-portal/tools-menu.png) 

2. Kliknij pozycję **Edytor zapytań (wersja zapoznawcza)**, kliknij pole wyboru **Warunki dotyczące wersji zapoznawczej**, a następnie kliknij przycisk **OK**. Zostanie otwarta strona Edytor zapytań.

3. Kliknij pozycję **Zaloguj**, a następnie po wyświetleniu monitu wybierz pozycję **Uwierzytelnianie programu SQL Server** i podaj utworzony wcześniej identyfikator oraz hasło logowania administratora serwera.

    ![logowanie](./media/sql-database-get-started-portal/login.png) 

4. Kliknij przycisk **OK**, aby się zalogować.

5. Po uwierzytelnieniu w okienku edytora zapytań wpisz następujące zapytanie.

   ```
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

6. Kliknij pozycję **Uruchom**, a następnie przejrzyj wyniki zapytania w okienku **Wyniki**.

    ![wyniki edytora zapytań](./media/sql-database-get-started-portal/query-editor-results.png)

7. Zamknij stronę **Edytor zapytań** i stronę **Narzędzia**.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Inne przewodniki Szybki start w tej kolekcji bazują na tym przewodniku. 

> [!TIP]
> Jeśli planujesz kontynuować pracę z kolejnymi przewodnikami Szybki start, nie usuwaj zasobów utworzonych w tym przewodniku Szybki start. Jeśli nie planujesz kontynuować pracy, wykonaj następujące czynności, aby usunąć wszystkie zasoby utworzone w witrynie Azure Portal w ramach tego szybkiego startu.
>

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij pozycję **myResourceGroup**. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym nazwę **myResourceGroup**, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

Teraz, gdy już masz bazę danych, możesz nawiązać z nią połączenie i uruchamiać zapytania za pomocą ulubionych narzędzi. Dowiedz się więcej, wybierając narzędzie poniżej:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [.NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

