<properties 
   pageTitle="Azure Automation でのグラフィカル作成"
   description="グラフィカル作成では、コードを操作せずに Azure Automation の Runbook を作成することができます。   この記事では、グラフィカル作成の概要と、グラフィカル Runbook の作成を開始するうえで必要なすべての詳細情報を示します。"
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
   ms.date="11/05/2015"
   ms.author="bwren" />

# Azure Automation でのグラフィカル作成

## はじめに

グラフィカル作成では、基になる Windows PowerShell ワークフローの複雑なコードを使用しなくても、Azure Automation の Runbook を作成することができます。  キャンバスに、コマンドレット ライブラリのアクティビティやその他のアクティビティを追加し、まとめてリンクしてワークフローを形成します。

この記事では、グラフィカル作成の概要と、グラフィカル Runbook の作成を開始するうえで必要な概念について説明します。

## グラフィック Runbook

Azure Automation の Runbook はすべて Windows PowerShell ワークフローです。  グラフィカル Runbook では、Automation ワーカーによって実行される PowerShell コードを生成しますが、ユーザーがそれを表示したり、直接変更したりすることはできません。  グラフィカル Runbook をテキスト Runbook に変換することも、既存のテキスト Runbook をグラフィカル エディターにインポートすることもできません。


## グラフィカル エディターの概要

Azure プレビュー ポータルでグラフィカル Runbook を作成または編集することで、グラフィカル エディターを開くことができます。

![グラフィカル ワークスペース](media/automation-graphical-authoring-intro/graphical-editor.png)


次のセクションでは、グラフィカル エディターのコントロールについて説明します。


### キャンバス
キャンバスは Runbook を設計する場所です。  ライブラリ コントロールのノードから Runbook にアクティビティを追加し、リンクで接続して Runbook のロジックを定義します。

### ライブラリ コントロール

ライブラリ コントロールでは、選択 [アクティビティ](#activities) 、runbook を追加します。  アクティビティをキャンバスに追加し、そこで他のアクティビティに接続します。  これには、以下の表で説明する次の 4 つのセクションが含まれます。

| セクション | 説明 |
|:---|:---|
| コマンドレット | Runbook で使用できるすべてのコマンドレットが含まれています。  コマンドレットはモジュールごとに整理されます。  オートメーション アカウントにインストールしたモジュールをすべて使用できます。  |
| Runbooks |  タグ別に整理されたオートメーション アカウントに Runbook が含まれています。  Runbook には複数のタグを設定できるため、これらの runbook は子 runbook として使用するキャンバスに追加できる複数のタグの下にある表示場合があります。 現在編集中の Runbook は表示されますが、それ自体を呼び出すことはできないため、キャンバスには追加できません。
| 資産 | 含む、 [オートメーションの資産](http://msdn.microsoft.com/library/dn939988.aspx) runbook で使用できるオートメーション アカウントにします。  Runbook に資産を追加すると、選択した資産を取得するワークフロー アクティビティが追加されます。  変数資産の場合は、変数を取得するアクティビティを追加するか、変数を設定するアクティビティを追加するかを選択できます。
| Runbook コントロール | 現在の Runbook で使用できる Runbook コントロール アクティビティが含まれています。 A *接合* 複数の入力を受け取り、ワークフローを続行する前にすべてが完了するまでに待機します。 A *ワークフロー スクリプト* PowerShell ワークフロー コードの 1 つまたは複数の行を実行します。  カスタム コードや、他のアクティビティでは実現できない機能のためにこのアクティビティを使用できます。|

### 構成コントロール

構成コントロールは、キャンバスで選択したオブジェクトの詳細を提供する場所です。このコントロールで使用できるプロパティは、選択したオブジェクトの種類によって異なります。  構成コントロールでオプションを選択すると、追加情報を提供するための追加のブレードが開きます。

### テスト コントロール

グラフィカル エディターが初めて起動された場合、テスト コントロールは表示されません。 開かれたときに、対話形式で [グラフィカル runbook をテスト](#graphical-runbook-procedures)します。  

## グラフィカル Runbook の手順 

### グラフィカル Runbook のエクスポートとインポート

グラフィカル Runbook は、発行済みのバージョンのみエクスポートできます。  Runbook が発行されていない場合、 **発行エクスポート** ボタンが無効になります。  クリックすると、 **発行エクスポート** ボタン、runbook はローカル コンピューターにダウンロードします。  ファイルの名前が使用して、runbook の名前と一致する、 *graphrunbook* 拡張機能です。

![発行済み分のエクスポート](media/automation-graphical-authoring-intro/runbook-export.png)

グラフィカルな runbook ファイルをインポートするにを選択して、 **インポート** runbook を追加するときのオプションです。   インポートするファイルを選択すると、同じを保持できます **名前** か、新しいものを提供します。

![Runbook のインポート](media/automation-graphical-authoring-intro/runbook-import.png)


### グラフィカル Runbook のテスト

発行バージョンの Runbook を変更せずに、ドラフト バージョンの Runbook を Azure プレビュー ポータルでテストしたり、新しい Runbook を発行前にテストしたりすることができます。 これにより、発行バージョンを置き換える前に、Runbook が正常に機能していることを確認することができます。 Runbook のテスト時に、ドラフトの Runbook が実行され、行われたすべての操作が完了します。 ジョブ履歴は作成されませんが、出力がテスト出力ペインに表示されます。 

Open the Test control for a runbook by opening the runbook for edit and then click on the **Test pane** button.

![[テスト ペイン] ボタン](media/automation-graphical-authoring-intro/runbook-edit-test-pane.png)

テストのコントロールはに関するプロンプトを出します入力パラメーター] をクリックして、runbook を開始する、 **開始** ] ボタンをクリックします。

![テスト コントロールのボタン](media/automation-graphical-authoring-intro/runbook-test-start.png)

### グラフィカル Runbook の発行

Azure Automation の各 Runbook には、ドラフト バージョンと発行バージョンがあります。 実行できるのは発行バージョンのみで、編集できるのはドラフト バージョンのみです。 ドラフト バージョンを変更しても発行バージョンに影響はありません。 ドラフト バージョンを使用する準備ができたら、それを発行します。これにより、発行バージョンがドラフト バージョンで上書きされます。

編集をクリックし、runbook を開いて、グラフィカルな runbook を発行することができます、 **発行** ] ボタンをクリックします。

![[発行] ボタン](media/automation-graphical-authoring-intro/runbook-edit-publish.png)

When a runbook has not yet been published, it has a status of **New**.  状態が公開されると、 **公開**します。  パブリッシュされている、ドラフトと発行バージョンが異なる後に、runbook を編集する際、runbook のステータス **編集**します。

![Runbook のステータス](media/automation-graphical-authoring-intro/runbook-statuses.png) 

また、発行バージョンの Runbook に戻すこともできます。  これにより、Runbook が最後に発行されてから加えられた変更が破棄され、ドラフト バージョンの Runbook が発行バージョンに置き換えられます。

![発行済みに戻すボタン](media/automation-graphical-authoring-intro/runbook-edit-revert-published.png)


## アクティビティ

アクティビティは Runbook の構成要素です。  An activity can be a PowerShell cmdlet, a child runbook, or a workflow activity.  右クリックして、ライブラリ コントロールを選択して、runbook にアクティビティを追加する **[キャンバスに追加**します。  その後、アクティビティをクリックしてドラッグし、キャンバス上の任意の場所に配置できます。  キャンバス上のアクティビティの場所が、Runbook の操作に影響することは決してありません。  Runbook をレイアウトできますが、その操作を視覚化するために最適なレイアウトを見つけてください。 

![キャンバスに追加](media/automation-graphical-authoring-intro/add-to-canvas.png)

構成ブレードでプロパティとパラメーターを構成するには、キャンバス上でアクティビティを選択します。  変更することができます、 **ラベル** に説明するようなものにアクティビティのです。  元のコマンドレットは引き続き実行されるため、グラフィカル エディターで使用される表示名を変更するだけです。  ラベルは、Runbook 内で一意である必要があります。 

### パラメーター セット

パラメーター セットは、特定のコマンドレットの値を受け入れる必須および省略可能なパラメーターを定義します。  すべてのコマンドレットには 1 つ以上のパラメーター セットがあり、複数存在する場合があります。  コマンドレットに複数のパラメーター セットがある場合は、使用するものを選択してからパラメーターを構成する必要があります。  構成可能なパラメーターは、選択するパラメーター セットによって異なります。  選択して、アクティビティによって使用されるパラメーター セットを変更する **パラメーター設定** 別のセットを選択するとします。  この場合、構成したパラメーター値はすべて失われます。

次の例では、Get-AzureVM コマンドレットに 2 つのパラメーター セットがあります。  パラメーター セットのいずれかを選択するまでは、パラメーター値を構成することはできません。  ListAllVMs パラメーター セットはすべての仮想マシンを返すためのものであり、1 つの省略可能なパラメーターがあります。  GetVMByServiceand VMName は、返される仮想マシンを指定するためのものであり、1 つの必須パラメーターと 2 つの省略可能なパラメーターがあります。

![パラメーター セット](media/automation-graphical-authoring-intro/parameter-set.png)

#### パラメーターの値

パラメーターの値を指定する場合は、データ ソースを選択して値の指定方法を決定します。  特定のパラメーターに使用できるデータ ソースは、そのパラメーターの有効な値によって異なります。  たとえば、null 値が許可されていないパラメーターに対するオプションとして Null を使用することはできません。

| データ ソース | 説明 |
|:---|:---|
|定数値|パラメーターの値を入力します。  これを使用できるのは、Int32、Int64、String、Boolean、DateTime、Switch というデータ型の場合のみです。 |
|アクティビティの出力|ワークフローの現在のアクティビティより前のアクティビティからの出力です。  有効なすべてのアクティビティが一覧表示されます。  パラメーター値にその出力を使用するアクティビティだけを選択します。  アクティビティが複数のプロパティを持つオブジェクトを出力する場合は、アクティビティを選択してからプロパティの名前を入力できます。|
|Runbook の入力パラメーター|Runbook の入力パラメーターをアクティビティ パラメーターへの入力として選択します。|  
|Automation の変数資産|Automation の変数を入力として選択します。|  
|Automation の資格情報資産|Automation の資格情報を入力として選択します。|  
|Automation の証明書資産|Automation の証明書を入力として選択します。|  
|Automation の接続資産|Automation の接続を入力として選択します。| 
|PowerShell 式|単純な指定 [PowerShell 式](#powershell-expressions)します。  式は、アクティビティとパラメーター値に使用される結果の前に評価されます。  変数を使用すれば、アクティビティの出力または Runbook の入力パラメーターを参照することができます。|
|空の文字列|空の文字列値です。|
|Null|Null 値です。|
|選択解除|以前に構成された値をクリアします。|


#### 省略可能な追加パラメーター

すべてのコマンドレットで、追加のパラメーターを指定することができます。  これらは、PowerShell の共通パラメーターまたはその他のカスタム パラメーターです。  PowerShell 構文を使用してパラメーターを指定できるテキスト ボックスが表示されます。  たとえば、使用するため、 **Verbose** 共通パラメーターを指定する **"-Verbose: $True"**します。

### ワークフロー スクリプト コントロール

ワークフロー スクリプト コントロールは特別なアクティビティで、機能を提供する (そうしないと使用できない場合がある) ために PowerShell ワークフローのコードを受け入れます。  これは完全なワークフローではありませんが、PowerShell ワークフロー コードの有効な行が含まれている必要があります。  これはパラメーターを受け入れることはできませんが、アクティビティの出力と Runbook の入力パラメーターで変数を使用できます。  発信リンクがない (Runbook の出力に追加される) 場合を除き、アクティビティの出力はデータバスに追加されます。

たとえば、次のコードでは $NumberOfDays という Runbook 入力変数を使用して、日付の計算を実行します。  次に、計算した日時が出力として送信され、Runbook の後続のアクティビティで使用されます。

    $DateTimeNow = InlineScript{(Get-Date).ToUniversalTime()}
    $DateTimeStart = InlineScript{($using:DateTimeNow).AddDays(-$using:NumberOfDays)}
    $DateTimeStart


## リンクとワークフロー

A **link** in a graphical runbook connects two activities.  キャンバスでは、ソース アクティビティから接続先アクティビティを指す矢印として表示されます。  アクティビティは矢印の方向に実行され、ソース アクティビティが完了すると接続先アクティビティが開始されます。  

### リンクの作成

ソース アクティビティを選択し、図形の下部にある円をクリックして、2 つのアクティビティ間のリンクを作成します。  接続先アクティビティまで矢印をドラッグして離します。

![リンクの作成](media/automation-graphical-authoring-intro/create-link.png)

リンクを選択して、構成ブレードでそのプロパティを構成します。  これには、次の表で説明されているリンクの種類が含まれます。

| リンクの種類 | 説明 |
|:---|:---|
| パイプライン | ソース アクティビティからのオブジェクト出力ごとに 1 回、接続先アクティビティが実行されます。  ソース アクティビティからの出力がない場合、接続先アクティビティは実行されません。  ソース アクティビティからの出力はオブジェクトとして使用できます。  |
| シーケンス | 接続先アクティビティは 1 回だけ実行されます。  ソース アクティビティからのオブジェクトの配列を受け取ります。  ソース アクティビティからの出力は、オブジェクトの配列として使用できます。 |

### アクティビティの開始

グラフィカル Runbook は、受信リンクのないすべてのアクティビティから開始します。  多くの場合、Runbook の開始アクティビティとして動作するアクティビティは 1 つだけです。  複数のアクティビティに受信リンクがない場合、Runbook は並列で実行して開始します。  その後、リンクに従って、各アクティビティの完了時に別のアクティビティを実行します。

### 条件

リンクに条件を指定すると、その条件が true に解決された場合にのみ、接続先アクティビティが実行されます。  ソース アクティビティからの出力を取得する場合、通常は条件に $ActivityOutput 変数を使用します。  

パイプライン リンクの場合は、1 つのオブジェクトに対して条件を指定します。この条件は、ソース アクティビティによってオブジェクトの出力ごとに評価されます。  その後、条件を満たす各オブジェクトに対して、接続先アクティビティが実行されます。  たとえば、Get-AzureVM のソース アクティビティの場合は、条件付きパイプライン リンクで次の構文を使用して、現在実行されている仮想マシンのみを取得できます。  

    $ActivityOutput['Get-AzureVM'].PowerState -eq 'Started'

シーケンス リンクの場合は、ソース アクティビティからのオブジェクト出力をすべて含む 1 つの配列が返されるため、条件は一度だけ評価されます。  そのため、シーケンス リンクをパイプライン リンクのようにフィルター処理に使用することはできませんが、次のアクティビティが実行されるかどうかは簡単に判別されます。  次のコードは、実行されている仮想マシンを判別するために Get-AzureVM からの出力を評価する同じ例を示しています。  この場合、コードは配列内の各オブジェクトを確認して、1 つ以上の仮想マシンが実行されている場合は true に解決されます。  接続先アクティビティでは、このデータを解析する必要があります。

    $test = $false
    $VMs = $ActivityOutput['Get-AzureVm']
    Foreach ($VM in VMs)
    {
        If ($VM.PowerState –eq 'Started')
            {
                $test = $true
            }
    }
    $test

条件付きリンクを使用する場合は、その分岐でソース アクティビティから他のアクティビティまでの使用可能なデータが、条件によってフィルター処理されます。  アクティビティが複数のリンクに対するソースの場合、各分岐のアクティビティで使用できるデータは、その分岐に接続するリンクの条件によって異なります。

たとえば、以下の Runbook のソース アクティビティではすべての仮想マシンが取得されます。  It has two conditional links and a link without a condition.  The first conditional link uses the expression *$ActivityOutput['Get-AzureVM'].PowerState -eq 'Started'* to filter only virtual machines that are currently running.  2 つ目は、式を使用して *$ActivityOutput [' Get-azurevm ']。PowerState-eq 'Stopped'* 現在停止している仮想マシンのみをフィルター処理します。  

![条件付きリンクの例](media/automation-graphical-authoring-intro/conditional-links.png)

最初のリンクに従って、Get-azurevm からのアクティビティ出力を使用するアクティビティでは、Get-AzureVM の実行時に開始された仮想マシンのみを取得します。  2 番目のリンクに従うアクティビティでは、Get-AzureVM の実行時に停止された仮想マシンのみを取得します。  3 番目のリンクに従うアクティビティでは、実行状態に関係なく、すべての仮想マシンを取得します。

### 接合

接合は特別なアクティビティで、受信分岐がすべて完了するまで待機します。  これにより、複数のアクティビティを並列で実行し、次に進む前にすべて完了したことを確認することができます。

接合できる受信リンクの数に制限はありませんが、パイプラインにすることができるのはこれらのリンクのうち 1 つだけです。  受信シーケンス リンクの数に制限はありません。  複数の受信パイプライン リンクを受け入れる接合点を作成し、Runbook を保存することはできますが、実行した場合は失敗します。

次の例は、一連の仮想マシンに同時に修正プログラムをダウンロードする間に、それらのマシンを開始する Runbook の一部です。  接合点を使用して、Runbook を続行する前に両方のプロセスが完了したことを確認します。

![接合](media/automation-graphical-authoring-intro/junction.png)

### サイクル

サイクルとは、接続先アクティビティがリンクに従ってそのソース アクティビティ、または最終的にソースに戻る別のアクティビティに戻ることです。  現在、グラフィカル作成ではサイクルを使用することはできません。  Runbook にサイクルがある場合、保存は正常に行われますが、実行時にエラーが発生します。

![サイクル](media/automation-graphical-authoring-intro/cycle.png)

### ループ

ループとは、アクティビティを指定した回数だけ繰り返すか、特定の条件が満たされるまで繰り返すことです。  現在、グラフィカル Runbook ではループはサポートされていません。   

### アクティビティ間のデータの共有

[送信] リンクを持つアクティビティから出力されるデータが書き込まれる、 *databus* 、runbook に対応します。  Runbook のすべてのアクティビティはデータバスのデータを使用して、パラメーターの値を設定したり、スクリプト コードに含めたりすることができます。  アクティビティは、ワークフロー内の前のアクティビティの出力にアクセスできます。     

データバスにデータが書き込まれる方法は、アクティビティのリンクの種類によって異なります。   **パイプライン**, 、データは、複数のオブジェクトとして出力します。  For a **sequence** link, the data is output as an array.  値が 1 つだけある場合、1 つの要素配列として出力されます。

2 つの方法のいずれかを使用して、データバス上のデータにアクセスできます。  最初を使用して、 **アクティビティの出力** データ ソースを別のアクティビティのパラメーターを設定します。  出力がオブジェクトの場合は、1 つのプロパティを指定できます。

![アクティビティの出力](media/automation-graphical-authoring-intro/activity-output-datasource.png)

内のアクティビティの出力を取得することも、 **PowerShell 式** データ ソースまたはから、 **ワークフロー スクリプト** 、ActivityOutput 変数を含むアクティビティです。  出力がオブジェクトの場合は、1 つのプロパティを指定できます。  ActivityOutput 変数には次の構文を使用します。

    $ActivityOutput['Activity Label']
    $ActivityOutput['Activity Label'].PropertyName 

### チェックポイント

設定するための同じガイダンス [チェックポイント](automation-powershell-workflow/#checkpoints) runbook では、グラフィカル runbook には適用されます。  チェックポイントを設定する必要がある場合は、Checkpoint-Workflow コマンドレットのアクティビティを追加することができます。  次に、このアクティビティの後に Add-AzureAccount を続ける必要があります (Runbook が別のワーカーのこのチェックポイントから開始する場合)。 


## Azure リソースの認証

Azure Automation のほとんどの Runbook では、Azure リソースの認証が必要になります。  この認証に使用される標準的な方法は、Add-azureaccount コマンドレットと、 [資格情報資産](http://msdn.microsoft.com/library/dn940015.aspx) Azure アカウントにアクセス権を持つ Active Directory のユーザーを表します。  詳細については [Azure Automation の構成](automation-configuring.md)します。

グラフィカル Runbook にこの機能を追加するには、キャンバスに資格情報資産を追加します。この後に Add-AzureAccount アクティビティが続きます。  Add-AzureAccount では、資格情報アクティビティを使用して入力します。  これを次の例に示します。

![認証アクティビティ](media/automation-graphical-authoring-intro/authentication-activities.png)

Runbook の開始時と各チェックポイント後に認証を行う必要があります。  つまり、すべての Checkpoint-Workflow アクティビティの後に Add-AzureAccount アクティビティを追加します。 資格情報アクティビティは同じものを使用できるため、追加する必要はありません。 

![アクティビティの出力](media/automation-graphical-authoring-intro/authentication-activity-output.png)

## Runbook の入力と出力

### Runbook の入力

Runbook では、Azure プレビュー ポータルで Runbook を開始する場合はユーザーから、現在の Runbook が子として使用されている場合は別の Runbook からの入力が必要になる可能性があります。
たとえば、仮想マシンを作成する Runbook がある場合、Runbook を開始するたびに仮想マシンの名前や他のプロパティなどの情報提供が必要になることがあります。  

Runbook の入力を受け入れる場合は、1 つ以上の入力パラメーターを定義します。  Runbook が開始されるたびに、これらのパラメーターの値を指定します。  Azure プレビュー ポータルで Runbook を開始すると、Runbook の入力パラメーターのそれぞれの値を指定するように求められます。

クリックして、runbook の入力パラメーターを表示、 **入力と出力** runbook ツールバーのボタンをクリックします。  

![Runbook の入出力](media/automation-graphical-authoring-intro/runbook-edit-input-output.png) 

開き、 **の入力し、出力** コントロールの既存の入力パラメーターを編集またはをクリックして新しいものを作成できます **入力の追加**します。 

![入力の追加](media/automation-graphical-authoring-intro/runbook-edit-add-input.png)

各入力パラメーターは、次の表のプロパティで定義されます。

|プロパティ|説明|
|:---|:---|
| 名前 | パラメーターの一意の名前です。  これに含めることができるのは英数字のみです。スペースを含めることはできません。 |
| 説明 | 入力パラメーターのオプションの説明です。  |
| 型 | パラメーター値に必要なデータ型です。  Azure プレビュー ポータルでは、入力を求められた場合に各パラメーターのデータ型に適したコントロールが提供されます。 |
| 必須 | パラメーターの値を指定する必要があるかどうかを示します。  既定値が定義されていない各必須パラメーターの値を指定しないと、Runbook を開始できません。 |
| 既定値 | パラメーターに値が指定されていない場合は、使用する値を指定します。  Null または特定の値を指定できます。 |


### Runbook の出力

発信リンクがない任意のアクティビティによって作成されたデータに追加されます、 [、runbook の出力](http://msdn.microsoft.com/library/azure/dn879148.aspx)します。  出力は Runbook ジョブと共に保存され、Runbook を子として使用する場合に親 Runbook で使用できます。  


## PowerShell の式

グラフィカル作成の利点の 1 つは、PowerShell の最小限の知識があれば Runbook を作成できることです。  現時点では、知識を PowerShell の特定の入力にもする必要は [パラメーター値](#activities) と設定の [条件結合](#links-and-workflow)します。  ここでは、PowerShell の式のことをよく知らないユーザーのためにその概要について簡単に説明します。  PowerShell の完全な詳細情報については、「 [Windows PowerShell でのスクリプティング](http://technet.microsoft.com/library/bb978526.aspx)します。 


### PowerShell 式のデータ ソース

データ ソースとしての値の設定に、PowerShell 式を使用することができます、 [アクティビティ パラメーター](#activities) PowerShell のコードをいくつかの結果。  簡単な関数を実行する 1 行のコードでも、複雑なロジックを実行する複数行のコードでもかまいません。  変数に割り当てられていないコマンドからの出力は、パラメーター値に出力されます。 

たとえば、次のコマンドは現在の日付を出力します。 

    Get-Date

次のコマンドは、現在の日付から文字列を作成し、変数に割り当てます。  その後、変数の内容は出力に送られます。 

    $string = "The current date is " + (Get-Date)
    $string

次のコマンドは、現在の日付を評価し、現在の日付が週末か平日かを示す文字列を返します。 

    $date = Get-Date
    if (($date.DayOfWeek = "Saturday") -or ($date.DayOfWeek = "Sunday")) { "Weekend" }
    else { "Weekday" }
    
 

### アクティビティの出力

前のアクティビティからの出力を Runbook で使用するには、次の構文で $ActivityOutput 変数を使用します。

    $ActivityOutput['Activity Label'].PropertyName

たとえば、仮想マシンの名前を必要とするプロパティを持つアクティビティがある場合、次の式を使用します。

    $ActivityOutput['Get-AzureVm'].Name

プロパティだけでなく仮想マシン オブジェクトを必要とするプロパティの場合は、次の構文を使用してオブジェクト全体を返します。

    $ActivityOutput['Get-AzureVm']

テキストを仮想マシン名に連結する次のようなさらに複雑な式で、アクティビティの出力を使用することもできます。

    "The computer name is " + $ActivityOutput['Get-AzureVm'].Name


### 条件

使用 [比較演算子](https://technet.microsoft.com/library/hh847759.aspx) を値の比較または指定したパターンに一致するかどうかは値を確認します。  比較からは、値 $true または $false が返ります。

たとえば、次の条件はアクティビティから仮想マシンが名前付きかどうかを決定 *Get-azurevm* は現在 *停止*します。 

    $ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped"

同じ仮想マシンが以外には任意の状態にするかどうか、次の条件をチェック *停止*します。

    $ActivityOutput["Get-AzureVM"].PowerState –ne "Stopped"

使用して複数の条件を結合する、 [論理演算子](https://technet.microsoft.com/library/hh847789.aspx) など **- および** または **- または**です。  たとえば、次の条件のチェック前の例では、同じ仮想マシンがの状態であるかどうか *停止* または *停止*します。

    ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopped") -or ($ActivityOutput["Get-AzureVM"].PowerState –eq "Stopping") 


### ハッシュテーブル

[ハッシュ テーブル](http://technet.microsoft.com/library/hh847780.aspx) 名前/値ペアの値のセットを返すに便利なは。  一部のアクティビティのプロパティでは、単純な値ではなくハッシュテーブルが想定されている場合があります。  ハッシュテーブルはディクショナリと考えることもできます。 

ハッシュテーブルは次の構文で作成します。  ハッシュテーブルは任意の数のエントリを含むことができますが、エントリごとに名前と値によって定義されます。

    @{ <name> = <value>; [<name> = <value> ] ...}

たとえば、次の式は、インターネット検索用の値のハッシュテーブルを期待するアクティビティ パラメーターに対するデータ ソースで使用されるハッシュテーブルを作成します。

    $query = "Azure Automation"
    $count = 10
    $h = @{'q'=$query; 'lr'='lang_ja';  'count'=$Count}
    $h

次のコードの例では、というアクティビティから出力 *Twitter 接続の取得* 、ハッシュ テーブルを作成します。

    @{'ApiKey'=$ActivityOutput['Get Twitter Connection'].ConsumerAPIKey;
      'ApiSecret'=$ActivityOutput['Get Twitter Connection'].ConsumerAPISecret;
      'AccessToken'=$ActivityOutput['Get Twitter Connection'].AccessToken;
      'AccessTokenSecret'=$ActivityOutput['Get Twitter Connection'].AccessTokenSecret}



## 関連記事:

- [Windows PowerShell ワークフローについて](automation-powershell-workflow.md)
- [Automation 資産](http://msdn.microsoft.com/library/azure/dn939988.aspx)
- [演算子](https://technet.microsoft.com/library/hh847732.aspx)
 

