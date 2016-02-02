<properties 
    pageTitle="Azure App Service でのアプリの復元" 
    description="アプリをバックアップから復元する方法について説明します。" 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="12/11/2015" 
    ms.author="cephalin"/>


# Azure App Service でのアプリの復元

ここで説明する以前にバックアップを使用して App Service アプリを復元する方法、 [App Service](app-service-value-prop-what-is) バックアップ機能。 詳細については、次を参照してください。 [App Service のバックアップ](web-sites-backup.md)します。

App Service の復元機能を使用すると、リンクされたデータベース (SQL Database または MySQL) をオンデマンドで使用してアプリを以前の状態に戻したり、元のアプリのいずれかのバックアップに基づいて新しいアプリを作成したりすることができます。 最新バージョンと並行して実行される新しいアプリを作成すると、A/B テストを実施する場合に役立ちます。

アプリ サービスは、復元、配置されている機能、 **バックアップ** ブレードで、 [Azure ポータル](http://portal.azure.com), 、Standard および Premium 価格レベルでのみ使用できます。 Standard または Premium 階層を使用して、アプリのスケーリングについては、次を参照してください。 [Azure App Service でのアプリケーションのスケーリング](web-sites-scale.md)します。 Premium レベルでは、日次バックアップの実行回数を Standard レベルよりも多く設定できます。

<a name="PreviousBackup"></a>
## 以前に作成したバックアップからアプリを復元するには

1. Azure ポータルにあるアプリの **[設定]** ブレードで、**[Backup]** をクリックして **[バックアップ]** ブレードを表示します。 コマンド バーで、**[今すぐ復元]** をクリックします。

    ![今すぐ復元の選択][chooserestorenow]

3. **[復元]** ブレードで、最初にバックアップ ソースを選択します。

    ![](./media/web-sites-restore/021ChooseSource.png)

    **[アプリのバックアップ]** オプションには、アプリ自体で直接作成されたすべてのバックアップが表示されます。アプリが認識するのはこれらのバックアップのみです。 1 つのバックアップを簡単に選択できます。 
    **[Storage]** オプションを使用して、**[Backup]** ブレードで構成されたストレージ アカウントおよびコンテナーから実際のバックアップ zip ファイルを選択できます。 その他のアプリでのバックアップ ファイルがある場合 
    コンテナーも復元することを選択できます。

4. 次に、**[復元先]** でアプリの復元先を指定します。

    ![](./media/web-sites-restore/022ChooseDestination.png)
    >[AZURE.WARNING] **[上書き]** を選択すると、既存のアプリに関連するすべてのデータが削除されます。 クリックする前に **OK**,、
    これであることを目的に合ったを確認します。

    **[既存のアプリ]** を選択して、アプリのバックアップを同じリソース グループ内の別のアプリに復元することができます。 このオプションを使用する前に 
    既に作成している別のアプリケーション、リソース グループに定義されているデータベースの構成をミラー化
    アプリのバックアップ。

5. **[OK]** をクリックします。

<a name="StorageAccount"></a>
## ストレージ アカウントからバックアップをダウンロードまたは削除するには

1. Azure ポータルのメインの **[参照]** ブレードで、**[ストレージ アカウント]** を選択します。

    既存のストレージ アカウントの一覧が表示されます。

2. ダウンロードまたは削除するバックアップが含まれているストレージ アカウントを選択します。

    **[ストレージ]** ブレードが表示されます。

3. **[ストレージ]** ブレードで **[コンテナー]** を選択し、**[コンテナー]** ブレードを表示します。

    コンテナーの一覧が表示されます。 この一覧には、コンテナーの URL と最終変更日も表示されます。

    ![コンテナーの表示][viewcontainers]

4. 一覧でコンテナーを選択してブレードを表示すると、ファイル名の一覧と各ファイルのサイズが表示されます。

5. ファイルを選択すると、ファイルを**ダウンロード**するか**削除**するかを選択できます。 主に使用するファイル タイプは、.zip ファイルと .xml ファイルの 2 種類です。

<a name="OperationLogs"></a>
## 監査ログを表示するには

1. アプリの復元操作が成功したかどうかについて詳細を確認するには、メインの **[参照]** ブレードで **[監査ログ]** を選択します。

    **[監査ログ]** ブレードには、すべての操作と、レベル、状態、リソース、および時刻が表示されます。

2. ブレードをスクロールして、アプリに関連する操作を検索します。
3. 操作に関する追加情報を表示するには、一覧で操作を選択します。

詳細ブレードに、操作に関連する利用可能な情報が表示されます。
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)



[chooserestorenow]: ./media/web-sites-restore/02ChooseRestoreNow.png 
[viewcontainers]: ./media/web-sites-restore/03ViewContainers.png 
[storageaccountfile]: ./media/web-sites-restore/02StorageAccountFile.png 
[browsecloudstorage]: ./media/web-sites-restore/03BrowseCloudStorage.png 
[storageaccountfileselected]: ./media/web-sites-restore/04StorageAccountFileSelected.png 
[chooserestoresettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png 
[choosedbserver]: ./media/web-sites-restore/06ChooseDBServer.png 
[restoretonewsqldb]: ./media/web-sites-restore/07RestoreToNewSQLDB.png 
[newsqldbconfig]: ./media/web-sites-restore/08NewSQLDBConfig.png 
[restoredcontosowebsite]: ./media/web-sites-restore/09RestoredContosoWebSite.png 
[dashboardoperationlogslink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png 
[managementservicesoperationlogslist]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png 
[detailsbutton]: ./media/web-sites-restore/12DetailsButton.png 
[operationdetails]: ./media/web-sites-restore/13OperationDetails.png 

