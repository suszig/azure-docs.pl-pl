<properties 
    pageTitle="Azure が提供するコンピューティング ホスティング オプション" 
    description="Azure コンピューティング ホスティング オプションおよびその動作に関する詳細情報: Virtual Machines、Web サイト、Cloud Services など。" 
    headerExpose="" 
    footerExpose="" 
    services="cloud-services,virtual-machines"
    authors="Thraka" 
    documentationCenter=""
    manager="timlt"/>

<tags 
    ms.service="multiple" 
    ms.workload="multiple" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2015" 
    ms.author="adegeo;cephalin;kathydav"/>




# Azure が提供するコンピューティング ホスティング オプション

Azure にはアプリケーションを実行するための複数のホスティング モデルがあります。 それぞれのモデルには異なるサービスが用意されているため、実行する内容に正確に基づいてモデルを選択する必要があります。 この記事では、オプションの各テクノロジとその使用例を紹介します。

| コンピューティング オプション    | 対象となる読者   |
| ------------------ | --------   |
| [App Service]      | 任意のデバイス用のスケーラブルな Web Apps、Mobile Apps、API Apps、Logic Apps |
| [Cloud Services]   | OS のより詳細な制御が可能な、可用性と拡張性の高い N 階層のクラウド アプリケーション |
| [Virtual Machines] | OS の完全な制御が可能な、カスタマイズされた Windows と Linux VM |

[AZURE.INCLUDE [content](../../includes/app-service-choose-me-content.md)]

[AZURE.INCLUDE [content](../../includes/cloud-services-choose-me-content.md)]

[AZURE.INCLUDE [content](../../includes/virtual-machines-choose-me-content.md)]

## その他のオプション

Azure には、次のような特殊な目的に合わせるためのコンピューティング ホスティング モデルも用意されています。

* [Mobile Services](/services/mobile-services/)  
  モバイル デバイスで実行されるアプリに対するクラウド バックエンドを提供するように最適化されています。
* [Batch
  ](/services/batch/)  
類似する大量のタスク (理想的には複数のコンピューターで並列タスクとして実行されるワークロード) を処理するように最適化されています。
* [HDInsight (Hadoop)](/services/hdinsight/)  
  実行するために最適化された [MapReduce](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-storage-options/#hadoop) Hadoop クラスターでジョブです。 

## 使用するモデルを 選択する

一般的な目的に適合する 3 つの Azure コンピューティング ホスティング モデルは、どのモデルでもスケーラブルで信頼性の高いアプリケーションをクラウドで構築できます。 この基本的な類似性を前提に、どれを使用すればよいでしょうか。

多くの Web apps には、App Service が適しています。 デプロイと管理機能がそのプラットフォームに統合され、トラフィックの負荷に応じてサイトのスケールを機敏に調整できるほか、組み込みの負荷分散機能と Traffic Manager によって高い可用性が得られます。 既存のサイトを使用して簡単に App Service に移動する、 [オンライン移行ツール](https://www.migratetoazure.net/), で Web アプリケーション ギャラリーからオープン ソース アプリケーションを使用するかフレームワークと好みのツールを使用して新しいサイトを作成します。  [WebJobs](http://go.microsoft.com/fwlink/?linkid=390226) 機能により、簡単にバック グラウンド ジョブ処理をアプリケーションを追加したり、web アプリをまったく以外をコンピューティング ワークロードを実行したりします。 

サーバーへのリモート アクセス、サーバーのスタートアップ タスクの構成など、Web サーバー環境をもっと細かく制御する必要がある場合は、Azure Cloud Services が通常、最善の選択肢となります。

既にあるアプリケーションを Azure Websites や Azure Cloud Services で実行するためには、かなりの修正が必要という場合、Azure Virtual Machines を選ぶことで、クラウドへの移行を単純化できる場合があります。 ただし、VM の構成、セキュリティ対策、メンテナンスを正しく行うためには、Azure Websites や Cloud Services と比べて、はるかに時間がかかり、IT に関する豊富な知識と経験が要求されます。 Azure Virtual Machines を選択する場合は、VM 環境に対する修正プログラムの適用、更新、管理に伴って日々発生するメンテナンスの労力を考慮してください。

1 つのオプションだけでは目的に合わない場合があります。 必要な場合には、オプションを組み合わせることもできます。 たとえば、クラウド サービス Web ロールの管理面の利点を気に入っているが、互換性やパフォーマンス上の理由から Virtual Machine でホストされる標準 SQL Server も使用する必要があるアプリケーションを構築しているとします。 

<!-- In this case, the best option is to combine compute hosting options, as the figure below shows.--

<a name="fig4"></a>
![07_CombineTechnologies][07_CombineTechnologies] 
 
**Figure: A single application can use multiple hosting options.**

As the figure illustrates, the Cloud Services VMs run in a separate cloud service from the Virtual Machines VMs. Still, the two can communicate quite efficiently, so building an app this way is sometimes the best choice.
[07_CombineTechnologies]: ./media/fundamentals-application-models/ExecModels_07_CombineTechnologies.png
!-->

[App Service]: #tellmeas
[Virtual Machines]: #tellmevm
[Cloud Services]: #tellmecs

## 次のステップ

* [比較](../choose-web-site-cloud-service-vm/) App Service、Cloud Services、および仮想マシン
* 詳細について [App Service](../app-service-web-overview.md)
* 詳細について [クラウド サービス](services/cloud-services/)
* 詳細について [仮想マシン](https://msdn.microsoft.com/library/azure/jj156143.aspx) 


