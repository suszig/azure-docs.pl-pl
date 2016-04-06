<properties 
    pageTitle="Web ジョブでバックグラウンド タスクを実行する" 
    description="Web アプリでバックグラウンド タスクを実行する方法について説明します。" 
    services="app-service" 
    documentationCenter="" 
    authors="tdykstra" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2015" 
    ms.author="tdykstra"/>

# Web ジョブでバックグラウンド タスクを実行する

## 概要

Web ジョブでプログラムまたはスクリプトを実行することができます、 [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) 3 つの方法で web アプリ: 必要に応じて、継続的に、またはスケジュールします。 Web ジョブの使用に追加コストはかかりません。

この記事を使用して web ジョブを展開する方法を示しています、 [Azure ポータル](https://portal.azure.com)します。 Visual Studio または継続的な配信プロセスを使用して展開する方法については、次を参照してください。 [Web アプリへの Azure web ジョブのデプロイ方法](websites-dotnet-deploy-webjobs.md)します。

Azure の Web ジョブ SDK は多くの Web ジョブのプログラミング タスクを簡略化しています。 詳細については、次を参照してください。 [web ジョブ SDK とは](websites-dotnet-webjobs-sdk.md)です。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="acceptablefiles"></a>スクリプトやプログラムで許容可能なファイルの種類

次のファイルの種類を指定できます。

* .cmd、.bat、.exe (windows cmd を使用)
* .ps1 (powershell を使用)
* .sh (bash を使用)
* .php (php を使用)
* .py (python を使用)
* .js (node を使用)
* .jar (java を使用)

## <a name="CreateOnDemand"></a>ポータルでのオンデマンドの Web ジョブの作成

1.  **Web アプリ** のブレード、 [Azure ポータル](http://portal.azure.com), 、] をクリックして **すべての設定 > WebJobs** を表示する、 **web ジョブ** ブレードです。
    
    ![[Web ジョブ] ブレード](./media/web-sites-create-web-jobs/wjblade.png)
    
5. クリックして **追加**します。  **Web ジョブの追加** ダイアログが表示されます。
    
    ![[Web ジョブの追加] ブレード](./media/web-sites-create-web-jobs/addwjblade.png)
    
2.  **名**, 、web ジョブの名前を指定します。 名前は、先頭を文字または数字にする必要があります。"-" と "_" 以外の特殊文字を使用することはできません。
    
4.  **実行方法** ボックスで、選択 **をオンデマンドで実行**します。
    
3.  **ファイル アップロード** ボックスでフォルダー アイコンをクリックし、スクリプトを含む zip ファイルを参照します。 この zip ファイルには、実行可能ファイル (.exe .cmd .bat .sh .php .py .js) のほか、プログラムまたはスクリプトの実行に必要な補助ファイルもすべて含まれている必要があります。
    
5. 確認 **作成** 、スクリプトを web アプリにアップロードします。 
    
    Web ジョブが一覧に表示用に指定した名前、 **WebJobs** ブレードです。
    
6. Web ジョブを実行するには、一覧でその名前を右クリックし、クリックして **実行**します。
    
    ![Web ジョブの実行](./media/web-sites-create-web-jobs/runondemand.png)
    
## <a name="CreateContinuous"></a>連続的に実行する Web ジョブの作成

1. 継続的に実行中のジョブを作成する手順は同じ 1 回実行する web ジョブを作成する場合でも、 **実行方法** ボックスで、選択 **Continuous**します。

2. 開始または継続的な web ジョブを停止、一覧で web ジョブを右クリックし、をクリックして **開始** または **停止**します。
    
> [AZURE.NOTE] Web アプリは、複数のインスタンスで稼働する場合、継続的に実行中の web ジョブは、すべてのインスタンスで実行されます。 オンデマンドで実行する Web ジョブおよびスケジュールに従って実行するタスクは、負荷分散目的で Microsoft Azure によって選択された単一のインスタンス上で実行されます。
    
> 連続的に実行する Web ジョブをすべてのインスタンス上で確実に実行するには、Web アプリで [常時接続]* 構成設定を有効にします。そうしないと、SCM ホスト サイトが長時間アイドル状態になったときに Web ジョブが実行を停止する可能性があります。

## <a name="CreateScheduledCRON"></a>CRON 式を使用してスケジュール済みの WebJob を作成する

この方法は、標準またはプレミアム モードで実行される Web アプリに使用され、必要があります、 **Always On** アプリで有効にする設定。

オンデマンドの WebJob をスケジュール済みの WebJob に変換するには、単に WebJob zip ファイルの root に `settings.job` ファイルを追加します。 この JSON ファイルを含める必要があります、 `schedule` プロパティを [CRON 式](https://en.wikipedia.org/wiki/Cron), 、あたり、次の例です。

CRON 式は 6 つのフィールド: `{second} {minute} {hour} {day} {month} {day of the week}` から成ります。

たとえば、15 分ごとに WebJob をトリガーする場合、`settings.job` は次のようになります。

```json
{
    "schedule": "0 */15 * * * *"
}
``` 

その他の CRON スケジュールの例:

- 1 時間ごと (分の値が 0 のとき): `0 0 * * * *` 
- 午前 9 時から午後 5 時まで 1 時間ごと: `0 0 9-17 * * *` 
- 毎日午前 9 時 30 分: `0 30 9 * * *`
- 平日の毎日午前 9 時 30 分: `0 30 9 * * 1-5`

**注**: Visual Studio から web ジョブを展開するときにマークしてください、 `settings.job` '新しい場合はコピー' としてファイルのプロパティです。


## <a name="CreateScheduled"></a>Azure Scheduler を使用してスケジュール済みの WebJob を作成する

次の代替の方法では、Azure Scheduler を使用します。 この場合、WebJob によってスケジュールは直接認識されません。 代わりに、スケジュールに基づいて WebJob をトリガーするように Azure Scheduler を構成します。 

Azure ポータルでは、スケジュールされた web ジョブを作成する機能がまだありませんが、その機能が追加されるまでを使用して行うことができます、 [旧ポータル](http://manage.windowsazure.com)します。

1.  [旧ポータル](http://manage.windowsazure.com) ] ページ、web ジョブに、クリックして **追加**します。

1.  **実行方法** ボックスで、選択 **、スケジュールに従って実行**します。
    
    ![スケジュールに従って実行する新しいジョブ][NewScheduledJob]
    
2. 選択、 **スケジューラのリージョン** 、ジョブと、次の画面に続行する] ダイアログの右下の矢印をクリックします。

3.  **ジョブの作成** ] ダイアログ ボックスでの種類を選択 **繰り返し** する: **一時的なジョブ** または **定期的なジョブ**します。
    
    ![繰り返しのスケジュール設定][SchdRecurrence]
    
4. 選択も、 **開始** 時間: **今すぐ** または **特定の時点で**します。
    
    ![開始時刻の指定][SchdStart]
    
5. 特定の時刻に開始する場合は、下にある、開始時刻の値を選択して **開始**します。
    
    ![指定時の開始を指定][SchdStartOn]
    
6. 定期的なジョブを選択する場合、 **繰り返しすべて** に見つかった位置の頻度を指定するオプションと **終了日** 終了日時を指定します。
    
    ![繰り返しのスケジュール設定][SchdRecurEvery]
    
7. 選択した場合 **週間**, を選択することができます、 **で特定のスケジュールに従って** ボックスし、ジョブを実行する曜日を指定します。
    
    ![曜日の指定][SchdWeeksOnParticular]
    
8. 選択した場合 **か月間** を選択し、 **で特定のスケジュールに従って** ボックスで、特定の番号付きで実行するジョブを設定することができます **日** 、1 か月です。 
    
    ![1 か月のうち特定の日を指定][SchdMonthsOnPartDays]
    
9. 選択した場合 **曜日**, 、ジョブを実行する月の日または曜日を選択できます。
    
    ![1 か月のうち特定の曜日を指定][SchdMonthsOnPartWeekDays]
    
10. 最後に、使用することも、 **出現** 月の何週目を選択するオプション (最初に、2 週目、3 週目など)、ジョブを指定した曜日に実行します。
    
    ![1 か月のうち特定の週の特定の曜日を指定][SchdMonthsOnPartWeekDaysOccurences]
    
11. ジョブ (複数可) を作成したら、それらの名前が状態、スケジュールの種類、その他の情報と共に [Web ジョブ] タブに表示されます。 30 の web ジョブの履歴情報が保持されます。
    
    ![ジョブの一覧][WebJobsListWithSeveralJobs]
    
### <a name="Scheduler"></a>スケジュールされたジョブと Azure Scheduler

Azure スケジューラ ページで、スケジュールされたジョブをさらに構成できる、 [旧ポータル](http://manage.windowsazure.com)します。

1.  Web ジョブ] ページで、ジョブのクリックして **スケジュール** Azure スケジューラ ポータル ページに移動するリンク。 
    
    ![Azure Scheduler へのリンク][LinkToScheduler]
    
2. スケジューラ ページで、ジョブをクリックします。
    
    ![Scheduler ポータル ページ上のジョブ][SchedulerPortal]
    
3.  **ジョブ アクション** ページが開き、ジョブをさらに構成できます。 
    
    ![Scheduler の [ジョブの操作] ページ][JobActionPageInScheduler]
    
## <a name="ViewJobHistory"></a>ジョブ履歴の表示

1. Web ジョブ SDK で作成したジョブを含め、ジョブの実行履歴を表示するには、下にある対応するリンクをクリックして、 **ログ** web ジョブ] ブレードの列です。 (必要に応じて、クリップボード アイコンを使用して、ログ ファイル ページの URL をクリップボードにコピーできます)。
    
    ![ログのリンク](./media/web-sites-create-web-jobs/wjbladelogslink.png)
        
2. リンクをクリックすると、Web ジョブの詳細ページが開きます。 このページには、実行されたコマンド、そのコマンドを最後に実行した時間、その実行が成功したか失敗したかが表示されます。  **最近実行されたジョブ**, 、さらに詳細を表示する時間をクリックします。
    
    ![Web ジョブ詳細][WebJobDetails]
    
3.  **Web ジョブの実行の詳細** ページが表示されます。 クリックして **出力の切り替え** ログの内容のテキストを表示します。 ログ出力がテキスト形式で表示されます。 
    
    ![Web ジョブ実行の詳細][WebJobRunDetails]
    
4. 別のブラウザー ウィンドウに出力テキストを表示する] をクリックして、 **ダウンロード** リンクします。 テキスト自体をダウンロードするには、リンクを右クリックしてから、ブラウザーのオプションを使用してファイルの内容を保存します。
    
    ![ログ出力のダウンロード][DownloadLogOutput]
    
5.  **WebJobs** 、ページの上部にあるリンクは、履歴ダッシュ ボードに web ジョブの一覧を表示する便利な手段を提供しています。
    
    ![Web ジョブの一覧へのリンク][WebJobsLinkToDashboardList]
    
    ![履歴ダッシュボードに表示された Web ジョブの一覧][WebJobsListInJobsDashboard]
    
    これらのリンクのいずれかをクリックすると、選択したジョブの Web ジョブ詳細ページに移動します。


## <a name="WHPNotes"></a>メモ
    
- scm (デプロイ) サイトへの要求がなく Azure で Web アプリのポータルが開かれていない場合、[無料] モードの Web アプリは 20 分後にタイムアウトすることがあります。 実際のサイトへの要求があっても、この状態はリセットされません。
- 連続するジョブのコードは、無限ループで実行されるように記述する必要があります。
- 連続するジョブが連続的に実行されるのは、アプリが稼働状態になっているときのみです。
- [基本] モードと [標準] モードには [常時接続] 機能が用意されており、この機能を有効にすると、Web アプリがアイドル状態にならなくなります。
- デバッグできるのは、連続的に実行する Web ジョブだけです。 スケジュールされた Web ジョブとオンデマンドの Web ジョブのデバッグはサポートされていません。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## <a name="NextSteps"></a>次のステップ
 
詳細については、次を参照してください。 [Azure WebJobs の推奨リソース][WebJobsRecommendedResources]します。

## 変更内容
* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [プレビュー ポータル内の移動に関するリファレンス。](http://go.microsoft.com/fwlink/?LinkId=529715)

[PSonWebJobs]:http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx
[WebJobsRecommendedResources]:http://go.microsoft.com/fwlink/?LinkId=390226

[OnDemandWebJob]: ./media/web-sites-create-web-jobs/01aOnDemandWebJob.png
[WebJobsList]: ./media/web-sites-create-web-jobs/02aWebJobsList.png
[NewContinuousJob]: ./media/web-sites-create-web-jobs/03aNewContinuousJob.png
[NewScheduledJob]: ./media/web-sites-create-web-jobs/04aNewScheduledJob.png
[SchdRecurrence]: ./media/web-sites-create-web-jobs/05SchdRecurrence.png
[SchdStart]: ./media/web-sites-create-web-jobs/06SchdStart.png
[SchdStartOn]: ./media/web-sites-create-web-jobs/07SchdStartOn.png
[SchdRecurEvery]: ./media/web-sites-create-web-jobs/08SchdRecurEvery.png
[SchdWeeksOnParticular]: ./media/web-sites-create-web-jobs/09SchdWeeksOnParticular.png
[SchdMonthsOnPartDays]: ./media/web-sites-create-web-jobs/10SchdMonthsOnPartDays.png
[SchdMonthsOnPartWeekDays]: ./media/web-sites-create-web-jobs/11SchdMonthsOnPartWeekDays.png
[SchdMonthsOnPartWeekDaysOccurences]: ./media/web-sites-create-web-jobs/12SchdMonthsOnPartWeekDaysOccurences.png
[RunOnce]: ./media/web-sites-create-web-jobs/13RunOnce.png
[WebJobsListWithSeveralJobs]: ./media/web-sites-create-web-jobs/13WebJobsListWithSeveralJobs.png
[WebJobLogs]: ./media/web-sites-create-web-jobs/14WebJobLogs.png
[WebJobDetails]: ./media/web-sites-create-web-jobs/15WebJobDetails.png
[WebJobRunDetails]: ./media/web-sites-create-web-jobs/16WebJobRunDetails.png
[DownloadLogOutput]: ./media/web-sites-create-web-jobs/17DownloadLogOutput.png
[WebJobsLinkToDashboardList]: ./media/web-sites-create-web-jobs/18WebJobsLinkToDashboardList.png
[WebJobsListInJobsDashboard]: ./media/web-sites-create-web-jobs/19WebJobsListInJobsDashboard.png
[LinkToScheduler]: ./media/web-sites-create-web-jobs/31LinkToScheduler.png
[SchedulerPortal]: ./media/web-sites-create-web-jobs/32SchedulerPortal.png
[JobActionPageInScheduler]: ./media/web-sites-create-web-jobs/33JobActionPageInScheduler.png
 


