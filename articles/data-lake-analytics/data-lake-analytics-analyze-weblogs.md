<properties 
   pageTitle="Azure Data Lake Analytics を使用して Web サイトのログを分析する | Azure" 
   description="Data Lake Analytics を使用して Web サイトのログを分析する方法について説明します。 " 
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
   ms.date="12/01/2015"
   ms.author="jgao"/>

# チュートリアル: Azure Data Lake Analytics を使用して Web サイトのログを分析する

Data Lake Analytics を使用して Web サイトのログを分析する方法について、特に、Web サイトへのアクセスを試みたときにエラーが発生した参照元の特定に重点を置いて説明します。

>[AZURE.NOTE] アプリケーションの正常動作を確認する場合は、通過する時間の節約 [対話型チュートリアルを使用して Azure Data Lake 分析](data-lake-analytics-use-interactive-tutorials.md)します。 このチュートリアルでは、同じシナリオと同じコードを使用します。 このチュートリアルは、Data Lake Analytics アプリケーションの作成と実行の作業全体を開発者に体験してもらうことを目的としています。

## 前提条件:

- **Visual Studio 2015、Visual Studio 2013 で、Visual C インストールされている、4、または Visual Studio 2012 を更新**します。 
- **Microsoft Azure SDK for .NET バージョン 2.5 またはそれ以降**します。  使用してインストール、 [Web プラットフォーム インストーラー](http://www.microsoft.com/web/downloads/platform.aspx)します。
- **[Visual Studio のツールの data Lake](http://aka.ms/adltoolsvs)**します。 

    Data Lake Tools for Visual Studio がインストールされていると表示されます、 **Data Lake** Visual Studio のメニュー。
    
    ![U-SQL Visual Studio のメニュー](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-menu.png)

- **Data Lake 分析とデータ湖 Tools for Visual Studio の基本的な知識**します。 作業を開始するには、次のトピックをご覧ください。
 
    - [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md)します。
    - [Visual Studio の Data Lake ツールを使用して U SQL スクリプトを開発](data-lake-analytics-data-lake-tools-get-started.md)します。

- **Data Lake Analytics アカウント。**  参照してください [Azure Data Lake 分析アカウントを作成する](data-lake-analytics-get-started-portal.md#create_adl_analytics_account)です。

    Data Lake Tools では、Data Lake Analytics アカウントの作成はサポートされません。  そのため、Azure ポータル、Azure PowerShell、.NET SDK、または Azure CLI を使用して作成する必要があります。 
- **Data Lake Analytics アカウントへのサンプル データのアップロード。** 参照してください [既定データ湖のストレージ アカウントにアップロード SearchLog.tsv](data-lake-analytics-get-started-portal.md#update-data-to-the-default-adl-storage-account)します。

    Data Lake Analytics ジョブを実行するには、いくつかのデータが必要です。 Data Lake Tools でデータのアップロードがサポートされていても、このチュートリアルに従いやすくするため、サンプル データのアップロードにはポータルを使用します。 
 
## Connect to Azure 

U-SQL スクリプトをビルドしてテストするには、Azure に接続しておく必要があります。

**Data Lake Analytics に接続するには**

1. Visual Studio を開きます。
2.  **Data Lake** ] メニューをクリックして **オプションと設定**します。
4. をクリックして **サイン イン**, 、または **ユーザーの変更** 他のユーザーがサインイン時に、指示に従って場合。
5. クリックして **OK** オプションと設定] ダイアログを閉じます。

**Data Lake Analytics アカウントを参照するには**

1. Visual Studio で開きます **サーバー エクスプ ローラー** 押下時 **CTRL + ALT + S**します。
2.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、順に展開 **Data Lake 分析**します。 Data Lake Analytics アカウントが複数ある場合は、そのリストが表示されます。 Studio で Data Lake Analytics アカウントを作成することはできません。 アカウントを作成するを参照してください。 [Azure ポータルを使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-portal.md) または [Azure PowerShell を使用して Azure Data Lake 分析を使ってみる](data-lake-analytics-get-started-powershell.md)します。

## U-SQL アプリケーションの作成 

U SQL アプリケーションの大部分は、U-SQL スクリプトです。 U SQL の詳細については、次を参照してください。 [U SQL を使ってみる](data-lake-analytics-u-sql-get-started.md)します。

アプリケーションにはユーザー定義演算子を追加できます。  詳細については、次を参照してください。 [開発の U SQL ユーザー定義の Data Lake 分析ジョブに対して演算子](data-lake-analytics-u-sql-develop-user-defined-operators.md)します。
 
**Data Lake Analytics ジョブを作成して送信するには** 

1.  **ファイル** ] メニューのをクリックして **新規**, 、クリックして **プロジェクト**します。
2. プロジェクトの種類として、[U-SQL プロジェクト] を選択します。

    ![新しい U-SQL Visual Studio プロジェクト](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
    
3. Click **OK**. Visual Studio で、Script.usql ファイルを使用してソリューションが作成されます。
4. 次のスクリプトを Script.usql ファイルに入力します。

        // Create a database for easy reuse, so you don't need to read from a file every time.
        CREATE DATABASE IF NOT EXISTS SampleDBTutorials;
        
        // Create a Table valued function. TVF ensures that your jobs fetch data from the weblog file with the correct schema. 
        DROP FUNCTION IF EXISTS SampleDBTutorials.dbo.WeblogsView;
        CREATE FUNCTION SampleDBTutorials.dbo.WeblogsView()
        RETURNS @result TABLE
        (
            s_date DateTime,
            s_time string,
            s_sitename string,
            cs_method string, 
            cs_uristem string,
            cs_uriquery string,
            s_port int,
            cs_username string, 
            c_ip string,
            cs_useragent string,
            cs_cookie string,
            cs_referer string, 
            cs_host string,
            sc_status int,
            sc_substatus int,
            sc_win32status int, 
            sc_bytes int,
            cs_bytes int,
            s_timetaken int
        )
        AS
        BEGIN
        
            @result = EXTRACT
                s_date DateTime,
                s_time string,
                s_sitename string,
                cs_method string,
                cs_uristem string,
                cs_uriquery string,
                s_port int,
                cs_username string,
                c_ip string,
                cs_useragent string,
                cs_cookie string,
                cs_referer string,
                cs_host string,
                sc_status int,
                sc_substatus int,
                sc_win32status int,
                sc_bytes int,
                cs_bytes int,
                s_timetaken int
            FROM @"/Samples/Data/WebLog.log"
            USING Extractors.Text(delimiter:' ');
            RETURN;
        END;
        
        // Create a table for storing referrers and status 
        DROP TABLE IF EXISTS SampleDBTutorials.dbo.ReferrersPerDay;
        @weblog = SampleDBTutorials.dbo.WeblogsView();
        CREATE TABLE SampleDBTutorials.dbo.ReferrersPerDay
        (
            INDEX idx1
            CLUSTERED(Year ASC)
            PARTITIONED BY HASH(Year)
        ) AS 
        
        SELECT s_date.Year AS Year,
            s_date.Month AS Month,
            s_date.Day AS Day,
            cs_referer,
            sc_status,
            COUNT(DISTINCT c_ip) AS cnt
        FROM @weblog
        GROUP BY s_date,
                cs_referer, 
                sc_status;
        
    U SQL 詳細については、「 [Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)します。    
       
5. 新しい U-SQL スクリプトをプロジェクトに追加し、以下を入力してください。

        // Query the referrers that ran into errors
        @content =
            SELECT *
            FROM SampleDBTutorials.dbo.ReferrersPerDay
            WHERE sc_status >=400 AND sc_status < 500;
        
        OUTPUT @content
        TO @"/Samples/Outputs/UnsuccessfulResponses.log"
        USING Outputters.Tsv();

6. 最初の U SQL スクリプトを作成し、横に戻り、 **送信** ] ボタン、Analytics アカウントを指定します。
7.  **ソリューション エクスプ ローラー**, を右クリックして **Script.usql**, 、クリックして **ビルド スクリプト**します。 [出力] ウィンドウで結果を確認します。
8.  **ソリューション エクスプ ローラー**, を右クリックして **Script.usql**, 、クリックして **送信スクリプト**します。
9. 確認、 **Analytics アカウント** クリックして、ジョブを実行する 1 つは、 **送信**します。 送信が完了すると、Data Lake Tools for Visual Studio の [結果] ウィンドウに送信結果とジョブのリンクが示されます。
10. ジョブが正常に完了するまで待機します。  ジョブが失敗した場合、ソース ファイルがない可能性があります。  このチュートリアルの「前提条件」を参照してください。 追加のトラブルシューティング情報を参照してください。 [モニターし、Azure Data Lake 分析ジョブのトラブルシューティング](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)します。

    ジョブが完了すると、次の画面が表示されます。
    
    ![Data Lake Analytics による Web サイトのログ (weblogs) の分析](./media/data-lake-analytics-analyze-weblogs/data-lake-analytics-analyze-weblogs-job-completed.png)

11. ここでの手順 7 ~ 10 を繰り返して **Script1.usql**します。

>[AZURE.NOTE]読み込まれたり、または作成された、同じスクリプトで変更が済んでいる U SQL テーブルに書き込むことはできません。  これが、この例で 2 つのスクリプトを使用する利用です。

**ジョブの出力を表示するには**

1.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、展開 **Data Lake 分析**, 、Data Lake 分析アカウントを展開し、 **ストレージ アカウント**, 、既定のデータ湖のストレージ アカウントを右クリックし、 **エクスプ ローラー**します。 
2.  ダブルクリックして **サンプル** 、フォルダーを開きをダブルクリックする **出力**します。
3.  ダブルクリックして **UnsuccessfulResponsees.log**します。
4.  ジョブのグラフ ビューで出力ファイルをダブルクリックして、出力ファイルに直接移動することもできます。
        
## 関連項目

さまざまなツールを使用して Data Lake Analytics の使用を開始する方法については、以下をご覧ください。

- [Azure ポータルで Azure Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-portal.md)
- [Azure PowerShell で Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-powershell.md)
- [.NET SDK で Data Lake Analytics の使用を開始する](data-lake-analytics-get-started-net-sdk.md)

開発に関するその他のトピックも参照してください。 

- [Data Lake Tools for Visual Studio を使用する U-SQL スクリプトの開発](data-lake-analytics-data-lake-tools-get-started.md)
- [Azure Data Lake Analytics U-SQL 言語の使用](data-lake-analytics-u-sql-get-started.md)
- [Data Lake Analytics ジョブの U-SQL ユーザー定義演算子の開発](data-lake-analytics-u-sql-user-defined-operators.md)


