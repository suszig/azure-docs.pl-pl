<properties
    pageTitle="Site Recovery を使用した異なる Azure リージョン間での Azure IaaS 仮想マシンの移行 | Microsoft Azure"
    description="Azure Site Recovery を使用して、異なる Azure リージョン間で Azure IaaS 仮想マシンを移行します。"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/14/2015"
    ms.author="raynew"/>


# Site Recovery を使用した Azure リージョン間での Azure IaaS 仮想マシンの移行

## 概要

Azure Site Recovery は、さまざまなデプロイ シナリオでの仮想マシンのレプリケーション、フェールオーバー、復旧を調整してビジネス継続性と障害復旧 (BCDR) 戦略に貢献します。 シナリオの「展開の完全な一覧については、 [Azure Site Recovery の概要](site-recovery-overview.md)します。

この記事では、Site Recovery を使用して、異なる Azure リージョン間で Azure IaaS 仮想マシンを移行する方法について説明します。 記事で説明する手順のほとんどを使用する [内部設置型 VMware バーチャル マシンまたは物理サーバーと Azure 間の保護を設定](site-recovery-vmware-to-azure.md)します。 デプロイの各手順の詳細については、そちらの記事をお読みいただくことをお勧めします。

## 作業開始

ここでは、開始する前に必要な項目を示します。

- **構成サーバー**: 構成サーバーとして機能する Azure 仮想マシン。 構成サーバーは、オンプレミスのマシンと Azure サーバー間のやり取りを調整します。
- **マスター ターゲット サーバー**: マスター ターゲット サーバーとして機能する Azure 仮想マシン。 このサーバーは、保護されたコンピューターからレプリケートされたデータを受信して保持します。
- **プロセス サーバー**: Windows Server 2012 R2 を実行する仮想マシン。 保護された仮想マシンは、このサーバーにレプリケーション データを送信します。
- **IaaS 仮想マシン**: 移行する VM。

- でこれらのコンポーネントの詳細を参照 [は何が必要ですか?](site-recovery-vmware-to-azure.md#what-do-i-need)
- 上のガイドラインを参照することも必要があります。 [キャパシティ プランニング](site-recovery-vmware-to-azure.md#capacity-planning) すべてがあるかどうかを確認し、 [展開の前提条件](site-recovery-vmware-to-azure.md#before-you-start) で開始する前にします。

## デプロイの手順

1. [コンテナーを作成します。](site-recovery-vmware-to-azure.md/#step-1-create-a-vault)
2. [構成サーバーのデプロイ](site-recovery-vmware-to-azure.md#step-2-deploy-a-configuration-server) 、Azure VM として。
3. [マスター ターゲット サーバーのデプロイ](site-recovery-vmware-to-azure.md#step-2-deploy-a-configuration-server) 、Azure VM として。
4. [プロセス サーバーのデプロイ](site-recovery-vmware-to-azure.md#step-4-deploy-the-on-premises-process-server)します。 以下の点に注意してください。

    - 同じ仮想ネットワーク/サブネット上に移行する IaaS Vm としてプロセス サーバーを展開する必要があります。 
        ![IaaS VM](./media/site-recovery-migrate-azure-to-azure/ASR_MigrateAzure1.png)

    - プロセス サーバーをデプロイしたら、移行する仮想マシンと通信できることを検証します。
    - 保護する各 VM には、モビリティ サービスがインストールされている必要があります。 このサービスは、プロセス サーバーにデータを送信します。 モビリティ サービスは、手動でインストールまたはプッシュすることができ、VM の保護が有効になっている場合はプロセス サーバーによって自動的にインストールされます。 移行する IaaS 仮想マシンのファイアウォール規則で、このサービスのプッシュ インストールを許可するように構成する必要があります。

    - プロセス サーバーをデプロイし、Site Recovery 資格情報コンテナーで構成サーバーに登録すると、Site Recovery コンソールの **[構成サーバー]** タブに表示されます。 これには最大 15 分かかることがあります。

        ![プロセス サーバー](./media/site-recovery-migrate-azure-to-azure/ASR_MigrateAzure2.png)

5. [最新の更新プログラムをインストール](site-recovery-vmware-to-azure.md#step-5-install-latest-updates)します。 インストールしたすべてのコンポーネント サーバーが最新の状態であることを確認します。
6. [保護グループを作成](site-recovery-vmware-to-azure.md#step-7-create-a-protection-group)します。 Site Recovery を使用して移行された仮想マシンの保護を開始するには、保護グループを設定する必要があります。 グループにレプリケーション設定を指定すると、そのグループに追加するすべてのコンピューターに適用されます。
7. [仮想マシンを設定](site-recovery-vmware-to-azure.md#step-8-set-up-machines-you-want-to-protect)します。 モビリティ サービスを各 VM にインストールする必要があります (自動または手動で)。
8. [手順 8: 仮想マシンの保護を有効にする](site-recovery-vmware-to-azure.md#step-9-enable-protection)します。 VM を保護グループに追加し、VM の保護を有効にします。 以下の点に注意してください。

    - 仮想マシンのプライベート IP アドレスを使用して、Azure に移行する IaaS 仮想マシンを検出できます。 このアドレスは、Azure の仮想マシン ダッシュボードにあります。
    -  [作成した保護グループ] タブで [マシンの追加] をクリックして. > 物理マシン
        ![EC2 の探索](./media/site-recovery-migrate-azure-to-azure/ASR_MigrateAzure3.png)
    - 仮想マシンのプライベート IP アドレスを指定します。
        - ![EC2 の探索](./media/site-recovery-migrate-azure-to-azure/ASR_MigrateAzure4.png)
    - 保護が有効になり、初期レプリケーションが保護グループの初期レプリケーション設定に従って実行されます。
9. [手順 9: 計画外のフェールオーバーを実行](site-recovery-failover.md#run-an-unplanned-failover)します。 初期レプリケーションが完了したら、異なる Azure リージョン間で計画されていないフェールオーバーを実行できます。 必要に応じて、復旧計画を作成し、計画されていないフェールオーバーを実行して、複数の仮想マシンをリージョン間で移行できます。 [詳細](site-recovery-create-recovery-plans.md) 復旧計画についてです。

## 次のステップ

にコメントや質問を投稿 [Site Recovery フォーラム](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)







