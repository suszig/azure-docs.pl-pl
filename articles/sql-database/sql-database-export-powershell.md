<properties 
    pageTitle="PowerShell を使用した Azure SQL Database の BACPAC の作成およびエクスポート" 
    description="PowerShell を使用した Azure SQL Database の BACPAC の作成およびエクスポート" 
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


# PowerShell を使用した Azure SQL Database の BACPAC の作成およびエクスポート

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-export.md)
- [PowerShell](sql-database-export-powershell.md)


この記事では、PowerShell を使用して Azure SQL Database の BACPAC をエクスポートする手順について説明します。

A [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) .bacpac ファイルで、データベース スキーマとデータが含まれています。 基本的なユース ケース BACPAC は、データベースを移動する 1 つのサーバー間には [ローカル データベースをクラウドに移行](sql-database-cloud-migrate.md), 、および既存のデータベースをオープン フォーマットでアーカイブされます。

> [AZURE.NOTE] Bacpac は、バックアップに使用すると、復元操作はしていません。 Azure SQL Database では、すべてのユーザー データベースのバックアップが自動的に作成されます。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)します。


BACPAC は Azure Storage BLOB コンテナーにエクスポートされます。操作が正常に完了した後にダウンロードできます。


この記事を完了するには、以下が必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- Azure SQL Database。 SQL データベースがない、1 つ作成この記事の手順: [最初の Azure SQL Database を作成する](sql-database-get-started.md)です。
-  [Azure ストレージ アカウント](storage-create-storage-account.md) に BACPAC を格納する blob コンテナーとします。 ストレージ アカウントが、従来の配置モデルを使用する必要があります現在ように選択 **クラシック** ストレージ アカウントを作成するときにします。
- Azure PowerShell。 ダウンロードして実行して、Azure PowerShell モジュールをインストール、 [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)します。 詳細については、次を参照してください。 [をインストールして、Azure PowerShell を構成する方法](powershell-install-configure.md)します。



## 資格情報を構成してサブスクリプションを選択

まず、Azure アカウントへのアクセスを確立する必要があるため、PowerShell を起動してから以下のコマンドレットを実行します。 ログイン画面で、Azure ポータルへのサインインに使用しているものと同じ電子メールとパスワードを入力します。

    Add-AzureAccount

正常にサインインすると、サインインしている ID や使用中の Azure サブスクリプションを含む情報が画面に表示されます。


### Azure サブスクリプションを選択します。

サブスクリプションを選択する必要があります、サブスクリプション Id とサブスクリプション名 (**-subscriptionname**)。 サブスクリプション Id をコピーするには、前の手順から表示された情報からか、サブスクリプションが複数あり、詳細情報を必要な場合は、実行、 **Get-azuresubscription** コマンドレット、結果セットから目的のサブスクリプション情報をコピーします。 ご利用のサブスクリプションを取得したら次のコマンドレットを実行します。

    Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

正常に実行した後 **Select-azuresubscription** プロンプト、PowerShell に戻されます。 1 つ以上のサブスクリプションがある場合は、実行 **Get-azuresubscription** サブスクリプションの表示を使用することを確認および **IsCurrent: True**します。


## 特定の環境用の変数の設定

変数には、例の値を、使用するデータベースとストレージ アカウントの特定の値に置き換える必要があるものがいくつかあります。

サーバーとデータベースの名前は、アカウントに現在存在するサーバーとデータベースに置き換えます。 BLOB 名については、作成される BACPAC ファイル名を入力します。 BACPAC ファイル名として任意の名前を入力できますが、.bacpac 拡張子を含める必要があります。

    $ServerName = "servername"
    $DatabaseName = "nameofdatabasetoexport"
    $BlobName = "filename.bacpac"

 [Azure ポータル](https://portal.azure.com) をこれらの値を取得するストレージ アカウントに移動します。 クリックしてプライマリ アクセス キーを検索できます **設定をすべて** し **キー** ストレージ アカウントのブレードからです。

    $StorageName = "storageaccountname"
    $ContainerName = "blobcontainername"
    $StorageKey = "primaryaccesskey"

## サーバーおよびストレージ アカウントを指すポインターの作成

実行している、 **Get-credential** コマンドレットは、ユーザー名とパスワードを確認するウィンドウを開きます。 SQL Server の管理ログインとパスワード (Azure アカウントのユーザー名とパスワードではない) を入力します。

    $credential = Get-Credential
    $SqlCtx = New-AzureSqlDatabaseServerContext -ServerName $ServerName -Credential $credential

    $StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey
    $Container = Get-AzureStorageContainer -Name $ContainerName -Context $StorageCtx


## データベースのエクスポート

このコマンドでは、サービスにデータベースのエクスポート要求を送信します。 データベースのサイズに応じて、エクスポート操作の完了に時間がかかる場合があります。

> [AZURE.IMPORTANT] BACPAC ファイルをトランザクション上の一貫性を保証するためにはまず [、データベースのコピーを作成](sql-database-copy-powershell.md) し、そのデータベースのコピーをエクスポートします。 


    $exportRequest = Start-AzureSqlDatabaseExport -SqlConnectionContext $SqlCtx -StorageContainer $Container -DatabaseName $DatabaseName -BlobName $BlobName
    

## エクスポート操作の進行状況の監視

[実行後 **開始 AzureSqlDatabaseExport** 要求の状態を確認することができます。 実行してこの直後に、要求は通常返さ **ステータス: 保留中** または **ステータス: を実行している** が表示されるまで、これを複数回を実行できるように **状態: 完了** 出力にします。 

このコマンドを実行すると、パスワードの入力を求められます。 SQL Server の管理パスワードを入力します。


    Get-AzureSqlDatabaseImportExportStatus -RequestId $exportRequest.RequestGuid -ServerName $ServerName -Username $credential.UserName
    


## SQL Database のエクスポートの PowerShell スクリプト


    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId "4cac86b0-1e56-bbbb-aaaa-000000000000"
    
    $ServerName = "servername"
    $DatabaseName = "databasename"
    $BlobName = "bacpacfilename"
    
    $StorageName = "storageaccountname"
    $ContainerName = "blobcontainername"
    $StorageKey = "primaryaccesskey"
    
    $credential = Get-Credential
    $SqlCtx = New-AzureSqlDatabaseServerContext -ServerName $ServerName -Credential $credential
    
    $StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey
    $Container = Get-AzureStorageContainer -Name $ContainerName -Context $StorageCtx
    
    $exportRequest = Start-AzureSqlDatabaseExport -SqlConnectionContext $SqlCtx -StorageContainer $Container -DatabaseName $DatabaseName -BlobName $BlobName
    
    Get-AzureSqlDatabaseImportExportStatus -RequestId $exportRequest.RequestGuid -ServerName $ServerName -Username $credential.UserName
    


## 次のステップ

- [Azure SQL Database のインポート](sql-database-import-powershell.md)


## その他のリソース

- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [障害復旧訓練](sql-database-disaster-recovery-drills.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)

