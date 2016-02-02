<properties
    pageTitle="Azure Batch サービスの基本 |Microsoft Azure"
    description="大規模な並列ワークロードと HPC ワークロードに関する Azure Batch サービスの概念、ワークフロー、シナリオについて説明します。"
    services="batch"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="11/19/2015"
    ms.author="danlep"/>


# Azure Batch の基礎

Azure Batch を使用すると、大規模な並列コンピューティングやハイ パフォーマンス コンピューティング (HPC) のアプリケーションをクラウドで効率的に実行できます。 Azure Batch は、多くのコンピューティング処理を要する作業を仮想マシン (コンピューティング ノード) の管理されたコレクション上で実行するようにスケジュール設定するためのプラットフォーム サービスです。ジョブのニーズに合わせてコンピューティング リソースをスケールすることができます。 Batch サービスでは、Azure のコンピューティング リソースと大規模なバッチ ジョブをオンデマンドで、またはスケジュールに従って実行するようにプログラムで定義します。HPC クラスター、個々の仮想マシン、仮想ネットワーク、またはジョブ スケジューラの構成と管理を手動で行う必要がありません。

## ユース ケース

Batch は、*バッチ処理*または*バッチ コンピューティング*のための管理されたサービスで、期待した結果が得られるように類似のタスクを大量に実行します。 バッチ コンピューティングは、スケジュールに従って、またはオンデマンドで大量のデータの処理、変換、および分析を行う組織にとっては一般的なアプローチで、金融サービスからエンジニアリングまで各種分野で実践されています。

Batch は、本質的に並列である ("驚異的並列性" とも呼ばれます) アプリケーションやワークロードに対応するため、複数のコンピューターで並列タスクを実行するのに役立ちます。 図 1 を参照してください。

![並列タスク][parallel]

* * 図 1 です。 複数のコンピューター * * で実行される並列タスク

たとえば、次のようになります。

* 財務リスクのモデリング
* 画像のレンダリングと画像処理
* メディアのエンコードとコード変換
* 遺伝子配列の分析
* ソフトウェアのテスト

また、Batch で並列計算を実行すると、最終的により少ないステップで、より複雑な他の並列ワークロードを処理できます。
>[AZURE.NOTE]現時点では、Batch は、Windows Server ベースの仮想マシンで実行されるワークロードのみをサポートしています。 また、現時点では、Batch は Message Passing Interface (MPI) アプリケーションをサポートしていません。

バッチの他の HPC ソリューションのオプションで、Azure との比較では、次を参照してください。 [Batch および HPC ソリューション](batch-hpc-solutions.md)します。

## Batch を使用した開発

Batch API を使用した開発では、コンピューティング ノードのプールの作成と管理、およびプールで実行されるジョブとタスクのスケジュール設定を行います。 必要に応じて、やなどのツールで管理される大きなワークフローの一部として、スケジュールでジョブとタスクを実行するには、クライアント アプリケーションまたはフロント エンドを記述 [Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/)します。

参照してください [Azure Batch の機能概要](batch-api-basics.md) Batch の概念の詳細について。

### 必要となるアカウント

+ **Azure アカウントとサブスクリプション** -できるアクティブ化するアカウントを持っていない場合、 [MSDN サブスクライバーの特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) サインアップや、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。

+ **Batch アカウント** - Batch の API 呼び出しを行う際に、Batch アカウントの名前と URL、およびアクセス キーを資格情報として使用します。 コンピューティング ノード、プール、ジョブ、タスクなどの Batch リソースはすべて Batch アカウントに関連付けられています。 Batch アカウントの作成し、アカウントは、使用するために、アクセス キーを管理する方法の 1 つ、 [Azure ポータル](batch-account-create-portal.md)します。

+ **ストレージ アカウント** - ほとんどの Batch シナリオでは、データの入力と出力、およびコンピューティング ノード上で実行されるスクリプトや実行可能ファイルを格納するために、Azure ストレージ アカウントが必要になります。 ストレージ アカウントを作成するを参照してください。 [は Azure ストレージ アカウント](../storage/storage-create-storage-account.md)します。

### Batch 開発ライブラリおよびツール

Azure Batch アプリケーションの開発と管理には、以下の .NET ライブラリとツールを使用します。

+ [バッチ クライアント .NET ライブラリ](http://www.nuget.org/packages/Azure.Batch/) (NuGet)-Batch サービスでジョブを実行するクライアント アプリケーションの開発
+ [バッチ管理 .NET ライブラリ](http://www.nuget.org/packages/Microsoft.Azure.Management.Batch/) (NuGet)-バッチの管理アカウント
+ [Batch Explorer](https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer) (GitHub) - 参照するには、この GUI アプリケーションを構築、アクセス、およびジョブとタスクを含む、Batch アカウントの更新リソースがノードとプール、計算ノード上のファイルを計算します。 See the [blog post](http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx).


## シナリオ: 並列ワークロードをスケールアウトする

Batch API を使用する一般的なシナリオでは、数千個ものコンピューティング コアを提供するコンピューティング プールでのイメージ レンダリングのような、本質的に並列な作業があります。

図 2 は、Batch で並列ワークロードを実行する Batch .NET クライアント アプリケーションを使用するワークフローを示しています。


![作業項目のワークフロー][work_item_workflow]

* * 図 2 です。 並列ワークロード バッチ * * をスケール アウト

1.  ジョブに必要な入力ファイル (ソース データやイメージ) を Azure ストレージ アカウントにアップロードします。 ジョブ タスクが実行されると、Batch サービスはコンピューティング ノードにファイルを読み込みます。

2.  コンピューティング ノードで実行されるプログラム ファイルまたはスクリプトをストレージ アカウントにアップロードします。 これらには、バイナリ ファイルとその依存アセンブリが含まれる場合があります。 タスクが実行されると、Batch サービスはコンピューティング ノードにもこれらのファイルを読み込みます。

3.  プログラムによって Batch アカウントに Batch コンピューティング ノードのプールを作成し、サイズや実行する OS などのプロパティを指定します。 定義することも方法、プール内のノードの数 [上または下のスケールを設定](batch-automatic-scaling.md) で、ワークロードに対応します。 タスクを実行すると、このプールからノードが割り当てられます。

4.  ノードのプールでワークロードを実行する Batch ジョブをプログラムで定義します。

5.  ジョブにタスクを追加します。 各タスクは、アップロードされたプログラムを使用して、アップロードされたファイルの情報を処理します。 実行する可能性があります、作業負荷に応じて [、同時に複数のタスク](batch-parallel-node-tasks.md) リソース使用率を最大化するには、各計算ノードにします。 サポートをバッチも特殊な [ジョブの準備と完了タスク](batch-job-prep-release.md) 、スケジュールされたタスクの実行前に、計算ノードを準備し、後にクリーンアップします。

6.  Batch ワークロードを実行し、進行状況と結果を監視します。 アプリケーションは、HTTPS 経由で Batch サービスと通信します。 アプリケーションのパフォーマンスを向上させるには、多数のプール、ジョブ、およびタスクを監視する際に活用 [クエリする方法は効率的な](batch-efficient-list-queries.md) Batch サービスです。






## 次のステップ

* を使用して最初のアプリケーションの開発を開始する [バッチ クライアント .NET ライブラリ](batch-dotnet-get-started.md)
* [Batch のコード サンプルを参照 [GitHub](https://github.com/Azure/azure-batch-samples)


[parallel]: ./media/batch-technical-overview/parallel.png 
[work_item_workflow]: ./media/batch-technical-overview/work_item_workflow.png 

