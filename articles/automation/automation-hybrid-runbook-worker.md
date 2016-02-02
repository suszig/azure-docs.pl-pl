<properties
   pageTitle="Azure Automation Hybrid Runbook Workers"
   description="この記事では、ローカル データ センター内のコンピューターで Runbook を実行できるようにする Azure Automation の機能である Hybrid Runbook Worker のインストールと使用について説明します。"
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
   ms.date="11/03/2015"
   ms.author="bwren" />


# Azure Automation の Hybrid Runbook Worker

Azure Automation の Runbook は、Azure クラウドで実行されるため、ローカル データ センター内のリソースにはアクセスできません。 Azure Automation の Hybrid Runbook Worker 機能を使用すれば、ローカル リソースを管理するために、データ センターに配置されたマシンで Runbook を実行することができます。 Runbook は Azure Automation で格納および管理されてから、実行される 1 つ以上のオンプレミス マシンに配信されます。

次の図にこの機能を示します。

![Hybrid Runbook Worker の概要](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-overview.png)

Hybrid Runbook Worker として機能し、Azure Automation の Runbook を実行するように、データ センターの 1 つ以上のコンピューターを指定できます。 各 worker には、Microsoft Operations Management Suite および Azure Automation Runbook 環境に接続できる Microsoft 管理エージェントが必要です。 Operations Management Suite は、管理エージェントをインストールして管理し、worker の機能を監視するためにのみ使用されます。 Runbook の配信と Runbook の実行指示は、Azure Automation によって行われます。

![Hybrid Runbook Worker のコンポーネント](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-components.png)
>[AZURE.NOTE] Operational Insights は Operations Management Suite への統合作業中であり、ポータルおよびドキュメントではどちらかの名前が使用されている場合があります。

Hybrid Runbook Worker をサポートするための受信ファイアウォールの要件はありません。 ローカル コンピューター上のエージェントは、クラウドでの Azure Automation とのすべての通信を開始します。 Runbook が開始されると、Azure Automation はエージェントによって取得される指示を作成します。 次に、エージェントは、それを実行する前に、Runbook と任意のパラメーターをプル ダウンします。 取得して [資産](http://msdn.microsoft.com/library/dn939988.aspx) Azure Automation の runbook で使用されます。
>[AZURE.NOTE] Hybrid Runbook Worker は現在サポートしていない [DSC 構成](automation-dsc-overview.md)します。

## Hybrid Runbook Worker のグループ

各 Hybrid Runbook Worker は、エージェントのインストール時に指定する Hybrid Runbook Worker グループのメンバーです。 グループには単一のエージェントを含めることができますが、高可用性グループに複数のエージェントをインストールすることができます。

Hybrid Runbook Worker で Runbook を開始する場合は、実行されるグループを指定します。 要求を処理するワーカーは、グループのメンバーが決定します。 特定のワーカーを指定することはできません。

## Hybrid Runbook Worker の要件

Hybrid Runbook のジョブを実行するには、少なくとも 1 台のオンプレミス コンピューターを指定する必要があります。 このコンピューターには次のものが必要です。

- Windows Server 2012 またはそれ以降
- Windows PowerShell 4.0 またはそれ以降

Hybrid Worker では次の推奨事項を考慮してください。

- 高可用性のため、グループごとに複数の Hybrid Worker を指定します。
- Hybrid Worker は、Service Management Automation または System Center Orchestrator の Runbook サーバーと共存できます。
- ジョブが完了すると Azure Automation にジョブのデータが返送されるため、Automation アカウントのリージョン内またはその近くに物理的に存在するコンピューターを使用することを検討します。

ファイアウォールの要件:

- Hybrid Runbook Worker を実行するオンプレミスのコンピューターは、ポート 443、9354、および 30000-30199 上の *.cloudapp.net への発信アクセス権を有する必要があります。

## Hybrid Runbook Worker のインストール

Hybrid Runbook Worker をインストールして構成する手順は次のとおりです。 最初の 2 つのステップは Automation 環境に対して 1 回だけ実行し、残りのステップは worker コンピューターごとに繰り返します。

### 1.Operations Management Suite のワークスペースを作成する

Operations Management Suite ワークスペースがあるない場合を使用して作成」の手順に  [オペレーション インサイト ワークスペースを設定する](../operational-insights/operational-insights-onboard-in-minutes.md)します。 既存のワークスペースがある場合は、それを使用できます。

### 2.Operations Management Suite ワークスペースに Automation ソリューションを追加します。

ソリューションにより、Operations Management Suite に機能が追加されます。 Automation ソリューションは、Hybrid Runbook Worker のサポートなど、Azure Automation 用の機能を追加します。 ソリューションをワークスペースに追加すると、次の手順でインストールする worker コンポーネントがエージェント コンピューターに自動的にプッシュダウンされます。

」の手順に従って [ソリューション ギャラリーを使用してソリューションを追加する](../operational-insights/operational-insights-setup-workspace.md#1-add-solutions) を追加する、 **オートメーション** Operations Management Suite ワークスペースにソリューションです。

### 3.Microsoft 管理エージェントをインストールする

Microsoft 管理エージェントは Operations Management Suite にコンピューターを接続します。 エージェントをオンプレミスのコンピューターにインストールし、ワークスペースに接続すると、Hybrid Runbook Worker に必要なコンポーネントが自動的にダウンロードされます。

」の手順に従って [コンピューター オペレーション インサイトに直接接続](../operational-insights/operational-insights-direct-agent.md) 内部設置型コンピューターにエージェントをインストールします。 コンピューターごとにこのプロセスを繰り返して、複数の worker を環境に追加できます。

エージェントが Operations Management Suite に正常に接続すると、Operations Management Suite の **[設定]** ウィンドウの **[接続されているソース]** タブにエージェントが表示されます。 C:\Program Files\Microsoft Monitoring Agent\Agent に **AzureAutomationFiles** という名前のフォルダーが作成されていることを調べて、エージェントが Automation ソリューションを正常にダウンロードしたことを確認できます。

### 4.Runbook 環境をインストールして、Azure Automation に接続する

エージェントを Operations Management Suite に追加すると、Automation ソリューションは、**Add-HybridRunbookWorker** コマンドレットを含む **HybridRegistration** PowerShell モジュールをプッシュダウンします。 コンピューターに Runbook 環境をインストールして、Azure Automation に登録する場合は、このコマンドレットを使用します。

管理者モードで PowerShell セッションを開き、次のコマンドを実行してモジュールをインポートします。

    cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
    Import-Module HybridRegistration.psd1

次に、以下の構文を使用して、**Add-HybridRunbookWorker** コマンドレットを実行します。

    Add-HybridRunbookWorker –Name <String> -EndPoint <Url> -Token <String>

このコマンドレットからの必要な情報を取得できます、  **キーの管理** Azure プレビュー ポータルのブレードでします。 Automation アカウントの [要素] パネルにあるキー アイコンをクリックして、このブレードを開くことができます。

![Hybrid Runbook Worker の概要](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

- **Name** は、Hybrid Runbook Worker グループの名前です。 オートメーション アカウントにこのグループが既に存在する場合は、現在のコンピューターがそれに追加されます。 まだ存在しない場合は、追加されます。
- **EndPoint** は、**[キーの管理]** ブレードの **[URL]** フィールドです。
- **Token** は、**[キーの管理]** ブレードの **[プライマリ アクセス キー]** です。

インストールに関する詳細な情報を受け取るには、**Add-HybridRunbookWorker** で **-Verbose** スイッチを使用します。

### 5.PowerShell モジュールをインストールする

Runbook は、Azure Automation 環境にインストールされているモジュールで定義されているアクティビティとコマンドレットをすべて使用できます。 これらのモジュールはオンプレミス マシンに自動的に配置されないため、手動でインストールする必要があります。 ただし、Azure モジュールは既定でインストールされ、すべての Azure サービスのコマンドレットと Azure Automation のアクティビティにアクセスできます。

Hybrid Runbook Worker 機能の主な目的はローカル リソースを管理することであるため、ほとんどの場合、これらのリソースをサポートするモジュールをインストールする必要があります。 参照することができます  [モジュールのインストール](http://msdn.microsoft.com/library/dd878350.aspx) Windows PowerShell モジュールをインストールする方法についてです。

## Hybrid Runbook Worker の削除

オンプレミスのコンピューターで **Remove-HybridRunbookWorker** コマンドレットを実行することにより、そのコンピューターから Hybrid Runbook Worker を削除できます。 削除処理の詳細なログを取得するには、**-Verbose** スイッチを使用します。

## Hybrid Runbook Worker での Runbook の開始

[Azure Automation で Runbook を開始](automation-starting-a-runbook.md) runbook を開始するためのさまざまな方法について説明します。 Hybrid Runbook Worker は、Hybrid Runbook Worker グループの名前を指定できる **RunOn** オプションを追加します。 グループが指定されている場合は、Runbook が取得され、そのグループ内のワーカーによって実行されます。 このオプションが指定されていない場合は、Azure Automation で通常どおり実行されます。

Azure プレビュー ポータルで Runbook を開始する際に、**Azure** または **Hybrid Worker** を選択できる **Run on** オプションが表示されます。 **Hybrid Worker** を選択した場合は、ドロップダウン リストからグループを選択できます。

使用して、 **RunOn** パラメータ、Hybrid Runbook Worker グループで Test-runbook という名前の runbook を開始する、次のコマンドを使用する可能性がありますし、Windows PowerShell を使用して MyHybridGroup の名前します。

    Start-AzureAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-Runbook" -RunOn "MyHybridGroup"

>[AZURE.NOTE] **RunOn** パラメーターは、バージョン 0.9.1 の Microsoft Azure PowerShell で **Start-AzureAutomationRunbook** コマンドレットに追加されました。 必要があります [最新バージョンをダウンロード](http://azure.microsoft.com/downloads) インストールされている以前のバージョンがある場合。 Windows PowerShell から Runbook を開始するワークステーションでは、このバージョンをインストールするだけです。 ワーカー コンピューターから Runbook を開始する場合を除き、そのコンピューターにインストールする必要はありません。 Automation アカウントに最新バージョンの Azure Powershell をインストールする必要があるため、現時点ではある Runbook を別の Runbook の Hybrid Runbook Worker で開始することはできません。 最新バージョンは Azure Automation で自動的に更新され、自動的にワーカーにすぐにプッシュダウンされます。

## Hybrid Runbook Worker での Runbook のトラブルシューティング

[Runbook の出力し、メッセージ](automation-runbook-output-and-messages.md) ハイブリッドから、Azure Automation に送信された runbook ジョブと同じように作業者が、クラウドで実行します。 他の Runbook と同じ方法で、Verbose および Progress ストリームも有効にできます。

ログは、各 Hybrid Worker の C:\ProgramData\Microsoft\System Center\Orchestrator\7.2\SMA\Sandboxes にローカルに格納されます。


## Hybrid Runbook Worker の Runbook の作成

Azure Automation で実行される Runbook と、Hybrid Runbook Worker で実行される Runbook の構造に違いはありません。 ただし、通常、Hybrid Runbook Worker の Runbook はデータ センター内のローカル リソースを管理しますが、Azure Automation の Runbook は Azure クラウド内のリソースを管理するため、ほとんどの場合、それぞれで使用する Runbook は大きく異なります。

### Runbook のアクセス許可

Runbook は Hybrid Runbook Worker でローカル システム アカウントのコンテキストで実行されるため、アクセスするリソースを独自に認証する必要があります。 同じが使用できない [runbook は Azure リソースに対する認証のために通常使用されるメソッド](automation-configuring.md#configuring-authentication-to-azure-resources) Azure 外部のリソースをアクセスするためです。

使用すること [資格情報](http://msdn.microsoft.com/library/dn940015.aspx) と [証明書](http://msdn.microsoft.com/library/dn940013.aspx) さまざまなリソースを認証できるように、資格情報を指定するためのコマンドレットで runbook の資産です。 次の例は、コンピューターを再起動する Runbook の一部を示しています。 資格情報資産から資格情報を取得し、変数資産からはコンピューター名を取得して、Restart-Computer コマンドレットでこれらの値を使用します。

    $Cred = Get-AutomationCredential "MyCredential"
    $Computer = Get-AutomationVariable "ComputerName"
    
    Restart-Computer -ComputerName $Computer  -Credential $Cred

活用することも [InlineScript](automation-powershell-workflow.md#inline-script) で指定された資格情報で別のコンピューター上のコード ブロックを実行することは、 [PSCredential 共通パラメーター](http://technet.microsoft.com/library/jj129719.aspx)します。


### Runbook の作成とテスト

Azure Automation で Hybrid Runbook Worker 用に Runbook を編集することはできますが、エディターで Runbook のテストを試行する場合は困難な場合があります。 ローカル リソースにアクセスする PowerShell モジュールを Azure Automation 環境にインストールできない場合があります。その場合、テストは失敗します。 必要なモジュールをインストールすれば、Runbook は実行されますが、ローカル リソースにアクセスして完全なテストを行うことはできません。

## Service Management Automation との関係

[Service Management Automation (SMA)](https://technet.microsoft.com/library/dn469260.aspx) を使用するローカル データ センターで Azure Automation でサポートされているのと同じ runbook を実行すると、Windows Azure パック (WAP) のコンポーネントです。 Azure Automation とは異なりは、SMA には、runbook および SMA の構成を保持するには、Windows Azure Pack 管理ポータルと、データベースを含むローカル インストールが必要です。 Azure Automation ではクラウドでこれらのサービスが提供されるため、必要なのはローカル環境での Hybrid Runbook Worker の維持のみです。

既存の SMA ユーザーの場合を変更しなくても Hybrid Runbook Worker と併用する」の説明とリソースに独自の認証を実行することと仮定すると、Azure Automation に runbook を移動できます [Hybrid Runbook Worker の runbook を作成する](#creating-runbooks-for-hybrid-runbook-worker)します。 SMA の Runbook は、Runbook の認証を提供できるワーカー サーバーのサービス アカウントのコンテキストで実行されます。

Hybrid Runbook Worker 機能を持つ Azure Automation と Service Management Automation のどちらがより要件に合っているかを判断するために、次の条件を使用できます。

- SMA には、複数のローカル リソースとローカル runbook ワーカーにインストールされているエージェントのみを必要とする Azure Automation より上位のメンテナンス コストを持つ Windows Azure Pack のローカル インストールが必要です。 エージェントは Operations Management Suite によって管理され、メンテナンス コストがより削減されます。
- Azure Automation はその Runbook をクラウド内に格納し、オンプレミスの Hybrid Runbooks Worker に配信します。 セキュリティ ポリシーでこの動作が許可されていない場合は、SMA を使用する必要があります。
- Windows Azure Pack は、無料でダウンロード Azure Automation には、サブスクリプションの料金が発生する可能性があります。
- Hybrid Runbook Worker 機能を持つ Azure Automation を使用することで、Azure Automation と SMA の両方を別々に管理するのではなく、1 つの場所でクラウド リソースとローカル リソースの Runbook を管理できるようになります。
- Azure Automation には、SMA で使用できないグラフィカル作成などの拡張機能があります。


## 関連記事:

- [Azure Automation で Runbook を開始](automation-starting-a-runbook.md)
- [Azure Automation で Runbook の編集](https://msdn.microsoft.com/library/dn879137.aspx)






