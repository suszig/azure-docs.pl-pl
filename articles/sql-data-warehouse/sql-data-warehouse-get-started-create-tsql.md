<properties
    pageTitle="TSQL で SQL Data Warehouse を作成する |Microsoft Azure"
    description="TSQL で Azure SQL Data Warehouse を作成する方法を説明します。"
    services="sql-data-warehouse"
    documentationCenter="NA"
    authors="lodipalm"
    manager="barbkess"
    editor=""
    tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="10/21/2015"
   ms.author="lodipalm"/>

#TSQL で SQL Data Warehouse を作成する 

> [AZURE.SELECTOR]
- [Azure ポータル](sql-data-warehouse-get-started-provision.md)
- [TSQL](sql-data-warehouse-get-started-create-TSQL.md)
- [PowerShell](sql-data-warehouse-get-started-create-powershell.md)

この記事では、Transact SQL を使用して SQL Data Warehouse を作成する方法を示します。  この記事の手順を完了するには、次のものが必要です。

- Azure サブスクリプション。 をクリックする必要がある場合、Azure サブスクリプションだけで **無料評価版** これの上部にあるページをこの記事を完了する通します。
- 見ることができます。 Visual Studio の無償コピーは、次を参照してください。、 [Visual Studio のダウンロード](https://www.visualstudio.com/downloads/download-visual-studio-vs) ページです。
- V12 SQL サーバー。  SQL Data Warehouse を作成するには、V12 SQL サーバーが必要です。  V12 SQL サーバーがない場合は、新しいサーバーで SQL Data Warehouse を作成できるように、ポータルで作成しておくことをお勧めします。

この記事では、Visual Studio を正しく設定して接続する方法については説明していません。  参照してこのくださいを実行する方法の詳細については、 [接続し、クエリ][] ドキュメントです。  開始するには、Visual Studio で SQL Server オブジェクト エクスプローラーを開き、SQL Data Warehouse の作成に使用するサーバーに接続する必要があります。  それが完了したら、Master データベースに対して次のコマンドを実行することで、SQL Data Warehouse を作成できます。

        CREATE DATABASE <Name> (EDITION='datawarehouse', SERVICE_OBJECTIVE = '<Compute Size - DW####>', MAXSIZE= <Storage Size - #### GB>);

コマンド ラインを開き、次のコマンドを実行して SQL Data Warehouse を作成することもできます。

        sqlcmd -S <Server Name>.database.windows.net -I -U <User> -P <Password> -Q "CREATE DATABASE <Name> (EDITION='datawarehouse', SERVICE_OBJECTIVE = '<Compute Size - DW####>', MAXSIZE= <Storage Size - #### GB>)"

これらの TSQL ステートメントを実行するときには、MAXSIZE パラメーターと SERVICE_OBJECTIVE パラメーターに注意してください。これらは、Data Warehouse インスタンスに割り当てられる最初のストレージ サイズおよびコンピューティングを決定します。  MAXSIZE には次のいずれかのサイズを指定できますが、将来の拡張を見越して、より大きなサイズを選択することをお勧めします。 

+ 250 GB
+ 500 GB
+ 750 GB
+ 1,024 GB
+ 5,120 GB
+ 10,240 GB
+ 20,480 GB
+ 30,720 GB
+ 40,960 GB
+ 51,200 GB

SERVICE_OBJECTIVE は、インスタンスに最初に割り当てられる DWU の数を示し、次のいずれかの値を指定できます。 

+ DW100
+ DW200
+ DW300
+ DW400
+ DW500
+ DW600
+ DW1000
+ DW1200
+ DW1500
+ DW2000

これらのパラメーターの課金の影響に関する情報を参照してください、 [料金のページ][]します。

## 次のステップ
SQL Data Warehouse のプロビジョニングを実行できますが完了した後 [サンプル データの読み込み][] またはチェック アウトする方法 [開発][], 、[ロード][], 、または [移行][]します。

[connect and query]: ./sql-data-warehouse-get-started-connect.md
[migrate]: ./sql-data-warehouse-overview-migrate.md
[develop]: ./sql-data-warehouse-overview-develop.md
[load]: ./sql-data-warehouse-overview-load.md
[load sample data]: ./sql-data-warehouse-get-started-manually-load-samples.md
[pricing page]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/

