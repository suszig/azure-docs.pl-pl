---
title: "Tworzenie aplikacji sieci web Ruby i MySQL w usłudze Azure App Service w systemie Linux | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak pobrać aplikację dopisków fonetycznych, praca na platformie Azure z połączenia z bazą danych MySQL na platformie Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: cfowler
ms.service: app-service-web
ms.workload: web
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 12/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 03b2b4e8f8827a08e1414512d848bd5bed48d674
ms.sourcegitcommit: df4ddc55b42b593f165d56531f591fdb1e689686
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/04/2018
---
# <a name="build-a-ruby-and-mysql-web-app-in-azure-app-service-on-linux"></a>Tworzenie aplikacji sieci web Ruby i MySQL w usłudze Azure App Service w systemie Linux

Usługa [App Service w systemie Linux](app-service-linux-intro.md) oferuje wysoce skalowalną i samonaprawialną usługę hostingu w Internecie przy użyciu systemu operacyjnego Linux. W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web dopisków fonetycznych i podłącz go do bazy danych MySQL. Po zakończeniu będziesz mieć [dopisków fonetycznych na szyny](http://rubyonrails.org/) aplikacji uruchomionej w usłudze aplikacji w systemie Linux.

![Ruby na szyny uruchomieniu aplikacji w usłudze Azure App Service](./media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz bazę danych MySQL na platformie Azure
> * Nawiązać Ruby w aplikacji szyny MySQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników diagnostycznych z platformy Azure
> * Zarządzanie aplikacją w portalu Azure

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* [Zainstaluj oprogramowanie Git](https://git-scm.com/)
* [Zainstaluj Ruby 2.3](https://www.ruby-lang.org/documentation/installation/)
* [Zainstaluj Ruby na szyny 5.1](http://guides.rubyonrails.org/v5.1/getting_started.html)
* [Zainstaluj i uruchom MySQL](https://dev.mysql.com/doc/refman/5.7/en/installing.html) 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prepare-local-mysql"></a>Przygotowywanie lokalnej MySQL

W tym kroku utworzysz bazę danych na lokalnym serwerze MySQL do użycia w tym samouczku.

### <a name="connect-to-local-mysql-server"></a>Połączyć się z lokalnym serwerem MySQL

W oknie terminalu połączenie z serwerem lokalnym MySQL. To okno terminalu służy do uruchamiania wszystkich poleceń w tym samouczku.

```bash
mysql -u root -p
```

Jeśli zostanie wyświetlony monit o podanie hasła, wprowadź hasło dla `root` konta. Jeśli nie pamiętasz hasła do konta głównego, zobacz [MySQL: sposób resetowania hasła głównego](https://dev.mysql.com/doc/refman/5.7/en/resetting-permissions.html).

Jeśli polecenie zostanie wykonane pomyślnie, jest uruchomiony serwer MySQL. Jeśli nie, upewnij się, że lokalny serwer MySQL została uruchomiona, postępując [MySQL poinstalacyjne czynności](https://dev.mysql.com/doc/refman/5.7/en/postinstallation.html).

Zakończyć połączenie z serwerem, wpisując `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-ruby-on-rails-app-locally"></a>Utwórz Ruby na szyny aplikacji lokalnie
W tym kroku uzyskać Ruby szyny przykładowej aplikacji, skonfiguruj połączenie bazy danych i uruchom lokalnie. 

### <a name="clone-the-sample"></a>Klonowanie próbki

W oknie terminalu `cd` do katalogu roboczego.

Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium.

```bash
git clone https://github.com/Azure-Samples/rubyrails-tasks.git
```

`cd`sklonowany katalogu. Zainstaluj wymagane pakiety.

```bash
cd rubyrails-tasks
bundle install --path vendor/bundle
```

### <a name="configure-mysql-connection"></a>Skonfiguruj połączenie MySQL

W repozytorium, otwórz _config/database.yml_ i podaj nazwę użytkownika w lokalnym katalogu głównego MySQL i hasło (wiersz 13). Jeśli zainstalowano MySQL przy użyciu narzędzia, takiego jak [Homebrew](https://brew.sh/), poświadczenia w pliku są już ustawione na wartości domyślne, a następnie (czyli `root` i pustego hasła).

```txt
default: &default
  adapter: mysql2
  encoding: utf8
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password:
  socket: /tmp/mysql.sock
```

### <a name="run-the-sample-locally"></a>Uruchom lokalnie próbki

Uruchom [migracje szyny](http://guides.rubyonrails.org/active_record_migrations.html#running-migrations) do tworzenia tabel aplikacja potrzebuje. Aby zobaczyć, które tabele są tworzone w migracja, Szukaj w _db/migracji_ katalogu w repozytorium Git.

```bash
rake db:create
rake db:migrate
```

Uruchom aplikację.

```bash
rails server
```

W przeglądarce przejdź do adresu `http://localhost:3000`. Dodaj kilka zadań na stronie.

![Ruby na szyny pomyślnie łączy się z MySQL](./media/tutorial-ruby-mysql-app/mysql-connect-success.png)

Aby zatrzymać serwer szyny, wpisz `Ctrl + C` w terminalu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Tworzenie MySQL na platformie Azure

W tym kroku utworzysz bazę danych MySQL w [bazy danych Azure dla programu MySQL (wersja zapoznawcza)](/azure/mysql). Następnie należy skonfigurować Ruby w aplikacji szyny nawiązać połączenia z tej bazy danych.

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Utwórz serwer MySQL

Tworzenie serwera w bazie danych Azure dla programu MySQL (wersja zapoznawcza) z [utworzenie przez serwer mysql az](/cli/azure/mysql/server?view=azure-cli-latest#az_mysql_server_create) polecenia.

W poniższym poleceniu zastąp nazwę serwera MySQL, w której występuje  _&lt;mysql_server_name >_ symbolu zastępczego (prawidłowe znaki to `a-z`, `0-9`, i `-`). Ta nazwa jest częścią nazwy hosta serwera MySQL (`<mysql_server_name>.mysql.database.azure.com`), musi on być globalnie unikatowe.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password My5up3r$tr0ngPa$w0rd!
```

Po utworzeniu serwer MySQL, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurowanie zapory serwera

Tworzenie reguły zapory dla serwera MySQL zezwolić na połączenia klientów przy użyciu [az mysql reguły zapory serwera — Utwórz](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az_mysql_server_firewall_rule_create) polecenia.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Bazy danych platformy Azure dla programu MySQL (wersja zapoznawcza) aktualnie nie ograniczyć połączenia tylko do usług platformy Azure. Jak adresy IP na platformie Azure są przypisywane dynamicznie, lepiej jest umożliwienie wszystkich adresów IP. Usługa jest w wersji zapoznawczej. Zaplanowano lepsze metody zabezpieczania bazy danych.
>

### <a name="connect-to-production-mysql-server-locally"></a>Połączyć się z produkcyjnym serwerem MySQL lokalnie

W oknie terminalu nawiązać serwer MySQL na platformie Azure. Użyj wartości określonej wcześniej dla  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.mysql.database.azure.com -P 3306 -p
```

Po wyświetleniu monitu o podanie hasła, użyj _My5up3r$ tr0ngPa$ w0rd!_, która została określona podczas tworzenia serwera bazy danych.

### <a name="create-a-production-database"></a>Utwórz bazę danych produkcyjnych

W `mysql` należy utworzyć bazę danych.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Utwórz użytkownika z uprawnieniami

Utwórz użytkownika bazy danych o nazwie _railsappuser_ i nadaj mu wszystkie uprawnienia `sampledb` bazy danych.

```sql
CREATE USER 'railsappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'railsappuser';
```

Zakończyć połączenie z serwerem, wpisując `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Łączenie aplikacji z platformy Azure MySQL

W tym kroku łączysz Ruby szyny aplikacji do bazy danych MySQL utworzonej w bazie danych Azure dla programu MySQL (wersja zapoznawcza).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Skonfiguruj połączenie bazy danych

W repozytorium, otwórz _config/database.yml_. W dolnej części pliku Zastąp zmienne produkcyjnych z następującym kodem. Aby uzyskać  _&lt;mysql_server_name >_ symbolu zastępczego, użyj nazwy utworzonego serwera MySQL.

```txt
production:
  <<: *default
  host: <%= ENV['DB_HOST'] %>
  port: 3306
  database: <%= ENV['DB_DATABASE'] %>
  username: <%= ENV['DB_USERNAME'] %>
  password: <%= ENV['DB_PASSWORD'] %>
  sslca: ssl/BaltimoreCyberTrustRoot.crt.pem
```

Zapisz zmiany.

> [!NOTE]
> `sslca` Zostanie dodany i punktów do istniejącej _PEM_ plik w repozytorium przykładowej. Domyślnie baza danych Azure dla programu MySQL wymusza połączenia SSL od klientów. To `.pem` certyfikat jest jak nawiązywać połączenia SSL do bazy danych Azure dla programu MySQL. Aby uzyskać więcej informacji, zobacz [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL (Konfigurowanie łączności SSL w aplikacji w celu bezpiecznego nawiązywania połączeń z usługą Azure Database for MySQL)](../../mysql/howto-configure-ssl.md).
>

### <a name="test-the-application-locally"></a>Testowanie aplikacji lokalnie

W terminalu lokalny ustaw następujące zmienne środowiskowe:

```bash
export DB_HOST=<mysql_server_name>.mysql.database.azure.com
export DB_DATABASE=sampledb 
export DB_USERNAME=railsappuser@<mysql_server_name>
export DB_PASSWORD=MySQLAzure2017
```

Uruchom szyny migracji bazy danych z wartościami produkcyjnego skonfigurowanego do tworzenia tabel w bazie danych MySQL w bazie danych Azure dla programu MySQL (wersja zapoznawcza). 

```bash
rake db:migrate RAILS_ENV=production
```

Podczas uruchamiania w środowisku produkcyjnym, aplikacja szyny musi zasoby wstępnie skompilowana. Generuj wymagane zasoby przy użyciu następującego polecenia:

```bash
rake assets:precompile
```

Środowiska produkcyjnego szyny używa również kluczy tajnych do zarządzania zabezpieczeniami. Generowanie klucza tajnego.

```bash
rails secret
```

Zapisz klucz tajny odpowiednich zmienne używane przez szyny środowiska produkcyjnego. Dla wygody można używać tego samego klucza, dla obu zmiennych.

```bash
export RAILS_MASTER_KEY=<output_of_rails_secret>
export SECRET_KEY_BASE=<output_of_rails_secret>
```

Włącz środowiska produkcyjnego szyny do obsługi plików JavaScript i CSS.

```bash
export RAILS_SERVE_STATIC_FILES=true
```

Uruchom przykładową aplikację w środowisku produkcyjnym.

```bash
rails server -e production
```

Przejdź do `http://localhost:3000`. Jeśli strona jest ładowana bez błędów, Ruby w aplikacji szyny nawiązuje połączenie z bazy danych MySQL na platformie Azure.

Dodaj kilka zadań na stronie.

![Ruby na szyny pomyślnie łączy się z bazą danych Azure dla programu MySQL (wersja zapoznawcza)](./media/tutorial-ruby-mysql-app/azure-mysql-connect-success.png)

Aby zatrzymać serwer szyny, wpisz `Ctrl + C` w terminalu.

### <a name="commit-your-changes"></a>Zatwierdź zmiany

Uruchom następujące polecenia Git, aby zatwierdzić zmiany:

```bash
git add .
git commit -m "database.yml updates"
```

Aplikacja jest gotowa do wdrożenia.

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tym kroku możesz wdrożyć aplikację połączone MySQL szyny usłudze Azure App Service.

### <a name="configure-a-deployment-user"></a>Konfigurowanie użytkownika wdrożenia

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web

Za pomocą polecenia [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) można utworzyć aplikację internetową w planie usługi App Service `myAppServicePlan` w usłudze Cloud Shell. 

W poniższym przykładzie zastąp ciąg `<app_name>` globalnie unikatową nazwą aplikacji (prawidłowe znaki to `a-z`, `0-9` i `-`). Środowisko wykonawcze ma ustawioną wartość `RUBY|2.3`, która wdraża [domyślny obraz dopisków fonetycznych](https://hub.docker.com/r/appsvc/ruby/). Aby wyświetlić wszystkie obsługiwane środowiska uruchomieniowe, uruchom polecenie [az webapp list-runtimes](/cli/azure/webapp?view=azure-cli-latest#az_webapp_list_runtimes). 

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --runtime "RUBY|2.3" --deployment-local-git
```

Po utworzeniu aplikacji internetowej w interfejsie wiersza polecenia platformy Azure zostaną wyświetlone dane wyjściowe podobne do następujących:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

Utworzona została nowa pusta aplikacja internetowa z włączonym wdrażaniem git.

> [!NOTE]
> Adres URL zdalnego repozytorium Git jest wyświetlany we właściwości `deploymentLocalGitUrl` w formacie `https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git`. Zapisz ten adres URL, ponieważ będzie on potrzebny później.
>

### <a name="configure-database-settings"></a>Konfiguruj ustawienia bazy danych

W usłudze App Service można ustawić zmienne środowiskowe jako _ustawień aplikacji_ za pomocą [az aplikacji sieci Web config appsettings zestaw](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) polecenie w powłoce chmury.

Następujące polecenia powłoki chmury konfiguruje ustawienia aplikacji `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, i `DB_PASSWORD`. Zastąp symbole zastępcze  _&lt;nazwa_aplikacji >_ i  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.mysql.database.azure.com" DB_DATABASE="sampledb" DB_USERNAME="railsappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```

### <a name="configure-rails-environment-variables"></a>Skonfiguruj szyny zmienne środowiskowe

W lokalnej terminal wygenerować nowy klucz tajny dla środowiska produkcyjnego szyny na platformie Azure.

```bash
rails secret
```

Skonfiguruj zmienne wymagane przez szyny środowiska produkcyjnego.

W poniższym poleceniu powłoki chmury, Zastąp dwa  _&lt;output_of_rails_secret >_ symbole zastępcze za pomocą nowego klucza tajnego generowane w terminalu lokalnego.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings RAILS_MASTER_KEY="<output_of_rails_secret>" SECRET_KEY_BASE="<output_of_rails_secret>" RAILS_SERVE_STATIC_FILES="true" ASSETS_PRECOMPILE="true"
```

`ASSETS_PRECOMPILE="true"`Określa, że domyślnego kontenera dopisków fonetycznych wstępnej kompilacji zasobów w każdym wdrożeniu Git.

### <a name="push-to-azure-from-git"></a>Wypychanie z narzędzia Git na platformę Azure

W terminalu lokalnego Dodaj zdalnego Azure w lokalnym repozytorium Git.

```bash
git remote add azure <paste_copied_url_here>
```

Wypychanie do platformy Azure zdalnego wdrażania Ruby szyny aplikacji. Zostanie wyświetlony monit o hasło, które wcześniej podane w ramach tworzenia użytkownika wdrożenia.

```bash
git push azure master
```

Podczas wdrażania usługi Azure App Service komunikuje się postęp za pomocą narzędzia Git.

```bash
Counting objects: 3, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (3/3), 291 bytes | 0 bytes/s, done.
Total 3 (delta 2), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: Updating submodules.
remote: Preparing deployment for commit id 'a5e076db9c'.
remote: Running custom deployment command...
remote: Running deployment command...
...
< Output has been truncated for readability >
```

### <a name="browse-to-the-azure-web-app"></a>Przejdź do aplikacji sieci web platformy Azure

Przejdź do `http://<app_name>.azurewebsites.net` i dodaj kilka zadań do listy.

![Ruby na szyny uruchomieniu aplikacji w usłudze Azure App Service](./media/tutorial-ruby-mysql-app/ruby-mysql-in-azure.png)

Gratulacje, w przypadku korzystania z danymi Ruby w szyny aplikacji w usłudze Azure App Service.

## <a name="update-model-locally-and-redeploy"></a>Aktualizuj model lokalnie i wdrożenie

W tym kroku zostaną wprowadzone zmiany proste do `task` modelu danych i aplikacji sieci Web, a następnie opublikować aktualizacji na platformie Azure.

W scenariuszu zadań modyfikowania aplikacji tak, aby oznaczyć zadanie jako ukończone.

### <a name="add-a-column"></a>Dodawanie kolumny

W terminalu przejdź do katalogu głównego repozytorium Git.

Generowanie nowego migracji, które dodaje logiczna kolumnę o nazwie `Done` do `Tasks` tabeli:

```bash
rails generate migration add_Done_to_Tasks Done:boolean
```

To polecenie generuje nowy plik migracji w _db/migracji_ katalogu.


W terminalu Uruchom szyny migracji bazy danych, aby wprowadzić zmiany w lokalnej bazie danych.

```bash
rake db:migrate
```

### <a name="update-application-logic"></a>Aktualizowanie aplikacji logiki

Otwórz *app/controllers/tasks_controller.rb* pliku. Na końcu pliku Znajdź następujący wiersz:

```rb
params.require(:task).permit(:Description)
```

Modyfikowanie tego wiersza, aby uwzględnić nowe `Done` parametru.

```rb
params.require(:task).permit(:Description, :Done)
```

### <a name="update-the-views"></a>Aktualizowanie widoków

Otwórz *app/views/tasks/_form.html.erb* pliku, który jest formularz edycji.

Znajdź wiersz `<%=f.error_span(:Description) %>` i Wstaw następujący kod bezpośrednio poniżej:

```erb
<%= f.label :Done, :class => 'control-label col-lg-2' %>
<div class="col-lg-10">
  <%= f.check_box :Done, :class => 'form-control' %>
</div>
```

Otwórz *app/views/tasks/show.html.erb* pliku, który jest jeden rekord strony widoku. 

Znajdź wiersz `<dd><%= @task.Description %></dd>` i Wstaw następujący kod bezpośrednio poniżej:

```erb
  <dt><strong><%= model_class.human_attribute_name(:Done) %>:</strong></dt>
  <dd><%= check_box "task", "Done", {:checked => @task.Done, :disabled => true}%></dd>
```

Otwórz *app/views/tasks/index.html.erb* pliku, który jest strona indeksu dla wszystkich rekordów.

Znajdź wiersz `<th><%= model_class.human_attribute_name(:Description) %></th>` i Wstaw następujący kod bezpośrednio poniżej:

```erb
<th><%= model_class.human_attribute_name(:Done) %></th>
```

W tym samym pliku, wyszukaj wiersz `<td><%= task.Description %></td>` i Wstaw następujący kod bezpośrednio poniżej:

```erb
<td><%= check_box "task", "Done", {:checked => task.Done, :disabled => true} %></td>
```

### <a name="test-the-changes-locally"></a>Testowanie zmiany lokalnie

W terminalu lokalnym, uruchom serwer szyny.

```bash
rails server
```

Aby wyświetlić stan zadania, zmienić, przejdź do `http://localhost:3000` i dodawanie lub edytowanie elementów.

![Dodano pole wyboru do zadania](./media/tutorial-ruby-mysql-app/complete-checkbox.png)

Aby zatrzymać serwer szyny, wpisz `Ctrl + C` w terminalu.

### <a name="publish-changes-to-azure"></a>Publikowanie zmian na platformie Azure

W terminalu Uruchom szyny migracji bazy danych w środowisku produkcyjnym wprowadzić zmiany w bazie danych Azure.

```bash
rake db:migrate RAILS_ENV=production
```

Przekaż wszystkie zmiany w usłudze Git, a następnie Wypchnij zmiany kodu na platformie Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Raz `git push` jest zakończenie, przejdź do aplikacji sieci web platformy Azure i przetestować nową funkcję.

![Zmiany modelu i bazy danych publikowanych w systemie Azure](media/tutorial-ruby-mysql-app/complete-checkbox-published.png)

Jeśli dodano żadnych zadań one zostaną zachowane w bazie danych. Aktualizacje schematu danych pozostawić istniejące dane bez zmian.

## <a name="manage-the-azure-web-app"></a>Zarządzanie aplikacją internetową platformy Azure

Przejdź do witryny [Azure Portal](https://portal.azure.com), aby zarządzać utworzoną aplikacją internetową.

W menu po lewej stronie kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji internetowej platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/tutorial-php-mysql-app/access-portal.png)

Zostanie wyświetlona strona Omówienie aplikacji internetowej. W tym miejscu można wykonać zadania podstawowe możliwości zarządzania, takie jak zatrzymanie, start ponownego uruchomienia, przeglądania i usuwania.

Menu po lewej stronie zawiera strony konfigurowania aplikacji.

![Strona usługi App Service w witrynie Azure Portal](./media/tutorial-php-mysql-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>

## <a name="next-steps"></a>Kolejne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz bazę danych MySQL na platformie Azure
> * Nawiązać Ruby w aplikacji szyny MySQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników diagnostycznych z platformy Azure
> * Zarządzanie aplikacją w portalu Azure

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak zamapować niestandardową nazwę DNS w aplikacji sieci web.

> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure Web Apps (Mapowanie istniejącej niestandardowej nazwy DNS na aplikacje internetowe platformy Azure)](../app-service-web-tutorial-custom-domain.md)
