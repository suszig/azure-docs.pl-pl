<properties
   pageTitle="3 ノード Deis クラスターのデプロイ | Microsoft Azure"
   description="この記事では、Azure リソース マネージャー テンプレートを使って、Azure 上で3 ノード Devis クラスターを作成する方法を説明します。"
   services="virtual-machines"
   documentationCenter=""
   authors="HaishiBai"
   manager="larar"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="06/24/2015"
   ms.author="hbai"/>

# 3 ノード Deis クラスターのデプロイ

この記事プロビジョニングについて説明する、 [Deis](http://deis.io/) Azure 上のクラスター。 展開およびサンプルを拡張するために必要な証明書の作成からのすべての手順を説明 **移動** 新しくプロビジョニングされたクラスタ上のアプリケーションです。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)] 従来のデプロイ モデルです。


次の図は、デプロイ済みシステムのアーキテクチャを示します。 クラスターを使用して、システム管理者の管理などの Deis ツール **deis** と **deisctl**します。 接続は、クラスター上のメンバーノードの 1 つに、接続を転送をする。Azure ロード バランサーを介して確立されます。 同様に Load Balancer を使用してアプリケーションをクライアントがアクセスにデプロイされます。 この場合は、Load Balancer は Deis ルーターメッシュへトラフィックを転送します。さらに、クラスターでホストされている、対応する Docker コンテナーへのトラフィックをルートします。

  ![デプロイ済みの Desis クラスターのアーキテクチャ図](media/virtual-machines-deis-cluster/architecture-overview.png)

次の手順を実行するため、以下の項目が必要となります。

 * 有効な Azure サブスクリプション ない場合で無償の試用版を入手できます [azure.com](https://azure.microsoft.com)します。
 *  Azure リソース グループを使用する職場または学校の ID 。 個人のアカウントと、Microsoft id でログインしていればする必要があります [個人から作業の id を作成](resource-group-create-work-id-from-personal.md)します。
 * いずれかの - クライアント オペレーティング システムに応じて--、 [Azure PowerShell](powershell-install-configure.md) または [Mac、Linux、および Windows 用 Azure CLI](xplat-cli-install.md)します。
 * [OpenSSL](https://www.openssl.org/)します。 OpenSSL を使用して、必要な証明書が生成されます。
 * などの Git クライアント [Git Bash](https://git-scm.com/)します。
 * サンプル アプリケーションをテストするには、DNS サーバーが必要です。 任意の DNS サーバーまたはワイルドカードの A レコードをサポートするサービスを使用することもできます。
 * Deis クライアント ツールを実行するコンピューターに。 ローカル コンピューターまたは仮想マシンのいずれかを使用することができます。 ほぼすべての Linux ディストリビューションでこれらのツールを実行することができますが、次の手順では、Ubuntu を使用します。

## クラスタのプロビジョニング

このセクションで使用する、 [Azure リソース マネージャー](../resource-group-overview.md) オープン ソース リポジトリからテンプレート [azure クイック スタート テンプレート](https://github.com/Azure/azure-quickstart-templates)します。 最初に、テンプレートをコピーします。 次に、認証の新しい SSH キー ペアを作成します。 次に、クラスターの新しい識別子を構成します。 最後に Shell スクリプトまたは PowerShell スクリプトのいずれかを、クラスターのプロビジョニングに使用します。

1. リポジトリを複製: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates)します。

        git clone https://github.com/Azure/azure-quickstart-templates

2. テンプレートのフォルダーを参照してください :

        cd azure-quickstart-templates\deis-cluster-coreos

3. ssh-keygen を使用して、新しい SSH キー ペアを作成します:

        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"

4. 上記の秘密キーを使用して、証明書を生成します:

        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file to be generated]

5. 移動して [https://discovery.etcd.io/new](https://discovery.etcd.io/new) のように表示されている、新しいクラスター トークンを生成します。

        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
<br />
各 CoreOS クラスターは、この無料サービスから一意のトークンが必要です。 参照してください [CoreOS ドキュメンテーション](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) 詳細です。

6. 変更、 **cloud-config.yaml** ファイルを置き換える既存  **検出** トークンを新しいトークンとします。

        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment the following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...

7. 変更、 **azuredeploy parameters.json** ファイル: テキスト エディターで、手順 4. で作成した証明書を開きます。 すべてのテキストの間でコピー `----BEGIN CERTIFICATE-----` と `-----END CERTIFICATE-----` に、 **sshKeyData** パラメーター (すべての改行文字を削除する必要があります)。

8. 変更、 **newStorageAccountName** パラメーター。 これは、VM  OS ディスクのストレージ アカウントです。 このアカウントの名前はグローバルに一意です。

9. 変更、 **publicDomainName** パラメーター。 これが Load Balancer のパブリック IP に関連付けられている DNS 名の一部になります。 最後の FQDN の形式が使用 _[このパラメーターの値]_._[地域]_します。cloudapp.azure.com します。 たとえば、deishbai32 として名前を指定して、リソース グループが米国西部リージョンにデプロイされている場合、Load Balancer の最終的な FQDN は deishbai32.westus.cloudapp.azure.com となります。

10. パラメーター ファイルを保存します。 Azure PowerShell を使用してプロビジョニングするには、次の手順を実行します。

        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml

  Azure CLI の場合

        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  

11. リソース グループがプロビジョニングされると、Azure クラシック ポータルで、グループ内のすべてのリソースを確認できます。 次のスクリーン ショットが示すように、リソース グループには、同じ可用性セットに参加している次の 3 つの VM を伴う仮想ネットワークが含まれます。 このグループには関連付けられているパブリック IP を持つ Load Balancer も含まれます。

  ![Azure クラシック ポータルでプロビジョニングされているリソース グループ](media/virtual-machines-deis-cluster/resource-group.png)

## クライアントをインストールします。

必要があります **deisctl** コントロールへの Deis クラスターです。 Deisctl はすべてのクラスター ノードで自動的にインストールされますが、管理用の個別のマシンで deisctl を使用することを推奨します。 さらに、プライベート IP アドレスのみを持つすべてのノードを構成するため Load Balancer は、ノード コンピューターへの接続に、パブリック ip 経由のトンネリング SSH を使用する必要があります。 物理マシンまたは仮想マシン上に個別の Ubuntu で deisctl をセットアップする手順を次に示します。

1. deisctl:mkdir deis をインストールします

        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl

2. ssh エージェントに秘密キーを追加します。

        eval `ssh-agent -s`
        ssh-add [path to the private key file, see step 1 in the previous section]

3. deisctl を構成します

        export DEISCTL_TUNNEL=[public ip of the load balancer]:2223

テンプレートは 2223 をインスタンス 1 に、 2224 をインスタンス 2 に、2225 をインスタンス 3 にマップするインバウンド NAT 規則を定義します。 これは、deisctl ツールを使用するための冗長性を提供します。 これらのルールは Azure クラシック ポータルで確認することができます。

![Load Balancer 上の NAT ルール](media/virtual-machines-deis-cluster/nat-rules.png)

> [AZURE.NOTE] 現在、テンプレートは、3 ノード クラスターのみをサポートします。 これは、ループ構文をサポートしていない Azure リソース マネージャー テンプレート NAT ルール定義の制限によるものです。

## Deis プラットフォームのインストールと開始

これで、Deis プラットフォームをインストールして開始する Deisctl を使用できるようになりました。

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path to the private key file]
    deisctl install platform
    deisctl start platform

> [AZURE.NOTE] プラットフォームの開始時間がかかります (10 分程度)。 特に、ビルダー サービスの開始には時間が長くかかることがあります。 また時々、試行が成功するかかる場合があります。 操作応答を停止する場合は、入力してみて `ctrl+c` をコマンドの実行を中断し、再試行してください。

すべてのサービスが実行されているかどうかを確認するには `deisctl list` を使用できます。

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

ご利用ありがとうございます。 これで Azure で Deis クラスターを実行することができました。 次に、サンプルの Go アプリケーションをデプロイしてみましょう。クラスタはどう動作するでしょうか。

## Hello World アプリケーションをデプロイ、スケールします。

次の手順は、"Hello World" の Go アプリケーションをクラスタにデプロイする方法を示します。 手順がに基づいて [Deis ドキュメンテーション](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles)します。

1. ルーティングメッシュを適切に機能させるには、Load Balancer のパブリック IP アドレスをポイントするドメインのワイルドカード A レコードを作成する必要があります。 次のスクリーン ショットは、GoDaddy のサンプル ドメイン登録用の A レコードです。

    ![Godaddy  A レコード](media/virtual-machines-deis-cluster/go-daddy.png)
<p />
2. deis のインストール:

        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
        
3. 新しい SSH キーを作成してその公開キーを GitHub (既存のキーはもちろんも再利用できます)に追加します。 ssh-keygen を使用して、新しい SSH キー ペアを作成します: 

        cd ~/.ssh
        ssh-keygen (press [Enter]s to use default file names and empty passcode)

4. GitHub に id_rsa.pub または任意の公開キーを追加します。 これは、SSH キー構成画面で SSH キー追加 ボタンを使用して実行できます。

  ![Github キー](media/virtual-machines-deis-cluster/github-key.png)
<p />
5. 新しいユーザーの登録

        deis register http://deis.[your domain]
<p />
6. SSH キーの追加:

        deis keys:add [path to your SSH public key]
  <p />      
7. アプリケーションの作成

        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
<p />
8. ビルドおよび展開するは、数分間かかる場合が Docker イメージは、git プッシュがトリガーされます。 私の経験では、場合によっては手順 10 (プライベート リポジトリにイメージをプッシュ) でハングアップする場合があります。 この場合、プロセスを停止、アプリケーションを使用して、削除 ' deis アプリ: – a を破棄 <application name>` to remove the application and try again. You can use `deis apps:list' アプリケーションの名前を確認します。 すべての動作が確認されたら、コマンドの出力の最後に、次のようなものが表示されます。

        -----> Launching...
               done, lambda-underdog:v2 deployed to Deis
               http://lambda-underdog.artitrack.com
               To learn more, use `deis help` or visit http://deis.io
        To ssh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
<p />
9. アプリケーションが動作しているかどうかを確認します:

        curl -S http://[your application name].[your domain]
  次のような結果が表示されます。

        Welcome to Deis!
        See the documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list to get the name of your application).
<p />
10. 3 つのインスタンスに、アプリケーションをスケール:

        deis scale cmd=3
<p />
11. 必要に応じて、deis 情報を使用して、アプリケーションの詳細を確認します。 次の出力は、私のアプリケーション デプロイです:

        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }

        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)

        === lambda-underdog Domains
        No domains

## 次のステップ

この記事ではAzure リソース マネージャーテンプレートを使用して Azure 上に新しい Deis プロビジョニングするすべての手順を説明 しました。 このテンプレートは、接続ツールの冗長性とデプロイ アプリケーションの付加バランスをサポートします。 このテンプレートにより、メンバー ノードでのパブリック IP の使用を回避します。これにより貴重なパブリック IP のリソースを保存し、アプリケーションのホストに安全な環境を提供します。 詳細については、次の記事を参照してください。

[Azure リソース マネージャーの概要] [resource-group-overview]  
[Azure CLI を使用する方法] [azure-command-line-tools]  
[Azure リソース マネージャーで Azure PowerShell の使用] [powershell-azure-resource-manager]  

[azure-command-line-tools]: ../xplat-cli-install.md
[resource-group-overview]: ../resource-group-overview.md
[powershell-azure-resource-manager]: ../powershell-azure-resource-manager.md

