<properties 
    pageTitle="PowerShell を使用した新しい SQL Database の設定 | Microsoft Azure" 
    description="PowerShell で新しい SQL Database を作成する方法について説明します。 PowerShell コマンドレットを使用して、一般的なデータベース設定タスクを管理できます。" 
    keywords="create new sql database,database setup"
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jeffreyg" 
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management" 
    ms.date="12/01/2015"
    ms.author="sstein"/>

# 新しい SQL Database を作成し、PowerShell コマンドレットで一般的なデータベース設定タスクを実行します。 

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-get-started.md)
- [C#](sql-database-get-started-csharp.md)
- [PowerShell](sql-database-get-started-powershell.md)


新しい SQL Database を作成し、PowerShell コマンドレットを使用して一般的なデータベース設定タスクを実行します。


PowerShell コマンドレットを実行するには、Azure PowerShell をインストールし、実行している必要があります。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。

- をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。


## 資格情報を構成してサブスクリプションを選択

これで、Azure リソース マネージャー モジュールが実行され、SQL データベースを作成するために必要なすべてのコマンドレットにアクセスできるようになりました。 

まず、Azure アカウントへのアクセスを確立させるため、以下のコマンドレットを実行してください。資格情報を入力するサインイン画面が表示されます。 Azure ポータルへのサインインに使用しているものと同じ電子メールとパスワードを使用します。

    Add-AzureRmAccount

正常にサインインすると、サインインしている ID や使用中の Azure サブスクリプションを含む情報が画面に表示されます。


### Azure サブスクリプションを選択します。

サブスクリプションを選択するには、サブスクリプション ID が必要です。 前の手順からコピーするか、複数のサブスクリプションがある場合は、実行、 **Get AzureRmSubscription** コマンドレット、結果セットから目的のサブスクリプション情報をコピーします。 ご利用のサブスクリプションを取得したら次のコマンドレットを実行します。

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

正常に実行した後 **選択 AzureRmSubscription** プロンプト、PowerShell に戻されます。 1 つ以上のサブスクリプションがある場合は、実行 **Get AzureRmSubscription** サブスクリプションの表示を使用することを確認および **IsCurrent: True**します。

## データベースの設定: リソース グループ、サーバー、ファイアウォール規則を作成する

これで選択した Azure サブスクリプションに対してコマンドレットを実行する準備ができましたので、次にデータベースを作成するサーバーを含むリソース グループを確立します。 次のコマンドを編集して選択した任意の有効な場所で使用できます。 実行 **(Get AzureRmLocation | where-object {$_ です。名前を付けます-eq"Microsoft.Sql/servers"})。場所** 有効な場所の一覧を取得します。

次のコマンドを実行して、新しいリソース グループを作成します。

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "West US"

新しいリソース グループを正常に作成した後を含む画面に情報を表示する **ProvisioningState: が成功した**します。


### サーバーの作成 

SQL Database は Azure SQL Database サーバーの内部で作成されます。 実行 **新規 AzureRmSqlServer** 新しいサーバーを作成します。 ServerName をご利用のサーバー名に置き換えます。 サーバー名が既に使われている場合はエラーが発生する可能性があるため、すべての Azure SQL Server で一意のサーバー名を使用する必要があります。 このコマンドは完了するまでに数分かかる場合があることに注意してください。 コマンドを編集して有効なロケーションを選択して使用できますが、前の手順でのリソース グループ作成時に使用したロケーションと同じものでなければなりません。

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "West US" -ServerVersion "12.0"

入力を求めるこのコマンドを実行すると、ウィンドウが開き、 **ユーザー名** と **パスワード**します。 これは、Azure 資格情報ではありません、ユーザー名を入力し、新しいサーバーを作成する管理者の資格情報となるパスワードします。

サーバーが正常に作成されると、サーバーの詳細が表示されます。

### サーバーのファイアウォール規則を構成し、サーバーへのアクセスを許可します。

ファイアウォール規則を確立してサーバーにアクセスします。 ご利用のコンピューターで有効な開始 IP アドレスと終了 IP アドレスに置き換え、次のコマンドを実行します。

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

規則が正常に作成されると、ファイアウォール規則の詳細が表示されます。

他の Azure サービスによるサーバーへのアクセスを許可するには、ファイアウォール規則を追加し、StartIpAddress と EndIpAddress を 0.0.0.0 に設定します。 これにより、任意の Azure サブスクリプションからの Azure トラフィックがサーバーへアクセスできるようになる点に注意してください。

詳細については、次を参照してください。 [Azure SQL Database ファイアウォール](sql-database-firewall-configure.md)します。


## 新しい SQL データベースの作成

これでリソース グループ、サーバー、ファイアウォール規則が構成され、サーバーにアクセスできるようになりました。

次のコマンドは、S1 パフォーマンス レベル、Standard (標準) サービス階層で新しい (空の) SQL データベースを作成します。


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


データベースが正常に作成されると、データベースの詳細が表示されます。

## 新しい SQL Database PowerShell スクリプトを作成する

    $SubscriptionId = "4cac86b0-1e56-bbbb-aaaa-000000000000"
    $ResourceGroupName = "resourcegroupname"
    $Location = "Japan West"
    
    $ServerName = "uniqueservername"
    
    $FirewallRuleName = "rule1"
    $FirewallStartIP = "192.168.0.0"
    $FirewallEndIp = "192.168.0.0"
    
    $DatabaseName = "database1"
    $DatabaseEdition = "Standard"
    $DatabasePerfomanceLevel = "S1"
    
    
    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId
    
    $ResourceGroup = New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
    
    $Server = New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName -ServerName $ServerName -Location $Location -ServerVersion "12.0"
    
    $FirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $ServerName -FirewallRuleName $FirewallRuleName -StartIpAddress $FirewallStartIP -EndIpAddress $FirewallEndIp
    
    $SqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -Edition $DatabaseEdition -RequestedServiceObjectiveName $DatabasePerfomanceLevel
    
    $SqlDatabase
    


## 次のステップ
新しい SQL Database を作成し、基本的なデータベース設定タスクを実行したら、次の手順を実行します。

- [SQL Server Management Studio (SSMS) での接続](sql-database-connect-to-database.md)


## その他のリソース

- [Azure SQL データベース](https://azure.microsoft.com/documentation/services/sql-database/)

