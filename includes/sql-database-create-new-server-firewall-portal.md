
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md
articles/sql-database/sql-database-configure-firewall-settings

-->
## Tworzenie nowej zapory poziomu serwera SQL Azure

Wykonaj następujące kroki w portalu Azure, aby utworzyć regułę zapory poziomu serwera, która umożliwia połączenia z indywidualnego adresu IP (komputera klienckiego) lub całego zakresu adresów IP z serwerem logicznym SQL. 

1. Połącz się z [portalem Azure](http://portal.azure.com), jeśli jeszcze nie nawiązano z nim połączenia.
2. W bloku domyślnym kliknij pozycję **Serwer SQL**.

    ![nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-1.png)

2. W bloku serwera SQL kliknij serwer SQL, na którym ma być utworzona reguła zapory. 

    ![nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-2.png)
           
3. Sprawdź właściwości serwera.

    ![nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-3.png)
      
4. W bloku Ustawienia kliknij opcję **Zapora**.

    ![nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-4.png)
    

    > [AZURE.IMPORTANT] Jeśli opcja **Zapora** nie jest widoczna w wyświetlanym bloku, cofnij się i upewnij, że wyświetlany jest blok serwera logicznego bazy danych SQL, a nie na blok bazy danych SQL.

5. Kliknij opcję **Dodaj adres IP klienta**, aby na platformie Azure została utworzona reguła dotycząca adresu IP klienta.

      ![nowa zapora serwera]    (./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-5.png)

6. Opcjonalnie kliknij dodany adres IP, aby edytować adres zapory w celu zezwolenia na dostęp do zakresu adresów IP.

      ![nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-6.png)
    
7. Kliknij przycisk **Zapisz**, aby utworzyć regułę zapory poziomu serwera.

     ![nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-7.png)

    >[AZURE.IMPORTANT] Adres IP klienta może od czasu do czasu ulegać zmianom i serwer może być niedostępny do momentu utworzenia nowej reguły zapory. Możesz sprawdzić adres IP przy użyciu usługi [Bing](http://www.bing.com/search?q=my%20ip%20address), a następnie dodać pojedynczy adres IP lub zakres adresów IP. Aby uzyskać szczegółowe informacje, zobacz temat [Zarządzanie ustawieniami zapory](sql-database-configure-firewall-settings.md#manage-existing-server-level-firewall-rules-through-the-azure-portal).



<!--HONumber=Jun16_HO2-->


