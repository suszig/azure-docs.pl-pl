<properties
 pageTitle="HPC クラスターのコンピューティング リソースの自動拡大縮小 | Microsoft Azure"
 description="Azure で HPC Pack クラスターのコンピューティング リソースを自動的に拡大縮小する方法について説明します。"
 services="virtual-machines"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="09/28/2015"
 ms.author="danlep"/>


# クラスター ワークロードに合わせ、HPC Pack クラスターで Azure コンピューティング リソースを自動的に拡大縮小します。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)] リソース マネージャーのモデルです。


HPC Pack クラスターに Azure「バースト」ノードを展開する場合
Azure Vm の HPC Pack クラスターを作成する方法場合は、
自動的に拡大または縮小」の手順に従って、Azure コンピューティング リソース
ジョブと、クラスター上のタスクの現在のワークロードです。 これにより、
Azure リソースをより効率的に使用し、そのコストを制御します。
これを行うを使用して、
**AzureAutoGrowShrink.ps1** と共にインストールされている HPC PowerShell スクリプト
HPC Pack。
>[AZURE.TIP] HPC Pack 2012 R2 Update 2 以降、HPC Pack には、組み込みが含まれます
サービスを自動的に拡大および縮小 Azure バースト ノードまたは
Azure VM はコンピューティング ノードです。 [HPC 内の設定で、サービスを構成します。
IaaS 展開 script](virtual-machines-hpcpack-cluster-powershell-script.md) をパックまたは手動で設定、 **AutoGrowShrink** クラスター
プロパティから取得) で呼び出すことで、通知を送信します。 [の新機能 Microsoft HPC Pack 2012 R2 更新プログラムを参照してください。
2] (https://technet.microsoft.com/library/mt269417.aspx)。

## 前提条件

* **HPC Pack 2012 R2 更新プログラム 1 以降のクラスター** - **AzureAutoGrowShrink.ps1** スクリプトが %CCP_HOME%bin フォルダーにインストールされています。 クラスターのヘッド ノードはオンプレミスにするか、Azure VM に配置できます。 参照してください [HPC Pack 使用したハイブリッド クラスターをセットアップ](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) の内部設置型ヘッド ノードと Azure「バースト」で開始します。 参照してください、 [HPC Pack IaaS デプロイメント スクリプト](virtual-machines-hpcpack-cluster-powershell-script.md)) をすばやく Azure Vm の HPC Pack クラスターをデプロイします。

* **Azure バースト ノードを含むクラスターの場合** -HPC Pack がインストールされているクライアント コンピューターで、またはヘッド ノードではスクリプトを実行します。 クライアント コンピューターで実行する場合、ヘッド ノードをポイントするように $env:CCP_SCHEDULER 変数を適切に設定します。 Azure の「バースト」ノードをクラスターに追加しておく必要がありますが、「未デプロイ」の状態になっている場合があります。

* **Azure VM にデプロイされているクラスター** - ヘッド ノード VM でスクリプトを実行します。そこでインストールされている **Start-HpcIaaSNode.ps1** スクリプトと **Stop-HpcIaaSNode.ps1** スクリプトに依存するためです。 これらのスクリプトは、さらに、Azure 管理証明書を要求または、発行設定ファイル (を参照してください [HPC pack コンピューティング ノードの管理は、Azure におけるクラスター](virtual-machines-hpcpack-cluster-node-manage.md))します。 必要なコンピューティング ノード VM がすべてクラスターに追加されていることを確認します。ただし、「停止」状態になっている場合があります。

## 構文

```
AzureAutoGrowShrink.ps1
[[-NodeTemplates] <String[]>] [[-JobTemplates] <String[]>] [[-NodeType] <String>]
[[-NumOfQueuedJobsPerNodeToGrow] <Int32>]
[[-NumOfQueuedJobsToGrowThreshold] <Int32>]
[[-NumOfActiveQueuedTasksPerNodeToGrow] <Int32>]
[[-NumOfActiveQueuedTasksToGrowThreshold] <Int32>]
[[-NumOfInitialNodesToGrow] <Int32>] [[-GrowCheckIntervalMins] <Int32>]
[[-ShrinkCheckIntervalMins] <Int32>] [[-ShrinkCheckIdleTimes] <Int32>]
[-UseLastConfigurations] [[-ArgFile] <String>] [[-LogFilePrefix] <String>]
[<CommonParameters>]
```
## パラメーター

 * **NodeTemplates** - ノードを拡大縮小するための範囲を定義するノード テンプレートの名前。 指定されていない場合 (既定値は @() です)、**NodeType** の値が AzureNodes のとき、**AzureNodes** ノード グループのすべてのノードが範囲内となり、**NodeType** の値が ComputeNodes のとき、**ComputeNodes** ノード グループのすべてのノードが範囲内になります。

 * **JobTemplates** - ノードを拡大するための範囲を定義するジョブ テンプレートの名前。

 * **NodeType** の拡大および縮小するノードの種類。 サポートされる値は次のとおりです。

     * **AzureNodes** – オンプレミスまたは Azure IaaS クラスターの Azure PaaS (バースト) ノード用。

     * **ComputeNodes** - Azure IaaS クラスターにのみ存在するコンピューティング ノード VM 用。

* **NumOfQueuedJobsPerNodeToGrow** - 1 つのノードを拡大するために必要で、キューに登録されたジョブの数。

* **NumOfQueuedJobsToGrowThreshold** - 拡大プロセスを始めるためにキューに登録されたジョブのしきい値数。

* **NumOfActiveQueuedTasksPerNodeToGrow** - 1 つのノードを拡大するために必要で、キューに登録されたアクティブなタスクの数。 **NumOfQueuedJobsPerNodeToGrow** に 0 より大きい値が指定されている場合、このパラメーターは無視されます。

* **NumOfActiveQueuedTasksToGrowThreshold** - 拡大プロセスを始めるためにキューに登録されたアクティブなタスクのしきい値数。

* **NumOfInitialNodesToGrow** - 範囲内の全ノードが「**未デプロイ**」または「**停止 (割り当て解除)**」の場合、拡大するノードの初回最小数。

* **GrowCheckIntervalMins** - 拡大チェック間の間隔 (分単位)。

* **ShrinkCheckIntervalMins** - 縮小チェック間の間隔 (分単位)。

* **ShrinkCheckIdleTimes**- ノードがアイドル状態であることを示す継続的縮小チェックの数 (**ShrinkCheckIntervalMins** で分割)。

* **UseLastConfigurations *** 引数ファイルに保存されている以前の構成。

* **ArgFile** - スクリプトを実行する構成を保存し、更新するための引数ファイルの名前。

* **LogFilePrefix** - ログ ファイルのプレフィックス名。 パスを指定できます。 既定では、ログは現在の作業ディレクトリに書き込まれます。

### 例 1

次の例でデプロイされた Azure バースト ノードを構成します
既定の AzureNode テンプレートが大きくなり、自動的に縮小します。 すべての場合、
ノードが最初に、 **未展開** 状態にある場合に、少なくとも 3 つのノードには
。 キューに置かれたジョブの数が 8 を超えている場合、スクリプトは、ノードを起動します。
キューに置かれたジョブの比率を超えるまで
**NumOfQueuedJobsPerNodeToGrow**します。 3 でアイドル状態になるノードが見つかった場合
連続するいくつかのアイドル時間が停止しています。

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### 例 2

次の例は、Azure コンピューティング ノード Vm の展開を構成します。
テンプレートを使用して、既定の ComputeNode を拡大および縮小を自動的にします。
既定のジョブ テンプレートで構成されたジョブは、そのスコープを定義する、
クラスターのワークロードです。 すべてのノードが最初に停止した場合に
少なくとも 5 つのノードが開始されます。 アクティブなキューに置かれたタスクの数を超えると
15、スクリプトが開始ノードの比率を超えるまで
アクティブなキューに置かれたタスクを **NumOfActiveQueuedTasksPerNodeToGrow**します。 場合は、
10 個の連続したアイドル時間のアイドル状態になるノードが見つかると、停止されています。

```
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```





