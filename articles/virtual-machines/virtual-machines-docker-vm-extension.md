<properties
    pageTitle="Azure の Linux 用 Docker 仮想マシン拡張機能"
    description="Azure V仮想マシン拡張機能である Docker とコンテナーについて説明し、Azure CLI およびポータルで Docker コンテナーを作成するためのリソースを紹介します。"
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
    ms.date="10/21/2015"
    ms.author="rasquill"/>


# Azure の Linux 用 Docker 仮想マシン拡張機能

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[Docker](https://www.docker.com/) を使用する最も一般的な仮想化アプローチの 1 つ [Linux コンテナー](http://en.wikipedia.org/wiki/LXC) のアプリケーション データの分離と共有リソースでのコンピューティングの手段として仮想マシンではなく。 使用することができます、 [Azure Docker VM 拡張機能](https://github.com/Azure/azure-docker-extension/blob/master/README.md) に、 [Azure Linux エージェント](virtual-machines-linux-agent-user-guide.md) Azure でのアプリケーションのコンテナーの任意の数をホストする Docker VM を作成します。

このトピックの内容:

+ [Docker と Linux コンテナー]
+ [Azure で Docker VM 拡張機能を使用する方法]
+ [仮想マシンは Linux と Windows の拡張機能]

Docker 対応 VM の作成をすぐに開始するには、以下を参照してください。

+ [Docker VM 拡張機能から、Azure コマンド ライン インターフェイス (Azure CLI) を使用する方法]
+ [Azure クラシック ポータルで Docker VM 拡張機能を使用する方法]
+ [Azure Marketplace で Docker をすばやく開始する方法]

拡張機能とそのしくみの詳細については、次を参照してください。、 [Docker 拡張機能のユーザー ガイド](https://github.com/Azure/azure-docker-extension/blob/master/README.md)します。

## Docker と Linux コンテナー

[Docker](https://www.docker.com/) を使用する最も一般的な仮想化アプローチの 1 つ [Linux コンテナー](http://en.wikipedia.org/wiki/LXC) のデータの分離とでのコンピューティングの手段として仮想マシンではなく、リソースを共有し、ビルドまたはアプリケーションを迅速に構築し、の他の Docker コンテナーに配布するための他のサービスを提供します。

Docker と Linux コンテナーがない [ハイパーバイザー](http://en.wikipedia.org/wiki/Hypervisor) Windows HYPER-V などと [KVM](http://www.linux-kvm.org/page/Main_Page) linux (その他の例の多くはあります)。 ハイパーバイザーでは基になるオペレーティング システムが仮想化されるため、オペレーティング システム (*仮想マシン*と呼ばれる) 全体がハイパーバイザー内でアプリケーションのように実行されます。

Docker をはじめとする*コンテナー* アプローチでは、Linux カーネルが備えるプロセスとファイル システムの分離機能を使用し、カーネル機能の利用以外の部分では互いに分離されたコンテナーを構築することで、起動時間と処理やストレージのオーバーヘッドを大幅に軽減しています。

ハイパーバイザーと Linux コンテナーが備える機能の種類の違いに関する概要を次の表に示します。 機能の中には自社のアプリケーション ニーズによってどちらが望ましいかが異なるものがあります。

| 機能| ハイパーバイザー| コンテナー|
| :------------- |-------------| ----------- |
| プロセスの分離| ほぼ全部| root が取得されると、コンテナー ホストが危害を受けるおそれがある。|
| 必要なディスク上のメモリ| OS 全体とアプリ| アプリ要件のみ|
| 起動にかかる時間| 非常に長い: OS のブートとアプリの読み込み| 非常に短い: カーネルが既に実行されているため必要なのはアプリの起動のみ|
| コンテナーの自動化| OS やアプリによって大きく異なる| [Docker イメージ ギャラリー](https://registry.hub.docker.com/); 他のユーザー

コンテナーとその利点に関する概要についてを参照してください、 [Docker 高レベルのホワイト ボード](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard)します。

Docker としくみの詳細については、を参照してください [What is Docker?](https://www.docker.com/whatisdocker/)。

#### Docker と Linux コンテナーのセキュリティに関するベスト プラクティス

コンテナーはホスト コンピューターのカーネルへのアクセスを共有するため、悪意のあるコードが root を取得できた場合、ホスト コンピューターだけでなく他のコンテナーにもアクセスできる可能性があります。 既定の構成よりも厳密にコンテナー システムをセキュリティで保護する [Docker で推奨](https://docs.docker.com/articles/security/) 追加のグループ ポリシーを使用または [ロール ベース セキュリティ](http://en.wikipedia.org/wiki/Role-based_access_control) 同様など [SELinux](http://selinuxproject.org/page/Main_Page) または [AppArmor](http://wiki.apparmor.net/index.php/Main_Page), 、例では、だけでなく、コンテナーが付与されているカーネル機能できる限り減らしたりすること。 します。 また、インターネット上には、Docker のようなコンテナーを使用する際のセキュリティ対策について記載されたドキュメントが多数あります。

## Azure で Docker VM 拡張機能を使用する方法

Docker VM 拡張機能は、作成した VM インスタンスにインストールされるコンポーネントで、拡張機能自身が Docker エンジンをインストールし、VM とのリモート通信を管理します。 VM 拡張機能をインストールする方法は、2 つあります。1 つは管理ポータルを使用して VM を作成する方法、もう 1 つは Azure コマンド ライン インターフェイス (Azure CLI) から作成する方法です。

ポータルでは、互換性のある任意の Linux VM に Docker VM 拡張機能を追加できます (現時点では、7 月以降の Ubuntu 14.04 LTS イメージのみが Docker VM 拡張機能をサポートしています)。 一方、Azure CLI コマンド ラインを使用すれば、VM インスタンスを作成するときに、同時に Docker VM 拡張機能をインストールし、Docker 通信証明書を作成してアップロードできます。

Docker 対応 VM の作成をすぐに開始するには、以下を参照してください。

+ [Docker VM 拡張機能から、Azure コマンド ライン インターフェイス (Azure CLI) を使用する方法]
+ [Azure クラシック ポータルで Docker VM 拡張機能を使用する方法]

## Linux および Windows 向けの仮想マシン拡張機能

[Azure 用 Docker VM 拡張機能](https://github.com/Azure/azure-docker-extension/blob/master/README.md) は特別な動作を提供するいくつかの VM 拡張機能の 1 つと、開発中の詳細。 たとえば、いくつかの [Linux VM エージェント拡張機能](virtual-machines-linux-agent-user-guide.md) 機能では、変更し、セキュリティ機能やカーネルとネットワークの機能を含む、バーチャル マシンを管理することができます。 また VMAccess 拡張機能では、管理者のパスワードまたは SSH キーをリセットできます。

完全な一覧については、次を参照してください。 [Azure VM 拡張機能](virtual-machines-extensions-features.md)します。



[how to use the docker vm extension from the azure command-line interface (azure cli)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/ 
[how to use the docker vm extension with the azure classic portal]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-portal/ 
[how to get started quickly with docker in the azure marketplace]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-ubuntu-quickstart/ 
[docker and linux containers]: #Docker-and-Linux-Containers 
[how to use the docker vm extension with azure]: #How-to-use-the-Docker-VM-Extension-with-Azure 
[virtual machine extensions for linux and windows]: #Virtual-Machine-Extensions-For-Linux-and-Windows 

