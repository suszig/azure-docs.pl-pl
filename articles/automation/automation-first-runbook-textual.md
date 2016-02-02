<properties
    pageTitle="Azure Automation での最初の PowerShell Workflow Runbook | Microsoft Azure"
    description="PowerShell Workflow を使用して簡単なテキスト形式の Runbook を作成、テスト、および発行する手順を説明するチュートリアルです。Azure リソースに対する認証、入力パラメーターなど、複数の概念について説明します。"
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



# 最初の PowerShell Workflow Runbook

> [AZURE.SELECTOR]
- [Graphical](automation-first-runbook-graphical.md)
- [PowerShell Workflow](automation-first-runbook-textual.md)


このチュートリアルの作成を通じて、 [PowerShell ワークフロー runbook](automation-runbook-types.md#powerShell-workflow-runbooks) Azure Automation でします。 簡単な Runbook を作成、テスト、発行しながら、Runbook ジョブの状態を追跡する方法を説明します。 その後、実際に Azure リソースを管理するように Runbook を変更し、ここでは Azure 仮想マシンを開始します。 そして、Runbook のパラメーターを追加することによって、Runbook をいっそう堅牢にします。

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
![Runbook の制御](media/automation-first-runbook-textual/runbooks-control.png)
2. **[Runbook の追加]** ボタンをクリックし、次に **[新しい Runbook の作成]** をクリックして、新しい Runbook を作成します
3. Runbook に *MyFirstRunbook-Workflow* という名前を付けます。
4. ここを作成しようとしている場合は、 [PowerShell ワークフロー runbook](automation-runbook-types.md#powerShell-workflow-runbooks) ので選択 **Powershell ワークフロー** の **Runbook の種類**します。<br>
![新しい Runbook](media/automation-first-runbook-textual/new-runbook.png)
5. **[作成]** をクリックして Runbook を作成し、テキスト エディターを開きます。

## 手順 2 - コードを Runbook に追加する

Runbook に直接コードを入力するか、ライブラリ コントロールからコマンドレット、Runbook、資産を選択し、関連するパラメーターを使って Runbook に追加できます。 このチュートリアルでは、Runbook に直接コードを入力します。

1. この runbook は空にのみ必要です。 *ワークフロー* キーワード、この runbook とワークフロー全体を表す、中かっこの名前。 <br>
![Runbook の制御](media/automation-first-runbook-textual/empty-runbook.png)
2. 型 *Write-output は、「Hello World.」* 中かっこの間です。 <br>
![Hello world](media/automation-first-runbook-textual/hello-world.png)
3.   クリックして、runbook を保存 **保存**します。<br>
![Runbook の保存](media/automation-first-runbook-textual/runbook-edit-toolbar-save.png)

## 手順 3 - Runbook をテストする

Runbook を発行して運用環境で使用できるようにする前に、Runbook をテストして正常に動作することを確認します。 Runbook をテストするときは、**ドラフト** バージョンを実行し、その出力を対話形式で表示します。

2. クリックして **テスト ペイン** テスト ウィンドウを開きます。<br>
![Test pane](media/automation-first-runbook-textual/runbook-edit-toolbar-test-pane.png)
2. **[開始]** をクリックしてテストを開始します。 有効なオプションはこれだけです。
3. A [runbook ジョブ](automation-runbook-execution) が作成され、状態が表示されます。  
最初のジョブの状態は*キューに設定*であり、クラウドの Runbook ワーカーが使用できるようになるのを待っていることを示します。 移動し、 *開始*  、ワーカーが、ジョブを要求し、 *を実行している* 、runbook が実際に実行を開始するとき。
4. Runbook ジョブが完了すると、その出力が表示されます。この場合、表示されるはず *Hello World*します。<br>
![Hello World](media/automation-first-runbook-textual/test-output-hello-world.png)
5. テスト ウィンドウを閉じてキャンバスに戻ります。

## 手順 4 - Runbook を発行して開始する

前の手順で作成した Runbook はまだドラフト モードです。 運用環境で実行できるようにするには、発行する必要があります。 Runbook を発行するときは、既存の発行済みバージョンをドラフト バージョンで上書きします。 この例では、Runbook を作成したばかりなので、発行済みバージョンはまだありません。

1. をクリックして **発行** 、runbook を発行し、 **[はい]** 入力を求められたらします。<br>
![Publish](media/automation-first-runbook-textual/runbook-edit-toolbar-publish.png)
2. **[Runbook]** ウィンドウで左にスクロールして Runbook を表示すると、**[作成状態]** は **[発行済]** になっています。
3. 右にスクロールして戻り、**MyFirstRunbook-Workflowl** のウィンドウを表示します。  
上部のオプションでは、runbook を開始、または今後、時間に開始して、作成するようにスケジュールすることができる、 [webhook](automation-webhooks.md) 開始できる HTTP 呼び出しで。
4. Runbook を開始するだけためをクリックして **開始** し、 **はい** 入力を求められたらします。<br>
![Runbook の開始](media/automation-first-runbook-textual/runbook-toolbar-start.png)
5. 作成した Runbook ジョブのジョブ ウィンドウが開かれます。 このウィンドウは閉じてもかまいませんが、ここではジョブの進行状況を確認できるように開いたままにします。
6.  ジョブの状態を示す **ジョブの概要** 、runbook をテストして確認した状態と一致します。<br>
![ジョブの概要](media/automation-first-runbook-textual/job-pane-summary.png)
7.  Runbook の状態が *[完了]* になったら、**[出力]** をクリックします。[出力] ペインを開くし、表示、 *Hello World*します。<br>
![ジョブの概要](media/automation-first-runbook-textual/job-pane-output.png)
8.  [出力] ウィンドウを閉じます。
9.  **[ストリーム]** をクリックして、Runbook ジョブのストリーム ウィンドウを開きます。だけを表示する必要があります *Hello World* 出力にストリームが、このストリームも表示できます他の詳細やエラーなどの runbook ジョブのかどうか、runbook がそれらに書き込みます。<br>
![ジョブの概要](media/automation-first-runbook-textual/job-pane-streams.png)
9. ストリーム ウィンドウを閉じると、ジョブ ウィンドウは MyFirstRunbook ウィンドウに戻ります。
9.  **[ジョブ]** をクリックして、この Runbook のジョブ ウィンドウを開きます。この Runbook によって作成されたジョブの一覧が表示されます。1 つのジョブしか実行したので、ジョブ 1 回の一覧だけを表示する必要があります。<br>
![ジョブ](media/automation-first-runbook-textual/runbook-control-jobs.png)
9. このジョブをクリックすると、Runbook を開始したときに表示されたものと同じジョブ ウィンドウが開きます。 これにより前に戻って、特定の Runbook に対して作成されたジョブの詳細を見ることができます。

## 手順 5 - Azure リソースを管理するための認証を追加する

Runbook をテストして発行しましたが、これまでのところ役に立つことは何もしていません。 ここでは、Runbook で Azure リソースを管理します。 認証で示されている資格情報を使用している場合を除きも行うことはできません、 [の前提条件](#prerequisites)します。 **Add-AzureAccount** コマンドレットでこれを行います。

1.  クリックして、テキスト エディターを開きます **編集** ワークフロー MyFirstRunbook ウィンドウにします。<br>
![Edit runbook](media/automation-first-runbook-textual/runbook-toolbar-edit.png)
2.  **Write-Output** の行は不要になったので削除します。
3.  中かっこ内の空白行にカーソルを置きます。
3.  ライブラリ コントロールで、**[資産]**、**[資格情報]** の順に展開します。
4.  資格情報を右クリックし、**[キャンバスに追加]** をクリックします。 これにより、資格情報に対する **Get-AutomationPSCredential** アクティビティが追加されます。
5.  **Get-AutomationPSCredential** の前に「*$Credential =*」と入力して、資格情報を変数に割り当てます。
3.  次の行に入力 *Add-azureaccount-資格情報を $Credential*します。 <br>
![認証](media/automation-first-runbook-textual/authentication.png)
3. **テスト ウィンドウ**をクリックして、Runbook をテストできるようにします。
10. **[開始]** をクリックしてテストを開始します。完了すると、次のような出力により、資格情報内のユーザーに関する情報が返されます。これは、資格情報が有効であることを確認します。<br>
![認証](media/automation-first-runbook-textual/authentication-test.png)

## 手順 6 - 仮想マシンを開始するコードを追加する

これで、Runbook で Azure サブスクリプションに対する認証が行われ、リソースを管理できるようになります。 仮想マシンを起動するコマンドを追加します。 Azure サブスクリプション内の任意の仮想マシンを選択し、ここではその名前をコマンドレットにハードコーディングします。


1. 後 *Add-azureaccount*, 、型 *Start-azurevm-名 'VMName' ServiceName 'VMServiceName'* の仮想マシンを起動するサービス名と名前を提供します。 <br>
![認証](media/automation-first-runbook-textual/start-azurevm.png)
9. Runbook を保存し、**テスト ウィンドウ**をクリックしてテストできるようにします。
10. **[開始]** をクリックしてテストを開始します。 完了したら、仮想マシンが開始されたことを確認します。


## 手順 7 - Runbook に入力パラメーターを追加する

この Runbook では、Runbook にハードコードした仮想マシンを開始しますが、Runbook を開始するときに仮想マシンを指定できるようにすればもっと便利です。 そこで次に、入力パラメーターを Runbook に追加して、その機能を提供します。

1. パラメーターを追加 *VMName* と *VMServiceName* runbook にこれらの変数を使用して、 **Start-azurevm** コマンドレットは、次の図に示すようにします。 <br>
![認証](media/automation-first-runbook-textual/params.png)
9. Runbook を保存してテスト ウィンドウを開きます。 テストで使用される 2 つの入力変数の値を指定できることに注意してください。
11.  テスト ウィンドウを閉じます。
12.  **[発行]** をクリックして、新しいバージョンの Runbook を発行します。
13.  前の手順で開始した仮想マシンを停止します。
13.  **[開始]** をクリックして Runbook を開始します。入力、 **VMName** と **VMServiceName** を起動しようとして、仮想マシンのです。<br>
![Runbook の開始](media/automation-first-runbook-textual/start-runbook-input-params.png)

14.  Runbook が完了したら、仮想マシンが開始されたことを確認します。


## 関連記事:

- [初めてのグラフィカルな runbook](automation-first-runbook-graphical.md)




