<properties
    pageTitle="Site Recovery とは | Microsoft Azure" 
    description="Azure Site Recovery は、オンプレミスに配置されている仮想マシンと物理サーバーの Azure またはセカンダリ オンプレミス サイトへのレプリケーション、フェールオーバー、および復旧を調整します。" 
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
    ms.date="12/14/2015" 
    ms.author="raynew"/>

#  Site Recovery とは

Site Recovery は、オンプレミスのセカンダリ データセンターまたは Azure へのオンプレミス サーバーと仮想マシンのレプリケーションを調整することにより、ビジネス継続性と障害復旧 (BCDR) の戦略を支援する Azure サービスです。 Site Recovery でレプリケーションが処理されるため、クリックするだけでフェールオーバーと復旧を開始することができます。 よく寄せられる質問の一覧を読み取り、 [よく寄せられる質問](site-recovery-faq.md)


## Site Recovery を使用する理由 

- **単純な BCDR ストーリー**-Site Recovery では、レプリケーション、フェールオーバーと復旧を内部設置型のワークロードとアプリケーションを扱いやすくします。
- **柔軟なレプリケーション**-内部設置型サーバー、HYPER-V 仮想マシンおよび VMware バーチャル マシンをレプリケートすることができます。  Site Recovery は、最初のレプリケーションに VHD 全体ではなくデータのブロックのみをレプリケートするスマート レプリケーションを使用します。 進行中のレプリケーションでは、差分の変更のみがレプリケートされます。 Site Recovery は、オフライン データ転送をサポートし、WAN オプティマイザーと連携します。 
- **セカンダリ データ センターの必要性を排除**-Site Recovery は、データ センター間でレプリケーションを自動化できますが、Azure にレプリケートすることによって、オンサイトのセカンダリの場所を断念する機会も提供します。 レプリケートされたデータは、元の復元性を十分に備えた状態で Azure Storage に格納されます。


## デプロイ シナリオ

次の表は、Site Recovery でサポートされているレプリケーションのシナリオをまとめたものです。

**REPLICATE** | **レプリケート元** | **レプリケート先** | **記事**
---|---|---|---
VMware 仮想マシン | オンプレミスの VMware サーバー | Azure Storage | [デプロイ](site-recovery-vmware-to-azure.md)
Windows または Linux の物理サーバー | オンプレミスの物理サーバー | Azure Storage | [デプロイ](site-recovery-vmware-to-azure.md)
Hyper-V 仮想マシン | VMM クラウド内のオンプレミスの Hyper-V ホスト サーバー | Azure Storage | [デプロイ](site-recovery-vmm-to-azure.md)
Hyper-V 仮想マシン | オンプレミスの Hyper-V サイト (1 つ以上の Hyper-V ホスト サーバー) | Azure Storage | [デプロイ](site-recovery-hyper-v-site-to-azure.md)
オンプレミスの Hyper-V 仮想マシン| VMM クラウド内のオンプレミスの Hyper-V ホスト サーバー | セカンダリ データセンターの VMM クラウド内のオンプレミス Hyper-V ホスト サーバー | [デプロイ](site-recovery-vmm-to-vmm.md)
Hyper-V 仮想マシン | VMM クラウド内のオンプレミス Hyper-V ホスト サーバーと SAN ストレージ| セカンダリ データセンターの VMM クラウド内のオンプレミス Hyper-V ホスト サーバーと SAN ストレージ | [デプロイ](site-recovery-vmm-san.md)
VMware 仮想マシン | オンプレミスの VMware サーバー | VMware が実行されているセカンダリ データセンター | [デプロイ](site-recovery-vmware-to-vmware.md) 
Windows または Linux の物理サーバー | オンプレミスの物理サーバー | セカンダリ データセンター | [デプロイ](site-recovery-vmware-to-vmware.md) 

これらを次の図にまとめて示します。

![オンプレミス間](./media/site-recovery-overview/asr-overview-graphic.png)

## 保護できるワークロードとは

Site Recovery は、アプライアンス対応のビジネス継続性に貢献します。 Site Recovery を使用すると、Windows とサード パーティ アプリの障害復旧を調整できます。 アプリケーションに対応したこの保護機能には、次の特徴があります。


- 最低値は、HYPER-V が 30 秒間の Rpo とほぼ同期レプリケーションとの連続レプリケーションを最も重要なアプリケーションのニーズを満たすために、VMware をします。
- アプリケーションとの整合性が取られたスナップショットであるため、単一層または N 層のアプリケーションに適しています。
- SQL Server AlwaysOn、Active Directory のレプリケーション、Exchange DAG および Oracle Data Guard を含むその他のアプリケーション レベルのレプリケーション テクノロジを使用するパートナーと統合できます。
- 柔軟な復旧計画を策定し、1 回のクリックでアプリケーション スタック全体を復旧できます。この計画には、外部スクリプトまたは手動のアクションを含めることもできます。 
- Site Recovery と Azure での高度なネットワーク管理により、IP アドレスの予約、ロード バランサーの構成、低 RTO ネットワーク切り替え用の Azure Traffic Manager の統合など、アプリのケットワーク要件が簡略化されます。
- 豊富な自動化ライブラリでは、運用環境ですぐに使えるアプリケーション固有のスクリプトを提供しています。これらのスクリプトは、ダウンロードして Site Recovery と統合できます。  


詳細について  [Site Recovery を保護できるワークロードはどれですか?](site-recovery-workload.md)します。


## 次のステップ

この概要作成を終了する後 [詳細](site-recovery-components.md) Site Recovery のアーキテクチャの概要です。 
 

