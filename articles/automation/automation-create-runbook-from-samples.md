<properties
    pageTitle="Get Started with Azure Automation | Microsoft Azure"
    description="Azure でオートメーション ジョブをインポートして実行する方法について説明します。"
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
    ms.topic="hero-article" 
    ms.date="12/08/2015"
    ms.author="bwren"/>


# Azure オートメーションの使用

## Azure Automation とは

ユーザーは Microsoft Azure オートメーションを使用すると、クラウド環境で一般的に実行される、手動で実行時間が長く、エラーが起こりやすく、頻繁に繰り返されるタスクを自動化する手段を入手できます。 Runbook を使用して、Azure 環境内でリソースを作成、監視、管理、およびデプロイすることができます。Runbook は、Windows PowerShell のワークフローに基づいています。 この記事では、チュートリアルを通じて、簡単なサンプルの Runbook を実行します。 次に、サービスのより高度な機能を探るためのリソースを紹介します。

## チュートリアル
このチュートリアルでは、オートメーション アカウントを作成し、Azure Automation にサンプルの「Hello World」Runbook をインポートし、その Runbook を実行して、出力を表示する手順について説明します。

このチュートリアルを完了するには、Azure サブスクリプションが必要です。 場合は 1 つはまだがあるない、 [MSDN サブスクライバーの特典をアクティブ化](../pricing/member-offers/msdn-benefits-details/) または [無料評価版にサインアップ](../pricing/free-trial.md)</a>します。

[AZURE.INCLUDE [automation-note-authentication](../../includes/automation-note-authentication.md)]

## ビデオ チュートリアル

以下は、このチュートリアルのガイドです。

[AZURE.VIDEO get-started-with-azure-automation]

## <a name="automationaccount"></a>オートメーション アカウントの作成

オートメーション アカウントは、Azure Automation リソースのコンテナーで、 環境の分離や、ワークフローの詳細な整理のための方法を提供します。 詳細については、次を参照してください。 [オートメーション アカウント](http://aka.ms/runbookauthor/azure/automationaccounts) 自動化ライブラリにします。  Automation アカウントを既に作成している場合は、次の手順を省略できます。

1.  ログイン、 [Azure ポータル](http://manage.windowsazure.com)します。

2.  Azure ポータルで、クリックして **オートメーション アカウントの作成**します。  

    ![アカウントの作成](./media/automation-create-runbook-from-samples/automation_01_CreateAccount.png)

3.   **新しいオートメーション アカウントの追加** ページで、名前を入力し、アカウントのリージョンを選択します。 リージョンでは、アカウントの Automation リソースが格納される場所を指定します。 これは、アカウントの機能には影響しませんが、アカウントのリージョンがご使用のその他の Azure リソースが格納された場所に近い場合、Runbook をより素早く実行できます。 準備ができたら、チェック マークをクリックします。

    ![新しいアカウントの追加](./media/automation-create-runbook-from-samples/automation_02_addnewautoacct.png)

## <a name="importrunbook"></a>Runbook ギャラリーからの Runbook のインポート

 [Runbook ギャラリー](http://aka.ms/runbookgallery) 、Azure Automation アカウントに直接インポートできるサンプルの runbook を含むその他の Azure Automation および PowerShell ユーザーの作業を活用することができます。 この手順では、ギャラリーを使用して「Hello World」サンプル Runbook をインポートします。

4.   **オートメーション** ] ページで、先ほど作成した新しいアカウント] をクリックします。

    ![新しいアカウント](./media/automation-create-runbook-from-samples/automation_03_NewAutoAcct.png)

5.  Click **RUNBOOKS**.

    ![[Runbook] タブ](./media/automation-create-runbook-from-samples/automation_04_RunbooksTab.png)

6.  Click **New** > **Runbook** > **From Gallery**.

    ![Runbook ギャラリー](./media/automation-create-runbook-from-samples/automation_05_ImportGallery.png)

7.  Select the **Tutorial** category, and then **Hello World for Azure Automation**. 右矢印ボタンをクリックします。

    ![Runbook のインポート](./media/automation-create-runbook-from-samples/automation_06_ImportRunbook.png)

8.  Runbook の内容を確認し、右矢印ボタンをクリックします。

    ![Runbook 定義](./media/automation-create-runbook-from-samples/automation_07_RunbookDefinition.png)

8.  Runbook の詳細を確認し、チェック マーク ボタンをクリックします。

    ![Runboon の詳細](./media/automation-create-runbook-from-samples/automation_08_RunbookDetails.png)

## <a name="publishrunbook"></a>Runbook の発行

Runbook は、まずドラフト モードでインポートされます。 つまりこれは、承認前に実行可能な新しいバージョンとして Runbook で作業を継続できることを意味します。 このサンプルの Runbook は追加の構成を必要としないため、そのまますぐに発行できます。  詳細については、次を参照してください。 [Runbook を公開する](http://aka.ms/runbookauthor/azure/publishrunbook)です。

9.  When the runbook has finished importing, click **Write-HelloWorld**.

    ![インポートした Runbook](./media/automation-create-runbook-from-samples/automation_07_ImportedRunbook.png)

9.  Click **AUTHOR**, and then click **DRAFT**.  

    ドラフト モードでは、Runbook の内容を変更できます。 この Runbook に対して、どのような変更も加える必要はありません。

    ![作成者のドラフト](./media/automation-create-runbook-from-samples/automation_08_AuthorDraft.png)  

10. Click **PUBLISH** to promote the runbook, marking it ready for production use.

    ![Publish](./media/automation-create-runbook-from-samples/automation_085_Publish.png)

11. 確認のメッセージが表示されたら、クリックして **はい**します。

    ![保存して発行するためのメッセージ](./media/automation-create-runbook-from-samples/automation_09_SavePubPrompt.png)

## <a name="startrunbook"></a>Runbook の開始

Runbook をインポートし、発行したら、それを実行して、出力を調べます。  詳細については、次を参照してください。 [Runbook を開始する](http://aka.ms/runbookauthor/azure/startrunbook) と [Runbook の出力とメッセージ](http://aka.ms/runbookauthor/azure/runbookoutput)します。

12. With the **Write-HelloWorld** runbook open, click **START**.

    ![公開先](./media/automation-create-runbook-from-samples/automation_10_PublishStart.png)

13. On the **Specify the runbook parameter values** page, type a **Name** that will be used as an input parameter for the Write-HelloWorld.ps1 script, and then click the check mark.

    ![Runbook のパラメーター](./media/automation-create-runbook-from-samples/automation_11_RunbookParams.png)

14. をクリックして **ジョブ** で開始した runbook ジョブの状態を確認してからのタイムスタンプ、 **ジョブの開始** ジョブの概要を表示する列。

    ![Runbook のステータス](./media/automation-create-runbook-from-samples/automation_12_RunbookStatus.png)

15.  **概要** 、概要を表示するページの入力パラメーターと、ジョブの出力です。

    ![Runbook の概要](./media/automation-create-runbook-from-samples/automation_13_RunbookSummary_callouts.png)

ご利用ありがとうございます。 チュートリアルが完了しました。

## <a name="nextsteps"></a>次のステップ
1. The simple runbook in this tutorial *does not manage Azure services*. ほとんどの runbook を使用して、 [Azure コマンドレット](http://msdn.microsoft.com/library/jj156055.aspx) ためには、Azure サブスクリプションへの認証を必要とします。 」の手順に従って [Azure Runbook での管理の構成](http://aka.ms/azureautomationauthentication) にこれらのコマンドレットを使用する Azure サブスクリプションを構成します。  
2. 参照してください、 [リソース](#resources) Azure Automation の機能の詳細について以下に示されました。
3. 購読、 [Azure Automation ブログ](http://azure.microsoft.com/blog/tag/azure-automation) 、Azure Automation チームから最新の最新の状態にします。

## <a name="resources"></a>リソース

その他にも、Azure Automation や、独自の Runbook の作成方法の詳細を確認できる多数のリソースが用意されています。

- [Azure Automation ライブラリ](http://go.microsoft.com/fwlink/p/?LinkId=392860) 構成や、独自の runbook を作成するため、Azure Automation の管理について詳しく説明します。
- [Azure PowerShell コマンドレット](http://msdn.microsoft.com/library/jj156055.aspx) Windows PowerShell を使用した Azure の操作の自動化に関する情報を提供します。  Runbook ではこれらのコマンドレットを使用して Azure リソースを操作します。
- [Azure Automation ブログ](http://azure.microsoft.com/blog/tag/azure-automation) Microsoft から Azure Automation に関する最新の情報を提供します。
- [Automation フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=390561) マイクロソフトや Automation コミュニティで対応する Azure Automation に関する質問を投稿することができます。


## サンプル Runbook とユーティリティ Runbook

マイクロソフトおよび Azure Automation コミュニティが提供するサンプル Runbook を使うと、独自のソリューションを作成できます。また、ユーティリティ Runbook は大きめのオートメーション タスクの構成ブロックとして使用できます。 これらの runbook からをダウンロードすることができますか、 [スクリプト センター](http://azure.microsoft.com/documentation/scripts/) を使用して Azure Automation に直接インポートすることも、 [Runbook ギャラリー](http://aka.ms/runbookgallery)します。


## フィードバック

**フィードバックの送信**  Azure Automation の Runbook ソリューションや統合モジュールを探索している場合は、スクリプト センターにスクリプトの要求を投稿することができます。 Azure Automation に関するフィードバックや機能の要求がある場合に投稿して [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback)します。 よろしくお願いいたします。


