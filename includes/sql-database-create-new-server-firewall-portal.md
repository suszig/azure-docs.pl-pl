
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md
articles/sql-database/sql-database-configure-firewall-settings

-->
## <a name="create-a-new-azure-sql-server-level-firewall"></a>Tworzenie nowej zapory poziomu serwera SQL Azure
Wykonaj następujące kroki w witrynie Azure Portal, aby utworzyć regułę zapory na poziomie serwera, która umożliwi połączenia z indywidualnego adresu IP (komputera klienckiego) lub całego zakresu adresów IP z serwerem logicznym usługi SQL Database.

1. Połącz się z [witryną Azure Portal](http://portal.azure.com), jeśli połączenie nie zostało nawiązane już wcześniej.
2. W bloku domyślnym kliknij pozycję **Serwery SQL**.
   
      ![Nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-1.png)
3. W bloku **Serwery SQL** kliknij serwer, dla którego ma zostać utworzona reguła zapory.
   
     ![Nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-2.png)
4. Sprawdź właściwości serwera, a następnie kliknij pozycję **Zapora**.
   
     ![Nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-3.png)
   
   > [!NOTE]
   > Dostęp do bloku **Ustawienia zapory** na poziomie serwera można uzyskać również z paska narzędzi bloku **Baza danych**.
    
    
6. Kliknij pozycję **Dodaj adres IP klienta**, aby na platformie Azure Twój adres IP został wprowadzony w pola reguł.
   
      ![Nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-5.png)
7. Opcjonalnie, aby zezwolić na dostęp dla zakresu adresów IP, kliknij dodany adres IP w celu zmodyfikowania adresu zapory.
   
      ![Nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-6.png)
8. Kliknij przycisk **Zapisz**, aby utworzyć regułę zapory poziomu serwera.
   
     ![Nowa zapora serwera](./media/sql-database-create-new-server-firewall-portal/sql-database-create-new-server-firewall-portal-7.png)
   
   > [!IMPORTANT]
   > Adres IP klienta może się zmieniać, co spowoduje, że serwer będzie niedostępny do momentu utworzenia nowej reguły zapory. Swój adres IP możesz sprawdzić za pomocą usługi [Bing](http://www.bing.com/search?q=my%20ip%20address). Następnie dodaj pojedynczy adres IP lub zakres adresów IP. Aby uzyskać szczegółowe informacje, zobacz temat [Zarządzanie ustawieniami zapory](../articles/sql-database/sql-database-configure-firewall-settings.md#manage-existing-server-level-firewall-rules-through-the-azure-portal).
   > 
   > 



<!--HONumber=Nov16_HO5-->


