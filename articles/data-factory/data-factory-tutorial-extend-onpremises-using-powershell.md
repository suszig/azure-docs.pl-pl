<properties 
    pageTitle="出力データをオンプレミスの SQL Server データベースにコピーする (Azure PowerShell)" 
    description="ここでは、Azure PowerShell を使用してチュートリアルを拡張して、パイプラインで出力データを SQL Server データベースにコピーするようにします。"
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


# チュートリアル: オンプレミスの SQL Server データベースへのキャンペーンの有効性のデータのコピー
このチュートリアルでは、パイプラインがオンプレミスのデータを扱えるようにするため、その環境を設定する方法を学びます。
 
パーティション -> 強化 -> 分析のワークフローを含む最初のチュートリアルにおいて、ログ処理のシナリオの最後の手順で、マーケティング キャンペーンの有効性のアウトプットが Azure SQL データベースにコピーされました。 分析のため、所属する組織内にあるオンプレミスの SQL Server にこのデータを移動することも可能です。
 
マーケティング キャンペーンの有効性データを Azure BLOB からオンプレミスの SQL Server にコピーするため、最初のチュートリアルで導入したものと同じコマンドレット一式を使って、オンプレミスのリンクされたサービス、テーブル、およびパイプラインを追加で作成する必要があります。

 **必要があります** でチュートリアルを実行、 [チュートリアル: 移動し、Data Factory を使用してログ ファイルの処理を][datafactorytutorial] この記事のチュートリアルを実行する前にします。 

**(推奨)** 確認し、実際に、 [パイプライン内部設置型データを扱えるようにする][useonpremisesdatasources] からデータを移動するパイプラインの作成に関するチュートリアルについての記事内部設置型 SQL Server は Azure blob ストアにします。


このチュートリアルでは、次の手順に従います。 

1. [手順 1: data management gateway を作成する](#OnPremStep1)です。 Data Management Gateway は、クラウドから、組織内の内部設置型データ ソースへのアクセスを提供するためのクライアント エージェントです。 このゲートウェイによって、オンプレミスの SQL Server と Azure データ ストアの間でデータ転送が可能になります。  

    オンプレミスの SQL Server データベースをリンクされたサービスとして Azure データ ファクトリに追加する前に、企業環境内に少なくとも 1 つのゲートウェイがインストールされており、それが Azure Data Factory に登録されている必要があります。

2. [手順 2: 内部設置型 SQL Server 用のリンクされたサービスを作成する](#OnPremStep2)です。 ここでは、まず内部設置型 SQL Server コンピューターに、データベースとテーブルを作成し、リンクされたサービスを作成します。 **OnPremSqlLinkedService**します。  
3. [手順 3: テーブルとパイプラインの作成](#OnPremStep3)します。 この手順では、テーブルを作成します **MarketingCampaignEffectivenessOnPremSQLTable** とパイプライン **EgressDataToOnPremPipeline**します。 

4. [手順 4: パイプラインを監視して結果を確認](#OnPremStep4)します。 この手順では、Azure クラシック ポータルを使用して、パイプライン、テーブル、データ スライスを監視します。


## <a name="OnPremStep1"></a>手順 1: Data Management Gateway を作成する

Data Management Gateway は、所属する組織内のオンプレミスのデータ ソースに、クラウドからのアクセスを提供するクライアント エージェントです。 このゲートウェイによって、オンプレミスの SQL Server と Azure データ ストアの間でデータ転送が可能になります。
  
オンプレミスの SQL Server データベースをリンクされたサービスとして Azure データ ファクトリに追加する前に、企業環境内に少なくとも 1 つのゲートウェイがインストールされており、それが Azure Data Factory に登録されている必要があります。

すでに使用できるデータ ゲートウェイがある場合は、この手順をスキップします。

1.  論理データ ゲートウェイを作成します。  **Azure ポータル**, 、] をクリックして **リンクされたサービス** 上、 **DATA FACTORY** ブレードです。
2.  クリックして **[+ データ ゲートウェイ** コマンド バーでします。  
3.   **新しいデータ ゲートウェイ** ブレードで、をクリックして **作成**します。
4.   **作成** ブレードで、入力 **MyGateway** データ ゲートウェイの **名前**します。
5.  クリックして **地域を選択** し、必要に応じて変更します。 
6.  をクリックして **OK** で、 **作成** ブレードです。 
7.  確認する必要があります、 **構成** ブレードです。 
8.   **構成** ブレードで、をクリックして **このコンピューターに直接インストール**します。 これによりゲートウェイのダウンロード、インストール、およびコンピューター上での構成が行われ、サービスに登録されます。 すでにコンピューターにインストール済みのゲートウェイがあり、それをポータル上に作成したこの論理ゲートウェイにリンクさせたい場合、このブレード上のキーを使用して Data Management Gateway Manager (プレビュー) ツールでゲートウェイの再登録をします。

    ![Data Management Gateway 構成マネージャー][image-data-factory-datamanagementgateway-configuration-manager]

9. をクリックして **[ok]** を閉じる、 **構成** ブレードと **[ok]** を閉じる、 **作成** ブレードです。 状態まで **MyGateway** で、 **リンクされたサービス** ブレードに変更 **良い**です。 起動することも **Data Management Gateway 構成マネージャー (プレビュー)** ゲートウェイの名前がポータル上の名前と一致していることを確認するツールと **ステータス** は **登録されている**します。 最新の状態を確認するため、場合によっては [リンクされたサービス] ブレードをいったん閉じて再度開く必要があります。 画面が最新の状態に更新されるまで、数分かかる場合があります。 

## <a name="OnPremStep2"></a>手順 2: オンプレミスの SQL Server 用にリンクされたサービスを作成する。

この手順では、まずオンプレミスの SQL Server コンピューター上にデータベースとテーブルを作成し、その後リンクされたサービスを作成します。

### オンプレミスのデータベースとテーブルを準備する

最初に、SQL Server データベース、テーブル、ユーザー定義型、およびストアド プロシージャを作成する必要があります。 これらの移動に使用する、 **MarketingCampaignEffectiveness** 結果 blob Azure から SQL Server データベースにします。

1.   **Windows エクスプ ローラー**, に移動し、 **OnPremises** サブフォルダー **C:\ADFWalkthrough** (またはサンプルを展開した場所)。
2.  開いている **します (& a) Table.ps1** 、好みのエディターで、強調表示部分を SQL Server 情報に置き換え、ファイルを保存 (を指定してください **SQL 認証** 詳細)。 このチュートリアルのために、データベースの SQL 認証を有効にします。 
            
        $dbServerName = "<servername>"
        $dbUserName = "<username>"
        $dbPassword = "<password>"

3.  **Azure PowerShell**, に移動 **C:\ADFWalkthrough\OnPremises** フォルダーです。
4.  実行 **します (& a) Table.ps1** **(いずれかと二重引用符で囲まれた、または次のように)**します。
            
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

1.   **Azure ポータル**, 、] をクリックして **リンクされたサービス** タイルを **DATA FACTORY** ブレード **LogProcessingFactory**します。
2.   **リンクされたサービス** ブレードで、をクリックして **[+ データ ストア**します。
3.   **新しいデータ ストア** ブレードで、入力 **OnPremSqlLinkedService** の **名前**します。 
4.  クリックして **種類 (設定が必要)** 選択 **SQL Server**です。 表示されます、 **DATA GATEWAY**, 、**サーバー**, 、**データベース**, 、および **資格情報** の設定、 **新しいデータ ストア** ブレードようになりました。 
5.  をクリックして **データ ゲートウェイ (必要な設定を構成する)** 選択 **MyGateway** 以前に作成しました。 
6.  入力 **名** をホストするデータベース サーバーの **MarketingCampaigns** データベースです。 
7.  入力 **MarketingCampaigns** データベース用です。 
8.  クリックして **資格情報**します。 
9.   **資格情報** ブレードで、をクリックして **ここをクリックして、安全に資格情報を設定する**です。
10. これによりワンクリック アプリケーションが初めてインストールされ、**[資格情報の設定]** ダイアログ ボックスが起動します。 
11.  **資格情報の設定** ] ダイアログ ボックスに、入力 **ユーザー名** と **パスワード**, 、] をクリック **OK**します。 ダイアログ ボックスが閉じるまで待ちます。 
12. をクリックして **OK** で、 **新しいデータ ストア** ブレードです。 
13.  **リンクされたサービス** ブレードで、いることを確認 **OnPremSqlLinkedService** 、一覧に表示され、 **ステータス** は、リンクされたサービスの **適切な**です。

## <a name="OnPremStep3"></a>手順 3: テーブルとパイプラインを作成する

### オンプレミスの論理テーブルを作成する

1.   **Azure PowerShell**, に切り替えて、 **C:\ADFWalkthrough\OnPremises** フォルダーです。 
2.  コマンドレットを使用して **新規 AzureRmDataFactoryDataset** の次のように、テーブルを作成する **MarketingCampaignEffectivenessOnPremSQLTable.json**します。

            
        New-AzureRmDataFactoryDataset -ResourceGroupName ADF -DataFactoryName $df –File .\MarketingCampaignEffectivenessOnPremSQLTable.json
     
#### パイプラインを作成して Azure BLOB から SQL Server へデータをコピーする

1.  コマンドレットを使用して **新規 AzureRmDataFactoryPipeline** の次のように、パイプラインを作成する **EgressDataToOnPremPipeline.json**します。

            
        New-AzureRmDataFactoryPipeline -ResourceGroupName ADF -DataFactoryName $df –File .\EgressDataToOnPremPipeline.json
     
2. コマンドレットを使用して **セット AzureRmDataFactoryPipelineActivePeriod** の有効期間を指定する **EgressDataToOnPremPipeline**します。

            
        Set-AzureRmDataFactoryPipelineActivePeriod -ResourceGroupName ADF -DataFactoryName $df -StartDateTime 2014-05-01Z -EndDateTime 2014-05-05Z –Name EgressDataToOnPremPipeline

    Press **‘Y’** to continue.
    
## <a name="OnPremStep4"></a>手順 4: パイプラインを監視して結果を確認する

導入された同じ手順を使って、 [手順 6: テーブルとパイプラインを監視](#MainStep6)  新しいパイプラインと、新しい内部設置型の ADF テーブル用のデータ スライスを監視します。
 
テーブルのスライスの状態が表示されている場合 **MarketingCampaignEffectivenessOnPremSQLTable** 意味は準備完了に、パイプラインがスライスの実行を完了しています。 結果を表示するには、クエリ、 **MarketingCampaignEffectiveness** テーブルに **MarketingCampaigns** SQL Server のデータベースです。
 
ご利用ありがとうございます。 オンプレミスのデータ ソースを使用するためのチュートリアルを無事に終了しました。 
 

[monitor-manage-using-powershell]: data-factory-monitor-manage-using-powershell.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456

[datafactorytutorial]: data-factory-tutorial-using-powershell.md
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
[old-cmdlet-reference]: https://msdn.microsoft.com/library/azure/dn820234(v=azure.98).aspx


[image-data-factory-datamanagementgateway-configuration-manager]: ./media/data-factory-tutorial-extend-onpremises-using-powershell/DataManagementGatewayConfigurationManager.png

 

