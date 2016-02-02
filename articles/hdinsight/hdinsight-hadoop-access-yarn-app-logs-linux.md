<properties
    pageTitle="Linux ベースの HDInsight での Hadoop YARN アプリケーション ログへのアクセス | Microsoft Azure"
    description="コマンドラインと Web ブラウザーの両方を使用して、Linux ベース HDInsight (Hadoop) クラスターで YARN アプリケーション ログにアクセスする方法について説明します。"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="Blackmist" 
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/04/2015"
    ms.author="larryfr"/>


# Linux ベースの HDInsight での YARN アプリケーション ログへのアクセス

このドキュメントでは、Azure HDInsight の Hadoop クラスターで完了した YARN (Yet Another Resource Negotiator) アプリケーションのログにアクセスする方法について説明します。
> [AZURE.NOTE] このドキュメントの情報は、Linux ベースの HDInsight クラスターに固有のものです。 Windows ベースのクラスターについては、を参照してください [アクセス YARN アプリケーション ログに Windows ベースの HDInsight](hdinsight-hadoop-access-yarn-app-logs.md)。

## 前提条件

* Linux ベースの HDInsight クラスター。

* 必要があります [SSH トンネルを作成する](hdinsight-linux-ambari-ssh-tunnel.md) ResourceManager ログ web UI にアクセスできるようにします。

## <a name="YARNTimelineServer"></a>YARN タイムライン サーバー

[YARN Timeline Server](http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html) 2 つの異なるインターフェイスを通じてもフレームワーク固有のアプリケーションの情報を完了したアプリケーションに関する汎用的な情報を提供します。 具体的には次の処理が行われます。

* HDInsight クラスター上での汎用アプリケーション情報の格納と取得はバージョン 3.1.1.374 以降で有効になります。
* タイムライン サーバーのフレームワーク固有のアプリケーション情報コンポーネントは、現在 HDInsight クラスターで使用できません。

アプリケーションの汎用情報には次のようなデータが含まれています。

* アプリケーション ID (アプリケーションの一意の識別子)
* アプリケーションを開始したユーザー
* アプリケーションを完了するために実行された試みに関する情報
* 特定のアプリケーションの試行で使用されたコンテナー

## <a name="YARNAppsAndLogs"></a>YARN アプリケーションとログ

YARN はアプリケーションのスケジュール設定/監視からリソース管理を切り離すことで、複数のプログラミング モデル (MapReduce はそのうちの 1 つ) をサポートします。 これは、グローバルな *リソース マネージャー* (RM)、ワーカー ノードごとの*ノード マネージャー* (NM)、アプリケーションごとの*アプリケーション マスター* (AM) によって実現されます。 アプリケーションごとの AM は、アプリケーションを実行するためのリソース (CPU、メモリ、ディスク、ネットワーク) を RM と調整します。 RM は NM と連携して、これらのリソースに*コンテナー*としての許可を付与します。 AM は、RM によって自身に割り当てられたコンテナーの進行状況を追跡します。 アプリケーションはその性質によって、多くのコンテナーを必要とする場合があります。

さらに、クラッシュが発生した場合または AM と RM の間で通信が失われた場合、アプリケーションを完了するために、各アプリケーションで複数の*アプリケーション試行*が行われる場合があります。 そのため、アプリケーションの特定の試行にコンテナーが付与されます。 コンテナーは YARN アプリケーションによって実行される作業の基本単位のコンテキストを提供します。そのコンテキストの中で行われるすべての作業は、コンテナーが割り当てられた 1 つのワーカー ノードで実行されます。 参照してください [YARN の概念 ][yarn-concepts] を参照します。

アプリケーションのログ (および関連するコンテナーのログ) は、問題のある Hadoop アプリケーションのデバッグに重要です。 YARN は、収集、集計、およびアプリケーションのログを格納するための便利なフレームワークを提供、 [ログの集計ログの集計][log-aggregation] 機能します。 ログの集計機能により、アプリケーションのログへのアクセスはより確実になります。この機能は、ワーカー ノード上のすべてのコンテナーでログを集計し、アプリケーションが終了した後でワーカー ノードごとに 1 つの集計されたログ ファイルとして既定のファイル システムに保存します。 アプリケーションは数百または数千のコンテナーを使用することがありますが、1 つのワーカー ノードで実行されるすべてのコンテナーのログは常に 1 つのファイルに集計されます。つまりアプリケーションで使用するワーカー ノードごとに 1 つのログ ファイルが生成されます。 ログの集計は、HDInsight クラスターでは既定で有効になっており (バージョン 3.0 以上)、集計されたログは、クラスターの既定のコンテナーの次の場所にあります。

    wasb:///app-logs/<user>/logs/<applicationId>

ここで、*user* はアプリケーションを開始したユーザーの名前であり、*applicationId* は YARN リソース マネージャーにより割り当てられたアプリケーションの一意の識別子です。

集計されたログは書かれているので、直接読み取ることが、 [TFile ][t-file], 、[バイナリ形式 ][binary-format] コンテナーでインデックスが作成されます。 YARN ResourceManager Logs または CLI ツールを使用して、対象のアプリケーションまたはコンテナーのログをプレーン テキストとして表示する必要があります。

## YARN CLI ツール

YARN CLI ツールを使用するには、まず SSH を使用して HDInsight クラスターに接続する必要があります。 HDInsight で SSH を使用する方法については、次のいずれかのドキュメントをご覧ください。

- [Linux、Unix、OS X から HDInsight 上の Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-unix.md)

- [Windows から Linux ベースの Hadoop で SSH を使用します。](hdinsight-hadoop-linux-use-ssh-windows.md)

次のいずれかのコマンドを実行することで、これらのログをプレーン テキストとして表示できます。

    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
    yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>

< Application Id > を指定する必要があります、< ユーザー-ユーザー-開始--アプリケーション >、< containerId > と & ltworker ノード アドレス > これらのコマンドを実行しているときに、情報。

## YARN ResourceManager UI

YARN リソース マネージャー UI は、クラスターのヘッド ノード上で実行し、Ambari web UI からアクセスできます。ただし、操作を行ってください [SSH トンネルを作成する](hdinsight-linux-ambari-ssh-tunnel.md) リソース マネージャー UI にアクセスできるようにします。

SSH トンネルを作成したら、次の手順に従って YARN のログを表示します。

1. Web ブラウザーでは、https://CLUSTERNAME.azurehdinsight.net に移動します。 CLUSTERNAME を、使用する HDInsight クラスターの名前に置き換えます。

2. 左側のサービスの一覧で、__[YARN]__ を選択します。

    ![選択された YARN サービス](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnservice.png)

3. __[クイック リンク]__ ボックスの一覧で、クラスター ヘッドノードのいずれかを選択し、__[ResourceManager Log]__ を選択します。

    ![YARN クイック リンク](./media/hdinsight-hadoop-access-yarn-app-logs-linux/yarnquicklinks.png)

    YARN のログへのリンクの一覧が表示されます。


[yarn-timeline-server]: http://hadoop.apache.org/docs/r2.4.0/hadoop-yarn/hadoop-yarn-site/TimelineServer.html 
[log-aggregation]: http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/ 
[t-file]: https://issues.apache.org/jira/secure/attachment/12396286/TFile%20Specification%2020081217.pdf 
[binary-format]: https://issues.apache.org/jira/browse/HADOOP-3315 
[yarn-concepts]: http://hortonworks.com/blog/apache-hadoop-yarn-concepts-and-applications/ 

