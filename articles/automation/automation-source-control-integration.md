<properties 
    pageTitle=" Azure Automation でのソース管理の統合 | Microsoft Azure"
    description="この記事では、Azure Automation での GitHub とのソース管理の統合について説明します。"
    services="automation"
    documentationCenter=""
    authors="SnehaGunda"
    manager="stevenka"
    editor="tysonn" />    
<tags 
    ms.service="automation"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="11/10/2015"
    ms.author="sngun" />

# Azure Automation でのソース管理の統合

ソース管理の統合により、Automation アカウントの Runbook を GitHub のソース管理リポジトリに関連付けることができます。 ソース管理により、チームとの共同作業、変更の追跡、Runbook の以前のバージョンへのロールバックを簡単に実行できるようになります。 たとえば、開発、テスト、または運用の Automation アカウントに異なるブランチをソース管理で同期できるようになり、開発環境内でテストされたコードを運用の Automation アカウントに昇格することが容易になります。

ソース管理することで、Azure Automation からソース管理にコードをプッシュしたり、ソース管理から Azure Automation に Runbook をプルしたりできます。 この記事では、Azure Automation 環境内でソース管理を設定する方法について説明します。 まず、Azure Automation を GitHub リポジトリにアクセスするよう構成し、ソース管理の統合を使用することで実行できるさまざまな操作について、順を追って説明します。 


>[AZURE.NOTE] ソース管理のプルとプッシュをサポートしています [PowerShell ワークフロー Runbook](automation-runbook-types.md#powershell-workflow-runbooks) だけでなく [PowerShell Runbook](automation-runbook-types.md#powershell-runbooks), 、[グラフィカル runbook](automation-runbook-types.md#graphical-runbooks) はまだサポートされていません。<br><br>


Automation アカウントのソース管理を構成するには、2 つの簡単な手順が必要です。ただし、既に GitHub アカウントを持っている場合、必要な手順は 1 つだけです。 次に例を示します。
## 手順 1. GitHub リポジトリを作成する

GitHub アカウントと、Azure Automation にリンクするリポジトリが既にある場合は、既存のアカウントにログインし、次の手順 2. から開始してください。 それ以外の場合に移動 [GitHub](https://github.com/), 、新しいアカウントにサインアップし、 [新しいリポジトリを作成する](https://help.github.com/articles/create-a-repo/)です。


## 手順 2. Azure Automation でソース管理を設定する

1. Azure プレビュー ポータルの [オートメーション アカウント] ブレードからクリックして **ソース管理を設定します。** 
 
    ![ソース管理の設定](media/automation-source-control-integration/automation_01_SetUpSourceControl.png)

2.  **ソース管理** ブレードが開き、GitHub アカウントの詳細を構成することができます。 構成するパラメーターを次に示します。  

  	|**パラメーター**            |**説明** |
  	|:---|:---| 
  	|ソースの選択   | ソースを選択します。 現在のところ、のみ **GitHub** はサポートされています。 |
  	|承認 | クリックして、 **Authorize** GitHub リポジトリの Azure Automation のアクセスを許可する] ボタンをクリックします。 既に別のウィンドウで GitHub アカウントにログインしている場合は、そのアカウントの資格情報が使用されます。 ブレードで [GitHub ユーザー名を表示する承認が成功すると **承認プロパティ**します。 |
  	|リポジトリの選択 | 使用可能なリポジトリの一覧から GitHub リポジトリを選択します。 |
  	|ブランチの選択 | 使用可能なブランチの一覧からブランチを選択します。 のみ、 **マスター** かどうかは、すべての分岐を作成していない分岐を表示します。 |
  	|Runbook フォルダーのパス | Runbook フォルダーのパスは、コードをプッシュまたはプルする GitHub リポジトリのパスを指定します。 形式で入力する必要があります **/フォルダー/subfoldername**します。 Runbook フォルダーのパスにある Runbook のみが Automation アカウントと同期されます。 Runbook フォルダーのパスのサブフォルダー内の Runbook は **いない** 同期されます。 使用 **/** リポジトリの下のすべての runbook を同期します。 |


3. たとえば、名前の付いたリポジトリがあれば **PowerShellScripts** という名前のフォルダーを格納している **して**, 、という名前のフォルダーを含む **サブフォルダー**します。 次の文字列を使用して、各フォルダー レベルで同期できます。

    1. Runbook を同期する **リポジトリ**, 、runbook フォルダーのパスが */*
    2. Runbook を同期する **して**, 、runbook フォルダーのパスが */RootFolder*
    3. Runbook を同期する **サブフォルダー**, 、runbook フォルダーのパスが */して/サブフォルダー*します。
  

4. 表示されるパラメーターを構成した後、 **ソース管理の設定ブレードです。**  
 
    ![[構成] ブレード](media/automation-source-control-integration/automation_02_SourceControlConfigure.png)


5. [OK] をクリックすると、ソース管理の統合は、お使いの Automation アカウント用に構成されるため、GitHub 情報を使用して更新する必要があります。 この部分をクリックすると、ソース管理の同期ジョブの履歴がすべて表示されます。  

    ![リポジトリの値](media/automation-source-control-integration/automation_03_RepoValues.png)

6. ソース管理の設定後、次の Automation リソースが Automation アカウントに作成されます。  
 2 つ [変数アセット](automation-variables.md) が作成されます。  
      
    * 変数 **Microsoft.Azure.Automation.SourceControl.Connection** 次のように、接続文字列の値が含まれています。  

  	|**パラメーター**            |**値** |
  	|:---|:---|
  	| 名前  | Microsoft.Azure.Automation.SourceControl.Connection |
  	| 型 | String |
  	| 値  | {「分岐」: \ <*分岐名*>、"RunbookFolderPath": \ <*Runbook フォルダーのパス*>、"プロバイダーの種類": \ <*GitHub の場合、値 1 を持つ*>、「リポジトリ」: \ <*、リポジトリの名前*>、"Username": \ <*、GitHub ユーザー名*>} |  <br>


    * The variable **Microsoft.Azure.Automation.SourceControl.OauthToken**, contains the secure encrypted value of your OAuthToken.  

  	|**Parameter**            |**Value** |
  	|:---|:---|
  	| Name  | Microsoft.Azure.Automation.SourceControl.OauthToken |
  	| Type | Unknown(Encrypted) |
  	| Value | <*Encrypted OAuthToken*> |  

    ![Variables](media/automation-source-control-integration/automation_04_Variables.png)  

    * **Automation Source Control** is added as an authorized application to your GitHub account. To view the application: From your GitHub home page, navigate to your **profile** > **Settings** > **Applications**. This application allows Azure Automation to sync your GitHub repository to an Automation account.  

    ![Git Application](media/automation-source-control-integration/automation_05_GitApplication.png)


## Automation でのソース管理の使用


### Azure Automation からソース管理に Runbook をチェックインする

Runbook をチェックインすると、Azure Automation で Runbook に対して行った変更をソース管理リポジトリにプッシュできます。 Runbook のチェックイン手順は次のとおりです。

1. オートメーション アカウントから [新しいテキストの runbook を作成する](automation-first-runbook-textual.md), 、または [、既存のテキストの runbook を編集](automation-edit-textual-runbook.md)します。 この Runbook は、PowerShell ワークフローまたは PowerShell スクリプト Runbook のどちらでもかまいません。  

2. Runbook を編集すると、保存し、クリックして **チェックイン** から、 **編集** ブレードです。  

    ![[チェックイン] ボタン](media/automation-source-control-integration/automation_06_CheckinButton.png)


     >[AZURE.NOTE] Azure Automation からのチェックインは、ソース管理に現在存在しているコードで上書きされます。 チェックインする Git と同等のコマンドライン命令は **git 追加 + git コミットと git プッシュ**  

3. クリックすると、 **チェックイン**, 、確認メッセージが表示されますを続行するには、[はい] をクリックします。  

    ![チェックイン メッセージ](media/automation-source-control-integration/automation_07_CheckinMessage.png)

4. チェックインには、ソース コントロールの runbook を開始します。 **同期 MicrosoftAzureAutomationAccountToGitHubV1**します。 この Runbook は、GitHub に接続し、変更を Azure Automation からリポジトリにプッシュします。 チェックインのジョブ履歴を表示するに戻り、 **ソース管理の統合** タブし、リポジトリ同期ブレードを開く] をクリックします。 このブレードには、すべてのソース管理ジョブが表示されます。  表示するジョブを選択し、クリックして詳細を表示します。  

    ![チェックイン Runbook](media/automation-source-control-integration/automation_08_CheckinRunbook.png)

    >[AZURE.NOTE] ソース コントロールの runbook は、特別なのオートメーション runbook を表示または編集することはできません。 これらは Runbook の一覧に表示されませんが、ジョブの一覧には同期ジョブが表示されます。
 
5. 変更された Runbook の名前は、入力パラメーターとしてチェックイン Runbook に送信されます。 実行できます [ジョブの詳細を表示](automation-runbook-execution.md#viewing-job-status-using-the-azure-management-portal) で runbook を展開して **リポジトリ同期** ブレードです。  

    ![チェックイン入力](media/automation-source-control-integration/automation_09_CheckinInput.png)

6. ジョブが完了したら GitHub リポジトリを更新して、変更を表示します。  リポジトリにコミット メッセージをコミットにする必要がありますがあります: **Updated *Runbook 名* Azure Automation でします。**  



### ソース管理から Azure Automation に Runbook を同期する 

[リポジトリの同期] ブレードの [同期] ボタンを使用すると、リポジトリの Runbook フォルダーのパスにあるすべての Runbook を Automation アカウントにプルできます。 同一リポジトリを複数の Automation アカウントに同期できます。 Runbook の同期手順は次のとおりです。

1. ソース管理を設定するため、オートメーション アカウントから開く、 **ソース管理の統合/リポジトリを同期ブレード** ] をクリック **同期** 確認のメッセージで促さ] をクリックし、 **[はい]** を続行します。  

    ![同期ボタン](media/automation-source-control-integration/automation_10_SyncButtonwithMessage.png)

2. 同期が runbook を開始します。 **同期 MicrosoftAzureAutomationAccountFromGitHubV1**します。 この Runbook は、GitHub に接続し、変更をリポジトリから Azure Automation にプルします。 新しいジョブが表示、 **リポジトリ同期** このアクションのブレードです。 同期ジョブの詳細を表示するには、[ジョブの詳細] ブレードをクリックして、開きます。  
 
    ![Runbook の同期](media/automation-source-control-integration/automation_11_SyncRunbook.png)

 
    >[AZURE.NOTE] ソース管理からの同期は、オートメーション アカウントに現在存在する runbook の下書きバージョンを上書き **すべて** に現在存在する runbook のソース管理します。 同期する Git と同等のコマンドライン命令は **git プル**


## ソース管理の問題のトラブルシューティング

チェックイン ジョブまたは同期ジョブにエラーがある場合は、ジョブが中断状態になるため、[ジョブ] ブレードでエラーに関する詳細情報を表示できます。   **のすべてのログ** 部分がそのジョブに関連付けられているすべての PowerShell ストリームを表示します。  これにより、チェックインまたは同期の問題を解決するために必要な詳細情報が提供されます。 Runbook での同期またはチェックイン中に発生した一連のアクションも表示されます。  

![AllLogs イメージ](media/automation-source-control-integration/automation_13_AllLogs.png)

## ソース管理の切断

切断するには、GitHub アカウントから、リポジトリ同期ブレードを開きクリックして **切断**します。 ソース管理を切断すると、それまでに同期された Runbook はそのまま Automation アカウントに保持されますが、[リポジトリの同期] ブレードは有効になりません。  

  ![[切断] ボタン](media/automation-source-control-integration/automation_12_Disconnect.png)



## 次のステップ

ソース管理の統合の詳細については、次のリソースをご覧ください。  
- [Azure Automation: Source Control Integration in Azure Automation](https://azure.microsoft.com/blog/azure-automation-source-control-13/)  
- [好みのソース管理システムの投票](https://www.surveymonkey.com/r/?sm=2dVjdcrCPFdT0dFFI8nUdQ%3d%3d)  
- [Azure Automation: Integrating Runbook Source Control using Visual Studio Online](https://azure.microsoft.com/blog/azure-automation-integrating-runbook-source-control-using-visual-studio-online/)  


