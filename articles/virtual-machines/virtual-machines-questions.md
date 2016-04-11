<properties
    pageTitle="VM に関してよく寄せられる質問 | Microsoft Azure"
    description="クラシック デプロイ モデルで作成された Azure 仮想マシンについてよく寄せられる質問への回答を示します。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/16/2015"
    ms.author="cynthn"/>

# クラシック デプロイ モデルで作成された Azure Virtual Machines についてよく寄せられる質問

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


この記事では、クラシック デプロイ モデルで作成された Azure Virtual Machines についてユーザーからよく寄せられる質問に回答します。

## Azure VM では何を実行できますか。

すべてのサブスクライバーは、Azure 仮想マシンでサーバー ソフトウェアを実行できます。 最近のバージョンの Windows Server だけでなく、さまざまな Linux ディストリビューションを実行できます。 サポートの詳細については、次の項目を参照してください。

• Windows Vm の場合-- [Microsoft Azure Virtual Machines のサーバー ソフトウェアのサポート](http://go.microsoft.com/fwlink/p/?LinkId=393550)

• Linux Vm の場合-- [Linux Azure-動作保証済みディストリビューション](http://go.microsoft.com/fwlink/p/?LinkId=393551)

Windows クライアント イメージについては、 MSDN Azure 特典のサブスクライバーと MSDN 開発テスト用従量課金制プラン (開発およびテスト用) のサブスクライバーを対象に、特定のバージョンの Windows 7 および Windows 8.1 が利用可能となっています。 手順については、制限などの詳細を参照してください [MSDN サブスクライバー向けの Windows クライアント イメージ](http://azure.microsoft.com/blog/2014/05/29/windows-client-images-on-azure/)します。

## 仮想マシンではどれくらいのストレージ容量を使用できますか。

各データ ディスクで最大 1 TB (テラバイト) を利用できます。 使用できるデータ ディスクの数は、仮想マシンのサイズによって決まります。 詳細については、「 [仮想マシンのサイズ](virtual-machines-size-specs.md)します。

Azure のストレージ アカウントでは、オペレーティング システム ディスクと任意のデータ ディスクのストレージを利用できます。 各ディスクは、実際には .vhd ファイルであり、ページ BLOB として保存されます。 料金の詳細について、次を参照してください。 [ストレージの料金詳細](http://go.microsoft.com/fwlink/p/?LinkId=396819)します。

## どのようなタイプの仮想ハード ディスクを使用できますか。

Azure では、VHD 形式の仮想ハード ディスク (固定型) のみをサポートしています。 HYPER-V マネージャーを使用して変換する必要がある場合は、Azure で使用する VHDXyou 場合がある場合は、または [CONVERT-VHD](http://go.microsoft.com/fwlink/p/?LinkId=393656) コマンドレットです。 その後を使用して [Add-azurevhd](https://msdn.microsoft.com/library/azure/dn495173.aspx) コマンドレットが (サービス管理モード) の仮想マシンで使用できるように、Azure でストレージ アカウントに、VHD をアップロードします。 

- Linux の手順については、次を参照してください。 [の作成とアップロード Linux オペレーティング システムを含むバーチャル ハード ディスク](virtual-machines-linux-create-upload-vhd.md)します。

- Windows の手順については、次を参照してください。 [の作成とアップロード Azure への Windows Server VHD](virtual-machines-create-upload-vhd-windows-server.md)します。

## これらの仮想マシンは、HYPER-V 仮想マシンと同じですか。

多くの点で "第 1 世代" の Hyper-V VM と似ていますが、まったく同じというわけではありません。 いずれのタイプも仮想化されたハードウェアを提供し、VHD 形式の仮想ハードディスクと互換性があります。 つまり、Hyper-V と Azure の間で移動させることができます。 HYPER-V ユーザーに驚かれることの多い主な違いは次の 3 点です。

- Azure では、仮想マシンにアクセスするためのコンソールが提供されません。 起動が完了するまで、VM にアクセスする方法はありません。
- ほとんどの azure Vm [サイズ](virtual-machines-size-specs.md) もがあることを 1 つだけの外部 IP アドレスを 1 つだけの仮想ネットワーク アダプターがあります。 (A8 サイズと A9 サイズでは、インスタンス間でのアプリケーション通信に 2 つ目のネットワーク アダプターが使用される場合があります。)
- Azure Vm では、第 2 世代の HYPER-V VM の機能はサポートされていません。 これらの機能に関する詳細については、「 [Hyper-v の仮想マシンの仕様](http://technet.microsoft.com/library/dn592184.aspx) と [ジェネレーション 2 仮想マシンの概要] (https://technet.microsoft.com/library/dn282285.aspx).

## これらの仮想マシンで、既存のオンプレミス ネットワーク インフラストラクチャを使用することはできますですか。

クラシック デプロイ モデルで作成された仮想マシンの場合は、Azure Virtual Network を使用して既存のインフラストラクチャを拡張できます。 このアプローチは、ブランチ オフィスのセットアップに似ています。 Azure 上で仮想プライベート ネットワーク (VPN) をプロビジョニングして管理できるだけでなく、それらの VPN をオンプレミスの IT インフラストラクチャにセキュアに接続することもできます。 詳細については、「 [仮想ネットワークの概要](../virtual-network/virtual-networks-overview.md)します。

仮想マシンを作成する際には、仮想マシンの参加先となるネットワークを指定する必要があります。 既存の仮想マシンを仮想ネットワークに参加させることはできません。 ただし、仮想ハードディスク (VHD) を既存の仮想マシンからデタッチし、それを使用して、目的のネットワーク構成で新しい仮想マシンを作成することにより、この問題を回避することは可能です。

## 仮想マシンをアクセスする方法はありますか

Windows 仮想マシンまたは Linux VM 用の Secure Shell (SSH) に対するリモート デスクトップ接続を使用してリモート接続を確立し、仮想マシンにログオンする必要があります。 手順については、次の項目を参照してください。

- [Windows Server を実行する仮想マシンにログオンする方法](virtual-machines-log-on-windows-server.md)します。 最大 2 つの同時接続がサポートされます (サーバーがリモート デスクトップ サービスのセッション ホストとして構成されている場合を除く)。  
- [Linux を実行する仮想マシンにログオンする方法](virtual-machines-linux-how-to-log-on.md)します。 SSH では、既定で最大 10 の同時接続が可能です。 この接続数は構成ファイルを編集することで増やすことができます。

リモート デスクトップまたは SSH について問題がある場合は、インストールして使用する、 [VMAccess](virtual-machines-extensions-features.md) 問題を修正するために拡張します。 

Windows VM の場合は、次のオプションもあります。

- Azure クラシック ポータルで VM を検索] をクリックし、 **リモート アクセスのリセット** コマンド バーからです。
- レビュー [Windows ベースの Azure 仮想マシンへのリモート デスクトップのトラブルシューティングを行う接続](virtual-machines-troubleshoot-remote-desktop-connections.md)します。
- Windows PowerShell リモート処理を使用して VM に接続するか、その他のリソースに対する追加のエンドポイントを作成して VM に接続する。 詳細については、「 [仮想マシンに対してエンドポイントを設定する方法](virtual-machines-set-up-endpoints.md)します。

Hyper-V に慣れている場合は、VMConnect と同様のツールを検討されるかもしれませんが、 Azure では、仮想マシンへのコンソール アクセスがサポートされていないため、類似のツールは提供されていません。

## 一時ディスク (Windows の場合は D: ドライブ、Linux の場合は /dev/sdb1) を使用してデータを保存できますか。

一時ディスク (Windows のデフォルトは D: ドライブ、Linux は /dev/sdb1) にデータを保存することはできません。 一時ディスクは一時的なストレージでしかなく、データ損失の発生時にデータを復旧できない恐れがあります。 このようなことは、仮想マシンを別のホストに移動するときに発生する可能性があります。 仮想マシンが移動される理由としては、ホストの更新、仮想マシンのサイズ変更、ホスト上のハードウェア障害などが挙げられます。

## 一時ディスクのドライブ文字を変更する方法について教えてください。

Windows 仮想マシンでは、ページ ファイルを移動してドライブ文字を再割り当てすることでドライブ文字を変更することができますが、正しい順序で手順を実行するよう注意する必要があります。 手順については、次を参照してください。 [Windows 一時ディスクのドライブ文字を変更](virtual-machines-windows-change-drive-letter.md)します。

## ゲスト オペレーティング システムのアップグレード方法について教えてください。

アップグレードという言葉は一般に、オペレーティング システムを現在のハードウェアのままで新しいリリースに移行することを指します。 Azure VM の場合、新しいリリースに移行するプロセスは Windows や Linux の場合と異なります。

- Linux VM の場合、配布には適切なパッケージ管理ツールと手順を使用してください。
- Windows 仮想マシンの場合、Windows Server 移行ツールのようなものを使用してサーバーを移行する必要があります。 ゲスト OS が Azure 上に存在する状態でアップグレードを行うことは避けてください。 仮想マシンへのアクセスが失われる恐れがあるため、この操作はサポートされていません。 アップグレード中に問題が発生すると、リモート デスクトップ セッションを開始できなくなり、問題のトラブルシューティングができなくなる可能性があります。 

ツールや Windows Server を移行するためのプロセスに関する詳細については、次を参照してください。 [移行の役割と機能 Windows Server を](http://go.microsoft.com/fwlink/p/?LinkId=396940)します。 



## 仮想マシンでの既定のユーザー名とパスワードを教えてください。

Azure によって提供されるイメージには、事前に構成されたユーザー名とパスワードはありません。 これらのイメージのいずれかを使用して仮想マシンを作成する際は、仮想マシンへのログオンに使用するユーザー名とパスワードを指定する必要があります。

VM エージェントをインストールしたパスワードを忘れてしまった場合は、ユーザー名、または、インストールして使用して、 [VMAccess](virtual-machines-extensions-features.md) 問題を解決する拡張機能です。

追加情報:


- Linux イメージの場合、Azure クラシック ポータルを使用すると既定のユーザー名として「azureuser」が指定されますが、仮想マシンの作成に [簡易作成] ではなく [ギャラリーから] を使用すれば、ユーザー名をを変更できます。 また [ギャラリーから] では、ログインにパスワードを使用するか、SSH キーを使用するか、それとも両方を使用するかを指定することもできます。 既定のユーザー アカウントは、特権のあるコマンドを実行するための "sudo" アクセス権を付与された、特権のないユーザーです。 "root" アカウントは無効化されます。


- Windows イメージの場合は、VM の作成時にユーザー名とパスワードを指定する必要があります。 アカウントは Administrators グループに追加されます。

## Azure では、仮想マシン上でウイルス対策を実行できますか。

Azure ではウイルス対策ソリューションとしていくつかのオプションが提供されていますが、管理はユーザーに委ねられています。 これは、マルウェア対策ソフトウェアを個別のサブスクリプションで導入したい場合や、スキャン実行、更新プログラム インストールのタイミングを指定したい場合などを考慮したものです。 ユーザーは、Windows 仮想マシンの作成時やそれ以降のタイミングで、Microsoft マルウェア対策、Symantec Endpoint Protection、または TrendMicro Deep Security Agent 用の VM 拡張機能により、ウイルス対策サポートを追加することができます。 Symantec と TrendMicro 用の拡張機能では、無償の期間限定の試用版サブスクリプションか、既存のエンタープライズ サブスクリプションを使用することができます。 Microsoft マルウェア対策は無償です。 詳細については、次のリンクを参照してください。

- [How to install and configure Symantec Endpoint Protection on an Azure VM (Azure VM に Symantec Endpoint Protection をインストールし、構成する方法)](http://go.microsoft.com/fwlink/p/?LinkId=404207)
- [How to install and configure Trend Micro Deep Security as a Service on an Azure VM (Azure VM に Trend Micro Deep Security をサービスとしてインストールし、構成する方法)](http://go.microsoft.com/fwlink/p/?LinkId=404206)
- [Azure Virtual Machines へのマルウェア対策ソリューションのデプロイ](http://azure.microsoft.com/blog/2014/05/13/deploying-antimalware-solutions-on-azure-virtual-machines/)

## バックアップと回復についてはどのようなオプションがありますか。

Azure Backup は、特定の地域でプレビューとして提供されています。 詳細については、「 [Azure の仮想マシンをバックアップ](backup-azure-vms.md)します。 その他のソリューションは認定パートナーから利用できます。 利用可能なソリューションについては、Azure Marketplace を検索して確認してください。

追加オプションとして、BLOB ストレージのスナップショット機能を使用することもできます。 これを使用する場合は、BLOB スナップショットに依存する操作を実行する前に、VM を停止する必要があります。 これにより、保留中のデータの書き込みが回避され、ファイル システムが一貫性のある状態に維持されます。

## Azure では VM の料金はどのように課金されますか。

Azure では、VM のサイズおよびオペレーティング システムに基づいて時間単位の料金を請求します。 時間単位を満たさない場合は、分単位でのみ請求します。 特定の事前インストール済みソフトウェアを含んだ VM イメージで VM を作成する場合は、1 時間ごとの追加ソフトウェア料金が適用される場合があります。 また Azure では、VM のオペレーティング システムとデータ ディスクについて、別途のストレージ料金が課金されます。 一時ディスク ストレージは無料です。

VM が実行中または停止状態のときには料金が発生しますが、VM が停止 (割り当て解除) 状態のときには料金は課金されません。 VM を停止 (割り当て解除) 状態にするには、次のいずれかを行います。

- Azure クラシック ポータルから VM をシャット ダウンまたは削除する。
- Stop-AzureVM コマンドレットを使用する (Azure PowerShell モジュールで利用可能)。
- サービス管理 REST API でロールのシャット ダウン操作を使用し、PostShutdownAction 要素に StoppedDeallocated を指定する。

詳細については、次を参照してください。 [Virtual Machines 料金](http://azure.microsoft.com/pricing/details/virtual-machines/)します。

## Azure では、メンテナンスのために VM が再起動されることはありますか。

Azure では、Azure データ センターでの定期的な計画メンテナンス更新の一環として、VM が再起動されることがあります。 

また、VM に影響する重大なハードウェア問題が Azure で検出された場合には、計画外のメンテナンス イベントが発生する場合もあります。 計画外イベントの場合、Azure は VM を正常な状態のホストへと自動的に移行し、VM を再起動します。

スタンドアロン VM (可用性セットに含まれない VM) については、計画メンテナンスの 1 週間前までに Azure からサブスクリプションのサービス管理者に電子メールが送られ、更新中に VM  が再起動される可能性がある旨が通知されます。 その場合、VM 上で実行されているアプリケーションにダウンタイムが発生する可能性があります。

計画メンテナンスのために再起動が発生した場合、利用者は Azure クラシック ポータルまたは Azure PowerShell を使用して再起動のログを確認できます。 詳細については、「 [VM の再起動ログの表示](http://azure.microsoft.com/blog/2015/04/01/viewing-vm-reboot-logs/)します。

冗長性を確保する必要がある場合は、同様に構成された VM を同じ可用性セット内に 2 つ以上配置してください。 そうすることで、計画メンテナンスや計画外メンテナンスの際にも、最低 1 つの VM を利用できるようになります。 Azure では、この構成について一定レベルの VM 可用性を保証しています。 詳細については、「 [仮想マシンの可用性管理](virtual-machines-manage-availability.md)します。

## その他のリソース

[Azure の仮想マシンについて](virtual-machines-about.md)

[Linux 仮想マシンを作成するさまざまな方法](virtual-machines-linux-choices-create-vm.md)

[Windows 仮想マシンを作成するさまざまな方法](virtual-machines-windows-choices-create-vm.md)

