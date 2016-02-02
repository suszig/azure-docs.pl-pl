<properties
    pageTitle="Azure ポータルを使用した HDInsight での Hadoop クラスターの管理 | Microsoft Azure"
    description="HDInsight サービスを管理する方法を学習します。HDInsight クラスターを作成し、対話型 JavaScript コンソールを開いて、Hadoop コマンド コンソールを開きます。"
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

使用して、 [Azure ポータルの ][azure-portal], 、Azure HDInsight で Hadoop クラスターを作成、Hadoop ユーザーのパスワードを変更して、クラスターで Hadoop コマンド コンソールにアクセスできるように、リモート デスクトップ プロトコル (RDP) を有効にすることができます。

この記事の情報は、Windows ベースの HDInsight クラスターにのみ適用されます。 Linux ベースのクラスターの管理については、上部のタブ セレクターをクリックしてください。

他のツールを使用して HDInsight で Hadoop クラスターを作成する方法については、タブ セレクターをクリックしてください。

[AZURE.INCLUDE [hdinsight-azure-preview-portal](../../includes/hdinsight-azure-preview-portal.md)]

* [Azure ポータルを使用して HDInsight で Hadoop クラスターを管理します。](hdinsight-administer-use-management-portal-v1.md)

**前提条件**

この記事を読み始める前に、次の項目を用意する必要があります。

- **Azure サブスクリプション**。 参照してください [取得 Azure 無料試用版](http://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/)します。
- **Azure ストレージ アカウント** - HDInsight クラスターは、既定のファイル システムとして Azure BLOB ストレージ コンテナーを使用します。 Azure Blob ストレージが HDInsight クラスターにシームレスなエクスペリエンスを提供する方法の詳細については、次を参照してください。 [HDInsight で Azure Blob ストレージの使用](../hdinsight-use-blob-storage.md)します。 Azure ストレージ アカウントの作成の詳細については、「 [をストレージ アカウントを作成する方法](../storage-create-storage-account.md)します。

## ポータルを開く

1. サインイン [https://portal.azure.com](https://portal.azure.com)します。
2. ポータルを開くと、次の操作を行うことができます。

    - 左側のメニューの **[新規]** をクリックして新しいクラスターを作成する。

        ![新しい ](./media/hdinsight-administer-use-management-portal/azure-portal-new-button.png)
    - 左側のメニューの **[HDInsight クラスター]** をクリックする。

        ![Azure ポータル ](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

    左側のメニューに **[HDInsight]** が表示されていない場合は、**[参照]** をクリックする。

    ![Azure ポータル ](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

## クラスターの作成

作成手順については、ポータルを使用して、次を参照してください。 [作成 HDInsight クラスターの](hdinsight-provision-clusters.md#create-using-the-preview-portal)します。

HDInsight は、広範囲の Hadoop コンポーネントで動作します。 検証され、サポートされているコンポーネントの一覧については 
参照してください [Azure HDInsight でサポートされている Hadoop のバージョン](hdinsight-component-versioning.md)します。 次のいずれかのオプションを使用して、HDInsight をカスタマイズできます。

- Script Action を使用してカスタム スクリプトを実行します。これによってクラスターをカスタマイズして、クラスター構成を変更することや、Giraph や Solr などのカスタム コンポーネントをインストールすることができます。 詳細については、次を参照してください。 [HDInsight クラスターのカスタマイズ Script Action を使って](hdinsight-hadoop-customize-cluster.md)します。
- クラスターの作成中に、HDInsight .NET SDK または Azure PowerShell でクラスター カスタマイズ パラメーターを使用します。 そうすることで、クラスターの有効期間中はこれらの構成変更が保持され、Azure プラットフォームが保守のために定期的に実行するクラスター ノードの再イメージ化の影響も受けません。 クラスター カスタマイズ パラメーターの使用に関する詳細については、次を参照してください。 [作成 HDInsight クラスターの](hdinsight-provision-clusters.md)します。
- Mahout や Cascading などの一部のネイティブ Java コンポーネントは、JAR ファイルとしてクラスター上で実行できます。 これらの JAR ファイルは、Azure BLOB ストレージに分配し、Hadoop ジョブ送信メカニズムによって HDInsight クラスターに送信できます。 詳細については、次を参照してください。 [プログラムによる Hadoop の送信ジョブ](hdinsight-submit-hadoop-jobs-programmatically.md)します。
    >[AZURE.NOTE] HDInsight クラスターへの JAR ファイルの配置に関する問題がある場合は、HDInsight クラスターでの JAR ファイルを呼び出すことにお問い合わせください [Microsoft サポート](http://azure.microsoft.com/support/options/)します。

  > Cascading は HDInsight ではサポートされておらず、Microsoft サポートの対象でもありません。 サポートされているコンポーネントの一覧は、を参照してください [HDInsight で提供されるクラスター バージョンの新機能ですか?](hdinsight-component-versioning.md)。.

リモート デスクトップ接続を使用したクラスターへのカスタム ソフトウェアのインストールはサポートされていません。 クラスターの再作成が必要な場合にファイルが失われるため、ヘッド ノードのドライブにはファイルを保存しないでください。 Azure BLOB ストレージにファイルを保存することをお勧めします。 BLOB ストレージは永続的です。

## クラスターの一覧と表示

1. サインイン [https://portal.azure.com](https://portal.azure.com)します。
2. 左側のメニューの **[HDInsight クラスター]** をクリックします。

    ![Azure ポータル ](./media/hdinsight-administer-use-management-portal/azure-portal-hdinsight-button.png)

    左側のメニューに **[HDInsight]** が表示されていない場合は、**[参照]** をクリックします。

    ![Azure ポータル ](./media/hdinsight-administer-use-management-portal/azure-portal-browse-button.png)

    クラスターが存在する場合はクラスターの一覧が表示されます。

    ![Azure ポータル　クラスター一覧](./media/hdinsight-administer-use-management-portal/hdinsight-list-clusters.png)

3. **[フィルター項目]** と "サブスクリプション" を使用して一覧を絞り込みます。
4. 一覧にあるクラスターをダブルクリックして詳細を表示します。

    **メニューと基本情報**:

    ![Azure ポータル HDInsight クラスター 基本情報](./media/hdinsight-administer-use-management-portal/hdinsight-essentials.png)

    - メニューをカスタマイズするには、メニュー上の任意の場所を右クリックし、**[カスタマイズ]** をクリックします。
    - **[設定]** と **[すべての設定]**: クラスター用の **[設定]** ブレードを表示します。このブレードを使用して、クラスターの詳しい構成情報にアクセスできます。
    - **[ダッシュボード]**、**[クラスター ダッシュボード]**、**[URL]: これらはすべてクラスター ダッシュボードにアクセスするために使用します。クラスター ダッシュボードは、Linux ベースのクラスター用の Ambari Web です。
    - **[リモート デスクトップ]**: RDP を使用してクラスターに接続します。
    - **[クラスターのスケーリング]**: このクラスターの worker ノードの数を変更できます。
    - **[削除]**: クラスターを削除します。
    - * * クイック スタート (![クラウドとサンダー ボルトのアイコンのクイック スタートを =](./media/hdinsight-administer-use-portal-linux/quickstart.png)) * *: のに役立つ情報を表示 HDInsight の概要です。
    - * * ユーザー (![ユーザー アイコン](./media/hdinsight-administer-use-portal-linux/users.png)) * *: 他のユーザーの Azure サブスクリプションで _portal management_ このクラスターのアクセス許可を設定することができます。
        > [AZURE.IMPORTANT] これは__Azure プレビュー ポータルでのこのクラスターへのアクセスと権限のみに影響し、どのユーザーが HDInsight クラスターに接続でき、ジョブを送信できるかには影響しません。
    - * * タグ (![タグ アイコン](./media/hdinsight-administer-use-portal-linux/tags.png)) * *: タグでは、クラウド サービスのカスタム分類を定義するキー/値ペアを設定することができます。 たとえば、__プロジェクト__という名前のキーを作成し、特定のプロジェクトに関連付けられているすべてのサービスに共通の値を使用できます。
    - **[ドキュメント]**: Azure HDInsight のドキュメントへのリンクです。
    > [AZURE.IMPORTANT] HDInsight クラスターによって提供されるサービスを管理するには、Ambari Web または Ambari REST API を使用する必要があります。 Ambari の使用に関する詳細については、次を参照してください。 [Ambari を使用した HDInsight の管理のクラスター](hdinsight-hadoop-manage-ambari.md)します。

    **Usage**:

    ![Azure ポータル HDInsight クラスター 使用状況](./media/hdinsight-administer-use-management-portal/hdinsight-portal-cluster-usage.png)

5. **[Settings]** をクリックします。

    ![Azure ポータル HDInsight クラスター 使用状況](./media/hdinsight-administer-use-management-portal/hdinsight.portal.cluster.settings.png)

    - **[プロパティ]**: クラスターのプロパティを表示します。
    - **[クラスター AAD ID]**:
    - **[Azure ストレージ キー]**: 既定のストレージ アカウントとそのキーを表示します。 ストレージ アカウントは、クラスターの作成処理中に構成します。
    - **[クラスター ログイン]**: クラスターの HTTP ユーザー名とパスワードを変更します。
    - **[外部メタストア]**: Hive メタストアと Oozie メタストアを表示します。 メタストアを構成できるのは、クラスターの作成処理中のみです。
    - **[クラスターのスケーリング]**: クラスターの worker ノードの数を増減します。
    - **[リモート デスクトップ]**: リモート デスクトップ (RDP) アクセスを有効および無効にし、RDP ユーザー名を構成します。 RDP ユーザー名は HTTP ユーザー名とは別にする必要があります。
    - **[指名パートナー]**:
    > [AZURE.NOTE] これは使用可能な設定の汎用リストです。すべてのクラスターの種類でこれらの設定がすべて表示されるわけではありません。

6. **[プロパティ]** をクリックします。

    次のプロパティが表示されます。

    - **[ホスト名]**: クラスター名。
    - **[クラスター URL]**。
    - **[状態]**: Aborted、Accepted、ClusterStorageProvisioned、AzureVMConfiguration、HDInsightConfiguration、Operational、Running、Error、Deleting、Deleted、Timedout、DeleteQueued、DeleteTimedout、DeleteError、PatchQueued、CertRolloverQueued、ResizeQueued、ClusterCustomization が表示されます。
    - **[リージョン]**: Azure の場所。 サポートされている Azure の場所の一覧は、次を参照してください。、 **領域** ドロップダウン リスト ボックスで [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。
    - **[データの作成日]**。
    - **[オペレーティング システム]**: **Windows** または **Linux**。
    - **[タイプ]**: Hadoop、HBase、Storm、Spark。
    - **[バージョン]**。 を参照してください [HDInsight バージョン](hdinsight-component-versioning.md)
    - **[サブスクリプション]**: サブスクリプション名。
    - **[サブスクリプション ID]**。
    - **[プライマリ データ ソース]**: 既定の Hadoop ファイル システムとして使用される Azure BLOB ストレージ アカウント。
    - **[worker ノードの価格レベル]**。
    - **[ヘッド ノードの価格レベル]**。

## クラスターの削除

クラスターを削除しても、既定のストレージ アカウントまたはリンクされたストレージ アカウントは削除されません。 同じストレージ アカウントと同じメタストアを使用してクラスターを再作成することができます。

1. サインイン、 [ポータルの ][azure-portal]します。
2. 左メニューから **[すべて参照]** をクリックし、**[HDInsight クラスター]** をクリックし、クラスター名をクリックします。
3. 上部のメニューの **[削除]** をクリックし、指示に従います。

関連項目 [クラスターをシャット ダウンの一時停止](#pauseshut-down-clusters)します。

## クラスターのスケール

クラスターのスケール設定機能を使用すると、Azure HDInsight で実行しているクラスターによって使用される worker ノードの数を、クラスターを再作成することなく、変更できます。
>[AZURE.NOTE] HDInsight バージョン 3.1.3 以降を使用しているクラスターのみがサポートされます。 クラスターのバージョンがわからない場合、[プロパティ] ページを確認できます。 参照してください [] ボックスの一覧と表示クラスター](hdinsight-adminster-use-management-portal/#list-and-show-clusters)します。

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

1. サインイン、 [ポータルの ][azure-portal]します。
2. 左メニューから **[すべて参照]** をクリックし、**[HDInsight クラスター]** をクリックし、クラスター名をクリックします。
3. 上メニューから **[設定]** をクリックし、**[クラスターの拡大縮小]** をクリックします。
4. **[worker ノードの数]** を入力します。 クラスター ノードの数に制限は Azure サブスクリプションによって異なります。 制限値を上げるには、課金サポートにお問い合わせください。 コスト情報にはノード数の変更が反映されます。

    ![HDInsight Hadoop HBase Storm および Spark の規模](./media/hdinsight-administer-use-management-portal/hdinsight.portal.scale.cluster.png)

## クラスターの一時停止またはシャットダウン

Hadoop ジョブの大半は、たまにしか実行されないバッチ ジョブです。 ほとんどの Hadoop クラスターでは、クラスターが処理に使用されていない期間がかなりあります。 HDInsight を使用すると、データは Azure Storage に格納されるため、クラスターは、使用されていない場合に安全に削除できます。
また、HDInsight クラスターは、使用していない場合でも課金されます。 クラスターの料金は Storage の料金の何倍にもなるため、クラスターを使用しない場合は削除するのが経済的にも合理的です。

このプロセスをプログラムで実行する方法は数多くあります。

- Azure Data Factory を使用する。 参照してください [Azure HDInsight のリンクされたサービス](../data-factory-compute-linked-services.md/#azure-hdinsight-linked-service) と [変換し、Azure Data Factory を使用して分析](../data-factory-data-transformation-activities.md) 、オンデマンドと独自の HDInsight 用のリンクされたサービスです。
- Azure PowerShell を使用する。 参照してください [フライト遅延データの分析](hdinsight-analyze-flight-delay-data.md)します。
- Azure CLI を使用する。 参照してください [管理 HDInsight クラスターの Azure CLI を使用して](hdinsight-administer-use-command-line.md)します。
- HDInsight .NET SDK を使用する。 参照してください [Hadoop ジョブの送信](hdinsight-submit-hadoop-jobs-programmatically.md)します。

料金情報については、次を参照してください。 [HDInsight 料金](https://azure.microsoft.com/pricing/details/hdinsight/)します。 ポータルからクラスターの削除、を参照してください [クラスター削除](#delete-clusters)。

## クラスターのユーザー名の変更

HDInsight クラスターは、2 つのユーザー アカウントを持つことができます。 HDInsight クラスターのユーザー アカウントは、作成プロセスで作成されます。 RDP を使用してクラスターにアクセスするために、RDP ユーザー アカウントを作成することもできます。 参照してください [リモート デスクトップを有効にする](#connect-to-hdinsight-clusters-by-using-rdp)します。

**HDInsight クラスターのユーザー名とパスワードを変更するには**

1. サインイン、 [ポータルの ][azure-portal]します。
2. 左メニューから **[すべて参照]** をクリックし、**[HDInsight クラスター]** をクリックし、クラスター名をクリックします。
3. 上メニューから **[設定]** をクリックし、**[クラスター ログイン]** をクリックします。
4. **[クラスター ログイン]** が有効になっている場合は、ユーザー名とパスワードを変更する前に、**[無効化]** をクリックしてから **[有効化]** をクリックする必要があります。
4. **[クラスター ログイン名]** と **[クラスター ログイン パスワード]** を変更し、**[保存]** をクリックします。

    ![HDInsight クラスターの HTTP ユーザーのユーザー名とパスワードの変更](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)

## アクセス権の付与/取り消し

HDInsight クラスターには、以下の HTTP Web サービスがあります (これらすべてのサービスには、REST ベースのエンドポイントがあります)。

- ODBC
- JDBC
- Ambari
- Oozie
- Templeton

既定では、これらのサービスへのアクセス許可が付与されます。 Azure ポータルからアクセス許可を取り消す/付与することができます。
>[AZURE.NOTE] アクセス許可を付与するか、取り消すことで、クラスターのユーザー名とパスワードがリセットされます。

**HTTP Web サービスのアクセス許可を付与する/取り消すには**

1. サインイン、 [ポータルの ][azure-portal]します。
2. 左メニューから **[すべて参照]** をクリックし、**[HDInsight クラスター]** をクリックし、クラスター名をクリックします。
3. 上メニューから **[設定]** をクリックし、**[クラスター ログイン]** をクリックします。
4. **[クラスター ログイン]** が有効になっている場合は、ユーザー名とパスワードを変更する前に、**[無効化]** をクリックしてから **[有効化]** をクリックする必要があります。
6. **[クラスター ログイン ユーザー名]** と **[クラスター ログイン パスワード]** に、クラスターの新しいユーザー名とパスワードをそれぞれ入力します。
7. **[保存]** をクリックします。

    ![HDInsight による HTTP Web サービス アクセスの付与と削除](./media/hdinsight-administer-use-management-portal/hdinsight.portal.change.username.password.png)




## 既定のストレージ アカウントの検索

各 HDInsight クラスターには、既定のストレージ アカウントが設定されています。 既定のストレージ アカウントとそのキーの下にあるクラスターと **設定**/**プロパティ**/**Azure ストレージ キー**します。 参照してください [] ボックスの一覧と表示クラスター](#list-and-show-clusters)します。


## リソース グループの検索

ARM モードでは、各 HDInsight クラスターは Azure リソース グループと共に作成されます。 クラスターが属している Azure リソース グループは、次の場所に表示されます。

- クラスター一覧の**リソース グループ**列。
- クラスターの **[基本情報]** タイル。

参照してください [] ボックスの一覧と表示クラスター](#list-and-show-clusters)します。

## HDInsight クエリ コンソールを開く

HDInsight クエリ コンソールには、次の機能が用意されています。

- **Getting started ギャラリー**: ギャラリーを使用するには、次を参照してください。 [Azure HDInsight の Getting Started ギャラリーを使用して学習 Hadoop](hdinsight-learn-hadoop-use-sample-gallery.md)します。
- **[Hive エディター]**: Hive ジョブを送信するための GUI Web インターフェイス。 参照してください [Hive クエリの実行、クエリ コンソールを使用して](hdinsight-hadoop-use-hive-query-console.md)します。

    ![HDInsight ポータル Hive エディター](./media/hdinsight-administer-use-management-portal/hdinsight-hive-editor.png)

- **[ジョブ履歴]**: Hadoop ジョブを監視します。

    ![HDInsight ポータル ジョブ履歴](./media/hdinsight-administer-use-management-portal/hdinsight-job-history.png)

    **[クエリ名]** をクリックすると、ジョブのプロパティ、**ジョブ クエリ**、**ジョブの出力など、ジョブの詳細が表示されます。 また、クエリと出力の両方をワークステーションにダウンロードすることもできます。

- **[ファイル ブラウザー]**: 既定のストレージ アカウントとリンクされたストレージ アカウントを参照します。

    ![HDInsight ポータル ファイル ブラウザー](./media/hdinsight-administer-use-management-portal/hdinsight-file-browser.png)

    スクリーン ショットで、* *<Account>* * の項目が、Azure ストレージ アカウントを示します。アカウント名をクリックすると、ファイルを参照できます。

- **[Hadoop UI]**。

    ![HDInsight ポータル Hadoop UI](./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-ui.png)

    ** Hadoop UI*, 、ファイルを参照し、ログを確認できます。

- **[Yarn UI]**。

    ![HDInsight ポータル Yarn UI](./media/hdinsight-administer-use-management-portal/hdinsight-yarn-ui.png)

## Hive クエリの実行

ポータルから Hive ジョブを実行するには、HDInsight クエリ コンソールで **[Hive エディター]** をクリックします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

## ジョブの監視

ポータルからジョブを監視するには、HDInsight クエリ コンソールで **[ジョブ履歴]** をクリックします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

## Browse files

既定のストレージ アカウントおよびリンクされたストレージ アカウントに格納されているファイルを参照するには、HDInsight クエリ コンソールで **[ファイル ブラウザー]** をクリックします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

また、HDInsight コンソールの **[Hadoop UI]** から **[ファイル システムの参照]** ユーティリティを使用することもできます。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。



## クラスターの使用状況の監視

HDInsight クラスター ブレードの __[使用状況]__ セクションには、サブスクリプションで HDInsight 用に使用できるコアの数、このクラスターに割り当てられているコアの数、およびこのクラスター内のノードへのコアの割り当て方法に関する情報が表示されます。 参照してください [] ボックスの一覧と表示クラスター](#list-and-show-clusters)します。
> [AZURE.IMPORTANT] HDInsight クラスターによって提供されるサービスを監視するには、Ambari Web または Ambari REST API を使用する必要があります。 Ambari の使用に関する詳細については、を参照してください [Ambari を使用した HDInsight の管理のクラスター](hdinsight-hadoop-manage-ambari.md)。


## Hadoop UI を開く

クラスターの監視、ファイル システムの参照、ログの確認を行うには、HDInsight クエリ コンソールで **[Hadoop UI]** をクリックします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

## Yarn UI を開く

Yarn のユーザー インターフェイスを使用するには、HDInsight クエリ コンソールで **[Yarn UI]** をクリックします。 参照してください [Open HDInsight クエリ コンソール](#open-hdinsight-query-console)します。

## RDP を使用したクラスターへの接続

クラスターの作成時に指定したクラスターの資格情報を使用するとクラスター上のサービスにアクセスできますが、リモート デスクトップを介してクラスター自体にアクセスすることはできません。 クラスターにプロビジョニングするとき、あるいはクラスターにプロビジョニングした後、リモート デスクトップ アクセスをオンにできます。 作成時にリモート デスクトップを有効にする方法については、「 [作成 HDInsight クラスター](hdinsight-provision-clusters.md)します。

**リモート デスクトップを有効にするには**

1. サインイン、 [ポータルの ][azure-portal]します。
2. 左メニューから **[すべて参照]** をクリックし、**[HDInsight クラスター]** をクリックし、クラスター名をクリックします。
3. 上メニューから **[設定]** をクリックし、**[リモート デスクトップ]** をクリックします。
4. **[有効期限]**、**[リモート デスクトップのユーザー名]**、**[リモート デスクトップのパスワード]** に入力し、**[有効化]** をクリックします。

    ![HDInsight によるリモート デスクトップ構成の有効化と無効化](./media/hdinsight-administer-use-management-portal/hdinsight.portal.remote.desktop.png)

    [有効期限] の既定値は 1 週間です。
> [AZURE.NOTE] HDInsight .NET SDK を使用して、クラスターに対するリモート デスクトップを有効にすることもできます。 HDInsight クライアント オブジェクトで、**EnableRdp** メソッドを次の形式で使用します: **client.EnableRdp(clustername, location, "rdpuser", "rdppassword", DateTime.Now.AddDays(6))**。 同様に、クラスターに対するリモート デスクトップを無効にするには、**client.DisableRdp(clustername, location)** を使用します。 これらの方法の詳細については、次を参照してください。 [HDInsight .NET SDK のリファレンス](http://go.microsoft.com/fwlink/?LinkId=529017)します。 この方法は、Windows で実行されている HDInsight クラスターにのみ適用できます。

**RDP を使用してクラスターに接続するには**

1. サインイン、 [ポータルの ][azure-portal]します。
2. 左メニューから **[すべて参照]** をクリックし、**[HDInsight クラスター]** をクリックし、クラスター名をクリックします。
3. 上メニューから **[設定]** をクリックし、**[リモート デスクトップ]** をクリックします。
4. **[接続]** クリックし、指示に従います。 接続が無効の場合は、最初に有効する必要があります。 必ずリモート デスクトップ ユーザーのユーザー名とパスワードを使用してください。 クラスターのユーザー資格情報は使用できません。


## Hadoop コマンド ラインを開く

リモート デスクトップを使用してクラスターに接続して Hadoop コマンド ラインを使用するには、まず、先のセクションで説明したように、クラスターに対するリモート デスクトップ アクセスを有効にする必要があります。

**Hadoop コマンド ラインを開くには**

1. リモート デスクトップを使用してクラスターに接続します。
8. デスクトップで、**[Hadoop コマンド ライン]** をダブルクリックします。

    ![HDI.HadoopCommandLine][image-hadoopcommandline]

    Hadoop コマンドの詳細については、次を参照してください。 [Hadoop コマンド リファレンス](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CommandsManual.html)します。

先のスクリーンショットで、フォルダー名には Hadoop のバージョン番号が埋め込まれています。 バージョン番号は、クラスターにインストールされている Hadoop コンポーネントのバージョンに基づいて変更できます。 Hadoop 環境変数を使用して、これらのフォルダーを参照できます。 次に例を示します。

    cd %hadoop_home%
    cd %hive_home%
    cd %hbase_home%
    cd %pig_home%
    cd %sqoop_home%
    cd %hcatalog_home%

## 次のステップ

この記事では、ポータルを使用して HDInsight クラスターを作成する方法、および Hadoop コマンド ライン ツールを開く方法について説明しました。 詳細については、次の記事を参照してください。

* [Azure PowerShell を使用した HDInsight を管理します。](hdinsight-administer-use-powershell.md)
* [Azure CLI を使用した HDInsight を管理します。](hdinsight-administer-use-command-line.md)
* [HDInsight クラスターを作成します。](hdinsight-provision-clusters.md)
* [プログラムによる Hadoop ジョブを送信します。](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Azure HDInsight を概要します。](../hdinsight-get-started.md)
* [Azure HDInsight で Hadoop のバージョンとは](hdinsight-component-versioning.md)


[azure-portal]: https://portal.azure.com 
[image-hadoopcommandline]: ./media/hdinsight-administer-use-management-portal/hdinsight-hadoop-command-line.png "Hadoop command line"

