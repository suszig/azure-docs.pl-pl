<properties
    pageTitle="Premium Storage: Azure の仮想マシン ワークロード用の高パフォーマンス ストレージ| Microsoft Azure"
    description="Premium Storage は、Azure Virtual Machines で実行される高負荷の I/O ワークロードのための、高パフォーマンスで待ち時間の少ないディスク サポートを提供します。 Azure DS シリーズおよび GS シリーズの VM は、Premium Storage をサポートしています。"
    services="storage"
    documentationCenter=""
    authors="ms-prkhad"
    manager=""
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="robinsh;selcint"/>


# Premium Storage: Azure 仮想マシン ワークロード向けの高パフォーマンス ストレージ

## 概要

Azure Premium Storage は、高負荷の I/O ワークロードを実行する仮想マシン向けに高パフォーマンスで待ち時間の少ないディスク サポートを提供します。 Premium Storage を使用する仮想マシン (VM) では、ソリッド ステート ドライブ (SSD) にデータを格納します。 アプリケーションの VM ディスクを Azure Premium Storage に移行することで、これらのディスクの速度とパフォーマンスを最大限に利用することができます。 

Azure VM では複数の Premium Storage ディスクをアタッチすることができるので、アプリケーションには VM あたり最大で 64 TB のストレージを設定することができます。 Premium Storage を使用すると、アプリケーションは、VM あたり 80,000 IOPS (1 秒あたりの入力/出力操作) と、VM あたり 2000 MB/秒のディスク スループットを実現でき、読み取り操作の待ち時間も非常に短くなります。

>[AZURE.NOTE] アプリケーションのパフォーマンスを最高にから Azure Premium Storage の高い IOPS を必要とする任意の仮想マシン ディスクを移行することをお勧めします。 ディスクが高い IOPS を必要としない場合は、ディスクを Standard Storage 内に保持することでコストを抑えることができます。Standard Storage の場合、仮想マシンのディスク データは SSD ではなくハード ディスク ドライブ (HDD) に格納されます。

Azure Premium Storage を開始するを参照してください。 [無料で始める](http://azure.microsoft.com/pricing/free-trial/) ページです。 Premium Storage への既存の仮想マシンを移行する方法については、次を参照してください。 [Azure Premium Storage への移行](storage-migration-to-premium-storage.md)します。

## Premium Storage について理解しておくべきこと

次は、Premium Storage の使用前や使用時に考慮すべき重要な事柄の一覧です。

- Premium Storage を使用するには、Premium Storage アカウントが必要です。 Premium Storage アカウントを作成する方法については、次を参照してください。 [の作成とディスクの Premium Storage アカウントを使用して](#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)します。

- Premium Storage は、 [Azure ポータル](portal.azure.com) SDK ライブラリからアクセス: [Storage REST API](http://msdn.microsoft.com//library/azure/dd179355.aspx) バージョン 2014年-02-14 以降、 [サービス管理 REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx) バージョン 2014年-10-01 以降、および [Azure PowerShell](../install-configure-powershell.md) バージョン 0.8.10 以降。

- 現在 Premium Storage をサポートしているリージョンの一覧は、次を参照してください。 [地域別の Azure サービス](http://azure.microsoft.com/regions/#services)します。

- Premium Storage は Azure ページ BLOB のみをサポートしています。これは、Azure Virtual Machines (VM) の永続ディスクを保持するために使用されます。 Azure ページ blob については、次を参照してください。 [ブロック Blob およびページ Blob について](http://msdn.microsoft.com/library/azure/ee691964.aspx)します。 Premium Storage は Azure ブロック BLOB、Azure ファイル、Azure テーブル、Azure キューをサポートしていません。

- Premium Storage アカウントローカル冗長 (LRS) であり、1 つのリージョン内にデータのコピーを 3 つ保持します。  Premium Storage を使用するときに、geo レプリケーションに関する考慮事項について、次を参照してください。、 [スナップショットと Copy Blob Premium Storage を使用する場合](#snapshots-and-copy-blob-when-using-premium-storage) のこの記事のセクションです。

- VM ディスクで Premium Storage アカウントを使用する場合は、DS シリーズまたは GS シリーズの VM を使用する必要があります。 DS シリーズまたは GS シリーズの VM では、Standard Storage ディスクと Premium Storage ディスクの両方を使用できます。 DS シリーズや GS シリーズ以外の VM では Premium Storage ディスクを使用できません。 使用可能な Azure VM ディスクの種類とサイズについては、次を参照してください。 [仮想マシンと Azure のクラウド サービスのサイズ](../virtual-machines/virtual-machines-size-specs.md)します。

- VM 用の Premium Storage ディスクの設定方法は、Standard Storage ディスクの場合と同じです。 使用する Azure ディスクと VM に最適な Premium Storage オプションを選択する必要があります。 VM のサイズは、Premium 製品のパフォーマンス特性に基づき、ワークロードに最適なものにします。 詳細については、次を参照してください。 [のスケーラビリティおよびパフォーマンスのターゲット Premium Storage 使用時](#scalability-and-performance-targets-when-using-premium-storage)します。

- Premium Storage のアカウントをカスタム ドメイン名にマッピングすることはできません。

- 現在、Premium Storage では、Storage Analytics はサポートされていません。 ディスクを Premium Storage アカウントを使用する Vm のパフォーマンス メトリックを分析する場合など、オペレーティング システムのツールを使用して [Windows パフォーマンス モニター](https://technet.microsoft.com/library/cc749249.aspx) Windows Vm の場合と [IOSTAT](http://linux.die.net/man/1/iostat) Linux vm のです。 Azure プレビュー ポータルで Azure VM 診断を有効にすることもできます。 参照してください [Microsoft Azure 診断の拡張機能での Azure 仮想マシン監視](http://azure.microsoft.com/blog/2014/09/02/windows-azure-virtual-machine-monitoring-with-wad-extension/) 詳細です。

## ディスク向け Premium Storage の使用
ディスク向け Premium Storage は次のいずれかの方法で使用できます。

- 最初に新しい Premium Storage アカウントを作成し、VM の作成時にそのアカウントを使用します。
- 新しい DS シリーズまたは GS シリーズの VM を作成します。 VM の作成時には、以前に作成した Premium Storage アカウントを選択する、新しいアカウントを作成する、Azure ポータルで自動的に既定の Premium アカウントを作成する、のいずれかの方法を選択できます。

ストレージ アカウントは、オペレーティング システム (OS) とデータ ディスクのコンテナーとして使用されます。 つまり、Azure DS シリーズまたは GS シリーズの VM を作成して Azure Premium Storage アカウントを選択すると、オペレーティング システムとデータ ディスクはそのストレージ アカウントに格納されます。

Premium Storage への既存の仮想マシンを移行する方法については、次を参照してください。 [Azure Premium Storage への移行](storage-migration-to-premium-storage.md)します。

Premium Storage のメリットを活用する、アカウントの種類を使用して Premium Storage アカウントの作成 *Premium_LRS* 最初です。 これを行うには、使用することができます、 [Azure ポータル](portal.azure.com), 、[Azure PowerShell](../install-configure-powershell.md), 、または [サービス管理 REST API](http://msdn.microsoft.com/library/azure/ee460799.aspx)します。 手順については、次を参照してください。 [の作成とディスクの Premium Storage アカウントを使用して](#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk)します。

### 重要:

- 容量と Premium Storage アカウントの帯域幅の制限に関する詳細については、「 [のスケーラビリティおよびパフォーマンスのターゲット Premium Storage 使用時](#scalability-and-performance-targets-when-using-premium-storage) セクションです。 アプリケーションのニーズが単一のストレージ アカウントの拡張性ターゲットを上回った場合は、複数のストレージ アカウントを使用するようにアプリケーションを構築し、それらのストレージ アカウント間でデータを分割します。 たとえば、多数の VM があり、アタッチするディスクの合計が 51 テラバイト (TB) の場合は、ディスクを 2 つのストレージ アカウントに分散させます。Premium Storage アカウント 1 つあたりの上限は 35 TB であるからです。 1 つの Premium Storage アカウントでプロビジョニングするディスクの容量が 35 TB を超えることがないようにしてください。
- 既定では、ディスクのキャッシュ ポリシーは、すべてのPremium データ ディスクに対して「読み取り専用」、VM にアタッチされた Premium オペレーティング システム ディスクに対して「読み取り/書き込み」です。 アプリケーションの I/O のパフォーマンスを最適化するには、この構成をお勧めします。 書き込み量の多いディスクや書き込み専用のディスク (SQL Server ログ ファイルなど) の場合は、ディスク キャッシュを無効にすることで、アプリケーションのパフォーマンスを向上できる場合があります。
- ディスク トラフィックが流れるのに十分な帯域幅が VM で利用できることを確認します。 たとえば、STANDARD_DS1 VM では、Premium Storage ディスク トラフィック専用の帯域幅は 32 MB/秒です。 つまり、この VM にアタッチされている Premium Storage ディスクが P10 の場合も、32 MB/秒が上限となり、P10 ディスクのスループットの上限である 100 MB に達することはありません。 同様に、STANDARD_DS13 VM 1 つあたりの全ディスクの帯域幅は最大 256 MB/秒です。 現時点では、DS シリーズ最大の VM は STANDARD_DS14 であり、この VM の全ディスクの帯域幅は最大 512 MB/秒です。 現時点では、DS シリーズ最大の VM は STANDARD_GS5 であり、この VM の全ディスクの帯域幅は最大 2000 MB/秒です。

    これらの制限は、ディスク トラフィックのみを対象としており、キャッシュ ヒットとネットワーク トラフィックは対象外となります。 VM のネットワーク トラフィックの帯域幅は別に用意されており、Premium Storage ディスク専用の帯域幅とは区別されています。 
    
    DS シリーズと GS シリーズの Vm の最大 IOPS とスループット (帯域幅) の最新の情報を次を参照してください。 [仮想マシンと Azure のクラウド サービスのサイズ](../virtual-machines/virtual-machines-size-specs.md)します。 Premium storage ディスクとそれぞれの IOPs とスループットの制限の詳細については、内のテーブルを参照してください。、 [のスケーラビリティおよびパフォーマンスのターゲット Premium Storage を使用する場合](#scalability-and-performance-targets-when-using-premium-storage) のこの記事のセクションです。

> [AZURE.NOTE] キャッシュ ヒットでは、ディスクの割り当て済みの IOPS とスループットによる制限はありません。 つまり、DS シリーズ VM または GS シリーズ VM でキャッシュを読み取り専用に設定したデータ ディスクを使用する場合、そのキャッシュからの読み取りは、Premium Storage のディスク制限の対象にはなりません。 そのため、ワークロードの大部分が読み取りの場合、ディスクから非常に高いスループットが得られます。 キャッシュには、VM サイズに基づいて、VM レベルで IOPS とスループットの個別の制限が適用されます。 DS シリーズ VM は、IOPS が約 4000 で、キャッシュとローカル SSD の IO がコアあたり 33 MB/秒です。

- 同じ DS シリーズの VM または GS シリーズの VM 内では、Premium Storage ディスクと Standard Storage ディスクの両方を使用できます。
- Premium Storage を使用すると、DS シリーズの VM をプロビジョニングし、複数の永続データ ディスクをVM に接続できます。 必要に応じて、ディスク全体をストライピングして容量を増やし、ボリュームのパフォーマンスを高めることができます。 使用して Premium Storage データ ディスクをストライピングする場合 [記憶域スペース](http://technet.microsoft.com/library/hh831739.aspx), 、1 つの列を使用するディスクごとに構成する必要があります。 そうしない場合は、ディスク全体のトラフィックの配分が不均等になるため、ストライプ ボリュームの全体的なパフォーマンスが低下する可能性があります。 既定では、サーバー マネージャー ユーザー インターフェイス (UI) で最大 8 つのディスクの列を設定できます。 ただし、8 つ以上のディスクがある場合は、PowerShell を使用してボリュームを作成し、列の数を手動で指定する必要があります。 そうしない場合、サーバー マネージャー UI はそれ以上のディスクがある場合でも 8 つの列を使用し続けます。 たとえば、1 つのストライプ セット内に 32 のディスクがある場合は、32 の列を指定する必要があります。 使用することができます、 *NumberOfColumns* のパラメーター、 [New-virtualdisk](http://technet.microsoft.com/library/hh848643.aspx) PowerShell コマンドレットを仮想ディスクで使用される列の数を指定します。 詳細については、次を参照してください。 [記憶域スペースの概要](http://technet.microsoft.com/library/jj822938.aspx) と [記憶域スペースに関してよく寄せられる質問](http://social.technet.microsoft.com/wiki/contents/articles/11382.storage-spaces-frequently-asked-questions-faq.aspx)します。
- DS シリーズ以外の VM が存在する既存のクラウド サービスに DS シリーズの VM を追加することは避けてください。 回避策の 1 つは、既存の VHD を新しいクラウド サービスに移行し、このクラウドでは DS シリーズ VM だけを運用するというものです。  使用して、DS シリーズ Vm をホストする新しいクラウド サービスの同じ仮想 IP アドレス (VIP) を保持する場合、 [予約済み IP アドレス](virtual-networks-configure-vnet-to-vnet-connection.md) 機能します。 GS シリーズの VM は G シリーズの VM のみ実行している既存のクラウド サービスに追加できます。
- DS シリーズの Azure 仮想マシンで使用するオペレーティング システム (OS) ディスクは、Standard Storage アカウントと Premium Storage アカウントのどちらでホストするように設定されていてもかまいません。 OS ディスクをマシンの起動のみに使用する場合は、Standard Storage ベースの OS ディスクの使用を検討してください。 コストの点で有利であり、起動後のパフォーマンスは Premium Storage とほぼ同じであるからです。 マシン起動以外のタスクも OS ディスクで実行する場合は、Premium Storage を使用すると、より高いパフォーマンスが得られます。 たとえば、アプリケーションで OS ディスクの読み取りと書き込みを行う場合は、Premium Storage ベースの OS ディスクを使うと VM のパフォーマンスが向上します。
- 使用する [Azure コマンド ライン インターフェイス (Azure CLI)](../xplat-cli-install.md) プレミアム ストレージを使用します。 Azure CLI を使用して、いずれかのディスクのキャッシュ ポリシーを変更するには、次のコマンドを実行します。

    `$ azure vm disk attach -h ReadOnly <VM-Name> <Disk-Name>`

    キャッシュ ポリシーのオプションは、ReadOnly、None、ReadWrite があります。 その他のオプションについては、次のコマンドを実行してヘルプを参照してください。

    `azure vm disk attach --help`


## Premium Storage を使用するときの拡張性とパフォーマンスのターゲット

Premium Storage アカウントにディスクをプロビジョニングする場合、アカウントが利用可能な 1 秒あたりに処理できる入力/出力 (IOPS) の量やスループット (帯域幅) は、ディスクのサイズによって異なります。 現在、Premium Storage ディスクには、P10、P20、P30 の 3 種類があります。 次の表に示すように、それぞれに IOPS とスループットに固有の制限があります。

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>Premium Storage ディスク タイプ</strong></td>
    <td><strong>P10</strong></td>
    <td><strong>P20</strong></td>
    <td><strong>P30</strong></td>
</tr>
<tr>
    <td><strong>ディスク サイズ</strong></td>
    <td>128 GiB</td>
    <td>512 GiB</td>
    <td>1024 GiB (1 TB)</td>
</tr>
<tr>
    <td><strong>ディスクあたりの IOPS</strong></td>
    <td>500</td>
    <td>2300</td>
    <td>5000</td>
</tr>
<tr>
    <td><strong>ディスクあたりのスループット</strong></td>
    <td>100 MB/秒 *</td>
    <td>150 MB/秒 *</td>
    <td>200 MB/秒 *</td>
</tr>
</tbody>
</table>

> [AZURE.NOTE] あることを確認するための十分な帯域幅使用可能なドライブのディスク トラフィックで説明したように、vm、 [ディスク向け Premium Storage の使用](#using-premium-storage-for-disks) この記事の前半の「します。 このとおりでない場合は、ディスクのスループットと IOPS が VM の上限の制約を受けて低くなり、前述の表で示したディスクの上限は達成できなくなります。  

Azure はディスク サイズ (切り上げたもの) を、表にある最も近い Premium Storage ディスク オプションにマッピングします。 たとえば、ディスクのサイズが 100 GiB ならば P10 に分類され、1 秒間に処理できる I/O ユニットは最大 500、スループットは最大 100 MB/秒となります。 同様に、ディスクのサイズが 400 GiB ならば P20 に分類され、1 秒間に処理できる I/O ユニットは最大 2300、スループットは最大 150 MB/秒となります。

入力/出力 (I/O) ユニット サイズは 256 KB です。 転送されるデータが 256 KB に満たない場合は、単一の I/O ユニットとみなされます。 I/O のサイズが大きくなると、サイズが 256 KB の複数の I/O としてカウントされます。 たとえば、1100 KB I/O は 5 つの I/O ユニットとしてカウントされます。

スループットの制限には、ディスクへの書き込みと、キャッシュからではなく、そのディスクからの読み取りが含まれます。 たとえば、P10 ディスクの場合、非キャッシュの読み取りと書き込みの合計は、1 秒あたり 100 MB 以下になります。  同様に、1 つの P10 ディスクは 1 秒あたり 100 MB の非キャッシュの読み取りまたは 1 秒あたり 100 MB の書き込みのいずれかを処理できます。また、たとえば、1 秒あたり 60 MB の読み取りと 1 秒あたり 40 MB の書き込みを処理することもできます。

Azure でディスクを作成する場合は、容量、パフォーマンス、拡張性、ピーク時の負荷に関するアプリケーションのニーズに基づいた最適な Premium Storage ディスク製品を選択します。

> [AZURE.NOTE] 既存のディスクのサイズを簡単に増やすことができます。 たとえば、30 GB のディスクのサイズを 128 GB または 1 TB (テラバイト) にしたい場合があります。 また、容量を増やしたり、IOPS とスループットを向上させたりする必要があるため、P20 ディスクを P30 ディスクに変換したい場合もあります。 "-ResizedSizeInGB" プロパティを指定した "Update-AzureDisk" PowerShell コマンドレットを使用して、ディスクを拡張できます。 この操作を実行するには、ディスクを VM からデタッチするか、VM を停止する必要があります。  

次の表は、Premium Storage アカウントの拡張性ターゲットを示しています。

<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>合計アカウント容量</strong></td>
    <td><strong>ローカル冗長ストレージ アカウントの合計帯域幅</strong></td>
</tr>
<tr>
    <td>
    <ul>
       <li type=round>ディスク容量: 35 TB</li>
       <li type=round>スナップショット容量: 10 TB</li>
    </ul>
    </td>
    <td>受信と送信を合わせて最大 50 GB/秒</td>
</tr>
</tbody>
</table>

- 受信とはストレージ アカウントに送信されるすべてのデータ (要求) のことです。
- 送信とはストレージ アカウントから送信されるすべてのデータ (応答) のことです。

詳細については、次を参照してください。 [Azure ストレージのスケーラビリティおよびパフォーマンスのターゲット](http://msdn.microsoft.com/library/azure/dn249410.aspx)します。

## Premium Storage を使用するときの調整
アプリケーションの IOPS またはスループットが Premium Storage ディスクの上限を超えた場合や、VM の全ディスクのディスク トラフィック合計がその VM でのディスク帯域幅上限を超えた場合に、調整 (スロットリング) が行われることがあります。 調整を回避するために、プロビジョニングしたディスクの拡張性とパフォーマンスのターゲット、および VM で使用可能なディスク帯域幅に基づいて、ディスクの保留中の I/O 要求の数を制限することをお勧めします。  

調整を回避できれば、アプリケーションの待ち時間を最短にすることができます。 一方で、ディスクの保留中の I/O 要求の数が少なすぎる場合は、ディスクで利用可能な最大 IOPS とスループット レベルのメリットを活用できません。

次の例は、調整レベルの計算方法を示しています。

### 例 1:
アプリケーションは、P10 ディスク上で、1 秒間に 16 KB ディスク サイズの I/O を 495 回実行しました (これは 495 I/O ユニットに等しくなります)。 同じ 1 秒内に 2 MB の I/O を実行すると、I/O の合計ユニットは 495 + 8 となります。 これは、I/O ユニット サイズが 256 KB であることから、2 MB の I/O は 2048 KB / 256 KB = 8 I/O ユニットとなるためです。 合計の 495 + 8 はディスク制限の 500 IOPS を超過するため、調整が発生します。

### 注:
すべての計算は、256 KB の I/O ユニット サイズに基づいて行われます。

### 例 2:
アプリケーションが P10 ディスク上で、256 KB ディスク サイズの I/O を 400 回実行しました。 合計消費帯域幅は、(400 * 256) / 1024 = 100 MB/秒となります。 100 MB/秒は、P10 ディスクのスループット制限です。 アプリケーションが 1 秒間あたりにより多くの I/O を実行しようとすると、割り当てられた制限を超過してしまうため、調整が発生します。

### 例 3:
DS4 VM に P30 ディスクが 2 つアタッチされています。 P30 ディスク 1 つあたりのスループットは 200 MB/秒です。 しかし、DS4 VM のディスク帯域幅合計は 256 MB/秒が上限です。 したがって、この DS4 VM では、アタッチされたディスクの両方が最大スループットを発揮することはできません。 このことを解決するには、一方のディスクのトラフィックを 200 MB/秒で維持し、他方では 56 MB/秒で維持します。 ディスク トラフィックの合計が 256 MB/秒を超えると、ディスク トラフィックの調整が行われます。

### 注:
ディスク トラフィックの大部分が小さなサイズの I/O で構成されていると、アプリケーションがスループット制限に達する前に、IOPS 制限に達してしまう場合がほとんどです。 逆に、ディスク トラフィックの大部分が大きなサイズの I/O で構成されている場合は、IOPS 制限ではなくスループット制限に達してしまうことがほとんどです。 最適なサイズの I/O を使用し、ディスクの保留中の I/O 要求の数を制限することで、アプリケーションの IOPS とスループットを最大化できます。

## Premium Storage を使用するときのスナップショットと Copy Blob
Premium Storage の場合も、Standard Storage を使用してスナップショットを作成するときと同じ方法でスナップショットを作成できます。 Premium Storage はローカル冗長であるため、スナップショットを作成して、それらのスナップショットを地理冗長 Standard Storage アカウントにコピーすることをお勧めします。 詳細については、次を参照してください。 [Azure Storage 冗長オプション](http://msdn.microsoft.com/library/azure/dn727290.aspx)します。

ディスクが VM にアタッチされている場合、ディスクをバックアップするページ BLOB で特定の API 操作が許可されなくなります。 たとえば、実行することはできません、 [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) ディスクは、VM にアタッチされている限り、その blob に対して操作します。 代わりに、使用して blob のスナップショットを作成して、 [Snapshot Blob](http://msdn.microsoft.com/library/azure/ee691971.aspx) REST API メソッドから実行して、 [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx) アタッチされたディスクをコピーするスナップショットのです。 または、ディスクのアタッチを解除してから、基盤となる BLOB に対して必要な操作を実行します。

### 重要:

- 単一の BLOB のスナップショットの数は 100 に制限されています。 スナップショットは最大で 10 分間隔で取得できます。
- Premium Storage アカウントあたりのスナップショットの最大容量は 10 TB (テラバイト) です。 スナップショット容量とは、スナップショットのみに存在するデータの総量のことであり、ベース BLOB のデータは含まれないことにご注意ください。
- スナップショットの地理冗長コピーを維持するには、AzCopy か Copy Blob を使用して、Premium Storage アカウントから地理冗長 Standard Storage アカウントにスナップショットをコピーできます。 詳細については、次を参照してください。 [Microsoft Azure Storage で AzCopy を使用する方法](storage-use-azcopy.md) と [Copy Blob](http://msdn.microsoft.com/library/azure/dd894037.aspx)します。
- Premium Storage アカウントでページ blob に対して REST 操作を実行する方法の詳細については、次を参照してください。 [Azure Premium Storage での Blob サービス操作の使用](http://go.microsoft.com/fwlink/?LinkId=521969) 、MSDN ライブラリです。

## Linux VM とともに Premium Storage を使用する
Premium Storage 上で Linux VM を設定するときは、次の重要な手順を参照してください。

- Premium Storage ディスクのキャッシュ設定が ReadOnly または None に設定されている場合は、ファイル システムをマウントするときに "バリア" を無効にする必要があります。有効のままでは Premium Storage の拡張性ターゲットを達成できないからです。 Premium Storage ディスクでこれらのキャッシュ設定を使用する場合は、ディスクへの書き込みの耐久性が保証されるので、バリアは必要ありません。 書き込み要求が正常に完了した時点で、データは永続的なストアに書き込まれた状態になっています。 お使いのファイル システムに応じて、次の方法を使用して "バリア" を無効にしてください。
    - 使用する場合 **reiserFS**, マウント オプションのバリアを無効にする] を"バリア = none"(バリアを有効にするには、次のように使用します"barrier = flush")。
    - 使用する場合 **ext3/ext4**, マウント オプションのバリアを無効にする] を"barrier = 0"(バリアを有効にするには、次のように使用します"barrier = 1")。
    - 使用する場合 **XFS**, 、マウント オプション"nobarrier"バリアを無効にする (バリアを有効にするため使用オプション"barrier")

- Premium Storage ディスクのキャッシュ設定が ReadWrite の場合は、書き込みの耐久性を保証するためにバリアを有効にしてください。
- VM を再起動してもボリューム ラベルが変更されないようにするには、ディスクに対する UUID 参照で /etc/fstab を更新する必要があります。 参照することも [にデータ ディスクを Linux 仮想マシンを接続する方法](../virtual-machines/virtual-machines-linux-how-to-attach-disk)

次に示す Linux ディストリビューションは、Premium Storage での検証が完了しています。 Premium Storage を使用するときのパフォーマンスと安定性を高めるために、VM をこれらのバージョン (以降) の少なくとも 1 つにアップグレードすることをお勧めします。 また、バージョンによっては最新の LIS (Linux Integration Services v4.0 for Microsoft Azure) が必要になります。 下記のリンクからダウンロードとインストールを行ってください。 マイクロソフトは今後もイメージの検証を行い、この一覧に追加していきます。 これらのイメージのパフォーマンスは変動することが検証時に判明しています。また、ワークロードの特性とイメージの設定にも依存することにご注意ください。 ワークロードの種類に応じて、異なるイメージをチューニングします。
<table border="1" cellspacing="0" cellpadding="5" style="border: 1px solid #000000;">
<tbody>
<tr>
    <td><strong>ディストリビューション</strong></td>
    <td><strong>バージョン</strong></td>
    <td><strong>サポートされるカーネル</strong></td>
    <td><strong>サポートされるイメージ</strong></td>
</tr>
<tr>
    <td rowspan="4"><strong>Ubuntu</strong></td>
    <td>12.04</td>
    <td>3.2.0-75.110</td>
    <td>Ubuntu-12_04_5-LTS-amd64-server-20150119-en-us-30GB</td>
</tr>
<tr>
    <td>14.04</td>
    <td>3.13.0-44.73</td>
    <td>Ubuntu-14_04_1-LTS-amd64-server-20150123-en-us-30GB</td>
</tr>
<tr>
    <td>14.10</td>
    <td>3.16.0-29.39</td>
    <td>Ubuntu-14_10-amd64-server-20150202-en-us-30GB</td>
</tr>
<tr>
    <td>15.04</td>
    <td>3.19.0-15</td>
    <td>Ubuntu-15_04-amd64-server-20150422-en-us-30GB</td>
</tr>
<tr>
    <td><strong>SUSE</strong></td>
    <td>SLES 12</td>
    <td>3.12.36-38.1</td>
    <td>suse-sles-12-priority-v20150213<br>suse-sles-12-v20150213</td>
</tr>
<tr>
    <td><strong>CoreOS</strong></td>
    <td>584.0.0</td>
    <td>3.18.4</td>
    <td>CoreOS 584.0.0</td>
</tr>
<tr>
    <td rowspan="2"><strong>CentOS</strong></td>
    <td>6.5、6.6、6.7、7.0</td>
    <td></td>
    <td>
        <a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409"> LIS 4.0 (必須) </a> </br>
        *See note below
    </td>
</tr>
<tr>
    <td>7.1</td>
    <td>3.10.0-229.1.2.el7</td>
    <td>
        <a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409"> LIS 4.0 を推奨 </a> <br/>
        *See note below
    </td>
</tr>

<tr>
    <td rowspan="2"><strong>Oracle</strong></td>
    <td>6.4.</td>
    <td></td>
    <td><a href="http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409"> LIS 4.0 が必要 </a></td>
</tr>
<tr>
    <td>7.0</td>
    <td></td>
    <td>詳細についてはサポートにお問い合わせください。</td>
</tr>
</tbody>
</table>


### Openlogic Centos 用 LIS ドライバー

OpenLogic CentOS VM を実行しているお客様は、次のコマンドを実行して最新のドライバーをインストールする必要があります。

    sudo rpm -e hypervkvpd  ## (may return error if not installed, that's OK)
    sudo yum install microsoft-hyper-v

その後再起動して、新しいドライバーをアクティブ化する必要があります。



## Premium Storage を使用するときの料金と課金
Premium Storage を使用するときには、課金に関する次の考慮事項が適用されます。

- Premium Storage ディスクへの課金はプロビジョニングしたディスクのサイズによって異なります。 Azure のマップある表で指定されているディスクのサイズ (切り上げたもの) として、最も近い Premium Storage ディスク オプションに、 [のスケーラビリティおよびパフォーマンスのターゲット Premium Storage を使用する場合](#scalability-and-performance-targets-when-using-premium-storage) セクションです。 プロビジョニングされたディスクには、Premium Storage プランの月額料金を使用して、時間割りで計算して課金されます。 たとえば、P10 ディスクをプロビジョニングし、20 時間後にそのディスクを削除した場合は、P10 製品の 20 時間分に対して課金されます。 これは、実際にディスクに書き込まれたデータの量や、使用した IOPS/スループットには関係ありません。
- Premium Storage でのスナップショットについては、スナップショットで使用した追加の容量に対して課金されます。 詳細については、スナップショットは、次を参照してください。 [Blob のスナップショットを作成する](http://msdn.microsoft.com/library/azure/hh488361.aspx)です。
- [送信データ転送](http://azure.microsoft.com/pricing/details/data-transfers/) (Azure データ センターからのデータ) では、帯域幅使用量に対して課金されます。

Premium Storage、DS シリーズ Vm および G シリーズの Vm の料金の詳細を参照してください。

- [Azure Storage の料金](http://azure.microsoft.com/pricing/details/storage/)
- [仮想マシンの料金](http://azure.microsoft.com/pricing/details/virtual-machines/)

## 仮想マシンのデータ ディスク用に Premium Storage アカウントを作成する

ここでは、Azure プレビュー ポータル、Azure PowerShell、Azure コマンド ライン インターフェイス (Azure CLI) を使用して、Premium Storage アカウントを作成する方法を示します。 さらに、Premium Storage を使用するときの仮想マシンの作成や仮想マシンへのデータ ディスクのアタッチといった、Premium Storage アカウントの使用例を紹介します。

### Premium Storage を使用する Azure 仮想マシンを Azure プレビュー ポータルで作成する

ここでは、Azure プレビュー ポータルを使用して Premium Storage アカウントを作成する方法を説明します。

1.  サインイン、 [Azure ポータル](portal.azure.com)します。 チェック アウト、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/) まだサブスクリプションがないかどうかに提供します。

2.  [ハブ] メニューをクリックして **新規**します。

3.   **新規**, 、クリックして **すべて**です。 選択 **ストレージ、キャッシュ、+ バックアップ**します。 をクリックします **ストレージ** ] をクリックし、 **作成**します。

4.  [ストレージ アカウント] ブレードで、ストレージ アカウントの名前を入力します。 クリックして **価格レベル**します。  **価格レベルの推奨** ブレードで、をクリックして **[価格レベルの表示**します。  **価格レベルを選択して** ブレードで、選択 **Premium ローカル冗長**します。 クリックして **選択**します。 注意してください、 **ストレージ アカウント** ブレードで番組 **STANDARD-GRS** として、 **料金レベル** 既定です。 クリックした後 **選択**, 、 **料金レベル** として表示される **Premium LRS**します。

    ![価格レベル][Image1]


5.   **ストレージ アカウント** ブレードの既定値はそのまま **リソース グループ**, 、**サブスクリプション**, 、**場所**, 、および **診断**します。 クリックして **作成**します。

Azure 環境内部の完全なチュートリアルについては、次を参照してください。 [Azure プレビュー ポータルで実行されている Windows の仮想マシンを作成する](../virtual-machines-windows-tutorial-azure-preview.md)です。

### Premium Storage を使用する Azure 仮想マシンを Azure PowerShell で作成する
この PowerShell の例では、新しい Premium Storage アカウントを作成してこのアカウントを使用するデータ ディスクを新しい Azure 仮想マシンにアタッチする方法を説明します。

1. 次に指定された手順に従って、PowerShell 環境を設定 [をインストールして、Azure PowerShell を構成する方法](../install-configure-powershell.md)します。
2. PowerShell コンソールを起動してサブスクリプションに接続し、コンソール ウィンドウで次の PowerShell コマンドレットを実行します。 この PowerShell ステートメントからわかるように、指定する必要があります、 **型** パラメーターとして **Premium_LRS** premium storage アカウントを作成する場合。

        New-AzureStorageAccount -StorageAccountName "yourpremiumaccount" -Location "West US" -Type "Premium_LRS"

3. 次に、DS シリーズの VM を作成し、コンソール ウィンドウで次の PowerShell コマンドレットを実行して Premium Storage を指定します。 同様の手順で GS シリーズの VM を作成できます。 コマンドでは適切な VM サイズを指定してください。 例: Standard_GS2

        $storageAccount = "yourpremiumaccount"
        $adminName = "youradmin"
        $adminPassword = "yourpassword"
        $vmName ="yourVM"
        $location = "West US"
        $imageName = "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201409.01-en.us-127GB.vhd"
        $vmSize ="Standard_DS2"
        $OSDiskPath = "https://" + $storageAccount + ".blob.core.windows.net/vhds/" + $vmName + "_OS_PIO.vhd"
        $vm = New-AzureVMConfig -Name $vmName -ImageName $imageName -InstanceSize $vmSize -MediaLocation $OSDiskPath
        Add-AzureProvisioningConfig -Windows -VM $vm -AdminUsername $adminName -Password $adminPassword
        New-AzureVM -ServiceName $vmName -VMs $VM -Location $location

4. VM のディスク領域を増やしたい場合は、VM の作成後に、コンソール ウィンドウで次の PowerShell コマンドレットを実行して既存の DS シリーズまたは GS シリーズの VM に新しいデータ ディスクをアタッチします。

        $storageAccount = "yourpremiumaccount"
        $vmName ="yourVM"
        $vm = Get-AzureVM -ServiceName $vmName -Name $vmName
        $LunNo = 1
        $path = "http://" + $storageAccount + ".blob.core.windows.net/vhds/" + "myDataDisk_" + $LunNo + "_PIO.vhd"
        $label = "Disk " + $LunNo
        Add-AzureDataDisk -CreateNew -MediaLocation $path -DiskSizeInGB 128 -DiskLabel $label -LUN $LunNo -HostCaching ReadOnly -VM $vm | Update-AzureVm

### Premium Storage を使用する Azure 仮想マシンを Azure コマンド ライン インターフェイスで作成する

 [Azure-コマンドライン インターフェイス](../xplat-cli-install.md)(Azure CLI) は、クロスプラット フォーム コマンドを Azure プラットフォームで使用できるオープン ソースのセットを提供します。 以下の例では、Azure CLI (バージョン 0.8.14 以降) を使用して、Premium Storage アカウントを作成する方法、新しい仮想マシンを作成する方法、新しいデータ ディスクを Premium Storage アカウントからアタッチする方法を示します。

#### Premium Storage アカウントを作成する

````
azure storage account create "premiumtestaccount" -l "west us" --type PLRS
````

#### DS シリーズの仮想マシンを作成する

    azure vm create -z "Standard_DS2" -l "west us" -e 22 "premium-test-vm"
        "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_10-amd64-server-20150202-en-us-30GB" -u "myusername" -p "passwd@123"

#### 仮想マシンに関する情報を表示する

    azure vm show premium-test-vm

#### 新しいデータ ディスクをアタッチする

    azure vm disk attach-new premium-test-vm 20 https://premiumstorageaccount.blob.core.windows.net/vhd-store/data1.vhd

#### 新しいデータ ディスクに関する情報を表示する

    azure vm disk show premium-test-vm-premium-test-vm-0-201502210429470316

## 次のステップ

- [Azure Premium Storage での BLOB サービス操作の使用](http://go.microsoft.com/fwlink/?LinkId=521969)
- [Azure Premium Storage への移行](storage-migration-to-premium-storage.md)します。
- [Windows を実行する仮想マシンの作成](../virtual-machines-windows-tutorial-azure-preview.md)
- [Azure の仮想マシンおよびクラウド サービスのサイズ](../virtual-machines/virtual-machines-size-specs.md)
- [ストレージのドキュメント](http://azure.microsoft.com/documentation/services/storage/)

[Image1]: ./media/storage-premium-storage-preview-portal/Azure_pricing_tier.png
 


