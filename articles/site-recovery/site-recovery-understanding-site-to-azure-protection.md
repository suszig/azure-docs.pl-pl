<properties
    pageTitle="Azure Site Recovery を使用した Hyper-V レプリケーションについて | Microsoft Azure" 
    description="この記事を使用すると、Azure Site Recovery を正常にインストール、構成、管理するのに役立つ技術的な概念を理解できます。" 
    services="site-recovery" 
    documentationCenter="" 
    authors="anbacker" 
    manager="mkjain" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery" 
    ms.date="12/14/2015" 
    ms.author="anbacker"/>


# Azure Site Recovery を使用した Hyper-V レプリケーションについて

この記事では、Azure Site Recovery を使用した HYPER-V サイトまたは VMM サイトと Azure 間の保護を正常に構成および管理するのに役立つ、技術的な概念について説明します。

## コンポーネントについて

### オンプレミスと Azure の間のレプリケーションのための Hyper-V サイトまたは VMM サイトのデプロイ

オンプレミスと Azure の間で DR を設定するとき、Azure Site Recovery プロバイダーをダウンロードして VMM サーバーにインストールし、Azure Recovery Services エージェントを各 Hyper-V ホストにインストールする必要があります。

![オンプレミスと Azure の間のレプリケーションのための VMM サイトのデプロイ](media/site-recovery-understanding-site-to-azure-protection/image00.png)

Hyper-V サイトのデプロイは VMM のデプロイと同じです。プロバイダーとエージェントが Hyper-V ホスト自体にインストールされるという点だけ異なります。

## ワークフローについて

### 保護の有効化
ポータルまたは内部設置型から仮想マシンを保護した後、ASR ジョブが名前付き *保護の有効化* が開始され、[ジョブ] タブで監視できます。 

![オンプレミスの Hyper-V に関する問題のトラブルシューティング](media/site-recovery-understanding-site-to-azure-protection/image01.png)

*保護を有効にする* を呼び出す前に、ジョブが、前提条件をチェック [CreateReplicationRelationship](https://msdn.microsoft.com/library/hh850036.aspx) 保護中に構成されている入力を使用して Azure へのレプリケーションを作成します。 *保護を有効にする* ジョブは、初期レプリケーションを内部設置型から呼び出すことによって起動 [StartReplication](https://msdn.microsoft.com/library/hh850303.aspx) バーチャル マシンの仮想ディスクを Azure に送信します。

![オンプレミスの Hyper-V に関する問題のトラブルシューティング](media/site-recovery-understanding-site-to-azure-protection/image02.png)

### 保護の最終処理
A [HYPER-V 仮想マシン スナップショット](https://technet.microsoft.com/library/dd560637.aspx) が初期レプリケーションがトリガーされたときに実行します。 仮想ハード ディスクは、すべてのディスクが Azure にアップロードされるまで 1 つずつ処理されます。 通常、この処理の完了には、ディスク サイズと帯域幅に応じて、時間がかかります。 参照 [を内部設置型 Azure 保護ネットワーク帯域幅の使用を管理する方法](https://support.microsoft.com/kb/3056159) 、ネットワーク使用率を最適化するためです。 初期レプリケーションが完了すると *仮想マシンの保護の最終処理* ジョブは、ネットワークおよびレプリケーション後の設定を構成します。 初期レプリケーションの処理中は、次の「差分レプリケーション」セクションで説明するように、ディスクに対するすべての変更が追跡されます。 追加のディスク記憶域は、初期レプリケーションの処理中にスナップショット ファイルと HRL ファイルに使用されます。 初期レプリケーションが完了すると、HYPER-V VM スナップショットが削除されます。その結果、初期レプリケーション後のデータ変更が親ディスクにマージされます。

![オンプレミスの Hyper-V に関する問題のトラブルシューティング](media/site-recovery-understanding-site-to-azure-protection/image03.png)

### 差分レプリケーション
HYPER-V レプリカ レプリケーション トラッカーは、HYPER-V レプリカ レプリケーション エンジンの一部で、仮想ハード ディスクに対する変更を HYPER-V レプリケーション ログ (*.hrl) として追跡します。 HRL ファイルは、関連付けられているディスクと同じディレクトリにあります。 レプリケーション用に構成された各ディスクには、関連付けられた HRL ファイルがあります。 このログは、初期レプリケーションの完了後、顧客のストレージ アカウントに送信されます。 ログが Azure に送信される間、プライマリでの変更は同じディレクトリ内の別のログ ファイルで追跡されます。

記載された VM ビューで初期レプリケーションまたはデルタ レプリケーション中に VM のレプリケーション状態を監視できる [バーチャル マシンのレプリケーションの正常性監視](./site-recovery-monitoring-and-troubleshooting.md#monitor-replication-health-for-virtual-machine)します。  

### 再同期 
差分レプリケーションが失敗し、完全な初期レプリケーションのコストがネットワーク帯域幅または完了に要する時間の面で高くなる場合、仮想マシンは再同期の対象としてマークが付きます。 たとえば、HRL ファイルのサイズが合計ディスク サイズの 50% まで増大すると、仮想マシンは再同期の対象としてマークが付きます。 再同期では、ソースとターゲットの仮想マシン ディスクのチェックサムを計算して差分のみを送信することで、ネットワーク経由で送信されるデータの量が最小限に抑えられます。 

再同期が完了したら、通常の差分レプリケーションを再開する必要があります。 再同期を再開できるのは、障害 (ネットワークの停止、VMM のクラッシュなど) が発生した場合です。 

既定で *再同期を自動的にスケジュールされた* office 以外の作業時間中に構成されます。 仮想マシンは、手動で再同期する必要がある、選択をクリックして、ポータルから仮想マシンが再同期します。
![内部設置型 HYPER-V に関する問題をトラブルシューティングします。](media/site-recovery-understanding-site-to-azure-protection/image04.png)

再同期では、固定ブロック チャンク アルゴリズムが使用されています。このアルゴリズムでは、ソース ファイルとターゲット ファイルを固定チャンクに分割されます。つまり、チャンクごとにチェックサムが生成され、それを比較することで、ソースのどのブロックをターゲットに適用する必要があるかが特定されます。 

### 再試行ロジック
レプリケーション エラーが発生した場合に備えて、組み込みの再試行ロジックがあります。 このロジックは、次に示すように 2 つのカテゴリに分類できます。

| カテゴリ                  | シナリオ                                    |
|---------------------------|----------------------------------------------|
| 回復不可能なエラー     | 再試行は行われません。 仮想マシンのレプリケーションの状態が [重大] と表示され、管理者による操作が必要になります。 例としては、 <ul><li>VHD のチェーンが破壊</li><li>レプリカ仮想マシンが、無効な状態</li><li>ネットワーク認証エラー</li><li>認証エラー</li><li>スタンドアロン HYPER-V サーバーの場合、仮想マシンが見つからない場合</li></ul>|
| 回復可能なエラー         | レプリケーションを実行するたびに、指数関数的バックオフを使用した間隔で再試行が行われます。指数関数的バックオフでは、最初の試行が開始された後、再試行の間隔が指数関数的に長くなります (1、2、4、8、10 分)。 エラーが解決しない場合は、30 分ごとに再試行してください。 例としては、 <ul><li>ネットワーク エラー</li><li>ディスク領域不足</li><li>メモリ不足の状態</li></ul>|

## HYPER-V 仮想マシンの保護と回復のライフ サイクルについて

![HYPER-V の仮想マシンの保護と回復のライフ サイクルについて](media/site-recovery-understanding-site-to-azure-protection/image05.png)

## その他のリファレンス

- [VMware、VMM、Hyper-V、および物理サイトの監視とトラブルシューティング](./site-recovery-monitoring-and-troubleshooting.md)
- [Microsoft サポートを得る](./site-recovery-monitoring-and-troubleshooting.md#reaching-out-for-microsoft-support)
- [ASR の一般的なエラーとその解決策](./site-recovery-monitoring-and-troubleshooting.md#common-asr-errors-and-their-resolutions)
