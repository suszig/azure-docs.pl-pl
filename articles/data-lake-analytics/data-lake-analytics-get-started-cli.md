<properties 
   pageTitle="Azure コマンド ライン インターフェイスを使用して Azure Data Lake Analytics の使用を開始する | Microsoft Azure" 
   description="Azure コマンド ライン インターフェイスを使用して Data Lake Store アカウントを作成し、U-SQL を使用して Data Lake Analytics ジョブを作成し、ジョブを送信する方法について説明します。 " 
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
   ms.date="11/02/2015"
   ms.author="jgao"/>


# チュートリアル: Azure コマンド ライン インターフェイス (CLI) を使用して Azure Data Lake Analytics の使用を開始する

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]


Azure CLI を使用して Azure Data Lake 分析アカウントを作成、Data Lake 分析を定義する方法を説明します。
ジョブの [U SQL](data-lake-analytics-u-sql-get-started.md), 、Data Lake 分析アカウントへのジョブを送信します。 サブスクリプションの 
Data Lake 分析に関する情報を参照してください [Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。

このチュートリアルをタブ区切り値 (TSV) ファイルに読み取り、コンマに変換ジョブを開発します 
区切り (CSV) ファイルです。 サポートされている他のツールを使用する同じチュートリアルを読み進めるには、このセクションの上部にあるタブをクリックします。

**基本的な Data Lake Analytics のプロセス:**

![Azure Data Lake Analytics プロセスのフロー図](./media/data-lake-analytics-get-started-portal/data-lake-analytics-process.png)

1. Data Lake Analytics アカウントを作成します。
2. ソース データを準備します。 Data Lake Analytics ジョブでは、Azure Data Lake Store アカウントまたは Azure BLOB ストレージ アカウントからデータを読み取ることができます。
3. U-SQL スクリプトを開発します。
4. ジョブ (U-SQL スクリプト) を Data Lake Analytics アカウントに送信します。 指示に従ってデータを処理するジョブは、ソース データから読み取ります 
U SQL スクリプトにし、データストア Lake アカウント、または Blob ストレージ アカウントへの出力を保存します。

**前提条件**

このチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](https://azure.microsoft.com/pricing/free-trial/)します。
- **Azure CLI**。 参照してください [をインストールし、Azure cli](xplat-cli.md)します。
    - ダウンロードし、インストール、 **プレリリース** [Azure CLI ツール](https://github.com/MicrosoftBigData/AzureDataLake/releases) このデモを完了するためにします。
- **認証**。次のコマンドを使用します。

        azure login

    職場または学校のアカウントを使用した認証の詳細については、次を参照してください。 [Azure CLI から Azure サブスクリプションへの接続](xplat-cli-connect.md)します。
- **Azure リソース マネージャー モードへの切り替え**。次のコマンドを使用します。

        azure config mode arm


## Data Lake Analytics アカウントを作成する

ジョブを実行するには、Data Lake Analytics アカウントが必要です。 Data Lake Analytics アカウントを作成するには、以下を指定する必要があります。

- **Azure リソース グループ**: Data Lake Analytics アカウントは、Azure リソース グループ内に作成する必要があります。 [Azure リソース マネージャー](resource-group-overview.md) グループとして、アプリケーション内のリソースを使用することができます。 アプリケーションのこれらすべてのリソースを、1 回の連携した操作でデプロイ、更新、または削除できます。

    サブスクリプションのリソース グループを列挙するには:

        azure group list 

    新しいリソース グループを作成するには:

        azure group create -n "<Resource Group Name>" -l "<Azure Location>"

- **Data Lake Analytics アカウント名**
- **場所**: Data Lake Analytics をサポートするいずれかの Azure データ センター。
- **既定の Data Lake アカウント**: 各 Data Lake Analytics アカウントには既定の Data Lake アカウントがあります。

    既存の Data Lake アカウントを一覧表示するには:

        azure datalake store account list

    新しい Data Lake アカウントを作成するには:

        azure datalake store account create "<Data Lake Store Account Name>" "<Azure Location>" "<Resource Group Name>"

    > [AZURE.NOTE] Data Lake アカウント名には小文字と数字のみを含める必要があります。



**Data Lake Analytics アカウントを作成するには**

        azure datalake analytics account create "<Data Lake Analytics Account Name>" "<Azure Location>" "<Resource Group Name>" "<Default Data Lake Account Name>"
    
        azure datalake analytics account list
        azure datalake analytics account show "<Data Lake Analytics Account Name>"          

![Data Lake Analytics でのアカウントの表示](./media/data-lake-analytics-get-started-cli/data-lake-analytics-show-account-cli.png)
> [AZURE.NOTE] Data Lake Analytics アカウント名には小文字と数字のみを含める必要があります。


## Data Lake Store にデータをアップロードする

このチュートリアルでは、いくつかの検索ログを処理します。 検索ログは、Data Lake Store または Azure BLOB ストレージに格納できます。

Azure ポータルには、検索ログ ファイルを含むサンプル データ ファイルを既定の Data Lake アカウントにコピーするためのユーザー インターフェイスが用意されています。 参照してください [ソース データを準備する](data-lake-analytics-get-started-portal.md#prepare-source-data) 湖のデータ ストアの既定のアカウントにデータをアップロードします。

CLI を使用してファイルをアップロードするには、次のコマンドを使用します。

    azure datalake store filesystem import "<Data Lake Store Account Name>" "<Path>" "<Destination>"
    azure datalake store filesystem list "<Data Lake Store Account Name>" "<Path>"

Data Lake Analytics は、Azure BLOB ストレージにもアクセスできます。 Azure Blob ストレージにデータをアップロードする、次を参照してください。 [Azure Storage で Azure CLI の使用](storage-azure-cli.md)します。

## Data Lake Analytics ジョブを送信する

Data Lake Analtyics ジョブは U-SQL 言語で記述されます。 U SQL の詳細については、次を参照してください。 [U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md) と [U SQL 言語リファレンス](http://go.microsoft.com/fwlink/?LinkId=691348)します。

**Data Lake Analytics ジョブ スクリプトを作成するには**

- 次の U-SQL スクリプトでテキスト ファイルを作成し、ワークステーションにテキスト ファイルを保存します。

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

  ソース ファイルを別の場所にコピーしない限り、2 つのパスを変更しないでください。 存在しない場合、Data Lake Analytics は出力フォルダーを作成します。

  既定の Data Lake アカウントに格納されたファイルの相対パスを使用する方が簡単です。 絶対パスを使用することもできます。 たとえば、次のように入力します。

      adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

  リンクされたストレージ アカウント内のファイルへのアクセスには、絶対パスを使用する必要があります。 リンクされた Azure ストレージ アカウントに格納されているファイルの構文は以下のとおりです。

      wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

  >[AZURE.NOTE] パブリック BLOB またはパブリック コンテナーのアクセス許可を持つ Azure BLOB コンテナーは、現在サポートされていません。      


**ジョブを送信するには**


    azure datalake analytics job create  "<Data Lake Analytics Account Name>" "<Job Name>" "<Script>"

次のコマンドを使用して、ジョブの一覧表示、ジョブの詳細の取得、およびジョブの取り消しを実行できます。

    azure datalake analytics job cancel "<Data Lake Analytics Account Name>" "<Job Id>"
    azure datalake analytics job list "<Data Lake Analytics Account Name>"
    azure datalake analytics job show "<Data Lake Analytics Account Name>" "<Job Id>"

ジョブが完了したら、以下のコマンドレットを使用し、ファイルをリストしてダウンロードできます。

    azure datalake store filesystem list "<Data Lake Store Account Name>" "/Output"
    azure datalake store filesystem export "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" "<Destination>"
    azure datalake store filesystem read "<Data Lake Store Account Name>" "/Output/SearchLog-from-Data-Lake.csv" <Length> <Offset>

## 関連項目

- 他のツールを使用する同じチュートリアルを表示するには、ページの上部にあるタブ セレクターをクリックします。
- Complexed クエリを表示するには、次を参照してください。 [分析の web サイトを Azure Data Lake 分析を使用してログに記録](data-lake-analytics-analyze-weblogs.md)します。
- U SQL アプリケーションの開発開始するを参照してください。 [Data Lake Tools を使用して、Visual Studio の開発の U SQL スクリプト](data-lake-analytics-data-lake-tools-get-started.md)します。
- U SQL については、次を参照してください。 [Azure Data Lake 分析 U SQL 言語を使ってみる](data-lake-analytics-u-sql-get-started.md)します。
- 管理タスクを参照してください。 [管理 Azure Data Lake 分析 Azure ポータルを使用して](data-lake-analytics-manage-use-portal.md)します。
- Data Lake 分析の概要を取得するには、次を参照してください。 [Azure Data Lake 分析の概要](data-lake-analytics-overview.md)します。






