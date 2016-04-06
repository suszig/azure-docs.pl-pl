<properties
    pageTitle="Linux のチュートリアル: Hadoop と Hive の使用 | Microsoft Azure"
    description="HDInsight で Hadoop を使用するには、この Linux チュートリアルに従います。 Linux のクラスターをプロビジョニングする方法、および Hive でデータを照会する方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    authors="nitinme"
    manager="paulettm"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="12/03/2015"
    ms.author="nitinme"/>

# Hadoop チュートリアル: Linux 上の HDInsight で Hive と Hadoop を使用する

> [AZURE.SELECTOR]
- [Windows](hdinsight-hadoop-tutorial-get-started-windows.md)
- [Linux](hdinsight-hadoop-linux-tutorial-get-started.md)

このドキュメントでは、Linux ベースの Hadoop クラスターを作成する方法、Ambari Web UI を開く方法、Ambari Hive ビューを使用して Hive クエリを実行する方法を紹介することで、Linux 上の Azure HDInsight の基本な使用方法について説明します。

> [AZURE.NOTE] Hadoop とビッグ データに慣れていない場合は、詳細を読み取る条件に関する [Apache Hadoop](http://go.microsoft.com/fwlink/?LinkId=510084), 、[MapReduce](http://go.microsoft.com/fwlink/?LinkId=510086), 、[Hadoop 分散ファイル システム (HDFS)](http://go.microsoft.com/fwlink/?LinkId=510087), 、および [Hive](http://go.microsoft.com/fwlink/?LinkId=510085)します。 HDInsight での Azure での Hadoop の使用方法については、次を参照してください。 [HDInsight での Hadoop 入門](hdinsight-hadoop-introduction.md)します。

## 前提条件

Linux でこの Hadoop チュートリアルを開始する前に、以下の条件を満たしている必要があります。

- **Azure サブスクリプション**: を参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。

## <a name="provision"></a>Linux での HDInsight クラスターのプロビジョニング

クラスターをプロビジョニングするときに、Hadoop サービスとリソースを含む Azure コンピューティング リソースを作成します。 このセクションでは、Hadoop バージョン 2.2 を含む HDInsight バージョン 3.2 クラスターをプロビジョニングします。 HDInsight バージョンとその Sla については、次を参照してください。 [HDInsight コンポーネントのバージョン](hdinsight-component-versioning.md)です。 HDInsight クラスターを作成する方法について詳細を参照してください。 [HDInsight クラスターのプロビジョニングのカスタム オプションを使用して][hdinsight-provision]します。

>[AZURE.NOTE]  Windows Server オペレーティング システムを実行する Hadoop クラスターを作成することもできます。 手順については、次を参照してください。 [Windows 上の HDInsight を使ってみる](hdinsight-hadoop-tutorial-get-started-windows.md)します。

新しいクラスターを作成するには、次の手順に従います。

1. サインイン、 [Azure ポータル](https://ms.portal.azure.com/)します。
2. クリックして **新規**, 、] をクリックして **データ分析**, 、] をクリックし、 **HDInsight**します。

    ![Azure ポータルでの新しいクラスターの作成](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.1.png "Creating a new cluster in the Azure Portal")

3. 入力、 **クラスター名**, [ **Hadoop** の **クラスターの種類**, との間、 **クラスターのオペレーティング システム** ドロップダウン リストで、[ **Ubuntu**します。 クラスターを使用できる場合は、クラスター名の横に緑色のチェック マークが表示されます。

    ![クラスターの名前と種類の入力](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.2.png "Enter cluster name and type")

4. 複数のサブスクリプションの場合はクリックして、 **サブスクリプション** エントリをクラスターに使用される Azure サブスクリプションを選択します。

5. クリックして **リソース グループ** を既存のリソース グループの一覧を表示しでクラスターを作成する 1 つを選択します。 またはをクリックして **新規作成** し、新しいリソース グループの名前を入力します。 新しいグループ名を使用できる場合は、緑のチェック マークが表示されます。

    > [AZURE.NOTE] このエントリは、いずれかが使用可能な場合、既存のリソース グループのいずれかに設定されます。

6. クリックして **資格情報** 管理者ユーザーのパスワードを入力するとします。 入力する必要がありますも、 **SSH ユーザー名**します。  **SSH 認証の種類**, 、クリックして **パスワード** し、SSH ユーザーのパスワードを指定します。 クリックして **選択** 下部の資格情報の構成を保存します。

    ![クラスターの資格情報の指定](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.3.png "Provide cluster credentials")

    > [AZURE.NOTE] SSH を使用して、コマンドラインを使用して HDInsight クラスターをリモートでアクセスします。 ここで使用するユーザー名とパスワードは、SSH でクラスターに接続するときに使用されます。

    HDInsight での SSH の使用方法の詳細については、次のいずれかのドキュメントをご覧ください。

    * [Linux、Unix、または OS X から HDInsight 上の Linux ベースの Hadoop で SSH キーを使用する](hdinsight-hadoop-linux-use-ssh-unix.md)
    * [HDInsight の Linux ベースの Hadoop で Windows から SSH を使用する](hdinsight-hadoop-linux-use-ssh-windows.md)


7. クリックして **データソース** クラスターの場合、既存のデータ ソースを選択するか、新しいものを作成します。 HDInsight で Hadoop クラスターをプロビジョニングするときに、Azure ストレージ アカウントを指定します。 Hadoop 分散ファイルシステム (HDFS) と同様、このアカウントの特定の Blob Storage コンテナーが、既定のファイル システムとして設定されます。 既定では、HDInsight クラスターは、指定されたストレージ アカウントと同じデータ センターにプロビジョニングされます。 詳細については、次を参照してください [HDInsight で使用する Azure Blob ストレージ。](hdinsight-use-blob-storage.md)

    ![[データ ソース] ブレード](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.4.png "Provide data source configuration")

    現在、HDInsight クラスターのデータ ソースとして Azure Storage アカウントを選択できます。 エントリを理解するために、次を使用して、 **データソース** ブレードです。

    - **選択方法**: これを設定 **すべてのサブスクリプションから** すべてのサブスクリプションからストレージ アカウントの参照を有効にします。 これを設定 **アクセス キー** を入力する場合、 **ストレージ名** と **アクセス キー** の既存のストレージ アカウント。

    - **ストレージ アカウントを選択]、[新規作成**: クリックして **ストレージ アカウントを選択** を参照して、クラスターに関連付ける既存のストレージ アカウントを選択します。 または、[ **新規作成** 新しいストレージ アカウントを作成します。 表示されたフィールドに、ストレージ アカウントの名前を入力します。 名前を使用できる場合は、緑色のチェック マークが表示されます。

    - **既定のコンテナーを選択して**: これを使用して、クラスターで使用する既定のコンテナーの名前を入力します。 任意の名前を入力できますが、特定のクラスターで使用されていることを簡単に認識できるように、クラスターと同じ名前を使用することをお勧めします。

    - **場所**: リージョン、ストレージ アカウントには、やで作成されます。

        > [AZURE.IMPORTANT] 既定のデータ ソースの場所を選択すると、HDInsight クラスターの場所も設定されます。 クラスターと既定のデータ ソースは、同じリージョンに存在する必要があります。

    クリックして **選択** をデータ ソースの構成を保存します。

8. クリックして **ノード料金レベル** このクラスター用に作成するノードに関する情報を表示します。 クラスターで必要な worker ノードの数を設定します。 クラスターの推定コストがブレード内に表示されます。

    ![[ノード価格レベル] ブレード](./media/hdinsight-hadoop-linux-tutorial-get-started/HDI.CreateCluster.5.png "Specify number of cluster nodes")
    
    > [AZURE.IMPORTANT] クラスターの作成で、または作成後、クラスターを拡大/縮小して、32 を超えるワーカー ノードに計画する場合は、少なくとも 8 つのコアと 14 GB の ram のヘッド ノード サイズを選択する必要があります。
    >
    > ノードのサイズとそれに伴うコストに関する詳細については、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。

    をクリックして **選択** 価格構成ノードを保存します。

9.  **新しい HDInsight クラスター** ブレードで、いることを確認 **スタート画面にピン** クリックして選択すると、 **作成**します。 これでクラスターが作成され、Azure ポータルのスタート画面にクラスター用のタイルが追加されます。 アイコンはクラスターがプロビジョニング中であることを示し、プロビジョニングが完了すると、[HDInsight] アイコンを表示するように変化します。

プロビジョニング中|プロビジョニング完了
------------------|---------------------
    ![スタート画面のプロビジョニング中インジケーター](./media/hdinsight-hadoop-linux-tutorial-get-started/provisioning.png)|![プロビジョニングされたクラスターのタイル](./media/hdinsight-hadoop-linux-tutorial-get-started/provisioned.png)

> [AZURE.NOTE] クラスターを作成、通常は約 15 分間での時間がかかります。 タイルを使用して、スタート画面で、または **通知** プロビジョニング プロセスをチェックするページの左にあるエントリです。

プロビジョニングが完了したら、スタート画面でクラスター用のタイルをクリックして、クラスター ブレードを起動します。

##Hive ビューへの接続

Ambari のビューでは、1 つの Web ページを介して複数のユーティリティを提供しています。 以降のセクションでは、Hive ビューを使用して、HDInsight クラスターに対して Hive クエリを実行します。

> [AZURE.NOTE] Ambari は、管理および監視ユーティリティが Linux ベースの HDInsight クラスターに付属しています。 Ambari には数多くの機能がありますが、このドキュメントではそれらについて説明しません。 詳細については、次を参照してください。 [Ambari Web UI を使用して、クラスターの HDInsight の管理](hdinsight-hadoop-manage-ambari.md)します。

Ambari ビューを作成することができます、Azure ポータルから HDInsight クラスターを選択し、[ __Ambari ビュー__ から、 __クイック リンク__ セクションです。

![quick links section](./media/hdinsight-hadoop-linux-tutorial-get-started/quicklinks.png)

Web ブラウザーで https://CLUSTERNAME.azurehdinsight.net に移動して Ambari に直接移動することもできます (ここで __CLUSTERNAME__ 、HDInsight クラスターの名前を指定) し、ページのメニューから四角形のセットを選択 (] の横に、 __Admin__ リンクと、ページの左側のボタン) をリストの使用可能なビューにします。 選択、 __ビュー [ハイブの__です。

![Selecting ambari views](./media/hdinsight-hadoop-linux-tutorial-get-started/selecthiveview.png).

> [AZURE.NOTE] Ambari にアクセスするときは、サイトに対する認証するように求められます。 クラスターを作成するときに使用した管理者アカウント名 (既定値は `admin`) とパスワードを入力します。

次のようなページが表示されます。

![Image of the hive view page, containing a query editor section](./media/hdinsight-hadoop-linux-tutorial-get-started/hiveview.png)

##<a name="hivequery"></a>Hive クエリを実行する

クラスターに格納されているデータに対して Hive クエリを実行するには、Hive ビューで次の手順を実行します。

1.  __クエリ エディター__ セクション、ページの次の HiveQL ステートメントをワークシートに貼り付けます。

        DROP TABLE log4jLogs;
        CREATE EXTERNAL TABLE log4jLogs(t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY ' '
        STORED AS TEXTFILE LOCATION 'wasb:///example/data/';
        SELECT t4 AS sev, COUNT(*) AS cnt FROM log4jLogs WHERE t4 = '[ERROR]' GROUP BY t4;

    これらのステートメントは次のアクションを実行します。

    - **DROP TABLE** -テーブルが既に存在する場合は、テーブルとデータ ファイルを削除します。
    - **CREATE EXTERNAL TABLE** -Hive に新しく「外部」テーブルを作成します。 外部テーブルは、Hive にテーブル定義のみを格納し、データは、元の場所に残します。
    - **ROW FORMAT** -Hive にデータの形式です。 ここでは、各ログのフィールドは、スペースで区切られています。
    - **STORED AS TEXTFILE LOCATION** - Hive にデータを格納格納先 (example/data ディレクトリ) といるテキストとして格納されています。
    - **選択** の各行の t4 列に [ERROR] の値が含まれているすべての行の数を選択します。

    >[AZURE.NOTE] 、または別の MapReduce 操作によってデータの自動アップロード処理などの外部ソースによって更新する基になるデータが予想される場合、外部テーブルを使用する必要がありますが、常に最新のデータを使用するための Hive クエリをします。 外部テーブルを削除しても *いない* データ、テーブル定義のみを削除します。

2. 使用して、 __Execute__ クエリを開始するクエリ エディターの下部にあるボタンをクリックします。 オレンジ色を有効にする必要があり、テキストに変更 __の実行を停止__します。 A __クエリ プロセスの結果__ セクションのクエリ エディターの下に表示し、ジョブに関する情報を表示する必要があります。

    > [AZURE.IMPORTANT] ブラウザーによっては、ログまたは結果の情報を正しく更新しない場合があります。 ジョブを実行したときに、ジョブの実行が続いているにもかかわらずログが更新されたり結果が返されたりしない場合は、Mozilla FireFox または Google Chrome を代わりに使用してください。
    
3. クエリが完了した後、 __クエリ プロセスの結果__ セクションは、操作の結果を表示します。  __の実行を停止__ ボタンが緑色にも変更されます __Execute__ ] ボタンをクリックします。  __結果__ ] タブで、次の情報を含める必要があります。

        sev       cnt
        [ERROR]   3

     __ログ__ 、ジョブによって作成されたログ情報を表示] タブを使用できます。 ログ情報は、クエリに問題が発生した場合のトラブルシューティングに使用できます。
    
    > [AZURE.TIP] 注、 __結果を保存__ の上にあるドロップダウン リストの左、 __クエリ プロセスの結果__ セクションです。 これを使用して、結果をダウンロードするか、CSV ファイルとして HDInsight のストレージに保存することができます。

3. このクエリの最初の 4 つの行を選択し、[ __Execute__します。 ジョブが完了したときに結果は生成されません。 これは、使用するため、 __Execute__ ボタン、クエリの一部を選択すると、選択したステートメントのみを実行します。 この場合は、テーブルの行を取得する最後のステートメントが選択範囲に含まれていませんでした。 その行だけを選択して使用する __Execute__, 、期待どおりの結果を表示する必要があります。

3. 使用して、 __新しいワークシート__ の下部にあるボタン、 __クエリ エディター__ 新しいワークシートを作成します。 新しいワークシートに、次の HiveQL ステートメントを入力します。

        CREATE TABLE IF NOT EXISTS errorLogs (t1 string, t2 string, t3 string, t4 string, t5 string, t6 string, t7 string) STORED AS ORC;
        INSERT OVERWRITE TABLE errorLogs SELECT t1, t2, t3, t4, t5, t6, t7 FROM log4jLogs WHERE t4 = '[ERROR]';

    これらのステートメントは次のアクションを実行します。

    - **テーブル IF NOT EXISTS の作成** -が既に存在しない場合は、テーブルを作成します。 以降、 **外部** キーワードが使用されていない、これは内部テーブルであり、Hive データ ウェアハウスに格納され、Hive によって完全に管理されます。 外部テーブルとは異なり、内部テーブルを削除すると、基盤となるデータも削除されます。
    - **STORED AS ORC** -Optimized Row Columnar (ORC) 形式でデータを格納します。 この形式は、Hive にデータを格納するための、非常に効率的で適切な形式です。
    - **INSERT OVERWRITE ...選択** の行を選択、 **log4jLogs** [ERROR] を含むとにデータを挿入するテーブル、 **errorLogs** テーブルです。
    
    使用して、 __Execute__ このクエリを実行するボタンをクリックします。  __結果__ このクエリによって行が返されませんが、として状態が [は、タブに情報は含まれない __SUCCEEDED__します。
    
4. クエリ エディターの右側には、一連のアイコンが表示されます。 チェーンのようなアイコンを選択します。

    ![icons](./media/hdinsight-hadoop-linux-tutorial-get-started/icons.png)
    
    これは、 __Visual 説明__ 複雑なクエリのフローを理解するうえで役に立ちますクエリのビューです。 このビューに相当するテキストを表示するにを使用して、 __説明__ クエリ エディターでのボタンをクリックします。
    
    ![visual explain image](./media/hdinsight-hadoop-linux-tutorial-get-started/visualexplain.png)
    
    その他のアイコンは次のとおりです。
    
        * Settings: The gear icon allows you to change Hive settings, such as setting `hive.execution.engine` or Tez parameters.
        * Tez: Displays the Directed Acyclic Graph (DAG) that Tez used to perform the query. If you want to view the DAG for queries you've ran in the past, use the __Tez View__ instead.
        * Notifications: Displays notifications, such as "Query has been submitted" or if an error occurs when running a query.

5. 選択、 __SQL__ クエリ エディターに戻るを切り替え、新しいワークシートを作成し、次のクエリを入力してアイコン。

        SELECT * from errorLogs;
    
    使用して、 __名前を付けて__ エディターの下部にあるボタンをクリックします。 このクエリに名前 __Errorlogs__ 選択 __OK__します。 ワークシートの名前を変更する __Errorlogs__します。
    
    保存されたクエリにも表示、 __保存されているクエリ__ 、ページの上部にあるタブをクリックします。 このオプションを選択して表示されます __Errorlogs__ 一覧表示します。 名前を選択すると、クエリ エディターにクエリが表示されます。

4. 実行、 __Errorlogs__ クエリ。 結果は次のようになります。

        errorlogs.t1    errorlogs.t2    errorlogs.t3    errorlogs.t4    errorlogs.t5    errorlogs.t6    errorlogs.t7
        2012-02-03  18:35:34    SampleClass0    [ERROR]     incorrect   id  
        2012-02-03  18:55:54    SampleClass1    [ERROR]     incorrect   id  
        2012-02-03  19:25:27    SampleClass4    [ERROR]     incorrect   id

## <a name="nextsteps"></a>次のステップ

このドキュメントでは、Azure ポータルを使用して Linux ベースの HDInsight クラスターを作成する方法、SSH を使用してクラスターに接続する方法、基本的な Hive クエリを実行する方法について説明しました。

HDInsight でデータを分析する方法の詳細については、次を参照してください。

- Visual Studio から Hive クエリを実行する方法を含め、HDInsight で Hive を使用する方法の詳細を参照してください。 [HDInsight での Hive の使用][hdinsight-use-hive]します。

- Pig、データを変換するための言語の詳細についてを参照してください。 [HDInsight での Pig の使用][hdinsight-use-pig]します。

- MapReduce、Hadoop 上のデータを処理するプログラムを記述する方法の詳細についてを参照してください。 [HDInsight での MapReduce の使用][hdinsight-use-mapreduce]します。

- HDInsight Tools for Visual Studio を使用して HDInsight でのデータを分析する方法については、次を参照してください。 [HDInsight 用の Visual Studio Hadoop tools の使用開始](hdinsight-hadoop-visual-studio-tools-get-started.md)します。

実際のデータを使用する準備が整っていて、HDInsight のデータの格納方法や HDInsight にデータを取り込む方法を確認する場合は、以下を参照してください。

- HDInsight での Azure blob ストレージの使用方法については、次を参照してください。 [Azure Blob ストレージの使用 HDInsight](hdinsight-use-blob-storage.md)します。

- 詳細データを HDInsight にアップロードする方法については、次を参照してください。 [データを HDInsight にアップロードする][hdinsight-upload-data]です。

HDInsight クラスターの作成または管理の詳細については、以下を参照してください。

- Linux ベースの HDInsight クラスターを管理する詳細については、次を参照してください。 [Ambari を使用した HDInsight の管理のクラスター](hdinsight-hadoop-manage-ambari.md)します。

- HDInsight クラスターを作成するときに選択できるオプションの詳細については、次を参照してください。 [カスタム オプションを使用した Linux での HDInsight のプロビジョニング](hdinsight-hadoop-provision-linux-clusters.md)します。

- Linux、および Hadoop に慣れているが、HDInsight での Hadoop についての詳細を知りたい場合は、次を参照してください。 [Linux で HDInsight の使用](hdinsight-hadoop-linux-information.md)します。 次のような情報が掲載されています。

    * クラスターでホストされる URL (Ambari や WebHCat など)
    * Hadoop ファイルの場所とローカル ファイル システムの例
    * 既定のデータ ストアとして、HDFS ではなく Azure Storage (WASB) を使用する


[1]: ../HDInsight/hdinsight-hadoop-visual-studio-tools-get-started.md

[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-upload-data]: hdinsight-upload-data.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md
[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-pig]: hdinsight-use-pig.md

[powershell-download]: http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409
[powershell-install-configure]: ../install-configure-powershell.md
[powershell-open]: ../install-configure-powershell.md#Install

[img-hdi-dashboard]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.png
[img-hdi-dashboard-query-select]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.png
[img-hdi-dashboard-query-select-result]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.png
[img-hdi-dashboard-query-select-result-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.select.result.output.png
[img-hdi-dashboard-query-browse-output]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.dashboard.query.browse.output.png
[image-hdi-clusterstatus]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.ClusterStatus.png
[image-hdi-gettingstarted-powerquery-importdata]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData.png
[image-hdi-gettingstarted-powerquery-importdata2]: ./media/hdinsight-hadoop-tutorial-get-started-windows/HDI.GettingStarted.PowerQuery.ImportData2.png


