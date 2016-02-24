<properties
    pageTitle="Azure Site Recovery で VMM で HYPER-V 仮想マシンの保護のネットワーク マッピングを準備する |Microsoft Azure"
    description="オンプレミスのデータセンターから Azure またはセカンダリ サイトに Hyper-V 仮想マシンをレプリケートするためのネットワーク マッピングを設定します。"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="12/01/2015"
    ms.author="raynew"/>


# Azure Site Recovery で VMM を使用して Hyper-V 仮想マシンを保護するために、ネットワーク マッピングを準備する

Azure Site Recovery は、仮想マシンと物理サーバーのレプリケーション、フェールオーバー、復旧を調整してビジネス継続性と障害復旧 (BCDR) 戦略に貢献します。 

この記事では、2 つのオンプレミス データセンター間またはオンプレミスのデータセンターと Azure の間で、Site Recovery を使って VMM クラウド内の Hyper-V 仮想マシンをレプリケートする場合に、ネットワーク設定を最適に構成するのに役立つネットワーク マッピングについて説明します。 VMM クラウドを使用していない Hyper-V VM をレプリケートする場合、または VMware VM や物理サーバーをレプリケートする場合は、この記事の内容は当てはまりません。 

この記事の投稿について質問を読んだ後、 [Azure Recovery Services フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## 概要

ネットワーク マッピングは、Hyper-V レプリカまたは SAN レプリケーションを使用して Azure またはセカンダリ データセンターに Hyper-V 仮想マシンをレプリケートするために Azure Site Recovery をデプロイするときに使用します。 

- **VMM クラウド内の HYPER-V 仮想マシンをレプリケートする 2 つのオンプレミス データ センター**— ネットワーク マッピングでは、次の操作をターゲット VMM サーバー上の VM ネットワークと、ソース VMM サーバー上の VM ネットワーク間。

    - **フェールオーバー後に仮想マシンを接続**— 仮想マシンがフェールオーバー後に適切なネットワークに接続することを確認します。 レプリカ仮想マシンは、ソース ネットワークにマップされているターゲット ネットワークに接続されます。
    - **ホスト サーバー上のレプリカ仮想マシンの配置**-HYPER-V ホスト サーバーにレプリカ仮想マシンを最適に配置します。 レプリカ仮想マシンは、マップされた VM ネットワークにアクセスできるホストに配置されます。
    - **ネットワーク マッピングなし**— レプリケートされた仮想マシンをフェールオーバー後の VM ネットワークに接続しないにネットワーク マッピングを構成していない場合。

- **内部設置型 VMM クラウド内の HYPER-V 仮想マシンを Azure にレプリケートする**— ネットワーク マッピングでは、ソース VMM サーバー上の VM ネットワークとターゲット Azure ネットワークを次の操作します。
    - **フェールオーバー後に仮想マシンを接続**-すべてのマシンが同じネットワーク上でフェールオーバーするが、相互に関係なく、どの復旧計画されているに接続できます。
    - **ネットワーク ゲートウェイ**-その他の内部設置型仮想マシンに仮想マシンが接続できる場合は、ターゲット Azure ネットワークでネットワーク ゲートウェイを設定します。
    - **ネットワーク マッピングなし**— ネットワーク マッピングを構成しない場合の仮想マシンのみ、同じ復旧計画でフェールオーバーができるようになります azure 経由でフェールオーバー後の相互に接続します。


## ネットワーク マッピングの例

ネットワーク マッピングは、2 つの VMM サーバー上の VM ネットワーク間、または 2 つのサイトが同じサーバーによって管理されている場合は、単一の VMM サーバー上の VM ネットワーク間に構成できます。 マッピングを正しく構成し、レプリケーションを有効にすると、1 次拠点にある仮想マシンはネットワークに接続され、ターゲットの拠点にあるそのレプリカは、マップされたネットワークに接続されます。

VMM でネットワークが正しくセットアップされている場合、ネットワーク マッピング中に、ターゲット VM ネットワークを選択すると、ソース VM ネットワークを使用する VMM ソース クラウドが表示されます。また、保護に使用されるターゲット クラウド上の使用可能なターゲット VM ネットワークも表示されます。

このメカニズムを説明するために、次に例を示します。 ニューヨークとシカゴという 2 つの拠点がある組織を例にします。

**Location (場所)** | **VMM サーバー** | **VM ネットワーク** | **マップ先**
---|---|---|---
ニューヨーク | VMM-NewYork| VMNetwork1-NewYork | VMNetwork1-Chicago にマップされています
 |  | VMNetwork2-NewYork | マッピングなし
シカゴ | VMM-Chicago| VMNetwork1-Chicago | VMNetwork1-NewYork にマップされています
 | | VMNetwork1-Chicago | マッピングなし

この例では、次の点に注意してください。

- VMNetwork1-NewYork に接続されている仮想マシンに対してレプリカ仮想マシンが作成されると、そのレプリカ仮想マシンは VMNetwork1-Chicago に接続されます。
- VMNetwork2-NewYork または VMNetwork2-Chicago 対してレプリカ仮想マシンが作成されると、そのレプリカ仮想マシンはどのネットワークにも接続されません。

サンプル組織で VMM クラウドがどのように設定され、論理ネットワークがクラウドにどのように関連付けられるかを次に示します。

### クラウドの保護設定

**保護されたクラウド** | **保護するクラウド** | **論理ネットワーク (ニューヨーク)**  
---|---|---
GoldCloud1 | GoldCloud2 |
SilverCloud1| SilverCloud2 |
GoldCloud2 | <p>該当なし</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>
SilverCloud2 | <p>該当なし</p><p></p> | <p>LogicalNetwork1-NewYork</p><p>LogicalNetwork1-Chicago</p>

### 論理設定と VM ネットワークの設定

**Location (場所)** | **論理ネットワーク** | **関連付けられた VM ネットワーク**
---|---|---
ニューヨーク | LogicalNetwork1-NewYork | VMNetwork1-NewYork
シカゴ | LogicalNetwork1-Chicago | VMNetwork1-Chicago
 | LogicalNetwork2-Chicago | VMNetwork2-Chicago

### ターゲット ネットワーク

これらの設定に基づき、次の表は、ターゲット VM ネットワークを選択する場合に使用可能な選択肢を示します。

**[** | **保護されたクラウド** | **保護するクラウド** | **使用可能なターゲット ネットワーク**
---|---|---|---
VMNetwork1-Chicago | SilverCloud1 | SilverCloud2 | 使用可能
 | GoldCloud1 | GoldCloud2 | 使用可能
VMNetwork2-Chicago | SilverCloud1 | SilverCloud2 | 使用できません。
 | GoldCloud1 | GoldCloud2 | 使用可能



## 複数のサブネット

ターゲット ネットワークに複数のサブネットがあり、そのサブネットのいずれかが、ソースの仮想マシンが配置されているサブネットと同じ名前である場合、レプリカの仮想マシンは、フェールオーバー後にそのターゲット サブネットに接続されます。 ターゲットのサブネットで名前が一致するものがなければ、仮想マシンはネットワークの最初のサブネットに接続されます。


### フェールバック

フェールバック (レプリケーションの反転) の場合の動作を確認するために、次の設定で VMNetwork1-NewYork が VMNetwork1-Chicago にマップされていると仮定します。


**仮想マシン** | **接続先の VM ネットワーク**
---|---
VM1 | VMNetwork1-NewYork
VM2 (VM1 のレプリカ) | VMNetwork1-Chicago

これらの設定で、いくつかの可能なシナリオでの動作を確認しましょう。

**シナリオ** | **結果**
---|---
フェールオーバー後に VM-2 のネットワークのプロパティの変更がない。 | VM-1 は、ソース ネットワークに接続されたままです。
VM-2 のネットワークのプロパティがフェールオーバー後に変更され、VM-2 が切断される。 | VM-1 が切断されます。
VM-2 のネットワークのプロパティがフェールオーバー後に変更され、VM-2 が VMNetwork2-Chicago に接続される。 | VMNetwork2-Chicago がマップされていない場合、VM-1 は切断されます。
VMNetwork1-Chicago のネットワークのマッピングが変更される。 | VM-1 は VMNetwork1-Chicago にマップされたネットワークに接続されます。


## 次のステップ

ネットワーク マッピングの理解を深めるしたら [Site Recovery のデプロイを使ってみる](site-recovery-best-practices.md)します。

