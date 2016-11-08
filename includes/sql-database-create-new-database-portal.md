
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## Tworzenie nowej bazy danych SQL Azure
Wykonaj następujące kroki w witrynie Azure Portal, aby utworzyć nową bazę danych SQL Azure na nowym lub istniejącym serwerze logicznym usługi Azure SQL Database.

1. Połącz się z [witryną Azure Portal](http://portal.azure.com), jeśli jeszcze nie nawiązano z nią połączenia.
2. Kliknij opcję **Nowa**, wpisz **Baza danych SQL**, a następnie kliknij opcję **Baza danych SQL (nowa baza danych)**
   
     ![nowa baza danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)
3. Kliknij opcję Baza danych SQL (nowa baza danych).
   
     ![nowa baza danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)
4. Kliknij przycisk **Utwórz**, aby utworzyć nową bazę danych w usłudze SQL Database.
   
     ![nowa baza danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)
5. Podaj wartości następujących właściwości serwera:
   
   * Nazwa bazy danych
   * Subskrypcja (tylko w przypadku posiadania wielu subskrypcji)
   * Grupa zasobów (jeśli rozpoczynasz pracę, użyj grupy zasobów serwera logicznego)
   * Wybierz źródło (można wybrać pustą bazę danych, przykładowe dane lub kopię zapasową bazy danych platformy Azure — zobacz linki na końcu tego artykułu, aby uzyskać informacje na temat migrowania lokalnej bazy danych programu SQL Server lub załadowania danych przy użyciu narzędzia BCP)
   * Serwer (nowy lub istniejący serwer logiczny)
   * Hasło administratora serwera
   * Hasło
   * Warstwa cenowa (jeśli rozpoczynasz pracę, użyj wartości domyślnej S0)
   * Sortowanie (tylko w przypadku wybrania pustej bazy danych)
     
        ![new database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)
6. Po kliknięciu przycisku **Utwórz** w obszarze powiadomień będzie można zobaczyć, że rozpoczęło się wdrażanie.
   
    ![nowa baza danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)
7. Po ukończeniu wdrażania przejdź do następnego kroku.
   
     ![nowa baza danych](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)

<!--HONumber=sep16_HO1-->


