<properties 
    pageTitle="Azure Automation での仮想マシンの起動と停止 - PowerShell ワークフロー | Microsoft Azure"
    description="クラシック仮想マシンを起動および停止するための Runbook を含む Azure Automation ソリューションのグラフィカル バージョン。"
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

これは、このソリューションの Runbook の PowerShell ワークフロー バージョンです。 使用可能な使用をお勧めします。 [グラフィカル runbook](automation-solutions-startstopvm-graphical.md)します。

## ソリューションの取得

このソリューションは、次のリンクからダウンロード可能な 2 つの PowerShell ワークフロー Runbook で構成されています。  参照してください、 [グラフィカル バージョン](automation-solutions-startstopvm-graphical.md) グラフィカル runbook へのリンクについては、このソリューションのです。

| Runbook | リンク | 型 | 説明 |
|:---|:---|:---|:---|
| Start-AzureVMs | [Azure クラシック VM の起動](https://gallery.technet.microsoft.com/Start-Azure-Classic-VMs-86ef746b) | PowerShell ワークフロー | Azure サブスクリプションのすべてのクラシック仮想マシンまたは特定のサービス名を持つすべての仮想マシンを起動します。 |
| Stop-AzureVMs | [Azure クラシック VM の停止](https://gallery.technet.microsoft.com/Stop-Azure-Classic-VMs-7a4ae43e) | PowerShell ワークフロー | Azure アカウントのすべての仮想マシンまたは特定のサービス名を持つすべての仮想マシンを停止します。  |


## ソリューションのインストール

### 1.Runbook をインストールする

Runbook をダウンロードした後の手順を使用してインポートできます [、Runbook をインポートする](http://msdn.microsoft.com/library/dn643637.aspx#ImportRunbook)です。

### 2.説明と要件を確認する
Runbook には、説明と必要な資産を含むコメント付きヘルプ テキストが含まれています。  また、この記事から同じ情報を取得することもできます。 

### 3.資産を構成する
Runbook には以下の資産が必要です。これらを作成し、適切な値を設定する必要があります。

|アセット タイプ |アセット名 |説明 |
|:---|:---|:---|:---|
|資格情報 |AzureCredential |起動し、Azure サブスクリプションの仮想マシンを停止する権限を持つアカウントの資格情報が含まれています。  または、別の資格情報資産を指定することができます、 **資格情報** のパラメーター、 **Add-azureaccount** アクティビティ。 |
|変数 |AzureSubscriptionId |Azure サブスクリプションのサブスクリプション ID が含まれています。 |

## ソリューションの使用

### パラメーター

Runbook にはそれぞれ以下のパラメーターがあります。  すべての必須パラメーターの値を指定する必要があります。必要に応じて、要件に合わせて他のパラメーターの値を指定することができます。

| パラメーター | 型 | 必須 | 説明 |
|:---|:---|:---|:---|
| ServiceName | string | いいえ | 値が指定されている場合、そのサービス名を持つすべての仮想マシンが起動または停止します。  値が指定されていない場合、Azure サブスクリプションのすべてのクラシック仮想マシンが起動または停止します。 |
| AzureSubscriptionIdAssetName | string | いいえ | 名前を含む、 [変数アセット](#installing-the-solution) 、Azure サブスクリプションのサブスクリプション ID を格納します。  値を指定しない場合 *AzureSubscriptionId* を使用します。  |
| AzureCredentialAssetName | string | いいえ | 名前を含む、 [資格情報資産](#installing-the-solution) を使用する runbook の資格情報を含みます。  値を指定しない場合 *AzureCredential* を使用します。  |

### Runbook の開始

内のメソッドのいずれかを使用する [Azure Automation で runbook を開始](automation-starting-a-runbook.md) にこのソリューションでは、runbook の開始。

次のサンプル コマンドでは、Windows PowerShell を使用して、実行 **StartAzureVMs** サービス名ですべての仮想マシンを開始する *MyVMService*します。

    $params = @{"ServiceName"="MyVMService"}
    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Start-AzureVMs" –Parameters $params

### 出力

Runbook は [、メッセージを出力](automation-runbook-output-and-messages.md) 開始または停止の命令が正常に送信されたかどうかを示す仮想マシンごとにします。  出力の特定の文字列を検索して、各 Runbook の結果を判別することができます。  考えられる出力文字列を次の表にリストします。

| Runbook | 条件 | メッセージ |
|:---|:---|:---|
| Start-AzureVMs | 仮想マシンが既に実行されている  | MyVM is already running |
| Start-AzureVMs | 仮想マシンの起動要求が正常に送信されました | MyVM has been started |
| Start-AzureVMs | 仮想マシンの起動要求に失敗しました  | MyVM failed to start |
| Stop-AzureVMs | 仮想マシンが既に実行されている  | MyVM is already stopped |
| Stop-AzureVMs | 仮想マシンの起動要求が正常に送信されました | MyVM has been started |
| Stop-AzureVMs | 仮想マシンの起動要求に失敗しました  | MyVM failed to start |

Runbook からは、次のコード スニペットがサービス名ですべての仮想マシンを起動しようとしたなど *MyServiceName*します。  起動要求に失敗した場合は、エラー アクションを実行できます。 

    $results = Start-AzureVMs -ServiceName "MyServiceName"
    foreach ($result in $results) {
        if ($result -like "* has been started" ) {
            # Action to take in case of success.
        }
        else {
            # Action to take in case of error.
        }
    }


## 詳細な内訳

このソリューションでの Runbook の詳細な内訳を以下に示します。  この情報を、Runbook をカスタマイズする場合や、学習のみの目的で使用することができます。

### パラメーター

    param (
        [Parameter(Mandatory=$false)] 
        [String]  $AzureCredentialAssetName = 'AzureCredential',
        
        [Parameter(Mandatory=$false)]
        [String] $AzureSubscriptionIdAssetName = 'AzureSubscriptionId',

        [Parameter(Mandatory=$false)] 
        [String] $ServiceName
    )

値を取得することによって、ワークフローが開始される、 [入力パラメーター](#using-the-solution)します。  資産名が指定されていない場合は、既定の名前が使用されます。

### Output

    # Returns strings with status messages
    [OutputType([String])]

この行では、Runbook の出力が文字列になることを宣言します。  これは不要ですが、ベスト プラクティスとして、runbook を使用する場合の [子 runbook](automation-child-runbooks.md) 親 runbook の予想される出力の種類が認識されるようです。

### 認証

    # Connect to Azure and select the subscription to work against
    $Cred = Get-AutomationPSCredential -Name $AzureCredentialAssetName
    $null = Add-AzureAccount -Credential $Cred -ErrorAction Stop
    $SubId = Get-AutomationVariable -Name $AzureSubscriptionIdAssetName
    $null = Select-AzureSubscription -SubscriptionId $SubId -ErrorAction Stop

次の行セット、 [資格情報](automation-configuring.md#configuring-authentication-to-azure-resources) と、runbook の残りの部分に使用される Azure サブスクリプション。
最初を使用して **Get-automationpscredential** を起動し、Azure サブスクリプションの仮想マシンを停止するアクセス権を持つ資格情報を保持する資産を取得します。 **Add-azureaccount** このアセットを使用して、資格情報を設定します。  出力はダミー変数に割り当てられるため、Runbook 出力に含まれません。  

ID は、その後で取得するサブスクリプションを使用して変数資産 **Get-automationvariable** とで設定されたサブスクリプション **Select-azuresubscription**します。

### VM の取得

    # If there is a specific cloud service, then get all VMs in the service,
    # otherwise get all VMs in the subscription.
    if ($ServiceName) 
    { 
        $VMs = Get-AzureVM -ServiceName $ServiceName
    }
    else 
    { 
        $VMs = Get-AzureVM
    }

**Get-azurevm** を使用して、runbook が連携する仮想マシンを取得します。  値が指定される場合、 **ServiceName** サービス同名の仮想マシンのみを取得し、変数を入力します。  場合 **ServiceName** が空の場合、すべての仮想マシンを取得します。

### 仮想マシンの起動/停止および出力の送信

    # Start each of the stopped VMs
    foreach ($VM in $VMs)
    {
        if ($VM.PowerState -eq "Started")
        {
            # The VM is already started, so send notice
            Write-Output ($VM.InstanceName + " is already running")
        }
        else
        {
            # The VM needs to be started
            $StartRtn = Start-AzureVM -Name $VM.Name -ServiceName $VM.ServiceName -ErrorAction Continue

            if ($StartRtn.OperationStatus -ne 'Succeeded')
            {
                # The VM failed to start, so send notice
                Write-Output ($VM.InstanceName + " failed to start")
            }
            else
            {
                # The VM started, so send notice
                Write-Output ($VM.InstanceName + " has been started")
            }
        }
    }

次の行では、各仮想マシンを順に確認します。  最初、 **PowerState** 仮想マシンのかがチェックは既に実行中または停止された、runbook に依存します。  既に対象の状態になっている場合は、メッセージが出力に送信されてから、Runbook が終了します。  以外の場合、 **Start-azurevm** または **Stop-azurevm** を開始または変数に格納されている要求の結果で仮想マシンを停止しようとするために使用します。  その後、起動または停止要求が正常に送信されたかどうかを示すメッセージが出力に送信されます。


## 関連記事:

- [Azure Automation での子 Runbook](automation-child-runbooks.md) 
- [Azure Automation での Runbook の出力および メッセージ](automation-runbook-output-and-messages.md)
