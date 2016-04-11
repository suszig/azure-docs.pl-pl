<properties 
   pageTitle="Azure Automation の Runbook の種類"
   description="Azure Automation で使用できる Runbook の種類、および使用する種類を決定するときの考慮事項について説明します。 "
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
   ms.date="11/13/2015"
   ms.author="bwren" />

# Azure Automation の Runbook の種類

Azure Automation では、次の表に簡単に説明できる runbook の次の 3 つの種類をサポートします。  その後のセクションでは、使用するときの考慮事項など、各種類に関して詳しく説明します。


| 型 |  説明 |
|:---|:---|
| [グラフィカル](#graphical-runbooks) | Windows PowerShell ワークフローに基づいており、Azure ポータルのグラフィカル エディターで完全に作成および編集されます。 | 
| [PowerShell ワークフロー](#powershell-workflow-runbooks) | Windows PowerShell ワークフローに基づくテキスト Runbook です。 |
| [PowerShell](#powershell-runbooks) | Windows PowerShell スクリプトに基づくテキスト Runbook です。 |

## グラフィック Runbook

[グラフィカルな runbook](automation-runbook-types.md#graphical-runbooks) が作成され、Azure ポータルでグラフィカル エディタで編集します。  ファイルにエクスポートして別の Automation アカウントにインポートできますが、別のツールで作成または編集することはできません。  グラフィカル Runbook は PowerShell ワークフローのコードを生成しますが、そのコードを直接表示または変更することはできません。 グラフィカル runbook は、のいずれかに変換できない、 [テキスト形式](automation-runbook-types.md), 、またはテキスト runbook をグラフィカルな形式に変換できます。

### 長所

- 最低限の知識を持つ runbook を作成する [PowerShell ワークフロー](automation-powershell-workflow.md)します。
- 管理プロセスが視覚的に表現されます。
- 使用 [チェックポイント](automation-powershell-workflow.md#checkpoints) エラーが発生した場合の runbook を再開します。
- 使用 [並列処理](automation-powershell-workflow.md#parallel-processing) する複数のアクティビティを並列に実行します。
- 高度なワークフローを作成するために、他のグラフィカル Runbook や PowerShell ワークフロー Runbook を子 Runbook として組み込むことができます。


### 制限事項

- Azure ポータルの外部では Runbook を編集できません。
- 必要な場合があります、 [ワークフロー スクリプト コントロール](automation-powershell-workflow.md#activities) 複雑なロジックを実行する PowerShell ワークフロー コードを含んでいます。
- グラフィカル ワークフローによって作成された PowerShell ワークフローのコードを表示したり、直接編集したりすることはできません。  ワークフロー スクリプトのアクティビティのコードは表示できます。
- グラフィカル Runbook は、実行する前にコンパイルする必要があるため、PowerShell Runbook より開始に時間がかかります。
- PowerShell Runbook を子 Runbook として組み込むには、新しいジョブを作成する Start-AzureAutomationRunbook コマンドレットを使用する必要があります。


## PowerShell ワークフロー Runbook

基づくテキスト runbook が、PowerShell ワークフローの runbook は [Windows PowerShell ワークフロー](automation-powershell-workflow.md)します。  Azure ポータルのテキスト エディターを使用して、Runbook のコードを直接編集します。  任意のオフラインのテキスト エディターを使用することもでき、 [、runbook をインポート](http://msdn.microsoft.com/library/azure/dn643637.aspx) Azure Automation にします。

### 長所

- すべての複雑なロジックを PowerShell ワークフローのコードで実装できます。
- 使用 [チェックポイント](automation-powershell-workflow.md#checkpoints) エラーが発生した場合の runbook を再開します。
- 使用 [並列処理](automation-powershell-workflow.md#parallel-processing) 並列で複数の操作を実行します。
- 高度なワークフローを作成するために、他のグラフィカル Runbook や PowerShell ワークフロー Runbook を子 Runbook として組み込むことができます。


### 制限事項

- 作成者は、PowerShell ワークフローについて熟知する必要があります。
- Runbook がなどの PowerShell ワークフローの他の複雑さを扱う必要があります [オブジェクトを逆シリアル化](automation-powershell-workflow.md#code-changes)します。
- グラフィカル Runbook は、実行する前にコンパイルする必要があるため、PowerShell Runbook より開始に時間がかかります。
- PowerShell Runbook を子 Runbook として組み込むには、新しいジョブを作成する Start-AzureAutomationRunbook コマンドレットを使用する必要があります。


## PowerShell Runbook

PowerShell Runbook は、Windows PowerShell に基づきます。  Azure ポータルのテキスト エディターを使用して、Runbook のコードを直接編集します。  任意のオフラインのテキスト エディターを使用することもでき、 [、runbook をインポート](http://msdn.microsoft.com/library/azure/dn643637.aspx) Azure Automation にします。

### 長所

- すべての複雑なロジックを PowerShell コードで実装でき、PowerShell ワークフローに関する複雑さが加わることはありません。 
- 実行前にコンパイルする必要がないため、グラフィカル Runbook または PowerShell ワークフロー Runbook より開始に時間がかかりません。

### 制限事項

- 作成者は、PowerShell スクリプトに熟知している必要があります。
- 使用できません [並列処理](automation-powershell-workflow.md#parallel-processing) 並列で複数の操作を実行します。
- 使用できません [チェックポイント](automation-powershell-workflow.md#checkpoints) エラーが発生した場合の runbook を再開します。
- PowerShell ワークフロー Runbook とグラフィカル Runbook を子 Runbook として組み込むには、新しいジョブを作成する Start-AzureAutomationRunbook コマンドレットを使用する必要があります。

### 既知の問題
PowerShell Runbook に関する現在の既知の問題は次のとおりです。

- PowerShell runbook を暗号化されていない取得できないことはできません [変数アセット](automation-variables.md) null 値を持つ。
- PowerShell runbook を取得できません、 [変数アセット](automation-variables.md) と *~* 名前にします。
- PowerShell Runbook のループ内の Get-Process が、約 80 回反復した後でクラッシュする可能性があります。 
- PowerShell Runbook は、非常に大量のデータを一度に出力ストリームに書き込もうとした場合、失敗する可能性があります。   通常は、大きいオブジェクトを使用する場合、必要な情報だけを出力することによってこの問題を回避できます。  出力のようなものではなく、たとえば、 *Get-process*, で、必要なフィールドだけを出力する *Get-process |プロセス名、CPU を選択して*します。

## 考慮事項

特定の Runbook に使用する種類を決定するときは、さらに次のことを考慮する必要があります。

- Runbook を別の種類に変換することはできません。
- 異なる種類の Runbook を子 Runbook として使用する場合は制限があります。  参照してください [Azure Automation の runbook を子](automation-child-runbooks.md) の詳細。



  
## 関連記事:

- [Azure Automation でのグラフィカル作成](automation-graphical-authoring-intro.md)
- [Windows PowerShell ワークフローについて](automation-powershell-workflow.md)
- [Runbook の作成またはインポート](http://msdn.microsoft.com/library/azure/dn643637.aspx)



