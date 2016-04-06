<properties 
    pageTitle="SQL Server のパフォーマンスに関するベスト プラクティス | Microsoft Azure"
    description="Microsoft Azure VM で SQL Server のパフォーマンスを最適化するためのベスト プラクティスを紹介します。"
    services="virtual-machines"
    documentationCenter="na"
    authors="rothja"
    manager="jeffreyg"
    editor="monicar" 
    tags="azure-service-management" />
    
<tags 
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="11/13/2015"
    ms.author="jroth" />

# Azure Virtual Machines における SQL Server のパフォーマンスに関するベスト プラクティス

## 概要

このトピックでは、Microsoft Azure 仮想マシンで SQL Server のパフォーマンスを最適化するためのベスト プラクティスを紹介します。 Azure Virtual Machines で SQL Server を実行するときは、オンプレミスのサーバー環境で SQL Server に適用されるデータベース パフォーマンス チューニング オプションと同じものを引き続き使用することをお勧めします。 ただし、パブリック クラウド内のリレーショナル データベースのパフォーマンスは、仮想マシンのサイズやデータ ディスクの構成などのさまざまな要素に左右されます。

SQL Server イメージを作成するときは、Azure ポータルを使用して、Premium Storage を既定使用する機能や、自動修正、自動バックアップ、および AlwaysOn 構成などのその他のオプションを活用することを検討してください。

このドキュメントは、Azure VM で SQL Server の最適なパフォーマンスを得ることに重点を置いています。 ワークロードの要求が厳しくない場合は、以下に示す最適化がすべて必要になるわけではありません。 各推奨事項を評価するときに、パフォーマンスのニーズとワークロードのパターンを考慮してください。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## クイック チェック リスト

Azure Virtual Machines で SQL Server の最適なパフォーマンスを実現するためのクイック チェック リストを次に示します。

|領域|最適化|
|---|---|
|**VM サイズ**|[DS3](virtual-machines-size-specs.md#standard-tier-ds-series) 以上 SQL Enterprise edition 用です。<br/><br/>[DS2](virtual-machines-size-specs.md#standard-tier-ds-series) 以上 SQL Standard、および Web エディションです。|
|**Storage**|使用 [Premium Storage](../storage/storage-premium-storage-preview-portal.md)します。<br/><br/>保持、 [ストレージ アカウント](../storage/storage-create-storage-account.md) と同じリージョンに SQL Server VM です。<br/><br/>Azure を無効にする [geo 冗長ストレージ](../storage/storage-redundancy.md) (geo レプリケーション)、ストレージ アカウントにします。|
|**ディスク**|2 を使用して [P30 ディスク](../storage/storage-premium-storage-preview-portal.md#scalability-and-performance-targets-when-using-premium-storage) (ログ ファイルの 1; のデータ ファイルと TempDB 1)。<br/><br/>データベース ストレージまたはログに、オペレーティング システム ディスクまたは一時ディスクを使用することは避けます。<br/><br/>データ ファイルと TempDB をホストするディスクで読み取りキャッシュを有効にします。<br/><br/>ログ ファイルをホストするディスクでは、キャッシュを有効にしないでください。<br/><br/>複数の Azure データ ディスクをストライプして、IO スループットを向上させます。<br/><br/>ドキュメントに記載されている割り当てサイズでフォーマットします。|
|**I/O**|データベース ページの圧縮を有効にします。<br/><br/>データ ファイルの瞬時初期化を有効にします。<br/><br/>データベースで自動拡張を制限するか、無効にします。<br/><br/>データベースで自動圧縮を無効にします。<br/><br/>システム データベースも含め、すべてのデータベースをデータ ディスクに移動します。<br/><br/>SQL Server エラー ログとトレース ファイルのディレクトリをデータ ディスクに移動します。<br/><br/>既定のバックアップおよびデータベース ファイルの場所を設定します。<br/><br/>ロックされたページを有効にします。<br/><br/>SQL Server パフォーマンス修正プログラムを適用します。|
|**機能固有**|BLOB ストレージに直接バックアップします。|

詳細については、以下のサブ セクションで説明するガイドラインに従ってください。

## 仮想マシン サイズとストレージ アカウントに関する考慮事項

パフォーマンス重視のアプリケーションでは、次の仮想マシン サイズを使用することをお勧めします。

- **SQL Server Enterprise Edition**: DS3 またはそれ以降

- **SQL Server Standard edition および Web Edition**: DS2 またはそれ以降

最新の状態の詳細については、サポートされている仮想マシンのサイズでは、次を参照してください。 [仮想マシンのサイズ](virtual-machines-size-specs.md)します。

さらに、転送遅延を低減するために、SQL Server 仮想マシンと同じデータ センターで Azure ストレージ アカウントを作成することをお勧めします。 ストレージ アカウントの作成時に、geo レプリケーションを無効にします。複数のディスクでの一貫性のある書き込み順序が保証されないためです。 代わりに、2 つの Azure データ センター間で SQL Server 障害復旧テクノロジを構成することを検討します。 詳細については、次を参照してください。 [高可用性と災害復旧の Azure 仮想マシンにおける SQL Server](virtual-machines-sql-server-high-availability-and-disaster-recovery-solutions.md)します。

## ディスクとパフォーマンスに関する考慮事項

Azure 仮想マシンを作成すると、プラットフォームによって、オペレーティング システム ディスク用に少なくとも 1 つのディスクが VM に接続されます。 このディスクは、ページ BLOB としてストレージに格納されている VHD です。 追加のディスクをデータ ディスクとして仮想マシンに接続することもできます。これらのディスクは、ページ BLOB としてストレージに格納されます。 Azure Virtual Machines には、一時ディスクと呼ばれる別のディスクも存在します。 これは、スクラッチ領域に使用できるノード上のディスクです。

### オペレーティング システム ディスク

オペレーティング システム ディスクをブートしてオペレーティング システムの実行中バージョンとしてマウントできる VHD は、ラベルが付いて、 **C** ドライブです。

キャッシュ ポリシーは、オペレーティング システム ディスクで既定値は **読み取り/書き込み**します。 パフォーマンス重視のアプリケーションでは、オペレーティング システム ディスクではなく、データ ディスクを使用することをお勧めします。 下記のデータ ディスクに関するセクションをご覧ください。

### 一時ディスク

一時ストレージ ドライブは、というラベルが付いて、 **D**: ドライブは、Azure blob ストレージには保存されません。 データまたはログ ファイルを保存しない、 **D**: ドライブです。

のみで TempDB またはバッファー プール拡張を格納、 **D** D シリーズまたは G シリーズ仮想マシン (Vm) を使用する場合のドライブ形です。 その他の VM シリーズとは異なり、 **D** ドライブ D シリーズ、G シリーズの Vm では、SSD ベースです。 そのため、一時オブジェクトを頻繁に使用するワークロードや、メモリに収まらないワーキング セットを持つワークロードのパフォーマンスを向上させることができます。 詳細については、次を参照してください。 [SQL Server TempDB およびバッファー プール拡張を格納する Azure Vm で Ssd を使用して](http://blogs.technet.com/b/dataplatforminsider/archive/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions.aspx)します。

### データ ディスク

- **データ ディスク数のデータとログ ファイルの**: には、少なくとも 2 を使用して [P30 ディスク](../storage/storage-premium-storage-preview-portal.md#scalability-and-performance-targets-when-using-premium-storage) あり、1 つのディスクには、ログ ファイルが含まれている、データ ファイルと TempDB に格納もう一方のです。 スループットを向上させるために、データ ディスクを追加することが必要になる場合もあります。 データ ディスクの数を決定するには、データおよびログ ディスクで使用可能な IOPS の数を分析する必要があります。 その情報を参照してください、テーブルあたりの IOPS [VM サイズ](virtual-machines-size-specs.md) し、次の資料の大きさのディスク: [ディスク向け Premium Storage の使用](../storage/storage-premium-storage-preview-portal.md)します。 さらに多くの帯域幅が必要な場合は、追加のディスクを接続し、ディスク ストライピングを使用できます。 サポートされるデータ ディスクの最大数を追加するという推奨事項は、Premium Storage を使用していない場合、 [VM サイズ](virtual-machines-size-specs.md) ディスク ストライピングを使用します。 ディスク ストライピングの詳細については、下記の関連セクションをご覧ください。

- **キャッシュ ポリシー**: データ ディスクをホストしているデータ ファイルと TempDB のみでのキャッシュの読み取りを有効にします。 Premium Storage を使用していない場合は、どのデータ ディスクでもキャッシュを有効にしないでください。 ディスクのキャッシュを構成する手順については、次のトピックを参照してください: [Set-azureosdisk](https://msdn.microsoft.com/library/azure/jj152847) と [Set-azuredatadisk](https://msdn.microsoft.com/library/azure/jj152851.aspx)します。

- **NTFS アロケーション ユニット サイズ**: データ ディスクをフォーマットするときに、データとログ ファイルと、TempDB、64 KB アロケーション ユニット サイズを使用することをお勧めします。

- **ディスク ストライピング**: 次のガイドラインに従うことをお勧めします。 

    - Windows 8/Windows Server 2012 以降を使用して [記憶域スペース](https://technet.microsoft.com/library/hh831739.aspx)します。 ストライプ サイズを、OLTP ワークロードでは 64 KB、データ ウェアハウス ワークロードでは 256 KB にそれぞれ設定して、パーティションの不適切な配置に起因するパフォーマンスの低下を防ぎます。 また、列数を物理ディスクの数に設定します。 8 個以上のディスクで記憶域スペースを構成するには、(サーバー マネージャーの UI ではなく) PowerShell を使用して、ディスクの数に一致する列数を明示的に設定する必要があります。 構成する方法の詳細についての [記憶域スペース](https://technet.microsoft.com/library/hh831739.aspx), を参照してください [Windows PowerShell で記憶域スペースのコマンドレット](https://technet.microsoft.com/library/jj851254.aspx)
    
    - Windows 2008 R2 以前では、ダイナミック ディスク (OS ストライプ ボリューム) を使用できます。ストライプ サイズは常に 64 KB です。 Windows 8 および Windows Server 2012 の時点で、このオプションは使用されていません。 詳細については、サポート ポリシーを参照してください。 [仮想ディスク サービスが Windows Storage Management API に遷移する](https://msdn.microsoft.com/library/windows/desktop/hh848071.aspx)です。
    
    - ワークロードで大量のログが発生するわけではなく、専用の IOPS を必要としない場合は、記憶域プールを 1 つだけ構成します。 それ以外の場合は、記憶域プールを 2 つ作成して、1 つをログ ファイルに使用し、もう 1 つをデータ ファイルと TempDB に使用します。 負荷予測に基づいて、各記憶域プールに関連付けるディスクの数を決定します。 接続できるデータ ディスクの数は VM サイズによって異なることに注意してください。 詳細については、次を参照してください。 [仮想マシンのサイズ](virtual-machines-size-specs.md)します。

## I/O パフォーマンスに関する考慮事項

- Premium Storage では、アプリケーションと要求の並列処理を実行するときに最良の結果が得られます。 Premium Storage は、IO キューの深さが 1 より大きいシナリオ向けに設計されているので、シングル スレッド シリアル要求では (ストレージを集中的に使用する場合でも)、パフォーマンスの向上はほとんどまたはまったくありません。 たとえば、これは、パフォーマンス分析ツール (SQLIO など) のシングル スレッド テストの結果に影響する可能性があります。

- 使用を検討して [データベースのページの圧縮](https://msdn.microsoft.com/library/cc280449.aspx) I/O ワークロードのパフォーマンスを向上させることができます。 ただし、データ圧縮を使用すると、データベース サーバーでの CPU 消費量が増加する場合があります。

- Azure との間での転送時にデータ ファイルを圧縮することを検討します。

- 初期ファイル割り当てに必要な時間を短縮するために、ファイルの瞬時初期化を有効にすることを検討します。 ファイルの瞬時初期化を利用する SE_MANAGE_VOLUME_NAME で SQL Server (MSSQLSERVER) サービス アカウントに付与してそれを追加、 **ボリューム メンテナンス タスクを実行する** セキュリティ ポリシー。 既定のサービス アカウント (NT service \mssqlserver) は追加されません Azure で SQL Server プラットフォーム イメージを使用している場合、 **ボリューム メンテナンス タスクを実行する** セキュリティ ポリシー。 つまり、SQL Server Azure プラットフォーム イメージでは、ファイルの瞬時初期化は有効になりません。 SQL Server サービス アカウントを追加した後、 **ボリューム メンテナンス タスクを実行する** セキュリティ ポリシーでは、SQL Server サービスを再起動します。 この機能を使用する場合、セキュリティに関する考慮事項があります。 詳細については、次を参照してください。 [データベース ファイルの初期化](https://msdn.microsoft.com/library/ms175935.aspx)します。

- **自動拡張** 偶発的な予想外の増加と見なされます。 自動拡張を使用して、データやログの増加に日常的に対処しないでください。 自動拡張を使用する場合は、Size スイッチを使用してファイルを事前に拡張します。

- 確認 **autoshrink** はパフォーマンスに悪影響が不要なオーバーヘッドを避けるために無効になります。

- SQL Server 2012 を実行している場合は、Service Pack 1 Cumulative Update 10 をインストールします。 この更新プログラムには、SQL Server 2012 で一時テーブルに対して SELECT INTO ステートメントを実行したときに I/O のパフォーマンスが低下する問題に対処するための修正プログラムが含まれています。 詳細については、この参照してください。 [サポート技術情報記事](http://support.microsoft.com/kb/2958012)します。

- パフォーマンスを向上させるために、システム データベース (msdb や TempDB など)、バックアップ、SQL Server の既定のデータ ディレクトリとログ ディレクトリをキャッシュされないデータ ディスクに移動します。 その後、次の操作を行います。

    - XEvent とトレース ファイルのパスを調整する。
    
    - SQL エラー ログのパスを調整する。
    
    - 既定のバックアップ パスを調整する
    
    - 既定のデータベースの場所を調整する。

- ロックされたページを確立して、IO とページング アクティビティを減らします。

## 機能固有のパフォーマンスに関する考慮事項

一部のデプロイでは、より高度な構成手法を使用することで、パフォーマンスがさらに向上する場合があります。 パフォーマンスの向上を実現する際に役立つ SQL Server の機能を次に示します。

- **Azure ストレージへのバックアップ**: Azure の仮想マシンで実行されている SQL Server のバックアップを実行するときを使用できます [SQL Server Backup to URL](https://msdn.microsoft.com/library/dn435916.aspx)します。 SQL Server 2012 SP1 CU2 以降で使用できるこの機能は、接続されているデータ ディスクにバックアップする場合に推奨されます。 バックアップ/復元する]、[Azure ストレージから次のある推奨事項 [SQL Server Backup to URL Best Practices と Troubleshooting and Restoring Azure ストレージに格納されているバックアップから](https://msdn.microsoft.com/library/jj919149.aspx)します。 使用してこれらのバックアップを自動化することもできます。 [Azure 仮想マシンにおける SQL Server の自動バックアップ](virtual-machines-sql-server-automated-backup.md)します。

    SQL Server 2012 では、前に行うこともできます [SQL Server Backup to Azure Tool](https://www.microsoft.com/download/details.aspx?id=40740)します。 このツールでは、複数のバックアップ ストライプ ターゲットを使用することで、バックアップ スループットを向上させることができます。

- **Azure で SQL Server データ ファイル**: この新しい機能 [Azure で SQL Server データ ファイル](https://msdn.microsoft.com/library/dn385720.aspx), は SQL Server 2014 以降で使用できます。 Azure 内のデータ ファイルを使用して SQL Server を実行すると、Azure データ ディスクを使用する場合と同等のパフォーマンス特性が得られます。

## 次のステップ

さらに詳しく SQL Server と Premium Storage を調べる場合は、記事を参照してください。 [Azure Premium Storage を使用して、仮想マシン上の SQL Server を](virtual-machines-sql-server-use-premium-storage.md)します。

セキュリティのベスト プラクティスについては、次を参照してください。 [Azure 仮想マシンにおける SQL Server のセキュリティの考慮事項](virtual-machines-sql-server-security-considerations.md)します。

あるその他の SQL Server 仮想マシンのトピックを確認 [Azure 仮想マシンの概要 [SQL Server](virtual-machines-sql-server-infrastructure-services.md)します。


