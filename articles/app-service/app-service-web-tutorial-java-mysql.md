---
title: Tworzenie aplikacji sieci web Java i MySQL na platformie Azure
description: "Dowiedz się, jak pobrać aplikację Java, która łączy się z usługą baza danych MySQL na platformie Azure pracy usługi aplikacji Azure."
services: app-service\web
documentationcenter: Java
author: bbenz
manager: jeffsand
editor: jasonwhowell
ms.assetid: 
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: tutorial
ms.date: 05/22/2017
ms.author: bbenz
ms.custom: mvc
ms.openlocfilehash: 8b8d7b026973de9dee6c834404f2ed80b2c9ad21
ms.sourcegitcommit: 4ac89872f4c86c612a71eb7ec30b755e7df89722
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/07/2017
---
# <a name="build-a-java-and-mysql-web-app-in-azure"></a>Tworzenie aplikacji sieci web Java i MySQL na platformie Azure

W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web Java na platformie Azure i podłącz go do bazy danych MySQL. Po zakończeniu, konieczne będzie [Spring rozruchu](https://projects.spring.io/spring-boot/) przechowywania danych w aplikacji [bazy danych Azure dla programu MySQL](https://docs.microsoft.com/azure/mysql/overview) systemem [aplikacji sieci Web usługi aplikacji Azure](app-service-web-overview.md).

![Aplikacja Java w usługi aplikacji Azure](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz bazę danych MySQL na platformie Azure
> * Łączenie aplikacji przykładowej bazy danych
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizowanie i ponowne wdrażanie aplikacji
> * Strumieniowe przesyłanie dzienników diagnostycznych z platformy Azure
> * Monitorowanie aplikacji w portalu Azure


## <a name="prerequisites"></a>Wymagania wstępne

1. [Pobierz i zainstaluj oprogramowanie Git](https://git-scm.com/)
1. [Pobierz i zainstaluj zestaw JDK Java 7 lub nowszy](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
1. [Pobierz, zainstaluj i uruchom MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Przygotowywanie lokalnej MySQL 

W tym kroku utworzysz bazę danych w lokalnym serwerze MySQL do użycia podczas testowania aplikacji lokalnie na komputerze.

### <a name="connect-to-mysql-server"></a>Połączyć się z serwerem MySQL

W oknie terminalu połączenie z serwerem lokalnym MySQL. To okno terminalu służy do uruchamiania wszystkich poleceń w tym samouczku.

```bash
mysql -u root -p
```

Jeśli zostanie wyświetlony monit o podanie hasła, wprowadź hasło dla `root` konta. Jeśli nie pamiętasz hasła do konta głównego, zobacz [MySQL: sposób resetowania hasła głównego](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Jeśli polecenie zostanie wykonane pomyślnie, na serwerze MySQL już jest uruchomiona. Jeśli nie, upewnij się, że lokalny serwer MySQL została uruchomiona, postępując [MySQL poinstalacyjne czynności](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

### <a name="create-a-database"></a>Tworzenie bazy danych 

W `mysql` należy utworzyć bazę danych i tabeli dla elementów do wykonania.

```sql
CREATE DATABASE tododb;
```

Zakończyć połączenie z serwerem, wpisując `quit`.

```sql
quit
```

## <a name="create-and-run-the-sample-app"></a>Tworzenie i uruchamianie przykładowej aplikacji 

W tym kroku sklonować przykładowej aplikacji rozruchu Spring, skonfigurować go do korzystania z lokalnej bazy danych MySQL i uruchom go na komputerze. 

### <a name="clone-the-sample"></a>Klonowanie próbki

W oknie terminalu przejdź do katalogu roboczego i klonowania repozytorium na próbkę. 

```bash
git clone https://github.com/azure-samples/mysql-spring-boot-todo
```

### <a name="configure-the-app-to-use-the-mysql-database"></a>Konfiguruje aplikację do używania bazy danych MySQL

Aktualizacja `spring.datasource.password` i wartość w *spring-boot-mysql-todo/src/main/resources/application.properties* z tego samego hasła głównego używana w celu otwarcia wiersza MySQL:

```
spring.datasource.password=mysqlpass
```

### <a name="build-and-run-the-sample"></a>Tworzenie i uruchamianie próbki

Tworzenie i uruchamianie przykładowych przy użyciu otoki Maven zawarte w repozytorium:

```bash
cd spring-boot-mysql-todo
mvnw package spring-boot:run
```

Otwórz przeglądarkę, aby `http://localhost:8080` w próbce w akcji. Podczas dodawania zadania do listy, użyj następujących poleceń SQL w wierszu polecenia programu MySQL, aby wyświetlić dane przechowywane w MySQL.

```SQL
use testdb;
select * from todo_item;
```

Zatrzymaj aplikację za pomocą `Ctrl` + `C` w terminalu. 

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-an-azure-mysql-database"></a>Utwórz bazę danych MySQL na platformie Azure

W tym kroku utworzysz [bazy danych Azure dla programu MySQL](../mysql/quickstart-create-mysql-server-database-using-azure-cli.md) wystąpienia przy użyciu [interfejsu wiersza polecenia Azure](https://docs.microsoft.com/cli/azure/install-azure-cli). Możesz skonfigurować przykładowej aplikacji do tej bazy danych później w samouczku.

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz [grupy zasobów](../azure-resource-manager/resource-group-overview.md) z [Tworzenie grupy az](/cli/azure/group#create) polecenia. Grupy zasobów platformy Azure jest kontenerem logicznym, w której wdrożone i zarządzane zasoby pokrewne, takie jak aplikacje sieci web, baz danych i konta magazynu. 

Poniższy przykład tworzy grupę zasobów w regionie, Europa Północna, Europa:

```azurecli-interactive
az group create --name myResourceGroup --location "North Europe"
```    

Aby sprawdzić możliwe wartości można użyć dla `--location`, użyj [appservice az listy lokalizacje](/cli/azure/appservice#list-locations) polecenia.

### <a name="create-a-mysql-server"></a>Utwórz serwer MySQL

W powłoce chmury Tworzenie serwera w bazie danych Azure dla programu MySQL (wersja zapoznawcza) przy użyciu [utworzenie przez serwer mysql az](/cli/azure/mysql/server#create) polecenia.    
Podstawić własną unikatową nazwę serwera MySQL, której występuje `<mysql_server_name>` symbolu zastępczego. Ta nazwa jest częścią nazwy hosta serwera MySQL, `<mysql_server_name>.mysql.database.azure.com`, a więc musi być globalnie unikatowe. Zamienić `<admin_user>` i `<admin_password>` z własne wartości.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user <admin_user> --admin-password <admin_password>
```

Po utworzeniu serwer MySQL, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu:

```json
{
  "administratorLogin": "admin_user",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mysql_server_name.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/mysql_server_name",
  "location": "northeurope",
  "name": "mysql_server_name",
  "resourceGroup": "mysqlJavaResourceGroup",
  ...
  < Output has been truncated for readability >
}
```

### <a name="configure-server-firewall"></a>Konfigurowanie zapory serwera

W powłoce chmury Tworzenie reguły zapory dla serwera MySQL zezwolić na połączenia klientów przy użyciu [az mysql reguły zapory serwera — Utwórz](/cli/azure/mysql/server/firewall-rule#create) polecenia. 

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Bazy danych platformy Azure dla programu MySQL (wersja zapoznawcza) nie obsługuje obecnie automatycznie połączenia z usługami Azure. Jak adresy IP na platformie Azure są przypisywane dynamicznie, lepiej jest teraz włączyć wszystkie adresy IP. Jak usługa będzie jej podgląd, lepiej metody zabezpieczania bazy danych zostanie włączona.

## <a name="configure-the-azure-mysql-database"></a>Skonfiguruj bazę danych MySQL na platformie Azure

W oknie terminalu lokalnego nawiązać serwer MySQL na platformie Azure. Użyj wartości określonej wcześniej dla `<admin_user>` i `<mysql_server_name>`.

```bash
mysql -u <admin_user>@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

### <a name="create-a-database"></a>Tworzenie bazy danych 

W `mysql` należy utworzyć bazę danych i tabeli dla elementów do wykonania.

```sql
CREATE DATABASE tododb;
```

### <a name="create-a-user-with-permissions"></a>Utwórz użytkownika z uprawnieniami

Tworzenie użytkownika bazy danych i nadaj mu wszystkie uprawnienia `tododb` bazy danych. Zastąp symbole zastępcze `<Javaapp_user>` i `<Javaapp_password>` z własnych unikatowej nazwy aplikacji.

```sql
CREATE USER '<Javaapp_user>' IDENTIFIED BY '<Javaapp_password>'; 
GRANT ALL PRIVILEGES ON tododb.* TO '<Javaapp_user>';
```

Zakończyć połączenie z serwerem, wpisując `quit`.

```sql
quit
```

## <a name="deploy-the-sample-to-azure-app-service"></a>Wdrażanie przykładowej w usłudze Azure App Service

Tworzenie planu usługi aplikacji Azure z **wolne** ceny za pomocą warstwy [Tworzenie planu usług aplikacji az](/cli/azure/appservice/plan#create) polecenia interfejsu wiersza polecenia. Planu usług aplikacji definiuje zasoby fizyczne, używana do hostowania aplikacji. Wszystkie aplikacje przypisane do planu usług aplikacji udostępniania tych zasobów, co umożliwia zapisywanie koszt odnośnie do hostowania wielu aplikacji. 

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE
```

Gdy plan jest gotowy, interfejsu wiersza polecenia Azure zawiera podobne dane wyjściowe do poniższego przykładu:

```json
{ 
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "North Europe",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "app",
  "location": "North Europe",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  ...
  < Output has been truncated for readability >
} 
``` 

### <a name="create-an-azure-web-app"></a>Tworzenie aplikacji sieci Web platformy Azure

 W powłoce chmury za pomocą [tworzenie aplikacji sieci Web az](/cli/azure/appservice/web#create) polecenia interfejsu wiersza polecenia do tworzenia definicji aplikacji sieci web w `myAppServicePlan` planu usługi aplikacji. Definicja aplikacji sieci web zawiera adres URL w celu uzyskania dostępu do aplikacji z i konfiguruje kilka sposobów wdrażania kodu na platformie Azure. 

```azurecli-interactive
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

SUBSTITUTE `<app_name>` symbol zastępczy własne unikatowej nazwy aplikacji. Ta nazwa jest częścią domyślna nazwa domeny dla aplikacji sieci web, nazwa musi być unikatowy w obrębie wszystkich aplikacji w usłudze Azure. Przed udostępnieniem użytkownikom, możesz mapować wpis nazwy domeny niestandardowej aplikacji sieci web.

Podczas definiowania aplikacji sieci web jest gotowy, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu: 

```json 
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "enabled": true,
   ...
  < Output has been truncated for readability >
}
```

### <a name="configure-java"></a>Konfigurowanie języka Java 

W powłoce chmury Ustawianie konfiguracji środowiska uruchomieniowego języka Java, wymagającym aplikacji z [aktualizacja konfiguracji sieci web appservice az](/cli/azure/appservice/web/config#update) polecenia.

Następujące polecenie konfiguruje aplikacji sieci web do uruchamiania na ostatnie JDK 8 Java i [Apache Tomcat](http://tomcat.apache.org/) 8.0.

```azurecli-interactive
az webapp config set --name <app_name> --resource-group myResourceGroup --java-version 1.8 --java-container Tomcat --java-container-version 8.0
```

### <a name="configure-the-app-to-use-the-azure-sql-database"></a>Konfiguruje aplikację do używania bazy danych Azure SQL

Przed uruchomieniem Przykładowa aplikacja, ustawienia aplikacji dotyczące aplikacji sieci web do używania bazy danych MySQL na platformie Azure, utworzona na platformie Azure. Te właściwości są widoczne dla aplikacji sieci web jako zmienne środowiskowe i Zastąp wartości application.properties wewnątrz aplikacji sieci web spakowanych. 

W powłoce chmury ustawienia aplikacji przy użyciu [appsettings konfiguracji aplikacji sieci Web az](https://docs.microsoft.com/cli/azure/appservice/web/config/appsettings) w interfejsu wiersza polecenia:

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_URL="jdbc:mysql://<mysql_server_name>.mysql.database.azure.com:3306/tododb?verifyServerCertificate=true&useSSL=true&requireSSL=false" --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_USERNAME=Javaapp_user@mysql_server_name --resource-group myResourceGroup --name <app_name>
```

```azurecli-interactive
az webapp config appsettings set --settings SPRING_DATASOURCE_PASSWORD=Javaapp_password --resource-group myResourceGroup --name <app_name>
```

### <a name="get-ftp-deployment-credentials"></a>Uzyskiwanie poświadczeń wdrożenia FTP 
Można wdrożyć aplikację na Azure appservice na różne sposoby, w tym FTP, lokalne Git GitHub, Visual Studio Team Services i BitBucket. Na przykład FTP do wdrożenia. Plik WAR wcześniej utworzony na komputerze lokalnym w usłudze Azure App Service.

Aby określić, jakie poświadczenia przekazać polecenia ftp do aplikacji sieci Web, użyj [az usługi aplikacji sieci web wdrożenia listy publikowania — profile](https://docs.microsoft.com/cli/azure/appservice/web/deployment#az_appservice_web_deployment_list_publishing_profiles) polecenie w powłoce chmury: 

```azurecli-interactive
az webapp deployment list-publishing-profiles --name <app_name> --resource-group myResourceGroup --query "[?publishMethod=='FTP'].{URL:publishUrl, Username:userName,Password:userPWD}" --output json
```

```JSON
[
  {
    "Password": "aBcDeFgHiJkLmNoPqRsTuVwXyZ",
    "URL": "ftp://waws-prod-blu-069.ftp.azurewebsites.windows.net/site/wwwroot",
    "Username": "app_name\\$app_name"
  }
]
```

### <a name="upload-the-app-using-ftp"></a>Przekaż aplikację za pomocą protokołu FTP

Aby wdrożyć za pomocą ulubionego narzędzia FTP. Plik WAR do */site/wwwroot/webapps* folderu na pobrane z adresu serwera `URL` w poprzednie polecenie. Usuń istniejący katalog domyślny (ROOT) w aplikacji i Zastąp istniejące ROOT.war z. Plik WAR wbudowane we wcześniejszej części tego samouczka.

```bash
ftp waws-prod-blu-069.ftp.azurewebsites.windows.net
Connected to waws-prod-blu-069.drip.azurewebsites.windows.net.
220 Microsoft FTP Service
Name (waws-prod-blu-069.ftp.azurewebsites.windows.net:raisa): app_name\$app_name
331 Password required
Password:
cd /site/wwwroot/webapps
mdelete -i ROOT/*
rmdir ROOT/
put target/TodoDemo-0.0.1-SNAPSHOT.war ROOT.war
```

### <a name="test-the-web-app"></a>Testowanie aplikacji sieci web

Przejdź do `http://<app_name>.azurewebsites.net/` i dodaj kilka zadań do listy. 

![Aplikacja Java w usługi aplikacji Azure](./media/app-service-web-tutorial-java-mysql/appservice-web-app.png)

**Gratulacje!** Używasz aplikacji Java opartych na danych w usłudze Azure App Service.

## <a name="update-the-app-and-redeploy"></a>Aktualizowanie aplikacji i ponowne wdrażanie

Aktualizuj aplikację do uwzględnienia dodatkowa kolumna na liście todo dzień, jakie została utworzona. Spring rozruchu obsługuje aktualizowanie schematu bazy danych dla Ciebie jako zmiany modelu danych bez zmieniania istniejących rekordy bazy danych.

1. W systemie lokalnym otwarcie *src/main/java/com/example/fabrikam/TodoItem.java* i dodaj następujące instrukcje importu do klasy:   

    ```java
    import java.text.SimpleDateFormat;
    import java.util.Calendar;
    ```

2. Dodaj `String` właściwości `timeCreated` do *src/main/java/com/example/fabrikam/TodoItem.java*, inicjowanie go z sygnaturą czasową podczas tworzenia obiektu. Dodaj metody pobierające/ustawiające właściwości dla nowego `timeCreated` właściwości podczas edytowania tego pliku.

    ```java
    private String name;
    private boolean complete;
    private String timeCreated;
    ...

    public TodoItem(String category, String name) {
       this.category = category;
       this.name = name;
       this.complete = false;
       this.timeCreated = new SimpleDateFormat("MMMM dd, YYYY").format(Calendar.getInstance().getTime());
    }
    ...
    public void setTimeCreated(String timeCreated) {
       this.timeCreated = timeCreated;
    }

    public String getTimeCreated() {
        return timeCreated;
    }
    ```

3. Aktualizacja *src/main/java/com/example/fabrikam/TodoDemoController.java* o wiersz w `updateTodo` metodę, aby ustawić sygnatura czasowa:

    ```java
    item.setComplete(requestItem.isComplete());
    item.setId(requestItem.getId());
    item.setTimeCreated(requestItem.getTimeCreated());
    repository.save(item);
    ```

4. Dodaj obsługę nowego pola w szablonie Thymeleaf. Aktualizacja *src/main/resources/templates/index.html* z użyciem nowego nagłówka tabeli dla znacznik czasu oraz nowe pole do wyświetlenia wartości znacznik czasu w każdym wierszu danych tabeli.

    ```html
    <th>Name</th>
    <th>Category</th>
    <th>Time Created</th>
    <th>Complete</th>
    ...
    <td th:text="${item.category}">item_category</td><input type="hidden" th:field="*{todoList[__${i.index}__].category}"/>
    <td th:text="${item.timeCreated}">item_time_created</td><input type="hidden" th:field="*{todoList[__${i.index}__].timeCreated}"/>
    <td><input type="checkbox" th:checked="${item.complete} == true" th:field="*{todoList[__${i.index}__].complete}"/></td>
    ```

5. Odbuduj aplikacji:

    ```bash
    mvnw clean package 
    ```

6. FTP zaktualizowane. WAR jako przed, usunięcie istniejących */wwwroot/webapps/katalogu głównego witryny* katalogu i *ROOT.war*, a następnie przekazywanie zaktualizowanego. Plik WAR jako ROOT.war. 

Podczas odświeżania aplikacji, **tworzony** kolumny jest teraz widoczne. Podczas dodawania nowego zadania, aplikacja zostanie automatycznie wypełnić sygnatura czasowa. Istniejących zadań pozostają bez zmian i Praca z aplikacją, nawet jeśli odpowiedni model danych został zmieniony. 

![Zaktualizowane o nową kolumnę aplikacji Java](./media/app-service-web-tutorial-java-mysql/appservice-updates-java.png)
      
## <a name="stream-diagnostic-logs"></a>Dzienniki diagnostyczne strumienia 

Podczas wykonywania aplikacji Java w usłudze Azure App Service można uzyskać dzienniki konsoli potoku bezpośrednio do terminalu. W ten sposób można uzyskać tego samego komunikaty diagnostyczne w celu ułatwienia debugowania błędów aplikacji.

Aby rozpocząć przesyłanie strumieniowe dziennika, użyj [końcowego fragmentu dziennika aplikacji sieci Web az](/cli/azure/webapp/log?view=azure-cli-latest#az_webapp_log_tail) polecenie w powłoce chmury.

```azurecli-interactive 
az webapp log tail --name <app_name> --resource-group myResourceGroup 
``` 

## <a name="manage-your-azure-web-app"></a>Zarządzanie aplikacji sieci web platformy Azure

Przejdź do portalu Azure, aby zobaczyć utworzonej aplikacji sieci web.

W tym celu zaloguj się do witryny [https://portal.azure.com](https://portal.azure.com).

W lewym menu kliknij pozycję **App Service**, a następnie kliknij nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-tutorial-java-mysql/access-portal.png)

Domyślnie blok aplikacji sieci Web wyświetla stronę **Przegląd**. Ta strona udostępnia widok sposobu działania aplikacji. W tym miejscu mogą też wykonywać zadania zarządzania, takie jak zatrzymanie, start, ponowne uruchomienie i usuwania. Na kartach po lewej stronie bloku są pokazane poszczególne strony konfiguracji, które można otworzyć.

![Blok usługi App Service w witrynie Azure Portal](./media/app-service-web-tutorial-java-mysql/web-app-blade.png)

Te karty w bloku pokazują wiele doskonałych funkcji, które możesz dodać do aplikacji sieci Web. Poniższa lista zawiera tylko kilka możliwości:
* Mapowanie niestandardowej nazwy DNS
* Tworzenie powiązania niestandardowego certyfikatu SSL
* Konfigurowanie ciągłego wdrażania
* Skalowanie w górę i w dół
* Dodawanie uwierzytelniania użytkownika

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli nie potrzebujesz tych zasobów innym samouczek (zobacz [następne kroki](#next)), można je usunąć, uruchamiając następujące polecenie w powłoce chmury: 
  
```azurecli-interactive
az group delete --name myResourceGroup 
``` 

<a name="next"></a>

## <a name="next-steps"></a>Następne kroki

> [!div class="checklist"]
> * Utwórz bazę danych MySQL na platformie Azure
> * Łączenie aplikacji Java próbki MySQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizowanie i ponowne wdrażanie aplikacji
> * Strumieniowe przesyłanie dzienników diagnostycznych z platformy Azure
> * Zarządzanie aplikacją w portalu Azure

Przejście do następnym samouczku, aby dowiedzieć się, jak zamapować niestandardową nazwę DNS w aplikacji.

> [!div class="nextstepaction"] 
> [Map an existing custom DNS name to Azure Web Apps (Mapowanie istniejącej niestandardowej nazwy DNS na aplikacje internetowe platformy Azure)](app-service-web-tutorial-custom-domain.md)
