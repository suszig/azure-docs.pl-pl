<properties
   pageTitle="Azure Container Service の概要 | Microsoft Azure"
   description="Azure Container Service (ACS) を使用すると、コンテナー化されたアプリケーションを実行するように事前構成されている仮想マシンのクラスターを簡単に作成、構成および管理できます。"
   services="virtual-machines"
   documentationCenter=""
   authors="rgardler"
   manager="nepeters"
   editor=""
   tags="acs, azure-container-service"
   keywords="Docker, Containers, Micro-services, Mesos, Azure"/>
   
<tags
   ms.service="virtual-machines"
   ms.devlang="na"
   ms.topic="home-page"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/02/2015"
   ms.author="rogardle"/>

# Azure Container Service の概要

Azure コンテナー サービス (ACS) を簡単に作成方法を使用します。
構成、および、バーチャル マシンのクラスターの管理
コンテナー化アプリケーションを実行する構成済みです。 最適なを使用します。
一般的なオープン ソースのスケジュール設定およびオーケストレーションのツールの構成
ACS を使用して、既存のスキルを使用したり、大規模な利用し、
コンテナーに基づく展開および管理するコミュニティの専門知識の本文を拡大
Microsoft Azure 上のアプリケーションです。

<br />
![ACS では、Azure 上の複数のホストのコンテナー化されたアプリケーションを管理できます。](./media/acs-intro/acs-cluster.png)
<br /><br />

ACS を活用すること、アプリケーション コンテナー Docker
完全に移植できます。 選択した Marathon Chronos もサポートしています、
これらのアプリケーションができることを確認する Apache Mesos または Docker Swarm
何千も数万のコンテナーにスケーリングされます。

Azure コンテナー サービスを活用することができます、
アプリケーションを維持しながら、Azure のエンタープライズ グレード機能
移植性をオーケストレーションのレイヤーに指定するなどです。

サービスは、プレビュー中は、お願いするテストに関心があります。
サービス [自己指名](http://aka.ms/acspreview)します。 プレビュー アクセスが提供できたら、デプロイメント テンプレートおよび開始手順を含む詳細を電子メールでお送りします。 サービスを使用する必要があります、Azure サブスクリプションしないまだが場合 1 つ、なぜにサインアップします。
[解放
試用版](http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=AA4C1C935)でしょうか。

Azure Container Service の使用
-----------------------------

Azure コンテナー サービスでの目標は、コンテナーを提供するには
これはオープン ソース ツールおよびテクノロジを使用して環境をホストしています。
現在、お客様の間で普及しています。 この目的を達成するので、標準に開示します。
Docker と、選択した orchestrator の API エンドポイント。 これらを使用します。
エンドポイントと通信ソフトウェアを活用することができます。
これらのエンドポイント。 たとえば、Docker Swarm エンドポイントの場合
Docker を作成する際に、Apache Mesos する場合を使用することもできます。
DCOS CLI の使用を選択します。

Azure Container Service を使用した Docker Cluster の構成
-------------------------------------------------------

取得したら [要求](http://aka.ms/acspreview) へのアクセス権と
プレビューすることができますを使用して Azure リソース マネージャーの数値のいずれか
最初、Azure でクラスターを展開するためのテンプレート
できます。 これらのテンプレートを使用することができますサービスをすばやく作成し、
すぐにアプリケーションの展開を開始します。 取得を開始します。
のみのサイズ、クラスターとするかどうかを決定する必要があります。
アプリケーションを管理するのにには、Docker Swarm または Apache Mesos を使用します。

こともできます [コマンドラインを使用して](/documentation/articles/resource-group-template-deploy/)
これらの同じテンプレートを使用して Azure コンテナー サービスを作成します。 した後
ことができますが、これらのテンプレートの構造に精通しています。
独自に作成し、Azure コンテナーの作成を完全に自動化します。
サービスのクラスター。

参加者をプレビューする完全なドキュメントとサポートが提供されます。
一般に、サービスが開かれた後は、ここで公開されます。

アプリケーションのデプロイ
------------------------

Docker Swarm または Apache のいずれかを選択することはプレビュー期間中
Mesos (と共に DCOS Marathon と DCOS Chronos フレームワーク)
オーケストレーションです。 

### Apache Mesos の使用

Apache Mesos は Apache Software で格納されているオープン ソース プロジェクトです。
Foundation。 一部を表示、 [有名
IT](http://mesos.apache.org/documentation/latest/powered-by-mesos/) として
ユーザーおよび寄稿者です。

![Swarm の構成がされた ACS のエージェントとマスターを示します。](media/acs-intro/acs-mesos.png)

Mesos には、優れた機能セットがあります。

-   何万ノードにも拡張できるスケーラビリティ

-   ZooKeeper を使用したフォールト トレランス機能を備えたレプリケートされたマスターおよびスレーブ

-   Docker コンテナーのサポート

-   Linux コンテナーを使用したタスク間のネイティブな分離

-   (メモリ、CPU、ディスクおよびポートの) マルチリソース スケジューリング

-   新しい並列アプリケーションを開発するための Java、Python、および C++ API

-   クラスターの状態を参照するための Web UI

Mesos は大きな数のサポート
[フレームワーク](http://mesos.apache.org/documentation/latest/frameworks/)
Azure コンテナー上のワークロードのスケジュール設定に使用することができます。
サービス。 ACS には、既定で Marathon および Chronos フレームワークが含まれています。

#### Marathon および Chronos の使用

Marathon でのサービスのクラスター全体にわたる init と制御システムは、します。
cgroups または、ACS は、Docker コンテナーです。 理想的なソリューションであります。
パートナー Chronos Mesos、フォールト トレラントなジョブのスケジューラにします。
依存関係と時間ベースのスケジュールを処理します。

Marathon と Chronos は、展開することができます Web UI を提供する、
アプリケーション。 このような URL にアクセスします。
`http://DNS\_PREFIX.REGION.cloudapp.azure.com`
DNS\_PREFIX と地域がどちらも定義されている展開時にします。 の
コースは、また、独自の DNS 名を指定することができます。

Marathon と通信するため、REST Api を使用することもでき、
Chronos します。 各ツールの使用はクライアント ライブラリのいくつか
HTTP を使用して、さまざまな言語と、もちろんをカバーすることができます。
任意の言語でプロトコルです。 さらに、多くの一般的な DevOps ツールを提供します。
これらのスケジューラをサポートします。 これにより、柔軟に、
ACS クラスタを使用する場合、チームを操作します。

参加者をプレビューする完全なドキュメントとサポートが提供されます。
一般に、サービスが開かれた後は、ここで公開されます。

### Docker Swarm の使用

Docker Swarm は、Docker のネイティブ クラスタリングを提供しています。 Docker Swarm ため
標準の Docker API は既にと通信する任意のツールの機能します。
Docker デーモンは、Swarm を使用して複数のホストに透過的にスケーリングするには
Azure コンテナー サービスです。 

![Apache Mesos の使用が構成がされた ACS のジャンプボックス、エージェントおよびマスターを示します。](media/acs-intro/acs-swarm.png)

Swarm クラスター上のコンテナーを管理するためのサポートされているツールなどがだけではありません。
次の。

-   Dokku

-   Docker CLI および Docker Compose

-   Krane

-   Jenkins

参加者をプレビューする完全なドキュメントとサポートが提供されます。
一般に、サービスが開かれた後は、ここで公開されます。

アクセスの取得
--------------

テスト関心があるものにあたってサービスはプレビュー段階ですが、
サービス [自己指名](http://aka.ms/acspreview)します。 Azure は、まず
サブスクリプションの場合、なぜにサインアップし、まだ 1 つがない場合、 [無料
試用版](http://www.windowsazure.com/en-us/pricing/free-trial/?WT.mc_id=AA4C1C935)でしょうか。

ビデオ
------
AzureCon に関するお知らせ:

> [AZURE.VIDEO azurecon-2015-deep-dive-on-the-azure-container-service-with-mesos]  

ACS の概要:  

> [AZURE.VIDEO connect-2015-getting-started-developing-with-docker-and-azure-container-service]

