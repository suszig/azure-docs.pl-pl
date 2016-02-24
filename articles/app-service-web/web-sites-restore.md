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

1.  **設定** Azure ポータルでアプリのブレードをクリックして **バックアップ** を表示する、 **バックアップ** ブレードです。 クリックして **今すぐ復元** コマンド バーにします。 
    
    ![今すぐ復元の選択][ChooseRestoreNow]

3.  **復元** ブレードで、まず、バックアップ元を選択します。 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
     **アプリのバックアップ** オプションには、これらは、ほかのアプリケーションが認識するので、アプリ自体で直接作成されるすべてのバックアップが示しています。 1 つのバックアップを簡単に選択できます。 
     **ストレージ** オプションを使用してストレージ アカウントからの実際のバックアップの ZIP ファイルを選択し、コンテナーで構成されている、 **バックアップ** ブレードです。 その他のアプリでのバックアップ ファイルがある場合 
    コンテナーも復元することを選択できます。  

4. 次に、アプリの復元の宛先の指定 **の復元先の**です。

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] 選択した場合 **上書き**, 、既存のアプリに関連するすべてのデータが削除されます。 クリックする前に **OK**,、
    これであることを目的に合ったを確認します。
    
    選択できる **既存のアプリ** を同じリソース グループ内の別のアプリにアプリのバックアップを復元します。 このオプションを使用する前に 
    既に作成している別のアプリケーション、リソース グループに定義されているデータベースの構成をミラー化
    アプリのバックアップ。 
    
5. Click **OK**.

<a name="StorageAccount"></a>
## ストレージ アカウントからバックアップをダウンロードまたは削除するには
    
1. メイン **参照** 選択である Azure ポータルのブレード **ストレージ アカウント**します。
    
    既存のストレージ アカウントの一覧が表示されます。 
    
2. ダウンロードまたは削除するバックアップが含まれているストレージ アカウントを選択します。
    
     **ストレージ** ブレードが表示されます。

3. 選択、 **コンテナー** 部、 **ストレージ** ブレードを表示、 **コンテナー** ブレードです。
    
    コンテナーの一覧が表示されます。 この一覧には、コンテナーの URL と最終変更日も表示されます。
    
    ![コンテナーの表示][ViewContainers]

4. 一覧でコンテナーを選択してブレードを表示すると、ファイル名の一覧と各ファイルのサイズが表示されます。
    
5. ファイルを選択することができますかを選択する **ダウンロード** または **削除** ファイルです。 主に使用するファイル タイプは、.zip ファイルと .xml ファイルの 2 種類です。 

<a name="OperationLogs"></a>
## 監査ログを表示するには
    
1. 詳細については、成功または失敗、アプリケーションの復元操作を選択して、 **監査ログ** 、メインの **参照** ブレードです。 
    
     **オーディオ ログ** ブレードでは、すべてのレベル、状態、リソース、および時間の詳細情報と共に、操作が表示されます。
    
2. ブレードをスクロールして、アプリに関連する操作を検索します。
3. 操作に関する追加情報を表示するには、一覧で操作を選択します。
    
詳細ブレードに、操作に関連する利用可能な情報が表示されます。
    
>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。
    
## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
