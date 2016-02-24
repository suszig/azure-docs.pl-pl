<properties 
    pageTitle="PowerShell を使用して Azure SQL Database の geo レプリケーションを構成する | Microsoft Azure" 
    description="PowerShell を使用した Azure SQL Database の geo レプリケーション" 
    services="sql-database" 
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

# PowerShell を使用して Azure SQL Database の geo レプリケーションを構成する



> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-geo-replication-portal.md)
- [PowerShell](sql-database-geo-replication-powershell.md)
- [Transact-SQL](sql-database-geo-replication-transact-sql.md)


この記事では、PowerShell を使用して SQL Database の geo レプリケーションを構成する方法を示します。

geo レプリケーションでは、さまざまなデータ センターの場所 (リージョン) に最大 4 つのレプリカ (セカンダリ) データベースを作成することができます。 セカンダリ データベースは、データ センターで障害が発生した場合やプライマリ データベースに接続できない場合に使用できます。

geo レプリケーションは、Standard データベースと Premium データベースにのみ使用できます。 

Standard データベースでは、読み取り不可のセカンダリ データベースを 1 つ置くことができますが、その場合は推奨されたリージョンを使用する必要があります。 Premium データベースでは、利用可能な任意のリージョンに、最大 4 つの読み取り可能なセカンダリ データベースを置くことができます。

geo レプリケーションを構成するには、次のものが必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database データベース。別の地理的リージョンにレプリケートするプライマリ データベースです。
- Azure PowerShell 1.0 以降。 ダウンロードして、Azure PowerShell モジュールをインストール [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。




## 資格情報を構成してサブスクリプションを選択

まず、Azure アカウントへのアクセスを確立する必要があるため、PowerShell を起動してから以下のコマンドレットを実行します。 ログイン画面で、Azure ポータルへのサインインに使用しているものと同じ電子メールとパスワードを入力します。


    Login-AzureRmAccount

正常にサインインすると、サインインしている ID や使用中の Azure サブスクリプションを含む情報が画面に表示されます。


### Azure サブスクリプションを選択します。

サブスクリプションを選択するには、サブスクリプション ID が必要です。 サブスクリプション Id をコピーするには、前の手順から表示された情報からもサブスクリプションが複数あり、詳細情報を必要な場合は、実行、 **Get AzureRmSubscription** コマンドレット、結果セットから目的のサブスクリプション情報をコピーします。 次のコマンドレットでサブスクリプション ID を使用し、現在のサブスクリプションを設定します。

    Select-AzureRmSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

正常に実行した後 **選択 AzureRmSubscription** プロンプト、PowerShell に戻されます。



## セカンダリ データベースの追加


次の手順では、geo レプリケーション パートナーシップに新しいセカンダリ データベースを作成します。  
  
セカンダリ データベースを有効にするには、サブスクリプションの所有者または共同所有者でなければなりません。 

使用することができます、 **新規 AzureRmSqlDatabaseSecondary** コマンドレットはパートナー サーバー上のセカンダリ データベースを先となるサーバー上のローカル データベースに追加するには、(プライマリ データベース) が接続されています。 

このコマンドレットでは置換 **Start-azuresqldatabasecopy** で、 **– IsContinuous** パラメーター。  出力には、 **AzureRmSqlDatabaseSecondary** 明確に特定のレプリケーション リンクを識別するために他のコマンドレットが使用できるオブジェクトです。 このコマンドレットは、セカンダリ データベースが作成され、そのシード処理が完全に行われると、戻ります。 データベースのサイズによって所要時間は異なり、数分 ～ 数時間の幅があります。

セカンダリ サーバー上のレプリケート データベースは、プライマリ サーバー上のデータベースと同じ名前となります。既定でのサービス レベルもプライマリ サーバー上のデータベースの場合と同じになります。 セカンダリ データベースは、読み取り可能または読み取り不可とすることができるほか、単一データベースまたはエラスティック データベースとすることができます。 詳細については、次を参照してください。 [新規 AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603689.aspx) と [サービス階層](sql-database-service-tiers.md)します。
セカンダリ データベースを作成しシード処理を行うと、プライマリ データベースから新しいセカンダリ データベースへのデータのレプリケートが開始されます。 次の手順では、PowerShell を使用してこの操作を行い、単一のデータベースまたはエラスティック データベースとして読み取り不可または読み取り可能なセカンダリ データベースを作成する方法を説明します。

パートナー データベースが既に存在する場合 (たとえば、前の geo レプリケーション リレーションシップを終了した結果として)、コマンドは失敗します。



### 読み取り不可のセカンダリ データベース (単一のデータベース) の追加

次のコマンドでは、リソース グループ "rg2" に属するサーバー "srv2" のデータベース "mydb" に対して読み取り不可のセカンダリ データベースが作成されます。

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "None"



### 読み取り可能なセカンダリ (単一データベース) の追加

次のコマンドでは、リソース グループ "rg2" に属するサーバー "srv2" のデータベース "mydb" に対して読み取り可能なセカンダリ データベースが作成されます。

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" -AllowConnections "All"




### 読み取り不可のセカンダリ データベース (エラスティック データベース) の追加

次のコマンドでは、リソース グループ "rg2" に属するサーバー "srv2" のエラスティック データベース プール "ElasticPool1" 内のデータベース "mydb" に対して読み取り不可のセカンダリ データベースが作成されます。

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "None"


### 読み取り可能なセカンダリ データベース (エラスティック データベース) の追加

次のコマンドでは、リソース グループ "rg2" に属するサーバー "srv2" のエラスティック データベース プール "ElasticPool1" 内のデータベース "mydb" に対して読み取り可能なセカンダリ データベースが作成されます。

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb"
    $secondaryLink = $database | New-AzureRmSqlDatabaseSecondary –PartnerResourceGroupName "rg2" –PartnerServerName "srv2" –SecondaryElasticPoolName "ElasticPool1" -AllowConnections "All"





## セカンダリ データベースを削除する

使用して、 **削除 AzureRmSqlDatabaseSecondary** コマンドレットをセカンダリ データベースとそのプライマリ間のレプリケーション パートナーシップを完全に終了します。 リレーションシップの終了後、セカンダリ データベースは読み取り/書き込みデータベースになります。 セカンダリ データベースへの接続が切断された場合、コマンドは成功します。ただし、接続が復元されると、セカンダリ データベースは読み取り/書き込みデータベースになります。 詳細については、次を参照してください。 [削除 AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt603457.aspx) と [サービス階層](sql-database-service-tiers.md)します。

このコマンドレットは、レプリケーション用の Stop-AzureSqlDatabaseCopy に取って代わります。 

この削除は強制終了の場合と同じです。強制終了では、レプリケーション リンクが削除され、前のセカンダリ データベースは、終了前に完全にレプリケートされるとは限らないスタンドアロン データベースとして残されます。 前のプライマリ データベースと前のセカンダリ データベースが利用可能な場合は、その両方ですべてのリンク データがクリーンアップされます。 このコマンドレットは、レプリケーション リンクが削除されると戻ります。 


セカンダリ データベースを削除するユーザーは、RBAC に従ってプライマリ データベースとセカンダリ データベースの両方への書き込みアクセス権を有する必要があります。 詳細については、ロールベースのアクセス制御を参照してください。

次のコードを実行すると、"mydb" という名前のデータベースと、リソース グループ "rg2" に属するサーバー "srv2" とのレプリケーション リンクが削除されます。 

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb"
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –SecondaryResourceGroup "rg2" –PartnerServerName "srv2"
    $secondaryLink | Remove-AzureRmSqlDatabaseSecondary 




## 計画されたフェールオーバーの開始

使用して、 **セット AzureRmSqlDatabaseSecondary** コマンドレットと、 **フェールオーバー** セカンダリに既存のプライマリに降格する、新しいプライマリ データベースがセカンダリ データベースを昇格するパラメーターです。 この機能は、障害復旧の訓練時など、計画されたフェールオーバー用に設計されたものであり、プライマリ データベースを使用できることが必要条件となります。

コマンドによって、次のワークフローが実行されます。

1. 一時的にレプリケーションを同期モードに切り替えます。 これにより、すべての未完了のトランザクションがセカンダリ データベースにフラッシュされます。

2. geo レプリケーション パートナーシップで 2 つのデータベースのロールを切り替えます。  

このシーケンスによってデータが失われることはありません。 ロールの切り替え中に、わずかですが両方のデータベースが使用できなくなる期間 (0 ～ 25 秒程度) が生じます。 通常の状況では、操作全体が完了するのに 1 分かかりません。 詳細については、次を参照してください。 [セット AzureRmSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx)します。


> [AZURE.NOTE] プライマリ データベースが使用できない場合、コマンドが発行されるときに、プライマリ サーバーが使用できないことを示すエラー メッセージで失敗します。 まれに、操作が完了しないで、スタックしたような状態になる場合があります。 この場合、ユーザーは強制フェールオーバー コマンド (計画されていないフェールオーバー) を呼び出すことができますが、データは失われることを容認する必要があります。



このコマンドレットは、セカンダリ データベースをプライマリに切り替えるプロセスが完了すると、戻ります。

次のコマンドでは、リソース グループ "rg2" に属するサーバー "srv2" 上の "mydb" という名前のデータベースのロールをプライマリに切り替えます。 "db2" の接続先である元のプライマリは、2 つのデータベースが完全に同期すると、セカンダリに切り替わります。

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb” –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary -Failover



## プライマリ データベースからセカンダリ データベースへの計画されていないフェールオーバーを開始します。


使用することができます、 **セット AzureRmSqlDatabaseSecondary** コマンドレットと **– フェールオーバー** と **-allowdataloss** 強制的にセカンダリをプライマリのデータベースが不要になった使用時に既存のプライマリの降格、計画されていない方法で新しいプライマリ データベースがセカンダリ データベースを昇格するパラメーターです。

この機能は、データベースの可用性を復元することが重要で、多少のデータ損失は許容されるような障害復旧を目的として設計されています。 強制フェールオーバーが呼び出されると、指定したセカンダリ データベースはすぐにプライマリ データベースになり、書き込みトランザクションの受け入れを開始します。 強制フェールオーバー操作の後、元のプライマリ データベースとこの新しいプライマリ データベースとの再接続が可能になるとすぐに、元のプライマリ データベース上で増分バックアップが行われます。古いプライマリ データベースは新しいプライマリ データベースのセカンダリ データベースとされ、以後、新しいプライマリ データベースの単なるレプリカとなります。

ただし、セカンダリ データベース上でポイントインタイム リストアはサポートされていないので、古いプライマリ データベースにコミットされているデータのうち、新しいプライマリ データベースにレプリケートされていないものを復旧する場合は、CSS を有効にして既知のログ バックアップにデータベースを復元する必要があります。

> [AZURE.NOTE] 場合は、コマンドを発行して、プライマリとセカンダリの両方がオンラインになって、古いプライマリは、新しいセカンダリになりますが、一部のデータ損失が発生するので、データの同期は試行されません。


プライマリ データベースに複数のセカンダリ データベースがある場合、コマンドは部分的に成功します。 コマンドが実行されたセカンダリ データベースがプライマリ データベースになります。 ただし、古いプライマリ データベースはプライマリ データベースのままです。すなわち、2 つのプライマリ データベースは不整合の状態になり、中断されたレプリケーション リンクによって接続されます。 ユーザーは、この 2 つのプライマリ データベースのいずれかで “remove secondary” API を使用して、この構成を手動で修復する必要があります。


次のコマンドでは、プライマリが利用できない場合に、"mydb" という名前のデータベースのロールをプライマリに切り替えます。 "mydb" の接続先であった元のプライマリ データベースは、それがオンラインに戻ると、セカンダリ データベースに切り替わります。 その時点で、同期化によってデータが失われる可能性があります。

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb” –ResourceGroupName "rg2” –ServerName "srv2”
    $database | Set-AzureRmSqlDatabaseSecondary –Failover -AllowDataLoss



## geo レプリケーションの構成と正常性を監視する

監視タスクには、geo レプリケーションの構成に関する監視と、データ レプリケーションの正常性に関する監視が含まれます。  

[Get AzureRmSqlDatabaseReplicationLink](https://msdn.microsoft.com/library/mt619330.aspx) sys.geo_replication_links カタログ ビューでの転送用のレプリケーション リンクに関する情報を取得するために使用できます。

次のコマンドでは、リソース グループ "rg2" に属するサーバー "srv2” 上のプライマリ データベース "mydb" とセカンダリ データベースの間のレプリケーション リンクのステータスを取得します。

    $database = Get-AzureRmSqlDatabase –DatabaseName "mydb”
    $secondaryLink = $database | Get-AzureRmSqlDatabaseReplicationLink –PartnerResourceGroup "rg2” –PartnerServerName "srv2”



   

## 次のステップ

- [障害復旧訓練](sql-database-disaster-recovery-drills.md)




## その他のリソース

- [新しい geo レプリケーション機能に関するスポットライト](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication)
- [geo レプリケーションを使用したビジネス継続性を実現するクラウド アプリケーションの設計](sql-database-designing-cloud-solutions-for-disaster-recovery.md)
- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)

