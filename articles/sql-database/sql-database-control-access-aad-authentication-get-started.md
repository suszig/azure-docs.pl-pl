---
title: "Uwierzytelnianie usługi AAD: zapory i uwierzytelnianie usługi Azure SQL Database oraz dostęp do niej | Microsoft Docs"
description: "W tym samouczku ułatwiającym rozpoczęcie pracy znajdziesz informacje na temat używania programu SQL Server Management Studio oraz języka Transact SQL do pracy z regułami zapory na poziomie serwera i na poziomie bazy danych, uwierzytelnianiem w usłudze Azure Active Directory, danymi logowania, użytkownikami i rolami na potrzeby udzielania praw dostępu i kontroli baz danych i serwerów usługi Azure SQL Database."
keywords: 
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 67797b09-f5c3-4ec2-8494-fe18883edf7f
ms.service: sql-database
ms.custom: authentication and authorization
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 7d061c083b23de823d373c30f93cccfe1c856ba3
ms.openlocfilehash: 8a6dc7d3dca80782a55e13b53180b1542b61544b


---
# <a name="azure-ad-authentication-access-and-database-level-firewall-rules"></a>Uwierzytelnianie usługi Azure AD, dostęp i reguły zapory na poziomie bazy danych
W tym samouczku znajdziesz informacje na temat używania programu SQL Server Management Studio do pracy z uwierzytelnianiem w usłudze Azure Active Directory, nazwami logowania, użytkownikami i rolami bazy danych na potrzeby udzielania praw dostępu i uprawnień do baz danych oraz serwerów usługi Azure SQL Database. Dowiesz się, jak wykonywać następujące czynności:

- Wyświetlanie uprawnień użytkownika w bazie danych master i bazach danych użytkowników
- Tworzenie danych logowania i użytkowników na podstawie uwierzytelniania usługi Azure Active Directory
- Przyznawanie użytkownikom uprawnień na poziomie całego serwera oraz określonej bazy danych
- Logowanie do bazy danych użytkownika jako użytkownik inny niż administrator
- Tworzenie reguł zapory na poziomie bazy danych dla użytkowników bazy danych
- Tworzenie reguł zapory na poziom serwera dla administratorów serwerów

**Szacowany czas**: ukończenie tego samouczka zajmuje około 45 minut (przy założeniu, że spełniasz już wymagania wstępne).

## <a name="prerequisites"></a>Wymagania wstępne

* Musisz mieć konto platformy Azure. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* Musisz być w stanie połączyć się z witryną Azure Portal przy użyciu konta, które jest członkiem roli właściciela subskrypcji albo współautora. Aby uzyskać więcej informacji o kontroli dostępu na podstawie ról (RBAC, role-based access control), zobacz [Getting started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md) (Wprowadzenie do zarządzania dostępem w witrynie Azure Portal).

* Ukończono kroki samouczka [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu witryny Azure Portal i programu SQL Server Management Studio](sql-database-get-started.md) lub samouczka w odpowiedniej [wersji dla programu PowerShell](sql-database-get-started-powershell.md). Jeśli nie, przed kontynuowaniem wykonaj kroki tego samouczka dotyczącego wymagań wstępnych lub uruchom skrypt PowerShell pod koniec pracy z tym samouczkiem w [wersji dla programu PowerShell](sql-database-get-started-powershell.md).

   > [!NOTE]
   > Wykonanie czynności opisanych w powiązanym samouczku dotyczącym uwierzytelniania w programie SQL Server, [Uwierzytelnianie SQL, nazwy logowania i konta użytkowników, role bazy danych, uprawnienia, reguły zapory na poziomie serwera oraz reguły zapory bazy danych na poziomie](sql-database-control-access-sql-authentication-get-started.md), jest opcjonalne, chociaż w tym samouczku omówiono pewne zagadnienia niepowtarzane tutaj. Opisane w tym samouczku procedury związane z zaporami na poziomie bazy danych i serwera nie są wymagane, jeśli czynności z powiązanego samouczka zostały wykonane na tych samych komputerach (o tych samych adresach IP) i z tego powodu zostały oznaczone jako opcjonalne. Ponadto w przypadku zrzutów ekranu w tym samouczku założono, że wykonano czynności opisane w pokrewnym samouczku. 
   >

* Katalog usługi Azure Active Directory został utworzony i wypełniony. Aby uzyskać więcej informacji, zobacz tematy [Integrating your on-premises identities with Azure Active Directory](../active-directory/active-directory-aadconnect.md) (Integrowanie tożsamości lokalnych z usługą Azure Active Directory), [Dodawanie niestandardowej nazwy domeny do usługi Azure AD](../active-directory/active-directory-add-domain.md), [Microsoft Azure now supports federation with Windows Server Active Directory](https://azure.microsoft.com/blog/2012/11/28/windows-azure-now-supports-federation-with-windows-server-active-directory/) (Platforma Microsoft Azure obsługuje teraz federację z usługą Windows Servder Active Directory), [Administering your Azure AD directory](https://msdn.microsoft.com/library/azure/hh967611.aspx) (Administrowanie katalogiem usługi Azure AD), [Manage Azure AD using Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) (Zarządzanie usługą Azure AD przy użyciu programu Windows PowerShell) i [Hybrid Identity Required Ports and Protocols](../active-directory/active-directory-aadconnect-ports.md) (Wymagane porty i protokoły tożsamości hybrydowych).

> [!NOTE]
> Ten samouczek ułatwia opanowanie informacji podanych w następujących tematach szkoleniowych: [Kontrola dostępu w usłudze Azure SQL Database](sql-database-control-access.md), [Logins, users, and database roles](sql-database-manage-logins.md) (Dane logowania, użytkownicy i role bazy danych), [Principals](https://msdn.microsoft.com/library/ms181127.aspx) (Podmioty zabezpieczeń), [Database roles](https://msdn.microsoft.com/library/ms189121.aspx) (Role bazy danych), [Omówienie reguł zapory usługi Azure SQL Database](sql-database-firewall-configure.md) oraz [Azure Active Directory authentication](sql-database-aad-authentication.md) (Uwierzytelnianie usługi Azure Active Directory). 
>  

## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Logowanie do witryny Azure Portal przy użyciu konta platformy Azure
Za pomocą [istniejącej subskrypcji](https://account.windowsazure.com/Home/Index) wykonaj poniższe kroki, aby połączyć się z witryną Azure Portal.

1. Otwórz wybraną przeglądarkę i połącz się z witryną [Azure Portal](https://portal.azure.com/).
2. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
3. Kiedy zostanie wyświetlona strona **logowania**, podaj poświadczenia subskrypcji.
   
   ![Logowanie](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="provision-an-azure-active-directory-admin-for-your-sql-logical-server"></a>Aprowizowanie administratora usługi Azure Active Directory na potrzeby logicznego serwera SQL

W tej części samouczka znajdziesz informacje na temat konfiguracji zabezpieczeń serwera logicznego w witrynie Azure Portal.

1. Otwórz blok **SQL Server** dla serwera logicznego i przejrzyj informacje na stronie **Omówienie**. Zauważ, że administrator usługi Azure Active Directory nie został skonfigurowany.

   ![Konto administratora serwera w witrynie Azure Portal](./media/sql-database-control-access-aad-authentication-get-started/sql_admin_portal.png)

2. Kliknij pozycję **Nie skonfigurowano** w okienku **Podstawy**, aby otworzyć blok **administratora usługi Active Directory**.

   ![Blok usługi AAD](./media/sql-database-control-access-aad-authentication-get-started/aad_blade.png)

3. Kliknij pozycję **Ustaw administratora** w bloku **Dodawanie administratora**, a następnie wybierz konto użytkownika lub grupy usługi Active Directory jako administratora usługi Active Directory dla serwera.

   ![Wybieranie konta administratora usługi AAD](./media/sql-database-control-access-aad-authentication-get-started/aad_admin.png)

4. Kliknij przycisk **Wybierz**, a następnie kliknij pozycję **Zapisz**.

   ![Zapisywanie wybranego konta administratora usługi AAD](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_save.png)

> [!NOTE]
> Aby przejrzeć informacje o połączeniu dla tego serwera, przejdź do tematu [Manage servers](sql-database-manage-servers-portal.md) (Zarządzanie serwerami). W tej serii samouczków w pełni kwalifikowana nazwa serwera to „sqldbtutorialserver.database.windows.net”.
>

## <a name="connect-to-sql-server-using-sql-server-management-studio-ssms"></a>Nawiązywanie połączenia z serwerem SQL przy użyciu programu SQL Server Management Studio (SSMS)

1. Jeśli jeszcze tego nie zrobiono, pobierz i zainstaluj najnowszą wersję programu SSMS ze strony [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (Pobieranie programu SQL Server Management Studio). Aby użytkownik był na bieżąco, najnowsza wersja programu SSMS wyświetla monit z informacją o możliwości pobrania jego nowej wersji.

2. Po zainstalowaniu w polu wyszukiwania systemu Windows wpisz nazwę **Microsoft SQL Server Management Studio**, a następnie naciśnij klawisz **Enter**, aby otworzyć program SSMS.

   ![SQL Server Management Studio](./media/sql-database-get-started/ssms.png)

3. W oknie dialogowym **Łączenie z serwerem** wybierz jedną z metod uwierzytelniania usługi Active Directory, a następnie podaj odpowiednie informacje dotyczące uwierzytelniania. Aby uzyskać informacje na temat wybierania metody, zobacz [Azure Active Directory authentication](sql-database-aad-authentication.md) (Uwierzytelnianie w usłudze Azure Active Directory) i [SSMS support for Azure AD MFA](sql-database-ssms-mfa-authentication.md) (Obsługa programu SSMS dla usługi Azure AD MFA).

   ![łączenie z serwerem przy użyciu usługi aad](./media/sql-database-control-access-aad-authentication-get-started/connect_to_server_with_aad.png)

4. Wprowadź informacje konieczne do nawiązania połączenia z serwerem SQL przy użyciu opcji Uwierzytelnianie programu SQL Server i konta administratora serwera.

5. Kliknij przycisk **Połącz**.

   ![nawiązane połączenie z serwerem przy użyciu usługi aad](./media/sql-database-control-access-aad-authentication-get-started/connected_to_server_with_aad.png)

## <a name="view-the-server-admin-account-and-its-permissions"></a>Wyświetlanie konta administratora serwera i jego uprawnień 
W tej części samouczka można zapoznać się z informacjami na temat konta administratora serwera i jego uprawnień w bazie danych master i w bazach danych użytkowników.

1. W Eksploratorze obiektów rozwiń kolejno węzły **Bazy danych**, **Systemowe bazy danych**, **master** i **Zabezpieczenia**, a następnie rozwiń węzeł **Użytkownicy**. Zauważ, że konto użytkownika zostało utworzone w bazie danych master dla administratora usługi Active Directory. Zauważ również, że dla konta użytkownika administratora usługi Active Directory nie utworzono nazwy logowania.

   ![konto użytkownika bazy danych master dla administratora usługi AAD](./media/sql-database-control-access-aad-authentication-get-started/master_database_user_account_for_AAD_admin.png)

   > [!NOTE]
   > Informacje dotyczące innych wyświetlanych kont użytkowników można znaleźć w temacie [Principals](https://msdn.microsoft.com/library/ms181127.aspx) (Podmioty zabezpieczeń).
   >

2. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **master**, a następnie kliknij pozycję **Nowe zapytanie**, aby otworzyć okno zapytania połączone z bazą danych master.
3. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat użytkownika wykonującego zapytanie. Zauważ, że adres user@microsoft.com jest zwracany w przypadku konta użytkownika wykonującego to zapytanie (w dalszej części tej procedury zobaczymy inny wynik otrzymywany w przypadku przetwarzania zapytania dotyczącego bazy danych użytkowników).

   ```
   SELECT USER;
   ```

   ![wybieranie zapytania użytkownika w bazie danych master](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_master_database.png)

4. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat uprawnień użytkownika administratora usługi Active Directory. Zauważ, że administrator usługi Active Directory ma uprawnienia do łączenia z bazą danych master, tworzenia danych logowania i użytkowników, wybierania informacji z tabeli sys.sql_logins i dodawania użytkowników do ról bazy danych dbmanager i dbcreator. Te uprawnienia są dodatkiem do uprawnień udzielanych roli publicznej, z której wszyscy użytkownicy dziedziczą uprawnienia (na przykład uprawnienia do wybierania informacji z określonych tabel). Aby uzyskać więcej informacji, zobacz [Permissions](https://msdn.microsoft.com/library/ms191291.aspx) (Uprawnienia).

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals p
   JOIN sys.database_permissions prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'user@microsoft.com';
   ```

   ![uprawnienia administratora usługi aad w bazie danych master](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_master_database.png)

6. W Eksploratorze obiektów rozwiń kolejno węzły **blankdb**, **Zabezpieczenia** i **Użytkownicy**. Zauważ, że w tej bazie danych nie ma konta użytkownika o nazwie user@microsoft.com.

   ![konta użytkowników w bazie danych blankdb](./media/sql-database-control-access-aad-authentication-get-started/user_accounts_in_blankdb.png)

7. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **blankdb**, a następnie kliknij pozycję **Nowe zapytanie**.

8. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat użytkownika wykonującego zapytanie. Zauważ, że wartość dbo jest zwracana dla konta użytkownika wykonującego to zapytanie (domyślnie dane logowania administratora serwera są mapowane na konto użytkownika dbo w każdej bazie danych użytkowników).

   ```
   SELECT USER;
   ```

   ![wybieranie zapytania użytkownika w bazie danych blankdb](./media/sql-database-control-access-aad-authentication-get-started/select_user_query_in_blankdb_database.png)

9. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat uprawnień użytkownika dbo. Zauważ, że użytkownik dbo jest członkiem roli publicznej oraz członkiem stałej roli bazy danych. Aby uzyskać więcej informacji, zobacz [Database-Level Roles](https://msdn.microsoft.com/library/ms189121.aspx).

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'dbo';
   ```

   ![uprawnienia administratora serwera w bazie danych blankdb](./media/sql-database-control-access-aad-authentication-get-started/aad_admin_permissions_in_blankdb_database.png)

10. Opcjonalnie powtórz poprzednie trzy kroki dla bazy danych użytkowników AdventureWorksLT.

## <a name="create-a-new-user-in-the-adventureworkslt-database-with-select-permissions"></a>Tworzenie nowego użytkownika z uprawnieniami SELECT w bazie danych AdventureWorksLT

W tej części samouczka utworzysz konto użytkownika w bazie danych AdventureWorksLT na podstawie nazwy głównej użytkownika usługi Azure AD lub nazwy wyświetlanej grupy usługi Azure AD, przetestujesz uprawnienia tego użytkownika jako członka roli publicznej, udzielisz temu użytkownikowi uprawnień SELECT i ponownie przetestujesz uprawnienia tego użytkownika.

> [!NOTE]
> Użytkownicy na poziomie bazy danych ([zawarci użytkownicy](https://msdn.microsoft.com/library/ff929188.aspx)) zwiększają możliwość przenoszenia bazy danych, którą omówimy w kolejnych samouczkach.
>

1. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **AdventureWorksLT**, a następnie kliknij pozycję **Nowe zapytanie**, aby otworzyć okno zapytania połączone z bazą danych AdventureWorksLT.
2. Wykonaj następującą instrukcję, aby utworzyć konto użytkownika w bazie danych AdventureWorksLT dla użytkownika w domenie firmy Microsoft o nazwie aaduser1.

   ```
   CREATE USER [aaduser1@microsoft.com]
   FROM EXTERNAL PROVIDER;
   ```
   ![nowy użytkownik aaduser1@microsoft.com w bazie AdventureWorksLT](./media/sql-database-control-access-aad-authentication-get-started/new_user_aaduser1@microsoft.com_aw.png)

3. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat uprawnień użytkownika user1. Zauważ, że jedyne uprawnienia użytkownika user1 to uprawnienia dziedziczone z roli publicznej.

   ```
   SELECT prm.permission_name
      , prm.class_desc
      , prm.state_desc
      , p2.name as 'Database role'
      , p3.name as 'Additional database role' 
   FROM sys.database_principals AS p
   JOIN sys.database_permissions AS prm
      ON p.principal_id = prm.grantee_principal_id
      LEFT JOIN sys.database_principals AS p2
      ON prm.major_id = p2.principal_id
      LEFT JOIN sys.database_role_members r
      ON p.principal_id = r.member_principal_id
      LEFT JOIN sys.database_principals AS p3
      ON r.role_principal_id = p3.principal_id
   WHERE p.name = 'aaduser1@microsoft.com';
   ```

   ![uprawnienia nowego użytkownika w bazie danych użytkowników](./media/sql-database-control-access-aad-authentication-get-started/new_user_permissions_in_user_database.png)

4. Wykonaj następujące zapytania w celu podjęcia próby wysłania zapytania dotyczącego tabeli w bazie danych AdventureWorksLT jako user1.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![brak uprawnienia select](./media/sql-database-control-access-aad-authentication-get-started/no_select_permissions.png)

5. Wykonaj następującą instrukcję, aby przyznać użytkownikowi user1 uprawnienia SELECT w tabeli ProductCategory w schemacie SalesLT.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to [aaduser1@microsoft.com];
   ```

   ![udzielanie uprawnień select](./media/sql-database-control-access-aad-authentication-get-started/grant_select_permissions.png)

6. Wykonaj następujące zapytania w celu podjęcia próby wysłania zapytania dotyczącego tabeli w bazie danych AdventureWorksLT jako user1.

   ```
   EXECUTE AS USER = 'aaduser1@microsoft.com';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![wybieranie uprawnień](./media/sql-database-control-access-aad-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-for-adventureworkslt-database-users"></a>Tworzenie reguły zapory na poziomie bazy danych dla użytkowników bazy danych AdventureWorksLT

> [!NOTE]
> Nie musisz wykonywać tej procedury, jeśli została ukończona równoważna procedura w powiązanym samouczku dotyczącym uwierzytelniania w programie SQL Server, [Uwierzytelnianie i autoryzacja SQL](sql-database-control-access-sql-authentication-get-started.md), i uczysz się, używając komputera z tym samym adresem IP.
>

W tej części samouczka spróbujesz się zalogować przy użyciu nowego konta użytkownika z komputera z innym adresem IP, utworzyć regułę zapory na poziomie bazy danych jako administratora serwera, a następnie pomyślnie zalogować się przy użyciu tej nowej reguły zapory na poziomie bazy danych. 

> [!NOTE]
> [Reguły zapory na poziomie bazy danych](sql-database-firewall-configure.md) zwiększają możliwość przenoszenia bazy danych, którą omówimy w kolejnych samouczkach.
>

1. Na innym komputerze, dla którego jeszcze nie utworzono reguły zapory na poziomie serwera, otwórz program SQL Server Management Studio.

   > [!IMPORTANT]
   > Zawsze używaj najnowszej wersji programu SSMS podanej w temacie [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (Pobieranie programu SQL Server Management Studio). 
   >

2. W oknie **Łączenie z serwerem** wprowadź nazwę serwera i informacje dotyczące uwierzytelniania, aby połączyć się przy użyciu uwierzytelniania programu SQL Server z konta aaduser1@microsoft.com. 
    
   ![Łączenie jako użytkownik aaduser1@microsoft.com bez reguły zapory rule1](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule1.png)

3. Kliknij przycisk **Opcje**, aby określić bazę danych, z którą chcesz się połączyć, a następnie wpisz ciąg **AdventureWorksLT** w polu listy rozwijanej **Połącz z bazą danych** na karcie **Właściwości połączenia**.
   
   ![Łączenie jako użytkownik aaduser1 bez reguły zapory rule2](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule2.png)

4. Kliknij przycisk **Połącz**. Zostanie wyświetlone okno dialogowe z informacją o tym, że komputer, z którego próbujesz połączyć się z bazą danych SQL, nie ma reguły zapory umożliwiającej dostęp do bazy danych. Wyświetlone okno dialogowe ma dwie odmiany w zależności od poprzednio wykonanych kroków dotyczących zapór, ale przeważnie pojawia się pierwsze okno dialogowe.

   ![Łączenie jako użytkownik user1 bez reguły zapory rule3](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule3.png)

   ![Łączenie jako użytkownik user1 bez reguły zapory rule4](./media/sql-database-control-access-aad-authentication-get-started/connect_aaduser1_no_rule4.png)

   > [!NOTE]
   > Najnowsza wersja programu SSMS oferuje funkcje umożliwiające współautorom i właścicielom subskrypcji logowanie się do platformy Microsoft Azure i tworzenie reguły zapory na poziomie serwera.
   > 

4. Skopiuj adres IP klienta z tego okna dialogowego do użycia w kroku 7.
5. Kliknij przycisk **Anuluj** , ale nie zamykaj okna dialogowego **Łączenie z serwerem**.
6. Przełącz się z powrotem do komputera, dla którego utworzono już regułę zapory na poziomie serwera, i połącz się z serwerem przy użyciu konta administratora serwera.
7. W nowym oknie zapytania połączonym z bazą danych AdventureWorksLT jako administrator serwera wykonaj następującą instrukcję, aby utworzyć zaporę na poziomie bazy danych, wykonując instrukcję [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) przy użyciu adresu IP z kroku 4:

   ```
   EXEC sp_set_database_firewall_rule @name = N'AdventureWorksLTFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![dodawanie reguły zapory na poziomie bazy danych rule4](./media/sql-database-control-access-aad-authentication-get-started/aaduser1_add_rule_aw.png)

8. Przełącz ponownie komputery, a następnie kliknij przycisk **Połącz** w oknie dialogowym **Łączenie z serwerem**, aby połączyć się z bazą danych AdventureWorksLT jako użytkownik aaduser1. 

9. W Eksploratorze obiektów rozwiń kolejno węzły **Bazy danych**, **AdventureWorksLT** oraz **Tabele**. Zauważ, że użytkownik user1 ma uprawnienie tylko do przeglądania pojedynczej tabeli **SalesLT.ProductCategory**. 

10. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **SalesLT.ProductCategory** i kliknij pozycję **Zaznacz 1000 pierwszych wierszy**.   

## <a name="next-steps"></a>Następne kroki
- Dostęp i kontrola w usłudze SQL Database zostały omówione w temacie [Kontrola dostępu w usłudze SQL Database](sql-database-control-access.md).
- Dane logowania, użytkownicy i role bazy danych w usłudze SQL Database zostały omówione w temacie [Logins, users, and database roles](sql-database-manage-logins.md) (Dane logowania, użytkownicy i role bazy danych).
- Aby uzyskać więcej informacji na temat podmiotów zabezpieczeń bazy danych, zobacz [Principals](https://msdn.microsoft.com/library/ms181127.aspx) (Podmioty zabezpieczeń).
- Aby uzyskać więcej informacji na temat ról bazy danych, zobacz [Database roles](https://msdn.microsoft.com/library/ms189121.aspx) (Role bazy danych).
- Aby uzyskać więcej informacji na temat reguł zapory w usłudze SQL Database, zobacz [Omówienie reguł zapory usługi SQL Database](sql-database-firewall-configure.md).




<!--HONumber=Feb17_HO3-->


