<properties 
    pageTitle="Azure App Service での Web アプリのバックアップ" 
    description="Azure App Service で Web アプリのバックアップを作成する方法を説明します。" 
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
    ms.date="12/10/2015" 
    ms.author="cephalin"/>

# Azure App Service での Web アプリのバックアップ


バックアップと復元の機能 [Azure App Service Web Apps](http://go.microsoft.com/fwlink/?LinkId=529714) 簡単に web アプリのバックアップ手動または自動作成することができます。 Web アプリを以前の状態に復元するか、または元のアプリのいずれかのバックアップに基づいて、新しい Web アプリを作成できます。 

Azure の web アプリをバックアップから復元する方法については、次を参照してください。 [web アプリの復元](web-sites-restore.md)します。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

<a name="whatsbackedup"></a>
## バックアップ対象 
Web Apps では、次の情報をバックアップできます。

* Web アプリの構成
* Web アプリ ファイルの内容
* アプリに接続されているすべての Azure SQL データベースまたは MySQL データベース (バックアップに含めるデータベースを選択できます)

この情報は、指定した Azure のストレージ アカウントとコンテナーにバックアップされます。 

> [AZURE.NOTE] 各バックアップは、増分更新ではないアプリの完全なオフライン コピーです。

<a name="requirements"></a>
## 要件および制限

* バックアップと復元の機能には、Standard 以上のレベルにある App Service プランが必要です。 上の階層を使用する App Service プランのスケーリングの詳細については、次を参照してください。 [Azure App Service で web アプリのスケール](web-sites-scale.md)します。 Premium レベルでは、Standard レベルよりも多くの回数の日次バックアップが可能です。

* バックアップと復元の機能では、バックアップする Web アプリと同じサブスクリプションに属する Azure ストレージ アカウントとコンテナーが必要です。 ストレージ アカウントがあるまだない場合は、作成する] をクリックして、 **ストレージ アカウント** で、 **バックアップ** のブレード、 [Azure ポータル](http://portal.azure.com), 、選択し、 **ストレージ アカウント** と **コンテナー** から、 **宛先** ブレードです。 Azure ストレージ アカウントの詳細については、次を参照してください。、 [リンク](#moreaboutstorage) この記事の最後にします。

* バックアップと復元の機能では、Web サイトとデータベースの最大 10 GB のコンテンツをサポートします。 ペイロードがこの上限を超えるためにバックアップ機能を続行できない場合は、エラーが示されます。 

<a name="manualbackup"></a>
## 手動バックアップの作成

1. Azure ポータルでは、[Web Apps] ブレードから Web アプリを選択します。 これにより、新しいブレードで、Web アプリの詳細が表示されます。
2. アプリのブレードで、選択 **設定**, 、し **バックアップ**します。  **バックアップ** ブレードが表示されます。
    
    ![バックアップ] ページで][ChooseBackupsPage]

3.  **バックアップ** ブレードで、をクリックして **ストレージ: 構成されていない** ストレージ アカウントを構成します。

    ![ストレージ アカウントの選択][ChooseStorageAccount]
    
4. 選択して、バックアップ先を選択して、 **ストレージ アカウント** と **コンテナー**します。 ストレージ アカウントは、バックアップする Web アプリと同じサブスクリプションに属する必要があります。 必要に応じて、各ブレードで新しいストレージ アカウントまたは新しいコンテナーを作成できます。 完了したら、クリックして **選択**します。
    
    ![ストレージ アカウントの選択](./media/web-sites-backup/02ChooseStorageAccount1.png)
    
5.  **バックアップ設定を構成する** も開いたままであるブレードをクリックして **データベースの設定**, をクリックし、(SQL データベースまたは MySQL) のバックアップに含めるデータベースを選択 **OK**します。  

    ![ストレージ アカウントの選択](./media/web-sites-backup/03ConfigureDatabase.png)

    > [AZURE.NOTE]  この一覧に表示されるデータベースの接続文字列が存在する必要があります、 **接続文字列** のセクション、 **Web アプリ設定** ポータルのブレードでします。

6.  **バックアップ設定を構成する** ブレードで、をクリックして **保存**します。  
6.  **バックアップ** ブレードで、 **バックアップ先**します。 既存のストレージ アカウントとコンテナーを選択する必要があります。
7. コマンド バーで、 **バックアップ** ブレードで、をクリックして **今すぐバックアップ**します。
    
    ![BackUpNow ボタン][BackUpNow]
    
    バックアップ処理中に、進行状況についてのメッセージが表示されます。
    

手動バックアップはいつでもできます。  

<a name="automatedbackups"></a>
## 自動バックアップの構成

1.  **バックアップ** ブレードで、をクリックして **スケジュール: 未構成**します。 

    ![ストレージ アカウントの選択](./media/web-sites-backup/05ScheduleBackup.png)
    
1.  **バックアップのスケジュール設定** ブレードで、 **バックアップ スケジュール** に **に**, 必要に応じて、バックアップのスケジュールを構成し、をクリックして **[ok]**します。
    
    ![自動バックアップを有効にする][SetAutomatedBackupOn]
    
4.  **バックアップの設定を構成する** も開いたままであるブレードをクリックして **記憶域の設定**, を選択して、バックアップ先を選択、 **ストレージ アカウント** と **コンテナー**します。 ストレージ アカウントは、バックアップする Web アプリと同じサブスクリプションに属する必要があります。 必要に応じて、各ブレードで新しいストレージ アカウントまたは新しいコンテナーを作成できます。 完了したら、クリックして **選択**します。
    
    ![ストレージ アカウントの選択](./media/web-sites-backup/02ChooseStorageAccount1.png)
    
5.  **バックアップ設定を構成する** ブレードで、をクリックして **データベース設定**, をクリックし、(SQL データベースまたは MySQL) のバックアップに含めるデータベースを選択 **[ok]**します。  

    ![ストレージ アカウントの選択](./media/web-sites-backup/03ConfigureDatabase.png)

    > [AZURE.NOTE]  この一覧に表示されるデータベースの接続文字列が存在する必要があります、 **接続文字列** のセクション、 **Web アプリ設定** ポータルのブレードでします。

6.  **バックアップ設定を構成する** ブレードで、をクリックして **保存**します。  

<a name="notes"></a>
## メモ

* いる接続文字列を各データベースに対して適切に設定しているかどうかを確認、 **Web アプリ設定** ブレード内 **設定** web アプリのバックアップと復元の機能がデータベースに含めるようにします。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

<a name="partialbackups"></a>
## Web アプリの一部のみをバックアップする

Web アプリのすべてをバックアップしたくない場合があります。 次に例をいくつか示します。

-    [毎週のバックアップをセットアップ](web-sites-backup.md#configure-automated-backups) 古いブログの投稿や画像など、決して変更される静的コンテンツを含む web アプリのです。
-   Web アプリのコンテンツが 10 GB を越えている (つまり、一度にバックアップできる最大量を超えている)。
-   ログ ファイルはバックアップしない。

部分バックアップでは、バックアップするファイルを詳細に選択できます。

### バックアップからファイルを除外する

バックアップからファイルとフォルダーを除外するには、`_backup.filter` ファイルを Web アプリの wwwroot フォルダー内に作成し、除外するファイルとフォルダーをその中に指定します。 このオプションを表示する簡単な方法は、 [Kudu コンソール](https://github.com/projectkudu/kudu/wiki/Kudu-console)します。 

今後まったく変更されることがない過去のログ ファイルや静止画像を含む Web アプリがあるとします。 古い画像を含む Web アプリの完全バックアップは既に存在します。 今後、Web アプリのバックアップを毎日実行しますが、変更されることがないログ ファイルや静止画像ファイルはバックアップしないようにするとします。

![ログのフォルダー][LogsFolder]
![Images フォルダー][ImagesFolder]
    
次の手順は、これらのファイルをバックアップから除外する方法を示しています。

1. `http://{yourapp}.scm.azurewebsites.net/DebugConsole` に移動し、バックアップから除外するフォルダーを識別します。 この例では、UI に表示されている次のファイルとフォルダーを除外します。

        D:\home\site\wwwroot\Logs
        D:\home\LogFiles
        D:\home\site\wwwroot\Images\2013
        D:\home\site\wwwroot\Images\2014
        D:\home\site\wwwroot\Images\brand.png

    [AZURE.NOTE] 最後の行では、個々 のファイルとフォルダーを除外することを示しています。

2. `_backup.filter` という名前のファイルを作成し、上のリストをこのファイルに配置します。ただし `D:\home` は削除します。 1 つのディレクトリまたはファイルを 1 行に配置します。 したがって、ファイルの内容は次のようになります。

    \site\wwwroot\Logs
    \LogFiles
    \site\wwwroot\Images\2013
    \site\wwwroot\Images\2014
    \site\wwwroot\Images\brand.png

3. このファイルをアップロード、 `D:\home\site\wwwroot\` を使用して、サイトのディレクトリ [ftp](web-sites-deploy.md#ftp) または他の方法です。 ファイルを `http://{yourapp}.scm.azurewebsites.net/DebugConsole` 内に直接作成し、そこにコンテンツを挿入することもできます。

4. バックアップする通常の手順と、同じ方法で実行 [手動で](#create-a-manual-backup) または [自動的に](#configure-automated-backups)します。

これで、`_backup.filter` 内に指定されたファイルとフォルダーはバックアップから除外されます。 この例では、ログ ファイル、2013 年と 2014 年の画像ファイル、および brand.png がバックアップされなくなります。

>[AZURE.NOTE] 場合と同様、サイトを部分バックアップを復元する [定期的なバックアップを復元](web-sites-restore.md)します。 復元プロセスは正常に実行されます。
>
>完全バックアップが復元されると、サイト上のすべてのコンテンツは、バックアップの中身に置き換えられます。 サイト上に存在したファイルも、バックアップに存在しなければ削除されます。 ただし、部分バックアップが復元されるとき、ブラック リスト化されたディレクトリまたはファイルのいずれかに配置されているコンテンツは放置されます。

<a name="aboutbackups"></a>

## バックアップの保存方法

Web アプリの 1 つまたは複数のバックアップを作成するバックアップに表示されますが、 **コンテナー** web アプリと同様に、ストレージ アカウントのブレードです。 ストレージ アカウントでは、各バックアップは、バックアップ データを含む .zip ファイルと、.zip ファイルの内容のマニフェストを含む .xml ファイルで構成されます。 バックアップにアクセスする場合は、これらのファイルを解凍して参照でき、Web アプリを実際に復元する必要はありません。

Web アプリのデータベースのバックアップは、.zip ファイルのルートに保存されます。 SQL データベースの場合、これは BACPAC ファイルで (ファイル拡張子はありません)、インポートできます。 BACPAC エクスポートに基づいて新しい SQL データベースを作成するを参照してください。 [新しいユーザー データベースを作成する BACPAC ファイルをインポート](http://technet.microsoft.com/library/hh710052.aspx)します。

> [AZURE.WARNING] 内のファイルを変更する、 **websitebackups** コンテナーが無効になり復元可能なため、バックアップが発生することができます。

<a name="nextsteps"></a>
## 次のステップ
Web アプリをバックアップから復元する方法の詳細については、次を参照してください。 [Azure App Service で web アプリの復元](web-sites-restore.md)します。

Azure で開始するを参照してください。 [Microsoft Azure の無料試用版](/pricing/free-trial/)します。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)

<!-- IMAGES -->
[ChooseBackupsPage]: ./media/web-sites-backup/01ChooseBackupsPage.png
[ChooseStorageAccount]: ./media/web-sites-backup/02ChooseStorageAccount.png
[IncludedDatabases]: ./media/web-sites-backup/03IncludedDatabases.png
[BackUpNow]: ./media/web-sites-backup/04BackUpNow.png
[BackupProgress]: ./media/web-sites-backup/05BackupProgress.png
[SetAutomatedBackupOn]: ./media/web-sites-backup/06SetAutomatedBackupOn.png
[Frequency]: ./media/web-sites-backup/07Frequency.png
[StartDate]: ./media/web-sites-backup/08StartDate.png
[StartTime]: ./media/web-sites-backup/09StartTime.png
[SaveIcon]: ./media/web-sites-backup/10SaveIcon.png
[ImagesFolder]: ./media/web-sites-backup/11Images.png
[LogsFolder]: ./media/web-sites-backup/12Logs.png
[GhostUpgradeWarning]: ./media/web-sites-backup/13GhostUpgradeWarning.png
 
