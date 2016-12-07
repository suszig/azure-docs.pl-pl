## <a name="connect-to-azure-sql-database-as-a-user"></a>Nawiązywanie połączenia z bazą danych SQL Azure jako użytkownik
Wykonaj poniższe kroki, aby nawiązać połączenie z bazą danych SQL Azure za pomocą narzędzia SSMS jako użytkownik.

1. W polu wyszukiwania systemu Windows wpisz „Microsoft SQL Server Management Studio”, a następnie kliknij aplikację klasyczną, aby uruchomić program SSMS.
2. W oknie połączenia z serwerem wprowadź następujące informacje:

* **Typ serwera**: domyślnie jest to aparat bazy danych; tej wartości nie należy zmieniać.
  
  * **Nazwa serwera**: wprowadź nazwę serwera hostującego bazę danych SQL w następującym formacie: *&lt;nazwa_serwera >*.**database.windows.net**
  * **Typ uwierzytelniania**: jeśli dopiero rozpoczynasz pracę, wybierz opcję Uwierzytelnianie SQL. Jeśli dla serwera logicznego bazy danych SQL została włączona usługa Active Directory, możesz wybrać opcję Uwierzytelnianie za pomocą hasła w usłudze Active Directory lub Zintegrowane uwierzytelnianie usługi Active Directory.
  * **Nazwa użytkownika**: w przypadku wybrania uwierzytelniania SQL lub uwierzytelniania za pomocą hasła w usłudze Active Directory wprowadź nazwę użytkownika z dostępem do bazy danych na serwerze.
  * **Hasło**: w przypadku wybrania uwierzytelniania SQL lub uwierzytelniania za pomocą hasła w usłudze Active Directory wprowadź hasło dla określonego użytkownika.
    
       ![SQL Server Management Studio: łączenie z serwerem usługi SQL Database](./media/sql-database-sql-server-management-studio-connect-user/connect-user-1.png)

1. Kliknij przycisk **Opcje**, aby określić bazę danych, z którą chcesz ustanowić połączenie.
   
      ![SQL Server Management Studio: łączenie z serwerem usługi SQL Database](./media/sql-database-sql-server-management-studio-connect-user/connect-user-2.png)
2. W oknie **Nawiązywanie połączenia z bazą danych** wybierz bazę danych, z którą chcesz ustanowić połączenie.
   
     ![SQL Server Management Studio: łączenie z serwerem usługi SQL Database](./media/sql-database-sql-server-management-studio-connect-user/connect-user-3.png)
3. Kliknij przycisk **Połącz**.
4. Jeśli adres IP klienta nie ma dostępu do serwera logicznego bazy danych SQL, zostanie wyświetlony monit o zalogowanie się na koncie platformy Azure i utworzenie reguły zapory poziomu serwera. Jeśli jesteś administratorem subskrypcji platformy Azure, kliknij przycisk **Zaloguj**, aby utworzyć regułę zapory poziomu serwera. Jeśli nie, poproś administratora o utworzenie reguły zapory poziomu serwera lub reguły zapory poziomu bazy danych w bazie danych, z którą próbujesz nawiązać połączenie.
   
      ![SQL Server Management Studio: łączenie z serwerem usługi SQL Database](./media/sql-database-sql-server-management-studio-connect-user/connect-user-4.png)
5. Jeśli poświadczenia umożliwiają dostęp do określonej bazy danych, zostanie otwarty Eksplorator obiektów i będzie można wykonywać zadania administracyjne lub wysyłać zapytania dotyczące danych, w zależności od uprawnień użytkownika.
   
      ![SQL Server Management Studio: łączenie z serwerem usługi SQL Database](./media/sql-database-sql-server-management-studio-connect-user/connect-user-5.png)

## <a name="troubleshoot-connection-failures"></a>Rozwiązywanie problemów z błędami połączeń
Najczęstszą przyczyną błędów połączeń są błędy w nazwie serwera (należy pamiętać, że <*nazwa_serwera*> jest nazwą serwera logicznego, a nie bazy danych), nazwie użytkownika lub haśle, jak również brak zezwolenia serwera na połączenie ze względów bezpieczeństwa. 



<!--HONumber=Nov16_HO2-->


