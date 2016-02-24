<properties
    pageTitle="Azure Backup とは | Microsoft Azure"
    description="Azure Backup と復元サービスを利用すれば、Windows Server、Windows クライアント コンピューター、SCDPM サーバー、Azure Virtual Machines のデータとアプリケーションをバックアップし、復元できます。"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor="tysonn"
    keywords="backup and restore; recovery services"/>

<tags
    ms.service="backup"
    ms.workload=「記憶域のバックアップ回復」
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/17/2015"
    ms.author="aashishr"です。"trinadhk"です。"jimpark"/>

# Azure Backup とは
Azure Backup は、オンプレミスまたは Azure にデータをバックアップし、復元できるマルチテナントの Azure サービスです。 既存のオンプレミスまたはオフサイトのバックアップ ソリューションを、信頼性の高い、セキュリティで保護された、コスト競争力のあるクラウド バックアップ ソリューションに置き換えます。 クラウドで実行されている資産を保護するための柔軟性も提供します。 Azure Backup は、拡張性、持続性、高可用性に優れた世界クラスのインフラストラクチャの上に構築されます。 このソリューションを使用すると、System Center Data Protection Manager (SCDPM) サーバー、Windows サーバー、Windows クライアント コンピューター、Azure IaaS 仮想マシンからデータやアプリケーションをバックアップできます。 Azure Backup と SCDPM は、Microsoft のクラウド統合バックアップ ソリューションを構成する基本的なテクノロジです。

> [AZURE.VIDEO what-is-azure-backup]

## クラウド設計ポイント
従来のバックアップ ソリューションは、クラウドをディスクやテープなどのエンドポイントのように扱えるように進化してきました。 この方法は簡単でデプロイしやすく、一貫性のある処理を行いますが、使用が制限され、基になるプラットフォームを最大限に活用できません。 これは、エンド ユーザーにとって非効率的でコストの高いソリューションになります。 Azure を "単なるストレージ エンドポイント" として扱うため、バックアップ ソリューションはパブリック クラウド プラットフォームの豊富な機能を活用できません。 一方、Azure Backup は、クラウドを利用して強力で低コストのバックアップ ソリューションを提供するという真のサービスを提供します。 オンプレミスのバックアップ ソリューション (SCDPM) と統合できるため、エンド ツー エンドのハイブリッド ソリューションを提供します。

この方法の利点は、次のとおりです。

- 低コストで回復力のあるデータ ストレージを提供する、効率的なクラウド ストレージ アーキテクチャ
- 高可用性を保証する、非侵入型の自動調整サービス
- 一貫した方法によるオンプレミス、ハイブリッド、IaaS デプロイへのバックアップ

このソリューションの主な機能は、次のとおりです。

1. **信頼性の高いサービス**: を Azure Backup を採用し、これは、可用性の高いバックアップ サービスを取得します。 このサービスはマルチ テナントであり、基になるコンピューティングやストレージの管理を心配する必要がありません。

2. **信頼性の高いストレージ**: Azure Backup は高い Sla に準拠する信頼性の高いクラウド ストレージの上に構築します。 ストレージを維持する上で資本や運用コストを心配する必要がありません。 さらに、バックアップ先を LRS (ローカル冗長ストレージ) または GRS (Geo レプリケーション ストレージ) から選択できます。 LRS は、同じ geo 内に 3 つのデータをコピーしてローカルのハードウェア障害から保護しますが、GRS は、ペアのデータ センター内にさらに 3 つのコピー (合計 6 つのコピー) を格納します。 これにより、バックアップ データの高可用性が保証されます。 Azure サイト レベルの障害が発生してもバックアップ データは安全です。

3. **セキュリティで保護された**: Azure のバックアップ データがソースで転送およびで暗号化された Azure の保存時に暗号化します。  暗号化キーはソースに保存されます。転送されたり Azure に保存されることはありません。 データを復元するには暗号化キーが必要です。お客様のみがサービス内のデータにフル アクセスできます。

4. **オフサイト保護**: オフサイトでテープ バックアップ ソリューションの料金を払いではなく、顧客が非常に低コストでテープに似た-セマンティクスで強力なソリューションを提供する Azure にバックアップできます。

5. **わかりやすくするため**: Azure Backup は、あらゆる規模のデプロイメントを保護するまで拡張可能な使いやすいインターフェイスを提供します。  サービスが進化すると、中央管理などの機能を使用して 1 つの場所からバックアップ インフラストラクチャを管理できるようになります。

6. **コスト効率に優れた**: Azure Backup の料金には、インスタンスごとのバックアップ管理手数料と Azure で使用中のストレージ (ブロック blob 料金) のコストが含まれます。  他のクラウド ベースのバックアップ ソリューションとは異なり、Azure Backup では復元操作に対して料金がかかりません。 また、復元操作時の出力 (送信) データの転送コストもかかりません。

7. **クラウドでのバックアップ**: 仮想マシンをシャット ダウンする必要がなく実行されている Azure IaaS 仮想マシンの VSS ベースのアプリケーション整合性バックアップを Azure Backup が提供されます。 また、ファイルシステムの整合性を維持しながら、Azure で Linux 仮想マシンをバックアップすることもできます。


## デプロイ シナリオ
| コンポーネント | Azure にデプロイできる? | オンプレミスにデプロイできる? | サポートされているターゲット ストレージ|
| --- | --- | --- | --- |
| Azure Backup エージェント | **[はい]** <br><br>Azure Backup エージェントを Azure で実行されているすべての Windows Server VM に展開できます。 | **[はい]** <br><br>Azure Backup エージェントを Windows Server 仮想マシンまたは物理マシンに展開できます。 | Azure Backup コンテナー |
| System Center Data Protection Manager (SCDPM) | **[はい]** <br><br>に関する詳細情報 [SCDPM を使用して Azure のワークロードを保護する](http://blogs.technet.com/b/dpm/archive/2014/09/02/azure-iaas-workload-protection-using-data-protection-manager.aspx)です。 | **[はい]** <br><br>に関する詳細情報 [、データ センター内のワークロードと仮想マシンを保護する](https://technet.microsoft.com/en-us/library/hh758173.aspx)です。 | ローカルに接続されたディスク<br>Azure Backup 資格情報コンテナー<br>テープ (内部設置型のみ) |
| Azure Backup (VM 拡張機能) | **[はい]** <br><br>に特化して [Azure IaaS 仮想マシンのバックアップ](backup-azure-vms-introduction.md)します。 | **いいえ** <br><br>仮想マシンをバックアップ データ センターに SCDPM を使用します。 | Azure Backup コンテナー |


## アプリケーションとワークロード

| ワークロード | ソース コンピューター | Azure Backup ソリューション |
| --- | --- |---|
| ファイルとフォルダー | Windows Server | [Azure Backup エージェント](backup-configure-vault.md),、<br> [System Center DPM](backup-azure-dpm-introduction.md) |
| ファイルとフォルダー | Windows クライアント | [Azure Backup エージェント](backup-configure-vault.md),、<br> [System Center DPM](backup-azure-dpm-introduction.md) |
| Hyper-V 仮想マシン (Windows) | Windows Server | System Center DPM |
| Hyper-V 仮想マシン (Linux) | Windows Server | System Center DPM |
| Microsoft SQL Server | Windows Server | [System Center DPM](backup-azure-backup-sql.md) |
| Microsoft SharePoint | Windows Server | [System Center DPM](backup-azure-backup-sharepoint.md) |
| Microsoft Exchange |  Windows Server | System Center DPM |
| Azure IaaS Vm (Windows)|  - | [Azure Backup (VM 拡張機能)](backup-azure-vms-introduction.md) |
| Azure IaaS Vm (Linux) | - | [Azure Backup (VM 拡張機能)](backup-azure-vms-introduction.md) |


## 次のステップ
- [Azure Backup を試す](backup-try-azure-backup-in-10-mins.md)
- よく寄せられる質問 Azure Backup サービスに関しては [ここ](backup-azure-backup-faq.md)します。
- 参照してください、 [Azure のバックアップ フォーラム](http://go.microsoft.com/fwlink/p/?LinkId=290933)します。

