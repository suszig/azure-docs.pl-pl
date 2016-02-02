<properties 
  pageTitle="Azure への独自のプライベート Docker Registry のデプロイ | Microsoft Azure"
  description="Docker Registry を使用して Azure BLOB ストレージ サービスでコンテナー イメージをホストする方法について説明します。"
  services="virtual-machines"
  documentationCenter="virtual-machines"
  authors="ahmetalpbalkan"
  editor="squillace"
  manager="" 
  tags="azure-service-management,azure-resource-manager" />

<tags
  ms.service="virtual-machines"
  ms.devlang="multiple"
  ms.topic="article"
  ms.tgt_pltfrm="vm-linux"
  ms.workload="infrastructure-services"
  ms.date="06/17/2015" 
  ms.author="ahmetb" />


# Azure への独自のプライベート Docker Registry のデプロイ

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]



ここでどのような Docker プライベート レジストリは、Docker Registry 2.0 を展開する方法を示しています。
Azure Blob ストレージを使用して Microsoft Azure で Docker プライベート レジストリにコンテナー イメージです。

このドキュメントでは以下のことが想定されています。

1. 読者は Docker の使用方法を理解しており、格納する Docker イメージを持っている。 (そうでない場合は、 [Docker についてください。](https://www.docker.com))
2. Docker エンジンがインストールされているサーバーがある。 (そうでない場合は、 [短時間では Azure です。](http://azure.microsoft.com/documentation/templates/docker-simple-on-ubuntu/))


## プライベート Docker レジストリとは

コンテナー化されたアプリケーションをクラウドに出荷するのには
Docker コンテナー イメージを構築し、格納できるようにどこかに
自分と他のユーザーが使用されます。

コンテナー イメージを作成して、クラウドに出荷する際に簡単です。
生成されたイメージを確実に格納するの困難になります。 そのため、
Docker と呼ばれる集中サービスを提供する [Docker Hub ][docker-hub] を格納するには
コンテナーは、クラウドでのイメージし、コンテナーを作成することができます。
いつでもそれらのイメージを使用します。

ただし、 [Docker Hub ][docker-hub] を格納するための有料サービスは、
プライベート アプリケーション コンテナー イメージ、Docker は、開発者のニーズを尊重し、
独自のプライベート Docker のイメージを格納する、オープン ソース ツールセットを提供します。
ファイアウォールまたはパブリック インターネット接続することがなくオンプレミスの背後にあるレジストリです。
Azure Blob ストレージはセキュリティで保護する簡単なので簡単に使えるを作成するには
自分で管理する Azure でのプライベート Docker registry を使用します。

## Azure で Docker Registry をホストする必要がある理由

Microsoft Azure で Docker Registry インスタンスをホストして格納すること、
Azure Blob ストレージにイメージ、いくつかの利点があることができます。

**セキュリティ:** ので、作業を行う、Docker イメージが Azure データ センター残されていません。
  Docker Hub を使用しているかのように、パブリック インターネットを通過しません。

**パフォーマンス:** 、Docker イメージは、同じ保存
データ センターまたはアプリケーションと地域。 これは、ため、イメージが表示されます。
高速かつ Docker Hub に比べて安全かつ確実にプルされます。

**信頼性:** Microsoft Azure Blob ストレージを使用して行うことができますを使用して
高可用性、冗長性、premium のような多くのストレージ プロパティ
storage (Ssd) とします。

## Azure BLOB ストレージを使用するための Docker Registry の構成

(、を読み取ることをお勧め [Docker Registry 2.0 のドキュメントの ][registry-docs]
続行します。) する前に

実行できます [[レジストリ設定] を構成する][registry-config] Docker Registry を 2 つの方法です。
そのための方法は次のとおりです。

1. 使用して、 `config.yml` ファイルです。 この場合、作成する必要が、
  上に別の Docker イメージ `レジストリ` イメージ。
2. 環境変数から既定の構成ファイルをオーバーライドします。
  作成して、別の Docker イメージを保守なしで実行されることを取得します。

このトピックに依存してオプション 2 を使用してわかりやすくする、
環境変数。

実行するのには、Docker Registry インスタンスします。
* Azure ストレージ アカウントを使用して、イメージを格納します。
* バーチャル マシンのポート 5000 でリッスンします。
* 認証が構成されていない (下記のメモを参照しないでください)

bash で次のような Docker コマンドを実行する必要があります。
ターミナル (置換 `< ストレージ アカウント >` と `< ストレージ キー >` 
資格情報を使用)。

```sh
$ docker run -d -p 5000:5000 \
     -e REGISTRY_STORAGE=azure \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTNAME="<storage-account>" \
     -e REGISTRY_STORAGE_AZURE_ACCOUNTKEY="<storage-key>" \
     -e REGISTRY_STORAGE_AZURE_CONTAINER="registry" \
     --name=registry \
     registry:2
```

コマンドが終了し後のコンテナーのホストを参照してください
実行して、プライベート Docker Registry インスタンス、 `docker ps`
ホスト上でコマンド:

```sh
$ docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED             STATUS              PORTS                    NAMES
3698ddfebc6f        registry:2          "registry cmd/regist   2 seconds ago       Up 1 seconds        0.0.0.0:5000->5000/tcp   registry
```

> [AZURE.IMPORTANT] Docker レジストリのセキュリティの構成
このドキュメントで説明されていないと、レジストリにアクセスできます
すべてのユーザーへのポートを開く場合、既定では認証なし
レジストリの仮想マシンのエンドポイントのポートまたはロード バランサーの場合は、
上記の展開コマンドを使用します。
>
> [構成 Docker をお読みください。
セキュリティで保護する方法については、ドキュメントをレジストリ] の [レジストリの構成]、
レジストリ インスタンスとイメージ。

## 次のステップ

レジストリを設定したら、使用できます。 Docker [レジストリ ドキュメント] を開始します。


[docker-hub]: https://hub.docker.com/ 
[registry]: https://github.com/docker/distribution 
[registry-docs]: http://docs.docker.com/registry/ 
[registry-config]: http://docs.docker.com/registry/configuration/ 

