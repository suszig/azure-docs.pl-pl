<properties 
   pageTitle="Runbook settings"
   description="Azure Automation の Runbook の構成設定について説明し、Azure 管理ポータルと Windows PowerShell のそれぞれで設定を変更する方法について取り上げます。"
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
   ms.date="10/22/2015"
   ms.author="bwren" />

# Runbook の設定

Azure Automation 内の各 Runbook には、それぞれを識別し、ログ記録の動作を変更するのに役立つ設定が複数あります。 これらの各設定を以下に示し、その後にそれらの変更方法手順を記します。

## 設定

### 名前と説明

Runbook の作成後に、名前を変更することはできません。 説明は省略可能で、最大 512 文字まで可能です。

### タグ

タグを使用すると、個別の単語や語句を割り当て、Runbook を識別するのに役立ちます。 Runbook を送信するときなど、 [Runbook ギャラリー](https://msdn.microsoft.com/library/dn781422.aspx), 、runbook が記載されているカテゴリを識別する特定のタグを指定します。 コンマで区切ると 1 つの Runbook に複数のタグを指定できます。

### ログの記録

既定では、詳細レコードと進行状況レコードはジョブ履歴に書き込まれません。 設定を変更し、特定の Runbook でこうしたレコードをログに記録できます。 これらのレコードの詳細については、次を参照してください。 [Runbook の出力とメッセージ](https://msdn.microsoft.com/library/dn879148.aspx)します。

## Runbook の設定変更

### Azure 管理ポータルで Runbook の設定を変更する

Azure 管理ポータルからでは、runbook の設定を変更することができます、 **構成** 、runbook のページです。

1. Azure の管理ポータルで [ **オートメーション** に automation アカウントの名前をクリックします。
1. 選択、 **Runbook** ] タブをクリックします。
1. Click the name of a runbook.
1. 選択、 **構成** ] タブをクリックします。

### Windows PowerShell で Runbook の設定を変更する

使用することができます、 [Set-azureautomationrunbook](https://msdn.microsoft.com/library/dn690275.aspx) コマンドレットを runbook の設定を変更します。 複数のタグを指定する場合は、配列を指定するか、コンマで区切った値を使用して 1 つの文字列をタグ パラメーターに指定できます。 使用の現在のタグを取得できます、 [Get-azureautomationrunbook](https://msdn.microsoft.com/library/dn690278.aspx)します。

以下のサンプル コマンドは、ある Runbook のプロパティ設定方法を示しています。 このサンプルでは、既存のタグに 3 つのタグを追加し、詳細レコードをログに記録するように指定します。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $tags = (Get-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName).Tags
    $tags += "Tag1,Tag2,Tag3"
    Set-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName –LogVerbose $true –Tags $tags

## 関連記事:
- [Runbook の出力とメッセージ](../automation-runbook-output-and-messages) 
- [Runbook の作成またはインポート](https://msdn.microsoft.com/library/dn643637.aspx) 
