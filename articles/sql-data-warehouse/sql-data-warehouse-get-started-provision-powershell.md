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
- [Azure ポータル](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-database-tsql.md)
- [PowerShell](sql-data-warehouse-get-started-provision-powershell.md)

> [AZURE.NOTE]  SQL Data Warehouse での Microsoft Azure Powershell を使用するのには、バージョン 0.9.4 必要がありますか大きいです。  Powershell で (Get-Module Azure).Version を実行することで、バージョンを確認できます。

## Azure PowerShell コマンドレットの取得と実行
Powershell をまだセットアップしていない場合は、次のようにします。

1. Azure PowerShell モジュールをダウンロードするには、実行 [Microsoft Web Platform Installer](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)します。
2. ウィンドウのスタートアップの種類で、モジュールを実行する **Microsoft Azure PowerShell**します。
3. コンピューターにアカウントをまだ追加していない場合は、次のコマンドレットを実行します  (詳細については、次を参照してください。 [インストールと Azure PowerShell][]) を構成する方法。

            Add-AzureAccount

4. ARM モードで PowerShell を実行する必要もあります。  このモードに切り替えるには、次のコマンドを実行します。

            switch-azuremode AzureResourceManager

## SQL Data Warehouse の作成
アカウントで Powershell をセットアップした後は、次のコマンドを実行して新しい SQL Data Wareouse をデプロイできます。

        New-AzureSqlDatabase -RequestedServiceObjectiveName "<Service Objective>" -DatabaseName "<Data Warehouse Name>" -ServerName "<Server Name>" -ResourceGroupName "<ResourceGroupName>" -Edition "DataWarehouse"

このコマンドレットに必要なパラメーターは次のとおりです。

 + **RequestedServiceObjectiveName**: 要求している DWU の量、フォームに"DWXXX"は現在サポートされている値: 100、200 人、300、400、500、600、1000、1200年, 1500年, 2000年です。
 + **DatabaseName**: 作成している SQL データ ウェアハウスの名前。
 + **ServerName**: の作成を使用しているサーバーの名前 (V12 をする必要があります)。
 + **ResourceGroupName**: リソース グループを使用しています。  サブスクリプションで使用可能なリソース グループを調べるには Get-AzureResource を使用します。
 + **エディション**: SQL Data Warehouse を作成するには、"DataWarehouse"へのエディションを設定する必要があります。 

## 次のステップ
SQL Data Warehouse では [サンプル データの読み込み] または、[読み込み] の [開発] 方法を確認して、プロビジョニングが完了した後か、[移行]。

プログラムを使用して SQL Data Warehouse を管理する方法についてさらに知りたい場合、[Powershell] や [REST API] ドキュメントをご覧ください。



<!--Image references-->

<!--Article references-->
[migrate]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-migrate/
[develop]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-develop/
[load]:https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-overview-load/
[load sample data]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-get-started-manually-load-samples/
[Powershell]: https://azure.microsoft.com/en-us/documentation/articles/sql-data-warehouse-reference-powershell-cmdlets/
[REST API]: https://msdn.microsoft.com/library/azure/dn505719.aspx
[MSDN]:https://msdn.microsoft.com/library/azure/dn546722.aspx
[firewall rules]:https://azure.microsoft.com/en-us/documentation/articles/sql-database-configure-firewall-settings/
[How to install and configure Azure PowerShell]: powershell-install-configure.md

