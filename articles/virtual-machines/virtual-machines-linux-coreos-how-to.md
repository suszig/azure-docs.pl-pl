<properties
    pageTitle="CoreOS を使用する方法 |Microsoft Azure"
    description="CoreOS について説明するほか、Azure 上でクラシック デプロイ モデルで CoreOS 仮想マシン クラスターを作成する方法とその基本的な使用方法について説明します。"
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

」の指示に従って [ssh Azure 上の Linux の使用方法](virtual-machines-linux-use-ssh-key.md) ssh パブリックとプライベート キーを作成します。 (基本的な手順は、以下の指示に記載されています)。 これらのキーを使用してクラスターの VM に接続し、VM が動作中で互いに通信できることを確認します。
> [AZURE.NOTE] このトピックでは、これらのキーはありませんし、作成する必要があると想定しています `'myprivatekey.pem'` と `myCert.pem` わかりやすくするためのファイルです。 パブリックが既にあり、秘密キーのペアを保存する場合 `~/.ssh/id_rsa`, 、だけを入力すれば `openssl req-x-x509-key ~/.ssh/id_rsa-ノード--days 365 - newkey rsa:2048--out Mycert.pem'` を Azure にアップロードする必要のある .pem ファイルを取得します。

1. 作業ディレクトリで次のように入力します。 `openssl req - x 509 - ノード--days 365 - newkey rsa:2048 - keyout myPrivateKey.key--out Mycert.pem'` 秘密キーを作成すると、X.509 証明書が関連付けられています。

2. 秘密キーの所有者が読み取りまたはファイルの書き込みができる、assert 入力 `chmod 600 Myprivatekey.key'`します。

この時点でが両方とも、 `myPrivateKey.key` と `myCert.pem` 作業ディレクトリでファイルです。


### 使用するクラスターの etcd ID を取得する

CoreOS の `etcd` デーモンでは、クラスターのすべてのノードを自動的に照会するのにデーモン検出 ID が必要です。 検出 ID を取得して、`etcdid` ファイルに保存するには、次を入力します。

```
curl https://discovery.etcd.io/new | grep ^http.* > etcdid
```

### cloud-config ファイルの作成

でも、同じ作業ディレクトリにファイルを作成、任意のテキストを次のテキスト エディター、として保存 `cloud-config.yaml`します。 (任意のファイル名で保存できますが、次の手順で VM を作成するときに、**azure vm create** コマンドの **--custom-data** オプションでこのファイル名を参照する必要があります)。
> [AZURE.NOTE] 入力 `cat etcdid` から etcd 検出 id を取得、 `etcdid` 上で作成したファイルし、置換 `< トークン >` 、次に `cloud-config.yaml` から生成された番号を持つファイル、 `etcdid` ファイルです。最後にクラスターを検証できない場合は、この手順を行っていない可能性があります。

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

(Cloud-config ファイルの詳細については、次を参照してください [Using Cloud-config](https://coreos.com/docs/cluster-management/setup/cloudinit-cloud-config/) 」を参照します。)。

### Azure CLI を使用して、新しい CoreOS VM を作成する

1. まだ行っていないためには、その著作物を使用してログインするか、[Azure コマンド ライン インターフェイス (Azure CLI)] をインストールまたは学校の ID をまたは .publishsettings ファイルをダウンロードし、アカウントにインポートします。
2. CoreOS イメージを探します。 いつでも使用可能なイメージを見つけるには、入力 `azure vm イメージ リスト | grep CoreOS` のような結果の一覧を表示する必要があります。

    データ: 2b171e93f07c4903bcad35bda10acf22__CoreOS 安定性-522.6.0 Public Linux

3. 」と入力して、基本的なクラスター用のクラウド サービスを作成します。
`azure サービスの作成 < クラウド サービス名 >` 、 <*cloud-service-name*> 、CoreOS クラウド サービスの名前を指定します。この例では、名 * *`coreos クラスター`* * は、クラウド サービス内のインスタンスに CoreOS VM を作成する名前を再利用する必要があります。

    1 つの注意: 作業をこれまでに確認する場合、 [プレビュー ポータル](https://portal.azure.com), 、クラウド サービスの名前がリソース グループとドメインの両方を次の画像で示すがわかります。

    ![][cloudserviceinnewportal]

4. クラウド サービスに接続し、**azure vm create** コマンドを使用して、新しい CoreOS VM インスタンスを作成します。 **--ssh-cert** オプションで X.509 証明書の場所を渡します。 次を入力して、最初の VM イメージを作成し、次のように必ず **coreos-cluster** を作成したクラウド サービスの名前に置き換えます。

    ```
azure vm create --custom-data=cloud-config.yaml --ssh=22 --ssh-cert=./myCert.pem --no-ssh-password --vm-name=node-1 --connect=coreos-cluster --location="West US" 2b171e93f07c4903bcad35bda10acf22__CoreOS-Stable-522.6.0 core
 ```

5. 手順 4. のコマンドを繰り返し、2 番目のノードを作成します。その際、**--vm-name** 値は **node-2** に、**--ssh** ポートの値は 2022 に置き換えます。

6. 手順 4. のコマンドを繰り返し、3 番目のノードを作成します。その際、**--vm-name** 値は **node-3** に、**--ssh** ポートの値は 3022 に置き換えます。

次の画像では、ポータルに CoreOS クラスターが表示されている状態を示します。

![][emptycoreoscluster]

### Azure VM で CoreOS クラスターをテストする

クラスターをテストするには、作業ディレクトリで次を入力し、**ssh** により **node-1** に接続して、秘密キーを渡します。

    ssh core@coreos-cluster.cloudapp.net -p 22 -i ./myPrivateKey.key

接続されると、入力 `sudo fleetctl 一覧-マシン` クラスターがクラスター内のすべての Vm を既に特定するかどうかを確認します。 次のような応答を受け取ります。


    core@node-1 ~ $ sudo fleetctl list-machines
    MACHINE     IP      METADATA
    442e6cfb... 100.71.168.115  -
    a05e2d7c... 100.71.168.87   -
    f7de6717... 100.71.188.96   -

### localhost で CoreOS クラスターをテストする

最後に、ローカル Linux クライアントで CoreOS クラスターをテストします。 インストールすることもできます **fleetctl** を使用して **npm**, 、インストールするか **fleet** およびビルド **fleetctl** ローカル クライアントで自分でします。 **fleet** 必要 **golang**, ので、その最初に入力して、インストールする必要があります。

`sudo apt get インストール golang`

次を入力して、github から **fleet** リポジトリを複製します。

`git クローン https://github.com/coreos/fleet.git`

ビルド **fleet** を変更することで、 `fleet` ディレクトリと種類

`。/build`

最後に、次のように **fleet** を使いやすいように配置します (**sudo** 使用する必要があるかどうかは、構成によって変わります)。

`cp bin/fleetctl/usr/local/bin`

確認 **fleet** へのアクセスを持つ、 `myPrivateKey.key` 」と入力して、作業ディレクトリ。

`ssh アド./myPrivateKey.key`
> [AZURE.NOTE] 既に使用している場合、 `~/.ssh/id_rsa` キーしている追加 `ssh アド ~/.ssh/id_rsa`します。

これで、**node-1** で使用したのと同じ **fleetctl** コマンドを使用してリモートでテストできます。ただし、以下のようにリモート引数をいくつか渡します。

`fleetctl--トンネル coreos cluster.cloudapp.net:22 一覧-マシン`

以下とまったく同じ結果が得られます。


    MACHINE     IP      METADATA
    442e6cfb... 100.71.168.115  -
    a05e2d7c... 100.71.168.87   -
    f7de6717... 100.71.188.96   -

## 次のステップ

これで、3 ノード構成の CoreOS クラスターが Azure 上で実行されています。 ここから、より複雑なクラスターを作成する方法、Docker を使用する方法、さらにより有益なアプリケーションを作成する方法を調べることができます。 いくつかの簡単な例については、[を使ってみる Azure 上の CoreOS で Fleet] を参照してください。





[coreos, clusters, and linux containers]: #intro 
[security considerations]: #security 
[how to use coreos on azure]: #usingcoreos 
[subheading 3]: #subheading-3 
[next steps]: #next-steps 
[cloudserviceinnewportal]: ./media/virtual-machines-linux-coreos-how-to/cloudservicefromnewportal.png 
[emptycoreoscluster]: ./media/virtual-machines-linux-coreos-how-to/endresultemptycluster.png 
[6]: ./media/markdown-template-for-new-articles/pretty49.png 
[7]: ./media/markdown-template-for-new-articles/channel-9.png 
[azure command-line interface (azure cli)]: ../xplat-cli-install.md 
[coreos]: https://coreos.com/ 
[coreos overview]: https://coreos.com/using-coreos/ 
[coreos with azure]: https://coreos.com/docs/running-coreos/cloud-providers/azure/ 
[tim park's coreos tutorial]: https://github.com/timfpark/coreos-azure 
[patrick chanezon's coreos tutorial]: https://github.com/chanezon/azure-linux/tree/master/coreos/cloud-init 
[docker]: http://docker.io 
[yaml]: http://yaml.org/ 
[get started with fleet on coreos on azure]: virtual-machines-linux-coreos-fleet-get-started.md 

