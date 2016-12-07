---
title: "Tworzenie aplikacji sieci Web za pomocą pamięci podręcznej Redis | Microsoft Docs"
description: "Informacje na temat tworzenia aplikacji sieci Web za pomocą pamięci podręcznej Redis"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 454e23d7-a99b-4e6e-8dd7-156451d2da7c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: hero-article
ms.date: 10/11/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 425729dde12d8477b7b8ecd6394b31b7c5a5e92b


---
# <a name="how-to-create-a-web-app-with-redis-cache"></a>Tworzenie aplikacji sieci Web za pomocą pamięci podręcznej Redis
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

W tym samouczku przedstawiono tworzenie i wdrażanie aplikacji sieci Web programu ASP.NET do aplikacji sieci Web w usłudze Azure App Service przy użyciu programu Visual Studio 2015. Przykładowa aplikacja wyświetla listę statystyk zespołu z bazy danych i pokazuje różne sposoby korzystania z usługi Azure Redis Cache w celu przechowywania danych w pamięci podręcznej i pobierania ich. Wykonanie instrukcji z samouczka umożliwi uzyskanie działającej aplikacji sieci Web wykonującej odczyt i zapis w bazie danych, zoptymalizowanej przy użyciu usługi Azure Redis Cache i hostowanej na platformie Azure.

Dowiesz się:

* Jak utworzyć aplikację sieci Web ASP.NET MVC 5 w programie Visual Studio.
* Jak uzyskać dostęp do danych z bazy danych przy użyciu platformy Entity Framework.
* Jak zwiększyć przepływność danych i ograniczyć obciążenie bazy danych dzięki przechowywaniu i pobieraniu danych przy użyciu usługi Azure Redis Cache.
* Jak użyć sortowanego zestawu Redis do pobrania pierwszych 5 zespołów.
* Jak wykonać aprowizację zasobów platformy Azure dla aplikacji przy użyciu szablonu usługi Resource Manager.
* Jak opublikować aplikację na platformie Azure przy użyciu programu Visual Studio.

## <a name="prerequisites"></a>Wymagania wstępne
Do ukończenia tego samouczka niezbędne są następujące wstępnie wymagane elementy.

* [Konto platformy Azure](#azure-account)
* [Program Visual Studio 2015 z zestawem Azure SDK dla platformy .NET](#visual-studio-2015-with-the-azure-sdk-for-net)

### <a name="azure-account"></a>Konto platformy Azure
Do ukończenia tego samouczka jest potrzebne konto platformy Azure. Możesz:

* [Utworzyć bezpłatne konto platformy Azure ](/pricing/free-trial/?WT.mc_id=redis_cache_hero). Otrzymasz środki, które możesz wykorzystać do wypróbowania płatnych usług Azure. Nawet po wyczerpaniu tych środków możesz zachować konto i korzystać z bezpłatnych usług i funkcji platformy Azure.
* [Aktywować korzyści subskrybenta programu Visual Studio](/pricing/member-offers/msdn-benefits-details/?WT.mc_id=redis_cache_hero). W ramach subskrypcji MSDN co miesiąc otrzymasz środki, które możesz przeznaczyć na płatne usługi platformy Azure.

### <a name="visual-studio-2015-with-the-azure-sdk-for-net"></a>Program Visual Studio 2015 z zestawem Azure SDK dla platformy .NET
Samouczek jest przeznaczony dla programu Visual Studio 2015 z [zestawem Azure SDK dla platformy .NET](../dotnet-sdk.md) w wersji 2.8.2 lub nowszej. [Pobierz najnowszy zestaw Azure SDK dla programu Visual Studio 2015 tutaj](http://go.microsoft.com/fwlink/?linkid=518003). Program Visual Studio jest automatycznie instalowany z zestawem SDK, jeśli nie był on zainstalowany wcześniej.

Jeśli masz program Visual Studio 2013, możesz [pobrać najnowszy zestaw Azure SDK dla programu Visual Studio 2013](http://go.microsoft.com/fwlink/?LinkID=324322). Niektóre ekrany mogą różnić się od przedstawionych na ilustracjach w tym samouczku.

> [!NOTE]
> Zależnie od liczby składników zależnych zestawu SDK, które znajdują się już na komputerze, instalowanie zestawu SDK może trwać od kilku minut do pół godziny lub dłużej.
> 
> 

## <a name="create-the-visual-studio-project"></a>Tworzenie projektu programu Visual Studio
1. Otwórz program Visual Studio i kliknij kolejno opcje **Plik**, **Nowy**, **Projekt**.
2. Rozwiń węzeł **Visual C#** na liście **Szablony**, wybierz pozycję **Chmura** i kliknij pozycję **Aplikacja sieci Web ASP.NET**. Upewnij się, że została wybrana pozycja **.NET Framework 4.5.2**.  Wpisz **ContosoTeamStats** w polu tekstowym **Nazwa** i kliknij przycisk **OK**.
   
    ![Tworzenie projektu][cache-create-project]
3. Wybierz **MVC** jako typ projektu. Wyczyść pole wyboru **Hostuj w chmurze**. W kolejnych krokach samouczka wykonasz [aprowizację zasobów platformy Azure](#provision-the-azure-resources) i [opublikujesz aplikację na platformie Azure](#publish-the-application-to-azure) Przykład aprowizacji aplikacji sieci Web usługi App Service z programu Visual Studio przez pozostawienie zaznaczonego pola wyboru **Hostuj w chmurze** opisano w artykule [Wdrażanie aplikacji sieci Web programu ASP.NET w usłudze Azure App Service przy użyciu programu Visual Studio](../app-service-web/web-sites-dotnet-get-started.md).
   
    ![Wybieranie szablonu projektu][cache-select-template]
4. Kliknij przycisk **OK**, aby utworzyć projekt.

## <a name="create-the-aspnet-mvc-application"></a>Tworzenie aplikacji platformy ASP.NET MVC
W tej części samouczka opisano tworzenie podstawowej aplikacji, która odczytuje i wyświetla statystyki zespołu z bazy danych.

* [Dodawanie modelu](#add-the-model)
* [Dodawanie kontrolera](#add-the-controller)
* [Konfigurowanie widoków](#configure-the-views)

### <a name="add-the-model"></a>Dodawanie modelu
1. Kliknij prawym przyciskiem myszy pozycję **Modele** w **Eksploratorze rozwiązań**, a następnie wybierz kolejno pozycje **Dodaj**, **Klasa**. 
   
    ![Dodawanie modelu][cache-model-add-class]
2. Wprowadź `Team` dla nazwy klasy i kliknij przycisk **Dodaj**.
   
    ![Dodawanie klasy modelu][cache-model-add-class-dialog]
3. Zastąp instrukcje `using` na początku pliku `Team.cs` poniższymi instrukcjami using.

        using System;
        using System.Collections.Generic;
        using System.Data.Entity;
        using System.Data.Entity.SqlServer;


1. Zastąp definicję klasy `Team` poniższym fragmentem kodu, który zawiera zaktualizowaną definicję klasy `Team` oraz niektóre inne klasy pomocnika platformy Entity Framework. Więcej informacji na temat podejścia typu Code First na platformie Entity Framework używanego w tym samouczku znajduje się w artykule [Code first to a new database](https://msdn.microsoft.com/data/jj193542) (Rozwiązanie Code First dla nowej bazy danych).

        public class Team
        {
            public int ID { get; set; }
            public string Name { get; set; }
            public int Wins { get; set; }
            public int Losses { get; set; }
            public int Ties { get; set; }

            static public void PlayGames(IEnumerable<Team> teams)
            {
                // Simple random generation of statistics.
                Random r = new Random();

                foreach (var t in teams)
                {
                    t.Wins = r.Next(33);
                    t.Losses = r.Next(33);
                    t.Ties = r.Next(0, 5);
                }
            }
        }

        public class TeamContext : DbContext
        {
            public TeamContext()
                : base("TeamContext")
            {
            }

            public DbSet<Team> Teams { get; set; }
        }

        public class TeamInitializer : CreateDatabaseIfNotExists<TeamContext>
        {
            protected override void Seed(TeamContext context)
            {
                var teams = new List<Team>
                {
                    new Team{Name="Adventure Works Cycles"},
                    new Team{Name="Alpine Ski House"},
                    new Team{Name="Blue Yonder Airlines"},
                    new Team{Name="Coho Vineyard"},
                    new Team{Name="Contoso, Ltd."},
                    new Team{Name="Fabrikam, Inc."},
                    new Team{Name="Lucerne Publishing"},
                    new Team{Name="Northwind Traders"},
                    new Team{Name="Consolidated Messenger"},
                    new Team{Name="Fourth Coffee"},
                    new Team{Name="Graphic Design Institute"},
                    new Team{Name="Nod Publishers"}
                };

                Team.PlayGames(teams);

                teams.ForEach(t => context.Teams.Add(t));
                context.SaveChanges();
            }
        }

        public class TeamConfiguration : DbConfiguration
        {
            public TeamConfiguration()
            {
                SetExecutionStrategy("System.Data.SqlClient", () => new SqlAzureExecutionStrategy());
            }
        }


1. W **Eksploratorze rozwiązań** kliknij dwukrotnie plik **web.config**, aby go otworzyć.
   
    ![Web.config][cache-web-config]
2. Dodaj poniższe parametry połączenia do sekcji `connectionStrings`. Nazwa parametrów połączenia musi być zgodna z nazwą klasy kontekstu bazy danych platformy Entity Framework, którą jest `TeamContext`.
   
       <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True" providerName="System.Data.SqlClient" />

    Po dodaniu sekcja `connectionStrings` powinna wyglądać jak w poniższym przykładzie.


        <connectionStrings>
            <add name="DefaultConnection" connectionString="Data Source=(LocalDb)\MSSQLLocalDB;AttachDbFilename=|DataDirectory|\aspnet-ContosoTeamStats-20160216120918.mdf;Initial Catalog=aspnet-ContosoTeamStats-20160216120918;Integrated Security=True"
                providerName="System.Data.SqlClient" />
            <add name="TeamContext" connectionString="Data Source=(LocalDB)\v11.0;AttachDbFilename=|DataDirectory|\Teams.mdf;Integrated Security=True"     providerName="System.Data.SqlClient" />
        </connectionStrings>

### <a name="add-the-controller"></a>Dodawanie kontrolera
1. Naciśnij klawisz **F6**, aby utworzyć projekt. 
2. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy folder **Kontrolery** i wybierz kolejno pozycje **Dodaj**, **Kontroler**.
   
    ![Dodawanie kontrolera][cache-add-controller]
3. Wybierz pozycję **Kontroler MVC 5 z widokami używający narzędzia Entity Framework** i kliknij przycisk **Dodaj**. Jeśli po kliknięciu przycisku **Dodaj** występuje błąd, upewnij się, czy został wcześniej utworzony projekt.
   
    ![Dodawanie klasy kontrolera][cache-add-controller-class]
4. Wybierz pozycję **Zespół (ContosoTeamStats.Models)** z listy rozwijanej **Klasa modelu**. Wybierz pozycję **TeamContext (ContosoTeamStats.Models)** z listy rozwijanej **Klasa kontekstu danych**. Wpisz nazwę `TeamsController` w polu tekstowym **Kontroler** (jeśli nie została automatycznie wypełniona). Kliknij przycisk **Dodaj**, aby utworzyć klasę kontrolera i dodać widoki domyślne.
   
    ![Konfigurowanie kontrolera][cache-configure-controller]
5. W **Eksploratorze rozwiązań** rozwiń folder **Global.asax** i kliknij dwukrotnie plik **Global.asax.cs**, aby go otworzyć.
   
    ![Global.asax.cs][cache-global-asax]
6. Dodaj dwie poniższe instrukcje using w początkowej części pliku, pod innymi instrukcjami using.

        using System.Data.Entity;
        using ContosoTeamStats.Models;


1. Dodaj poniższy wiersz kodu na końcu metody `Application_Start`.

        Database.SetInitializer<TeamContext>(new TeamInitializer());


1. W **Eksploratorze rozwiązań** rozwiń folder `App_Start` i kliknij dwukrotnie pozycję `RouteConfig.cs`.
   
    ![RouteConfig.cs][cache-RouteConfig-cs]
2. Zastąp ciąg `controller = "Home"` w poniższym kodzie w metodzie `RegisterRoutes` ciągiem `controller = "Teams"`, jak pokazano w poniższym przykładzie.

        routes.MapRoute(
            name: "Default",
            url: "{controller}/{action}/{id}",
            defaults: new { controller = "Teams", action = "Index", id = UrlParameter.Optional }
        );


### <a name="configure-the-views"></a>Konfigurowanie widoków
1. W **Eksploratorze rozwiązań** rozwiń folder **Widoki**, a następnie folder **Współużytkowane**, a następnie kliknij dwukrotnie pozycję **_Layout.cshtml**. 
   
    ![_Layout.cshtml][cache-layout-cshtml]
2. Zmień zawartość elementu `title` i zastąp ciąg `My ASP.NET Application` ciągiem `Contoso Team Stats`, jak pokazano w poniższym przykładzie.

        <title>@ViewBag.Title - Contoso Team Stats</title>


1. W sekcji `body` zaktualizuj pierwszą instrukcję `Html.ActionLink` i zastąp parametr `Application name` ciągiem `Contoso Team Stats` oraz parametr `Home` parametrem `Teams`.
   
   * Przed: `@Html.ActionLink("Application name", "Index", "Home", new { area = "" }, new { @class = "navbar-brand" })`
   * Po: `@Html.ActionLink("Contoso Team Stats", "Index", "Teams", new { area = "" }, new { @class = "navbar-brand" })`
     
     ![Zmiany kodu][cache-layout-cshtml-code]
2. Naciśnij klawisze **Ctrl+F5**, aby utworzyć i uruchomić aplikację. Ta wersja aplikacji odczytuje wyniki bezpośrednio z bazy danych. Zwróć uwagę na akcje **Utwórz nowe**, **Edytuj**, **Szczegóły** oraz **Usuń**, które zostały automatycznie dodane do aplikacji przez szkielet **Kontroler MVC 5 z widokami używający narzędzia Entity Framework**. W następnej sekcji samouczka opisano dodawanie pamięci podręcznej Redis w celu optymalizacji dostępu do danych i zapewnienia dodatkowych funkcji w aplikacji.

![Aplikacja startowa][cache-starter-application]

## <a name="configure-the-application-to-use-redis-cache"></a>Konfigurowanie aplikacji do korzystania z pamięci podręcznej Redis
W tej sekcji samouczka opisano konfigurowanie przykładowej aplikacji do przechowywania i pobierania statystyk zespołu Contoso z wystąpienia usługi Azure Redis Cache za pomocą klienta pamięci podręcznej [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis).

* [Konfigurowanie aplikacji do korzystania z programu StackExchange.Redis](#configure-the-application-to-use-stackexchangeredis)
* [Aktualizacja klasy TeamsController w celu zwracania wyników z pamięci podręcznej lub bazy danych](#update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database)
* [Aktualizacja metod Utwórz, Edytuj i Usuń w celu pracy z pamięcią podręczną](#update-the-create-edit-and-delete-methods-to-work-with-the-cache)
* [Aktualizacja widoku Indeks zespołów w celu pracy z pamięcią podręczną](#update-the-teams-index-view-to-work-with-the-cache)

### <a name="configure-the-application-to-use-stackexchangeredis"></a>Konfigurowanie aplikacji do korzystania z programu StackExchange.Redis
1. Aby skonfigurować aplikację klienta w programie Visual Studio przy użyciu pakietu NuGet StackExchange.Redis, kliknij prawym przyciskiem myszy projekt w **Eksploratorze rozwiązań** i wybierz pozycję **Zarządzaj pakietami NuGet**. 
   
    ![Zarządzanie pakietami NuGet][redis-cache-manage-nuget-menu]
2. Wpisz **StackExchange.Redis** w polu tekstowym wyszukiwania, wybierz z wyników żądaną wersję i kliknij przycisk **Zainstaluj**.
   
    ![Pakiet NuGet StackExchange.Redis][redis-cache-stack-exchange-nuget]
   
    Pakiet NuGet pobiera i dodaje wymagane odwołania do zestawu umożliwiające aplikacji klienta uzyskanie dostępu do usługi Azure Redis Cache przy użyciu klienta pamięci podręcznej StackExchange.Redis. Jeśli wolisz użyć wersji biblioteki klienckiej **StackExchange.Redis** o silnej nazwie, wybierz pozycję **StackExchange.Redis.StrongName**; w innym wypadku wybierz pozycję **StackExchange.Redis**.
3. W **Eksploratorze rozwiązań** rozwiń folder **Kontrolery** i kliknij dwukrotnie plik **TeamsController.cs**, aby go otworzyć.
   
    ![Kontroler zespołów][cache-teamscontroller]
4. Dodaj dwie poniższe instrukcje using do pliku **TeamsController.cs**.
   
        using System.Configuration;
        using StackExchange.Redis;
5. Dodaj dwie poniższe właściwości do klasy `TeamsController`.
   
        // Redis Connection string info
        private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
        {
            string cacheConnection = ConfigurationManager.AppSettings["CacheConnection"].ToString();
            return ConnectionMultiplexer.Connect(cacheConnection);
        });
   
        public static ConnectionMultiplexer Connection
        {
            get
            {
                return lazyConnection.Value;
            }
        }
6. Utwórz na komputerze plik o nazwie `WebAppPlusCacheAppSecrets.config` i umieść go w lokalizacji, która nie zostanie zaewidencjonowana kodem źródłowym przykładowej aplikacji, na wypadek podjęcia decyzji o zaewidencjonowaniu jej w innym miejscu. W tym przykładzie plik `AppSettingsSecrets.config` znajduje się w folderze `C:\AppSecrets\WebAppPlusCacheAppSecrets.config`.
   
    Edytuj plik `WebAppPlusCacheAppSecrets.config` i dodaj następującą zawartość. Jeśli aplikacja jest uruchamiana lokalnie, informacje te są używane do łączenia z wystąpieniem usługi Azure Redis Cache. Później w samouczku wykonasz aprowizację wystąpienia usługi Azure Redis Cache i zaktualizujesz nazwę pamięci podręcznej i hasło. Jeśli nie planujesz uruchamiania przykładowej aplikacji lokalnie, możesz pominąć tworzenie tego pliku oraz następne kroki odnoszące się do tego pliku, ponieważ w przypadku wdrażania na platformie Azure aplikacja pobiera informacje dotyczące połączenia z pamięcią podręczną z ustawienia aplikacji dla aplikacji sieci Web, a nie z tego pliku. Ponieważ plik `WebAppPlusCacheAppSecrets.config` nie jest wdrażany na platformie Azure wraz z aplikacją, nie potrzebujesz go, o ile nie zamierzasz uruchamiać aplikacji lokalnie.

        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


1. W **Eksploratorze rozwiązań** kliknij dwukrotnie plik **web.config**, aby go otworzyć.
   
    ![Web.config][cache-web-config]
2. Dodaj następujący atrybut `file` do elementu `appSettings`. Jeśli wcześniej używana była inna nazwa pliku lub lokalizacja, podstaw te wartości w miejsce wyświetlonych w przykładzie.
   
   * Przed: `<appSettings>`
   * Po: ` <appSettings file="C:\AppSecrets\WebAppPlusCacheAppSecrets.config">`
     
     Środowisko uruchomieniowe ASP.NET scala zawartość pliku zewnętrznego ze znacznikami w elemencie `<appSettings>`. Środowisko uruchomieniowe ignoruje atrybut pliku, jeśli nie można odnaleźć określonego pliku. Klucze tajne (parametry połączenia do pamięci podręcznej) nie są dołączone jako część kodu źródłowego dla aplikacji. Podczas wdrażania aplikacji sieci Web na platformie Azure plik `WebAppPlusCacheAppSecrests.config` nie zostanie wdrożony (jest to zamierzone). Istnieje kilka sposobów na określenie tych kluczy tajnych na platformie Azure, a w tym samouczku są one konfigurowane automatycznie podczas [aprowizacji zasobów Azure](#provision-the-azure-resources) w kolejnym kroku samouczka. Więcej informacji na temat pracy z kluczami tajnymi na platformie Azure znajduje się w artykule [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](http://www.asp.net/identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure) (Najlepsze praktyki dotyczące wdrażania haseł i innych poufnych danych w programie ASP.NET i Usłudze aplikacji Azure).

### <a name="update-the-teamscontroller-class-to-return-results-from-the-cache-or-the-database"></a>Aktualizacja klasy TeamsController w celu zwracania wyników z pamięci podręcznej lub bazy danych
W tym przykładzie statystyki zespołu można pobrać z bazy danych lub z pamięci podręcznej. Statystyki zespołu są przechowywane w pamięci podręcznej jako zserializowany obiekt `List<Team>`, a także jako posortowany zestaw korzystający z typów danych Redis. Podczas pobierania elementów z zestawu posortowanego możesz pobrać część z nich, wszystkie lub przesłać zapytanie o określone elementy. W tym przykładzie prześlesz do zestawu posortowanego zapytanie dotyczące najlepszych 5 zespołów uporządkowanych według liczby zwycięstw.

> [!NOTE]
> Do korzystania z usługi Azure Redis Cache nie jest wymagane przechowywanie statystyk zespołu w wielu formatach w pamięci podręcznej. W tym samouczku używa się wielu formatów, aby przedstawić różne sposoby i różne typy danych, których można użyć do buforowania danych.
> 
> 

1. Dodaj poniższe instrukcje using na początku pliku `TeamsController.cs` razem z innymi instrukcjami using.
   
        using System.Diagnostics;
        using Newtonsoft.Json;
2. Zastąp bieżącą metodę `public ActionResult Index()` następującą implementacją.

        // GET: Teams
        public ActionResult Index(string actionType, string resultType)
        {
            List<Team> teams = null;

            switch(actionType)
            {
                case "playGames": // Play a new season of games.
                    PlayGames();
                    break;

                case "clearCache": // Clear the results from the cache.
                    ClearCachedTeams();
                    break;

                case "rebuildDB": // Rebuild the database with sample data.
                    RebuildDB();
                    break;
            }

            // Measure the time it takes to retrieve the results.
            Stopwatch sw = Stopwatch.StartNew();

            switch(resultType)
            {
                case "teamsSortedSet": // Retrieve teams from sorted set.
                    teams = GetFromSortedSet();
                    break;

                case "teamsSortedSetTop5": // Retrieve the top 5 teams from the sorted set.
                    teams = GetFromSortedSetTop5();
                    break;

                case "teamsList": // Retrieve teams from the cached List<Team>.
                    teams = GetFromList();
                    break;

                case "fromDB": // Retrieve results from the database.
                default:
                    teams = GetFromDB();
                    break;
            }

            sw.Stop();
            double ms = sw.ElapsedTicks / (Stopwatch.Frequency / (1000.0));

            // Add the elapsed time of the operation to the ViewBag.msg.
            ViewBag.msg += " MS: " + ms.ToString();

            return View(teams);
        }


1. Dodaj następujące trzy metody do klasy `TeamsController`, aby wdrożyć typy akcji `playGames`, `clearCache` i `rebuildDB` z instrukcji switch dodanej w poprzednim fragmencie kodu.
   
    Metoda `PlayGames` aktualizuje statystyki zespołu, symulując sezon gier, zapisuje wyniki w bazie danych i czyści obecnie nieaktualne dane z pamięci podręcznej.

        void PlayGames()
        {
            ViewBag.msg += "Updating team statistics. ";
            // Play a "season" of games.
            var teams = from t in db.Teams
                        select t;

            Team.PlayGames(teams);

            db.SaveChanges();

            // Clear any cached results
            ClearCachedTeams();
        }


    Metoda `RebuildDB` ponownie inicjuje bazę danych z domyślnym zestawem zespołów, generuje ich statystyki i czyści obecnie nieaktualne dane z pamięci podręcznej.

        void RebuildDB()
        {
            ViewBag.msg += "Rebuilding DB. ";
            // Delete and re-initialize the database with sample data.
            db.Database.Delete();
            db.Database.Initialize(true);

            // Clear any cached results
            ClearCachedTeams();
        }


    Metoda `ClearCachedTeams` usuwa wszystkie buforowane statystyki zespołów z pamięci podręcznej.


        void ClearCachedTeams()
        {
            IDatabase cache = Connection.GetDatabase();
            cache.KeyDelete("teamsList");
            cache.KeyDelete("teamsSortedSet");
            ViewBag.msg += "Team data removed from cache. ";
        } 


1. Dodaj poniższe cztery metody do klasy `TeamsController`, aby wdrożyć różne sposoby pobierania statystyk zespołu z pamięci podręcznej i bazy danych. Każda z tych metod zwraca obiekt `List<Team>`, który jest następnie wyświetlany w widoku.
   
    Metoda `GetFromDB` odczytuje statystyki zespołu z bazy danych.
   
        List<Team> GetFromDB()
        {
            ViewBag.msg += "Results read from DB. ";
            var results = from t in db.Teams
                orderby t.Wins descending
                select t; 
   
            return results.ToList<Team>();
        }

    Metoda `GetFromList` odczytuje statystyki zespołu z pamięci podręcznej jako zserializowany obiekt `List<Team>`. W przypadku braku trafienia pamięci podręcznej, statystyki zespołu są odczytywane z bazy danych, a następnie zapisywane w pamięci podręcznej do użytku następnym razem. W tym przykładzie używamy serializacji JSON.NET, aby serializować obiekty platformy .NET podczas zapisu w pamięci podręcznej i odczytu z niej. Więcej informacji znajduje się w temacie [Praca z obiektami .NET w usłudze Azure Redis Cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache)

        List<Team> GetFromList()
        {
            List<Team> teams = null;

            IDatabase cache = Connection.GetDatabase();
            string serializedTeams = cache.StringGet("teamsList");
            if (!String.IsNullOrEmpty(serializedTeams))
            {
                teams = JsonConvert.DeserializeObject<List<Team>>(serializedTeams);

                ViewBag.msg += "List read from cache. ";
            }
            else
            {
                ViewBag.msg += "Teams list cache miss. ";
                // Get from database and store in cache
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                cache.StringSet("teamsList", JsonConvert.SerializeObject(teams));
            }
            return teams;
        }


    Metoda `GetFromSortedSet` odczytuje statystyki zespołu z buforowanego zestawu posortowanego. W przypadku braku trafienia pamięci podręcznej, statystyki zespołu są odczytywane z bazy danych i przechowywane w pamięci podręcznej jako zestaw posortowany.


        List<Team> GetFromSortedSet()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();
            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", order: Order.Descending);
            if (teamsSortedSet.Count() > 0)
            {
                ViewBag.msg += "Reading sorted set from cache. ";
                teams = new List<Team>();
                foreach (var t in teamsSortedSet)
                {
                    Team tt = JsonConvert.DeserializeObject<Team>(t.Element);
                    teams.Add(tt);
                }
            }
            else
            {
                ViewBag.msg += "Teams sorted set cache miss. ";

                // Read from DB
                teams = GetFromDB();

                ViewBag.msg += "Storing results to cache. ";
                foreach (var t in teams)
                {
                    Console.WriteLine("Adding to sorted set: {0} - {1}", t.Name, t.Wins);
                    cache.SortedSetAdd("teamsSortedSet", JsonConvert.SerializeObject(t), t.Wins);
                }
            }
            return teams;
        }


    Metoda `GetFromSortedSetTop5` odczytuje pierwsze 5 zespołów z buforowanego zestawu posortowanego. Rozpoczyna od sprawdzenia pamięci podręcznej pod kątem istnienia klucza `teamsSortedSet`. Jeśli klucz ten nie jest obecny, wywołana zostaje metoda `GetFromSortedSet` w celu odczytu statystyk zespołu i zapisania ich w pamięci podręcznej. Następnie zostaje przesłane zapytanie do buforowanego zestawu posortowanego, które zwraca 5 najlepszych zespołów.


        List<Team> GetFromSortedSetTop5()
        {
            List<Team> teams = null;
            IDatabase cache = Connection.GetDatabase();

            // If the key teamsSortedSet is not present, this method returns a 0 length collection.
            var teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            if(teamsSortedSet.Count() == 0)
            {
                // Load the entire sorted set into the cache.
                GetFromSortedSet();

                // Retrieve the top 5 teams.
                teamsSortedSet = cache.SortedSetRangeByRankWithScores("teamsSortedSet", stop: 4, order: Order.Descending);
            }

            ViewBag.msg += "Retrieving top 5 teams from cache. ";
            // Get the top 5 teams from the sorted set
            teams = new List<Team>();
            foreach (var team in teamsSortedSet)
            {
                teams.Add(JsonConvert.DeserializeObject<Team>(team.Element));
            }
            return teams;
        }


### <a name="update-the-create-edit-and-delete-methods-to-work-with-the-cache"></a>Aktualizacja metod Utwórz, Edytuj i Usuń w celu pracy z pamięcią podręczną
Kod tworzenia szkieletu, który został wygenerowany w ramach tego przykładu, zawiera metody dodawania, edytowania i usuwania zespołów. Za każdym razem, gdy zespół jest dodawany, edytowany lub usuwany, dane w pamięci podręcznej stają się nieaktualne. W tej sekcji zmodyfikujesz te trzy metody w celu czyszczenia buforowanych zespołów w taki sposób, aby pamięć podręczna była zsynchronizowana z bazą danych.

1. Przejdź do metody `Create(Team team)` w klasie `TeamsController`. Dodaj wywołanie metody `ClearCachedTeams`, jak pokazano w poniższym przykładzie.

        // POST: Teams/Create
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Create([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Teams.Add(team);
                db.SaveChanges();
                // When a team is added, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }

            return View(team);
        }


1. Przejdź do metody `Edit(Team team)` w klasie `TeamsController`. Dodaj wywołanie metody `ClearCachedTeams`, jak pokazano w poniższym przykładzie.

        // POST: Teams/Edit/5
        // To protect from overposting attacks, please enable the specific properties you want to bind to, for 
        // more details see http://go.microsoft.com/fwlink/?LinkId=317598.
        [HttpPost]
        [ValidateAntiForgeryToken]
        public ActionResult Edit([Bind(Include = "ID,Name,Wins,Losses,Ties")] Team team)
        {
            if (ModelState.IsValid)
            {
                db.Entry(team).State = EntityState.Modified;
                db.SaveChanges();
                // When a team is edited, the cache is out of date.
                // Clear the cached teams.
                ClearCachedTeams();
                return RedirectToAction("Index");
            }
            return View(team);
        }


1. Przejdź do metody `DeleteConfirmed(int id)` w klasie `TeamsController`. Dodaj wywołanie metody `ClearCachedTeams`, jak pokazano w poniższym przykładzie.

        // POST: Teams/Delete/5
        [HttpPost, ActionName("Delete")]
        [ValidateAntiForgeryToken]
        public ActionResult DeleteConfirmed(int id)
        {
            Team team = db.Teams.Find(id);
            db.Teams.Remove(team);
            db.SaveChanges();
            // When a team is deleted, the cache is out of date.
            // Clear the cached teams.
            ClearCachedTeams();
            return RedirectToAction("Index");
        }


### <a name="update-the-teams-index-view-to-work-with-the-cache"></a>Aktualizacja widoku Indeks zespołów w celu pracy z pamięcią podręczną
1. W **Eksploratorze rozwiązań** rozwiń folder **Widoki**, a następnie folder **Zespoły** i kliknij dwukrotnie plik **Index.cshtml**.
   
    ![Index.cshtml][cache-views-teams-index-cshtml]
2. W początkowej części pliku odszukaj następujący element akapitu.
   
    ![Tabela akcji][cache-teams-index-table]
   
    Jest to link do tworzenia nowego zespołu. Zastąp element akapitu poniższą tabelą. Ta tabela zawiera linki akcji do tworzenia nowego zespołu, odtwarzania nowego sezonu gier, czyszczenia pamięci podręcznej, pobierania zespołów z pamięci podręcznej w wielu formatach, pobierania zespołów z bazy danych i odbudowy bazy danych przy użyciu nowych przykładowych danych.

        <table class="table">
            <tr>
                <td>
                    @Html.ActionLink("Create New", "Create")
                </td>
                <td>
                    @Html.ActionLink("Play Season", "Index", new { actionType = "playGames" })
                </td>
                <td>
                    @Html.ActionLink("Clear Cache", "Index", new { actionType = "clearCache" })
                </td>
                <td>
                    @Html.ActionLink("List from Cache", "Index", new { resultType = "teamsList" })
                </td>
                <td>
                    @Html.ActionLink("Sorted Set from Cache", "Index", new { resultType = "teamsSortedSet" })
                </td>
                <td>
                    @Html.ActionLink("Top 5 Teams from Cache", "Index", new { resultType = "teamsSortedSetTop5" })
                </td>
                <td>
                    @Html.ActionLink("Load from DB", "Index", new { resultType = "fromDB" })
                </td>
                <td>
                    @Html.ActionLink("Rebuild DB", "Index", new { actionType = "rebuildDB" })
                </td>
            </tr>    
        </table>


1. Przewiń w dół pliku **Index.cshtml** i dodaj poniższy element `tr` w taki sposób, aby znajdował się w ostatnim wierszu w ostatniej tabeli w pliku.
   
        <tr><td colspan="5">@ViewBag.Msg</td></tr>
   
    W tym wierszu wyświetlana jest wartość `ViewBag.Msg` zawierająca raport o stanie dotyczący bieżącej operacji, która zostaje ustawiona po kliknięciu jednego z linków akcji z poprzedniego kroku.   
   
    ![Komunikat o stanie][cache-status-message]
2. Naciśnij klawisz **F6**, aby utworzyć projekt.

## <a name="provision-the-azure-resources"></a>Aprowizacja zasobów Azure
Aby hostować aplikację na platformie Azure, musisz najpierw aprowizować usługi Azure, których wymaga dana aplikacja. Przykładowa aplikacja w tym samouczku korzysta z poniższych usług Azure.

* Azure Redis Cache
* Aplikacja sieci Web usługi App Service
* SQL Database

Aby wdrożyć te usługi w wybranej nowej lub istniejącej grupie zasobów, kliknij następujący przycisk **Wdróż na platformie Azure**.

[![Wdróż na platformie Azure][deploybutton]](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-web-app-redis-cache-sql-database%2Fazuredeploy.json)

Ten przycisk **Wdróż na platformie Azure** używa szablonu [Azure Quickstart](https://github.com/Azure/azure-quickstart-templates) (Szybki start Azure) opisanego w artykule [Create a Web App plus Redis Cache plus SQL Database](https://github.com/Azure/azure-quickstart-templates/tree/master/201-web-app-redis-cache-sql-database) (Tworzenie aplikacji sieci Web z usługą Redis Cache i usługą SQL Database) w celu aprowizacji tych usług oraz ustawienia parametrów połączenia dla usługi SQL Database i ustawienia aplikacji dla parametrów połączenia usługi Azure Redis Cache.

> [!NOTE]
> Jeśli nie masz konta platformy Azure, możesz [utworzyć bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=redis_cache_hero) w zaledwie kilka minut.
> 
> 

Kliknięcie przycisku **Wdróż na platformie Azure** powoduje przejście do witryny Azure Portal i inicjuje proces tworzenia zasobów opisanych przez szablon.

![Wdrażanie na platformie Azure][cache-deploy-to-azure-step-1]

1. W bloku **Wdrożenie niestandardowe** wybierz subskrypcję Azure do użycia i wybierz istniejącą grupę zasobów lub utwórz nową, a także określ lokalizację grupy zasobów.
2. W bloku **Parametry** określ nazwę konta administratora (**ADMINISTRATORLOGIN** — nie należy używać pojęcia **admin**), hasło logowania administratora (**ADMINISTRATORLOGINPASSWORD**) i nazwę bazy danych (**DATABASENAME**). Inne parametry są skonfigurowane na potrzeby planu hostingu Usługi aplikacji w warstwie Bezpłatna i tańszych opcji usług SQL Database i Azure Redis Cache, które nie są oferowane w warstwie Bezpłatna.
3. Zmień inne ustawienia, jeśli to konieczne, lub zachowaj ustawienia domyślne, a następnie kliknij przycisk **OK**.

![Wdrażanie na platformie Azure][cache-deploy-to-azure-step-2]

1. Kliknij przycisk **Przejrzyj postanowienia prawne**.
2. Przeczytaj postanowienia w bloku **Zakup** i kliknij przycisk **Kup**.
3. Aby rozpocząć aprowizację zasobów, kliknij przycisk **Utwórz** w bloku **Wdrożenie niestandardowe**.

Aby wyświetlić postęp wdrożenia, kliknij ikonę powiadomienia, a następnie kliknij przycisk **Wdrażanie rozpoczęte**.

![Wdrażanie rozpoczęte][cache-deployment-started]

Stan wdrożenia możesz wyświetlić w bloku **Microsoft.Template**.

![Wdrażanie na platformie Azure][cache-deploy-to-azure-step-3]

Po ukończeniu aprowizacji możesz opublikować aplikację na platformie Azure z programu Visual Studio.

> [!NOTE]
> Wszelkie błędy, które mogą wystąpić w trakcie procesu aprowizacji są wyświetlane w bloku **Microsoft.Template**. Typowe błędy polegają na tym, że jest za dużo serwerów SQL Server lub za dużo planów hostowania Usługi aplikacji w warstwie Bezpłatna na subskrypcję. Usuń wszelkie błędy i uruchom ponownie proces, klikając pozycję **Wdróż ponownie** w bloku **Microsoft.Template** lub przycisk **Wdróż na platformie Azure** w tym samouczku.
> 
> 

## <a name="publish-the-application-to-azure"></a>Publikowanie aplikacji na platformie Azure
W tym kroku samouczka opublikujesz aplikację na platformie Azure i uruchomisz ją w chmurze.

1. Kliknij prawym przyciskiem myszy projekt **ContosoTeamStats** w programie Visual Studio i wybierz polecenie **Publikuj**.
   
    ![Publikowanie][cache-publish-app]
2. Kliknij przycisk **Usługa aplikacji Microsoft Azure**.
   
    ![Publikowanie][cache-publish-to-app-service]
3. Wybierz subskrypcję używaną podczas tworzenia zasobów platformy Azure, rozwiń grupę zasobów zawierającą odpowiednie zasoby, wybierz żądaną aplikację sieci Web i kliknij przycisk **OK**. Jeśli został użyty przycisk **Wdróż na platformie Azure** nazwa aplikacji sieci Web rozpoczyna się od ciągu **webSite**, po którym występują dodatkowe znaki.
   
    ![Wybieranie aplikacji sieci Web][cache-select-web-app]
4. Kliknij przycisk **Sprawdź poprawność połączenia**, aby sprawdzić ustawienia, a następnie kliknij przycisk **Publikuj**.
   
    ![Publikowanie][cache-publish]
   
    Po krótkim czasie zakończy się proces publikowania i zostanie otwarta przeglądarka z uruchomioną przykładową aplikacją. Jeśli podczas sprawdzania poprawności lub publikowania wystąpił błąd DNS, a proces aprowizacji zasobów platformy Azure dla aplikacji został dopiero niedawno ukończony, zaczekaj chwilę i spróbuj ponownie.
   
    ![Dodano pamięć podręczną][cache-added-to-application]

W poniższej tabeli opisano każdy link akcji z przykładowej aplikacji.

| Akcja | Opis |
| --- | --- |
| Create New (Utwórz nowe) |Tworzenie nowego zespołu. |
| Play Season (Odtwarzaj sezon) |Odtwarzanie sezonu gier, aktualizacja statystyk zespołu i czyszczenie nieaktualnych danych zespołu z pamięci podręcznej. |
| Clear Cache (Wyczyść pamięć podręczną) |Czyszczenie statystyk zespołu z pamięci podręcznej. |
| List from Cache (Lista z pamięci podręcznej) |Pobieranie statystyk zespołu z pamięci podręcznej. W przypadku braku trafienia pamięci podręcznej ładuje statystyki z bazy danych i zapisuje je w pamięci podręcznej do użytku następnym razem. |
| Sorted Set from Cache (Zestaw posortowany z pamięci podręcznej) |Pobieranie statystyk zespołu z pamięci podręcznej przy użyciu zestawu posortowanego. W przypadku braku trafienia pamięci podręcznej ładuje statystyki z bazy danych i zapisuje je w pamięci podręcznej przy użyciu zestawu posortowanego. |
| Top 5 Teams from Cache (5 najlepszych zespołów z pamięci podręcznej) |Pobieranie najlepszych 5 zespołów z pamięci podręcznej przy użyciu zestawu posortowanego. W przypadku braku trafienia pamięci podręcznej ładuje statystyki z bazy danych i zapisuje je w pamięci podręcznej przy użyciu zestawu posortowanego. |
| Load from DB (Ładuj z bazy danych) |Pobieranie statystyk zespołu z bazy danych. |
| Rebuild DB (Odbuduj bazę danych) |Odbudowywanie bazy danych i ponowne jej ładowanie z użyciem przykładowych danych zespołu. |
| Edytuj / Szczegóły / Usuń |Edytowanie zespołu, wyświetlanie szczegółów dla zespołu, usuwanie zespołu. |

Klikaj różne akcje i eksperymentuj z pobieraniem danych z różnych źródeł. Zwróć uwagę na różnice czasu, który jest potrzebny do realizacji różnych sposobów pobierania danych z bazy danych i pamięci podręcznej.

## <a name="delete-the-resources-when-you-are-finished-with-the-application"></a>Usuwanie zasobów po zakończeniu pracy z aplikacją
Po zakończeniu pracy z przykładową aplikacją z samouczka możesz usunąć używane zasoby platformy Azure w celu oszczędności kosztów i zasobów. Jeśli w sekcji [Aprowizacja obsługi zasobów Azure](#provision-the-azure-resources) użyto przycisku **Wdróż na platformie Azure** i wszystkie zasoby znajdują się w tej samej grupie zasobów, możesz usunąć je razem w ramach jednej operacji przez usunięcie grupy zasobów.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com) i kliknij pozycję **Grupy zasobów**.
2. Wpisz nazwę grupy zasobów w polu tekstowym **Filtruj elementy...**.
3. Kliknij przycisk **...** na prawo od swojej grupy zasobów.
4. Kliknij polecenie **Usuń**.
   
    ![Usuwanie][cache-delete-resource-group]
5. Wpisz nazwę grupy zasobów, a następnie kliknij przycisk **Usuń**.
   
    ![Potwierdzenie usunięcia][cache-delete-confirm]

Po krótkim czasie grupa zasobów i wszystkie zawarte w niej zasoby zostaną usunięte.

> [!IMPORTANT]
> Pamiętaj, że usunięcie grupy zasobów jest nieodwracalne i że grupa zasobów oraz wszystkie zawarte w niej zasoby zostaną trwale usunięte. Uważaj, aby nie usunąć przypadkowo niewłaściwych zasobów lub grupy zasobów. Jeśli zasoby do hostowania w tym przykładzie zostały utworzone wewnątrz istniejącej grupy zasobów, możesz usuwać poszczególne zasoby pojedynczo z odpowiednich bloków.
> 
> 

## <a name="run-the-sample-application-on-your-local-machine"></a>Uruchamianie przykładowej aplikacji na komputerze lokalnym
Do uruchomienia aplikacji lokalnie na komputerze potrzebne jest wystąpienie usługi Azure Redis Cache, w ramach którego będą buforowane dane. 

* Jeśli aplikacja została opublikowana na platformie Azure, jak opisano w poprzedniej sekcji, możesz użyć wystąpienia usługi Azure Redis Cache, które zostało zaaprowizowane w tamtym kroku.
* Jeśli masz inne istniejące wystąpienie usługi Azure Redis Cache, możesz go użyć, aby uruchomić ten przykład lokalnie.
* Jeśli wystąpi potrzeba utworzenia wystąpienia usługi Azure Redis Cache, możesz wykonać kroki w opisane w temacie [Tworzenie pamięci podręcznej](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache).

Po wybraniu lub utworzeniu pamięci podręcznej do użycia przejdź do pamięci podręcznej w witrynie Azure Portal i pobierz [nazwę hosta](cache-configure.md#properties) i [klucze dostępu](cache-configure.md#access-keys) dla swojej pamięci podręcznej. Instrukcje znajdują się w artykule [Configure Redis cache settings](cache-configure.md#configure-redis-cache-settings) (Konfigurowanie ustawień pamięci podręcznej Redis).

1. Za pomocą dowolnego edytora otwórz plik `WebAppPlusCacheAppSecrets.config` utworzony w kroku [Konfigurowanie aplikacji do korzystania z pamięci podręcznej Redis](#configure-the-application-to-use-redis-cache) tego samouczka.
2. Edytuj atrybut `value` i zastąp ciąg `MyCache.redis.cache.windows.net` [nazwą hosta](cache-configure.md#properties) pamięci podręcznej oraz określ [klucz podstawowy lub pomocniczy](cache-configure.md#access-keys) pamięci podręcznej jako hasło.

        <appSettings>
          <add key="CacheConnection" value="MyCache.redis.cache.windows.net,abortConnect=false,ssl=true,password=..."/>
        </appSettings>


1. Naciśnij klawisze **Ctrl+F5**, aby uruchomić aplikację.

> [!NOTE]
> Pamiętaj, że ponieważ aplikacja, łącznie z bazą danych, działa lokalnie, a pamięć podręczna Redis jest obsługiwana na platformie Azure, może wydawać się, że pamięć podręczna jest mniej wydajna niż baza danych. Aby uzyskać najlepszą wydajność, aplikacja klienta i wystąpienie usługi Azure Redis Cache powinny być w tej samej lokalizacji. 
> 
> 

## <a name="next-steps"></a>Następne kroki
* Dowiedz się więcej na temat [rozpoczęcia pracy z platformą ASP.NET MVC 5](http://www.asp.net/mvc/overview/getting-started/introduction/getting-started) w witrynie [ASP.NET](http://asp.net/).
* Więcej przykładów dotyczących tworzenia aplikacji sieci Web platformy ASP.NET w usłudze App Service, zobacz [Create and deploy an ASP.NET web app in Azure App Service](https://github.com/Microsoft/HealthClinic.biz/wiki/Create-and-deploy-an-ASP.NET-web-app-in-Azure-App-Service) (Tworzenie i wdrażanie aplikacji sieci web ASP.NET w usłudze Azure App Service) w [pokazie](https://blogs.msdn.microsoft.com/visualstudio/2015/12/08/connectdemos-2015-healthclinic-biz/) 2015 Connect witryny [HealthClinic.biz](https://github.com/Microsoft/HealthClinic.biz).
  * Aby uzyskać więcej materiałów szybkiego startu z w wersji demonstracyjnej witryny HealthClinic.biz, zobacz [Azure Developer Tools Quickstarts](https://github.com/Microsoft/HealthClinic.biz/wiki/Azure-Developer-Tools-Quickstarts) (Materiały szybkiego startu narzędzi deweloperskich platformy Azure).
* Dowiedz się więcej na temat używanego w tym samouczku podejścia [Code first to a new database](https://msdn.microsoft.com/data/jj193542) (Rozwiązanie Code First dla nowej bazy danych) stosowanego w narzędziu Entity Framework.
* Dowiedz się więcej na temat [aplikacji sieci Web w Usłudze aplikacji Azure](../app-service-web/app-service-web-overview.md).
* Poznaj informacje dotyczące [monitorowania](cache-how-to-monitor.md) pamięci podręcznej w witrynie Azure Portal.
* Poznaj funkcje Premium usługi Azure Redis Cache
  
  * [Konfigurowanie trwałości dla usługi Azure Redis Cache w wersji Premium](cache-how-to-premium-persistence.md)
  * [Konfigurowanie klastrowania dla usługi Azure Redis Cache w wersji Premium](cache-how-to-premium-clustering.md)
  * [Konfigurowanie obsługi sieci wirtualnej dla usługi Azure Redis Cache w wersji Premium](cache-how-to-premium-vnet.md)
  * Więcej szczegółowych informacji na temat rozmiaru, przepływności i przepustowości w pamięciach podręcznych Premium znajduje się w artykule [Azure Redis Cache FAQ](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) (Usługa Azure Redis Cache — często zadawane pytania).

<!-- IMAGES -->
[cache-starter-application]: ./media/cache-web-app-howto/cache-starter-application.png
[cache-added-to-application]: ./media/cache-web-app-howto/cache-added-to-application.png
[cache-create-project]: ./media/cache-web-app-howto/cache-create-project.png
[cache-select-template]: ./media/cache-web-app-howto/cache-select-template.png
[cache-model-add-class]: ./media/cache-web-app-howto/cache-model-add-class.png
[cache-model-add-class-dialog]: ./media/cache-web-app-howto/cache-model-add-class-dialog.png
[cache-add-controller]: ./media/cache-web-app-howto/cache-add-controller.png
[cache-add-controller-class]: ./media/cache-web-app-howto/cache-add-controller-class.png
[cache-configure-controller]: ./media/cache-web-app-howto/cache-configure-controller.png
[cache-global-asax]: ./media/cache-web-app-howto/cache-global-asax.png
[cache-RouteConfig-cs]: ./media/cache-web-app-howto/cache-RouteConfig-cs.png
[cache-layout-cshtml]: ./media/cache-web-app-howto/cache-layout-cshtml.png
[cache-layout-cshtml-code]: ./media/cache-web-app-howto/cache-layout-cshtml-code.png
[redis-cache-manage-nuget-menu]: ./media/cache-web-app-howto/redis-cache-manage-nuget-menu.png
[redis-cache-stack-exchange-nuget]: ./media/cache-web-app-howto/redis-cache-stack-exchange-nuget.png
[cache-teamscontroller]: ./media/cache-web-app-howto/cache-teamscontroller.png
[cache-web-config]: ./media/cache-web-app-howto/cache-web-config.png
[cache-views-teams-index-cshtml]: ./media/cache-web-app-howto/cache-views-teams-index-cshtml.png
[cache-teams-index-table]: ./media/cache-web-app-howto/cache-teams-index-table.png
[cache-status-message]: ./media/cache-web-app-howto/cache-status-message.png
[deploybutton]: ./media/cache-web-app-howto/deploybutton.png
[cache-deploy-to-azure-step-1]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-1.png
[cache-deploy-to-azure-step-2]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-2.png
[cache-deploy-to-azure-step-3]: ./media/cache-web-app-howto/cache-deploy-to-azure-step-3.png
[cache-deployment-started]: ./media/cache-web-app-howto/cache-deployment-started.png
[cache-publish-app]: ./media/cache-web-app-howto/cache-publish-app.png
[cache-publish-to-app-service]: ./media/cache-web-app-howto/cache-publish-to-app-service.png
[cache-select-web-app]: ./media/cache-web-app-howto/cache-select-web-app.png
[cache-publish]: ./media/cache-web-app-howto/cache-publish.png
[cache-delete-resource-group]: ./media/cache-web-app-howto/cache-delete-resource-group.png
[cache-delete-confirm]: ./media/cache-web-app-howto/cache-delete-confirm.png




<!--HONumber=Nov16_HO2-->


