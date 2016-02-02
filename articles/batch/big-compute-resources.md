<properties
   pageTitle="ビッグ コンピューティング: バッチとハイ パフォーマンス コンピューティング (HPC) に関するテクニカル リソース | Microsoft Azure"
   description="Azure で大規模並列、Batch、HPC ワークロードを実行するのに役立つテクニカル リソースの一覧を示します。"
   services="batch, cloud-services, virtual-machines"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="multiple"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-compute"
   ms.date="09/29/2015"
   ms.author="danlep"/>


# Azure における大規模なコンピューティング: バッチとハイ パフォーマンス コンピューティング (HPC) に関するテクニカル リソース

これは、Azure で大規模並列、Batch、HPC ワークロードを実行するのに役立つテクニカル リソース ガイドです。 既存の Batch または HPC ワークロードを Azure クラウドに拡張するか、新しい大規模なコンピューティング ソリューションを Azure サービスを使用して Azure に構築します。

## ソリューションのオプション

Azure の大規模なコンピューティング オプションについて説明し、ワークロードやビジネス ニーズに適切なアプローチを選択します。

* [Batch および HPC ソリューション](batch-hpc-solutions.md)

* [Azure と HPC を使用したクラウドで大規模なコンピューティングのビデオ:](http://azure.microsoft.com/documentation/videos/teched-europe-2014-big-compute-in-the-cloud-with-high-performance-computing-on-azure/)


## Azure Batch

[バッチ](http://azure.microsoft.com/services/batch/) プラットフォーム サービス、アプリケーションをクラウド対応にして、セットアップして、クラスターやジョブ スケジューラーを管理しなくてもジョブを実行するが簡単です。 SDK を使用して、さまざまな言語を介してクライアント アプリケーションを Azure Batch と統合し、Azure にデータをステージングし、ジョブの実行パイプラインをビルドします。

* [ドキュメント](http://azure.microsoft.com/documentation/services/batch/)

* [API リファレンス](https://msdn.microsoft.com/library/azure/dn820177.aspx)

* [.NET 向け Azure Batch ライブライの概要チュートリアル。](batch-dotnet-get-started.md)

* [Batch フォーラム](https://social.msdn.microsoft.com/Forums/home?forum=azurebatch)

* [Batch のビデオ](https://azure.microsoft.com/documentation/videos/index/?services=batch)

## HPC クラスターのソリューション

既存の Windows または Linux HPC クラスターを Azure にデプロイまたは拡張して、コンピューティング集中型のワークロードを実行します。

### Microsoft HPC Pack

HPC Pack は、Microsoft Azure と Windows Server テクノロジに基づいて構築された、Microsoft の無料 HPC ソリューションであり、Windows と Linux HPC の両方のワークロードを実行できます。

* [HPC Pack 2012 R2 更新プログラム 3 をダウンロードします。](https://www.microsoft.com/en-us/download/details.aspx?id=49922)

* [ドキュメント](https://technet.microsoft.com/library/jj899572.aspx)

* [Azure での Microsoft HPC Pack での HPC クラスターのオプション](../virtual-machines/virtual-machines-hpcpack-cluster-options.md)

* [HPC Pack での Azure worker インスタンスへのバーストします。](https://technet.microsoft.com/library/gg481749.aspx)

* [Azure Batch と HPC Pack へのバーストします。](https://technet.microsoft.com/library/mt612877.aspx)

* [Windows HPC フォーラム](https://social.microsoft.com/Forums/home?category=windowshpc)

### Linux と OSS クラスターのソリューション

これらの Azure クイック スタート テンプレートを使用して、オープン ソース ツールを備えた Linux HPC クラスターをデプロイします。

* [SLURM クラスターを起動します。](http://azure.microsoft.com/documentation/templates/slurm/)
 [ブログの投稿](http://blogs.technet.com/b/windowshpc/archive/2015/06/06/deploy-a-slurm-cluster-on-azure.aspx)

* [トルク クラスターを起動します。](http://azure.microsoft.com/documentation/templates/torque-cluster/)

## Microsoft MPI

[Microsoft MPI](https://msdn.microsoft.com/library/bb524831.aspx) (MS-MPI) はマイクロソフトの開発および Windows プラットフォームで並列アプリケーションを実行する標準 Message Passing Interface の実装です。


* [MS-MPI をダウンロードします。](http://go.microsoft.com/FWLink/p/?LinkID=389556)

* [MS-MPI リファレンス](https://msdn.microsoft.com/library/dn473458.aspx)

* [MPI フォーラム](https://social.microsoft.com/Forums/home?forum=windowshpcmpi)


## コンピューティング集中型インスタンス

Azure キャンペーン、 [さまざまなサイズ](../virtual-machines/virtual-machines-size-specs.md), 、コンピューティング集中型を含む [A8、A9、A10、および A11 インスタンス](../virtual-machines/virtual-machines-a8-a9-a10-a11-specs.md), 、Linux および Windows HPC ワークロードを実行します。

* [MPI アプリケーションを実行する Linux RDMA クラスターを設定します。](../virtual-machines/virtual-machines-linux-cluster-rdma.md)

* [MPI アプリケーションを実行する Microsoft HPC Pack で Windows RDMA クラスターを設定します。](../virtual-machines/virtual-machines-windows-hpcpack-cluster-rdma.md)

## アーキテクチャの設計図

* [大規模なコンピューティング - 金融サービス](http://go.microsoft.com/fwlink/?LinkId=536378) (PDF) は、運用し、リスク管理、レポート、シミュレーション用のクラウドで大規模なコンピューター処理とデータの分析を調整する方法を示します。

## サンプルとデモ

* [Azure Batch のコード サンプル](https://github.com/Azure/azure-batch-samples)

* [Batch Apps Blender のサンプル](https://github.com/Azure/azure-batch-apps-blender) と [ブログの投稿](http://azure.microsoft.com/blog/2015/01/26/blender-on-azure-batch/)

## 関連する Azure サービス

* [Data Factory](http://azure.microsoft.com/documentation/services/data-factory/)

* [機械学習](http://azure.microsoft.com/documentation/services/machine-learning/)

* [HDInsight](http://azure.microsoft.com/documentation/services/hdinsight/)

* [仮想マシン](http://azure.microsoft.com/documentation/services/virtual-machines/)

* [クラウド サービス](http://azure.microsoft.com/documentation/services/cloud-services/)

* [メディア サービス](http://azure.microsoft.com/documentation/services/media-services/)



## 次のステップ

* 最新情報については、次を参照してください。、 [Microsoft HPC と Batch のチームのブログ](http://blogs.technet.com/b/windowshpc/) と [Azure ブログ](http://azure.microsoft.com/blog/tag/hpc/)します。
* 参照してください [バッチの新機能](http://azure.microsoft.com/updates/?service=batch) やサブスクライブを実行、 [RSS フィード](http://azure.microsoft.com/updates/feed/?service=batch)します。





