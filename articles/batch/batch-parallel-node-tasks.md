<properties
   pageTitle="並列タスクで Batch ノードの使用率を最大にする |Microsoft Azure"
   description="Azure Batch プール内の各ノードで同時実行タスクを実行することにより、使用するコンピューティング ノードの数を減らして効率を向上させ、コストを削減します"
   services="batch"
   documentationCenter=".net"
   authors="mmacy"
   manager="timlt"
   editor=""/>

   <tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="11/02/2015"
    ms.author="v-marsma"/>


# 同時実行ノード タスクで Azure Batch コンピューティング リソースの使用率を最大にする

大規模な並列タスクの実行は Azure Batch の中核機能であり、この機能は、タスクを実行する複数のコンピューティング ノードだけでなく、これらのノードでの同時実行タスクの実行にも及びます。 Batch では、並列タスク実行はスケールアップおよびスケールアウトします。

プールのコンピューティング ノードでの同時実行タスクの実行を有効にすると、プール内の少数のノードでのリソース使用率を最大化できます。 単一のタスクにすべてのノードのリソースを割り当てることでメリットがあるシナリオもありますが、複数のタスクでリソースを共有することにメリットがある状況もあります。

 - 複数のタスクがデータを共有できるときに**データ転送を最小限に抑える**。 このシナリオでは、共有データを少数のノードにコピーして、各ノードでタスクを並列に実行すると、データ転送料金を大幅に削減できます。各ノードにコピーされるデータを異なる地理的地域間で転送する必要がある場合は特にそうです。

 - タスクが実行中に不特定の回数、大量のメモリを短時間だけ必要とする場合に、**メモリの使用率を最大化する**。 多くのメモリを使用する少数の大きいノード インスタンスを使用して、このようなスパイクを効率的に処理できます。各ノードで実行する並列タスクは、ノードの十分にあるメモリを別々の時間に利用します。

 - プール内でノード間通信が必要な場合に、**ノード数の制限を緩和する**。 現在、ノード間通信用に構成されたプールは 50 ノードに制限されているので、このようなプール内の各ノードがタスクを並列に実行できると、より多くのタスクを同時に実行できます。

 - 初めてコンピューティング環境を Azure に移動するときなどに、**オンプレミスのコンピューティング クラスターをレプリケートする**。 既存の物理構成が現在 1 つのコンピューティング ノードで複数のタスクを実行している場合、ノード タスクの最大数を増やして、そのような構成をいっそう緊密にミラーできます。

## サンプル シナリオ

並列タスクの実行の利点を示すためには、たとえばタスク アプリケーションの CPU とメモリの要件を Standard\_D1 ノード サイズが、適切なですが、必要な時間にジョブを実行するためにこのような 1000年のノードが必要なします。 Standard\_D1 ノードを使用する代わりには、16 のコアを備えた Standard\_D14 ノードを使用し、並列タスクの実行を有効にした可能性があります。 この場合は、*ノードの数は 16 分の 1* なので、必要なのは 1000 ノードではなく 63 ノードだけです。 各ノードで大きなアプリケーション ファイルまたは参照データが必要な場合は、これでジョブの実行時間と効率を大幅に向上できます。

## 並列タスク実行を有効にする

並列タスク実行のための Batch ソリューションでのコンピューティング ノードの構成は、プールのレベルで行います。 バッチ .NET API を使用する場合、 [CloudPool.MaxTasksPerComputeNode][maxtasks_net] プールを作成するときにプロパティが設定されています。 バッチの REST API で、 [maxTasksPerNode ][maxtasks_rest] 要素は、プールの作成中に要求本文に設定します。

Azure Batch では、ノード設定あたりのタスクの数を、ノード コアの数の最大 4 倍まで設定できます。 たとえばのノードで、プールが構成されている場合は、サイズ「大」(4 コア)、 `maxTasksPerNode` 16 に設定することががあります。 コアの数の詳細で検出されたそれぞれのノード サイズに [クラウド サービスのサイズ](../cloud-services/cloud-services-sizes-specs.md), 、サービスの制限の詳細については記載されていると [クォータや、Azure Batch サービスの制限](batch-quota-limit.md)します。
> [AZURE.TIP] 考慮してください、 `maxTasksPerNode` を構築するときに、 [自動スケール式 ][enable_autoscaling] プール用です。 たとえば、評価される式 `$RunningTasks` ノードあたりのタスクが増加して大幅な影響を受ける可能性があります。 参照してください [スケールが自動的に Azure Batch プール内のノードをコンピューティング](batch-automatic-scaling.md) の詳細。

## タスクの分散

プール内のコンピューティング ノードがタスクを同時に実行できる場合、プール内のノード間へのタスクの分散方法を指定することが重要です。

使用して、 [CloudPool.TaskSchedulingPolicy][task_schedule] プロパティには、(「展開」)、プール内のすべてのノードでタスクを均等に割り当てられる必要があります、またはタスクがプール (「パッキング」) 内の別のノードに割り当てられている前に、できるだけ多くのタスクが各ノードに割り当てられることを指定できます。

この機能は役に立つ方法の例は、として検討してください、上記の例では Standard\_D14 ノードのプールを使用して構成、 [CloudPool.MaxTasksPerComputeNode][maxtasks_net] 値は 16 です。 場合、 [CloudPool.TaskSchedulingPolicy][task_schedule] で構成されて、 [ComputeNodeFillType ][fill_type] の *パック*, 、各ノードのすべての 16 コアの使用率を最大化しを許可するには、 [オートスケー リングのプール](./batch-automatic-scaling.md) にプール (割り当てられているタスクなしノード)、最小限に抑えるためのリソース使用状況およびコストから未使用のノードを排除するようにします。

## Batch .NET の例

これは、 [バッチ .NET ][api_net] API コード スニペットは、1 ノードあたり 4 つのタスクの最大値を使用して、大規模な 4 つのノードが含まれていて、プール内の別のノードにタスクを割り当てる前にタスクの各ノードが入力ポリシーのスケジュール タスクを指定するプールを作成する要求を示しています。 バッチ .NET API を使用してプールを追加する方法については、次を参照してください。 [BatchClient.PoolOperations.CreatePool][poolcreate_net]します。

        CloudPool pool = batchClient.PoolOperations.CreatePool(poolId: "mypool",
                                                            osFamily: "2",
                                                            virtualMachineSize: "large",
                                                            targetDedicated: 4);
        pool.MaxTasksPerComputeNode = 4;
        pool.TaskSchedulingPolicy = new TaskSchedulingPolicy(ComputeNodeFillType.Pack);
        pool.Commit();

## Batch REST の例

これは、 [バッチ REST ][api_rest] API スニペットは、ノードあたり 4 つのタスクの最大値を使用して、大規模な 2 つのノードを含むプールを作成する要求を示しています。 REST API を使用してプールを追加する方法については、次を参照してください。 [プール アカウントに追加する ][maxtasks_rest]します。

        {
          "id": "mypool",
          "vmSize": "Large",
          "osFamily": "2",
          "targetOSVersion": "*",
          "targetDedicated": 2,
          "enableInterNodeCommunication": false,
          "maxTasksPerNode": 4
        }

> [AZURE.NOTE] `MaxTasksPerNode` 要素と [MaxTasksPerComputeNode ][maxtasks_net] プロパティは、プールの作成時にのみ設定できます。 作成後のプールでは変更できません。

## サンプル プロジェクトの例

チェック アウト、 [ParallelNodeTasks ][parallel_tasks_sample] GitHub の使用方法を示す実際のコード サンプルのプロジェクト [CloudPool.MaxTasksPerComputeNode][maxtasks_net]します。 この c# コンソール アプリケーションを使用して、 [バッチ .NET ][api_net] を 1 つまたは複数のプールを作成するライブラリの計算ノードと負荷が変動をシミュレートするには、そのノードで構成可能な一連のタスクを実行します。 アプリケーションの出力では、各タスクを実行したノード、およびジョブ パラメーターと期間の概要が、詳しく示されています。 サンプル アプリケーションの 2 つの異なる実行からの出力の概要部分を以下に示します。

```
Nodes: 1
Node size: large
Max tasks per node: 1
Tasks: 32
Duration: 00:30:01.4638023
```

サンプル アプリケーションの 1 回目の実行では、プール内の 1 つのノードで、ノードごとに 1 タスクの既定値が使用されており、ジョブの実行時間は 30 分以上であることが示されています。

```
Nodes: 1
Node size: large
Max tasks per node: 4
Tasks: 32
Duration: 00:08:48.2423500
```

サンプルの 2 回目の実行では、ジョブの実行時間が大幅に短縮されています。これは、ノードごとのタスク数を 4 にしてプールが構成されているためであり、並列タスク実行によりほぼ 4 分の 1 の時間でジョブを完了できます。
> [AZURE.NOTE] 上記のジョブ期間には、プールの作成時間は含まれません。 上の各ジョブが前に作成されたプールに送信され、送信の時点でコンピューティング ノードは*アイドル*状態でした。

## Batch Explorer のヒート マップ

[Batch Explorer ][batch_explorer], 、Azure Batch のいずれかの [サンプル アプリケーション ][github_samples], を含む、 *ヒート マップ* タスクの実行の視覚化を提供する機能です。 実行時に、 [ParallelTasks ][parallel_tasks_sample] サンプル アプリケーション、ヒート マップ機能を使用して、各ノードで並列タスクの実行を簡単に視覚化できます。

![Batch Explorer のヒート マップ][1]

*Batch Explorer のヒート マップに 4 つのノードから成るプールが表示され、各ノードでは現在それぞれ 4 つのタスクが実行されている*


[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx 
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx 
[batch_explorer]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx 
[cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx 
[enable_autoscaling]: https://msdn.microsoft.com/library/azure/dn820173.aspx 
[fill_type]: https://msdn.microsoft.com/library/microsoft.azure.batch.common.computenodefilltype.aspx 
[github_samples]: https://github.com/Azure/azure-batch-samples 
[maxtasks_net]: http://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.maxtaskspercomputenode.aspx 
[maxtasks_rest]: https://msdn.microsoft.com/library/azure/dn820174.aspx 
[parallel_tasks_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/ParallelTasks 
[poolcreate_net]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.createpool.aspx 
[task_schedule]: https://msdn.microsoft.com/library/microsoft.azure.batch.cloudpool.taskschedulingpolicy.aspx 
[1]: ./media/batch-parallel-node-tasks\heat_map.png 

