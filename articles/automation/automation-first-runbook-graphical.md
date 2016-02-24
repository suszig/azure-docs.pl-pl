<properties
    pageTitle="Azure Automation での初めてのグラフィカルな Runbook"
    description="簡単なグラフィカル Runbook を作成、テスト、および発行する手順を説明するチュートリアルです。  Azure リソースに対する認証、入力パラメーター、条件付きリンクなど、複数の概念について説明します。"
    services="automation"
    documentationCenter=""
    authors="bwren"
    manager="stevenka"
    editor=""/>

<tags
    ms.service="automation"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article" 
    ms.date="09/17/2015"
    ms.author="bwren"/>


# 初めてのグラフィカルな Runbook

> [AZURE.SELECTOR]
- [グラフィカル](automation-first-runbook-graphical.md)
- [PowerShell ワークフロー](automation-first-runbook-textual.md)

このチュートリアルの作成を通じて、 [グラフィカルな runbook](automation-runbook-types.md#graphical-runbooks) Azure Automation でします。  簡単な Runbook を作成、テスト、発行しながら、Runbook ジョブの状態を追跡する方法を説明します。  その後、実際に Azure リソースを管理するように Runbook を変更し、ここでは Azure 仮想マシンを開始します。  そして、Runbook のパラメーターおよび条件付きリンクを追加することによって、Runbook をいっそう堅牢にします。  

## 前提条件

このチュートリアルを最後まで行うには、以下のものが必要です。

- として機能します。 場合は 1 つはまだがあるない、 [MSDN サブスクライバーの特典をアクティブ化](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) または <a href="/pricing/free-trial/" target="_blank">[無料評価版にサインアップ](http://azure.microsoft.com/pricing/free-trial/)します。
- [オートメーション アカウント](automation-configuring.md) runbook を保持します。
- Azure 仮想マシン。  マシンを停止して起動するので、運用環境のものは使用しないでください。
- [Azure の Active Directory ユーザーと Automation の資格情報資産](automation-configuring.md) Azure リソースへの認証にします。  このユーザーには、仮想マシンを開始および停止するアクセス許可が必要です。

## 手順 1 - 新しい Runbook を作成する

We'll start by creating a simple runbook that outputs the text *Hello World*.

1. Azure プレビュー ポータルで、Automation アカウントを開きます。  
Automation アカウント ページでは、そのアカウントのリソースを簡単に確認できます。  既に資産がいくつかあります。  これらのほとんどは、新しい Automation アカウントに自動的に含まれるモジュールです。  記載されている資格情報資産も必要、 [の前提条件](#prerequisites)します。
2. をクリックして、 **Runbook** タイルを開くには runbook の一覧です。<br>
![Runbook の制御](media/automation-first-runbook-graphical/runbooks-control.png)
2. Create a new runbook by clicking on the **Add a runbook** button and then **Create a new runbook**.
3. Give the runbook the name *MyFirstRunbook-Graphical*.
4. ここを作成しようとしている場合は、 [グラフィカルな runbook](automation-graphical-authoring-intro.md) ので選択 **グラフィカル** の **Runbook の種類**します。<br>
![新しい runbook](media/automation-first-runbook-graphical/new-runbook.png)
5. Click **Create** to create the runbook and open the graphical editor.

## 手順 2 - Runbook にアクティビティを追加する

エディターの左側にあるライブラリ コントロールでは、Runbook に追加するアクティビティを選択できます。  We're going to add a **Write-Output** cmdlet to output our text from the runbook.

1.   ライブラリ コントロールで、展開、 **コマンドレット** ノードし **Microsoft.PowerShell.Utility**します。<br>
![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/cmdlets-powershell-utility.png)
2.   一覧の一番下までスクロールします。  Right click **Write-Output** and click **Add to canvas**.
4.   Click on the  **Write-Output** activity on the canvas.  構成コントロールが開き、アクティビティを構成できます。
5.    **ラベル** 既定値は、コマンドレットの名前が、もっとわかりやすい変更しました。 Change it to *Write Hello World to output*.
6.   クリックして **パラメーター** コマンドレットのパラメーターの値を指定します。  
一部のコマンドレットには複数のパラメーター セットがあり、使用するものを選択する必要があります。  この場合、 **Write-output** 1 つだけパラメーター セットなので、いずれかを選択する必要はありません。 <br>
![出力を書き込むプロパティ](media/automation-first-runbook-graphical/write-output-properties.png)
7.   選択、 **InputObject** パラメーター。  このパラメーターでは、出力ストリームに送信するテキストを指定します。
9.    **データソース** ドロップダウンで、選択 **PowerShell 式**します。
 **データソース** ドロップダウンを使用するパラメーター値の設定に使用するさまざまなソースです。  別のアクティビティ、Automation の資産、PowerShell 式などのソースからの出力を使用できます。  この場合、するだけというテキストを出力 *Hello World*します。  PowerShell 式を使用して文字列を指定すればそれが可能です。
10.    **式** ボックスに、入力 *"Hello World"* ] をクリックし、 **OK** 2 回クリックしてキャンバスに戻ります。<br>
![PowerShell 式](media/automation-first-runbook-graphical/expression-hello-world.png)
11.   クリックして、runbook を保存 **保存**します。<br>
![Runbook を保存します。](media/automation-first-runbook-graphical/runbook-edit-toolbar-save.png)

## 手順 3 - Runbook をテストする

Runbook を発行して運用環境で使用できるようにする前に、Runbook をテストして正常に動作することを確認します。  Runbook をテストするときに実行するその **ドラフト** バージョンの出力を対話形式で表示します。  
 
2. クリックして **テスト ペイン** テスト ウィンドウを開きます。<br>
![テスト ウィンドウ](media/automation-first-runbook-graphical/runbook-edit-toolbar-test-pane.png)
2. Click **Start** to start the test.  有効なオプションはこれだけです。
3. A [runbook ジョブ](automation-runbook-execution) を作成し、ステータス ウィンドウに表示します。  
最初のジョブの状態は *キューに入れられた* に利用可能なクラウドの runbook ワーカーが待っていることを示します。  移動し、 *開始*  、ワーカーが、ジョブを要求し、 *を実行している* 、runbook が実際に実行を開始するとき。  
4. Runbook ジョブが完了すると、その出力が表示されます。  この場合、表示されるはず *Hello World*します。<br>
![Hello World](media/automation-first-runbook-graphical/test-output-hello-world.png)
5. テスト ウィンドウを閉じてキャンバスに戻ります。


## 手順 4 - Runbook を発行して開始する

前の手順で作成した Runbook はまだドラフト モードです。 運用環境で実行できるようにするには、発行する必要があります。  Runbook を発行するときは、既存の発行済みバージョンをドラフト バージョンで上書きします。  この例では、Runbook を作成したばかりなので、発行済みバージョンはまだありません。 

1. をクリックして **発行** 、runbook を発行し、 **[はい]** 入力を求められたらします。<br>
![Publish](media/automation-first-runbook-graphical/runbook-edit-toolbar-publish.png)
2. Runbook を表示する左にスクロールする場合、 **Runbook** ウィンドウに表示されます、 **の作成状態** の **公開**します。
3. ウィンドウを表示する権限をスクロールして戻り **MyFirstRunbook**します。  
上部のオプションでは、runbook を開始、または今後、時間に開始して、作成するようにスケジュールすることができる、 [webhook](automation-webhooks.md) 開始できる HTTP 呼び出しで。 
4. Runbook を開始するだけためをクリックして **開始** し、 **はい** 入力を求められたらします。<br>
![Runbook を開始します。](media/automation-first-runbook-graphical/runbook-toolbar-start.png)
5. 作成した Runbook ジョブのジョブ ウィンドウが開かれます。  このウィンドウは閉じてもかまいませんが、ここではジョブの進行状況を確認できるように開いたままにします。
6.  ジョブの状態を示す **ジョブの概要** 、runbook をテストして確認した状態と一致します。<br>
![ジョブの概要](media/automation-first-runbook-graphical/job-pane-summary.png)
7.  Runbook のステータス表示では 1 回 *完了*, 、クリックして **出力**します。   **出力** ウィンドウを開くし、表示、 *Hello World*します。<br>
![ジョブの概要](media/automation-first-runbook-graphical/job-pane-output.png)  
8.  [出力] ウィンドウを閉じます。
9.  クリックして **ストリーム** runbook ジョブのストリーム ウィンドウを開きます。  だけを表示する必要があります *Hello World* 出力にストリームが、このストリームも表示できます他の詳細やエラーなどの runbook ジョブのかどうか、runbook がそれらに書き込みます。<br>
![ジョブの概要](media/automation-first-runbook-graphical/job-pane-streams.png) 
9. ストリーム ウィンドウを閉じると、ジョブ ウィンドウは MyFirstRunbook ウィンドウに戻ります。
9.  クリックして **ジョブ** この runbook のジョブ ウィンドウを開きます。  この Runbook によって作成されたジョブの一覧が表示されます。  1 つのジョブしか実行したので、ジョブ 1 回の一覧だけを表示する必要があります。<br>
![ジョブ](media/automation-first-runbook-graphical/runbook-control-jobs.png) 
9. このジョブをクリックすると、Runbook を開始したときに表示されたものと同じジョブ ウィンドウが開きます。  これにより前に戻って、特定の Runbook に対して作成されたジョブの詳細を見ることができます。

## 手順 5 - Azure リソースを管理するための認証を追加する

Runbook をテストして発行しましたが、これまでのところ役に立つことは何もしていません。  ここでは、Runbook で Azure リソースを管理します。  認証で示されている資格情報を使用している場合を除きも行うことはできません、 [の前提条件](#prerequisites)します。  そうと、 **Set-azureaccount** コマンドレットです。

1.  クリックして、グラフィカル エディターを開きます **編集** MyFirstRunbook ウィンドウでします。<br>
![Runbook を編集します。](media/automation-first-runbook-graphical/runbook-toolbar-edit.png) 
2.  必要ない、 **Write Hello World を出力する** 、したがって右クリックして選択 **削除**します。
8.  In the Library control, expand **Cmdlets** and then **Azure**. 
9.  追加 **Add-azureaccount** をキャンバスにします。<br>
![Add-azureaccount](media/automation-first-runbook-graphical/add-azureaccount.png) 
12.  Select **Add-AzureAccount** and then click **Parameters** in the Configuration pane.
13.  **Add-azureaccount** 複数のパラメーター セットを持つため、パラメーター値を指定する前に、いずれかを選択する必要があります。  をクリックして **パラメーター設定** し、[、 **ユーザー** パラメーターのセットです。
14.  パラメーター セットを選択すると、アクティビティ パラメーター構成ウィンドウにパラメーターが表示されます。  クリックして **資格情報**します。<br>
![Azure アカウントのパラメーターを追加します。](media/automation-first-runbook-graphical/add-azureaccount-parameters.png)
15.  このコマンドレットを使用する資格情報資産は Automation アカウントの選択のために **資格情報資産** の **データソース**します。
16.  起動し、Azure 環境内のバーチャル マシンを停止するアクセス権を持つ資格情報資産を選択します。<br>
![Azure アカウントのデータ ソースの追加](media/automation-first-runbook-graphical/credential-data-source.png)
17.  Click **OK** twice to return to the canvas.


## 手順 6 - 仮想マシンを開始するアクティビティを追加する

We'll now add a **Start-AzureVM** activity to start a virtual machine.  Azure サブスクリプション内の任意の仮想マシンを選択し、ここではその名前をコマンドレットにハードコーディングします。  

1. Expand **Cmdlets** and then **Azure**.
2. 追加 **Start-azurevm** をキャンバスをクリックして、下にドラッグして **Add-azureaccount**します。
11.  ポインターを合わせる **Add-azureaccount** まで、図形の下部に円を表示します。  円をクリックし、矢印をドラッグして **Start-azurevm**します。  
作成した矢印は、 *リンク*します。  Runbook が開始 **Add-azureaccount** し、実行 **Start-azurevm**します。<br>
![Start-azurevm](media/automation-first-runbook-graphical/start-azurevm.png)
5. Select **Start-AzureVM**.  Click **Parameters** and then **Parameter Set** to view the sets for **Start-AzureVM**.  Select  the **ByName** parameter set. 
Note that **Name** and **ServiceName** have exclamation points next them.  This indicates that they are required parameters.
7. Select **Name**.  使用 **定数値** の **データソース** および開始する、仮想マシンの名前を入力します。  Click **OK**.
8. 選択 **ServiceName**します。  使用 **定数値** の **データソース** および開始する、仮想マシンの名前を入力します。  クリックして **OK**します。<br>
![Start-azurevm のパラメーター](media/automation-first-runbook-graphical/start-azurevm-params.png)
9. テスト ウィンドウをクリックして、Runbook をテストできるようにします。
10. Click **Start** to start the test.  完了したら、仮想マシンが開始されたことを確認します。


## 手順 7 - Runbook に入力パラメーターを追加する

この runbook は現在で指定した仮想マシンを開始、 **Start-azurevm** 、このコマンドレットでは、runbook は便利であること場合は、runbook を開始したときに、仮想マシンを指定できます。  そこで次に、入力パラメーターを Runbook に追加して、その機能を提供します。

1. Open the graphical editor by clicking **Edit** on the **MyFirstRunbook** pane.
2. をクリックして **入力と出力** し **入力の追加** 、Runbook の入力パラメーター ウィンドウを開きます。<br>
![Runbook の入力と出力](media/automation-first-runbook-graphical/runbook-edit-toolbar-inputoutput.png)
4. Specify *VMName* for the **Name**.  Keep *string* for the **Type**, but change **Mandatory** to *Yes*.  Click **OK**.
5. 呼ばれる 2 番目の必須入力パラメーターを作成 *VMServiceName* ] をクリックし、 **[ok]** を閉じる、 **の入力し、出力** ウィンドウです。<br>
![Runbook の入力パラメーター](media/automation-first-runbook-graphical/input-parameters.png) 
6. Select the **Start-AzureVM** activity and then click **Parameters**.
7. 変更、 **データソース** の **名** に **Runbook の入力** し、[ **VMName**します。<br>
![Start-azurevm の Name パラメーター](media/automation-first-runbook-graphical/vmname-property.png) 
8. 変更、 **データソース** の **ServiceName** に **Runbook の入力** し、[ **VMServiceName**します。<br>
![Start-azurevm のパラメーター](media/automation-first-runbook-graphical/start-azurevm-params-inputs.png) 
9. Runbook を保存してテスト ウィンドウを開きます。  テストで使用される 2 つの入力変数の値を指定できることに注意してください。 
11.  テスト ウィンドウを閉じます。
12.  Click **Publish** to publish the new version of the runbook.
13.  前の手順で開始した仮想マシンを停止します。
13.  クリックして **開始** runbook を開始します。  入力、 **VMName** と **VMServiceName** を起動しようとして、仮想マシンのです。<br>
![Runbook を開始します。](media/automation-first-runbook-graphical/start-runbook-input-params.png) 
14.  Runbook が完了したら、仮想マシンが開始されたことを確認します。

## 手順 8 - 条件付きリンクを作成する

次に、まだ開始されていない場合にのみ Runbook を開始するように Runbook を変更します。  それでは、追加することで、 **Get-azurevm** コマンドレットでは、仮想マシンの現在の状態を含む runbook をします。  のみを実行する条件付きリンクを追加し、 **Start-azurevm** 現在実行中の状態が停止している場合。  If the runbook isn't stopped, then output a message.

1. Open **MyFirstRunbook** in the graphical editor.
2. Remove the link between **Add-AzureAccount** and **Start-AzureVM** by clicking on it and then pressing the *Delete* key.
3. In the Library control, expand **Cmdlets** and then **Azure**.
4. Add **Get-AzureVM** to the canvas.
5. リンクを作成する **Add-azureaccount** に **Get-azurevm**します。  別のリンクを作成する **Get-azurevm** に **Start-azurevm**します。<br>
![Get-azurevm での Runbook](media/automation-first-runbook-graphical/get-azurevm.png)   
6.  Select **Get-AzureVM** and click **Parameters**.  Select the parameter set *GetVMByServiceAndVMName*.
7.  For **Name**, set the **Date source** to **Runbook input** and then select **VMName**.   
7.  For **ServiceName**, set the **Date source** to **Runbook input** and then select **VMServiceName**.  
8.  Select the link between **Get-AzureVM** and **Start-AzureVM**.
9.  In the Configuration pane, change **Apply condition** to **True**.  条件が真の場合にのみ対象のアクティビティが実行されることを示す破線にリンクが変わることに注意してください。
10.   **条件式**, 、型 *$ActivityOutput [' Get-azurevm ']。PowerState-eq"Stopped"*します。  **Start-azurevm** のみ実行される仮想マシンが停止している場合。<br>
![リンク条件](media/automation-first-runbook-graphical/link-condition.png) 
11.  In the Library control, expand **Cmdlets** and then **Microsoft.PowerShell.Utility**.
12.  Add **Write-Output** to the canvas.
13.  Create a link from **Get-AzureVM** to **Write-Output**.
14.  リンクを選択し、変更 **条件が適用** に **True**します。
14.   **条件式**, 、型 *$ActivityOutput [' Get-azurevm ']。PowerState-ne"Stopped"*します。  **出力を書き込む** 仮想マシンが停止していない場合にのみ実行ようになりました。<br>
![Write-output での Runbook](media/automation-first-runbook-graphical/write-output-link.png) 
15.  Select **Write-Output** and click **Parameters**.
16.  For **InputObject**, change **Data source** to **PowerShell expression** and type in the expression *"$VMName.Name already started."*.
17.  Save the runbook and open the Test pane.
18.  仮想マシンが停止している状態で Runbook を開始すると、仮想マシンが開始します。
19.  仮想マシンが開始している状態で Runbook を開始すると、仮想マシンが既に開始しているという出力が表示されます。
 

## 関連記事:

- [Azure Automation でのグラフィカル作成](automation-graphical-authoring-intro.md)
- [最初の PowerShell Workflow Runbook](automation-first-runbook-textual.md)

 
