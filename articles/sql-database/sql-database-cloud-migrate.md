<properties
   pageTitle="SQL Server データベースの Azure SQL Database への移行"
   description="Microsoft Azure SQL Database、データベースのデプロイ、データベースの移行、データベースのインポート、データベースのエクスポート、移行ウィザード"
   services="sql-database"
   documentationCenter=""
   authors="carlrabeler"
   manager="jeffreyg"
   editor=""/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-management"
   ms.date="10/12/2015"
   ms.author="carlrab"/>


# SQL Server データベースの Azure SQL Database への移行

オンプレミス データベースを Azure SQL Database に移動する作業の複雑性は、お使いのデータベースまたはアプリケーションの設計や許容されるダウンタイムによって変わります。 互換性のあるデータベースの場合、Azure SQL Database データベースの移行は、スキーマとデータの簡単な移動操作であり、スキーマの変更がほとんど不要で、アプリケーションの再構築もほとんど、もしくはまったく必要としません。 [Azure SQL Database V12](../sql-database-v12-whats-new.md) はほぼ完全なエンジン互換性を SQL Server 2014 および SQL Server 2016 に提供します。 Most SQL Server 2016 Transact-SQL ステートメントは Microsoft Azure SQL Database で完全サポートされています。 これには SQL Server のデータ型、演算子、文字列関数、算術関数、論理関数、カーソル関数、ほとんどのアプリケーションが依存するその他の Transact-SQL 要素が含まれます。 部分的にサポートされる関数やサポートされない関数は SQL Databse によるデータベースの管理方法の違い (ファイル、高い可用性、セキュリティ機能など) に関連するか、Service Broker など、特別目的の機能のためのものです。 SQL Database はマスター データベースに対する依存関係から多くの機能を分離するため、サーバーレベルの多くのアクティビティが不適切となり、サポートされません。 通常、SQL Server で廃止になった機能は SQL Database でサポートされません。 データベースおよびアプリケーションに依存する [部分的にサポートされていない関数または](../sql-database-transact-sql-information.md) いくつか前に再構築、移行する必要があります。

SQL Server データベースを Azure SQL Database に移行するワークフローは次のとおりです。

 1. [データベースが互換性があるかどうかの確認します。](#determine-if-your-database-is-compatible)
 2. [互換性のない場合は、データベースの互換性の問題を解決します。](#fix-database-compatibility-issues)
 3. [互換性のあるデータベースを移行します。](#options-to-migrate-a-compatible-database-to-azure-sql-database)

>[AZURE.NOTE] Microsoft Access、Sybase、MySQL Oracle および DB2 から Azure SQL データベースを含むデータベースの他の種類を移行するには、「 [SQL Server Migration Assistant](http://blogs.msdn.com/b/ssma/)します。

## データベースに互換性があるかどうかを調べる

使用して、ソース データベースが互換性があるかを判断する 2 つの主な方法はあります。
- データ層アプリケーションをエクスポートします。 このメソッドは、Management Studio で、ウィザードを使用して、データベースを分析し、コンソールに存在する場合は、データベースの互換性の問題が表示されます。
- SQLPackage: このメソッドは、SQLPackage.exe を使用して [sqlpackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) データベースを分析し、レポートを生成するコマンド ライン ユーティリティです。 SQLPackage は Visual Studio と SQL Server に付属します。

> [AZURE.NOTE] 3 つ目の方法もあります。その方法では、追加のソース情報としてトレース ファイルを利用し、アプリケーション レベルとデータベース レベルで互換性を検証します。 これは、 [SQL Azure 移行ウィザード](http://sqlazuremw.codeplex.com/), 、Codeplex の無償のツールです。 ただし、このツールは現在、 Azure SQL Database V12 では問題にならないが Azure SQL Database V11 では問題であった互換性エラーを検出できます。

データベースの非互換性が検出された場合は、Azure SQL Database にデータベースを移行する前に、これらの非互換性を修正する必要があります。 データベースの互換性の問題を修正する方法のガイダンスについてを参照してください [データベースの互換性の問題を修正](#fix-database-compatibility-issues)します。
> [AZURE.IMPORTANT] これらのオプションでは、SQL Server データベースの異なるレベル (つまり、レベル 90、100、110) の間における互換性の問題をすべて検出することはできません。 古いデータベース (レベル 80、90、100、110) から移行する場合は、最初に (少なくとも開発環境で) アップグレードを行い、SQL Server 2014 以降にした後で、Azure SQL Database に移行する必要があります。

## sqlpackage.exe を使用してデータベースの互換性を調べる

1. コマンド プロンプトを開き、sqlpackage.exe の最新バージョンを含むディレクトリに変更します。 このユーティリティは、Visual Studio と SQL Server の両方に含まれます。 こともできます [ダウンロード](https://msdn.microsoft.com/library/mt204009.aspx) このユーティリティを取得する SQL Server データ ツールの最新バージョンです。
2. 次の sqlpackage.exe コマンドをご利用の環境に合わせた引数で実行します。

    ' sqlpackage.exe/Action:Export/ssn: < server_name >/sdn: < database_name >/tf: < target_file >/p:TableData = < schema_name.table_name >>< 出力ファイル > 2 > & 1'

   | 引数| 説明|
   |---|---|
   | < サーバー名 >| ソース サーバー名|
   | < database_name >| 移行元データベースの名前|
   | < target_file >| BACPAC ファイルのファイル名と場所|
   | < schema_name.table_name >| ターゲット ファイルに出力するデータのテーブル|
   | < 出力ファイル >| エラーがある場合、エラーのある出力ファイルのファイル名と場所|

    /p:TableName 引数を指定するのは、すべてのテーブルからデータをエクスポートするのではなく、Azure SQL DB V12 にエクスポートするためのデータベースの互換性をテストするだけであるためです。残念ながら、sqlpackage.exe の export{b> <b}引数ではテーブルを抽出しないように指定することはできないので、小さいテーブルを 1 つ指定する必要があります。< 出力ファイル > は、エラーのレポートが格納されます。「> 2>&1」文字列は、標準出力とコマンド実行の結果として生成された標準エラーの両方を指定の出力ファイルにパイプ処理します。

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01.png)

3. 出力ファイルを開き、互換性エラーがある場合は確認します。 データベースの互換性の問題を修正する方法のガイダンスについてを参照してください [データベースの互換性の問題を修正](#fix-database-compatibility-issues)します。

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage02.png)

## データ層アプリケーションのエクスポートを使用してデータベースの互換性を調べる

1. SQL Server Management Studio がバージョン 13.0.600.65 以降であることを確認します。 Management Studio は毎月新しいバージョンに更新されて、Azure ポータルの更新との同期が維持されます。
     > [AZURE.IMPORTANT] ダウンロード、 [最新](https://msdn.microsoft.com/library/mt238290.aspx) SQL Server Management Studio のバージョン。 最新バージョンの Management Studio を使用することを常にお勧めします。

2. Management Studio を開き、オブジェクト エクスプローラーで移行元データベースに接続します。
3. オブジェクト エクスプローラーで移行元データベースを右クリックし、**[タスク]** をポイントして **[データ層アプリケーションのエクスポート...]** をクリックします。

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. エクスポート ウィザードで **[次へ]** をクリックし、**[設定]** タブで、BACPAC ファイルがローカル ディスクまたは Azure BLOB に保存されるようにエクスポートを構成します。 BACPAC ファイルは、データベースの互換性に問題がない場合にのみ保存されます。 互換性の問題がある場合は、コンソールに表示されます。

    ![設定のエクスポート](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS02.png)

5. **[詳細設定]** タブをクリックし、**[すべて選択]** チェック ボックスをオフにしてデータのエクスポートをスキップします。 ここでの目的は、互換性をテストすることだけです。

    ![設定のエクスポート](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS03.png)

6. **[次へ]** をクリックし、**[完了]** をクリックします。 データベースの互換性に問題がある場合は、ウィザードによるスキーマの検証後に表示されます。

    ![設定のエクスポート](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS04.png)

7. エラーが表示されない場合は、データベースは互換性があり、移行できる状態です。 エラーがある場合は、修正する必要があります。 エラーを表示するには、**[スキーマの検証]** の **[エラー]** をクリックします。 これらのエラー修正に移動する方法について [データベースの互換性の問題を修正](#fix-database-compatibility-issues)します。

    ![設定のエクスポート](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS05.png)

8.  *.BACPAC ファイルが生成されたら、お使いのデータベースは SQL Database と互換性があるので移行できます。

## 互換性のあるデータベースを Azure SQL Database に移行するためのオプション

データベースの互換性が確認されたら、移行方法を選択する必要があります。 最初に、移行中、データベースを本稼働から外す余裕があるかどうかを決定する必要があります。 ない場合、SQL Server トランザクション レプリケーションを使用します。これについては後に説明します。 ある程度のダウンタイムが許される場合、あるいは本稼働データベースの移行をテストしている場合 (後にトランザクション レプリケーションの利用を選択する可能性がある)、次の 3 つの方法のいずれかを検討してください。

### 互換性のあるデータベースをダウンタイムありで移行する

次の一覧では、移行中と、Azure SQL Database に移行したデータベースにユーザーとアプリケーションをポイントする前に、ある程度のダウンタイムが許されるとき、互換性のあるデータベースを移行する際の選択肢についてまとめてあります。 これらの方法では、特定の時点に存在するものとしてデータベースを移行します。
> [AZURE.WARNING] これらの方法のいずれかでデータベースを移行する前に、移行中にトランザクションの整合性が維持されるように、アクティブなトランザクションがないことを確認します。 データベースの作成へのクライアント接続を無効にすることを停止する多くのメソッドがある、 [データベース スナップショット](https://msdn.microsoft.com/library/ms175876.aspx)します。

- 小規模から中規模のデータベースを移行するため、 [互換性のある](#determine-if-your-database-is-compatible) SQL Server 2005 またはそれ以降のデータベースが実行されている同じくらい簡単です、 [データベース Microsoft Azure Database ウィザードへのデプロイ](#use-deploy-database-to-microsoft-azure-database-wizard) SQL Server Management Studio でします。 接続に関する課題 (接続、低帯域幅やタイムアウトの問題) の場合は、 [BACPAC を使用して移行する](#use-a-bacpac-to-migrate-a-sql-server-database-to-azure-sql-database) Azure SQL Database への SQL Server データベース。
- 中規模から大規模なデータベースまたは接続に関する課題がある場合に [BACPAC を使用して移行する](#use-a-bacpac-to-migrate-a-sql-server-database-to-azure-sql-database) Azure SQL Database への SQL Server データベース。 このメソッドを使用してデータとスキーマをエクスポートする SQL Server Management Studio を使用する、 [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) (ローカルまたは Azure blob に格納されている) ファイルと、Azure の SQL インスタンスに、BACPAC ファイルをインポートします。 内から BACPAC ファイルをインポートする BACPAC を Azure blob に保存する場合もできる、 [Azure ポータル](sql-database-import.md) または [PowerShell を使用して](sql-database-import-powershell.md)します。
- 大規模なデータベースの場合は、スキーマとデータを個別に移行することでパフォーマンスを最適化できます。 このメソッドを使用して作成、 [データなしで BACPAC ファイル](#use-a-bacpac-to-migrate-a-sql-server-database-to-azure-sql-database) Azure SQL データベースにこの BACPAC をインポートします。 Azure SQL データベースにスキーマをインポート後、使用する [BCP](https://msdn.microsoft.com/library/ms162802.aspx) をフラット ファイルへのデータを抽出し、Azure SQL データベースにこれらのファイルをインポートします。

     ![SSMS の移行ダイアグラム](./media/sql-database-cloud-migrate/01SSMSDiagram_new.png)

### 互換性のあるデータベースをダウンタイムなしで移行する

移行中、SQL Server データベースを外す余裕がない場合、移行ソリューションとして SQL Server トランザクション レプリケーションを使用できます。 トランザクション レプリケーションでは、移行を開始した瞬間から移行を完了した瞬間までに発生したデータやスキーマの変更が Azure SQL Database に表示されます。 移行が完了したら、アプリケーションの接続文字列を変更し、オンプレミス データベースの代わりに Azure SQL Database にポイントします。 トランザクション レプリケーションがオンプレミス データベースに残っているすべての変更を抜き取り、すべてのアプリケーションが Azure DB をポイントしたら、Azure SQL Database を本稼働システムとして残し、レプリケーションを安全にアンインストールできます。

 ![SeedCloudTR ダイアグラム](./media/sql-database-cloud-migrate/SeedCloudTR.png)


トランザクション レプリケーションは、SQL Server 6.5 以降、SQL Server に内蔵された技術です。 非常に成熟した、証明済みの技術であり、ほとんどの DBA に運用経験があります。  [SQL Server 2016 preview](http://www.microsoft.com/server-cloud/products/sql-server-2016/), 、として Azure SQL Database を構成することが今すぐ、 [トランザクション レプリケーションのサブスクライバー](https://msdn.microsoft.com/library/mt589530.aspx) 、内部設置型の文書にします。 Management Studio の設定は、オンプレミス サーバーでトランザクション レプリケーション サブスクライバーを設定する場合とまったく同じです。 このシナリオは次の SQL Server バージョンでサポートされます。

 - SQL Server 2016 CTP3 (プレビュー) 以降
 - SQL Server 2014 SP1 CU3 以降
 - SQL Server 2014 RTM CU10 以降
 - SQL Server 2012 SP2 CU8 以降
 - SQL Server 2013 SP3 のリリース時

また、トランザクション レプリケーションを使用し、オンプレミス データベースの一部を移行できます。 Azure SQL Database に複製するパブリケーションは、複製されるデータベースのテーブルの一部に制限できます。 また、複製されるテーブルごとに、行の一部または列の一部にデータを制限できます。

## Microsoft Azure SQL Database へのデータベースのデプロイ ウィザードを使用する

SQL Server Management Studio での Microsoft Azure Database ウィザードへのデータベースのデプロイを移行、移行、 [互換性のある](#determine-if-your-database-is-compatible) SQL Server 2005 またはそれ以降のデータベース、Azure SQL の論理サーバー インスタンスに直接します。
> [AZURE.NOTE] 次の手順は、既にあることを前提としています。 [プロビジョニング](../sql-database-get-started.md) 、Azure SQL の論理インスタンスと接続情報を手元にがあります。

1. SQL Server Management Studio がバージョン 13.0.600.65 以降であることを確認します。 Management Studio は毎月新しいバージョンに更新されて、Azure ポータルの更新との同期が維持されます。
     > [AZURE.IMPORTANT] ダウンロード、 [最新](https://msdn.microsoft.com/library/mt238290.aspx) SQL Server Management Studio のバージョン。 最新バージョンの Management Studio を使用することを常にお勧めします。

2. Management Studio を開き、オブジェクト エクスプローラーで移行元データベースに接続します。
3. オブジェクト エクスプローラーで移行元データベースを右クリックし、**[タスク]** をポイントして **[Microsoft Azure SQL Database へのデータベースのデプロイ...]** をクリックします。

    ![[タスク] メニューの "Azure へのデプロイ"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard01.png)

4.  デプロイメント ウィザードで、**[次へ]** をクリックし、**[接続]** をクリックし、Azure SQL Database サーバーへの接続を構成します。

    ![[タスク] メニューの "Azure へのデプロイ"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard002.png)

5. [サーバーに接続] ダイアログ ボックスで、接続情報を入力し、Azure SQL Database サーバーに接続します。

    ![[タスク] メニューの "Azure へのデプロイ"](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard00.png)

5.  Azure SQL DB でのデータベースの **[新しいデータベース名]** を指定し、**[Microsoft Azure SQL Database のエディション]** (サービス階層)、**[データベースの最大サイズ]**、**[サービスの目的]** (パフォーマンス レベル)、および移行プロセスの間にこのウィザードが作成する BACPAC ファイルの **[一時ファイル名]** を設定します。 参照してください [Azure SQL Database サービス階層](sql-database-service-tiers.md) 詳細については、サービス階層とパフォーマンス レベル。

    ![設定のエクスポート](./media/sql-database-cloud-migrate/MigrateUsingDeploymentWizard02.png)

6.  データベースを移行するウィザードを完了します。 データベースのサイズと複雑さに応じて、デプロイメントには数分から数時間かかる場合があります。
7.  オブジェクト エクスプローラーを使用して、Azure SQL Database サーバーの移行されたデータベースに接続します。
8.  Azure ポータルを使用して、データベースとそのプロパティを表示します。

## BACPAC を使用して SQL Server データベースを Azure SQL Database に移行する

中規模から大規模のデータベースの場合、または接続に問題がある場合は、移行プロセスを 2 つの個別の手順に分けることができます。 スキーマとそのデータをエクスポートすること、 [BACPAC](https://msdn.microsoft.com/library/ee210546.aspx#Anchor_4) ファイルの 1 つまたは 2 つのメソッドを使用します。

- [SQL Server Management Studio を使用して BACPAC ファイルにエクスポートします。](#export-a-compatible-sql-server-database-to-a-bacpac-file-using-sql-server-management-studio)
- [SqlPackage を使用して BACPAC をエクスポートします。](#export-a-compatible-sql-server-database-to-a-bacpac-file-using-sqlpackage)

この BACPAC は、ローカルまたは Azure BLOB に保存できます。 その後、次のいずれかの方法を使用してこの BACPAC ファイルを Azure SQL Database にインポートします。

- [SQL Server Management Studio を使用して Azure SQL データベースに BACPAC ファイルからインポートします。](#import-from-a-bacpac-file-into-azure-sql-database-using-sql-server-management-studio)
- [SqlPackage を使用して Azure SQL データベースに BACPAC ファイルからインポートします。](#import-from-a-bacpac-file-into-azure-sql-database-using-sqlpackage)
- [Azure ポータルを使用して Azure SQL データベースに BACPAC ファイルからインポートします。](sql-database-import.md)
- [使用して Azure SQL データベースまたは PowerShell に BACPAC ファイルからインポートします。](sql-database-import-powershell.md)

## SQL Server Management Studio を使用して互換性のある SQL Server データベースを BACPAC ファイルにエクスポートする

Management Studio を使用して、移行をエクスポートするを次の手順を使用して、 [互換性のある](#determine-if-your-database-is-compatible) SQL Server データベースを BACPAC ファイルにします。

1. SQL Server Management Studio がバージョン 13.0.600.65 以降であることを確認します。 Management Studio は毎月新しいバージョンに更新されて、Azure ポータルの更新との同期が維持されます。
     > [AZURE.IMPORTANT] ダウンロード、 [最新](https://msdn.microsoft.com/library/mt238290.aspx) SQL Server Management Studio のバージョン。 最新バージョンの Management Studio を使用することを常にお勧めします。

2. Management Studio を開き、オブジェクト エクスプローラーで移行元データベースに接続します。

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. オブジェクト エクスプローラーで移行元データベースを右クリックし、**[タスク]** をポイントして **[データ層アプリケーションのエクスポート...]** をクリックします。

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSSMS01.png)

4. エクスポート ウィザードで、BACPAC ファイルがローカル ディスクまたは Azure BLOB に保存されるようにエクスポートを構成します。 エクスポートされた BACPAC には、常にデータベース スキーマ全体が含まれ、既定ではすべてのテーブルのデータが含まれます。 一部またはすべてのテーブルからデータを除外する場合は、[詳細設定] タブを使用してください。 たとえば、すべてのテーブルではなく参照テーブルのデータのみをエクスポートするように選択できます。

    ![設定のエクスポート](./media/sql-database-cloud-migrate/MigrateUsingBACPAC02.png)

## SqlPackage を使用して互換性のある SQL Server データベースを BACPAC ファイルにエクスポートする

次の手順を使用して、 [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) 、移行をエクスポートするコマンド ライン ユーティリティ、 [互換性のある](#determine-if-your-database-is-compatible) データベースを BACPAC ファイルにします。
> [AZURE.NOTE] Azure SQL Database サーバーを既にプロビジョニングしてあり、接続情報がわかっていて、移行元データベースに互換性があることを検証済みであるものとします。

1. コマンド プロンプトを開き、sqlpackage.exe コマンド ライン ユーティリティのあるディレクトリに移動します。このユーティリティは、Visual Studio および SQL Server に付属します。
2. 次の sqlpackage.exe コマンドをご利用の環境に合わせた引数で実行します。

    ' sqlpackage.exe/Action:Export/ssn: < server_name >/sdn: < database_name >/tf: < target_file >

   | 引数| 説明|
   |---|---|
   | < サーバー名 >| ソース サーバー名|
   | < database_name >| 移行元データベースの名前|
   | < target_file >| BACPAC ファイルのファイル名と場所|

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01b.png)

## SQL Server Management Studio を使用して BACPAC ファイルを Azure SQL Database にインポートする

以下の手順を使用して、BACPAC ファイルを Azure SQL Database にインポートします。
> [AZURE.NOTE] 以下の手順では、Azure SQL 論理インスタンスを既にプロビジョニングしてあり、接続情報がわかっているものとします。

1. SQL Server Management Studio がバージョン 13.0.600.65 以降であることを確認します。 Management Studio は毎月新しいバージョンに更新されて、Azure ポータルの更新との同期が維持されます。
    > [AZURE.IMPORTANT] ダウンロード、 [最新](https://msdn.microsoft.com/library/mt238290.aspx) SQL Server Management Studio のバージョン。 最新バージョンの Management Studio を使用することを常にお勧めします。

2. Management Studio を開き、オブジェクト エクスプローラーで移行元データベースに接続します。

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/MigrateUsingBACPAC01.png)

3. BACPAC が作成されたら、Azure SQL Database サーバーに接続し、**[データベース]** フォルダーを右クリックして、**[データ層アプリケーションのインポート...]** をクリックします。

    ![[データ層アプリケーションのインポート] メニュー項目](./media/sql-database-cloud-migrate/MigrateUsingBACPAC03.png)

4.  インポート ウィザードで、エクスポートした BACPAC ファイルを選択して、Azure SQL Database に新しいデータベースを作成します。

    ![設定のインポート](./media/sql-database-cloud-migrate/MigrateUsingBACPAC04.png)

5.  Azure SQL DB でのデータベースの **[新しいデータベース名]** を指定し、**[Microsoft Azure SQL Database のエディション]** (サービス階層)、**[データベースの最大サイズ]**、および **[サービスの目的]** (パフォーマンス レベル) を設定します。

    ![データベース設定](./media/sql-database-cloud-migrate/MigrateUsingBACPAC05.png)

6.  **[次へ]**、**[完了]** の順にクリックし、Azure SQL Database サーバーの新しいデータベースに BACPAC ファイルをインポートします。

7. オブジェクト エクスプローラーを使用して、Azure SQL Database サーバーの移行されたデータベースに接続します。

8.  Azure ポータルを使用して、データベースとそのプロパティを表示します。

## SqlPackage を使用して BACPAC ファイルを Azure SQL Database にインポートする

次の手順を使用して、 [SqlPackage.exe](https://msdn.microsoft.com/library/hh550080.aspx) BACPAC ファイルから SQL Server データベースの互換性のある (または Azure SQL データベース) をインポートするコマンド ライン ユーティリティです。
> [AZURE.NOTE] Azure SQL Database サーバーを既にプロビジョニングしてあり、接続情報がわかっているものとします。

1. コマンド プロンプトを開き、sqlpackage.exe コマンド ライン ユーティリティのあるディレクトリに移動します。このユーティリティは、Visual Studio および SQL Server に付属します。
2. 次の sqlpackage.exe コマンドをご利用の環境に合わせた引数で実行します。

    ' sqlpackage.exe/Action:Import/tsn: < server_name >/tdn: < database_name >/tu: < user_name >/tp: < パスワード >/sf: < source_file >

   | 引数| 説明|
   |---|---|
   | < サーバー名 >| ターゲット サーバー名|
   | < database_name >| ターゲット データベース名|
   | < ユーザー名 >| ターゲット サーバーのユーザー名|
   | < パスワード >| ユーザーのパスワード|
   | < source_file >| インポートする BACPAC ファイルのファイル名と場所|

    ![[タスク] メニューの ](./media/sql-database-cloud-migrate/TestForCompatibilityUsingSQLPackage01c.png)


## データベースの互換性の問題を修正する

ソース SQL Server データベースの互換性がないことは、さまざまなデータベースの互換性を解決するオプションがあると判断した場合の問題を [確認済み](#determine-if-your-database-is-compatible)します。

- 使用して、 [SQL Azure 移行ウィザード](http://sqlazuremw.codeplex.com/)します。 この Codeplex ツールを使うと、互換性のない移行元データベースから T-SQL スクリプトを生成した後、SQL Database に対応するようにこのスクリプトを変換し、Azure SQL Database に接続してこのスクリプトを実行できます。 このツールは、トレース ファイルを分析して互換性の問題の特定も行います。 スキーマのみを含むスクリプトを生成することも、BCP 形式のデータを含むスクリプトを生成することもできます。 その他のドキュメントは codeplex のステップ バイ ステップ ガイダンスを含む、 [SQL Azure 移行ウィザード](http://sqlazuremw.codeplex.com/)します。

 ![SAMW の移行ダイアグラム](./media/sql-database-cloud-migrate/02SAMWDiagram.png)
 > [AZURE.NOTE] ウィザードで検出できるすべての非互換スキーマを組み込み変換で処理できるとは限りません。 処理できない非互換スクリプトは、生成されたスクリプトにコメントが挿入され、エラーとして報告されます。 多くのエラーが検出された場合は、Visual Studio または SQL Server Management Studio を使用して、SQL Server 移行ウィザードでは修正できない各エラーを修正します。

- Visual Studio を使用します。 Visual Studio を使用すると、データベース スキーマを Visual Studio データベース プロジェクトにインポートして分析できます。 分析するには、SQL Database V12 にプロジェクトのターゲット プラットフォームを指定し、プロジェクトを構築します。 構築に成功した場合は、データベースには互換性があります。 構築に失敗した場合は、Visual Studio 用の SQL Server Data Tools ("SSDT") でエラーを解決できます。 プロジェクトが正常に構築されたら、これをソース データベースのコピーとして発行し、SSDT のデータ比較機能を使用して、ソース データベースから Azure SQL V12 互換のデータベースにデータをコピーできます。 この更新されたデータベースがオプションを使用して Azure SQL データベースに展開し、 [前に説明した](#options-to-migrate-a-compatible-database-to-azure-sql-database)します。

 ![VSSSDT の移行ダイアグラム](./media/sql-database-cloud-migrate/03VSSSDTDiagram.png)
 > [AZURE.NOTE] スキーマのみの移行が必要な場合は、Visual Studio から Azure SQL Database に直接、発行することができます。 この方法は移行ウィザードのみで処理できる変更よりも多数の変更を、データベース スキーマで行う必要がある場合に使用します。

- SQL Server Management Studio。 Management Studio では、**ALTER DATABASE** などのさまざまな Transact-SQL コマンドを使用して問題を修正できます。





