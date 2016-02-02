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

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。






## アカウントの管理

Data Lake Analytics ジョブを実行するには、Data Lake Analytics アカウントが必要です。 Azure HDInsight とは異なり料は発生しません Analytics アカウントされていないとき 
ジョブを実行します。 ジョブの実行時にのみ課金されます。 詳細については、次をご覧ください。 
[Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。

**Data Lake Analytics アカウントを作成するには**

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com)します。
2. **[新規]**、**[データ + 分析]**、**[Data Lake Analytics]** の順にクリックします。
6. 次の項目を入力または選択します。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **名前**: Analytics アカウントに名前を付けます。
    - **Data Lake Store**: 各 Data Lake Analytics アカウントには、従属する Azure Data Lake Store アカウントがあります。 Data Lake Analytics アカウントと従属する Data Lake Store アカウントは、同じ Azure データ センターに配置する必要があります。 以下の指示に従って、新しい Data Lake Store アカウントを作成するか、既存のものを選択します。
    - **サブスクリプション**: Analytics アカウントに使用する Azure サブスクリプションを選択します。
    - **リソース グループ**。 既存の Azure リソース グループを選択するか、新しいグループを作成します。 Azure リソース マネージャー (ARM) を使用すると、アプリケーション内のリソースを 1 つのグループと見なして作業できます。 詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。
    - **[場所]**:  Data Lake Analytics アカウントの Azure データ センターを選択します。

8. **[作成]** をクリックします。 ポータルのホーム画面が表示されます。 新しいタイルはスタート画面に追加され、"Azure Data Lake Analytics のデプロイ" を示すラベルが付けられます。 Data Lake Analytics アカウントの作成にはしばらく時間がかかります。 アカウントが作成されると、ポータルの新しいブレードにアカウントが開きます。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

<a name="access-adla-account"></a> **Data Lake 分析アカウントのアクセス/開く**

1. 新しいへのサインオン [Azure ポータル](https://portal.azure.com/)します。
2. 左側のメニューで **[参照]** をクリックしてから、**[Data Lake Analytics]** をクリックします。
3. アクセスする Data Lake Analytics アカウントをクリックします。 新しいブレードにアカウントが開きます。

**Data Lake Analytics アカウントを削除するには**

1. 削除する Data Lake Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. ブレードの上部にあるボタン メニューで、**[削除]** をクリックします。
3. アカウント名を入力してから、**[削除]** をクリックします。

Analytics アカウントを削除しても、従属する Data Lake Store アカウントは削除されません。 手順については、削除します。
Data Lake のストレージ アカウントを参照してください [Delete Data Lake ストア アカウント](data-lake-store-get-started-portal.md#delete-azure-data-lake-store-account)します。






























## アカウント データ ソースの管理

Data Lake Analytics では現在、以下のデータ ソースがサポートされています。

- [Azure Data Lake ストア](data-lake-store-overview.md)
- [Azure Blob ストレージ](storage-introduction.md)

Data Lake 分析アカウントを作成する際は、既定値となる Azure Data Lake ストア アカウントを指定する必要があります。 
入力します。 既定の Data Lake Store アカウントは、ジョブ メタデータとジョブ監査ログの格納に使用されます。 用意した後 
Data Lake 分析アカウントを作成するには、その他の Data Lake ストア アカウントや Azure ストレージ アカウントを追加できます。

<a name="default-adl-account"></a>**既定の Data Lake ストレージ アカウントを検索するには**

- 管理する Data Lake Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。 既定の Data Lake Store は以下のように **[要点]** に表示されます。

    ![Azure Data Lake Analytics のデータ ソースの追加](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-default-adl-storage-account.png)

**データ ソースをさらに追加するには**

1. 管理する Data Lake Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. **[設定]** をクリックしてから、**[データ ソース]** をクリックします。 表示される既定湖のデータ ストアのアカウントが表示されます。
あります。
3. **[データ ソースの追加]** をクリックします。

    ![Azure Data Lake Analytics のデータ ソースの追加](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-add-data-source.png)

    Azure Data Lake Store アカウントを追加するには、アカウント名、およびクエリを実行できるようにするためのアカウントへのアクセスが必要になります。
    Azure BLOB ストレージを追加するには、ストレージ アカウントとアカウント キーが必要です。これらは、ポータルでストレージ アカウントに移動して見つけることができます。

<a name="explore-data-sources"></a>**データ ソースを探索するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. **[設定]** をクリックしてから、**[データ エクスプローラー]** をクリックします。

    ![Azure Data Lake Analytics データ エクスプ ローラー](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-data-explorer.png)

3. Data Lake Store アカウントをクリックして開きます。

    ![Azure Data Lake Analytics データ エクスプ ローラーのストレージ アカウント](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-explore-adls.png)

    Data Lake Store アカウントごとに、以下を行うことができます。

    - **新しいフォルダー**: 新しいフォルダーを追加します。
    - **アップロード**: ワークステーションからストレージ アカウントにファイルをアップロードします。
    - **アクセス**: アクセス許可を構成します。
    - **フォルダー名の変更**: フォルダーの名前を変更します。
    - **フォルダーのプロパティ**: WASB パス、WEBHDFS パス、最終変更時刻など、ファイルまたはフォルダーのプロパティを表示します。
    - **フォルダーの削除**: フォルダーを削除します。

<a name="upload-data-to-adls"></a> **湖のデータ ストアのアカウントにファイルをアップロードする**

1. ポータルで、次のようにクリックします。 **参照 * * をクリックし、左側のメニューから * * データ湖ストア**します。
2. データをアップロードする Data Lake Store アカウントをクリックします。 既定のデータ湖のストレージ アカウントを検索するには、次を参照してください。 [ここ](#default-adl-account)します。
3. 上部のメニューにある **[データ エクスプローラー]** をクリックします。
4. **[新しいディレクトリ]** をクリックして新しいフォルダーを作成するか、フォルダー名をクリックしてフォルダーを変更します。
6. 上部のメニューにある **[アップロード]** をクリックしてファイルをアップロードします。


<a name="upload-data-to-wasb"></a> **ファイルを Azure Blob ストレージ アカウントにアップロードする**

参照してください [HDInsight での Hadoop ジョブ用データのアップロード](hdinsight-upload-data.md)します。 この情報は Data Lake Analytics に適用されます。


## ユーザーの管理

Data Lake Analytics では、Azure Active Directory でのロール ベースの Access Control を使用します。 Data Lake 分析を作成する場合 
アカウント、「サブスクリプション管理者」ロールが、アカウントに追加します。 その他のユーザーとのセキュリティ グループに追加することができます。 
次の役割:

| ロール| 説明|
|----|-----------|
| 所有者| リソースへのアクセスを含め、すべてを管理できます。|
| 共同作成者| ポータルにアクセスし、ジョブを送信および監視します。ジョブを送信するには、共同作成者に Data Lake Store アカウントに対する読み取りまたは書き込みアクセス許可も必要になります。|
| DataLakeAnalyticsDeveloper| ユーザーはジョブを送信し、すべてのジョブを監視できますが、取り消すことができるのは自分のジョブのみです。ユーザーの追加、アクセス許可の変更、アカウントの削除などを行って、自分のアカウントを管理することはできません。ジョブを実行するには、Data Lake Store アカウントに対する読み取りまたは書き込みアクセス許可が必要です。|
| 閲覧者| すべてを表示できますが、変更することはできません。|
| DevTest Lab ユーザー| すべてを表示し、仮想マシンを接続、開始、再起動、シャットダウンできます。|
| ユーザーアクセスの管理者| Azure リソースに対するユーザー アクセスを管理します。|

Azure Active Directory ユーザーとセキュリティ グループの作成方法の詳細については、次を参照してください。 [Azure Active Directory は](active-directory-whatis.md)します。

**Analytics アカウントにユーザーまたはセキュリティ グループを追加するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. **[設定]** をクリックし、**[ユーザー]** をクリックします。 以下のスクリーンショットに示されているように、**[要点]** タイトル バーの **[アクセス]** をクリックすることもできます。

    ![Azure Data Lake Analytics アカウントのユーザーの追加](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-access-button.png)
3. **[ユーザー]** ブレードで、**[追加]** をクリックします。
4. ロールを選択し、ユーザーを追加してから **[OK]** をクリックします。

* * 注: このユーザーまたはセキュリティ グループは、ジョブを送信する必要があるを場合、データ湖ストアに対するアクセス許可を付与する、必要があります。 詳細については、次を参照してください [湖のデータ ストアに格納されたデータをセキュリティで保護された](data-lake-store-secure-data.md)。 * *。







## ジョブの管理

ジョブを実行するには、Data Lake Analytics アカウントが必要です。 詳細については、次を参照してください。 [管理 Data Lake 分析アカウント](#manage-data-lake-analytics-accounts)します。

<a name="create-job"></a>**ジョブを作成するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください [アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. **[新しいジョブ]** をクリックします。

    ![Azure Data Lake Analytics U-SQL ジョブの作成](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-create-job-button.png)

    次のような新しいブレードが表示されます。

    ![Azure Data Lake Analytics U-SQL ジョブの作成](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-new-job.png)

    ジョブごとに、以下を構成することができます。

   | 名前| 説明|
   |----|-----------|
   | ジョブ名| ジョブの名前を入力します。|
   | 優先順位| 数が小さいほど優先順位が高くなります。2 つのジョブが両方ともキューに登録されている場合は、優先順位の低いものが最初に実行されます。|
   | 並列処理| 同時に発生する可能性があるコンピューティング プロセスの最大数。この数を増やすことで、パフォーマンスを向上させることができますが、コストが増加することもあります。|
   | スクリプト| ジョブの U-SQL スクリプトを入力します。|

    同じインターフェイスを使用して、リンク データ ソースを検索し、リンクされているデータ ソースにファイルをさらに追加することもできます。
3. ジョブを送信する場合は、**[ジョブの送信]** をクリックします。

**ジョブを送信するには**

参照してください [Data Lake 分析の作成ジョブ](#create-job)します。

<a name="monitor-jobs"></a>**ジョブを監視するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください。 
[アクセス Data Lake 分析アカウント](#access-adla-account)します。 ジョブの管理パネルは、基本的なジョブを示しています。 
情報:

    ![Azure Data Lake Analytics U-SQL ジョブの管理](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-manage-jobs.png)

3. 前述のスクリーンショットに示されている **[ジョブ管理]** をクリックします。

    ![Azure Data Lake Analytics U-SQL ジョブの管理](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-manage-jobs-details.png)

4. リストのジョブをクリックします。 または、以下のように **[フィルター]** をクリックします。これは、ジョブを検索するのに役立ちます。

    ![Azure Data Lake Analytics U-SQL ジョブのフィルター処理](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-filter-jobs.png)

    ジョブは、**時間範囲**、**ジョブ名**、および**作成者**でフィルター処理できます。
5. ジョブを再送信する場合は、**[再送信]** をクリックします。

**ジョブを再送信するには**

参照してください [モニター Data Lake 分析ジョブ](#monitor-jobs)します。

## アカウントの使用状況の監視

概要 - 我々 に必要な用語について説明し、構成要素を接続します。 ]

**アカウントの使用状況を監視するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください。 
[アクセス Data Lake 分析アカウント](#access-adla-account)します。 使用状況は、以下のように [使用状況] パネルに表示されます。

    ![Azure Data Lake Analytics 使用状況の監視](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-monitor-usage.png)

2. ペインをダブルクリックすると、詳細が示されます。

## U-SQL カタログの表示

[U SQL カタログ](data-lake-analytics-use-u-sql-catalog.md) U SQL スクリプトで共有できるように、データやコードを構成するために使用します。 カタログでは、Azure Data Lake のデータを使用して可能な限り最高のパフォーマンスを実現できます。 Azure ポータルで、U-SQL カタログを表示することができます。

**U-SQL カタログを参照するには**

1. 管理する Analytics アカウントを開きます。 手順を参照してください。 
[アクセス Data Lake 分析アカウント](#access-adla-account)します。
2. 上部のメニューにある **[データ エクスプローラー]** をクリックします。
3. 展開 **カタログ**, 、exapnd **マスター**, 、展開 **テーブル、または * * テーブル値関数**, 、または **アセンブリ**します。 次のスクリーンショットには、1 つのテーブル値関数が示されています。

    ![Azure Data Lake Analytics データ エクスプ ローラーのストレージ アカウント](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-explore-catalog.png)




## Azure リソース マネージャー グループの使用

アプリケーションが通常、web アプリ、データベース、データベース サーバー、ストレージ、たとえば、多くのコンポーネントの構成します。
およびサード パーティのサービスです。 Azure リソース マネージャー (ARM) では、アプリケーションでのリソースを操作できます。 
Azure リソース グループと呼ばれるグループです。 デプロイ、更新、監視、すべての削除、 
1 つ、組織的な操作で、アプリケーションのリソース。 展開用テンプレートを使用します。 
テンプレートは、テスト、ステージングと運用環境などのさまざまな環境を操作できます。 課金を明確にすることができます。 
グループ全体のロールアップ コストを表示することによって組織の 詳細については、[Azure を参照してください。 
リソース マネージャー Overview](resource-group-overview.md) します。

Data Lake Analtyics サービスには、次のコンポーネントを含めることができます。

- Azure Data Lake Analytics アカウント
- 必要な既定の Azure Data Lake Store アカウント
- 追加の Azure Data Lake Store アカウント
- 追加の Azure Storage アカウント

管理しやすくするために 1 つの ARM グループの下にこれらすべてのコンポーネントを作成することができます。

![Azure Data Lake Analytics のアカウントとストレージ](./media/data-lake-analytics-manage-use-portal/data-lake-analytics-arm-structure.png)

Data Lake Analytics アカウントと従属するストレージ アカウントは同じ Azure データ センターに配置する必要があります。
ただし、ARM グループは別のデータ センターに配置できます。



## 関連項目

- [Microsoft Azure Data Lake 分析の概要](data-lake-analytics-overview.md)
- [Azure ポータルを使用して Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell を使用して Azure Data Lake 分析を管理します。](data-lake-analytics-use-powershell.md)
- [監視し、Azure ポータルを使用して Azure Data Lake 分析ジョブのトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)






