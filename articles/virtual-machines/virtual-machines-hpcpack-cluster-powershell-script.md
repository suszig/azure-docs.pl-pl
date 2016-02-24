<properties
   pageTitle="PowerShell スクリプトで HPC Pack クラスターをデプロイする | Microsoft Azure"
   description="Windows PowerShell スクリプトを実行し、Azure インフラストラクチャ サービスで完全な HPC Pack クラスターをデプロイする"
   services="virtual-machines"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management,hpc-pack"/>
<tags
   ms.service="virtual-machines"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-multiple"
   ms.workload="big-compute"
   ms.date="09/29/2015"
   ms.author="danlep"/>

# HPC Pack IaaS デプロイ スクリプトを使用し、Azure VM でハイ パフォーマンス コンピューティング (HPC) クラスターを作成する

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。



クライアントでの HPC Pack IaaS デプロイメント PowerShell スクリプトの実行します。
Azure インフラストラクチャの完全な HPC Pack クラスターを展開するコンピューター
サービス (IaaS)。 スクリプトは、いくつかの展開オプションを提供し、サポートされる Linux を実行しているクラスター計算ノードを追加できます。
ディストリビューションまたは Windows Server オペレーティング システムです。

環境や選択肢によっては、このスクリプトにより、Azure virtual network、ストレージ アカウント、クラウド サービス、ドメイン コントローラー、リモートまたはローカルの SQL データベース、ヘッド ノード、ブローカー ノード、コンピューティング ノード、Azure クラウド サービス (「バースト」または PaaS) ノードを含む、すべてのクラスター インフラストラクチャを作成できます。 あるいは、このスクリプトにより既存の Azure インフラストラクチャを利用し、HPC クラスター ヘッド ノード、ブローカー ノード、コンピューティング ノード、Azure バースト ノードを作成できます。


HPC Pack クラスターの計画に関する背景情報を参照してください、 [製品の評価と計画](https://technet.microsoft.com/library/jj899596.aspx) と [作業の開始](https://technet.microsoft.com/library/jj899590.aspx) HPC Pack は、TechNet ライブラリのコンテンツ。

>[AZURE.NOTE]また、HPC Pack クラスターをデプロイするのに Azure リソース マネージャー テンプレートを使用することができます。 例については、次を参照してください。 [HPC クラスターを作成](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/), 、[、HPC クラスターを作成、カスタムの計算ノード イメージ](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-custom-image/), 、または [Linux 計算ノードと共に HPC クラスターを作成する](https://azure.microsoft.com/documentation/templates/create-hpc-cluster-linux-cn/)です。

## 前提条件

* **Azure サブスクリプション** -Azure Global または Azure China service でサブスクリプションを使用することができます。 ご利用のサブスクリプションの制限によりデプロイ可能なクラスター ノードの数と種類が変わります。 詳細については、次を参照してください。 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../azure-subscription-service-limits.md)します。


* **Windows クライアント コンピューターと Azure PowerShell 0.8.7 以降がインストールおよび構成されている** -を参照してください [をインストールして、Azure PowerShell を構成する方法](../powershell-install-configure.md)します。 このスクリプトは Azure サービス管理で実行されます。


* **HPC Pack IaaS デプロイメント スクリプト** - をダウンロードしてから、スクリプトの最新バージョンを開梱、 [Microsoft ダウンロード センター](https://www.microsoft.com/download/details.aspx?id=44949)します。 `New-HPCIaaSCluster.ps1 –Version` を実行すると、スクリプトのバージョンを確認できます。 この記事はバージョン 4.4.0 のスクリプトに基づきます。

* **スクリプトの構成ファイル** -HPC クラスターを構成するスクリプトで使用される XML ファイルを作成する必要があります。 情報とサンプルについては、この記事の後半のセクションを参照してください。


## 構文

```
New-HPCIaaSCluster.ps1 [-ConfigFile] <String> [-AdminUserName]<String> [[-AdminPassword] <String>] [[-HPCImageName] <String>] [[-LogFile] <String>] [-Force] [-NoCleanOnFailure] [-PSSessionSkipCACheck] [<CommonParameters>]
```
>[AZURE.NOTE]管理者として、スクリプトを実行する必要があります。

### パラメーター

* **ConfigFile** -HPC クラスターを記述する構成ファイルのファイル パスを指定します。 詳細については、次を参照してください。 [構成ファイル](#Configuration-file) このトピックでは、またはある Manual.rtf ファイル、フォルダーにスクリプトを含みます。

* **AdminUserName** -ユーザー名を指定します。 ドメイン フォレストがこのスクリプトにより作成される場合、これがすべての VM のローカル管理者ユーザー名となり、また、ドメイン管理者名となります。 ドメイン フォレストが既に存在する場合、HPC Pack をインストールするために、ドメイン ユーザーがローカル管理者ユーザー名として指定されます。

* **AdminPassword** -管理者のパスワードを指定します。 コマンド ラインで指定されない場合、パスワードを入力するように求められます。

* **HPCImageName** (省略可能) - HPC Pack VM を指定する HPC クラスターをデプロイするために使用するイメージの名前。 これは Azure Marketplace から Microsoft が提供する HPC Pack イメージにする必要があります。 指定されていない場合 (ほとんどの場合に推奨)、最近公開された HPC Pack イメージが選択されます。

    >[AZURE.NOTE] HPC Pack の有効なイメージを指定しない場合は、展開は失敗します。

* **ログ ファイル** (省略可能) - 展開のログ ファイルのパスを指定します。 指定しない場合、スクリプトを実行しているコンピューターの一時ディレクトリにログ ファイルが作成されます。

* **Force** (省略可能) - すべての確認プロンプトを表示しないようにします。

* **NoCleanOnFailure** (省略可能) - 正常に展開していない Azure Vm が削除されなかったことを指定します。 デプロイできなかった VM を最初に手動で削除してから、スクリプトを再実行し、デプロイを続行します。そうしないと、デプロイは失敗します。

* **PSSessionSkipCACheck** (省略可能) - このスクリプトによって展開されたバーチャル マシンのすべてのクラウド サービスの自己署名証明書が自動的に Azure によって生成し、クラウド サービス内のすべての Vm は、既定の Windows リモート管理 (WinRM) 証明書としてこの証明書を使用します。 HPC 機能を Azure Vm を展開するには、スクリプト既定で一時的にこれらの証明書でインストールをスクリプトの実行中に"信頼されていない CA"セキュリティ エラーを抑制するクライアント コンピューターのローカル Computer\\Trusted ルート証明機関ストアスクリプトが完了したら、証明書が削除されます。 このパラメーターが指定されている場合、証明書はクライアント コンピューターにインストールされず、セキュリティ警告が非表示になります。

    >[AZURE.IMPORTANT] 運用環境のデプロイメントでは、このパラメーターは推奨されません。

### 例

次の例は、HPC Pack クラスターを使用して新しいを作成します
構成ファイル MyConfigFile.xml と管理を指定します
クラスターをインストールするための資格情報。

```
New-HPCIaaSCluster.ps1 –ConfigFile MyConfigFile.xml -AdminUserName <username> –AdminPassword <password>
```

### 追加の考慮事項

* このスクリプトは Azure Marketplace の HPC Pack VM イメージを使用し、クラスター ヘッド ノードを作成します。 現在のイメージは HPC Pack 2012 R2 更新プログラム 3 をインストールした Windows Server 2012 R2 Datacenter に基づいています。

* このスクリプトでは、任意で、HPC Pack Web ポータルまたは HPC Pack REST API を通してジョブ送信を有効にできます。


* このスクリプトでは、追加ソフトウェアをインストールするか、その他の設定を構成する場合、カスタムの構成前スクリプトと構成後スクリプトを任意で実行できます。


## 構成ファイル

配置スクリプトの構成ファイルは XML
ファイルを保存します。 HPC Pack IaaS でスキーマ ファイル HPCIaaSClusterConfig.xsd は
スクリプトの展開フォルダ。 **IaaSClusterConfig** のルート要素は、
説明されている構成ファイルは、子要素が含まれています
デプロイ スクリプト フォルダーにある Manual.rtf ファイルの詳細です。 たとえば、さまざまなシナリオ用のファイルを参照してください。
[構成ファイルの例](#Example-configuration-files) この記事の内容。

## サンプル構成ファイル

### 例 1

次の構成ファイルでは、既存のドメイン フォレストで HPC Pack クラスターが展開されます。 このクラスターにはローカル データベースを持つヘッド ノードが 1 つあり、BGInfo VM 拡張機能が適用されたコンピューティング ノードが 12 あります。
内のすべての Vm の Windows 更新プログラムの自動インストールが無効になっています
ドメイン フォレストです。 すべてのクラウド サービス内で直接作成します
東アジアの場所です。 コンピューティング ノードは、3 つのクラウド サービスで作成されます。
3 つのストレージ アカウント (つまり、MyHPCCN-0001 で MyHPCCN 0005
MyHPCCNService01 と mycnstorage01 です。Myhpccn-0006 ~ MyHPCCN0010
Mycnstorage02 です。で MyHPCCN 0012
MyHPCCNService03 と mycnstorage03 に作成されます)。 コンピューティング ノードから作成されます。
コンピューティング ノードからキャプチャされた、既存のプライベート イメージ。 自動拡張します。
圧縮とサービスが既定で有効になっている拡大および縮小の間隔。

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
      </DomainController>
     <NoWindowsAutoUpdate />
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
  </Certificates>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0001%</VMNamePattern>
<ServiceNamePattern>MyHPCCNService%01%</ServiceNamePattern>
<MaxNodeCountPerService>5</MaxNodeCountPerService>
<StorageAccountNamePattern>mycnstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>12</NodeCount>
    <ImageName HPCPackInstalled=”true”>MyHPCComputeNodeImage</ImageName>
    <VMExtensions>
       <VMExtension>
          <ExtensionName>BGInfo</ExtensionName>
          <Publisher>Microsoft.Compute</Publisher>
          <Version>1.*</Version>
       </VMExtension>
    </VMExtensions>
  </ComputeNodes>
  <AutoGrowShrink>
    <CertificateId>1</CertificateId>
  </AutoGrowShrink>
</IaaSClusterConfig>

```

### 例 2

次の構成ファイルは、HPC Pack クラスターをデプロイします。
既存のドメイン フォレストです。 クラスターには、個のヘッド ノード 1 が含まれています。
Windows を実行する 2 つのブローカー ノード、500 GB のデータ ディスクでデータベース サーバー
Server 2012 R2 オペレーティング システム、および Windows を実行する 5 つの計算ノード
Server 2012 R2 オペレーティング システムです。 クラウド サービス MyHPCCNService は、します。
アフィニティ グループ MyIBAffinityGroup、およびその他のすべてのクラウドで作成されました。
サービスは、アフィニティ グループ MyAffinityGroup に作成されます。 HPC ジョブ
ヘッド ノードでは、scheduler REST API と HPC web ポータルが有効にします。

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>    
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>NewRemoteDB</DBOption>
    <DBVersion>SQLServer2014_Enterprise</DBVersion>
    <DBServer>
      <VMName>MyDBServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>ExtraLarge</VMSize>
      <DataDiskSizeInGB>500</DataDiskSizeInGB>
    </DBServer>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
    <EnableRESTAPI />
    <EnableWebPortal />
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%0000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>A8</VMSize>
<NodeCount>5</NodeCount>
<AffinityGroup>MyIBAffinityGroup</AffinityGroup>
  </ComputeNodes>
  <BrokerNodes>
    <VMNamePattern>MyHPCBN-%0000%</VMNamePattern>
    <ServiceName>MyHPCBNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>2</NodeCount>
  </BrokerNodes>
</IaaSClusterConfig>
```

### 例 3

次の構成ファイルには、新しいドメイン フォレストが作成されます。
展開 HPC Pack クラスターがローカルの個のヘッド ノードと
データベースと 20 Linux はコンピューティング ノードです。 すべてのクラウド サービスを作成します。
東アジア地域で直接。 Linux 計算ノードが作成されます。
4 つのクラウド サービスを 4 つのストレージ アカウント (つまり MyLnxCN-0001 内
MyLnxCNService01 と mylnxstorage01 に MyLnxCN 0006 MyHPCCN 0005
MyLnxCNService02 と mylnxstorage02 に MyLnxCN 0011 MyLnxCN 0010
MyLnxCNService03 および mylnxstorage03 とに MyLnxCN 0016 MyLnxCN 0015
MyLnxCNService04 と mylnxstorage04 MyLnxCN-0020)。 計算ノード
OpenLogic CentOS 7.0 のバージョンの Linux イメージから作成されます。

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>MyDCServer</VMName>
      <ServiceName>MyHPCService</ServiceName>
      <VMSize>Large</VMSize>
    </DomainController>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <LinuxComputeNodes>
    <VMNamePattern>MyLnxCN-%0001%</VMNamePattern>
    <ServiceNamePattern>MyLnxCNService%01%</ServiceNamePattern>
    <MaxNodeCountPerService>5</MaxNodeCountPerService>
    <StorageAccountNamePattern>mylnxstorage%01%</StorageAccountNamePattern>
    <VMSize>Medium</VMSize>
    <NodeCount>20</NodeCount>
    <ImageName>5112500ae3b842c8b9c604889f8753c3__OpenLogic-CentOS-70-20150325 </ImageName>
    <SSHKeyPairForRoot>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </SSHKeyPairForRoot>
  </LinuxComputeNodes>
</IaaSClusterConfig>
```


### 例 4

次の構成ファイルは、HPC Pack クラスターをデプロイします。
ローカル データベースを備えたヘッド ノードを持つし、5 個のコンピューティングを実行するノード
Windows Server 2008 R2 オペレーティング システム。 すべてのクラウド サービスは、
東アジア地域に直接作成されます。 ヘッド ノードはドメインとして機能します。
ドメイン フォレストのコント ローラー。

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionId>08701940-C02E-452F-B0B1-39D50119F267</SubscriptionId>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>HeadNodeAsDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
    <VMSize>ExtraLarge</VMSize>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>MyHPCCN-%1000%</VMNamePattern>
    <ServiceName>MyHPCCNService</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>5</NodeCount>
    <OSVersion>WindowsServer2008R2</OSVersion>
  </ComputeNodes>
</IaaSClusterConfig>
```

### 例 5

次の構成ファイルは、HPC Pack クラスターをデプロイします。
既存のドメイン フォレストです。 クラスター用のローカル個のヘッド ノードには、します。
データベース、2 つの Azure ノード テンプレートを作成し、3 つの中サイズの Azure
ノードは、Azure ノード テンプレート AzureTemplate1 に対して作成されます。 スクリプト ファイル
ヘッド ノードを構成した後、ヘッド ノードで実行されます。

```
<?xml version="1.0" encoding="utf-8" ?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>Subscription-1</SubscriptionName>
    <StorageAccount>mystorageaccount</StorageAccount>
  </Subscription>
  <AffinityGroup>MyAffinityGroup</AffinityGroup>
  <Location>East Asia</Location>  
  <VNet>
    <VNetName>MyVNet</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>ExistingDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
  </Domain>
  <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>MyHeadNode</VMName>
    <ServiceName>MyHPCService</ServiceName>
<VMSize>ExtraLarge</VMSize>
    <PostConfigScript>c:\MyHNPostActions.ps1</PostConfigScript>
  </HeadNode>
  <Certificates>
    <Certificate>
      <Id>1</Id>
      <PfxFile>d:\mytestcert1.pfx</PfxFile>
      <Password>MyPsw!!2</Password>
    </Certificate>
    <Certificate>
      <Id>2</Id>
      <PfxFile>d:\mytestcert2.pfx</PfxFile>
    </Certificate>    
  </Certificates>
  <AzureBurst>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate1</TemplateName>
      <SubscriptionId>bb9252ba-831f-4c9d-ae14-9a38e6da8ee4</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc1</ServiceName>
      <StorageAccount>myteststorage1</StorageAccount>
      <NodeCount>3</NodeCount>
      <RoleSize>Medium</RoleSize>
    </AzureNodeTemplate>
    <AzureNodeTemplate>
      <TemplateName>AzureTemplate2</TemplateName>
      <SubscriptionId>ad4b9f9f-05f2-4c74-a83f-f2eb73000e0b</SubscriptionId>
      <CertificateId>1</CertificateId>
      <ServiceName>mytestsvc2</ServiceName>
      <StorageAccount>myteststorage2</StorageAccount>
      <Proxy>
        <UsesStaticProxyCount>false</UsesStaticProxyCount>     
        <ProxyRatio>100</ProxyRatio>
        <ProxyRatioBase>400</ProxyRatioBase>
      </Proxy>
      <OSVersion>WindowsServer2012</OSVersion>
    </AzureNodeTemplate>
  </AzureBurst>
</IaaSClusterConfig>
```
## 既知の問題


* **「VNet がありません」エラー** - 複数展開する HPC Pack IaaS デプロイメント スクリプトを実行する場合
1 つまたは複数の 1 つのサブスクリプションで同時に Azure のクラスター
展開がエラーで失敗する可能性があります"VNet *VNet\_Name* が存在しない"です。
このエラーが発生した場合、失敗したデプロイに対してスクリプトを再実行してください。

* **Azure の仮想ネットワークからインターネットへのアクセスの問題** - を使用して新しいドメイン コント ローラーで、HPC Pack クラスターを作成する場合
ドメインに VM が手動で昇格するか、配置スクリプト
コント ローラー、Azure の Vm の接続に問題が発生する可能性があります。
仮想ネットワークからインターネットへ。 これは、問題は、フォワーダー DNS の場合に発生することができます。
サーバーがドメイン コント ローラーとこれに自動的に構成されています。
フォワーダー DNS サーバーが正しく解決されなかった。

    この問題を解決するには、ドメイン コント ローラーにログオンし、いずれかを記録します
    フォワーダーの構成設定を削除するか、有効な構成
    フォワーダー DNS サーバーです。 サーバー マネージャーのクリックで **ツール** >
    **DNS** DNS マネージャーを開きをダブルクリックする **フォワーダー**します。

* **RDMA ネットワークのサイズ A8 または A9 の Vm のアクセスに関する問題** - Windows Server のコンピューティング ノードまたはブローカー ノードのサイズが A8 Vm を追加する場合、または
配置スクリプトを使用して、A9、問題が発生する可能性があります。
これらの Vm をアプリケーションの RDMA ネットワークに接続します。 この理由の 1 つ
発生する可能性が HpcVmDrivers 拡張機能が正しくインストールされていないかどうかは、
ときに A8 または A9 Vm のサイズは、クラスターに追加されます。 たとえば、
拡張機能は、インストール中の状態で動かなくなる可能性があります。

    この問題を回避するには、まずで拡張機能の状態を確認します。
    バーチャル マシン。 拡張機能が正しくインストールされていない場合は、削除してみてください、
    HPC からノードは、クラスターし、再度ノードを追加します。 たとえば、次のように入力します。
    コンピューティング ノード Vm を追加するには、ヘッド ノードで Add-hpciaasnode.ps1 スクリプトを実行します。


## 次のステップ

* クラスターでテスト ワークロードを実行してみます。 例については、HPC Pack を参照してください。 [ファースト ステップ ガイド](https://technet.microsoft.com/library/jj884144)します。

* クラスターを作成し、HPC ワークロードを実行するスクリプトを使用するチュートリアルについては、次を参照してください。 [Excel と SOA ワークロードを実行する Azure で HPC Pack クラスターを使ってみる](virtual-machines-excel-cluster-hpcpac), 、[Linux での Microsoft HPC Pack での実行の NAMD の計算ノードとして Azure](virtual-machines-linux-cluster-hpcpack-namd.md), 、または [Linux での Microsoft HPC Pack での実行の OpenFOAM の計算ノードとして Azure](virtual-machines-linux-cluster-hpcpack-openfoam.md)します。

* HPC Pack のツールを試し、作成したクラスターからコンピューティング ノードを開始、停止、追加、削除してください。 参照してください [Azure のクラスターの HPC pack コンピューティング ノードの管理](virtual-machines-hpcpack-cluster-node-manage.md)

