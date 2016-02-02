<properties
   pageTitle="Azure での docker マシンの使用 | Microsoft Azure"
   description="クラシック デプロイ モデルを使用して、Azure で Ubuntu の docker マシンを起動して実行する方法について説明します。"
   services="virtual-machines"
   documentationCenter="virtual-machines"
   authors="squillace"
   manager="timlt"
   editor="tysonn"
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure"
   ms.date="09/22/2015"
   ms.author="rasquill"/>


# Azure で docker マシンを使用する方法

使用する方法について説明 [Docker](https://www.docker.com/) と [マシン](https://github.com/docker/machine) と [Azure CLI](https://github.com/Azure/azure-xplat-cli) を迅速かつ簡単には、Ubuntu を実行しているコンピューターから Linux コンテナーを管理する Azure 仮想マシンを作成します。 両方をデプロイする方法についての例として、説明、 [busybox Docker Hub イメージ](https://registry.hub.docker.com/_/busybox/) イメージ、さらに、 [nginx Docker Hub イメージ](https://registry.hub.docker.com/_/nginx/) と web 要求を nginx コンテナーにルートするコンテナーの構成します。 (Docker **machine** ドキュメントではこの手順が他のプラットフォームでどのように変わるかについて説明しています)。


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


このチュートリアルを完了するにはいくつかの前提条件があります。 次をインストールする必要があります。

1. [npm](https://docs.npmjs.com/) and [Node.js](http://nodejs.org/)
2. [Azure CLI](https://github.com/Azure/azure-xplat-cli)
3. [Docker クライアント](https://docs.docker.com/installation/)

上記の順番でインストールすると、Ubuntu コンピューターでこのチュートリアルを行う準備は完了します (このチュートリアルは大部分が Debian などその他の dpkg ベースのディストリビューションと同じです。 条件や手順に追加する事項がありましたらコメントでお知らせください)。

## docker マシンを取得またはビルドする

開始する最も簡単な方法 **docker マシン** から直接該当のリリースをダウンロードするには、 [リリース共有](https://github.com/docker/machine/releases)します。 このチュートリアルでは、クライアントとして x64 コンピューターで Ubuntu を実行していたので、**docker-machine_linux-amd64** イメージを使用しています。

ビルドすることも、 **docker マシン** 次の手順に従って、自分で [マシンに貢献して](https://github.com/docker/machine#contributing)します。 ビルドには 1 GB 以上の空き容量が必要になりますが、これでご希望の環境を自由にビルドできるようになります。
> [AZURE.NOTE] 作成できますが、 [シンボリック リンク](http://en.wikipedia.org/wiki/Symbolic_link) プラットフォームがこのチュートリアルのバージョンは直接動作を確認する非常に明確にバイナリを使用します。 結果コマンドではなくなど `docker マシン env` として、 **docker マシン** ドキュメントで示されたこのチュートリアルでは `docker machine_linux-amd64 env` 代わりにします。 symlink でもバイナリ名ディレクトリでも自由に作成できますが、使用している名前を変更する場合は下記の手順で変更する必要がありますのでご注意ください。

<br />

>  いずれの方法にしても、コマンドラインでバイナリ ディレクトリを直接呼び出すか、**/usr/local/bin** などのパス情報をバイナリで指定する必要があります。 」と入力して実行可能とマークされていることを確認する `chmod + x` < *`binaryName`*>、<*`binaryName`* > Docker マシン実行可能ファイルの名前を指定します。 このチュートリアルでは、**docker-machine_linux-amd64** という名前を使用しています。

## Docker、Machine、Azure の証明書とキー ファイルの作成

次に、Azure で ID やアクセス許可を確認する必要があるため、証明書とキー ファイルを作成します。これは **docker-machine** がご利用の Azure 仮想マシンと通信してリモートでコンテナーを作成して管理するためにも必要です。 これらのファイルがすでにディレクトリにある場合、おそらく Docker で使用したものですが、再利用できます。 ただし、ベスト プラクティスとしては、**docker-machine** のテストのため、別のディレクトリに作成して docker-machine を指定することをお勧めします。
> [AZURE.NOTE] しようとしてしまう場合 **docker マシン** オーバー、および何度もあるか、同じ証明書とキー ファイルを再利用することを確認します。 **docker マシン** 一連のクライアント証明書の作成に、その同様作成検証できる `~/.docker/machine`します。 これらの証明書を別のコンピューターに移す場合は、**docker-machine** 証明書フォルダーも同様に移す必要があります。 これによって、動作の検証などの目的で **docker-machine** を別のプラットフォームで使う場合でも、問題なく動作させることが可能になります。

Linux ディストリビューションの使用経験がある場合は、既にしたこれらのファイルは、コンピューターの特定の場所で使用できる、 [Docker HTTPS ドキュメントが、これらの手順を説明します](https://docs.docker.com/articles/https/)します。 以下に、この手順を最も簡単に行う方法を示します。

1. ローカル フォルダーを作成し、コマンドラインでそのフォルダーに移動して次を入力します。

        openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
        openssl pkcs12 -export -out mycert.pfx -in mycert.pem -name "My Certificate"

    ここで、今後の使用に備えて、証明書のエクスポート パスワードを入力してキャプチャします。 次を入力します。

        openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

2. 証明書の .cer ファイルを Azure にアップロードします。  [Azure クラシック ポータル](https://manage.windowsazure.com), 、クリックして **設定** 下図のとおりサービス エリアの左下にあります。

    ![][portalsettingsitem]

    次に、下図の **[管理証明書]** をクリックします。

    ![][managementcertificatesitem]

    **アップロード** (ページの下部) ![][uploaditem] をアップロードする、 **mycert.cer** 前の手順で作成したファイルです。

3. ポータルの同じ **[設定]** ウィンドウで、**[サブスクリプション]** をクリックして VM の作成時に使ったサブスクリプション ID をキャプチャします。この ID は次の手順で使用します  (Azure CLI コマンドを使用してコマンドラインで、サブスクリプション id を検索することも `azure アカウント一覧`, 、アカウントがある各サブスクリプションのサブスクリプション id が表示されます)。

4. **docker-machine create** コマンドを使って Azure で Docker ホスト VM を作成します。 このコマンドには、前の手順でキャプチャしたサブスクリプション ID と手順 1 で作成した **.pem** ファイルのパスが必要です。 このトピックでは、Azure クラウド サービス (とご利用の VM) の名前として「machine-name」を使用していますが、ご自分で使う名前に変更し、VM の名前が必要なすべての手順でこのクラウド サービス名を使用してください (この例では完全なバイナリ名を使用しており、**docker-machine** symlink は使っていません)。

        docker-machine_linux-amd64 create \
        -d azure \
        --azure-subscription-id="<subscription ID acquired above>" \
        --azure-subscription-cert="mycert.pem" \
        machine-name

    始めの 2 つの手順で新しい VM を作成し、Linux Azure エージェントを読み込み、新しい VM をアップデートしているため、現在は次のような状態になっています。

        INFO[0001] Creating Azure machine...
        INFO[0049] Waiting for SSH...
        modprobe: FATAL: Module aufs not found.
        + sudo -E sh -c sleep 3; apt-get update
        + sudo -E sh -c sleep 3; apt-get install -y -q linux-image-extra-3.13.0-36-generic
        E: Unable to correct problems, you have held broken packages.
        modprobe: FATAL: Module aufs not found.
        Warning: tried to install linux-image-extra-3.13.0-36-generic (for AUFS)
         but we still have no AUFS.  Docker may not work. Proceeding anyways!
        + sleep 10
        + [ https://get.docker.com/ = https://get.docker.com/ ]
        + sudo -E sh -c apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
        gpg: requesting key A88D21E9 from hkp server keyserver.ubuntu.com
        gpg: key A88D21E9: public key "Docker Release Tool (releasedocker) <docker@dotcloud.com>" imported
        gpg: Total number processed: 1
        gpg:               imported: 1  (RSA: 1)
        + sudo -E sh -c echo deb https://get.docker.com/ubuntu docker main > /etc/apt/sources.list.d/docker.list
        + sudo -E sh -c sleep 3; apt-get update; apt-get install -y -q lxc-docker
        + sudo -E sh -c docker version
        INFO[0368] "machine-name" has been created and is now the active machine.
        INFO[0368] To point your Docker client at it, run this in your shell: $(docker-machine_linux-amd64 env machine-name)

    > [AZURE.NOTE] VM が作成され、使用できる状態になるまでに数分かかる場合もあります。 」と入力して、新しい Docker ホストの状態を確認するには、待っている間 `azure vm 一覧` Vm が表示されるまで、Azure CLI を使用して、 **ReadyRole** 状態です。

5. Docker とマシンの環境変数をターミナル セッション向けに設定します。 最後の行に記述されているとおり、直ちに **env** コマンドを実行して必要な環境変数をエクスポートし、特定のマシンにおける Docker クライアントで直接使用します。

        $(docker-machine_linux-amd64 env machine-name)

    この場合、特別なコマンドを入力することなく、Docker **machine** が作成された VM に接続するローカル Docker クライアントを使うことができます。

        $ docker info
        Containers: 0
        Images: 0
        Storage Driver: devicemapper
         Pool Name: docker-8:1-131736-pool
         Pool Blocksize: 65.54 kB
         Backing Filesystem: extfs
         Data file: /dev/loop0
         Metadata file: /dev/loop1
         Data Space Used: 305.7 MB
         Data Space Total: 107.4 GB
         Metadata Space Used: 729.1 kB
         Metadata Space Total: 2.147 GB
         Udev Sync Supported: false
         Data loop file: /var/lib/docker/devicemapper/devicemapper/data
         Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
         Library Version: 1.02.82-git (2013-10-04)
        Execution Driver: native-0.2
        Kernel Version: 3.13.0-36-generic
        Operating System: Ubuntu 14.04.1 LTS
        CPUs: 1
        Total Memory: 1.639 GiB
        Name: machine-name
        ID: W3FZ:BCZW:UX24:GDSV:FR4N:N3JW:XOC2:RI56:IWQX:LRTZ:3G4P:6KJK
        WARNING: No swap limit support


> [AZURE.NOTE] このチュートリアルでは、VM 1 つを作成する **docker-machine** について説明していますが、上記の手順を繰り返すことで、数の制限なく複数作成することも可能です。複数作成する場合、VM と Docker を切り替える最善の方法は、インラインで **env** コマンドを使用して各コマンドに **docker** 環境変数を設定することです。例については、使用する **docker info** を異なる VM を入力できます `docker (docker machine env < VM 名 >) を $ 情報` と **env** コマンドによってその VM で使用する docker 接続情報が設定されます。

## 設定完了。Docker ハブから Docker とイメージを使ってリモートでいくつかのアプリケーションを実行

これで、通常の方法で Docker を使用してコンテナーでアプリケーションを作成できます。 示すためには、簡単なは [busybox](https://registry.hub.docker.com/_/busybox/):

        $  docker run busybox echo hello world
        Unable to find image 'busybox:latest' locally
        511136ea3c5a: Pull complete
        df7546f9f060: Pull complete
        ea13149945cb: Pull complete
        4986bf8c1536: Pull complete
        busybox:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
        Status: Downloaded newer image for busybox:latest
        hello world

ただし、すぐに確認できます、インターネット上など、アプリケーションを作成することがあります、 [nginx](https://registry.hub.docker.com/_/nginx/) から、 [Docker Hub](https://registry.hub.docker.com/)します。
> [AZURE.NOTE] **-P** オプションを使ってイメージに対するランダム ポートを **Docker** にアサインすることと、**-d** オプションを使用してコンテナーがバックグラウンドで継続的に実行される状態になっていることをご確認ください  (これを実行しないと、nginx を起動してもすぐにシャットダウンしますので、 確実に設定してください)。

    $ docker run --name machinenginx -P -d nginx
    Unable to find image 'nginx:latest' locally
    30d39e59ffe2: Pull complete
    c90d655b99b2: Pull complete
    d9ee0b8eeda7: Pull complete
    3225d58a895a: Pull complete
    224fea58b6cc: Pull complete
    ef9d79968cc6: Pull complete
    f22d05624ebc: Pull complete
    117696d1464e: Pull complete
    2ebe3e67fb76: Pull complete
    ad82b43d6595: Pull complete
    e90c322c3a1c: Pull complete
    4b5657a3d162: Pull complete
    511136ea3c5a: Already exists
    nginx:latest: The image you are pulling has been verified. Important: image verification is a tech preview feature and should not be relied on to provide security.
    Status: Downloaded newer image for nginx:latest
    5883e2ff55a4ba0aa55c5c9179cebb590ad86539ea1d4d367d83dc98a4976848

インターネット経由で確認するには、ポート 80 で Azure VM に対してパブリック エンドポイントを作成し、それを nginx コンテナーのポートに対してマッピングします。 まず、使用して `docker ps-a` コンテナーを検索し、どのポートを見つける **docker** がコンテナーのポート 80 に割り当てられています。 (下記はポート情報のみを抽出しています。実際はより多くの情報が表示されます)。

    $ docker ps -a
    IMAGE               PORTS
    nginx:latest        0.0.0.0:49153->80/tcp, 0.0.0.0:49154->443/tcp
    busybox:latest

Docker でコンテナーのポート 80 が VM のポート 49153 に割り当てられていることがわかります。 ここで Azure CLI を使って外部クラウド サービスのパブリック ポート 80 を VM のポート 49153 にマッピングします。 すると Docker によって、VM のポート 49153 における受信 TCP トラフィックが nginx コンテナーにルーティングされます。

    $ azure vm endpoint create machine-name 80 49153
    info:    Executing command vm endpoint create
    + Getting virtual machines
    + Reading network configuration
    + Updating network configuration
    info:    vm endpoint create command OK

任意のブラウザーを使ってご確認ください。

![][nginx]


## 次のステップ

移動して、 [Docker ユーザー ガイド](https://docs.docker.com/userguide/) し、Microsoft Azure で一部のアプリケーションを作成します。 または、遊んで [* * * * docker と swarm](https://github.com/docker/swarm) Azure](virtual-machines-docker-swarm) と docker と Azure での swarm の使用方法を参照してください。




[nginx]: ./media/virtual-machines-docker-machine/nginxondocker.png 
[portalsettingsitem]: ./media/virtual-machines-docker-machine/portalsettingsitem.png 
[managementcertificatesitem]: ./media/virtual-machines-docker-machine/managementcertificatesitem.png 
[uploaditem]: ./media/virtual-machines-docker-machine/uploaditem.png 
[link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-tutorial.md 
[link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md 
[link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md 

