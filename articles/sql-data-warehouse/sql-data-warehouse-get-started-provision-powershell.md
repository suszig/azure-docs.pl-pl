<properties
   pageTitle="Powershell を使用して SQL Data Warehouse を作成する |Microsoft Azure"
   description="Powershell を使用して SQL Data Warehouse を作成します"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/20/2015"
   ms.author="lodipalm"/>


# Powershell を使用して SQL Data Warehouse を作成する

> [AZURE.SELECTOR]
- [Azure Portal](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)


> [AZURE.NOTE]  SQL Data Warehouse で Microsoft Azure Powershell を使用するには、バージョン 0.9.4 以降が必要です。 Powershell で (Get-Module Azure).Version を実行することで、バージョンを確認できます。

## Azure PowerShell コマンドレットの取得と実行

Powershell をまだセットアップしていない場合は、次のようにします。

1. Azure PowerShell モジュールをダウンロードするには、実行 [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)します。
2. モジュールを実行するには、スタート ウィンドウに「**Microsoft Azure PowerShell**」と入力します。
3. コンピューターにアカウントをまだ追加していない場合は、次のコマンドレットを実行します  (詳細については、次を参照してください。 [をインストールして、Azure PowerShell の [] を構成する方法][])。

            Add-AzureAccount

4. ARM モードで PowerShell を実行する必要もあります。 このモードに切り替えるには、次のコマンドを実行します。

            switch-azuremode AzureResourceManager


## SQL Data Warehouse の作成

アカウントで Powershell をセットアップした後は、次のコマンドを実行して新しい SQL Data Wareouse をデプロイできます。

        New-AzureSqlDatabase -RequestedServiceObjectiveName "<Service Objective>" -DatabaseName "<Data Warehouse Name>" -ServerName "<Server Name>" -ResourceGroupName "<ResourceGroupName>" -Edition "DataWarehouse"

このコマンドレットに必要なパラメーターは次のとおりです。

 + **RequestedServiceObjectiveName**: 要求する DWU の量。"DWXXX" の形式です。現在サポートされている値は、100、200、300、400、500、600、1000、1200、1500、2000 です。
 + **DatabaseName**: 作成する SQL Data Warehouse の名前。
 + **ServerName**: 作成の際に使用するサーバーの名前 (V12 にする必要があります)。
 + **ResourceGroupName**: 使用するリソース グループ。 サブスクリプションで使用可能なリソース グループを調べるには Get-AzureResource を使用します。
 + **Edition**: SQL Data Warehouse を作成するには、エディションを "DataWarehouse" に設定する必要があります。

## 次のステップ

SQL Data Warehouse のプロビジョニングを実行できますが完了した後 [サンプル データのを読み込む][] またはチェック アウトする方法 [を開発][], 、[を読み込む][], 、または [を移行][]します。

プログラムを使用して SQL Data Warehouse を管理する方法についてさらに知りたい場合はチェック アウト、 [Powershell][] または [REST API の][] ドキュメントです。







[migrate]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-migrate/ 
[develop]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-develop/ 
[load]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-load/ 
[load sample data]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-manually-load-samples/ 
[powershell]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-reference-powershell-cmdlets/ 
[rest api]: https://msdn.microsoft.com/library/azure/dn505719.aspx 
[msdn]: https://msdn.microsoft.com/library/azure/dn546722.aspx 
[firewall rules]: https://azure.microsoft.com/en-us/documentation/articles/sql-database-configure-firewall-settings/ 
[how to install and configure azure powershell]: powershell-install-configure.md 

