<properties
    pageTitle="Azure Batch での効率的なリスト クエリ | Microsoft Azure"
    description="Azure Batch のプール、ジョブ、タスク、コンピューティング ノードなどのエンティティを問い合わせた場合に返されるデータ量を減らすことで、パフォーマンスを向上させます。"
    services="batch"
    documentationCenter=".net"
    authors="mmacy"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows"
    ms.workload="big-compute"
    ms.date="10/12/2015"
    ms.author="v-marsma"/>

# 効率的な Azure Batch サービスのクエリ

この記事では、項目の数とを使用する場合に返されるデータの量を削減する方法を説明します、 [バッチ .NET][api_net] ジョブ、タスクの一覧は、Batch サービスを照会する API は、ノード、および詳細を計算します。

Azure Batch は大規模なコンピューティングです。運用環境では、ジョブ、タスク、コンピューティング ノードのようなエンティティは数千単位になることがあります。 そのため、そのようなアイテムに関する情報を取得するとき、クエリごとに大量のデータを転送しなければならないことがあります。 クエリごとに返される情報の項目数と種類を制限すると、クエリの時間が短縮され、アプリケーションのパフォーマンスが改善されます。

ジョブ、タスク、コンピューティング ノードのリストは、Azure Batch を使用するほぼすべてのアプリケーションが多くの場合、頻繁に実行する必要がある操作の例です。 監視が一般的な使用例です。 たとえば、プールの容量と状態を判断するには、クエリを実行するそのプールのすべてのコンピューティング ノードが必要になります。 また、ジョブのいずれかのタスクがまだキューにあるかどうかを判断するために、それらのタスクを照会する場合もあります。

これは、 [バッチ .NET][api_net] API のコード スニペットでは、すべての包括的なそれらのタスクのプロパティと共に、ジョブに関連するタスクを取得します。

```
// Get a collection of all of the tasks and all of their properties for job-001
IPagedEnumerable<CloudTask> allTasks = batchClient.JobOperations.ListTasks("job-001");
```

はるかに効率的なリストのクエリが可能、ただし、提供することによって、 [ODATADetailLevel][odata] に、 [JobOperations.ListTasks][net_list_tasks] メソッドです。 以下のスニペットでは、完了したタスクのみの ID、コマンド ライン、コンピューティング ノード情報のプロパティだけを返します。

```
// Configure an ODATADetailLevel specifying a subset of tasks and their properties to return
ODATADetailLevel detailLevel = new ODATADetailLevel();
detailLevel.FilterClause = "state eq 'completed'";
detailLevel.SelectClause = "id,commandLine,nodeInfo";

// Supply the ODATADetailLevel to the ListTasks method
IPagedEnumerable<CloudTask> completedTasks = batchClient.JobOperations.ListTasks("job-001", detailLevel);
```

上記の例のシナリオでジョブに何千ものタスクがある場合、通常は 2 つ目のクエリの結果は最初のものより速く返されます。 Batch .NET API による項目のリスト時の ODATADetailLevel の使用の詳細については、以下に示します。

> [AZURE.IMPORTANT]
> 強くお勧めする **常に** 効率性を高めると、アプリケーションのパフォーマンスを確保する、.NET API の一覧の呼び出しに、ODATADetailLevel を指定します。 詳細レベルの指定は、Batch サービスの応答時間の短縮、ネットワーク使用率の改善、およびクライアント アプリケーションによるメモリ使用量の最小化に役立ちます。

## 効率的なクエリのためのツール

 [バッチ .NET][api_net] と [バッチ REST][api_rest] Api で返される項目の両方の数を減らす機能を提供するリストだけでなく、情報の量を指定してごとに返される *フィルター*, 、*選択*, と *展開* 文字列を実行する場合は、クエリを一覧表示します。

- **フィルター** - *フィルター文字列* 返される項目数を削減する式を指定します。 たとえば、あるジョブの実行中のタスクのみ、またはタスクの実行準備が完了しているコンピューティング ノードのみをリストします。
  - filter 文字列は、プロパティ名、演算子、値で構成される、1 つ以上の式から成ります。 指定できるプロパティは、演算子が各プロパティでサポートされる場合、各 API 呼び出しの種類に固有です。
  - 論理演算子の `and` と `or` を使用して、複数の式を結合できます。
  - たとえば、実行中のレンダリング タスクのみをリストする場合、filter 文字列は `startswith(id, 'renderTask') and (state eq 'running')` となります。
- **選択** - *文字列選択* 項目ごとに返されるプロパティの値を制限します。 項目のプロパティ リストを select 文字列に指定でき、リスト クエリの結果では、各項目に対してこれらのプロパティの値のみが返されます。
  - select 文字列は、プロパティ名のコンマ区切りリストで構成されます。 リスト操作によって返される項目のすべてのプロパティを指定できます。
  - たとえば、各項目に対して 3 つのプロパティのみを返すように指定する場合、select 文字列は `id, state, stateTransitionTime` になります。
- **展開** - *文字列を展開し* を特定の情報を取得するために必要な API 呼び出しの数を削減します。 各リスト項目の詳細情報は、リストを取得してから、リスト内の各項目を呼び出すのではなく、1 つのリスト API 呼び出しで取得できます。
  - select 文字列と同様に、expand 文字列はリスト クエリ結果に特定のデータを含めるかどうかを制御します。
  - expand 文字列がサポートされるのは、ジョブ、ジョブのスケジュール、タスク、およびプールをリストする場合のみであり、現在サポートされているのは統計情報のみです。
  - たとえば、各項目に対して統計情報が返されるように指定する場合、expand 文字列は `stats` のようになります。
  - すべてのプロパティが必要な select 文字列が指定されていない場合、文字列を展開 *必要があります* 統計情報を取得するために使用します。 select 文字列を使用してプロパティのサブセットを取得する場合は、`stats` を select 文字列で指定でき、expand 文字列を指定する必要はありません。

> [AZURE.NOTE] (フィルター、[選択] を展開) の 3 つのクエリ文字列型のいずれかを構築するときに、プロパティの名前とケースと一致すること、REST API 要素の対応するを確認する必要があります。 たとえば、.NET を使用する場合 [CloudTask](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask), を指定する必要があります **状態** の代わりに **状態** 場合でも、.NET プロパティ [CloudTask.State](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.state)します。 .NET と REST API 間のプロパティ マッピングについては、以下の表を参照してください。

### filter、select、および expand 文字列の仕様

- フィルターで指定したプロパティを選択し、文字列の展開に示すようにプロパティ名の時と見なされる、 [バッチ REST][api_rest] API - これも大文字と小文字を使用する場合、 [バッチ .NET][api_net] ライブラリです。
- すべてのプロパティ名は大文字と小文字が区別されますが、プロパティの値は大文字と小文字を区別しません。
- 日付/時刻文字列は、次の 2 つの形式のいずれかで、`DateTime` の前に使用する必要があります。
  - W3CDTF 形式の例: `creationTime gt DateTime'2011-05-08T08:49:37Z'`
  - RFC1123 形式の例: `creationTime gt DateTime'Sun, 08 May 2011 08:49:37 GMT'`
- ブール文字列は、`true` または `false` のいずれかです。
- 無効なプロパティまたは演算子を指定すると、`400 (Bad Request)` エラーが表示されます。

## Batch .NET の効率的クエリ

内で、 [バッチ .NET][api_net] API、 [ODATADetailLevel][odata] はフィルターを提供するために使用されるを選択し、文字列のリスト操作を順に展開します。 ODataDetailLevel オブジェクトに、コンス トラクターで指定または直接設定できる 3 つのパブリック文字列プロパティがあり、このオブジェクトは、パラメーターとしてなどに渡さリスト操作のさまざまな [ListPools][net_list_pools], 、[ListJobs][net_list_jobs], 、および [ListTasks][net_list_tasks]します。

- [ODATADetailLevel.FilterClause][odata_filter] – 返される項目の数を制限します。
- [ODATADetailLevel.SelectClause][odata_select] – 各項目に返されるプロパティの値のサブセットを指定してください
- [ODATADetailLevel.ExpandClause][odata_expand] – ごとに呼び出しを発行するのではなく、単一の API で項目のデータを取得するを呼び出す

以下のコード スニペットでは Batch .NET API を使用して、特定のプール セットの統計値を Batch サービスに効率的に問い合わせます。 このシナリオでは、Batch ユーザーにはテスト プールと運用プールの両方が与えられています。テスト プールの ID 接頭辞は「test」であり、運用プールの ID 接頭辞は「prod」です。 スニペットでは、 *myBatchClient* のインスタンスが適切に初期化された [BatchClient](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient)します。

    // First we need an ODATADetailLevel instance on which to set the expand, filter, and select
    // clause strings
    ODATADetailLevel detailLevel = new ODATADetailLevel();

    // We want to pull only the "test" pools, so we limit the number of items returned by using a
    // FilterClause and specifying that the pool IDs must start with "test"
    detailLevel.FilterClause = "startswith(id, 'test')";

    // To further limit the data that crosses the wire, configure the SelectClause to limit the
    // properties returned on each CloudPool object to only CloudPool.Id and CloudPool.Statistics
    detailLevel.SelectClause = "id, stats";

    // Specify the ExpandClause so that the .NET API pulls the statistics for the CloudPools in a single
    // underlying REST API call. Note that we use the pool's REST API element name "stats" here as opposed
    // to "Statistics" as it appears in the .NET API (CloudPool.Statistics)
    detailLevel.ExpandClause = "stats";

    // Now get our collection of pools, minimizing the amount of data returned by specifying the
    // detail level we configured above
    List<CloudPool> testPools = await myBatchClient.PoolOperations.ListPools(detailLevel).ToListAsync();

> [AZURE.TIP] インスタンス [ODATADetailLevel][odata] での選択を構成し、Expand 句は、Get メソッドのなど、適切に渡すこともできます [PoolOperations.GetPool](https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.getpool.aspx) 返されるデータの量を制限します。

## Batch REST から NET API へのマッピング

フィルターのプロパティ名を選択し、文字列を展開し *必要があります* 、REST API の対応する名前とケースの両方を反映します。 次の表では、.NET および REST API の対応部分の間のマッピングを示します。

### filter 文字列のマッピング

- **.NET リスト メソッド** -このコラムで .NET API の方法を受け入れる、 [ODATADetailLevel][odata] オブジェクトをパラメーターとして。
- **残りの部分リストの要求** - このコラムで各 REST API のページには、プロパティを指定しで操作が許可 *フィルター* 文字列。 構築するときに、これらのプロパティ名と操作を使用するが、 [ODATADetailLevel.FilterClause][odata_filter] 文字列。

| .NET リスト メソッド | REST リスト要求 |
|---|---|
| [CertificateOperations.ListCertificates][net_list_certs] | [アカウントの証明書を一覧表示する][rest_list_certs]
| [CloudTask.ListNodeFiles][net_list_task_files] | [タスクに関連付けられているファイルを一覧表示する][rest_list_task_files]
| [JobOperations.ListJobPreparationAndReleaseTaskStatus][net_list_jobprep_status] | [ジョブのジョブ準備とジョブ リリース タスクの状態を一覧表示する][rest_list_jobprep_status]
| [JobOperations.ListJobs][net_list_jobs] | [アカウントのジョブを一覧表示する][rest_list_jobs]
| [JobOperations.ListNodeFiles][net_list_nodefiles] | [ノードのファイルを一覧表示する][rest_list_nodefiles]
| [JobOperations.ListTasks][net_list_tasks] | [ジョブに関連付けられているタスクを一覧表示する][rest_list_tasks]
| [JobScheduleOperations.ListJobSchedules][net_list_job_schedules] | [アカウントのジョブ スケジュールを一覧表示する][rest_list_job_schedules]
| [JobScheduleOperations.ListJobs][net_list_schedule_jobs] | [ジョブ スケジュールに関連付けられているジョブを一覧表示する][rest_list_schedule_jobs]
| [PoolOperations.ListComputeNodes][net_list_compute_nodes] | [プールのコンピューティング ノードを一覧表示する][rest_list_compute_nodes]
| [PoolOperations.ListPools][net_list_pools] | [アカウントのプールを一覧表示する][rest_list_pools]

### select 文字列のマッピング

- **バッチ .NET 型** -Batch .NET API の種類
- **REST API エンティティ** -このコラムでは、各ページには種類の REST API のプロパティ名を一覧表示する 1 つ以上のテーブルが含まれています。 構築するときにこれらのプロパティ名が使用される *選択* 文字列。 構築する場合、これらの同じプロパティ名を使用する [ODATADetailLevel.SelectClause][odata_select] 文字列。

| Batch .NET の種類 | REST API のエンティティ |
|---|---|
| [証明書][net_cert] | [証明書に関する情報を取得する][rest_get_cert] |
| [CloudJob][net_job] | [ジョブに関する情報を取得する][rest_get_job] |
| [CloudJobSchedule][net_schedule] | [ジョブ スケジュールに関する情報を取得する][rest_get_schedule] |
| [ComputeNode][net_node] | [ノードに関する情報を取得する][rest_get_node] |
| [CloudPool][net_pool] | [プールに関する情報を取得する][rest_get_pool] |
| [CloudTask][net_task] | [タスクに関する情報を取得する][rest_get_task] |

### 例: filter 文字列の構築

フィルター文字列を構築するときに、 [ODATADetailLevel.FilterClause][odata_filter], 、上記の下の表を参照してください *フィルター文字列のマッピング* を実行する] ボックスの一覧操作に対応する REST API ドキュメント ページを検索します。 そのページの最初の複数行の表に、フィルター可能なプロパティとサポートされている演算子が表示されます。 終了コードが 0 以外のすべてのタスクを取得する場合は、たとえば、この行に [ジョブに関連するタスクを一覧表示][rest_list_tasks] 許容の演算子と適用可能なプロパティの文字列を指定します。

| プロパティ | 許可される操作 | 型 |
| :--- | :--- | :--- |
| `executionInfo/exitCode` | `eq, ge, gt, le , lt` | `Int` |

したがって、終了コードがゼロ以外のタスクをすべて一覧表示するための filter 文字列は次のようになります。

`(executionInfo/exitCode lt 0) or (executionInfo/exitCode gt 0)`

### 例: select 文字列の構築

構築する、 [ODATADetailLevel.SelectClause][odata_select], 、上記の下の表を参照してください *マッピング select 文字列を* し、一覧表示するエンティティの種類に対応する REST API のページに移動します。 そのページの最初の複数行の表に、選択可能なプロパティとサポートされている演算子が表示されます。 ID およびリスト内の各タスクのコマンドラインを取得する場合は、たとえば、これらの行で見つかります適用可能なテーブルで [タスクに関する情報を取得][rest_get_task]:

| プロパティ | 型 | メモ |
| :--- | :--- | :--- |
| `id` | `String` | `The id of the task.` |
| `commandLine` | `String` | `The command line of the task.` |

リストされている各タスクの ID とコマンド ラインのみを含めるための select 文字列は次のようになります。

`id, commandLine`

## 次のステップ

チェック アウト、 [EfficientListQueries][efficient_query_sample] 効率性の一覧を照会して github サンプル プロジェクトは、アプリケーションのパフォーマンスに影響を与えます。 この c# コンソール アプリケーションを作成し、ジョブに大量のタスクを追加し、異なるバッチ サービスに対してクエリを [ODATADetailLevel][odata] 仕様では、次のような出力を表示します。

        Adding 5000 tasks to job jobEffQuery...
        5000 tasks added in 00:00:47.3467587, hit ENTER to query tasks...

        4943 tasks retrieved in 00:00:04.3408081 (ExpandClause:  | FilterClause: state eq 'active' | SelectClause: id,state)
        0 tasks retrieved in 00:00:00.2662920 (ExpandClause:  | FilterClause: state eq 'running' | SelectClause: id,state)
        59 tasks retrieved in 00:00:00.3337760 (ExpandClause:  | FilterClause: state eq 'completed' | SelectClause: id,state)
        5000 tasks retrieved in 00:00:04.1429881 (ExpandClause:  | FilterClause:  | SelectClause: id,state)
        5000 tasks retrieved in 00:00:15.1016127 (ExpandClause:  | FilterClause:  | SelectClause: id,state,environmentSettings)
        5000 tasks retrieved in 00:00:17.0548145 (ExpandClause: stats | FilterClause:  | SelectClause: )

        Sample complete, hit ENTER to continue...

経過時間情報に示すように、プロパティと返される項目の数を制限することで、クエリの応答時間を大幅に短縮できます。 これと他のサンプル プロジェクトで確認できます、 [azure batch サンプル][github_samples] GitHub のリポジトリです。

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[efficient_query_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/EfficientListQueries
[github_samples]: https://github.com/Azure/azure-batch-samples
[odata]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.aspx
[odata_ctor]: https://msdn.microsoft.com/library/azure/dn866178.aspx
[odata_expand]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.expandclause.aspx
[odata_filter]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.filterclause.aspx
[odata_select]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.odatadetaillevel.selectclause.aspx

[net_list_certs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.listcertificates.aspx
[net_list_compute_nodes]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listcomputenodes.aspx
[net_list_job_schedules]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobschedules.aspx
[net_list_jobprep_status]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobpreparationandreleasetaskstatus.aspx
[net_list_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[net_list_nodefiles]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listnodefiles.aspx
[net_list_pools]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listpools.aspx
[net_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobscheduleoperations.listjobs.aspx
[net_list_task_files]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.listnodefiles.aspx
[net_list_tasks]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listtasks.aspx

[rest_list_certs]: https://msdn.microsoft.com/library/azure/dn820154.aspx
[rest_list_compute_nodes]: https://msdn.microsoft.com/library/azure/dn820159.aspx
[rest_list_job_schedules]: https://msdn.microsoft.com/library/azure/mt282174.aspx
[rest_list_jobprep_status]: https://msdn.microsoft.com/library/azure/mt282170.aspx
[rest_list_jobs]: https://msdn.microsoft.com/library/azure/dn820117.aspx
[rest_list_nodefiles]: https://msdn.microsoft.com/library/azure/dn820151.aspx
[rest_list_pools]: https://msdn.microsoft.com/library/azure/dn820101.aspx
[rest_list_schedule_jobs]: https://msdn.microsoft.com/library/azure/mt282169.aspx
[rest_list_task_files]: https://msdn.microsoft.com/library/azure/dn820142.aspx
[rest_list_tasks]: https://msdn.microsoft.com/library/azure/dn820187.aspx

[rest_get_cert]: https://msdn.microsoft.com/library/azure/dn820176.aspx
[rest_get_job]: https://msdn.microsoft.com/library/azure/dn820106.aspx
[rest_get_node]: https://msdn.microsoft.com/library/azure/dn820168.aspx
[rest_get_pool]: https://msdn.microsoft.com/library/azure/dn820165.aspx
[rest_get_schedule]: https://msdn.microsoft.com/library/azure/mt282171.aspx
[rest_get_task]: https://msdn.microsoft.com/library/azure/dn820133.aspx

[net_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificate.aspx
[net_job]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.aspx
[net_pool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_schedule]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjobschedule.aspx
[net_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.aspx

