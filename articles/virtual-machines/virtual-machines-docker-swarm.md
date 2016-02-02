<properties
   pageTitle =「入門 Azure 上の swarm における docker の使用を」
   description =「Docker VM 拡張機能を Vm のグループを作成し、swarm を使用して、Docker クラスターを作成する方法について説明します」。
   サービス =「仮想マシン」
   documentationCenter =「仮想マシン」
   authors ="squillace"
   manager ="timlt"
   editor="tysonn"/>

<tags
   ms.service=「仮想マシン」
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm linux"
   ms.workload="infrastructure"
   ms.date="09/22/2015"
   ms.author="rasquill"/>

# Docker Swarm を使用する方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


このトピックは、使用する簡単な方法を示しています。 [docker](https://www.docker.com/) と [swarm](https://github.com/docker/swarm) を Azure で swarm 管理のクラスターを作成します。 ここでは Azure で 4 つの仮想マシンが作成されます。1 つは Swarm マネージャーとして動作し、その他の 3 つは Docker ホストのクラスターの一部として機能します。 終了すれば、Swarm を使ってクラスターを確認し、クラスター上で Docker を使用できるようになります。 また、このトピックの Azure CLI 呼び出しではサービス管理 (asm) モードを使用します。
> [AZURE.NOTE] こちらの記述ではソフトウェアの以前のバージョンが使用されています。大規模で、バランスがとれ、コントロールされた Docker コンテナーのクラスターを Azure で使用できるようにするため、アップデートをご確認ください。また Docker Swarm のドキュメントもご確認いただき、すべての機能をご覧ください。

> また、このトピックでは docker を swarm および Azure CLI *せず* を使用して **docker マシン** さまざまなツールがどのように連携を表示するが、独立性を確保するためにします。 **docker マシン** が **--swarm** スイッチを使用するための **docker マシン** 直接 swarm にノードを追加します。 例については、次を参照してください。、 [docker マシン](https://github.com/docker/machine) ドキュメントです。 不足している場合 **docker マシン** Azure Vm に対して実行されているを参照してください [Azure で docker マシンを使用する方法](virtual-machines-docker-machine.md)します。

## Azure 仮想マシンでの Docker ホストの作成

ここでは 4 つの VM を作成しますが、作成する VM の数は任意に決められます。 使用して、次を呼び出す *< パスワード >* で指定したパスワードを交換してください。

    azure vm docker create swarm-master -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-1 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-2 -l "East US" -e 22 $imagename ops <password>
    azure vm docker create swarm-node-3 -l "East US" -e 22 $imagename ops <password>

呼び出しが終了すると、**azure vm list** を使って Azure VM を確認できるようになります。

    $ azure vm list | grep "swarm-[mn]"
    data:    swarm-master     ReadyRole           East US       swarm-master.cloudapp.net                               100.78.186.65
    data:    swarm-node-1     ReadyRole           East US       swarm-node-1.cloudapp.net                               100.66.72.126
    data:    swarm-node-2     ReadyRole           East US       swarm-node-2.cloudapp.net                               100.72.18.47  
    data:    swarm-node-3     ReadyRole           East US       swarm-node-3.cloudapp.net                               100.78.24.68  

## Swarm マスター VM での Swarm のインストール

このトピックでは、 [docker のインストールのコンテナーのモデルの swarm のドキュメント](https://github.com/docker/swarm#1---docker-image) --に ssh で通信こともできますが、 **swarm マスター**します。 このモデルでは、**Swarm** は Swarm を実行する Docker コンテナーとしてダウンロードされます。 下記では、*Docker を使ってノート パソコンからリモートで*この手順を実行し、**swarm-master** VM に接続してクラスターの ID 生成コマンドである **swarm create** を使うように指定しています。 クラスター ID を使うことで、**Swarm** で Swarm グループのメンバーを見つけられるようになります (リポジトリを複製して自身で構築することも可能です。そうすると完全に制御でき、デバッグもできます)。

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm create
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    36731c17189fd8f450c395db8437befd

最後の行がクラスター ID です。ノード VM を Swarm マスターに結合して「Swarm」を作成する際に再度使いますので、どこかに保存しておいてください）。 この例では、クラスター ID は **36731c17189fd8f450c395db8437befd** です。
> [AZURE.NOTE] ここでは、ローカルにインストールされている Docker を使用して Azure の **swarm-master** VM に接続しています。また、**swarm-master** で **create** コマンドをダウンロード、インストール、実行する手順を説明します。このコマンドによって、後で検索に使用するクラスター ID が返されます。

> これを確認するには実行 `docker-h tcp://`*< ホスト名 >* ` イメージ` で処理されるコンテナーを一覧表示する、 **swarm マスター** コンピューターと比較のための別のノードで (で前に swarm コマンドを実行しているため、 **--rm** スイッチ、コンテナーが終了した後、これを使用して削除された **docker ps-a** も返されません).。


        $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        swarm               latest              92d78d321ff2        11 days ago         7.19 MB
        $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 images
        REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
        $

<P />
> **Docker** の使用経験がある場合は、イメージがダウンロードされておらず実行されていないため、その他のノードにエントリがないことはご存知でしょう。

## ノード VM の Docker クラスターへの結合

各ノードで Azure CLI を使ってエンドポイント情報を一覧表示します。 ここではノードの Docker ポートを取得するため、**swarm-node-1** Docker ホストに対してこれを行っています。

    $ azure vm endpoint list swarm-node-1
    info:    Executing command vm endpoint list
    + Getting virtual machines
    data:    Name    Protocol  Public Port  Private Port  Virtual IP      EnableDirectServerReturn  Load Balanced
    data:    ------  --------  -----------  ------------  --------------  ------------------------  -------------
    data:    docker  tcp       2376         2376          138.91.112.194  false                     No
    data:    ssh     tcp       22           22            138.91.112.194  false                     No
    info:    vm endpoint list command OK

使用して **docker** と `-h` ノード VM における docker クライアントをオプションでは、ノードをクラスター id とノードの docker ポートを渡すことによって作成する swarm に参加 (後者を使用して、 **--addr**)。

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 run -d swarm join --addr=138.91.112.194:2376 token://36731c17189fd8f450c395db8437befd
    Unable to find image 'swarm:latest' locally
    511136ea3c5a: Pull complete
    a8bbe4db330c: Pull complete
    9dfb95669acc: Pull complete
    0b3950daf974: Pull complete
    633f3d9a9685: Pull complete
    bba5f98a0414: Pull complete
    defbc1ab4462: Pull complete
    92d78d321ff2: Pull complete
    swarm:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for swarm:latest
    bbf88f61300bf876c6202d4cf886874b363cd7e2899345ac34dc8ab10c7ae924

問題ないようです。 **swarm** が **swarm-node-1** で実行されていることを確認するには、次のように入力します。

    $ docker --tls -H tcp://swarm-node-1.cloudapp.net:2376 ps -a
        CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS               NAMES
        bbf88f61300b        swarm:latest        "/swarm join --addr=   13 seconds ago      Up 12 seconds       2375/tcp            angry_mclean

クラスターのすべてのノードに対して繰り返します。 ここでは、**swarm-node-2** と **swarm-node-3** に対して行っています。

## Swarm クラスター管理の開始

    $ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run -d -p 2375:2375 swarm manage token://36731c17189fd8f450c395db8437befd
    d7e87c2c147ade438cb4b663bda0ee20981d4818770958f5d317d6aebdcaedd5

これで次のようにクラスターのノードを一覧表示できます。

    ralph@local:~$ docker --tls -H tcp://swarm-master.cloudapp.net:2376 run --rm swarm list token://73f8bc512e94195210fad6e9cd58986f
    54.149.104.203:2375
    54.187.164.89:2375
    92.222.76.190:2375

## 次のステップ

Swarm でお試しください。 インスピレーションを探すように、次を参照してください。 [https://github.com/docker/swarm/](https://github.com/docker/swarm/), 、または多くの場合、 [ビデオ](https://www.youtube.com/watch?v=EC25ARhZ5bI)します。




[docker-machine-azure]: virtual-machines-docker-machine.md 

