<properties 
    pageTitle="SQL Database の更新プログラムの V12 でデータベースを作成する" 
    description="Azure SQL Database の更新プログラムの V12 でデータベースを作成する方法を示しています" 
    services="sql-database" 
    documentationCenter="" 
    authors="sonalmm" 
    manager="jeffreyg" 
    editor=""/>

<tags 
    ms.service="sql-database" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-management" 
    ms.date="04/28/2015" 
    ms.author="sonalm"/>


# SQL Database V12 でデータベースを作成する


<!--
True author is: authors="sonalmm" , ms.author="sonalm".
-->


[サインアップ](https://portal.azure.com) SQL Database V12 の [(一部の地域ではプレビュー版)](sql-database-v12-whats-new.md#V12AzureSqlDbPreviewGaTable) SQL データベースの次世代の Microsoft Azure を利用します。 使用を開始するには、Microsoft Azure のサブスクリプションが必要です。 サインアップするため、 [無料評価版の Azure](http://azure.microsoft.com/pricing/free-trial) 確認と [料金](http://azure.microsoft.com/pricing/details/sql-database) 情報。 


| データベースを作成する | スクリーン ショット |
| :--- | :--- |
| 1.サインイン [http://portal.azure.com/](http://portal.azure.com/)します。 | ![新しい Azure クラシック ポータル][1] |
| 2.左側で、ページの下部にあるをクリックして **新規**します。 | ![新しいサービスを開始する][2]|
| 3.クリックして **SQL データベース**します。| ![から選択する別のサービス][3] |
| 4.A **SQL データベース** ブレードが開きます。  **名前** フィールドに、データベース名を指定します。 | ![データベースの名前][4] |
| 5. **SQL Database] ブレード**, 、クリックして **SERVER**します。 A **Server** 2 つの選択肢を提供するブレードが開きます。 新しいサーバーを作成するか、既存のサーバーを使用することができますか。| ![サーバーの種類の選択][4] |
|5a. 選択した場合 **既存のサーバーを使用して** オプションを任意のサーバーの選択] をクリックして **選択**します。 次に、手順 6 以降のすべての処理を完了します。| ![一覧からサーバーを選択][5]| 
|5b.   選択した場合 **サーバーの新規作成**, 、 **新しいサーバー** ブレードが開きます。 サーバー名、サーバー管理ログイン、パスワードを指定します。 クリックして **場所** サーバーの場所を選択します。 | ![完全なサーバーの新しいオプションを作成する][9]| 
|5 c です。 **新しいサーバー** ブレードでは、V12 更新プログラムを新しいサーバーを作成するオプションを選択します。 V12 サーバーの機能の詳細については、次のように確認します。 [SQL Database V12 の新機能](sql-database-v12-whats-new.md)です。| ![V12 サーバーの選択][6]|
|5d. 選択を行います、 **新しいサーバー** ブレードをクリック **OK**します。 時間がかかりますに戻る、 **SQL Database** ブレードを残りのデータベースを作成する操作を完了します。 | ![新しいサーバー ブレードの操作を行います][8]|
|6.クリックして **ソースを選択して**します。 データベースを作成するために選択できるソースの種類として、空のデータベース、サンプル データベース、データベースのバックアップがあります。| ![データベースのソースの選択][10]|
|7.次に、 **SQL データベース** ブレードで、をクリックして **料金レベル**します。 推奨される価格レベルのいずれかを選択または **すべてを表示** 価格レベルを使用します。 選択が完了したら、クリックして **選択**します。 <p> 価格レベルの詳細については、次を参照してください。 [を新しいサービス階層 SQL Database Web/ビジネス データベースをアップグレード](./sql-database-upgrade-new-service-tiers/) と [Azure SQL Database サービス階層とパフォーマンス レベル](sql-database-service-tiers.md)します。 |![価格レベルの選択][7]
| 8.次に、 **SQL データベース** ブレードで、をクリックして **オプションの構成**, の選択を行い、] をクリックして **OK**します。 
| 9.既存のサーバーを選択すると **リソース グループ** と **サブスクリプション** が既に選択します。  **SQL データベース** ブレードで、横にロック アイコンが表示されます **リソース グループ** と **サブスクリプション**します。 新しいサーバーを作成する場合は、リソース グループを選択するか、作成します。 詳細については、「 [リソース グループを使用した Azure リソースを管理します。](resource-group-overview.md)|![リソース グループを指定する][11]
| 10.クリックして **作成**します。 SQL Database V12 の機能を持った新しいデータベースが作成されます。 |![新しいデータベースを作成する][12]

## 関連リンク

- [SQL Database V12 の新機能](sql-database-v12-whats-new.md)
- [Azure SQL Database V12 へのアップグレードの計画と準備をする](sql-database-v12-plan-prepare-upgrade.md)

<!--Image references-->
[1]: ./media/sql-database-create/firstscreenportal.png
[2]: ./media/sql-database-create/new.png
[3]: ./media/sql-database-create/sqldatabase.png
[4]: ./media/sql-database-create/databasename.png
[5]: ./media/sql-database-create/useexistingserver.PNG
[6]: ./media/sql-database-create/v12server.PNG
[7]: ./media/sql-database-create/pricingtierdetails.png
[8]: ./media/sql-database-create/finishnewserverblade.png
[9]: ./media/sql-database-create/createnewserver.png
[10]: ./media/sql-database-create/selectsource.png
[11]: ./media/sql-database-create/resourcegroup.png
[12]: ./media/sql-database-create/create.png

 
