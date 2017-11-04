### <a name="prerequisites"></a>Wymagania wstępne
* Konto platformy Azure; można utworzyć [bezpłatne konto](https://azure.microsoft.com/free)
* [Bazy danych SQL Azure](../articles/sql-database/sql-database-get-started.md) informacje o połączeniu, w tym nazwy serwera, nazwy bazy danych oraz nazwy użytkownika i hasła. Te informacje znajdują się w ciągu połączenia bazy danych SQL:
  
    Serwer = tcp:*yoursqlservername*. database.windows.net,1433;Initial katalogu =*yourqldbname*; Utrzymuj informacje o zabezpieczeniach = False; Nazwa użytkownika = {your_username}; Hasło = {your_password}; MultipleActiveResultSets = False; Szyfrowanie = True; TrustServerCertificate = False; Limit czasu połączenia = 30;
  
    Przeczytaj więcej na temat [baz danych SQL Azure](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Podczas tworzenia bazy danych SQL Azure można również utworzyć przykładowe bazy danych dołączone do programu SQL. 
> 
> 

Przed rozpoczęciem korzystania z bazy danych SQL Azure w aplikacji logiki, połączenia z bazą danych SQL. Można to zrobić łatwo w aplikacjach logiki w portalu Azure.  

Podłącz do Twojej bazy danych SQL Azure, wykonując następujące czynności:  

1. Tworzenie aplikacji logiki. W Projektancie aplikacji logiki dodać wyzwalacz, a następnie dodaj akcję. Wybierz **Pokaż Microsoft zarządzanych interfejsów API** na rozwijanej liście, a następnie wprowadź "sql" w polu wyszukiwania. Wybierz jedną z akcji:  
   
    ![Krok tworzenia połączenia SQL Azure](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Jeśli nie utworzono jeszcze żadnych połączenia z bazą danych SQL, zostanie wyświetlony monit o informacje dotyczące połączenia:  
   
    ![Krok tworzenia połączenia SQL Azure](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Wprowadź szczegóły bazy danych SQL. Właściwości oznaczone gwiazdką są wymagane.
   
   | Właściwość | Szczegóły |
   | --- | --- |
   | Łączenie za pośrednictwem bramy |Pozostaw zaznaczenie opcji. To jest używany podczas nawiązywania połączenia z lokalnego serwera SQL. |
   | Nazwa połączenia * |Wprowadź dowolną nazwę połączenia. |
   | Nazwa serwera SQL * |Wprowadź nazwę serwera; czyli przypominać *servername.database.windows.net*. Nazwa serwera jest wyświetlane we właściwościach bazy danych SQL w portalu Azure, a także wyświetlany w parametrach połączenia. |
   | Nazwa bazy danych SQL * |Wprowadź nazwę nadaną bazy danych SQL. To jest wyświetlany w oknie Właściwości bazy danych SQL w ciągu połączenia: Initial Catalog =*yoursqldbname*. |
   | Nazwa użytkownika * |Wprowadź nazwę użytkownika, utworzonej podczas tworzenia bazy danych SQL. Jest to wymienione we właściwościach bazy danych SQL w portalu Azure. |
   | Hasło * |Wprowadź hasło utworzone podczas tworzenia bazy danych SQL. |
   
    Te poświadczenia są używane do autoryzacji aplikację logiki, aby podłączyć się i uzyskać dostęp do danych SQL. Po wykonaniu tych czynności szczegóły połączenia wyglądać podobnie do następującego:  
   
    ![Krok tworzenia połączenia SQL Azure](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Wybierz pozycję **Utwórz**. 
5. Należy zauważyć, że utworzono połączenie. Teraz kontynuować pozostałe kroki w aplikacji logiki: 
   
    ![Krok tworzenia połączenia SQL Azure](./media/connectors-create-api-sqlazure/table.png)

