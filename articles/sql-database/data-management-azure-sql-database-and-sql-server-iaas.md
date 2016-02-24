<properties
    pageTitle="SQL Database (PaaS) とクラウドの Azure VM 上の SQL Server (IaaS) | Microsoft Azure"
    description="Azure SQL Database (PaaS) とクラウドの Azure Virtual Machines 上の SQL Server のどちらのクラウド SQL Server オプションがお客様のアプリケーションに適合するかを説明します。"
    services="sql-database, virtual-machines"
    keywords="SQL Server cloud, SQL Server in the cloud, SaaS database, cloud SQL Server, DBaaS"
    documentationCenter=""
    authors="jeffgoll"
    manager="jeffreyg"
    editor="cjgronlund"/>

<tags
    ms.service="sql-database"
    ms.workload="data-management"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="12/04/2015"
    ms.author="jeffreyg"/>

# Azure の SQL Server オプションの選択: Azure SQL Database (PaaS) と Azure VM 上の SQL Server (IaaS)

Azure には、クラウドで SQL Server ワークロードをホストするためのオプションが 2 つあります。

* [Azure SQL Database](https://azure.microsoft.com/services/sql-database/): SQL データベースをネイティブにクラウド サービス (PaaS) データベース、またはサービスとしてのソフトウェア (SaaS) アプリケーションの開発用に最適化されているサービス (DBaaS) としてデータベースとしてのプラットフォームとも呼ばれます。 SQL Server の機能の大部分と互換性があります。
* [Azure 仮想マシン上の SQL Server](https://azure.microsoft.com/services/virtual-machines/sql-server/): SQL Server がインストールされているし、Windows Server 仮想マシン (Vm) サービス (IaaS) としてのインフラストラクチャとも呼ばれ、Azure で実行されているクラウドでホストされています。

この記事では、各オプションがマイクロソフトのデータ プラットフォームにどのように組み込まれているかを説明します。お客様のビジネス要件に適したオプションを判断するのにお役立てください。 この記事を参照することで、コスト削減と管理の最小化のいずれを優先するかに関係なく、最大の関心事項であるビジネス要件に適したアプローチを判断できます。


## マイクロソフトのデータ プラットフォーム

Azure とオンプレミス SQL Server データベースの話を始める前にまず、それらがすべてが利用可能であるということを理解しておいてください。 Microsoft のデータ プラットフォームは、SQL Server テクノロジを活用し、物理的なオンプレミスマシン、プライベート クラウド環境、サード パーティにホストされているプライベート クラウド環境、パブリック クラウドのすべてでそのテクノロジを使用できるようにします。 これにより、環境全体でサーバー製品、開発ツール、専門知識を同じ組み合わせで使用しながら、オンプレミスとクラウド ホスト型のデプロイメントを組み合わせることで、多様な独自のビジネス ニーズを満たすことができます。

   ![クラウド SQL Server オプション: IaaS 上の SQL Server またはクラウド内の SaaS SQL Database](./media/data-management-azure-sql-database-and-sql-server-iaas/SQLIAAS_SQL_Server_Cloud_Continuum.png)

図に示すように、各製品は、インフラストラクチャに及ぼす管理のレベル (X 軸) と、データベース レベルの統合と自動化で実現するコスト効率の度合い (Y 軸) によって特徴付けることができます。

アプリケーションの設計時には、アプリケーションの SQL Server の部分をホストするために 4 つの基本的なオプションを使用できます。

- 仮想化されていない物理マシン上の SQL Server
- オンプレミス仮想マシン内の SQL Server (プライベート クラウド)
- Azure Virtual Machines 内の SQL Server (パブリック クラウド)
- Azure SQL Database (パブリック クラウド)

次のセクションでは、パブリック クラウドの SQL Server (Azure SQL Database と Azure VM 上の SQL Server) について説明します。 さらに、アプリケーションにとって最適なオプションを決定する一般的なビジネスの要因について確認します。

## Azure SQL Database と Azure VM 上の SQL Server の詳細

**Azure SQL Database** 、リレーショナル データベースとしてのサービス (DBaaS) 業界のカテゴリに分類される Azure クラウドでホストされているは *ソフトウェアとしてのサービス (SaaS)* と *プラットフォームとしてのサービス (PaaS)*します。 SQL Database は、マイクロソフトが所有、ホスト、保守する標準的なハードウェアおよびソフトウェアをベースにして構築されています。 SQL Database では、組み込みの機能を使用して、サービス上で直接開発を行えます。 SQL Database は従量課金制で利用でき、中断することなくスケールアップまたはスケールアウトして強化できるオプションもあります。

**Azure 仮想マシン (Vm) の SQL Server** 、業界内のカテゴリに分類 *インフラストラクチャとしてのサービス (IaaS)* 、クラウドにバーチャル マシンの内部 SQL Server を実行することができます。 SQL Database と同様に、マイクロソフトが所有、ホスト、保守する標準的なハードウェア上に構築されています。 VM 上の SQL Server を使用する場合は、お客様が所有している SQL Server ライセンスを移行するか、事前にライセンス付与された SQL Server イメージを Azure ポータルから使用できます。

概して、目的別に最適化された次の 2 つの SQL オプションがあります。

- **SQL Database** プロビジョニングおよび多くのデータベースを管理するための最低限に全体的なコストを削減するように最適化します。 仮想マシン、オペレーティング システム、データベース ソフトウェアを管理する必要がないため、継続的な管理コストが軽減されます。 アップグレード、高可用性、バックアップなどを管理する必要がありません。 一般的には、Azure SQL Database を使用すると、単一の IT または開発リソースが管理するデータベースの数を大幅に増やすことができます。
- **Azure Vm で実行される SQL Server** 既存内部設置型のハイブリッド シナリオでのクラウドへの SQL Server アプリケーションを拡張または既存のアプリケーションを Azure に移行または開発/テストのシナリオでの展開に適しています。 ハイブリッド シナリオの例は、レプリカを保持するセカンダリ データベースで、Azure の使用による [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)します。 Azure VM 上の SQL Server では、専用の SQL Server インスタンスとクラウド ベースの VM に対して完全な管理者権限があります。 仮想マシンの維持に使用できる IT リソースが組織に既にある場合は、Azure VM 内の SQL Server を選択することをお勧めします。 VM 上の SQL Server を使用すると、高度にカスタマイズされたシステムを構築して、アプリケーション固有のパフォーマンスや可用性の要件に対処できます。

次の表に SQL Database と Azure VM 上の SQL Server の主な特徴をまとめます。

<table cellspacing="0" border="1">
<tr>
   <th align="left" valign="middle"></th>
   <th align="left" valign="middle">SQL Database</th>
   <th align="left" valign="middle">Azure VM 内の SQL Server</th>

</tr>
<tr>
   <td valign="middle"><p><b>最も適した</b></p></td>
   <td valign="middle">
          <ul>
          <li type=round>開発とマーケティングに時間的制約があるクラウド設計の新しいアプリケーション。
          <li type=round>必要とする組み込みの高可用性、災害復旧のメカニズムをアップグレードするアプリケーション。
          <li type=round>基になるオペレーティング システムと構成設定を管理したくないチームです。
         <li type=round>スケール アウト パターンを使用するアプリケーション。
         <li type=round>サイズは最大 1 TB のデータベース。
         <li type=round>ソフトウェアとしてのサービス (SaaS) アプリケーションを構築します。
  </ul>
</td>
   <td valign="middle">
      <ul>
      <li type=round>最小限の変更でクラウドに迅速に移行を必要とする既存のアプリケーションです。
      <li type=round>セキュリティで保護されたトンネルを使用して内部設置型リソース (Active Directory) などを Azure からのアクセスに必要な SQL Server アプリケーション。
      <li type=round>完全な管理者権限を持つカスタマイズされた IT 環境をする場合。
      <li type=round>迅速な開発およびテスト シナリオ、内部設置型の非運用 SQL Server のハードウェアを購入したくない場合。
      <li type=round>使用して内部設置型 SQL Server アプリケーションの災害復旧 [backup to Azure Storage](http://msdn.microsoft.com/library/jj919148.aspx) または [AlwaysOn replicas with Azure VMs](../virtual-machines/virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)します。
      <li type=round>1 TB を超える大きなデータベース。
      </ul></td>
</tr>
<tr>
   <td valign="middle"><p><b>リソース</b></p></td>
   <td valign="middle">
       <ul>
       <li type=round>基になるインフラストラクチャのサポートおよび保守のための IT リソースを使用したくないです。
       <li type=round>アプリケーション層に注目するには。
       </ul></td>
   <td valign="middle"><ul><li type=round>サポートおよび保守のための IT リソースがあります。</ul></td>

</tr>
<tr>
   <td valign="middle"><p><b>総保有コスト</b></p></td>
   <td valign="middle"><ul><li type=round>ハードウェアのコストを削減します。 管理コストを削減します。</ul></td>
   <td valign="middle"><ul><li type=round>ハードウェアのコストを削減します。 </ul></td>

</tr>
<tr>
   <td valign="middle"><p><b>ビジネス継続性</b></p></td>
   <td valign="middle"><ul><li type=round>組み込みのフォールト トレランスのインフラストラクチャ機能以外にも Azure SQL Database は、特定の時点へタイム リストア、Geo リストア、Geo レプリケーションのビジネス継続性を向上させるなどの機能を提供します。 詳細については、次を参照してください [SQL Database business continuity overview](sql-database-business-continuity.md)。</ul></td>
   <td valign="middle"><ul><li type=round>。Azure Vm 上の SQL Server では、データベースの特定のニーズに応じて高可用性と災害復旧のソリューションを設定することができます。 そのため、アプリケーション向けにシステムを大幅に最適化できます。 必要なときに自分でフェールオーバーをテストして実行できます。 詳細については、[高可用性と災害復旧の Azure 仮想マシン上の SQL Server] を参照してください ((../virtual-machines/virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)。</ul></td>

</tr>
<tr>
   <td valign="middle"><p><b>ハイブリッド クラウド</b></p></td>
   <td valign="middle"><ul><li type=round>、内部設置型アプリケーションが Azure SQL Database のデータにアクセスできます</ul></td>
   <td valign="middle"><ul>
      <li type=round>。Azure Vm で SQL server、クラウドの一部を実行し、オンプレミスするアプリケーションがあることができます。 内部設置型ネットワークと Active Directory ドメインを使用してクラウドに拡張するなど [Azure Virtual Network](../virtual-network/virtual-networks-overview.md)します。 Azure ストレージを使用して内部設置型のデータ ファイルを格納するさらに、 [SQL Server Data Files in Azure](http://msdn.microsoft.com/library/dn385720.aspx)します。 詳細については、次を参照してください。 [Introduction to SQL Server 2014 Hybrid Cloud](http://msdn.microsoft.com/library/dn606154.aspx)します。
      <li type=round>使用して内部設置型 SQL Server アプリケーションの災害復旧をサポートしている [SQL Server Backup and Restore with Azure Blob Storage](http://msdn.microsoft.com/library/jj919148.aspx) または [AlwaysOn replicas in Azure VMs](../virtual-machines/virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)します。
      </ul></td>

</tr>
</table>

## Azure SQL Database と Azure VM 上の SQL Server のいずれかを選択するときのビジネスの要因

### コスト

現金が潤沢にないスタートアップ企業や厳しい予算で経営している老舗企業では、資金調達での限界がデータベースをホストする方法を決定する主な要因になることがあります。 このセクションではまず、SQL Database と Azure VM 上の SQL Server の 2 つのリレーショナル データベース オプションに関する Azure の課金とライセンスの基礎について説明します。 次に、アプリケーションの総コストを計算する方法を見てみましょう。

#### 課金とライセンスの基礎

**SQL Database** Azure Vm 上の SQL Server では、従来の SQL Server のライセンスが必要ですが、ライセンスではなく、サービスとして顧客に販売されます。

現在のところ、 **SQL Database** は選択したサービス階層とパフォーマンス レベルに基づく固定の率で時間単位に課金はすべてのいくつかのサービス階層で使用できます。 Basic、Standard、および Premium のサービス階層は、アプリケーションのピーク時の要件を満たすために、複数のパフォーマンス レベルで、予測できるパフォーマンスを実現するように設計されています。 サービス階層とパフォーマンス レベルを変更して、アプリケーションのさまざまなスループット ニーズを満たすことができます。 現在サポートされているに関する最新情報については、サービス層を参照してください [Azure SQL Database サービス階層](sql-database-service-tiers.md)します。

 **SQL Database**, 、データベース ソフトウェアは自動的に構成されている、この修正プログラムを適用すると、および Microsoft では、管理コストを削減してアップグレードします。 さらに、その [組み込みバックアップ](sql-database-business-continuity.md) 機能を使用して、データベースの数が多い場合は特に、大幅なコスト削減を実現できます。 SQL データベースを使用する場合に課金されません個々 のクエリを実行している SQL データベース、着信インターネット トラフィックに対しては課金されます [送信インターネット トラフィック](http://azure.microsoft.com/pricing/details/data-transfers/)します。 データベースのトランザクション量が膨大で、多数の同時ユーザーをサポートする必要がある場合は、Premium サービス階層をお勧めします。

 **Azure Vm 上の SQL Server**, 、従来の SQL Server のライセンスを利用します。 プラットフォームによって提供される SQL Server イメージ (ライセンスが含まれています) を使用するか、所有している SQL Server ライセンスを移行することができます。 Azure によって提供されるイメージを使用する場合、追加コストは、VM のサイズだけでなく選択する SQL Server のエディションによって決まります。 VM のサイズや SQL Server のエディションに関係なく、VM ディスクの Azure ストレージのコストと共に SQL Server および Windows Server の 1 分あたりのライセンス コストを支払います。 1 分あたりの課金のオプションでは、SQL Server の追加ライセンスを購入することなく、必要なだけ SQL Server を使用することができます。 Azure に SQL Server のライセンスを移行する場合は、Windows Server コストとストレージ コストのみに対して課金されます。 ライセンスの表示-独自の詳細については、次を参照してください。 [Azure でのソフトウェア アシュアランスによるライセンス モビリティ](http://azure.microsoft.com/pricing/license-mobility/)します。

#### アプリケーションの総コストの計算

クラウド プラットフォームの使用を開始する場合、アプリケーションの実行コストには主に、開発および管理のコストと、パブリック クラウド プラットフォームで必要とされるサービスのコストが含まれます。

SQL Database と Azure VM 上の SQL Server で実行されるアプリケーションの詳細なコストの計算方法を次に示します。

**Azure SQL Database を使用する場合:**

*アプリケーションの総コスト = 大幅に最小化される管理コスト + ソフトウェア開発コスト + SQL Database サービス コスト*

**Azure VM 上の SQL Server を使用する場合:**

*アプリケーションの総所有コスト = 最小化されたソフトウェアの開発および変更コスト + 管理コスト + SQL Server および Windows Server のライセンス コスト + Azure Storage のコスト*

価格の詳細については、次のリソースを参照してください。

- [SQL データベースの料金](http://azure.microsoft.com/pricing/details/sql-database/)
- [仮想マシンの料金](http://azure.microsoft.com/pricing/details/virtual-machines/) の [SQL](http://azure.microsoft.com/pricing/details/virtual-machines/#sql) および [Windows](http://azure.microsoft.com/pricing/details/virtual-machines/#windows)
- [Azure 料金計算ツール](http://azure.microsoft.com/pricing/calculator/)

> [AZURE.NOTE] 該当するか、SQL データベースでは利用できないではない SQL Server の機能の小さなサブセットがあります。 参照してください [SQL Database の一般的な制限事項とガイドライン](sql-database-general-limitations.md) と [SQL データベース TRANSACT-SQL について](sql-database-transact-sql-information.md) の詳細。 既存の SQL Server ソリューションをクラウドに移動する場合は、次を参照してください。 [Azure SQL データベースに SQL Server データベースを移行する](sql-database-cloud-migrate.md)です。 既存のオンプレミス SQL Server アプリケーションを SQL Database に移行する場合は、クラウド サービスで提供されている機能を活用できるようにアプリケーションを更新することを検討してください。 たとえばが使用を検討する [Azure Web App Service](https://azure.microsoft.com/services/app-service/web/) または [Azure Cloud Services](http://azure.microsoft.com/services/cloud-services/) をコストのメリットを増やすには、アプリケーション層をホストします。

### 管理

多くの企業にとって、クラウド サービスに切り替えるかどうかの決定には、コストだけでなく、管理の複雑さの軽減も重要な要素です。  **SQL Database**, 、Microsoft が基になるハードウェアを管理する、自動的に高可用性を提供するすべてのデータをレプリケート、構成データベース ソフトウェアをアップグレードを負荷分散を管理し、透過的なフェールオーバーではサーバーの障害が発生した場合。 ユーザーは引き続きデータベースを管理できますが、データベース エンジン、サーバー オペレーティング システム、ハードウェアを管理する必要はありません。  引き続き管理できるアイテムの例として、データベースとログイン、インデックスとクエリのチューニング、監査とセキュリティなどがあります。

一方で、社内に専門知識を蓄積し、ディスクに配置するデータベースの場所まで制御したいとユーザーが希望することもあります。  **Azure Vm で実行される SQL Server**, 、オペレーティング システムと SQL Server インスタンスの構成を完全に制御があります。 VM を使用する場合、オペレーティング システムとデータベース ソフトウェアの更新やアップグレードのタイミングに加えて、ウイルス対策やバックアップ ツールなど追加ソフトウェアのインストールのタイミングは、ユーザーが決定します。 また、VM のサイズ、ディスクの数、ストレージの構成を制御できます。  たとえば、Azure では、必要に応じて、VM のサイズを変更できます。 詳細については、次を参照してください。 [仮想マシンと Azure のクラウド サービスのサイズ](../virtual-machines/virtual-machines-size-specs.md)します。

### サービス レベル アグリーメント (SLA)

多くの IT 部門では、サービス レベル アグリーメント (SLA) の稼働時間に関する義務を遂行することは、最優先事項です。 このセクションでは、各データベースのホスト オプションに適用される SLA について説明します。

 **SQL Database** Basic、Standard、および Premium のサービス階層をマイクロソフトは可用性 99.99% の SLA を提供します。 最新情報については、次を参照してください。 [サービス レベル アグリーメント](https://azure.microsoft.com/support/legal/sla/sql-database/)します。 SQL Database サービス階層とサポートされているビジネス継続性のプランの最新情報は、次を参照してください。 [サービス階層](sql-database-service-tiers.md)します。

 **Azure Vm で実行される SQL Server**, 、マイクロソフトは可用性仮想マシンだけをカバーする 99.95% の SLA を提供します。 この SLA は VM 上で実行される (SQL Server などの) プロセスは対象としておらず、可用性セットに少なくとも 2 つの VM インスタンスをホストしている必要があります。 最新の情報を参照してください、 [VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/)します。 データベース内の高可用性 (HA) Vm に構成してください、サポートされている高可用性オプションのいずれかの SQL Server など [AlwaysOn 可用性グループ](http://blogs.technet.com/b/dataplatforminsider/archive/2014/08/25/sql-server-alwayson-offering-in-microsoft-azure-portal-gallery.aspx)します。

### <a name="market"></a>製品化に要する時間

**SQL Database** クラウド用に設計されたアプリケーションに最適なソリューションは、開発者の生産性と迅速なタイム トゥ マーケットが重要な場合です。 プログラムによる DBA のような機能を備えることで、基になるオペレーティング システムとデータベースを管理する必要性が減少するため、クラウドの設計者と開発者に最適です。 たとえば、使用、 [REST API](http://msdn.microsoft.com/library/azure/dn505719.aspx) と [PowerShell コマンドレット](http://msdn.microsoft.com/library/azure/dn546726.aspx) を自動化し、数千のデータベースの管理の操作を管理します。 などの機能 [弾力性データベース プール](sql-database-elastic-pool.md) を使用するアプリケーション層に注目し、ソリューションをより早く市場に提供できます。

**Azure Vm で実行される SQL Server** は既存または新規のアプリケーションは、SQL Server インスタンスのすべての機能に対するアクセスと制御が必要な場合に最も適しています。 また、既存のオンプレミス アプリケーションとデータベースを Azure にそのまま移行する場合にも適しています。 プレゼンテーション層、アプリケーション層、およびデータ層を変更する必要がないため、既存のソリューションを再設計する時間と予算が節約されます。 その一方で、すべてのソリューションを Azure に移行し、Azure プラットフォームで必要な一部のパフォーマンスの最適化に集中できます。 詳細については、次を参照してください。 [Azure 仮想マシン上の SQL Server のパフォーマンスのベスト プラクティス](../virtual-machines/virtual-machines-sql-server-performance-best-practices.md)します。

## 概要

この記事では、SQL Database と Azure Virtual Machines (VM) 上の SQL Server について詳しく紹介し、意思決定に影響する可能性のある一般的なビジネス要因について説明しました。 以下は、検討に役立つ提案をまとめたものです。

選択 **Azure SQL Database** 場合。

- 新しいクラウド ベース アプリケーションを構築しようとしている。または、クラウド サービスによって実現されるコスト削減やパフォーマンス最適化を活用するために既存の SQL Server ソリューションを移行したい。 このアプローチでは、完全に管理されたクラウド サービスの利点を活かして、最初の市場投入までの時間を短縮し、長期的なコスト最適化を実現できます。

- マイクロソフトのサービスを利用して、データベースで一般的な管理操作を実行し、データベースの可用性の SLA を強化する場合。

参照してください [最初の Azure SQL database を作成する](sql-database-get-started.md) 開始します。

選択 **Azure Vm 上の SQL Server** 場合。

- 既存のオンプレミス アプリケーションがあり、自社によるハードウェア管理を停止したい。または、ハイブリッド ソリューションを検討している。 このアプローチでは、処理能力の高いデータベースに短時間でアクセスできる一方で、セキュリティで保護されたトンネルを使用してオンプレミス アプリケーションにスムーズに接続できます。

- 既存の IT リソースがあり、SQL Server に対する完全な管理者権限が必要で、オンプレミス SQL Server との完全な互換性が必要である。 この方法では、ほとんどのアプリケーションを柔軟に実行できることで、既存のアプリケーションの開発や変更のコストを最小限に抑えることができます。 また、VM、オペレーティング システム、データベースの構成を全面的に制御できます。

参照してください [Azure で SQL Server 仮想マシンをプロビジョニング](virtual-machines-provision-sql-server.md) 開始します。

> [AZURE.NOTE] SQL Server 2016 CTP2 を試みますか? Microsoft Azure にサインアップし、 [ここ](http://aka.ms/sql2016vm "ここ") で SQL Server 2016 CTP2 が既にインストールされている仮想マシンを作成します。
