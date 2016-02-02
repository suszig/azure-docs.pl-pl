<properties
    pageTitle="Azure Automation での初めてのグラフィカルな Runbook"
    description="簡単なグラフィカル Runbook を作成、テスト、および発行する手順を説明するチュートリアルです。Azure リソースに対する認証、入力パラメーター、条件付きリンクなど、複数の概念について説明します。"
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
- [Graphical](automation-first-runbook-graphical.md)
- [PowerShell Workflow](automation-first-runbook-textual.md)


このチュートリアルの作成を通じて、 [グラフィカルな runbook](automation-runbook-types.md#graphical-runbooks) Azure Automation でします。 簡単な Runbook を作成、テスト、発行しながら、Runbook ジョブの状態を追跡する方法を説明します。 その後、実際に Azure リソースを管理するように Runbook を変更し、ここでは Azure 仮想マシンを開始します。 そして、Runbook のパラメーターおよび条件付きリンクを追加することによって、Runbook をいっそう堅牢にします。

## 前提条件

このチュートリアルを最後まで行うには、以下のものが必要です。

- として機能します。場合は 1 つはまだがあるない、 [MSDN サブスクライバーの特典をアクティブ化](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) または <a href="/pricing/free-trial/" target="_blank">[無料評価版にサインアップ](http://azure.microsoft.com/pricing/free-trial/)します。
- [オートメーション アカウント](automation-configuring.md) runbook を保持します。
- Azure 仮想マシン。 マシンを停止して起動するので、運用環境のものは使用しないでください。
- [Azure Active Directory ユーザーとオートメーション資格情報資産](automation-configuring.md) Azure リソースへの認証にします。 このユーザーには、仮想マシンを開始および停止するアクセス許可が必要です。

## 手順 1 - 新しい Runbook を作成する

最初に、*Hello World* というテキストを出力する簡単な Runbook を作成します。

1. Azure プレビュー ポータルで、Automation アカウントを開きます。  
Automation アカウント ページでは、そのアカウントのリソースを簡単に確認できます。 既に資産がいくつかあります。 これらのほとんどは、新しい Automation アカウントに自動的に含まれるモジュールです。 記載されている資格情報資産も必要、 [の前提条件](#prerequisites)します。
2. をクリックして、 **Runbook** タイルを開くには runbook の一覧です。<br>
![Runbook の制御](media/automation-first-runbook-graphical/runbooks-control.png)
2. **[Runbook の追加]** ボタンをクリックし、次に **[新しい Runbook の作成]** をクリックして、新しい Runbook を作成します
3. Runbook の名前を *MyFirstRunbook-Graphical* に設定します。
4. ここを作成しようとしている場合は、 [グラフィカルな runbook](automation-graphical-authoring-intro.md) ので選択 **グラフィカル** の **Runbook の種類**します。<br>
![新しい Runbook](media/automation-first-runbook-graphical/new-runbook.png)
5. **[作成]** をクリックして Runbook を作成し、グラフィカル エディターを開きます。

## 手順 2 - Runbook にアクティビティを追加する

エディターの左側にあるライブラリ コントロールでは、Runbook に追加するアクティビティを選択できます。 **Write-Output** コマンドレットを追加し、Runbook からテキストを出力します。

1.   ライブラリ コントロールで、展開、 **コマンドレット** ノードし **Microsoft.PowerShell.Utility**します。<br>
![Microsoft.PowerShell.Utility](media/automation-first-runbook-graphical/cmdlets-powershell-utility.png)
2.   一覧の一番下までスクロールします。 **[Write-Output]** を右クリックし、**[キャンバスに追加]** をクリックします。
4.   をクリックして、  **Write-output** キャンバス上のアクティビティです。 構成コントロールが開き、アクティビティを構成できます。
5.   **[ラベル]** の既定値はコマンドレットの名前ですが、もっとわかりやすい名前に変更できます。 「*Write Hello World to output*」に変更します。
6.   **[パラメーター]** をクリックし、コマンドレットのパラメーターの値を指定します。  
一部のコマンドレットには複数のパラメーター セットがあり、使用するものを選択する必要があります。この場合、 **Write-output** 1 つだけパラメーター セットなので、いずれかを選択する必要はありません。 <br>
![Write-Output のプロパティ](media/automation-first-runbook-graphical/write-output-properties.png)
7.   **[InputObject]** パラメーターを選択します。 このパラメーターでは、出力ストリームに送信するテキストを指定します。
9.   **[データ ソース]** ドロップダウンで、**[PowerShell 式]** を選択します。
**[データ ソース]** ドロップダウンでは、パラメーター値の設定に使用するさまざまなソースが提供されます。 別のアクティビティ、Automation の資産、PowerShell 式などのソースからの出力を使用できます。 この場合は、*Hello World* というテキストを出力するだけです。 PowerShell 式を使用して文字列を指定すればそれが可能です。
10.   **式** ボックスに、入力 *"Hello World"* ] をクリックし、 **OK** 2 回クリックしてキャンバスに戻ります。<br>
![PowerShell 式](media/automation-first-runbook-graphical/expression-hello-world.png)
11.   クリックして、runbook を保存 **保存**します。<br>
![Runbook の保存](media/automation-first-runbook-graphical/runbook-edit-toolbar-save.png)

## 手順 3 - Runbook をテストする

Runbook を発行して運用環境で使用できるようにする前に、Runbook をテストして正常に動作することを確認します。 Runbook をテストするときは、**ドラフト** バージョンを実行し、その出力を対話形式で表示します。

2. クリックして **テスト ペイン** テスト ウィンドウを開きます。<br>
![Test pane](media/automation-first-runbook-graphical/runbook-edit-toolbar-test-pane.png)
2. **[開始]** をクリックしてテストを開始します。 有効なオプションはこれだけです。
3. A [runbook ジョブ](automation-runbook-execution) を作成し、ステータス ウィンドウに表示します。  
最初のジョブの状態は*キューに設定*であり、クラウドの Runbook ワーカーが使用できるようになるのを待っていることを示します。 移動し、 *開始*  、ワーカーが、ジョブを要求し、 *を実行している* 、runbook が実際に実行を開始するとき。
4. Runbook ジョブが完了すると、その出力が表示されます。この場合、表示されるはず *Hello World*します。<br>
![Hello World](media/automation-first-runbook-graphical/test-output-hello-world.png)
5. テスト ウィンドウを閉じてキャンバスに戻ります。


## 手順 4 - Runbook を発行して開始する

前の手順で作成した Runbook はまだドラフト モードです。 運用環境で実行できるようにするには、発行する必要があります。 Runbook を発行するときは、既存の発行済みバージョンをドラフト バージョンで上書きします。 この例では、Runbook を作成したばかりなので、発行済みバージョンはまだありません。

1. をクリックして **発行** 、runbook を発行し、 **[はい]** 入力を求められたらします。<br>
![Publish](media/automation-first-runbook-graphical/runbook-edit-toolbar-publish.png)
2. **[Runbook]** ウィンドウで左にスクロールして Runbook を表示すると、**[作成状態]** は **[発行済]** になっています。
3. 右にスクロールして戻り、**MyFirstRunbook** のウィンドウを表示します。  
上部のオプションでは、runbook を開始、または今後、時間に開始して、作成するようにスケジュールすることができる、 [webhook](automation-webhooks.md) 開始できる HTTP 呼び出しで。
4. Runbook を開始するだけためをクリックして **開始** し、 **はい** 入力を求められたらします。<br>
![Runbook の開始](media/automation-first-runbook-graphical/runbook-toolbar-start.png)
5. 作成した Runbook ジョブのジョブ ウィンドウが開かれます。 このウィンドウは閉じてもかまいませんが、ここではジョブの進行状況を確認できるように開いたままにします。
6.  ジョブの状態を示す **ジョブの概要** 、runbook をテストして確認した状態と一致します。<br>
![ジョブの概要](media/automation-first-runbook-graphical/job-pane-summary.png)
7.  Runbook の状態が *[完了]* になったら、**[出力]** をクリックします。 **出力** ウィンドウを開くし、表示、 *Hello World*します。<br>
![ジョブの概要](media/automation-first-runbook-graphical/job-pane-output.png)
8.  [出力] ウィンドウを閉じます。
9.  **[ストリーム]** をクリックして、Runbook ジョブのストリーム ウィンドウを開きます。だけを表示する必要があります *Hello World* 出力にストリームが、このストリームも表示できます他の詳細やエラーなどの runbook ジョブのかどうか、runbook がそれらに書き込みます。<br>
![ジョブの概要](media/automation-first-runbook-graphical/job-pane-streams.png)
9. ストリーム ウィンドウを閉じると、ジョブ ウィンドウは MyFirstRunbook ウィンドウに戻ります。
9.  **[ジョブ]** をクリックして、この Runbook のジョブ ウィンドウを開きます。この Runbook によって作成されたジョブの一覧が表示されます。1 つのジョブしか実行したので、ジョブ 1 回の一覧だけを表示する必要があります。<br>
![ジョブ](media/automation-first-runbook-graphical/runbook-control-jobs.png)
9. このジョブをクリックすると、Runbook を開始したときに表示されたものと同じジョブ ウィンドウが開きます。 これにより前に戻って、特定の Runbook に対して作成されたジョブの詳細を見ることができます。

## 手順 5 - Azure リソースを管理するための認証を追加する

Runbook をテストして発行しましたが、これまでのところ役に立つことは何もしていません。 ここでは、Runbook で Azure リソースを管理します。 認証で示されている資格情報を使用している場合を除きも行うことはできません、 [の前提条件](#prerequisites)します。 **Set-AzureAccount** コマンドレットでこれを行います。

1.  クリックして、グラフィカル エディターを開きます **編集** MyFirstRunbook ウィンドウでします。<br>
![Edit runbook](media/automation-first-runbook-graphical/runbook-toolbar-edit.png)
2.  **Write Hello World to output** はもう必要ないので、右クリックして **[削除]** を選択します。
8.  ライブラリ コントロールで、**[コマンドレット]**、**[Azure]** の順に展開します。
9.  追加 **Add-azureaccount** をキャンバスにします。<br>
![Add-AzureAccount](media/automation-first-runbook-graphical/add-azureaccount.png)
12.  **Add-AzureAccount** を選択し、構成ウィンドウで **[パラメーター]** をクリックします。
13.  **Add-AzureAccount** には複数のパラメーター セットがあるので、パラメーター値を指定する前に、1 つのパラメーター セットを選択する必要があります。 をクリックして **パラメーター設定** し、[、 **ユーザー** パラメーターのセットです。
14.  パラメーター セットを選択すると、アクティビティ パラメーター構成ウィンドウにパラメーターが表示されます。クリックして **資格情報**します。<br>
![Azure アカウントのパラメーターの追加](media/automation-first-runbook-graphical/add-azureaccount-parameters.png)
15.  このコマンドレットでは Automation アカウントの資格情報資産を使用するので、**[データ ソース]** で **[資格情報資産]** を選択します。
16.  起動し、Azure 環境内のバーチャル マシンを停止するアクセス権を持つ資格情報資産を選択します。<br>
![Azure アカウントのデータ ソースの追加](media/automation-first-runbook-graphical/credential-data-source.png)
17.  **[OK]** を 2 回クリックしてキャンバスに戻ります。


## 手順 6 - 仮想マシンを開始するアクティビティを追加する

次に、仮想マシンを開始するための **Start-AzureVM** アクティビティを追加します。 Azure サブスクリプション内の任意の仮想マシンを選択し、ここではその名前をコマンドレットにハードコーディングします。

1. **[コマンドレット]**、**[Azure]** の順に展開します。
2. **Start-AzureVM** をキャンバスに追加した後、それをクリックして **Add-AzureAccount** の下にドラッグします。
11.  **Add-AzureAccount** にポインターを合わせて、図形の下部に円を表示します。 円をクリックし、矢印を **Start-AzureVM** までドラッグします。  
作成した矢印は*リンク*です。Runbook が開始 **Add-azureaccount** し、実行 **Start-azurevm**します。<br>
![Start-AzureVM](media/automation-first-runbook-graphical/start-azurevm.png)
5. **Start-AzureVM** を選択します。 **[パラメーター]**、**[パラメーター セット]** の順にクリックし、**Start-AzureVM** のセットを表示します。 選択、 **ByName** パラメーターのセットです。 
**[Name]** と **[ServiceName]** の横に感嘆符が付いていることに注意してください。 これは、必須のパラメーターであることを示します。
7. **[Name]** を選択します。 **[データ ソース]** には **[定数値]** を使用し、開始する仮想マシンの名前を入力します。 **[OK]** をクリックします。
8. **[ServiceName]** を選択します。**[データ ソース]** には **[定数値]** を使用し、開始する仮想マシンの名前を入力します。クリックして **OK**します。<br>
![Start-AzureVM のパラメーター](media/automation-first-runbook-graphical/start-azurevm-params.png)
9. テスト ウィンドウをクリックして、Runbook をテストできるようにします。
10. **[開始]** をクリックしてテストを開始します。 完了したら、仮想マシンが開始されたことを確認します。


## 手順 7 - Runbook に入力パラメーターを追加する

この Runbook は現在は **Start-AzureVM** コマンドレットで指定した仮想マシンを開始しますが、Runbook を開始するときに仮想マシンを指定できるようにすればもっと便利です。 そこで次に、入力パラメーターを Runbook に追加して、その機能を提供します。

1. **MyFirstRunbook** ウィンドウで **[編集]** をクリックして、グラフィカル エディターを開きます。
2. をクリックして **入力と出力** し **入力の追加** 、Runbook の入力パラメーター ウィンドウを開きます。<br>
![Runbook の入力と出力](media/automation-first-runbook-graphical/runbook-edit-toolbar-inputoutput.png)
4. **[名前]** を「*VMName*」と指定します。 **[型]** は *[文字列]* のままにしますが、**[必須]** は *[はい]* に変更します。 **[OK]** をクリックします。
5. 呼ばれる 2 番目の必須入力パラメーターを作成 *VMServiceName* ] をクリックし、 **[ok]** を閉じる、 **の入力し、出力** ウィンドウです。<br>
![Runbook の入力パラメーター](media/automation-first-runbook-graphical/input-parameters.png)
6. **Start-AzureVM** アクティビティを選択し、**[パラメーター]** をクリックします。
7. 変更、 **データソース** の **名** に **Runbook の入力** し、[ **VMName**します。<br>
![Start-AzureVM の Name パラメーター](media/automation-first-runbook-graphical/vmname-property.png)
8. 変更、 **データソース** の **ServiceName** に **Runbook の入力** し、[ **VMServiceName**します。<br>
![Start-AzureVM のパラメーター](media/automation-first-runbook-graphical/start-azurevm-params-inputs.png)
9. Runbook を保存してテスト ウィンドウを開きます。 テストで使用される 2 つの入力変数の値を指定できることに注意してください。
11.  テスト ウィンドウを閉じます。
12.  **[発行]** をクリックして、新しいバージョンの Runbook を発行します。
13.  前の手順で開始した仮想マシンを停止します。
13.  **[開始]** をクリックして Runbook を開始します。入力、 **VMName** と **VMServiceName** を起動しようとして、仮想マシンのです。<br>
![Runbook の開始](media/automation-first-runbook-graphical/start-runbook-input-params.png)
14.  Runbook が完了したら、仮想マシンが開始されたことを確認します。

## 手順 8 - 条件付きリンクを作成する

次に、まだ開始されていない場合にのみ Runbook を開始するように Runbook を変更します。 そのためには、仮想マシンの現在の状態を含む Runbook に **Get-AzureVM** コマンドレットを追加します。 その後、現在の実行状態が停止の場合にのみ **Start-AzureVM** を実行する条件付きリンクを追加します。 Runbook が停止されていない場合は、メッセージを出力します。

1. グラフィカル エディターで **MyFirstRunbook** を開きます。
2. **Add-AzureAccount** と **Start-AzureVM** の間のリンクをクリックして *Delete* キーを押し、リンクを削除します。
3. ライブラリ コントロールで、**[コマンドレット]**、**[Azure]** の順に展開します。
4. **Get-AzureVM** をキャンバスに追加します。
5. **Add-AzureAccount** から **Get-AzureVM** へのリンクを作成します。別のリンクを作成する **Get-azurevm** に **Start-azurevm**します。<br>
![Get-AzureVM での Runbook](media/automation-first-runbook-graphical/get-azurevm.png)
6.  **Get-AzureVM** を選択して **[パラメーター]** をクリックします。 パラメーター セット *GetVMByServiceAndVMName* を選択します。
7.  **[Name]** の **[データ ソース]** を **[Runbook の入力]** に設定し、**VMName** を選択します。
7.  **[ServiceName]** の **[データ ソース]** を **[Runbook の入力]** に設定し、**VMServiceName** を選択します。
8.  **Get-AzureVM** と **Start-AzureVM** の間のリンクを選択します。
9.  構成ウィンドウで、**[条件の適用]** を **[True]** に変更します。 条件が真の場合にのみ対象のアクティビティが実行されることを示す破線にリンクが変わることに注意してください。
10.  **[条件式]** に「*$ActivityOutput['Get-AzureVM'].PowerState -eq "Stopped"*」と入力します。**Start-azurevm** のみ実行される仮想マシンが停止している場合。<br>
![リンク条件](media/automation-first-runbook-graphical/link-condition.png)
11.  ライブラリ コントロールで、**[コマンドレット]**、**[Microsoft.PowerShell.Utility]** の順に展開します。
12.  **Write-Output** をキャンバスに追加します。
13.  **Get-AzureVM** から **Write-Output** へのリンクを作成します。
14.  リンクを選択し、**[条件の適用]** を **[True]** に変更します。
14.  **[条件式]** に「*$ActivityOutput['Get-AzureVM'].PowerState -ne "Stopped"*」と入力します。**Write-output** 仮想マシンが停止していない場合にのみ実行ようになりました。<br>
![Write-Output での Runbook](media/automation-first-runbook-graphical/write-output-link.png)
15.  **Write-Output** を選択し、**[パラメーター]** をクリックします。
16.  **[InputObject]** の **[データ ソース]** を **[PowerShell 式]** に変更し、式に「*"$VMName.Name already started."*」と入力します。
17.  Runbook を保存してテスト ウィンドウを開きます。
18.  仮想マシンが停止している状態で Runbook を開始すると、仮想マシンが開始します。
19.  仮想マシンが開始している状態で Runbook を開始すると、仮想マシンが既に開始しているという出力が表示されます。


## 関連記事:

- [Azure Automation でのグラフィカル作成](automation-graphical-authoring-intro.md)
- [初めて PowerShell ワークフローの runbook](automation-first-runbook-textual.md)






