<properties 
    pageTitle="Azure Data Factory を使用してログ ファイルの移動と処理を行う (Azure PowerShell)" 
    description="この高度なチュートリアルでは、現実に近いシナリオについて説明し、そのシナリオを Azure Data Factory サービスと Azure PowerShell を使用して実装します。" 
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
    ms.date="10/29/2015" 
    ms.author="spelluru"/>

# チュートリアル: Data Factory を使用したログ ファイルの移動と処理 [PowerShell]
この記事では、Azure Data Factory を使用してデータをログ ファイルから洞察へと変化させながら、ログ処理の標準的なシナリオを包括的なチュートリアルとして提供します。

## シナリオ
Contoso は、ゲーム機、携帯デバイス、パーソナル コンピューター (PC) など、複数のプラットフォーム向けにゲームを製作するゲーム会社です。 これらのゲームはそれぞれが大量のログを産み出します。 Contoso 社の目標は、これらのゲームが産み出すログを収集解析することで有益な情報を手に入れ、アップセルやクロスセルの機会を見極め、新しい魅力的な機能などを開発する、つまりビジネスを向上させて顧客により良い体験を提供することです。
 
このチュートリアルでは、サンプル ログを収集、処理して参照データで強化し、このデータを変換して Contoso 社が最近開始したマーケティング キャンペーンの有効性を評価します。

## チュートリアルの準備をする
1.  読み取り [Azure Data Factory の概要][adfintroduction] に最上位レベルの概念を理解し、Azure Data Factory の概要について説明します。
2.  このチュートリアルを実施するには Azure サブスクリプションが必要です。 サブスクリプションの入手方法の詳細については、次を参照してください。 [購入に関するオプション] [azure-purchase-options], 、[メンバー プラン][azure-member-offers], 、または [無料評価版][azure-free-trial]します。
3.  ダウンロードしてインストールする必要があります [Azure PowerShell][download-azure-powershell] コンピューターにします。

    この記事では、すべての Data Factory コマンドレットを取り上げているわけではありません。 参照してください [Data Factory コマンドレット リファレンス](https://msdn.microsoft.com/library/dn820234.aspx) Data Factory コマンドレットに関する包括的なドキュメントについてです。
    
    Azure PowerShell を使用している場合 **バージョン < 1.0**, 、記載されているコマンドレットを使用する必要があります [ここ][old-cmdlet-reference]します。 また、Data Factory コマンドレットを使用する前に、次のコマンドを実行する必要があります。 

    1. 実行 **Add-azureaccount** ユーザー名と、Azure ポータルにサインインするために使用するパスワードを入力します。
    2. 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    3. 実行 **Select-azuresubscription** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。
    
    Azure PowerShell は、このチュートリアルが終わるまで開いたままにしておいてください。 Azure PowerShell を閉じて再度開いた場合は、これらのコマンドをもう一度実行する必要があります。

2. Azure Data Factory コマンドレットはこのモードで利用可能な AzureResourceManager モードに切り替えます。 **Switch-azuremode AzureResourceManager**します。
 
2.  **(推奨)** 確認し、練習をチュートリアルで、 [Azure Data Factory を使ってみる][adfgetstarted] ポータルとコマンドレットに慣れるための簡単なチュートリアルについての記事です。
3.  **(推奨)** 確認し、実際に、 [の Pig の使用と Azure Data Factory を使って Hive][usepigandhive] 内部設置型のデータ ソースからデータを Azure blob ストアに移動するパイプラインの作成のチュートリアルについての記事です。
4.  ダウンロード [ADFWalkthrough][adfwalkthrough-download] ファイルを **C:\ADFWalkthrough** フォルダー **フォルダー構造は維持**:
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
    - 実行 **ログイン AzureRmAccount** ユーザー名と、Azure ポータルにサインインに使用するパスワードを入力します。  
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
   
2. ローカル コンピューターに Azure SQL Database へのアクセス権があることを確認してください。 アクセスを有効にするには、 [Azure Classic Portal](http://manage.windowsazure.com) または **sp_set_firewall_rule** 、コンピューターの IP アドレスに対するファイアウォール規則を作成する master データベースにします。 この変更が有効になるまで最大で 5 分かかる場合があります。 参照してください [Azure SQL のファイアウォール ルールを設定する][azure-sql-firewall]です。
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

1. 切り替える **Azure PowerShell** していれば、既に開かれている (または) 起動 **Azure PowerShell**します。 Azure PowerShell を一度閉じてから再度開いた場合は、次のコマンドを実行する必要があります。 
    - 実行 **ログイン AzureRmAccount** ユーザー名と、Azure ポータルにサインインに使用するパスワードを入力します。  
    - 実行 **Get-azuresubscription** このアカウントのすべてのサブスクリプションを表示します。
    - 実行 **Select-azuresubscription** を使用するサブスクリプションを選択します。 このサブスクリプションは、Azure ポータルで使用したものと同じである必要があります。 

2. という名前の Azure リソース グループを作成する **ADFTutorialResourceGroup** (既に作成していない) 場合、次のコマンドを実行しています。

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    このチュートリアルの一部の手順は、ADFTutorialResourceGroup という名前のリソース グループを使用することを前提としています。 異なるリソース グループを使用する場合は、このチュートリアルで ADFTutorialResourceGroup の代わりにそのリソース グループを使用する必要があります。
4. 実行、 **新規 AzureRmDataFactory** コマンドレットを DataFactoryMyFirstPipelinePSH をという名前の data factory を作成します。  

        New-AzureRmDataFactory -ResourceGroupName ADFTutorialResourceGroup -Name LogProcessingFactory –Location "West US"

    > [AZURE.IMPORTANT] Azure Data Factory の名前はグローバルに一意である必要があります。 エラーが発生する場合 **データ ファクトリ名""LogProcessingFactory"は使用できません**, 、(例: yournameLogProcessingFactory) の名前を変更します。 このチュートリアルの手順の実行中に、この名前を LogProcessingFactory の代わりに使用します。 参照してください [Data Factory - 名前付け規則](data-factory-naming-rules.md) Data Factory アーティファクトの名前付け規則についてのトピックです。
    > 
    > データ ファクトリの名前は今後、DNS 名として登録される可能性があるため、一般ユーザーに表示される場合があります。

 
## <a name="MainStep3"></a>手順 3. リンクされたサービスを作成する

> [AZURE.NOTE] この記事では、Azure PowerShell を使用して、リンクされたサービス、テーブル、およびパイプラインを作成します。 参照してください [Data Factory エディターを使用してチュートリアル][adftutorial-using-editor] Azure ポータル、具体的には Data Factory エディターを使用してこのチュートリアルを実行するかどうか。 

この手順では、StorageLinkedService、AzureSqlLinkedService、HDInsightStorageLinkedService、HDInsightLinkedService というリンクされたサービスを作成します。

16. Azure PowerShell に移動、 **LinkedServices** サブフォルダーに **C:\ADFWalkthrough** フォルダー ファイルを展開した場所から (または)。
17. 以下のコマンドを使用して、データ ファクトリの名前に $df 変数を設定します。

        $df = “LogProcessingFactory”
17. 開いている **StorageLinkedService.json** 、好みのエディターで次のように入力します。 **アカウント名** と **アカウント キー** 数値を使用すると、ファイルを保存します。
17. コマンドレットを使用して **新規 AzureRmDataFactoryLinkedService** に次のようにリンクされたサービスを作成します。 

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADF -DataFactoryName $df -File .\StorageLinkedService.json
    
18. 開いている **StorageLinkedService.json** 、好みのエディターで次のように入力します。 **アカウント名** と **アカウント キー** 数値を使用すると、ファイルを保存します。
19. 作成、 **HDInsightStorageLinkedService**します。

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADF -DataFactoryName $df -File .\HDInsightStorageLinkedService.json
 
19. 開いている **AzureSqlLinkedService.json** 、好みのエディターで次のように入力します。 **azure の sql server** 名、 **ユーザー名** と **パスワード** 数値を使用すると、ファイルを保存します。
19. コマンドレットを使用して **新規 AzureRmDataFactoryLinkedService** に次のようにリンクされたサービスを作成します。 

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADF -DataFactoryName $df -File .\AzureSqlLinkedService.json
19. 開いている **HDInsightLinkedService.json** お気に入りのエディターに、型が設定されていることを確認で **HDInsightOnDemandLinkedService**します。

    Azure Data Factory サービスはオンデマンド クラスターの作成をサポートしており、このクラスターを使用して入力データの処理と出力データの生成を行います。 また、独自のクラスターを使って同じ処理を行うことも可能です。 オンデマンド HDInsight クラスターを使用する場合は、スライスごとにクラスターが作成されます。 一方、独自の HDInsight クラスターを使用する場合、そのクラスターはすぐにスライスを処理できる状態にあります。 そのため、オンデマンド クラスターを使用すると、独自のクラスターを使用するよりデータの出力が遅いと感じる場合があります。 試しに、オンデマンド クラスターを使用してみましょう。 
    
    HDInsightLinkedService は、オンデマンド HDInsight クラスターをデータ ファクトリにリンクします。 独自の HDInsight クラスターを使用するには、次に示すように HDInsightLinkedService.json ファイルの Properties セクションを更新します (クラスター名、ユーザー名、パスワードを適切な値に置き換えます)。 
    
        "Properties": 
        {
            "Type": "HDInsightBYOCLinkedService",
            "ClusterUri": "https://<clustername>.azurehdinsight.net/",
            "UserName": "<username>",
            "Password": "<password>",
            "LinkedServiceName": "HDInsightStorageLinkedService"
        }
        


19. コマンドレットを使用して **新規 AzureRmDataFactoryLinkedService** に次のようにリンクされたサービスを作成します。 ストレージ アカウントから始めます。

        New-AzureRmDataFactoryLinkedService -ResourceGroupName ADF -DataFactoryName $df -File .\HDInsightLinkedService.json
 
    ResourceGroupName、DataFactoryName または LinkedService の名前に別の名前を使用している場合は、上記コマンドレットでそれらの名前を参照します。 また、リンクされたサービスの JSON ファイルが見つからない場合は、そのファイルの完全なパスを入力します。

 

## <a name="MainStep4"></a>手順 4. テーブルを作成する 
この手順では、以下のテーブルを作成します。 

- RawGameEventsTable
- PartitionedGameEventsTable
- RefGeoCodeDictionaryTable
- RefMarketingCampaignTable
- EnrichedGameEventsTable
- MarketingCampaignEffectivenessSQLTable
- MarketingCampaignEffectivenessBlobTable

    ![チュートリアルの全体のフロー][image-data-factory-tutorial-end-to-end-flow]
 
上の図は、中央の列がパイプラインを、上と下の列がテーブルを表しています。 

Azure クラシック ポータルはデータ セットとテーブルの作成をまだサポートしていないため、このリリースでは Azure PowerShell を使用してテーブルを作成する必要があります。

### テーブルを作成するには

1.  Azure PowerShell に移動、 **テーブル** フォルダー (**C:\ADFWalkthrough\Tables\**) サンプルを展開した場所からです。 
2.  コマンドレットを使用して **新規 AzureRmDataFactoryDataset** の次のように、データセットを作成する **RawGameEventsTable**.json  


        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\RawGameEventsTable.json

    If you are using a different name for ResourceGroupName and DataFactoryName, refer them in the above cmdlet. Also, provide the full file path of the Table JSON file if the file cannot be found by the cmdlet.

3. 前の手順を繰り返して、以下のテーブルを作成します。 
        
        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\PartitionedGameEventsTable.json
        
        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\RefGeoCodeDictionaryTable.json
            
        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\RefMarketingCampaignTable.json
            
        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\EnrichedGameEventsTable.json
            
        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\MarketingCampaignEffectivenessSQLTable.json
            
        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\MarketingCampaignEffectivenessBlobTable.json



4.  **Azure ポータル**, 、] をクリックして **データセット** で、 **DATA FACTORY** ブレード **LogProcessingFactory** のすべてのデータセットを表示することを確認し、(テーブルは四角形のデータセットです)。 

    ![Data Sets All][image-data-factory-tutorial-datasets-all]

    また、Azure PowerShell から次のコマンドを使用することも可能です。
            
        Get-AzureRmDataFactoryDataset –ResourceGroupName ADF –DataFactoryName $df

    


## <a name="MainStep5"></a>手順 5. パイプラインを作成してスケジュールを設定する
この手順では、PartitionGameLogsPipeline、EnrichGameLogsPipeline、AnalyzeMarketingCampaignPipeline の各パイプラインを作成します。

1.  **Windows エクスプ ローラー**, に移動し、 **パイプライン** サブフォルダー **C:\ADFWalkthrough** フォルダー (またはサンプルを展開した場所から)。
2.  開いている **PartitionGameLogsPipeline.json** 、好みのエディターで置き換え、強調表示されているデータのストレージ アカウント情報のストレージ アカウントでファイルを保存します。
            
        "RAWINPUT": "wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/logs/rawgameevents/",
        "PARTITIONEDOUTPUT": "wasb://adfwalkthrough@<storageaccountname>.blob.core.windows.net/logs/partitionedgameevents/",

3. 手順を繰り返して、以下のパイプラインを作成します。
    1. **EnrichGameLogsPipeline**.json (出現回数 3 回)
    2. **AnalyzeMarketingCampaignPipeline**.json (出現回数 3 回)

    **重要:** 置き換えたことのすべての確認 <storageaccountname> ストレージ アカウント名。 
 
4.   **Azure PowerShell**, に移動し、 **パイプライン** サブフォルダー **C:\ADFWalkthrough** フォルダー (またはサンプルを展開した場所から)。
5.  コマンドレットを使用して **新規 AzureRmDataFactoryPipeline** の次のように、パイプラインを作成する **PartitionGameLogspeline**.json    
            
        New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName $df –File .\PartitionGameLogsPipeline.json

    ResourceGroupName、DataFactoryName、またはパイプライン名に別の名前を使用している場合は、上記コマンドレットでそれらの名前を参照します。 また、パイプラインの JSON ファイルのファイルの完全パスを提供します。
6. 前の手順を繰り返して、以下のパイプラインを作成します。
    1. **EnrichGameLogsPipeline**
            
            New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName $df –File .\EnrichGameLogsPipeline.json

    2. **AnalyzeMarketingCampaignPipeline**
                
            New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName $df –File .\AnalyzeMarketingCampaignPipeline.json

7. コマンドレットを使用して **Get AzureRmDataFactoryPipeline** パイプラインの一覧を取得します。
            
        Get-AzureRmDataFactoryPipeline –ResourceGroupName ADF –DataFactoryName $df

8. パイプラインを作成したら、データ処理を実行する期間を指定できます。 パイプラインの有効期間を指定することで、各 ADF テーブルに対して定義された Availability プロパティに基づいてデータ スライスが処理される期間を定義します。

パイプラインの有効期間を指定するために、Set-AzureRmDataFactoryPipelineActivePeriod コマンドレットが使用できます。 このチュートリアルでは、サンプル データは 05/01 ～ 05/05 のものです。 2014-05-01 を StartDateTime として使用します。 EndDateTime は任意です。
            
        Set-AzureRmDataFactoryPipelineActivePeriod -ResourceGroupName ADF -DataFactoryName $df -StartDateTime 2014-05-01Z -EndDateTime 2014-05-05Z –Name PartitionGameLogsPipeline
  
9. パイプラインの有効期間を設定することを確認します。
            
            Confirm
            Are you sure you want to set pipeline 'PartitionGameLogsPipeline' active period from '05/01/2014 00:00:00' to '05/05/2014 00:00:00'?
            [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): n

10. 前の 2 つの手順を繰り返して、以下のパイプラインの有効期間を設定します。
    1. **EnrichGameLogsPipeline**
            
            Set-AzureRmDataFactoryPipelineActivePeriod -ResourceGroupName ADF -DataFactoryName $df -StartDateTime 2014-05-01Z –EndDateTime 2014-05-05Z –Name EnrichGameLogsPipeline

    2. **AnalyzeMarketingCampaignPipeline** 
            
            Set-AzureRmDataFactoryPipelineActivePeriod -ResourceGroupName ADF -DataFactoryName $df -StartDateTime 2014-05-01Z -EndDateTime 2014-05-05Z –Name AnalyzeMarketingCampaignPipeline

11.  **Azure ポータル**, 、] をクリックして **パイプライン** タイル (パイプライン名) ではなく、 **DATA FACTORY** ブレード、 **LogProcessingFactory**, 、作成したパイプラインが表示されます。

    ![All Pipelines][image-data-factory-tutorial-pipelines-all]

12.  **DATA FACTORY** ブレード、 **LogProcessingFactory**, 、] をクリックして **ダイアグラム**します。

    ![ダイアグラム リンク][image-data-factory-tutorial-diagram-link]

13. 表示されているダイアグラムは並べ替えることが可能で、以下のダイアグラムは上部が直接の入力を示し、下部が出力を示しています。 あることがわかりますの出力、 **PartitionGameLogsPipeline** EnrichGameLogsPipeline の出力を入力として渡される、 **EnrichGameLogsPipeline** に渡される、 **AnalyzeMarketingCampaignPipeline**します。 タイトルをダブルクリックして、ブレードが示すアーティファクトについての詳細を表示します。

    ![Diagram View][image-data-factory-tutorial-diagram-view]

    **ご利用ありがとうございます。** Azure Data Factory、リンクされたサービス、パイプライン、テーブルを作成し、ワークフローを開始することに成功しました。 


## <a name="MainStep6"></a>手順 6. パイプラインとデータ スライスを監視する 

1.  [LogProcessingFactory] の [データ ファクトリ] ブレードを開いていない場合、以下のいずれかを実施できます。
    1.  クリックして **LogProcessingFactory** 上、 **スタート画面**します。 データ ファクトリを作成するときに、 **スタート画面に追加** オプションは自動的にチェックします。

        ![監視スタート画面][image-data-factory-monitoring-startboard]

    2. をクリックして **参照** ハブ、およびクリック **すべて**です。
        
        ![すべてのハブの監視][image-data-factory-monitoring-hub-everything]

         **参照** ブレードで、 **データ ファクトリ** 選択 **LogProcessingFactory** で、 **データ ファクトリ** ブレードです。

        ![データ ファクトリの監視][image-data-factory-monitoring-browse-datafactories]
2. いくつかの方法でデータ ファクトリが監視できます。 パイプラインまたはデータ セットで監視が始められます。 パイプラインで監視を始め、さらに詳しく学びましょう。 
3.  クリックして **パイプライン** 上、 **DATA FACTORY** ブレードです。 
4.  クリックして **PartitionGameLogsPipeline** パイプライン] ブレードでします。 
5.   **パイプライン** ブレード **PartitionGameLogsPipeline**, 、パイプラインを使用するを参照してください **RawGameEventsTable** データセット。  クリックして **RawGameEventsTable**します。

    ![パイプラインでの使用と生成][image-data-factory-monitoring-pipeline-consumed-produced]

6. [テーブル] ブレードに **RawGameEventsTable**, 、すべてのスライスを参照してください。 次のスクリーン ショットは、すべてのスライスが **準備ができて** 状態および問題のあるスライスはありません。 これは、そのデータがすぐに処理できることを意味します。 

    ![[RawGameEventsTable] の [テーブル] ブレード][image-data-factory-monitoring-raw-game-events-table]
 
7. 、 **パイプライン** ブレード **PartiionGameLogsPipeline**, 、クリックして **Produced**します。 
8. このパイプラインが生成するデータ セットの一覧が表示されるはずです。 
9. クリックして **PartitionedGameEvents** テーブルに、 **データセットを生成** ブレードです。 
10. いることを確認、 **ステータス** のすべてのスライスに設定されている **準備**します。 
11. になっているスライスのいずれかをクリックして **準備** を表示する、 **データ スライス** そのスライスのブレードです。

    ![[RawGameEventsTable] の [データ スライス] ブレード][image-data-factory-monitoring-raw-game-events-table-dataslice-blade]

    エラーが発生していた場合、ここに **[失敗]** という状態が表示されます。  か両方のスライスの状態を表示することもあります **準備**, 、またはその両方の状態 **PendingValidation**, のスライスの処理速度に応じて、します。
 
    参照してください、 [Azure Data Factory 開発者向けリファレンス][developer-reference] すべての可能なスライスの状態を把握します。

12.  **データ スライス** ブレードから実行] をクリックして、 **アクティビティの実行** ] ボックスの一覧です。 そのスライスの [アクティビティの実行] ブレードが表示されるはずです。 次を参照する必要があります **アクティビティの実行の詳細** ブレードです。

    ![[アクティビティの実行の詳細] ブレード][image-data-factory-monitoring-activity-run-details]

13. クリックして **ダウンロード** ファイルをダウンロードします。 この画面は、HDInsight の処理で発生するエラーのトラブルシューティングを行うときに、特に役立ちます。 
     
    
すべてのパイプラインには、実行が完了した場合に検討することができます、 **MarketingCampaignEffectivenessTable** で、 **MarketingCampaigns** Azure SQL データベースが結果を表示します。 

**ご利用ありがとうございます。** これでワークフローの監視とトラブルシューティングができます。 Azure Data Factory を使用してデータを処理し分析結果を得る方法を学びました。

## チュートリアルをさらに進めてオンプレミス データを使用する
この記事のチュートリアルにあるログ処理のシナリオの最後の手順で、マーケティング キャンペーンの有効性の出力が Azure SQL Database にコピーされました。 分析のため、所属する組織内にあるオンプレミスの SQL Server にこのデータを移動することも可能です。
 
マーケティング キャンペーンの有効性データを Azure BLOB からオンプレミスの SQL Server にコピーするには、この記事のチュートリアルで導入したオンプレミスのリンクされたサービス、テーブル、パイプラインを追加で作成する必要があります。

実習、 [チュートリアル: を使用して内部設置型データ ソース][tutorial-onpremises-using-powershell] を内部設置型 SQL Server データベースにマーケティング キャンペーン有効性データをコピーするパイプラインを作成する方法を参照してください。
 

[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[adftutorial-using-editor]: data-factory-tutorial.md

[adfgetstarted]: data-factory-get-started.md
[adfintroduction]: data-factory-introduction.md
[usepigandhive]: data-factory-data-transformation-activities.md
[tutorial-onpremises-using-powershell]: data-factory-tutorial-extend-onpremises-using-powershell.md
[download-azure-powershell]: ../powershell-install-configure.md

[azure-portal]: http://portal.azure.com
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[sqlcmd-install]: http://www.microsoft.com/download/details.aspx?id=35580
[azure-sql-firewall]: http://msdn.microsoft.com/library/azure/jj553530.aspx


[adfwalkthrough-download]: http://go.microsoft.com/fwlink/?LinkId=517495
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx


[image-data-factory-tutorial-end-to-end-flow]: ./media/data-factory-tutorial-using-powershell/EndToEndWorkflow.png

[image-data-factory-tutorial-partition-game-logs-pipeline]: ./media/data-factory-tutorial-using-powershell/PartitionGameLogsPipeline.png

[image-data-factory-tutorial-enrich-game-logs-pipeline]: ./media/data-factory-tutorial-using-powershell/EnrichGameLogsPipeline.png

[image-data-factory-tutorial-analyze-marketing-campaign-pipeline]: ./media/data-factory-tutorial-using-powershell/AnalyzeMarketingCampaignPipeline.png


[image-data-factory-tutorial-egress-to-onprem-pipeline]: ./media/data-factory-tutorial-using-powershell/EgreeDataToOnPremPipeline.png

[image-data-factory-tutorial-set-firewall-rules-azure-db]: ./media/data-factory-tutorial-using-powershell/SetFirewallRuleForAzureDatabase.png

[image-data-factory-tutorial-portal-new-everything]: ./media/data-factory-tutorial-using-powershell/PortalNewEverything.png

[image-data-factory-tutorial-datastorage-cache-backup]: ./media/data-factory-tutorial-using-powershell/DataStorageCacheBackup.png

[image-data-factory-tutorial-dataservices-blade]: ./media/data-factory-tutorial-using-powershell/DataServicesBlade.png

[image-data-factory-tutorial-new-datafactory-blade]: ./media/data-factory-tutorial-using-powershell/NewDataFactoryBlade.png

[image-data-factory-tutorial-resourcegroup-blade]: ./media/data-factory-tutorial-using-powershell/ResourceGroupBlade.png

[image-data-factory-tutorial-create-resourcegroup]: ./media/data-factory-tutorial-using-powershell/CreateResourceGroup.png

[image-data-factory-tutorial-datafactory-homepage]: ./media/data-factory-tutorial-using-powershell/DataFactoryHomePage.png

[image-data-factory-tutorial-create-datafactory]: ./media/data-factory-tutorial-using-powershell/CreateDataFactory.png

[image-data-factory-tutorial-linkedservice-tile]: ./media/data-factory-tutorial-using-powershell/LinkedServiceTile.png

[image-data-factory-tutorial-linkedservices-add-datstore]: ./media/data-factory-tutorial-using-powershell/LinkedServicesAddDataStore.png

[image-data-factory-tutorial-datastoretype-azurestorage]: ./media/data-factory-tutorial-using-powershell/DataStoreTypeAzureStorageAccount.png

[image-data-factory-tutorial-azurestorage-settings]: ./media/data-factory-tutorial-using-powershell/AzureStorageSettings.png

[image-data-factory-tutorial-storage-key]: ./media/data-factory-tutorial-using-powershell/StorageKeyFromAzurePortal.png

[image-data-factory-tutorial-linkedservices-blade-storage]: ./media/data-factory-tutorial-using-powershell/LinkedServicesBladeWithAzureStorage.png

[image-data-factory-tutorial-azuresql-settings]: ./media/data-factory-tutorial-using-powershell/AzureSQLDatabaseSettings.png

[image-data-factory-tutorial-azuresql-database-connection-string]: ./media/data-factory-tutorial-using-powershell/DatabaseConnectionString.png

[image-data-factory-tutorial-linkedservices-all]: ./media/data-factory-tutorial-using-powershell/LinkedServicesAll.png

[image-data-factory-tutorial-datasets-all]: ./media/data-factory-tutorial-using-powershell/DataSetsAllTables.png

[image-data-factory-tutorial-pipelines-all]: ./media/data-factory-tutorial-using-powershell/AllPipelines.png

[image-data-factory-tutorial-diagram-link]: ./media/data-factory-tutorial-using-powershell/DataFactoryDiagramLink.png

[image-data-factory-tutorial-diagram-view]: ./media/data-factory-tutorial-using-powershell/DiagramView.png

[image-data-factory-monitoring-startboard]: ./media/data-factory-tutorial-using-powershell/MonitoringStartBoard.png

[image-data-factory-monitoring-hub-everything]: ./media/data-factory-tutorial-using-powershell/MonitoringHubEverything.png

[image-data-factory-monitoring-browse-datafactories]: ./media/data-factory-tutorial-using-powershell/MonitoringBrowseDataFactories.png

[image-data-factory-monitoring-pipeline-consumed-produced]: ./media/data-factory-tutorial-using-powershell/MonitoringPipelineConsumedProduced.png

[image-data-factory-monitoring-raw-game-events-table]: ./media/data-factory-tutorial-using-powershell/MonitoringRawGameEventsTable.png

[image-data-factory-monitoring-raw-game-events-table-dataslice-blade]: ./media/data-factory-tutorial-using-powershell/MonitoringPartitionGameEventsTableDataSliceBlade.png

[image-data-factory-monitoring-activity-run-details]: ./media/data-factory-tutorial-using-powershell/MonitoringActivityRunDetails.png

[image-data-factory-datamanagementgateway-configuration-manager]: ./media/data-factory-tutorial-using-powershell/DataManagementGatewayConfigurationManager.png

[image-data-factory-new-datafactory-menu]: ./media/data-factory-tutorial-using-powershell/NewDataFactoryMenu.png

[image-data-factory-new-datafactory-create-button]: ./media/data-factory-tutorial-using-powershell/DataFactoryCreateButton.png 

