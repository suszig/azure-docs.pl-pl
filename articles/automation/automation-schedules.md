<properties 
   pageTitle="Azure Automation のスケジュール | Microsoft Azure"
   description="Automation のスケジュールは、Azure Automation の Runbook を自動的に開始するためのスケジュール設定に使用します。  この記事では、スケジュールの作成方法について説明します。"
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

Automation のスケジュールは、Runbook を自動的に実行するためのスケジュール設定に使用します。  Runbook を 1 回実行する単一の日付時刻が可能です。  または、Runbook を複数回実行する反復スケジュールが可能です。  スケジュールは、Runbook からは通常はアクセスできません。

>[AZURE.NOTE]  現在、スケジュールでは、Azure Automation DSC 構成をサポートしていません。

## Windows PowerShell コマンドレット

以下の表にあるコマンドレットを使用すると、Windows PowerShell で使用される変数を Azure Automation で作成して管理できます。 一部として出荷される、 [Azure PowerShell モジュール](../powershell-install-configure.md)します。

|コマンドレット|説明|
|:---|:---|
|[Get-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690274.aspx)|スケジュールを取得します。|
|[New-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690271.aspx)|新しいスケジュールを作成します。|
|[Remove-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690279.aspx)|スケジュールを削除します。|
|[Set-AzureAutomationSchedule](http://msdn.microsoft.com/library/dn690270.aspx)|既存のスケジュールのプロパティを設定します。|
|[Get-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn913778.aspx)|スケジュール済みの Runbook を取得します。|
|[Register-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690265.aspx)|Runbook をスケジュールに関連付けます。|
|[Unregister-AzureAutomationScheduledRunbook](http://msdn.microsoft.com/library/dn690273.aspx)|Runbook とスケジュールの関連付けを解除します。|

## 新しいスケジュールを作成する

### Azure ポータルで新しいスケジュールを作成するには


1. オートメーション アカウントからクリックして **資産** ウィンドウの上部にあります。
1. At the bottom of the window, click **Add Setting**.
1. Click **Add Schedule**.
1. ウィザードを完了し、チェック ボックスをクリックして新しい変数を保存します。

### Azure プレビュー ポータルで新しいスケジュールを作成するには

1. クリックして、オートメーション アカウントから、 **資産** を開くには一部、 **資産** ブレードです。
1. クリックして、 **スケジュール** を開くには一部、 **スケジュール** ブレードです。
1. クリックして **スケジュールを追加** ブレードの上部にあります。
1. Complete the form and click **Create** to save the new schedule.

### Windows PowerShell で新しいスケジュールを作成するには

 [New-azureautomationschedule](http://msdn.microsoft.com/library/dn690271.aspx) コマンドレットは、新しいスケジュールを作成し、既存のスケジュールの値を設定します。  次の Windows PowerShell サンプル コマンドは My Daily Schedule という名前の新しいスケジュールを作成します。このスケジュールは、明日の正午に開始して、1 年間毎日実行されます。

    $automationAccountName = "MyAutomationAccount"
    $scheduleName = "My Daily Schedule"
    $startTime = (Get-Date).Date.AddDays(1).AddHours(12)
    $expiryTime = $startTime.AddYears(1)
    
    New-AzureAutomationSchedule –AutomationAccountName $automationAccountName –Name $scheduleName –StartTime $startTime –ExpiryTime $expiryTime –DayInterval 1


## 関連項目
- [Azure Automation の Runbook をスケジュール設定する](automation-scheduling-a-runbook.md)
 

