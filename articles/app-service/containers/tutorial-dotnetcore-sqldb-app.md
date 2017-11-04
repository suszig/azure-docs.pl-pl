---
title: "Tworzenie aplikacji sieci web platformy .NET Core i bazy danych SQL w usłudze Azure App Service w systemie Linux | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak pobrać aplikację .NET Core, pracy z połączenia z bazą danych SQL w usłudze Azure App Service w systemie Linux."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: syntaxc4
editor: 
ms.assetid: 0b4d7d0e-e984-49a1-a57a-3c0caa955f0e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: tutorial
ms.date: 10/10/2017
ms.author: cephalin
ms.custom: mvc
ms.openlocfilehash: ef68f64437935f08f76c29ecf15d574279cca7f1
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="build-a-net-core-and-sql-database-web-app-in-azure-app-service-on-linux"></a>Tworzenie aplikacji sieci web platformy .NET Core i bazy danych SQL w usłudze Azure App Service w systemie Linux

[Usługa aplikacji w systemie Linux](app-service-linux-intro.md) oferuje wysoce skalowalną, własnym poprawiania usługi hosta sieci web przy użyciu systemu operacyjnego Linux. W tym samouczku pokazano, jak utworzyć aplikację sieci web .NET Core i podłącz go do bazy danych SQL. Gdy wszystko będzie gotowe, będziesz mieć aplikację .NET Core MVC, działające w usłudze aplikacji w systemie Linux.

![aplikacji uruchomionej w usłudze App Service w systemie Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

Czego możesz dowiedzieć się, jak:

> [!div class="checklist"]
> * Tworzenie bazy danych SQL na platformie Azure
> * Połącz .NET Core aplikacji z bazą danych SQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników diagnostycznych z platformy Azure
> * Zarządzanie aplikacją w portalu Azure

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

1. [Zainstaluj oprogramowanie Git](https://git-scm.com/)
1. [Zainstaluj oprogramowanie .NET Core SDK 1.1.2](https://github.com/dotnet/core/blob/master/release-notes/download-archives/1.1.2-download.md)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="create-local-net-core-app"></a>Tworzenie lokalnych aplikacji .NET Core

Ten krok służy do konfigurowania lokalnego projektu platformy .NET Core.

### <a name="clone-the-sample-application"></a>Klonowanie przykładowej aplikacji

W oknie terminalu `cd` do katalogu roboczego.

Uruchom następujące polecenia w klonowania repozytorium przykładowej i zmień jego głównym.

```bash
git clone https://github.com/azure-samples/dotnetcore-sqldb-tutorial
cd dotnetcore-sqldb-tutorial
```

Przykładowy projekt zawiera podstawowe aplikacji CRUD (Tworzenie odczytu aktualizowania i usuwania), za pomocą [Entity Framework Core](https://docs.microsoft.com/ef/core/).

### <a name="run-the-application"></a>Uruchamianie aplikacji

Uruchom następujące polecenia, aby zainstalować wymagane pakiety, uruchom migracji bazy danych i uruchom aplikację.

```bash
dotnet restore
dotnet ef database update
dotnet run
```

W przeglądarce przejdź do adresu `http://localhost:5000`. Wybierz **Utwórz nowy** link i Utwórz kilka _zadań do wykonania_ elementów.

![pomyślnie nawiąże połączenie z bazą danych SQL](./media/tutorial-dotnetcore-sqldb-app/local-app-in-browser.png)

Aby zatrzymać .NET Core w dowolnym momencie, naciśnij klawisz `Ctrl+C` w terminalu.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-production-sql-database"></a>Utwórz produkcyjnej bazy danych SQL

W tym kroku utworzysz bazę danych SQL platformy Azure. Po wdrożeniu aplikacji na platformie Azure wykorzystuje tę bazę danych w chmurze.

Bazy danych SQL, w tym samouczku używana [bazy danych SQL Azure](/azure/sql-database/).

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)]

### <a name="create-a-sql-database-logical-server"></a>Tworzenie serwera logicznego bazy danych SQL

W powłoce chmury Tworzenie serwera logicznego bazy danych SQL z [az programu sql server Utwórz](/cli/azure/sql/server#create) polecenia.

Zastąp  *\<nazwa_serwera >* symbolu zastępczego o unikatowej nazwie bazy danych SQL. Ta nazwa jest używana jako część punktu końcowego bazy danych SQL `<server_name>.database.windows.net`, więc nazwa musi być unikatowa na wszystkich serwerach logiczne w systemie Azure. Nazwa musi zawierać tylko małe litery, cyfry i znaki łącznika (-) i musi mieć długość od 3 do 50 znaków. Ponadto Zastąp  *\<db_username >* i  *\<db_password >* przy użyciu nazwy użytkownika i hasła wybranych przez użytkownika. 


```azurecli-interactive
az sql server create --name <server_name> --resource-group myResourceGroup --location "West Europe" --admin-user <db_username> --admin-password <db_password>
```

Podczas tworzenia serwera logicznego bazy danych SQL Azure CLI zawiera informacje podobne do poniższego przykładu:

```json
{
  "administratorLogin": "sqladmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<server_name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Sql/servers/<server_name>",
  "identity": null,
  "kind": "v12.0",
  "location": "westeurope",
  "name": "<server_name>",
  "resourceGroup": "myResourceGroup",
  "state": "Ready",
  "tags": null,
  "type": "Microsoft.Sql/servers",
  "version": "12.0"
}
```

### <a name="configure-a-server-firewall-rule"></a>Konfigurowanie reguły zapory serwera

Utwórz [regułę zapory na poziomie serwera Azure SQL Database](../../sql-database/sql-database-firewall-configure.md) za pomocą polecenia [az sql server firewall create](/cli/azure/sql/server#create). Gdy IP początkowy i końcowy IP są wartość 0.0.0.0, Zapora tylko został otwarty do innych zasobów platformy Azure. 

```azurecli-interactive
az sql server firewall-rule create --resource-group myResourceGroup --server <server_name> --name AllowYourIp --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

### <a name="create-a-database"></a>Tworzenie bazy danych

Utwórz bazę danych [o poziomie wydajności S0](../../sql-database/sql-database-service-tiers.md) na serwerze za pomocą polecenia [az sql db create](/cli/azure/sql/db#create).

```azurecli-interactive
az sql db create --resource-group myResourceGroup --server <server_name> --name coreDB --service-objective S0
```

### <a name="create-connection-string"></a>Tworzenie parametrów połączenia

Zastąp następujący ciąg z  *\<nazwa_serwera >*,  *\<db_username >*, i  *\<db_password >* możesz używana wcześniej.

```
Server=tcp:<server_name>.database.windows.net,1433;Initial Catalog=coreDB;Persist Security Info=False;User ID=<db_username>;Password=<db_password>;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

Jest to ciąg połączenia dla aplikacji .NET Core. Należy skopiować go do użycia później.

## <a name="deploy-app-to-azure"></a>Wdrażanie aplikacji na platformie Azure

W tym kroku w przypadku wdrażania aplikacji połączonej bazy danych SQL platformy .NET Core App Service w systemie Linux.

### <a name="configure-local-git-deployment"></a>Konfigurowanie lokalnego wdrożenia narzędzia Git

[!INCLUDE [Configure a deployment user](../../../includes/configure-deployment-user-no-h.md)]

### <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-a-web-app"></a>Tworzenie aplikacji sieci Web

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-dotnetcore-no-h.md)] 

### <a name="configure-an-environment-variable"></a>Skonfigurować zmienną środowiskową

Aby ustawić parametry połączenia dla aplikacji platformy Azure, użyj [zaktualizować appsettings konfiguracji aplikacji sieci Web az](/cli/azure/webapp/config/appsettings#update) polecenie w powłoce chmury. W poniższym poleceniu zastąp  *\<Nazwa aplikacji >*, jak również  *\<ciągu łączącym >* parametru utworzoną wcześniej parametrami połączenia.

```azurecli-interactive
az webapp config connection-string set --resource-group myResourceGroup --name <app name> --settings MyDbConnection='<connection_string>' --connection-string-type SQLServer
```

Następnie należy ustawić `ASPNETCORE_ENVIRONMENT` ustawienia aplikacji na _produkcji_. To ustawienie umożliwia wiedzieć, czy jest uruchomiona na platformie Azure, ponieważ używają SQLLite dla bazy danych SQL i lokalne Środowisko deweloperskie do środowiska Azure.

Poniższy przykład konfiguruje `ASPNETCORE_ENVIRONMENT` ustawienie aplikacji w aplikacji sieci web platformy Azure. Zastąp  *\<nazwa_aplikacji >* symbolu zastępczego.

```azurecli-interactive
az webapp config appsettings set --name <app_name> --resource-group myResourceGroup --settings ASPNETCORE_ENVIRONMENT="Production"
```

### <a name="connect-to-sql-database-in-production"></a>Połącz z bazą danych SQL w środowisku produkcyjnym

W lokalnym repozytorium Otwórz Startup.cs i znajdź następujący kod:

```csharp
services.AddDbContext<MyDatabaseContext>(options =>
        options.UseSqlite("Data Source=localdatabase.db"));
```

Zastąp go następującym kodem, który używa zmiennych środowiskowych, które zostało wcześniej skonfigurowane.

```csharp
// Use SQL Database if in Azure, otherwise, use SQLLite
if(Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") == "Production")
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlServer(Configuration.GetConnectionString("MyDbConnection")));
else
    services.AddDbContext<DotNetCoreSqlDbContext>(options =>
            options.UseSqlite("Data Source=MvcMovie.db"));

// Automatically perform database migration
services.BuildServiceProvider().GetService<DotNetCoreSqlDbContext>().Database.Migrate();
```

W przypadku wykrycia przez ten kod jest uruchomiony w środowisku produkcyjnym (co oznacza środowiska platformy Azure), a następnie używa parametrów połączenia, należy skonfigurować do nawiązania połączenia z bazą danych SQL.

`Database.Migrate()` Wywołania pomaga uruchamianych na platformie Azure, ponieważ spowoduje to automatyczne utworzenie bazy danych który potrzeb aplikacji .NET Core na podstawie konfiguracji migracji. 

Zapisz zmiany.

### <a name="push-to-azure-from-git"></a>Wypychanie z narzędzia Git na platformę Azure

[!INCLUDE [app-service-plan-no-h](../../../includes/app-service-web-git-push-to-azure-no-h.md)]

```bash
Counting objects: 98, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (92/92), done.
Writing objects: 100% (98/98), 524.98 KiB | 5.58 MiB/s, done.
Total 98 (delta 8), reused 0 (delta 0)
remote: Updating branch 'master'.
remote: .
remote: Updating submodules.
remote: Preparing deployment for commit id '0c497633b8'.
remote: Generating deployment script.
remote: Project file path: ./DotNetCoreSqlDb.csproj
remote: Generated deployment script files
remote: Running deployment command...
remote: Handling ASP.NET Core Web Application deployment.
remote: .
remote: .
remote: .
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
remote: App container will begin restart within 10 seconds.
To https://<app_name>.scm.azurewebsites.net/<app_name>.git
 * [new branch]      master -> master
```

### <a name="browse-to-the-azure-web-app"></a>Przejdź do aplikacji sieci web platformy Azure

Przejdź do wdrożonej aplikacji sieci web za pomocą przeglądarki sieci web.

```bash
http://<app_name>.azurewebsites.net
```

Dodaj kilka elementów do wykonania.

![aplikacji uruchomionej w usłudze App Service w systemie Linux](./media/tutorial-dotnetcore-sqldb-app/azure-app-in-browser.png)

**Gratulacje!** Używasz aplikacji .NET Core sieci opartych na danych w usłudze App Service w systemie Linux.

## <a name="update-locally-and-redeploy"></a>Zaktualizuj lokalnie i Wdróż ponownie

W tym kroku możesz wprowadzić zmianę do schematu bazy danych i opublikowania go w usłudze Azure.

### <a name="update-your-data-model"></a>Aktualizacja modelu danych

Otwórz _Models\Todo.cs_ w edytorze kodu. Dodaj następujące właściwości do `ToDo` klasy:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Uruchom lokalnie migracje Code First

Uruchomienie kilku poleceń, aby aktualizacje z lokalną bazą danych.

```bash
dotnet ef migrations add AddProperty
```

Aktualizacja lokalnej bazy danych:

```bash
dotnet ef database update
```

### <a name="use-the-new-property"></a>Użyj nowej właściwości

Niektóre zmiany w kodzie do użycia `Done` właściwości. Dla uproszczenia w tym samouczku, tylko zamierzasz zmienić `Index` i `Create` widoków, aby wyświetlić właściwości działania.

Otwórz _Controllers\TodosController.cs_.

Znajdź `Create()` — metoda i Dodaj `Done` do listy właściwości w `Bind` atrybutu. Gdy wszystko będzie gotowe, Twoje `Create()` podpis metody wygląda podobnie do następującego kodu:

```csharp
public async Task<IActionResult> Create([Bind("ID,Description,CreatedDate,Done")] Todo todo)
```

Otwórz _Views\Todos\Create.cshtml_.

W kodzie Razor, powinna zostać wyświetlona `<div class="form-group">` elementu `Description`, a następnie inne `<div class="form-group">` elementu `CreatedDate`. Bezpośrednio po tych dwóch elementów, dodać kolejne `<div class="form-group">` elementu `Done`:

```csharp
<div class="form-group">
    <label asp-for="Done" class="col-md-2 control-label"></label>
    <div class="col-md-10">
        <input asp-for="Done" class="form-control" />
        <span asp-validation-for="Done" class="text-danger"></span>
    </div>
</div>
```

Otwórz _Views\Todos\Index.cshtml_.

Poszukaj pustych `<th></th>` elementu. Powyżej tego elementu Dodaj następujący kod Razor:

```csharp
<th>
    @Html.DisplayNameFor(model => model.Done)
</th>
```

Znajdź `<td>` element, który zawiera `asp-action` pomocników tagów. Powyżej tego elementu Dodaj następujący kod Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.CreatedDate)
</td>
```

To już wszystko, czego potrzebujesz, aby zobaczyć zmiany w `Index` i `Create` widoków.

### <a name="test-your-changes-locally"></a>Przetestuj zmiany lokalnie

Uruchamianie aplikacji lokalnie.

```bash
dotnet run
```

W przeglądarce przejdź do `http://localhost:5000/`. Można teraz dodać element do wykonania i sprawdzić **gotowe**. Następnie go powinny być widoczne w strony głównej jako element ukończone. Należy pamiętać, że `Edit` nie wyświetla widok `Done` pól, ponieważ nie zmieniły `Edit` widoku.

### <a name="publish-changes-to-azure"></a>Publikowanie zmian na platformie Azure

```bash

git commit -am "added done field"
git push azure master
```

Raz `git push` jest zakończenie, przejdź do aplikacji sieci web platformy Azure i wypróbowanie nowych funkcji.

![Aplikacja sieci web platformy Azure po zakończeniu migracji pierwszy kodu](./media/tutorial-dotnetcore-sqldb-app/this-one-is-done.png)

Nadal wyświetlane są wszystkie istniejące elementy zadań do wykonania. Istniejące dane w bazie danych SQL można ponownie opublikować aplikację .NET Core, nie zostaną utracone. Ponadto Entity Framework Core migracje tylko zmiany schematu danych oraz pozostawia niezmienione istniejących danych.

## <a name="manage-your-azure-web-app"></a>Zarządzanie aplikacji sieci web platformy Azure

Przejdź do [portalu Azure](https://portal.azure.com) zobaczyć aplikacji sieci web został utworzony.

W lewym menu kliknij pozycję **App Services**, a następnie kliknij nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/tutorial-dotnetcore-sqldb-app/access-portal.png)

Domyślnie portalu zawiera aplikację sieci web **omówienie** strony. Ta strona udostępnia widok sposobu działania aplikacji. Tutaj możesz również wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. W lewej części strony kartach stron innej konfiguracji może być otwarty.

![Strona usługi App Service w witrynie Azure Portal](./media/tutorial-dotnetcore-sqldb-app/web-app-blade.png)

[!INCLUDE [cli-samples-clean-up](../../../includes/cli-samples-clean-up.md)]

<a name="next"></a>
## <a name="next-steps"></a>Następne kroki

Wiadomości:

> [!div class="checklist"]
> * Tworzenie bazy danych SQL na platformie Azure
> * Połącz .NET Core aplikacji z bazą danych SQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników z platformy Azure na terminalu
> * Zarządzanie aplikacją w portalu Azure

Przejdź do następnego samouczek, aby dowiedzieć się, jak zamapować niestandardową nazwę DNS na aplikację sieci web.

> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure Web Apps (Mapowanie istniejącej niestandardowej nazwy DNS na aplikacje internetowe platformy Azure)](../app-service-web-tutorial-custom-domain.md)