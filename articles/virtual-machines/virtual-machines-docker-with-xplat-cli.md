<properties
    pageTitle="Azure での Linux 用 Docker VM 拡張機能の使用"
    description="Docker と Azure 仮想マシン拡張機能について説明し、Azure CLI を使用してコマンド ラインから Azure 上に Docker ホストである仮想マシンをプログラムで作成する方法を示します。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/22/2015"
    ms.author="rasquill"/>


# Azure コマンド ライン インターフェイス (Azure CLI) での Docker VM 拡張機能の使用

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。



このトピックでは、任意のプラットフォーム上のサービス管理 (asm) モードの Azure CLI から Docker VM 拡張機能を使用して VM を作成する方法について説明します。 [Docker](https://www.docker.com/) を使用する最も一般的な仮想化アプローチの 1 つ [Linux コンテナー](http://en.wikipedia.org/wiki/LXC) のデータの分離と共有リソースでのコンピューティングの手段として仮想マシンではなく。 Docker VM 拡張機能を使用することができます、 [Azure Linux エージェント](virtual-machines-linux-agent-user-guide.md) Azure でのアプリケーションのコンテナーの任意の数をホストする Docker VM を作成します。 コンテナーとその利点に関する概要についてを参照してください、 [Docker 高レベルのホワイト ボード](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard)します。

+ [Azure で Docker VM 拡張機能を使用する方法]
+ [仮想マシンは Linux と Windows の拡張機能]
+ [コンテナーと Azure 用のコンテナー管理リソース]
+ [次のステップ]

## Azure で Docker VM 拡張機能を使用する方法

Azure で Docker VM 拡張機能を使用するには、バージョンをインストールする必要があります、 [Azure コマンド ライン インターフェイス](https://github.com/Azure/azure-sdk-tools-xplat) (Azure CLI) 0.8.6 (この記事の執筆は現在のバージョンは 0.8.10 です)。 Azure CLI は Mac、Linux、Windows のいずれにもインストールできます。



Azure 上で Docker を使用するためのプロセスはシンプルです。

+ Azure CLI とその依存関係を、Azure を制御するコンピューターにインストールする (Windows の場合は、仮想マシンとして実行される Linux ディストリビューション)。
+ Azure CLI Docker コマンドを使用して、Azure に VM Docker ホストを作成する。
+ ローカルの Docker コマンドを使用して、Azure 上の Docker VM 内の Docker コンテナーを管理する。


### Azure コマンド ライン インターフェイス (Azure CLI) のインストール

インストールして、Azure cli の次を参照してください。 [Azure コマンド ライン インターフェイスのインストール方法](../xplat-cli-install.md)します。 インストールを確認するには、次のように入力します。 `azure` に使用できる基本的なコマンドを一覧表示するコマンド プロンプトで、しばらく Azure CLI の ASCII アートが表示されます。 インストールが正常である場合を入力する必要がありますできる `azure ヘルプ vm` "docker"が表示されているコマンドのいずれかを確認します。
> [AZURE.NOTE] Docker は、Windows のセットアップ プログラムは [Boot2Docker](https://docs.docker.com/installation/windows/), 、docker ホストとして Azure Vm と操作に使用できる、docker クライアントの作成を自動化するのにも使用することができます。

### Azure アカウントへの Azure CLI の接続

Azure CLI を使用する前に、プラットフォーム上で Azure アカウントの資格情報を Azure CLI に関連付ける必要があります。 セクションでは、 [、Azure サブスクリプションに接続する方法](../xplat-cli-connect.md) ダウンロードしてインポートする方法について説明して **.publishsettings** ファイルまたは Azure cli コマンドラインを組織 id に関連付けます。
> [AZURE.NOTE] 使用する認証方法によって動作に若干の違いがあるため、必ず前述のドキュメントをよく読んで、機能の違いを理解してください。

### Docker のインストールと Azure 用 Docker VM 拡張機能の使用

次の [Docker のインストール手順](https://docs.docker.com/installation/#installation) Docker をコンピューターにローカルにインストールします。

Azure 仮想マシンで Docker を使用する、VM に使用する Linux イメージにいること、 [Azure Linux VM エージェント](virtual-machines-linux-agent-user-guide.md) をインストールします。 現在、このエージェントを提供するイメージは次の 2 種類のみです。

+ Azure イメージ ギャラリーで入手できる Ubuntu イメージ

+ インストールされ、構成済みの Azure Linux VM エージェントで作成したカスタム Linux イメージ 参照してください [Azure Linux VM エージェント](virtual-machines-linux-agent-user-guide.md) 詳細については、Azure VM エージェント カスタム Linux VM を作成する方法です。

### Azure イメージ ギャラリーの使用

bash またはターミナル セッションから、次の Azure CLI コマンドを入力して、VM ギャラリーで最新の Ubuntu イメージを見つけます。

`azure vm image list | grep Ubuntu-14_04`

など、イメージ名のいずれかを選択して `b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB`, 、そのイメージを使用して新しい VM を作成する次のコマンドを使用します。

```
azure vm docker create -e 22 -l "West US" <vm-cloudservice name> "b39f27a8b8c64d52b05eac6a62ebad85__Ubuntu-14_04-LTS-amd64-server-20140724-en-us-30GB" <username> <password>
```

各値の説明:

+ *<vm-cloudservice name>* は、Azure で Docker コンテナーのホスト コンピューターになる VM の名前です。

+  *&lt;username&gt;* は、VM の既定の root ユーザーのユーザー名です。

+ *&lt;password&gt;* は、Azure の複雑さの基準を満たした *username* アカウントのパスワードです。

> [AZURE.NOTE] 現時点では、パスワードが、少なくとも 8 文字以上含める必要があります、1 つの小文字と大文字、数字、および次の文字のいずれかなどの特殊文字が含まれて: `! @# $% ^ & + =`します。) を 1 つずつ含める必要があります。ピリオドは特殊文字ではありません。

コマンドが正常に実行されると、使用した引数とオプションに応じて、以下のような応答が表示されます。

![](./media/virtual-machines-docker-with-xplat-cli/dockercreateresults.png)
> [AZURE.NOTE] バーチャル マシンの作成は、数分かかることができますが、後でプロビジョニングされました (状態の値は `ReadyRole`)、Docker デーモン (Docker サービス) が起動し、Docker コンテナー ホストに接続することができます。

Azure 上に作成した Docker VM をテストするために、次のコマンドを入力します。

`docker--tls-h tcp://< vm の名前にする-使用 >. cloudapp.net:2376 情報`

ここで *< vm-名前--使用した >* への呼び出しで使用した仮想マシンの名前を指定 `azure vm の docker 作成`します。 以下のような応答が表示されます。Azure 上で Docker ホスト VM が稼働し、コマンドの入力を待機していることが示されています。

これで、情報を取得する docker クライアントを使用して接続しようとすることができます (を mac の場合など、いくつかの Docker クライアント設定では、使用する必要あります `sudo`)。

    sudo docker --tls -H tcp://testsshasm.cloudapp.net:2376 info
    Password:
    Containers: 0
    Images: 0
    Storage Driver: devicemapper
    Pool Name: docker-8:1-131781-pool
    Pool Blocksize: 65.54 kB
    Backing Filesystem: extfs
    Data file: /dev/loop0
    Metadata file: /dev/loop1
    Data Space Used: 1.821 GB
    Data Space Total: 107.4 GB
    Data Space Available: 28 GB
    Metadata Space Used: 1.479 MB
    Metadata Space Total: 2.147 GB
    Metadata Space Available: 2.146 GB
    Udev Sync Supported: true
    Deferred Removal Enabled: false
    Data loop file: /var/lib/docker/devicemapper/devicemapper/data
    Metadata loop file: /var/lib/docker/devicemapper/devicemapper/metadata
    Library Version: 1.02.77 (2012-10-15)
    Execution Driver: native-0.2
    Logging Driver: json-file
    Kernel Version: 3.19.0-28-generic
    Operating System: Ubuntu 14.04.3 LTS
    CPUs: 1
    Total Memory: 1.637 GiB
    Name: testsshasm
    WARNING: No swap limit support

すべてが機能していることを確認するために、VM で Docker 拡張機能を調査できます。

    azure vm extension get testsshasm
    info: Executing command vm extension get
    + Getting virtual machines
    data: Publisher Extension name ReferenceName Version State
    data: -------------------- --------------- ------------------------- ------- ------
    data: Microsoft.Azure.E... DockerExtension DockerExtension 1.* Enable
    info: vm extension get command OK

### Docker ホスト VM の認証

Docker VM を作成するだけでなく、 `azure vm の docker 作成` コマンドは、Docker クライアント コンピューターは、HTTPS を介して Azure コンテナー ホストに接続を許可するように必要な証明書も自動的に作成し、必要に応じて、クライアントとホストの両方のマシンに証明書も格納します。 これ以降に実行しようとすると、既存の証明書が再利用され、新しいホストと共有されます。

既定では、証明書に `~/.docker`, 、Docker は、ポート上で実行するように構成されます **2376年**します。 別のポートやディレクトリを使用したいかどうかは、次のいずれかを使用することがあります `azure vm の docker 作成` コマンド ライン オプションを Docker コンテナーを構成するのには、クライアント接続に別のポートや別の証明書を使用する VM をホストします。

```
-dp, --docker-port [port]              Port to use for docker [2376]
-dc, --docker-cert-dir [dir]           Directory containing docker certs [.docker/]
```

リッスンし生成される証明書を使用して指定されたポートでクライアント接続を認証するホストの Docker デーモンが構成されている、 `azure vm の docker 作成` コマンドです。 クライアント コンピューターには、Docker ホストにアクセスできるように、これらの証明書が配置されている必要があります。
> [AZURE.NOTE] ネットワークに接続されたホストにこれらの証明書がない場合は、コンピューターに接続できる第三者に対して脆弱になります。 既定の構成を変更する前に、コンピューターやアプリケーションに対するリスクについてよく理解してください。

## 次のステップ

[Docker ユーザー ガイド] に移動し、Docker VM の使用準備ができました。 新しいポータルで Docker 対応 VM を作成するには、[ポータルで Docker VM 拡張機能を使用する方法] を参照してください。





[subheading 1]: #subheading-1 
[subheading 2]: #subheading-2 
[subheading 3]: #subheading-3 
[next steps]: #next-steps 
[how to use the docker vm extension with azure]: #How-to-use-the-Docker-VM-Extension-with-Azure 
[virtual machine extensions for linux and windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows 
[container and container management resources for azure]: #Container-and-Container-Management-Resources-for-Azure 
[5]: ./media/markdown-template-for-new-articles/octocats.png 
[6]: ./media/markdown-template-for-new-articles/pretty49.png 
[7]: ./media/markdown-template-for-new-articles/channel-9.png 
[link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-tutorial.md 
[link 2 to another azure.microsoft.com documentation topic]: ../web-sites-custom-domain-name.md 
[link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md 
[how to use the docker vm extension with the portal]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/ 
[docker user guide]: https://docs.docker.com/userguide/ 

