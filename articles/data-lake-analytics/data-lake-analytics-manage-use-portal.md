<properties 
   pageTitle="Azure ポータルを使用する Azure Data Lake Analytics の管理 | Azure" 
   description="Data Lake Analytics のアカウント、データ ソース、ユーザー、およびジョブの管理方法について説明します。" 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="paulettm" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/27/2015"
   ms.author="jgao"/>

# Azure ポータルを使用する Azure Data Lake Analytics の管理

[AZURE.INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Azure ポータルを使用して、Azure Data Lake Analytics のアカウント、データ ソース、ユーザー、およびジョブを管理する方法について説明します。 他のツールを使用する管理のトピックを表示する場合は、上のタブ セレクターをクリックします。

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。




<!-- ################################ -->
<!-- ################################ -->
## アカウントの管理

Data Lake Analytics ジョブを実行するには、Data Lake Analytics アカウントが必要です。 Azure HDInsight とは異なり料は発生しません Analytics アカウントされていないとき 
ジョブを実行します。  ジョブの実行時にのみ課金されます。  詳細については、次をご覧ください。 
[Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。  

**Data Lake Analytics アカウントを作成するには**

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。
2. クリックして **新規**, 、] をクリックして **データ + 分析**, 、] をクリックし、 **Data Lake 分析**します。
6. 次の項目を入力または選択します。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **名前**: Analytics アカウントの名前します。
    - **Data Lake ストア**: 各 Data Lake 分析アカウントが依存する Azure Data Lake ストア アカウントを持っています。 Data Lake Analytics アカウントと従属する Data Lake Store アカウントは、同じ Azure データ センターに配置する必要があります。 以下の指示に従って、新しい Data Lake Store アカウントを作成するか、既存のものを選択します。
    - **サブスクリプション**: Analytics アカウントに使用される Azure サブスクリプションを選択します。
    - **リソース グループ**します。 既存の Azure リソース グループを選択するか、新しいものを作成します。 Azure リソース マネージャー (ARM) を使用すると、アプリケーション内のリソースを 1 つのグループと見なして作業できます。 詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。 
    - **場所**します。 Data Lake Analytics アカウントの Azure データ センターを選択します。 

8. クリックして **作成**します。 ポータルのホーム画面が表示されます。 新しいタイルはスタート画面に追加され、"Azure Data Lake Analytics のデプロイ" を示すラベルが付けられます。 Data Lake Analytics アカウントの作成にはしばらく時間がかかります。 アカウントが作成されると、ポータルの新しいブレードにアカウントが開きます。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

<a name="access-adla-account"></a> **アクセス/開く Data Lake 分析アカウント**

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com/)します。
2. クリックして **参照** クリックして、左側のメニュー **Data Lake 分析**します。
3. アクセスする Data Lake Analytics アカウントをクリックします。 新しいブレードにアカウントが開きます。

**Data Lake Analytics アカウントを削除するには**

1. 削除する Data Lake Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. クリックして **削除** ブレードの上部にあるボタン メニューからです。
3. アカウント名を入力し、クリックして **削除**します。

Analytics アカウントを削除しても、従属する Data Lake Store アカウントは削除されません。 手順については、削除します。
Data Lake のストレージ アカウントを参照してください [Delete Data Lake ストア アカウント](data-lake-store-get-started-portal.md#delete-azure-data-lake-store-account)します。




























<!-- ################################ -->
<!-- ################################ -->
## アカウント データ ソースの管理

Data Lake Analytics では現在、以下のデータ ソースがサポートされています。

- [Azure Data Lake Store](data-lake-store-overview.md)
- [Azure BLOB ストレージ](storage-introduction.md)

Data Lake 分析アカウントを作成する際は、既定値となる Azure Data Lake ストア アカウントを指定する必要があります。 
入力します。 既定の Data Lake Store アカウントは、ジョブ メタデータとジョブ監査ログの格納に使用されます。 用意した後 
Data Lake 分析アカウントを作成するには、その他の Data Lake ストア アカウントや Azure ストレージ アカウントを追加できます。 

<a name="default-adl-account"></a>**既定の Data Lake ストレージ アカウントを検索するには**

- 管理する Data Lake Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。 既定の Data Lake ストアを示す **不可欠**:

    ![Azure Data Lake Analytics のデータ ソースの追加](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-default-adl-storage-account.png)

**データ ソースをさらに追加するには**

1. 管理する Data Lake Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. クリックして **設定** ] をクリックし、 **データ ソース**します。 表示される既定湖のデータ ストアのアカウントが表示されます。
あります。 
3. クリックして **データ ソースを追加**します。

    ![Azure Data Lake Analytics のデータ ソースの追加](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-add-data-source.png)

    Azure Data Lake Store アカウントを追加するには、アカウント名、およびクエリを実行できるようにするためのアカウントへのアクセスが必要になります。
    Azure BLOB ストレージを追加するには、ストレージ アカウントとアカウント キーが必要です。これらは、ポータルでストレージ アカウントに移動して見つけることができます。

<a name="explore-data-sources"></a>**データ ソースを探索するには**  

1. 管理する Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. クリックして **設定** ] をクリックし、 **データ エクスプ ローラー**します。 
 
    ![Azure Data Lake Analytics データ エクスプ ローラー](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-data-explorer.png)
    
3. Data Lake Store アカウントをクリックして開きます。

    ![Azure Data Lake Analytics データ エクスプ ローラーのストレージ アカウント](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-explore-adls.png)
    
    Data Lake Store アカウントごとに、以下を行うことができます。
    
    - **新しいフォルダー**: 新しいフォルダーを追加します。
    - **アップロード**: ワークステーションからストレージ アカウントにファイルをアップロードします。
    - **アクセス**: アクセスの構成のアクセス許可。
    - **フォルダーの名前変更**: フォルダーの名前を変更します。
    - **フォルダー プロパティ**: WASB のパス、WEBHDFS のパス、最後に変更された時刻になど、ファイルまたはフォルダーのプロパティを表示します。
    - **フォルダーを削除する**: フォルダーを削除します。

<a name="upload-data-to-adls"></a> **湖のデータ ストアのアカウントにファイルをアップロードするには**

1. ポータルで、次のようにクリックします。 * * [参照] * * をクリックし、左側のメニューから **湖のデータ ストア**します。
2. データをアップロードする Data Lake Store アカウントをクリックします。 既定のデータ湖のストレージ アカウントを検索するには、次を参照してください。 [ここ](#default-adl-account)します。
3. クリックして **データ エクスプ ローラー** 上部のメニューからです。
4. クリックして **新しいディレクトリ** を新しいフォルダーを作成またはフォルダーを変更するフォルダー名をクリックします。
6. クリックして **アップロード** ファイルをアップロードする上部のメニューからです。


<a name="upload-data-to-wasb"></a> **ファイルを Azure Blob ストレージ アカウントにアップロードするには**

参照してください [HDInsight での Hadoop ジョブ用データのアップロード](hdinsight-upload-data.md)します。  この情報は Data Lake Analytics に適用されます。


## ユーザーの管理

Data Lake Analytics では、Azure Active Directory でのロール ベースの Access Control を使用します。 Data Lake 分析を作成する場合 
アカウント、「サブスクリプション管理者」ロールが、アカウントに追加します。 その他のユーザーとのセキュリティ グループに追加することができます。 
次の役割:

|役割|説明|
|----|-----------|
|所有者|リソースへのアクセスを含め、すべてを管理できます。|
|共同作成者|ポータルにアクセスし、ジョブを送信および監視します。 ジョブを送信するには、共同作成者に Data Lake Store アカウントに対する読み取りまたは書き込みアクセス許可も必要になります。|
|DataLakeAnalyticsDeveloper | ユーザーはジョブを送信し、すべてのジョブを監視できますが、取り消すことができるのは自分のジョブのみです。 ユーザーの追加、アクセス許可の変更、アカウントの削除などを行って、自分のアカウントを管理することはできません。 ジョブを実行するには、Data Lake Store アカウントに対する読み取りまたは書き込みアクセス許可が必要です。     | 
|閲覧者|すべてを表示できますが、変更することはできません。|  
|DevTest Lab ユーザー|すべてを表示し、仮想マシンを接続、開始、再起動、シャットダウンできます。|  
|ユーザーアクセスの管理者|Azure リソースに対するユーザー アクセスを管理します。|  

Azure Active Directory ユーザーとセキュリティ グループの作成方法の詳細については、次を参照してください。 [Azure Active Directory は](active-directory-whatis.md)です。

**Analytics アカウントにユーザーまたはセキュリティ グループを追加するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. クリックして **設定**, 、] をクリックし、 **ユーザー**します。 クリックすることも **アクセス** 上、 **Essentials** タイトル バーの次のスクリーン ショットに示すようにします。

    ![Azure Data Lake Analytics アカウントのユーザーの追加](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-access-button.png)
3.  **ユーザー** ブレードで、をクリックして **追加**します。
4. ロールを選択し、ユーザーを追加し、] をクリックし、 **OK**します。

**注: このユーザーまたはセキュリティ グループは、ジョブを送信する必要があるを場合、データ湖ストアに対するアクセス許可を付与する、必要があります。 詳細については、次を参照してください。 [湖のデータ ストアに格納されたデータをセキュリティで保護された](data-lake-store-secure-data.md)します。**





<!-- ################################ -->
<!-- ################################ -->
## ジョブの管理

ジョブを実行するには、Data Lake Analytics アカウントが必要です。  詳細については、次を参照してください。 [管理 Data Lake 分析アカウント](#manage-data-lake-analytics-accounts)します。

<a name="create-job"></a>**ジョブを作成するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. クリックして **新しいジョブ**します。

    ![Azure Data Lake Analytics U-SQL ジョブの作成](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-create-job-button.png)

    次のような新しいブレードが表示されます。

    ![Azure Data Lake Analytics U-SQL ジョブの作成](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-new-job.png)

    ジョブごとに、以下を構成することができます。


  	|Name|Description|
  	|----|-----------|
  	|Job Name|Enter the name of the job.|
  	|Priority|Lower number is higher priority. If two jobs are both queued, the one with lower priority will be run first|
  	|Parallelism |Max number of compute processes that can happen at the same time. Increasing this number can improve performance but can also increase cost.|
  	|Script|Enter the U-SQL script for the job.|

    Using the same interface, you can also explore the link data sources, and add addtional files to the linked data sources. 
3. クリックして **ジョブの送信** 、ジョブを送信する場合。

**ジョブを送信するには**

参照してください [Data Lake 分析の作成ジョブ](#create-job)します。

<a name="monitor-jobs"></a>**ジョブを監視するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください。 
[Data Lake 分析アカウントにアクセス](#access-adla-account)します。 ジョブの管理パネルは、基本的なジョブを示しています。 
情報:

    ![Azure Data Lake Analytics U-SQL ジョブの管理](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-manage-jobs.png)

3. クリックして **ジョブ管理** 先のスクリーン ショットに示すようにします。

    ![Azure Data Lake Analytics U-SQL ジョブの管理](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-manage-jobs-details.png)

4. リストのジョブをクリックします。 [ **フィルター** ジョブを検索するのに役立ちます。

    ![Azure Data Lake Analytics U-SQL ジョブのフィルター処理](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-filter-jobs.png)

    別にジョブをフィルター処理できます **期間**, 、**ジョブ名**, 、および **作成者**します。
5. クリックして **を再送信** 、ジョブを再送信する場合。

**ジョブを再送信するには**

参照してください [モニター Data Lake 分析ジョブ](#monitor-jobs)します。

##アカウントの使用状況の監視

概要 - 我々 に必要な用語について説明し、構成要素を接続します。 ]

**アカウントの使用状況を監視するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください。 
[Data Lake 分析アカウントにアクセス](#access-adla-account)します。 使用状況は、以下のように [使用状況] パネルに表示されます。

    ![Azure Data Lake Analytics 使用状況の監視](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-monitor-usage.png)

2. ペインをダブルクリックすると、詳細が示されます。

##U-SQL カタログの表示

 [U SQL カタログ](data-lake-analytics-use-u-sql-catalog.md) U SQL スクリプトで共有できるように、データやコードを構成するために使用します。 カタログでは、Azure Data Lake のデータを使用して可能な限り最高のパフォーマンスを実現できます。 Azure ポータルで、U-SQL カタログを表示することができます。

**U-SQL カタログを参照するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください。 
[Data Lake 分析アカウントにアクセス](#access-adla-account)します。
2. クリックして **データ エクスプ ローラー** 上部のメニューからです。
3. 展開 **カタログ**, 、exapnd **マスター**, 、展開 * * テーブル、または **テーブル値関数**, 、または **アセンブリ**します。 次のスクリーンショットには、1 つのテーブル値関数が示されています。

    ![Azure Data Lake Analytics データ エクスプ ローラーのストレージ アカウント](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-explore-catalog.png)


<!-- ################################ -->
<!-- ################################ -->
## Azure リソース マネージャー グループの使用

アプリケーションが通常、web アプリ、データベース、データベース サーバー、ストレージ、たとえば、多くのコンポーネントの構成します。
およびサード パーティのサービスです。 Azure リソース マネージャー (ARM) では、アプリケーションでのリソースを操作できます。 
Azure リソース グループと呼ばれるグループです。 デプロイ、更新、監視、すべての削除、 
1 つ、組織的な操作で、アプリケーションのリソース。 展開用テンプレートを使用します。 
テンプレートは、テスト、ステージングと運用環境などのさまざまな環境を操作できます。 課金を明確にすることができます。 
グループ全体のロールアップ コストを表示することによって組織の 詳細については、次を参照してください [Azure。
リソース マネージャーの概要](resource-group-overview.md)します。 

Data Lake Analtyics サービスには、次のコンポーネントを含めることができます。

- Azure Data Lake Analytics アカウント
- 必要な既定の Azure Data Lake Store アカウント
- 追加の Azure Data Lake Store アカウント
- 追加の Azure Storage アカウント

管理しやすくするために 1 つの ARM グループの下にこれらすべてのコンポーネントを作成することができます。

![Azure Data Lake Analytics のアカウントとストレージ](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Data Lake Analytics アカウントと従属するストレージ アカウントは同じ Azure データ センターに配置する必要があります。
ただし、ARM グループは別のデータ センターに配置できます。  



##関連項目 

- [Microsoft Azure Data Lake Analytics の概要](data-lake-analytics-overview.md)
- [Azure ポータルで Azure Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell を使用する Azure Data Lake Analytics の管理](data-lake-analytics-use-powershell.md)
- [Azure ポータルを使用する Azure Data Lake Analytics ジョブの監視とトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)



