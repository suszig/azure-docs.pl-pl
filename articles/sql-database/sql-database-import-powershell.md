<properties 
    pageTitle="PowerShell で BACPAC ファイルをインポートして新しい Azure SQL Database を作成する" 
    description="PowerShell で BACPAC ファイルをインポートして新しい Azure SQL Database を作成する" 
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

# PowerShell で BACPAC ファイルをインポートして新しい Azure SQL Database を作成する

**1 つのデータベース**

> [AZURE.SELECTOR]
- [Azure ポータル](sql-database-import.md)
- [PowerShell](sql-database-import-powershell.md)


この記事は、PowerShell で BACPAC ファイルをインポートして Azure SQL Database を作成する手順について説明します。

BACPAC は、データベース スキーマとデータを含む .bacpac ファイルです。 詳細については、バックアップ パッケージ (.bacpac) を参照してください [データ層アプリケーション](https://msdn.microsoft.com/library/ee210546.aspx)します。

Azure Storage BLOB コンテナーからインポートされた BACPAC からデータベースが作成されます。 Azure ストレージに .bacpac ファイルがあるない場合は、次の手順に従って、1 つを作成 [作成と Azure SQL Database の BACPAC のエクスポート](sql-database-export-powershell.md)します。

> [AZURE.NOTE] Azure SQL データベースは自動的に作成され、すべてのユーザー データベースを復元するバックアップを保持します。 詳細については、「 [ビジネス継続性の概要](sql-database-business-continuity.md)します。


SQL Database をインポートするには、以下が必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- 復元するデータベースの .bacpac ファイル (BACPAC)。 BACPAC がである必要があります、 [Azure ストレージ アカウント (クラシック)](storage-create-storage-account.md) blob コンテナーです。


> [AZURE.IMPORTANT] この記事が最大のバージョンの Azure PowerShell のコマンドを含む *含まない* 1.0 以降のバージョン。 Azure PowerShell のバージョンを確認することができます、 **Get-module azure | format-table バージョン** コマンドです。



## 資格情報を構成してサブスクリプションを選択

まず、Azure アカウントへのアクセスを確立する必要があるため、PowerShell を起動してから以下のコマンドレットを実行します。 ログイン画面で、Azure ポータルへのサインインに使用しているものと同じ電子メールとパスワードを入力します。

    Add-AzureAccount

正常にサインインすると、サインインしている ID や使用中の Azure サブスクリプションを含む情報が画面に表示されます。


### Azure サブスクリプションを選択します。

サブスクリプションを選択するには、サブスクリプション ID が必要です。 サブスクリプション Id をコピーするには、前の手順から表示された情報からもサブスクリプションが複数あり、詳細情報を必要な場合は、実行、 **Get-azuresubscription** コマンドレット、結果セットから目的のサブスクリプション情報をコピーします。 サブスクリプション ID を取得したら、次のコマンドレットを実行します。

    Select-AzureSubscription -SubscriptionId 4cac86b0-1e56-bbbb-aaaa-000000000000

正常に実行した後 **Select-azuresubscription** プロンプト、PowerShell に戻されます。 1 つ以上のサブスクリプションがある場合は、実行 **Get-azuresubscription** 番組を選択したサブスクリプションの検証と **IsCurrent: True**します。


## 使用環境用の変数の設定

変数には、例の値を、使用するデータベースとストレージ アカウントの特定の値に置き換える必要があるものがいくつかあります。

サーバー名は、前の手順で選択したサブスクリプション内に現在存在しており、データベースを作成するサーバーを指定する必要があります。

データベース名は、新しいデータベースの名前です。

    $ServerName = "servername"
    $DatabaseName = "databasename"


BACPAC が配置されているストレージ アカウントの変数を以下に示します。  [Azure ポータル](https://portal.azure.com) をこれらの値を取得するストレージ アカウントに移動します。 クリックしてプライマリ アクセス キーを検索できます **設定をすべて** し **キー** ストレージ アカウントのブレードからです。

BLOB 名は、データベースの作成元の、既存の .bacpac ファイルの名前です。 .bacpac 拡張子を含める必要があります。

    $StorageName = "storageaccountname"
    $ContainerName = "blobcontainername"
    $BlobName = "filename.bacpac"
    $StorageKey = "primaryaccesskey"

## サーバーおよびストレージ アカウントを指すポインターの作成

実行している、 **Get-credential** コマンドレットは、ユーザー名とパスワードを確認するウィンドウを開きます。 データベースを作成する SQL Server (上記の $ServerName) の管理ログインとパスワードを入力します。Azure アカウントのユーザー名とパスワードではありません。

    $credential = Get-Credential
    $SqlCtx = New-AzureSqlDatabaseServerContext -ServerName $ServerName -Credential $credential

    $StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey
    $Container = Get-AzureStorageContainer -Name $ContainerName -Context $StorageCtx


## データベースのインポート

このコマンドでは、サービスにデータベース のインポート要求を送信します。 データベースのサイズに応じて、インポート操作の完了に時間がかかる場合があります。

    $importRequest = Start-AzureSqlDatabaseImport -SqlConnectionContext $SqlCtx -StorageContainer $Container -DatabaseName $DatabaseName -BlobName $BlobName
    

## 操作の進行状況の監視

[実行後 **開始 AzureSqlDatabaseImport** 要求の状態を確認することができます。 

要求の直後後の状態の確認の状態を返します通常 **保留中**, 、または **を実行している** と現在の達成率が表示されるまで、これを複数回を実行できるようにすることが **状態: 完了** 出力にします。 

このコマンドを実行すると、パスワードの入力を求められます。 SQL Server の管理ログインとパスワードを入力します。


    Get-AzureSqlDatabaseImportExportStatus -RequestId $ImportRequest.RequestGuid -ServerName $ServerName -Username $credential.UserName
 


## SQL Database の PowerShell 復元スクリプト


    Add-AzureAccount
    Select-AzureSubscription -SubscriptionId "4cac86b0-1e56-bbbb-aaaa-000000000000"
    
    $ServerName = "servername"
    $DatabaseName = "nameofnewdatabase"

    $StorageName = "storageaccountname"
    $ContainerName = "blobcontainername"
    $BlobName = "filename.bacpac"
    $StorageKey = "primaryaccesskey"
    
    $credential = Get-Credential
    $SqlCtx = New-AzureSqlDatabaseServerContext -ServerName $ServerName -Credential $credential
    
    $StorageCtx = New-AzureStorageContext -StorageAccountName $StorageName -StorageAccountKey $StorageKey
    $Container = Get-AzureStorageContainer -Name $ContainerName -Context $StorageCtx
    
    $ImportRequest = Start-AzureSqlDatabaseImport -SqlConnectionContext $SqlCtx -StorageContainer $Container -DatabaseName $DatabaseName -BlobName $BlobName
    
    Get-AzureSqlDatabaseImportExportStatus -RequestId $ImportRequest.RequestGuid -ServerName $ServerName -Username $credential.UserName
    

## 次のステップ

- [SQL Server Management Studio (SSMS) での接続](sql-database-connect-to-database.md)




## その他のリソース

- [ビジネス継続性の概要](sql-database-business-continuity.md)
- [障害復旧訓練](sql-database-disaster-recovery-drills.md)
- [SQL Database のドキュメント](https://azure.microsoft.com/documentation/services/sql-database/)

