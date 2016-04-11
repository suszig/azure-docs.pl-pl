<properties
    pageTitle="Azure での Linux 入門 | Microsoft Azure"
    description="Azure での Linux 仮想マシンの使用に関する詳細です。"
    services="virtual-machines"
    documentationCenter="python"
    authors="szarkos"
    manager="timlt"
    editor=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/13/2015"
    ms.author="szark"/>

#Azure での Linux 入門

このトピックでは、Azure クラウドで Linux 仮想マシンを使用するさまざまな局面について概説します。 Linux 仮想マシンのデプロイは、ギャラリー内に存在するイメージを使用する際に簡単に使用できるプロセスです。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## 認証: ユーザー名、パスワード、SSH 鍵。

Azure クラシック ポータルを使用して Linux 仮想マシンを作成すると、ユーザー名、パスワードまたは SSH 公開キーの入力が求められます。 Azure で Linux 仮想マシンをデプロイするユーザー名を選択する場合、root など、既に仮想マシン内に存在するシステム アカウント (UID <100) の名前は許可されない、という制約があります。


 - 参照してください [Linux を実行する仮想マシンの作成](virtual-machines-linux-tutorial.md)
 - 参照してください [Azure 上の Linux で SSH を使用する方法](../linux-use-ssh-key.md)


## `sudo` を使用したスーパーユーザー権限の取得

Azure での仮想マシン インスタンスをデプロイする際に指定したユーザー アカウントは、特権を持つアカウントです。 このアカウントは、Azure Linux エージェントによって `sudo` ユーティリティを使用して権限を root (スーパーユーザー アカウント) に昇格できるように構成されます。 このユーザー アカウントを使用してログインすると、コマンド構文を使用して、root としてコマンドを実行できるようになります。

    # sudo <COMMAND>

使用して root シェルを必要に応じて取得 **sudo-s**します。

- 参照してください [Azure で Linux 仮想マシンで root 権限の使用](virtual-machines-linux-use-root-privileges.md)


## ファイアウォールの構成

Azure では、Azure クラシック ポータルで指定されたポートに接続を制限する受信パケット フィルターが用意されています。 既定では、許可されている唯一のポートは SSH です。 Azure クラシック ポータルでエンドポイントを構成することで、Linux 仮想マシンの追加ポートへのアクセスを設定できます。

 - 参照: [仮想マシンに対してエンドポイントを設定する方法](virtual-machines-set-up-endpoints.md)

Azure ギャラリー内の Linux のイメージを有効にしない、 *iptables* 既定ではファイアウォールです。 必要があれば、このファイアウォールにフィルターを追加するように構成することはできます。


## ホスト名の変更

最初に Linux イメージのインスタンスをデプロイするときに、仮想マシンのホスト名を提供するように求められます。 仮想マシンが実行されると、このホスト名はプラットフォーム DNS サーバーに公開されます。相互に接続された複数の仮想マシンがホスト名を使用して IP アドレス検索を実行できるようになります。

仮想マシンがデプロイされた後でホスト名を変更する場合は、次のコマンドを使用します。

    # sudo hostname <newname>

Azure Linux エージェントには、この名前の変更を自動的に検出する機能、この変更を保持するように仮想マシンを正しく構成する機能、この変更をプラットフォーム DNS サーバーに公開する機能があります。

 - [Azure Linux エージェント ユーザー ガイド](virtual-machines-linux-agent-user-guide.md)

### Cloud-Init
**Ubuntu** と **CoreOS** イメージは、cloud-init を仮想マシンをブートス トラップするその他の機能を提供する Azure を使用します。

 - [カスタム データを挿入する方法](virtual-machines-how-to-inject-custom-data.md)
 - [Microsoft Azure のカスタム データと Cloud-Init](http://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Cloud-Init を使用した Azure スワップ パーティションの作成](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Azure 上で CoreOS を使用する方法](virtual-machines-linux-coreos-how-to.md)


## 仮想マシン イメージのキャプチャ

Azure には、既存の仮想マシンの状態をイメージにキャプチャする機能があります。このイメージは、後で追加の仮想マシンのインスタンスをデプロイするために使用できます。 Azure Linux エージェントは、プロビジョニング中に実行したカスタマイズをロールバックするために使用することもできます。 仮想マシンをイメージとしてキャプチャする手順は次のとおりです。

1. 実行 **waagent-プロビジョニング解除** プロビジョニング中のカスタマイズを元に戻す。 または **waagent-プロビジョニング解除 + ユーザー** を必要に応じて、プロビジョニングや関連付けられているすべてのデータの中に指定したユーザー アカウントを削除します。

2. 仮想マシンをシャットダウン/電源オフします。

3. クリックして *キャプチャ* Azure クラシック ポータルまたは使用で、Powershell ツールまたは CLI ツールをバーチャル マシンをイメージとしてキャプチャします。

 - 参照: [をテンプレートとして使用するための Linux 仮想マシンをキャプチャする方法](virtual-machines-linux-capture-image.md)


## ディスクの接続

各仮想マシンは一時的なローカル *リソース ディスク* アタッチします。 アプリケーションおよび過渡的仮想マシンで実行中のプロセスで多くの場合、使用リソース ディスク上のデータがないため永続的な再起動を行った、および **一時** データの格納します。 また、オペレーティング システムがページ ファイルやスワップ ファイルを格納する場合にも使用されます。

Linux では、リソース ディスクは通常、Azure Linux エージェントによって管理し、に自動的にマウント **/mnt/retention/リソース** (または **/mnt** Ubuntu イメージで)。


>[AZURE.NOTE] リソース ディスクは、 **一時** ディスク、および削除され、VM が再起動したときに再フォーマット可能性があります。

Linux では、データ ディスクはカーネルによって `/dev/sdc` という名前が付けられる場合があり、ユーザーはこのリソースをパーティション分割し、フォーマットした上で、マウントする必要があります。 このチュートリアルで詳しく説明については: [にデータ ディスクを仮想マシンに接続する方法](virtual-machines-linux-how-to-attach-disk.md)します。

 - **関連項目:** [Linux でのソフトウェア RAID の構成](virtual-machines-linux-configure-raid.md)

