<properties 
   pageTitle="VMware 仮想マシンと物理サーバーを  Azure から VMware にフェールバックする | Microsoft Azure" 
   description="この記事では、Azure Site Recovery を使用して Azure にレプリケートされている VMware 仮想マシンをフェールバックする方法について説明します。" 
   services="site-recovery" 
   documentationCenter="" 
   authors="ruturaj" 
   manager="mkjain" 
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="storage-backup-recovery" 
   ms.date="12/14/2015"
   ms.author="ruturajd@microsoft.com"/>


# Azure Site Recovery を使用して VMware 仮想マシンと物理サーバーを  Azure から VMware にフェールバックする

## 概要

このドキュメントでは、VMware 仮想マシンと Windows/Linux 物理サーバーを Azure からオンプレミスのサイトにフェールバックする方法について説明します。

設定するレプリケーションとフェールオーバー シナリオではこの次の指示 [今回](site-recovery-vmware-to-azure.md)します。 Site Recovery を使用して VMware 仮想マシンまたは物理サーバーが Azure にフェールバックされると、マシンが [Azure 仮想マシン] タブに表示されます。
>[AZURE.NOTE] VMware 仮想マシンと Windows/Linux 物理サーバーを、Azure からオンプレミスのプライマリ サイトの VMware 仮想マシンにのみフェールバックできます。 物理マシンをフェールバックしている場合、Azure へのフェールオーバーによって物理サーバーが Azure VM に変換され、VMware へのフェールバックによって物理サーバーが VMware VM に変換されます。 

この図は、フェールオーバーとフェールバックのシナリオを表します。 青色の線は、フェールオーバー時に使用される接続です。 赤色の線は、フェールバック時に使用される接続です。 矢印のある線は、インターネットを経由することを意味します。

![](./media/site-recovery-failback-azure-to-vmware/vconports.png)

## 手順1: vContinuum をオンプレミスにインストールする

vContinuum サーバーをオンプレミスにインストールし、構成サーバーをポイントする必要があります。

1.  [VContinuum をダウンロード](http://go.microsoft.com/fwlink/?linkid=526305)します。
2.  ダウンロードし、[更新のダウンロード後 [vContinuum 更新](http://go.microsoft.com/fwlink/?LinkID=533813) バージョンです。
3.  vContinuum をインストールするには、最新バージョンのセットアップを実行します。  **へようこそ]** ページをクリックして **次**します。
    ![](./media/site-recovery-failback-azure-to-vmware/image2.png)
4.  ウィザードの最初のページで CX サーバーの IP アドレスとポートを指定します。 **[HTTPS の使用]** を選択します。

    ![](./media/site-recovery-failback-azure-to-vmware/image3.png)

5.  構成サーバー IP アドレスを確認、 **ダッシュ ボード** 構成サーバーで、Azure VM のタブをクリックします。
    ![](./media/site-recovery-failback-azure-to-vmware/image4.png)

6.  Azure の構成サーバー VM の **[エンドポイント]** タブで、構成サーバーの HTTPS パブリック ポートを探します。

    ![](./media/site-recovery-failback-azure-to-vmware/image5.png)

7. **[CS パスフレーズ詳細]** ページで、構成サーバーを登録したときにメモしたパスフレーズを指定します。 覚えていない場合は、チェックイン **C:\\Program Files (x86) \\InMage Systems\\private\\connection.passphrase** 構成サーバーの VM にします。

    ![](./media/site-recovery-failback-azure-to-vmware/image6.png)

8.  **[アップグレード先の場所の選択]** ページで、vContinuum サーバーをインストールする場所を指定し、**[インストール]** をクリックします。

    ![](./media/site-recovery-failback-azure-to-vmware/image7.png)

9. インストールが完了したら、vContinuum を起動することができます。
    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)


## 手順 2: Azure にプロセス サーバーをインストールする

Azure の VM がデータをオンプレミスのマスター ターゲット サーバーに戻せるようにするために、プロセス サーバーを Azure 上にインストールする必要があります。

1.  Azure の **[構成サーバー]** ページで、新しいプロセス サーバーの追加を選択します。

    ![](./media/site-recovery-failback-azure-to-vmware/image9.png)

2.  プロセス サーバー名、名前、およびパスワードを指定して、管理者として仮想マシンに接続します。 プロセス サーバーの登録先となる構成サーバーを選択します。 これは仮想マシンの保護とフェールオーバーに使用しているサーバーと同じにする必要があります。
3.  プロセス サーバーをデプロイする Azure ネットワークを指定します。 構成サーバーと同じネットワークにする必要があります。 一意の IP アドレスが選択されたサブネットを指定し、デプロイメントを開始します。

    ![](./media/site-recovery-failback-azure-to-vmware/image10.png)

4.  プロセス サーバーをデプロイするジョブがトリガーされます。

    ![](./media/site-recovery-failback-azure-to-vmware/image11.png)

5.  プロセス サーバーは、Azure に配置された後は、サーバーにログインすることができます。
指定した資格情報を使用します。 オンプレミスのプロセス サーバーを登録したのと同じ方法で、プロセス サーバーを登録します。

    ![](./media/site-recovery-failback-azure-to-vmware/image12.png)

>[AZURE.NOTE] フェールバック中に登録されたサーバーは、Site Recovery の VM プロパティの下に表示されません。 それらのサーバーは、それらが登録されている構成サーバーの  **[サーバー]** タブの下にのみ表示されます。 プロセス サーバーがタブに表示されるまで約 10 分から 15 分かかることがあります。


## 手順 3: マスター ターゲット サーバーをオンプレミスにインストールする

ソースの仮想マシンのオペレーティング システムに応じて、オンプレミスに Linux または Windows のマスター ターゲット サーバーをインストールすることが必要になります。

### Windows のマスター ターゲット サーバーをデプロイする

Windows のマスター ターゲットは、vContinuum セットアップに既にバンドルされています。 インストールするときに
vContinuum、マスター サーバーが同じコンピューター上に展開されても、
構成サーバーに登録されます。

1.  デプロイメントを開始するには、Azure からの VM の復旧先となる、オンプレミスの ESX ホスト上に空のマシンを作成します。

2.  VM に 2 つ以上のディスクがアタッチされていることを確認します。1 つ目はオペレーティング システム用に使用され、2 つ目はリテンション期間ドライブとして使用されます。

3.  オペレーティング システムをインストールします。

4.  vContinuum をサーバーにインストールします。 これにより、マスター ターゲット サーバーのインストールも完了します。

### Linux のマスター ターゲット サーバーをデプロイする

1.  デプロイメントを開始するには、Azure からの VM の復旧先となる、オンプレミスの ESX ホスト上に空のマシンを作成します。

2.  VM に 2 つ以上のディスクがアタッチされていることを確認します。1 つ目はオペレーティング システム用に使用され、2 つ目はリテンション期間ドライブとして使用されます。

3.  Linux オペレーティング システムをインストールします。 Linux マスター ターゲット システムでは、LVM をルートまたはリテンション期間ストレージ スペースとして使用しないでください。 Linux マスター ターゲット サーバーは、既定で、LVM パーティション/ディスク検出を回避するように構成されます。
4.  作成できるパーティションを次の表に示します。

    ![](./media/site-recovery-failback-azure-to-vmware/image13.png)

5.  マスター ターゲットのインストールを開始する前に、次のインストール後の手順を実行します。


#### OS インストール後の手順

各 Linux 仮想マシンで SCSI ハード_ディスクの SCSI ID を取得するには
"ディスク パラメーターを有効にします。EnableUUID = TRUE"次のようにします。

1. 仮想マシンをシャットダウンします。
2. 左側のパネルで VM のエントリを右クリックし、**[Edit Settings]** を選択します。
3. **[Options]** タブをクリックします。 選択 **詳細設定 > [General item** > **構成パラメーター**します。 **[Configuration Parameters]** オプションは、マシンのシャット ダウン時にのみ使用されます。

    ![](./media/site-recovery-failback-azure-to-vmware/image14.png)

4. **[disk.EnableUUID]** と表示される行が存在するかどうかが確認されます。 設定されている場合は **False** に設定 ** True **(しない大文字と小文字は区別されます。 そのような行が存在し、値が [True] の場合は、**[Cancel]** をクリックし、ゲスト オペレーティング システムが起動した後に、その OS 内で SCSI コマンドをテストします。 そのような行が存在しない場合、**[Add Row]** をクリックします。
5. **[Name]** 列に disk.EnableUUID を追加します。 その値を TRUE に設定します。 前述の値を追加する際には、二重引用符で囲まないでください。

    ![](./media/site-recovery-failback-azure-to-vmware/image15.png)

#### その他のパッケージをダウンロードおよびインストールする

注: その他のパッケージをダウンロードおよびインストールする前に、システムがインターネットに接続されていることを確認します。

\ # yum-y xfsprogs perl lsscsi rsync wget kexec-ツールのインストール

このコマンドは、CentOS 6.6 からこれらの 15 のパッケージをダウンロードします。
リポジトリにインストールします。

bc-1.06.95-1.el6.x86\_64.rpm

busybox-1.15.1-20.el6.x86\_64.rpm

elfutils-libs-0.158-3.2.el6.x86\_64.rpm

kexec-tools-2.0.0-280.el6.x86\_64.rpm

lsscsi-0.23-2.el6.x86\_64.rpm

lzo-2.03-3.1.el6\_5.1.x86\_64.rpm

perl-5.10.1-136.el6\_6.1.x86\_64.rpm

perl-Module-Pluggable-3.90-136.el6\_6.1.x86\_64.rpm

perl-Pod-Escapes-1.04-136.el6\_6.1.x86\_64.rpm

perl-Pod-Simple-3.13-136.el6\_6.1.x86\_64.rpm

perl-libs-5.10.1-136.el6\_6.1.x86\_64.rpm

perl-version-0.77-136.el6\_6.1.x86\_64.rpm

rsync-3.0.6-12.el6.x86\_64.rpm

snappy-1.1.0-1.el6.x86\_64.rpm

wget-1.12-5.el6\_6.1.x86\_64.rpm

注: ソース コンピューターは、ルートまたはブートとして Reiser または XFS ファイル システムを使用している場合
デバイスで、次のパッケージをダウンロードおよびにインストールされている必要があります。
保護する前に Linux マスター ターゲットです。

\ # cd またはローカル

\ # wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/kmod-reiserfs-0.0-1.el6.elrepo.x86_64.rpm>

\ # wget
<http://elrepo.org/linux/elrepo/el6/x86_64/RPMS/reiserfs-utils-3.6.21-1.el6.elrepo.x86_64.rpm>

\ # rpm - ivh kmod-reiserfs-0.0-1.el6.elrepo.x86\_64.rpm
reiserfs-utils-3.6.21-1.el6.elrepo.x86\_64.rpm

\ # wget
<http://mirror.centos.org/centos/6.6/os/x86_64/Packages/xfsprogs-3.1.1-16.el6.x86_64.rpm>

\ # rpm - ivh xfsprogs-3.1.1-16.el6.x86\_64.rpm

#### カスタム構成変更を適用する

これらの変更を適用する前に、前のセクションが完了していることを確認してから、次の手順に従います。


1. RHEL 6-64 ユニファイド・エージェント バイナリを、新たに作成した OS にコピーします。

2. バイナリを解凍するには、このコマンドを実行します * * - この tar \<File name\>* *。

3. アクセス許可を付与するには、このコマンドを実行します \ # **chmod 755./ApplyCustomChanges.sh**。

4. スクリプトを実行します。 **\ #./ApplyCustomChanges.sh**します。 スクリプトは、サーバー上で 1 回のみ実行してください。 スクリプトの実行後に、サーバーを再起動します。



### Linux サーバーをインストールする

1. [ダウンロード](http://go.microsoft.com/fwlink/?LinkID=529757) インストール ファイル。
2. 任意の sftp クライアント ユーティリティを使用して、ファイルを Linux マスター ターゲット仮想マシンにコピーします。 仮想の Linux マスター ターゲットにログオンする代わりに
コンピューターし、インストール パッケージをダウンロードする wget を使用して、
リンクを提供
3. Linux マスター ターゲット サーバー仮想マシンを使用して、ログオン、ssh
任意のクライアント
4. 展開先の Azure ネットワークに接続しているかどうかは、
Linux マスター ターゲット サーバー、VPN 接続を使用しを使用して、
内部の IP アドレスをサーバーで見つかりますで、
仮想マシン **ダッシュ ボード** タブをクリックし、Linux マスターへの接続に 22
Secure Shell を使用してサーバーを対象に。
5. パブリック経由で Linux マスター ターゲット サーバーに接続する場合
インターネット接続には、Linux マスター ターゲット サーバーのパブリック仮想を使用します。
IP アドレス (仮想マシンから **ダッシュ ボード** ] タブ) と、パブリック
作成されたエンドポイントを ssh Linux servder にログインします。
6. Gzip 圧縮された Linux マスター ターゲット サーバー インストーラーからファイルを抽出します。
実行して、tar アーカイブ: *"tar – xvzf Microsoft-ASR\_UA\_8.2.0.0\_RHEL6-64\*"* インストーラ ファイルを含んでいるディレクトリからです。

    ![](./media/site-recovery-failback-azure-to-vmware/image16.png)

7. 別のディレクトリの変更にインストーラー ファイルを展開した場合
tar アーカイブの内容がいたディレクトリに
抽出されます。 このディレクトリ パスから、“sudo ./install.sh” を実行します。

    ![](./media/site-recovery-failback-azure-to-vmware/image17.png)

8. プライマリ ロールの選択を求められたら、**2 (マスター ターゲット)** を選択します。 その他の対話型インストール オプションについては、既定値のままにします。
9. インストールを続行して、ホストの構成の待機
インターフェイスを表示します。 Linux マスターの Host Configuration ユーティリティ
sarget サーバーは、コマンド ライン ユーティリティです。 サイズ変更されない、ssh クライアント
ユーティリティのウィンドウです。 方向キーを使用して、 **グローバル** オプションとキーを押します
キーボードを入力します。

    ![](./media/site-recovery-failback-azure-to-vmware/image18.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image19.png)

10. **[IP]** フィールドに構成サーバーの内部 IP アドレス (構成サーバー VM の **[ダッシュボード]** タブにあります) を入力し、Enter キーを押します。 **[Port]** に、「**22**」と入力し、Enter キーを押します。
11.  **[HTTPS の使用]** は **[はい]** のままにして、Enter キーを押します。
12.  構成サーバーで生成されたパスフレーズを入力します。 Windows マシンから PuTTY クライアントを使用して、ssh 経由で Linux 仮想マシンに接続している場合、Shift キーを押しながら Insert キーを押すことにより、クリップボードのコンテンツを貼り付けることができます。 Ctrl キーを押しながら C キーを押してパスフレーズをローカルのクリップボードにコピーし、Shift キーを押しながら Insert キーを押して貼り付けます。 Enter キーを押します。
13.  右矢印キーを使用して [quit] を選択し、Enter キーを押します。 インストールが完了するのを待ちます。

    ![](./media/site-recovery-failback-azure-to-vmware/image20.png)


何らかの理由は、Linux マスター ターゲットの登録が失敗した場合
構成サーバーにサーバー行うことができますのでもう一度実行して、
ホスト/usr/local/ASR/Vx/bin/hostconfigcli から構成ユーティリティ (学習
まず、スーパー ユーザーとして chmod を実行して、このディレクトリにアクセス許可を設定) します。


#### マスター ターゲットの登録を検証する

マスター ターゲット サーバーが正常に登録されていることを検証することができます。
Azure Site Recovery コンテナーで構成サーバーに > **構成サーバー** > **サーバーの詳細**します。
>[AZURE.NOTE] マスター ターゲット サーバーを登録した後に、仮想マシンが Azure から削除された可能性がある、またはエンドポイントが正しく構成されていないという構成エラーを受け取った場合、これはマスター ターゲットが Azure にデプロイされるときに Azure のエンドポイントでマスター ターゲットの構成が検出されているにもかかわらず、これがオンプレミスのマスター ターゲット サーバーに適用できないことが原因です。 これはフェールバックには影響しないため、このエラーは無視することができます。 



## 手順 4: オンプレミスのサイトに仮想マシンを保護する

フェールバックする前に、オンプレミスのサイトに VM を保護する必要があります。

### 開始する前に

VM は、Azure にフェールオーバーは、余分な一時ドライブのページが追加されます。
ファイルを保存します。 これは通常必要はありませんが余分なドライブ、
専用のドライブが既にありますので、VM をフェールオーバーします
ページのファイルです。 仮想マシンの逆方向に保護を開始する前にする必要があります。
このドライブがオフラインになるにならないようにすることを確認します。
保護されています。 これを行うには、次の手順を実行します。

1.  [コンピューターの管理] を開き、[ストレージ管理] を選択します。これにより、オンラインおよびマシンにアタッチされているディスクが一覧表示されます。
2.  マシンにアタッチされている一時ディスクを選択し、オフラインにする手順を実行します。

### VM を保護する

1. Azure ポータルで仮想マシンの状態をチェックし、フェールオーバーしていることを確認します。

    ![](./media/site-recovery-failback-azure-to-vmware/image21.png)

2. お使いのマシンで vContinuum を起動します。

    ![](./media/site-recovery-failback-azure-to-vmware/image8.png)

3. **[New Protection]** をクリックし、オペレーティング システムの種類を選択します。

4.  新しいウィンドウが開き、フェールバックする VM の **[OS type]** > **[Get Details]** を選択します。 **[Primary server details]** で、保護対象の仮想マシンを識別して選択します。 VM は、フェールオーバー前の vCenter のホスト名の下に表示されます。
5.  (既に Azure にフェールオーバーしている) 保護対象の仮想マシンを選択すると、ポップアップ ウィンドウに仮想マシンの 2 つのエントリが表示されます。 これは、構成サーバーが、登録されている仮想マシンの 2 つのインスタンスを検出しているためです。 オンプレミス VM のエントリを削除して、正しい VM を保護できるようにする必要があります。 ここで、正しい Azure VM エントリを識別するには、Azure VM にログインし、C:\Program Files (x86)\Microsoft Azure Site Recovery\Application Data\etc にアクセスします。ファイル drscout.conf に、ホスト ID のエントリがあります。 VContinuum のダイアログには、VM のホスト ID を確認したエントリを保持します。 他のエントリはすべて削除します。 正しい VM を選択するため、VMの IP アドレスを参照できます。 オンプレミスの IP アドレス範囲は、オンプレミスの VM になります。

    ![](./media/site-recovery-failback-azure-to-vmware/image22.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image23.png)

6. vCenter サーバーで、仮想マシンを停止します。 VM をオンプレミスで削除することもできます。
7. 次に、VM を保護するために宛先とするオンプレミス MT サーバーを指定します。 これを行うには、フェールバック先の vCenter サーバーに接続します。

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

8. VM の復旧先となるホストに基づいて、マスター ターゲット サーバーを選択します。

    ![](./media/site-recovery-failback-azure-to-vmware/image24.png)

9. 次に、各仮想マシンのレプリケーション オプションを設定します。 これを行うには、復旧側のデータストアを選択する必要があります。これは、VM の復旧先となるデータストアです。 テーブルは、各 VM に提供する必要があるさまざまなオプションをまとめたものです。

    ![](./media/site-recovery-failback-azure-to-vmware/image25.png)

    **オプション**| **推奨されるオプション値**
   ---|---
    プロセス サーバーの IP アドレス| Azure にデプロイされたプロセス サーバーの選択
    保持するサイズ (MB)|
    保持するサイズの値| 1
    日数/時間| 日数
    一貫性を維持する期間| 1
    ターゲット データストアの選択| 復旧側で利用可能なデータストア。このデータストアは、十分な容量があり、仮想マシンの復元先の ESX ホストで利用できる必要があります。

10. 仮想マシンが、オンプレミス サイトにフェールオーバーした後に取得するプロパティを構成します。 プロパティを次のテーブルにまとめて示します。

    ![](./media/site-recovery-failback-azure-to-vmware/image26.png)

     **プロパティ**| **詳細**
    ---|---
     ネットワーク構成| 検出された各ネットワーク アダプターを選択し、**[変更]** をクリックして、仮想マシンのフェールバック IP アドレスを構成します。
     ハードウェア構成| VM の CPU とメモリを指定します。設定を、保護対象のすべての VM に適用することができます。[CPU] と [Memory] の正しい値を識別するには、IaaS VM ロール サイズを参照し、割り当てられているコア数とメモリ サイズを確認します。
     表示名| オンプレミスへのフェールバック後は、仮想マシンの名前を vCenter のインベントリに表示されるとおりに変更することができます。既定の名前は、仮想マシンのコンピューター ホスト名です。VM 名を識別するには、保護グループの VM リストを参照できます。
     NAT 構成| 以降で詳細に説明します。

    ![](./media/site-recovery-failback-azure-to-vmware/image27.png)

#### NAT 設定を構成する

1. 仮想マシンの保護を有効にするには、2 つの通信チャネルを確立する必要があります。 最初のチャネルは、仮想マシンとプロセス サーバーとの間に確立します。 このチャネルは、VM から収集したデータを、プロセス サーバーに送信します。この後、データはマスター ターゲット サーバーに送信されます。 プロセス サーバーと保護対象の仮想マシンが同じ Azure 仮想ネットワーク上に存在する場合、NAT 設定を使用する必要はありません。 それ以外の場合は、NAT 設定を指定します。 Azure でのプロセス サーバーのパブリック IP アドレスを確認します。

    ![](./media/site-recovery-failback-azure-to-vmware/image28.png)

2. 2 つ目のチャネルは、プロセス サーバーとマスター ターゲット サーバーとの間に確立します。 NAT を使用するかしないかのオプション選択は、VPN ベースの接続を使用しているか、またはインターネット経由で通信しているかによって異なります。 VPN を使用している場合は NAT を選択しないでください。ただし、インターネット接続を使用している場合は除きます。

    ![](./media/site-recovery-failback-azure-to-vmware/image29.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image30.png)

3. 指定されたとおりにオンプレミス仮想マシンを削除しておらず、フェールバック先のデータストアに依然として古い VMDK が残っている場合、フェールバック VM が新しい場所に作成されることを確認する必要があります。 このために、 **[Advanced]** 設定を選択し、**[Folder Name Settings]** セクションでリストア先となる新しい代替フォルダーを指定します。 他のオプションは既定の設定のままにします。 フォルダー名設定をすべてのサーバーに適用します。

    ![](./media/site-recovery-failback-azure-to-vmware/image31.png)

4. 適合性チェックを実行して、仮想マシンが、オンプレミスを宛先として保護される準備が整っていることを確認します。

    ![](./media/site-recovery-failback-azure-to-vmware/image32.png)

5. 完了するまで待ちます。 すべての VM で完了したら、保護計画の名前を指定できます。 **[保護]** をクリックして開始します。

    ![](./media/site-recovery-failback-azure-to-vmware/image33.png)

6. 進捗状況は vContinuum で監視できます。

    ![](./media/site-recovery-failback-azure-to-vmware/image34.png)

7. **保護計画のアクティブ化**の手順が完了すると、Site Recovery ポータルで VM の保護を監視することができます。

    ![](./media/site-recovery-failback-azure-to-vmware/image35.png)

8. VM をクリックすることで正確な状態を確認し、進行状況を監視することができます。

    ![](./media/site-recovery-failback-azure-to-vmware/image36.png)

## フェールバック計画を準備する

フェールバック計画は vContinuum を使用してを準備するようにアプリケーション
いつでも、内部設置型サイトに再び失敗ことができます。 これらの復旧計画は、Site Recovery の復旧計画に非常に似ています。

1.  vContinuum を起動し、**[計画管理]** > **[復旧]** の順に選択します。VM のフェールオーバーに使用されているすべての計画の一覧を確認できます。 同じ計画を使用して復旧することができます。

    ![](./media/site-recovery-failback-azure-to-vmware/image37.png)

2. 保護計画と、その中で復旧するすべての VM を選択します。 各 VM を選択すると、ターゲット ESX サーバーおよびソース VM ディスクなどの詳細を確認できます。 **[次へ]** をクリックすると復旧ウィザードが起動し、復旧する VM を選択できます。

    ![](./media/site-recovery-failback-azure-to-vmware/image38.png)

3. 複数のオプションに基づいて復旧できますが、仮想マシンの最新のデータが使用されるように、**[最新タグ]** を使用し、 **[すべての VM に適用]** を選択することをお勧めします。
4. **[Run Readiness check]** をクリックして、適合性チェックを実行します。これにより、VM の復旧を有効にするための適切なパラメーターが構成されているかどうかが確認されます。 すべてのチェックが成功したら、**[次へ]** をクリックします。 成功しない場合は、ログを確認し、エラーを解決します。

    ![](./media/site-recovery-failback-azure-to-vmware/image39.png)

8.  **[VM 構成]** で、復旧設定が正しく設定されていることを確認します。 VM の設定は、必要に応じて変更できます。

    ![](./media/site-recovery-failback-azure-to-vmware/image40.png)

9. 最後に、復旧対象の仮想マシンの一覧を確認し、復旧順序を指定します。 VM はコンピューターのホスト名で表示されることに注意してください。 その方法は
コンピューターのホスト名を仮想マシンにマップできない場合があります。
名前をマップするには、Azure の仮想マシンの **[ダッシュボード]** に移動して、VM のホスト名を確認します。

    ![](./media/site-recovery-failback-azure-to-vmware/image41.png)

10. 計画の名前を指定し、**[後で復旧する]** を選択します。 初期の保護が完了しない可能性があるため、復旧は後で行うことをお勧めします。
11. 後からではなく今すぐ復旧することを選択した場合は、**[復旧]** をクリックして計画を保存するか、復旧をトリガーします。 計画が保存されているかどうかは復旧の状態で確認できます。

    ![](./media/site-recovery-failback-azure-to-vmware/image42.png)

    ![](./media/site-recovery-failback-azure-to-vmware/image43.png)

## 仮想マシンを復旧する

計画が作成されたら、仮想マシンを復旧できます。 復旧の前に、仮想マシンの同期が完了していることを確認してください。 [Recovery Status] に [OK] が表示されていると、保護が完了し、RPO しきい値が満たされています。 VM のプロパティで正常性を確認することができます。

![](./media/site-recovery-failback-azure-to-vmware/image44.png)

復旧を開始する前に、Azure 仮想マシンを停止します。 これにより、スプリット ブレインがなくなり、ユーザーがアプリケーションの 1 つのコピーにしかアクセスしなくなります。


1.  保存された計画を開始します。 vContinuum で、**[Monitor]** を選択して計画を監視します。 これにより、実行されているすべての計画が一覧表示されます。

    ![](./media/site-recovery-failback-azure-to-vmware/image45.png)

2.  **[復旧]** で計画を選択して、**[開始]** をクリックします。 復旧を監視することができます。 VM が起動したら、vCenter で VM に接続することができます。

    ![](./media/site-recovery-failback-azure-to-vmware/image46.png)

## フェールバック後に Azure を宛先として保護する

フェールバックが完了した後おそらくにしておくバーチャル マシンを保護します。
マシンでもう一度使用します。 これを行うには、次の手順を実行します。

1.  オンプレミスの仮想マシンが動作しており、アプリケーションにアクセスできることを確認します。
2.  Azure Site Recovery ポータルで、仮想マシンを選択して削除します。 仮想マシンの保護を無効にします。 これにより、VM が保護されなくなります。
3.  Azure でフェールオーバーされた Azure の仮想マシンを削除します。
4.  VSphere 上の古い仮想マシンを削除します。 これらは、以前に Azure にフェールオーバーした VM です。
5.  Site Recovery ポータルで、最近フェールオーバーした VM を保護します。 保護されてこと後は、復旧計画に追加できます。

## 次のステップ

[について](site-recovery-vmware-to-azure.md) VMware 仮想マシンを Azure にレプリケート







