<properties
    pageTitle="Azure Data Factory を使ってみる"
    description="このチュートリアルでは、Azure HDInsight を使用してデータを変換するサンプル データ パイプラインを作成する方法を示します。"
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
    ms.topic="hero-article" 
    ms.date="11/02/2015"
    ms.author="spelluru"/>

# Azure Data Factory を使ってみる
> [AZURE.SELECTOR]
- [チュートリアルの概要](data-factory-build-your-first-pipeline.md)
- [Data Factory エディターの使用](data-factory-build-your-first-pipeline-using-editor.md)
- [PowerShell の使用](data-factory-build-your-first-pipeline-using-powershell.md)
- [Visual Studio の使用](data-factory-build-your-first-pipeline-using-vs.md)
- [Resource Manager テンプレートの使用](data-factory-build-your-first-pipeline-using-arm.md)

この記事では、Azure Data Factory を初めて構築する方法について説明します。 

> [AZURE.NOTE] この記事では、Azure Data Factory サービスの概念については説明しません。 サービスの詳細については、次を参照してください。、 [Azure Data Factory の概要](data-factory-introduction.md) 記事です。

## チュートリアルの概要
このチュートリアルでは、初めて Data Factory とパイプラインを使用するために必要な手順を示します。 パイプラインを作成し、必要なすべてのリソースを最初から指定します。

最初から作成せずにすばやく Data Factory のさまざまな機能を調べたい場合は、Azure ポータルで提供されているサンプルを使用できます。 参照してください [Azure Data Factory の更新: サンプルの配置を簡素化](http://azure.microsoft.com/blog/2015/04/24/azure-data-factory-update-simplified-sample-deployment/) Azure ポータルを使用して、ユース ケースに基づくサンプルを展開する方法をします。

## 前提条件
このチュートリアルを開始する前に、以下の前提条件を満たしている必要があります。

1.  **Azure サブスクリプション** -Azure サブスクリプションを取得していない場合は、ほんの数分で無料の試用アカウントを作成することができます。 参照してください、 [無料評価版](http://azure.microsoft.com/pricing/free-trial/) 無料の試用アカウントを取得する方法に関する記事です。

2.  **Azure ストレージ** – このチュートリアルでは、データを格納する Azure ストレージ アカウントを使用します。 Azure ストレージ アカウントを取得していない場合、 [ストレージ アカウントの作成](../storage-create-storage-account/#create-a-storage-account) 記事です。 ストレージ アカウントを作成した後は、ストレージにアクセスするために使用するアカウント キーを取得する必要があります。 参照してください [表示、コピーおよび再生成するストレージ アクセス キー](../storage-create-storage-account/#view-copy-and-regenerate-storage-access-keys)します。

## このチュートリアルの内容 
Azure Data Factory では、データ駆動型ワークフローとしてデータ移動タスクやデータ処理タスクを構成できます。 HDInsight を使用して毎月 Web ログを変換および分析する初めてのパイプラインを作成する方法を説明します。  

このチュートリアルでは、以下の手順を実施します。

1.  データ ファクトリを作成する
2.  次のリンクされたサービスを作成します。
    1.  **Azure ストレージ アカウント** – Azure ストレージ アカウントは、オンデマンド HDInsight クラスターで使用されるファイルの保管に使用します。
    2.  **オンデマンド HDInsight クラスター** – HDInsight クラスターにオンデマンドで変換およびデータを分析を開始します。
3.  出力データセットを作成します。 
4.  Hive スクリプトを実行して出力データセットに結果を格納するパイプラインを作成します。 Hive スクリプトは、まず、外部テーブルを作成し、Azure Blob Storage に格納されている未加工の Web ログ データを参照します。 次に、年と月で未加工データを分割します。

呼ばれる、最初のパイプライン **MyFirstPipeline**, 、Hive アクティビティを使用して、変換および HDInsight クラスターの一部としてデプロイされに格納される web ログ分析 **/HdiSamples/という名前/SampleLog/**します。 

![Diagram View](./media/data-factory-build-your-first-pipeline/diagram-view.png)

結果を Azure blob ストレージ コンテナーには、Hive スクリプトを実行した後: **データ/partitioneddata**します。

定義されている可用性、 **AzureBlobOutput** データセットでは、Hive アクティビティを実行する頻度を指定します。 このチュートリアルでは、これは毎月に設定されています。

## チュートリアルの Azure Storage を準備する
チュートリアルを開始する前に、チュートリアルに必要なファイルで Azure Storage を準備する必要があります。

1. 起動 **メモ帳** し、次の HQL スクリプトを貼り付けます。 この Hive スクリプトは、2 つの外部テーブルを作成します。 **WebLogsRaw** と **WebLogsPartitioned**します。 クリックして **ファイル** ] を選択し、メニューに **名前を付けて保存**します。 切り替えて、 **C:\adfgettingstarted** 、ハード ドライブ上のフォルダーです。 選択 **のすべてのファイル (*.*)* * を **ファイルの種類** フィールドです。 入力 **partitionweblogs.hql** の **ファイル名**します。 いることを確認、 **エンコード** ] ダイアログ ボックスの下部にあるフィールドに設定されている **ANSI**します。 ない場合は、設定 **ANSI**します。  
    
        set hive.exec.dynamic.partition.mode=nonstrict;
        
        DROP TABLE IF EXISTS WebLogsRaw; 
        CREATE TABLE WebLogsRaw (
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        LINES TERMINATED BY '\n' 
        tblproperties ("skip.header.line.count"="2");
        
        LOAD DATA INPATH '/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log' OVERWRITE INTO TABLE WebLogsRaw;
        
        DROP TABLE IF EXISTS WebLogsPartitioned ; 
        create external table WebLogsPartitioned (  
          date  date,
          time  string,
          ssitename string,
          csmethod  string,
          csuristem  string,
          csuriquery string,
          sport int,
          susername string,
          cipcsUserAgent string,
          csCookie string,
          csReferer string,
          cshost  string,
          scstatus  int,
          scsubstatus  int,
          scwin32status  int,
          scbytes int,
          csbytes int,
          timetaken int
        )
        partitioned by ( year int, month int)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
        STORED AS TEXTFILE 
        LOCATION '${hiveconf:partitionedtable}';
        
        INSERT INTO TABLE WebLogsPartitioned  PARTITION( year , month) 
        SELECT
          date,
          time,
          ssitename,
          csmethod,
          csuristem,
          csuriquery,
          sport,
          susername,
          cipcsUserAgent,
          csCookie,
          csReferer,
          cshost,
          scstatus,
          scsubstatus,
          scwin32status,
          scbytes,
          csbytes,
          timetaken,
          year(date),
          month(date)
        FROM WebLogsRaw
     
2. チュートリアル用に Azure Storage を準備するには:
    1. ダウンロード、 [最新バージョンの **AzCopy**](http://aka.ms/downloadazcopy), 、または [最新のプレビュー バージョン](http://aka.ms/downloadazcopypr)です。 参照してください [AzCopy を使用する方法](../storage/storage-use-azcopy.md) 資料については、ユーティリティを使用します。
    2. AzCopy をインストールした後は、コマンド プロンプトで次のコマンドを実行してシステム パスに追加できます。 
    
            set path=%path%;C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy           

    3. c:\adfgettingstarted フォルダーに移動し、次のコマンドを実行して Hive .HQL ファイルをストレージ アカウントにアップロードします。 置換 **StorageAccountName** 、ストレージ アカウントの名前と **ストレージ キー** をストレージ アカウント キー。

            AzCopy /Source:. /Dest:https://<StorageAccountName>.blob.core.windows.net/script /DestKey:<Storage Key>

        > [AZURE.NOTE] 上記のコマンドはという名前のコンテナーを作成 **スクリプト** で Azure Blob ストレージとコピー、 **partitionweblogs.hql** ファイルをローカル ドライブから blob コンテナーです。 
    >
    5. ファイルが正常にアップロードされると、AzCopy から次の出力が表示されます。
    
            Finished 1 of total 1 file(s).
            [2015/06/15 15:47:13] Transfer summary:
            -----------------
            Total files transferred: 1
            Transfer successfully:   1
            Transfer skipped:        0
            Transfer failed:         0
            Elapsed time:            00.00:00:01

以下の手順を実行します。

- クリックして [Data Factory エディターを使用して](data-factory-build-your-first-pipeline-using-editor.md) Azure Classic Portal の一部である Data Factory エディターを使用して、チュートリアルを実行する上部にあるリンクです。
- クリックして [PowerShell を使用した](data-factory-build-your-first-pipeline-using-powershell.md) Azure PowerShell を使用して、チュートリアルを実施する上部にあるリンクです。
- クリックして [Visual Studio を使って](data-factory-build-your-first-pipeline-using-vs.md) Visual Studio を使用して、チュートリアルを実施する上部にあるリンクです。 



