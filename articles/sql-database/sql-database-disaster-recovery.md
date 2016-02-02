<properties 
   pageTitle="SQL Database の障害復旧" 
   description="Azure SQL Database のアクティブ geo レプリケーション、標準の geo レプリケーション、geo リストア機能を使用して、地域のデータ センターの停止や障害からデータベースを復旧する方法について説明します。" 
   services="sql-database" 
   documentationCenter="" 
   authors="elfisher" 
   manager="jeffreyg" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management" 
   ms.date="11/09/2015"
   ms.author="elfish"/>


# Azure SQL Database を障害から回復する

Azure SQL Database は、障害から回復するために次の機能を備えています。

- アクティブ Geo レプリケーション [(ブログ)](http://azure.microsoft.com/blog/2014/07/12/spotlight-on-sql-database-active-geo-replication/)
- 標準 Geo レプリケーション [(ブログ)](http://azure.microsoft.com/blog/2014/09/03/azure-sql-database-standard-geo-replication/)
- 地理的復元 [(ブログ)](http://azure.microsoft.com/blog/2014/09/13/azure-sql-database-geo-restore/)
- 新しい Geo レプリケーション機能 [(ブログ)](https://azure.microsoft.com/blog/spotlight-on-new-capabilities-of-azure-sql-database-geo-replication/)

災害とデータベースを回復するための準備の詳細については、次を参照してください。 当社 [ビジネス継続性を設計](sql-database-business-continuity-design.md) ページです。

## 復旧を開始するタイミング

復旧操作はアプリケーションに影響します。 SQL 接続文字列を変更する必要があり、永続的にデータが失われる可能性があります。 そのため、障害がアプリケーションの RTO よりも長くかかる可能性が高い場合にのみ行ってください。 アプリケーションが実稼働環境にデプロイされている場合は、アプリケーションの健全性の定期的な監視を実行し、次のデータ ポイントを使用して、復旧が保証されていることをアサートします。

1. 接続の永続的な障害は、アプリケーション層からデータベースに渡ります。
2. Azure クラシック ポータルは、幅広い影響のあるリージョンでのインシデントに関するアラートを示します。

> [AZURE.NOTE] データベースを復旧後に構成するに従って、使用されるように、 [回復後に、データベースを構成](#postrecovery) ガイドです。

## Geo レプリケートのセカンダリ データベースへのフェールオーバー

> [AZURE.NOTE] フェールオーバーにセカンダリ データベースを使用するように構成する必要があります。 geo レプリケーションでは、Standard データベースと Premium データベースにのみ使用できます。 学習 [Geo レプリケーションを構成する方法](sql-database-business-continuity-design.md)

### Azure クラシック ポータル

Azure クラシック ポータルを利用し、geo レプリケーションされたセカンダリ データベースとの連続コピー リレーションシップを終了します。

1. ログイン、 [Azure クラシック ポータル](https://portal.Azure.com)
2. 画面の左側にある、**[参照]** をクリックして、**[SQL Database]** を選択します。
3. 目的のデータベースに移動し、それを選択します。
4. データベースのブレードの下部にある **[Geo Replication map]** を選択します。
4. **[セカンダリ]** の下で、復旧するデータベースの名前の行を右クリックし、**[フェールオーバー]** を選択します。

### PowerShell

PowerShell を使用して、使用して Geo レプリケートのセカンダリ データベースへのフェールオーバーを開始する、 [セット AzureRMSqlDatabaseSecondary](https://msdn.microsoft.com/library/mt619393.aspx) コマンドレットです。

        $database = Get-AzureRMSqlDatabase –DatabaseName "mydb” –ResourceGroupName "rg2” –ServerName "srv2”
        $database | Set-AzureRMSqlDatabaseSecondary –Failover -AllowDataLoss

### REST API

REST を使用して、プログラムによって、セカンダリ データベースへのフェールオーバーを開始します。

1. 特定セカンダリを使用してへのレプリケーション リンクの取得、 [レプリケーション リンクの取得](https://msdn.microsoft.com/library/mt600778.aspx) 操作します。
2. フェールオーバーするセカンダリを使用して、 [設定セカンダリ データベースとしてプライマリ](https://msdn.microsoft.com/library/mt582027.aspx) データ損失の許可します。

## geo リストアを使用した復旧

データベースの障害が発生した場合は、地理リストアを使用して、最新の地理冗長バックアップからデータベースを復旧できます。
> [AZURE.NOTE] データベースを復旧すると、新しいデータベースが作成されます。 復旧先サーバーに新しいデータベース用の十分な DTU 容量があることが重要です。 によってこのクォータの増加を要求する [サポートに連絡して](http://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/)します。

### Azure クラシック ポータル

Azure クラシック ポータルで SQL Database を geo リストアを使用して復元するには、以下の手順を使用します。

1. ログイン、 [Azure クラシック ポータル](https://portal.Azure.com)
2. 画面の左側にある、**[新規]** を選択し、次に **[データ + ストレージ]** を選択し、次に **[SQL Database]** を選択します。
2. 選択 **バックアップ** ソースとから復旧する場合、地理冗長バックアップを選択します。
3. データベースのプロパティの残りの部分を指定して **[作成]** をクリックします。
4. データベースの復元処理が開始され、画面の左側にある **[通知]** を使用してこれを監視することができます。

### PowerShell

PowerShell で Geo-restore を使用して SQL データベースを復元すると地理リストア要求を開始、 [Start-azuresqldatabaserecovery](https://msdn.microsoft.com/library/azure/dn720224.aspx) コマンドレットです。

        $Database = Get-AzureSqlRecoverableDatabase -ServerName "ServerName" –DatabaseName “DatabaseToBeRecovered"
        $RecoveryRequest = Start-AzureSqlDatabaseRecovery -SourceDatabase $Database –TargetDatabaseName “NewDatabaseName” –TargetServerName “TargetServerName”
        Get-AzureSqlDatabaseOperation –ServerName "TargetServerName" –OperationGuid $RecoveryRequest.RequestID

### REST API

REST を使用して、プログラムでデータベースの復旧を実行します。

1.  使用して回復可能なデータベースの一覧を取得、 [List Recoverable Databases](http://msdn.microsoft.com/library/azure/dn800984.aspx) 操作します。

2.  使用して回復するデータベースの取得、 [Get Recoverable Database](http://msdn.microsoft.com/library/azure/dn800985.aspx) 操作します。

3.  回復要求を使用して、作成、 [Database Recovery Request](http://msdn.microsoft.com/library/azure/dn800986.aspx) 操作します。

4.  使用して、回復の状態を追跡、 [Database Operation Status](http://msdn.microsoft.com/library/azure/dn720371.aspx) 操作します。

## 回復後に、データベースを構成します。<a name="postrecovery"></a>

復旧されたデータベースを運用できる状態にするためのタスクのチェックリストを以下に示します。

### 接続文字列を更新する

アプリケーションの接続文字列が新しく復旧されたデータベースを指し示していることを確認します。 次のどちらかの条件に該当する場合は、接続文字列を更新します。

  + 復旧されたデータベースがソース データベース名と異なる名前を使用している。
  + 復旧されたデータベースがソース データベースと異なるサーバー上にある。

接続文字列の変更の詳細については、次を参照してください。 [Azure SQL Database への接続: 中央の推奨事項 ](sql-database-connect-central-recommendations.md)します。

### ファイアウォール ルールを変更する

サーバー レベルとデータベース レベルでファイアウォール ルールを確認し、クライアント コンピューターまたは Azure からサーバーと新しく復旧されたデータベースへの接続が有効になっていることを確認します。 詳細については、次を参照してください。 [方法: ファイアウォールの設定を構成する (Azure SQL データベース)](sql-database-configure-firewall-settings.md)します。

### サーバー ログインとデータベース ユーザーを確認する

アプリケーションで使用するすべてのログインが、復旧されたデータベースをホストしているサーバー上に存在することを確認します。 不足しているログインを再作成し、復旧されたデータベースに対する適切なアクセス許可を与えます。 詳細については、次を参照してください。 [Azure SQL Database におけるデータベースの管理とログイン](sql-database-manage-logins.md)します。

復旧されたデータベースの各データベース ユーザーに有効なサーバー ログインが関連付けられているかどうかを確認します。 ユーザーを有効なサーバー ログインにマップするには、ALTER USER ステートメントを使用します。 詳細については、次を参照してください。 [ALTER USER](http://go.microsoft.com/fwlink/?LinkId=397486)します。


### 製品利用統計情報アラートをセットアップする

既存のアラート ルール設定が復旧されたデータベースにマップされているかどうかを確認します。 次のどちらかの条件に該当する場合は、設定を更新します。

  + 復旧されたデータベースがソース データベース名と異なる名前を使用している。
  + 復旧されたデータベースがソース データベースと異なるサーバー上にある。

データベースのアラート ルールの詳細については、次を参照してください。 [アラート通知を受け取り](insights-receive-alert-notifications.md) と [追跡サービスのヘルス状態](insights-service-health.md)します。


### 監査を有効にする

データベースにアクセスするために監査が必要な場合は、データベースの復旧後に監査を有効にする必要があります。 クライアント アプリケーションで *.database.secure.windows.net パターンのセキュリティで保護された接続文字列を使用している場合は、監査が必要であることを表しています。 詳細については、次を参照してください。 [SQL database の監査を使ってみる](sql-database-auditing-get-started.md)します。





