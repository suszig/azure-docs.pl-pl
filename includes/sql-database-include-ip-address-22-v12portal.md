
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, the following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through the new Azure portal
-->


1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

2. Na liście po lewej stronie wybierz **Przeglądaj**. 

3. Przewiń i wybierz **serwerów SQL**. 
   
    ![Znaleźć serwera bazy danych SQL Azure w portalu][b21-FindServerInPortal]
4. Dla wygody zminimalizować **Przeglądaj** bloku.

5. W polu filtru zacznij wpisywać nazwę serwera. Twoje wiersz jest wyświetlany.

6. Wybierz wiersz dla serwera. Zostanie wyświetlony blok serwera.

7. W bloku serwera, wybierz **ustawienia**. 

8. Wybierz **zapory**. 
   
    ![Wybierz ustawienia > zapory][b31-SettingsFirewallNavig]
9. Wybierz **Dodawanie klienta IP**. W pierwszym polu tekstowym, wpisz nazwę nowej reguły.

10. Wpisz wartości adresu IP niski i wysoki dla zakresu, który ma zostać włączony.
    
    * Może być przydatny zakończeń niskiej wartości z **.0** i wysokiej wartości kończyć **.255**.
    
    ![Dodaj zakres adresów IP, aby umożliwić][b41-AddRange]
11. Wybierz pozycję **Zapisz**.

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
