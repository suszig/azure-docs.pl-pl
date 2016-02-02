<properties
    pageTitle="Azure Automation を使用した Azure Storage の管理"
    description="Azure Automation サービスを使用して大規模に Azure Storage を管理する方法について説明します。"
    services="storage, automation"
    documentationCenter=""
    authors="jodoglevy"
    manager="eamono"
    editor=""/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/03/2015"
    ms.author="jolevy"/>




# Azure Automation を使用した Azure Storage の管理

このガイドでは、Azure Automation サービスと、このサービスを使用して Azure Storage BLOB、テーブル、およびキューの管理を簡略化する方法について紹介します。


## Azure Automation とは

[Azure Automation](http://azure.microsoft.com/services/automation/) プロセスの自動化によってクラウド管理を簡略化するための Azure サービスです。 Azure Automation を使用して実行時間の長いタスク、手動タスク、エラーが発生しやすいタスク、頻繁に繰り返されるタスクを自動化し、信頼性と効率性を向上して組織のゴール達成までの時間を短縮することができます。

Azure Automation は、組織の拡大に伴って変化するニーズに対応可能な、信頼性と可用性の高いワークフロー実行エンジンを提供します。 Azure Automation では、サード パーティ製のシステムによって手動でプロセスを開始したり、必要なときに正確にタスクが起動されるようにスケジュールされた間隔でプロセスを開始できます。

Azure Automation でクラウド管理タスクを自動実行すれば、運用上のオーバーヘッドが削減され、IT/DevOps スタッフの負担が軽減されるため、ビジネス価値の向上にフォーカスすることができます。


## Azure Automation を Azure Storage の管理に役立てる方法

使用できる PowerShell コマンドレットを使用して Azure Automation で azure Storage を管理できる、 [Azure PowerShell ツール](https://msdn.microsoft.com/library/azure/jj156055.aspx)します。 Azure Automation には、このような Storage PowerShell コマンドレットがあらかじめ用意されており、サービス内ですべての BLOB、テーブル、およびキュー管理タスクを実行することができます。 Azure Automation 内のこれらのコマンドレットと別の Azure サービスのコマンドレットを組み合わせて、Azure サービスおよびサード パーティ システム全体の複雑なタスクを自動化することもできます。

[Azure Automation runbook ギャラリー](http://azure.microsoft.com/blog/2014/10/07/introducing-the-azure-automation-runbook-gallery/) 製品チームおよびコミュニティの runbook を Azure Storage、その他の Azure サービスおよびサード パーティ製システムの管理を自動化するためのさまざまなが含まれています。 ギャラリーに含まれている Runbook の例を示します。

 * [X 日よりも古い Azure ストレージ blob を削除します。](https://gallery.technet.microsoft.com/scriptcenter/Remove-Storage-Blobs-that-aae4b761)
 * [Azure ストレージから Azure Automation に blob をダウンロードします。](https://gallery.technet.microsoft.com/scriptcenter/a-Blob-from-Azure-Storage-6bc13745)
 * [Azure クラウド サービスの Azure VM データ ディスクのコピーを作成します。](https://gallery.technet.microsoft.com/scriptcenter/Make-copies-of-Azure-VM-065a6394)


## 次のステップ

ここまで、Azure Automation の基本と Azure Automation を使用して Azure Storage BLOB、テーブル、およびキューを管理する方法について説明しました。Azure Automation の詳細については、これらのリンクを参照してください。

Azure automation の [チュートリアル入門](../automation-create-runbook-from-samples.md)






