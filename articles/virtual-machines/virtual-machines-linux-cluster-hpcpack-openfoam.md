<properties
 pageTitle="Linux VM で HPC Pack を使用して OpenFOAM を実行する | Microsoft Azure"
 description="Microsoft HPC Pack クラスターを Azure にデプロイし、RDMA ネットワークに接続された複数の Linux 計算ノードで OpenFOAM ジョブを実行します。"
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
 ms.tgt_pltfrm="vm-linux"
 ms.workload="big-compute"
 ms.date="11/25/2015"
 ms.author="danlep"/>

# Azure の Linux RDMA クラスター上で Microsoft HPC Pack を使用して OpenFoam を実行する

この記事は、Azure と実行上の Microsoft HPC Pack クラスターをデプロイする方法を示します、 [OpenFoam](http://openfoam.com/) Azure リモート ダイレクト メモリ アクセス (RDMA) ネットワーク経由で接続する複数の Linux 計算ノード上のジョブ Intel MPI をします。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。

OpenFOAM (Open Field Operation And Manipulation) は、無償提供されているオープン ソースの計算流体力学 (CFD) ソフトウェア パッケージです。工学分野や科学分野の企業や学術機関で幅広く利用されています。 メッシュ化するためのツール (特に、複雑な CAD ジオメトリ用の並列メッシュ処理機構である snappyHexMesh) や、前処理と後処理のためのツールが備わっています。 ほぼすべての処理が並列に実行され、ユーザーは、コンピューターのハードウェアを最大限に活用できます。  

Microsoft HPC Pack は、Microsoft Azure 仮想マシンのクラスター上で各種の大規模な HPC および並列アプリケーション (MPI アプリケーションなど) を実行する機能を備えています。 Microsoft HPC Pack 2012 R2 Update 2 以降、HPC Pack では、HPC Pack クラスターにデプロイされた Linux 計算ノード VM で Linux HPC アプリケーションを実行する機能もサポートしています。 参照してください [Azure で HPC Pack クラスターで Linux 計算ノードを使ってみる](virtual-machines-linux-cluster-hpcpack.md) Linux を使用する入門用の計算ノード HPC Pack を使用します。

>[AZURE.NOTE] この記事では、Linux HPC クラスターで MPI ワークロードを実行して Linux システムの管理に関する知識があると想定しています。 

## 前提条件

*   **Linux での HPC Pack クラスター計算ノード** -を参照してください [Azure で HPC Pack クラスターで Linux 計算ノードを使ってみる](virtual-machines-linux-cluster-hpcpack.md) の前提条件と Linux の HPC Pack クラスターを展開する手順は、Azure Marketplace で Azure PowerShell スクリプトと HPC Pack イメージを使用して、Azure 上のノードを計算します。 Azure の RDMA ネットワークにアクセスする A8 コンピューティング集中型インスタンスを使用する追加の考慮事項を参照してください。 [A8、A9、A10、A11 コンピューティング集中型インスタンス](virtual-machines-a8-a9-a10-a11-specs.md)します。

    A8 サイズの Windows Server 2012 R2 ヘッド ノード 1 つと、A8 サイズの SUSE Linux Enterprise Server 12 計算ノード 2 つとから成る Azure ベースの HPC Pack クラスターをデプロイする際に、そのスクリプトで使用する XML 構成ファイルのサンプルを次に示します。 該当する値は、実際のサブスクリプションとサービス名に置き換えてください。

    >[AZURE.NOTE]現時点では、ネットワークで、Azure Linux RDMA は Azure Marketplace (b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708) に RDMA 対応の SUSE Linux Enterprise Server 12 イメージから作成された Vm でのみサポートされています。

    ```
    <?xml version="1.0" encoding="utf-8" ?>
    <IaaSClusterConfig>
      <Subscription>
        <SubscriptionName>Subscription-1</SubscriptionName>
        <StorageAccount>allvhdsje</StorageAccount>
      </Subscription>
      <Location>Japan East</Location>  
      <VNet>
        <VNetName>suse12rdmavnet</VNetName>
        <SubnetName>SUSE12RDMACluster</SubnetName>
      </VNet>
      <Domain>
        <DCOption>HeadNodeAsDC</DCOption>
        <DomainFQDN>hpclab.local</DomainFQDN>
      </Domain>
      <Database>
        <DBOption>LocalDB</DBOption>
      </Database>
      <HeadNode>
        <VMName>SUSE12RDMA-HN</VMName>
        <ServiceName>suse12rdma-je</ServiceName>
        <VMSize>A8</VMSize>
        <EnableRESTAPI />
        <EnableWebPortal />
      </HeadNode>
      <LinuxComputeNodes>
        <VMNamePattern>SUSE12RDMA-LN%1%</VMNamePattern>
        <ServiceName>suse12rdma-je</ServiceName>
        <VMSize>A8</VMSize>
        <NodeCount>2</NodeCount>
        <ImageName>b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708</ImageName>
      </LinuxComputeNodes>
    </IaaSClusterConfig>
```

    **その他の注意事項**

    *   すべての Linux 計算ノードを 1 つのクラウド サービスにデプロイし、そのノード間で RDMA ネットワーク接続を使用する。

    *   Linux ノードのデプロイ後、特別な管理タスクを実行する目的で、SSH 接続を使用する必要がある場合、Linux VM ごとの SSH 接続の情報を Azure ポータルから探す。  
        
*   **Intel MPI** : Azure で Linux 計算ノードに OpenFOAM を実行する Intel MPI ライブラリ 5 のランタイムをする必要があります、 [Intel.com のサイト](https://software.intel.com/en-us/intel-mpi-library/)します。 その後、Linux 計算ノードに Intel MPI をインストールすることになります。 この準備をするために、Intel に登録した後、確認の電子メールに含まれる関連 Web ページへのリンクをクリックし、適切なバージョンの Intel MPI (.tgz ファイル) のダウンロード リンクをコピーします。 この記事は、Intel MPI バージョン 5.0.3.048 に基づきます。

*   **OpenFOAM ソース パック** -から Linux 用 OpenFOAM ソース パック ソフトウェアをダウンロード、 [OpenFOAM Foundation サイト](http://www.openfoam.org/download/source.php)します。 この記事は、OpenFOAM-2.3.1.tgz としてダウンロードできる Source Pack Version 2.3.1 に基づいて説明しています。 Linux 計算ノードに対する OpenFOAM のアンパックとコンパイルについては、この記事で後述する手順に従ってください。

*   **EnSight** (省略可能) - OpenFOAM シミュレーションの結果を表示し、ダウンロードしの Windows バージョンをインストールする、 [EnSight](https://www.ceisoftware.com/download/) HPC Pack クラスターのヘッド ノード上の視覚エフェクトと分析プログラムです。 ライセンスとダウンロードの詳細については、EnSight のサイトを参照してください。


## コンピューティング ノードの相互の信頼関係をセットアップする

複数の Linux ノードにノード間のジョブを実行しているノードが相互に信頼関係が必要です (によって **rsh** または **ssh**)。 Microsoft HPC Pack IaaS デプロイ スクリプトを使用して HPC Pack クラスターを作成する場合は、指定した管理者アカウントに対して永続的な相互の信頼関係がスクリプトによって自動的にセットアップされます。 管理者以外のユーザーをクラスター ドメインに作成した場合は、それらのユーザーにジョブを割り当てるときに、ノード間に一時的な相互の信頼関係をセットアップする必要があります。ジョブ終了後、この関係は破棄します。 これをユーザーごと行うには、HPC Pack で使用するクラスターに RSA キー ペアを指定して、信頼関係を確立します。

### RSA キー ペアの生成

Linux を実行して、公開キーと秘密キーが含まれている RSA キー ペアを生成する簡単 **で問題** コマンドです。

1.  Linux コンピューターにログオンします。

2.  次のコマンドを実行します。

    ```
    ssh-keygen -t rsa
    ```

    >[AZURE.NOTE] キーを押して **Enter** コマンドが完了するまで、既定の設定を使用します。 ここでは、パスフレーズを入力しないでください。パスワードが表示されたら、キーを押して **Enter**します。

    ![RSA キー ペアの生成][keygen]

3.  ディレクトリを ~/.ssh ディレクトリに変更します。 秘密キーは id_rsa に格納され、公開キーは id_rsa.pub に格納されます。

    ![公開キーと秘密キー][keys]

### HPC Pack クラスターにキー ペアを追加する
1.  HPC Pack の管理者アカウント (デプロイ スクリプトを実行したときにセットアップした管理者アカウント) を使用してヘッド ノードへのリモート デスクトップ接続を行います。

2. 標準的な Windows Server 手順を使用して、クラスターの Active Directory ドメインにドメイン ユーザー アカウントを作成します。 たとえば、ヘッド ノードで Active Directory ユーザーとコンピューター ツールを使用します。 この記事の例では、hpclab\hpcuser という名前のドメイン ユーザーを作成することを前提とします。

3.  C:\cred.xml という名前のファイルを作成し、そこに RSA キーのデータをコピーします。 このファイルの例については、この記事の最後にあるサンプル ファイルを参照してください。

    ```
    <ExtendedData>
        <PrivateKey>Copy the contents of private key here</PrivateKey>
        <PublicKey>Copy the contents of public key here</PublicKey>
    </ExtendedData>
    ```

4.  コマンド プロンプトを開き、次のコマンドを入力して、hpclab\hpcuser アカウントの資格情報データを設定します。 使用する、 **extendeddata** キーのデータ用に作成した C:\cred.xml ファイルの名前を渡すためのパラメーターです。

    ```
    hpccred setcreds /extendeddata:c:\cred.xml /user:hpclab\hpcuser /password:<UserPassword>
    ```

    このコマンドは、出力なしで正常に終了します。 ジョブを実行するために必要なユーザー アカウントの資格情報を設定したら、cred.xml ファイルを安全な場所に保存するか、または削除します。

5.  Linux ノードの 1 つに対して RSA キー ペアを生成した場合は、キーの使用が終わった後に、それらを削除することを忘れないでください。 既存の id_rsa ファイルまたは id_rsa.pub ファイルが見つかった場合、HPC Pack は相互の信頼関係をセットアップしません。

>[AZURE.IMPORTANT] Linux ノードにルート アカウントで、管理者によって実行されるジョブが実行されるので、クラスター管理者として共有クラスターで Linux ジョブを実行する推奨されません。 管理者以外のユーザーによって送信されたジョブは、ジョブ ユーザーと同じ名前を持つローカルの Linux ユーザー アカウントで実行されます。HPC Pack は、ジョブに割り当てられたすべてのノード間に、この Linux ユーザー用の相互の信頼関係をセットアップします。 ジョブを実行する前に Linux ノードに対して手動で Linux ユーザーをセットアップすることも、ジョブの送信時に HPC Pack がユーザーを自動的に作成するようにすることもできます。 HPC Pack でユーザーを作成した場合、ジョブの完了後にユーザーは HPC Pack によって削除されます。 ノード上でジョブが完了すると、セキュリティ上の脅威を軽減するためにキーは削除されます。

## Linux ノード用にファイル共有をセットアップする

今度は、ヘッド ノード上のフォルダーに標準の SMB 共有を設定し、すべての Linux ノード上に共有フォルダーをマウントすることで、それらの Linux ノードが共通のパスを使用してアプリケーション ファイルにアクセスできるようにします。 必要であれば他のファイル共有方法 (Azure Files 共有、NFS 共有など) を使用することもできます。特に Azure Files 共有は多くのシナリオに対応します。 ファイル共有での詳細な手順を参照してください [Azure で HPC Pack クラスターで Linux 計算ノードを使ってみる](virtual-machines-linux-cluster-hpcpack.md)します。

1.  ヘッド ノードにフォルダーを作成します。読み書き権限を設定して、フォルダーを全員で共有します。 たとえば、ヘッド ノードで C:\OpenFOAM を \\\SUSE12RDMA-HN\OpenFOAM として共有します。 ここでは、 *SUSE12RDMA HN* ヘッド ノードのホスト名です。

2.  Windows PowerShell ウィンドウを開き、次のコマンドを実行し、共有フォルダーをマウントします。

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

    clusrun /nodegroup:LinuxNodes mount -t cifs //SUSE12RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>’`,dir_mode=0777`,file_mode=0777
    ```

最初のコマンドで「/openfoam」という名前のフォルダーが LinuxNodes グループのすべてのノードで作成されます。 2 つ目のコマンドにより、dir_mode ビットと file_mode ビットが「777」に設定された共有フォルダー //SUSE12RDMA-HN/OpenFOAM が Linux ノードにマウントされます。  *Username* と *パスワード* コマンドでヘッド ノード上のユーザーの資格情報である必要があります。

>[AZURE.NOTE]"\'"2 番目のコマンドで記号は PowerShell のエスケープ記号です。 "\'、"、「,」(コンマ) ことを意味のコマンドの一部であります。

## MPI と OpenFOAM のインストール

RDMA ネットワークで OpenFOAM を MPI ジョブとして実行するには、Intel MPI ライブラリを使って OpenFOAM をコンパイルする必要があります。 

いくつかを最初に実行します **clusrun** Intel MPI ライブラリと OpenFOAM すべての Linux ノードをインストールするコマンドです。 先ほど構成したヘッド ノードの共有場所を使用して、Linux ノード間でインストール ファイルを共有します。

>[AZURE.IMPORTANT]これらのインストールの手順をコンパイルする例とは特に依存コンパイラおよびライブラリが正しくインストールされていることを確認するための Linux のシステム管理タスクのいくつかの知識が必要です。 ご使用のバージョンの Intel MPI および OpenFOAM に必要な特定の環境変数や設定を変更しなければならない場合があります。 詳細をご覧ください [Linux のインストール ガイドについては、Intel MPI Library](http://scc.ustc.edu.cn/zlsc/tc4600/intel/impi/INSTALL.html) と [OpenFOAM ソース パックのインストール](http://www.openfoam.org/download/source.php)します。


### Intel MPI のインストール

ダウンロードした Intel MPI インストール パッケージ (この例では l_mpi_p_5.0.3.048.tgz) をヘッド ノード上の C:\OpenFoam に保存し、Linux ノードが /openfoam からこのファイルにアクセスできるようにします。 実行して **clusrun** Linux ノードのすべての Intel MPI ライブラリをインストールします。

1.  次のコマンドで、各ノードの /opt/intel にインストール パッケージをコピーして抽出します。

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/intel

    clusrun /nodegroup:LinuxNodes cp /openfoam/l_mpi_p_5.0.3.048.tgz /opt/intel/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/intel/l_mpi_p_5.0.3.048.tgz -C /opt/intel/
    ```

2.  Intel MPI Library をサイレント インストールするには、silent.cfg ファイルを使用します。 例については、この記事の最後にあるサンプル ファイルを参照してください。 このファイルを共有フォルダー /openfoam に格納します。 Silent.cfg ファイルに関する詳細については、「 [Linux Installation Guide - サイレント インストール用の Intel MPI Library](http://scc.ustc.edu.cn/zlsc/tc4600/intel/impi/INSTALL.html#silentinstall)します。

    >[AZURE.TIP]Linux でのテキスト ファイル、silent.cfg ファイルを保存する改行位置 (のみ LF、CR LF ありません) をことを確認します。 これにより、スクリプトは Linux ノード上で適切に動作します。

3.  サイレント モードで Intel MPI Library をインストールします。
 
    ```
    clusrun /nodegroup:LinuxNodes bash /opt/intel/l_mpi_p_5.0.3.048/install.sh --silent /openfoam/silent.cfg
    ```
    
### MPI の構成

テストを行うため、各 Linux ノード上の /etc/security/limits.conf に以下の行を追加してください。

```
*               hard    memlock         unlimited
*               soft    memlock         unlimited
```

limits.conf ファイルを更新した後で Linux ノードを再起動します。 たとえば、次を使用して **clusrun** コマンドです。

```
clusrun /nodegroup:LinuxNodes systemctl reboot
```

再起動後、共有フォルダーが /openfoam としてマウントされていることを確認してください。

### OpenFOAM のコンパイルとインストール

ダウンロードした OpenFOAM Source Pack のインストール パッケージ (この例では OpenFOAM-2.3.1.tgz) をヘッド ノード上の C:\OpenFoam に保存し、Linux ノードが /openfoam からこのファイルにアクセスできるようにします。 実行して **clusrun** をすべての Linux ノードに対して OpenFOAM をコンパイルします。


1.  すべての Linux ノードに /opt/OpenFOAM フォルダーを作成し、そのフォルダーにソース パッケージをコピーして抽出します。

    ```
    clusrun /nodegroup:LinuxNodes mkdir -p /opt/OpenFOAM

    clusrun /nodegroup:LinuxNodes cp /openfoam/OpenFOAM-2.3.1.tgz /opt/OpenFOAM/

    clusrun /nodegroup:LinuxNodes tar -xzf /opt/OpenFOAM/OpenFOAM-2.3.1.tgz -C /opt/OpenFOAM/
    ```

2.  Intel MPI Library を使って OpenFOAM をコンパイルするにはまず、Intel MPI と OpenFOAM の両方について、いくつかの環境変数を設定します。 設定には settings.sh という bash スクリプトを使用します。 例については、この記事の最後にあるサンプル ファイルを参照してください。 このファイルを共有フォルダー /openfoam に格納します (Linux の改行コードで保存すること)。 このファイルには、後で OpenFOAM ジョブを実行するときに使用する MPI と OpenFOAM のランタイムの設定が格納されています。

3. OpenFOAM をコンパイルするために必要な依存パッケージをインストールします。 そのために、Linux のディストリビューションによっては、最初にリポジトリの追加が必要になる場合があります。 実行 **clusrun** には、次のようなコマンド。

    ```
    clusrun /nodegroup:LinuxNodes zypper ar http://download.opensuse.org/distribution/13.2/repo/oss/suse/ opensuse
    
    clusrun /nodegroup:LinuxNodes zypper -n --gpg-auto-import-keys install --repo opensuse --force-resolution -t pattern devel_C_C++
    ```
    
    必要な場合は、個々の Linux ノードに ssh で接続し、コマンドが正しく動作することを確認します。

4.  以下のコマンドを実行して、OpenFOAM をコンパイルします。 コンパイル処理はすぐに完了し、大量のログ情報を標準出力が生成されるので、使用して、 **インターリーブ/** インターリーブの出力を表示するにはオプションです。

    ```
    clusrun /nodegroup:LinuxNodes /interleaved source /openfoam/settings.sh `&`& /opt/OpenFOAM/OpenFOAM-2.3.1/Allwmake
    ```
    
    >[AZURE.NOTE]"\'"コマンドで記号は PowerShell のエスケープ記号です。 "\' (& m)"を意味の"&"をコマンドの一部であります。

## OpenFOAM ジョブを実行するための準備

ここでは、sloshingTank3D (OpenFoam サンプルの 1 つ) という MPI ジョブを 2 つの Linux ノード上で実行するための準備を行います。 

### ランタイム環境のセットアップ

すべての Linux ノードに MPI と OpenFOAM のランタイム環境をセットアップするには、ヘッド ノードの Windows PowerShell ウィンドウで次のコマンドを実行します。 (このコマンドは、SUSE Linux に対してのみ有効です。)

```
clusrun /nodegroup:LinuxNodes cp /openfoam/settings.sh /etc/profile.d/
```

### サンプル データの準備

先ほど構成した (/openfoam としたマウント) ヘッド ノードの共有場所を使用して、Linux ノード間でファイルを共有します。

1.  いずれかの Linux 計算ノードに SSH で接続します。

2.  OpenFOAM ランタイム環境のセットアップが済んでいない場合は、次のコマンドで設定します。

    ```
    $ source /openfoam/settings.sh
    ```
    
3.  sloshingTank3D サンプルを共有フォルダーにコピーし、そのフォルダーに移動します。

    ```
    $ cp -r $FOAM_TUTORIALS/multiphase/interDyMFoam/ras/sloshingTank3D /openfoam/

    $ cd /openfoam/sloshingTank3D
    ```

4.  このサンプルのパラメーターをそのまま使用すると、数十分またはそれ以上、実行に時間がかかる場合があります。実行時間を短縮するために、必要に応じて一部のパラメーターを変更してください。 簡単な方法としては、system/controlDict (時間の制御や、ソリューション データの読み取り/書き込みに関連したあらゆる入力データを格納するファイル) で、時間ステップ変数 deltaT と writeInterval に変更を加えることが考えられます。 たとえば deltaT の値を 0.05 から 0.5 に、writeInterval の値を 0.05 から 0.5 に変更します。

    ![Modify step variables][step_variables]

5.  system/decomposeParDict ファイル内の変数に適切な値を指定します。 この例では、それぞれ 8 つのコアを持った 2 つの Linux ノードを使用しているため、numberOfSubdomains は 16 に、hierarchicalCoeffs の n は (1 1 16) に設定することになります。つまり、16 のプロセスで OpenFOAM を並列実行するという意味です。 OpenFOAM が並列で実行する方法の詳細について「 [OpenFOAM ユーザー ガイド: 並列で実行するアプリケーションで 3.4](http://cfd.direct/openfoam/user-guide/running-applications-parallel/#x12-820003.4)します。

    ![Decompose processes][decompose]

6.  sloshingTank3D ディレクトリから次のコマンドを実行して、サンプル データを準備します。

    ```
    $ . $WM_PROJECT_DIR/bin/tools/RunFunctions

    $ m4 constant/polyMesh/blockMeshDict.m4 > constant/polyMesh/blockMeshDict

    $ runApplication blockMesh

    $ cp 0/alpha.water.org 0/alpha.water

    $ runApplication setFields  
    ```
    
7.  ヘッド ノードの C:\OpenFoam\sloshingTank3D にサンプル データ ファイルがコピーされていることがわかります  (C:\OpenFoam はヘッド ノード上の共有フォルダー)。

    ![Data files on the head node][data_files]

### mpirun のホスト ファイル

この手順では、ホスト ファイル (計算ノードのリスト) を作成する、 **mpirun** コマンドを使用します。

1.  いずれかの Linux ノードの /openfoam に、hostfile という名前の新しいファイルを作成します。このファイルには、すべての Linux ノードから /openfoam/hostfile でアクセスできます。

2.  実際の Linux ノードの名前をこのファイルに入力します。 このファイルの例を次に示します。
    
    ```       
    SUSE12RDMA-LN1
    SUSE12RDMA-LN2
    ```
    
    >[AZURE.TIP]ヘッド ノードで C:\OpenFoam\hostfile でこのファイルを作成することもできます。 その場合、Linux の改行コード (CR LF ではなく LF のみ) を使ったテキスト ファイルとして保存してください。 これにより、スクリプトは Linux ノード上で適切に動作します。

    **Bash スクリプト ラッパー**

    Linux ノードが多数存在し、なおかつその一部でのみジョブを実行する場合、固定ホスト ファイルの使用はお勧めできません。どのノードがジョブに割り当てられるかを把握できないためです。 ここでは、bash スクリプト ラッパーを書き込んで **mpirun** ホスト ファイルを自動的に作成します。 この記事の最後にあるサンプル ファイルで bash スクリプト ラッパーのサンプル (hpcimpirun.sh) を探し、/openfoam/hpcimpirun.sh として保存してください。 そのサンプル スクリプトでは、次の処理が実行されます。

    1.  環境変数を設定 **mpirun**, 、および RDMA のネットワークを介して MPI ジョブを実行する追加のコマンド パラメーターの一部です。 このケースでは、次の設定が行われます。

        *   I_MPI_FABRICS=shm:dapl
        *   I_MPI_DAPL_PROVIDER=ofa-v2-ib0
        *   I_MPI_DYNAMIC_CONNECTION=0

    2.  環境変数 $CCP_NODES_CORES に従ってホスト ファイルを作成します。この変数は、ジョブを起動したときに HPC ヘッド ノードによって設定されます。

        $CCP_NODES_CORES の形式は、次のパターンに従います。

        ```
        <Number of nodes> <Name of node1> <Cores of node1> <Name of node2> <Cores of node2>...`
        ```

        各値の説明:

        * `<Number of nodes>`: このジョブに割り当てるノードの数。  
        
        * `<Name of node_n_...>`: このジョブに割り当てる各ノードの名前。
        
        * `<Cores of node_n_...>`: このジョブに割り当てるノードのコア数。

        たとえば、ジョブを実行するために 2 つのノードが必要である場合、$CCP_NODES_CORES は次のようになります。
        
        ```
        2 SUSE12RDMA-LN1 8 SUSE12RDMA-LN2 8
        ```
        
    3.  呼び出し、 **mpirun** コマンドを使用し、コマンドラインに 2 つのパラメーターを追加します。

        * `--hostfile <hostfilepath>: <hostfilepath>` - スクリプトによって作成されたホスト ファイルのパス。

        * `-np ${CCP_NUMCPUS}: ${CCP_NUMCPUS}` - HPC Pack のヘッド ノードによって設定される環境変数。このジョブに割り当てる合計コア数を格納します。 この場合のプロセスの数を指定 **mpirun**します。


## OpenFOAM ジョブの送信

いよいよ HPC クラスター マネージャーでジョブを送信します。 いくつかのジョブ タスクでは、コマンド ラインにスクリプト hpcimpirun.sh を指定する必要があります。

1. クラスター ヘッド ノードに接続し、HPC クラスター マネージャーを開始します。

2. **リソース管理で**, 、Linux 計算ノードが確実に、 **オンライン** 状態です。 間違っている場合は、選択し、をクリックして **オンライン**します。

3.   **ジョブ管理**, 、クリックして **新しいジョブ**します。

4.  ジョブの名前を入力します。 _sloshingTank3D_します。

    ![Job details][job_details]

5.   **ジョブ リソース**, 、「ノード」としてリソース種類を選択し、、最小値 2 に設定します。 この例では、それぞれ 8 つのコアを持つ 2 つの Linux ノード上でジョブが実行されます。

    ![ジョブ リソース][job_resources]

6.  4 つのタスクをジョブに追加します。それぞれのタスクには、以下のコマンド ラインと設定を使用します。

    >[AZURE.NOTE]実行している `source /openfoam/settings.sh` OpenFOAM コマンドの前に呼び出し、次のタスクのために、OpenFOAM MPI ランタイム環境を設定します。

    *   **タスク 1**します。 実行 **decomposePar** を実行するためのデータ ファイルを生成する **interDyMFoam** 並列にします。
    
        *   このタスクには 1 つのノードを割り当てます。

        *   **コマンド ライン** - `source /openfoam/settings.sh && decomposePar -force > /openfoam/decomposePar${CCP_JOBID}.log`
    
        *   **作業ディレクトリ** -openfoam/sloshingTank3D
        
        次の図を参照してください。 残りのタスクも同様に構成します。

        ![Task 1 details][task_details1]

    *   **タスク 2**します。 実行 **interDyMFoam** と並行して、サンプルを計算します。

        *   このタスクには 2 つのノードを割り当てます。

        *   **コマンド ライン** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh interDyMFoam -parallel > /openfoam/interDyMFoam${CCP_JOBID}.log`

        *   **作業ディレクトリ** -openfoam/sloshingTank3D

    *   **タスク 3**します。 実行 **reconstructPar** 時間ディレクトリの単一のセットへの各 processor_N_ ディレクトリからディレクトリを時間のセットをマージします。

        *   このタスクには 1 つのノードを割り当てます。

        *   **コマンド ライン** - `source /openfoam/settings.sh && reconstructPar > /openfoam/reconstructPar${CCP_JOBID}.log`

        *   **作業ディレクトリ** -openfoam/sloshingTank3D

    *   **タスク 4**します。 実行 **foamToEnsight** OpenFOAM 結果を変換する並列 EnSight へのファイルをフォーマットし、ケースのディレクトリに Ensight という名前のディレクトリに EnSight ファイルを配置します。

        *   このタスクには 2 つのノードを割り当てます。

        *   **コマンド ライン** - `source /openfoam/settings.sh && /openfoam/hpcimpirun.sh foamToEnsight -parallel > /openfoam/foamToEnsight${CCP_JOBID}.log`

        *   **作業ディレクトリ** -openfoam/sloshingTank3D

6.  これらのタスクに依存関係を追加します。タスクの昇順に追加してください。

    ![Task dependencies][task_dependencies]

7.  クリックして **送信** このジョブを実行します。

    既定では、HPC Pack は、ログオンした現在のユーザー アカウントとしてジョブを送信します。 クリックした後 **送信**, 、ユーザー名とパスワードを入力するように指示するダイアログ ボックスが表示する可能性があります。

    ![ジョブの資格情報][creds]

    条件によっては、HPC Pack は前に入力されたユーザー情報を記憶していて、このダイアログ ボックスを表示しません。 このダイアログ ボックスが HPC Pack によって再び表示されるようにするには、コマンド プロンプトに次のコマンドを入力し、ジョブを送信します。

    ```
    hpccred delcreds
    ```

8.  ジョブの実行には、サンプルに対して設定したパラメーターによって数十分から数時間かかります。 2 つの Linux ノードで実行中のジョブがヒートマップに表示されます。 

    ![ヒート マップ][heat_map]

    それぞれのノードで 8 つのプロセスが開始されます。

    ![Linux processes][linux_processes]

9.  ジョブが終了したら、C:\OpenFoam\sloshingTank3D 下のフォルダーにあるジョブの結果と、C:\OpenFoam にあるログ ファイルを探します。


## EnSight で結果を表示する

必要に応じて使用 [EnSight](https://www.ceisoftware.com/) に視覚化して OpenFOAM ジョブの結果を分析します。 詳細については、視覚化および EnSight でのアニメーションは、情報を参照して [ビデオ ガイド](http://www.ceisoftware.com/wp-content/uploads/screencasts/vof_visualization/vof_visualization.html)します。

1.  ヘッド ノードに EnSight をインストールし、起動します。

2.  C:\OpenFoam\sloshingTank3D\EnSight\sloshingTank3D.case を開きます。

    ビューアーにタンクが表示されます。

    ![Tank in EnSight][tank]

3.  作成、 **アイソサーフェス** から **internalMesh** 変数をクリックして **alpha_water**します。

    ![Create an isosurface][isosurface]

4.  色を設定する **Isosurface_part** 前の手順で作成します。 たとえば水色に設定します。

    ![Edit isosurface color][isosurface_color]

5.  作成、 **Iso ボリューム** から **壁** を選択して **壁** で、 **パーツ** パネル、をクリックし、 **アイソサーフェス** ツールバーのボタンです。

6.  ダイアログ ボックスで選択 **型** として **Isovolume** の最小値を設定および **Isovolume 範囲** を 0.5 にします。 クリックして **で選択した部分を作成する** 、isovolume を作成します。

7.  色を設定する **Iso_volume_part** 前の手順で作成します。 たとえば濃い水色に設定します。

8.  色を設定する **壁**します。 たとえば透明白色に設定します。

9. クリックして **再生** シミュレーションの結果を確認します。

    ![Tank result][tank_result]

## サンプル ファイル


### サンプル cred.xml ファイル

```
<ExtendedData>
  <PrivateKey>-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAxJKBABhnOsE9eneGHvsjdoXKooHUxpTHI1JVunAJkVmFy8JC
qFt1pV98QCtKEHTC6kQ7tj1UT2N6nx1EY9BBHpZacnXmknpKdX4Nu0cNlSphLpru
lscKPR3XVzkTwEF00OMiNJVknq8qXJF1T3lYx3rW5EnItn6C3nQm3gQPXP0ckYCF
Jdtu/6SSgzV9kaapctLGPNp1Vjf9KeDQMrJXsQNHxnQcfiICp21NiUCiXosDqJrR
AfzePdl0XwsNngouy8t0fPlNSngZvsx+kPGh/AKakKIYS0cO9W3FmdYNW8Xehzkc
VzrtJhU8x21hXGfSC7V0ZeD7dMeTL3tQCVxCmwIDAQABAoIBAQCve8Jh3Wc6koxZ
qh43xicwhdwSGyliZisoozYZDC/ebDb/Ydq0BYIPMiDwADVMX5AqJuPPmwyLGtm6
9hu5p46aycrQ5+QA299g6DlF+PZtNbowKuvX+rRvPxagrTmupkCswjglDUEYUHPW
05wQaNoSqtzwS9Y85M/b24FfLeyxK0n8zjKFErJaHdhVxI6cxw7RdVlSmM9UHmah
wTkW8HkblbOArilAHi6SlRTNZG4gTGeDzPb7fYZo3hzJyLbcaNfJscUuqnAJ+6pT
iY6NNp1E8PQgjvHe21yv3DRoVRM4egqQvNZgUbYAMUgr30T1UoxnUXwk2vqJMfg2
Nzw0ESGRAoGBAPkfXjjGfc4HryqPkdx0kjXs0bXC3js2g4IXItK9YUFeZzf+476y
OTMQg/8DUbqd5rLv7PITIAqpGs39pkfnyohPjOe2zZzeoyaXurYIPV98hhH880uH
ZUhOxJYnlqHGxGT7p2PmmnAlmY4TSJrp12VnuiQVVVsXWOGPqHx4S4f9AoGBAMn/
vuea7hsCgwIE25MJJ55FYCJodLkioQy6aGP4NgB89Azzg527WsQ6H5xhgVMKHWyu
Q1snp+q8LyzD0i1veEvWb8EYifsMyTIPXOUTwZgzaTTCeJNHdc4gw1U22vd7OBYy
nZCU7Tn8Pe6eIMNztnVduiv+2QHuiNPgN7M73/x3AoGBAOL0IcmFgy0EsR8MBq0Z
ge4gnniBXCYDptEINNBaeVStJUnNKzwab6PGwwm6w2VI3thbXbi3lbRAlMve7fKK
B2ghWNPsJOtppKbPCek2Hnt0HUwb7qX7Zlj2cX/99uvRAjChVsDbYA0VJAxcIwQG
TxXx5pFi4g0HexCa6LrkeKMdAoGAcvRIACX7OwPC6nM5QgQDt95jRzGKu5EpdcTf
g4TNtplliblLPYhRrzokoyoaHteyxxak3ktDFCLj9eW6xoCZRQ9Tqd/9JhGwrfxw
MS19DtCzHoNNewM/135tqyD8m7pTwM4tPQqDtmwGErWKj7BaNZARUlhFxwOoemsv
R6DbZyECgYEAhjL2N3Pc+WW+8x2bbIBN3rJcMjBBIivB62AwgYZnA2D5wk5o0DKD
eesGSKS5l22ZMXJNShgzPKmv3HpH22CSVpO0sNZ6R+iG8a3oq4QkU61MT1CfGoMI
a8lxTKnZCsRXU1HexqZs+DSc+30tz50bNqLdido/l5B4EJnQP03ciO0=
-----END RSA PRIVATE KEY-----</PrivateKey>
  <PublicKey>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDEkoEAGGc6wT16d4Ye+yN2hcqigdTGlMcjUlW6cAmRWYXLwkKoW3WlX3xAK0oQdMLqRDu2PVRPY3qfHURj0EEellpydeaSekp1fg27Rw2VKmEumu6Wxwo9HddXORPAQXTQ4yI0lWSerypckXVPeVjHetbkSci2foLedCbeBA9c/RyRgIUl227/pJKDNX2Rpqly0sY82nVWN/0p4NAyslexA0fGdBx+IgKnbU2JQKJeiwOomtEB/N492XRfCw2eCi7Ly3R8+U1KeBm+zH6Q8aH8ApqQohhLRw71bcWZ1g1bxd6HORxXOu0mFTzHbWFcZ9ILtXRl4Pt0x5Mve1AJXEKb username@servername;</PublicKey>
</ExtendedData>
```
### サンプル silent.cfg ファイル

```
# Patterns used to check silent configuration file
#
# anythingpat - any string
# filepat     - the file location pattern (/file/location/to/license.lic)
# lspat       - the license server address pattern (0123@hostname)
# snpat       - the serial number pattern (ABCD-01234567)

# accept EULA, valid values are: {accept, decline}
ACCEPT_EULA=accept

# optional error behavior, valid values are: {yes, no}
CONTINUE_WITH_OPTIONAL_ERROR=yes

# install location, valid values are: {/opt/intel, filepat}
PSET_INSTALL_DIR=/opt/intel

# continue with overwrite of existing installation directory, valid values are: {yes, no}
CONTINUE_WITH_INSTALLDIR_OVERWRITE=yes

# list of components to install, valid values are: {ALL, DEFAULTS, anythingpat}
COMPONENTS=DEFAULTS

# installation mode, valid values are: {install, modify, repair, uninstall}
PSET_MODE=install

# directory for non-RPM database, valid values are: {filepat}
#NONRPM_DB_DIR=filepat

# Serial number, valid values are: {snpat}
#ACTIVATION_SERIAL_NUMBER=snpat

# License file or license server, valid values are: {lspat, filepat}
#ACTIVATION_LICENSE_FILE=

# Activation type, valid values are: {exist_lic, license_server, license_file, trial_lic, serial_number}
ACTIVATION_TYPE=trial_lic

# Path to the cluster description file, valid values are: {filepat}
#CLUSTER_INSTALL_MACHINES_FILE=filepat

# Intel(R) Software Improvement Program opt-in, valid values are: {yes, no}
PHONEHOME_SEND_USAGE_DATA=no

# Perform validation of digital signatures of RPM files, valid values are: {yes, no}
SIGNING_ENABLED=yes

# Select yes to enable mpi-selector integration, valid values are: {yes, no}
ENVIRONMENT_REG_MPI_ENV=no

# Select yes to update ld.so.conf, valid values are: {yes, no}
ENVIRONMENT_LD_SO_CONF=no

```

### サンプル settings.sh スクリプト

```
#!/bin/bash

# impi
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# openfoam
export FOAM_INST_DIR=/opt/OpenFOAM
source /opt/OpenFOAM/OpenFOAM-2.3.1/etc/bashrc
export WM_MPLIB=INTELMPI
```


###サンプル hpcimpirun.sh スクリプト

```
#!/bin/bash

# The path of this script
SCRIPT_PATH="$( dirname "${BASH_SOURCE[0]}" )"

# Set mpirun runtime evironment
source /opt/intel/impi/5.0.3.048/bin64/mpivars.sh
export MPI_ROOT=$I_MPI_ROOT
export I_MPI_FABRICS=shm:dapl
export I_MPI_DAPL_PROVIDER=ofa-v2-ib0
export I_MPI_DYNAMIC_CONNECTION=0

# mpirun command
MPIRUN=mpirun
# Argument of "--hostfile"
NODELIST_OPT="--hostfile"
# Argument of "-np"
NUMPROCESS_OPT="-np"

# Get node information from ENVs
NODESCORES=(${CCP_NODES_CORES})
COUNT=${#NODESCORES[@]}

if [ ${COUNT} -eq 0 ]
then
    # CCP_NODES_CORES is not found or is empty, just run the mpirun without hostfile arg.
    ${MPIRUN} $*
else
    # Create the hostfile file
    NODELIST_PATH=${SCRIPT_PATH}/hostfile_$$

    # Get every node name and write into the hostfile file
    I=1
    while [ ${I} -lt ${COUNT} ]
    do
        echo "${NODESCORES[${I}]}" >> ${NODELIST_PATH}
        let "I=${I}+2"
    done

    # Run the mpirun with hostfile arg
    ${MPIRUN} ${NUMPROCESS_OPT} ${CCP_NUMCPUS} ${NODELIST_OPT} ${NODELIST_PATH} $*

    RTNSTS=$?
    rm -f ${NODELIST_PATH}
fi

exit ${RTNSTS}

```





<!--Image references-->
[keygen]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/keygen.png
[keys]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/keys.png
[step_variables]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/step_variables.png
[data_files]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/data_files.png
[decompose]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/decompose.png
[job_details]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/job_details.png
[job_resources]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/job_resources.png
[task_details1]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/task_details1.png
[task_dependencies]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/task_dependencies.png
[creds]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/creds.png
[heat_map]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/heat_map.png
[tank]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/tank.png
[tank_result]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/tank_result.png
[isosurface]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/isosurface.png
[isosurface_color]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/isosurface_color.png
[linux_processes]: ./media/virtual-machines-linux-cluster-hpcpack-openfoam/linux_processes.png


