---
title: "Tworzenie aplikacji platformy ASP.NET na platformie Azure w usłudze SQL Database | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak pobrać aplikację ASP.NET, działa na platformie Azure w ramach połączenia z bazą danych SQL."
services: app-service\web
documentationcenter: nodejs
author: cephalin
manager: erikre
editor: 
ms.assetid: 03c584f1-a93c-4e3d-ac1b-c82b50c75d3e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 06/09/2017
ms.author: cephalin
ms.custom: mvc, devcenter
ms.openlocfilehash: db3be8068ef9e560614daa0e7f0dcf62467fd338
ms.sourcegitcommit: 3e3a5e01a5629e017de2289a6abebbb798cec736
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/27/2017
---
# <a name="build-an-aspnet-app-in-azure-with-sql-database"></a>Tworzenie aplikacji platformy ASP.NET na platformie Azure z bazy danych SQL

Usługa [Azure Web Apps](app-service-web-overview.md) oferuje wysoce skalowalną i samonaprawialną usługę hostowaną w Internecie. Ten samouczek pokazuje, jak wdrożyć aplikację sieci web platformy ASP.NET opartych na danych na platformie Azure i połącz go z [bazy danych SQL Azure](../sql-database/sql-database-technical-overview.md). Po zakończeniu, aplikacja ASP.NET działających na platformie Azure i połączone z bazą danych SQL.

![Opublikowana aplikacja ASP.NET w aplikacji sieci web platformy Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie bazy danych SQL na platformie Azure
> * Połącz aplikację ASP.NET do bazy danych SQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników z platformy Azure na terminalu
> * Zarządzanie aplikacją w portalu Azure

## <a name="prerequisites"></a>Wymagania wstępne

W celu ukończenia tego samouczka:

* Zainstaluj program [Visual Studio 2017](https://www.visualstudio.com/downloads/) z następującymi pakietami roboczymi:
  - **Tworzenie aplikacji na platformie ASP.NET i tworzenie aplikacji internetowych**
  - **Tworzenie aplikacji na platformie Azure**

  ![Tworzenie aplikacji na platformie ASP.NET i tworzenie aplikacji internetowych oraz tworzenie aplikacji na platformie Azure (w ramach Internetu i chmury)](media/app-service-web-tutorial-dotnet-sqldatabase/workloads.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Pobierz przykład

[Pobierz przykładowy projekt](https://github.com/Azure-Samples/dotnet-sqldb-tutorial/archive/master.zip).

Wyodrębnij (Rozpakuj) *dotnet-sqldb — samouczek master.zip* pliku.

Przykładowy projekt zawiera basic [ASP.NET MVC](https://www.asp.net/mvc) CRUD (Tworzenie odczytu aktualizowania i usuwania) aplikacji w usłudze [Entity Framework Code First](/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="run-the-app"></a>Uruchomienie aplikacji

Otwórz *dotnet-sqldb — samouczek — wzorzec/DotNetAppSqlDb.sln* pliku w programie Visual Studio. 

Typ `Ctrl+F5` do uruchomienia aplikacji bez debugowania. Aplikacja jest wyświetlana w domyślnej przeglądarce. Wybierz **Utwórz nowy** link i Utwórz kilka *zadań do wykonania* elementów. 

![Okno dialogowe Nowy projekt ASP.NET](media/app-service-web-tutorial-dotnet-sqldatabase/local-app-in-browser.png)

Test **Edytuj**, **szczegóły**, i **usunąć** łącza.

Aplikacja używa kontekstu bazy danych do połączenia z bazą danych. W tym przykładzie kontekst bazy danych używa parametrów połączenia o nazwie `MyDbConnection`. Ustawiono parametrów połączenia *Web.config* plików i do którego odwołuje się *Models/MyDatabaseContext.cs* pliku. Nazwa ciągu połączenia jest używany w dalszej części samouczka Aby połączyć aplikację sieci web platformy Azure z bazy danych SQL Azure. 

## <a name="publish-to-azure-with-sql-database"></a>Publikowanie na platformie Azure z bazy danych SQL

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy użytkownika **DotNetAppSqlDb** projekt i wybierz **publikowania**.

![Publikowanie z Eksploratora rozwiązań](./media/app-service-web-tutorial-dotnet-sqldatabase/solution-explorer-publish.png)

Upewnij się, że jest zaznaczona usługa **Microsoft Azure App Service**, a następnie kliknij przycisk **Publikuj**.

![Publikowanie ze strony przeglądu projektu](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-to-app-service.png)

Publikowanie otwiera **Tworzenie usługi App Service** okno dialogowe, które ułatwia tworzenie zasobów Azure należy uruchomić aplikację sieci web programu ASP.NET na platformie Azure.

### <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

W oknie dialogowym **Tworzenie usługi App Service** kliknij pozycję **Dodaj konto**, a następnie zaloguj się do swojej subskrypcji platformy Azure. Jeśli zalogowano się już do konta Microsoft, upewnij się, że to konto zawiera Twoją subskrypcję platformy Azure. Jeśli użyte do logowania konto Microsoft nie ma subskrypcji platformy Azure, kliknij je, aby dodać prawidłowe konto.
   
![Logowanie do platformy Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/sign-in-azure.png)

Po zalogowaniu możesz w tym oknie dialogowym utworzyć wszystkie zasoby potrzebne dla aplikacji sieci Web platformy Azure.

### <a name="configure-the-web-app-name"></a>Konfigurowanie nazwy aplikacji sieci web

Zachowaj nazwę aplikacji sieci web wygenerowany lub zmień ją na inną nazwę unikatową (prawidłowe znaki to `a-z`, `0-9`, i `-`). Nazwa aplikacji sieci web jest używana jako część domyślnego adresu URL dla aplikacji (`<app_name>.azurewebsites.net`, gdzie `<app_name>` oznacza nazwę aplikacji sieci web). Nazwa aplikacji sieci web musi być unikatowy w obrębie wszystkich aplikacji w usłudze Azure. 

![Tworzenie aplikacji usługi z okna dialogowego](media/app-service-web-tutorial-dotnet-sqldatabase/wan.png)

> [!NOTE]
> Nie klikaj pozycji **Utwórz**. Należy najpierw skonfigurować bazy danych SQL w późniejszym kroku.

### <a name="create-a-resource-group"></a>Tworzenie grupy zasobów

[!INCLUDE [resource-group](../../includes/resource-group.md)]

Obok pozycji **Grupa zasobów** kliknij przycisk **Nowa**.

![Obok grupy zasobów kliknij przycisk Nowy.](media/app-service-web-tutorial-dotnet-sqldatabase/new_rg2.png)

Określ nazwę grupy zasobów **myResourceGroup**.

### <a name="create-an-app-service-plan"></a>Tworzenie planu usługi App Service

[!INCLUDE [app-service-plan](../../includes/app-service-plan.md)]

Obok pozycji **Plan usługi App Service** kliknij przycisk **Nowy**. 

W oknie dialogowym **Konfiguruj plan usługi App Service** skonfiguruj nowy plan usługi App Service przy użyciu następujących ustawień:

![Tworzenie planu usługi App Service](./media/app-service-web-tutorial-dotnet-sqldatabase/configure-app-service-plan.png)

| Ustawienie  | Sugerowana wartość | Aby uzyskać więcej informacji |
| ----------------- | ------------ | ----|
|**Plan usługi aplikacji**| myAppServicePlan | [Plany usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) |
|**Lokalizacja**| Europa Zachodnia | [Regiony platformy Azure](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) |
|**Rozmiar**| Bezpłatna | [Warstwa cenowa](https://azure.microsoft.com/pricing/details/app-service/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)|

### <a name="create-a-sql-server-instance"></a>Utwórz wystąpienie programu SQL Server

Przed utworzeniem bazy danych, należy [serwera logicznego bazy danych SQL Azure](../sql-database/sql-database-features.md). Serwer logiczny zawiera grupę baz danych zarządzanych jako grupa.

Wybierz **Eksploruj dodatkowych usług Azure**.

![Konfigurowanie nazwy aplikacji sieci Web](media/app-service-web-tutorial-dotnet-sqldatabase/web-app-name.png)

W **usług** , kliknij pozycję  **+**  obok opcji **bazy danych SQL**. 

![Na karcie usługi kliknij pozycję + ikona obok bazy danych SQL.](media/app-service-web-tutorial-dotnet-sqldatabase/sql.png)

W **Konfigurowanie bazy danych SQL** okna dialogowego, kliknij przycisk **nowy** obok **programu SQL Server**. 

Generowany jest unikatową nazwą serwera. Ta nazwa jest używana jako część domyślnego adresu URL dla serwera logicznego, `<server_name>.database.windows.net`. Musi być unikatowa we wszystkich wystąpieniach serwera logicznego w systemie Azure. Możesz zmienić nazwę serwera, ale w tym samouczku, należy zachować wartość wygenerowany.

Dodaj użytkownika administratora i hasła. Wymagania dotyczące złożoności hasła, aby zapoznać [zasady haseł](/sql/relational-databases/security/password-policy).

Należy pamiętać, ta nazwa użytkownika i hasło. Należy je później zarządzać wystąpienia serwera logicznego.

![Utwórz wystąpienie programu SQL Server](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database-server.png)

Kliknij przycisk **OK**. Nie zamykaj **Konfigurowanie bazy danych SQL** jeszcze okna dialogowego.

### <a name="create-a-sql-database"></a>Tworzenie bazy danych SQL

W **Konfigurowanie bazy danych SQL** okna dialogowego: 

* Zachowaj domyślne wygenerowany **Nazwa bazy danych**.
* W **Nazwa ciągu połączenia**, typ *MyDbConnection*. Ta nazwa musi być zgodna parametry połączenia, które odwołują się *Models/MyDatabaseContext.cs*.
* Kliknij przycisk **OK**.

![Skonfiguruj bazę danych SQL](media/app-service-web-tutorial-dotnet-sqldatabase/configure-sql-database.png)

**Tworzenie usługi App Service** okno dialogowe zawiera zasoby po utworzeniu. Kliknij przycisk **Utwórz**. 

![zasoby, które zostały utworzone](media/app-service-web-tutorial-dotnet-sqldatabase/app_svc_plan_done.png)

Po zakończeniu pracy Kreatora tworzenia zasobów platformy Azure, publikowanie aplikacji ASP.NET na platformie Azure. Domyślnej przeglądarki jest uruchamiana z adresem URL do wdrożonej aplikacji. 

Dodaj kilka elementów do wykonania.

![Opublikowana aplikacja ASP.NET w aplikacji sieci web platformy Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/azure-app-in-browser.png)

Gratulacje! Aplikacja ASP.NET opartych na danych jest uruchomiona na żywo w usłudze Azure App Service.

## <a name="access-the-sql-database-locally"></a>Dostęp do bazy danych SQL lokalnie

Visual Studio umożliwia Eksplorowanie i nowej bazy danych SQL w prosty sposób zarządzać **Eksplorator obiektów SQL Server**.

### <a name="create-a-database-connection"></a>Utwórz połączenie z bazą danych

Z **widoku** menu, wybierz opcję **Eksplorator obiektów SQL Server**.

W górnej części **Eksplorator obiektów SQL Server**, kliknij przycisk **dodawania serwera SQL** przycisku.

### <a name="configure-the-database-connection"></a>Skonfiguruj połączenie bazy danych

W **Connect** okna dialogowego, rozwiń węzeł **Azure** węzła. Swoich wystąpień bazy danych SQL platformy Azure są wyświetlane tutaj.

Wybierz bazy danych SQL, który został utworzony wcześniej. Połączenie utworzony wcześniej jest automatycznie wypełniane u dołu.

Wpisz hasło administratora bazy danych utworzone wcześniej, a następnie kliknij przycisk **Connect**.

![Skonfiguruj połączenie bazy danych z programu Visual Studio](./media/app-service-web-tutorial-dotnet-sqldatabase/connect-to-sql-database.png)

### <a name="allow-client-connection-from-your-computer"></a>Zezwalaj na połączenia klienckie z komputera

**Utwórz nową regułę zapory** otworzyć okna dialogowego. Domyślnie wystąpienia bazy danych SQL umożliwia tylko połączeń z usługami Azure, takich jak aplikacji sieci web platformy Azure. Aby połączyć się z bazą danych, należy utworzyć regułę zapory w wystąpieniu bazy danych SQL. Reguła zapory zezwala na publiczny adres IP komputera lokalnego.

Okno dialogowe jest już wypełniony publiczny adres IP tego komputera.

Upewnij się, że **Dodaj mój adres IP klienta** jest zaznaczone, a następnie kliknij przycisk **OK**. 

![Ustaw zapory dla wystąpienia bazy danych SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/sql-set-firewall.png)

Po zakończeniu tworzenia ustawień zapory dla swojego wystąpienia bazy danych SQL programu Visual Studio połączenia zostaną wyświetlone w **Eksplorator obiektów SQL Server**.

W tym miejscu można wykonywać najbardziej typowe operacje bazy danych, takie jak wykonywania zapytania, Utwórz widoki i procedury składowane i inne. 

Rozwiń węzeł połączenia > **baz danych** > **&lt;bazy danych >** > **tabel**. Kliknij prawym przyciskiem myszy `Todoes` tabeli i wybierz **danych widoku**. 

![Eksploruj obiektów bazy danych SQL](./media/app-service-web-tutorial-dotnet-sqldatabase/explore-sql-database.png)

## <a name="update-app-with-code-first-migrations"></a>Aktualizowanie aplikacji z migracje Code First

Znanych narzędzi programu Visual Studio umożliwia aktualizacji aplikacji sieci web i bazy danych na platformie Azure. W tym kroku użyjesz migracje Code First w Entity Framework zmiany do schematu bazy danych, a następnie opublikuj ją do platformy Azure.

Aby uzyskać więcej informacji o używaniu migracje Code First Framework jednostki, zobacz [wprowadzenie do programu Entity Framework 6 Code First przy użyciu MVC 5](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).

### <a name="update-your-data-model"></a>Aktualizacja modelu danych

Otwórz _Models\Todo.cs_ w edytorze kodu. Dodaj następujące właściwości do `ToDo` klasy:

```csharp
public bool Done { get; set; }
```

### <a name="run-code-first-migrations-locally"></a>Uruchom lokalnie migracje Code First

Uruchomienie kilku poleceń, aby aktualizacje z lokalną bazą danych. 

Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet** > **Konsola Menedżera pakietów**.

W oknie Konsola Menedżera pakietów należy włączyć migracje Code First:

```PowerShell
Enable-Migrations
```

Dodaj migracji:

```PowerShell
Add-Migration AddProperty
```

Aktualizacja lokalnej bazy danych:

```PowerShell
Update-Database
```

Typ `Ctrl+F5` do uruchomienia aplikacji. Testowanie edycji, uzyskać szczegółowe informacje i utworzyć łącza.

Jeśli bez błędów ładowania aplikacji powiodła się migracje Code First. Jednak ze strony nadal jest taki sam ponieważ logiki aplikacji nie korzysta z tej nowej właściwości jeszcze. 

### <a name="use-the-new-property"></a>Użyj nowej właściwości

Niektóre zmiany w kodzie do użycia `Done` właściwości. Dla uproszczenia w tym samouczku, tylko zamierzasz zmienić `Index` i `Create` widoków, aby wyświetlić właściwości działania.

Otwórz _Controllers\TodosController.cs_.

Znajdź `Create()` i metody w wierszu 52 dodać `Done` do listy właściwości w `Bind` atrybutu. Gdy wszystko będzie gotowe, Twoje `Create()` podpis metody wygląda podobnie do następującego kodu:

```csharp
public ActionResult Create([Bind(Include = "Description,CreatedDate,Done")] Todo todo)
```

Otwórz _Views\Todos\Create.cshtml_.

W kodzie Razor, powinna zostać wyświetlona `<div class="form-group">` element, który używa `model.Description`, a następnie inne `<div class="form-group">` element, który używa `model.CreatedDate`. Bezpośrednio po tych dwóch elementów, dodać kolejne `<div class="form-group">` element, który używa `model.Done`:

```csharp
<div class="form-group">
    @Html.LabelFor(model => model.Done, htmlAttributes: new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        <div class="checkbox">
            @Html.EditorFor(model => model.Done)
            @Html.ValidationMessageFor(model => model.Done, "", new { @class = "text-danger" })
        </div>
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

Znajdź `<td>` element, który zawiera `Html.ActionLink()` metody pomocnicze. _Powyżej_ to `<td>`, dodać kolejne `<td>` element z następującym kodem Razor:

```csharp
<td>
    @Html.DisplayFor(modelItem => item.Done)
</td>
```

To już wszystko, czego potrzebujesz, aby zobaczyć zmiany w `Index` i `Create` widoków. 

Typ `Ctrl+F5` do uruchomienia aplikacji.

Można teraz dodać element do wykonania i sprawdzić **gotowe**. Następnie go powinny być widoczne w strony głównej jako element ukończone. Należy pamiętać, że `Edit` nie wyświetla widok `Done` pól, ponieważ nie zmieniły `Edit` widoku.

### <a name="enable-code-first-migrations-in-azure"></a>Włącz migracje Code First na platformie Azure

Teraz, gdy kod działa, łącznie z migracji bazy danych zmian przed opublikowaniem aplikacji sieci web platformy Azure i zbyt aktualizacji bazy danych SQL z migracje Code First.

Tak jak wcześniej, kliknij prawym przyciskiem myszy projekt i wybierz **publikowania**.

Kliknij przycisk **ustawienia** aby otworzyć Kreatora publikowania.

![Otwórz ustawienia publikowania](./media/app-service-web-tutorial-dotnet-sqldatabase/publish-settings.png)

W kreatorze kliknij **dalej**.

Upewnij się, że parametry połączenia bazy danych SQL jest wypełniana w **MyDatabaseContext (MyDbConnection)**. Musisz wybrać **myToDoAppDb** bazy danych z listy rozwijanej. 

Wybierz **wykonaj migracje Code First (wywoływane po uruchomieniu aplikacji)**, następnie kliknij przycisk **zapisać**.

![Włącz migracje Code First w aplikacji sieci web platformy Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/enable-migrations.png)

### <a name="publish-your-changes"></a>Opublikuj zmiany

Skoro migracje Code First jest włączone w aplikacji sieci web platformy Azure, Opublikuj zmiany kodu.

Na stronie publikowania kliknij przycisk **Publikuj**.

Spróbuj ponownie dodać elementów do wykonania i wybierz **gotowe**, i powinny być widoczne w strony głównej jako element ukończone.

![Aplikacja sieci web platformy Azure po zakończeniu migracji pierwszy kodu](./media/app-service-web-tutorial-dotnet-sqldatabase/this-one-is-done.png)

Nadal wyświetlane są wszystkie istniejące elementy zadań do wykonania. Istniejące dane w bazie danych SQL ponownie opublikować aplikację ASP.NET nie zostaną utracone. Ponadto migracje Code First tylko zmiany schematu danych oraz pozostawia niezmienione istniejących danych.


## <a name="stream-application-logs"></a>Strumieniowe przesyłanie dzienników aplikacji

Wiadomości śledzenia można strumienia bezpośrednio z aplikacji sieci web platformy Azure dla programu Visual Studio.

Otwórz _Controllers\TodosController.cs_.

Każda akcja rozpoczyna się od `Trace.WriteLine()` metody. Ten kod jest dodawany do pokazano, jak dodawać komunikaty śledzenia do aplikacji sieci web platformy Azure.

### <a name="open-server-explorer"></a>W Eksploratorze serwera Otwórz

Z **widoku** menu, wybierz opcję **Eksploratora serwera**. Można skonfigurować rejestrowanie dla aplikacji sieci web platformy Azure na **Eksploratora serwera**. 

### <a name="enable-log-streaming"></a>Przesyłania strumieniowego dzienników

W **Eksploratora serwera**, rozwiń węzeł **Azure** > **usługi aplikacji**.

Rozwiń węzeł **myResourceGroup** grupy zasobów utworzone podczas tworzenia aplikacji sieci web platformy Azure.

Kliknij prawym przyciskiem myszy aplikację sieci web platformy Azure i wybierz **Wyświetl dzienniki przesyłania strumieniowego**.

![Przesyłania strumieniowego dzienników](./media/app-service-web-tutorial-dotnet-sqldatabase/stream-logs.png)

Dzienniki są teraz przesyłane strumieniowo do **dane wyjściowe** okna. 

![Dziennik przesyłania strumieniowego w oknie danych wyjściowych](./media/app-service-web-tutorial-dotnet-sqldatabase/log-streaming-pane.png)

Jednak nie widzisz żadnych komunikatów śledzenia jeszcze. To jest ponieważ po wybraniu **Wyświetl dzienniki przesyłania strumieniowego**, aplikacji sieci web platformy Azure ustawia poziom śledzenia `Error`, która rejestruje tylko zdarzenia błędów (z `Trace.TraceError()` metody).

### <a name="change-trace-levels"></a>Zmień poziomy śledzenia

Aby zmienić poziom śledzenia do wyjściowego inne komunikaty śledzenia, przejdź wstecz do **Eksploratora serwera**.

Ponownie kliknij prawym przyciskiem myszy aplikację sieci web platformy Azure i wybierz **ustawienia widoku**.

W **rejestrowania aplikacji (w systemie plików)** listy rozwijanej wybierz **pełne**. Kliknij pozycję **Zapisz**.

![Poziom śledzenia zmian Verbose](./media/app-service-web-tutorial-dotnet-sqldatabase/trace-level-verbose.png)

> [!TIP]
> Możesz eksperymentować z poziomów śledzenia różnych, aby zobaczyć, jakie komunikaty są wyświetlane dla każdego poziomu. Na przykład **informacji** poziom obejmuje wszystkie dzienniki utworzone przez `Trace.TraceInformation()`, `Trace.TraceWarning()`, i `Trace.TraceError()`, ale nie Dzienniki tworzone przez `Trace.WriteLine()`.
>
>

W przeglądarce przejdź do aplikacji sieci web w ponownie *http://&lt;nazwę aplikacji >. azurewebsites.net*, spróbuj kliknięcie wokół aplikacji listy zadań do wykonania na platformie Azure. Wiadomości śledzenia są teraz przesyłane strumieniowo do **dane wyjściowe** okna w programie Visual Studio.

```console
Application: 2017-04-06T23:30:41  PID[8132] Verbose     GET /Todos/Index
Application: 2017-04-06T23:30:43  PID[8132] Verbose     GET /Todos/Create
Application: 2017-04-06T23:30:53  PID[8132] Verbose     POST /Todos/Create
Application: 2017-04-06T23:30:54  PID[8132] Verbose     GET /Todos/Index
```



### <a name="stop-log-streaming"></a>Zatrzymaj dzienników przesyłania strumieniowego

Aby zatrzymać usługę przesyłania strumieniowego dzienników, kliknij przycisk **zatrzymać monitorowanie** przycisk **dane wyjściowe** okna.

![Zatrzymaj dzienników przesyłania strumieniowego](./media/app-service-web-tutorial-dotnet-sqldatabase/stop-streaming.png)

## <a name="manage-your-azure-web-app"></a>Zarządzanie aplikacji sieci web platformy Azure

Przejdź do [portalu Azure](https://portal.azure.com) zobaczyć aplikacji sieci web został utworzony. 



W lewym menu kliknij pozycję **App Service**, a następnie kliknij nazwę swojej aplikacji sieci Web platformy Azure.

![Nawigacja w portalu do aplikacji sieci Web platformy Azure](./media/app-service-web-tutorial-dotnet-sqldatabase/access-portal.png)

Jest strony aplikacji sieci web. 

Domyślnie przedstawia portalu **omówienie** strony. Ta strona udostępnia widok sposobu działania aplikacji. Tutaj możesz również wykonywać podstawowe zadania zarządzania, takie jak przeglądanie, zatrzymywanie, uruchamianie, ponowne uruchamianie i usuwanie. W lewej części strony kartach stron innej konfiguracji może być otwarty. 

![Strona usługi App Service w witrynie Azure Portal](./media/app-service-web-tutorial-dotnet-sqldatabase/web-app-blade.png)

[!INCLUDE [Clean up section](../../includes/clean-up-section-portal-web-app.md)]

<a name="next"></a>

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie bazy danych SQL na platformie Azure
> * Połącz aplikację ASP.NET do bazy danych SQL
> * Wdrażanie aplikacji na platformie Azure
> * Aktualizacja modelu danych i ponownie wdrożyć aplikację
> * Strumieniowe przesyłanie dzienników z platformy Azure na terminalu
> * Zarządzanie aplikacją w portalu Azure

Przejdź do następnego samouczkiem, aby dowiedzieć się, jak zamapować niestandardową nazwę DNS w aplikacji sieci web.

> [!div class="nextstepaction"]
> [Map an existing custom DNS name to Azure Web Apps (Mapowanie istniejącej niestandardowej nazwy DNS na aplikacje internetowe platformy Azure)](app-service-web-tutorial-custom-domain.md)
