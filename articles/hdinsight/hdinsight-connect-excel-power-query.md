<properties
    pageTitle="Power Query を使用した Excel から Hadoop への接続 | Microsoft Azure"
    description="ビジネス インテリジェンス コンポーネントを活用し、HDInsight 上の Hadoop に格納されているデータに Power Query for Excel でアクセスする方法を説明します。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/25/2015"
    ms.author="jgao"/>


#Power Query を使用した Excel から Hadoop への接続

マイクロソフトのビッグ データ ソリューションの重要な特徴の 1 つに、Microsoft ビジネス インテリジェンス (BI) コンポーネントと Azure HDInsight の Hadoop クラスターとの統合があります。 この統合の主な例は、Microsoft Power Query for Excel アドインを使用して Hadoop クラスターと関連付けられたデータを格納する Azure Storage アカウントに Excel を接続する機能です。 この記事では、Power Query をセットアップして、HDInsight で管理される Hadoop クラスターに関連付けられたデータの照会に使用する方法を説明します。

> [AZURE.NOTE] この記事の手順では、Linux または Windows ベースの HDInsight クラスターで使用できますが、Windows は、クライアント ワークステーションに必要です。

## 前提条件

この記事を読み始める前に、次の項目を用意する必要があります。

- **HDInsight クラスター**します。 構成方法については、次を参照してください。 [Azure HDInsight の概要][hdinsight-get-started]します。
- **ワークステーション** Windows 7、Windows Server 2008 R2、または以降のオペレーティング システムを実行しています。
- **Office 2013 Professional Plus、Office 365 ProPlus、Excel 2013 Standalone、または Office 2010 Professional Plus**します。


## <a id="InstallPowerQuery"></a>Microsoft Power Query for Excel のインストール

Power Query を使うと、各種ソースから Microsoft Excel にデータをインポートして、そこで PowerPivot や Power View のような BI ツールを利用することができます。 特に、Power Query は、HDInsight クラスターで実行される Hadoop ジョブによって出力されたデータや生成されたデータをインポートすることができます。

Microsoft Power Query を使用して Excel for ダウンロード、 [Microsoft ダウンロード センター][powerquery-download] し、インストールします。

## <a id="ImportData"></a>HDInsight データを Excel にインポート

Power Query for Excel アドインを使うと、HDInsight クラスターから Excel にデータを簡単にインポートして、そこで PowerPivot や Power Map のような BI ツールを使用してデータの調査、分析、表示ができます。

**HDInsight クラスターから Excel にデータをインポートするには**

1. Excel を開きます。

2. 新しい空のブックを作成します。

3. クリックして、 **Power Query** ] メニューのをクリックして **から Azure**, 、] をクリックし、 **Microsoft Azure HDInsight から**です。

    ![HDI.PowerQuery.SelectHdiSource][image-hdi-powerquery-hdi-source]

    **注:** が表示されない場合、 **Power Query** ] メニューの [に移動 **ファイル** > **オプション** > **アドイン**, を選択し、 **COM アドイン** ドロップダウン リストから **管理** ページの下部にあるボックス。 選択、 **... 移動** ボタンをクリックし、Power Query for Excel アドインのボックスがオンになっていることを確認します。

    **注:** Power Query をクリックして、HDFS からデータをインポートすることもできます **その他のソース**します。

3.  **アカウント名**, をクラスターに関連付けられている Azure Blob ストレージ アカウントの名前を入力して、クリックして **OK**します。

4.  **アカウント キー**, を Blob ストレージ アカウントのキーを入力して、クリックして **保存**します。 (この操作が必要となるのは、このストアに最初にアクセスするときだけです)。

5.  **ナビゲーター** クエリ エディターの左側のウィンドウが Blob ストレージ コンテナーの名前をダブルクリックします。 既定で、コンテナー名はクラスター名と同じです。

6. 検索 **HiveSampleData.txt** で、 **名前** 列 (フォルダーのパスは **../hive/ウェアハウス/hivesampletable/**) を順にクリック **バイナリ** HiveSampleData.txt の左側にします。

    ![HDI.PowerQuery.ImportData][image-hdi-powerquery-importdata]

7. 列名を変更することもできます。 準備ができたら、クリックして **適用して閉じる**します。

    ![HDI.PowerQuery.ImportedTable][image-hdi-powerquery-imported-table]

## <a id="NextSteps"></a>次のステップ

この記事では、Power Query を使用して HDInsight から Excel にデータを取得する方法を学習しました。 同様に、Azure SQL Database に HDInsight からデータを取得することもできます。 また、HDInsight にデータをアップロードすることもできます。 詳細については、次の記事を参照してください。

* [Microsoft Hive ODBC ドライバーを使用した Excel から HDInsight への接続に関するページ][hdinsight-ODBC]
* [HDInsight へのデータのアップロードに関するページ][hdinsight-upload-data]

[hdinsight-ODBC]: hdinsight-connect-excel-hive-ODBC-driver.md
[hdinsight-get-started]: ../hdinsight-get-started.md
[hdinsight-upload-data]: hdinsight-upload-data.md

[image-hdi-powerquery-hdi-source]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.SelectHdiSource.png
[image-hdi-powerquery-importdata]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportData.png
[image-hdi-powerquery-imported-table]: ./media/hdinsight-connect-excel-power-query/HDI.PowerQuery.ImportedTable.PNG

[powerquery-download]: http://go.microsoft.com/fwlink/?LinkID=286689


