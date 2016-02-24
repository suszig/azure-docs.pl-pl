<properties
    pageTitle="仮想マシン スケール セットの概要 | Microsoft Azure"
    description="仮想マシン スケール セットについて説明します。"
    services="virtual-machines"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="11/11/2015"
    ms.author="guybo"/>

# 仮想マシン スケール セットの概要

仮想マシン スケール セットは、同一の VM のセットをデプロイおよび管理するために使用できる Azure コンピューティング リソースです。 すべての VM が同じ構成になっており、VM を事前にプロビジョニングする必要がない、真の自動スケールをサポートするように設計されています。そのため、ビッグ コンピューティング、ビッグ データ、コンテナー化されたワークロードなどを対象にした大規模サービスを簡単に構築できます。

[AZURE.INCLUDE [virtual-machines-vmss-preview](../../includes/virtual-machines-vmss-preview-include.md)]

コンピューティング リソースをスケール アウトしたりスケール インしたりする必要のあるアプリケーションでは、複数の障害ドメインと更新ドメインに対してスケール操作が暗黙的にバランシングされます。 VM スケールへの入門セットを参照してください、最近 [Azure ブログのお知らせ](https://azure.microsoft.com/blog/azure-vm-scale-sets-public-preview)します。

以下のビデオで、VM スケール セットが詳しく解説されています。

 - [Mark Russinovich が語る Azure スケール セット](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  

 - [仮想マシン スケール セットを Guy Bowerman が解説](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## VM スケール セットの作成と管理

VM のスケール設定が定義でき、JSON テンプレートを使用して展開し、 [REST Api](https://msdn.microsoft.com/library/mt589023.aspx) 個別の Azure リソース マネージャーの仮想マシンと同じようにします。 そのため、Azure リソース マネージャーの標準的なデプロイ方法を利用することができます。 テンプレートの詳細については、次を参照してください。 [Azure リソース マネージャーの作成テンプレート](../resource-group-authoring-templates.md)します。

VM スケール セットのサンプル テンプレートは、Azure Quickstart teamplates GitHub リポジトリから入手できます。

[https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates) -にあるテンプレートの検索 _vmss_ タイトルにします。

これらのテンプレートの詳細ページには、ポータルのデプロイ機能にリンクしているボタンが表示されます。 VM スケール セットをデプロイするには、このボタンをクリックし、ポータルで必要なすべてのパラメーターを指定します。 リソースで大文字がサポートされているかどうかが不明な場合は、常に小文字のパラメーター値を使用する方が安全です。 以下のサイトで、VM スケール セット テンプレートについてやさしく解説したビデオをご覧いただけます。

[VM スケール セット テンプレートの解説](https://channel9.msdn.com/Blogs/Windows-Azure/VM-Scale-Set-Template-Dissection/player)

## VM スケール セットのスケール アウトとスケール イン

VM の拡張セット内の仮想マシンの数を増減するだけで変更、 _容量_ プロパティと、テンプレートを再展開します。 このように単純なので、Azure 自動スケールでサポートされていないカスタム スケール イベントを定義する場合に、簡単に独自のカスタム スケール層を記述できます。

容量を変更するためにテンプレートを再デプロイする場合は、SKU と更新された容量だけが含まれている、非常に小さいテンプレートを定義できます。 この例のとおりです: [https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-scale-in-or-out/azuredeploy.json](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-linux-nat/azuredeploy.json)します。

自動的に拡張されるスケール セットを作成する手順を紹介する、次を参照してください [仮想マシンのスケール設定で自動的にスケール マシン。](virtual-machines-vmss-walkthrough.md)

## VM スケール セットの監視

使用することは現在推奨、 [Azure リソース エクスプ ローラー](https://resources.azure.com) セットを VM のスケールを表示します。 VM スケール セットは、Microsoft.Compute の下のリソースなので、このサイトから以下のリンクを展開すると表示できます。

    subscriptions -> your subscription -> resourceGroups -> providers -> Microsoft.Compute -> virtualMachineScaleSets -> your VM scale set -> etc.

## VM スケール セットのシナリオ

このセクションでは、標準的な VM スケール セットのシナリオをいくつか紹介します。 一部の高レベルの Azure サービス (Batch、Service Fabric、Azure Container Service など) も、これらのシナリオを使用します。

 - **RDP スケールの VM に ssh で通信設定インスタンス/** - VNET 内に VM スケール セットを作成し、個々 の Vm では、パブリック IP アドレスを割り当てられていません。 これは、望ましいことです。通常は、コンピューティング グリッド内のすべてのステートレス リソースに個別の IP アドレスを割り当てるための費用や管理オーバーヘッドを避けたいためです。また、これらの VM には VNET 内の他のリソース (ロード バランサー、スタンドアロン仮想マシンなど、パブリック IP アドレスを持つリソース) から簡単に接続できます。

 - **NAT 規則を使用して Vm に接続** -パブリック IP アドレスを作成、ロード バランサーに割り当てるおよび IP アドレスのポートを VM のスケール設定では、VM 上のポートにマップされる受信の NAT 規則を定義することができます。 例: 

    パブリック IP ポート]-> [50000 vmss\_0]-> [ポート 22 を ->
    パブリック IP ポート]-> [50, 001 vmss\_1]-> [ポート 22 を ->
    パブリック IP ポート]-> [50002 vmss\_2]-> [ポート 22 を ->

    1 つのパブリック IP を使用して設定するスケール内のすべての VM に SSH 接続を有効にする NAT 規則を使用する VM スケール セットを作成する例を次に示します: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)

    RDP と Windows で作業を行うことの例を次に示します: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)

 - **"Jumpbox"を使用して Vm に接続** - VM スケール セットを作成して、スタンドアロンの VM で同じ VNET、スタンドアロンの VM および VM のスケール設定の Vm が VNET/サブネットによって定義されている、内部 IP アドレスを使用して相互に接続できる場合です。 パブリック IP アドレスを作成し、スタンドアロン VM に割り当てる場合は、スタンドアロン VM に RDP または SSH 接続して、そのコンピューターから VM スケール セット インスタンスに接続できます。 これでお気付きかもしれませんが、単純な VM スケール セットは、本質的に、既定の構成のパブリック IP アドレスを持つ単純なスタンドアロン VM よりも安全です。

    この方法の例は、このテンプレートを作成、スタンドアロンの Vm の VM スケール セット ベースのクラスターを管理するマスター VM で構成される単純な Mesos クラスター: [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)

 - **ラウンド ロビンの負荷分散 VM スケールを行い、インスタンスを設定する** -負荷分散と、Azure ロード バランサーを構成するには「ラウンド ロビン」方式を使用する Vm の計算クラスターへの作業を提供する場合は、それに応じてルールします。 また、指定されたプロトコル、間隔、および要求パスでポートを ping して、アプリケーションが実行されていることを確認するためのプローブを定義することもできます。

    以下に、IIS Web サーバーを実行している VM の VM スケール セットを作成し、各 VM が受け取る負荷を分散するロード バランサーを使用する例を示します。 各 VM での特定の URL に ping を実行する HTTP プロトコルを使用: [https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json](https://github.com/gbowerman/azure-myriad/blob/master/vmss-win-iis-vnet-storage-lb.json) -Microsoft.Network/loadBalancers リソースの種類と networkProfile と、virtualMachineScaleSet で extensionProfile を確認します。

 - **PaaS クラスター マネージャーでのコンピューティング クラスターとセットの VM のスケールを展開** -VM のスケール設定は、次世代のワーカー ロールとも呼ばします。 有効な説明ですが、スケール セットの機能を PaaS v1 worker ロール機能と混同させる危険性があります。 ある意味で、VM スケール セットは、真の "worker ロール" または worker リソースを提供します。これにより、プラットフォーム/ランタムに依存せず、カスタマイズ可能で、Azure リソース マネージャー IaaS に統合された、汎用コンピューティング リソースを利用できます。

    PaaS v1 worker ロール、プラットフォーム、および実行時のサポート (Windows プラットフォーム イメージのみ) という点では限られたしかは、VIP スワップ、構成可能なアップグレードの設定、ランタイム/アプリ展開固有の設定がないかなどのサービスも含まれています。 _まだ_ VM で利用可能なセットを拡大/縮小、またはその他のは、Service Fabric のような高レベル PaaS サービスに実現されます。 このような点を踏まえておけば、VM スケール セットは PaaS をサポートするインフラストラクチャであると見なすことができます。 つまり、 Service Fabric のような PaaS ソリューションや Mesos のようなクラスター マネージャーを、スケーラブルなコンピューティング層として VM スケール セット上に構築できます。

    以下に、スタンドアロン VM と同じ VNET 内に VM スケール セットをデプロイする Mesos クラスターの例を示します。 スタンドアロンの VM は、Mesos マスタであり、VM の拡張セットは、スレーブ ノードのセットを表します。 [https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json](https://github.com/gbowerman/azure-myriad/blob/master/mesos-vmss-simple-cluster.json)します。 将来のバージョン、 [Azure コンテナー サービス](https://azure.microsoft.com/blog/azure-container-service-now-and-the-future/) VM スケール セットに基づくこのシナリオの他の複雑な/書き込まれたバージョンを展開します。

## VM スケール セットのパフォーマンスとスケールのガイダンス

- パブリック プレビュー期間中は、複数の VM スケール セット内に 500 個を超える VM を同時に作成しないでください。
- ストレージ アカウントあたり 40 個以下の VM を計画します。
- 各ストレージ アカウント名の最初の文字は、できるだけ重複しないようにします。  例の VMSS テンプレート [Azure クイック スタート テンプレート](https://github.com/Azure/azure-quickstart-templates/) これを行う方法の例を紹介します。
- カスタム VM を使用している場合は、1 つのストレージ アカウントで、VM スケール セットあたり 40 個以下の VM を計画します。  VM スケール セットのデプロイを開始する前に、イメージをストレージ アカウントに事前コピーする必要があります。 詳細については、FAQ を参照してください。
- VNET あたり 2,048 個以下の VM を計画します。  この制限は、今後緩和されます。
- 作成できる VM の数は、どのリージョンでも、コンピューティング コア クォータによって制限されます。 コンピューティング クォータの制限を緩和するには、現時点でクラウド サービスや IaaS v1 で使用するためのコアの上限が高い場合でも、カスタマー サポートへの連絡が必要である場合があります。 クォータを照会するには、次の Azure CLI コマンドを実行することができます: _azure vm の一覧-使用状況_, 、および次の PowerShell コマンド: _Get AzureRmVMUsage_ (1.0 を使用して次の PowerShell のバージョンを使用する場合 _Get AzureVMUsage_)。

## VM スケール セットに関してよく寄せられる質問

**Q。**VM スケール セットには何個の VM を設定できますか?

**A.**複数のストレージ アカウントに分散できるプラットフォーム イメージを使用する場合は、100 個です。 カスタム イメージを使用する場合は、最大で 40 個です。プレビュー期間中は、カスタム イメージが 1 つのストレージ アカウントに制限されているためです。

**Q** VM スケール設定のどのような他のリソース制限が存でしょうか。

**A.**プレビュー期間中は、リージョンごとに複数のスケール セットで 500 個を超える VM を作成できないという制限があります。 既存の [Azure サブスクリプションのサービスの制限/](../azure-subscription-service-limits.md) を適用します。

**Q。**VM スケール セット内でデータ ディスクはサポートされていますか?

**A.**最初のリリースではされていません。 データを格納するためのオプションは、次のとおりです。

- Azure ファイル (SMB 共有ドライブ)

- OS ドライブ

- 一時ドライブ (ローカルで、Azure Storage にサポートされない)

- 外部データ サービス (リモート データベース、Azure テーブル、Azure BLOB など)

**Q。**VM スケール セットは、どの Azure リージョンでサポートされていますか?

**A.**Azure リソース マネージャーがサポートされているすべてのリージョンで、VM スケール セットがサポートされています。

**Q。**カスタム イメージを使用して VM スケール セットを作成するにはどうすればよいですか?

**A.**vhdContainers プロパティを空白のままにします。たとえば、次のようにします。

    "storageProfile": {
        "osDisk": {
            "name": "vmssosdisk",
            "caching": "ReadOnly",
            "createOption": "FromImage",
            "image": {
                "uri": [https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd](https://mycustomimage.blob.core.windows.net/system/Microsoft.Compute/Images/mytemplates/template-osDisk.vhd)
            },
            "osType": "Windows"
        }
    },


**Q。**VM スケール セット容量を 20 から 15 に減らすと、どの VM が削除されますか?

**A.**可用性を最大限に高めるために仮想マシンは、すべてのアップグレード ドメインと障害ドメインのスケール セットから均等に削除されます。

**Q。**その後、容量を 15 から 18 に増やす場合はどうなるでしょうか?

**A.**18 に増やすと、15、16、17 のインデックスを持つ VM が作成されます。 どちらの場合も、VM は FD と UD 間に分散されます。

**Q。**VM スケール セットで複数の拡張機能を使用する場合、実行順序を強制できますか?

**A.**直接的にはありませんが、カスタム スクリプト拡張機能のスクリプトが待機できる他の拡張を完了する (たとえば、拡張機能を監視することによってログ - を参照してください [https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install \_lap.sh](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh))。

**Q。**VM スケール セットは、Azure 可用性セットと連携できますか?

**A.**はい。 VM スケール セットは、3 つの FD と 5 つの UD を持つ、暗黙的な可用性セットです。 virtualMachineProfile の下のものは、何も構成する必要はありません。 今後のリリースでは、VM スケール セットが複数のテナントにまたがる可能性がありますが、現時点では 1 つの可用性セットです。

