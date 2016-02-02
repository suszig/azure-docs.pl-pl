<properties
 pageTitle="HPC Pack クラスター内の Linux コンピューティング VM | Microsoft Azure"
 description="Windows Server を実行するヘッド ノードと Linux 計算ノードを含む HPC Pack クラスターを Azure にデプロイするスクリプトを作成する方法について説明します。"
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
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="12/02/2015"
 ms.author="danlep"/>


# Azure の HPC Pack クラスターで Linux コンピューティング ノードの使用を開始する

この記事では、Azure PowerShell スクリプトを使用し、Windows Server を実行するヘッド ノードと Linux ディストリビューションを実行するコンピューティング ノードを含む Microsoft HPC Pack クラスターを Azure に設定する方法を紹介します。 Linux コンピューティング ノードにデータ ファイルを移動する方法もいくつか紹介します。 このクラスターを使用し、Azure で Linux HPC ワークロードを実行できます。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


これから作成する HPC Pack クラスターの概要は次の図のようになります。

![Linux ノードを含む HPC クラスター][scenario]

## Linux コンピューティング ノードと共に HPC Pack クラスターをデプロイします。

Microsoft HPC Pack IaaS デプロイ スクリプト (**New-HpcIaaSCluster.ps1**) を使用して、Azure インフラストラクチャ サービス (IaaS) でクラスターのデプロイを自動化します。 この Azure PowerShell スクリプトは迅速なプロイメントのために Azure Marketplace の HPC Pack VM イメージを使用します。包括的な構成パラメーターを備え、簡単かつ柔軟なデプロイを可能にします。 スクリプトは、Azure の仮想ネットワークを展開、ストレージ アカウント、クラウド サービス、ドメイン コント ローラー、任意の個別の SQL Server データベース サーバー、クラスターのヘッド ノード、計算ノード、ブローカー ノード、Azure PaaS (「バースト」) ノード、および Linux 計算ノード (で導入された Linux サポート [HPC Pack 2012 R2 Update 2](https://technet.microsoft.com/library/mt269417.aspx))します。

HPC Pack クラスター デプロイメント オプションの詳細については、「、 [HPC Pack 2012 R2 と HPC Pack 2012 のための方法の概要ガイド](https://technet.microsoft.com/library/jj884144.aspx) と [の Azure with Microsoft HPC Pack クラスター オプションを作成し、、高パフォーマンス コンピューティング (HPC) の管理](virtual-machines-hpcpack-cluster-options.md)します。

### 前提条件

* **クライアント コンピューター** - クラスター デプロイ スクリプトを実行するには、Windows ベースのクライアント コンピューターが必要です。

* **Azure PowerShell** - [をインストールし、Azure PowerShell を構成](../powershell-install-configure.md) (バージョン 0.8.10 以降)、クライアント コンピューターにします。

* **HPC Pack IaaS デプロイメント スクリプト** - をダウンロードしてから、スクリプトの最新バージョンを開梱、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=44949)します。 実行して、スクリプトのバージョンをチェックする `New-hpciaascluster.ps1 – バージョン`します。 この記事はバージョン 4.4.0 以降のスクリプトに基づきます。

* **Azure サブスクリプション** - Azure Global または Azure China サービスのサブスクリプションを利用できます。 アカウントがない場合は、無料試用版アカウントを数分で作成することができます。 詳細については、「 [Azure 無料試用版](http://azure.microsoft.com/pricing/free-trial/)します。

* **コア クォータ** - 場合によっては、コアのクォータを増やす必要があります。特に、マルチコア VM サイズのクラスター ノードをいくつかデプロイする場合に必要となる可能性があります。 この記事の例では、使用可能なコアが少なくとも 12 個必要です。 クォータを増やすには [オンライン カスタマー サポートに申請](http://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) 無償でします。

### 構成ファイルを作成する

HPC Pack IaaS デプロイ スクリプトは HPC クラスターのインフラストラクチャについて記載された XML 構成ファイルを入力として使用します。 1 つのヘッド ノードと 2 つの Linux コンピューティング ノードから構成される小規模のクラスターをデプロイするには、ご利用の環境の値を次のサンプル構成ファイルに代入します。 詳細については、構成ファイル、スクリプト フォルダーにある Manual.rtf ファイルを参照してくださいと [HPC Pack IaaS デプロイメント スクリプトで HPC クラスターを作成](virtual-machines-hpcpack-cluster-powershell-script.md)します。

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>allvhdsje</StorageAccount>
  </Subscription>
  <Location>Japan East</Location>  
  <VNet>
    <VNetName>centos7rdmavnetje</VNetName>
    <SubnetName>CentOS7RDMACluster</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>CentOS7RDMA-HN</VMName>
    <ServiceName>centos7rdma-je</ServiceName>
  <VMSize>A4</VMSize>
  <EnableRESTAPI />
  <EnableWebPortal />
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>CentOS7RDMA-LN%1%</VMNamePattern>
    <ServiceName>centos7rdma-je</ServiceName>
    <VMSize>A7</VMSize>
    <NodeCount>2</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325</ImageName>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```

次は構成ファイルの構成要素の簡単な説明です。

* **IaaSClusterConfig** - 構成ファイルのルート要素です。

* **サブスクリプション** - HPC Pack クラスターのデプロイに使用される Azure サブスクリプションです。 下のコマンドを使用し、Azure サブスクリプションの名前を設定し、それがクライアント コンピューターで一意になるようにします。 このサンプルでは、Azure サブスクリプションの「Subscription-1」を使用します。

    ```
    PS > Get-AzureSubscription –SubscriptionName <SubscriptionName>
    ```

    >[AZURE.NOTE]または、サブスクリプション ID を使用し、使用するサブスクリプションを指定できます。 スクリプト フォルダーにある Manual.rtf ファイルを参照します。

* **StorageAccount** - HPC Pack クラスターの永続的なデータはすべて、指定のストレージ アカウント (この例では allvhdsje) に保存されます。 ストレージ アカウントがない場合、このスクリプトによって、**場所**に指定されているリージョンにストレージ アカウントが作成されます。

* **場所** - HPC Pack クラスターをデプロイする Azure リージョン (この例では東日本) です。

* **VNet** - HPC クラスターが作成される仮想ネットワークとサブネットの設定です。 このスクリプトを実行する前に仮想ネットワークとサブネットを自分で作成できます。あるいは、このスクリプトによりアドレス空間が「192.168.0.0/20」の仮想ネットワークと「192.168.0.0/23」のサブネットが作成されます。 この例では、このスクリプトにより、仮想ネットワーク centos7rdmavnetje とサブネット CentOS7RDMACluster が作成されます。

* **ドメイン** - HPC Pack クラスターの Active Directory ドメイン設定です。 このスクリプトで作成された Windows VM はすべてこのドメインに参加します。 現在のところ、このスクリプトでは、ExistingDC、NewDC、HeadNodeAsDC の 3 つのドメインを選択できます。 この例では、完全修飾ドメイン名が hpc.local のドメイン コントローラーとしてヘッド ノードを構成します。

* **データベース** - HPC Pack クラスターのデータベース設定です。 現在のところ、このスクリプトでは、ExistingDB、NewRemoteDB、LocalDB の 3 つのデータベースを選択できます。 この例では、ヘッド ノードでローカル データベースを作成します。

* **HeadNode** - HPC Pack ヘッド ノードの設定です。 この例では、クラウド サービス centos7rdma-je で「CentOS7RDMA-HN」という名前の A7 ヘッド ノードを作成します。 リモート (ドメイン不参加) クライアント コンピューターから HPC ジョブを送信するために、このスクリプトにより HPC ジョブ スケジューラ REST API と HPC Web ポータルが有効になります。

* **LinuxComputeNodes** - HPC Pack Linux コンピューティング ノードの設定です。 この例では、2 つのサイズの A7 CentOS 7 Linux コンピューティング ノード (CentOS7RDMA-LN1 と CentOS7RDMA-LN2) をクラウド サービス centos7rdma-je で作成します。

### Linux コンピューティング ノードの追加の考慮事項

* 現在のところ、HPC Pack ではコンピューティング ノードの Linux ディストリビューションとして Ubuntu Server 14.10、CentOS 6.6、CentOS 7.0、SUSE Linux Enterprise Server 12 に対応しています。

* この記事の例では Azure Marketplace で入手できる特定の CentOS バージョンを使用してクラスターを作成します。 使用可能なその他のイメージを使用する場合を使用して、 **get azurevmimage** Azure PowerShell コマンドレットを見つける必要があります。 たとえば、すべての CentOS 7.0 イメージを一覧表示するには、次のコマンドを実行します。

    ```
    get-azurevmimage | ?{$_.Label -eq "OpenLogic 7.0"}
    ```

    必要なイメージを見つけ、構成ファイルの **ImageName** 値を置き換えます。

* サイズ A8 と A9 の VM の RDMA 接続をサポートする Linux イメージを利用できます。 Linux RDMA ドライバーがインストールされ、有効になっているイメージを指定した場合、HPC Pack IaaS デプロイ スクリプトによりドライバーがデプロイされます。 たとえば、イメージの名前を指定 `b4590d9e3ed742e4a1d46e5424aa335e__suse-sles-12-hpc-v20150708` ために、現在の SUSE Linux Enterprise Server 12 – Marketplace のハイパフォーマンス コンピューティング イメージを最適化します。

* サポートされているイメージから作成した Linux VM で Linux RDMA を有効にして MPI ジョブを実行するには、アプリケーション ニーズに基づいたクラスターのデプロイの後に、Linux ノードに特定の MPI ライブラリをインストールし、構成します。 例については、次を参照してください。 [Azure のクラスターの Linux RDMA 上の Microsoft HPC Pack での実行の OpenFOAM](virtual-machines-linux-cluster-hpcpack-openfoam.md)します。

* RDMA ネットワーク接続がノード間で機能するように、必ず 1 つのサービス内にすべての Linux RDMA ノードをデプロイします。


## HPC Pack IaaS デプロイ スクリプトを実行する

1. 管理者としてクライアント コンピューターで PowerShell を開きます。

2. ディレクトリをスクリプト フォルダー (この例では、E:\IaaSClusterScript) に変更します。

    ```
    cd E:\IaaSClusterScript
    ```

3. 下のコマンドを実行し、HPC Pack クラスターをデプロイします。 この例では、構成ファイルは E:\HPCDemoConfig.xml にあるものと想定しています。

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

    スクリプトはため自動的にログ ファイルを生成、 **- ログ ファイル** パラメーターが指定されていません。 ログはリアルタイムで書き込まれませんが、検証とデプロイの終わりに回収されます。そのため、スクリプトの実行中に PowerShell プロセスが停止した場合、一部のログが失われます。

    a. 上記のコマンドには **AdminPassword** が指定されていないため、ユーザー *MyAdminName* のパスワードを入力するように促されます。

    b. スクリプトにより構成ファイルの検証が開始されます。 ネットワーク接続の状態にもよりますが、数秒から数分かかります。

    ![検証][validate]

    c. 検証に合格すると、HPC クラスターに作成されるリソースが一覧表示されます。 「*Y*」と入力して続行します。

    ![リソース][resources]

    d. スクリプトにより HPC Pack クラスターのデプロイが開始され、ユーザーの介入なしで構成が完了します。 これには数分かかる場合があります。

    ![デプロイする][deploy]

4. 構成が完了したら、ヘッド ノードにリモート デスクトップ接続し、HPC Cluster Manager を開き、HPC Pack クラスターの状態を確認します。 Windows コンピューティング ノードの場合と同じように Linux コンピューティング ノードを管理し、監視できます。 たとえば、ノード管理に Linux ノードが一覧表示されます。

    ![ノード管理][management]

    ヒート マップ ビューにも Linux ノードが表示されます。

    ![ヒート マップ][heatmap]

## Linux ノードのあるクラスターにデータを移動する方法

クラスターの Linux ノードと Windows ヘッド ノードの間でデータを移動するにはいくつかの方法があります。 次は 3 つの一般的な方法です。

* **Azure ファイル** - Azure Storage にデータ ファイルを格納するファイル共有を公開します。 デプロイされている仮想ネットワークが異なる場合でも、Windows ノードと Linux ノードの両方でドライブまたはフォルダーとして同時に Azure File 共有をマウントできます。

* **ヘッド ノード SMB 共有** - Linux ノードにヘッド ノードの共有フォルダーをマウントします。

* **ヘッド ノード NFS サーバー**  -Windows と Linux の混在環境のファイル共有ソリューションを提供します。

### Azure File ストレージ

[Azure File](https://azure.microsoft.com/services/storage/files/) サービスは標準の SMB 2.1 プロトコルを使用してファイル共有を公開します。 Azure の VM とクラウド サービスでは、マウントされている共有を介して、アプリケーション コンポーネント間でファイル データを共有できます。また、オンプレミスのアプリケーションでは、File ストレージ API を介して、共有内のファイル データにアクセスできます。 詳細については、次を参照してください。 [PowerShell と .NET で Azure File ストレージを使用する方法](../storage/storage-dotnet-how-to-use-files.md)します。

Azure File 共有を作成するには、詳細な手順を参照してください。 [Microsoft Azure ファイル サービスの概要](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/12/introducing-microsoft-azure-file-service.aspx)します。 固定接続を設定するを参照してください。 [Microsoft Azure Files への接続の維持](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/05/27/persisting-connections-to-microsoft-azure-files.aspx)します。

この例では、ストレージ アカウント allvhdsje に「rdma」という名前の Azure File 共有を作成します。 ヘッド ノードに共有をマウントするには、コマンド ウィンドウを開き、次のコマンドを入力します。

```
> cmdkey /add:allvhdsje.file.core.windows.net /user:allvhdsje /pass:<storageaccountkey>
> net use Z: \\allvhdje.file.core.windows.net\rdma /persistent:yes
```

この例では、allvhdsje はストレージ アカウントの名前、storageaccountkey はストレージ アカウントのキー、rdma は Azure File 共有名になります。 Azure File 共有はヘッド ノードの Z: にマウントされます。

Linux ノードに Azure File 共有をマウントするには、ヘッド ノードで **clusrun** コマンドを実行します。 * *[Clusrun](https://technet.microsoft.com/library/cc947685.aspx)* * 複数のノードで管理タスクを実行する便利な HPC Pack ツールです。 (「」も参照 [Linux ノードの CLusrun](#CLusrun-for-Linux-nodes) この記事の内容です)。

Windows PowerShell ウィンドウを開き、次のコマンドを入力します。

```
PS > clusrun /nodegroup:LinuxNodes mkdir -p /rdma

PS > clusrun /nodegroup:LinuxNodes mount -t cifs //allvhdsje.file.core.windows.net/rdma /rdma -o vers=2.1`,username=allvhdsje`,password=<storageaccountkey>'`,dir_mode=0777`,file_mode=0777
```

最初のコマンドで「/rdma」という名前のフォルダーが LinuxNodes グループのすべてのノードで作成されます。 2 つ目のコマンドにより、ディレクトリとファイル モードのビットが「777」に設定された「/rdma」フォルダーに Azure File 共有 allvhdsjw.file.core.windows.net/rdma がマウントされます。 2 番目のコマンドでは、allvhdsje はストレージ アカウント名と storageaccountkey はストレージ アカウント キー。
>[AZURE.NOTE]"\'"2 番目のコマンドで記号は PowerShell のエスケープ記号です。 "\'、"、「,」(コンマ) ことを意味のコマンドの一部であります。

### ヘッド ノード共有

または、Linux ノードにヘッド ノードの共有フォルダーをマウントできます。 これはファイルを共有する最も簡単な方法です。ただし、ヘッド ノードとすべての Linux ノードを同じ仮想ネットワークにデプロイする必要があります。 手順は次のようになります。

1. ヘッド ノードでフォルダーを作成し、読み書き権限を持つ「全員」と共有します。 たとえば、ヘッド ノードで D:\OpenFOAM を \\CentOS7RDMA-HN\OpenFOAM として共有します。 ここでは CentOS7RDMA-HN がヘッド ノードのホスト名です。

    ![ファイル共有のアクセス許可][fileshareperms]

    ![ファイル共有][filesharing]

2. Windows PowerShell ウィンドウを開き、次のコマンドを実行し、共有フォルダーをマウントします。

```
PS > clusrun /nodegroup:LinuxNodes mkdir -p /openfoam

PS > clusrun /nodegroup:LinuxNodes mount -t cifs //CentOS7RDMA-HN/OpenFOAM /openfoam -o vers=2.1`,username=<username>`,password='<password>'`,dir_mode=0777`,file_mode=0777
```

最初のコマンドで「/openfoam」という名前のフォルダーが LinuxNodes グループのすべてのノードで作成されます。 2 つ目のコマンドにより、ディレクトリとファイル モードのビットが「777」に設定されたフォルダーに共有フォルダー //CentOS7RDMA-HN/OpenFOAM がマウントされます。 コマンド内のユーザー名とパスワードは、ヘッド ノードのクラスター ユーザーのユーザー名とパスワードにする必要があります。
>[AZURE.NOTE]"\'"2 番目のコマンドで記号は PowerShell のエスケープ記号です。 "\'、"、「,」(コンマ) ことを意味のコマンドの一部であります。


### NFS サーバー

NFS サービスでは、SMB プロトコルを利用して Windows Server 2012 オペレーティング システムを実行するコンピューターと NFS プロトコルを利用する Linux ベースのコンピューターの間でファイルを共有し、移行できます。 NFS サーバーとその他のすべてのノードを同じ仮想ネットワークにデプロイする必要があります。 SMB 共有と比較すると、Linux との互換性が良くなります。たとえば、ファイル リンクがサポートされます。

1. インストールして、NFS サーバーをセットアップ、」の手順に従います [ネットワーク File System First Share エンド ツー エンドのサーバー](http://blogs.technet.com/b/filecab/archive/2012/10/08/server-for-network-file-system-first-share-end-to-end.aspx)します。

    たとえば、次のプロパティを持つ「nfs」という名前の NFS 共有を作成します。

    ![NFS 認証][nfsauth]

    ![NFS 共有のアクセス許可][nfsshare]

    ![NFS NTFS アクセス許可][nfsperm]

    ![NFS 管理のプロパティ][nfsmanage]

2. Windows PowerShell ウィンドウを開き、次のコマンドを実行して NFS 共有をマウントします。

  ```
  PS > clusrun /nodegroup:LinuxNodes mkdir -p /nfsshare
  PS > clusrun /nodegroup:LinuxNodes mount CentOS7RDMA-HN:/nfs /nfsshared
  ```

  最初のコマンドで「/nfsshared」という名前のフォルダーが LinuxNodes グループのすべてのノードで作成されます。 2 つ目のコマンドにより、NFS 共有 CentOS7RDMA-HN:/nfs がフォルダーにマウントされます。 ここで、CentOS7RDMA-HN:/nfs は NFS 共有のリモート パスです。

## ジョブの送信方法

いくつかの方法で HPC Pack クラスターにジョブを送信できます。

* HPC Cluster Manager または HPC Job Manager GUI

* HPC Web ポータル

* REST API

HPC Pack GUI ツールと HPC Web ポータル経由で Azure のクラスターにジョブを送信する方法は Windows コンピューティング ノードの場合と同じです。 参照してください [HPC ジョブ マネージャー](https://technet.microsoft.com/library/ff919691.aspx) と [からのジョブと内部設置型のクライアントを送信する方法](virtual-machines-hpcpack-cluster-submit-jobs.md)します。

REST API を使用してジョブを送信するを参照してください [Creating and Submitting Jobs Microsoft HPC Pack で REST API を使用して、](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx)します。 Python サンプルを参照しても、 [HPC Pack SDK](https://www.microsoft.com/download/details.aspx?id=47756) Linux クライアントからジョブを送信します。

## Linux ノードの clusrun

HPC Pack **clusrun** ツールを使用して、コマンド プロンプトまたは HPC クラスター マネージャーを介して Linux ノードでコマンドを実行できます。 基本的な例の一部を次に示します。

* クラスター内のすべてのノードの現在のユーザー名を表示します。

    ```
    > clusrun whoami
    ```

* linuxnodes グループ内のすべてのノードに **gdb** デバッガー ツールを **yum** でインストールし、10 分後にノードを再起動します。

    ```
    > clusrun /nodegroup:linuxnodes yum install gdb –y; shutdown –r 10
    ```

* クラスター内の各 Linux ノードに 1 ～ 10 の番号を 1 秒間表示するシェル スクリプトを作成して実行すると、ノードからの出力がすぐに表示されます。

    ```
    > clusrun /interleaved /nodegroup:linuxnodes echo \"for i in {1..10}; do echo \\\"\$i\\\"; sleep 1; done\" ^> script.sh; chmod +x script.sh; ./script.sh
    ```

>[AZURE.NOTE] 場合によっては、**clusrun** コマンドで特定のエスケープ文字を使用する必要があります。 この例に示すように、コマンド ウィンドウで ^ を使用すると、">" 記号がエスケープされます。

## 次のステップ

* ノード数を増やしてクラスターをスケール アップするか、クラスター上で Linux のワークロードの実行を試行します。 例については、次を参照してください。 [Linux での Microsoft HPC Pack での実行の NAMD の計算ノードとして Azure](virtual-machines-linux-cluster-hpcpack-namd.md)します。

* サイズを使用するクラスターを実行してください [A8 または A9](virtual-machines-a8-a9-a10-a11-specs.md) 計算ノード MPI ワークロードを実行します。 例については、次を参照してください。 [Azure のクラスターの Linux RDMA 上の Microsoft HPC Pack での実行の OpenFOAM](virtual-machines-linux-cluster-hpcpack-openfoam.md)します。

* 実行してください、 [Azure クイック スタート テンプレート](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/) より多くの Linux の HPC パックの展開を高速化する Azure リソース マネージャーでのコンピューティング ノードです。



[scenario]: ./media/virtual-machines-linux-cluster-hpcpack/scenario.png 
[validate]: ./media/virtual-machines-linux-cluster-hpcpack/validate.png 
[resources]: ./media/virtual-machines-linux-cluster-hpcpack/resources.png 
[deploy]: ./media/virtual-machines-linux-cluster-hpcpack/deploy.png 
[management]: ./media/virtual-machines-linux-cluster-hpcpack/management.png 
[heatmap]: ./media/virtual-machines-linux-cluster-hpcpack/heatmap.png 
[fileshareperms]: ./media/virtual-machines-linux-cluster-hpcpack/fileshare1.png 
[filesharing]: ./media/virtual-machines-linux-cluster-hpcpack/fileshare2.png 
[nfsauth]: ./media/virtual-machines-linux-cluster-hpcpack/nfsauth.png 
[nfsshare]: ./media/virtual-machines-linux-cluster-hpcpack/nfsshare.png 
[nfsperm]: ./media/virtual-machines-linux-cluster-hpcpack/nfsperm.png 
[nfsmanage]: ./media/virtual-machines-linux-cluster-hpcpack/nfsmanage.png 

