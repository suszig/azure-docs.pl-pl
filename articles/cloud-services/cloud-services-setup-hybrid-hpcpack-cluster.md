<properties
    pageTitle="Microsoft HPC Pack を使用したハイブリッド コンピューティング クラスターのセットアップ"
    description="Microsoft HPC Pack と Azure を使用して、小規模なハイブリッド ハイパフォーマンス コンピューティング (HPC) クラスターをセットアップする方法について説明します。"
    services="cloud-services"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""
    tags="azure-service-management,hpc-pack"/>

<tags
    ms.service="cloud-services"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2015"
    ms.author="danlep"/>


# Microsoft HPC Pack を使用したハイブリッド コンピューティング クラスターのセットアップ
このチュートリアルでは、Microsoft HPC Pack 2012 R2 と Azure を使用して、小規模なハイブリッド ハイパフォーマンス コンピューティング (HPC) クラスターをセットアップする方法について説明します。 クラスターは、オンプレミスのヘッド ノード (Windows Server オペレーティング システムと HPC Pack を実行するコンピューター) と、Azure クラウド サービス上に worker ロール インスタンスとしてオンデマンドでデプロイされるいくつかの計算ノードで構成されます。 コンピューティング ジョブを、ハイブリッド クラスター上で実行することができます。

![ハイブリッド HPC クラスター][Overview]

このチュートリアルでは、「クラウド バースト」とも呼ばれる方法を紹介します。Azure 上で多くのコンピューティング処理を要するアプリケーションを実行するためにコンピューティング リソースをオンデマンドで使用する、拡張性に優れた方法です。

このチュートリアルは、コンピューティング クラスターまたは HPC Pack を使用した経験がない読者を対象に作成されています。 これは、ハイブリッド コンピューティング クラスターを迅速にデプロイする方法のデモンストレーションを目的としています。 考慮事項と、運用環境での大規模なハイブリッド HPC Pack クラスターを展開する手順では、次を参照してください。、 [ガイダンスを詳細な](http://go.microsoft.com/fwlink/p/?LinkID=200493)です。 完全に Azure の HPC Pack クラスターを設定する場合は、「 [Azure での Microsoft HPC Pack での HPC クラスター オプション](../virtual-machines/virtual-machines-hpcpack-cluster-options.md)します。

>[AZURE.NOTE] Azure は、 [さまざまなサイズ](../virtual-machines/virtual-machines-size-specs.md) 、コンピューティング リソースは、さまざまなワークロードに適した向けです。 たとえば、A8 と A9 インスタンスでは、高いパフォーマンスとアクセスが組み合わされ、特定の HPC アプリケーションで必要となる、待機時間が短い高スループットのアプリケーション ネットワークが実現されます。 参照してください [A8、A9、A10、A11 コンピューティング集中型インスタンスについて](../virtual-machines/virtual-machines-a8-a9-a10-a11-specs.md)します。

## 前提条件

>[AZURE.NOTE]このチュートリアルを完了するには、Azure アカウントが必要です。 アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、「 [Azure アカウントの作成](http://azure.microsoft.com/develop/php/tutorials/create-a-windows-azure-account/)します。

さらに、このチュートリアルでは次のものが必要です。

* Windows Server 2012 R2 または Windows Server 2012 のいずれのエディションかを実行しているオンプレミスのコンピューターが必要です。 このコンピューターが HPC クラスターのヘッド ノードになります。 Windows Server をまだ実行されていない場合は、ダウンロードし、インストール、 [評価版](http://technet.microsoft.com/evalcenter/dn205286.aspx)します。

    * コンピューターは Active Directory ドメインに結合されている必要があります。

    * 追加のサーバー ロールまたはロール サービスがインストールされていないことを確認します。

    * HPC Pack をサポートするには、オペレーティング システムは、英語、日本語、中国語 (簡体字) のいずれかでインストールする必要があります。

    * 重要な更新プログラムがインストールされていることを確認します。

* HPC Pack 2012 R2 のインストール ファイル。無料で利用できます。 [ダウンロード](http://go.microsoft.com/fwlink/p/?linkid=328024) 最新のバージョン、ヘッド ノード コンピューターまたはネットワーク上の場所にファイルをコピーします。 Windows Server のインストールと同じ言語を選択してください。

* ヘッド ノード上にローカル管理者権限があるドメイン アカウント。

* ヘッド ノードから Azure への TCP 接続 (ポート 443)。

## HPC Pack をヘッド ノードにインストールする

最初に、HPC クラスターのヘッド ノードにする、Windows Server が実行されているオンプレミスのコンピューターに Microsoft HPC Pack をインストールします。

1. ローカル管理者権限があるドメイン アカウントでヘッド ノードにログオンします。

2. HPC Pack のインストール ファイルから Setup.exe を実行して、HPC Pack のインストール ウィザードを開始します。

3.  **HPC Pack 2012 R2 セットアップ** 画面で、[ **新規インストールまたは既存のインストールに新しい機能の追加**します。

    ![HPC Pack 2012 セットアップ][install_hpc1]

4.  **Microsoft ソフトウェア使用許諾契約書] ページ**, 、] をクリックして **次**します。

5.  **[インストールの種類** ] ページで [ **ヘッド ノードを作成することで新しい HPC クラスターを作成**, 、] をクリックし、 **次**します。

    ![インストールの種類の選択][install_hpc2]

6. ウィザードによってインストール前のテストがいくつか実行されます。 をクリックして **次** 上、 **インストール ルール** ] ページですべてのテストに合格します。 失敗した場合は、表示された情報を確認して、必要な変更を環境に加えます。 テストをもう一度実行するか、必要に応じてインストール ウィザードをもう一度開始します。

    ![インストール規則][install_hpc3]

7.  **HPC DB 構成** ] ページで確認 **ヘッド ノードの** と、すべての HPC データベースを選択し、をクリックして **次**します。

    ![DB 構成][install_hpc4]

8. ウィザードの残りのページは既定の設定のままにします。  **ために必要なコンポーネントのインストール** ] ページで [ **インストール**します。

    ![インストール][install_hpc6]

9. インストールの完了後にボックスをオフに **HPC クラスター マネージャーを開始** ] をクリックし、 **完了**します。 (ヘッド ノードの構成を完了するために HPC クラスター マネージャーを後の手順で開始します)。

    ![完了][install_hpc7]

## Azure サブスクリプションを準備する
使用して、 [Azure クラシック ポータル](https://manage.windowsazure.com) 、Azure サブスクリプションに次の手順を実行します。 これは、後で、オンプレミスのヘッド ノードから Azure ノードをデプロイするために必要です。

- 管理証明書のアップロード (ヘッド ノードと Azure サービス間の接続をセキュリティ保護するために必要です)

- Azure ノード (worker ロール インスタンス) を実行する Azure クラウド サービスの作成

- Azure のストレージ アカウントの作成

    >[AZURE.NOTE]Azure サブスクリプション ID、後でする必要を書き留めます。 この ID を確認するには、Azure の<a href="[https://account.windowsazure.com/Subscriptions">アカウント情報</a>を調べてください。

### <a>既定の管理証明書のアップロード</a>
HPC Pack ではヘッド ノードに自己署名証明書 (既定の Microsoft HPC Azure 管理証明書) がインストールされます。これを Azure 管理証明書としてアップロードできます。 この証明書は、テスト目的および概念実証のデプロイ用に提供されます。

1. ヘッド ノード コンピューターからへのサインイン、 [Azure クラシック ポータル](https://manage.windowsazure.com)します。

2. クリックして **設定**, 、] をクリックし、 **管理証明書**します。

3. コマンド バー] をクリックして **アップロード**します。

    ![証明書の設定][upload_cert1]

4. ヘッド ノードでファイル C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer を参照し、 クリックして、 **チェック** ] ボタンをクリックします。

    ![証明書のアップロード][install_hpc10]

表示されます **既定の HPC Azure 管理** 管理証明書の一覧にします。

### <a>Azure クラウド サービスの作成</a>

>[AZURE.NOTE]最適なパフォーマンスを同じリージョンでクラウド サービスとストレージ アカウントを作成します。

1. コマンド バーで、ポータルで、クリックして **新規**します。

2. クリックして **コンピューティング**, 、] をクリックして **クラウド サービス**, 、] をクリックし、 **簡易作成**します。

3. クラウド サービスの URL を入力し、クリックして **クラウド サービスの作成**します。

    ![サービスの作成][createservice1]

### <a>Azure のストレージ アカウントの作成</a>

1. コマンド バーで、ポータルで、クリックして **新規**します。

2. クリックして **Data Services**, 、] をクリックして **ストレージ**, 、順にクリック **簡易作成**します。

3. アカウントの URL を入力し、クリックして **ストレージ アカウントの作成**します。

    ![ストレージの作成][createstorage1]

## ヘッド ノードを構成する

HPC クラスター マネージャーを使用して Azure ノードをデプロイし、ジョブを送信するには、最初にクラスターの構成手順をいくつか実行する必要があります。

1. ヘッド ノードで、HPC クラスター マネージャーを開始します。 場合、 **ヘッド ノードの選択** をクリックしてダイアログ ボックスが表示されたら、 **ローカル コンピューター**します。  **デプロイメントの To-do リスト** が表示されます。

2.  **必須のデプロイ タスク**, 、クリックして **ネットワークを構成する**です。

    ![ネットワークの構成][config_hpc2]

3. ネットワークの構成ウィザードで選択 **[企業ネットワークのみのすべてのノード** (トポロジ 5)。

    ![トポロジ 5][config_hpc3]

    >[AZURE.NOTE]これは、ヘッド ノードでは、Active Directory とインターネットに接続するための 1 つのネットワーク アダプターのみが必要であるために、デモンストレーションを目的として、最もシンプルな構成です。 このチュートリアルでは、追加のネットワークを必要とするクラスター シナリオは扱いません。

4. をクリックして **次** ウィザードの残りのページの既定の設定のままにします。 次に、 **レビュー** ] タブ、[ **構成** ネットワーク構成を完了します。

5.  **デプロイメントの To-do リスト**, 、クリックして **インストール資格情報を提供**します。

6.  **インストール資格情報** ] ダイアログ ボックスで、HPC Pack をインストールするために使用するドメイン アカウントの資格情報を入力します。 クリックして **OK**します。

    ![インストール資格情報][config_hpc6]

    >[AZURE.NOTE]のみ HPC Pack サービスでは、インストール資格情報を使用して、ドメインに参加している計算ノードを展開します。 このチュートリアルで追加する Azure ノードはドメインに参加していません。

7.  **デプロイメントの To-do リスト**, 、クリックして **新しいノードの名前の付け方**します。

8.  **ノードの名前付け系列の指定** ] ダイアログ ボックスで、既定の系列とクリックを名前付け **OK**します。

    ![ノードの名前付け][config_hpc8]

    >[AZURE.NOTE]この命名系では、ドメインに参加している計算ノードの名前のみを生成します。 Azure ノードは自動的に命名されます。

9.  **デプロイメントの To-do リスト**, 、クリックして **ノード テンプレートの作成**します。 ノード テンプレートを使用して、Azure ノードをクラスターに追加します。

10. ノード テンプレートの作成ウィザードで、次の操作を行います。

    a.  **ノード テンプレートの種類の選択** ] ページで [ **Azure ノード テンプレート**, 、] をクリックし、 **次**します。

    ![ノード テンプレート][config_hpc10]

    b. クリックして **次** を既定のテンプレート名を受け入れるようにします。

    c.  **サブスクリプション情報の入力** ] ページで、Azure サブスクリプション ID を入力してください (azure <a href="[https://account.windowsazure.com/Subscriptions">アカウント情報</a>)。 その後、[ **管理証明書**, 、] をクリックして **参照** 選択 **既定の HPC Azure 管理します。**クリックして **次**します。

    ![ノード テンプレート][config_hpc12]

    d.  **サービス情報の入力** ページで、クラウド サービスと、前の手順で作成したストレージ アカウントを選択します。 クリックして **次**します。

    ![ノード テンプレート][config_hpc13]

    e. をクリックして **次** ウィザードの残りのページの既定の設定のままにします。 次に、 **レビュー** ] タブをクリックして **作成** ノード テンプレートを作成します。

    >[AZURE.NOTE]既定では、Azure ノード テンプレートには、開始 (プロビジョニング) と、ノードを手動で停止するための設定が含まれます。 Azure ノードを自動的に開始および停止するスケジュールを構成することもできます。

## クラスターに Azure ノードを追加する

ここでは、ノード テンプレートを使用して、Azure ノードをクラスターに追加します。 ノードをクラスターに追加すると、ノードの構成情報が保存されるため、ノードをいつでもクラウド サービスのロール インスタンスとして開始 (プロビジョニング) することができます。 Azure ノードについてサブスクリプションに課金されるのは、クラウド サービスでロール インスタンスを実行した後だけです。

このチュートリアルでは、2 つの小規模ノードを追加します。

1. HPC クラスター マネージャーで **ノード管理**, で、 **アクション** ] ウィンドウで、をクリックして **ノードの追加**します。

    ![ノードの追加][add_node1]

2. ノードの追加ウィザードで、 **展開方法の選択** ] ページで [ **[Azure ノードの**, 、順にクリック **次**します。

    ![Azure ノードの追加][add_node1_1]

3.  **新しいノードの指定** ページで、以前に作成した Azure ノード テンプレートを選択 (既定で呼ばれる **既定の AzureNode テンプレート**)。 指定し、 **2** ノードのサイズ **小さな**, 、] をクリックし、 **次**します。

    ![ノードの指定][add_node2]

    使用可能な仮想マシンのサイズについては、次を参照してください。 [仮想マシンと Azure のクラウド サービスのサイズ](https://msdn.microsoft.com/library/azure/dn197896.aspx)します。

4.  **ノードの追加ウィザードを完了する** ] ページで [ **完了**します。

     という名前の 2 つの Azure ノード **azurecn-0001** と **azurecn-0002**, 、HPC クラスター マネージャーに表示されます。 どちらも、 **未展開** 状態です。

    ![追加されたノード][add_node3]

## Azure ノード
Azure のクラスター リソースを使用する場合、HPC クラスター マネージャーを使用して、Azure ノードを開始 (プロビジョニング) してオンラインにします。

1.  HPC クラスター マネージャーで **ノード管理**, 、一方または両方のノードをクリックし、[、 **アクション** ] ウィンドウで、をクリックして **開始**します。

    ![ノードの開始][add_node4]

2.  **Azure ノードの開始** ダイアログ ボックスで、をクリックして **開始**します。

    ![ノードの開始][add_node5]

    ノードに移行、 **プロビジョニング** 状態です。 プロビジョニング ログを表示して、プロビジョニング進捗状況を確認できます。

    ![ノードのプロビジョニング][add_node6]

3. プロビジョニングが完了し、Azure ノード、数分後に、 **オフライン** 状態です。 この状態では、ロール インスタンスは実行中ですが、クラスター ジョブは受け入れられません。

4. ロール インスタンスが実行中であるのかを確認する、 [ポータル](https://manage.windowsazure.com), 、クリックして **クラウド サービス**, をクラウド サービスの名前をクリックして、クリックして **インスタンス**します。

    ![実行中のインスタンス][view_instances1]

    2 つの worker ロール インスタンスがサービスで実行中であることがわかります。 HPC Pack では 2 つのことも自動的に展開 **HpcProxy** インスタンス (サイズは m) をヘッド ノードと Azure 間の通信を処理します。

5. ダウン Azure ノードをオンラインでは、クラスター ジョブを実行、ノードを右クリックし、クリックして選択して **オンライン**します。

    ![オフラインのノード][add_node7]

    HPC クラスター マネージャーでは、ノードにあることを示します、 **オンライン** 状態です。

## クラスター全体にコマンドを実行する
HPC Pack を使用する **clusrun** コマンドを 1 つまたは複数のクラスター ノードでコマンドまたはアプリケーションを実行します。 簡単な例として使用して **clusrun** Azure ノードの IP 構成を取得します。

1. ヘッド ノードで、コマンド プロンプトを開きます。

2. 次のコマンドを入力します。

    `clusrun /nodes:azurecn* ipconfig`

3. 次のような出力が表示されます。

    ![clusrun][clusrun1]

## テスト ジョブを実行する

ハイブリッド クラスター上で実行するテスト ジョブを送信できます。 この例は、単純な「パラメーター スイープ」ジョブ (並列コンピューティングの一種) を使用して、自身に整数を追加するサブタスクを実行、 **set/a** コマンドです。 クラスター内のすべてのノードが、1 ～ 100 の整数に関するサブタスクを完了します。

1. HPC クラスター マネージャーで **ジョブ管理**, で、 **アクション** ] ウィンドウで、をクリックして **新しいパラメーター スイープ ジョブ**します。

    ![新しいジョブ][test_job1]

2.  **新しいパラメーター スイープ ジョブ** ダイアログ ボックスで、 **コマンドライン**, 、型 `set /a *+*` (表示される既定のコマンドラインを上書きして)。 残りの設定の既定値のままにし、をクリックし、 **送信** ジョブを送信します。

    ![パラメーター スイープ][param_sweep1]

3. ジョブが完了したら、ダブルクリック、 **マイ スイープ タスク** ジョブです。

4. をクリックして **タスクを表示する**, 、サブタスクの計算された出力を表示するサブタスクを順にクリックします。

    ![タスクの結果][view_job361]

5. サブタスクの計算を実行したノードを表示する] をクリックして **割り当て済みのノード**します。 (クラスターによっては異なるノード名が表示されます)。

    ![タスクの結果][view_job362]

## Azure ノードを停止する

クラスターを試した後は、HPC クラスター マネージャーを使用して Azure ノードを停止し、アカウントへの不必要な課金を避けることができます。 ノードを停止するとクラウド サービスが停止し、Azure ロール インスタンスが削除されます。

1. HPC クラスター マネージャーで、[ **ノード管理]** 両方の Azure ノードを選択します。 次に、 **アクション** ] ウィンドウで、をクリックして **停止**します。

    ![ノードの停止][stop_node1]

2.  **Azure ノードの停止** ダイアログ ボックスで、をクリックして **停止**します。

    ![ノードの停止][stop_node2]

3. ノードに移行、 **停止** 状態です。 数分後に、HPC クラスター マネージャーで表示、ノードがある **未展開**します。

    ![未配置のノード][stop_node4]

4. ロール インスタンスが実行されていないこと、Azure でのことを確認する、 [ポータル](https://manage.windowsazure.com), 、クリックして **クラウド サービス**, をクラウド サービスの名前をクリックして、クリックして **インスタンス**します。 運用環境にはインスタンスはデプロイされません。

    ![インスタンスなし][view_instances2]

    このチュートリアルはこれで終わりです。

## 関連リソース

* [HPC Pack 2012 R2 と HPC Pack 2012](http://go.microsoft.com/fwlink/p/?LinkID=263697)
* [Microsoft HPC Pack を使用した Azure へのバースト](http://go.microsoft.com/fwlink/p/?LinkID=200493)
* [Azure での Microsoft HPC Pack を使用した HPC クラスター オプション](../virtual-machines/virtual-machines-hpcpack-cluster-options.md)
* [Azure における大規模なコンピューティング: Batch とハイ パフォーマンス コンピューティング (HPC) に関するテクニカル リソース](big-compute-resources.md)


[Overview]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/hybrid_cluster_overview.png
[install_hpc1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc1.png
[install_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc2.png
[install_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc3.png
[install_hpc4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc4.png
[install_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc6.png
[install_hpc7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc7.png
[install_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/install_hpc10.png
[upload_cert1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/upload_cert1.png
[createstorage1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createstorage1.png
[createservice1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/createservice1.png
[config_hpc2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc2.png
[config_hpc3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc3.png
[config_hpc6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc6.png
[config_hpc8]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc8.png
[config_hpc10]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc10.png
[config_hpc12]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc12.png
[config_hpc13]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/config_hpc13.png
[add_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1.png
[add_node1_1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node1_1.png
[add_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node2.png
[add_node3]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node3.png
[add_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node4.png
[add_node5]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node5.png
[add_node6]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node6.png
[add_node7]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/add_node7.png
[view_instances1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances1.png
[clusrun1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/clusrun1.png
[test_job1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/test_job1.png
[param_sweep1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/param_sweep1.png
[view_job361]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job361.png
[view_job362]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_job362.png
[stop_node1]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node1.png
[stop_node2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node2.png
[stop_node4]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/stop_node4.png
[view_instances2]: ./media/cloud-services-setup-hybrid-hpcpack-cluster/view_instances2.png

