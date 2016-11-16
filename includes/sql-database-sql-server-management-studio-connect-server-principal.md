

## <a name="connect-to-azure-sql-database-using-sql-server-authentication"></a>Łączenie z bazą danych SQL platformy Azure za pomocą uwierzytelniania programu SQL Server
Poniższe kroki pokazują, jak nawiązać połączenie z serwerem i bazą danych SQL platformy Azure przy użyciu programu SSMS. Jeśli nie masz serwera i bazy danych, a chcesz je utworzyć, zobacz temat [Tworzenie bazy danych SQL w ciągu kilku minut](../articles/sql-database/sql-database-get-started.md).

1. Aby uruchomić program SSMS, w polu wyszukiwania systemu Windows wpisz nazwę **Microsoft SQL Server Management Studio**, a następnie kliknij aplikację klasyczną.
2. W oknie **Połącz z serwerem** wprowadź następujące informacje (jeśli program SSMS jest już uruchomiony, kliknij kolejno pozycje **Połącz > Aparat bazy danych**, aby otworzyć okno **Łączenie z serwerem**):
   
   * **Typ serwera**: domyślnie jest to aparat bazy danych; tej wartości nie należy zmieniać.
   * **Nazwa serwera**: wprowadź w pełni kwalifikowaną nazwę serwera bazy danych SQL platformy Azure w następującym formacie: *&lt;nazwa_serwera>*.**database.windows.net**
   * **Typ uwierzytelniania**: ten artykuł pokazuje, jak nawiązać połączenie przy użyciu **Uwierzytelniania programu SQL Server**. Szczegółowe informacje dotyczące nawiązywania połączenia z usługą Azure Active Directory znajdziesz w tematach [Connect using Active Directory integrated authentication](../articles/sql-database/sql-database-aad-authentication.md#connect-using-active-directory-integrated-authentication) (Nawiązywanie połączenia przy użyciu zintegrowanego uwierzytelniania usługi Active Directory), [Connect using Active Directory password authentication](../articles/sql-database/sql-database-aad-authentication.md#connect-using-active-directory-password-authentication) (Nawiązywanie połączenia przy użyciu uwierzytelniania hasła usługi Active Directory) i [Connect using Active Directory Universal Authentication](../articles/sql-database/sql-database-ssms-mfa-authentication.md) (Nawiązywanie połączenia przy użyciu uniwersalnego uwierzytelniania usługi Active Directory).
   * **Nazwa użytkownika**: wprowadź nazwę użytkownika z dostępem do bazy danych na serwerze (na przykład nazwę *administratora serwera* podaną podczas tworzenia serwera). 
   * **Hasło**: wprowadź hasło dla określonego użytkownika (na przykład *hasło* ustawione podczas tworzenia serwera).
     
       ![SQL Server Management Studio: łączenie z serwerem usługi SQL Database](./media/sql-database-sql-server-management-studio-connect-server-principal/connect.png)
3. Kliknij przycisk **Połącz**.
4. Domyślnie nowe serwery nie mają określonych [reguł zapory](../articles/sql-database/sql-database-firewall-configure.md), dlatego klienci mają zablokowaną możliwość nawiązywania połączenia. Jeśli serwer nie ma jeszcze reguły zapory umożliwiającej nawiązywanie połączenia z określonego adresu IP, program SSMS wysyła monit o utworzenie reguły zapory na poziomie serwera.
   
    Kliknij przycisk **Zaloguj** i utwórz regułę zapory na poziomie serwera. Aby utworzyć regułę zapory na poziomie serwera, musisz być administratorem platformy Azure.
   
       ![SQL Server Management Studio: Connect to SQL Database server](./media/sql-database-sql-server-management-studio-connect-server-principal/newfirewallrule.png)
5. Po pomyślnym połączeniu się z bazą danych Azure SQL Database zostanie otwarty **Eksplorator obiektów** i uzyskasz dostęp do bazy danych, aby móc [wykonywać zadania administracyjne lub zapytania do danych](../articles/sql-database/sql-database-manage-azure-ssms.md).
   
     ![nowa zapora poziomu serwera](./media/sql-database-sql-server-management-studio-connect-server-principal/connect-server-principal-5.png)

## <a name="troubleshoot-connection-failures"></a>Rozwiązywanie problemów z błędami połączeń
Najbardziej typowymi przyczynami błędów połączenia są błędy w nazwie serwera i problemy z połączeniem sieciowym. Należy pamiętać, że <*nazwa_serwera*> jest nazwą serwera, a nie bazy danych, poza tym należy podać w pełni kwalifikowaną nazwę serwera: `<servername>.database.windows.net`

Sprawdź również, czy nazwa użytkownika i hasło nie zawierają żadnych literówek ani dodatkowych spacji (dla nazwy użytkownika wielkość liter nie ma znaczenia, ale dla hasła już tak). 

Można także jawnie ustawić protokół i numer portu z nazwą serwera, jak pokazano poniżej: `tcp:servername.database.windows.net,1433`

Problemy z połączeniem sieciowym także mogą powodować błędy połączeń i limitów czasu. Próba ponownego nawiązania połączenia (gdy wiadomo, że nazwa serwera, poświadczenia i reguły zapory są poprawne) może rozwiązać problem.

Szczegóły oraz więcej informacji na temat problemów z łącznością zawiera temat [Rozwiązywanie i diagnozowanie błędów połączeń SQL oraz krótkotrwałych błędów bazy danych SQL i zapobieganie im](../articles/sql-database/sql-database-connectivity-issues.md).



<!--HONumber=Nov16_HO2-->


