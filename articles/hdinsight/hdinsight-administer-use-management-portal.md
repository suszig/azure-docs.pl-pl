<properties
    pageTitle="Azure ポータルを使用した HDInsight での Hadoop クラスターの管理 | Microsoft Azure"
    description="HDInsight サービスを管理する方法を学習します。 HDInsight クラスターを作成し、対話型 JavaScript コンソールを開いて、Hadoop コマンド コンソールを開きます。"
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
    ms.date="11/24/2015"
    ms.author="jgao"/>

# Azure ポータルを使用した HDInsight での Hadoop クラスターの管理

[AZURE.INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

使用して、 [Azure ポータル][azure-portal], 、Azure HDInsight で Hadoop クラスターを作成、Hadoop ユーザーのパスワードを変更して、クラスターで Hadoop コマンド コンソールにアクセスできるように、リモート デスクトップ プロトコル (RDP) を有効にすることができます。

この記事の情報は、Windows ベースの HDInsight クラスターにのみ適用されます。 Linux ベースのクラスターの管理については、上部のタブ セレクターをクリックしてください。

他のツールを使用して HDInsight で Hadoop クラスターを作成する方法については、タブ セレクターをクリックしてください。 

[AZURE.INCLUDE [hdinsight-azure-preview-portal](../../includes/hdinsight-azure-preview-portal.md)]

* [Azure ポータルを使用した HDInsight での Hadoop クラスターの管理](hdinsight-administer-use-management-portal-v1.md)

**前提条件**

この記事を読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**します。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **Azure ストレージ アカウント** -HDInsight クラスターは既定のファイル システムとして Azure Blob ストレージ コンテナーを使用します。 Azure Blob ストレージが HDInsight クラスターにシームレスなエクスペリエンスを提供する方法の詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージの使用](../hdinsight-use-blob-storage.md)します。 Azure ストレージ アカウントの作成の詳細については、「 [をストレージ アカウントを作成する方法](../storage-create-storage-account.md)します。

##ポータルを開く

1. サインイン [https://portal.azure.com](https://portal.azure.com)します。
2. ポータルを開くと、次の操作を行うことができます。

    - をクリックして **新規** から左のメニューで新しいクラスターを作成します。
    
        ![新しい [HDInsight クラスター] ボタン](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
    - クリックして **HDInsight クラスター** 左側のメニューからです。
    
        ![Azure ポータル [HDInsight クラスター] ボタン](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

    場合 **HDInsight** 左側のメニューに表示しない] をクリックして **参照**します。 

    ![Azure ポータル [参照] ボタン](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

##クラスターの作成

作成手順については、ポータルを使用して、次を参照してください。 [作成 HDInsight クラスターの](hdinsight-provision-clusters.md#create-using-the-preview-portal)です。

HDInsight は、広範囲の Hadoop コンポーネントで動作します。 検証され、サポートされているコンポーネントの一覧については 
参照してください [Azure HDInsight でサポートされている Hadoop のバージョン](hdinsight-component-versioning.md)です。 次のいずれかのオプションを使用して、HDInsight をカスタマイズできます。

- Script Action を使用してカスタム スクリプトを実行します。これによってクラスターをカスタマイズして、クラスター構成を変更することや、Giraph や Solr などのカスタム コンポーネントをインストールすることができます。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ Script Action を使って](hdinsight-hadoop-customize-cluster.md)します。
- クラスターの作成中に、HDInsight .NET SDK または Azure PowerShell でクラスター カスタマイズ パラメーターを使用します。 そうすることで、クラスターの有効期間中はこれらの構成変更が保持され、Azure プラットフォームが保守のために定期的に実行するクラスター ノードの再イメージ化の影響も受けません。 クラスター カスタマイズ パラメーターの使用に関する詳細については、次を参照してください。 [作成 HDInsight クラスターの](hdinsight-provision-clusters.md)です。
- Mahout や Cascading などの一部のネイティブ Java コンポーネントは、JAR ファイルとしてクラスター上で実行できます。 これらの JAR ファイルは、Azure BLOB ストレージに分配し、Hadoop ジョブ送信メカニズムによって HDInsight クラスターに送信できます。 詳細については、次を参照してください。 [プログラムによる Hadoop の送信ジョブ](hdinsight-submit-hadoop-jobs-programmatically.md)します。

    >[AZURE.NOTE] HDInsight クラスターへの JAR ファイルの配置に関する問題がある場合は、HDInsight クラスターでの JAR ファイルを呼び出すことにお問い合わせください [Microsoft サポート](http://azure.microsoft.com/support/options/)します。

    > Cascading は HDInsight ではサポートされておらず、Microsoft サポートの対象でもありません。 サポートされているコンポーネントの一覧は、次を参照してください。 [HDInsight で提供されるクラスター バージョンの新機能ですか?](hdinsight-component-versioning.md)します。

リモート デスクトップ接続を使用したクラスターへのカスタム ソフトウェアのインストールはサポートされていません。 クラスターの再作成が必要な場合にファイルが失われるため、ヘッド ノードのドライブにはファイルを保存しないでください。 Azure BLOB ストレージにファイルを保存することをお勧めします。 BLOB ストレージは永続的です。

##クラスターの一覧と表示

1. サインイン [https://portal.azure.com](https://portal.azure.com)します。
2. クリックして **HDInsight クラスター** 左側のメニューから。
    
    ![Azure ポータル [HDInsight クラスター] ボタン](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)
        
    場合 **HDInsight** 左側のメニューに表示しない] をクリックして **参照**: 

    ![Azure ポータル [参照] ボタン](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

    クラスターが存在する場合はクラスターの一覧が表示されます。
    
    ![Azure ポータル　クラスター一覧](./media/hdinsight-administer-use-management-portal/hdinsight-list-clusters.png)

3. 使用 **項目をフィルター処理** とリストを絞り込むには、「サブスクリプション」です。
4. 一覧にあるクラスターをダブルクリックして詳細を表示します。

    **メニューと essentials**:

    ![Azure ポータル HDInsight クラスター 基本情報](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)
    
    - Memu をカスタマイズするには、任意の場所を右クリックしてメニューで、クリックして **カスタマイズ**します。
    - **設定** と **すべての設定**: 表示、 **設定** ブレードで、クラスターのクラスターの詳細な構成情報にアクセスすることができます。
    - **ダッシュ ボード**, 、**クラスターのダッシュ ボード** と * * URL: これらは、Linux ベースのクラスターの Ambari Web は、クラスターのダッシュ ボードにアクセスするすべての方法です。
    - **リモート デスクトップ**: RDP を使用してクラスターに接続します。
    - **クラスターのスケール**: このクラスター workder ノードの数を変更することができます。
    - **削除**: クラスターを削除します。
    - **クイック スタート (![クラウドとサンダー ボルトのアイコンのクイック スタートを =](./media/hdinsight-administer-use-portal-linux/quickstart.png))**: のに役立つ情報を表示 HDInsight の概要です。
    - **ユーザー (![ユーザー アイコン](./media/hdinsight-administer-use-portal-linux/users.png))**: アクセス許可を設定することができます _ポータル管理_ 、Azure サブスクリプションに他のユーザーは、このクラスターのです。
    
        > [AZURE.IMPORTANT] これは、 _のみ_ アクセスと Azure プレビュー ポータルでは、このクラスターへのアクセス許可に影響しへの接続または HDInsight クラスターにジョブを送信できるユーザーに影響を与えません。
    - **タグ (![タグ アイコン](./media/hdinsight-administer-use-portal-linux/tags.png))**: タグでは、クラウド サービスのカスタム分類を定義するキー/値ペアを設定することができます。 たとえば、という名前のキーを作成することがあります __プロジェクト__, 、し、特定のプロジェクトに関連付けられているすべてのサービスの一般的な値を使用します。
    - **ドキュメント**: Azure HDInsight のドキュメントにリンクします。
    
    > [AZURE.IMPORTANT] HDInsight クラスターで提供されるサービスを管理するには、Ambari Web または Ambari REST API を使用する必要があります。 Ambari の使用に関する詳細については、次を参照してください。 [Ambari を使用した HDInsight の管理のクラスター](hdinsight-hadoop-manage-ambari.md)します。

    **使用状況**:
    
    ![Azure ポータル HDInsight クラスター 使用状況](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)
    
5. クリックして **設定**します。

    ![Azure ポータル HDInsight クラスター 使用状況](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

    - **プロパティ**: クラスターのプロパティを表示します。
    - **クラスターの AAD Id**: 
    - **Azure のストレージ キー**: 既定のストレージ アカウントとそのキーを表示します。 ストレージ アカウントは、クラスターの作成処理中に構成します。
    - **クラスターのログイン**: クラスターの HTTP ユーザー名とパスワードを変更します。
    - **外部メタストア**: Hive と Oozie メタストアを表示します。 メタストアを構成できるのは、クラスターの作成処理中のみです。
    - **クラスターのスケール**: 増加し、クラスターのワーカー ノード数が減少します。
    - **リモート デスクトップ**: を有効にして、リモート デスクトップ (RDP) アクセスを無効にし、RDP ユーザー名を構成します。  RDP ユーザー名は HTTP ユーザー名とは別にする必要があります。
    - **指名パートナーが**:
    
    > [AZURE.NOTE] これは、使用可能な設定のジェネリック リストすべてのクラスターの種類のすべてのなります。

6. クリックして **プロパティ**:

    次のプロパティが表示されます。
    
    - **ホスト名**: クラスター名です。
    - **クラスターの URL**します。
    - **ステータス**: 含める中止されたため、受け入れられると、ClusterStorageProvisioned、AzureVMConfiguration、HDInsightConfiguration、運用、実行して、エラー、削除、削除すると、タイムアウト、DeleteQueued、DeleteTimedout、DeleteError、PatchQueued、CertRolloverQueued、ResizeQueued、ClusterCustomization
    - **地域**: Azure の場所。 サポートされている Azure の場所の一覧は、次を参照してください。、 **地域** ドロップダウン リスト ボックスで [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。
    - **作成されたデータ**します。
    - **オペレーティング システム**: か **Windows** または **Linux**です。
    - **型**: Hadoop、HBase、Storm、Spark です。 
    - **バージョン**です。 参照してください [HDInsight のバージョン](hdinsight-component-versioning.md)
    - **Subscriotion**: サブスクリプションの名前。
    - **します ID**します。
    - **プライマリ データ ソース**します。 既定の Hadoop ファイル システムとして使用される Azure BLOB ストレージ アカウント。
    - **ワーカー ノード料金レベル**します。
    - **ヘッド ノードの価格レベル**します。

##クラスターの削除

クラスターを削除しても、既定のストレージ アカウントまたはリンクされたストレージ アカウントは削除されません。 同じストレージ アカウントと同じメタストアを使用してクラスターを再作成することができます。

1. サインイン、 [ポータル][azure-portal]します。
2. をクリックして **すべてを参照** 左側のメニューからクリックして **HDInsight クラスター**, 、クラスター名をクリックします。
3. クリックして **削除** 上部のメニューからし、指示に従います。

関連項目 [クラスターをシャット ダウンの一時停止](#pauseshut-down-clusters)します。

##クラスターのスケール
クラスターのスケール設定機能を使用すると、Azure HDInsight で実行しているクラスターによって使用される worker ノードの数を、クラスターを再作成することなく、変更できます。

>[AZURE.NOTE] HDInsight バージョン 3.1.3 でのみクラスターまたは以降がサポートされています。 クラスターのバージョンがわからない場合、[プロパティ] ページを確認できます。  参照してください [] ボックスの一覧と表示クラスター](hdinsight-adminster-use-management-portal/#list-and-show-clusters)します。

HDInsight でサポートされているクラスターの種類ごとに、データ ノード数を変更した場合の影響:

- Hadoop は、

    保留中または実行中のジョブに影響を与えることなく、実行中の Hadoop クラスター内の worker ノードの数をシームレスに増加できます。 処理の進行中に新しいジョブを送信することもできます。 スケール設定処理の失敗は正常に処理され、クラスターは常に機能状態になります。

    データ ノードの数を減らして Hadoop クラスターのスケールを小さくした場合、クラスター内の一部のサービスが再起動されます。 これにより、スケール設定処理の完了時に、実行中および保留中のすべてのジョブが失敗します。 ただし、処理が完了した後にジョブを再送信できます。

- HBase

    実行中の HBase クラスターに対して、ノードの追加または削除をシームレスに実行できます。 地域サーバーは、スケール設定処理の完了の数分以内に自動的に分散されます。 ただし、クラスターのヘッドノードにログインし、コマンド プロンプト ウィンドウから次のコマンドを実行して、地域サーバーを手動で分散することもできます。

        >pushd %HBASE_HOME%\bin
        >hbase shell
        >balancer

    HBase シェルの使用に関する詳細については、を参照してください。
- Storm

    実行中の Storm クラスターに対して、データ ノードの追加または削除をシームレスに実行できます。 ただし、スケール設定処理が正常に完了した後、トポロジのバランス再調整が必要になります。

    バランス再調整は、次の 2 つの方法で実行できます。

    * Storm Web UI
    * コマンド ライン インターフェイス (CLI) ツール

    参照してください、 [Apache Storm に関するドキュメント](http://storm.apache.org/documentation/Understanding-the-parallelism-of-a-Storm-topology.html) 詳細です。

    Storm Web UI は、HDInsight クラスターで使用できます。

    ![HDInsight Storm の規模のバランス調整](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.storm.rebalance.png)

    CLI コマンドを使用して Storm トポロジのバランスを再調整する方法を次の例で示します。

        ## Reconfigure the topology "mytopology" to use 5 worker processes,
        ## the spout "blue-spout" to use 3 executors, and
        ## the bolt "yellow-bolt" to use 10 executors

        $ storm rebalance mytopology -n 5 -e blue-spout=3 -e yellow-bolt=10


**クラスターの規模を設定するには**

1. サインイン、 [ポータル][azure-portal]します。
2. をクリックして **すべてを参照** 左側のメニューからクリックして **HDInsight クラスター**, 、クラスター名をクリックします。
3. クリックして **設定** をクリックし、上部のメニューから **スケール クラスター**します。
4. 入力 **数のワーカー ノード**します。 クラスター ノードの数に制限は Azure サブスクリプションによって異なります。 制限値を上げるには、課金サポートにお問い合わせください。  コスト情報にはノード数の変更が反映されます。

    ![HDInsight Hadoop HBase Storm および Spark の規模](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

##クラスターの一時停止またはシャットダウン

Hadoop ジョブの大半は、たまにしか実行されないバッチ ジョブです。 ほとんどの Hadoop クラスターでは、クラスターが処理に使用されていない期間がかなりあります。 HDInsight を使用すると、データは Azure Storage に格納されるため、クラスターは、使用されていない場合に安全に削除できます。
また、HDInsight クラスターは、使用していない場合でも課金されます。 クラスターの料金は Storage の料金の何倍にもなるため、クラスターを使用しない場合は削除するのが経済的にも合理的です。

このプロセスをプログラムで実行する方法は数多くあります。

- Azure Data Factory を使用する。 参照してください [Azure HDInsight のリンクされたサービス](../data-factory-compute-linked-services.md/#azure-hdinsight-linked-service) と [変換し、Azure Data Factory を使用して分析](../data-factory-data-transformation-activities.md) 、オンデマンドと独自の HDInsight 用のリンクされたサービスです。
- Azure PowerShell を使用する。  参照してください [フライト遅延データの分析](hdinsight-analyze-flight-delay-data.md)します。
- Azure CLI を使用する。 参照してください [管理 HDInsight クラスターの Azure CLI を使用して](hdinsight-administer-use-command-line.md)します。
- HDInsight .NET SDK を使用する。 参照してください [Hadoop ジョブの送信](hdinsight-submit-hadoop-jobs-programmatically.md)します。

料金情報については、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。 ポータルからクラスターを削除するのを参照してください [クラスターを削除。](#delete-clusters)

##クラスターのユーザー名の変更

HDInsight クラスターは、2 つのユーザー アカウントを持つことができます。 HDInsight クラスターのユーザー アカウントは、作成プロセスで作成されます。 RDP を使用してクラスターにアクセスするために、RDP ユーザー アカウントを作成することもできます。 参照してください [リモート デスクトップを有効にする](#connect-to-hdinsight-clusters-by-using-rdp)です。

**HDInsight クラスターのユーザー名とパスワードを変更するには**

1. サインイン、 [ポータル][azure-portal]します。
2. をクリックして **すべてを参照** 左側のメニューからクリックして **HDInsight クラスター**, 、クラスター名をクリックします。
3. クリックして **設定** をクリックし、上部のメニューから **クラスター ログイン**します。
4. 場合 **クラスター ログイン** が有効にする必要があります] をクリックして **を無効にする**, 、順にクリック **を有効にする** ユーザー名とパスワードを変更する前に.
4. 変更、 **クラスター ログイン名** と **クラスター ログインのパスワード**, 、] をクリックし、 **保存**します。

    ![HDInsight クラスターの HTTP ユーザーのユーザー名とパスワードの変更](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

##アクセス権の付与/取り消し

HDInsight クラスターには、以下の HTTP Web サービスがあります (これらすべてのサービスには、REST ベースのエンドポイントがあります)。

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton

既定では、これらのサービスへのアクセス許可が付与されます。 Azure ポータルからアクセス許可を取り消す/付与することができます。

>[AZURE.NOTE] アクセス権を付与/取り消しでは、クラスターのユーザー名とパスワードがリセットされます。

**HTTP Web サービスのアクセス許可を付与する/取り消すには**

1. サインイン、 [ポータル][azure-portal]します。
2. をクリックして **すべてを参照** 左側のメニューからクリックして **HDInsight クラスター**, 、クラスター名をクリックします。
3. クリックして **設定** をクリックし、上部のメニューから **クラスター ログイン**します。
4. 場合 **クラスター ログイン** が有効にする必要があります] をクリックして **を無効にする**, 、順にクリック **を有効にする** ユーザー名とパスワードを変更する前に.
6.  **クラスター ログイン ユーザー名** と **クラスター ログインのパスワード**, 、クラスターの新しいユーザー名とパスワードを (それぞれ) を入力します。
7. クリックして **保存**します。

    ![HDInsight による HTTP Web サービス アクセスの付与と削除](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)




##既定のストレージ アカウントの検索

各 HDInsight クラスターには、既定のストレージ アカウントが設定されています。 既定のストレージ アカウントとそのキーの下にあるクラスターと **設定**/**プロパティ**/**Azure ストレージ キー**します。 参照してください [] ボックスの一覧と表示クラスター](#list-and-show-clusters)します。

    
##リソース グループの検索 

ARM モードでは、各 HDInsight クラスターは Azure リソース グループと共に作成されます。 クラスターが属している Azure リソース グループは、次の場所に表示されます。

- クラスターの一覧には、 **リソース グループ** 列です。
- クラスター **不可欠** を並べて表示します。  

参照してください [] ボックスの一覧と表示クラスター](#list-and-show-clusters)します。
   
##HDInsight クエリ コンソールを開く

HDInsight クエリ コンソールには、次の機能が用意されています。

- **概要ギャラリー**: ギャラリーを使用するには、次を参照してください。 [Azure HDInsight の Getting Started ギャラリーを使用して学習 Hadoop](hdinsight-learn-hadoop-use-sample-gallery.md)します。
- **Hive エディター**: Hive ジョブを送信するための GUI web インターフェイスです。  参照してください [Hive クエリの実行、クエリ コンソールを使用して](hdinsight-hadoop-use-hive-query-console.md)します。

    ![HDInsight ポータル Hive エディター](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)

- **ジョブの履歴を**: モニター Hadoop ジョブです。  

    ![HDInsight ポータル ジョブ履歴](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    をクリックして **クエリ名** ジョブのプロパティを含む詳細を表示する **ジョブ クエリ**, 、および * * ジョブの出力です。 また、クエリと出力の両方をワークステーションにダウンロードすることもできます。

- **ファイル ブラウザー**: 既定のストレージ アカウントとリンクされたストレージ アカウントを参照します。

    ![HDInsight ポータル ファイル ブラウザー](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    スクリーン ショットで、 **<Account>** アイテムが、Azure ストレージ アカウントを示します。  アカウント名をクリックすると、ファイルを参照できます。
    
- **Hadoop UI**します。

    ![HDInsight ポータル Hadoop UI](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)
    
    **Hadoop UI*, 、ファイルを参照し、ログを確認できます。 

- **Yarn UI**します。

    ![HDInsight ポータル Yarn UI](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

##Hive クエリの実行

ポータルから Hive ジョブを実行するにはクリックして **Hive エディター** HDInsight クエリ コンソールでします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

##ジョブの監視

ポータルからジョブを監視する] をクリックして **ジョブ履歴** HDInsight クエリ コンソールでします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

##Browse files

既定のストレージ アカウントとリンクされたストレージ アカウントに格納されたファイルを参照するにはクリックして **ファイル ブラウザー** HDInsight クエリ コンソールでします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

使用することも、 **ファイル システムから参照** からユーティリティ、 **Hadoop UI** HDInsight コンソールでします。  参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。



##クラスターの使用状況の監視

 __使用__ HDInsight クラスターのブレードのセクションでは、このクラスター内のノードに割り当てられているどのようにこのクラスターに割り当てられているコアの数だけでなく、サブスクリプションで HDInsight を使用するために使用できるコアの数に関する情報を表示します。 参照してください [] ボックスの一覧と表示クラスター](#list-and-show-clusters)します。

> [AZURE.IMPORTANT] HDInsight クラスターで提供されるサービスを監視するには、Ambari Web または Ambari REST API を使用する必要があります。 Ambari の使用に関する詳細については、次を参照してください [Ambari を使用したクラスターの HDInsight の管理。](hdinsight-hadoop-manage-ambari.md)


##Hadoop UI を開く

クラスタを監視するには、そのファイル システムを参照し、ログを確認し] をクリックして **Hadoop UI** HDInsight クエリ コンソールでします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

##Yarn UI を開く

Yarn ユーザー インターフェイスを使用する] をクリックして **Yarn UI** HDInsight クエリ コンソールでします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

##RDP を使用したクラスターへの接続

クラスターの作成時に指定したクラスターの資格情報を使用するとクラスター上のサービスにアクセスできますが、リモート デスクトップを介してクラスター自体にアクセスすることはできません。 クラスターにプロビジョニングするとき、あるいはクラスターにプロビジョニングした後、リモート デスクトップ アクセスをオンにできます。 作成時にリモート デスクトップを有効にする方法については、「 [作成 HDInsight クラスター](hdinsight-provision-clusters.md)します。

**リモート デスクトップを有効にするには**

1. サインイン、 [ポータル][azure-portal]します。
2. をクリックして **すべてを参照** 左側のメニューからクリックして **HDInsight クラスター**, 、クラスター名をクリックします。
3. クリックして **設定** をクリックし、上部のメニューから **リモート デスクトップ**します。
4. 入力 **の有効期限が切れる**, 、**リモート デスクトップ ユーザー名** と **リモート デスクトップ パスワード**, 、] をクリックし、 **を有効にする**です。

    ![HDInsight によるリモート デスクトップ構成の有効化と無効化](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    [有効期限] の既定値は 1 週間です。
> [AZURE.NOTE] HDInsight .NET SDK を使用して、クラスターにリモート デスクトップを有効にすることができますもします。 使用して、 **EnableRdp** 次のように HDInsight クライアント オブジェクトでメソッド: **クライアントです。EnableRdp (clustername、location,"rdpuser","rdppassword", DateTime.Now.AddDays(6))**します。 同様に、クラスターにリモート デスクトップを無効にして **クライアントです。DisableRdp, location)**します。 これらの方法の詳細については、次を参照してください。 [HDInsight .NET SDK のリファレンス](http://go.microsoft.com/fwlink/?LinkId=529017)します。 この方法は、Windows で実行されている HDInsight クラスターにのみ適用できます。

**RDP を使用してクラスターに接続するには**

1. サインイン、 [ポータル][azure-portal]します。
2. をクリックして **すべてを参照** 左側のメニューからクリックして **HDInsight クラスター**, 、クラスター名をクリックします。
3. クリックして **設定** をクリックし、上部のメニューから **リモート デスクトップ**します。
4. クリックして **接続** の指示に従います。 接続が無効の場合は、最初に有効する必要があります。 必ずリモート デスクトップ ユーザーのユーザー名とパスワードを使用してください。  クラスターのユーザー資格情報は使用できません。


##Hadoop コマンド ラインを開く

リモート デスクトップを使用してクラスターに接続して Hadoop コマンド ラインを使用するには、まず、先のセクションで説明したように、クラスターに対するリモート デスクトップ アクセスを有効にする必要があります。

**Hadoop コマンド ラインを開くには**

1. リモート デスクトップを使用してクラスターに接続します。
8. ダブルクリックして、デスクトップから **Hadoop コマンド ライン**します。

    ![HDI.HadoopCommandLine][image-hadoopcommandline]

    Hadoop コマンドの詳細については、次を参照してください。 [Hadoop コマンド リファレンス](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html)します。

先のスクリーンショットで、フォルダー名には Hadoop のバージョン番号が埋め込まれています。 バージョン番号は、クラスターにインストールされている Hadoop コンポーネントのバージョンに基づいて変更できます。 Hadoop 環境変数を使用して、これらのフォルダーを参照できます。 次に例を示します。

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%
    
##次のステップ
この記事では、ポータルを使用して HDInsight クラスターを作成する方法、および Hadoop コマンド ライン ツールを開く方法について説明しました。 詳細については、次の記事を参照してください。

* [Azure PowerShell を使用した HDInsight の管理](hdinsight-administer-use-powershell.md)
* [Azure CLI を使用した HDInsight の管理](hdinsight-administer-use-command-line.md)
* [HDInsight クラスターの作成](hdinsight-provision-clusters.md)
* [プログラムによる Hadoop ジョブの送信](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Azure HDInsight の概要](../hdinsight-get-started.md)
* [Azure HDInsight でサポートされている Hadoop のバージョン](hdinsight-component-versioning.md)

[azure-portal]: https://portal.azure.com
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop command line"

