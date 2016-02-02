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


# Azure での Linux 入門

このトピックでは、Azure クラウドで Linux 仮想マシンを使用するさまざまな局面について概説します。 Linux 仮想マシンのデプロイは、ギャラリー内に存在するイメージを使用する際に簡単に使用できるプロセスです。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

## 認証: ユーザー名、パスワード、SSH 鍵。

Azure クラシック ポータルを使用して Linux 仮想マシンを作成すると、ユーザー名、パスワードまたは SSH 公開キーの入力が求められます。Azure で Linux 仮想マシンをデプロイするユーザー名を選択する場合、root など、既に仮想マシン内に存在するシステム アカウント (UID <100) の名前は許可されない、という制約があります。


 - を参照してください [Linux を実行する仮想マシンを作成する](virtual-machines-linux-tutorial.md)
 - を参照してください [Azure 上の Linux で SSH を使用する方法](../linux-use-ssh-key.md)


## スーパー ユーザーの特権を使用して取得する `sudo`

Azure での仮想マシン インスタンスをデプロイする際に指定したユーザー アカウントは、特権を持つアカウントです。 このアカウントが root (スーパー ユーザー アカウント) を使用する特権を昇格できる Azure Linux エージェントによって構成されている、 `sudo` ユーティリティです。 このユーザー アカウントを使用してログインすると、コマンド構文を使用して、root としてコマンドを実行できるようになります。

    # sudo <COMMAND>

**sudo -s** を使用して root シェルを取得することもできます。

- を参照してください [Azure で Linux 仮想マシンで root 権限の使用](virtual-machines-linux-use-root-privileges.md)


## ファイアウォールの構成

Azure では、Azure クラシック ポータルで指定されたポートに接続を制限する受信パケット フィルターが用意されています。 既定では、許可されている唯一のポートは SSH です。 Azure クラシック ポータルでエンドポイントを構成することで、Linux 仮想マシンの追加ポートへのアクセスを設定できます。

 - 参照: [仮想マシンに対してエンドポイントを設定する方法](virtual-machines-set-up-endpoints.md)

既定では、Azure ギャラリー内の Linux のイメージを使用して*iptables* ファイアウォールを有効にすることはできません。 必要があれば、このファイアウォールにフィルターを追加するように構成することはできます。


## ホスト名の変更

最初に Linux イメージのインスタンスをデプロイするときに、仮想マシンのホスト名を提供するように求められます。 仮想マシンが実行されると、このホスト名はプラットフォーム DNS サーバーに公開されます。相互に接続された複数の仮想マシンがホスト名を使用して IP アドレス検索を実行できるようになります。

仮想マシンがデプロイされた後でホスト名を変更する場合は、次のコマンドを使用します。

    # sudo hostname <newname>

Azure Linux エージェントには、この名前の変更を自動的に検出する機能、この変更を保持するように仮想マシンを正しく構成する機能、この変更をプラットフォーム DNS サーバーに公開する機能があります。

 - [Azure Linux エージェント ユーザー ガイド](virtual-machines-linux-agent-user-guide.md)

### Cloud-Init

**Ubuntu** イメージと **CoreOS** イメージでは、仮想マシンをブートストラップするための追加機能を提供する Azure の cloud-init を使用します。

 - [カスタム データを挿入する方法](virtual-machines-how-to-inject-custom-data.md)
 - [カスタム データと Cloud-init on Microsoft Azure](http://azure.microsoft.com/blog/2014/04/21/custom-data-and-cloud-init-on-windows-azure/)
 - [Cloud-init を使用した Azure スワップ パーティションを作成します。](https://wiki.ubuntu.com/AzureSwapPartitions)
 - [Azure で CoreOS を使用する方法](virtual-machines-linux-coreos-how-to.md)


## 仮想マシン イメージのキャプチャ

Azure には、既存の仮想マシンの状態をイメージにキャプチャする機能があります。このイメージは、後で追加の仮想マシンのインスタンスをデプロイするために使用できます。 Azure Linux エージェントは、プロビジョニング中に実行したカスタマイズをロールバックするために使用することもできます。 仮想マシンをイメージとしてキャプチャする手順は次のとおりです。

1. **waagent -deprovision** を実行してプロビジョニング中のカスタマイズを元に戻します。 または、**waagent -deprovision+user** を実行して、プロビジョニング中に指定したユーザー アカウントとすべての関連データを削除します。

2. 仮想マシンをシャットダウン/電源オフします。

3. Azure クラシック ポータルで *[キャプチャ]* をクリックするか、Powershell ツールまたは CLI ツールを使用して仮想マシンをイメージとしてキャプチャします。

 - 参照: [をテンプレートとして使用するための Linux 仮想マシンをキャプチャする方法](virtual-machines-linux-capture-image.md)


## ディスクの接続

各仮想マシンには、一時的なローカル *リソース ディスク*が接続されています。 リソース ディスク上のデータには、再起動を行った場合の持続性がないため、仮想マシン上で実行されているアプリケーションやプロセスは、多くの場合、このディスクを過渡的および**一時的な**ストレージとして使用します。 また、オペレーティング システムがページ ファイルやスワップ ファイルを格納する場合にも使用されます。

Linux では通常、リソース ディスクは Azure Linux エージェントによって管理され、**/mnt/resource** (または Ubuntu イメージでは **/mnt**) に自動的にマウントされます。

>[AZURE.NOTE] リソース ディスクは **一時ディスク**であるため、仮想マシンが再起動されると削除され再フォーマットされることに注意してください。

Linux ではカーネルによって、データ ディスクをという可能性があります `/デベロッパー/sdc`, 、ユーザーがパーティション分割し、書式を設定し、そのリソースをマウントする必要があります。 このチュートリアルで詳しく説明については: [にデータ ディスクを仮想マシンに接続する方法](virtual-machines-linux-how-to-attach-disk.md)します。

 - **関連項目:** [Linux でのソフトウェア RAID を構成する](virtual-machines-linux-configure-raid.md)





