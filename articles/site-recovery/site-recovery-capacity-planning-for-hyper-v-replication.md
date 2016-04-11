<properties
    pageTitle="Hyper-V 仮想マシンのレプリケーションの容量計画"
    description="この記事には、Azure Site Recovery の Hyper-V Capacity Planner ツールの使用方法を示す手順が含まれています。"
    services="site-recovery"
    documentationCenter="na"
    authors="rayne-wiselman"
    manager="jwhit"
    editor="" />
<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="12/01/2015"
    ms.author="raynew" />

# Hyper-V 仮想マシンのレプリケーションの容量計画

Azure Site Recovery では、Hyper-V レプリカを使用して、Hyper-V 仮想マシンをオンプレミスのサイトから Azure またはセカンダリ データ センターにレプリケートします。 Site Recovery の Capacity Planner ツールを使用すると、Hyper-V 仮想マシンのレプリケーションにおけるレプリケーション要件および帯域幅要件を容易に算出することができます。 

## 開始する前に

プライマリ サイトの Hyper-V サーバーまたはクラスター ノードでツールを実行します。 ツールを実行するには、Hyper-V ホスト サーバーが次の要件を満たしている必要があります。

- オペレーティング システム: Windows Server ® 2012 または Windows Server ® 2012 R2
- メモリ: 20 MB (最小)
- CPU: 5% オーバーヘッド (最小)
- ディスク領域: 5 MB (最小)

このツールを実行する前に、プライマリ サイトの準備を行う必要があります。 2 つのオンプレミスのサイト間でレプリケートする場合に帯域幅を確認するには、レプリカ サーバーの準備も必要です。


## 手順 1: プライマリ サイトを準備します。
1. プライマリ サイトでは、レプリケートする Hyper-V 仮想マシンと、それらが配置されている Hyper-V ホスト/クラスターとをすべて掲載した一覧を作成します。 ツールは毎回、複数のスタンドアロン サーバー ホストに対して、または単一のクラスターに対して実行できますが、両方に対して同時に実行することはできません。 また、ツールはオペレーティング システムごとに別々に実行する必要があります。したがって、Hyper-V サーバーを次のようにまとめてメモしておく必要があります。 

  - Windows Server ® 2012 スタンドアロン サーバー
  - Windows Server ® 2012 クラスター
  - Windows Server ® 2012 R2 スタンドアロン サーバー
  - Windows Server ® 2012 R2 クラスター

3. すべての Hyper-V ホストおよびクラスターで WMI へのリモート アクセスを有効にします。 次のコマンドを各サーバー/クラスターで実行して、ファイアウォール規則とユーザー アクセス許可が設定されていることを確認します。

        netsh firewall set service RemoteAdmin enable

5. 次のように、サーバーおよびクラスター上でパフォーマンスの監視を有効にします。

  - 強化された Windows ファイアウォールを開き、 **セキュリティが強化** スナップイン、ルールを有効に、次の受信: **COM + ネットワーク アクセス (DCOM-IN)** とすべてのルール、 **リモート イベント ログの管理グループ**します。

## 手順 2: レプリカ サーバーを準備します (オンプレミス間のレプリケーション)

Azure にレプリケートする場合、この手順は必要ありません。 

1 つの Hyper-V ホストを復旧サーバーとしてセットアップすることをお勧めします。そうすれば、そのサーバーにダミーの VM をレプリケートすることで帯域幅を調べることができます。  この作業は省略しても構いませんが、この作業を行わない限り帯域幅を測定することはできません。 

1. クラスター ノードをレプリカとして使用する場合は、Hyper-V レプリカ ブローカーを構成します。

    -  **サーバー マネージャー**, 、開かれている **フェールオーバー クラスター マネージャー**します。
    - クラスターに接続し、クラスター名を強調表示をクリックして **アクション** > **役割の構成** 高可用性ウィザードを開きます。
    -  **役割の選択** クリックして **HYPER-V レプリカ ブローカー**します。 ウィザードで指定する **NetBIOS 名** と **IP アドレス** (クライアント アクセス ポイントと呼ばれます)、クラスターへの接続ポイントとして使用します。  **HYPER-V レプリカ ブローカー** 構成はクライアント アクセス ポイント名にご注意ください。 
    - Hyper-V レプリカ ブローカーのロールがオンラインになることとクラスターの全ノード間でフェールオーバーできることを検証します。 これを行うには、右クリックして、ロールをポイントして **移動**, 、クリックして **ノードの選択**します。 ノードを選択 > **OK**します。 
    - 証明書ベースの認証を使用する場合は、各クラスター ノードとクライアント アクセス ポイントのすべてに証明書がインストールされていることを確認します。
2.  レプリカ サーバーを有効にします。

    - クラスターの障害クラスター マネージャーを開き、クラスターに接続し、をクリックして **ロール** > [ロール > **レプリケーション設定**s > **このクラスターをレプリカ サーバーとして有効にする**です。 クラスターをレプリカとして使用する場合は、プライマリ サイト内のクラスターにも Hyper-V レプリカ ブローカーのロールが存在する必要があるので注意してください。
    - スタンドアロン サーバーの場合は、Hyper-V マネージャーを開きます。  **アクション** ] ウィンドウで、をクリックして **HYPER-V 設定** を有効にするサーバーおよび **レプリケーションの構成** ] をクリックして **レプリカ サーバーとしてこのコンピューターを有効にする**です。
3. 認証をセットアップします。

    -  **認証とポート** をプライマリ サーバーと認証ポートを認証する方法を選択します。 証明書をクリックして使用する場合は **証明書の選択** いずれかを選択します。 プライマリと復旧の両方の Hyper-V ホストが同じドメインにあるか、信頼されたドメインにある場合は、Kerberos を使用します。 異なるドメイン、またはワークグループのデプロイの場合は証明書を使用します。
    -  **承認と記憶域** セクションで、許可 **、** このレプリカ サーバーにレプリケーション データを送信する (プライマリ) サーバーを認証します。 クリックして **OK** または **適用**します。

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)

    - 実行 **netsh http show servicestate** リスナーが実行されているプロトコルとポートにするかを確認するには指定します。  
4. ファイアウォールをセットアップします。 Hyper-V のインストール時には、既定のポートでトラフィックを許可するように (443 では HTTPS、80 では Kerberos)、ファイアウォール規則が作成されます。 次のように、これらのルールを有効にします。
    
        - Certificate authentication on cluster (443): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}**
        - Kerberos authentication on cluster (80): **Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}**
        - Certificate authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"**
        - Kerberos authentication on standalone server: **Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"**

## 手順 3: 容量計画ツールを実行します。

プライマリ サイトを準備し、復旧サーバーをセットアップすると、ツールを実行できます。

1. [ダウンロード](https://www.microsoft.com/download/details.aspx?id=39057) Microsoft ダウンロード センターからツールです。
2. いずれかのプライマリ サーバー (またはプライマリ クラスターのノード) からツールを実行します。 .Exe ファイルを右クリックし、[ **管理者として実行**します。
3.  **開始する前に** データを収集する期間を指定します。 代表的なデータが対象となるように、業務時間中にツールを実行することをお勧めします。 ネットワーク接続を検証するだけである場合は、数分の収集にとどめることもできます。

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)

4.   **プライマリ サイトの詳細** スタンドアロンのホストをサーバー名または FQDN を指定するか、クラスターでは、クライアントの FQDN を受け入れるポイントを指定する名前、またはクラスター内の任意のノードをクラスターし順にクリックして **次**します。 ツールは、それが実行されているサーバーの名前を自動的に検出します。 このツールは、指定されたサーバーについて監視できる仮想マシンを取得します。

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)

5.  **レプリカ サイト詳細** Azure にレプリケートする場合、またはセカンダリ データ センターにレプリケートして、レプリカ サーバーを設定していない場合は、[ **レプリカ サイトに関連するテストをスキップ**します。 セカンダリ データ センターにレプリケートしているし、スタンドアロン サーバーまたはクラスターにクライアント アクセス ポイントの FQDN でレプリカの種類を設定した場合 **サーバー名 (または) HYPER-V レプリカ ブローカー CAP**します。 

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)

6. 内で **拡張レプリカの詳細** を有効にする **拡張レプリカ サイトに関連するテストをスキップ**します。 これらは、Site Recovery によってサポートされていません。
7.  **レプリケートする Vm を選択して** プライマリ サーバーで実行されているディスクの設定に従ってで指定したとツールがサーバーまたはクラスターに接続し、Vm を表示、 **プライマリ サイトの詳細** ページです。 レプリケーションが既に有効になっている VM または実行していない VM は表示されないので注意してください。 メトリックスを収集する VM を選択します。 VHD を選択すると、VM のデータも自動的に収集されます。
9. レプリカのサーバーまたはクラスターを構成した場合は **ネットワーク情報** と思われるおおよそ WAN の帯域幅がプライマリとレプリカ サイト間で使用され、証明書の認証を構成した場合は、証明書を選択を指定します。

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)

10.  **概要** 設定を確認して、をクリックして **次** メトリックの収集を開始します。 ツールの進行状況と状態に表示される、 **処理能力の計算** ページです。 ときに、ツールの完了] をクリックを実行している **レポートの表示** に出力を参照してください。 既定にレポートとログを格納します。 **%systemdrive%\Users\Public\Documents\Capacity プランナー**します。

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)


## 手順 4: 結果を解釈します。
重要なメトリックスは次のとおりです。 ここに一覧されていないメトリックスは無視して構いません。 Site Recovery とは関連がないからです。

### オンプレミス間のレプリケーション
  - プライマリ ホストのコンピューティングとメモリに対するレプリケーションの影響
  - プライマリ、復旧ホストのストレージ ディスク領域、IOPS に対するレプリケーションの影響
  - デルタ レプリケーション (Mbps) に必要な帯域幅の合計
  - プライマリ ホストと復旧ホストの間で観察されたネットワーク帯域幅 (Mbps)
  - 2 つのホスト/クラスター間の有効な並列転送の理想的な数に関する提案

### オンプレミスから Azure へのレプリケーション
  - プライマリ ホストのコンピューティングとメモリに対するレプリケーションの影響
  - プライマリ ホストのストレージ ディスク領域と IOPS に対するレプリケーションの影響
  - デルタ レプリケーション (Mbps) に必要な帯域幅の合計

## その他のリソース

- ツールの詳細については、ダウンロード時にツールに付属していたドキュメントを参照してください。
- Keith Mayer のツールのチュートリアルを視聴 [TechNet ブログ](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx)します。
- [結果を得る](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx) の内部設置型、内部設置型 HYPER-V レプリケーションのためのテスト、パフォーマンス



## 次のステップ

- [オンプレミスの VMM サイトと Azure 間の保護の設定](site-recovery-vmm-to-azure.md)
- [オンプレミスの Hyper-V サイトと Azure 間の保護の設定](site-recovery-hyper-v-site-to-azure.md)
- [Set up protection between two on-premises VMM sites (2 つのオンプレミスの VMM サイト間の保護の設定)](site-recovery-vmm-to-vmm.md)

