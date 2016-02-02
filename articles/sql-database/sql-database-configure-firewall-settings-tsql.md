<properties
    pageTitle="方法: ファイアウォール設定を構成する | Microsoft Azure"
    description="Azure SQL データベースにアクセスする IP アドレス用のファイアウォールの構成方法を説明します。"
    services="sql-database"
    documentationCenter=""
    authors="BYHAM"
    manager="jeffreyg"
    editor=""/>


<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article" 
    ms.date="11/13/2015"
    ms.author="rickbyh"/>



# 方法: TSQL を使用してファイアウォール設定を構成する (SQL Database)

> [AZURE.SELECTOR]
- [Azure Portal](sql-database-configure-firewall-settings.md)
- [TSQL](sql-database-configure-firewall-settings-tsql.md)
- [PowerShell](sql-database-configure-firewall-settings-powershell.md)
- [REST API](sql-database-configure-firewall-settings-rest.md)



Microsoft Azure SQL Database では、サーバーとデータベースの接続許可に、ファイアウォール規則を使用します。 データベースへのアクセスを選択的に許可するには、Azure SQL Database サーバーのマスター データベースまたはユーザー データベースに、サーバーレベルおよびデータベースレベルのファイアウォール設定を定義します。
> [AZURE.IMPORTANT] Azure のアプリケーションからデータベース サーバーに接続を許可するには、Azure の接続を有効にする必要があります。 詳細については、ファイアウォール規則と Azure からの接続を有効にすると、次を参照してください。 [Azure SQL Database ファイアウォール](sql-database-firewall-configure.md)します。 Azure クラウド境界内で接続を行う場合は、追加の TCP ポートをいくつか開かなければならない場合があります。 詳細については、次を参照してください、 **SQL Database の V12: 外部と内部** の [1433 for ADO.NET 4.5 と SQL Database V12 における 1433 以外のポート](sql-database-develop-direct-route-ports-adonet-v12.md)。


## Transact-SQL を使用してサーバー レベルのファイアウォール規則を管理する

1. クラシック ポータルまたは SQL Server Management Studio を使用して、クエリ ウィンドウを起動します。
2. マスター データベースに接続していることを確認します。
3. サーバーレベルのファイアウォール規則は、クエリ ウィンドウから選択、作成、更新、または削除することができます。
4. サーバーレベルのファイアウォール規則を更新または作成するには、sp_set_firewall rule ストアド プロシージャを実行します。 次の例では、サーバー Contoso の範囲の IP アドレスを使用します。<br/>まず、どのようなルールが既に存在します。

        SELECT * FROM sys.firewall_rules ORDER BY name;

    次に、ファイアウォール規則を追加します。

        EXECUTE sp_set_firewall_rule @name = N'ContosoFirewallRule',
            @start_ip_address = '192.168.1.1', @end_ip_address = '192.168.1.10'

    サーバーレベルのファイアウォール規則を削除するには、sp_delete_firewall_rule ストアド プロシージャを実行します。 次の例では、ContosoFirewallRule という名前の規則を削除します。

        EXECUTE sp_delete_firewall_rule @name = N'ContosoFirewallRule'



## データベース レベルのファイアウォール規則

1. サーバーレベルのファイアウォールを IP アドレスで作成した後、クラシック ポータルまたは SQL Server Management Studio からクエリ ウィンドウを起動します。
2. データベースレベルのファイアウォール規則を作成するデータベースに接続します。

    既存のデータベースレベルのファイアウォール規則を新規作成または更新するには、sp_set_database_firewall_rule ストアド プロシージャを実行します。 次の例では ContosoFirewallRule という名前の新しいファイアウォール規則を作成します。

        EXEC sp_set_database_firewall_rule @name = N'ContosoFirewallRule', @start_ip_address = '192.168.1.11', @end_ip_address = '192.168.1.11'

    既存のデータベースレベルのファイアウォール規則を削除するには、sp_delete_database_firewall_rule ストアド プロシージャを実行します。 次の例では、ContosoFirewallRule という名前の規則を削除します。

        EXEC sp_delete_database_firewall_rule @name = N'ContosoFirewallRule'



## 次のステップ

Database の作成に関するチュートリアルについては、次を参照してください。 [最初の Azure SQL Database を作成する](sql-database-get-started.md)します。
オープン ソースまたはサードパーティ製のアプリケーションから Azure SQL database に接続する方法の詳細については、次を参照してください。 [プログラムで SQL データベースを Azure に接続するためのガイドライン](https://msdn.microsoft.com/library/azure/ee336282.aspx)します。
データベースを参照してください] に移動する方法を理解する [Azure SQL Database におけるデータベースの管理とログイン](https://msdn.microsoft.com/library/azure/ee336235.aspx)します。






