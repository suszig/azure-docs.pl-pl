<properties 
   pageTitle="Azure ポータルを使用する Azure Data Lake Analytics ジョブのトラブルシューティング | Azure" 
   description="Azure ポータルを使用して、Data Lake Analytics ジョブのトラブルシューティングを行う方法について説明します。 " 
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

# Azure ポータルを使用する Azure Data Lake Analytics ジョブのトラブルシューティング

Azure ポータルを使用して、Data Lake Analytics ジョブのトラブルシューティングを行う方法について説明します。

このチュートリアルでは、不足しているソース ファイルの問題をセットアップし、Azure ポータルを使用してその問題のトラブルシューティングを行います。

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Data Lake 分析ジョブ プロセスの基本的な知識**します。 参照してください [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-use-portal.md)します。
- **Data Lake 分析アカウント**します。 参照してください [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-use-portal.md#create-adl-analytics-account)します。
- **既定のデータ湖ストア アカウントにサンプル データをコピー**します。  参照してください [ソース データを準備します。](data-lake-analytics-get-started-use-portal.md.md#prepare-source-data)

##Data Lake Analytics ジョブの送信

ここでは、不適切なソース ファイル名で U-SQL ジョブを作成します。  

**ジョブを送信するには**

1. Azure ポータルからクリックして **Microsoft Azure** ウィンドウの左上隅です。
2. Data Lake Analytics アカウント名のタイルをクリックします。  ここにはアカウントの作成時にピン留めされます。
場合は、アカウントがある固定されずを参照してください。 
[ポータルから Analytics アカウントを開く](data-lake-analytics-manage-use-portal.md#access-adla-account)します。
3. クリックして **新しいジョブ** 上部のメニューからです。
4. ジョブ名および以下の U-SQL スクリプトを入力します。

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv1"
            USING Extractors.Tsv();
        
        OUTPUT @searchlog   
            TO "/output/SearchLog-from-adls.csv"
        USING Outputters.Csv();

    スクリプトで定義されているソース ファイルが **/Samples/Data/SearchLog.tsv1**, ものとすることで、 **/Samples/Data/SearchLog.tsv**します。
     
5. クリックして **ジョブの送信** 上部にあります。 新しいジョブの詳細ペインが開きます。 タイトル バーに、ジョブの状態が示されます。 完了するまで数分かかります。 クリックして **更新** 最新の状態を取得します。
6. ジョブの状態が変更されるまでの待機 **失敗**します。  ジョブの場合 **Succeeded**, 、/Samples フォルダーを削除していないためにです。 参照してください、 **前提条件となる** チュートリアルの先頭にあるセクションです。

小規模なジョブにどうしてこんなにも時間がかかるのか疑問に思われるかもしれません。  Data Lake Analytics はビッグ データを処理するように設計されていることに注意してください。  これは、分散システムを使用して大量のデータを処理する場合に役立ちます。

ジョブを送信したと仮定し、ポータルを閉じます。  次のセクションでは、ジョブのトラブルシューティングを行う方法を学習します。


## ジョブのトラブルシューティング

この最後のセクションでは、ジョブが送信済みで、そのジョブは失敗した状態にあります。  

**すべてのジョブを表示するには**

1. Azure ポータルからクリックして **Microsoft Azure** ウィンドウの左上隅です。
2. Data Lake Analytics アカウント名のタイルをクリックします。  ジョブの概要が表示される、 **ジョブの管理** を並べて表示します。

    ![Azure Data Lake Analytics ジョブの管理](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-job-management.png)
    
    [ジョブ管理] を見れば、ジョブの状態が一目でわかります。 失敗したジョブがあることに注目してください。
   
3. クリックして、 **ジョブ管理** タイルをジョブを表示します。 ジョブに分類されます **を実行している**, 、**キューに入れられた**, 、および **終了**します。 は、失敗したジョブが表示されます、 **終了** セクションです。 これがリストの先頭になります。 多数のジョブを使用する場合は、クリックして **フィルター** ジョブを検索するのに役立ちます。

    ![Azure Data Lake Analytics のジョブのフィルター](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-filter-jobs.png)

4. リストから失敗したジョブをクリックして、以下のように新しいブレードでジョブの詳細を開きます。

    ![Azure Data Lake Analytics の失敗したジョブ](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job.png)
    
    通知、 **を再送信** ] ボタンをクリックします。 問題を修正した後、ジョブを再送信することができます。

5. 前のスクリーンショットの強調表示されている部分をクリックして、エラーの詳細を開きます。  次のような結果が表示されます。

    ![Azure Data Lake Analytics の失敗したジョブの詳細](./media/data-lake-analytics-monitor-and-troubleshoot-tutorial/data-lake-analytics-failed-job-details.png)

    ソース フォルダーが見つからないことが示されています。
    
6. クリックして **スクリプトが重複して**します。
7. 更新プログラム、 **FROM** には、次のパス。

    "/Samples/Data/SearchLog.tsv"

8. クリックして **ジョブを送信**します。


##関連項目

- [Azure Data Lake Analytics の概要](data-lake-analytics-overview.md)
- [Azure PowerShell で Azure Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-powershell.md)
- [Visual Studio で Azure Data Lake Analytics と U-SQL の使用を開始する](data-lake-analytics-get-started-u-sql-studio.md)
- [Azure ポータルを使用する Azure Data Lake Analytics の管理](data-lake-analytics-manage-use-portal.md)






