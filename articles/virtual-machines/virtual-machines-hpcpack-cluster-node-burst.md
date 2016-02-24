<properties
 pageTitle="HPC Pack クラスターへのバースト ノードの追加 |Microsoft Azure"
 description="クラウド サービスで実行されている worker ロール インスタンスを必要に応じてコンピューティング リソースとして Azure の HPC Pack ヘッド ノードに追加する方法について説明します。"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="09/28/2015"
 ms.author="danlep"/>

# オンデマンド「バースト」ノード (worker ロール インスタンス) をコンピューティング リソースとして Azure の HPC Pack クラスターに追加する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


この記事は、Azure「バースト」ノード (ワーカー ロール インスタンスを追加する方法を示します
クラウド サービスで実行されている) オンデマンドでコンピューティング リソースとして、
Azure の HPC Pack ヘッド ノードに既存します。 これにより、事前構成された一連のコンピューティング ノード VMを整備しなくても、Azure の HPC クラスターのコンピューティング能力を拡大できます。

![バースト ノード][バースト]

>[AZURE.TIP] 使用する場合、 [HPC Pack IaaS デプロイメント スクリプト](virtual-machines-hpcpack-cluster-powershell-script.md) Azure では、クラスターを作成するには
自動化された Azure バースト ノードを含めることができます。
次のセキュリティに関するヒントを検討してください。

この記事の手順を簡単に Azure ノードを追加します。
クラウド ベース HPC Pack ヘッド ノード VM のテストまたは概念実証のデプロイします。 手順は、本質的には、
「Azure にバースト」する 1 つとして同じクラウドを追加するには、コンピューティングの能力を
内部設置型の HPC Pack クラスターです。 チュートリアルについては、次を参照してください。 [Microsoft HPC Pack を使用したハイブリッド コンピューティング クラスターをセットアップ](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)します。 次に
詳細なガイダンスと運用環境のデプロイメントに関する考慮事項を参照してください。
[Microsoft HPC を使用した Azure へのバーストします。
パック](https://technet.microsoft.com/library/gg481749.aspx)します。

A8 を使用する場合、または A9 のコンピューティング集中型インスタンス サイズを参照してください。
[A8、A9、A10、A11 コンピューティング集中型インスタンスに関する](virtual-machines-a8-a9-a10-a11-specs.md)します。

## 前提条件

* **HPC Pack ヘッド ノードを Azure VM で展開** -を参照してください [、HPC の展開
Azure VM にヘッド ノードをパック](virtual-machines-hpcpack-cluster-headnode.md) の
クラシック (サービス管理) の配置モデルでクラスターのヘッド ノードを作成する手順です。

* **Azure サブスクリプション** : Azure ノードを追加する同じを選択できます
ヘッド ノード VM、または別の展開に使用するサブスクリプション
サブスクリプション (または配信登録)。

* **コア クォータ** -マルチコア サイズでいくつかの Azure ノードを展開する場合に特にコアのクォータを増やす必要があります。 クォータを増やすには [オンライン カスタマー サポートに申請](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) 無償でします。

## 手順 1: クラウド サービスとストレージ アカウントを作成し、Azure ノードを追加します。

Azure クラシック ポータルや相当するツールを使用して、展開に必要なことを次の構成
Azure ノード:

* 新しい Azure クラウド サービス
* 新しい Azure ストレージ アカウント

>[AZURE.NOTE] サブスクリプション内の既存のクラウド サービスを再利用しません。 しません。
このクラウド サービスに個別のカスタム クラウド サービス パッケージを展開します。
HPC Pack クラウド サービスを自動的に展開するときにパッケージ化します。
(プロビジョニング)、Azure ノードを起動します。

**考慮事項**

* 作成する予定の Azure ノード テンプレートごとに別個のクラウド サービスを構成します。 ただし、複数のノード テンプレートに同じストレージ アカウントを使用できます。

* 通常、デプロイのクラウド サービスとストレージ アカウントが同じリージョンにあるはずです。




## 手順 2: Azure 管理証明書の作成

Azure ノードをコンピューティング リソースとして追加するには、管理する必要があります。
証明書をヘッド ノードと対応する証明書のアップロード
 展開に使用する Azure サブスクリプション。

このシナリオを選択できます、 **既定の HPC Azure 管理
証明書** その HPC Pack は、インストールし、構成時に自動的に、
ヘッド ノードです。 この証明書はテスト目的で役立つと
概念実証の展開。 この証明書を使用するアップロードするだけで、
ヘッド ノード VM から C:\Program files \microsoft HPC Pack 2012\Bin\hpccert.cer をファイルに、
使用します。

管理証明書の構成オプションは、次を参照してください。
[Azure バーストの Azure 管理証明書を構成するシナリオ
展開](http://technet.microsoft.com/library/gg481759.aspx)します。

## 手順 3: クラスターに Azure ノードをデプロイする



追加し、開始する手順
このシナリオでの azure ノードは、通常を使用したものと同じ
内部設置型ヘッド ノードです。 詳細については、次を参照してください。
セクション [Microsoft HPC Pack を使用して Azure ノードを展開する手順](https://technet.microsoft.com/library/gg481758.aspx):

* Azure ノード テンプレートを作成する

* Azure ノードを Windows HPC クラスターに追加する

* Azure ノードを起動 (プロビジョニング) する

ノードを追加し、起動したら、ノードを利用してクラスター ジョブを実行できます。

Azure ノードを展開するときに問題が発生した場合は、次を参照してください [トラブルシューティング。
Microsoft HPC を使用して Azure ノードの展開
パック](http://technet.microsoft.com/library/jj159097(v=ws.10).aspx)します。

## 次のステップ

* 望む場合
自動的に拡大または縮小」の手順に従って、Azure コンピューティング リソース
ジョブと、クラスター上のタスクの現在のワークロードを参照してください [拡大および縮小を Azure でコンピューティング リソース、HPC Pack クラスター](virtual-machines-hpcpack-cluster-node-autogrowshrink.md)します。

<!--Image references-->
[burst]: ./media/virtual-machines-hpcpack-cluster-node-burst/burst.png

