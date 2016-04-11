<properties 
    pageTitle="PowerShell を使用した Azure SQL Database の管理" 
    description="PowerShell を使用した Azure SQL Database の管理。" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jeffreyg" 
    editor="monicar"/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="sstein"/>

# PowerShell を使用した Azure SQL Database の管理


> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-manage-portal.md)
- [Transact SQL (SSMS)](sql-database-manage-azure-ssms.md)
- [PowerShell](sql-database-command-line-tools.md)

このトピックでは、多数の Azure SQL Database タスクを実行する PowerShell コマンドについて説明します。


PowerShell コマンドレットを実行するには、Azure PowerShell をインストールし、実行している必要があります。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。



## 資格情報の構成

Azure サブスクリプションに PowerShell コマンドレットを実行するには、Azure アカウントにまずアクセスできるようにする必要があります。 次を実行すると資格情報を入力するサインイン画面が表示されます。 Azure クラシック ポータルへのサインインに使用しているものと同じ電子メールとパスワードを使用します。

    Add-AzureRmAccount

正常にサインインすると、サインインしている ID や使用中の Azure サブスクリプションを含む情報が画面に表示されます。


## Azure サブスクリプションを選択します。

使用するサブスクリプションを選択する、サブスクリプション Id を作成する必要があります (**- SubscriptionId**) とサブスクリプション名 (**-subscriptionname**)。 前の手順からコピーするか、複数のサブスクリプションがある場合は、実行、 **Get-azuresubscription** コマンドレット、結果セットから目的のサブスクリプション情報をコピーします。

現在のサブスクリプションを設定するには、サブスクリプション情報と共に次のコマンドレットを実行します。

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

上で選択したサブスクリプションに対し、次のコマンドが実行されます。

## リソース グループの作成

サーバーを含むリソース グループを作成します。 任意の有効な場所を使用するには、次のコマンドを編集します。 

有効な Azure SQL Database サーバーの場所一覧については、次のコマンドレットを実行します。

    $AzureSQLLocations = (Get-AzureRmResourceProvider -ListAvailable | Where-Object {$_.ProviderNamespace -eq 'Microsoft.Sql'}).Locations

既にリソース グループがある場合、サーバーの作成に進みます。次のコマンドを実行して新しいリソース グループを作ることもできます。

    New-AzureRmResourceGroup -Name "resourcegroupJapanWest" -Location "Japan West"

## サーバーの作成 

新しい V12 サーバーの作成、 [新規 AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603715.aspx) コマンドレットです。 server12 をご利用のサーバー名に置き換えます。 サーバー名が既に使われている場合はエラーが発生する可能性があるため、Azure SQL Server で一意のサーバー名を使用する必要があります。 このコマンドは完了するまでに数分かかる場合があることに注意してください。 サーバーが正常に作成されると、サーバーの詳細と PowerShell のプロンプトが表示されます。 有効な場所を使用するコマンドを編集することができます。

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -Location "Japan West" -ServerVersion "12.0"

入力を求めるこのコマンドを実行すると、ウィンドウが開き、 **ユーザー名** と **パスワード**します。 これは、Azure 資格情報ではありません、ユーザー名を入力し、新しいサーバーを作成する管理者の資格情報となるパスワードします。

## サーバー ファイアウォール規則の作成

サーバーの使用にアクセスするファイアウォール ルールを作成する、 [新規 AzureRmSqlServerFirewallRule](https://msdn.microsoft.com/library/azure/mt603860.aspx) コマンドです。 ご利用のクライアントで有効な開始 IP アドレスと終了 IP アドレスに置き換え、次のコマンドを実行します。

他の Azure サービスへのアクセスを許可するように、サーバーが必要な場合は、追加、 **- AllowAllAzureIPs** 特別なファイアウォール規則を追加し、サーバーへのすべての azure トラフィック アクセスを許可するスイッチです。

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -FirewallRuleName "clientFirewallRule1" -StartIpAddress "192.168.0.198" -EndIpAddress "192.168.0.199"

詳細については、次を参照してください。 [Azure SQL Database ファイアウォール](https://msdn.microsoft.com/library/azure/ee621782.aspx)します。

## SQL Database の作成

使用してデータベースを作成する、 [新規 AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619339.aspx) コマンドです。 データベースを作成するには、サーバーが必要です。 次の例では TestDB12 という名前の SQL Database を作成します。 このデータベースは、Standard S1 データベースとして作成されます。

    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12" -Edition Standard -RequestedServiceObjectiveName "S1"


## SQL Database のパフォーマンス レベルを変更します

データベースをスケール アップまたはダウンで、 [セット AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619433.aspx) コマンドです。 次の例では、TestDB12 という名前の SQL Database を、現在のパフォーマンス レベルから Standard S3 レベルにスケールアップします。

    Set-AzureRmSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12" -Edition Standard -RequestedServiceObjectiveName "S3"


## SQL Database の削除

SQL データベースを削除する、 [削除 AzureRmSqlDatabase](https://msdn.microsoft.com/library/azure/mt619368.aspx) コマンドです。 次の例では TestDB12 という名前の SQL Database を削除します。

    Remove-AzureRmSqlDatabase -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12" -DatabaseName "TestDB12"

## サーバーの削除

持つサーバーを削除することも、 [削除 AzureRmSqlServer](https://msdn.microsoft.com/library/azure/mt603488.aspx) コマンドです。 次の例では server12 という名前のサーバーを削除します。

    Remove-AzureRmSqlServer -ResourceGroupName "resourcegroupJapanWest" -ServerName "server12"



これらの Azure SQL リソースまたは同様のリソースを再作成する場合は、次のことができます。 

- これを PowerShell スクリプト ファイル (*.ps1) として保存します
- Azure クラシック ポータルの [Automation] セクションに、これを Azure Automation Runbook として保存します 

## 次のステップ

コマンドを結合し、自動化します。 たとえば、置換を含む引用符内のすべての < および > サーバー、ファイアウォール規則、およびデータベースを作成するため、値付きの文字。


    New-AzureRmResourceGroup -Name "<resourceGroupName>" -Location "<Location>"
    New-AzureRmSqlServer -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -Location "<Location>" -ServerVersion "12.0"
    New-AzureRmSqlServerFirewallRule -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -FirewallRuleName "<firewallRuleName>" -StartIpAddress "<192.168.0.198>" -EndIpAddress "<192.168.0.199>"
    New-AzureRmSqlDatabase -ResourceGroupName "<resourceGroupName>" -ServerName "<serverName>" -DatabaseName "<databaseName>" -Edition <Standard> -RequestedServiceObjectiveName "<S1>"

## 関連情報

- [Azure SQL Database コマンドレット](https://msdn.microsoft.com/library/azure/mt574084.aspx)
