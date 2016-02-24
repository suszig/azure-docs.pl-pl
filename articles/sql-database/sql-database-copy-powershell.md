<properties 
    pageTitle="PowerShell を使用した Azure SQL Database のコピーの作成" 
    description="PowerShell を使用した Azure SQL Database のコピーの作成" 
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jeffreyg"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="12/01/2015"
    ms.author="sstein"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# PowerShell を使用した Azure SQL Database のコピーの作成

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-copy.md)
- [PowerShell](sql-database-copy-powershell.md)
- [SQL](sql-database-copy-transact-sql.md)



次の手順では、PowerShell で SQL データベースをコピーする方法を説明します。 データベース コピー操作では、使用して新しいデータベースに SQL データベースをコピー、 [Start-azuresqldatabasecopy](https://msdn.microsoft.com/library/dn720220.aspx) コマンドレットです。 コピーは、同じサーバーか別のサーバーで作成するデータベースのスナップショット バックアップです。

> [AZURE.NOTE] Azure SQL データベースは自動的に作成され、すべてのユーザー データベースを復元するバックアップを保持します。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)します。

コピー プロセスが完了すると、新しいデータベースは、コピー元のデータベースに依存せずに完全に機能するデータベースになります。 コピーの完了時点で、新しいデータベースのトランザクションはコピー元のデータベースと同じになります。 データベース コピーのサービス レベルとパフォーマンス レベル (価格レベル) はコピー元のデータベースと同じになります。 コピーの完了後、コピーは完全に機能する独立したデータベースになります。 ログイン、ユーザー、アクセス許可は非依存で管理できます。


データベースを同じ論理サーバーにコピーすると、両方のデータベースで同じログインを利用できます。 データベースをコピーするために使用するセキュリティ プリンシパルが、新しいデータベースのデータベース所有者 (DBO) になります。 すべてのデータベース ユーザー、アクセス許可、セキュリティ識別子 (SID) がデータベースのコピーにコピーされます。


この記事を完了するには、以下が必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database。 SQL データベースがない、1 つ作成この記事の手順: [最初の Azure SQL Database を作成する](sql-database-get-started.md)です。
- Azure PowerShell。 ダウンロードして実行して、Azure PowerShell モジュールをインストール、 [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)します。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。



## 資格情報を構成してサブスクリプションを選択

まず、Azure アカウントへのアクセスを確立する必要があるため、PowerShell を起動してから以下のコマンドレットを実行します。 ログイン画面で、Azure クラシック ポータルへのサインインに使用しているものと同じ電子メールとパスワードを入力します。

    Add-AzureAccount

正常にサインインすると、サインインしている ID や使用中の Azure サブスクリプションを含む情報が画面に表示されます。


### Azure サブスクリプションを選択します。

サブスクリプションを選択する必要があります、サブスクリプション Id とサブスクリプション名 (**-subscriptionname**)。 サブスクリプション Id をコピーするには、前の手順から表示された情報からか、サブスクリプションが複数あり、詳細情報を必要な場合は、実行、 **Get-azuresubscription** コマンドレット、結果セットから目的のサブスクリプション情報をコピーします。 ご利用のサブスクリプションを取得したら次のコマンドレットを実行します。

    Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

正常に実行した後 **Select-azuresubscription** プロンプト、PowerShell に戻されます。 1 つ以上のサブスクリプションがある場合は、実行 **Get-azuresubscription** サブスクリプションの表示を使用することを確認および **IsCurrent: True**します。


## 特定の環境用の変数の設定

変数には、例の値を、使用するデータベースとサーバーの特定の値に置き換える必要があるものがいくつかあります。

プレースホルダー値をお使いの環境の値で置き換えます。

    # The name of the server on which the source database resides.
    $ServerName = "sourceServerName"

    # The name of the source database (the database to copy). 
    $DatabaseName = "sourceDatabaseName" 
    
    # The name of the server that hosts the target database. This server must be in the same Azure subscription as the source database server. 
    $PartnerServerName = "partnerServerName"

    # The name of the target database (the name of the copy).
    $PartnerDatabaseName = "partnerDatabaseName" 





## SQL データベースを同じサーバーにコピーする

このコマンドでは、サービスにデータベースのコピー要求を送信します。 データベースのサイズに応じて、コピー操作の完了に時間がかかる場合があります。

    # Copy a database to the same server
    Start-AzureSqlDatabaseCopy -ServerName $ServerName -DatabaseName $DatabaseName -PartnerDatabase $PartnerDatabaseName

## SQL データベースを別のサーバーにコピーする

このコマンドでは、サービスにデータベースのコピー要求を送信します。 データベースのサイズに応じて、コピー操作の完了に時間がかかる場合があります。

    # Copy a database to a different server
    Start-AzureSqlDatabaseCopy -ServerName $ServerName -DatabaseName $DatabaseName -PartnerServer $PartnerServerName -PartnerDatabase $PartnerDatabaseName
    

## コピー操作の進行状況の監視

[実行後 **Start-azuresqldatabasecopy** コピー要求の状態を確認することができます。 この後すぐに実行して、要求は通常返します **状態: 保留中** または **状態: を実行している** が表示されるまで、これを複数回を実行できるように **状態: 完了** 出力にします。 


    Get-AzureSqlDatabaseOperation -ServerName $ServerName -DatabaseName $DatabaseName


## SQL Database のコピーの PowerShell スクリプト

    # The name of the server where the source database resides
    $ServerName = "sourceServerName"

    # The name of the source database (the database to copy) 
    $DatabaseName = "sourceDatabaseName" 
    
    # The name of the server to host the database copy. This server must be in the same Azure subscription as the source database server
    $PartnerServerName = "partnerServerName"

    # The name of the target database (the name of the copy)
    $PartnerDatabaseName = "partnerDatabaseName" 


    Add-AzureAccount
    Select-AzureSubscription -SubscriptionName "myAzureSubscriptionName"
      
    # Copy a database to a different server (remove the -PartnerServer parameter to copy to the same server)
    Start-AzureSqlDatabaseCopy -ServerName $ServerName -DatabaseName $DatabaseName -PartnerServer $PartnerServerName -PartnerDatabase $PartnerDatabaseName
    
    # Monitor the status of the copy
    Get-AzureSqlDatabaseOperation -ServerName $ServerName -DatabaseName $DatabaseName
    

## 次のステップ

- [SQL Server Management Studio (SSMS) での接続](sql-database-connect-to-database.md)
- [データベースを BACPAC にエクスポートする](sql-database-export-powershell.md)


## その他のリソース

- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [障害復旧訓練](sql-database-disaster-recovery-drills.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)

