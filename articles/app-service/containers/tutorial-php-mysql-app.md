---
title: Tworzenie aplikacji sieci web PHP i MySQL na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak pobrać aplikację PHP, praca na platformie Azure z połączenia z bazą danych MySQL na platformie Azure."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 14feb4f3-5095-496e-9a40-690e1414bd73
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 07/21/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: 11e8708987f4e085fc8bf1db10144283a9a17d2e
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="build-a-php-and-mysql-web-app-in-azure"></a>Tworzenie aplikacji sieci web PHP i MySQL na platformie Azure

[Usługa aplikacji w systemie Linux](app-service-linux-intro.md) oferuje wysoce skalowalną, własnym poprawiania usługi hosta sieci web przy użyciu systemu operacyjnego Linux. W tym samouczku pokazano, jak utworzyć aplikację sieci web PHP i podłącz go do bazy danych MySQL. Po zakończeniu będziesz mieć [Laravel](https://laravel.com/) aplikacji uruchomionej w usłudze aplikacji w systemie Linux.

![Aplikacja PHP działające w usłudze aplikacji Azure](./media/tutorial-php-mysql-app/complete-checkbox-published.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz bazę danych MySQL na platformie Azure
> * Łączenie aplikacji PHP MySQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników diagnostycznych z platformy Azure
> * Zarządzanie aplikacją w portalu Azure

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* [Zainstaluj oprogramowanie Git](https://git-scm.com/)
* [Instalowanie języka PHP 5.6.4 lub nowszy](http://php.net/downloads.php)
* [Zainstaluj Composer](https://getcomposer.org/doc/00-intro.md)
* Włącz następujące rozszerzenia PHP potrzeb Laravel: biblioteki OpenSSL, PDO MySQL, Mbstring, Tokenizatora, XML
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

### <a name="create-a-database-locally"></a>Utwórz bazę danych lokalnie

W `mysql` należy utworzyć bazę danych.

```sql 
CREATE DATABASE sampledb;
```

Zakończyć połączenie z serwerem, wpisując `quit`.

```sql
quit
```

<a name="step2"></a>

## <a name="create-a-php-app-locally"></a>Utwórz aplikację PHP lokalnie
W tym kroku Pobierz przykładową aplikację Laravel, skonfiguruj połączenie bazy danych i uruchom lokalnie. 

### <a name="clone-the-sample"></a>Klonowanie próbki

W oknie terminalu `cd` do katalogu roboczego.

Uruchom następujące polecenie w celu sklonowania przykładowego repozytorium.

```bash
git clone https://github.com/Azure-Samples/laravel-tasks
```

`cd`sklonowany katalogu.
Zainstaluj wymagane pakiety.

```bash
cd laravel-tasks
composer install
```

### <a name="configure-mysql-connection"></a>Skonfiguruj połączenie MySQL

W katalogu głównym repozytorium, Utwórz plik o nazwie *.env*. Skopiuj następujące zmienne do *.env* pliku. Zastąp  _&lt;root_password >_ symbolu zastępczego hasła użytkownika root MySQL.

```
APP_ENV=local
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_DATABASE=sampledb
DB_USERNAME=root
DB_PASSWORD=<root_password>
```

Aby uzyskać informacje o używaniu Laravel _.env_ plików, zobacz [konfiguracji środowiska Laravel](https://laravel.com/docs/5.4/configuration#environment-configuration).

### <a name="run-the-sample-locally"></a>Uruchom lokalnie próbki

Uruchom [Laravel bazy danych migracji](https://laravel.com/docs/5.4/migrations) do tworzenia tabel aplikacja potrzebuje. Aby zobaczyć, które tabele są tworzone w migracja, Szukaj w _bazy danych/migracje_ katalogu w repozytorium Git.

```bash
php artisan migrate
```

Wygeneruj nowy klucz aplikacji Laravel.

```bash
php artisan key:generate
```

Uruchom aplikację.

```bash
php artisan serve
```

W przeglądarce przejdź do adresu `http://localhost:8000`. Dodaj kilka zadań na stronie.

![PHP pomyślnie łączy się z MySQL](./media/tutorial-php-mysql-app/mysql-connect-success.png)

Aby zatrzymać PHP, wpisz `Ctrl + C` w terminalu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-mysql-in-azure"></a>Tworzenie MySQL na platformie Azure

W tym kroku utworzysz bazę danych MySQL w [bazy danych Azure dla programu MySQL (wersja zapoznawcza)](/azure/mysql). Następnie należy skonfigurować aplikację PHP do nawiązania połączenia tej bazy danych.

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-mysql-server"></a>Utwórz serwer MySQL

Tworzenie serwera w bazie danych Azure dla programu MySQL (wersja zapoznawcza) z [utworzenie przez serwer mysql az](/cli/azure/mysql/server#create) polecenia.

W poniższym poleceniu zastąp nazwę serwera MySQL, w której występuje  _&lt;mysql_server_name >_ symbolu zastępczego (prawidłowe znaki to `a-z`, `0-9`, i `-`). Ta nazwa jest częścią nazwy hosta serwera MySQL (`<mysql_server_name>.database.windows.net`), musi on być globalnie unikatowe.

```azurecli-interactive
az mysql server create --name <mysql_server_name> --resource-group myResourceGroup --location "North Europe" --admin-user adminuser --admin-password MySQLAzure2017 --ssl-enforcement Disabled
```

Po utworzeniu serwer MySQL, interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql_server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql_server_name>",
  "location": "northeurope",
  "name": "<mysql_server_name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Konfigurowanie zapory serwera

Tworzenie reguły zapory dla serwera MySQL zezwolić na połączenia klientów przy użyciu [az mysql reguły zapory serwera — Utwórz](/cli/azure/mysql/server/firewall-rule#create) polecenia.

```azurecli-interactive
az mysql server firewall-rule create --name allIPs --server <mysql_server_name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Bazy danych platformy Azure dla programu MySQL (wersja zapoznawcza) aktualnie nie ograniczyć połączenia tylko do usług platformy Azure. Jak adresy IP na platformie Azure są przypisywane dynamicznie, lepiej jest umożliwienie wszystkich adresów IP. Usługa jest w wersji zapoznawczej. Zaplanowano lepsze metody zabezpieczania bazy danych.
>

### <a name="connect-to-production-mysql-server-locally"></a>Połączyć się z produkcyjnym serwerem MySQL lokalnie

W oknie terminalu nawiązać serwer MySQL na platformie Azure. Użyj wartości określonej wcześniej dla  _&lt;mysql_server_name >_.

```bash
mysql -u adminuser@<mysql_server_name> -h <mysql_server_name>.database.windows.net -P 3306 -p
```

Po wyświetleniu monitu o podanie hasła, użyj _$tr0ngPa$ w0rd!_, która została określona podczas tworzenia bazy danych.

### <a name="create-a-production-database"></a>Utwórz bazę danych produkcyjnych

W `mysql` należy utworzyć bazę danych.

```sql
CREATE DATABASE sampledb;
```

### <a name="create-a-user-with-permissions"></a>Utwórz użytkownika z uprawnieniami

Utwórz użytkownika bazy danych o nazwie _phpappuser_ i nadaj mu wszystkie uprawnienia `sampledb` bazy danych.

```sql
CREATE USER 'phpappuser' IDENTIFIED BY 'MySQLAzure2017'; 
GRANT ALL PRIVILEGES ON sampledb.* TO 'phpappuser';
```

Zakończyć połączenie z serwerem, wpisując `quit`.

```sql
quit
```

## <a name="connect-app-to-azure-mysql"></a>Łączenie aplikacji z platformy Azure MySQL

W tym kroku możesz połączyć aplikację PHP bazy danych MySQL utworzonej w bazie danych Azure dla programu MySQL (wersja zapoznawcza).

<a name="devconfig"></a>

### <a name="configure-the-database-connection"></a>Skonfiguruj połączenie bazy danych

W folderze głównym repozytorium, Utwórz _. env.production_ plik i skopiuj następujące zmienne do niego. Zastąp symbol zastępczy  _&lt;mysql_server_name >_.

```
APP_ENV=production
APP_DEBUG=true
APP_KEY=SomeRandomString

DB_CONNECTION=mysql
DB_HOST=<mysql_server_name>.database.windows.net
DB_DATABASE=sampledb
DB_USERNAME=phpappuser@<mysql_server_name>
DB_PASSWORD=MySQLAzure2017
```
<!-- MYSQL_SSL=true -->

Zapisz zmiany.

> [!TIP]
> Aby zabezpieczyć informacje o połączeniu MySQL, ten plik jest już wykluczone z repozytorium Git (zobacz _.gitignore_ w folderze głównym repozytorium). Później możesz dowiedzieć się, jak skonfigurować zmienne środowiskowe w usłudze App Service w celu połączenia z bazą danych w bazie danych Azure dla programu MySQL (wersja zapoznawcza). Zmienne środowiskowe nie wymagają *.env* pliku w usłudze App Service.
>

<!-- ### Configure SSL certificate

By default, Azure Database for MySQL enforces SSL connections from clients. To connect to your MySQL database in Azure, you must use a _.pem_ SSL certificate.

Open _config/database.php_ and add the _sslmode_ and _options_ parameters to `connections.mysql`, as shown in the following code.

```php
'mysql' => [
    ...
    'sslmode' => env('DB_SSLMODE', 'prefer'),
    'options' => (env('MYSQL_SSL')) ? [
        PDO::MYSQL_ATTR_SSL_KEY    => '/ssl/certificate.pem', 
    ] : []
],
```

To learn how to generate this _certificate.pem_, see [Configure SSL connectivity in your application to securely connect to Azure Database for MySQL](../../mysql/howto-configure-ssl.md).

> [!TIP]
> The path _/ssl/certificate.pem_ points to an existing _certificate.pem_ file in the Git repository. This file is provided for convenience in this tutorial. For best practice, you should not commit your _.pem_ certificates into source control. 
> -->

### <a name="test-the-application-locally"></a>Testowanie aplikacji lokalnie

Uruchom Laravel migracji bazy danych z _. env.production_ jako plik środowiska do tworzenia tabel w bazie danych MySQL w bazie danych Azure dla programu MySQL (wersja zapoznawcza). Należy pamiętać, że _. env.production_ zawiera informacje o połączeniu z bazą danych MySQL na platformie Azure.

```bash
php artisan migrate --env=production --force
```

_. env.production_ nie ma jeszcze klucza prawidłową aplikację. Generuj nową dla niego w terminalu.

```bash
php artisan key:generate --env=production --force
```

Uruchom przykładową aplikację z _. env.production_ jako plik środowiska.

```bash
php artisan serve --env=production
```

Przejdź do `http://localhost:8000`. Jeśli strona jest ładowana bez błędów, aplikacji PHP nawiązuje połączenie z bazą danych MySQL na platformie Azure.

Dodaj kilka zadań na stronie.

![PHP pomyślnie łączy się z bazą danych Azure dla programu MySQL (wersja zapoznawcza)](./media/tutorial-php-mysql-app/mysql-connect-success.png)

Aby zatrzymać PHP, wpisz `Ctrl + C` w terminalu.

### <a name="commit-your-changes"></a>Zatwierdź zmiany

Uruchom następujące polecenia Git, aby zatwierdzić zmiany:

```bash
git add .
git commit -m "database.php updates"
```

Aplikacja jest gotowa do wdrożenia.

## <a name="deploy-to-azure"></a>Wdrażanie na platformie Azure

W tym kroku możesz wdrożyć aplikację PHP, MySQL, podłączone do usługi Azure App Service.

### <a name="configure-a-deployment-user"></a>Konfigurowanie użytkownika wdrożenia

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service

[!INCLUDE [Create app service plan no h](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-php-no-h.md)] 

### <a name="configure-database-settings"></a>Konfigurowanie ustawień bazy danych

W usłudze App Service można ustawić zmienne środowiskowe jako _ustawień aplikacji_ za pomocą [az aplikacji sieci Web config appsettings zestaw](/cli/azure/webapp/config/appsettings#set) polecenia.

Następujące polecenie konfiguruje ustawienia aplikacji `DB_HOST`, `DB_DATABASE`, `DB_USERNAME`, i `DB_PASSWORD`. Zastąp symbole zastępcze  _&lt;nazwa_aplikacji >_ i  _&lt;mysql_server_name >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DB_HOST="<mysql_server_name>.database.windows.net" DB_DATABASE="sampledb" DB_USERNAME="phpappuser@<mysql_server_name>" DB_PASSWORD="MySQLAzure2017"
```
 <!-- MYSQL_SSL="true" -->

Można użyć PHP [getenv —](http://www.php.net/manual/function.getenv.php) metodę, aby uzyskać dostęp do ustawień. używa kod Laravel [env](https://laravel.com/docs/5.4/helpers#method-env) otoki za pośrednictwem PHP `getenv`. Na przykład konfiguracji MySQL w _config/database.php_ wygląda podobnie do następującego kodu:

```php
'mysql' => [
    'driver'    => 'mysql',
    'host'      => env('DB_HOST', 'localhost'),
    'database'  => env('DB_DATABASE', 'forge'),
    'username'  => env('DB_USERNAME', 'forge'),
    'password'  => env('DB_PASSWORD', ''),
    ...
],
```

### <a name="configure-laravel-environment-variables"></a>Skonfiguruj Laravel zmienne środowiskowe

Laravel musi mieć klucz aplikacji w usłudze App Service. Możesz ją skonfigurować z ustawieniami aplikacji.

Użyj `php artisan` aby wygenerować nowy klucz aplikacji bez jej zapisywania _.env_.

```bash
php artisan key:generate --show
```

Ustaw klucz aplikacji w usłudze App Service aplikacji sieci web przy użyciu [az aplikacji sieci Web config appsettings zestaw](/cli/azure/webapp/config/appsettings#set) polecenia. Zastąp symbole zastępcze  _&lt;nazwa_aplikacji >_ i  _&lt;outputofphpartisankey: generowanie >_.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings APP_KEY="<output_of_php_artisan_key:generate>" APP_DEBUG="true"
```

`APP_DEBUG="true"`Określa, że Laravel do zwracania informacji debugowania, gdy wdrożonej aplikacji sieci web wystąpią błędy. Podczas uruchamiania aplikacji produkcyjnych, ustaw ją na `false`, który jest bardziej bezpieczne.

### <a name="set-the-virtual-application-path"></a>Ustaw ścieżkę aplikacji wirtualnej

Ustaw ścieżkę aplikacji wirtualnej dla aplikacji sieci web. Ten krok jest wymagany, ponieważ [cyklem życia aplikacji Laravel](https://laravel.com/docs/5.4/lifecycle) rozpoczyna się w _publicznego_ katalogu zamiast katalogu głównego aplikacji. Innych platform PHP, na których cyklu życia start w katalogu głównym może działać bez ręcznej konfiguracji ścieżki aplikacji wirtualnej.

Ustaw ścieżkę aplikacji wirtualnych za pomocą [aktualizacja zasobu az](/cli/azure/resource#update) polecenia. Zastąp  _&lt;nazwa_aplikacji >_ symbolu zastępczego.

```azurecli-interactive
az resource update --name web --resource-group myResourceGroup --namespace Microsoft.Web --resource-type config --parent sites/<app_name> --set properties.virtualApplications[0].physicalPath="site\wwwroot\public" --api-version 2015-06-01
```

Domyślnie usługi Azure App Service punktów ścieżki katalogu głównego aplikacji wirtualnej (_/_) do katalogu głównego plików wdrożonej aplikacji (_sites\wwwroot_).

### <a name="push-to-azure-from-git"></a>Wypychanie z narzędzia Git na platformę Azure

Dodaj zdalną platformę Azure do lokalnego repozytorium Git.

```bash
git remote add azure <paste_copied_url_here>
```

Wypychanie do platformy Azure zdalnego, aby wdrożyć aplikację PHP. Zostanie wyświetlony monit o hasło, które wcześniej podane w ramach tworzenia użytkownika wdrożenia.

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

> [!NOTE]
> Można zauważyć, że proces wdrażania instaluje [Composer](https://getcomposer.org/) pakietów na końcu. Usługi aplikacji nie działa te automatyzacji podczas wdrażania domyślne, to repozytorium przykładowej ma trzy dodatkowe pliki w katalogu głównym ją włączyć:
>
> - `.deployment`— Ten plik zawiera usługę aplikacji w celu uruchomienia `bash deploy.sh` jako niestandardowe wdrożenie skryptu.
> - `deploy.sh`-Niestandardowe wdrożenie skryptu. Jeśli przejrzenie pliku, zobaczysz, że działa `php composer.phar install` po `npm install`.
> - `composer.phar`-Composer Menedżera pakietów.
>
> Ta metoda służy do dodawania każdy krok do wdrożenia na podstawie Git do usługi App Service. Aby uzyskać więcej informacji, zobacz [niestandardowego skryptu wdrażania](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).
>

### <a name="browse-to-the-azure-web-app"></a>Przejdź do aplikacji sieci web platformy Azure

Przejdź do `http://<app_name>.azurewebsites.net` i dodaj kilka zadań do listy.

![Aplikacja PHP działające w usłudze aplikacji Azure](./media/tutorial-php-mysql-app/php-mysql-in-azure.png)

Gratulacje, używasz aplikacji PHP sieci opartych na danych w usłudze Azure App Service.

## <a name="update-model-locally-and-redeploy"></a>Aktualizuj model lokalnie i wdrożenie

W tym kroku zostaną wprowadzone zmiany proste do `task` modelu danych i aplikacji sieci Web, a następnie opublikować aktualizacji na platformie Azure.

W scenariuszu zadań modyfikowania aplikacji tak, aby oznaczyć zadanie jako ukończone.

### <a name="add-a-column"></a>Dodaj kolumnę

W terminalu przejdź do katalogu głównego repozytorium Git.

Generuj nowe migracja bazy danych dla `tasks` tabeli:

```bash
php artisan make:migration add_complete_column --table=tasks
```

To polecenie przedstawia nazwę wygenerowanego pliku migracji. Ten plik w _bazy danych/migracje_ i otwórz go.

Zastąp `up` metodę z następującym kodem:

```php
public function up()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->boolean('complete')->default(False);
    });
}
```

Poprzedni kod dodaje logiczna kolumnę w `tasks` tabeli o nazwie `complete`.

Zastąp `down` metoda akcji wycofywania następującym kodem:

```php
public function down()
{
    Schema::table('tasks', function (Blueprint $table) {
        $table->dropColumn('complete');
    });
}
```

W terminalu Uruchom Laravel migracji bazy danych, aby wprowadzić zmiany w lokalnej bazie danych.

```bash
php artisan migrate
```

Na podstawie [konwencji nazewnictwa Laravel](https://laravel.com/docs/5.4/eloquent#defining-models), model `Task` (zobacz _app/Task.php_) mapuje `tasks` tabeli domyślnie.

### <a name="update-application-logic"></a>Aktualizowanie aplikacji logiki

Otwórz *routes/web.php* pliku. Aplikacja definiuje jego tras i logiki biznesowej w tym miejscu.

Na końcu pliku należy dodać trasę z następującym kodem:

```php
/**
 * Toggle Task completeness
 */
Route::post('/task/{id}', function ($id) {
    error_log('INFO: post /task/'.$id);
    $task = Task::findOrFail($id);

    $task->complete = !$task->complete;
    $task->save();

    return redirect('/');
});
```

Poprzedni kod umożliwia proste aktualizacji do modelu danych przełączając wartość `complete`.

### <a name="update-the-view"></a>Aktualizowanie widoku

Otwórz *resources/views/tasks.blade.php* pliku. Znajdź `<tr>` tagu początkowego i zastąpić go ciągiem:

```html
<tr class="{{ $task->complete ? 'success' : 'active' }}" >
```

Poprzedni kod zmienia kolor wiersza, w zależności od tego, czy zadanie zostało ukończone.

W następnym wierszu masz następujący kod:

```html
<td class="table-text"><div>{{ $task->name }}</div></td>
```

Zastąp cały wiersz z następującym kodem:

```html
<td>
    <form action="{{ url('task/'.$task->id) }}" method="POST">
        {{ csrf_field() }}

        <button type="submit" class="btn btn-xs">
            <i class="fa {{$task->complete ? 'fa-check-square-o' : 'fa-square-o'}}"></i>
        </button>
        {{ $task->name }}
    </form>
</td>
```

Poprzedni kod dodaje przycisk przesyłania, który odwołuje się do wcześniej zdefiniowanego trasy.

### <a name="test-the-changes-locally"></a>Testowanie zmiany lokalnie

Z katalogu głównego repozytorium Git należy uruchomić serwera wdrożeniowego.

```bash
php artisan serve
```

Aby wyświetlić stan zadania, zmienić, przejdź do `http://localhost:8000` i zaznacz pole wyboru.

![Dodano pole wyboru do zadania](./media/tutorial-php-mysql-app/complete-checkbox.png)

Aby zatrzymać PHP, wpisz `Ctrl + C` w terminalu.

### <a name="publish-changes-to-azure"></a>Publikowanie zmian na platformie Azure

W terminalu Uruchom Laravel migracji bazy danych z parametrami połączenia produkcji wprowadzić zmianę w bazie danych Azure.

```bash
php artisan migrate --env=production --force
```

Przekaż wszystkie zmiany w usłudze Git, a następnie Wypchnij zmiany kodu na platformie Azure.

```bash
git add .
git commit -m "added complete checkbox"
git push azure master
```

Raz `git push` jest zakończenie, przejdź do aplikacji sieci web platformy Azure i przetestować nową funkcję.

![Zmiany modelu i bazy danych publikowanych w systemie Azure](media/tutorial-php-mysql-app/complete-checkbox-published.png)

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

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Utwórz bazę danych MySQL na platformie Azure
> * Łączenie aplikacji PHP MySQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników diagnostycznych z platformy Azure
> * Zarządzanie aplikacją w portalu Azure

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak zamapować niestandardową nazwę DNS w aplikacji sieci web.

> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure Web Apps (Mapowanie istniejącej niestandardowej nazwy DNS na aplikacje internetowe platformy Azure)](../app-service-web-tutorial-custom-domain.md)
