<properties
    pageTitle="Ubuntu の Docker VM イメージから Docker をすばやく使用する方法"
    description="Azure イメージ ギャラリーから直接、すばやく Ubuntu Server で Docker を使用する方法について詳しく説明します"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure"
    ms.date="10/04/2015"
    ms.author="rasquill"/>

# Azure Marketplace で Docker をすばやく開始する方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。
 

使用を開始する最も速い方法 [Docker] は、Azure Marketplace に移動しを使用して VM を作成、 **Ubuntu Server の Docker** によって作成されたイメージ テンプレート [Canonical] と共に [MSOpenTech]します。 これは、Ubuntu Server VM を作成し、自動的にインストール、 [Docker VM 拡張機能](virtual-machines-docker-vm-extension.md) と共に、 **最新** Docker engine のプレインストールとでの実行、Azure です。  

すぐに SSH で VM を接続し、そのまま直接 Docker で作業を始められます。

> [AZURE.NOTE]Azure Marketplace テンプレートで作成された VM は、リモートの docker クライアントによる管理のための Docker リモート API をホストしていません。 この VM に Docker ホストをリモートで管理できるように、表示されること [HTTPS による Docker の実行](https://docs.docker.com/articles/https/) かの手順に従って [Azure クラシック ポータルから Docker VM 拡張機能を使用して](virtual-machines-docker-with-portal.md) または [Azure CLI から Docker VM 拡張機能を使用して](virtual-machines-docker-with-xplat-cli-install.md)します。 
<!-- -->
Windows Azure の Docker VM を自動化する場合は、 [install the Docker Toolbox](https://docs.docker.com/installation/windows/) Docker.exe を取得または [from Chocolatey](https://chocolatey.org/packages/docker)します。

## ポータルへのログオン

この作業は Azure アカウントさえあれば、問題ありません。 [無料版を簡単にも取得](http://azure.microsoft.com/pricing/free-trial/)!

## Canonical と MSOpenTech の Docker イメージを使用した VM の作成

1. これでログオンしている **新規** 、左下にある新しい VM イメージを作成します。 すぐにバナーに適切なイメージが表示されます。

> ![バナーの Docker Ubuntu イメージの選択](./media/virtual-machines-docker-ubuntu-quickstart/CreateNewDockerBanner.png)

2. 表示されない場合は、イメージ ギャラリーで検索します。

> ![イメージ ギャラリーでのイメージの検索](./media/virtual-machines-docker-ubuntu-quickstart/DockerOnUbuntuServerMSOpenTech.png)

3. ユーザー名とパスワードのいずれかをインスタンスの内容の提供、 **.pub** ファイル (ssh rsa 形式) と証明書を使用して、SSH を有効にします。 (以下の図はユーザー名とパスワードの組み合わせの入力画面を示しています)。キーを押します **作成** 下部にあります。

> ![VM インスタンスの構成](./media/virtual-machines-docker-ubuntu-quickstart/CreateVMDockerUbuntuPwd.png)

4. 稼働するまで待ちます。 通常、長くはかかりません。

> ![ポータルでの Docker イメージの実行](./media/virtual-machines-docker-ubuntu-quickstart/DockerUbuntuRunning.png)

## SSH による接続と簡単な設定

次は楽しい作業です。 SSH ですぐに VM に接続できます。

> ![SSH との接続](./media/virtual-machines-docker-ubuntu-quickstart/SSHToDockerUbuntu.png)

Docker コマンドを発行する、この Azure VM で、既定の構成が必要である記憶を開始および **`sudo`**:

> ![イメージの取得](./media/virtual-machines-docker-ubuntu-quickstart/DockerPullSmallImages.png)

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 次のステップ

使用を開始するしようとして [Docker]!

<!--Anchors-->
[Log on to the Portal]: #logon
[Create a VM with the Docker Image from Canonical and MSOpenTech]: #createvm
[Connect with SSH and Have Fun]: #havingfun
[Next steps]: #next-steps


[Docker]: https://www.docker.com/
[BusyBox]: http://en.wikipedia.org/wiki/BusyBox
[Docker scratch image]: https://docs.docker.com/articles/baseimages/#creating-a-simple-base-image-using-scratch
[Canonical]: http://www.canonical.com/
[MSOpenTech]: http://msopentech.com/
 

