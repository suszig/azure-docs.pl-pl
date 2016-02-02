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

Azure の Web ジョブ SDK は多くの Web ジョブのプログラミング タスクを簡略化しています。 詳細については、次を参照してください。 [web ジョブ SDK とは](websites-dotnet-webjobs-sdk.md)します。

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

## <a name="CreateOnDemand"></a>ポータルでのオンデマンドの web ジョブを作成します。

1. **Web アプリ** のブレード、 [Azure ポータル](http://portal.azure.com), 、] をクリックして **すべての設定 > WebJobs** を表示する、 **web ジョブ** ブレードです。

    ![[Web ジョブ] ブレード](./media/web-sites-create-web-jobs/wjblade.png)

5. **[追加]** をクリックします。 **[Web ジョブの追加]** ダイアログが表示されます。

    ![[Web ジョブの追加] ブレード](./media/web-sites-create-web-jobs/addwjblade.png)

2. **[名前]** ボックスに、Web ジョブの名前を入力します。 名前は、先頭を文字または数字にする必要があります。"-" と "_" 以外の特殊文字を使用することはできません。

4. **[実行方法]** ボックスの一覧の **[オンデマンドで実行]** をクリックします。

3. **[ファイル アップロード]** ボックスで、フォルダー アイコンをクリックし、スクリプトが含まれている zip ファイルを参照します。 この zip ファイルには、実行可能ファイル (.exe .cmd .bat .sh .php .py .js) のほか、プログラムまたはスクリプトの実行に必要な補助ファイルもすべて含まれている必要があります。

5. **[作成]** をクリックし、Web アプリにスクリプトをアップロードします。

    Web ジョブに指定した名前が、**[Web ジョブ]** ブレードの一覧に表示されます。

6. Web ジョブを実行するには、一覧でその名前を右クリックし、**[実行]** をクリックします。

    ![Web ジョブの実行](./media/web-sites-create-web-jobs/runondemand.png)

## <a name="CreateContinuous"></a>継続的に実行中の web ジョブを作成します。

1. 連続的に実行する Web ジョブを作成するには、一度だけ実行する Web ジョブを作成する場合と同じ手順に従います。ただし、**[実行方法]** ボックスの一覧では **[連続的]** を選択します。

2. 連続的な Web ジョブを開始または停止するには、一覧で Web ジョブを右クリックし、**[開始]** または **[停止]** をクリックします。

> [AZURE.NOTE] Web アプリが複数のインスタンスで実行される場合、連続的に実行する Web ジョブはすべてのインスタンス上で実行されます。 オンデマンドで実行する Web ジョブおよびスケジュールに従って実行するタスクは、負荷分散目的で Microsoft Azure によって選択された単一のインスタンス上で実行されます。

> 連続的に実行する Web ジョブをすべてのインスタンス上で確実に実行するには、Web アプリで [常時接続]* 構成設定を有効にします。そうしないと、SCM ホスト サイトが長時間アイドル状態になったときに Web ジョブが実行を停止する可能性があります。

## <a name="CreateScheduledCRON"></a>CRON 式を使用して、スケジュールされた web ジョブを作成します。

この方法は、Standard または Premium モードで実行されている Web Apps に使用でき、アプリで **Always On** 設定が有効になっている必要があります。

[オンデマンド web ジョブをスケジュールされた web ジョブを有効にするだけで含める、 `含んだ settings.job` 、web ジョブの zip ファイルのルートにあるファイルです。 この JSON ファイルを含める必要があります、 `スケジュール` プロパティを [CRON 式](https://en.wikipedia.org/wiki/Cron), 、あたり、次の例です。

CRON 式は 6 個のフィールドで構成されます: `{秒} {分} {時間} {日} {月} {週の曜日}`します。

たとえば、15 分ごとに、web ジョブをトリガーするため、 `含んだ settings.job` 必要があります。

```json
{
    "schedule": "0 */15 * * * *"
}
```

その他の CRON スケジュールの例:

- (分単位の数は 0) つまりされるたびに毎時間: `0 0 利用`
- 午前 9 時から午後 5 時までのすべての時間: `0 0-9 17 * * *`
- 毎日の午前 9 時 30 分: `0 30 9 * * *`
- 平日の午前 9 時 30 分で: `0 30 9 * * 1 ~ 5`

**注**: Visual Studio から web ジョブを展開するときにマークしてください、 `含んだ settings.job` '新しい場合はコピー' としてファイルのプロパティです。


## <a name="CreateScheduled"></a>Azure スケジューラを使用して、スケジュールされた web ジョブを作成します。

次の代替の方法では、Azure Scheduler を使用します。 この場合、WebJob によってスケジュールは直接認識されません。 代わりに、スケジュールに基づいて WebJob をトリガーするように Azure Scheduler を構成します。

Azure ポータルでは、スケジュールされた web ジョブを作成する機能がまだありませんが、その機能が追加されるまでを使用して行うことができます、 [旧ポータル](http://manage.windowsazure.com)します。

1. [旧ポータル](http://manage.windowsazure.com) ] ページ、web ジョブに、クリックして **追加**します。

1. **[実行方法]** ボックスの一覧の **[スケジュールに従って実行]** をクリックします。

    ![スケジュールに従って実行する新しいジョブ][newscheduledjob]

2. **[Scheduler のリージョン]** ボックスの一覧で、ジョブに応じたリージョンを選択し、ダイアログ ボックスの右下にある右矢印をクリックして、次の画面に進みます。

3. **[ジョブの作成]** ダイアログ ボックスの **[繰り返し]** ボックスで、繰り返しの種類として **[一度だけのジョブ]** または **[定期的なジョブ]** を選択します。

    ![繰り返しのスケジュール設定][schdrecurrence]

4. また、**[開始]** ボックスの一覧で、開始時刻として **[現在]** または **[指定時]** を選択します。

    ![開始時刻の指定][schdstart]

5. 特定の日時に開始するには、**[開始日時]** の各ボックスで、開始時刻の値を選択します。

    ![指定時の開始を指定][schdstarton]

6. 定期的なジョブを選択した場合は、**[繰り返しの間隔]** で実行頻度を指定し、**[終了日]** で終了日時を指定します。

    ![繰り返しのスケジュール設定][schdrecurevery]

7. **[週]** を選択した場合は **[特定のスケジュールに従って]** チェック ボックスをオンにし、ジョブを実行する曜日 (複数も可能) を指定することができます。

    ![曜日の指定][schdweeksonparticular]

8. **[月]** を選択して **[特定のスケジュールに従って]** チェック ボックスをオンにした場合は、**[日]** をクリックして、1 か月のうちジョブを実行する日 (複数も可能) を指定することができます。

    ![1 か月のうち特定の日を指定][schdmonthsonpartdays]

9. **[平日]** を選択した場合は、1 か月のうちジョブを実行する曜日 (複数も可能) を指定することができます。

    ![1 か月のうち特定の曜日を指定][schdmonthsonpartweekdays]

10. 最後に、使用することも、 **出現** 、1 か月の何週目を選択するオプション (最初に、2 週目、3 番目などです)。 指定した曜日に実行するジョブを必要とします。

    ![1 か月のうち特定の週の特定の曜日を指定][schdmonthsonpartweekdaysoccurences]

11. ジョブ (複数可) を作成したら、それらの名前が状態、スケジュールの種類、その他の情報と共に [Web ジョブ] タブに表示されます。 30 の web ジョブの履歴情報が保持されます。

    ![ジョブの一覧][webjobslistwithseveraljobs]

### <a name="Scheduler"></a>スケジュールされたジョブと Azure スケジューラ

Azure スケジューラ ページで、スケジュールされたジョブをさらに構成できる、 [旧ポータル](http://manage.windowsazure.com)します。

1.  [Web ジョブ] ページで、ジョブの **[スケジュール]** リンクをクリックして Azure スケジューラ ポータル ページに移動します。

    ![Azure スケジューラへのリンク][linktoscheduler]

2. Scheduler ページで、ジョブをクリックします。

    ![スケジューラ ポータル ページ上のジョブ][schedulerportal]

3. **[ジョブの操作]** ページが開き、ここでジョブをさらに構成できます。

    ![Scheduler の ][jobactionpageinscheduler]

## <a name="ViewJobHistory"></a>ジョブ履歴を表示します。

1. Web ジョブ SDK で作成したジョブを含め、ジョブの実行履歴を表示するには、下にある対応するリンクをクリックして、 **ログ** web ジョブ] ブレードの列です。 (必要に応じて、クリップボード アイコンを使用して、ログ ファイル ページの URL をクリップボードにコピーできます)。

    ![ログのリンク](./media/web-sites-create-web-jobs/wjbladelogslink.png)

2. リンクをクリックすると、Web ジョブの詳細ページが開きます。 このページには、実行されたコマンド、そのコマンドを最後に実行した時間、その実行が成功したか失敗したかが表示されます。 **[最近実行されたジョブ]** で、さらに詳細を表示する時間をクリックします。

    ![Web ジョブ詳細][webjobdetails]

3. **[Web ジョブ実行の詳細]** ページが表示されます。 ログの内容のテキストを表示するには、**[出力切り替え]** をクリックします。 ログ出力がテキスト形式で表示されます。

    ![Web ジョブ実行の詳細][webjobrundetails]

4. 出力されたテキストを別のブラウザー ウィンドウで確認するには、**[ダウンロード]** リンクをクリックします。 テキスト自体をダウンロードするには、リンクを右クリックしてから、ブラウザーのオプションを使用してファイルの内容を保存します。

    ![ログ出力のダウンロード][downloadlogoutput]

5. ページの上部にある **[Web ジョブ]** リンクを使用すると、履歴ダッシュボードに Web ジョブの一覧を簡単に表示できます。

    ![Web ジョブの一覧へのリンク][webjobslinktodashboardlist]

    ![履歴ダッシュボードに表示された Web ジョブの一覧][webjobslistinjobsdashboard]

    これらのリンクのいずれかをクリックすると、選択したジョブの Web ジョブ詳細ページに移動します。


## <a name="WHPNotes"></a>ノート

- scm (デプロイ) サイトへの要求がなく Azure で Web アプリのポータルが開かれていない場合、[無料] モードの Web アプリは 20 分後にタイムアウトすることがあります。 実際のサイトへの要求があっても、この状態はリセットされません。
- 連続するジョブのコードは、無限ループで実行されるように記述する必要があります。
- 連続するジョブが連続的に実行されるのは、アプリが稼働状態になっているときのみです。
- [基本] モードと [標準] モードには [常時接続] 機能が用意されており、この機能を有効にすると、Web アプリがアイドル状態にならなくなります。
- デバッグできるのは、連続的に実行する Web ジョブだけです。 スケジュールされた Web ジョブとオンデマンドの Web ジョブのデバッグはサポートされていません。

>[AZURE.NOTE] 場合は、Azure アカウントがサインアップする前に Azure App Service の使用を開始するには、 [App Service の試用](http://go.microsoft.com/fwlink/?LinkId=523751), 、App Service で有効期間の短いスターター web アプリをすぐに作成する場所です。 このサービスの利用にあたり、クレジット カードは必要ありません。契約も必要ありません。

## <a name="NextSteps"></a>次のステップ

詳細については、次を参照してください。 [Azure WebJobs 推奨リソース ][webjobsrecommendedresources]します。

## 変更内容

* Web サイトから App Service への変更のガイドを参照してください: [Azure App Service と既存の Azure サービスへの影響](http://go.microsoft.com/fwlink/?LinkId=529714)
* 古いポータルから新しいポータルへの変更ガイドについては、次を参照してください: [プレビュー ポータル内の移動に関するリファレンス](http://go.microsoft.com/fwlink/?LinkId=529715)。


[psonwebjobs]: http://blogs.msdn.com/b/nicktrog/archive/2014/01/22/running-powershell-web-jobs-on-azure-websites.aspx 
[webjobsrecommendedresources]: http://go.microsoft.com/fwlink/?LinkId=390226 
[ondemandwebjob]: ./media/web-sites-create-web-jobs/01aOnDemandWebJob.png 
[webjobslist]: ./media/web-sites-create-web-jobs/02aWebJobsList.png 
[newcontinuousjob]: ./media/web-sites-create-web-jobs/03aNewContinuousJob.png 
[newscheduledjob]: ./media/web-sites-create-web-jobs/04aNewScheduledJob.png 
[schdrecurrence]: ./media/web-sites-create-web-jobs/05SchdRecurrence.png 
[schdstart]: ./media/web-sites-create-web-jobs/06SchdStart.png 
[schdstarton]: ./media/web-sites-create-web-jobs/07SchdStartOn.png 
[schdrecurevery]: ./media/web-sites-create-web-jobs/08SchdRecurEvery.png 
[schdweeksonparticular]: ./media/web-sites-create-web-jobs/09SchdWeeksOnParticular.png 
[schdmonthsonpartdays]: ./media/web-sites-create-web-jobs/10SchdMonthsOnPartDays.png 
[schdmonthsonpartweekdays]: ./media/web-sites-create-web-jobs/11SchdMonthsOnPartWeekDays.png 
[schdmonthsonpartweekdaysoccurences]: ./media/web-sites-create-web-jobs/12SchdMonthsOnPartWeekDaysOccurences.png 
[runonce]: ./media/web-sites-create-web-jobs/13RunOnce.png 
[webjobslistwithseveraljobs]: ./media/web-sites-create-web-jobs/13WebJobsListWithSeveralJobs.png 
[webjoblogs]: ./media/web-sites-create-web-jobs/14WebJobLogs.png 
[webjobdetails]: ./media/web-sites-create-web-jobs/15WebJobDetails.png 
[webjobrundetails]: ./media/web-sites-create-web-jobs/16WebJobRunDetails.png 
[downloadlogoutput]: ./media/web-sites-create-web-jobs/17DownloadLogOutput.png 
[webjobslinktodashboardlist]: ./media/web-sites-create-web-jobs/18WebJobsLinkToDashboardList.png 
[webjobslistinjobsdashboard]: ./media/web-sites-create-web-jobs/19WebJobsListInJobsDashboard.png 
[linktoscheduler]: ./media/web-sites-create-web-jobs/31LinkToScheduler.png 
[schedulerportal]: ./media/web-sites-create-web-jobs/32SchedulerPortal.png 
[jobactionpageinscheduler]: ./media/web-sites-create-web-jobs/33JobActionPageInScheduler.png 

