<properties
    pageTitle="クラウド サービスで仮想マシンを接続する | Microsoft Azure"
    description="クラシック デプロイメント モデルで作成された仮想マシンを、Azure クラウド サービスまたは仮想ネットワークに接続します。"
    services="virtual-machines"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-multiple"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/15/2015"
    ms.author="cynthn"/>


# 仮想ネットワークまたはクラウド サービスで構成されるクラシック デプロイ モデルで作成された仮想マシンに接続します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


クラシック デプロイメント モデルで作成された仮想マシンは、常にクラウド サービス内に配置されます。 クラウド サービスはコンテナーとして機能し、インターネット経由で仮想マシンにアクセスするために、一意のパブリック DNS 名、パブリック IP アドレス、一連のエンドポイントを提供します。 クラウド サービスは仮想ネットワークに配置できますが、必須ではありません。

クラウド サービスは、仮想ネットワークではありませんが、関数が呼び出される、 *スタンドアロン* クラウド サービスです。 スタンドアロン クラウド サービス内にある仮想マシンが他の仮想マシンと通信するには、他の仮想マシンのパブリック DNS 名を使用する必要があり、そのトラフィックはインターネットを介して送信されます。 クラウド サービスが仮想ネットワークにある場合、そのクラウド サービス内にある仮想マシンは、トラフィックをインターネットに送信することなく、仮想ネットワーク内の他のすべての仮想マシンと通信できます。

同じスタンドアロン クラウド サービスに仮想マシンを配置する場合は、負荷分散と可用性セットを引き続き使用できます。 詳細については、「 [仮想マシンの負荷分散](../articles/load-balance-virtual-machines.md) と [仮想マシンの可用性管理](../articles/manage-availability-virtual-machines.md)します。 ただし、仮想マシンをサブネットでまとめることや、スタンドアロン クラウド サービスをオンプレミスのネットワークに接続することはできません。 次に例を示します。

![Virtual machines in a standalone cloud service](./media/howto-connect-vm-cloud-service/CloudServiceExample.png)

仮想マシンを仮想ネットワークに配置する場合、負荷分散と可用性セットに使用するクラウド サービスの数を決めることができます。 さらに、オンプレミスのネットワークと同じように仮想マシンをサブネットにまとめ、仮想ネットワークをオンプレミスのネットワークに接続することもできます。 次に例を示します。

![Virtual machines in a virtual network](./media/howto-connect-vm-cloud-service/VirtualNetworkExample.png)

仮想ネットワークは、Azure で仮想マシンを接続するためのお勧めの方法です。 また、アプリケーションの各階層を別々のクラウド サービスで構成することもお勧めします。 ただし、各サブスクリプションで割り当てられるクラウド サービスの最大数である 200 を超えないように、異なるアプリケーション層にあるいくつかの仮想マシンを同じクラウド サービスにまとめることが必要になる場合もあります。 これと他の制限を確認するを参照してください。 [Azure サブスクリプションとサービスの制限、クォータ、および制約](../azure-subscription-service-limits.md)します。

## 仮想ネットワークでの VM の接続

仮想マシンを仮想ネットワークで接続する方法:

1.  仮想ネットワークを作成、 [Azure ポータル](../articles/virtual-network/virtual-networks-create-vnet-classic-pportal.md)します。
2.  デプロイメントで可用性セットと負荷分散用の設計を反映させるため、クラウド サービスのセットを作成します。 Azure クラシック ポータルで、クリックして **新規 > コンピューティング > クラウド サービス > カスタム作成** クラウド サービスごとにします。
3.  新しい各仮想マシンを作成するにはクリックして **新規 > コンピューティング > 仮想マシン > ギャラリーから**します。 VM の適切なクラウド サービスと仮想ネットワークを選択します。 仮想ネットワークに既に参加しているクラウド サービスがある場合、その名前が既定で選択されます。

![Selecting a cloud service for a virtual machine](./media/howto-connect-vm-cloud-service/VMConfig1.png)

## スタンドアロン クラウド サービスでの VM の接続

仮想マシンをスタンドアロンのクラウド サービスで接続するには:

1.  クラウド サービスを作成、 [Azure クラシック ポータル](http://manage.windowsazure.com)します。 クリックして **新しい > コンピューティング > クラウド サービス > カスタム作成**します。 また、仮想マシンを初めて作成するときに、デプロイメント用のクラウド サービスを作成することもできます。
2.  仮想マシンを作成するときは、前の手順で作成したクラウド サービスの名前を選択します。
![仮想マシンを既存のクラウド サービスに追加する](./media/howto-connect-vm-cloud-service/Connect-VM-to-CS.png)

##リソース
[分散仮想マシンを負荷します。](../articles/load-balance-virtual-machines.md)

[仮想マシンの可用性管理](../articles/manage-availability-virtual-machines.md)

[仮想ネットワークの構成タスク](../documentation/services/virtual-machines/)

仮想マシンを作成したら、サービスやワークロードがデータを格納するための場所として、データ ディスクを追加することをお勧めします。 次を参照してください。

[データ ディスクを Linux 仮想マシンに接続する方法](../articles/virtual-machines/virtual-machines-linux-how-to-attach-disk.md)

[データ ディスクを Windows 仮想マシンに接続する方法](../articles/virtual-machines/storage-windows-attach-disk.md)

