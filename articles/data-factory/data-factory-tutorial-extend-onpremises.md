<properties 
    pageTitle="出力データをオンプレミスの SQL Server データベースにコピーする (Azure クラシック ポータル)" 
    description="ここでは、Azure クラシック ポータルで Data Factory Editor を使用してチュートリアルを拡張して、パイプラインで出力データを SQL Server データベースにコピーするようにします。"
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


# チュートリアル: オンプレミスの SQL Server データベースへのキャンペーンの有効性のデータのコピー 
このチュートリアルでは、パイプラインがオンプレミスのデータを扱えるようにするため、その環境を設定する方法を学びます。
 
パーティション -> 強化 -> 分析のワークフローを含む最初のチュートリアルにおいて、ログ処理のシナリオの最後の手順で、マーケティング キャンペーンの有効性のアウトプットが Azure SQL データベースにコピーされました。 分析のため、所属する組織内にあるオンプレミスの SQL Server にこのデータを移動することも可能です。
 
マーケティング キャンペーンの有効性データを Azure BLOB からオンプレミスの SQL Server にコピーするため、最初のチュートリアルで導入したものと同じコマンドレット一式を使って、オンプレミスのリンクされたサービス、テーブル、およびパイプラインを追加で作成する必要があります。

## 前提条件

 **必要があります** でチュートリアルを実行、 [チュートリアル: 移動し、Data Factory を使用してログ ファイルの処理を][datafactorytutorial] この記事のチュートリアルを実行する前にします。 

**(推奨)** 確認し、実際に、 [パイプライン内部設置型データを扱えるようにする][useonpremisesdatasources] からデータを移動するパイプラインの作成に関するチュートリアルについての記事内部設置型 SQL Server は Azure blob ストアにします。


このチュートリアルでは、次の手順に従います。 

1. [手順 1: data management gateway を作成する](#OnPremStep1)です。 Data Management Gateway は、クラウドから、組織内の内部設置型データ ソースへのアクセスを提供するためのクライアント エージェントです。 このゲートウェイによって、オンプレミスの SQL Server と Azure データ ストアの間でデータ転送が可能になります。  

    オンプレミスの SQL Server データベースをリンクされたサービスとして Azure データ ファクトリに追加する前に、企業環境内に少なくとも 1 つのゲートウェイがインストールされており、それが Azure Data Factory に登録されている必要があります。

2. [手順 2: 内部設置型 SQL Server 用のリンクされたサービスを作成する](#OnPremStep2)です。 ここでは、まず内部設置型 SQL Server コンピューターに、データベースとテーブルを作成し、リンクされたサービスを作成します。 **OnPremSqlLinkedService**します。  
3. [手順 3: テーブルとパイプラインの作成](#OnPremStep3)します。 この手順では、テーブルを作成します **MarketingCampaignEffectivenessOnPremSQLTable** とパイプライン **EgressDataToOnPremPipeline**します。 

4. [手順 4: パイプラインを監視して結果を確認](#OnPremStep4)します。 この手順では、Azure ポータルを使用して、パイプライン、テーブル、およびデータ スライスを監視します。


## <a name="OnPremStep1"></a> 手順 1: Data Management Gateway を作成します。

Data Management Gateway は、所属する組織内のオンプレミスのデータ ソースに、クラウドからのアクセスを提供するクライアント エージェントです。 このゲートウェイによって、オンプレミスの SQL Server と Azure データ ストアの間でデータ転送が可能になります。
  
オンプレミスの SQL Server データベースをリンクされたサービスとして Azure データ ファクトリに追加する前に、企業環境内に少なくとも 1 つのゲートウェイがインストールされており、それが Azure Data Factory に登録されている必要があります。

すでに使用できるデータ ゲートウェイがある場合は、この手順をスキップします。

1.  論理データ ゲートウェイを作成します。  **Azure ポータル**, 、] をクリックして **リンクされたサービス** 上、 **DATA FACTORY** data factory のブレードです。
2.  クリックして **[+ データ ゲートウェイ** コマンド バーでします。  
3.   **新しいデータ ゲートウェイ** ブレードで、をクリックして **作成**します。
4.   **作成** ブレードで、入力 **MyGateway** データ ゲートウェイの **名前**します。
5.  クリックして **地域を選択** し、必要に応じて変更します。 
6.  をクリックして **OK** で、 **作成** ブレードです。 
7.  確認する必要があります、 **構成** ブレードです。 
8.   **構成** ブレードで、をクリックして **このコンピューターに直接インストール**します。 これによりゲートウェイのダウンロード、インストール、およびコンピューター上での構成が行われ、サービスに登録されます。 すでにコンピューターにインストール済みのゲートウェイがあり、それをポータル上に作成したこの論理ゲートウェイにリンクさせたい場合、このブレード上のキーを使用して Data Management Gateway Manager (プレビュー) ツールでゲートウェイの再登録をします。

    ![Data Management Gateway 構成マネージャー][image-data-factory-datamanagementgateway-configuration-manager]

9. をクリックして **[ok]** を閉じる、 **構成** ブレードと **[ok]** を閉じる、 **作成** ブレードです。 状態まで **MyGateway** で、 **リンクされたサービス** ブレードに変更 **良い**です。 起動することも **Data Management Gateway 構成マネージャー (プレビュー)** ゲートウェイの名前がポータル上の名前と一致していることを確認するツールと **ステータス** は **登録されている**します。 最新の状態を確認するため、場合によっては [リンクされたサービス] ブレードをいったん閉じて再度開く必要があります。 画面が最新の状態に更新されるまで、数分かかる場合があります。 

## <a name="OnPremStep2"></a> 手順 2: 内部設置型 SQL Server 用のリンクされたサービスを作成します。

この手順では、まずオンプレミスの SQL Server コンピューター上にデータベースとテーブルを作成し、その後リンクされたサービスを作成します。

### オンプレミスのデータベースとテーブルを準備する

最初に、SQL Server データベース、テーブル、ユーザー定義型、およびストアド プロシージャを作成する必要があります。 これらの移動に使用する、 **MarketingCampaignEffectiveness** 結果 blob Azure から SQL Server データベースにします。

1.   **Windows エクスプ ローラー**, に移動し、 **OnPremises** サブフォルダー **C:\ADFWalkthrough** (またはサンプルを展開した場所)。
2.  開いている **します (& a) 二重** 、好みのエディターで、強調表示部分を SQL Server 情報に置き換え、ファイルを保存 (を指定してください **SQL 認証** 詳細)。 このチュートリアルのために、データベースの SQL 認証を有効にします。 
            
        $dbServerName = "<servername>"
        $dbUserName = "<username>"
        $dbPassword = "<password>"

3.  **Azure PowerShell**, に移動 **C:\ADFWalkthrough\OnPremises** フォルダーです。
4.  実行 **します (& a) 二重** **(いずれかと二重引用符で囲まれた、または次のように)**します。
            
        & '.\prepareOnPremDatabase&Table.ps1'

5. スクリプトが正常に実行されると、以下が表示されます。   
            
        PS E:\ADF\ADFWalkthrough\OnPremises> & '.\prepareOnPremDatabase&Table.ps1'
        6/10/2014 10:12:33 PM Script to create sample on-premises SQL Server Database and Table
        6/10/2014 10:12:33 PM Creating the database [MarketingCampaigns], table and stored procedure on [.]...
        6/10/2014 10:12:33 PM Connecting as user [sa]
        6/10/2014 10:12:33 PM Summary:
        6/10/2014 10:12:33 PM 1. Database 'MarketingCampaigns' created.
        6/10/2014 10:12:33 PM 2. 'MarketingCampaignEffectiveness' table and stored procedure 


### リンクされたサービスの作成

1.   **Azure ポータル**, 、] をクリックして **作成者とデプロイ** タイルを **DATA FACTORY** ブレード **LogProcessingFactory**します。
2.   **Data Factory エディター**, 、クリックして **新しいデータ ストア** 選択し、ツールバー **内部設置型 SQL Server データベース**します。
3.  JSON スクリプトで、次の手順に従います。 
    1.  置換 **<servername>** 、SQL Server データベースをホストするサーバーの名前に置き換えます。
    2.  置換 **<databasename>** と **MarketingCampaigns**します。
    3.  使用している場合は、 **SQL 認証**
        1.  指定 **<username>** と **<password>** で、 **connectionString**します。
        2.  最後の 2 つの行を削除する (**username** と **パスワード** JSON のプロパティが必要なは、Windows 認証を使用している場合のみ)。 
        3.  削除 * *、(コンマ) * * の最後に **gatewayName** 行です。
        
        **Windows 認証を使用する: 場合**
        1. 値を設定 **統合セキュリティ** に **True** で、 **connectionString**します。 削除"**ユーザー ID =<username>です。パスワード =<password>;**"から接続文字列。 
        2. データベースに対するアクセス権を持つユーザーの名前を指定、 **username** プロパティです。 
        3. 指定 **パスワード** のユーザー アカウントです。   
    4. ゲートウェイの名前を指定 (**MyGateway**) gatewayName プロパティにします。             
3.  をクリックして **展開** 、ツールバーのリンク サービスをデプロイします。 

## <a name="OnPremStep3"></a> 手順 3: テーブルとパイプラインを作成します。

### オンプレミスの論理テーブルを作成する

1.   **Data Factory エディター**, 、クリックして **新しいデータ セット** ツールバー、および選択から **内部設置型 SQL**します。 
2. 右側のウィンドウの JSON スクリプトを置き換えます、 **MarketingCampaignEffectivenessOnPremSQLTable.json** ファイルから、 **C:\ADFWalkthrough\OnPremises** フォルダーです。
3. リンクされたサービスの名前を変更 (**linkedServiceName** プロパティ) から **OnPremSqlServerLinkedService** に  **SqlServerLinkedService**します。
4. クリックして **展開** 、ツールバーのテーブルをデプロイします。 
     
#### パイプラインを作成して Azure BLOB から SQL Server へデータをコピーする

1.  1.  **Data Factory エディター**, をクリックして **新しいパイプライン** ツールバーのボタンをクリックします。 ボタンが表示されない場合は、ツール バーの **[...](省略記号)** をクリックします。 またを右クリックして **パイプライン** ツリー ビュー] をクリック **新しいパイプライン**します。
2. 右側のウィンドウの JSON スクリプトを置き換えます、 **EgressDataToOnPremPipeline.json** ファイルから、 **C:\ADFWalkthrough\OnPremises** フォルダーです。
3. 追加、 **コンマ (',')** の最後に **閉じ角かっこ ('] ')** JSON にし、その閉じ角かっこの後に次の 3 行を追加します。 

        "start": "2014-05-01T00:00:00Z",
        "end": "2014-05-05T00:00:00Z",
        "isPaused": false

    注意してください、 **開始** と **エンド** このチュートリアルのサンプル データが 2014 年 5 月 05/05/01/2014年からあるために、時刻が 2014 年 5 月 01 および 2014 年 5/05/に設定されます。 
 
3. クリックして **展開** 、ツールバーのパイプラインを作成してデプロイします。 表示されていることを確認、 **パイプラインは正常に作成された** エディターのタイトル バーにメッセージです。
    
## <a name="OnPremStep4"></a> 手順 4: パイプラインを監視して結果を確認

導入された同じ手順を使用できます、 **パイプラインとデータ スライスを監視する** のセクションで、 [メインのチュートリアル][datafactorytutorial] 新しいパイプラインと、新しい内部設置型の ADF テーブル用のデータ スライスを監視します。
 
テーブルのスライスの状態が表示されている場合 **MarketingCampaignEffectivenessOnPremSQLTable** 意味は準備完了に、パイプラインがスライスの実行を完了しています。 結果を表示するには、クエリ、 **MarketingCampaignEffectiveness** テーブルに **MarketingCampaigns** SQL Server のデータベースです。
 
ご利用ありがとうございます。 オンプレミスのデータ ソースを使用するためのチュートリアルを無事に終了しました。 
 

[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[datafactorytutorial]: data-factory-tutorial.md
[adfgetstarted]: data-factory-get-started.md
[adfintroduction]: data-factory-introduction.md
[useonpremisesdatasources]: data-factory-move-data-between-onprem-and-cloud.md

[azure-portal]: http://portal.azure.com
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[sqlcmd-install]: http://www.microsoft.com/download/details.aspx?id=35580
[azure-sql-firewall]: http://msdn.microsoft.com/library/azure/jj553530.aspx

[download-azure-powershell]: http://azure.microsoft.com/documentation/articles/install-configure-powershell
[adfwalkthrough-download]: http://go.microsoft.com/fwlink/?LinkId=517495
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908

[image-data-factory-datamanagementgateway-configuration-manager]: ./media/data-factory-tutorial-extend-onpremises/DataManagementGatewayConfigurationManager.png

 
