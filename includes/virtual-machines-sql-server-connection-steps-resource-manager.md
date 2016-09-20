### Konfigurowanie etykiety DNS dla publicznego adresu IP

Aby połączyć się z aparatem bazy danych programu SQL Server z Internetu, należy najpierw skonfigurować etykietę DNS dla publicznego adresu IP.

> [AZURE.NOTE] Etykiety DNS nie są wymagane, jeśli planujesz tylko połączenie z wystąpieniem programu SQL Server w ramach tej samej sieci wirtualnej lub połączenie lokalne.

Aby utworzyć etykietę DNS, wybierz najpierw **maszyny wirtualne** w portalu. Wybierz maszynę wirtualną programu SQL Server, aby wyświetlić jej właściwości.

1. W bloku maszyny wirtualnej wybierz **publiczny adres IP**.

    ![publiczny adres IP](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

2. We właściwościach publicznego adresu IP rozwiń sekcję **Konfiguracja**.

3. Wprowadź nazwę etykiety DNS. Ta nazwa to rekord A, którego można użyć do nawiązania połączenia z maszyną wirtualną programu SQL Server przy użyciu nazwy, a nie bezpośrednio adresu IP.

4. Kliknij przycisk **Zapisz**.

    ![etykieta DNS](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### Nawiązywanie połączenia z aparatem bazy danych z innego komputera

1. Na komputerze podłączonym do Internetu otwórz program SQL Server Management Studio (SSMS).

2. W oknie dialogowym **Connect to Server** (Łączenie z serwerem) lub **Connect to Database Engine** (Łączenie z aparatem bazy danych) edytuj wartość **Server name** (Nazwa serwera). Wprowadź pełną nazwę DNS maszyny wirtualnej (określoną w poprzednim zadaniu).

3. W polu **Authentication** (Uwierzytelnianie) wybierz opcję **SQL Server Authentication** (Uwierzytelnianie programu SQL Server).

5. W polu **Login** (Logowanie) wpisz nazwę prawidłowego identyfikatora logowania SQL.

6. W polu **Password** (Hasło) wpisz hasło logowania.

7. Kliknij przycisk **Connect** (Połącz).

    ![nawiązywanie połączenia w programie ssms](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)



<!--HONumber=sep16_HO1-->


