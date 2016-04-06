<properties
    pageTitle="Site Recovery のデプロイの準備 | Microsoft Azure"
    description="Azure Site Recovery は、オンプレミスのサーバーに配置されている仮想マシンと物理サーバーの Azure またはセカンダリ データセンターへのレプリケーション、フェールオーバー、および復旧を調整します。"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="12/07/2015"
    ms.author="raynew"/>

# Azure Site Recovery のデプロイの準備

## この記事の内容

この記事では、Azure Site Recovery をデプロイするための準備の方法について説明します。 質問内容がある場合は、この記事に投稿して、 [Azure Recovery Services フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)します。 

## Hyper-V 仮想マシンの保護

Hyper-V 仮想マシンを保護するためのデプロイとしていくつかのオプションがあります。 オンプレミス ハイパー-仮想マシンは、Azure、またはセカンダリ データ センターにレプリケートできます。 各デプロイには、要件に次のような違いがあります。

**要件** | **Azure にレプリケート (VMM を使用)** | **Azure に Hyper-V VM をレプリケート (VMM なし)** | **セカンダリ サイトに Hyper-V VM をレプリケート (VMM を使用)** | **詳細**
---|---|---|---|---
**VMM** | System Center 2012 R2 で実行されている VMM <br/><br/>1 つまたは複数の VMM ホスト グループを含む 1 つ以上の VMM クラウド | 該当なし | 最新の更新プログラムが適用された少なくとも 1 つの System Center 2012 SP1 で実行されている、プライマリおよびセカンダリ サイト内の VMM サーバー。 <br/><br/> 各 VMM サーバー上のクラウドを少なくとも 1 つです。 クラウドは HYPER-V キャパシティ プロファイル セットが必要です。<br/><br/> ソース クラウドには、少なくとも 1 つの VMM ホスト グループが必要です。 | 省略可能。 Hyper-V 仮想マシンを Azure にレプリケートする上で System Center VMM をデプロイする必要はありません。デプロイした場合は、VMM サーバーが正しくセットアップされていることを確認する必要があります。 具体的には、適切な VMM バージョンを実行しているかどうか、クラウドがセットアップされているかどうか、などを確認します。
**Hyper-V** | Windows Server 2012 R2 以降を実行している、オンプレミスのデータ センター内の少なくとも 1 つの Hyper-V ホスト サーバー | 以降を実行するソースとターゲットのサイトで 1 つ以上の HYPER-V サーバー Windows Server 2012 R2 では、インターネットに接続されているとします。<br/><br/> Hyper-V サーバーは、VMM クラウド内のホスト グループに属している必要があります。 | 以降を実行するソースとターゲットのサイトで 1 つ以上の HYPER-V サーバーを最新の更新と Windows Server 2012 がインストールされ、インターネットに接続します。<br/><br/> Hyper-V サーバーは、VMM クラウド内のホスト グループに属している必要があります。 | 
**仮想マシン** | ソース Hyper-V ホスト サーバー上の少なくとも 1 つの VM | ソース VMM クラウド内の Hyper-V ホスト サーバーにある少なくとも 1 つの VM | ソース VMM クラウド内の Hyper-V ホスト サーバーにある少なくとも 1 つの VM。 |  Azure にレプリケートする Vm が準拠する必要があります [Azure の仮想マシンの前提条件](site-recovery-best-practices.md/#virtual-machines)
**Azure アカウント** | 必要があります、 [Azure](http://azure.microsoft.com/) アカウントと、Site Recovery サービスへのサブスクリプション。 | 必要があります、 [Azure](http://azure.microsoft.com/) アカウントと、Site Recovery サービスへのサブスクリプション。 | 該当なし | アカウントを持っていない場合は始めて、 [無料評価版](pricing/free-trial/)します。 について [料金](pricing/details/site-recovery/) サービス用です。
**Azure Storage** | geo レプリケーションが有効になっている Azure ストレージ アカウントのサブスクリプションが必要です。 | geo レプリケーションが有効になっている Azure ストレージ アカウントのサブスクリプションが必要です。 | 該当なし | アカウントは Azure Site Recovery コンテナーと同じリージョンである必要があり、同じサブスクリプションに関連付けられている必要があります。 [詳細については、記憶域](../storage/storage-introduction.md)します。
**ストレージ マッピング** | 該当なし | 該当なし | 仮想マシンがフェールオーバー後に最適な状態で確実にストレージに接続されるように、ストレージ マッピング オプションを設定できます。 2 つのオンプレミス VMM サイト間でレプリケートする場合、既定では、レプリカの仮想マシンは、ターゲットの Hyper-V ホスト サーバー上の指定された場所に格納されます。 ソースとターゲットの VMM サーバーにある、VMM ストレージの分類間でのマッピングを構成することができます。  | この機能を使用するには、デプロイを開始する前にストレージの分類を設定する必要があります。 [詳細](site-recovery-storage-mapping.md)します。
**SAN レプリケーション** | 該当なし | 該当なし | 2 つのオンプレミス VMM サイト間で SAN レプリケーションを使用してレプリケートする場合は、既存の SAN 環境を使用することができます。 | 必要があります、 [SAN アレイがサポートされている](http://social.technet.microsoft.com/wiki/contents/articles/28317.deploying-azure-site-recovery-with-vmm-and-san-supported-storage-arrays.aspx), 、および SAN 記憶域を検出し、VMM で分類する必要があります。<br/><br/>現在ではない場合、Lun を作成し、VMM コンソールで記憶域の割り当てする必要がありますレプリケートを実行します。 既にレプリケートを実行している場合、この手順はスキップできます。
**ネットワーク** | ネットワーク マッピングをセットアップすると、属している復旧計画の種類にかかわらず、同じ Azure ネットワーク上でフェールオーバーするすべての仮想マシンが相互に通信できるようになります。 さらに、ネットワーク ゲートウェイがターゲットの Azure ネットワーク上で構成されている場合、仮想マシンは他のオンプレミス仮想マシンに接続できます。 ネットワーク マッピングを設定しない場合は、同じ復旧計画に属する、フェールオーバーするマシンのみが接続できます。 | 該当なし |  <br/><br/>ネットワーク マッピングを設定すると、フェールオーバー後に、仮想マシンが適切なネットワークに接続し、レプリカ仮想マシンが HYPER-V ホスト サーバーへ最適に配置することを確認します。 ネットワーク マッピングを構成しない場合、フェールオーバー後に、レプリケートされたマシンは、どの VM ネットワークにも接続されません。 | [詳細](site-recovery-network-mapping) ネットワーク マッピングに関するします。 <br/><br/> VMM でネットワーク マッピングを設定するように、VMM 論理必要があり、VM ネットワークが正しく構成されています。 [詳細については](http://blogs.technet.com/b/scvmm/archive/2013/02/14/networking-in-vmm-2012-sp1-logical-networks-part-i.aspx) と [VM ネットワーク](https://technet.microsoft.com/library/jj721575.aspx)します。 また「 [VMM の考慮事項をネットワーク](site-recovery-network-design.md/#vmm-design)します。  
**プロバイダーおよびエージェント** | デプロイ時には、VMM サーバーに Azure Site Recovery プロバイダーをインストールします。 VMM クラウド内にある Hyper-V サーバー上で、Azure Recovery Services エージェントをインストールします。 | デプロイ時には、Azure Site Recovery プロバイダーと Azure Recovery Services エージェントの両方を、Hyper-V ホスト サーバーまたはクラスター ノードにインストールします。| デプロイ時には、VMM サーバーに Azure Site Recovery プロバイダーをインストールします。 VMM クラウド内にある Hyper-V サーバー上で、Azure Recovery Services エージェントをインストールします。 | プロバイダーとエージェントは、インターネット経由で、暗号化された HTTPS 接続を使用して Site Recovery に接続されます。 ファイアウォールの例外を追加するか、プロバイダー接続の特定のプロキシを作成する必要はありませんが、カスタム プロキシを使用する場合の展開を開始する前に、ファイアウォール経由のこれらの Url を許可する必要があります。 <br/><br/>  *.hypervrecoverymanager.windowsazure.com <br/><br/> *.accesscontrol.windows.net<br/><br/> *.backup.windowsazure.com <br/><br/> *。 >.blob.core.windows.net <br/><br/>*。 store.core.windows.net
**インターネット接続** | VMM サーバーのみでインターネット接続が必要 | HYPER-V ホスト サーバーのみでインターネット接続が必要 | VMM サーバーのみでインターネット接続が必要 | 仮想マシンにインストールされているものは不要し、インターネットに直接接続しないでください。



## VMware 仮想マシンまたは物理サーバーの保護

VMware 仮想マシンまたは Windows/Linux 物理サーバーを保護するデプロイ オプションがいくつかがあります。 Azure、またはセカンダリ データ センターにレプリケートできます。 各デプロイには、要件に次のような違いがあります。

**要件** | **Azure に VMware VM/物理サーバーをレプリケート** | * **セカンダリ サイトにレプリケート VMware Vm/物理サーバー**  
---|---|--- 
**プライマリ サイト** | **プロセス サーバー**: 専用の Windows サーバー (物理または仮想) | **プロセス サーバー**: (物理または VMware バーチャル マシン専用の Windows サーバー<br/><br/>  
**セカンダリ オンプレミス サイト** | 該当なし | **構成サーバー**: 専用の Windows サーバー (物理または仮想) <br/><br/> **マスター ターゲット サーバー**: 専用のサーバー (物理または仮想)。 Windows マシンを保護するには Windows で、Linux マシンを保護するには Linux で構成を行います。
**Azure** | **サブスクリプション**: Site Recovery サービスのサブスクリプションが必要です。 ご覧 [料金](pricing/details/site-recovery/) <br/><br/> **ストレージ アカウント**: ストレージ アカウントを geo レプリケーションを有効になっている必要があります。 アカウントは Site Recovery コンテナーと同じリージョンである必要があり、同じサブスクリプションに関連付けられている必要があります。 [詳細](../storage/storage-introduction.md)します。<br/><br/> **構成サーバー**: Azure VM として、構成サーバーを設定する必要があります <br/><br/> **マスター ターゲット サーバー**: Azure VM としてマスター ターゲット サーバーを設定する必要があります <br/><br/> Windows マシンを保護するには Windows で、Linux マシンを保護するには Linux で構成を行います。<br/><br/> **Azure の仮想ネットワーク**: Azure の仮想ネットワークが、構成サーバーとマスター ターゲット サーバーが展開する必要があります。 Azure 仮想ネットワークは、Azure Site Recovery コンテナーと同じリージョンである必要があり、同じサブスクリプションに関連付けられている必要があります。 | 該当なし  
**仮想マシン/物理サーバー** | 少なくとも 1 つの VMware バーチャル マシンまたは物理 Windows または Linux サーバーです。<br/><br/>展開時に、モビリティ サービスを各コンピューターにインストールされます。| 少なくとも 1 つの VMware VM または Windows または Linux の物理サーバーです。<br/><br/> デプロイ時には、各マシンに統合エージェントがインストールされます。




## Azure 仮想マシンの要件

Site Recovery をデプロイすると、Azure でサポートされた任意のオペレーティング システムを実行している仮想マシンまたは物理サーバーをレプリケートできます。 これには、Windows と Linux のほとんどのバージョンが含まれます。 保護するオンプレミスの仮想マシンが Azure 要件を満たしていることを確認する必要があります。


**機能** | **サポート** | **詳細**
---|---|---
Hyper-V ホスト オペレーティング システム | Windows Server 2012 R2 | サポートされていない場合、前提条件の確認は失敗します。
VMware ハイパーバイザー オペレーティング システム | サポートされたオペレーティング システムの実行 | [詳細](site-recovery-vmware-to-azure.md/#before-you-start) 
ゲスト オペレーティング システム |  Azure に HYPER-V は、Site Recovery のレプリケーションはすべてのオペレーティング システムをサポートしています。 [Azure でサポートされている](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)します。 <br/><br/> VMware と物理サーバーのレプリケーションを確認して、Windows と Linux の [の前提条件](site-recovery-vmware-to-azure.md/#before-you-start) | サポートされていない場合、前提条件の確認は失敗します。 
ゲスト オペレーティング システムのアーキテクチャ | 64 ビット | サポートされていない場合、前提条件の確認は失敗します。
オペレーティング システムのディスク サイズ |  最大 1023 GB | サポートされていない場合、前提条件の確認は失敗します。
オペレーティング システムのディスク数 | 1 | サポートされていない場合、前提条件の確認は失敗します。 
データ ディスク数 | 16 以下 (最大値は、作成される仮想マシンのサイズの関数であり、 16 = XL で求められます。) | サポートされていない場合、前提条件の確認は失敗します。
データ ディスク VHD のサイズ | 最大 1023 GB | サポートされていない場合、前提条件の確認は失敗します。
ネットワーク アダプター | 複数のアダプターがサポートされます。 |
静的 IP アドレス | サポートされています | プライマリ仮想マシンが静的 IP アドレスを使用している場合、Azure で作成される仮想マシンに静的 IP アドレスを指定できます。
iSCSI ディスク | サポートされていません | サポートされていない場合、前提条件の確認は失敗します。
共有 VHD | サポートされていません | サポートされていない場合、前提条件の確認は失敗します。
FC ディスク | サポートされていません | サポートされていない場合、前提条件の確認は失敗します。
ハード ディスク フォーマット| VHD <br/><br/> VHDX | VHDX は現在、Azure でサポートされていませんが、Azure にフェールオーバーするとき、Site Recovery が VHDX を VHD に自動的に変換します。 オンプレミスにフェールバックした場合、仮想マシンは引き続き VHDX 形式を使用します。
仮想マシン名| 1 ～ 63 文字で指定します。 名前に使用できるのは、英文字、数字、およびハイフンのみです。 名前の先頭と末尾は、英文字または数字である必要があります。 | Site Recovery で仮想マシンのプロパティの値を更新します。
仮想マシンのタイプ | <p>第 1 世代</p> <p>第 2 世代 - Windows</p> |  OS ディスク タイプがベーシック ディスクの第 2 世代仮想マシンがサポートされています。この OS ディスクは、ディスク フォーマットが VHDX で、1 つまたは 2 つのデータ ボリュームを含み、300 GB 未満であることが必要です。 Linux の第 2 世代仮想マシンはサポートされていません。 [詳細についてはこちら](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)



## デプロイの最適化

次のヒントは、デプロイの最適化およびスケーリングの際に役立ちます。

- **オペレーティング システムのボリューム サイズ**: Azure に仮想マシンをレプリケートするときに、オペレーティング システムのボリュームが 1 TB より小さくをする必要があります。 これよりもボリュームが大きい場合は、デプロイを開始する前に、手動で別のディスクに移動することができます。
- **データ ディスクのサイズ**: Azure にレプリケートする場合は、最大 1 TB の各仮想マシンに 32 個のデータ ディスクを持つことができます。 これにより、最大 32 TB の仮想マシンのレプリケートとフェールオーバーを効率的に行うことができます。
- **復旧計画の制限**: Site Recovery は、数千のバーチャル マシンにスケール アップできます。 復旧計画は、ひとまとまりでフェールオーバーする必要があるアプリケーション向けのモデルとして設計されているため、復旧計画で用いるマシン数は 50 個までに制限されています。
- **Azure サービスの制限**: すべての Azure サブスクリプションにコアや一連の既定の制限ではクラウド サービスなどです。サブスクリプション内のリソースの可用性については、テスト フェールオーバーを実行して検証することをお勧めします。 これらの制限は、Azure サポートを使用して変更することができます。
- **キャパシティ プランニング**: 読み取り、 [キャパシティ プランナー](http://www.microsoft.com/download/details.aspx?id=39057) HYPER-V 仮想マシンをレプリケートする場合。
- **レプリケーションの帯域幅**: レプリケーションの帯域幅が不足する場合は注意してください。
    - **ExpressRoute**: Site Recovery は、Riverbed など Azure ExpressRoute や WAN オプティマイザーと連携します。 [詳細](http://blogs.technet.com/b/virtualization/archive/2014/07/20/expressroute-and-azure-site-recovery.aspx) ExpressRoute についてです。
    - **レプリケーション トラフィック**: Site Recovery はデータ ブロックのみ、VHD 全体を使用してスマート初期レプリケーションを実行します。 レプリケーション進行中にレプリケートされるのは、変更箇所のみです。
    - **ネットワーク トラフィックを**: レプリケーションの設定で使用されるネットワーク トラフィックを制御する [Windows QoS](https://technet.microsoft.com/library/hh967468.aspx) 送信先 ip アドレスに基づいてポリシーを使用してアドレスし、ポートします。  さらに、Azure Backup エージェントを使用して、Azure Site Recovery にレプリケートする場合には、 そのエージェントの調整を構成することができます。 [詳細](https://support.microsoft.com/kb/3056159)します。
- **RTO**: Site Recovery と予期できる回復時間目標 (RTO) を測定する場合はお勧めテスト フェールオーバーを実行して、表示量を分析するサイトの回復ジョブは、かかる時間を操作を完了します。 Azure にフェールオーバーする場合、最適な RTO を実現するには、Site Recovery を Azure Automation と復旧計画に統合して、すべての手動操作を自動化することをお勧めします。
- **RPO**: Site Recovery は、Azure にレプリケートする場合、ほぼ同期の回復ポイントの目標 (RPO) をサポートしています。 これは、データセンターと Azure の間に十分な帯域幅があることを前提としています。





## 次のステップ

これらのベスト プラクティスを確認した後に、Site Recovery のデプロイを開始できます。

- [オンプレミスの VMM サイトと Azure 間の保護の設定](site-recovery-vmm-to-azure.md)
- [オンプレミスの Hyper-V サイトと Azure 間の保護の設定](site-recovery-hyper-v-site-to-azure.md)
- [Set up protection between two on-premises VMM sites (2 つのオンプレミスの VMM サイト間の保護の設定)](site-recovery-vmm-to-vmm.md)
- [Set up protection between two on-premises VMM sites with SAN (SAN を使用した 2 つのオンプレミスの VMM サイト間の保護の設定)](site-recovery-vmm-san.md)
- [単一の VMM サーバーを使用した保護の設定](site-recovery-single-vmm.md)
 


