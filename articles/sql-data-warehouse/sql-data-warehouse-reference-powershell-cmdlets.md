<properties
   pageTitle="SQL Data Warehouse でのコマンドレットの概要"
   description="PowerShell コマンドレットを使用した SQL Data Warehouse の一時停止と再開"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sidneyh"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="12/15/2015"
   ms.author="twounder;sidneyh;barbkess"/>

# Azure Data Warehouse コマンドレットと REST API の概要

SQL Data Warehouse は、Azure PowerShell コマンドレットまたは REST API を使用して管理できます。 

に対して定義されたコマンド **Azure SQL Database** にも使用 **SQL Data Warehouse**します。 現在の一覧については、次を参照してください。 [Azure SQL コマンドレット](https://msdn.microsoft.com/library/mt574084.aspx)します。 コマンドレットは、 **Suspend-azuresqldatabase** と **Resume-azuresqldatabase** (以下参照) は SQL Data Warehouse 用に設計された追加します。

同様に、REST Api を **SQL Azure データベース** にも使用 **SQL Data Warehouse** インスタンス。 現在の一覧については、次を参照してください。 [Azure SQL データベースの操作](https://msdn.microsoft.com/library/azure/dn505719.aspx)します。

## Azure PowerShell コマンドレットの取得と実行

1. Azure PowerShell モジュールをダウンロードするには、実行 [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)します。 
2. ウィンドウのスタートアップの種類で、モジュールを実行する **Microsoft Azure PowerShell**します。
3. コンピューターにアカウントをまだ追加していない場合は、次のコマンドレットを実行します  (詳細については、次を参照してください [。インストールして、Azure PowerShell を構成する方法]():

        Add-AzureAccount
3. 次のコマンドレットを使用してモードを切り替えます。

        Switch-AzureMode AzureResourceManager

## Suspend-AzureSqlDatabase
### 例 1: サーバー上で名前によってデータベースを一時停止する

この例では、"Server01" という名前のサーバーにホストされている "Database02" という名前のデータベースが一時停止されます。 サーバーは "ResourceGroup1" という名前の Asure リソース グループ内にあります。

    Suspend-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName
    "Server01" –DatabaseName "Database02"

### 例 2: データベース オブジェクトを一時停止する

この例では、"ResourceGroup1" という名前のリソース グループに含まれている "Server01" という名前のサーバーから "Database02" という名前のデータベースが取得されます。 取得したオブジェクトをパイプを使用 **Suspend-azuresqldatabase**します。 その結果、データベースが一時停止されます。

    $database = Get-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
    $resultDatabase = $database | Suspend-AzureSqlDatabase

## Resume-AzureSqlDatabase

### 例 1: サーバー上で名前によってデータベースを再開する

この例では、"Server01" という名前のサーバーにホストされている "Database02" という名前のデータベースが再開されます。 サーバーは "ResourceGroup1" という名前のリソース グループ内にあります。

    Resume-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"

### 例 2: データベース オブジェクトの再開

この例では、"ResourceGroup1" という名前のリソース グループに含まれている "Server01" という名前のサーバーから "Database02" という名前のデータベースが取得されます。 オブジェクトをパイプ **Resume-azuresqldatabase**します。 

    $database = Get-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
    $resultDatabase = $database | Resume-AzureSqlDatabase

## Get-AzureSqlDatabaseRestorePoints

このコマンドレットは、Azure SQL Database のバックアップ復元ポイントを一覧表示します。 復元ポイントは、データベースの復元に使用されます。
返されるオブジェクトのプロパティは、次のとおりです。

プロパティ|説明
---|---
RestorePointType|DISCRETE / CONTINUOUS. 不連続復元ポイントは、Azure SQL Database を復元できる特定の時点を示します。 連続復元ポイントは、Azure SQL Database を復元できる最も早い時点を示します。 データベースは、最も早い時点以降の任意の時点に復元できます。
EarliestRestoreDate|最も早い復元時間 (restorePointType = CONTINUOUS のときに入力)
RestorePointCreationDate |バックアップのスナップショット時間 (restorePointType = DISCRETE のときに入力)

### 例 1: サーバー上で名前によってデータベースの復元ポイントを取得する
この例では、"ResourceGroup1" という名前のリソース グループに含まれている "Server01" という名前のサーバーから "Database02" という名前のデータベースの復元ポイントが取得されます。

    $restorePoints = Get-AzureSqlDatabaseRestorePoints –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"



### 例 2: データベース オブジェクトの再開

この例では、"ResourceGroup1" という名前のリソース グループに含まれている "Server01" という名前のサーバーから "Database02" という名前のデータベースが取得されます。 データベース オブジェクトをパイプ **Get-azuresqldatabase**, 、され、結果は、データベースの復元ポイント。

    $database = Get-AzureSqlDatabase –ResourceGroupName "ResourceGroup11" –ServerName "Server01" –DatabaseName "Database02"
    $restorePoints = $database | Get-AzureSqlDatabaseRestorePoints



> [AZURE.NOTE] サーバーが、foo.database.windows.net である場合は、powershell コマンドレットで、サーバー名として"foo"を使用するに注意してください。


## 次のステップ
参照の詳細については、次を参照してください。 [SQL Data Warehouse のリファレンス概要][]します。

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse reference overview]: sql-data-warehouse-overview-reference.md
[How to install and configure Azure PowerShell]: ../articles/powershell-install-configure.md

<!--MSDN references-->


<!--Other Web references-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/
[msn]: http://search.msn.com/


