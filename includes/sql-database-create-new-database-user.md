

## Tworzenie nowego użytkownika bazy danych przy użyciu narzędzia SSMS

W następujących krokach opisano tworzenie nowego użytkownika bazy danych w istniejącej bazie danych przy użyciu narzędzia SSMS. 

W tych krokach przyjęto założenie, że użytkownik jest połączony z bazą danych SQL w Eksploratorze obiektów przy użyciu narzędzia SSMS, a także z serwerem logicznym bazy danych SQL jako podstawowy administrator na poziomie serwera lub za pomocą konta użytkownika z uprawnieniami do tworzenia nowego użytkownika. 

1. W Eksploratorze obiektów rozwiń węzeł Bazy danych, a następnie wybierz bazę danych, w której chcesz utworzyć nowe konto użytkownika.

     ![SQL Server Management Studio: łączenie z serwerem bazy danych SQL](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-1.png)

2. Kliknij prawym przyciskiem myszy wybraną bazę danych, a następnie kliknij opcję **Zapytanie**.

     ![SQL Server Management Studio: łączenie z serwerem bazy danych SQL](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-2.png)

3. W oknie zapytania edytuj następującą instrukcję Transact-SQL i użyj jej do utworzenia zawartego użytkownika w bazie danych użytkownika. 

    ```CREATE USER user1 WITH PASSWORD ='p@ssw0rd1';

     ![SQL Server Management Studio: łączenie z serwerem bazy danych SQL](./media/sql-database-create-new-database-user/sql-database-create-new-database-user-3.png)






<!--HONumber=Jun16_HO2-->


