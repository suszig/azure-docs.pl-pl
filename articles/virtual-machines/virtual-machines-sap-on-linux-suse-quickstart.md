<properties
   pageTitle="Microsoft Azure SUSE Linux VM での SAP NetWeaver のテスト | Microsoft Azure"
   description="Microsoft Azure SUSE Linux VM での SAP NetWeaver のテスト"
   services="virtual-machines,virtual-network,storage"
   documentationCenter="saponazure"
   authors="hermanndms"
   manager="juergent"
   editor=""
   tags="azure-resource-manager"
   keywords=""/>
<tags
   ms.service="virtual-machines"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="na"
   ms.date="11/26/2015"
   ms.author="hermannd"/>

# Microsoft Azure SUSE Linux VM での SAP NetWeaver のテスト


Microsoft Azure SUSE Linux VM で SAP NetWeaver をテストする際に考慮すべき項目の一覧を次に示します。
現時点では、SAP-Linux-Azure に対する正式な SAP サポートの発表はありません。 
それでもユーザーが実行いくつかのテスト、デモまたはプロトタイピング依存していない限り、
公式の SAP サポートします。 

次の一覧は、いくつかの潜在的な落とし穴を回避し、作業をやりやすくすることに役立ちます。



## Microsoft Azure で SAP をテストするための SUSE イメージ 

Azure での SAP のテストには、SLES 11SP4 と SLES 12 のみを使用します。 特殊な SUSE イメージのことができます。
Azure イメージ ギャラリーにあります: SLES 11 SP3 の SAP CAL」
これは、一般的な使用はしていません。 SAP Cloud Appliance Library 用に予約されています
SAP CAL」というソリューション ( <https://cal.sap.com/> )。オプションがありませんでした。
パブリック ネットワークからこのイメージを非表示にします。 使用しないでください。

Azure での新しいテストはすべて、Azure リソース マネージャーを使用して行う必要があります。 SUSE の SLES イメージを検索するには 
Azure Powershell または CLI を使用して、バージョンは、次のコマンドを使用する. 出力を使用し、
例: OS イメージを新しい SUSE Linux VM を展開するための json テンプレートで定義します。

* SUSE を含む既存の発行元の検索:

   ```
   PS  : Get-AzureVMImagePublisher -Location "West Europe"  | where-object { $_.publishername -like "*US*"  }
   CLI : azure vm image list-publishers westeurope | grep "US"
   ```

* SUSE からの既存の提供を検索:
      
   ```
   PS  : Get-AzureVMImageOffer -Location "West Europe" -Publisher "SUSE"
   CLI : azure vm image list-offers westeurope SUSE
   ```
      
* SUSE SLES の提供を検索:
      
   ```
   PS  : Get-AzureVMImageSku -Location "West Europe" -Publisher "SUSE" -Offer "SLES"
   CLI : azure vm image list-skus westeurope SUSE SLES
   ```
      
* SLES sku の特定のバージョンを検索:
      
   ```
   PS  : Get-AzureVMImage -Location "West Europe" -Publisher "SUSE" -Offer "SLES" -skus "12"
   CLI : azure vm image list westeurope SUSE SLES 12
   ```
     
## SUSE VM への WALinuxAgent のインストール 
 
このエージェントは、Azure ギャラリー内の SLES イメージの一部です。 ここでは場所のいずれかがどこで入手できます。
手動でインストールすることに関する情報 (例: ときから SLES OS vhd をアップロードする内部設置型)。

<http://software.opensuse.org/package/WALinuxAgent>

<https://azure.microsoft.com/documentation/articles/virtual-machines-linux-endorsed-distributions/>

<https://www.suse.com/communities/blog/suse-linux-enterprise-server-configuration-for-windows-azure/>

## Azure Linux VM への Azure データ ディスクの接続

決してデバイス ID を使用して Azure データ ディスクを Azure Linux VM にマウントしないでください。 代わりに UUID を使用してください。 注意が必要
マウント Azure データ ディスクをグラフィカルなツールなどを使用する場合 /etc/fstab 内のエントリを再確認してください。
デバイス id を持つ問題は、ことが変わるし、Azure VM は、起動時にハング可能性があります。 
プロセスです。 /etc/fstab に nofail パラメーターを追加すると、問題を軽減できる可能性があります。 注意が、
nofail アプリケーションとマウント ポイントを使用する前回と同様に、および maybe 自作ルート
外部の Azure データ ディスクがブート時にマウントされている場合に、ファイルのシステムを実行します。

## オンプレミスから Azure への SUSE VM のアップロード

次のブログでは、この手順について説明しています。
<https://azure.microsoft.com/documentation/articles/virtual-machines-linux-create-upload-vhd-suse/>

1 つが既存の SAP などを保持するには、最後にプロビジョニング解除の手順を行わない VM をアップロードする場合
インストールだけでなく、次の項目をチェックする必要があるホスト名:

* デバイス ID ではなく、UUID を使用して OS ディスクがマウントされていることを確認します。 OS ディスクには、/etc/fstab で UUID に変更するだけでは十分ではありません。 OS ディスクが以前のブート ローダーを覚えていたり、YaST 経由や /boot/grub/menu.lst の編集によりそのブート ローダーを適用したりする場合があります。
* SUSE OS ディスクに vhdx 形式を使用して、Azure にアップロードするために vhd に変換している場合、ネットワーク デバイスが eth0 から eth1 に変更される可能性が非常に高くなります。
Eth0 などに変更する必要があります後で Azure で起動するときに、問題を回避するには、次のように説明します。
ここでは: <https://dartron.wordpress.com/2013/09/27/fixing-eth1-in-cloned-sles-11-vmware/>

この記事に記載されていることに加え、以下も削除することをお勧めします。 

   /lib/udev/rules.d/75-persistent-net-generator.rules

複数の NIC がある場合を除き、waagent をインストールして潜在的な問題を回避する必要もあります。

## SAP のライセンスとハードウェア キー

計算する公式の SAP Windows Azure 証明書の新しいメカニズムが導入されました、
SAP のライセンスの使用は、SAP ハードウェア キーです。 SAP カーネルをするように調整する必要があるを使用して。 
このします。 
Linux 向けの SAP カーネルの現在のバージョンには、このコード変更が含まれていません。 そのために発生可能性があります。 
状況によっては (例: Azure VM のサイズ変更) を SAP ハードウェアの主な変更点と SAP ライセンス
無効になった

## 分散 SAP インストールでの NFS 共有

例: データベースと、SAP をインストールしようと 1 つの分散インストールが発生した場合
1 つの独立した Vm 内のアプリケーション サーバーでは、NFS を使って/sapmnt ディレクトリを共有できます。 場合があります
/sapmnt チェックの NFS 共有を作成した後、インストール手順に関する問題をする必要があります。
場合は、共有の「もの」が設定されています。 これは内部テスト ケースでの解決策でした。


## 論理ボリューム

LVM は、Azure で完全に検証されていません。 1 つの場合に複数の Azure 全体で大きな論理ボリュームが必要があります。 
データ ディスク (たとえば、SAP データベース) mdadm を使用してください。 ここでは優れたブログを
mdadm を使用して Azure で Linux RAID を設定する方法について説明します。

<https://azure.microsoft.com/documentation/articles/virtual-machines-linux-configure-raid/>


## SUSE Azure リポジトリ

標準の Azure SUSE リポジトリへのアクセスに問題がある場合にします。 
リセットし、単純なコマンドです。 これは、Azure のプライベート OS イメージを作成するときに発生する可能性があります。
地域と 1 つのイメージを別のリージョンにコピーが新しいバーチャル マシンを展開するには
この秘密の OS イメージに基づいています。 VM 内で次のコマンドを実行します。

   ```
   service guestregister restart
   ```

## Gnome デスクトップ

内の完全な SAP デモ システムをインストールするため Gnome デスクトップを使用するユーザーが希望の場合
1 つの 1 つの VM を含む SAP GUI、ブラウザー、SAP 管理コンソールには、ここでは、ちょっとしたヒント 
インストール Azure SLES イメージ。

   SLES 11

   ```
   zypper in -t pattern gnome
   ```
      
   SLES 12
   
   ```
   zypper in -t pattern gnome-basic
   ```

## クラウド内の Linux での SAP Oracle のサポート
 
これは実際には Azure 固有のトピックではなく、全般的なものですが、 それでもことが重要です。
理解します。 仮想化環境では、Linux 上の Oracle にはサポートの制約があります。
最後につまり SUSE またはもパブリック クラウドで RedHat で SAP が Oracle をサポートしないこと
Azure など。 
このトピックについては、お客様が直接 Oracle に問い合わせる必要があります。



