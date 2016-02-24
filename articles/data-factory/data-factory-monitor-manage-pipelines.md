<properties 
    pageTitle="Azure Data Factory のパイプラインの監視と管理" 
    description="Azure クラシック ポータルおよび Azure PowerShell を使用して、作成した Azure Data Factory とパイプラインを監視および管理する方法について説明します。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/20/2015" 
    ms.author="spelluru"/>

# Azure Data Factory のパイプラインの監視と管理
Data Factory サービスでは、データの保存、処理、移動の各サービスの全体像について、信頼性の高い情報が得られます。 データ パイプライン全体の正常性を迅速に評価し、問題を特定して、必要に応じて是正処置を取ることができます。 データの系列とデータ間のリレーションシップをソース全体にわたって視覚的に追跡し、ジョブの実行、システムの正常性、依存関係の履歴全体を 1 つの監視ダッシュボードから確認することもできます。

この記事では、パイプラインを監視、管理、およびデバッグする方法について説明します。 また、警告を作成して障害時に通知を受け取る方法についての情報も提供します。

## パイプラインとアクティビティの状態の理解
Azure ポータルを使用すると、Data Factory のダイアグラム、パイプラインのアクティビティ、入力と出力のデータセットなどを表示できます。 このセクションでは、スライスの状態がどのように移行するかについても説明します。   

### Data Factory に移動する
1.  サインインする、 [Azure ポータル](http://portal.azure.com)します。
2.  クリックして **すべてを参照** 選択 **データ ファクトリ**します。
    
    ![[すべて参照] -> [Data Factory]](./media/data-factory-monitor-manage-pipelines/browseall-data-factories.png)

    内のすべてのデータ ファクトリを表示する必要があります、 **データ ファクトリ** ブレードです。 
4. データ ファクトリ] ブレードで、興味のあるデータ ファクトリを選択して、ホーム ページを表示する必要があります (**Data factory** ブレード) のデータ ファクトリ。

    ![Data Factory ブレード](./media/data-factory-monitor-manage-pipelines/data-factory-blade.png)

#### Data Factory のダイアグラム ビュー
Data Factory のダイアグラム ビューでは、Data Factory とその資産を監視および管理する 1 つのウィンドウが提供されます。

クリックして **ダイアグラム** data factory のダイアグラム ビューを表示するに data factory のホーム ページにします。

![ダイアグラム ビュー](./media/data-factory-monitor-manage-pipelines/diagram-view.png)

拡大、縮小、ウィンドウのサイズに合わせて大きさを変更、100% に拡大、ダイアグラムのレイアウトのロック、パイプラインとテーブルの自動的な配置、系列情報の表示 (選択した項目の上位項目や下位項目の表示) などが可能です。
 

### パイプライン内のアクティビティ 
1. パイプラインを右クリックし、クリックして **パイプラインを開く** にアクティビティの入力呼び出し力データセットとパイプラインのすべての活動を参照してください。 これは、パイプラインが複数のアクティビティで構成されている場合に、1 つのパイプラインの動作系列を理解するときに便利です。

    ![パイプラインを開くメニュー](./media/data-factory-monitor-manage-pipelines/open-pipeline-menu.png)  
2. 次の例では、パイプラインの 2 つのアクティビティとその入力および出力がわかります。 「アクティビティ **JoinData** HDInsight Hive 型アクティビティのおよび **EgressDataAzure** コピー アクティビティは、このサンプル パイプラインには型のです。 
    
    ![パイプライン内のアクティビティ](./media/data-factory-monitor-manage-pipelines/activities-inside-pipeline.png) 
3. 左上隅の階層リンクの [Data Factory] リンクをクリックして、Data Factory のホーム ページに戻ることができます。

    ![Data Factory に戻る](./media/data-factory-monitor-manage-pipelines/navigate-back-to-data-factory.png)

### パイプライン内の各アクティビティの状態を表示する
アクティビティによって生成されるデータセットのステータスを表示することにより、アクティビティの現在の状態を確認できます。 

例: 次の例では、 **BlobPartitionHiveActivity** が正常に実行し、という名前のデータセットを生成 **PartitionedProductsUsageTable** が内にある **準備ができて** 状態です。

![パイプラインの状態](./media/data-factory-monitor-manage-pipelines/state-of-pipeline.png)

ダブルクリックすると、 **PartitionedProductsUsageTable** の図に表示が、パイプライン内の別のアクティビティの実行によって生成されたすべてのスライスを紹介します。 あることがわかります、 **BlobPartitionHiveActivity** 毎月正常に実行の最後の 8 か月間、スライスを生成 **準備ができて** 状態です。

Data Factory のデータセット スライスは、次のいずれかの状態になります。

<table>
<tr>
    <th align="left">状態</th><th align="left">下位状態</th><th align="left">説明</th>
</tr>
<tr>
    <td rowspan="8">待機中</td><td>ScheduleTime</td><td>スライスを実行する時刻になっていません。</td>
</tr>
<tr>
<td>DatasetDependencies</td><td>アップ ストリームの依存関係の準備ができていません。</td>
</tr>
<tr>
<td>ComputeResources</td><td>コンピューティング リソースが使用できません。</td>
</tr>
<tr>
<td>ConcurrencyLimit</td> <td>すべてのアクティビティ インスタンスが、他のスライスを実行しています。</td>
</tr>
<tr>
<td>ActivityResume</td><td>アクティビティは一時停止しており、再開されるまでスライスを実行することはできません。</td>
</tr>
<tr>
<td>Retry</td><td>アクティビティの実行が再試行されます。</td>
</tr>
<tr>
<td>検証</td><td>検証がまだ開始されていません。</td>
</tr>
<tr>
<td>ValidationRetry</td><td>検証の再試行を待機しています。</td>
</tr>
<tr>
<tr
<td rowspan="2">InProgress</td><td>Validating</td><td>検証を実行中です。</td>
</tr>
<td></td>
<td>スライスの処理中です。</td>
</tr>
<tr>
<td rowspan="4">Failed</td><td>TimedOut</td><td>アクティビティで許可されている実行時間を超過しました。</td>
</tr>
<tr>
<td>Canceled</td><td>ユーザーの操作によって取り消されました。</td>
</tr>
<tr>
<td>検証</td><td>検証が失敗しました。</td>
</tr>
<tr>
<td></td><td>スライスの生成、またはスライスの検証に失敗しました。</td>
</tr>
<td>Ready</td><td></td><td>スライスは使用可能な状態です。</td>
</tr>
<tr>
<td>Skipped</td><td></td><td>スライスは処理されていません。</td>
</tr>
<tr>
<td>なし</td><td></td><td>別のステータスで存在していたが、リセットされたスライスです。</td>
</tr>
</table>



スライスに関する詳細を表示するにはでスライスのエントリをクリックして、 **最近更新されたスライス** ブレードです。

![スライスの詳細](./media/data-factory-monitor-manage-pipelines/slice-details.png)
 
スライスが複数回実行されている場合に複数の行が表示されます、 **活動が実行される** ] ボックスの一覧です。

![スライスに対するアクティビティの実行](./media/data-factory-monitor-manage-pipelines/activity-runs-for-a-slice.png)

実行エントリをクリックして、実行、アクティビティに関する詳細を表示する、 **活動が実行される** ] ボックスの一覧です。 すべてのログ ファイルと、ある場合はエラー メッセージが表示されます。 これは、Data Factory を離れずにログを表示およびデバッグするのに非常に便利です。

![アクティビティ実行の詳細](./media/data-factory-monitor-manage-pipelines/activity-run-details.png)

スライスのではない場合、 **準備ができて** 状態、準備ができていないし、現在のスライスの実行をブロックするアップ ストリームのスライスを確認することができます、 **準備ができていないアップ ストリーム スライス** ] ボックスの一覧です。 これは非常に、スライスが **待機している** 状態したい場合、スライスが待機している上位依存関係を理解します。

![アップストリーム スライスの準備ができていない](./media/data-factory-monitor-manage-pipelines/upstream-slices-not-ready.png)

### データセット状態ダイアグラム
Data Factory をデプロイした後、パイプラインに有効なアクティブ期間があると、データセット スライスは 1 つの状態から別の状態に遷移します。 現在、スライスのステータスは、次の状態ダイアグラムに従います。

![状態ダイアグラム](./media/data-factory-monitor-manage-pipelines/state-diagram.png)

Data factory 内のデータセット状態遷移フローには、次の項目が含まれます。
待機中に進行状況/で-進行中 (検証)]-> [準備完了/失敗しました->

スライスの開始、 **待機している** 事前条件が実行する前に満たされる状態。 アクティビティが実行を開始し、スライスは、その後、 **In-progress** 状態です。 アクティビティの実行は成功または失敗し、それに基づいてスライスはいずれか **準備**' または **失敗** 状態です。 

ユーザーはからにスライスをリセットできます **準備** または **失敗** 状態から **待機している** 状態です。 ユーザーは、スライスの状態をマークできますも **Skip** は、アクティビティが実行されないようにして、スライスは処理されません。


## パイプラインを管理する
Azure PowerShell を使用してパイプラインを管理できます。 たとえば、Azure PowerShell コマンドレットを実行してパイプラインを一時停止および再開できます。 

### パイプラインを一時停止および再開する
一時停止/中断できますパイプラインを使用して、 **Suspend AzureRmDataFactoryPipeline** Powershell コマンドレット。 これは、データに問題があることがわかり、問題が解決されるまでパイプラインでそれ以上データを処理しないようにする場合に便利です。

例: で次のスクリーン ショット、問題が発見されました、 **PartitionProductsUsagePipeline** で **productrecgamalbox1dev** data factory パイプラインを中断します。

![パイプラインの中断](./media/data-factory-monitor-manage-pipelines/pipeline-to-be-suspended.png)

中断する、次の PowerShell コマンドを実行、 **PartitionProductsUsagePipeline**します。

    Suspend-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

次に例を示します。

    Suspend-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 

後で、問題を解決して、 **PartitionProductsUsagePipeline**, 、次の PowerShell コマンドを実行して中断されているパイプラインを再開することができます。

    Resume-AzureRmDataFactoryPipeline [-ResourceGroupName] <String> [-DataFactoryName] <String> [-Name] <String>

次に例を示します。

    Resume-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName productrecgamalbox1dev -Name PartitionProductsUsagePipeline 


## パイプラインをデバッグする
Azure Data Factory では、パイプラインをデバッグおよびトラブルシューティングするための充実した機能が Azure クラシック ポータルおよび Azure PowerShell で提供されています。

### パイプラインのエラーを発見する
パイプラインでアクティビティの実行が失敗した場合、パイプラインによって生成されるデータセットは障害のためにエラー状態になります。 次のメカニズムを使用して、Azure Data Factory のエラーをデバッグおよびトラブルシューティングできます。

#### Azure クラシック ポータルを使用してエラーをデバッグします。

1.  をクリックして **が、エラーが** に **データセット** data factory のホーム ページに並べて表示します。
    
    ![エラーのあるデータセット タイル](./media/data-factory-monitor-manage-pipelines/datasets-tile-with-errors.png)
2.   **エラーありデータセット** ブレードで、興味のあるテーブルをクリックします。

    ![[エラーありデータセット] ブレード](./media/data-factory-monitor-manage-pipelines/datasets-with-errors-blade.png)
3.   **テーブル** ブレードで、問題のあるスライスをクリックして **ステータス** に設定 **失敗**します。

    ![問題のあるスライスを表示している [テーブル] ブレード](./media/data-factory-monitor-manage-pipelines/table-blade-with-error.png)
4.   **データ スライス** ブレードで、失敗したアクティビティの実行] をクリックします。
    
    ![エラーのあるデータスライス](./media/data-factory-monitor-manage-pipelines/dataslice-with-error.png)
5.   **アクティビティの実行の詳細** ブレードで、HDInsight 処理に関連するファイルをダウンロードすることができます。 [状態/stderr] の [ダウンロード] をクリックし、エラーの詳細を含むエラー ログ ファイルをダウンロードします。

    ![エラーのあるアクティビティ実行詳細ブレード](./media/data-factory-monitor-manage-pipelines/activity-run-details-with-error.png)  

#### PowerShell を使用してエラーをデバッグする
1.  起動 **Azure PowerShell**します。
2.  切り替える **AzureResourceManager** モードを Data Factory コマンドレットはこのモードでのみ使用できます。

        switch-azuremode AzureResourceManager
3.  実行 **Get AzureRmDataFactorySlice** 、スライスとその状態を表示するコマンドです。 状態になっているスライスが表示されます: **失敗**します。

        Get-AzureRmDataFactorySlice [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    次に例を示します。


        Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Replace **StartDateTime** with the StartDateTime value you specified for the Set-AzureRmDataFactoryPipelineActivePeriod.
4. これで、実行、 **Get AzureRmDataFactoryRun** コマンドレットは、スライスのアクティビティの実行について詳細を取得します。

        Get-AzureRmDataFactoryRun [-ResourceGroupName] <String> [-
        DataFactoryName] <String> [-TableName] <String> [-StartDateTime] 
        <DateTime> [-Profile <AzureProfile> ] [ <CommonParameters>]
    
    次に例を示します。


        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    The value of StartDateTime is the start time for the error/problem slice you noted from the previous step. The date-time should be enclosed in double quotes.
5.  エラーの詳細を含む (以下のような) 出力結果が表示されます。

        Id                      : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
        ResourceGroupName       : ADF
        DataFactoryName         : LogProcessingFactory3
        TableName               : EnrichedGameEventsTable
        ProcessingStartTime     : 10/10/2014 3:04:52 AM
        ProcessingEndTime       : 10/10/2014 3:06:49 AM
        PercentComplete         : 0
        DataSliceStart          : 5/5/2014 12:00:00 AM
        DataSliceEnd            : 5/6/2014 12:00:00 AM
        Status                  : FailedExecution
        Timestamp               : 10/10/2014 3:04:52 AM
        RetryAttempt            : 0
        Properties              : {}
        ErrorMessage            : Pig script failed with exit code '5'. See wasb://     adfjobs@spestore.blob.core.windows.net/PigQuery
                                        Jobs/841b77c9-d56c-48d1-99a3-
                    8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for
                    more details.
        ActivityName            : PigEnrichLogs
        PipelineName            : EnrichGameLogsPipeline
        Type                    :
    
    
6.  実行する **保存 AzureRmDataFactoryLog** コマンドレットの Id 値は、上記の出力からわかり、ログをダウンロードするファイルを使用して、 **- DownloadLogsoption** コマンドレットのです。

    Save-AzureRmDataFactoryLog -ResourceGroupName "ADF" -DataFactoryName "LogProcessingFactory" -Id "841b77c9-d56c-48d1-99a3-8c16c3e77d39" -DownloadLogs -Output "C:\Test"


## パイプラインのエラーを再実行する

### Azure クラシック ポータルの使用

エラー スライスに移動し、クリックするとエラーを再実行のトラブルシューティングを行うし、パイプラインのエラーをデバッグすると、 **実行** コマンド バーのボタンをクリックします。

![障害が発生したスライスの再実行](./media/data-factory-monitor-manage-pipelines/rerun-slice.png)

スライスには、ポリシー エラーのための検証が失敗した場合に (の ex: データは使用できません)、エラーを修正しをクリックして再度検証、 **検証** コマンド バーのボタンをクリックします。
![エラーを修正し、検証](./media/data-factory-monitor-manage-pipelines/fix-error-and-validate.png)

### Azure PowerShell の使用

Set-AzureRmDataFactorySliceStatus コマンドレットを使用してエラーを再実行できます。

    Set-AzureRmDataFactorySliceStatus [-ResourceGroupName] <String> [-DataFactoryName] <String> [-TableName] <String> [-StartDateTime] <DateTime> [[-EndDateTime] <DateTime> ] [-Status] <String> [[-UpdateType] <String> ] [-Profile <AzureProfile> ] [ <CommonParameters>]

**例:**
次の例では、Azure data factory"WikiADF"DAWikiAggregatedData"PendingExecution"にテーブルのすべてのスライスの状態を設定します。

**注:** UpdateType は、テーブルの各スライスのすべての依存 (アップ ストリーム) テーブル、パイプライン内のアクティビティに対する入力テーブルとして使用されている状態が"PendingExecution"に設定されていることを意味 UpstreamInPipeline に設定します。 このパラメーターに指定できる他の値は、"Individual" です。

    Set-AzureRmDataFactorySliceStatus -ResourceGroupName ADF -DataFactoryName WikiADF -TableName DAWikiAggregatedData -Status PendingExecution -UpdateType UpstreamInPipeline -StartDateTime 2014-05-21T16:00:00 -EndDateTime 2014-05-21T20:00:00


## アラートを作成する
Azure では、Azure のリソース (データ ファクトリなど) の作成、更新、または削除時に、ユーザー イベントがログに記録されます。 これらのイベントでアラートを作成できます。 Data Factory では、さまざまなメトリックを収集し、メトリックに対してアラートを作成できます。 イベントはリアルタイムの監視に使用し、メトリックは履歴目的に使用することをお勧めします。 

### イベントでのアラート
Azure イベントは、Azure のリソースで何が起きているのかを把握するための便利な情報を提供します。 Azure では、Azure のリソース (データ ファクトリなど) の作成、更新、または削除時に、ユーザー イベントがログに記録されます。 Azure Data Factory の使用時には、次の場合にイベントが生成されます。

- Azure Data Factory が作成、更新、または削除された場合。
- データ処理 (実行と呼びます) が開始または完了した場合。
- オンデマンド HDInsight クラスターが作成および削除された場合。

これらのユーザー イベントに対してアラートを作成し、サブスクリプションの管理者と共同管理者に電子メール通知を送信するよう構成できます。 さらに、条件が満たされた場合に電子メール通知を受け取る必要があるユーザーの追加の電子メール アドレスを指定できます。 これは、Data Factory を常時監視するのではなく、障害が発生したら通知を受け取るようにする場合に非常に便利です。

#### アラートの定義の指定:
アラートの定義を指定するには、アラートの対象となる操作を記述する JSON ファイルを作成します。 以下の例では、アラートによって RunFinished 操作に関する電子メール通知が送信されます。 具体的には、データ ファクトリで実行が完了し、その実行が失敗していた場合 (Status = FailedExecution) に電子メール通知が送信されます。

    {
        "contentVersion": "1.0.0.0",
         "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters": {},
        "resources": 
        [
            {
                "name": "ADFAlertsSlice",
                "type": "microsoft.insights/alertrules",
                "apiVersion": "2014-04-01",
                "location": "East US",
                "properties": 
                {
                    "name": "ADFAlertsSlice",
                    "description": "One or more of the data slices for the Azure Data Factory has failed processing.",
                    "isEnabled": true,
                    "condition": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.ManagementEventRuleCondition",
                        "dataSource": 
                        {
                            "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleManagementEventDataSource",
                            "operationName": "RunFinished",
                            "status": "Failed",
                            "subStatus": "FailedExecution"   
                        }
                    },
                    "action": 
                    {
                        "odata.type": "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails": [ "<your alias>@contoso.com" ]
                    }
                }
            }
        ]
    }

上記の JSON 定義から **副状態** 特定のエラーに関するアラートを生成したくない場合は削除できます。

上記の例では、サブスクリプション内のすべてのデータ ファクトリのアラートを設定しています。 データ ファクトリを指定する、アラートが特定のデータ ファクトリに対して設定する場合は、 **resourceUri** で、 **データソース** 以下のようなブロックします。

    "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName>/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>"

次の表では、使用できる操作と状態 (および副状態) の一覧を示します。

操作の名前 | 状態 | 副状態
-------------- | ------ | ----------
RunStarted | 開始済み | Starting
RunFinished | Failed / Succeeded | <p>FailedResourceAllocation</p><p>Succeeded</p><p>FailedExecution</p><p>TimedOut</p><p>< キャンセル/p ><p>FailedValidation</p><p>Abandoned</p>
OnDemandClusterCreateStarted | 開始済み
OnDemandClusterCreateSuccessful | Succeeded
OnDemandClusterDeleted | Succeeded

参照してください [アラート ルールの作成](https://msdn.microsoft.com/library/azure/dn510366.aspx) 上の例で使用される JSON 要素の詳細。 

#### アラートのデプロイ 
アラートをデプロイするには、Azure PowerShell コマンドレットを使用: **New-azureresourcegroupdeployment**, の次の例に示すようにします。

    New-AzureResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\ADFAlertFailedSlice.json  

リソース グループのデプロイメントが正常に終了すると、次のメッセージが表示されます。

    VERBOSE: 7:00:48 PM - Template is valid.
    WARNING: 7:00:48 PM - The StorageAccountName parameter is no longer used and will be removed in a future release.
    Please update scripts to remove this parameter.
    VERBOSE: 7:00:49 PM - Create template deployment 'ADFAlertFailedSlice'.
    VERBOSE: 7:00:57 PM - Resource microsoft.insights/alertrules 'ADFAlertsSlice' provisioning status is succeeded
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :

#### Azure リソース グループのデプロイメント一覧の取得
展開済みの Azure リソース グループのデプロイの一覧を取得するには、コマンドレットを使用して: **Get-azureresourcegroupdeployment**, の次の例に示すようにします。

    Get-AzureResourceGroupDeployment -ResourceGroupName adf
    
    DeploymentName    : ADFAlertFailedSlice
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 10/11/2014 2:01:00 AM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           :


#### ユーザー イベントのトラブルシューティング


- クリックした後で生成されたすべてのイベントを表示、 **操作** タイル、およびアラートに表示されているこれらの操作のいずれかでセットアップを指定できます、 **イベント** ブレード。

    ![操作](./media/data-factory-monitor-manage-pipelines/operations.png)


- 参照してください [Azure Insight コマンドレット](https://msdn.microsoft.com/library/mt282452.aspx) アラートの追加と取得/削除に使用できる PowerShell コマンドレットについては、資料です。 使用する例をいくつか次のとおり、 **Get AlertRule** コマンドレット。 


        PS C:\> get-alertrule -res $resourceGroup -n ADFAlertsSlice -det
            
                Properties :
                Action      : Microsoft.Azure.Management.Insights.Models.RuleEmailAction
                Condition   :
                DataSource :
                EventName             :
                Category              :
                Level                 :
                OperationName         : RunFinished
                ResourceGroupName     :
                ResourceProviderName  :
                ResourceId            :
                Status                : Failed
                SubStatus             : FailedExecution
                Claims                : Microsoft.Azure.Management.Insights.Models.RuleManagementEventClaimsDataSource
                Condition   :
                Description : One or more of the data slices for the Azure Data Factory has failed processing.
                Status      : Enabled
                Name:       : ADFAlertsSlice
                Tags       :
                $type          : Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage
                Id: /subscriptions/<subscription ID>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/ADFAlertsSlice
                Location   : West US
                Name       : ADFAlertsSlice
        
        PS C:\> Get-AlertRule -res $resourceGroup
    
                Properties : Microsoft.Azure.Management.Insights.Models.Rule
                Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
                Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
                Location   : West US
                Name       : FailedExecutionRunsWest0
        
                Properties : Microsoft.Azure.Management.Insights.Models.Rule
                Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
                Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest3
                Location   : West US
                Name       : FailedExecutionRunsWest3
    
        PS C:\> Get-AlertRule -res $resourceGroup -Name FailedExecutionRunsWest0
        
                Properties : Microsoft.Azure.Management.Insights.Models.Rule
                Tags       : {[$type, Microsoft.WindowsAzure.Management.Common.Storage.CasePreservedDictionary, Microsoft.WindowsAzure.Management.Common.Storage]}
                Id         : /subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/microsoft.insights/alertrules/FailedExecutionRunsWest0
                Location   : West US
                Name       : FailedExecutionRunsWest0

    Run the following get-help commands to see details and examples for the Get-AlertRule cmdlet. 

        get-help Get-AlertRule -detailed 
        get-help Get-AlertRule -examples


- ポータルのブレードにアラート生成イベントが表示されるのに、電子メール通知を受け取らない場合は、指定されている電子メール アドレスが外部の送信者からの電子メールを受信するように設定されているかどうかを確認します。 アラートの電子メールが、電子メールの設定によってブロックされている可能性があります。

### メトリックでのアラート
Data Factory では、さまざまなメトリックを収集し、メトリックに対してアラートを作成できます。 Data Factory のスライスに対して次のメトリックのアラートを監視および作成できます。
 
- 失敗した実行
- 成功した実行

これらのメトリックは非常に便利であり、ユーザーは Data Factory での全体的な失敗および成功した実行の概要を取得できます。 メトリックは、スライスが実行するたびに生成されます。 0 時になると、メトリックは集計されて、ストレージ アカウントにプッシュされます。 したがって、メトリックを有効にするには、ストレージ アカウントを設定する必要があります。

#### メトリックの有効化:
メトリックを有効にするには、Data Factory ブレードから次のようにクリックします。

**監視** ]-> [ **メトリック** ]-> [ **診断設定** ]-> [ **診断**

 **診断** ブレードで、をクリックして **に** ストレージ アカウントを選択し、保存します。

![メトリックの有効化](./media/data-factory-monitor-manage-pipelines/enable-metrics.png)

メトリックの集計は 1 時間ごとに行われるので、保存した後、メトリックが監視ブレードに表示されるまでに最大で 1 時間かかることがあります。


### メトリックに対するアラートの設定:

メトリックにアラートを設定するには、データ ファクトリ] ブレードから次のようをクリックします。
**監視** ]-> [ **メトリック** ]-> [ **アラートの追加** ]-> [ **アラート ルールの追加**します。

アラート ルールの詳細を入力し、電子メールを指定して、クリックして **OK**します。


![メトリックに対するアラートの設定](./media/data-factory-monitor-manage-pipelines/setting-up-alerts-on-metrics.png)

終了すると、次のように [アラート ルール] タイルに有効になった新しいアラート ルールが表示されます。

![有効なアラート ルール](./media/data-factory-monitor-manage-pipelines/alert-rule-enabled.png)

ご利用ありがとうございます。 メトリックに最初のアラートが設定されました。 特定の期間にアラート ルールが一致するたびに通知を受け取るようになります。

### アラート通知:
設定ルールが条件に一致すると、アラートでアクティブ化された電子メールを受け取ります。 問題が解決されてアラート条件が一致しなくなると、アラート解決電子メールを受け取ります。

この動作は、アラート ルールが適合するすべてのエラーで通知が送信されるイベントとは異なります。

### PowerShell を使用したアラートのデプロイ
イベントの場合と同じ方法で、メトリックのアラートをデプロイできます。 

**アラートの定義:**

    {
        "contentVersion" : "1.0.0.0",
        "$schema" : "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
        "parameters" : {},
        "resources" : [
        {
                "name" : "FailedRunsGreaterThan5",
                "type" : "microsoft.insights/alertrules",
                "apiVersion" : "2014-04-01",
                "location" : "East US",
                "properties" : {
                    "name" : "FailedRunsGreaterThan5",
                    "description" : "Failed Runs greater than 5",
                    "isEnabled" : true,
                    "condition" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.ThresholdRuleCondition, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.ThresholdRuleCondition",
                        "dataSource" : {
                            "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleMetricDataSource, Microsoft.WindowsAzure.Management.Mon.Client",
                            "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleMetricDataSource",
                            "resourceUri" : "/SUBSCRIPTIONS/<subscriptionId>/RESOURCEGROUPS/<resourceGroupName
    >/PROVIDERS/MICROSOFT.DATAFACTORY/DATAFACTORIES/<dataFactoryName>",
                            "metricName" : "FailedRuns"
                        },
                        "threshold" : 5.0,
                        "windowSize" : "PT3H",
                        "timeAggregation" : "Total"
                    },
                    "action" : {
                        "$type" : "Microsoft.WindowsAzure.Management.Monitoring.Alerts.Models.RuleEmailAction, Microsoft.WindowsAzure.Management.Mon.Client",
                        "odata.type" : "Microsoft.Azure.Management.Insights.Models.RuleEmailAction",
                        "customEmails" : ["abhinav.gpt@live.com"]
                    }
                }
            }
        ]
    }
 
上のサンプルの subscriptionId、resourceGroupName、dataFactoryName を、適切な値に置き換えます。

*metricName* 現時点では、2 つの値をサポートしています。
- FailedRuns
- SuccessfulRuns

**アラートのデプロイ:**

アラートをデプロイするには、Azure PowerShell コマンドレットを使用: **New-azureresourcegroupdeployment**, の次の例に示すようにします。

    New-AzureResourceGroupDeployment -ResourceGroupName adf -TemplateFile .\FailedRunsGreaterThan5.json

デプロイが成功すると、次のメッセージが表示されます。

    VERBOSE: 12:52:47 PM - Template is valid.
    VERBOSE: 12:52:48 PM - Create template deployment 'FailedRunsGreaterThan5'.
    VERBOSE: 12:52:55 PM - Resource microsoft.insights/alertrules 'FailedRunsGreaterThan5' provisioning status is succeeded
    
    
    DeploymentName    : FailedRunsGreaterThan5
    ResourceGroupName : adf
    ProvisioningState : Succeeded
    Timestamp         : 7/27/2015 7:52:56 PM
    Mode              : Incremental
    TemplateLink      :
    Parameters        :
    Outputs           



