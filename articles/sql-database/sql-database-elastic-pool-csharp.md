<properties
    pageTitle="C# データベース開発: エラスティック データベース プール | Microsoft Azure"
    description="多数のデータベース全体でリソースを共有できるように、C# データベース開発手法を使用して Azure SQL Database エラスティック データベース プールを作成します。"
    services="sql-database"
    keywords="c# database,sql development"
    documentationCenter=""
    authors="stevestein"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="12/01/2015"
    ms.author="sstein"/>

# C&#x23; データベース開発: SQL データベースのエラスティック データベース プールの作成と構成

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-elastic-pool-portal.md)
- [C#](sql-database-elastic-pool-csharp.md)
- [PowerShell](sql-database-elastic-pool-powershell.md)


この記事は、作成する方法を示します、 [弾力性データベース プール](sql-database-elastic-pool.md) c# を使用してアプリケーションからデータベースを sql には、データベースの開発手法です。

> [AZURE.NOTE] エラスティック データベース プールは現在プレビューで SQL Database V12 サーバーでのみ使用できます。 SQL Database V11 サーバーがある場合は、 [V12 にアップグレードし、プールを作成する PowerShell を使用して](sql-database-upgrade-server.md) で 1 つの手順です。

例を使用して、 [Azure SQL Database Library for .NET](https://www.nuget.org/packages/Microsoft.Azure.Management.Sql)します。
各コード スニペットはわかりやすさを重視し、細かく分けて説明しています。また、サンプルのコンソール アプリケーションのすべてのコマンドは、この記事の下部にまとめられています。

Azure SQL Database Library for .NET の提供、 [Azure リソース マネージャー](resource-group-overview.md)-ベースの API をラップする、 [SQL Database REST API のリソース マネージャー ベース](https://msdn.microsoft.com/library/azure/mt163571.aspx)します。 このクライアント ライブラリは、リソース マネージャー ベースのクライアント ライブラリの一般的なパターンに従います。 リソース マネージャーは、リソース グループ、および認証を受ける必要があります。 [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (AAD)。

<br>

> [AZURE.NOTE] SQL Database Library for .NET は現在プレビュー段階にします。

<br>

Azure サブスクリプションがないをクリックするだけ **無料評価版** これの上部にあるページ、およびこの記事に戻る。 Visual Studio の無償コピーは、次を参照してください。 および、 [Visual Studio のダウンロード](https://www.visualstudio.com/downloads/download-visual-studio-vs) ページです。

## 必要なライブラリのインストール

使用して次のパッケージをインストールすることによって必要な管理ライブラリの入手、 [パッケージ マネージャー コンソール](http://docs.nuget.org/Consume/Package-Manager-Console) SQL での開発用。

    Install-Package Microsoft.Azure.Management.Sql –Pre
    Install-Package Microsoft.Azure.Management.Resources –Pre
    Install-Package Microsoft.Azure.Common.Authentication –Pre


## Azure Active Directory による認証の構成

C# で SQL の開発を開始する前に、Azure ポータルでいくつかのタスクを実行する必要があります。 まず、必要な認証を設定して、アプリケーションで REST API にアクセスできるようにする必要があります。

 [Azure リソース マネージャー REST Api](https://msdn.microsoft.com/library/azure/dn948464.aspx) 、以前の Azure サービス管理 REST Api によって使用される証明書ではなく、認証の Azure Active Directory を使用します。

現在のユーザーに基づいてクライアント アプリケーションを認証するには、まず、Azure のリソースが作成されたサブスクリプションに関連付けられている AAD ドメインにアプリケーションを登録する必要があります。 職場アカウントまたは学校アカウントではなく、Microsoft アカウントで Azure サブスクリプションが作成されている場合は、既定の AAD ドメインが既に存在します。 アプリケーションの登録を行う、 [旧ポータル](https://manage.windowsazure.com/)します。

新しいアプリケーションを作成し、適切な Active Directory に登録するには、以下を実行します。

1. 検索を左側にあるメニューをスクロール、 **Active Directory** サービスを提供し、開きます。

    ![C# SQL データベース開発: Active Directory の設定][1]

2. アプリケーションを認証し、このボタンをクリックするディレクトリを選択の **名前**します。

    ![ディレクトリを選択します。][4]

3. [ディレクトリ] ページをクリックして **アプリケーション**します。

    ![[アプリケーション] をクリックします。][5]

4. クリックして **追加** 新しいアプリケーションを作成します。

    ![[追加] ボタンをクリックします。C# アプリケーションを作成します。][6]

5. 選択 **[組織が開発中のアプリケーションを追加**します。

5. 提供、 **名前** アプリ、および選択 **ネイティブ クライアント アプリケーション**します。

    ![アプリケーションの追加][7]

6. 提供、 **リダイレクト URI**します。 実際のエンドポイントである必要はありませんが、有効な URI である必要があります。

    ![アプリケーションの追加][8]

7. アプリの作成 [完了] をクリックして **構成**, 、コピー、 **クライアント ID** (クライアント id は、コードでは必要されます)。

    ![クライアント ID の取得][9]


1. ページの下部にあるをクリックして **アプリケーション追加**します。
1. 選択 **Microsoft Apps**します。
1. 選択 **Azure サービス管理 API**, 、して、ウィザードを完了します。
2. API とを選択した状態を選択すると、この API にアクセスするために必要な特定のアクセス許可を付与する必要があります **Access Azure サービス管理 (プレビュー)**します。

    ![アクセス許可の設定][2]

2. クリックして **保存**します。



### ドメイン名の指定

コードにはドメイン名が必要です。 適切なドメイン名を指定する簡単な方法を以下に示します。

1. 移動して、 [Azure ポータル](https://portal.azure.com)します。
2. 右上隅の自分の名前にマウスを合わせ、ポップアップ ウィンドウに表示されるドメインをメモしてください。 置換 **domain.onmicrosoft.com** 、アカウントの値を持つ次のコード スニペットにします。

    ![ドメイン名の指定][3]



**その他の AAD リソース**  

認証に Azure Active Directory の使用に関する追加情報 [この便利なブログ投稿](http://www.cloudidentity.com/blog/2013/09/12/active-directory-authentication-library-adal-v1-for-net-general-availability/)します。


### 現在のユーザーのアクセス トークンの取得

クライアント アプリケーションでは、現在のユーザーのアプリケーション アクセス トークンを取得する必要があります。 ユーザーが最初にコードを実行すると、ユーザーの資格情報を入力するよう求められ、作成されたトークンがローカルにキャッシュされます。 以降の実行では、キャッシュからトークンが取得されます。トークンの有効期限が切れている場合のみログインが必要になります。


    /// <summary>
    /// Prompts for user credentials when first run or if the cached credentials have expired.
    /// </summary>
    /// <returns>The access token from AAD.</returns>
    private static AuthenticationResult GetAccessToken()
    {
        AuthenticationContext authContext = new AuthenticationContext
            ("https://login.windows.net/" /* AAD URI */
                + "domain.onmicrosoft.com" /* Tenant ID or AAD domain */);

        AuthenticationResult token = authContext.AcquireToken
            ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                "aa00a0a0-a0a0-0000-0a00-a0a00000a0aa" /* application client ID from AAD*/,
        new Uri("urn:ietf:wg:oauth:2.0:oob") /* redirect URI */,
        PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

        return token;
    }



> [AZURE.NOTE] この記事の例では、各 API 要求の同期フォームを使用し、残りの部分の完了が、基になるサービスに対する呼び出しになるまでブロックします。 非同期の手法も利用できます。



## リソース グループの作成

リソース マネージャーでは、すべてのリソースをリソース グループに作成する必要があります。 リソース グループは、1 つのアプリケーションの関連リソースを保持するコンテナーです。 エラスティック データベース プールを作成するには、既存のリソース グループに、Azure SQL Database サーバーが必要です。 次の C# コードを実行して、リソース グループを作成します。


    // Create a resource management client
    ResourceManagementClient resourceClient = new ResourceManagementClient(new TokenCloudCredentials("XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" /*subscription id*/, token.AccessToken ));

    // Resource group parameters
    ResourceGroup resourceGroupParameters = new ResourceGroup()
    {
        Location = "South Central US"
    };

    //Create a resource group
    var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate("resourcegroup-name", resourceGroupParameters);



## サーバーの作成

エラスティック データベース プールは、Azure SQL Database サーバーに格納されるため、次の手順ではサーバーを作成します。 サーバー名がすでに使われている場合はエラーが発生する可能性があるため、すべての Azure SQL Server でグローバルに一意のサーバー名を使用する必要があります。 このコマンドは完了するまでに数分かかる場合があることに注意してください。 アプリケーションがサーバーに接続できるようにするには、サーバーにファイアウォール規則を作成し、クライアントの IP アドレスからのアクセスを開く必要があります。


    // Create a SQL Database management client
    SqlManagementClient sqlClient = new SqlManagementClient(new TokenCloudCredentials("XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" /* Subscription id*/, token.AccessToken));

    // Create a server
    ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
    {
        Location = "South Central US",
        Properties = new ServerCreateOrUpdateProperties()
        {
            AdministratorLogin = "ServerAdmin",
            AdministratorLoginPassword = "P@ssword1",
            Version = "12.0"
        }
    };

    var serverResult = sqlClient.Servers.CreateOrUpdate("resourcegroup-name", "server-name", serverParameters);




## サーバーのファイアウォール規則を作成し、サーバーへのアクセスを許可します。

既定では、サーバーにはファイアウォール規則がないので、いかなる場所からも接続することはできません。 または、サーバー上のデータベース サーバーに接続するために、 [ファイアウォール規則](sql-database-firewall-configure.md) 定義する必要がある IP アドレス、クライアントからのアクセスを許可します。

次の例では、任意の IP アドレスからサーバーへのアクセスを開くサーバー ファイアウォール規則を作成します。 データベースをセキュリティで保護し、侵入に対する基本的な対策としてファイアウォール規則に依存しないよう、適切な SQL ログインおよびパスワードを作成することをお勧めします。 詳細については、「 [Azure SQL Database におけるデータベースとログインを管理する](sql-database-manage-logins.md)です。


    // Create a firewall rule on the server to allow TDS connection
    FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
    {
        Properties = new FirewallRuleCreateOrUpdateProperties()
        {
            StartIpAddress = "0.0.0.0",
            EndIpAddress = "255.255.255.255"
        }
    };

    var firewallResult = sqlClient.FirewallRules.CreateOrUpdate("resourcegroup-name", "server-name", "FirewallRule1", firewallParameters);




他の Azure サービスによるサーバーへのアクセスを許可するには、ファイアウォール規則を追加し、StartIpAddress と EndIpAddress を 0.0.0.0 に設定します。 これによりからのトラフィックを Azure に注意してください *、* サーバーにアクセスする Azure サブスクリプション。


## データベースの作成

次の例では、新しい Basic データベースを作成します。サーバーに同じ名前のデータベースが存在する場合は、既存のデータベースが更新されます。

        // Create a database

        // Retrieve the server on which the database will be created
        Server currentServer = sqlClient.Servers.Get("resourcegroup-name", "server-name").Server;

        // Create a database: configure create or update parameters and properties explicitly
        DatabaseCreateOrUpdateParameters newDatabaseParameters = new DatabaseCreateOrUpdateParameters()
        {
            Location = currentServer.Location,
            Properties = new DatabaseCreateOrUpdateProperties()
            {
                Edition = "Basic",
                RequestedServiceObjectiveName = "Basic",
                MaxSizeBytes = 2147483648,
                Collation = "SQL_Latin1_General_CP1_CI_AS"
            }
        };

        var dbResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", newDatabaseParameters);



## エラスティック データベース プールの作成

次の例では、新しいエラスティック データベース プールが作成されます。



    // Create elastic pool: configure create or update parameters and properties explicitly
    ElasticPoolCreateOrUpdateParameters newPoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = "South Central US",
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = "Standard",
            Dtu = 100,  // alternatively set StorageMB, if both are specified they must agree based on the eDTU:storage ratio of the edition
            DatabaseDtuMin = 0,
            DatabaseDtuMax = 100
         }
    };

    // Create the pool
    var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);


## エラスティック データベース プールの更新

次の例では、既存のエラスティック データベース プールのパフォーマンス特性が更新されます。

    // Retrieve existing pool properties
    var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

    // Configure create or update parameters with existing property values, override those to be changed.
    ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
    {
        Location = currentPool.Location,
        Properties = new ElasticPoolCreateOrUpdateProperties()
        {
            Edition = currentPool.Properties.Edition,
            DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
            DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
            Dtu = (int)currentPool.Properties.Dtu,
            StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
        }
    };

    newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);



## 既存のデータベースをエラスティック データベース プールに移動

*プールの作成後に TRANSACT-SQL プールとの間の既存のデータベースを移動するために使用することもできます。 詳細情報を参照してください、 [Transact SQL の弾力性データベース プール参照](sql-database-elastic-pool-reference.md#Transact-SQL)します。*

次の例では、既存の Azure SQL Database がプールに移動されます。


    // Update database service objective to add the database to a pool

    // Retrieve current database properties
    currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

    // Configure create or update parameters with existing property values, override those to be changed.
    DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
    {
        Location = currentDatabase.Location,
        Properties = new DatabaseCreateOrUpdateProperties()
        {
            Edition = "Standard",
            RequestedServiceObjectiveName = "ElasticPool",
            ElasticPoolName = "ElasticPool1",
            MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
            Collation = currentDatabase.Properties.Collation,
        }
    };

    // Update the database
    var dbUpdateResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", updatePooledDbParameters);




## エラスティック データベース プールでの新しいデータベースの作成

*プールを作成した後、プールで新しい弾力性データベースを作成する TRANSACT-SQL を使用することもできます。 詳細情報を参照してください、 [Transact SQL の弾力性データベース プール参照](sql-database-elastic-pool-reference.md#Transact-SQL)します。*

次の例では、プールに直接、新しいデータベースが作成されます。


    // Create a new database in the pool

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



## エラスティック データベース プールのすべてのデータベースの一覧表示

次の例では、プール内のすべてのデータベースが一覧表示されます。

    //List databases in the elastic pool
    DatabaseListResponse dbListInPool = sqlClient.ElasticPools.ListDatabases("resourcegroup-name", "server-name", "ElasticPool1");
    Console.WriteLine("Databases in Elastic Pool {0}", "server-name.ElasticPool1");
    foreach (Database db in dbListInPool)
    {
        Console.WriteLine("  Database {0}", db.Name);
    }




## コンソール アプリケーションのサンプル


    using Microsoft.Azure;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.Azure.Management.Sql;
    using Microsoft.Azure.Management.Sql.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using System;
    using System.Security;

    namespace AzureSqlDatabaseRestApiExamples
    {
    class Program
    {
        /// <summary>
        /// Prompts for user credentials when first run or if the cached credentials have expired.
        /// </summary>
        /// <returns>The access token from AAD.</returns>
        private static AuthenticationResult GetAccessToken()
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" /* AAD URI */
                + "domain.onmicrosoft.com" /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken
                ("https://management.azure.com/"/* the Azure Resource Management endpoint */,
                "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" /* application client ID from AAD*/,
                new Uri("urn:ietf:wg:oauth:2.0:oob") /* redirect URI */,
                PromptBehavior.Auto /* with Auto user will not be prompted if an unexpired token is cached */);

            return token;
        }

        private static AuthenticationResult GetAccessTokenUsingUserCredentials(UserCredential userCredential)
        {
            AuthenticationContext authContext = new AuthenticationContext
                ("https://login.windows.net/" /* AAD URI */
                + "YOU.onmicrosoft.com" /* Tenant ID or AAD domain */);

            AuthenticationResult token = authContext.AcquireToken(
                "https://management.azure.com/"/* the Azure Resource Management endpoint */,
                "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" /* application client ID from AAD*/,
                userCredential);

            return token;
        }
        private static SecureString convertToSecureString(string secret)
        {
            var secureStr = new SecureString();
            if (secret.Length > 0)
            {
                foreach (var c in secret.ToCharArray()) secureStr.AppendChar(c);
            }
            return secureStr;
        }

        static void Main(string[] args)
        {
            var token = GetAccessToken();

            // Who am I?
            Console.WriteLine("Identity is {0} {1}", token.UserInfo.GivenName, token.UserInfo.FamilyName);
            Console.WriteLine("Token expires on {0}", token.ExpiresOn);
            Console.WriteLine("");

            // Create a resource management client
            ResourceManagementClient resourceClient = new ResourceManagementClient(new TokenCloudCredentials("XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" /*subscription id*/, token.AccessToken));

            // Resource group parameters
            ResourceGroup resourceGroupParameters = new ResourceGroup()
            {
                Location = "South Central US"
            };

            //Create a resource group
            var resourceGroupResult = resourceClient.ResourceGroups.CreateOrUpdate("resourcegroup-name", resourceGroupParameters);

            Console.WriteLine("Resource group {0} create or update completed with status code {1} ", resourceGroupResult.ResourceGroup.Name, resourceGroupResult.StatusCode);

            //create a SQL Database management client
            TokenCloudCredentials tokenCredentials = new TokenCloudCredentials("XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" /* Subscription id*/, token.AccessToken);

            SqlManagementClient sqlClient = new SqlManagementClient(tokenCredentials);

            // Create a server
            ServerCreateOrUpdateParameters serverParameters = new ServerCreateOrUpdateParameters()
            {
                Location = "South Central US",
                Properties = new ServerCreateOrUpdateProperties()
                {
                    AdministratorLogin = "ServerAdmin",
                    AdministratorLoginPassword = "P@ssword1",
                    Version = "12.0"
                }
            };

            var serverResult = sqlClient.Servers.CreateOrUpdate("resourcegroup-name", "server-name", serverParameters);

            var serverGetResult = sqlClient.Servers.Get("resourcegroup-name", "server-name");


            Console.WriteLine("Server {0} create or update completed with status code {1}", serverResult.Server.Name, serverResult.StatusCode);

            // Create a firewall rule on the server to allow TDS connection

            FirewallRuleCreateOrUpdateParameters firewallParameters = new FirewallRuleCreateOrUpdateParameters()
            {
                Properties = new FirewallRuleCreateOrUpdateProperties()
                {
                    StartIpAddress = "0.0.0.0",
                    EndIpAddress = "255.255.255.255"
                }
            };

            var firewallResult = sqlClient.FirewallRules.CreateOrUpdate("resourcegroup-name", "server-name", "FirewallRule1", firewallParameters);

            Console.WriteLine("Firewall rule {0} create or update completed with status code {1}", firewallResult.FirewallRule.Name, firewallResult.StatusCode);


            var dbResponse = sqlClient.Databases.CreateOrUpdate("resourcegroup-name", "server-name", "Database1", newDatabaseParameters);

            Console.WriteLine("Database {0} create or update completed with status code {1}. Service Objective {2} ", dbResponse.Database.Name, dbResponse.StatusCode, dbResponse.Database.Properties.ServiceObjective);


           // Create an elastic database pool
            var newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);

            Console.WriteLine("Elastic pool {0} create or update completed with status code {1}.", newPoolResponse.ElasticPool.Name, newPoolResponse.StatusCode);

            // Update pool: retrieve existing pool properties
            var currentPool = sqlClient.ElasticPools.Get("resourcegroup-name", "server-name", "ElasticPool1").ElasticPool;

            // Update pool: configure create or update parameters with existing property values, override those to be changed.
            ElasticPoolCreateOrUpdateParameters updatePoolParameters = new ElasticPoolCreateOrUpdateParameters()
            {
                Location = currentPool.Location,
                Properties = new ElasticPoolCreateOrUpdateProperties()
                {
                    Edition = currentPool.Properties.Edition,
                    DatabaseDtuMax = 50, /* Setting DatabaseDtuMax to 50 limits the eDTUs that any one database can consume */
                    DatabaseDtuMin = 10, /* Setting DatabaseDtuMin above 0 limits the number of databases that can be stored in the pool */
                    Dtu = (int)currentPool.Properties.Dtu,
                    StorageMB = currentPool.Properties.StorageMB,  /* For a Standard pool there is 1 GB of storage per eDTU. */
                }
            };
            newPoolResponse = sqlClient.ElasticPools.CreateOrUpdate("resourcegroup-name", "server-name", "ElasticPool1", newPoolParameters);

            Console.WriteLine("Elastic pool {0} create or update completed with status code {1}.", newPoolResponse.ElasticPool.Name, newPoolResponse.StatusCode);

            // Update a databases service objective to add the database to a pool

            // Update database: retrieve current database properties
            currentDatabase = sqlClient.Databases.Get("resourcegroup-name", "server-name", "Database1").Database;

            // Update database: configure create or update parameters with existing property values, override those to be changed.
            DatabaseCreateOrUpdateParameters updatePooledDbParameters = new DatabaseCreateOrUpdateParameters()
            {
                Location = currentDatabase.Location,
                Properties = new DatabaseCreateOrUpdateProperties()
                {
                    Edition = "Standard",
                    RequestedServiceObjectiveName = "ElasticPool",
                    ElasticPoolName = "ElasticPool1",
                    MaxSizeBytes = currentDatabase.Properties.MaxSizeBytes,
                    Collation = currentDatabase.Properties.Collation,
                }
            };


            // Create a new database in the pool

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

            Console.WriteLine("Database {0} create or update completed with status code {1}. Service Objective: {2}({3}) ", poolDbResponse.Database.Name, poolDbResponse.StatusCode, poolDbResponse.Database.Properties.ServiceObjective, poolDbResponse.Database.Properties.ElasticPoolName);
      }
    }







## その他のリソース


[SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)

[Azure リソース管理 API](https://msdn.microsoft.com/library/azure/dn948464.aspx)

[エラスティック データベース プールのリファレンス](sql-database-elastic-pool-reference.md)します。


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-csharp/aad.png
[2]: ./media/sql-database-elastic-pool-csharp/permissions.png
[3]: ./media/sql-database-elastic-pool-csharp/getdomain.png
[4]: ./media/sql-database-elastic-pool-csharp/aad2.png
[5]: ./media/sql-database-elastic-pool-csharp/aad-applications.png
[6]: ./media/sql-database-elastic-pool-csharp/add.png
[7]: ./media/sql-database-elastic-pool-csharp/add-application.png
[8]: ./media/sql-database-elastic-pool-csharp/add-application2.png
[9]: ./media/sql-database-elastic-pool-csharp/clientid.png


