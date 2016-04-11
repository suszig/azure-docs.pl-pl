<properties
    pageTitle="Batch でのジョブの準備とクリーンアップ | Microsoft Azure"
    description="Azure Batch コンピューティング ノード間のデータ転送を最小にするにはジョブ準備タスクを使用し、ジョブの完了時にノードをクリーンアップするにはジョブ解放タスクを使用します。"
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
    ms.date="10/15/2015"
    ms.author="v-marsma"/>

# Azure Batch コンピューティング ノードでのジョブ準備タスクとジョブ完了タスクの実行

Azure Batch ジョブでは、実行前の何らかのセットアップ、およびジョブ タスク完了後の何らかのジョブ後保守を行う必要があることがよくあります。 Batch には、この準備とオプションの形式でのメンテナンスのためのメカニズムが用意されています *ジョブの準備* と *リリースをジョブ* タスクです。

ジョブ準備タスクは、タスクの実行がスケジュールされているすべての計算ノードにおいて、ジョブのタスク実行前に実行されます。 ジョブが完了すると、少なくとも 1 つのタスクを実行したプールの各ノードでジョブ解放タスクが実行されます。 ジョブ準備タスクおよびジョブ解放タスクのどちらでも、タスクをいつ呼び出すかをコマンド ラインで指定し、ファイルのダウンロード、管理者特権での実行、カスタム環境変数、最大実行期間、再試行回数、ファイル保持時間などの機能を提供できます。

次のセクションで紹介を使用してこれらの 2 つの特殊なタスク型を使用する方法、 [JobPreparationTask][net_job_prep] と [JobReleaseTask][net_job_release] で、 [バッチ .NET][api_net] API です。

> [AZURE.TIP] ジョブの準備とリリースのタスクは、「共有プール」環境を計算ノードのプールがジョブの実行の間で永続化し、さまざまなジョブの数の間で共有されて、これらの環境で特に役立ちます。

## ジョブ準備タスクおよびジョブ解放タスクを使用するのに適した状況

ジョブ準備タスクおよびジョブ解放タスクは、さまざまな状況でメリットがあります。 次に、いくつか示します。

- **一般的なタスク データの転送** -バッチ ジョブでは、ジョブのタスクの入力としてデータの共通セットが必要と多くの場合。 たとえば、毎日のリスク分析の計算では、市場データはジョブ固有ですが、ジョブのすべてのタスクに共通です。 このような市場データはサイズが数ギガバイトになることも多く、各計算ノードに 1 回だけダウンロードして、そのノードで実行する各タスクがそれを使用できるようにする必要があります。 使用して、 *ジョブ準備タスク* に他のタスクの実行前に各ノードにデータをダウンロードします。
- **ジョブのデータの削除** - でプールの計算ノードがノード上のディスク領域を節約するために、組織のセキュリティ ポリシーを満たしているか、実行の間でデータを必要とする可能性のジョブを削除するジョブの間で使用停止されたにない共有プール環境内でします。 使用して、 *ジョブのリリース タスク* ジョブ準備タスクによってダウンロードされたか、タスクの実行中に生成されたデータを削除します。
- **ログ保存** で、タスクによって生成されるログ ファイルのコピーを保持するか、クラッシュ ダンプ ファイルが失敗したアプリケーションによって生成される可能性があります。 使用して、 *ジョブのリリース タスク* を圧縮し、このデータをアップロードするには、このような場合、 [Azure Storage][azure_storage] アカウントです。

## ジョブ準備タスク

ジョブ準備タスクは、ジョブのタスクを実行する前に、タスクの実行がスケジュールされている各計算ノードで実行されます。 既定では、Batch サービスはジョブ準備タスクが完了するまで待ってからスケジュールされているタスクをノードで実行しますが、完了を待たないようにサービスを構成できます。 計算ノードが再起動するとジョブ準備タスクも再度実行されますが、この動作もやはり無効にできます。

ジョブ準備タスクは、タスクを実行するようにスケジュールされているノードでのみ実行されます。 これにより、タスクを割り当てられていないノードで必要のない準備タスクが実行されなくなり、たとえばデータ転送コストを節約できます。 ジョブのタスクの数が、プール内のノード数よりも少ない場合や、このような状況が適用される [同時実行タスクの実行](batch-parallel-node-tasks.md) が有効になってタスク数が集計可能な同時実行タスクを下回る場合、それらのいくつかのノードのアイドル状態を終了します。

> [AZURE.NOTE]  [JobPreparationTask][net_job_prep_cloudjob] と異なる、 [CloudPool.StartTask][pool_starttask] 、JobPreparationTask はコンピューティング ノードときにのみ、StartTask が実行されますが、各ジョブの開始時に実行する点で最初に、プールに参加または再起動します。

## ジョブ解放タスク

ジョブが完了すると、ジョブのリリース タスクは、少なくとも 1 つのタスクを実行するプール内の各ノードで実行されます。 ジョブを完了済みとして指定するには、終了要求を発行します。 Batch サービス ジョブの状態を設定し、 *終了*, 、ジョブに関連付けられている作業中または実行中のタスクを終了し、ジョブのリリースのタスクを実行します。 ジョブに移動、 *完了* 状態です。

> [AZURE.NOTE] ジョブが削除では、ジョブのリリース タスクも実行します。 ただし、先にジョブを終了した場合は、その後でジョブを削除しても解放タスクが再度実行されることはありません。

## バッチ .NET におけるジョブの準備とリリースのタスク

作成および構成してジョブ準備タスクを指定することは、 [JobPreparationTask][net_job_prep] し、ジョブに割り当てる [CloudJob.JobPreparationTask][net_job_prep_cloudjob] プロパティです。 同様に、初期化、 [JobReleaseTask][net_job_release] し、ジョブに割り当てる [CloudJob.JobReleaseTask][net_job_prep_cloudjob] プロパティをジョブのリリースのタスクを設定します。

このコード スニペットで `myBatchClient` のインスタンスが完全に初期化された [BatchClient][net_batch_client], と `myPool` は Batch アカウント内の既存のプールです。

        // Create the CloudJob for CloudPool "myPool"
        CloudJob myJob = myBatchClient.JobOperations.CreateJob("JobPrepReleaseSampleJob",
                                                               new PoolInformation() { PoolId = "myPool" });

        // Specify the command lines for the job preparation and release tasks
        string jobPrepCmdLine = "cmd /c echo %AZ_BATCH_NODE_ID% > %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";
        string jobReleaseCmdLine = "cmd /c del %AZ_BATCH_NODE_SHARED_DIR%\\shared_file.txt";

        // Assign the job preparation task to the job
        myJob.JobPreparationTask = new JobPreparationTask { CommandLine = jobPrepCmdLine };

        // Assign the job release task to the job
        myJob.JobReleaseTask = new JobPreparationTask { CommandLine = jobReleaseCmdLine };

        await myJob.CommitAsync();

前述のように、解放タスクはジョブが終了または削除されたときに実行されます。 呼び出して実行バッチ .NET API でジョブを終了して [PoolOperations.TerminateJobAsync][net_job_terminate], でジョブの削除が行われると [PoolOperations.DeleteJobAsync][net_job_delete], 、ジョブのタスクが完了または定義したタイムアウトに達したときにどちらも通常行います。

        // Terminate the job to mark it as Completed; this will initiate the Job Release Task on any node
        // that executed job tasks. Note that the Job Release Task is also executed when a job is deleted,
        // thus you need not call Terminate if you typically delete your jobs upon task completion.
        await myBatchClient.JobOperations.TerminateJobAsync("JobPrepReleaseSampleJob");

## 次のステップ

### GitHub のサンプル プロジェクト

チェック アウト、 [JobPrepRelease][job_prep_release_sample] アクション内のジョブの準備およびリリースを確認する github サンプル プロジェクトのタスクです。 このコンソール アプリケーションは次のことを行います。

1. 2 つの "小さい" ノードを含むプールを作成します
2. ジョブ準備、解放、および標準タスクを使用するジョブを作成します。
3. ノード ID をノードの「共有」ディレクトリ内のテキスト ファイルに書き込むジョブ準備タスクを実行します
4. 同じテキスト ファイルにタスク ID を書き込むタスクを各ノードで実行します
5. すべてのタスクが完了したら (またはタイムアウトに達したら)、各ノードのテキスト ファイルの内容をコンソールに出力します。
6. ジョブが完了したら、ジョブ解放タスクを実行してノードからファイルを削除します
6. 実行した各ノードのジョブ準備タスクとジョブ解放タスクの終了コードを出力します
7. ジョブまたはプールの削除を確認できるように実行を一時停止します

サンプル アプリケーションからの出力は次のようになります。

```
Attempting to create pool: JobPrepReleaseSamplePool
The pool already existed when we tried to create it
Checking for existing job JobPrepReleaseSampleJob...
Job JobPrepReleaseSampleJob not found, creating...
Submitting tasks and awaiting completion...
All tasks completed.

Contents of shared\job_prep_and_release.txt on tvm-3105992504_1-20151015t150030z:
-------------------------------------------
tvm-3105992504_1-20151015t150030z tasks:
  task001
  task002
  task006
  task007

Contents of shared\job_prep_and_release.txt on tvm-3105992504_2-20151015t150030z:
-------------------------------------------
tvm-3105992504_2-20151015t150030z tasks:
  task003
  task005
  task004
  task008

Waiting for job JobPrepReleaseSampleJob to reach state Completed
....

tvm-3105992504_1-20151015t150030z:
  Prep task exit code:    0
  Release task exit code: 0

tvm-3105992504_2-20151015t150030z:
  Prep task exit code:    0
  Release task exit code: 0

Delete job? [yes] no
yes
Delete pool? [yes] no
no

Sample complete, hit ENTER to exit...
```

### Batch Explorer でのジョブ準備タスクとジョブ解放タスクの確認

 [Batch Explorer][batch_explorer_article], 、バッチでも見つかった [サンプル コード リポジトリ][batch_explorer_project] GitHub では、ときに使用する優れたツール ソリューションの開発を Azure Batch とします。 たとえば、上のサンプル アプリケーションを実行するときに Batch Explorer を使用して、ジョブとそのタスクのプロパティを表示したり、ジョブのタスクによって変更された共有テキスト ファイルをダウンロードしたりできます。

次のスクリーン ショットは、ジョブの詳細ウィンドウに表示されるジョブの準備とリリース タスク プロパティを強調表示と、 *JobPrepReleaseSampleJob* [ジョブ] タブでジョブを選択します。

![Batch Explorer][1]

*ジョブ準備タスクとジョブ解放タスクが表示されている Batch Explorer のスクリーンショット*

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_listjobs]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.listjobs.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_explorer_article]: http://blogs.technet.com/b/windowshpc/archive/2015/01/20/azure-batch-explorer-sample-walkthrough.aspx
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[job_prep_release_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/JobPrepRelease
[net_batch_client]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.batchclient.aspx
[net_cloudjob]:https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.aspx
[net_job_prep]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobpreparationtask.aspx
[net_job_prep_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobpreparationtask.aspx
[net_job_delete]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.deletejobasync.aspx
[net_job_terminate]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.joboperations.terminatejobasync.aspx
[net_job_release]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.jobreleasetask.aspx
[net_job_release_cloudjob]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobreleasetask.aspx
[pool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx

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

[1]: ./media/batch-job-prep-release/batchexplorer-01.png
[2]: ./media/batch-job-prep-release/batchexplorer-02.png

