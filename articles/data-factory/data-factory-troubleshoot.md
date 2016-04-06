<properties 
    pageTitle="Azure Data Factory のトラブルシューティング" 
    description="Azure データ ファクトリの使用に関する問題のトラブルシューティングを行う方法について説明します。" 
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
    ms.date="11/12/2015" 
    ms.author="spelluru"/>

# Data Factory のトラブルシューティング
Azure クラシック ポータルまたは Azure PowerShell コマンドレットを使用して、Azure Data Factory の問題のトラブルシューティングを行うことができます。 このトピックには、Azure クラシック ポータルを使用して、Data Factory で発生するエラーをすばやく解決する方法を示すチュートリアルが含まれています。 

## 問題: Data Factory コマンドレットを実行できない
バージョン 1.0 より前のバージョンの Azure PowerShell を使用する場合
 
この問題を解決するのには Azure モードを切り替える **AzureResourceManager**: 

起動 **Azure PowerShell** に切り替えるには、次のコマンドを実行し、 **AzureResourceManager** モードです。Azure Data Factory コマンドレットに、 **AzureResourceManager** モードです。

         switch-azuremode AzureResourceManager

## 問題: Data Factory コマンドレットを実行する際の認証エラー
Azure PowerShell で使用する Azure アカウントまたはサブスクリプションが正しくない可能性があります。 次のコマンドレットを使用して、Azure PowerShell で使用する適切な Azure アカウントとサブスクリプションを選択してください。 

1. Add-AzureAccount - 適切なユーザー ID とパスワードを使用します。
2. Get-AzureSubscription - アカウントのサブスクリプションをすべて表示します。 
3. Select-AzureSubscription <subscription name> -適切なサブスクリプションを選択します。 Azure ポータルでデータ ファクトリの作成に使用するのと同じものを使用します。

## 問題: Azure クラシック ポータルからデータ ゲートウェイの高速セットアップを起動できない
Data Gateway の高速セットアップを起動するには、Internet Explorer または Microsoft ClickOnce と互換性のある Web ブラウザーが必要です。 高速セットアップを開始できない場合、次の手順を実行できます。

1. 他のブラウザーで失敗した場合は、Internet Explorer に切り替えます。 または
2. ポータルの同じブレードに表示される [手動セットアップ] リンクを使用してインストールを実行し、Data Management Gateway 構成の準備ができたら、画面に表示されているキーをコピーしておき、Data Management Gateway 構成の準備ができたときに貼り付けます。 起動しない場合は、"Microsoft Data Management Gateway" の [スタート] メニューを確認し、起動した時点でキーを貼り付けます。 


## 問題: Azure クラシック ポータルから資格情報マネージャーを起動できない
Azure クラシック ポータルから SQL Server のリンクされたサービスをセットアップまたは更新する場合、セキュリティを確保するために、資格情報マネージャー アプリケーションを起動します。 この場合、Internet Explorer か、Microsoft の ClickOnce と互換性のある Web ブラウザーが必要です。 他のブラウザーで失敗した場合は、Internet Explorer に切り替えることができます。

## 問題: オンプレミス SQL Server に接続できない 
ゲートウェイがインストールされているコンピューターから SQL Server に接続できることを確認します。 ゲートウェイがインストールされているコンピューターで、次のことを実行できます。

1. SQL Server がインストールされているコンピューターに Ping を送信します。 または
2. SQL Server Management Studio (SSMS) を使用して、Azure クラシック ポータルで指定した資格情報で SQL Server インスタンスに接続してみます。


## 問題: 入力スライスが PendingExecution または PendingValidation 状態のままになっている

スライスがある可能性もあります **PendingExecution** または **PendingValidation** によりさまざまな理由と一般的な理由の 1 つの状態、 **外部** にプロパティが設定されていない **true**します。 Azure Data Factory のスコープ外で生成される任意のデータセットを指定してください **外部** プロパティです。 これは、データが外部データであり、データ ファクトリ内のパイプラインでサポートされていないことを示します。 データ スライスが付いている **準備** それぞれのストアでデータが使用可能です。 

使用状況は、次の例を参照してください、 **外部** プロパティです。 必要に応じて指定することができます **externalData*** external を true に設定すると. 

テーブルのトピックを参照して [JSON スクリプティング リファレンス][json-scripting-reference] このプロパティの詳細についてです。
    
    {
      "name": "CustomerTable",
      "properties": {
        "type": "AzureBlob",
        "linkedServiceName": "MyLinkedService",
        "typeProperties": {
          "folderPath": "MyContainer/MySubFolder/",
          "format": {
            "type": "TextFormat",
            "columnDelimiter": ",",
            "rowDelimiter": ";"
          }
        },
        "external": true,
        "availability": {
          "frequency": "Hour",
          "interval": 1
        },
        "policy": {
          "externalData": {
            "dataDelay": "00:10:00",
            "retryInterval": "00:01:00",
            "retryTimeout": "00:10:00",
            "maximumRetry": 3
          }
        }
      }
    }

 エラーを解決するには、追加、 **外部** プロパティと、省略可能な **externalData** セクションに、入力テーブルの JSON 定義とテーブルを再作成します。 

## 問題: ハイブリッド コピー操作に失敗する
詳細情報:

1. ゲートウェイがインストールされているコンピューターで Data Management Gateway Configuration Manager を起動します。 いることを確認、 **ゲートウェイ名** に論理ゲートウェイ名に設定されている、 **Azure Classic Portal**, 、**ゲートウェイ キーの状態** は **登録** と **サービスのステータスを** は **開始**します。 
2. 起動 **イベント ビューアー**します。 展開 **アプリケーションとサービス ログ** ] をクリック **Data Management Gateway**します。 Data Management Gateway に関連するエラーがあるか確認します。 

## 問題: オンデマンドの HDInsight プロビジョニングがエラーで失敗する

種類が HDInsightOnDemandLinkedService のリンクされたサービスを使用する場合は、Azure Blob ストレージを指す linkedServiceName を指定してください。 このストレージ アカウントを使用して、オンデマンド HDInsight クラスターのすべてのログとサポート ファイルをコピーします。  HDInsight でオンデマンド プロビジョニングを実行するアクティビティは、次のエラーで失敗することがあります。

        Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.

通常このエラーは、linkedServiceName で指定されたストレージ アカウントの場所が、HDInsight のプロビジョニングが行われているのと同じデータ センターの場所でないことを示します。 などの場合は、Azure Data Factory の場所が米国西部、およびオンデマンドの HDInsight のプロビジョニングで行われますが米国西部、Azure blob ストレージ アカウントの場所は、米国東部に設定されて、オンデマンド プロビジョニングは失敗します。

さらに、2 つ目の JSON プロパティ additionalLinkedServiceNames では、オンデマンド HDInsight に追加のストレージ アカウントを指定できます。 これらの追加のリンクされたストレージ アカウントは HDInsight クラスターと同じ場所に存在する必要があります。そうでないと、同じエラーで失敗します。



## 問題: カスタム アクティビティが失敗する
Azure Data Factory でカスタム アクティビティ (パイプライン アクティビティの種類が CustomActivity) を使用する場合、カスタム アプリケーションは、所定のリンクされたサービスから HDInsight に対して、マップのみのストリーミング MapReduce ジョブとして実行されます。 

Azure Data Factory が HDInsight クラスターからその出力をキャプチャして保存できるようにするカスタム アクティビティを実行すると、 *adfjobs* Azure Blob ストレージ アカウント内のストレージ コンテナー。 エラー発生時のテキストを読み取ることができます **stderr** 、エラーが発生した後に、テキスト ファイルを出力します。 このファイルは、Web ブラウザーで開いた Azure クラシック ポータルからアクセスして読み取ることができます。または、ストレージ エクスプローラー ツールを使用して、Azure Blob Storage のストレージ コンテナーに保存されているファイルに直接アクセスできます。 

特定のカスタム アクティビティのログを列挙して読み取るには、このページの後半で紹介するチュートリアルのいずれかの手順に従います。 概要:

1.  Azure クラシック ポータルで **参照** を Data Factory を検索します。
2.  使用して、 **ダイアグラム** をデータ ファクトリのダイアグラムを表示するボタンをクリックし、をクリックして、 **データセット** 、固有の仕様を次の表を **パイプライン** カスタム アクティビティを持つします。 
3.   **テーブル** ブレードで、関心のあるスライスをクリックして、 **問題のあるスライス** 調査する時間枠にします。
4.  詳細な **データ スライス** ブレードが表示されが一覧 **活動が実行される** のあるスライスです。 をクリックして、 **アクティビティ** リストからです。 
5.   **アクティビティの実行の詳細** ブレードが表示されます。 一覧表示、 **エラー メッセージ** いくつかのブレードで、中央 **ログ ファイル** そのアクティビティの実行と連携して、ブレードの下部に一覧表示します。
    - Logs/system-0.log
    - 状態
    - Status/exit
    - Status/stderr
    - Status/stdout

6. 最初のクリックして **ログ ファイル** 一覧と、ログ内の項目が新しいブレード フルテキストに読みやすい表示で開きます。 各ログをクリックして、そのログのテキストを確認します。 テキスト ビューアー ブレードが開きます。 クリックして、 **ダウンロード** オフラインで表示のテキスト ファイルのダウンロード] ボタンをクリックします。  

1 つ **の一般的なエラー** カスタム アクティビティからは、 
        パッケージの実行が終了コード '1' で失敗しました。 参照してください"wasb://adfjobs@storageaccount.blob.core.windows.net/PackageJobs/<guid>/<jobid>/Status/stderr ' 詳細についてです。

この種のエラーの詳細を表示する、 **stderr** ファイルです。 表示される 1 つの一般的なエラーは、次のようにタイムアウト状態を示します。
        Mapreduce の情報です。ジョブ: タスク Id: attempt_1424212573646_0168_m_000000_0, Status: に失敗しました 
        AttemptID:attempt_1424212573646_0168_m_000000_0 が 600 秒経過後にタイムアウト

たとえば、30 分間以上にわたり、このジョブを 3 回再試行すると、同じエラーが複数回表示されます。 

このタイムアウト エラーは、600 秒 (10 分) のタイムアウトが発生したことを示します。 これは通常、カスタム .Net アプリケーションから、ステータスの更新情報が 10 分間発行されていないことを意味します。 アプリケーションが何らかの操作の待機中に長時間にわたって応答を停止した場合、アプリケーションがそのまま待機し続けたり、Azure Data Factory パイプラインの遅延を招いたりするのを防ぐために、10 分のタイムアウトが安全なメカニズムとなります。 

このタイムアウトは、カスタム アクティビティにリンクされている HDInsight クラスターの構成に由来します。 設定が **mapred.task.timeout**, 、Apache の既定設定は、ここに記載されているとおり、600,000 ミリ秒です既定値: http://hadoop.apache.org/docs/r2.4.0/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml

この既定値は、HDInsight プロビジョニング クラスターのプロビジョニング時に既定値を変更することでオーバーライドできます。 Azure Data Factory を使用する場合と **HDInsight オンデマンド** のリンクされたサービス、近くに HDInsightOnDemandLinkedService の JSON プロパティは、JSON のプロパティを追加できます。 たとえば、この JSON プロパティを使用して、値を 20 分に増やすことができます。
        
        "mapReduceConfiguration" :
        {
            "mapreduce.task.timeout":"1200000"
        }
        

構成プロパティが MSDN ドキュメント内の例 3 を参照してください詳細なコンテキストとこれらのマップを編集する JSON の例の全体を減らすには、ここ https://msdn.microsoft.com/library/azure/dn893526.aspx

## 問題: 登録済みのリソース プロバイダーが見つからないことを示すエラー 400 (無効な要求) により、PowerShell の要求に失敗する

2015 年 3 月 10 日の時点で、Azure Data Factory PowerShell 初期プライベート プレビュー バージョン 2014-05-01-preview、2014-07-01-preview、2014-08-01-preview は廃止されます。 に、Azure PowerShell のダウンロード、この URL http://go.microsoft.com/?linkid=9811175&clcid=0x409 からのダウンロードなどの一部になっている、ADF コマンドレットの最新バージョンを使用することをお勧めします。 

Azure PowerShell SDK の廃止されたバージョンを使用すると、次のエラーが表示される場合があります。

        HTTP/1.1 400 Bad Request
        Cache-Control: no-cache
        Pragma: no-cache
        Content-Type: application/json; charset=utf-8
        Expires: -1
        x-ms-request-id: e07181e4-e421-46be-8a08-1f71d5e90494
        x-ms-correlation-request-id: e07181e4-e421-46be-8a08-1f71d5e90494
        x-ms-routing-request-id: WESTUS:20150306T234829Z:e07181e4-e421-46be-8a08-1f71d5e90494
        Strict-Transport-Security: max-age=31536000; includeSubDomains
        Date: Fri, 06 Mar 2015 23:48:29 GMT
        Content-Length: 157
        {"error":{"code":"NoRegisteredProviderFound","message":"No registered resource provider found for location 'west US' and API version '2014-05-01-preview'."}}


## <a name="copywalkthrough"></a>チュートリアル: データのコピーで発生するエラーのトラブルシューティング
このチュートリアルでは、「Data Factory を使ってみる」の記事にあるチュートリアルでエラーを発生させ、Azure クラシック ポータルを使用してエラーのトラブルシューティングを行う方法について説明します。

### 前提条件
1. チュートリアルを完了して、 [Azure Data Factory を使ってみる][adfgetstarted] 記事です。
2. いることを確認、 **ADFTutorialDataFactory** でデータを生成、 **emp** Azure SQL データベースのテーブルです。  
3. これで、削除、 **emp** テーブル (**ドロップ emp テーブル**) Azure SQL Database からです。 これにより、エラーが発生します。
4. 次のコマンドを実行、 **Azure PowerShell** へのデータの書き込みを試みるように、パイプラインのアクティブな期間を更新する、 **emp** テーブルは存在しません。

         
        Set-AzureRmDataFactoryPipelineActivePeriod -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactory -StartDateTime 2014-09-29 –EndDateTime 2014-09-30 –Name ADFTutorialPipeline
    
    Replace **StartDateTime** value with the current day and **EndDateTime** value with the next day. 


### Azure ポータルを使用したエラーのトラブルシューティング

1.  ログイン [Azure ポータル][azure-portal]します。 
2.  クリックして **ADFTutorialDataFactory** から、 **スタート画面**します。 データ ファクトリの上のリンクが表示されない場合、 **スタート画面**, 、] をクリックして **参照** ハブ アンド クリック **すべて**です。 クリックして **データ ファクトリ.**  **参照** ブレードで、およびクリック **ADFTutorialDataFactory**します。
3.  表示される通知 **が、エラーが** 上、 **データセット** を並べて表示します。 クリックして **エラーのある**です。 はず **エラーありデータセット** ブレードです。

    ![［エラーあり］ リンクがある Data Factory][image-data-factory-troubleshoot-with-error-link]

4.  **データセット** エラー ブレードで、クリックして **EmpSQLTable** を表示する、 **テーブル** ブレードです。 

    ![[エラーありデータセット] ブレード][image-data-factory-troubleshoot-datasets-with-errors-blade]

5.  **テーブル** ブレードで、問題のあるスライスなどのエラーが発生したスライスを確認する必要があります、 **問題のあるスライス** 下部の一覧にします。 内のエラーが発生したスライスを確認できます、 **最近のスライス** ] ボックスの一覧です。 内のスライスをクリックして、 **問題のあるスライス** ] ボックスの一覧です。 

    ![問題のあるスライスを表示している [テーブル] ブレード][image-data-factory-troubleshoot-table-blade-with-problem-slices]

    クリックすると **問題のあるスライス** 表示されます (、特定の問題) ではなく、 **データ スライス** ブレードをクリック、 **特定の問題のあるスライス** を表示する、 **データ スライス** スライド選択したデータ スライスをします。

6.  **データ スライス** ブレード **EmpSQLTable**, 、すべて表示する **アクティビティの実行** 下部にあるリスト内のスライスに対してです。 をクリックして、 **アクティビティの実行** 失敗した一覧からです。

    ![データ スライス ブレード、アクティビティの実行][image-data-factory-troubleshoot-dataslice-blade-with-active-runs]


7.  **アクティビティの実行の詳細** ブレードで、アクティビティの実行を選択すると、エラーの詳細が表示されます。 このシナリオでは、参照してください: **Invalid object name 'emp'**します。

    ![エラーが発生しているアクティビティの実行の詳細][image-data-factory-troubleshoot-activity-run-with-error]

この問題を解決するには、作成、 **emp** SQL を使用してテーブルをスクリプトから [Data Factory を使ってみる][adfgetstarted] 記事です。


### Azure PowerShell コマンドレットを使用して、エラーのトラブルシューティングを行う
1.  起動 **Azure PowerShell**します。 
3. Get-AzureDataFactorySlice コマンドを実行してスライスとその状態を確認します。 [状態] が [失敗] になっているスライスが表示されるはずです。    

         
        Get-AzureRmDataFactorySlice -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactory -TableName EmpSQLTable -StartDateTime 2014-10-15

    Replace **StartDateTime** with the StartDateTime value you specified for the **Set-AzureRmDataFactoryPipelineActivePeriod**. 

        ResourceGroupName       : ADFTutorialResourceGroup
        DataFactoryName         : ADFTutorialDataFactory
        TableName               : EmpSQLTable
        Start                   : 10/15/2014 4:00:00 PM
        End                     : 10/15/2014 5:00:00 PM
        RetryCount              : 0
        Status                  : Failed
        LatencyStatus           :
        LongRetryCount          : 0

    Note the **Start** time for the problem slice (the slice with **Status** set to **Failed**) in the output. 
4. これで、実行、 **Get AzureRmDataFactoryRun** コマンドレットは、スライスのアクティビティの実行について詳細を取得します。
         
        Get-AzureRmDataFactoryRun -ResourceGroupName ADFTutorialResourceGroup -DataFactoryName ADFTutorialDataFactory -TableName EmpSQLTable -StartDateTime "10/15/2014 4:00:00 PM"

    値 **StartDateTime** 、前の手順でメモしたエラーまたは問題スライスの開始日時です。 日時は二重引用符で囲む必要があります。
5. エラーの詳細を含む (以下のような) 出力結果が表示されます。

        Id                      : 2b19475a-c546-473f-8da1-95a9df2357bc
        ResourceGroupName       : ADFTutorialResourceGroup
        DataFactoryName         : ADFTutorialDataFactory
        TableName               : EmpSQLTable
        ResumptionToken         :
        ContinuationToken       :
        ProcessingStartTime     : 10/15/2014 11:13:39 PM
        ProcessingEndTime       : 10/15/2014 11:16:59 PM
        PercentComplete         : 0
        DataSliceStart          : 10/15/2014 4:00:00 PM
        DataSliceEnd            : 10/15/2014 5:00:00 PM
        Status                  : FailedExecution
        Timestamp               : 10/15/2014 11:13:39 PM
        RetryAttempt            : 0
        Properties              : {}
        ErrorMessage            : Unknown error in CopyActivity: System.Data.SqlClient.SqlException (0x80131904): **Invalid object name 'emp'.**
                         at System.Data.SqlClient.SqlConnection.OnError(SqlException exception, Boolean
                      breakConnection, Action`1 wrapCloseInAction)
                         at System.Data.SqlClient.TdsParser.ThrowExceptionAndWarning(TdsParserStateObject stateObj,

 

## <a name="pighivewalkthrough"></a> チュートリアル: Hive/Pig 処理で発生するエラーのトラブルシューティング
このチュートリアルでは、Azure ポータルと Azure PowerShell の両方を使用して、Hive/Pig 処理で発生するエラーのトラブルシューティングを行う手順を説明します。 


### チュートリアル: Azure クラシック ポータルを使用した Pig/Hive 処理で発生するエラーのトラブルシューティング
このシナリオでは、HDInsight クラスターでの Hive 処理で発生するエラーが原因で、データ セットがエラー状態になっています。

1. をクリックして **エラーのある** に **データセット** タイルを **DATA FACTORY** ホーム ページです。

    ![[データセット] タイルの ［エラーあり］ リンク][image-data-factory-troubleshoot-walkthrough2-with-errors-link]

2.  **エラーありデータセット** ブレードで、をクリックして、 **テーブル** 興味のあることです。

    ![[エラーありデータセット] ブレード][image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]

3.  **テーブル** ブレードをクリックして、 **問題のあるスライス** と **ステータス** に設定 **失敗**します。

    ![問題のあるスライスを含むテーブル][image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]

4.  **データ スライス** ブレードで、をクリックして、 **アクティビティの実行** 失敗しました。

    ![実行が失敗しているデータ スライス][image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]

5.  **アクティビティの実行の詳細** ブレードで、HDInsight 処理に関連するファイルをダウンロードすることができます。 をクリックして **ダウンロード** の **status/stderr** エラーに関する詳細を含むエラー ログ ファイルをダウンロードします。

    ![［ダウンロード］ リンクがあるアクティビティの実行の詳細][image-data-factory-troubleshoot-activity-run-details]

    
### チュートリアル: Azure PowerShell を使用して Pig/Hive 処理で発生するエラーのトラブルシューティングを行う
1.  起動 **Azure PowerShell**します。 
3. Get-AzureDataFactorySlice コマンドを実行してスライスとその状態を確認します。 [状態] が [失敗] になっているスライスが表示されるはずです。    

         
        Get-AzureRmDataFactorySlice -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime 2014-05-04 20:00:00

    Replace **StartDateTime** with the StartDateTime value you specified for the **Set-AzureRmDataFactoryPipelineActivePeriod**. 

        ResourceGroupName : ADF
        DataFactoryName   : LogProcessingFactory
        TableName         : EnrichedGameEventsTable
        Start             : 5/5/2014 12:00:00 AM
        End               : 5/6/2014 12:00:00 AM
        RetryCount        : 0
        Status            : Failed
        LatencyStatus     :
        LongRetryCount    : 0


    Note the **Start** time for the problem slice (the slice with **Status** set to **Failed**) in the output. 
4. これで、実行、 **Get AzureRmDataFactoryRun** コマンドレットは、スライスのアクティビティの実行について詳細を取得します。
         
        Get-AzureRmDataFactoryRun -ResourceGroupName ADF -DataFactoryName LogProcessingFactory -TableName EnrichedGameEventsTable -StartDateTime "5/5/2014 12:00:00 AM"

    値 **StartDateTime** 、前の手順でメモしたエラーまたは問題スライスの開始日時です。 日時は二重引用符で囲む必要があります。
5. エラーの詳細を含む (以下のような) 出力結果が表示されます。

        Id                  : 841b77c9-d56c-48d1-99a3-8c16c3e77d39
        ResourceGroupName   : ADF
        DataFactoryName     : LogProcessingFactory3
        TableName           : EnrichedGameEventsTable
        ProcessingStartTime : 10/10/2014 3:04:52 AM
        ProcessingEndTime   : 10/10/2014 3:06:49 AM
        PercentComplete     : 0
        DataSliceStart      : 5/5/2014 12:00:00 AM
        DataSliceEnd        : 5/6/2014 12:00:00 AM
        Status              : FailedExecution
        Timestamp           : 10/10/2014 3:04:52 AM
        RetryAttempt        : 0
        Properties          : {}
        ErrorMessage        : Pig script failed with exit code '5'. See 'wasb://adfjobs@spestore.blob.core.windows.net/PigQuery
                                Jobs/841b77c9-d56c-48d1-99a3-8c16c3e77d39/10_10_2014_03_04_53_277/Status/stderr' for more details.
        ActivityName        : PigEnrichLogs
        PipelineName        : EnrichGameLogsPipeline
        Type                :

6. 実行する **保存 AzureRmDataFactoryLog** コマンドレットの Id 値は、上記の出力からわかり、ログをダウンロードするファイルを使用して、 **-downloadlogs** コマンドレットに対してオプション。



[adfgetstarted]: data-factory-get-started.md
[adf-tutorial]: data-factory-tutorial.md
[use-custom-activities]: data-factory-use-custom-activities.md
[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
 

