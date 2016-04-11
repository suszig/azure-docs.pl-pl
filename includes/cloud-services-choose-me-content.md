<a name="tellmecs"></a>
## Cloud Services の概要

Cloud Services は、サービスとしてのプラットフォーム (PaaS) の 1 つの例です。 ような [App Services](app-service-web-overview.md), 、このテクノロジは、低運用コスト、信頼性の高いスケーラブルなアプリケーションをサポートするために設計されています。 同じように、 [App Services](app-service-web-overview.md) ので、Vm でホストされても、クラウド サービス、ただし、Vm に対してより詳細に制御があります。 独自のソフトウェアを Cloud Services の VM にインストールして、リモートで操作できます。

![cs_diagram](./media/cloud-services-choose-me-content/diagram.png) 

詳細に制御にもなります少ない使いやすさです。追加のコントロール オプションを必要な場合以外は、迅速かつ容易に web アプリケーションでは通常され、クラウド サービスに比較 App service Web Apps で実行されます。 

テクノロジには、2 つの若干異なる VM オプションが用意されています。 インスタンスの *web ロール* 中のインスタンスに、IIS で Windows Server のバリアントを実行 *ワーカー ロール* IIS ことなく、同じ Windows Server 形態を実行します。 Cloud Services アプリケーションはこの 2 つのオプションの組み合わせに依存します。 

これらのわずかな相違がある 2 つの VM ホスト オプションを任意に組み合わせて、クラウド サービスで使用できます。

* **Web ロール**  
  IIS に自動的に配置された web アプリを Windows Server を実行します。
* **Worker ロール**  
  Windows Server IIS 機能なしに実行されます。

たとえば、単純なアプリケーションは Web ロールだけを使用するかもしれませんが、複雑なアプリケーションは Web ロールを使用してユーザーから受け取った要求を処理してから、それらの要求が作成する作業を Worker ロールに渡して処理することがあります  (この通信を使えたら [Service Bus](../articles/service-bus/fundamentals-service-bus-hybrid-solutions.md) または [Azure キュー](../articles/storage/storage-introduction.md).)

上の図に示すように、1 つのアプリケーションのすべての VM は同じクラウド サービスで実行されます。 このため、ユーザーは 1 つのパブリック IP アドレスを通してアプリケーションにアクセスし、要求はアプリケーションの VM 間で自動的に負荷分散されます。 プラットフォームは [スケールおよびデプロイ](../articles/cloud-services/cloud-services-how-to-scale.md) ハードウェア障害の 1 つの点を回避する方法でクラウド サービス アプリケーションで Vm です。 

アプリケーションは仮想マシンで実行しますが、Cloud Services は IaaS ではなく PaaS を提供することを理解することが重要です。 次の例を考えてみましょう。Azure Virtual Machines のような IaaS では、アプリケーションが動作する環境を最初に作成して構成してから、この環境にアプリケーションをデプロイします。 この方法では、パッチが適用された新しいオペレーティング システムのバージョンを各 VM にデプロイするなど、ほとんどの管理をユーザーが担当します。 それに対し、PaaS では環境が既に存在するかのように管理できます。 ユーザーはアプリケーションをデプロイするだけで済みます。 オペレーティング システムの新バージョンのデプロイをはじめ、実行するプラットフォームの管理がユーザーに代わって処理されます。

## スケーリングと管理
Cloud Services では、ユーザーは仮想マシンを作成しません。 それぞれの数などを Azure に指示する構成ファイルを提供する代わりに、 **web ロール インスタンスに 3 つの** と **2 つのワーカー ロール インスタンス**, 、プラットフォームでは、それらを作成するとします。  まだ選択して [大きさはどの程度](../articles/cloud-services/cloud-services-sizes-specs.md) Vm をバックアップする必要がありますが明示的に作成しないに自分でします。 アプリケーションが高い負荷を処理する場合は、追加の VM を要求すると、Azure がそれらのインスタンスを作成します。 負荷が減少した場合は、それらのインスタンスをシャットダウンして支払いを停止できます。

Cloud Services アプリケーションは通常、2 つの手順から成るプロセスで利用可能になります。 開発者は最初 [アプリケーションをアップロード](../articles/cloud-services/cloud-services-how-to-create-deploy.md) プラットフォームのステージング領域にします。 アプリケーションを稼働する準備が整ったら、Microsoft Azure 管理ポータルを使用して運用開始を要求します。 これは、 [ステージングと実稼働の切り替え](../articles/cloud-services/cloud-services-nodejs-stage-application.md) ダウンタイムなど、ユーザーに影響を与えずに新しいバージョンにアップグレードする実行中のアプリケーションを使用して実行できます。 

## Monitoring
Cloud Services は監視も提供します。 Azure Virtual Machines と同様に、故障した物理サーバーを検出し、そのサーバーで実行していた VM を別のマシンで再開します。 さらに、Cloud Services はハードウェアの故障だけではなく、エラーが発生した VM やアプリケーションも検出します。 Virtual Machines と異なり、各 Web ロール内と Worker ロール内にエージェントが含まれているので、エラーが発生したときに、新しい VM とアプリケーションのインスタンスを開始できます。

PaaS という Cloud Services の本質には、他の含意もあります。 最も重要なことの 1 つは、このテクノロジを基盤に構築されるアプリケーションは、Web または Worker ロール インスタンスでエラーが発生したときに正しく実行するように記述することが必要です。 これを実現するには、Cloud Services のアプリケーションがそれ自体の VM のファイル システムで状態を維持してはなりません。 Azure Virtual Machines で作成された VM と異なり、Cloud Services VM への書き込みは永続的ではありません。その点で Virtual Machines のデータ ディスクとは大きく異なります。 Cloud Services アプリケーションはすべての状態を SQL Database、BLOB、テーブルか、その他の外部ストレージに明示的に書き込む必要があります。 この方法でアプリケーションを構築すると、スケーリングしやすく、耐障害性が備わります。この 2 つは Cloud Services の重要なゴールです。


