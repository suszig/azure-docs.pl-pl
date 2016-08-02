<properties
    pageTitle="Tworzenie elastycznej puli baz danych w języku C# | Microsoft Azure"
    description="Techniki tworzenia bazy danych w języku C# umożliwiają tworzenie elastycznej puli baz danych w bazie danych Azure SQL Database i udostępnianie w ten sposób zasobów przez wiele baz danych."
    services="sql-database"
    documentationCenter=""
    authors="srinia"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="csharp"
    ms.workload="data-management"
    ms.date="05/27/2016"
    ms.author="srinia"/>

# Tworzenie nowej elastycznej puli baz danych w języku C&#x23;

> [AZURE.SELECTOR]
- [Portal Azure](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)


Dowiedz się, jak utworzyć [elastyczną pulę baz danych](sql-database-elastic-pool.md) w języku C&#x23;. 

Typowe kody błędów można znaleźć w artykule [SQL error codes for SQL Database client applications: Database connection error and other issues](sql-database-develop-error-messages.md) (Kody błędów SQL dla aplikacji klienckich usługi SQL Database: błąd połączenia z bazą danych i inne problemy).

Elastyczne pule baz danych są obecnie w wersji zapoznawczej i są dostępne tylko na serwerach V12 usługi SQL Database. Jeśli masz serwer V11 usługi SQL Database, możesz [użyć programu PowerShell do uaktualnienia do wersji V12 i utworzenia puli](sql-database-upgrade-server-portal.md) w jednym kroku.

Ponieważ w przykładach użyto biblioteki [SQL Database Library for .NET](https://msdn.microsoft.com/library/azure/mt349017.aspx), należy ją zainstalować. Można to zrobić, uruchamiając następujące polecenie z poziomu [konsoli menedżera pakietów](http://docs.nuget.org/Consume/Package-Manager-Console) w programie Visual Studio (**Narzędzia**  >  **Menedżer pakietów NuGet**  >  **Konsola menedżera pakietów**):

    PM> Install-Package Microsoft.Azure.Management.Sql –Pre

## Tworzenie nowej puli

Utwórz instancję [SqlManagementClient](https://msdn.microsoft.com/library/microsoft.azure.management.sql.sqlmanagementclient) przy użyciu wartości usługi [Azure Active Directory](sql-database-client-id-keys.md). Utwórz instancję [ElasticPoolCreateOrUpdateParameters](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.elasticpoolcreateorupdateparameters) i wywołaj metodę [CreateOrUpdate](https://msdn.microsoft.com/library/microsoft.azure.management.sql.databaseoperationsextensions.createorupdate). Wartości jednostek eDTU na pulę oraz minimalne i maksymalne liczby jednostek DTU są ograniczone przez wartość warstwy usługi (Podstawowa, Standardowa lub Premium). Zobacz [limity liczby jednostek eDTU i magazynu dla elastycznych pul i elastycznych baz danych](sql-database-elastic-pool.md#eDTU-and-storage-limits-for-elastic-pools-and-elastic-databases).


    ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = "South Central US",
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = "Standard",
            Dtu = 400,
            DatabaseDtuMin = 0,
            DatabaseDtuMax = 100
         }
    };

    // Create the pool
    var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);

## Tworzenie nowej bazy danych w puli

Utwórz wystąpienie bazy danych [DataBaseCreateorUpdateProperties](https://msdn.microsoft.com/library/microsoft.azure.management.sql.models.databasecreateorupdateproperties) i ustaw właściwości nowej bazy danych. Następnie wywołaj metodę CreateOrUpdate z grupą zasobów, nazwą serwera i nową nazwą bazy danych.

    // Create a database: configure create or update parameters and properties explicitly
    DatabaseCreateOrUpdateParameters newPooledDatabaseParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentServer.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = 268435456000, // 250 GB,
            Collation = "SQL_Latin1_General_CP1_CI_AS"
        }
    };

    var poolDbResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database2", newPooledDatabaseParameters);

Aby przenieść istniejącą bazę danych do puli, zobacz temat [Move a database into an elastic pool](sql-database-elastic-pool-manage-csharp.md#Move-a-database-into-an-elastic-pool) (Przenoszenie bazy danych do puli elastycznej).

## Przykład: tworzenie puli w języku C&#x23

W tym przykładzie zostanie utworzona nowa grupa zasobów Azure, nowe wystąpienie serwera Azure SQL Server i nowa pula elastyczna. 
 

Do uruchomienia tego przykładu wymagane są poniższe biblioteki. Instalację można wykonać, uruchamiając następujące polecenia z poziomu [konsoli menedżera pakietów](http://docs.nuget.org/Consume/Package-Manager-Console) w programie Visual Studio (**Narzędzia**  >  **Menedżer pakietów NuGet**  >  **Konsola menedżera pakietów**):

    Install-Package Microsoft.Azure.Management.Sql –Pre
    Install-Package Microsoft.Azure.Management.Resources –Pre
    Install-Package Microsoft.Azure.Common.Authentication –Pre

Utwórz aplikację konsoli i zastąp zawartość pliku Program.cs następującymi wartościami. Aby uzyskać wymagany identyfikator klienta i związane z nim wartości, zobacz temat [Register your app and get the required client values for connecting your app to SQL Database](sql-database-client-id-keys.md) (Rejestrowanie aplikacji i uzyskiwanie wartości klienta wymaganych do połączenia aplikacji z bazą danych SQL Database). Użyj polecenia cmdlet [Get-AzureRmSubscription](https://msdn.microsoft.com/library/mt619284.aspx), aby pobrać wartość identyfikatora subskrypcji.

    using Microsoft.Azure;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbElasticPoolsSample
    {
    class Program
    {
        // authentication variables
        static string subscriptionId = "<your Azure subscription>";
        static string clientId = "<your client id>";
        static string redirectUri = "<your redirect URI>";
        static string domainName = "<i.e. microsoft.onmicrosoft.com>";
        static AuthenticationResult token;

        // resource group variables
        static string datacenterLocation = "<location>";
        static string resourceGroupName = "<resource group name>";

        // server variables
        static string serverName = "<server name>";
        static string adminLogin = "<server admin>";
        static string adminPassword = "<server password (store it securely!)>";
        static string serverVersion = "12.0";

        // pool variables
        static string elasticPoolName = "<pool name>";
        static string poolEdition = "Standard";
        static int poolDtus = 400;
        static int databaseMinDtus = 0;
        static int databaseMaxDtus = 100;


        static void Main(string[] args)
        {
            // Sign in to Azure
            token = GetAccessToken();
            Console.WriteLine("Logged in as: " + token.UserInfo.DisplayableId);

            // Create a resource group
            ResourceGroup rg = CreateResourceGroup();
            Console.WriteLine(rg.Name + " created.");

            // Create a server
            Console.WriteLine("Creating server... ");
            ServerGetResponse srvr = CreateServer();
            Console.WriteLine("Creation of server " + srvr.Server.Name + ": " + srvr.StatusCode.ToString());

            // Create a pool
            Console.WriteLine("Creating elastic database pool... ");
            ElasticPoolCreateOrUpdateResponse epool = CreateElasticDatabasePool();
            Console.WriteLine("Creation of pool " + epool.ElasticPool.Name + ": " + epool.Status.ToString());


            // keep the console open until Enter is pressed!
            Console.WriteLine("Press Enter to exit.");
            Console.ReadLine();
        }


        static ElasticPoolCreateOrUpdateResponse CreateElasticDatabasePool()
        {
            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create elastic pool: configure create or update parameters and properties explicitly
            ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = poolEdition,
                    Dtu = poolDtus, 
                    DatabaseDtuMin = databaseMinDtus,
                    DatabaseDtuMax = databaseMaxDtus
                }
            };

            // Create the pool
            var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate(resourceGroupName, serverName, elasticPoolName, newPoolParameters);
            return newPoolResponse;
        }



        static ServerGetResponse CreateServer()
        {

            // Create a SQL Database management client
            SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials(subscriptionId, token.AccessToken));

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = datacenterLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = adminLogin,
                    AdministratorLoginPassword = adminPassword,
                    Version = serverVersion
                }
            };

            var serverResult = sqlClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;

        }


        static ResourceGroup CreateResourceGroup()
        {
           
            Microsoft.Rest.TokenCredentials creds = new Microsoft.Rest.TokenCredentials(token.AccessToken);
            // Create a resource management client 
            ResourceManagementClient resourceClient = new ResourceManagementClient(creds);

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = datacenterLocation,
            };

            //Create a resource group
            resourceClient.SubscriptionId = subscriptionId;
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
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

- [Zarządzanie pulą](sql-database-elastic-pool-manage-csharp.md)
- [Tworzenie zadań elastycznych](sql-database-elastic-jobs-overview.md): zadania elastyczne umożliwiają uruchamianie skryptów T-SQL na dowolnej liczbie baz danych w puli.
- [Skalowanie w poziomie przy użyciu usługi Azure SQL Database](sql-database-elastic-scale-introduction.md): użyj narzędzi elastycznej bazy danych do skalowania w poziomie.

## Dodatkowe zasoby

- [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Interfejsy API usługi Azure Resource Management](https://msdn.microsoft.com/library/azure/dn948464.aspx)


<!--HONumber=Jun16_HO2-->


