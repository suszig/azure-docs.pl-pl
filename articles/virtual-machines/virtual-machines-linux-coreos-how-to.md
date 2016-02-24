<properties
    pageTitle="CoreOS を使用する方法 |Microsoft Azure"
    description="CoreOS について説明するほか、Azure 上でクラシック デプロイメント モデルで CoreOS 仮想マシン クラスターを作成する方法とその基本的な使用方法について説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="10/21/2015"
    ms.author="rasquill"/>

# Azure 上で CoreOS を使用する方法

このトピックでは、[CoreOS] について説明し、このオペレーティング システムを理解するためのクイック スタートとして Azure に 3 台の CoreOS 仮想マシンのクラスターを作成する方法を示します。 CoreOS デプロイメントと [Azure での CoreOS] から例の非常に基本的な要素を使用して [Tim Park CoreOS のチュートリアル] と、CoreOS デプロイメントの基本構造を理解して、正常に実行する 3 つの仮想マシンのクラスターを取得する絶対最小要件を示すために [Patrick Chanezon の CoreOS のチュートリアル] です。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [リソース マネージャーのデプロイ モデル](https://azure.microsoft.com/documentation/templates/coreos-with-fleet-multivm/)します。


## CoreOS、クラスター、Linux コンテナー

CoreOS は可能性のある非常に大規模のみと Linux コンテナーを使用する Vm のクラスターの迅速な作成をサポートするために Linux の軽量バージョン [Docker] コンテナーなどのパッケージング機構。 CoreOS は、以下をサポートすることを目的としています。

+ きわめて高度な自動化
+ より容易で一貫性のあるアプリケーションのデプロイ
+ アプリケーション レベルおよびシステム レベルでの拡張性

大まかに言えば、以下の CoreOS の機能がこれらの目的を支援します。

1. 1 つのパッケージ システム: CoreOS では、スピード、均一性、およびデプロイのしやすさを実現するため、Linux コンテナーで実行される Linux コンテナー イメージのみを実行します。
2. アトミックに実行されるオペレーティング システムの更新。そのため、オペレーティング システムは単一のエンティティとして更新され、容易に既知の状態にロールバックできます。
3. 組み込み [etcd](https://github.com/coreos/etcd) と [fleet](https://github.com/coreos/fleet) 動的 VM およびクラスター通信と管理用のデーモン (サービス)

以上は、CoreOS とその機能の一般的な説明です。 CoreOS の詳細については、[CoreOS の概要] を参照してください。

## セキュリティに関する考慮事項
現在、CoreOS では、SSH でクラスターと通信できるユーザーには、そのクラスターを管理するためのアクセス許可が与えられていることを前提としています。 そのため、CoreOS クラスターは、変更しなくてもテスト環境および開発環境として優れています。ただし、運用環境では、より強力なセキュリティ対策を実施する必要があります。

## Azure 上で CoreOs を使用する方法

このセクションでは、[Azure コマンド ライン インターフェイス (Azure CLI)] を使用して 3 台の CoreOS 仮想マシンを Azure クラウド サービスを作成する方法について説明します。 基本的な手順は次のとおりです。

1. SSH 証明書とキーを作成し、CoreOS 仮想マシンとのコミュニケーションを保護する
2. 相互に通信するために、使用するクラスターの etcd ID を取得する
3. [YAML] 形式でのクラウド構成ファイルを作成します。
4. Azure CLI を使用して、新しい Azure クラウド サービスと 3 台の CoreOS VM を作成する
5. Azure VM で CoreOS クラスターをテストする
6. localhost で CoreOS クラスターをテストする

### 通信用に公開キーと秘密キーを作成する

」の指示に従って [ssh Azure 上の Linux の使用方法](virtual-machines-linux-use-ssh-key.md) ssh パブリックとプライベート キーを作成します。 (基本的な手順は、以下の指示に記載されています)。これらのキーを使用してクラスターの VM に接続し、VM が動作中で互いに通信できることを確認します。

> [AZURE.NOTE] このトピックでは、これらのキーはありませんし、作成する必要があると想定しています `myPrivateKey.pem` と `myCert.pem` わかりやすくするためのファイルです。 既に公開キーと秘密キーのペアを `~/.ssh/id_rsa` に保存している場合は、「`openssl req -x509 -key ~/.ssh/id_rsa -nodes -days 365 -newkey rsa:2048 -out myCert.pem`」と入力するだけで、Azure にアップロードする必要のある .pem ファイルを取得できます。

1. 作業ディレクトリで、「`openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout myPrivateKey.key -out myCert.pem`」と入力し、秘密キーと秘密キーに関連付けられた X.509 証明書を作成します。

2. 秘密キーの所有者がファイルの読み取りや書き込みをできるかどうかをアサートするには、「`chmod 600 myPrivateKey.key`」と入力します。

これで、作業ディレクトリに、`myPrivateKey.key` と `myCert.pem` ファイルの両方が作成されます。


### 使用するクラスターの etcd ID を取得する

CoreOS の `etcd` デーモンでは、クラスターのすべてのノードを自動的に照会するのに検出 ID が必要です。 検出 ID を取得して、`etcdid` ファイルに保存するには、次を入力します。

```
curl https://discovery.etcd.io/new | grep ^http.* > etcdid
```

### cloud-config ファイルの作成

引き続き同じ作業ディレクトリで、任意のテキスト エディターを使用して次のテキストを含むファイルを作成し、`cloud-config.yaml` として保存します。 (するは、任意のファイル名を保存できますが、次の手順で Vm を作成する場合にこのファイルの名前を参照する必要があります、 **-カスタム データ** のオプションとして、 **azure 仮想マシンの作成** コマンドです)。

> [AZURE.NOTE] 入力 `cat etcdid` から etcd 検出 id を取得、 `etcdid` 上で作成したファイルし、置換 `<token>` 、次に `cloud-config.yaml` から生成された番号を持つファイル、 `etcdid` ファイルです。 最後にクラスターを検証できない場合は、この手順を行っていない可能性があります。

```
#cloud-config

coreos:
  etcd:
    # generate a new token for each unique cluster from https://discovery.etcd.io/new
    discovery: https://discovery.etcd.io/<token>
    # deployments across multiple cloud services will need to use $public_ipv4
    addr: $private_ipv4:4001
    peer-addr: $private_ipv4:7001
  units:
    - name: etcd.service
      command: start
    - name: fleet.service
      command: start
```

(Cloud-config ファイルの詳細については、次を参照してください [。Using Cloud-config](https://coreos.com/docs/cluster-management/setup/cloudinit-cloud-config/) 」を参照します)。

### Azure CLI を使用して、新しい CoreOS VM を作成する
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

1. まだ行っていないためには、その著作物を使用してログインするか、[Azure コマンド ライン インターフェイス (Azure CLI)] をインストールまたは学校の ID をまたは .publishsettings ファイルをダウンロードし、アカウントにインポートします。
2. CoreOS イメージを探します。 「`azure vm image list | grep CoreOS`」と入力するといつでも使用可能なイメージを探すことができ、次のような結果が一覧表示されます。

    データ: 2b171e93f07c4903bcad35bda10acf22__CoreOS 安定性-522.6.0 Public Linux

3. 」と入力して、基本的なクラスター用のクラウド サービスを作成します。
`azure service create <cloud-service-name>` ここで <*クラウド サービス名*>、CoreOS クラウド サービスの名前を指定します。 この例では、名 **`coreos-cluster`**; CoreOS VM クラウド サービス内のインスタンスを作成する名前を再利用する必要があります。

    1 つの注意: 作業をこれまでに確認する場合、 [プレビュー ポータル](https://portal.azure.com), 、クラウド サービスの名前がリソース グループとドメインの両方を次の画像で示すがわかります。

    ![CloudServiceInNewPortal]

4. クラウド サービスに接続しを使用して、新しい CoreOS VM を作成、 **azure 仮想マシンの作成** コマンドです。 X.509 証明書の場所を渡しますが、 **--ssh-cert** オプション。 次のコマンドを入力して、最初の VM イメージを作成交換を記述する **coreos クラスター** を作成したクラウド サービスの名前。

    ```
azure の仮想マシンの作成 --custom-data = cloud-config.yaml - ssh = 22--ssh-cert=./myCert.pem--なし-ssh-パスワード--vm 名 = ノード 1 - 接続 coreos クラスター--場所の = =「米国西部」2b171e93f07c4903bcad35bda10acf22__CoreOS 安定性-522.6.0 コア
```

5. Create the second node by repeating the command in step 4, replacing the **--vm-name** value with **node-2** and the **--ssh** port value with 2022.

6. Create the third node by repeating the command in step 4, replacing the **--vm-name** value with **node-3** and the **--ssh** port value with 3022.

You can see from the shot below how the CoreOS cluster appears in the portal.

![][EmptyCoreOSCluster]

### Test your CoreOS cluster from an Azure VM

To test your cluster, make sure you are in your working directory and then connect to **node-1** using **ssh**, passing the private key by typing:

    ssh core@coreos-cluster.cloudapp.net -p 22 -i ./myPrivateKey.key

Once connected, type `sudo fleetctl list-machines` to see whether the cluster has already identified all VMs in the cluster. You should receive a response similar to the following:


    core@node-1 ~ $ sudo fleetctl list-machines
    MACHINE     IP      METADATA
    442e6cfb... 100.71.168.115  -
    a05e2d7c... 100.71.168.87   -
    f7de6717... 100.71.188.96   -


### Test your CoreOS cluster from localhost

Finally, let's test your CoreOS cluster from your local Linux client. You might be able to install **fleetctl** by using **npm**, or you might want to install **fleet** and build **fleetctl** yourself on your local client. **fleet** requires **golang**, so you may need to install that first by typing:

`sudo apt-get install golang`

Then clone the **fleet** repository from github by typing:

`git clone https://github.com/coreos/fleet.git`

Build **fleet** by changing to the `fleet` directory and type

`./build`

And finally place **fleet** for easy use (depending upon your configuration you may or may not need to **sudo**):

`cp bin/fleetctl /usr/local/bin`

Make sure **fleet** has access to your `myPrivateKey.key` in the working directory by typing:

`ssh-add ./myPrivateKey.key`

> [AZURE.NOTE] If you are already using the `~/.ssh/id_rsa` key, then add that with `ssh-add ~/.ssh/id_rsa`.

Now you are ready to test remotely using the same **fleetctl** command you used from **node-1**, but passing some remote arguments:

`fleetctl --tunnel coreos-cluster.cloudapp.net:22 list-machines`

The results should be exactly the same:


    MACHINE     IP      METADATA
    442e6cfb... 100.71.168.115  -
    a05e2d7c... 100.71.168.87   -
    f7de6717... 100.71.188.96   -

## Next steps

You should now have a running three-node CoreOS cluster on Azure. From here, you can explore how to create more complex clusters and use Docker and create more interesting applications. To try a couple of quick examples, see [Get Started with Fleet on CoreOS on Azure].

<!--Anchors-->
[CoreOS, Clusters, and Linux Containers]: #intro
[Security Considerations]: #security
[How to use CoreOS on Azure]: #usingcoreos
[Subheading 3]: #subheading-3
[Next steps]: #next-steps

<!--Image references-->
[CloudServiceInNewPortal]: ./media/virtual-machines-linux-coreos-how-to/cloudservicefromnewportal.png
[EmptyCoreOSCluster]: ./media/virtual-machines-linux-coreos-how-to/endresultemptycluster.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[Azure Command-line Interface (Azure CLI)]: ../xplat-cli-install.md
[CoreOS]: https://coreos.com/
[CoreOS Overview]: https://coreos.com/using-coreos/
[CoreOS with Azure]: https://coreos.com/docs/running-coreos/cloud-providers/azure/
[Tim Park's CoreOS Tutorial]: https://github.com/timfpark/coreos-azure
[Patrick Chanezon's CoreOS Tutorial]: https://github.com/chanezon/azure-linux/tree/master/coreos/cloud-init
[Docker]: http://docker.io
[YAML]: http://yaml.org/
[Get Started with Fleet on CoreOS on Azure]: virtual-machines-linux-coreos-fleet-get-started.md

