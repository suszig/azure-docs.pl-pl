<properties 
    pageTitle="Azure Automation での Runbook のテスト"
    description="Azure Automation で Runbook を発行する前にテストを行い、期待どおりに動作することを確認できます。この記事では、Runbook のテスト方法と出力の見方について説明します。"
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
    ms.date="09/23/2015"
    ms.author="bwren" />


# Azure Automation での Runbook のテスト

Runbook をテストするときに、 [下書きバージョン](automation-creating-importing-runbook.md#publishing-a-runbook) が実行されるを実行したすべての操作が完了しました。 ジョブ履歴は作成されませんが、 [出力](automation-runbook-output-and-messages.md#output-stream) と [警告およびエラー](automation-runbook-output-and-messages.md#message-streams) ストリームがテストに表示されます] ウィンドウに出力します。 メッセージを [詳細ストリーム](automation-runbook-output-and-messages.md#message-streams) 場合のみ、出力ウィンドウに表示される、 [$VerbosePreference 変数](automation-runbook-output-and-messages.md#preference-variables) Continue に設定されています。

ドラフト バージョンの実行中でも、Runbook ではワークフローを通常どおり実行して、環境内のリソースに対するすべてのアクションを行います。 このため、Runbook をテストするのは非運用環境のリソースのみにしてください。

それぞれのテスト手順 [runbook の種類](automation-runbook-types.md) は同じ、およびそこに、テキスト エディターと Azure プレビュー ポータルで、グラフィカル エディターの間でのテストに違いはありません。


## Azure プレビュー ポータルで Runbook をテストするには

いずれかを扱える [runbook の種類](automation-runbook-types.md) Azure プレビュー ポータルで。

1. いずれかで、runbook の下書きバージョンを開き、 [テキスト エディター](automation-editing-a-runbook#Portal) または [グラフィカル エディター](automation-graphical-authoring-intro.md)します。
2. **[テスト]** ボタンをクリックして [テスト] ブレードを開きます。
3. Runbook にパラメーターがある場合は左側のウィンドウに表示され、そこでテストに使用する値を指定することができます。
4. テストを実行する場合、 [Hybrid Runbook Worker](automation-hybrid), 、変更し、 **実行設定** に **Hybrid Worker** ターゲット グループの名前を選択します。 それ以外の場合は、既定の **[Azure]** をそのまま使用してクラウドでテストを実行します。
5. **[開始]** をクリックしてテストを開始します。
6. 場合は、runbook が [PowerShell ワークフロー](automation-runbook-types.md#powershell-workflow-runbooks) または [グラフィカル](automation-runbook-types.md#graphical-runbooks), 、停止または中断ままが [出力] ペインの下にあるボタンでテストされていることができます。 Runbook は、中断される場合は中断前に現在のアクティビティを完了します。 Runbook を中断した後で、停止または再開できます。
7. [出力] ウィンドウで Runbook の出力を確認します。



## Azure ポータルで Runbook をテストするには

操作するだけ [PowerShell ワークフロー runbook](automation-runbook-types.md#powershell-workflow-runbooks) Azure ポータルで。


1. [Runbook の下書きバージョンを開きます](automation-edit-textual-runbook.md#to-edit-a-runbook-with-the-azure-portal)します。
2. **[テスト]** をクリックしてテストを開始します。 Runbook にパラメーターがある場合は、テストに使用する値を指定するダイアログ ボックスが表示されます。
6. [出力] ウィンドウの下にあるボタンを使用して、テスト中に Runbook を停止または中断することができます。 Runbook は、中断される場合は中断前に現在のアクティビティを完了します。 Runbook を中断した後で、停止または再開できます。
7. [出力] ウィンドウで Runbook の出力を確認します。


## 関連記事:

- [作成または Azure Automation で runbook をインポートします。](automation-creating-importing-runbook.md)
- [Azure Automation のグラフィカルな runbook](automation-graphical-authoring-intro.md)
- [Azure Automation のテキストの runbook の編集](automation-edit-textual-runbook.md)
- [Runbook の出力と Azure Automation 内のメッセージ](automation-runbook-output-and-messages.md)




