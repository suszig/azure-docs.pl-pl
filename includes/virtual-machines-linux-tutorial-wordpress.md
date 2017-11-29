## <a name="install-wordpress"></a>Zainstaluj program WordPress

Jeśli chcesz spróbować stosu, należy zainstalować przykładową aplikację. Na przykład następujące kroki instalowania typu open source [WordPress](https://wordpress.org/) platformę do tworzenia witryn sieci Web i blogów. Inne obciążenia, aby spróbować zawierają [Drupal](http://www.drupal.org) i [Moodle](https://moodle.org/). 

Ten Instalator WordPress jest tylko do weryfikacji koncepcji. Aby zainstalować najnowsze WordPress w środowisku produkcyjnym z zalecanych ustawień zabezpieczeń, zobacz [dokumentacji WordPress](https://codex.wordpress.org/Main_Page). 



### <a name="install-the-wordpress-package"></a>Zainstaluj pakiet WordPress

Uruchom następujące polecenie:

```bash
sudo apt install wordpress
```

### <a name="configure-wordpress"></a>Konfigurowanie usługi WordPress

Konfigurowanie usługi WordPress, aby użyć MySQL i PHP.

W katalogu roboczym, Utwórz plik tekstowy `wordpress.sql` skonfigurować bazy danych MySQL platformy WordPress: 

```bash
sudo sensible-editor wordpress.sql
```

Dodaj następujące polecenia, zastępując hasła bazy danych wybrane do *yourPassword* (Pozostaw bez zmian wartości). Jeśli wcześniej tak skonfigurować zasadę zabezpieczeń MySQL, do sprawdzania siły hasła, upewnij się, że hasło spełnia wymagania dotyczące siły. Zapisz plik.

```sql
CREATE DATABASE wordpress;
GRANT SELECT,INSERT,UPDATE,DELETE,CREATE,DROP,ALTER
ON wordpress.*
TO wordpress@localhost
IDENTIFIED BY 'yourPassword';
FLUSH PRIVILEGES;
```

Uruchom następujące polecenie, aby utworzyć bazę danych:

```bash
cat wordpress.sql | sudo mysql --defaults-extra-file=/etc/mysql/debian.cnf
```

Ponieważ plik `wordpress.sql` zawiera poświadczenia bazy danych, Usuń, po użyciu:

```bash
sudo rm wordpress.sql
```

Aby skonfigurować PHP, uruchom następujące polecenie, aby otworzyć wybranym w edytorze tekstu i utworzyć plik `/etc/wordpress/config-localhost.php`:

```bash
sudo sensible-editor /etc/wordpress/config-localhost.php
```
Skopiuj następujące wiersze do pliku, zastępując hasła WordPress bazy danych dla *yourPassword* (Pozostaw bez zmian wartości). Następnie zapisz plik.

```php
<?php
define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpress');
define('DB_PASSWORD', 'yourPassword');
define('DB_HOST', 'localhost');
define('WP_CONTENT_DIR', '/usr/share/wordpress/wp-content');
?>
```


Przenoszenie instalacji WordPress głównego dokumentu serwera sieci web:

```bash
sudo ln -s /usr/share/wordpress /var/www/html/wordpress

sudo mv /etc/wordpress/config-localhost.php /etc/wordpress/config-default.php
```

Można teraz zakończyć WordPress i publikowanie na platformie. Otwórz przeglądarkę i przejdź do `http://yourPublicIPAddress/wordpress`. Należy zastąpić publicznego adresu IP maszyny Wirtualnej. Powinien być podobny do tego obrazu.

![Strona instalacji WordPress](./media/virtual-machines-linux-tutorial-wordpress/wordpressstartpage.png)