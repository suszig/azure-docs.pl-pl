<properties 
    pageTitle="Azure Automation での Runbook の作成またはインポート"
    description="この記事では、Azure Automation で新しい Runbook を作成する方法、またはファイルから Runbook をインポートする方法について説明します。"
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
    ms.date="09/22/2015"
    ms.author="bwren" />

# Azure Automation での Runbook の作成またはインポート

Runbook を Azure Automation に追加するにはいずれかで [新規に作成する](#creating-a-new-runbook) またはファイルから、または既存の runbook をインポートすることによって、 [Runbook ギャラリー](automation-runbook-gallery.md)します。 この記事では、Runbook を作成またはファイルからインポートする方法について説明します。  すべてのコミュニティの runbook とモジュールにアクセスする方法の詳細を取得できます [Azure Automation の Runbook とモジュールのギャラリー](automation-runbook-gallery.md)します。

## 新しい Runbook の作成

Azure ポータルまたは Windows PowerShell のいずれかを使用して、Azure Automation で新しい Runbook を作成できます。 Runbook を作成すると後で情報を使用してを編集できます [PowerShell ワークフローについて](automation-powershell-workflow.md) と [Azure Automation でのグラフィカル作成](automation-graphical-authoring-intro.md)します。

### Azure ポータルで新しい Azure Automation Runbook を作成するには

操作するだけ [PowerShell ワークフロー runbook](automation-runbook-types.md#powershell-workflow-runbooks) Azure ポータルで。

1. Azure ポータルでクリックすると、 **新規**, 、**App Services**, 、**オートメーション**, 、**Runbook**, 、**簡易作成**します。
2. 必要な情報を入力し、クリックして **作成**します。 Runbook 名は、先頭を英字にする必要があり、英字、数字、アンダースコア、およびダッシュを使用できます。
3. Runbook を今すぐ編集する場合はクリックして **Runbook の編集**します。 それ以外の場合、 **OK**します。
4. 新しい runbook が表示されます、 **Runbook** ] タブをクリックします。


### Azure プレビュー ポータルで新しい Azure Automation Runbook を作成するには

1. Azure プレビュー ポータルで、Automation アカウントを開きます。 
2. をクリックして、 **Runbook** タイルを開くには runbook の一覧です。
3. をクリックして、 **runbook を追加** ] ボタンをクリックし、 **新しい runbook を作成**します。
2. 型 a **名** の runbook を選択、 [型](automation-runbook-types.md)します。 Runbook 名は、先頭を英字にする必要があり、英字、数字、アンダースコア、およびダッシュを使用できます。
3. クリックして **作成** を runbook を作成し、エディターを開きます。


### Windows PowerShell で新しい Azure Automation Runbook を作成するには

使用することができます、 [New-azureautomationrunbook](https://msdn.microsoft.com/library/dn690272.aspx) コマンドレットでは、空の作成を [PowerShell ワークフロー runbook](automation-runbook-types.md#powershell-workflow-runbooks)します。 いずれかを指定できます、 **名前** 後で編集できますかを指定できますが空の runbook を作成するパラメーター、 **パス** スクリプト ファイルをインポートするパラメーターです。 

次に示すサンプル コマンドでは、新しい空の Runbook を作成しています。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    New-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName 

## ファイルから Azure Automation への Runbook のインポート

PowerShell スクリプトまたは PowerShell ワークフロー (拡張子 .ps1) をインポートして、またはエクスポートされたグラフィカル Runbook (.graphrunbook) をインポートして、Azure Automation で新しい Runbook を作成できます。  指定する必要があります、 [runbook の種類](automation-runbook-types.md) は、次の事項を考慮に入れてインポートから作成されます。 

- .Graphrunbook ファイルを新しいにインポートできるはのみ [グラフィカルな runbook](automation-runbook-types.md#graphical-runbooks), 、およびグラフィカル runbook は .graphrunbook ファイルからのみ作成できます。
- だけに、PowerShell ワークフローを含む .ps1 ファイルをインポートできます、 [PowerShell ワークフロー runbook](automation-runbook-types.md#powershell-workflow-runbooks)します。  ファイルに複数の PowerShell ワークフローが含まれている場合、インポートは失敗します。 各ワークフローを専用のファイルに保存し、それぞれを個別にインポートする必要があります。
- いずれかにワークフローが含まれていない .ps1 ファイルをインポートすることができます、 [PowerShell runbook](automation-runbook-types.md#powershell-runbooks) または [PowerShell ワークフロー runbook](automation-runbook-types.md#powershell-workflow-runbooks)します。  PowerShell ワークフロー Runbook にインポートされた場合は、ワークフローに変換されて、行われた変更を示すコメントが Runbook に追加されます。

### Azure ポータルでファイルから Runbook をインポートするには
次の手順を使用して、スクリプト ファイルを Azure Automation にインポートできます。  このポータルを使用して PowerShell ワークフロー Runbook にインポートできるのは .ps1 ファイルだけであることに注意してください。  他の種類の場合は、Azure プレビュー ポータルを使用する必要があります。

1. Azure 管理ポータルで [ **オートメーション** オートメーション アカウントを選択します。
2. クリックして **インポート**します。
3. クリックして **ファイルの参照** をインポートするスクリプト ファイルを探します。
4. Runbook を今すぐ編集する場合はクリックして **Runbook の編集**します。 それ以外の場合は、[OK] をクリックします。
5. 新しい runbook が表示されます、 **Runbook** 、オートメーション アカウント] タブをクリックします。
6. 必要があります [runbook を発行する](#publishing-a-runbook) 前に、これを行うことができます。


### Azure プレビュー ポータルでファイルから Runbook をインポートするには
次の手順を使用して、スクリプト ファイルを Azure Automation にインポートできます。  このポータルを使用して PowerShell ワークフロー Runbook にインポートできるのは .ps1 ファイルだけであることに注意してください。

1. Azure プレビュー ポータルで、Automation アカウントを開きます。 
2. をクリックして、 **Runbook** タイルを開くには runbook の一覧です。
3. をクリックして、 **runbook を追加** ] ボタンをクリックし、 **インポート**します。
4. クリックして **Runbook ファイル** をインポートするファイルを選択するには
2. 場合、 **名前** フィールドが有効にし、それを変更するオプションがあります。  Runbook 名は、先頭を英字にする必要があり、英字、数字、アンダースコア、およびダッシュを使用できます。
3. 選択、 [runbook の種類](automation-runbook-types.md) 上記の制限を考慮します。
3. 新しい Runbook が、Automation アカウントの Runbook の一覧に表示されます。
4. 必要があります [runbook を発行する](#publishing-a-runbook) 前に、これを行うことができます。

### Windows PowerShell でスクリプト ファイルから Runbook をインポートするには

使用することができます、 [AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690267.aspx) コマンドレットを既存の runbook のドラフト バージョンにスクリプト ファイルをインポートします。 スクリプト ファイルには、Windows PowerShell ワークフローが 1 つだけ含まれている必要があります。 Runbook にドラフト バージョンが既にある場合、Overwrite パラメーターを使用しないと、インポートは失敗します。 Runbook をインポートした後を使用してを公開 [Publish-azureautomationrunbook](https://msdn.microsoft.com/library/dn690266.aspx)します。

次のサンプル コマンドでは、既存の Runbook にスクリプト ファイルをインポートして発行する方法を示します。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    $scriptPath = "c:\runbooks\Sample-TestRunbook.ps1"

    Set-AzureAutomationRunbookDefinition –AutomationAccountName $automationAccountName –Name $runbookName –Path $ scriptPath -Overwrite
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName


## Runbook の発行

新しい Runbook を作成またはインポートしたときは、実行する前に発行する必要があります。  Azure Automation の各 Runbook には、ドラフト バージョンと発行バージョンがあります。 実行できるのは発行バージョンのみで、編集できるのはドラフト バージョンのみです。 ドラフト バージョンを変更しても発行バージョンに影響はありません。 ドラフト バージョンを使用できるようにする必要がある場合は、それを発行します。これにより、発行バージョンがドラフト バージョンで上書きされます。

## Azure ポータルを使用して Runbook を発行するには

1. Azure ポータルで Runbook を開きます。
1. クリックして、画面の上部にある **作成者**します。
1. 画面の下部にあるをクリックして **発行** し **はい** 、確認メッセージに対してです。

## Azure プレビュー ポータルを使用して Runbook を発行するには

1. Azure プレビュー ポータルで Runbook を開きます。
1. クリックして、 **編集** ] ボタンをクリックします。
1. をクリックして、 **発行** ] ボタンをクリックし、 **はい** 確認メッセージにします。


## Windows PowerShell を使用して Runbook を発行するには

使用することができます、 [Publish-azureautomationrunbook](https://msdn.microsoft.com/library/dn690266.aspx) コマンドレットを Windows PowerShell で runbook を発行します。 次のサンプル コマンドでは、サンプルの Runbook を発行する方法を示します。

    $automationAccountName = "MyAutomationAccount"
    $runbookName = "Sample-TestRunbook"
    
    Publish-AzureAutomationRunbook –AutomationAccountName $automationAccountName –Name $runbookName



## 関連記事:

- [Azure Automation 用の Runbook ギャラリーとモジュール ギャラリー](automation-runbook-gallery.md)
- [Azure Automation でのテキスト形式の Runbook の編集](automation-edit-textual-runbook.md)
- [Azure Automation でのグラフィカル作成](automation-graphical-authoring-intro.md)
