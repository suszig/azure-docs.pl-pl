<properties
 pageTitle="A8、A9、A10、A11 インスタンスについて | Microsoft Azure"
 description="仮想マシンとクラウド サービスに Azure A8、A9、A10、A11 という多くのコンピューティング処理を要するサイズを使用する場合の背景情報と考慮事項について説明します。"
 services="virtual-machines, cloud-services"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="infrastructure-services"
 ms.date="10/30/2015"
 ms.author="danlep"/>


# A8、A9、A10、A11 コンピューティング集中型インスタンスについて

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


この記事では、*コンピューティング集中型*インスタンスとも呼ばれる Azure A8、A9、A10、A11 インスタンスの背景情報および使用上の注意事項について説明します。 これらのインスタンスの主な機能:

* **高性能のハードウェア** – これらのインスタンスを実行する Azure データ センター ハードウェアは、ハイ パフォーマンス コンピューティング (HPC) クラスター アプリケーション、モデリング、シミュレーションなど、コンピューティング集中型およびネットワーク集中型アプリケーション用に設計および最適化されています。

* **MPI アプリケーションの RDMA ネットワーク接続** – A8 および A9 インスタンスは必要なネットワーク ドライバーを使用して構成した場合、リモート ダイレクト メモリ アクセス (RDMA) テクノロジに基づく Azure の低待機時間で高スループットのネットワークを介して他の A8 および A9 インスタンスと通信できます。 この機能により、サポートされている Linux または Windows Message Passing Interface (MPI) の実装を使用するアプリケーションのパフォーマンスを向上させることができます。

* **Linux および Windows HPC クラスターのサポート** – Azure でクラスター管理およびジョブ スケジューリング ソフトウェアを A8、A9、A10、および A11 インスタンスにデプロイして、スタンドアロン HPC クラスターを作成するか、オンプレミスのクラスターに容量を追加します。 他の Azure VM のサイズと同様に、A8、A9、A10、および A11 インスタンスは、Azure VM (IaaS) での標準またはカスタムの Windows Server および Linux オペレーティング システム イメージまたは Azure リソース マネージャー テンプレート、またはクラウド サービス (Windows Server 専用 PaaS) での Azure ゲスト OS リリースをサポートします。

>[AZURE.NOTE]A10 および A11 インスタンスのパフォーマンス最適化と仕様は、A8 および A9 インスタンスと同じです。 ただし、Azure での RDMA ネットワークへのアクセスは含まれません。 A10 と A11 のインスタンスは、ノード間で一定した待ち時間の少ない通信を必要としない HPC アプリケーション (パラメトリックまたは驚異的並列アプリケーションとも呼ばれる) 向けに設計されています。


## 仕様

### CPU とメモリ

次の表に示すように、Azure の A8、A9、A10、A11 コンピューティング集中型インスタンスは、高速のマルチコア CPU と大容量のメモリを備えています。

 サイズ| CPU| メモリ
------------- | ----------- | ----------------
 A8 および A10| Intel Xeon E5 2670<br/>8 コア @ 2.6 GHz| DDR3 1600 MHz<br/>56 GB
 A9 および A11| Intel Xeon E5 2670<br/>16 コア @ 2.6 GHz| DDR3 1600 MHz<br/>112 GB

>[AZURE.NOTE]サポートされる命令セット拡張機能など、プロセッサに関する詳細については、Intel.com の Web サイトを参照してください。 VM ストレージの容量とディスクの詳細については、次を参照してください。 [仮想マシンのサイズ](virtual-machines-size-specs.md)します。

### ネットワーク アダプター

A8 および A9 インスタンスは 2 個のネットワーク アダプターを備え、次の 2 つのバックエンド Azure ネットワークに接続します。


 ネットワーク| 説明
-------- | -----------
 10 Gbps イーサネット| Azure サービス (Azure Storage や Azure Virtual Network など) およびインターネットに接続します。
 32 Gbps バックエンド、RDMA 対応| 1 つのクラウド サービスまたは可用性セット内のインスタンス間での低待機時間、高スループットのアプリケーション通信を可能にします。MPI トラフィック専用に予約されます。

>[AZURE.IMPORTANT]Linux を実行する A8 および A9 VM では、現在、RDMA ネットワークへのアクセスは、SUSE Linux Enterprise Server 12 (SLES 12) で Azure Linux RDMA および Intel MPI Library 5 を使用するアプリケーションを通じてのみ有効です。 Windows Server を実行するA8 および A9 インスタンスでは、RDMA ネットワークへのアクセスは現在、Microsoft Network Direct インターフェイスを使用するアプリケーションを通じてのみ実行できます。 参照してください [RDMA ネットワークへのアクセス](#access-the-rdma-network) で、この記事の他の要件です。

A10 および A11 インスタンスは、Azure サービスおよびインターネットに接続する 10 Gbps イーサネット ネットワーク アダプターを 1 個備えています。

## サブスクリプションに関する考慮事項

* **Azure アカウント** – 少数のコンピューティング集中型インスタンスより多数のインスタンスをデプロイする場合は、従量課金のサブスクリプションまたは他の購入オプションを検討してください。 MSDN サブスクリプションを使用することもできます。 参照してください [MSDN サブスクライバー向けの Azure 特典](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)します。 使用している場合、 [Azure 無料評価版](http://azure.microsoft.com/pricing/free-trial/), 、限られた数の Azure コンピューティング コアのみに使用することができます。

* **コア クォータ** – Azure サブスクリプションのコア クォータを、既定値のサブスクリプションあたり 20 コア (Azure サービス管理のデプロイメントの場合) またはリージョンあたり 20 コア (Azure リソース管理のデプロイメントの場合) から増やすことが必要な場合があります。8 コアまたは 16 コア インスタンスの多くのシナリオでは不十分です。 初期テストの場合、100 コアへのクォータ増量を要求することを検討してください。 これを行うには、ようにに無償サポート チケットを開く [について Azure の制限し、増設](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)します。
    >[AZURE.NOTE]Azure のクォータは容量保証ではなくクレジット制限です。 使用したコアに対してのみ課金されます。

* **仮想ネットワーク** – コンピューティング集中型インスタンスを使用するために、Azure Virtual Network は必要ありません。 ただし、多くの IaaS シナリオの場合、またはアプリケーション ライセンス サーバーなどのオンプレミス リソースへのアクセスが必要なサイト間接続の場合は、少なくともクラウド ベースの Azure Virtual Network が必要になる可能性があります。 インスタンスをデプロイする前に、新しい (リージョン) 仮想ネットワークを作成する必要があります。 アフィニティ グループ内の仮想ネットワークに A8、A9、A10、または A11 VM を追加することはできません。 詳細については、次を参照してください。、 [virtual network ドキュメント](https://azure.microsoft.com/documentation/services/virtual-network/)します。

* **クラウド サービスまたは可用性セット** – RDMA ネットワーク経由で接続するには、A8 および A9 インスタンスを同じクラウド サービス (Azure サービス管理で Linux ベースの VM または Windows ベースの VM を使用する IaaS シナリオ、または Windows Server を使用する PaaS シナリオ用) または同じ可用性セット (Azure リソース マネージャーでの Linux ベースの VM または Windows ベースの VM 用).にデプロイする必要があります。

## HPC Pack を使用する場合の考慮事項

### HPC Pack および Linux を使用する場合の考慮事項

[HPC Pack](https://technet.microsoft.com/library/jj899572.aspx) は、Windows 向け Microsoft の無料 HPC クラスターおよびジョブ管理ソリューションです。 HPC Pack 2012 R2 Update 2 以降、HPC Pack ではいくつかの Linux ディストリビューションを Azure VM にデプロイされ、Windows Server ヘッド ノードで管理されるコンピューティング ノードで実行できるようになりました。 HPC Pack の最新版では、Azure で RDMA ネットワークにアクセスする MPI アプリケーションを実行できる Linux ベースのクラスターをデプロイできます。 詳細については、次を参照してください。、 [Azure で HPC Pack クラスターで Linux 計算ノードを使ってみる](virtual-machines-linux-cluster-hpcpack.md)します。

### HPC Pack および Windows を使用する場合の考慮事項

HPC Pack は、Windows Server で A8、A9、A10、および A11 インスタンスを使用するためには必要ありませんが、Azure で Windows HPC Server ベースのクラスターを作成する場合は推奨されるツールです。 A8 および A9 インスタンスの場合、HPC Pack は、Azure の RDMA ネットワークにアクセスする Windows ベースの MPI アプリケーションを実行する最も効率的な方法です。 HPC Pack には、マイクロソフトによる Windows 用の Message Passing Interface の実装のためのランタイム環境が含まれます。

詳細および Windows Server 上の HPC Pack でコンピューティング集中型インスタンスを使用するためのチェックリストは、「 [MPI アプリケーションを実行する HPC Pack で Windows RDMA クラスターをセットアップ](virtual-machines-windows-hpcpack-cluster-rdma.md)します。

## RDMA ネットワークへのアクセス

### Linux A8 および A9 VM からのアクセス

単一のクラウド サービスまたは可用性セット内では、Linux RDMA ドライバーを使用してインスタンス間の通信を行う MPI アプリケーションが実行されていれば、A8 および A9 インスタンスは Azure の RDMA ネットワークにアクセスできます。 この時点で Azure Linux RDMA はでのみサポート [Intel MPI ライブラリ 5](https://software.intel.com/en-us/intel-mpi-library/)します。
>[AZURE.NOTE] 現在、Azure Linux RDMA ドライバーは、ドライバー拡張機能を介したインストールでは使用できません。 これを使用する唯一の方法は、Azure Marketplace の RDMA 対応 SLES 12 イメージを使用することです。

以下の表に、Linux MPI アプリケーションがコンピューティング ノード (IaaS) のクラスターの RDMA ネットワークにアクセスするための前提条件を記載します。 参照してください [MPI アプリケーションを実行する Linux RDMA クラスターをセットアップ](virtual-machines-linux-cluster-rdma.md) 展開オプションと構成手順を実行します。

 前提条件| 仮想マシン (IaaS)
------------ | -------------
 オペレーティング システム| Azure Marketplace から入手した SLES 12 HPC イメージ
 MPI| Intel MPI Library 5

### Windows A8 および A9 インスタンスからのアクセス

単一のクラウド サービスまたは可用性セット内では、Microsoft Network Direct インターフェイスを使用してインスタンス間の通信を行う MPI アプリケーションが実行されていれば、A8 および A9 インスタンスは Azure の RDMA ネットワークにアクセスできます。 A10 および A11 インスタンスには、RDMA ネットワークへのアクセスは含まれません。

A8 または A9 インスタンスの仮想マシン (IaaS) およびクラウド サービス (PaaS) デプロイメント内の RDMA ネットワークにアクセスするための MPI アプリケーションの要件を、次の表に示します。 一般的な展開シナリオを参照してください。 [MPI アプリケーションを実行する HPC Pack で Windows RDMA クラスターをセットアップ](virtual-machines-windows-hpcpack-cluster-rdma.md)します。


 前提条件| 仮想マシン (IaaS)| Cloud Services (PaaS)
---------- | ------------ | -------------
 オペレーティング システム| Windows Server 2012 R2 または Windows Server 2012| Windows Server 2012 R2、Windows Server 2012、または Windows Server 2008 R2 ゲスト OS ファミリ
 MPI| MS-MPI 2012 R2 またはそれ以降、スタンドアロンまたは HPC Pack 2012 R2 を使用してインストールされているまたはそれ以降<br/><br/>Intel MPI ライブラリ 5| MS-MPI 2012 R2 以降、HPC Pack 2012 R2 を使用して、またはそれ以降がインストールされて<br/><br/>Intel MPI ライブラリ 5

>[AZURE.NOTE]IaaS シナリオの場合、HpcVmDrivers 拡張機能を VM に追加して RDMA 接続に必要な Windows ネットワーク デバイス ドライバーをインストールする必要があります。 デプロイメントの方法によって、サイズ A8 または A9 の VM への HpcVmDrivers 拡張機能の追加は、自動的に行われることも、手動で行う必要があることもあります。 拡張機能を追加するを参照してください。 [管理 VM 拡張機能](virtual-machines-extensions-install.md)します。


## その他の注意事項

* **価格** -A8 ~ A11 の VM のサイズは、標準料金レベルでのみ使用できます。

* **サイズ変更** – A8 ～ A11 以外のサイズのインスタンスを、コンピューティング集中型インスタンス サイズ (A8 ～ 11) のいずれかにサイズ変更することはできません。また、コンピューティング集中型インスタンスを非コンピューティング集中型サイズにサイズ変更することはできません。 これは、コンピューティング集中型インスタンスに固有の特殊なハードウェアとパフォーマンスの最適化のためです。

* **RDMA ネットワーク アドレス空間** -Azure 内の RDMA ネットワークはアドレス空間 172.16.0.0/12 を予約します。 Azure の仮想ネットワークでの A8 および A9 インスタンスで MPI アプリケーションを実行する場合は、RDMA ネットワークに仮想ネットワーク アドレス空間が重複していないことを確認します。

## 次のステップ

* 詳細については、可用性と料金の A8、A9、A10、および A11 インスタンスは、次を参照してください。 [仮想マシンの料金](http://azure.microsoft.com/pricing/details/virtual-machines/) と [クラウド サービスの料金](http://azure.microsoft.com/pricing/details/cloud-services/)します。
* 展開し、Azure RDMA ネットワークへのアクセスの A8 および A9 インスタンスと Linux ベースのクラスターを設定して、次を参照してください。 [MPI アプリケーションを実行する Linux RDMA クラスターをセットアップ](virtual-machines-linux-cluster-rdma.md)します。
* 最初に展開して、Windows 上の HPC Pack での A8 および A9 インスタンスを使用して、次を参照してください。 [MPI アプリケーションを実行する HPC Pack で Windows RDMA クラスターをセットアップ](virtual-machines-windows-hpcpack-cluster-rdma.md)します。





