<properties
    pageTitle="オンプレミスの VMM サイトと Azure 間の保護の設定"
    description="Azure Site Recovery は、オンプレミスの VMM クラウドに配置された Hyper-V 仮想マシンの Azure へのレプリケーション、フェイルオーバー、および回復を調整します。"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor="tysonn"/>

<tags
    ms.service="site-recovery"
    ms.workload="backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="11/18/2015"
    ms.author="raynew"/>

#  オンプレミスの VMM サイトと Azure 間の保護の設定

## 概要

Azure Site Recovery は、さまざまなデプロイ シナリオでの仮想マシンのレプリケーション、フェールオーバー、復旧を調整してビジネス継続性と障害復旧 (BCDR) 戦略に貢献します。 展開の完全な一覧のシナリオを参照して  [Azure Site Recovery の概要](site-recovery-overview.md)します。

このシナリオのガイドでは、Site Recovery をデプロイして、VMM プライベート クラウドにデプロイされている Hyper-V ホスト サーバー上の仮想マシンで実行されているワークロードの保護を調整および自動化する方法について説明します。 このシナリオでは、仮想マシンはプライマリ VMM サイトから Azure に Hyper-V Replica を使用してレプリケートされます。

ガイドには、シナリオの前提条件が含まれています。また、Site Recovery コンテナーを設定する方法、ソース VMM サーバーに Azure Site Recovery プロバイダーをインストールする方法、このコンテナーにサーバーを登録する方法、Azure ストレージ アカウントを追加する方法、Hyper-V ホスト サーバーに Azure Recovery Services エージェントをインストールする方法、保護されるすべての仮想マシンに適用される VMM クラウドの保護設定を構成する方法、およびこれらの仮想マシンの保護を有効にする方法についても説明しています。 すべてが正しく動作していることを確認するために、最後にフェールオーバーをテストします。

設定するこのシナリオの投稿、質問に問題が生じたかどうか、 [Azure Recovery Services フォーラム](http://go.microsoft.com/fwlink/?LinkId=313628)します。

## 開始する前に

次の前提条件を満たしていることを確認してください。
### Azure の前提条件

- 必要があります、 [Microsoft Azure](http://azure.microsoft.com/) アカウントです。 ない場合は始めて、 [無料評価版](http://aka.ms/try-azure)します。 さらに知っておくことができます [Azure Site Recovery Manager の料金](http://go.microsoft.com/fwlink/?LinkId=378268)します。
- Azure にレプリケートしたデータを格納するために Azure ストレージ アカウントが必要になります。 アカウントでは geo レプリケーションを有効にする必要があります。 アカウントは Azure Site Recovery サービスと同じリージョンである必要があり、同じサブスクリプションに関連付けられている必要があります。 Azure ストレージのセットアップの詳細については、次を参照してください。 [Microsoft Azure Storage の概要](http://go.microsoft.com/fwlink/?LinkId=398704)します。
- 保護する仮想マシンが Azure 要件に準拠していることを確認する必要があります。 参照してください [仮想マシンのサポート](https://msdn.microsoft.com/library/azure/dn469078.aspx#BKMK_E2A) 詳細です。

### VMM の前提条件
- VMM サーバーの System Center 2012 R2 で実行されている必要があります。
- 保護する仮想マシンを含むすべての VMM サーバーが Azure Site Recovery プロバイダーを実行している必要があります。 このプロバイダーは Azure Site Recovery のデプロイ時にインストールされます。
- 保護する VMM サーバーにクラウドが少なくとも 1 つ必要です。 このクラウドには以下のものが含まれている必要があります。
    - 1 つ以上の VMM ホスト グループ。
    - 各ホスト グループ内に 1 つ以上の Hyper-V ホスト サーバーまたはクラスター。
    - ソース Hyper-V サーバー上に配置された 1 つ以上の仮想マシン。 仮想マシンは世代 1 にする必要があります。
- VMM クラウドの設定について理解を深めます。
    - プライベート VMM クラウドの詳細については [と System Center 2012 R2 VMM のプライベート クラウドの新](http://go.microsoft.com/fwlink/?LinkId=324952) し、[ [VMM 2012 とクラウド](http://go.microsoft.com/fwlink/?LinkId=324956)します。
    - について学習 [ファブリックにクラウド、VMM の構成](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
    - クラウド ファブリック要素の配置後は、プライベート クラウドでの作成について学習  [VMM でプライベート クラウドの作成](http://go.microsoft.com/fwlink/?LinkId=324953) と [チュートリアル: System Center 2012 SP1 VMM でプライベート クラウドを作成する](http://go.microsoft.com/fwlink/?LinkId=324954)です。

### Hyper-V の前提条件

- ホスト Hyper-V サーバーは、少なくとも Hyper-V ロールを持つ Windows Server 2012 R2 を実行しており、最新の更新プログラムがインストールされている必要があります。
- クラスターで Hyper-V を実行している場合に、静的 IP アドレス ベースのクラスターが存在すると、クラスター ブローカーが自動的に作成されません。 クラスター ブローカーを手動で構成する必要があります。 手順を参照してください [HYPER-V レプリカ ブローカーを構成する](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx)です。
- 保護を管理するすべての Hyper-V ホスト サーバーまたはクラスターが VMM クラウドに属している必要があります。

次の図は、Azure Site Recovery でオーケストレーションやレプリケーションに使用される、さまざまな通信チャネルと通信ポートを示しています。

![E2A トポロジ](./media/site-recovery-vmm-to-azure/E2ATopology.png)

### ネットワーク マッピングの前提条件
仮想マシンを Azure ネットワーク マッピングで保護する場合、ソース VMM サーバー上の VM ネットワークとターゲットの Azure ネットワークをマップして以下を実現します。

- 同じネットワーク上でフェールオーバーするすべてのマシンは、どの復旧計画に含まれていても、相互に接続できる。
- ターゲットの Azure ネットワークでネットワーク ゲートウェイをセットアップすると、仮想マシンを他のオンプレミスの仮想マシンに接続できる。
- ネットワーク マッピングを構成しない場合、Azure へのフェールオーバー後、同じ復旧計画でフェールオーバーする仮想マシンのみ相互に接続できる。

ネットワーク マッピングをデプロイする場合は、以下のことが必要になります。

- ソース VMM サーバー上の保護する仮想マシンが VM ネットワークに接続している。 そのネットワークは、クラウドに関連付けられた論理ネットワークにリンクされている必要があります。
- レプリケートされた仮想マシンがフェールオーバー後に接続できる Azure ネットワーク。 フェールオーバー時にこのネットワークを選択します。 ネットワークは Azure Site Recovery サブスクリプションと同じリージョンである必要があります。
- ネットワーク マッピングについての理解を深める。
    - [VMM での論理ネットワークの構成](http://go.microsoft.com/fwlink/?LinkId=386307)
    - [VMM での VM ネットワークとゲートウェイの構成](http://go.microsoft.com/fwlink/?LinkId=386308)
    - [Azure における仮想ネットワークの構成および監視](http://go.microsoft.com/fwlink/?LinkId=402555)


## ステップ 1: Site Recovery コンテナーを作成する

1. サインイン、 [管理ポータル](https://portal.azure.com) に登録する VMM サーバーからです。


2. [
3. *データ サービス*, 、展開 *Recovery Services*, 、] をクリック *Site Recovery コンテナー*します。
*
3. クリックして *新規作成* ] をクリックし、 *簡易作成*します。


4.  *名*, 、コンテナーを識別するフレンドリ名を入力します。

5.  *地域*, 、資格情報コンテナーのリージョンを選択します。 利用可能なリージョンには、東アジア、西ヨーロッパ、米国西部、米国東部、北ヨーロッパ、東南アジアが含まれています。
6. クリックして *コンテナーの作成*します。

    ![新しいコンテナー](./media/site-recovery-vmm-to-azure/ASRE2AVMM_HvVault.png)

<P>ステータス バーを確認して、コンテナーが正常に作成されたことを確かめます。 資格情報コンテナーが表示されます *Active* メイン [復旧サービス] ページにします。</P>


## ステップ 2: コンテナー登録キーを生成する

コンテナーの登録キーを生成します。 Azure Site Recovery プロバイダーをダウンロードして VMM サーバーにインストールした後で、このキーを使用して、VMM サーバーをコンテナーに登録します。

1. *[Recovery Services]* ページで、資格情報コンテナーをクリックして、[クイック スタート] ページを開きます。 [クイック スタート] は、アイコンを使っていつでも開くことができます。

    ![[クイック スタート] アイコン](./media/site-recovery-vmm-to-azure/ASRE2AVMM_QuickStartIcon.png)

2. ドロップダウン リストで選択 **内部設置型 VMM サイトと Microsoft Azure 間**します。
3.  **VMM サーバーの準備**, 、クリックして **登録キーの生成** ファイルです。 キー ファイルは自動的に生成され、生成後 5 日間有効です。 VMM サーバーから Azure ポータルにアクセスしていない場合は、このファイルをサーバーにコピーする必要があります。

    ![登録キー](./media/site-recovery-vmm-to-azure/ASRE2AVMM_RegisterKey.png)

## ステップ 3: Azure Site Recovery プロバイダーをインストールする

4.  *クイック スタート* ] ページの [ **VMM サーバーの準備**, をクリックして *ダウンロード Microsoft Azure Site Recovery プロバイダーの VMM サーバーにインストールするため* プロバイダー インストール ファイルの最新バージョンを取得します。

2. ソース VMM サーバーでこのファイルを実行します。 プロバイダーを初めてインストールするときに、VMM がクラスターにデプロイされている場合は、プロバイダーをアクティブなノードにインストールします。インストールが終了したら、VMM サーバーをコンテナーに登録します。 次に、プロバイダーを他のノードにインストールします。 プロバイダーをアップグレードする場合は、すべてのノードでアップグレードする必要があります。これは、すべてのノードで同じバージョンのプロバイダーを実行する必要があるためです。


3. インストーラーはいくつか **前提条件の確認** し、プロバイダーのセットアップを開始する VMM サービスを停止する許可を要求します。 セットアップが完了すると、VMM サービスは自動的に再起動されます。 VMM クラスターにインストールしている場合は、クラスター ロールを停止するよう求められます。

4.  **Microsoft Update** 更新プログラムを設定できます。 この設定を行うことで、設定した Microsoft Update のポリシーに従って、プロバイダーの有効な更新がインストールされます。

    ![Microsoft 更新プログラム](./media/site-recovery-vmm-to-azure/VMMASRInstallMUScreen.png)


1.  インストール場所に設定されている **<SystemDrive>\Program Files\Microsoft System Center 2012 r2 \virtual Machine manager \bin**します。 [インストール] ボタンをクリックすると、プロバイダーのインストールが開始されます。
    ![InstallLocation](./media/site-recovery-vmm-to-azure/VMMASRInstallLocationScreen.png)



1. プロバイダーがインストールされたら、[登録] ボタンをクリックしてサーバーをコンテナーに登録します。
    ![InstallComplete](./media/site-recovery-vmm-to-azure/VMMASRInstallComplete.png)

5.  **インターネット接続** VMM サーバーで実行されるプロバイダーがインターネットに接続する方法を指定します。 選択 *既定のシステム プロキシ設定を使用して* 、サーバーで構成されている既定のインターネット接続設定を使用します。

    ![インターネット設定](./media/site-recovery-vmm-to-azure/VMMASRRegisterProxyDetailsScreen.png)
    - カスタム プロキシを使用する場合は、プロバイダーをインストールする前に設定する必要があります。 カスタム プロキシ設定を構成すると、プロキシの接続を確認するためのテストが実施されます。
    - カスタム プロキシを使用する場合、または既定のプロキシで認証が必要な場合、プロキシのアドレスやポートなどの詳細を入力する必要があります。
    - VMM サーバーと Hyper-V ホストから次の URL にアクセスできる必要があります。
        - *.hypervrecoverymanager.windowsazure.com
        - *.accesscontrol.windows.net
        - *.backup.windowsazure.com
        - *.blob.core.windows.net
        - *.store.core.windows.net
    - 記載されている IP アドレスを許可する [Azure Datacenter の IP 範囲](http://go.microsoft.com/fwlink/?LinkId=511094) と HTTPS (443) プロトコルです。 使用を計画している Azure リージョンの IP の範囲と米国西部の IP の範囲をホワイトリストに登録する必要があります。

    - カスタム プロキシを使用する場合、指定されたプロキシの資格情報を使用して VMM RunAs アカウント (DRAProxyAccount) が自動的に作成されます。 このアカウントが正しく認証されるようにプロキシ サーバーを構成します。 VMM RunAs アカウントの設定は VMM コンソールで変更できます。 変更するには、[設定] ワークスペースを開いて [セキュリティ] を展開し、[実行アカウント] をクリックします。その後、DRAProxyAccount のパスワードを変更します。 新しい設定を有効にするには、VMM サービスを再起動する必要があります。

6.  **登録キー**, 、Azure Site Recovery からダウンロードし、VMM サーバーにコピーしたことを選択します。
7.  **資格情報コンテナー名**, 、サーバーを登録するコンテナーの名前を確認します。 クリックして *次*します。


    ![Server registration](./media/site-recovery-vmm-to-azure/VMMASRRegisterVaultCreds.png)

9. データの暗号化用に自動的に生成された SSL 証明書を保存する場所を指定できます。 この証明書は、Azure Site Recovery ポータル内で Azure によって保護されているクラウドのデータ暗号化が有効な場合に使用されます。 この証明書を安全な場所に保管します。 Azure へのフェールオーバーを実行する際に、暗号化されたデータの暗号化を解除するために、この証明書を選択します。

    ![サーバー登録](./media/site-recovery-vmm-to-azure/VMMASRRegisterEncryptionScreen.png)

8.  **サーバー名**, 、資格情報コンテナーで VMM サーバーを識別する表示名を指定します。 クラスター構成で、VMM クラスターのロール名を指定します。

8.  **初期クラウド メタデータ** VMM サーバー上のすべてのクラウドのメタデータをコンテナーと同期するかどうか選択を同期します。 この操作は、各サーバーで 1 回のみ実行する必要があります。 すべてのクラウドを同期したくない場合は、この設定をオフのままにして、VMM コンソールのクラウドのプロパティで各クラウドを個別に同期できます。
    ![サーバーの登録](./media/site-recovery-vmm-to-azure/VMMASRRegisterFriendlyName.png)


8. クリックして *次* プロセスを完了します。 登録後に、VMM サーバーからのメタデータが、Azure Site Recovery によって取得されます。 サーバーが表示されます、  *VMM サーバー* ] タブで、 **サーバー** ] ページで、資格情報コンテナー。

>[AZURE.NOTE] 次のコマンドラインを使用して、Azure Site Recovery プロバイダーをインストールすることもできます。 このメソッドを使用すると、Windows Server 2012 R2 の Server CORE にプロバイダーをインストールできます。

1. プロバイダーのインストール ファイルと登録キーを C:\ASR などのフォルダーにダウンロードします。
1. System Center Virtual Machine Manager サービスを停止します。
1. プロバイダーのインストーラーを実行して抽出、次のコマンド プロンプトでコマンド **管理者** 特権

        C:\Windows\System32> CD C:\ASR
        C:\ASR> AzureSiteRecoveryProvider.exe /x:. /q
1. 次のコマンドを実行して、プロバイダーをインストールします。

        C:\ASR> setupdr.exe /i
1. 次のコマンドを実行して、プロバイダーを登録します。

        CD C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin
        C:\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin\> DRConfigurator.exe /r  /Friendlyname <friendly name of the server> /Credentials <path of the credentials file> /EncryptionEnabled <full file name to save the encryption certificate>       

  
#### コマンド ラインのインストール パラメーター一覧

 - **/資格情報** : 登録キー ファイルが配置されている場所を指定する必須パラメーター  
 - **/Friendlyname** : Azure Site Recovery ポータルに表示されている HYPER-V ホスト サーバーの名前を表す必須パラメーターです。
 - **/EncryptionEnabled** : 休息中で、Azure で仮想マシンを暗号化する必要がある場合は、Azure のシナリオのための VMM でのみ使用する必要があります。 省略可能なパラメーターです。 指定したファイル名があることを確認してください、 **.pfx** 拡張機能です。
 - **/proxyAddress** : プロキシ サーバーのアドレスを指定する省略可能なパラメーターです。
 - **/proxyport** : プロキシ サーバーのポートを指定する省略可能なパラメーターです。
 - **/proxyUsername** : (プロキシ認証を必要とする場合は、プロキシ ユーザー名を指定する省略可能なパラメーターです。
 - **/proxyPassword** : 省略可能なパラメーター (プロキシ認証を必要とする場合は、プロキシ サーバーを使用した認証のパスワードを指定します。  


## ステップ 4: Azure のストレージ アカウントを作成する

ない場合、Azure ストレージ アカウントをクリックして **Azure ストレージ アカウントの追加**します。 アカウントでは geo レプリケーションを有効にする必要があります。 アカウントは Azure Site Recovery サービスと同じリージョンである必要があり、同じサブスクリプションに関連付けられている必要があります。


![ストレージ アカウント](./media/site-recovery-vmm-to-azure/ASRE2AVMM_StorageAgent.png)

## ステップ 5: Azure Recovery Services エージェントをインストールする

保護する VMM クラウドに配置されている Hyper-V ホスト サーバーごとに、Azure Recovery Services Agent をインストールします。

1. [クイック スタート] ページで、<b>[Azure Site Recovery Services エージェントをダウンロードしてホストにインストールする]</b> をクリックして、最新バージョンのエージェントのインストール ファイルを取得します。

    ![Recovery Services エージェントのインストール](./media/site-recovery-vmm-to-azure/ASRE2AVMM_InstallHyperVAgent.png)

2. 保護する VMM クラウドに配置されている Hyper-V ホスト サーバーごとにインストール ファイルを実行します。
3.  **前提条件の確認** ページをクリックして <b>次</b>します。 不足している前提条件があると自動的にインストールされます。

    ![Recovery Services エージェントの前提条件](./media/site-recovery-vmm-to-azure/ASRE2AVMM_AgentPrereqs.png)

4.  **インストール設定** ] ページで、エージェントをインストールし、バックアップ メタデータがインストールされるキャッシュの場所を選択する場所を指定します。 その後、 <b>[インストール]</b> をクリックします。

## ステップ 6: クラウドの保護設定を構成する

VMM サーバーが登録されると、クラウドの保護設定を構成することができます。 オプションを有効になっている **、資格情報コンテナーとクラウド データの同期** プロバイダーをインストールするときに、VMM サーバー上のすべてのクラウドが表示されます、 <b>保護された項目</b> ] タブで、資格情報コンテナー。

![発行済みのクラウド](./media/site-recovery-vmm-to-azure/ASRE2AVMM_CloudsList.png)


1. クイック スタート] ページで、次のようにクリックします。 **VMM クラウドの保護を設定**します。
2.  **保護された項目** ] タブで、構成に移動するクラウドをクリックして、 **構成** ] タブをクリックします。
3. <b>[ターゲット]</b> で、<b>[Microsoft Azure]</b> を選択します。
4. <b>[ストレージ アカウント]</b> で、仮想マシンのレプリケートに使用する Azure ストレージ アカウントを選択します。
5. <b>[格納データの暗号化]</b> を <b>[オフ]</b> に設定します。 この設定は、オンプレミスのサイトと Azure 間のレプリケートでデータを暗号化する必要があることを指定します。
6. <b>[コピーの頻度]</b> では、既定の設定をそのまま使用します。 この値は、ソースとターゲットの場所の間でデータが同期される頻度を指定します。
7. <b>[保持する復旧ポイント]</b> では、既定の設定をそのまま使用します。 既定値である 0 を使用する場合は、プライマリ仮想マシンに対応する最新の復旧ポイントのみが、レプリカのホスト サーバーに格納されます。
8. <b>[アプリケーションの整合性スナップショットの頻度]</b> では、既定の設定をそのまま使用します。 この値は、スナップショットを作成する頻度を指定します。 スナップショットは、ボリューム シャドウ コピー サービス (VSS) を使用して、スナップショットを作成するときにアプリケーションを一貫性のある状態に保ちます。  値を設定する場合は、構成する追加の復旧ポイント数よりも少ない値にしてください。
9. <b>[レプリケーションの開始時刻]</b> で、Azure へのデータの初期レプリケーションを開始する時刻を指定します。 Hyper-V ホスト サーバーのタイムゾーンが使用されます。 初期レプリケーションはピーク時以外にスケジュールすることをお勧めします。

    ![クラウドのレプリケーション設定](./media/site-recovery-vmm-to-azure/ASRE2AVMM_CloudSettings.png)

この設定を保存すると、ジョブが作成され、これを <b>[ジョブ]</b> タブで監視できます。 VMM ソース クラウド内のすべての Hyper-V ホスト サーバーは、レプリケーション用に構成されます。

保存後は、クラウド設定は <b>[構成]</b> タブで変更できます。 ターゲットの場所またはターゲットのストレージ アカウントを変更するには、クラウド構成を削除してから、クラウドを再構成する必要があります。 ストレージ アカウントを変更する場合は、ストレージ アカウントの修正後に保護を有効にした仮想マシンにのみ、その変更が適用されることに注意してください。 既存の仮想マシンは新しいストレージ アカウントに移行されません。</p>

## ステップ 7: ネットワーク マッピングを構成する
ネットワーク マッピングを開始する前に、ソース VMM サーバー上の仮想マシンが VM ネットワークに接続されていることを確認してください。 さらに、1 つまたは複数の Azure Virtual Network を作成します。 複数の VM ネットワークを 1 つの Azure ネットワークにマップできることに注意してください。

1. クイック スタート] ページで、次のようにクリックします。 **ネットワーク マップ**します。
2.  **ネットワーク** ] タブの [ **ソースの場所**, 、ソース VMM サーバーを選択します。  **ターゲットの場所** Azure を選択します。
3.  **ソース** ネットワークが VMM サーバーに関連付けられている VM ネットワークの一覧が表示されます。  **ターゲット** ネットワーク サブスクリプションに関連付けられている Azure ネットワークが表示されます。
4. ソース VM ネットワークを選択し、クリックして **マップ**します。
5.  **ターゲット ネットワークの選択** ] ページで、ターゲットを使用する Azure ネットワークを選択します。
6. チェック マークをクリックして、マッピング処理を完了します。

    ![クラウドのレプリケーション設定](./media/site-recovery-vmm-to-azure/ASRE2AVMM_MapNetworks.png)

設定を保存すると、マッピングの進行状況を追跡するジョブが起動します。進行状況は [ジョブ] タブで監視することができます。 ソースの VM ネットワークに対応する既存のレプリカの全仮想マシンが、ターゲットの Azure ネットワークに接続します。 ソースの VM ネットワークに接続する新しい仮想マシンは、レプリケーション後、マップされた Azure ネットワークに接続します。 既存のマッピングを新しいネットワークで変更すると、レプリカの仮想マシンは新しい設定で接続されます。

ターゲット ネットワークに複数のサブネットがあり、そのサブネットのいずれかが、ソースの仮想マシンが配置されているサブネットと同じ名前である場合、フェールオーバー後、レプリカの仮想マシンはそのターゲット サブネットに接続することに注意してください。 ターゲットのサブネットで名前が一致するものがなければ、仮想マシンはネットワークの最初のサブネットに接続されます。

## ステップ 8: 仮想マシンの保護を有効化する

サーバー、クラウド、およびネットワークを正しく構成した後で、クラウド内の仮想マシンの保護を有効にすることができます。 以下の点に注意してください。

- 仮想マシンは Azure 要件を満たしている必要があります。 計画ガイドの「<a href="http://go.microsoft.com/fwlink/?LinkId=402602">前提条件とサポート</a>」で確認してください。
- オペレーティング システムとオペレーティング システム ディスクの保護を有効にするには、仮想マシンにプロパティを設定する必要があります。 仮想マシン テンプレートを使用して VMM 内で仮想マシンを作成する際に、プロパティを設定できます。 既存の仮想マシンに対してこれらのプロパティを設定することも、 **全般的な** と **ハードウェア構成** 、仮想マシンのプロパティ タブ。 VMM でこれらのプロパティを設定していない場合は、Azure Site Recovery ポータルで構成できます。

![Create virtual machine](./media/site-recovery-vmm-to-azure/ASRE2AVMM_EnableNew.png)

![仮想マシンのプロパティの変更](./media/site-recovery-vmm-to-azure/ASRE2AVMM_EnableExisting.png)


1. 保護を有効にするために、仮想マシンが配置されているクラウドの <b>[Virtual Machines]</b> タブで、<b>[保護を有効にする]</b> をクリックしてから <b>[仮想マシンを追加する]</b> を選択します。
2. クラウド内の仮想マシンのリストから、保護する仮想マシンを選択します。

    ![仮想マシンの保護の有効化](./media/site-recovery-vmm-to-azure/ASRE2AVMM_SelectVM.png)

    保護の有効化アクションの進行状況を追跡、 **ジョブ** ] タブで、初期レプリケーションなどです。 保護の最終処理のジョブが実行されると、仮想マシンは、フェールオーバーを実行できる状態になります。 保護が有効され仮想マシンが複製されると、Azure でそれらの状態を表示できます。


    ![Virtual machine protection job](./media/site-recovery-vmm-to-azure/ASRE2AVMM_VMJobs.png)

3. 仮想マシンのプロパティを確認し、必要に応じて変更します。

    ![仮想マシンの確認](./media/site-recovery-vmm-to-azure/VMProperties.png)

4. 仮想マシンのプロパティの [構成] タブでは、次のネットワークのプロパティを変更することができます。


    1.  Number of network adapters of target virtual machine - The number of network adapters is dictated by the size you specify for the target virtual machine. Check [virtual machine size specs](../virtual-machines/virtual-machines-size-specs.md#size-tables) for the number of nics supported by the virtual machine size. 

        When you modify the size for a virtual machine and save the settings, the number of network adapter will change when you open **Configure** page the next time.The number of network adapters of target virtual machines is minimum of the number of network adapters on source virtual machine and maximum number of network adapters supported by the size of the virtual machine chosen. It is explained below:


        - If the number of network adapters on the source machine is less than or equal to the number of adapters allowed for the target machine size, then the target will have the same number of adapters as the source.
        - If the number of adapters for the source virtual machine exceeds the number allowed for the target size then the target size maximum will be used.
        - For example if a source machine has two network adapters and the target machine size supports four, the target machine will have two adapters. If the source machine has two adapters but the supported target size only supports one then the target machine will have only one adapter.     


    1. Network of the target virtual machine - The network to which the virtual machine connects to is determined by network mapping of the network of source virtual machine. In case source virtual machine has more than one network adapters and source networks are mapped to different networks on target, then the user would have to choose between one of the target networks.

    1. Subnet of each of the network adapters - For each network adapter the user can choose the subnet to which the failed over virtual machine would connect to.

    1. Target IP - If the network adapter of source virtual machine is configured to use static IP then the user can provide the ip for the target virtual machine. User can use this capability to retain the ip of the source virtual machine after a failover. If no IP is provided any available IP would be given to network adapter at the time of failover. In case the target IP provided by user is already used by some other virtual machine that is already running in Azure then the failover would fail.  

        ![Modify network properties](./media/site-recovery-vmm-to-azure/MultiNic.png)

>[AZURE.NOTE] 静的 ip を使用する Linux 仮想マシンがサポートされていません。

## デプロイのテスト
デプロイをテストするために、1 台の仮想マシンに対するテスト フェールオーバーを実行することや、複数の仮想マシンで構成される復旧計画を作成して、その計画のテスト フェールオーバーを実行することができます。  テスト フェールオーバーは、孤立したネットワークでフェールオーバーと復旧のシミュレーションを実行します。 以下の点に注意してください。

- フェールオーバー後に Azure でリモート デスクトップを使用して仮想マシンに接続する場合は、テストのフェールオーバーを実行する前に、仮想マシンのリモート デスクトップ接続を有効にします。
- フェールオーバー後、パブリック IP アドレスを使用して、Azure の仮想マシンにリモート デスクトップで接続します。 この接続では、パブリック アドレスを使用する仮想マシンの接続を妨げるドメイン ポリシーを使用していないことを確認してください。

### 復旧計画の作成

1.  **復旧計画** ] タブで、新しい計画を追加します。 名前を指定 **VMM** で **ソースの種類**, 、およびソース VMM サーバーに **ソース**, 、ターゲットは Azure になります。

    ![復旧計画の作成](./media/site-recovery-vmm-to-azure/ASRE2AVMM_RP1.png)

2.  **仮想マシンの選択** ] ページで、復旧計画に追加する仮想マシンを選択します。 これらの仮想マシンは、復旧計画の既定のグループ "グループ 1" に追加されます。 最大 100 台の仮想マシンを 1 つの復旧計画でテストしています。

    - 仮想マシンを計画に追加する前に、仮想マシンのプロパティを検証する場合は、仮想マシンが配置されているクラウドの [プロパティ] ページで仮想マシンをクリックします。 仮想マシンのプロパティは VMM コンソールでも構成できます。
    - 表示されるすべての仮想マシンは、保護が有効になっています。 一覧には、保護が有効で、初期レプリケーションが完了している仮想マシンと、保護が有効だが、初期レプリケーションが保留中の仮想マシンの両方が含まれています。 初期レプリケーションが完了したマシンのみ、復旧計画の一環としてフェールオーバーできます。 そのため、復旧計画のフェールオーバーを開始する前に、この計画に含まれている仮想マシンの初期レプリケーションの状態を確認してください。


    ![Create recovery plan](./media/site-recovery-vmm-to-azure/ASRE2AVMM_SelectVMRP.png)

復旧計画作成が完了した後に表示されます、 **復旧計画** ] タブをクリックします。 追加することも [Azure Automation Runbook](site-recovery-runbook-automation.md) フェールオーバー時の操作を自動化する復旧計画にします。

### テスト フェールオーバーの実行

Azure へのテスト フェールオーバーを実行する方法は 2 つあります。

- Azure ネットワークを使用しないテスト フェールオーバー — この種のテスト フェールオーバーでは、仮想マシンが Azure で正しく動作することを確認します。 フェールオーバー後、仮想マシンはどの Azure ネットワークにも接続しません。
- Azure ネットワークを使用するテスト フェールオーバー — この種のフェールオーバーでは、レプリケーション環境全体が正しく動作することと、フェールオーバーされた仮想マシンが指定したターゲットの Azure ネットワークに接続されることを確認します。 サブネットの処理については、テスト フェールオーバーでは、テストの仮想マシンのサブネットはレプリカの仮想マシンのサブネットに基づいて決定されます。 これは、レプリカの仮想マシンのサブネットが、ソースの仮想マシンのサブネットに基づいている通常のレプリケーションとは異なります。

Azure ターゲット ネットワークを指定せずに、保護が有効になっている仮想マシンの Azure へのテスト フェールオーバーを実行する場合は、特に準備は必要ありません。 ターゲット Azure ネットワークを指定してテスト フェールオーバーを実行するには、Azure 運用ネットワークから独立した新しい Azure ネットワークを作成する必要があります (Azure で新しいネットワークを作成する場合の既定の動作)。 方法を紹介 [テスト フェールオーバーを実行](site-recovery-failover.md#run-a-test-failover) 詳細です。 


また、レプリケートされた仮想マシンのインフラストラクチャを正常に動作するように設定する必要があります。 たとえば、ドメイン コントローラーと DNS を含む仮想マシンは、Azure Site Recovery を使用して Azure にレプリケート可能で、テスト フェールオーバーを使用して、テスト ネットワーク内に作成できます。 見て [active directory のフェールオーバーの考慮事項をテスト](site-recovery-active-directory.md#considerations-for-test-failover) 詳細についてです。 

テスト フェールオーバーを実行するには、次の手順に従います。

1.  **復旧計画** ] タブで、プランを選択し、クリックして **テスト フェールオーバー**します。
1.  **テスト フェールオーバーの確認** ] を選択] ページ **None** または特定の Azure ネットワーク。  [なし] を選択した場合、テスト フェールオーバーでは、仮想マシンが Azure に正しくレプリケートされたかどうかは確認されますが、レプリケーションのネットワーク構成は確認されないことに注意してください。

    ![ネットワークなし](./media/site-recovery-vmm-to-azure/ASRE2AVMM_TestFailoverNoNetwork.png)

1. クラウド向け、データの暗号化が有効になっているかどうかは **暗号化キー** 、クラウドのデータの暗号化を有効にするオプションをオンにしたときに、VMM サーバー上のプロバイダーのインストール中に発行された証明書を選択します。
1.  **ジョブ** ] タブのフェールオーバーの進行状況を追跡することができます。 仮想マシンのテスト レプリカも Azure ポータルで確認できます。 オンプレミスのネットワークから、仮想マシンへのアクセスをセットアップすると、仮想マシンへのリモート デスクトップ接続を開始できます。
1. フェールオーバーが、 **包括的なテスト** フェーズをクリックして **テストの完了** テスト フェールオーバーを完了します。 までドリルダウン、 **ジョブ** タブ フェールオーバーの進行状況と状態を追跡するために必要な操作を実行します。
1. フェールオーバー後に、Azure ポータルで仮想マシンのテスト レプリカを表示することができます。 オンプレミスのネットワークから、仮想マシンへのアクセスをセットアップすると、仮想マシンへのリモート デスクトップ接続を開始できます。 以下の点に注意してください。

    1. 仮想マシンが正常に起動することを確認します。
    1. フェールオーバー後に Azure でリモート デスクトップを使用して仮想マシンに接続する場合は、テストのフェールオーバーを実行する前に、仮想マシンのリモート デスクトップ接続を有効にします。 また、仮想マシンに RDP エンドポイントを追加する必要もあります。 利用できる、 [Azure Automation Runbook](site-recovery-runbook-automation.md) を行うには。
    1. フェールオーバー後、リモート デスクトップでパブリック IP アドレスを使用して Azure の仮想マシンに接続する場合、パブリック アドレスを使用して仮想マシンに接続することを妨げるドメイン ポリシーがないことを確認します。

1.  テストが完了したら、以下の手順を実行します。
    - クリックして **テスト フェールオーバーが完了**します。 テスト環境をクリーンアップして、自動的に電源をオフにし、テスト仮想マシンを削除します。
    - クリックして **ノート** を記録して、テスト フェールオーバーに関連する監察結果をすべてを保存します。

## <a id="runtest" name="runtest" href="#runtest"></a>アクティビティを監視する
<p>使用することができます、 *ジョブ* ] タブをクリックし、 *ダッシュ ボード* 表示し、メイン ジョブを監視する Azure Site Recovery コンテナーを含む構成、クラウドに対する保護を有効にして、バーチャル マシンの保護を無効にする、フェールオーバー (計画された、計画されていない、またはテスト) を実行して、計画されていないフェールオーバーのコミットが実行されます。</p>

<p> *ジョブ* ] タブのジョブの表示、ドリル ダウンするには、ジョブの詳細と、特定の条件に一致するジョブを取得するジョブ クエリの実行エラー エクスポート ジョブには、Excel、およびジョブの再開に失敗しました。</p>

<p> *ダッシュ ボード* 最新バージョンのプロバイダーとエージェントのインストール ファイルをダウンロード、資格情報コンテナーの構成情報の取得、保護コンテナーによって管理される、最近のジョブの確認、コンテナー証明書の管理仮想マシンの数を表示および仮想マシンの再同期します。</p>

<p>ジョブの操作とダッシュ ボードの詳細については、次を参照してください。、 <a href="http://go.microsoft.com/fwlink/?LinkId=398534">運用と監視ガイド</a>します。</p>

##<a id="next" name="next" href="#next"></a>次のステップ
<UL>
<LI>計画を完全な運用環境での Azure Site Recovery のデプロイは、次を参照してください。 <a href="http://go.microsoft.com/fwlink/?LinkId=321294">Azure Site Recovery 計画ガイド</a> と <a href="http://go.microsoft.com/fwlink/?LinkId=321295">Azure Site Recovery デプロイ ガイド</a>します。</LI>


<LI>質問については、次を参照してください。、 <a href="http://go.microsoft.com/fwlink/?LinkId=313628">Azure Recovery Services フォーラム</a>します。</LI>
</UL>


