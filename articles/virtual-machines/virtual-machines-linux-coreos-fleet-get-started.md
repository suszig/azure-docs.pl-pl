<properties
    pageTitle="CoreOS で Fleet を使ってみる | Microsoft Azure"
    description="Azure 上のクラシック デプロイメント モデルで作成された CoreOS Linux VM クラスター上での Fleet と Docker の使用方法を紹介します。"
    services="virtual-machines"
    documentationCenter=""
    authors="dlepow"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-linux"
    ms.workload="infrastructure-services"
    ms.date="11/16/2015"
    ms.author="danlep"/>

# Azure の CoreOS VM クラスターで fleet を使ってみる

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [リソース マネージャー モデル](https://azure.microsoft.com/documentation/templates/coreos-with-fleet-multivm/)します。


この記事で使用する 2 つ簡単な例については、 [fleet](https://github.com/coreos/fleet) と [Docker](https://www.docker.com/) のクラスターでアプリケーションを実行する [CoreOS] 仮想マシン。

これらの例を使用するクラスターを設定する 3 つのノードの CoreOS」の説明に従って [How to Use CoreOS on Azure]します。 設定が終わったら、CoreOS デプロイの極めて基本的な要素について理解し、動作中のクラスターとクライアント コンピューターが準備できたことになります。 これらの例では、同じクラスター名を使用します。 また、これらの例を実行するローカルの Linux ホストを使用している、 **fleetctl** コマンドです。 参照してください [クライアントを使用して](https://coreos.com/fleet/docs/latest/using-the-client.html) 詳細については、 **fleetctl** クライアントです。


## <a id='simple'>例 1: Docker を使用した Hello World</a>

これは、1 つの Docker コンテナーで実行されている単純な「Hello World」アプリケーションです。 これを使用して、 [busybox Docker Hub image]します。

Linux クライアント コンピューターでは、次を作成する任意のテキスト エディターを使用 **systemd** 単位ファイルし、名付けます `helloworld.service`します。 (構文の詳細については、「 [Unit Files])。

```
[Unit]
Description=HelloWorld
After=docker.service
Requires=docker.service

[Service]

TimeoutStartSec=0
ExecStartPre=-/usr/bin/docker kill busybox1
ExecStartPre=-/usr/bin/docker rm busybox1
ExecStartPre=/usr/bin/docker pull busybox
ExecStart=/usr/bin/docker run --name busybox1 busybox /bin/sh -c "while true; do echo Hello World; sleep 1; done"
ExecStop=/usr/bin/docker stop busybox1

```

ここで CoreOS クラスターに接続し、以下を実行してユニットを開始 **fleetctl** コマンドです。 出力に、ユニットが開始したこと、およびユニットの場所が表示されます。


```
fleetctl --tunnel coreos-cluster.cloudapp.net:22 start helloworld.service


Unit helloworld.service launched on 62f0f66e.../100.79.86.62
```

>[AZURE.NOTE] リモコンを実行する **fleetctl** せずコマンド、 **--トンネル** パラメーターは、必要に応じて、要求に FLEETCTL_TUNNEL 環境変数を設定します。 (例: `export FLEETCTL_TUNNEL=coreos-cluster.cloudapp.net:22`)。


コンテナーに接続すると、サービスの出力を参照できます。

```
fleetctl --tunnel coreos-cloudapp.cluster.net:22 journal helloworld.service

Mar 04 21:29:26 node-1 docker[57876]: Hello World
Mar 04 21:29:27 node-1 docker[57876]: Hello World
Mar 04 21:29:28 node-1 docker[57876]: Hello World
Mar 04 21:29:29 node-1 docker[57876]: Hello World
Mar 04 21:29:30 node-1 docker[57876]: Hello World
Mar 04 21:29:31 node-1 docker[57876]: Hello World
Mar 04 21:29:32 node-1 docker[57876]: Hello World
Mar 04 21:29:33 node-1 docker[57876]: Hello World
Mar 04 21:29:34 node-1 docker[57876]: Hello World
Mar 04 21:29:35 node-1 docker[57876]: Hello World
```

クリーンアップするには、そのユニットを停止し、アンロードします。

```
fleetctl --tunnel coreos-cluster.cloudapp.net:22 stop helloworld.service
fleetctl --tunnel coreos-cluster.cloudapp.net:22 unload helloworld.service
```


## <a id='highavail'>例 2: 高可用性の nginx サーバー</a>

CoreOS、Docker を使用する利点の 1 つと **fleet** は簡単に可用性の高い方法でサービスを実行します。 この例では、3 つの同じコンテナーで構成される、nginx Web サーバーを実行するサービスをデプロイします。 これらのコンテナーは、クラスター内にある 3 つの VM 上で実行されます。 この例は、のいずれかのような [Launching containers with fleet] を使用して、 [nginx Docker Hub image]します。

>[AZURE.IMPORTANT] 高可用性 web サーバーを実行するには、仮想マシン (パブリック ポート 80、プライベート ポート 80) で負荷分散された HTTP エンドポイントを構成する必要があります。 これを行う、CoreOS クラスターを作成した後、Azure クラシック ポータルを使用または **azure vm endpoint** コマンドです。 参照してください [Configure a load-balanced set] の詳細。

クライアント コンピューターに作成する任意のテキスト エディターを使用して、 **systemd** nginx@.service をという名前のテンプレート ユニット ファイルです。 この単純なテンプレートを使用して nginx@1.service、nginx@2.service、および nginx@3.service という 3 つのインスタンスを起動します。

```
[Unit]
Description=High Availability Nginx
After=docker.service
Requires=docker.service

[Service]
TimeoutStartSec=0
ExecStartPre=/usr/bin/docker pull nginx
ExecStart=/usr/bin/docker run --rm --name nginx1 -p 80:80 nginx
ExecStop=/usr/bin/docker stop nginx1

[X-Fleet]
X-Conflicts=nginx@*.service
```

>[AZURE.NOTE]  `X-Conflicts` 属性は含まれる唯一の 1 つであり、このコンテナーのインスタンスは、特定の CoreOS ホストで実行できます。 詳細をご覧ください [Unit Files]します。

次に、CoreOS クラスター上で、これらのユニット インスタンスを開始します。 3 つの異なるマシン上でユニット インスタンスが実行されていることを確認してください。

```
fleetctl --tunnel coreos-cluster.cloudapp.net:22 start nginx@{1,2,3}.service

unit nginx@3.service launched on 00c927e4.../100.79.62.16
unit nginx@1.service launched on 62f0f66e.../100.79.86.62
unit nginx@2.service launched on df85f2d1.../100.78.126.15

```
これらのユニットのいずれか 1 つで実行されている Web サーバーに接続するには、その CoreOS クラスターをホストしているクラウド サービスに簡単な要求を送信します。

`curl http://coreos-cluster.cloudapp.net`

次のような既定テキストが、nginx サーバーから返されます。

```
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

Web サービスが続行することを確認するために、クラスター内の 1 つまたは複数の仮想マシンをシャット ダウンすることもできます。

完了したら、ユニットを停止してアンロードします。

```
fleetctl --tunnel coreos-cluster.cloudapp.net:22 stop nginx@{1,2,3}.service
fleetctl --tunnel coreos-cluster.cloudapp.net:22 unload nginx@{1,2,3}.service

```

## 次のステップ

* Azure 上の 3 ノード構成 CoreOS クラスターでは、この他さまざまな操作を試すことができます。 さらに複雑なクラスターを作成し、Docker を使用してを読み取ってより興味深いアプリケーションを作成する方法を調べる [Tim Park's CoreOS Tutorial], 、[Patrick Chanezon's CoreOS Tutorial], 、[Docker] ドキュメントについては、および [CoreOS Overview]します。

* 最初に Fleet と CoreOS Azure リソース マネージャーで、実際に使ってみる [クイック スタート テンプレート](https://azure.microsoft.com/documentation/templates/coreos-with-fleet-multivm/)します。

* 参照してください [Linux and Open-Source Computing on Azure] Azure Linux Vm でのオープン ソース環境の使用に関する詳細。

<!--Link references-->
[Azure Command-Line Interface (Azure)]: ../xplat-cli-install.md
[CoreOS]: https://coreos.com/
[CoreOS Overview]: https://coreos.com/using-coreos/
[CoreOS with Azure]: https://coreos.com/docs/running-coreos/cloud-providers/azure/
[Tim Park's CoreOS Tutorial]: https://github.com/timfpark/coreos-azure
[Patrick Chanezon's CoreOS Tutorial]: https://github.com/chanezon/azure-linux/tree/master/coreos/cloud-init
[Docker]: http://docker.io
[YAML]: http://yaml.org/
[How to Use CoreOS on Azure]: virtual-machines-linux-coreos-how-to.md
[Configure a load-balanced set]: ../load-balancer/load-balancer-internet-getstarted.md
[Launching containers with fleet]: https://coreos.com/docs/launching-containers/launching/launching-containers-fleet/
[Unit Files]: https://coreos.com/docs/launching-containers/launching/fleet-unit-files/
[busybox Docker Hub image]: https://registry.hub.docker.com/_/busybox/
[nginx Docker Hub image]: https://hub.docker.com/_/nginx/
[Linux and Open-Source Computing on Azure]: virtual-machines-linux-opensource.md


