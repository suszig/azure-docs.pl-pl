<properties 
    pageTitle="Azure Data Factory を使用してログ ファイルの移動と処理を行う (Azure クラシック ポータル)" 
    description="この高度なチュートリアルでは、現実に近いシナリオについて説明し、そのシナリオを Azure クラシック ポータルで Azure Data Factory サービスと Data Factory Editor を使用して実装します。" 
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

# チュートリアル: マーケティング キャンペーンの有効性の測定  
Contoso は、ゲーム機、携帯デバイス、パーソナル コンピューター (PC) など、複数のプラットフォーム向けにゲームを製作するゲーム会社です。 これらのゲームはそれぞれが大量のログを産み出します。Contoso 社の目標は、これらのゲームが産み出すログを収集解析することで、顧客の好み、人口統計データ、使用形態などの有益な情報を手に入れ、アップセルやクロスセルの機会を見極め、新しい魅力的な機能を開発してビジネスの成長を促進し、顧客により良い体験を提供することです。

このチュートリアルでは、Data Factory パイプラインを作成して、サンプル ログの収集、参照データによるログの処理と強化、データの変換により、Contoso 社が最近立ち上げたマーケティング キャンペーンの有効性を評価します。 次の 3 つのパイプラインがあります。

1.   **PartitionGameLogsPipeline** blob ストレージから未処理のゲーム イベントを読み取り、年、月、日に基づくパーティションを作成します。
2.   **EnrichGameLogsPipeline** を geo コードの参照データのパーティション分割されたゲーム イベントを結合し、IP アドレスを対応する地理的場所にマップすることによって、データを拡充します。
3.   **AnalyzeMarketingCampaignPipeline** パイプラインはマーケティング キャンペーン有効性を含む最終的な出力を作成する広告データと共に処理して、強化されたデータを活用します。

## チュートリアルの準備をする
1.  読み取り [Azure Data Factory の概要][adfintroduction] に最上位レベルの概念を理解し、Azure Data Factory の概要について説明します。
2.  このチュートリアルを実施するには Azure サブスクリプションが必要です。 サブスクリプションの入手方法の詳細については、次を参照してください。 [購入に関するオプション](http://azure.microsoft.com/pricing/purchase-options/), 、[メンバー プラン](http://azure.microsoft.com/pricing/member-offers/), 、または [無料評価版](http://azure.microsoft.com/pricing/free-trial/)します。
3.  ダウンロードしてインストールする必要があります [Azure PowerShell][ダウンロード azure powershell] コンピューターにします。 Data Factory コマンドレットを実行し、サンプル データと pig/hive スクリプトを BLOB ストレージにアップロードします。 
2.  **(推奨)** 確認し、練習をチュートリアルで、 [Azure Data Factory を使ってみる][adfgetstarted] ポータルとコマンドレットに慣れるための簡単なチュートリアルについての記事です。
3.  **(推奨)** 確認し、実際に、 [の Pig の使用と Azure Data Factory を使って Hive][usepigandhive] 内部設置型のデータ ソースからデータを Azure blob ストアに移動するパイプラインの作成のチュートリアルについての記事です。
4.  ダウンロード [ADFWalkthrough][adfwalkthrough ダウンロード] ファイルを **C:\ADFWalkthrough** フォルダー **フォルダー構造は維持**:
    - **パイプライン:** パイプラインの定義を含む JSON ファイルが含まれています。
    - **テーブル:** テーブルの定義を含む JSON ファイルが含まれています。
    - **LinkedServices:** ストレージおよびコンピューティング (HDInsight) の定義を含む JSON ファイルを含むクラスター 
    - **[スクリプト]:** はデータの処理で使用され、パイプラインから呼び出される Hive および Pig のスクリプトが含まれています
    - **SampleData:** このチュートリアル用サンプル データが含まれています
    - **OnPremises:** 内部設置型データへのアクセス方法を示すために使用する JSON ファイルとスクリプトが含まれています
    - **uploadSampleDataAndScripts.ps1:** このスクリプトは、サンプル データとスクリプトを Azure にアップロードします。
5. 以下の Azure リソースが作成済みであることを確認してください。            
    - Azure ストレージ アカウント
    - Azure SQL Database
    - Azure HDInsight クラスター Version 3.1 以上 (または、Data Factory サービスで自動的に作成されるオンデマンド HDInsight クラスターを使用します)   
7. Azure リソースを作成したなら、上記の各リソースへの接続に必要な情報を持っていることを確認します。
    - **Azure ストレージ アカウント** - アカウント名とアカウント キー。  
    - **Azure SQL Database** -サーバー、データベース、ユーザー名およびパスワード。
    - **Azure HDInsight クラスター**します。 -HDInsight のクラスター、ユーザー名、パスワード、およびアカウント名およびこのクラスターに関連付けられている Azure ストレージのアカウント キーの名前。 独自の HDInsight クラスターではなく、オンデマンド HDInsight クラスターを使用する場合は、この手順を省略できます。  
8. 起動 **Azure PowerShell** し、次のコマンドを実行します。 Azure PowerShell は開いたままにしておきます。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。
    - 実行 **Add-azureaccount** ユーザー名と、Azure ポータルにサインインに使用するパスワードを入力します。  
    - 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    - 実行 **Select-azuresubscription** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。 

## 概要
全体のワークフローは以下のとおりです。

![チュートリアルの全体のフロー][image-data-factory-tutorial-end-to-end-flow]

1.  **PartitionGameLogsPipeline** blob ストレージ (RawGameEventsTable) から未処理のゲーム イベントを読み取り、年、月、日 (PartitionedGameEventsTable) に基づくパーティションを作成します。
2.  **EnrichGameLogsPipeline** と geo コード (RefGetoCodeDictionaryTable) パーティション分割されたゲーム イベント (table PartitionGameLogsPipeline の出力である) を結合および IP アドレスを対応する地理的位置 (EnrichedGameEventsTable) にマッピングすることによって、データを拡充します。
3.  **AnalyzeMarketingCampaignPipeline** パイプラインは、これを広告データ (RefMarketingCampaignnTable) マーケティング キャンペーンの有効性の最終的な出力を作成する Azure SQL database (MarketingCampainEffectivensessSQLTable) と分析用の Azure blob ストレージ (MarketingCampaignEffectivenessBlobTable) にコピーされますと共に処理して、強化されたデータ (EnrichGameLogsPipeline によって生成される処理) を活用します。
    
## チュートリアル: ワークフローの作成、デプロイ、監視
1. [手順 1. サンプル データとスクリプトをアップロードする](#MainStep1)です。 この手順では、すべてのサンプル データ (すべてのログと参照データを含む) とワークフローによって実行される Hive/Pig スクリプトをアップロードします。 実行するスクリプトは、Azure SQL Database (MarketingCampaigns)、テーブル、ユーザー定義型、およびストアド プロシージャの作成も行います。
2. [手順 2: Azure data factory を作成する](#MainStep2)です。 この手順では、"LogProcessingFactory" という名前の Azure データ ファクトリを作成します。
3. [手順 3: リンクされたサービスを作成する](#MainStep3)です。 この手順では、以下のリンクされたサービスを作成します: 
    
    -   **StorageLinkedService**します。 未処理のゲーム イベント、パーティションに分割されたゲーム イベント、強化されたゲーム イベント、マーケティング キャンペーンの有効な情報、geo コードの参照データを含む Azure ストレージの場所をリンクし、LogProcessingFactory へのマーケティング データの参照を行います。   
    -   **AzureSqlLinkedService**します。 マーケティング キャンペーンの有効性の情報を含む Azure SQL データベースをリンクします。 
    -   **HDInsightStorageLinkedService**します。 HDInsightLinkedService が参照する HDInsight クラスターに関連付けられている Azure BLOB ストレージをリンクします。 
    -   **HDInsightLinkedService**します。 Azure HDInsight クラスターを LogProcessingFactory にリンクします。 このクラスターはデータの pig/hive 処理に使用されます。 
        
4. [手順 4: テーブルを作成する](#MainStep4)です。 この手順では、以下のテーブルを作成します。     
    
    - **RawGameEventsTable**します。 このテーブルは、未処理のゲーム イベントのデータの場所を、StorageLinkedService によって定義された Azure BLOB ストレージ内に指定します (adfwalkthrough/logs/rawgameevents/)。 
    - **PartitionedGameEventsTable**します。 このテーブルは、パーティションに分割されたゲーム イベントのデータの場所を、StorageLinkedService によって定義された Azure BLOB ストレージ内に指定します (adfwalkthrough/logs/partitionedgameevents/)。 
    - **RefGeoCodeDictionaryTable**します。 このテーブルは、地理的参照コードの場所を、StorageLinkedService によって定義された Azure BLOB ストレージ内に指定します (adfwalkthrough/refdata/refgeocodedictionary/)。
    - **RefMarketingCampaignTable**します。 このテーブルは、マーケティング キャンペーンの参照データの場所を、StorageLinkedService によって定義された Azure BLOB ストレージ内に指定します (adfwalkthrough/refdata/refmarketingcampaign/)。
    - **EnrichedGameEventsTable**します。 このテーブルは、強化されたゲーム イベントのデータの場所を、StorageLinkedService によって定義された Azure BLOB ストレージ内に指定します (adfwalkthrough/logs/enrichedgameevents/)。
    - **MarketingCampaignEffectivenessSQLTable**します。次の表は、マーケティング キャンペーン有効性データを含む AzureSqlLinkedService によって定義された Azure SQL Database での SQL テーブル (MarketingCampaignEffectiveness) を指定します。 
    - **MarketingCampaignEffectivenessBlobTable**します。 このテーブルは、マーケティング キャンペーンの有効性データの場所を、StorageLinkedService によって定義された Azure BLOB ストレージ内に指定します (adfwalkthrough/marketingcampaigneffectiveness/)。 

    
5. [手順 5 を作成してパイプラインのスケジュール](#MainStep5)します。 この手順では、以下のパイプラインを作成します。
    - **PartitionGameLogsPipeline**します。 このパイプラインは、BLOB ストレージ (RawGameEventsTable) から未処理のゲーム イベントを読み取り、年、月、日に基づくパーティション (PartitionedGameEventsTable) を作成します。 


        ![PartitionGamesLogs pipeline][image-data-factory-tutorial-partition-game-logs-pipeline]


    - **EnrichGameLogsPipeline**. This pipeline joins partitioned game events (PartitionedGameEvents table, which is an output of the PartitionGameLogsPipeline) with geo-code (RefGetoCodeDictionaryTable) and enriches the data by mapping an IP address to the corresponding geo-location (EnrichedGameEventsTable) 

        ![EnrichedGameLogsPipeline][image-data-factory-tutorial-enrich-game-logs-pipeline]

    - **AnalyzeMarketingCampaignPipeline**. This pipeline leverages the enriched game event data (EnrichedGameEventTable produced by the EnrichGameLogsPipeline) and processes it with the advertising data (RefMarketingCampaignnTable) to create the final output of marketing campaign effectiveness, which is copied to the Azure SQL database (MarketingCampainEffectivensessSQLTable) and an Azure blob storage (MarketingCampaignEffectivenessBlobTable) for analytics


        ![MarketingCampaignPipeline][image-data-factory-tutorial-analyze-marketing-campaign-pipeline]


6. [手順 6. パイプラインとデータ スライスを監視する](#MainStep6)です。 この手順では、Azure クラシック ポータルを使用して、パイプライン、テーブル、データ スライスを監視します。

## <a name="MainStep1"></a> 手順 1. サンプル データとスクリプトをアップロードする
この手順では、すべてのサンプル データ (すべてのログと参照データを含む) とワークフローによって呼び出される Hive/Pig スクリプトをアップロードします。 また実行するスクリプトと呼ばれる Azure SQL データベースを作成する **MarketingCampaigns**, 、テーブル、ユーザー定義型、およびストアド プロシージャです。 

テーブル、ユーザー定義型、およびストアド プロシージャは、マーケティング キャンペーン有効性の結果を Azure BLOB ストレージから Azure SQL データベースに移動する際に使用されます。

1. 開いている **uploadSampleDataAndScripts.ps1** から **C:\ADFWalkthrough** フォルダー (または、抽出したファイルが含まれているフォルダー)、好みのエディターで、強調表示部分を自分のクラスター情報に置き換えるし、ファイルを保存します。


        $storageAccount = <storage account name>
        $storageKey = <storage account key>
        $azuresqlServer = <sql azure server>.database.windows.net
        $azuresqlUser = <sql azure user>@<sql azure server>
        $azuresqlPassword = <sql azure password>

 
    This script requires you have sqlcmd utility installed on your machine. If you have SQL Server isntalled, you already have it. Otherwise, [download][sqlcmd-install] and install the utility. 
    
    Alternatively, you can use the files in the folder: C:\ADFWalkthrough\Scripts to upload pig/hive scripts and sample files to the adfwalkthrough container in the blob storage, and create MarketingCampaignEffectiveness table in the MarketingCamapaigns Azure SQL database.
   
2. ローカル コンピューターに Azure SQL Database へのアクセス権があることを確認してください。 アクセスを有効にするには、 [Azure Classic Portal](http://manage.windowsazure.com) または **sp_set_firewall_rule** 、コンピューターの IP アドレスに対するファイアウォール規則を作成する master データベースにします。 この変更が有効になるまで最大で 5 分かかる場合があります。 参照してください [Azure SQL のファイアウォール ルールを設定する][azure sql ファイアウォール]します。
4. Azure PowerShell で、サンプルを展開した場所に移動します (例: **C:\ADFWalkthrough**)
5. 実行 **uploadSampleDataAndScripts.ps1** 
6. スクリプトが正常に実行されると、以下が表示されます。

        $storageAccount = <storage account name>
        PS C:\ ADFWalkthrough> & '.\uploadSampleDataAndScripts.ps1'

        Name            PublicAccess        LastModified
        -----           --------        ------
        ADFWalkthrough      off         6/6/2014 6:53:34 PM +00:00
    
        Uploading sample data and script files to the storage container [adfwalkthrough]

        Container Uri: https://<yourblobstorage>.blob.core.windows.net/adfwalkthrough

        Name                        BlobType   Length   ContentType               LastModified                        
        ----                        --------   ------   -----------               ------------                        
        logs/rawgameevents/raw1.csv  BlockBlob  12308   application/octet-stream  6/6/2014 6:54:35 PM 
        logs/rawgameevents/raw2.csv  BlockBlob  16119   application/octet-stream  6/6/2014 6:54:35 PM 
        logs/rawgameevents/raw3.csv  BlockBlob  16062   application/octet-stream  6/6/2014 6:54:35 PM 
        logs/rawgameevents/raw4.csv  BlockBlob  16245   application/octet-stream  6/6/2014 6:54:35 PM 
        refdata/refgeocodedictiona.. BlockBlob  18647   application/octet-stream  6/6/2014 6:54:36 PM 
        refdata/refmarketingcampai.. BlockBlob  8808    application/octet-stream  6/6/2014 6:54:36 PM
        scripts/partitionlogs.hql    BlockBlob  2449    application/octet-stream  6/6/2014 6:54:36 PM 
        scripts/enrichlogs.pig       BlockBlob  1631    application/octet-stream  6/6/2014 6:54:36 PM
        scripts/transformdata.hql    BlockBlob  1753    application/octet-stream  6/6/2014 6:54:36 PM

        6/6/2014 11:54:36 AM Summary
        6/6/2014 11:54:36 AM 1. Uploaded Sample Data Files to blob container.
        6/6/2014 11:54:36 AM 2. Uploaded Sample Script Files to blob container.
        6/6/2014 11:54:36 AM 3. Created ‘MarketingCampaigns’ Azure SQL database and tables.
        6/6/2014 11:54:36 AM You are ready to deploy Linked Services, Tables and Pipelines. 

## <a name="MainStep2"></a> 手順 2. Azure Data Factory を作成する
この手順で、という名前の Azure データ ファクトリを作成する **LogProcessingFactory**します。

1.  ログインした後、 [Azure ポータル][azure ポータル], 、] をクリックして **新規** の左下隅から] をクリックして **データ分析** で、 **作成** ブレードをクリック **Data Factory** 上、 **データ分析** ブレードです。 

    ![New->DataFactory][image-data-factory-new-datafactory-menu] 

5.  **新しいデータ ファクトリ** ブレードで、入力 **LogProcessingFactory** の **名前**します。

    ![[データ ファクトリ] ブレード][image-data-factory-tutorial-new-datafactory-blade]

6. という名前の Azure リソース グループを作成していない場合は **ADF** 既に、以下を実行します。
    1. クリックして **リソース グループ名**, 、] をクリック **新しいリソース グループを作成**します。
    
        ![[リソース グループ] ブレード][image-data-factory-tutorial-resourcegroup-blade]
    2.  **リソース グループの作成** ブレードで、入力 **ADF** 、リソース グループの名前の **[ok]**します。
    
        ![リソース グループの作成][image-data-factory-tutorial-create-resourcegroup]
7. 選択 **ADF** の **リソース グループ名**します。  
8.   **新しいデータ ファクトリ** ブレードで、ことに注意して **スタート画面に追加** が既定で選択します。 これにより、スタート画面 (Azure ポータルへのログイン時に表示される画面) のデータ ファクトリにリンクが追加されます。

    ![データ ファクトリ作成ブレード
][image-data-factory-tutorial-create-datafactory]

9.   **新しいデータ ファクトリ** ブレードで、をクリックして **作成** data factory を作成します。
10. データ ファクトリが作成された後が表示される、 **DATA FACTORY** 」というタイトルのブレード **LogProcessingFactory**します。

    ![データ ファクトリのホーム ページ][image-data-factory-tutorial-datafactory-homepage]

    
    If you do not see it, do one of the following:

    - Click **LogProcessingFactory** on the **Startboard** (home page)
    - Click **BROWSE** on the left, click **Everything**, click **Data factories**, and click the data factory.
 
    The name of the Azure data factory must be globally unique. If you receive the error: **Data factory name “LogProcessingFactory” is not available**, change the name (for example, yournameLogProcessingFactory). Use this name in place of LogProcessingFactory while performing steps in this tutorial.
 
## <a name="MainStep3"></a>手順 3. リンクされたサービスを作成する

> [AZURE.NOTE] この資料では、Azure 旧ポータルで、具体的には、Data Factory エディターを使用して、リンクされたサービス、テーブル、およびパイプラインを作成します。 参照してください [Azure PowerShell を使用してチュートリアル][adftutorial を使用して powershell] Azure PowerShell を使用してこのチュートリアルを実行する場合。 

この手順では、以下のリンクされたサービスを作成します:

- StorageLinkedService
- AzureSqlLinkedService
- HDInsightStorageLinkedService
- HDInsightLinkedService。 

### StorageLinkedService と HDInsightStorageLinkedService の作成

1.   **DATA FACTORY** ブレードで、をクリックして **作成者および展開** を起動するタイル、 **エディター** data factory のです。

    ![[作成とデプロイ] タイル][image-author-deploy-tile] 

    参照してください [Data Factory エディター][data factory エディター] Data Factory エディターの詳細についてのトピックです。

2.   **エディター**, 、クリックして **新しいデータ ストア** ] ボタンをクリックして [ **Azure ストレージ** 、ドロップ ダウン メニューからです。 Azure Storage のリンクされたサービスを作成するための JSON テンプレートが右側のウィンドウに表示されます。    
    
    ![エディターの [新しいデータ ストア] ボタン][image-editor-newdatastore-button]

3. 置換 **accountname** と **accountkey** アカウント名と、Azure ストレージ アカウントのアカウント キーの値を使用します。

    ![エディターの Blob Storage JSON][image-editor-blob-storage-json]    
    
    参照してください [JSON スクリプティング リファレンス](http://go.microsoft.com/fwlink/?LinkId=516971) JSON プロパティの詳細について。

4. クリックして **展開** 、ツールバーの StorageLinkedService をデプロイします。 メッセージを表示することを確認 **リンクされたサービス作成が正常に** タイトル バーにします。

    ![エディターの Blob Storage デプロイ][image-editor-blob-storage-deploy]

5. リンクされたサービスのという名前の別の Azure ストレージを作成する手順を繰り返します。 **HDInsightStorageLinkedService** 、HDInsight クラスターに関連付けられている記憶域にします。 リンクされたサービスの JSON スクリプトでの値を変更、 **名前** プロパティを **HDInsightStorageLinkedService**します。 

### AzureSqlLinkedService の作成
1.  **Data Factory エディター** , 、クリックして **新しいデータ ストア** ] ボタンをクリックして [ **Azure SQL データベース** 、ドロップ ダウン メニューからです。 Azure SQL のリンクされたサービスを作成するための JSON テンプレートが右側のウィンドウに表示されます。
2. 置換 **servername**, 、**username@servername**, 、および **パスワード** Azure SQL サーバー、ユーザー アカウントおよびパスワードの名前を持つ。  
3. 置換 **databasename** と **MarketingCampaigns**します。 これは、手順 1. で実行したスクリプトによって作成された Azure SQL Database です。 このデータベースがスクリプトによって実際に作成されていることを確認する必要があります (エラーが発生した場合)。 
3. クリックして **展開** 、ツールバーの AzureSqlLinkedService を作成してデプロイします。

### HDInsightLinkedService の作成
Azure Data Factory サービスはオンデマンド クラスターの作成をサポートしており、このクラスターを使用して入力データの処理と出力データの生成を行います。 また、独自のクラスターを使って同じ処理を行うことも可能です。 オンデマンド HDInsight クラスターを使用する場合は、スライスごとにクラスターが作成されます。 一方、独自の HDInsight クラスターを使用する場合、そのクラスターはすぐにスライスを処理できる状態にあります。 そのため、オンデマンド クラスターを使用すると、独自のクラスターを使用するよりデータの出力が遅いと感じる場合があります。 試しに、オンデマンド クラスターを使用してみましょう。 

#### オンデマンド HDInsight クラスターを使用するには
1. クリックして **新しいコンピューティング** し、コマンド バーから **、オンデマンド HDInsight クラスター** ] メニューからです。
2. JSON スクリプトで、以下の手順を実行します。 
    1.  **ClusterSize** プロパティには、HDInsight クラスターのサイズを指定します。
    3.  **TimeToLive** プロパティには、どの顧客アイドル状態でいられる時間が削除されるかを指定します。 
    4.  **バージョン** プロパティには、使用する HDInsight のバージョンを指定します。 このプロパティを除外した場合は、最新バージョンが使用されます。  
    5.  **LinkedServiceName**, 、指定 **HDInsightStorageLinkedService** 入門チュートリアルで作成したすることです。 

            {
                "name": "HDInsightOnDemandLinkedService",
                "properties": {
                    "type": "HDInsightOnDemand",
                    "description": "",
                    "typeProperties": {
                        "clusterSize": "4",
                        "timeToLive": "00:30:00",
                        "version": "3.2",
                        "linkedServiceName": "StorageLinkedService"
                    }
                }
            }

         **型** リンクされたサービスの設定は **HDInsightOnDemand**します。

2. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。
   
   
#### 独自の HDInsight クラスターを使用するには 

1. クリックして **新しいコンピューティング** し、コマンド バーから **HDInsight クラスター** ] メニューからです。
2. JSON スクリプトで、以下の手順を実行します。 
    1.  **ClusterUri** プロパティには、HDInsight の URL を入力します。 例: https://<clustername>.azurehdinsight.net/     
    2.  **UserName** プロパティには、HDInsight クラスターへのアクセス権を持つユーザー名を入力します。
    3.  **パスワード** プロパティには、ユーザーのパスワードを入力します。 
    4.  **LinkedServiceName** プロパティには、入力 **StorageLinkedService**します。 これは、入門チュートリアルで作成したリンク サービスです。 

    自分を **型** リンクされたサービスの設定は **HDInsightBYOCLinkedService** (BYOC は、独自のクラスターを表します)。 

2. をクリックして **展開** 、コマンド バー、リンクされたサービスをデプロイします。


## <a name="MainStep4"></a>手順 4. テーブルを作成する
 
この手順では、以下の Data Factory テーブルを作成します。 

- RawGameEventsTable
- PartitionedGameEventsTable
- RefGeoCodeDictionaryTable
- RefMarketingCampaignTable
- EnrichedGameEventsTable
- MarketingCampaignEffectivenessSQLTable
- MarketingCampaignEffectivenessBlobTable

    ![チュートリアルの全体のフロー][image-data-factory-tutorial-end-to-end-flow]
 
上の図は、中央の列がパイプラインを、上と下の列がテーブルを表しています。 

### テーブルを作成するには
    
1.  **エディター** Data Factory をクリックして **新しいデータセット** をクリックしてツール バー ボタン **Azure Blob ストレージ** 、ドロップ ダウン メニューからです。 
2. 右側のウィンドウの JSON スクリプトを置き換えます、 **RawGameEventsTable.json** ファイルから、 **C:\ADFWalkthrough\Tables** フォルダーです。
3. クリックして **展開** 、ツールバーのテーブルを作成してデプロイします。 表示されていることを確認、 **テーブルは正常に作成された** エディターのタイトル バーにメッセージです。
4. 次のファイルの内容で手順 1. ～ 3. を繰り返します。 
    1. PartitionedGameEventsTable.json
    2. RefGeoCodeDictionaryTable.json
    3. RefMarketingCampaignTable.json
    4. EnrichedGameEventsTable.json
    5. MarketingCampaignEffectivenessBlobTable.json 
5. 次のファイルの内容で手順 1. ～ 3. を繰り返します。 Select **Azure Sql** ] をクリックした後 **新しいデータセット**します。
    1. MarketingCampaignEffectivenessSQLTable.json
    

## <a name="MainStep5"></a>手順 5. パイプラインを作成してスケジュールを設定する
この手順では、以下のパイプラインを作成します。 

- PartitionGameLogsPipeline
- EnrichGameLogsPipeline
- AnalyzeMarketingCampaignPipeline

### パイプラインを作成するには

1.  **Data Factory エディター**, をクリックして **新しいパイプライン** ツールバーのボタンをクリックします。 ボタンが表示されない場合は、ツール バーの **[...](省略記号)** をクリックします。 またを右クリックして **パイプライン** ツリー ビュー] をクリック **新しいパイプライン**します。
2. 右側のウィンドウの JSON スクリプトを置き換えます、 **PartitionGameLogsPipeline.json** ファイルから、 **C:\ADFWalkthrough\Pipelines** フォルダーです。
3. 追加、 **コンマ (',')** の最後に **閉じ角かっこ ('] ')** JSON にし、その閉じ角かっこの後に次の 3 行を追加します。 

        "start": "2014-05-01T00:00:00Z",
        "end": "2014-05-05T00:00:00Z",
        "isPaused": false

    このチュートリアルのサンプル データが 2014 年 5 月 1 日から 2014 年 5 月 5 日であるため、開始時刻と終了時刻が 2014 年 5 月 1 日および 2014 年 5 月 5 日に設定されていることに注意してください。 
    
    オンデマンド HDInsight リンク サービスを使用している場合は、設定 **linkedServiceName** プロパティを **HDInsightOnDemandLinkedService**します。
3. クリックして **展開** 、ツールバーのパイプラインを作成してデプロイします。 表示されていることを確認、 **パイプラインは正常に作成された** エディターのタイトル バーにメッセージです。
4. 次のファイルの内容で手順 1. ～ 3. を繰り返します。 
    1. EnrichGameLogsPipeline.json
    2. AnalyzeMarketingCampaignPipeline.json
4. クリックして Data Factory のブレードを閉じ **X** (右上隅)、ホーム ページを参照してください (* * DATA FACTORY * * ブレード)、データ ファクトリ。 

### ダイアグラム ビュー

1.  **DATA FACTORY** ブレード、 **LogProcessingFactory**, 、] をクリックして **ダイアグラム**します。 

    ![Diagram Link][image-data-factory-tutorial-diagram-link]

2. 表示されているダイアグラムは並べ替えることが可能で、以下のダイアグラムは上部が直接の入力を示し、下部が出力を示しています。 あることがわかりますの出力、 **PartitionGameLogsPipeline** EnrichGameLogsPipeline の出力を入力として渡される、 **EnrichGameLogsPipeline** に渡される、 **AnalyzeMarketingCampaignPipeline**します。 タイトルをダブルクリックして、ブレードが示すアーティファクトについての詳細を表示します。

    ![Diagram View][image-data-factory-tutorial-diagram-view]

3. 右クリック **AnalyzeMarketingCampaignPipeline**, 、] をクリック **パイプラインを開く**します。 アクティビティの入力呼び出し力のデータセットと、パイプライン内のすべてのアクティビティが表示されます。 
 
    ![パイプラインを開く](./media/data-factory-tutorial/AnalyzeMarketingCampaignPipeline-OpenPipeline.png)

    クリックして **Data factory** 、すべてのパイプラインでダイアグラム ビューに戻るために左上隅にある階層リンクにします。  


**ご利用ありがとうございます。** Azure Data Factory、リンクされたサービス、パイプライン、テーブルを作成し、ワークフローを開始することに成功しました。 


## <a name="MainStep6"></a>手順 6. パイプラインとデータ スライスを監視する 

1.  ない場合、 **DATA FACTORY** ブレード、 **LogProcessingFactory** 開いた状態で行うことができます、次のいずれか。
    1.  クリックして **LogProcessingFactory** 上、 **スタート画面**します。 データ ファクトリを作成するときに、 **スタート画面に追加** オプションは自動的にチェックします。

        ![Monitoring Startboard][image-data-factory-monitoring-startboard]

    2. をクリックして **参照**, で、 **参照** ブレードで、 **データ ファクトリ** 選択 **LogProcessingFactory** で、 **データ ファクトリ** ブレードです。

        ![データ ファクトリの監視][image-data-factory-monitoring-browse-datafactories]
2. いくつかの方法でデータ ファクトリが監視できます。 パイプラインまたはデータ セットで監視が始められます。 パイプラインで監視を始め、さらに詳しく学びましょう。 
3.  クリックして **パイプライン** 上、 **DATA FACTORY** ブレードです。 
4.  クリックして **PartitionGameLogsPipeline** パイプライン] ブレードでします。 
5.   **パイプライン** ブレード **PartitionGameLogsPipeline**, 、パイプラインを使用するを参照してください **RawGameEventsTable** データセット。  クリックして **RawGameEventsTable**します。

    ![パイプラインでの使用と生成][image-data-factory-monitoring-pipeline-consumed-produced]

6. [テーブル] ブレードに **RawGameEventsTable**, 、すべてのスライスを参照してください。 次のスクリーン ショットは、すべてのスライスが **準備ができて** 状態および問題のあるスライスはありません。 これは、そのデータがすぐに処理できることを意味します。 

    ![RawGameEventsTable TABLE blade][image-data-factory-monitoring-raw-game-events-table]

    両方とも **最近更新したスライス** と **最近失敗したスライス** リストは並んで、 **最終更新時刻**します。 次の状況では、スライスの更新時刻が変更されます。    

    -  スライスのステータス手動で更新するなどを使用して、 **セット AzureRmDataFactorySliceStatus** () をクリックして **実行** 上、 **スライス** のあるスライス] ブレードです。
    -  スライスの実行 (実行の開始、実行の終了と失敗、実行の終了と成功など) により、スライスの状態が変わります。
 
    一覧のタイトルをクリックしてまたは **.(省略記号)** スライスの大規模な一覧を表示します。 クリックして **フィルター** 、ツールバーのスライスをフィルター処理します。  
    
    代わりに、スライスの開始/終了時刻で並べ替えられたデータ スライスを表示する] をクリックして **データ スライス (スライスの時刻) 別** を並べて表示します。  

    ![データ スライス (スライスの時刻別)][DataSlicesBySliceTime]
 
7. 、 **パイプライン** ブレード **PartiionGameLogsPipeline**, 、クリックして **Produced**します。 
8. このパイプラインが生成するデータ セットの一覧が表示されるはずです。 
9. クリックして **PartitionedGameEvents** テーブルに、 **データセットを生成** ブレードです。 
10. いることを確認、 **ステータス** のすべてのスライスに設定されている **準備**します。 
11. になっているスライスのいずれかをクリックして **準備** を表示する、 **データ スライス** そのスライスのブレードです。

    ![RawGameEventsTable DATA SLICE blade][image-data-factory-monitoring-raw-game-events-table-dataslice-blade]

    エラーが発生していた場合、ここに **[失敗]** という状態が表示されます。  か両方のスライスの状態を表示することもあります **準備**, 、またはその両方の状態 **PendingValidation**, のスライスの処理速度に応じて、します。

    スライスのではない場合、 **準備ができて** 状態、準備ができていないし、現在のスライスの実行をブロックするアップ ストリームのスライスを確認することができます、 **準備ができていないアップ ストリーム スライス** ] ボックスの一覧です。
 
    参照してください、 [Azure Data Factory 開発者向けリファレンス][開発者向けリファレンス] すべての可能なスライスの状態を把握します。

12.  **データ スライス** ブレードから実行] をクリックして、 **アクティビティの実行** ] ボックスの一覧です。 そのスライスの [アクティビティの実行] ブレードが表示されるはずです。 次を参照する必要があります **アクティビティの実行の詳細** ブレードです。

    ![[アクティビティの実行の詳細] ブレード][image-data-factory-monitoring-activity-run-details]

13. クリックして **ダウンロード** ファイルをダウンロードします。 この画面は、HDInsight の処理で発生するエラーのトラブルシューティングを行うときに、特に役立ちます。 
     
    
すべてのパイプラインには、実行が完了した場合に検討することができます、 **MarketingCampaignEffectivenessTable** で、 **MarketingCampaigns** Azure SQL データベースが結果を表示します。 

**ご利用ありがとうございます。** これでワークフローの監視とトラブルシューティングができます。 Azure Data Factory を使用してデータを処理し分析結果を得る方法を学びました。

## チュートリアルをさらに進めてオンプレミス データを使用する
この記事のチュートリアルにあるログ処理のシナリオの最後の手順で、マーケティング キャンペーンの有効性の出力が Azure SQL Database にコピーされました。 分析のため、所属する組織内にあるオンプレミスの SQL Server にこのデータを移動することも可能です。
 
マーケティング キャンペーンの有効性データを Azure BLOB からオンプレミスの SQL Server にコピーするには、この記事のチュートリアルで導入したオンプレミスのリンクされたサービス、テーブル、パイプラインを追加で作成する必要があります。

実習、 [チュートリアル: を使用して内部設置型データ ソース][チュートリアル onpremises] を内部設置型 SQL Server データベースにマーケティング キャンペーン有効性データをコピーするパイプラインを作成する方法を参照してください。


[モニターを使用して-powershell]: data-factory-monitor-manage-using-powershell.md
[カスタム アクティビティを使用します。]: data-factory-use-custom-activities.md
[トラブルシューティングを行う]: data-factory-troubleshoot.md
[コマンドレット リファレンス]: http://go.microsoft.com/fwlink/?LinkId=517456
[data factory エディター]: data-factory-editor.md

[adfsamples]: data-factory-samples.md
[adfgetstarted]: data-factory-get-started.md
[adftutorial を使用して powershell]: data-factory-tutorial-using-powershell.md
[adfintroduction]: data-factory-introduction.md
[usepigandhive]: data-factory-data-transformation-activities.md
[チュートリアル onpremises]: data-factory-tutorial-extend-onpremises.md
[ダウンロード azure powershell]: ../powershell-install-configure.md

[azure ポータル]: http://portal.azure.com
[azure 購入オプション]: http://azure.microsoft.com/pricing/purchase-options/
[azure のオファー メンバー]: http://azure.microsoft.com/pricing/member-offers/
[azure の無料評価版]: http://azure.microsoft.com/pricing/free-trial/

[sqlcmd インストール]: http://www.microsoft.com/download/details.aspx?id=35580
[azure sql ファイアウォール]: https://azure.microsoft.com/documentation/articles/sql-database-configure-firewall-settings/


[adfwalkthrough ダウンロード]: http://go.microsoft.com/fwlink/?LinkId=517495
[開発者向けリファレンス]: http://go.microsoft.com/fwlink/?LinkId=516908

[DataSlicesBySliceTime]: ./media/data-factory-tutorial/DataSlicesBySliceTime.png
[イメージの作成者の展開-タイル]: ./media/data-factory-tutorial/author-deploy-tile.png
[イメージ エディター newdatastore ボタン]: ./media/data-factory-tutorial/editor-newdatastore-button.png
[イメージ エディター-blob のストレージ--json]: ./media/data-factory-tutorial/editor-blob-storage-json.png
[イメージ-エディター-blob の記憶域に展開します。]: ./media/data-factory-tutorial/editor-blob-storage-deploy.png

[image-data-factory-tutorial-end-to-end-flow]: ./media/data-factory-tutorial/EndToEndWorkflow.png

[image-data-factory-tutorial-partition-game-logs-pipeline]: ./media/data-factory-tutorial/PartitionGameLogsPipeline.png

[image-data-factory-tutorial-enrich-game-logs-pipeline]: ./media/data-factory-tutorial/EnrichGameLogsPipeline.png

[image-data-factory-tutorial-analyze-marketing-campaign-pipeline]: ./media/data-factory-tutorial/AnalyzeMarketingCampaignPipeline.png


[image-data-factory-tutorial-egress-to-onprem-pipeline]: ./media/data-factory-tutorial/EgreeDataToOnPremPipeline.png

[image-data-factory-tutorial-set-firewall-rules-azure-db]: ./media/data-factory-tutorial/SetFirewallRuleForAzureDatabase.png

[image-data-factory-tutorial-portal-new-everything]: ./media/data-factory-tutorial/PortalNewEverything.png

[image-data-factory-tutorial-datastorage-cache-backup]: ./media/data-factory-tutorial/DataStorageCacheBackup.png

[image-data-factory-tutorial-dataservices-blade]: ./media/data-factory-tutorial/DataServicesBlade.png

[image-data-factory-tutorial-new-datafactory-blade]: ./media/data-factory-tutorial/NewDataFactoryBlade.png

[image-data-factory-tutorial-resourcegroup-blade]: ./media/data-factory-tutorial/ResourceGroupBlade.png

[image-data-factory-tutorial-create-resourcegroup]: ./media/data-factory-tutorial/CreateResourceGroup.png

[image-data-factory-tutorial-datafactory-homepage]: ./media/data-factory-tutorial/DataFactoryHomePage.png

[image-data-factory-tutorial-create-datafactory]: ./media/data-factory-tutorial/CreateDataFactory.png

[image-data-factory-tutorial-linkedservice-tile]: ./media/data-factory-tutorial/LinkedServiceTile.png

[image-data-factory-tutorial-linkedservices-add-datstore]: ./media/data-factory-tutorial/LinkedServicesAddDataStore.png

[image-data-factory-tutorial-datastoretype-azurestorage]: ./media/data-factory-tutorial/DataStoreTypeAzureStorageAccount.png

[image-data-factory-tutorial-azurestorage-settings]: ./media/data-factory-tutorial/AzureStorageSettings.png

[image-data-factory-tutorial-storage-key]: ./media/data-factory-tutorial/StorageKeyFromAzurePortal.png

[image-data-factory-tutorial-linkedservices-blade-storage]: ./media/data-factory-tutorial/LinkedServicesBladeWithAzureStorage.png

[image-data-factory-tutorial-azuresql-settings]: ./media/data-factory-tutorial/AzureSQLDatabaseSettings.png

[image-data-factory-tutorial-azuresql-database-connection-string]: ./media/data-factory-tutorial/DatabaseConnectionString.png

[image-data-factory-tutorial-linkedservices-all]: ./media/data-factory-tutorial/LinkedServicesAll.png

[image-data-factory-tutorial-datasets-all]: ./media/data-factory-tutorial/DataSetsAllTables.png

[image-data-factory-tutorial-pipelines-all]: ./media/data-factory-tutorial/AllPipelines.png

[image-data-factory-tutorial-diagram-link]: ./media/data-factory-tutorial/DataFactoryDiagramLink.png

[image-data-factory-tutorial-diagram-view]: ./media/data-factory-tutorial/DiagramView.png

[イメージのデータの工場出荷時の監視-スタート画面]: ./media/data-factory-tutorial/MonitoringStartBoard.png

[image-data-factory-monitoring-hub-everything]: ./media/data-factory-tutorial/MonitoringHubEverything.png

[image-data-factory-monitoring-browse-datafactories]: ./media/data-factory-tutorial/MonitoringBrowseDataFactories.png

[image-data-factory-monitoring-pipeline-consumed-produced]: ./media/data-factory-tutorial/MonitoringPipelineConsumedProduced.png

[image-data-factory-monitoring-raw-game-events-table]: ./media/data-factory-tutorial/MonitoringRawGameEventsTable.png

[image-data-factory-monitoring-raw-game-events-table-dataslice-blade]: ./media/data-factory-tutorial/MonitoringPartitionGameEventsTableDataSliceBlade.png

[image-data-factory-monitoring-activity-run-details]: ./media/data-factory-tutorial/MonitoringActivityRunDetails.png

[image-data-factory-datamanagementgateway-configuration-manager]: ./media/data-factory-tutorial/DataManagementGatewayConfigurationManager.png

[image-data-factory-new-datafactory-menu]: ./media/data-factory-tutorial/NewDataFactoryMenu.png

[image-data-factory-new-datafactory-create-button]: ./media/data-factory-tutorial/DataFactoryCreateButton.png 
<!--HONumber=Apr16_HO2-->
