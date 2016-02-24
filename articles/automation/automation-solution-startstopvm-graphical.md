<properties 
    pageTitle="仮想マシンの起動と停止 - グラフ | Microsoft Azure"
    description="クラシック仮想マシンを起動および停止するための Runbook を含む Azure Automation ソリューションの PowerShell ワークフロー バージョン。"
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
    ms.date="09/25/2015"
    ms.author="bwren" />

# Azure Automation ソリューション - 仮想マシンの起動と停止

この Azure Automation ソリューションには、クラシック仮想マシンを起動および停止するための Runbook が含まれています。  このソリューションを次のいずれかの場合に使用できます。  

- 独自の環境で Runbook を変更せずに使用する場合。 
- カスタマイズされた機能を実行するために Runbook を変更する場合。  
- ソリューション全体の一部として別の Runbook から Runbook を呼び出す場合。 
- Runbook の作成概念を学習するためのチュートリアルとして Runbook を使用する場合。 

> [AZURE.SELECTOR]
- [グラフィカル](automation-solution-startstopvm-graphical.md)
- [PowerShell ワークフロー](automation-solution-startstopvm-psworkflow.md)

これは、このソリューションの Runbook のグラフィカル バージョンです。 使用可能な使用をお勧めします。 [PowerShell ワークフロー runbook](automation-solutions-startstopvm-psworkflow.md)します。

## ソリューションの取得

このソリューションは、次のリンクからダウンロード可能な 2 つのグラフィカル Runbook で構成されています。  参照してください、 [PowerShell ワークフローのバージョンは](automation-solutions-startstopvm-psworkflow.md) PowerShell ワークフロー runbook へのリンクについては、このソリューションのです。


| Runbook | リンク | 型 | 説明 |
|:---|:---|:---|:---|
| StartAzureClassicVM | [Azure クラシック VM のグラフィカル Runbook の開始](https://gallery.technet.microsoft.com/scriptcenter/Start-Azure-Classic-VM-c6067b3d) | グラフィカル | Azure サブスクリプションのすべてのクラシック仮想マシンまたは特定のサービス名を持つすべての仮想マシンを起動します。 |
| StopAzureClassicVM | [Azure クラシック VM のグラフィカル Runbook の停止](https://gallery.technet.microsoft.com/scriptcenter/Stop-Azure-Classic-VM-397819bd) | グラフィカル | Azure アカウントのすべての仮想マシンまたは特定のサービス名を持つすべての仮想マシンを停止します。  |


## ソリューションのインストール

### 1.Runbook をインストールする

Runbook をダウンロードした後の手順を使用してインポートできます [グラフィカルな runbook プロシージャ](automation-graphical-authoring-intro.md#graphical-runbook-procedures)します。

### 2.説明と要件を確認する
Runbook と呼ばれるアクティビティを用意する **お読みください** 説明し、必要なアセットが含まれます。  選択してこの情報を表示する、 **お読みください** 活動し、 **ワークフロー スクリプト** パラメーター。  また、この記事から同じ情報を取得することもできます。 

### 3.資産を構成する
Runbook には以下の資産が必要です。これらを作成し、適切な値を設定する必要があります。  名前は既定値です。  それらの名前を指定する場合、異なる名前を持つアセットを使用することができます、 [入力パラメーター](#using-the-solution) 、runbook を開始するとします。

|アセット タイプ |既定名 |説明 |
|:---|:---|:---|:---|
| [資格情報](automation-credentials.md) |AzureCredential |起動し、Azure サブスクリプションの仮想マシンを停止する権限を持つアカウントの資格情報が含まれています。  |
| [変数](automation-variables.md) |AzureSubscriptionId |Azure サブスクリプションのサブスクリプション ID が含まれています。 |

## ソリューションの使用

### パラメーター

各 runbook は、次をいる [入力パラメーター](automation-starting-a-runbook#runbook-parameters)します。  すべての必須パラメーターの値を指定する必要があります。必要に応じて、要件に合わせて他のパラメーターの値を指定することができます。

| パラメーター | 型 | 必須 | 説明 |
|:---|:---|:---|:---|
| ServiceName | string | いいえ | 値が指定されている場合、そのサービス名を持つすべての仮想マシンが起動または停止します。  値が指定されていない場合、Azure サブスクリプションのすべてのクラシック仮想マシンが起動または停止します。 |
| AzureSubscriptionIdAssetName | string | いいえ | 名前を含む、 [変数アセット](#installing-the-solution) 、Azure サブスクリプションのサブスクリプション ID を格納します。  値を指定しない場合 *AzureSubscriptionId* を使用します。  |
| AzureCredentialAssetName | string | いいえ | 名前を含む、 [資格情報資産](#installing-the-solution) を使用する runbook の資格情報を含みます。  値を指定しない場合 *AzureCredential* を使用します。  |

### Runbook の開始

内のメソッドのいずれかを使用する [Azure Automation で runbook を開始](automation-starting-a-runbook.md) にこのソリューションでは、runbook の開始。

次のサンプル コマンドでは、Windows PowerShell を使用して、実行 **StartAzureClassicVM** サービス名ですべての仮想マシンを開始する *MyVMService*します。

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "StartAzureClassicVM" –Parameters $params

### 出力

Runbook は [、メッセージを出力](automation-runbook-output-and-messages.md) 開始または停止の命令が正常に送信されたかどうかを示す仮想マシンごとにします。  出力の特定の文字列を検索して、各 Runbook の結果を判別することができます。  考えられる出力文字列を次の表にリストします。

| Runbook | 条件 | メッセージ |
|:---|:---|:---|
| StartAzureClassicVM | 仮想マシンが既に実行されている  | MyVM is already running |
| StartAzureClassicVM | 仮想マシンの起動要求が正常に送信されました | MyVM has been started |
| StartAzureClassicVM | 仮想マシンの起動要求に失敗しました  | MyVM failed to start |
| StopAzureClassicVM | 仮想マシンが既に実行されている  | MyVM is already stopped |
| StopAzureClassicVM | 仮想マシンの起動要求が正常に送信されました | MyVM has been started |
| StopAzureClassicVM | 仮想マシンの起動要求に失敗しました  | MyVM failed to start |


次に、イメージを使用する、 **StartAzureClassicVM** として、 [子 runbook](automation-child-runbooks.md) サンプルのグラフィカルな runbook でします。  この場合、以下の表の条件付きリンクが使用されます。

| リンク | 条件 |
|:---|:---|
| 正常なリンク | $ActivityOutput ['StartAzureClassicVM'] のように"\ * が開始されました"    |
| エラー リンク   | $ActivityOutput ['StartAzureClassicVM'] の notlike"\ * が開始されました" |

![子 Runbook の例](media/automation-solution-startstopvm/graphical-childrunbook-example.png)


## 詳細な内訳

このソリューションでの Runbook の詳細な内訳を以下に示します。  Runbook をカスタマイズする場合や、独自のソリューションの作成方法の学習のみの目的でこの情報を使用することができます。
 

### 認証

![認証](media/automation-solution-startstopvm/graphical-authentication.png)

アクティビティを設定すると、runbook を開始、 [資格情報](automation-configuring.md#configuring-authentication-to-azure-resources) と、runbook の残りの部分に使用される Azure サブスクリプション。

最初の 2 つのアクティビティでは、 **サブスクリプション Id の取得** と **Azure 資格情報の取得**, 、取得、 [資産](#installing-the-solution) 次の 2 つのアクティビティによって使用されています。  これらのアクティビティでは直接資産を指定できますが、資産名が必要になります。  ユーザーがそれらの名前を指定できるようにしてから、 [入力パラメーター](#using-the-solution), 、これらのアクティビティの入力パラメーターで指定された名前のアセットを取得する必要があります。

**Add-azureaccount** 、runbook の残りの部分に使用される資格情報を設定します。  取得した資格情報資産 **Azure 資格情報の取得** を起動し、Azure サブスクリプションの仮想マシンを停止するアクセス権が必要です。  使用するサブスクリプションが選択されている **Select-azuresubscription** サブスクリプション Id を使用してから **サブスクリプション Id の取得**します。

### 仮想マシンの取得

![VM の取得](media/automation-solution-startstopvm/graphical-getvms.png)

Runbook は、使用する仮想マシンと、(Runbook に応じて) それらが既に起動しているか停止しているかを判別する必要があります。   2 つのアクティビティのいずれかで VM が取得されます。  **サービス内の Vm の取得** 場合は実行、 *ServiceName* 、runbook の入力パラメーターに値が含まれています。  **すべての Vm の取得** 場合は実行、 *ServiceName* 、runbook の入力パラメーターに値が含まれていません。  このロジックは、各アクティビティの前の条件付きリンクで実行されます。

両方のアクティビティを使用して、 **Get-azurevm** コマンドレットです。  **すべての Vm の取得** を使用して、 **ListAllVMs** パラメーターを設定し、すべての仮想マシン。  **サービス内の Vm の取得** を使用して、 **GetVMByServiceAndVMName** 設定パラメーターは、提供、 **ServiceName** の入力パラメーター、 **ServiceName** パラメーター。  

### VM のマージ

![VM のマージ](media/automation-solution-startstopvm/graphical-mergevms.png)

 **マージ Vm** アクティビティがへの入力のために必要な **Start-azurevm** を開始する vm のサービス名と名前が必要があります。  入力は、いずれかのものがあります **すべての Vm の取得** または **サービス内の Vm の取得**, が **Start-azurevm** 、入力の 1 つのアクティビティだけを指定できます。   

ソリューションは、作成する **マージ Vm** 実行に使用する、 **Write-output** コマンドレットです。   **InputObject** そのコマンドレットのパラメーターは、前の 2 つのアクティビティの入力を結合する PowerShell 式です。  これらのアクティビティの 1 つのみが実行されるため、予想される出力のセットは 1 つのみです。  **Start-azurevm** 、入力パラメーターにその出力を使用できます。 

### 仮想マシンの起動/停止

![VM の起動](media/automation-solution-startstopvm/graphical-startvm.png) ![VM の停止](media/automation-solution-startstopvm/graphical-stopvm.png)

次のアクティビティが開始または停止を使用して runbook を試みる、runbook に依存 **Start-azurevm** または **Stop-azurevm**します。  返されたオブジェクトごとに 1 回実行されますので、パイプライン リンクは、アクティビティの後、 **マージ Vm**します。  場合にのみ、アクティビティが実行されるように、リンクが条件付き、 *RunningState* は仮想マシンの *Stopped* の **Start-azurevm** と *開始* の **Stop-azurevm**します。 この条件が満たされない場合、[ **への通知が既に開始されて** または **通知は既に停止して** メッセージを送信するまで実行を使用して **Write-output**します。

### 出力の送信

![VM の起動通知](media/automation-solution-startstopvm/graphical-notifystart.png) ![VM の停止通知](media/automation-solution-startstopvm/graphical-notifystop.png)

Runbook の最後の手順は、各仮想マシンの起動または停止要求が正常に送信されたかどうかを示す出力の送信です。 独立 **Write-output** アクティビティごとに、条件付きリンクを使用して実行することを決定します。  **VM の開始を通知** または **VM の停止の通知** 場合に実行 *OperationStatus* は *Succeeded*します。  場合 *OperationStatus* し、その他の値は、 **通知に失敗しましたに起動** または **停止を通知できませんでした** を実行します。


## 関連記事:

- [Azure Automation でのグラフィカル作成](automation-graphical-authoring-intro.md)
- [Azure Automation での子 Runbook](automation-child-runbooks.md) 
- [Azure Automation での Runbook の出力および メッセージ](automation-runbook-output-and-messages.md)
