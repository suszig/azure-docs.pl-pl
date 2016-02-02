<properties
    pageTitle="HBase のチュートリアル: Hadoop での HBase の使用 | Microsoft Azure"
    description="HDInsight の Hadoop で Apache HBase を使用するには、この HBase チュートリアルの手順に従ってください。HBase シェルからテーブルを作成し、Hive を使用したクエリを実行します。"
    keywords="apache hbase,hbase,hbase shell,hbase tutorial"
    services="hdinsight"
    documentationCenter=""
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="12/02/2015"
    ms.author="jgao"/>




# HBase チュートリアル: HDInsight の Hadoop で Apache HBase を使用する

HDInsight で HBase クラスターを作成する方法、HBase テーブルを作成する方法、Hive を使用してテーブルを照会する方法について説明します。 HBase の概要については、次を参照してください。 [HDInsight HBase の概要 ][hdinsight-hbase-overview]します。

[AZURE.INCLUDE [hdinsight-azure-portal](../../includes/hdinsight-azure-portal.md)]

* [HBase のチュートリアル: HDInsight での Hadoop での Apache HBase の使用](hdinsight-hbase-tutorial-get-started.md)

> [AZURE.NOTE] HBase (バージョン 0.98.0) は、(Apache Hadoop および YARN 2.4.0 を基盤とする) HDInsight の HDInsight 3.1 クラスターでのみ使用できます。 バージョン情報を参照してください [HDInsight で提供される Hadoop クラスター バージョンの新機能ですか? ][hdinsight-versions]

次のビデオでも同じ内容を確認できます。

> [AZURE.VIDEO get-started-with-hbase-in-hdinsight]




**前提条件**

この HBase のチュートリアルを読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **ワークステーション** Visual Studio 2013: 手順については、次を参照してください。 [Visual Studio のインストール](http://msdn.microsoft.com/library/e2h7fzkw.aspx)します。

## HBase クラスターの作成

[AZURE.INCLUDE [provisioningnote](../../includes/hdinsight-provisioning.md)]

**Azure ポータルを使用して HBase クラスターを作成するには**


1. サインイン、 [Azure ポータルの ][azure-management-portal]します。
2. クリックして **新規** をクリックして左下にある **Data services**, 、**HDInsight**,  、**HBase**します。
    >[AZURE.NOTE] **[カスタム作成]** オプションも使用できます。
3. **クラスター名**、**クラスター サイズ**、HTTP ユーザーのパスワード、**ストレージ アカウント**を入力します。

    ![Create HBase cluster in HDInsight][img-hdinsight-hbase-cluster-quick-create]

    既定の HTTP ユーザー名は admin です。 名前は、[カスタム作成] オプションを使用してカスタマイズできます。

    既定の HBase 作成プロセスを使用する場合は、Azure ストレージ アカウントが必要になります。 参照してください [Azure ストレージ アカウント ][azure-create-storageaccount] を作成します。 [カスタム作成] オプションを選択すると、クラスターの作成プロセス中にストレージ アカウントを作成できます。
    > [AZURE.WARNING] HBase サービスの高可用性を実現するには、最低 **3 つ**のノードを含むクラスターを作成する必要があります。 これで 1 つのノードがダウンしても、HBase データ領域は他のノードで利用できます。

   > 学習目的で HBase を使用する場合は、コスト削減のため、クラスター サイズには必ず 1 を選択し、クラスターの使用後にクラスターを削除してください。

4. 右下にある **[HDInsight クラスターの作成]** をクリックして、HBase クラスターを作成します。

>[AZURE.NOTE] HBase クラスターを削除したら、同じ既定の BLOB コンテナーを使用して別の HBase クラスターを作成できます。 新しいクラスターでは、元のクラスターで作成した HBase テーブルを選択します。

## HBase シェルの使用

現在、2 つの方法で HBase にアクセスできます。 このセクションでは、HBase シェルを使用する方法について説明します。 次のセクションでは、.NET SDK を使用する方法について説明します。

多くの場合、データは次のような表形式で表示されます。

![hdinsight hbase tabular data][img-hbase-sample-data-tabular]

BigTable の実装である HBase では、同じデータが次のように表示されます。

![hdinsight hbase bigtable data][img-hbase-sample-data-bigtable]

次の手順を完了すると、この操作をよく理解できます。


**HBase シェルを使用するには**

1. RDP を使用して、HDInsight の HBase クラスターに接続します。 RDP の手順についてを参照してください。 [Azure ポータルの ][hdinsight-manage-portal]します。
2. RDP セッション内で、デスクトップにある **[Hadoop コマンド ライン]** ショートカットをクリックします。
3. 次のように HBase シェルを開きます。

        cd %HBASE_HOME%\bin
        hbase shell

4. 2 つの列ファミリを持つ HBase を作成します。

        create 'Contacts', 'Personal', 'Office'
        list

5. いくつかのデータを挿入します。

        put 'Contacts', '1000', 'Personal:Name', 'John Dole'
        put 'Contacts', '1000', 'Personal:Phone', '1-425-000-0001'
        put 'Contacts', '1000', 'Office:Phone', '1-425-000-0002'
        put 'Contacts', '1000', 'Office:Address', '1111 San Gabriel Dr.'
        scan 'Contacts'

    ![hdinsight hadoop hbase shell][img-hbase-shell]

6. 1 つの行を取得します。

        get 'Contacts', '1000'

    行は 1 行のみのため、スキャン コマンドを使用した場合と同じ結果が得られます。

    Hbase テーブル スキーマの詳細については、次を参照してください。 [HBase スキーマの設計 ][hbase-schema]します。 HBase コマンドについて [Apache HBase リファレンス ガイド ][hbase-quick-start]します。

6. シェルを終了します。

        exit


**Contacts HBase テーブルにデータを一括で読み込むには**

HBase では、いくつかの方法でテーブルにデータを読み込ことができます。 詳細については、次を参照してください。 [一括読み込みを行う](http://hbase.apache.org/book.html#arch.bulk.load)します。


サンプル データ ファイルがパブリック blob コンテナーにアップロードされて wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt します。 このデータ ファイルの内容は次のとおりです。

    8396    Calvin Raji     230-555-0191    230-555-0191    5415 San Gabriel Dr.
    16600   Karen Wu        646-555-0113    230-555-0192    9265 La Paz
    4324    Karl Xie        508-555-0163    230-555-0193    4912 La Vuelta
    16891   Jonn Jackson    674-555-0110    230-555-0194    40 Ellis St.
    3273    Miguel Miller   397-555-0155    230-555-0195    6696 Anchor Drive
    3588    Osa Agbonile    592-555-0152    230-555-0196    1873 Lion Circle
    10272   Julia Lee       870-555-0110    230-555-0197    3148 Rose Street
    4868    Jose Hayes      599-555-0171    230-555-0198    793 Crawford Street
    4761    Caleb Alexander 670-555-0141    230-555-0199    4775 Kentucky Dr.
    16443   Terry Chander   998-555-0171    230-555-0200    771 Northridge Drive

必要に応じて、自分でテキスト ファイルを作成し、そのファイルを自分のストレージ アカウントにアップロードできます。 手順については、次を参照してください。 [HDInsight ][hdinsight-upload-data]します。
> [AZURE.NOTE] この手順では、前回の手順で作成した Contacts HBase テーブルを使用します。

1. RDP セッション内で、デスクトップにある **[Hadoop コマンド ライン]** ショートカットをクリックします。
2. ディレクトリを変更します。

        cd %HBASE_HOME%\bin

3. 次のコマンドを実行して、データ ファイルをストア ファイルに変換し、Dimporttsv.bulk.output で指定された相対パスに格納します。

        hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.columns="HBASE_ROW_KEY,Personal:Name, Personal:Phone, Office:Phone, Office:Address" -Dimporttsv.bulk.output="/example/data/storeDataFileOutput" Contacts wasb://hbasecontacts@hditutorialdata.blob.core.windows.net/contacts.txt

4. /Example/data/storeDataFileOutput から HBase テーブルにデータをアップロードするには、次のコマンドを実行します。

        hbase org.apache.hadoop.hbase.mapreduce.LoadIncrementalHFiles /example/data/storeDataFileOutput Contacts

5. HBase シェルを開き、スキャン コマンドを使用して、テーブルの内容の一覧を表示することができます。

## クラスターの状態の確認

HDInsight の HBase には、クラスターを監視するための Web UI が付属します。 この Web UI を使用すると、統計情報やリージョンに関する情報を要求できます。

Web UI を開くには、RDP を使用してクラスターに接続した後、デスクトップ上の HMaster Info Web UI のショートカットをクリックするか、Web ブラウザーで次の URL を使用します。

    http://zookeeper[0-2]:60010/master-status

高可用性クラスターの場合は、Web UI をホストしている現在アクティブな HBase マスター ノードへのリンクがあります。



## Hive を使用して HBase テーブルを照会する

Hive を使用して HBase テーブルのデータを照会できます。 このセクションでは、HBase テーブルにマッピングする Hive テーブルを作成し、作成した Hive テーブルを使用して HBase テーブルのデータを照会します。

**クラスターのダッシュボードを開くには**

1. サインイン、 [Azure ポータルの ][azure-management-portal]します。
2. 左側のウィンドウで **[HDInsight]** をクリックします。 このチュートリアルの前のセクションで作成したクラスターを含め、クラスターが一覧表示されます。
3. Hive ジョブを実行するクラスター名をクリックします。
4. ページの下部にある **[クエリ コンソール]** をクリックし、クラスターのダッシュボードを開きます。 別のブラウザー タブで Web ページが開きます。
5. Hadoop ユーザー アカウントとパスワードを入力します。 既定のユーザー名は **admin** で、パスワードは作成処理中に入力したパスワードです。 新しいブラウザーのタブが開きます。
6. ページ上部の **[Hive エディター]** をクリックします。 次のような Hive エディターが開きます。

    ![HDInsight クラスターのダッシュボード][img-hdinsight-hbase-hive-editor]





























**Hive クエリを実行するには**

1. 次の HiveQL スクリプトを Hive エディターに入力し、**[送信]** をクリックして、HBase テーブルへの Hive テーブル マッピングを作成します。 ここで、HBase シェルを使用して、先ほど参照したサンプル テーブルが HBase に作成されたことを確認してから、このステートメントを実行してください。

        CREATE EXTERNAL TABLE hbasecontacts(rowkey STRING, name STRING, homephone STRING, officephone STRING, officeaddress STRING)
        STORED BY 'org.apache.hadoop.hive.hbase.HBaseStorageHandler'
        WITH SERDEPROPERTIES ('hbase.columns.mapping' = ':key,Personal:Name,Personal:Phone,Office:Phone,Office:Address')
        TBLPROPERTIES ('hbase.table.name' = 'Contacts');

    **[状態]** が **[完了]** に更新されるまで待機します。

2. 次の HiveQL スクリプトを Hive エディターに入力してから、**[送信]** をクリックします。 次のように Hive クエリが HBase テーブル内のデータを照会します。

        SELECT count(*) FROM hbasecontacts;

4. Hive クエリの結果を取得するには、ジョブが実行を終了するときに、**[ジョブ セッション]** ウィンドウで **[詳細の表示**] リンクをクリックします。 HBase テーブルに挿入したレコードは 1 つなので、ジョブの出力ファイルは 1 つだけになります。




**出力ファイルを参照するには**

1. クエリ コンソールで **[ファイル ブラウザー**] をクリックします。
2. HBase クラスターの既定のファイル システムとして使用する Azure ストレージ アカウントをクリックします。
3. HBase クラスター名をクリックします。 既定の Azure ストレージ アカウントのコンテナーでこのクラスター名が使用されます。
4. **[ユーザー]**、**[Admin]** と順にクリックします  (これが Hadoop ユーザー名です)。
6. Hive クエリ SELECT が実行された時刻と **[最終更新]** の時刻が一致しているジョブ名をクリックします。
4. **[stdout]** をクリックします。 ファイルを保存し、メモ帳でファイルを開きます。 出力ファイルは 1 つになります。

    ![HDInsight HBase Hive エディター ファイル ブラウザー][img-hdinsight-hbase-file-browser]

## .NET HBase REST API クライアント ライブラリの使用

GitHub から HBase REST API Client Library for .NET をダウンロードして HBase .NET SDK が使用できるように、プロジェクトをビルドする必要があります。 このタスクを実行する手順は、次のとおりです。

1. 新しい C# Visual Studio Windows デスクトップ コンソール アプリケーションを作成します。
2. **[ツール]** メニュー、**[Nuget パッケージ マネージャー]**、**[パッケージ マネージャー コンソール]** の順にクリックして NuGet のパッケージ マネージャー コンソールを開きます。
3. コンソールで次の NuGet コマンドを実行します。

        Install-Package Microsoft.HBase.Client

5. 次の **using** ステートメントをファイルの先頭に追加します。

        using Microsoft.HBase.Client;
        using org.apache.hadoop.hbase.rest.protobuf.generated;

6. **Main** 関数を次のコードに置き換えます。

     static void Main(string[] args)
     {
         string clusterURL = "https://<yourHBaseClusterName>.azurehdinsight.net";
         string hadoopUsername= "<yourHadoopUsername>";
         string hadoopUserPassword = "<yourHadoopUserPassword>";
    
         string hbaseTableName = "sampleHbaseTable";
    
         // Create a new instance of an HBase client.
         ClusterCredentials creds = new ClusterCredentials(new Uri(clusterURL), hadoopUsername, hadoopUserPassword);
         HBaseClient hbaseClient = new HBaseClient(creds);
    
         // Retrieve the cluster version
         var version = hbaseClient.GetVersion();
         Console.WriteLine("The HBase cluster version is " + version);
    
         // Create a new HBase table.
         TableSchema testTableSchema = new TableSchema();
         testTableSchema.name = hbaseTableName;
         testTableSchema.columns.Add(new ColumnSchema() { name = "d" });
         testTableSchema.columns.Add(new ColumnSchema() { name = "f" });
         hbaseClient.CreateTable(testTableSchema);
    
         // Insert data into the HBase table.
         string testKey = "content";
         string testValue = "the force is strong in this column";
         CellSet cellSet = new CellSet();
         CellSet.Row cellSetRow = new CellSet.Row { key = Encoding.UTF8.GetBytes(testKey) };
         cellSet.rows.Add(cellSetRow);
    
         Cell value = new Cell { column = Encoding.UTF8.GetBytes("d:starwars"), data = Encoding.UTF8.GetBytes(testValue) };
         cellSetRow.values.Add(value);
         hbaseClient.StoreCells(hbaseTableName, cellSet);
    
         // Retrieve a cell by its key.
         cellSet = hbaseClient.GetCells(hbaseTableName, testKey);
         Console.WriteLine("The data with the key '" + testKey + "' is: " + Encoding.UTF8.GetString(cellSet.rows[0].values[0].data));
         // with the previous insert, it should yield: "the force is strong in this column"
    
         //Scan over rows in a table. Assume the table has integer keys and you want data between keys 25 and 35.
         Scanner scanSettings = new Scanner()
         {
             batch = 10,
             startRow = BitConverter.GetBytes(25),
             endRow = BitConverter.GetBytes(35)
         };
    
         ScannerInformation scannerInfo = hbaseClient.CreateScanner(hbaseTableName, scanSettings);
         CellSet next = null;
         Console.WriteLine("Scan results");
    
         while ((next = hbaseClient.ScannerGetNext(scannerInfo)) != null)
         {
             foreach (CellSet.Row row in next.rows)
             {
                 Console.WriteLine(row.key + " : " + Encoding.UTF8.GetString(row.values[0].data));
             }
         }
    
         Console.WriteLine("Press ENTER to continue ...");
         Console.ReadLine();
     }

7. **Main** 関数の最初の 3 つの変数を設定します。
8. **F5** キーを押してアプリケーションを実行します。



## 次の手順

この HDInsight の HBase のチュートリアルでは、HBase クラスターの作成方法と、テーブルを作成してそのテーブルのデータを HBase シェルから表示する方法について学習しました。 また、Hive を使用して HBase テーブルのデータを照会する方法、HBase C# REST API を使用して HBase テーブルを作成し、テーブルからデータを取得する方法についても学習しました。

詳細については、次を参照してください。

- [HDInsight HBase の概要 ][hdinsight-hbase-overview]:
HBase は、Hadoop 上に構築された Apache オープン ソースの NoSQL データベースです。大量の非構造化データおよび半構造化データに対するランダム アクセスと強力な一貫性を実現します。
- [作成の HBase クラスターを Azure Virtual Network ][hdinsight-hbase-provision-vnet]:
アプリケーションが HBase と直接通信できるように、仮想ネットワーク統合を使用して、HBase クラスターをアプリケーションと同じ仮想ネットワークにデプロイできます。
- [HDInsight での HBase の構成のレプリケーション](hdinsight-hbase-geo-replication.md): 2 つの Azure データ センター間の HBase レプリケーションを構成する方法について説明します。
- [分析 Twitter のセンチメントを HBase を HDInsight ][hbase-twitter-sentiment]:
方法については、リアルタイム [センチメント分析](http://en.wikipedia.org/wiki/Sentiment_analysis) HDInsight の Hadoop クラスターで HBase を使用して、ビッグ データのです。


[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md 
[hdinsight-upload-data]: hdinsight-upload-data.md 
[hbase-reference]: http://hbase.apache.org/book.html#importtsv 
[hbase-schema]: http://0b4af6cdc2f0c5998459-c0245c5c937c5dedcca3f1764ecc9b2f.r43.cf2.rackcdn.com/9353-login1210_khurana.pdf 
[hbase-quick-start]: http://hbase.apache.org/book.html#quickstart 
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md 
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md 
[hdinsight-versions]: hdinsight-component-versioning.md 
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md 
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/ 
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/ 
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/ 
[azure-management-portal]: https://manage.windowsazure.com/ 
[azure-create-storageaccount]: http://azure.microsoft.com/documentation/articles/storage-create-storage-account/ 
[img-hdinsight-hbase-cluster-quick-create]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-quick-create.png 
[img-hdinsight-hbase-hive-editor]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-hive-editor.png 
[img-hdinsight-hbase-file-browser]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-file-browser.png 
[img-hbase-shell]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-shell.png 
[img-hbase-sample-data-tabular]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-tabular.png 
[img-hbase-sample-data-bigtable]: ./media/hdinsight-hbase-tutorial-get-started/hdinsight-hbase-contacts-bigtable.png 

