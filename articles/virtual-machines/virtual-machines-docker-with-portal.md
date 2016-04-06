<properties
    pageTitle="Linux 用 Docker VM 拡張機能の使用 | Microsoft Azure"
    description="Docker と Azure Virtual Machines の拡張機能について説明し、クラシック デプロイ モデルで Azure CLI を使用して、Docker ホストである Azure 仮想マシンを作成する方法を示します。"
    services="virtual-machines"
    documentationCenter=""
    authors="squillace"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="09/22/2015"
    ms.author="rasquill"/>


# Azure クラシック ポータルで Docker VM 拡張機能を使用する方法

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


[Docker](https://www.docker.com/) を使用する最も一般的な仮想化アプローチの 1 つ [Linux コンテナー](http://en.wikipedia.org/wiki/LXC) のデータの分離と共有リソースでのコンピューティングの手段として仮想マシンではなく。 管理する Docker VM 拡張機能を使用する [Azure Linux Agent] Azure でのアプリケーションのコンテナーの任意の数をホストする Docker VM を作成します。

> [AZURE.NOTE] このトピックでは、Azure クラシック ポータルから Docker VM を作成する方法について説明します。 コマンドラインで Docker VM を作成する方法を確認するには、次を参照してください。 [How to use the Docker VM Extension from the Azure Command-line Interface (Azure CLI)]します。 コンテナーとその利点に関する概要についてを参照してください、 [Docker 高レベルのホワイト ボード](http://channel9.msdn.com/Blogs/Regular-IT-Guy/Docker-High-Level-Whiteboard)します。

## イメージ ギャラリーからの新しい VM の作成
最初のステップとして、Docker VM 拡張機能をサポートする Linux イメージから Azure VM を作成する必要があります。イメージ ギャラリーから取得した Ubuntu 14.04 LTS イメージをサンプルのサーバー イメージとして、Ubuntu 14.04 デスクトップをクライアントとして使用します。 ポータルで、次のようにクリックします。 **+ 新規** 、画面左下に新しい VM インスタンスを作成し、次のように選択可能な項目または完全なイメージ ギャラリーから Ubuntu 14.04 LTS イメージを選択します。

> [AZURE.NOTE] 現時点では、2014 年 7 月以降の Ubuntu 14.04 LTS イメージのみが Docker VM 拡張機能をサポートします。

![新しい Ubuntu イメージの作成
](./media/virtual-machines-docker-with-portal/ChooseUbuntu.png)

## Docker 証明書の作成

VM の作成が完了したら、クライアント コンピューターに Docker がインストールされていることを確認します (詳細については、「 [Docker のインストール手順](https://docs.docker.com/installation/#installation).)

」の手順に従って Docker の通信用の証明書とキー ファイルを作成する [Running Docker with https] に配置し、 **`~/.docker`** ディレクトリにクライアント コンピューターにします。

> [AZURE.NOTE] 現在、ポータルで Docker VM 拡張機能には、base64 でエンコードされている資格情報が必要です。

コマンド ラインでを使用して **`base64`** か別の使い慣れたエンコード ツールを base64 でエンコードされたトピックを作成します。 証明書とキー ファイルのシンプルなセットをしてこの操作を行った場合、次のようになります。

```
 ~/.docker$ l
 ca-key.pem  ca.pem  cert.pem  key.pem  server-cert.pem  server-key.pem
 ~/.docker$ base64 ca.pem > ca64.pem
 ~/.docker$ base64 server-cert.pem > server-cert64.pem
 ~/.docker$ base64 server-key.pem > server-key64.pem
 ~/.docker$ l
 ca64.pem    ca.pem    key.pem            server-cert.pem   server-key.pem
 ca-key.pem  cert.pem  server-cert64.pem  server-key64.pem
```

## Docker VM 拡張機能の追加
Docker VM 拡張機能を追加するには、作成した VM インスタンスを検索およびまで下へスクロール **拡張** 次のように、VM 拡張機能を表示する] をクリックします。
> [AZURE.NOTE] この機能は、プレビュー ポータルでのみサポートされます: https://portal.azure.com/

![](./media/virtual-machines-docker-with-portal/ClickExtensions.png)
### 拡張機能の追加
クリックして、 **+ 追加** 拡張子を表示する、可能な VM をこの VM に追加することができます。

![](./media/virtual-machines-docker-with-portal/ClickAdd.png)
### Docker VM 拡張機能の選択
、Docker の説明と重要なリンクを表示して、Docker VM 拡張機能を選択し、クリックして **作成** 下部には、インストール手順が開始します。

![](./media/virtual-machines-docker-with-portal/ChooseDockerExtension.png)

![](./media/virtual-machines-docker-with-portal/CreateButtonFocus.png)
### 証明書とキー ファイルの追加

下図に示すように、フォームのフィールドに、Base64 でエンコードされた CA 証明書、サーバー証明書、およびサーバー キーを入力します。

![](./media/virtual-machines-docker-with-portal/AddExtensionFormFilled.png)

> [AZURE.NOTE] (前イメージ) と同じ 2376年が入力されて既定では注意してください。 ここに任意のエンドポイントを入力できますが、次のステップで、一致するエンドポイントを開く必要があります。 既定値を変更した場合は、必ず、次のステップで一致するエンドポイントを開いてください。

## Docker 通信エンドポイントの追加
作成したリソース グループ内の仮想マシンを表示しているときは、クリックまでスクロール **エンドポイント** を次に示すように、VM でエンドポイントを表示します。

![](./media/virtual-machines-docker-with-portal/AddingEndpoint.png)

をクリックして **+ 追加** を別のエンドポイントを追加して、既定では、エンドポイントの名前を入力してください (この例では **docker**)、およびプライベートとパブリックの両方のポートとして 2376年します。 表示されているプロトコル値のままにして **TCP**, 、] をクリック **OK** 、エンドポイントを作成します。

![](./media/virtual-machines-docker-with-portal/AddEndpointFormFilledOut.png)


## Docker クライアントと Azure Docker ホストのテスト
見つけて、クライアント コンピューターの種類のコマンドラインで VM のドメインの名前をコピー `docker --tls -H tcp://`*dockerextension*`.cloudapp.net:2376 info` (ここで *dockerextension* は、VM のサブドメインに置き換えます)。

結果は次のようになります。

```
$ docker --tls -H tcp://dockerextension.cloudapp.net:2376 info
Containers: 0
Images: 0
Storage Driver: devicemapper
 Pool Name: docker-8:1-131214-pool
 Pool Blocksize: 65.54 kB
 Data file: /var/lib/docker/devicemapper/devicemapper/data
 Metadata file: /var/lib/docker/devicemapper/devicemapper/metadata
 Data Space Used: 305.7 MB
 Data Space Total: 107.4 GB
 Metadata Space Used: 729.1 kB
 Metadata Space Total: 2.147 GB
 Library Version: 1.02.82-git (2013-10-04)
Execution Driver: native-0.2
Kernel Version: 3.13.0-36-generic
WARNING: No swap limit support
```

これまでの手順を完了すると、Azure VM で実行する Docker ホストが正常に機能し、別のクライアントからリモートで接続されるように構成された状態になります。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## 次のステップ

移動する準備ができたら、 [Docker User Guide] Docker VM を使用します。 Azure Vm 上でコマンド ライン インターフェイスの Docker ホストの作成を自動化する場合は、次を参照してください。 [How to use the Docker VM Extension from the Azure Command-line Interface (Azure CLI)]

<!--Anchors-->
[Create a new VM from the Image Gallery]: #createvm
[Create Docker Certificates]: #dockercerts
[Add the Docker VM Extension]: #adddockerextension
[Test Docker Client and Azure Docker Host]: #testclientandserver
[Next steps]: #next-steps

<!--Image references-->
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[StartingPoint]: ./media/StartingPoint.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[How to use the Docker VM Extension from the Azure Command-line Interface (Azure CLI)]: http://azure.microsoft.com/documentation/articles/virtual-machines-docker-with-xplat-cli/
[Azure Linux Agent]: virtual-machines-linux-agent-user-guide.md
[Link 3 to another azure.microsoft.com documentation topic]: ../storage-whatis-account.md

[Running Docker with https]: http://docs.docker.com/articles/https/
[Docker User Guide]: https://docs.docker.com/userguide/


