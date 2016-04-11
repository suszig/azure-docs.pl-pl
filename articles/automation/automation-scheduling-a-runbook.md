<properties 
   pageTitle="Scheduling a runbook in Azure Automation"
   description="特定の時刻または定期的なスケジュールで自動的に Runbook を開始できるように Azure Automation でスケジュールを作成する方法について説明します。"
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/01/2015"
   ms.author="bwren" />

# Azure Automation の Runbook をスケジュール設定する

指定の時刻に開始するように Azure Automation の Runbook をスケジュール設定するには、Runbook を 1 つ以上のスケジュールにリンクします。 スケジュールは 1 回だけ実行するように構成できるほか、指定された間隔 (時間数または日数) で定期的に実行するように構成することもできます。 1 つの Runbook を複数のスケジュールにリンクし、1 つのスケジュールを複数の Runbook にリンクすることができます。

## スケジュールを作成する

Azure 管理ポータルまたは Windows PowerShell で新しいスケジュールを作成することができます。 Azure 管理ポータルを使用して Runbook をスケジュールにリンクするときに新しいスケジュールを作成するというオプションもあります。

### Azure 管理ポータルで新しいスケジュールを作成するには

1. Azure 管理ポータルで、[Automation] を選択し、次に Automation アカウントの名前をクリックします。
1. 選択、 **資産** ] タブをクリックします。
1. At the bottom of the window, click **Add Setting**.
1. Click **Add Schedule**.
1. Type a **Name** and optionally a **Description** for the new schedule.
1. Select whether the schedule will run **One Time**, **Hourly**, or **Daily**.
1. 指定する **開始時刻** や他のオプションを選択したスケジュールの種類に応じて。

### Windows PowerShell で新しいスケジュールを作成するには

使用することができます、 [New-azureautomationschedule](http://msdn.microsoft.com/library/azure/dn690271.aspx) コマンドレットを Azure Automation で新しいスケジュールを作成します。 スケジュールの開始時刻と、スケジュールの実行頻度 (1 回限り、1 時間ごと、または毎日) を指定する必要があります。

次のサンプル コマンドは、2015 年 1 月 20 日から毎日午後 3 時 30 分に実行される新しいスケジュールを作成する方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name $scheduleName –StartTime "1/20/2015 15:30:00" –DayInterval 1

## スケジュールを Runbook にリンクする

1 つの Runbook を複数のスケジュールにリンクし、1 つのスケジュールを複数の Runbook にリンクすることができます。 Runbook にパラメーターがある場合は、その値を指定できます。 必須のパラメーターについては必ず値を指定してください。オプションのパラメーターについては値の指定は任意です。  このスケジュールで Runbook が開始されるたびに、これらの値が使用されます。  同じ Runbook を別のスケジュールに関連付けて、異なるパラメーター値を指定することができます。

### Azure 管理ポータルで Runbook にスケジュールをリンクするには

1. Azure の管理ポータルで [ **オートメーション** に automation アカウントの名前をクリックします。
1. 選択、 **Runbook** ] タブをクリックします。
1. Runbook の名前をクリックして、スケジュールを設定します。
1. クリックして、 **スケジュール** ] タブをクリックします。
2. Runbook が現在スケジュールにリンクされていないかどうかは、するためのオプションを指定する **新しいスケジュールへのリンク** または **既存のスケジュールへのリンク**します。  If the runbook is currently linked to a schedule, click **Link** at the bottom of the window to access these options.
1. Runbook がパラメーターを持っている場合は、その値を入力するように求められます。  

### Windows PowerShell で Runbook にスケジュールをリンクするには

使用することができます、 [Register-azureautomationscheduledrunbook](http://msdn.microsoft.com/library/azure/dn690265.aspx) を runbook にスケジュールをリンクします。 Parameters パラメーターを使用して、Runbook のパラメーターに値を指定できます。 参照してください [Azure Automation で Runbook を開始](automation-starting-a-runbook.md) パラメーターの値を指定する方法の詳細。

次のサンプル コマンドは、パラメーターを使用して Runbook にスケジュールをリンクする方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Test-Runbook"
    $scheduleName = "Sample-DailySchedule"
    $params = @{"FirstName"="Joe";"LastName"="Smith";"RepeatCount"=2;"Show"=$true}
    Register-AzureAutomationScheduledRunbook –AutomationAccountName $automationAccountName –Name $runbookName –ScheduleName $scheduleName –Parameters $params

## スケジュールを無効にする

スケジュールを無効にすると、そのスケジュールにリンクされた Runbook はいずれもそのスケジュールに基づいて実行できなくなります。 スケジュールは手動で無効にできるほか、時間単位のスケジュールと日単位のスケジュールについては、その作成時に有効期限を設定することができます。 有効期限の時刻に達すると、スケジュールが無効になります。

### Azure 管理ポータルでスケジュールを無効にするには

Azure 管理ポータルで、スケジュール用の [スケジュールの詳細] ページからスケジュールを無効にすることができます。

1. Azure 管理ポータルで、[Automation] を選択し、次に Automation アカウントの名前をクリックします。
1. Select the Assets tab.
1. Click the name of a schedule to open its detail page.
2. Change **Enabled** to **No**.

### Windows PowerShell でスケジュールを無効にするには

使用することができます、 [Set-azureautomationschedule](http://msdn.microsoft.com/library/azure/dn690270.aspx) コマンドレットを既存のスケジュールのプロパティを変更します。 To disable the schedule, specify **false** for the **IsEnabled** parameter.

次のサンプル コマンドは、スケジュールを無効にする方法を示しています。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "Sample-DailySchedule"
    Set-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name $scheduleName –IsEnabled $false

## 関連記事:

- [Azure Automation のスケジュール資産](http://msdn.microsoft.com/library/azure/dn940016.aspx)
- [Azure Automation での Runbook を開始する](automation-starting-a-runbook.md) 
