
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Tworzenie reguły zapory na poziomie serwera w witrynie Azure Portal

1. W bloku serwera SQL, w obszarze Ustawienia, kliknij pozycję **Zapora**, aby otworzyć blok Zapora dla serwera SQL.

    ![zapora serwera SQL](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png)
    
2. Sprawdź wyświetlony adres IP klienta i przy użyciu wybranej przeglądarki zweryfikuj w Internecie, czy jest to Twój adres IP (zadaj pytanie „jaki jest mój adres IP”). Czasami te adresy nie zgadzają się z różnych powodów.

    ![Twój adres IP](../articles/sql-database/media/sql-database-get-started/your-ip-address.png)

3. Zakładając, że adresy IP się zgadzają, na pasku narzędzi kliknij pozycję **Dodaj adres IP klienta**.

    ![dodawanie adresu IP klienta](../articles/sql-database/media/sql-database-get-started/add-client-ip.png)

    > [!NOTE]
    > Zaporę usługi SQL Database możesz otworzyć na serwerze dla pojedynczego adresu IP lub dla całego zakresu adresów. Otwarcie zapory umożliwia administratorom SQL i użytkownikom logowanie się do dowolnej bazy danych na serwerze, dla którego mają prawidłowe poświadczenia.
    >

4. Kliknij pozycję **Zapisz** na pasku narzędzi, aby zapisać tę regułę zapory na poziomie serwera, a następnie kliknij przycisk **OK**.

    ![dodawanie adresu IP klienta](../articles/sql-database/media/sql-database-get-started/save-firewall-rule.png)

> [!Tip]
> Aby zapoznać się z samouczkiem, zobacz artykuł [Samouczek usługi SQL Database: tworzenie serwera, reguły zapory na poziomie serwera, przykładowej bazy danych i reguły zapory na poziomie bazy danych oraz nawiązywanie połączenia z programem SQL Server](../articles/sql-database/sql-database-get-started.md).    
>


<!--HONumber=Nov16_HO5-->


