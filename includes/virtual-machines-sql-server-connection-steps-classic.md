### <a name="determine-the-dns-name-of-the-virtual-machine"></a>Określanie nazwy DNS maszyny wirtualnej
Aby połączyć się z aparatem bazy danych programu SQL Server z innego komputera, musisz znać nazwy systemu nazw domen (DNS, Domain Name System) maszyny wirtualnej. (Jest to nazwa, używanych przez internet na identyfikację maszyny wirtualnej. Możesz użyć adresu IP, ale adres IP mogą ulec zmianie, gdy Azure przenosi zasoby nadmiarowość lub konserwacji. Nazwa DNS będzie stabilny, ponieważ mogą zostać przekierowane do nowego adresu IP).  

1. W portalu Azure (lub w poprzednim kroku), wybierz **maszyn wirtualnych (klasyczne)**.
2. Wybierz maszynę Wirtualną programu SQL.
3. Na **maszyny wirtualnej** bloku, kopiowania **nazwy DNS** dla maszyny wirtualnej.
   
    ![Nazwa DNS](./media/virtual-machines-sql-server-connection-steps/sql-vm-dns-name.png)

### <a name="connect-to-the-database-engine-from-another-computer"></a>Nawiązywanie połączenia z aparatem bazy danych z innego komputera
1. Na komputerze podłączonym do Internetu Otwórz program SQL Server Management Studio.
2. W **Połącz z serwerem** lub **nawiązywanie połączenia z aparatem bazy danych** okna dialogowego, **nazwy serwera** wprowadź nazwę DNS maszyny wirtualnej (określoną w poprzednim zadaniu) i numeru portu publicznego punktu końcowego w formacie *DNSName, numer_portu* takich jak **mysqlvm.cloudapp.net,57500**.
   
    ![Połącz przy użyciu narzędzia SSMS](./media/virtual-machines-sql-server-connection-steps/33Connect-SSMS.png)
   
    Jeśli nie pamiętasz numeru portu publicznego punktu końcowego została wcześniej utworzona, można znaleźć w **punkty końcowe** obszar **maszyny wirtualnej** bloku.
   
    ![Port publiczny](./media/virtual-machines-sql-server-connection-steps/sql-vm-port-number.png)
3. W polu **Authentication** (Uwierzytelnianie) wybierz opcję **SQL Server Authentication** (Uwierzytelnianie programu SQL Server).
4. W **logowania** wpisz nazwę logowania, utworzony w starszej zadań.
5. W **hasło** wpisz hasło logowania, utworzone w wcześniejszych zadań.
6. Kliknij przycisk **Połącz**.

