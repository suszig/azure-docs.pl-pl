<properties 
    pageTitle="PowerShell を使用した Azure SQL Database V12 へのアップグレード | Microsoft Azure" 
    description="Web および Business データベースのアップグレード方法を含む Azure SQL Database V12 へのアップグレード方法を説明します。また、PowerShell を使用してエラスティック データベース プールにデータベースを直接移行することで V11 サーバーをアップグレードする方法も説明します。" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.workload="data-management" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/01/2015" 
    ms.author="sstein"/>

# PowerShell を使用した Azure SQL Database V12 へのアップグレード


> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-upgrade-server-portal.md)
- [PowerShell](sql-database-upgrade-server-powershell.md)


SQL Database V12 が最新バージョンであるため、SQL Database V12 にアップグレードすることをお勧めします。
SQL データベース V12 が多く [以前のバージョンに比べて利点があります](sql-database-v12-whats-new.md) など。

- SQL Server との互換性の強化。
- Premium のパフォーマンスの向上と新しいパフォーマンス レベル。
- [エラスティック データベース プール](sql-database-elastic-pool.md)します。

この記事では、既存の SQL Database V11 サーバーとデータベースを SQL Database V12 にアップグレードする方法について説明します。 

V12 にアップグレードする過程で、すべての Web および Business データベースを新しいサービス レベルにアップグレードするため、Web および Business データベースのアップグレードに関する説明も含まれています。 

さらへの移行、 [弾力性データベース プール](sql-database-elastic-pool.md) single database の個々 のパフォーマンス レベル (価格レベル) にアップグレードするよりコスト効率であることができます。 プールを使用してデータベース管理を簡略化することもできます。これは、個々のデータベースのパフォーマンス レベルを個別に管理するのではなく、プールのパフォーマンス設定を管理するだけでよいからです。 複数のサーバーにデータベースがある場合は、それらを同じサーバーに移動し、それらを 1 つのプールに置くと便利です。

この記事で説明する手順に従うと、V11 サーバーからエラスティック データベース プールにデータベースを簡単に自動移行できます。 

アップグレード操作全体にわたって、データベースはオンラインのままであり、機能し続けます。 新しいパフォーマンス レベルに実際に切り替えるときには、データベースへの接続が非常に短い時間、一時的に解除される場合があります。多くの場合、これは 90 秒程度ですが、最長で 5 分間ほど解除される場合もあります。 アプリケーションで [一時的な障害処理の接続の終了を](sql-database-connect-central-recommendations.md) し、アップグレードの終了時の接続解除に対して保護するだけで十分です。 

SQL Database V12 へのアップグレードを元に戻すことはできません。 アップグレードした後に、サーバーを V11 に戻すことはできません。 

V12 にアップグレードした後に [サービス レベルの提案](sql-database-service-tier-advisor.md) と [弾力性プールの推奨事項](sql-database-elastic-pool-portal.md#step-2-choose-a-pricing-tier) はすぐにするまでできない、サービスには、新しいサーバー上のワークロードを評価する時間。 V11 サーバーの推奨履歴は V12 サーバーには適用されないため、履歴は保持されません。  

## アップグレードの準備

- **すべての Web および Business データベースをアップグレード**: を参照してください [すべての Web および Business データベースをアップグレード](sql-database-v12-upgrade.md#upgrade-all-web-and-business-databases) 、以下の「またはを使用して [データベースとサーバーをアップグレードする PowerShell](sql-database-upgrade-server-powershell.md)します。
- **確認し、geo レプリケーションを中断**: geo レプリケーションの Azure SQL データベースが構成されている場合、現在の構成を文書化する必要があり、 [geo レプリケーションを停止する](sql-database-geo-replication-portal.md#remove-secondary-database)です。 アップグレードが完了したら、geo レプリケーション用にデータベースを再構成します。
- **Azure VM 上のクライアントがある場合は、これらのポートを開く**: クライアント プログラムは、Azure の仮想マシン (VM) に、クライアントの実行中に、SQL Database V12 に接続する場合は、11000 11999 と VM に 14000 14999 ポート範囲を開く必要があります。 詳細については、「 [SQL Database V12 用のポート](sql-database-develop-direct-route-ports-adonet-v12.md)します。


## 前提条件 

PowerShell を使用した V12 へのサーバーをアップグレードするには、Azure PowerShell をインストールして、実行している必要があり、バージョンによっては、Azure リソース マネージャーの PowerShell コマンドレットにアクセスするリソース マネージャー モードに切り替える必要があります。 

PowerShell コマンドレットを実行するには、Azure PowerShell をインストールし、実行している必要があります。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。


## 資格情報を構成してサブスクリプションを選択

Azure サブスクリプションに PowerShell コマンドレットを実行するには、Azure アカウントにまずアクセスできるようにする必要があります。 次を実行すると資格情報を入力するサインイン画面が表示されます。 Azure ポータルへのサインインに使用しているものと同じ電子メールとパスワードを使用します。

    Add-AzureRmAccount

正常にサインインすると、サインインしている ID や使用中の Azure サブスクリプションを含む情報が画面に表示されます。

使用するサブスクリプションを選択する、サブスクリプション Id を作成する必要があります (**- SubscriptionId**) とサブスクリプション名 (**-subscriptionname**)。 前の手順からコピーするか、複数のサブスクリプションがある場合は、実行、 **Get AzureRmSubscription** コマンドレット、結果セットから目的のサブスクリプション情報をコピーします。

現在のサブスクリプションを設定するには、サブスクリプション情報と共に次のコマンドレットを実行します。

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

上で選択したサブスクリプションに対し、次のコマンドが実行されます。

## 推奨の取得

サーバーのアップグレードに関する推奨設定を取得するには、次のコマンドレットを実行します。 

    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName “resourcegroup1” -ServerName “server1” 

詳細については、次を参照してください。 [Azure SQL Database エラスティック データベース プールの推奨事項](sql-database-elastic-pool-portal.md#elastic-database-pool-pricing-tier-recommendations) と [picing レベルの提案を Azure SQL Database](sql-database-service-tier-advisor.md)します。 



## アップグレードを開始する

サーバーのアップグレードを開始するには、次のコマンドレットを実行します。 

    Start-AzureRmSqlServerUpgrade -ResourceGroupName “resourcegroup1” -ServerName “server1” -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


このコマンドを実行すると、アップグレード プロセスが開始されます。 推奨設定の出力をカスタマイズし、編集した推奨設定をこのコマンドレットに提供できます。 


## サーバーをアップグレードする


    # Adding the account
    #
    Add-AzureRmAccount
    
    # Setting the variables
    #
    $SubscriptionName = 'YOUR_SUBSCRIPTION' 
    $ResourceGroupName = 'YOUR_RESOURCE_GROUP' 
    $ServerName = 'YOUR_SERVER' 
    
    # Selecting the right subscription 
    # 
    Select-AzureRmSubscription -SubscriptionName $SubscriptionName 
    
    # Getting the upgrade recommendations 
    #
    $hint = Get-AzureRmSqlServerUpgradeHint -ResourceGroupName $ResourceGroupName -ServerName $ServerName 
    
    # Starting the upgrade process 
    #
    Start-AzureRmSqlServerUpgrade -ResourceGroupName $ResourceGroupName -ServerName $ServerName -ServerVersion 12.0 -DatabaseCollection $hint.Databases -ElasticPoolCollection $hint.ElasticPools  


## カスタム アップグレードのマッピング

推奨設定がお使いのサーバーとビジネス ケースに適していない場合は、データベースをどのようにアップグレードするかを選択でき、それらを単一のデータベースまたはエラスティック データベースにマップできます。

ElasticPoolCollection と DatabaseCollection parameters は省略可能です。
    
    # Creating elastic pool mapping
    #
    $elasticPool = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeRecommendedElasticPoolProperties 
    $elasticPool.DatabaseDtuMax = 100 
    $elasticPool.DatabaseDtuMin = 0 
    $elasticPool.Dtu = 800 
    $elasticPool.Edition = "Standard" 
    $elasticPool.DatabaseCollection = ("DB1", “DB2”, “DB3”, “DB4”) 
    $elasticPool.Name = "elasticpool_1" 
    $elasticPool.StorageMb = 800 
     
    # Creating single database mapping for 2 databases. DBMain1 mapped to S0 and DBMain2 mapped to S2
    #
    $databaseMap1 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties 
    $databaseMap1.Name = "DBMain1" 
    $databaseMap1.TargetEdition = "Standard" 
    $databaseMap1.TargetServiceLevelObjective = "S0"
    
    $databaseMap2 = New-Object -TypeName Microsoft.Azure.Management.Sql.Models.UpgradeDatabaseProperties 
    $databaseMap2.Name = "DBMain2" 
    $databaseMap2.TargetEdition = "Standard" 
    $databaseMap2.TargetServiceLevelObjective = "S2"
     
    # Starting the upgrade
    #
    Start-AzureRmSqlServerUpgrade –ResourceGroupName resourcegroup1 –ServerName server1 -Version 12.0 -DatabaseCollection @($databaseMap1, $databaseMap2) -ElasticPoolCollection @($elasticPool) 

    

## SQL Database V12 へのアップグレード後のデータベースの監視


アップグレードした後は、アプリケーションが希望のパフォーマンスで実行されていることを確認し、必要に応じて最適化できるように、データベースを積極的に監視することをお勧めします。 

エラスティック データベース プールを監視する個々 のデータベースの監視だけでなく [ポータルを使用して](sql-database-elastic-pool-portal.md#monitor-and-manage-an-elastic-database-pool) または [PowerShell](sql-database-elastic-pool-powershell.md#monitoring-elastic-databases-and-elastic-database-pools) 


**リソース消費データ:** Basic、Standard、および Premium のデータベース リソース消費データをで使用できる、 [sys.dm_ db _ resource_stats](http://msdn.microsoft.com/library/azure/dn800981.aspx) ユーザー データベースで DMV。 この DMV は、直近 1 時間の操作に関する 15 秒おきのほぼリアル タイムのリソース消費情報を提供します。 1 つの間隔内の DTU 消費割合は、CPU、IO、ログにおける最大消費割合として算出されます。 次は、過去 1 時間における DTU の平均消費割合を算出するクエリです。

    SELECT end_time
         , (SELECT Max(v)
             FROM (VALUES (avg_cpu_percent)
                         , (avg_data_io_percent)
                         , (avg_log_write_percent)
           ) AS value(v)) AS [avg_DTU_percent]
    FROM sys.dm_db_resource_stats
    ORDER BY end_time DESC;

その他の監視情報

- [Single database の azure SQL データベース パフォーマンス ガイダンス](http://msdn.microsoft.com/library/azure/dn369873.aspx)します。
- [エラスティック データベース プールの価格およびパフォーマンスに関する考慮事項](sql-database=elastic-pool-guidance.md)します。
- [動的管理ビューを使用した Azure SQL Database の監視](sql-database-monitoring-with-dmvs.md)



**警告:** にアップグレードしたデータベースの DTU 消費が特定の高レベルに近づいたときに通知する、Azure ポータルで 'Alerts' を設定します。 データベース アラートは、Azure ポータルで、DTU、CPU、IO、ログといったさまざまなパフォーマンス指標に対して設定できます。 データベースとを選択するには、「参照」 **アラート ルール** で、 **設定** ブレードです。

たとえば、DTU の平均割合の値が 5 分間にわたって 75% を超過したような場合に、「DTU 割合」に関する電子メール アラートを送るように設定できます。 参照してください [警告通知を受け取る](insights-receive-alert-notifications.md) をアラートの通知を構成する方法の詳細を参照してください。



## 次のステップ

- [エラスティック データベース プールを作成する](sql-database-elastic-pool-portal.md) し、一部またはすべてのデータベースをプールに追加します。
- [データベースのサービス階層とパフォーマンス レベルを変更する](sql-database-scale-up.md)です。



## 関連情報

- [Get-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603582.aspx)
- [Start-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt619403.aspx)
- [Stop-AzureRmSqlServerUpgrade](https://msdn.microsoft.com/library/azure/mt603589.aspx)


