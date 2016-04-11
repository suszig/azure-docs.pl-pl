<properties
    pageTitle="Azure コマンド ライン インターフェイスのインストール | Microsoft Azure"
    description="Mac、Linux、および Windows に Azure CLI をインストールして Azure サービスの利用を開始する"
    editor=""
    manager="timlt"
    documentationCenter=""
    authors="dlepow"
    services=""
    tags="azure-resource-manager,azure-service-management"/>

<tags
    ms.service="multiple"
    ms.workload="multiple"
    ms.tgt_pltfrm="command-line-interface"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2015"
    ms.author="danlep"/>

# Azure CLI のインストール

この記事では、Azure コマンド ライン インターフェイス (Azure CLI) をインストールする方法について説明します。 Azure CLI には、Microsoft Azure 上のリソースを作成および管理するための一連のオープン ソースのシェル ベース コマンドが用意されています。

[AZURE.INCLUDE [learn-about-deployment-models](../includes/learn-about-deployment-models-both-include.md)]


Azure CLI は、JavaScript で記述されており、必要があります [Node.js](https://nodejs.org)します。 使用して実装されている、 [Azure SDK for Node](https://github.com/azure/azure-sdk-for-node), 、おり、Apache 2.0 ライセンスに基づいてリリースされています。 プロジェクトのリポジトリは [https://github.com/azure/azure-xplat-cli](https://github.com/azure/azure-xplat-cli)します。

> [AZURE.NOTE] 既に Azure CLI をインストールした場合は、Azure リソースに接続します。 詳細については、次を参照してください。 [、Azure サブスクリプションに接続する方法](xplat-cli-connect.md#configure)します。

<a id="install"></a>
## 方法: Azure CLI をインストールする

Azure CLI をインストールする方法はいくつかあります。

1. インストーラーの使用
2. Node.js と npm をインストールし、使用、 **npm インストール** コマンド
3. Docker コンテナーとして Azure CLI を実行する

使用して Azure CLI がインストールされるができます、 **azure** コマンド、コマンド ライン インターフェイス (Bash、ターミナル、コマンド プロンプトおよびなど) を Azure CLI コマンドにアクセスを実行します。

## インストーラーの使用

次のインストーラー パッケージを使用できます。

* [Windows インストーラー][windows-installer]

* [OS X インストーラー](http://go.microsoft.com/fwlink/?LinkId=252249)

* [Linux インストーラー][linux-installer]


## Node.js と npm のインストールと使用

Node.js が既にシステムにインストールされている場合、次のコマンドを使用して Azure CLI をインストールします。

    npm install azure-cli -g

> [AZURE.NOTE] Linux ディストリビューションの場合は、使用する必要があります `sudo` を実行するため、 __npm__ コマンドです。

### 使用する Linux ディストリビューションで node.js と npm のインストール [dpkg](http://en.wikipedia.org/wiki/Dpkg) パッケージ管理
いずれかを使用するようなディストリビューション、最も一般的な [(apt) のパッケージ化ツールを高度な](http://en.wikipedia.org/wiki/Advanced_Packaging_Tool) またはその他のツールに基づく、 `.deb` パッケージ形式です。 例としては、Ubuntu や Debian が該当します。

これらのパッケージが新しいほどのほとんどは、インストールが必要な **nodejs レガシ** 適切に構成を取得するために **npm** Azure CLI をインストールするツールです。 次のコードは、インストールするコマンドを示しています。 **npm** Ubuntu 14.04 に適切にします。

    sudo apt-get install nodejs-legacy
    sudo apt-get install npm
    sudo npm install -g azure-cli

Ubuntu 12.04 など以前のディストリビューションでは、現在の node.js のバイナリ ディストリビューションのインストールが必要です。 次のコードは、インストールして使用を実行する方法を示しています。 **curl**します。

>[AZURE.NOTE] ここでのコマンドは、Joyent のインストールの手順から取得 [ここ](https://github.com/joyent/node/wiki/installing-node.js-via-package-manager)します。 ただしを使用する場合 **sudo** をパイプ ターゲットとしてインストールし、適切に動作すること実行する前に実施ことを検証するスクリプトを常にチェックする必要があります **sudo**します。 強力な機能だけに、慎重に使用することが重要です。

    sudo apt-get install curl
    curl -sL https://deb.nodesource.com/setup | sudo bash -
    sudo apt-get install -y nodejs
    sudo npm install -g azure-cli

### 使用する Linux ディストリビューションで node.js と npm のインストール [rpm](http://en.wikipedia.org/wiki/RPM_Package_Manager) パッケージ管理

RPM ベースのディストリビューションで Node.js をインストールするには、EPEL リポジトリを有効にする必要があります。 次のコードは、CentOS 7 上でのインストールのベスト プラクティスを示しています  (下の最初の行の '-' (ハイフン) が重要ですので注意してください)。

    su -
    yum update [enter]
    yum upgrade –y [enter]
    yum install epel-release [enter]
    yum install nodejs [enter]
    yum install npm [enter]
    npm install -g azure-cli  [enter]

### Windows および Mac OS X への node.js および npm のインストール

Node.js と npm をインストールするには Windows および OS X インストーラーを使用で [Nodejs.org](https://nodejs.org/download/)します。 インストールを完了するために、コンピューターを再起動する必要がある場合があります。 node.js と npm が正しくインストールされていることを確認するには、コマンド プロンプトを開き、次のように入力します。

    npm -v

インストールされている npm のバージョンが表示された場合は、次のコマンドを使用して Azure CLI をインストールします。

    npm install -g azure-cli

インストールの最後に、次のような情報が表示されます。

    azure-cli@0.8.0 ..\node_modules\azure-cli
    |-- easy-table@0.0.1
    |-- eyes@0.1.8
    |-- xmlbuilder@0.4.2
    |-- colors@0.6.1
    |-- node-uuid@1.2.0
    |-- async@0.2.7
    |-- underscore@1.4.4
    |-- tunnel@0.0.2
    |-- omelette@0.1.0
    |-- github@0.1.6
    |-- commander@1.0.4 (keypress@0.1.0)
    |-- xml2js@0.1.14 (sax@0.5.4)
    |-- streamline@0.4.5
    |-- winston@0.6.2 (cycle@1.0.2, stack-trace@0.0.7, async@0.1.22, pkginfo@0.2.3, request@2.9.203)
    |-- kuduscript@0.1.2 (commander@1.1.1, streamline@0.4.11)
    |-- azure@0.7.13 (dateformat@1.0.2-1.2.3, envconf@0.0.4, mpns@2.0.1, mime@1.2.10, validator@1.4.0, xml2js@0.2.8, wns@0.5.3, request@2.25.0)

>[AZURE.NOTE] Linux システムでインストールすることも、Azure CLI から作成することにより、 [ソース](http://go.microsoft.com/fwlink/?linkid=253472)します。 ソースからの構築の詳細については、アーカイブにある INSTALL ファイルを参照してください。

## Docker コンテナーの使用

Docker ホストで、次のコマンドを実行します。
```
    docker run -it microsoft/azure-cli
```

## Azure CLI コマンドの実行

使用して Azure CLI がインストールされるができます、 **azure** コマンド、コマンド ライン インターフェイス (Bash、ターミナル、コマンド プロンプトおよびなど)、Azure CLI コマンドにアクセスを実行します。 たとえば、Windows で help コマンドを実行するには、コマンド ウィンドウを起動して、次のように入力します。

```
    c:\> azure help
```

これで準備が整いました。 次に実行できます [Azure CLI から Azure サブスクリプションへの接続](xplat-cli-connect.md) の使用を開始し、 **azure** コマンドです。


<a id="additional-resources"></a>
## その他のリソース

* [Azure CLI でのリソース管理コマンドの使用][cliarm]

* [Azure CLI でのサービス管理 (クラシック) コマンドの使用][cliasm]

* 詳細については、Azure CLI、ソース コードのダウンロード、問題の報告や、プロジェクトへの協力、 [GitHub リポジトリの Azure CLI](https://github.com/azure/azure-xplat-cli)します。

* Azure CLI または Azure を使用して問題が発生した場合にアクセスして、 [Azure フォーラム](http://social.msdn.microsoft.com/Forums/windowsazure/home)します。



[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/?linkid=9828653&clcid=0x409
[linux-installer]: http://go.microsoft.com/fwlink/?linkid=253472
[cliasm]: virtual-machines/virtual-machines-command-line-tools.md
[cliarm]: virtual-machines/xplat-cli-azure-resource-manager.md


