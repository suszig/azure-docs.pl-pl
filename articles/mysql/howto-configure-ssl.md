---
title: "Konfiguracja połączenia SSL bezpiecznie łączyć się z bazą danych Azure dla programu MySQL | Dokumentacja firmy Microsoft"
description: "Instrukcje poprawnie skonfigurować bazy danych Azure MySQL i skojarzonych aplikacji, aby poprawnie używać połączeń SSL"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 10/25/2017
ms.openlocfilehash: 83830e4776eaa7c4f10bc14dcefd47c6eaf25997
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/26/2017
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Konfiguracja połączenia SSL w aplikacji w celu bezpiecznego łączenia z bazą danych Azure dla programu MySQL
Bazy danych platformy Azure dla programu MySQL obsługuje połączenie bazy danych Azure, aby serwer MySQL aplikacji klienta przy użyciu protokołu Secure Sockets Layer (SSL). Wymuszanie połączenia SSL między serwerem bazy danych i aplikacji klienckich pomaga chronić przed atakami "man w środku" szyfrując strumienia danych między serwerem i aplikacji.

## <a name="step-1-obtain-ssl-certificate"></a>Krok 1: Uzyskiwanie certyfikatu SSL
Pobierz certyfikat potrzebne do komunikowania się za pośrednictwem protokołu SSL z bazy danych Azure, aby serwer MySQL z [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) i Zapisz plik certyfikatu na dysk lokalny (z Ten samouczek użyliśmy c:\ssl).
**Programu Microsoft Internet Explorer oraz Microsoft Edge:** po zakończeniu pobierania, Zmień nazwę certyfikatu na BaltimoreCyberTrustRoot.crt.pem.

## <a name="step-2-bind-ssl"></a>Krok 2: Powiązanie SSL
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Łączenie z serwerem przy użyciu narzędzia MySQL Workbench za pośrednictwem protokołu SSL
Konfigurowanie środowiska roboczego MySQL, aby bezpieczne łączenie się za pośrednictwem protokołu SSL. Z dialogu instalacji nowego połączenia, przejdź do **SSL** kartę. W **pliku urząd certyfikacji SSL:** wprowadź lokalizację pliku **BaltimoreCyberTrustRoot.crt.pem**. 
![Zapisz niestandardowe kafelka](./media/howto-configure-ssl/mysql-workbench-ssl.png) dla istniejących połączeń, powiązania SSL, klikając prawym przyciskiem myszy ikonę połączenie i wybierz polecenie Edycja. Następnie przejdź do **SSL** karcie i powiąż pliku certyfikatu.

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Łączenie z serwerem przy użyciu interfejsu wiersza polecenia MySQL za pośrednictwem protokołu SSL
Innym sposobem powiązać certyfikatu SSL jest przy użyciu interfejsu wiersza polecenia MySQL przez wykonanie następującego polecenia:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>Krok 3: Wymuszanie połączeń SSL na platformie Azure 
### <a name="using-the-azure-portal"></a>Korzystanie z witryny Azure Portal
Przy użyciu portalu Azure, odwiedź bazy danych Azure, aby serwer MySQL, a następnie kliknij przycisk **zabezpieczenia połączeń**. Użyj przycisku przełączania, aby włączyć lub wyłączyć **połączenia SSL wymusić** ustawienia, a następnie kliknij przycisk **zapisać**. Firma Microsoft zaleca, aby włączyć zawsze **połączenia SSL wymusić** ustawienie w celu zwiększenia bezpieczeństwa.
![Włącz ssl](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Korzystanie z interfejsu wiersza polecenia platformy Azure
Można włączyć lub wyłączyć **wymuszania ssl** parametr przy użyciu wartości włączone lub wyłączone odpowiednio w wiersza polecenia platformy Azure.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-the-ssl-connection"></a>Krok 4: Sprawdź połączenia SSL
Wykonanie mysql **stan** polecenie, aby zweryfikować, że nawiązano połączenie z serwerem MySQL przy użyciu protokołu SSL:
```dos
mysql> status
```
Upewnij się, połączenie jest szyfrowane, przeglądając dane wyjściowe, które powinny być widoczne: **SSL: szyfrowania używany jest algorytm SHA AES256** 

## <a name="sample-code"></a>Przykładowy kod
Można ustanowić bezpiecznego połączenia do bazy danych Azure dla programu MySQL za pośrednictwem protokołu SSL z aplikacji, można znaleźć w następujących przykładach kodu.
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306, MYSQLI_CLIENT_SSL, MYSQLI_CLIENT_SSL_DONT_VERIFY_SERVER_CERT);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python-mysqlconnector-python"></a>Python (MySQLConnector Python)
```
try:
    conn=mysql.connector.connect(user='myadmin@myserver4demo', 
        password='yourpassword', 
        database='quickstartdb', 
        host='myserver4demo.mysql.database.azure.com', 
        ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
    print(err)
```
### <a name="python-pymysql"></a>Python (PyMySQL)
```
conn = pymysql.connect(user = 'myadmin@myserver4demo', 
        password = 'yourpassword', 
        database = 'quickstartdb', 
        host = 'myserver4demo.mysql.database.azure.com', 
        ssl = {'ssl': {'ca': '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'}})
```
### <a name="ruby"></a>Ruby
```
client = Mysql2::Client.new(
        :host     => 'myserver4demo.mysql.database.azure.com', 
        :username => 'myadmin@myserver4demo',      
        :password => 'yourpassword',    
        :database => 'quickstartdb',
        :ssl_ca => '/var/www/html/BaltimoreCyberTrustRoot.crt.pem'
    )
```
### <a name="golang"></a>Golang
```
rootCertPool := x509.NewCertPool()
pem, _ := ioutil.ReadFile("/var/www/html/BaltimoreCyberTrustRoot.crt.pem")
if ok := rootCertPool.AppendCertsFromPEM(pem); !ok {
    log.Fatal("Failed to append PEM.")
}
mysql.RegisterTLSConfig("custom", &tls.Config{RootCAs: rootCertPool, InsecureSkipVerify: true})
var connectionString string
connectionString = fmt.Sprintf("%s:%s@tcp(%s:3306)/%s?allowNativePasswords=true&tls=custom",'myadmin@myserver4demo' , 'yourpassword', 'myserver4demo.mysql.database.azure.com', 'quickstartdb') 
db, _ := sql.Open("mysql", connectionString)
```
### <a name="javajdbc"></a>JAVA(JDBC)
```
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mysql://%s/%s?serverTimezone=UTC&useSSL=true", 'myserver4demo.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@myserver4demo');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```
### <a name="javamariadb"></a>Java(MariaDB)
```
# generate truststore and keystore in code
String importCert = " -import "+
    " -alias mysqlServerCACert "+
    " -file " + ssl_ca +
    " -keystore truststore "+
    " -trustcacerts " + 
    " -storepass password -noprompt ";
String genKey = " -genkey -keyalg rsa " +
    " -alias mysqlClientCertificate -keystore keystore " +
    " -storepass password123 -keypass password " + 
    " -dname CN=MS ";
sun.security.tools.keytool.Main.main(importCert.trim().split("\\s+"));
sun.security.tools.keytool.Main.main(genKey.trim().split("\\s+"));

# use the generated keystore and truststore 
System.setProperty("javax.net.ssl.keyStore","path_to_keystore_file");
System.setProperty("javax.net.ssl.keyStorePassword","password");
System.setProperty("javax.net.ssl.trustStore","path_to_truststore_file");
System.setProperty("javax.net.ssl.trustStorePassword","password");

url = String.format("jdbc:mariadb://%s/%s?useSSL=true&trustServerCertificate=true", 'myserver4demo.mysql.database.azure.com', 'quickstartdb');
properties.setProperty("user", 'myadmin@myserver4demo');
properties.setProperty("password", 'yourpassword');
conn = DriverManager.getConnection(url, properties);
```

## <a name="next-steps"></a>Następne kroki
Przegląd różnych opcji łączności aplikacji po [biblioteki połączeń dla bazy danych Azure dla programu MySQL](concepts-connection-libraries.md)
