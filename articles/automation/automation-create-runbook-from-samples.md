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

このチュートリアルを完了するには、Azure サブスクリプションが必要です。場合は 1 つはまだがあるない、 [MSDN サブスクライバーの特典をアクティブ化](../pricing/member-offers/msdn-benefits-details/) または [無料評価版にサインアップ](../pricing/free-trial.md)</a>します。

[AZURE.INCLUDE [automation-note-authentication](../../includes/automation-note-authentication.md)]

## ビデオ チュートリアル

以下は、このチュートリアルのガイドです。

[AZURE.VIDEO get-started-with-azure-automation]

## <a name="automationaccount"></a>オートメーション アカウントを作成します。

オートメーション アカウントは、Azure Automation リソースのコンテナーで、 環境の分離や、ワークフローの詳細な整理のための方法を提供します。 詳細については、次を参照してください。 [オートメーション アカウント](http://aka.ms/runbookauthor/azure/automationaccounts) 自動化ライブラリにします。 オートメーション アカウントを既に作成している場合は、次の手順を省略できます。

1.  ログイン、 [Azure ポータル](http://manage.windowsazure.com)します。

2.  Azure ポータルで、**[オートメーション アカウントの作成]** をクリックします。

    ![アカウントの作成](./media/automation-create-runbook-from-samples/automation_01_CreateAccount.png)

3.  **[新しいオートメーション アカウントの追加]** ページで、名前を入力し、アカウントのリージョンを選択します。 リージョンでは、アカウントの Automation リソースが格納される場所を指定します。 これは、アカウントの機能には影響しませんが、アカウントのリージョンがご使用のその他の Azure リソースが格納された場所に近い場合、Runbook をより素早く実行できます。 準備ができたら、チェック マークをクリックします。

    ![新しいアカウントの追加](./media/automation-create-runbook-from-samples/automation_02_addnewautoacct.png)

## <a name="importrunbook"></a>Runbook ギャラリーから runbook をインポートします。

[Runbook ギャラリー](http://aka.ms/runbookgallery) 、Azure Automation アカウントに直接インポートできるサンプルの runbook を含むその他の Azure Automation および PowerShell ユーザーの作業を活用することができます。 この手順では、ギャラリーを使用して「Hello World」サンプル Runbook をインポートします。

4.  **[オートメーション]** ページで、作成した新しいアカウントをクリックします。

    ![新しいアカウント](./media/automation-create-runbook-from-samples/automation_03_NewAutoAcct.png)

5.  **[Runbook]** をクリックします。

    ![[Runbook] タブ](./media/automation-create-runbook-from-samples/automation_04_RunbooksTab.png)

6.  **[新規]** > **[Runbook]** > **[ギャラリーから]** の順にクリックします。

    ![Runbook ギャラリー](./media/automation-create-runbook-from-samples/automation_05_ImportGallery.png)

7.  **[チュートリアル]** カテゴリを選択し、**[Hello World for Azure Automation]** を選択します。 右矢印ボタンをクリックします。

    ![Runbook のインポート](./media/automation-create-runbook-from-samples/automation_06_ImportRunbook.png)

8.  Runbook の内容を確認し、右矢印ボタンをクリックします。

    ![Runbook 定義](./media/automation-create-runbook-from-samples/automation_07_RunbookDefinition.png)

8.  Runbook の詳細を確認し、チェック マーク ボタンをクリックします。

    ![Runboon の詳細](./media/automation-create-runbook-from-samples/automation_08_RunbookDetails.png)

## <a name="publishrunbook"></a>Runbook を発行します。

Runbook は、まずドラフト モードでインポートされます。 つまりこれは、承認前に実行可能な新しいバージョンとして Runbook で作業を継続できることを意味します。 このサンプルの Runbook は追加の構成を必要としないため、そのまますぐに発行できます。 詳細については、次を参照してください。 [Runbook を公開する](http://aka.ms/runbookauthor/azure/publishrunbook)します。

9.  Runbook のインポートが完了したら、**[Write-HelloWorld]** をクリックします。

    ![インポートした Runbook](./media/automation-create-runbook-from-samples/automation_07_ImportedRunbook.png)

9.  **[作成者]** をクリックし、**[ドラフト]** をクリックします。

    ドラフト モードでは、Runbook の内容を変更できます。 この Runbook に対して、どのような変更も加える必要はありません。

    ![作成者のドラフト](./media/automation-create-runbook-from-samples/automation_08_AuthorDraft.png)

10. **[発行]** をクリックして Runbook を昇格し、運用環境で使用する準備をします。

    ![Publish](./media/automation-create-runbook-from-samples/automation_085_Publish.png)

11. 確認を求められたら、**[はい]** をクリックします。

    ![保存して発行するためのメッセージ](./media/automation-create-runbook-from-samples/automation_09_SavePubPrompt.png)

## <a name="startrunbook"></a>Runbook を開始します。

Runbook をインポートし、発行したら、それを実行して、出力を調べます。 詳細については、次を参照してください。 [Runbook を開始する](http://aka.ms/runbookauthor/azure/startrunbook) と [Runbook の出力とメッセージ](http://aka.ms/runbookauthor/azure/runbookoutput)します。

12. **[Write-HelloWorld]** の Runbook が開いた状態で **[開始]** をクリックします。

    ![発行済](./media/automation-create-runbook-from-samples/automation_10_PublishStart.png)

13. **[Runbook パラメーターの値を指定します]**ページで、Write-HelloWorld.ps1 スクリプトの入力パラメーターとして使用される **[名前]** を入力し、チェック マークをクリックします。

    ![Runbook のパラメーター](./media/automation-create-runbook-from-samples/automation_11_RunbookParams.png)

14. **[ジョブ]** をクリックして、開始した Runbook ジョブの状態を確認し、**[ジョブ開始]** 列にあるタイムスタンプをクリックしてジョブの概要を表示します。

    ![Runbook のステータス](./media/automation-create-runbook-from-samples/automation_12_RunbookStatus.png)

15. **[概要]** ページで、ジョブの概要、入力パラメーター、出力を表示できます。

    ![Runbook の概要](./media/automation-create-runbook-from-samples/automation_13_RunbookSummary_callouts.png)

ご利用ありがとうございます。 チュートリアルが完了しました。

## <a name="nextsteps"></a>次のステップ

1. このチュートリアルの単純な Runbook では、*Azure サービスを管理できません*。 ほとんどの runbook を使用して、 [Azure コマンドレット](http://msdn.microsoft.com/library/jj156055.aspx) ためには、Azure サブスクリプションへの認証を必要とします。 」の手順に従って [Azure Runbook での管理の構成](http://aka.ms/azureautomationauthentication) にこれらのコマンドレットを使用する Azure サブスクリプションを構成します。
2. 参照してください、 [リソース](#resources) Azure Automation の機能の詳細について以下に示されました。
3. 購読、 [Azure Automation ブログ](http://azure.microsoft.com/blog/tag/azure-automation) 、Azure Automation チームから最新の最新の状態にします。

## <a name="resources"></a>リソース

その他にも、Azure Automation や、独自の Runbook の作成方法の詳細を確認できる多数のリソースが用意されています。

- [Azure Automation ライブラリ](http://go.microsoft.com/fwlink/p/?LinkId=392860) 構成や、独自の runbook を作成するため、Azure Automation の管理について詳しく説明します。
- [Azure PowerShell コマンドレット](http://msdn.microsoft.com/library/jj156055.aspx) Windows PowerShell を使用した Azure の操作の自動化に関する情報を提供します。 Runbook ではこれらのコマンドレットを使用して Azure リソースを操作します。
- [Azure Automation ブログ](http://azure.microsoft.com/blog/tag/azure-automation) Microsoft から Azure Automation に関する最新の情報を提供します。
- [Automation フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=390561) マイクロソフトや Automation コミュニティで対応する Azure Automation に関する質問を投稿することができます。


## サンプル Runbook とユーティリティ Runbook

マイクロソフトおよび Azure Automation コミュニティが提供するサンプル Runbook を使うと、独自のソリューションを作成できます。また、ユーティリティ Runbook は大きめのオートメーション タスクの構成ブロックとして使用できます。 これらの runbook からをダウンロードすることができますか、 [スクリプト センター](http://azure.microsoft.com/documentation/scripts/) を使用して Azure Automation に直接インポートすることも、 [Runbook ギャラリー](http://aka.ms/runbookgallery)します。


## フィードバック

**フィードバックの送信**Azure Automation の Runbook ソリューションや統合モジュールを探索している場合は、スクリプト センターにスクリプトの要求を投稿することができます。 Azure Automation に関するフィードバックや機能の要求がある場合に投稿して [User Voice](http://feedback.windowsazure.com/forums/34192--general-feedback)します。 よろしくお願いいたします。





