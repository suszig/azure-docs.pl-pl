---
title: Tworzenie aplikacji sieci web Docker Python i PostgreSQL na platformie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak pobrać aplikację Docker Python, działa na platformie Azure z połączeniem z bazą danych PostgreSQL."
services: app-service\web
documentationcenter: python
author: berndverst
manager: erikre
editor: 
ms.assetid: 2bada123-ef18-44e5-be71-e16323b20466
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: tutorial
ms.date: 05/03/2017
ms.author: beverst
ms.custom: mvc
ms.openlocfilehash: fa3aa3a73338970fde2d0b0230e7b2e6ca687dc9
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/25/2017
---
# <a name="build-a-docker-python-and-postgresql-web-app-in-azure"></a>Tworzenie aplikacji sieci web Docker Python i PostgreSQL na platformie Azure

Aplikacji dla kontenerów sieci Web oferuje wysoce skalowalną, własnym poprawiania usługi hosta sieci web. W tym samouczku przedstawiono sposób tworzenia podstawowej aplikacji sieci web Docker Python na platformie Azure. Będzie połączyć tę aplikację z bazy danych programu PostgreSQL. Gdy wszystko będzie gotowe, będziesz mieć aplikację platformy Python Flask uruchomione w kontenerze Docker na [usługi aplikacji w systemie Linux](app-service-linux-intro.md).

![Docker Python Flask aplikacji w usłudze App Service w systemie Linux](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

Na macOS można wykonać poniższe czynności. Instrukcje dotyczące systemu Linux i Windows są takie same, w większości przypadków, ale różnice nie są szczegółowo opisane w tym samouczku.
 
## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

1. [Zainstaluj oprogramowanie Git](https://git-scm.com/)
1. [Zainstaluj język Python](https://www.python.org/downloads/)
1. [Zainstaluj i uruchom PostgreSQL](https://www.postgresql.org/download/)
1. [Zainstaluj Docker Community Edition](https://www.docker.com/community-edition)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Jeśli zdecydujesz się zainstalować interfejs wiersza polecenia i korzystać z niego lokalnie, ten temat będzie wymagał interfejsu wiersza polecenia platformy Azure w wersji 2.0 lub nowszej. Uruchom polecenie `az --version`, aby dowiedzieć się, jaka wersja jest używana. Jeśli konieczna będzie instalacja lub uaktualnienie, zobacz [Instalowanie interfejsu wiersza polecenia platformy Azure 2.0]( /cli/azure/install-azure-cli). 

## <a name="test-local-postgresql-installation-and-create-a-database"></a>Testowanie instalacji lokalnej PostgreSQL i utworzyć bazę danych

Otwórz okno terminalu i uruchom `psql postgres` nawiązać połączenia z serwerem lokalnym PostgreSQL.

```bash
psql postgres
```

W przypadku pomyślnego nawiązania połączenia z bazą danych PostgreSQL jest uruchomiona. Jeśli nie, upewnij się, że lokalnej bazy danych PostgresQL została uruchomiona, wykonując kroki opisane w temacie [pobiera - dystrybucji Core PostgreSQL](https://www.postgresql.org/download/).

Utwórz bazę danych o nazwie *eventregistration* i Ustaw użytkownika oddzielnej bazy danych o nazwie *Menedżera* hasłem *supersecretpass*.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```
Typ *\q* aby zamknąć klienta programu PostgreSQL. 

<a name="step2"></a>

## <a name="create-local-python-flask-application"></a>Tworzenie aplikacji platformy Python Flask lokalnej

Ten krok służy do konfigurowania lokalnego projektu platformy Python Flask.

### <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

Otwórz okno terminala i `CD` do katalogu roboczego.

Uruchom następujące polecenia w klonowania repozytorium przykładowej i przejdź do *initialapp 0,1* wersji.

```bash
git clone https://github.com/Azure-Samples/docker-flask-postgres.git
cd docker-flask-postgres
git checkout tags/0.1-initialapp
```

To repozytorium przykładowej zawiera [Flask](http://flask.pocoo.org/) aplikacji. 

### <a name="run-the-application"></a>Uruchamianie aplikacji

> [!NOTE] 
> W kolejnym kroku można ułatwić ten proces przez utworzenie kontenera Docker można używać z produkcyjną bazę danych.

Zainstaluj wymagane pakiety i uruchom aplikację.

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
pip install -r requirements.txt
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Gdy aplikacja zostanie całkowicie załadowany, zobacz podobny do następującego:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

W przeglądarce przejdź do adresu `http://127.0.0.1:5000`. Kliknij przycisk **zarejestrować!** i tworzenie użytkownika testowego.

![Platformy Python Flask aplikacji uruchomionej na komputerze lokalnym](./media/tutorial-docker-python-postgresql-app/local-app.png)

Przykładowa aplikacja platformy Flask przechowuje dane użytkownika w bazie danych. W przypadku pomyślnego na rejestrowanie użytkowników, aplikacja zapisuje dane w lokalnej bazie danych PostgreSQL.

Aby zatrzymać serwer platformy Flask, w dowolnym momencie, wpisz klawisze Ctrl + C w terminalu. 

## <a name="create-a-production-postgresql-database"></a>Utwórz bazę danych PostgreSQL produkcji

W tym kroku utworzysz bazę danych PostgreSQL na platformie Azure. Gdy aplikacja jest wdrażana na platformie Azure, będzie on używać tej bazy danych w chmurze.

### <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.

Teraz ma Azure CLI 2.0 umożliwia tworzenie zasobów niezbędnych do hostowania aplikacji języka Python w aplikacji sieci Web dla kontenerów.  Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

Utwórz [grupę zasobów](../../azure-resource-manager/resource-group-overview.md) za pomocą polecenia [az group create](/cli/azure/group#create).

[!INCLUDE [Resource group intro](../../../includes/resource-group.md)]

Poniższy przykład tworzy grupę zasobów regionu zachodnie stany USA:

```azurecli-interactive
az group create --name myResourceGroup --location "West US"
```

Użyj [appservice az listy lokalizacje](/cli/azure/appservice#list-locations) polecenia wiersza polecenia platformy Azure do listy dostępnych lokalizacji.

### <a name="create-an-azure-database-for-postgresql-server"></a>Tworzenie serwera usługi Azure Database for PostgreSQL

Utwórz serwer PostgreSQL z [utworzenie przez serwer postgres az](/cli/azure/documentdb#create) polecenia.

W poniższym poleceniu zastąp unikatową nazwą serwera dla  *\<postgresql_name >* nazw dla symbolu zastępczego i użytkownik  *\<admin_username >* symbolu zastępczego. Nazwa serwera jest używana jako część PostgreSQL punktu końcowego (`https://<postgresql_name>.postgres.database.azure.com`), więc nazwa musi być unikatowa na wszystkich serwerach w systemie Azure. Nazwa użytkownika jest dla konta użytkownika administracyjnego początkowej bazy danych. Zostanie wyświetlony monit wybierz hasło dla tego użytkownika.

```azurecli-interactive
az postgres server create --resource-group myResourceGroup --name <postgresql_name> --admin-user <admin_username>
```

Po utworzeniu bazy danych Azure dla serwera PostgreSQL interfejsu wiersza polecenia Azure zawiera informacje podobne do poniższego przykładu:

```json
{
  "administratorLogin": "<my_admin_username>",
  "fullyQualifiedDomainName": "<postgresql_name>.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>",
  "location": "westus",
  "name": "<postgresql_name>",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capacity": 100,
    "family": null,
    "name": "PGSQLS3M100",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

### <a name="create-a-firewall-rule-for-the-azure-database-for-postgresql-server"></a>Tworzenie reguły zapory bazy danych Azure PostgreSQL serwera

Uruchom następujące polecenie wiersza polecenia platformy Azure, aby zezwolić na dostęp do bazy danych ze wszystkich adresów IP.

```azurecli-interactive
az postgres server firewall-rule create --resource-group myResourceGroup --server-name <postgresql_name> --start-ip-address=0.0.0.0 --end-ip-address=255.255.255.255 --name AllowAllIPs
```

Azure CLI potwierdza tworzenia reguły zapory, przy czym dane wyjściowe podobne do poniższego przykładu:

```json
{
  "endIpAddress": "255.255.255.255",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforPostgreSQL/servers/<postgresql_name>/firewallRules/AllowAllIPs",
  "name": "AllowAllIPs",
  "resourceGroup": "myResourceGroup",
  "startIpAddress": "0.0.0.0",
  "type": "Microsoft.DBforPostgreSQL/servers/firewallRules"
}
```

## <a name="connect-your-python-flask-application-to-the-database"></a>Połączyć aplikację platformy Python Flask do bazy danych

W tym kroku łączysz przykładowej aplikacji platformy Python Flask do bazy danych Azure PostgreSQL serwera, który został utworzony.

### <a name="create-an-empty-database-and-set-up-a-new-database-application-user"></a>Utwórz pustą bazę danych i ustaw nowego użytkownika bazy danych aplikacji

Utwórz użytkownika bazy danych z dostępem do tylko jednej bazy danych. Te poświadczenia będą używane w celu uniknięcia nadanie pełny dostęp do aplikacji na serwerze.

Połączenia z bazą danych (zostanie wyświetlony monit o hasło administratora).

```bash
psql -h <postgresql_name>.postgres.database.azure.com -U <my_admin_username>@<postgresql_name> postgres
```

Utwórz bazę danych i użytkownika z interfejsu wiersza polecenia PostgreSQL.

```bash
CREATE DATABASE eventregistration;
CREATE USER manager WITH PASSWORD 'supersecretpass';
GRANT ALL PRIVILEGES ON DATABASE eventregistration TO manager;
```

Typ *\q* aby zamknąć klienta programu PostgreSQL.

### <a name="test-the-application-locally-against-the-azure-postgresql-database"></a>Testowanie aplikacji lokalnie w bazie danych Azure PostgreSQL

Po powrocie teraz do *aplikacji* folderu sklonowanego repozytorium Github, można uruchomić aplikacji platformy Python Flask, aktualizując bazy danych zmiennych środowiskowych.

```bash
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Gdy aplikacja zostanie całkowicie załadowany, zobacz podobny do następującego:

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
INFO  [alembic.runtime.migration] Running upgrade  -> 791cd7d80402, empty message
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```

Przejdź do http://127.0.0.1:5000 w przeglądarce. Kliknij przycisk **zarejestrować!** i Utwórz rejestracji testu. Dane są teraz zapisywania do bazy danych na platformie Azure.

![Platformy Python Flask aplikacji uruchomionej na komputerze lokalnym](./media/tutorial-docker-python-postgresql-app/local-app.png)

### <a name="running-the-application-from-a-docker-container"></a>Uruchamianie aplikacji z kontenera Docker

Tworzenie obrazu kontenera Docker.

```bash
cd ..
docker build -t flask-postgresql-sample .
```

Docker wyświetla potwierdzenie, że pomyślnie utworzono kontener.

```bash
Successfully built 7548f983a36b
```

Dodaj zmiennych środowiskowych bazy danych do pliku zmiennej środowiskowej *db.env*. Aplikacja będzie łączyć się PostgreSQL produkcyjną bazę danych na platformie Azure.

```text
DBHOST="<postgresql_name>.postgres.database.azure.com"
DBUSER="manager@<postgresql_name>"
DBNAME="eventregistration"
DBPASS="supersecretpass"
```

Uruchamianie aplikacji z w kontenerze Docker. Polecenie Określa plik zmiennej środowiskowej i mapuje Flask domyślny port 5000 na port lokalny 5000.

```bash
docker run -it --env-file db.env -p 5000:5000 flask-postgresql-sample
```

Wynik jest podobny do wcześniej przedstawiono. Jednak migracji wstępnej bazy danych nie jest już musi zostać wykonana i w związku z tym jest pomijana.

```bash
INFO  [alembic.runtime.migration] Context impl PostgresqlImpl.
INFO  [alembic.runtime.migration] Will assume transactional DDL.
 * Serving Flask app "app"
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
```

Baza danych zawiera już utworzonego wcześniej rejestracji.

![Docker na podstawie kontenera platformy Python Flask aplikacji uruchomionej na komputerze lokalnym](./media/tutorial-docker-python-postgresql-app/local-docker.png)

## <a name="upload-the-docker-container-to-a-container-registry"></a>Przekaż kontenera Docker w rejestrze kontenera

W tym kroku możesz przekazać kontenera Docker rejestru kontenera. Użyjesz rejestru kontenera platformy Azure, ale można również używać innych popularnych protokołów, takich jak Centrum Docker.

### <a name="create-an-azure-container-registry"></a>Tworzenie rejestru Azure Container Registry

W następujące polecenie, aby utworzyć rejestru kontenera Zastąp  *\<registry_name >* o nazwie rejestru unikatowy kontenera platformy Azure, wybranych przez użytkownika.

```azurecli-interactive
az acr create --name <registry_name> --resource-group myResourceGroup --location "West US" --sku Basic
```

Dane wyjściowe

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-05-04T08:50:55.635688+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<registry_name>",
  "location": "westus",
  "loginServer": "<registry_name>.azurecr.io",
  "name": "<registry_name>",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "<registry_name>01234"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="retrieve-the-registry-credentials-for-pushing-and-pulling-docker-images"></a>Pobieranie poświadczeń rejestru dla wypychanie i ściąganie obrazy usługi Docker

Aby pokazać rejestru poświadczeń, Włącz tryb administratora najpierw.

```azurecli-interactive
az acr update --name <registry_name> --admin-enabled true
az acr credential show -n <registry_name>
```

Możesz sprawdzić dwa hasła. Zanotuj nazwę użytkownika i hasło pierwszy.

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "<registry_password>"
    },
    {
      "name": "password2",
      "value": "<registry_password2>"
    }
  ],
  "username": "<registry_name>"
}
```

### <a name="upload-your-docker-container-to-azure-container-registry"></a>Przekazywanie z kontenera Docker w rejestrze kontenera platformy Azure

```bash
docker login <registry_name>.azurecr.io -u <registry_name> -p "<registry_password>"
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
```

## <a name="deploy-the-docker-python-flask-application-to-azure"></a>Wdrażanie aplikacji platformy Python Flask Docker na platformie Azure

W tym kroku, w przypadku wdrażania rozwiązania Docker na podstawie kontenera platformy Python Flask aplikacji do usługi Azure App Service.

### <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service

Utwórz plan usługi App Service za pomocą polecenia [az appservice plan create](/cli/azure/appservice/plan#create).

[!INCLUDE [app-service-plan](../../../includes/app-service-plan-linux.md)]

Poniższy przykład tworzy plan usługi aplikacji opartych na systemie Linux o nazwie *myAppServicePlan* przy użyciu cennik S1 warstwy:

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Po utworzeniu planu usługi aplikacji Azure CLI pokazuje informacje podobne do poniższego przykładu:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "West US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan", 
  "kind": "linux",
  "location": "West US",
  "maximumNumberOfWorkers": 10,
  "name": "myAppServicePlan",
  "numberOfSites": 0,
  "perSiteScaling": false,
  "provisioningState": "Succeeded",
  "reserved": true,
  "resourceGroup": "myResourceGroup",
  "sku": {
    "capabilities": null,
    "capacity": 1,
    "family": "S",
    "locations": null,
    "name": "S1",
    "size": "S1",
    "skuCapacity": null,
    "tier": "Standard"
  },
  "status": "Ready",
  "subscription": "00000000-0000-0000-0000-000000000000",
  "tags": null,
  "targetWorkerCount": 0,
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

### <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web

Tworzenie aplikacji sieci web w *myAppServicePlan* planu usługi aplikacji z [tworzenie aplikacji sieci Web az](/cli/azure/webapp#create) polecenia.

Aplikacja sieci web udostępnia hostingu miejsca, aby wdrożyć kod i zawiera adres URL do wyświetlania wdrożonej aplikacji. Służy do tworzenia aplikacji sieci web.

W poniższym poleceniu zastąp  *\<nazwa_aplikacji >* symbol zastępczy unikatowej nazwy aplikacji. Ta nazwa jest częścią domyślny adres URL aplikacji sieci web, więc nazwa musi być unikatowy w obrębie wszystkich aplikacji w usłudze Azure App Service.

```azurecli
az webapp create --name <app_name> --resource-group myResourceGroup --plan myAppServicePlan
```

Po utworzeniu aplikacji internetowej w interfejsie wiersza polecenia platformy Azure zostaną wyświetlone informacje podobne do następujących:

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

### <a name="configure-the-database-environment-variables"></a>Skonfiguruj zmienne środowiskowe bazy danych

Wcześniej w samouczku zdefiniowano zmiennych środowiskowych w celu połączenia z bazą danych PostgreSQL.

W usłudze App Service można ustawić zmienne środowiskowe jako _ustawień aplikacji_ za pomocą [az aplikacji sieci Web config appsettings zestaw](/cli/azure/webapp/config#set) polecenia.

W poniższym przykładzie szczegóły połączenia bazy danych jako ustawienia aplikacji. Ponadto użyto *portu* zmienną do mapy PORT 5000 z Twojej kontenera Docker na odbieranie ruchu HTTP na porcie 80.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings DBHOST="<postgresql_name>.postgres.database.azure.com" DBUSER="manager@<postgresql_name>" DBPASS="supersecretpass" DBNAME="eventregistration" PORT=5000
```

### <a name="configure-docker-container-deployment"></a>Konfigurowanie wdrożenia kontenera Docker

Usługi aplikacji może automatycznie Pobierz i uruchom kontener Docker.

```azurecli
az webapp config container set --resource-group myResourceGroup --name <app_name> --docker-registry-server-user "<registry_name>" --docker-registry-server-password "<registry_password>" --docker-custom-image-name "<registry_name>.azurecr.io/flask-postgresql-sample" --docker-registry-server-url "https://<registry_name>.azurecr.io"
```

Zawsze, gdy aktualizacji kontenera Docker lub zmienić ustawienia, uruchom ponownie aplikację. Ponowne uruchomienie gwarantuje, że wszystkie ustawienia są stosowane i kontenerze najnowsze są pobierane z rejestru.

```azurecli-interactive
az webapp restart --resource-group myResourceGroup --name <app_name>
```

### <a name="browse-to-the-azure-web-app"></a>Przejdź do aplikacji sieci web platformy Azure 

Przejdź do wdrożonej aplikacji sieci web za pomocą przeglądarki sieci web. 

```bash 
http://<app_name>.azurewebsites.net 
```
> [!NOTE]
> Aplikacja sieci web obsługuje żądanie dłużej załadować, ponieważ kontener ma zostać pobrana i uruchomić po zmianie konfiguracji kontenera.

Widać wcześniej zarejestrowanego gości, które zostały zapisane w bazie danych Azure środowiska produkcyjnego w poprzednim kroku.

![Docker na podstawie kontenera platformy Python Flask aplikacji uruchomionej na komputerze lokalnym](./media/tutorial-docker-python-postgresql-app/docker-app-deployed.png)

**Gratulacje!** Używasz Docker aplikacji platformy Python Flask kontenera w usłudze Azure App Service.

## <a name="update-data-model-and-redeploy"></a>Aktualizacja modelu danych i utwórz je ponownie

W tym kroku dodaniu liczba uczestników do każdej rejestracji zdarzeń, aktualizując modelu gościa.

Zapoznaj się z *migracji 0,2* wersji przy użyciu następującego polecenia git:

```bash
git checkout tags/0.2-migration
```

Tej wersji zostały już wprowadzone niezbędne widoki, kontrolery i modelu. Obejmuje też migracja bazy danych, wygenerowane za pomocą *alembic* (`flask db migrate`). Można wyświetlić wszystkie zmiany dokonane za pomocą następujących poleceń git:

```bash
git diff 0.1-initialapp 0.2-migration
```

### <a name="test-your-changes-locally"></a>Przetestuj zmiany lokalnie

Uruchom następujące polecenia, aby przetestować zmiany lokalnie, uruchamiając serwera platformy flask.

Mac / Linux:
```bash
source venv/bin/activate
cd app
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask db upgrade
FLASK_APP=app.py DBHOST="localhost" DBUSER="manager" DBNAME="eventregistration" DBPASS="supersecretpass" flask run
```

Przejdź do http://127.0.0.1:5000 w przeglądarce, aby zobaczyć zmiany. Utwórz rejestracji testu.

![Docker na podstawie kontenera platformy Python Flask aplikacji uruchomionej na komputerze lokalnym](./media/tutorial-docker-python-postgresql-app/local-app-v2.png)

### <a name="publish-changes-to-azure"></a>Publikowanie zmian na platformie Azure

Utwórz nowy obraz docker wypchnąć go do rejestru kontenera i uruchom ponownie aplikację.

```bash
docker build -t flask-postgresql-sample .
docker tag flask-postgresql-sample <registry_name>.azurecr.io/flask-postgresql-sample
docker push <registry_name>.azurecr.io/flask-postgresql-sample
az appservice web restart --resource-group myResourceGroup --name <app_name>
```

Przejdź do aplikacji sieci web platformy Azure i ponownie wypróbowanie nowych funkcji. Utwórz inną rejestracja zdarzeń.

```bash 
http://<app_name>.azurewebsites.net 
```

![Docker Python Flask aplikacji w usłudze aplikacji Azure](./media/tutorial-docker-python-postgresql-app/docker-flask-in-azure.png)

## <a name="manage-your-azure-web-app"></a>Zarządzanie aplikacji sieci web platformy Azure

Przejdź do [portalu Azure](https://portal.azure.com) zobaczyć aplikacji sieci web został utworzony.

W lewym menu kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/tutorial-docker-python-postgresql-app/app-resource.png)

Domyślnie portalu zawiera aplikację sieci web **omówienie** strony. Ta strona udostępnia widok sposobu działania aplikacji. Tutaj możesz również wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. W lewej części strony kartach stron innej konfiguracji może być otwarty.

![Strona usługi App Service w witrynie Azure Portal](./media/tutorial-docker-python-postgresql-app/app-mgmt.png)

## <a name="next-steps"></a>Następne kroki

Przejdź do następnego samouczek, aby dowiedzieć się, jak zamapować niestandardową nazwę DNS na aplikację sieci web.

> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure Web Apps (Mapowanie istniejącej niestandardowej nazwy DNS na aplikacje internetowe platformy Azure)](../app-service-web-tutorial-custom-domain.md)
