<properties 
    pageTitle="Site Recovery: よく寄せられる質問 | Microsoft Azure" 
    description="この記事では、Azure Site Recovery に関してよく寄せられる質問について説明します。" 
    services="site-recovery" 
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags 
    ms.service="get-started-article"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na" 
    ms.workload="storage-backup-recovery"
    ms.date="12/07/2015" 
    ms.author="raynew"/>


# Azure Site Recovery: よく寄せられる質問 (FAQ)
## この記事の内容

この記事には、Site Recovery に関してよく寄せられる質問が含まれます。 昇格よく寄せられる質問を読んだ後に質問がある場合に投稿して、 [Azure Recovery Services フォーラム](https://social.msdn.microsoft.com/Forums/azure/home?forum=hypervrecovmgr)します。


## サポート

###Site Recovery は何をするものですか。

Site Recovery は、オンプレミスの仮想マシンと物理サーバーから Azure またはセカンダリ データセンターへのレプリケーションを調整および自動化することで、ビジネス継続性と障害復旧 (BCDR) 戦略に貢献します。 [詳細については](site-recovery-overview.md)です。


### Site Recovery が保護できるものは何ですか。

- **HYPER-V 仮想マシン**: Site Recovery は、HYPER-V 仮想マシンで実行されているすべてのワークロードを保護できます。 
- **物理サーバー**: Site Recovery は、Windows または Linux を実行する物理サーバーを保護できます。
- **VMware バーチャル マシン**: Site Recovery は、VMware バーチャル マシンで実行されているすべてのワークロードを保護できます。


###保護できる Hyper-V VM は何ですか。

これは、デプロイメント シナリオによって異なります。 

Hyper-V ホスト サーバーの前提条件を以下で確認します。

- [Hyper-V VM を (VMM なしで) Azure にレプリケートする](site-recovery-hyper-v-site-to-azure.md/#before-you-start)
- [Hyper-V VM を (VMM を使って) Azure にレプリケートする](site-recovery-vmm-to-azure.md/#before-you-start)
- [Hyper-V VM をセカンダリ データセンターにレプリケートする](site-recovery-vmm-to-vmm.md/#before-you-start)

ゲスト オペレーティング システムについて:

- セカンダリ データ センターにレプリケートする場合、HYPER-V ホスト サーバーで実行されている Vm のサポート対象のゲスト オペレーティング システムの一覧を参照してください [HYPER-V Vm のゲスト オペレーティング システムをサポートされている](https://technet.microsoft.com/library/mt126277.aspx)します。
- Site Recovery がであるすべてのゲスト オペレーティング システムをサポートしているか、Azure にレプリケートするには、 [Azure でサポートされている](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)します。

Site Recovery は、サポートされている VM で実行されているすべてのワークロードを保護できます。

### クライアント オペレーティング システムで Hyper-V が実行されているときに VM を保護できますか。

いいえ、それはサポートされていません。 代わりに、しなければならない [の物理的なクライアント マシンにレプリケーションを](site-recovery-vmware-to-azure.md) を Azure またはセカンダリ データ センターにします。


### Site Recovery で保護できるワークロードは何ですか。

Site Recovery を使用すると、仮想マシンまたは物理サーバーで実行されているほとんどのワークロードを保護できます。 Site Recovery は、アプリケーションに対応した DR をデプロイするのに役立ちます。 Microsoft などのアプリケーションと、SharePoint、Exchange、Dynamics、SQL Server および Active Directory に統合し、Oracle、SAP、IBM、および Red Hat を含む主要なベンダーとの緊密な動作が特定のアプリケーションごとに、災害復旧ソリューションをカスタマイズできます。 [詳細については](site-recovery-workload.md) ワークロードの保護についてです。


### Hyper-V 仮想マシンを保護するには、System Center VMM サーバーが必ず必要ですか。 

いいえ、できません。 Azure にレプリケートする場合は、HYPER-V ホストは/されていないサーバーで VMM クラウドに配置されている HYPER-V Vm を複製できます。 セカンダリ データセンターにレプリケートする場合は、VMM クラウドで Hyper-V ホスト サーバーを管理する必要があります。 [詳細については、こちらを参照してください。](site-recovery-hyper-v-site-to-azure.md)

### VMM サーバーが 1 つしかない場合、VMM で Site Recovery をデプロイできますか。 

はい。 複製を Azure に VMM サーバー上のクラウドでの HYPER-V Vm も、同じサーバー上の VMM クラウドの間でレプリケートすることができます。 オンプレミス間のレプリケーションには、プライマリ サイトとセカンダリ サイトに VMM サーバーを配置することを強くお勧めします。  [詳細については、こちらを参照してください。](site-recovery-single-vmm.md)

### どの物理サーバーを保護できますか。

Windows および Linux を実行している物理サーバーを保護することができます。 オペレーティング システムの要件は、「 [は何が必要ですか?](site-recovery-vmware-to-azure.md/#what-do-i-need)します。 物理サーバーを Azure にレプリケートする場合も、セカンダリ サイトにレプリケートする場合も、同じ制限が適用されます。

オンプレミスのサーバーがダウンした場合には、物理サーバーは Azure では VM として実行されることに注意してください。 オンプレミスの物理サーバーへのフェールバックは現在サポートされていませんが、Hyper-V または VMware 上で実行されている仮想マシンにフェールバックすることはできます。

### どの VMware VM を保護できますか。

このシナリオでは VMware vCenter サーバー、vSphere ハイパーバイザー、および VMware ツールが実行されている仮想マシンが必要です。 正確な要件を参照してください [は何が必要ですか?](site-recovery-vmware-to-azure.md/#what-do-i-need)します。 物理サーバーを Azure にレプリケートする場合も、セカンダリ サイトにレプリケートする場合も、同じ制限が適用されます。

### 仮想マシンを Azure にレプリケートするための前提条件はありますか。

Azure にレプリケートする仮想マシンに従うか [Azure 要件](site-recovery-best-practices.md/#virtual-machines)します。 

### Hyper-V 第 2 世代仮想マシンを Azure にレプリケートできますか。

はい。 Site Recovery は、フェールオーバー中に第 2 世代から第 1 世代への変換を行います。 フェールバック時に、マシンは第 2 世代に変換し直されます。 [詳細](http://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/)します。

### Azure にレプリケートした場合、Azure VM の支払いはどうなりますか。 

通常のレプリケーション中に、データが地理冗長 Azure Storage にレプリケートされるため、Azure IaaS 仮想マシンの料金は何も支払う必要はありません (すばらしいメリット)。 Azure へのフェールオーバーを実行すると、Site Recovery により、Azure IaaS 仮想マシンが自動的に作成されます。その後、Azure で消費したコンピューティング リソースについて、料金が請求されます。

### Site Recovery を使用してブランチ オフィスの障害復旧を管理できますか。

はい。 ブランチ オフィスでのレプリケーションとフェールオーバーを調整するために Site Recovery を使用すると、1 か所ですべてのブランチ オフィスのワークロードの統一されたオーケストレーションとビューが得られます。 簡単にフェールオーバーを実行し、ブランチ オフィスを移動することがなく、本社からすべての分岐の災害復旧を管理できます。 

### Site Recovery ワークフローを自動化するために利用できる SDK は提供されていますか。

はい。 Rest API、PowerShell、Azure SDK のいずれかを使用して、Site Recovery ワークフローを自動化することができます。 詳細について [PowerShell を使用した Site Recovery のデプロイ](site-recovery-deploy-with-powershell.md)します。


## セキュリティとコンプライアンス

### Site Recovery サービスにデータが送信されますか。

いいえ。Site Recovery がお客様のアプリケーション データをインターセプトしたり、仮想マシンまたは物理サーバーで実行されていることに関するいかなる情報も持つことはありません。

レプリケーション データは、Hyper-V ホスト間、VMware ハイパーバイザー間、プライマリ データセンターとセカンダリ データセンター内の物理サーバー間、あるいはデータセンターと Azure ストレージ間でやり取りされます。 Site Recovery には、これらのデータをインターセプトする能力はありません。 レプリケーションとフェールオーバーを調整するために必要なメタデータのみが、Site Recovery サービスに送信されます。 

Site Recovery は ISO 27001:2005 の認証を受けており、HIPAA、DPA、および FedRAMP JAB の評価が進行中です。

### コンプライアンスのため、オンプレミス環境からのメタデータであっても、同じ地理的リージョン内に維持される必要があります。 Site Recovery は役立ちますか。

はい。 リージョンに Site Recovery コンテナーを作成すると、レプリケーションとフェールオーバーを有効にし、調整するために必要なすべてのメタデータが、そのリージョンの地理的境界内に維持されます。

### Site Recovery はレプリケーションを暗号化しますか。
オンプレミスのサイト間で仮想マシンと物理サーバーをレプリケートする場合には、転送中の暗号化がサポートされます。 オンプレミスのサイトと Azure 間で仮想マシンと物理サーバーをレプリケートする場合には、転送中の暗号化と Azure での暗号化の両方がサポートされます。 




## レプリケーションとフェールオーバー

### Azure にレプリケートする場合、どの種類のストレージ アカウントが必要ですか。

ストレージ アカウントが必要 [標準の geo 冗長ストレージ](../storage/storage-redundancy.md/#geo-redundant-storage)します。 A [premium storage アカウント](../storage/storage-premium-storage-preview-portal/) VMware 仮想マシンまたは Windows/linux 物理サーバーを Azure にレプリケートする場合にのみサポートされます。   

標準的なローカル冗長ストレージのサポートがバックログには、この機能についてのフィードバックを送信、 [フィードバック フォーラム](http://feedback.azure.com/forums/256299-site-recovery/suggestions/7204469-local-redundant-type-azure-storage-support)します。

### どのくらいの頻度でデータをレプリケートできますか。
- **Hyper-v:** HYPER-V Vm ごとの 30 秒、5 分間、15 分がレプリケートされることができます。 SAN レプリケーションを設定した場合は、レプリケーションと同期します。
- **VMware と物理サーバー:** レプリケーションの頻度はここで無効です。 レプリケーションは継続的に行われます。 

### 既存の復旧サイトから第 3 のサイトにレプリケーションを拡張することができますか。
拡張またはチェーン レプリケーションはサポートされていません。 この機能についてのフィードバックを送信 [フィードバック フォーラム](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959-support-for-exisiting-extended-replication)します。


### Azure に初めてレプリケートする際に、オフライン レプリケーションを行うことができますか。 

これはサポートされていません。 この機能についてのフィードバックの送信、 [フィードバック フォーラム](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from)します。


### レプリケーションから特定のディスクを除外することはできますか。

これはサポートされていません。 この機能についてのフィードバックの送信、 [フィードバック フォーラム](http://feedback.azure.com/forums/256299-site-recovery/suggestions/6418801-exclude-disks-from-replication)します。

### ダイナミック ディスクの仮想マシンをレプリケートすることができますか。

ダイナミック ディスクは Hyper-V 仮想マシンのレプリケート時にサポートされますが、 VMware 仮想マシンまたは物理サーバーをレプリケートする際にはサポートされません。 この機能についてのフィードバックの送信、 [フィードバック フォーラム](http://feedback.azure.com/forums/256299-site-recovery)します。

### Azure にフェールオーバーする場合、フェールオーバー後に Azure の仮想マシンにどうしたらアクセスできますか。 

Azure VM には、セキュリティで保護されたインターネット接続を経由して、またはある場合には、サイト間 VPN (または Azure ExpressRoute) 経由でアクセスできます。 VPN 接続を経由した通信は、VM が配置されている Azure ネットワーク上の内部ポートにマップされます。 インターネットを経由した通信は、VM の Azure クラウド サービスのパブリック エンドポイントにマップされます。 [詳細については、こちらを参照してください。](site-recovery-network-design.md/#connectivity-after-failover)

### Azure にフェールオーバーする場合、Azure はどのようにデータの回復力を確認しますか。

Azure はサービスを復元するように設計されています。 Site Recovery は、必要に応じて、Azure SLA に従ってセカンダリ Azure データセンターにフェールオーバーする機能を既に備えています。 これが発生した場合には、お客様のメタデータとコンテナーは、お客様が選択したコンテナーと同じリージョン内に保持されます。 

### 2 つのデータ センター間でレプリケートしている場合、プライマリ データセンターに予期しない障害が発生するとどうなりますか。

セカンダリ サイトから計画外のフェールオーバーをトリガーすることができます。 Site Recovery は、フェールオーバーを実行するためにプライマリ サイトからの接続を必要としません。


### フェールオーバーは自動で行われますか。

フェールオーバーは自動では行われません。 ポータルで、シングル クリックでのフェールオーバーを開始することも行うこともできます [Site Recovery の PowerShell](https://msdn.microsoft.com/library/dn850420.aspx) へのフェールオーバーをトリガーします。 Site Recovery ポータルではフェールバックも簡単な操作で行えます。 自動化するには、オンプレミスの Orchestrator または Operations Manager を使用すると、仮想マシンのエラーを監視し、SDK を使用してフェールオーバーをトリガーできます。

### Hyper-V VM をレプリケートする場合、Hyper-V レプリケーション トラフィックに割り当てられた帯域幅を調整できますか。
- Hyper-V VM を 2 つのオンプレミス サイト間でレプリケートする場合、Windows QoS を使用できます。 サンプル スクリプトを次に示します。 

        New-NetQosPolicy -Name ASRReplication -IPDstPortMatchCondition 8084 -ThrottleRate (2048*1024)
        gpupdate.exe /force

- Hyper-V VM を Azure にレプリケートする場合は、次のサンプル PowerShell コマンドレットを使用して調整を構成できます。

        Set-OBMachineSetting -WorkDay $mon, $tue -StartWorkHour "9:00:00" -EndWorkHour "18:00:00" -WorkHourBandwidth (512*1024) -NonWorkHourBandwidth (2048*1024)



## サービス プロバイダーのデプロイメント

### Site Recovery は、専用または共有のインフラストラクチャ モデルで動作しますか。
はい。Site Recovery は、専用および共有のインフラストラクチャ モデルの両方をサポートしています。

### テナントの ID は Site Recovery サービスと共有されますか。
いいえ、できません。 実際には、ユーザーのテナントでは、Site Recovery ポータルへのアクセスは必要ありません。 サービス プロバイダーの管理者だけが、Site Recovery ポータルでアクションを実行します。


### テナントのアプリケーション データは Azure に送信されますか。
サービス プロバイダーが所有するサイト間でレプリケートするときに、アプリケーション データが Azure に送信されることはありません。 データは転送中に暗号化され、サービス プロバイダーのサイト間で直接レプリケートされます。

Azure にレプリケートする場合、アプリケーション データは Site Recovery サービスではなく、Azure ストレージに送信されます。 データは、転送中に暗号化され、暗号化された状態のまま Azure に残ります。 

### テナントに Azure サービスの請求書が届きますか。

いいえ、できません。 Azure の請求関係は、サービス プロバイダーと直接です。 サービス プロバイダーは、そのテナントに対して固有の請求書を生成する必要があります。

### Azure にレプリケートする場合、仮想マシンを Azure で常時実行する必要がありますか。

いいえ。データは、地理冗長 Azure ストレージ アカウントにお客様のサブスクリプションでレプリケートされます。 テスト フェールオーバー (DR の訓練) または実際のフェールオーバーを実行すると、Site Recovery は自動的に仮想マシンをお客様のサブスクリプションで作成します。

### Azure にレプリケートする際に、テナント レベルでの隔離は保証されますか。

はい。

### 現在、どのプラットフォームがサポートされていますか。

Azure パック、クラウド プラットフォーム システム、および System Center ベース (Hyper 2012 以降) のデプロイメントがサポートされています。 Azure Pack と Site Recovery との統合の詳細について [仮想マシンの保護を構成する](https://technet.microsoft.com/library/dn850370.aspx)です。

### 単一の Azure パックおよび単一の VMM サーバーのデプロイメントもサポートされますか。

はい。Hyper-V 仮想マシンと Azure のレプリケート、またはサービス プロバイダー サイト間のレプリケートができます。  サービス プロバイダー サイト間でレプリケートする場合は、Azure Runbook 統合が使用できないことに注意してください。


## 次のステップ

- 読み取り、 [Site Recovery の概要](site-recovery-overview.md)
- について学習 [Site Recovery のアーキテクチャ](site-recovery-components.md)  

 


