<properties 
   pageTitle="Azure Automation のスケジュール | Microsoft Azure"
   description="Automation のスケジュールは、Azure Automation の Runbook を自動的に開始するためのスケジュール設定に使用します。この記事では、スケジュールの作成方法について説明します。"
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
   ms.date="10/26/2015"
   ms.author="bwren" />


# Azure Automation のスケジュール

Automation のスケジュールは、Runbook を自動的に実行するためのスケジュール設定に使用します。 1 つの日付と時間を 1 回実行する runbook が考えられます。 または、runbook を複数回起動する定期的なスケジュールも設定できます。 スケジュールは、Runbook からは通常はアクセスできません。
>[AZURE.NOTE]  スケジュールは、現時点では Azure Automation DSC 構成をサポートしていません。

## Windows PowerShell コマンドレット

以下の表にあるコマンドレットを使用すると、Windows PowerShell で使用される変数を Azure Automation で作成して管理できます。 一部として出荷される、 [Azure PowerShell モジュール](../powershell-install-configure.md)します。

| コマンドレット| 説明|
|:---|:---|
| [Get AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)| スケジュールを取得します。|
| [新しい AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)| 新しいスケジュールを作成します。|
| [削除 AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)| スケジュールを削除します。|
| [Set-azureautomationschedule](http://msdn.microsoft.com/library/dn690270.aspx)| 既存のスケジュールのプロパティを設定します。|
| [Get AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)| スケジュール済みの Runbook を取得します。|
| [Register-azureautomationscheduledrunbook](http://msdn.microsoft.com/library/dn690265.aspx)| Runbook をスケジュールに関連付けます。|
| [登録を解除 AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)| Runbook とスケジュールの関連付けを解除します。|

## 新しいスケジュールを作成する

### Azure ポータルで新しいスケジュールを作成するには

1. Automation アカウントから、ウィンドウ上部にある **[資産]** をクリックします。
1. ウィンドウの下部にある **[設定の追加]** をクリックします。
1. **[スケジュールの追加]** をクリックします。
1. ウィザードを完了し、チェック ボックスをクリックして新しい変数を保存します。

### Azure プレビュー ポータルで新しいスケジュールを作成するには

1. Automation アカウントから、**[資産]** 部分をクリックして **[資産]** ブレードを開きます。
1. **[スケジュール]** 部分をクリックして、**[スケジュール]** ブレードを開きます。
1. ブレード上部の **[スケジュールの追加]** をクリックします。
1. フォームを完成させ、**[作成]** をクリックして新しいスケジュールを保存します。

### Windows PowerShell で新しいスケジュールを作成するには

[New-azureautomationschedule](http://msdn.microsoft.com/library/dn690271.aspx) コマンドレットは、新しいスケジュールを作成し、既存のスケジュールの値を設定します。 次の Windows PowerShell サンプル コマンドは My Daily Schedule という名前の新しいスケジュールを作成します。このスケジュールは、明日の正午に開始して、1 年間毎日実行されます。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "My Daily Schedule"
    $startTime = (Get-Date).Date.AddDays(1).AddHours(12)
    $expiryTime = $startTime.AddYears(1)
    
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name $scheduleName –StartTime $startTime –ExpiryTime $expiryTime –DayInterval 1

## 関連項目

- [Azure Automation で runbook をスケジュール設定](automation-scheduling-a-runbook.md)






