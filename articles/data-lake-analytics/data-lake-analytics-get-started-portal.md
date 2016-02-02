<properties 
   pageTitle="チュートリアル: Azure ポータルで Azure Data Lake Analytics の使用を開始する | Azure" 
   description="Azure ポータルを使用して Data Lake Analytics アカウントを作成し、U-SQL で Data Lake Analytics ジョブを作成して、ジョブを送信する方法について説明します。 " 
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
   ms.date="10/22/2015"
   ms.author="jgao"/>


# チュートリアル: Azure ポータルで Azure Data Lake Analytics の使用を開始する

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Azure ポータルを使用して Azure Data Lake 分析アカウントを作成、Data Lake 分析を定義する方法を説明します。
ジョブの [U SQL](data-lake-analytics-u-sql-get-started.md), 、Data Lake 分析アカウントへのジョブを送信します。 サブスクリプションの 
Data Lake 分析に関する情報を参照してください [Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。

このチュートリアルをタブ区切り値 (TSV) ファイルに読み取り、コンマに変換ジョブを開発します 
区切り (CSV) ファイルです。 サポートされている他のツールを使用する同じチュートリアルを読み進めるには、このセクションの上部にあるタブをクリックします。 最初のジョブが成功したら、U-SQL でのより複雑なデータ変換の記述を開始できます。

**基本的な Data Lake Analytics のプロセス:**

![Azure Data Lake Analytics プロセスのフロー図](./media/data-lake-analytics-get-started-portal/data-lake-analytics-process.png)

1. Data Lake Analytics アカウントを作成します。
2. ソース データを準備します。 Data Lake Analytics ジョブでは、Azure Data Lake Store アカウントまたは Azure BLOB ストレージ アカウントからデータを読み取ることができます。 この例では、Azure Data Lake Store から読み取ります。
3. U-SQL スクリプトを開発します。
4. ジョブ (U-SQL スクリプト) を Data Lake Analytics アカウントに送信します。 指示に従ってデータを処理するジョブは、ソース データから読み取ります 
U SQL スクリプトにし、データストア Lake アカウント、または Blob ストレージ アカウントへの出力を保存します。

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。

## Data Lake Analytics アカウントを作成する

ジョブを実行するには、Data Lake Analytics アカウントが必要です。

Data Lake 分析アカウントごとに、 [Azure Data Lake ストア]() 依存関係を考慮します。 このアカウントを参照します。
既定のデータストア湖アカウントです。 湖のデータ ストアのアカウントを作成するには、あらかじめまたは作成する場合 
Data Lake 分析アカウントです。 このチュートリアルでは、Data Lake 分析とデータ湖ストア アカウントを作成します 
宣言します。

**Data Lake Analytics アカウントを作成するには**

1. 新しいへのサインオン [Azure Classic Portal](https://portal.azure.com)します。
2. **[新規]**、**[データ + 分析]**、**[Data Lake Analytics]** の順にクリックします。
6. 次の項目を入力または選択します。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **名前**: Analytics アカウントに名前を付けます。
    - **Data Lake Store**: 各 Data Lake Analytics アカウントには、従属する Data Lake Store アカウントがあります。 Data Lake Analytics アカウントと従属する Data Lake Store アカウントは、同じ Azure データ センターに配置する必要があります。 以下の指示に従って、新しい Data Lake Store アカウントを作成するか、既存のものを選択します。
    - **サブスクリプション**: Analytics アカウントに使用する Azure サブスクリプションを選択します。
    - **リソース グループ**。 既存の Azure リソース グループを選択するか、新しいグループを作成します。 Azure リソース マネージャー (ARM) を使用すると、アプリケーション内のリソースを 1 つのグループと見なして作業できます。 詳細については、次を参照してください。 [Azure リソース マネージャーの概要](resource-group-overview.md)します。
    - **[場所]**:  Data Lake Analytics アカウントの Azure データ センターを選択します。
7. **[スタート画面にピン留めする]** を選択します。 このチュートリアルに従う場合はこれが必要です。
8. **[作成]** をクリックします。 ポータルのスタート画面が表示されます。 新しいタイルがスタート画面に追加され、"Azure Data Lake Analytics のデプロイ" を示すラベルが付けられます。 Data Lake Analytics アカウントの作成にはしばらく時間がかかります。 アカウントが作成されると、ポータルの新しいブレードにアカウントが開きます。

    ![Azure Data Lake Analytics ポータルのブレード](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)


Data Lake 分析アカウントを作成した後は、追加湖のデータ ストアのアカウントと Azure ストレージを追加することができます。 
使用します。 手順については、次を参照してください。 [Data lake 分析の管理アカウントのデータ ソース](data-lake-analytics-manage-use-portal.md#manage-account-data-sources)します。

## ソース データの準備

このチュートリアルでは、いくつかの検索ログを処理します。 検索ログは、Data Lake Store または Azure BLOB ストレージに格納できます。

Azure ポータルには、検索ログ ファイルを含むサンプル データ ファイルを既定の Data Lake アカウントにコピーするためのユーザー インターフェイスが用意されています。

**サンプル データ ファイルをコピーするには**

1. Azure ポータルで、左上隅にある **[Microsoft Azure]** をクリックします。
2. Data Lake Analytics アカウント名のタイルをクリックします。 ここにはアカウントの作成時にピン留めされます。
場合は、アカウントがある固定されずを参照してください。 
[ポータルから Data Lake 分析アカウントを開く](data-lake-analytics-manage-use-portal.md#access-adla-account) を開くには、
宣言します。
3. **[要点]** ペインを展開してから、**[サンプル ジョブの検索]** をクリックします。 呼ばれる別のブレードを開き、* * サンプル
ジョブ * です。
4. **[サンプル データのコピー]** をクリックしてから **[OK]** をクリックして確定します。
5. **[通知]** (ベルの形のアイコン) をクリックします。 **サンプル データの更新が完了した**ことを示すログが表示されます。 通知ペインの外側の任意の場所をクリックして閉じます。
7. Data Lake Analytics アカウント ブレードで、上部の **[データ エクスプ ローラー]** をクリックします。

    ![Azure Data Lake Analytics データ エクスプローラーのボタン](./media/data-lake-analytics-get-started-portal/data-lake-analytics-data-explorer-button.png)

    2 つのブレードが開きます。 1 つは **データ エクスプ ローラー**、もう 1 つは既定の Data Lake Store アカウントです。
8. 既定の Data Lake Store アカウント ブレードで、**[サンプル]** をクリックしてフォルダーを展開してから、**[データ]** をクリックしてフォルダーを展開します。 次のファイルとフォルダーが表示されます。

    - AmbulanceData/
    - AdsLog.tsv
    - SearchLog.tsv
    - version.txt
    - WebLog.log

    このチュートリアルでは、SearchLog.tsv を使用します。

実際には、リンクされたストレージ アカウントにデータを書き込むか、データをアップロードするようにアプリケーションをプログラムします。 ファイルをアップロードするため、次を参照してください。 
[湖のデータ ストアにデータをアップロード](data-lake-analytics-manage-use-portal.md#upload-data-to-adls) または [Blob ストレージにデータをアップロード](data-lake-analytics-manage-use-portal.md#upload-data-to-wasb)します。

## Data Lake Analytics ジョブの作成と送信

ソース データの準備ができたら、U-SQL スクリプトの開発を開始できます。

**ジョブを送信するには**

1. ポータルの Data Lake Analytics アカウント ブレードで、**[新しいジョブ]** をクリックします。

    ![Azure Data Lake Analytics の新しいジョブ ボタン](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job-button.png)

    ブレードが表示されない場合は、次を参照してください。 [ポータルから Data Lake 分析アカウントを開く](data-lake-analytics-manage-use-portal.md#access-adla-account)します。
4. **ジョブ名**を入力し、次の U-SQL スクリプトを入力します。

 ![Azure Data Lake Analytics U-SQL ジョブの作成](./media/data-lake-analytics-get-started-portal/data-lake-analytics-new-job.png)

     @searchlog =
         EXTRACT UserId          int,
                 Start           DateTime,
                 Region          string,
                 Query           string,
                 Duration        int?,
                 Urls            string,
                 ClickedUrls     string
         FROM "/Samples/Data/SearchLog.tsv"
         USING Extractors.Tsv();
    
     OUTPUT @searchlog   
         TO "/Output/SearchLog-from-Data-Lake.csv"
     USING Outputters.Csv();

 この U-SQL スクリプトでは、**Extractors.Tsv()** を使用してソース データ ファイルを読み取ってから、**Outputters.Csv()** を使用して csv ファイルを作成します。

 ソース ファイルを別の場所にコピーしない限り、2 つのパスを変更しないでください。 存在しない場合、Data Lake Analytics は出力フォルダーを作成します。 この場合は、単純な相対パスを使用します。

 既定の Data Lake アカウントに保存されたファイルの相対パスを使用する方が簡単です。 絶対パスを使用することもできます。 たとえば、次のように入力します。

     adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

 詳細については、U SQL は、次を参照してください。 [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md) と [U SQL 言語リファレンス](http://go.microsoft.com/fwlink/?LinkId=691348)します。

5. 上部の **[ジョブの送信]** をクリックします。 新しいジョブの詳細ペインが開きます。 タイトル バーに、ジョブの状態が示されます。
6. ジョブの状態が **[成功]** に変わるまで待機します。 ジョブが完了したら、ポータルの新しいブレードにジョブの詳細が開きます。

    ![Azure Data Lake Analytics ジョブの詳細](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-completed.png)

    前述のスクリーンショットから、ジョブが完了する (状態が送信済みから終了に変わる) まで約 1.5 分かかったことがわかります。

    ジョブが失敗した場合は、次を参照してください。 [モニター Data Lake 分析ジョブのトラブルシューティングと](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorials.md)します。

7. **[ジョブの詳細]** ブレードの下部にある **SearchLog-from-Data-Lake.csv** のジョブ名をクリックします。 出力ファイルはプレビュー、ダウンロード、名前変更、削除できます。

    ![Azure Data Lake Analytics ジョブの出力ファイルのプロパティ](./media/data-lake-analytics-get-started-portal/data-lake-analytics-output-file-properties.png)
8. **[プレビュー]** をクリックして出力ファイルを表示します。

    ![Azure Data Lake Analytics ジョブの出力ファイルのプレビュー](./media/data-lake-analytics-get-started-portal/data-lake-analytics-job-output-preview.png)

## 関連項目

- 複雑なクエリを表示するには、次を参照してください。 [分析の web サイトを Azure Data Lake 分析を使用してログに記録](data-lake-analytics-analyze-weblogs.md)します。
- U SQL アプリケーションの開発開始するを参照してください。 [Data Lake Tools を使用して、Visual Studio の開発の U SQL スクリプト](data-lake-analytics-data-lake-tools-get-started.md)します。
- U SQL については、次を参照してください。 [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)します。
- 管理タスクを参照してください。 [管理 Azure Data Lake 分析 Azure ポータルを使用して](data-lake-analytics-manage-use-portal.md)します。
- Data Lake 分析の概要を取得するには、次を参照してください。 [Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。
- 他のツールを使用する同じチュートリアルを表示するには、ページの上部にあるタブ セレクターをクリックします。





