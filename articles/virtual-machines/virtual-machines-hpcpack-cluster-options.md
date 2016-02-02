<properties
 pageTitle="クラウドでの HPC Pack クラスター オプション | Microsoft Azure"
 description="Microsoft HPC Pack を使用して Azure クラウドでハイ パフォーマンス コンピューティング (HPC) クラスターを作成および管理するためのオプションについて学習します。"
 services="virtual-machines,cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/08/2015"
 ms.author="danlep"/>


# Microsoft HPC Pack を使用して Azure でハイパフォーマンス コンピューティング (HPC) クラスターを作成および管理するためのオプション

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


Microsoft HPC Pack と、Azure のコンピューティングおよびインフラストラクチャ サービスを使用して、クラウド ベースのハイ パフォーマンス コンピューティング (HPC) クラスターを作成および管理します。 [HPC Pack](https://technet.microsoft.com/library/jj899572.aspx) Microsoft Azure、Windows Server テクノロジに基づいている、Microsoft の無料の HPC ソリューションは、Windows と Linux の HPC の両方のワークロードをサポートしています。 クラウド ベースの HPC Pack クラスターには、クラスター管理者や独立系ソフトウェア ベンダー (ISV) がオンプレミスのコンピューティング クラスター インフラストラクチャへの投資を減らしながら、コンピューティング集約型アプリケーションを実行できる、柔軟でスケーラブルなプラットフォームが用意されています。


## Azure VM での HPC Pack クラスターの実行

### Azure VM イメージ

* [Windows Server 2012 R2 上の HPC Pack](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/)

* [Windows Server 2012 R2 上の HPC Pack コンピューティング ノード](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodeonwindowsserver2012r2/)

* [HPC Pack コンピューティング ノードと Windows Server 2012 R2 上の Excel](http://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2computenodewithexcelonwindowsserver2012r2/)

### Azure クイック スタート テンプレート

* [HPC クラスターを作成します。](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/)

* [Linux 計算ノードと共に HPC クラスターを作成します。](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)

* [カスタムの計算ノード イメージ、HPC クラスターを作成します。](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/)

### PowerShell デプロイメント スクリプト

* [HPC Pack IaaS デプロイメント スクリプトで HPC クラスターを作成します。](virtual-machines-hpcpack-cluster-powershell-script.md)

### Tutorials (チュートリアル)

* [チュートリアル: Azure で HPC Pack クラスターで Linux 計算ノードの使用開始します。](virtual-machines-linux-cluster-hpcpack.md)

* [チュートリアル: Linux で実行 NAMD with Microsoft HPC Pack コンピューティング Azure 内のノード](virtual-machines-linux-cluster-hpcpack-namd.md)

* [チュートリアル: 実行 OpenFOAM Microsoft HPC Pack で、Azure Linux RDMA クラスターを使用](virtual-machines-linux-cluster-hpcpack-openfoam.md)

* [チュートリアル: Excel と SOA ワークロードを実行する Azure の HPC Pack クラスターと開始します。](virtual-machines-excel-cluster-hpcpack.md)



### Azure クラシック ポータルを使用した手動デプロイメント

* [Azure VM での HPC Pack クラスターのヘッド ノードの設定します。](virtual-machines-hpcpack-cluster-headnode.md)

### クラスターの管理

* [Azure で HPC Pack クラスターでコンピューティング ノードを管理します。](virtual-machines-hpcpack-cluster-node-manage.md)

* [Azure での HPC Pack ヘッド ノードに Azure「バースト」ノードを追加します。](virtual-machines-hpcpack-cluster-node-burst.md)

* [HPC Pack クラスターでの Azure コンピューティング リソースの増減](virtual-machines-hpcpack-cluster-node-autogrowshrink.md)

* [Azure で HPC Pack クラスターにジョブを送信します。](virtual-machines-hpcpack-cluster-submit-jobs.md)


## HPC Pack クラスターへの worker ロールの追加

* [HPC Pack での Azure worker インスタンスへのバーストします。](https://technet.microsoft.com/library/gg481749.aspx)

* [チュートリアル: Azure で HPC Pack でハイブリッド クラスターのセットアップします。](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)

* [Azure での HPC Pack ヘッド ノードに Azure「バースト」ノードを追加します。](virtual-machines-hpcpack-cluster-node-burst.md)

* [HPC Pack クラスターでの Azure コンピューティング リソースの増減](virtual-machines-hpcpack-cluster-node-autogrowshrink.md)

## Azure Batch との統合

* [Azure Batch と HPC Pack へのバーストします。](https://technet.microsoft.com/library/mt612877.aspx)

## MPI ワークロードのための RDMA のクラスターの作成

* [MPI アプリケーションを実行する HPC Pack で Windows RDMA クラスターを設定します。](virtual-machines-windows-hpcpack-cluster-rdma.md)

* [チュートリアル: 実行 OpenFOAM Microsoft HPC Pack で、Azure Linux RDMA クラスターを使用](virtual-machines-linux-cluster-hpcpack-openfoam.md)







