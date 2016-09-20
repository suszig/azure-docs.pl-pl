<properties
    pageTitle="Wypróbuj usługę SQL Database: używanie języka C# do tworzenia bazy danych SQL | Microsoft Azure"
    description="Wypróbuj usługę SQL Database SQL do tworzenia aplikacji w językach SQL i C#, a także utwórz bazę danych Azure SQL w języku C# przy użyciu biblioteki SQL Database Library for .NET."
    keywords="wypróbuj usługę sql, sql c#"   
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="csharp"
   ms.workload="data-management"
   ms.date="05/26/2016"
   ms.author="sstein"/>

# Wypróbuj usługę SQL Database: używanie języka C# do tworzenia bazy danych SQL z zastosowaniem biblioteki SQL Database Library for .NET


> [AZURE.SELECTOR]
- [Azure Portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Dowiedz się, jak używać poleceń języka C# do tworzenia bazy danych Azure SQL z zastosowaniem biblioteki [Azure SQL Database Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). Wypróbujesz usługę SQL Database, tworząc pojedynczą bazę danych w językach SQL i C#. Aby tworzyć elastyczne pule baz danych, zobacz artykuł [Tworzenie elastycznej puli baz danych](sql-database-elastic-pool-create-portal.md). Poszczególne fragmenty kodu są podzielone dla uzyskania przejrzystości, a przykładowa aplikacja konsolowa łączy wszystkie polecenia w dolnej części tego artykułu.

Biblioteka Azure SQL Database Library for .NET dostarcza interfejs API oparty na usłudze [Azure Resource Manager](../resource-group-overview.md), który opakowuje [interfejs API REST usługi SQL Database oparty na usłudze Resource Manager](https://msdn.microsoft.com/library/azure/mt163571.aspx). Ta biblioteka kliencka jest zgodna ze wspólnym wzorcem bibliotek klienckich opartych na usłudze Resource Manager. Usługa Resource Manager wymaga grup zasobów oraz uwierzytelniania [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (AAD).

<br>

> [AZURE.NOTE] Biblioteka SQL Database Library for .NET jest obecnie w wersji zapoznawczej.

<br>

Do wykonania kroków opisanych w tym artykule potrzebne są:

- Subskrypcja platformy Azure. Jeśli potrzebujesz subskrypcji platformy Azure, po prostu kliknij łącze **BEZPŁATNA WERSJA PRÓBNA** u góry tej strony, a następnie wróć do artykułu.
- Program Visual Studio. Aby uzyskać bezpłatną kopię programu Visual Studio, zobacz stronę [Visual Studio — pliki do pobrania](https://www.visualstudio.com/downloads/download-visual-studio-vs).


## Instalowanie wymaganych bibliotek

Aby skonfigurować bazę danych SQL w języku C#, pobierz wymagane biblioteki zarządzania. W tym celu zainstaluj następujące pakiety przy użyciu [konsoli Menedżera pakietów](http://docs.nuget.org/Consume/Package-Manager-Console) w programie Visual Studio (**Narzędzia** > **Menedżer pakietów NuGet** > **Konsola Menedżera pakietów**):

    Install-Package Microsoft.Azure.Management.Sql –Pre
    Install-Package Microsoft.Azure.Management.ResourceManager –Pre -Version 1.1.1-preview
    Install-Package Microsoft.Azure.Common.Authentication –Pre


## Konfigurowanie uwierzytelniania w usłudze Azure Active Directory

Należy najpierw umożliwić aplikacji klienckiej dostęp do usługi SQL Database, konfigurując wymagane uwierzytelnianie.

W celu uwierzytelniania aplikacji klienckiej na podstawie bieżącego użytkownika trzeba najpierw zarejestrować aplikację w domenie AAD skojarzonej z subskrypcją, w której zostały utworzone zasoby systemu Azure. Jeśli subskrypcja platformy Azure została utworzona za pomocą konta Microsoft, a nie za pomocą konta służbowego lub szkolnego, masz już domyślną domenę usługi AAD.

Aby utworzyć nową aplikację i zarejestrować ją w odpowiedniej usłudze Active Directory, wykonaj następujące czynności:

1. Przejdź do [klasycznego portalu Azure](https://manage.windowsazure.com/).
1. Po lewej stronie wybierz usługę **Active Directory**, a następnie wybierz katalog do uwierzytelniania aplikacji i kliknij jego **nazwę**.

    ![Wypróbuj usługę SQL Database: konfigurowanie usługi Azure Active Directory (AAD).][1]

2. Na stronie katalogu kliknij opcję **APPLICATIONS** (aplikacje).

    ![Strona katalogu z aplikacjami.][5]

4. Kliknij przycisk **ADD** (dodaj), aby utworzyć nową aplikację.

5. Wybierz opcję **Add an application my organization is developing** (Dodaj aplikację, którą programuje moja organizacja).

5. Podaj **NAME** (nazwę) aplikacji oraz wybierz opcję **NATIVE CLIENT APPLICATION** (natywna aplikacja kliencka).

    ![Podaj informacje o aplikacji SQL C#.][7]

6. Podaj **REDIRECT URI** (identyfikator URI przekierowania). Nie musi to być istniejący punkt końcowy; wystarczy prawidłowy identyfikator URI.

    ![Dodaj adres URL przekierowania aplikacji SQL C#.][8]

7. Zakończ tworzenie aplikacji, kliknij przycisk **CONFIGURE** (konfiguruj) i skopiuj **CLIENT ID** (identyfikator klienta). Będzie on potrzebny w dalszej części kodu.

    ![Pobieranie identyfikatora klienta potrzebnego w aplikacji SQL C#.][9]


1. W dolnej części strony kliknij przycisk **Add application** (Dodaj aplikację).
1. Wybierz opcję **Microsoft Apps** (aplikacje firmy Microsoft).
1. Wybierz opcję **Azure Service Management API** (Interfejs API zarządzania usługą Azure), a następnie zakończ pracę kreatora.
2. Przy użyciu wybranego interfejsu API trzeba teraz udzielić określonych uprawnień wymaganych do dostępu do tego interfejsu API, wybierając opcję **Access Azure Service Management (preview)** (Dostęp do zarządzania usługami Azure — wersja zapoznawcza).

    ![Ustawianie uprawnień.][2]

2. Kliknij przycisk **SAVE** (Zapisz).



### Określ nazwę domeny

Nazwa domeny jest wymagana w kodzie programu. Oto prosty sposób określania nazwy domeny:

1. Przejdź do [Portalu Azure](http://portal.azure.com).
2. Umieść kursor nad swoją nazwą w prawym górnym rogu i zanotuj domenę wyświetloną w oknie podręcznym.

    ![Określanie nazwy domeny.][3]





**Dodatkowe zasoby AAD**  

Dodatkowe informacje dotyczące korzystania z usługi Azure Active Directory w celu uwierzytelniania są zawarte w tym [przydatnym wpisie w blogu](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/).


### Pobieranie tokenu dostępu dla bieżącego użytkownika

Aplikacja kliencka musi pobrać token dostępu do aplikacji dla bieżącego użytkownika. Przy pierwszym wykonywaniu tego kodu przez użytkownika zostanie wyświetlony monit o podanie poświadczeń, a wynikowy token zostanie zbuforowany lokalnie. Podczas kolejnych wykonań kodu token będzie pobierany z bufora, a użytkownik będzie monitowany o zalogowanie jedynie po wygaśnięciu ważności tokenu.

Ten kod zwraca token typu Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationResult, który będzie potrzebny w innych fragmentach kodu poniżej.

        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }



> [AZURE.NOTE] W przykładach podanych w tym artykule używane są synchroniczne formy poszczególnych żądań interfejsu API, które blokują działanie programu do momentu ukończenia wywołania REST na podległej usłudze. Dostępne są również metody asynchroniczne.



## Tworzenie grupy zasobów

W usłudze Resource Manager wszystkie zasoby muszą być tworzone w grupie zasobów. Grupa zasobów to kontener, który zawiera powiązane zasoby aplikacji. W usłudze Azure SQL Database serwer bazy danych musi zostać utworzony w istniejącej grupie zasobów.

        static void CreateResourceGroup()
        {
            creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);

            // Create a resource management client
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = location,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
        }


## Tworzenie serwera

Bazy danych SQL znajdują się na serwerach. Nazwa serwera musi być globalnie unikatowa w zakresie wszystkich serwerów SQL platformy Azure, zatem podanie zajętej nazwy serwera spowoduje błąd. Warto także zauważyć, że wykonanie tego polecenia może potrwać kilka minut.

    static void CreateServer()
    {
        // Create a SQL Database management client
        SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

        // Create a server
        ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
        {
            Location = location,
            Properties = new ServerCreateOrUpdateProperties()
            {
                AdministratorLogin = administratorLogin,
                AdministratorLoginPassword = administratorPassword,
                Version = serverVersion
            }
        };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
    }


### Tworzenie zewnętrznego administratora serwera


    // Create a server external admin
    ServerAdministratorCreateOrUpdateParameters aadAdminParameters = new ServerAdministratorCreateOrUpdateParameters()
    {
        Properties = new ServerAdministratorCreateOrUpdateProperties()
        {
            AdministratorType = "ActiveDirectory",
            Login = "<login>",
            Sid = new Guid("<Azure AD admin sid>"),
            TenantId = new Guid("<Azure AD tenant id>")
        }
    };

    var adminResult = sqlClient.ServerAdministrators.CreateOrUpdate(resourceGroupName, serverName, "ActiveDirectory", aadAdminParameters);
    var adminGetResult = sqlClient.ServerAdministrators.Get(resourceGroupName, serverName, "ActiveDirectory");




## Tworzenie reguły zapory serwera, aby zezwolić na dostęp do serwera

Domyślnie do serwera nie ma dostępu z żadnej lokalizacji. Aby można było nawiązać połączenie z serwerem lub zawartymi w nim bazami danych, trzeba zdefiniować [regułę zapory](https://msdn.microsoft.com/library/azure/ee621782.aspx) umożliwiającą dostęp z adresu IP klienta.

W poniższym przykładzie jest tworzona reguła, która otwiera dostęp do serwera z dowolnego adresu IP. Zaleca się utworzenie odpowiednich kont logowania SQL i haseł w celu zabezpieczenia bazy danych. Nie należy polegać na regułach zapory jako głównej linii obrony przed nieautoryzowanym dostępem.


        static void CreateFirewallRule()
        {
            // Create a firewall rule on the server
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = "0.0.0.0",
                    EndIpAddress = "255.255.255.255"
                }
            };
            var firewallResult = sqlClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
        }




Aby zezwolić innym usługom Azure na dostęp do serwera, dodaj regułę zapory i ustaw wartość 0.0.0.0 we właściwościach StartIpAddress (początkowy adres IP) i EndIpAddress (końcowy adres IP). Należy zauważyć, że pozwala to na dostęp do serwera ruchu pochodzącego z *dowolnej* subskrypcji platformy Azure.


## Tworzenie bazy danych SQL za pomocą języka C&#x23;

Następujące polecenie języka C# spowoduje utworzenie nowej bazy danych SQL, jeśli baza danych o takiej samej nazwie jeszcze nie istnieje na serwerze. Jeśli baza danych o tej nazwie istnieje, zostanie zaktualizowana.


        static void CreateDatabase()
        {
            // Create a database

            // Retrieve the server on which the database will be created
            Server currentServer = sqlClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel,
                }
            };
            var dbResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
        }



## Przykładowa aplikacja konsolowa w języku C&#x23;

W poniższym przykładzie tworzone są: grupa zasobów, serwer, reguła zapory i baza danych SQL. W sekcji *Konfigurowanie uwierzytelniania w usłudze Azure Active Directory* w górnej części tego artykułu można zobaczyć, skąd pobrać wartości zmiennych clientId (identyfikator klienta), redirectUri (identyfikator URI przekierowania) i domainName (nazwa domeny).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Text;
    using System.Threading.Tasks;

    namespace SqlDbConsoleApp
    {
    class Program
    {
        // Get these values from the Azure portal
        static string subscriptionId = "<Azure subscription id>";
        static string clientId = "<Azure App clientId>";
        static string redirectUri = "<Azure App redirectURI>";
        static string domainName = "<domain>";

        // You create these values
        static string resourceGroupName = "<your resource group name>";
        static string location = "<Azure data center location>";

        static string serverName = "<your server name>";
        static string administratorLogin = "<your server admin>";

        // store your password securely!
        static string administratorPassword = "<your server admin password>";
        static string serverVersion = "12.0";

        static string firewallRuleName = "<your firewall rule name>";

        static string databaseName = "dbfromcsharparticle";
        static string databaseEdition = "Basic"; // Basic, Standard, or Premium
        static string databasePerfLevel = "";

        static AuthenticationResult token;
        static Microsoft.Rest.TokenCredentials creds;

        static SqlManagementClient sqlClient;


        static void Main(string[] args)
        {
            Console.WriteLine("Logging in...");

            token = GetAccessToken();

            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            Console.WriteLine("Creating resource group...");
            CreateResourceGroup();

            sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            Console.WriteLine("Creating server...");
            CreateServer();

            Console.WriteLine("Creating firewall rule...");
            CreateFirewallRule();

            Console.WriteLine("Creating database...");

            DatabaseCreateOrUpdateResponse dbResponse = CreateDatabase();
            Console.WriteLine("Status: " + dbResponse.Status.ToString()
                + " Code: " + dbResponse.StatusCode.ToString());

            Console.WriteLine("Press enter to exit...");
            Console.ReadLine();
        }

        static void CreateResourceGroup()
        {
            creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);

            // Create a resource management client
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = location,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
        }

        static void CreateServer()
        {
            // Create a SQL Database management client
            //SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = location,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = administratorLogin,
                    AdministratorLoginPassword = administratorPassword,
                    Version = serverVersion
                }
            };
            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
        }

        static void CreateFirewallRule()
        {
            // Create a firewall rule on the server
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    // replace with your client IP address
                    StartIpAddress = "0.0.0.0",
                    EndIpAddress = "255.255.255.255"
                }
            };
            var firewallResult = sqlClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
        }

        static DatabaseCreateOrUpdateResponse CreateDatabase()
        {
            // Create a database

            // Retrieve the server on which the database will be created
            Server currentServer = sqlClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel,
                }
            };
            var dbResponse = sqlClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }

        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" + domainName /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                    clientId,
            new Uri(redirectUri) /* redirect URI */,
            PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }
    }
    }




## Następne kroki
Po wypróbowaniu usługi SQL Database i skonfigurowaniu bazy danych przy użyciu języka C# możesz przystąpić do czytania następujących artykułów:

- [Nawiązywanie połączenia z usługą SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL](sql-database-connect-query-ssms.md)

## Dodatkowe zasoby

- [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)





<!--Image references-->
[1]: ./media/sql-database-get-started-csharp/aad.png
[2]: ./media/sql-database-get-started-csharp/permissions.png
[3]: ./media/sql-database-get-started-csharp/getdomain.png
[4]: ./media/sql-database-get-started-csharp/aad2.png
[5]: ./media/sql-database-get-started-csharp/aad-applications.png
[6]: ./media/sql-database-get-started-csharp/add.png
[7]: ./media/sql-database-get-started-csharp/add-application.png
[8]: ./media/sql-database-get-started-csharp/add-application2.png
[9]: ./media/sql-database-get-started-csharp/clientid.png



<!--HONumber=sep16_HO1-->


