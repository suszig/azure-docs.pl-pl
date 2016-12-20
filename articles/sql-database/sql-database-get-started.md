---
title: "Samouczek usługi SQL Database: tworzenie serwera, reguły zapory na poziomie serwera, przykładowej bazy danych i reguły zapory na poziomie bazy danych oraz nawiązywanie połączenia z programem SQL Server Management Studio | Microsoft Docs"
description: "Dowiedz się, jak skonfigurować logiczny serwer usługi SQL Database, regułę zapory serwera, bazę danych SQL i przykładowe dane. Dowiedz się też, jak połączyć się z narzędziami klienta, skonfigurować użytkowników i regułę zapory bazy danych."
keywords: "samouczek usługi sql database, tworzenie bazy danych sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: overview
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 11/23/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: d772dabf84fc3b6c061c0f4607c989aabf9dd272
ms.openlocfilehash: 6d15839ce4084a808d9ecd5900a39a4f67dec822


---
# <a name="get-started-with-azure-sql-database-servers-databases-and-firewall-rules-by-using-the-azure-portal-and-sql-server-management-studio"></a>Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu witryny Azure Portal i programu SQL Server Management Studio

Z tego samouczka ułatwiającego rozpoczęcie pracy nauczysz się używać witryny Azure Portal do wykonywania następujących czynności:

* Tworzenie nowej grupy zasobów platformy Azure
* Tworzenie serwera logicznego usługi Azure SQL
* Wyświetlanie właściwości serwera logicznego usługi Azure SQL
* Tworzenie reguły zapory na poziomie serwera
* Tworzenie przykładowej bazy danych Adventure Works LT
* Wyświetlanie właściwości przykładowej bazy danych Adventure Works LT na platformie Azure

W tym samouczku używana jest również najnowsza wersja programu SQL Server Management Studio w celu wykonania następujących czynności:

* Nawiązywanie połączenia z serwerem logicznym i jego bazą danych master
* Wyświetlanie właściwości bazy danych master
* Nawiązywanie połączenia z przykładową bazą danych
* Wyświetlanie właściwości użytkownika bazy danych

Po ukończeniu tego samouczka uzyskasz przykładową bazę danych i pustą bazę danych, uruchomione w grupie zasobów platformy Azure i dołączone do serwera logicznego. Ponadto będziesz mieć regułę zapory na poziomie serwera skonfigurowaną tak, aby umożliwić podmiotowi zabezpieczeń na poziomie serwera logowanie się do serwera z określonego adresu IP (lub zakresu adresów IP). 

**Szacowany czas**: ten samouczek zajmie Ci około 30 minut (przy założeniu, że spełniasz już wymagania wstępne).

## <a name="prerequisites"></a>Wymagania wstępne

* Musisz mieć konto platformy Azure. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](/pricing/free-trial/?WT.mc_id=A261C142F) lub [aktywować korzyści dla subskrybentów programu Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F). 

* Musisz być w stanie połączyć się z witryną Azure Portal przy użyciu konta, które jest członkiem roli właściciela subskrypcji albo współautora. Aby uzyskać więcej informacji o kontroli dostępu na podstawie ról (RBAC, role-based access control), zobacz [Getting started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md) (Wprowadzenie do zarządzania dostępem w witrynie Azure Portal).

> [!TIP]
> Te same czynności w samouczku ułatwiającym rozpoczęcie pracy można wykonać przy użyciu języka [C#](sql-database-get-started-csharp.md) lub programu [PowerShell](sql-database-get-started-powershell.md).
>

### <a name="sign-in-by-using-your-existing-account"></a>Logowanie przy użyciu istniejącego konta
Za pomocą [istniejącej subskrypcji](https://account.windowsazure.com/Home/Index) wykonaj poniższe kroki, aby połączyć się z witryną Azure Portal.

1. Otwórz wybraną przeglądarkę i połącz się z witryną [Azure Portal](https://portal.azure.com/).
2. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
3. Kiedy zostanie wyświetlona strona **logowania**, podaj poświadczenia subskrypcji.
   
   ![Logowanie](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Tworzenie nowego serwera logicznego SQL w witrynie Azure Portal

1. Kliknij pozycję **Nowy**, wpisz ciąg **sql server**, a następnie naciśnij klawisz **ENTER**.

    ![serwer logiczny SQL](./media/sql-database-get-started/logical-sql-server.png)
2. Kliknij pozycję **SQL Server (serwer logiczny)**.
   
    ![tworzenie serwera logicznego SQL](./media/sql-database-get-started/create-logical-sql-server.png)
3. Kliknij pozycję **Utwórz**, aby otworzyć nowy blok SQL Server (serwer logiczny).

    ![nowy serwer logiczny SQL](./media/sql-database-get-started/new-logical-sql-server.png)
3. W polu tekstowym Nazwa serwera podaj prawidłową nazwę nowego serwera logicznego. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![nazwa nowego serwera](./media/sql-database-get-started/new-server-name.png)

    > [!IMPORTANT]
    > W pełni kwalifikowana nazwa nowego serwera to <Twoja_nazwa_serwera>.database.windows.net.
    >
    
4. W polu tekstowym Identyfikator logowania administratora serwera podaj nazwę użytkownika na potrzeby logowania w celu uwierzytelniania SQL dla tego serwera. Ta nazwa logowania jest nazywana główną nazwą logowania serwera. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![identyfikator logowania administratora SQL](./media/sql-database-get-started/sql-admin-login.png)
5. W polach tekstowych **Hasło** i **Potwierdź hasło** podaj hasło do głównego konta logowania serwera. Zielony znacznik wyboru wskazuje, czy podane hasło jest poprawne.
    
    ![Hasło administratora SQL](./media/sql-database-get-started/sql-admin-password.png)
6. Wybierz subskrypcję, w której masz uprawnienia do tworzenia obiektów.

    ![subskrypcja](./media/sql-database-get-started/subscription.png)
7. W polu tekstowym Grupa zasobów wybierz opcję **Utwórz nową**, a następnie w polu tekstowym Grupa zasobów podaj prawidłową nazwę nowej grupy (możesz także użyć istniejącej grupy zasobów, jeśli masz już taką utworzoną na własne potrzeby). Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.

    ![nowa grupa zasobów](./media/sql-database-get-started/new-resource-group.png)

8. W polu tekstowym **Lokalizacja** wybierz centrum danych odpowiednie dla danej lokalizacji — np. „Australia Wschodnia”.
    
    ![lokalizacja serwera](./media/sql-database-get-started/server-location.png)
    
    > [!TIP]
    > W tym bloku nie można zmienić pola wyboru dla opcji **Zezwalaj usługom platformy Azure na dostęp do serwera**. To ustawienie można zmienić w bloku zapory serwera. Aby uzyskać więcej informacji, zobacz artykuł [Wprowadzenie do zabezpieczeń](sql-database-get-started-security.md).
    >
    
9. Kliknij przycisk **Utwórz**.

    ![przycisk Utwórz](./media/sql-database-get-started/create.png)

## <a name="view-the-logical-sql-server-properties-in-the-azure-portal"></a>Wyświetlanie właściwości serwera logicznego SQL Server w witrynie Azure Portal

1. W witrynie Azure Portal kliknij pozycję **Więcej usług**.

    ![Więcej usług](./media/sql-database-get-started/more-services.png)
2. W polu tekstowym Filtr wpisz ciąg **SQL**, a następnie kliknij gwiazdkę obok pozycji Serwery SQL, aby określić serwery SQL jako ulubione na platformie Azure. 

    ![ustawianie ulubionych](./media/sql-database-get-started/favorite.png)
3. W bloku domyślnym kliknij pozycję **Serwery SQL**, aby otworzyć listę serwerów SQL w subskrypcji platformy Azure. 

    ![nowy serwer SQL](./media/sql-database-get-started/new-sql-server.png)

4. Kliknij nowy serwer SQL, aby wyświetlić jego właściwości w witrynie Azure Portal. Kolejne samouczki pomogą Ci zrozumieć opcje dostępne w tym bloku.

    ![blok serwera SQL](./media/sql-database-get-started/sql-server-blade.png)
5. W obszarze Ustawienia kliknij pozycję **Właściwości**, aby wyświetlić różne właściwości serwera logicznego SQL.

    ![właściwości serwera SQL](./media/sql-database-get-started/sql-server-properties.png)
6. Skopiuj w pełni kwalifikowaną nazwę serwera do schowka, aby skorzystać z niej na dalszym etapie tego samouczka.

    ![pełna nazwa serwera SQL](./media/sql-database-get-started/sql-server-full-name.png)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Tworzenie reguły zapory na poziomie serwera w witrynie Azure Portal

1. W bloku serwera SQL, w obszarze Ustawienia, kliknij pozycję **Zapora**, aby otworzyć blok Zapora dla serwera SQL.

    ![zapora serwera SQL](./media/sql-database-get-started/sql-server-firewall.png)

2. Sprawdź wyświetlony adres IP klienta i przy użyciu wybranej przeglądarki zweryfikuj w Internecie, czy jest to Twój adres IP (zadaj pytanie „jaki jest mój adres IP”). Czasami te adresy nie zgadzają się z różnych powodów.

    ![Twój adres IP](./media/sql-database-get-started/your-ip-address.png)

3. Zakładając, że adresy IP się zgadzają, na pasku narzędzi kliknij pozycję **Dodaj adres IP klienta**.

    ![dodawanie adresu IP klienta](./media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Zaporę usługi SQL Database możesz otworzyć na serwerze dla pojedynczego adresu IP lub dla całego zakresu adresów. Otwarcie zapory umożliwia administratorom SQL i użytkownikom logowanie się do dowolnej bazy danych na serwerze, dla którego mają prawidłowe poświadczenia.
    >

4. Kliknij pozycję **Zapisz** na pasku narzędzi, aby zapisać tę regułę zapory na poziomie serwera, a następnie kliknij przycisk **OK**.

    ![dodawanie adresu IP klienta](./media/sql-database-get-started/save-firewall-rule.png)

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Nawiązywanie połączenia z serwerem SQL przy użyciu programu SQL Server Management Studio (SSMS)

1. Jeśli jeszcze tego nie zrobiono, pobierz i zainstaluj najnowszą wersję programu SSMS ze strony [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (Pobieranie programu SQL Server Management Studio). Aby użytkownik był na bieżąco, najnowsza wersja programu SSMS wyświetla monit z informacją o możliwości pobrania jego nowej wersji.

2. Po zainstalowaniu w polu wyszukiwania systemu Windows wpisz nazwę **Microsoft SQL Server Management Studio**, a następnie naciśnij klawisz **Enter**, aby otworzyć program SSMS:

    ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)
3. W oknie dialogowym Łączenie z serwerem wprowadź informacje konieczne do nawiązania połączenia z serwerem SQL przy użyciu opcji Uwierzytelnianie programu SQL Server.

    ![łączenie z serwerem](./media/sql-database-get-started/connect-to-server.png)
4. Kliknij przycisk **Połącz**.

    ![nawiązane połączenie z serwerem](./media/sql-database-get-started/connected-to-server.png)
5. W Eksploratorze obiektów rozwiń kolejno węzły **Bazy danych**, **Systemowe bazy danych** i **master**, aby wyświetlić obiekty w bazie danych master.

    ![baza danych master](./media/sql-database-get-started/master-database.png)
6. Kliknij prawym przyciskiem myszy pozycję **master**, a następnie kliknij pozycję **Nowe zapytanie**.

    ![odpytywanie bazy danych master](./media/sql-database-get-started/query-master-database.png)

8. W oknie zapytania wpisz następujące zapytanie:

   ```select * from sys.objects```

9.  Na pasku narzędzi kliknij przycisk **Wykonaj**, aby zwrócić listę wszystkich obiektów systemowych w bazie danych master.

    ![odpytywanie bazy danych master o obiekty systemowe](./media/sql-database-get-started/query-master-database-system-objects.png)

    > [!NOTE]
    > Aby poznać zabezpieczenia SQL, zobacz [Wprowadzenie do zabezpieczeń SQL](sql-database-get-started-security.md)
    >

## <a name="create-new-database-in-the-azure-portal-using-adventure-works-lt-sample"></a>Tworzenie nowej bazy danych w witrynie Azure Portal przy użyciu przykładu Adventure Works LT

1. W witrynie Azure Portal kliknij pozycję **Bazy danych SQL** w bloku domyślnym.

    ![Bazy danych SQL](./media/sql-database-get-started/new-sql-database.png)
2. W bloku Bazy danych SQL kliknij pozycję **Dodaj**.

    ![dodawanie bazy danych SQL](./media/sql-database-get-started/add-sql-database.png)
3. W bloku Baza danych SQL sprawdź automatycznie uzupełnione informacje.

    ![blok Baza danych SQL](./media/sql-database-get-started/sql-database-blade.png)
4. Podaj prawidłową nazwę bazy danych.

    ![nazwa bazy danych SQL](./media/sql-database-get-started/sql-database-name.png)
5. W obszarze Wybierz źródło kliknij pozycję **Przykład**, a następnie w obszarze Wybierz przykład kliknij pozycję **AdventureWorksLT [V12]**.
   
    ![Adventure Works LT](./media/sql-database-get-started/adventureworkslt.png)
6. W obszarze Serwer podaj nazwę użytkownika oraz hasło administratora serwera na potrzeby logowania.

    ![poświadczenia serwera](./media/sql-database-get-started/server-credentials.png)

    > [!NOTE]
    > Bazę danych można dodać do serwera jako pojedynczą bazę danych (jest to opcja domyślna) lub dodać ją do puli elastycznej SQL. Aby uzyskać więcej informacji o pulach elastycznych, zobacz artykuł [Pule elastyczne](sql-database-elastic-pool.md).
    >

7. W obszarze Warstwa cenowa zmień warstwę cenową na opcję **Podstawowa** (w razie potrzeby warstwę cenową można później zwiększyć, ale dla celów szkoleniowych zalecamy użycie najniższej warstwy).

    ![warstwa cenowa](./media/sql-database-get-started/pricing-tier.png)
8. Kliknij przycisk **Utwórz**.

    ![przycisk Utwórz](./media/sql-database-get-started/create.png)

## <a name="view-database-properties-in-the-azure-portal"></a>Wyświetlane właściwości bazy danych w witrynie Azure Portal

1. W bloku Bazy danych SQL kliknij nową bazę danych, aby wyświetlić jej właściwości w witrynie Azure Portal. Kolejne samouczki pomogą Ci zrozumieć opcje dostępne w tym bloku. 

    ![blok nowej przykładowej bazy danych](./media/sql-database-get-started/new-sample-db-blade.png)
2. Kliknij pozycję **Właściwości**, aby wyświetlić dodatkowe informacje o bazie danych.

    ![właściwości nowej przykładowej bazy danych](./media/sql-database-get-started/new-sample-db-properties.png)

3. Kliknij pozycję **Pokaż parametry połączenia bazy danych**.

    ![parametry połączenia nowej przykładowej bazy danych](./media/sql-database-get-started/new-sample-db-connection-strings.png)
4. Kliknij pozycję **Przegląd**, a następnie nazwę serwera w okienku Podstawy.
    
    ![okienko Podstawy nowej przykładowej bazy danych](./media/sql-database-get-started/new-sample-db-essentials-pane.png)
5. W okienku Podstawy serwera wyświetl nowo dodaną bazę danych.

    ![nowa przykładowa baza danych w okienku Podstawy serwera](./media/sql-database-get-started/new-sample-db-server-essentials-pane.png)

## <a name="connect-and-query-sample-database-using-sql-server-management-studio"></a>Łączenie i odpytywanie przykładowej bazy danych przy użyciu programu SQL Server Management Studio

1. Przełącz się do programu SQL Server Management Studio i w Eksploratorze obiektów kliknij pozycję **Bazy danych**, a następnie przycisk **Odśwież** na pasku narzędzi, aby wyświetlić przykładową bazę danych.

    ![nowa przykładowa baza danych w programie SSMS](./media/sql-database-get-started/new-sample-db-ssms.png)
2. W Eksploratorze obiektów rozwiń węzeł nowej bazy danych, aby wyświetlić jej obiekty.

    ![obiekty nowej przykładowej bazy danych w programie SSMS](./media/sql-database-get-started/new-sample-db-objects-ssms.png)
3. Kliknij prawym przyciskiem myszy przykładową bazę danych, a następnie kliknij pozycję **Nowe zapytanie**.

    ![zapytanie względem nowej przykładowej bazy danych w programie SSMS](./media/sql-database-get-started/new-sample-db-query-ssms.png)
4. W oknie zapytania wpisz następujące zapytanie:

   ```select * from sys.objects```
   
9.  Na pasku narzędzi kliknij przycisk **Wykonaj**, aby zwrócić listę wszystkich obiektów systemowych w przykładowej bazie danych.

    ![zapytanie względem nowej przykładowej bazy danych o obiekty systemowe w programie SSMS](./media/sql-database-get-started/new-sample-db-query-objects-ssms.png)

## <a name="create-a-new-blank-database-using-sql-server-management-studio"></a>Tworzenie nowej pustej bazy danych przy użyciu programu SQL Server Management Studio

1. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **Bazy danych**, a następnie kliknij pozycję **Nowa baza danych**.

    ![nowa pusta baza danych w programie SSMS](./media/sql-database-get-started/new-blank-database-ssms.png)

    > [!NOTE]
    > W programie SSMS można również automatycznie utworzyć skrypt tworzenia bazy danych w celu utworzenia nowej bazy danych przy użyciu języka Transact-SQL.
    >

2. W oknie dialogowym Nowa baza danych podaj nazwę bazy danych w polu tekstowym Nazwa bazy danych. 

    ![nazwa nowej pustej bazy danych w programie SSMS](./media/sql-database-get-started/new-blank-database-name-ssms.png)

3. W oknie dialogowym Nowa baza danych kliknij pozycję **Opcje**, a następnie zmień wersję na wartość **Podstawowa**.

    ![opcje nowej pustej bazy danych w programie SSMS](./media/sql-database-get-started/new-blank-database-options-ssms.png)

    > [!TIP]
    > Zapoznaj się z innymi opcjami w tym oknie dialogowym, które można zmodyfikować dla bazy danych Azure SQL Database. Aby uzyskać więcej informacji o tych opcjach, zobacz artykuł [Create Database](https://msdn.microsoft.com/library/dn268335.aspx) (Instrukcja CREATE DATABASE).
    >

4. Kliknij przycisk **OK**, aby utworzyć pustą bazę danych.
5. Po zakończeniu odśwież węzeł Baza danych w Eksploratorze obiektów, aby wyświetlić nowo utworzoną pustą bazę danych. 

    ![nowa pusta baza danych w Eksploratorze obiektów](./media/sql-database-get-started/new-blank-database-object-explorer.png)

> [!TIP]
> W trakcie nauki możesz zaoszczędzić pieniądze, usuwając nieużywane bazy danych. Bazy danych w wersji Podstawowa możesz przywrócić w ciągu siedmiu dni. Nie usuwaj jednak serwera. Jeśli to zrobisz, nie będzie można odzyskać ani serwera, ani jego usuniętych baz danych.
>


## <a name="next-steps"></a>Następne kroki
Po ukończeniu tego samouczka możesz zapoznać się z kilkoma dodatkowymi samouczkami, które poszerzą zdobytą wiedzę. 

* Jeśli chcesz zacząć poznawać zabezpieczenia usługi Azure SQL Database, zobacz artykuł [Wprowadzenie do zabezpieczeń](sql-database-get-started-security.md).
* Jeśli posługujesz się programem Excel, naucz się [nawiązywać połączenie z bazą danych SQL w programie Excel](sql-database-connect-excel.md).
* Jeśli chcesz zacząć programować, wybierz język programowania w obszarze [Biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md).
* Jeśli chcesz przenieść lokalne bazy danych programu SQL Server na platformę Azure, zobacz artykuł [Migrating a database to Azure SQL Database](sql-database-cloud-migrate.md) (Migracja bazy danych do usługi Azure SQL Database).
* Jeśli chcesz załadować dane z pliku CSV do nowej tabeli przy użyciu narzędzia wiersza polecenia BCP, zobacz artykuł [Loading data into SQL Database from a CSV file using BCP](sql-database-load-from-csv-with-bcp.md) (Ładowanie danych do usługi SQL Database z pliku CSV przy użyciu narzędzia BCP).
* Jeśli chcesz zacząć tworzyć tabele i inne obiekty, zobacz temat „Aby utworzyć tabelę” w artykule [Tworzenie tabeli](https://msdn.microsoft.com/library/ms365315.aspx).

## <a name="additional-resources"></a>Dodatkowe zasoby
[Co to jest SQL Database?](sql-database-technical-overview.md)




<!--HONumber=Dec16_HO1-->


