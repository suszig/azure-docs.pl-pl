

## Przyznawanie uprawnień db_owner nowemu użytkownikowi bazy danych

Wykonaj poniższe kroki, aby przyznać istniejącemu użytkownikowi bazy danych uprawnienia db_owner.

W tych krokach przyjęto założenie, że ustanowiono połączenie z bazą danych SQL w Eksploratorze obiektów w programie SSMS oraz połączenie z serwerem logicznym bazy danych SQL z uprawnieniami głównego administratora na poziomie serwera lub z użyciem konta użytkownika z uprawnieniami umożliwiającymi przyznawanie uprawnień użytkownikom. 

1. W Eksploratorze obiektów rozwiń węzeł baz danych, a następnie wybierz bazę danych zawierającą użytkownika, któremu chcesz przydzielić uprawnienia dbo.

     ![SQL Server Management Studio: łączenie z serwerem bazy danych SQL](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Kliknij prawym przyciskiem myszy wybraną bazę danych, a następnie kliknij opcję **Zapytanie**.

     ![SQL Server Management Studio: łączenie z serwerem bazy danych SQL](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. W oknie zapytania edytuj następującą instrukcję Transact-SQL i użyj jej do przyznania uprawnień dbo określonemu użytkownikowi. 

    '''ALTER ROLE db_owner ADD MEMBER user1;

     ![SQL Server Management Studio: łączenie z serwerem bazy danych SQL](./media/sql-database-grant-database-user-dbo-permissions/sql-database-grant-database-user-dbo-permissions-1.png)





<!--HONumber=Jun16_HO2-->


