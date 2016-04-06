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

使用する方法について説明 [Docker](https://www.docker.com/) と [マシン](https://github.com/docker/machine) と [Azure CLI](https://github.com/Azure/azure-xplat-cli) を迅速かつ簡単には、Ubuntu を実行しているコンピューターから Linux コンテナーを管理する Azure 仮想マシンを作成します。 両方をデプロイする方法についての例として、説明、 [busybox Docker Hub イメージ](https://registry.hub.docker.com/_/busybox/) イメージ、さらに、 [nginx Docker Hub イメージ](https://registry.hub.docker.com/_/nginx/) と web 要求を nginx コンテナーにルートするコンテナーの構成します。 (Docker **マシン** ドキュメントでこの手順の他のプラットフォームを変更する方法について説明します)。


[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


このチュートリアルを完了するにはいくつかの前提条件があります。 次をインストールする必要があります。

1. [npm](https://docs.npmjs.com/) と [Node.js](http://nodejs.org/)
2. [Azure CLI](https://github.com/Azure/azure-xplat-cli)
3. A [Docker クライアント](https://docs.docker.com/installation/)

上記の順番でインストールすると、Ubuntu コンピューターでこのチュートリアルを行う準備は完了します (このチュートリアルは大部分が Debian などその他の dpkg ベースのディストリビューションと同じです。 条件や手順に追加する事項がありましたらコメントでお知らせください)。

## docker マシンを取得またはビルドする

開始する最も簡単な方法 **docker マシン** から直接該当のリリースをダウンロードするには、 [リリース共有](https://github.com/docker/machine/releases)します。 このチュートリアルでは、クライアント コンピューターが、x64 で Ubuntu を実行して、コンピューターのため、 **docker machine_linux-amd64** イメージを使用します。

ビルドすることも、 **docker マシン** 次の手順に従って、自分で [マシンに貢献して](https://github.com/docker/machine#contributing)します。 ビルドには 1 GB 以上の空き容量が必要になりますが、これでご希望の環境を自由にビルドできるようになります。

> [AZURE.NOTE] 作成できますが、 [シンボリック リンク](http://en.wikipedia.org/wiki/Symbolic_link) プラットフォームがこのチュートリアルのバージョンは直接動作を確認する非常に明確にバイナリを使用します。 結果コマンドではなくなど `docker-machine env` として、 **docker マシン** ドキュメントで示されたこのチュートリアルでは `docker-machine_linux-amd64 env` 代わりにします。 symlink でもバイナリ名ディレクトリでも自由に作成できますが、使用している名前を変更する場合は下記の手順で変更する必要がありますのでご注意ください。

<br />

>  どちらの方法を選択すると、直接コマンドラインでバイナリを呼び出すか、バイナリなど、パスに配置 **/usr/local/bin**します。 」と入力して実行可能とマークされていることを確認する `chmod +x` & lt;*`binaryName`*& gt;ここで & lt;*`binaryName`*& gt;Docker マシンの名前が実行可能ファイルです。 このチュートリアルでは **docker machine_linux-amd64**します。

## Docker、Machine、Azure の証明書とキー ファイルの作成

その Azure 証明書とキー ファイルを作成する必要がありますので、id およびアクセス許可と、そのことを確認する必要があります。 **docker マシン** 通信ご利用の Azure 仮想マシンを作成し、リモートでコンテナーを管理する必要があります。 これらのファイルがすでにディレクトリにある場合、おそらく Docker で使用したものですが、再利用できます。 テスト用推奨ただし、 **docker マシン** の個別のディレクトリに作成し、docker マシンを指すこともあります。

> [AZURE.NOTE] しようとしてしまう場合 **docker マシン** オーバー、および何度もあるか、同じ証明書とキー ファイルを再利用することを確認します。 **docker マシン** 一連のクライアント証明書の作成に、その同様作成検証できる `~/.docker/machine`です。 これらの証明書を別のコンピューターに移動する場合に移動する必要があります、 **docker マシン** 証明書のフォルダーも同様です。 使用しようとしている場合は、これによって **docker マシン** 別のプラットフォームでなどは、[だけをどのように動作を確認します。

Linux ディストリビューションの使用経験がある場合は、既にしたこれらのファイルは、コンピューターの特定の場所で使用できる、 [Docker HTTPS ドキュメントが、これらの手順を説明します](https://docs.docker.com/articles/https/)します。 以下に、この手順を最も簡単に行う方法を示します。

1. ローカル フォルダーを作成し、コマンドラインでそのフォルダーに移動して次を入力します。

        openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem
        openssl pkcs12 -export -out mycert.pfx -in mycert.pem -name "My Certificate"

    ここで、今後の使用に備えて、証明書のエクスポート パスワードを入力してキャプチャします。 次を入力します。

        openssl x509 -inform pem -in mycert.pem -outform der -out mycert.cer

2. 証明書の .cer ファイルを Azure にアップロードします。  [Azure クラシック ポータル](https://manage.windowsazure.com), 、クリックして **設定** 下図のとおりサービス エリアの左下にあります。

    ![][portalsettingsitem]

    クリックして **管理証明書**:

    ![][managementcertificatesitem]

     **アップロード** (ページの下部) ![][uploaditem] をアップロードする、 **mycert.cer** 前の手順で作成したファイルです。

3. 同じ **設定** ポータルで、ウィンドウをクリックします **サブスクリプション** し、次の手順で使用するために、VM を作成するときに使用するサブスクリプション ID をキャプチャします。 (サブスクリプション ID はコマンドラインで Azure CLI コマンド `azure account list` を使っても確認できます。アカウントに保持している各サブスクリプションの ID が表示されます)。

4. 使用して Azure で docker ホスト VM を作成、 **docker マシンを作成** コマンドです。 コマンドには、前の手順およびへのパスでキャプチャしたサブスクリプション ID が必要な **.pem** 手順 1. で作成したファイルです。 このトピックでは、Azure クラウド サービス (とご利用の VM) の名前として「machine-name」を使用していますが、ご自分で使う名前に変更し、VM の名前が必要なすべての手順でこのクラウド サービス名を使用してください (この例では、完全なバイナリ名が使用していることに注意してくださいと not、 **docker マシン** シンボリック リンクです)。

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

    > [AZURE.NOTE] VM が作成され、準備完了状態にするのには数分かかることがあります。 」と入力して、新しい Docker ホストの状態を確認するには、待っている間 `azure vm list` Vm が表示されるまで、Azure CLI を使用して、 **ReadyRole** 状態です。

5. Docker とマシンの環境変数をターミナル セッション向けに設定します。 フィードバックの最後の行は、すぐに実行ことを提案、 **env** docker クライアントを特定のマシンに直接使用するために必要な環境変数をエクスポートするコマンドです。

        $(docker-machine_linux-amd64 env machine-name)

    そうとは、その Docker を VM に接続するローカル docker クライアントを使用する特別なコマンドは必要ありません **マシン** を作成します。

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

> [AZURE.NOTE] このチュートリアルでは **docker マシン** 1 つの VM を作成します。 上記の手順を繰り返すことで、数の制限なく複数作成することも可能です。 使用する Vm と docker を切り替える最善の方法は、これを行う場合、 **env** を設定するコマンドをインライン、 **docker** 各コマンドの環境変数です。 例については、使用する **docker 情報** を異なる VM を入力できます `docker $(docker-machine env <VM name>) info` と **env** コマンドによってその VM で使用する docker 接続情報が設定されます。

## 設定完了。 Docker ハブから Docker とイメージを使ってリモートでいくつかのアプリケーションを実行

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

> [AZURE.NOTE] 使用してください、 **-p** したオプションを **docker** 、イメージに対するランダム ポートを割り当てると **-d** コンテナーがバック グラウンドで継続的に実行できるようにします。 (これを実行しないと、nginx を起動してもすぐにシャットダウンしますので、 確実に設定してください)。

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

インターネット経由で確認するには、ポート 80 で Azure VM に対してパブリック エンドポイントを作成し、それを nginx コンテナーのポートに対してマッピングします。 まず、使用して `docker ps -a` コンテナーを検索し、どのポートを見つける **docker** がコンテナーのポート 80 に割り当てられています。 (下記はポート情報のみを抽出しています。実際はより多くの情報が表示されます)。

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

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 次のステップ
移動して、 [Docker ユーザー ガイド](https://docs.docker.com/userguide/) し、Microsoft Azure で一部のアプリケーションを作成します。 または、遊んで [**docker** と swarm](https://github.com/docker/swarm) Azure](virtual-machines-docker-swarm) と docker と Azure での swarm の使用方法を参照してください。

<!--Image references-->
[nginx]: ./media/virtual-machines-docker-machine/nginxondocker.png
[portalsettingsitem]: ./media/virtual-machines-docker-machine/portalsettingsitem.png
[managementcertificatesitem]: ./media/virtual-machines-docker-machine/managementcertificatesitem.png
[uploaditem]: ./media/virtual-machines-docker-machine/uploaditem.png

<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md


