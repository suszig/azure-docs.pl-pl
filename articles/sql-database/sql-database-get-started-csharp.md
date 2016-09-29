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
   ms.date="09/14/2016"
   ms.author="sstein"/>


# Wypróbuj usługę SQL Database: używanie języka C# do tworzenia bazy danych SQL z zastosowaniem biblioteki SQL Database Library for .NET


> [AZURE.SELECTOR]
- [Azure Portal](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)

Dowiedz się, jak używać języka C# do tworzenia bazy danych Azure SQL z zastosowaniem biblioteki [Azure SQL Database Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql). W tym artykule opisano sposób tworzenia jednej bazy danych SQL w języku C#. Aby tworzyć elastyczne pule baz danych, zobacz artykuł [Tworzenie elastycznej puli baz danych](sql-database-elastic-pool-create-portal.md).

Biblioteka Azure SQL Database Library for .NET dostarcza interfejs API oparty na usłudze [Azure Resource Manager](../resource-group-overview.md), który opakowuje [interfejs API REST usługi SQL Database oparty na usłudze Resource Manager](https://msdn.microsoft.com/library/azure/mt163571.aspx).


> [AZURE.NOTE] Biblioteka SQL Database Library for .NET jest obecnie w wersji zapoznawczej.


Do wykonania kroków opisanych w tym artykule potrzebne są:

- Subskrypcja platformy Azure. Jeśli potrzebujesz subskrypcji platformy Azure, po prostu kliknij pozycję **BEZPŁATNE KONTO** u góry tej strony, a następnie wróć do artykułu.
- Program Visual Studio. Aby uzyskać bezpłatną kopię programu Visual Studio, zobacz stronę [Visual Studio — pliki do pobrania](https://www.visualstudio.com/downloads/download-visual-studio-vs).


## Tworzenie aplikacji konsoli i instalowanie wymaganych bibliotek

1. Uruchom program Visual Studio.
2. Kliknij pozycję **Plik** > **Nowy** > **Projekt**.
3. Utwórz **aplikację konsolową** w języku C# i nadaj jej następującą nazwę: *SqlDbConsoleApp*


Aby utworzyć bazę danych SQL w języku C#, załaduj wymagane biblioteki zarządzania (przy użyciu [konsoli menedżera pakietów](http://docs.nuget.org/Consume/Package-Manager-Console)):

1. Kliknij pozycję **Narzędzia** > **Menedżer pakietów NuGet** > **Konsola menedżera pakietów**.
2. Wpisz polecenie `Install-Package Microsoft.Azure.Management.Sql –Pre`, aby zainstalować [Bibliotekę zarządzania SQL platformy Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql).
3. Wpisz polecenie `Install-Package Microsoft.Azure.Management.ResourceManager –Pre`, aby zainstalować [Bibliotekę usługi Microsoft Azure Resource Manager](https://www.nuget.org/packages/Microsoft.Azure.Management.ResourceManager).
4. Wpisz polecenie `Install-Package Microsoft.Azure.Common.Authentication –Pre`, aby zainstalować [Wspólną bibliotekę uwierzytelniania platformy Microsoft Azure](https://www.nuget.org/packages/Microsoft.Azure.Common.Authentication). 



> [AZURE.NOTE] W przykładach podanych w tym artykule używane są synchroniczne formy poszczególnych żądań interfejsu API, które blokują działanie programu do momentu ukończenia wywołania REST na podległej usłudze. Dostępne są również metody asynchroniczne.


## Tworzenie serwera programu SQL Database, reguły zapory i bazy danych SQL — przykład w języku C#

W poniższym przykładzie tworzone są: grupa zasobów, serwer, reguła zapory i baza danych SQL. Zobacz [Tworzenie usługi podmiotu używanej do uzyskiwania dostępu do zasobów](#create-a-service-principal-to-access-resources), aby uzyskać zmienne `_subscriptionId, _tenantId, _applicationId, and _applicationSecret`.

Zastąp zawartość pliku **Program.cs** poniższym kodem i zaktualizuj elementy `{variables}` wartościami aplikacji (bez znaków `{}`).


    using Microsoft.Azure;
    using Microsoft.Azure.Management.ResourceManager;
    using Microsoft.Azure.Management.ResourceManager.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    
    namespace SqlDbConsoleApp
    {
    class Program
       {
        // For details about these four (4) values, see
        // https://azure.microsoft.com/documentation/articles/resource-group-authenticate-service-principal/
        static string _subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _tenantId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}";
        static string _applicationSecret = "{your-password}";

        // Create management clients for the Azure resources your app needs to work with.
        // This app works with Resource Groups, and Azure SQL Database.
        static ResourceManagementClient _resourceMgmtClient;
        static SqlManagementClient _sqlMgmtClient;

        // Authentication token
        static AuthenticationResult _token;

        // Azure resource variables
        static string _resourceGroupName = "{resource-group-name}";
        static string _resourceGrouplocation = "{Azure-region}";

        static string _serverlocation = _resourceGrouplocation;
        static string _serverName = "{server-name}";
        static string _serverAdmin = "{server-admin-login}";
        static string _serverAdminPassword = "{server-admin-password}";

        static string _firewallRuleName = "{firewall-rule-name}";
        static string _startIpAddress = "{0.0.0.0}";
        static string _endIpAddress = "{255.255.255.255}";

        static string _databaseName = "{dbfromcsarticle}";
        static string _databaseEdition = DatabaseEditions.Basic;
        static string _databasePerfLevel = ""; // "S0", "S1", and so on here for other tiers


        static void Main(string[] args)
        {
            // Authenticate:
            _token = GetToken(_tenantId, _applicationId, _applicationSecret);
            Console.WriteLine("Token acquired. Expires on:" + _token.ExpiresOn);

            // Instantiate management clients:
            _resourceMgmtClient = new ResourceManagementClient(new Microsoft.Rest.TokenCredentials(_token.AccessToken));
            _sqlMgmtClient = new SqlManagementClient(new TokenCloudCredentials(_subscriptionId, _token.AccessToken));


            Console.WriteLine("Resource group...");
            ResourceGroup rg = CreateOrUpdateResourceGroup(_resourceMgmtClient, _subscriptionId, _resourceGroupName, _resourceGrouplocation);
            Console.WriteLine("Resource group: " + rg.Id);


            Console.WriteLine("Server...");
            ServerGetResponse sgr = CreateOrUpdateServer(_sqlMgmtClient, _resourceGroupName, _serverlocation, _serverName, _serverAdmin, _serverAdminPassword);
            Console.WriteLine("Server: " + sgr.Server.Id);

            Console.WriteLine("Server firewall...");
            FirewallRuleGetResponse fwr = CreateOrUpdateFirewallRule(_sqlMgmtClient, _resourceGroupName, _serverName, _firewallRuleName, _startIpAddress, _endIpAddress);
            Console.WriteLine("Server firewall: " + fwr.FirewallRule.Id);

            Console.WriteLine("Database...");
            DatabaseCreateOrUpdateResponse dbr = CreateOrUpdateDatabase(_sqlMgmtClient, _resourceGroupName, _serverName, _databaseName, _databaseEdition, _databasePerfLevel);
            Console.WriteLine("Database: " + dbr.Database.Id);


            Console.WriteLine("Press any key to continue...");
            Console.ReadKey();
        }

        static ResourceGroup CreateOrUpdateResourceGroup(ResourceManagementClient resourceMgmtClient, string subscriptionId, string resourceGroupName, string resourceGroupLocation)
        {
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = resourceGroupLocation,
            };
            resourceMgmtClient.SubscriptionId = subscriptionId;
            ResourceGroup resourceGroupResult = resourceMgmtClient.ResourceGroups.CreateOrUpdate(resourceGroupName, resourceGroupParameters);
            return resourceGroupResult;
        }

        static ServerGetResponse CreateOrUpdateServer(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverLocation, string serverName, string serverAdmin, string serverAdminPassword)
        {
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = serverLocation,
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = serverAdmin,
                    AdministratorLoginPassword = serverAdminPassword,
                    Version = "12.0"
                }
            };
            ServerGetResponse serverResult = sqlMgmtClient.Servers.CreateOrUpdate(resourceGroupName, serverName, serverParameters);
            return serverResult;
        }


        static FirewallRuleGetResponse CreateOrUpdateFirewallRule(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string firewallRuleName, string startIpAddress, string endIpAddress)
        {
            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = startIpAddress,
                    EndIpAddress = endIpAddress
                }
            };
            FirewallRuleGetResponse firewallResult = sqlMgmtClient.FirewallRules.CreateOrUpdate(resourceGroupName, serverName, firewallRuleName, firewallParameters);
            return firewallResult;
        }



        static DatabaseCreateOrUpdateResponse CreateOrUpdateDatabase(SqlManagementClient sqlMgmtClient, string resourceGroupName, string serverName, string databaseName, string databaseEdition, string databasePerfLevel)
        {
            // Retrieve the server that will host this database
            Server currentServer = sqlMgmtClient.Servers.Get(resourceGroupName, serverName).Server;

            // Create a database: configure create or update parameters and properties explicitly
            DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentServer.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    CreateMode = DatabaseCreateMode.Default,
                    Edition = databaseEdition,
                    RequestedServiceObjectiveName = databasePerfLevel
                }
            };
            DatabaseCreateOrUpdateResponse dbResponse = sqlMgmtClient.Databases.CreateOrUpdate(resourceGroupName, serverName, databaseName, newDatabaseParameters);
            return dbResponse;
        }



        private static AuthenticationResult GetToken(string tenantId, string applicationId, string applicationSecret)
        {
            AuthenticationContext authContext = new AuthenticationContext("https://login.windows.net/" + tenantId);
            _token = authContext.AcquireToken("https://management.core.windows.net/", new ClientCredential(applicationId, applicationSecret));
            return _token;
        }
      }
    }





## Tworzenie usługi podmiotu używanej do uzyskiwania dostępu do zasobów

Poniższy skrypt środowiska PowerShell tworzy aplikację usługi Active Directory (AD) oraz jednostkę usługi wymaganą do uwierzytelnienia aplikacji w języku C#. Skrypt generuje wartości wyjściowe potrzebne w poprzednim przykładzie w języku C#. Aby uzyskać szczegółowe informacje, zobacz [Tworzenie usługi podmiotu używanej do uzyskiwania dostępu do zasobów przy użyciu programu Azure PowerShell](../resource-group-authenticate-service-principal.md).

   
    # Sign in to Azure.
    Add-AzureRmAccount
    
    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId
    
    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.
    
    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"
    
    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret
    
    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId
    
    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15
    
    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid
    
    
    # Output the values we need for our C# application to successfully authenticate
    
    Write-Output "Copy these values into the C# sample app"
    
    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret



## Następne kroki
Po wypróbowaniu usługi SQL Database i skonfigurowaniu bazy danych przy użyciu języka C# możesz przystąpić do czytania następujących artykułów:

- [Nawiązywanie połączenia z usługą SQL Database za pomocą programu SQL Server Management Studio i wykonywanie przykładowego zapytania T-SQL](sql-database-connect-query-ssms.md)

## Dodatkowe zasoby

- [SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)
- [Klasa bazy danych](https://msdn.microsoft.com/library/azure/microsoft.azure.management.sql.models.database.aspx)




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



<!--HONumber=Sep16_HO3-->


