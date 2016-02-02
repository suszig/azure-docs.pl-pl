<properties
   pageTitle="仮想マシンでの Docker および Compose | Microsoft Azure"
   description="Azure Virtual Machines での Compose と Docker の操作を簡単に紹介します。"
   services="virtual-machines"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="virtual-machines"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="vm-linux"
   ms.workload="infrastructure-services"
   ms.date="11/16/2015"
   ms.author="danlep"/>


# Docker と Compose を使用して Azure 仮想マシン上で複数コンテナー アプリケーションを定義して実行する

この記事は、Docker を使用する方法を示しますと [Compose](http://github.com/docker/compose) を定義し、Azure で Linux 仮想マシンで複雑なアプリケーションを実行します。 Compose (*Fig* の後継) では、単純なテキスト ファイルを使用して、複数の Docker コンテナーで構成されるアプリケーションを定義します。 次に、VM 上でアプリケーションを実行するためのあらゆる操作を実行する単一のコマンドで、アプリケーションを起動します。 たとえば、この記事では、バックエンド MariaDB SQL Database で WordPress ブログをすばやくセットアップする方法を示していますが、Compose を使用してさらに複雑なアプリケーションをセットアップすることもできます。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] [リソース マネージャー モデル](https://azure.microsoft.com/documentation/templates/docker-wordpress-mysql/)します。


Docker とコンテナーに新しい場合を参照してください、 [Docker 高レベル ホワイト ボード](http://azure.microsoft.com/documentation/videos/docker-high-level-whiteboard/)します。

## 手順 1: Docker ホストとしての Linux VM のセットアップ

Azure のさまざまな手順と Azure Markeplace で入手できるイメージを使用して、Linux VM を作成し、Docker ホストとしてセットアップできます。 たとえばを参照してください [Docker VM 拡張機能から Azure コマンド ライン インターフェイスを使用して](virtual-machines-docker-with-xplat-cli.md) Docker VM 拡張機能と、Ubuntu VM を作成する簡単な手順についてです。 Docker VM 拡張機能を使用すると、VM が自動的に Docker ホストとしてセットアップされます。 その記事の例では、使用する方法を示します、 [Mac、Linux、および Windows 用 Azure コマンド ライン インターフェイス](../xplat-cli-install.md) (Azure CLI)、VM を作成するサービス管理モードにします。

## 手順 2: Compose のインストール

Docker がインストールされた Linux VM を実行したら、SSH を使用してクライアント コンピューターから VM に接続します。 インストールする必要がある場合 [Compose](https://github.com/docker/compose/blob/882dc673ce84b0b29cd59b6815cb93f74a6c4134/docs/install.md) 次の 2 つのコマンドを実行しています。
>[AZURE.TIP] Docker VM 拡張機能を使用して VM を作成した場合、Compose は既にインストールされています。 これらのコマンドを省略して、手順 3. に進んでください。 VM に Docker を自分でインストールした場合は、Compose をインストールするだけでかまいません。

```
$ curl -L https://github.com/docker/compose/releases/download/1.1.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

$ chmod +x /usr/local/bin/docker-compose
```
>[AZURE.NOTE]"アクセスが拒否されました" という内容のエラーが発生した場合は、VM の /usr/local/bin ディレクトリが書き込み可能でない可能性があるため、Compose をスーパーユーザーとしてインストールする必要があります。 実行 `sudo-i`, 、2 つのコマンド、上、 `終了`します。

Compose のインストールをテストするには、次のコマンドを実行します。

```
$ docker-compose --version
```

ような出力が得られます `1.4.1 を docker 作成`します。


## 手順3: docker-compose.yml 構成ファイルの作成

次に、作成、 `docker-compose.yml` ファイルで、単なるテキスト構成ファイル、VM 上で実行する Docker コンテナーを定義します。 このファイルでは、各コンテナーで実行するイメージ (イメージは Dockerfile からのビルドも使用できます)、必要な環境変数と依存関係、ポート、コンテナー間のリンクなどを指定します。 Yml ファイルの構文の詳細については、「 [docker-compose.yml のリファレンス](http://docs.docker.com/compose/yml/)します。

VM 上の作業ディレクトリを作成し、任意のテキスト エディターを使用して作成 `docker-compose.yml`します。 簡単な例を試すために、次のテキストをファイルにコピーしてください。 この構成からイメージを使用して、 [DockerHub レジストリ](https://registry.hub.docker.com/_/wordpress/) WordPress (オープン ソースのブログ作成およびコンテンツ管理システム) と、リンクされたバックエンド MariaDB データベースをインストールします。

 ```
 wordpress:
  image: wordpress
  links:
    - db:mysql
  ports:
    - 8080:80

db:
  image: mariadb
  environment:
    MYSQL_ROOT_PASSWORD: <your password>
 ```

## 手順 4: compose を搭載したコンテナーを起動します。

VM の作業ディレクトリで、次のコマンドを実行するだけです。

```
$ docker-compose up -d
```

指定された Docker コンテナーが起動 `docker-compose.yml`します。 次のような出力が表示されます。

```
Creating wordpress_db_1...
Creating wordpress_wordpress_1...
```

>[AZURE.NOTE] コンテナーがバックグラウンドで継続的に実行されるように、必ず起動時に **-d** オプションを使用してください。

コンテナーが起動することを確認するには、次のように入力します。 `ps の docker 作成`します。 次のような結果が表示されます。

```
Name             Command             State              Ports
-------------------------------------------------------------------------
wordpress_db_1     /docker-           Up                 3306/tcp
             entrypoint.sh
             mysqld
wordpress_wordpr   /entrypoint.sh     Up                 0.0.0.0:8080->80
ess_1              apache2-for ...                       /tcp
```

参照して直接 VM 上の WordPress に接続できるようになりました `http://localhost:8080`します。 インターネット経由で VM に接続する場合は、まず、パブリック ポート 80 をプライベート ポート 8080 にマッピングする HTTP エンドポイントを VM 上に構成します。 たとえば、Azure サービス管理デプロイメントで、次の Azure CLI コマンドを実行します。

```
$ azure vm endpoint create <machine-name> 80 8080
```

WordPress スタート画面が表示されます。この画面の手順に従ってインストールを完了し、アプリケーションを使用できます。

![WordPress のスタート画面][wordpress_start]


## 次のステップ

* チェック アウト、 [Compose CLI リファレンス](http://docs.docker.com/compose/reference/) と [ユーザー ガイド](http://docs.docker.com/compose/) のビルドと複数コンテナー アプリの展開の例についてです。
* か、Azure リソース マネージャー テンプレートを使用して、独自のプロパティまたは 1 つの影響、 [コミュニティ](http://azure.microsoft.com/documentation/templates/), 、Docker と compose を搭載したセットアップ アプリケーションで、Azure VM をデプロイします。 たとえば、 [Docker での WordPress ブログを展開](https://azure.microsoft.com/documentation/templates/docker-wordpress-mysql/) テンプレートは Docker と Compose を使用して、Ubuntu VM で MySQL バックエンドで WordPress を迅速に導入します。
* Docker Compose との統合、 [Docker Swarm](virtual-machines-docker-swarm.md) クラスターです。 手順については、「
[Docker Compose/swarm の統合](https://github.com/docker/compose/blob/master/SWARM.md) のシナリオです。




[wordpress_start]: ./media/virtual-machines-docker-compose-quickstart/WordPress.png 

