---
title: "Uwierzytelnianie SQL: zapory i uwierzytelnianie usługi Azure SQL Database oraz dostęp do niej | Microsoft Docs"
description: "W tym samouczku ułatwiającym rozpoczęcie pracy znajdziesz informacje na temat używania programu SQL Server Management Studio oraz języka Transact SQL do pracy z regułami zapory na poziomie serwera i na poziomie bazy danych, uwierzytelnianiem SQL, danymi logowania, użytkownikami i rolami na potrzeby udzielania praw dostępu i kontroli baz danych i serwerów usługi Azure SQL Database."
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
ms.date: 02/17/2017
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: c70b3b23fa95af6614c34bd951943f0559409220
ms.openlocfilehash: cf43790c329ef156ae17579d2281c861533ec201


---
# <a name="sql-server-authentication-access-and-database-level-firewall-rules"></a>Uwierzytelnianie programu SQL Server, dostęp i reguły zapory na poziomie bazy danych

W tym samouczku znajdziesz informacje na temat używania programu SQL Server Management Studio do pracy z uwierzytelnianiem w programie SQL Server, nazwami logowania, użytkownikami i rolami bazy danych na potrzeby udzielania praw dostępu i uprawnień do baz danych oraz serwerów usługi Azure SQL Database. Po ukończeniu tego samouczka opanujesz:

- Tworzenie danych logowania i użytkowników na podstawie uwierzytelniania programu SQL Server
- Dodawanie użytkowników do ról i udzielanie uprawnień rolom
- Używanie języka T-SQL do tworzenia reguły zapory na poziomie bazy danych i serwera 
- Łączenie się jako użytkownik z określoną bazą danych przy użyciu programu SSMS
- Wyświetlanie uprawnień użytkownika w bazie danych master i bazach danych użytkowników

**Szacowany czas**: ukończenie tego samouczka zajmuje około 45 minut (przy założeniu, że spełniasz już wymagania wstępne).

> [!NOTE]
> Ten samouczek ułatwia opanowanie informacji podanych w następujących tematach: [Kontrola dostępu w usłudze Azure SQL Database](sql-database-control-access.md), [Logins, users, and database roles](sql-database-manage-logins.md) (Nazwy logowania, użytkownicy i role bazy danych), [Principals](https://msdn.microsoft.com/library/ms181127.aspx) (Podmioty zabezpieczeń), [Database roles](https://msdn.microsoft.com/library/ms189121.aspx) (Role bazy danych) oraz [Omówienie reguł zapory usługi Azure SQL Database](sql-database-firewall-configure.md). Aby przejść do samouczka poświęconego uwierzytelnianiu za pomocą usługi Azure Active Directory, zobacz [Getting started with Azure AD Authentication](sql-database-control-access-aad-authentication-get-started.md) (Wprowadzenie do uwierzytelniania za pomocą usługi Azure AD).
>  

## <a name="prerequisites"></a>Wymagania wstępne

* **Konto platformy Azure**. Musisz mieć konto platformy Azure. Możesz [utworzyć konto bezpłatnej wersji próbnej Azure](https://azure.microsoft.com/free/) lub [aktywować korzyści dla subskrybentów programu Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/). 

* **Uprawnienia do tworzenia na platformie Azure**. Musisz być w stanie połączyć się z witryną Azure Portal przy użyciu konta, które jest członkiem roli właściciela subskrypcji albo współautora. Aby uzyskać więcej informacji o kontroli dostępu na podstawie ról (RBAC, role-based access control), zobacz [Getting started with access management in the Azure portal](../active-directory/role-based-access-control-what-is.md) (Wprowadzenie do zarządzania dostępem w witrynie Azure Portal).

* **SQL Server Management Studio**. Najnowszą wersję programu SSMS możesz zainstalować po pobraniu ze strony [Download SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx) (Pobieranie programu SQL Server Management Studio). Zawsze używaj najnowszej wersji programu SSMS podczas łączenia się z usługą Azure SQL Database, ponieważ cały czas wydawane są nowe funkcje.

* **Koniec podstawowego samouczka**. Ukończono kroki samouczka [Wprowadzenie do serwerów, baz danych i reguł zapory usługi Azure SQL Database przy użyciu witryny Azure Portal i programu SQL Server Management Studio](sql-database-get-started.md) lub samouczka w odpowiedniej [wersji dla programu PowerShell](sql-database-get-started-powershell.md). Jeśli nie, przed kontynuowaniem wykonaj kroki tego samouczka dotyczącego wymagań wstępnych lub uruchom skrypt PowerShell pod koniec pracy z tym samouczkiem w [wersji dla programu PowerShell](sql-database-get-started-powershell.md).



## <a name="sign-in-to-the-azure-portal-using-your-azure-account"></a>Logowanie do witryny Azure Portal przy użyciu konta platformy Azure
Kroki opisane w tej procedurze pokazują, jak łączyć się z witryną Azure Portal za pomocą konta platformy Azure] (https://account.windowsazure.com/Home/Index).

1. Otwórz wybraną przeglądarkę i połącz się z witryną [Azure Portal](https://portal.azure.com/).
2. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).
3. Kiedy zostanie wyświetlona strona **logowania**, podaj poświadczenia subskrypcji.
   
   ![Logowanie](./media/sql-database-get-started/login.png)


<a name="create-logical-server-bk"></a>

## <a name="view-logical-server-security-information-in-the-azure-portal"></a>Wyświetlanie informacji o zabezpieczeniach serwera logicznego w witrynie Azure Portal

Kroki opisane w tej procedurze pokazują, jak przeglądać informacje na temat konfiguracji zabezpieczeń serwera logicznego w witrynie Azure Portal.

1. Otwórz blok **SQL Server** dla serwera i przejrzyj informacje na stronie **Omówienie**.

   ![Konto administratora serwera w witrynie Azure Portal](./media/sql-database-control-access-sql-authentication-get-started/sql_admin_portal.png)

2. Zanotuj nazwę administratora serwera na serwerze logicznym. 

3. Jeśli nie pamiętasz hasła, kliknij pozycję **Resetuj hasło**, aby ustawić nowe hasło.

4. Jeśli potrzebujesz uzyskać informacje o połączeniu dla tego serwera, kliknij pozycję **Właściwości**.

## <a name="view-server-admin-permissions-using-ssms"></a>Wyświetlanie uprawnień administratora serwera przy użyciu programu SSMS

Kroki opisane w tej procedurze pokazują, jak wyświetlić informacje na temat konta administratora serwera i jego uprawnień w bazie danych master i w bazach danych użytkowników.

1. Otwórz program SQL Server Management Studio i połącz się ze swoim serwerem jako administrator serwera, używając uwierzytelniania programu SQL Server i konta administratora serwera.

   ![łączenie z serwerem](./media/sql-database-get-started/connect-to-server.png)

2. Kliknij przycisk **Połącz**.

   ![nawiązane połączenie z serwerem](./media/sql-database-get-started/connected-to-server.png)

3. W Eksploratorze obiektów rozwiń pozycję **Zabezpieczenia**, a następnie rozwiń pozycję **Nazwy logowania**, aby wyświetlić istniejące nazwy logowania dla swojego serwera — jedyna nazwa logowania na nowym serwerze to nazwa logowania dla konta administratora serwera.

   ![Identyfikator logowania administratora serwera](./media/sql-database-control-access-sql-authentication-get-started/server_admin_login.png)

4. W Eksploratorze obiektów rozwiń kolejno pozycje **Bazy danych**, **Systemowe bazy danych**, **master**, **Zabezpieczenia** i **Użytkownicy**, aby wyświetlić konto użytkownika, które zostało utworzone dla nazwy logowania administratora serwera w tej bazie danych.

   ![konto użytkownika bazy danych master dla administratora serwera](./media/sql-database-control-access-sql-authentication-get-started/master_database_user_account_for_server_admin.png)

   > [!NOTE]
   > Aby uzyskać informacje dotyczące innych kont użytkowników widocznych w węźle Użytkownicy, zobacz [Principals](https://msdn.microsoft.com/library/ms181127.aspx) (Podmioty zabezpieczeń).
   >

5. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **master**, a następnie kliknij pozycję **Nowe zapytanie**, aby otworzyć okno zapytania połączone z bazą danych master.
6. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat użytkownika wykonującego zapytanie. 

   ```
   SELECT USER;
   ```

   ![wybieranie zapytania użytkownika w bazie danych master](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_master_database.png)

7. W oknie zapytania uruchom następujące zapytanie, aby zwrócić informacje na temat uprawnień użytkownika sqladmin w bazie danych **master**. 

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
   WHERE p.name = 'sqladmin';
   ```

   ![uprawnienia administratora serwera w bazie danych master](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_master_database.png)

   >[!NOTE]
   > Administrator serwera ma uprawnienia do łączenia z bazą danych master, tworzenia nazw logowania i użytkowników, wybierania informacji z tabeli sys.sql_logins i dodawania użytkowników do ról bazy danych dbmanager i dbcreator. Te uprawnienia są dodatkiem do uprawnień udzielanych roli publicznej, z której wszyscy użytkownicy dziedziczą uprawnienia (na przykład uprawnienia do wybierania informacji z określonych tabel). Aby uzyskać więcej informacji, zobacz [Permissions](https://msdn.microsoft.com/library/ms191291.aspx) (Uprawnienia).
   >

8. W Eksploratorze obiektów rozwiń kolejno pozycje **blankdb**, **Zabezpieczenia** i **Użytkownicy**, aby wyświetlić konto użytkownika, które zostało utworzone dla nazwy logowania administratora serwera w tej bazie danych (i w każdej bazie danych użytkownika).

   ![konta użytkowników w bazie danych blankdb](./media/sql-database-control-access-sql-authentication-get-started/user_accounts_in_blankdb.png)

9. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **blankdb**, a następnie kliknij pozycję **Nowe zapytanie**.

10. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat użytkownika wykonującego zapytanie.

   ```
   SELECT USER;
   ```

   ![wybieranie zapytania użytkownika w bazie danych blankdb](./media/sql-database-control-access-sql-authentication-get-started/select_user_query_in_blankdb_database.png)

11. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat uprawnień użytkownika dbo. 

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

   ![uprawnienia administratora serwera w bazie danych blankdb](./media/sql-database-control-access-sql-authentication-get-started/server_admin_permissions_in_blankdb_database.png)

   > [!NOTE]
   > Użytkownik dbo jest członkiem roli publicznej oraz członkiem stałej roli bazy danych db_owner. Aby uzyskać więcej informacji, zobacz [Database-Level Roles](https://msdn.microsoft.com/library/ms189121.aspx).
   >

## <a name="create-a-new-user-with-select-permissions"></a>Tworzenie nowego użytkownika z uprawnieniami SELECT

Kroki opisane w tej procedurze pokazują, jak utworzyć użytkownika na poziomie bazy danych, przetestować domyślne uprawnienia nowego użytkownika (za pośrednictwem roli publicznej), przydzielić użytkownikowi uprawnienia **SELECT** i wyświetlić te zmodyfikowane uprawnienia.

> [!NOTE]
> Użytkownicy na poziomie bazy danych są także nazywani [zawartymi użytkownikami](https://msdn.microsoft.com/library/ff929188.aspx) i zwiększają przenośność bazy danych. Aby zapoznać się z informacjami na temat korzyści z przenośności, zobacz [Configure and manage Azure SQL Database security for geo-restore or failover to a secondary server](sql-database-geo-replication-security-config.md) (Konfigurowanie zabezpieczeń usługi Azure SQL Database i zarządzanie nimi w celu geograficznego przywracania lub przełączania w tryb failover na serwer pomocniczy).
>

1. W Eksploratorze obiektów kliknij prawym przyciskiem myszy pozycję **sqldbtutorialdb**, a następnie kliknij pozycję **Nowe zapytanie**.
2. Wykonaj następującą instrukcję w tym oknie zapytania, aby utworzyć użytkownika o nazwie **user1** w bazie danych sqldbtutorialdb.

   ```
   CREATE USER user1
   WITH PASSWORD = 'p@ssw0rd';
   ```
   ![nowy użytkownik user1 sqldbtutorialdb](./media/sql-database-control-access-sql-authentication-get-started/new_user_user1_aw.png)

3. W oknie zapytania wykonaj następujące zapytanie, aby zostały zwrócone informacje na temat uprawnień użytkownika user1.

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
   WHERE p.name = 'user1';
   ```

   ![uprawnienia nowego użytkownika w bazie danych użytkowników](./media/sql-database-control-access-sql-authentication-get-started/new_user_permissions_in_user_database.png)

   > [!NOTE]
   > Nowy użytkownik w bazie danych ma tylko uprawnienia odziedziczone z roli publicznej.
   >

4. Uruchom następujące zapytania za pomocą instrukcji **EXECUTE AS USER**, aby spróbować odpytać tabelę SalesLT.ProductCategory w bazie danych sqldbtutorialdb jako użytkownik **user1** wyłącznie z uprawnieniami odziedziczonymi z roli publicznej.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![brak uprawnienia select](./media/sql-database-control-access-sql-authentication-get-started/no_select_permissions.png)

   > [!NOTE]
   > Domyślnie rola publiczna nie przydziela uprawnień **SELECT** do obiektów użytkowników.
   >

5. Wykonaj następującą instrukcję, aby przyznać użytkownikowi **user1** uprawnienia **SELECT** do tabeli SalesLT.ProductCategory.

   ```
   GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
   ```

   ![udzielanie uprawnień select](./media/sql-database-control-access-sql-authentication-get-started/grant_select_permissions.png)

6. Wykonaj następujące zapytania, aby pomyślnie odpytać tabelę SalesLT.ProductCategory w bazie danych sqldbtutorialdb jako użytkownik **user1**.

   ```
   EXECUTE AS USER = 'user1';  
   SELECT * FROM [SalesLT].[ProductCategory];
   REVERT;
   ```

   ![wybieranie uprawnień](./media/sql-database-control-access-sql-authentication-get-started/select_permissions.png)

## <a name="create-a-database-level-firewall-rule-using-t-sql"></a>Tworzenie reguły zapory na poziomie bazy danych za pomocą języka T-SQL

Kroki opisane w tej procedurze pokazują, jak utworzyć regułę zapory na poziomie bazy danych przy użyciu systemowej procedury składowanej [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx). Reguła zapory na poziomie bazy danych pozwala administratorowi serwera na umożliwianie użytkownikom dostępu przez zaporę usługi Azure SQL Database tylko do konkretnych baz danych.

> [!NOTE]
> [Reguły zapory na poziomie bazy danych](sql-database-firewall-configure.md) zwiększają przenośność bazy danych. Aby zapoznać się z informacjami na temat korzyści z przenośności, zobacz [Configure and manage Azure SQL Database security for geo-restore or failover to a secondary server](sql-database-geo-replication-security-config.md) (Konfigurowanie zabezpieczeń usługi Azure SQL Database i zarządzanie nimi w celu geograficznego przywracania lub przełączania w tryb failover na serwer pomocniczy).
>

> [!IMPORTANT]
> Aby przetestować regułę zapory na poziomie bazy danych, połącz się z innego komputera (lub usuń regułę zapory na poziomie serwera w witrynie Azure Portal).
>

1. Otwórz program SQL Server Management Studio na komputerze, dla którego jeszcze nie utworzono reguły zapory na poziomie serwera.

2. W oknie **Łączenie z serwerem** wprowadź nazwę serwera i informacje dotyczące uwierzytelniania, aby połączyć się przy użyciu uwierzytelniania programu SQL Server z konta **user1**. 
    
   ![Łączenie jako użytkownik user1 bez reguły zapory rule1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule1.png)

3. Kliknij pozycję **Opcje**, aby określić bazę danych, z którą chcesz się połączyć, a następnie wpisz ciąg **sqldbtutorialdb** w polu rozwijanym **Połącz z bazą danych** na karcie **Właściwości połączenia**.
   
   ![Łączenie jako użytkownik user1 bez reguły zapory rule2](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule2.png)

4. Kliknij przycisk **Połącz**. 

   Zostanie wyświetlone okno dialogowe z informacją o tym, że komputer, z którego próbujesz połączyć się z bazą danych SQL, nie ma reguły zapory umożliwiającej dostęp do bazy danych. 

   ![Łączenie jako użytkownik user1 bez reguły zapory rule4](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_no_rule4.png)


5. Skopiuj adres IP klienta z tego okna dialogowego do użycia w kroku 8.
6. Kliknij przycisk **OK**, aby zamknąć okno dialogowe błędu, ale nie zamykaj okna dialogowego **Łączenie z serwerem**.
7. Przełącz się z powrotem do komputera, dla którego utworzono już regułę zapory na poziomie serwera. 
8. Połącz się z bazą danych sqldbtutorialdb w programie SSMS jako administrator serwera i wykonaj następującą instrukcję w celu utworzenia zapory na poziomie bazy danych przy użyciu adresu IP (lub zakresu adresów) z kroku 5.  

   ```
   EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

   ![dodawanie reguły zapory](./media/sql-database-control-access-sql-authentication-get-started/user1_add_rule_aw.png)

9. Przełącz ponownie komputery, a następnie kliknij pozycję **Połącz** w oknie dialogowym **Łączenie z serwerem**, aby połączyć się z bazą danych sqldbtutorialdb jako użytkownik user1. 

   > [!NOTE]
   > Po utworzeniu reguły zapory na poziomie bazy danych jej uaktywnienie może zająć do 5 minut.
   >

10. Po pomyślnym połączeniu się rozwiń węzeł **Bazy danych** w Eksploratorze obiektów. Zwróć uwagę, że użytkownik **user1** może tylko wyświetlać bazę danych **sqldbtutorialdb**.

   ![Łączenie jako użytkownik user1 z regułą zapory rule1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_rule1.png)

11. Rozwiń węzeł **sqldbtutorialdb**, a następnie rozwiń węzeł **Tabele**. Zauważ, że użytkownik user1 ma uprawnienie tylko do przeglądania pojedynczej tabeli **SalesLT.ProductCategory**. 

   ![Łączenie jako użytkownik user1 i wyświetlanie obiektu objects1](./media/sql-database-control-access-sql-authentication-get-started/connect-user1_view_objects1.png)

## <a name="create-a-new-user-as-dbowner-and-a-database-level-firewall-rule"></a>Tworzenia nowego użytkownika jako użytkownika db_owner i reguły zapory na poziomie bazy danych

Kroki opisane w tej procedurze pokazują, jak utworzyć użytkownika w innej bazie danych z uprawnieniami roli bazy danych db_owner i utworzyć zaporę na poziomie bazy danych dla tej innej bazy danych. Ten nowy użytkownik z członkostwem w roli **db_owner** będzie w stanie łączyć się wyłącznie z tą pojedynczą bazą danych i nią zarządzać.

1. Przełącz się do komputera z połączeniem z bazą danych SQL Database przy użyciu konta administratora serwera.
2. Otwórz okno zapytania połączone z bazą danych **blankdb** i wykonaj następującą instrukcję, aby utworzyć użytkownika o nazwie blankdbadmin w bazie danych blankdb.

   ```
   CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. W tym samym oknie zapytania wykonaj następującą instrukcję, aby dodać użytkownika blankdbadmin użytkownika do roli bazy danych db_owner. Ten użytkownik może teraz wykonywać wszystkie akcje niezbędne do zarządzania bazą danych blankdb.

   ```
   ALTER ROLE db_owner ADD MEMBER blankdbadmin; 
   ```

4. W tym samym oknie zapytania wykonaj następującą instrukcję, aby utworzyć zaporę na poziomie bazy danych przez wykonanie instrukcji [sp_set_database_firewall_rule](https://msdn.microsoft.com/library/dn270010.aspx) przy użyciu adresu IP z kroku 4 poprzedniej procedury (lub zakresu adresów IP dla użytkowników tej bazy danych):

   ```
   EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Przełącz komputery (do tego, dla którego utworzono regułę zapory na poziomie bazy danych) i połącz się z bazą danych blankdb przy użyciu konta użytkownika blankdbadmin.
6. Otwórz okno zapytania połączone z bazą danych blankdb i wykonaj następującą instrukcję, aby utworzyć użytkownika o nazwie blankdbuser1 w bazie danych blankdb.

   ```
   CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
   ```
 
7. Zgodnie z wymaganiami środowiska szkoleniowego utwórz dodatkową regułę zapory na poziomie bazy danych dla tego użytkownika. Jeśli jednak utworzono regułę zapory na poziomie bazy danych przy użyciu zakresu adresów IP, to może nie być konieczne.

## <a name="grant-dbmanager-permissions-and-create-a-server-level-firewall-rule"></a>Przydzielanie uprawnień dbmanager i tworzenie reguły zapory na poziomie serwera

Kroki opisane w tej procedurze pokazują, jak w bazie danych master utworzyć nazwę logowania i użytkownika z uprawnieniami do tworzenia nowych baz danych użytkownika i zarządzania nimi. Pokazują one również, jak utworzyć dodatkową regułę zapory na poziomie serwera przy użyciu języka Transact-SQL i instrukcji [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx). 

> [!IMPORTANT]
>Pierwszą regułę zapory na poziomie serwera należy zawsze tworzyć na platformie Azure (w witrynie Azure Portal, za pomocą programu PowerShell lub interfejsu API REST).
>

> [!IMPORTANT]
> Utworzenie nazw logowania w bazie danych master i utworzenie konta użytkownika na podstawie nazwy logowania jest wymagane, aby administrator serwera mógł delegować tworzenie uprawnień bazy danych do innego użytkownika. Jednak utworzenie nazw logowania, a następnie utworzenie na ich podstawie użytkowników zmniejsza przenośność środowiska.
>

1. Przełącz się do komputera z połączeniem z bazą danych SQL Database przy użyciu konta administratora serwera.
2. Otwórz okno zapytania połączone z bazą danych master i wykonaj następującą instrukcję, aby utworzyć logowanie o nazwie dbcreator w bazie danych master.

   ```
   CREATE LOGIN dbcreator
   WITH PASSWORD = 'p@ssw0rd';
   ```

3. W tym samym oknie zapytania 

   ```
   CREATE USER dbcreator
   FROM LOGIN dbcreator;
   ```

3. W tym samym oknie zapytania wykonaj następującą instrukcję, aby dodać użytkownika dbcreator użytkownika do roli bazy danych dbmanager. Ten użytkownik może teraz tworzyć bazy danych i zarządzać nimi.

   ```
   ALTER ROLE dbmanager ADD MEMBER dbcreator; 
   ```

4. W tym samym oknie zapytania wykonaj następujące zapytanie, aby utworzyć zaporę na poziomie serwera przez wykonanie instrukcji [sp_set_firewall_rule](https://msdn.microsoft.com/library/dn270017.aspx) przy użyciu odpowiedniego adresu IP w danym środowisku:

   ```
   EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
   ```

5. Przełącz komputery (do tego, dla którego utworzono regułę zapory na poziomie serwera) i połącz się z bazą danych master przy użyciu konta użytkownika dbcreator.
6. Otwórz okno zapytania związanego z bazą danych master i wykonaj następujące zapytanie w celu utworzenia bazy danych o nazwie foo.

   ```
   CREATE DATABASE FOO (EDITION = 'basic');
   ```
 7. Opcjonalnie usuń tę bazę danych w celu zaoszczędzenia pieniędzy za pomocą następującej instrukcji:

   ```
   DROP DATABASE FOO;
   ```

## <a name="complete-script"></a>Kompletny skrypt

Aby utworzyć nazwy logowania i użytkowników, dodać ich do ról, przyznać im uprawnienia, utworzyć reguły zapory na poziomie bazy danych i utworzyć reguły zapory na poziomie serwera, wykonaj poniższe instrukcje w odpowiednich bazach danych na serwerze.

### <a name="master-database"></a>baza danych master
Wykonaj te instrukcje w bazie danych master przy użyciu konta administratora serwera, dodając odpowiednie adresy IP lub ich zakresy.

```
CREATE LOGIN dbcreator WITH PASSWORD = 'p@ssw0rd';
CREATE USER dbcreator FROM LOGIN dbcreator;
ALTER ROLE dbmanager ADD MEMBER dbcreator;
EXEC sp_set_firewall_rule @name = N'dbcreatorFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="sqldbtutorialdb-database"></a>baza danych sqldbtutorialdb
Wykonaj te instrukcje w bazie danych sqldbtutorialdb przy użyciu konta administratora serwera, dodając odpowiednie adresy IP lub ich zakresy.

```
CREATE USER user1 WITH PASSWORD = 'p@ssw0rd';
GRANT SELECT ON OBJECT::[SalesLT].[ProductCategory] to user1;
EXEC sp_set_database_firewall_rule @name = N'sqldbtutorialdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
```

### <a name="blankdb-database"></a>baza danych blankdb
Wykonaj te instrukcje w bazie danych blankdb przy użyciu konta administratora serwera, dodając odpowiednie adresy IP lub ich zakresy.

```
CREATE USER blankdbadmin
   WITH PASSWORD = 'p@ssw0rd';
ALTER ROLE db_owner ADD MEMBER blankdbadmin;
EXEC sp_set_database_firewall_rule @name = N'blankdbFirewallRule', 
     @start_ip_address = 'x.x.x.x', @end_ip_address = 'x.x.x.x';
CREATE USER blankdbuser1
   WITH PASSWORD = 'p@ssw0rd';
```

## <a name="next-steps"></a>Następne kroki
- Dostęp i kontrola w usłudze SQL Database zostały omówione w temacie [Kontrola dostępu w usłudze SQL Database](sql-database-control-access.md).
- Dane logowania, użytkownicy i role bazy danych w usłudze SQL Database zostały omówione w temacie [Logins, users, and database roles](sql-database-manage-logins.md) (Dane logowania, użytkownicy i role bazy danych).
- Aby uzyskać więcej informacji na temat podmiotów zabezpieczeń bazy danych, zobacz [Principals](https://msdn.microsoft.com/library/ms181127.aspx) (Podmioty zabezpieczeń).
- Aby uzyskać więcej informacji na temat ról bazy danych, zobacz [Database roles](https://msdn.microsoft.com/library/ms189121.aspx) (Role bazy danych).
- Aby uzyskać więcej informacji na temat reguł zapory w usłudze SQL Database, zobacz [Omówienie reguł zapory usługi SQL Database](sql-database-firewall-configure.md).
- Samouczek dotyczący korzystania z uwierzytelniania usługi Azure Active Directory można znaleźć na stronie [Uwierzytelnianie i autoryzacja usługi Azure AD](sql-database-control-access-aad-authentication-get-started.md).




<!--HONumber=Feb17_HO3-->


