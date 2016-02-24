<properties
    pageTitle="HDInsight Tools for Visual Studio を使用する方法 | Microsoft Azure"
    description="Visual Studio Hadoop Tools for HDInsight をインストールして、Hadoop クラスターに接続し、Hive クエリを実行する方法について説明します。"
    keywords="hadoop tools,hive query,visual studio"
    services="HDInsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-data"
    ms.date="11/30/2015"
    ms.author="jgao"/>

# HDInsight Tools for Visual Studio を使用して Hive クエリを実行する

HDInsight Tools for Visual Studio を使用して HDInsight クラスターに接続し、Hive クエリを送信する方法について説明します。 HDInsight の使用方法の詳細については、[HDInsight][hdinsight.introduction の概要] を参照してください [始めるや HDInsight][hdinsight.get.started] です。 Storm クラスターへの接続に関する詳細については、[開発の c# トポロジ Visual Studio][hdinsight.storm.visual.studio.tools を使用した HDInsight での Apache Storm] を参照してください。

**前提条件**

このチュートリアルを完了して、Visual Studio で Hadoop ツールを使用するには、次が必要になります。

- Azure HDInsight クラスター: Linux ベースまたは Windows ベースのクラスターは、このドキュメントの手順で動作します。 クラスター作成の詳細については、次のいずれかをご覧ください。

    - [Hadoop チュートリアル: Linux 上の HDInsight で Hive と Hadoop を使用する](hdinsight-hadoop-linux-tutorial-get-started.md)
    - [Hadoop のチュートリアル: Windows 上の HDInsight で Hadoop と Hive クエリを使用する](hdinsight-hadoop-tutorial-get-started-windows.md)

- 次のソフトウェアを搭載したワークステーション

    - Windows 8.1、Windows 8、Windows 7
    - 下記のいずれかのバージョンの Visual Studio
        - Visual Studio 2013 Community/Professional/Premium/Ultimate の[アップデート 4](https://www.microsoft.com/download/details.aspx?id=44921)
        - Visual Studio 2015 (Community/Enterprise)

    >[AZURE.NOTE] 現時点では、HDInsight Tools for Visual Studio は、英語版にのみ付属します。


## HDInsight Tools for Visual Studio をインストールする

HDInsight Tools for Visual Studio と Microsoft Hive ODBC ドライバーは、Microsoft Azure SDK for .NET バージョン 2.5.1 以降に付属しています。 使用してインストールすることができます、 [Web Platform Installer](http://go.microsoft.com/fwlink/?LinkId=255386)します。 お使いの Visual Studio バージョンに対応するものを選択する必要があります。 Visual Studio がインストールされていない場合は、最新の Visual Studio Community と Azure SDK を使用してをインストールすることができます、 [Web Platform Installer](http://go.microsoft.com/fwlink/?LinkId=255386) または、次のリンクを使用します。

- [Visual Studio Community 2015 と Microsoft Azure SDK](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VS2015CommunityAzurePack.appids) 
- [Visual Studio Community 2013 と Microsoft Azure SDK](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VS2013CommunityAzurePack.appids) 
- [Microsoft Azure SDK for .NET (VS 2015)](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VWDOrVs2015AzurePack.appids) 
- [Microsoft Azure SDK for .NET (VS 2013)](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/VWDOrVs2013AzurePack.appids) 

![Hadoop ツール: HDinsight Tools for Visual Studio Web Platform installer]。[1]

## Azure サブスクリプションに接続する
HDInsight Tools for Visual Studio を使用して、HDInsight クラスターへの接続、いくつかの基本的な管理操作の実行、Hive クエリの実行が可能です。

>[AZURE.NOTE] HDInsight Emulator に接続する方法については、次を参照してください。 [HDInsight Emulator の概要](../hdinsight-get-started-emulator.md/#vstools)します。

>[AZURE.NOTE] 汎用の Hadoop クラスター (プレビュー) に接続する方法については、次を参照してください。 [Visual Studio を使用して Hive クエリを送信](http://blogs.msdn.com/b/xiaoyong/archive/2015/05/04/how-to-write-and-submit-hive-queries-using-visual-studio.aspx)します。


**Azure サブスクリプションに接続するには**

1.  Visual Studio を開きます。
2.   **ビュー** ] メニューのをクリックして **サーバー エクスプ ローラー** サーバー エクスプ ローラー] ウィンドウを開きます。
3.  展開 **Azure**, 、順に展開 **HDInsight**します。

    >[AZURE.NOTE]通知、 **HDInsight タスク一覧** ウィンドウが開いている必要があります。 表示されない場合はクリックして **その他のウィンドウ** から、 **ビュー** ] メニューの [クリックして **HDInsight タスク一覧ウィンドウ**です。  
4.  Azure サブスクリプションの資格情報を入力し、クリックして **サイン イン**します。 この操作は、このワークステーションで、まだ一度も Visual Studio から Azure サブスクリプションに接続していない場合にのみ必要です。
5.  サーバー エクスプローラーに、既存の HDInsight クラスターの一覧が表示されます。 クラスターが 1 つもない場合は、Azure ポータル、Azure PowerShell、または HDInsight SDK を使用してプロビジョニングできます。 詳細については、[HDInsight クラスターのプロビジョニング] を参照してください。 [hdinsight プロビジョニング] です。

    ![Hadoop ツール: HDInsight Tools for Visual Studio サーバー エクスプ ローラー クラスターの一覧][5]
6.  HDInsight クラスターを展開します。 わかります **Hive データベース**, 、既定のストレージ アカウント、リンクされたストレージ アカウント、および **Hadoop サービス ログ**します。 さらに、エンティティを展開できます。

Azure サブスクリプションに接続した後で、次を実行できます。

**Visual Studio から Azure ポータルに接続するには**

- サーバー エクスプ ローラーから、展開 **Azure** > **HDInsight**, 、HDInsight クラスターを右クリックし、[クリックして **Manage Cluster in Azure Portal**します。

**Visual Studio から質問をしたりフィードバックを提供したりするには**

-  **ツール** ] メニューのをクリックして **HDInsight**, 、] をクリックし、 **MSDN フォーラム** を質問したり、クリックしてしたり **フィードバックの送信**します。

## リンクしているリソースへの移動

サーバー エクスプローラーで、既定のストレージ アカウント、すべてのリンクされたストレージ アカウントを確認できます。 既定のストレージ アカウントを展開すると、そのストレージ アカウントのコンテナーを表示できます。 既定のストレージ アカウントと既定のコンテナーがマークされます。 コンテナーのいずれかの右クリックしてコンテンツを表示できます。

![HDInsight Tools for Visual Studio サーバー エクスプ ローラー クラスターの一覧][2]

## Hive クエリを実行する
[Apache Hive][apache.hive] データ ウェアハウス インフラストラクチャで Hadoop に構築されるデータの概要、クエリ、および分析を提供することができます。 HDInsight Tools for Visual Studio は Visual Studio からの Hive クエリの実行をサポートします。 Hive の詳細については、[Hive と使用 HDInsight][hdinsight.hive] を参照してください。

HDInsight クラスターに対して Hive スクリプトをテストするには、 数分以上かかる場合があります。 HDInsight Tools for Visual Studio では、ライブ クラスターに接続しなくても、Hive スクリプトをローカルで検証できます。

加えて、HDInsight Tools for Visual Studio では、特定の Hive ジョブの YARN ログを収集して表示することで、Hive ジョブの内容を見ることができます。

### ビュー、 **hivesampletable**
すべての HDInsight クラスターは、という Hive テーブルのサンプルに付属 *hivesampletable*します。 このテーブルを使用して Hive テーブルの一覧表示、テーブル スキーマの表示、Hive テーブル内の行の一覧表示を行う方法を説明します。



**Hive テーブルを一覧表示し、Hive テーブル スキーマを表示するには**

1.   **サーバー エクスプ ローラー**, 、展開 **Azure** > **HDInsight** >、任意のクラスター > **Hive データベース** > **既定** > **hivesampletable** テーブル スキーマを表示します。
4.  右クリック **hivesampletable**, 、クリックして **ビュー上位 100 行** 、行を一覧します。 これは、Hive ODBC ドライバーを使用して、次の Hive クエリを実行することと同等です。

        SELECT * FROM hivesampletable LIMIT 100

    行カウントをカスタマイズできます。

    ![Hadoop ツール: HDinsight Hive Visual Studio スキーマ クエリ][6]

### Hive テーブルの作成

GUI を使用して Hive テーブルを作成するか、Hive クエリを使用できます。 Hive クエリの使用方法の詳細については、次を参照してください。 [Hive クエリの実行](#run.queries)します。

**Hive テーブルを作成するには**

1.  **サーバー エクスプ ローラー**, 、展開 **Azure** > **HDInsight クラスター** HDInsight クラスター > **Hive データベース**, を右クリックし、 **既定**, 、] をクリック **Create Table**します。
2. テーブルを構成します。
3. クリックして **Create Table** 、新しい Hive テーブルを作成するジョブを送信します。

    ![Hadoop ツール: hdinsight visual studio のツールは、hive テーブルを作成する][7]

### <a name="run.queries"></a>Hive クエリの検証と実行
Hive クエリを作成して実行するには次の 2 つの方法があります。

- アドホック クエリを作成する
- Hive アプリケーションを作成する

**アドホック クエリを作成、検証、実行するには**

1.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、順に展開 **HDInsight クラスター**します。
2. クリックして、クエリを実行するクラスターを右クリックして **Write a Hive Query**します。
3. Hive クエリを入力します。 Hive エディターは Intellisense をサポートしています。 HDInsight Tools for Visual Studio で、Hive スクリプトの編集時にリモート メタデータの読み込みがサポートされます。 たとえば、"SELECT * FROM" と入力すると、Intellisense には推奨されるテーブル名が一覧表示されます。 テーブル名を指定すると、列名が Intellisense に一覧表示されます。 ツールは、ほとんどすべての Hive の DML ステートメント、サブクエリ、および組み込みの UDF をサポートします。

    ![Hadoop ツール: HDInsight Visual Studio Tools IntelliSense][13]

    ![Hadoop ツール: HDInsight Visual Studio Tools IntelliSense][14]

    > [AZURE.NOTE] クラスターの HDInsight のツールバーで選択されているメタデータのみが提案されます。
4. (省略可能):] をクリックして **検証スクリプト** スクリプトの構文エラーを確認します。

    ![Hadoop ツール: hdinsight tools for Visual Studio ローカル検証][10]

4. クリックして **送信** または **(高度な) 送信**します。 高度な送信オプションを使用して構成する **ジョブ名**, 、**引数**, 、**追加の構成**, 、および **ステータス ディレクトリ** スクリプト。

    ![hdinsight の hadoop hive query][9]

    表示、ジョブを送信した後、 **Hive ジョブの概要** ウィンドウです。

    ![HDInsight の Hadoop Hive クエリの概要][8]
5. 使用して、 **更新** にジョブの状態が変更されるまで状態を更新するボタン **完了**します。
6. 下部には、以下を参照してリンクをクリックして: **ジョブ クエリ**, 、**ジョブの出力**, 、**ジョブ ログ**, 、または **Yarn ログ**します。



**Hive ソリューションを作成し、実行するには**

1.  **ファイル** ] メニューのをクリックして **新規**, 、クリックして **プロジェクト**します。
2. 選択 **HDInsight** 左側のウィンドウから次のように選択します。 **Hive アプリケーション** 中央のペインで、プロパティを入力し、クリック **OK**します。

    ![Hadoop ツール: hdinsight visual studio ツール新しい hive プロジェクト][11]
3.  **ソリューション エクスプ ローラー**, をダブルクリックして **Script.hql** を開きます。
4. Hive スクリプトを検証するには、クリックすることができます、 **検証スクリプト** ] ボタンをクリックしてまたは Hive エディターでスクリプトを右クリックし、クリックして **検証スクリプト** 、コンテキスト メニューからです。


### Hive ジョブの表示
Hive ジョブのジョブ クエリ、ジョブ出力、ジョブのログ、Yarn ログを表示できます。 詳細については、先のスクリーンショットをご覧ください。

ツールの最新のリリースでは、提示の YARN ログを収集して、Hive ジョブの内容を確認できます。 YARN ログは、パフォーマンス問題の検証に役立ちます。 HDInsight での YARN ログを収集する方法の詳細については、[アクセス HDInsight アプリケーション ログ Programmatically][hdinsight.access.application.logs] を参照してください。

**Hive ジョブの表示**

1.  **サーバー エクスプ ローラー**, 、展開 **Azure**, 、順に展開 **HDInsight**します。
2. HDInsight クラスターを右クリックし、をクリックし、 **ジョブの表示**します。 クラスター上で実行した Hive ジョブの一覧が表示されます。
3. オンにし、使用するジョブの一覧でジョブをクリックして、 **Hive ジョブの概要** ウィンドウを開きます **ジョブ クエリ**, 、**ジョブの出力**, 、**ジョブ ログ**, 、または **Yarn ログ**します。

    ![Hadoop ツール: HDInsight Visual Studio ツール Hive ジョブの表示][12]

### HiveServer2 による Hive 実行の高速化

>[AZURE.NOTE] この機能は、HDInsight クラスター version 3.2 以降でのみ機能します。

HDInsight Tools は以前、WebHCat (Templeton とも呼ばれます) を介して Hive ジョブを送信していました。 そのためジョブの詳細やエラー情報を返すのに長い時間がかかっていました。
このようなパフォーマンスの問題を解決するために、HDInsight Tools では、HiveServer2 を通して直接クラスターで Hive ジョブを実行することにより RDP/SSH をバイパスすることができるようになりました。 
パフォーマンスが向上するだけでなく、ユーザーは、Hive on Tez のグラフやタスクの詳細を表示することもできます。

HDInsight クラスター version 3.2 以降は、表示、 **HiveServer2 経由での Execute** ] ボタンをクリックします。

![hdinsight visual studio tools execute via hiveserver2](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.execute.via.hiveserver2.png)

ログがリアルタイムでストリームバックしていることを確認できます。また、Hive クエリが Tez で実行されている場合はジョブ グラフも確認できます。
 
![hdinsight visual studio tools fast path hive execution](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.fast.path.hive.execution.png)

**HiveServer2 を使用したクエリの実行と WebHCat を使用したクエリの送信の違い**

HiveServer2 を使用してクエリを実行した場合、パフォーマンス上の利点は多数ありますが、いくつかの制限もあります。 制限の一部は、運用環境での使用に適していません。 その違いを次の表に示します。

| |HiveServer2 を使用して実行した場合 |WebHCat を使用して送信した場合|
|---|---|---|
|クエリの実行|WebHCat のオーバーヘッドが解消されます (これにより、"TempletonControllerob" という名前の MapReduce ジョブが開始されます)。|クエリが WebHCat を使用して実行される間、WebHCat によって MapReduce ジョブが開始されるため、待機時間がさらに長くなります。|
|ログの再ストリーミング|ほぼリアルタイムで行われます。|ジョブの実行ログを利用できるのは、ジョブが完了したときのみです。|
|ジョブ履歴の表示|HiveServer2 を使用してクエリを実行する場合、そのジョブ履歴 (ジョブのログ、ジョブの出力) は保持されません。 アプリケーションは YARN UI で表示できますが、情報は限定的です。|WebHCat を使用してクエリを実行する場合、そのジョブ履歴 (ジョブのログ、ジョブの出力) は保持され、Visual Studio、HDInsight SDK、PowerShell を使用して表示できます。 |
|ウィンドウを閉じる|  HiveServer2 を使用した実行は "同期的な" 方法であるため、ウィンドウを開いたままにしておく必要があります。ウィンドウを閉じると、クエリの実行は取り消されます。|WebHCat を使用した送信は "非同期的な" 方法であるため、WebHCat を使用してクエリを送信し、Visual Studio を終了することができます。 結果は、いつでも戻って確認できます。|


### Tez Hive ジョブのパフォーマンス グラフ

HDInsight Tools for Visual Studio は Tez 実行エンジンで実行された Hive ジョブのパフォーマンス グラフの表示をサポートしています。 Tez を有効にする方法については、次を参照してください。 [HDInsight][hdinsight.hive での Hive の使用]。 Visual Studio で Hive ジョブを送信した後、ジョブが完了すると、Visual Studio にグラフが表示されます。  をクリックする必要があります、 **更新** を最新のジョブの状態を取得する] ボタンをクリックします。

> [AZURE.NOTE] この機能は、HDInsight クラスター バージョン 3.2.4.593 についてでのみ使用、完了したジョブに対してのみ機能することができます。 これは、Windows ベースと Linux ベースの両方のクラスターで機能します。

![hadoop hive tez パフォーマンス グラフ](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.hive.tez.performance.graph.png)

Hive クエリを理解しやすくするために、今回のリリースでツールに Hive 演算子表示機能が追加されました。 ジョブ グラフの頂点をダブルクリックするだけで、頂点内のすべての演算子を表示できます。 特定の演算子の上にマウス ポインターを置きことで、その演算子の詳細を表示することもできます。

### Hive on Tez ジョブのタスク実行ビュー

Hive on Tez ジョブのタスク実行ビューは、Hive ジョブの構造化および視覚化された情報の取得と、ジョブの詳細の取得に使用できます。 ディレクトリ階層内に 
パフォーマンスの問題がある、さらに詳細を取得するビューを使用することができます。 各タスクの動作と各タスクに関する詳細情報など 
(データの読み取り/書き込み、スケジュール/開始/終了時刻など) ジョブの構成または視覚化された情報に基づいてシステム アーキテクチャが調整できるようにします。

![hdinsight visual studio tools task execution view](./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.task.execution.view.png)

## Pig のスクリプトを実行する

HDInsight Tools for Visual Studio は、Pig スクリプトの作成と、HDInsight クラスターへの送信をサポートしています。 ユーザーは、テンプレートから Pig プロジェクトを作成して、HDInsight クラスターにスクリプトを送信できます。

## 次のステップ
この記事では、Hadoop ツール パッケージを使用して Visual Studio から HDInsight クラスターに接続し、Hive クエリを実行する方法を説明しました。 詳細については、次を参照してください。

- [HDInsight での Hadoop Hive の使用][hdinsight.hive]
- [HDInsight の Hadoop の使用を開始][hdinsight.get.started]
- [HDInsight での Hadoop ジョブの送信][hdinsight.submit.jobs]
- [HDInsight の Hadoop を使用した Twitter データを分析する][hdinsight.analyze.twitter.data]


<!--Anchors-->
[Installation]: #installation
[Connect to your Azure subscription]: #connect-to-your-azure-subscription
[Navigate the linked resources]: #navigate-the-linked-resources
[Run Hive queries]: #run-hive-queries
[Next steps]: #next-steps

<!--Image references-->
[1]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.wpi.png
[2]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.linked.resources.png
[5]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.server.explorer.png
[6]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.hive.schema.png
[7]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.create.hive.table.png
[8]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.run.hive.job.summary.png
[9]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.submit.jobs.advanced.png
[10]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.validate.hive.script.png
[11]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.new.hive.project.png
[12]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.view.hive.jobs.png
[13]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.table.names.png
[14]: ./media/hdinsight-hadoop-visual-studio-tools-get-started/hdinsight.visual.studio.tools.intellisense.column.names.png


<!--Link references-->
[hdinsight-provision]: ../hdinsight/hdinsight-provision-clusters.md
[hdinsight.introduction]: ../hdinsight-introduction.md
[hdinsight.get.started]: ../hdinsight-get-started.md
[hdinsight.hive]: ../hdinsight/hdinsight-use-hive.md
[hdinsight.submit.jobs]: ../hdinsight/hdinsight-submit-hadoop-jobs-programmatically.md
[hdinsight.analyze.twitter.data]: ../hdinsight/hdinsight-analyze-twitter-data.md
[hdinsight.storm.visual.studio.tools]: ../hdinsight/hdinsight-storm-develop-csharp-visual-studio-topology.md
[hdinsight.access.application.logs]: ../hdinsight/hdinsight-hadoop-access-yarn-app-logs.md

[apache.hive]: http://hive.apache.org

