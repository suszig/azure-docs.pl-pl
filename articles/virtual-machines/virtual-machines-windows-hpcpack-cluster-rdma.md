<properties
 pageTitle="MPI アプリケーションを実行するように Windows RDMA クラスターを設定する |Microsoft Azure"
 description="A8 または A9 サイズの VM を使用する Windows HPC Pack クラスターを作成し、Azure の RDMA ネットワークを使用して MPI アプリを実行する方法について説明します。"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="09/28/2015"
 ms.author="danlep"/>

# HPC Pack と A8 および A9 インスタンスを使用して Windows RDMA クラスターを設定して MPI アプリケーションを実行する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


この記事は、azure で Windows RDMA クラスターを設定する方法を示します [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) と [A8 および A9 のコンピューティングを消費するインスタンスのサイズを](virtual-machines-a8-a9-a10-a11-specs.md) 並列 Message Passing Interface (MPI) アプリケーションを実行します。 A8 および A9 サイズの Windows Server ベースのインスタンスを構成して、サポートされている MPI 実装を実行すると、MPI アプリケーションは、リモート ダイレクト メモリ アクセス (RDMA) テクノロジに基づく Azure の低待機時間で高スループットのネットワークを介して効率的に通信します。

>[AZURE.NOTE] Windows の azure の RDMA は現在、A8 および A9 インスタンス間で通信するために、Microsoft Network Direct インターフェイスを使用して MPI アプリケーションでサポートされています。
>
> Azure は、RDMA バックエンド ネットワークに接続せずに、A8 および A9 インスタンスと同じ処理機能を持つ、A10 および A11 のコンピューティング集中型インスタンスも提供します。 Azure で MPI ワークロードを実行する場合、一般に、A8 および A9 インスタンスで最良のパフォーマンスが得られます。

MPI ワークロードを Azure の RDMA ネットワークへのアクセスは、「Linux Vm 上で実行する場合は、 [MPI アプリケーションを実行する Linux RDMA クラスターをセットアップ](virtual-machines-linux-cluster-rdma.md)します。

## Windows HPC クラスターのデプロイ オプション
Microsoft HPC Pack は、Azure で Windows Server ベースの HPC クラスターを作成する場合に推奨されるツールです。 A8 および A9 インスタンスと組み合わせて使用した場合、HPC Pack は、Azure の RDMA ネットワークにアクセスする Windows ベースの MPI アプリケーションを効率的に実行する方法を提供します。 HPC Pack には、マイクロソフトによる Windows 用の Message Passing Interface の実装のためのランタイム環境が含まれます。

この記事にはクラスター化された A8 および A9 を展開するための 2 つのシナリオが導入されています
Microsoft HPC Pack でのインスタンス。

* シナリオ 1.  コンピューティング集中型 worker ロール インスタンスをデプロイする (PaaS)

* シナリオ 2.  計算ノードをコンピューティング集中型 VM にデプロイする (IaaS)

## 前提条件

* **レビュー [情報と考慮事項をバック グラウンド](virtual-machines-a8-a9-a10-a11-specs.md)** コンピューティング集中型インスタンスについての説明


* **Azure サブスクリプション** のアカウントを持っていない場合は、ほんの数分で無料の試用アカウントを作成することができます。 詳細については、[Azure の無料試用版サイト](http://azure.microsoft.com/pricing/free-trial/)をご覧ください。


* **コア クォータ** -A8 または A9 Vm のクラスターをデプロイするコアのクォータを増やす必要があります。 たとえば、HPC Pack と共に 8 個の A9 インスタンスをデプロイする場合は、少なくとも 128 コアが必要です。 クォータを増やすを開き、 [オンライン カスタマー サポートに申請](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) 無償でします。

## シナリオ 1.  コンピューティング集中型 worker ロール インスタンスをデプロイする (PaaS)


既存の HPC Pack クラスターから、クラウドで実行される Azure ワーカー ロール インスタンス (Azure ノード) でコンピューティング リソースを追加します。
サービス (PaaS)。 この機能は、HPC から「Azure にバースト」とも呼ばれます
パッケージの場合は、ワーカー ロール インスタンスのサイズの範囲をサポートしています。 使用するには
コンピューティング集中型インスタンスでは、単に A8 または A9 のサイズを指定時に
Azure ノードを追加します。

A8 または A9 Azure インスタンスにバーストする手順は、次のとおり、
既存の (通常内部設置型) クラスター。 同じような手順を使用してください。
展開されている HPC Pack ヘッド ノードにワーカー ロール インスタンスを追加するには
Azure VM です。

>[AZURE.NOTE] HPC Pack での Azure にバーストするチュートリアルについては、次を参照してください。 [HPC Pack 使用したハイブリッド クラスターをセットアップ](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md)します。 次の手順の考慮事項は、特に A8 および A9 サイズの Azure ノードに適用されます。

![Azure へのバースト][バースト]



4. **HPC Pack 2012 R2 ヘッド ノードをデプロイして構成する**

    最新の HPC Pack のインストール パッケージをダウンロード、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=49922)します。 要件と Azure バースト展開を準備する手順については、次を参照してください [HPC Pack ファースト ステップ ガイド](https://technet.microsoft.com/library/jj884144.aspx) と [Azure with Microsoft HPC Pack へのバースト。](https://technet.microsoft.com/library/gg481749.aspx)

5. **Azure のサブスクリプションで管理証明書を構成する**

    ヘッド ノードと Azure の間の接続をセキュリティで保護するための証明書を構成します。 オプションと手順では、次を参照してください。 [HPC Pack 用の Azure 管理証明書を構成するシナリオ](http://technet.microsoft.com/library/gg481759.aspx)します。

6. **新しいクラウド サービスとストレージ アカウントを作成する**

    Azure クラシック ポータルを使用して、コンピューティング集中型インスタンスが使用可能なリージョンでのデプロイメントのためにクラウド サービスとストレージ アカウントを作成します (クラウド サービスとストレージ アカウントは、他のデプロイに使用されている既存のアフィニティ グループに関連付けないでください)。

7. **Azure ノード テンプレートを作成する**

    HPC クラスター マネージャーのノード テンプレートの作成ウィザードを使用します。 手順については、次を参照してください。 [Azure ノード テンプレートを作成する](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Templ) "手順展開 Azure ノード Microsoft HPC Pack を使用する"にします。

    初期テスト用に、手動可用性ポリシーをテンプレートに構成することをお勧めします。

8. **クラスターにノードを追加する**

    HPC クラスター マネージャーのノードの追加ウィザードを使用します。 詳細については、次を参照してください。 [Windows HPC クラスターへの Azure ノードの追加](http://technet.microsoft.com/library/gg481758.aspx#BKMK_Add)します。

    ノードのサイズを指定するときに、A8 または A9 を選択します。

9. **ノードを開始 (プロビジョニング) し、オンラインにしてジョブを実行できるようにする**

    ノードを選択して、 **開始** アクションでは、HPC クラスター マネージャー。 プロビジョニングが完了すると、ノードを選択し、使用して、 **オンライン** アクションでは、HPC クラスター マネージャー。 ノードでジョブを実行する準備が整います。

10. **クラスターにジョブを送信する**

    HPC Pack のジョブ送信ツールを使用して、クラスター ジョブを実行します。 参照してください [Microsoft HPC Pack: ジョブ管理](http://technet.microsoft.com/library/jj899585.aspx)します。

11. **ノードを停止 (プロビジョニング解除) する**

    ジョブの実行が終わったら、ノードをオフラインにして使用して、 **停止** アクションでは、HPC クラスター マネージャー。


### 追加の考慮事項

* **プロキシ ノード** -各バーストへのコンピューティング処理を要するインスタンスをデプロイするで HPC Pack 自動的に最低 2 つの追加サイズ A8 インスタンスのプロキシ ノードとして指定する Azure ワーカー ロール インスタンスに加えて、します。 詳細については、次を参照してください。 [Azure プロキシ ノードの数を設定](https://technet.microsoft.com/library/jj899633.aspx)します。 プロキシ ノードは、サブスクリプションに割り当てられたコアを使用します。これにより、Azure worker ロールのインスタンスに加えて料金が発生します。

* **仮想ネットワーク** -HPC Pack PaaS 展開に関して、ポイント対サイト VPN の構成はサポートされていません。


## シナリオ 2.  計算ノードをコンピューティング集中型 VM にデプロイする (IaaS)

このシナリオでは、Azure 仮想ネットワークの Active Directory ドメインに参加している VM に HPC Pack ヘッド ノードとクラスター計算ノードをデプロイします。
 [HPC Pack IaaS デプロイメント
スクリプト](virtual-machines-hpcpack-cluster-powershell-script.md)
このプロセスのほとんどを自動化し、柔軟な展開オプション
クラスターで A8 または A9 VM のサイズを指定する機能を含む
ノードです。 次の手順を説明すると、この自動展開を使用して
メソッドを呼び出します。 また、Azure のクイック スタート テンプレートを使用して、リソース マネージャー デプロイメント モデルでクラスターをデプロイすることもできます。 テスト展開の場合は、Active Directory も手動で展開できます。
ドメイン、ヘッド ノード VM をコンピューティング ノード Vm との他の部分、
Azure で HPC Pack クラスター インフラストラクチャです。 参照してください [Azure での Microsoft HPC Pack での HPC クラスター オプション](virtual-machines-hpcpack-cluster-options.md)します。

![Azure Vm のクラスター][iaas]


1. **クライアント コンピューターで HPC Pack の IaaS デプロイ スクリプトを実行して、クラスター ヘッド ノードと計算ノード VM を作成する**

 HPC Pack IaaS デプロイメント スクリプトのパッケージをダウンロード、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=49922)します。

 クライアント コンピューターを準備するを作成する、スクリプトの構成ファイルと、スクリプトを実行 [HPC Pack IaaS デプロイメント スクリプトで HPC クラスターを作成](virtual-machines-hpcpack-cluster-powershell-script.md)します。 A8 および A9 サイズの計算ノードをデプロイする場合は、この記事の後半にある追加の考慮事項を参照してください。

2. **計算ノードをオンラインにしてジョブを実行できるようにする**

    ノードを選択して、 **オンライン** アクションでは、HPC クラスター マネージャー。 ノードでジョブを実行する準備が整います。

3. **クラスターにジョブを送信する**

    ヘッド ノードに接続してジョブを送信するか、またはオンプレミスのコンピューターを設定してジョブを送信します。 詳細については、次を参照してください。 [、HPC にジョブの送信は、Azure におけるクラスター](virtual-machines-hpcpack-cluster-submit-jobs.md)します。

4. **ノードをオフラインにし、停止 (割り当て解除) する**

    ジョブの実行が完了したら、HPC クラスター マネージャーでノードをオフラインにします。 次に、Azure の管理ツールを使用して、ノードをシャットダウンします。

### クラスターのデプロイ スクリプトを実行するための追加の考慮事項
* **仮想ネットワーク** -A8 および A9 インスタンスが使用可能な地域で新しい仮想ネットワークを指定します。


* **Windows Server オペレーティング システム** RDMA 接続をサポートして、Windows Server 2012 R2 または Windows Server 2012 オペレーティング システムのサイズの A8 または A9 コンピューティング ノード Vm を指定します。


* **クラウド サービス** - 1 つのクラウド サービスで、ヘッド ノードを展開することをお勧めし、A8 および A9 のコンピューティングとは異なるクラウド サービス内のノードです。


* **ヘッド ノードのサイズ** 以上のサイズを考慮 A8 または A9 のサイズのコンピューティング ノード Vm を追加するときに A4 (特大)、ヘッド ノードのです。


* **HpcVmDrivers 拡張機能** - 配置スクリプトは、Azure VM エージェントをインストールし、A8 または A9 サイズを展開するときに自動的に HpcVmDrivers 拡張機能が Windows Server オペレーティング システムを持つノードを計算します。 HpcVmDrivers は、RDMA ネットワークに接続するためのドライバーを計算ノード VM にインストールします。 参照してください [Azure VM 拡張機能と機能](virtual-machines-extensions-features.md)です。


* **クラスター ネットワークの構成** -配置スクリプトでは、トポロジ 5 (企業ネットワーク上のすべてのノード) で、HPC Pack クラスターを自動的に構成します。 このトポロジは、A8 または A9 サイズの計算ノードを持つデプロイを含め、VM でのすべての HPC Pack クラスターのデプロイに必要です。 クラスター ネットワークのトポロジを後で変更しないでください。

## A8 および A9 インスタンス上で MPI アプリケーションを実行する

### 例: HPC Pack クラスターで mpipingpong を実行する

コンピューティング集中型インスタンスの HPC Pack 展開を確認するには、HPC Pack を実行 **mpipingpong** コマンド、クラスターをします。 **mpipingpong**
計算するには、繰り返しのペアのノード間でデータのパケットを送信します。
待機時間とスループットの測定値と RDMA 対応の統計情報
アプリケーション ネットワークが実現します。 この例は、実行するための一般的なパターンを示しています、。
MPI ジョブ (この場合、 **mpipingpong**)、クラスターを使用して **mpiexec**
コマンドを使用して SSH キーを追加する必要が発生する場合もあります。

この例では、「Azure にバースト」内の Azure ノードを追加します。
構成 ([シナリオ 1](#scenario-1.-deploy-compute-intensive-worker-role-instances-(PaaS)) この記事で)。 Azure の仮想マシンのクラスターで HPC Pack を展開した場合
別のノードを指定するコマンドの構文を修正する必要があります。
グループ化し、ネットワーク トラフィックをリダイレクトするための他の環境変数を設定
RDMA ネットワークです。


クラスターで mpipingpong を実行するには:


1. ヘッド ノードまたは適切に構成されたクライアント コンピューターで、コマンド ウィンドウを開きます。

2. 4 ノードの Azure バースト デプロイ内のノード ペア間の待機時間を推定するために、次のコマンドを入力して、パケット サイズが小さく繰り返し数が多い設定で mpipingpong を実行するジョブを送信します。

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 1:100000 -op -s nul
    ```

    このコマンドを実行すると、送信されたジョブの ID が返されます。

    Azure Vm に展開されている HPC Pack クラスターを展開する場合を含むノード グループはコンピューティング ノード Vm の 1 つのクラウド サービスの展開を指定し、変更、 **mpiexec** コマンドを次のようにします。

  ```
  job submit /nodegroup:vmcomputenodes /numnodes:4 mpiexec -c 1 -affinity -env MSMPI\_DISABLE\_SOCK 1 -env MSMPI\_PRECONNECT all -env MPICH\_NETMASK 172.16.0.0/255.255.0.0 mpipingpong -p 1:100000 -op -s nul
  ```

3. ジョブが完了したら、出力 (この場合は、ジョブの 1 番目のタスクの出力) を表示するために次のように入力します。

    ```
    task view <JobID>.1
    ```

    ここで & lt;*JobID*& gt; 送信されたジョブの id を指定します。

    出力には、次のように待機時間の結果が含まれます。

    ![Ping pong の待機時間][pingpong1]

4. Azure バースト ノード ペア間のスループットを見積もるを実行するジョブを送信する次のコマンドを入力 **mpipingpong** 、大きなパケット サイズと小さな反復数。

    ```
    job submit /nodegroup:azurenodes /numnodes:4 mpiexec -c 1 -affinity mpipingpong -p 4000000:1000 -op -s nul
    ```

    このコマンドを実行すると、送信されたジョブの ID が返されます。

    Azure VM にデプロイされた HPC Pack クラスターで、手順 2. に示したようにコマンドを変更します。

5. ジョブが完了したら、出力 (この場合は、ジョブの 1 番目のタスクの出力) を表示するために次のように入力します。

    ```
    task view &lt;JobID&gt;.1
    ```
    
  出力には、次のようにスループットの結果が含まれます。

  ![Ping pong スループット][pingpong2]


### MPI アプリケーションに関する考慮事項


Azure で MPI アプリケーションを実行するための考慮事項を次に示します
名前で作成します。 Azure ノード (worker ロールの展開にのみ適用されるもの
インスタンスを追加構成では「Azure にバースト」) です。

* クラウド サービスの worker ロール インスタンスは、(たとえば、システムのメンテナンスやインスタンスの障害が原因で) Azure によって通知なく定期的に再プロビジョニングされます。 インスタンスが MPI ジョブの実行中に再プロビジョニングされた場合、インスタンスのすべてのデータが失われ、インスタンスが最初にデプロイされた状態に戻り、MPI ジョブが失敗する原因となります。 1 つの MPI ジョブに使用するノードの数が多いほど、およびジョブの実行に時間がかかるほど、ジョブの実行中にいずれかのインスタンスが再プロビジョニングされる可能性が高くなります。 デプロイの 1 つのノードをファイル サーバーとして指定する場合もこの点に注意する必要があります。


* Azure で MPI ジョブを実行するために A8 および A9 インスタンスを使用する必要はありません。 HPC Pack でサポートされている任意のインスタンス サイズを使用することができます。 ただし、待機時間やノードを接続するネットワークの帯域幅に依存する比較的大規模な MPI ジョブの実行には A8 および A9 インスタンスをお勧めします。 A8 と A9 以外のインスタンスを使用して待機時間と帯域幅に依存する MPI ジョブを実行する場合は、サイズが小さなジョブを実行して、1 つのタスクが少数のノードで実行されるようにすることをお勧めします。

* Azure インスタンスにデプロイされたアプリケーションには、アプリケーションに関連付けられているライセンス条件が適用されます。 クラウドで実行するためのライセンスまたはその他の制限事項については、商用アプリケーションのベンダーに確認してください。 すべてのベンダーが従量課金制ライセンスを提供しているとは限りません。


* オンプレミスのノード、共有、ライセンス サーバーにアクセスするには、Azure インスタンスでさらに設定が必要になります。 たとえば、オンプレミスのライセンス サーバーにアクセスできるように Azure ノードを有効にする場合、サイト間 Azure 仮想ネットワークを構成できます。


* Azure インスタンスで MPI アプリケーションを実行する各 MPI アプリケーションで Windows ファイアウォールをインスタンスで実行して、登録、 **hpcfwutil** コマンドです。 これにより、ファイアウォールによって動的に割り当てられたポートで MPI 通信が実行されます。

    >[AZURE.NOTE] Azure のデプロイにバースト、クラスターに追加するすべての新しい Azure ノードで自動的に実行するファイアウォール例外コマンドを構成することもできます。 実行した後、 **hpcfwutil** コマンドを使用し、アプリケーションの動作が、Azure ノードのスタートアップ スクリプトにコマンドを追加することを確認します。 詳細については、次を参照してください。 [Azure ノードのスタートアップ スクリプトを使用して](https://technet.microsoft.com/library/jj899632(v=ws.10).aspx)します。



* HPC Pack では、CCP_MPI_NETMASK クラスター環境変数を使用して、MPI 通信で許容されるアドレスの範囲を指定します。 HPC Pack 2012 R2 以降では、CCP_MPI_NETMASK クラスター環境変数は、(オンプレミスまたは Azure VM の) ドメインに参加しているクラスター計算ノード間の MPI 通信にのみ影響を与えます。 この変数は、Azure へのバースト構成で追加されたノードで無視されます。


* MPI ジョブは、異なるクラウド サービス (たとえば、異なるノード テンプレートを使用した Azure へのバースト デプロイ、または複数のクラウド サービスにデプロイされた Azure VM 計算ノード) にデプロイされた Azure インスタンス間で実行できません。 異なるノード テンプレートを使用して開始された複数の Azure ノード デプロイの場合は、MPI ジョブを 1 つの Azure ノードのセットで実行する必要があります。


* Azure ノードをクラスターに追加してオンラインにすると、HPC ジョブ スケジューラ サービスは、すぐにノードでジョブを開始しようとします。 ワークロードの一部のみを Azure で実行できる場合は、ジョブ テンプレートを更新または作成して、Azure で実行できるジョブの種類を定義してください。 たとえば、ジョブ テンプレートを使用して送信されたジョブが Azure ノードでのみ実行されるようにするには、ジョブ テンプレートにノード グループ プロパティを追加し、必須の値として AzureNodes を選択します。 Azure ノードに対応するカスタム グループを作成する場合、Add-HpcGroup HPC PowerShell コマンドレットを使用できます。


## 次のステップ

* Linux MPI Azure RDMA ネットワークへのアクセスは、「アプリケーションを実行する場合は、 [MPI アプリケーションを実行する Linux RDMA クラスターをセットアップ](virtual-machines-linux-cluster-rdma.md)します。

<!--Image references-->
[burst]: ./media/virtual-machines-windows-hpcpack-cluster-rdma/burst.png
[iaas]: ./media/virtual-machines-windows-hpcpack-cluster-rdma/iaas.png
[pingpong1]: ./media/virtual-machines-windows-hpcpack-cluster-rdma/pingpong1.png
[pingpong2]: ./media/virtual-machines-windows-hpcpack-cluster-rdma/pingpong2.png

