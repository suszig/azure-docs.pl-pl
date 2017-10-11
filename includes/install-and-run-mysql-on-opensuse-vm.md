
1. Aby eskalować uprawnienia, wpisz:
   
        sudo -s
   
    Wprowadź hasło.
2. Aby zainstalować serwer MySQL Community edition, wpisz:
   
        zypper install mysql-community-server
   
    Zaczekaj, aż MySQL pobiera i instaluje.
3. Aby ustawić MySQL można uruchomić podczas rozruchu systemu, wpisz:
   
        insserv mysql
4. Ręcznie Uruchom demona MySQL (mysqld) za pomocą tego polecenia:
   
        rcmysql start
   
    Aby sprawdzić stan demona MySQL, wpisz:
   
        rcmysql status
   
    Aby zatrzymać demona MySQL, wpisz:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > Po zakończeniu instalacji MySQL hasła głównego jest domyślnie pusta. Zaleca się, że uruchamiasz **mysql\_bezpiecznego\_instalacji**, skrypt, który pomaga bezpiecznego MySQL. Skrypt wyświetli monit o zmianę hasła głównego MySQL, usunąć konta użytkownika anonimowego, wyłącz logowania zdalnego głównego, usunąć bazy danych testu i ponownie załadować tabelę uprawnień. Zaleca się odpowiedzieć, tak aby wszystkie te opcje, a następnie zmień hasło główne.
   > 
   > 
5. Wpisz, aby uruchomić skrypt MySQL skrypt instalacji:
   
        mysql_secure_installation
6. Zaloguj się do MySQL:
   
        mysql -u root -p
   
    Wprowadź hasło głównego MySQL (które można zmienić w poprzednim kroku) i zostanie wyświetlony wraz z monitem o których możesz wykonywać instrukcje SQL do interakcji z bazą danych.
7. Aby utworzyć nowy użytkownik MySQL, uruchom następujące polecenie w **mysql >** wiersza:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Uwaga: średnikami (;) na końcu wiersza, które są istotne dla zakończenia polecenia.
8. Aby utworzyć bazę danych i udzielić `mysqluser` uprawnienia użytkownika do niego, wydawać następujące polecenia:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Należy pamiętać, że bazy danych, nazwy użytkownika i hasła są używane tylko przez skrypty łączenia z bazą danych.  Nazwy kont użytkowników bazy danych nie przedstawiają niekoniecznie konta użytkowników w systemie.
9. Aby zalogować się na innym komputerze, wpisz:
   
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    gdzie `ip-address` to adres IP komputera, z którym połączy się MySQL.
10. Aby zamknąć narzędzie administracyjne bazy danych MySQL, wpisz:
    
        quit

## <a name="add-an-endpoint"></a>Dodawanie punktu końcowego
1. Po zainstalowaniu MySQL, musisz skonfigurować punkt końcowy do zdalnego dostępu MySQL. Zaloguj się do [klasycznego portalu Azure][AzurePortal]. Kliknij przycisk **maszyn wirtualnych**, kliknij nazwę nowej maszyny wirtualnej, a następnie kliknij przycisk **punkty końcowe**.
2. Kliknij przycisk **Dodaj** w dolnej części strony.
3. Dodawanie punktu końcowego o nazwie "MySQL" z protokołem **TCP**, i **publicznego** i **prywatnej** porty ustawioną wartość "3306".
4. Zdalne połączenia z maszyną wirtualną z komputera, wpisz:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Na przykład przy użyciu maszyny wykorzystanie wirtualnej utworzone w tym samouczku, wpisz następujące polecenie:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
