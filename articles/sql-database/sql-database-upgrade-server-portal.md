<properties 
    pageTitle="Azure ポータルを使用した Azure SQL Database V12 へのアップグレード | Microsoft Azure" 
    description="Web および Business データベースのアップグレード方法を含む Azure SQL Database V12 へのアップグレード方法を説明します。また、Azure ポータルを使用してエラスティック データベース プールにデータベースを直接移行することで V11 サーバーをアップグレードする方法も説明します。" 
    services="sql-database" 
    documentationCenter="" 
    authors="stevestein" 
    manager="jeffreyg"
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-management" 
    ms.date="12/01/2015" 
    ms.author="sstein"/>


# Azure ポータルを使用した Azure SQL Database V12 へのアップグレード


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

さらへの移行、 [弾力性データベース プール](sql-database-elastic-pool.md) single database の個々 のパフォーマンス レベル (価格レベル) にアップグレードするよりコスト効率であることができます。 プールを使用してデータベース管理を簡略化することもできます。これは、個々のデータベースのパフォーマンス レベルを個別に管理するのではなく、プールのパフォーマンス設定を管理するだけでよいからです。 複数のサーバーにデータベースがある場合は、それらを同じサーバーに移動し、それらを 1 つのプールに置くと便利です。 簡単に [PowerShell を使用して、エラスティック データベース プールに直接 V11 サーバーからのデータベースの自動移行](sql-database-upgrade-server.md)します。 ポータルを使用して V11 データベースをプールに移行することもできますが、ポータルにはプールを作成するための V12 サーバーが既にある必要があります。 方向がある場合は、サーバーのアップグレード後、プールを作成するには、この記事の後半で提供される [プールから利点を得られるデータベース](sql-database-elastic-pool-guidance.md)します。

アップグレード操作全体にわたって、データベースはオンラインのままであり、機能し続けます。 新しいパフォーマンス レベルに実際に切り替えるときには、データベースへの接続が非常に短い時間、一時的に解除される場合があります。多くの場合、これは 90 秒程度ですが、最長で 5 分間ほど解除される場合もあります。 アプリケーションで [一時的な障害処理の接続の終了を](sql-database-connect-central-recommendations.md) し、アップグレードの終了時の接続解除に対して保護するだけで十分です。 

SQL Database V12 へのアップグレードを元に戻すことはできません。 アップグレードした後に、サーバーを V11 に戻すことはできません。 

V12 にアップグレードした後に [サービス レベルの提案](sql-database-service-tier-advisor.md) と [弾力性プールの推奨事項](sql-database-elastic-pool-portal.md#step-2-choose-a-pricing-tier) はすぐにするまでできない、サービスには、新しいサーバー上のワークロードを評価する時間。 V11 サーバーの推奨履歴は V12 サーバーには適用されないため、履歴は保持されません。  


## アップグレードの準備

- **すべての Web および Business データベースをアップグレード**: を参照してください [すべての Web および Business データベースをアップグレード](sql-database-upgrade-server-portal.md#upgrade-all-web-and-business-databases) 、以下の「またはを使用して [データベースとサーバーをアップグレードする PowerShell](sql-database-upgrade-server-powershell.md)します。
- **確認し、geo レプリケーションを中断**: geo レプリケーションの Azure SQL データベースが構成されている場合、現在の構成を文書化する必要があり、 [geo レプリケーションを停止する](sql-database-geo-replication-portal.md#remove-secondary-database)です。 アップグレードが完了したら、geo レプリケーション用にデータベースを再構成します。
- **Azure VM 上のクライアントがある場合は、これらのポートを開く**: クライアント プログラムは、Azure の仮想マシン (VM) に、クライアントの実行中に、SQL Database V12 に接続する場合は、11000 11999 と VM に 14000 14999 ポート範囲を開く必要があります。 詳細については、「 [SQL Database V12 用のポート](sql-database-develop-direct-route-ports-adonet-v12.md)します。



## アップグレードを開始する

1.  [Azure ポータル](http://portal.azure.com/) を選択してアップグレードするサーバーを参照する **すべてを参照** > **の SQL server**, 、目的のサーバーを選択するとします。
2. 選択 **の最新の SQL database update**, 選択してから、 **このサーバーをアップグレード**します。

      ![アップグレード server][1]

## すべての Web および Business データベースのアップグレード

サーバーに Web データベースまたは Business データベースがある場合は、それらをアップグレードする必要があります。 SQL Database V12 へのアップグレードの過程で、すべての Web/Business データベースを新しいサービス レベルに更新します。    

アップグレードする際、SQL Database サービスによって、各データベースに適したサービス層とパフォーマンス レベル (価格レベル) が推奨されます。 サービスでは、データベースの使用履歴を分析することで、既存データベースのワークロードを実行するための最適なレベルを使用することが推奨されます。 
    
3.  **このサーバーをアップグレード** 確認し、推奨される価格設定を選択するには、各データベースの階層にアップグレードするブレードを選択します。 利用できる価格レベルはいつでも参照でき、お使いの環境に最適な価格レベルを選択できます。


     ![databases][2]


7. 提案されたレベルをクリックした後に表示されます、 **価格レベルを選択** ブレードで、レベルを選択し、をクリックして、 **選択** ] ボタンをクリックすると、そのレベルに変更します。 Web データベースまたは Business データベースごとに新しいレベルを選択します。

    SQL Database は特定のサービス レベルやパフォーマンス レベルに固定されるわけではないことに注意してください。このため、データベースの要件の変化に応じて、使用可能なサービス レベルとパフォーマンス レベル間での変更は簡単に行えます。 実際、Basic、Standard、Premium の SQL Database は時間単位で課金され、各データベースは 24 時間に 4 回スケールアップまたはスケールダウンすることができます。

    ![推奨][6]


サーバー上のすべてのデータベースを処理した後、アップグレードを開始できます。

## アップグレードの確認

3. サーバー上のすべてのデータベース処理する必要があるアップグレードに適合される **型のサーバー名** クリックしてアップグレードを実行することを確認する **OK**。 

    ![アップグレードを確認する][3]


4. アップグレードが開始され、進行中であることを通知するメッセージが表示されます。 アップグレード処理が始まります。 お使いのデータベースの詳細によっては、V12 へのアップグレードに時間がかかることがあります。 その間、サーバー上のすべてのデータベースはオンラインのままですが、サーバーとデータベースの管理アクションは制限されます。

    ![アップグレードが進行中][4]

    新しいパフォーマンス レベルに実際に切り替えるときには、データベースへの接続が非常に短い時間 (通常は数秒)、一時的に解除される場合があります。 接続が終了した場合に発生する一時的なエラーに対処するための機能 (再試行ロジック) がアプリケーションに備わっている場合は、アップグレードの終了に発生する接続解除に備えるだけで十分です。 

5. アップグレード操作が完了した後、 **最新の更新プログラム** ブレードが表示されます **有効**します。 

    ![有効になっている V12][5]  

## エラスティック データベース プールへのデータベースの移動

 [Azure ポータル](https://portal.azure.com/) V12 サーバーを参照してクリックして **プールの追加**します。

または

メッセージを表示する場合は **ここをクリックすると、このサーバーの推奨エラスティック データベース プールを表示する**, 、簡単にサーバーのデータベース用に最適化されているプールを作成する] をクリックします。 詳細については、「 [推奨エラスティック データベース プール](sql-database-elastic-pool-portal.md#recommended-elastic-database-pools)します。

![サーバー プールの追加][7]
   
手順に従い、 [弾力性データベース プールを作成する](sql-database-elastic-pool.md) 資料、プールの作成を完了します。

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

- [エラスティック データベース プールの推奨事項の確認](sql-database-elastic-pool-portal.md#recommended-elastic-database-pools)します。
- [エラスティック データベース プールを作成する](sql-database-elastic-pool-portal.md) し、一部またはすべてのデータベースをプールに追加します。
- [データベースのサービス階層とパフォーマンス レベルを変更する](sql-database-scale-up.md)です。



## 関連リンク

- [SQL Database V12 の新機能](sql-database-v12-whats-new.md)
- [SQL Database V12 へのアップグレードの計画と準備をする](sql-database-v12-plan-prepare-upgrade.md)


<!--Image references-->
[1]: ./media/sql-database-upgrade-server-portal/latest-sql-database-update.png
[2]: ./media/sql-database-upgrade-server-portal/upgrade-server2.png
[3]: ./media/sql-database-upgrade-server-portal/upgrade-server3.png
[4]: ./media/sql-database-upgrade-server-portal/online-during-upgrade.png
[5]: ./media/sql-database-upgrade-server-portal/enabled.png
[6]: ./media/sql-database-upgrade-server-portal/recommendations.png
[7]: ./media/sql-database-upgrade-server-portal/new-elastic-pool.png





