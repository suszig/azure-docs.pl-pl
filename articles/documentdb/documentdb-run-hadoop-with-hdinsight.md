<properties 
    pageTitle="DocumentDB と HDInsight を使用した Hadoop ジョブの実行 | Microsoft Azure" 
    description="DocumentDB と Azure HDInsight を使用してシンプルな Hive、Pig、および MapReduce ジョブを実行する方法を説明します。"
    services="documentdb" 
    authors="AndrewHoh" 
    manager="jhubbard" 
    editor="mimig"
    documentationCenter=""/>


<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="java" 
    ms.topic="article" 
    ms.date="10/28/2015" 
    ms.author="anhoh"/>

#<a name="DocumentDB-HDInsight"></a>DocumentDB と HDInsight を使用した Hadoop ジョブの実行

このチュートリアルを実行する方法を示します [Apache Hive][apache-hive], 、[Apache Pig][apache-pig], 、および [Apache Hadoop][apache-hadoop] DocumentDB の Hadoop コネクタでは、Azure HDInsight で MapReduce ジョブです。 DocumentDB の Hadoop コネクタを使用すると、DocumentDB が Hive、Pig、および MapReduce の各ジョブに対してソースとシンクの両方として機能します。 このチュートリアルでは、Hadoop ジョブのデータ ソースと出力先の両方に DocumentDB を使用します。 

このチュートリアルを完了すると、次の項目について説明できるようになります。

- Hive、Pig、または MapReduce ジョブを使用して DocumentDB からどのようにしてデータを読み込むか。
- Hive、Pig、または MapReduce ジョブを使用して DocumentDB にどのようにしてデータを格納するか。

まずは、次のビデオを視聴することをお勧めします。このビデオでは、DocumentDB と HDInsight を使用して Hive ジョブがひととおり実行されます。

> [AZURE.VIDEO use-azure-documentdb-hadoop-connector-with-azure-hdinsight]

その後で、この記事に戻ってください。この記事では、DocumentDB データに対して分析ジョブを実行する方法について詳しく説明します。

> [AZURE.TIP] このチュートリアルでは、Apache Hadoop、Hive、pig を使用した経験があることを前提としています。 アクセスをお勧め Apache Hadoop、Hive および Pig に慣れていない場合、 [Apache Hadoop のドキュメント][apache-hadoop-doc]します。 さらに、このチュートリアルでは、DocumentDB の使用経験があり、DocumentDB アカウントを持っていることを想定しています。 DocumentDB に慣れていない、または DocumentDB アカウントがないを確認してください、 [概要][getting-started] ページです。

チュートリアルを完了する時間がなく、Hive、Pig、および MapReduce の完全なサンプル PowerShell スクリプトが必要な場合は、 問題ではなく、全員で共有 [ここ][documentdb-hdinsight-samples]します。 ダウンロードには、これらのサンプルの hql、pig、および java の各ファイルも含まれています。

## <a name="NewestVersion"></a>最新バージョン

<table border='1'>
    <tr><th>Hadoop コネクタのバージョン</th>
        <td>1.1.0</td></tr>
    <tr><th>スクリプト URI</th>
        <td>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v03.ps1</td></tr>
    <tr><th>更新日時</th>
        <td>07/20/2015</td></tr>
    <tr><th>サポートされる HDInsight のバージョン</th>
        <td>3.1、3.2</td></tr>
    <tr><th>変更履歴</th>
        <td>DocumentDB Java SDK 1.1.0 への更新</br>
            Removed additional output parameter for custom indexing paths</br>
            Added optional parameter for custom string precision (-1 by default)</br>
            6/11/2015</br>
            Fixed connector's compatability with the <a href="https://www.microsoft.com/download/details.aspx?id=40886">Microsoft Hive ODBC ドライバー</a></br>
            Added ability to change output collection offer type (S3 offer by default)</br>
        </td></tr>
</table>

## <a name="Prerequisites"></a>前提条件
このチュートリアルの手順を実行する前に、次のものを備えておく必要があります。

- DocumentDB アカウント、データベース、およびドキュメントが含まれているコレクション 詳細については、次を参照してください。 [DocumentDB の概要][getting-started]します。 サンプル データを使用して DocumentDB アカウントをインポート、 [DocumentDB インポート ツール][documentdb-import-data]します。
- スループット。 HDInsight の読み取りと書き込みは、コレクションに割り当てられた要求単位に達するまでカウントされます。 詳細については、次を参照してください。 [スループット、要求単位、およびデータベース操作を準備][documentdb-manage-throughput]します。
- 各出力コレクションに含まれる追加のストアド プロシージャ用の容量。 ストアド プロシージャは、生成されたドキュメントを転送するために使用されます。 詳細については、次を参照してください。 [コレクションとプロビジョニング済みスループット][documentdb-manage-document-storage]します。
- Hive、Pig、または MapReduce のジョブから生成されたドキュメント用の容量。 詳細については、次を参照してください。 [DocumentDB の容量とパフォーマンス][documentdb-manage-collections]します。
- [*オプション*] 追加コレクション用の容量。 詳細については、次を参照してください。 [ドキュメント ストレージとインデックス オーバーヘッドをプロビジョニング][documentdb-manage-document-storage]します。
    
> [AZURE.WARNING] ジョブのいずれかで新しいコレクションの作成を回避するために、stdout に結果を印刷、出力を WASB コンテナーに保存したり既存のコレクションを指定できます。 コレクション内、新しいドキュメントを作成、既存のコレクションを指定する場合とで競合する場合は、既存のドキュメントは影響のみ *id*します。 **コネクタは、id の競合既存のドキュメントを自動的に上書きします。**します。 この機能を無効にするには、upsert オプションを false に設定します。 upsert が false に設定された状態で競合が発生すると、Hadoop ジョブは失敗し、ID 競合エラーが報告されます。

## <a name="CreateStorage"></a>手順 1: Azure のストレージ アカウントを作成する

> [AZURE.IMPORTANT] 場合する **既に** Azure ストレージ アカウントを所有していてそのアカウント内に新しい blob コンテナーを作成したいにスキップできます [手順 2: カスタマイズされた HDInsight クラスターの作成](#ProvisionHDInsight)します。

Azure HDInsight では、データの格納に Azure BLOB ストレージを使用します。 呼び出された *WASB* または *Azure ストレージ - Blob*します。 WASB は、HDFS を Azure BLOB ストレージ上で Microsoft が実装したものです。 詳細については、次を参照してください。 [Azure Blob ストレージの使用 HDInsight][hdinsight-storage]します。

HDInsight クラスターをプロビジョニングするときは、Azure ストレージ アカウントを指定します。 HDFS と同様、このアカウントの特定の BLOB ストレージ コンテナーが、既定のファイル システムとして設定されます。 既定では、HDInsight クラスターは、指定されたストレージ アカウントと同じデータ センターにプロビジョニングされます。

**Azure ストレージ アカウントを作成するには**

1. サインイン、 [Azure クラシック ポータル][azure-classic-portal]します。
    
    > [AZURE.NOTE] Azure HDInsight は、Azure DocumentDB では、Microsoft Azure ポータルにしか Azure 旧ポータルでがサポートされています。

2. クリックして **+ 新規** 、左下隅のをポイント **DATA SERVICES**, 、] をポイント **ストレージ**, 、順にクリック **簡易作成**します。
    ![Azure クラシック ポータルを使用する簡易作成新しいストレージ アカウントを設定します。][image-storageaccount-quickcreate]

3. 入力、 **URL**, を選択、 **場所** と **レプリケーション** 値、およびクリック **ストレージ アカウントの作成**します。 アフィニティ グループはサポートされていません。 
    
    新しいストレージ アカウントがストレージ一覧に表示されます。

    > [AZURE.IMPORTANT] 最適なパフォーマンスを同じ Azure リージョンにストレージ アカウント、HDInsight クラスター、および DocumentDB アカウントがあるになっていることを確認します。 次の 3 つのすべてのサービスをサポートしている azure リージョンは:  **東アジア**, 、**東南アジア**, 、**北ヨーロッパ**, 、**西ヨーロッパ**, 、**米国東部**, 、および **米国西部**します。

4. まで待って、 **ステータス** にアカウントを変更、新しい記憶域の **オンライン**します。

## <a name="ProvisionHDInsight"></a>手順 2: カスタマイズした HDInsight クラスターを作成する
このチュートリアルでは、Azure クラシック ポータルからスクリプト アクションを使用して、HDInsight クラスターをカスタマイズします。 このチュートリアルでは、カスタマイズされたクラスターの作成に、Azure クラシック ポータルを使用します。 PowerShell コマンドレットまたは HDInsight .NET SDK を使用する方法の手順については、チェック アウト、 [をカスタマイズする HDInsight クラスターの Script Action を使って][hdinsight-custom-provision] 記事です。

1. サインイン、 [Azure クラシック ポータル][azure-classic-portal]します。 前の手順で既にサインインしている場合もあります。

2. をクリックして **+ 新規** をクリックして、ページの下部にある **DATA SERVICES**, 、] をクリックして **HDINSIGHT**, 、] をクリックし、 **カスタム作成**します。

3.  **クラスターの詳細** ページで入力、または、次の値を選択します。

    ![Hadoop HDInsight の初期クラスターの詳細を提供][image-customprovision-page1]

    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>クラスター名</td><td>クラスターの名前を指定します。<br/>
            DNS 名の最初と最後の文字は英数字にする必要があります。それ以外ではダッシュ (-) を使用できます。<br/>
            3 ～ 63 文字のクラスター名を入力できます。</td></tr>
        <tr><td>サブスクリプション名</td>
            <td>1 つ以上の Azure サブスクリプションがある場合からストレージ アカウントに対応するサブスクリプションを選択します。 <strong>手順 1.</strong>します。 </td></tr>
        <tr><td>クラスターの種類</td>
            <td>クラスターの種類を選択 <strong>Hadoop</strong>します。</td></tr>
        <tr><td>オペレーティング システム</td>
            <td>オペレーティング システムを選択 <strong>Windows Server 2012 R2 Datacenter</strong>します。</td></tr>
        <tr><td>HDInsight のバージョン</td>
            <td>バージョンを選択します。 </br>選択 <Strong>HDInsight バージョン 3.1</strong>します。</td></tr>
        </table>

    <p>表に示されている値を入力または選択し、右矢印をクリックします。</p>

4.  **クラスターの構成** ページで入力するか、次の値を選択します。

    <table border="1">
    <tr><th>名前</th><th>値</th></tr>
    <tr><td>データ ノード</td><td>デプロイするデータ ノードの数です。 </br>HDInsight のデータ ノードはパフォーマンスと料金の両方に関連していることに注意してください。</td></tr>
    <tr><td>リージョン/仮想ネットワーク</td><td>新たに作成したのと同じリージョンを選択して <strong>ストレージ アカウント</strong> と <strong>DocumentDB アカウント</strong>します。 </br> HDInsight は、同じリージョンに配置されたストレージ アカウントを必要とします。 これ以後の構成作業では、ここで指定した地域と同じリージョンにあるストレージ アカウントしか選択できません。</td></tr>
    </table>
    
    右矢印をクリックします。

5.  **クラスター ユーザーの構成** ページで、次の値を指定します。

    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>ユーザー名</td>
            <td>HDInsight クラスターのユーザー名を指定します。</td></tr>
        <tr><td>パスワード/パスワードの確認</td>
            <td>HDInsight クラスターのユーザー パスワードを指定します。</td></tr>
    </table>
    
    右矢印をクリックします。
    
6.  **ストレージ アカウント** ページで、次の値を指定します。

    ![HDInsight Hadoop クラスターに対してストレージ アカウントを指定][image-customprovision-page4]

    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>ストレージ アカウント</td>
            <td>HDInsight クラスターの既定のファイル システムとして使用する Azure ストレージ アカウントを指定します。 3 つのオプション [既存のストレージを使用する]、[新しいストレージを作成する]、[別のサブスクリプションのストレージを使用する] のいずれかを選択できます。</br></br>
            選択してください <strong>既存のストレージを使用する</strong>です。
            </td>
            </td></tr>
        <tr><td>アカウント名</td>
            <td>
             <strong>アカウント名</strong>, で作成したアカウントを選択して <strong>手順 1.</strong>します。 ボックスの一覧には、クラスターをプロビジョニングする対象として選択したデータ センターと同じデータ センター内の同じ Azure サブスクリプション下にあるストレージ アカウントのみが表示されます。
            </td></tr>
        <tr><td>既定のコンテナー</td>
            <td>ストレージ アカウントの既定のコンテナーを指定します。既定のコンテナーは、HDInsight クラスターの既定のファイル システムとして使用されます。 選択した場合 <strong>既存のストレージを使用する</strong> の <strong>ストレージ アカウント</strong> フィールドが既定では、クラスター名と同じ名前のコンテナーは、そのアカウントに既存のコンテナーが作成されません。 クラスター名と同じ名前のコンテナーが既に存在する場合は、コンテナー名に連番が付加されます。
        </td></tr>
        <tr><td>追加のストレージ アカウント</td>
            <td>HDInsight は複数のストレージ アカウントをサポートします。 クラスターで使用できる追加ストレージ アカウントの数に制限はありません。 ただし、Azure クラシック ポータルを使用してクラスターを作成する場合は、UI の制約により 7 が上限になります。 ここで指定した追加のストレージ アカウントのそれぞれに対する [ストレージ アカウント] ページがウィザードに追加され、そこでアカウント情報を指定できます。</td></tr>
    </table>

    右矢印をクリックします。

7.  **Script Action** ] ページで [ **[スクリプト アクションの追加** 、クラスターが作成される際に、クラスターのカスタマイズを実行する PowerShell スクリプトに関する詳細を指定します。 PowerShell スクリプトによって、クラスターの作成中に DocumentDB Hadoop コネクタが HDInsight クラスターにインストールされます。
    
    ![HDInsight クラスターをカスタマイズするスクリプト操作を構成][image-customprovision-page5]

    <table border='1'>
        <tr><th>プロパティ</th><th>値</th></tr>
        <tr><td>名前</td>
            <td>スクリプト アクションの名前を指定します。</td></tr>
        <tr><td>スクリプト URI</td>
            <td>クラスターのカスタマイズのために呼び出されるスクリプトへの URI を指定します。</br></br>
            次の値を入力してください。 </br> <strong>https://portalcontent.blob.core.windows.net/scriptaction/documentdb-hadoop-installer-v03.ps1</strong>します。</td></tr>
        <tr><td>ノードの種類</td>
            <td>カスタマイズ スクリプトが実行されるノードを指定します。 選択する <b>のすべてのノード</b>, 、<b>ヘッド ノードのみ</b>, 、または <b>ワーカー ノード</b> のみです。</br></br>
            選択してください <strong>のすべてのノード</strong>します。</td></tr>
        <tr><td>パラメーター</td>
            <td>スクリプトで必要な場合は、パラメーターを指定します。</br></br>
            <strong>パラメーターは必要ありません</strong>します。</td></tr>
    </table>

    チェック マークをクリックして、クラスターの作成を完了します。

## <a name="InstallCmdlets"></a>手順 3: Azure PowerShell をインストールおよび構成する

1. Azure PowerShell をインストールします。 手順を参照して [ここ][powershell-install-configure]します。

    > [AZURE.NOTE] また、Hive クエリに対して HDInsight のオンライン Hive エディターを使用することができます。 これを行うにサインイン、 [Azure Classic Portal][azure-classic-portal], 、] をクリックして **HDInsight** 左側で、HDInsight クラスターの一覧を表示します。 クリックして、Hive クエリを実行するクラスターをクリックして **クエリ コンソール**します。

2. Azure PowerShell Integrated Scripting Environment を開きます。
    - Windows 8 または Windows Server 2012 以降を実行しているコンピューターでは、組み込みの検索機能を使用できます。 スタート画面から次のように入力します。 **powershell ise** ] をクリック **Enter**します。 
    - Windows 8 よりも前、または Windows Server 2012 より前のバージョンを実行しているコンピューターでは、[スタート] メニューを使用します。 [スタート] メニューから次のように入力します。 **コマンド プロンプト** 検索ボックスでクリックし、結果の一覧でクリック **コマンド プロンプト**します。 コマンド プロンプトで入力 **powershell_ise** ] をクリック **Enter**します。

3. Azure アカウントを追加します。
    1. コンソール ウィンドウで次のように入力します。 **Add-azureaccount** ] をクリック **Enter**します。 
    2. Azure サブスクリプションに関連付けられている電子メール アドレスを入力し、をクリックして **続行**します。 
    3. Azure サブスクリプションのパスワードを入力します。 
    4. クリックして **サインイン**します。

4. 次の図は、Azure PowerShell Scripting Environment の重要な要素を示しています。 

    ![図: Azure PowerShell][azure-powershell-diagram]

## <a name="RunHive"></a>手順 4: DocumentDB と HDInsight を使用して Hive ジョブを実行する

> [AZURE.IMPORTANT] < > で示されたすべての変数は、構成設定を使用して入力する必要があります。

1. PowerShell スクリプト ウィンドウで次の変数を設定します。

        # Provide Azure subscription name, the Azure Storage account and container that is used for the default HDInsight file system.
        $subscriptionName = "<SubscriptionName>"
        $storageAccountName = "<AzureStorageAccountName>"
        $containerName = "<AzureStorageContainerName>"

        # Provide the HDInsight cluster name where you want to run the Hive job.
        $clusterName = "<HDInsightClusterName>"

2. 
    <p>クエリ文字列の作成から始めましょう。 作成する Hive クエリでは、DocumentDB コレクションからすべてのドキュメントのシステム生成のタイムスタンプ (_ts) と一意の ID (_rid) を取得し、すべてのドキュメントを分単位で集計して、その結果を新しい DocumentDB コレクションに格納します。 </p>

    <p>まず、DocumentDB コレクションから Hive テーブルを作成します。 次のコード スニペットを PowerShell スクリプト ウィンドウに追加 <strong>後</strong> 1 から始まっているコード スニペットです。 _ts および _rid に合わせてドキュメントをトリミングするためのオプションの DocumentDB.query パラメーターが含まれていることを確認してください。 </p>

    > [AZURE.NOTE] **DocumentDB.inputCollections の名前付けが誤りではありません。** 次のように、複数のコレクションを 1 つの入力として追加することができます。 </br>
    '*DocumentDB.inputCollections*'='*\ < DocumentDB 入力コレクション名 1 >*,、*\ < DocumentDB 入力コレクションの名前の 2 \ >*' </br> コレクション名は、間にスペースを入れずにコンマだけで区切ります。


        # Create a Hive table using data from DocumentDB. Pass DocumentDB the query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "drop table DocumentDB_timestamps; "  + 
                            "create external table DocumentDB_timestamps(id string, ts BIGINT) "  +
                            "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' "  +
                            "tblproperties ( " + 
                                "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                "'DocumentDB.key' = '<DocumentDB Primary Key>', " +
                                "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                "'DocumentDB.inputCollections' = '<DocumentDB Input Collection Name>', " +
                                "'DocumentDB.query' = 'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "
 
3.  次に、出力コレクション用に Hive テーブルを作成します。 出力ドキュメントのプロパティは、月、日、時間、分、および発生した合計回数です。

    > [AZURE.NOTE] **、Documentdb.outputcollections という名前誤りではありません。** 次のように、複数のコレクションを 1 つの出力として追加することができます。 </br>
    '*DocumentDB.outputCollections*'='*\ < DocumentDB 出力コレクション名 1 >*,、*\ < 2 \ という名前の出力の DocumentDB コレクション >*' </br> コレクション名は、間にスペースを入れずにコンマだけで区切ります。 </br></br>
    ドキュメントは複数のコレクションに対してラウンドロビン形式で分散されます。 ドキュメントの 1 つ目のバッチが 1 つのコレクションに格納され、2 つ目のバッチが次のコレクションに格納されて、以降、同様に処理されます。

        # Create a Hive table for the output data to DocumentDB.
        $queryStringPart2 = "drop table DocumentDB_analytics; " +
                              "create external table DocumentDB_analytics(Month INT, Day INT, Hour INT, Minute INT, Total INT) " +
                              "stored by 'com.microsoft.azure.documentdb.hive.DocumentDBStorageHandler' " + 
                              "tblproperties ( " + 
                                  "'DocumentDB.endpoint' = '<DocumentDB Endpoint>', " +
                                  "'DocumentDB.key' = '<DocumentDB Primary Key>', " +  
                                  "'DocumentDB.db' = '<DocumentDB Database Name>', " +
                                  "'DocumentDB.outputCollections' = '<DocumentDB Output Collection Name>' ); "

4. 最後は、ドキュメントを月単位、日単位、時間単位、および分単位で集計し、結果を出力 Hive テーブルに格納します。

        # GROUP BY minute, COUNT entries for each, INSERT INTO output Hive table.
        $queryStringPart3 = "INSERT INTO table DocumentDB_analytics " +
                              "SELECT month(from_unixtime(ts)) as Month, day(from_unixtime(ts)) as Day, " +
                              "hour(from_unixtime(ts)) as Hour, minute(from_unixtime(ts)) as Minute, " +
                              "COUNT(*) AS Total " +
                              "FROM DocumentDB_timestamps " +
                              "GROUP BY month(from_unixtime(ts)), day(from_unixtime(ts)), " +
                              "hour(from_unixtime(ts)) , minute(from_unixtime(ts)); "

5. 次のスクリプト スニペットを追加して、前のクエリから Hive ジョブ定義を作成します。

        # Create a Hive job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $hiveJobDefinition = New-AzureHDInsightHiveJobDefinition -Query $queryString

    -File スイッチを使用して、HDFS 上の HiveQL スクリプト ファイルを指定することもできます。

6. 次のスニペットを追加して、開始時刻を格納し、Hive ジョブを送信します。

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $hiveJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $hiveJobDefinition

7. 次のスニペットを追加して、Hive ジョブが完了するのを待機します。

        # Wait for the Hive job to complete.
        Wait-AzureHDInsightJob -Job $hiveJob -WaitTimeoutInSeconds 3600

8. 次のスニペットを追加して、標準出力および開始時刻と終了時刻を表示します。

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $hiveJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green

9. **実行** 作成できたスクリプトです。 **クリックして** 緑色の実行] ボタンをクリックします。

10. 結果を確認します。 サインイン、 [Azure ポータル][azure-portal]します。 
    1. 左側ウィンドウの <strong>[参照]</strong> をクリックします。 </br>
    2. 参照ウィンドウの右上にある <strong>[すべて]</strong> をクリックします。 </br>
    3. <strong>[DocumentDB アカウント]</strong> を見つけてクリックします。 </br>
    4. 次に、検索、 <strong>DocumentDB アカウント</strong>, 、し <strong>DocumentDB データベース</strong> と <strong>DocumentDB コレクション</strong> 、Hive クエリで指定した出力コレクションに関連付けられています。</br>
    5. 最後に、<strong>[開発者ツール]</strong> の下にある <strong>[ドキュメント エクスプローラー]</strong> をクリックします。</br></p>

    Hive クエリの結果が表示されます。

    ![Hive クエリの結果][image-hive-query-results]

## <a name="RunPig"></a>手順 5: DocumentDB と HDInsight を使用して Pig ジョブを実行する

> [AZURE.IMPORTANT] < > で示されたすべての変数は、構成設定を使用して入力する必要があります。

1. PowerShell スクリプト ウィンドウで次の変数を設定します。

        # Provide Azure subscription name.
        $subscriptionName = "Azure Subscription Name"

        # Provide HDInsight cluster name where you want to run the Pig job.
        $clusterName = "Azure HDInsight Cluster Name"

2. <p>クエリ文字列の作成から始めましょう。 作成する Pig クエリでは、DocumentDB コレクションからすべてのドキュメントのシステム生成のタイムスタンプ (_ts) と一意の ID (_rid) を取得し、すべてのドキュメントを分単位で集計して、その結果を新しい DocumentDB コレクションに格納します。</p>
    <p>まず、DocumentDB から HDInsight にドキュメントを読み込みます。 次のコード スニペットを PowerShell スクリプト ウィンドウに追加 <strong>後</strong> 1 から始まっているコード スニペットです。 _ts および _rid に合わせてドキュメントをトリミングするためのオプションの DocumentDB.query パラメーターが DocumentDB クエリに追加されていることを確認してください。</p>

    > [AZURE.NOTE] はい、入力として複数のコレクションを追加できるようにします。 </br>
    '*\ < DocumentDB 入力コレクション名 1 >*,、*\ < DocumentDB 入力コレクションの名前の 2 \ >*'</br> コレクション名は、間にスペースを入れずにコンマだけで区切ります。 </b>

    ドキュメントは複数のコレクションに対してラウンドロビン形式で分散されます。 ドキュメントの 1 つ目のバッチが 1 つのコレクションに格納され、2 つ目のバッチが次のコレクションに格納されて、以降、同様に処理されます。

        # Load data from DocumentDB. Pass DocumentDB query to filter transferred data to _rid and _ts.
        $queryStringPart1 = "DocumentDB_timestamps = LOAD '<DocumentDB Endpoint>' USING com.microsoft.azure.documentdb.pig.DocumentDBLoader( " +
                                                        "'<DocumentDB Primary Key>', " +
                                                        "'<DocumentDB Database Name>', " +
                                                        "'<DocumentDB Input Collection Name>', " +
                                                        "'SELECT r._rid AS id, r._ts AS ts FROM root r' ); "

3.  次に、ドキュメントを月単位、日単位、時間単位、分単位、および発生した合計回数で集計します。

        # GROUP BY minute and COUNT entries for each.
        $queryStringPart2 = "timestamp_record = FOREACH DocumentDB_timestamps GENERATE `$0#'id' as id:int, ToDate((long)(`$0#'ts') * 1000) as timestamp:datetime; " +
                            "by_minute = GROUP timestamp_record BY (GetYear(timestamp), GetMonth(timestamp), GetDay(timestamp), GetHour(timestamp), GetMinute(timestamp)); " +
                            "by_minute_count = FOREACH by_minute GENERATE FLATTEN(group) as (Year:int, Month:int, Day:int, Hour:int, Minute:int), COUNT(timestamp_record) as Total:int; "

4. 最後に、その結果を新しい出力コレクションに格納します。

    > [AZURE.NOTE] はい、出力として複数のコレクションを追加できるようにします。 </br>
    ' \ < DocumentDB 出力コレクション名 1 > \ < 2 \ という名前の出力の DocumentDB コレクション >'</br> コレクション名は、間にスペースを入れずにコンマだけで区切ります。</br>
    ドキュメントは複数のコレクションに対してラウンドロビン形式で分散されます。 ドキュメントの 1 つ目のバッチが 1 つのコレクションに格納され、2 つ目のバッチが次のコレクションに格納されて、以降、同様に処理されます。

        # Store output data to DocumentDB.
        $queryStringPart3 = "STORE by_minute_count INTO '<DocumentDB Endpoint>' " +
                            "USING com.microsoft.azure.documentdb.pig.DocumentDBStorage( " +
                                "'<DocumentDB Primary Key>', " +
                                "'<DocumentDB Database Name>', " +
                                "'<DocumentDB Output Collection Name>'); "

5. 次のスクリプト スニペットを追加して、前のクエリから Pig ジョブ定義を作成します。

        # Create a Pig job definition.
        $queryString = $queryStringPart1 + $queryStringPart2 + $queryStringPart3
        $pigJobDefinition = New-AzureHDInsightPigJobDefinition -Query $queryString -StatusFolder $statusFolder

    -File スイッチを使用して、HDFS 上の Pig スクリプト ファイルを指定することもできます。

6. 次のスニペットを追加して、開始時刻を格納し、Pig ジョブを送信します。

        # Save the start time and submit the job to the cluster.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $pigJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $pigJobDefinition

7. 次のスニペットを追加して、Pig ジョブが完了するのを待機します。

        # Wait for the Pig job to complete.
        Wait-AzureHDInsightJob -Job $pigJob -WaitTimeoutInSeconds 3600

8. 次のスニペットを追加して、標準出力および開始時刻と終了時刻を表示します。

        # Print the standard error, the standard output of the Hive job, and the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $pigJob.JobId -StandardOutput
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green
        
9. **実行** 作成できたスクリプトです。 **クリックして** 緑色の実行] ボタンをクリックします。

10. 結果を確認します。 サインイン、 [Azure ポータル][azure-portal]します。 
    1. 左側ウィンドウの <strong>[参照]</strong> をクリックします。 </br>
    2. 参照ウィンドウの右上にある <strong>[すべて]</strong> をクリックします。 </br>
    3. <strong>[DocumentDB アカウント]</strong> を見つけてクリックします。 </br>
    4. 次に、検索、 <strong>DocumentDB アカウント</strong>, 、し <strong>DocumentDB データベース</strong> と <strong>DocumentDB コレクション</strong> 、Pig クエリで指定した出力コレクションに関連付けられています。</br>
    5. 最後に、<strong>[開発者ツール]</strong> の下にある <strong>[ドキュメント エクスプローラー]</strong> をクリックします。</br></p>

    Pig クエリの結果が表示されます。

    ![Pig クエリの結果][image-pig-query-results]

## <a name="RunMapReduce"></a>手順 6: DocumentDB と HDInsight を使用して MapReduce ジョブを実行する

1. PowerShell スクリプト ウィンドウで次の変数を設定します。
        
        $subscriptionName = "<SubscriptionName>"   # Azure subscription name
        $clusterName = "<ClusterName>"             # HDInsight cluster name
        
2. DocumentDB コレクションから各ドキュメント プロパティの発生回数を集計する MapReduce ジョブを実行します。 このスクリプト スニペットを追加 **後** 上記のスニペットです。

        # Define the MapReduce job.
        $TallyPropertiesJobDefinition = New-AzureHDInsightMapReduceJobDefinition -JarFile "wasb:///example/jars/TallyProperties-v01.jar" -ClassName "TallyProperties" -Arguments "<DocumentDB Endpoint>","<DocumentDB Primary Key>", "<DocumentDB Database Name>","<DocumentDB Input Collection Name>","<DocumentDB Output Collection Name>","<[Optional] DocumentDB Query>"

    > [AZURE.NOTE] Tallyproperties-v01.jar は-しています、DocumentDB Hadoop コネクタのカスタム インストールします。

3. 次のコマンドを追加して、MapReduce ジョブを送信します。

        # Save the start time and submit the job.
        $startTime = Get-Date
        Select-AzureSubscription $subscriptionName
        $TallyPropertiesJob = Start-AzureHDInsightJob -Cluster $clusterName -JobDefinition $TallyPropertiesJobDefinition | Wait-AzureHDInsightJob -WaitTimeoutInSeconds 3600  

    MapReduce ジョブ定義に加えて、MapReduce ジョブを実行する HDInsight クラスター名、および資格情報も指定します。 Start-AzureHDInsightJob は非同期呼び出しです。 ジョブの完了を確認するには、使用、 *Wait-azurehdinsightjob* コマンドレットです。

4. 次のコマンドを追加して、MapReduce ジョブの実行中に発生したエラーを確認します。    
    
        # Get the job output and print the start and end time.
        $endTime = Get-Date
        Get-AzureHDInsightJobOutput -Cluster $clusterName -JobId $TallyPropertiesJob.JobId -StandardError
        Write-Host "Start: " $startTime ", End: " $endTime -ForegroundColor Green 

5. **実行** 作成できたスクリプトです。 **クリックして** 緑色の実行] ボタンをクリックします。

6. 結果を確認します。 サインイン、 [Azure ポータル][azure-portal]します。 
    1. 左側ウィンドウの <strong>[参照]</strong> をクリックします。
    2. 参照ウィンドウの右上にある <strong>[すべて]</strong> をクリックします。
    3. <strong>[DocumentDB アカウント]</strong> を見つけてクリックします。
    4. 次に、自分の <strong>DocumentDB アカウント</strong>、<strong>DocumentDB データベース</strong>、MapReduce ジョブで指定した出力コレクションに関連付けられた <strong>DocumentDB コレクション</strong>を見つけます。
    5. 最後に、<strong>[開発者ツール]</strong> の下にある <strong>[ドキュメント エクスプローラー]</strong> をクリックします。

    MapReduce ジョブの結果が表示されます。

    ![MapReduce クエリの結果][image-mapreduce-query-results]

## <a name="NextSteps"></a>次のステップ

ご利用ありがとうございます。 Azure DocumentDB および HDInsight を使用して最初の Hive、Pig、および MapReduce ジョブを実行しました。 

Microsoft では Hadoop コネクタをオープン ソース化しています。 投稿できます興味があるなら、 [GitHub][documentdb-github]します。

詳細については、次の記事を参照してください。

- [DocumentDB を使用した Java アプリケーションの開発][documentdb-java-application]
- [HDInsight での Hadoop 用 Java MapReduce プログラムの開発][hdinsight-develop-deploy-java-mapreduce]
- [HDInsight で Hive と Hadoop を使用し、モバイル ハンドセットの使用状況を分析する][hdinsight-get-started]
- [HDInsight での MapReduce の使用][hdinsight-use-mapreduce]
- [HDInsight での Hive の使用][hdinsight-use-hive]
- [HDInsight での Pig の使用][hdinsight-use-pig]
- [Script Action を使って HDInsight クラスターをカスタマイズする][hdinsight-hadoop-customize-cluster]

[apache-hadoop]: http://hadoop.apache.org/
[apache-hadoop-doc]: http://hadoop.apache.org/docs/current/
[apache-hive]: http://hive.apache.org/
[apache-pig]: http://pig.apache.org/
[getting-started]: documentdb-get-started.md

[azure-classic-portal]: https://manage.windowsazure.com/
[azure-powershell-diagram]: ./media/documentdb-run-hadoop-with-hdinsight/azurepowershell-diagram-med.png
[azure-portal]: https://portal.azure.com/

[documentdb-hdinsight-samples]: http://portalcontent.blob.core.windows.net/samples/documentdb-hdinsight-samples.zip
[documentdb-github]: https://github.com/Azure/azure-documentdb-hadoop
[documentdb-java-application]: documentdb-java-application.md
[documentdb-manage-collections]: documentdb-manage.md#Collections
[documentdb-manage-document-storage]: documentdb-manage.md#IndexOverhead
[documentdb-manage-throughput]: documentdb-manage.md#ProvThroughput
[documentdb-import-data]: documentdb-import-data.md

[hdinsight-custom-provision]: ../hdinsight/hdinsight-provision-clusters.md#powershell
[hdinsight-develop-deploy-java-mapreduce]: ../hdinsight/hdinsight-develop-deploy-java-mapreduce.md
[hdinsight-hadoop-customize-cluster]: ../hdinsight/hdinsight-hadoop-customize-cluster.md
[hdinsight-get-started]: ../hdinsight-get-started.md 
[hdinsight-storage]: ../hdinsight-use-blob-storage.md
[hdinsight-use-hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight-use-mapreduce]: ../hdinsight/hdinsight-use-mapreduce.md
[hdinsight-use-pig]: ../hdinsight/hdinsight-use-pig.md

[image-customprovision-page1]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page1.png
[image-customprovision-page4]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page4.png
[image-customprovision-page5]: ./media/documentdb-run-hadoop-with-hdinsight/customprovision-page5.png
[image-storageaccount-quickcreate]: ./media/documentdb-run-hadoop-with-hdinsight/storagequickcreate.png
[image-hive-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/hivequeryresults.PNG
[image-mapreduce-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/mapreducequeryresults.PNG
[image-pig-query-results]: ./media/documentdb-run-hadoop-with-hdinsight/pigqueryresults.PNG

[powershell-install-configure]: ../install-configure-powershell.md
 


