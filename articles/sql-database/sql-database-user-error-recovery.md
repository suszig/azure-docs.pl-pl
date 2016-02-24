<properties 
   pageTitle="SQL Database のユーザー エラーの復旧" 
   description="Azure SQL Database の特定時点に復元 (PITR) 機能を使用して、ユーザー エラー、偶発的なデータの破損、または削除済みデータベースを復旧する方法について説明します。" 
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

# ユーザー エラーからの Azure SQL Database の復旧

Azure SQL Database は、ユーザー エラーまたは予期しないデータ変更から復旧するための、2 つの中核的な機能を提供します。

- ポイントインタイム リストア 
- 削除済みデータベースの復元

これで、これらの機能についての詳細については、 [ブログの投稿](http://azure.microsoft.com/blog/2014/10/01/azure-sql-database-point-in-time-restore/)します。

Azure SQL Database では、常に新しいデータベースに復元します。 これらの復元機能は、Basic、Standard、Premium のすべてのデータベースに提供されます。

##ポイントインタイム リストア
ユーザー エラーや意図しないデータ変更が発生した場合、ポイントインタイム リストア (PITR) 機能を使用して、データベースの保持期間内の任意の時点にデータベースを復元できます。 

Basic データベースの保持期間は 7 日間、Standard データベースの保持期間は 14 日間、Premium データベースの保持期間は 35 日です。 データベースの保有期間の詳細については、次を参照してください [ビジネス継続性の概要](sql-database-business-continuity.md)します。

> [AZURE.NOTE] データベースを復元するには、新しいデータベースが作成されます。 復元先サーバーに新しいデータベース用の十分な DTU 容量があることが重要です。 によってこのクォータの増加を要求する [サポートに連絡して](http://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/)します。

###Azure ポータル
Azure ポータルでポイントインタイム リストアを使用するには、次の手順に従います。

1. ログイン、 [Azure ポータル](https://portal.Azure.com)
2. 画面の左側にある次のように選択します。 **参照** し、[ **SQL データベース**します。
3. 目的のデータベースに移動し、それを選択します。
4. データベースのブレードの上部にある次のように選択します。 **復元**します。
5. データベース名を指定して、特定の時点および順にクリックして **作成**します。
6. データベースの復元処理が開始されを使用して監視できる **通知** 、画面の左側にあります。

###PowerShell
PowerShell を使用してプログラムを使用して、ポイント イン タイム リストアでを実行する、 [Start-azuresqldatabaserestore](https://msdn.microsoft.com/library/dn720218.aspx?f=255&MSPPError=-2147217396) コマンドレットです。 手順の詳細、お問い合わせください [この手順のビデオを見る](http://azure.microsoft.com/documentation/videos/restore-a-sql-database-using-point-in-time-restore-with-microsoft-azure-powershell/)します。

> [AZURE.IMPORTANT] この記事が最大のバージョンの Azure PowerShell のコマンドを含む *含まない* 1.0 以降のバージョン。 Azure PowerShell のバージョンを確認することができます、 **Get-module azure | format-table バージョン** コマンドです。

        $Database = Get-AzureSqlDatabase -ServerName "YourServerName" –DatabaseName “YourDatabaseName”
        $RestoreRequest = Start-AzureSqlDatabaseRestore -SourceDatabase $Database –TargetDatabaseName “NewDatabaseName” –PointInTime “2015-01-01 06:00:00”
        Get-AzureSqlDatabaseOperation –ServerName "YourServerName" –OperationGuid $RestoreRequest.RequestID
         

###REST API 
プログラムでデータベースの復元を実行するには、REST を使用します。

1. 使用して復元するデータベースの取得、 [Get Database](http://msdn.microsoft.com/library/azure/dn505708.aspx) 操作します。

2.  復元要求を使用して、作成、 [Create Database Restore Request](http://msdn.microsoft.com/library/azure/dn509571.aspx) 操作します。
    
3.  復元要求を使用して、追跡、 [Database Operation Status](http://msdn.microsoft.com/library/azure/dn720371.aspx) 操作します。

##削除されたデータベースの復元
データベースが削除された場合、Azure SQL Database では、削除されたデータベースを削除された時点の状態に復元できます。 Azure SQL Database では、データベースの保持期間にわたり、削除されたデータベースのバックアップを格納します。

削除されたデータベースの保持期間は、データベースのサービス階層に基づく日数とデータベースが存在していた日数のどちらか短い方になります。 詳細については、データベースの保有期間の読み取り、 [ビジネス継続性の概要](sql-database-business-continuity.md)します。

> [AZURE.NOTE] データベースを復元するには、新しいデータベースが作成されます。 復元先サーバーに新しいデータベース用の十分な DTU 容量があることが重要です。 によってこのクォータの増加を要求する [サポートに連絡して](http://azure.microsoft.com/blog/azure-limits-quotas-increase-requests/)します。

###Azure ポータル
削除されたデータベースを Azure ポータルを使用して復元するには、次の手順に従います。

1. ログイン、 [Azure ポータル](https://portal.Azure.com)
2. 画面の左側にある次のように選択します。 **参照** し、[ **の SQL Server**します。
3. 目的のサーバーに移動し、それを選択します。
4.  **操作** [サーバーのブレードで、次のように選択します。 **削除済みデータベース**します。
5. 復元する削除済みデータベースを選択します。
6. データベース名を指定し、クリックして **作成**します。
7. データベースの復元処理が開始されを使用して監視できる **通知** 、画面の左側にあります。

###PowerShell
PowerShell を使用して削除されたデータベースを復元するには、使用、 [Start-azuresqldatabaserestore](https://msdn.microsoft.com/library/dn720218.aspx?f=255&MSPPError=-2147217396) コマンドレットです。  手順の詳細、お問い合わせください [この手順のビデオを見る](http://azure.microsoft.com/documentation/videos/restore-a-deleted-sql-database-with-microsoft-azure-powershell/)します。

1. 削除済みデータベースと削除日は、削除済みデータベースの一覧から検索します。
        
        Get-AzureSqlDatabase -RestorableDropped -ServerName "YourServerName"

2. 削除された特定のデータベースを取得し、復元を開始します。

        $Database = Get-AzureSqlDatabase -RestorableDropped -ServerName "YourServerName" –DatabaseName “YourDatabaseName” -DeletionDate "1/01/2015 12:00:00 AM""
        $RestoreRequest = Start-AzureSqlDatabaseRestore -SourceRestorableDroppedDatabase $Database –TargetDatabaseName “NewDatabaseName”
        Get-AzureSqlDatabaseOperation –ServerName "YourServerName" –OperationGuid $RestoreRequest.RequestID
         

###REST API 
プログラムでデータベースの復元を実行するには、REST を使用します。

1.  使用して、復元可能な削除済みデータベースのすべてを一覧表示、 [List Restorable Dropped Databases](http://msdn.microsoft.com/library/azure/dn509562.aspx) 操作します。
    
2.  使用して復元する削除済みデータベースの詳細を取得、 [Get Restorable Dropped Database](http://msdn.microsoft.com/library/azure/dn509574.aspx) 操作します。

3.  使用して、復元を開始、 [Create Database Restore Request](http://msdn.microsoft.com/library/azure/dn509571.aspx) 操作します。
    
4.  使用して、復元の状態を追跡、 [Database Operation Status](http://msdn.microsoft.com/library/azure/dn720371.aspx) 操作します。

