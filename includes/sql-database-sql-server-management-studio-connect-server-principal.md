

## Nawiązywanie połączenia z bazą danych SQL Azure przy użyciu głównego identyfikatora logowania poziomu serwera

Wykonaj następujące kroki, aby nawiązać połączenie z bazą danych SQL Azure za pomocą narzędzia SSMS przy użyciu głównego identyfikatora logowania poziomu serwera.

1. W polu wyszukiwania systemu Windows wpisz „Microsoft SQL Server Management Studio”, a następnie kliknij aplikację klasyczną, aby uruchomić program SSMS.

2. W oknie połączenia z serwerem wprowadź następujące informacje:

 - **Typ serwera**: domyślnie jest to aparat bazy danych; tej wartości nie należy zmieniać.
 - **Nazwa serwera**: wprowadź nazwę serwera hostującego bazę danych SQL w następującym formacie: *&lt;nazwa_serwera >*.**database.windows.net**
 - **Typ uwierzytelniania**: jeśli dopiero rozpoczynasz pracę, wybierz opcję Uwierzytelnianie SQL. Jeśli dla serwera logicznego bazy danych SQL została włączona usługa Active Directory, możesz wybrać opcję Uwierzytelnianie za pomocą hasła w usłudze Active Directory lub Zintegrowane uwierzytelnianie usługi Active Directory.
 - **Nazwa użytkownika**: w przypadku wybrania uwierzytelniania SQL lub uwierzytelniania za pomocą hasła w usłudze Active Directory wprowadź nazwę użytkownika z dostępem do bazy danych na serwerze.
 - **Hasło**: w przypadku wybrania uwierzytelniania SQL lub uwierzytelniania za pomocą hasła w usłudze Active Directory wprowadź hasło dla określonego użytkownika.
   
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-1.png)

3. Kliknij przycisk **Połącz**.
 
4. Jeśli adres IP klienta nie ma dostępu do serwera logicznego bazy danych SQL, zostanie wyświetlony monit o zalogowanie się na koncie platformy Azure i utworzenie reguły zapory poziomu serwera. Jeśli jesteś administratorem subskrypcji platformy Azure, kliknij przycisk **Zaloguj**, aby utworzyć regułę zapory poziomu serwera. Jeśli nie, poproś administratora platformy Azure o utworzenie reguły zapory poziomu serwera.
 
      ![SQL Server Management Studio: łączenie z serwerem bazy danych SQL](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-2.png)
 
1. Jeśli jesteś administratorem subskrypcji platformy Azure i musisz się zalogować, po wyświetleniu strony logowania podaj poświadczenia dla subskrypcji i zaloguj się.

      ![logowanie](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-3.png)
 
1. Po pomyślnym zalogowaniu się do konta platformy Azure sprawdź proponowaną regułę zapory poziomu serwera (możesz ją zmodyfikować, aby zezwolić na dostęp do zakresu adresów IP). Następnie kliknij przycisk **OK**, aby utworzyć regułę zapory i zakończyć połączenie z bazą danych SQL.
 
      ![nowa zapora poziomu serwera](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-4.png)
 
5. Jeśli poświadczenia umożliwiają dostęp, zostanie otwarty Eksplorator obiektów i będzie można wykonywać zadania administracyjne lub wysyłać zapytania dotyczące danych. 
 
     ![nowa zapora poziomu serwera](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-5.png)
 
     
## Rozwiązywanie problemów z błędami połączeń

Najczęstszą przyczyną błędów połączeń są błędy w nazwie serwera (należy pamiętać, że <*nazwa_serwera*> jest nazwą serwera logicznego, a nie bazy danych), nazwie użytkownika lub haśle, jak również brak zezwolenia serwera na połączenie ze względów bezpieczeństwa. 





<!--HONumber=Jun16_HO2-->


