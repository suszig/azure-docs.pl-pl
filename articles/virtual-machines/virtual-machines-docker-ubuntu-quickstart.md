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


[Docker] を使用して開始する最も簡単な方法は、Azure Marketplace に移動を使用して、VM を作成する、 **Ubuntu Server の Docker** [MSOpenTech] と共に [Canonical] によって作成されたイメージ テンプレートです。 これは、Ubuntu Server VM を作成し、自動的にインストール、 [Docker VM 拡張機能](virtual-machines-docker-vm-extension.md) と共に、 **最新** Docker engine のプレインストールとでの実行、Azure です。

すぐに SSH で VM を接続し、そのまま直接 Docker で作業を始められます。
> [AZURE.NOTE]Azure Marketplace テンプレートで作成された VM は、リモートの Docker クライアントによる管理目的の Docker リモート API をホストしません。 この VM に Docker ホストをリモートで管理できるように、表示されること [HTTPS による Docker の実行](https://docs.docker.com/articles/https/) かの手順に従って [Azure クラシック ポータルから Docker VM 拡張機能を使用して](virtual-machines-docker-with-portal.md) または [Azure CLI から Docker VM 拡張機能を使用して](virtual-machines-docker-with-xplat-cli-install.md)します。 

Windows Azure の Docker VM を自動化する場合は、 [Docker ツールボックスをインストール](https://docs.docker.com/installation/windows/) Docker.exe を取得または [Chocolatey から](https://chocolatey.org/packages/docker)します。

## ポータルへのログオン

この作業は Azure アカウントさえあれば、問題ありません。 [も簡単に、無料版に取得](http://azure.microsoft.com/pricing/free-trial/)!

## Canonical と MSOpenTech の Docker イメージを使用した VM の作成

1. ログオンしたら、左下にある **[新規]** をクリックして新しい VM イメージを作成します。 すぐにバナーに適切なイメージが表示されます。

> ![バナーの Docker Ubuntu イメージの選択](./media/virtual-machines-docker-ubuntu-quickstart/CreateNewDockerBanner.png)

2. 表示されない場合は、イメージ ギャラリーで検索します。

> ![イメージ ギャラリーでのイメージの検索](./media/virtual-machines-docker-ubuntu-quickstart/DockerOnUbuntuServerMSOpenTech.png)

3. ユーザー名とパスワードのいずれかをインスタンスの内容の提供、 **.pub** ファイル (ssh rsa 形式) と証明書を使用して、SSH を有効にします。 (以下の図はユーザー名とパスワードの組み合わせの入力画面を示しています)。 次に、下部にある **[作成]** をクリックします。

> ![VM インスタンスの構成](./media/virtual-machines-docker-ubuntu-quickstart/CreateVMDockerUbuntuPwd.png)

4. 稼働するまで待ちます。 通常、長くはかかりません。

> ![ポータルでの Docker イメージの実行](./media/virtual-machines-docker-ubuntu-quickstart/DockerUbuntuRunning.png)

## SSH による接続と簡単な設定

次は楽しい作業です。 SSH ですぐに VM に接続できます。

> ![SSH との接続](./media/virtual-machines-docker-ubuntu-quickstart/SSHToDockerUbuntu.png)

Docker コマンドを発行する、この Azure VM で、既定の構成が必要である記憶を開始して * *`sudo`* *。

> ![イメージの取得](./media/virtual-machines-docker-ubuntu-quickstart/DockerPullSmallImages.png)


## 次のステップ

[Docker] の使用を開始することです。



[log on to the portal]: #logon 
[create a vm with the docker image from canonical and msopentech]: #createvm 
[connect with ssh and have fun]: #havingfun 
[next steps]: #next-steps 
[docker]: https://www.docker.com/ 
[busybox]: http://en.wikipedia.org/wiki/BusyBox 
[docker scratch image]: https://docs.docker.com/articles/baseimages/#creating-a-simple-base-image-using-scratch 
[canonical]: http://www.canonical.com/ 
[msopentech]: http://msopentech.com/ 

