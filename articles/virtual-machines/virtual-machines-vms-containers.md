<properties 
    pageTitle="Virtual Machines とコンテナー | Microsoft Azure" 
    description="Virtual Machines、Docker、および Linux コンテナーの概要を説明するとともに、それらのグループを Azure で使用するための方法と、各方法のメリットについて説明します。また、どの方法がどのようなシナリオに適しているかについても説明します。" 
    services="virtual-machines" 
    documentationCenter="virtual-machines" 
    authors="squillace" 
    manager="timlt"
    tags="azure-resource-manager,azure-service-management" 
/>
    

<tags 
    ms.service="virtual-machines" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="infrastructure" 
    ms.workload="infrastructure" 
    ms.date="12/14/2015" 
    ms.author="rasquill" 
/>

<!--The next line, with one pound sign at the beginning, is the page title--> 
# Virtual Machines とコンテナーが Azure にもたらすメリット

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]
 

Azure は、物理コンピューター ハードウェアをエミュレートした仮想マシンを利用して、ソフトウェア デプロイメントの移動を迅速化し、物理ハードウェアよりも格段に効率的なリソース統合を可能にするすばらしいクラウド ソリューションです。 過去数年で主に感謝、 [Docker](https://www.docker.com) コンテナー アプローチと docker エコシステムが業界では、Linux コンテナー テクノロジは大幅のあり方を開発および配布されたソフトウェアを管理することができます。 コンテナー内のアプリケーション コードは、ホストの Azure VM からだけでなく、同じ VM 上にあるその他のコンテナーからも分離されるため、Azure VM がもたらす開発やデプロイメントのアジリティをアプリケーション レベルでさらに高めることにつながります。

**これはもはやニュースです。** *新しい* ニュースがメリットを Azure で、さらに多くの Docker 適合度。

- [多く](virtual-machines-docker-with-xplat-cli-install.md) [異なる](virtual-machines-docker-with-portal.md) [方法](virtual-machines-docker-ubuntu-quickstart.md) に [Docker ホストを作成](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu) 状況に応じて、コンテナーの
- [Azure リソース マネージャー](resource-group-overview.md) と [リソース グループ テンプレート](resource-group-authoring-templates.md) を展開して、複雑な分散アプリケーションの更新を簡略化するには
- 自社製かオープン ソースかを問わず、さまざまな構成管理ツールを統合できる

Vm や Linux をプログラムで作成できるため、コンテナー Azure では、使用することも VM とコンテナー *オーケストレーション* ツール仮想マシン (Vm) のグループを作成し、両方の Linux コンテナー内にアプリケーションを展開してすぐに [Windows コンテナー](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)します。 

この記事では、これらの技術の概念について俯瞰的に説明とともに、 Azure でのコンテナーとクラスターの使用に関連する詳細情報、チュートリアル、および製品へのリンクを紹介します。 ご存知してリンクのみが必要であってもここ [のコンテナーを操作するためのツール](#tools-for-working-with-containers)します。

## 仮想マシンとコンテナーの違い

によって提供される隔離されたハードウェア仮想化環境内の仮想マシンの実行、 [ハイパーバイザー](http://en.wikipedia.org/wiki/Hypervisor)します。 Azure で、 [仮想マシン](http://azure.microsoft.com/services/virtual-machines/) サービスによってすべての: オペレーティング システムを選択して、思いどおり & mdash; を実行するように構成またはカスタム VM イメージをアップロードしてにだけ、仮想マシンが作成されます。 Virtual Machines は市場で長年使用され、その価値を広く認められているテクノロジであり、オペレーティング システムの管理やアプリケーションの構成を行うためのツールも多数提供されています。 仮想マシンで実行されるプログラムはホスト オペレーティング システムからは見えず、仮想マシン内で実行されているアプリケーションやユーザーの観点から見ると、仮想マシンは自立した物理コンピューターのように見えます。

[Linux コンテナー](http://en.wikipedia.org/wiki/LXC)& mdash; を含む、作成、docker ツールを使用してホストしその他のアプローチと mdash がある; を必要としたりしないで、ハイパーバイザーを使用して分離を提供します。 代わりにコンテナー ホストによって、特定のカーネル機能と独自の分離型ファイル システムだけが (最小限の単位で) コンテナー (およびそのアプリケーション) に公開されます。 コンテナーの内部で実行されるアプリケーションから見ると、コンテナーは独立したオペレーティング システム インスタンスのように見えます。 内部のアプリケーションからは、コンテナーの外部にあるその他のプロセスやリソースは見えません。 

この分離型の実行モデルでは、Docker ホスト コンピューターのカーネルが共有され、コンテナーのディスク要件にオペレーティング システム全体を含める必要がなくなるため、コンテナーの起動時間と、必要なディスク ストレージのオーバーヘッドがかなり縮小されます。 

実に合理的です。

Windows コンテナーは、Linux コンテナーと同じメリットを Windows 上のアプリケーションに提供するためのものです。 Windows コンテナーは、Docker イメージのフォーマットと Docker API をサポートしていますが、PowerShell を使用して管理することもできます。 2 つのコンテナーのランタイムは、Windows コンテナー、Windows コンテナー、Hyper-V コンテナーで使用可能です。 Hyper-V コンテナーは、非常に最適化された仮想マシンで各コンテナーをホストすることで分離のレイヤーを追加します。 については Windows コンテナーの詳細を参照してください [Windows コンテナーに関する]( https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)します。 Azure で Windows のコンテナーを参照してください、 [Windows コンテナー Azure クイック スタート]( https://msdn.microsoft.com/virtualization/windowscontainers/quick_start/azure_setup)します。 

これも大きな魅力と言えます。

### 話がうますぎるとお思いですか。

確かに、& mdash; なしとします。 その他のテクノロジのように、コンテナーは魔法のように消去しない離れたの分散アプリケーションで必要なすべてのハード作業します。 しかし、次のようなメリットをもたらしてくれることは事実です。

- アプリケーション コードをすばやく開発し、広範囲に共有できる
- テストにかかる時間を短縮し、その信頼性を高めることができる
- デプロイにかかるを短縮し、その信頼性を高めることができる

ただし、コンテナーを実行するにはコンテナー ホスト (オペレーティング システム) が必要です。Azure でいうところの Azure 仮想マシンです。 コンテナーのメリットを活用するには、コンテナーをホストする VM インフラストラクチャが必要なのです。しかし、コンテナーの利点は実行するための VM を選ばないという点にあります (コンテナーの実行環境として Linux と Windows のどちらが適しているか、などといったことは重要になりますが)。 

## コンテナーのメリット

のに多くの点でたいへん役立ちますが、奨励 & mdash; のように [Azure Cloud Services](http://azure.microsoft.com/services/cloud-services/) と [Azure Service Fabric](service-fabric-overview.md)& mdash; 単一サービスの [マイクロ サービス] の作成-指向の分散アプリケーション、アプリケーションの設計は、ベースより大規模で結合された以降はよほどのコンポーネントではなく、小さな、構成可能なその他のパーツです。 

これは、必要な場所に必要なタイミングで VM をレンタルできる、Azure のようなパブリック クラウド環境においては特に有効です。 分離性を確保し、デプロイメントを迅速化し、便利なオーケストレーション ツールを活用できるだけでなく、アプリケーション インフラストラクチャをより効率的に管理することができます。 

たとえば現在、高可用性を備えた大規模な分散アプリケーションのデプロイメントを、9 つの Azure VM で構成しているとします。 このアプリケーションのコンポーネントをコンテナーでデプロイすれば、わずか 4 つの VM を使用して、20 個のコンテナー内にアプリケーション コンポーネントをデプロイし、冗長性と負荷分散を実現することができます。 

これは単なる例に過ぎませんが、組織のシナリオに合わせてこのようなデプロイメントを行えば、Azure VM ではなくコンテナーの追加によって処理パワーを調整し、残りの CPU パワーをより効率的に使用することができます。

またコンテナーは、マイクロサービス アプローチ以外にもさまざまなシナリオに適用できます。ここからは、マイクロサービスとコンテナーのメリットについてより具体的に説明していきます。 

### 開発者にとってのコンテナーの利点

コンテナー テクノロジが新たなメリットをもたらすことは一般的な観点からも明らかですが、事業部単位でもさまざまなメリットが期待できます。 Docker コンテナーの例で見てみましょう。 このトピックはないについて深く掘り下げる Docker 今 (読み取り [What is Docker?](https://www.docker.com/whatisdocker/) そのストーリーのまたは [wikipedia](http://wikipedia.org/wiki/Docker_%28software%29))、Docker とそのエコシステムは開発者と IT プロフェッショナルの両方に大きなメリットを提供します。

Docker コンテナーは開発者の間で急速に普及していますが、これは、Linux コンテナーを使いやすくする次のようなメリットがあるためです。

- シンプルなコマンドを使用して固定イメージを作成し、それらを簡単にデプロイできるだけでなく、Dockerfile を使用してそれらのイメージ作成を自動化することができる
- 簡単に、単純なを使用してそれらのイメージを共有できる [git](https://git-scm.com/)-スタイル プッシュとプルのコマンドを [パブリック](https://registry.hub.docker.com/) または [プライベートの docker レジストリ](virtual-machines-docker-registry-on-azure-blob-storage.md) 
- コンピューター単位ではなく、分離されたアプリケーション コンポーネント単位で開発が行える
- さまざまなツールを使用して、Docker コンテナーや各種ベース イメージの構成を把握できる

### オペレーション スタッフや IT プロフェッショナルにとってのコンテナーの利点

IT コンテナーと仮想マシンの組み合わせは、オペレーション スタッフや IT プロフェッショナルにもメリットをもたらします。 

- コンテナー内のサービスを VM ホストの実行環境から分離できる
- コンテナー内のコードを完全に統一できる
- コンテナー内のサービスをすばやく起動、停止できるだけでなく、開発環境、テスト環境、および運用環境間の移動もすばやく行える。

これら機能は、老舗の大手企業にとっては特に魅力的なものです。このような企業の IT スタッフは、事業継続だけでなく、顧客満足度や顧客リーチの改善のために必要な各種のタスクに対し、処理パワーなどのリソースをバランスよく割り当てなければならないからです。 小規模企業、ISV、新興企業などでも同様の要件はありますが、具体的な事情は多少異なるかもしれません。 

## 仮想マシンの役割

仮想マシンはクラウド コンピューティングのバックボーンを提供するものであり、そのことは今でも変わりません。 仮想マシンは起動が遅く、ディスク フットプリントが大きく、マイクロサービス アーキテクチャに直接マップすることもできませんが、非常に重要な役割を担っています。 

1. ホスト コンピューターに対する強力なセキュリティ保護機能を標準で備えている
2. 主要な OS とアプリケーション構成をすべてサポートしている
3. コマンドや管理ツールのエコシステムが長年にわたって整備されている
4. コンテナーをホストするための実行環境を提供してくれる

最後の項目は特に重要です。コンテナー内のアプリケーションでは、プロセスの呼び出し方法に応じて特定のオペレーティング システムや CPU タイプが必要になるからです。 コンテナーをインストールするには、あくまでも VM が必要です。コンテナーはデプロイするアプリケーションの格納手段であり、VM やオペレーティング システムに取って代わるものではないのです。

## VM とコンテナーの大まかな機能比較

次の表は、VM と Linux コンテナーの機能 の違いを、大規模な作業を伴わない機能に限って、きわめて大まかに示したものです。 どちらの機能がより有効かは、組織のニーズによって変わってくる場合もあります。また、どのアプリケーションでも、追加作業によって機能のサポート レベルを向上させることは可能です (特にセキュリティ面)。

|   機能      | VM | コンテナー  |
| :------------- |-------------| ----------- |
| "既定" のセキュリティ サポート | より充実している | 充実度においてやや劣る |
| 必要なディスク上のメモリ | OS 全体とアプリ | アプリ要件のみ |
| 起動にかかる時間 | 非常に長い: OS のブートとアプリの読み込み | 非常に短い: カーネルが既に実行されているため必要なのはアプリの起動のみ  |
| 移植性 | 適切な準備をすれば移植可能 | イメージ フォーマット内でのみ移植可能 (通常はより限定的) | 
| イメージの自動化 | OS やアプリによって大きく異なる | [Docker レジストリ](https://registry.hub.docker.com/); 他のユーザー

## VM とコンテナーのグループ作成とその管理

ここまでお読みになって、設計者、開発者、IT オペレーション スペシャリストの皆さんはこう思っているのではないでしょうか。「これなら何でも自動化できる。これこそ真の DCaaS (Data-Center-As-A-Service) だ」と。 

確かに、できるし、システムのうちの多くが既に使用して、Azure Vm のグループを管理およびいずれかに多くの場合、スクリプトを使用してカスタム コードを挿入できる任意の数がある、 [CustomScriptingExtension for Windows](https://msdn.microsoft.com/library/azure/dn781373.aspx) または [Linux 用 CustomScriptingExtension](http://azure.microsoft.com/blog/2014/08/20/automate-linux-vm-customization-tasks-using-customscript-extension/)します。 できる & mdash; おそらくをまだ所有して & mdash; PowerShell や Azure CLI のスクリプトを使用して、Azure デプロイメントを自動化 [次のような](virtual-machines-create-multi-vm-deployment-xplat-cli-install.md)です。 

これらの機能はなどのツールに移行し、多くの場合、 [Puppet](https://puppetlabs.com/) と [Chef](https://www.chef.io/) の作成および規模での Vm の構成を自動化します。 (へのリンクをここでは [Azure でこれらのツールを使用して](#tools-for-working-with-containers).) 

### Azure のリソース グループ テンプレート

最近では、Azure がリリースされた、 [Azure リソース管理](virtual-machines-azurerm-versus-azuresm.md) REST API、および更新された、PowerShell と Azure CLI ツールは、簡単に使用します。 展開、変更、またはを使用して、アプリケーション全体のトポロジを再展開 [Azure リソース マネージャー テンプレート](../resource-group-authoring-templates.md) Azure リソース管理 API を使用して。

-  [テンプレートを使用して Azure ポータル](https://github.com/Azure/azure-quickstart-templates)& mdash; を"DeployToAzure"ボタンを使用して
-  [Azure CLI](virtual-machines-deploy-rmtemplates-azure-cli.md)
-  [Azure PowerShell モジュール](virtual-machines-deploy-rmtemplates-azure-cli.md)


### Azure VM とコンテナーのグループの一括的なデプロイメントと管理

複数の VM グループを一括でデプロイし、それらのデプロイメントに Docker (またはその他の Linux コンテナー ホスト システム) をインストールして、自動化可能なグループにするための主なシステムを紹介します。 ダイレクト リンクは、次を参照してください。、 [コンテナーとツール](#containers-and-vm-technologies) セクションは、以下です。 機能の範囲はシステムによって異なります。また、ここに挙げるものがこの種のシステムのすべてというわけではありません。 どのシステムが便利かは、ユーザーのスキル セットやシナリオによって変わってきます。 

Docker は、独自の VM 作成ツールのセット ([docker マシン](virtual-machines-docker-machine.md)) と、負荷分散、docker コンテナー クラスター管理ツール ([swarm](virtual-machines-docker-swarm.md))。 さらに、 [Azure Docker VM 拡張機能](https://github.com/Azure/azure-docker-extension/blob/master/README.md) に対する既定のサポートが付属 [`docker-compose`](https://docs.docker.com/compose/), 、複数のコンテナー構成済みアプリケーション コンテナーをデプロイすることができます。

さらに、お試しいただく [Mesosphere のデータ センター オペレーティング システム (DCOS)](http://docs.mesosphere.com/install/azurecluster/)します。 DCOS はオープン ソースに基づいて [mesos](http://mesos.apache.org/) 「分散システム カーネル」1 つのアドレス指定可能なサービスとしてデータ センターを処理することができます。 DCOS は、いくつかの重要なシステムの組み込みパッケージをなどが [Spark](http://spark.apache.org/) と [Kafka](http://kafka.apache.org/) (など) などの組み込みサービスだけでなく [Marathon](https://mesosphere.github.io/marathon/) (コンテナー管理システム) と [Chronos](https://mesosphere.github.io/chronos/) (分散スケジューラ)。 Mesos は、Twitter、AirBnb、およびその他の Web スケール ビジネスに影響を受けて開発されたものです。

また、 [kubernetes](http://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure) Google にから派生した VM およびコンテナーのグループ管理のためのオープン ソース システムです。 使用することも [kubernetes を weave ネットワー キングのサポートを提供する](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)です。

[Deis](http://deis.io/overview/) 、オープン ソースは、「サービスとしてのプラットフォームで、-」(PaaS) を展開して、独自のサーバー上のアプリケーションを管理できるようになります。 Deis はDocker と CoreOS をベースとしており、それによって、Heroku 風のワークフローを使用した軽量な PaaS を実現しています。 簡単に実行できます [3 ノードの Azure VM グループを作成して Deis をインストール](virtual-machines-deis-cluster.md) Azure でし [Hello World Go アプリケーションをインストールする](virtual-machines-deis-cluster.md#deploy-and-scale-a-hello-world-application)です。 

[CoreOS](virtual-machines-linux-coreos-how-to.md), 、最適なフット プリントと Docker サポート、および独自のコンテナー システムでの Linux ディストリビューションと呼ばれる [rkt](https://github.com/coreos/rkt), 、というコンテナー グループの管理ツールも備えて [fleet](virtual-machines-linux-coreos-fleet-get-started.md)します。 

Ubuntu、もう 1 つの非常にポピュラーな Linux ディストリビューションが非常にうまく Docker のサポートもサポートされています [Linux (LXC スタイル) クラスター](https://help.ubuntu.com/lts/serverguide/lxc.html)します。 

## Azure VM とコンテナーを操作するためのツール

コンテナーと Azure VM の操作には各種のツールを使用します。 このセクションでは、コンテナー、グループ、またはより大きな構成を扱ううえで特に有用かつ重要な概念とツールを紹介するとともに、それらを操作するためのオーケストレーション ツールを列挙します。 

> [AZURE.NOTE] この領域が急速に驚くことには、変更して、このトピックとそのリンクを最新の状態に保持する、最善を尽くします中にある不可能なタスク。 興味のあるテーマについては、ご自身でも検索を行い、最新の情報をチェックしてください。

### コンテナーと VM のテクノロジ

主な Linux コンテナー テクノロジ:

- [Docker](https://www.docker.com)
- [LXC](https://linuxcontainers.org/)
- [CoreOS と rkt](https://github.com/coreos/rkt)
- [Open Container Project](http://opencontainers.org/)
- [RancherOS](http://rancher.com/rancher-os/)

Windows コンテナー関連のリンク:

- [Windows コンテナー](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)

Visual Studio Docker 関連のリンク:

- [Visual Studio 2015 RC Tools for Docker - Preview](https://visualstudiogallery.msdn.microsoft.com/6f638067-027d-4817-bcc7-aa94163338f0)

Docker ツール:

- [Docker デーモン](https://docs.docker.com/installation/#installation)
- Docker クライアント
    - [Chocolatey の Windows Docker クライアント](https://chocolatey.org/packages/docker)
    - [Docker のインストール手順](https://docs.docker.com/installation/#installation)


Docker on Microsoft Azure:

- [Azure での Linux 用 Docker 仮想マシン拡張機能](virtual-machines-docker-vm-extension.md)
- [Azure の Docker 用 VM 拡張機能のユーザー ガイド](https://github.com/Azure/azure-docker-extension/blob/master/README.md)
- [Azure コマンド ライン インターフェイス (Azure CLI) での Docker VM 拡張機能の使用](virtual-machines-docker-with-xplat-cli-install.md)
- [Azure ポータルでの Docker VM 拡張機能の使用](virtual-machines-docker-with-portal.md)
- [Azure Marketplace で Docker をすばやく開始する](virtual-machines-docker-ubuntu-quickstart.md)
- [Azure 上で CoreOs を使用する方法](virtual-machines-docker-machine.md)
- [How to use docker with swarm on Azure (Azure 上の Swarm における Docker の使用方法)](virtual-machines-docker-swarm.md)
- [Azure での Docker および Compose の概要](virtual-machines-docker-compose-quickstart.md)
- [Azure のリソース グループ テンプレートを使用して Azure 上に Docker ホストをすばやく作成する方法](https://github.com/Azure/azure-quickstart-templates/tree/master/docker-simple-on-ubuntu)
- [組み込みサポート `compose`](https://github.com/Azure/azure-docker-extension#11-public-configuration-keys) コンテナー内アプリケーションの
- [Azure への独自のプライベート Docker Registry のデプロイ](virtual-machines-docker-registry-on-azure-blob-storage.md)

Linux ディストリビューションと Azure での導入例:

- [CoreOS](virtual-machines-linux-coreos-how-to.md)

構成、クラスター管理、およびコンテナー オーケストレーション:

- [CoreOS の Fleet](virtual-machines-linux-coreos-fleet-get-started.md)

-   Deis
    - [3 ノードの Azure VM グループを作成し、Deis をインストールして、Hello World Go アプリケーションを起動する方法](virtual-machines-deis-cluster.md)
    
-   Kubernetes
    - [CoreOS と Weave を使用した Kubernetes クラスターのデプロイの自動化に関する包括的なガイド](https://github.com/GoogleCloudPlatform/kubernetes/blob/master/docs/getting-started-guides/coreos/azure/README.md#kubernetes-on-azure-with-coreos-and-weave)
    - [Kubernetes Visualizer](http://azure.microsoft.com/blog/2014/08/28/hackathon-with-kubernetes-on-azure)
    
-   [Mesos](http://mesos.apache.org/)
    -   [Mesosphere のデータ センター オペレーティング システム (DCOS)](http://beta-docs.mesosphere.com/install/azurecluster/)
    
-   [Jenkins](https://jenkins-ci.org/) と [Hudson](http://hudson-ci.org/)
    - [ブログ: Azure 用 Jenkins スレーブ プラグイン](http://msopentech.com/blog/2014/09/23/announcing-jenkins-slave-plugin-azure/)
    - [GitHub のリポジトリ: Azure 用 Jenkins ストレージ プラグイン](https://github.com/jenkinsci/windows-azure-storage-plugin)
    - [サード パーティ: Azure 用 Hudson スレーブ プラグイン](http://wiki.hudson-ci.org/display/HUDSON/Azure+Slave+Plugin)
    - [サード パーティ: Azure 用 Hudson ストレージ プラグイン](https://github.com/hudson3-plugins/windows-azure-storage-plugin)
    
-   [Chef](https://docs.chef.io/index.html)
    - [Chef と仮想マシン](virtual-machines-windows-install-chef-client.md)
    - [ビデオ: Chef の概要とそのしくみ](https://msopentech.com/blog/2014/03/31/using-chef-to-manage-azure-resources/)

-   [Azure Automation](http://azure.microsoft.com/services/automation/)
    - [ビデオ: Linux VM で Azure Automation を使用する方法](http://channel9.msdn.com/Shows/Azure-Friday/Azure-Automation-104-managing-Linux-and-creating-Modules-with-Joe-Levy)
    
-   Linux 用の Powershell DSC
    - [ビデオ: Linux 用 Powershell DSC を実行する方法](http://blogs.technet.com/b/privatecloud/archive/2014/05/19/powershell-dsc-for-linux-step-by-step.aspx)
    - [GitHub: Docker クライアント DSC](https://github.com/anweiss/DockerClientDSC)

## 次のステップ

チェック アウト [Docker](https://www.docker.com) と [Windows コンテナー](https://msdn.microsoft.com/virtualization/windowscontainers/about/about_overview)します。

<!--Anchors-->
[microservices]: http://martinfowler.com/articles/microservices.html
[microservice]: http://martinfowler.com/articles/microservices.html
<!--Image references-->

